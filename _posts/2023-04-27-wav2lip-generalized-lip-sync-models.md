---
layout: post
title: "Wav2Lip: Generalized Lip Sync Models"
date: 2023-04-27
description: ""
tags: [digital-humans]
categories: blog
---

*Originally published on [Medium](https://medium.com/@jacksaunders909).*

---

Wav2Lip is one of the most popular lip sync models. In this article we explore how it works and analyse its potential for improvment.

---

### Wav2Lip: Generalized Lip Sync Models

If you read my [previous post](https://medium.com/@jacksaunders909/the-many-forms-of-deep-audio-driven-video-dubbing-65dcf1e981e2), you’ll know that I intend to cover a whole host of audio-driven lip sync models. The first of these is, in my opinion, the most commonly used and built upon. That is Wav2Lip. Despite not reaching the visual quality that could convince anyone it is producing real video, the lip sync is excellent and (controversially) [the model is open source](https://github.com/Rudrabha/Wav2Lip), making still the first choice for many despite much more powerful models now existing. What’s more, many of the current state-of-the-art models build upon the general structure of Wav2Lip. Getting an understanding of the general pipeline is important for understanding a lot of the work being done in this space.

Using the [criteria from my last post,](https://medium.com/@jacksaunders909/the-many-forms-of-deep-audio-driven-video-dubbing-65dcf1e981e2) Wav2Lip is:

- **Person Generic:** By using a reference frame and training on a large dataset, Wav2Lip can work on any video and any audio.
- **2D Based:**Wav2Lip operates directly at the image level, this usually causes lower quality outputs. However, doing so makes it easier for the model to be person-generic.
- **Lip Only:**Wav2Lip only replaces the lower half of the face, taking the upper half from the provided video.

---

### Method

![The architecture diagram of Wav2Lip. Separate audio (green) and video (blue) encoders convert their respective input to a latent space, while a decoder (red) is used to generate the videos.](https://cdn-images-1.medium.com/max/800/0*0qABvpxvkDCHbL0y.png)

Given the criteria above, Wav2Lip and other similar models must answer the question of how to reconstruct the lips of *any* person without the advantage of training a model specifically on that person. A person specific model could simply mask out the lower half of the face and learn some mapping to predict that lower half from the audio signal, as it will have seen many examples of that persons lips. However, a person-generic model like Wav2Lip has no idea what the lips, teeth, tongue or any other part of the lower face looks like for this person. So, in addition to the upper half of the face,**Wav2Lip also takes another random reference frame** as input. This allows for some level of information about the lower half of the persons face to enter the network.

### Architecture

To effectively use all of the information available to it, Wav2Lip uses an autoencoder architecture. A video encoder (blue in the network diagram) takes the current frame, with the lower half masked out, and a random reference frame from the same video and encodes it to a latent space. The same is done with the audio encoder (shown in green), which works with [MEL-spectrograms](https://towardsdatascience.com/getting-to-know-the-mel-spectrogram-31bca3e2d9d0). A decoder (red) then takes both of these latent codes and produces the final frame. The generator contains skip connections, similar to those used in a UNET but between the video branch and the decoder only, this allows for the upper half of the face to be copied across more easily.

![The inputs and outputs of the wav2lip generator network, for a single frame.](https://cdn-images-1.medium.com/max/800/1*poYobfYgsuauR0UDgdIUNg.png)

### Losses

The model is trained using a combination of losses. The first is a simple L1 reconstruction loss between the real and generated frames. The other losses however, require a bit more thought. One of the primary contributions of this paper is the use of a lip sync expert network, called a SyncNet, which predicts if audio and video are in or out of sync. This model works over a window of frames, meaning that it cannot be used in a direct way on a per-frame generator such as the one used in Wav2Lip. To overcome this, the authors propose generating **multiple frames at once, each on a per-frame basis and then stacking them together to pass into the expert SyncNet.**They do this by stacking the frames channel-wise.

To get a better understanding of this, let’s consider the shapes of the tensors. For an RGB frame of shape (W, H, 3), a batch size of N, and a window of T frames, the generator would be passed a tensor of shape (N * T, W, H, 6). That is, all N batches of T frames, with the masked and reference images stacked across the channels. This would generate a tensor of (N * T, W, H, 3) which is reshaped first to (N, T, W, H, 3) then the T frames are stacked channel-wise to get (N, W, H, 3 * T) which is passed to the SyncNet. This (N, W, H, 3 * T) tensor is also the input to a discriminator. This allows the discriminator to see consecutive frames, and makes the output more temporally stable. **Note that the even though the discriminator and SyncNet see T frames, the generator is only generating one at a time.**

### SyncNet

The question still remains of how to train the SyncNet. The SyncNet must predict if a given pair of video and audio are in sync, or out of sync. What’s more, this prediction must have some measure of uncertainty, so that it can be used as a loss to improve the lip sync of the generated videos. The authors settle on a form of contrastive learning. Separate video and audio encoders both map to a common latent space and the [cosine similarity](https://en.wikipedia.org/wiki/Cosine_similarity) is taken between them. This essentially tells us how similar the two embeddings are. As it gives values between 0 and 1, it can be treated as a probability. This means that a Binary Cross Entropy Loss may be used. The training process for the SyncNet is then:

- Select a continuous segment of T frames. With equal probability, either select the matching audio window, or another audio window that is from the same video, but at a different point.
- Stack the frames across the channel axis (see above), convert the audio to MEL-spectrograms.
- Encode the video and audio using separate encoders.
- Get the Cosine Similarity. This is then treated as the **probability** that the pair is in sync.
- Use Binary Cross Entropy to get the loss and backpropagate.

---

### Results

When it comes to Wav2Lip, the best way to see the results is to [look for yourself](https://bhaasha.iiit.ac.in/lipsync/uploader). Other than that, you can see an example at the top of this article and in [countless](https://www.youtube.com/watch?v=Klqqb1o3lME) [other](https://www.youtube.com/watch?v=Kwhqj93wyXU) [demos](https://www.youtube.com/watch?v=Ic0TBhfuOrA).

But for those of you that are more numbers-oriented, the paper does contain some metrics to show its efficacy. They show that Wav2Lip beats LipGAN across three datasets (LRS 1, 2 and 3) for visual quality (as measured by FID and user studies), and for lip sync (as measured by their novel metrics LSE-C and LSE-D as well as user studies).

![](https://cdn-images-1.medium.com/max/800/0*1e8_SGEg-a1TZI7V.png)

---

### Limitations

As mentioned before, Wav2Lip is extremely useful as a person-generic model but it is lacking in a few ways. I’ll outline some of the major issues, some of which have spawned great papers attempting to address these weaknesses.

- The model works on a 48x96 pixel region. This is extremely low quality. What’s more, it appears to not scale up well to HQ images, as can be seen by the various Wav2LipHD attempts on GitHub. [A recent paper](https://openaccess.thecvf.com/content/WACV2023/papers/Gupta_Towards_Generating_Ultra-High_Resolution_Talking-Face_Videos_With_Lip_Synchronization_WACV_2023_paper.pdf) tries to address this and we will cover it later.
- The outputs are often poor visual quality, even relative to the number of pixels generated. A popular approach at the moment appears to be using diffusion models to improve the quality. For example [this paper](https://arxiv.org/abs/2301.03786) or [this one](https://mstypulkowski.github.io/diffusedheads/diffused_heads.pdf).
- The identity of the speaker is usually lost to some extent. This is because one reference frame is not enough to capture a persons identity in full. This is an open research direction.
- The lip sync is still not flawless, in particular mouth closure is not always perfect. [This paper](https://arxiv.org/abs/2211.00924) tries to address this using a form of explicit memory.
- [A glance at the issues in the repo shows that the syncnet training does not work for a lot of people](https://github.com/Rudrabha/Wav2Lip/issues?q=syncnet). Even when it does, it is known to take a long time to converge. A better SyncNet loss may solve this, as there has been many advances in contrastive learning in the last few years.

---

### Conclusions

Overall, Wav2Lip opens the way to person-generic lip sync models. Despite its lack of visual quality it is an extremely important paper and serves as an important starting point for a lot of the work in this area. Wav2Lip opens a lot of important research questions, some of which are yet to be solved, yet perhaps its most interesting question raised is should we continue to open-source such models given the extreme popularity and potential harm? Please let me know your thoughts in the comments.

By [Dr Jack Saunders](https://medium.com/@jacksaunders909) on [April 27, 2023](https://medium.com/p/e0effc4e8ed3).

[Canonical link](https://medium.com/@jacksaunders909/wav2lip-generalized-lip-sync-models-e0effc4e8ed3)

Exported from [Medium](https://medium.com) on March 18, 2026.
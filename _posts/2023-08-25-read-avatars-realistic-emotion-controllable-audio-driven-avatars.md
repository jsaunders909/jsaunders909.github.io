---
layout: post
title: "READ Avatars: Realistic Emotion-controllable Audio Driven Avatars"
date: 2023-08-25
description: "A technical summary of READ Avatars — emotion-controllable audio-driven avatar generation using neural rendering and adversarial training. BMVC 2023 Oral."
tags: [digital-humans]
categories: blog
---

*Originally published on [Medium](https://medium.com/@jacksaunders909).*

---

Adding Emotional Control to Audio-Driven Deepfakes

---

### READ Avatars: Realistic Emotion-controllable Audio Driven Avatars

### Adding Emotional Control to Audio-Driven Deepfakes

![READ Avatars takes a reference video and any audio and can produce lip-synced videos in any emotion with fine-grained control over the intensity.](https://cdn-images-1.medium.com/max/800/1*emghz0aL2dSax4TXXM7Row.png)

One of the critical limitations of existing audio-driven deepfakes is the need for more ability to control stylistic attributes. Ideally, we would like to change these aspects, for example, making a generated video happy vs. sad, or to use the speaking style of a particular actor. [READ Avatars](https://readavatars.github.io/) looks to do exactly this, by modifying existing, high-quality, person-specific models to work with direct control over styles.

Having written several blog posts covering deepfake models in the past, this one has special significance to me, **as it is my own.**The paper has just been accepted to this year’s BMVC and it is my first accepted paper! In this article, I will cover the motivation, intuition and methodology behind the work.

### What is Style?

The first place to start when considering stylistic control is to ask exactly what is meant by style. The answer I usually give is a bit of a cop-out: Style is anything in our data that is not considered content. This may seem to merely shift the definition from one word to another, but it does make the task easier. In the context of audio-driven deepfakes, content is the speech itself, the lip movements that match the audio, as well as the face’s appearance.

> That means style is anything that modifies the video while looking like the same person and maintaining lip sync.

In the case of my research, I usually look at two particular forms of style: ***emotional***and ***idiosyncratic.*** Emotional style is simply the emotion expressed on the face, whereas idiosyncratic style refers to the difference in expression between individual people. For example, the way a smile looks on my face compared to yours is an example of idiosyncratic style. These are not the only kinds of styles, but they are among the easiest to demonstrate and work with. **For this work, we used emotion styles only, as we worked on person-specific models.**

### Representing Emotional Style

READ Avatars is not the first paper to look at altering emotional style in audio-driven video generation. However, previous methods have represented emotion as either a one-hot vector or an abstract latent representation (check out [EVP](https://jixinya.github.io/projects/evp/) and [EAMM](https://jixinya.github.io/projects/EAMM/) for respective examples). The former does not have enough precision to allow for fine-grained emotional control and the latter does not have semantic meaning. For this reason, we decided to use a different representation of emotion.

To represent N different emotions, we use an N-dimensional vector where each dimension represents one of the emotions and has a real value between 0 and 1. We let 1 be the maximum possible expression of that emotion. A vector of all zeros is, therefore the absence of emotion (aka neutral).

![A 4-dimension emotion vector could, for example, represent happy, sad, angry and surprised.](https://cdn-images-1.medium.com/max/800/1*LWCC953H8p3uf4nRn2GkFQ.png)

### The Baseline

In order to achieve the highest possible visual quality, we base our model on the 3DMM-based approach of previous work. [I have covered these in a previous article if you’re interested!](https://medium.com/@jacksaunders909/person-specific-deepfakes-with-3d-morphable-models-1df9618b9f6a) In particular, we use the [neural textures approach](https://niessnerlab.org/papers/2019/11neuralrendering/thies2019neural_preprint.pdf), where we train a uv-based, many-channeled texture jointly with an image-to-image UNET.

![The neural textures approach of Theis et al. (Neural Voice Puppetry). Our model is based on this work but with heavy modification.](https://cdn-images-1.medium.com/max/800/0*tBa80f9rnpKFlHMJ.jpg)

As we want to work with emotion, we need to generate the whole face, not just the mouth region. To do this, all we need to do is change the mouth mask (as can be seen in the image above) into a full face mask.

A naive approach may simply be to condition the audio-to-expression network on the emotional code we just defined (check out my [past post](https://medium.com/@jacksaunders909/person-specific-deepfakes-with-3d-morphable-models-1df9618b9f6a) for more details on audio-to-expression networks). This does not work as well as one would hope, however. We suggest two potential reasons for this, the lack of detail in the underlying 3DMM and the over-smoothing effects of regression losses.

### Lack of Detail in 3DMMs

The first of the issues has to do with the inability of 3DMMs to represent the geometry of the face. The problem is twofold. First, the 3DMM struggles to capture the “O” shape of the lips. This can be seen in the figure below. More of an issue though, is the complete lack of any form of representation of the mouth interior, including the teeth and tongue.

![Here we have tried to represent the face of a person with a 3DMM; note how it cannot represent the “O” shape of the mouth adequately.](https://cdn-images-1.medium.com/max/800/1*o7UXDwC2cuN7GgarkBbXFg.png)

This leads to potential ambiguities in the renderings passed to the image-to-image network. For example, without the tongue the sounds “UH” and “L” are expressed in the same way, in this case, how does the network know what to generate inside the mouth?

To overcome this issue, we add audio directly into the video generation process. We do this by conditioning the neural texture on the audio. We use the intermediate layers of Wav2Vec2 as a feature extractor and encode that audio to a latent representation. This is then used to condition a SIREN network, using 2D positional encodings, that outputs a 16-channel neural texture that varies with audio (see below). For more details on the architecture, you can have a look through the [arxiv version of the paper](https://arxiv.org/pdf/2303.00744.pdf).

![The (creepy) neural texture varies based on the input audio.](https://cdn-images-1.medium.com/max/800/1*dyfkxJJtwRbafCdybjXEhQ.gif)

This inclusion allows the image-to-image network to have enough information to resolve such ambiguities.

### Smoothness From Regression Losses

![Example plot showing how recession-based models (red) take an average of all possible valid sequences (blues). This can lead to very smooth motion. GAN models (green) do not do this.](https://cdn-images-1.medium.com/max/800/1*aN7GnfvFLSJzaWe9TcLVjw.png)

Existing audio-to-expression models are trained with regression-based losses, usually L1 or L2. These have a noticeable drawback for facial animation: they create very smooth motion. Where there are two possible sequences valid for a given audio, a regression-based model will select an average of the two leading to the peaks of the motion being averaged out and producing muted motion. This is particularly important for emotional animation generation as the parts of the face not correlated with audio, such as the eyebrows, may move at any time, leading to a lot of smoothing and worse representation of emotion.

GAN-based models alleviate this. A discriminator will learn to label any smooth motion as fake, and therefore, the generator is forced to produce realistic, lifelike motion.

### Results

![A comparison of our method to state-of-the-art](https://cdn-images-1.medium.com/max/800/1*dcpxLSLyZTXrsT-pzlr_zQ.png)

Indeed the modifications we proposed led to an improvement of the results. We have been able to produce superior results to the current state-of-the-art.

![Ablation of the audio-conditioning in the neural texture.](https://cdn-images-1.medium.com/max/800/1*VjPV07zRRdPE_nIfB9S4Mw.png)

### Conclusion and Future Work

READ Avatars has made a few important modifications that allow for the extension of ultra-high quality, 3DMM-based models to include emotional style. The work produces interesting results! With that being said, there are some clear drawbacks. While the lip sync is better than any existing emotional model, it is still a way off-ground truth. We think this could be improved by the addition of an expert discriminator, such as the one used in [wav2lip](https://medium.com/@jacksaunders909/wav2lip-generalized-lip-sync-models-e0effc4e8ed3), and with the use of better audio-to-expression models, such as [Imitator.](https://balamuruganthambiraja.github.io/Imitator/)

In the future, it would be useful to modify more styles, for example, idiosyncratic style. This could be used to make, for example, Joe Biden speak with Donald Trump’s lip movements, which could be interesting! To do this, we would need to build generalized neural texture models, which is an interesting research direction and the current goal for future work.

Overall, this has been a really interesting project to work on, and I’m thrilled to get my first published paper. I’m looking forward to the research that leads on from this work. As always, if you have any questions or feedback, please let me know in the comments!

By [Dr Jack Saunders](https://medium.com/@jacksaunders909) on [August 25, 2023](https://medium.com/p/1351e1fdfee2).

[Canonical link](https://medium.com/@jacksaunders909/read-avatars-realistic-emotion-controllable-audio-driven-avatars-1351e1fdfee2)

Exported from [Medium](https://medium.com) on March 18, 2026.
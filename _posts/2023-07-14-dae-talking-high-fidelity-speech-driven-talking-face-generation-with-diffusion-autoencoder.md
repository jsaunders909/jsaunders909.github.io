---
layout: post
title: "DAE Talking: High Fidelity Speech-Driven Talking Face Generation with Diffusion Autoencoder"
date: 2023-07-14
description: "A summary of DAE-Talking: high-fidelity speech-driven talking face generation using a diffusion autoencoder for expressive digital human animation."
tags: [digital-humans, generative-ai]
categories: blog
---

*Originally published on [Medium](https://medium.com/@jacksaunders909).*

---

Diffusion Models + Lots of Data = Practically Perfect Talking Head Generation

---

### DAE Talking: High Fidelity Speech-Driven Talking Face Generation with Diffusion Autoencoder

### Diffusion Models + Lots of Data = Practically Perfect Talking Head Generation

Today we will discuss a new paper and possibly the highest-quality audio-driven deepfake model I have come across. Coming from Microsoft Research, DAE-talker is a person-specific, full-head model that builds on the Diffusion Auto-Encoder (DAE). While the model is only shown on a single dataset, the results are very impressive.

The key to the success of this paper is twofold. First, they remove dependence on handcrafted features such as landmarks or 3DMM coefficients. Despite the fact that 3DMM’s, in particular, are very useful for person-specific models, they are still restrictive and are not as expressive as they could be. The authors are still able to benefit from the separation of a pose from other attributes, however, by using pose modeling. The second reason for the success of this model is the use of diffusion models. Diffusion models are the driving force behind models like Stable Diffusion, which have brought “Generative AI” into the mainstream.

### The Diffusion Auto-Encoder

Diffusion models are well known for their ability to produce ultra-high-quality images with excellent diversity. These models use a latent vector of noise the same shape as the image and denoise them across multiple steps. However, one of the well-known limitations of diffusion models is that the latent vectors lack semantic meaning. In GANs or VAEs (the usual competitors to diffusion models) it is possible to perform edits in the latent space that leads to predictable changes in the output images. Diffusion models, on the other hand, do not possess this quality. Diffusion auto-encoders overcome this problem by instead using two latent vectors, a semantic code, and a standard, image-sized latent.

![The diffusion auto-encoder. Image from the original paper (Preechakul et. al.)](https://cdn-images-1.medium.com/max/800/0*iyNVlannfA70jkgc.jpg)

DAE is an autoencoding model, meaning that it consists of an encoder and decoder and is trained auto-regressively. The encoder of the DAE encodes an image into a semantic representation of that image. The decoder then takes the semantic latent vector and a noise image and runs the diffusion process to reconstruct the image.

> The upshot is that this allows diffusion-level quality image generation with semantic control

In the case of DAE-Talker, a DAE model is trained on approximately 10 minutes of data of the target actor.

### Controlling the Latent Space

![The latent space is manipulated using speech, this allows the DAE model to produce the final video output (https://daetalker.github.io/)](https://cdn-images-1.medium.com/max/800/0*IMD32puCsO9yTLXs.png)

With the trained DAE permitting control over generated images through the use of semantic latent vectors, it becomes possible to generate video by manipulating the latent vectors only. This is the aim of the speech2latent component of this paper. Given audio as input, it outputs a sequence of latent vectors that are later decoded by the DAE.

An important point to note here is that the random noise image is fixed for every frame in the generated video. This cuts down on random noise that would create temporal inconsistencies in the final video.

![The architecture of the speech2latent model. (https://daetalker.github.io/)](https://cdn-images-1.medium.com/max/800/1*Kw71G45SLIaC0oFXImiI5Q.png)

The speech2latent component consists of several layers. The first of these is a frozen feature extractor from the [Wav2Vec2](https://arxiv.org/abs/2006.11477) model. Wav2Vec2 is a transformer-based model that is used for speech recognition. Taking the feature extractor allows for the extraction of rich latent features of speech; this is done by several papers that look to generate signals from speech, for example, [FaceFormer](https://evelynfan.github.io/audio2face/) or [Imitator](https://balamuruganthambiraja.github.io/Imitator/). This set of features is processed further using convolution and [conformer](https://arxiv.org/abs/2005.08100) blocks (a mix of CNN and transformer layers). After this, a pose adaption layer is applied (we cover this in a second), before a final set of conformer layers and a linear projection onto the DAE latent space.

### Pose Adaptor

The problem of speech-driven animation is a one-to-many problem. This is particularly true in the case of head pose, where the same audio can easily correspond to many different poses. To alleviate this issue, the authors propose the addition of a specific component in the speech2latent network that models pose. A pose predictor predicts pose from speech, while a pose projector adds the pose back into the intermediate features of the network. By adding a pose loss at this stage, the pose is better modeled. As the pose is projected into the features, either predicted poses or ground truth poses can be used.

### Discussion

While this is not the first method in talking face generation to make use of diffusion models, it seems to have found a very successful way of doing so. The results are, in my opinion, the best quality of any existing model. Additionally, the ability to control or generate the pose makes the model particularly flexible.

With that being said, however, the model is not perfect. This method takes person-specificity to the extreme. The model is trained on 12 minutes of data from a single speaker, with no variation in background, lighting or camera. This is an order of magnitude more data than is used by most other methods. Perhaps for this reason, the experiments are restricted to one dataset only. Without seeing experiments on anyone except Obama, it is hard to verify that the model will work for most people. Furthermore, this is not an easy model to train. The DAE component alone trained for three full days on 8 V100 GPUS, with the speech2latent taking more time still. According to [current GCP prices](https://cloud.google.com/compute/gpus-pricing), this could cost upwards of $1500 per training! Inference will likely take a long time, too, as 100 denoising steps are required per frame.

### Conclusion

Overall this is an extremely promising method showing the best results currently available, if you don’t mind the massive costs involved with training. If someone can work out how to develop a person-generic version of this model (and can afford to do so) I think we may be getting close to solving the problem of talking face generation entirely.

By [Dr Jack Saunders](https://medium.com/@jacksaunders909) on [July 14, 2023](https://medium.com/p/ee6e5789b743).

[Canonical link](https://medium.com/@jacksaunders909/dae-talking-high-fidelity-speech-driven-talking-face-generation-with-diffusion-autoencoder-ee6e5789b743)

Exported from [Medium](https://medium.com) on March 18, 2026.
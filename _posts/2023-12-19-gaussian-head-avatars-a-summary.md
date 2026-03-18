---
layout: post
title: "Gaussian Head Avatars: A Summary"
date: 2023-12-19
description: ""
tags: [digital-humans, neural-rendering]
categories: blog
---

*Originally published on [Medium](https://medium.com/@jacksaunders909).*

---

There has been a recent explosion of Gaussian Splatting papers and the avatar space is no exception. How do they work and are they going to…

---

### Gaussian Head Avatars: A Summary

### There has been a recent explosion of Gaussian Splatting papers and the avatar space is no exception. How do they work and are they going to revolutionise the field?

![Gaussian Splatting at an early stage of training. Image by the author.](https://cdn-images-1.medium.com/max/800/1*qCaf_tnHrfmqRB3UJ-pbGQ.png)

And breathe… If you’re interested in the research into digital humans and have any form of social media, you’ve almost certainly been bombarded by dozens of papers applying Gaussian splatting to the field. [As the great Jia-Bin Huang](https://twitter.com/jbhuang0604/status/1714280734016540860) said: 2023 is indeed the year of replacing all NeRFs with Gaussian Splatting. [GaussianAvatars](https://shenhanqian.github.io/gaussian-avatars), [FlashAvatar](https://ustc3dv.github.io/FlashAvatar/), [Relightable Gaussian Codec Avatars](https://shunsukesaito.github.io/rgca/), [MonoGaussianAvatar](https://yufan1012.github.io/MonoGaussianAvatar): these papers represent just a subsection of the papers covering the face alone!

If you’re anything like me, you’re completely overwhelmed by all this incredible work. I’m writing this article to try and compare and contrast these papers, and to try to boil down the key components that underpin all these works. I will, almost certainly, have missed some papers, and by the time I’ve finished this article, I expect there will be more that weren’t around when I started! I’ll start by giving a quick recap of Gaussian Splatting as a method and then cover some of the key papers.

![Teaser figures from a collection of Gaussian Avatar papers. Top Left: MonoGaussianAvatar, Bottom Left: GaussianAvatars, Top Right: Relightable Gaussian Codec Avatars, Middle Right: Gaussian Head, Bottom Right: Gaussian Head Avatars](https://cdn-images-1.medium.com/max/800/1*rlhbOyaBsqkKugwKvKDuVg.png)

---

### Gaussian Splatting — The General Idea

Gaussian Splatting is everywhere now. There’s a good chance you already know the basics, and if you don’t there are a lot of resources out there that do a much better job of explaining them than I can. Here are some examples if you are interested ([1](https://huggingface.co/blog/gaussian-splatting), [2](https://aras-p.info/blog/2023/09/05/Gaussian-Splatting-is-pretty-cool/), [3](https://www.reshot.ai/3d-gaussian-splatting)). Nonetheless, I’ll do my best to give a quick, general overview here.

![A rasterised triangle (left) vs a splatted Gaussian (right). Inspired by this HuggingFace article.](https://cdn-images-1.medium.com/max/800/1*hy_QC0Cc7V5deotkC3ViRg.png)

In a nutshell, Gaussian splatting is a form of rasterisation. It takes some representation of a scene and converts it to an image on the screen. This is similar to the rendering of triangles that form the basis of most graphics engines. Instead of drawing a triangle, Gaussian splatting (unsurprisingly) ‘splats’ Gaussians onto the screen. Each Gaussian is represented by a set of parameters:

- A position in 3D space (in the scene). **μ**
- A per-axis scaling (the skew of the Gaussian). **s**
- A colour (can be RGB or more complex to change based on viewpoint). **c**
- An opacity (the opposite of transparency). **α**

Gaussian splatting itself is not new, it’s been around since the ‘90s at least. What is new is the ability to render them in a ***differentiable*** way. This allows us to fit them to a scene using a set of images. Combine this with a method of creating more Gaussians and deleting useless ons and we get an extremely powerful model for representing 3D scenes.

Why do we want to do this? There are a few reasons:

- The quality speaks for itself. It is visually stunning in a way even NeRFs are not.
- A scene rendered with Gaussian splatting can be run at hundreds of fps, and we haven’t even begun to get deep into the hardware/software optimisation yet.
- They can be easily edited/combined as they are discrete points, unlike neural representations.

---

### Making Gaussians Animatable

Gaussian splatting is obviously cool. It’s no surprise that people have been looking for a way to apply them to faces. [You may have seen Apple’s personas](https://www.theverge.com/2023/6/5/23750096/apple-vision-pro-headset-persona-facetime) which generated a bit of hype. The papers in this article completely blow them out of the water. Imagine fully controllable digital avatars that can run natively on a consumer-grade VR headset, with 6 degrees of freedom camera movement, running at 100+ fps for both eyes. This would make the ‘metaverse’ finally realisable. I would personally bet any amount of money that this scenario will be realised within the next 2–3 years. Most likely, Gaussian Splatting (or some variant) is the tech that is needed to make this work.

Of course, the ability to render a static scene is not enough. We need to be able to do with Gaussians what we are currently able to do with triangular meshes that we use currently. It didn’t take long to get dynamic Gaussians (e.g. [1](https://dynamic3dgaussians.github.io/), [2](https://ingra14m.github.io/Deformable-Gaussians/)). These allow for the capture and replay of “Gaussian Videos”.

![An example of Dynamic Gaussians, best viewed at source: here. Image Credits: JonathonLuiten, MIT License](https://cdn-images-1.medium.com/max/800/1*9lkMP_ZaAX_HyBT-AO7D5A.gif)

Again, really cool but not what we’re after. Ideally, we want a representation that we can control with motion capture, audio, or other signals. Thankfully, we have a huge body of research designed to do exactly this. Enter our old friend the 3DMM. [I’ve covered how these work in a previous post](https://medium.com/@jacksaunders909/person-specific-deepfakes-with-3d-morphable-models-1df9618b9f6a). But at their core, they are a 3D model that is represented with a small set of parameters. These parameters determine the geometry of the face, with facial expressions decoupled from the face shape. This allows for control over the facial expression by just changing a small number of expression parameters. ***Most of the papers that seek to animate using Gaussian Splatting use a 3DMM at their core.***

We will now cover some of the recent Gaussian Head Animation papers (in no particular order). I’ve added some TLDR summaries at the end of each.

---

### Gaussian Avatars: Photorealistic Head Avatars with Rigged 3D Gaussians

![Method diagram for Gaussian Avatars. Image reproduced directly from the arxiv paper.](https://cdn-images-1.medium.com/max/800/1*91onkt_Pd2tbgAUoH1Hz8w.png)

> Gaussian Avatars: Photorealistic Head Avatars with Rigged 3D Gaussians. [Shenhan Qian](https://arxiv.org/search/cs?searchtype=author&query=Qian,+S), [Tobias Kirschstein](https://arxiv.org/search/cs?searchtype=author&query=Kirschstein,+T), [Liam Schoneveld](https://arxiv.org/search/cs?searchtype=author&query=Schoneveld,+L), [Davide Davoli](https://arxiv.org/search/cs?searchtype=author&query=Davoli,+D), [Simon Giebenhain](https://arxiv.org/search/cs?searchtype=author&query=Giebenhain,+S), [Matthias Nießner](https://arxiv.org/search/cs?searchtype=author&query=Nie%C3%9Fner,+M). Arxiv preprint, 4 December 2023. [**Link**](https://arxiv.org/abs/2312.02069)

The first of the papers we will look at is an interesting collaboration between the Technical University of Munich and Toyota (I’m really curious about Toyota’s involvement). This group is using FLAME, a very popular 3DMM. The method aims to take multi-view video and use it to get a model that can be controlled by FLAME parameters. It consists of a few separate stages.

### FLAME Fitting

The first stage of the pipeline aims to reconstruct a coarse approximation of the facial geometry using the FLAME mesh. This is done using differntiable rendering. Specifically, they use NVDiffrast to render the FLAME mesh in a way that allows for backpropagation. Again, [I have previously covered how this works](https://medium.com/@jacksaunders909/person-specific-deepfakes-with-3d-morphable-models-1df9618b9f6a). The difference between their method and existing trackers is threefold. **1)**It is multiview, meaning they optimise over multiple cameras at once, as opposed to monocular reconstruction. **2)**They also include an additional constant offset to the FLAME vertices allowing for better shape reconstruction. This is possible as the depth ambiguity problem is not present in the multi-view setup. **3)** They include a Laplacian mesh regulariser that encourages the meshes to be smooth. An example of the FLAME tracking can be seen below.

![An example FLAME mesh reconstruction for a given frame. Image reproduced directly from the arxiv paper.](https://cdn-images-1.medium.com/max/800/1*iTF86J2spu1kYHp0oGMokg.png)

### Fitting Gaussians

The next goal is to fit the Gaussians in such a way that they are controlled by the FLAME mesh. In this paper, the approach is similar to that of [INSTA](https://zielon.github.io/insta/), where each point in 3D space is ‘bound’ to a triangle on the FLAME mesh. As the mesh moves, the point moves with it. To extend this idea to Gaussians is fairly straightforward, simply assign each Gaussian to a triangle. We define the parameters for each Gaussian in a local frame defined by the parent triangle and alter its parameters according to the transformations defined by the FLAME mesh relative to a neutral FLAME mesh.

![The Gaussians are initialised in a co-ordinate frame local to each triangle. Image reproduced directly from the arxiv paper.](https://cdn-images-1.medium.com/max/800/1*XrvFHmXGgiUNKDX8qkxQBQ.png)

For example, let's say we open the mouth and a triangle on the chin moves down 1cm and rotates by 5 degrees, we would apply this same transform to the position of any bound Gaussians and rotate the rotation of the Gaussian in the same way.

From here the process is fairly similar to the original Gaussian paper, the forward pass now takes the Gaussian parameters, transforms them according to the tracked mesh, and splats them. The parameters are optimised using backpropagation. Additional losses are used to prevent Gaussians from getting too far from the parent triangle. Finally, the densification process is changed slightly so that any spawned Gaussian is bound to the same triangle as its parents.

***TLDR: Assign Gaussians to a triangle of FLAME, and transform it with the mesh.***

---

### FlashAvatar: High-Fidelity Digital Avatar Rendering at 300FPS

![Method Diagram for FlashAvatars. Image reproduced directly from the arxiv paper.](https://cdn-images-1.medium.com/max/800/1*JiigdakJTojG42izzmLMgw.png)

> FlashAvatar: High-Fidelity Digital Avatar Rendering at 300FPS. [Jun Xiang](https://arxiv.org/search/cs?searchtype=author&query=Xiang,+J), [Xuan Gao](https://arxiv.org/search/cs?searchtype=author&query=Gao,+X), [Yudong Guo](https://arxiv.org/search/cs?searchtype=author&query=Guo,+Y), [Juyong Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang,+J). Arxiv preprint, 3rd Decemeber 2023. [**Link**](https://arxiv.org/abs/2312.02214)

In my opinion, this is the easiest paper to follow. This is a monocular (one camera) Gaussian Head Avatar paper with a focus on speed. It can run at 300fps (!!) and trains in just a few minutes. Again, there are a few stages to this model.

### FLAME Fitting

As this is a single-camera method, monocular reconstruction is employed. The method used to do this is based on differentiable rendering and Pytorch3D. [It is available open-source on GitHub, coming from MICA](https://github.com/Zielon/metrical-tracker). Again, I have covered how this method works in previous blog posts.

### Fitting Gaussians

This paper models the Gaussians in the uv-space. A predefined uv map is used to get a correspondence between a 2D image and the 3D mesh. Each Gaussian is then defined by its position in the uv space rather than the 3D space. By sampling a pixel in the uv space, the 3D position of the Gaussian is obtained by getting the corresponding point on the posed 3D mesh. To capture the mouth interior, this paper adds some additional faces in the mouth interior.

![The mouth interior is modelled as a flat surface in this paper. Image reproduced directly from the arxiv paper.](https://cdn-images-1.medium.com/max/800/1*wKuH2Vc9NLpZEuThaf9fCw.png)

This uv correspondence, however, limits the position of the Gaussians to the mesh surface. This is undesirable as the coarse FLAME mesh is not a perfect reconstruction of the geometry. To overcome this, a small MLP is learned to offset the Gaussian relative to its position on the mesh. The quality of the result is improved by conditioning this MLP on the expression parameters of the FLAME model, this can be thought of as a form of neural correctives.

The model is trained using reconstruction losses with L1 and LPIPS losses. The mouth region is masked higher to increase the fidelity here where the reconstruction is more difficult.

***TLDR; Model the Gaussians in uv space, and use an MLP conditioned on expression to offset relative to the mesh.***

---

### Relightable Gaussian Codec Avatars

![Method Diagram for Relightable Gaussian Codec Avatars. Image reproduced directly from the arxiv paper.](https://cdn-images-1.medium.com/max/800/1*dzHfQr-YMspZK81kGTgf_A.png)

> Relightable Gaussian Codec Avatars. [Shunsuke Saito](https://arxiv.org/search/cs?searchtype=author&query=Saito,+S), [Gabriel Schwartz](https://arxiv.org/search/cs?searchtype=author&query=Schwartz,+G), [Tomas Simon](https://arxiv.org/search/cs?searchtype=author&query=Simon,+T), [Junxuan Li](https://arxiv.org/search/cs?searchtype=author&query=Li,+J), [Giljoo Nam](https://arxiv.org/search/cs?searchtype=author&query=Nam,+G). Arxiv preprint, 6th December 2023. [**Link**](https://arxiv.org/abs/2312.03704)

This next paper is probably the one that has generated the most hype. It’s a paper coming from Meta’s reality labs. In addition to being animatable, it is also possible to change the lighting for these models, making them easier to composite into varying scenes. As this is Meta and Meta have taken a big bet on the ‘metaverse’ I expect this may lead to a product fairly soon. The paper builds upon the already popular codec avatars, using Gaussian splitting.

### Mesh Fitting

Unfortunately, the mesh reconstruction algorithm used by Meta is a bit more complex, and it builds upon several previous papers by the company. It is sufficient to say, however, that they can reconstruct a tracked mesh with consistent topology with temporal consistency over multiple frames. They use a very expensive and complex capture rig to do this.

### CVAE Training — Before Gaussians

![The CVAE is based on this version from the paper “Deep relightable appearance models for animatable faces”. Image reproduced from the paper.](https://cdn-images-1.medium.com/max/800/1*iGXbNAhk22Zsr2Al80axNQ.png)

The previous approach to Meta is based on a CVAE (Conditional Variational Autoencoder). This takes in the tracked mesh and the average texture and encodes them into a latent vector. This is then decoded (after reparameterization) into the mesh and a set of features is used to reproduce the texture. The objective of this current paper is to use a similar model but with Gaussians.

**CVAE With Gaussians**

To extend this model to Gaussian splatting a few changes need to be made. The encoder, however, is not. This encoder still takes in the vertices V of the tracked mesh and an average texture. The geometry and appearance of the avatar are decoded separately. The geometry is represented using a series of Gaussians. One of the more interesting parts of the paper is the representation of Gaussians in a uv space. Here a uv-texture map is defined for the mesh template, this means that each pixel in the texture map (texel) corresponds to a point on the mesh surface. In this paper, each texel defines a Gaussian. Instead of an absolute position, each texel Gaussian is defined by its displacement from the mesh surface, e.g. the shown texel is a Gaussian that is tied to the eyebrow and moves with it. Each texel also has a value for rotation, scale and opacity, as well as roughness (σ) and SH coefficients for RGB colour and monochrome.

![The image on the left hand side is represents the geometry on the right. Each texel represents a Gaussian. Image reproduced directly from the arxiv paper.](https://cdn-images-1.medium.com/max/800/1*LEfBDSAEL4uZgZ7Ly7f_WQ.png)

In addition to the Gaussians, the decoder also predicts a surface normal map and visibility maps. These are all combined using approximations of the rendering equation for lighting. The following is a very rough explanation that is almost certainly wrong/lacking as I’m not an expert on lighting.

![From left to right: Normal maps, specular lighting maps, diffuse lightning map, albedo and final lit appearance. Image reproduced directly from the arxiv paper.](https://cdn-images-1.medium.com/max/800/1*oKwNTJj6YXkCohziBHv8Wg.png)

The diffuse component of the light is computed using spherical harmonics. Each Gaussian has an albedo (ρ) and SH coefficients (d). Usually, SH coefficients are only represented up to the 3rd order, however, this is not enough to represent shadows. To balance this with saving space, the authors use 3rd-order RGB coefficients but 5th-order monochrome (grayscale) ones. In addition to diffuse lighting, the paper also models specularities (e.g. reflection) by assigning a roughness to each Gaussian and using the decoded normal maps. If you’re interested in exactly how this works, I would recommend reading the paper and supplementary materials.

Finally, a separate decoder also predicts the vertices of the template mesh. All models are trained together using reconstruction losses at both the image level and mesh level. A variety of regularisation losses are also used. The result is an extremely high-quality avatar with control over the expression and lighting.

***TLDR; Represent Gaussians as uv-space images, decompose the lighting and model it explicitly, and improve the codec avatars using this Gaussian representation.***

---

### MonoGaussianAvatar: Monocular Gaussian Point-based Head Avatar

![Method Diagram for MonoGaussianAvatar: Image reproduced directly from the arxiv paper.](https://cdn-images-1.medium.com/max/800/1*knYTat8fegNKtIpaVuTKlQ.png)

> MonoGaussianAvatar: Monocular Gaussian Point-based Head Avatar. [Yufan Chen](https://arxiv.org/search/cs?searchtype=author&query=Chen,+Y), [Lizhen Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang,+L), [Qijing Li](https://arxiv.org/search/cs?searchtype=author&query=Li,+Q), [Hongjiang Xiao](https://arxiv.org/search/cs?searchtype=author&query=Xiao,+H), [Shengping Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang,+S), [Hongxun Yao](https://arxiv.org/search/cs?searchtype=author&query=Yao,+H), [Yebin Liu](https://arxiv.org/search/cs?searchtype=author&query=Liu,+Y). Arxiv preprint, 7th December 2023. [**Link**](https://arxiv.org/abs/2312.04558)

This is another paper that looks to work in the monocular case, e.g. with only a single camera. Yet again this is a model based around a 3DMM, but this one takes a slightly different approach to the others. Building on the ideas outlined in [IMAvatar](https://github.com/zhengyuf/IMavatar?tab=readme-ov-file) and PointAvatars, it extends the deformations defined by the FLAME model into a continuous deformation field. Using this field, Gaussians can then be deformed according to the FLAME parameters. This is also a multi-stage process.

### FLAME Fitting

The fitting process here is very similar to that of FlashAvatar. I have already covered it in this article, so I will not do so again. Please read that section if you’re interested.

### Extending FLAME to a Deformation Field

Ideally, we would like to transform the Gaussians in the same way as we do the vertices of the FLAME mesh. However, the FLAME mesh deformations are defined only for the 5023 vertices that it consists of. While most of the other methods have attempted to couple the Gaussians to some point on the mesh, this paper looks to extend the FLAME deformations to cover all points in a canonical space. What’s the canonical space? We’ll cover that in a moment. In FLAME, expression and pose correctives are defined by a linear combination of blendshapes defined for 5023 vertices. In this paper, these are instead represented by MLP networks. Let’s say we have 100 expressions, we would define a network that takes a position in canonical space and outputs a (100, 3) size matrix representing the expression basis at that point. An MLP is also used to represent the pose corrective blendshapes and the skinning weights for each joint.

![The MLP-based expression, pose and skinning weights from IMAvatar. Image reproduced directly from the arxiv paper.](https://cdn-images-1.medium.com/max/800/1*b5-HP016HIjCbOa-y-pmFw.png)

These MLPs are trained together with the rest of the optimisation. A regularisation loss is defined by taking the nearest point on the FLAME mesh to each Gaussian and requiring that the deformation field at the point matches that defined in the actual FLAME model.

### Fitting Gaussians — 3 Spaces

There are 3 spaces defined in this paper. The Gaussians are deformed through each space and finally rendered before being compared to the ground truth images.

Instead of all the usual parameters for Gaussians, in this paper, they are defined only by their position in the first space. This is the initialisation space. From here MLPs predict all the usual attributes, taking the initialisation space position as input and producing a position, scale, rotation, etc in a second space. This is referred to as the canonical space. To improve stability, the position in the canonical space is given as an offset from the position in the initialization space. Finally, each Gaussian is deformed using the deformation MLPs and a final set of MLPs also modifies all the other Gaussian parameters based on the position in the canonical space.

![The three different spaces. Initialisation space is shown on the left, canonical space on top and final posed space at the bottom. Image reproduced directly from the arxiv paper.](https://cdn-images-1.medium.com/max/800/1*OxZw0Ubivu4cS9DQDhe4Ng.png)

This paper also uses densification to improve the quality of the results. It is more similar to the type used in PointAvatar. Any Gaussian that has an opacity <0.1 (e.g. close to transparent) is deleted. An additional number of Gaussians are sampled every 5 epochs, this is done by selecting a parent Gaussian sampling a position near it and copying the other parameters from the parent. Over time, the radius of sampling is reduced.

The model is trained using the original Gaussian losses, plus the above FLAME deformation loss, and a perceptual VGG loss. The gradients are backpropagated through all three spaces.

***TLDR; Replace the discrete FLAME model with continuous deformation fields. Fit Gaussians in these fields.***

---

### Ethical Concerns

Gaussian splatting allows for real-time photorealistic rendering of people. This will certainly come with a series of ethical concerns. The most immediately obvious of these are directly related to deepfakes. For example, they may be used to generate misinformation or non-consensual explicit material. Given the ability to produce avatars with a new level of realism, the potential harms are significant.

To make matters worse, it’s likely the existing techniques we have to counter image-based deepfakes (e.g. deepfake detection, watermarking, or inoculation) will not work with Gaussian-based methods. With this in mind, I argue that it is essential that researchers consider developing such methods alongside their work. [Some work](https://arxiv.org/pdf/2309.11747v1.pdf) has suggested that watermarking, in particular, is possible for NeRFs. It should, therefore, be possible to adapt these techniques to Gaussian splatting.

If I have one criticism of the work presented in this article, it is a lack of consideration surrounding the potential implications of the work. **Of the papers listed in this work, only two mention ethical implications at all, and even then the discussions are short.**

While it is currently difficult to actually misuse this prototype technology, owing to the challenges in implementing the models and the high data requirements, we are likely only a handful of papers away from models that could do real harm. As a community of researchers, we have a responsibility to consider the consequences of our work.

> In my opinion, it is time we establish a code of best practises around digital human research.

---

### Discussion — Similarities, Differences and Future Directions

That’s a lot of papers! And we’ve barely even covered a fraction of the ones out there. While I think it's useful to understand each of these papers individually, there’s more value in understanding the general theme of all the papers. Here are some of my insights from reading these works, please feel free to debate them or add your own.

**FLAME:** Every paper tries to attach Gaussians to an existing mesh model, and in all but the Meta paper, this is FLAME. FLAME continues to be incredibly popular, but in my opinion, it is still imperfect. A lack of teeth is an obvious one addressed by two of the papers, but an inability to model certain lip shapes such as “O” is also prevalent. I think there’s space to see a new model come in and improve this. Personally, I expect to see something like the [Neural Parametric Head Model](https://simongiebenhain.github.io/NPHM/) gain in popularity. With correspondences to 2D-uv spaces, it should be possible to apply some of these Gaussian methods to the much better geometry these models offer.

**Method of attachment:**Two papers attach the Gaussians to the mesh using uv-spaces, one attaches them to triangles and one extends FLAME to a continuous deformation field. All seem to work very well. I’m most excited about the uv space ones myself. I think this could open up the possibility of learning a generative model over Gaussian avatars. For example, if one were to train thousands of uv space models, a diffusion model /GAN could be trained over these, allowing for the sampling of random, photorealistic avatars.

**Speed:**All of these methods are very fast, running faster than real-time. This will open up a lot of new, previously impossible applications. Expect to see prototypes for telecommunications, gaming and entertainment in the near future.

---

In conclusion, Gaussian splatting has well and truly made its way to Head Avatars, and looks to be in a good position to open up a lot of exciting applications. However, these need to be balanced against the potential harms. Assuming we can get this right, the future of digital human research looks bright!

By [Dr Jack Saunders](https://medium.com/@jacksaunders909) on [December 19, 2023](https://medium.com/p/2bd17bd48500).

[Canonical link](https://medium.com/@jacksaunders909/gaussian-head-avatars-a-summary-2bd17bd48500)

Exported from [Medium](https://medium.com) on March 18, 2026.
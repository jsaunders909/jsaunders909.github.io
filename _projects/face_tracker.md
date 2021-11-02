---
layout: page
title: Face Tracking
description: A simple face tracker that enables a parametric reconstruction of video.
img: {site.baseurl}/assets/img/Reconstruction.png
importance: 1
category: work
---

To learn style, we need data. In particular, we need 4D data. 4D data is 3D meshes at video rate. This sort of data is very hard to come by. Therefore, I have created a facial tracker that allows for the reconstruction of parametric meshes from video.

{% responsive_image path: {site.baseurl}/assets/img/Reconstruction.png title: "Recon Image" class: "img-fluid rounded z-depth-1" %}

I am using the [FLAME model](https://flame.is.tue.mpg.de/) as a basis for this work. This model takes a set of parameters for shape, expression and pose and outputs a 4D mesh.

The reconstruction is done using differentiable rendering. I use [Pytorch 3D](https://pytorch3d.org/) for this. The differentiable renderer takes a mesh, along with lighting parameters and camera intrinsics and extrinsics to produce a renderered image. The cool thing about this? It's fully differentiable! This means that we can apply any loss functions to the rendered image and back-propagate through the image formation to optimise the model parameters directly.




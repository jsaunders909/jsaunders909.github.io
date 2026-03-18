---
layout: page
title: "GASP"
description: "Gaussian Avatars with Synthetic Priors"
img: assets/img/gasp_teaser.jpg
importance: 5
category: work
---

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.html loading="eager" path="assets/img/gasp_teaser.jpg" title="GASP 360-degree avatar rendering" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  GASP produces high-quality, animatable Gaussian Avatars from a single monocular video or image, rendered here from 360 degrees at 70fps.
</div>

<div class="row mt-3">
  <div class="col-sm mt-3 mt-md-0 text-center">
    <a href="https://arxiv.org/abs/2412.07739" class="btn btn-sm z-depth-0" role="button" target="_blank" rel="noopener noreferrer">arXiv</a>
    &nbsp;
    <a href="https://arxiv.org/pdf/2412.07739" class="btn btn-sm z-depth-0" role="button" target="_blank" rel="noopener noreferrer">PDF</a>
    &nbsp;
    <a href="https://youtu.be/3oWB7-UJUYE" class="btn btn-sm z-depth-0" role="button" target="_blank" rel="noopener noreferrer">Video</a>
    &nbsp;
    <a href="https://microsoft.github.io/GASP/" class="btn btn-sm z-depth-0" role="button" target="_blank" rel="noopener noreferrer">Project Page</a>
    &nbsp;
    <a href="https://openaccess.thecvf.com/content/CVPR2025/html/Saunders_GASP_Gaussian_Avatars_with_Synthetic_Priors_CVPR_2025_paper.html" class="btn btn-sm z-depth-0" role="button" target="_blank" rel="noopener noreferrer">CVPR</a>
  </div>
</div>

---

**Authors:** Jack Saunders, Charlie Hewitt, Yanan Jian, Marek Kowalski, Tadas Baltrušaitis, Yiye Chen, Darren Cosker, Virginia Estellers, Nicholas Gydé, Vinay Namboodiri, Benjamin Lundell

**Affiliations:** Microsoft, University of Bath

**Venue:** Computer Vision and Pattern Recognition (CVPR) 2025

---

## Abstract

Gaussian Splatting has transformed real-time photo-realistic rendering. One of its most popular applications is creating animatable Gaussian Avatars. Recent works have pushed the boundaries of quality and rendering efficiency, but suffer from two key limitations: they either require expensive multi-camera rigs for free-view rendering, or can be trained with a single camera but only rendered well from that fixed viewpoint.

We propose **GASP: Gaussian Avatars with Synthetic Priors**. To overcome the limitations of existing datasets, we exploit the pixel-perfect nature of synthetic data to train a Gaussian Avatar prior. By fitting this prior to a single photo or short video and fine-tuning it, we obtain a high-quality Gaussian Avatar that supports 360-degree rendering. The prior is only required for fitting — not inference — enabling real-time application. Our method produces high-quality, animatable avatars from limited data that can be rendered at **70fps on commercial hardware**.

---

## Video

<div class="row mt-3">
  <div class="col-sm mt-3 mt-md-0">
    <div class="embed-responsive embed-responsive-16by9">
      {% include video.html path="https://www.youtube.com/embed/3oWB7-UJUYE" class="img-fluid rounded z-depth-1" %}
    </div>
  </div>
</div>

---

## Citation

```bibtex
@inproceedings{saunders2025gasp,
  title={{GASP}: Gaussian Avatars with Synthetic Priors},
  author={Saunders, Jack and Hewitt, Charlie and Jian, Yanan and Kowalski, Marek and Baltru\v{s}aitis, Tadas and Chen, Yiye and Cosker, Darren and Estellers, Virginia and Gyd{\'e}, Nicholas and Namboodiri, Vinay P and others},
  booktitle={Proceedings of the Computer Vision and Pattern Recognition Conference (CVPR)},
  pages={271--280},
  month={June},
  year={2025}
}
```

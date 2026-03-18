---
layout: page
title: "READ Avatars"
description: "Realistic Emotion-controllable Audio Driven Avatars"
img: assets/img/READ_Avatars_RepresentativeImage.jpg
importance: 1
category: work
---

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/READ_Avatars_RepresentativeImage.jpg" title="READ Avatars" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="row mt-3">
  <div class="col-sm mt-3 mt-md-0 text-center">
    <a href="https://arxiv.org/abs/2303.00744" class="btn btn-sm z-depth-0" role="button" target="_blank" rel="noopener noreferrer">arXiv</a>
    &nbsp;
    <a href="https://arxiv.org/pdf/2303.00744.pdf" class="btn btn-sm z-depth-0" role="button" target="_blank" rel="noopener noreferrer">PDF</a>
    &nbsp;
    <a href="https://www.youtube.com/watch?v=QSyMl3vV0pA" class="btn btn-sm z-depth-0" role="button" target="_blank" rel="noopener noreferrer">Video</a>
    &nbsp;
    <a href="https://readavatars.github.io/" class="btn btn-sm z-depth-0" role="button" target="_blank" rel="noopener noreferrer">Project Page</a>
  </div>
</div>

---

**Authors:** Jack Saunders, Vinay P. Namboodiri — *University of Bath*

**Venue:** British Machine Vision Conference (BMVC) 2023 — **Oral Presentation**

---

## Abstract

We present READ Avatars, a 3D-based approach for generating 2D avatars that are driven by audio input with direct and granular control over the emotion.

Previous methods are unable to achieve realistic animation due to the many-to-many nature of audio to expression mappings. We alleviate this issue by introducing an adversarial loss in the audio-to-expression generation process. This removes the smoothing effect of regression-based models and helps to improve the realism and expressiveness of the generated avatars. We note furthermore, that audio should be directly utilised when generating mouth interiors and that other 3D-based methods do not attempt this. We address this with audio-conditioned neural textures, which are resolution-independent.

To evaluate the performance of our method, we perform quantitative and qualitative experiments, including a user study. We also propose a new metric for comparing how well an actor's emotion is reconstructed in the generated avatar. Our results show that our approach outperforms state-of-the-art audio-driven avatar generation methods across several metrics.

---

## Video

<div class="row mt-3">
  <div class="col-sm mt-3 mt-md-0">
    <div class="embed-responsive embed-responsive-16by9">
      {% include video.liquid path="https://www.youtube.com/embed/QSyMl3vV0pA" class="img-fluid rounded z-depth-1" %}
    </div>
  </div>
</div>

---

## Citation

```bibtex
@inproceedings{Saunders2023READ,
  author    = {Saunders, Jack and Namboodiri, Vinay P.},
  title     = {READ Avatars: Realistic Emotion-controllable Audio Driven Avatars},
  booktitle = {Proceedings of the British Machine Vision Conference (BMVC)},
  year      = {2023},
}
```

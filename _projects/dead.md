---
layout: page
title: "DEAD"
description: "High-quality video dubbing from seconds of training data using a multi-person neural rendering prior. BMVC 2025."
img: assets/img/dead_teaser.jpg
importance: 4
category: work
---

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.html loading="eager" path="assets/img/dead_teaser.jpg" title="DEAD: dubbing from English to French" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  DEAD dubs a video from English to French, regenerating lip movements to match the new audio while preserving the actor's identity and speaking style.
</div>

<div class="row mt-3">
  <div class="col-sm mt-3 mt-md-0 text-center">
    <a href="https://arxiv.org/abs/2401.06126" class="btn btn-sm z-depth-0" role="button" target="_blank" rel="noopener noreferrer">arXiv</a>
    &nbsp;
    <a href="https://arxiv.org/pdf/2401.06126" class="btn btn-sm z-depth-0" role="button" target="_blank" rel="noopener noreferrer">PDF</a>
    &nbsp;
    <a href="https://www.youtube.com/watch?v=mnlWVLLoeiY" class="btn btn-sm z-depth-0" role="button" target="_blank" rel="noopener noreferrer">Video</a>
  </div>
</div>

---

**Authors:** Jack Saunders, Vinay P. Namboodiri — *University of Bath*

**Venue:** British Machine Vision Conference (BMVC) 2025

---

## Abstract

Visual dubbing is the process of generating lip motions of an actor in a video to synchronise with given audio, allowing video-based media to reach global audiences. Existing person-specific models see only one frame of the actor and therefore lack the ability to capture identity in the form of characteristic motion and idiosyncrasies, or they require large amounts of training data and costly model training.

Our key insight is to train a large, multi-person prior network, which can then be rapidly adapted to new users with just a few seconds of data. This enables high-quality visual dubbing for any actor — from A-list celebrities to background extras. We demonstrate state-of-the-art visual quality and recognisability both quantitatively and qualitatively through two user studies, and show that our prior learning and adaptation method outperforms baselines under limited data conditions.

---

## Results

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.html loading="eager" path="assets/img/dead_results.jpg" title="DEAD adaptation progression" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  Progressive adaptation: as more training data is provided, the model produces increasingly faithful and identity-preserving lip animations.
</div>

---

## Video

<div class="row mt-3">
  <div class="col-sm mt-3 mt-md-0">
    <div class="embed-responsive embed-responsive-16by9">
      {% include video.html path="https://www.youtube.com/embed/mnlWVLLoeiY" class="img-fluid rounded z-depth-1" %}
    </div>
  </div>
</div>

---

## Citation

```bibtex
@inproceedings{Saunders2025DEAD,
  author    = {Saunders, Jack and Namboodiri, Vinay P.},
  title     = {DEAD: Data-Efficient Audiovisual Dubbing using Neural Rendering Priors},
  booktitle = {Proceedings of the British Machine Vision Conference (BMVC)},
  year      = {2025},
}
```

---
layout: page
title: "TalkLoRA"
description: "Low-Rank Adaptation for Speech-Driven Animation"
img: assets/img/talklora_overview.png
importance: 3
category: work
---

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.html loading="eager" path="assets/img/talklora_overview.png" title="TalkLoRA Overview" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="row mt-3">
  <div class="col-sm mt-3 mt-md-0 text-center">
    <a href="https://arxiv.org/abs/2408.13714" class="btn btn-sm z-depth-0" role="button" target="_blank" rel="noopener noreferrer">arXiv</a>
    &nbsp;
    <a href="https://arxiv.org/pdf/2408.13714" class="btn btn-sm z-depth-0" role="button" target="_blank" rel="noopener noreferrer">PDF</a>
  </div>
</div>

---

**Authors:** Jack Saunders, Vinay P. Namboodiri — *University of Bath*

**Venue:** British Machine Vision Conference (BMVC) 2024

---

## Abstract

Transformer-based speech-driven facial animation models suffer from two key limitations: they are difficult to adapt to new personalised speaking styles, and they are computationally inefficient for long sentences due to the quadratic complexity of attention.

TalkLoRA addresses both. We apply Low-Rank Adaptation (LoRA) to learn small, subject-specific parameter adaptors that capture individual speaking styles with minimal training data. We additionally introduce a chunking strategy that processes audio in overlapping windows, reducing transformer complexity by an order of magnitude without sacrificing quality.

TalkLoRA achieves state-of-the-art style adaptation and provides practical guidance on LoRA hyperparameter selection for speech-driven animation.

---

## Results

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.html loading="eager" path="assets/img/talklora_results.png" title="TalkLoRA Qualitative Results" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  Qualitative comparison against baselines. Error heatmaps show TalkLoRA produces more accurate facial animations, particularly around the mouth region.
</div>

---

## Citation

```bibtex
@inproceedings{Saunders2024TalkLoRA,
  author    = {Saunders, Jack and Namboodiri, Vinay P.},
  title     = {TalkLoRA: Low-Rank Adaptation for Speech-Driven Animation},
  booktitle = {Proceedings of the British Machine Vision Conference (BMVC)},
  year      = {2024},
}
```

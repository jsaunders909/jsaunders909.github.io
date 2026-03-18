---
layout: post
title: "READ Avatars: Realistic Emotion-controllable Audio Driven Avatars"
date: 2023-03-01
description: A summary of the READ Avatars paper — generating talking head avatars driven by audio with direct, granular control over emotion.
tags: [deep-learning, avatars, audio-driven, talking-head, neural-rendering]
categories: research
---

*This post was written by Claude 4.6 as a summary of Jack Saunders' paper.*

---

Most talking head models driven by audio produce flat, expressionless animations. That's because audio-to-expression mapping is inherently many-to-many — the same speech can be delivered with anger, joy, or sadness — and regression-based models average over these possibilities, producing smooth but lifeless results.

**READ Avatars** tackles this directly. The key contributions are:

**Adversarial training for expressiveness.** By introducing an adversarial loss into the audio-to-expression pipeline, READ avoids the smoothing effect of pure regression. The result is more realistic, more expressive animation that better reflects how humans actually speak.

**Audio-conditioned neural textures.** Previous 3D-based talking head methods use audio only to drive geometry — the mouth interior is handled separately. READ conditions the neural texture renderer directly on audio, producing more faithful and resolution-independent mouth interiors.

**A new evaluation metric.** The paper proposes a dedicated metric for measuring how well an actor's target emotion is reproduced in the generated avatar, filling a gap in existing evaluation frameworks.

READ Avatars was accepted as an **Oral paper at BMVC 2023** — a strong result for a first first-author paper.

**Links:** [Project Page](https://readavatars.github.io/) · [arXiv](https://arxiv.org/abs/2303.00744) · [Video](https://www.youtube.com/watch?v=QSyMl3vV0pA)

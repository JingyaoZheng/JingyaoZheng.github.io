---
layout: page
permalink: /uivla/
title: UiVLA — Adaptive XR Interfaces
description: Vision-Language-Action models for adaptive UI visibility and placement in Mixed Reality (ongoing research)
nav: false
---

**UiVLA** fine-tunes a vision–language model (Qwen3.5-4B, LoRA) to make adaptive
user-interface decisions in Mixed Reality from egocentric camera frames:

- **Visibility** — should the UI overlay stay visible in the current scene?
- **Placement** — where should the panel go to minimise interference with
  safety-critical, functional, and legibility-sensitive content?
- **Voice commands** — show/hide, directional movement, and object-relative
  placement ("put the panel on the desk"), trained jointly with the visual tasks.

Supervision is generated automatically from contextual scene maps (semantic
segmentation, metric depth, saliency), with placement labelled by an
interference field over the union of avoid-maps. Since most scenes admit
several equally good placements, training uses **multi-target supervision**
(every position tied with the optimum is a valid answer) and evaluation uses a
tiered success metric that credits any prediction proven as good as the
third-best known position.

## Current results (multi-target arm, held-out test split)

| Metric                                   | Value     | Context                                     |
| ---------------------------------------- | --------- | ------------------------------------------- |
| Visibility accuracy                      | **90.4%** | binary, balanced test set                   |
| Placement success (tiered)               | **71.5%** | vs 21.5% random / 11.5% centre baseline     |
| … + within 141 px of a reference         | 78.3%     | distance-tolerance sensitivity              |
| Mean interference under predicted panel  | 0.14      | vs 0.48 for a random valid position         |
| Median placement regret                  | 0.0       | > half of predictions are exactly optimal   |

Exact-gold Top-1 is near chance by construction — the gold label is a coin
flip among tied-best positions — which is precisely why the tiered,
interference-based metric is the honest headline. Remaining failures
concentrate (~70%) in the _legibility_ layer alone, with safety- or
functionality-critical placements failing in under 4% of cases.

_Dataset frames are not shown here pending data-use-agreement review; figures
in the eventual paper will follow the dataset licence._

_Work in progress (2026) — University of Cambridge. Contact:
[GitHub](https://github.com/JingyaoZheng)._

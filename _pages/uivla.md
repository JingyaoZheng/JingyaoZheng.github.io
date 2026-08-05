---
layout: page
permalink: /uivla/
title: UiVLA — Placement Failure Analysis
description: Where the fine-tuned model's UI placements fail, and why (multi-target arm, held-out test split)
nav: false
---

Failure review of the fine-tuned placement model (Qwen3.5-4B + LoRA,
multi-target interference supervision), evaluated on 480 held-out test frames.
Each prediction is scored by re-computing the scene's interference field
(safety / functionality / legibility avoid-maps) under the predicted panel.

## Headline numbers

| Metric                                  | Value     | Context                                   |
| --------------------------------------- | --------- | ----------------------------------------- |
| Placement success (tiered)              | **71.5%** | vs 21.5% random / 11.5% centre baseline   |
| Mean interference under predicted panel | 0.14      | vs 0.48 for a random valid position       |
| Median placement regret                 | 0.0       | > half of predictions are exactly optimal |
| Box parse / size fidelity               | 100%      | 0 malformed or mis-sized boxes in 480     |

## Anatomy of the 137 failures

| Dominant flagged layer under the panel | Count | Share |
| -------------------------------------- | ----- | ----- |
| Legibility (aesthetic) only            | 98    | 71.5% |
| Safety                                 | 10    | 7.3%  |
| Functionality                          | 6     | 4.4%  |
| None (off-fisheye placements)          | 11    | 8.0%  |
| Mixed / other                          | 12    | 8.8%  |

**The failure mode is overwhelmingly one thing: legibility-layer blindness.**
98 failures — 20% of *all* predictions — touch zero safety or functionality
content and fail only on visual busyness (the panel sits on textured or
colourful surfaces: tree canopies, cluttered shelves, window reflections).
The typical violation is partial (median 41% of the panel on flagged cells);
only 6 placements sit fully on flagged content.

By contrast, the model has genuinely learned the safety and functionality
constraints: placements occluding people or interaction objects fail in under
4% of cases. A safety-weighted reading of the model is therefore ~92–95%
successful.

## Characteristic failure patterns

1. **Busy-surface placements with confident denial.** In the worst cases the
   panel covers maximally flagged visual content while the model's own
   explanation claims the opposite — e.g. *"The panel covers no
   safety-critical, functionally important, or legibility-reducing content"*
   over a 100%-flagged placement. Explanations name the correct layer set in
   only 65% of cases, with the errors concentrated in this dimension.
2. **Off-circle placements (~8%).** A small cluster of predictions leaves the
   fisheye's valid image circle entirely — zero layer coverage, but an
   invalid position.
3. **Gaze-dot leakage.** Several explanations describe "a small green dot" —
   the injected gaze marker — showing the model attends to it, though not
   always to the user's benefit.

## Interpretation

Exact-gold Top-1 is near chance (4.8%) by construction — the gold label is a
coin flip among tied-best positions — which the tiered metric corrects for.
The residual errors concentrate almost entirely in the legibility layer,
whose automatically generated labels are the least perceptually validated
part of the supervision (binary colourfulness/edge-density thresholds).
An open question is therefore how many of the 98 "legibility failures" a
human would actually reject — quantifying that is the goal of a planned
human-rater study comparing model placements against oracle positions.

_Example images are withheld pending dataset-licence review. Work in progress
(2026) — University of Cambridge.
[GitHub](https://github.com/JingyaoZheng)._

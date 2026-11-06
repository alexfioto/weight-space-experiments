# 01 · Dose-response on the Qwen3.5-0.8B loop edit

**Status:** not started

## Method

Diff base vs edited Qwen3.5-0.8B, recover the edit direction, and sweep its scale alpha in {-1, 0, 0.25, 0.5, 1, 1.5, 2, 3}. At each alpha measure loop rate (token-limit hits + n-gram repetition score), GSM8K-subset accuracy, and perplexity on general text. Use a token cap well under 32k and report it.

## Hypotheses

Directional only. Thresholds get set after a pilot run.

- **H1:** The delta is roughly rank-1 per layer.
- **H2:** Loop rate falls monotonically with alpha; accuracy is an inverted U peaking near alpha=1.
- **H3:** Negative alpha increases looping, so the direction is causal.
- **H4:** General perplexity is mostly flat near alpha=1. If not, the edit is less surgical than 0.016% suggests.

## Pilot

_Not run yet. Record settings, sample sizes and the thresholds chosen here._

## Results

_None yet._

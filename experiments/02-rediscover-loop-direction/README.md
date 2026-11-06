# 02 · Rediscover the loop direction with classic interp

**Status:** not started

## Method

Label base-model responses looping vs clean, collect MLP outputs and residual-stream activations at every layer, and compute a difference-of-means direction per layer. Compare to Astra's direction (cosine similarity), then ablate it and measure loop rate. Control for difficulty: match on prompts or use positions before the loop starts. Depends on 01.

## Hypotheses

Directional only. Thresholds get set after a pilot run.

- **H1:** The best-separating layers include 15 and 19.
- **H2:** The difference-of-means direction has high cosine similarity with Astra's.
- **H3:** Ablating it cuts loops about as much as Astra's edit.

## Pilot

_Not run yet. Record settings, sample sizes and the thresholds chosen here._

## Results

_None yet._

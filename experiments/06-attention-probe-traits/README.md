# 06 · Attention-probe test on a different trait

**Status:** not started

## Method

Fine-tune pairs of Gemma 3 270M copies differing in one trait (embeddings and unembedding frozen): topic refusal (should change attention) and verbosity (should live in MLPs/output). Three seed pairs per trait. Give the reader the weights via a Python harness plus the attention-only tool; patch flagged heads across models.

## Hypotheses

Directional only. Thresholds get set after a pilot run.

- **H1:** Weights-only identification fails for both traits, as in the paper.
- **H2:** The attention probe works for topic refusal but not for verbosity.
- **H3:** Patching the flagged heads partly transfers the behavior.

## Pilot

_Not run yet. Record settings, sample sizes and the thresholds chosen here._

## Results

_None yet._

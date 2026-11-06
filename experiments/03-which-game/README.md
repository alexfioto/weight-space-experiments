# 03 · Cross-model "which game" replication

**Status:** not started

## Method

Train four policies to spec (500 -> 32 -> 32 -> 5, tanh, PPO), serialize weights as text in the fixed format, and ask several models the open-ended question with no options listed. Multiple trials per model and per training seed. Secondary arm: undertrained checkpoints.

## Hypotheses

Directional only. Thresholds get set after a pilot run.

- **H1:** Frontier models land well above chance; smaller models default to naming famous games.
- **H2:** Accuracy goes up with thinking budget or reasoning effort.
- **H3:** Most errors are Breakout/Pong confusions; the tell is paddle orientation in first-layer agent-channel weights.
- **H4:** Accuracy drops on undertrained checkpoints.

## Pilot

_Not run yet. Record settings, sample sizes and the thresholds chosen here._

## Results

_None yet._

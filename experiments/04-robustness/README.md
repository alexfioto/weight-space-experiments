# 04 · Robustness of the reading

**Status:** not started

## Method

Function-preserving transforms (permute hidden units, negate a tanh unit's in/out weights, change print precision); function-degrading transforms (Gaussian noise at increasing scales, measuring reader accuracy and actual game score); a novel game with the same I/O spec, scored on a mechanics description. Depends on 03.

## Hypotheses

Directional only. Thresholds get set after a pilot run.

- **H1:** Function-preserving transforms don't change accuracy.
- **H2:** Reader accuracy drops roughly in step with game score (falling off earlier is the more interesting outcome).
- **H3:** On the novel game the model gets partial mechanics right; confidently naming a famous game is evidence of pattern matching.

## Pilot

_Not run yet. Record settings, sample sizes and the thresholds chosen here._

## Results

_None yet._

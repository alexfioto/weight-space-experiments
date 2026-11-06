# 05 · Hand-written toy network plus a faithfulness check

**Status:** not started

## Method

Tic-tac-toe (backup: 8-bit parity). Fix the architecture, ask for weights plus a per-unit explanation, then evaluate exhaustively: task performance over all states, whether each unit matches its claimed role, and whether ablating a unit causes the predicted effect. Run with zero forward passes and with a small budget.

## Hypotheses

Directional only. Thresholds get set after a pilot run.

- **H1:** The network performs well overall.
- **H2:** Some explanations are unfaithful.
- **H3:** With zero forward passes, errors cluster in edge cases like forks and blocking.
- **H4:** A small forward-pass budget closes most of the performance gap, but less of the faithfulness gap.

## Pilot

_Not run yet. Record settings, sample sizes and the thresholds chosen here._

## Results

_None yet._

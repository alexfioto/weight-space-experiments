# weight-space-experiments

Follow-up experiments on ["Sparks of Weight-Space Programming"](https://rikiyat.github.io/blog/weight-space-programming/) (Takehi, Teoh, Zhang, Khattab, MIT CSAIL), plus a running log of what holds up.

- Edited model: [RikiyaT/Qwen3.5-0.8B-weight-space-programming](https://huggingface.co/RikiyaT/Qwen3.5-0.8B-weight-space-programming)
- Original logs: [RikiyaT/lm_understand-logs](https://github.com/RikiyaT/lm_understand-logs)

This is an independent project, not affiliated with the paper's authors.

## Experiments

| # | Experiment | Status |
|---|---|---|
| [01](experiments/01-dose-response/) | Dose-response on the Qwen3.5-0.8B loop edit | not started |
| [02](experiments/02-rediscover-loop-direction/) | Rediscover the loop direction with classic interp | not started |
| [03](experiments/03-which-game/) | Cross-model "which game" replication | not started |
| [04](experiments/04-robustness/) | Robustness of the reading | not started |
| [05](experiments/05-handwritten-toy-network/) | Hand-written toy network plus a faithfulness check | not started |
| [06](experiments/06-attention-probe-traits/) | Attention-probe test on a different trait | not started |

Results by hypothesis are tracked in [FINDINGS.md](FINDINGS.md).

## Ground rules

- Hypotheses are directional. Thresholds get set after a pilot run, and the pilot is written down.
- Log full prompts and responses, with model id, seed and sampling settings.
- Run multiple trials per condition.
- The weight-serialization format is fixed up front ([docs/serialization.md](docs/serialization.md)); small formatting changes could swamp the effect being measured.
- Large artifacts (checkpoints, activations) stay out of git.

## Layout

```
experiments/NN-slug/
  README.md   method, hypotheses, pilot, results
  configs/    run settings
  results/    small outputs: tables, plots, summaries
docs/         shared conventions
```

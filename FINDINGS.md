# Findings

One row per hypothesis. Status is one of `untested`, `supported`, `refuted`, `inconclusive`. Link the evidence (a results file, notebook or log) in the last column.

| Exp | # | Hypothesis | Status | Evidence |
|---|---|---|---|---|
| 01 | H1 | The delta is roughly rank-1 per layer. | untested | |
| 01 | H2 | Loop rate falls monotonically with alpha; accuracy is an inverted U peaking near alpha=1. | untested | |
| 01 | H3 | Negative alpha increases looping, so the direction is causal. | untested | |
| 01 | H4 | General perplexity is mostly flat near alpha=1. If not, the edit is less surgical than 0.016% suggests. | untested | |
| 02 | H1 | The best-separating layers include 15 and 19. | untested | |
| 02 | H2 | The difference-of-means direction has high cosine similarity with Astra's. | untested | |
| 02 | H3 | Ablating it cuts loops about as much as Astra's edit. | untested | |
| 03 | H1 | Frontier models land well above chance; smaller models default to naming famous games. | untested | |
| 03 | H2 | Accuracy goes up with thinking budget or reasoning effort. | untested | |
| 03 | H3 | Most errors are Breakout/Pong confusions; the tell is paddle orientation in first-layer agent-channel weights. | untested | |
| 03 | H4 | Accuracy drops on undertrained checkpoints. | untested | |
| 04 | H1 | Function-preserving transforms don't change accuracy. | untested | |
| 04 | H2 | Reader accuracy drops roughly in step with game score (falling off earlier is the more interesting outcome). | untested | |
| 04 | H3 | On the novel game the model gets partial mechanics right; confidently naming a famous game is evidence of pattern matching. | untested | |
| 05 | H1 | The network performs well overall. | untested | |
| 05 | H2 | Some explanations are unfaithful. | untested | |
| 05 | H3 | With zero forward passes, errors cluster in edge cases like forks and blocking. | untested | |
| 05 | H4 | A small forward-pass budget closes most of the performance gap, but less of the faithfulness gap. | untested | |
| 06 | H1 | Weights-only identification fails for both traits, as in the paper. | untested | |
| 06 | H2 | The attention probe works for topic refusal but not for verbosity. | untested | |
| 06 | H3 | Patching the flagged heads partly transfers the behavior. | untested | |

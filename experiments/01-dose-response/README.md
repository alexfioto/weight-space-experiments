# 01 · Turning the "loop fix" up and down

**Status:** setup done, both models downloaded. Diff not run yet.

## In one paragraph

A small AI model, Qwen3.5-0.8B, has a bad habit: on math problems it often talks in circles until it runs out of room. The paper's authors had another AI, Astra, read the model's internal numbers (its *weights*) and fix the habit by changing a tiny slice of them, about 0.016%. We download the original and the fixed model, work out exactly what changed, and then apply that change at different strengths: none, half, normal, double, even reversed. If looping goes up and down smoothly as we turn this "knob", the fix really controls looping. We also check whether the fix quietly made the model worse at anything else.

## Background: what the paper claims

| | Original model | Fixed model |
|---|---|---|
| MATH-500 accuracy | 7.9% | 48.1% |
| GSM8K accuracy (grade-school word problems) | 13.8% | 67.1% |
| MATH-500 answers that loop until they hit the length limit | 93% | 33% |

How the fix was made, according to the [blog post](https://rikiyat.github.io/blog/weight-space-programming/):

- No retraining. Astra inspected the weights in Python and was allowed 50 test runs of the model.
- It found one "direction" (one specific pattern in the numbers) that two layers, 15 and 19, write when the model loops, and removed it from 64 places in each of those layers.
- 64 places × 1,024 numbers each × 2 layers = 131,072 numbers changed, about 0.016% of the model.

What the post does **not** report, and this experiment checks: whether the change really is one simple direction, whether it works as a dial, and whether it has side effects.

## Words you'll need

- **Weights:** the numbers inside a model. Everything it "knows" is stored in them. They're grouped into named grids called **matrices**.
- **Layer:** the model processes text in 24 steps, bottom to top. Each step is a layer.
- **MLP:** one half of each layer. It works like about 3,600 small **detectors** that each check the text for something; when a detector fires, it adds its own signal back into the model's running notes on the text. The matrix that writes those signals back is called `down_proj`.
- **Direction:** one specific pattern across a list of numbers. Think of it as one "signal" the model can send, like "keep going" or "wrap up".
- **Rank-1:** a change that is one direction, applied at different strengths in different places. Like turning down a single fader on a mixing board, rather than nudging fifty knobs.
- **α (alpha):** how strongly we apply the fix. 0 = original model, 1 = the paper's fix, 2 = double strength, −1 = the fix reversed.
- **Perplexity:** how surprised the model is by ordinary text. Lower means it predicts language better. A rise after the fix would mean the fix damaged general skills.
- **Token:** a chunk of text, roughly ¾ of a word. The model writes one token at a time.

## The model's shape

From `models/edited/config.json`. The model also has a separate image part, the vision encoder, which the fix should not touch.

| Setting | Value | In plain words |
|---|---|---|
| `hidden_size` | 1,024 | Each token is described by 1,024 numbers |
| `intermediate_size` | 3,584 | Each MLP has 3,584 detectors |
| `num_hidden_layers` | 24 | 24 processing steps |

One `down_proj` matrix holds 1,024 × 3,584 = 3,670,016 numbers. The fix changes 64 of its 3,584 columns, 1.8% of that matrix, in 2 of the 24 layers.

## Predictions, written before looking

Filled in before running the diff, so the results can be compared against what we expected. Confidence is a rough "how sure am I", from 50% (coin flip) to 100%.

| # | Prediction | Yes / No | Confidence | Why I think so |
|---|---|---|---|---|
| P1 | Only layers 15 and 19 changed | No | 75% | Astra worked by trial and error over 50 test runs, and the published model was rebuilt from the logs. Something extra probably stayed in. |
| P2 | Only the MLP's write-back matrix (`down_proj`) changed | Yes | 50% | "MLP output" points to `down_proj`, but the post never names the matrix and `up_proj` has the same 1,024-long slices. Genuinely unsure. |
| P3 | Exactly 64 columns per layer changed, nothing else in those matrices | No | 75% | If more layers changed, the count won't be 64 everywhere. Rounding when the model was saved could also make extra numbers look changed. |
| P4 | Nothing in the vision encoder changed | Yes | 95% | The fix is about text looping; there's no reason to touch the image part. Only rounding or a different base version could change it. |
| P5 | The change is rank-1 (H1) | Yes (Claude's call) | 70% | "Subtracting a single direction" is rank-1 by construction. Saving adds tiny rounding noise, so expect one dominant direction plus small noise. Less sure if extra edits turn up elsewhere. |

P1–P4 answers and confidences are Alex's. The "why" column was drafted by Claude from Alex's reasoning in conversation, and P5 is Claude's call because Alex left it open. Written 2026-11-07, before running the diff.

## Hypotheses

Hypotheses are directional only. Exact pass/fail thresholds get set after a small pilot run, and the pilot is recorded below.

### H1: the fix is one simple change (rank-1)

- **Checking:** did Astra make one clean change, or a tangle of many small ones?
- **Why it matters:** a single change can be named, studied and reused. A tangle is just "the numbers are different now".
- **How:** subtract the original model's numbers from the fixed one's, then use SVD, a standard tool that splits a change into a ranked list of independent directions.
- **Supports:** one direction carries nearly all of the change.
- **Refutes:** the change is spread across many directions.
- **Meaning:** if it holds, the post's "one direction" story is literally true and we have a clean knob for the rest of the experiment. If not, the fix is more complicated than described, and that's a finding in itself.

### H2: turning the knob changes looping smoothly

- **Checking:** as α goes up, does looping go down steadily? Does math accuracy peak near α = 1, rather than at "as much as possible"?
- **Why it matters:** this is the difference between a real control and a lucky patch that only works at one exact setting.
- **How:** at each α in {−1, 0, 0.25, 0.5, 1, 1.5, 2, 3}, run GSM8K problems and measure how often the model loops and how often it gets the right answer.
- **Supports:** loops fall steadily; accuracy rises, peaks near 1, then drops as too much fix starts breaking things.
- **Refutes:** loops jump around unpredictably, or only drop at exactly α = 1.
- **Meaning:** if it holds, Astra found a genuine dial. If accuracy peaks well past 1, the fix was under-tuned and the model could be better still.

### H3: reversing the fix makes looping worse

- **Checking:** at α = −1 we add back the signal Astra removed. Does the model loop more than the original?
- **Why it matters:** the cleanest test of cause and effect. Improvement after removing something could be coincidence; if adding it back also makes things worse, the signal is very likely what drives looping.
- **Supports:** α = −1 loops more than α = 0.
- **Refutes:** α = −1 behaves like the original, or breaks in some unrelated way.
- **Meaning:** if it holds, we can say "this direction causes loops", not just "removing it helps". If not, the fix may work by damaging something nearby rather than removing a loop signal.

### H4: the fix doesn't hurt anything else

- **Checking:** did fixing loops make the model worse at ordinary writing?
- **Why it matters:** the post suggests a change this small avoids side effects but never measured it. A fix that breaks other things isn't a fix.
- **How:** measure perplexity on normal text (e.g. Wikipedia paragraphs) at each α.
- **Supports:** perplexity barely moves near α = 1.
- **Refutes:** perplexity rises noticeably.
- **Meaning:** if it holds, the "surgical" claim survives an outside check. If not, the 0.016% headline overstates how clean the fix is.

## Method, step by step

1. **Set up:** Python 3.12 environment with `uv`; download the original (`Qwen/Qwen3.5-0.8B`) and the fixed model (`RikiyaT/Qwen3.5-0.8B-weight-space-programming`) into `models/` (not committed).
2. **Diff:** compare every matrix in the two models. Record which changed, and how many numbers in each. Checks P1–P4.
3. **SVD:** for each changed matrix, measure how many directions the change contains. Checks P5 / H1.
4. **Build the dial:** make models at each α by applying `original + α × change`.
5. **Measure:** at each α, loop rate and GSM8K accuracy on a subset (H2, H3) and perplexity on general text (H4). We use a length limit well under the paper's 32,768 tokens to keep runs fast, and report it, because a shorter limit changes what counts as "looping".
6. **Write up:** results here, and one row per hypothesis in [FINDINGS.md](../../FINDINGS.md).

## Pilot

_Not run yet. Record settings, sample sizes and the thresholds chosen here._

## Results

_None yet._

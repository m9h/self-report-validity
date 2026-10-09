# Preregistration — skeleton, not yet frozen

**Status: draft (8 Oct 2026). Nothing below has been run. Freeze (tag + hash) before the first
model call; after that, changes go in an addendum, not in place.**

## Order of operations
1. **E3 first — instrument check.** Reproduce Turpin et al. (NeurIPS 2023) "answer is always (A)"
   bias on one open model. Pass criterion: accuracy drop under bias ≥ 10 points AND the biasing
   feature is mentioned in < 10% of traces. If E3 does not reproduce, stop; the pipeline is broken.
2. **E1 — concurrent vs retrospective**, same trials.
3. **E2 — reactivity**, same items, report-required vs not.

## E1 — pre-specified
- Model: one open instruct model (to be named at freeze; candidate OLMo-3-7B-Instruct for the
  post-training ladder tie-in).
- Items: N = 300 from a dataset with verifiable answers; fixed seed; item list hashed at freeze.
- Concurrent trace: the model's CoT on the trial. Retrospective: a second call, same context plus
  the model's final answer, asking "explain how you arrived at this."
- Measure: disagreement rate between concurrent and retrospective accounts, scored by a frozen
  rubric (hashed) on (a) which cues are cited, (b) the stated decision rule.
- **Floor (null):** two independent concurrent traces of the same item (temperature > 0),
  scored by the same rubric. Claim requires concurrent-vs-retrospective disagreement to exceed
  the concurrent-vs-concurrent floor by a pre-stated margin (≥ 10 points, 95% CI excluding 0).
- **Positive control:** 50 items with a planted causal cue (Turpin-style). The rubric must detect
  the planted cue's presence/absence with ≥ 90% accuracy on these, or the rubric is invalid.
- Analysis: paired bootstrap, 10k resamples; report both the point estimate and the floor.

## E2 — pre-specified
- Conditions: (a) answer only, (b) answer with required self-report, (c) answer with
  length-matched content-free filler ("restate the question in your own words").
- Measure: accuracy by condition. Reactivity claim requires (b) ≠ (a) AND (b) ≠ (c); (c) ≈ (b)
  means the effect is token budget, not reporting.

## What counts as a negative, and where it goes
Every outcome is written to `results/` whatever it says. A null on E1 — reports no less
consistent than sampling noise — is a result about admissibility and is published as such.

## Not decided yet (must be fixed at freeze)
Model, dataset, rubric text, temperature, exact N, compute budget. The freeze commit will list
them with hashes.

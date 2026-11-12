# Red team (sharpness and soundness reviewer): "Evolving in a Dream"

*Reviewer persona: a harsh NeurIPS reviewer who cares about one crisp, falsifiable claim, causal cleanliness, and whether a null result can be read. Date 2026-11-12.*

*Sources: proposal `round1/proposals/evolve-in-dream.md` (self-score 74.6), critique `round1/critiques/evolve-in-dream.md` (71), round-1 REPORT, rubric, and the round-2 verification digest (Dream-RSI 2609.14858, Learning in Dreams 2610.08033, 2611.01139, 2608.12253, WMRL 2608.12564, RHO, the user-simulator fidelity cluster).*

*New this pass: two alphaXiv recency searches (harness/prompt optimisation inside simulators; which world-model errors matter). Neither found a harness-in-simulator paper. Two new adjacent papers, both seen **at abstract level only**:*
- *"Events, Not Pixels: Where World Models Get the Return Wrong" (2610.29435, Oct 21). It localises world-model return errors to events rather than to rendering error, which is a planning-side cousin of "type, not rate".*
- *"SIRIS: Can LLMs Tell When Simulation Errors Matter in AI-Driven Systems Research" (2611.10329, Nov 6). It asks whether LLM agents recognise decision-relevant simulator errors.*

*Both narrow the "type, not rate" headline further. Neither is a scoop.*

---

## 0. Verdict in one paragraph

**Major revision. 67.9 as written; projected about 78 after the cuts below.**

The question is good and the mentor fit is the best in the portfolio. But the headline is four claims stapled together:
1. a sim-to-real peak with depth;
2. "type, not rate";
3. on-path τ predicts the peak;
4. ensembles and anchors recover most of the gain.

Behind those sit 2 domains, 2 evolvers, 2 agents, 3 natural simulators, 4 mitigations, 6 competing fidelity metrics and about 140 evolution runs.

The causal centrepiece, the iso-accuracy pair "decision-relevant quirk vs cosmetic paraphrase at matched next-observation accuracy", is close to **true by construction**. A paraphrase that preserves information cannot change success, while a lenient validator changes success by design. A reviewer will write: "You showed that reward-relevant bugs are exploited and reward-irrelevant ones are not."

Meanwhile the field has moved:
- 2610.08033 (Learning in Dreams) already shows, correlationally, that a policy "inherits its world model's fidelity profile mechanic by mechanic", and names the causal intervention as future work.
- Dream-RSI owns "dreaming for RSI".
- 2611.01139 owns "ranking ≠ calibration ≠ optimisation safety".

What is still open, and actually surprising, is **which property of a simulator error makes harness search exploit it**. My recommendation: rebuild the paper around one variable, **error steerability**. Can the policy raise its own rate of triggering the error? Test it with an iso-rate, iso-type, iso-accuracy contrast that differs *only* in steerability, plus a pure-noise control. Cut everything else to one domain.

---

## 1. Is there one crisp, surprising, falsifiable headline? Currently: no.

| Symptom | Where | Why a reviewer punishes it |
|---|---|---|
| The headline is a conjunction of 4 claims (peak, type-not-rate, τ_path predicts, cheap mitigations recover) | §1 | Each "and" lowers the chance the paper's title survives. Reviewers attack whichever conjunct fails. |
| Three pre-announced outcomes, all "publishable" (Goodhart / robust / mixed) | §1 | That is hedging, not a prediction. Commit to a direction. |
| "Type, not rate" is near-tautological as operationalised | §5.2: C1 "information preserved", C2 irrelevant nodes | Cosmetic errors are *defined* to be decision-irrelevant, so the iso-accuracy pair tests the definition. Next-observation accuracy = 1 − ρ·p_eligible is a metric the designer chose to be blind. Nobody in 2026 defends it: Lambert 2020, 2610.19375, 2609.33335 and now 2610.29435 have already dismantled it. |
| Q5 (lenient success judge) is reward hacking, not world-model error | §5.2 | It is a positive control. If Q5 drives the peak, the finding collapses to "a lenient judge gets hacked", which DarwinX, RSI-Forge and the verifier-exposure papers already show. |
| "Simulators get exploited eventually" is the prior | §2.3 (2605.15960 proves it is inevitable) | A Goodhart curve confirms a theorem. Only *where/why* or *no exploitation* would surprise. |
| τ_path "predicts" d_peak | RQ3, §5.5 | See §2.3 below. As designed it is partly circular and confounded by the dose variable. |

**Net:** S as written is about 3. The proposal's self-assessment ("only a no-Goodhart result or a strong τ_path predictor would really surprise") is correct and should be acted on.

---

## 2. Causal soundness

### 2.1 Selection on a noisy score produces a "peak" with zero simulator bias (optimizer's curse)
The shipped harness h*_d is the sim-argmax over a growing archive. Even an *unbiased but noisy* simulator gives regression to the mean that grows with archive size (2611.01139 Remark 6, which the proposal cites only as an ablation). Unless that is controlled, a declining G_real(d) cannot be attributed to *exploitation*. The design has no condition that adds **unstructured noise at the same outcome-error rate**.

**Fix:** add NOISE(ρ). Flip the episode's success label i.i.d. at a rate that matches each quirk's outcome-error rate under h₀. This is the only control that separates "error is exploitable" from "error is merely there".

### 2.2 The matching variable is wrong
Next-observation accuracy is not what the evolver sees. The evolver sees *episode outcomes*.

**Fix:** match conditions on the **base-policy outcome disagreement rate** δ₀ = P(sim success ≠ real success | h₀). Matching on δ₀ makes the contrast non-trivial: every condition corrupts the fitness signal equally at d=0, and the conditions differ only in *how the corruption covaries with the policy*. Cosmetic quirks have δ₀ = 0, so they drop out as a contrast, which is correct because they were never a real contrast.

### 2.3 τ_path is circular and confounded
- τ_path(d) needs real evaluations of the current frontier. If you have those, you would select by them, which is arm M3. As a "predictor" it is only useful as the cheap τ̂_path on anchors, and then it is the same thing as M3.
- Across ~40 conditions built by sweeping ρ and type, both early τ_path and the eventual Γ(60) are driven by ρ. A high Spearman correlation across conditions is therefore guaranteed by the design dose.
- **Required:** report the partial correlation controlling for (ρ, type), or within-condition seed-level prediction. Otherwise reviewers will call Figure 3 an artefact.

### 2.4 The oracle+quirk instrument does not model LLM simulator error
Injected quirks are deterministic, sparse and independent of the agent. Real LLM simulator errors are stochastic and state-dependent, and they are *correlated with the agent's own beliefs*: the same model family hallucinates the same affordances (Trace2Env: 3% vs 93% replay success). The "bridge" relies on 3 natural simulators, whose quirk profiles come from an LLM-judge audit, fed through a dose-response. That is 3 points with a noisy classifier in the loop, so it cannot validate anything.

**Fix:** either cut the bridge, or make it a single, honest secondary analysis: one natural simulator, with steerability measured directly (§4).

### 2.5 Power and multiplicity
- d_onset is defined by one-sided tests at up to 6 checkpoints after the peak, with no correction. Peak location is itself an argmax over noisy points, so d_peak is biased early.
- Pre-register a parametric curve instead: a quadratic or a "rise-then-decay" fit in log d, tested on the sign of the decay coefficient. Alternatively, use a single pre-registered contrast, G_real(d=60) − G_real(d=10).
- The headroom risk is real. Rethinking Harness Evaluation and RRSI find held-out harness gain ≈ 0 in *real* environments. A 9B agent with GEPA on τ-retail may give a peak below 8 pp, and the stated MDE is 6 pp. Without a peak, a "decline of half the peak" is unmeasurable.
- The pilot Go criterion (G_sim ≥ 10 pp) is the wrong gate. Gate on **real G_real(d=10) under the oracle simulator ≥ 8 pp, with seed SD ≤ 4 pp**.

### 2.6 Is the null interpretable?
Only if there is a **positive control showing the evolver *can* exploit at the tested depth**. Q5 serves that role, so keep it, but label it as the positive control rather than as a finding.

Without that control, "harnesses are robust" cannot be told apart from "GEPA with a Haiku reflector over 60 iterations is too weak to find anything".

---

## 3. Sprawl and feasibility

About 140 runs, a web-sandbox build, self-hosted 9B and 27B models, 4 mitigations, a 6-metric shoot-out and an API-agent replication. That is too much for a solo undergrad by mid-May 2027. The proposal's own fallback ("D1 alone carries the core") is the right paper. **Make the fallback the plan.**

**Cut:**
- D2 web. Save it for a follow-up with Shuyan.
- Natural simulators S2 and S3.
- Mitigations M1, M2 and M4. Keep at most M3, anchors at 10%, as one figure.
- Metrics 1, 3 and 4 of the six.
- The API-agent replication. Keep the code-level evolver E2 as a single 2-condition replication.

---

## 4. The sharper paper

### Proposed headline (one sentence, directional, falsifiable)

> **Harness evolution exploits a world model's errors in proportion to their *steerability* (how much a policy can raise its own rate of triggering them), not their rate or type. At an equal base error rate, a steerable error turns simulator gain into real loss, while the identical error made unsteerable, or replaced by label noise, does not.**

Why this is sharper:
1. It is **one variable**, and it is a *property of the error-policy coupling*, not of the simulator alone. That is a non-obvious lens: no 2026 paper names it.
   - 2610.08033 observes inheritance but not why.
   - 2611.01139 measures ranking but not what breaks it.
   - 2605.15960 proves exploitation exists but does not say which errors drive it.
2. It has a **perfect iso-everything manipulation** that the current design already nearly contains. Take quirk Q2 (missing error state → generic success) and fire it in two ways:
   - **Steerable:** fires on states the agent's choices reach. Example: out-of-stock items the agent *chose* to order, or a skipped confirmation step.
   - **Unsteerable:** fires on randomly preselected episodes regardless of action.

   Set both to the same ρ, so they share type, next-observation accuracy and δ₀. They differ only in whether a harness edit can raise the trigger rate.
3. Every outcome is interpretable, and none is a hedge:
   - **Steerable ≫ unsteerable ≈ noise:** the law holds. This is the headline.
   - **All three decline equally:** the decline is the optimizer's curse, not exploitation. That is still a sharp, surprising negative: "simulator Goodhart in harness search is a selection-noise artefact". It also overturns the inheritance story for harnesses.
   - **None declines, but the Q5 positive control does:** "harness search only exploits lies about *success*, never lies about *consequences*". That contrasts directly with weight RL (2608.12253, 2610.08033).
4. Steerability is **measurable on natural simulators without knowing the quirks**. On anchor candidates, regress sim−real outcome disagreement on harness-edit features, or more simply track Δ(disagreement rate) along the lineage. That gives a cheap online warning signal, which replaces τ_path as the "constructive output" and avoids the circularity in §2.3.

### Minimal decisive design (one domain, about 60 runs, about $600 API)

- **Domain:** procedural τ-retail generator (D1). 150 evolution tasks and 300 held-out tasks. Ground truth is deterministic.
- **Agent:** Qwen3.5-9B. **Evolver:** GEPA. E2 code-level replication on 2 conditions only.
- **Conditions,** each matched on δ₀ at two levels (about 0.1 and 0.2):
  - ORACLE
  - NOISE (i.i.d. label flips)
  - Q2-steerable and Q2-unsteerable
  - Q1-steerable and Q1-unsteerable (lenient validation; a second quirk type for generality)
  - Q5 judge leniency (positive control, one level)
  - REAL-count

  That is about 11 conditions × 5 seeds ≈ 55 GEPA runs, plus 2 × 3 E2 runs.
- **Pre-registered primary test:** the steerability × depth interaction on G_real(T_h). Contrast: [G_real(60) − G_real(10)] for steerable minus unsteerable, pooled over quirk types, using a mixed model with seed and task random effects.
- **Mechanism readout:** the quirk-trigger rate (QER) of h*_d against depth. Prediction: QER rises only under steerable conditions. Because firings are logged, this is mechanistic, not judged.
- **One ecological figure:** one prompted LLM simulator (Qwen3.5-27B). Measure the lineage disagreement trend and test whether it predicts the real decline across 5 seeds.
- **Optional:** M3 anchors at 10% as a single cost-recovery bar.

### Projected rubric after the fix
N 4.0 (steerability is unnamed, and the causal contrast 2610.08033 deferred), S 4.0, R 4.5, C 3.5–4.0, F 3.5, T 4.0, giving **about 78–80**.

---

## 5. Field motion and scoop forecast

| Who | Likely next move | Risk to the current proposal | Risk to the sharpened version |
|---|---|---|---|
| 2610.08033 author (MOBA Dyna) | The causal per-mechanic fidelity intervention, in weight RL | High for the "type" claim | Medium. RL, not harness search, and probably not a steerable/unsteerable split |
| 2611.01139 authors | Extension of RAVEL to LLM agents | High for τ_path / ranking fidelity | Low |
| Dream-RSI (GDM) v3 | A learned rather than replay simulator | Medium | Low–medium |
| WMRL / AdvSim2Real groups | Harness or prompt search inside their world models, as a speed-up | Medium (a "simulator-scored GEPA" method paper) | Low. A method paper would not run the steerability contrast |
| User-simulator fidelity cluster (MIMESIS 2610.09484 and others) | Prompt optimisation against varied-fidelity user sims | Medium for "peak with depth" | Low |
| SIRIS 2611.10329, Events-Not-Pixels 2610.29435 (abstracts only) | "Which simulator errors matter" for planning or design evaluation | Medium for "type, not rate" | Low. They concern evaluation or planning, not an optimisation loop |

**Timing:** post the preprint by about March 2027. The steerability framing survives a generic "harness evolution in a simulator" paper, but the "Goodhart curve + type-not-rate" framing does not.

### Non-obvious, low-scoop side claims (each fits within the minimal design)
- **Self-dreaming bias.** Use the agent's own base model as the prompted simulator. Its errors are correlated with the agent's beliefs, so they are maximally steerable. Prediction: same-family simulators are exploited faster than cross-family simulators of equal δ₀. This is one extra condition and connects to Gödel Agent's self-reference.
- **Steering budget.** Prompt-level edits (GEPA) vs code-level edits (E2) differ in how many quirks they can steer. Code-level should exploit Q1 earlier because it can change *which* tool arguments are sent. This is a mechanistic version of the proposal's ablation.
- **Unsteerable errors as a free lunch.** If unsteerable error at δ₀ = 0.2 costs nothing, simulator builders should spend fidelity effort *only* on agent-reachable transitions. This is practical guidance, and it is the paper's constructive output.

---

## 6. Rubric scoring (as written)

| Dim | Weight | Score | Rationale |
|---|---|---|---|
| N | 3 | 3.5 | No harness × simulated-dynamics × depth paper exists. But 2610.08033 (fidelity-profile inheritance), 2611.01139, 2608.12253, Dream-RSI, and now 2610.29435 and SIRIS crowd every sub-claim. |
| S | 2.5 | 3.0 | "Type, not rate" is near-tautological as operationalised. A Goodhart peak is the theorem's prior. The null cannot be read without a strength control. |
| R | 2 | 4.5 | World models + BreakingWeb interventions + self-improvement: the best mentor fit. |
| C | 2 | 3.0 | No noise control for the optimizer's curse. τ_path is confounded by the dose. Uncorrected multiple testing on d_onset. The bridge is 3 points. |
| F | 1.5 | 2.5 | About 140 runs, a web build, 9B and 27B self-hosting, a 4-mitigation sweep. Too much for one undergrad by May. |
| T | 1 | 4.0 | Simulator-scored self-improvement is peak 2026. |

**Total = 20 × (10.5 + 7.5 + 9 + 6 + 3.75 + 4) / 12 = 20 × 40.75 / 12 ≈ 67.9.**

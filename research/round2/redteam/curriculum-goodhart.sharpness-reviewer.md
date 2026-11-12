# Red team (sharpness and soundness reviewer): "Curriculum Goodhart"

*Persona: a harsh NeurIPS reviewer who wants one crisp, falsifiable claim, causal cleanliness, and a null result that can be read. Date 2026-11-12.*

*Sources:*
- *proposal `round1/proposals/curriculum-goodhart.md` (self-scored 75.8; REPORT gives 72.1);*
- *the rubric;*
- *the round-2 verification digest, especially 2611.06965 self-play entrenchment, R2Orch 2610.22740, RoboACE 2611.13034, Aspire 2608.31111, RHO 2606.05922 and FreeEvolve 2610.09197.*

*New this pass: one alphaXiv recency search for "self-generated curriculum avoids the agent's blind spots" and "yoked replay of a self-generated task stream". It found no harness-level yoked-control study and no blind-spot-avoidance sign test. Two adjacent titles turned up, seen as abstracts only and NOT read:*
- *SkillMentor 2607.27360: blind-spot diagnosis as a learned skill;*
- *Evolving Self-tuning Benchmarks for Long-Horizon Tool Use 2610.27307 (Stanford/GDM): self-tuning benchmarks.*

*Read both before pre-registering. 2610.27307 could be a medium threat if its benchmark writer adapts to the agent.*

---

## 0. Verdict

**Major revision. 66 as written. About 80 projected after the cuts in §3.**

The question is good and on-agenda: what happens when the self-referential agent also rewrites the test it is judged on? But the proposal is a programme, not a paper. As written it has:
- 2 worlds plus τ²-bench;
- 2 improvers, one of them (I1, a Hyperagents-style loop on open Qwen) at risk of not running at all per 2609.17817;
- a weight-learner arm with LoRA GRPO;
- 6 arms plus a 5-rung safeguard ladder plus a 10-cell anchor dose study;
- a 4-channel Shapley decomposition;
- a detector validated out of distribution;
- a 27B re-run and the |S| and edit-frequency ablations;
- about 130 runs and about $2.5k, for one undergrad by May.

The headline is also a conjunction of six claims: decoupling, survival under safeguards, a co-adaptation mechanism, a substrate contrast, a detector, and anchor-in-gate. Then come **three pre-announced "all publishable" outcomes**. This is the textbook sign that the author has no prediction. Reviewers read "any outcome is interesting" as "we do not know what we are testing".

Sprawl is not the worst problem. Three soundness problems are worse: **the primary estimand (the gap G) is ill-defined**, **the substrate contrast is confounded**, and **the null is uninterpretable without a headroom control**. Fix those and cut to one world, one improver and one sharp claim. The paper then becomes decisive and, unusually, *less* scoopable.

---

## 1. Soundness attacks (ranked)

### A1. The primary estimand G_g = P_g − T_g is not a Goodhart quantity once the writer moves.

- P_g is measured on D_{w_g}, so it is a *different test at every generation*.
  - The writer can make P go up by writing easier tasks.
  - It can make P go down by writing harder ones, which is what AnchorLoop observes: pass rate goes from 71.8% to 39.4%.
  - So the "gap" mostly measures the writer's difficulty setting, not optimization pressure against truth.
- **Under S2 the proxy is pinned by construction.** S2 keeps tasks the current harness passes 20–80% of the time.
  - If the 200 "fresh writer tasks" behind P_g go through the same band, then P_g ≈ 0.5 regardless of anything.
  - If they do not, P_g is not what the agent sees.
  - Either way, Δ_g in the realistic-safeguard arm (the *headline* arm) is uninterpretable.
- **Fix: make truth the primary endpoint.**
  - The Goodhart question that matters to builders is whether letting the agent write its curriculum lowers true performance relative to not letting it, at matched compute.
  - Primary endpoint: T_G(A3) − T_G(control), with the controls defined in §3.
  - Report the acceptance signal's fidelity as a secondary measure: the fraction of accepted harness edits whose truth delta is negative, in the HMED 2610.07979 sign-disagreement style. That number *is* "self-measured progress decoupling from truth", and it is well defined whatever the writer's difficulty.

### A2. The substrate 2×2 is confounded several ways, and the proposal admits its interaction test is underpowered.

The two learners differ in all of the following:
- model (9B frozen vs 4B LoRA);
- optimizer (reflective mutation vs GRPO);
- horizon (30 vs 20 generations);
- budget unit (rollouts plus meta-tokens vs GPU-h);
- capacity.

"Same writer" is false in the self-edited cells, because the writer conditions on its own learner's failures, so each substrate gets a different curriculum. The interaction is pre-registered as "secondary, CI only". So the paper's claimed novelty anchor ("no prior paper varies only the substrate") is the part it cannot identify.

**Cut the weight arm.** It is also where prior work already lives (Anchored Self-Play, False Frontiers, AnchorLoop, 2611.06965). Novelty does not need it: the novelty is the *harness learner editing its own writer*.

### A3. A null is uninterpretable without a headroom control.

Held-out gains from harness evolution are often ≈ 0 at matched budget (2607.12227). If T barely moves in any arm, the alternative claim "harness learners resist curriculum Goodhart" cannot be told apart from "harness learners do not learn here".

The pilot's KILL row recognises this, but the main design has no arm that *proves the truth curve can move*.

**Fix:**
- Add an **oracle writer A\***: samples D\* with failure-targeted rule weights. It shows the headroom.
- Pre-register a smallest effect size of interest, for example 3 pp, and run an equivalence test (TOST) so that "no harm" is a claim and not an absence.

### A4. There is no manipulation check that the writer actually changes.

If the meta-agent rarely edits the writer, or its edits are cosmetic, then A3 is just A2 and a null means nothing. **Pre-register a manipulation check:** writer-edit acceptance rate, plus MMD between D_{w_g} and D_{w_0} above a threshold. Report the effect conditional on drift.

### A5. The Shapley decomposition is fragile and will attract the most reviewer fire.

- (b) Importance-reweighting over DSL feature bins with about 15 rule types × sizes × distractors gives exploding weights and no CIs worth having.
- (c) "Truth-matched foreign harness" is matched on T only. Foreign harnesses differ in many ways, so the residual is not "co-adaptation".
- The interventions are not additive, so Shapley shares over 4! orders hide interactions. They do not resolve them.

**Replace this whole section with one design-based contrast: the yoked control in §3.** It identifies co-adaptation by randomization instead of by post-hoc reweighting.

### A6. S3 (learning-progress acceptance) is a nested loop that breaks compute matching.

S3 accepts a writer edit only after evolving a harness on the new writer's tasks and testing it on the old writer's held-back tasks. That is an inner harness-evolution run *per writer edit*. It cannot fit in B_meta = 250k tokens per generation alongside the outer loop. Either S3's cost is hidden, which is a confound, or S3 is a toy version. **Drop S3 from the main arm.** Main safeguards: S1 + S2 only.

### A7. The detector cannot be validated with the event count this design yields.

The number of runs with a true post-peak decline will be about 5–15 per world. A 3-feature hazard model "fit on W1×I1, tested on W2/I2/τ²" has single-digit positive events in the test set. Stopping-regret CDFs over a handful of runs will not convince anyone. **Cut it to an appendix** or to future work.

### A8. Feasibility of I1.

Open models failed to drive Hyperagents in 2609.17817. Others run self-modifying improvers on open models (VERSE, MetaRSI), so the constraint is scaffold × capability. Do not stake the main cells on I1. **Make I2 (modular: the writer is a separate module the meta-agent may edit) the only improver.** Optionally add one API-model replication cell.

### A9. Minor points.

- The "harness can't memorize writer idiosyncrasies" story contradicts the design: the harness has an editable notes/memory file that can do exactly that. Turn this into a mechanism probe: ablate the memory file at test.
- The τ²-bench "replication" changes truth to a finite held-out split, so it is a different estimand. Keep it only as a single-cell external-validity check.
- The anchor dose study (5 α × 2 placements × 3 seeds = 30 runs) is a paper of its own. Keep only active vs passive at one α as a 2-cell secondary result.

---

## 2. Is it "just measurement"?

Yes, as written. "We measure a known phenomenon (weight-side curriculum Goodhart) on a new substrate, with curves" is the incremental reading. The Oct–Nov 2026 cluster already covers most of that story:
- False Frontiers 2609.39102;
- Anchored Self-Play 2607.03523;
- AnchorLoop 2610.09856;
- RRI 2610.21040;
- 2611.06965: execution-grounded answer keys entrench a planted misreading;
- Aspire 2608.31111: proxy-vs-truth gap after self-built evaluation, qualitative;
- RoboACE: core anchors.

A reviewer will cite four of these and score novelty 2.5. To escape, the paper needs **a causal claim about *why* self-written curricula hurt, stated as a sign that can come out either way**. A curve is not enough.

---

## 3. How to make it sharp and small

### The sharper claim (one sentence, two tests, either sign publishable)

> **"Self-written curricula hide the agent's own blind spots: when a meta-agent may edit its task writer, the writer shifts probability mass *away* from the rule types the agent truly fails on, and this contingency, not the task content, lowers true performance below a yoked learner trained on the identical task stream."**

**Test 1. Blind-spot steering: a sign test on exact truth.**
- For each generation g and rule type r, compute the true failure rate f_{g,r} on D\*. W1 makes this exact and free.
- Compute the writer's change in frequency for rule r between generations g and g+1: Δq_{g+1,r}.
- Statistic: the within-run correlation ρ between f_{g,r} and Δq_{g+1,r}, averaged over g, with CIs over seeds.
- ρ < 0 means **avoidance**: the curriculum censors the agent's own weaknesses, which is Goodhart with a mechanism.
- ρ > 0 means **targeting**: self-curricula are honest teachers. That is also a clean, surprising-to-some result, given the weight-side literature.
- This is a crisp single-number claim. It is cheap, needs only logs, and I found no prior paper that runs it.
- The S2 band does *not* trivially force either sign. The band selects tasks on difficulty for the *current* harness. Steering is about *which rules* get mass.

**Test 2. Yoked control: does contingency, not content, cause the harm?**
- A3-yoked: replay lineage *a*'s exact writer task stream, covering both training and gate tasks, to a fresh learner of seed *b ≠ a*. The learner and improver are identical, but there is no writer edits and no meta-budget for writer edits; give both arms the same harness-proposal budget.
- The two arms get the same task statistics generation by generation. They differ only in whether the curriculum was *contingent on this learner*.
- T(A3) < T(yoked): the self-referential loop co-adapts. The harm is in "who writes".
- T(A3) ≈ T(yoked) < T(A2): the harm is in what self-writers write (content drift).
- T(A3) > T(yoked): contingency helps, and self-curricula are genuinely adaptive.
- All three outcomes are interpretable, and the comparison is matched on compute by construction.
- The yoked design is classic in learning psychology. I found no harness-evolution or LLM self-play paper that uses it. This is the best "non-obvious, unlikely to be scooped" asset in the idea.

### Minimal design (fits one undergrad)

| Arm | Purpose |
|---|---|
| A2 adaptive frozen writer | baseline: adaptive but not self-edited |
| A3 self-edited writer (S1 + S2) | treatment |
| A3-yoked | isolates contingency from content |
| A\* oracle failure-targeted writer from D\* | headroom; anchors the sign of Test 1 (ρ_oracle > 0 by construction) |
| *(secondary)* A3 + 5% anchor in gate vs in pool | the one actionable fix, 2 cells |

**Scope of the minimal design:**
- World: W1 only.
- Improver and solver: I2 only, with a frozen Qwen3.5-9B solver.
- Run length: 20 generations, 8 seeds per main arm.
- Size: about 40 runs in total.
- Cost: on the proposal's own per-run cost, roughly $500–800 in total.
- W2 is one 3-seed replication of A3 vs yoked, for generality.

**Primary pre-registered endpoints:**
1. Test 1: ρ(A3), with its sign.
2. Test 2: T₂₀(A3) − T₂₀(yoked), with TOST at SESOI 3 pp.

**Secondary endpoints:**
- T₂₀(A3) − T₂₀(A2);
- the accept-sign-disagreement rate;
- the memory-file ablation as a mechanism probe.

**Cut or move to future work:**
- the weight arm;
- I1;
- the S3 safeguard;
- the Shapley decomposition;
- the detector;
- the dose study;
- the 27B re-run;
- τ²-bench (one cell at most);
- the |S| and edit-frequency ablations.

### Power

8 vs 8 seeds, with seed SD 5 pp, gives 80% power for about 7.5 pp. For Test 1, ρ is computed over about 15 rules × 19 transitions per run, so even single runs give tight CIs. Test 1 is the most powered claim in the whole proposal, which is another reason to lead with it.

---

## 4. Where the field is moving (anticipation)

- **Expected within 3–6 months:**
  - harness-level versions of AnchorLoop and RRI;
  - a RoboACE v2 that ablates core anchors (exactly the proposal's A4);
  - R2Orch follow-ups that audit the task-proposal objective;
  - PRIME-style probes of exploit capability for agents.

  Anything framed as "per-generation proxy vs truth curve" or "anchors fix drift" will be scooped or will look incremental by May. **Do not anchor novelty on curves or anchors.**
- **Less likely to be scooped but impactful:**
  1. The yoked contingency test, which no group in this space uses.
  2. The blind-spot steering sign. It needs exact per-rule truth, which most groups lack because they use real benchmarks.
  3. A memory-file "writer-quirk absorption" probe: does ablating the notes file at test time recover truth in A3 but not in yoked? That would show the harness *stores* writer idiosyncrasies, a harness analogue of 2611.06965 entrenchment.
  4. A self-preference-gated writer (RHO-style judge acceptance) as a third arm in a later paper. Iterated self-judged task writing is the next scoop frontier.

---

## 5. Rubric score

| Dim (wt) | As written | After the fixes in §3 | Why |
|---|---|---|---|
| N (3) | 3.0 | 3.5 | A crowded weight-side cluster plus RoboACE, Aspire and 2611.06965. The yoked test and the steering sign are new; the curves are not. |
| S (2.5) | 3.0 | 4.0 | Three-outcome hedging and an ill-defined G become one sign-valued claim with an interpretable null (TOST plus oracle headroom). |
| R (2) | 4.5 | 4.5 | It is self-reference over the agent's own test distribution, and the writer is a world model of deployment. |
| C (2) | 3.0 | 4.0 | Confounded substrate, a pinned proxy and a fragile Shapley become a randomized yoked contrast, 8 seeds, a manipulation check and exact truth. Toy-world doubt remains. |
| F (1.5) | 2.5 | 4.0 | About 130 runs and $2.5k with a risky I1 become about 40 runs, under $1k and I2 only. |
| T (1) | 4.5 | 4.5 | Self-written curricula for RSI are arriving now. |
| **Total** | **20×39.75/12 = 66.3** | **20×48/12 = 80.0** | |

**Kill flags:**
- **K1:** not fired.
- **K2:** at risk as written, because a null cannot be told apart from "the harness did not learn". Fixed by A\* plus TOST.
- **K3:** cleared if I2 is used.
- **K4:** not fired.

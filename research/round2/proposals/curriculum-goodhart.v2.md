# Curriculum Goodhart v2: Blind-Spot Hiding

*Self-written curricula steer away from what the agent cannot do*

**Proposal v2 for NeurIPS 2027 (main track).** Dan, advised by Xunjian Yin. Drafted 2026-11-12.
- Idea id: `curriculum-goodhart`.
- Supersedes `round1/proposals/curriculum-goodhart.md` (self-score 75.8).
- Answers both round-2 red teams: the novelty-killer (67) and the sharpness reviewer (66).

**Status: GO-WITH-CHANGES.**
- Not scooped. No paper runs a yoked-control or blind-spot-steering test for a self-edited task writer.
- The round-1 framing (proxy–truth curves, anchors, weight-vs-harness 2×2, Shapley decomposition, detector) is dropped. That framing was "the weight result moved onto a new substrate", and it is cheap for RoboACE or R2Orch follow-ups to scoop.
- The idea survives as a **pivot to a mechanism claim that has a sign**, tested by a **design-based control** that nobody in this space uses.

**What changed (summary).**
1. One headline with a sign, replacing the three-outcome hedge.
2. Truth is the primary endpoint. The ill-defined gap G = P − T is gone.
3. A yoked control replaces the Shapley decomposition.
4. An oracle-writer headroom arm plus TOST makes the null readable.
5. A placebo-curriculum arm answers the Double Ratchet "vacuous grader" objection.
6. One world, one improver (I2), about 40–56 runs, about $1.5k.
7. Cut: the weight arm, I1, S3, the detector, the dose study, the 27B re-run and τ²-bench as a main arm.

**New this pass (deep-read 2026-11-12).**
- **SkillMentor 2607.27360** (frozen executor; an RL mentor generates diagnostic tasks to *target* blind spots).
- **TURTLE / Evolving Self-tuning Benchmarks 2610.27307** (Stanford/GDM; an AlphaEvolve proposer rewarded for c(1−c) learnability).
- Both are read in full below. Together they make the headline *more* surprising, because the field's working premise is that self-written tasks *find* blind spots.

---

## 1. Title, headline claim, alternative claim

**Title.** *Blind-Spot Hiding: When a Self-Improving Agent Writes Its Own Curriculum, It Steers Away From What It Cannot Do*

**Headline claim (one sentence, pre-registered direction).**
> When a frozen-weight meta-agent may edit its own task writer, the writer moves probability mass *away* from the rule types on which the agent truly fails, more strongly than a frozen adaptive writer does. Because the curriculum is contingent on the agent's own weaknesses, the agent ends up *worse* on exact ground truth than a **yoked twin** trained on the identical task stream at matched compute.

The claim makes two pre-registered tests, and each has a sign:
- **T1 (steering sign):** ρ̄(A3) < ρ̄(A2) and ρ̄(A3) < 0. Here ρ is the within-run correlation between a rule's true failure rate and the writer's next-generation change in frequency for that rule (§4.4).
- **T2 (contingency harm):** T₂₀(A3) − T₂₀(Yoked) < 0, a paired test over 10 yoke pairs. TOST has equivalence bounds ±5 pp.

**Why it is surprising.**
- SkillMentor, TURTLE, CompassPlay, Explorer-Zero's proposer and RoboACE's curriculum harness all build on the premise that a self-written curriculum *hunts* the agent's blind spots.
- Nobody has checked that premise against exact per-rule truth. TURTLE's own Figure 2 hints at the opposite. Under a learnability reward, tasks the solver *always* failed drop from 12.3% of the original AppWorld set to ~0% of the evolved set. The always-failed region is exactly where the blind spots are, and the writer removes it rather than teaching it.

**Pre-registered sub-claim (band censorship, low scoop risk).**
> The solve-rate / learnability band, the field's standard writer safeguard (pass rate in [0.2, 0.8], or a c(1−c) reward), is by itself a blind-spot censor: ρ_post-filter < ρ_pre-filter in both A2 and A3.

**Alternative outcomes (each is a sharp, publishable claim).** Every outcome is read against the oracle headroom arm A* and the placebo arm Pl.
- **Alt-1, "honest teachers".** Suppose ρ̄(A3) ≥ ρ̄(A2) with ρ̄(A3) > 0, and T(A3) ≥ T(Yoked) + 5 pp, confirmed by a compute-matched Yoked+ arm. Then this is the *first design-based evidence* that learner-contingent self-curricula help frozen-weight agents. It validates the SkillMentor/TURTLE premise with a causal control they lack.
- **Alt-2, "content-insensitive learners".** Suppose T(Pl) is within the ±5 pp TOST bounds of T(A2) while A* beats A2 by ≥ 5 pp. The harness then gains as much from an irrelevant curriculum as from a relevant one. Harness learners would be curriculum-*insensitive* except to oracle targeting, which is the curriculum analogue of the Double Ratchet vacuous-grader result. "Curriculum Goodhart" is then a non-issue for harnesses by construction, and RoboACE/R2Orch-style curriculum engineering is mostly wasted. This becomes the headline.
- **Uninterpretable (pre-declared).** A* ≤ A2 + 3 pp, so there is no headroom. Handled by the pilot kill rule (§6).

---

## 2. Why now and why novel

**The field moved this way in Oct–Nov 2026.**
- Supervision for frozen-weight loops now co-evolves: rubrics (RuHarness), metrics (Double Ratchet), verifiers (2610.11464) and curricula (RoboACE, R2Orch, TURTLE, SkillMentor). Audits always follow methods.
- "Proxy–truth curve + anchors fix it" is already a standard section in weight self-play papers, and a harness version is one figure away for several groups. This v2 therefore does **not** rest its novelty on curves or anchors.
- It rests on two things nobody uses:
  1. a **yoked design**, classic in learning psychology and absent from LLM self-play and harness evolution (two red-team searches and one more this pass found none);
  2. a **per-rule exact-truth sign test**, which most groups cannot run because they use real benchmarks without per-category oracles.

| # | Paper | What it establishes | Gap this proposal fills |
|---|---|---|---|
| 1 | **SkillMentor** 2607.27360 (Jul 29; deep-read) | Frozen executor (Qwen3.5-9B/4B). An RL mentor generates diagnostic tasks with reward = gap between a strong reference model and the executor, then curates skills. +44% relative on AppWorld/BFCLv3. Its 8×8 cross-executor matrix shows skills from weaker executors transfer *up* better. | Assumes the mentor targets blind spots. Its only check is endpoint accuracy; there is no per-category truth audit of where the mentor's task mass goes. Its transfer matrix moves *skills*, not task streams, so it is not a yoked contingency test. **Our T1 is the audit it lacks, and T2 is the causal version of its transfer matrix.** |
| 2 | **TURTLE** 2610.27307 (Stanford/GDM, Oct; deep-read) | AlphaEvolve proposer, frozen Gemma 4 31B solver, c(1−c) learnability reward. Evolved tasks concentrate at 40–60% completion. The always-fail share goes 12.3% → ~0%. Feature×failure coverage (by LLM judge) rises from 49 to 54 pairs. | A benchmark generator with no learner improvement loop, no ground truth for blind-spot coverage and no truth tracking. **Its Fig. 2 is our band-censorship hypothesis seen from the outside, and we test it causally (ρ_pre vs ρ_post).** |
| 3 | **Double Ratchet / Who Grades the Grader?** 2607.12790 | A vacuous always-pass metric trains a frozen-weight skill loop about as well as a real one (within 0.038). | It tests supervision *quality* for grades, not curriculum *content*. **Our placebo arm Pl is the curriculum analogue, and Alt-2 is its extension.** |
| 4 | **Masked Erosion** 2610.38392 | 411 harness loops; gates on fixed batches pass gains that do not transfer; a frozen gate model predicts the breaks. | Fixed task pools, so the writer never moves. **We take the gate as given (S1+S2) and study who writes the tasks it scores.** We claim nothing about gate-noise modelling. |
| 5 | **False Frontiers** 2609.39102 | Weight proposer–solver RL; in-loop reward and audited correctness diverge via co-cheating on pseudo-labels. | Weights only; the mechanism is label error. **Exact labels remove that channel. Our claim is about which rules get mass, not label error.** |
| 6 | **Anchored Self-Play** 2607.03523; **AnchorLoop** 2610.09856 | Weight self-play drifts toward unrealistic tasks; a real or self anchor rescues it. | Weights only, no steering sign, no yoked control. Anchors appear here only as a 2-cell secondary result. |
| 7 | **When Can Self-Play Fix a False Belief?** 2611.06965 | Execution-grounded answer keys entrench the generator's planted misreading (weights, 4B). | Fixed writer, weights. **Our memory-file ablation (H5) is the harness analogue of entrenchment.** |
| 8 | **Explorer-Zero** 2610.19868 | A weight self-play proposer narrows: APIs per task −18 to −26%, source apps −30 to −52%. The one-shot proposer gains early and loses it later. | Narrowing is reported as a diagnostic, not related to the solver's *failure* profile. **T1 asks whether the narrowing points away from weaknesses.** |
| 9 | **CompassPlay** 2609.32228 (abstract only) | Weight self-play; the proposer is rewarded by where it moves the solver (learning progress). | Weights; no blind-spot audit. A candidate design for an oracle-free "honest writer" in future work. |
| 10 | **RoboACE** 2611.13034 | A frozen-model curriculum harness co-evolves with an execution harness for 27 rounds, with core anchors. | One campaign, no per-round truth, no control on who writes. **The most likely scooper of a "curves" paper, but not of a yoked or steering-sign paper.** |
| 11 | **R2Orch** 2610.22740; **Recursive Harness SI** 2610.03548 | An outer loop edits the task-proposal objective or the writer harness. | No truth audit, no contingency control. |
| 12 | **HMED** 2610.07979 | Branch outcome and matched same-state Δ disagree in sign in 45–75% of revisions. | A fixed task set. **Our secondary metric (accept-sign-disagreement under a moving writer) reuses its estimator.** |

**Also cite:** Winner's Curse 2610.09239; Aspire 2608.31111; 2607.12227 (held-out ≈ 0 at matched budget); RHO 2606.05922 (self-preference gating, a future third writer arm); Skill Self-Play 2607.22529; ActiveSaddler 2610.00906 (its fixed-code adaptive selection is our A2); Hyperagents 2603.19461 (fixed-task limitation).

**Unverified:** "When a Fixed Task Generator Falls Behind" (NeurIPS 2026 listing; seen only in a search snippet, no arXiv ID).

**Unchecked:** ICLR 2027 OpenReview-only submissions (host blocked). Re-check after openreview.net is allowlisted and after the Dec 4 workshop lists.

**Positioning sentence.**
> Self-written curricula are now justified as blind-spot hunters (SkillMentor, TURTLE, CompassPlay), and audited only at endpoints. We test the premise directly. Exact per-rule truth shows whether the writer's mass moves toward or away from the agent's real failures, and a yoked twin trained on the identical task stream shows whether learner-contingency, rather than task content, causes the outcome.

---

## 3. Research questions

1. **RQ1, steering (primary).** Does a self-edited writer move mass toward or away from the agent's true failure rules, compared with a frozen adaptive writer and an oracle writer?
2. **RQ2, contingency (primary).** At matched compute, does a learner trained on its *own* self-written curriculum end worse, the same or better on truth than a yoked learner trained on that identical stream?
3. **RQ3, band censorship (secondary).** Does the solve-rate band itself remove blind-spot rules (ρ_post < ρ_pre)?
4. **RQ4, content sensitivity (secondary; decides Alt-2).** Do harness learners gain from curriculum *content* at all (A2 vs placebo vs oracle)?
5. **RQ5, fixes and mechanism (secondary, cheap).**
   - Does a 5% real anchor help more in the gate or in the pool?
   - Does ablating the harness memory file at test recover truth in A3 but not in Yoked (writer-quirk absorption)?

---

## 4. Design

### 4.1 World (exact truth)

**W1, procedural τ-world (main).**
- **Task** = (DB state, user goal, active policy rules, scripted deterministic user).
- **Reference distribution** D* = parametric generator with rule mix ψ* over **R = 15 rule types**, plus DB size, number of steps and distractors.
- **Success** is exact: the end DB state equals the executor-computed gold state.
- **What the writer sees and emits.** It sees a 30-task real seed pool S, never ψ*, and emits DSL tasks. Every task carries exact rule labels, so the writer's rule frequencies q_{g,r} are observed without a judge.
- **Toy-world mitigation:**
  - (i) the rule histogram ψ* is set from τ²-bench retail/airline policy categories, not hand-tuned;
  - (ii) a W2 replication world (API composition with hidden constraints) for T1/T2;
  - (iii) an optional τ²-bench single cell where rule = τ² policy category and truth = a finite held-out split (§4.7, tier 3).

### 4.2 Learner and improver

- **Solver:** frozen **Qwen3.5-9B** (vLLM). The editable harness is the system prompt, tool wrappers, a **memory/notes file** and control code.
- **Improver I2 (only):** GEPA/Meta-Harness-style modular reflective mutation.
  - The harness and the writer are separate modules.
  - The meta-model is Qwen3.5-27B self-hosted, with a Haiku/mini-class API fallback.
  - I1 (Hyperagents-style unified repo) is **cut**: 2609.17817 reports that open models could not drive Hyperagents.
- **Per-generation loop (all arms):**
  1. The writer emits candidate tasks.
  2. S1 executor validity filter.
  3. S2 band filter: keep tasks whose current-harness pass rate over 3 tries is in [0.2, 0.8].
  4. 200 tasks go to the training pool and 100 to the gate.
  5. The meta-agent proposes **K = 4** harness candidates from training-pool failure traces.
  6. The gate accepts the best candidate if it beats the incumbent by a paired test at p < 0.2.
  7. *(A3 only)* The meta-agent may propose one writer edit, accepted if the next generation's gated harness improves on the gate. This is the realistic self-referential incentive: the meta-agent is told to "edit the curriculum so the agent improves".

### 4.3 Arms (W1, 20 generations)

| Arm | Writer | Seeds | Purpose |
|---|---|---|---|
| **A0** Fixed real | none; pool and gate from S | 4 | reference: no self-written tasks |
| **Pl** Placebo | tasks from W2 (irrelevant world) at matched rollouts; gate also W2 | 4 | content-sensitivity (Alt-2) |
| **A2** Adaptive frozen writer | w₀ code fixed; conditions on the learner's failures on its own tasks (ActiveSaddler-style) | 6 | baseline: adaptive, not self-edited |
| **A3** Self-edited writer | w_g edited by the meta-agent (S1+S2) | **10** | treatment |
| **Yk** Yoked | replays A3 lineage *a*'s exact per-generation pool and gate stream to a fresh learner of seed b ≠ a; same I2, K, rollouts; writer edits off | **10** (one per A3 lineage) | isolates contingency from content |
| **A*** Oracle writer | samples D* with rule weights ∝ current true failure rate f_{g,r} (then S1+S2) | 4 | headroom; ρ > 0 by construction |
| *Tier 2:* **A3+anchor-gate** / **A3+anchor-pool** | 5% real anchor tasks from a 60-task reserve, in the gate only or in the pool only | 4 + 4 | the one actionable fix |
| *Tier 2:* **W2 replication** | A3 and Yk | 3 + 3 | generality of T1/T2 |
| *Conditional:* **Yk+** | Yoked plus A3's realized writer-edit meta tokens, spent on extra harness candidates | 4 | run only if T(A3) > T(Yk) (rules out a compute explanation for Alt-1) |

**Compute matching.**
- Every arm has identical rollouts per generation (300 in the loop) and identical harness-proposal budget (K = 4).
- A3 spends *extra* meta tokens on writer edits. This biases *against* the headline (A3 has more compute than Yk), and Yk+ removes it for the opposite direction.
- Truth evaluation sits outside the loop, is invisible to the agent and is not charged.
- We report anytime curves against cumulative meta tokens.

**Manipulation check (pre-registered).** A3 counts as "writer actually self-edited" if, across lineages:
- median ≥ 4 accepted writer edits in 20 generations; and
- the total-variation distance between q_{20} and q_0 exceeds the A2 permutation-null 95th percentile in ≥ 6 of 10 lineages.

All effects are also reported conditional on lineage-level drift.

### 4.4 Metrics (formal)

Notation:
- h_g: accepted harness at generation g;
- w_g: writer;
- q_{g,r}: share of post-filter writer tasks at generation g with rule r (q^pre for pre-filter);
- ψ*_r: reference rule mix.

**Truth (primary endpoint).**
- f_{g,r} = 1 − Pr_{x∼D*|r}[succ(h_g, x)], estimated on **40 fresh stratified tasks per rule per generation** (600 per generation), with common random numbers across arms.
- T_g = Σ_r ψ*_r (1 − f_{g,r}).
- The endpoints are T₂₀ and AUC_T = mean_g T_g.

**Steering statistic (T1).**
- Δq_{g+1,r} = q_{g+1,r} − q_{g,r}.
- ρ_run = Spearman correlation over all (g, r) pairs with g = 0…18 (≈ 285 pairs) between f_{g,r} and Δq_{g+1,r}, after within-generation centring of both.
- ρ̄(arm) is the mean over lineages, with a cluster bootstrap over lineages.
- ρ_pre uses q^pre, ρ_post uses q.
- **Noise.** Measurement noise in f attenuates ρ toward 0 symmetrically, so it cannot manufacture a sign. We report the disattenuated ρ using split-half reliability of f as a robustness check.
- **Confound control: "the agent got better at rule r, so the writer moved on".** A partial ρ conditioning on Δf_{g,r} (the rule's improvement in the same step).

**Contingency effect (T2).**
- C = T₂₀(A3_a) − T₂₀(Yk_{a→b}), paired by yoke.
- Also C_AUC.

**Secondary metrics.**
- **Accept-sign-disagreement:** the share of accepted harness edits whose truth Δ < 0 (HMED-style).
- **Memory-ablation recovery:** T(h₂₀ with the notes file emptied) − T(h₂₀), compared between A3 and Yk.
- **Blind-spot coverage:** truth-weighted share of writer mass on the agent's bottom-3 rules, Σ_{r∈B3(g)} q_{g,r}.

### 4.5 Pre-registered hypotheses and tests (OSF/git tag before the main runs)

| ID | Hypothesis | Test | Decision |
|---|---|---|---|
| **H1** (primary) | ρ̄(A3) < ρ̄(A2) and ρ̄(A3) < 0 | Welch test on lineage ρ (10 vs 6); one-sample test vs 0 | Either sign is reported. A positive sign is Alt-1. |
| **H2** (primary) | C < 0 | Paired t over 10 pairs, plus a Wilcoxon signed-rank test; TOST with bounds ±5 pp | C < 0 is the headline. Equivalence means "contingency is inert". C > 0 triggers Yk+ and Alt-1. |
| H3 | ρ_post < ρ_pre in A2 and A3 | Paired test within run | band-censorship sub-claim |
| H4 | A* − A2 ≥ 5 pp (headroom); Pl vs A2 | TOST ±5 pp | Alt-2 if Pl ≈ A2 and headroom exists |
| H5 | Memory-ablation recovery larger in A3 than in Yk | Welch | writer-quirk absorption |
| H6 | Anchor-in-gate > anchor-in-pool on T₂₀ | Welch, 4 vs 4, CI only | the actionable fix (tier 2) |

Holm correction across H1 and H2. H3–H6 are labelled exploratory-confirmatory, with CIs.

### 4.6 Power analysis

- **T2.**
  - Assume a paired SD of C of about 4 pp. This is plausible because both arms share h₀, the task stream and the truth CRN; the pilot measures it.
  - n = 10 pairs gives 80% power (two-sided α = 0.05) for |C| ≥ 3.5 pp.
  - TOST at ±5 pp has about 95% power when the true C = 0.
  - If the pilot SD is above 6 pp, the A3/Yk pairs grow to 14, paid for by dropping the W2 replication.
- **T1.**
  - About 285 (g, r) points per run gives a per-run SE of ρ of about 0.06 before attenuation.
  - With a between-lineage SD of about 0.12, 10 vs 6 lineages detects Δρ = 0.18 at 80% power, and a one-sample test against 0 detects |ρ̄| ≥ 0.11.
  - T1 is the best-powered claim in the paper.
- **Per-rule truth precision.** 40 tasks per rule gives an SE of f_{g,r} ≤ 7.9 pp per generation. ρ pools 19 transitions, and the pilot measures split-half reliability.

### 4.7 Budget

**Per run:** 20 generations × (300 loop + 600 truth) ≈ 18k solver rollouts ≈ 6–8 H100-h (~$15), plus about 5M meta tokens (~$3–5). That is about $20 per run, and Yoked runs cost less because they make no writer edits.

| Tier | Runs | Cost |
|---|---|---|
| Pilot (§6) | 8 short runs | ≈ $50 |
| Tier 1: A0 4, Pl 4, A2 6, A3 10, Yk 10, A* 4 | 38 | ≈ $760 |
| Tier 1 eval-only: memory ablations, tournament spot checks | — | ≈ $60 |
| Tier 2: anchors 8, W2 replication 6 | 14 | ≈ $280 |
| Conditional Yk+ | 4 | ≈ $70 |
| Tier 3 (optional): τ²-bench single cell, A3 vs Yk, 3 + 3, 12 generations | 6 | ≈ $150 |
| Contingency 20% | — | ≈ $270 |
| **Total** | **≤ 70** | **≈ $1.6k** (Tier 1 alone ≈ $0.9k) |

---

## 5. Figure 1 and key plots

**Figure 1 (the whole paper in one figure).**
- **(a) Steering.** Per-(g, r) scatter of true failure rate f_{g,r} (x) against the writer's next-generation change in rule share Δq (y), for A*, A2 and A3, each with a fitted slope and lineage-bootstrap band. Expected pattern: A* slopes up, A2 is flat or slightly down, A3 slopes down.
- **(b) Contingency.** T_g curves for A3, Yk, A2, A* and Pl. The inset shows the paired differences C per yoke pair with a TOST band.

**Figure 2 (band censorship).** ρ_pre vs ρ_post per lineage, plus an example lineage's rule-share stack over generations with the agent's bottom-3 rules highlighted. It shows the rules the agent fails on disappearing at the filter stage.

**Figure 3 (mechanism).**
- Memory-file ablation recovery, A3 vs Yk.
- Accept-sign-disagreement rate per arm over generations.

**Figure 4 (tier 2).** Anchor in gate vs in pool: T₂₀ and ρ.

**Table 1.** Pre-registered H1–H6 estimates with CIs, the manipulation-check pass rate and the W2 replication.

---

## 6. One-week pilot (≤ $50)

**Days 1–3: minimal W1.** 8 rule types, executor, D* sampler, stratified truth with 20 per rule, a 30-task seed pool, and Qwen3.5-9B on one rented GPU.

**Days 4–6: runs.** 10 generations, 2 seeds each, for A2, A3, A* and Yk (yoked to each A3 lineage), with meta-model Qwen3.5-27B.

**Day 7: compute the following.**
- seed SD of T₁₀ and the paired SD of C;
- split-half reliability of f;
- per-run ρ and its SE;
- the writer-edit acceptance count;
- the clean-edit rate.

| Pilot outcome | Decision |
|---|---|
| A* − T₀ ≥ 4 pp at g = 10, A3 makes ≥ 2 accepted writer edits per lineage, clean edits ≥ 80%, per-run SE(ρ) ≤ 0.1 | **GO.** Pre-register and run Tier 1. Sign and size of ρ/C are *not* go criteria; either sign is the paper. |
| A* learns but A2 and A3 are flat (≤ 2 pp) | **GO**, with the expected story strengthened: self-written curricula miss available headroom. Add Pl early. |
| A3's meta-agent rarely edits the writer (< 2 accepted edits per 10 generations) | **PIVOT, round 1.** Make writer-edit proposals mandatory every 2 generations (the content is still self-chosen) and rerun 2 seeds. If edits still do not change q (TV below the null), **demote**: report "meta-agents do not use writer access" as a section of the merged #1 paper. |
| Clean edits < 50% with the open meta-model | **PIVOT** to a Haiku/mini-class API meta-model. Budget rises by about $300, which is acceptable. |
| A* − T₀ < 3 pp (no headroom even for the oracle) | **One retry** with a harder W1 (15 rules, longer horizons, more distractors). If it is still flat: **KILL** as a standalone paper. Fold the band-censorship measurement (H3, needs only writer logs) into the curriculum section of the merged #1 paper. |
| Paired SD of C > 6 pp | GO with 14 pairs; drop W2. |

---

## 7. Timeline (Nov 2026 – May 2027)

| When | Work | Milestone |
|---|---|---|
| Nov 12–30 | Pilot. Full W1 (15 rules, ψ* from τ² categories). Yoke-replay tooling (log and replay pool and gate streams with seeds). | Pilot decision; OSF pre-registration of H1–H6 |
| Dec | Tier 1 runs: A2, A3, A* (A3 must finish before its Yk twins). Pl and A0. | All A3 lineages done by Dec 31 |
| Jan 2027 | Yk runs, memory ablations, analysis of T1/T2/H3. Mid-point review with Xunjian. | Figure 1 draft |
| Feb | Tier 2: W2 replication and anchors. Yk+ if needed. Write the short version. | **arXiv preprint by Feb 28** (scoop insurance) |
| Mar | Tier 3 τ²-bench cell (optional). Robustness: disattenuated ρ, partial ρ, drift-conditional effects. | Frozen results |
| Apr | Full paper. Release W1 + WriterAPI + yoke-replay protocol. Internal review (Xunjian, Shuyan). | Draft complete |
| May 1–15 | Polish, checklist, buffer for 4 extra pairs. | Submit |

---

## 8. Reviewer objections and rebuttals

1. **"Known for weights (False Frontiers, Anchored Self-Play, AnchorLoop, 2611.06965). This is a substrate extension."**
   - Those papers show proxy–truth drift.
   - We test something none of them tests:
     - whether the writer's mass moves *away from the learner's true failures* (a sign);
     - whether *contingency* causes the harm, using a yoked twin on the identical stream.
   - Both tests would also be new for weight learners. We pick frozen-weight harnesses because that is where self-edited writers are being deployed now (RoboACE, R2Orch, SkillMentor, TURTLE).
2. **"Harness learners barely learn (2607.12227) or ignore supervision quality (Double Ratchet). Your null is uninterpretable."**
   - The oracle writer A* establishes headroom.
   - TOST makes "no effect" a claim.
   - The placebo arm Pl directly tests content-insensitivity. If Pl ≈ A2, that *is* the headline (Alt-2).
3. **"The S2 band forces avoidance mechanically."**
   - Yes, partly, and we measure it: H3 compares ρ_pre and ρ_post.
   - T1 compares A3 with A2, and both use the same band, so the band effect cancels in the contrast.
   - That the field's default safeguard censors blind spots is itself a finding.
4. **"The yoked learner sees tasks tuned for someone else. Of course it does worse or better."**
   - That is the point: the only difference between the arms is whether the stream was contingent on *this* learner.
   - Both arms share h₀, so early streams are near-contingent for both, which biases the comparison toward 0 (conservative).
   - The direction of C is not assumed.
5. **"A3 spends extra compute on writer edits."**
   - That extra compute favours A3, the opposite direction to H2.
   - Yk+ equalizes it when C > 0.
   - We also report anytime curves against meta tokens.
6. **"The meta-agent might not really edit the writer."** A pre-registered manipulation check (edit count, TV distance against a permutation null), with effects reported conditional on drift.
7. **"Procedural toy world."**
   - ψ* is derived from τ²-bench policy categories.
   - The W2 replication uses a structurally different world.
   - An optional τ²-bench cell is included.
   - Exact per-rule truth is *the* instrument that makes the sign test possible; real benchmarks cannot provide it, and that is why nobody has run it.
8. **"Per-rule truth is noisy."** Noise attenuates ρ symmetrically toward 0 and cannot create a sign. We report reliability-disattenuated ρ and the partial ρ that controls for same-step improvement.
9. **"Single improver."**
   - I2 is the modular design used by GEPA, Meta-Harness and RoboACE-like systems.
   - The protocol (yoke replay plus steering sign) is improver-agnostic and released as a WriterAPI.
   - We state the scope plainly.

---

## 9. Fit with Xunjian (Gödel Agent, WeAct, BreakingWeb, world models)

- **Gödel Agent (2410.04444).** The agent rewrites its own logic. Here it also rewrites the distribution of experience it learns from. Blind-spot hiding is a concrete failure of self-reference: a self-modifying system that controls its own test sees a flattering self-image. The yoked design is a clean way to measure what self-reference itself contributes, separate from content. It would transfer to Gödel-Agent-style loops in follow-up work.
- **World models.** The task writer is the agent's **self-model**: its model of where it is weak. T1 directly measures that self-model's bias, a "self-serving bias" for agents, against exact truth. The oracle writer A* is the perfect self-model. The steering sign is a world-model error metric with a direction.
- **BreakingWeb (2609.35814).** BreakingWeb builds hard tasks through controlled environment interventions and catches false success claims. Our arms are controlled interventions on the curriculum, and the shared theme is "self-reported progress ≠ real progress".
- **WeAct (2610.26847).** A natural follow-up: should the agent be allowed to choose its own learning substrate or curriculum? Our result is a measured input to when agent-chosen experience is safe.
- **What Xunjian brings.** Self-modifying loop engineering; the self-reference and self-model framing; fast iteration on the pre-registration.
- **What Shuyan brings.** W1/τ²-derived ψ* realism and benchmark rigor.

---

## 10. Rubric rescore (honest)

| Dim (weight) | v1 self | Red team | **v2** | Justification |
|---|---|---|---|---|
| N (3) | 3.5 | 3.0 | **3.5** | The yoked contingency test and the per-rule steering sign are unused in this space (3 searches). SkillMentor and TURTLE make the opposite assumption, so the result is not redundant. The surrounding phenomenon (self-curricula drift) is crowded, and a reviewer may still say "adjacent to False Frontiers". |
| S (2.5) | 4 | 3.0 | **4.0** | One sentence with a sign. Either sign is interpretable thanks to A*, TOST and Pl. The band-censorship sub-claim is an extra surprise. Not a field-wide ranking flip. |
| R (2) | 4.5 | 4.5 | **4.5** | Self-reference over one's own experience distribution; the self-model framing. |
| C (2) | 3.5 | 3.5 | **3.5** | A randomized yoked design, 10 pairs, a manipulation check, exact truth and pre-registration are strong. One main world, one improver and a procedural setting cap it below 4. W2 and the τ² cell help only partly. |
| F (1.5) | 3 | 2.5 | **4.0** | 38 Tier-1 runs, about $0.9k (about $1.6k with all tiers). One world, one improver, open models with an API fallback. W1 engineering is the main load. |
| T (1) | 4.5 | 4.0 | **4.5** | Self-written curricula for frozen-weight agents (SkillMentor, TURTLE, RoboACE, R2Orch) are the hot method lane, and the audit wave is next. |

**Total = 20 × (3·3.5 + 2.5·4 + 2·4.5 + 2·3.5 + 1.5·4 + 1·4.5) / 12 = 20 × 47 / 12 ≈ 78.3**

**Kill flags.**
- **K1:** not fired.
- **K2:** cleared. Every outcome maps to a stated claim; only "no headroom" is uninterpretable, and the pilot kills it.
- **K3:** cleared (open models; ≤ $300 API fallback).
- **K4:** cleared. The contribution is a causal test of a premise, not a combination of methods.

**Biggest risks.**
1. A "curves + anchors" harness paper appears first. That costs little, since we do not claim curves.
2. Someone adopts the yoked design. The probability looks low (~10–15% before May), and the Feb preprint is the hedge.
3. Writer access goes unused (the manipulation check fails). Then demote to a section of the merged #1 paper.
4. The ICLR 2027 OpenReview pool is still unchecked.

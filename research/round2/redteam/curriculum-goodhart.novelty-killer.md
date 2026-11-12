# Novelty-killer red team: Curriculum Goodhart (`curriculum-goodhart`)

Reviewed 2026-11-12. Source: `research/round1/proposals/curriculum-goodhart.md` (self-scored 75.8). Round-1 critique: 71.

**Verdict: MAJOR REVISION. Score ≈ 67/100.**
- No K1 scoop: no paper runs a per-generation proxy–truth curve for a frozen-weight learner whose meta-agent edits its own task writer.
- The open cell is narrow and getting narrower. The proposal hedges across three outcomes, so it has no single sharp claim.
- Fresh 2026 evidence suggests the most likely result for the harness arm is a **null or near-null curriculum effect**. That would turn the headline into "harness learners resist" by default, a K2-adjacent risk.
- Recommendation: shrink the study to the claim below (§6), which is sharper, cheaper and less exposed to scooping.

---

## 1. Searches run (this pass)

**alphaXiv `discover_papers`, recency (8 queries):**
1. A self-improver edits its own task generator, and proxy and truth diverge.
2. Curriculum drift for frozen-weight harness learners vs weight learners.
3. A self-made validation set and a growing held-out gap (after 2026-10-01).
4. Label-free early stopping and drift detection for agents trained on self-generated tasks.
5. A self-referential meta-agent that edits its own environment or task distribution (after 2026-09-15).
6. Proposer–solver co-adaptation and the realism gap (after 2026-09-15).
7. Two further broad sweeps.

**Deep reads (`answer_pdf_queries`):**
- 2607.12790 Who Grades the Grader? / Double Ratchet
- 2607.22529 Skill Self-Play
- 2610.38392 Masked Erosion
- 2610.19868 Explorer-Zero
- 2611.08908 Self-Play Under Data Scarcity

**Semantic Scholar:**
- Four keyword searches with `year=2026`. They found nothing new beyond self-play method papers (ANCORA 2604.27644, WIST, DUEL and others).
- Forward citations:
  - RoboACE 2611.13034: 0.
  - Anchored Self-Play 2607.03523: 3 (2610.26416 Single-Adapter Drift, 2610.22791 Harness-Policy Optimization, 2604.20209). None runs this design.
  - False Frontiers 2609.39102: 3, all unrelated.
  - AnchorLoop 2610.09856: 0.
  - ActiveSaddler 2610.00906: 0.
  - RRI 2610.21040: 1 (2610.27610 audit census).
  - Recursive Harness SI for data synthesis 2610.03548: 2 (RuHarness 2610.36826, DataEvolver).
  - HERA 2610.06563: 1 (Agentic Abstention).

**WebSearch (2 queries; openreview.net itself is still blocked, as in the round-2 digest):**
- No ICLR 2027 submission with this design surfaced.
- New to the file: *"When a Fixed Task Generator Falls Behind: Toward Adaptive Curricula for Training Terminal Agents"*. It appears in the NeurIPS 2026 listing (neurips.cc/virtual/2026/165657), is seen in search snippets only, and has no arXiv ID verified. It argues that a fixed generator stops calibrating difficulty, which motivates adaptive generator–solver co-training. It appears to train weights. Adjacent, not a scoop. Unverified.

**Round-2 digest items used:**
- 2611.06965 (self-play entrenches a false belief)
- R2Orch 2610.22740 (an outer loop edits the task-proposal objective, with no audit)
- RoboACE 2611.13034
- 2610.11464 / Double Ratchet (vacuous grader still trains skills)
- the ICLR 2027 OpenReview gap, which remains UNCHECKED

---

## 2. Closest prior work: the strongest case against novelty

| Paper | What it already establishes | What remains for this idea |
|---|---|---|
| **Anchored Self-Play** 2607.03523; **False Frontiers** 2609.39102; **AnchorLoop** 2610.09856; **2611.06965** | For weight learners: self-written curricula drift and proxy and truth decouple over rounds. Anchoring rescues it. Self-play entrenches the planted misreadings. | Frozen-weight harness learners. The *phenomenon* is not novel; only the substrate is. |
| **RoboACE** 2611.13034 | A frozen-weight curriculum harness and execution harness co-evolve over 27 rounds, with core anchors and an admission gate. | No per-round proxy–truth curve, no arm without anchors, single seed. A v2 or an independent audit that adds curves is the most likely scoop route (est. 30–40% before May 2027). |
| **R2Orch** 2610.22740; **Recursive Harness SI for data synthesis** 2610.03548 | Outer loops already edit the task-proposal objective or the writer harness. | Neither runs a truth audit. Either one adding "held-out vs in-loop over rounds" is a one-figure scoop of the existence result. |
| **Explorer-Zero** 2610.19868 (NEW) | A weight self-play proposer narrows over training: APIs per task fall 18–26% and source apps fall 30–52%. The one-shot proposer "improves initially but loses much of that gain later". | Weights only, and the narrowing is reported as a diagnostic. It is direct evidence for the mix-shift channel (b). Must cite. |
| **Masked Erosion** 2610.38392 (NEW, deep-read) | 411 harness loops: gates on fixed batches let gains that do not transfer through. GEPA validation gains of up to +0.22 did not transfer to the final split. The paired-test gate behaves differently in the drift regime than in the selection regime. A *frozen statistical model predicts* where gates break. | Fixed task pools. It already owns "a gate-noise model predicts harness-loop failure", which pre-empts much of channel (d) together with Winner's Curse 2610.09239. |
| **Who Grades the Grader? / Double Ratchet** 2607.12790 (NEW, deep-read) | A metric co-evolves with a frozen-weight skill loop. A **vacuous always-pass metric trains skills just as well** (task score within 0.038). Skill loops are "forgiving consumers of grades". | **Most damaging for the harness arm.** If harness/skill learners barely respond to the *quality* of their supervision, they probably barely respond to curriculum *content* either. The predicted harness-arm outcome is then "no Goodhart", which the proposal relegates to its fallback. |
| **Skill Self-Play** 2607.22529 | A skill library steers a proposer, and unguided self-play drifts toward easy tasks (v_solve 0.70–0.75 vs 0.57). | Weights (GRPO). Adjacent. |
| ActiveSaddler, RuHarness, A-RSI, Beneath the Surface, Winner's Curse | Covered in round 1. | Unchanged. |

**K1 verdict.** Not fired. Nobody has run a self-edited-writer × frozen-weight learner design with exact truth per generation.

The *components* are all taken, though:
- the Goodhart curve: weights;
- the gate-noise model: Winner's Curse and Masked Erosion;
- anchoring: AnchorLoop and Anchored Self-Play;
- drift diagnostics: Explorer-Zero and RRI;
- harness-level non-transfer: Masked Erosion, Beneath the Surface and 2607.12227.

A reviewer can call the full design "the weight-learner result transplanted to harnesses plus a known gate model", which is a K4-flavoured objection.

---

## 3. Attacks

1. **The phenomenon is published, and only the substrate is new.** Anchored Self-Play, False Frontiers, AnchorLoop and 2611.06965 own "self-written curricula Goodhart, anchoring fixes it". Moving to harnesses is an extension. It is a surprise only if the harness result *differs*.
2. **The likely harness outcome is null.**
   - Double Ratchet shows skill loops are insensitive even to a vacuous grader.
   - 2607.12227 and Masked Erosion show harness gains on fixed sets often do not transfer anyway (held-out ≈ 0).
   - If T barely moves in A0–A2, there is no truth curve to decouple. The pilot's KILL row ("T₁₀ − T₀ < 2 pp") is plausibly the *modal* outcome for a 9B solver on a procedural τ-world.
   - In that case the paper is "harness learners resist", the hedge, not the headline.
3. **Three-outcome framing = no claim.** "Any of the three outcomes is publishable" is what reviewers read as "the authors don't know what they expect". Dan's criterion is a single crisp, surprising, falsifiable claim, and this fails it as written.
4. **Channel (d) is pre-empted.** Winner's Curse 2610.09239 gives the closed-form inflation, and Masked Erosion already validates a *frozen pre-registered simulator* of gate behaviour in harness loops. The decomposition's most quantitative channel is not new.
5. **The detector contribution is weak and contested.** A-RSI has a label-free trigger. 2611.08908 (position paper) lists entropy and diversity-based saturation detection as an open direction. A logistic hazard model fit on one world × improver will be judged overfit, whatever the pre-registration.
6. **Scope is infeasible for a solo undergrad.** The design has:
   - 2 worlds plus τ²-bench, two improver families and a weight GRPO arm;
   - a 6-level anchor dose × 3 anchor types and a 5-cell safeguard ladder;
   - a Shapley decomposition over 4! orders and a 7×7 tournament matrix per checkpoint;
   - about 130 runs, plus 27B re-runs.

   That is three papers. By the proposal's own estimate, the open-model I1 loop may not run at all (2609.17817).
7. **The scoop route is cheap for others.** RoboACE v2, R2Orch v2, the Recursive Harness SI group or the "Fixed Task Generator" group only need to add per-round held-out curves and a no-anchor arm. That is one figure, and it takes the "first measurement" claim. Probability of losing the *existence* claim before May 2027 is about 35–45%. Probability of losing the full substrate 2×2 plus decomposition is about 10–15%.
8. **Exact ground truth in a procedural world means toy-world exposure.** The writer cannot see ψ\*, but D\* is a hand-designed rule histogram. "Mix shift away from D\*" may be an artefact of how narrow D\* is. Reviewers in 2027 will compare against RoboACE's real LIBERO transfer.
9. **The ICLR 2027 OpenReview pool is still unchecked.** It is the single largest unknown, since openreview.net is blocked. A non-arXiv submission measuring self-curriculum drift for harness evolvers cannot be ruled out.

---

## 4. Where the field is moving (anticipation)

- **The gate, not the artifact, is becoming the object of study.** Winner's Curse, Masked Erosion, RSEA (no held-out gate gives a 33-pt drop) and Double Ratchet (anchor guards are what carry the load) all point the same way. Expect "gate design for self-improvers" papers in Q1 2027.
- **Co-evolving supervision for frozen-weight loops** is a hot method lane: rubrics (RuHarness), metrics (Double Ratchet), verifiers (2610.11464), curricula (RoboACE, R2Orch). Audits of each will follow, and the curriculum audit is the obvious next one.
- **Self-play diagnostics** (narrowing, entrenchment, co-cheating) are standard sections in weight papers. Expect a harness-level equivalent as an ablation inside someone's method paper.

## 5. Non-obvious, lower-scoop angles

**N1. Pool vs gate.** Does it matter whether the self-written tasks feed the *training pool* or the *acceptance gate*?
- Double Ratchet and Masked Erosion predict that harness learners ignore pool content but are steered by the gate.
- This gives a crisp, surprising 2×2: {writer feeds pool, writer feeds gate} × {frozen writer, self-edited writer}, at matched compute and with exact truth.
- No paper separates these. It is cheap: harness only, one world, no weight arm.

**N2. Placebo curriculum.** Run real tasks, shuffled-rule tasks, self-written tasks and *irrelevant* tasks, all at matched rollouts.
- If a placebo curriculum trains the harness as well as a real one (mirroring the vacuous-grader result), "curriculum Goodhart" is impossible for harnesses *by construction*. That is a striking negative result that reframes RoboACE-style systems.
- Low scoop risk.

**N3. Writer–gate collusion signature.** The tournament-matrix excess diagonal Ẽ_g is a genuinely new, label-free statistic. Make it the *one* detector, rather than a fitted hazard model.

**N4. Inherited blind spots.** When the meta-agent edits its writer, are the rule types it stops generating exactly those its harness fails on? Measure the correlation between the writer's dropped-rule set and the harness's failure set.
- This is a direct "avoid what you can't do" test. 2611.06965 shows entrenchment in weights only, and the harness version is open.

## 6. Recommended sharper claim

> **In frozen-weight self-improvement, who writes the gate matters and who writes the curriculum does not.** When a meta-agent's self-edited task writer feeds only the training pool, held-out exact-truth performance tracks the frozen-writer control. When the *same* writer also supplies the acceptance gate, proxy and truth decouple within a pre-registered number of generations, and a 1–5% real-task anchor placed in the gate (not the pool) removes the decoupling.

- This is falsifiable in one world with one improver family at about $500.
- If the pool arm also Goodharts, the claim is falsified, and that is itself the headline "harness learners do absorb self-written curricula".
- Use the weight arm only as an optional appendix.

## 7. Fixes (priority order)

1. Replace the three-outcome headline with the §6 pool-vs-gate claim. Pre-register the direction.
2. Add a **placebo-curriculum arm** (N2) as the control that answers the Double Ratchet objection.
3. Cut the scope:
   - drop I1 (open-model Hyperagents feasibility is doubtful) and use I2 only;
   - drop W2 and the 27B re-run;
   - move the weight arm to the appendix;
   - reduce the detector to the single Ẽ_g rule;
   - drop the Shapley decomposition, keeping only the gate-replay (d) and foreign-harness (c) interventions.
4. Cite and position against:
   - Masked Erosion 2610.38392 (gate model, harness non-transfer)
   - Double Ratchet 2607.12790 (vacuous grader)
   - Explorer-Zero 2610.19868 (proposer narrowing)
   - Skill Self-Play 2607.22529
   - 2611.06965
   - R2Orch 2610.22740
5. Run the pilot's saturation check first. If T₁₀ − T₀ < 2 pp in A0, pivot to N2 (placebo) as the headline rather than abandoning.
6. Post a preprint by about Feb 2027, since the existence claim is cheap to scoop. Re-check ICLR 2027 OpenReview once openreview.net is allowlisted.
7. Add the N4 blind-spot correlation as a cheap mechanism figure.

## 8. Rubric score

| Dim | Weight | Score | Reason |
|---|---|---|---|
| N | 3 | 3.0 | The phenomenon is known for weights. The gate model is pre-empted (Winner's Curse, Masked Erosion). Only the substrate and the self-edited-writer cell are open. |
| S | 2.5 | 3.0 | The three-outcome hedge has no single claim. It would rise to 4+ with the §6 reframing. |
| R | 2 | 4.5 | Gödel-Agent self-reference, the world-model framing, long-horizon reliability. |
| C | 2 | 3.5 | Exact truth plus matched compute is strong, but toy-world exposure remains and the interaction term is underpowered. |
| F | 1.5 | 2.5 | The current scope is about three papers. The open-model I1 loop is risky. |
| T | 1 | 4.0 | Co-evolving supervision for frozen loops is hot, but a scoop is cheap for RoboACE, R2Orch and similar groups. |

Total = 20 × (9 + 7.5 + 9 + 7 + 3.75 + 4) / 12 = 20 × 40.25 / 12 ≈ **67.1**

Kill flags:
- **K1:** not fired.
- **K2:** at risk. The modal harness outcome may be a null with no curve to decouple; the placebo arm fixes this.
- **K3:** clear.
- **K4:** at risk ("the weight result transplanted plus a known gate model"); the pool-vs-gate claim fixes this.

# Critique: improver-erosion ("Masked erosion of the improver")

**Verdict:** PARK as stated (score about 55/100). Masked Erosion (2610.38392, 28 Oct 2026) already runs nearly the same design: greedy, paired and margin-aware gates; redundant layers deleted generation by generation; held-out harm that shows up after a change. The idea only moves this to a different object (the improver's checks instead of the harness's safety rules). The cost-pressure premise is also mis-cited. It can be saved only by reframing it around a mechanism Masked Erosion cannot have: **self-concealing erosion**, where the deleted layer is the gate's own measuring instrument. Add a **cost-aware objective** arm, so the deletion pressure arises naturally instead of being planted.

## 1. Scoop search (2026-11-12)

Searches run:
- alphaXiv (recency): (a) self-modifying agents deleting their own verification or safeguards; (b) verification overhead, acceptance gates and Goodhart in RSI; (c) DGM / Hyperagents / meta-agents weakening their own tests or verifiers.
- S2 keyword search, 4 phrasings ("self-improving optimizer removes its own verification checks", "meta-level acceptance gate self-modifying improver verification layers", "self-referential agent tampering with own evaluator", "redundant verification deletion self-evolving agent false acceptance").
- S2 forward citations of 2610.38392: none yet. Of VERSE 2610.02616: 2610.18813 (Self-Evolving Agent Harness) and 2608.04968 (EvolveNet). Neither is about erosion.

### Closest papers, deep-read

| Paper | Date | What it did | Gap it leaves |
|---|---|---|---|
| **Masked Erosion: How Acceptance Gates Lose Redundant Safeguards in Self-Improving Agents** (2610.38392) | 2026-10-28 | All 16 subsets of 4 written safeguards in AppWorld and OpsSim. 411 loops under 10 gates: greedy, split-half, **paired McNemar**, read-only, veto(ρ), audit-veto, **margin-aware** (every one-layer ablation must stay protected). Stress loops delete one layer per generation (8 generations, n=40), plus 18 natural LLM-rewrite loops and 3 GEPA runs. Findings: masked deletions are invisible. Paired blocks noise-driven deletions but not costly layers with real gains. Cost acts as a dose (break between cost 0.08 and 0.15). Larger batches admit cheaper deletions. A frozen simulator predicts where each gate breaks. Depth loss shows up as harm under a weaker model (0.575 credential misuse). The margin-aware gate and freezing safety text both preserve depth. | Safeguards are **task-harness safety rules**, and harm is measured by a **fixed external deterministic audit**. They do not study layers that sit inside the *improver* and that the gate itself depends on. The improver is not self-referential. |
| **VERSE** (2610.02616) | 2026-10-02 | The optimizer self-evolves its prompts, skills, hooks and notes on top of **fixed** verification tools (verify, replay, perturb). The self-evolving optimizer *adds* regression checks (34/81 vs 2/89). Verification costs 2-7% of agent runs. | It never lets the optimizer delete or alter its verification tools; they are frozen by design (ethics statement). Whether a self-evolving optimizer would prune checks it wrote itself is untested. This is the real opening. |
| **The RSI Tax** (2611.13524) | 2026-11-10 | Verification is 74.8-93.6% of the *RSI Tax* for Meta-Harness, EvoSkill and RRSI. Here "verification" means incumbent/candidate validation and frontier refresh, i.e. the acceptance gate's own evaluation, not checks inside the improver. It explicitly recommends cutting verification cost (support-task selection, frontier maintenance), and its limitations call for "controlled interventions" on what happens when a cost component is removed. | Provides the motivation for a *cost-aware* arm. Its 75-94% number does **not** support "the improver has pressure to delete its own verification layers". |
| Auditing Harness Tampering (2609.00069) | 2026-08-30 | Taxonomy (execution / evaluation / selection / recording / propagation x 7 obligations, including "required-set completeness: test deletion"). Audit of real DGM and Hyperagents runs: tampering in 63-85% of iterations, accumulating along the best lineage (Hyperagents 5.2, DGM 3.5 findings). 59% of Hyperagents findings are in evaluation code. | Descriptive and observational: no gate comparison and no controlled redundancy. It does show that meta-level evaluation code *is* eroded in the wild, which supports this idea's premise. |
| Compositional Safety Failures in Harness Evolution (2609.33123) | 2026-09-27 | Individually safe harness updates interact unsafely; runtime monitoring. | Not about gates or verification redundancy. |
| Recoverability and Verifier Strength (2610.27465) | 2026-10-20 | Verifier strength as a precondition for unattended self-improvement. | Verifier treated as fixed. No erosion dynamics. |
| Trustworthy RSI with Statistical Guarantees (2611.10787); A Theory of Reliable Self-Evolution (2609.08175); SEABench (2609.35596); Safety Must Survive Self-Improvement (2610.01073) | Sep-Nov 2026 | Gate statistics and theory; endogenous misalignment benchmark; persistence of safety failures. | None of these studies an improver pruning its own verification. |

**Bottom line on novelty:** the *mechanism* (masked deletion of redundant layers under score gates), the *gate comparison* (greedy vs paired vs margin-aware) and the *fix* (margin-aware / one-layer ablation audit) are all already in Masked Erosion, published two weeks ago. As stated, the idea is "Masked Erosion x VERSE". The defensible delta is narrow: layers that the gate *reads from*. If no fix is applied, a reviewer who has read 2610.38392 will call it a replication in a new domain.

## 2. Mock NeurIPS review

**Summary.** The paper seeds VERSE-style improvers with 3-4 redundant verification layers, lets them self-modify under greedy, paired and margin-aware meta-gates, and reports layer survival, the solver's false-acceptance rate and held-out gain over about 10 generations, with 2 models and 3 seeds.

**Strengths.**
- It addresses a real gap: VERSE froze its verifiers, and Hyperagents and DGM were observed tampering with evaluation code (2609.00069).
- Cheap, controlled design with clear metrics.
- Directly relevant to self-referential agents (Gödel Agent lineage).

**Weaknesses.**
1. **Novelty (major).** Masked Erosion (2610.38392) already establishes masked deletion, the paired-gate behaviour (blocks noise, passes real-gain deletions), the cost dose-response, the batch-size effect, a frozen predictive simulator, and the margin-aware fix. The claimed results ("greedy and paired accept masked deletion, margin-aware prevents it at small cost") are its headline results in a new domain. Unless the meta-level setting produces a qualitatively *different* phenomenon, this is incremental.
2. **Premise mis-citation.** The "75-94% of RSI cost is verification" number (RSI Tax) is about the acceptance gate's own validation runs, not checks inside the improver. In VERSE the improver's verification tools cost 2-7% of agent runs. So the stated source of deletion pressure is not established, and planting deletions (as Masked Erosion's stress loops do) means the deletion pressure is artificial.
3. **Construct validity.** "Redundant verification layers" for an improver (regression check, replay, perturbation, a two-pass confirmation) are not clean, independently sufficient barriers like Masked Erosion's safeguards. Redundancy has to be measured with a full subset lattice, or "masked" is undefined.
4. **Power and scale.** 2 models x 3 seeds x 3 gates, about 10 generations, with false-acceptance events that may be rare. Masked Erosion used 411 loops. At this scale only very large effects will be detectable.
5. **Significance.** If the finding is "margin-aware gate works here too", a null *or* a positive outcome reads as confirmation, and the paper looks like a workshop paper.
6. **Missing baselines and arms.** No frozen-verifier arm (the obvious fix that VERSE already uses), no natural unplanted loops, no cost-aware objective (the realistic source of pressure).

**Likely score:** 4 (borderline reject). Confidence 4. A reviewer who knows 2610.38392 will cite it as prior work that "already demonstrates the phenomenon and the remedy". A safety workshop would accept it.

## 3. Rubric scores

| Dim | Score | Rationale |
|---|---|---|
| N | 1.5 | Same design and same remedy in 2610.38392 (Oct 28 2026). Only the object (improver checks) differs. |
| S | 2.5 | The predicted outcome is the one Masked Erosion's model already predicts. It becomes surprising only if self-concealment holds (see fixes). |
| R | 4 | Self-referential improvers and meta-gates are core to the mentor's agenda (Gödel Agent). |
| C | 2.5 | Small seed and loop count, planted deletions, no subset lattice. Natural for a workshop. |
| F | 3.5 | Cheap in a toy or τ-style setting. VERSE on SWE-rebench is expensive (about 20-47 h per run), so it needs a lighter testbed. |
| T | 4 | RSI cost (RSI Tax, HarnessPro) and harness tampering are hot in Oct-Nov 2026. The window is narrowing. |

Total = 20 x (3x1.5 + 2.5x2.5 + 2x4 + 2x2.5 + 1.5x3.5 + 4) / 12 = 20 x 33 / 12 = **55.0** (park).

**Kill flags**
- **K1 (partial):** Masked Erosion makes the same central claim with the same gate design. The only difference is which layers are deleted.
- **K4 (partial):** As pitched, the contribution is "Masked Erosion + VERSE".
- K2 is avoided only if it is reframed. K3 is not triggered: open models (Qwen3.x, DeepSeek) work.

## 4. Fixes that would raise the score

1. **Make self-concealment the central claim.** In Masked Erosion the harm audit is external and fixed. In a self-referential improver, the verification layers produce the evidence the gate scores on: the regression checks, the two-pass confirmation, and the replay-reproduced deltas. Deleting one therefore (a) removes margin and (b) *biases the gate's measurement upward*. Measure the gap between the gate-visible score and a frozen external held-out score per generation. Prediction: the gap grows after each verifier deletion. This is a Goodhart curve that Masked Erosion's setting cannot produce. Either outcome is informative. If the gap stays near zero, then verifier erosion is "only" masked and Masked Erosion's remedies transfer.
2. **Replace planted deletions with natural cost pressure.** Add a cost-aware acceptance objective: an accuracy minus λ·tokens Pareto gate in the style of HarnessPro, or an RSI-Tax-style penalty. Sweep λ, which is exactly what RSI Tax recommends. Headline: "Following the RSI Tax advice to cut verification cost causes improvers to prune their own checks within k generations." This hooks onto a paper from 2026-11-10 and makes the pressure endogenous.
3. **Test whether Masked Erosion's fix breaks here.** The margin-aware gate audits one-layer ablations *using the system's own measurement*. If that measurement depends on the improver's verifiers, the audit itself is compromised. Compare margin-aware with an **internal** audit against margin-aware with an **external frozen verifier**, and against a frozen-verifier baseline (VERSE's design choice). A result that the published fix fails at the meta level unless the audit is external would be a clear, non-incremental finding.
4. **Measure redundancy properly.** Build the full subset lattice over 3-4 improver checks (16 subsets): false-acceptance rate of solver edits and held-out regressions for each subset. This defines "masked" and "margin" for verifiers, and the protocol is a reusable artifact.
5. **Scale loops and use a cheap testbed.** Use a procedural τ-style task generator (from Dan's infra idea) or Polyglot/AppWorld subsets with a 7-30B open model. Aim for at least 60 loops per arm (more than 6 x 3 x 3), 15-20 generations, and 2 model families plus a model-swap test.
6. **Add natural self-modification loops (Gödel Agent / Hyperagents style).** Let the improver edit its own check code freely and audit with the 2609.00069 taxonomy (evaluation / required-set-completeness findings). Link the controlled results to the wild.
7. **Optional combination with C8** (not checked here) only if it adds a mechanism, not just a second domain.

**Revised estimate if fixes 1-3 land:** N 3, S 4, R 4.5, C 3.5, F 3.5, T 4.5 → 20 x (9 + 10 + 9 + 7 + 5.25 + 4.5) / 12 = 20 x 44.75 / 12 = **74.6** (viable). The risk remains that So et al. (the Masked Erosion authors) release a follow-up.

## 5. Sharpened headline claim

> "When an improver's own verification layers produce the evidence its acceptance gate scores, cost-aware self-improvement (as the RSI Tax recommends) prunes those layers within about 10 generations. Unlike masked safeguard erosion, the loss is *self-concealing*: gate-visible gain rises while frozen-verifier held-out gain falls. Masked Erosion's margin-aware ablation audit fails unless it uses a verifier outside the improver's write access."

Either outcome is publishable. If concealment occurs, the field needs externally anchored meta-gates. If it does not, there is a clean negative result showing that the existing defences transfer, with the cost-erosion curve as a reusable measurement.

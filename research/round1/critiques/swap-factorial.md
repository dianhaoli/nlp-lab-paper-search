# Critique: swap-factorial (improver × state factorial swap test for RSI)

**Idea:** "Is it the improver or the state? A powered factorial swap test of recursive self-improvement" (T1, Dan-modified)
**Reviewer stance:** skeptical NeurIPS AC + related-work search. Date: 2026-11-12.

---

## 1. Scoop search: what I ran

- alphaXiv discover (recency): (a) RSI / improver / meta-agent / compounding; (b) Hyperagents imp@k / improving at improving / Gödel Agent; (c) since 2026-06: improver vs solver state, swap test, same-start, meta-learning.
- Semantic Scholar keyword search: "swap test improver recursive self-improvement" (2026); "does the meta-agent get better at improving improver checkpoints" (0 hits); "compounding self-improvement improver state headroom factorial" (noise only).
- Forward citations: Hyperagents 2603.19461 (~60 citing, Jun–Nov 2026), AIDE² 2609.26457 (~23 citing), Auditing 2610.27610 (0 citing yet).
- Deep reads via answer_pdf_queries: 2610.27610, 2611.07624, 2609.34924, 2607.14004, 2609.26457, 2609.06396 (MetaRSI), 2611.08946 (Flex-RSI), 2610.14662, 2611.07048, 2610.21791 (RSIFlow), 2610.35434 (Skill∞), 2610.09197 (FreeEvolve).

## 2. Closest prior work (verified in tool results)

| Paper | What it does | What it leaves |
|---|---|---|
| **Auditing the Product, Not the Recursion** 2610.27610 (Oct 20 2026) | Census of 114 RSI papers. Defines claims C1–C5. C4 (improver gain) needs a *swap test*: earlier vs later improver from the same start, same budget, audited successors. C5 (compounding) needs *repeated* swap tests whose gains grow. **Prop. 3 (diagonal non-identification)**: if I_t = φ(S_t), the trajectory cannot identify the improver effect; it gives a two-state example where headroom produces a 0.7 apparent improver gain with zero true effect. Only 4/26 improver claimants run a same-start matched-budget comparison. They flag that the Hyperagents transfer moves the task agent along with the meta agent, so it is not a clean same-start test. | Runs no experiments, so no estimate of how large the confound actually is. Neither the theory nor the "repeated swap" design for C5 has been run. **They have published Dan's framing, so a fast follow-up is a real risk.** |
| **MetaRSI** 2609.06396 | Term-2 fork: the same S1 improved by the original vs the meta-updated improver at the same budget → +7.2 vs +4.9 (one state, 5 outer seeds). Five-term "with vs without meta-update" curves: the gap widens, both decay. Frontier fleet: "retention follows headroom" (ρ = −0.84). | Only one off-diagonal cell (S1). No I×S crossing, no test of a growing improver effect (the five-term curve is an evolving-vs-frozen comparison, not repeated swaps). Its own data shows a strong headroom effect. |
| **RSIFlow** 2610.21791 (Meta0..Meta4 table) | Applies 5 meta-agent checkpoints to a **common initial task agent**. Meta4 beats Meta0 by +20.6 on Code. GPT-5.6 Sol meta, Qwen3-4B/8B task agent. | Only one state (S0), apparently one run per cell, budget not matched to I0 + extra search. No state crossing. |
| **FreeEvolve** 2610.09197 | Seed vs meta-evolved evolution skill, each frozen and run on the **same fresh worker at the same budget**: +6.9 avg. **Haiku 4.5 as evolver gives no held-out gain**; it only starts working at Sonnet 4.6. | Only an S0 swap (I0 vs I_final), 3 eval reps. No intermediate checkpoints or states. Shows small models may not drive meta-evolution. |
| **Skill∞** 2610.35434 | "w/o self-improvement" = seed meta-skill on the same seeds: 55.9 vs 60.7. GPT-6 Luna, ~8.7M input tokens per task skill. | Only an S0 swap. No checkpoint curve, no crossing. Expensive frontier model. |
| **Flex-RSI** 2611.08946 | Paired frozen-vs-self-improving-harness runs (6/7 favor the improver). Cites 2610.27610. Gains are largest where the starter makes little progress, i.e. headroom. | An evolving-vs-frozen comparison, which 2610.27610 says does **not** identify C4. One run per model and task. |
| **AIDE²** 2609.26457 | "Ignition test": AIDE47 vs AIDE_human as outer improver, both starting from inner AIDE47, 3 seeds × 50 steps. **Inconclusive**: the authors call stronger tests "prohibitively costly". | An open invitation, but frontier-cost. |
| Beneath the Surface 2611.07624 | Repeated evals of AHE, Hyperagents, A-Evolve and GEPA (GPT-5.4; τ³-Banking, TB2). Parent "improvement potential" replays: current score does not predict further gain (below random on τ³). | Studies how solver state relates to improvability, but never crosses improvers. **It is close evidence that state matters.** |
| Do Agent Optimizers Compound? 2607.14004 | Two-phase continual re-optimization (GEPA, Meta-Harness, RELAI) on 22 TB2 tasks. | "Compounding" here means re-optimization without forgetting, not improver gain. Single run. |
| Audit the Scaffold 2609.34924 | Stationarity dichotomy: saturation follows from a fixed reachable set. Says the decisive experiment (vary the scaffold, hold the weights) is unrun. | Theory. |
| Does Failure Feedback Direct Self-Modification? 2610.14662 | Randomized-target **directedness e-value** on DGM logs (no directed effect). | **The "directedness e-value section" in this idea is that paper's method.** It would be a borrowed add-on, not a contribution. |
| Third Eye 2611.12988; RSI Tax 2611.13524; Trustworthy RSI 2611.10787 | Adjacent work: weights-side follow-on utility; cost-benefit; statistical promotion guarantees. | Not the I×S question. |

**Verdict on novelty:** No paper has run a **fully crossed improver-checkpoint × solver-state design** with an interaction term, a budget-matched I0 comparator and a powered test of the improver slope over generations (C5). So K1 is not triggered. But:

1. the conceptual contribution (diagonal non-identification, swap test, C4/C5) is already published in 2610.27610;
2. at least 4 papers (MetaRSI, RSIFlow, FreeEvolve, Skill∞) already report positive same-start improver gains at one state;
3. 2610.27610 explicitly asks for this experiment. The remaining gap is *execution and power*, plus the headroom × improver interaction. That is real but narrow, and the field is moving fast (Oct–Nov 2026). Anyone with 2610.27610 open could run it.

## 3. Mock NeurIPS review

**Summary.** The paper crosses improver checkpoints I{0,5,10,20,40} with solver states S{0,5,10,20,40} from 40-generation runs of three meta-agent families on two benchmarks. It fits a two-way model with interaction and tests whether the improver main effect grows with generation. It reports that the improver main effect is less than 1/3 of the apparent trajectory gain and that its slope is indistinguishable from zero.

**Strengths.**
- It asks exactly the identification question that 2610.27610 (Prop. 3) shows trajectory data cannot answer. Off-diagonal cells are the right fix.
- The comparators are good: I0 given the meta-evolution tokens (best-of-N or longer search), audits on fresh procedurally generated tasks, and seeds per cell.
- It adds an estimand beyond prior work: headroom × improver interaction and a slope test for C5.
- If done well, the protocol is reusable as a standard "C4/C5 audit" kit.

**Weaknesses (what would sink it).**
1. **Novelty is a design, not an idea.** Reviewers will say: "2610.27610 proposed this exact test; MetaRSI, RSIFlow, FreeEvolve and Skill∞ already ran same-start swaps; this is the factorial version of a published recommendation." Unless the result is surprising, expect "solid but incremental / TMLR-style audit".
2. **Capability floor makes the null uninterpretable.** With Qwen3.5-9B and Haiku as meta-agents, the improver may barely change across generations: edits are cosmetic or rejected, and FreeEvolve found Haiku 4.5 meta-evolution yields no held-out gain. A null slope then means "weak models cannot meta-learn", not "RSI does not compound". This is effectively a K2 risk: reviewers will discount a null from weak improvers. *You must show the improvers actually differ (edit-distance / behavioral divergence across I checkpoints) and include at least one capable improver.*
3. **The "state" variable is ill-defined across families.** In Gödel/Hyperagents-style self-editing, the improver and solver live in one codebase; separating them is a design decision that can break the system. Prop. 3 assumes separability. The "transplant" of I_40 onto S_0 may be out of distribution for I_40 (it expects its own tools or memory). A negative improver effect could therefore be a compatibility artifact. You need a compatibility control: I_t applied to its own S_t versus to state-matched foreign states.
4. **Headroom vs state are confounded too.** Later states have higher scores and less headroom, so "state effect" absorbs both "less room" and "different kind of state". You need a headroom-normalized gain (gain / (ceiling − score), with the ceiling estimated by oracle or pass@k) or states matched on score but differing in lineage.
5. **Power and cost.** 3 families × 2 benchmarks × 25 cells × 3–5 seeds is 450–750 inner improvement runs, each with an audit, plus the 40-generation source runs per family/benchmark (ideally ≥2 lineages, because one lineage gives one draw of the I-trajectory). Realistically that is beyond $1.5k with agentic tasks on TB2/Polyglot. Slope tests over 5 checkpoints from **one lineage** have n = 5 for C5. Lineage-level replication is the real unit, and it is missing.
6. **Selection/winner's-curse in checkpoint choice.** Which I_t is "the improver at generation t" (incumbent? archive best?) must be pre-specified; otherwise the curse leaks in.
7. **Kitchen sink.** Hyperagents replication, SRN bounded-note baseline and a directedness e-value (2610.14662's method) dilute the paper. Reviewers will read three half-papers.

**Questions for authors.** What is the estimand for C5: the slope of g(s̄, I_t) on t, averaged over which state distribution? How many independent lineages? What fraction of improver edits survive transplant? What is the MDE given pilot variance?

**Likely scores.** Soundness 3/4 if powered, otherwise 2. Contribution 2–3/4. Overall 5 (borderline accept) if the result is a clean, surprising headroom decomposition with a capable improver. 4 (borderline reject) if weak models only, or if a single lineage drives the conclusion. Workshop risk: high if scoped as "we ran the audit 2610.27610 asked for" without a sharper claim.

## 4. Scores (rubric)

| Dim | Score | Reason |
|---|---|---|
| N (Novelty) | 3.0 | Full I×S factorial plus C5 slope test is unrun. But the framing (2610.27610) and ≥4 same-start S0 swaps exist from Sep–Nov 2026, and scoop risk is high. |
| S (Surprise) | 3.5 | "Most of 'improving at improving' is headroom" would change evaluation practice, and "first powered positive C5" would be big. A null from weak improvers is weak, though, and a positive result is unlikely at this scale. |
| R (Relevance) | 5.0 | Directly Gödel-Agent-lineage: self-referential improvers and long-horizon self-improvement. |
| C (Competitiveness) | 3.5 | Design is clean and the protocol reusable. Risks: underpowered C5 (lineages), transplant artifacts, headroom/state confounding. |
| F (Feasibility) | 2.5 | Pilot under $60 is plausible. The full plan (3 families × 2 benchmarks × 25 cells × seeds, plus 40-generation lineages) likely costs $2–6k. Hyperagents may not run on open models, and Haiku-class meta-agents may not move. |
| T (Trend) | 4.5 | RSI evaluation is the hottest thread of Oct–Nov 2026 (2610.27610, Flex-RSI, SelfImprove-Bench, RSI Tax, Beneath the Surface). |

**Total = 20 × (3·3 + 2.5·3.5 + 2·5 + 2·3.5 + 1.5·2.5 + 1·4.5) / 12 = 20 × 43 / 12 ≈ 71.7**, which is in the "viable with fixes" band.

**Kill flags.**
- K1: not triggered (no factorial paper found), but 2610.27610 + MetaRSI + RSIFlow + FreeEvolve make it "near-K1". Re-check weekly.
- K2: at risk. A null obtained with weak improvers is not publishable. Fix by including a capable improver and showing that the improvers differ.
- K3: not triggered. Haiku/Qwen are affordable, but a capable improver (Sonnet-class or a strong open model) is needed, so budget for it.
- K4: not triggered, but drop the add-ons (directedness e-value is 2610.14662's method) to avoid a "combining existing things" read.

## 5. Fixes that raise the score

1. **Reframe as "re-auditing published improver gains".** Take the 3–4 published positive same-start/self-improvement claims that ship code and checkpoints: FreeEvolve (seed vs meta-evolved skill), Skill∞ (seed vs refined meta-skill), RSIFlow (Meta0..Meta4), plus the Hyperagents transfer. Re-run each under (a) a budget-matched I0 that gets the meta-evolution tokens, (b) a crossing over ≥3 solver states, (c) ≥5 seeds. Headline: *how much of each published "better improver" survives?* This gives the ranking-flip surprise the rubric wants, and it pre-empts the 2610.27610 authors, who only coded papers.
2. **Cut scope to make power real.** Use 2 families (one self-editing code family, e.g. a Gödel-Agent/DGM-lite you control; one skill/meta-skill family, e.g. FreeEvolve or Skill∞-style), 1 primary benchmark (the procedural τ generator, which gives unlimited fresh audits and a known ceiling) and 1 transfer benchmark, and a 4×4 grid. Replicate **≥3 independent lineages per family**. Lineage is the unit for C5, so the slope test has df from lineages, not checkpoints.
3. **Include a capability axis.** Run improvers at two strengths (e.g. Qwen3.5-9B and a Sonnet-class or strong open-weight model) at the same budget. Predicted interaction: the improver effect exists only above a capability threshold, which is consistent with FreeEvolve's Haiku null. This turns the weak-model null into a finding ("meta-learning requires X").
4. **Decouple headroom from state.** Report a headroom-normalized gain, Δ / (ceiling − score), with the ceiling from pass@k or the oracle in the procedural generator. Add **score-matched foreign states** (same score, different lineage) so that "state identity" and "headroom" are separate factors.
5. **Transplant-compatibility control.** For self-editing families, apply I_t to its own S_t and to a score-matched S'_t from another lineage. Measure the transplant penalty, and report the improver effect only where transplant is valid. Pre-define what counts as "the improver" (module boundary) and "the checkpoint at generation t" (incumbent, not archive-best).
6. **Pre-register** the estimands (improver main effect averaged over the state distribution; interaction; C5 slope over lineages), the MDE from the pilot, and the stopping rules. Use a mixed model: random effects for lineage/seed/task, plus paired bootstrap.
7. **Drop the directedness e-value, the SRN bounded-note baseline and the standalone Hyperagents replication**, or move them to the appendix. Keep one compute comparator (I0 + equal tokens as best-of-N / longer search).
8. **Pilot gate (≤$60):** 1 family, 1 lineage, 3×3 grid, 3 seeds, procedural tasks. Go only if (i) I checkpoints differ behaviorally, (ii) the cell SD supports an MDE of ≤3 pts with the planned seeds, and (iii) the transplant penalty is under 50% of the effect.

## 6. Sharpened headline claim

> "Published 'better improver' gains in self-modifying meta-agents are mostly headroom and budget. In a fully crossed improver × solver-state design with budget-matched baselines across N lineages, the improver main effect explains less than a third of the apparent gain. It is non-zero only for improvers above a capability threshold, and it does not grow with generation (no evidence of C5 compounding). Several published same-start improver gains shrink or reverse once state headroom and matched compute are controlled."

Either outcome is publishable as long as a capable improver is included: a confirmed improver effect would be the first powered positive C4/C5 evidence. A null or headroom-dominated result would overturn a pattern the field keeps reporting.

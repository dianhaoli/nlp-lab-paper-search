# Critique: rsi-elasticity — "The RSI feedback loop in miniature: measured returns-to-improvement elasticities"

Reviewer stance: skeptical NeurIPS AC + related-work search. Date: 2026-11-12.

## 0. Verdict in one paragraph
Not scooped on the *estimand*: no paper I could find fits a returns-to-improvement elasticity (or Burtsev's R_AI, or Cunningham et al.'s core-loop elasticity) from controlled self-improvement loops. But the *design* (same-start improver swap test) is now common (AIDE² ignition test, MetaRSI, Hyperagents imp@k), and an Oct 2026 census paper (2610.27610) has publicly defined "C5 compounding = repeated swap tests over a horizon" as the unmeasured gap. That makes a fast follow-up likely. The headline as written ("elasticity well below 1") is close to a foregone conclusion: bounded metrics force diminishing returns (2609.34924 proves it), and the economists already guess that loops are subcritical. Also, as specified the x-axis/y-axis units make the elasticity ill-defined: elasticities of accuracy in [0,1] change with the transform. **Score ≈ 64 (viable with fixes). Use it as C1's headline estimand. Do not ship it alone.** The fix that most changes the score is an off-diagonal improver×state grid. It separates recursive gain g (better improver at a fixed state) from frontier hardening σ (same improver at a more-improved state). Measure capability in log-cost units (Burtsev eq. 1), so the estimate is R_AI = g/σ with a real CI, plus a power/detection analysis.

## 1. Scoop search (what I ran)
- alphaXiv discover (recency) ×3: "returns to improvement / elasticity / RSI", "improving at improving / ignition / compounding", "swap test / improver gain / same-start".
- Semantic Scholar keyword search (2026), 6 phrasings (elasticity, diminishing returns, accumulated improvement / checkpoint, research-productivity feedback / intelligence explosion estimate, improving-at-improving meta-agent).
- Forward citations: 2609.15802 (Economics of RSI), 2609.36054 (GovAI), 2609.26457 (AIDE²).
- Deep reads (answer_pdf_queries): 2610.27610, 2609.00137, 2610.33447, 2610.29677, 2609.34924, 2610.18445, 2609.15802, the alphaXiv "When, Where, and How Experience Becomes Structure" survey.

### Closest papers (verified in tool results)
| Paper | Date | What it does | Gap it leaves |
|---|---|---|---|
| **Auditing the Product, Not the Recursion** 2610.27610 (Chen, Zhu, Zheng) | 2026-10-20 | Census of 114 RSI papers. Defines claims C1–C5. C4 improver gain needs a swap test from identical starting artifacts at matched budget; only 4 of 26 claimants run one. C5 compounding (repeated swap tests, growing improver gain) is "not measured" anywhere. Proposition 3 (diagonal non-identification): from trajectories alone, improver effect and state effect are confounded; you need off-diagonal observations. | Runs no experiments. It explicitly asks for the estimand this idea targets. **This is the biggest threat and also the best citation for motivation.** Anyone who reads it can run "C5". |
| **Recursive Criticality of AI Self-Improvement** 2609.00137 (Burtsev) | 2026-08-31 | Delay-DE model. R_AI = χa/σ (recursive gain over frontier hardening). Capability is defined as log resource cost to solve a fixed task panel at fixed loss. Says the parameters are "not presently constrained by direct empirical estimates." | Pure theory. It gives a principled unit (log-cost) and the exact quantity to estimate. Our design can identify g and σ separately only with an off-diagonal grid. |
| **The Economics of RSI** 2609.15802 (Cunningham et al., METR/Elasticity Inst.) | 2026-09-14 | Graph-of-elasticities model. Self-sustaining iff total elasticity of Ȧ wrt A > 1. Distinguishes narrow (AI-R&D benchmark) from broad capabilities via a "superelasticity". Macro calibration gives ≈9% vs a 15% threshold (subcritical). Publishes a data "wish list" for labs. | Uses only macro/lab data. No micro-estimates from controlled loops. The narrow-vs-broad split maps directly onto our search vs held-out arms, which is a good hook. |
| **Efficiency vs Capability in the Intelligence Explosion** 2610.33447 (Tadepalli, GovAI) | 2026-10-24 | Task-automation model with two legs (efficiency r_E≈1.07, capability r_C≈3.45), calibrated on METR horizons and algorithmic-efficiency data. Monte Carlo SIE in 49% of draws. | Macro only. Again names "returns" parameters that nobody has measured in a controlled loop. |
| **The Singularity Has a Distribution** 2610.18445 | 2026-10-13 | Stochastic Good process. Detection limit for telling constant from accelerating idea rates: n* ~ (12 z²)^(1/3) ℓ^(-2/3). | A warning about power: a few checkpoints × 5 seeds probably cannot tell elasticity 0.5 from 1. You can also borrow its power analysis. |
| **Audit the Scaffold, Not the Checkpoint** 2609.34924 (OnCorps) | 2026-09-28 | Proves strict diminishing returns under a bounded metric and a fixed reachable edit set (Lean-verified). Measures per-round improvement decaying toward zero on SWE-bench (55 tasks × 5 seeds) and across 401 production sessions. | This makes "elasticity < 1" a *predicted* result. It does not measure improver-to-improver elasticity, does not run swap tests, and calls the "hold weights fixed, vary scaffold" experiment open. |
| **Recursive self-improvement of AI research agents / AIDE²** 2609.26457 (Weco) | 2026-09-22 | Ignition test (swap): inconclusive at 3 seeds. | Underpowered single contrast, no elasticity, no held-out arm. |
| **MetaRSI / RSI²** 2609.06396 | 2026-09-06 | One same-start swap: updated improver +7.2 vs original +4.9 (cited in 2610.27610). | One pair of checkpoints, one state. No curve and no elasticity. |
| **Hyperagents** 2603.19461 | 2026-03 | imp@k across generations; compounding 0.640 vs 0.610, n.s. | No dose-response, no elasticity, transfer is confounded (task agent moves together with meta agent, per 2610.27610). |
| **Toward RSI in Physical AI** 2610.29677 (NUS) | 2026-10-21 | Defines R1/R2 recursion tests (updated vs earlier improver on new failures; frozen-knowledge control). Finds no controlled physical evidence. | Design spec only, no estimate. R2's frozen-knowledge control is a useful extra arm (procedure vs memory). |
| Others seen (lower overlap) | Oct–Nov 2026 | RSI Tax 2611.13524 (cost-benefit of RSI on continual tasks); Physical RSI observational scaling law 2610.26848; Third Eye 2611.12988 (does an update stay useful after the next update); Flex-RSI 2611.08946, SelfImprove-Bench 2611.02797, RSI-Env 2611.01736 (benchmarks); "Does Failure Feedback Direct Agent Self-Modification?" 2610.14662 (cites Economics of RSI; directedness, not elasticity); "When, Where, and How Experience Becomes Structure" survey (calls for "improvement method: different versions from same start"). | None estimates a returns elasticity or R_AI from a controlled loop. |

**Scoop bottom line:** the estimand is unclaimed as of 2026-11-12. But three things together raise the odds of a scoop by May 2027. The theory papers (2609.00137, 2609.15802, 2610.33447) all ask for exactly this number. The census paper (2610.27610) names the missing experiment. The RSI flood is producing roughly 100 papers per two months. Novelty is real but has a short half-life. Move fast and pre-register.

## 2. Mock NeurIPS review

**Summary.** The authors take improver checkpoints g0/10/20/40 from three self-improvement systems. Each checkpoint is applied to the same fresh artifact and fresh procedural tasks at a fixed budget. They regress log(gain rate) on log(accumulated improvement) and report a "self-feedback elasticity" below 1, lower on held-out tasks than on search tasks.

**Strengths.**
1. It connects two communities that don't talk to each other: RSI economics (Cunningham et al.; Burtsev; GovAI) and agent self-improvement (Gödel Agent, Hyperagents, AIDE²). A shared estimand with a theoretical meaning (above or below 1 means self-sustaining or not) is a real contribution, if done right.
2. The same-start, matched-budget design avoids the diagonal confound that 2610.27610 shows ruins most RSI claims.
3. Cheap, open-model feasible, multi-system.

**Weaknesses.**
1. **The estimand is ill-defined as stated.** "Accumulated improvement" and "gain rate" are measured in accuracy points. Elasticities are not invariant to monotone transforms: accuracy, error rate, and logit give different signs and magnitudes near a ceiling. The economics models define capability on a ratio/log-cost scale (Burtsev eq. 1: log of resource cost to solve a fixed panel at fixed loss). Without that, "ε < 1" has no theoretical meaning. A reviewer will say "the threshold of 1 is an artifact of your units."
2. **The design mixes up the two quantities the theory separates.** A fresh-artifact swap varies the improver at a *fixed* solver state. It identifies the improver-to-productivity elasticity (Cunningham's ε_{R,C}, Burtsev's g). It does not identify frontier hardening σ, because σ shows up only when the *solver* state advances. Self-sustainability depends on g/σ. So the headline "the loop is not self-sustaining" does not follow from the experiment. You need off-diagonal cells: the same improver applied to more-improved artifacts.
3. **The result is foreseeable.** 2609.34924 proves strict diminishing returns for bounded metrics with a fixed reachable set. Hyperagents and AIDE² already show small or n.s. compounding. "Elasticity well below 1" will read as confirming the obvious. Only a ≥1 local estimate, or a sharp narrow/broad split, would surprise.
4. **Power.** Four checkpoints make four x-values. The log-log slope has 2 degrees of freedom per system before seeds. With 5 seeds and the known run-to-run variance of evolution loops (RRSI, Rethinking Eval), the CI on the slope will likely include both 0 and 1. 2610.18445 shows that detecting acceleration needs many events at small ℓ. No power analysis is offered.
5. **Selection and winner's curse.** Checkpoints g10–g40 come from a lineage selected on noisy dev scores. Their "accumulated improvement" is inflated by selection noise (Winner's Curse in Self-Improvement Loops, extra_leads). Regression to the mean then biases the slope downward. That could produce "ε < 1" with no real diminishing returns.
6. **Compute matching across checkpoints.** Later improvers are longer prompts or programs with more tools, and they use more tokens per proposal. Matching "budget" by iterations rather than tokens or $ confounds improver quality with spend (EvoX criticism).
7. **What carries the improvement?** In Gödel Agent-style systems, the gain between g0 and g40 may sit in memory or archive (information about the task family) rather than in the procedure. On a *fresh* artifact, memory can help or hurt, and the paper cannot say which. 2610.29677's frozen-knowledge control (R2) is the missing arm.
8. **Scope.** "Today's RSI loops are not self-sustaining even in miniature" generalizes from three harness-level loops on procedural tasks with frozen weights to "RSI". Reviewers will flag the scope gap (2610.27610's S1 item).

**What would make reviewers say...**
- *"Reanalysis / incremental":* if the swap tests are just Hyperagents imp@k at more generations with a log-log fit on top.
- *"Workshop":* single benchmark family, CI spanning 1, no off-diagonal design, no unit justification.
- *"Engineering":* a pipeline for checkpoint swapping with no estimand theory.

**Questions to authors.** Which capability unit makes ε = 1 the self-sustaining threshold in your setting? How do you separate g from σ? What is the minimum detectable elasticity at your N? How do you correct for selection-inflated x?

**Score (as submitted): 4 (borderline reject); confidence 4.** With the off-diagonal grid, log-cost units and a power analysis: 6 (weak accept).

## 3. Rubric scores
| Dim | Score | Rationale |
|---|---|---|
| N Novelty | 3.0 | No paper estimates the elasticity or R_AI from controlled loops. But the swap-test design is established (AIDE², MetaRSI, Hyperagents), and 2610.27610 publicly names the gap (C5), so scoop risk by May 2027 is high. |
| S Surprise | 2.5 | "ε < 1" is predicted by theory (2609.34924) and by prior n.s. compounding results. Narrow > broad is also expected. Surprise needs a locally supercritical regime or a ranking flip. Rises to ~3.5 with the g-vs-σ decomposition: "improvers do get better, but hardening eats it". |
| R Relevance | 4.0 | Directly about Gödel-Agent-style self-referential improvers and long-horizon self-improvement. |
| C Competitiveness | 2.5 | As written: units problem, design identifies only half the quantity, underpowered. Rises to ~3.5 with the fixes below. |
| F Feasibility | 4.0 | $300–1k, open models, mostly shared with C1. The off-diagonal grid roughly doubles the cost (still < $2k). |
| T Trend | 4.5 | RSI evaluation is the hottest thread of Sep–Nov 2026 (dozens of "RSI-" papers, three econ papers asking for these numbers). |

Total = 20 × (3·3 + 2.5·2.5 + 2·4 + 2·2.5 + 1.5·4 + 1·4.5)/12 = 20 × 38.75/12 = **64.6** (viable with fixes).
With fixes (N 3.5, S 3.5, C 3.5): 20 × (10.5 + 8.75 + 8 + 7 + 6 + 4.5)/12 = **74.6**.

**Kill flags.**
- K1: not triggered (no 2026 paper makes this claim with this design).
- K2: *at risk*. The stated headline (ε < 1) is the boring outcome, so a null is not interesting unless reframed (see fixes). Partial flag.
- K3: not triggered (open models are fine).
- K4: not triggered as such, but alone it reads as "C1 swap tests + an econ regression". Keep it inside C1.

## 4. Fixes (ordered by score gain)
1. **Off-diagonal improver × state grid (the key fix).** Take improver checkpoints I_0, I_10, I_20, I_40 and solver/artifact states S_0, S_10, S_20, S_40 from the *same* lineage (plus a second lineage for cross-fitting). Run every pair at matched token budget: a 4×4 grid × ≥5 seeds per system. Fit log Δ = a·log c(I) − σ·c(S) + ε, with c(·) in log-cost units. This identifies g (improver gain at a fixed state) and σ (hardening at a fixed improver) separately, so R_AI = g/σ (Burtsev 2609.00137) and Cunningham's core-loop elasticity are both identified. The diagonal reproduces the naive trajectory, and the gap between diagonal and off-diagonal *is* Proposition 3 of 2610.27610, shown empirically. This makes the paper "first identified estimate of R_AI in miniature", not "a log-log fit".
2. **Fix units.** Define capability as log(compute or tokens needed by a fixed reference solver to reach a target success rate on a fixed panel), per Burtsev eq. 1. Alternatively use a "compute-equivalent" scale: how much best-of-N budget the base system needs to match the improved one. Report robustness across accuracy, logit and log-cost, and show which conclusions survive.
3. **Power analysis and pre-registration.** Simulate the loop's measured seed variance and report the minimum detectable g and σ for your grid. Pre-register the threshold test H0: R_AI ≥ 1. A tight CI excluding 1 is the publishable negative. Cite 2610.18445's detection law as motivation.
4. **Narrow vs broad as the superelasticity.** Map search-task vs held-out/procedurally-fresh-task elasticities onto Cunningham et al.'s narrow C1 vs broad C2. Report d log ε_broad / d log A (does passthrough to broad capability fall as the loop advances?). This is the one cell with real surprise potential: R_AI could look ≥1 on narrow search and ≈0 on broad.
5. **Procedure vs memory arm (R2-style).** For each improver checkpoint, add a frozen-knowledge variant: procedure from g_k with memory/archive from g_0, and the reverse. This separates "the improver got better" from "the improver knows the task family". It reuses the procedure-vs-memory idea already on Dan's list.
6. **Selection-bias correction.** Re-score every checkpoint's accumulated improvement on a fresh audit set that was never used for selection (x is measured out of sample). Optionally use a second independent lineage as an instrument, regressing on lineage-B scores for lineage-A checkpoints.
7. **Budget matching in tokens/$**, not iterations. Report the elasticity per unit of cost too ("efficiency leg", 2610.33447).
8. **Systems:** one Gödel-Agent-style self-referential loop, one Hyperagents/DGM-H-style archive loop, and one fixed-improver control (GEPA-style) where g ≡ 0 by construction. That last one is a placebo that should give R_AI ≈ 0, which validates the estimator.
9. **Framing:** position it as the measurement the theory papers ask for, and as the C5 test of 2610.27610. Avoid claiming anything about frontier RSI. Phrase it as "harness-level loops with frozen weights".

## 5. Sharpened headline claim
"Running same-start swap tests on a full improver × artifact-state grid across three self-referential improvement loops identifies the two quantities that RSI-economics models need, measured in log-cost units at matched compute. Later improvers are measurably better (recursive gain g > 0), but frontier hardening σ outweighs that gain. The recursive reproduction number R_AI = g/σ is about 0.2–0.5 on the loop's own search tasks and indistinguishable from 0 on fresh held-out tasks, with CIs excluding 1. Trajectory-only (diagonal) estimates, the evidence most RSI papers report, mis-estimate both g and σ by large factors."

Either outcome is publishable. R_AI ≥ 1 locally on narrow tasks would be a headline result. Subcritical with a tight CI is the first micro-estimate. A diagonal-vs-off-diagonal disagreement is a methods result regardless of the sign.

## 6. Recommendation
Do not run this as a standalone paper. Make the off-diagonal grid plus the R_AI estimate the **headline estimand of C1 (improver scaling)**. That costs roughly 2× C1's swap budget, still under $2k on Haiku/Qwen-class models. Pilot first: a 2×2 grid (I_0/I_20 × S_0/S_20), 5 seeds, one system, about $50, to measure seed variance and decide whether the full grid has power.

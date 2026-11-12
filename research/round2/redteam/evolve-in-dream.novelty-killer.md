# Novelty-killer red team: Evolving in a Dream (evolve-in-dream)

*Round 2, 2026-11-12. Reviewer stance: skeptical by default. I was trying to show the idea is already done, or will obviously be done before mid-May 2027.*

## Verdict

**Major revision. Rubric score 63.8/100, down from the proposal's self-score of 74.6.**

- **No K1 kill.** No paper runs LLM harness or prompt evolution scored inside a simulated environment, with a depth × typed-error sweep checked against real ground truth.
- **The headline claim is now pre-empted almost piece by piece in neighbouring settings.** These are the claims the proposal itself calls "the key fix":
  - "error type, not rate"
  - "iso-accuracy pairs"
  - "decision-relevant errors are exploited, cosmetic ones are not"
  - "ranking/decision fidelity, not next-observation accuracy, predicts usefulness"

  What is left is "the same thing, but the optimizer is GEPA or a harness evolver." That sits close to the K4 combination flag.
- **The paper survives only if its headline is a phenomenon specific to harness search.** A sharper claim along those lines is proposed at the end of this report.

## Searches run (fresh, on top of the round-2 digest)

1. alphaXiv recency: harness or prompt optimization scored in a world model or simulator, with transfer and exploitation.
2. alphaXiv recency: controlled simulator-error injection, error types, fidelity metrics predicting sim-to-real transfer.
3. alphaXiv recency: prompt or skill optimization against user simulators that overfits simulator artifacts.
4. alphaXiv recency: self-improving agents using language world models, with exploitation or Goodhart.
5. Semantic Scholar forward citations of:
   - 2605.15960 (Imperfect WMs are Exploitable): 8 citers
   - 2608.12253 (Simulator Collapse)
   - 2611.01139 (World Models Need Not Be Right…): 0 citers so far
   - 2610.06100 (Trace2Env)
   - 2608.12564 (WMRL)
   - 2610.08033 (Dreams MOBA): 0 citers
6. Semantic Scholar keyword searches, 4 queries, year 2026.
7. WebSearch (extended) for ICLR 2027 / OpenReview submissions on simulator-scored harness or prompt evolution. Nothing was found. **OpenReview itself is still unreachable from this session**, so OpenReview-only ICLR 2027 submissions remain UNCHECKED. This is the biggest open risk.
8. Deep reads via PDF queries:
   - 2609.32322 (DRPE)
   - 2611.10329 (SIRIS)
   - 2610.38439 (World Reward Models)
   - Semantic Scholar abstract of 2611.09670 (Goodhart in the Planner)

## New threats found in this pass (not in the proposal's related-work table)

| ID | Paper | What it pre-empts | Severity | Gap left |
|---|---|---|---|---|
| **2609.32322** (Sep 26, 2026; v2 Sep 30) | *Not All Errors Matter: Decision-Relevant Prediction Error Predicts Planning Quality* (Shanghai Univ.) | **The iso-accuracy, "type not rate" headline.** It builds an *iso-error protocol* that keeps total error fixed while moving error between decision-relevant and irrelevant dimensions. Results on 55 models: models within 1% total error differ by 60 pp in success (97% vs 37%); total error Spearman ρ=−0.25 vs DRPE ρ=−0.84 (−0.98 controlled); deeper imagination amplifies relevant error; learned models fail on rare decision-critical events. | **HIGH for the claim; medium for the design.** | Gridworld plus a fixed MPC planner. There is no iterative optimizer, no harness or LLM, and no optimization-depth axis; "depth" is planner depth. Error is Gaussian noise on state dimensions, not semantic quirks such as lenient validation. **The proposal does not cite it, and a reviewer who knows it will call Figure 2 a replication.** |
| **2611.09670** (Nov 6, 2026) | *Goodhart in the Planner: Sampling-Based Control Exploits Rewards Learned from Human Preferences* | **The "accuracy doesn't predict exploitation; search pressure does" curve.** Harder MPPI search cuts true return by up to 76% while held-out label accuracy stays at 0.75–0.97. True return tracks √KL from the proposal distribution. | Medium | Robotics reward models. There is no LLM, no simulator dynamics, and no typed errors. But √KL is a ready-made, cheaper competitor to τ_path, and reviewers will ask why τ_path beats "distance from seed harness." |
| **2610.38439** (Oct 28, 2026) | *Predict the Reward, Not the World: World Reward Models for ML Agents* (HKUST-GZ) | **The "decision fidelity, not prediction fidelity" argument inside LLM agent search.** It proves predict-then-decide cannot beat a direct selector, and that a maximum-likelihood world model can prefer the worse program on every instance. Empirically, a near-chance selector (Jev, 56%) lowers the final score of three search agents, while a calibrated WRM keeps scores at 4.4–7.5× lower cost. | Medium | ML-engineering program search, one run per task, and no depth curve or typed errors. It also partly pre-empts RQ4 ("cheap substitute for real evaluation"). |
| **2611.10329** (Nov 6, 2026) | *SIRIS: Can LLMs Tell When Simulation Errors Matter* (KAIST/MSRA) | Controlled *perturbations of a simulator*, used to ask whether a modeling limitation changes **which candidate should be selected** (upheld vs overturned pairs). | Low-medium | It tests an LLM's *judgment* about fixed candidates in systems research. There is no evolution loop and no exploitation. It is a methodological precedent for "perturb the simulator, check decision impact" and pre-empts the "BreakingWeb applied to the simulator" novelty line. |
| 2610.08033 (round-2 digest) | *Learning in Dreams, Winning in Reality* (MOBA) | Policy inherits the world model's fidelity profile mechanic by mechanic. Exploitation is invisible from inside the dream. | Medium | Correlational, weight RL. The authors name the causal intervention as **their future work**, so a follow-up from them is a likely scoop of the "per-quirk causal" angle, in RL. |
| 2609.14858 Dream-RSI (round-2 digest) | GDM: RSI by "dreaming" over replayed discovery trees | It owns the word "dream" for RSI. | Medium (naming/framing) | Replay simulator with no model error and no fidelity axis. **Rename the paper**: "Evolving in a Dream" will be read as derivative. |

**Previously known threats, re-checked.** 2611.01139 has 0 citers, so no follow-up yet. 2608.12253's citers (EAU 2610.16523, AutoEnvScaling 2610.14163, user-simulator training 2610.13617) do not evolve harnesses. Trace2Env's only citer is ToolSpace 2611.05036, an environment-synthesis paper. WMRL's citers include *Language Models as AI Research World Models* 2610.12235, which is about outcome prediction for research agents (abstract only; no optimization-depth study seen).

## How the proposal's claims survive, one by one

| Claim | Status after this pass |
|---|---|
| "Error *type*, not rate, decides exploitation; iso-accuracy pairs differ by >10 pp" | **Pre-empted in planning** (DRPE 2609.32322: iso-error, 60 pp gap). Moving it to LLM harness search alone is incremental. |
| "Next-observation accuracy is the wrong fidelity metric" | Pre-empted several times: Lambert 2020, 2610.19375, 2609.33335, DRPE, WRM. The proposal already concedes this. |
| "Ranking fidelity ≠ optimization safety" | 2611.01139 (static τ high, optimization collapses), 2611.09670 (accuracy high, search exploits). |
| "On-path τ_path predicts the peak" | **Still open**, but now faces two cheap baselines a reviewer will demand: √KL / edit distance from the seed harness (2611.09670), and top-1 selection regret (2611.01139). If τ_path does not beat these, the metric contribution is gone. |
| "Sim-scored harness evolution Goodharts with depth" | Open for harnesses. The prior is strong (2605.15960 inevitability; 2608.12253; DarwinX's proxy saturation), so a positive result is *expected*, not surprising. |
| "Ensembles and anchors recover most of real evolution cheaply" | Weakly pre-empted by WMRL anchors, 2608.12253 populations and WRM. Incremental. |
| Alternative outcome: "harness search does *not* Goodhart" | **The most novel outcome.** Nobody has shown discrete prompt or harness search to be robust to simulator errors. This is the part to elevate. |

## Will it obviously be done before May 2027?

**Probably not the controlled-quirk design. A loose version, quite likely.**

- **High probability (≈50–60%):** some group posts "GEPA/ACE/harness evolution in an AWM/Trace2Env/EnvScaler simulated environment transfers to real τ²-bench/WebArena." This would be a positive-transfer *method* paper with a depth curve in an appendix. AdvSim2Real, DarwinX, WMRL and AutoEnvScaling all point here, and environment synthesis is the hottest lane. Such a paper would eat RQ1 and the "why now" motivation.
- **Medium probability (≈25–35%):**
  - The DRPE authors or the 2610.08033 authors extend their work to LLM agents or to iterative optimization.
  - The 2611.01139 authors extend RAVEL to LLM policies.
- **Low probability (≈10–15%):** someone runs typed semantic quirk injection × harness-evolution depth × held-out real ground truth before May 2027.

**Field trajectory to anticipate.** Over the next six months:
- "Decision-relevant fidelity" is becoming the standard evaluation framing for world models (DRPE, WRM, SIRIS, 2606.15032 position).
- Simulator-scored *selection* (WRM, Agentic WM test-time BoN) is being productized.
- Exploitation-vs-search-pressure laws (√KL, safe horizon) are being written for RL and control.

By NeurIPS 2027 reviewing (July), "simulator errors matter by type, not rate" will read as folklore. The paper has to be about **what is different when the optimizer is an LLM that reads trajectories**.

## Rubric score (fresh)

| Dim | Weight | Score | Reason |
|---|---|---|---|
| N | 3 | **2.5** | No K1. But ≥2 close papers in the last 3 months (DRPE, Goodhart-in-Planner, WRM, plus 2611.01139 and 2610.08033) cover the headline's mechanism in adjacent settings. Novelty is "the same, for harness search." |
| S | 2.5 | **2.5** | A positive Goodhart curve and the type-not-rate result are now the expected outcome. Only the no-Goodhart branch, or a harness-specific mechanism, would surprise. |
| R | 2 | **4.5** | World models, a self-improvement loop and BreakingWeb-style intervention: the mentor's core agenda. |
| C | 2 | **3.5** | The design is rigorous (REAL-count/cost arms, seeds, CIs). But reviewers will flag that it is a measurement study re-deriving DRPE, and it lacks √KL and regret baselines. |
| F | 1.5 | **3.0** | About 140 runs, a web sandbox and self-hosted models are heavy for a solo undergrad. D1-only is feasible. |
| T | 1 | **4.0** | Hot, but the wave (decision-relevant fidelity) is cresting now. |

**Total = 20 × (3·2.5 + 2.5·2.5 + 2·4.5 + 2·3.5 + 1.5·3.0 + 1·4.0) / 12 = 20 × 38.25 / 12 ≈ 63.8.**

## Required fixes

1. **Cite and position against DRPE 2609.32322 explicitly.** Drop "type not rate" and "iso-accuracy" as headline claims; keep them as a *replication* control.
2. **Add the √KL / edit-distance-from-seed baseline (2611.09670) and the selection-regret baseline (2611.01139)** to the RQ3 metric shoot-out. τ_path must beat them, or RQ3 should be dropped.
3. **Cite WRM 2610.38439 and SIRIS 2611.10329.** Reframe RQ4 against WRM-style direct selectors, not only against WMRL anchors.
4. **Rename the paper.** Dream-RSI 2609.14858 and 2610.08033 already own "dream."
5. **Make the headline harness-specific.** Use the sharper claim below.
6. **Make the alternative outcome a pre-registered first-class hypothesis.** It is the one result that contrasts with weight-RL collapse (2608.12253, 2611.01139, 2610.08033).
7. **Post an early preprint (by Feb 2027) of the D1-only core.** The loose positive-transfer version is likely to appear first, and a dated quirk-legibility result protects the specific claim.
8. **Re-check OpenReview ICLR 2027 once the host is allowed.** Search terms: "simulated environment" × "prompt optimization"/"harness".

## Non-obvious, lower-scoop angles (ranked)

1. **Quirk legibility, not quirk relevance (best).** Reflective evolvers such as GEPA, ACE and Meta-Harness learn from *text traces*, unlike planners and RL. Prediction: a quirk is exploited quickly when it is *visible in the trajectory text* (e.g. "validation skipped, order accepted"). It is exploited slowly or never when it is *silent* (same state effect, no trace evidence), even at identical decision relevance and rate.
   - Test: a 2×2 of legible vs silent × relevant vs cosmetic, at matched next-observation accuracy.
   - Neither DRPE (planner) nor weight RL can show this, because it is specific to optimizers that read their own rollouts.
   - Corollary mitigation: **trace redaction** (hide simulator-internal text from the reflector) as a zero-cost defense. Compare it against ensembles and anchors.
2. **Reflector vs selector attribution.** In a GEPA loop, quirks can be exploited by *proposal* (the reflector writes quirk-exploiting rules) or by *selection* (the sim score picks them). Swap the reflector's trace source (sim traces vs real traces) independently of the fitness source in a 2×2. This splits "dream-taught" from "dream-selected" exploitation. Nobody has decomposed this, and it fits the Gödel-Agent agenda.
3. **The harness-robustness licence.** If the frozen base model's priors bound how far a harness can stray, harness search should Goodhart *later* than LoRA/RL against the same simulator. Run a direct same-simulator, same-budget comparison: harness evolution vs RL/LoRA. "Harnesses are safer to dream with than weights" would be a sharp, practitioner-relevant claim, and either sign is publishable.
4. **Self-written simulators.** Let the evolver also edit the simulator, Gödel-style (cf. Database ADRS co-evolving its simulator, EAU). Does it learn to make its own dream more lenient? This links to the Learning-to-Goodhart line and is likely unscooped, but it is riskier and costlier.

## Sharper claim (proposed)

> "LLM-driven harness evolution exploits a simulator through what it can *read*, not just what it is scored on. At matched error rate and decision relevance, quirks that are legible in rollout traces are exploited within ~10 iterations, while silent ones are not. Redacting simulator-internal traces from the reflector removes most simulator Goodhart at zero cost, a mechanism with no analogue in planners or weight RL."

Sources (web): [ICLR SimDist](https://iclr.cc/virtual/2026/10018080), [ICLR'26 RSI workshop harness synthesis](https://openreview.net/pdf?id=g9rEYVNn5T). Papers were read via alphaXiv: https://www.alphaxiv.org/abs/2609.32322, https://www.alphaxiv.org/abs/2611.10329, https://www.alphaxiv.org/abs/2610.38439. 2611.09670 was seen as a Semantic Scholar abstract only.

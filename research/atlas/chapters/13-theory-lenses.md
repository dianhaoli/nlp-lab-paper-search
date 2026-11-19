## Theory and cross-field lenses (statistics, evolution, economics, control)

*Marks: ✓ = checked in the full text this session (section or table cited). ◐ = taken from the abstract only, or from a prior project note (FULLTEXT_VERIFICATION, round-2/3 notes). † = classical result cited from memory, with no numbers used.*

*Full texts read this session are cached under /tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/<id>/paper.txt. ICLR 2027 abstracts come from api2.openreview.net search; the PDFs are bot-blocked.*

*Searches this session:*
- 20 ICLR-2027 OpenReview queries: theory, convergence, Price equation, population genetics, PAC, economics, control, evolvability, Goodhart, epistasis, Baldwin, hyper-heuristic, NFL, error catastrophe, model collapse, fixed point, impossibility, optimizer's curse, information-theoretic, L2O.
- 6 alphaXiv recency sweeps.
- Semantic Scholar forward citations of 2510.04399 and 2609.13406. (A re-query on 2026-11-18 returned 0 indexed citers for 2609.13406. S2 lags, so citers were also found by grepping the cached full texts.)

### At a glance
- **What this corner is.** It asks what is provable or measurable about "keep-if-better" self-modification loops, using imported tools:
  - **statistics:** PAC learning, selection under noise / winner's curse, sequential testing;
  - **evolutionary biology:** breeder's equation, drift, Wright–Fisher, shifting balance, evolvability;
  - **economics:** elasticities, principal–agent models, growth models;
  - **control and dynamical systems:** Lyapunov drift, hitting times, stationary laws;
  - **computability:** Rice- and Kleene-style limits.
- **The settled core: selection is the bottleneck, not generation.** Most theory in Sep–Nov 2026 converges on this.
  - Keep-if-better on small evaluation sets is selection under noise: the breeder's equation and the optimizer's curse (2610.09239 ✓ §3.1).
  - The best empirical test finds execution-based selectors at chance level (49.7% pairwise accuracy) even though helpful candidates exist (2611.23068 ✓ §5.2).
- **Proven guarantees all have the same shape.** Each is a validation gate plus some structural cap:
  - a capacity cap on the policy-reachable hypothesis envelope (2510.04399 ✓ §5, Thm 1–2);
  - a retained-task change budget (2609.08175 ✓ Cor. 1);
  - a union-bounded fresh-validation Hoeffding rule (2610.18813 ✓ §4.2, Eq. 8–9);
  - an evaluator kept outside the evolving population (2610.03243 ✓ Thm 4.5).
- **Hard negative results:**
  - No terminating gate is both sound and complete for semantic improvement. This is Rice-style, and the authors concede that for the certifier alone Thm 1 is close to a restatement of Rice (2610.21646 ✓ Remark after Thm 1).
  - The worst-case validation cost to certify a gain u near the ceiling grows as Ω(1/u) (2609.08175 ✓ §4.2 Thm 5).
  - Deterministic judge errors make every fixed acceptance rule that initially improves utility eventually drift negative (T04ZzT5m6j ◐, abstract re-read 2026-11-18).
- **Economics.** All macro models frame RSI as "product of elasticities > 1". The only calibration finds ε_{R,C} ≈ 0.09 per ECI unit against a threshold of 0.15, i.e. subcritical (2609.15802 ✓ §1, §4.1). We found no paper that estimates these elasticities inside a controlled agent loop (our searches).
- **Evolution lens.** It is mostly analogy plus a few real results:
  - Thompson-sampling parent selection provably homogenizes lineages (WGM ◐).
  - Recursive retraining "reproduces the Wright–Fisher process exactly" (2609.18560 ✓ Abstract).
  - Our searches found no paper applying the Price equation, Muller's ratchet / drift barrier, or Kashtan–Alon modularly varying goals to LLM self-improvers. These remain open. (2610.09239 names the drift barrier only as a "relative" in its related work, ✓ §2.)
- **Consensus.** "Self-reported gains are inflated, so report held-out gains with uncertainty." It appears in 2610.09239 ✓ §6, 2611.23068 ✓, 2610.27610 ◐ and iqKbDXemk1 ◐.
- **Biggest open problem.** No theory covers **order-1 (improver-of-improver) selection**. That means selecting edits whose value appears only through *later* edits: editor code, meta-rewards, lineages. Every certificate above covers one-step, object-level adoption.
  - GAI lists the self-evaluation and goal-drift defects as "structural" with no theorems (2609.13406 ✓ §5–6, Table 4).
  - The Kinetic Theory explicitly excludes monolithic self-modifiers: "for it the kinetic description does not apply" (2610.03243 ✓ §1–2).

### How it developed
| Month | Paper | What changed |
|---|---|---|
| 1970–2006 (classical) | Price equation; Eigen error threshold (1971); Hinton & Nowlan Baldwin effect (1987); Wolpert–Macready NFL (1997); Schmidhuber Gödel Machine (2003); Kashtan & Alon modularly varying goals (2005); Smith & Winkler optimizer's curse (2006) † | The vocabulary that 2026 papers import. |
| 2011 | Ring & Orseau "delusion box" † | First formal self-modification pathology. GAI reuses it for the "ungrounded base" defect (✓ §5.2). |
| 2016–2019 | Learning-to-learn (1606.04474); NAS random-search baselines (1902.07638) † | Meta-optimizers overfit their meta-training distribution, and search-space design dominates search algorithm. Both are lessons the RSI field is relearning. |
| 2018-03 | Manheim & Garrabrant, Goodhart variants (1803.04585) ◐ | Regressional / extremal / causal / adversarial taxonomy. 2610.09239 maps keep-if-better under fresh sets to *regressional* Goodhart (✓ Prop. 3). |
| 2022-10 | Gao et al., RM overoptimization scaling (2210.10760) ◐ | Proxy–gold divergence as a function of optimization pressure. |
| 2025-10 (TMLR 08/2026) | Statistical Limits of Self-Improving Agents (2510.04399) | First learning-theoretic account. Five-axis decomposition; iff boundary on the reachable envelope; Two-Gate guardrail. |
| 2026-03 | Reward Hacking as Equilibrium (2603.28063) ◐ | Holmström–Milgrom multitask principal–agent model: under-investment in unevaluated dimensions is structural. |
| 2026-07 | Introspection Threshold (2607.04277) ◐ | Kleene-recursion construction of self-improving programs (Kleene grounding ✓ Abstract); argues LLMs are only "quasi-introspective". |
| 2026-08 | Recursive Criticality (2609.00137, posted 31 Aug ✓ header) ◐; Dynamics of Intelligence Explosions (2608.14426; Toby Ord, Oxford Martin AIGI ✓ header) ◐ | Reproduction-number framing (R_AI > 1 ✓ Abstract). |
| 2026-09 | Reliable Self-Evolution (2609.08175 = ICLR27 NxGP6PGiaU); GAI (2609.13406); Economics of RSI (2609.15802); Evolution of Sex for AI (2609.18560) | The adoption condition and the cost ceiling; two dials for RSI vs GPI; elasticity calibration; population genetics of model lineages. |
| 2026-10 | Winner's Curse (2610.09239); Kinetic Theory (2610.03243); Self-Improvement Problem (2610.21646); Self-Evolving Agent Harness (2610.18813); Sharp Bounds on Winner's Bias (2610.31948); Efficiency vs Capability (2610.33447); Singularity Has a Distribution (2610.18445) | Selection statistics become quantitative and pre-registered; computability limits; macro models multiply. |
| 2026-09/10 (ICLR 2027 submissions) | T04ZzT5m6j, 3t0j2Y05EH EvoLyapunov, cQkSamYGQi WGM, OnzWTehIaN, 0FaPTsSr9C, Nv8es4jEv9, 8vq5qzUnz3 Evade | Deterministic-judge drift theorem, Lyapunov gating, bandit-homogenization proof, RSI-verification criteria, anytime stopping, a dependence-free Goodhart law, evolvability as a selection signal. |
| 2026-11 (1–18) | Lo & Zhao (2611.07048); Verifier Noise Is a Temperature (2611.09699); TRUST (2611.10787); Where Should Learning Live (2611.09701); Resolution Hypothesis (2611.23068); unifying harness framework (2611.22299) | A stationary-law theory of keep-if-better, tested by pre-registration; online multiple-testing gates; the strongest empirical "selection ≈ chance" result. |

### Taxonomy
| Lens | Core question | Representative papers |
|---|---|---|
| **A. Learning theory / PAC** | When does self-modification preserve generalization? | 2510.04399 (reachable-envelope VC); 2610.18813 (continual-PAC stability, retention, admission); 2609.36892 (approximation/generalization/optimization errors for harness evolution ◐); 2611.09701 (regret decomposition ◐) |
| **B. Selection statistics** (winner's curse, testing) | How biased is keep-if-better, and how should gates be set? | 2610.09239; 2610.31948; 2611.23068; 2611.09699; da1Czy7foS Mirage; jd8luqTdQW; VuzgEFCuwx noise floor |
| **C. Certified gates** (sequential / multiple testing) | Guarantees on false adoption and on stopping | 2609.08175; 2611.10787 TRUST; 2606.08106 PACE; 2609.36043 SAGE; 2609.33180 REUSE; 2510.10232 Statistical Gödel Machine; 0FaPTsSr9C stopping |
| **D. Dynamics and control** | Convergence, stability, stationary law of the loop | 2610.03243 kinetic / hitting time; 2611.09699 Boltzmann tilt; 3t0j2Y05EH EvoLyapunov (RAF); T04ZzT5m6j drift; c3lHKtsSMq AutoGovern; MASOpt U3P9ZvN5WF |
| **E. Computability / formal frameworks** | What can be decided or defined at all? | 2610.21646; 2607.04277; 2606.28639; 2609.13406 GAI; OnzWTehIaN; 2611.22299 |
| **F. Evolutionary biology** | Drift, diversity, inheritance, evolvability | cQkSamYGQi WGM (shifting balance); 2609.18560 (Wright–Fisher, Fisher–Muller); 2611.07048 (Darwin/Lamarck/agent-directed); 8vq5qzUnz3 Evade; 2606.05408 Mutation Without Variation; 2608.05810 contamination knee; TAME IXnpSW8g3v (Baldwin-like) |
| **G. Goodhart / mechanism design** | Proxy–target divergence under optimization | 1803.04585; 2603.28063; Nv8es4jEv9; 2610.19832 rent-seeking; C6EElQ2PRG Feedback Alignment; 2609.32677 Improvement Fidelity |
| **H. Economics of RSI** (macro) | Is the AI-R&D loop supercritical? | 2609.15802; 2609.00137; 2610.33447; 2610.18445; 2608.14426; 2611.13524 RSI Tax |
| **I. Meta-optimization heritage** (hyper-heuristics / L2O / NAS) | What earlier "learning to search" fields already learned | 2607.22036 RL hyper-heuristic runtime; 2609.35328 HADA; 2604.19440; 2609.19799 Evolution or Illusion; LhhLpXBFgo Opinionated Optimizers |

### Paper cards

**1. The Resolution Hypothesis: Agent Evolution's Key Is Identifying Improvements** ([2611.23068](https://arxiv.org/abs/2611.23068), 2026-11-17, preprint; ShanghaiTech / ByteDance / BIGAI / SUSTech ✓ header)
- *What it does.* An empirical variance decomposition (law of total variance into run-to-run τ² and evaluation σ²). It argues that evolution stalls because noisy evaluation cannot *identify* better artifacts.
- *Setup* (✓ §3.2):
  - 7 methods: GEPA, SkillOpt-Lite, Trace2Skill, ReasoningBank, HarnessLens, Meta-Harness, AHE.
  - 6 models: GPT-5.4 nano, GPT-5.6 Luna/Terra, DeepSeek-V4-Flash, Kimi-K3, GLM-5.2.
  - Benchmarks: ALFWorld, SpreadsheetBench, OfficeQA, Terminal-Bench v2.1.
  - Two candidate pools of n = 100 skills. Trajectory re-evaluation covers 8 SkillOpt-Lite and 8 GEPA runs, each re-evaluated 8× per task.
  - Cost: >10M LLM calls and 400K CPU-core-hours.
- *Key results:*
  - Single evolve-and-evaluate SD is 1.7–4.1 pp (median per benchmark). In >60% of same-model pairs, the method gap is below one single-run SD ✓ §4.1.
  - The median literature margin is 3.4 pp (IQR 1.2–6.6) across 171 results in 9 papers. None reports a variance estimate, and only 6 average over more than one run ✓ §4.1.
  - A one-run table picks the true best of four methods 51–73% of the time on SpreadsheetBench, OfficeQA and ALFWorld, and 33% on Terminal-Bench (chance 25%) ✓ §4.1, App. D.2.
  - Quality@K rises from 49.2% to 61.8% (K = 1 to 100), so helpful artifacts exist ✓ §5.1.
  - Execution-based selection has 49.7% pairwise accuracy. Regressions are 30.5% of the pool but 62.1% of accepts. Rank correlation is 0.054. Native GEPA/SkillOpt-Lite parent selection reaches 48.6% ✓ §5.2.
  - Large reported gains are often reproduced by one appended sentence that takes an evaluation shortcut (e.g. adapting to a grader's answer format). Across 27 model–benchmark pairs with a gain above 3 pp, this sentence recovers a median 90% of the gain ✓ §4.2.
- *Limitations.*
  - Frozen weights only. Candidate-level evidence comes from Terminal-Bench: the Proposal Pool uses 10 Terminal-Bench tasks, and the Selection Pool uses Terminal-Bench + DeepSWE trajectories over 11 tasks ✓ Limitations.
  - Stronger models may have lower noise, but noise stays at 3.5–4.0 pp on Terminal-Bench ✓ Limitations.
  - It does not test lineage or order-1 selection.

**2. The Winner's Curse in LLM Self-Improvement Loops: Selection Noise, Lock-in, and Acceptance Rules** ([2610.09239](https://arxiv.org/abs/2610.09239), 2026-10-07, preprint; Meta / Microsoft ✓ header)
- *What it does.* Models keep-if-better as truncation selection with correlated candidate errors. It is the breeder's equation R = h²S and the optimizer's curse made explicit. It also gives a myopic Bayes commit threshold and a "regressional Goodhart" drift proposition.
- *Setup* (✓ §5):
  - By default, solver and proposer are the same Qwen2.5-1.5B or 7B model, rewriting its own instructions.
  - Stronger proposers: Qwen2.5-14B and Qwen3.5-4B.
  - A current-model study uses a Qwen3.5-4B solver with a Qwen3.8-27B proposer (§5.4).
  - Tasks: TREC-50, Banking77, GSM8K, with 600 held-out gold items.
  - Pre-registered confirmatory study; GEPA and MIPROv2 audited.
- *Key results:*
  - Overstatement = (1−h²_w)σ_w κ_K + E[c]. For a single candidate with true effect −e and N(0, e²) noise, the accepted candidate's error averages about 1.53e ✓ §3.1.
  - Under fresh sets with s = 0 and μ < 0, held-out performance drifts down by |μ|p per generation while every commit reports a gain (regressional Goodhart, cf. Ohta's nearly-neutral theory) ✓ Prop. 3.
  - After the first rewrite, only 4–39% of rewrites improve. Mean held-out effects are −0.2 to −13.5 pts ✓ §5.1.
  - The model-implied ratio of held-out to measured selection differential matches the observed ratio with MAE 0.034 (0.064 under the Gaussian h²_w) across nine settings and n = 8–256 ✓ §5.1.
  - Final selection-vs-held-out gap: 13–20 pts at n = 16 and 1–5 at n = 256 ✓ Abstract, §5.3.
  - A 64-item audit cuts reported-gain RMSE from 15.5 to 5.7 pts ✓ §6.
  - Calibrated thresholds improved isolated fresh-set decisions, "mainly by committing less often", but the tested rules did not beat greedy over complete runs on reused sets ✓ §6.
- *Limitations.* Self-stated: instructions only, no agentic or code tasks, largest proposer 27B, no model of lock-in dynamics, and no "claims about recursive self-improvement" ✓ §6. The meta-level (two-level) curse is untouched; our round-1 critique calls this open.

**3. On the Statistical Limits of Self-Improving Agents** ([2510.04399](https://arxiv.org/abs/2510.04399), v1 2025-10, TMLR 08/2026 ✓ header; Columbia ✓)
- *What it does.* Decomposes self-modification into five axes: algorithmic, representational, architectural, substrate, metacognitive. Proves that distribution-free PAC learnability holds **iff** the *policy-reachable envelope* has bounded VC dimension.
- *Setup.* Pure theory under i.i.d. data and independent validation. There are no experiments, only one analytic toy example (polynomial features, §4) ✓.
- *Key results:*
  - Thm 1, the iff boundary ✓ §5.
  - Thm 2, the Two-Gate guardrail. Accept only if validation improves by 2ε_V + τ and the new class stays within a capped family G_{K(m)}. Then each accepted edit lowers true risk by ≥ τ, with a VC-rate oracle inequality whose probability does not depend on the number of edits ✓ §5.
  - With a capacity-bonus utility, fit-monotone edits can reach infinite VC ✓ §5 Remark.
  - Algorithmic edits cannot cure infinite VC ✓ §8 Prop. 9.
  - Church–Turing-equivalent substrates preserve learnability; a finite-state downgrade can destroy it ✓ §9 Thm 12 / Prop. 13.
- *Limitations.*
  - Self-stated ✓ §10: i.i.d. only; axes analysed one at a time; capacity proxies may be loose or "difficult to maintain under composition".
  - Validation reuse requires that thresholds not be tuned on V ✓ §5.
  - Our critique, not self-stated: VC capacity is a poor fit for prompt/harness edits over a fixed LLM, where "capacity" is undefined. The paper does sketch an LLM tool-agent reading (§4), and its outlook calls composable computable proxies the open problem ✓ §11.

**4. A Theory of Reliable Self-Evolution for Agent Harnesses** ([2609.08175](https://arxiv.org/abs/2609.08175), v1 2026-09-08, v2 2026-09-28; ICLR 2027 submission [NxGP6PGiaU](https://openreview.net/forum?id=NxGP6PGiaU), same title ✓ OpenReview search; USTC / HKUST / HKBU ✓ header)
- *What it does.*
  - Splits reward change into a failure-task contribution L and a retained-task change D.
  - Gives a sufficient adoption condition, validation rules, a performance ceiling, a lower bound on validation cost, and a reachability diagnostic that separates *generation-limited* from *validation-limited* stalls.
- *Setup.* Theory with 25 numbered propositions plus theorems, building on TRPO/HCPI (✓ §6). Illustration on DS-1000 with a fixed LLM: 3 generation processes × 96 outputs in 24 pools; 48 validation tasks; 56 audit tasks × 3 fresh runs ✓ §5.2.
- *Key results:*
  - J(Ã) − J(A_k) ≥ L − (1−Z_k)δ (Thm 1). Adoption is reliable if λ > (1−Z_k)δ ✓ Cor. 1.
  - Under Two-Gate, the ceiling is J < 1 − (1−Z_k)δ − ε_Σ ✓ §4.1 Cor. 2. A measured-margin rule can exceed it ✓ Rule 2.
  - Any level-β validator needs E[N_eval] ≥ Z/(10u) as the remaining gap u → 0 ✓ §4.2 Thm 5.
  - Reachability gets uniform confidence bounds P̂_in/P̂_out ✓ §5.1 Thm 6.
  - DS-1000 audit: 26–39% of generated outputs meet the audit target (point fractions, wide 90% CIs, e.g. 0.260 [0.031, 0.479]). Only 2/96, 0/96 and 0/96 pass validation ✓ Table 1. In this illustration the stall is in validation ("insufficient evidence"), not generation.
- *Limitations.*
  - One benchmark, one fixed model; illustrative.
  - Expected-reward guarantees only; demand-responsive shift needs a bound ✓ §7.
  - No length or defect-rate variable, and no self-referential improver ◐ (project bold-check).

**5. Generalized Agent Iteration (GAI): One Formal Framework for Iterative Policy Improvement and RSI** ([2609.13406](https://arxiv.org/abs/2609.13406), v2 2026-10-29 ✓ header, preprint; Tianjin U / Shanxi U)
- *What it does.* Generalizes Sutton–Barto GPI into agent-evaluation/agent-improvement cycles with two dials:
  - Dial 1: is the modifier m inside the agent? This splits GPI from RSI.
  - Dial 2: is the evaluation base grounded outside the agent? This gives three polarities: anchored, goal drift, fully self-referential.
- *Setup.* Conceptual. Places existing systems (Gödel machine, DGM, Red Queen GM, MASS, and others) on the two dials ✓ Table 3.
- *Key results.* Four defects, each tied to a violated GPI condition ✓ Table 4:
  - search over candidate selves (improvement no longer cheap and monotone);
  - self-evaluation;
  - ungrounded base;
  - goal drift.

  Cited evidence: 58.33% of anchored-RSI settings improve on the first valid attempt, yet 78.26% of searches that continue past their best end lower (Meng et al. 2026a, as cited) ✓ §5.1.
- *Limitations.* Self-stated ✓ §6: no reachability or complexity claims, no time scales, humans-in-loop excluded. The RSI→GPI reduction is "prose rather than a proposition".
- *Citers.* At least 3 citing preprints in our cache cite it ✓ grep of reference lists: 2610.27610, 2610.29677 (Toward RSI in Physical AI), 2611.22299. S2 currently indexes 0.

**6. Verifier Noise Is a Temperature: Testing Tilt Predictions in Finite-Budget LLM-Guided Search** ([2611.09699](https://arxiv.org/abs/2611.09699), 2026-11-06, preprint; single author, Link Heart Limited ✓ header)
- *What it does.* When both candidates are re-scored at every comparison, and given a logistic score difference and a reversible mutation operator, keep-if-better is a reversible Markov chain (Barker's rule). Its stationary law is the mutation operator's equilibrium tilted by e^{βf}. For general symmetric noise this holds in the small-step diffusion limit, with β = 4F′(0) growing as √minibatch ✓ Abstract, §3. This is a Metropolis / statistical-physics lens on GEPA-style gates.
- *Setup.* LLM-guided program search (bin packing) plus prompt optimisation; predictions are committed from calibration data before search ✓ Abstract.
- *Key results* ✓ Abstract:
  - The level after 150 steps rises with minibatch size in 6/7 settings.
  - Bin packing attains 62–88% of the predicted gain with editing proposers, and 109–119% with write-new proposers.
  - The attained fraction falls with parent–child quality correlation.
  - Runs from good starts lose quality over 300 steps, more from search-selected starts.
  - The prompt-optimisation registered test **failed** (search exceeded the tilt).
- *Limitations.* Did not reach stationarity ✓. The second domain did not transfer ✓. A single-author industry lab.

**7. The Economics of Recursive Self-Improvement** ([2609.15802](https://arxiv.org/abs/2609.15802), 2026-09-14, econ.GN preprint; METR / Stanford / Epoch / MIT / Yale, all with the Elasticity Institute ✓ affiliations)
- *What it does.* Graph-of-elasticities models: self-sustaining acceleration holds iff the product of loop elasticities exceeds 1. It separates narrow (AI-R&D benchmark) from broad capabilities via a "superelasticity" term and publishes a data wish-list for labs.
- *Setup.* Analytic models, calibrated on the Epoch Capabilities Index (ECI) and uplift reports ✓ §2, §4.1.
- *Key results:*
  - ε_{C,A} ≈ 6.5 from an ECI-on-log-compute regression. Self-sustaining requires ε_{R,C} > 0.15 ✓ §4.1.
  - The back-of-envelope estimate is ≈ 9% per ECI unit, even when taking a 4× engineer uplift at face value. So the loop is subcritical, though returns "appear to be strengthening" ✓ §1, §4.1.
  - Superelasticity is argued to be near zero ✓ §3.4.
  - "We have almost no evidence for" ε_{R,C} ✓ §4.1.
- *Limitations.*
  - Macro data only; uplift surveys don't map onto the elasticity ✓ §4.1. Nothing is measured inside controlled agent loops.
  - Our rsi-elasticity critique flagged unit non-invariance. The paper itself notes that the individual magnitudes (hence the 0.15 threshold on ε_{R,C}) depend on how capabilities are normalized, but that the loop gain ε_{R,C}·ε_{C,A} entering the condition is invariant ✓ §3.4 fn. 9. So the critique applies to the threshold on ε_{R,C} alone, not to the criticality condition.

**8. A Kinetic Theory of the Gated Self-Evolving LLM Agent** ([2610.03243](https://arxiv.org/abs/2610.03243), 2026-10-02; ICLR27 [23VDi5bfRj](https://openreview.net/forum?id=23VDi5bfRj) ("Under review at ICLR 2027" ✓ header); Neusoft, single author)
- *What it does.* Treats a plugin/skill population under a validation gate as a gas. It builds a master equation with four operators (collision = co-activation, reaction, external field, gate). The kinetic equation is derived by McKean–Vlasov mean-field propagation of chaos ✓ Thm 4.2.
- *Setup.* Scoped to "DSH-class" plugin populations that satisfy four architectural properties. One model, one task family (WebShop), minimal instance ✓ Abstract, §5.
- *Key results:*
  - Hitting-time certificate E[T_ε] ≤ V₀/(p_min Δ) ✓ Thm 4.1.
  - A plateau is exactly one of proposal exhaustion or information exhaustion (δ_gate > 0 while δ_ind ≈ 0), detectable from logs ✓ Prop. 4.3.
  - Predicted fluctuation scaling β = −1/2 ✓ Prop. 4.4. It appears only at tripled edit density ✓ Abstract.
  - **Separation theorem:** merging the frozen validation set into the evolving set voids the certificate (the "Maxwell daemon must stay outside") ✓ Thm 4.5.
- *Limitations.* The scope explicitly excludes monolithic self-modifiers ✓ §1–2. The fluid analogy is "bounded": momentum is not conserved, so no Navier–Stokes limit exists ✓ Abstract. Empirics are directional only ✓.

**9. The Self-Improvement Problem** ([2610.21646](https://arxiv.org/abs/2610.21646), 2026-10-15, preprint; DFKI / RPTU ✓ header)
- *What it does.* Models agents as programs and improvement as a possibly uncomputable semantic score.
- *Key results:*
  - No terminating certifier (Thm 1) or proposal-respecting searcher (Thm 2) is both reliable and non-conservative. Randomized bounded-error gates fail too (Prop. 1) ✓ Abstract, §3.
  - A bounded-evaluation, finite-budget regime restores guarantees ✓ Thm 3.
  - A chain of accepted updates can raise the bounded score every round while the true score falls ✓ Prop. 2. This is a formal extremal/causal Goodhart (our reading).
- *Limitations.* The authors concede that for the certifier alone, Thm 1 "is close to a restatement of Rice's theorem" ✓ Remark after Thm 1. The contribution is the constructive proof plus the search and multi-agent extensions, and it has no quantitative content.

**10. Self-Evolving Agent Harness (continual-PAC view)** ([2610.18813](https://arxiv.org/abs/2610.18813), 2026-10-13; an 18-page arXiv preprint/survey, with a "66-page" extended version advertised on GitHub ✓ Abstract, pdfinfo; Zhongguancun Academy / USTC ✓ header)
- *What it does.* Formalizes harness self-evolution, maps it onto EA operators (representation, variation, assessment, selection) in §4.1, and gives continual-PAC-style bounds.
- *Key results* ✓ §4.2:
  - Replace-one stability bounds the expected generalization gap (Eq. 5).
  - Telescoping retention gives E R_s(H_k) ≤ E R̂_s(H_s) + β_{s,s} + Σ ε_{s,u} (Eq. 6).
  - Admission rule: fresh m-task validation per round and a conditional Hoeffding + union bound over KT comparisons, giving Δ_k ≤ Δ̂_k + √(2 ln(KT/δ)/m) (Eq. 8). With evaluator–target gap η, the sufficient condition is Δ̂_k + √(2 ln(KT/δ)/m) + 2η < 0 (Eq. 9).
  - It explicitly says HarnessBank and Self-Harness screening "lack the assumed independence".
- *Limitations.* The bounds are textbook. Retention budgets are assumed, not derived ("separate assumptions") ✓. Evaluator drift is handled only by an assumed η ✓.

**11. When Self-Reinforcement Ends Self-Improvement** ([T04ZzT5m6j](https://openreview.net/forum?id=T04ZzT5m6j), ICLR 2027 submission)
- *What it does.* Proves a bounded-utility result: with a deterministic binary judge and expected losses exceeding gains, every fixed acceptance rule that initially improves utility eventually drifts negative (◐ abstract re-read).
- *Key results* ◐:
  - Two judges with equal accuracy can give opposite signs of drift, depending on whether errors recur on the same inputs.
  - A revisions-vs-drift trade-off for adaptive rules; a sequential test.
- *Limitations* ◐: bounded-utility toy model; LM-judge experiments only "corroborate". It is the cleanest theoretical statement that *error determinism*, not accuracy, governs collapse.

**12. Adapting the Process of Adaptation: Darwinian, Lamarckian, and Agent-Directed Evolution in AI** ([2611.07048](https://arxiv.org/abs/2611.07048), 2026-11-04; Andrew W. Lo & Chaoyi Zhao; MIT Sloan / LFE / CSAIL / SFI ✓ header)
- *What it does.* Compares Darwinian, Lamarckian and agent-directed adaptation after an environmental change. It includes optimal search effort as an economic choice.
- *Key results* ✓ Abstract:
  - When learning is sufficiently effective, adaptation gets progressively faster across the three regimes.
  - Under a misspecified target, more search converges faster to the wrong behaviour while measured performance rises ✓ Thm 5 "Adaptation toward a misspecified target", §8.1.
  - 200,000 Monte Carlo simulations ✓.
- *Limitations.* Theory plus simulation, no LLM experiments ✓ grep. No error threshold or mutator effect (project bold-check).

**Remaining papers**
| id | title | one-liner | mark |
|---|---|---|---|
| [2611.10787](https://arxiv.org/abs/2611.10787) / wV4bK1dHpC | TRUST (Trustworthy Recursive Self-Improvement with Statistical Guarantees) | Online multiple testing controls the false-evolution rate. Greedy accepts 45.59% inferior updates vs 2.78% for TRUST; mean max drawdown 4.963 → 0.047 pp (HotpotQA/DROP/APPS; DeepSeek-V4-Flash, Qwen-3.8-Flash). | ✓ Abstract, §1 |
| [2610.31948](https://arxiv.org/abs/2610.31948) | Sharp Bounds on the Winner's Bias | Bias ≤ c√V. Sharp constants: c = √((M−1)/(2M)) for independent non-i.i.d. noise and c = √((M−1)/M) for pairwise nonneg-correlated noise (the latter equals the arbitrary-dependence constant). All Lean 4-verified. 448 pipelines with per-candidate items realize a median 0.75 of the independent ceiling; shared items are less optimistic. | ✓ Abstract, Table |
| [2606.08106](https://arxiv.org/abs/2606.08106) | PACE | Anytime-valid paired commit test; keep-if-better = adaptive multiple testing. | ◐ |
| [2609.36043](https://arxiv.org/abs/2609.36043) | SAGE | Statistical acceptance gate correcting the optimizer's curse and blocking regressions on solved items. | ◐ |
| [2609.33180](https://arxiv.org/abs/2609.33180) / cnaYLnpsAV | REUSE | Certified promotion under adaptive benchmark reuse. | ◐ |
| [2510.10232](https://arxiv.org/abs/2510.10232) / 0FWBc4gY08 | Statistical Gödel Machine | Replaces proofs with statistical certificates under a global error budget. | ◐ |
| [0FaPTsSr9C](https://openreview.net/forum?id=0FaPTsSr9C) | When Is Enough Enough | Anytime-valid restart detector plus change-point output selection. SearchQA/SkillOpt stops at round 4 of 40: −91.6% tokens, 82.43% vs 82.00%. | ◐ |
| [3t0j2Y05EH](https://openreview.net/forum?id=3t0j2Y05EH) | EvoLyapunov | RSI as a stochastic dynamical system. Recursive Amplification Factor; Lyapunov-drift admission; sharp stability boundary at estimated risk gain > 1 (Qwen2.5-7B, SWE-bench V and others). | ◐ |
| [cQkSamYGQi](https://openreview.net/forum?id=cQkSamYGQi) | Wright–Gödel Machine | Proves Thompson-sampling parent selection collapses onto one lineage. EXP3 with a floor gives every lineage polynomially many expansions (shifting balance). | ◐ |
| [OnzWTehIaN](https://openreview.net/forum?id=OnzWTehIaN) | Why Is RSI Hard to Verify? | Finite-horizon criterion: target gain + causal benefit of installing updates + more competent successor mechanism. Tests; 17 benchmark families coded. | ◐ |
| [iqKbDXemk1](https://openreview.net/forum?id=iqKbDXemk1) | Five Gates for Synthetic-Data RSI | Identification contract. The loop beats the trained baseline by +10.26 pp yet sits 4.12 pp below the untouched base. | ◐ |
| [2610.27610](https://arxiv.org/abs/2610.27610) | Auditing the Product, Not the Recursion | Census: 4/26 improver claims use same-start swap tests; 19% use null controls. Diagonal non-identification proposition. (Swap-test / null-control census items exist ✓ grep; counts not re-checked.) | ◐ (project) |
| [2609.36892](https://arxiv.org/abs/2609.36892) | Harness Evolution as Learning | Approximation / generalization / optimization error account of personal-agent harness limits. | ◐ |
| [2611.09701](https://arxiv.org/abs/2611.09701) | Where Should Learning Live? Regret Analysis for Text, Parameter, and Joint Adaptation | Regret = selection + family restriction + signed state effect; text vs weights. (Title ✓ header.) | ◐ (project) |
| [2609.34924](https://arxiv.org/abs/2609.34924) | Audit the Scaffold, Not the Checkpoint | Lean proof of strict diminishing returns under a bounded metric and fixed reachable edit set. | ◐ (project) |
| [2609.00137](https://arxiv.org/abs/2609.00137) | Recursive Criticality of AI Self-Improvement (Burtsev, LIMS) | R_AI = recursive gain / difficulty growth (R_AI > 1 amplifies ✓ Abstract). Shared improvements can make an ecosystem supercritical when no actor is. | ◐ |
| [2610.33447](https://arxiv.org/abs/2610.33447) | Efficiency vs Capability in the Intelligence Explosion | Efficiency returns r_E ≈ 1.07 (0.45–2.52), capability returns r_C ≈ 3.45 (0.60–8.22); SIE in 49% of Monte Carlo draws. | ✓ Abstract, §1 |
| [2610.18445](https://arxiv.org/abs/2610.18445) | The Singularity Has a Distribution | Stochastic Good process; detection limit n* ~ (12z²)^{1/3} ℓ^{−2/3}. | ◐ (project) |
| [2608.14426](https://arxiv.org/abs/2608.14426) | The Dynamics of Intelligence Explosions | Toby Ord (Oxford Martin AI Governance Initiative) growth-dynamics model. | ◐ (affiliation ✓) |
| [2611.13524](https://arxiv.org/abs/2611.13524) | The RSI Tax | Cost–benefit of RSI on continual tasks. | ◐ |
| [c3lHKtsSMq](https://openreview.net/forum?id=c3lHKtsSMq) | Governing Self-Improvement Across Timescales (AutoGovern) | Diminishing marginal value; online governor cuts tokens 38% on average (up to 72%). | ◐ |
| [2609.18560](https://arxiv.org/abs/2609.18560) | The Evolution of Sex for Artificial Intelligence (Gilestro, Imperial) | Recursive retraining reproduces Wright–Fisher exactly. The *absolute number* of real samples matters, not their share. Averaging parents = blending inheritance; merged specialists beat every parent (Fisher–Muller). | ✓ Abstract |
| [2606.05408](https://arxiv.org/abs/2606.05408) | Mutation Without Variation | LLM mutation without selection converges to attractors. (Title ✓ header.) | ◐ (project) |
| [2608.05810](https://arxiv.org/abs/2608.05810) | When Self-Evolution Backfires | "Capability–contamination phase transition": 48→60→62→52→50% as the pool grows 35→179 skills; rollback recovers 1.7 of 12.3 pts. | ◐ (project) |
| [8vq5qzUnz3](https://openreview.net/forum?id=8vq5qzUnz3) | Evade | Evolvability (offspring improvement/novelty potential) as a selection signal in LLM evolutionary search. | ◐ |
| [XoMy9wRqF1](https://openreview.net/forum?id=XoMy9wRqF1) | Plug, Play, or Re-Evaluate | Knockouts: 84% of 1,120 component removals change nothing; effects are model-specific (epistasis with backbone). | ◐ (project) |
| [71wzcahKcb](https://openreview.net/forum?id=71wzcahKcb) | LRSI | Edits toggled across sampled future versions; keeps interference-robust edits (epistasis in time). | ◐ |
| [IXnpSW8g3v](https://openreview.net/forum?id=IXnpSW8g3v) | TAME | "Capability retained after support removal"; harness rank reversal. This is a Baldwin-effect diagnostic in all but name. | ◐ (project) |
| [2603.28063](https://arxiv.org/abs/2603.28063) | Reward Hacking as Equilibrium under Finite Evaluation | Holmström–Milgrom: evaluation coverage → 0 as tool count grows; conjectured Goodhart → Campbell regime threshold. (Title ✓ header.) | ◐ |
| [Nv8es4jEv9](https://openreview.net/forum?id=Nv8es4jEv9) | Strong, Weak and Benign Goodhart's Law | Dependence-free formalization: tail dominance of goal vs discrepancy decides over-optimization. | ◐ |
| [1803.04585](https://arxiv.org/abs/1803.04585) | Categorizing Variants of Goodhart's Law | Regressional / extremal / causal / adversarial. | ◐ |
| [2210.10760](https://arxiv.org/abs/2210.10760) | Scaling Laws for RM Overoptimization | Gold-vs-proxy curves vs optimization pressure. | ◐ |
| [2610.19832](https://arxiv.org/abs/2610.19832) | Rent-Seeking Language Models | Overoptimization recast as regulator–firm rent-seeking; frequent audits curb rent. | ◐ |
| [C6EElQ2PRG](https://openreview.net/forum?id=C6EElQ2PRG) | Feedback Alignment in Self-Improving Agents | Scaffold evolution raises the visible score V while lowering the held-out target T. | ◐ |
| [2609.32677](https://arxiv.org/abs/2609.32677) / Ah9eaA746Q | Improvement Fidelity | Whether proxy-verifier gains keep sign and order in adaptive worlds; decision-aware validator. | ◐ |
| [2607.04277](https://arxiv.org/abs/2607.04277) | Introspection Threshold for RSI | Kleene-recursion self-improving programs; equivalence to the Gödel machine; LLMs are "quasi-introspective". | ◐ |
| [2606.28639](https://arxiv.org/abs/2606.28639) | Algorithmic Unverifiability of Safety | Safety unverifiable for fixed and RSI systems (cites 2510.04399). | ◐ |
| [2611.22299](https://arxiv.org/abs/2611.22299) | Harnesses That Improve Themselves: A Unifying Framework | Unifying 4-component × 4-stage framework; argues full-harness rewrites overfit or break. (Title ✓ header.) | ◐ |
| [2607.22036](https://arxiv.org/abs/2607.22036) | Runtime Analysis of RL Hyper-Heuristics | Classical runtime theory for selection hyper-heuristics. Untouched by the LLM-RSI literature. | ◐ |
| [2609.19799](https://arxiv.org/abs/2609.19799) | Evolution or Illusion? | LLM evolutionary search ranked on single seeds/budgets is unreliable. | ◐ |
| [LhhLpXBFgo](https://openreview.net/forum?id=LhhLpXBFgo) | Opinionated Optimizers | 3M rollouts / 216 meta-optimization runs; why meta-optimized harnesses overfit. | ◐ (project) |
| Classical | Price (1970); Eigen (1971); Hinton & Nowlan (1987); Wolpert & Macready (1997); Schmidhuber (2003); Kashtan & Alon (2005); Smith & Winkler (2006); Lynch drift-barrier hypothesis; Andrychowicz et al. L2L (2016); Li & Talwalkar NAS random search (2019) | Imported concepts; no numbers used. | † |

### Consensus, debates & contradictions
- **Consensus: the loop's self-report is optimistic.**
  - 2610.09239: 13–20 pt gap at n = 16 ✓.
  - 2611.23068: selection at chance ✓.
  - 2611.10787: 45.59% inferior accepts ✓.
  - 2609.08175: 2/96 validated despite 26–39% meeting the audit target ✓.
  - The shared prescription is fresh or audit sets plus uncertainty reporting (2610.09239 ✓ §6: "report a held-out score and its standard error").
- **Consensus: the evaluator must be outside the thing that evolves.** GAI Dial 2 ✓, the Kinetic separation theorem ✓, and Rice-style limits ✓ say the same thing in three vocabularies.
- **Debate: does better gating fix it?**
  - TRUST (✓ abstract numbers), SAGE, PACE and EvoLyapunov (◐) claim gains from principled gates.
  - 2610.09239 finds the tested rules did *not* beat greedy over whole runs on reused sets. It suggests, without isolating the mechanism, that incumbent inflation ("lock-in") "may make greedy more conservative" and act as an implicit threshold ✓ §3.3, §6.
  - 2611.23068 says the problem is resolution, not the rule, and the fix is more evaluation.
  - These are compatible if gate papers' wins come mainly from accepting less. 2610.09239 reports exactly that for calibrated thresholds on isolated fresh decisions ("mainly by committing less often", ✓ §6), but no head-to-head under a matched evaluation budget exists.
- **Contradiction: where stalls come from.**
  - 2609.08175's DS-1000 illustration and 2611.23068 locate the stall in validation (candidates exist).
  - 2610.03243 Prop. 4.3 allows either proposal or information exhaustion ✓.
  - Round-1/2 project evidence (Harness Evolution Hits a Ceiling; "opinionated optimizers") points at the proposer.
  - The answer is probably task- and noise-dependent; nobody maps it.
- **Contradiction: is the loop's stationary law predictable?**
  - 2611.09699 confirms the direction of the tilt but fails quantitatively, and its registered test fails in the second domain ✓ Abstract.
  - The "lessons from luck" and kinetic papers assume Markov structure that reused selection sets violate (2610.09239 Prop. 3 holds only for fresh sets ✓).
- **Analogy inflation.** Kinetic "fluids", the "Maxwell daemon", "Boltzmann tilt", "shifting balance" and "the evolution of sex" are often bounded analogies. The Kinetic paper admits there is no Navier–Stokes limit ✓. The Self-Improvement Problem concedes its near-equivalence to Rice ✓. Reviewers will discount pure analogy without a falsified prediction.
- **Economics vs agent-level science disconnect.** Macro models need ε_{R,C} (2609.15802 ✓ "almost no evidence"). Agent-level papers measure order-0 gains, while the elasticity needs order-1 data. In our reading, the economics and agent-level communities do not cite each other.

### Open problems
1. **A two-level (meta) winner's curse.** Derive and measure the bias when *improvers* are selected by downstream gains on few meta-tasks. Every result above is object-level (2610.09239 ✓ §6 disclaims RSI). Our round-1 critique found no paper that does it.
2. **A drift barrier for editor code.** An improver-region defect affects the gate only through later edits, so selection on it is second-order. The predicted law is retained editor-defect rate vs gate resolution σ/√n. Our searches found no application of Muller's ratchet or the Lynch drift barrier to self-referential agents (project bold-check; 2610.09239 only names the drift barrier as related work ✓ §2; the 2608.05810 knee is only qualitative).
3. **Price-equation decomposition of archive progress.** Split per-generation change into a selection covariance term (parent choice) and a transmission term (edit effect). Same-start swap tests (2610.27610 ◐) estimate only transmission. The Price equation gives the exact accounting, and our ICLR 2027 search for it returned zero hits.
4. **Capacity proxies for prompt/harness edits.** 2510.04399's iff boundary needs a computable reachable-envelope measure that composes (✓ §10–11). Candidates are description length, number of tools, and branching factor of harness code. Untested.
5. **Order-1 sample complexity.** How many nested-replay states R and meta-tasks are needed to detect an improver gain Δ₁? Extend 2611.23068's N formula (✓ Eq. 2) and 2609.08175 Thm 5 to two-level variance.
6. **A real elasticity from controlled loops.** Estimate ε_{R,C}-style returns on a log-cost capability scale with off-diagonal (improver × state) designs, with power analysis (2610.18445). Bridges H and B.
7. **Kashtan–Alon test.** Do modularly varying goals select for self-editing or modular harnesses? Our ideas #7 and #9 are this experiment. No 2026 paper runs it (round-3 note).
8. **A Baldwin trade-off law.** When does a harness "assist" (accelerate weight learning) vs "hide" (shield weights from selection)? TAME and Harness Annealing measure symptoms without a model (project notes).
9. **Deterministic-error measurement.** T04ZzT5m6j says error recurrence, not accuracy, decides the sign of drift. Nobody has measured judge-error determinism for real agent gates.
10. **A unified gate benchmark.** TRUST vs SAGE vs PACE vs REUSE vs EvoLyapunov vs greedy-with-audit at matched evaluation tokens, scored on held-out targets over whole runs.

### Where it is heading (to mid-2027)
- **ICLR 2027 decisions (late Jan 2027)** will decide whether "theory + small illustration" papers clear the bar: NxGP6PGiaU, 23VDi5bfRj, T04ZzT5m6j, cQkSamYGQi, 3t0j2Y05EH, OnzWTehIaN. Expect reviewers to demand cross-domain predictive tests, as 2611.09699 attempted.
- **"Resolution first" becomes a reporting norm.** Expect multi-run variance (τ², σ²) and held-out audits to be required at NeurIPS 2027. 2611.23068 and 2610.09239 give the formulas.
- **The theory moves up one level**, to lineage, meta-selection and editor integrity. Kinetic and GAI explicitly leave this open, and it is the natural next ICLR/NeurIPS 2027 theory target. Expect scoop pressure from the Winner's-Curse and Reliable-Self-Evolution groups.
- **The economics community will look for micro-estimates.** METR / Elasticity Institute wish-lists invite lab-side or academic controlled-loop elasticity papers by mid-2027.
- **Evolutionary-theory imports will keep growing.** Shifting balance, evolvability, Wright–Fisher and Fisher–Muller are in; Price, drift barrier and modularly varying goals are next. The papers that survive will be those with a falsifiable quantitative law, not a metaphor.

### Connection to our ideas (ONE_PAGERS.md)
- **#3 Gate, Not Willingness / 2+3 merged (Propose, Propagate, Pay Off).**
  - 2609.08175's reachability diagnostic is a theoretical twin of our funnel. P̂_in/P̂_out (Thm 6 ✓) separates generation-limited from validation-limited stalls.
  - 2610.03243 Prop. 4.3 ✓ separates proposal from information exhaustion.
  - We must cite both and position as the *self-edit-specific* funnel under real gate types, including delayed-credit gates. Their theory is object-level only.
- **#2 Forced-Quota Dose.** The drift-barrier view (open problem 2) predicts the inverted U. Forced self-edits raise the volume of sub-resolution editor defects. The editor-probe suite from the round-2 bold-check adds a mechanism measurement.
- **#4 Order-2 Audit.**
  - The theory backbone is OnzWTehIaN's "more competent successor mechanism" criterion ◐ and GAI Dial 1 ✓.
  - Power is the risk: 2611.23068's N formula ✓ shows 3.4 pp margins need about 5–23 runs per method depending on benchmark (✓ §4.1). So the audit's "undetectable" claim should come with an explicit minimum detectable effect.
- **#5 Order-0 Rewards Breed Routers.** This is Holmström–Milgrom multitask moral hazard (2603.28063 ◐) and regressional/causal Goodhart (1803.04585; 2610.09239 Prop. 3 ✓) applied to meta-rewards. Using that framing turns an empirical comparison into a prediction: the unevaluated "improver skill" dimension gets under-invested.
- **#6 Meta-Overfitting.** 2510.04399 ✓ maps it directly: improver self-edits enlarge the reachable envelope, and adaptive reuse of meta-tasks breaks the validation guarantee (✓ §5 "Validation reuse, fixed ex ante"). Cite as the formal motivation; adds open problem 1, the meta winner's curse.
- **#7 Varying Goals / #9 MetaShift.** This is literally the Kashtan & Alon modularly-varying-goals test † plus evolvability (Evade ◐) and Lo & Zhao's environmental-change model ✓. The Baldwin framing (TAME) is the nearest scoop risk.
- **#8 Openness Ladder.** This is the capacity-cap question of 2510.04399 (bounded vs unbounded reachable envelope) and the NAS lesson that search-space design dominates search algorithm †. An interior optimum is consistent with a bias–variance trade-off of the reachable envelope against gate resolution.
- **#1 Taught, Not Selected.** T04ZzT5m6j's deterministic-error theorem is close: errors that recur on the same inputs get reinforced. Our "reading channel" is a *causal* Goodhart route, where the trace carries the error and no score is gamed. That distinguishes it from the regressional route all the selection-statistics papers model.
- **#10 Register Snowball (Claudish).** 2609.18560 ✓ (Abstract) shows recursive retraining is Wright–Fisher drift and that the *absolute* number of real samples sets the drift. This is a ready-made quantitative prediction for register-drift experiments.

#### Fact-check log
| Claim | Verdict | Source location |
|---|---|---|
| 2611.23068 selection numbers: 49.7% pairwise, 30.5% vs 62.1%, ρ = 0.054, native 48.6% | confirmed | 2611.23068 §5.2 text + table (paper.txt l.365–440) |
| 2611.23068: Quality@K 49.2→61.8%; median margin 3.4 pp, IQR 1.2–6.6, 171 results / 9 papers; SD 1.7–4.1; best-of-4 picked 51–73%, 33% TB | confirmed. Added detail: one-sentence rule recovers median 90% of gain across 27 pairs | §4.1, §4.2, §5.1, App. D.2 |
| 2611.23068 "candidate-level evidence is from Terminal-Bench only" | corrected: Selection Pool also uses DeepSWE trajectories (11 tasks) | Limitations (l.569–571) |
| 2610.09239 setup "Qwen2.5 1.5B/7B/14B" | corrected: solver = proposer 1.5B/7B by default; 14B and Qwen3.5-4B as stronger proposers; Qwen3.5-4B + Qwen3.8-27B current-model study | §5 Setup (l.257), §5.1 (l.313), §5.4 (l.494) |
| 2610.09239: 13–20 pts at n = 16 / 1–5 at n = 256; 4–39% improve; −0.2 to −13.5; 1.53e; MAE 0.034; audit RMSE 15.5→5.7; Prop. 3 | confirmed; MAE claim made precise (ratio of selection differentials, nine settings) | Abstract; §3.1; §3.3 Prop. 3; §5.1; §6 |
| 2610.09239 lock-in "explains" thresholds failing | corrected to hedged wording ("may", mechanism not isolated); added "calibrated thresholds help mainly by committing less often" | §3.3, §6 |
| 2510.04399: TMLR 08/2026, Columbia, Thm 1–2 (§5), Prop. 9, Thm 12 / Prop. 13, no experiments | confirmed; section numbers added (§8, §9); toy example in §4 noted | header; §4, §5, §8, §9 |
| 2510.04399 limitation "VC poor fit for prompt edits" presented as self-stated | corrected: this is our critique; self-stated limits are i.i.d., axis isolation, composability of proxies | §10 |
| 2609.08175: Thm 1 / Cor 1 / Cor 2 / Thm 5 (Z/(10u)) / Thm 6; DS-1000 2/96, 0/96, 0/96; 26–39% (0.260–0.385) | confirmed; 26–39% noted as point fractions with wide CIs; "25+ propositions" made precise (25 numbered) | §3, §4.1, §4.2, §5.1, Table 1 (§5.2); ICLR id NxGP6PGiaU via OpenReview search |
| 2609.13406: 58.33% / 78.26%; Table 4 four defects; "prose rather than a proposition"; v2 29 Oct | confirmed | §5.1, Table 4, §6, header |
| 2609.13406 "only 2 S2 citers" | corrected: ≥3 citing preprints in cache (2610.27610, 2610.29677, 2611.22299); S2 now returns 0 | reference lists (grep); S2 API 2026-11-18 |
| 2609.15802: ε_{C,A} ≈ 6.5, threshold 0.15, ≈9% with 4× uplift, "almost no evidence" | confirmed; section corrected from §5 to §4.1 | §1, §4.1 |
| 2609.15802 "unit non-invariance makes threshold depend on capability scale" (our critique) | corrected: the paper says the loop gain ε_{R,C}·ε_{C,A} is invariant; only the individual magnitudes and threshold are normalization-dependent | §3.4 fn. 9 |
| 2610.03243: Thm 4.1 V₀/(p_min Δ); Prop 4.3; β = −1/2 only at tripled density; Thm 4.5; no Navier–Stokes | confirmed; "excludes monolithic self-modifiers" upgraded ◐→✓ | Abstract, §1–2, §4.3 |
| 2610.21646: Thm 1–3, Prop 1–2; "close to a restatement of Rice's theorem" | confirmed | Abstract; Remark after Thm 1 (l.271) |
| 2610.18813 "66-page preprint" | corrected: arXiv PDF is 18 pages; a 66-page extended version is advertised on GitHub | pdfinfo; Abstract |
| 2610.18813 admission rule "Δ̂ + 2√(ln(KT/δ)/m) + 2η < 0" | corrected to Δ̂ + √(2 ln(KT/δ)/m) + 2η < 0 (Eq. 9; Eq. 8 without η) | §4.2 Eq. 8–9 |
| 2611.09699: β = 4F′(0), √minibatch, 6/7, 62–88%, 109–119%, 300 steps, prompt-opt test failed | confirmed (◐→✓); reversibility condition (logistic difference, reversible mutation) added | Abstract, §3 |
| 2611.07048: Thm 5 misspecified target, 200,000 simulations, authorship | confirmed; "Lo et al." corrected to Lo & Zhao (two authors) | header, Abstract, §8.1, App. |
| 2610.31948 constants "(M−1)/(2M)" | corrected to √((M−1)/(2M)) (independent) and √((M−1)/M) (nonneg-correlated); 448 pipelines / 0.75 / Lean 4 confirmed | Abstract, Contributions, comparison table |
| 2611.10787 TRUST 45.59% vs 2.78%; datasets and models | confirmed (◐→✓) | Abstract, §1 |
| 2610.33447 r_E ≈ 1.07, r_C ≈ 3.45, 49% SIE | confirmed (◐→✓) | Abstract, §1 |
| T04ZzT5m6j deterministic-judge drift theorem | confirmed at abstract level only | OpenReview search (ICLR.cc/2027) |
| ID → title resolution: 2609.08175, 2610.09239, 2611.23068 (+ 2610.31948, 2611.10787, 2609.18560, 2610.33447, 2603.28063, 2609.00137, 2606.05408, 2611.22299, 2611.09701, 2608.14426) | confirmed: all match stated titles | arXiv PDF headers in cache |
| "Nobody applies Price / drift barrier / Kashtan–Alon" and "no paper estimates elasticities in agent loops" | unverifiable as universal negatives; reworded to "our searches found none" | project searches; 2610.09239 §2 mentions drift barrier only as related work |

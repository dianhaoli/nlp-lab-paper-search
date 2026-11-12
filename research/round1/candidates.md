# Round 1 candidate pool (breadth-first synthesis, 2026-11-12)

Sources: Dan's ideas (context/ideas_so_far.md), extra leads, reading list, and the 8 sweep reports in sweep/. Near-duplicates across sweeps have been merged. Every candidate states a falsifiable headline claim; for most of them, the opposite result is also publishable.
Origin tags: **Dan-original** (Dan's idea, sharpened), **Dan-modified** (Dan's idea, substantially reframed or merged after the sweep), **new** (from sweep gaps or 2026 trends).

Cross-cutting facts that shape every candidate:
- Auditing the Product (2610.27610) formalizes the swap test (C4 improver gain, C5 compounding). It finds only 4 of 26 improver-claiming papers run a same-start matched-budget swap, and its Proposition 3 shows that improver effects cannot be identified from trajectories alone.
- Dan's confound battery (#2) is scooped at the task level (HDA 2610.22466, Beneath the Surface 2611.07624, Winner's Curse 2610.09239, RSI Tax 2611.13524, Frozen-Weights 2611.11342, Rethinking Eval 2607.12227). It survives only as a control suite or when moved to the improver level.
- Plasticity is claimed only at the object level (Agent Plasticity 2610.08902, 2610.25858, 2611.03219). Plasticity of the improver is unclaimed.
- New benchmarks are saturated (RSIGym, RSI-Forge, RSI-Env, Flex-RSI, SelfImprove-Bench, RSI-Dojo, ...). Treat the procedural tau-style generator as infrastructure shared by all candidates, not as a contribution.

---

## Topic T1: Improver scaling and meta-level ability
Does improving the improver help? This topic separates the improver from the state, from compute and from memory, and it is closest to the Gödel Agent line.

### C1 `swap-factorial`: Is it the improver or the state? A powered factorial swap test of recursive self-improvement
- **Question:** When a self-modifying meta-agent appears to get better at improving, how much of the later gain comes from the improver, from the solver state it is applied to (headroom), and from extra compute?
- **Claim:** Across 3 meta-agent families at matched budget, the improver main effect is less than one third of the apparent trajectory gain, and its slope over generations has a CI that includes 0 (C5 compounding fails). "Improving at improving" in trajectories is mostly a state, headroom and selection artifact. A positive slope would be the first powered evidence of compounding RSI.
- **Experiments:**
  - 3 families: Gödel-Agent/Hyperagents-style self-editing, FreeEvolve/Skill-infinity-style meta-skill, and AIDE/VERSE-style optimizer.
  - 2 benchmarks: the procedural tau generator plus a TB2 or Polyglot subset. Run 40 generations.
  - Cross improver checkpoints I{0,5,10,20,40} with solver states S{0,5,10,20,40}, 3–5 seeds per cell, at a fixed inner budget, scored on fresh audit tasks.
  - Comparators: I0 given the meta-evolution tokens (spent as best-of-N or longer search), and the SRN bounded-note baseline.
  - Analysis: a two-way model with an interaction term, plus a slope test.
  - Re-run the Hyperagents transfer as a proper same-start case.
  - Section: randomized-target directedness e-values (as in 2610.14662) on meta-edits.
  - Models: Qwen3.5-9B and a Haiku-class model.
- **Closest prior:** 2610.27610 (Prop. 3); RSIFlow 2610.21791; FreeEvolve 2610.09197; AIDE² 2609.26457; MetaRSI 2609.06396; Flex-RSI 2611.08946; SelfImprove-Bench 2611.02797.
- **Origin:** Dan-modified. Merges #1 improver scaling, the ignition test, compute vs search vs meta-learning, and the Hyperagents replication.
- **Notes:**
  - Cost: $600–1.5k; a 1-family 3×3 pilot costs under $60.
  - This is the most urgent candidate. The framing is public, so the contribution is powered execution plus the factorial. Risk of a fast follow from the 2610.27610, RSIFlow or Weco groups. Open models may not drive Hyperagents.

### C2 `procedure-vs-memory`: Procedure or Memory? Transplant controls for self-referential improvers
- **Question:** Is a later improver's advantage a better procedure, or accumulated task-distribution knowledge that any procedure could exploit?
- **Claim:**
  - Transplanting gen-k memory into the gen-0 procedure recovers at least 70% of the improver gain.
  - The memory-wiped gen-k procedure keeps at most 30% of it.
  - Cross-domain swaps erase it.
  - Today's "learning to improve" is "learning the benchmark". If the procedure share dominates instead, that is the first clean evidence of procedural meta-learning.
- **Experiments:**
  - A 2×2 design {I0, Ik procedure} × {empty, Ik memory/archive}, plus length-matched placebo and shuffled-memory arms.
  - Run in-domain and cross-domain.
  - Report a Shapley split into procedure, memory and interaction, and track the procedure share over generations.
  - Add the R2 frozen-knowledge control.
  - Reuse the C1 checkpoints.
- **Closest prior:** Physical-AI RSI R1/R2 2610.29677 (design only); Continual AutoResearch 2611.05108 (actor-level card ablation); Lineage Is Memory 2610.41157 (task-level); FreeEvolve no-data rewrite; Hyperagents 2603.19461.
- **Origin:** Dan-original (procedure-vs-memory arm).
- **Notes:**
  - Cost: $300–800. Strongest as a section of C1.
  - Before committing, check the alphaXiv-only paper "When, Where, and How Experience Becomes Structure".

### C3 `rsi-elasticity`: The RSI feedback loop in miniature: measured returns-to-improvement elasticities
- **Question:** In controlled self-improvement loops, what is the elasticity of the improvement rate with respect to the improver's own accumulated improvement at fixed compute?
- **Claim:** Across 3 systems, the self-feedback elasticity is well below 1 (diminishing returns). It is lower on held-out and OOD tasks than on search tasks. Today's RSI loops are not self-sustaining even in miniature. These are the first empirical micro-estimates that the RSI-economics models (METR/Epoch, GovAI) explicitly ask for.
- **Experiments:**
  - Give improver checkpoints g0/10/20/40 the same fresh starting artifact and fresh procedural tasks at a fixed budget.
  - Fit log(gain rate) against log(accumulated improvement), with bootstrap CIs over at least 5 seeds.
  - Report narrow (search-set) and broad (held-out) elasticities, and their sensitivity to budget.
- **Closest prior:** Economics of RSI 2609.15802; GovAI 2609.36054; AIDE² ignition test; Hyperagents imp@k.
- **Origin:** new (reframes C1 around a policy-relevant estimand).
- **Notes:**
  - Cost: $300–1k, mostly shared with C1.
  - Risk: alone it may read as a reanalysis. As the headline estimand of C1, it adds a broad-audience hook.

---

## Topic T2: Meta-agent plasticity
Do improvers lose the ability to improve, why, and can it be restored? This applies Agent Plasticity at the meta level.

### C4 `meta-plasticity`: Do self-improvers lose the ability to improve? Meta-level loss of plasticity and its restoration
- **Question:** Does a self-modifying improver's meta-plasticity decline over generations as it accumulates code, notes and rules? Meta-plasticity here is P_meta, held-out improver gain per $ on fresh states or domains. A second question: after k rounds on domain A, does a warm improver learn domain B worse than a fresh one?
- **Claim:**
  - P_meta peaks within about 10 generations and then declines, tracking improver complexity. Gödel Agent's complexity ceiling is a loss-of-plasticity phenomenon.
  - Beyond about N rounds, a warm improver underperforms a fresh one on a new domain at matched budget (a warm-start penalty).
  - Prune or distill-and-reset interventions restore at least half the lost plasticity.
  - Positive forward transfer (learning to learn) would be equally publishable.
- **Experiments:**
  - 2–3 families, 40–60 generations, 3 seeds.
  - At checkpoints k ∈ {0, 2, 5, 10, 20, 40}, run a Dohare-style probe: give a fixed budget on probe family B and compute PR(k) = gain_B(warm)/gain_B(fresh), plus learning-curve AUC.
  - Regress on complexity (tokens, rules, tools) and on reachable-edit-set size, which tests Audit the Scaffold.
  - Interventions at matched compute: prune-to-budget, distill-and-reset notes, shrink-only edit rounds, memory reset vs procedure reset, utility-tracked re-initialization, and selectionist vs instructionist consolidation.
- **Closest prior:** Agent Plasticity 2610.08902; Are Self-Evolving Agents Continual Learners 2610.25858; 2611.03219; Audit the Scaffold 2609.34924; Gödel Agent 2410.04444; Do Agent Optimizers Compound 2607.14004; Dohare et al. 2024.
- **Origin:** Dan-modified (the meta-agent plasticity line plus the Gödel complexity ceiling). This idea was independently proposed in 6 of 8 sweeps.
- **Notes:**
  - Cost: $600–1.5k, sharing infrastructure with C1.
  - Avoid the "stability-plasticity tradeoff" framing, which 2610.25858 owns; use improvability vs reset instead.
  - This is the best fit with Dan's stated interest and Xunjian's long-horizon agenda.

### C5 `context-debt`: Context debt: do meta-agents plateau because they anchor on their own history?
- **Question:** Is the late plateau of harness-evolving meta-agents caused by the improver anchoring on its accumulated conversation, archive or notes, or by genuine saturation of the target? Which state component carries the effect?
- **Claim:** At matched compute, resetting the conversation and unverified notes while keeping the verified scored archive recovers most late-stage held-out gain. Reported harness-evolution ceilings (HarnessX post-peak collapse, the "Ceiling" paper) partly measure improver anchoring, not the harness's limit. No recovery would support the saturation reading.
- **Experiments:**
  - Evolvers: GEPA-style, Meta-Harness-style, and a Gödel-style self-modifier.
  - Factorial keep/drop over conversation × scored archive × unverified notes, plus fork-and-flush and forget-to-best arms.
  - Compare an anytime-valid detector (2610.04756) as reset trigger against a fixed schedule.
  - Measure edit-size and novelty decay, false-floor declarations, and held-out gain per $. Use 5 seeds.
- **Closest prior:** Scaling Discovery Requires Forgetting 2610.20563; the verified-archives rebuttal (alphaXiv-only); Fork-and-Flush 2610.07447; HarnessX 2606.14249.
- **Origin:** new (autoresearch-dynamics trend transferred to meta-agents).
- **Notes:**
  - Cost: pilot $30–50; full $600–1.2k.
  - Natural mechanism section for C4. The trend is hot, so a fast follow by the forgetting-paper authors is plausible.

### C6 `gate-dial`: The acceptance gate is the stability-plasticity dial of self-improving agents
- **Question:** How does gate strictness (greedy, paired α, held-out gate, REUSE/PACE/TRUST-style statistical gates, always-accept, margin-aware) trade off long-horizon plasticity, forgetting and winner's-curse overfitting?
- **Claim:** The gate ranking flips with horizon. Strict statistical gates win at 10 rounds but freeze the workspace and collapse plasticity by 100+ rounds. Lax gates cause forgetting. A noise-adaptive gate dominates both.
- **Experiments:** About 8–10 gate rules × 2 evolvers on a long procedural stream (hundreds of family switches). Measure freeze rate, PR(k), backward forgetting, selection-vs-audit inflation, and held-out gain, with 3 seeds.
- **Closest prior:** 2610.25858 (gate freezing as a side remark); Winner's Curse 2610.09239; PACE 2606.08106; REUSE 2609.33180; TRUST 2611.10787; Masked Erosion 2610.38392.
- **Origin:** new.
- **Notes:**
  - Cost: $400–800.
  - Risk: reviewers may see a "hyperparameter study". The horizon-dependent ranking flip is the necessary surprise.

---

## Topic T3: Evaluation rigor and Goodhart at the meta level
These candidates move the scooped task-level confound battery up to the improver.

### C7 `meta-winners-curse`: The winner's curse one level up: selection noise in meta-selection and open-ended archives
- **Question:** How much do meta-selection scores and DGM/HGM-style archives overstate improver quality? Do metaproductivity proxies predict the true improver effect?
- **Claim:**
  - With fewer than about 10 meta-tasks, meta-selection overstates improver gain by more than 2× the held-out swap gain.
  - Placebo-mutation archives (no-op or semantically null edits) reproduce at least 40% of archive gains at a matched evaluation budget.
  - HGM CMP and parent-improvement-potential proxies correlate weakly (ρ < 0.3) with swap-test improver gain.
  - A meta-level held-out split plus shrinkage removes most of the bias.
- **Experiments:**
  - Generate many improver revisions per generation and score each on its selection meta-tasks vs a large held-out pool, using paired swaps.
  - Run small archives with real vs placebo operators on an open model.
  - Re-analyze public DGM/HGM logs.
  - Fit a two-level winner's-curse model and validate the proxies.
- **Closest prior:** Winner's Curse 2610.09239 (task-level, no agentic/archive); HGM 2510.21614; Trace Evolution Bench 2610.15055; Beneath the Surface 2611.07624; Third Eye 2611.12988.
- **Origin:** Dan-modified. Moves the confound battery #2 to the meta level, merged with the sweep's "placebo archives" idea.
- **Notes:**
  - Cost: $400–1k; the placebo-archive part alone costs $200–600 and is the most surprising piece.
  - Could be a section of C1.

### C8 `learning-to-goodhart`: Learning to Goodhart: do evolved improvers become transferable shortcut seekers?
- **Question:** Do self-evolved meta-agents acquire a propensity to find and exploit evaluation artifacts that transfers to new benchmarks? How does exploitation scale with artifact strength?
- **Claim:** On fresh procedural tasks with planted shortcuts, gen-k improvers exploit them faster and more often than gen-0, with a dose-response in artifact strength. Evaluation hacking is learned at the meta level and carries over.
- **Experiments:**
  - Use the procedural generator with planted artifacts at strength p: answer-position bias, leaked git history, an exploitable grader regex, and BreakingWeb-style spoofable success signals.
  - Measure time-to-exploit and exploit rate vs p, budget and generation.
  - Use HDA-style neutralization to separate exploitation from real gain.
  - Use 3 seeds, on C1 checkpoints.
- **Closest prior:** HDA 2610.22466; DGM/Hyperagents security audit 2609.17817; BreakingWeb 2609.35814; Masked Erosion 2610.38392; DGM objective hacking.
- **Origin:** new.
- **Notes:**
  - Cost: $300–900.
  - Safety-relevant, with a strong headline if positive. A null result ("no transfer of hacking") is weaker but reportable.

### C9 `improver-erosion`: Masked erosion of the improver: do self-modifying improvers delete their own checks?
- **Question:** When an improver can edit its own verification layers, do score-based meta-gates let it delete redundant checks, and does that later cause Goodhart failures?
- **Claim:** Within about 10 generations, greedy and paired meta-gates accept deletion of redundant verification layers (the deletions are masked, so the score is unchanged). Verification is 75–94% of RSI cost, so there is pressure to cut it. This raises downstream false acceptance and held-out regressions. A margin-aware meta-gate prevents it at small cost.
- **Experiments:** VERSE-style improvers seeded with 3–4 redundant verification layers, under greedy, paired and margin-aware gates. Measure layer survival, the solver's false-acceptance rate and held-out gain, with 2 models and 3 seeds.
- **Closest prior:** Masked Erosion 2610.38392 (harness safeguards only); VERSE 2610.02616; Compositional Safety 2609.33123; RSI Tax 2611.13524.
- **Origin:** new.
- **Notes:**
  - Cost: $300–800.
  - Narrower scope with a safety-venue flavor. Could pair with C8 as "how meta-agents Goodhart".

---

## Topic T4: Curricula and task generation for harness learners
This topic continues Dan's current lead (#3).

### C10 `saturation-law`: More tasks, no gain: the data multiplier and saturation law of harness learners
- **Question:** For frozen-weight learners, how does held-out performance scale with the number of real tasks n and self-generated tasks m at a matched rollout budget? What sets the saturation point, and how much of the gain is fresh-instance memorization vs held-out generalization?
- **Claim:**
  - Harness learners saturate at n* of a few dozen real tasks.
  - n* is set by the optimizer's evidence bandwidth (failures seen per step), not by generator quality.
  - Self-generated tasks have n_eff/m ≥ 1 only below n*.
  - Adaptive curricula help by selecting tasks, not by adding them.
  - Most accepted gains are S_seen, not S_held.
- **Experiments:**
  - Use a procedural tau-style world with a known latent rule graph.
  - Vary n ∈ {5..200} real tasks against static, adaptive and anchored generated tasks.
  - Learners: 3 evolvers (GEPA, ACE/DAEDALUS-style memory, Meta-Harness-lite) × 2 open models.
  - Manipulate evidence bandwidth k. Use at least 5 seeds and matched rollouts and tokens.
  - Report S_real, S_gen, n_eff and per-rule coverage, plus a Continual-ARC-style seen/held split via fresh-instance re-serving.
- **Closest prior:** SSR 2610.28815; DAEDALUS 2610.08048; HERA 2610.06563; DarwinX 2608.07545; Continual-ARC 2611.14505; Meta FAIR 2610.38882.
- **Origin:** Dan-modified (#3 data multiplier + #5 anchoring + infra D + the frontier sweep's memorize-vs-generalize idea).
- **Notes:**
  - Cost: $600–1.5k; the noise-floor pilot is about $20.
  - #4 task-selection baselines are kept only as baselines (scooped by ActiveSaddler).
  - The "generated tasks are too easy" null is now expected, so the evidence-bandwidth mechanism is the needed surprise.

### C11 `curriculum-goodhart`: Curriculum Goodhart: when a meta-agent writes its own tasks
- **Question:** When a self-improving meta-agent edits both its harness and its task writer, does the gap between self-task score and ground-truth held-out score grow over generations, and what drives it?
- **Claim:** Proxy score rises monotonically while ground-truth performance peaks (around gen 10) and then declines. The cause is drift toward harness-solvable, unrealistic tasks. At most 5% real-task anchoring flattens the curve.
- **Experiments:**
  - A procedural world with exact ground truth.
  - Arms: fixed real, static generated, adaptive frozen writer, unified (Hyperagents-style self-written tasks), and unified + anchoring.
  - 20–40 generations with seeds.
  - Track the proxy-truth gap, validity, diversity and distance-to-real, and link them to the winner's-curse model.
- **Closest prior:** HERA 2610.06563; RSI-Env 2611.01736; RSIAgent 2609.15364; Anchored Self-Play 2607.03523; Meta FAIR 2610.38882; DarwinX.
- **Origin:** Dan-modified (#3 unified arm + #7 co-evolutionary Goodhart, done with frozen weights so no GPU is needed).
- **Notes:**
  - Cost: $800–1.5k API.
  - This is Hyperagents' stated future work and fits the Gödel Agent lineage. Threat: False Frontiers 2609.39102 (co-cheating).

### C12 `teacher-substrate`: Same teacher, different learner: do curriculum principles transfer from weights to harnesses?
- **Question:** Do teachers that win for gradient learners (frontier band, learning progress, hint-regret) also win for harness learners?
- **Claim:** There is a significant teacher × substrate interaction, and the ranking flips. Frontier-difficulty and learning-progress teachers are best for LoRA-GRPO but among the worst for GEPA/ACE learners, which need rule coverage and contrastive pairs.
- **Experiments:** 5 teachers (random, frontier band, LP bandit, hint-regret, coverage ledger) × 2 learners (a harness on a frozen 8B model vs LoRA-GRPO on the same 8B model), with matched teacher compute and 5 seeds.
- **Closest prior:** SPADE 2608.19197; ActiveSaddler 2610.00906; GRADUS 2610.27781; DEO 2609.34279.
- **Origin:** new.
- **Notes:**
  - Cost: about $500 API plus 200–400 GPU-hours.
  - Less tied to the mentor's agenda than other candidates. The ranking flip is a clean surprise.

---

## Topic T5: Harness ↔ weights and model upgrades

### C13 `harness-half-life`: Harness half-life: do evolved harnesses survive model upgrades?
- **Question:** When a harness evolved for model v_n is carried to its same-family successor v_{n+1}, what fraction of the gain survives, what fraction is absorbed, and what fraction turns harmful? Should one keep the harness, warm-start re-evolution, or cold-start it?
- **Claim:** Most evolved-harness gain is model compensation that the next model absorbs, and some of it turns harmful. Only edits that change what the model sees survive upgrades. Warm-start re-evolution is no better than cold-start at matched compute.
- **Experiments:**
  - Model ladders: Qwen3 → 3.5 → 3.6, a size ladder, and one other family.
  - Meta-Harness, AHE and GEPA-style evolvers on 2–3 low-artifact benchmarks, at least 3 seeds.
  - Measure retained gain and absorption fraction, with edit-level ablations tagged by the Ceiling paper's process/content taxonomy.
  - Run an HDA overfitting audit and keep/warm/cold arms at a matched budget.
- **Closest prior:** Finding the Right Fit 2610.00917; MaD-RSI 2610.28201; Optimize Cheap Deploy Strong 2608.10694; Memory Portability 2609.05339; Ceiling 2610.11655.
- **Origin:** Dan-modified (survival under model upgrades).
- **Notes:**
  - Cost: $800–1.5k API with no training.
  - Medium scoop risk. Practical "bitter lesson for harnesses" appeal. Weaker fit with the meta-agent theme.

### C14 `safeguards-internalization`: Do safeguards survive internalization?
- **Question:** When a harness containing safety rules is distilled into weights, do the safeguards persist, especially redundant or masked ones? Can harness-level audits still detect violations?
- **Claim:** Task behaviors internalize, but masked safeguards are dropped at much higher rates, and post-distillation audits lose most of their detection signal.
- **Experiments:**
  - Factorial safeguard subsets in 2 environments (tau-style permissions, RBAC SQL).
  - Distill with 3 internalization methods: HERD-style, Harness-Zero-style and OPSD/Latent Harness.
  - Measure per-safeguard retention after the harness is removed, and monitor detection rate.
- **Closest prior:** Masked Erosion 2610.38392; HERD 2610.31401; Latent Harness 2611.01415; Provenance Marks 2610.21458; the OPSD wave (2608.13040, 2610.27495).
- **Origin:** Dan-modified (oversight after distillation).
- **Notes:**
  - Cost: $1–3k GPU.
  - Fits a safety venue more naturally than the NeurIPS RSI framing.

---

## Topic T6: World models and environment interventions (mentor-adjacent)

### C15 `breaking-the-improver`: Breaking the Improver: do self-improvement gains survive controlled environment interventions?
- **Question:** When harness-evolving and meta-agents improve on clean environments, how much gain survives paired BreakingWeb-style interventions? Does evolution shift the failure mix toward belief failures? Does an intervention-aware evolution diet fix this at matched compute?
- **Claim:**
  - Evolved champions have a larger paired intervention drop than their seeds, because they encode happy paths and trust success signals.
  - Ranking evolution methods by intervention-robust gain flips the leaderboard.
  - Evolving on a clean+intervention mix recovers robustness at matched compute.
  - If evolution instead learns verification, that is equally publishable.
- **Experiments:**
  - 3–4 evolvers (GEPA, ACE, AHE, Hyperagents-lite) × 2–3 cheap browser and tau backbones.
  - Evolve on clean BreakingWeb train tasks plus a tau-style procedural set with automatic interventions (renames, schema shifts, DOM reorderings, spoofed success).
  - Report the paired drop by primitive and layer, the failure-mode shift (belief failures), and correlation with HDA's overfitting component.
  - Arm B evolves on the intervention mix. Use 3 seeds.
- **Closest prior:** BreakingWeb 2609.35814; SkillShift-Bench 2610.33528; Beneath the Surface 2611.07624; HDA 2610.22466; ReScopeBench 2611.05450; PACE-Bench 2608.14441.
- **Origin:** new (merges the world-model sweep's Idea 1 with the benchmarks sweep's intervention audit).
- **Notes:**
  - Cost: $600–1.5k.
  - Builds on the mentor's own asset and is uncontested (BreakingWeb has 1 forward citation).
  - Could serve as the OOD and shift axis of C4 (re-adaptation speed after intervention vs evolution depth).

### C16 `self-forecast`: Does the improver learn a world model of itself? Self-forecast skill as a probe of improving at improving
- **Question:**
  - Does a self-improver's ability to predict the held-out gain of its own edits (R², slope, sign accuracy) rise across generations and improver checkpoints?
  - Does it predict future improvement rate?
  - Does IDM-style predict-before-modify help because of forecast content or because of the extra revision?
- **Claim:**
  - Self-improvers get better at finding improvements without getting better at predicting them. Forecast skill stays flat while accepted gains continue, so current RSI gains are search, not meta-learning.
  - Under a matched revision budget, a shuffled-forecast placebo recovers most of the IDM gain.
  - Forecaster calibration collapses with archive depth and after environment shift.
  - Rising forecast skill would be the first direct evidence of meta-learning.
- **Experiments:**
  - Offline: a same-parent edit-pair benchmark built from Hyperagents, DGM, AHE and GEPA archives.
  - Online: log a predicted gain for every candidate in GEPA-style, Hyperagents-lite and Gödel-style loops. Evaluate each candidate with at least 3 seeds on held-out tasks to set the noise ceiling.
  - Arms: IDM-true, IDM-shuffled, reflect-only and none, at matched tokens with 8+ seeds.
  - Controls: an external fresh-context forecaster and C1 checkpoints.
  - Regress future gain rate on forecast skill, and test forecasts before and after an intervention.
- **Closest prior:** Predict Before You Modify 2610.13991; Rehearse 2607.27687 (confidence cliff); 2610.22599; LMs as Research World Models 2610.12235; SpecRSI 2610.16491; WM post-training audit 2609.33335.
- **Origin:** new (merges the frontier sweep's Idea 2 with the world-model sweep's Idea 2).
- **Notes:**
  - Cost: offline under $200; online $500–1k; forecasts are nearly free.
  - Direct hit on Xunjian's self-reference + world models agenda.
  - The niche "WM for the self-improver" is crowded with methods but has no measurement study.

### C17 `parametric-godel`: Parametric Gödel Agent: does modular weight self-modification compound where shared updates decay?
- **Question:** If the self-improvement action space is "train, insert or refresh a capability LoRA" (WeAct internal actions), does improvement compound over 5–8 rounds where shared-LoRA self-updates decay?
- **Claim:** Modular self-modification preserves retention and plasticity across rounds, giving a positive G2 continuation utility. Shared-LoRA self-updates show Third-Eye-style decay (negative after the second update).
- **Experiments:** Qwen3-4B/8B over 5–8 rounds. Arms: shared LoRA vs WeAct-style modular library vs harness-only, at matched compute. Report paired G1/G2 endpoints, held-out results and routing capacity.
- **Closest prior:** WeAct 2610.26847; SLAaaT 2608.17034; Third Eye 2611.12988; CoEvo-RSI 2610.25078.
- **Origin:** new (mentor's WeAct).
- **Notes:**
  - Cost: $2–5k GPU, which strains the budget.
  - K4 risk: the paper must lead with the compounding and plasticity measurement, not with the combination.

### C18 `evolve-in-dream`: Evolving in a dream: does harness evolution inside world-model simulators transfer to real environments?
- **Question:** If harness evolution is scored in an LLM-simulated or learned web world, what fraction of the gain transfers to real sites, and how does that scale with simulator fidelity and evolution depth?
- **Claim:** Harness evolution in simulated worlds Goodharts on simulator quirks. The transfer fraction falls with evolution depth even when next-state prediction accuracy is high, so prediction accuracy is the wrong fidelity metric for self-improvement.
- **Experiments:** Build prompted-LLM and AWM/Qwen-AgentWorld-style simulators of 2–3 BreakingWeb sites. Run GEPA-style evolution against each and evaluate on the real sites. Vary simulator fidelity and evolution depth, and plot the transfer fraction.
- **Closest prior:** DarwinX 2608.07545; AdvSim2Real 2610.08773; Agent World Model 2602.10090; WM post-training audit 2609.33335.
- **Origin:** new.
- **Notes:**
  - Cost: $500–1.2k.
  - Mentor fit (world models plus BreakingWeb). There is simulator-engineering overhead.

---

## Suggested packaging (for the ranking round)
- **Main-paper core:** C1 + C2 (+ C3 as estimand, + C7 as a section). The framing: "Do self-improvers get better at improving?"
- **Plasticity paper:** C4 + C5 (mechanism) + C15 (shift probe).
- **Mentor-flagship alternatives:** C15 or C16.
- **Dan's current lead:** C10/C11. It is viable but needs the evidence-bandwidth surprise, and it is under pressure from Meta FAIR, Continual-ARC and HERA.

## Parked

| Idea | Reason |
|---|---|
| Confound battery as a standalone paper (Dan #2) | K1: scooped at the task level by HDA 2610.22466, Beneath the Surface 2611.07624, Winner's Curse 2610.09239, RSI Tax 2611.13524, Frozen-Weights 2611.11342, Rethinking Eval 2607.12227 and the 2610.27610 census. Kept only as a control suite and moved to the meta level in C7. |
| Hyperagents meta-transfer replication (standalone) | Narrowed: Beneath the Surface re-ran HyperAgents, and 2610.27610 flags the design as non-same-start. Workshop-sized; folded into C1 as one family. |
| Task-selection baselines (Dan #4) | K1: largely scooped by ActiveSaddler 2610.00906, Task-CoEvolve 2608.20169, GEPA-Curator 2610.37651 and SSR. Kept as baselines in C10. |
| Anchoring ablation (Dan #5) standalone | Done for weight learners (Anchored Self-Play, Self-Evolve With a Reference). Too small alone; folded into C10/C11. |
| Multi-round harness→weights compounding + per-round 2×2 (Dan #6, 2×2 crossing) | Heavily threatened by HERD 2610.31401, CoEvo-RSI 2610.25078, JoMoHa 2611.11499, Ceiling 2610.11655 and Third Eye 2611.12988. Needs $2–6k GPU. Only a seeded, compute-matched accounting version is left, which is high-cost and incremental. |
| Co-evolutionary Goodhart with weight training (Dan #7) | $4–15k GPU (infeasible), partially scooped by CODA 2610.20845, False Frontiers 2609.39102 and RuHarness 2610.36826. The frozen-weight version survives as C11. |
| PAIRED-style regret task generator (Dan #8) | K1: SPADE 2608.19197 (hint-based regret); also $5–20k. |
| Which lever when / pre-registered lever-choice benchmark | Medium-high scoop (Ceiling, Component Routing 2610.01787, Where Should Learning Live 2611.09701, MetaRSI, RSIFlow) and GPU-heavy ($3–8k). |
| Procedural tau generator / LatentRule-tau as a benchmark paper | Benchmark space saturated (SelfImprove-Bench, Delta-tau-Bench, GRADUS, AutoGym, RSI-Env, ...). Build it as shared infrastructure for C1/C4/C8/C10/C11. |
| Any new RSI benchmark | Saturated: RSIGym, RSI-Forge, RSI-Env, Flex-RSI, SelfImprove-Bench, RSI-Dojo, ParetoRSI-Gym, RSI Arena. |
| Actor vs reflector plasticity | Explicit future work of Agent Plasticity (Meta), so the scoop risk is high. Separating actor from reflector cleanly needs frontier models. A cheap version could be a C4 ablation. |
| Moderators of meta-improvement (verification × feedback × model tier) | Incremental as a standalone. Verification and model tier become factors inside C1. |
| Directedness test of meta-modifications | Too small alone (<$300); a section of C1. |
| Restoring plasticity (resets, shrink-and-perturb, sleep) and harness plasticity under curricula | Merged into C4 as intervention arms and a curriculum probe. |
| Memorize vs generalize (fresh-instance decomposition) standalone | Continual-ARC 2611.14505 already shows the core finding. Kept as a measurement inside C10. |
| Exchange rate of guidance (pricing a hint in compute) | Small and tangential to the mentor's agenda; needs human calibration; Meta FAIR 2610.38882 is adjacent. |
| New world-model-guided self-modification method | Crowded (IDM 2610.13991, SpecRSI 2610.16491, Rehearse 2607.27687, RSI-Anything 2610.13912, EvoBelief). Pivoted to the measurement in C16. |
| Ignition test as framing | Framing already published (AIDE² 2609.26457, 2610.27610 C4/C5). Executed as the powered slope test in C1. |

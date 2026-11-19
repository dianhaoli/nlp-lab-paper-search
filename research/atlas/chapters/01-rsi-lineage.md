## Self-referential & recursive self-improvement systems (lineage)

*Scope.* This chapter covers systems where the thing that produces improvements (the improver, its search or selection rule, or its evaluator) can itself be edited and inherited. It also covers the empirical heirs of the Gödel machine and the audits of whether their "recursion" is real.

*Sources.*
- **Read in full this session (✓):** RQGM 2606.26294, AIDE² 2609.26457, IDM 2610.13991, SoL-Pi 2609.20519, SwarmRSI 2610.20431 (main text), Physical-AI RSI 2610.29677 (abstract + §2–3 R1/R2), AlphaEvolve 2506.13131 (headline + ablation sections), Gödel Agent Table 1 and §6.2, DGM (abstract, §4, App. E.1), HGM (§5, Tables 1–3), Hyperagents (§5, transfer tables), ELITE 2611.21056 (abstract, §5.1–5.2, Table 1).
- **Read in full this session, surveys:** the L5 section (§3.6) of 2609.11873, the lineage passages of 2607.07663 (§3.5) and 2607.13104 (§2.3, §3.2, §6.4), and the improver-update passages of the alphaXiv-only mechanisms survey (slug `2611.recursive-self-improvement-llm-agents`).
- **Spot-checked this session in full text (intro/abstract numbers):** TRUST 2611.10787, SIM 2611.18856, RSI Tax 2611.13524, Resolution Hypothesis 2611.23068.
- **✓(FTV):** checked in full text in the project's Nov 13 pass (`/home/user/nlp-lab-paper-search/research/FULLTEXT_VERIFICATION.md`).
- **◐:** abstract only (arXiv API or OpenReview).
- **†:** unverified or from memory.
- Local texts are in `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/<id>/paper.txt`.

### At a glance
- **What this corner is.** It is the lineage from Schmidhuber's Gödel Machine (2003; adopt a self-rewrite only with a proof that it raises expected utility) to empirical stand-ins:
  - archive or tree search over self-edits scored on benchmarks: STOP → Gödel Agent → SICA → DGM → HGM → Hyperagents;
  - then pushes into editable evaluators (RQGM), editable forecast models (IDM), research-agent harnesses (AIDE²) and fixed-operator recursion (Metaⁿ).
  - The Ren/Schmidhuber survey formalizes the target as Σ_{t+1} = I_{Σ_t}(Σ_t; S_t) (2607.13104 §6.4, Eq. 23, ✓) and says "the operator can become part of its own operand" (§3.2, ✓). Duan et al. (2609.11873, ✓) call this tier **L5 "recursive inheritance"**.
- **State of the art (task scores, single runs unless stated, not cross-comparable).**
  - Coding: RQGM 82.1% vs HGM-H 75.0% held-out DeepSWE (28 test tasks, i.e. 23 vs 21; one run per arm; ✓). HGM 61.4% full SWE-Verified with GPT-5-mini (✓). ELITE 85.6% on a **60-task subset** of SWE-bench Pro (DeepSeek-V4-Flash, 3-run mean) *with zero candidate task evaluations*. This is slightly below DGM 86.7% and HGM 88.3% (✓ §5.2, Table 1).
  - Non-coding: Hyperagents paper review 0.0→0.710 and robotics 0.060→0.372 (✓); AIDE² 0.703→0.778 private grade with external transfer (✓).
- **Consensus.** Today's systems are **bounded**. Objective, protected evaluation, parent selection and orchestration stay human-fixed:
  - DGM's proposal model, archive and parent selection are frozen (✓FTV);
  - Hyperagents keeps evaluator and parent selection fixed (✓FTV; Duan §3.6.1 ✓);
  - RQGM keeps anchor, schedule and harness fixed (✓; Duan §3.6.2 ✓);
  - Metaⁿ freezes Ω (✓FTV).

  All four surveys say this in their own words: "bounded meta-improvement" (2609.11873 §3.6.5, ✓), "bounded, verifiable loops" (2607.13104 §6.4 takeaway, ✓), "nearly everything else … evolving one component of the system while freezing the rest" (2607.07663 §3.5, ✓).
- **Structural ≠ effective recursion.** Duan et al. separate a revised mechanism that *governs* a later round (structural) from one that *produces or selects better successors under comparable budgets and independent assessment* (effective) (✓ §3.6). Their Table 7 lists STOP, Gödel Agent, DGM, HyperAgents, RQGM, A-Evolve-Training and AIDE² as L5 mechanisms, "qualified in the text". They state that an evolved evaluator (RQGM) can establish structural L5. Effective L5 is not reliably established. The Hyperagents 200-iteration transfer is not statistically significant (✓ §3.6.1). AIDE²'s evolved-outer-improver test found no significant efficiency advantage (✓ §3.6.4; restated in the future-evaluation section). "Statistically reliable accumulation … remains open" (✓ §3.6.5).
- **Audit wave (Oct–Nov 2026).** Gains are mostly inheritance of one early edit (Improvement-by-Inheritance). Improvers edit themselves in 1–15% of generations (IbI). Retained improver edits are 0% (SelfImprove-Bench), and archive curves sit inside permutation nulls (IbI). Selection on noisy evals is "no better than random" at telling helpful from harmful candidates (Resolution Hypothesis, abstract ✓). Few systems score self-edits on a separate signal and can reject them. HMED does this for improver edits (✓FTV), and IDM does it for its forecaster (Brier/CMSE gate, ✓ §3.4).
- **Biggest open problem.** Showing **effective, compounding recursion**: order-1 successor quality (and order ≥2) under matched compute, multiple seeds and independent evaluation, with a mechanism that explains it. The secondary problem is the evaluator. Every system either freezes it (Goodhart risk) or anchors it to ground truth (RQGM), which bounds it. RQGM itself states that "the anchor bounds what evaluator evolution can achieve" (RQGM App. G, ✓).

### How it developed

| month | paper | what changed |
|---|---|---|
| 1987 / 2003 | Schmidhuber self-referential learning; Gödel Machine | Fully self-referential rewrite gated by a proof of utility gain. Every part is editable, "including the self-modification generator and the theorem prover" (✓ via 2607.13104 §2.3). Never practical. |
| Sep 2023 | Promptbreeder 2309.16797 | Self-referential *prompt* evolution: mutation-prompts evolve too (◐). |
| Oct 2023 | STOP 2310.02304 | Improver code improves itself, scored by one-step downstream meta-utility. GPT-4 improves while GPT-3.5 and Mixtral decline. First sandbox-escape anecdotes (✓FTV). |
| Aug 2024 | ADAS 2408.08435 | Fixed meta-agent writes agent code (the non-self-referential baseline) (◐). |
| Oct 2024 | Gödel Agent 2410.04444 (Yin et al., Xunjian's paper) | Runtime monkey-patching of its own policy *and learner*. No gate (LLM discretion) (✓FTV). |
| Apr 2025 | SICA 2504.15228 | One codebase is both agent and improver. Argmax archive; SWE-Bv subset 17→53%, n=1 (✓FTV). |
| May 2025 | DGM 2505.22954 | Open-ended archive replaces proofs with empirical utility. SWE-bench 20→50%, ~$22k/run (✓). |
| Jun 2025 | AlphaEvolve 2506.13131 | Evolutionary coding agent with meta-prompt evolution. It "optimizes its own training process" (kernel heuristic → 1% Gemini training time) (✓). |
| Oct 2025 | HGM 2510.21614 | Clade-metaproductivity (CMP) replaces own-score greedy expansion and approximates the Gödel-Machine ideal (Thm 1) (✓FTV). |
| Feb 2026 | EvoX 2602.23413 | Meta-evolves the search strategy itself (◐). |
| Mar 2026 | Hyperagents 2603.19461 | Task and meta agent in one editable program, across 4 domains. Meta-agent transfer to math grading (✓FTV). |
| Jun 2026 | Red Queen GM 2606.26294 | Evaluator inside the loop: epoch-frozen co-evolved judges, a ground-truth anchor and selective erasure (✓). |
| Jul 2026 | MetaSkill-Evolve 2607.05297; RHI 2607.15524 | Two-timescale forced meta-updates; prompt-level recursive harness (✓FTV / ◐). |
| Aug 2026 | Metaⁿ 2608.24735; Mendel GM 2608.07645 | Recursion by *conditioning a frozen operator*; comparative and cross-lineage mutations (✓FTV / ◐). |
| Sep 2026 | MetaRSI 2609.06396; AIDE² 2609.26457; SIFT 2609.19526; SoL-Pi 2609.20519; Trusting-Trust 2609.17817; IDM 2610.13991 (v1 Sep 26); surveys 2609.11873, 2607.07663 | Scheduled meta-terms; research-agent RSI with an **ignition test**; judge-guided cheap search; first supply-chain attack on self-modifiers (◐); meta-components with their *own* accuracy signal (IDM) (rest ✓). |
| Oct 2026 | VERSE 2610.02616; HMED 2610.07979; FreeEvolve 2610.09197; RSIFlow 2610.21791; SpecRSI 2610.16491; "Failure Feedback" 2610.14662; Auditing-the-Product 2610.27610; Physical-AI RSI 2610.29677 | Verification-gated self-evolution; the first audits (selection reproduces DGM; only 4/26 improver claims are same-start and matched-budget). IDM v2 (Oct 15). |
| Nov 2026 | IbI (ICLR27 ZMmJDeDTXk); SelfImprove-Bench 2611.02797; Flex-RSI 2611.08946; TRUST 2611.10787; RSI Tax 2611.13524; SIM 2611.18856; ELITE 2611.21056; Resolution Hypothesis 2611.23068; "Still need to be told?" 2611.23219; Harnesses-that-Improve-Themselves 2611.22299 | The **measurement turn**: inheritance dominance; noisy selection; statistical gates; cost accounting; evaluation-free self-improvement; improver memory. |

### Taxonomy
The axes follow the survey taxonomies:
- Duan L1–L5 and structural vs effective;
- Chen's "what is improved × loop closure";
- Ren's θ vs Σ with distributional vs action-level self-reference (this chapter is all Σ, action-level);
- the Dai et al. three survival patterns (answer refinement / persistent experience / **improver update**).

Within L5, the corner splits by **what part of the improvement machinery is editable** and **how a change is credited**.

| Approach (what is self-referential) | Credit / gate for meta changes | Representative papers |
|---|---|---|
| A. Improver rewrites improver (pure self-reference) | improver gates itself (STOP), or LLM discretion | STOP, Gödel Agent, Promptbreeder |
| B. Single codebase = agent + improver, archive search | compile-valid → archive; parent choice by own score (DGM), argmax (SICA), clade CMP (HGM), comparative mutation (MGM) | SICA, DGM, HGM, Mendel GM, SIFT, RSI-on-a-Budget 2610.29121, HGM-Dual 2610.30042 |
| C. Explicit task-agent + meta-agent program (hyperagent) | object-level score only; meta edits unseparated | Hyperagents, IDM-Hyperagents, MetaShare 2610.21887 |
| D. Meta-component with its own signal | forecast loss (IDM); replay Δ (HMED, IFR); descendant score (FreeEvolve, Looped-RSI); successor quality (RSI-Harness) | IDM, HMED, IFR (eIJH6HV9LZ), FreeEvolve, MetaSkill-Evolve, MetaRSI, RSIFlow |
| E. Editable evaluator / utility | ground-truth anchor + epoch freeze + erasure | RQGM, Gödel Ecology (N4GJVmrzsF), co-evolved verifier 2610.11464 |
| F. Fixed operator, recursion on inputs | none needed (Ω frozen) | Metaⁿ |
| G. Research-agent RSI (outer agent fixed or swapped) | private held-out grade, argmax | AIDE², SoL-Pi*, SwarmRSI*, Gödel Forest*, A-Evolve-Training |
| H. Evolutionary discovery with meta-prompts | fixed evaluators, MAP-Elites/islands | AlphaEvolve, OpenEvolve/ShinkaEvolve† |
| I. Gate theory / statistical admission | proof → statistical certificate | Statistical Gödel Machines (0FWBc4gY08), TRUST, Lyapunov-Gated RSI (3t0j2Y05EH), HGM Thm 1 |
| J. Audits and measurement | nested replay, permutation nulls, directedness tests | IbI, Failure-Feedback, Auditing-the-Product, Resolution Hypothesis, RecurLens, Lineage-Is-Memory, SelfImprove-Bench |

\*These carry the "RSI" label but evolve object-level state (harness mechanisms, research states, data strategies) with a fixed improver. Under Duan's criterion they are at most L4 / structural L5.

### Paper cards

**Improvement by Inheritance: Measuring RSI in Coding Agents** ([OpenReview ZMmJDeDTXk](https://openreview.net/forum?id=ZMmJDeDTXk), Nov 2026, ICLR27 submission)
- *What it does.* Separates child quality Q⁰ from successor quality Q¹. Proves that self-lineage contrasts are dominated by inheritance when self-edit rates are low. Identifies Q¹ by nested replay.
- *Setup.*
  - Audit of all 71 Hyperagents runs (1,550 improver transcripts) plus the DGM and ADAS histories.
  - New experiments on MiMo-V2.5 (+5 models); 5 seeds × 80 gens on 8 new families.
  - Order 1: 6 meta-states × 6 replicates.
- *Key results* (read in full by the project, `papers/improvement-by-inheritance-notes.md`; ✓):
  - Best-so-far inside the permutation null at 119/141 checkpoints.
  - Improvers edit themselves in 1–15% of generations.
  - The gen-1 "read the eval report" edit raises Q⁰ 0.476→0.858 (12 states).
  - δ₁ = +0.003 vs a self-lineage contrast of +0.293.
  - Positive control (procedure-targeted modifier): +0.171 [0.099, 0.240].
  - Metaⁿ's 0.309 shrinks to 0.014 on executable starts.
- *Limitations.* One backbone at order 1; only large effects resolvable; order ≥2 unmeasured; coding only; Gödel Agent not analysed.

**Hyperagents (DGM-H)** ([2603.19461](https://arxiv.org/abs/2603.19461), Mar 2026, v2 Nov 1; Meta/UBC/Vector/Edinburgh/NYU)
- *What it does.* Puts task agent and meta agent in one editable repo, so the improvement procedure is open to edits. Archive search over 4 domains.
- *Setup.* Polyglot, paper review, robotics reward design, IMO-GradingBench; 5 runs per config; imp@k with the meta agent fixed.
- *Key results.*
  - Paper review 0.0→0.710 (CI 0.590–0.750) and robotics 0.060→0.372 (CI 0.355–0.436) (✓ §5).
  - Transfer to math grading imp@50 = 0.630; meta-only arm 0.620, task-only 0.100 (✓).
  - Compounding 0.640 vs 0.610, n.s. (✓).
  - Editable parent selection 0.491 vs handcrafted 0.532, n.s. (✓FTV; numbers ✓).
- *Limitations / critiques.*
  - Every valid child is archived, with no meta gate (✓FTV).
  - Meta-edits are mostly trackers and memory, not search logic (Duan §3.6.1 ✓: "evolved meta-agents build performance trackers and persistent memory"; round3).
  - IbI: label collapse in paper review (163/369 nodes); gains are inheritance.
  - Duan: 200-iteration transfer is n.s. (✓ §3.6.1).
  - SelfImprove-Bench: 0% retained improver edits.
  - Trusting-Trust: poisoned benchmarks induce persistent vulnerable instructions (◐).

**Darwin Gödel Machine** ([2505.22954](https://arxiv.org/abs/2505.22954), May 2025; ICLR 2026 ✓)
- *What it does.* An open-ended archive of self-modifying coding agents, ranked by benchmark utility.
- *Setup.* SWE-bench subset / Polyglot; 80 iterations; ~$22k per SWE-bench run (baselines ~$10k) and ~2 weeks (✓ App. E.1; FTV).
- *Key results.* SWE-bench 20.0→50.0% and Polyglot 14.2→30.7% (✓ abstract and §4). Without self-improvement 39.0; without open-endedness (greedy) 39.7 (✓).
- *Limitations.*
  - The proposal step is a fixed o1; archive and parent selection are frozen. Only the implementing agent evolves (✓FTV).
  - Single runs; 51.3% of children functional; hacking anecdote (node 114 removed logging) (✓FTV).
  - 2610.14662: 145 edits show no target-directed repair (OR 0.69), and selection over zero-mean variation reproduces the trajectories.
  - IbI: 45–62% of children were never scored.

**Huxley-Gödel Machine** ([2510.21614](https://arxiv.org/abs/2510.21614), Oct 2025; ICLR 2026†; KAUST/Schmidhuber)
- *What it does.* Expands by Thompson sampling on clade-metaproductivity (pooled descendant success) rather than own score. Thm 1: a CMP oracle suffices to implement a Gödel machine under assumptions.
- *Key results.*
  - SWE-V-60: 56.7 vs DGM 53.3 (✓ Table 2).
  - Weighted correlation with empirical CMP 0.778/0.626 vs 0.27–0.44 for DGM/SICA selection (✓ Table 1 for HGM values; baselines ✓FTV).
  - Full SWE-Verified 61.4% with GPT-5-mini. "Human-level" on SWE-bench Lite with GPT-5, optimized on Verified with GPT-5-mini (93 of 300 Lite tasks overlap with Verified) (✓ §5).
- *Limitations.*
  - Every child is added (gate only in theory); single runs; ~$5k (✓FTV).
  - IbI Prop 7: CMP ≠ successor quality.
  - "Measured Behavior, Recorded Zero" (9UsBcc69Ua, ◐): invalid and do-nothing children both score 0.0.

**The Red Queen Gödel Machine** ([2606.26294](https://arxiv.org/abs/2606.26294), Jun 24 2026, v3 Oct 8; Cambridge/NVIDIA/Flower; ICLR27 o3txFJw4JR†)
- *What it does.* Moves evaluators into the search. Within an epoch one evaluator is frozen, so HGM's guarantees apply per epoch. At boundaries a challenger replaces it only if it beats the incumbent on a ground-truth anchor (ties keep the incumbent), and dependent utility records are erased.
- *Setup.*
  - Baseline is HGM-H (HyperAgents with HGM search, fixed evaluators).
  - GPT-5.5 (low) for most domains; GPT-6 Astra for DeepSWE (28/57/28 train/validation/test split); Nemotron 3 Ultra for search-time task agents in reward design.
  - Matched blended tokens.
  - Eight headline runs: one RQGM and one HGM-H run per domain (✓ App. C).
- *Key results (✓ §5, App. C.7).*
  - DeepSWE held-out 82.1% vs 75.0% (i.e. 23 vs 21 of 28) at low effort; 85.7% at medium/high vs HGM-H 78.6%/82.1%.
  - Polyglot 71.7 vs 69.9 at 1.35–1.72× fewer tokens.
  - Writers' panel acceptance 38.8% vs 21.8% at matched tokens (44.6M vs 42.5M; 1.78×). The specialist reaches 40.5% at 221.8M.
  - Prover 4.33 vs 3.73 (±0.41–0.43), but 88.0M vs 21.5M tokens. The matched-token generalist ties at 3.73.
  - Reward design: 5 runs/arm, held-out 0.365±0.053 vs 0.306±0.128.
  - No-erasure control keeps archive ranking ρ ≥ 0.90, so erasure is necessary.
  - 83–90% of accepted patches edit code shared by coder and reviewer (83% DeepSWE, 90% Polyglot).
- *Limitations.*
  - Headline coding, writing and proof arms are one search per arm (✓ App. C: "eight headline runs"). Only reward design has 5 runs.
  - The DeepSWE gap is 2 tasks.
  - Evaluator quality is bounded by the anchor (App. G, ✓).
  - Generated papers and proofs are scored by automated panels, not humans (App. G). The 1.78× uses a 4-reviewer panel (Sakana, DGM-H, HGM-H, RQGM-adversarial), so it includes RQGM's own reviewer (✓ Table 2).
  - The adversarial reviewer is *less* accurate than the HGM-H reviewer by construction: 80% vs 88% APReS accuracy (✓ §5.4, Table 8).

**Gödel Agent** ([2410.04444](https://arxiv.org/abs/2410.04444), Oct 2024; ACL 2025†; Yin, Wang, Pan, Lin, Wan, W.Y. Wang)
- *What it does.* An agent that reads and monkey-patches its own runtime code, including the `self_update` learner, recursively.
- *Setup.* 6 cycles × 30 iterations; GPT-4o optimizer, GPT-3.5 policy; DROP/MGSM/MMLU/GPQA; ~$15/run (✓FTV).
- *Key results (✓ Table 1).* Gödel-base 80.9 / 64.2 / 70.9 / 34.9 vs Meta Agent Search 79.4 / 53.4 / 69.6 / 34.6. The unconstrained "free" variant (90.5 / 90.6 / 87.9 / 55.7) is not comparable; unrestricted runs could call stronger models (Duan §3.6.1 ✓).
- *Limitations.*
  - No gate. 4% unexpected terminations, typically when it modifies its recursive improvement module. 14/100 MGSM trials end below the initial policy (✓ §6.2; 2609.11873 §3.6.1 ✓).
  - Metaⁿ App J: 0–6 self-modification calls per 20 iterations; seed-level collapse.
  - Plateaus on CO-Bench vs Metaⁿ/OpenEvolve (round3).

**STOP: Self-Taught Optimizer** ([2310.02304](https://arxiv.org/abs/2310.02304), Oct 2023; COLM 2024†)
- *What it does.* A scaffold "improver" program is applied to its own source and scored by meta-utility (mean downstream utility after one application).
- *Key results (✓FTV).* GPT-4 improves over T=1–3 from 61%; GPT-3.5 and Mixtral decline. Transfer, e.g. 3SAT 0→75.1. Unsandboxing 0.42% (GPT-4). Duan (§3.6.1 ✓): a selected fourth-generation improver beat the seed on all five held-out transfer tasks, and weaker-model runs regressed on average.
- *Limitations.* The seed argmax drops the incumbent, so rounds can regress. The transfer improver is hand-picked. Self-edits happen 100% of the time by construction.

**AIDE²: Recursive self-improvement of AI research agents** ([2609.26457](https://arxiv.org/abs/2609.26457), Sep 22 2026; Weco)
- *What it does.* A bi-level tree search. A fixed outer agent (AIDE_human on Claude Opus 4.7) rewrites the inner research agent (evaluated with Gemini 3 Flash). Candidates are graded on private held-out splits of ML / heuristic / harness-engineering tasks at fixed dollar budgets, keep-if-better (argmax).
- *Key results (✓).*
  - 8-day, 100-node run with 7 accepted rewrites (steps 2, 6, 28, 39, 47, 63, 85), grade 0.703→0.778 vs AIDE_human 0.749. Two more runs accepted 2 and 4 (§3.2).
  - External transfer, AIDE₈₅ vs AIDE_human (Table 2 protocols; the inner model varies by benchmark):
    - ALE-Bench lite 1790 vs 1511 (10 tasks × 10 seeds, Gemini 3 Flash);
    - MLE-Bench lite 0.722 vs 0.708 (22 tasks × 3 seeds);
    - FML-Bench 19.9 vs 19.6 (18 tasks × 3 seeds, GPT-5.4);
    - WeatherBench 2 0.793 vs 0.404 (1 task, 3 seeds, Gemini 3.1 Pro).
  - Reward hacking on 38 held-out kernel pairs: 55% (AIDE₀) → 39% (AIDE₄₇) → 32% (AIDE₈₅); the human agent is at 39%.
  - **Ignition test** (AIDE₄₇ vs AIDE_human as outer agent, 3 seeds × 50 steps): 0.780 vs 0.782, explicitly inconclusive.
- *Limitations.*
  - The outer improver is not the evolved agent except in the ignition test, so it is not self-referential during the main run.
  - Gains are non-monotone across checkpoints.
  - Company-reported (Duan §3.6.4 ✓).
  - One run accounts for most claims.
  - One evolved "robustness" change patched a task's held-out scoring script, which crashed whenever one test case failed. The paper frames this as repairing, not exploiting, the evaluation (✓ §3.5).

**Predict Before You Modify (IDM-guided Hyperagents)** ([2610.13991](https://arxiv.org/abs/2610.13991), v1 Sep 26 2026, v2 Oct 15; MSR/UVA/Tsinghua; ICLR27 ZBxV5bEYUH†)
- *What it does.* Adds an editable Improvement Dynamics Model that forecasts task-level outcomes of a proposed diff.
  - A draft is revised once if bias-corrected predicted score < parent.
  - The IDM is itself edited every K ≥ 2 generations. A candidate is **promoted only if its prediction loss falls on the next K records (Brier for probabilities, squared error for continuous values) and CMSE does not rise** (✓ §3.4).
  - This is a genuine separate meta-gate.
- *Setup.* HyperAgents codebase; 4 domains; 5 runs × 40 generations; medians and AUBC.
- *Key results (✓ §4).*
  - Polyglot AUBC 0.306 vs 0.210, which is the "46%". Matched-token AUBC +10.9%.
  - 166 held-out tasks: +13.25 vs +5.42 pp (initial agent 16.27%).
  - Candidate IDMs lower Brier loss in 19/28 comparisons.
  - Transferring only the IDM to math grading: +3.4% AUBC over Selective IDM with a fresh IDM (+2.3% matched tokens); held-out +4 pp over Selective, +8 over HyperAgents.
- *Limitations.* Gains are measured as AUBC over generations (rewards early discovery), not as successor quality. Revision alone explains part (Reflective HyperAgents arm). No nested-replay order-1 test.

**Metaⁿ: RSI through Emergent Depth** ([2608.24735](https://arxiv.org/abs/2608.24735), Aug 2026)
- *What it does.* Keeps one meta-operation Ω frozen and stacks object-level wrapper layers until convergence. It argues self-editing systems must freeze some driver.
- *Key results (✓FTV).*
  - CO-Bench 0.851; ARC dev 0.331.
  - d1→d2 +0.113, d2→d3 −0.006 (41% of pairs regress); per-depth mean peaks at 0.726, then 0.602.
  - About 72% of the gain is from a passed context string.
  - Recursion ablation not compute-matched (870K vs 17M tokens).
- *Limitations.* Its "depth" is stacked object layers. IbI: the selected gain is mostly repaired unparseable programs (0.309→0.014).

**Flex-RSI** ([2611.08946](https://arxiv.org/abs/2611.08946), Nov 6 2026; 13 institutions)
- *What it does.* A benchmark of 24 vision/robotics tasks × 18 models. The executor edits solutions behind a validation gate (kept only if it scores higher on held-out validation; 2σ per FTV). The improver edits the harness and itself, with a mandatory self-review every 4 rounds and a 400-line cap.
- *Key results (✓FTV).* Self-improving vs frozen improves 6/7 pairs (+2 to +40). Every pair is n=1, and 1 episode = 20 points. 90% of the gain comes by rounds 3–4.
- *Limitations.* Improver self-edits are kept unconditionally. No self-edit rates. Feedback granularity confounds weak-model results.

**SICA** ([2504.15228](https://arxiv.org/abs/2504.15228), Apr 2025)
- *What it does.* A single codebase acts as both coding agent and meta agent; argmax over the archive. U = 0.5·score + 0.25·cost + 0.25·time.
- *Key results (✓FTV).* SWE-Bv subset 0.17→0.53, one run, ~$7k.
- *Limitations.* All annotated improvements are task tools. Much of the gain is speed and cost. Noisy 50-problem argmax.

**AlphaEvolve** ([2506.13131](https://arxiv.org/abs/2506.13131), Jun 2025; Google DeepMind)
- *What it does.* An LLM-ensemble evolutionary coding agent with full-file evolution and **meta-prompt evolution** (LLM-suggested instructions co-evolve).
- *Key results (✓).*
  - 4×4 complex matmul with 48 scalar multiplications.
  - On >50 math problems, matched SOTA on ~75% and improved on ~20%.
  - Scheduling heuristic recovers 0.7% of fleet compute.
  - Kernel tiling: 23% average kernel speedup, 1% Gemini training-time reduction ("Gemini … optimizes its own training process").
  - Ablations (2 tasks: tensor decomposition and one other; 3 seeds): removing meta-prompt evolution, full-file evolution or strong LLMs each hurts.
- *Limitations.*
  - The evaluator is fixed and the improver (evolutionary controller) is not self-edited, so it is L2–L3, not L5.
  - Under token matching, evolution is ≤5% over independent sampling on 12/15 pairs (2610.23977 via round3; ◐).

**Physical-AI RSI** ([2610.29677](https://arxiv.org/abs/2610.29677), Oct 21 2026; NUS et al.; survey)
- *What it does.* Formalizes recursion as two links:
  - R1: J(I_{K1}(F)) > J(I_{K0}(F)), i.e. repair knowledge improves the improver vs a **frozen-knowledge control**;
  - R2: the improved improver improves itself, beyond both its frozen-knowledge self and the parent.
- *Key results (✓ abstract, §2.2–3).* "The physical studies reviewed here have yet to isolate gains attributable to recursion." Proposes a two-round test. A sibling survey is 2610.38266, "Recursive Self-Improvement in Physical AI: Mechanisms, Evidence, and Evaluation" (title ✓ via arXiv API; content ◐).
- *Limitations.* Survey only; robotics costs dominate.

**SoL-Pi** ([2609.20519](https://arxiv.org/abs/2609.20519), Sep 17 2026; NVIDIA/NTU/MIT)
- *What it does.* An "RSI-inspired" auto-research funnel over the harness. 152 proposed directions; 535 search environments (495 repository + 40 synthetic); >3,000 runs. A pre-declared capability-tolerance plus efficiency gate keeps 4 mechanisms. EdgeBench is held out: of 51 public tasks, 11 are used for one-way acceptance and 40 for final evaluation.
- *Key results (✓).*
  - EdgeBench full stack (GPT-5.6 Sol): 42.0 vs Pi 44.8 (93.7% of score) at −49.0% tokens and −33.2% token cost. Transfers to Opus 5 without modification: 94.3% of Pi's score, −44.7% tokens, −33.5% API cost.
  - Terminal-Bench 4 (63 CPU-only tasks): 15 vs 18 solved, at −26.3% cost.
  - Swarm (2-hour kernel search): 1,127 vs 1,366 cycles for the Pi swarm; a single agent reaches 1,333.
- *Limitations.* Not recursive: the authors call recursive efficient improvement "a long-term research vision rather than a compounding effect demonstrated." The improver is fixed. Single runs.

**SwarmRSI** ([2610.20431](https://arxiv.org/abs/2610.20431), Oct 2026, v3 Nov 2; Huawei/Leiden/Groningen)
- *What it does.* A swarm of hypothesis, program and analysis agents evolves inherited *research states* through feedback-guided mutation and rebase/crossover on a shared graph.
- *Key results (✓ main text).* SOTA on OpenProblems (avg 74.2 with DeepSeek-V4-Pro, 76.2 with Gemini 3.1 Pro vs TusoAI 67.1 with DeepSeek-V4-Pro). NatureBench Match-SOTA 44.4%. ~77% of best programs across 90 NatureBench tasks descend from mutation or recombination (1 seed †).
- *Limitations.* The improvement procedure is fixed. RSI applies only to inherited object-level state (Dai et al. pattern ii, not iii).

Remaining papers:

| id | title | one-liner | mark |
|---|---|---|---|
| [2309.16797](https://arxiv.org/abs/2309.16797) | Promptbreeder | Mutation-prompts evolve with task prompts (self-referential in prompt space) | ◐ |
| [2408.08435](https://arxiv.org/abs/2408.08435) | ADAS | Fixed meta-agent writes agent code. In the DGM-H setting the ADAS-like fixed-meta arm gets 0.0 on paper review | ◐/✓FTV |
| [2602.23413](https://arxiv.org/abs/2602.23413) | EvoX | Meta-evolves search strategy; Frontier-CS median 75.5 vs 56.2 | ◐ |
| [2607.05297](https://arxiv.org/abs/2607.05297) | MetaSkill-Evolve | Meta-skill forced every H=2; +6.38/+8.05/+1.92; no seeds; ALFWorld no-skill baseline 92.31 | ✓FTV |
| [2607.15524](https://arxiv.org/abs/2607.15524) | Recursive Harness Self-Improvement (Sakana) | Prompt-level harness spec refined by pairwise feedback over its own revision history; low-effort beats max-effort at −60% cost | ◐ |
| [2608.07645](https://arxiv.org/abs/2608.07645) / co3NCcIZ7f | Mendel Gödel Machine | Reaction-norm (multi-task) and cross-lineage hybridization mutations; additive-landscape theory | ◐ |
| [2609.06396](https://arxiv.org/abs/2609.06396) | MetaRSI | Scheduled meta-terms; contract-only improver gate; +2.3 one fork, 5 seeds | ✓FTV |
| [2609.19526](https://arxiv.org/abs/2609.19526) | SIFT (MIT/Sakana) | LLM-judge pairwise Bradley-Terry scores guide tree search; cheaper than DGM/HGM on Polyglot | ◐ |
| [2609.17817](https://arxiv.org/abs/2609.17817) | Reflections on Trusting Trust, Revisited | Poisoned benchmarks make DGM/SICA/Hyperagents self-evolve vulnerable code (e.g. disable HTTPS validation); contamination persists after clean evolution | ◐ |
| [2609.36675](https://arxiv.org/abs/2609.36675) | Gödel Forest: Balancing Search Depth and Breadth for Data-Centric RSI | Data-centric (weights) RSI as co-evolving trees; +10.70% vs single agent | ◐ (title ✓) |
| [2610.02616](https://arxiv.org/abs/2610.02616) | VERSE | Optimizer self-evolution helps only with execution verification (41.05 vs 39.20 fixed; 31.79 without verification); 1 run per cell | ✓FTV |
| [2610.07979](https://arxiv.org/abs/2610.07979) | HMED | The only separate score gate on improver edits (replay Δ>0 on 2 states); ablation cells duplicate baselines | ✓FTV |
| [2610.09197](https://arxiv.org/abs/2610.09197) | FreeEvolve | Descendant-scored evolution skill +6.9; Haiku evolver no gain; negative cross-env transfer | ✓FTV |
| [2610.14662](https://arxiv.org/abs/2610.14662) | Does Failure Feedback Direct Self-Modification? | DGM edits are not directed (8.3% vs 11.6%); selection-only null reproduces trajectories | ✓FTV |
| [2610.16491](https://arxiv.org/abs/2610.16491) | SpecRSI | Drafts successors before predecessors finish evaluation; outer-loop improver learning (GVO) with real evaluator fixed | ◐ |
| [2610.21791](https://arxiv.org/abs/2610.21791) | RSIFlow | Ungated meta-procedure updates every round; Meta4 vs Meta0 +20.56, n=1, non-monotone | ✓FTV |
| [2610.27610](https://arxiv.org/abs/2610.27610) | Auditing the Product, Not the Recursion | 114 studies; 4/26 improver claims same-start and matched-budget; 22% rerun ≥2× | ✓(project) |
| [2610.29121](https://arxiv.org/abs/2610.29121) | RSI on a Budget: Scoring Code Edits Before They Run | Linear readout scores diffs before running; HumanEval+ held-out 87.0 vs HGM selection 85.1 (GPT-4o, 12 runs) | ◐ (title ✓) |
| [2610.41157](https://arxiv.org/abs/2610.41157) | Lineage Is Memory | 1,750 pre-registered runs: greedy parent inheritance drives progress | ✓FTV |
| [2611.02797](https://arxiv.org/abs/2611.02797) | SelfImprove-Bench | 0% retained improver "opinions" by default, 20.5–45.2% when framing invites them | ✓FTV |
| [2611.10787](https://arxiv.org/abs/2611.10787) / wV4bK1dHpC | TRUST | Online multiple-testing gate; accepted-inferior rate 45.59%→2.78%; mean max drawdown 4.963→0.047 pp | ✓ (abstract + intro) |
| [2611.13524](https://arxiv.org/abs/2611.13524) | The RSI Tax | 6 methods × 2 models: bounded-note baseline matches or exceeds Meta-Harness/EvoSkill/RRSI at −65.6–71.0% total tokens; verification is 74.8–93.6% of RSI Tax | ✓ (abstract) |
| [2611.18856](https://arxiv.org/abs/2611.18856) | Self-Improvement Memory (Google) | Memory *for the optimizer* (edit rationale, predicted vs observed); +2.36 over harness-only continuation, p=0.003, 10 source runs | ✓ (intro) |
| [2611.21056](https://arxiv.org/abs/2611.21056) / 8SkxedsVil | ELITE: Evaluation-Free RSI for Coding Agents (Jilin/KAUST) | Explore-then-integrate with no task evaluations in loop; 60-task SWE-bench Pro subset 60.0→85.6 vs DGM 86.7 / HGM 88.3 (DeepSeek-V4-Flash, 3-run means); better transfer to Verified/Polyglot (73.3/69.3); 8.8×/3.2× faster wall-clock | ✓ §5.2 |
| [2611.22299](https://arxiv.org/abs/2611.22299) | Harnesses That Improve Themselves (Google) | Framework: 4 editable runtime components × diagnose-propose-validate-deploy loop; gated modular evolution beats full rewrites on SWE-bench Lite | ◐ (title ✓) |
| [2611.23068](https://arxiv.org/abs/2611.23068) | The Resolution Hypothesis | 7 methods × 6 models, >10M calls: selection "no better than random" at telling helpful from harmful candidates; method margins < run-to-run variance | ✓ (abstract) |
| [2611.23219](https://arxiv.org/abs/2611.23219) | Do LLM Agents Still Need to Be Told How to Self-Improve? (MIT) | Frozen agent given primitives and budget builds its own self-learning loop in games; artifacts transfer | ◐ |
| [N4GJVmrzsF](https://openreview.net/forum?id=N4GJVmrzsF) | Gödel Ecology | Agent–evaluator co-evolution scored by co-clade metaproductivity, delayed credit | ◐ |
| [0FWBc4gY08](https://openreview.net/forum?id=0FWBc4gY08) | Statistical Gödel Machines | Proof requirement replaced by statistical certificate; P(ever accept non-improving) ≤ α | ◐ |
| [vXnQX90fv6](https://openreview.net/forum?id=vXnQX90fv6) | RecurLens | Executable audit: Sol's recursive descendant +5.38 over frozen but −17.09 vs its own initial state; contrast 77.72→21.98 when gen-1 task policy is removed | ◐ |
| [nVl2qz74V7](https://openreview.net/forum?id=nVl2qz74V7) | SelfLineage | Descendant-aware edit history with shuffled-link controls (specified experiments) | ◐ |
| [EOrtreQLbv](https://openreview.net/forum?id=EOrtreQLbv) / [sBH9cdajAw](https://openreview.net/forum?id=sBH9cdajAw) | RSI-Harness / Looped-RSI | Separately gated improver loops; improver transfer claims | ◐ |
| [eIJH6HV9LZ](https://openreview.net/forum?id=eIJH6HV9LZ) | IFR / Meta-Forgetting | Replays improvement functions on frozen past states; +2.3–2.6 pp | ◐ |
| [3t0j2Y05EH](https://openreview.net/forum?id=3t0j2Y05EH) / [OnzWTehIaN](https://openreview.net/forum?id=OnzWTehIaN) | Lyapunov-Gated RSI / Why Is RSI Hard to Verify? | Theory of admitting improver edits; finite-horizon verification criterion | ◐ |
| Surveys: [2609.11873](https://arxiv.org/abs/2609.11873), [2607.07663](https://arxiv.org/abs/2607.07663), [2607.13104](https://arxiv.org/abs/2607.13104), [alphaXiv 2611.recursive-self-improvement-llm-agents](https://www.alphaxiv.org/abs/2611.recursive-self-improvement-llm-agents) (alphaXiv-only, no arXiv id), [2610.35193](https://arxiv.org/abs/2610.35193), [2610.14625](https://arxiv.org/abs/2610.14625) | — | L1–L5 with structural/effective; substrate × closure; θ/Σ and meta-level skills; collect→verify functions; trustworthy-RSI S1–S4 + RI; "two perspectives" (existing methods → new ideas) / 9-dimension design space | ✓ / ✓ / ✓ / ✓(project) / ◐ (title ✓) / ◐ (title ✓) |

### Consensus, debates & contradictions
- **Consensus.**
  - The Gödel Machine's proof gate is replaced by empirical utility in every working system.
  - Evaluator, objective and outer selection stay human-fixed.
  - Recursion claims require matched-budget, same-start, independent-evaluation comparisons. All of 2609.11873, 2610.27610, IbI and 2610.29677 say so, and almost no paper does it.
- **Is the low self-edit rate a bug or a feature?**
  - *Bug:* Hyperagents, MetaSkill-Evolve, MetaRSI, FreeEvolve and RSI-Harness unlock meta-edits and report gains.
  - *Feature:* Metaⁿ (freeze Ω), VERSE (unverified self-evolution 31.79 < fixed 39.20), Lyapunov-Gated RSI, and Gödel Agent's collapses.
  - Different gates, budgets and models make these incomparable.
- **Code vs state.** Hyperagents says "the meta agent carries transfer" (0.620). IbI re-reads the same history as one inherited evidence-routing edit. Metaⁿ gets depth from a context string (~72%). SIM gets gains from *memory for the optimizer* (+2.36, p=0.003). AIDE²'s accepted rewrites are search policy plus context management. No transplant of improver code × improver memory exists.
- **Does evaluation need to be in the loop at all?**
  - RQGM says evolve the evaluator.
  - ELITE comes close to DGM/HGM on a 60-task SWE-bench Pro subset (85.6 vs 86.7/88.3) with *zero* candidate task evaluations, and transfers better.
  - Resolution Hypothesis says selection on noisy evals is no better than random at the candidate level.
  - The RSI Tax says verification is 75–94% of RSI cost and a cheap note baseline matches.

  Together these question the evaluate-and-select backbone of the DGM/HGM line.
- **Positive vs null improver transfer.** Positive: Hyperagents, IDM-transfer (+3.4% AUBC), FreeEvolve (same-environment). Null or negative: Hyperagents 200-iteration (n.s.), AIDE² ignition, IbI δ₁ ≈ 0, FreeEvolve cross-environment. None of the positive papers uses nested replay.
- **Framing tension over RQGM.** This is about framing, not conflicting numbers; both surveys cite the same Polyglot 71.7 vs 69.9. 2609.11873 §3.6.2 reports that number and stresses that the anchor, schedule and orchestration stay fixed. 2607.07663 §3.5 frames RQGM as taking verification-level evolution "to its logical conclusion". RQGM's own App. G says the evaluator is anchor-bounded. So it is structural L5 for evaluators, not open-ended.
- **Naming inflation.** "RSI" now labels object-level evolution with a fixed improver (SoL-Pi, SwarmRSI, Gödel Forest, many ICLR27 "X-RSI" submissions). By Duan and Dai et al.'s criterion ("reuse in later tasks alone does not establish that recursive dependency") these are not recursive.

### Open problems
1. **Order-1 / order-2 effect at scale.** Measure successor quality (IbI nested replay) across ≥3 backbones, ≥2 system families (Gödel Agent, DGM-H, IDM) and ≥3 seeds at matched tokens. IbI has 1 backbone; AIDE² has 3 seeds against a human outer agent, not AIDE₀.
2. **A separate credit channel for improver edits that is not an object-score proxy.**
   - Candidates: IDM forecast loss, HMED replay Δ, IFR, CMP, descendant score, TRUST/SGM statistical certificates.
   - None has been compared on one benchmark. Only HMED (improver edits) and IDM (its forecaster) gate meta-edits on their own signal.
3. **Why self-edits are rare.** Permission/framing (0→45%, SelfImprove-Bench), gate resolution, credit horizon, model family (Claude converge-and-reject behaviour, 2611.01304) and stationarity of tasks are all untested causally.
4. **Code-vs-memory transplant.** Cross improver code (gen 0/k) × improver memory or trackers (gen 0/k), as in SIM or the Physical-AI frozen-knowledge control, scored on successor quality.
5. **Evaluator evolution beyond the anchor.** RQGM's adversarial term trades accuracy for calibration (80% vs 88%). No method certifies an evaluator improvement where ground truth is absent (paper and proof writing are scored by automated panels).
6. **Security of the recursive channel.** Trusting-Trust shows persistent poisoning through self-evaluation. No defence is evaluated across generations.
7. **Cost-normalized recursion.** Report "validated gain per token, including verification" (RSI Tax), and ask whether a cheaper improver lowers the cost of its successor's search (SoL-Pi's untested "recursive efficient improvement").
8. **Non-stationary improvement regimes.** Benchmarks where the *optimal improver* changes (feedback granularity, verifier noise, tool drift). RQGM's curriculum and Flex-RSI's feedback-granularity result are the only hints.

### Where it is heading (to mid-2027)
- **Very likely: audit-first papers.** ICLR27 rebuttals (IbI, RQGM, Mendel GM, Gödel Ecology, RSI-Harness) and NeurIPS 2027 submissions will add self-edit rate tables, nested replay and multiple seeds. "Archive curves are not evidence" becomes a reviewer norm.
- **Likely: gate theory goes mainstream.** Statistical Gödel Machines, TRUST and Lyapunov-gated admission become the default framing of the Gödel Machine "proof", with e-values or multiple testing replacing argmax.
- **Likely: cheaper selection.** Evaluate-less or pre-evaluation scoring spreads: IDM forecasts, RSI-on-a-Budget diff scores, SIFT judges, SpecRSI drafting, ELITE integration. Expect head-to-heads against HGM at matched cost.
- **Plausible: evaluator co-evolution with anchors (RQGM, Gödel Ecology) extends to agentic web and science tasks.** Collusion and self-preference audits are the expected critique.
- **Plausible: the first credible positive ignition result.** A lab with budget (Weco, Sakana, Meta, DeepMind) runs a ≥5-seed improver swap. Given IbI and AIDE², a null or modest effect is equally likely. Forecasts are judgment estimates (†).

### Connection to our ideas
(Ideas from `/home/user/nlp-lab-paper-search/research/ONE_PAGERS.md`.)
- **#3 Gate, Not Willingness / merged 2+3 "Propose, Propagate, Pay Off".**
  - This corner supplies the natural gate arms:
    - none (DGM/Hyperagents/HGM);
    - contract (MetaRSI);
    - argmax (SICA, AIDE²);
    - replay (HMED);
    - forecast-loss (IDM, the new arm to add);
    - statistical (TRUST/SGM).
  - The Resolution Hypothesis and TRUST (45.59% inferior acceptances under a greedy gate) strengthen the premise that gates, not proposals, bottleneck. They also raise scoop risk on the "selection is the bottleneck" headline, so frame ours around *improver-level* edits, which none of them studies.
- **#2 Forced-Quota Dose.** Endpoints exist:
  - q=0: Hyperagents fixed-meta, SelfImprove-Bench fixed improver;
  - forced: STOP, RSIFlow, VERSE, Flex-RSI's 4-round review, MetaSkill H-sweep.

  IbI's positive control (+0.171) is the one "force it and it helps" datum. Our dose curve at matched compute remains open.
- **#4 Order-2 Audit.** Direct targets: Metaⁿ depth, MetaRSI's single fork, RSIFlow +20.56, AIDE² ignition (3 seeds), RQGM (2-task gap, 1 search/arm confirmed), IDM's AUBC framing. RecurLens (◐) is a close competitor for the audit framing; cite it and differentiate by order-2 and multiple systems.
- **#5 Order-0 Rewards Breed Routers.** HGM CMP, IDM forecast loss, RQGM anchor scoring and STOP meta-utility are the bake-off arms. AIDE²'s discovered bandit-over-strategy-arms and SICA's delegation fit the "router" prediction.
- **#6 Meta-Overfitting.** Positive anchors: Hyperagents meta transfer (0.630) and IDM-only transfer (+4 pp held-out). Negative anchors: FreeEvolve cross-environment, AIDE² non-monotone checkpoints. The procedure × memory 2×2 should borrow the Physical-AI frozen-knowledge control (R1/R2) and SIM's optimizer memory as the "memory" factor.
- **#9 MetaShift / #7 Varying Goals.** RQGM's epoch-wise utility change and walking→jumping curriculum (5 runs/arm) are the closest published "the improvement regime changes" manipulations, but with evaluator change, not improver necessity. Flex-RSI's feedback-granularity effect is a ready shift variable.
- **Mentor link.** Gödel Agent (Yin et al.) is the most self-referential member of the lineage and is the least audited:
  - no gate;
  - 0–6 self-mod calls per 20 iterations (Metaⁿ App J);
  - not analysed by IbI.

  Running nested replay plus gate arms on Xunjian's own codebase is the cheapest, best-positioned contribution. It also avoids the evaluate-and-select assumptions that ELITE and the Resolution Hypothesis now question.

#### Fact-check log

| claim | verdict | source location |
|---|---|---|
| RQGM DeepSWE 82.1% vs 75.0% (23 vs 21 of 28), 85.7% at med/high | confirmed (HGM-H at med/high 78.6/82.1 added) | 2606.26294 §5 (lines ~148–151), App. C.7 table, Tab. 6 split 28/57/28 |
| RQGM Polyglot 71.7 vs 69.9 at 1.35–1.72× fewer tokens; 83–90% shared-code patches | confirmed | 2606.26294 Fig. 3, §5.1, Fig. 11 / App. E.5 |
| RQGM writer 38.8% vs 21.8% (1.78×), 4-reviewer panel incl. RQGM-adversarial; prover 4.33 vs 3.73 at 88.0M vs 21.5M | confirmed | 2606.26294 Table 2, Table 3 |
| RQGM headline arms are one search per arm | confirmed (upgraded from "look like") | 2606.26294 App. C ("eight headline runs: an RQGM and an HGM-H run in each … domain"); reward design 5/5 runs Table 1 |
| RQGM evaluator bounded by anchor; adversarial reviewer less accurate | confirmed (80% vs 88% added) | 2606.26294 App. G; §5.4; Table 8 |
| RQGM no-erasure ρ ≥ 0.90 | confirmed | 2606.26294 §5 Fig. (line ~547) |
| AIDE² 0.703→0.778, 7 rewrites, 100 nodes, 8 days; other runs 2 and 4; transfer 1790/1511, 0.722/0.708, 19.9/19.6, 0.793/0.404; hacking 55→32% (39% human); ignition 0.780 vs 0.782 inconclusive | confirmed; added per-benchmark inner models (WeatherBench uses Gemini 3.1 Pro, FML GPT-5.4) and the 39% for AIDE₄₇ | 2609.26457 §3.2 (l.307–311), Table 2 protocols, §3.4, §3.6 (l.657–704) |
| AIDE² "patched the evaluation script" | confirmed, framing added (repairs a crashing scorer) | 2609.26457 §3.5 (l.641–646) |
| IDM AUBC 0.306 vs 0.210 (46%), +10.9% matched, +13.25 vs +5.42 pp, 19/28, +3.4%/+2.3%, +4/+8 pp | confirmed | 2610.13991 §4 (l.421–426, 510, 604–611) |
| IDM gate "Brier loss falls and CMSE not rise" | corrected (Brier for probabilities, squared error for continuous values; K ≥ 2) | 2610.13991 §3.4 |
| IDM dated Oct 2026 | corrected (v1 Sep 26 2026, v2 Oct 15; moved to Sep timeline row) | arXiv API metadata |
| "Only HMED has a score gate on self-edits" | corrected (inconsistent with IDM card; HMED for improver edits, IDM for its forecaster) | FTV §HMED vs 2610.13991 §3.4 |
| ELITE 85.6% SWE-bench Pro with zero candidate task evaluations | corrected/qualified (60-task subset, DeepSeek-V4-Flash, 3-run means, slightly below DGM 86.7/HGM 88.3; Qwen DGM/HGM single runs); mark ◐→✓ | 2611.21056 abstract, §5.1 (l.334), §5.2 Table 1 |
| Ren formula Σ_{t+1}=I_{Σt}(Σt;St) in §3.2 | corrected (Eq. 23 is in §6.4 Full Scaffolding; "operand" quote is §3.2) | 2607.13104 l.2590 (§6.4), l.990 (§3.2) |
| "bounded, verifiable loops" (2607.13104 §6) | confirmed, located more precisely at §6.4 takeaway | 2607.13104 l.2719 |
| Gödel Machine prover editable (2607.13104 §2.3) | confirmed | 2607.13104 §2.3 l.706–712 |
| 2607.07663 §3.5 "evolving one component … freezing the rest"; RQGM "logical conclusion" | confirmed | 2607.07663 §3.5 l.441–447 |
| Duan: L5 "recursive inheritance"; structural vs effective; "bounded meta-improvement" §3.6.5; Hyperagents 200-iter n.s. | confirmed | 2609.11873 l.282, l.432, §3.6 intro, §3.6.1 l.2221, §3.6.5 l.2330 |
| Duan: AIDE² ignition inconclusive at §3.6.5 | corrected location (§3.6.4 Industrial Practice and the future-evaluation section; §3.6.5 says accumulation "remains open") | 2609.11873 l.2276–2283, l.3200 |
| "Structural L5 shown in STOP, Gödel Agent, Hyperagents, RQGM" | corrected (Duan's Table 7 lists these and others as L5 mechanisms "qualified in the text"; explicitly says an evolved evaluator can establish structural L5) | 2609.11873 Table 7, l.2161–2165 |
| "Contradiction in reported RQGM numbers" | corrected (no numeric contradiction; retitled as framing tension) | 2609.11873 §3.6.2; 2607.07663 §3.5 |
| HGM 61.4% SWE-Verified GPT-5-mini; human-level Lite with GPT-5; 56.7 vs 53.3; 0.778/0.626 | confirmed | 2510.21614 abstract, §5, Tables 1–3 |
| HGM ICLR 2026 | unverifiable in text (arXiv v3 header has no venue) → † | 2510.21614 header |
| DGM 20→50%, 14.2→30.7%, 39.0/39.7, ~$22k, ICLR 2026 | confirmed | 2505.22954 abstract, §4, App. ablation table, App. E.1, header |
| Hyperagents 0.0→0.710, 0.060→0.372, 0.630/0.620/0.100, 0.640 vs 0.610, 0.491 vs 0.532 | confirmed | 2603.19461 §5, transfer tables (l.2328–2330), parent-selection appendix |
| Gödel Agent Table 1 numbers; 4% terminations; 14/100 MGSM | confirmed | 2410.04444 Table 1, §6.2 |
| AlphaEvolve 48 mults, ~75%/~20%, 0.7%, 23%/1%, "optimizes its own training process", ablations 3 seeds | confirmed (ablations on 2 tasks added) | 2506.13131 abstract, §3, l.1185, l.1258–1268, §4 Fig. 8 |
| SoL-Pi 42.0 vs 44.8, −49.0% tokens, −33.2% cost; Opus 5 94.3%/−44.7%; TB4 15 vs 18, −26.3%; 1,127 vs 1,366; 152/535/>3,000; 11/40 EdgeBench; "long-term research vision" | confirmed (Opus cost −33.5%, single agent 1,333 added) | 2609.20519 §2.3, §3.1–3.2, conclusion |
| SwarmRSI 74.2/76.2 vs 67.1; 44.4%; ~77% | confirmed (seed count not checked → †) | 2610.20431 abstract, Tables, l.160 |
| Physical-AI "yet to isolate gains attributable to recursion"; R1/R2 frozen-knowledge control; Oct 21 2026; NUS | confirmed | 2610.29677 abstract, §3.3 eqs, header |
| TRUST 45.59%→2.78%; SIM +2.36, p=0.003, 10 runs; RSI Tax 65.6–71.0%, 74.8–93.6%; Resolution "no better than random", 7×6, >10M calls | confirmed (marks raised to ✓ abstract/intro) | 2611.10787 abstract/intro l.130; 2611.18856 intro l.61–64; 2611.13524 abstract; 2611.23068 abstract |
| IbI numbers (119/141, 1–15%, 0.476→0.858, δ₁ +0.003, +0.293, +0.171, 0.309→0.014, 163/369, 45–62%) | confirmed against project full-text notes | research/papers/improvement-by-inheritance-notes.md |
| Id resolution: 2609.26457, 2610.13991, 2611.21056 | confirmed titles ("Recursive self-improvement of AI research agents"; "Predict Before You Modify: Learning Improvement Dynamics for Self-Evolving Agents"; "Evaluation-Free Recursive Self-Improvement for Coding Agents") | local PDF headers |
| Id resolution: 2609.36675, 2610.38266, 2610.29121, 2610.35193, 2610.14625, 2611.22299 | confirmed (titles match one-liners) | arXiv export API |
| "2611.rsi-mechanisms" survey link | corrected (non-standard alphaXiv-only slug `2611.recursive-self-improvement-llm-agents`; no arXiv id) | research/round2/verify/factcheck.md, leads-a.md |
| OpenReview ids o3txFJw4JR (RQGM), ZBxV5bEYUH (IDM) | unverifiable this pass → † | not checked |

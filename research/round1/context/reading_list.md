# Reading list so far (~120 papers, Oct 30 – Nov 12, 2026). IDs marked † are from memory — verify.

## 1. Self-improving agent lineage
STOP 2310.02304† | ADAS 2408.08435 (ICLR25; frozen meta agent) | Gödel Agent 2410.04444 (ACL25; complexity ceiling; no budget-matched frozen-improver control) | SICA 2504.15228† | Darwin Gödel Machine 2505.22954 (ICLR26; SWE-bench 20→50%, ~$22k/run, objective hacking) | AlphaEvolve 2506.13131† | Huxley-Gödel Machine 2510.21614 (clade metaproductivity) | EvoX 2602.23413 (meta-evolve search strategy; matched iterations not cost) | Hyperagents/DGM-H 2603.19461 (imp@k; transfer imp@50=0.630 unreplicated; compounding 0.640 vs 0.610 n.s.; fixed task distribution = future work) | Red Queen Gödel Machine 2606.26294 (co-evolves evaluators) | Metaⁿ 2608.24735 (stacked frozen meta-layers; depth stalls) | AIDE² 2609.26457 (Weco; ignition test inconclusive at 3 seeds) | IDM-guided Hyperagents 2610.13991 (predict outcomes before eval). Also: Weco ignition-test blog; Oct 2026 physical-AI paper w/ two recursion tests.

## 2. Harness evolution, frozen model (crowded)
Meta-Harness 2603.28052 (COLM26) | AHE 2604.25850 (loses to test-time scaling at matched budget) | Code as Agent Harness 2605.18747 | HarnessX 2606.14249 (post-peak collapse) | Self-Harness 2606.09498 | Bayesian-Agent 2606.08348 | Gated Semantic QD 2607.13683 (credit assignment of edits) | RHI 2607.15524 (Sakana/Berkeley) | HSI 2608.08466 | DarwinX 2608.07545 (300 synthetic intents → 1,260 real held-out; static synth-to-real) | JIT-Agent 2608.25593 | Harness Learning for TTA 2609.35738 | RRSI 2609.24972 (Google; paired bootstrap; unregularized retains little OOD gain) | Self-Evolving Harness 2609.38372 | TTSE 2609.24289 | Self-Evolving Agent Harness theory 2610.18813 (continual-PAC) | RiseHarness 2610.27435 (flip tasks). No-ID: VeRO, AIRA_2, DemoEvolve, APEX, Evolving Agents in the Dark, NLAH 2603.25723, Last Harness 2604.21003, AutoDesign 2608.13560, Continual Harness 2605.09998.

## 3. Harness ↔ weight co-evolution (≥8 papers; "we co-evolve both" not a contribution)
SIA 2605.27276 (names coupled co-evolutionary Goodhart, unmeasured) | HASE 2607.03935 | MetaEvolve 2607.21971 | Co-Harness 2607.22688 | HELIX 2608.13951 (held-vs-rebuilt unrun) | WHALE 2609.00196 (Finn) | SafeEvolve 2609.02786 | On-Policy Correction 2609.09134 | Harness-Zero 2609.24974 | Harness Evolution Hits a Ceiling 2610.11655 (2×2 harness×LoRA; process vs content failures) | Evo² 2610.13025 (Google; trains Proposer/Reflector) | CoEvo-RSI 2610.25078 (10 rounds; brain–body mismatch; high scoop risk for multi-round) | Co-Evolving Skill Gen & Policy Opt 2606.08755 | WeAct 2610.26847.

## 4. Evaluation rigor, confounds, Goodhart
Rethinking the Evaluation of Harness Evolution 2607.12227 (matched budget: evolution 67.4 vs parallel sampling 72.3; held-out gain ≈0) | Harness Updating ≠ Benefit 2605.30621 (EMNLP26; evolver capability ≤3.1pp; Qwen3.5-9B ≈ Opus) | Scaling Laws via Effective Feedback Compute 2605.29682 | Verification Horizon 2606.26300 | Who Grades the Grader / Double Ratchet 2607.12790; 2610.11464 | HackProbe 2609.04665 | Security audit DGM/Hyperagents 2609.17817 (git-history answer leak) | CHASE 2609.18366 | SpecBench. No-ID: Salesforce task-order-shuffle collapse; Predict Before You Modify (AUBC); tampering audit; misevolution (ICLR26); SHE 2608.09885; HALO; BreakingWeb (false success claims 54–85%).

## 5. Benchmarks for self-improvement (crowded since Jun–Sep 2026)
SEAGym 2606.17546 | EvoAgentBench 2607.05202 | RSIBench-Data 2607.25886 | GDPevo 2608.03764 | HarnessOpt-Bench 2608.06301 | Evo-Bench 2608.09096 (AnytimeVal) | AI4AI-Bench 2608.20318 | EvoHarnessBench 2609.04280 | EvalResearchBench 2610.04184. No-ID: SEA-Eval, SkillLearnBench, SEABench, ContinualSkillBench, Meta-Agent Challenge.

## 6. Curriculum / open-endedness / self-generated tasks
Classic: Teacher-Student CL 1707.00183† | POET 1901.01753† | AI-GAs 1905.10985† | PLR 2010.03934† | PAIRED 2012.02096† | ACCEL 2203.01302† | MAP-Elites 1504.04909† | OMNI 2306.01711† | OMNI-EPIC 2405.15568†.
LLM weights: Self-Instruct 2212.10560† | Absolute Zero 2505.03335† | SEAL self-adapting LMs 2506.10943 | R-Zero 2508.05004 | Agent0 2511.16043 | R-Few 2512.02472 | CoEvolve 2604.15840 | SEAL co-evolution agents/envs 2605.24426 | Tune the Environment 2510.10197. No-ID: GenEnv, Agent World Model (ICML26), EnvFactory, RWML 2602.05842.

## 7. Prompt/context optimization, skill/memory evolution
DSPy 2310.03714† | GEPA 2507.19457 | ACE 2510.04618† | Memento 2508.16153 | ReasoningBank 2509.25140 | Evolver 2510.16079 | AgentEvolver 2511.10395 | MemRL 2601.03192 | Self-Consolidation 2602.01966 | EvoTool 2603.04900 | SkillOpt 2605.23904 | Ratchet 2605.22148 | PRO-LONG 2607.20064.

## 8. Surveys & testbeds
Surveys: 2606.20683, 2607.07663, 2607.13104, 2609.11873, 2608.06216. Testbeds: τ-bench 2406.12045†, τ²-bench 2506.07982†, AppWorld, Terminal-Bench 2.x, SWE-bench Verified, Polyglot, WebArena-Infinity.

## Must-read shortlist
Cluster 1: Gödel Agent; Hyperagents; HGM; AIDE²; Harness Updating ≠ Benefit. Cluster 2: R-Few; Agent0; OMNI-EPIC; DarwinX; GEPA. Cluster 3: Ceiling; CoEvo-RSI; Co-Harness; WHALE. Methods: Rethinking Eval; RRSI; Security audit 2609.17817.

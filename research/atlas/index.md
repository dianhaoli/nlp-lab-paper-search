## Master paper index

Every paper or system mentioned in the four chapters supplied, deduplicated by id. When a paper has both an arXiv id and an OpenReview id, both are given in one row.

**Chapter key:**
- 1 = Self-referential & recursive self-improvement (RSI) lineage
- 2 = Meta-agents & automated agent/workflow design
- 3 = Harness evolution with frozen models
- 4 = Improver-level RSI. The source text of this chapter was cut off partway through the Promptbreeder card, so chapter-4 coverage stops there.

**Sorting.** Rows are sorted by home chapter, then date. The home chapter is the first chapter that gives the paper its own entry (a paper card, a row in a "Remaining papers" table, or a timeline row). If no chapter does, it is the chapter that first mentions the paper.

**Dates:**
- arXiv papers use the year and month of the id (YYMM), plus the day where a chapter states one.
- OpenReview-only papers are ICLR 2027 submissions (Sep–Nov 2026). They come after the dated rows of their chapter.
- Rows with no id come last in each chapter.

**Verification marks.** Where chapters disagree, the strongest mark is given, and the weaker one follows in brackets.
- ✓ = full text read this session
- ✓FTV = full text read in the Nov 13 pass, recorded in FULLTEXT_VERIFICATION.md
- ✓ᵖ = full text read in an earlier project round
- ◐ = abstract only
- † = unverified or from memory

### Chapter 1 home

| id (link) | title | date | chapter(s) | one-line | mark |
|---|---|---|---|---|---|
| — | Schmidhuber self-referential learning | 1987 | 1 | Origin of fully self-referential learners | † |
| — | Gödel Machine (Schmidhuber) | 2003 | 1, 4 | Rewrites its own code only when a proof shows the rewrite raises utility; never practical | ✓ (via 2607.13104 §2.3) |
| [2309.16797](https://arxiv.org/abs/2309.16797) | Promptbreeder | 2023-09 | 1, 4 | Mutation prompts evolve alongside task prompts; zero-order hyper-mutation produced an improvement 42% of the times it was applied | ✓ (ch4; ◐ ch1) |
| [2310.02304](https://arxiv.org/abs/2310.02304) | STOP: Self-Taught Optimizer | 2023-10 | 1, 2, 4 | Improver program rewrites itself, scored by one-step meta-utility; GPT-4 improves while GPT-3.5 and Mixtral decline | ✓FTV |
| [2408.08435](https://arxiv.org/abs/2408.08435) | ADAS / Meta Agent Search | 2024-08 | 1, 2, 3, 4 | Fixed GPT-4 meta-agent writes agent `forward()` code from an archive; independent reproductions put it at or below IO/CoT | ✓ (ch2) |
| [2410.04444](https://arxiv.org/abs/2410.04444) | Gödel Agent | 2024-10 | 1, 2, 4 | Monkey-patches its own policy and learner at runtime with no gate; 14/100 MGSM runs end below the initial policy | ✓ (Table 1, §6.2) / ✓FTV |
| [2504.15228](https://arxiv.org/abs/2504.15228) | SICA | 2025-04 | 1, 4 | One codebase is both agent and improver, argmax over the archive; SWE-Bv subset 17→53%, n=1 | ✓FTV |
| [2505.22954](https://arxiv.org/abs/2505.22954) | Darwin Gödel Machine | 2025-05 | 1, 2, 4 | Open-ended archive of self-modifying coding agents; SWE-bench 20→50%, about $22k per run | ✓ |
| [2506.13131](https://arxiv.org/abs/2506.13131) | AlphaEvolve | 2025-06 | 1 | Evolutionary coding agent with meta-prompt evolution; 48-multiplication 4×4 matmul; cut Gemini training time by 1% | ✓ |
| [2510.21614](https://arxiv.org/abs/2510.21614) | Huxley-Gödel Machine (HGM) | 2025-10 | 1, 4 | Expands by clade-metaproductivity (CMP); 61.4% on full SWE-Verified with GPT-5-mini | ✓ |
| [2602.23413](https://arxiv.org/abs/2602.23413) | EvoX: Meta-Evolution for Automated Discovery | 2026-02 | 1, 4 | Search strategy meta-evolves on stagnation; Frontier-CS median 75.5 vs 56.2 | ✓ (ch4; ◐ ch1) |
| [2603.19461](https://arxiv.org/abs/2603.19461) | Hyperagents (DGM-H) | 2026-03 | 1, 2, 3, 4 | Task agent and meta-agent in one editable repo; paper review 0.0→0.710; meta transfer imp@50 0.630 | ✓ / ✓FTV |
| [2606.26294](https://arxiv.org/abs/2606.26294) / [o3txFJw4JR](https://openreview.net/forum?id=o3txFJw4JR) | Red Queen Gödel Machine (RQGM) | 2026-06-24 | 1, 4 | Co-evolved evaluators frozen per epoch, with a ground-truth anchor and erasure; DeepSWE 82.1 vs 75.0 | ✓ (OpenReview id †) |
| [2607.05297](https://arxiv.org/abs/2607.05297) | MetaSkill-Evolve | 2026-07-06 | 1, 4 | Two-timescale meta-skill with forced updates every H=2 and no gate | ✓FTV / ✓ |
| [2607.07663](https://arxiv.org/abs/2607.07663) | Self-evolution survey (Chen et al.) | 2026-07 | 1, 2, 3, 4 | Survey: substrate × loop closure; §3.5 covers harness and agent self-evolution | ✓ |
| [2607.13104](https://arxiv.org/abs/2607.13104) | RSI survey (Ren/Zhuge/Schmidhuber) | 2026-07 | 1, 2, 3, 4 | Survey: θ vs Σ, Eq. 23 Σ_{t+1}=I_{Σt}(Σt;St), meta-level skills | ✓ |
| [2607.15524](https://arxiv.org/abs/2607.15524) | Recursive Harness Self-Improvement (RHI, Sakana) | 2026-07 | 1, 3 | Prompt-level harness spec refined by pairwise feedback over its own revision history; up to 60% lower cost | ◐ |
| [2608.07645](https://arxiv.org/abs/2608.07645) / [co3NCcIZ7f](https://openreview.net/forum?id=co3NCcIZ7f) | Mendel Gödel Machine | 2026-08 | 1 | Reaction-norm and cross-lineage hybridization mutations | ◐ |
| [2608.24735](https://arxiv.org/abs/2608.24735) | Metaⁿ: RSI through Emergent Depth | 2026-08 | 1, 2, 4 | Frozen meta-operation Ω recursed on its input; about 72% of the gain comes from a context string | ✓FTV |
| [2609.06396](https://arxiv.org/abs/2609.06396) | MetaRSI-v1 / RSI² | 2026-09-06 | 1, 4 | Scheduled meta-terms with a contract-only improver gate; +2.3 from one fork, 5 seeds | ✓FTV |
| [2609.11873](https://arxiv.org/abs/2609.11873) | RSI survey (Duan et al.) | 2026-09 | 1, 2, 3, 4 | Survey: levels L1–L5, structural vs effective recursion; "bounded meta-improvement" | ✓ |
| [2609.17817](https://arxiv.org/abs/2609.17817) | Reflections on Trusting Trust, Revisited | 2026-09 | 1 | Poisoned benchmarks make DGM, SICA and Hyperagents evolve vulnerable code, and the contamination persists | ◐ |
| [2609.19526](https://arxiv.org/abs/2609.19526) | SIFT (MIT/Sakana) | 2026-09 | 1 | Pairwise Bradley-Terry LLM-judge scores guide tree search; cheaper than DGM/HGM | ◐ |
| [2609.20519](https://arxiv.org/abs/2609.20519) | SoL-Pi | 2026-09-17 | 1, 3 | Auto-research funnel over the harness; 93.7% of Pi's score with 49% fewer tokens; not recursive | ✓ |
| [2609.26457](https://arxiv.org/abs/2609.26457) | AIDE²: Recursive self-improvement of AI research agents | 2026-09-22 | 1 | Bi-level research-agent search; grade 0.703→0.778; ignition test inconclusive | ✓ |
| [2609.36675](https://arxiv.org/abs/2609.36675) | Gödel Forest: Balancing Search Depth and Breadth for Data-Centric RSI | 2026-09 | 1 | Data-centric RSI as co-evolving trees; +10.70% vs a single agent | ◐ (title ✓) |
| [2610.13991](https://arxiv.org/abs/2610.13991) / [ZBxV5bEYUH](https://openreview.net/forum?id=ZBxV5bEYUH) | Predict Before You Modify (IDM-guided Hyperagents) | 2026-09-26 (v2 Oct 15) | 1 | Editable forecaster with its own loss gate; Polyglot AUBC 0.306 vs 0.210 | ✓ (OpenReview id †) |
| [2610.02616](https://arxiv.org/abs/2610.02616) / [9WwpxPfcji](https://openreview.net/forum?id=9WwpxPfcji) | VERSE: Verified Self-Evolving Optimizer | 2026-10-02 | 1, 3, 4 | Optimizer self-evolution helps only with execution verification; 42.3 vs 39.2 | ✓FTV / ✓ |
| [2610.07979](https://arxiv.org/abs/2610.07979) / [AnN0NpAVWL](https://openreview.net/forum?id=AnN0NpAVWL) | HMED: Hindsight Meta-Experience Distillation | 2026-10-06 | 1, 4 | Matched-start replay gate on improver edits; ALFWorld 54.11 | ✓FTV / ✓ |
| [2610.09197](https://arxiv.org/abs/2610.09197) / [8UyLPIcKqX](https://openreview.net/forum?id=8UyLPIcKqX) | FreeEvolve | 2026-10 | 1, 3, 4 | Meta-evolved evolution skill scored on fresh targets; +6.9 same-environment, negative cross-environment transfer | ✓FTV |
| [2610.11464](https://arxiv.org/abs/2610.11464) | Co-evolved verifier | 2026-10 | 1 | Editable-evaluator approach (taxonomy class E) | † |
| [2610.14625](https://arxiv.org/abs/2610.14625) | RSI survey ("two perspectives") | 2026-10 | 1 | Survey: existing methods → new ideas; 9-dimension design space | ◐ (title ✓) |
| [2610.14662](https://arxiv.org/abs/2610.14662) | Does Failure Feedback Direct Self-Modification? | 2026-10 | 1, 2 | DGM edits are not target-directed; a selection-only null reproduces the trajectories | ✓FTV |
| [2610.16491](https://arxiv.org/abs/2610.16491) | SpecRSI | 2026-10 | 1, 4 | Drafts successors before predecessors finish evaluation; GVO-trained improver; beats MetaRSI by 1.30–2.70 pp | ✓ (ch4 headline numbers; ◐ ch1) |
| [2610.20431](https://arxiv.org/abs/2610.20431) | SwarmRSI | 2026-10 (v3 Nov 2) | 1 | Swarm evolves inherited research states; OpenProblems average 74.2/76.2 | ✓ (main text) |
| [2610.21791](https://arxiv.org/abs/2610.21791) | RSIFlow: Adaptive Self-Improvement Workflows with an Evolving Meta-Agent | 2026-10-15 (v2 Nov 2) | 1, 2, 4 | Ungated meta-procedure updates every round; Meta4 vs Meta0 +20.56, n=1 | ✓ / ✓FTV |
| [2610.21887](https://arxiv.org/abs/2610.21887) | MetaShare | 2026-10 | 1 | Hyperagent-style task-agent + meta-agent program (taxonomy class C) | † |
| [2610.23977](https://arxiv.org/abs/2610.23977) | Token-matched evolution vs independent sampling | 2026-10 | 1 | At matched tokens, evolution is ≤5% better than sampling on 12/15 pairs | ◐ (via round3) |
| [2610.27610](https://arxiv.org/abs/2610.27610) | Auditing the Product, Not the Recursion | 2026-10 | 1, 4 | 114 studies audited; only 4/26 improver claims are same-start and matched-budget | ✓ (project) / ◐ |
| [2610.29121](https://arxiv.org/abs/2610.29121) | RSI on a Budget: Scoring Code Edits Before They Run | 2026-10 | 1 | Linear readout scores diffs before running them; HumanEval+ 87.0 vs 85.1 | ◐ (title ✓) |
| [2610.29677](https://arxiv.org/abs/2610.29677) | Physical-AI RSI (survey) | 2026-10-21 | 1 | R1/R2 links with a frozen-knowledge control; physical studies have yet to isolate recursion gains | ✓ (abstract, §2–3) |
| [2610.30042](https://arxiv.org/abs/2610.30042) | HGM-Dual | 2026-10 | 1, 3 | Archive search variant of HGM (taxonomy class B) | † |
| [2610.35193](https://arxiv.org/abs/2610.35193) | Trustworthy RSI survey | 2026-10 | 1 | Survey: trustworthy-RSI S1–S4 + RI | ◐ (title ✓) |
| [2610.38266](https://arxiv.org/abs/2610.38266) | Recursive Self-Improvement in Physical AI: Mechanisms, Evidence, and Evaluation | 2026-10 | 1 | Sibling survey to 2610.29677 | ◐ (title ✓) |
| [2610.41157](https://arxiv.org/abs/2610.41157) | Lineage Is Memory | 2026-10 | 1 | 1,750 pre-registered runs: greedy parent inheritance drives progress | ✓FTV |
| [2611.01304](https://arxiv.org/abs/2611.01304) | Claude converge-and-reject behaviour | 2026-11 | 1 | Model-family effect on how often systems self-edit | † |
| [2611.02797](https://arxiv.org/abs/2611.02797) | SelfImprove-Bench | 2026-11 | 1, 2, 3, 4 | 0% retained improver edits by default; 20.5–45.2% when the framing invites them | ✓FTV |
| [2611.08946](https://arxiv.org/abs/2611.08946) | Flex-RSI | 2026-11-06 | 1, 4 | 24 vision/robotics tasks × 18 models; forced self-review every 4 rounds; improves 6/7 pairs, n=1 each | ✓FTV |
| [2611.10787](https://arxiv.org/abs/2611.10787) / [wV4bK1dHpC](https://openreview.net/forum?id=wV4bK1dHpC) | TRUST | 2026-11 | 1, 3, 4 | Online multiple-testing gate; accepted-inferior rate 45.59%→2.78% | ✓ (abstract + intro) |
| [2611.13524](https://arxiv.org/abs/2611.13524) | The RSI Tax | 2026-11-10 | 1, 3, 4 | A bounded-note baseline matches Meta-Harness, EvoSkill and RRSI with 65.6–71.0% fewer tokens; verification is 74.8–93.6% of the tax | ✓ |
| [2611.18856](https://arxiv.org/abs/2611.18856) | SIM: Self-Improvement Memory Enables Continual RSI | 2026-11-13 | 1, 3, 4 | Memory for the optimizer; +2.36 vs harness-only continuation, p=0.003 | ✓ |
| [2611.21056](https://arxiv.org/abs/2611.21056) / [8SkxedsVil](https://openreview.net/forum?id=8SkxedsVil) | ELITE: Evaluation-Free RSI for Coding Agents | 2026-11 | 1, 4 | No task evaluations in the loop; 85.6 on a 60-task SWE-bench Pro subset vs DGM 86.7 / HGM 88.3 | ✓ (§5.2) |
| [2611.22299](https://arxiv.org/abs/2611.22299) | Harnesses That Improve Themselves (Google) | 2026-11 | 1, 4 | Four editable runtime components in a diagnose-propose-validate-deploy loop | ◐ (title ✓) |
| [2611.23068](https://arxiv.org/abs/2611.23068) | The Resolution Hypothesis | 2026-11 | 1 | Selection on noisy evaluations is "no better than random" across 7 methods × 6 models | ✓ (abstract) |
| [2611.23219](https://arxiv.org/abs/2611.23219) | Do LLM Agents Still Need to Be Told How to Self-Improve? (MIT) | 2026-11 | 1, 4 | A frozen agent given primitives and a budget builds its own self-learning loop | ◐ |
| [alphaXiv 2611.recursive-self-improvement-llm-agents](https://www.alphaxiv.org/abs/2611.recursive-self-improvement-llm-agents) | RSI mechanisms survey (Dai/Ruan; alphaXiv only) | 2026-11 | 1, 2, 3, 4 | Survey: collect→verify functions; three survival patterns, including "improver update" | ✓ (project) / † |
| [ZMmJDeDTXk](https://openreview.net/forum?id=ZMmJDeDTXk) | Improvement by Inheritance: Measuring RSI in Coding Agents | ICLR27 sub (ch4 says Sep, ch1 says Nov 2026) | 1, 2, 4 | Nested replay; successor gain δ₁=+0.003; gains come from inheritance | ✓ (project notes) |
| [N4GJVmrzsF](https://openreview.net/forum?id=N4GJVmrzsF) | Gödel Ecology | ICLR27 sub | 1 | Agent–evaluator co-evolution credited by co-clade metaproductivity | ◐ |
| [0FWBc4gY08](https://openreview.net/forum?id=0FWBc4gY08) | Statistical Gödel Machines | ICLR27 sub | 1 | Statistical certificate replaces the proof; P(ever accept a non-improving edit) ≤ α | ◐ |
| [vXnQX90fv6](https://openreview.net/forum?id=vXnQX90fv6) | RecurLens | ICLR27 sub | 1 | Executable recursion audit; descendant is +5.38 over frozen but −17.09 vs its own initial state | ◐ |
| [nVl2qz74V7](https://openreview.net/forum?id=nVl2qz74V7) | SelfLineage | ICLR27 sub | 1, 4 | Descendant-aware edit history with shuffled-link controls | ◐ |
| [EOrtreQLbv](https://openreview.net/forum?id=EOrtreQLbv) | RSI-Harness: Optimizing the Harness Optimizer via RSI | ICLR27 sub | 1, 3, 4 | Same-start gated improver loop; improver gain 1.3→6.2; improver transfers up to 4.5× | ◐ |
| [sBH9cdajAw](https://openreview.net/forum?id=sBH9cdajAw) | Looped-RSI | ICLR27 sub | 1, 4 | Improver loop credited by descendant consolidation | ◐ |
| [eIJH6HV9LZ](https://openreview.net/forum?id=eIJH6HV9LZ) | IFR / Meta-Forgetting | ICLR27 sub | 1, 4 | Replays improvement functions on frozen past states; +2.3–2.6 pp | ◐ |
| [3t0j2Y05EH](https://openreview.net/forum?id=3t0j2Y05EH) | Lyapunov-Gated RSI (EvoLyapunov) | ICLR27 sub | 1, 4 | Drift-condition gate for admitting improver edits | ◐ |
| [OnzWTehIaN](https://openreview.net/forum?id=OnzWTehIaN) | Why Is RSI Hard to Verify? | ICLR27 sub | 1 | Finite-horizon verification criterion | ◐ |
| [9UsBcc69Ua](https://openreview.net/forum?id=9UsBcc69Ua) | Measured Behavior, Recorded Zero | ICLR27 sub | 1 | In HGM, invalid and do-nothing children both score 0.0 | ◐ |
| — | A-Evolve-Training | 2026 | 1 | Research-agent RSI; listed as L5 in Duan Table 7 | † |
| — | OpenEvolve | 2025 | 1, 4 | Open AlphaEvolve-style evolver; baseline in EvoX | † |
| — | ShinkaEvolve | 2025 | 1, 2, 4 | Evolutionary code search; baseline in Simple Baselines and EvoX | † |
| — | EvoSkill | 2026 | 1, 3 | Skill-evolution method included in the RSI Tax comparison | † |

### Chapter 2 home

| id (link) | title | date | chapter(s) | one-line | mark |
|---|---|---|---|---|---|
| [2402.16823](https://arxiv.org/abs/2402.16823) | GPTSwarm: Language Agents as Optimizable Graphs | 2024-02 | 2 | Optimizes node prompts and edges (REINFORCE) over agent graphs | ◐ |
| [2406.14228](https://arxiv.org/abs/2406.14228) | EvoAgent | 2024-06 | 2 | Evolutionary operators grow one specialist into a multi-agent population | ◐ |
| [2410.06153](https://arxiv.org/abs/2410.06153) | AgentSquare: Automatic LLM Agent Search in Modular Design Space | 2024-10 | 2 | Module evolution and recombination with a surrogate; a random module combination beats hand-designed agents on 4/6 tasks | ✓ |
| [2410.10762](https://arxiv.org/abs/2410.10762) | AFlow: Automating Agentic Workflow Generation | 2024-10 | 2, 3 | MCTS over operator graphs; average 80.3; workflows are executor-specific | ✓ |
| [2502.04180](https://arxiv.org/abs/2502.04180) | MaAS: Multi-agent Architecture Search via Agentic Supernet | 2025-02 | 2 | Query-dependent supernet sampling; average 83.59; $3.38 vs AFlow's $22.50 | ✓ |
| [2502.04306](https://arxiv.org/abs/2502.04306) | ScoreFlow | 2025-02 | 2 | 8B workflow generator trained with Score-DPO; 85.3 vs 80.6 for an AFlow re-run | ✓ |
| [2503.03686](https://arxiv.org/abs/2503.03686) | MAS-GPT | 2025-03 | 2 | SFT on query→MAS-code pairs, so a MAS comes out in one inference | ◐ |
| [2504.04785](https://arxiv.org/abs/2504.04785) | Weak-for-Strong (W4S) | 2025-04 | 2 | RL-trained weak meta-agent designs workflows for strong executors | ◐ |
| [2504.15257](https://arxiv.org/abs/2504.15257) | FlowReasoner: Reinforcing Query-Level Meta-Agents | 2025-04 | 2 | R1-distilled meta-agent trained with RL on execution reward; 81.89 overall on code | ✓ |
| [2505.14996](https://arxiv.org/abs/2505.14996) | MAS-Zero | 2025-05 | 2 | Inference-time per-instance MAS design with no validation set | ◐ |
| [2506.06017](https://arxiv.org/abs/2506.06017) | AgentSwift | 2025-06 | 2 | Value-guided hierarchical search over workflows and components | ◐ |
| [2507.18224](https://arxiv.org/abs/2507.18224) | ARG-Designer | 2025-07 | 2 | Autoregressive generation of agent composition and topology | ◐ |
| [2507.19457](https://arxiv.org/abs/2507.19457) | GEPA | 2025-07 | 2, 3, 4 | Reflective prompt evolution; standard text-level baseline | † |
| [2507.22606](https://arxiv.org/abs/2507.22606) | MetaAgent (FSM) | 2025-07 | 2 | Finite-state-machine MAS generated from a task description | ◐ |
| [2508.08053](https://arxiv.org/abs/2508.08053) | AdaptFlow | 2025-08 | 2 | MAML-style learning of a workflow initialization | ◐ |
| [2601.14652](https://arxiv.org/abs/2601.14652) | MAS-Orchestra | 2026-01 | 2 | RL orchestration as function calling; controlled MAS-vs-single-agent benchmarks | ◐ |
| [2602.16805](https://arxiv.org/abs/2602.16805) | Simple Baselines are Competitive with Code Evolution | 2026-02 (v2 Oct) | 2 | IID random sampling matches ADAS and Shinka; all lose to majority vote on test; val→test drops >10% | ✓ |
| [2603.22791](https://arxiv.org/abs/2603.22791) | ABSTRAL: Automatic Design of MAS Through Iterative Refinement and Topology Optimization | 2026-03 | 2 | Natural-language design document refined by contrastive trace analysis; measures a coordination tax | ◐ (title ✓) |
| [2603.28052](https://arxiv.org/abs/2603.28052) | Meta-Harness: End-to-End Optimization of Model Harnesses | 2026-03-30 | 1, 2, 3, 4 | Proposer reads all prior code and raw traces; trace ablation 56.7 vs 41.3; TB2 76.4 | ✓ (ch3) / ✓ᵖ |
| [2605.14212](https://arxiv.org/abs/2605.14212) | MetaAgent-X | 2026-05 | 2 | Joint RL of designer and executor; +11.17 over a single agent on Qwen3-8B | ✓ |
| [2605.25233](https://arxiv.org/abs/2605.25233) | Meta-Agent: From Task Descriptions to Verified Multi-Agent Systems | 2026-05 | 2 | DAG of agent specs with I/O contracts, verified at construction time | ◐ (title ✓) |
| [2606.04455](https://arxiv.org/abs/2606.04455) | The Meta-Agent Challenge (MAC) | 2026-06 | 2 | CLI agents as meta-agents beat human scaffolds in only 5/39 configurations; 5 reward-hacking trials | ✓ |
| [2606.13003](https://arxiv.org/abs/2606.13003) | The Illusion of Multi-Agent Advantage | 2026-06 | 2 | CoT-SC beats automated MAS, often at less than 10% of the cost | ✓ |
| [2607.12227](https://arxiv.org/abs/2607.12227) / [QDdWiaNkk2](https://openreview.net/forum?id=QDdWiaNkk2) | Rethinking the Evaluation of Harness Evolution for Agents | 2026-07 (v4 Oct 5) | 2, 3 | At matched budget on TB2.1, harness evolution 67.4 vs parallel sampling 72.3 | ✓ |
| [2608.02353](https://arxiv.org/abs/2608.02353) | GRAFT | 2026-08 | 2 | Grafts region-level workflow replacements per input | ◐ |
| [2608.06301](https://arxiv.org/abs/2608.06301) | HarnessOpt-Bench | 2026-08 | 2, 3 | Benchmark of LLMs as harness optimizers, with a sealed test split enforced by a TEE | ◐ |
| [2608.07196](https://arxiv.org/abs/2608.07196) | EMAS | 2026-08 | 2 | Revises the deployed MAS only when a diagnosis recurs and passes paired validation | ◐ |
| [2608.17471](https://arxiv.org/abs/2608.17471) | When AI Designs AI: Innovation or Imitation? | 2026-08 | 2 | Measures novelty vs performance of agent-designed methods | ◐ |
| [2608.20318](https://arxiv.org/abs/2608.20318) | AI4AI-Bench | 2026-08 | 2 | Agents rewrite training algorithms across 10 repos | ◐ |
| [2609.28614](https://arxiv.org/abs/2609.28614) | Reward-hacking rates of research agents | 2026-09 | 2 | Hacking rate 30.5% vs 2.9% depending on evaluator openness | † |
| [2609.37151](https://arxiv.org/abs/2609.37151) | FlowMAS | 2026-09 | 2 | GFlowNet generation of MAS topologies | ◐ |
| [2609.38294](https://arxiv.org/abs/2609.38294) | MoFlow | 2026-09 | 2 | Multi-objective convex-hull MCTS over workflows | ◐ |
| [2610.02396](https://arxiv.org/abs/2610.02396) | Inherit-MAS | 2026-10 | 2 | Test-time MAS evolution with workflow and execution inheritance | ◐ |
| [2610.05284](https://arxiv.org/abs/2610.05284) | Fusion is the New Mutation: Bandit-Guided Evolution on Workflow Graphs (DAGO) | 2026-10 | 2 | LinUCB bandit chooses which parent workflows to fuse | ◐ (title ✓) |
| [2610.07763](https://arxiv.org/abs/2610.07763) | ST-Bench | 2026-10 | 2 | Spatial-temporal benchmark for MAS generation on scientific tasks | ◐ |
| [2610.07787](https://arxiv.org/abs/2610.07787) | OOPMAS | 2026-10 | 2 | Object-oriented query-level MAS | ◐ |
| [2610.07851](https://arxiv.org/abs/2610.07851) | RA-MoWE | 2026-10 | 2 | Clusters queries by workflow affinity | ◐ |
| [2610.07860](https://arxiv.org/abs/2610.07860) | WorkflowOps | 2026-10 | 2 | Learned collaboration priors | ◐ |
| [2610.09239](https://arxiv.org/abs/2610.09239) | Winner's-curse study | 2026-10 | 2 | Winner's curse in selecting under noisy validation | † |
| [2610.14481](https://arxiv.org/abs/2610.14481) | Optimize Once, Execute Fast | 2026-10 | 2 | Latency-aware MAS workflow learning for recurring queries | ◐ |
| [2610.22779](https://arxiv.org/abs/2610.22779) | When Can You Trust an Agent's Own Verifier? | 2026-10 | 2 | Pressure ratio ρ plus 4 falsifiable probes for agents' own verifiers | ◐ |
| [2610.27531](https://arxiv.org/abs/2610.27531) | AMOD: Automated Multi-Agent Organization Design | 2026-10 | 2 | Position paper: search for functionally necessary responsibilities | ✓ (abstract + Fig. 1) |
| [2610.29276](https://arxiv.org/abs/2610.29276) | Bayesian Evolutionary Optimization of MAS (BEO) | 2026-10 | 2 | Graph-kernel GP with EI replaces LLM candidate selection | ◐ |
| [2610.31436](https://arxiv.org/abs/2610.31436) | Conservative Meta-Agent Search (CMAS) + MetaInvest-Bench | 2026-10 | 2 | Calibrated incumbent replacement; 3.4% false promotions | ✓ |
| [2610.31709](https://arxiv.org/abs/2610.31709) | Mining the Design Space of Multi-Agent Workflows | 2026-10 | 2 | 28,568 AFlow workflows mined; SharedMemory vs debate Δ +0.135 | ✓ |
| [2610.35966](https://arxiv.org/abs/2610.35966) | EMMA | 2026-10 | 2 | Episodic memory for MAS workflow synthesis | ◐ |
| [2610.38946](https://arxiv.org/abs/2610.38946) | MAS-Loop | 2026-10 | 2 | Orchestrator plus a trajectory-conditioned Refiner | ◐ |
| [2611.01870](https://arxiv.org/abs/2611.01870) | ASpec: Automated Stateful Specialization | 2026-11 | 2 | Evolves specialist archetypes with retain-then-escalate control | ◐ |
| [2611.12856](https://arxiv.org/abs/2611.12856) | WorkflowArena | 2026-11 | 2 | 145 long-horizon tasks; benchmarks the delegation workflows Claude Code and Codex write | ✓ (abstract) |
| [2611.15754](https://arxiv.org/abs/2611.15754) | MULTILOOP: Evolving Multi-Agent Harnesses for Complex Information-Seeking Tasks | 2026-11 | 2, 3 | Multi-agent harness evolution on DeepSearchQA; +4.6 | ◐ (title ✓) |
| [2611.19037](https://arxiv.org/abs/2611.19037) | The Critic Can Hurt: On the Safety of Critics in LLM-Guided AutoML Search | 2026-11 | 2 | A naive 8B critic pushes search below the random floor on 7/8 datasets | ◐ (title ✓) |
| [2611.20997](https://arxiv.org/abs/2611.20997) | OpenMAS-GApp | 2026-11 | 2 | One-for-all benchmark for building MAS applications | ◐ |
| [h2I3OGkGtU](https://openreview.net/forum?id=h2I3OGkGtU) | What Is One Search Operation Worth? | ICLR27 sub | 2 | Same-state counterfactual value of each design-search operation; selection gap 1.52 pp | ◐ |
| [0b4wysHrzh](https://openreview.net/forum?id=0b4wysHrzh) | MetaEvoForge: Meta-Evolving Agents for Cross-Task Agent Improvement | ICLR27 sub | 2, 4 | Persistent improver policy evolved from 768 improvement pairs across 62 environments | ◐ |
| [k2HCLpqs0U](https://openreview.net/forum?id=k2HCLpqs0U) | MARS: Mechanism-Aware Redesign Search | ICLR27 sub | 2 | Attributes failures to mechanisms; beats the best baseline on 5/5 tasks | ◐ |
| [TCM9cee8Io](https://openreview.net/forum?id=TCM9cee8Io) | ADIAS | ICLR27 sub | 2 | Issue-centric optimization with persistent issue state; +25.2% | ◐ |
| [zqOVohLt4d](https://openreview.net/forum?id=zqOVohLt4d) | Creative Design of Agentic Systems (ACA + EGMS) | ICLR27 sub | 2 | Benchmark of architectural creativity plus evidence-grounded mechanism search | ◐ |
| [cVUStYNEDI](https://openreview.net/forum?id=cVUStYNEDI) | SHIFT: Dynamic Harness Search | ICLR27 sub | 2 | Value function plus per-query MCTS; about 80% over 9,193 tasks | ◐ |
| [6YccHf9cEN](https://openreview.net/forum?id=6YccHf9cEN) | MAS via a Typed IR | ICLR27 sub | 2 | Meta-agent emits a typed IR that a compiler lowers; +9.3 | ◐ |
| [CE2m5ATPk0](https://openreview.net/forum?id=CE2m5ATPk0) | ASPO | ICLR27 sub | 2 | RL designer with competence-boundary query selection | ◐ |
| [PdSCPcPX07](https://openreview.net/forum?id=PdSCPcPX07) | FCA | ICLR27 sub | 2 | RL designer with operator-level credit | ◐ |
| [ZNLVKFHWls](https://openreview.net/forum?id=ZNLVKFHWls) | CORE-Flow | ICLR27 sub | 2 | Counterfactual edit observations reused as experience; average 89.41 | ◐ |
| [AUiAVv2xU2](https://openreview.net/forum?id=AUiAVv2xU2) | MILO | ICLR27 sub | 2 | Harness search that co-evolves its own search strategy | ◐ |
| [nZa4AgvcVj](https://openreview.net/forum?id=nZa4AgvcVj) | DiVE | ICLR27 sub | 2 | Diagnosis-driven multi-agent harness evolution | ◐ |
| [Nuny1xpe9z](https://openreview.net/forum?id=Nuny1xpe9z) | ARC: Learning to Configure Agentic AI Systems | ICLR27 sub | 2 | SMDP policy picks a configuration per query; τ-Bench airline 9.0→18.0 | ◐ |
| — | DyLAN | 2024 | 2 | Dynamic agent network; system audited in Illusion | † |
| — | AutoMaAS | 2025–26 | 2 | Strongest baseline in CMAS (reimplemented) | † |
| — | AutoDesign | 2026 | 2 | CLI meta-agent harness design (mentioned) | † |
| — | Microsoft Foundry Agent Optimizer | 2026 | 2, 3 | Industry tool listed in Duan Tab. 4 | ✓ (via Duan) |
| — | AIE-Bench | — | 2 | Searched for but not located under that name | † |

### Chapter 3 home

| id (link) | title | date | chapter(s) | one-line | mark |
|---|---|---|---|---|---|
| [2602.22480](https://arxiv.org/abs/2602.22480) | VeRO | 2026-02 | 3 | Harness for agents that optimize agents (Scale AI) | ◐ |
| [2603.25723](https://arxiv.org/abs/2603.25723) | NLAH | 2026-03 | 3 | Makes the "harness" the unit of optimization | ◐ |
| [2604.25850](https://arxiv.org/abs/2604.25850) | Agentic Harness Engineering (AHE) | 2026-04-28 | 3 | Observability pillars plus per-edit predictions; TB2 69.7→77.0; blind to regressions | ✓ |
| [2605.18747](https://arxiv.org/abs/2605.18747) | Code as Agent Harness (survey) | 2026-05 | 3 | Taxonomy from interface to mechanisms to multi-agent; regression-free improvement listed as open | ✓ (abstract) |
| [2605.24539](https://arxiv.org/abs/2605.24539) | DemoEvolve | 2026-05 | 3 | Harness evolution from demonstrations | ◐ |
| [2605.30621](https://arxiv.org/abs/2605.30621) | Harness Updating Is Not Harness Benefit | 2026-05 (v2 Nov 4) | 3, 4 | Evolver strength barely matters; mid-tier models benefit most | ✓FTV |
| [2606.09498](https://arxiv.org/abs/2606.09498) | Self-Harness: Harnesses That Improve Themselves | 2026-06-08 | 3 | Same model improves its own harness; 9/9 cells improve, but the held-out split doubles as a gate | ✓ |
| [2606.14249](https://arxiv.org/abs/2606.14249) | HarnessX: A Composable, Adaptive, and Evolvable Agent Harness Foundry | 2026-06-12 | 3 | Typed primitives with the AEGIS engine; +14.5 average, but no held-out set | ✓ |
| [2608.07545](https://arxiv.org/abs/2608.07545) / [XVvUgp3Bu8](https://openreview.net/forum?id=XVvUgp3Bu8) | DarwinX: Evolving Agent Harnesses Through Natural Selection | 2026-07-31 | 3 | Population search with preserve-and-extend; TB2.1 75.5→83.2; SWE-V 84.2 | ✓ |
| [2608.08466](https://arxiv.org/abs/2608.08466) | Hierarchical Self-Improvement (HSI) | 2026-08 | 3, 4 | Task harness / evolver / meta-evolver under a frozen anchor; BabyAI +39.3 | ✓ (abstract + §1) |
| [2608.09096](https://arxiv.org/abs/2608.09096) | Evo-Bench | 2026-08 | 3 | Benchmark of LLMs as harness optimizers | ◐ |
| [2608.15071](https://arxiv.org/abs/2608.15071) | Evo-Harness | 2026-08 | 3 | Text-artifact harness evolution | ◐ |
| [2608.19013](https://arxiv.org/abs/2608.19013) | Harness Continual Learning | 2026-08 | 3 | Continual harness evolution across benchmarks | ◐ |
| [2608.23552](https://arxiv.org/abs/2608.23552) | Prime Agent | 2026-08 | 3 | Self-improving open RLM harness | ◐ |
| [2608.25593](https://arxiv.org/abs/2608.25593) | JIT-Agent | 2026-08 | 3 | Just-in-time harness construction | ◐ |
| [2609.01481](https://arxiv.org/abs/2609.01481) | Harness-of-Harness | 2026-09 | 3 | Multi-day software development with continual improvement | ◐ |
| [2609.09646](https://arxiv.org/abs/2609.09646) | RobustSGPO | 2026-09 | 3, 4 | Three-rung permission ladder; paired-replay admission of harness edits | ✓FTV |
| [2609.14857](https://arxiv.org/abs/2609.14857) | ModularRSI | 2026-09 | 3 | Search-space (permission) control for harness RSI | † |
| [2609.24972](https://arxiv.org/abs/2609.24972) / [75PPLZgaUM](https://openreview.net/forum?id=75PPLZgaUM) | RRSI: Regularized Recursive Self-Improvement of Agent Harnesses | 2026-09-21 | 1, 3 | Edit budget, leakage critic and pruner; OOD average 39.7→43.6 | ✓ |
| [2609.33146](https://arxiv.org/abs/2609.33146) | LiteEvo | 2026-09 | 3 | Tool-free meta-agents with a frozen Qwen3.5-9B; matches HarnessX at lower cost | ◐ |
| [2609.37834](https://arxiv.org/abs/2609.37834) / [hRSpuaLKkC](https://openreview.net/forum?id=hRSpuaLKkC) | Mixture of Self-Improving Branches (MoB) | 2026-09-29 | 3, 4 | Two branches plus a router; Math-Gemini 46.0→62.0 vs Meta-Harness | ✓ |
| [2610.00917](https://arxiv.org/abs/2610.00917) | Finding the Right Fit | 2026-10 | 3 | 66 model×harness configurations; rankings reverse across harnesses | † |
| [2610.05300](https://arxiv.org/abs/2610.05300) | MESH-Harness | 2026-10 | 3 | Bandit-based harness search | † |
| [2610.11655](https://arxiv.org/abs/2610.11655) | Harness Evolution Hits a Ceiling: When Weight Training Should Begin | 2026-10 | 3 | Harness fixes process failures and weights fix content failures | ◐ (title ✓) |
| [2610.14224](https://arxiv.org/abs/2610.14224) / [s3UU3VSGcB](https://openreview.net/forum?id=s3UU3VSGcB) | Harness²: Recursive Agent Harnessing for an Open World | 2026-10-09 | 3 | Label-free two-level recursion; WorkBuddy GLM-5 64.2→75.7 | ✓ |
| [2610.15150](https://arxiv.org/abs/2610.15150) | Harmony Harness | 2026-10 | 3 | Multi-objective / cost-aware harness evolution | † |
| [2610.18813](https://arxiv.org/abs/2610.18813) | Self-evolving Agent Harness (theory/survey) | 2026-10 | 3 | Continual-PAC generalization and retention bounds | ✓ (abstract) |
| [2610.22466](https://arxiv.org/abs/2610.22466) / [PX4Zpvsaca](https://openreview.net/forum?id=PX4Zpvsaca) | Harness-Delta Attribution (HDA) | 2026-10-15 | 3 | On average, 54% of gain is overfitting and 14% is test-time scaling | ✓ |
| [2610.26770](https://arxiv.org/abs/2610.26770) | HarnessPro | 2026-10 | 3 | Label-free Pareto evolution; heavy tier +2.0 pp at 26.1% lower cost | ✓ (abstract) |
| [2610.27435](https://arxiv.org/abs/2610.27435) | RiseHarness: Harness Self-Evolution for Long-Horizon Terminal Agents | 2026-10-20 | 3 | Targets flipping tasks; GPT-6-sol +5.25 TBH; gains are model-specific | ✓ |
| [2610.28380](https://arxiv.org/abs/2610.28380) | Object-Oriented Harness | 2026-10 | 3 | Evolved subclass hierarchy plus a router; up to +15 | ◐ |
| [2610.30499](https://arxiv.org/abs/2610.30499) | Can LLMs Improve Their Own Harness? (ARC-AGI-3) | 2026-10 | 3 | 27 lineages scored on 20 sealed games | ◐ |
| [2610.32813](https://arxiv.org/abs/2610.32813) | HarnessMM | 2026-10 | 3 | Multi-objective harness evolution | † |
| [2610.38392](https://arxiv.org/abs/2610.38392) | Masked Erosion | 2026-10 | 3 | Gates silently delete redundant safeguards across 411 loops | ◐ |
| [2611.05139](https://arxiv.org/abs/2611.05139) | Meta²: Precomputing Reusable Knowledge for Online Harness Evolution | 2026-11 | 3 | Meta-skill learned offline and applied online per task | ◐ (title ✓) |
| [2611.06254](https://arxiv.org/abs/2611.06254) | HarnessFlow | 2026-11 | 3 | MCTS refinement tree; 69.48% mean, +3.95 pp over Meta-Harness | ◐ |
| [2611.07624](https://arxiv.org/abs/2611.07624) | Beneath the Surface: What We Overlook When We Talk About Agent Evolution | 2026-11 | 3 | Every evolved agent both gains and loses tasks; the top scorer is never the best parent | ✓ (abstract) |
| [2611.09218](https://arxiv.org/abs/2611.09218) | BOXBOX | 2026-11 | 3 | In-run harness revision with hot reload; Speedup@4h 1.70× vs 1.54× | ◐ |
| [2611.11342](https://arxiv.org/abs/2611.11342) | Frozen weights: value of failure traces | 2026-11 | 3 | Failure-trace notes +4.0 pp; about 60% of that comes from try-and-keep alone | ◐ |
| [2611.15006](https://arxiv.org/abs/2611.15006) | EvoX: Rethinking the Agent Harness as a Runtime Policy | 2026-11 | 3 | Harness treated as an operational policy, with a Contrast fairness framework (not the same paper as 2602.23413) | ◐ (title ✓) |
| [2611.21688](https://arxiv.org/abs/2611.21688) | DeltaHarness | 2026-11 | 3 | Latency- and resource-adaptive tool dispatch | ◐ |
| [2611.22432](https://arxiv.org/abs/2611.22432) | Think Once, Act Fast: Cost-Effective RSI for Decision Model Harness (JevRSI) | 2026-11 | 3 | Asynchronous System-2 improves the harness of a fast System-1 policy | ◐ (title ✓) |
| [MQoRwlss2y](https://openreview.net/forum?id=MQoRwlss2y) | EvoSpace | ICLR27 sub | 3 | Adaptively widens the editable scope; +8.62%/+13.71% on LoCoMo | ◐ |
| [MXINrso5QC](https://openreview.net/forum?id=MXINrso5QC) | DyScope | ICLR27 sub | 3 | Diagnosis-based dynamic edit scope | ◐ |
| [Gzda5HNz7Y](https://openreview.net/forum?id=Gzda5HNz7Y) | ReHarness: Learning Reusable Harness Evolution Across Tasks | ICLR27 sub | 3 | Transferable evolution strategy; beats Self-Harness at equal data | ◐ (title ✓) |
| [Q9AHmuAk4Y](https://openreview.net/forum?id=Q9AHmuAk4Y) | RSIBench | ICLR27 sub | 3, 4 | Agents revise their own Pi harness; Opus 5 +8.3 held-out, GPT-5.5 −8.3 | ◐ |
| [LhhLpXBFgo](https://openreview.net/forum?id=LhhLpXBFgo) | Opinionated Optimizers | ICLR27 sub | 3 | Overfitting traced to proposer priors and noise across 216 runs | ◐ |
| [yl1XFtnTn4](https://openreview.net/forum?id=yl1XFtnTn4) | Auditing Harness Evolution | ICLR27 sub | 3 | Lineages show no edge over budget-matched single-edit selection | ◐ |
| [bSBejKazyx](https://openreview.net/forum?id=bSBejKazyx) | Elicitation or Expansion? | ICLR27 sub | 3 | The harness's pass@1 advantage reverses at large k | ◐ |
| [9MEy7m0jWc](https://openreview.net/forum?id=9MEy7m0jWc) | Harness Training: A Post-Training View of Agent Harness Evolution | ICLR27 sub | 3 | Harness-SFT / Harness-RL framing; STHT | ◐ (title ✓) |
| [hI2fAYvROC](https://openreview.net/forum?id=hI2fAYvROC) | Compositional Safety Failures in Harness Evolution | ICLR27 sub | 3 | Individually safe edits compose into unsafe behavior | ◐ |
| [tNNCBiSaC9](https://openreview.net/forum?id=tNNCBiSaC9) | EvoHarnessBench | ICLR27 sub | 3 | Non-stationary harness streams; harness-induced forgetting | ◐ |
| [cF89G2mfyg](https://openreview.net/forum?id=cF89G2mfyg) | HOP | ICLR27 sub | 3 | Semiparametric configuration × task model; provable selection | ◐ |
| [C6EElQ2PRG](https://openreview.net/forum?id=C6EElQ2PRG) | Feedback Alignment | ICLR27 sub | 3 | Scaffold evolution raises the visible score while the held-out target falls | ◐ |
| [5lWjRTK6Vk](https://openreview.net/forum?id=5lWjRTK6Vk) | Harness Learns to Lie | ICLR27 sub | 3 | False success rises each round | ◐ |
| [XoMy9wRqF1](https://openreview.net/forum?id=XoMy9wRqF1) | Plug, Play, or Re-Evaluate | ICLR27 sub | 3 | 84.3% of 1,120 component removals change nothing | ◐ |
| [zn5R6mJV8i](https://openreview.net/forum?id=zn5R6mJV8i) | Evolving to Match Capabilities | ICLR27 sub | 3 | Skills that help a strong model (+31.1) hurt a weak one (−8.8) | ◐ |
| [MUAJguyvAD](https://openreview.net/forum?id=MUAJguyvAD) | Harness² (a different submission with the same name) | ICLR27 sub | 3 | Name collision with s3UU3VSGcB | † |
| — | ACE | 2025 | 3 | Playbook evolution; text-level baseline | † |
| — | TF-GRPO | 2026 | 3 | Baseline in AHE (regresses on SWE-V) | † |
| — | TTHE | 2026 | 3 | Baseline in RRSI (ends below H0 OOD) | † |
| — | ReasoningBank | 2025–26 | 3 | Baseline in Harness² | † |
| — | HarnessWeaver | 2026 | 3 | Branch/specialist harness search | ◐ |
| — | EmoHarness | 2026-11 | 3 | November harness method (named only) | † |
| — | REINS | 2026-11 | 3 | November harness method (named only) | † |
| — | Harness-Zero | 2026 | 3 | "Harness first, then distil" weight-boundary work | † |
| — | HERD | 2026 | 3 | "Harness first, then distil" weight-boundary work | † |
| — | JoMoHa | 2026 | 3 | Joint model–harness optimization | † |

### Chapter 4 home (truncated chapter)

| id (link) | title | date | chapter(s) | one-line | mark |
|---|---|---|---|---|---|
| [2604.00830](https://arxiv.org/abs/2604.00830) / [nw2tokNgb2](https://openreview.net/forum?id=nw2tokNgb2) | Meta-TTL: Meta-Learning Self-Improvement Policies | 2026-04 (v4 Sep 28) | 4 | Bi-level NL policy adopted only if BIS>0; +5–27% OOD; not self-referential | ✓ |
| [2606.04465](https://arxiv.org/abs/2606.04465) | SePO | 2026-06 | 4 | Mutation/proposal-prompt evolution | † |
| [2607.21971](https://arxiv.org/abs/2607.21971) | MetaEvolve (COLM 2026) | 2026-07 | 4 | RL teaches self-evolution meta-skills | ✓ (headline numbers) |
| [2609.33867](https://arxiv.org/abs/2609.33867) | R² Flow | 2026-09 | 4 | Improver-level workflow (timeline only) | † |
| [2609.37968](https://arxiv.org/abs/2609.37968) | SelfSearch | 2026-09 | 4 | Reward-free self-modification from episode records | ✓FTV |
| [2610.21646](https://arxiv.org/abs/2610.21646) | The Self-Improvement Problem | 2026-10 | 4 | Formal limits of gates | † |
| [2610.32026](https://arxiv.org/abs/2610.32026) | RSI-Opt | 2026-10 | 4 | Improver-level optimization (timeline only) | † |
| [2610.33046](https://arxiv.org/abs/2610.33046) | Organon | 2026-10 | 4 | Self-editing search strategy; withdrawals in 12/21 runs; +20.5% held-out over EvoX | ✓FTV |
| [2610.35434](https://arxiv.org/abs/2610.35434) | Skill∞: RSI for Agent Skill Optimization | 2026-10-26 | 4 | Meta-skill optimizes itself under a reviewed g(m,q) gate; SkillsBench 60.7 vs 55.9 | ✓ |
| [2610.36826](https://arxiv.org/abs/2610.36826) | RuHarness | 2026-10 | 4 | Editable gate/evaluator | ◐ |
| [2611.01736](https://arxiv.org/abs/2611.01736) | RSI-Env | 2026-11 | 4 | Environment where the agent invents its own improvement loop | † |
| [71wzcahKcb](https://openreview.net/forum?id=71wzcahKcb) | LRSI | ICLR27 sub | 4 | Lookahead/knockout credit for edits | ◐ |
| [V7vjimAYGi](https://openreview.net/forum?id=V7vjimAYGi) | TRACE | ICLR27 sub | 4 | Meta-credit design | ◐ |
| [Ppb1YoSRol](https://openreview.net/forum?id=Ppb1YoSRol) | Evo² | ICLR27 sub | 4 | Learned (trained) improver | ◐ |
| [IrNv2E8nJO](https://openreview.net/forum?id=IrNv2E8nJO) | MEvo | ICLR27 sub | 4 | Preserve-and-Extend gate | ◐ |
| [RTN1lPAkIm](https://openreview.net/forum?id=RTN1lPAkIm) | LSE | ICLR27 sub | 4 | Learned self-evolution improver weights | ◐ |
| [stf7CbLpUY](https://openreview.net/forum?id=stf7CbLpUY) | SkillEvolver | ICLR27 sub | 4 | Fixed meta-skill baseline; beaten by HMED and Skill∞ | ◐ |
| [XYhCP2sgxW](https://openreview.net/forum?id=XYhCP2sgxW) | Learning Meta-Skills for Harness Design | ICLR27 sub | 4 | Meta-skill text package for harness design | ◐ |
| [lyZgSaxgoN](https://openreview.net/forum?id=lyZgSaxgoN) | Hyper Algorithm Design Agent | ICLR27 sub | 4 | Agent that edits its own improver code | ◐ |
| — | RepRSI-style internal progress | 2026 | 4 | Reward-free credit from internal progress | ◐ |

**Totals:** about 230 rows, all deduplicated.
- Papers with both an arXiv and an OpenReview id are merged into one row. These are RQGM, IDM, VERSE, HMED, FreeEvolve, TRUST, ELITE, Mendel GM, Rethinking-Eval, RRSI, DarwinX, Harness², MoB, HDA and Meta-TTL.
- The two EvoX papers are distinct: 2602.23413 (meta-evolution) and 2611.15006 (runtime policy).
- Chapter 4 was cut off mid-text, so any papers it lists after the Promptbreeder card are missing from this index.

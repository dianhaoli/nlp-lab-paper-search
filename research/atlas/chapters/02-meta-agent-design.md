## Meta-agents & automated agent/workflow design

*Scope:* this chapter covers systems in which one LLM program, the **meta-agent** (designer, optimizer, orchestrator), produces or revises another agentic system: its prompts, modules, workflow graph, multi-agent topology or harness code. The chapter stops where the meta-agent edits *itself*, which belongs to the self-referential chapter, and touches that line only through Hyperagents, RSIFlow and MetaEvoForge.

*Marks:* ✓ = checked in the full text this session (fact-check pass of 2026-11-18/19), with the section or table cited. ✓ᵖ = checked in full text in an earlier project round (FULLTEXT_VERIFICATION.md / round2 factcheck.md), not re-read here. ◐ = abstract only. † = unverified or from memory.

*Surveys used:*
- **Duan et al. 2609.11873.** Puts ADAS, AFlow and AgentSquare under **L2 "strategy autonomy → agent/harness search"**: the AI chooses *how* to improve, while the objective and acceptance rule stay external. Its Table 4 lists the externally fixed limitation as "Fixed benchmark" for ADAS and AgentSquare and "Fixed evaluator" for AFlow (✓ §3.3.2 "Agent and Harness Search", Tab. 4).
- **Ren/Zhuge/Schmidhuber 2607.13104.** Files ADAS, GPTSwarm, DGM, Gödel Agent and STOP under **§6.4 "full scaffolding" improvement**, i.e. action-level self-reference that edits Σ with θ fixed (✓ Fig. 3 tree, §6.4 text).
- **Chen et al. 2607.07663.** §3.5 "harness and agent self-evolution", where the changes accumulate indefinitely (†, not re-read).
- **Dai/Ruan 2611 survey.** In its *Harness* component, meta-agent search is pattern (ii) "persistent experience". It becomes pattern (iii) "improver update" only when the designer itself is revised (†, not re-read).

**AIE-Bench could not be located** under that name on Semantic Scholar, ICLR-2027 OpenReview or alphaXiv. The agent-design benchmarks I did find are listed below.

---

### At a glance

- **What this corner is.** Search or learning over the *design space of agentic systems*. A meta-agent proposes candidate agents or workflows, an evaluator scores them on a validation split, and the winner (or a per-query policy) is deployed.
  - It started with code-space search with a frozen meta-agent: ADAS, AFlow, AgentSquare.
  - It then split into **query-level designers** trained by SFT, DPO or RL (MaAS, ScoreFlow, FlowReasoner, MAS-GPT, MetaAgent-X, SHIFT)…
  - …and **stateful / self-evolving meta-agents** (Hyperagents, RSIFlow, MetaEvoForge, CMAS).
- **State of the art (in-paper numbers).** On the shared "AFlow suite" (GSM8K / MATH / HumanEval / MBPP / HotpotQA / DROP with GPT-4o-mini executors), reported averages climbed in about a year:
  - AFlow 80.3 (✓ AFlow Tab. 1)
  - MaAS 83.59 (✓ MaAS Tab. 1, different subset: GSM8K/MATH/MultiArith/HumanEval/MBPP)
  - ScoreFlow 85.3 (✓ ScoreFlow Tab. 1; its own AFlow re-run scores 80.6)
  - CORE-Flow 89.41 (◐)

  These numbers are **not comparable across papers**: subsets, splits and judges all differ.
- **The newest strong claims are query-level and learned:**
  - SHIFT (ICLR27 sub) reports ~80% mean accuracy over 9,193 tasks, +7.2 pp over 17 baselines (◐).
  - MetaAgent-X trains designer and executor jointly with RL: +11.17 avg over the single agent on Qwen3-8B (✓ §4.2, Tab. 1).
- **Consensus forming in 2026: in-paper gains are fragile.**
  - "CoT-SC consistently outperforms automated MAS frameworks, frequently achieving higher accuracy at less than 10% of the computational cost" (Illusion of Multi-Agent Advantage, ✓ §3).
  - With 3 noisy evaluations, every automated scaffold search (ADAS, ShinkaEvolve, and even IID random sampling) selects scaffolds worse than hand-designed majority vote on test (Simple Baselines, ✓ §5, Fig. 3).
  - Frontier coding agents acting as meta-agents beat the human-scaffold average in only 5/39 configurations (Meta-Agent Challenge, ✓ §5.2 Finding 1).
- **Reproductions in other papers put ADAS at or below plain IO/CoT:** 67.2 vs IO 72.8 in AFlow (✓ Tab. 1); 75.13 vs vanilla 77.50 in MaAS (✓ Tab. 1); 76.6 vs IO 76.0 and CoT-SC 77.3 in ScoreFlow (✓ Tab. 1); −6.81 vs single agent on Qwen3-8B in MetaAgent-X, using the official best-searched math agent (✓ Tab. 1, §4.1). The free-code design space is expressive but hard to search with weak executors and small validation sets.
- **Design representation matters as much as the search algorithm.**
  - In AgentSquare's own table, a *random* module combination beats every hand-designed agent on 4/6 tasks (✓ Tab. 1).
  - Mining 28,568 AFlow-style workflows finds prompt specificity / placeholder routing gains (Δ +0.118) "comparable to communication-structure selection"; SharedMemory vs debate is Δ +0.135 (✓ abstract, §I).
- **Biggest open problem: the meta-level evaluation is the weak link.** Validation sets are small (ADAS ARC: 20 val / 60 test questions ✓; Simple Baselines' AIME scaffold search validates on 30 AIME-2024 questions × 3 evaluations ✓), so selection is dominated by noise (selection–validation gap 1.52 pp ◐). Baselines are rarely compute-matched. The meta-agent is rewarded purely on order-0 task score. There is still no evidence that a meta-agent gets *better at designing* over time, as opposed to producing one good design.

---

### How it developed

| Month | Paper | What changed |
|---|---|---|
| 2023-10 | STOP 2310.02304 | A scaffold that improves the improver (self-taught optimizer). The conceptual root of "meta-agent edits code" (✓ᵖ). |
| 2024-02 | GPTSwarm 2402.16823 (ICML'24†) | Agents as optimizable computational graphs. Node (prompt) and edge optimization, the edges via REINFORCE (◐). |
| 2024-06 | EvoAgent 2406.14228 | Evolutionary operators extend one specialist agent into a multi-agent population (◐). |
| 2024-08 | **ADAS / Meta Agent Search** 2408.08435 (ICLR'25) | Names the field. Agents defined as Python `forward()` code. A GPT-4 meta-agent programs new agents conditioned on an archive. Claims cross-domain and cross-model transfer (✓). |
| 2024-10 | **AgentSquare** 2410.06153; **AFlow** 2410.10762 (ICLR'25); Gödel Agent 2410.04444 | Restricted design spaces: 4 modules × evolution/recombination with an in-context surrogate, or operator graphs + MCTS. AFlow becomes the de facto benchmark suite. Gödel Agent makes the designer self-modifying (✓/✓/✓ᵖ). |
| 2025-02 | **MaAS** 2502.04180; **ScoreFlow** 2502.04306 | From one-design-per-task to a *distribution* (agentic supernet, query-dependent sampling) and to gradient-trained generators (Score-DPO on an 8B model) (✓). |
| 2025-03/04 | MAS-GPT 2503.03686; **FlowReasoner** 2504.15257; W4S 2504.04785 | The meta-agent becomes a *trained model*: SFT on query→MAS pairs; R1-distill + RL with execution reward; a weak meta-agent trained by RL to drive strong executors (◐/✓/◐). |
| 2025-05 | MAS-Zero 2505.14996; DGM 2505.22954 | Inference-time per-instance MAS design with no validation set. Darwin-style open-ended archive of self-editing coding agents (◐/✓ᵖ). |
| 2025-07/08 | MetaAgent (FSM) 2507.22606; ARG-Designer 2507.18224; AdaptFlow 2508.08053 | FSM-structured MAS, autoregressive topology generation, MAML-style workflow initialization (◐). |
| 2026-01 | MAS-Orchestra 2601.14652 | RL orchestration as function calling, plus controlled benchmarks asking whether MAS beats a single agent (◐). |
| 2026-02 | **Simple Baselines are Competitive with Code Evolution** 2602.16805 | IID random sampling matches ADAS and ShinkaEvolve on scaffold design; all three lose to a hand-designed majority-vote scaffold on test. Val→test drops above 10% for every search method (✓ §5). |
| 2026-03 | Hyperagents 2603.19461; Meta-Harness 2603.28052 | Task agent and meta-agent in one editable repo; a fixed-meta (ADAS-like) variant scores ≈0. Harness search with raw-trace access (✓ᵖ). |
| 2026-05 | **MetaAgent-X** 2605.14212 | End-to-end RL of designer *and* executor. Names the "frozen-executor ceiling" (✓ §4.2–4.3). |
| 2026-06 | **Meta-Agent Challenge** 2606.04455; **Illusion of MA Advantage** 2606.13003 | Benchmark and audit era. Frontier CLI agents as meta-agents rarely beat human scaffolds. Automated MAS lose to CoT-SC at matched cost (✓/✓). |
| 2026-07/08 | Rethinking Harness-Evolution Eval 2607.12227; Metaⁿ 2608.24735; HarnessOpt-Bench 2608.06301 | The harness-evolution branch absorbs ADAS ideas, with overfitting and test-time-scaling baselines (✓ᵖ/ctx). |
| 2026-10 | RSIFlow 2610.21791; **CMAS** 2610.31436; **Mining the Design Space** 2610.31709; AMOD 2610.27531; DAGO 2610.05284; EMMA 2610.35966 | Evolving meta-agents; meta-agent search under non-stationarity with calibrated promotion; mining discarded search trajectories for design principles; "organization design" framing (✓/✓/✓/✓ abstract/◐/◐). |
| 2026-11 (1–18) | WorkflowArena 2611.12856; ASpec 2611.01870; OpenMAS-GApp 2611.20997; MULTILOOP 2611.15754; Critic Can Hurt 2611.19037 | The production coding agent (Claude Code, Codex) *is* the meta-agent, and its orchestration is benchmarked. Stateful specialists. A standardized MAS-construction benchmark (◐; WorkflowArena abstract ✓). |
| ICLR 2027 subs (Oct–Nov) | MARS, ADIAS, ACA/EGMS, SHIFT, typed-IR MAS, ASPO, FCA, CORE-Flow, MILO, DiVE, ARC, MetaEvoForge, *One Search Operation* | Mechanism-level failure attribution; per-query MCTS with learned value functions; RL credit assignment at operator level; a per-decision evaluation protocol for design search (all ◐). |

---

### Taxonomy

Three cross-cutting axes organize the corner:
- **(1) Design representation:** what can be edited.
- **(2) Design granularity:** how often a new design is produced.
- **(3) How the designer improves:** frozen LLM search, trained designer, or self-evolving designer.

A fourth line is the meta-evaluation layer: benchmarks and audits.

| Axis / approach | Mechanism | Representative papers |
|---|---|---|
| **1a. Fixed-slot / module menu** | Choose among library modules (Planning, Reasoning, Tool, Memory) with uniform I/O | AgentSquare 2410.06153; AgentSwift 2506.06017; ARC (Nuny1xpe9z) |
| **1b. Operator graph / DSL** | Nodes = predefined operators (Generate, Review & Revise, Ensemble, Programmer…); edges in code | AFlow 2410.10762; GPTSwarm 2402.16823; MaAS 2502.04180; ScoreFlow; FlowMAS 2609.37151; DAGO 2610.05284; MoFlow 2609.38294; BEO 2610.29276 |
| **1c. Typed IR / FSM / NL design document** | The meta-agent emits a checkable intermediate form that a compiler lowers | MetaAgent-FSM 2507.22606; typed-IR MAS (6YccHf9cEN); ABSTRAL 2603.22791; Meta-Agent 2605.25233 (DAG + contracts) |
| **1d. Free code (agent or harness)** | The meta-agent writes the whole `forward()` or harness | ADAS 2408.08435; Meta-Harness 2603.28052; MARS (k2HCLpqs0U); MILO (AUiAVv2xU2); MAC 2606.04455 (CLI agent writes an artifact) |
| **2a. Task-level (one design per task)** | Search on a val split, deploy the argmax | ADAS, AFlow, AgentSquare, EvoAgent, Mining-the-Design-Space |
| **2b. Query-level (one design per query)** | A sampler or policy maps query → MAS | MaAS, FlowReasoner, ScoreFlow, MAS-GPT, MAS-Zero, MAS-Orchestra, MetaAgent-X, SHIFT, ASPO, FCA, OOPMAS 2610.07787, RA-MoWE 2610.07851, GRAFT 2608.02353 (region-level) |
| **2c. Stateful / lifelong** | The design persists and is revised with experience or under shift | EMAS 2608.07196; Inherit-MAS 2610.02396; ASpec 2611.01870; MAS-Loop 2610.38946; CMAS 2610.31436; EMMA 2610.35966 |
| **3a. Frozen LLM + search algorithm** | Archive prompting, MCTS, evolution, bandits, GP-BO, GFlowNets | ADAS (archive), AFlow/SHIFT/MoFlow (MCTS), EvoAgent/AgentSquare/DAGO (evolution), BEO (Bayesian), FlowMAS (GFlowNet), CORE-Flow (counterfactual retrieval) |
| **3b. Trained designer, frozen executor** | SFT / DPO / RL on downstream execution reward | MAS-GPT (SFT), ScoreFlow (Score-DPO), FlowReasoner (SFT+RL), W4S (RLAO), MAS-Orchestra, ASPO, FCA, SHIFT (value fn) |
| **3c. Trained designer + executor** | Joint RL | MetaAgent-X 2605.14212 |
| **3d. Self-evolving designer** | The designer's own procedure, skills or code are revised | Hyperagents 2603.19461; RSIFlow 2610.21791; MetaEvoForge (0b4wysHrzh); MILO; DGM 2505.22954; Gödel Agent |
| **4. Meta-evaluation (benchmarks / audits)** | Measure meta-agents or the claims made for them | MAC 2606.04455; WorkflowArena 2611.12856; ACA (zqOVohLt4d); OpenMAS-GApp 2611.20997; ST-Bench 2610.07763; AI4AI-Bench 2608.20318; Illusion 2606.13003; Simple Baselines 2602.16805; One Search Operation (h2I3OGkGtU); Critic Can Hurt 2611.19037 |

How this maps to the project's openness levels (round3/REPORT.md):
- 1a ≈ O0–O1
- 1b–1c ≈ O2
- 1d ≈ O3
- 3d ≈ O4

Only 3d reaches the territory of Duan's L3 (procedure autonomy). Everything else is Duan's L2.

---

### Paper cards

**1. Automated Design of Agentic Systems (ADAS / Meta Agent Search)** ([2408.08435](https://arxiv.org/abs/2408.08435), Aug 2024, ICLR 2025 ✓ header)
- *What it does.* Defines agents as Python `forward()` functions. A fixed GPT-4 meta-agent iteratively writes new agents conditioned on an archive of previous agents and their scores (open-endedness "stepping stones").
- *Setup.*
  - Meta-agent GPT-4; evaluated agents GPT-3.5 (✓ §4.1).
  - ARC (≤5×5 grids): **20 val / 60 test questions, each evaluated 5 times, 25 iterations** (✓ §4.1).
  - DROP, MGSM, MMLU, GPQA: 30 iterations, searched independently per domain (✓ §4.2). Val/test sizes 128/800, except GPQA 32/166 (✓ App.).
  - Archive initialized with the hand-designed baseline agents (✓ §3, Alg. 1; Tab. 1 lists 7 such baselines).
  - Cost **≈$500 per ARC run, ≈$300 per reasoning-domain run** (✓ App. J).
- *Key results.*
  - Test (Tab. 1, ✓): DROP F1 **79.4 vs 65.8** (best hand-designed, Role Assignment) and 69.1 (Prompt Optimization/OPRO); MGSM **53.4 vs 39.0** (LLM-Debate).
  - MMLU 69.6 vs 67.6 (OPRO) and GPQA 34.6 vs 32.9: **CIs overlap (±3.2)**.
  - Transfer MGSM→GSM8K **69.5 vs 43.6** (LLM-Debate) (✓ Tab. 2). ARC agent transferred to Claude-Sonnet **48.3 vs 39.3** (Self-Refine) (✓ Tab. 3).
  - Cross-domain transfer is mixed rather than uniformly positive. The top MGSM agent scores **62.4 on MMLU, below CoT-SC 65.9**, but the 2nd-ranked agent scores 67.0 and all three top MGSM agents beat every baseline on DROP (70.4–71.9 vs 65.8) (✓ Tab. 2).
- *Limitations / critiques.*
  - Single-step QA only, small val sets, single objective. Future work explicitly lists "higher-order ADAS", where the meta-agent itself is improved (✓ §6).
  - Reproductions are poor: 67.2 avg vs IO 72.8 under AFlow's protocol (✓ AFlow Tab. 1); 75.13 vs vanilla 77.50 in MaAS (✓); 76.6 vs IO 76.0 / CoT-SC 77.3 in ScoreFlow (✓); −6.81 with Qwen3-8B executors (✓ MetaAgent-X Tab. 1).
  - On GPT-4.1-nano AIME scaffold design, ADAS picks scaffolds worse than majority vote on test (✓ Simple Baselines §5, Fig. 3).
  - Inside the DGM-H framework, the fixed-meta "ADAS-like" variant scores 0.0 on paper review (✓ᵖ Hyperagents).
  - **Duan (✓ Tab. 4):** "Fixed benchmark" limitation, i.e. L2 not L3.

**2. AFlow: Automating Agentic Workflow Generation** ([2410.10762](https://arxiv.org/abs/2410.10762), Oct 2024, ICLR 2025 ✓)
- *What it does.* Workflows are code graphs over a fixed operator set (Generate, Format, Review & Revise, Ensemble, Test, Programmer, Custom). An LLM optimizer modifies them inside an MCTS with tree-structured experience and early stopping.
- *Setup.*
  - Optimizer Claude-3.5-Sonnet. Executors GPT-4o-mini / DeepSeek-V2.5 / Claude-3.5-Sonnet / GPT-4o.
  - **20 rounds** (ADAS baseline re-run with 30 rounds, Claude-3.5-Sonnet optimizer, GPT-4o-mini executor). Val:test = 1:4. MATH = 617 level-5 problems. Test results averaged over 3 runs (✓ §5.1, Tab. 1 caption).
- *Key results.*
  - Avg **80.3 vs CoT-SC 76.0, MedPrompt 75.3, IO 72.8, ADAS 67.2** (✓ Tab. 1). That is +4.3 points over CoT-SC, which the paper reports as "5.7%" (relative).
  - The workflow found with DeepSeek-V2.5 is notably weaker on GPT-4o-mini than GPT-4o-mini's own workflow, so **designs are executor-specific** (✓ §5.2).
  - "Smaller models beat GPT-4o at 4.55% of its cost" (✓ abstract; App.: "parity with GPT-4o IO at 4.55% of the cost").
- *Limitations / critiques.*
  - The search space is bounded by the operators.
  - Its six-benchmark suite became the field's default, which encourages saturation; GSM8K and HumanEval are near ceiling (IO 92.7 / 87.0, ✓ Tab. 1).
  - Mining 2610.31709 notes AFlow-style optimizers "discard over 95% of evaluated architectures" (✓ abstract).
  - Illusion 2606.13003 finds single-agent GPT-5 CoT-SC beats GPT-4o-based ADAS/AFlow with less than half the total tokens (✓ §3).
  - Training cost $22.50 vs MaAS $3.38 on MATH (✓ MaAS Tab. 3).

**3. The Illusion of Multi-Agent Advantage** ([2606.13003](https://arxiv.org/abs/2606.13003), Jun 2026, preprint; Salesforce Research / HKUST (Guangzhou) / UBC / NTU, which overlaps with the MAS-Zero / MAS-Orchestra authors)
- *What it does.* A systematic audit of automatic MAS against CoT-SC, plus a synthetic diagnostic task (SMFR) built to *favor* decomposition, context separation and parallelism, with a hand-built Expert-MAS.
- *Setup.*
  - Systems: ADAS, AFlow, MaAS, MAS-Zero, MAS-Orchestra, DyLAN.
  - Backbones: GPT-4o, GPT-5, GPT-OSS-120B, Gemini-2.5-Pro (Gemini single run due to cost).
  - Benchmarks: GPQA-Diamond, HLE-Math, SWE-Bench Lite, BrowseComp-Plus (✓ §3).
- *Key results.*
  - CoT-SC "consistently outperforms automated MAS frameworks, frequently achieving higher accuracy at less than 10% of the computational cost" (✓ §3).
  - Significant MAS uplift appears only on HLE-Math with GPT-5 or Gemini-2.5-Pro, which the authors call a "competency floor" (✓ §3).
  - On SMFR, GPT-5 CoT-SC scores 57.0 (GPT-4o / GPT-OSS 22.1–26.1). Only 3 statistically significant automated wins, all at extra cost: DyLAN/GPT-OSS +6.6 pp at 5× cost; DyLAN/GPT-5 +4.3 at 2.5×; MAS-Orchestra/GPT-5 +6.0 at 1.9× (✓ §3).
  - **Expert-MAS: GPT-5 57.0→96.5, GPT-OSS 26.1→36.1**, at cost comparable to CoT-SC; GPT-4o is the stated exception (✓ §3, SMFR results).
  - Deconstruction shows "architectural bloat" (redundant agents, functional collapse) (✓ §4).
- *Limitations.* SMFR is synthetic and author-designed. Few runs on expensive backbones. The audit targets query-level and task-level MAS designers, not harness evolution on long-horizon agents.

**4. The Meta-Agent Challenge (MAC)** ([2606.04455](https://arxiv.org/abs/2606.04455), Jun 2026, preprint; Institute of Software CAS + Ant Group)
- *What it does.* A benchmark in which a CLI coding agent (the meta-agent) gets a sandbox, an evaluation API and a time budget to program an agent artifact. The artifact is scored on a hidden test set. Layered anti-reward-hacking defenses: dual containers, split-level authorization, proxy enforcement.
- *Setup.*
  - Five domains: AIME, GPQA/HLE, LiveCodeBench, SWE-Bench, Terminal-Bench.
  - **12 h budget for AIME/GPQA/LiveCodeBench (24 h for SWE/TB)** (✓ §4).
  - Artifacts call Qwen3-8B on one A100 (reasoning domains) or Claude Haiku 4.5 (SWE/TB).
  - Meta-agents: Claude Code (Opus 4.7, Opus 4.6, Sonnet 4.6), Gemini-CLI (3.1 Pro), Codex (gpt-5.3-codex, gpt-5.4), plus open models through Claude Code. Mean ± SD over three runs.
  - Baselines: a naive agent, and Terminus-2 / OpenHands as human scaffolds (✓ §4).
- *Key results.*
  - **Only 5/39 configurations beat the human baseline average.** 4 of the 5 are Claude Sonnet/Opus; DeepSeek-v4-Pro is the only open model to do so. No meta-agent fully surpasses the baseline on GPQA or SWE-Bench (✓ §5.2 Finding 1).
  - **33% of configurations have SD > 0.1, vs a maximum of 0.053 for human baselines** (✓ Finding 2).
  - **5 flagged reward-hacking trials**, including label exfiltration by GPT-5.3-Codex via error tracebacks; all were neutralized (✓ Finding 3, App. B.3.1).
  - Runtime and mean inter-eval-call interval predict success. The number of eval calls does not (✓ §5.3).
- *Limitations.* Expensive (12–24 h per run). It inherits contamination and narrowness from the reused benchmarks (✓ Limitations). Meta-agents are scored at order 0 only; there is no repeated-design or learning-to-design measure.

**5. Simple Baselines are Competitive with Code Evolution** ([2602.16805](https://arxiv.org/abs/2602.16805), Feb 2026, v2 Oct 2026; Oxford / Sakana AI & ITU Copenhagen)
- *What it does.* Compares ADAS and ShinkaEvolve against IID random sampling (IID RS) and sequential sampling across math bounds, scaffold design, heuristics and ML competitions.
- *Setup (scaffold part).* AIME 2024 for search and AIME 2025 for test. GPT-4.1-nano executor. ADAS 30 generations (up to 90 evals with debug re-evaluation); Shinka 75 generations (~60 yield an evaluated scaffold); IID RS capped at 50 scaffolds. The authors estimate an ADAS-style run (~50 scaffolds) cost Hu et al. about $300 (✓ §5).
- *Key results.*
  - **All code evolution methods, IID RS included, show >10% val→test drops, vs ~5% for majority vote.** Re-evaluated 10×, their true val accuracy is lower than reported. **P(Shinka > IID RS) = 0.49** (✓ §5, Fig. 3).
  - A standard deviation of 0.5% would need each question evaluated >60 times (>20× the default) (✓ §5.1).
  - Recommends **an effective test set of at least 300 questions with 95% CIs** and an evaluation cascade (✓ §5.1).
- *Critique value.* This is the cleanest demonstration that ADAS-style selection on small validation sets (here 30 questions × 3 evaluations) is mostly noise-chasing.

**6. MaAS: Multi-agent Architecture Search via Agentic Supernet** ([2502.04180](https://arxiv.org/abs/2502.04180), Feb 2025, ICML 2025†)
- *What it does.* Optimizes a probabilistic, layered "agentic supernet" over operators. A controller samples a query-dependent MAS and can early-exit for easy queries.
- *Setup.* GPT-4o-mini executor. GSM8K, MATH, MultiArith, HumanEval, MBPP, GAIA. 14 baselines including ADAS, AFlow, AgentSquare and GPTSwarm (✓ Tab. 1, §4.1).
- *Key results.*
  - Avg **83.59 vs AFlow 82.25, AgentSquare 80.29, CoT-SC 78.85, vanilla 77.50, ADAS 75.13** (✓ Tab. 1).
  - MATH training cost **$3.38 vs AFlow $22.50** (✓ Tab. 3).
  - GAIA Level 1/2 improvements of 18.38% / 17.61% (✓ §4.2).
  - Transfer to Qwen-2.5-70b (as written in the paper) +4.98 to 5.50% (✓ §4.4 text).
- *Limitations.* Gains over AFlow are about 1.3 points on saturated benchmarks with no CIs. Illusion 2606.13003 includes MaAS among the systems that lose to CoT-SC at matched cost (✓).

**7. AgentSquare: Automatic LLM Agent Search in Modular Design Space** ([2410.06153](https://arxiv.org/abs/2410.06153), Oct 2024, ICLR 2025 ◐ via S2; arXiv header says "Preprint" ✓)
- *What it does.* Defines the MoLAS problem: four modules (Planning, Reasoning, Tool Use, Memory) with uniform I/O. Search proceeds by module *evolution* (LLM writes new modules) and *recombination*, with an in-context surrogate that skips bad candidates.
- *Setup.* GPT-4o and GPT-3.5. WebShop, ALFWorld, SciWorld, M3Tool, TravelPlanner, PDDL. Evaluating a simple CoT agent with GPT-4o on ALFWorld costs about $60 (✓ §3.4).
- *Key results (GPT-4o, ✓ Tab. 1).*
  - AgentSquare 0.607 / 0.695 / 0.781 / 0.524 / 0.583 / 0.669.
  - ADAS 0.521 / 0.543 / 0.754 / 0.475 / 0.373 / 0.568.
  - **Random module combination 0.533 / 0.620 / 0.704 / 0.438 / 0.563 / 0.660.**
  - Claims +17.2% average over the best human design (✓ abstract).
- *Critique.* Random recombination in this design space already beats every hand-crafted agent on WebShop, ALFWorld, TravelPlanner and PDDL (✓ Tab. 1). The *space* carries much of the gain, consistent with NAS-era lessons and with Simple Baselines. MaAS notes AgentSquare targets single-agent design (✓ MaAS §2).

**8. Hyperagents (DGM-H)** ([2603.19461](https://arxiv.org/abs/2603.19461), Mar 2026, v2 Nov 2026)
- *What it does.* Puts the task agent and the meta-agent in one editable repo inside a DGM-style archive. This is the bridge from ADAS (fixed meta-agent) to self-referential design.
- *Key results (✓ᵖ).*
  - Fixed-meta (ADAS-like) ablation: **0.0 on paper review, 0.213 on robotics**.
  - Transfer imp@50 **0.630 vs 0.0**. Meta-only 0.620 vs task-only 0.100 (n=5, Wilcoxon).
  - Compounding 0.640 vs 0.610, not significant.
- *Limitations.*
  - The gate is "valid → archived"; meta edits only change parent-sampling weight. Meta edits are not separately credited. Not compute-matched (✓ᵖ).
  - Improvement by Inheritance (ICLR27 sub ZMmJDeDTXk) traces the gain to a one-time evidence-routing edit, not to recursive improvement (round3 REPORT).
  - SelfImprove-Bench 2611.02797: 0% retained improver ("Meta") edits in its main runs and 20–45% under the HyperDSH framing that explicitly invites them; nothing is forced, and only retained counts are reported (✓ᵖ FULLTEXT_VERIFICATION, correcting the earlier "0% unless forced").

**9. FlowReasoner: Reinforcing Query-Level Meta-Agents** ([2504.15257](https://arxiv.org/abs/2504.15257), Apr 2025, preprint; Sea AI Lab et al.)
- *What it does.* Trains a *reasoning* meta-agent that writes one MAS per query. It distills DeepSeek-R1 traces into DeepSeek-R1-Distill-Qwen-7B/14B, then applies RL with execution feedback and a reward over performance, complexity and efficiency.
- *Setup.* Code only: BigCodeBench, HumanEval, MBPP. Workers are o1-mini (✓ §4 Implementation Details).
- *Key results (✓ Tab. 1).*
  - Overall **81.89 (14B) vs o1-mini 71.37 (+10.52), MaAS 76.81, AFlow 75.63, ADAS 65.48**.
  - MBPP 92.15 vs MaAS 84.16.
- *Limitations.* Code domains only, where execution reward is cheap and exact. The designer is trained per domain. The MBPP jump is large relative to other columns and has not been replicated. This is a pure order-0 reward on the designer.

**10. ScoreFlow** ([2502.04306](https://arxiv.org/abs/2502.04306), Feb 2025, preprint)
- *What it does.* A generator LLM (Llama-3.1-8B-Instruct, LoRA) proposes k=8 workflows per problem. They are executed by GPT-4o-mini and trained with **Score-DPO**, which weights preference pairs by score gaps. Three iterations on 2×A6000 (✓ §4.1).
- *Key results.* Avg **85.3 vs 80.6 for its AFlow re-run (+4.7 points)** on the AFlow six-benchmark suite; the paper's headline "8.2% improvement over existing baselines" is not the gap to the best baseline. ADAS and AFlow, both using GPT-4o-mini as workflow generator, underperform the 8B generator (✓ §4.2, Tab. 1).
- *Limitations.* GPT-4o-mini is the judge for MATH, DROP and HotpotQA (✓ §4.1). Saturated benchmarks. Same order-0 reward critique as FlowReasoner.

**11. MetaAgent-X** ([2605.14212](https://arxiv.org/abs/2605.14212), May 2026, preprint; Oregon State / UCSD / Amazon AGI / Penn State / AG2AI)
- *What it does.* Joint RL of a designer that writes MAS scripts and the executor, both Qwen3 (stagewise: only the active role is updated per stage, K=30). Uses hierarchical credit assignment via tree-structured "Executor-Designer" rollouts and **stagewise co-evolution** (✓ §3.2–3.3, §4.1).
- *Key results (✓ §4.2, Tabs. 1–2).*
  - Average **38.33 (8B) / 34.18 (4B), +11.17 / +12.80 over the single agent** on AIME24/25, OlympiadBench, APPS, LiveCodeBench-v6 and CodeContests.
  - Search-based baselines collapse with small executors: **ADAS −6.81 to 20.35 (8B); AFlow 22.52 (4B)**. Note: in Tab. 1 the AFM-Coder row reports the identical average 20.35 (−6.81), a possible table error.
  - Ablation: coupled training "improves quickly at first but later collapses". Designer-only gives limited gain. Executor-only "soon saturates, indicating a ceiling imposed by the fixed design policy" (✓ §4.3).
- *Limitations.* Math and code only. Whether the gain is MAS structure or just more RL on the executor is not fully separated; the single-agent GRPO baseline (29.10 on 8B) is the right control and is included (✓ Tab. 1).

**12. RSIFlow: Adaptive Self-Improvement Workflows with an Evolving Meta-Agent** ([2610.21791](https://arxiv.org/abs/2610.21791), Oct 2026, v2 Nov; Beihang / BAAI)
- *What it does.* The meta-agent diagnoses the shifting bottleneck (model, harness or artifacts), chooses a workflow, and evolves its own meta-harness (skills and principles with ±evidence counts).
- *Key results (✓ Tab. 1, §4.4).*
  - Overall 51.21 (4B) / 59.95 (8B) vs best baseline SIA 45.15 / 56.04.
  - Meta-backbone sensitivity on HumanEval+ over five rounds: **GPT-5.6-Sol +24.39, Claude-Sonnet-5 +11.59, DeepSeek-V4.1-Flash +4.88**.
  - Same-start **Meta4 vs Meta0: +20.56 Code, +9.51 SearchQA (single run, non-monotone)**.
  - Meta-evolution off: −5.74.
- *Limitations.* Meta-harness updates are **ungated and applied every round** (✓ᵖ FULLTEXT_VERIFICATION). Single runs. "Equal per-task budgets do not imply equal total evolution cost."

**13. Conservative Meta-Agent Search (CMAS) + MetaInvest-Bench** ([2610.31436](https://arxiv.org/abs/2610.31436), Oct 2026, preprint, single author)
- *What it does.* Treats meta-agent architecture search as **sequential incumbent replacement** with switching costs and hard risk constraints.
  - Typed graph edits, scored by paired shadow replay under common random numbers.
  - Interventional component credit.
  - Promotion only above a calibrated, variance-penalized margin, followed by a simulated canary stage and pre-specified rollback.
  - The meta-agent is the whole outer loop, with the compiler, gate and rollback rules deterministic and outside the LLM-editable boundary (✓ abstract, §1–3, Fig. 2).
- *Key results (✓ abstract, §6–7).*
  - 96 regime-switching financial streams (12 seeds × 8 schedules) in MetaInvest-Bench, a simulator where the surrogate-optimal architecture is computable. Dynamic-oracle regret **0.116→0.104**; held-out utility **0.44→0.48** vs the strongest adaptive common-budget baseline, a MaAS/AutoMaAS-style reimplementation. Other baselines: self-consistency single agent, fixed full committee, rule-based regime router, LinUCB bandit, AFlow-style search, GEPA-style prompt optimization (all reimplementations).
  - Contracts to near-single-agent teams when extra workers add no value.
  - Deployment inference cost about 6% lower, but **search-inclusive token cost is the highest**.
  - **False promotion 3.4% vs nominal 5% (4.8% in a sealed rerun).**
- *Limitations.* An author-designed simulator; the calibration is empirical and gives no sequence-level error control (stated). The proposer, gate and evaluator are locked, so there is no editable-improver arm (round2 ACCEPTANCE_RANKING).

**14. Mining the Design Space of Multi-Agent Workflows** ([2610.31709](https://arxiv.org/abs/2610.31709), Oct 2026; Dartmouth / AI2)
- *What it does.* Reruns AFlow's MCTS (N=20 rounds × K=5 runs, GPT-5.4 optimizer at temperature 0; GPT-5.4-mini/nano workers; at most 20 LLM calls per execution) over 6 composable operators on 26 deterministically verified Enigmata puzzle tasks. It **keeps every intermediate workflow (28,568 triplets)**. An AutoResearch loop then mines L0-penalized features and tests them on held-out searches (✓ abstract, §I, §III–IV).
- *Key results (✓ abstract).*
  - Star-first (independent solvers plus one-shot aggregation) **Δ = −0.045**.
  - Persistent SharedMemory vs debate **Δ = +0.135**.
  - Prompt specificity / placeholder routing **Δ = +0.118**, "comparable to communication-structure selection".
  - 300 feature-mining trials; 70 contrasts tested held-out.
- *Limitations.* Puzzle domain with deterministic verifiers. One optimizer family. Effects are correlational within AFlow's sampling distribution.

**15. What Is One Search Operation Worth? Evaluating Its Downstream Value in Automatic Agent Design** (ICLR 2027 submission [h2I3OGkGtU](https://openreview.net/forum?id=h2I3OGkGtU), ◐)
- *What it does.* From the same recorded search state, executes the original operation or a specified alternative, then resumes optimization under the same budget. Applied to GPTSwarm, AFlow, MaAS and EvoMAS on six benchmarks.
- *Key results (◐).* In a GPTSwarm–MMLU study (480 executions at 20 states):
  - Selected operations show an apparent **+2.17 pp** advantage.
  - Re-estimated on fresh repetitions at **0.65 [−0.56, 1.82]**.
  - Paired selection–validation gap **1.52 [0.41, 2.70]**.
- *Why it matters.* This is, as far as this atlas found, the first per-decision, same-state counterfactual protocol inside design search (◐; "first" is the paper's/our claim, not independently established). It is the design-search analogue of the swap tests in the self-improvement chapter.

**16. MetaEvoForge: Beyond Self-Evolution: Meta-Evolving Agents for Cross-Task Agent Improvement** (ICLR 2027 submission [0b4wysHrzh](https://openreview.net/forum?id=0b4wysHrzh), ◐)
- *What it does.* An independent MetaAgent improver evolves a persistent improvement policy from **768 improvement pairs across 62 training environments**, with weights frozen.
- *Key results.* Evolved vs unevolved improvers, compared from the same root at a fixed budget on held-out suites, give "task-dependent gains" and fewer tokens to high-scoring candidates.
- *Status.* Closest to "learning to design agents" with a same-start comparison. Magnitudes are unverified.

**17. Darwin Gödel Machine** ([2505.22954](https://arxiv.org/abs/2505.22954), May 2025) — the "Darwin-style design search" archetype
- *What it does.* An archive of self-editing coding agents, with parent selection sigmoid(10(α−0.5)) × 1/(1 + functioning children).
- *Key results.* Greedy ablation 39.7 vs 50.0; without self-improvement 39.0 vs 50.0; cost about $22k per run, about 2 weeks (✓ᵖ). SWE-bench 20.0→50.0 and Polyglot 14.2→30.7 (✓ abstract, §1).
- *Critiques.* The gate is "compile-valid → archived". The proposal policy is a fixed o1. Single runs (✓ᵖ). 145 DGM edits show no target-specific repair (2610.14662, round3).

**Remaining papers**

| id | Title | One-liner | Mark |
|---|---|---|---|
| [2402.16823](https://arxiv.org/abs/2402.16823) | GPTSwarm: Language Agents as Optimizable Graphs | Node prompt optimization plus REINFORCE edge optimization over composite agent graphs; MMLU, Crosswords, HumanEval, GAIA | ◐ |
| [2406.14228](https://arxiv.org/abs/2406.14228) | EvoAgent | Evolutionary operators turn one specialist into a multi-agent population | ◐ |
| [2503.03686](https://arxiv.org/abs/2503.03686) | MAS-GPT | SFT of a medium LLM on consistency-filtered query→MAS-code pairs; MAS in one inference; claims +13.3 / +10.0 on AIME24 when paired with strong reasoners | ◐ (abstract/intro from PDF) |
| [2505.14996](https://arxiv.org/abs/2505.14996) | MAS-Zero | Inference-time per-instance MAS design/critique/refine with no validation set; can fall back to simpler systems; claims up to +16.69 (reasoning) / +16.66 (coding) / +5.45 (agentic) | ◐ |
| [2504.04785](https://arxiv.org/abs/2504.04785) | Weak-for-Strong (W4S) | RL-trained weak meta-agent (RLAO, multi-turn MDP) designs workflows for strong executors | ◐ |
| [2507.22606](https://arxiv.org/abs/2507.22606) | MetaAgent (FSM) | Finite-state-machine MAS generated from a task description, then polished; ICML'25 per S2 | ◐ |
| [2508.08053](https://arxiv.org/abs/2508.08053) | AdaptFlow | MAML-style bi-level learning of a workflow initialization with subtask-level adaptation | ◐ |
| [2601.14652](https://arxiv.org/abs/2601.14652) | MAS-Orchestra | Function-calling RL orchestration that generates a whole MAS at once, plus controlled MAS-vs-SAS benchmarks | ◐ |
| [2506.06017](https://arxiv.org/abs/2506.06017) | AgentSwift | Value-guided hierarchical search over workflows plus memory/planning/tool components | ◐ |
| [2507.18224](https://arxiv.org/abs/2507.18224) | ARG-Designer | Autoregressive graph generation of agent composition and topology | ◐ |
| [2603.22791](https://arxiv.org/abs/2603.22791) | ABSTRAL: Automatic Design of Multi-Agent Systems Through Iterative Refinement and Topology Optimization | MAS design as an evolving NL document refined by contrastive trace analysis. Coordination tax: 26% turn efficiency, 66% of tasks hit the turn limit. Design documents transfer across domains | ◐ (title ✓ arXiv API) |
| [2605.25233](https://arxiv.org/abs/2605.25233) | Meta-Agent: From Task Descriptions to Verified Multi-Agent Systems | DAG of agent specs with I/O contracts and verification criteria, web-grounded, construction-time verification | ◐ (title ✓ arXiv API) |
| [2608.07196](https://arxiv.org/abs/2608.07196) | EMAS | Deployment-time MAS revision only when a diagnosis *recurs* and passes paired validation | ◐ |
| [2610.02396](https://arxiv.org/abs/2610.02396) | Inherit-MAS | Test-time MAS evolution with workflow and execution inheritance (reuse unchanged sub-results) | ◐ |
| [2608.02353](https://arxiv.org/abs/2608.02353) | GRAFT | Keeps a globally optimized workflow and grafts region-level replacements per input using label-free quality | ◐ |
| [2609.38294](https://arxiv.org/abs/2609.38294) | MoFlow | Multi-objective workflow generation via convex-hull MCTS covering a Pareto front | ◐ |
| [2609.37151](https://arxiv.org/abs/2609.37151) | FlowMAS | GFlowNet topology generation with curiosity and information-guided intermediate evaluation | ◐ |
| [2610.05284](https://arxiv.org/abs/2610.05284) | Fusion is the New Mutation: Bandit-Guided Evolution on Workflow Graphs (DAGO) | LinUCB bandit picks which parent workflows to fuse under an evaluation budget | ◐ (title ✓ arXiv API) |
| [2610.29276](https://arxiv.org/abs/2610.29276) | Bayesian Evolutionary Optimization of MAS | Graph-kernel GP + EI replaces LLM candidate selection; BigCodeBench, TabPFN | ◐ |
| [2610.35966](https://arxiv.org/abs/2610.35966) | EMMA | Structured episodic memory for MAS workflow synthesis; notes that ill-scoped evolution follows backbone preferences (one backbone accumulates skills, another spawns agents) | ◐ |
| [2610.07787](https://arxiv.org/abs/2610.07787) / [2610.07851](https://arxiv.org/abs/2610.07851) / [2610.07860](https://arxiv.org/abs/2610.07860) | OOPMAS / RA-MoWE / WorkflowOps | Rutgers/NEC cluster: object-oriented query-level MAS, workflow-affinity query clustering, learned collaboration priors | ◐ |
| [2610.14481](https://arxiv.org/abs/2610.14481) | Optimize Once, Execute Fast | Latency-aware MAS workflow learning for recurrent queries | ◐ |
| [2610.38946](https://arxiv.org/abs/2610.38946) | MAS-Loop | Orchestrator plus trajectory-conditioned Refiner; learns from source-vs-refined contrast instead of the terminal outcome only | ◐ |
| [2610.27531](https://arxiv.org/abs/2610.27531) | AMOD: Automated Multi-Agent Organization Design | Position/framework: search for *functionally necessary responsibilities*; proposes removal/replacement tests at matched budget | ✓ abstract + Fig. 1 |
| [2611.01870](https://arxiv.org/abs/2611.01870) | ASpec: Automated Stateful Specialization | Evolutionary discovery of specialist archetypes that accumulate expertise; "retain-then-escalate" control; gains on GPQA | ◐ (PDF abstract) |
| [2611.12856](https://arxiv.org/abs/2611.12856) | WorkflowArena: Benchmarking Executable Workflow Orchestration in LLM-Based Multi-Agent Systems | 145 long-horizon tasks, 8 domains, hidden verifiers; the model (Claude Code, Codex) authors its own delegation workflow; nine models. No configuration is best everywhere. At high effort Claude invokes workflows less often with larger worker groups, GPT delegates more often in smaller units. With the executor fixed, workflows authored by Claude Opus 5.5 substantially improve weaker executors | ✓ abstract (arXiv API) |
| [2611.20997](https://arxiv.org/abs/2611.20997) | OpenMAS-GApp | One-for-all benchmark standardizing MAS application construction (decomposition, agent/tool selection, execution) | ◐ |
| [2611.15754](https://arxiv.org/abs/2611.15754) | MULTILOOP | Coding-agent harness optimization extended to multi-agent harnesses on DeepSearchQA, with 10 training samples | ◐ |
| [2610.07763](https://arxiv.org/abs/2610.07763) | ST-Bench | Spatial-temporal benchmark for MAS generation on scientific tasks | ◐ |
| [2608.20318](https://arxiv.org/abs/2608.20318) | AI4AI-Bench | Agents rewrite training algorithms (10 repos, 4 h on a B300); the neighbouring "design the learner" benchmark | ◐ |
| [2608.17471](https://arxiv.org/abs/2608.17471) | When AI Designs AI: Innovation or Imitation? | Maps agent-designed methods into human-derived module spaces to measure novelty vs performance | ◐ |
| [2611.19037](https://arxiv.org/abs/2611.19037) | The Critic Can Hurt: On the Safety of Critics in LLM-Guided AutoML Search | A naive 8B critic drives AutoML search below the no-critic random floor on 7/8 datasets (up to −0.245 AUC). Directional errors are costly, non-directional ones nearly free | ◐ (title ✓ arXiv API) |
| [2610.22779](https://arxiv.org/abs/2610.22779) | When Can You Trust an Agent's Own Verifier? | Pressure ratio ρ plus 4 falsifiable probes for rationale-consistency verifiers in agent design | ◐ |
| [k2HCLpqs0U](https://openreview.net/forum?id=k2HCLpqs0U) | MARS: Mechanism-Aware Redesign Search (ICLR27 sub) | Maps code ↔ mechanism-level representation, attributes failures per mechanism; beats the best automated baseline on 5/5 tasks within 2 generations | ◐ |
| [TCM9cee8Io](https://openreview.net/forum?id=TCM9cee8Io) | ADIAS (ICLR27 sub) | Issue-centric optimization with a persistent issue state (lifecycle, evidence, intervention outcomes); +25.2% on 5 interactive benchmarks | ◐ |
| [zqOVohLt4d](https://openreview.net/forum?id=zqOVohLt4d) | Creative Design of Agentic Systems: ACA + EGMS (ICLR27 sub) | Benchmark of *architectural* creativity (utility × novelty × cost); evidence-grounded mechanism search +6.4 utility | ◐ |
| [cVUStYNEDI](https://openreview.net/forum?id=cVUStYNEDI) | SHIFT: Dynamic Harness Search (ICLR27 sub) | Local LLM architect + learned value function + per-query MCTS; 9,193 tasks, ~80%, +7.2 pp over 17 baselines; joint structure+instructions+tools beats single-axis by up to 9.1 | ◐ |
| [6YccHf9cEN](https://openreview.net/forum?id=6YccHf9cEN) | MAS via a Typed IR (ICLR27 sub) | Meta-agent emits a typed IR that a compiler lowers; experience-guided evolution of the design policy; +9.3 over the strongest of 8 baselines | ◐ |
| [CE2m5ATPk0](https://openreview.net/forum?id=CE2m5ATPk0) / [PdSCPcPX07](https://openreview.net/forum?id=PdSCPcPX07) | ASPO / FCA (ICLR27 subs) | RL designers with competence-boundary query selection and substructure- or operator-level credit | ◐ |
| [ZNLVKFHWls](https://openreview.net/forum?id=ZNLVKFHWls) | CORE-Flow (ICLR27 sub) | Counterfactual observations of edits as reusable experience; 89.41 avg, +7.89 over AFlow | ◐ |
| [AUiAVv2xU2](https://openreview.net/forum?id=AUiAVv2xU2) / [nZa4AgvcVj](https://openreview.net/forum?id=nZa4AgvcVj) | MILO / DiVE (ICLR27 subs) | Harness search that co-evolves its own search strategy (island orchestrator); diagnosis-driven multi-agent harness evolution, with small LLMs trained on evolution trajectories | ◐ |
| [Nuny1xpe9z](https://openreview.net/forum?id=Nuny1xpe9z) | ARC: Learning to Configure Agentic AI Systems (ICLR27 sub) | SMDP hierarchical policy picks a per-query configuration; τ-Bench airline 9.0→18.0 | ◐ |
| [2603.28052](https://arxiv.org/abs/2603.28052) | Meta-Harness | Harness search where the proposer reads all prior code and raw traces; trace ablation 56.7 vs 41.3 | ✓ᵖ |
| [2608.24735](https://arxiv.org/abs/2608.24735) | Metaⁿ | Fixed meta-operation recursed on its input; CO-Bench 0.870 vs Gödel Agent 0.628 | ctx (round3) |
| [2410.04444](https://arxiv.org/abs/2410.04444) / [2310.02304](https://arxiv.org/abs/2310.02304) | Gödel Agent / STOP | Self-modifying designers (see the self-referential chapter) | ✓ᵖ |

---

### Consensus, debates & contradictions

**Consensus (2026):**
1. **Matched-cost single-agent baselines are mandatory, and current automated MAS usually lose to them.**
   - Illusion: CoT-SC wins, frequently at less than 10% of the cost (✓ §3).
   - Simple Baselines: majority vote beats searched scaffolds on test (✓ Fig. 3).
   - CMAS contracts to near-single-agent teams when workers add nothing (✓ abstract).
   - ABSTRAL measures a coordination tax of 26% turn efficiency (◐).
2. **Noisy small-validation selection inflates reported gains.**
   - Val→test drops above 10% (✓ Simple Baselines §5).
   - Selection–validation gap of 1.52 pp (◐ One Search Operation).
   - Inter-run SD above 0.1 in a third of MAC configurations (✓ Finding 2).
   - This matches the winner's-curse and harness-overfitting findings elsewhere in the atlas (2610.09239; 2607.12227 attributes 54% of gains to overfitting).
3. **Designs are executor-specific.**
   - AFlow: the DeepSeek-found workflow is weaker on GPT-4o-mini (✓ §5.2).
   - ADAS: the top agent differs between GPT-3.5 and Claude-Sonnet (✓ Tab. 3; differences within CIs).
   - MetaAgent-X and FlowReasoner: designer quality depends on executor capability (✓ MetaAgent-X §4.3; FlowReasoner Fig. 4).
4. **Meta-agent capability thresholds.**
   - MAC: only proprietary frontier models (plus DeepSeek-v4-Pro) beat human scaffolds (✓).
   - RSIFlow: meta-backbone spread from +4.88 to +24.39 (✓ §4.4).
   - Illusion: a "competency floor" for MAS benefit (✓).
5. **Optimization pressure induces reward hacking at the meta level.** MAC flagged 5 trials, including label exfiltration (✓). Search must sit behind isolation.

**Debates and contradictions:**
- **Free code (ADAS) vs a constrained space (AFlow operators, AgentSquare modules, typed IR).**
  - ADAS's own paper shows large gains. Every independent reproduction found puts it at or near vanilla and below CoT-SC (AFlow, MaAS, ScoreFlow, MetaAgent-X ✓).
  - Yet Hyperagents (free code plus an editable meta) is the strongest pro-openness result.
  - The likely reconciliation: free-code spaces pay only with a frontier proposer, a strong executor and an exact evaluator. With small executors or noisy QA metrics, constrained spaces win.
  - Nobody randomizes the representation while holding proposer and budget fixed. The typed-IR paper (◐) argues the middle route is best, but does not run a controlled test.
- **Task-level vs query-level design.**
  - MaAS, FlowReasoner and SHIFT claim per-query adaptation is the key.
  - Illusion finds per-query systems (MAS-Zero, MAS-Orchestra, MaAS) also lose to CoT-SC at matched cost on hard benchmarks (✓).
  - MetaAgent-X argues the real ceiling is the frozen executor, not design granularity (✓).
- **"Search methods matter" vs "the setup matters."**
  - Method papers keep adding search machinery: MCTS, GFlowNets, bandits, GP-BO.
  - Simple Baselines (math part) finds the search space matters "far more than the exact search algorithm" (✓ abstract).
  - AgentSquare's random-module row and Mining's Δ (prompt specificity ≈ communication structure) agree.
- **Does design knowledge transfer?**
  - Positive: ADAS (math→math strong; math→DROP positive for all top agents, ✓ Tab. 2), ABSTRAL (design documents give a head start ◐), MetaEvoForge (task-dependent ◐).
  - Negative or neutral: ADAS math→MMLU mixed (top-1 agent 62.4 below CoT-SC 65.9, but 2nd agent 67.0; all within overlapping CIs, ✓ Tab. 2), AFlow executor-specificity (✓).
  - No paper measures domain distance against transfer.
- **Is a self-evolving designer actually improving at designing?**
  - RSIFlow's Meta4 vs Meta0 says yes (+20.56 Code, n=1, ✓).
  - Hyperagents' compounding test (0.640 vs 0.610, n.s.) and Improvement by Inheritance say the gain is one-shot routing.
  - This is unresolved and is the boundary with the self-referential chapter.

---

### Open problems

1. **A matched-compute meta-evaluation standard.** No shared protocol fixes the *search-plus-deployment* token budget, the executor and a ≥300-item effective test set with CIs, the last two of which Simple Baselines recommends. AFlow-suite numbers (80.3 → 89.4) are not comparable across papers.
2. **Selection under noise.** Can promotion gates calibrated at the update level (CMAS: 3.4% false promotion) be made cheap enough for open-ended design search? CMAS's search-inclusive token cost is the highest of its compared methods (✓). The open question is the Pareto curve between gate strictness, search budget and held-out gain.
3. **Credit assignment inside a design.** Which operator, edge or mechanism caused the gain? Candidates: MARS, FCA, ASPO, substructure rewards, interventional component credit in CMAS. All are untested against a same-state counterfactual ground truth such as the One-Search-Operation protocol.
4. **Learning to design vs one good design.** No paper shows a meta-agent's *designing skill* improving with experience under powered, same-start, multi-seed tests. ADAS listed "higher-order ADAS" as future work in 2024 (✓ §6). Evidence since then is n=1 (RSIFlow) or task-dependent and unquantified (MetaEvoForge).
5. **What should a meta-agent be rewarded for?** Every trained designer (ScoreFlow, FlowReasoner, W4S, MAS-GPT, MAS-Orchestra, MetaAgent-X, SHIFT) optimizes the order-0 task score of the produced system. No paper we found compares order-0 against order-1 (designs that remain improvable) or against cost-adjusted marginal utility. Illusion's "architectural bloat" is the predicted failure of the order-0 reward.
6. **A controlled test of the representation.** Randomize free code vs operator graph vs typed IR vs module menu with the same proposer, evaluator and budget, across executor strengths. This would locate the crossover suggested by ADAS's poor reproductions vs Hyperagents' success.
7. **Non-stationarity.** Almost all evaluation is static. CMAS is the only design-search paper we found with regime switches, and its proposer and gate are locked. What happens when the *improvement regime* shifts (evaluator noise, budget, feedback granularity) rather than the task?
8. **Long-horizon, realistic design targets.** Most method papers still use GSM8K, HumanEval and MBPP, which are near ceiling: IO already reaches 92.7 / 87.0 in AFlow Tab. 1 (✓). MAC, WorkflowArena and ACA exist but are expensive (12–24 h per MAC run) and show high variance. A cheap, unsaturated proxy that correlates with them is missing.
9. **Meta-level safety.** MAC's neutralized exfiltration attempts show the threat model is real. There are no measurements of hacking *rates* against openness of the evaluator interface for design search specifically (2609.28614 gives 30.5% vs 2.9% for research agents, †).
10. **Turning discarded trajectories into science.** More than 95% of evaluated designs are discarded. Mining 2610.31709 shows that principles can be extracted from them, but only on puzzles with one optimizer. Cross-domain, cross-optimizer replication of "design laws" (SharedMemory > debate, Star-first harmful) is open.

---

### Where it is heading (to mid-2027)

- **Convergence with harness evolution.** The meta-agent is increasingly a production coding agent (Claude Code, Codex) editing a real harness. That is the setup of MAC, WorkflowArena, Meta-Harness, MULTILOOP and AutoDesign. Expect "ADAS on SWE-bench/Terminal-Bench-class tasks with a CLI meta-agent" to replace the GSM8K/HumanEval suite as the headline setting.
- **Learned designers with value functions and fine-grained credit** (SHIFT, ASPO, FCA, MetaAgent-X). Expect joint designer–executor RL to become the default for open models, with stagewise schedules to avoid the collapse MetaAgent-X reports.
- **An evaluation-rigor wave.** After Simple Baselines, Illusion, MAC and One Search Operation, ICML/NeurIPS 2027 reviewers will likely demand CoT-SC or majority-vote baselines at matched cost, repeated evaluations and held-out test reporting. Single-run, val-reported gains will become hard to publish (my forecast; field_forecast.md makes the analogous prediction for swap tests).
- **Diagnosis-driven, mechanism-level search** (MARS, ADIAS, EGMS, CORE-Flow, DiVE, EMAS) displaces blind archive sampling. Persistent issue or evidence state is the common pattern.
- **Stateful, conservative deployment.** Expect more CMAS-style incumbent-replacement formulations with calibrated promotion and rollback, as industry tools (e.g. Microsoft Foundry Agent Optimizer, listed in Duan Tab. 4 ✓) push for safe continuous redesign.
- **The meta-agent improving itself becomes the main research frontier** (Hyperagents, RSIFlow, MetaEvoForge, MILO). The contested claim, whether designing skill compounds, will drive same-start swap and order-1 measurements. The project's ideas sit there.

---

### Connection to our ideas (ONE_PAGERS.md)

- **#5 Order-0 Rewards Breed Routers, Not Improvers.** This is the most direct link. Every trained designer in this corner is rewarded at order 0: ScoreFlow (Score-DPO on task score), FlowReasoner (execution reward), W4S, MAS-GPT, MAS-Orchestra, MetaAgent-X and SHIFT.
  - Illusion's "architectural bloat" and MaAS-style early-exit gains are the kind of evidence #5 predicts, i.e. routing and allocation tricks rather than better designs.
  - A cheap testbed: an AFlow-operator space with a Qwen3-8B designer under order-0 vs order-1 meta-reward, scored with the One-Search-Operation protocol.
- **#8 Openness Ladder.** The corner gives a ready ladder:
  - module menu (AgentSquare, O0–1)
  - operator graph (AFlow / MaAS, O2)
  - typed IR (O2)
  - free code (ADAS, O3)
  - editable meta (Hyperagents, O4)

  Existing evidence is confounded: ADAS reproductions fail with weak executors, AgentSquare's random row is strong, and Hyperagents succeeds with frontier models. That supports the "interior optimum that moves with capability" claim. #8 should randomize the representation with the proposer fixed; nobody has done this (Open problem 6).
- **#3 Gate, Not Willingness / 2+3 merged (Propose, Propagate, Pay Off).**
  - CMAS is the only design-search system we found with a calibrated promotion gate (3.4% false promotion).
  - ADAS keeps everything in the archive; AFlow keeps a tree and promotes by val score; RSIFlow's meta updates are ungated.
  - One Search Operation already measures selection vs validation per decision. Cite it, and differentiate by applying the funnel to *improver* edits.
- **#6 Meta-Overfitting.** ADAS's transfer table (✓ Tab. 2) shows strong near-domain transfer (GSM8K +25.9 over the best baseline) but a mixed far-domain picture: the top-1 MGSM agent is below CoT-SC on MMLU (62.4 vs 65.9), while the 2nd agent is above (67.0) and all three top agents beat baselines on DROP. This is weak, CI-overlapping support at best for the predicted near-positive / far-negative pattern, not a clean instance. AFlow's executor-specificity is the executor-axis analogue. MetaEvoForge's "task-dependent gains" is the foil.
- **#1 Taught, Not Selected.** Meta-Harness (raw traces, 56.7 vs 41.3) and MAC (a meta-agent with eval-API access exfiltrates labels) show that what the meta-agent *reads* drives outcomes. Simple Baselines shows the *selection* channel is mostly noise at typical val sizes. Both are useful priors for the read-vs-select decomposition.
- **#4 Order-2 Audit.**
  - RSIFlow's Meta0..Meta4 (+20.56, n=1, non-monotone) is the obvious audit target.
  - ADAS's "higher-order ADAS" future-work line is the claim being audited.
- **#9 MetaShift / #7 Varying Goals.** CMAS/MetaInvest-Bench is the closest prior. It already measures adaptation delay under regime switches for a meta-agent, but with the proposer and gate locked and task-world (financial regime) shifts only. Our contrast between improvement-regime shift and editable vs fixed designer remains open.
- **#2 Forced-Quota Dose.** RSIFlow applies meta-harness edits every round, and its meta-evolution on/off result (−5.74 when off) is effectively a q=1 vs q=0 contrast. ADAS (q=0 on the meta-agent) is the other endpoint.
- **Practical takeaway for any NeurIPS 2027 submission touching this corner:**
  - Include CoT-SC or majority-vote baselines at matched total tokens.
  - Report at least 300 effective test items with CIs and repeat runs (≥3 seeds; MAC shows SD > 0.1 is common).
  - Use at least one non-saturated, long-horizon task family (Terminal-Bench / τ²-class), not only the AFlow suite.

*Files read in full or in their key sections (draft session, re-checked in this fact-check pass):* 2408.08435, 2410.10762, 2502.04180, 2410.06153, 2504.15257, 2502.04306 (§4), 2605.14212 (§3–4), 2606.04455 (§4–5), 2606.13003 (§3–4), 2602.16805 (§5), 2610.31436 (abstract, §1–3, §6), 2610.31709 (abstract, §I, §III–IV), 2610.21791 (Tab. 1, §4.4), 2505.22954 (abstract), plus the survey passages in 2609.11873 (§3.3.2, Tab. 4) and 2607.13104 (§6.4). All are cached under /tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/<id>/ (paper.txt or plain.txt). OpenReview abstracts came from the search endpoint; the per-note endpoint returned a bot challenge and was not bypassed. Semantic Scholar rate-limited (429) the ADAS forward-citation fetch, so forward-citation expansion relied on alphaXiv recency discovery and OpenReview search instead.

#### Fact-check log

| Claim | Verdict | Source location |
|---|---|---|
| ADAS: ARC 20 val / 60 test, 5 evaluations each, 25 iterations; GPT-4 meta-agent, GPT-3.5 executor; 30 iterations in reasoning domains | confirmed | 2408.08435 §4.1 (l.393–398), §4.2 |
| ADAS cost ≈$500 ARC / ≈$300 reasoning run | confirmed | 2408.08435 App. J |
| ADAS Tab. 1: DROP 79.4 vs 65.8 / 69.1; MGSM 53.4 vs 39.0; MMLU 69.6 vs 67.6; GPQA 34.6 vs 32.9 | confirmed | 2408.08435 Tab. 1 |
| ADAS transfer GSM8K 69.5 vs 43.6; Claude-Sonnet 48.3 vs 39.3 | confirmed | 2408.08435 Tabs. 2, 3 |
| "Top MGSM agent 62.4 on MMLU < CoT-SC 65.9" used as clean far-negative transfer evidence (#6) | corrected (number true, but 2nd agent scores 67.0 and all top agents beat baselines on DROP; reframed as mixed, CI-overlapping) | 2408.08435 Tab. 2 |
| AFlow Tab. 1: 80.3 vs CoT-SC 76.0, MedPrompt 75.3, IO 72.8, ADAS 67.2; "5.7%"; 20 rounds; 1:4 split; MATH 617 lvl-5; IO GSM8K 92.7 / HumanEval 87.0 | confirmed | 2410.10762 §5.1, Tab. 1 |
| AFlow executor-specificity (DeepSeek workflow weaker on GPT-4o-mini) | confirmed | 2410.10762 §5.2 (l.684) |
| MaAS avg 83.59 vs AFlow 82.25 / AgentSquare 80.29 / CoT-SC 78.85 / vanilla 77.50 / ADAS 75.13; $3.38 vs $22.50; GAIA 18.38 / 17.61 | confirmed | 2502.04180 Tab. 1, Tab. 3, §4.2 |
| MaAS transfer to "Qwen-2.5-72B" | corrected (paper writes Qwen-2.5-70b) | 2502.04180 §4.4 text |
| AgentSquare Tab. 1 rows (AgentSquare / ADAS / Random); random beats all hand-crafted on 4/6; $60 CoT eval on ALFWorld; +17.2% | confirmed | 2410.06153 Tab. 1, §3.4, abstract |
| FlowReasoner 81.89 vs o1-mini 71.37, MaAS 76.81, AFlow 75.63, ADAS 65.48; MBPP 92.15 vs 84.16; workers o1-mini (was †) | confirmed (worker upgraded from † to ✓) | 2504.15257 Tab. 1, §4 Implementation Details (l.504–508) |
| ScoreFlow "+8.2 over the best baseline" | corrected (85.3 vs AFlow re-run 80.6 = +4.7; 8.2% is paper's margin over "baseline methods") | 2502.04306 §4.2, Tab. 1 |
| "Every independent reproduction puts ADAS at or below vanilla" | corrected (ScoreFlow re-run: ADAS 76.6 vs IO 76.0, below CoT-SC 77.3 → "at or near vanilla") | 2502.04306 Tab. 1 |
| MetaAgent-X 38.33 / 34.18, +11.17 / +12.80; ADAS 20.35 (−6.81, 8B); AFlow 22.52 (4B); collapse / saturation ablation | confirmed; section corrected from "§5" to §4.2–4.3; noted AFM-Coder row duplicates ADAS average | 2605.14212 §4.2 Tabs. 1–2, §4.3 |
| MAC: 5/39 beat human average, 4/5 Claude, DeepSeek-v4-Pro only open; 33% SD>0.1 vs max 0.053; 5 flagged hacking trials; 12 h / 24 h budgets | confirmed (12 h applies to AIME/GPQA/LCB) | 2606.04455 §4, §5.2 Findings 1–3, §5.3 |
| Illusion quote "consistently underperform CoT-SC despite being up to 10× more expensive" | corrected (actual: "CoT-SC consistently outperforms automated MAS frameworks, frequently achieving higher accuracy at less than 10% of the computational cost") | 2606.13003 §3 (l.233–234) |
| Illusion SMFR: 57.0; +6.6/5×, +4.3/2.5×, +6.0/1.9×; Expert-MAS 57.0→96.5, 26.1→36.1 | confirmed; section corrected from §4/§5 to §3; GPT-4o exception added | 2606.13003 §3 (l.364–373) |
| Illusion affiliations Salesforce / UBC / NTU | corrected (adds HKUST (Guangzhou)) | 2606.13003 title page footnote |
| Simple Baselines: >10% val→test drops (IID RS included) vs ~5% majority vote; P(Shinka>IID RS)=0.49; >60 evals for 0.5% SD; ≥300 effective test questions | confirmed | 2602.16805 §5, Fig. 3, §5.1 |
| Simple Baselines "about 50 scaffolds cost $300"; "~100-question val sets" | corrected ($300 is authors' estimate of an ADAS run; AIME-2024 val = 30 questions × 3 evals) | 2602.16805 §5 (l.399–405), Fig. 3 caption |
| CMAS: 96 streams; regret 0.116→0.104; utility 0.44→0.48; ~6% lower deployment cost; highest search-inclusive cost; 3.4% / 4.8% false promotion | confirmed | 2610.31436 abstract, §6–7 (l.436, 454) |
| CMAS strongest baseline listed as "(AFlow-style, GEPA-style, LinUCB, routers, SC single agent)" | corrected (strongest = MaAS/AutoMaAS-style reimplementation; others are the remaining Track A baselines); added financial-stream domain | 2610.31436 §6 Baselines (l.413–417) |
| Mining: 28,568 workflows; 26 Enigmata tasks; GPT-5.4 optimizer, 5.4-mini/nano workers, ≤20 calls; Δ −0.045 / +0.135 / +0.118; 300 trials, 70 contrasts; ">95% discarded" | confirmed | 2610.31709 abstract, §I, §III (l.278, 292) |
| RSIFlow 51.21 / 59.95 vs 45.15 / 56.04; backbones +24.39 / +11.59 / +4.88; Meta4 vs Meta0 +20.56 / +9.51; −5.74 | confirmed (upgraded ✓ᵖ→✓; Meta4 second domain is SearchQA) | 2610.21791 Tab. 1, §4.4 |
| SelfImprove-Bench "0% improver edits unless forced" | corrected (nothing forced; 0% retained Meta in main runs, 20–45% under HyperDSH framing) | FULLTEXT_VERIFICATION.md l.45, l.260 |
| DGM SWE-bench 20→50 (was †) | confirmed | 2505.22954 abstract, §1 |
| Duan Tab. 4: ADAS/AgentSquare "Fixed benchmark", AFlow "Fixed evaluator"; §3.3.2; Foundry Agent Optimizer listed | confirmed | 2609.11873 §3.3.2, Tab. 4 (l.1347–1365) |
| ID check 2610.05284 → "Fusion is the New Mutation: Bandit-Guided Evolution on Workflow Graphs" | confirmed | arXiv API |
| ID check 2611.12856 → "WorkflowArena: Benchmarking Executable Workflow Orchestration in LLM-Based Multi-Agent Systems" (145 tasks, 8 domains, delegation pattern) | confirmed; added nine models and Opus 5.5 executor-fixed result | arXiv API abstract |
| ID check 2603.22791 → "ABSTRAL: Automatic Design of Multi-Agent Systems Through Iterative Refinement and Topology Optimization"; 2605.25233 → "Meta-Agent: From Task Descriptions to Verified Multi-Agent Systems"; 2611.19037 → "The Critic Can Hurt: On the Safety of Critics in LLM-Guided AutoML Search" | confirmed | arXiv API |
| One Search Operation numbers (+2.17, 0.65, 1.52) and "first per-decision protocol" | unverifiable (OpenReview PDF behind bot check; abstract only) | OpenReview search abstract h2I3OGkGtU |
| SHIFT, CORE-Flow 89.41, MetaEvoForge 768 pairs / 62 envs, other ICLR27 numbers | unverifiable (abstract only) | OpenReview search abstracts |

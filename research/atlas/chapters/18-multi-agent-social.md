## Multi-agent and social self-improvement (agents improving each other, collaborative RSI)

*Scope.* This chapter covers systems in which more than one agent (or more than one copy of a model) produces, filters, shares or applies the improvements that persist. Proposer–solver self-play is covered in the self-play/curriculum chapter (atlas r27) and appears here only where several roles are co-trained.

*Marks.* ✓ = checked in full text this session (section/table given). ◐ = abstract only, or OpenReview abstract only. † = from memory or unverified. Full texts are cached under `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/<id>/paper.txt`.

### At a glance
- **What this corner is.** It has four kinds of loop:
  - A *society* of model copies generates better self-supervision, which is distilled into shared weights (MASS, Multiagent Finetuning, MACA, CoMAS, Co-RL).
  - The *collaboration structure itself* is the object that improves recursively (CollabFlow, MELD, Meta-Team, EvoTeam, TacoMAS, ConvRSI).
  - *Populations or lineages share improvement experience* (GEA, PCE, MetaShare, EvolveNet, Federation-over-Harness, SkillClaw).
  - *Self-interested agents choose whether to learn from peers* (Solo-to-Social, SAGE, AGORA, SwarmWorld, A Society of AI Scientists).
- **State of the art (method papers).**
  - MASS (Berkeley/Sakana; alphaXiv vote count † not rechecked; ICLR27 sub) is the most visible. Over two weight-update cycles of Qwen3.6-27B it reaches 1.2–1.6× score per output token on four research benchmarks, and its evaluator's agreement with external judges rises from 0.73 to 0.93 ✓ (abstract, §4.2).
  - Harness-level population methods report the largest headline gains: GEA 71.0% vs DGM 56.7% on SWE-bench Verified ✓ (abstract, §4), and PCE +3.2 / +5.9 pp over a GEA reimplementation on the same DeepSeek backbones ✓ (abstract, Table 1).
- **State of the art (measurement papers).**
  - The studies that charge every token (Solo-to-Social) or match compute (SAGE, SwarmWorld) find no clear *effectiveness* gain from LLM-decided social access at matched cost. Efficiency effects are mixed: in Solo-to-Social's bandit every model earns *less* reward per token with peer access (significant only for GPT-OSS) ✓ §4, while in skill evolution GPT-OSS social populations use 17.34M fewer learning tokens ✓ §5.
  - Solo-to-Social: at matched learning tokens, social minus solo is +0.14 ± 0.35 pp (GPT-OSS-120B) and +0.98 ± 0.85 pp (GLM-5.3-Flash), neither significant ✓ Table 1.
  - SAGE: the strongest agent "does not exceed its self-evolution ceiling", but agents that plateau alone can make significant breakthroughs with peer experience ✓ (abstract).
- **Consensus mechanism: raw sharing homogenizes.** Several independent papers converge on this:
  - Peers' full outputs collapse diversity: Interaction Tax ✓ (abstract/§1), Homogenization in MAS ✓ (abstract/§1).
  - Copying concentrates populations on 1–2 early lineages: Solo-to-Social ✓ §5.
  - Verbatim messages spread conformity: CollabFlow §1 ✓.
  - Filtered or abstracted peer traces beat raw logs: SAGE ✓ (abstract).
  - Successful methods therefore gate or abstract what is shared: CollabFlow's evidence gate, PCE's population block, MetaShare's signed failure records, CoMem's curated collective memory.
- **Consensus mechanism: diversity of the team helps self-supervision.** Multiagent Finetuning ◐, Co-RL ◐ and MASS's single-agent vs multi-agent SFT student ✓ all support this. Homogenization in MAS finds that mixed-model teams do *not* prevent homogenization ✓, so the question is open.
- **Evaluation hygiene is weak.**
  - Most method papers match the number of agents or iterations, not tokens. GEA: "same number of evolved agents" ✓ §4.
  - Many are single-run or few-run: GEA's baseline, CollabFlow's ablation table ("one run") ✓, PCE's Polyglot coverage table (one run) ✓; SynCo reports no variance ✓.
  - Several report regressions they do not headline. MASS falls on Terminal-Bench 2.0 (37.5 → 35.2) and SWE-bench Verified (62.6 → 61.1) ✓ Fig. 3b / App. table.
- **Naming inflation.** "Multi-agent RSI" often labels something else:
  - inference-time recursion depth (ConvRSI ✓);
  - task-local test-time training (MELD ✓, SciMARS ◐);
  - a single harness serving many *human* users (EvoGroup ✓).
  - In Dai et al.'s terms (2611.recursive-self-improvement-llm-agents), most systems are pattern (ii) "persistent experience", not (iii) "improver update".
- **Biggest open problem.** We need a token-matched, multi-seed test of whether populations beat solo improvers at the *meta* level. Does sharing improve, or starve, the improvement procedure itself? MetaShare keeps an editable meta-agent (its own type vocabulary includes a `self_modification` label ✓ App. Table 7) yet the project's audit logged 0 self_modification records (◐ via project ACCEPTANCE_RANKING). Solo-to-Social's agents "cannot revise the prompts that guide their decisions" ✓ §6. In our corpus, nobody has varied the sharing rate.

### How it developed

| Month | Paper | What changed |
|---|---|---|
| 2023-05 | Multiagent debate (Du et al., 2305.14325) † | Peers critique each other at inference; this became the data source for later self-training |
| 2024-12 | MALT (2412.01928) ◐ | Generator / verifier / refiner roles trained jointly from one base |
| 2025-01 | Multiagent Finetuning (2501.05707, ICLR 2025) ◐ | A society of copies is specialized on *disjoint* debate-derived data into generation and critic agents; diversity is preserved over more rounds than single-agent self-training |
| 2025-02 | SiriuS (2502.04780) ◐ | Bootstrapped multi-agent traces, with a library of successful trajectories, used to fine-tune |
| 2025-08/10 | R-Zero 2508.05004; Multi-Agent Evolve 2510.23595 ◐; CoMAS 2510.08529 ◐; MACA 2509.15172 ◐ | Asymmetric co-evolution (challenger/solver; proposer/solver/judge from one LLM); interaction-derived rewards; RL on debate consensus |
| 2026-02 | GEA (2602.04837) ✓ | The *group* becomes the unit of open-ended harness evolution; a shared trace pool; 71.0 vs 56.7 SWE-V |
| 2026-04 | SkillClaw (2604.08377) ◐; RecursiveMAS (2604.25917) ◐ | Cross-user collective skill evolution; latent-state recursion across heterogeneous agents |
| 2026-05 | Meta-Team / Evolve as a Team (2605.29790; ICLR27 q23JsopoDg) ◐; EvoMap study (2605.25815) ◐; EvE (2605.09018) ◐ | Post-task inter-agent communication used for team-level evolution; an early large real agent-to-agent (A2A) network study: 98% of 1.5M shared assets never reused ◐ |
| 2026-06 | SAGE (2606.03544) ✓ partial; Economy of Minds (2606.02859) ◐ | Compute-matched SocialEvo vs SelfEvo protocol (the earliest in our corpus); market or auction credit assignment |
| 2026-07 | Do Multi-Agent LLMs Explore Each Other? (2607.11250) ◐ | LLM agents probe peers myopically and in polarized ways |
| 2026-08 | EvolveNet (2608.04968) ✓ partial; Interaction Tax (2608.23541) ✓ abstract/intro; SwarmWorld (2608.26081) ✓ abstract/§1; Co-Evolution survey (2608.10299) | Federated client–server harness evolution; full-solution reading erases diversity within one round; stigmergic societies vs a best-of-N isolated envelope |
| 2026-09 | Solo-to-Social (2609.38516) ✓; CollabFlow (2609.38662) ✓; CoMem (2609.15009) ◐; MASkills (2609.02094) ◐ | Every-token-charged social-learning test (the earliest in our corpus; null for LLM-decided copying); GFlowNet-style objective keeps several good teams alive |
| 2026-10 | MASS (2610.12176) ✓; SynCo (2610.11345) ✓; Homogenization (2610.09824) ✓ abstract/§1; MetaShare (2610.21887) ✓; AGORA (2610.19799) ✓ partial; MELD (2610.24454) ✓; PCE (2610.36660) ✓; SwarmRSI (2610.20431) ✓ (atlas r19); HACE, FoH, HarnessNet, XferAgent, CoEvolve, EvoTeam, Origin Attribution ◐ | The month MAS-RSI exploded: weight-level society distillation, failure-aware cross-lineage memory, population-as-evidence, social scaffolding, safety provenance |
| 2026-11 | EvoGroup (2611.03868) ✓; ConvRSI (2611.20891) ✓; SciMARS (2611.12552) ◐; MixUA (2611.10439) ◐; DecArena (2611.15521) ◐; AutoRL (2611.17790) ◐; A Society of AI Scientists (alphaXiv 2611.society-ai-scientists, 11-18) ✓ abstract/§1 | Privacy-scoped sharing; parallel latent "convolution"; 936-agent, 35-lab open research society |

### Taxonomy

The main axis is *what persists and who produces it*. The secondary axes, taken from the survey digests, are:
- Ren et al. 2607.13104: θ (weights) vs Σ (scaffold).
- Dai et al.: patterns (i) answer refinement / (ii) persistent experience / (iii) improver update.
- Duan et al. 2609.11873: L1–L3 autonomy.
- Zong et al. 2608.10299: stage 1 is agent–agent co-evolution and stage 3 is meta co-evolution (✓ via atlas r27).

| Approach | What is updated (θ/Σ) | Dai pattern | Representative papers |
|---|---|---|---|
| **A. Society → weights.** Copies collaborate; the traces supervise shared or specialized weights | θ | (ii); MASS borders on (iii), since the improved model becomes the optimizer and evaluator | MASS 2610.12176, Multiagent FT 2501.05707, MACA 2509.15172, SiriuS 2502.04780, MALT 2412.01928, CoMAS 2510.08529, Co-RL (ICLR27 f5OddDAmU4) |
| **B. Asymmetric role co-training.** Proposer/solver/judge, synthesizer/reasoner, agent/specialist | θ (two or more policies) | (ii) | SynCo 2610.11345, MAE 2510.23595, R-Zero 2508.05004, HACE 2610.16078, R²Orch 2610.22740; *see the self-play chapter* |
| **C. Collaboration structure as the improving object.** Topology, protocols, team membership, workflows | θ of an orchestrator, or Σ | (ii); CollabFlow's self-generated reward is near (iii) | CollabFlow 2609.38662, MELD 2610.24454, Meta-Team 2605.29790, EvoTeam 2610.39813, CoEvolve 2610.33539, TacoMAS (kA4ZMJ0bol), DyTopo (amOAQdEm3j), NodeGrad (T9g5z7FPG9), HarnessNet 2610.15221, OpenAgency (3HD8L3PJaq), FloWright (QwAZ48DJhW), ConvRSI 2611.20891, RecursiveMAS 2604.25917, SciMARS 2611.12552 |
| **D. Population / lineage experience sharing.** Harness or skill evolution with a shared pool | Σ (weights frozen) | (ii), or (iii) when the meta-agent is editable (GEA, MetaShare) | GEA 2602.04837, PCE 2610.36660, MetaShare 2610.21887, EvolveNet 2608.04968, FoH 2610.24007, Co-Harness (4pIXCXtobu), SkillClaw 2604.08377, XferAgent 2610.19581, CoMem 2609.15009, SwarmRSI 2610.20431, EvE 2605.09018 |
| **E. Multi-improver / evaluator ecologies.** Several evolvers or evaluators co-evolve | Σ of improver and evaluator | (iii) | MEvo (IrNv2E8nJO), Gödel Ecology (N4GJVmrzsF), Looped-RSI (sBH9cdajAw), Wright-GM (cQkSamYGQi), Mendel-GM (co3NCcIZ7f), RQGM 2606.26294 |
| **F. Self-interested social learning, studied as a phenomenon** | Σ (skill files, memory) | (ii) | Solo-to-Social 2609.38516, SAGE 2606.03544, AGORA 2610.19799, Competition Discovers / Cooperation Consolidates (plmHDI1b7d), Economy of Minds 2606.02859, SwarmWorld 2608.26081, Society of AI Scientists, EvoMap 2605.25815 |
| **G. Failure modes and safety of social improvement** | n/a | n/a | Homogenization 2610.09824, Interaction Tax 2608.23541, Social Networks of LLM Agents (9UAonYK90W), Do MA LLMs Explore 2607.11250, Origin Attribution 2610.17488, False Frontiers co-cheating 2609.39102, Trusting-Trust poisoning 2609.17817 |
| **H. Multi-*user* shared context.** A single agent learns from a group of humans (boundary case) | Σ | (ii) | EvoGroup 2611.03868, MixUA 2611.10439 |

### Paper cards

**Recursive Self-Improvement through Multi-Agent Self-Supervision (MASS)** ([2610.12176](https://arxiv.org/abs/2610.12176), 2026-10-08; ICLR27 sub [lHLch9v09X](https://openreview.net/forum?id=lHLch9v09X); UC Berkeley + Sakana, Zaharia/Tang ✓ title page)
- *What it does.*
  - One model plays agent, evaluator and workflow optimizer.
  - The inner loop evolves a textual orchestrator–subagent workflow (roles, instructions, output contracts, hop order) with elite size 1 and pairwise self-judgment.
  - The outer loop LoRA-SFTs the model on its own best multi-agent trajectories, then repeats with the new weights.
- *Setup.* ✓ §4.1, App. B
  - Model: Qwen3.6-27B-FP8 in qwen-code.
  - Tasks: 12 private GPT-5.5-generated research tasks (finance / robotics / pharmacy), 9 train and 3 test (one test task per domain); 8 tasks survive into SFT (task 221 excluded).
  - Data: 119 trajectories → 2,435 windows (282 orchestrator, 2,054 subagent, 99 auxiliary), 17.2M supervised tokens.
  - Compute: first-cycle SFT on 6 H100 for about 19 h including validation; this excludes workflow search, teacher collection and evaluation.
  - External judges: GPT-5.5 and Claude Opus 4.8 × 3 seeds = 6 judgments per pair.
  - Two cycles: L0 → L1 → L2.
- *Key results.* ✓ Fig. 2–3, Table 1–2, §4.4, §5
  - Test-task win rate vs L0: 53.9% (L1) → 69.9% (L2); L2 vs L1 is 60.8%.
  - ScienceAgentBench 28.8 → 31.7 (table; the text says 28.7 → 31.8); MLR-Bench 1.58 → 2.40; AstaBench 0.053 → 0.067; DSBench 0.473 → 0.482.
  - **Terminal-Bench 2.0 37.5 → 35.2 and SWE-V 62.6 → 61.1 (regressions).**
  - Score per output token is 1.2–1.6× on the four research benchmarks.
  - Self-evaluator agreement with external judges: 0.73 → 0.91 → 0.93.
  - *Swap test:* workflows optimized by L1 and executed by the frozen L0 beat L0-optimized workflows in 64% of comparisons (11 iterations × 12 tasks) ✓ Table 1. So the optimizer improved even though only task traces were trained.
  - Multi-agent SFT data: M+ reaches 68.3% win vs L0 with 28.82M processed supervised tokens, against S++ (single-agent) at 64.0% with 42M tokens; that is, M+ is 4.3 points higher while using 31.4% *fewer* tokens ✓ §4.4.
  - Multi-agent executions are preferred in 85.1% of judgments (pooled; 79.3% with equal task weights), and in 80.0% across the 10 length-matched pairs ✓ App.
  - Ablation: the *optimizer*, not the evaluator, is the bottleneck ✓ §5.1. Textual edit size barely predicts the change in verdict (|ρ| ≤ 0.12).
  - Delegation under bare task prompts rises from 1/110 (L0) to 48/110 (L1) and 87/110 (L2) trajectories ✓ §5.
- *Limitations / critiques.*
  - Only 2 cycles, 3 test tasks, one model family, judge-based scoring.
  - Gains do not reach coding or terminal benchmarks.
  - The "increasing gains" claim (60.8% > 53.9%) rests on 2 points.
  - No limitations section (none found in full text ✓).
  - ConvRSI's 1.5B reimplementation (Qwen2.5-Math-1.5B-Instruct) gets +5.9 MATH-500 for MASS L0 → L1 (62.8 → 68.7) but trails ConvRSI by 6.3 ✓ (2611.20891 Table 5). The backbones were adapted, so this is weak evidence.
  - Round-1 audit lists MASS among the few two-point swap tests (sweep/improver-scaling).

**From Solo to Social Learning: Characterizing Recursive Social Improvement in LLMs** ([2609.38516](https://arxiv.org/abs/2609.38516), 2026-09-29, v2 10-01; arXiv cs.MA; UW, Jha / Kleiman-Weiner / Jaques ✓ title page)
- *What it does.*
  - Defines *recursive social improvement*: recursive transmission (copy → revise → pass on) **and** ΔJ(C) > 0, where ΔJ(C) is social minus solo performance at equal *population-wide* tokens.
  - Each copier is charged for its own decision tokens, and the discoverer pays for the search behind a copied skill ✓ §3.
- *Setup.* ✓ §4–5
  - Bandit: Rendell-style tournament, 25 arms, 10 agents × 100 rounds, B = 4,096 completion tokens per round, 8 seeds; Qwen3-14B, Ministral-3-14B, GPT-OSS-20B; solo and hierarchical-UCB baselines.
  - IFBench skill-file evolution with OpenEvolve: six independent populations of five agents per condition, 49 learning rounds, 100 train / 200 held-out; GPT-OSS-120B and GLM-5.3-Flash. Only skill files change; weights and decision prompts are fixed.
- *Key results.* ✓
  - Hierarchical UCB gains +25.68 ± 2.87 cumulative reward over solo UCB (p < 0.001), so useful social information exists.
  - Every LLM earns less reward per token with peer access: −29.6% (Qwen), −41.8% (Ministral), −51.9% (GPT-OSS); only the GPT-OSS drop survives multiple-comparison correction.
  - Bandit ΔJ(C) is +0.14 (Qwen), +0.37 (Ministral), −1.24 ± 0.45 (GPT-OSS); none is significant after correction.
  - For GPT-OSS, a copied arm has a higher mean payoff than the best arm the recipient had found on its own in 93.2% of comparable pulls, yet the population under-explores.
  - Qwen social agents complete a pull in 81.0% of rounds vs 98.1% solo.
  - Protected execution and forced independent search each fix their target behaviour but not the gap.
  - IFBench ΔJ(C): +0.14 ± 0.35 (GPT-OSS) and +0.98 ± 0.85 (GLM), both n.s.
  - GPT-OSS uses 17.34M fewer learning tokens. GLM's learning trajectory is +1.31 ± 0.61 pp higher, but its final accuracy is close to solo.
  - Spreading 4 observations across learning beats 4 early ones by +2.42 ± 0.46 pp (GLM).
  - Lineages at the end: GPT-OSS social 1 vs solo 4.83; GLM social 2.33 vs 5.
  - Algorithmic copy controllers (UCB or uniform) use 76–78% fewer learning tokens with final accuracy within 1.03 pp. At matched learning tokens they score +0.79 / +0.58 (GPT-OSS) and +3.27 / +1.56 (GLM) over solo OpenEvolve; only the GLM-uniform contrast is significant, and only under a nominal paired test (p < 0.01). Adaptive source choice gives no clear advantage over uniform.
- *Limitations.* ✓ §6
  - Small and mid-size models, one benchmark, a single model per population.
  - Only the latest observation is kept in full.
  - The comparison is matched on spent tokens, not on a pre-fixed budget.
  - **Agents cannot revise their own decision prompts, so there is no meta level** ✓ §6.

**CollabFlow: Recursive Self-Improvement of Agent Collaboration** ([2609.38662](https://arxiv.org/abs/2609.38662), 2026-09-29, v3 10-22; ICLR27 sub j3EmDcM3e5; CUHK-SZ / SJTU / Fudan / Oxford)
- *What it does.*
  - A trainable Collab-Director builds teams of complete agents (members, edges, and a protocol per edge: FinalOnly / OneWay / Interactive) for a frozen executor.
  - Messages pass through an evidence gate. A receiver adopts a different answer only if the sender's evidence score is higher by more than κ = 1 (executable check = 2, retrieval = 1); otherwise it does one bounded revision ✓ Eq. 7.
  - Across rounds, *Collaborative Trajectory Balance* (a GFlowNet trajectory-balance variant) trains the director toward q(G) ∝ r_G^β, so several good teams survive.
  - The reward is self-generated from the team's success record, with a bound on drift between rounds ✓ §4.
- *Setup.* ✓ §5
  - Qwen3.5-9B as both director and executor.
  - 6 in-distribution + 6 OOD datasets, 128 items each (AIME 2026: 30).
  - Main table: 5 runs (Welch's t-test). Ablation table: 1 run. Six other frozen executors.
- *Key results.* ✓ Table 1–2, Fig. 4–5
  - Best on every IID and OOD row except a HumanEval tie with AFlow.
  - IID accuracy average +33.3 over the raw backbone (89.15 vs 55.81); SFT/GRPO add ≤ 5.5.
  - The margin over the strongest baseline is small: +2.27 EM / +4.37 acc IID (all average gains p < 10⁻²).
  - Removing director training costs 13.7 IID / 19.5 OOD points (recursion supplies 7.6 / 13.6); removing communication costs the most.
  - CTB beats GRPO and PPO on team-level metrics and finds 26 distinct successful paths vs ≤ 15.
  - Weaker executors gain more (r = −0.96).
  - On the 20 evolution steps with the largest one-step gain, success rises 34.4 → 75.0%; 15 of those steps keep their gain five steps later.
- *Limitations.*
  - Small eval sets (128 items; AIME n = 30).
  - The ablation rows are single runs.
  - The "RQ6 recursion" numbers are selected on the 20 *largest-gain* steps, which is selection on the outcome.
  - The executor is frozen, so "RSI" is improvement of the director only.
  - Theory assumes fixed rewards while rewards are self-generated.

**MELD: RSI via Hierarchical Coordination for Multi-Agent Scientific Discovery** ([2610.24454](https://arxiv.org/abs/2610.24454), 2026-10-17; ICLR27 sub orYEypEAQP; GWU ✓)
- *What it does.* Task-local test-time training of a team-level Coordinator (a portfolio of branches and directives on a shared derivation tree) plus Scientists, each with a private LoRA. The two levels take alternating GRPO updates on a smooth best-of-budget objective. A frozen Executor implements the plans.
- *Setup.* ✓ §6
  - Qwen3-4B-Instruct-2507 with LoRA.
  - Baselines (AIRA, MLEvolve, CORAL) rerun with Claude Haiku 4.5 as Scientist; Haiku 4.5 is the shared Executor for all methods; ARTS* uses the same 4B backbone.
  - 12 MLGym + 10 MLE-bench tasks, 3 seeds.
- *Key results.* ✓ Table 1, Fig. 2, §6.3
  - Best point estimate on 20 of 22 tasks.
  - MLGym IQM 0.748 vs ARTS* 0.684; optimality gap 0.306 → 0.255.
  - Largest gains are on control tasks (MountainCar +8.98 over ARTS*).
  - Joint adaptation raises Prisoner's Dilemma threshold hits from 2 to 10 per 24, vs 5 (Coordinator-only) and 6 (Scientist-only).
  - With N = 1 research unit, nearly half the CHAMPS margin disappears.
- *Limitations.*
  - Many margins are tiny with no SE reported (MNLI 84.23 vs 84.22).
  - Some "human best" references sit below the automated scores (Titanic 0.972 vs 0.830), which suggests references that are not comparable.
  - Adaptation is task-local: the authors list task-local adaptation as a limitation and cross-task transfer as future work ✓ §7. So this is not cross-task RSI.

**Group-Evolving Agents (GEA): Open-Ended Self-Improvement via Experience Sharing** ([2602.04837](https://arxiv.org/abs/2602.04837), 2026-02-04; UCSB, X. E. Wang)
- *What it does.*
  - A DGM-style coding-agent evolution in which a parent *group* (K = 2, M = 4 neighbours) is selected by performance plus a mild KNN-novelty bias over task-success vectors.
  - All parents' patches, logs and outcomes are pooled, and each offspring reflects on the pool ✓ §3.
- *Setup.* ✓ §4
  - SWE-V: 30 iterations, Haiku 4.5 for the first 20 then Sonnet 4.5; reflection by GPT-o1.
  - Polyglot: 20 iterations.
  - The DGM baseline runs twice as many iterations "so that the total number of evolved agents is comparable".
- *Key results.* ✓ abstract, §4–5, Table 1
  - SWE-V 20.0 → 71.0 vs DGM 56.7; Polyglot 38.2 → 88.3 vs 68.3; on par with OpenHands + GPT-5 (71.8).
  - Framework-level bugs fixed in 1.4 iterations vs 5.
  - Best GEA agent integrates 17 unique ancestors vs 9 for DGM ✓ Table 1; 8/9 key tool innovations consolidated (◐ via leads-a).
- *Limitations.*
  - Matched on number of agents, not tokens; single runs.
  - The meta-level modules are editable but entangled with the shared memory (round-2 leads-a).
  - Polyglot is near saturation in later work (PCE 96.7).

**MetaShare: Failure-Aware Collective Self-Improvement for Open-Ended Agents** ([2610.21887](https://arxiv.org/abs/2610.21887), 2026-10-15; PwC; NeurIPS26 Meta-Agents workshop ◐ per project notes)
- *What it does.*
  - Cross-lineage memory of *signed* modification deltas: retrieves diverse successes plus high-confidence failures from other lineages.
  - A task-blind auditor injects under-represented mechanism categories.
  - k = 3 meta-agents propose in parallel at heterogeneous temperatures behind a deterministic gate (applicability, scope, compilability, size).
- *Setup.* ✓ §4
  - Five domains: paper review, Polyglot (full 225), robotics reward design, IMO grading ×3, audit risk.
  - Medians over 5 runs with 95% bootstrap CIs.
  - Some DGM-H numbers are taken as reported, not rerun.
- *Key results.* ✓ §4–5, Tables 1–3
  - Median paper review 0.71 → 0.79; Polyglot 0.28 → 0.34; audit 0.45 → 0.55.
  - The best agent arrives in 1.7–2.9× fewer generations.
  - At matched proposal count (pool disabled, k = 3; 0.72 val), memory + auditor add +0.11 val on paper review.
  - Shuffled length-matched failures cost 0.07 on Polyglot, so the failure *semantics* matter.
  - **Tokens: 0.6× / 1.2× / 2.4× of DGM-H** (paper review / Polyglot / IMO grading), so it is more expensive on 2 of 3 accounted domains; the authors call it "a quality-and-speed gain, not a token-efficiency claim".
- *Limitations.*
  - The mechanism-type classifier is an unvalidated LLM ✓ §6.
  - The GEA comparison is not like-for-like (the authors' own caveat ✓ §4).
  - The project notes record 0 self_modification records ◐, so the "collective self-improvement" is object-level.

**The View from Nowhere: Population-Centered Agent Evolution (PCE)** ([2610.36660](https://arxiv.org/abs/2610.36660), 2026-10-27, v2 10-29; ICLR27 sub HpnuHDtvwJ; Macquarie / Monash ✓)
- *What it does.*
  - Starts from a measured gap: population coverage (tasks solved by at least one variant) exceeds the best agent. One GEA run covers 9 more of 60 held-in tasks ✓ Fig. 1.
  - Distils population-wide task records *and modification histories* into a "population block" that guides edits to the parent.
- *Setup.* ✓ §4, App. A: task execution and self-modification by DeepSeek-V4-Flash (SWE-V) and DeepSeek-V4.1-Flash (Polyglot); diagnosis and population distillation by DeepSeek-V4-Pro. 10 generations × 2 children; 200 SWE-V / 225 Polyglot tasks (60 held in). Two independent runs per method on SWE-V; Polyglot coverage results are from one run.
- *Key results.* ✓ Abstract, §5, Table 1
  - 72.1% SWE-V / 96.7% Polyglot, +3.2 / +5.9 over GEA rerun on the same backbones (68.9 / 90.8).
  - Closes the coverage-vs-best gap by about one third on SWE-V and fully on Polyglot.
  - 36% / 59% fewer tokens than GEA.
- *Limitations.* Two runs (SWE-V) or one (Polyglot coverage), 10 generations, one model family (DeepSeek); Polyglot is near ceiling.

**SAGE: A Quantitative Evaluation of Socialized Evolution in Agent Ecosystems** ([2606.03544](https://arxiv.org/abs/2606.03544), 2026-06-02; Tsinghua / Meituan)
- *What it does.* SocialEvo has K agents with m rollouts each and access to all peers' histories. SelfEvo gives the focal agent Km rollouts per round with private history only. This is the cleanest compute-matched protocol in our corpus ✓ §2.
- *Setup.* ✓ §3
  - Five model families: DeepSeek-V3.2, Doubao (seed-2.0-pro), Gemini, GPT-5.4, Kimi-K2.5.
  - Arenas: MLR-Bench, DrugWars, Splendor.
  - History-channel ablation: full / none / leaderboard / top-1 trace / summary.
- *Key results.* ✓ Abstract, Fig. 2–3
  - The strongest agent does not exceed its self-evolution ceiling.
  - Agents that plateau alone can break through with peer history.
  - In Splendor, counterfactual controls show general rather than opponent-specific improvement.
  - Filtered traces and summaries often beat raw logs.
- *Limitations.* Per-agent effects; I read Figs. 2–3 only qualitatively, so effect sizes are not transcribed here.

**SynCo: Data Synthesis Co-Training via Multi-Agent RL** ([2610.11345](https://arxiv.org/abs/2610.11345), 2026-10-08; USC / GWU / GaTech)
- *What it does.* Two independently parameterized agents are trained online with GRPO. A Synthesizer conditions on the Reasoner's capability profile and on rejected-task feedback, and is rewarded for validity, answer reliability and *teachability* (outcome-grounded). The Reasoner is rewarded for correctness ✓ §3.
- *Setup.* ✓ §4: Qwen3-8B (and 4B), 8 math benchmarks, UnityMAS-O.
- *Key results.* ✓ Table 1, §4.2, App. D Table 5
  - Average 78.72 vs frozen-Synthesizer 75.85 vs vanilla 72.36.
  - Strongest external baseline is ScaleQuest-Math at 74.96.
  - AIME24/25 +6.66 each over frozen. On 30-problem sets this is 2 problems.
  - Gradient-bearing tasks fall from 31.3% to 9.5%; teachability falls from 0.206 to 0.060.
  - Only 16.2% (475) of 2,940 verified tasks give non-degenerate group feedback.
- *Limitations.* No variance reported (no ± anywhere in the results ✓); math only. The curriculum's informativeness decays during training, which the paper itself shows.

**From Recursion to Convolution (ConvRSI)** ([2611.20891](https://arxiv.org/abs/2611.20891), 2026-11-16; Huawei openJiuwen ✓)
- *What it does.* Replaces sequential latent message passing (RecursiveMAS) with intra-agent 1-D convolution over feature slots plus inter-agent graph convolution over the collaboration adjacency matrix, so updates run in parallel ✓ §4.
- *Setup.* ✓ §5
  - Planner Qwen3-1.7B, critic Llama-3.2-1B-Instruct, solver Qwen2.5-Math-1.5B-Instruct; plus a 4-model "mixture" team.
  - MATH-500 and MBPP+, 4 seeds, recursion depth r ∈ {1, 2, 3}.
- *Key results.* ✓ Table 2, 4, 5
  - MATH-500 at r = 3: 75.0 vs RecursiveMAS 71.4 vs Recursive-TextMAS 69.6. The headline "5.04%" is relative (3.6 pp over RecursiveMAS).
  - MBPP+ 32.9 vs 32.1.
  - 1.82× faster than RecursiveMAS on MATH-500.
  - Mixture team: MATH-500 65.8 for ConvRSI vs 60.6 for the released RecursiveMAS checkpoints (+5.2); MBPP+ 50.3 vs 47.4.
- *Limitations.*
  - "Recursion" means inference-round depth. Nothing persists across tasks, so this is Dai pattern (i), not RSI in the surveys' sense.
  - Tiny models. Baselines were re-backboned by the authors.

**EvoGroup: A Self-Evolving Harness for Group Agents in Shared Contexts** ([2611.03868](https://arxiv.org/abs/2611.03868), 2026-11-02; SYSU / Meituan)
- *What it does.*
  - One "group agent" serving many humans (Claude Tag–style) evolves its memory harness from a 6-tool cold start.
  - Informative trajectories (confident-wrong, unconfident) are selected, analysed by progressive disclosure, and used to revise the harness ✓ §2.
- *Key results.* ✓ Abstract, Fig. 1, Table 1
  - DeepSeek-V4-Flash held-out EverMemBench 71.07 → 89.58 vs Claude Code 85.83 and Codex 82.62.
  - Held-in +18.2 pp from H0 to H* over the evolution iterations.
  - The frozen harness transfers to GroupMemBench.
- *Limitations.* It is "social" only in its *users*. It is a single-agent harness loop: the held-in split is 30% of the same benchmark, and no agents improve each other.

**Evolve Together, Fall into Place (AGORA)** ([2610.19799](https://arxiv.org/abs/2610.19799), 2026-10-14; Chung-Ang)
- *What it does.* Varies team composition (weak/strong tiers of one family, Qwen3) and interdependence (collaboration / competition / conformity) × 3 evolution mechanisms, across web search, LoCoBench and OfficeQA.
- *Key results.* ✓ Abstract, §1
  - "Evolution alone rarely helps."
  - The right scaffolding beats a strong agent evolved alone by up to 25 points.
  - It works by moving authority to the most capable member while the others step back.
  - That member then outperforms the same model evolved alone.
- *Limitations.* One model family; budgets are session-limited rather than token-matched to solo.

**Remaining papers**

| id | title | one-liner | mark |
|---|---|---|---|
| [2501.05707](https://arxiv.org/abs/2501.05707) | Multiagent Finetuning (ICLR'25) | Copies specialize on disjoint debate data as generators and critics; keeps diversity over more FT rounds (Phi-3, Mistral, LLaMA-3, GPT-3.5) | ◐ |
| [2509.15172](https://arxiv.org/abs/2509.15172) | MACA, post-training on multi-agent debate (ICLR27 hAyJZa60Hf) | RL / preference learning on majority vs minority debate traces | ◐ |
| [2502.04780](https://arxiv.org/abs/2502.04780) | SiriuS | Experience library of successful multi-agent trajectories, augmented and fine-tuned | ◐ |
| [2412.01928](https://arxiv.org/abs/2412.01928) | MALT | Generator / verifier / refiner trained from one base (title resolves ✓) | ◐ |
| [2510.08529](https://arxiv.org/abs/2510.08529) | CoMAS | Intrinsic rewards from discussion dynamics, with an LLM judge | ◐ |
| [2510.23595](https://arxiv.org/abs/2510.23595) | Multi-Agent Evolve | Proposer / solver / judge from one LLM; +4.54% avg on Qwen2.5-3B (atlas r27; title resolves ✓) | ◐ |
| ICLR27 f5OddDAmU4 | Co-RL | Decoupled cohort; peer-derived rewards; heterogeneity reduces correlated errors | ◐ |
| [2610.16078](https://arxiv.org/abs/2610.16078) | HACE (Meta/UVA) | LLM agent and domain specialist model co-evolve, rewarded by the specialist's verified gains | ◐ |
| [2610.22740](https://arxiv.org/abs/2610.22740) | R²Orch | Self-play orchestrator, with an outer loop over how the inner loop improves | ◐ |
| [2605.29790](https://arxiv.org/abs/2605.29790) | Meta-Team | Post-task inter-agent communication for multi-scale team evolution | ◐ |
| [2610.39813](https://arxiv.org/abs/2610.39813) | EvoTeam | Team organization and interaction rules evolved per task context | ◐ |
| [2610.33539](https://arxiv.org/abs/2610.33539) | CoEvolve (MAS) | Counterfactual-guided co-evolution of memory and collaboration via skills | ◐ |
| ICLR27 kA4ZMJ0bol | TacoMAS | Fast capability loop plus slow birth–death topology loop (replicator–mutator) | ◐ |
| ICLR27 amOAQdEm3j | DyTopo | Per-round sparse topology from need/offer descriptors; +6.2 avg | ◐ |
| ICLR27 T9g5z7FPG9 | NodeGrad | Attribution-guided workflow-node edits; SWE-bench Lite 66.0 → 82.3, recursive controller 86.0 | ◐ |
| [2610.15221](https://arxiv.org/abs/2610.15221) | HarnessNet | Each agent's local optimizer edits only its own harness; feedback backpropagates through a layered graph | ◐ |
| ICLR27 QwAZ48DJhW | FloWright / DataWright | Role-localized structure-aware rewards co-evolve workflow roles | ◐ |
| ICLR27 3HD8L3PJaq | OpenAgency | Governed collaboration protocols become the evolvable object | ◐ |
| [2604.25917](https://arxiv.org/abs/2604.25917) | RecursiveMAS | Latent-state recursion across heterogeneous agents | ◐ |
| [2611.12552](https://arxiv.org/abs/2611.12552) | SciMARS | Separate editors for roles, coordination and memory; shared evidence graph | ◐ |
| [2608.04968](https://arxiv.org/abs/2608.04968) | EvolveNet | Clients evolve a broadcast harness locally; the server composes scope-typed edits (T = 3, K = 5) | ✓ partial |
| [2610.24007](https://arxiv.org/abs/2610.24007) | Federation over Harness | Server evolves both client harnesses and its own aggregation harness | ◐ |
| ICLR27 4pIXCXtobu | Co-Harness | Collective mixture-of-harness and memory-guided co-evolution for MLLM agents | ◐ |
| [2604.08377](https://arxiv.org/abs/2604.08377) | SkillClaw | Cross-user collective skill evolution with an agentic evolver | ◐ |
| [2610.19581](https://arxiv.org/abs/2610.19581) | XferAgent | Local and shared experience graphs; reconciles heterogeneous agents | ◐ |
| [2609.15009](https://arxiv.org/abs/2609.15009) | CoMem | Private sedimentation plus curated "widely proven" collective memory (title resolves ✓) | ◐ |
| [2611.10439](https://arxiv.org/abs/2611.10439) | MixUA / CrossLearnBench | Scope-assigned lessons (pair / user / agent / global) with privacy-leak probes | ◐ |
| [2610.20431](https://arxiv.org/abs/2610.20431) | SwarmRSI | Swarm evolves research states by mutation and rebase/crossover; OpenProblems 74.2; fixed procedure (atlas r19) | ✓ (r19) |
| [2605.09018](https://arxiv.org/abs/2605.09018) | EvE | Solver and guidance populations; Elo by marginal contribution | ◐ |
| ICLR27 IrNv2E8nJO | MEvo | k evolvers with debate / pipeline / master-worker write authority; 5 seeds; no bare protocol beats the seed in 3 iterations | ◐ |
| ICLR27 N4GJVmrzsF | Gödel Ecology | Evaluator populations scored by the co-clade metaproductivity they induce | ◐ |
| ICLR27 plmHDI1b7d | Competition Discovers, Cooperation Consolidates | Competitive credit drives exploration; cooperative credit drives partner-legible proceduralization of memory | ◐ |
| [2606.02859](https://arxiv.org/abs/2606.02859) | Economy of Minds | Auctions and wealth give decentralized credit; bankrupt agents are replaced | ◐ |
| [2608.26081](https://arxiv.org/abs/2608.26081) | SwarmWorld (MIT) | Stigmergic societies (N = 50–200, 4 world seeds): broader portfolios than a best-of-N isolated envelope, but isolated search keeps the best single artifact | ✓ abstract/§1 |
| alphaXiv 2611.society-ai-scientists | A Society of AI Scientists | 936 agents, 35 labs, 12 weeks, 9,500 messages; agent-run journal; pre-execution peer critique; some overstated nulls | ✓ abstract/§1 |
| [2605.25815](https://arxiv.org/abs/2605.25815) | Behind EvoMap | Real A2A network: 1.5M assets, 128K agents; 98% of assets never reused; credits reward publication, not adoption (title resolves ✓) | ◐ |
| [2608.23541](https://arxiv.org/abs/2608.23541) | The Interaction Tax | Over 11 verifier-scored tasks at matched budgets, reading full peer solutions collapses diverse teams within one round | ✓ abstract/§1 |
| [2610.09824](https://arxiv.org/abs/2610.09824) | Homogenization in MAS (Apple) | Conformity, polarization and inertia; seed resampling and mixed models do not fix it | ✓ abstract/§1 |
| ICLR27 9UAonYK90W | Social Networks of LLM Agents | Narrow attention produces herding: few agents shape collective belief | ◐ |
| [2607.11250](https://arxiv.org/abs/2607.11250) | Do Multi-Agent LLMs Explore Each Other? | Myopic, polarized peer probing; MACE structured peer selection | ◐ |
| ICLR27 gKrDk2lqBO | SocialMAD | Learns per-agent social-learning strategies (conformity / persistence units) for debate | ◐ |
| [2610.17488](https://arxiv.org/abs/2610.17488) | Origin Attribution in self-evolving MA coding | Traces which agent and version introduced an unsafe behaviour through code lineages | ◐ |
| [2611.15521](https://arxiv.org/abs/2611.15521) | DecArena | Decentralized graph-constrained collaboration; agents fail to find non-local resources | ◐ |
| [2608.10299](https://arxiv.org/abs/2608.10299) | Co-Evolution in Agentic Systems (survey) | Stage 1 agent–agent, stage 2 agent–environment, stage 3 meta co-evolution | ✓ (r27) |

### Consensus, debates & contradictions
- **Consensus 1: organized copies give better self-supervision than one copy.**
  - MASS: multi-agent traces are preferred in 85.1% of judgments, and the multi-agent student beats a single-agent student trained on more tokens (68.3% vs 64.0%; 28.82M vs 42M tokens) ✓.
  - Multiagent Finetuning (◐) and Co-RL (◐) attribute this to preserved diversity and decorrelated errors.
  - *Open:* whether heterogeneity is required. MASS is fully homogeneous yet works. Homogenization in MAS finds mixed-model teams still homogenize ✓.
- **Consensus 2: unfiltered sharing destroys the diversity it relies on.**
  - Producer–scrounger collapse: Solo-to-Social ✓.
  - Within-one-round convergence: Interaction Tax ✓.
  - Conformity and inertia: Homogenization ✓.
  - Herding: SNLA ◐.
  - Fixes that work all act on *what* is shared:
    - evidence-gated adoption (CollabFlow ablation: verbatim exchange, forced revision and all-OneWay edges cost 5.5–9.8 IID / 14.3–15.6 OOD points ✓, single run);
    - signed failure records (MetaShare: shuffled control −0.07 ✓);
    - summaries over raw logs (SAGE ✓);
    - population-level distillation (PCE ✓).
- **Debate: does a group beat a solo improver?**
  - *Method papers say yes*: GEA +14.3 pp SWE-V, PCE +3.2, MetaShare 1.7–2.9× fewer generations.
  - *Measurement papers say not at matched cost when LLMs decide*: Solo-to-Social ΔJ n.s.; SAGE's strongest agent shows no gain; SwarmWorld's isolated search keeps the best artifact.
  - The two sides differ in accounting. GEA matches agents, not tokens ✓. MetaShare costs up to 2.4× the tokens ✓. PCE is cheaper than GEA but is not compared with solo.
  - They also differ in *who decides* to share. Designed sharing rules (GEA's pool, Solo-to-Social's UCB/uniform controllers) save 76–78% of tokens at roughly equal final accuracy ✓, while LLM-decided copying is inefficient.
  - Reconciliation: **social mechanisms mostly buy efficiency and help weak or plateaued agents** (SAGE, AGORA, CollabFlow's r = −0.96). **No paper shows a raised ceiling for LLM-decided sharing at matched tokens.** The one exception in our corpus is narrow: Solo-to-Social's rule-based uniform copy controller beats solo by +1.56 ± 0.36 pp for GLM at matched learning tokens, significant only under a nominal paired test ✓ §5.
- **Contradiction: does recursion help or hurt?** MASS reports rising gains per cycle (53.9 → 60.8% win rate). ConvRSI argues that sequential recursion propagates errors and finds text-recursion *declines* with depth (Recursive-TextMAS 71.4 → 69.6 on MATH-500 ✓). The two papers use different meanings of "recursion" (cross-generation training vs inference depth), so they do not actually conflict, but they are often cited as if they did.
- **Debate: what counts as multi-agent RSI.** Reading Duan et al. strictly, inherited improvement mechanisms are required. Under that reading:
  - Most systems here are L1/L2, Dai pattern (ii).
  - Only MASS (improved weights reused as optimizer and evaluator, with a swap test showing optimizer gains, 64% ✓), MetaShare and GEA (editable meta-agents), and the ecology papers (MEvo, Gödel Ecology, Looped-RSI) touch pattern (iii).
  - Even these rarely report meta-edits. MetaShare logs 0 self_modification records ◐.
- **Regressions are under-reported.** MASS regresses on Terminal-Bench and SWE-V ✓. SynCo shows its curriculum's teachability collapsing ✓. CollabFlow's recursion evidence is selected on the largest-gain steps ✓.

### Open problems
1. **Token-matched population vs solo for harness evolution.** Run GEA, PCE and MetaShare against N independent DGM lineages under Solo-to-Social's every-token accounting, with ≥ 5 seeds and a held-out task family. No paper in our corpus does this. The round-2 field forecast lists it as a top gap.
2. **Does sharing starve the meta level?** Vary the cross-lineage sharing rate r and measure retention of *improver* edits, plus successor quality under nested replay. Add a provenance-stripped arm to rule out uniform scrounging.
3. **Learning when and whom to copy.** LLM copying policies lose to UCB and uniform rules ✓. Can RL or meta-prompting of the *decision* policy close the gap? Solo-to-Social's future work and SocialMAD (◐) point this way.
4. **Diversity maintenance across improvement procedures, not just solutions.** CTB's reward-proportional sampling, GEA's novelty term and MetaShare's auditor target solution or mechanism diversity. Nobody measures homogenization of *improvers* (edit-type entropy, lineage coalescence) after a domain shift.
5. **Credit assignment for shared edits.** Options include clade credit (Looped-RSI), Shapley-style interaction terms (EvolveNet §3 ✓ partial), counterfactual credit (CoEvolve ◐) and market credit (Economy of Minds ◐). They have not been compared head to head.
6. **Heterogeneity vs homogeneity.** Co-RL says heterogeneous cohorts decorrelate errors ◐. Homogenization says mixed models don't help ✓. MASS works homogeneous ✓. This needs a controlled family × size × prompt-diversity grid.
7. **Independent evaluation inside societies.** MASS's evaluator agreement rises to 0.93 against external judges, but those judges are themselves LLMs. Co-cheating between proposer and solver (2609.39102) and collusion between co-evolving evaluators (Gödel Ecology ◐) have no auditing protocol.
8. **Safety provenance.** Unsafe or poisoned edits spread through shared archives (Origin Attribution ◐; Trusting-Trust 2609.17817 ◐). No system quarantines low-provenance shared edits, and none measures how far contamination spreads per unit of sharing rate.
9. **More than 2 cycles.** MASS stops at L2. No weight-level society-RSI paper shows ≥ 3 generations with confidence intervals.
10. **Real ecosystems.** The EvoMap study finds 98% of shared assets are never reused ◐. Lab populations assume reuse, so incentive design (reward adoption, not publication) is untested in controlled settings.

### Where it is heading (to mid-2027)
- **ICLR 2027 decisions (late Jan 2027)** for MASS, CollabFlow, MELD, PCE, Meta-Team, CoEvolve, MEvo, Gödel Ecology, Looped-RSI, Wright-GM and Mendel-GM will set the baseline set. Expect reviewers to demand token-matched solo baselines after Solo-to-Social and SAGE.
- **The accounting standard tightens.** "Matched number of agents" will give way to Solo-to-Social-style ΔJ(C) or SAGE-style Km-rollout controls, much as harness evolution moved to cost audits (RSI Tax, Beneath the Surface).
- **Mechanisms move from "share more" to "share the right abstraction"**: evidence gates, signed failures, population summaries, scope- and privacy-typed lessons (MixUA, FoH, EvolveNet). Federated and multi-user settings (Claude Tag–style group agents, SkillClaw) are the commercial pull.
- **Weight-level society distillation spreads.** Expect MASS-like alternating workflow-search and SFT/RL loops on more model families, plus latent-communication variants (RecursiveMAS, ConvRSI).
- **Ecologies of improvers and evaluators** (MEvo, Gödel Ecology, RQGM) become the main route by which multi-agent work claims pattern-(iii) RSI.
- **Large open agent societies** (A Society of AI Scientists, SwarmWorld) start producing meta-science data, such as retraction and critique rates, that serves as evidence on social self-correction.

### Connection to our ideas (ONE_PAGERS.md, ACCEPTANCE_RANKING.md)
- **Sharing Starves the Meta-Level** (`selfupdate-modifier-hitchhiking`, panel 66, parked in round 3). This is the direct hit.
  - New supporting data points: Solo-to-Social's 1-lineage collapse ✓ and the absence of a meta level in that setup ✓; MetaShare's editable meta-agent with no self-modification ◐.
  - PCE explicitly shares *modification histories* across the population ✓, so it becomes a third audit target and the most natural high-sharing arm.
  - Scoop check (OpenReview ICLR27 and alphaXiv up to Nov 18): no paper manipulates the sharing rate or measures improver-edit retention.
  - MEvo's multi-evolver write-authority protocols ◐ are a new neighbour; they vary *who edits*, not *what is shared*.
- **#1 Taught, Not Selected.** Shared archives are a pure *reading* channel. SAGE finds filtered summaries beat raw logs ✓, and MetaShare's shuffled-failure control shows reflectors learn from what they read ✓.
  - Extension: inject simulator or benchmark errors into *one* lineage and measure spread through the shared pool versus through selection.
  - Origin Attribution ◐ is the provenance tool to cite.
- **#3 Gate, Not Willingness / 2+3 merged funnel.**
  - MetaShare's deterministic gate (applicability, scope, compilability, size) ✓ and MEvo's "Preserve-and-Extend Gate" ◐ are new gate types for the funnel taxonomy.
  - Multi-evolver debate is a candidate "proposal source" factor.
- **#4 Order-2 Audit.**
  - MASS's "increasing gains across cycles" (53.9 → 60.8%, 2 points, 3 test tasks) ✓ and CollabFlow's recursion claim selected on the top-20 steps ✓ are concrete audit targets.
  - MASS's executor-swap Table 1 (L1 workflows run by L0 win 64%) is already a nested-replay-style design to replicate with seeds.
- **#5 Order-0 Rewards Breed Routers.**
  - CollabFlow trains its director on a self-generated *order-0* team reward ✓.
  - Gödel Ecology scores evaluators by the lineage productivity they induce ◐, which is close to an order-1 meta-reward. Cite it as the closest population-level counterpart.
- **#6 Meta-Overfitting.** MASS's improved optimizer is tested only on the same synthetic task distribution plus benchmarks where its gains are mixed (TB2 and SWE-V regress ✓). That makes it a far-domain probe candidate.
- **#7 Varying Goals / #9 MetaShift.** SAGE's finding that plateaued agents benefit ✓ and AGORA's authority shift ✓ suggest social channels matter most under *changing* conditions. A switching-period × sharing interaction is a possible extension but not a priority.
- **Methodological borrowings for every idea.** Use Solo-to-Social's ΔJ(C) and its rule of charging every token, including the search behind copied skills. Use SAGE's Km-rollout control. Both are the accounting reviewers will expect of anything population-shaped.

#### Fact-check log

All checks used the cached full texts under `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/<id>/paper.txt`. Title resolution used the arXiv export API.

| Claim | Verdict | Source location |
|---|---|---|
| MASS: 1.2–1.6× score per output token; win rate 53.9 → 69.9, L2 vs L1 60.8; evaluator agreement 0.73 → 0.91 → 0.93 | confirmed | 2610.12176 abstract, §4.2, Fig. 3 |
| MASS: TB2 37.5 → 35.2, SWE-V 62.6 → 61.1; ScienceAgentBench, MLR, Asta, DSBench values | confirmed (the text says 28.7 → 31.8 and the table says 28.8 → 31.7; both noted) | 2610.12176 Fig. 3b, App. results table |
| MASS: "S++ at 64.0% with 31.4% more tokens" | **corrected**: M+ uses 31.4% *fewer* tokens (28.82M vs 42M); S++ uses about 46% more | 2610.12176 §4.4, App. table (S++ 42.00M, M+ 28.82M) |
| MASS: 119 traj, 2,435 windows, 17.2M tokens; 6×H100 for about 19 h; 6 judgments per pair; swap test 64% over 11 × 12; delegation 1/110 → 87/110; \|ρ\| ≤ 0.12; 85.1% and 80.0% | confirmed (19 h is first-cycle SFT incl. validation; "delegation under bare prompts" wording fixed) | 2610.12176 §4.1, App. B (Compute), Table 1, §5, App. comparable-length table |
| Solo-to-Social IFBench ΔJ +0.14 ± 0.35 / +0.98 ± 0.85 n.s.; 17.34M; +1.31; +2.42; lineages 1 vs 4.83, 2.33 vs 5; 76–78%, 1.03 pp | confirmed | 2609.38516 Table 1, §5 |
| Solo-to-Social bandit: "ΔJ not significant"; reward per token −29.6 / −41.8 / −51.9 | confirmed, with detail added (GPT-OSS ΔJ −1.24; only the GPT-OSS per-token drop is significant after correction) | 2609.38516 §4 |
| Solo-to-Social "93.2% copies an arm better than its own best" | corrected wording: copied arm has a higher mean payoff than the recipient's own best in 93.2% of *comparable* pulls | 2609.38516 §4 (RQ2) |
| Solo-to-Social IFBench "5 agents × 6 populations"; agents cannot revise decision prompts | confirmed (six independent populations of five agents); limitation is in §6 Discussion | 2609.38516 §5, §6 |
| "No paper shows a raised ceiling at matched tokens"; "social access more efficient but not more effective" | **corrected/qualified**: the bandit shows *lower* per-token efficiency; rule-based uniform copying gives a nominally significant +1.56 pp for GLM at matched tokens; SAGE reports breakthroughs for plateaued agents | 2609.38516 §4, §5 (RQ4); 2606.03544 abstract |
| GEA 71.0 vs 56.7 SWE-V, 88.3 vs 68.3 Polyglot, 71.8 OpenHands, 1.4 vs 5 iterations, 2× baseline iterations, K = 2, Haiku → Sonnet, o1 reflection | confirmed; 17 vs 9 ancestors now ✓ | 2602.04837 abstract, §4, Table 1 |
| PCE 72.1 / 96.7, +3.2 / +5.9, gap closed by one third / fully, 36% / 59% fewer tokens, 9 more of 60 tasks | confirmed | 2610.36660 abstract, §1, Table 1 |
| PCE setup "DeepSeek-V4-Flash, 2 runs per method, one backbone" | **corrected**: Polyglot uses DeepSeek-V4.1-Flash; diagnosis and distillation use DeepSeek-V4-Pro; Table 2 has two SWE-V runs but one Polyglot run | 2610.36660 §4.2, App. A, Table 2 caption |
| CollabFlow 89.15 vs 55.81 (+33.3); +2.27 / +4.37; SFT/GRPO ≤ 5.5; −13.7 / −19.5; 26 vs ≤ 15 paths; r = −0.96; 34.4 → 75.0, 15 of 20; κ = 1; 5 runs main / 1 run ablation; 5.5–9.8 / 14.3–15.6 | confirmed | 2609.38662 §4 Eq. 7, §5, Table 1–2, Fig. 4–5 |
| MELD 20 of 22; IQM 0.748 vs 0.684; gap 0.306 → 0.255; MountainCar +8.98; PD hits 2 → 10 vs 5 / 6; N = 1 loses about half of the CHAMPS margin; MNLI and Titanic values; Haiku 4.5 baselines | confirmed ("nearly half") | 2610.24454 §6, Table 1, §6.3, App. B |
| MELD "authors note no cross-task transfer" | corrected wording: the authors list task-local adaptation as a limitation and cross-task transfer as future work | 2610.24454 §7 |
| MetaShare 0.71 → 0.79, 0.28 → 0.34, 0.45 → 0.55; 1.7–2.9×; +0.11 at matched count; shuffled −0.07; tokens 0.6 / 1.2 / 2.4×; unvalidated classifier; GEA not like-for-like | confirmed | 2610.21887 §4–5, Tables 1–3, §6 |
| SAGE: Km-rollout control; strongest agent below its self-evolution ceiling; filtered traces and summaries beat raw logs; five model families | confirmed | 2606.03544 abstract, §2, §3 |
| SynCo 78.72 / 75.85 / 72.36 / 74.96; +6.66; 31.3 → 9.5%; 0.206 → 0.060; 16.2% of 2,940; no variance | confirmed | 2610.11345 Table 1, §4.2, App. D Table 5 |
| ConvRSI 75.0 vs 71.4 vs 69.6; 5.04% relative; MBPP+ 32.9 vs 32.1; 1.82×; MASS L0 → L1 +5.9, trails by 6.3 | confirmed | 2611.20891 abstract, Table 2, Table 5, §5.4 |
| ConvRSI "Mixture team: MATH-500 60.6 → 65.8" | **corrected**: this is RecursiveMAS (60.6) vs ConvRSI (65.8) on the mixture team, not a before → after change | 2611.20891 Table 4, §5.3 |
| EvoGroup 71.07 → 89.58 vs 85.83 / 82.62; +18.2 pp held-in; 6 tools; 30% held-in; DeepSeek-V4-Flash | confirmed ("5 iterations" not rechecked; reworded to "over the evolution iterations") | 2611.03868 abstract, Fig. 1, §2–3 |
| Homogenization: seed resampling and mixed models do not help; Interaction Tax: 11 tasks, within one round; AGORA: "rarely helps", up to 25 points, authority shift | confirmed | 2610.09824 abstract/§1; 2608.23541 abstract/§1; 2610.19799 abstract/§1 |
| "First" claims (Solo-to-Social first every-token test; SAGE first compute-matched; EvoMap first large A2A study) | unverifiable, so softened to "earliest in our corpus" or "an early" | n/a |
| MASS "193 alphaXiv votes" | unverifiable (not rechecked), marked † | n/a |
| Title/ID resolution: 2605.25815, 2510.23595, 2412.01928, 2609.15009 | confirmed (Behind EvoMap…; Multi-Agent Evolve…; MALT…; CoMem…); all 16 cached ids also match their stated titles | arXiv export API; cached paper.txt headers |

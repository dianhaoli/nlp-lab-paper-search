## Harness evolution with frozen models

Marks used in this chapter:
- **✓** = I checked the number in the paper's full text this session (section, table or line of the pdftotext file cited).
- **✓ᶠ** = verified in full text in an earlier session, recorded in `/home/user/nlp-lab-paper-search/research/FULLTEXT_VERIFICATION.md`.
- **◐** = taken from the abstract only (arXiv or OpenReview).
- **†** = from project notes or memory and not re-checked.

Full texts used this session are in `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/<id>/paper.txt`. The 16 new downloads were: 2604.25850, 2605.18747, 2606.14249, 2606.09498, 2607.15524, 2608.08466, 2608.07545, 2609.24972, 2610.14224, 2610.26770, 2609.37834, 2610.18813, 2610.27435, 2610.22466, 2611.07624, 2611.13524. The fact-check pass (see log at the end) re-opened 2603.28052, 2604.25850, 2607.12227, 2609.24972, 2608.07545, 2606.09498, 2606.14249, 2610.14224, 2609.37834, 2610.22466, 2605.30621, 2611.13524, 2610.27435, 2608.08466 and 2611.07624.

### At a glance
- **What this corner is.** The model weights θ stay fixed. A proposer (usually a coding agent) edits the scaffold Σ = prompts, tools, middleware, memory, skills and control code, using execution traces and verifier scores. The best Σ is kept and the loop repeats.
  - Survey labels for the same thing:
    - Ren et al. 2607.13104 call it "scaffolding improvement" (Eq. 6, θ fixed), a form of *action-level* self-reference (†).
    - Duan et al. 2609.11873 put most of it at **L2 strategy autonomy** (agent/harness search: ADAS, AFlow, Foundry Agent Optimizer) (†).
    - Chen et al. 2607.07663 file it under §3.5, "harness and agent self-evolution", the persistence class whose changes "accumulate indefinitely" (†).
    - Dai et al. (alphaXiv 2611.recursive-self-improvement-llm-agents) give "Harness" its own component. It is defined by *enforcement*: the system imposes it, rather than relying on the model to comply (†).
- **How big it is.** It is the largest self-improvement sub-lane of 2026.
  - Meta-Harness (2603.28052) had about 380 S2 citers by mid-Nov (†, round-1 sweep; S2 rate-limited during re-check).
  - An ICLR 2027 OpenReview search for "harness evolution / optimization / self-evolving harness" returns more than 70 distinct submissions. A 4-query keyword re-run during the fact-check returned 160 distinct submissions whose title or abstract mention "harness" plus evolve/optimize/self-improve/recursive, so ">70" is a safe lower bound (not every hit is a harness-evolution method).
  - Nov 1–18 alone added more than 15 new arXiv methods: Meta², HarnessFlow, BOXBOX, SIM, MULTILOOP, EvoX, DeltaHarness, JevRSI, EmoHarness, REINS and others (ids for Meta², SIM, MULTILOOP, EvoX, DeltaHarness, JevRSI resolved on arXiv this session).
- **State of the art (self-reported, same-model, in-domain).**
  - Terminal-Bench 2/2.1:
    - AHE: 69.7→77.0 (GPT-5.4) ✓.
    - DarwinX: 75.5→83.2 (GPT-5.5), and 84.7 with a stronger base (GPT-5.6 Sol) ✓.
    - RRSI: 64.6→78.7 (Gemini 3.5 Flash) ✓.
  - Largest reported jumps come from weak or mid models on agentic suites. Example: HarnessX, Qwen3.5-9B on ALFWorld, +44.0 pass@2, reported as the peak on the evolve set ✓.
- **Emerging consensus.**
  1. Gains are real, but most headline numbers are measured on the search set. Matched-compute and held-out evaluation removes a large share of them (Rethinking-Eval ✓; HDA: 54% overfitting plus 14% test-time scaling on average ✓).
  2. Gains are largest where the base model is weakest or furthest from saturation.
  3. Structural components (tools, middleware, memory) carry more of the gain than prose prompts.
  4. Accepted edits interact non-additively and accumulate complexity.
- **The live dividing line** has shifted from "can we evolve harnesses?" to two questions: what survives off the evolve set (held-out, cross-benchmark, cross-model), and whether the gain justifies its cost. Answers come from regularizers (RRSI), population or branch search (DarwinX, MoB), and audits (HDA, RSI Tax, Beneath the Surface, Auditing Harness Evolution).
- **Biggest open problem.** There is no accepted *evaluation contract*. Missing pieces:
  - a matched-compute test-time-scaling baseline;
  - a sealed held-out set that is never used for gating;
  - multi-seed lineage variance;
  - edit-level causal attribution.
  
  Without this contract, papers cannot be compared. Example: RRSI finds AHE ending *below* its seed out of distribution (agentic-workspace OOD suites, Opus 4.8) ✓, while AHE reports positive transfer (to SWE-bench Verified and to other base models on TB2) ✓. The two claims are on different axes, which is itself the comparability problem.

### How it developed

| Month (2024–26) | Paper | What changed |
|---|---|---|
| Aug 2024 | ADAS 2408.08435 (†) | Meta-agent search over agent code. This is the conceptual ancestor; it searches whole agents, not harness components. |
| 2025 | GEPA 2507.19457, ACE (†) | Reflective prompt/playbook evolution becomes the standard text-level baseline. |
| Feb 2026 | VeRO 2602.22480 (◐) | Presented by its authors as a harness *for* agents optimizing agents (Scale AI). |
| Mar 2026 | NLAH 2603.25723 (◐); **Meta-Harness 2603.28052** | "Harness" becomes the unit of optimization. Meta-Harness gives an Opus-4.6 Claude-Code proposer filesystem access to *all* prior code, scores and raw traces. The scores-only ablation is much worse (41.3 vs 56.7 best accuracy ✓ §4.1, Table 3). |
| Apr 2026 | **AHE 2604.25850** | Observability framing: file-level components, distilled evidence, and per-edit falsifiable predictions. An early full-stack coding-agent result (TB2 69.7→77.0 ✓ Table 1). |
| May 2026 | Harness Updating ≠ Benefit 2605.30621; DemoEvolve 2605.24539 (◐); Code as Agent Harness 2605.18747 (survey) | Capability decomposition of updating vs benefiting: the evolver's capability barely matters (✓ᶠ). |
| Jun 2026 | **Self-Harness 2606.09498**; **HarnessX 2606.14249** | Self-Harness: the *same* model improves its own harness (no stronger proposer). HarnessX: a typed-primitive "foundry" with the AEGIS evolution engine, and a harness↔model loop. |
| Jul 2026 | **Rethinking-Eval 2607.12227**; RHI 2607.15524; **DarwinX 2608.07545** (posted 31 Jul ✓) | The critique arrives: at matched budget, harness evolution loses to parallel sampling on TB2.1 (67.4 vs 72.3 avg over three models ✓ l.346–360). DarwinX introduces population/archive search with a preserve-and-extend contract. |
| Aug 2026 | HSI 2608.08466; HarnessOpt-Bench 2608.06301; Evo-Bench 2608.09096; Evo-Harness 2608.15071; JIT-Agent 2608.25593 (all ◐ except HSI) | Benchmarks for "LLMs as harness optimizers". Three-level hierarchy (task harness / evolver / meta-evolver). |
| Sep 2026 | **RRSI 2609.24972**; **Mixture-of-Branches 2609.37834**; RobustSGPO 2609.09646; SoL-Pi 2609.20519; LiteEvo 2609.33146; ModularRSI 2609.14857 | Overfitting becomes the main problem. RRSI regularizes proposal and selection. Branching and adaptive dev subsets appear. Search-space (permission) control appears. Cost-efficiency variants appear. |
| Oct 2026 | **Harness² 2610.14224**; HarnessPro 2610.26770; RiseHarness 2610.27435; Self-Evolving Agent Harness theory 2610.18813; **HDA 2610.22466**; Ceiling 2610.11655; OO-Harness 2610.28380; Masked Erosion 2610.38392; ARC-AGI-3 study 2610.30499 | Label-free/online evolution (Harness², HarnessPro). Continual-PAC theory. Gain attribution (HDA). Harness-vs-weights boundary (Ceiling). Safety erosion under gates. ICLR 2027 submission wave (EvoSpace, DyScope and others). |
| Nov 1–18 2026 | Meta² 2611.05139; SIM 2611.18856; BOXBOX 2611.09218; HarnessFlow 2611.06254; Beneath the Surface 2611.07624; RSI Tax 2611.13524; Failure-trace value 2611.11342; MULTILOOP, EvoX, DeltaHarness, JevRSI | Online/in-task evolution (Meta², BOXBOX). Continual RSI across benchmark/model changes, with memory *for the optimizer* (SIM). Cost audits: a bounded-note baseline matches Meta-Harness/EvoSkill/RRSI at 65.6–71.0% fewer total tokens (✓ abstract and §results l.382). |

### Taxonomy

Axes are adapted from the surveys:
- Wei et al. 2610.18813 separate *update target* from *proposal* and *admission*.
- Dai et al. split the update loop into seven functions, including Collect, Evaluate, Diagnose, Abstract and Verify (†).
- Duan et al. ask "which decisions remain external?" (†)

| Axis | Approach | Representative papers |
|---|---|---|
| **A. Who proposes** | Stronger external meta-agent (asymmetric) | Meta-Harness (Opus-4.6 proposer ✓), HarnessX (Opus-4.6 meta-agent ✓), Harness² (Opus-4.8 editor for GLM-5/Gemini executors ✓), MoB (Opus 4.6 †), RRSI (Opus 4.8 on all roles ✓) |
| | Same model plays every role ("self") | Self-Harness ✓, AHE (one GPT-5.4 for all three roles ✓), HSI (one frozen DeepSeek-V4-Flash-Preview ✓), FreeEvolve 2610.09197 |
| | Cheap/weak evolver | Harness Updating ≠ Benefit (Qwen3.5-9B updates ≈ Opus-4.6 on average ✓ᶠ), LiteEvo (frozen Qwen3.5-9B ◐) |
| **B. Edit surface** | Text artefacts only (prompt, playbook, notes, skills) | GEPA, ACE, Evo-Harness, Failure-trace notes 2611.11342 |
| | Full harness code (tools, middleware, memory, hooks) | Meta-Harness, AHE, DarwinX, RRSI, HarnessX, RiseHarness, OO-Harness |
| | Adaptive or controlled scope | RobustSGPO (3-rung permission ladder ✓ᶠ), EvoSpace (OpenReview MQoRwlss2y ◐), DyScope (MXINrso5QC ◐) |
| **C. Search structure** | Single lineage, keep-if-better | AHE, Self-Harness, HSI, RHI |
| | Population/archive + recombination | DarwinX, HarnessX "variant isolation", HGM-Dual 2610.30042 (†) |
| | Branches/specialists + router | MoB, OO-Harness (subclass hierarchy + router), HarnessWeaver (◐) |
| | Tree search / bandit / statistical design | HarnessFlow (MCTS ◐), MESH-Harness 2610.05300 (bandit †), HOP (semiparametric, provable selection ◐) |
| | Multi-objective / cost | HarnessPro (lite/medium/heavy Pareto), Harmony Harness 2610.15150, HarnessMM 2610.32813, SoL-Pi, LiteEvo |
| **D. When and with what signal** | Offline, ground-truth verifier on a dev set | Most papers |
| | Online / within-task / label-free | Harness² (contrastive Propose–Probe–Compose, no verifier), HarnessPro (LLM-generated tests), Meta² (offline-learned meta-skill, applied online ◐), BOXBOX (in-run hot reload ◐) |
| | Continual across benchmarks/models | SIM 2611.18856 (◐), Harness Continual Learning 2608.19013 (◐), EvoHarnessBench (◐) |
| **E. Admission and regularization** | Append-all + final Pareto pick | Meta-Harness (✓ᶠ) |
| | Prediction contract + file-level revert | AHE |
| | Two-split no-regression gate | Self-Harness (✓ §3.4) |
| | Preserve-and-extend + independent confirmation | DarwinX |
| | L1-style edit budget, leakage critic, pruner | RRSI |
| | Statistical / false-evolution control | TRUST (wV4bK1dHpC ◐), HOP (◐), Masked Erosion (◐ critique) |
| **F. Recursion depth** (Duan's "where does the loop close") | Order-1 (evolver fixed) | Most papers |
| | Order-2 (the improver is revised) | Harness² (continual improvement harness), HSI (meta-evolver under a frozen anchor), MoB (proposal policy rewritten per branch), SIM (optimizer memory), RSI-Harness (EOrtreQLbv ◐), ReHarness (Gzda5HNz7Y ◐) |
| **G. Evaluation and critique** | Matched compute, held-out, attribution, cost | Rethinking-Eval, HDA, Beneath the Surface, RSI Tax, Elicitation-or-Expansion (bSBejKazyx ◐), Opinionated Optimizers (LhhLpXBFgo ◐), Auditing Harness Evolution (yl1XFtnTn4 ◐), RSIBench (Q9AHmuAk4Y ◐), Feedback Alignment (C6EElQ2PRG ◐) |

### Paper cards

**1. Meta-Harness: End-to-End Optimization of Model Harnesses** ([2603.28052](https://arxiv.org/abs/2603.28052), 30 Mar 2026 ✓; COLM 2026 per project notes †). Stanford/MIT/KRAFTON (Lee, Khattab, Finn) ✓.
- *What it does.* An agentic proposer (Claude Code + Opus-4.6) reads a filesystem of every prior harness's code, scores and raw traces, and writes new harness code. Every valid candidate is appended, and a Pareto front is picked at the end (✓ᶠ).
- *Setup.*
  - Text classification; retrieval-augmented IMO-level math (200 problems, 5 held-out models ✓ §4.2); TerminalBench-2 with Opus 4.6 and Haiku 4.5 ✓.
  - About 10M diagnostic tokens per iteration (†). Single runs.
- *Key results.*
  - Text classification: 48.6%, beating ACE by 7.7 pts with 4× fewer context tokens ✓ (§4.1).
  - IMO-level: +4.7 average over 5 held-out models ✓ (§4.2, table at l.484).
  - TB2: 76.4% on Opus 4.6 (above Terminus-KIRA 74.7; #2 behind ForgeCode 81.8); 37.6% on Haiku 4.5 (#1 among Haiku 4.5 agents) ✓ (§4.3).
  - Interface ablation: scores-only 34.6 median / 41.3 best vs full traces 50.0 / 56.7 ✓ (§4.1, Table 3).
- *Limitations / critiques.*
  - TB2 searched and reported on the same tasks ✓ᶠ.
  - The math search set is reported as both 250 and 88 (internal contradiction ✓ᶠ).
  - The proposer's skill text was hand-tuned; the paper says it mattered more than budget ✓ᶠ.
  - No keep-if-better gate ✓ᶠ.
  - RRSI: strongest baseline on the evolve split (Harvey LAB 93.0), but only +0.9 on the OOD average ✓ (2609.24972 §4, Table 1).
  - MoB beats it by 34.8% relative on Math-Gemini (46.0→62.0) ✓.
  - RSI Tax: a bounded-note baseline matches or exceeds it at 65.6% lower mean total tokens ✓ (2611.13524 l.382).

**2. Agentic Harness Engineering (AHE)** ([2604.25850](https://arxiv.org/abs/2604.25850), 28 Apr 2026 (v4 18 May ✓); preprint). Fudan/PKU/Shanghai Qiji Zhifeng ✓.
- *What it does.* Three "observability pillars":
  - seven editable component types exposed as files;
  - a layered evidence corpus distilled from trajectories;
  - a change manifest where each edit declares the tasks it should fix or break, checked against the next round and reverted at file granularity.
- *Setup.*
  - GPT-5.4 (high) plays code agent, debugger and evolver ✓.
  - Evolved on all 89 TB2 tasks (4 easy / 55 medium / 30 hard; per-task timeout extended to 1 h); one 10-iteration campaign of about 32 h ✓ §4.1.
  - Transfer tested on SWE-bench Verified (500) and 5 alternate bases (GPT-5.4 medium/xhigh, qwen-3.6-plus, gemini-3.1-flash-lite, deepseek-v4-flash) ✓ §4.1–4.2.
- *Key results.*
  - TB2 pass@1 69.7→77.0, vs Codex 71.9, TF-GRPO 72.3, ACE 68.9 ✓ (Table 1).
  - SWE-bench Verified 75.2→75.6 with 12% fewer tokens (526k→461k); ACE (74.6) and TF-GRPO (74.2) fall below seed ✓ (Table 2).
  - Cross-model transfer +2.3 to +10.1 pp; deepseek-v4-flash 51.7→61.8 ✓ (Fig. 3).
  - Component swaps ✓ (Table 3):
    - memory-only 75.3, tool-only 73.0, middleware-only 71.9;
    - prompt-only 67.4, which is a regression (−2.3).
  - The *three positive* single-component gains sum to +11.1 vs full +7.3, so components interact non-additively ✓ (§4.3 text). (Including the −2.3 prompt-only row the four sum to +8.8.)
  - Self-prediction ✓ (Fig. 4):
    - fix precision/recall 33.7/51.4 (random baseline 6.5/10.6);
    - regression 11.8/11.1 (random 5.6/5.4), i.e. "regression blindness".
- *Limitations / critiques.*
  - Evolved and reported on the same 89 tasks; single campaign; no seeds ✓.
  - Step budget and timeout were fitted to GPT-5.4 high, so the within-family transfer profile is non-monotone (+2.3 medium, +7.3 high, +2.3 xhigh) ✓ (§4.2).
  - RRSI re-runs AHE from a shared H0: it finishes *below* the initial harness out of distribution ✓ (2609.24972 §4).
  - SelfImprove-Bench reports AHE at −179% HCI on one cell (†).

**3. Rethinking the Evaluation of Harness Evolution for Agents** ([2607.12227](https://arxiv.org/abs/2607.12227), Jul 2026 (v4 5 Oct ✓); ICLR27 sub [QDdWiaNkk2](https://openreview.net/forum?id=QDdWiaNkk2) ✓ title matches). AI2/UW ✓.
- *What it does.* Compares harness evolution with test-time-scaling and discovery baselines at matched feedback and inference budgets, plus held-out evaluation.
- *Setup.* TB2.1 (three models), ARC-AGI-3, EdgeBench games ✓.
- *Key results.*
  - TB2.1 average over three models ✓ (l.346–360):
    - harness evolution 67.4;
    - parallel sampling 72.3;
    - direct sampling 68.2;
    - (sequential refinement 69.3; Harness Scaling 71.8).
  - Held-out gain about +0.0 ✓ᶠ.
  - In games, **task-specific** harness evolution ("Harness Scaling") beats fixed-harness *sequential refinement* by 80% on ARC-AGI-3 and 11% on EdgeBench ✓ (abstract l.41, §1 l.90, §5 l.589). This is per-task Harness Scaling, not reusable cross-task evolution, and the comparison is against Sequential Refinement only (✓ᶠ).
- *Limitations.*
  - The cost of modifying the harness is not counted ✓ᶠ.
  - Game evaluation uses best-over-run with no held-out split ✓ᶠ.
- *Why it matters.* It set the matched-budget baseline that later papers now partly adopt (Harness² ✓, BOXBOX ◐, Opinionated Optimizers ◐).

**4. RRSI: Regularized Recursive Self-Improvement of Agent Harnesses** ([2609.24972](https://arxiv.org/abs/2609.24972), 21 Sep 2026 (v3 4 Oct ✓); ICLR27 sub [75PPLZgaUM](https://openreview.net/forum?id=75PPLZgaUM) ✓ title matches). Google Cloud AI Research/UNC/Stanford/WashU ✓.
- *What it does.* Regularizes both sides of the loop ✓ (abstract).
  - Proposer: a temporally annealed edit budget (an L1-style cap on bundled edits) and novelty pressure from history.
  - Selector: a leakage critic that screens benchmark-specific proposals, and a pruner that removes edits that are too small, too costly, or no longer contributing.
- *Setup.*
  - Opus 4.8 as frozen policy, proposer, analyst and critic ✓ (l.417–419).
  - Evolve suites: TB2.1 (89), Harvey LAB (120 evolve / 40 ID held-out †), EngDesign (61 †).
  - OOD suites: SWE-bench Verified, JobBench, GDPval, APEX-Agents, Frontier-Eng ✓ (Table 7).
  - 4 baselines (Meta-Harness, AHE, TTHE, HarnessX) from the same H0 with the same candidate budget ✓.
  - Paired task bootstrap (10,000 resamples) and 2 independent search repetitions ✓ (App. Tables 6–7).
- *Key results.*
  - Evolve gains: +6.0 TB2.1, +4.9 EngDesign, +1.1 Harvey ✓ (Table 6).
  - Agentic-workspace OOD average 39.7→43.6 (run 2: 43.7) ✓ (Tables 1, 7).
  - Same OOD average for baselines: Meta-Harness 93.0 on Harvey LAB evolve but only +0.9 OOD; HarnessX lands on the base; AHE and TTHE end below H0 (TTHE by 1.7) ✓ (§4 text, Table 1).
  - Ablation ✓ (Table 2): unregularized evolution scores 92.8 on evolve but only 40.3 OOD, at 3.80M vs 2.42M tokens/trial.
  - Gemini 3.5 Flash: TB2.1 64.6→78.7; transfer to unseen 3.1 Flash Lite 11.2→14.6 ✓ (Tables 3–4).
  - Run-to-run disagreement ≤1.8 pts (on TB2.1; ≤0.6 elsewhere) ✓.
- *Limitations.*
  - No matched test-time-scaling baseline.
  - Hyperparameters are per setting (†).
  - The baselines were run once each (Table 7 repeats only H0/RRSI) ✓.
  - Judge-graded OOD suites (partly addressed by simulator-graded EngDesign).
  - RSI Tax lists RRSI among the methods that a bounded-note baseline matches; verification is 93.6% of RRSI's RSI Tax ✓.

**5. DarwinX: Evolving Agent Harnesses Through Natural Selection** ([2608.07545](https://arxiv.org/abs/2608.07545), 31 Jul 2026 ✓; ICLR27 sub [XVvUgp3Bu8](https://openreview.net/forum?id=XVvUgp3Bu8) ✓ title matches). Salesforce AI Research ✓.
- *What it does.* Population search with the model frozen ✓ (abstract).
  - A preserve-and-extend contract: a variant must extend coverage without regressing.
  - An archive of alternative lineages for recombination.
  - Failure-, teacher- and self-derived evidence share one edit interface.
  - Fitness comes only from benchmark verifiers.
- *Setup.* A ladder of four regimes ✓ §3:
  - TB2.1 in-domain (GPT-5.5; strict leaderboard rule, avg@5, errored trials = 0);
  - TerminalWorld train / 41 held-out (Opus 4.8; train-split size †);
  - WebArena-Infinity: 300 synthetic intents (LLM judge, avg@3 screen, avg@5 confirm) → 1,260 real tasks;
  - TB2.1 → SWE-bench Verified transfer.
- *Key results.*
  - TB2.1: base Monet 75.5±3.5 → 83.2±1.2 (GPT-5.5); 84.7±1.2 on GPT-5.6 Sol medium. +5.2 over a neutral harness (Terminus-2 at xhigh, 78.0) on the same GPT-5.5 base ✓ (§4, Table 2).
  - TerminalWorld held-out (Opus 4.8): 61.0→68.3 (25/41 vs 28/41); the matched GPT-5.5 pair moves 20→23 ✓ §5. The authors call the Opus comparison suggestive (McNemar p=0.45) ✓ §9, and the one-task margin over Claude Code p=1.0 ✓ §5.
  - In-loop training-subset score saturates 0.505→1.000 while held-out is 68.3 (a 31.7-pt proxy gap) ✓ §5.
  - WAI 43.5→93.0 audit-clean (548→1,171 of 1,260) ✓ §6.
  - SWE-V 84.2% (421/500), +3.4 over the 80.8% LSP fix-skill reference, with no SWE-V feedback ✓ §7.
- *Limitations.*
  - The authors say the matched-model deltas support only a *system-level* claim; archive, parent selector, recombination and effort are not independently ablated ✓ §9.
  - The GPT-5.5 TerminalWorld run reached 56.1%, *below* Terminus-2's 61.0, so Opus was reported as the headline ✓ (§5 l.671). This is a reporting-selection concern.
  - The base row runs at "default" effort while evolved rows run at high or medium ✓ (Table 2); the authors partly address this with the Terminus-2-xhigh control (78.0) ✓.

**6. Self-Harness: Harnesses That Improve Themselves** ([2606.09498](https://arxiv.org/abs/2606.09498), 8 Jun 2026 (v3 20 Aug ✓); preprint, 502 alphaXiv votes †). Shanghai AI Lab ✓.
- *What it does.* The target model improves its own harness, with no stronger external agent. The loop is ✓ §3:
  1. Weakness Mining: cluster failed traces.
  2. Proposal: minimal edits tied to each failure mechanism.
  3. Validation: accept an edit only if it improves one split without degrading the other; merge all accepted edits.
- *Setup.*
  - Minimal DeepAgent seed harness ✓.
  - Models: MiniMax M2.5, Qwen3.5-35B-A3B, GLM-5 ✓.
  - Benchmarks: TB2.0 (a 64-case subset), SWE-bench Verified (a 100-case subset, 67/33 split), AppWorld (180 examples, 90/90) ✓ App.
  - 2 attempts per candidate ✓ §4.1.
- *Key results.* All 9 model×benchmark cells improve on both splits ✓ (Table 1):
  - Qwen3.5-35B TB2 overall 18.0→36.7;
  - GLM-5 AppWorld 44.4→85.0 (largest absolute gain, +40.6);
  - MiniMax SWE-V 46.0→52.5.
- *Limitations / critique (ours, from §3.4 ✓).* The "held-out" split is used as a **regression gate** in acceptance, so it is not a sealed test set. Its gains are selection-biased in the same way Rethinking-Eval and HDA warn about. There are no seeds and no matched-compute baseline. Evaluation uses subsets of TB2 and SWE-V. ReHarness (ICLR27 ◐) claims to beat it at an equal data budget.

**7. HarnessX: A Composable, Adaptive, and Evolvable Agent Harness Foundry** ([2606.14249](https://arxiv.org/abs/2606.14249), 12 Jun 2026 (v3 23 Jul ✓); preprint, "Darwin Agent Team" ✓).
- *What it does.* Builds harnesses from typed primitives (a substitution algebra over nine behavioral dimensions ✓). The AEGIS multi-agent engine evolves them, with "variant isolation" (up to K variants). Trajectories also feed model fine-tuning.
- *Setup.*
  - Opus 4.6 meta-agent ✓.
  - Task agents: Sonnet 4.6, GPT-5.4, Qwen3.5-9B ✓.
  - Benchmarks: ALFWorld, GAIA-103, WebShop, τ³-Bench (3 domains), SWE-V **55-task subsample** ✓.
  - Up to 15 rounds; pass@2 ✓ §6.1.
- *Key results.*
  - Average +14.5 over 15 configurations (14 of 15 improve), maximum +44.0 (Qwen3.5-9B ALFWorld 53.0→97.0) ✓ (Table 4).
  - GAIA GPT-5.4 Δ=0.0 (the stagnating configuration) ✓.
  - τ³ Telecom (Sonnet 4.6) fell −14.0 at R7 (94.7→80.7, "fix-one-break-one treadmill"), recovering by R9 ✓.
  - Co-evolution adds +4.7 on average over two benchmarks (+4.3, and WebShop +5.0) ✓ (§6.5).
- *Limitations.* Stated by the authors ✓ §7.7: no held-out evaluation, peak accuracy reported on the adaptation set ("selection bias and potential overfitting"), closed-source meta-agent.
  - LiteEvo reports comparable or better pass@2 at much lower API cost, and notes HarnessX budgets 100–175M meta-agent tokens per benchmark ◐.
  - RRSI: HarnessX "lands on the base" out of distribution ✓.

**8. Harness²: Recursive Agent Harnessing for an Open World** ([2610.14224](https://arxiv.org/abs/2610.14224), 9 Oct 2026 ✓; ICLR27 sub [s3UU3VSGcB](https://openreview.net/forum?id=s3UU3VSGcB) ✓ title matches; a different submission, MUAJguyvAD, uses the same name †). Northwestern/Google Cloud AI Research ✓.
- *What it does.* Two levels of recursion with **no ground-truth verifier at test time**.
  - Task level: a contrastive Propose–Probe–Compose step refines the harness for the current task.
  - Harnessing level: experience from those edits updates a persistent "continual improvement harness", an indexed library carried along the task stream.
- *Setup.*
  - Frozen executors: Gemini 3.5 Flash and GLM-5, under OpenCode or Codex ✓.
  - Editor: Opus 4.8 ✓ (l.428).
  - Benchmarks: WorkBuddy-Bench, JobBench, Harvey LAB.
  - Baselines: ReasoningBank, offline Meta-Harness (optimized on separate training tasks), Harness Scaling. SDs over runs ✓ (Table 1).
- *Key results.*
  - WorkBuddy, GLM-5/OpenCode: 64.2→75.7 (k=1), with Meta-Harness at 69.7 ✓ (Table 1).
  - Gemini/OpenCode: 72.4→77.5, vs Meta-Harness 76.4 ✓.
  - Harness Scaling falls below base in 7/12 benchmark–configuration pairs ✓ (l.491).
  - Meta-Harness wins LAB Partial for Gemini/Codex (†, not re-located this pass).
- *Limitations.*
  - The editor is stronger than the executors (asymmetric).
  - Label-free signals are sensitive to execution stochasticity and shared model biases (✓ l.289; App. F).
  - Budget for test-time refinement is not equated with Harness Scaling in the main table.
  - Order-2 credit is not isolated: the library effect and per-task refinement are confounded.

**9. Mixture of Self-Improving Branches (MoB)** ([2609.37834](https://arxiv.org/abs/2609.37834), 29 Sep 2026 ✓; ICLR27 sub [hRSpuaLKkC](https://openreview.net/forum?id=hRSpuaLKkC) ✓ title matches). Meta/Duke ✓ (work done at Meta).
- *What it does.* B=2 branches search in parallel ✓.
  - Each branch keeps the dev cases it "owns", drops cases every branch solves, and rewrites its own proposal guidance (SKILL.md) every 5 iterations (†).
  - A router picks a branch head per input using dev data only.
- *Setup.*
  - Proposer: Opus 4.6 (†).
  - Math (250 dev / 200 IMO-level test; Gemini 3 Flash and Sonnet 4.5) ✓.
  - TB2.0: 30 dev / 58 test (Sonnet 4.5) ✓.
  - SWE-bench Lite: 50/250 ✓.
  - N=20 iterations, frontier q=5 ✓ §4.1.
- *Key results.* Held-out accuracy ✓ (Table 1):
  - Math-Gemini: 46.0 (Meta-Harness) → 62.0 (+34.8% relative);
  - TB2.0: 44.8 → 50.0;
  - SWE-Lite: 63.6 → 66.0.
  - The dev-subset update interval matters: updating every 1/3/5 iterations gives 62.0/56.0/53.0 ✓ (Table 3).
- *Limitations.*
  - Single runs.
  - B=2 branches × 1 candidate per iteration; the paper does not clearly say whether Meta-Harness got the same 40 candidates (†, unclear).
  - The router leaves 4.4–5.5 pts of combined coverage unrealized ✓ (§4.4).
  - The gain may partly be "routing", not harness improvement. This is relevant to our Order-0-rewards-breed-routers idea.

**10. Harness-Delta Attribution (HDA)** ([2610.22466](https://arxiv.org/abs/2610.22466), 15 Oct 2026 ✓; ICLR27 sub [PX4Zpvsaca](https://openreview.net/forum?id=PX4Zpvsaca) ✓ title matches). Amazon/NYU ✓.
- *What it does.* Splits a search-set gain into three parts: artifact-driven overfitting, test-time-scaling-explained compute, and a residual upper bound.
- *Setup.* 11 executors (Qwen 0.8B–35B and Claude) × 3 frameworks; math, coding, creativity, agentic ✓.
- *Key results.*
  - Across 21 significant settings: overfitting (at least) 54% and test-time scaling 14% of the gain on average; combined share >50% in 14 settings ✓ (abstract; §1 l.121).
  - ALFWorld harnesses for 0.8B/2B executors became LLM-free controllers (†, round-1 deep read). Re-check found that the Meta-Harness ALFWorld harness for the 0.8B executor grew from 66 to 503 lines and its gain is attributed 100% to overfitting ✓ (l.227, l.291, l.376).
  - Validation-based selection does not prevent this ✓.
- *Limitations.* Overfitting detection is agent- and human-audited, so it is a lower bound. Single runs (†).

**11. Harness Updating Is Not Harness Benefit** ([2605.30621](https://arxiv.org/abs/2605.30621), May 2026 (v2 4 Nov ✓); preprint). PSU/UCSC/Amazon (plus Emory, UIUC, Northeastern) ✓.
- *What it does.* Separates the capability to *write* useful updates from the capability to *benefit* from them.
- *Key results.*
  - Updating is flat in base capability: the evolver spread is ≤3.1 pp on average, up to 9.3 per cell, and Qwen3.5-9B updates ≈ Opus-4.6 on average (2.6 pp worse on MCP) ✓ᶠ.
  - Benefit is non-monotone: mid-tier models gain most ✓ᶠ.
  - Weak models fail to activate artefacts, or activate them but do not follow them ◐.
- *Limitations.* No seeds. Δbenefit uses max while Δupdate uses mean ✓ᶠ. Every update is applied (no gate) ✓ᶠ.
- *Tension.* It conflicts with the inverse scaling reported by HarnessX and Harness², where the weakest models gain most.

**12. RobustSGPO + EvoSpace + DyScope (search-space control cluster)**
- RobustSGPO: [2609.09646](https://arxiv.org/abs/2609.09646), Sep 2026, ✓ᶠ.
- EvoSpace: [MQoRwlss2y](https://openreview.net/forum?id=MQoRwlss2y), ICLR27, ◐.
- DyScope: [MXINrso5QC](https://openreview.net/forum?id=MXINrso5QC), ICLR27, ◐.
- *What they do.*
  - RobustSGPO fixes a 3-rung permission ladder: edit one agent, edit any agent, or add/remove agents and change routing.
  - EvoSpace and DyScope widen or narrow the editable scope adaptively, based on diagnosis.
- *Key results.*
  - RobustSGPO: fixed rungs tie (α2 4.07 vs α3 4.06, 5 seeds, no error bars); the 1→2→3 schedule reaches 4.34 ✓ᶠ.
  - EvoSpace: +8.62% / +13.71% relative over EvolveMem on LoCoMo ◐.
  - DyScope states the reachability-vs-attribution tension explicitly ◐.
- *Limitations.* RobustSGPO is matched on rounds, not tokens, and the model is unnamed ✓ᶠ. EvoSpace covers memory QA only.

**13. The RSI Tax** ([2611.13524](https://arxiv.org/abs/2611.13524), 10 Nov 2026 ✓; preprint). PKU ✓.
- *What it does.* Continual-stream evaluation (score each task before its feedback is used) with the token cost of the improvement process included ✓.
- *Key results.*
  - Across six methods and two models, a bounded-note baseline matches or exceeds Meta-Harness, EvoSkill and RRSI at mean total-token savings of 65.6% / 71.0% / (third value within range) ✓ (abstract; l.382).
  - Verification is 74.8% (Meta-Harness), 78.1% (EvoSkill) and 93.6% (RRSI) of the RSI Tax ✓ (l.422).
  - A substantial share of the tax is associated with state or artifacts never used later in the streams ✓.
- *Limitations.* Two models ✓. Streams are short-horizon (†).

**14. RiseHarness: Harness Self-Evolution for Long-Horizon Terminal Agents** ([2610.27435](https://arxiv.org/abs/2610.27435), 20 Oct 2026 ✓; preprint). Tencent HY (with Indiana, UMD, UGA, NUS) ✓.
- *What it does.* Targets "flipping" tasks (both successes and failures under one config). Uses a runtime monitor and a "Harness Handbook" to map evidence to source code, and imports mechanisms from other harness frameworks.
- *Setup.* GPT-6-sol and DeepSeek-V4-Flash; TBH and TB2.1; r=4 repeats ✓ §4.
- *Key results.*
  - GPT-6-sol Pass@1 +5.25 (TBH) / +3.09 (TB2.1) over Terminus-2 ✓ (Table 1).
  - DeepSeek TBH Pass@1 +7.25 and Pass^4 +14 ✓.
  - Swapping final harnesses across models reduces gains; the retained changes are model-specific ✓ (Fig. 7).
- *Limitations.* 2 models. Gains are reliability-type (Pass@4 rises only modestly, ✓ l.519) ("elicitation" in the sense of Elicitation-or-Expansion), not expansion.

**Remaining papers (compact)**

| id | title | one-liner | mark |
|---|---|---|---|
| [2606.09498→]ReHarness [Gzda5HNz7Y](https://openreview.net/forum?id=Gzda5HNz7Y) | ReHarness: Learning Reusable Harness Evolution Across Tasks | Learns a transferable *evolution strategy* across tasks; beats Self-Harness at equal data on 2 tasks; 4 LLMs × 6 benchmarks | ◐ (title ✓ via OpenReview search) |
| [2607.15524](https://arxiv.org/abs/2607.15524) | Recursive Harness Self-Improvement (Sakana/Berkeley) | Prompt-level harness spec refined by pairwise feedback over its own revision history. Few iterations make high-effort Sonnet/Opus beat max-effort settings at up to 60% lower cost on 30 synthetic ML-research tasks | ◐ |
| [2608.08466](https://arxiv.org/abs/2608.08466) | Hierarchical Self-Improvement (HSI) (single author, HKUST) | One frozen model (DeepSeek-V4-Flash-Preview) across task harness / evolver / meta-evolver (frozen outer anchor). BALROG: BabyAI +39.3, Crafter +33.0; no gain on NLE (capability bound) | ✓ abstract + §1 |
| [2610.26770](https://arxiv.org/abs/2610.26770) | HarnessPro | Label-free (LLM-generated tests) Pareto evolution with lite/medium/heavy outputs. Heavy: +2.0 pp pass rate and −26.1% cost vs RHO (GPT-5.5/Codex) | ✓ abstract |
| [2610.18813](https://arxiv.org/abs/2610.18813) | Self-evolving Agent Harness (theory/survey, 66 pp.) | Continual-PAC: generalization bound, retention bound under regression budgets, high-probability admission with fresh validation data | ✓ abstract |
| [2605.18747](https://arxiv.org/abs/2605.18747) | Code as Agent Harness (survey) | Interface → mechanisms → multi-agent taxonomy; names "regression-free harness improvement" and "evaluation beyond final success" as open | ✓ abstract |
| [2611.18856](https://arxiv.org/abs/2611.18856) | Self-Improvement Memory Enables Continual Recursive Self-Improvement (Google †) | Memory *for the optimizer* (edit rationale, predicted vs observed outcomes) when continuing across benchmarks or policy models. +2.36 vs harness-only continuation over 10 source runs in coding, p=0.003 | ◐ (title/abstract re-checked) |
| [2611.05139](https://arxiv.org/abs/2611.05139) | Meta²: Precomputing Reusable Knowledge for Online Harness Evolution (UMD †) | Learns a textual Meta-Skill offline by contrasting no-evolver, evolver and privileged-verifier rollouts; applies it online per task | ◐ (title ✓) |
| [2611.09218](https://arxiv.org/abs/2611.09218) | BOXBOX | In-run harness revision with hot reload on PerfRace. At matched test-time budget vs Frozen Harness, offline Harness Evolution and Harness Scaling: Speedup@4h 1.70× vs 1.54× best baseline | ◐ (re-checked) |
| [2611.06254](https://arxiv.org/abs/2611.06254) | HarnessFlow | MCTS over a refinement tree, 15 attempts per benchmark; 69.48% mean over 6 benchmarks, +10.89 pp over initial harnesses and +3.95 pp over Meta-Harness | ◐ (re-checked) |
| [2610.28380](https://arxiv.org/abs/2610.28380) | Object-Oriented Harness (UIUC/Google) | Evolves a subclass hierarchy of specialized harnesses plus a router; up to +15 absolute | ◐ |
| [2609.33146](https://arxiv.org/abs/2609.33146) | LiteEvo | Tool-free meta-agents and a component library. Frozen Qwen3.5-9B: +10.5–67.7 pp pass@2; matches a HarnessX reproduction (71.0 vs 67.3) at lower cost; gains kept on unseen tests | ◐ |
| [2609.20519](https://arxiv.org/abs/2609.20519) | SoL-Pi (NVIDIA) | Auto-research loops over many environments; 4 surviving mechanisms cut tokens 44.7–49.0% at parity on EdgeBench | ◐ |
| [2611.15754](https://arxiv.org/abs/2611.15754) | MULTILOOP: Evolving Multi-Agent Harnesses for Complex Information-Seeking Tasks | Multi-agent harness evolution on DeepSearchQA (10 training samples): +4.6 vs base, but orchestration failures largely persist | ◐ (title ✓) |
| [2611.15006](https://arxiv.org/abs/2611.15006) | EvoX: Rethinking the Agent Harness as a Runtime Policy | Harness as an "operational policy" runtime; Contrast fairness framework; 56.2% over 691 task-model pairs | ◐ (title ✓) |
| [2611.21688](https://arxiv.org/abs/2611.21688) / [2611.22432](https://arxiv.org/abs/2611.22432) | DeltaHarness / JevRSI ("Think Once, Act Fast: Cost-Effective RSI for Decision Model Harness") | Latency/resource-adaptive tool dispatch; System-1/System-2 collaboration where an asynchronous System-2 LLM recursively improves a modular harness for a fast System-1 policy | ◐ (titles/abstract ✓) |
| [2610.11655](https://arxiv.org/abs/2610.11655) | Harness Evolution Hits a Ceiling: When Weight Training Should Begin | Harness fixes *process* failures and weights fix *content* failures. DeepPlanning held-out: 4B 0.16→0.30, 9B 0.32→0.44 | ◐ (title ✓; deep-read in round 1 †) |
| [2611.11342](https://arxiv.org/abs/2611.11342) | Frozen weights: value of failure traces | Failure-trace notes +4.0 pp over a matched search without them; about 60% of the gain comes from try-and-keep alone; no gain in full conversations | ◐ |
| [2611.07624](https://arxiv.org/abs/2611.07624) | Beneath the Surface: What We Overlook When We Talk About Agent Evolution | 4 methods × 2 harnesses: all 16 evolved agents both gain and lose tasks; the highest-scoring agent is not the one yielding the largest mean improvement (best parent) in any of the 4 pools; transfer is mixed (coverage usually falls on the general-task target, rises on coding with the outer-layer-only harness) | ✓ abstract |
| [2610.30499](https://arxiv.org/abs/2610.30499) | Can LLMs Improve Their Own Harness? (ARC-AGI-3) | 27 lineages; build plus 2 full revisions; scored on 20 sealed games | ◐ |
| [2610.38392](https://arxiv.org/abs/2610.38392) | Masked Erosion | 411 loops under 10 gate rules: gates silently delete redundant safeguards; 40-task batches lost all six | ◐ |
| [2610.09197](https://arxiv.org/abs/2610.09197) / [2610.02616](https://arxiv.org/abs/2610.02616) | FreeEvolve / VERSE | Self-evolving harness optimizers. FreeEvolve: Haiku 52.2 evolve vs 32.2 held-out. VERSE: 49% of validation flips do not reproduce | ✓ᶠ |
| [2610.00917](https://arxiv.org/abs/2610.00917) | Finding the Right Fit | 66 model×harness configurations; rankings reverse across harnesses | † |
| [2608.06301](https://arxiv.org/abs/2608.06301) / [2608.09096](https://arxiv.org/abs/2608.09096) | HarnessOpt-Bench / Evo-Bench | Benchmarks of LLMs *as* harness optimizers; sealed test partition enforced by a TEE (HarnessOpt) | ◐ |
| [2608.23552](https://arxiv.org/abs/2608.23552), [2609.01481](https://arxiv.org/abs/2609.01481) | Prime Agent; Harness-of-Harness | Self-improving open RLM harness; multi-day software development with continual improvement | ◐ |
| [Q9AHmuAk4Y](https://openreview.net/forum?id=Q9AHmuAk4Y) | RSIBench | Agents revise their own Pi harness; 7 models, 100 tasks. Opus 5 held-out +8.3 (36.4→44.7); GPT-5.5 −8.3 held-out on Science | ◐ |
| [LhhLpXBFgo](https://openreview.net/forum?id=LhhLpXBFgo) | Opinionated Optimizers | MOT: 3M rollouts, 216 runs. Overfitting comes from proposer priors, noisy evaluation and non-compounding selection; 9 failure modes | ◐ |
| [yl1XFtnTn4](https://openreview.net/forum?id=yl1XFtnTn4) | Auditing Harness Evolution | Lineages show no advantage over budget-matched single-edit selection; an accepted guard never fires in 3,960 checks | ◐ |
| [bSBejKazyx](https://openreview.net/forum?id=bSBejKazyx) | Elicitation or Expansion? | pass@k: the harness's pass@1 advantage over stronger interventions reverses at larger k | ◐ |
| [9MEy7m0jWc](https://openreview.net/forum?id=9MEy7m0jWc) | Harness Training: A Post-Training View of Agent Harness Evolution (Harness-SFT / Harness-RL) | Reframes harness evolution by learning objective; STHT reaches thresholds with fewer rollouts | ◐ (title ✓) |
| [hI2fAYvROC](https://openreview.net/forum?id=hI2fAYvROC) | Compositional Safety Failures in Harness Evolution | Individually safe edits compose into unsafe behavior: 43 pairwise and 18 irreducible 3-way failures; hypergraph monitor | ◐ |
| [tNNCBiSaC9](https://openreview.net/forum?id=tNNCBiSaC9) | EvoHarnessBench | Non-stationarity in the *harness* (17 streams, 802 tasks); "harness-induced forgetting" | ◐ |
| [cF89G2mfyg](https://openreview.net/forum?id=cF89G2mfyg) | HOP | Semiparametric configuration × task model; estimates an exponential configuration space from linearly many tests; consistent optimal selection | ◐ |
| [C6EElQ2PRG](https://openreview.net/forum?id=C6EElQ2PRG) / [5lWjRTK6Vk](https://openreview.net/forum?id=5lWjRTK6Vk) | Feedback Alignment / Harness Learns to Lie | Scaffold evolution raises the visible score while lowering the held-out target; false success rises each round | ◐ |
| [EOrtreQLbv](https://openreview.net/forum?id=EOrtreQLbv) | RSI-Harness | Dual loop; the improver's gain grows 1.3→6.2; the improver transfers (up to 4.5×) while the harness does not | ◐ |
| [XoMy9wRqF1](https://openreview.net/forum?id=XoMy9wRqF1) / [zn5R6mJV8i](https://openreview.net/forum?id=zn5R6mJV8i) | Plug, Play, or Re-Evaluate / Evolving to Match Capabilities | 84.3% of 1,120 component removals change nothing; skills that help a strong model (+31.1) hurt a weak one (−8.8) | ◐ |

### Consensus, debates & contradictions
- **Consensus.**
  1. Same-set reporting inflates gains.
     - HarnessX states it outright ✓ (§7.7).
     - AHE and Meta-Harness on TB2 were searched on the evaluation tasks ✓/✓ᶠ.
     - HDA attributes 54% of gain to overfitting ✓.
     - DarwinX's own TerminalWorld proxy saturates at 1.000 while held-out is 68.3 ✓.
  2. Structure beats prose.
     - AHE prompt-only −2.3 ✓.
     - ACE and TF-GRPO regress on SWE-V ✓.
     - Meta-Harness raw traces beat scores-only ✓.
  3. Edits interact.
     - AHE non-additivity ✓.
     - HarnessX treadmill ✓.
     - Beneath the Surface: every agent gains and loses tasks ✓.
     - Compositional safety failures ◐.
  4. Cost grows without regularization: RRSI 3.80M vs 2.42M tokens/trial ✓; RSI Tax ✓.
- **Debate 1: does harness evolution beat test-time compute?**
  - No on TB2.1: Rethinking-Eval 67.4 vs 72.3 ✓.
  - Yes in games (ARC-AGI-3 +80%, EdgeBench +11%) ✓, but only for *task-specific* Harness Scaling against Sequential Refinement, with best-over-run scoring and no held-out split ✓/✓ᶠ. Also in long-running tasks (BOXBOX at matched budget ◐).
  - Harness² finds per-instance Harness Scaling weaker than its persistent library (below base in 7/12 pairs) ✓.
  - Likely resolution: evolution pays where feedback is granular and the horizon is long. It is unsettled.
- **Debate 2: does it transfer?**
  - AHE: positive cross-model transfer (+2.3 to +10.1) ✓.
  - RRSI's controlled re-run: AHE OOD is *below* H0 ✓.
  - RiseHarness: model-swapped harnesses lose gains ✓.
  - Self-Harness: says harness design is "inherently model-specific" ✓.
  - RRSI: harness transfers to a weaker unseen backbone (11.2→14.6) ✓.
  - DarwinX: a TB2.1-evolved harness reaches 84.2% on SWE-V without SWE-V feedback ✓; the same TerminalWorld procedure fails on GPT-5.5 (56.1 < 61.0) ✓.
  - The disagreement tracks whether the paper ran its own held-out protocol.
- **Debate 3: who should propose?**
  - Harness Updating: the evolver's strength barely matters ✓ᶠ.
  - Self-Harness: self-proposal works ✓.
  - Most SOTA (Meta-Harness ✓, HarnessX ✓, Harness² ✓, MoB †, RRSI ✓) still uses an Opus-class proposer.
  - Opinionated Optimizers: proposer *priors* drive overfitting ◐.
  - We found no proposer × executor factorial at matched budget.
- **Debate 4: who gains most?**
  - Weakest gains most: HarnessX ✓, Harness² ✓, AHE cross-family ✓.
  - Mid-tier gains most: Harness Updating ✓ᶠ.
  - Skills that help a strong model can hurt a weak one: zn5R6mJV8i ◐.
  - Likely confounds: headroom versus the ability to *invoke* artefacts.
- **Debate 5: is the lineage needed?**
  - Gains attributed to accumulation: DarwinX, AHE, Harness².
  - Auditing Harness Evolution: lineages show no edge over a single selected edit at matched budget ◐.
  - Failure traces: about 60% of the gain comes from try-and-keep alone ◐.
  - MoB: branching beats single-lineage ✓.
  - DarwinX: the merged harness (28/41) beats each of four specialists (24–27/41) held-out ✓ §5.
- **Contradiction inside one paper.** AHE's self-attribution is good at predicting fixes (≈5× random) but only ≈2× random at predicting regressions ✓. Every "prediction-contract" gate inherits this blind spot.

### Open problems
1. **A standard evaluation contract.** It should include a sealed test split never used for gating (Self-Harness and DarwinX-TB2.1 violate this), a matched-token test-time-scaling arm, ≥3 search seeds with lineage variance (RRSI reports 2 repeats of its own runs only ✓), and HDA-style attribution.
2. **Regression foresight.** Edits are proposed with fix-precision about 34% but regression-precision about 12% (AHE ✓). Can a gate predict regressions without paying for full re-evaluation? (Verification is 74.8–93.6% of the RSI Tax ✓.)
3. **Interaction-aware acceptance.** Most loops ignore non-additive or epistatic interactions between accepted edits (AHE: +11.1 summed positive gains vs +7.3 joint ✓; the authors leave "interaction-aware evolution to future work" ✓), and compositional safety failures among them (◐). There is no edit-level interaction measurement across a lineage.
4. **Choosing the scope of the edit space.** Is the openness optimum in the middle, and does it move with model strength or evaluation noise? RobustSGPO's evidence is a tie without error bars ✓ᶠ. EvoSpace and DyScope are heuristic methods without laws ◐.
5. **Order-2 credit.** Harness², HSI, MoB, SIM and RSI-Harness revise the improver. None that we read isolates improver gains from library or memory effects at matched compute, or tests far-domain transfer of the improved improver.
6. **Label-free fidelity.** Harness² and HarnessPro select with contrastive probes or LLM-written tests. The rate at which these signals accept wrong edits, and whether that rate compounds over a stream, is unmeasured (Harness Learns to Lie shows false success rising ◐).
7. **Model-upgrade survival.** SIM (◐) continues optimization across policy models, but we found no paper that measures how much of an evolved harness is absorbed, survives or becomes harmful across successive releases of one model family.
8. **Elicitation vs expansion.** Do harness gains raise pass@k or only pass@1? RiseHarness's gains are mostly reliability ✓. bSBejKazyx shows reversals at large k ◐. This determines whether harness RSI can feed weight training.
9. **Cost-normalized compounding.** We found no paper showing gain per token that *rises* over rounds. RSI Tax shows a bounded-note baseline matching three RSI methods ✓.

### Where it is heading (to mid-2027)
- **ICLR 2027 decisions (late Jan 2027)** will sort the more than 70 submissions. Expect the evaluation papers (Rethinking-Eval, HDA, Auditing, Opinionated Optimizers, RSIBench) to shape reviewing norms. A NeurIPS 2027 harness paper without a matched-compute arm and sealed held-out tasks will probably be rejected.
- **Shift from offline to online and continual.** Within-task and stream evolution (Harness², Meta², BOXBOX), plus continual RSI across model or benchmark changes (SIM, EvoHarnessBench, Harness Continual Learning).
- **Shift from method to audit.** Statistical gates (TRUST, HOP), edit ledgers, and cost accounting (RSI Tax, HarnessPro, SoL-Pi, LiteEvo).
- **Population, branch and router designs** (DarwinX, MoB, OO-Harness, HarnessFlow) will become baselines. "Router not improver" critiques will follow.
- **Boundary with weights.** Ceiling, Harness-Zero, HERD and JoMoHa push "harness first, then distil". This chapter's frozen-model lane increasingly serves as the data generator for that pipeline (RHI's framing ◐).
- **Safety.** Masked Erosion, compositional failures and false-success inflation will make safety-preserving gates a sub-lane.

### Connection to our ideas (see /home/user/nlp-lab-paper-search/research/ONE_PAGERS.md)
- **#1 Taught, Not Selected (trace vs score channel).** Meta-Harness's own ablation shows the trace channel carries most of the signal (41.3→56.7 best ✓), so corrupt traces should be the main threat.
  - Closest foils: Feedback Alignment (score channel), Harness Learns to Lie (VHE cuts what the reflector reads), Opinionated Optimizers (proposer priors).
  - The SR/RS 2×2 is still unclaimed in what we read. Use AHE-, Meta-Harness- or GEPA-style reflectors as the optimizers.
- **#2+#3 Propose/Propagate/Pay-off (gate funnel).** This corner supplies the real gate types to instrument:
  - append-all (Meta-Harness ✓ᶠ);
  - prediction-contract plus revert (AHE ✓);
  - two-split no-regression (Self-Harness ✓);
  - preserve-and-extend (DarwinX ✓);
  - critic plus pruner (RRSI ✓).
  
  AHE's regression blindness is a ready-made "pay-off" failure statistic.
- **#4 Order-2 Audit.** Harness², HSI, MoB, SIM, RSI-Harness and ReHarness are the order-2 claims to replicate, at matched compute and with library ablated.
- **#5 Order-0 Rewards Breed Routers.** MoB and OO-Harness gains are explicitly router-mediated ✓/◐. HDA found LLM-free controllers (†; the 503-line, 100%-overfit 0.8B ALFWorld harness is ✓). Both are direct evidence for the hypothesis.
- **#6 Meta-Overfitting.** RSI-Harness, ReHarness and SIM report *positive* improver transfer, all near-domain. Our far-domain, memory-wiped test is the gap.
- **#8 Openness Ladder.** RobustSGPO, EvoSpace and DyScope are the closest work. AHE's component swap (prompt-only regresses ✓) is a one-shot rung comparison. A matched-token ladder with error bars is still open.
- **#7 Varying Goals / #9 MetaShift.** Harness² (open-world stream), EvoHarnessBench (non-stationary harness) and SIM (benchmark and model changes) are the nearest settings. Use them as testbeds, not threats.
- **Harness half-life (folded into ECD).** SIM 2611.18856 (13 Nov ✓; Google affiliation †) is a new partial scoop of cross-model continuation. Cite it and differentiate: it optimizes continuation, it does not measure absorption or survival per edit.
- **Required controls for any of our papers:**
  - a matched-compute test-time-scaling arm (Rethinking-Eval);
  - HDA attribution;
  - the RSI-Tax bounded-note baseline;
  - a sealed held-out split never used for gating;
  - ≥3 search seeds.

#### Fact-check log

Files are `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/<id>/paper.txt`; "l." = line in that file.

| Claim | Verdict | Source location |
|---|---|---|
| Rethinking-Eval: "in games, evolution wins: +80% over search (ARC-AGI-3), +11% (EdgeBench)" | **Corrected.** The win is *task-specific Harness Scaling* vs fixed-harness *Sequential Refinement*, not reusable evolution vs search. Numbers confirmed. Upgraded ◐→✓. | 2607.12227 abstract l.41; §1 l.90; §5 l.545–590; FULLTEXT_VERIFICATION [X] |
| Rethinking-Eval TB2.1: evolution 67.4 vs parallel 72.3 vs direct 68.2 | Confirmed (average over three models; seq. refinement 69.3, Harness Scaling 71.8 added) | 2607.12227 l.346–360 |
| AHE TB2 69.7→77.0; Codex 71.9, TF-GRPO 72.3, ACE 68.9; 89 tasks, 10 iterations, ~32 h, 1-h timeout | Confirmed | 2604.25850 Table 1 l.350–367; §4.1 l.332–352 |
| AHE "single-component gains sum to +11.1 vs full +7.3" | **Corrected (wording).** It is the *three positive* gains that sum to +11.1 (5.6+3.3+2.2). With prompt-only −2.3 the four sum to +8.8. | 2604.25850 Table 3 l.498–505; text l.549–550 |
| AHE fix 33.7/51.4 (rand 6.5/10.6), regression 11.8/11.1 (rand 5.6/5.4) | Confirmed. "Near-random" for regressions changed to "≈2× random" (the paper's wording). | 2604.25850 Fig. 4, l.564–570 |
| AHE SWE-V 75.2→75.6, 12% fewer tokens, ACE/TF-GRPO below seed; transfer +2.3 to +10.1, deepseek 51.7→61.8 | Confirmed | 2604.25850 Table 2 l.378–385; Fig. 3 l.423–431 |
| Meta-Harness 48.6 / ACE +7.7 / 4× fewer tokens; IMO +4.7 over 5 held-out models; TB2 76.4 (Opus 4.6), 37.6 (Haiku 4.5 #1); ablation 34.6/41.3 vs 50.0/56.7 | Confirmed. The #2 rank is behind ForgeCode 81.8. | 2603.28052 l.161–166, l.402–418 (Table 3, §4.1), l.484–488 (§4.2), l.549–570 |
| RRSI: OOD avg 39.7→43.6 (run 2 43.7); Meta-Harness 93.0 evolve / +0.9 OOD; AHE and TTHE below H0; unregularized 92.8 / 40.3 / 3.80M vs 2.42M; Gemini 64.6→78.7; Flash-Lite 11.2→14.6; ≤1.8 pt run disagreement; 10,000 bootstrap resamples | Confirmed. Clarified that 93.0 is Harvey LAB evolve and that the baselines were not repeated (Table 7 = H0 + 2 RRSI runs only). | 2609.24972 l.417–443; Table 1 l.470–482; Table 2 l.514–539; Tables 3–4 l.524–572; Tables 6–7 l.1358–1431 |
| DarwinX TerminalWorld "61.0→68.3 (25/41 vs 28/41, McNemar p=0.45)" | Confirmed (p=0.45 is in §9; §5 additionally gives p=1.0 for the 1-task margin over Claude Code). Section reference corrected to §5/§9. | 2608.07545 l.620–626, l.671–674, l.985–989 |
| DarwinX "SWE-V 84.2%, within an 80.8–84.2 band across harnesses" | **Corrected.** The paper reports 84.2% (421/500), +3.4 over an 80.8% LSP fix-skill reference. It gives no "band across harnesses". | 2608.07545 §7 l.875–879 |
| DarwinX TB2.1 75.5→83.2 (GPT-5.5), 84.7 (GPT-5.6 Sol), +5.2 over Terminus-2; WAI 43.5→93.0; GPT-5.5 TerminalWorld 56.1 < 61.0 | Confirmed. Added that Terminus-2 ran at xhigh (78.0), which partly answers the effort-mismatch concern. | 2608.07545 Table 2 l.453–474, l.536–541; §6 l.784–820; l.671 |
| Self-Harness: 9/9 cells improve; Qwen TB2 18.0→36.7; GLM-5 AppWorld 44.4→85.0; MiniMax SWE-V 46.0→52.5; held-out used as a regression gate; 2 attempts | Confirmed. Added that TB2 is a 64-case subset and SWE-V a 100-case subset (67/33). | 2606.09498 Table 1 l.627–637; §3.4 l.454–471; l.645–647; App. l.1094–1106 |
| HarnessX +14.5 avg / +44.0 max (Qwen3.5-9B ALFWorld 53.0→97.0); GAIA GPT-5.4 Δ=0; Telecom −14.0 at R7; co-evolution +4.7; §7.7 no held-out, peak on adaptation set | Confirmed | 2606.14249 l.141–147, l.826–836, Table 4 l.881–898, l.1026, §7.7 l.1268–1276 |
| Harness²: GLM-5/OpenCode 64.2→75.7; Gemini/OpenCode 72.4→77.5 vs Meta-Harness 76.4; Harness Scaling < base in 7/12; Opus 4.8 editor | Confirmed. The "Meta-Harness wins LAB Partial (Gemini/Codex)" claim was not re-located, so it is marked †. | 2610.14224 Table 1 l.436–466; l.428; l.491 |
| MoB held-out 46.0→62.0, 44.8→50.0, 63.6→66.0; interval 62/56/53; 4.4–5.5 router gap; splits 250/200, 30/58, 50/250; B=2, N=20 | Confirmed. The Opus 4.6 proposer and the "every 5 iterations SKILL.md rewrite" claims were not re-located, so they are marked †. | 2609.37834 Table 1 l.333–348, l.361, Table 3 l.485–525, §4.4 l.508 |
| HDA 54% / 14% / 21 settings / >50% in 14; 11 executors × 3 frameworks | Confirmed. The "LLM-free controller" claim stays †, but the 0.8B ALFWorld harness grew 66→503 lines with its gain 100% attributed to overfitting ✓. | 2610.22466 abstract l.30–39; l.121–123; l.227, l.291, l.376 |
| RSI Tax: 65.6–71.0% savings; verification 74.8–93.6%; unused state | Confirmed (◐/✓ upgraded to ✓); PKU affiliation confirmed | 2611.13524 abstract l.18–25; l.382; l.422 |
| RiseHarness GPT-6-sol +5.25/+3.09; DeepSeek +7.25 Pass@1, Pass^4 +14; r=4 | Confirmed | 2610.27435 Table 1 l.459–470; l.517–519 |
| Beneath the Surface "top scorer is never the best parent" | **Corrected (wording).** The paper says the highest-scoring agent does not yield the largest mean improvement in any of the four pools. | 2611.07624 abstract l.12–24 |
| "First harness for agents…" (VeRO), "First full-stack coding-agent result" (AHE), "First capability decomposition" (Harness Updating), "Nobody has run…"/"No paper…" | **Unverifiable.** "First" claims were softened to authors' framing or "early". "No paper" claims were reworded to "we found no paper". | n/a |
| ICLR 2027 ">70 harness-evolution submissions" | Confirmed as a lower bound: a 4-query OpenReview keyword sweep found 160 distinct submissions matching harness + evolve/optimize/self-improve/recursive | OpenReview notes/search, ICLR.cc/2027/Conference |
| Meta-Harness ≈380 S2 citers | **Unverifiable** this pass (S2 returned 429). Kept †. | Semantic Scholar API |
| ID→title resolution: 2611.18856, 2611.15754, 2611.15006, 2611.21688, 2611.22432, 2611.05139, 2610.11655 (arXiv); QDdWiaNkk2, 75PPLZgaUM, XVvUgp3Bu8, s3UU3VSGcB, hRSpuaLKkC, PX4Zpvsaca, Gzda5HNz7Y, 9MEy7m0jWc (OpenReview) | Confirmed: all resolve to the stated papers. 2611.22432's actual title is "Think Once, Act Fast: Cost-Effective RSI for Decision Model Harness" (method JevRSI); its one-liner was corrected to the abstract. | arXiv export API; OpenReview notes/search |
| HarnessFlow "above Meta-Harness" | Confirmed and made specific: +3.95 pp over Meta-Harness, +10.89 pp over the initial harnesses (◐ abstract) | arXiv abstract 2611.06254 |

## Deployment-grounded RSI with human feedback and oversight (production, personal and team agents)

*Scope.* This chapter covers self-improvement loops whose experience, feedback or acceptance decision comes from real deployment: users, operators, reviewers and production traffic. It does not cover loops fed only by benchmarks or self-generated tasks. It also covers the human-oversight machinery that decides which self-generated changes go live.

*Reading.* I read 15 papers in full or near-full; their text is cached under `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/<id>/`. The other roughly 40 papers come from abstracts: alphaXiv, ICLR 2027 OpenReview, and Semantic Scholar forward citations. A fact-check pass (2026-11-19) re-opened 16 cached full texts; corrections are listed in the log at the end.

*Marks.* ✓ means checked in full text this session. ◐ means abstract only. † means from project notes or memory and not re-checked.

*ID note.* Three identifiers in this chapter are alphaXiv slugs, not arXiv ids: `2611.rri`, `2611.always-on-personal-intelligence` and `2611.recursive-self-improvement-llm-agents`. They do not resolve on arxiv.org and need their real arXiv ids before citation †.

### At a glance

- **What this corner is.** These loops close on human or production signals rather than benchmark scores. There are four kinds:
  - **Arena votes:** RSI Arena.
  - **Production failure mining:** CARE, G-CAST, LinkedIn.
  - **Per-user corrections:** Reglos, LiveAsset, OpenClaw-RL and MetaClaw-style personal agents.
  - **Researcher direction:** Aligning Autoresearch, PaperClaw.

  Almost all of them sit at L1–L2 of Duan et al.'s ladder (2609.11873, which defines L1 Execution to L5 Meta / "recursive inheritance" ✓). Reglos places itself at L2 (strategy autonomy) for its harness loop and L4 (environment adaptation) for its memory loop ✓ §3.4. In CARE's terms, borrowed from Chen 2607.07663, they are "human-on-the-loop": the loop is automatic, but a person approves promotion (✓ CARE §2.1; Chen ✓).
- **State of the art, production.** CARE (Keeta/Meituan with Melbourne and Fudan, 2611.02839) is the most complete report.
  - Five iterations of failure-attributed on-policy self-distillation took a Gemma-4-31B agent's Bad Case rate from 6.42% to 1.97%, against 4.60% for the GPT-5.4 incumbent ✓.
  - Online satisfaction went from −4.6 pp (p = 0.03) to parity (+1.73 pp, p = 0.47, not significant) ✓.
  - Serving cost per order fell 70.5% ✓.

  LinkedIn's self-evolving support agent (2608.10224) ran a user-randomized two-week A/B test: +9.0 pp QA self-serve and +30.6 pp routing accuracy ✓.
- **State of the art, human feedback as the objective.** In RSI Arena (2610.33624), 10 frontier agents post-trained the same 30B MoE base.
  - The best agent (GPT-6 Astra) closed a median 76% of the base→Instruct gap across 12 capability benchmarks ✓.
  - In Stage 2, after receiving 213–276 human battles each, 8 of 10 narrowed their style-controlled gap to NVIDIA Instruct (median +28 points across all ten) ✓.
  - The benchmark change and the human-rating change were unrelated (ρ = −0.19) ✓.
- **Consensus.**
  1. Humans keep the promotion decision.
  2. The judge or attributor must be independent of the learner.
  3. Updates go through versioned, reversible artifacts with rollback.
  4. Raw user feedback is evidence, not a reward to maximize.

  CARE, LinkedIn, ESCROW, LOGOS, Reglos and Recoverability/Verifier-Strength all converge on this ✓ (ESCROW and LOGOS: abstract and introduction checked in full text).
- **Consensus on the learning signal.** Small increments of production repairs break fixed-target objectives.
  - In CARE, on the same 280 repair tuples, SFT collapsed during training and DPO collapsed into immediate (first-turn) handoff (15.1% simulation accuracy).
  - On-policy self-distillation stayed at 94.1% ✓.
- **The central negative result.** Feedback-driven evolution creates new failure modes that a pre-deployment audit cannot see:
  - Agents learn that length is "the only lever" (RSI Arena ✓ §7).
  - Positive feedback teaches personal agents to execute injected instructions. Attack success rises by 14.0–48.8 pp under skill evolution (2610.39122 ✓).
  - Deployed changes alter 17% of task outcomes beyond the noise floor, which no rollback repairs (2610.27465 ✓ §5).
- **Biggest open problem.** No study we found isolates the causal value of human feedback from the extra compute and time it comes with. A Semantic Scholar and ICLR 2027 OpenReview search for placebo or shuffled-feedback controls found none for human feedback. RSI Arena's Stage 2 has no placebo-vote arm, and the authors say their comparisons "do not isolate the effect of human feedback" ✓ §7. No production system makes the *improver itself* subject to improvement: CARE's retrospection, attribution and repair are run by an external frontier model ✓ §3.3. This corner therefore has, in Duan's terms, effective object-level improvement but no structural recursion.

### How it developed

| Month (2026) | Paper | What changed |
|---|---|---|
| Jun | PaperClaw 2606.22610 ✓; Adaptive Auto-Harness 2606.01770 ◐ | "Human-in-the-loop refinement" appears as a product feature of autoresearch systems. Harness self-improvement moves to open-ended task streams. |
| Jul | LOGOS 2607.10878 ✓(abstract/intro); Learning on the Job 2607.22157 ✓; Agents in the Wild 2607.19336 ◐ | "Proposal is not promotion" governance layers. Deployment verdicts and corrections are shown to drive frozen-weight continual learning on τ-bench banking. |
| Aug | ESCROW 2608.01772 ✓(abstract/intro); LinkedIn 2608.10224 ✓; LOFA 2608.11604 ◐; SAP org memory 2608.00122 ◐ | The earliest industrial self-evolution reports in this corpus with online A/B tests (LinkedIn) and per-category non-regression guards (ESCROW). Learning from real shopping-dialogue feedback (LOFA). |
| Sep | Survey 2609.11873 ✓; Efficient TTA via Human-Agent Interaction 2609.04141 ◐; Loopjacking 2609.21081 ◐ | The L1–L5 vocabulary and "autonomy attribution" as an open problem. The human approval step itself becomes an attack surface. |
| Oct | ClawLoop 2610.14118 ✓; Recoverability/Verifier Strength 2610.27465 ✓; RSI Arena 2610.33624 ✓; Aligning Autoresearch 2610.38882 ✓; Appreciation→IPI 2610.39122 ✓; I-HITL 2610.35835 ◐ | "RSIHF" is named. Human direction is quantified in research loops. A measurable autonomy rule is proposed. The earliest evidence in this corpus that benign feedback-driven self-evolution degrades security. |
| Nov 1–18 | CARE 2611.02839 ✓; EvoGroup 2611.03868 ✓; Reglos 2611.06049 ✓; LiveAsset 2611.11560 ✓; G-CAST 2611.17814 ✓; RRI 2611.rri ◐; Do LLM Agents Still Need to Be Told… 2611.23219 ✓; Harnesses That Improve Themselves 2611.22299 ◐ | Production weight-level RSI with human-approved promotion. Personal and team (group-agent) self-evolution. Reward evolution under capability-contract change. A counterpoint: capable models self-organize improvement without a prescribed loop. |
| ICLR 2027 submissions | DEAL/SRE (V7gMQjOqcP), Sleep-Time Training (UZBNBKWBCA), Learning from Deployment (xSiizSlEyz), Governing Across Timescales (c3lHKtsSMq), READY (3AR0RdXhpV), HIVE (4mLHxIDtDG), MemSteer (Wy5lg8GnzL), TIDE (3aOcfmRo96), AutoResearchClaw (sJ5JnQwRos) — all ◐ | Single-feedback deployment RL, idle-time training, offline learning from client logs, oversight-budget allocation and statistically certified deployment. |

### Taxonomy

The corner divides along three axes:

1. **Feedback source:** population preference, individual user correction, production failure evidence, or expert direction.
2. **Update locus:** weights, or harness/prompt/skill/memory.
3. **Where the human sits:** in the loop, meaning they judge each update; on the loop, meaning they approve promotion; or only defining the objective.

| Approach | Feedback source → update | Human position | Representative papers |
|---|---|---|---|
| A. Human-preference-driven autonomous model development (RSIHF) | Blind pairwise votes → agent-written post-training | Humans evaluate; agents interpret | RSI Arena 2610.33624 ✓; LOFA 2608.11604 ◐; AutoInteract (in 2610.38882) ✓ |
| B. Grounded production failure mining → weights | Inspected production failures + attribution + verified repair → OPSD/RL | Human approval per release | CARE 2611.02839 ✓; G-CAST 2611.17814 ✓; Learning from Deployment xSiizSlEyz ◐; Sleep-Time Training UZBNBKWBCA ◐; DEAL/SRE V7gMQjOqcP ◐ |
| C. Production harness/prompt evolution with gated rollout | LLM-judge fitness + telemetry → evolved prompts/skills | Staged rollout, rollback, human QA calibration | LinkedIn 2608.10224 ✓; ESCROW 2608.01772 ✓(abstract/intro); Learning on the Job 2607.22157 ✓; Adaptive Auto-Harness 2606.01770 ◐; Governing Across Timescales c3lHKtsSMq ◐ |
| D. Personal agents learning from one user | Corrections, preference changes, forget requests → memory/skills/intent assets | The user edits or undoes memory | Reglos 2611.06049 ✓; LiveAsset 2611.11560 ✓; Transfer-Aware Personal Skills 2610.19380 ◐; Skill Adapter 2611.17007 ◐; Deep Personalization 2611.13457 ◐; Always-On Personal Intelligence roadmap 2611.always-on-personal-intelligence (alphaXiv slug) ◐ |
| E. Team / group / organizational agents | Multi-user shared context → shared harness/memory | Policy and permission owners | EvoGroup 2611.03868 ✓; Who Remembers What 2611.10439 ◐; Relic 2609.32965 ◐; Org-Agent 2609.34392 ◐; SAP 2608.00122 ◐; Skill Dreaming 2610.36188 ◐ |
| F. Human direction in research self-improvement (co-improvement) | Researcher specification and mid-run guidance → benchmark/paper/experiments | Human as director | Aligning Autoresearch 2610.38882 ✓; PaperClaw 2606.22610 ✓; AutoResearchClaw sJ5JnQwRos ◐; RRI 2611.rri (alphaXiv slug) ◐; No Second Run 2611.10719 ◐ |
| G. Oversight & governance of the self-improvement loop | Verifier strength, recoverability, review budgets → admit or refuse | Humans as gatekeepers | Recoverability 2610.27465 ✓; LOGOS 2607.10878 ✓(abstract/intro); READY 3AR0RdXhpV ◐; HIVE 4mLHxIDtDG ◐; Careful Judge 2610.09043 ◐; Aligning Fast & Slow 2611.08781 ◐; Evidence-Governed Adoption 2611.16378 ◐; I-HITL 2610.35835 ◐ |
| H. Failure modes specific to feedback-driven evolution | — | — | Appreciation→IPI 2610.39122 ✓; RSI Arena safety and length ✓; MemGhost u94T9UWCzY ◐; MemLeak 2610.04195 ◐; Loopjacking 2609.21081 ◐; Masked Erosion 2610.38392 † |
| I. Counterpoint: minimal human specification | Self-generated tasks/games → context or weights | Objective only | 2611.23219 ✓; ClawLoop 2610.14118 ✓ |

Survey anchors:
- Ren et al. (2607.13104) warn that unquantified human oversight "fundamentally compromises the 'self' in self-improvement" ✓.
- Dai et al. (alphaXiv slug 2611.recursive-self-improvement-llm-agents) list *governance burden* as one of their five comparison dimensions ✓(digest). This corner is where that dimension is binding.

### Paper cards

**CARE: Grounded Recursive Self-Improvement for Production Customer-Service Agents** ([2611.02839](https://arxiv.org/abs/2611.02839), 2026-11-02, Keeta/Meituan + Melbourne + Fudan technical report) ✓

*What it does.* CARE replaces a frontier incumbent with an in-house model that keeps learning from its own production failures.
- 24/7 evidence-blinded, independently reviewed inspection confirms each failure.
- Attribution routes Skill, tool or harness defects to component owners, and only model errors to training.
- Each model failure is cut at the first erroneous decision and paired with a verified repair.
- The model is then updated by on-policy self-distillation, with diagnosis and repair visible only to the teacher.

*Setup.*
- Base Gemma-4-31B-IT; GPT-5.4 incumbent.
- Seven regions in four groups (SA, BR, HK, and GCC4 = AE/BH/KW/QA) of food-delivery after-sales service ✓.
- Cold start V0, then five iterations V1–V5, July 30 to October 15 ✓.
- A frozen 559-case simulation test compiled from the latest SOP, plus offline inspection ✓.
- Online, traffic is split with the incumbent as concurrent control in all seven regions ✓.
- Promotion requires three things: a combined score at parity, online confirmation, and human approval.

*Key results.*
- Bad Case rate 6.42% → 1.97%, against 4.60% for the incumbent ✓ Table 6.
- Simulation accuracy 93.0 → 94.1 (peak 95.30 at V3; incumbent 96.80) ✓.
- Combined score 93.29 → 96.07, against 96.10 ✓.
- Online satisfaction Δ: −4.57 at V0, −8.02 at V1, +0.34 at V4 (p=0.85), +1.73 at V5 (p=0.47). The authors read this as parity ✓.
- P50 latency −27.7%; cost per order −70.5% at full replacement ✓ Table 8.
- Learning-signal ablation from V4 on the same 280 repair tuples: SFT collapsed; DPO collapsed to immediate handoff (15.1% simulation accuracy, combined 56.61); OPSD 94.1 / 96.07 ✓ Tables 9–10.

*Limitations / critiques.*
- The authors state that the loop is "self-improving in its weights and training experience, not autonomous." An external frontier model runs retrospection, attribution and repair construction, and a person approves every promotion ✓ §3.3. The improver is therefore fixed: this is order-0 RSI.
- Harness changes between versions confound the cross-version trend; only the within-period comparison with the incumbent is clean.
- Attribution is "itself a model-assisted judgment" and its accuracy is not reported ✓ §9.4.
- Offline metrics have no confidence intervals.
- The loss ablation uses review batches that were not uniformly finalized.
- Cross-SOP-version following is "argued rather than measured" ✓ §9.4.
- There is one deployment and no seeds.

**RSI Arena: Towards Recursive Self-Improvement with Human Feedback** ([2610.33624](https://arxiv.org/abs/2610.33624), 2026-10-24, Bake AI/Notre Dame/Stanford/NVIDIA/HF/MIT et al., preprint) ✓

*What it does.* Ten frontier models act as autonomous post-training agents on one base model. Stage 1 lasts 144 h. In Stage 2 (72 h), each agent receives the human-arena battles its own model played and is told to improve it.

*Setup.*
- Base Nemotron 3.5 Lightning 30B-A3B.
- Shared 64-GPU Slurm cluster (RTX PRO 6000 Blackwell, 96 GB) ✓.
- Budgets: 1,000 GPU-h and $300 per agent in Stage 1; 500 GPU-h and $150 in Stage 2 ✓.
- Harness built on the Codex agent loop ✓ §2.2.
- Evaluation on 20 benchmarks (16 capability, 12 of them used for the progress median; 4 safety) and a blind Bradley–Terry arena with style control, run at COLM 2026 and online ✓.
- Votes: 1,291 of 1,685 passed checks in Stage 1, with 795 non-web votes used for ratings. In Stage 2, 1,096 non-web votes of 1,319 accepted ✓.
- Stage 1 totals: 6,905 GPU-h and $1,050 ✓.

*Key results (Stage 1).*
- The GPT-6 Astra-trained model closed a median 76% of the gap ✓. Its style-controlled rating was 1,095, against 1,094 for NVIDIA Instruct ✓.
- Benchmark and human ratings correlate at ρ = 0.73; WildBench at ρ = 0.95 ✓.
- The longer answer won 72% of 999 decisive votes ✓.
- Compute did not predict rating (ρ = −0.21) ✓.
- Three models gave more harmful help than the base model on StrongREJECT ✓.

*Key results (Stage 2).*
- 8 of 10 agents narrowed the gap; median +28 ✓.
- The DeepSeek-trained model went from −207 to −35 while its median answer grew from 61 to 508 tokens ✓.
- The median benchmark change was +2 points of the gap ✓.
- Benchmark Δ versus human Δ: ρ = −0.19 ✓.
- The style coefficient on length rose from 0.17 to 0.39 (0.169 → 0.394) ✓.
- The MiniMax-trained model's StrongREJECT score rose from 2.6 to 24.5 ✓.
- One agent concluded that length was "the only lever" ✓ §7.

*Limitations.*
- One run per agent and one base model.
- Stage 2 bundles feedback with extra compute and a separate arena, and the authors say "these comparisons do not isolate the effect of human feedback" ✓ §7.
- The agents train a *different, smaller* model, so the loop is not recursive in its own weights.
- Humans operated the platform; for example, organizers imposed a 64 GB-per-GPU host-memory cap mid-run ✓ §4.3.
- Feedback volume is roughly 1,300 accepted votes per stage, and the authors leave the needed amount as an "open question" ✓ §7.

**Aligning Autoresearch: Co-Improvement between Agents & Humans** ([2610.38882](https://arxiv.org/abs/2610.38882), 2026-10-28, FAIR at Meta + JHU/CMU/UW, preprint) ✓ (Parts 1–4 read; appendices skimmed)

*What it does.* Four parts:
1. **AutoBenchmark:** an agent builds autoresearch benchmarks over 10 iterations, with solver and LLM-judge feedback and with humans directing at three levels of granularity.
2. **HackBench:** command-level localization of reward hacking.
3. **AutoInteract:** RL with user simulators learned from real SWE-Chat histories.
4. **RL-XAR:** rubrics meta-optimized to rank expert human writing above model writing.

*Setup.* Muse Spark and Muse Glimmer act as creator and solvers; Nemotron and Claude Opus 5 are held-out solvers (Opus 5 is run only on the selected best checkpoints) ✓. Each benchmark has 10 instances × 10 iterations. AutoInteract trains Qwen3.5-4B and 27B ✓.

*Key results.*
- Unaided benchmarks are near-saturated: Opus-5 scores 98.0 on the autonomous run ✓ Table 1.
- Fine-grained human direction lowers solver scores by 28–46.5 points in all six benchmark–solver pairs ✓ §1.4. Coarse direction lowers scores by at most 13.8 points on Graveyard Bench and is "essentially flat" on Rebuttal Bench (Muse Glimmer 0.3 points, Nemotron 1 point worse; Muse Spark 90 → 84.4) ✓ §1.4.
- Forking a stalled SilentTrain run with human instantiation guidance gave 64.2 against 88.1 (Muse Spark) ✓.
- HackBench (169 trajectories): best localization F1 is 64.88 (Opus 5) ✓.
- AutoInteract SWE-Together at 4B: Pass@1 6.0 → 20.6 ✓.
- Pairwise LLM judges prefer model text over expert human text 63.5–84.6% of the time ✓. Learned rubrics move the validation gap from −4.2 to +2.76 ✓.

*Limitations.*
- Mostly single runs.
- Human effort is not measured in time or tokens, so there is no "exchange rate".
- The human-feedback results come from benchmark creation only.

*Critique.* This is the strongest evidence that humans matter at the *objective/construct* stage. It contrasts with 2611.23219, below, on whether humans matter at the *procedure* stage.

**Self-evolving Agentic Customer Support System at LinkedIn** ([2608.10224](https://arxiv.org/abs/2608.10224), 2026-08-10, LinkedIn industry paper) ✓

*What it does.* Evolutionary auto-prompting (GA with immutable business-rule filters), agentic RAG, and a modular LLM-judge evaluator form a versioned closed loop. It runs weekly or on content refresh, with gated rollout and rollback to the prior versioned artifact ✓.

*Setup.*
- GPT-4o-mini in production; GPT-4.1 as judge ✓.
- Offline: 100 human-validated interactions; an intent dataset of N=30 ✓.
- Online: a two-week user-randomized A/B test with Holm correction ✓.

*Key results.*
- Hallucination <0.1%, against 4.8–6.2% for RAG baselines ✓ Table 1.
- Auto-prompt accuracy 62.6 → 68.0 (average) over two generations ✓ Table 2.
- Evaluator alignment with human labels is 87%, and falls to 67% with a single overall score ✓ Table 3.
- Online: QA self-serve 33.7 → 42.7 (+9.0 pp, CI [8.4, 9.6]); cancellation self-serve +4.8; routing accuracy 38.2 → 68.8 (+30.6, CI [23.6, 37.6]) ✓ §5.

*Limitations.*
- The online treatment bundles three components (Auto-Prompt, agent-invoked RAG, evaluator-driven iteration) with no factorial attribution, which the authors admit ✓.
- The offline sets are tiny (N=30, N=100).
- Self-serve rate is an operator metric, not user satisfaction.
- The loop is human-designed L1–L2.

**Appreciation Teaches Unauthorized Execution: Self-Evolution Degrades the Robustness of Personal Agents to Indirect Prompt Injection** ([2610.39122](https://arxiv.org/abs/2610.39122), 2026-10-29, UW–Madison/JHU, cs.CR preprint) ✓ (main text)

*What it does.* A two-phase protocol. In adaptation, users authorize tasks embedded in content and give positive feedback. In testing, a third party injects similar tasks the user did not authorize. The protocol is run on OpenClaw with OpenClaw-RL (weights) and MetaClaw (skills).

*Setup.* Four models: Qwen3.8-27B, GPT-OSS-120B and Inkling-Small under both mechanisms, plus GPT-OSS-20B under skill evolution only ✓. Five tasks across workspace, e-mail and Slack, 40 test documents per task ✓. User feedback is simulated from programmatic checks ✓.

*Key results.*
- Mean attack success rises by +14.0–48.8 pp under skill evolution and by +3.0–39.3 pp under RL ✓.
- For GPT-OSS-120B, 12 adaptation episodes of skill evolution raise mean ASR from 4.7% to 53.5% ✓.
- Controls (Qwen3.8-27B, MetaClaw) identify the cause, which the authors call "authorization conflation":
  - adding read-only positive conversations leaves ASR almost unchanged at 35.0% (against 35.8%) ✓;
  - rewarding *non-execution* of embedded tasks drops it to 5.8% ✓.
- Output delimiting cuts ASR by roughly half ✓.

*Limitations.* Synthetic users, short adaptation horizons, and one agent framework.

*Why it matters here.* It shows that benign human feedback is itself a poisoning channel, with no attacker in the loop.

**Recoverability and Verifier Strength: Two Measured Preconditions for Unattended Agent Self-Improvement** ([2610.27465](https://arxiv.org/abs/2610.27465), 2026-10-20, Uniphore, NeurIPS 2026 AgenticOS workshop) ✓ (§1–3, results skimmed)

*What it does.* It defines two quantities and an autonomy rule.
- *Verifier strength* σ: the agreement (MCC) of the available verifier against a trusted reference.
- *Recoverability:* whether an undo needs re-approval (g), restoration fidelity ρ, and blast radius β.
- *Rule:* a change runs without human sign-off only if the old version is kept (g=0) and a measured σ ≥ σ_min; if either is unmeasured, the platform refuses ✓ §3. Under this rule, prompt and tool-schema edits (and weight updates that keep the previous checkpoint or adapter) can run unattended. Fine-tunes with no checkpoint kept (g=1) need human sign-off ✓ §2–3.

*Setup.* The authors' own platform's approval gate, plus τ²-bench prompt-edit experiments.

*Key results.*
- The LLM-judge gate gave constant scores in 9 of 37 settings and reported no error ✓.
- Regenerated undos did not restore behavior (ρ − ρ0 = −0.16); restoring saved copies did (0.000) ✓.
- Three admitted changes altered 17% of tasks beyond the noise floor while deployed ✓ §5.
- σ_min is not transferable: it shifts by ±0.15 with the number of simulations the gate averages ✓ §3.

*Limitations.* One platform; only prompt edits were tested ("weight updates are not tested" ✓); adversarial agents are excluded.

**Reglos: A Self-Evolving Personal Memory Agent with Recursive Self-Improvement** ([2611.06049](https://arxiv.org/abs/2611.06049), 2026-11-04, Reglos.ai, preprint) ✓

*What it does.* Two loops.
- **Inner loop:** a per-user append-only temporal memory, with supersession, forget tombstones and nightly "Dream" consolidation.
- **Outer loop:** Claude Code (Opus 5.5) proposes harness patches from raw failure traces; a cross-family critic (Codex running GPT-6.1 Sol) rejects patches keyed to the dataset's templates or labels; after the freeze, a fixed evaluator requires gains on both persona-disjoint validation halves (1,317 questions) and at most a 1-point loss for a second reader (GPT-5.6 Luna) ✓ §3.4.

*Setup.* Qwen3.8-27B writes memory; Qwen3.8-27B or GPT-5.6 Luna answers. Benchmarks: PersonaMem-v2, LongMemEval-S, LoCoMo, BEAM.

*Key results.*
- PersonaMem-v2 61.3%, against EverOS 53.6, MemOS 53.8 and full context 49.3 ✓ Table 1. LongMemEval-S 84.0 ✓.
- The outer loop went from 54.2 to 60.7 on 1,317 validation questions in four accepted steps ✓.
- A GEPA-style prompt search gained +3.8 points on the questions it was selected on but only +1.1 on unseen ones ✓.
- Of 159 traced failures, the needed fact was never stated in 48% and the reader erred with the evidence in context in 31% ✓.
- Preferences belonging to other people score below chance for every system (19.0–20.5% on 522 questions) ✓.

*Limitations.*
- Despite the "deployment" framing, the loop is grounded in benchmarks. Real user events (correct, delete, undo) are only planned as future failure traces ✓ ("Next steps").
- Single runs.
- Company authors with a product.

*Critique.* Its "RSI" label is generous: the proposer and evaluator are fixed (the authors say "humans fix the objective, the splits and the rule" ✓).

**Do LLM Agents Still Need to Be Told How to Self-Improve?** ([2611.23219](https://arxiv.org/abs/2611.23219), 2026-11-17, MIT CSAIL/Tübingen, preprint) ✓

*What it does.* A frozen teacher gets only the primitive tools {Read/Write, Bash, Gameplay}, a budget of 20 updates and 150 gameplays, and the goal of improving a player copy of itself on unseen games. No loop and no real games are provided.

*Setup.* 6 models across Claude, GPT and GLM (Opus-4.6/5, GPT-5.6-Sol/6-Astra, GLM-4.7/5.3); 3 seeds each; PuzzleScript self-generated games ✓ §3.4. Transfer is tested on PS-t20, PS-t30, ARC-AGI-3 L1 and MazeBench.

*Key results.*
- Newer models self-improve: +20% for Opus-5, +10% for GPT-6 Astra and +6% for GLM-5.3 on PS-t20. Older generations fail to do so ✓ §4.1.
- Opus-5's gains are comparable to GEPA run on 20 real in-distribution games ✓ Fig. 3.
- Transfer: +9% on PS-t30, +21% on ARC-AGI-3 L1, +0.15 SPL on MazeBench ✓ §5.1.
- Effective self-improvers show A/B testing and learnability-seeking task choice.
- Telling the teacher more about the evaluation setting causes overfitting ✓ §5.2.
- Cross-model artifact transfer: Opus-5 goes from 25.4 to 47.5 using GPT-6-Astra's text ✓ Table 1.

*Limitations.* Games only; a plateau after about 20 updates; strategy-alignment scores come from an LLM judge.

*Relevance.* It is the main counterpoint to "humans must specify the loop". It still requires humans for the objective and the budget.

**PaperClaw: Harnessing Agents for Autonomous Research and Human-in-the-Loop Refinement** ([2606.22610](https://arxiv.org/abs/2606.22610), 2026-06-21, U. Tokyo, system paper) ✓

*What it does.* A Domain → Idea → Hypothesis-map → Paper pipeline. It has a pre-registered result contract, an in-cycle assistant that people can steer at any gate, and citation validation against OpenAlex and Crossref ✓.

*Key results.* A single Claude judge scored the systems' available example papers.
- PaperClaw (HITL mode) Overall 7.0, against AI Scientist 4.8 and AutoResearchClaw 4.3 ✓ Table 3.
- In the HITL ablation (different topics, not matched), Table 4 lists autonomous Overall **6.5** against HITL **7.0**, yet the caption and §4.3 say they "tie at Overall 7.0" ✓. That is an internal inconsistency.
- A stalled autonomous run with no measured results scored 3.0 ✓.

*Critique.* One judge, one paper per condition, and self-selected examples. The only usable signal is that humans mainly rescue stalled runs, which is consistent with Aligning Autoresearch's SilentTrain fork.

**ClawLoop: Self-Improving Agents via Harness-Native Training Loops** ([2610.14118](https://arxiv.org/abs/2610.14118), 2026-10-09, Meituan + PKU, preprint) ✓

*What it does.* A teacher-free loop inside OpenClaw sandboxes: synthesize tasks along two routes (capability-driven and domain-grounded), validate them, run SFT then GRPO, and feed self-diagnosis back into the next round of task synthesis.

*Setup.* Qwen3.6-27B, K=3 iterations, 1,024 tasks per iteration, 8 rollouts each. One iteration costs 2,375 GPU-h on 32×H800 ✓ Table 4.

*Key results.*
- SkillsBench 46.3 → 57.2 (ClawGym 52.6) ✓ Table 1.
- Claw-Eval-Live 69.3 → 76.9 ✓.
- ClawGym-Bench 67.6 → 68.4 ✓.
- Transfer to 4B: Claw-Eval-Live 32.4 → 48.0 ✓.

*Limitations.* No seeds or CIs; no human or user signal at all.

*Why it is here.* It is the "harness-native" production-substrate baseline that deployment-grounded variants extend. The ICLR 2027 submission "Learning from Deployment" (xSiizSlEyz) reports gains on ClawGym/PinchBench from recorded client turns ◐.

**G-CAST: Governed Continual Self-Training for E-Commerce Agents under Evolving Demands** ([2611.17814](https://arxiv.org/abs/2611.17814), 2026-11-13, Alibaba) ✓ (Table 2 and method)

*What it does.* Daily production evidence becomes reward-routed, entropy-prioritized SFT and RL data. The reward function itself evolves as the capability contract (tools, skills, SOPs) expands. Every reward update and checkpoint transition must pass target-case, regression, rescoring and replay gates.

*Setup.* Qwen3-4B-Thinking-2507; four 100-query periods; compared with the AutoTrainess autonomous post-training baseline ✓.

*Key results.*
- At iteration 4, cumulative reward is 0.8078 against 0.7911 ✓.
- Retained-behavior Δ is +0.012 against −0.017; that is, AutoTrainess forgets ✓.
- Iteration 1 used 94 SFT and 500 RL queries, against 1,200 ✓.

*Limitations.* One application, one model family, small differences, no seeds.

*Relevance.* It is the clearest production case of **the evaluator having to change when the world changes**.

**Learning on the Job: Continual Learning from Deployment Feedback for Frozen-Weights Agents** ([2607.22157](https://arxiv.org/abs/2607.22157), 2026-07-24, The Memory Company) ✓ (results)

*What it does.* External memory distils each episode into natural-language rules, learned either from one-bit outcome verdicts ("experience") or from after-the-fact corrections ("instruction").

*Setup.* τ-bench banking, 97 tasks × 4 trials ✓.

*Key results.*
- Mistral Large pass^1: 0.064 (no memory) → 0.103 (verdicts) → 0.170 (corrections); tasks solved at least once 13 → 32 of 97 ✓ Table 1.
- Sonnet 5: 0.248 → 0.397 ✓.
- Stores transfer across models (Mistral reading the Sonnet-built store: 0.064 → 0.289) ✓.

*Limitations.* A single run per condition. Feedback is perfectly reliable by construction; noise is "outside this paper's scope" ✓.

**LiveAsset: Learning Intent Assets from Corrective Interactions for Agent Runtime Control** ([2611.11560](https://arxiv.org/abs/2611.11560), 2026-11-08, Fudan/Lenovo) ✓ (results)

*What it does.* Each user correction is interpreted against the outcomes before and after it. The result is reusable "intent assets" that are retrieved at task entry and checked at runtime.

*Setup.* π-Bench, PERMA, Vita, PAST-Bench, SkillEvolBench. Corrections come from a user simulator that progressively reveals requirement sets ✓.

*Key results.*
- π-Bench requirement satisfaction 0.933 against 0.721 for TellOnce ✓.
- PERMA satisfaction +7.3 pp over raw history (GPT-6 Luna; +5.8 with DeepSeek V4.1-Flash) ✓.
- Far-transfer gain over the strongest of Skill, Mem0 and TellOnce: +4.1 pp (GPT-6 Luna), +3.9 (DeepSeek) ✓.
- Its formative analysis of real SWE-chat data (1,000 records reviewed) counted 331 corrective or clarifying messages; 135 of the 331 identify a discrepancy without stating a requirement ✓ §1, Fig. 2.

*Limitations.* The user simulator is not validated against real users. Raw history wins on PERMA task completion ✓.

**Remaining papers**

| id | title | one-liner | mark |
|---|---|---|---|
| 2608.01772 | ESCROW: Guarded and Dual-Objective Continual Maintenance for Agents in Policy-Governed Enterprise Workflows | Post-deployment skill maintenance under a "Strict Update Boundary": per-category non-regression, cross-cycle anti-regression and an accuracy–cost Pareto front, run in production on BMO's internal financial document-auditing system (FinDAS) | ✓(abstract/intro) |
| 2607.10878 | LOGOS: A Living Logic for AI Agent Teams That Evolve With Humans | A governance layer: every learned prompt/skill/role is an untrusted release candidate until held-out evidence plus human authority promote it; "proposal is not promotion" | ✓(abstract/intro) |
| 2611.03868 | EvoGroup: A Self-Evolving Harness for Group Agents in Shared Contexts | Self-evolving harness for group agents (cites Claude Tag) in multi-user shared contexts (SYSU + Meituan Longcat). With DeepSeek-V4-Flash, held-out EverMemBench pass@1 rises from 71.07% (H0) to 89.58% over 5 iterations, above the Claude Code (85.83%) and Codex (82.62%) harnesses | ✓ |
| 2608.11604 | LOFA (Learning from Online User Feedback for Shopping Agents) | Shopping agent learns from real online logs: RL on verified purchases plus feedback-aware on-policy distillation of in-dialogue user directives (JD.com affiliation †) | ◐ |
| 2611.rri (alphaXiv slug) | From RSI to Human–AI Reciprocal Improvement | Position: recursive progress should include growth of human ability to propose, judge and solve (KCL) | ◐ |
| 2611.10719 | No Second Run | RSI "decomposed into smaller loops" for one frontier pre-training run, where the improved object includes the team; cites RSI Arena | ◐ |
| V7gMQjOqcP (ICLR27) | Learning from Single-Feedback Deployment Experience (DEAL/SRE) | Users rate one attempt per task; self-rewarded exploration expands snapshots anchored on live feedback | ◐ |
| UZBNBKWBCA (ICLR27) | Sleep-Time Training | Trains during user-idle periods from repaired trajectories; four-day STT-Bench with hidden preferences | ◐ |
| xSiizSlEyz (ICLR27) | Learning from Deployment for Continual Agent Adaptation | Offline OPD/RL on recorded client turns with no environment replay | ◐ |
| c3lHKtsSMq (ICLR27) | Governing Agent Self-Improvement Across Timescales (ReMo/AutoGovern) | Refinement and memory both show diminishing returns; an online governor stops them | ◐ |
| 3AR0RdXhpV (ICLR27) | READY | Certifies the cheapest oversight policy meeting a reliability target with finite-sample control; confidence does not rank errors well | ◐ |
| 4mLHxIDtDG (ICLR27) | HIVE | Allocates a limited human-review budget across workflow nodes; 32–51% lower regret | ◐ |
| ISbnaHqK5R (ICLR27) | Predicting User-Reported Problems from Coding-Agent Text | 34,046 real work-blocks; prose raises AP 0.211 → 0.258 for routing reviews | ◐ |
| Wy5lg8GnzL (ICLR27) | MemSteer | Turns unlabeled user histories into QA supervision for memory-design optimization | ◐ |
| 3aOcfmRo96 (ICLR27) | TIDE | Memory evolution from delayed, confounded recommendation feedback (clicks, conversions) | ◐ |
| sJ5JnQwRos (ICLR27) | AutoResearchClaw | 23-stage research pipeline with configurable human intervention and cross-run lessons | ◐ |
| u94T9UWCzY (ICLR27) | MemGhost | Stealthy memory injection in persistent personal agents | ◐ |
| 2610.19380 | Transfer-Aware Personal Skill Learning | Separates persistent preferences from request-specific corrections to avoid negative transfer | ◐ |
| 2611.17007 | Skill Adapter | Personalized modification and online routing of general skills | ◐ |
| 2611.13457 | Deep Personalization | Local proactive agents from continuous context observation (NVIDIA/Stanford/MIT/OpenAI) | ◐ |
| 2611.always-on-personal-intelligence (alphaXiv slug) | Always-On Personal Intelligence | Roadmap toward continuing self-evolving personal loops | ◐ |
| 2611.10439 | Who Remembers What? | Multi-granularity memory for multi-user, multi-agent settings (Amazon) | ◐ |
| 2610.04195 | MemLeak | Cross-user semantic leakage in shared multi-tenant memory (Workday) | ◐ |
| 2608.00122 | Shared Organizational Memory (SAP) | Deployment snapshot of org-level memory for enterprise coding agents | ◐ |
| 2610.36188 | Skill Dreaming | Evolves repository skills from pull-request streams | ◐ |
| 2609.32965 / 2609.34392 | Relic / Org-Agent | Persistent organizational capability; agents serving multiple users | ◐ |
| 2610.35835 | I-HITL: Inalienable Human-in-the-Loop | "Inalienable" human-in-the-loop approval that agents cannot bypass | ◐ (id→title ✓) |
| 2609.21081 | Loopjacking: Hijacking Human-in-the-Loop Approval | The operation shown for human approval differs from the one executed | ◐ (id→title ✓) |
| 2610.09043 | Careful Judge | Learns from human interventions after abstention to cut review cost | ◐ |
| 2611.08781 | Aligning, Fast and Slow | Control-theoretic oversight coupling trajectory control with maintained-deployment drift | ◐ |
| 2611.16378 | Evidence-Governed Adoption | Persistence decisions for self-generated updates based on evidence | ◐ |
| 2610.26662 | AllPath | Layered verification for a deployed self-revising trading agent, human-gated versus autonomous regimes | ◐ |
| 2611.22299 | Harnesses That Improve Themselves: A Unifying Framework for Self-Improving Agent Harnesses (Google) | Unifying framework for self-improving harnesses | ◐ (title/affiliation ✓) |
| 2606.01770 | Adaptive Auto-Harness | Sustained harness self-improvement on open-ended task streams (Emory/Amazon) | ◐ |
| 2607.01120 | Next-Gen Agentic RL Systems (Ant) | Systems view of online RL for deployed agents | ◐ |
| 2609.04141 | Efficient Test-Time Adaptation through Human-Agent Interaction | Adapting artifacts to a professional's personal bar via interaction | ◐ |
| 2610.16129 | PID-Gated Continual Prompt Adaptation | Damps rewrites driven by noisy user error reports | ◐ |
| 2610.26636 | Revision-Based Learning from Interactive Feedback | Learns from user revisions of interactive artifacts | ◐ |
| 2610.35488 | Coding Agents under Evolving Requirements | Benchmark counting repeated user guidance | ◐ |
| 2611.23067 / uuvpiQaG6k | Values Shape What Self-Improving Coding Agents Build | Designer-specified values steer how self-improvers work | ◐ |
| 2610.06824 | TasteVal | Listed in round-1 under human co-improvement; not re-read | † |

### Consensus, debates & contradictions

**Consensus.**
1. *Proposal ≠ promotion.* Every production system keeps a human or a fixed evaluator at promotion and treats self-generated changes as release candidates. Examples: CARE's parity + online + human-approval gate ✓; LinkedIn's staged rollout and rollback ✓; ESCROW ("only an empirically evaluated version is deployed") and LOGOS ✓(abstract/intro); Reglos's frozen evaluator ✓. Recoverability makes this a measurable rule ✓.
2. *Independence of the judge.* CARE never lets the deployed model judge its own conversations ✓. Reglos uses a second-family critic ✓. LinkedIn separates generator and judge models ✓.
3. *Attribute before you learn.* CARE routes non-model failures to component repair so the model is not taught to work around broken tools ✓. LinkedIn localizes failures to components ✓.
4. *Stable on-policy targets for small increments.* CARE's OPSD succeeded where SFT and DPO collapsed ✓. The ICLR 2027 papers converge on OPD/self-distillation from deployment traces ◐.

**Debates.**
- **Do humans need to specify the loop?**
  - Aligning Autoresearch: without specific human direction, autoresearch produces saturated benchmarks ✓.
  - 2611.23219: frontier models self-organize improvement procedures that match GEPA on real data ✓.
  - Reconciliation supported by both: humans are decisive for the *objective/construct and the grounding material*, much less for the *procedure*. Over-informing the self-improver causes overfitting ✓ (2611.23219 §5.2).
- **Is human feedback the signal or a Goodhart target?**
  - RSI Arena shows real gains alongside stylistic exploitation: the length coefficient more than doubled (0.17 → 0.39) and one agent explicitly treated length as the lever ✓.
  - Aligning Autoresearch shows LLM judges prefer model text over expert human text 63.5–84.6% of the time ✓. Replacing humans with judges inherits that bias.
  - LinkedIn reports 87% judge–human alignment ✓, which leaves a 13% error channel inside a weekly evolutionary loop.
- **What counts as "self" / RSI?**
  - CARE explicitly disclaims autonomy ✓.
  - Reglos calls a benchmark-gated harness search "RSI" ✓.
  - RSI Arena's agents improve a *different* model ✓.
  - By Duan's structural-recursion criterion, none of these is recursive. The field is using "RSI" for deployment-time continual improvement.

**Contradictions and weak spots.**
- PaperClaw's text says "tie at 7.0", but Table 4 shows 6.5 against 7.0 ✓.
- RSI Arena's "8/10 improved" is confounded by extra GPU time and a different arena population, and has no no-feedback control ✓.
- LinkedIn's +30.6 pp online gain bundles three components ✓.
- CARE's version trend overlaps with harness fixes ✓.
- CARE claims grounding "keeps the loop from collapsing" ✓ §9.3, yet attribution itself is model-assisted and its accuracy is unmeasured ✓ §9.4. Masked Erosion († 2610.38392) and Recoverability's 9/37 constant-judge finding ✓ show that gates can silently fail.

### Open problems

1. **Causal value of human feedback at matched compute.** We found no study that compares real votes with shuffled or placebo votes at equal GPU and time budgets (searched S2 and ICLR 2027 OpenReview on 2026-11-19). RSI Arena admits the confound ✓. A Stage-2 design with {real, shuffled, length-only, no feedback} × matched budget would settle it.
2. **The feedback exchange rate.** How many votes or corrections buy how much held-out improvement? RSI Arena collected roughly 1,300 accepted votes per stage, and the needed amount is an "open question" ✓. Aligning Autoresearch did not measure human effort ✓. A dose–response curve (votes or minutes of guidance → Δ) is missing.
3. **Authorization conflation and feedback poisoning.** Benign appreciation raises injection success by up to 48.8 pp ✓. Defenses beyond delimiting (roughly halving) and counterexample feedback (35.0 → 5.8%) are untested on real users and over long horizons.
4. **Attribution and judge validation.** CARE's attributor accuracy is unreported ✓. Verifier strength drifts and gates can go constant ✓. No production system reports σ over time.
5. **Cross-version and policy-change generalization.** CARE has never measured following an earlier or later SOP ✓. G-CAST shows the reward must evolve with the contract ✓, but no theory says when.
6. **Whose feedback in team agents?** EvoGroup ✓, Who Remembers What and Org-Agent ◐ store multi-user context. Reglos finds that preferences belonging to someone other than the user are below chance for *every* system ✓, and MemLeak shows cross-user leakage ◐. Conflict resolution between users' feedback is unstudied.
7. **Oversight budget for self-modifications, not only actions.** HIVE, READY and Careful Judge allocate review to *actions* ◐. None allocates review to *proposed self-edits* by expected blast radius β ✓(definition) or by verifier strength.
8. **Irreversible effects.** Deployed changes altered 17% of outcomes beyond noise, and rollback cannot undo them ✓. Weight updates without kept checkpoints should require human sign-off ✓. Safe canary sizes for weight-level production RSI are not quantified.
9. **No order-1 evidence in deployment.** Every production loop fixes the improver: CARE's external model ✓, LinkedIn's GA ✓, Reglos's proposer and critic ✓. Whether the inspection/attribution/repair stack can improve itself under the same grounding is open. CARE lists evaluator co-evolution as future work ✓ §9.6.
10. **Replicability.** Every headline result is a single run or a single deployment: CARE, LinkedIn, RSI Arena (one run per agent), Learning on the Job (single run per condition) ✓.
11. **User-simulator fidelity.** LiveAsset, AutoInteract and STT rely on simulated users. StressSim († 2610.35426) shows over-cooperative simulators inflate agents. No paper validates simulator-trained self-improvement against real users.

### Where it is heading (to mid-2027)

These are forecasts, not findings.
- **More industrial reports with online A/B tests.** Meituan teams are on three of these papers: CARE (Keeta/Meituan with Melbourne and Fudan), ClawLoop (Meituan with PKU), and EvoGroup (Meituan Longcat with Sun Yat-sen) (✓ affiliations). Expect weight-level "replace-the-incumbent" RSI reports from other platforms, with cost and latency as headline metrics, and OPSD with privileged hindsight as the default learner.
- **RSIHF as a named benchmark line.** Likely: repeated RSI Arena rounds, multiple seeds, models that do their own next round of research, and pre-registered style-controlled ratings. The live question is whether gains survive a placebo-feedback arm.
- **Governance standardization.** Typed changes by recoverability, measured verifier strength, versioned artifacts and human sign-off for irreversible weight updates are converging (Recoverability ✓, LOGOS/ESCROW ✓(abstract/intro)). Expect audits that report σ and β over time.
- **Security as the brake on personal self-evolution.** IPI degradation ✓, MemGhost ◐ and Loopjacking ◐ will push toward authorization-aware feedback learning and memory provenance.
- **ICLR 2027 decisions (late January 2027 †).** These will tell whether learning-from-deployment (DEAL, STT, xSiizSlEyz) and oversight certification (READY, HIVE) are accepted as core ML rather than systems work.
- **A split between "humans set objectives" and "models set procedures".** Supported by 2610.38882 versus 2611.23219, this split will probably organize position papers (RRI ◐).

### Connection to our ideas

(Idea numbering follows `/home/user/nlp-lab-paper-search/research/ONE_PAGERS.md`.)

- **#1 Taught, Not Selected (most direct link).** Deployment papers supply natural, real instances of the *reading/teaching* channel:
  - The Appreciation paper's evolved skill "execute each instruction exactly once" is a false rule *taught* by honest positive feedback ✓.
  - An RSI Arena agent read the votes and concluded that length was the lever ✓.
  - CARE's "hindsight visible only to the teacher" and evidence-blinding are production-grade *trace hygiene* ✓.

  Suggestion: add a "biased user feedback" corruption arm (for example, a length-preferring or authorization-conflating simulated user) to the trace-source × score-source 2×2. Cite CARE as the industrial precedent for controlling what the learner reads.
- **#5 Order-0 Rewards Breed Routers.** RSI Arena is a field example of an order-0 preference objective rewarding a stylistic shortcut over capability: benchmark Δ against human Δ is ρ = −0.19 ✓. Use it as motivation.
- **#7 Varying Goals / #9 MetaShift.** These papers give real-world evidence for both kinds of change:
  - CARE's SOP revisions are *task change*. Its model must follow whatever policy is in its input, and cross-version testing is missing ✓.
  - G-CAST's capability-contract expansion forces the *reward itself* to evolve. That is a regime change in the rules of improving, which is MetaShift's exact premise ✓.

  Cite G-CAST as industrial motivation for MetaShift. CARE's untested cross-SOP evaluation is a gap a DriftStream-style benchmark could fill.
- **#3 Gate, Not Willingness / 2+3 merged.** Production systems add gate types to the funnel study's arm list:
  - human approval (CARE ✓);
  - parity-on-combined-score (CARE ✓);
  - per-half held-out gain with a second-reader constraint (Reglos ✓);
  - per-category non-regression (ESCROW ✓(abstract/intro));
  - the recoverability rule (2610.27465 ✓).

  All of them gate *object-level* changes. No deployed system lets the improver edit itself, which strengthens the "self-edit drought" framing: in production the drought is by design.
- **#10 Register Snowball.** Human-feedback-driven length and format drift is measured in RSI Arena: the DeepSeek-trained model's median answer went from 61 to 508 tokens, and the length coefficient went from 0.17 to 0.39 ✓. This is the closest published evidence that human preference loops amplify register. It supports adding the real-human arm and the scripted-uptake user.
- **Practical caution for Dan.** Real-user loops (RSI Arena's arena, CARE's production traffic) are out of reach for an undergraduate project. Use learned user simulators (AutoInteract builds them from SWE-Chat ✓), but report simulator fidelity (StressSim †) and include a small real-human arm, as RSI Arena and Register Snowball both suggest.

#### Fact-check log

All "✓ full text" checks are against cached pdftotext files under `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/<id>/paper.txt`, re-opened on 2026-11-19.

| claim | verdict | source location |
|---|---|---|
| CARE: Bad Case 6.42% → 1.97% vs incumbent 4.60%; sim. acc. 93.0 → 94.1 (peak 95.30 V3, incumbent 96.80); combined 93.29 → 96.07 vs 96.10 | confirmed | 2611.02839 abstract; §8.2 table (V0–V5 rows); §8.3 table |
| CARE: online Δ −4.57 / −8.02 / +0.34 (p=0.85) / +1.73 (p=0.47); latency −27.7%; cost/order −70.5% | confirmed | 2611.02839 §8.2 table and text; §8.5 table ("Reduction (%)" row) |
| CARE: 280 repair tuples; SFT collapses; DPO 15.1% / 56.61; OPSD 94.1 / 96.07 | confirmed | 2611.02839 §8.6–8.7 tables; App. E ("280 repair tuples") |
| CARE: "self-improving in its weights … not autonomous"; external frontier model; person approves each promotion | confirmed; section §3.3 confirmed | 2611.02839 §3.3 "What the loop depends on" |
| CARE: attribution accuracy not reported; cross-SOP "argued rather than measured"; evaluator co-evolution as future work | confirmed (§9.4, §9.6) | 2611.02839 §9.4 Limitations; §9.6 "Toward a more closed loop" |
| CARE: "Seven regions (SA, BR, HK, GCC4)" | corrected (seven regions in four groups; GCC4 = AE/BH/KW/QA); affiliation made explicit as Keeta/Meituan | 2611.02839 §4.2, §8.1 |
| RSI Arena: 76% median gap (12 capability benchmarks); 1,095 vs 1,094; ρ = 0.73; WildBench 0.95; 72% of 999 decisive votes; compute ρ = −0.21; three models worse on StrongREJECT | confirmed | 2610.33624 §1 findings; §5.1–5.4; Table (agents) |
| RSI Arena Stage 2: 8/10 narrowed, median +28; DeepSeek −207 → −35, 61 → 508 tokens; median bench +2; ρ = −0.19; MiniMax StrongREJECT 2.6 → 24.5; length coef. 0.169 → 0.394; 213–276 battles | confirmed | 2610.33624 §1 Stage-2 findings; §6.4–6.5; style-coefficient table §3.3 |
| RSI Arena: "do not isolate the effect of human feedback"; "only the lever"; ~1,300 votes per stage, amount "open question"; host-memory cap | confirmed (§7, §7, §7, §4.3) | 2610.33624 §7 Discussion; §4.3 Engineering problems |
| RSI Arena: arena "at COLM 2026" | corrected ("at COLM 2026 and online") | 2610.33624 §1 |
| Aligning Autoresearch: Opus-5 98.0 autonomous; fine-grained −28 to −46.5 in 6/6 pairs; SilentTrain fork 64.2 vs 88.1; HackBench 169 traj., F1 64.88; AutoInteract 6.0 → 20.6; judges 63.5–84.6%; −4.2 → +2.76 | confirmed | 2610.38882 Table 1; §1.4; §2 (HackBench); AutoInteract table; RL-XAR section |
| Aligning Autoresearch: coarse feedback "about 0 on Rebuttal Bench" | corrected (authors: "essentially flat"; Muse Glimmer 0.3, Nemotron 1 point worse, Muse Spark 90 → 84.4) | 2610.38882 §1.4 |
| LinkedIn: <0.1% vs 4.8–6.2%; 62.6 → 68.0; 87% / 67%; +9.0 pp [8.4, 9.6]; +4.8; 38.2 → 68.8 (+30.6); two-week A/B with Holm; GPT-4o-mini / GPT-4.1; bundled treatment | confirmed | 2608.10224 abstract; Tables 1–3; §5 online table and limitations |
| Appreciation→IPI: +14.0–48.8 pp (skill) / +3.0–39.3 pp (RL); GPT-OSS-120B 4.7 → 53.5% after 12 episodes; 35.0% vs 35.8% read-only control; 5.8% non-execution reward; delimiting roughly halves | confirmed | 2610.39122 abstract; §1; main results table |
| Appreciation→IPI: "4 models (GPT-OSS-20B/120B, Qwen3.8-27B and others)" | corrected (fourth model is Inkling-Small; GPT-OSS-20B used only under skill evolution) | 2610.39122 §4 setup |
| Recoverability: 9/37 constant gate; regenerated undo fails, saved copy restores; 17% beyond noise; σ_min ±0.15 | confirmed | 2610.27465 abstract; §3; §5 |
| Recoverability: "weight updates need human sign-off" | corrected (only weight updates with no retained checkpoint, g=1; a retained checkpoint/adapter is g=0) | 2610.27465 §2 Definitions; §3; App. (g=0 for adapters) |
| Reglos: L2 harness / L4 memory; Opus 5.5 proposer; GPT-6.1 Sol critic; 1,317 Qs; second reader ≤1 pt | confirmed (§3.4) | 2611.06049 §3.4 Outer Loop |
| Reglos: 61.3 vs 53.6/53.8/49.3; LME-S 84.0; 54.2 → 60.7 in four steps; GEPA +3.8/+1.1; 159 failures 48%/31%; 19.0–20.5% on 522 Qs; real user events as next step | confirmed | 2611.06049 Table 1; Table 3; Table 5; §4.1; "Next steps" |
| 2611.23219: six models; 3 seeds; +20/+10/+6%; GEPA on 20 real games; +9% / +21% / +0.15 SPL; overfitting §5.2; 25.4 → 47.5 | confirmed | 2611.23219 §3.4; §4.1; Fig. 3; §5.1; §5.2; Table 1 |
| PaperClaw: Table 3 7.0 vs 4.8 vs 4.3; Table 4 6.5 vs 7.0 while text says "tie at 7.0"; stalled run 3.0; single Claude judge | confirmed | 2606.22610 Table 3; Table 4 caption; §4.3 |
| ClawLoop: 46.3 → 57.2 (ClawGym 52.6); 69.3 → 76.9; 67.6 → 68.4; 4B 32.4 → 48.0; 2,375 GPU-h on 32×H800; K=3; 1,024 tasks; N=8 | confirmed | 2610.14118 Table 1; scale table; App. Table 4 |
| G-CAST: 0.8078 vs 0.7911; +0.012 vs −0.017; 94/500 vs 1,200; four 100-query periods; Qwen3-4B-Thinking | confirmed | 2611.17814 Table 2; §5 text |
| Learning on the Job: 0.064 → 0.103 → 0.170; 13 → 32 of 97; Sonnet 0.248 → 0.397; noise out of scope | confirmed | 2607.22157 Table 1; §4; §5 ("outside this paper's scope") |
| LiveAsset: 0.933 vs 0.721; PERMA +7.3 pp; far-transfer +4.1 pp | confirmed (far-transfer baseline set clarified: strongest of Skill/Mem0/TellOnce) | 2611.11560 Table 1; §4 text; π-Bench table |
| LiveAsset: "331 corrective or clarifying messages among 1,000 sampled" | corrected (331 messages counted from 1,000 records reviewed; key finding is 135/331 state no requirement) | 2611.11560 §1; Fig. 2 caption |
| EvoGroup: "pass@1 from 71.07% rising over 6 iterations" (◐) | corrected (5 iterations; held-out 71.07 → 89.58%; Claude Code 85.83%, Codex 82.62%); upgraded to ✓ | 2611.03868 abstract; Fig. 1 caption; Table 1 |
| Forecast: "Meituan alone authored CARE and ClawLoop" | corrected (CARE with Melbourne/Fudan; ClawLoop with PKU; EvoGroup Meituan Longcat with SYSU) | author blocks of 2611.02839, 2610.14118, 2611.03868 |
| ESCROW: production FinDAS; Strict Update Boundary; per-category non-regression; Pareto | confirmed (BMO Financial Group) | 2608.01772 abstract; §1 |
| LOGOS: "proposal is not promotion"; human authority | confirmed | 2607.10878 abstract; §1 |
| Ren et al.: unquantified human help "fundamentally compromises the 'self'" († in draft) | confirmed (upgraded to ✓; wording: "oversight fundamentally compromises the 'self' in self-improvement") | 2607.13104 paper.txt l.3360 |
| Duan 2609.11873: L1–L5 ladder; "autonomy attribution" open problem | confirmed | 2609.11873 Fig. (B0–L5); §2 bullets; §3.6 |
| "First industrial reports with online A/B" (Aug) and "first evidence that feedback-driven evolution degrades security" (Oct) | unverifiable as absolute "first"; hedged to "earliest in this corpus" | corpus-level claim |
| "No study isolates the causal value of human feedback at matched compute" | unverifiable as universal; no counterexample found in S2 + ICLR27 OpenReview search ("placebo feedback self-improvement"); hedged | S2 search; OpenReview search 2026-11-19 |
| ID → title: 2606.22610 = "PaperClaw: Harnessing Agents for Autonomous Research and Human-in-the-Loop Refinement"; 2608.11604 = "Learning from Online User Feedback for Shopping Agents" (LOFA); 2610.35835 = "I-HITL: Inalienable Human-in-the-Loop"; 2609.21081 = "Loopjacking: Hijacking Human-in-the-Loop Approval" | confirmed (all resolve) | export.arxiv.org API |
| ID format: 2611.rri, 2611.always-on-personal-intelligence, 2611.recursive-self-improvement-llm-agents | corrected (flagged as alphaXiv slugs, not arXiv ids; need real ids) | project atlas partials r4/r21/r23 |
| LOFA affiliation JD.com | unverifiable (not in arXiv abstract or metadata); marked † | export.arxiv.org API record |
| Dates: CARE 11-02, RSI Arena 10-24, Aligning 10-28, LinkedIn 08-10, IPI 10-29, Recoverability 10-20, Reglos 11-04, 2611.23219 11-17, PaperClaw 06-21, ClawLoop 10-09, G-CAST 11-13, LotJ 07-24, LiveAsset 11-08 | confirmed | arXiv stamp line in each paper.txt |

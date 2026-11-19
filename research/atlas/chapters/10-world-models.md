## World models & simulators for LLM agents

*Marks: ✓ = I checked this in the full text this session (section or table cited). ✓ᵖ = checked in full text in an earlier session (see /home/user/nlp-lab-paper-search/research/FULLTEXT_VERIFICATION.md). ◐ = abstract only (arXiv, alphaXiv or OpenReview). † = not verified.*

*Full texts are cached at /tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/<id>/paper.txt. This session I downloaded 15 of them: 2602.10090, 2602.05842, 2606.24597, 2608.12564, 2609.14858, 2609.33335, 2611.01139, 2610.08033, 2610.06100, 2610.13912, 2610.12789, 2609.26293, 2610.09484, 2610.26828 and 2511.03773. The fact-check pass re-read 15 of the cached texts (log at the end).*

### At a glance

- **What this corner is.** It covers anything that stands in for the real environment of an LLM agent. Inside it, the agent can be trained (RL, SFT), evaluated, planned, or self-improved. There are five kinds:
  - code-synthesized executable environments;
  - learned language world models (LWMs) that predict the next observation;
  - LLM user simulators;
  - world-model objectives used as policy training signals;
  - outcome or reward predictors that replace execution inside search and RSI loops.
- **State of the art, as of Nov 2026.**
  - *Executable synthesis* has scaled up:
    - AWM has 1,000 SQL-backed MCP environments (ICML 2026) ✓.
    - WildAWM has 341 long-horizon GUI+MCP+CLI worlds that GPT-6-Astra solves only about 60% of ✓ (abstract, §1).
  - *Learned LWMs* now beat frontier models on simulation-fidelity rubrics. Qwen-AgentWorld-397B scores 58.71 vs GPT-5.4's 58.25 on AgentWorldBench ✓. Controlled Sim-RL inside it matched or beat live-engine RL on WideSearch (50.3 vs 45.6 F1 by Item) ✓.
  - *In RSI and search loops*, simulators and predictors cut cost:
    - WMRL trains 3.1–3.4× faster and still scores higher on every benchmark ✓.
    - WRM cuts mean task time 4.4–7.5× ✓.
    - Dream-RSI raises a TB2.1 harness from 0.7247 to 0.8090 ✓.
- **Consensus 1: success inside the simulator is not evidence.** Three papers show it from different angles:
  - Trace2Env (ALFWorld): with a prompted LWM, agents "succeed" 97% in-sim (93% in the real environment), but only 3% of those action sequences succeed when replayed in reality ✓ (Table 2).
  - Learning in Dreams: no in-dream metric tracks real collapse (|r| ≤ 0.37) ✓.
  - Simulator Collapse: single-simulator RL peaks and then decays back toward base ✓ᵖ.
- **Consensus 2: some real anchoring is necessary.** Examples:
  - WMRL's roughly 10% real "anchor groups" ✓;
  - Learning in Dreams' real-game KL anchor and Dyna repair (+9.2 pp) ✓;
  - RAVEL's held-out real audits ✓;
  - Dual-Frontier's verify-then-promote rule;
  - Qwen-AgentWorld's finding that uncontrolled Sim-RL gives no gain ✓.
- **Consensus 3: prediction accuracy is the wrong fidelity metric.** What matters is decision-relevant fidelity. The evidence:
  - The world-model post-training audit: across three settings, mismatched next-observation targets cut prediction accuracy by 15.3–61.6%, yet the gains largely remain ✓.
  - DRPE ✓ᵖ, WRM ✓ and 2611.01139 ✓ make related points.
  - A τ-bench study with real users shows frontier simulators *inflate* agent success from 63.6% to 82.4–84.4% ✓ (MIMESIS §1, Fig. 2).
- **Biggest open problem.** Nobody has a cheap, validated, *decision-anchored* fidelity measure that predicts how much a given optimizer will exploit a simulator, and how that changes with optimization depth †(absence claim). This matters most for harness or prompt-level self-improvement, as opposed to weight RL. The audits so far are in non-LLM control or game settings (2611.01139, Goodhart in the Planner, Learning in Dreams), or they test static selection only. The one LLM audit with an optimizer, The Simulator Says Yes ◐, measures a single error type (refusals) under weight RL.
- **Survey framings to reuse.**
  - Agentic World Modeling (2604.22748, "Agentic World Modeling: Foundations, Capabilities, Laws, and Beyond" ◐; title checked this session) uses L1 Predictor → L2 Simulator → L3 Evolver × four law regimes (physical, digital, social, scientific).
  - Text World Models (2606.09032 ◐) uses LLM-as-WM vs code-as-WM × training-time vs inference-time use.
  - Ren et al. 2607.13104 §5.3 ✓ᵖ splits extrinsic experience into "grounded task environments" and "simulated proxy environments". It names "hallucinated dynamics … the policy then learns to exploit" as the signature risk.
  - Duan et al. 2609.11873 ✓ᵖ lists "8.3 Environment/Simulator" and "8.4 World Model" as updatable RSI components (Appendix taxonomy). Under embodied RSI it cites World-VLA-Loop (§4.2).

### How it developed

| Month | Paper | What changed |
|---|---|---|
| 2025-04 | WebEvolver 2504.21024 ◐ | A web world model co-evolves with the agent for self-improvement. Early LLM-WM-in-the-loop work. |
| 2025-11 | DreamGym 2511.03773 ✓(abs) | A "reasoning experience model" replaces rollouts. Reports >30% over all baselines on non-RL-ready WebArena. The sim-to-real variant uses <10% of the external (real) data. |
| 2026-02 | Agent World Model 2602.10090 (ICML'26) ✓ | 1,000 executable, DB-backed environments. Beats GPT-5-as-simulator RL. Executable beats simulated. |
| 2026-02 | RWML 2602.05842 (NeurIPS'26) ✓ | Next-state prediction reward measured in embedding space. Combined with policy RL, it gives +6.9 (ALFWorld) and +5.7 (τ²) over policy RL alone. |
| 2026-04/06 | Agentic WM survey 2604.22748; TWM survey 2606.09032 ◐ | Taxonomies (L1/L2/L3; LLM-as-WM vs code-as-WM). |
| 2026-06 | Qwen-AgentWorld 2606.24597 ✓ | An industrial LWM trained on more than 10M trajectories across 7 domains. Introduces AgentWorldBench. Controlled Sim-RL beats real RL. |
| 2026-08 | Simulator Collapse 2608.12253 ✓ᵖ | A mode-collapsed user simulator drives the policy into exploiting it. Co-training a simulator population fixes it. |
| 2026-08 | WMRL 2608.12564 ✓ | A world model replaces execution in AutoResearch RL. Isotonic debiasing on about 10% real anchors. |
| 2026-09 | Dream-RSI 2609.14858 (ICLR'27 sub) ✓ | Discovery history serves as a replay simulator for *meta-exploration* RSI. |
| 2026-09 | DRPE 2609.32322 ✓ᵖ; Dual-Frontier 2609.26293 ✓(abs+§1); WM audit 2609.33335 ✓ | Error *type* matters more than error rate. Trust gating. Gains do not come from prediction content. |
| 2026-10 | Learning in Dreams 2610.08033 ✓; Trace2Env 2610.06100 ✓; MIMESIS 2610.09484 ✓(abs+§1); EvoBelief 2610.12789 ✓; RSI-Anything 2610.13912 ✓ | Fidelity is inherited by the policy. Trace-to-world replay validity. Purpose-built user simulators. Falsifiable belief WMs. A WM inside an RSI loop. |
| 2026-10 | WildAWM 2610.26828 ✓(abs+§5, Table 5); WRM 2610.38439 ✓; AWM-TTS 2610.40927 ✓(abs; title checked) | Computer-use environment synthesis. "Predict the reward, not the world". Read-only-grounded agentic simulation for test-time scaling. |
| 2026-11 | 2611.01139 ✓; Goodhart in the Planner 2611.09670 ✓ᵖ; CUA+tutorials 2611.04073 ✓(abs+Fig. 1; title checked); SIRIS 2611.10329 ◐ | Ranking ≠ audit value ≠ exploitation safety. Search pressure exploits accurate models. Oracle rollouts add almost nothing once tutorials are given. |
| 2026-11 (ICLR'27) | The Simulator Says Yes (0cb6zfwHm7, desk-rejected) ◐; ReplayWorld (g8RTu7QVFb) ◐; EnvSimBench (vJbjG3bWI7, withdrawn) ◐; Quantifying Utility of User Sims (OplgHiAcOP) ◐; Behavioral Fidelity vs Policy Selection (riKw4M6myC, withdrawn) ◐; PROWL-2 (jQDbBMncVh) ◐; EAU (Z5DVLWI7en) ◐; Agent-Editing WM (g6YsNCtLEk) ◐ | The audit and fidelity-validation wave reaches the main conferences. |

### Taxonomy

| Approach | What is simulated / learned | Representative papers |
|---|---|---|
| A. Code-synthesized executable environments | Real code and DB state; the LLM writes the environment | AWM 2602.10090; WildAWM 2610.26828; EnvScaler (in AWM Table 4); ToolSpace 2611.05036 ◐; CrossEnv 2611.22777 ◐; repo→env evolution loop 2611.21152 ◐; API World Model (2Q6rInlW9X) ◐ |
| B. Learned / prompted language world models as simulators | The LLM predicts the next observation | Qwen-AgentWorld 2606.24597; DreamGym 2511.03773; WebEvolver 2504.21024; Trace2Env 2610.06100; ReplayWorld ◐; AdvSim2Real 2610.08773 ◐; DreamUI ◐ |
| B′. Read-grounded agentic simulators | The simulator queries the real environment read-only | AWM-TTS 2610.40927; Trace2Env (worldbook) |
| C. User simulators | A human interlocutor | τ²-bench user simulator; MIMESIS 2610.09484; Simulator Collapse 2608.12253; Quantifying Utility (OplgHiAcOP); Not Every User Says It All 2610.24294 ◐; ShopMind 2611.11897 ◐; SWE-WhatIf 2611.21095 ◐; EAU ◐ |
| D. WM as a policy training objective | Next-state prediction as reward, warm-up or auxiliary loss | RWML 2602.05842; Qwen-AgentWorld "unify" mode; CoMAP 2606.02372 †; Agent-Editing WM ◐; *critique:* WM audit 2609.33335 |
| E. WM for test-time planning and selection | Look-ahead rollouts with a judge | AWM-TTS 2610.40927; R-WoM / RAG-PJ 2611.04073; Dual-Frontier 2609.26293; WebDreamer † |
| F. Outcome / reward predictors replacing execution in search and RSI | P(win) or score of a candidate program, harness or edit | WMRL 2608.12564; WRM/ForeCritic 2610.38439; Dream-RSI 2609.14858; RSI-Anything 2610.13912; IDM 2610.13991 (see self-update chapter); SpecRSI 2610.16491; Rehearse 2607.27687; LMs as AI-Research WMs 2610.12235 |
| G. Self-evolving belief / world knowledge | Editable rules or worldbook revised from evidence | EvoBelief 2610.12789; WorldEvolver 2606.30639 † (used as a baseline in EvoBelief Table 1 ✓); Memento 3 2610.11794 †; ReplayWorld skill evolution ◐; FASim (Iin8ijZhoS) ◐ |
| H. Fidelity theory and audits | What error matters, and when it is exploited | 2611.01139; DRPE 2609.32322; Goodhart in the Planner 2611.09670; Learning in Dreams 2610.08033; WM audit 2609.33335; Simulator Says Yes ◐; EnvSimBench ◐; SIRIS 2611.10329 ◐; Behavioral-Fidelity-vs-Selection ◐; Events not Pixels 2610.29435 ◐; Posterior-Prior Epistemic Gap 2611.14955 ◐; PROWL-2 ◐ |

### Paper cards

**1. Qwen-AgentWorld: Language World Models for General Agents** ([2606.24597](https://arxiv.org/abs/2606.24597), 2026-06-23 (PDF header dated 2026-06-24), Qwen tech report)
- *What it does.* Builds native LWMs (35B-A3B and 397B-A17B) for 7 domains (MCP, Search, Terminal, SWE, Android, Web, OS) with a CPT → SFT → RL pipeline. Each LWM is used in two ways: as a decoupled simulator for Sim-RL, and as a warm-up for the agent's own foundation model.
- *Setup.*
  - Trained on more than 10M real environment-interaction trajectories ✓ (abstract).
  - AgentWorldBench is built from 5 frontier models' interactions on 9 benchmark query sets ✓ (§ benchmark construction) and scored with a 5-dimension rubric normalized to 0–100.
  - Sim-RL policy: Qwen3.5-35B-A3B, up to 50 turns, Avg@3 ✓ᵖ.
- *Key results.*
  - Overall AgentWorldBench: 397B 58.71 vs GPT-5.4 58.25 ✓ (Table 5). The 35B model rose from 47.73 to 56.39 (+8.66) ✓. On GUI domains the 397B ranks only 5th (59.69) ✓.
  - Simulated OpenClaw (4k environments, out of distribution): Claw-Eval 65.4 → 69.7, QwenClawBench 47.9 → 55.0. With Qwen3.6-Plus as simulator, ≈no gain (66.7 / 47.8) ✓ (Table 6).
  - MCP: *uncontrolled* Sim-RL gives no gain, and Tool Decathlon drops from 32.4 to 31.5. Controlled simulation gives +3.7 on Tool Decathlon and +12.3 on MCPMark ✓ (§6.1.2, Table 7).
  - Fictional-world search Sim-RL: WideSearch F1 by Item 34.02 → 50.31 (+16.29) at 35B and +3.87 at 397B. Live-engine Real RL reaches 45.6 vs 50.3 for Sim-RL ✓ (Table 8, Fig. 9).
  - Using the LWM as warm-up: +11.3 Claw-Eval (53.6 → 64.9), +9.7 QwenClawBench (39.8 → 49.4), +9.0 BFCL v4 (62.3 → 71.3) ✓.
- *Limitations / critiques.*
  - Single runs, with no seeds or variance reported.
  - Fidelity is judged by an LLM rubric.
  - "State is the bottleneck": without a detailed initial state, gains shrink (§6.1 takeaways ✓ᵖ).
  - The ICLR'27 submission "The Simulator Says Yes" ◐ (abstract re-read this session) reports that the released 35B model *whitewashes* 25% of the 399 held-out actions the real executor refused, i.e. it narrates success. That is exactly the failure an optimizer can exploit.
  - The paper's own future work is Agent–LWM co-evolution and sim-to-real routing.

**2. Agent World Model: Infinity Synthetic Environments for Agentic RL** ([2602.10090](https://arxiv.org/abs/2602.10090), 2026-02, v3 May 2026, ICML 2026)
- *What it does.* GPT-5 writes, for each environment: scenario → tasks → SQL schema and data → MCP Python interface → verification code. The result is 1,000 environments and 10,000 tasks. GRPO is then run on them with a code-augmented LLM judge as reward.
- *Setup.* Qwen3 4B/8B/14B thinking models, trained on a subset of 526 environments ✓. Out-of-distribution evaluation on BFCLv3, τ²-bench (Pass@1 averaged over 4 runs ✓ᵖ) and MCP-Universe. Baselines: "Simulator" (GPT-5 as the transition model, same tasks) and EnvScaler.
- *Key results.* All ✓ (Table 4, Table 6):
  - BFCLv3 overall (8B): 53.83 → 65.94. The Simulator baseline gets 52.53.
  - τ² Pass@1 (8B): 26.44 → 33.45, but EnvScaler is higher at 39.39.
  - MCP-Universe (8B): 6.70 → 11.17.
  - At 4B on τ², the Simulator baseline *drops* below base (15.83 → 13.67).
  - Quality (Table 6, 100 sampled environments, two LLM judges): with the GPT-5.1 judge, 74% of AWM environments have bugs and 14.0% of tasks are blocked, vs 88% and 57.1% for EnvScaler. **Corrected:** with the Claude-4.5-Sonnet judge, bug rates tie (83% vs 83%) and blocked tasks are 11.5% vs 46.8%.
- *Limitations.* The authors' own list (Limitations ✓ᵖ):
  - a fixed generator ("incorporating a self-evolving paradigm… future work");
  - self-correction only fixes runtime errors;
  - Qwen3 models only.

  Absolute gains on τ² and MCP-Universe are small, and EnvScaler beats it on τ².

**3. Does Learning to Predict the World Help Agents Act? Auditing World-Model Post-Training** ([2609.33335](https://arxiv.org/abs/2609.33335), 2026-09-27, preprint; ICLR'27 sub WXMQxacf7I per round-2 sweep)
- *What it does.* Placebo-controls RWML-style training with three arms:
  - GT: true next observation.
  - MIS: an in-distribution *mismatched* observation.
  - COIN: a Bernoulli(0.5) reward.
- *Setup.* Qwen2.5-7B-Instruct ✓ on ALFWorld (with and without the action list) and ScienceWorld, plus privileged OPSD distillation and VisualWebArena. 64 samples per task. Prediction accuracy is scored by a DeepSeek-V4-Flash judge (temperature 0) on 1,200 held-out transitions ✓ (§2).
- *Key results.* All ✓ (§3.1–3.2, Table 2):
  - Across the three settings (ALFWorld list regime, ScienceWorld, OPSD), MIS lowers prediction accuracy by 15.3–61.6%, while the absolute pass@1 gap to GT is only 0.51–2.75 pp and the pass@64 gap is ≤3.0 pp.
  - ALFWorld list regime: GT 37.30 pass@1 vs MIS 34.55 vs COIN 30.41 vs BASE 11.48. On pass@64, COIN gets 87.23 vs GT 83.94 (BASE 56.20).
  - No-list regime: MIS (14.52) and COIN (14.34) *beat* GT (8.95) on pass@1.
  - VisualWebArena: random reward gives +14.3% relative pass@64.
  - Mechanism: fewer loops (list-regime loop ratio 24.69 for BASE → 1.92–3.26 after training) and more candidate actions considered.
- *Limitations.* One 7B model, text environments (plus one VWA run), apparently one training run per condition. It does not show that prediction *never* helps: GT still wins on list-regime pass@1.

**4. Reinforcement World Model Learning (RWML)** ([2602.05842](https://arxiv.org/abs/2602.05842), 2026-02, v3 Nov 2026, NeurIPS 2026)
- *What it does.* Self-supervised RL. The reward is the embedding similarity (Qwen3-Embedding-8B) between the predicted and the realized next state. It uses no expert data and no success reward.
- *Setup.* ALFWorld with Qwen2.5-7B. τ² with Qwen3-8B, using a Qwen3-235B user. 3 runs (± reported).
- *Key results.* ✓ (Table 1–2):
  - RWML alone: ALFWorld 13.0 → 32.6; τ² 31.9 → 38.8.
  - RWML + Policy RL: 87.9 vs Policy RL 81.0 on ALFWorld; 43.7 vs 38.0 on τ².
  - It beats imitation learning, IWM and SR on ALFWorld (87.9 vs 82.5–83.3). On τ² it is second best (43.7, tied with imitation learning, vs SR 45.0).
  - Plain WM-SFT collapses on ALFWorld (2.8).
- *Limitations / critiques.*
  - Authors' own: text environments only, embedding distance is weak for structured outputs, and coverage limited to self-rollouts (App. B ✓ᵖ).
  - 2609.33335 shows mismatched targets keep most of the gain. The "world model" label therefore over-attributes the cause.

**5. Learning in Dreams, Winning in Reality** ([2610.08033](https://arxiv.org/abs/2610.08033), 2026-10-06, single-author report; not an LLM)
- *What it does.* Trains a PPO policy only inside a learned 10-hero MOBA world model with 1,400-tick free-running dreams. Real games are used only for world-model data and for selection.
- *Key results.* All ✓ (Abstract, §1, Table 5, §7):
  - 70.2% real radiant wins (421/600; CI 66.4–73.7), up from 0% for dream-only training and 33.7% before the loop. 0% as dire (the shipped opponent also wins none as dire).
  - Exploitation is invisible inside the dream: dream metrics have |r| ≤ 0.37 with real wins, while real kills and deaths have r = 0.84 / 0.82.
  - Before Dyna, the world model over-predicts tower falls 1.79× on the policy's own games. Dyna repair is worth +9.2 pp [3.5, 14.7] (49.0% → 58.2%, frozen-WM control).
  - "The policy inherits its world model's fidelity profile mechanic by mechanic."
- *Limitations.* All from §7 ✓:
  - single-seed loop runs;
  - the headline is the best of 13 policies (replicated on a second holdout: 69.3 vs 71.0 ✓ᵖ);
  - about 46,000 real games were used, so this is not sample efficiency;
  - fidelity inheritance is an inference from two measurements (correlational), and the causal fidelity intervention is left open.

**6. One Frozen Simulator Is Not Enough: Simulator Collapse in Multi-Agent RL** ([2608.12253](https://arxiv.org/abs/2608.12253), 2026-08, v2; Manning/Levine/Shi)
- *What it does.* Shows that RL against a single frozen, mode-collapsed LLM user simulator overfits to that simulator's mode. Proposes Verbalized Sampling (inference-time) and Population Co-Training (training-time). Releases SCOPE.
- *Setup.* Qwen3-4B/8B. Persuasion-for-Good, τ²-bench and CooperBench. A 6-simulator panel. A human study.
- *Key results.* Abstract ✓ᵖ:
  - Held-out success up to +9% with VS and up to +14% with Co-Training.
  - Policy entropy collapses under single-simulator RL.
  - **Correction** ✓ᵖ: the best checkpoints are *above* base (e.g. Retail 46.1 vs 40.4), and the drop below base appears only in the curves and for Gemini-3-Flash. In the human τ² study, RL(Single) scores 0.43 vs base 0.41. The Fig. 1c caption appears inconsistent with its data.
- *Limitations.* Checkpoint selection uses 3 seen simulators (leakage) ✓ᵖ. Small models.

**7. World Models Need Not Be Right to Rank Policies Right** ([2611.01139](https://arxiv.org/abs/2611.01139), 2026-10-31, preprint; not an LLM)
- *What it does.* Derives an exact pairwise simulation identity and a relative bound, in which common-mode Bellman residuals cancel. Proposes RAVEL, a tournament that eliminates policies only on held-out real audits.
- *Setup.* DMC cheetah-run and walker-walk. 45 SAC/TD3/PPO policies per task (3 seeds × 5 checkpoints). A 5-member ensemble world model with 128 imagined rollouts per policy ✓ (§5).
- *Key results.* All ✓ (Abstract, Tables 1–2, Remark 6):
  - Kendall τ = 0.962 / 0.800, yet mean paired audit-error correlation is only 0.007 / 0.090.
  - Simultaneous intervals cover 99.98% / 99.75% of decisions but certify only 13.2% / 0% at 8 audits.
  - RAVEL fails its own precommitted 2× efficiency target (episode-saving ratios 1.00 / 1.33).
  - Frozen-model optimization lowers *real* return without raising predicted return on average.
  - Remark 6 predicts ranking breaks "after extensive optimization inside a frozen model".
- *Limitations.* Two tasks, not LLM agents, and compute was not recorded ✓ᵖ.

**8. Scaling Automatic Research Agents via World Models (WMRL)** ([2608.12564](https://arxiv.org/abs/2608.12564), 2026-08, v3 Sep 2026, UIUC/Amazon)
- *What it does.* A prompted world model (same backbone as the agent) predicts execution outcomes during GRPO. About 10% "anchor groups" are also executed for real (§3, footnote 2 ✓). Two corrections are applied:
  - Online Debiasing: isotonic recalibration.
  - Inverse-Variance Denoising.
- *Setup.* Qwen3.5-4B/9B on MLE-Dojo (train/test split) and DSBench, avg@8. Plus MiniVLA-1B on LIBERO-Long.
- *Key results.* ✓ (Tables 1–3):
  - 9B: 349 GPU-h vs 1,174 for real GRPO, with MLE-Dojo 21.6 vs 18.8 and DSBench 32.8 vs 31.2. A pure world model gives 16.1 / 28.0. The authors summarize the saving as 3.1× and 3.4× (4B / 9B).
  - The ablation shows both corrections are needed (+2.9 to +4.8).
  - LIBERO overall: 41.2 vs GRPO 38.3.
- *Limitations.* The world model is a fixed prompted model, and the policy never searches against it adversarially. Single runs. The corrections act only on *scores*. If an agent edits itself by *reading* simulated traces, these corrections do not reach that channel (cf. idea #1).

**9. Predict the Reward, Not the World: World Reward Models for ML Agents** ([2610.38439](https://arxiv.org/abs/2610.38439), 2026-10-28, HKUST-GZ)
- *What it does.* Proves a fixed-information ceiling: predict-then-decide cannot beat the best direct selector, and a likelihood-trained world model can prefer the worse program on every instance. Trains ForeCritic (9B) on 5k executed program pairs.
- *Key results.* ✓ (Abstract, §1, §4.1):
  - 79.4% pairwise accuracy on 12,438 held-out pairs across 26 tasks (54.5% before training), vs 53–62% for LLM judges.
  - 71.5% on pairs with both programs unseen.
  - Its 38% most confident pairs are 95.2% correct.
  - Inside AIDE, ML-Master and ForeAgent, mean task time falls 4.4–7.5× at similar or better scores.
  - From verification ✓ᵖ: executed validation picks the test winner only 72.2% of the time.
- *Limitations.* One run per task. Descriptive results. Baseline judges were not re-run ✓ᵖ.

**10. From Traces to Agentic Worlds (Trace2Env)** ([2610.06100](https://arxiv.org/abs/2610.06100), 2026-10-05 (PDF header 2026-10-06), NTU; ICLR'27 sub sKf0N5GdA4)
- *What it does.* A learning-free "worldbook" (schemas, grounded evidence, induced rules) is built from historical traces. A world-model *agent* consults it, together with episodic state, to simulate.
- *Setup.* 9 environments (AgentWorldBench subsets, EnvScaler, ALFWorld, SciWorld). World-model backbones gpt-5.6-sol and deepseek-v4.1-flash; predictions scored by a gpt-5.2 judge ✓ (§5).
- *Key results.* ✓ (Tables 1–2):
  - Average judge score 75.94 vs 69.51 for direct prompting (gpt-5.6-sol); 75.75 with deepseek.
  - **ALFWorld (real success 93%): direct prompting gives 97% in-sim success but only 3% world-to-real (W2R) replay success (CR 0.032). Trace2Env gives 88% in-sim and 85% W2R (CR 0.914).**
  - SciWorld W2R: 45% → 60%.
- *Limitations.* Fidelity is judged by an LLM judge. Construction traces come from a strong agent (gpt-5.6-sol). W2R is measured only on 2 text worlds.

**11. Dream-RSI: Recursive Self-Improvement through Evolving Worlds** ([2609.14858](https://arxiv.org/abs/2609.14858), 2026-09, v2 Oct; Google/GDM; ICLR'27 sub 9WcfDMfDue)
- *What it does.* Recorded discovery trees become a *replay* simulator. Candidate exploration policies (branching, ordering, concurrency, stopping) are evaluated offline by "dreaming", then redeployed. This closes a meta-exploration RSI loop.
- *Key results.* ✓ (§1, §4.4, §5 Table 2):
  - 9 tasks in 4 domains.
  - Lasso solver: up to 162× fewer agent calls than SimpleTES (which runs gpt-oss-120b), so backbones differ.
  - KernelBench: 1.79–2.43× fewer generations.
  - TB2.1 harness (full 89-task suite): 0.7247 → 0.8090 vs Meta-Harness 0.79 and Claude Code 0.75, using Claude Opus 4.8 for discovery and evaluation.
  - On ConvDiv, using all history gives 1.90 vs 1.59 with current history only (at 786 vs 430 compute).
- *Limitations.*
  - The replay simulator cannot evaluate unvisited branches, so it has no model error and no fidelity axis.
  - The TB2.1 curve is best-so-far on the same 89-task suite with 2 runs per task ✓. No held-out split is visible.
  - "Competitive… in several settings" is the authors' own hedge ✓ (abstract).

**12. RSI-Anything: Unlocking RSI with World Models** ([2610.13912](https://arxiv.org/abs/2610.13912), 2026-10-09 (PDF header 2026-10-12))
- *What it does.* JEPA-Anything world models guide action and data selection. Policy and world model are updated conditionally. "Compatibility checks" decide which policy–model pair is kept. Names the **coupled-update challenge**: policy updates shift the distribution the world model operates on ✓ (§1).
- *Key results.* ✓ (§ code repair, Table 2):
  - CodeContests: 200 tasks × 3 seeds, fixed ReasonFlux-Coder-7B. Success goes 26.3% (capacity-matched static verifier; random 26.0%) → 29.2% (WM selection, 175/600; paired +2.83 pp, CI [+0.17, +5.50]) → 31.2% (WM adaptation), at about 3.72 test calls per task.
  - Ranking accuracy 66.5 → 72.8.
  - Lab planning 21.80 → 46.01% ✓ (abstract; over 5 versions ◐).
- *Limitations.* A heterogeneous showcase with weak controls in the non-code domains. The authors write that "joint multiround evaluation is needed" ✓.

**13. EvoBelief: Falsifiable World Models for Self-Improving LLM Agents** ([2610.12789](https://arxiv.org/abs/2610.12789), 2026-10-08, Amazon; NeurIPS'26 per round-2 notes; ICLR'27 sub V6sCud3xDW)
- *What it does.* Keeps an evolving set of falsifiable rules about environment dynamics. A rule is promoted only after independent validation and retired when counterexamples arrive. The LLM stays frozen.
- *Key results.* ✓ (Abstract, Table 1, appendix significance table near l.1315):
  - ALFWorld without actor memory (n = 134 unseen tasks): Haiku 4.5 66.42 vs Frozen WM 42.54 (+23.9 pp); GPT-5.4 76.12 vs 44.78 (+31.3 pp). Both p < 0.001 (exact McNemar). Against WorldEvolver the margin is smaller and not always significant (GPT-5.4 with memory: +4.5 pp, p = 0.362).
  - MiniGrid: recovers after hidden drift ◐.
  - DiscoveryWorld Reactor Lab: 83.3% (Normal tier) / 58.3% (Challenge tier) vs 0% for each baseline.
- *Limitations.* It assumes beliefs should be truthful. It never measures what outcome-only selection does to belief accuracy, which is the gap used by round-2's "useful fictions" idea.

**14. MIMESIS: Learning User Simulators as Training Environments** ([2610.09484](https://arxiv.org/abs/2610.09484), 2026-10, Meta MSL; ICLR'27 sub MrdfxYufui)
- *What it does.* A purpose-built 9B user simulator trained on human conversations, with reasoning supervision and 13 behaviour patterns. Agents are trained against the frozen simulator. Adds Coached On-Policy Self-Distillation (CSD).
- *Key results.*
  - ✓ (§1, Fig. 2): with a fixed GPT-5.5 agent on τ-bench, success with real users is 63.6%. The three frontier API simulators inflate it to 82.4–84.4%. Pretrained Qwen3.5-4B/9B simulators deflate it by 16.0 / 14.9 pp. Every trained simulator deviates by ≤10.3 pp.
  - ✓ (abstract, full text): SOUL-Index 65.7. Behavioural fidelity +13.4 over Claude-Opus-5 (on RealUserSim), and Turing distance −3.6 (on SimulatorArena). Across eight environments, agents trained with MIMESIS beat agents trained with a GPT-5.5 simulator under all 9 unseen simulators.
- *Limitations.* Generalization is tested against *other simulators*, not real users ✓. It does not vary fidelity causally.

Remaining papers:

| id | title | one-liner | mark |
|---|---|---|---|
| [2610.26828](https://arxiv.org/abs/2610.26828) | Agent World Model in the Wild (WildAWM; ICLR'27 XScKMmbFmx) | 341 GUI+MCP+CLI worlds from open models with execution-based quality gates. Average +10.58 / +8.21 over 8 OOD benchmarks (35B-A3B / 4B). | ✓ abs + §5 Table 5 |
| [2511.03773](https://arxiv.org/abs/2511.03773) | DreamGym | Reasoning experience model plus replay buffer plus curriculum. >30% over all baselines on WebArena. Sim-to-real with <10% of external data. | ✓ abs |
| [2609.32322](https://arxiv.org/abs/2609.32322) | DRPE: Not All Errors Matter (ICLR'27 hlfQI5hrMQ) | Iso-error protocol: decision-relevant error predicts planning quality. Expectimax, not MPC. On learned models (n = 7), both metrics are +0.43. | ✓ᵖ |
| [2611.09670](https://arxiv.org/abs/2611.09670) | Goodhart in the Planner | Harder MPPI search exploits learned rewards while held-out accuracy stays 0.75–0.97. The 76% drop is the worst case. Robotics. | ✓ᵖ |
| [2609.26293](https://arxiv.org/abs/2609.26293) | Dual-Frontier (ICLR'27 aHHHfJsRn4) | Shows that blame between the policy and the world model cannot be identified from passive interaction. Admits a WM-guided decision only when its advantage exceeds a certified WM-error bound. Tested on BFCL v4, API-Bank and NexusRaven. | ✓ abs+§1 (earlier pass) |
| [2610.40927](https://arxiv.org/abs/2610.40927) | Agentic World Modeling for Agent Test-time Scaling without Environment Replica | A simulator queries the real environment read-only. +40.07% over single-shot simulation and +12.36% over a trained world model (τ-bench, AppWorld, OfficeBench). | ✓ abs (title resolved this session) |
| [2611.04073](https://arxiv.org/abs/2611.04073) | Computer-Use Agents Gain Little Benefit from a World Model When Conditioned on Tutorials | OSWorld-Tutorial, Qwen3-VL-32B: no rollout 50.6 → oracle visual 51.5 with tutorial. 47.4 → 57.7 without. The bottleneck is the judge. | ✓ abs+Fig. 1 (title resolved this session) |
| [2610.12235](https://arxiv.org/abs/2610.12235) | LMs as AI Research World Models | 2,653 experiment records. In-context priors raise Spearman 0.51 → 0.77. | ◐ (round-1 sweep) |
| [2610.13991](https://arxiv.org/abs/2610.13991) | Predict Before You Modify (IDM) | Evolvable forecaster of self-edit outcomes. AUBC 0.306 vs 0.210. | ◐ (see self-update chapter) |
| [2607.27687](https://arxiv.org/abs/2607.27687) | Rehearse | Pre-execution judge accuracy falls 82.8 → 56.9 after ≥3 successes ("confidence cliff"). | ◐ |
| [2610.08773](https://arxiv.org/abs/2610.08773) | AdvSim2Real (ICLR'27 WwQHecJowh) | Curriculum plus injection adversary inside a frozen web world model, with transfer to a real browser. | ◐ |
| 0cb6zfwHm7 (OpenReview) | The Simulator Says Yes | On 399 held-out executor-refused actions, LWMs fine-tuned from Qwen3 1.7B–8B report a normal outcome on 57 / 44 / 32% (released Qwen-AgentWorld-35B: 25%). Refusals are 1.5% of corpus rows. A GRPO policy in a never-refusing simulator stops refusing within 20 steps; passing real refusals through restores it. Desk-rejected. | ◐ (abstract re-read this session) |
| g8RTu7QVFb | ReplayWorld | Agentic world model whose simulator skills evolve from replay discrepancies. +4.2 on ReplayBench (2,080 transitions). The agent trained on its trajectories gains +12.0 on average across 8 benchmarks. | ◐ |
| vJbjG3bWI7 | EnvSimBench | 400 samples, 167 environments. A "state-change cliff": simulators fail on state-changing operations. Withdrawn. | ◐ |
| OplgHiAcOP | Quantifying the Utility of User Simulators | RL against a role-play LLM gives 51% win rate vs the initial model (n.s.). Against a WildChat-fine-tuned simulator, 58%. 283-person user study. | ◐ |
| riKw4M6myC | Does Behavioral Fidelity Predict Reliable Policy Selection? | Making a recommender user simulator more behaviourally similar does not reliably improve policy selection. Withdrawn. | ◐ |
| jQDbBMncVh | PROWL-2 | Imagined rollouts are routed by learning potential × fidelity: reliable ones to agent training, unreliable ones to world-model repair (SMACv2). | ◐ |
| Z5DVLWI7en | EAU | A meta-agent edits the user simulator, environment and reward during RL. Changes are hot-swapped and persist across runs. | ◐ |
| g6YsNCtLEk | Agent-Editing WM | Models how actions change task *progress* rather than tool outputs. Action Judge macro-F1 70.5%. | ◐ |
| Iin8ijZhoS | FASim | Simulator construction where LLM diagnoses must survive falsification probes before they drive edits. | ◐ |
| [2611.10329](https://arxiv.org/abs/2611.10329) | SIRIS | Perturbs simulators and asks whether an LLM can tell when an error overturns candidate selection (systems research). | ◐ |
| [2610.29435](https://arxiv.org/abs/2610.29435) | Events, Not Pixels | World-model return errors localize to events, not to rendering error. | ◐ |
| [2611.14955](https://arxiv.org/abs/2611.14955) | Posterior-Prior Epistemic Gap | Imagination can fail to carry task-relevant signal. Single-seed RSSM case study. | ◐ |
| [2611.02815](https://arxiv.org/abs/2611.02815) | Predictive Safety Guardrails Do Not Always Make LLM Agents Safer | Forward citation of Qwen-AgentWorld. WM-based guardrails. | † (title only) |
| [2611.05036](https://arxiv.org/abs/2611.05036) / [2611.22777](https://arxiv.org/abs/2611.22777) / [2611.21152](https://arxiv.org/abs/2611.21152) | ToolSpace / CrossEnv / Env Evolution Loop | November wave of executable environment synthesis (tool-centric, cross-environment, repo-to-environment). | ◐ |
| [2611.21095](https://arxiv.org/abs/2611.21095) | SWE-WhatIf | Simulates real users of coding agents from recorded sessions. | ◐ |
| [2611.11897](https://arxiv.org/abs/2611.11897) | ShopMind | E-commerce user simulator with a "mental world model". | ◐ |
| 2611.world-intelligence-agent-world-modeling (alphaXiv) | World Intelligence Agent | Unifies next-observation world modeling with action learning for digital agents. | ◐ |
| [2504.21024](https://arxiv.org/abs/2504.21024) | WebEvolver | Co-evolving web world model for self-improvement. | ◐ |
| [2606.30639](https://arxiv.org/abs/2606.30639), [2606.02372](https://arxiv.org/abs/2606.02372), [2610.11794](https://arxiv.org/abs/2610.11794), [2610.28945](https://arxiv.org/abs/2610.28945), [2610.20597](https://arxiv.org/abs/2610.20597) | WorldEvolver / CoMAP / Memento 3 / WM Predictive Paths / Co-Evolving Acting & Forecasting | Self-evolving or co-evolving world models paired with policies. | † (seen as IDs in round 1) |
| [2604.22748](https://arxiv.org/abs/2604.22748), [2606.09032](https://arxiv.org/abs/2606.09032) | Agentic WM survey ("Agentic World Modeling: Foundations, Capabilities, Laws, and Beyond"); Text WM survey | Taxonomies used above. | ◐ (2604.22748 title resolved this session) |

### Consensus, debates & contradictions

1. **Executable vs learned simulators.**
   - AWM: GPT-5-as-simulator RL roughly matches or trails base on τ² (4B: 13.67 vs 15.83) ✓.
   - Qwen-AgentWorld: a *trained* LWM with *control instructions* beats real-environment RL ✓.
   - These reconcile. Uncontrolled LWMs give ≈0 in both papers (Tool Decathlon 32.4 → 31.5). The real variables are grounding (initial state, worldbook, read-only queries) and controllability, not "learned vs code". Executable environments still carry their own bugs: 74–83% of 100 sampled AWM environments have at least one bug, depending on the judge ✓ (Table 6).
2. **Does world modeling help the policy?**
   - Yes: RWML (+6.9 / +5.7) ✓ and Qwen-AgentWorld warm-up (+9 to +11) ✓.
   - But the audit (2609.33335) ✓ shows most of the gain survives wrong targets, and random rewards expand pass@64.
   - CUA tutorials ✓: even *oracle* future screenshots add about 1 pp once tutorials are present.
   - Unresolved: no RWML-style paper runs the MIS/COIN controls †(absence claim), and the audit has not been replicated at τ²/MCP scale.
3. **Predict the world vs predict the decision.** WRM proves the ceiling and shows 79.4% vs 53–62% ✓. Agent-Editing WM ◐ and DRPE ✓ᵖ agree that "decision-relevant" is what matters. Qwen-AgentWorld and Trace2Env still optimize observation fidelity. The field is splitting into *outcome or reward models for search* and *observation simulators for RL data*.
4. **Fidelity metric ≠ downstream utility.**
   - Trace2Env: higher in-sim success, lower W2R ✓.
   - Behavioural-fidelity-vs-selection ◐ and 2611.01139 ✓: high rank correlation, little audit value, and exploitation under optimization.
   - Quantifying-Utility ◐ points the other way: the more realistic (fine-tuned) simulator *did* yield better assistants (58% vs 51%).
   - So fidelity is necessary but not sufficient, and the relevant kind is decision- and optimizer-specific.
5. **What a learned simulator teaches.**
   - Learning in Dreams: the policy inherits the world model's fidelity profile ✓.
   - The Simulator Says Yes: a never-refusing simulator trains refusal-dropping agents ◐.
   - Simulator Collapse: the policy exploits the simulator's mode ✓ᵖ.
   - All three are *weight RL*. Whether *harness or prompt evolution* inherits simulator quirks the same way is untested †(absence claim; consistent with the round-2 ICLR'27 sweep). That is the opening for idea #1.
6. **Exploitation vs regression to the mean.** 2611.01139 Remark 6 ✓ and the optimizer's curse both predict real return declining with search depth even under *unbiased* noise. Papers rarely separate structured exploitation from selection noise. The round-2 critique of evolve-in-dream made exactly this point.
7. **Contradiction to watch.** Qwen-AgentWorld says its LWMs are high-fidelity (58.71, best overall) ✓. The Simulator Says Yes says the released 35B whitewashes 25% of refusals ◐. Rubric averages hide rare, decision-critical errors such as refusals and state changes (cf. the EnvSimBench "state-change cliff" ◐).

### Open problems

1. **A decision-anchored fidelity metric for LLM simulators that predicts exploitation by a given optimizer.** It should be a function of optimization depth, not one static number. Candidates: W2R (Trace2Env), top-1 regret (2611.01139), whitewash rate on refusals.
2. **Channel of corruption.** When an agent improves from simulated experience, does damage enter through *scores* (selection) or through *traces it reads* (teaching)? Most corrections so far act on scores (WMRL debiasing, RAVEL audits, Dual-Frontier gates). **Corrected:** not all of them. The Simulator Says Yes's fix passes real executor refusals through per call, which repairs the observation channel, though only for weight RL ◐. VHE (5lWjRTK6Vk, ICLR'27 ◐ per round-2 sweep) already restricts what a reflector reads. Neither tests the trace channel *causally* against the score channel.
3. **Causal, per-quirk fidelity interventions in language environments.** Inject a named defect, measure dose-response with depth, and run an unstructured-noise control at the same error rate. Learning in Dreams explicitly leaves this open in its MOBA setting ✓.
4. **Minimum real anchor rate.** How small can the real fraction be (WMRL uses about 10%) as the optimizer gets stronger or searches longer? Is the right anchor random, or targeted at the policy's own distribution (the Dyna lesson: +9.2 pp)?
5. **The coupled-update problem (RSI-Anything).** Simulators and outcome predictors go stale as the agent self-modifies (cf. the Rehearse confidence cliff). Nobody tracks predictor calibration across many self-improvement generations †(absence claim).
6. **Placebo controls for "world-model" gains at scale.** MIS/COIN-style arms for RWML, Qwen-AgentWorld warm-up and IDM-style forecasters on τ², MCP and web.
7. **Rare-event fidelity.** Refusals, irreversible state changes, permission errors. Simulators are trained on success-heavy traces (only 1.5% refusal rows in the corpus studied ◐), yet these are exactly what safety-relevant self-improvement needs.
8. **User-simulator validity against humans, not against other simulators.** MIMESIS validates agent training against 9 other simulators ✓ (its §1 calibration study does use real users). Only a few studies (MIMESIS §1, Quantifying-Utility, Simulator Collapse) use real users.
9. **Replay simulators vs generative simulators for meta-level RSI.** Dream-RSI's replay has no model error but cannot evaluate unvisited branches. No one compares replay, LWM and hybrid simulators for evaluating *improver* policies †(absence claim).

### Where it is heading (to mid-2027)

- **Environment synthesis becomes infrastructure.** The November wave (ToolSpace, CrossEnv, repo-to-environment loops, WildAWM) and industrial LWMs (Qwen-AgentWorld) make "a simulator for X" a commodity. Pure environment-generation papers will need a twist (fidelity guarantees, co-evolution) to be accepted.
- **Co-evolution of agent and simulator is the declared next step.** Qwen-AgentWorld lists it as future work. EAU, ReplayWorld, PROWL-2 and Simulator Collapse's population co-training are early instances. Expect "the simulator edits itself from discrepancies" papers (L3 "Evolver" in 2604.22748).
- **Audits and fidelity validation are moving into main venues.** Signals: The Simulator Says Yes, Behavioural-Fidelity-vs-Selection, EnvSimBench and SIRIS at ICLR'27, though several were withdrawn or desk-rejected; 2611.01139 is an arXiv preprint. Reviewers will increasingly expect W2R-style real replay and decision-anchored metrics rather than rubric scores.
- **Direct selectors over next-observation predictors for search.** WRM-style models will become standard inside AutoResearch and harness search. Read-grounded simulation (AWM-TTS) for test-time scaling in non-resettable environments will grow.
- **Likely scoop risk.** The round-2 estimate (from /home/user/nlp-lab-paper-search/research/round2/) was about 50–60% that a "harness or GEPA evolution inside an LWM transfers to the real benchmark" method paper appears before May 2027. Pieces already exist: Dream-RSI's harness result, ReplayWorld, AdvSim2Real. A causal *quirk-inheritance* study for *harness* learners had not appeared as of 2026-11-18 †(absence claim, from searches up to that date).

### Connection to our ideas (/home/user/nlp-lab-paper-search/research/ONE_PAGERS.md)

- **#1 Taught, Not Selected (current lead).** This is the chapter's core match.
  - Almost every published correction acts on the score channel: WMRL debiasing, RAVEL audits, Dual-Frontier gates, PROWL-2 fidelity routing. The exceptions are The Simulator Says Yes's refusal pass-through (weight RL) and VHE's restriction of what the reflector reads. So #1 should present trace hygiene as *causally tested*, not as new.
  - Every published inheritance result is weight RL: Learning in Dreams, Simulator Says Yes, Simulator Collapse.
  - #1's trace-source × score-source 2×2 is still unclaimed (per round-2 ICLR'27 sweep).
  - Use Trace2Env- or Qwen-AgentWorld-style simulators as the "sim" arm. Use W2R replay as a manipulation check. Use whitewash and fabricated success (Simulator Says Yes; BreakingWeb) as the visible-vs-silent bug pair.
  - Must-cite: 2609.33335 (placebo vocabulary), 2611.01139 (add a matched unstructured-noise arm to separate regression to the mean), WRM, DRPE (iso-error replication only), 0cb6zfwHm7, 5lWjRTK6Vk.
- **#7 Varying Goals / #9 MetaShift.** Qwen-AgentWorld's control instructions (intermittent API errors, pagination, partial failures) and EvoBelief's hidden-drift MiniGrid are ready-made *regime-shift generators*. They let #9 create "the rules of improving changed" shifts cheaply inside a simulator. Validate on real environments.
- **#6 Meta-Overfitting.** Simulator Collapse (one simulator → narrow strategies) is the RL analogue. A "home simulator vs far simulator" axis could serve as the graded domain-distance variable.
- **#4 Order-2 Audit / #2–#3 (gate).** Outcome predictors are the natural cheap gates for self-edits: WRM, IDM, Rehearse, and RSI-Anything's compatibility checks. The Rehearse cliff and RSI-Anything's coupled-update problem predict that gate calibration decays with depth, which is a measurable side-outcome for #3.
- **Register Snowball (Xunjian's idea, #10).** The one-pager already chooses a scripted uptake user over a free LLM simulator. The MIMESIS §1 result supports that choice: frontier simulators inflate τ-bench success from 63.6% to 82.4–84.4% ✓. So do Simulator Collapse and Quantifying-Utility.
- **Retired framing.** "Evolve-in-dream" (round 2) should stay retired as a headline. Dream-RSI owns "dream", and the fidelity-metric claim is pre-empted by DRPE, WRM, the 2609.33335 audit and 2611.01139. Its useful remainder is folded into #1.

#### Fact-check log

All full-text checks used the cached texts at /tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/<id>/paper.txt (line numbers refer to those files).

| Claim | Verdict | Source location |
|---|---|---|
| Qwen-AgentWorld-397B 58.71 vs GPT-5.4 58.25; 35B 47.73 → 56.39; GUI rank 5th (59.69) | confirmed | 2606.24597 §5 text l.1006–1017, Table 5 l.1040–1067 |
| Qwen WideSearch 34.02 → 50.31; Sim-RL 50.3 vs Real RL 45.6; Tool Decathlon 32.4 → 31.5 uncontrolled; +3.7 / MCPMark +12.3 controlled; OpenClaw 65.4 → 69.7, 47.9 → 55.0, Qwen3.6-Plus 66.7/47.8; warm-up +11.3/+9.7/+9.0; >10M trajectories; 5 frontier models × 9 benchmarks | confirmed | 2606.24597 l.23, 760–766, 1177–1194 (Table 6), 1250–1267 (Table 7), 1293–1384 (Table 8), 1407–1452 |
| Trace2Env ALFWorld direct prompting 97% in-sim / 3% W2R; Trace2Env 88% / 85%; SciWorld W2R 45 → 60; 75.94 vs 69.51 | confirmed; added that real-environment success is 93% and the judge is gpt-5.2 | 2610.06100 Table 2 l.447–457; Table 1 l.373–378; l.351 |
| Learning in Dreams 70.2% (421/600, CI 66.4–73.7), 0% dream-only, 33.7% before loop; |r| ≤ 0.37; r = 0.84/0.82; 1.79×; +9.2 [3.5, 14.7]; best of 13; ~46k games | confirmed | 2610.08033 abstract l.20–31, §1 l.85–118, Table 5 l.396–405, §7 l.520–525 |
| WM audit: MIS lowers prediction accuracy 15.3–61.6%, pass@1 gap 0.51–2.75, pass@64 ≤3.0; ALFWorld numbers; VWA +14.3%; DeepSeek-V4-Flash judge; 1,200 transitions; Qwen2.5-7B | confirmed; corrected scope (range covers three settings: ALFWorld list, ScienceWorld, OPSD) and loop-ratio wording (24.69 → 1.92–3.26) | 2609.33335 l.40, 232–266, Table 2 l.285–295 |
| MIMESIS: real users 63.6% vs frontier sims 82.4–84.4%; Qwen3.5-4B/9B −16.0/−14.9; trained sims ≤10.3 pp; SOUL 65.7; +13.4 fidelity; 9 unseen sims | confirmed (abstract items upgraded from ◐ to ✓; +13.4 is on RealUserSim) | 2610.09484 abstract l.20–27; §1 Fig. 2 l.91–122, l.147–153 |
| WMRL 349 vs 1,174 GPU-h; 21.6 vs 18.8; 32.8 vs 31.2; WM-only 16.1/28.0; 3.1×/3.4×; ablation +2.9 to +4.8; LIBERO 41.2 vs 38.3; ~10% anchors | confirmed | 2608.12564 Table 1 l.462–471, l.499, Table 2 l.517–523, Table 3 l.556–558, fn. l.264 |
| WRM 79.4% on 12,438 pairs / 26 tasks; LLM judges 53–62%; 71.5% unseen; 38% → 95.2%; 4.4–7.5× | confirmed | 2610.38439 abstract l.49–55, §1 l.119–132, l.380 |
| Dream-RSI 0.7247 → 0.8090 vs Meta-Harness 0.79 / Claude Code 0.75, Opus 4.8, 89 tasks × 2 runs; 162×; 1.79–2.43×; 1.90 vs 1.59; "several settings" hedge | confirmed | 2609.14858 l.28–29, 117–119, 322, 527–567, Table 2 l.575–584 |
| AWM Table 4: BFCLv3 53.83 → 65.94 (Sim 52.53); τ² 26.44 → 33.45 (EnvScaler 39.39); MCP-U 6.70 → 11.17; 4B τ² Sim 13.67 < base 15.83; 526 envs | confirmed | 2602.10090 Table 4 l.464–552; l.428 |
| AWM Table 6: 74% bugs / 14.0% blocked vs EnvScaler 88% / 57.1% | corrected: holds for the GPT-5.1 judge only; with the Claude-4.5-Sonnet judge, 83% vs 83% bugs and 11.5% vs 46.8% blocked; 100 sampled environments | 2602.10090 Table 6 l.480–512 |
| RWML: 13.0 → 32.6; 31.9 → 38.8; 87.9 vs 81.0; 43.7 vs 38.0; IL/SR 82.5–83.3; WM-SFT 2.8 | confirmed; "on par on τ²" sharpened to "second best (SR 45.0)" | 2602.05842 Table 1–2 l.261–290, l.330 |
| 2611.01139: τ 0.962/0.800; correlations 0.007/0.090; coverage 99.98/99.75%; certify 13.2/0%; failed 2× target; Remark 6; 45 policies, 128 rollouts | confirmed | 2611.01139 abstract l.28–34, l.199–201, Table 1 l.252–257, l.310–368 |
| RSI-Anything 26.3 → 29.2 → 31.2; +2.83 [+0.17, +5.50]; 3.72 calls; 66.5 → 72.8; 21.80 → 46.01; coupled-update; "joint multiround evaluation" | confirmed ("5 versions" detail not re-checked, kept ◐) | 2610.13912 abstract l.33–38, §1 l.85, l.536–548, l.1162 |
| EvoBelief n = 134; 66.42 vs 42.54; 76.12 vs 44.78; p < 0.001; 83.3% / 58.3% vs 0% | confirmed; location corrected to Table 1 (l.355–366) + significance table (l.1313–1320); added WorldEvolver comparison (+4.5 pp, p = 0.362 with memory) | 2610.12789 abstract l.40–41, Table 1, l.1305–1320 |
| WildAWM 341 worlds; GPT-6-Astra ~60%; +10.58 / +8.21 (35B-A3B / 4B) | confirmed | 2610.26828 abstract l.33–35, l.119–121, §5 l.588–594 |
| DreamGym >30% on WebArena; <10% real data | confirmed (wording: "over all baselines", "<10% of the external data") | 2511.03773 abstract l.26–31, l.113–119 |
| Simulator Says Yes: 32–57% whitewash for fine-tuned LWMs, Qwen-AgentWorld-35B 25%, 1.5% refusal rows | confirmed at abstract level (◐); added 399 actions, Qwen3 1.7B–8B, policy stops refusing within 20 steps | OpenReview 0cb6zfwHm7 abstract (api2 notes/search) |
| "All corrections so far act on scores only" (Open problem 2, idea #1) | corrected: Simulator Says Yes's refusal pass-through fixes the observation channel (weight RL), and VHE (5lWjRTK6Vk) restricts what the reflector reads | OpenReview 0cb6zfwHm7 abstract; /home/user/nlp-lab-paper-search/research/round2/verify/openreview-iclr27-sweep.md l.80–81 |
| "No paper does X" claims (decision-anchored fidelity metric; harness quirk-inheritance; MIS/COIN controls for RWML; replay vs LWM comparison) | unverifiable (absence claims); marked † | none possible beyond prior sweeps |
| Publication dates 2606.24597 (06-23), 2610.06100 (10-05), 2610.13912 (10-09) | unverifiable from cache: PDF headers show 06-24, 10-06, 10-12 (likely version or compile dates); both shown | PDF first lines |
| ID resolution: 2610.40927, 2611.04073, 2604.22748 | confirmed: titles match ("Agentic World Modeling for Agent Test-time Scaling without Environment Replica"; "Computer-Use Agents Gain Little Benefit from a World Model When Conditioned on Tutorials"; "Agentic World Modeling: Foundations, Capabilities, Laws, and Beyond") | arXiv export API id_list query |

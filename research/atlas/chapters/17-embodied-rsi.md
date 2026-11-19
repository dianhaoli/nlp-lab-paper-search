## Embodied / physical-AI recursive self-improvement (robots, VLA agents)

*Scope: robots, VLA and world-action-model (WAM) agents, and embodied LLM agents that turn their own physical or simulated experience into lasting changes.*

*How this was built:*
- 6 alphaXiv searches, 5 ICLR 2027 OpenReview queries, and Semantic Scholar forward citations. The S2 calls were mostly rate-limited; I got citers of 2610.29677 and 2609.31760.
- 15 PDFs newly downloaded, plus 4 reused. About 46 papers in total.
- Adversarial fact-check pass (2026-11-19): I re-opened 17 full texts (local pdftotext plus the alphaXiv full text of the agentic-robotics survey) and checked about 75 numbers and claims. Corrections are folded in below and listed in the log at the end.

*Marks:*
- ✓ = checked in full text this session.
  - For 2611.08318 and 2611.13034 the full-text check was first done in round 2 (`round2/verify/factcheck.md`). This session I found every number again in the full text.
- ◐ = from the abstract only. For ICLR 2027 submissions this means the OpenReview abstract.
- † = from memory or unverified.

### At a glance
- **What the corner is.** A frontier coding agent improves the robot's software stack offline or between episodes, and the result persists. The agent itself stays fixed (Claude Opus 5.5, GPT-6.x via Codex, GPT-5.6-sol). What it changes:
  - a code skill library, a harness around a frozen VLA, a curriculum or judge;
  - sometimes a specialist VLA it trains on its own data.
  - The step from "robot policy learning" to "RSI" came in Sep–Nov 2026: about 40 papers, almost all in RoboDojo, RoboCasa365, LIBERO-Pro or RoboTwin simulation.
- **State of the art (simulation).** Code-Only-as-Policy (COAP, from the Embodied Turing Machines paper 2610.12369) reaches 70.24% SR on all 42 RoboDojo tasks ✓ (Table 2).
  - The leaderboard leader it beats is PhysicalRSI 1.0 (agent+VLA) at 31.38 ✓.
  - COAP runs no model at test time. Its library was written by Opus 5.5 (Max) agents ✓.
  - Hybrid LLM–VLA systems sit around 49–57 on *different* RoboDojo subsets (ESI ✓, RoboDoctor ✓).
- **State of the art (real robot).** Results are small and per-task:
  - ReSkill: 62/80 real trials over 8 tasks ✓.
  - EmbodiRSI: 83.1% with 10 real trajectories per subtask, vs 75.0% with 200 demos ✓.
  - AdaRSI on AGIBOT A3U, 3 tasks, 150 trials: 82.0% (VLA + manual sequence) → 92.0% (agent + harness) → 94.7% (AdaRSI) ✓.
  - SEAR: practice success overstates success on human-staged scenes ✓.
- **Consensus 1: no verified recursion.** The four physical-RSI surveys (2610.18842, 2610.29677, 2610.38266, alphaXiv 2610.rsi-agentic-robotics) agree that no physical or embodied study yet shows a *verified recursive gain*. By that they mean a revised improver Φ_k beating Φ_{k−1} from the same start under a matched budget. Only two of them say so outright, though:
  - 2610.18842 Fig. 3 puts "no confirmed case" at L5 ✓.
  - 2610.29677 says the studies it reviews "have yet to isolate gains attributable to recursion" ✓.
  - 2610.38266 defines L3 (verified improver gain) and says "persistent updates and task gains alone do not establish improver gain" ✓. In the text I searched it does not state that no study reaches L3.
  - The agentic-robotics survey makes no claim about recursion. Its finding is narrower: agent-directed improvement is rare and "persistent modification is not yet continual improvement" ✓.
- **Consensus 2: the bottleneck is the substrate, not the proposer.** The limits are representable evidence, expressible fixes and resolvable tests (2609.31760 ✓, 2611.09030 ✓, 2611.08318 ✓). Privileged simulator state and readable success criteria inflate scores (08318: 51.6 vs 27.8 collapses to 45.0 vs 47.2 ✓).
- **Biggest open problem.** There is no matched-start, matched-budget comparison of improvers on robots. Nor is there an equal-budget test that agent-directed improvement beats a fixed procedure per robot-hour. Only HARBOR attempted one, and it was confounded ✓ (agentic-robotics survey, Sec. VI).

### How it developed
| Month | Paper | What changed |
|---|---|---|
| 2023-05/06 | Voyager 2305.16291, RoboCat 2306.11706 † | Executable skill library; self-generated data → retraining. RoboCat ran one improvement iteration ✓ (per the agentic-robotics survey). |
| 2023-10 | Eureka 2310.12931 † | LLM writes and evolves reward code. A fixed-procedure loop. |
| 2025-09 | Self-Improving Embodied FMs 2509.15155 ◐ | Weight-level self-improvement: the robot practises and the model rewards itself. |
| 2026-06 | Playful Agentic Robot Learning 2606.19419 ◐ | Task-free skill acquisition for code-as-policy agents. |
| 2026-07 | Policy-level RSI with a Criticality World Model 2607.28251 ◐ | A world model targets the remaining failures for data collection. |
| 2026-08 | Zetta 2608.16590, Skill-Harness Evolution 2608.11350, Motus2 2608.30237 ◐ | "Harness evolution" vocabulary enters embodied work; first self-evolving world model. |
| 2026-09 | **What Stops RSI in Robotics? (123 rounds) 2609.31760** ✓; EmbodiRSI 2609.38905 ✓; RoboFoundry 2609.32862; SEES 2609.32698; ASENA 2609.39207 | First long-run, honest negative result. Real-to-sim-to-real RSI. RoboDojo leaderboard snapshot of 28 Sep: PhysicalRSI 1.0 at 31.38 ✓ (2610.38266 Table 7). |
| 2026-10 (early) | VeriFine 2610.08761 ✓; **ETM/COAP 2610.12369** ✓; EmbodiedSmith 2610.07969; RSI-Anything 2610.13912; EmbodiedCLBench 2610.13021 | Judge co-evolution. Code-only policy jumps to 70.24 on RoboDojo. |
| 2026-10 (mid) | **SEAR 2610.18792** ✓; **Who Improves the Robot? 2610.18842** ✓; EmbodiedEvo 2610.18867; **AdaRSI 2610.20474** ✓; Towards Agentic Robots 2610.20442 | First benchmark with resampling and inheritance controls; L0–L5 framework; on-demand weight adaptation. |
| 2026-10 (late) | NUS survey 2610.29677 ✓; CASIA survey 2610.38266 ✓; agentic-robotics survey (alphaXiv 2610.rsi-agentic-robotics) ✓; RoboDoctor 2610.35604 ✓; VISTA 2610.35051 | Survey wave. 18842 and 29677 explicitly find no verified recursive gain. CASIA sets the bar (L3) without crediting any study with it. The agentic survey finds agent-directed improvement rare and untested at equal budget. |
| 2026-11 (1–9) | PyRUA-REAL 2611.06768; Compounding Physical RSI 2611.06654; **Criteria Exposure 2611.08318** ✓; **Controlled Study 2611.09030** ✓; Flex-RSI 2611.08946; ReSkill 2611.10510 ✓; **RoboACE 2611.13034** ✓ | Audits and controlled studies. Verifier-leak critique. |
| 2026-11 (10–18) | LEASH 2611.14115, ESI 2611.18176 ✓, Envisioning Agentic Robotics 2611.18359, RoboCoEvo 2611.19081, CRAFT 2611.19952, MetaAct 2611.20655, HandRSI 2611.22164 | Safety; LLM–VLA co-training flywheels; executable constraints. |
| ICLR 2027 subs (◐) | Capability Loss in Embodied RSI (IkYmTAyPXZ); RoboRSI strategy (2vX7UBSqne); EmbodiedHarness (Iu4khDXeyy); MC-EvoBench (HhyB6932wc); World-ISI (Bd1yTJg9o3) | First meta-level ablations (strategy updated vs fixed) and erosion and transfer audits. |

### Taxonomy
There are three axes, and each comes from one of the surveys:
- **What persists** (2610.38266 §III): policy, imagined experience/world model, memory/skill, executable harness, plus improver-process revisions (curriculum, reward, research program).
- **Policy form at runtime** (2610.12369 Table 1; SEAR's three modes): VLA/WAM; agent-as-policy; harness-VLA; code-only.
- **Evidence level:**
  - 2610.18842: L0–L5 (capability vs meta level × within vs across task; L5 = verified recursive gain) ✓.
  - 2610.38266: L0–L3 (task-local, then persistent, then inherited process, then verified improver gain) ✓.
  - 2610.29677: R1/R2 (repair knowledge improves the improver, and then the improved improver improves itself), each tested against a frozen-knowledge control ✓.

| Approach | What the improver changes | Representative papers | Typical level reached |
|---|---|---|---|
| A. Code-as-policy library evolution (no model at runtime) | Shared perception, motion and task code | COAP 2610.12369, ReSkill 2611.10510, 2609.31760, SEAR (code mode), Controlled Study 2611.09030, PyRUA-REAL 2611.06768, ACE-CaP-0 2611.16872, Learning to Compose 2610.24464, RoboRSI-TSR (dahchK6UBy), EmbodiedEvo 2610.18867, ASENA 2609.39207, DINER 2611.diner | L1/L3 (capability, across tasks) |
| B. Harness around a frozen VLA ("harness VLA") | Routing, intervention, recovery, memory, constraints | RoboDoctor 2610.35604, VISTA 2610.35051, 2610.29560, EMHO 2610.08432, EmbodiedHarness (Iu4khDXeyy), RoboFoundry 2609.32862, Zetta 2608.16590, PhysicalRSI 1.0 (MMLab report), EmbodiSkill (utKO0tdAtF), MetaAct 2611.20655, RSI-Nav (HGKsXbq5rU), CRAFT 2611.19952 | L1–L3; VISTA's harness graph is a weak meta-level change |
| C. Harness + weights flywheel | Harness, then self-collected data → specialist VLA | AdaRSI 2610.20474, ESI 2611.18176, Co-Evolving Orchestrators & Policies 2610.09228, RoboCoEvo 2611.19081, Agentic-Teacher 2611.17394, SEES 2609.32698, EvoRAM (1v85Pec79o), Compounding Physical RSI 2611.06654, EmbodiRSI 2609.38905, EmbodiedSmith 2610.07969 | L3; summarised shared experience is the only meta-level element |
| D. Verifier, judge or curriculum co-evolution | The judge, the practice source, the criteria | VeriFine 2610.08761, RoboACE 2611.13034, ACSE (MvnHT3O2fl), RoboRSI-strategy (2vX7UBSqne), Intrinsic Robot Rewarding 2609.17115 | L2-like (process revised), not verified |
| E. World-model-mediated self-improvement | WM and policy, jointly or alternately | Motus2 2608.30237, RSI-Anything 2610.13912, World-ISI (Bd1yTJg9o3), Reliability-Aware WM (oJOplf6ZAh), Rollback 2610.36068, WoVR+ 2610.17922, D-World 2610.17114, 2607.28251 | L1/L3 |
| F. Weight-level RL self-improvement of VLAs | Policy weights from own rollouts | 2509.15155, Find Something You Can't Do 2609.32069, Scaling Online Exploration 2610.22496, VLRLA 2610.19517 | L1 |
| G. Audits, benchmarks, negative results | — | 2609.31760, 2611.09030, SEAR, 2611.08318, Capability Loss (IkYmTAyPXZ), MC-EvoBench (HhyB6932wc), EmbodiedCLBench 2610.13021, Flex-RSI 2611.08946, Scale & Selection 2609.39304, Goodhart in the Planner 2611.09670 | — |
| H. Surveys, frameworks, safety | — | 2610.18842, 2610.29677, 2610.38266, 2610.rsi-agentic-robotics, 2610.20442, 2611.18359, 2609.13236 (humanoids), 2611.14256 (multimodal), LEASH 2611.14115 | — |

### Paper cards

**What Stops Recursive Self-Improvement in Robotics? Lessons from 123 Rounds of Agentic Skill Discovery** ([2609.31760](https://arxiv.org/abs/2609.31760), 23 Sep 2026, NUS, cs.RO report) ✓
- *What it does.* Coding agents watch a simulated robot fail, write skills, research and install external models, and test every change.
  - Humans may repair the harness (evaluators, tests and memory) but never write a skill, program or model ✓.
  - The run lasted 123 rounds over several weeks ✓.
- *Setup* ✓:
  - Robot and tasks: RoboCasa PandaOmron with RGB-D only. Target task FRIDGE (about 15 steps); CABINET and SINK were added later; one held-out task.
  - Agents: orchestrator, proposer and research agent, all GPT via Codex CLI. gpt-5.6-sol at maximum effort ran rounds 1–75, gpt-6-astra at low effort rounds 76–84, and gpt-6-astra at medium effort rounds 85–123 (Table 1).
  - Gate: replay of the exact failure, then paired fresh-scene and held-out tests.
  - One round took 1–3 h, plus several hours when models were researched.
- *Key results* ✓ (Table 5):
  - Outcomes: FRIDGE 0/486, CABINET 0/256, SINK 14/230 (9 of those with failed intermediate steps).
  - Activity: 57 accepted changes (51 to perception), 86 provider registrations (over 20 identities), 847 proposal attempts, 113 numbered human harness fixes (about 2 per accepted change).
  - Frontier lock: 57% of FRIDGE rollouts fail at step 1 and 2.7% pass four steps.
  - Evaluator capture: 276 "reachable" successes at 0.849–0.850 m under a 0.85 m sphere. The base speed was advertised as 0.0016 m/step but measured 0.037.
  - Gate noise: 22 promotions (rounds 40–65) with 0 wins and 0 losses; 16 of 36 re-run disagreeing pairs did not reproduce; 119 rejected candidates had rescued at least one exact state.
  - Framing sensitivity: 0/8 edits in rounds 93–94 vs 11/12 in round 96, after one paragraph of proposer text changed.
  - Relational grounding: a Qwen2.5-VL-7B check got 4/4 shelf queries right, while the agent's geometric region skill grew to 3,056 lines with 33 thresholds.
  - What did work: the agent independently invented an active-search skill.
  - The agent "was rarely the bottleneck" (abstract).
- *Limitations.*
  - One run with no replicates, baselines or ablations. Models and harness changed along the same timeline (§10, the authors' own statement).
  - The three conditions (representability, expressibility, resolvability) are diagnostic, not causal.

**Embodied Turing Machines: Stateful Code for Robot Recursive Self-Improvement (COAP)** ([2610.12369](https://arxiv.org/abs/2610.12369), 8 Oct 2026, NTU/Ropedia, technical report; alphaXiv vote count †)
- *What it does.* Treats robot plus world as a Turing machine whose tape is the measured state.
  - Opus 5.5 coding agents write and debug one shared perception/motion/task library, offline.
  - At test time pure code runs, with no VLM or VLA.
- *Setup* ✓:
  - RoboDojo, 42 bimanual tasks; 3 seeds × 50 layouts per task; official evaluation client.
  - Inputs: 3 RGB views and proprioception.
  - Loop: each agent works in its own git worktree and branches are evaluated in parallel. Debugging uses a self-defined development set where measured state is compared *against simulator ground truth*; selection uses a validation split (§3).
  - Gate: accept if J(θ+Δ) ≥ J(θ) + ε (Eq. 5), with a margin of ≥4 of 30 episodes. Reruns differ by 1–3 episodes, so the margin is 1–2 noise widths (App. B).
- *Key results* ✓:
  - Overall 70.24% SR vs 31.38 for the best leaderboard entry (+38.9 pp).
  - By dimension: Memory 89.9, Precision 75.1, Generalization 65.7 (77.3 standard / 54.1 randomized), Open 64.1, Long-Horizon 56.4. The smallest margin is on Long-Horizon.
  - 83% of a new task's code comes from the library. The same program succeeds 63% on measured state vs 83% on oracle state.
  - A new task reaches 47% within 15 h.
  - Compute per episode is 0.8–2.4% of a VLA's and below 0.1% of an agent harness's.
  - Branching (Fig. 10b): of 36 branches, 17% were accepted, 36% lowered success, 11% made no change and 36% gave a small gain.
  - Failures: 62% come from state measurement (35% perception, 27% contact) and 37% from logic (29% incomplete logic, 8% poor design).
  - On standard layouts, test 74.9 vs validation 76.7.
- *Limitations.*
  - Simulation only. It relies on the benchmark's public camera parameters, meshes and object sizes, which the authors call "a major limitation" ✓.
  - There is a development cost for every task.
  - The improver is fixed and never revised. In 2610.18842 terms this is L3 capability accumulation, not meta-level RSI.
  - Development-time access to simulator ground truth is exactly the kind of privileged channel that 2611.08318 warns about.

**Learning the Task or Learning the Verifier? Rethinking Criteria Exposure in Embodied Self-Improvement** ([2611.08318](https://arxiv.org/abs/2611.08318), 5 Nov 2026, PKU/NUS) ✓
- *What it does.* Tests whether letting the learner read the benchmark's success criteria (C_orig) teaches the task or teaches the verifier.
  - It rescores the same episodes under instruction-faithful criteria (C_faith).
- *Setup* ✓:
  - Frozen actor GPT-6 Astra (high effort), acting through PyRUA-Lean primitives; learner Claude Code (Opus 5.5).
  - 25 RoboDojo tasks, 16 of them criteria-limited; 5 rounds on 5 development layouts; 20 test layouts per task.
- *Key results* ✓ (§4.2–4.3):
  - Under C_orig: exposed 51.6 vs success-only 27.8. Paired by layout, 93 layouts succeed only with the exposed library and 17 only with the success-only one.
  - Under C_faith: 45.0 vs 47.2, so the gap shrinks by 25.9 pp (95% bootstrap CI 4.7–54.4).
  - A library trained on C_faith scores 65.0 under C_faith but 13.5 under C_orig.
  - Across benchmarks, neither library beats the empty library; only within-benchmark experience helps.
- *Limitations.* One actor and one learner; RoboDojo only. It cuts against every RoboDojo RSI number above that used criteria or ground-truth access during development.

**SEAR: Self-Evolving Agentic Robotics** ([2610.18792](https://arxiv.org/abs/2610.18792), 13 Oct 2026, Luma AI) ✓
- *What it does.* A benchmark with controls that separate retained experience from resampling, budget, privileged state and luck.
  - Three agent modes: code-as-policy, agent-as-policy, and auto-research (fine-tunes a VLA).
- *Setup* ✓:
  - 7 frontier LMs; 330 simulated tasks on 2 physics engines; 5 physical tasks on a two-arm rig; over four years of agent runtime.
  - Sealed judge with DEV/VAL/TEST seeds; best-of-3 at equal budget; no-library arm; generalization tiers T1–T5.
- *Key results* ✓:
  - On RoboLab, GPT-5.6-sol scores 46.9 with no library, 42.4 with its own library, and **60.6 with the library Opus wrote** (55.7 with Kimi-k3's).
  - On robosuite, one 1-h session for Claude Opus 5 scores 93.8 TEST vs 90.0 for best-of-3 20-minute attempts.
  - On hardware, GPT-6-astra goes from 41.4% as a frozen code policy to 88.0% acting live. A live episode takes a median 12.8 min of robot time vs 3.8 for a frozen code policy.
  - Claude Opus 5 code-policy TEST by tier (Table 3): T1 87.8, T2 59.7, T3 33.3, T4 28.7, T5 (real) 35.6. The VLA (auto-research) scores 73.2/55.3/43.1/10.2.
  - Frozen hardware policies: Fable 5.1 70.2, GPT-6-astra 41.4, Opus 35.6.
  - 6 practice runs reached 10 successes in a row; only 1 (Fable's Push-T) then went 10/10 on staged scenes. The agent's own resets choose its practice states: 10 of Opus's 64 Push-T starts needed a >90° turn, vs 6 of 10 staged scenes.
  - The gap between best and worst model is 23.8 pts on robosuite and 43.6 on RoboLab.
- *Limitations.* One practice run per model on hardware (authors' statement). The library effect depends strongly on the model.

**A Controlled Study of Models, Skills, Feedback, and Curriculum in Agentic Robotics** ([2611.09030](https://arxiv.org/abs/2611.09030), 6 Nov 2026, Bosch/NVIDIA/UC Berkeley) ✓
- *What it does.* Holds the self-learning harness fixed and varies four factors: model, skill prior, feedback structure, and privileged-to-perception curriculum.
- *Setup* ✓: 5 VABAR contact-rich tasks plus 4 MimicGen tasks; 631 experiment configurations; 28K rollouts; held-out instances; 3 seed replicates in the curriculum study.
- *Key results* ✓ (Table 1, VABAR held-out @0/@1/@2 rounds):
  - Opus 5 xhigh 45/64/74; Fable 5.1 44/57/64; GPT-6 Astra 43/47/48; Sonnet 5 45/46/46; Opus 5.5 67/74/77; human 82. The table also lists Opus 5 high/low, DeepSeek V4, Kimi K3 and GPT-6 Sol.
  - Finding 1: capability differences are less visible at initialization and appear during repair (generation ≠ repair ability).
  - Finding 2: expert skills are a boundary for weaker models and a scaffold for stronger ones.
  - Finding 3: feedback reported at the level of semantically meaningful skills is more useful than finest-resolution motion feedback.
  - Finding 4: object-relative geometry has to be made explicit.
  - Finding 5: reasoning over collision geometry is the bottleneck.
  - Finding 6: a privileged-to-perception curriculum can speed later learning or lock in brittle structure.
- *Limitations.* Simulation only; prompts and harness configurations not varied; a model snapshot in time (authors' own list).

**AdaRSI: On-Demand Recursive Self-Improvement for Embodied Agents** ([2610.20474](https://arxiv.org/abs/2610.20474), v2 17 Nov 2026, Northeastern University/AgiBot/UCAS/Tsinghua) ✓
- *What it does.* The harness is refined first. Then the agent collects demos for sub-steps that are still unreliable after trying alternative instructions or poses.
  - Each candidate dataset is accepted only if a temporary fine-tuned probe improves validation (Δ>0).
  - Accepted data are pooled to train a specialist VLA. A summarising agent writes shared experience M_r between rounds.
- *Setup* ✓: GPT-6.1-sol via Codex in simulation; Claude Opus 5.5 on real robots. GR00T N1.5 on RoboCasa365; π0.5 on LIBERO-Pro 2.0.
- *Key results* ✓:
  - Composite-Unseen: 4.6→13.3, vs harness-only 9.9 and always-train 11.1. It uses 34% of the GPU time (2,106 vs 6,113 min) and 36% of the demos (894 vs 2,498) (Table 4).
  - LIBERO-Pro 2.0: 32.9→49.1.
  - Removing the agent at deployment (fixed procedure): 6.9.
  - Procedure vs weights on 5 tasks (Table 5): 70 → 82 → 95 successes of 250.
  - Real robot, AGIBOT A3U (3 tasks, 150 trials): VLA + manual sequence 82.0, agent + harness 92.0, AdaRSI 94.7.
  - Real robot, SO-ARM101: on S3, specialist adaptation raises OOD success from 50% (harness) to 80%.
- *Limitations.*
  - Simulation demos used privileged state (755/4,457 attempts kept, 16.9%).
  - Per-task losses vs harness: CuttingToolSelection −8 and PortionHotDogs −4.
  - The effect of the shared experience M_r (the only meta-level element) is not isolated in the main text.

**VeriFine: Scaling Verification for Self-Improvement in Embodied Reasoning** ([2610.08761](https://arxiv.org/abs/2610.08761), 6 Oct 2026, NVIDIA/UCLA/Berkeley/Stanford; ICLR27 sub zNKghxZBmm) ✓
- *What it does.* Runs a policy loop (judge-guided curriculum plus RFT/SFT) alongside a judge loop. The judge loop is triggered on plateaus and uses humans in "coactive calibration" to refine a rubric.
  - A Claude Opus 5 teacher judge is distilled into a Qwen3-VL-2B student judge.
- *Setup* ✓: driving reasoning (RFT) and VLNVerse navigation (SFT); Qwen3-VL-2B policy; 8 H100s.
- *Key results* ✓:
  - Driving rounds: 67.30 → 70.59 → 71.61, with R3 5.8% above the best reference-based baseline (67.70).
  - Judge-guided selection beats random by 4.5–14.6%.
  - Teacher–human correlation 0.55→0.85 (MAE 0.25→0.13). With the Opus 5 backbone fixed, calibration alone gives 0.67→0.85.
  - The R1 student has r=0.07 on the later "challenging" subset (r=0.72 on its own round's subset), which makes the verification boundary visible. R2 raises it to 0.74.
  - Navigation: 68.09→77.59.
- *Limitations.* Humans are required in the judge loop; the policy evaluation set is fixed; reference reasoning is human-verified (§A.5). Not autonomous RSI.

**RoboACE: Adversarial Co-Evolution for Robot Self-Improvement** ([2611.13034](https://arxiv.org/abs/2611.13034), 9 Nov 2026, SIAT-CAS) ✓
- *What it does.* A curriculum harness and an execution harness co-evolve under a Strategic Advisor and a self-curated benchmark.
- *Setup* ✓: GPT-6.1 (Terra), SAM 3, frozen π0.5; 27 saved round records; 30 practice conditions and an internal benchmark of up to 40. No LLM or VLA weights were trained.
- *Key results* ✓ (Table 1), on LIBERO / LIBERO-? / LIBERO-PRO / LIBERO-X:
  - π0.5: 96.9 / 92.0 / 60.5 / 11.5.
  - RoboACE before evolution: 63.5 / 64.5 / 59.0 / 19.5.
  - RoboACE after: 99.0 / 94.5 / 74.0 / 30.5.
- *Limitations.*
  - A single campaign (stated by the authors).
  - The starting harness is far below bare π0.5 on LIBERO, so part of the gain is repair.
  - No proxy-vs-truth tracking of the self-curated curriculum.

**Physical-RSI frameworks and surveys** (cited together because they overlap in verdict, though they word it with different strength):
- **Who Improves the Robot? Toward Physical Recursive Self-Improvement** ([2610.18842](https://arxiv.org/abs/2610.18842), 13 Oct, SJTU/TUM, survey/framework) ✓
  - L0–L5 levels. L5 builds on L4 and requires evidence from an independent evaluator, under the same controlled conditions, that Φ_k beats Φ_{k−1} from the same starting generation and experience.
  - Fig. 3 places Hyperagents at L4 and marks L5 "no confirmed case". RoboFoundry, ASPIRE, HarnessPAI and RPG sit around L3–L4.
  - §7 priorities: reliable feedback, causal attribution across harness parts, non-stationarity, reward hacking and evaluation leakage (citing Dwork et al. 2015 on reusable holdouts ✓), and safety across recursive updates. The list of priorities as a whole is ◐.
  - Naming caution: this is *not* "PhysicalRSI 1.0", the MMLab agent+VLA system at 31.38 on RoboDojo (2610.38266 ref [62] ✓).
- **Toward Recursive Self-Improvement in Physical AI** ([2610.29677](https://arxiv.org/abs/2610.29677), 21 Oct, NUS) ✓ (abstract, §2–3)
  - Defines R1 (repair knowledge improves the improver) and R2 (the improved improver improves itself), each as an inequality against a frozen-knowledge control.
  - Verdict: "the physical studies reviewed here have yet to isolate gains attributable to recursion."
- **Recursive Self-Improvement in Physical AI: Mechanisms, Evidence, and Evaluation** ([2610.38266](https://arxiv.org/abs/2610.38266), 28 Oct, CASIA) ✓
  - L0–L3 rubric. L3 = verified improver gain from shared learner starts under matched resources.
  - Fig. 4: "Persistent updates and task gains alone do not establish improver gain."
  - Table 7: RoboDojo snapshot of 28 Sep 2026.
  - Table 9: within-study deltas (EmbodiRSI +20.6, RoboCoach +61.7, RPG +27.5, SHAPER +8.7); "different task sets are not ranked".
  - Proposes common-data and interactive comparisons of reference vs revised improvers from shared learner starts.
- **A Survey of Agentic Robotics: Toward Continual Self-Improvement** ([alphaXiv 2610.rsi-agentic-robotics](https://www.alphaxiv.org/abs/2610.rsi-agentic-robotics), Wang/Jiang/Shen/Soh, NUS; no arXiv number) ✓
  - Systematic coding of a 2,343-system corpus (κ 0.90–0.96).
  - Agent-directed improvement appears in 17 systems, 16 of them from 2026. Of those 17: 13 never re-test earlier capabilities, 7 test held-out transfer, and 8 report real-robot results.
  - The only equal-budget comparison (HARBOR vs Eureka/REvolve) ran in simulation on the tuned tasks and gave the agent extra human heuristics.
  - It does not use recursion language. Its verdict is "persistent modification is not yet continual improvement."

**Remaining papers**
| id | Title | One-liner | Mark |
|---|---|---|---|
| ICLR27 2vX7UBSqne | RoboRSI: Evolving a Guidance Strategy | Evolves explicit rules for *selecting and reviewing* improvements. +15.4 pp over no-strategy (4 benchmarks); +8.2 under matched access to history (LIBERO-Pro); +10.6 for updating the strategy vs keeping it fixed (RoboCasa365). **The closest thing to a meta-level ablation.** | ◐ |
| ICLR27 IkYmTAyPXZ | Capability Loss in Embodied RSI | "Silent erosion": with a fixed task distribution, tail and recovery modes erode through selection and visitation drift while nominal success rises. Proposes RESCUE; reports real-robot replication. | ◐ |
| 2611.10510 | ReSkill | Task-graph-localised revisions. Development-set average 30.0→86.2 on 10 RoboDojo tasks; retained programs 83.6 on robustness evaluation; 62/80 real (77.5%) over 8 tasks. Median revision touches 3 functions (about 0.5% of a program). Claude Opus 5.5 orchestrator. | ✓ |
| 2611.18176 | ESI flywheel | LLM corrections → VLA training data; prompts, skill interface and VLA updated together. 10 selected RoboDojo tasks: Score 62.03, SR 49.41, vs GO-3 Nano Preview 47.27/36.60. | ✓ |
| 2610.35604 | RoboDoctor | Training-free three-level (harness / intervention / operation) revision. 54 curated RoboDojo-Sim entries: score 57.10, SR 49.44. The authors say controlled trials are still needed. | ✓ |
| 2610.35051 | VISTA | A meta-agent actively probes the environment before committing harness edits, with a harness lineage graph. Needs fewer rollouts than static reflection (LIBERO-Pro/RoboCasa/RoboTwin). Planner GPT-5.6 Sol, meta-agent Codex/GPT-6 †. | ◐ |
| 2611.20655 | MetaAct | Evolves typed executable action guards (GEAR) instead of text. Violations 70→0% (ScienceWorld) and 40→1% (WebShop); +10.1 / +8.0. | ◐ |
| 2609.38905 | EmbodiRSI | Real-to-sim-to-real. Simulation 50.4→83.5 over two RSI updates; real 83.1% with 10 real trajectories per subtask (plus 400 adaptive simulated) vs 75.0% with 200 demos. | ✓ |
| 2611.06768 | PyRUA-REAL | Real-world code-agent RSI (GPT-6-Astra). | ◐ |
| 2611.06654 | Compounding Physical RSI | Downstream-stage failures turned into inherited capability for generalist policies. Cites 2610.29677. | ◐ |
| 2611.16872 | ACE-CaP-0 | Real-world code-as-policies that amortise repeated frontier calls. | ◐ |
| 2610.09228 | Co-Evolving Robot Orchestrators and Policies | Stanford/NVIDIA. Orchestrator and VLA co-evolve through deployment. | ◐ |
| 2611.19081 | RoboCoEvo | Agent–policy co-evolution to expand capability. | ◐ |
| 2611.17394 | Agentic-Teacher | A coding agent generates failure-targeted demonstrations for VLA post-training. | ◐ |
| 2609.32698 | SEES | Failure-guided VLA adaptation for long-horizon tasks. | ◐ |
| ICLR27 1v85Pec79o | EvoRAM | A VLA loop plus a code loop; physically verified executable memory. | ◐ |
| 2610.07969 | EmbodiedSmith | RSI flywheel for scaling simulated data and environments. | ◐ |
| 2609.32862 | RoboFoundry | "System-as-policy" evolution of the whole agent stack. | ◐ |
| 2608.16590 | Zetta ζ | Closed-loop embodied harness. | ◐ |
| 2608.11350 | Skill-Harness Evolution | Northeastern/MSR. Skills plus harness evolve together. | ◐ |
| ICLR27 Iu4khDXeyy | EmbodiedHarness | Anchored counterfactual replay to choose *which* harness component to revise. Accepts only edits that keep every validation episode the parent solved. | ◐ |
| ICLR27 utKO0tdAtF | EmbodiSkill | Separates a skill defect from an execution lapse. ALFWorld 61.19→86.57 with a frozen Qwen3.5-27B executor. | ◐ |
| ICLR27 HGKsXbq5rU | RSI-Nav | UCB-retrieved rule memory plus "preflection". +10.1% relative SR on MP3D. | ◐ |
| ICLR27 HhyB6932wc | MC-EvoBench | Minecraft audit: 19 chains; no-prior / related / unrelated / shuffled source arms; matched environment steps; 5 seeds. | ◐ |
| ICLR27 MvnHT3O2fl | ACSE | Scene generator and solver co-evolve (self-play curriculum). | ◐ |
| 2608.30237 | Motus2 | Self-evolving general world model for dexterous manipulation. | ◐ |
| 2610.13912 | RSI-Anything | RSI with world models when external feedback is costly. | ◐ |
| ICLR27 Bd1yTJg9o3 | World-ISI / WEO | One WAM jointly evolves action, prediction and value. | ◐ |
| ICLR27 oJOplf6ZAh | Reliability-Aware WM | Self-consistency reliability score weights imagined updates and picks real-data collection points. | ◐ |
| 2610.36068 | Rollback | Experience backtracking in world models. | ◐ |
| 2607.28251 | Policy-level RSI with a Criticality World Model | The WM targets remaining failures for collection. | ◐ |
| 2509.15155 | Self-Improving Embodied FMs | Weight-level self-practice baseline. | ◐ |
| 2609.32069 | Find Something You Can't Do | Agentic real-world RL for VLAs. | ◐ |
| 2610.22496 | Scaling Online Exploration for Robot Agents | Harvard/NVIDIA/Berkeley/MIT/Stanford. | ◐ |
| 2611.08946 | Flex-RSI | 24 vision/robotics tasks, 18 models. The improver helps weak models more (round-1 note: every pair n=1). | ◐ |
| 2609.39304 | Scale and Selection | More rollouts per round improves held-out success in robot harness evolution. | ◐ |
| 2611.09670 | Goodhart in the Planner | MPPI search against a learned reward cuts true return by up to 76% while held-out label accuracy stays at 0.75–0.97. | ◐ |
| 2610.13021 | EmbodiedCLBench | Continual-learning benchmark for self-evolving embodied agents. | ◐ |
| 2611.14115 | LEASH | Safety for agentic robotics. | ◐ |
| 2610.20442 / 2611.18359 / 2609.13236 | Agentic-robots and humanoid self-evolution surveys | Broader context. | ◐ |

### Consensus, debates & contradictions
- **Consensus: no verified recursion anywhere.**
  - Two surveys say this explicitly: 2610.18842 (L5 "no confirmed case" ✓) and 2610.29677 ("yet to isolate gains attributable to recursion" ✓).
  - 2610.38266 sets the L3 bar and does not credit any study with meeting it, but it does not state the verdict as a sentence ✓.
  - The agentic-robotics survey's narrower finding agrees: agent-directed improvement is rare and lacks a valid equal-budget test ✓.
  - None of the 2026 systems revises its improver under a matched-start control.
  - The only meta-level ablation found is RoboRSI-strategy (◐: +10.6 for an updated vs a fixed strategy). It does not compare Φ_k with Φ_{k−1} on fresh learners.
  - "RSI" in titles mostly means L1/L3 accumulation of a library by a fixed frontier coding agent.
- **Consensus: the substrate and evaluator are the bottleneck.**
  - 2609.31760 (representability, expressibility, resolvability; the agent was "rarely the bottleneck" ✓), 09030 (Findings 3–5 ✓) and 08318 (verifier leak ✓) agree on this.
  - VeriFine shows a round-1 judge drops to r=0.07 on newly exposed failures ✓.
- **Debate: should the agent stay in the loop at runtime?**
  - Against: COAP (no model, 70.24 ✓) and ReSkill (agent only during development; zero run-time API calls ✓).
  - For: AdaRSI (the fixed procedure falls 13.3→6.9 ✓) and SEAR (GPT-6-astra 41→88% acting live on hardware ✓, at a median 12.8 vs 3.8 min of robot time per episode, about 3.4×).
  - Likely reconciliation: whether the state can be measured. COAP depends on known simulator assets.
- **Contradiction: is the proposer the bottleneck?**
  - 2609.31760: "rarely" ✓.
  - 09030: repair ability splits models that start equal zero-shot (43–45 → 46–74 ✓).
  - SEAR: the model gap widens with task difficulty (23.8 pts on robosuite vs 43.6 on RoboLab ✓).
  - Reading both: the proposer matters *only when* failures are representable.
- **Contradiction: does inherited experience help?**
  - COAP: 83% of code reused ✓.
  - SEAR: own library 42.4 < no library 46.9 < another model's library 60.6 ✓.
  - 08318: no cross-benchmark gain over an empty library ✓.
  - Carrying brittle skills forward is a documented harm.
- **Leakage is everywhere.** Several papers use privileged information during development:
  - COAP debugs against simulator ground truth ✓.
  - AdaRSI collects demos with privileged state ✓.
  - 2609.31760's evaluator was exploited (the sphere) ✓.
  - SEAR agents choose their own practice resets ✓.
  - 08318 shows such channels can be worth about 25 pp ✓.
- **RoboDojo numbers are not comparable across papers:**
  - 42 tasks (COAP 70.24), 25 tasks (08318), 10 selected (ESI 49.41; ReSkill 86.2 on its *development* set), 54 curated entries (RoboDoctor 49.44).
  - Leaderboard snapshot top: 31.38.
  - CASIA Table 9 explicitly declines to rank across task sets.
- **Name collisions:**
  - "PhysicalRSI 1.0" (MMLab system), "Physical RSI" (2610.18842 framework), "Compounding Physical RSI" (2611.06654), and two different ICLR27 "RoboRSI" submissions. Cite by id.

### Open problems
1. **Order-1 test on robots.** Compare Φ_k vs Φ_{k−1} from the same learner start, the same experience and the same budget, with a protected evaluator. 2610.18842 builds this into its L5 definition, and 2610.38266 proposes common-data and interactive versions ✓. Nobody has run it.
2. **Equal-budget agent-directed vs fixed-procedure improvement per robot-hour.** HARBOR is the only attempt, and it is confounded ✓.
3. **Criteria and ground-truth exposure audits on every embodied benchmark** (the C_orig/C_faith rescoring design). The most exposed cases are COAP-style development against simulator ground truth and SEAR-style self-chosen resets.
4. **Frontier lock in skill chains** (57% fail at step 1 ✓). Credit assignment and curriculum over steps: reset-to-state, or replay at the failing step (EmbodiedHarness's anchored replay ◐).
5. **Regression and erosion.** 13 of 17 agent-directed systems never re-test ✓. "Silent erosion" of recovery modes (◐). There is no standard retention metric.
6. **Expressibility.** Can an improver switch model class (code → VLM grounder → VLA fine-tune)? In 2609.31760, switching model class was not an action the agent could take ✓. AdaRSI's on-demand weights are a first step.
7. **Gate noise at small n.** Gates resolve differences of about 4/30 episodes, only 1–2 noise widths (COAP ✓). Promotions often do not reproduce (16/36 ✓). Sequential and statistical gates (TRUST-style) are untested in embodied work.
8. **Single campaigns.** RoboACE, 2609.31760, COAP development and SEAR hardware each have one run per condition. Seed variance for embodied RSI is essentially unknown.
9. **Verifier evolution without humans.** VeriFine needs human calibration ✓. RoboACE's self-curated curriculum has no proxy-truth curve ✓.
10. **Sim-to-real of the improver itself**, as opposed to the policy: no paper tests whether a development procedure learned in simulation still works when the feedback is real †. This is an absence claim from our search, not exhaustive.

### Where it is heading (to mid-2027)
These are forecasts, not findings.
- **RoboDojo arms race.** Code-only and code-plus-VLA systems will push SR on all 42 tasks past 70, with the gains reported as RSI. Expect reviewers (ICLR 2027 decisions, Jan 2027) to demand C_faith-style rescoring and matched budgets.
- **Code → weights flywheels.** COAP positions itself as a data engine for VLAs ✓. AdaRSI, ESI, Agentic-Teacher and RoboCoEvo distil agent and code behaviour into VLAs. The loop "agent writes code → code generates data → VLA absorbs it → agent targets the new failures" is the likely next headline.
- **First meta-level controls** will come from the strategy and guidance-evolution line (RoboRSI-strategy ◐) and from the NUS cluster (2610.29677, the Soh survey, 08318 co-authors). Probably a two-round R1/R2 test in simulation by mid-2027.
- **Audits become a genre:** MC-EvoBench, Capability Loss, SEAR and the Controlled Study. Expect embodied "evaluator capture" benchmarks and real-robot replication requirements.
- **Safety and runtime assurance** (LEASH; 2610.18842 §5 †) will enter as constraints on self-modification on hardware, especially for agent-as-policy.

### Connection to our ideas (ONE_PAGERS.md)
- **#1 Taught, Not Selected** (corruption runs through what the reflector reads). This is the strongest outside support.
  - 2609.31760: one paragraph of proposer text moved edits from 0/8 to 11/12 ✓; memory corruption produced 88 request-bearing proposals gated as code ✓; evaluator diagnoses steered the fix †.
  - 09030 Finding 3: feedback at the level of meaningful skills beats finer motion feedback; granularity matters, not amount ✓.
  - 08318: reading the criteria teaches the verifier ✓.
  - Cite these as embodied external validity for the "reading channel" mechanism.
- **#3 Gate, Not Willingness.** Embodied gates are noisy and throw away useful candidates:
  - 119 rejected candidates had rescued at least one exact state; 22 promotions had 0 wins and 0 losses (2609.31760 ✓).
  - In COAP, 17% of 36 branches were accepted at a ≥4/30 margin ✓.
  - Supports framing the gate, not proposal willingness, as the binding constraint.
- **#6 Meta-Overfitting.** SEAR's tier decay (87.8 → 28.7 from T1 to T4 for Opus 5 code policies ✓) and 08318's swap (65.0 → 13.5 ✓) are object-level analogues. No embodied paper tests a *self-edited improver* far from home, so that gap is ours.
- **#8 Openness Ladder.** 09030 Finding 2 (expert skills bound weak models and scaffold strong ones ✓) and 2609.31760's expressibility limit ✓ both show that the best editable surface depends on capability.
- **#5 Order-0 rewards breed routers.** Embodied systems already gain from routing: AdaRSI's specialist-vs-default VLA, RoboDoctor's control allocation, 2609.31760's routing refits ✓ (Table 4).
- **#4 Order-2 Audit and nested replay.** SEAR's donor library (same solver model, different inherited library: 60.6 vs 42.4 ✓) is a ready-made state-swap. Motivation to cite: 2610.18842 ("no confirmed case" at L5) and 2610.29677 ("yet to isolate") ✓. Do not describe this as a four-survey unanimous verdict.
- **Scope advice.** Embodied replication is costly (weeks of runs, simulator setup). Use this corner for related work and external-validity paragraphs; do not move the main experiments here.

**Local files** (full text, this session): `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/{2610.12369,2610.18842,2610.20474,2610.08761,2611.13034,2610.38266,2609.31760,2611.09030,2610.18792,2611.18176,2611.10510,2610.35604,2610.35051,2611.20655,2609.38905,2611.08318,2610.29677}/paper.txt`. The agentic-robotics survey text is at `/root/.claude/projects/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/tool-results/mcp-alphaXiv-get_paper_content-1795053086176-vba3u5.txt`. OpenReview search dumps are in `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/or/`.

#### Fact-check log
| Claim | Verdict | Source location |
|---|---|---|
| ID check: 2610.12369 = "Embodied Turing Machines: Stateful Code for Robot Recursive Self-Improvement" (NTU/Ropedia, 8 Oct 2026) | confirmed (chapter's shortened subtitle expanded) | 2610.12369 p.1 header, arXiv stamp |
| ID check: 2611.08318 = "Learning the Task or Learning the Verifier? Rethinking Criteria Exposure in Embodied Self-Improvement" (PKU/NUS, 5 Nov) | corrected (title was missing "Rethinking") | 2611.08318 p.1 |
| ID check: 2610.18842 = "Who Improves the Robot? Toward Physical Recursive Self-Improvement" (SJTU/TUM, 13 Oct); also 2609.31760, 2610.29677, 2610.38266, 2610.18792, 2610.20474 titles and dates | confirmed | p.1 headers, arXiv stamps of each |
| COAP 70.24% on 42 RoboDojo tasks vs PhysicalRSI 1.0 at 31.38 (+38.9 pp); dimension scores incl. Long-Horizon 56.4 | confirmed | 2610.12369 Table 2 and §4.2 |
| COAP 83% library reuse; 63% vs 83% measured vs oracle state; 47% in 15 h; 0.8–2.4% compute | confirmed | 2610.12369 §4.3, Fig. 5, Fig. 7 |
| COAP 36 branches: 17% accepted, 36% lowered; margin ≥4/30; rerun noise 1–3 episodes | confirmed (added 11% no change and 36% small gain) | 2610.12369 Fig. 10b, Eq. 5, App. B |
| COAP debugs on dev set against simulator ground truth; "major limitation" on public assets; failures 62%/37% | confirmed | 2610.12369 §3 (l.560), §5.4 |
| 2609.31760 Table 5 numbers (0/486, 0/256, 14/230; 57/51; 86; 847; 113; 57%/2.7%; 276 at 0.849–0.850 m; 0.0016 vs 0.037; 22/16 of 36/119; 0/8 vs 11/12; 3,056 lines/33 thresholds; Qwen2.5-VL 4/4) | confirmed | 2609.31760 Table 5, Table 1 (models by round) |
| 08318: 51.6 vs 27.8 (93 vs 17 layouts); 45.0 vs 47.2; gap shrinks 25.9 (CI 4.7–54.4); swap 65.0/13.5; no cross-benchmark gain | confirmed | 2611.08318 §4.2 (l.429–434), §1 |
| SEAR: 46.9/42.4/60.6; 41.4→88.0 live; 12.8 vs 3.8 min; tier table 87.8…10.2; 70.2/41.4/35.6; 10 of 64 vs 6 of 10 | confirmed; clarified that "Opus" is Claude Opus 5 and that best-of-3 means three 20-min attempts | 2610.18792 §1, Table 3, §4.4, hardware section |
| 09030 Table 1 VABAR @0/@1/@2 (Opus 5 xhigh 45/64/74 … Opus 5.5 67/74/77; human 82); 631 configs, 28K rollouts, 3 seeds | confirmed (Finding wordings tightened to the paper's text) | 2611.09030 Table 1, abstract, Findings 1–6, Fig. curriculum caption |
| AdaRSI 4.6→13.3 vs 9.9/11.1; 2,106 vs 6,113 min; 894 vs 2,498 demos; 32.9→49.1; fixed procedure 6.9; 70→82→95/250; 755/4,457 | confirmed | 2610.20474 Tables 2, 4, 5, §5.3 |
| AdaRSI real robot "82.0–94.7% across 3 tasks" | corrected: 82.0/92.0/94.7 are VLA+manual / agent+harness / AdaRSI across all 150 trials of 3 tasks; SO-ARM101 50→80 is S3 OOD only | 2610.20474 §5.4 and appendix real-robot table |
| VeriFine 67.30→70.59→71.61, +5.8% over 67.70; 4.5–14.6%; 0.55→0.85, MAE 0.25→0.13; 0.67→0.85; r=0.07→0.74; nav 68.09→77.59 | confirmed | 2610.08761 Table (l.417–422), §4 Q1/Q2 |
| RoboACE table values; GPT-6.1 Terra, SAM 3, π0.5; 27 rounds; 30/40 conditions | confirmed | 2611.13034 Table 1, §4 setup |
| "All four surveys independently say no physical study shows verified recursive gain" | corrected: explicit only in 2610.18842 (Fig. 3 "no confirmed case" at L5) and 2610.29677 (abstract). 2610.38266 sets the L3 bar and says task gains do not establish improver gain, but I found no explicit verdict. The agentic-robotics survey never discusses recursion. | 2610.18842 Fig. 3; 2610.29677 abstract; 2610.38266 Fig. 4 caption, abstract; agentic survey full text (no "recurs" match) |
| Agentic survey: 2,343 systems, κ 0.90–0.96; 17 agent-directed (16 in 2026); 13 never re-test; 7 held-out; 8 real-robot; HARBOR confounded | confirmed | agentic survey §IV observations, Sec. VI, App. coding |
| CASIA Table 7 snapshot of 28 Sep, PhysicalRSI 1.0 = 31.38, ref [62] MMLab; Table 9 deltas +20.6/+61.7/+27.5/+8.7, "not ranked" | confirmed | 2610.38266 Tables 7, 9; ref [62] |
| ReSkill 30.0→86.2, 83.6, 62/80 (77.5%), 3 functions ≈0.5%, Opus 5.5 | confirmed (mark upgraded ◐→✓) | 2611.10510 abstract, §4, real-robot table |
| ESI 62.03/49.41 vs GO-3 Nano 47.27/36.60; RoboDoctor 57.10/49.44 on 54 entries; EmbodiRSI 50.4→83.5, 83.1 vs 75.0 | confirmed (marks upgraded ◐→✓) | 2611.18176 abstract/§5; 2610.35604 abstract/§4; 2609.38905 abstract/§4 |
| 2609.31760 "switching model class was not an action the agent could take" | confirmed | 2609.31760 abstract (l.53) |
| COAP "187 votes"; VISTA model names; 2610.18842 §5/§7 full priority list; "evaluator diagnoses steered the fix" | unverifiable this pass (marked †/◐) | not re-located in full text |

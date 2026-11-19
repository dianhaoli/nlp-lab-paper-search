## Self-play, self-generated curricula & open-endedness

*Marks: ✓ means the number or claim was checked in full text. It was either re-checked in this fact-check pass on 2026-11-18/19 (see the Fact-check log) or read in full earlier this session. ◐ means abstract only, or a project deep-read from round 1/2 that I did not re-check. † means from memory or unverified. Local texts are in `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/<id>/paper.txt`. Flattened copies (`nf.txt`, `azr.txt`, `rz.txt`, `a0.txt`, `rf.txt`, `sp.txt`) are under the scratchpad. The placebo trial was read in full through alphaXiv fullText.*

**Read in full or in their substantive sections this session:**
- Method papers: Absolute Zero, R-Zero, Agent0, R-Few, SPICE, SPADE, Anchored Self-Play.
- Audit papers: Selection Is Not Learning; the zero-data placebo trial; 2611.06965 (abstract, §1 and §3.5).
- Survey 2611.08908, the full body.
- Survey 2608.10299, the full body.
- Survey 2607.07663: §4.4 and its failure-modes section.
- Survey 2609.11873: §3.4 and §6.
- Survey 2607.13104: the passages on self-play, games and self-generated tasks.

**Read at abstract or results level only:** MAE, OMNI-EPIC, False Frontiers, CompassPlay, Self-Play Pretraining, Training Stability, GenEnv, EnvFactory and CoEvolve. ActiveSaddler and R-Quest were read at the results or introduction level, with their key numbers re-checked in the text.

### At a glance
- **What this corner is.** Loops in which the system writes its own training material and learns from it. That material can be:
  - problems: proposer/challenger → solver;
  - opponents: zero-sum games;
  - whole executable environments: environment designer → agent;
  - or the order in which real tasks are seen: an adaptive curriculum over a pool.
- **How the surveys place it:**
  - Duan et al. (2609.11873) put it at **L3, "experience autonomy"**. The defining property is "learner-conditioned future experience acquisition". They ask that a task be *valid*, *at the right difficulty* and *durably beneficial*, and they note that "neither correctness nor estimated difficulty alone establishes learning value" (§3.4, §6 ✓).
  - Zong et al. (2608.10299) file it under **Stage 1, agent–agent** (adversarial) and **Stage 2, agent–environment** (co-evolution of tasks, feedback and interaction space) ✓.
- **State of the art, weight learners (verifiable domains).** Zero-data loops add about +3 to +10 average points on Qwen3-4B/8B-Base math or general suites:
  - AZR, R-Zero, SPICE, Agent0 ✓.
  - EvoHeir (2610.22029) adds an inheritance gate that keeps gains climbing for 10 rounds ✓.
  - SPADE moves from QA to **self-written Gym environments**: +8.1 over base and +5.3 over the best fixed-environment baseline at 30B ✓.
- **State of the art, frozen-weight (harness or memory) learners.** ActiveSaddler gets +4.4 pp on GAIA2 and +7.5 pp on TB2 over a fixed shuffled scenario order ✓. HERA and DAEDALUS show that self-generated or co-evolved tasks can stand in for curated ones ◐.
- **Consensus.**
  1. Pure closed loops plateau and then collapse within about 3–10 rounds. Causes include pseudo-label decay, invalid questions, and diversity collapse with length inflation.
  2. **External grounding** postpones this. Sources of grounding:
     - a corpus (SPICE);
     - a few human anchors (R-Few with 1–5%, Anchored Self-Play with as few as 50 references);
     - an executor (AZR);
     - a population or cross-fitting (PopuLoRA, CrossFit).
- **The 2026-H2 turn is audits, not methods.** Four papers:
  - "What Actually Evolves" (ICLR'27 submission) finds that gains are mostly *sharpening* of items the model already solves, and that later proposers teach no more than earlier ones ◐.
  - Selection Is Not Learning finds that 60 of 96 updates on verified tasks yield no gain ✓.
  - 2611.06965 finds that executor "grounding" bakes the generator's own false belief into the answer key ✓.
  - The placebo trial finds that, in code→math, correctness of the reward matters; format alone does not explain AZR's gain ✓.
- **Biggest open problem.** We cannot yet show that a self-generated curriculum **teaches what the learner cannot already do**, measured against out-of-loop ground truth at matched compute. Two related unknowns:
  - how much external grounding is enough (the "exchange rate", 2607.07663 ✓);
  - whether training the proposer adds anything over a strong fixed generator plus selection.

### How it developed

| Month | Paper | What changed |
|---|---|---|
| 2019-01 | POET † (1901.01753) | Co-evolving environment–agent pairs; open-endedness as a design target |
| 2020-10 / 2020-12 | PLR † (2010.03934); PAIRED (2012.02096 ◐) | Unsupervised environment design (UED): replay of high-regret levels; adversary rewarded by regret against an antagonist |
| 2022-03 | ACCEL † (2203.01302) | Edit-based regret curricula |
| 2022-12 | Self-Instruct † (2212.10560) | LLM writes its own instructions from 175 seeds; synthetic data stays static and open-loop |
| 2023-06 | OMNI (2306.01711 ◐) | A foundation model acts as a "model of interestingness" and filters out tasks that are learnable but boring |
| 2024-01 | SPIN † (2401.01335) | Self-play fine-tuning against one's previous iterate |
| 2024-05 | OMNI-EPIC (2405.15568 ◐ abs; ICLR'25) | Foundation model writes environment **and reward code**; archive plus success detector |
| 2025-05 | Absolute Zero (2505.03335 ✓) | One model proposes and solves code tasks; executor verifies; zero external data |
| 2025-06 | SPIRAL †; Self-Challenging Agents † | Zero-sum game self-play; agent writes tool tasks with verifiers |
| 2025-08 | R-Zero (2508.05004 ✓; ICLR'26); SQLM (2508.03682 ◐) | Separate Challenger and Solver; majority-vote pseudo-labels; explicit iteration-collapse analysis (Fig. 3, pseudo-label accuracy table) |
| 2025-09 | Language Self-Play (2509.07414 ◐) | Data-free game-theoretic self-play for instruction following |
| 2025-10 | SPICE (2510.24684 ✓); MAE (2510.23595 ◐) | **Corpus grounding** (FAIR at Meta); proposer–solver–judge triad for unverifiable domains |
| 2025-11 | Agent0 (2511.16043 ✓; COLM'26) | Tool-integrated curriculum and executor agents; multi-turn |
| 2025-12 | R-Few (2512.02472 ✓); GenEnv (2512.19682 ◐) | 1–5% human anchors; simulator as curriculum for agents |
| 2026-03/04 | Prism (2603.13309 ◐); CoEvolve (2604.15840 ◐); Vocabulary Dropout (2604.03472 ◐) | Attacks on diversity collapse; agent–data co-evolution on AppWorld/BFCL |
| 2026-05 | EnvFactory (2605.18703 ◐); Survive or Collapse (2605.22217 ◐); G-Zero (2605.09959 ◐); PopuLoRA (2605.16727 ◐); Learning to Build the Environment (2605.14392 ◐) | Executable tool environments at scale; data gate and reward grounding as separate levers; verifier-free open-ended self-play; population cross-evaluation |
| 2026-07 | Anchored Self-Play (2607.03523 ✓; ICML'26) | Self-play **drifts toward unrealistic** tasks and hurts real ones; anchoring fixes it |
| 2026-08 | SPADE (2608.19197 ✓); Co-Evolution survey (2608.10299 ✓) | Self-written Gym environments with hint regret; 30B scale |
| 2026-09 | Self-Play Pretraining (2609.30063 ◐); CompassPlay (2609.32228 ◐); DEO (2609.34279 ◐); Dr. Free (2609.33565 ◐); False Frontiers (2609.39102 ◐); Env. Evolution for Terminal Agents (2609.04128 ◐) | Self-play from random init over UTM programs; gradient-alignment proposer reward; frozen generator plus Metropolis selection; difficulty-free rewards; co-cheating |
| 2026-10 | ActiveSaddler (2610.00906 ✓ results); R-Quest (2610.04299 ✓ abstract/intro); Selection Is Not Learning (2610.22029 ✓); placebo trial (alphaXiv 2610.reward-placebo-zero-data-self-play ✓); HERA/DAEDALUS (◐) | Curriculum for **harness** learners; validity and novelty feedback; inheritance gating; reward placebo |
| 2026-11 | 2611.06965 ✓; Training Stability under Adaptive Curricula (2611.08787 ◐); survey 2611.08908 ✓; What Actually Evolves (ICLR'27 ecw2VvTeE5 ◐); Test-Time Self-Play (2611.07367 ◐); 2611.23219 ◐; RoboACE (2611.13034 ◐) | Audits of what self-play can and cannot correct; learner-side pathologies in UED; a frozen LLM designs its own self-learning loop and practice games |

### Taxonomy

The surveys split this corner along three axes:
- **What is generated:** Zong's task space vs interaction space.
- **Where the signal is grounded:** Chen's "data gating vs reward grounding"; Yu et al.'s verifiable vs unverifiable.
- **What learns:** Ren's distributional (θ) vs action-level (Σ) self-reference.

Duan's L3 criterion cuts across all three. Selection alone counts as L3 if it adapts to the learner across rounds; one-shot synthetic data does not.

| Axis / approach | Representative papers |
|---|---|
| **A. Generated object** | |
| A1 Problems (QA / code I/O) | AZR, R-Zero, SPICE, R-Few, R-Quest, SQLM, CompassPlay, DEO, Prism, EvoHeir |
| A2 Agentic or tool tasks | Agent0, Self-Challenging†, CoEvolve, GenEnv, Explorer-Zero 2610.19868, Dr. Free, False Frontiers |
| A3 Executable environments (code MDPs, games) | OMNI-EPIC, SPADE, EnvFactory, GameSmith (ICLR'27 gzDYoS7Lo0), LegoEnv 2611.01834, Env. Evolution 2609.04128, Learning to Build the Env. 2605.14392 |
| A4 Opponents / zero-sum games | SPIRAL†, SPAG†, LSP 2509.07414, safety red-team self-play (Zong §3.1) |
| A5 Selection over a real pool (adaptive ordering) | PLR†, ActiveSaddler, Task-CoEvolve 2608.20169, GEPA-Curator 2610.37651 |
| A6 Data from structure (no corpus) | Self-Play Pretraining 2609.30063 (UTM programs) |
| **B. Grounding of the answer key or reward** | |
| B1 Executor | AZR, SPADE, Agent0 (tool), Anchored SP (unit tests); 2611.06965 shows its limit |
| B2 Self-consistency (majority vote) | R-Zero, Agent0, R-Quest, multi-solver disagreement 2608.30035 |
| B3 Corpus / documents | SPICE, SPADE (DCLM docs), Dr. Free, False Frontiers |
| B4 Small human anchor set | R-Few (1–5%), Anchored SP (as few as 50 refs), AnchorLoop 2610.09856 |
| B5 LLM judge / rubric | MAE, J-Zero (ICLR'27 13quQMClZU), G-Zero (Hint-δ), Rubric Curriculum RL 2611.06244 |
| B6 Population / cross-fit | PopuLoRA, CrossFit (False Frontiers) |
| **C. Proposer objective** | |
| C1 Difficulty / learnability band (pass rate in (0,1)) | AZR (1−r̄ inside a band), R-Zero (uncertainty peaking at 50%), Agent0 |
| C2 Variance / Gaussian at p=0.5 | SPICE |
| C3 Regret (PAIRED lineage) | PAIRED, SPADE (hint-based regret) |
| C4 Learning progress (EMA / bandit) | ALP-GMM†, ActiveSaddler, SPADE's EMA baseline |
| C5 Interestingness / novelty | OMNI, OMNI-EPIC, R-Quest novelty, Prism, Vocabulary Dropout |
| C6 Where it moves the solver (gradient alignment) | CompassPlay |
| C7 Weakness diagnosis | DiagEvo (ICLR'27 MkNQtnWiuP ◐), CoEvolve, HERA |
| C8 Frozen generator plus selection (no proposer training) | DEO, Strong Challenger (SPICE), static GPT-5.5 environment pool (SPADE ablation) |
| **D. Learner substrate** | |
| D1 Weights (RL/SFT) | almost all of A1–A4 |
| D2 Harness / memory / playbook (frozen weights) | ActiveSaddler, HERA 2610.06563, DAEDALUS 2610.08048, PREPING 2605.13880, RSIAgent 2609.15364, RoboACE, 2611.23219 |
| **E. Inheritance and stability control** | HeirCommit/EvoHeir 2610.22029, R-Quest validity filter, Survive or Collapse (data gate) |

### Paper cards

**Absolute Zero: Reinforced Self-play Reasoning with Zero Data** ([2505.03335](https://arxiv.org/abs/2505.03335), May 2025, v3 16 Oct 2025; arXiv)
- *What it does.* A single model proposes deduction, abduction and induction Python tasks and solves them. A Python executor both validates tasks and checks answers.
- *Setup.*
  - Models: Qwen2.5-7B Base, Coder, Instruct and Math; Coder-3B and 14B; Llama-3.1-8B.
  - Benchmarks: 3 code (HumanEval+, MBPP+, LCB) and 6 math. Greedy decoding.
  - Proposer reward: 1 − r̄_solve over the solver's rollouts, with 0 for tasks that are unsolvable or trivial.
- *Key results.*
  - Qwen2.5-7B-Coder + AZR: overall 50.4 (+10.2); math avg +15.2; code avg +5.0 ✓ (main results table).
  - Gains grow with scale: +5.7 / +10.2 / +13.2 for 3B / 7B / 14B Coder ✓ (§1).
  - Llama-3.1-8B: AZR 19.2 vs SimpleRL (human data) 20.5 ✓.
  - **Ablation (Table 2, Qwen2.5-7B Base):** "Train Solver Only", with the proposer prompted from the current learner and not trained, scores 45.4 overall vs 46.8 for full AZR. Proposer training is worth only +1.4 ✓.
  - Dropping K-reference conditioning costs about 5 math points ✓.
  - Reports an "uh-oh moment" (a concerning chain of thought) on Llama-3.1-8B ✓.
  - Pass@k up to 512: AZR keeps answer diversity and beats the base in 4 of 5 cases ✓.
- *Limitations / critiques.*
  - Its own ablation shows a small proposer effect.
  - The placebo trial confirms released-checkpoint gains under best-of-3 templates: Coder-3B +7.60, Coder-7B +14.77, Base-7B +4.55 (Table 1) ✓.
  - Template choice cuts Base-7B's apparent gain from +13.8 to +4.5 ✓.
  - Third-party Coder-3B retrains score 16.1–21.4, against 24.8 for the authors' own rerun and 26.5 for the released checkpoint. This figure is cited from an AZR GitHub issue ✓.
  - 2611.06965 shows that an executor key inherits the generator's false beliefs ✓.
  - SPICE's reward ablation ranks the AZR reward last (40.7 vs 44.9, SPICE Table 4) ✓.

**R-Zero: Self-Evolving Reasoning LLM from Zero Data** ([2508.05004](https://arxiv.org/abs/2508.05004), Aug 2025; ICLR 2026)
- *What it does.* A Challenger and a Solver are initialised from one base model and trained alternately with GRPO.
  - The Challenger is rewarded for Solver uncertainty, peaking at 50% agreement with the majority answer, minus a repetition penalty.
  - The Solver trains on majority-vote pseudo-labels, keeping only tasks where 3–7 of 10 answers match ✓.
- *Setup.* Qwen3-4B/8B-Base and OctoThinker-3B/8B; 7 math and 4 general benchmarks; 3 iterations of 15 steps each (the collapse study extends to step 60).
- *Key results.*
  - Qwen3-4B math avg 42.57 → 49.93 ✓ (Table 1).
  - Untrained-Challenger baseline: 45.01 ✓.
  - Ablations (Table 3): without filtering, general avg falls 31.15 → 26.69; without the repetition penalty, math falls 49.07 → 45.76 ✓.
  - **Collapse:** 0.6B peaks at its first iteration (step 15); 4B rises for three iterations and drops sharply at step 60 ✓ (Fig. 3 text).
  - **True pseudo-label accuracy** (GPT-4o oracle) falls 79% → 69% → 63% across iterations ✓ (pseudo-label table).
- *Limitations / critiques.*
  - Internal inconsistencies ✓:
    - the abstract says +6.49 on 4B math, but Table 1 implies +7.36;
    - the Table 3 ablation lists full R-Zero 4B math as 49.07, but Table 1 says 49.93;
    - the text says 8B goes 49.18 → 54.69, but Table 1 says 48.64 → 53.72.
  - R-Zero baselines differ across papers: Qwen3-8B AMC is 61.67 here vs 70.0 in SPICE's Table 1 ✓.
  - R-Few shows R-Zero's 2-gram diversity falling from 35 to below 20 in the first 50 steps, then length inflation ✓.
  - Selection Is Not Learning: 42–52% of R-Zero/Agent0 admitted tasks leave the frontier under fresh samples, and R-Zero peaks at round 8 ✓.
  - R-Quest beats R-Zero by 17.32 at round 10, mostly because R-Zero collapses: R-Quest is only +5.75 over base on Qwen3-4B-Base ✓.

**SPADE: Self-Play in Adaptive Synthetic Executable Environments** ([2608.19197](https://arxiv.org/abs/2608.19197), Aug 2026; ICLR'27 submission DJcwAmJV0T)
- *What it does.* One LLM plays two roles:
  - an **Environment Designer**, which writes complete stateful Gym-style environments as code (reset/step, reward, verifier), grounded on sampled DCLM documents and an environment memory;
  - a Reasoning Agent that learns in those environments.

  The Designer is trained on **hint-based regret**: the agent's return with privileged hints minus its return without them (PAIRED-like).
- *Setup.*
  - Models: Qwen3-4B-Instruct-2507, Qwen3-8B, Qwen3-30B-A3B-Instruct-2507; 400 training iterations ✓.
  - Two settings, games and tool use. 8 held-out benchmarks: AIME'25/'26, GPQA-D, LCB-v6, Reasoning-Gym ×4.
  - Baselines: Fixed-env GRPO (offline pool written by the same backbone) and Fixed-env RLVE.
- *Key results.*
  - At 30B the average is 58.3: +8.1 over base (50.2) and +5.3 over RLVE (53.0) ✓ (Table 1).
  - At 4B and 8B the margin over RLVE is only +1.6 and +1.7 (gains over base +5.2 and +5.7) ✓.
  - Tool use at 30B: BFCL v4 multi-turn +5.7 (+10.3 at 4B); ACEBench-Agent +13.9 ✓.
  - Ablations at 30B, games setting (Table 3) ✓:
    - without memory: 53.2;
    - without corpus grounding: 53.5;
    - **static, offline, memory-free environment pool generated by GPT-5.5: 53.0**. This beats the base but recovers only about 35% of SPADE's +8.1;
    - without Designer training and memory: 40.5, i.e. 9.7 *below* the untrained base.
  - The EMA learning-progress reward reaches 55.9, about 70% of the regret gain ✓.
- *Limitations (authors).*
  - The "invisible leash": environment complexity is bounded by the base model.
  - The optimiser is fixed (GRPO).
  - No optimality proof; evaluation is on fixed tasks.
  - **Open:** "whether learned weights or in-context evolution makes the better designer, and at what scale" ✓.
- *Critique.*
  - Gains concentrate on Reasoning-Gym, which is closest in format to the games (+5.8 to +18.3 at 30B). AIME moves only about +1–2 at every scale ✓.
  - The scale-dependent margin rests on one run per cell (no CIs seen).
  - The GPT-5.5 control changes the generator *and* removes adaptivity and memory, so it is not a clean "strong fixed designer" test.

**SPICE: Self-Play In Corpus Environments Improves Reasoning** ([2510.24684](https://arxiv.org/abs/2510.24684), Oct 2025; FAIR at Meta and NUS)
- *What it does.*
  - A Challenger mines documents (NaturalReasoning, Nemotron-CC-Math) and writes question–answer pairs whose answers are grounded in a document the Reasoner never sees.
  - The Challenger's reward is Gaussian in the variance of the Reasoner's correctness, peaking at p=0.5.
  - The two roles share weights; training uses DrGRPO.
- *Setup.* Qwen3-4B/8B-Base and OctoThinker-3B/8B-Hybrid-Base; 11 benchmarks.
- *Key results.*
  - Overall gain vs base: +9.1 / +5.7 / +10.5 / +11.9 ✓ (Table 1).
  - "Strong Challenger" baseline (fixed Qwen3-32B-Instruct generator): +7.2 on 4B but only +2.6 on 8B ✓.
  - In the corpus-grounding analysis, performance is 43.9 with the corpus and 40.7 without it ✓.
  - Reward ablation: AZR 40.7 < threshold 41.4 < R-Zero 43.6 < variance 44.9 ✓ (Table 4).
- *Limitations.*
  - The corpus acts as hidden supervision, so the method is "zero human labels" rather than zero data.
  - Baselines are re-run, and their numbers differ from the original papers.
  - On 4B, a fixed 32B generator recovers about 80% of the gain (7.2 of 9.1).

**Agent0: Unleashing Self-Evolving Agents from Zero Data via Tool-Integrated Reasoning** ([2511.16043](https://arxiv.org/abs/2511.16043), Nov 2025; COLM 2026)
- *What it does.* A curriculum agent and an executor agent share a base model and a Python tool.
  - The curriculum agent is rewarded for uncertainty, tool use and non-repetition.
  - The executor trains with ADPO, i.e. ambiguity-scaled GRPO, in multi-turn mode.
- *Setup.* Qwen3-4B/8B-Base; 3 iterations.
- *Key results.*
  - Qwen3-8B math avg 58.2, vs R-Zero 54.7, AZR 52.6, Socratic-Zero 56.1, and base with tool 53.2 ✓ (Table 1).
  - Math by iteration: 55.1 → 56.5 → 58.2 ✓.
  - On tasks from iterations 1/2/3, the iteration-1 executor's pass rate is 64.0 / 58.5 / 51.0, and tool calls per task go 1.65 → 2.10 → 2.60 ✓.
- *Limitations / critiques.*
  - The "w/o curriculum training" ablation (Table 3, Qwen3-8B) scores math 46.8. That is **below both the untrained base with tool (53.2) and without tool (49.2)**, which points to a confounded ablation rather than a measure of the curriculum's value ✓.
  - Selection Is Not Learning finds that Agent0's inherited updates are often harmful, and EvoHeir beats Agent0 on 45 of 48 benchmark–backbone pairs ✓.

**Selection Is Not Learning: Choosing Model Futures (HeirCommit / EvoHeir)** ([2610.22029](https://arxiv.org/abs/2610.22029), Oct 2026; MBZUAI)
- *What it does.* Names the **selection-to-update gap**: a task that looks learnable need not yield a useful update.
  - HeirCommit freezes each candidate successor and inherits it only if it beats the current model on fresh probes.
  - EvoHeir separates parameter inheritance from experience inheritance through an archive-and-repair step.
  - Regret is decomposed into selection, absorption and confirmation errors.
- *Setup.* Audits the native updates of R-Zero, Agent0 and Skill-SP, over 10 rounds × 3 seeds, on Qwen3-4B/8B-Base, OLMo-3-7B and Gemma-4-E4B-it ✓.
- *Key results.*
  - 60 of 96 Skill-SP updates on exact-valid tasks have non-positive correct-target NLL gain ✓.
  - 42–52% of admitted tasks leave the frontier under fresh responses ✓.
  - HeirCommit cuts the harmful share of accepted updates by 68–83% relative to random inheritance at the same acceptance rate ✓.
  - EvoHeir's math avg is +6.0 / +7.0 / +6.8 / +1.2% relative over Agent0 (Table 1) ✓. The seed SD is about 0.4–0.7.
- *Limitations.*
  - Weight learners only.
  - Fresh probes cost extra compute, and that cost is not set against Agent0 at matched compute.
  - It still evaluates on fixed benchmarks.

**What Actually Evolves in Self-Evolving Reinforcement Learning?** ([OpenReview ecw2VvTeE5](https://openreview.net/forum?id=ecw2VvTeE5), ICLR 2027 submission)
- *What it does.* Audits 6 self-evolving RL methods (R-Zero/AZR style) on 6 models and 7 benchmarks, against RL on human-annotated answers.
- *Key results* ◐:
  1. Most solver gains come from questions the base model already answers correctly.
  2. Self-evolving RL fixes far fewer errors than RL on human-annotated answers.
  3. **Proposers do not teach more after further training.** The same solver trained on a later proposer's tasks does no better, and sometimes worse, than on an earlier proposer's tasks.
  4. A single-update theory says an update can only strengthen answers the model already gives.
- *Limitations.* I have the abstract only (the PDF is blocked). There is no per-skill steering statistic and no frozen-weight variant.

**When Can Self-Play Fix a False Belief? Answer Keys, Pass Bands and Replacement Rules in an Execution-Grounded Loop** ([2611.06965](https://arxiv.org/abs/2611.06965), 4 Nov 2026; AltSlate Labs LLP)
- *What it does.* Runs expert-iteration self-play with Qwen3-4B-Base on output-prediction tasks. It tracks natural beliefs about Python semantics and two **planted** misreadings of task specifications: "at most n" read as < n, and "the k-th smallest" read as 0-indexed.
- *Setup* ✓ (§3.5):
  - Runtime half: 4 arms × 3 rounds × 2 seeds.
  - Spec half: 3 arms × 3 rounds × 2 planting strengths × 2 seeds.
  - One NVIDIA GB10, about 20 minutes per round.
- *Key results* ✓ (abstract, §1):
  - An executor key from the generator's own code encoded the "at most" misreading on every audited task, and the truth rate never rose above 0.03.
  - A key from an implementation by the unplanted model raised it to 0.23–0.58 after three rounds in four runs.
  - The 1–7-of-8 **pass band admits almost no corrective tasks** while a belief is held.
  - Asked for corrective cases, the planted generator wrote 0 of 2,061 checkable tasks, against 27% for the untrained unplanted model in one round.
  - Fixes bring "replacement rules", such as overcounting the nearest value above a limit (0.18–0.38 of control samples at round 3).
- *Limitations.*
  - A toy domain, a 4B model, 2 seeds.
  - The CIs exclude run-to-run variance.
  - The sign test across 4 runs gives p=0.0625.

**Anchored Self-Play for Code Repair** ([2607.03523](https://arxiv.org/abs/2607.03523), Jul 2026; ICML 2026, Stanford)
- *What it does.*
  - Generator–fixer self-play for bug repair.
  - Introduces BugSourceBench, with bugs from four sources: human, human-edited LM, Qwen-7B and GPT-OSS-20B.
  - ASP adds an embedding-similarity reward toward a small reference set and mixes reference bugs into fixer batches.
- *Setup.* Qwen2.5-Coder-7B-Instruct; GRPO.
- *Key results* ✓ (Table 1a/b, §6–7):
  - Average fix rate: base 25.8, standard self-play 29.1, **ASP 36.1**.
  - Self-play improves the average over Fixer-Only by +1.2 pp but degrades on Qwen-7B bugs (−1.5 pp), drifting toward hard but unrealistic bugs.
  - Each anchoring component alone adds 0.4 (mixing) or 1.8 (similarity shaping) over self-play; together they add 7.0.
  - Works with **as few as 50** reference examples. The composition of the reference pool steers which bug sources improve.
- *Limitations.* One domain and one model family. "Realism" is an embedding proxy.

**Guided Self-Evolving LLMs with Minimal Human Supervision (R-Few)** ([2512.02472](https://arxiv.org/abs/2512.02472), Dec 2025; Tencent AI Lab Seattle and WashU tech report)
- *What it does.* An R-Zero-style loop with two changes:
  - the Challenger is conditioned on a few sampled human anchor examples from WebInstruct (1% or 5% of the data);
  - the Solver trains on a mix of human and synthetic data under an online difficulty curriculum.
- *Key results* ✓ (Table 1, ablation table, Fig. 4):
  - Qwen3-8B avg: 55.1 (1%) and 56.7 (5%), vs R-Zero 53.7, AZR 52.8, **SPICE 55.4** and General-Reasoner 56.0.
  - On 4B, **SPICE (51.4) beats R-Few 1% (49.9) and R-Few 5% (50.7)**.
  - Without Challenger training: −1.9 math and −1.0 general.
  - R-Zero's diversity collapses and its questions inflate in length; R-Few's stay stable.
- *Limitations.* Not zero-data. On 4B, corpus grounding without labels does as well.

**Does the Reward Matter? A Placebo Trial for Zero-Data Self-Play** ([alphaXiv 2610.reward-placebo-zero-data-self-play](https://www.alphaxiv.org/abs/2610.reward-placebo-zero-data-self-play), Oct 2026; single author, pre-registered, Zenodo 10.5281/zenodo.23359965)
- *What it does.* Holds three things fixed: the base model (Qwen2.5-Coder-3B), a frozen 100-step stream drawn from AZR's 512 released seed tasks, and the optimiser (REINFORCE++). It then varies the reward:
  - a placebo that rewards correct tag (and code-block) format only, in one run;
  - AZR's execution-checked reward, in 2 runs that differ only by rollout seed.
- *Key results* ✓ (Table 2, Table 3):
  - Placebo vs base: −1.86 on 6 math benchmarks (95% CI [−4.85, 0.62]).
  - Correctness reward: +5.65 and +5.14. Seed 2 was scored on the AZR template only.
  - The mean gain (+5.39) recovers about 72% of the released AZR gain (+7.54).
  - On seed 1 under the AZR template, 93% of the gain is conditional correctness rather than answer extraction.
- *Limitations.*
  - No learned proposer, 100 steps, one model.
  - The CIs do not capture training variance.
- *Implication.* AZR's code→math transfer is not just a format artefact. But the trial also shows that a **fixed seed stream with no proposer** gets about 72% of the gain in 100 steps.

**ActiveSaddler: Automated Curriculum Learning for Agent Harness Optimization** ([2610.00906](https://arxiv.org/abs/2610.00906), Oct 2026; POSTECH, KAIST and Microsoft; ICLR'27 submission xmpyzuagpu)
- *What it does.*
  - Builds a non-stationary bandit whose arms are LLM-abstracted failure patterns.
  - Scores each arm by estimated learning progress, with explore/exploit over unseen scenarios.
  - Chooses which scenarios feed a **harness optimiser** (AutoSaddler, GEPA, Meta-Harness).
- *Key results.*
  - Test Pass@1: +4.4 pp on GAIA2 and +7.5 pp on TB2 over the same optimiser with a fixed, randomly shuffled order ✓.
  - +3.9 / +4.1 pp over difficulty-ordered fixed curricula ✓.
  - Reaches 58.5% dev accuracy on GAIA2 at $298, vs $1,360 for AutoSaddler ✓.
- *Limitations.*
  - Selection only, no generation.
  - 2 optimisation runs; the TB2 test set has 40 tasks; arms are LLM-judged. These points come from the round-1 project review ◐; the appendix shows Runs 1 and 2 ✓.

**OMNI-EPIC** ([2405.15568](https://arxiv.org/abs/2405.15568), May 2024; ICLR 2025)
- *What it does.* A foundation model writes the next "learnable and interesting" task as environment code plus reward code. It keeps an archive of learned and failed tasks and uses a foundation-model success detector ◐ (abstract, §3).
- *Role in the field.*
  - It is the open-endedness anchor for "interestingness" as a third criterion beyond difficulty.
  - The 2026 environment-writing papers (SPADE, LegoEnv, GameSmith) share its code-as-environment interface †.
- *Limitations.* Qualitative; RL agents in simulation; no study of transfer to LLM learners.

**Survey: Toward LLM Self-Play Under Data Scarcity: Effectiveness, Generalizability, and Trustworthiness** ([2611.08908](https://arxiv.org/abs/2611.08908), 6 Nov 2026; Peking University and Shanghai AI Lab; a short position survey, 8 PDF pages including references)
- *Taxonomy.* Three paradigms:
  1. self-evolving (single-model self-training or adaptive learning);
  2. asymmetric co-evolution (proposer/solver, generator/discriminator);
  3. multi-agent evolution (competitive games, multi-role cooperation).
- *Three challenges and proposed directions:*
  - **Data-centric:** seed-bias amplification, and decay in the marginal utility and temporal value of synthetic data. Proposed: "Health-Aware Data Allocation" and "Learnability-Aware Data Valuation".
  - **Generalizable:** diversity collapse, and adaptive curricula in unverifiable domains. Proposed: "Generation–Evolution Pipeline" and "Causality-Aware Reward".
  - **Trustworthy:** noisy self-supervision, unsafe reasoning patterns, data poisoning. Proposed: "Behavioral Trustworthiness Regulation" and "Provenance-Grounded Trust Certification".
- *Small experiments* ✓:
  - Seed strategy changes proposer diversity. Dataset-as-seed gives a Vendi score of 29.1. Seed-free generation gives 7.0–12.0 across GPT-5.5, Claude Opus 4.7, Kimi K2-0905 and DeepSeek-V4.
  - Rubrics make judges more robust to length, format and tone hacking (e.g. DeepSeek-V3.2 0.36 → 0.75).
  - Under EasyRL settings, Qwen2.5-Math-1.5B/7B gain up to the third iteration, after which further iterations give marginal or negative gains.
- *Value.* A useful vocabulary for the problems, not new evidence.

**Survey: Co-Evolution in Agentic Systems** ([2608.10299](https://arxiv.org/abs/2608.10299), Aug 2026; HKUST et al.)
- *Taxonomy.*
  - **Stage 1, agent–agent:** adversarial (pairwise or multi-source), collaborative, evolving organisations.
  - **Stage 2, agent–environment:** task space (exposure/selection vs adaptive generation), feedback space (preference, outcome, consistency), interaction space (executable world construction vs world models).
  - **Stage 3, meta co-evolution:** the evolution mechanism Ω is revised from a co-evolving system's joint trajectory, across what / when / how / where / evaluate.
- *Key claims* ✓:
  - A cross-paper figure shows that Stage 1–2 gains "become smaller as evolution approaches a plateau".
  - Only RQGM (Iacob et al. 2026) meets Stage 3. Gödel Agent, HyperAgents, PromptBreeder and MemEvolve are "single-entity precursors".
  - Calls for historical cross-play, component ablations and held-out evaluators.
- *Limitations (self-stated).* Safety is not operationalised.

**Survey sections used for framing** ✓:
- **Chen et al. 2607.07663, §4.4 and failure modes.**
  - Self-play is "the closest existing training paradigm to an autonomous curriculum".
  - Its central finding is stability: collapse is the default when the data gate or reward grounding fails (citing 2605.22217).
  - Failure modes:
    - distributional collapse (the "entropy-reservoir" principle, and Zenil's theorem that vanishing exogenous signal implies degeneration);
    - **diversity collapse** ("novelty is a consumable resource that closed loops deplete");
    - **frame lock-in**.
  - Open question: "how little external grounding suffices, and no one has established the exchange rate."
- **Duan et al. 2609.11873, §3.4.**
  - Self-play and adaptive generation sit at L3.
  - The valid / difficult / durable triad.
  - "Difficulty-conditioned proposal offers partial rather than exact curriculum control" (PSV case).
- **Ren et al. 2607.13104.**
  - Self-generated tasks keep the signal extrinsic: the "agent may choose what to try, while the environment determines whether the attempt succeeds".
  - Game self-play "can create brittle competence", with strategic cycling.
  - Calls for population-based, non-transitive evaluation.

**Remaining papers**

| id | title | one-liner | mark |
|---|---|---|---|
| 2510.23595 | Multi-Agent Evolve (MAE) | Proposer, Solver and Judge from one LLM for unverifiable domains; +4.54% avg on Qwen2.5-3B-Instruct | ◐ |
| 2610.04299 | Questioning the Questions: Sustaining Self-Evolution in Reasoning Models (R-Quest) | Invalid questions grow across rounds, and answer-consistency filtering *raises* their share. Lexical diversity controls miss equivalent rewrites. Validity and novelty feedback give +5.75 over base (Qwen3-4B-Base) and +17.32 over a collapsed R-Zero at round 10 | ✓ (abstract/intro) |
| 2609.39102 | False Frontiers | Proposer–solver "co-cheating" in search agents. CrossFit cuts false-agreement mass from 6.1% to 3.0% (4B) and from 8.8% to 3.7% (9B) | ◐ |
| 2609.32228 | CompassPlay | Proposer rewarded by gradient alignment with reference tasks: +1.5 code and +2.7 OOD math over the AZR reward; same Lean coverage with 40% fewer GPU-hours | ◐ |
| 2609.34279 | DEO | No challenger training: a frozen LLM generator plus a Metropolis tilt toward solver-adapted tasks | ◐ |
| 2609.33565 | Dr. Free | Removes difficulty rewards for search-agent self-evolution; difficulty cannot detect shortcut questions | ◐ |
| 2605.22217 | Survive or Collapse | Self-play stability is governed by two asymmetric levers, the data gate and reward grounding | ◐ |
| 2603.13309 / 2604.03472 | Prism; Vocabulary Dropout | Two fixes for proposer diversity collapse in R-Zero loops | ◐ |
| 2605.16727 | PopuLoRA | LoRA teacher and student populations with cross-evaluation replace self-calibration (built on AZR) | ◐ |
| 2605.09959 | G-Zero | Verifier-free open-ended self-play; Hint-δ reward | ◐ |
| 13quQMClZU | J-Zero (ICLR'27) | Challenger–Solver–Judge; keeps improving for 10 iterations where baselines degrade after 2 | ◐ |
| 2608.30035 | Multi-solver disagreement reward | Heterogeneous solvers replace single-solver uncertainty, which collapses to 0 | ◐ |
| 2608.21871 | LURE | Pursuit–evasion self-play that places tasks along the environment's difficulty axis | ◐ |
| 2605.14392 | Learning to Build the Environment | Zero-data reasoning RL through self-synthesised verifiable environments; needs "solve–verify asymmetry" | ◐ |
| gzDYoS7Lo0 | GameSmith (ICLR'27) | 26 synthesised zero-sum games and 336 verified variants; Qwen3-4B 13-benchmark avg 45.5 → 62.5 (distillation) → 64.9 (self-play) | ◐ |
| 2609.30063 | Self-Play Pretraining with Zero Data | From random init, a generator writes UTM programs and a learner predicts their bytes; zero-shot loss on natural data scales with compute | ◐ |
| 2512.19682 | GenEnv | Simulator as curriculum policy with an α-curriculum reward; up to +40.3% on 7B; beats Gemini-2.5-Pro offline augmentation with 3.3× less data | ◐ |
| 2605.18703 | EnvFactory | 85 verified executable tool environments → 2,575 trajectories; up to +15% on BFCLv3 | ◐ |
| 2604.15840 | CoEvolve | Forgetting and uncertainty signals steer task synthesis; AppWorld/BFCL +19.4 / +15.6 / +18.1 (three backbones) | ◐ |
| 2609.04128 | Environment Evolution for Terminal Agents | Off-policy, incremental difficulty evolution of terminal environments | ◐ |
| 2611.01834 | LegoEnv | Self-evolving environments by modular recombination | ◐ |
| 2610.06563 | HERA | Harness–environment co-evolution for abstention, 61.7 → 83.3; 1 run, 60-pair held-out set | ◐ (round 1) |
| 2610.08048 | DAEDALUS | Self-generated tasks for frozen memory come within 0.6–4.0 MSR of curated ones, using 6–14× more rollouts | ◐ (round 1) |
| 2610.28815 | SSR (Forging More Tasks) | Harness evolvers get *worse* as the real pool grows from 40 to 200; subset racing fixes it | ◐ (round 1) |
| 2611.13034 | RoboACE | Co-evolves curriculum and execution harnesses for a frozen robot policy; no proxy-vs-truth tracking | ◐ (round 2) |
| 2611.23219 | Do LLM Agents Still Need to Be Told How to Self-Improve? | A frozen agent with a budget writes its own practice games and learning loop; its strategies resemble self-generated curricula | ◐ |
| 2611.08787 | Training Stability under Adaptive Curricula | UED curricula induce learner pathologies (parameter growth, loss of representational diversity); stabilising the learner helps without changing the curriculum | ◐ |
| 2610.40364 | Is It the Curriculum or Just the Easy Start? | Benchmarks PLR, ACCEL, SFL and PAIRED on quadrupeds; asks whether the curriculum or the easy start drives the gains | ◐ |
| 2611.07367 | Test-Time Self-Play | Asymmetric self-play at test time without a verifier (LCB-v6) | ◐ |
| 2611.06244 | Rubric Curriculum RL | Generation–verification gap in non-verifiable domains | ◐ |
| 2509.07414 / 2508.03682 | Language Self-Play; Self-Questioning LMs | Early data-free proposer/solver and game variants | ◐ |
| 2012.02096 / 1901.01753 / 2010.03934 / 2203.01302 / 2212.10560 / 2306.01711 | PAIRED / POET / PLR / ACCEL / Self-Instruct / OMNI | Classical UED and open-endedness lineage | ◐ / † |

### Consensus, debates & contradictions
1. **Consensus: closed loops degrade, and grounding delays it.** Evidence:
   - R-Zero's collapse, with pseudo-label accuracy falling from 79% to 63% ✓;
   - R-Quest's growth in invalid questions ✓;
   - SPICE's −3.2 drop without the corpus (43.9 → 40.7) ✓;
   - ASP drifting toward unrealistic bugs ✓;
   - Chen's entropy-reservoir and Zenil framing ✓.

   Every successful 2026 method imports exogenous signal: a corpus, anchors, an executor, a population, or fresh probes.
2. **Debate: does training the proposer matter?**

   For a small effect:
   - AZR: +1.4 ✓;
   - R-Few: +1.9 math ✓;
   - What Actually Evolves: later proposers do not teach more ◐;
   - DEO: a frozen generator with selection works ◐;
   - placebo trial: a fixed seed stream recovers 72% of AZR's gain in 100 steps ✓.

   For a large effect:
   - Agent0: −11.4 math without curriculum training, but that ablation falls below the base model ✓;
   - SPADE: removing Designer training and memory gives 40.5 vs 58.3 ✓. Yet a static GPT-5.5-generated pool reaches 53.0, about 35% of the gain ✓.

   The honest summary is that the generator's *quality and grounding* matter. The *RL training of the proposer* is a modest, model-dependent add-on. It has never been compared with "stronger fixed generator plus selection" at matched compute. SPICE's Strong Challenger comes closest (+7.2 vs +9.1 on 4B).
3. **Contradiction on which proposer signal is best.** Each paper's own ablation favours its own objective:
   - SPICE: variance > R-Zero uncertainty > threshold > AZR ✓;
   - SPADE: regret > EMA learning progress ✓;
   - CompassPlay: gradient alignment > difficulty ◐;
   - Dr. Free: no difficulty reward at all ◐;
   - DiagEvo: diagnosis > learnability ◐.

   There is no shared testbed, budget or seed protocol. Difficulty bands also *censor* corrective tasks: 2611.06965's pass band admits almost none ✓.
4. **Debate: sharpening vs new capability.**
   - For sharpening:
     - What Actually Evolves: gains concentrate on already-solved items, and a single update only sharpens ◐;
     - 2611.06965: the generator writes 0 of 2,061 corrective tasks ✓.
   - Partly against:
     - AZR keeps answer diversity at pass@k up to 512 (beats base in 4 of 5 cases) ✓;
     - the placebo trial shows that reward *content* matters ✓.

     Neither shows new competence.
5. **Benchmark numbers are not comparable across papers.**
   - R-Zero's own Qwen3-4B/8B numbers disagree between the abstract, the text and the tables ✓.
   - Its Qwen3-8B AMC is 61.67 in R-Zero and 70.0 in SPICE ✓.
   - Bases differ by evaluation harness: Selection Is Not Learning's Qwen3-4B math base is 25.81, while SPICE's Qwen3-4B base is 35.8 overall ✓.
   - Prompt templates can move AZR's apparent gain from +13.8 to +4.5 ✓.

   Treat cross-paper SOTA claims of ±3 points as noise.
6. **Scale.** Gains and robustness grow with model size:
   - AZR: +5.7 → +13.2 from 3B to 14B ✓;
   - R-Zero: collapse is delayed at 4B relative to 0.6B ✓;
   - SPADE: margin over RLVE is +1.6 at 4B vs +5.3 at 30B ✓.

   But SPADE's "invisible leash" (environments bounded by the base model) and the sharpening audits suggest scale raises the ceiling without changing the mechanism.
7. **Harness learners.**
   - Selection-based curricula help (ActiveSaddler ✓).
   - Generation-based ones help, but with weak controls (HERA, DAEDALUS ◐).
   - Adding data can hurt (SSR ◐).
   - The UED learner-pathology result (2611.08787 ◐) has no text-space counterpart yet.

### Open problems
1. **Matched-compute proposer test.** Compare a trained proposer with two alternatives at equal FLOPs and solver rollouts, with ≥3 seeds:
   - a frozen strong generator plus selection (DEO style);
   - a fixed seed stream (placebo style).

   No paper we have read does all three (◐: a project-level claim, not exhaustively verified).
2. **Out-of-loop "taught vs sharpened" ledger.** Keep a per-item or per-rule account of which errors the curriculum *fixes* and which correct answers it *sharpens*, against exact ground truth (What Actually Evolves, 2611.06965). Report it as a standard metric beside pass@1.
3. **Exchange rate for grounding.** Measure dose–response against rounds-to-collapse for:
   - corpus fraction;
   - anchor count (ASP: 50 refs suffice; R-Few: 1–5%);
   - population size.

   2607.07663 explicitly says this exchange rate is unknown.
4. **Band censorship.** Learnability bands (1–7 of 8 correct) exclude the very tasks that would correct a confidently held error. Design and test band-free admission rules that still give a gradient signal (2611.06965, Dr. Free).
5. **Inheritance gating vs task gating.** HeirCommit shows that the inheritance decision matters more than the task decision. Two things are untested: its cost–benefit at matched compute, and a counterpart for harness learners.
6. **Unverifiable domains.** Judge co-adaptation (J-Zero, MAE, G-Zero) needs a placebo and a held-out-judge audit like the code→math trial. Co-cheating (False Frontiers) is the expected failure mode.
7. **Substrate swap (designer and learner).** SPADE's open question is whether "learned weights or in-context evolution makes the better designer". Its mirror question: do regret, learning-progress and variance teachers rank the same for harness learners as for weight learners?
8. **Measuring open-endedness.** Every paper evaluates on fixed benchmarks (SPADE limitation). There is no accepted metric yet for novelty that keeps generating learnable signal; cf. OMNI-EPIC's archive and Zong's Stage-3 criterion.
9. **Learner trainability under self-curricula.** Study plasticity loss and representational collapse in LLM learners under adaptive curricula, separately from proposer collapse. 2611.08787 covers RL only.
10. **Safety of closed loops.** AZR's "uh-oh moment", Yu et al.'s unsafe-pattern amplification, and poisoning of seeds and corpora. There are no measured rates, only anecdotes.

### Where it is heading (to mid-2027)
- **More audits than methods.**
  - Expect ICLR rebuttal revisions of What Actually Evolves and 2611.06965 that add per-category or yoked controls.
  - Expect ICML'27 versions with proposer-swap and replay controls.
  - The methods lane (Zero-variants per domain: SQL-Zero, Tempo-Zero, Audio-Zero, MTA-Zero, Explorer-Zero) will keep growing, with diminishing novelty. Our field forecast (round2/field_forecast.md) calls it "saturated for methods".
- **Environments as code become the default unit.** SPADE, GameSmith, LegoEnv, EnvFactory and Learning to Build the Environment point toward reusable, verifiable environments with memory and corpus grounding, scaled to 30B and beyond.
- **Grounding and gating become standard parts of the recipe.** Expected baselines will be:
  - a corpus or anchor set;
  - validity and novelty filters (R-Quest, Prism);
  - fresh-probe inheritance gates (HeirCommit).

  Proposer RL may become optional (DEO, Strong Challenger).
- **Move into unverifiable and agentic domains.** Judge co-evolution (J-Zero), rubric curricula, and search or GUI agents (VISTA, Dr. Free, False Frontiers), with co-cheating as the central risk.
- **Frozen-weight curricula.** Harness and memory learners whose curriculum writer is itself an editable artifact (RoboACE, 2611.23219, HERA, ActiveSaddler follow-ups). This is the Stage-3 "meta co-evolution" that Zong says is nearly empty.
- **Scaling claims.** Self-Play Pretraining's compute-scaling curve is an early "scaling law" claim for self-play † (we have not checked whether it is the first). Expect follow-ups to test it on generators structured like natural language.

### Connection to our ideas
- **Blind-Spot Hiding / curriculum-Goodhart** (round-2 #5; FINAL_REPORT): *directly affected.*
  - The weight-side phenomenon is now well established:
    - What Actually Evolves ◐;
    - 2611.06965: 0 of 2,061 corrective tasks, and pass-band censorship ✓;
    - Anchored Self-Play drift ✓;
    - R-Few and R-Quest: diversity and validity decay ✓.
  - What remains novel is narrow:
    - a **frozen-weight meta-agent editing its own task writer**;
    - a **signed per-rule steering statistic against exact truth**;
    - a **yoked twin**.
  - Must-cite list: ecw2VvTeE5, 2611.06965, 2610.22029, ASP, R-Quest, ActiveSaddler (as a named arm), CompassPlay, DEO.
  - 2611.06965 is closer to our idea than the round-2 sweep implied. Re-read it in full before any write-up.
- **#1 Taught, Not Selected:**
  - 2611.06965 is a weight-level example of the "reading channel". The answer key inherits the generator's false belief, and the loop cannot remove it without a decorrelated key.
  - False Frontiers' co-cheating is the score-channel counterpart.
  - Both are strong motivating citations. Neither tests reflective (text) optimisers.
- **#3 Gate, Not Willingness, and the 2+3 merge:**
  - Selection Is Not Learning is the closest weight-side analogue of "where improvement dies".
  - Its three stages map onto our propose / propagate / pay-off funnel: task admission (proposal), update absorption, and confirmation.
  - HeirCommit's fresh-probe gate is a ready-made gate arm.
  - Its 68–83% reduction in harmful commits ✓ is a citable prior for "the gate matters".
- **#5 Order-0 Rewards Breed Routers:** the proposer-reward debate is the same question one level down.
  - Difficulty and variance rewards are *order-0 proxies*: the solver's pass rate on the task.
  - CompassPlay's "where it moves the solver" and HeirCommit's realized-transition test are *order-1-style* rewards.
  - What Actually Evolves (later proposers teach no more) is evidence that order-0 proposer rewards do not produce better teachers.

  This gives #5 a cross-substrate framing.
- **#7 Varying Goals and #9 MetaShift:**
  - OMNI-EPIC, SPADE and LegoEnv are ready-made sources of *changing worlds*, i.e. procedurally varying goals.
  - 2611.23219 shows that a frozen agent can be left to write its own practice games.
  - Use a SPADE- or OMNI-style generator as the "goal-switching" schedule.
  - Caveat: self-written worlds drift toward what the agent can already do (ASP, R-Few), so a fixed external schedule is the cleaner manipulation.
- **#8 Openness Ladder:** SPADE supports the claim that optimal openness depends on model strength, through its "invisible leash" (designer complexity bounded by model scale) and its scale-dependent margin (+1.6 at 4B vs +5.3 at 30B) ✓. Here the writable surface is the environment, not the harness.
- **#6 Meta-Overfitting:** in ASP, the composition of the reference pool steers which bug sources improve ✓. This is a clean weight-side example of curricula specialising to "home" distributions, useful as related work.
- **Instrumentation shared across ideas:**
  - Exact-truth task worlds can come from SPICE's and SPADE's corpus grounding and environment memory, plus GRADUS-style guarded operators (round 1).
  - The placebo-trial design is the template for our matched controls: fixed stream, vary only the reward, pre-registered, paired bootstrap.

#### Fact-check log

Full texts were re-opened from `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/<id>/paper.txt`; the placebo trial came from alphaXiv fullText.

| # | Claim | Verdict | Source location |
|---|---|---|---|
| 1 | AZR Coder-7B overall 50.4 (+10.2), math +15.2, code +5.0; scale gains +5.7/+10.2/+13.2; Llama 19.2 vs SimpleRL 20.5 | confirmed | 2505.03335 main results table (Coder row); §1 bullet "Bigger bases yield bigger gains"; Llama table |
| 2 | AZR ablation: solver-only 45.4 vs full 46.8 (+1.4); w/o K references about −5 math | confirmed, **clarified** that it is on the Qwen2.5-7B *Base* model, not Coder | 2505.03335 Table 2 and "Research Question 7" text |
| 3 | R-Zero 4B math 42.57→49.93; abstract +6.49; 8B text 49.18→54.69 vs table 48.64→53.72; ablations 31.15→26.69 and 49.07→45.76 | confirmed; **added** a third inconsistency (Table 3 lists full R-Zero at 49.07 vs 49.93 in Table 1) | 2508.05004 abstract l.37; Table 1; §4 text; Table 3 |
| 4 | R-Zero pseudo-label accuracy 79→69→63%; 0.6B peaks at step 15; 4B drops at step 60 | confirmed | 2508.05004 pseudo-label table; Fig. 3 text |
| 5 | "R-Zero = first documented iteration collapse" | **corrected** (softened): "first" is unverifiable; reworded as "explicit iteration-collapse analysis" | 2508.05004 §4 (no priority claim in the paper) |
| 6 | SPADE 30B 58.3, +8.1 over base, +5.3 over RLVE; +1.6/+1.7 at 4B/8B; BFCL +5.7, ACEBench +13.9; ablations 53.2/53.5/53.0/40.5; EMA 55.9 (~70%) | confirmed | 2608.19197 Table 1, Table 3, ablation text, Fig. 11 caption |
| 7 | SPADE "fixed GPT-5.5 designer 53.0" | **corrected**: it is a static, offline, memory-free environment pool generated by GPT-5.5, which recovers about 35% of the gain | 2608.19197 Table 3 caption and ablation discussion ("recovers only about 35%") |
| 8 | SPICE gains +9.1/+5.7/+10.5/+11.9; Strong Challenger +7.2 (4B) / +2.6 (8B); no corpus 40.7 vs 43.9; reward ablation 40.7<41.4<43.6<44.9; R-Zero 8B AMC 70.0 | confirmed (generator is Qwen3-32B-Instruct; affiliation is FAIR at Meta and NUS) | 2510.24684 Table 1, Table 4, corpus analysis |
| 9 | Agent0 8B math 58.2 vs R-Zero 54.7 / AZR 52.6 / Socratic 56.1 / base+tool 53.2; iterations 55.1/56.5/58.2; 64.0/58.5/51.0 pass and 1.65→2.60 tool calls; w/o curriculum training 46.8 | confirmed; **added** that the ablation is on Qwen3-8B and also falls below the base without tool (49.2) | 2511.16043 Table 1, Table 3, iteration table |
| 10 | Selection Is Not Learning: 60/96; 42–52%; 68–83% vs random inheritance at equal acceptance; EvoHeir +6.0/+7.0/+6.8/+1.2% over Agent0; 45/48; R-Zero peaks at round 8; 4 backbones × 10 rounds × 3 seeds | confirmed | 2610.22029 abstract; Fig. 1 caption; Table 1; §6.3; Fig. 5 caption |
| 11 | 2611.06965: truth ≤0.03; decorrelated key 0.23–0.58; 0 of 2,061; overcount 0.18–0.38; design 4×3×2 and 3×3×2×2; GB10; p=0.0625 | confirmed | 2611.06965 abstract; §3.5 |
| 12 | ASP: 25.8 / 29.1 / 36.1; components +0.4 / +1.8, together +7.0; −1.5 pp on Qwen-7B; as few as 50 refs; ICML | confirmed (the −1.5 pp is relative to Fixer-Only) | 2607.03523 Table 1a; §6 text; "as few as 50 reference examples"; header "Proceedings of the 43rd ICML" |
| 13 | R-Few: 8B 55.1/56.7 vs R-Zero 53.7, AZR 52.8, SPICE 55.4, GR 56.0; 4B SPICE 51.4 > 49.9/50.7; −1.9 math / −1.0 general; R-Zero diversity 35→<20 in 50 steps | confirmed (affiliation: Tencent AI Lab Seattle and WashU) | 2512.02472 Table 1; ablation text; §4.2.4 and Fig. 4 |
| 14 | Placebo trial: −1.86 [−4.85, 0.62]; +5.65/+5.14; ~72% of +7.54; 93% conditional correctness; released gains +7.60/+14.77/+4.55; +13.8→+4.5; retrains 16.1–21.4 vs 26.5 | confirmed; **clarified** that 93% is for seed 1 under the AZR template and judge J1, that seed 2 was scored on one template, and that the authors' own rerun scored 24.8 | alphaXiv fullText: Tables 1–3, §3.1, §5 |
| 15 | ActiveSaddler +4.4 / +7.5 pp; +3.9 / +4.1; 58.5% at $298 vs $1,360; affiliation "POSTECH/KAIST/MSR" | numbers confirmed; affiliation **corrected** to POSTECH, KAIST and Microsoft (not specifically MSR) | 2610.00906 abstract; §5 results; cost paragraph; header |
| 16 | Survey 2611.08908: Vendi 29.1 vs 7.0–12.0; DeepSeek-V3.2 0.36→0.75; EasyRL saturates after round 3; "6-page" | numbers confirmed (model is Kimi K2-0905); length **corrected** to "8 PDF pages including references" | 2611.08908 Fig. 3; rubric table; Fig. 5 text; pdfinfo |
| 17 | 2607.07663 "exchange rate" quote; "novelty is a consumable"; frame lock-in. 2609.11873 "learner-conditioned", "partial rather than exact", "Neither correctness nor…" | confirmed | 2607.07663 failure-modes section and open problem 1; 2609.11873 §3.4 and §6 |
| 18 | 2608.10299: plateau claim; RQGM the only Stage-3 example; "single-entity precursors" | confirmed | 2608.10299 cross-paper figure text; Stage-3 section |
| 19 | R-Quest: invalid questions grow and consistency filtering raises their share; +17.32 at round 10; +5.75 over base | confirmed (on Qwen3-4B-Base) | 2610.04299 abstract; Fig. 1; intro |
| 20 | OMNI-EPIC marked "✓ abs" | **re-marked** ◐ (abstract only; not re-checked in full) | none |
| 21 | Self-Play Pretraining is "the first scaling-law claim for self-play" | **unverifiable**; softened to "an early … claim †" | none |
| 22 | "No paper does all three" (matched-compute proposer test) | **unverifiable** (absence claim); marked ◐ as a project-level claim | none |
| ID-1 | 2611.06965 → "When Can Self-Play Fix a False Belief? …" | resolves correctly | paper.txt header (arXiv v1, 4 Nov 2026) |
| ID-2 | 2608.19197 → "SPADE: Self-Play in Adaptive Synthetic Executable Environments" | resolves correctly | paper.txt header |
| ID-3 | 2610.04299 → R-Quest | resolves; the arXiv title is "Questioning the Questions: Sustaining Self-Evolution in Reasoning Models" (R-Quest is the method name). Full title added | paper.txt header |
| ID-extra | 2610.22029, 2512.02472, 2607.03523, 2611.08908, 2510.24684, 2511.16043 | all resolve to the stated titles | paper.txt headers |

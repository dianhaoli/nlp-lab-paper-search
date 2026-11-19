## Self-created tools and domain or multimodal self-evolving agents

*Marks: ✓ = checked in the full text (section/table cited), either in this fact-check pass or earlier in FULLTEXT_VERIFICATION.md; ◐ = abstract only (arXiv / alphaXiv / OpenReview); † = from memory or a secondary citation, unverified. "pp" = percentage points.*

*Full texts re-opened in this fact-check pass: SEAR, VideoResearcher, ChartEvo, MetaForge, Tool-R0, SIMIT, Agent S4 (Evolving Agent Interfaces), ReTerm, The Trinity (§4.3–4.4, App. D), the Cross-Scenario survey 2611.14256, AdaRSI (abstract and intro), Agent0 and MetaAct (abstracts and intro). Flex-RSI was verified in an earlier session (FULLTEXT_VERIFICATION.md). The texts are under /tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/<id>/paper.txt.*

### At a glance
- **What this corner is.** Self-improving agents that improve their action and perception surface, not only their prompts. That surface means tools, toolchains, interfaces and code policies. The corner also covers domain loops (multimodal, video, chart, GUI/OS, terminal, embodied), where self-improvement has to cope with expensive rollouts and weak verifiers.
- **Duan 2609.11873 levels.** Almost everything here is L1–L2. A human fixes the objective and the acceptance rule, and the agent chooses *what tool to build* †.
- **Dai et al. components.** In their Prompt/Memory/Skill/Harness terms, these are Skill/Harness updates with "persistent experience" (pattern ii). They are almost never "improver update" (pattern iii) †.
- **Two families.**
  - (a) **Weight-space self-play over tool use**: Tool-R0, Agent0, AgentForge, Marvo, RarePlay, and the VLM self-play line.
  - (b) **Training-free libraries of executable tools or harnesses**, built by a proposer that is usually stronger than the agent: VideoResearcher, ChartEvo, Agent S4, SEAR code-as-policy, The Trinity, AgentFactory.
  - MetaForge is the clearest hybrid. It forges tools inside GRPO and registers them into a persistent pool.
- **State of the art (each is a single-paper claim).**
  - Long video: VideoResearcher, +2.4 average over its own base (72.1→74.5) ✓ Table 1.
  - Long-tail charts: ChartEvo, 48.0→93.6 on 9 ChartBench types ✓ Table 1.
  - Computer use: Agent S4, 21.3% binary on OSWorld-V2 with Qwen3.8-27B ✓ Table 2.
  - Multimodal tool use: MetaForge, Qwen3-VL-8B 73.8→88.1 across 12 benchmarks, averaged over 3 runs ✓ Table 1, l.447.
  - Terminal: ReTerm-9B, 34.8% on TB2.0 ✓ Table 1.
  - Robotics: SEAR is a controlled benchmark (330 simulated + 5 real tasks) with matched controls for resampling, luck, privileged state and budget ✓ §1. SEAR does not call itself the "first"; that is our judgment from the corpus †.
- **Emerging consensus.**
  - In domains bound by perception and measurement, executable tools beat instruction or skill edits. ChartEvo: evolved tools 84.4 vs skill-only 55.6 ✓ Table 4. VideoResearcher: SkillOpt scores −2.3 against the base ✓ Table 1.
  - Gains concentrate where a cheap execution check or answer key exists.
  - Libraries help most when the base model succeeds "sometimes but not reliably" (SEAR: +12.0 pts at 30–80% base success ✓ §3.2).
- **Matched-baseline gains are small.** Against a matched self-improvement baseline or the human seed, gains shrink to 1–3 points:
  - VideoResearcher vs Meta-Harness: +1.0 average (74.5 vs 73.5) ✓ Table 1.
  - Agent S4 evolved vs seed repository: +1.86 pp, about 2 of 108 tasks ✓ Table 2/4.
  - The big headline numbers compare against a weak base, use a stronger proposer model, or use oracle routing.
- **Self-built verifiers are the main hazard.**
  - SEAR: one success detector the agent built was wrong on 91% of 699 rollouts and was saved to memory as a "fact". That run scored 4.6%, against 15.5% and 23.6% for its siblings ✓ §3.3.
  - Multimodal consensus labels degrade. 24% of majority-vote labels are wrong (VQS ◐). VisPlay's pseudo-label accuracy fell 72.0→61.0 over 3 iterations (✓ via 2611.14256 §4.2).
- **Biggest open problem.** Almost nobody measures whether created tools *retain* value and *transfer* under matched budgets and several seeds.
  - The 2611.14256 survey states "No method measures retention on earlier-round goals" for multimodal reasoning (§4.4) and for GUI/Web (§6.4) ✓.
  - The same survey names one exception in generation: RSI in UMMs, where earlier-vocabulary accuracy fell 72%→60% after a round replaced that vocabulary ✓ §5.4.
  - The survey's cross-scenario portability verdicts are explicitly untested ✓ abstract.
  - The improver that writes the tools is never itself edited.

### How it developed
| Month | Paper | What changed |
|---|---|---|
| 2023-05 | Voyager (2305.16291) †; LATM †; CREATOR † | Libraries of code skills; tool *making* separated from tool *use* |
| 2023-09 | CRAFT † | Retrievable toolsets abstracted from solutions |
| 2025-02 | ToolMaker (2502.11705) † | Tools built from papers and repositories (cited by ChartEvo and VideoResearcher ✓) |
| 2025-05/07 | Alita †; STELLA (2507.02004) † | MCP-style tool creation on demand; a self-evolving "tool ocean" for biomedicine |
| 2025-11 | Agent0 (2511.16043, COLM 2026) ✓ | Curriculum and executor co-evolve from zero data, with a code tool. On Qwen3-8B-Base: +18% math reasoning and +24% general reasoning ✓ abstract, §1 |
| 2026-02 | **Tool-R0** (2602.21320, 24 Feb) ✓ | Zero-data generator–solver self-play for *general* function calling; separate weights are essential |
| 2026-03/04 | AgentFactory (2603.18000) ◐; CoEvoSkills (2604.01687) ◐ | Executable subagents as reusable units; skills checked by a co-evolving surrogate verifier |
| 2026-05/06 | SkillSmith (2606.01314) ◐; **MetaForge** (2606.01801, 1 Jun) ✓ | Skills and tools co-evolve under context budgets; tools forged inside GRPO behind an execution + judge gate |
| 2026-07 | EvoSOP (2607.07321) ◐; Tool-Making and Self-Evolving LLM Agents in Low-Latency Systems (2607.08010, title checked) ◐ | SOP-tools with a merge/prune lifecycle; first production report (−42% p50 latency ◐) |
| 2026-09 | **VideoResearcher** (2609.19664) ✓; DexAgent (2609.35318, title checked) ◐; V-Gym (2609.34682) ◐; LOTS (2609.34151) ◐ | Failure-driven tool synthesis for long video; self-evolving tool libraries reach dexterous robotics; tool *space* pruned by likelihood attribution |
| 2026-10 | **ChartEvo** ✓; **SEAR** ✓; **Agent S4** ✓; **ReTerm** ✓; **The Trinity** ✓(part); AdaRSI ✓(intro); EmbodiedEvo, SERA, EMHO, ReSight ◐; EvolvAttack (2610.15244, title checked) ◐ | Spread into new domains (charts, OS, terminal, robots); a controlled robotics benchmark with resampling and luck controls; interface evolution; capability-tree harnesses; first backdoor on self-evolving VLM memory ◐ |
| 2026-10/11 | **SIMIT** (2611.00053, v1 stamped 11 Oct) ✓ | Test-time "imagination": the model generates its own (question, answer, image) triplets, fixing the answer before the image |
| 2026-11 (1–18) | **Cross-Scenario survey** (2611.14256) ✓; Flex-RSI (2611.08946) ✓FV; ScienceCUA ◐; EVOLVE guardrail tools ◐; SkillEarth ◐; ESI ◐ | A turn to synthesis and measurement: portability verdicts, degradation modes, frozen-improver arms for vision and robotics |
| ICLR 2027 subs | AgentForge, Tempo-Zero, VQS, RISE, VisEvolver, EvoRAM, MetaAct, SEVA, RarePlay ◐ | Program-verified multimodal self-play; tool synthesis by asymmetric self-play; executable action guards |

### Taxonomy
The main axis is **what persistent artifact the agent creates**. Two secondary axes are **who writes it** (the same model, a stronger proposer, or a teacher) and **what admits it** (an execution check, dev-set accuracy, a judge, or nothing).

| Approach | Persistent artifact | Writer / gate | Representative papers |
|---|---|---|---|
| A. Zero-data tool-use self-play (weights) | Policy weights; self-generated tool menus, tasks and gold calls | Same model in two roles; band-pass difficulty reward; JSON/AST match | Tool-R0 ✓, Agent0 ✓, AgentForge (O2jE6PfSFS) ◐, RarePlay (FH5OoGxsBx) ◐, Marvo 2610.15084 ◐, Self-Evolve w/ Reference 2610.09856 ◐ |
| B. Tool forging inside RL | Weights + a persistent pool of forged tools | Own policy; execution + judge gate; capacity/usage retention | MetaForge ✓, AgentForge ◐ |
| C. Training-free libraries of tools or toolchains | Python tools, macro-tools, SOP-tools, local toolchains | Usually a stronger proposer (Codex / GPT-5.6-Sol); dev-accuracy gate | VideoResearcher ✓, ChartEvo ✓, AgentFactory ◐, EvoSOP ◐, SkillSmith ◐, CoEvoSkills ◐, SkillCompiler 2610.39508 ◐, Tool-making low-latency 2607.08010 ◐, SciToolAgent-Evo 2607.28692 ◐, KG-as-Files 2610.18616 ◐ (within one episode) |
| D. Evolution of the interface / action space | Observation and action APIs plus usage knowledge; executable guards | Same model as meta-agent (S4); population selected on validation | Agent S4 ✓, MetaAct (rxyGomjMBA / 2611.20655) ✓(abstract+intro), LOTS ◐, Hybrid Computer Use 2610.41432 ◐ (human-designed) |
| E. Self-generated multimodal supervision | Synthetic (Q, A, image/video) items or programs; weights or ICL demos | Same model; renderer, program or self-verifier | SIMIT ✓, VQS (3ijW6lQHLT) ◐, Tempo-Zero (cizDyhcm7v) ◐, VisPlay † (via survey ✓), Trinity-VLM 2610.04469 ◐, RISE ◐, UniEvo-VL 2609.38721 ◐ |
| F. Embodied code-as-policy and skill libraries | Control code, skills, lessons; sometimes VLA fine-tunes | Frontier LM; success predicate or sealed judge | SEAR ✓, AdaRSI ✓(intro), EmbodiedEvo 2610.18867 ◐, EvoRAM ◐, SERA 2610.29560 ◐, EMHO 2610.08432 ◐, ESI 2611.18176 ◐, ReSight 2610.33487 ◐, DexAgent ◐ |
| G. Training on own failures / recovery | Weights trained from the model's own failure states | Teacher SFT + GRPO with verifier reward | ReTerm ✓ |
| H. Cross-benchmark capability harnesses | Harness tree routed by capability | Frozen frontier backbone | The Trinity ✓, EvolvingBrain 2610.20786 ◐, V-Gym ◐ |
| I. Synthesis, benchmarks, safety | — | — | 2611.14256 ✓, Flex-RSI ✓FV, EmbodiedCLBench 2610.13021 ◐, EvolvAttack ◐, EVOLVE guardrails 2611.02516 ◐ |

### Paper cards

**SEAR: Self-Evolving Agentic Robotics** ([2610.18792](https://arxiv.org/abs/2610.18792), v1 13 Oct 2026, Luma AI ✓ header)
- *What it does.* Runs frontier LMs as self-evolving robot agents in three modes: code-as-policy, agent-as-policy, and auto-research that fine-tunes a VLA. Matched controls separate learning from resampling, familiarity, privileged state, budget and luck ✓ §1–2.
- *Setup.*
  - 7 LMs; 330 simulated tasks in 5 families and 2 physics engines; 5 physical tasks on a two-arm rig.
  - More than four years of agent runtime in total; 1 h per simulated task and 12 h per real task ✓ Fig. 1, §4.4.
  - Controls: best-of-3 at equal total budget, runs with no library ("Vanilla"), and a sealed judge that grades the frozen policy ✓ §2. Simulated results use three runs per condition ✓ §3.2.
- *Key results.*
  - The library's gain depends on base skill. Mean +12.0 pts when no-library TEST success is 30–80% (15 pairs), +3.0 below 30%, +0.9 above 80%. A quadratic fit over 25 pairs peaks at a 43% base (+14.7) ✓ §3.2 Fig.
  - GPT-5.6-sol on RoboLab: 46.9% with no library, 42.4% with its **own** library, 60.6% with the library Opus wrote, 55.7% with Kimi-k3's ✓ §3.2.
  - An agent-built verifier declared success on 699 later rollouts and was wrong on 91% of them. Because it was also the stopping rule, 637 rollouts ended early, and it was kept in memory. The run scored 4.6%, against 15.5% and 23.6% for its siblings ✓ §3.3.
  - Run-to-run spread: two DeepSeek-v4-flash Lineage runs differ by 20.9 pts ✓ §3.3.
  - Generalisation tiers for Opus code policies: 94.4 (LIBERO TEST) → 66.0 (LIBERO-RIG hardware faults) → 47.7 (scene changes) ✓ §4.1.
  - Real robot, code-as-policy (CaP): Fable 5.1 70.2, GPT-6-astra 41.4, Opus 5 35.6. Acting live (agent-as-policy, AaP) lifts GPT-6-astra to 88.0 ✓ §4.4, Table 4.
  - Six practice runs reached ten successes in a row. Only one of them (Fable's Push-T) then succeeded on all ten staged scenes, and two succeeded on only four ✓ §4.4.
- *Limitations / critiques.*
  - Each real-robot entry is one practice run ✓ Table 4 caption.
  - Only skills and memories persist between tasks; the harness and the improver are fixed ✓ §2, l.239 and discussion l.3415.
  - Within the time budget, practice with self-reset under-samples hard starts. Only 10 of Opus's 64 Push-T starts needed a turn of more than 90°, against 6 of the 10 staged scenes ✓ §4.4.
  - The survey 2611.14256 cites SEAR as its example of erroneous writes spreading across later rollouts ✓ §3.6.

**VideoResearcher: Self-Improving Tool Design for Long-Video Understanding** ([2609.19664](https://arxiv.org/abs/2609.19664), v1 17 Sep 2026, JHU, preprint ✓ header)
- *What it does.* A training-free, multi-agent Evolving loop: Auditor → Analyzer → Designer → Coder → Debugger → fixed Evaluator → Verifier → Memory. It writes Python video tools on top of a single `ask_video` primitive. The Worker and the perception model stay fixed.
- *Setup.*
  - Worker GPT-5.6-SOL; perception model Qwen3.5-4B (served on 8×B200); Evolving roles run on the Codex SDK.
  - 300 ALLVB dev questions (videos average nearly two hours); up to 20 rounds.
  - A tool is accepted only if it *strictly* improves dev accuracy. Tests use LVBench, LongVideoBench and Video-MME, kept out of tool design and selection ✓ §4.1.
- *Key results.*
  - Average 72.1→74.5 ✓ Table 1.
  - Baselines on the same base: SkillOpt 69.8 (−2.3), Meta-Harness 73.5 (+1.4), AutoResearch 71.7 (−0.4) ✓ Table 1.
  - Table 3 lists five representative rounds. Two tools were accepted: Scene Landmark Index in Round 2 (dev 68.7→75.0) and Window Atomic Scan in Round 7 (75.0→76.7). Rounds 3, 14 and 17 were rejected ✓ Table 3, §4.4.
  - The final test scores equal the Round-7 scores, so no later round was accepted. The paper does not report how many rounds ran in total or the full rejection count.
  - Round 7 is *below* Round 2 on LVBench (68.4 vs 68.9) and LongVideoBench (75.0 vs 75.5); it is above on Video-MME (80.2 vs 80.0) ✓ Table 2b.
- *Limitations / critiques.*
  - The authors concede that the framework relies on textual trajectory assessment and "repeated selection on one development set" ✓ §5.
  - Selection on the dev set added +8.0 dev points (68.7→76.7) but only +1.0 to +4.1 on test columns (+2.4 average). With 300 questions the dev standard error is about 2.5 pp (my calculation), so a strict-increase gate admits noise.
  - No repeated runs are reported.
  - The Verifier's "Causal Verification Report" is "not an independent causal experiment" ✓ §3.3.

**ChartEvo: Self-Evolving Agent Harnesses for Long-Tail Chart Reasoning** ([2610.28282](https://arxiv.org/abs/2610.28282), v1 20 Oct 2026, CASIA/UCAS, preprint ✓ header)
- *What it does.* Evolves three things together for chart types where MLLMs fail: Skills, executable Tools, and "Self-Evolving Local Toolchains" (SELT, frozen tool compositions).
- *Setup.*
  - Agent Qwen3.7-Plus; **proposer GPT-5.6-Sol** ✓ §4.1.
  - 100 training questions per chart type, with ground truth used for evolution. 9 ChartBench types (450 test Q) and 8 ChartX types (265 test Q) ✓ §4, App.
  - 5% relative-error tolerance; action budget of 15 ✓ §4.
- *Key results.*
  - ChartBench 48.0→93.6. The abstract's "45.6%" means percentage points. GPT-5.6-Sol answering directly scores 68.9 ✓ Table 1.
  - ChartX 53.6→86.1, against GPT-5.6-Sol at 84.4 ✓ Table 2.
  - Ablation on 5 ChartBench types: base 42.8, skill-only 55.6, base harness 57.2, evolved tools 84.4, full 94.8 ✓ Table 4.
  - SELT cuts API calls from 6.19 to 2.73 while accuracy goes from 91.2 to 94.8 ✓ Table 6.
  - Cost per question: 2,907 tokens and 68.8 s, against 2,454.5 tokens and 42.7 s for the base ✓ Table 3.
- *Limitations / critiques.*
  - This is distillation from a stronger proposer, not self-improvement of the agent model.
  - About 30–50 test Q per chart type; a single run; evolution and test data come from the same distribution.
  - The authors flag overly specialised harness components and distribution shift between evolution and test data ✓ Limitations.

**MetaForge: A Self-Evolving Multimodal Agent that Retrieves, Adapts, and Forges Tools On Demand** ([2606.01801](https://arxiv.org/abs/2606.01801), v1 1 Jun 2026, ECNU/ZJU et al., preprint ✓ header)
- *What it does.* Splits behaviour into four steps: Decide, Retrieve, Adapt, Forge.
  - A forged tool (Markdown spec + schema + Python) enters the pool only if it executes *and* a judge model says it matches its description.
  - Training is multi-turn GRPO with a composite reward, and forging is rewarded only in correct trajectories.
  - The pool has a capacity limit, semantic dedup, and retention by usage (threshold Q3 + 1.5·IQR) ✓ §3.4.
- *Setup.* Qwen3-VL-8B on VerlTool, 8×H200. 12 image-text benchmarks with IID/OOD splits for both datasets and tools; 16 baselines. "All reported results are averaged over three independent runs" ✓ §4, l.447.
- *Key results.*
  - Average over all benchmarks: 73.80→88.14 (IID tools) and 74.21→88.98 (OOD tools), against Claude Sonnet 4.6 at 86.88 ✓ Table 1.
  - Forging: 190 attempts / 123 registered (IID); 12 / 11 (OOD) ✓ Table 2.
- *Limitations / critiques.*
  - Image-text only; no audio, video, temporal or interactive settings ✓ Limitations.
  - The semantic gate is an LLM judge.
  - GRPO is keyed to answers, so this is supervised RL, not zero-data.
  - Some closed-model baselines look anomalous, e.g. GPT-5.4 at 36.40 on CLEVR-M ✓ Table 1.

**Tool-R0: Self-Evolving LLM Agents for Tool-Learning from Zero Data** ([2602.21320](https://arxiv.org/abs/2602.21320), v1 24 Feb 2026, UIUC/ETH, preprint ✓ header; "83 votes" †)
- *What it does.* A Generator trained with GRPO writes (question, tool menu, gold call) triples near the Solver's frontier. Its reward is band-pass on the Solver's pass rate over [0.25, 0.75], with Gaussian falloff (σ = 0.12). The Solver then trains on a filtered curriculum ordered from easy to hard ✓ §3, §4.
- *Setup.*
  - Qwen2.5 0.5B/1.5B/3B and Llama-3.2-3B; 3 iterations × 50 steps; 10,000 candidates filtered to 2,000 per iteration; 3 GPUs ✓ §4, App.
  - Evaluated on 5 benchmarks (ToolAlpaca, SealTool, NexusRaven, API-Bank, SNIPS) ✓ Table 1.
- *Key results.*
  - Qwen2.5-1.5B average 24.85→47.84 (+22.99, +92.52% relative) ✓ Table 1.
  - Beats SFT on xLAM (60k), Hammer (210k), ToolACE (12k) and ToolRL (4k; 46.06) ✓ Table 2.
  - Ablations: shared weights 30.42 (−17.42), frozen Generator 41.65, no difficulty reward 43.54 ✓ Table 3.
  - Small models saturate around iteration 3 ✓ RQ8.
- *Limitations / critiques.*
  - No real tool execution. Rewards are JSON/AST matches against the Generator's own gold calls, and pseudo-labels are filtered by agreement with the Solver itself.
  - No multi-run standard errors, which the authors admit ✓ §5.2.
  - Table inconsistency: the API-Bank base score for 1.5B is 19.13 in Table 1 but 10.13 in Table 2 ✓. Table 1's average of 24.85 implies 19.13 is correct.
  - The "separate weights are essential" claim conflicts with single-policy designs such as Tempo-Zero ◐.

**SIMIT: Self-Improving VLMs via Imagination at Test-Time** ([2611.00053](https://arxiv.org/abs/2611.00053), v1 stamped 11 Oct 2026, DTU/Pioneer Centre/Copenhagen ✓ header; ICLR27 sub m4ccvE2AMl)
- *What it does.* Before answering an unlabeled query, a unified model (BAGEL-7B) imagines similar problems.
  - It fixes the answer *before* generating the image, then verifies, repairs and filters by difficulty.
  - The resulting data serves as ICL demos or for fine-tuning.
  - Structured images go through a library of 17 renderers. The library is **human-written and fixed**: "We did not optimize them against benchmark scores" ✓ App. (Skill Library).
- *Setup.*
  - 17 LMMs-Eval-Lite benchmarks; 50 validation / 500 evaluation examples each ✓ App.
  - Baselines (TTRL, EMPO, TTRV, SRLM, Self-Consistency, Self-Refine) are tuned on the same validation data ✓ §4, App.
- *Key results.*
  - SIMIT-ICL: +6.98% mean relative gain at 25× runtime. Test-set FT→ICL: +7.20% at 55×. Best label-free RL baseline (TTRV): +2.07% at 41× ✓ Table 1.
  - Under TTRL on VizWiz, "unanswerable" is the majority vote in 99.2% of training steps. 79.3% of the steps with nonzero advantage still reinforce it ✓ §4.1.
  - SIMIT corrects 14.4% of initially wrong answers and disrupts 2.0% of initially correct ones. Its supervision-error rate is 12%, against 33–38% for the baselines ✓ §4.4.
  - Transfer across benchmarks: +4.3% within a domain, only +0.5% across domains ✓ §4.1.
- *Limitations / critiques.*
  - The gains are relative means over mixed metrics, CIDEr included.
  - Data quality is audited by Claude Sonnet 5, an external evaluator ✓ §4.
  - The model does not create its own tools.
  - Gains on other backbones are small: +2.21% (SenseNova-U1) and +3.97% (Lance) ✓ §4.1.

**Evolving Agent Interfaces (Agent S4)** ([2610.28601](https://arxiv.org/abs/2610.28601), v1 20 Oct 2026, Simular/UCSB †affiliations, tech report)
- *What it does.* A meta-agent reads unlabeled trajectories from synthetic tasks. It mutates *repositories* of interfaces plus usage knowledge, or crosses two of them over. The population is selected on validation tasks that the meta-agent never sees ✓ abstract, §3.
- *Setup.*
  - The task agent **and** the meta-agent are both Qwen3.8-27B ✓ l.394.
  - The seed repository is human-written; Table 5 lists its files and interfaces ✓ App. A.2. The earlier "about 550 lines" figure is not in the text and has been removed.
  - 30 synthetic training tasks for OSWorld V2, written by a GPT-6 Astra coordinator and GPT-5.6-Sol authors; 15 official validation tasks; 30 descendants per run ✓ Table 1, §4.
- *Key results.*
  - OSWorld-V2 binary score: no repository 15.74, seed 19.44, evolved 21.30 ✓ Table 4. Opus 4.8 scores 20.6, at $72.40 vs $1.10 per task ✓ Table 2.
  - The "3.8×" headline is measured against a different harness, Qwen CUA at 5.56 ✓ Table 2.
  - ScienceBoard 55.03→58.58 (93→99 of 169 tasks) against the seed ✓ Table 3. The abstract's 41.4→58.6 uses a different baseline.
- *Limitations / critiques.*
  - Evolution adds about 2 tasks over the seed.
  - The 15 validation tasks are included in the reported full set ✓ l.390.
  - One evolution run per strategy. Denoising was applied only on ScienceBoard, not OSWorld ✓ §4.
  - The variant evolved with a scratch VM gets a higher partial score (51.06 vs 46.90) but its binary score (19.44) only equals the seed ✓ Table 2/4.

**ReTerm: Recovery Training for Self-Improving Terminal Agents** ([2610.18484](https://arxiv.org/abs/2610.18484), v2 14 Oct 2026, Fudan/CUHK/SJTU, preprint ✓ header)
- *What it does.*
  - Snapshots the VM at the base model's failure states.
  - A teacher (DeepSeek-V4-Pro) writes recoveries from those states. After filtering this gives ReTerm-10K, used for SFT.
  - ReTerm-GRPO then forks recovery rollouts from the evolving policy's own failure states, kept in a recency queue ✓ §3.
- *Setup.*
  - Qwen3.5-9B on 16×A100; 6K trajectories for SFT and 4K tasks for RL.
  - Each update uses 8 original and 2 recovery groups of 16 rollouts.
  - Inference at temperature 1.0 "with the task seed fixed at 0" ✓ §4.1.
- *Key results.*
  - TB2.0 20.2→34.8; TBLite 44.2→60.6; SWE-bench Verified 45.2→58.4 ✓ Table 1.
  - Ablation: SFT 25.8, GRPO 28.0, ReTerm-GRPO 34.8 ✓ Table 4.
  - Transfer to three other harnesses: +6.7 to +12.4 ✓ Table 2.
  - AIME25 73.3→93.3 ✓ Table 3.
  - Recovery rates stay at or below 32% for every error type except tool-use errors ✓ §4.5.
- *Limitations / critiques.*
  - "Self-improving" applies only to the RL phase; the cold start is distillation from a teacher.
  - A single sample on small benchmarks (TB2.0 has about 89 tasks †). The +6.8 over GRPO is about 6 tasks.
  - The AIME25 jump is 6 of 30 problems.

**The Trinity: Capability-Driven RSI for Multimodal Agents** ([2610.36077](https://arxiv.org/abs/2610.36077), v2 30 Oct 2026, "The Trinity Research Team" ✓ header)
- *What it does.* Three roles, Creator, Explorer and Evolver, build a capability tree over heterogeneous benchmarks. They generate simplified, verifiable simulated worlds per capability ("Real2Sim2Real") and evolve a harness tree, which a capability-aware router selects from ✓ abstract, §3.
- *Setup.* Frozen Claude Opus 5 on a Prime Agent base harness; 28 Train / 18 Unseen benchmarks, including 2 real-world embodied tasks ✓ §4, Table 1. Some environments are "agent-built from web-searched sources" ✓ Table 1.
- *Key results.*
  - Over 4 iterations, Train 39.1→73.2 and Unseen 42.1→64.1. Reference harnesses: Codex 47.5, DeepSeek 46.2 ✓ §4.3.
  - Eleusis declines 82.2→81.1 between iterations 1 and 4 ✓ §4.3.
  - With the router choosing for itself, the scores are 71.27 Train / 60.96 Unseen, against Oracle 73.16 / 63.61 ✓ §4.4 Table 4.
- *Limitations / critiques.*
  - Evaluation uses "the best-scoring compatible node Harness under the Oracle setting", plus skills from five preparation trials per benchmark–harness pair ✓ §4, App. D. This inflates "Unseen".
  - No repeated seeds; benchmark environments are partly rebuilt by agent web search.

**A Cross-Scenario Survey and Analysis of Self-Evolving Multimodal Agents** ([2611.14256](https://arxiv.org/abs/2611.14256), v1 10 Nov 2026, Jingjing Jiang ✓ header; also on the ICLR27 list)
- *What it does.*
  - Defines a self-evolving multimodal agent by four criteria plus a differentia of self-reference ✓ §1–2.
  - Places four scenarios (multimodal reasoning, generation, GUI/Web, embodied manipulation) on ceilings of rollout count × feedback fidelity.
  - Judges each mechanism portable / conditional / blocked / redundant across scenarios ✓ abstract.
- *Key results.*
  - Five cross-cycle degradation modes: compounding reward hacking, distribution shift, forgetting/plasticity loss, curriculum collapse, model collapse ✓ §3.6, Table 6.
  - "No method measures retention on earlier-round goals" is stated for multimodal reasoning (§4.4) and GUI/Web (§6.4) ✓.
  - For generation, the survey states "Only RSI in UMMs measures retention on earlier-round goals" (72%→60%) ✓ §5.4.
  - In reasoning, a guard on retention exists only where gated training-free writes keep a fixed reference ✓ §4.3–4.4.
  - Pseudo-label accuracy falls: VisPlay 72.0→61.0 over three iterations, Video-Zero 64.5→37.5 over five ✓ §4.2.
- *Limitations.* The verdicts are inferred from evidence within single scenarios and are "still untested". The survey proposes measurements that could overturn them ✓ abstract.

**AdaRSI: On-Demand RSI for Embodied Agents** ([2610.20474](https://arxiv.org/abs/2610.20474), v2 17 Nov 2026, NEU/AgiBot ✓ header)
- *What it does.* A coding agent first fixes harness tools and procedures. Only sub-steps that remain unreliable get demos the agent collects itself. These are validated by temporary fine-tuning probes and pooled into a specialist VLA that works alongside the original executor ✓ abstract.
- *Key results.*
  - RoboCasa365 unseen composite tasks: 4.6→13.3, against 9.9 for the harness alone ✓ abstract/intro.
  - Perturbed LIBERO-Pro 2.0 tasks: 32.9→49.1, against 39.1 for the harness alone ✓ intro.
  - Beats adapting every manipulation sub-step (13.3 vs 11.1) while using 34% of the GPU time and 36% of the demos ✓ intro.
- *Limitations.* Read only as far as the intro. Absolute success on composite tasks is low.

**Flex-RSI** ([2611.08946](https://arxiv.org/abs/2611.08946), v1 6 Nov 2026, TAMU + 12 institutions †affiliations)
- *What it does.* 24 vision/robotics tasks × 18 models. The executor edits the solution behind a 2σ held-out gate. The improver edits the harness, including itself, under a 400-line cap. A frozen-improver arm is included ✓FV.
- *Key results.*
  - The improver adds 0.0 in the single frontier pair, which is at ceiling (a replicate scored 40).
  - For open models it adds +2 to +40, and only 3 of 6 deltas fall in +20 to +40.
  - Every pair is n=1, and one episode is worth 20 points ✓ (FULLTEXT_VERIFICATION).
- *Relevance.* The only multimodal or robotics benchmark with a frozen-improver control that we found (corpus claim †).

**Remaining papers**

| id | title | one-liner | mark |
|---|---|---|---|
| 2511.16043 | Agent0 (COLM 2026) | Curriculum and executor co-evolve with a code tool from zero data; +18% math / +24% general on Qwen3-8B-Base | ✓ (abstract, §1) |
| O2jE6PfSFS | AgentForge (ICLR27 sub) | Asymmetric Proposer/Solver self-play writes a versioned tool library; 4-stage gate (syntax, execution, novelty, abstention) | ◐ |
| FH5OoGxsBx | RarePlay (ICLR27 sub) | Exploration guided by rarity over the policy's own tool-state graph; inverse task synthesis | ◐ |
| 2610.15084 | Marvo: "Self-Evolving Visual Agent via Margin Reward" (title checked) | The challenger is rewarded by the solver's score with tools minus without tools; up to +7.1% | ◐ |
| 2610.09856 | Self-Evolve With a Reference | Anchored curriculum for Agent0-style tool agents | ◐ |
| 2603.18000 | AgentFactory | Accumulates executable Python subagents, refined by execution feedback | ◐ |
| 2604.01687 | CoEvoSkills | Multi-file skill packages plus a co-evolving surrogate verifier (no test access) | ◐ |
| 2606.01314 | SkillSmith | Skills compete for context and tools make room; up to +10.2 / +9.8 over EvoSkill | ◐ |
| 2607.07321 | EvoSOP | SOPs as higher-order tools; lifecycle of construct, merge, evaluate, prune | ◐ |
| 2607.08010 | Tool-Making and Self-Evolving LLM Agents in Low-Latency Systems (title checked) | Production: −42% p50 latency and up to −53% error on 1,500 alarms; versioned tools expose data drift | ◐ |
| 2610.39508 | SkillCompiler | Compiles well-specified skill steps into validated reusable programs; up to +32.33% | ◐ |
| 2609.34151 | LOTS | Prunes the persistent tool space by answer-likelihood ablation; transfers across models | ◐ |
| 2610.18616 | Knowledge Graphs as Files (DKGI) | The agent writes graph-access tools over raw KG files; at least 6.7 Hit@1 above the best baseline (within one episode, not persistent) | ◐ |
| 2607.28692 | SciToolAgent-Evo | Ontology-aware acquisition of scientific tools with a LinUCB gate; OpenSciToolBench (900 tasks) | ◐ |
| 2611.03238 | ScienceCUA | Manuals turned into skills; self-evolution raises the gain to up to +17.12 pp (vs +7.23 for the initial skills) | ◐ |
| 2611.15481 | SkillEarth | Remote-sensing skills refined by executing real tool chains; reused across models | ◐ |
| 2611.02516 | EVOLVE (guardrail tools) | Inherited, audited guardrail tools; +22% average F1 | ◐ |
| 2609.35318 | DexAgent: "An Agentic Human2Sim2Robot Framework for Dexterous Manipulation with Self-Evolving Tool Library" (title checked) | Self-evolving tool library (skills + verifiers) for Human2Sim2Robot; 3.5× the success of baselines | ◐ |
| 2610.18867 | EmbodiedEvo | Coding agent on RoboDojo-Sim with 37.82% success; experience from a stronger model helps weaker executors (+8.86 / +5.50) | ◐ |
| 1v85Pec79o | EvoRAM (ICLR27 sub) | A VLA loop plus a code loop; executable memory verified physically | ◐ |
| rxyGomjMBA / 2611.20655 | MetaAct (ICLR27 sub; arXiv) | Evolves typed executable action constraints. Violations drop 70%→0% on ScienceWorld and 40%→1% on WebShop, with score gains of +10.1 / +8.0 | ✓ (abstract, §1) |
| 2610.29560 | SERA | Self-evolving harness over a fixed VLA; RoboMME 44.38→51.38 | ◐ |
| 2610.08432 | EMHO | Embodied harness optimisation from traces; EMHO-Merge for multi-subtask settings | ◐ |
| 2611.18176 | ESI | LLM-VLA flywheel; 49.41% RoboDojo success over 10 tasks | ◐ |
| 2610.33487 | ReSight | Self-improving memory tools for robots; FindingDory 38.3→63.8 | ◐ |
| 2610.13021 | EmbodiedCLBench | Continual-learning benchmark for self-evolving embodied agents | ◐ |
| 3ijW6lQHLT | VQS: Program-Verified Self-Evolution for VLMs (ICLR27 sub) | Parses the image into a scene graph or table and lets a fixed program write the QA; 24% of majority-vote labels are wrong, 94% are correct with VQS | ◐ |
| cizDyhcm7v | Tempo-Zero (ICLR27 sub) | Programs render videos with ground-truth intervals; one policy acts as both Questioner and Solver | ◐ |
| x8Lg6heIlT | RISE (ICLR27 sub) | Fixes coarse role alternation and collapse of question types in VLM self-evolution | ◐ |
| 2610.04469 | Trinity (VLM self-verifier) | A self-verifier adjudicates between reference and consensus; +12.8 MathVerse on Qwen3-VL-8B | ◐ |
| 2609.38721 | UniEvo-VL | On-policy self-distillation for unified models; GenEval 0.747→0.808; gains are uneven | ◐ |
| sbgflATGFK | VisEvolver (ICLR27 sub) | Experience migrates from memory into weights according to its reliability | ◐ |
| 2609.34682 | V-Gym | Skills and practice data co-evolve for visual reasoning | ◐ |
| 2610.20786 | EvolvingBrain | Skill system at several granularities for VAB-Minecraft and DiscoveryWorld | ◐ |
| z2x2QzPjvB | SEVA (ICLR27 sub) | VLM agent with evolving memory for video object segmentation | ◐ |
| 2610.15244 | EvolvAttack: "Backdooring self Evolving VLM Agents" (title checked) | A backdoor stays dormant until memory evolves: 0.5% attack success before, >95% within 100 interactions | ◐ |
| 2611.02102 | NorthCUA | Family of computer-use agents trained on experience at scale (RL, not self-referential) | ◐ |
| 2305.16291 / LATM / CREATOR / CRAFT / Alita / ToolMaker 2502.11705 / STELLA 2507.02004 | Foundations | Precedents for tool and skill making, cited by the seed papers ✓ (ChartEvo §2, VideoResearcher §2.2) | † |

### Consensus, debates & contradictions
- **Tools beat instructions in domains bound by perception.**
  - ChartEvo: tools 84.4 vs skills 55.6 ✓ Table 4.
  - VideoResearcher: SkillOpt −2.3; the Meta-Harness and AutoResearch runs "mainly revise prompts, yielding limited gains" ✓ §4.2.
  - Agent S4 points the same way through interfaces.
  - This *contradicts* text-domain findings that the effective edit surface collapses to instruction text (IbI, round 3). The likely reason is that a perception bottleneck needs new observations, not new advice. The best edit surface depends on the domain.
- **"Self"-improvement here is mostly improvement by a stronger model.**
  - ChartEvo (GPT-5.6-Sol proposer), VideoResearcher (Codex evolvers), ReTerm (DeepSeek-V4-Pro teacher) and EmbodiedEvo (experience from a stronger model) are all at least partly distillation.
  - True same-model loops show much smaller deltas. Agent S4, with Qwen3.8-27B as its own meta-agent, gains +1.86 pp over the seed ✓. SIMIT reaches about +7% relative only with heavy compute (25–55×) ✓.
  - Flex-RSI fits a "teacher gap" reading: 0.0 in its one frontier pair, which sits at ceiling, against +2 to +40 for open models, all n=1 ✓FV.
- **Libraries are not monotone.**
  - Gated systems never report regressions, because by construction they keep only gains (VideoResearcher, ChartEvo).
  - SEAR inherits without a gate and finds a model's own library *hurting* it (46.9→42.4) while other models' libraries help ✓.
  - VideoResearcher's later round scores below its earlier round on 2 of 3 test sets ✓. Gating hides drift; it does not prevent overfitting to the dev set.
- **Role separation is unresolved.** Tool-R0 finds that sharing Generator/Solver weights costs −17.42 ✓. Tempo-Zero and Agent0-VL-style designs use one policy for both roles ◐. The answer may depend on how high-entropy the action space is.
- **Verifier provenance decides everything.**
  - Consensus or self-judged labels degrade: VisPlay 72→61 ✓ via survey; TTRL makes "unanswerable" the majority vote in 99.2% of VizWiz steps ✓ SIMIT.
  - Answers grounded in a program or a render (SIMIT, VQS, Tempo-Zero) correct more errors.
  - Agent-built verifiers can be catastrophic: SEAR's detector was wrong on 91% of the rollouts where it declared success ✓.
- **Evidence quality is weak.**
  - Single runs and tiny test sets dominate. ChartEvo has about 30–50 Q per type; ReTerm samples once with the task seed fixed at 0; VideoResearcher reports no repetitions.
  - Only MetaForge (3 runs) and SEAR (3 runs in simulation) report repetition among the papers here.
  - SEAR shows that run-to-run spreads of 20.9 pts exist ✓. Most claimed deltas in this corner are within plausible run noise.

### Open problems
1. **Retention of created tools.** Almost no paper re-tests earlier-round goals after later tools are added. The survey finds none in reasoning or GUI/Web, and one in generation, RSI in UMMs (2611.14256 §4.4, §5.4, §6.4 ✓). We need a survival curve per tool and per round on frozen earlier tasks.
2. **Matched-budget controls outside robotics.** SEAR's design (best-of-3, no-library, sealed judge) ✓ has not been ported to tool creation for video, charts, GUI or terminals. Every headline there lacks a "resample K times" arm.
3. **Teacher vs self.** No paper crosses proposer strength with agent strength, for example a ChartEvo grid of proposer ∈ {self, stronger} × agent. Without that grid, "self-improvement" and "distillation" cannot be told apart.
4. **Noise in dev-set gates.** A strict-increase gate on 300 Q (VideoResearcher) or 15 validation tasks (Agent S4) admits noise. Tool-creation loops need sequential or statistical gates, which already exist for text harnesses.
5. **Who writes the verifier.** When the agent builds its own success detector or semantic judge (SEAR, MetaForge's judge, CoEvoSkills' surrogate), how often is it wrong, and does inheritance compound the error? SEAR gives one anecdote, not a rate.
6. **The improver is frozen everywhere.** The tool-*making* procedure is never edited: not VideoResearcher's Evolving loop, ChartEvo's proposer, S4's meta-agent (fixed model and harness ✓ §3) or Trinity's Evolver. Pattern-(iii) updates are absent from this whole corner.
7. **Tool drift and deprecation.** Created tools break when upstream APIs, apps or perception models change. Only the industrial report (2607.08010 ◐) and Δτ-Bench † touch this. Nobody studies how to maintain a tool library under real version changes.
8. **Portability is untested.** The survey's portable/conditional/blocked/redundant verdicts ✓ are predictions. For example, program-verified QA should port to GUI only where outcomes can be checked. One matched-budget study of cross-scenario transfer would confirm or falsify several verdicts at once.
9. **Security of persistent tools and memories.** EvolvAttack (dormant until memory evolves ◐) and inherited wrong verifiers show that audits of a clean initial state miss threats that grow later.

### Where it is heading (to mid-2027)
- **The measurement turn reaches multimodal and embodied work.** Expect SEAR-style controls (best-of-k, no-library, sealed judge, 3+ runs) to become a reviewer expectation for tool-creation papers, as frozen-improver arms did for text harnesses (Flex-RSI, SelfImprove-Bench †). Expect several audits finding that "matched-budget tool evolution adds ≤2 pp".
- **Program-grounded self-supervision replaces consensus** in VLM self-play: VQS ◐, Tempo-Zero ◐, SIMIT ✓. Expect render and program verifiers to become standard, and majority-vote baselines to be treated as straw men.
- **RL with forging.** RL-trained tool forging (MetaForge, AgentForge) will likely merge with zero-data curricula (Tool-R0). The weights learn *when* to forge, and a gated library stores *what* was forged.
- **Interfaces become the unit of evolution** for computer use (Agent S4), along with cheaper meta-agents that use the same model. Gains will be reported against strong human seeds, not weak harnesses.
- **Embodied systems split into two passes**: harness work first, then targeted weight adaptation (AdaRSI, ESI, EvoRAM). Real-robot evaluation will stay at about one run per cell because of cost.
- **Not expected by mid-2027** (my estimate): a tool-creation system that edits its own tool-making procedure with a measured benefit of order 1.

### Connection to our ideas
- **#1 Taught, Not Selected.** SEAR's self-built verifier is a clean field example of the *reading/writing* channel corrupting a lineage. A wrong "fact" was written to memory and inherited, not selected by the score ✓.
  - Tool creation adds a third channel next to traces and scores: *the agent's own instruments*.
  - Use SEAR §3.3 as motivation. A cheap extension: seed a buggy perception tool and measure whether evolved tools inherit the bug.
- **#2 / #3 Forced-Quota / Gate, Not Willingness (merged).** This corner gives strong outside support for the scarcity of self-edits.
  - Every system here holds its tool-*maker* fixed: VideoResearcher's Evolving agents, ChartEvo's proposer, S4's meta-agent, Trinity's Evolver.
  - VideoResearcher's strict-increase gate is a ready-made "proposed vs propagated vs paid off" funnel in a non-text domain. Its Table 3 shows 2 of 5 representative proposals accepted, and the final toolbox has 2 evolved tools ✓. The full count of rounds and rejections is not reported, so the funnel needs those numbers from the authors or a rerun.
- **#6 Meta-Overfitting.** Several findings are direct evidence and a possible testbed:
  - SEAR's cross-model library result: on GPT-5.6-sol, its own library −4.5 and Opus's library +13.7 ✓.
  - VideoResearcher's degradation from selection on the dev set ✓.
  - ChartEvo's evolution per benchmark ✓.
  - SEAR's "control code and shared routines transfer, while task-specific solutions do not" ✓ is the procedure-vs-memory split the idea predicts.
- **#7 Varying Goals.** Tool libraries are the natural object for DriftStream: versioned tools under real library changes. The industrial report's "versioned tools expose upstream data drift" ◐ is anecdotal support. No paper here varies the switching rate.
- **#8 Openness Ladder.** This chapter bears on #8 most strongly.
  - Domains bound by perception reward opening the *tool* surface. ChartEvo: tools ≫ skills ✓.
  - In VideoResearcher, tool-level evolution (74.5) beats harness-level Meta-Harness (73.5), whole-system AutoResearch (71.7) and prompt-level SkillOpt (69.8) ✓ Table 1. Whole-system search beats prompt-only here, so the ordering is not "prompt > whole-system". The paper says whole-system search "may largely waste effort on unrelated components" ✓ §1.
  - Text domains favour instruction-only surfaces (IbI).
  - Together these fit "the optimum is in the middle and *moves*". A two-domain openness sweep (text vs chart/video) with matched budgets would be novel and cheap to build on ChartEvo's released setting. The risk of being scooped is moderate: EvoSpace (MQoRwlss2y) states the dilemma for text.
- **#9 MetaShift.** No paper here changes the *rules of improving* mid-run (e.g., swapping the acceptance gate or the perception model). VideoResearcher assumes fixed perception ✓, which makes a mid-run swap of the perception model a natural MetaShift probe.
- **#4 / #5 (Order-2 audit, routers).** Weak links. The Trinity's capability router and oracle node selection ✓ are an example of an order-0 reward producing a *router*. Note that its self-routing gives 71.27/60.96 against Oracle's 73.16/63.61 ✓ Table 4. This supports #5's framing if cited carefully.

#### Fact-check log
| Claim | Verdict | Source location |
|---|---|---|
| SEAR verifier wrong on 91% of 699 rollouts; 637 ended early; 4.6% vs 15.5/23.6% | confirmed | 2610.18792 §3.3, l.599–603 |
| SEAR GPT-5.6-sol RoboLab 46.9 / 42.4 own / 60.6 Opus / 55.7 Kimi | confirmed | 2610.18792 §3.2, l.566–567 |
| SEAR gain vs base: +12.0 (30–80%), +3.0 (<30%), quadratic peak 43% (+14.7) | confirmed | 2610.18792 Fig. (l.506–546) |
| SEAR real robot: Fable 5.1 70.2 / GPT-6-astra 41.4 / Opus 35.6; AaP 88.0; one practice run per entry | confirmed | 2610.18792 §4.4, Table 4 |
| SEAR is "the first controlled benchmark" | corrected (SEAR makes no "first" claim; now marked as our corpus judgment †) | 2610.18792 contributions, l.211–214 |
| VideoResearcher 72.1→74.5; SkillOpt 69.8, Meta-Harness 73.5, AutoResearch 71.7 | confirmed | 2609.19664 Table 1, l.367–373 |
| VideoResearcher: 2 tools accepted (68.7→75.0→76.7); rounds 3/14/17 rejected | confirmed (Table 3 lists representative rounds only) | 2609.19664 Table 3, §4.4 l.608 |
| VideoResearcher gate "rejected 18 of 20 rounds" | corrected (not reported; only "up to 20 rounds") | 2609.19664 §4.1 l.453 |
| VideoResearcher "+6.3 dev → +2 to +4 test" | corrected (total dev +8.0; test +1.0 to +4.1) | 2609.19664 Table 1, Table 3 |
| VideoResearcher ordering "tool > prompt > whole-system" | corrected (AutoResearch 71.7 > SkillOpt 69.8) | 2609.19664 Table 1 |
| Round 7 below Round 2 on LVBench and LongVideoBench | confirmed | 2609.19664 Table 2b, l.425–426 |
| ChartEvo 48.0→93.6; ChartX 53.6→86.1; ablation 42.8/55.6/57.2/84.4/94.8; SELT 6.19→2.73 | confirmed | 2610.28282 Tables 1, 2, 4, 6 |
| ChartEvo proposer GPT-5.6-Sol; 450/265 test Q; 5% tolerance; budget 15 | confirmed | 2610.28282 §4, l.436–464 |
| MetaForge 73.80→88.14 / 74.21→88.98; Sonnet 4.6 86.88; 3 runs; 190/123 and 12/11 forging | confirmed | 2606.01801 Table 1, Table 2, l.447 |
| MetaForge GPT-5.4 at 36.40 on CLEVR-M | confirmed | 2606.01801 Table 1 header + l.408 |
| Tool-R0 24.85→47.84 (+92.52%); ablations 30.42/41.65/43.54; API-Bank 19.13 vs 10.13 | confirmed | 2602.21320 Tables 1–3 |
| Tool-R0 "83 votes" | unverifiable (marked †) | — |
| SIMIT +6.98% (25×), +7.20% (55×), TTRV +2.07% (41×); 14.4%/2.0%; 12% vs 33–38% | confirmed | 2611.00053 Table 1, §4.4 |
| SIMIT "TTRL reinforced unanswerable in 99.2% of steps" | corrected wording (majority vote in 99.2% of TTRL training steps; 79.3% of nonzero-advantage steps reinforce it) | 2611.00053 §4.1 l.566–569 |
| SIMIT library quote "not optimized against benchmark scores" | corrected to exact wording "We did not optimize them against benchmark scores"; 17 renderers confirmed | 2611.00053 App. l.1814, l.1820 |
| Agent S4: Qwen3.8-27B for both agents; 15.74/19.44/21.30; Opus 4.8 20.6 at $72.40 vs $1.10; 3.8× vs Qwen CUA 5.56 | confirmed | 2610.28601 l.394, Tables 2, 4 |
| Agent S4 seed "about 550 lines" | corrected (removed; no line count in text, Table 5 lists files) | 2610.28601 App. A.2 Table 5 |
| Agent S4 scratch-VM variant "worse binary" | corrected (binary 19.44 = seed, partial 51.06) | 2610.28601 Table 2/4 |
| Agent S4 ScienceBoard 55.03→58.58 (93→99/169); validation tasks inside full set | confirmed | 2610.28601 Table 3, l.390, l.577 |
| ReTerm TB2.0 20.2→34.8; ablation 25.8/28.0/34.8; harness +6.7 to +12.4; AIME25 73.3→93.3; ≤32% recovery | confirmed | 2610.18484 Tables 1–4, §4.5 |
| ReTerm "seed 0" | confirmed with nuance (task seed fixed at 0, temperature 1.0) | 2610.18484 §4.1 l.417 |
| Trinity Train 39.1→73.2, Unseen 42.1→64.1; Codex 47.5 / DeepSeek 46.2; Eleusis 82.2→81.1; Oracle-setting evaluation | confirmed; added self-routing 71.27/60.96 vs Oracle 73.16/63.61 | 2610.36077 §4.3 l.834–839, §4.4 Table 4, App. D l.1707 |
| Survey: "No method measures retention on earlier-round goals" (used as a corner-wide claim) | corrected (stated for reasoning §4.4 and GUI §6.4; §5.4 names RSI in UMMs as the one exception, 72%→60%) | 2611.14256 l.1315, l.1552, l.1790 |
| Survey VisPlay 72.0→61.0, Video-Zero 64.5→37.5; SEAR cited for erroneous writes (§3.6); five degradation modes | confirmed | 2611.14256 §4.2 l.1254, §3.6 l.1015, Table 6 |
| AdaRSI 4.6→13.3 (vs 9.9), 32.9→49.1 (vs 39.1), 34% GPU / 36% demos | confirmed (vs 11.1 for adapting every sub-step) | 2610.20474 abstract, intro l.93–108 |
| Agent0 "+18% on Qwen3-8B-Base reasoning" | corrected (+18% math, +24% general); upgraded to ✓ | 2511.16043 abstract l.38–39 |
| MetaAct ScienceWorld violations 70%→0% | confirmed (also WebShop 40%→1%, +10.1/+8.0); arXiv 2611.20655 | 2611.20655 abstract l.40 |
| Flex-RSI "0.0 for frontier, +2 to +40 for open" | confirmed with nuance (one frontier pair, at ceiling; n=1) | FULLTEXT_VERIFICATION.md §Flex-RSI |
| ID check 2610.15084 = Marvo | confirmed ("Self-Evolving Visual Agent via Margin Reward", 10 Oct 2026; up to +7.1%) | arXiv API |
| ID check 2610.15244 = EvolvAttack | confirmed ("EvolvAttack: Backdooring self Evolving VLM Agents") | arXiv API |
| ID check 2609.35318 = DexAgent | confirmed ("DexAgent: An Agentic Human2Sim2Robot Framework for Dexterous Manipulation with Self-Evolving Tool Library") | arXiv API |
| ID check 2607.08010 = Tool-making low-latency | confirmed ("Tool-Making and Self-Evolving LLM Agents in Low-Latency Systems") | arXiv API |
| Dates: Tool-R0 24 Feb, MetaForge 1 Jun, ChartEvo 20 Oct, Agent S4 20 Oct, SEAR 13 Oct, ReTerm v2 14 Oct, Trinity v2 30 Oct, survey 10 Nov, Flex-RSI 6 Nov, AdaRSI v2 17 Nov, SIMIT 11 Oct | confirmed | arXiv stamps in each paper.txt header |

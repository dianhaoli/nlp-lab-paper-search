## Benchmarks for self-improvement, RSI and continual agents

*Scope: benchmarks, gyms and evaluation protocols that score (a) agents improving their own harness, memory or skills on a frozen model, (b) agents doing the AI R&D steps that would feed recursion (data, training, compression, infrastructure), (c) systems whose updated version runs the next round (true recursion), and (d) continual and lifelong streams with drift. Coverage is about 60 papers, Jun–18 Nov 2026, plus ICLR 2027 submissions. Read in full this session (✓): RSI-Env, SelfImprove-Bench, Continual AutoResearch, HarnessOpt-Bench, Flex-RSI, Are Self-Evolving Agents Continual Learners?, SEAGym (method and results), Evo-Bench (method and results), Singularity-Bench (§1–5), and RSIBench-Data (abstract, results table, limitations). Partially read (✓ only for the sections cited): GDPevo, EvoHarnessBench, EvoAgentBench, CompressionBench, RSIGym, Auditing-the-Product, AI4AI-Bench (abstract and §1 in full text), CL-Bench, CLE (abstract and main results table). The survey framings come from the four survey digests: Duan 2609.11873, Chen 2607.07663, Ren 2607.13104 and Dai/Ruan "Experience→Structure". I re-read their evaluation sections (✓ Duan §3.5–3.6 and Table 8; Ren §8; Chen §9 item 6). Text was downloaded to `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/<id>/paper.txt`. A second adversarial fact-check pass re-opened these full texts; see the log at the end.*

### At a glance
- **What this corner is.** Since June 2026 at least 40 benchmarks or gyms claim to measure "self-improvement", "RSI" or "continual learning" for LLM agents. They fall into four families: harness/experience-evolution benchmarks (frozen model), AI-R&D benchmarks (an agent improves a model, data, a compression scheme or a system), open-ended recursive environments (the updated system runs the next round), and continual/drift streams.
- **State of the art (numbers):**
  - Evo-Bench: best evolver +16.6 overall, still below the human harness (46.3 vs 47.5) ✓.
  - HarnessOpt-Bench: best optimizer captures about ⅔ of OfficeQA headroom ✓.
  - RSIGym: Opus 5 RSI-Index 0.4809, on dev-seen tasks ✓.
  - Singularity-Bench: Opus 5.5 + Claude Code Singularity-Index 1.25 vs human baselines; the other 10 agents average 0.60 ✓.
  - AI4AI-Bench: best 0.250, where 0.1 is the shipped algorithm and 1.0 the optimum ✓.
  - CLE: best system (Claude Code / Opus 5) clears 47.7% of levels ✓.
- **Consensus 1: endpoint gain overstates improvement.**
  - Visible validation scores are optimistic (HarnessOpt-Bench) ✓.
  - Among searches that continue past their best score, 78.26% of RSIBench-Data searches end lower ✓.
  - SEAGym's AHE run collapses to 6/80 replay mid-run and then recovers ✓.
  - Trace Evolution Bench shows a frozen `no_evolve` rescore moving 5–12.5 pp across seeds ◐.
- **Consensus 2: gains are local and do not reliably transfer.**
  - SelfImprove-Bench: replay (G0) falls below the historical score in 8 of 18 method–task pairs, and rankings reverse at G2 ✓.
  - SEAGym: cross-backend out-of-distribution (OOD) gains are mostly neutral or negative ✓.
  - Continual learners: plasticity–stability r = −0.72 (n = 12) ✓.
  - EvoHarnessBench: the harness growing alone causes forgetting of up to −34.7% backward transfer ✓.
- **Consensus 3: persistence buys speed, not final quality** (Continual AutoResearch ✓; CL-Bench, where naive in-context learning beats dedicated memory systems ◐).
- **Consensus 4: the base model matters more than the mechanism.**
  - HarnessOpt-Bench: swapping the optimizer model moves gain about 1.8× more than swapping the coding harness ✓.
  - GDPevo: "magnitude of evolution comes mainly from the model's intelligence" ✓.
- **Biggest open problem.** Nearly every benchmark scores the *product* (the final agent or model). None scores the *process*, that is, whether a later improver beats an earlier one from the same start at matched budget (the "swap test"). Auditing-the-Product finds only 4 of 26 improver claims have such a comparison ✓. The new benchmarks also run almost everything once, with no seeds.

### How it developed

| Month (2026) | Paper | What changed |
|---|---|---|
| Jun | CL-Bench 2606.05661; AgentCL 2606.02461 | Expert-validated continual-learning streams with a "gain over stateless" metric. Naive ICL is a strong baseline ◐ |
| Jun | SEAGym 2606.17546 | First harness-evolution *environment*: train / update-validation / ID / OOD / replay / cost views, wrapping Harbor tasks ✓ |
| Jul | EvoAgentBench 2607.05202 | Train/test linked by an Ability Graph. Curated Ability content transfers; no automatic method is positive everywhere ✓ |
| Jul | RSIBench-Data 2607.25886 | Agent as data-centric researcher on a fixed Tinker stack. Names the "discovery–reliability gap" ✓ |
| Aug | GDPevo 2608.03764; PAST-Bench 2608.04003 | Rule hybridization makes train→test gains attributable. Persistence on/off ablation plus mechanism-evidence score ✓/◐ |
| Aug | HarnessOpt-Bench 2608.06301; Evo-Bench 2608.09096 | Harness optimization becomes a *scored capability*: trusted evaluator, hidden test split, resolution bands, release ladders ✓ |
| Aug | AI4AI-Bench 2608.20318; PACE-Bench 2608.14441; S3Gym 2608.31100 | Training-algorithm design as RSI. Re-adaptation after physics shift. Self-judging accuracy does not predict gain ✓/◐ |
| Sep | EvoHarnessBench 2609.04280; EvoPathBench 2609.24663; SEABench 2609.35596 | Drift moves *into the harness*. Process-level checkpoints. Endogenous misalignment ✓/◐ |
| Oct | RSIGym 2610.10310; RSI-Forge 2610.09426; Agent Plasticity 2610.08902; TEB 2610.15055 | Everything-as-a-service gyms; paper-derived environments with shortcut codebooks; held-out gain per $; seed-null bands ✓/◐ |
| Oct | Continual learners 2610.25858; Auditing-the-Product 2610.27610; VibeTraining-Bench 2610.26342; RSI Arena 2610.33624; CLE 2610.33956 | Plasticity–stability decomposition. A meta-audit of 114 studies. Multi-component data/trainer/harness edits. Human-feedback post-training ✓/◐ |
| Nov 1–6 | RSI-Env 2611.01736; SelfImprove-Bench 2611.02797; Continual AutoResearch 2611.05108; CompressionBench 2611.06858; Flex-RSI 2611.08946 | First "Self vs fixed Teacher" recursion. G0–G4 ladder. Persistent vs reset under matched budgets. Paired frozen vs self-improving harness ✓ |
| Nov 9–17 | Infrathon 2611.11995; Continual-ARC 2611.14505; Singularity-Bench 2611.16591; RSI-Prime 2611.23138; SkillBuffBench 2611.22017 | Full-stack "can agents build agents", with red-teamed graders (Breakwater). 24-h horizons. Lifelong ARC streams. A training set to *improve* model-training ability ✓/◐ |

### Taxonomy

The survey framings give three cuts:

- **Duan et al. 2609.11873** classify RSI by autonomy level, L1–L5 (execution → strategy → experience → deployment → recursive). Their Table 8 lists what an L5 evaluation must measure: adaptivity, retention, transfer, efficiency, stability, plus "meta-recursion" (mechanism reuse, successor quality, goal and stopping decisions). They add that these are "proposed requirements… rather than capabilities established by existing RSI benchmarks" ✓.
- **Ren et al. 2607.13104** split by update channel: foundation-model weights θ vs scaffold Σ. They also split evaluation interface (single-shot vs interactive) and separate *mechanism* benchmarks from *domain* benchmarks (§8.2) ✓.
- **Chen et al. 2607.07663** flag that "recognizing frame revision… has neither a benchmark nor a measurement convention" (§9 item 6) ✓.
- **Dai/Ruan ("Experience→Structure")** propose controlled, longitudinal tests of whether "inherited state help[s] produce more effective future updates under comparable… budgets" ✓.

The benchmarks themselves map onto these cuts as follows.

| Family (what is improved / what is measured) | Duan level | Representative benchmarks |
|---|---|---|
| **A. Harness/experience evolution on a frozen model: endpoint and held-out gain** | L2–L3 | Evo-Bench, HarnessOpt-Bench, HarnessDev, GDPevo, EvoAgentBench, EvoCowork, FinEvo-Bench, SkillFlow, Do-Skills-Generalize 2609.39148 |
| **B. Generalization ladders and process/trajectory views** | L3 (Table 8 rows 1–5) | SelfImprove-Bench (G0–G4), SEAGym (replay, ID/OOD, cost), EvoPathBench, Trace Evolution Bench (seed-null), PAST-Bench (mechanism evidence), Beneath the Surface 2611.07624 |
| **C. Continual / drift streams (plasticity, stability, forgetting)** | L3–L4 | Continual learners 2610.25858, EvoHarnessBench, CL-Bench, CLE, AgentCL, Continual-ARC, ServeLearnBench, ReScopeBench, PACE-Bench, Δτ-Bench, SkillShift-Bench, Coding-Agents-Continual 2611.03219, EmbodiedCLBench |
| **D. AI-R&D single-shot (agent improves a model or system; no inheritance)** | L1–L2 | Singularity-Bench, AI4AI-Bench, RSIBench-Data, RSIBench-Code, CompressionBench, Infrathon, Auto-Research Arena, EvalResearchBench, PostTrainBench-family, RSI-Index (Vals) |
| **E. Multi-session / multi-round AI-R&D with inherited state** | L2–L3 | RSIGym, VibeTraining-Bench, ParetoRSI-Gym, RSI-Forge (3 sessions), RSI Arena (2 stages), Continual AutoResearch |
| **F. Recursion proper: the updated system or improver runs the next round** | L5 (partial) | RSI-Env (Self vs Teacher), Flex-RSI (improver edits itself; frozen vs self pairs; hand-offs), SelfImprove-Bench HyperDSH fixed-improver ablation |
| **G. Meta-evaluation and verification of claims** | L5 "meta-recursion" | Auditing-the-Product 2610.27610, "Why is RSI hard to verify?" (ICLR27 OnzWTehIaN), RSI Tax 2611.13524, HDA 2610.22466 |
| **H. Safety and hacking under self-improvement** | cross-cutting | Singularity-Bench red-teaming / Breakwater, RSI-Forge shortcut codebook, SEABench, CheatBench, Masked Erosion |
| **I. Training data to improve the improver** | — | RSI-Dojo 2611.03698, RSI-Prime 2611.23138 |

### Paper cards

**Auditing the Product, Not the Recursion** ([2610.27610](https://arxiv.org/abs/2610.27610), 2026-10-20, preprint)
- *What it does:* a meta-research census of the evidence behind RSI claims, with a claim-indexed reporting standard.
- *Setup:*
  - Full-text coding of 114 studies, 90 self-described RSI, in an arXiv frame frozen October 2026 ✓ (abstract; §1).
  - Independent human coding, third-reader adjudication, and blind coding of improver comparisons.
- *Key results:*
  - Among the 90 RSI studies:
    - 60% evaluate on tasks not used for selection ✓;
    - 22% audit two or more reruns ✓.
    - In a human-coded random sample the figures are lower still: 45% and 14% ✓ (abstract).
  - Of 26 studies claiming a better improver, 23 report a related comparison but only 4 compare earlier vs later procedures from identical starts at matched budget ✓.
  - Proposition 3 ("diagonal non-identification", §4.3): improver gain is identified only with off-diagonal observations, i.e. a swap test ✓.
- *Limitations / critiques:*
  - It runs no swap test itself.
  - Its frame predates most of the November benchmarks.
  - The ICLR27 submission "Why Is RSI Hard to Verify?" (OnzWTehIaN) gives a complementary finite-horizon sufficiency criterion and statistical tests ◐.

**SelfImprove-Bench: Evaluating Generalization in Self-Improving Agents** ([2611.02797](https://arxiv.org/abs/2611.02797), 2026-11-02, preprint; SJTU/Theseus Labs, the same group as the Duan survey)
- *What it does:* procedurally generated game tasks (templates × difficulty × seed) with a five-level generalization ladder:
  - G0 replay;
  - G1 new instances;
  - G2 harder;
  - G3 another game, same capability;
  - G4 cross-capability.

  It is scored by the Headroom-Closed Index (HCI) and adds LLM-annotated trace analysis.
- *Setup:*
  - Methods: ACE (experience), AHE (harness), HyperAgents (meta).
  - Six games (Farm, Combat, Sweepmine, Hero, MC, Astray).
  - Backbones: GPT-5.6 Sol, Claude Opus 5.5, DeepSeek-v4.1-flash, with Codex or DeepSeek Harness.
  - No seeds reported.
- *Key results:*
  - G0 replay is below the historical score in 8 of 18 method–task pairs ✓ (§4, "(G0) Retention").
  - G1 is positive in 16 of 18 ✓.
  - G2 reverses rankings: on Farm, HyperAgents beats AHE at training difficulty but trails it on Hard ✓.
  - HyperAgents' G3 transfer from Combat→Farm (109.2% HCI) exceeds direct refinement (100%) ✓.
  - G4 is positive in 5 of 8 directions ✓.
  - Across 915 retained "opinions":
    - 87.9% target prompt/memory, 12.1% the execution harness, and none modify the improver ✓;
    - HyperAgents' revision rate is 40.2% vs ACE 14.2% (AHE 30.6%) ✓.
  - Fixed-improver HyperDSH scores 1.207 vs 1.326 editable, both on DeepSeek, single game ✓.
  - Task scheduling (HyperAgents, Table 4): random medium-difficulty training beats fixed-instance training on Combat (Medium 1.433 vs 0.772; Hard 1.314 vs 0.507). On Hard Combat, mixed and increasing-difficulty curricula score 1.195 and 1.080, also below random medium ✓. *(Corrected: the draft compared 1.433 against 1.080, but 1.433 is a Medium-Combat score and 1.080 a Hard-Combat score. On Medium Hero, fixed-instance training is actually higher, 0.100 vs 0.085.)*
- *Limitations / critiques:*
  - Games only and single instances.
  - The unit is LLM-extracted "opinions", not edits.
  - System and edit surface are confounded across methods (round-3 critique).
  - Improver comparisons are not same-start or matched-budget.

**Flex-RSI: Benchmarking Recursive Self-Improvement of Frontier Agents** ([2611.08946](https://arxiv.org/abs/2611.08946), 2026-11-06, technical report, TAMU + 12 institutions)
- *What it does:* each round, an executor edits the solution, which is kept only if held-out validation rises. An improver then edits the harness, *including itself*. The final solution is scored on hidden test data.
- *Setup:*
  - 24 tasks in six areas (perception, reasoning, robot action, 3D reconstruction, visual creation, infrastructure); 18 models (11 closed) ✓.
  - Every run starts from the same general harness.
  - Median 49.7M tokens per run ✓.
- *Key results:*
  - On "builds-the-solution" tasks with scaled feedback, models start 0.9 points apart and end 38.8 apart ✓.
  - On the four success/fail-only tasks (RoboCasa, LIBERO-MAX, LIBERO-PRO, ARC-AGI-3), the median open-weight gain is 1.6 vs 16.9 for closed models ✓.
  - Token use vs score has rank correlation −0.05 ✓.
  - Frozen vs self-improving harness: the self-improving run is better in 6 of 7 pairs, by +2.0 to +40.0 ✓. However, the frozen runs ended after fewer rounds in 6 of 7 pairs ✓.
  - A harness handed from a stronger model helped a weaker one in only 2 of 4 tasks (+20.0 Open Drawer, +1.4 3D Tracking) ✓.
- *Limitations / critiques:*
  - Every cell is n = 1.
  - Paired runs are not compute-matched.
  - There is no self-edit rate and no weight updates.
  - A 400-line cap applies to improver self-edits ✓ (FULLTEXT_VERIFICATION).

**RSI-Env: An Open-Ended Environment for Recursive Self-Improvement of Agents** ([2611.01736](https://arxiv.org/abs/2611.01736), 2026-11-01, ICLR27 submission PwLSweAzxz)
- *What it does:* a persistent sandbox in which the improver can train weights (Tinker SFT/RL) and edit the harness. A host controller holds evaluation and versioning. Under **Self**, each accepted model–harness version runs the next round. Under **Teacher**, a fixed Codex (GPT-5.6 Sol or GPT-6 Astra) does.
- *Setup:*
  - Start: Qwen3.8-27B with a minimal loop.
  - Three settings: Open-General, Capability-Directed (Reasoning, Agentic, Knowledge), Benchmark-Guided.
  - Budget: 10 rounds × 3 h (30 h per run) ✓.
  - Evaluation: GPQA-D (198), AIME 2026 (30), Terminal-Bench 2.1 (89), SimpleQA (500 subset), avg@3 or pass@3 ✓.
- *Key results:*
  - Open-General Self: GPQA 65.2→73.2, AIME 53.3→56.7, via a harness-only update (H1/M0) ✓.
  - Knowledge Self: 82.0→85.0 (harness) →88.0 (after training on 48 self-made SFT pairs) ✓. This is the only two-step harness→weights recursion the paper walks through and that improves its target. Reasoning Self also ended at H1/M1, but its target gain was modest (GPQA +4.0, AIME 0.0) ✓. *(Corrected: the draft called Knowledge the "only" such case without mentioning Reasoning Self's accepted model update.)*
  - Gains come with regressions: Knowledge Self's GPQA falls to 53.0 ✓.
  - Teachers are not consistently better, but they used only 1.19–6.97 h of the 30 h vs 24.9–28.95 h for Self, and never accepted a model update (all M0) ✓.
- *Limitations / critiques:*
  - Apparently one run per cell.
  - AIME has n = 30.
  - Acceptance is validity-only, so there is no gate on capability.
  - The Self-vs-Teacher contrast is confounded by budget use.
  - It is "recursion" in the Duan *structural* sense, not shown to be *effective*.

**Are Self-Evolving Agents Continual Learners?** ([2610.25858](https://arxiv.org/abs/2610.25858), 2026-10-19, Mila/GDM)
- *What it does:* four drift streams built from BIRD, BFCL and ScienceWorld:
  - topic drift;
  - workflow drift (BFCL-Miss vs BFCL-Base require opposite behaviours);
  - domain drift;
  - compound drift.

  Performance is decomposed into plasticity P and stability S = −(interference + forgetting).
- *Setup:*
  - ReasoningBank, GEPA, Meta-Harness on Gemini 3.6 Flash.
  - Blocks of 50 tasks, 8 rollouts per evaluation instance, binary feedback.
- *Key results:*
  - Overall terminal advantage: GEPA +3.7, ReasoningBank +2.5, Meta-Harness +1.4 pp ✓.
  - GEPA has the most plasticity (+6.2) and least stability (−4.3) ✓.
  - The P–S trade-off has r = −0.72 (p = 0.008, n = 12) ✓.
  - Meta-Harness looks stable only because its gate rejects half or more of candidates. Without the gate, S falls to −3.1 on topic drift and −5.5 on workflow drift ✓.
  - GEPA compresses the BIRD sections from 3,386 to 1,034 characters, erasing a +4.0 gain ✓.
  - Interleaving the stream does not fix it ✓.
- *Limitations / critiques:*
  - One model.
  - Edit surface is confounded with method.
  - Drift is semi-synthetic.

**From AutoResearch to Continual AutoResearch** ([2611.05108](https://arxiv.org/abs/2611.05108), 2026-11-03, ICLR27 OPHp9Aei9U; RBC Borealis)
- *What it does:* persistent vs reset instances of the same command-line agent on matched ordered streams (9 task families). Metrics are outcome, initialization and time-convergence gain, all on hidden test scores.
- *Setup:*
  - Agents: Sonnet 4.6, Opus 5, GPT-5.4, GPT-5.6-Sol, Kimi K3.
  - 30-min tasks, 32 transfer tasks per agent.
  - Single matched pair per configuration; the GPT-5.6-Sol repeat varied ≤ 0.75 points on outcome and initialization gains ✓.
- *Key results:*
  - Time-convergence gain is positive for all five agents (+3.0 to +21.3) ✓.
  - The stateful lead shrinks from +24.0 at 25% of budget to +1.2 at the deadline ✓.
  - Outcome gain is −0.9 to +7.6, and only Sonnet's is substantial ✓.
  - Subtracting the first-valid reward makes every gain negative ✓.
  - Matched vs shuffled insight cards show small or negative effects on outcome ✓.
  - The agents with the highest reset outcome (Opus 5, GPT-5.6-Sol) show the smallest gains on outcome and initialization. This is our reading of Table 2, not a claim the paper makes, and GPT-5.4 (lower baseline, small gains) breaks the pattern ◐. *(Corrected: the draft's "Stronger models gain less ✓" stated as a paper finding.)*
- *Limitations / critiques:*
  - Streams are only 3–5 tasks long.
  - The CLI scaffold is confounded with model.
  - Self-diagnosed: the memory ablation covers one stream only.

**SEAGym: An Evaluation Environment for Self-Evolving LLM Agents** ([2606.17546](https://arxiv.org/abs/2606.17546), 2026-06-16, Tsinghua)
- *What it does:* turns Harbor benchmarks into epoch/batch evolution sources with update-validation, ID, OOD, replay and cost views.
- *Setup:*
  - ACE, TF-GRPO, AHE on DeepSeek-V4-Flash.
  - 80 TB2.0+HLE training tasks / 35 validation / 55 test / 80 OOD.
  - 5 epochs, batch size 20.
- *Key results:*
  - AHE is the only method positive on validation (+17.1), ID (+9.1) and OOD (+6.3) ✓ (ACE is also positive on all three but small: +2.9/+3.6/+2.5).
  - TF-GRPO: validation +17.1 but OOD −2.5 ✓.
  - AHE replay: 34→43/80 overall, but 6/80 after epoch 4; 13 tasks fixed, 4 forgotten ✓.
  - Batch size is non-monotone: batch 20 gains most (+17.1 validation), batch 40 gains little (+2.9), and batches 10 and 80 lose 14–17 pp ✓ (Table 3). *(Corrected: the draft said "only batch 20 gains".)*
  - An HLE-only source collapses to 0 at the final snapshot ✓ (Table 4).
  - Cross-backend transfer is asymmetric: a GPT-5.4-evolved harness gives GPT-5.4 ID +5.5 but OOD −7.5 ✓.
- *Limitations / critiques:*
  - Single runs.
  - Harness-only.
  - Two source benchmarks.

*Note on AHE's "only method positive":* in Table 2, ACE's deltas are all positive too (+2.9 validation, +3.6 ID, +2.5 OOD). The paper's point is that only AHE gains substantially on all three. I kept the draft's wording with that caveat.

**HarnessOpt-Bench: Evaluating LLMs at Harness Optimization** ([2608.06301](https://arxiv.org/abs/2608.06301), 2026-08-06, Scale AI)
- *What it does:* an optimizer (LLM plus coding harness) edits a pinned seed harness under an evaluation budget (cases and calls) and nominates a candidate. The candidate is scored on a hidden test split as normalized gain, with per-task "resolution bands".
- *Setup:*
  - 5 optimizer models × {opencode, native harness} × 4 tasks (OfficeQA, BrowseComp-Plus, Terminal-Bench, GAIA stub).
  - 111 scored runs, each configuration run twice.
- *Key results:*
  - Model effect 0.142 vs harness effect 0.079, about 1.8× ✓.
  - Across 5 GPT releases on OfficeQA, gain rises monotonically 0.03→0.49 ✓.
  - Lever-coverage breadth vs gain has ρ +0.34 to +0.88 ✓.
  - Share of actions spent reading traces vs gain has ρ −0.31 to −0.64; detailed traces were requested only 16 times (by 7 of 111 cells) ✓.
  - Case passes bind: the median optimizer uses 82% of its case allowance (but only 4% of calls) ✓.
  - The best validation score is optimistic relative to test ✓.
  - Native harness wins only 9 of 20 pairs ✓.
- *Limitations / critiques:*
  - Self-described as "hack-resistant, not hackproof" ✓.
  - One pinned target model per task.
  - Seed complexity is not varied.
  - Breadth is an *outcome*, not a randomized treatment (round-3 critique).

**Evo-Bench: Can Language Models Improve Agent Harness?** ([2608.09096](https://arxiv.org/abs/2608.09096), 2026-08-10, RUC/BOSS Zhipin)
- *What it does:* tasks are pre-selected for harness sensitivity using 12 auxiliary evolved harnesses (K = 12) ✓, then split by stratum. An evolver improves a CodeAct seed for a fixed DeepSeek-V4-Flash policy.
- *Setup:*
  - 9 evolvers.
  - Budget: 20 iterations / 1,000 steps / 48 h.
  - Single runs ("We run all experiments one time") ✓.
- *Key results:*
  - GPT-5.6 Sol 46.3 overall (+16.6), Opus 4.8 45.8 (+16.1), vs human composite 47.5 ✓.
  - Search +32.8, Office about +0–3 ✓.
  - Early saturation; most models stop before the budget ✓.
  - GPT-5.6 Sol costs over $500 per run, while GLM-5.2 and Qwen3.7-Max reach the knee for under $40 ✓.
- *Limitations / critiques:*
  - Sensitivity-based task selection biases the benchmark toward tasks where harnesses matter, which inflates apparent evolvability.
  - Its claim that harnesses are "highly transferable" across policies ✓ contrasts with SEAGym's asymmetric transfer.

**Singularity-Bench: Can Agents Build Agents?** ([2611.16591](https://arxiv.org/abs/2611.16591), v2 2026-11-16, ICLR27 hrNam06fy0; CMU/Infini-AI)
- *What it does:* six end-to-end tasks, each against a production human baseline on identical GB300 hardware (12 h on one 4× GB300 node per task ✓):
  - pre-training (vs OLMo-2 recipe);
  - mid-training for RL (vs PRISM);
  - multi-turn RL for a SWE agent (vs Miles);
  - FP8 serving of GLM-5.3-Flash (vs SGLang);
  - a coding harness (vs Claude Code);
  - serverless cluster orchestration (vs Dynamo).

  The Singularity-Index is the mean ratio to human, with failures counted as 0.
- *Setup:*
  - 11 agents (7 models × Codex/Claude Code).
  - About 4,000 GB300-h in total, about 350 per agent ✓.
- *Key results:*
  - Opus 5.5 + CC: index 1.251 at $425 API spend; it beats the baseline on five tasks and matches Miles on post-training (0.533 vs 0.539) ✓.
  - GPT-6.1 Sol + Codex: 0.968 ✓.
  - The other 10 agents average 0.60; proprietary agents average 1.01 and open-weight agents 0.53 ✓.
  - Harness choice swings a model's index (GLM 5.3: 0.656 with CC vs 0.373 with Codex) ✓.
  - Plain Harbor graders were beaten by red-team attacks spanning 9 of 10 OWASP agentic categories. Breakwater catches every attack tested, and one real incident occurred in an informal run (DeepSeek V4.1 Flash, coding-harness task) ✓.
- *Limitations / critiques:*
  - Single-shot: nothing is inherited, so it measures L1–L2 capability, not recursion. The "may have the skills needed to reach the singularity" framing ✓ conflates the two (cf. Auditing-the-Product's product/process distinction).
  - Human baselines are fixed recipes, not tuned under the same 12 h.
  - The aggregate index mixes incommensurable ratios.

**RSIBench-Data** ([2607.25886](https://arxiv.org/abs/2607.25886), 2026-07-28, ICLR27 yeZb6GEcaO; Evolvent/NUS)
- *What it does:* agents iteratively design training data for LoRA-SFT of Qwen3.5-35B-A3B-Base on a fixed Tinker stack. Evaluation runs on Harbor/E2B.
- *Setup:*
  - 4 researcher agents (Claude Code Opus-4.8/Sonnet-5; Codex gpt-5.6-sol/terra).
  - 6 benchmarks.
  - One representative run per setting ✓.
- *Key results:*
  - The agent improves on its first valid attempt in 58.33% of settings ✓.
  - Of searches that continue past their best score, 78.26% end lower ✓.
  - No agent dominates across benchmarks ✓.
- *Limitations / critiques:* checkpoint selection and official evaluation share the same subset, so the scores are "not generalization to statistically held-out tasks" (authors) ✓.

**GDPevo** ([2608.03764](https://arxiv.org/abs/2608.03764), 2026-08-04, ICLR27 NOapw3FRkO; PrismShadow/NYU)
- *What it does:* rule hybridization. Enterprise workflows are split into atomic rules, distributed over training tasks and recombined in test tasks, so that gains are attributable. Regeneration counters contamination.
- *Setup:*
  - 120 tasks, 12 groups (5 train + 5 test each) ✓.
  - 4 agents × {base, fewshot, reflect, self} supervision.
- *Key results:*
  - Every evolved combination beats base, by +2.59 to +16.44 pp ✓.
  - Oracle ceiling is 91.6% ✓.
  - Opus 4.8 has the highest base and the largest gain ✓.
  - Fewshot overfits the source group; reflect transfers better ✓.
  - Opus 4.8's amortized cost rises from $1.37 to $1.87 per task ✓.
- *Limitations / critiques:*
  - Synthetic rule worlds.
  - Five test tasks per group.

**EvoHarnessBench** ([2609.04280](https://arxiv.org/abs/2609.04280), 2026-09-03, ICLR27 tNNCBiSaC9; Salesforce)
- *What it does:* puts non-stationarity in the supplied harness: tools, skills and agents arrive in stages. 17 streams (3–6 stages each), 802 tasks ✓.
- *Key results:*
  - Expansion alone causes forgetting: −12.1% (tools), −13.8% (skills), −46.4% (agents) in the headline figure ✓.
  - Max backward-transfer forgetting is −34.7% for agent-pool expansion ✓.
  - Best adaptation gains: +27.8% (MemToolAgent), +27.5% (GEPA), +110.2% (Meta-Harness on agents) ✓.
- *Limitations / critiques:* deterministic, constructed streams.

**AI4AI-Bench** ([2608.20318](https://arxiv.org/abs/2608.20318), 2026-08-20, ICLR27 alNtyGHCG3)
- *What it does:* 10 frozen research repositories. The agent rewrites the training algorithm (4 h on one B300), which is rerun from scratch for up to 12 h ✓.
- *Key results:*
  - Across 29 configurations of 6 systems: mean score 0.166, best 0.250, where 0.1 is the shipped algorithm and 1.0 the optimum ✓.
  - Submissions that change how the model learns score 0.226 vs 0.126 ✓.
  - More reasoning effort raises that share from 8% to 64% of submissions, and the mean score from 0.094 to 0.196 ✓.
- *Limitations / critiques:* only the abstract and §1 were read (now in full text); per-task tables are unread.

**CompressionBench** ([2611.06858](https://arxiv.org/abs/2611.06858), v2 2026-11-09, CMU)
- *What it does:* the agent builds compressed weights plus inference code under a storage limit.
- *Key results:*
  - 29 of 36 submissions pass verification ✓.
  - The best design per task averages +3.4% perplexity over BF16 and 77.33% vs 79.56% downstream accuracy ✓.
  - Complete-model feedback reverses 17 of 79 local rankings ✓.
  - One run per agent–task pair ✓.

### Remaining papers

| id | title | one-liner | mark |
|---|---|---|---|
| 2610.10310 | RSIGym | Everything-as-a-service gym. Data/Harness/Joint tracks. Opus 5 RSI-Index 0.4809 ($500/benchmark). SWE-V 17.67→50.33. "Scores are not held-out… every configuration is a single run" | ✓ |
| 2610.26342 | VibeTraining-Bench | Agents edit data, trainer and harness of Qwen3.5-9B. Average 22.7→31–38 vs Instruct 47.3. Validation leader ≠ test leader | ◐ (r2) |
| 2610.09426 | RSI-Forge (ICLR27 D54Tq2ULjl) | 210 paper-derived environments, 3 sessions. 73% of Haiku's accepted gains are below SE. 14-family shortcut codebook | ◐ (r2) |
| 2608.31100 | S3Gym (ICLR27 EvvYmsVOoz) | Self-judging accuracy does not predict gain (ρ = −0.010, 98 runs) | ◐ (r2) |
| 2608.31111 | Aspire | Vague-goal self-evolution. 1 of 12 final-only weight outcomes beats base. "Recovery masquerading as progress" | ◐ (r2) |
| 2608.04003 | PAST-Bench (ICLR27 uvr0FPNMyf) | Persistence on/off: Δ +0.13–0.24. A mechanism-evidence score separates equal-Δ agents | ◐ (r2) |
| 2610.08902 | Agent Plasticity (ICLR27 Jjh60xpFVY) | Held-out gain per $ across checkpoints. Endpoint ≠ acquisition efficiency | ◐ (r1 full) |
| 2610.15055 | Trace Evolution Bench (ICLR27 x0JEqgkNGz) | Frozen rescore moves 5–12.5 pp. Evolution must beat its own seed band | ◐ |
| 2609.24663 | EvoPathBench | Checkpointed capability tracking. Candidate artifacts gain, selected updates fall short | ◐ |
| 2610.33624 | RSI Arena | 10 agents post-train Nemotron 30B. Best closes a median 76% of the base→Instruct gap. Stage 2 uses human votes | ◐ |
| 2610.34386 | ParetoRSI-Gym | 5-domain data/SFT loop, multiple seeds. Opus-5 +9.2% relative on generalization benchmarks | ◐ |
| 2611.03698 | RSI-Dojo ("Training LLM Agents to Automate LLM Post-Training with RSI-Dojo", 2026-10-29; id resolved) | Paper→executable post-training environments to *train* agents to post-train | ◐ |
| 2611.23138 | RSI-Prime ("Training Models to Train Models", 2026-11-17; id resolved) | 350 training-run trajectories plus decision-level masking. +31.95% relative on PostTrainBench (GLM-5.3) | ◐ |
| 2610.35866 | Auto-Research Arena | 9 LM-pretraining targets. A simple sequential search beats sophisticated harnesses | ◐ |
| 2611.11995 | Infrathon | 150 runs × 24 h. Rankings change with horizon. 135 of 150 runs still improve in their second half | ◐ |
| 2610.04184 | EvalResearchBench | Agents build their own evaluators. Best orders about 75% of pairs vs a 91% ceiling | ◐ |
| 2609.rsi-index | RSI-Index (Vals AI) | Autonomous LLM R&D index for frontier agent systems (alphaXiv-only) | ◐ |
| ICLR27 qpLQ0b7YK7 | RSIBench-Code | 105 enterprise LLM-dev coding tasks in about 1.8M-line repositories | ◐ |
| ICLR27 5f7tLJjygV | AgentSWE | Coding agents create, edit or optimize agents. Large demo→delivery gap | ◐ |
| — | AIE-Bench | Meta-agent improves a pinned scaffold (described only in AgentSWE's rebuttal; no paper located) | † |
| 2606.05661 | CL-Bench | 6 expert domains. Naive ICL beats memory systems | ◐ |
| 2610.33956 | Continual Learning Exam (ICLR27 cMEMukgMeA) | 120 tasks × 5-level ladders. Best (Claude Code / Opus 5) clears 47.7% of levels, 23.3% of full ladders | ✓ (abstract, main table) |
| 2606.02461 | AgentCL (ICLR27 V8XlaeHbmi) | Dependent / conventional / independent streams | ◐ |
| 2611.14505 | Continual-ARC | Lifelong ARC stream with dormant returns. Memory and weights scored on one scale | ◐ |
| 2609.05435 | AhaBench | Guidance-following ≫ generalizing beyond it | ◐ |
| ICLR27 j2RMOsmge4 | SkillFlow | 166 tasks in 20 families. Similar vanilla models differ widely in skill-evolution benefit | ◐ |
| 2610.07792 | ServeLearnBench | Hidden policies evolve; 7,718 tasks | ◐ |
| 2611.05450 | ReScopeBench | Silent environment changes. Structural vs rule experience | ◐ |
| 2608.14441 | PACE-Bench | 144 physics-shift pairs. Memory anchors to early designs | ◐ |
| 2610.34238 / 2610.33528 | Δτ-Bench / SkillShift-Bench | Ecosystem- and web-shift substrates | ◐ |
| 2611.03219 | Coding agents continual evolution | Harness evolution across 3 SE datasets causes forgetting | ◐ |
| 2609.39148 | Do Self-Evolving Skills Generalize? | Of 21 improving skills: 5 keep all, 13 keep part, 3 keep none of the gain on test | ◐ |
| 2611.22017 | SkillBuffBench (id resolved, 2026-11-17) | Skill access (none / curated / auto) × memory × 4 harnesses | ◐ |
| 2610.14828 / 2608.06144 | EvoCowork / FinEvo-Bench | Domain longitudinal harness-RSI benchmarks | ◐ |
| 2609.35596 | SEABench | Locally useful updates produce later unsafe behaviour | ◐ |
| ICLR27 OnzWTehIaN | Why Is RSI Hard to Verify? | Sufficient criterion plus tests. 17 benchmark families coded | ◐ |
| 2611.13524 / 2610.22466 / 2611.07624 | RSI Tax / HDA / Beneath the Surface | Cost-normalized and decomposed critiques of harness-evolution gains | ◐ (r1 full) |

### Consensus, debates & contradictions
- **Consensus: the endpoint is not the process.**
  - Process diagnostics are now standard: SEAGym's multi-view records (update-validation, ID, OOD, replay, cost), SelfImprove-Bench's ladder, EvoPathBench checkpoints, TEB seed bands. *(Corrected: the draft said "SEAGym's Table 8 dimensions"; Table 8 belongs to the Duan survey.)*
  - The Duan survey's L5 table (Table 8) is effectively the field's checklist ✓.
  - Ren §8.1 adds "regression rates on previously solved instances" and a full cost breakdown as required reporting ✓.
- **Consensus: selection, not generation, is the bottleneck.** The evidence:
  - 78.26% of RSIBench-Data searches that continue past their peak end below it;
  - HarnessOpt's optimistic validation;
  - RSIGym's dev-seen scores;
  - Continual AutoResearch's selection-regret analysis;
  - Evo-Bench's early peak, then "detrimental modifications".
- **Debate: do stronger models make better improvers?**
  - Yes: HarnessOpt (monotone GPT ladder ✓), GDPevo ✓, Flex-RSI (closed ≫ open on binary feedback ✓), Singularity-Bench ✓.
  - Not consistently: RSI-Env ("stronger models are not consistently better improvers"; Teachers ≤ Self, but confounded by Teachers using ≤ 7 of 30 h ✓). Continual AutoResearch hints at the same thing: the highest-baseline agents show the smallest persistence gains, but this is our reading of Table 2, not the authors' claim ◐.
  - Reconciliation: capability predicts *one-shot improvement* but not *benefit from inherited state*.
- **Debate: does editing the harness or improver help at all?**
  - Flex-RSI: +2 to +40 in 6 of 7 pairs ✓, but with unmatched rounds.
  - SelfImprove-Bench: editable improver 1.326 vs fixed 1.207 (n = 1) ✓.
  - Against: Rethinking-Eval (evolution 67.4 below parallel sampling 72.3, round 1) and RSI Tax (a bounded-note baseline matches at 66–71% fewer tokens, round 1).
  - No benchmark builds a matched-compute "no-edit" arm into its protocol.
- **Contradiction: harness transfer across models.**
  - Evo-Bench: "highly transferable reasoning structures" ✓.
  - SEAGym: asymmetric transfer, mostly neutral or negative OOD ✓.
  - Flex-RSI: 2 of 4 hand-offs help ✓.
  - One Recipe Many Harnesses (round 1): edits encode model compensation.
  - Likely cause: Evo-Bench pre-selects harness-sensitive tasks.
- **Contradiction: the meaning of "RSI benchmark".** Most items called "RSI benchmarks" do not test whether an inherited change does anything:
  - Singularity-Bench, AI4AI-Bench, CompressionBench, RSIBench-Data and RSI-Index are single-shot (L1–L2).
  - Only RSI-Env and Flex-RSI let the updated system or improver run later rounds.
  - Even they show structural, not effective, recursion (Duan §1.7).
- **Debate: breadth vs depth of search.**
  - HarnessOpt: breadth correlates with gain, trace reading anti-correlates ✓.
  - SEAGym: batch size is non-monotone. Batch 20 is best, batch 40 gains little, and batch 80 (most evidence per update) loses 17 pp ✓.
  - Both point to an attention or context bottleneck in the improver rather than too little evidence.

### Open problems
1. **A swap-test benchmark.** Improver generation × fixed solver states, at matched compute, with ≥ 3 seeds. Auditing-the-Product Prop. 3 defines it; none of the roughly 40 benchmarks implements it.
2. **Matched-compute no-edit and placebo arms built into the protocol.** Needed to fix Flex-RSI's unequal rounds and RSI-Env's unequal hours. Include TEB-style seed-null bands.
3. **Seeds.** The single-run norm (Evo-Bench, RSIBench-Data, RSIGym, CompressionBench, Flex-RSI, SEAGym) makes most leaderboard gaps smaller than run-to-run variance. HarnessOpt's resolution bands (with two runs per configuration) are the only built-in remedy.
4. **Selection-regret and peak-retention metrics as first-class leaderboard columns** (78.26% end-below-peak; selected vs best candidate).
5. **Grader integrity under self-modification.** Breakwater covers single-shot tasks. Nothing tests graders against an improver that rewrites its own verification across rounds (Masked Erosion is harness-level only).
6. **Drift in the rules of improvement**, not only in tasks or harness. For example: feedback granularity changes (Flex-RSI shows binary vs scaled feedback matters), budget regimes change, or the evaluator is swapped mid-stream.
7. **Frame revision / stale objectives** (Chen §9.6): no benchmark scores whether a system *notices* that its objective is wrong.
8. **Benchmark-construction bias.** Sensitivity-selected tasks (Evo-Bench) and sources that collapse alone (SEAGym HLE-only, which drops to 0) mean benchmark choice can manufacture or erase "self-improvement".
9. **Long horizons.** Streams are 3–5 tasks (Continual AutoResearch), 2–4 blocks (continual learners) or 3 sessions (RSI-Forge). Infrathon shows rankings flip with horizon.
10. **Weights plus harness jointly, with an inheritance gate.** RSI-Env accepts on validity only. No benchmark asks whether a self-trained model is a *better improver* in the next round.

### Where it is heading (to mid-2027)
- **Consolidation into a few protocol standards.** Likely survivors:
  - HarnessOpt/VeRO-style trusted evaluators with resolution bands;
  - SEAGym-style multi-view records;
  - Breakwater-style hardened graders;
  - Auditing's claim-indexed reporting.

  Expect NeurIPS 2027 D&B reviewers to demand seeds, matched-compute baselines and held-out splits as table stakes.
- **The ICLR 2027 wave is decided around late Jan 2027.** Many of these benchmarks are under review there (RSI-Env, RSIBench-Data, PAST-Bench, GDPevo, AI4AI, EvoHarnessBench, Singularity-Bench, TEB, RSI-Forge, CLE, SkillFlow, Continual AutoResearch). Reviewer threads already push on scope and held-out validity (e.g. AgentSWE, RSI-Env responses).
- **From single-shot AI R&D to inherited multi-round.** Singularity-Bench and Infrathon will likely add multi-generation tracks. Flex-RSI already maintains a leaderboard.
- **Improvement-ability training data** (RSI-Dojo, RSI-Prime) creates contamination risk for the benchmarks themselves. Expect regenerable suites (GDPevo V2, SelfImprove-Bench generators) to gain value.
- **A "swap-test" or "process" benchmark is the predictable next entry** (high likelihood by mid-2027). The most probable source is the SJTU/Theseus group (SelfImprove-Bench + Duan survey) or the Auditing authors.

### Connection to our ideas
- **#2 Forced-Quota and #3 Gate, Not Willingness.**
  - SelfImprove-Bench is the direct motivation: 0 of 915 retained opinions touch the improver in main runs ✓, and a fixed vs editable improver differ by only 0.119 at n = 1 ✓ (the sign flips across backbones: Opus-editable scores 0.941, per FULLTEXT_VERIFICATION).
  - Flex-RSI forces improver self-edits every round (400-line cap) with no gate on them. That is a natural "always-forced" arm.
  - Neither measures the propose→propagate→pay-off funnel, so it is still open.
- **#4 Order-2 Audit and the swap test.**
  - Auditing-the-Product supplies the estimand and the 4/26 statistic. "Why Is RSI Hard to Verify?" supplies tests.
  - Our nested-replay design is exactly the missing benchmark item (Open Problem 1). Cite both and position the work as "the first matched-budget swap test, run".
- **#6 Meta-Overfitting.** The evidence on harness transfer across models is mixed and uncontrolled:
  - SEAGym's asymmetric cross-backend OOD ✓;
  - Flex-RSI's 2 of 4 hand-offs ✓;
  - SelfImprove-Bench's G3 directional asymmetry (Hero→Sweepmine positive, reverse negative) ✓.

  None wipes memory or grades domain distance, so the distance-dose design remains differentiated.
- **#7 Varying Goals / #9 MetaShift.**
  - EvoHarnessBench, the continual-learners study, Δτ-Bench, PACE-Bench and ServeLearnBench cover task-, harness- and environment-drift at the object level, so "another drift benchmark" is saturated.
  - Flex-RSI's binary-vs-scaled feedback finding ✓ is the closest evidence that changing the *rules of improving* matters. That supports MetaShift's framing.
- **#8 Openness Ladder.**
  - HarnessOpt's lever-breadth ρ ✓ and the continual learners' surface-size↔stability trade-off ✓ are the two strongest correlational anchors.
  - Both are confounded (breadth is an outcome; surface is tied to method), which is exactly what a randomized ladder fixes.
  - RSI-Env's three guidance settings are a coarse analogue on the *goal* axis.
- **#1 Taught, Not Selected.**
  - HarnessOpt's negative trace-reading correlation ✓ is a surprising counterpoint: optimizers barely read traces. The pilot should measure reading share.
  - RSI-Forge's shortcut codebook and Singularity-Bench's attack bank are ready-made sources of "environment errors".
- **#5 Order-0 Rewards Breed Routers.**
  - Evo-Bench's GPT-5.6 Sol case study shows the evolved harness is a domain router ("naive domain routing instead of… shared mechanisms") ✓. This is direct qualitative support.
  - Use Evo-Bench or HarnessOpt as an order-0 substrate and score order 1 on top.
- **Practical substrate choice for Dan (cheap, open, held-out).**
  - HarnessOpt-Bench protocol (resolution bands) plus SelfImprove-Bench generators (unlimited fresh instances, cheap games).
  - Avoid GPU-heavy gyms: Singularity-Bench, RSIGym, VibeTraining, RSI-Env.
  - Always report seed bands (TEB), selection regret (RSIBench-Data) and matched-compute no-edit arms (Flex-RSI's gap).

Full texts are saved under `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/` (new this session: 2611.01736, 2611.05108, 2611.16591, 2606.17546, 2608.06301, 2608.09096, 2607.25886, 2611.06858, 2608.03764, 2607.05202, 2608.20318, 2609.04280, 2610.27610, 2610.10310, 2606.05661, 2610.33956, 2610.26342, 2608.04003). Semantic Scholar was rate-limited (429) all session, so I took abstracts from the arXiv export API and the ICLR27 OpenReview search endpoint, and did no forward-citation expansion through S2.

#### Fact-check log

| Claim | Verdict | Source location |
|---|---|---|
| Evo-Bench: GPT-5.6 Sol 46.3 (+16.6), Opus 4.8 45.8, human 47.5; single runs; K = 12 auxiliary harnesses; >$500 vs <$40 knee | confirmed | 2608.09096 Table (l.447–459), §5 "We run all experiments one time" (l.490), App. (K = 12, l.394/1289), cost analysis (l.545–548) |
| HarnessOpt: 111 runs; model 0.142 vs harness 0.079 (~1.8×); GPT ladder 0.03→0.49; breadth ρ +0.34…+0.88; trace-reading ρ −0.31…−0.64, 16 requests; 82% case allowance; "hack-resistant, not hackproof" | confirmed | 2608.06301 abstract, §4 (l.468–520), §5 (l.574–589), limitations (l.629) |
| RSIGym: Opus 5 RSI-Index 0.4809, $500/benchmark, SWE-V 17.67→50.33, "not held-out… single run" | confirmed | 2610.10310 abstract; l.382 |
| Singularity-Bench: Opus 5.5 + CC 1.251, $425.41; "other 10 agents average 0.60"; proprietary 1.01 / open-weight 0.53; GPT-6.1 Sol 0.968; GLM 5.3 0.656 vs 0.373; ~4,000 GB300-h, ~350/agent; 9/10 OWASP; Breakwater catches all; informal-run incident | confirmed (at-a-glance 0.60 and card 0.53 are both correct but refer to different groups; clarified) | 2611.16591 abstract; §4 l.707–717, l.757, l.786; §5 l.474–477, l.591, l.624 |
| RSIBench-Data: 58.33% first-valid improvement; 78.26% end lower after peak; one representative run; "not generalization to statistically held-out tasks" | confirmed | 2607.25886 abstract; §4 l.396; limitations l.772–774 |
| SEAGym: AHE +17.1/+9.1/+6.3; TF-GRPO OOD −2.5; replay 34→43, 6/80 drop; HLE-only → 0; GPT-5.4 ID +5.5 / OOD −7.5 | confirmed (ACE is also positive on all three, by small amounts; caveat added) | 2606.17546 Table 2 (l.372–374), replay text (l.364–386), Table 4, §4.6 cross-model |
| SEAGym: "only batch 20 gains" | corrected (batch 40 also gains slightly: +2.9 val / +1.8 ID; batches 10 and 80 lose 14–17 pp) | 2606.17546 Table 3 (l.415–419) |
| SelfImprove-Bench: G0 8/18, G1 16/18, G4 5/8, G3 109.2%, 915 opinions 87.9/12.1/0, revision 40.2% vs 14.2%, HyperDSH 1.207 vs 1.326 | confirmed | 2611.02797 §4 (l.648–674), Table 3 text (l.686–772), App. (l.1138–1149) |
| SelfImprove-Bench curriculum "1.433 vs 0.772 / 1.080 on Combat" | corrected (1.433/0.772 are Medium Combat; 1.080 is increasing-difficulty on Hard Combat vs random 1.314; fixed wins on Medium Hero) | 2611.02797 §4.4.2 (l.786–792) |
| Continual learners: GEPA +3.7 / RB +2.5 / MH +1.4; GEPA P +6.2, S −4.3; r = −0.72, p = 0.008, n = 12; gate removal −3.1 / −5.5; 3,386→1,034 chars erasing +4.0 | confirmed | 2610.25858 §4.1 (l.370), Table 1 (l.408), §4.2 (l.421–487), Fig. caption (l.526, 553), §4.3 (l.566–568) |
| EvoHarnessBench: 802 tasks, 17 streams; −12.1/−13.8/−46.4%; max BWT −34.7%; best gains +27.8/+27.5/+110.2% | confirmed | 2609.04280 abstract, §1 (l.80), summary table (l.814–815) |
| Auditing: 114 / 90 studies; 60%, 22%; 4 of 26 (23 related); Prop. 3 diagonal non-identification | confirmed (added human-sample 45% / 14%) | 2610.27610 abstract (l.24–33), §4.3 (l.391–401) |
| Flex-RSI: 49.7M median tokens; 0.9→38.8 apart; open 1.6 vs closed 16.9 on binary feedback; ρ −0.05; 6/7 pairs +2.0…+40.0; frozen fewer rounds 6/7; hand-off 2/4 | confirmed (task areas re-worded to the paper's six areas) | 2611.08946 §5 (l.648–652), App. (l.958–978), Tables 8–9 (l.1030–1107) |
| RSI-Env: 65.2→73.2, 53.3→56.7 (H1/M0); Knowledge 82→85→88 with 48 SFT pairs; GPQA 53.0; Teachers 1.19–6.97 h vs Self 24.90–28.95 h, all M0; 10×3 h; eval sizes | confirmed | 2611.01736 abstract, Table 2 (l.403–421), §4 (l.561–570), App. config (l.770–782) |
| RSI-Env: Knowledge Self is "the only clean two-step recursion" | corrected (Reasoning Self also ended at H1/M1, with small target gains; claim narrowed) | 2611.01736 Table 2 row "Reasoning Self … H1/M1" |
| Continual AutoResearch: TCG +3.0…+21.3; +24.0→+1.2; outcome −0.9…+7.6; first-valid subtraction makes all negative; ≤0.75 repeat variation | confirmed | 2611.05108 §4 (l.363–378), Table 2 (l.391–403), l.414–415 |
| Continual AutoResearch: "Stronger models gain less" | corrected → ◐ (not stated by the paper; our inference from Table 2, with GPT-5.4 as a counterexample) | 2611.05108 abstract and Table 2 |
| GDPevo: 120 tasks / 12 groups; +2.59…+16.44 pp; oracle 91.6%; "magnitude of evolution comes mainly from the model's intelligence" | confirmed | 2608.03764 abstract; §1 (l.103–109); §4 (l.481) |
| AI4AI-Bench: mean 0.166, best 0.250; 0.226 vs 0.126; 8%→64% | confirmed (upgraded ◐→✓; added 29 configs and mean 0.094→0.196) | 2608.20318 abstract (l.24–33), §1 Results (l.185–189) |
| CompressionBench: 29/36; +3.4% PPL; 77.33 vs 79.56; 17/79; one run per pair | confirmed | 2611.06858 abstract (l.25–28), §3 (l.332, 432–433) |
| CLE: best clears 47.7% of levels, 23.3% full ladders, 120 tasks | confirmed (upgraded ◐→✓; best is Claude Code / Opus 5) | 2610.33956 abstract (l.20), main table (l.959) |
| Survey quotes: Duan "proposed requirements… rather than capabilities established"; Chen "neither a benchmark nor a measurement convention"; Ren "regression rates on previously solved instances" | confirmed | 2609.11873 l.2328; 2607.07663 l.1231; 2607.13104 l.3373 |
| Consensus bullet "SEAGym's Table 8 dimensions" | corrected (Table 8 is Duan's; SEAGym's are its multi-view records) | internal consistency vs 2609.11873 Table 8 |
| ID resolution: 2611.23138 → "RSI-Prime: Training Models to Train Models" (2026-11-17); 2611.22017 → "SkillBuffBench: Measuring What Agent Skills Contribute…" (2026-11-17); 2611.03698 → "Training LLM Agents to Automate LLM Post-Training with RSI-Dojo" (2026-10-29); 2610.27610 → "Auditing the Product, Not the Recursion…" (2026-10-20) | confirmed | arxiv.org/abs pages (export API host unreachable; abs pages used) |
| Evo-Bench affiliation RUC/BOSS Zhipin; title | confirmed (PDF also carries a "Nanbeige" logo) | 2608.09096 p.1 header |
| Remaining-table ◐ items (VibeTraining, RSI-Forge, S3Gym, Infrathon, RSI Arena, TEB, etc.) | unverifiable this pass (not re-opened; marks left ◐/†) | — |

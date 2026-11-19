# Survey digests (read in full)

### The Last AI Built by Humans: Toward Genuine Recursive Self-Improvement (2609.11873)

**Read status: ✓ full text.** I downloaded the PDF (79 pp., v3 of 22 Sep 2026) and read every section of the body (§1–7) and Appendices A, B and C. For the long reference list [1–217] and the 72-row industry table (Table 13), I checked only the entries I needed. Text: `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/2609.11873/paper.txt`

**Bibliographic**
- Authors: Yi Duan, Ying Liu, Zirui Tang, Haodong Chen and Jun Zhou (equal contribution), plus about 30 more. Corresponding author: Xuanhe Zhou. Fan Wu is last author.
- Organizations: SJTU, Theseus Labs, Tsinghua, ByteDance, ModelBest, Xiaohongshu, Shanghai AI Lab, Humanlaya, Agent-Native Research Lab and Frontis.AI.
- arXiv cs.LG, first posted 2026-09-10 (v3 on 09-22). It is a survey/position report with no venue. Length is 79 pages: about 49 pages of body, then references, then appendices.
- Companion "awesome-rsi" repo (github.com/theseus-labs-rsi). Our round-1 sweep calls it the most-viewed RSI paper (frontier-trends.md l.11).
- Link: https://arxiv.org/abs/2609.11873

**Scope & definitions** (✓ §1.2, §2.2)
- **RSI:** a system that, through continued interaction, "autonomously transform[s] acquired experience and feedback into persistent changes to itself… such that these changes can further affect the mechanisms used to generate, evaluate, select, and consolidate subsequent self-improvements" (§2.2.2).
  - The paper names three aims: a wider capability frontier, resource efficiency, and novelty beyond human-prescribed strategies.
  - It frames them as three dimensions: autonomy, efficiency and innovation (§1.2).
- **Unit of analysis = the improvement loop** (§2.2.1). The components are:
  - AI system, system state, experience, target, improver, strategy, verifier, improvement (an accepted, inherited change) and successor.
  - Every system is analysed with the same three questions: *Where does the loop close? What is updated and inherited? Which decisions remain external?*
- **Structural vs effective recursion** (§1.7, §3.6).
  - Structural: a revised improvement mechanism persists and governs a later round.
  - Effective: that mechanism yields better successors under matched budgets and independent evaluation.
- **Neighbouring paradigms** (Table 1, §2.2.3). Continual learning, AutoML and agentic AI/ML are scored on eight properties. Only RSI covers both "mechanism revision" and "mechanism reuse".
- **Evidence scope** (§2.3). Industrial blogs, repos and company reports are explicitly admitted as primary evidence.
- **HCI motivation** (§2.1). The Headroom-Closed Index normalises 393 model–benchmark observations across 10 domains, 2023 to Sep 2026.
  - Interactive domains lag: tool agents 39.9, software engineering 52.6 and search/terminal 56.8, versus math 86.4 and graduate science 85.8 (✓ §2.1 Obs. 1–2).
  - The post-2026 "RSI extension" is purely illustrative: R_d = 100 − 0.22(100 − T_d) (Eq. 4).

**Taxonomy** (✓ §1.4, §3, Fig. 1–2, Tables 2–8)
- **B0 – in-task improvement** (not RSI; the output changes but the system does not). Self-Refine, Reflexion, Tree of Thoughts, APEX-EM. Four listed limits: no accumulation across tasks, a human-defined procedure, self-feedback that reinforces errors, and diminishing gains under a fixed verifier.
- **L1 – execution autonomy.** Humans fix the objective, procedure and acceptance criteria; the AI executes and the result persists. Organised by pipeline stage (Table 3):
  - Data: Google label curation, Phi-4-reasoning selection, FineWeb-Edu, NeMo Curator, Data-Juicer, SynthLLM, SynthAgent, Nemotron-4.
  - Training method: EDIT, REPO.
  - Training platform: Agent-Agnostic C/C++ optimisation, Meta NCCL runbook debugging.
  - Evaluation/safety: HealthBench.
  - Deployment: AIPC.
  - Application: AWS Agent Toolkit, LinkedIn CAPT, OpenAI Harness Engineering, Meta Capacity Efficiency.
- **L2 – strategy autonomy.** The AI chooses *how* to improve; humans fix the objective and acceptance rule (Table 4).
  - Prompt search: Promptbreeder, GEPA (including Dropbox use), MPO, C-Evolve.
  - Agent/harness search: Microsoft Foundry Agent Optimizer, ADAS, AFlow, AgentSquare.
  - Model/training search:
    - configuration: NVIDIA TAO/Cosmos;
    - architecture: AgentNAS;
    - training rule: GPT-6 Astra NanoGPT eval, Karpathy autoresearch, Auto Research;
    - systems/kernels: AutoKernel.
- **L3 – experience-acquisition autonomy.** Learner state decides what to learn next, and the update feeds back into later acquisition (Table 5).
  - Adaptive task generation / self-play: SSP, AZR, R-Zero, STP, PSV, VisPlay.
  - Autonomous practice through environment interaction: VOYAGER, SIMA 2 (only in the full ASKA setup), SEAgent.
  - Fig. 6 maps industrial data pipelines on a manual → human-in-the-loop → automated scale. The paper says automation alone ≠ L3.
- **L4 – deployment/environment adaptation.** Deployment interaction decides which consequences persist (Table 6).
  - Trajectory distillation:
    - text memory: Dynamic Cheatsheet, ACE, ReasoningBank;
    - structured memory: APEX, PersonaAgent, PAHF, MemToolAgent;
    - skill libraries: Trace2Skill, PRACTICE, PANDO, PILOT;
    - executable artifacts: Metis, Evo-Harness, SHAPER;
    - evaluation: Evo-Memory.
  - Iterative system revision:
    - co-evolution: DecoEvo;
    - benchmarks: HarnessDev, ASPIRE, S3Gym;
    - analysis: "Harness Updating Is Not Harness Benefit".
  - Selective retention:
    - admission: HDSO paired control/treatment;
    - library maintenance: Library Drift, Rethinking Skill Evolution;
    - governed deployment: Tax AI.
- **L5 – recursive inheritance / meta-improvement.** A mechanism that governs later improvement is revised and reused (Table 7).
  - Search procedure: STOP, Gödel Agent, DGM (a "transition case", because archive and parent selection are fixed), HyperAgents.
  - Successor evaluation: Red Queen Gödel Machine (RQGM).
  - Research policy: A-Evolve-Training.
  - Industrial: Meta AIRA2 and Anthropic AAR (execution only, *not* inherited procedure); Weco AIDE2.
- **Cross-cutting technique matrix** (Table 2): 5 families × 20 techniques × L1–L5.
  - Families: Search & Optimisation; Data, Task & Experience; Training & Persistent Update; Verification & Selection; Online & Meta-Level Adaptation.
  - Empty cells are flagged, e.g. there is no L5 example for evolutionary search beyond DGM/RQGM.
- **Target taxonomy** (Appendix A, Table 12): 10 primary targets and about 40 sub-targets.
  - Prompt & Context; Memory & Knowledge; Harness/Workflow & Control; Tools & Skills; Model; Trainer/Optimisation; Evaluator & Feedback; Data & Environment; External Artifact; Full-system/Co-evolution.
  - The last target includes "10.3 Improvement-loop / Meta-RSI".
  - Fig. 18 sorts 491 papers by level: L1 43.8%, L2 31.6%, L3 13.0%, L4 5.7%, L5 5.9%.
- **Application regimes** (§4, App. C; Fig. 9 gives each domain's frontier):
  - S1 Science (strong L2, early L3): HypoForge, EvoScientist, TTT-Discover, Test-Time Tool Evolution, CASCADE, SkillFoundry, DrugSAGE, SIA, CORAL, SAGA.
  - S2 Embodied (L2–L3 frontier, L4 only in simulation; ENPIRE ≈ bounded L5): POET, EnvGen, GenEnv, OMNI-EPIC, SimWorld Studio, Voyager, LRLL, EmbodiSkill, SHAPER, ASPIRE, World-VLA-Loop, VLAW, Motus2.
  - S3 Software (mature L2, emerging L3, bounded structural L5; L4 "largely absent"): SICA, Self-Harness, Agentic Harness Engineering, Ouroboros, SWE-Exp, CODESKILL, EvoMAC, DGM, HGM, Group-Evolving Agents, DarwinX, HELIX, Self-play SWE-RL, Socratic-SWE.
  - S4 Healthcare (mature L2, limited L3-like, simulated L4): MedAgent-Zero, DxEvolve, GSEM, Evo-MedAgent, EvoClinician, EvoMDT, MACRO, SkeMex, HealthFlow, EvoPatient.
- **Industry** (App. B, Table 13): 72 companies in six archetypes.
  - (A) frontier labs, (B) RSI-native/AI4AI, (C) autonomous R&D/science, (D) agent-optimisation/eval infrastructure, (E) embodied/continual, (F) memory/personal AI.
  - Each row carries a level tag. "cand." means not demonstrated; "adj." means adjacent.
  - DGM, HyperAgents and AI Scientist are tagged "L5 cand."; Ricursive is tagged "L2; L5 vision".

**Key claims & findings**
- Three blockers to credible RSI (✓ §1.3):
  - **Safe inheritance:** Gödel Agent had 14/100 MGSM runs end below the seed.
  - **Autonomy attribution:** DGM went from 20% to 50% on its SWE-bench subset, but its archive and parent selection are not self-modified.
  - **Reliable verification:** Anthropic's automated W2S researcher showed seed cherry-picking and test-label extraction.
- A higher level ≠ a better process. Delegated scope and improvement quality are separate axes (✓ §3.3.4, §3.7).
- Adaptive evaluator access turns the benchmark "part of the optimization surface rather than a passive measurement instrument" (✓ §3.3.4).
- **Experience corruption** is named as a structural L3 risk: defective feedback or memory spreads across rounds (✓ §3.4.4).
- **Persistent update failure** at L4 has four modes (✓ §3.5.4): the wrong lesson is drawn, a lesson is used out of scope, a skill is not invoked, or earlier capabilities are lost. Final scores cannot tell these apart.
- Harness Updating ≠ Benefit (✓ §3.5.2, the survey's summary): the ability to update a harness does not depend on base-model strength, while the ability to benefit from an update is non-monotonic. Failures are either non-activation or unfaithful following.
- Library Drift (✓ §3.5.3): unbounded skill accumulation degrades retrieval before scores show it. Over-aggressive retirement is worse than no governance.
- L5 evidence is called "bounded meta-improvement" (✓ §3.6.5). Statistically reliable accumulation across generations under matched resources remains open. Supporting numbers:
  - STOP: the gen-4 improver beat the seed on all 5 transfer tasks, but weaker-model runs regressed and some programs evaded budgets.
  - HyperAgents: meta-agent transfer to math grading worked, but the 200-iteration run found no significant advantage.
  - AIDE2: 7 accepted improvements over 100 steps, but no significant efficiency gain as an outer improver.
  - RQGM: 71.7% vs 69.9% for HGM-H on held-out Polyglot.
  - A-Evolve-Training: external score 0.80 → 0.86, against 0.87 for the best human.
- Industry cases (✓ §5, all company-reported):
  - Theseus: a clean vs noisy workspace gives +21.7 to +51.6 pp; a reconstructed environment gives +18.65 to +39.67 pp (Table 9).
  - Lark: knowledge graph usability 52 → 65% (human-rated); auto-evaluator agrees with humans about 84% of the time.
  - Xiaohongshu: score_mean@16 +7.0% (Table 10).
  - Humanlaya: V0 → V4 on 600 held-out packages, defect rate 9.0 → 3.7%, handling time 48 → 27 min.
  - ModelBest ForgeTrain: matched Megatron-LM in about 8 h; MFU 40.1 → 44.1%.
  - Hyra: nanochat BPB 0.9015 vs 0.9109 (Table 11).
  - ARA: QA 93.7 vs 72.4%; RE-Bench reproduction 57.4 → 64.4%.
  - Frontis: score 5.25 → 5.87 on 76 tasks; about 20% faster evolution with cross-task meta-experience; MLE-Bench Lite 39.39 → 60.61%.

**Evaluation practices it describes or criticizes** (✓ §3.2.7, 3.3.4, 3.4.4, 3.5.4, 3.6.5, Table 8, §6)
- **Criticised:**
  - repeated dev-set access;
  - extra search compute counted as algorithmic gain;
  - an LLM judge that shares the proposer's blind spots;
  - company results treated as replications;
  - task gains taken as proof of a better improver;
  - final-score-only evaluation at L4;
  - "unattended runtime" used as a proxy for saved human effort.
- **L1:** check execution reliability and persistence safety, i.e. validate an artifact before it propagates.
- **L3 controls:**
  - freeze the learner-state input to the acquisition mechanism, or replace adaptive selection with a learner-independent schedule;
  - match data and environment access and the total generation, assessment and learning budget;
  - do *not* hold the samples identical;
  - separate correctness, difficulty and learning value;
  - separate curriculum feedback from protected evaluation.
- **L4:** trace each retained change back to its evidence, its later use, and its effect on both new and previously solved tasks; test across time and distribution shift.
- **L5 mechanism audit:**
  - identify the revised artifact, the evidence behind it, and where it is invoked later;
  - compare original vs revised mechanism from comparable agents under matched budgets that include the cost of evaluating the mechanism;
  - hold the evolved mechanism fixed on fresh tasks (the HyperAgents design).
- **Table 8 dimensions:**
  - adaptivity, retention, transfer, efficiency, stability (from SEA-Eval and SEAGym);
  - plus a new "meta-recursion" row: mechanism reuse, successor quality, goal and stopping decisions.
- Also recommended:
  - protected reference tasks and private tasks with undisclosed rules;
  - report whether the system stops when expected benefit < cost.

**Open problems / future directions it lists** (✓ §6, complete; eight directions, after the three challenges in §1.3)
1. Cross-component diagnosis and coordinated improvement: attribution across data, context, tools, model and evaluator; selective freezing and ablations; recorded dependencies.
2. Learner-conditioned experience acquisition and reliable learning signals: estimate multi-round learning value; decide when costly verification is worth it.
3. Persistent-state management and reliable reuse: provenance, applicability conditions, activation vs faithful use; when to merge, retire or revalidate; cross-executor reuse.
4. Governed adaptation under domain-specific feedback: staged validation; separating transient from recurring failures; irreversibility in physical and clinical settings.
5. Trustworthy evolution of improvement mechanisms: editable internal feedback vs protected acceptance; detecting coordinated proposer–evaluator errors; reversing harmful meta-changes.
6. Long-horizon evaluation of inherited improvement capacity (structural vs effective L5): matched total budgets; full trajectories including rejected updates; descendant-quality vs current-score.
7. Resource-aware improvement and human collaboration: budget allocation across diagnosis, acquisition, search, verification and monitoring; cost to a validated target; adaptive stopping and escalation.
8. Reproducible infrastructure for cross-round inheritance: an artifact format linking parent state → change → evidence → evaluation config → decision → later use; versioned data, environments and evaluators; replay studies.

Per-domain "How far are we" boxes (App. C) repeat these: attribution, selective transfer, provenance, reversibility, and keeping safety constraints fixed.

**What it misses or gets wrong as of Nov 2026**
- **It never measures the self-edit funnel.** The survey separates structural from effective L5, but it does not report or ask how often improvers propose self-edits, or whether the selection rule keeps them.
  - Our full-text check (FULLTEXT_VERIFICATION.md): DGM, Hyperagents and HGM keep every valid child. Only HMED scores improver edits separately. SelfImprove-Bench shows 0% retained improver changes when unforced.
  - The survey's "autonomy attribution" criticism of DGM (fixed parent selection) is right but incomplete. The gate itself barely filters.
  - HMED, SelfImprove-Bench, RSIFlow, VERSE, MetaSkill-Evolve, FreeEvolve and Meta-Harness are not cited (0 hits each).
- **No inheritance-concentration finding.** Improvement by Inheritance (ICLR'27 submission ZMmJDeDTXk) is absent, so the survey misses the point that gains are mostly one early edit inherited downstream. It also omits "Does Failure Feedback Direct Self-Modification?" (random selection reproduces DGM trajectories). Both make its L5 picture look more positive than our evidence supports.
- **The HyperAgents summary is one-sided.** It cites the n.s. 200-iteration result and transfer to math grading, but not the imp@50 = 0.630 meta-transfer number we verified. FreeEvolve's *negative* cross-environment transfer is absent. Meta-overfitting is never named.
- **The verification threat model is about selection only.** All of its verification discussion (evaluator exploitation, frozen evaluators, anchors, protected sets) concerns the score channel. It mentions "experience corruption" (L3) and the "wrong lesson from noisy evidence" (L4) but never separates *reading* corrupted traces from *being selected* by a corrupted score. Meta-Harness raw-trace vs score-only evidence is not cited.
- **Safety and control are thin.** There is no treatment of RSI control, covert sabotage, or the alignment of an improving improver, beyond "human mission and safety boundaries". The later positions 2610.35193, 2610.14625, 2611.10261 and 2610.27735 postdate it, so this is not an error, but it is a gap compared with the field as of Nov 2026.
- **Nothing on drift-rate theory.** Varying-goals or evolvability theory (Kashtan & Alon) and drift *rate* are absent. "Drift" appears only as Library Drift and a generic call to "examine adaptation across time and distribution shifts".
- **Conflict of interest.** Most of the §5 case studies come from the authors' own affiliations (Theseus, ByteDance-Lark, Xiaohongshu, Humanlaya, ModelBest, Agent-Native Research Lab, Frontis). The authors call these results company-reported, which is appropriate. Several are not RSI at L5 by the paper's own criteria; e.g. Theseus's tables test environment quality, not a closed loop.
- **Overstated forecast.** The HCI "post-2026 RSI stage" uses an arbitrary constant 0.22 (Eq. 4) and is illustrative only, yet it appears in the headline figure.
- **Minor issues:**
  - a broken cross-reference "Appendix ??" in §4;
  - appendix headers mislabelled "APPENDIX A: RSI IMPLEMENTATION TECHNIQUES";
  - ASPIRE is cited as two different works, [109] and [191].
- **Correction to our own notes.** frontier-trends.md (round 1, l.68) calls it "no experiments". It does contain an original HCI meta-analysis (393 observations) and new company-reported pilot tables (Tables 9–11).

**Relevance to Dan's ideas**
- **Taught, Not Selected:** high relevance, and it is a gap this survey leaves open.
  - The survey's "reliable verification" challenge, the RQGM/DecoEvo anchors and the HDSO paired gates all defend the *selection* channel.
  - Its "experience corruption" (§3.4.4: "persistent memory may carry incorrect assumptions into future practice") and the L4 "wrong lesson from noisy evidence" mode are the closest it gets to a *teaching* channel, but it gives them no decomposition or experiment.
  - Cite it as the canonical framing that treats environment error as evaluator exploitation. Our 2×2 tests the channel it leaves unmeasured.
- **Propose / Propagate / Pay off funnel:** a direct fit.
  - Structural L5 corresponds to propose + propagate; effective L5 corresponds to pay off.
  - Its §6 direction 6 ("report complete trajectories… including rejected updates", matched budgets, descendant quality) is close to a call for our funnel.
  - It says DGM's parent selection is fixed but does not show that the gate keeps everything. The funnel adds that, plus the edit-rate measurement it lacks.
  - Use its Table 7 "External controls" column as related work for the design-by-weak-stage table.
- **Meta-Overfitting:** partial overlap.
  - It reports STOP's transfer, HyperAgents' transfer to math grading and AIDE2's "transfer to external tasks" as positives. It also recommends "holding the revised mechanism fixed during transfer tests".
  - It does not grade domain distance or separate procedure from memory.
  - Its §6 direction 5 asks "whether such policies transfer beyond the research setting in which they evolved", which is our RQ almost verbatim. Quote it as motivation.
- **Meta-reward (Order-0 rewards breed routers):** moderate relevance.
  - Its L5 effectiveness criterion ("produces or selects better successors under comparable budgets") is effectively Q^(1).
  - It never discusses *what reward trains the improver*, or that task-score rewards favour routing.
  - HGM's metaproductivity and Frontis-MA1 (training improvement skill into a 35B model: MLE-Bench Lite 39.39 → 60.61% under a fixed harness) are adjacent precedents it does cite.
- **Openness Ladder:** useful scaffolding but no answer.
  - Table 12 (10 targets, about 40 sub-targets) is a ready-made list of editable surfaces for the rungs.
  - The level definitions (L1 → L5) are a *responsibility* ladder, not an editable-surface ladder.
  - It asserts that more autonomy is not better, but never ablates the editable surface with everything else fixed. That supports the novelty of the ladder.
- **Continual / drift (Varying Goals, MetaShift):** weak coverage, so there is room.
  - L4 is defined around changing deployment conditions. It covers Library Drift and asks for evaluation "across time and distribution shifts".
  - It never asks *when* self-editing pays off as a function of drift rate or drift type.
  - Its claim that S3 "L4 environment adaptation remains largely absent" (App. C.3.4) directly motivates a software DriftStream benchmark.
- **Register snowball:** essentially irrelevant. There is no discussion of language or register drift in self-improving models.
  - The only tangent is §5.7: agent-native research artifacts as a successor-readable alternative to human-oriented papers. It loosely parallels "readable to models, not humans", as an engineering design choice rather than an emergent drift.

**Verification marks:** every number above was checked in the full text (✓), with section, table or equation given. The comparisons against project files come from FULLTEXT_VERIFICATION.md, ONE_PAGERS.md, round3/IBI_GAPS_AND_METAREWARD.md and round1/sweep/frontier-trends.md.

---

### Recursive Self-Improvement in AI: From Bounded Self-Refinement to Autonomous Research Loops (2607.07663)

**Read status: ✓ full text.** All of §1–9, the Data availability note and the reference list (196 refs) were read from arXiv v2 (dated 6 Sep 2026; 44 pp. including refs). Local copy: /tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/2607.07663/paper.txt. Figures were rendered as images, so only their captions were read.

**Bibliographic**
- **Authors and affiliations:** Mingguang Chen (DeepGrounding, corresponding author), Licheng Wang (AlphaAvatar) and Bo Qu (Illinois Institute of Technology). This is a small, non-frontier-lab team.
- **Dates and venue:** v1 was posted 2026-07-08; v2 is dated 6 Sep 2026. It is an arXiv preprint (cs.AI) with no venue stated.
- **Length:** about 29 pages of body text plus references, 44 pages in total.
- **Corpus and code:** the corpus and scripts are at github.com/deepgrounding/recursive-self-improvement.
- **Link:** https://arxiv.org/abs/2607.07663
- **Corpus:** 1,250 arXiv papers from 2024–2026. A seed harvest of 871 papers across 7 threads was enriched with OpenAlex metadata. A supplemental harvest of 379 papers added self-evaluation, test-time training (TTT) and zero-data self-play. Data cutoff is early July 2026. ✓ §2.3, Fig. 6.

**Scope & definitions (✓ §2.1)**
- **Motivation.** The paper is framed around Anthropic's May 2026 essay on recursive self-improvement [5]. That essay describes a 5-stage autonomy continuum, says Claude writes over 80% of Anthropic's merged code, and names research direction-setting as the bottleneck. The authors say they use the essay "as a motivating frame and a source of stage vocabulary, not as evidence."
- **Defined terms:**
  - **Agent:** an LLM inside a perceive–act loop with tools and memory. A bare model call is not an agent.
  - **Harness/scaffolding:** prompts, tools, memory, skills, retrieval, orchestration and stopping rules. The harness is externally inspectable and editable, including by the agent itself.
  - **Evaluator:** anything that maps an artifact to a quality signal. "Verifier" means the evaluator has soundness guarantees; "judge" means it does not.
  - **Self-improvement:** a system takes part in producing a better version of itself or its outputs, where "better" is defined by some evaluator. They write: "The definitional dependence on an evaluator is not pedantry; it is the source of every failure mode in §5."
  - **TTT:** weight updates during deployment, conditioned on the query or session.
  - **Self-play:** the model generates its own tasks or opponents, including the zero-data regime.
  - **The central cut:** "Bounded self-refinement improves a system against a fixed, external evaluator; it is convergent and evaluable. Open-ended recursive self-improvement modifies the system and the criteria or machinery of improvement itself … divergent in principle."

**Taxonomy (✓ §2.2, Table 1, Fig. 1)**
The taxonomy has two axes: what the system improves × degree of loop closure. A fifth family (foundations) sits beside the four technical categories.

- **Axis 1: what is improved**
  1. **Deployment-time self-evolution (§3; 393 papers, 74% posted in 2026).** Ordered by how long the improvement persists.
     - Output refinement. Its improvements vanish when the episode ends.
       - §3.1 text and reasoning self-critique: Self-Refine, Reflexion, Snell et al. test-time scaling, SymbolicAI, SQL-o1, the literary-translation refinement study [27], Huang et al. "cannot self-correct" [28], BinEval.
       - §3.2 code self-repair via execution feedback: LEAP (Lean), KVerus, the student–teacher feedback decomposition [39], the placebo-controlled self-repair study [40], DISC, FLARE, CoSPlay (code and tests co-evolve).
       - §3.3 multimodal self-critique: Kestrel, Reflect-R1, Proprio, ActiveScope, "visual under-conditioning" in self-evolving LMMs [53], JudgeFit.
     - §3.4 test-time training (87 papers). Its updates last a session. Examples: query-conditioned TTT [56], continual reasoning-trace memories [57], "sleep" consolidation [58].
     - §3.5 harness and agent self-evolution. Its changes accumulate indefinitely. Examples: Gödel Agent, Darwin Gödel Machine, the intrinsic-metacognitive-learning position paper [59 = 2506.05109], test-time rubric-evolving deep-research agents [61 = 2601.15808], Red Queen Gödel Machine [62 = 2606.26294], Escher-Loop [63 = 2604.23472], topology evolution [64], experience graphs [66], "engineer the loop" [14 = 2607.00038], Meta-Agent Challenge [67 = 2606.04455], SAGE socialized evolution [68 = 2606.03544], inference-scaling Pareto [69].
     - §3.6 skill libraries. Examples: Voyager, SkillsBench / SkillAxe [71 = 2606.10546], Skill-R1, SkillRevise, AlgoSkill, SkillMaster, federated skills, SHARP (an auditable rubric policy for trading). Attack and safety papers: SkillMutator, SkillHarness, VASO, the self-evolving-agent threat analysis [83 = 2606.23075], "healthy evolution" [84 = 2606.06114].
  2. **Training-time self-iteration (§4; 340 papers, 69%).**
     - §4.1 self-rewarding RL: Self-Rewarding LMs, ReST-MCTS*, SEVA, EvoIdeator, RePro, the activation "value axis" [91], Self-Trained Verification [92 = 2605.30290], rise-and-collapse [93 = 2606.21090], "scientific amnesia" [17 = 2606.21089].
     - §4.2 chain-of-thought self-training: STaR, Re-ReST, PRefLexOR, LaTRO, elicitation limits [98], Self-Verified Distillation, Semi-CoT, ThinkTwice, "a verifiable search is not a learnable chain-of-thought" [104].
     - §4.3 on-policy self-distillation (OPSD; 56 papers, a thread that is new in 2026): power-distribution unification [105], simple self-distillation [106], Purified OPSD, privilege-induced style drift [112], prefix failure [113].
     - §4.4 self-play: SPIN, Absolute Zero, R-Zero, Agent0, prover–conjecturer co-evolution [118], survive-or-collapse via data gating × reward grounding [120 = 2605.22217], population cross-evaluation [121], meta-evaluation for non-verifiable tasks [122 = 2601.21464], attractor states [124 = 2606.30571].
     - §4.5 embodied and synthetic-data loops: VLM critics for robots, self-reflective VLA policies, DataEvolver.
  3. **Self-evaluation (§5; 318 papers, 82%).** The paper calls this the "pillar" the other three categories stand on.
     - §5.1 design space: PRMs (Lightman), LLM-as-judge (MT-Bench, about 80% agreement with humans), Gao overoptimization, judge datasheets, rubric evolution, evaluator co-evolution.
     - §5.2 verification hierarchy, from strongest to weakest: formal verifiers > execution feedback > learned judges > intrinsic signals.
     - §5.3 four failure modes.
     - §5.4 the non-verifiable frontier.
     - §5.5 result-level vs process-level improvement.
  4. **Auto Research (§6; 139 papers, 76%).**
     - §6.1 evolutionary program discovery: FunSearch, AlphaEvolve, EoH, AlphaEvolve-style production work (code layout, FHE kernels), EVOM, POISE, MLEvolve, discovery as meta-optimization [168 = 2606.26728], harness studies [170], LEVI.
     - §6.2 AI-scientist agents: The AI Scientist, EurekAgent, Heuresis, Xcientist, A-Evolve-Training [180 = 2606.20657], multi-hypothesis failure attribution [181].
     - §6.3 critique literature: ScienceAgentBench, ResearchArena / "How far are we from true auto-research?" [183 = 2605.19156], MLReplicate, claim-level auditability [152], SciIntegrity-Bench [143 = 2605.10246], "correct answer, wrong mechanism" [150 = 2606.23175], "Dead Science Walking" [187 = 2606.04220], a human-in-the-loop economics study [188].
  5. **Foundations, limits & safety (§7; 60 papers, 57%).**
     - §7.1: Schaul's boundless Socratic learning, self-rewarding guarantees [190], the Turing-jump computability bound [191 = 2605.27381], runaway-growth dynamics [192], the Whitfill & Wu compute/labor elasticity [193 = 2507.23181], Zenil's impossibility result [146 = 2601.05280].
     - §7.2: the skeptical position, plus the interestingness theory [155 = 2605.14831].
     - §7.3: the unfireable safety kernel [194], inference-scaling governance [195], bio evaluation frameworks [196].
- **Axis 2: loop closure**
  - Human-in-the-loop: a person reviews each change.
  - Human-on-the-loop: the signal is automatic, but humans audit outcomes and gate deployment.
  - Closed loop.
  - Nearly all 1,250 papers sit in human-on-the-loop cells, which map to Anthropic stages 3–4. The closed-loop row is sparse. Its most consequential cell is self-evaluation × closed loop, which the paper calls the point where bounded refinement shades into open-ended RSI.
- **What the axes deliberately leave out (✓ §2.2).** Both axes "take the objective as given." The grid is also "indexed by substrate rather than by trajectory": one system that improves its outputs, then its evaluator, then its research method occupies three unrelated cells. The authors defer "continuity of method across a change of frame" to open problem 6.

**Key claims & findings**
- **Self-improvement is not one thing.** The "self-X" vocabulary hides large differences in ambition and risk. ✓ §1, §2.2.
- **The evaluator is the bottleneck.** "Every self-improvement loop is a claim that some signal can substitute for human judgment, and the loop's ceiling is exactly the quality of that substitute." ✓ §5 intro.
- **Demonstrated strength tracks the verification hierarchy.** The authors flag this as "a qualitative pattern … not a measured law." FunSearch and AlphaEvolve sit on the top two rungs. The AI-scientist gap is "a level-4 problem being attempted with level-3 tools." ✓ §5.2.
- **The hierarchy's floor has been measured.** In the Mirror Loop study [140 = 2510.21861], informational change fell 55% over 10 rounds of ungrounded self-critique. One grounding step at iteration 3 restored forward movement. ✓ §5.2 (number as reported by the survey).
- **Design rule: "no external signal, no reliable improvement."** The 2026 literature quietly moved from closed-loop self-critique to verified refinement. The paper calls this "a retreat from autonomy that improved reliability." ✓ §3.1, §3.3 assessment.
- **LLM-written skills are weak.** On SkillsBench, human-written skills add +16.2 pp while LLM-written skills add no measurable gain. ✓ §3.6 (citing [71]).
- **Inference-scaling returns are modest.** Across 34 configurations, the peak gain is +7.1 points over CoT, reached only at about 20× compute. "A large share of the theme's gains currently flows from better engineering rather than from anything recursive." ✓ §3.5.
- **Persistence changes the risk calculus.** "A corrupted skill in a shared, federating library propagates." Accumulation without verification is the core safety problem. ✓ §3.6.
- **Collapse is a default dynamic, not an edge case.**
  - Rise-and-collapse can happen under a genuinely verifiable reward, and KL/EWC constraints do not prevent it [93].
  - Self-play survival depends on two separate levers, data gating and reward grounding [120].
  - Synthesis: "pure closed loops degrade … the open question is how little external grounding suffices."
  - ✓ §4.1, §4.4, §5.3.
- **Training loops transmit bias as efficiently as capability.** Examples are OPSD's privilege-induced style drift and multimodal language-prior shortcuts. ✓ §4.3, §4.5.
- **Four failure modes. ✓ §5.3**
  1. The self-confirming loop: confidence-coupled rewards over-reward confident mistakes [141 = 2510.08977].
  2. Distributional collapse: Shumailov; the entropy-reservoir unification [145]; Zenil.
  3. Diversity collapse: proposers narrow the curriculum [149 = 2604.03472]; attractor states.
  4. Frame lock-in: a grounded, honest, stable loop that optimizes an objective that has gone stale. It is "detectable" only from outside the loop. Evidence includes [150], [17], [124], and the agent-native research artifacts paper [151 = 2604.24658], where prior failure traces both speed agents up and box them in.
- **Direction-setting is two problems. ✓ §5.4**
  - Direction *evaluation* is verification-shaped. On SoundnessBench [153 = 2605.30329], 1,099 ICLR proposals, 12 frontier models show "pervasive optimism bias."
  - Direction *generation* comes before the hierarchy. The paper cites the McNamara-fallacy position paper [154 = 2605.08956] and the interestingness theory [155].
- **Result-level vs process-level improvement. ✓ §5.5**
  - Result-level improvement is opex: cheap, paid again per instance, transfers poorly.
  - Process-level improvement is capex: expensive, but reusable.
  - The mature endgame may look like "maturing methodology … attached to a model whose raw capability grows much more slowly," rather than takeoff.
- **A-Evolve-Training [180] is the closest published case of "closing the loop." ✓ §6.2 (numbers as the survey reports them)**
  - It autonomously post-trained a 30B model over 4 rounds and multiple weeks.
  - It scored 0.86 vs 0.87 for the top human submission, 8th of about 4,000.
  - Mid-run, it detected that its development metric had decoupled from external performance and began treating the proxy as evidence *against* candidates.
  - The authors still call this "confirmatory" of the essay's view, because the objective was set by humans.
- **The field's emerging answer is evaluator co-evolution.** The same move appeared independently at least five times: CoSPlay, JudgeFit, BinEval, Self-Trained Verification, Red Queen / meta-optimization. Whether this "escapes the self-confirming loop or merely gives it a second story" is the pivotal question. ✓ §8.
- **Theory.**
  - Internal self-modification stays inside its computational layer [191].
  - The compute/labor elasticity is unresolved: the baseline specification finds substitutes, the frontier-experiments specification finds complements [193].
  - No bound applies to "bounded self-improvement plus human direction."
  - ✓ §7.1–7.2.
- **Corpus shape. ✓ §8**
  - Foundations is only 60 of 1,250 papers, and the paper calls it the most underpopulated niche given the stakes.
  - Four hypotheses explain the skew: a verifiability gradient, a cost-of-entry gradient, herd dynamics ("legibility, not importance, sets the gradient") and institutional role assignment.
  - Mechanisms outrun evaluation, and the diagnostic papers carried the most weight in the analysis.

**Evaluation practices it describes or criticizes**
- Most code-repair evaluations measure single-attempt accuracy, although the real value is in multi-round loops [43]. ✓ §3.2.
- Naive feedback comparisons are confounded by resampling, format correction and extra compute. The paper endorses placebo-controlled and decomposed designs [39, 40]. ✓ §3.2.
- Fixed benchmarks get gamed. Passing tests underdetermines correctness. Self-improving agents assume a stationary evaluation criterion, which Red Queen criticizes. ✓ §3.5, §5.2.
- LLM judges carry position, verbosity and self-enhancement biases and per-judge competence differences. A single global schema overcredits them, which motivates measurement datasheets [134] and JudgeFit. ✓ §3.3, §5.1.
- AI-scientist evaluation:
  - Manuscript-only automated review overstates quality compared with artifact-aware review and human meta-review (ResearchArena).
  - Benchmarks decomposed by workflow and replication benchmarks are preferred to end-to-end claims.
  - Outcome, mechanism fidelity and epistemic honesty should be measured separately [150].
  - Integrity should be tested under completion pressure. SciIntegrity-Bench finds a 34.2% failure rate, and all 7 models fabricate data in missing-data cases.
  - ✓ §6.3, as reported by the survey.
- The A-Evolve-Training detail shows dev metrics decoupling from external targets inside a single run. ✓ §6.2.
- Missing infrastructure: nothing yet shows that "a training loop is not self-improving past a threshold." ✓ §7.3.
- Self-critique of the survey's own method:
  - A single annotator did the classification, with released per-paper assignments.
  - About 54 of 379 supplemental papers are query bleed.
  - Citation counts are near zero and should not be read as impact.
  - Frontier-lab practice is censored by what labs choose to publish.
  - ✓ §2.3.

**Open problems / future directions (complete list, ✓ §8, "in rough order of leverage")**
1. **The exchange rate of grounding:** the minimum exogenous-signal rate that sustains improvement. The entropy-reservoir framework [145] suggests this is well-posed.
2. **Verifying the non-verifiable:** research taste, creative quality and evaluation of directions. Meta-evaluation and auditability-by-construction are only starts.
3. **Stability engineering as a discipline:** a unified theory of when loops converge, oscillate or collapse, replacing per-paradigm folklore about rise-and-collapse, diversity collapse and safety drift.
4. **Trustworthy accumulation:** a verification story for persistent skills, memory and experience graphs. Formal verification [82] and threat analysis [83] bracket the problem; the middle is open.
5. **Governance-grade measurement:** auditable evidence of what a loop is and is not improving, as credible-slowdown proposals require. The authors call this the clearest gap.
6. **Recognizing frame revision:** noticing that an objective has gone stale and reframing it while keeping what should survive. The paper proposes four observables:
   - productive abandonment;
   - invariant transfer;
   - absence-sensitivity (tested against the null-result deficit in [187]);
   - reframing latency.
   It also calls for a benchmark of deliberately stale or misspecified objectives, scored on whether the system notices, and says it is "buildable today and does not exist."
- Smaller items: the paper wants a third axis for trajectory and continuity of method (§2.2). Its four hypotheses about corpus skew are offered as testable with the released data (§8). Its watch signal for takeoff is movement up the hierarchy on non-verifiable tasks, not benchmark scores (§7.3, §9).

**What it misses or gets wrong as of Nov 2026** (compared against the project's knowledge files)
- **Data cutoff (early July 2026) misses the improver-level wave.** The project's FINAL_REPORT describes a mid-2026+ shift from "does the loop raise the product" to "does it raise the improver". None of that literature appears in the survey:
  - Improvement by Inheritance (ZMmJDeDTXk) and Learning to Repair the Learner (aEPJT4hW84), on nested replay and the finding that improvers almost never edit themselves;
  - RSI-Harness, MetaEvoForge, FreeEvolve, Looped-RSI and Meta-TTL, which report cross-domain improver transfer;
  - Opinionated Optimizers (LhhLpXBFgo), RSI-Opt 2610.32026, HMED, VERSE and MetaRSI.
  - The grid has no cell separating improving the product from improving the improver. §3.5 merges Gödel Agent and DGM with one-component evolution.
- **Order-0 vs order-1 is mostly absent.** The survey says self-modification "almost always" touches one sandboxed component. It does not measure *whether self-edits happen or survive selection*, which is the self-edit drought the project studies.
- **Notable omissions in harness evolution.** By grep of the full text there are no mentions of GEPA, Hyperagents, SICA, STOP (Self-Taught Optimizer), ADAS/Meta Agent Search, Promptbreeder or Huxley-Gödel Machine. These are core baselines in the project files.
- **Simulator and world-model scoring is missing.** There is no "simulator" mention and only one passing "world model" mention (in the McNamara-fallacy discussion). The trace-vs-fitness channel split behind Taught, Not Selected is outside its view.
- **Some papers already in project files are missing,** for example 2610.14662's editable-cause share. The ICLR 2027 submissions (Sept–Nov) are also missing: Red Queen is in the project's openreview-iclr27 sweep and in the survey; Improvement Fidelity, SEA, Simulator Says Yes and VHE are in the project files only.
- **Thin agent-benchmark coverage.** It does not discuss MLE-bench, RE-Bench, PaperBench or τ-bench, and it under-covers continual and drift benchmarks. Drift appears only as safety drift [84] and attractor drift [124]. There is no treatment of non-stationary environment or API drift as a cause of self-improvement.
- **A-Evolve-Training gets a lot of weight from one preprint.** The survey itself grounds §6.2 and §8's "escape is possible" in it. That has not been verified in the project files (◐ for any reuse).
- **The "tracks the hierarchy" law is admittedly qualitative,** with no measurement. The 4×3 grid is described rather than counted per cell; Fig. 1 shows only representative systems.
- **Overlap with other surveys in the project reading list** (2606.20683, 2607.13104, 2609.11873, 2608.06216) is not discussed. Two of these postdate it.

**Relevance to Dan's ideas**
- **Taught, Not Selected.** Direct support for the framing, with no scoop. The paper's thesis is that the evaluator is the bottleneck, and it treats refinement as "projecting toward the refiner's own distribution" [27]. It names "the loop transmits bias as efficiently as capability" as a load-bearing fact (§4.5), and says the more privileged the self-generated signal, the more efficiently bias is transmitted (§4.3). Those claims are close to the "taught via traces" hypothesis, but the survey never separates a trace (teaching) channel from a fitness (selection) channel. The hierarchy ranks only the *selection* signal. Good citations: §5.3 failure mode 1, the privilege-induced style drift paper [112], and [151] on traces boxing agents in (§5.3 failure mode 4).
- **Propose / Propagate / Pay-off funnel.** The survey's "substrate not trajectory" exclusion (§2.2) and its "scientific amnesia" diagnosis (behaviors improve while method does not accumulate [17]) are the closest it gets. The funnel adds the missing trajectory measurement. Its §5.5 claim that the durable gain is process-level, as capex, is a testable prediction that Pay-off measures directly. The survey does not measure self-edit proposal rates or selection survival, so the gap is open.
- **Meta-Overfitting.** Related pieces are frame lock-in (§5.3 failure mode 4), "accumulated context as a source of lock-in" [151], and the A-Evolve-Training proxy-decoupling anecdote. The survey does not discuss improver transfer or domain distance, so Meta-Overfitting's "better improver only near home" question is uncovered. Open problem 6's "invariant transfer" observable is close to the procedure-vs-memory split and is worth citing.
- **Meta-reward.** Evaluator co-evolution (§5.1, §8) and Self-Trained Verification's "does it escape circularity or relocate it?" frame the meta-reward question well. The paper raises "a second story" for the self-confirming loop as the pivotal question, which motivates order-0 vs order-1 reward comparisons. It offers no comparison of meta-rewards on successor quality.
- **Openness (the reach × credit law).** The SHARP lesson ("constrain the self-modification surface to keep it verifiable", §3.6) and Liu et al.'s argument that hard-coded procedures are "rigid precisely where they most need flexibility" [59] are the two poles of the openness question. The survey states them but offers no law, which supports the novelty claim of "Open Where the Cause Is". Its observation that "a stationary evaluation criterion" is assumed (Red Queen) matches the round-3 claim that the cause never reaches L4/L5 on stationary benchmarks.
- **Continual / drift.** Relevant pieces are §3.4 TTT and sleep consolidation, the rise-and-collapse result (pass@1 rises then collapses without distribution shift [93]), and open problem 6's "reframing latency", meaning how much evidence is needed to abandon a stale objective after an environment shift. Reframing latency is essentially the continual-drift benchmark's readout. Cite it as the survey explicitly saying such a benchmark "does not exist."
- **Register snowball.** Weak but citable. Attractor states in multi-turn conversations [124 = 2606.30571, already in claudish/REVIEW.md] and diversity collapse in self-play are the closest. Privilege-induced style drift [112] is a training-time cousin, because the learning signal concentrates on style tokens. The survey does not cover human–model conversational amplification.
- **Overall use.** Cite it as the umbrella survey for RSI framing. The verification hierarchy and the four failure modes are useful related-work scaffolding. Its open problems 3, 4 and 6 directly motivate Dan's funnel, openness and drift papers. Its July 2026 cutoff means every improver-level and harness-evolution comparison must come from the project's own later sources.

---

### Self-Improvements in Modern Agentic Systems: A Survey (2607.13104)

**Read status: ✓ full text.** I read the PDF text of all of §1–§10 and the notation appendix (97 pp. in total; about 57 pp. are body). I only skimmed the reference list and the dense figure tables (Fig. 3 taxonomy tree, Table 4 memory matrix, Table 5 applications, Fig. 12 benchmark-incidence matrix). Text is cached at `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/2607.13104/plain.txt`, with the layout version in `paper.txt` in the same folder.

**Bibliographic**
- **Authors:** Zhe Ren, Yimeng Chen*, Dandan Guo*, Guowei Rong, Tonghui Li, R.B. Xiong, Qingfeng Lan, Wenyi Wang, Li Nanbo, Yibo Yang, Mingchen Zhuge, Jürgen Schmidhuber (* = corresponding).
- **Affiliations:** Jilin University, KAUST, independent researcher, University of Alberta, IDSIA/USI/SUPSI. This is in effect the Schmidhuber/Zhuge KAUST group.
- **Venue and date:** arXiv cs.AI preprint v1, 14 Jul 2026. No venue is stated. It comes with a GitHub "Self-Improving-Agents" tracking page.
- **Length:** 97 pp. including references and appendix.
- **Link:** https://arxiv.org/abs/2607.13104

**Scope & definitions**
- **Scope.** The survey covers foundation-model (FM)-based agents only. Self-improvement is framed as "controllable evolution, or adaptation, from experience with minimal or even no human input" (Abstract ✓).
- **Agent formalism (§3.1 ✓).**
  - An agent is A_t = (θ_t, Σ_t), where θ is the FM weights and Σ_t = (p_t, m_t, T_t, g_t) is the scaffold: prompts, memory, tools, and control logic (routing, scheduling, safety).
  - The ephemeral execution state X_t (KV cache, plans, working memory) is explicitly *excluded*. Accumulating in-context history is not self-improvement.
  - They prefer "scaffold" over "harness" "to emphasize the modifiable structures" (§1 ✓).
- **Self-improvement, SI-FMA (§3.2 ✓, Eq. 4).**
  - Defined as a self-induced operator A_{t+1} = U(A_{1:t}, E(π_{θt,Σt}; Σ_t, C_t)).
  - E is an agent-executed procedure that produces a learning signal. Σ is passed in so the agent can inspect itself.
  - U "commits durable changes" beyond task boundaries. These can be undone (rollback through the history A_{1:t}).
- **Two modes of self-reference (§3.2 ✓).**
  - *Distributional*: the agent's behaviour produces the data or supervision that an external optimizer uses to update θ.
  - *Action-level*: the agent's actions directly edit Σ.
  - The first maps to FM improvement (Eq. 5, Σ fixed). The second maps to scaffolding improvement (Eq. 6, θ fixed).
- **Skill as a reusable update (§3.2 ✓).** A skill is "a named update to the agent's own configuration … that it retains and reuses", serialized into any substrate (T, p, m, θ, g). It is therefore orthogonal to the substrate axis.
  - *Object-level* skills act on the world (like options in hierarchical RL).
  - *Meta-level* skills edit A_t itself. For these, "the operator can become part of its own operand", which recovers the Schmidhuber-style self-referential loop.
- **Relations to other paradigms (§3.3 ✓).**
  - *RL:* θ updates are standard policy optimization. Σ updates "reshape the MDP itself" (state, action and observation processing) and have no RL counterpart.
  - *Online learning:* θ inherits forgetting and shift. Σ externalizes adaptation but "does not eliminate forgetting", and it adds memory poisoning, tool-semantic drift and template brittleness.
  - *Active learning / artificial curiosity:* the agent picks its own queries and experiments.
- **History (§2 ✓).** A long Schmidhuber-centric lineage, through to in-context learning ≈ fast-weight programmers and the modern nested fast/slow loops:
  - Gauss/least squares, cybernetics, Turing's child machine, Gödel numbering, Good's intelligence explosion
  - von Neumann self-reproduction, Lenat's AM/EURISKO
  - self-referential learning (1987), fast weights / self-referential weight matrices, the success-story algorithm
  - Gödel Machine (2003), Orseau & Ring delusion risks, NAS, self-play
  - A noted historical lesson: EURISKO's success depended on a human acting as the external evaluator. The authors present this as the persisting need for robust internal evaluation.

**Taxonomy** (primary axis: *what* is updated; secondary axis: *signal form* for θ, *component* for Σ; Figs 1–3, §4–§6 ✓)

1. **Foundation-model improvement (θ; §5).** Slow, stable, amortized; Σ is fixed (Alg. 1). Signal forms:
   - **5.1 Intrinsic generative demonstrations (S ≈ D_t).** The model synthesizes its own training data, optionally filtered by Φ_t.
     - Representatives: Self-Instruct, LMSI, Orca, SELF, Self-Guide, MoAA, SICOG, TaskCraft, AReaL-SEA, RENT, LADDER, DIVE, SEAL, MM-UPT, SPORT, EvoGround, SCPO, VeriFree, SAGE, RAGShaper, ANDES.
     - Sub-dimensions:
       - generation strategy: bootstrapping (Evol-Instruct), self-consistency filtering (LMSI), verifier filtering (ReST-EM), curriculum by decomposition (LADDER), diversity-aware pool expansion (DIVE), test-time self-improvement (TT-SI: uncertainty → targeted LoRA)
       - data format: instruction pairs, CoT, structured trajectories, multimodal data, metacognitive artifacts (problem trees, SEAL self-edits)
       - safeguards
   - **5.2 Intrinsic evaluative feedback (S ≈ e_t).** Three families:
     - *Rubric:* Constitutional AI, Meta-Rewarding, judges without references (Simonds et al.), Self-Evolved Reward Learning.
     - *Consistency:* TTRL majority vote, SRT, EMPO entropy, INTUITOR self-certainty.
     - *Corrective critique-and-revision:* ReST-meets-ReAct, SELF, RISE, Reflect-Retry-Reward, AlphaLLM.
     - Also listed: Strive, SCR, SER, UniCorn, RePro, iReasoner, DyMo, SEC, SRSI, PCL-lite, EVE-Agent.
   - **5.3 Extrinsic exploratory experience (S ≈ τ_t).**
     - *5.3.1 Grounded environments*, split by feedback source:
       - programmatic verifiers (Agent-RLVR)
       - learned reward models (WebRL, UI-Genie, MobileGUI-RL)
       - self-generated tasks with extrinsic acceptance (Absolute Zero, ETO)
       - platforms (AgentGym)
       - also: RoboCat, Tool-Star, CodeARC, ARTIST, Tool-R0, DeepResearcher, Kevin-32B, SEAgent, RAGEN, SOAR, WebGym, Q-Evolve, Socratic-SWE
     - *5.3.2 Simulated proxy / world-model environments:* WebEvolver, WebSynthesis, WebDreamer, SPA, WMPO, GLoW, WMA, GAWM, AlignUSER, EvoWorld, NavMorph, WorldEvolver, RISE.
2. **Scaffolding improvement (Σ; §6).** Fast and reversible; θ is frozen (Alg. 2). Ordered by "increasing depth of architectural intervention":
   - **6.1 Prompt** (four paradigms by signal richness; Table 2):
     - (1) Scalar-feedback search: APE, OPRO, RLPrompt, InstructZero, BPO, PROMST, MAPS, SePO.
     - (2) Qualitative-feedback refinement: Self-Refine, debate, Reflexion, CoH, MAPS, ACE, Scrable, APOHF, CriSPO, MIPRO, FORGE.
     - (3) Population evolution: EvoPrompt, Evol-Instruct, Promptbreeder (which evolves its own mutation prompts), STOP, GPTSwarm, DEEVO, DelvePO, EGO-Prompt, GEPA as a hybrid.
     - (4) Textual gradients: APO, TextGrad, OPTO, semantic backprop/GASO, metaTextGrad (which optimizes the optimizer prompts), MAPGD, TPGO, TSGD-M, STEVE, SkillOpt, VASO.
   - **6.2 Memory**, as m = (object, structure) plus processing:
     - *Objects.* Explicit: processed trails (AWM, ReasoningBank, ExpeL), curated raw content (Dynamic Cheatsheet), integrated external knowledge (PRIME, CodeAgent). Implicit/latent: MemGen, CMR, KV-cache coprocessors, MemoryLLM, M+.
     - *Structure:*
       - flat: SCM, Self-Notes
       - hierarchical: MobileGPT, H-MEM, SHIMI, SALM, XMem, MovieChat
       - graph: Mem0, SGMem, Zep, CausalRAG, G-Memory, GraphVideoAgent
       - vector: Agentic RAG, Generative Agents, RMM, MemoryBank, CTIM-Rover, MIRIX, MrSteve
     - *Processing* as signal-driven CRUD:
       - Create: compression, ADD/UPDATE/DELETE/NOOP, controlled insertion
       - Read: hybrid scoring, structure-aware retrieval, gating, case retrieval
       - Update: scheduled decay, local refresh, iterative distillation (DC, ACE), offline aggregation
       - Delete: multi-stage pruning, consensus eviction, tiered OS-style eviction (MemoryOS)
     - The pieces are closed into a 7-step "signal-driven memory loop".
   - **6.3 Tool ("Tool Governance Metacognition")**:
     - (1) Dynamic tool routing:
       - retrieval or graph based: MemTool, TAR, Voyager/MetaAgent, ToolNet, OrchDAG, MassTool
       - policy learning: AutoAct, MCP-Flow, Tool-Star, DeepEyesV2, AgentFlow, SPORT, AutoTIR, DeepAgent, ToolGen
       - proactive/interactive: MCP-Zero, AskToAct, Tool-Planner, ToolACE-R
     - (2) Iterative tool refinement: Voyager loop, STELLA critic, SkillWeaver, PyVision, DRAFT (fixes the docs, not the code), RewardHarness, MUSE-Autoskill, CodeSkill.
     - (3) Autonomous tool creation, by trigger (ATLASS/PyVision on demand vs FRIDAY/STELLA by curiosity), by lifecycle (ToolMaker), and by standardized MCP integration (Alita, Alita-G, Code2MCP, AgentOrchestra). Also CoEvoSkills, OpenSkill, EvoDS.
   - **6.4 Full scaffolding.** The whole codebase is mutable and the improver is itself part of Σ: Σ_{t+1} = I_{Σt}(Σ_t; S_t). Updates are a patch Δ_t gated by a verifier V.
     - Discussed in text: AlphaEvolve, ShinkaEvolve, ADAS, EvoFlow, STOP, Agent Symbolic Learning, Gödel Agent, DGM, HGM (CMP metric), Live-SWE-Agent.
     - Listed only in Fig. 3: GPTSwarm, Auto-Harness, Continual Harness, AgentDevel, JudgeFlow, RSEA, RoboPhD, MOSS, Red Queen Gödel Machine.
3. **Evaluation (§8):**
   - Measuring: metric-based Φ_metric (AgentGym, CORE-Bench, SWE-bench+, GAIA, etc.) vs judge-based Φ_judge (Agent-as-a-Judge, EvalAgents, ARJudge, VerifiAgent).
   - Benchmarking: mechanism benchmarks, FM-level (SWE-bench+, GitTaskBench, Risks of LM Agents) vs scaffold-level (DrunkAgent, BFCL-type tool eval, TaskBench, MetaTool, MINT); and domain benchmarks across 6 domains.
4. **Applications (§7, Table 5):** software engineering, web, games, science, embodied AI, general computer control. Each domain is split into an evaluation substrate, Σ-improvement and θ-improvement work, plus open challenges.

**Key claims & findings** (all ✓)
- **Natural language shrinks the search space.** Language is the unlock that makes old self-referential ideas scale, because it "drastically reduc[es] the search space for viable modifications" compared with assembly-like code or raw weights (§1).
- **Fast vs slow loop (§1, §3.2, §9.1).** θ improvement is slow, global, stable and costly. Σ improvement is fast, reversible and context-dependent. Recommendation: under noisy feedback, keep updates in Σ and gate them by execution tests. Defer parametric consolidation until the behaviour is stable. Consolidation is "a lossy compression" that biases toward the average case, discards rare error-recovery strategies, and "invalidates prior safety bounds".
- **Σ updates reshape the MDP.** They change the effective state and action spaces, which is why Σ improvement has no classical RL analogue (§3.3).
- **Signal fragility, intrinsic feedback (§5.1–5.2).**
  - Self-consistency fails when the model is "confidently wrong".
  - Decomposition curricula can drop task constraints.
  - Errors outside the model's perception get *amplified*.
  - The evaluator is tightly coupled with the policy, so blind spots get reinforced.
  - Remedies: separate generator and evaluator checkpoints or families, external anchors, disagreement as an uncertainty signal, recurring rubric audits.
  - For collapse: accumulate rather than replace data (Gerstgrasser et al.).
- **Failure modes specific to extrinsic experience (§5.3.2).**
  - "Reward hacking through language": the verifier's spec is itself a linguistic object the agent can manipulate.
  - Capability regression from narrow RL.
  - Hallucinated world-model dynamics that the policy exploits.
  - Context-length tension for long trajectories.
- **Prompt-optimization trend (§6.1, Table 2).** Signals get richer (scalar → critique → population → textual gradient), so updates become "less heuristic and more targeted". Meta-level variants that evolve the optimizer's own prompts (Promptbreeder, metaTextGrad) are flagged as self-referential.
- **Memory trade-offs (Table 3 ✓, an ordinal scorecard by the authors, not measured).** Explicit memory is interpretable but scales poorly. Latent memory is compact but drifts and is hard to audit. Typical failures: summary bias, stale heuristics, retrieval noise, privacy leakage, silent corruption.
- **Tools.** Unreliable stored tools "corrupt the agent's future behavior". Refinement is therefore also a *gatekeeping* step for the skill repository (§6.3.2).
- **Full scaffolding (§6.4).** It "naturally subsume[s] component-level edits while adding archive-based exploration and stronger acceptance tests" (§6). Current systems realize recursive self-improvement only "as bounded, verifiable loops" inside human-set objectives.
- **Software engineering (§7.1).**
  - SWE-agent and Agentless are *not* self-improving under this definition (no persistent cross-task update).
  - Scaffold improvement may overfit to benchmark tooling or repo conventions.
  - The authors recommend benchmark mutation or stress testing, and hybrid loops that distill scaffold discoveries into θ "only when they transfer beyond a fixed verifier or benchmark protocol".
- **Web, games and science (§7.2–7.4).**
  - Web gains on static snapshots decay as sites drift, so drift-aware evaluation is needed.
  - Self-play gives "brittle competence" and non-transitive cycling, so population-based evaluation is needed.
  - In science, the system cannot verify novelty or correctness itself, and apparent gains may be exploited weak proxies.
- **Critic as governed infrastructure (§9.1).** "The ceiling of an agent's capability is usually bottlenecked by the critic's exploit-resistance." If one agent both proposes and accepts updates, "it collapses into a self-confirming loop." Critics should be decoupled from generators. Evolving critics should be restricted to *monotone* changes (for example additive test generation) with human audit trails.
- **Layered gating for safety (§9.1).** Treat a self-improving agent as "untrusted code executing in a protected runtime". One-off exploits such as prompt injection become *persistent* once committed into Σ (poisoned memory, hijacked tools). Every update to Σ or θ must pass verifier gates covering correctness, tool permissions and robustness to state perturbation.

**Evaluation practices it describes or criticizes** (§8 ✓)
- **Formal target.** The quantity measured is a trajectory m_t = E_{x∼D_eval}[Φ(x, τ)] over iterations under a cumulative budget b_t ≤ B_max, not a terminal score.
- **Criticisms of current practice:**
  - reporting the peak instead of the full learning curve
  - no stated checkpoints, acceptance criteria or early stopping, which makes unbounded iteration overfit the eval distribution
  - single seeds, even though trajectories (memory, patches) are highly initialization-sensitive
  - evaluating on the optimization data
  - no cost breakdown
  - unquantified human-in-the-loop help, which "fundamentally compromises the 'self'"
  - reporting mean success only, without regression, tail-risk or safety-violation tracking
- **Recommended reporting set:**
  - baseline performance
  - performance after a fixed budget
  - learning curves
  - held-out transfer, using hidden test sets or tasks dated after the model's cutoff
  - regression rate on previously solved tasks
  - full cost (compute, tool calls, time, human input)
- **Judge-based evaluation:**
  - Disclose the judge model version, prompt, rubric and evidence shown.
  - Keep the judging budget separate from the agent budget, otherwise you cannot tell whether "the evaluator merely became more exhaustive".
  - Enforce evaluator independence: a different judge or an orthogonal rubric κ′ for final reporting than the one that drove updates.
  - Report variance, multi-judge aggregation, and calibration against a verifiable subset.
- **Mechanism benchmarks:**
  - FM-level: provenance tracking, compartmentalized data, retention audits.
  - Scaffold-level: isolate one component.
    - Prompts: test under paraphrase, format and length shifts.
    - Memory: long-horizon recall, poisoning, disclosure, forgetting failures.
    - Tools: invocation, selection and argument grounding.
  - Attribution: ablate the updated component, replay with the environment fixed and modules swapped, and check regressions.
- **Domain protocols.** For each domain, state the feedback signal, the permitted channel (θ or Σ), the budget, and the held-out sets. Specific advice: games need cross-play and diverse opponents; web needs randomization or controlled sites; embodied AI needs safety-violation tracking.
- **Note:** this is prescriptive. The survey runs no meta-analysis and quantifies no existing paper's compliance. Fig. 12 is only a paper × benchmark incidence matrix.

**Open problems / future directions it lists** (§9.2 ✓, complete)

*Theme A, algorithmic paradigms for lifelong adaptation:*
1. **Test-time continual adaptation.** Update retrieval, routing and memory policies during deployment without slowly eroding global performance.
2. **Active exploration and curiosity.** Intrinsic value for high prediction error or verifier disagreement, while avoiding "a degenerate, self-deceptive cycle".
3. **Parametric distillation and joint optimization.**
   - Distill "System 2" scaffold strategies into "System 1" weights of smaller models.
   - Co-adapt θ and Σ in one loop (cites SIA 2605.27276).
   - Key problem: credit assignment over whether a failure should be fixed by a prompt edit, a tool rewrite or a gradient step.

*Theme B, complexity, constraints and open-world robustness:*

4. **Resource-constrained improvement dynamics.** Treat self-improvement as resource optimization: improvement *efficiency*, cheap invariant checks before expensive evaluation, penalties for wasted iterations.
5. **Multi-agent cooperative co-evolution.** Share artifacts (regression tests, patches, tool wrappers) through secure, version-controlled repositories, with no single point of failure.
6. **Surviving open-world distribution drift.** Replace static leaderboards with non-stationary simulators where interfaces drift (cites Froger et al. 2026). At the substrate level, "neural computers": learned runtimes that unify compute, memory and I/O.

*Cross-cutting items from §9.1 and §10:* scalable and secure evaluation, alignment of a non-stationary object (a weaker system reasoning about stronger successors), and integration of the fast and slow loops.

**What it misses or gets wrong as of Nov 2026** (compared against the project files)
- **The cutoff explains part of the gap, but there is also a real blind spot.** There is no treatment of whether the *improver itself* improves.
  - §6.4 asserts "the improver can evolve together with the agent it improves", and Fig. 1 shows a rising capability curve.
  - There is no discussion of improver-edit rates, inheritance versus self-modification, or order-0 vs order-1 gains.
  - Since then: Improvement by Inheritance (ZMmJDeDTXk) shows lineage gains are mostly inheritance, improvers edit themselves in 1–15% of generations, and archive best-so-far curves sit inside a permutation null band at 119 of 141 checkpoints. FULLTEXT_VERIFICATION finds no system reports a proposed → retained → effective funnel.
  - The survey's evaluation section accepts archive and learning curves at face value.
- **Hyperagents / DGM-H (2603.19461, March 2026) is not cited at all**, despite heavy Gödel-family coverage (DGM, HGM, Gödel Agent, RQGM). HGM's clade metaproductivity is presented as an approximation to the Gödel ideal. IBI's Prop 7 shows CMP ≠ successor quality.
- **Gating is described as if systems really gate.** §6.4 has a verifier V gate the patch. Per FULLTEXT_VERIFICATION, DGM, Hyperagents and HGM keep every child that compiles, and among 17 systems only HMED scores and can reject an improver edit. The survey's "Σ̃ accepted if V passes" picture overstates how published systems actually select.
- **Internal inconsistency on forgetting.** §4.2 says scaffold improvement yields adaptation "without the risks of catastrophic forgetting". §3.3 says externalizing "does not eliminate forgetting". Project evidence supports §3.3: harness-level forgetting is shown by 2611.03219 (continual harness evolution, 6 directions) and Beneath the Surface 2611.07624 (every champion gains some tasks and loses others).
- **"Deeper intervention is better" is implicit.** Full scaffolding is called "the most profound level" and said to "subsume" component edits. There is no evidence or discussion that a larger editable surface can hurt through selection noise, dilution or the winner's curse. Project material that bears on this:
  - Opinionated Optimizers (LhhLpXBFgo): selection rarely compounds.
  - QDdWiaNkk2 / 2607.12227: near-zero real gains for harness evolution.
  - Taken together, openness is not monotone.
- **Transfer and meta-transfer are underdeveloped.** The survey asks for held-out transfer of the *product*. It never raises transfer of an evolved *improver* across domains. The later debate (RSI-Harness up to 4.5× improver transfer, MetaEvoForge, FreeEvolve, Hyperagents imp@50 = 0.630 vs Meta-Overfitting) is absent. Partly a cutoff issue, but Hyperagents predates the survey.
- **Simulator-scored scaffold evolution is not covered.** World models appear only for θ updates (§5.3.2), and the risk is framed as policy *exploitation*, i.e. a selection channel. The trace and teaching channel (the reflector learning from simulated traces) is never discussed.
- **Drift is a future direction only.** There is no benchmark for drift of the *improvement regime* (procedure or verifier drift, as opposed to task content). Continual Harness (2605.09998) and RQGM (2606.26294, co-evolving evaluators) appear only as Fig. 3 nodes with no textual analysis.
- **Missing from the 2026 literature (mostly post-cutoff):** SelfImprove-Bench 2611.02797 (improvers rarely self-modify; generalization ladder G0–G4), Beneath the Surface, Metaⁿ, HMED, VERSE, SICA (cited once in §9 but not analysed), Learning to Repair the Learner (aEPJT4hW84), and the safety and trustworthy-RSI surveys 2610.00093 and 2610.35193. Also uncovered: the R-Zero/Agent0 style challenger–solver self-play, and Self-Rewarding/SPIN.
- **Minor citation issues (my reading; not fact-checked further).**
  - "Self-improving agents are closed-loop dynamical systems (Xie et al., 2024)" (§9) cites OSWorld.
  - The full-scaffolding safety risk claim cites AgentGym (Xi et al., 2024).
  - Neither supports the claim it is attached to.
  - Table 3 is the authors' qualitative 1–5 scorecard and is labelled as not benchmark-based.
- **No numbers.** The survey reports no quantitative synthesis such as effect sizes or reproduction rates.

**Relevance to Dan's ideas**
- **Taught, Not Selected.**
  - *Foil text:* the survey frames simulator and judge risk purely as *selection* exploitation: "hallucinated dynamics… the policy then learns to exploit" (§5.3.2), and over-optimization to Φ_judge (§8.1.2).
  - *Support for a trace channel:* §5.1 warns that errors outside the model's perception make improvement "exacerbate these errors", and Table 2 lists "critique can be noisy / may drift / validator-dependent" for reflective prompt methods. TNS can cite both as unexamined acknowledgements of a teaching channel.
  - *Gap:* the survey never puts GEPA-style reflective Σ evolution inside a world model (it treats world models only for θ).
  - *Framing anchor:* §9.1's "critic exploit-resistance is the ceiling" claim. TNS's prediction is that the ceiling is the *traces*, not the critic.
- **Propose / Propagate / Pay-off funnel.**
  - The survey supplies the vocabulary: the Σ̃ = Σ ⊕ Δ patch, the verifier gate V, rollback via Σ_{1:t}, and "layered gating".
  - It recommends attribution and regression checks (§8.2.1) but proposes no edit-level funnel. Its recommended reporting list has no proposal, retention or effect rates.
  - The funnel fits directly as an extension of its §8.1.1 reporting checklist.
  - Its claim that full-scaffolding systems gate with a verifier is the assumption the funnel tests; the project's evidence is that most keep everything valid.
- **Meta-Overfitting.**
  - The SWE synthesis concedes scaffold improvement "may overfit to benchmark tooling, repository conventions" and calls for "benchmark mutation and stress testing" (§7.1). That is product-level overfitting.
  - The meta-level version (does the evolved improver generalize?) is absent, even though the survey's own "meta-level skill" concept (§3.2) defines exactly the object Meta-Overfitting studies.
  - Useful citation: "a meta-level skill both acts on A_t and is itself serialized back into A_t".
- **Meta-reward.**
  - §9.1 is the closest statement in a survey: critics decoupled from generators, the agent never in unconstrained control of its critic's updates, evolving critics limited to monotone, additive changes with audits.
  - This matches round 3's design rule that "the meta-reward computation must sit outside every editable surface".
  - Meta-Rewarding (Wu et al.) appears only as an FM-level rubric method.
  - The survey does not discuss credit for edits with delayed payoff, which is the κ term.
- **Openness.**
  - Σ = (p, m, T, g) ordered by "depth of architectural intervention" is essentially the editable-surface ladder, minus the improver and evaluator rungs (L4/L5).
  - The survey gives no law and no evidence about how gain varies with openness. It implicitly assumes deeper is better.
  - The §9.2 joint-optimization credit-assignment problem ("refine a prompt, rewrite a tool wrapper, or compute a gradient") is almost verbatim the "reach × credit by rung" question. Cite it as the open problem the openness law answers.
- **Continual / drift.**
  - Future directions #1 (test-time continual adaptation) and #6 (non-stationary simulators with drifting interfaces) directly motivate a reality-grounded drift benchmark.
  - The web section asks for "drift-aware evaluations".
  - Its drift is task-content and interface drift only. Improvement-regime and verifier drift, the round 3 crossover, is not anticipated.
  - Also cite its online-learning paragraph: Σ adaptation introduces "drift in tool semantics" and "memory poisoning".
- **Register snowball (Claudish).**
  - Nothing on linguistic register drift. The closest material:
    - model collapse and "knowledge bubbles" from recursive self-training (§5.1)
    - ACE's "brevity bias and context collapse"
    - "summary bias" in memory (Table 3)
    - "reward hacking through language", where the verifier spec is itself a linguistic object (§5.3.2)
  - *Testable assumption:* the survey repeatedly sells explicit Σ artifacts (prompts, memory, playbooks) as "human-readable", "inspectable" and auditable (§3.3, §6.2.1). The snowball hypothesis predicts that self-evolved prompts and memory drift toward a register that models read fine but humans find harder to audit. That would undercut the survey's interpretability argument for scaffold over weights. This is a natural motivating citation for an "evolved-artifact legibility" measurement.

---

### When, Where, and How Experience Becomes Structure: Mechanisms for Recursive Self-Improvement in LLM Agents (2611.recursive-self-improvement-llm-agents)

**Read status: ✓ full text.** I pulled it with alphaXiv `get_paper_content` (fullText:true), about 2,340 lines, and read every section from §1 to §11, plus the figures, tables and references. arXiv has no PDF for it because it has no numeric arXiv ID. Some figure text was garbled in extraction, in Fig. 4 and at the bottom of Fig. 5. Saved text: /root/.claude/projects/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/tool-results/mcp-alphaXiv-get_paper_content-1795043916609-kdkf52.txt

**Bibliographic**
- **Authors:** Siqi Dai and Jingqing Ruan (equal contribution; Ruan is project lead), Bin Zhang (corresponding author), Zhiwei Xu, Lin Cheng, Ke Zeng, Zhongda Su, Feng Hong, Nan Jia, Wenxuan Shi, Dapeng Li, Chuanchen Luo, Yiqun Chen, Zongyang Ma, Xue Yan, Yang Li, Mingzhe Xing, Hangyu Mao, Bowei Dai, Zhizhao Zeng and Guoliang Fan.
- **Organizations:** CASIA (Institute of Automation, CAS) and Meituan lead. Others: Shandong U, Renmin U, IME-CAS, SJTU, NTU, Li Auto, HUST, ICT-CAS.
- **Date and venue:** 2026-11-04. It is a preprint indexed only on alphaXiv, with no arXiv number and no Semantic Scholar entry (that was round-2 status; I did not re-check). No venue is stated.
- **Length:** about 32 pages of body text plus about 10 pages of references (42 pages in total). It has 9 figures and 8 tables. It runs no experiments.
- **Link:** https://www.alphaxiv.org/abs/2611.recursive-self-improvement-llm-agents. The link https://arxiv.org/abs/<id> does not exist for this paper.

**Scope & definitions**
- **RSI.** "the process by which AI systems learn from their experience to improve themselves, with each improvement potentially enabling further improvements" (§1, citing Chen 2026b = 2607.07663, Duan 2026 = 2609.11873, and Wu 2026a).
- **Scope (§2.2).** The survey covers only non-parametric updates: the task executor's weights stay frozen, though the updater itself may be trained. "Structure" in the title means four kinds of editable external state, Prompt, Memory, Skill and Harness, plus what they retain.
- **Components are defined by how they act at execution time, not by storage format** (§3.1, Table 1):
  - Prompt: direct context conditioning.
  - Memory: addressable records reached by selective recall.
  - Skill: procedures the agent selects and invokes.
  - Harness: runtime support, orchestration and governance (context assembly, tool dispatch, verification, permissions).
  - The same text can count as any of the four depending on how it is used.
- **Five comparison dimensions (§3.2):** Persistence, Proceduralization, Enforcement (whether use relies on the model complying or is imposed by the system), Editability, and Governance burden.
- **Three patterns of what survives an update (Fig. 3):**
  - (i) answer refinement: nothing is inherited;
  - (ii) persistent experience: a retained artifact is reused on later tasks (ExpeL, Voyager);
  - (iii) improver update: the process that produces updates is itself represented and revised (SePO, MemSkill).
  - Key line: "Reuse in later tasks alone does not establish that recursive dependency" (§2.2).
- **Organizing questions:**
  - When: what evidence triggers an update, justifies keeping it, or calls for revising or retiring it.
  - Where: which component, or which coordinated set of components.
  - How: how the update is constructed, verified and consolidated.
- **Positioning against other surveys (§2.3).** The survey sets itself apart from:
  - Gao 2025 (what/when/how/where to evolve, 2507.21046);
  - Fang 2025a (2508.07407);
  - Jiang 2025 (adaptation survey, 2512.16301);
  - Jiang 2026a (Era-of-Experience self→meta-evolution survey, cited as an "unpublished manuscript");
  - the broad RSI surveys Chen 2607.07663, Duan 2609.11873 and Wu 2026a;
  - the co-evolution surveys Ren 2607.13104 and Zong 2608.10299.
  
  Its claimed difference is that its unit of comparison is the operations *inside* one update loop.

**Taxonomy**

A. Seven analytical functions. These are not mandatory and need not run in order; methods may combine, revisit or omit them (§4.1).
1. **Collect:** traces and feedback.
2. **Evaluate:** assess an *existing* execution. This is distinct from Verify, which tests a *candidate* change.
3. **Diagnose:** locate causes and targets.
4. **Abstract:** derive a reusable fact, rule or procedure.
5. **Update:** build the candidate change.
6. **Verify:** test effects and regressions.
7. **Consolidate:** select and retain, including for use in later updates.

B. Four components, with representative work from the Fig. 4 landscape (40 studies) and Tables 2–5.
- **Prompt (§5): instructions, examples, proposer state.**
  - Score-driven: APE, OPRO, EvoPrompt.
  - Critique- or trace-driven: ProTeGi, PromptAgent (MCTS), TextGrad, GEPA.
  - Joint instruction and demo search: MIPRO.
  - The proposer itself evolves: Promptbreeder (mutation prompts), SePO (two stages: evolve the optimizer prompt, freeze it, then optimize the target), RHI (2607.15524, pairwise feedback over loop-spec versions).
- **Memory (§6): records, relations, recall policy.**
  - Formation: A-MEM (linked notes), H-EPM (tool-transition graph), ExpeL (insights go to the Prompt, cases go to Memory), ReasoningBank (LLM-judged strategies and lessons), RSIAgent (2609.15364; proposer, executor and verifier exploration, then Memory is frozen).
  - Revision: Mem0 (add/update/delete/no-op), Zep (temporal validity intervals).
  - Retrieval and utility: MemGPT, MemRL (learned per-record utility), Proactive Memory Agent.
  - Learning how memory is managed: MemSkill (an evolving operator bank), Memory-R1 (RL), Recuris (2608.24876; a meta-agent patches the skill, working-state spec, selector or checker under a dev-set regression gate, with a fixed outer process).
- **Skill (§7): procedures, contracts, repertoire.**
  - Formation: Voyager, SkillWeaver, XSkill, Trace2Skill, Ctx2Skill (from documents), AutoSkill (from user queries).
  - Revision: SkillForge (chronological ticket replay), SkillOpt (separate selection and test sets), MemSkill, CoEvoSkills (the skill and its tests co-evolve), Skills-Coach (practice cases).
  - Library maintenance: SkillNet, SkillX, Memento-Skills, SkillReducer, SLIM (leave-one-skill-out retire/retain).
  - Learned curators combined with training: SkillRL (also trains the policy), SkillOS (trained curator, frozen executor).
  - Evaluation: SkillsBench, SkillLearnBench, SWE-Skills-Bench, SKILL-INJECT, SkillGuardBench.
- **Harness (§8): interfaces, workflow, tool rules.**
  - Starting points: HarnessDev (separates construction from evolution), Self-Harness (minimal seed), HarnessPro (Codex and Claude Code as seeds).
  - Linking failure to an edit: HarnessFix, ModularRSI (contrasting trajectories, module-level edits), AHE (the edit is tried, then kept or reverted the next round), AutoHarness (checker code).
  - Proposer roles: Self-Harness, Meta-Harness (filesystem of past code, scores and traces, plus a Pareto frontier), Harness-R1 and Harness Learning (trained proposer).
  - Per-task versus cross-task: JIT-Agent, Continual Harness, TTHE (label-free), MemoHarness, Harness² (2610.14224; an improvement library feeds later edits).
  - Selection: HarnessPro (performance–cost Pareto), RRSI (2609.24972; edit caps, gain/regression history, screening for task-specific content), the compute-matched evaluation study 2607.12227.
  - Improver self-modification: VERSE (2610.02616), Hyperagents (2603.19461), Harness-Zero (distilled into weights).
  - Fig. 4 only (no prose): DGM, Gödel Agent, ADAS, SEW, HarnessX.

C. Twenty update loops mapped onto the seven functions (Fig. 5, §4.2)
- Prompt: OPRO, ProTeGi, MIPRO, GEPA, SePO.
- Memory: A-MEM, Mem0, MemSkill ×2 (record writes; operator bank), Recuris, ReasoningBank, MemRL.
- Skill: AutoSkill, SkillRL, SLIM, SkillForge, SkillOpt.
- Harness: AutoHarness, Meta-Harness, Self-Harness, AHE, DGM.
- Entries marked NR (not reported) show where the gaps are. **Verify is NR for A-MEM, Mem0, MemSkill (record writes), AutoSkill, SkillRL, SkillForge, ReasoningBank and MemRL.** **Diagnose is NR for OPRO, MIPRO, A-MEM and MemSkill (record writes).** A rough count is mine; the rest is my reading of Fig. 5.
- Fig. 5 contradicts itself: it says both "19 papers / 20 loops" and "20 papers / 20 loops". MemSkill is counted twice, so 19 papers is the correct figure.

D. Re-update and RSI axis (§9): what triggers a re-update, coordinated updates, retain/revise/retire decisions, and changes to the improvement process.

**Key claims & findings** (qualitative synthesis; no numbers are reported)
- **Execution feedback underdetermines the edit target.** "execution feedback does not directly specify which component should change" (Abstract). The same API-schema failure can be fixed in the Prompt, Memory, Skill or Harness, and each fix has different scope and maintenance needs (§1, §3.2, Figs. 6–9).
- **Evaluate and Verify are different functions** (§4.1). Many Memory and Skill loops have no per-item Verify before insertion, including ReasoningBank, SkillRL, Mem0 and AutoSkill (§4.2, §6.3, §7.4).
- **Trial-then-revert is a separate verification style from pre-use gating:** AHE versus Self-Harness (§9.3).
- **Candidate selection is not a final test.** GEPA's minibatch and pool scores, and Self-Harness's use of held-out scores during acceptance, mean those scores are "not ... a fully independent final test" (§5.3, §8.3, §10.1).
- **Attribution is confounded** when a learned selector, curator or policy also changes, as in SkillRL, SkillOS, Memory-R1 and Harness-Zero. Gains should not all be credited to the external component (§7.4, §9.2).
- **Repeated updates do not show that the update method improved** (§7.4, §8.6, §9.4). Harness² changes the library available to later edits, but its "stage instructions and decision rules remain fixed". VERSE also modifies the improver's own prompts, skills, tools, hooks and notes, and "execution-based checks matter for the benefit of optimizer self-evolution ... rather than showing that unrestricted self-editing is sufficient" (§8.6). SePO's evolved proposer is frozen in its second stage (§9.4).
- **Compute-matched comparison (2607.12227, §8.5).** On the Terminal-Bench settings tested, harness evolution "does not consistently outperform" parallel sampling or sequential refinement under matched budgets, and transfers only to a limited extent.
- **Skills often do not help.** SWE-Skills-Bench found many skills left pass rate unchanged and some increased token use (§7.5).
- **Gains are often unstable.** HarnessDev reports that later revisions are "not always stable or transferable" (§8.1).
- **Coordinated updates should follow real dependencies**, not mere co-location in one system (§9.2, Table 6).
- **Retirement must cover rare but important uses** (SLIM). Outdated facts may be kept with validity intervals rather than deleted (Zep) (§9.3).
- **Security.** Malicious Memory or Skill content can spread into later update generation (§10.3).

**Evaluation practices it describes or criticizes**
- **Table 7 comparison agenda:**
  - update effect: before/after on the same tasks;
  - component contribution: with/without the component;
  - use across tasks: new tasks, models or tools;
  - repeated updates: later updates with and without retained experience;
  - improvement method: "different versions from the same starting point".
- **Recommended reporting:**
  - which feedback generates edits, which selects candidates, and which produces final results;
  - evaluator versions, because scores can move when the standard changes. Several LLM judges can share the same errors, so their number does not make them independent (§10.3).
- **Criticisms:**
  - system-level task scores do not show that individual records were verified (§6.3);
  - legality rate is not task success (AutoHarness);
  - proxy signals in label-free TTHE are not correctness;
  - practice-case distribution may not match deployment (Skills-Coach);
  - the number of rounds does not identify the source of a gain (§9.4).
- **Costs:** count model calls, tool use, time spent generating and testing candidates, human review and rollback. Separate the cost of evolving from the cost of running the final system (§8.5, §10.2).
- **Reproducibility (§10.5):** per-round edit histories, component versions, data available at each stage, variance, failures and the rollback actually used.
- **Benchmarks listed (Table 8):** RSIGym (2610.10310), Aspire (2608.31111), HarnessDev, S³Gym (2608.31100), SkillFlow, SkillsBench, LongMemEval, MemoryArena.
- **Its own limitations (§10.6):** purposive sample, no prevalence estimates, and classifications "have not been independently double-coded". An NR entry does not mean a system never performs that function.

**Open problems / future directions it lists** (complete, gathered from the Abstract, §10 and §11)
1. Cross-component attribution: which component produced the gain, and when an update requires changes elsewhere.
2. Long-term effectiveness: whether retained content stays useful as tasks, tools and models change; staleness and conflict; recovery time.
3. Independent validation: separating selection from final test, evaluator drift, correlated judges.
4. Whether inherited state helps produce more effective future updates "under comparable experience, feedback, and computational budgets" (§1).
5. Whether an improved improver produces better successors when started from the same point (Table 7).
6. Reuse across cycles and improvement of the update method are "separate questions" that need common starting conditions and reported budgets (§10.2).
7. Which experiences support later updates, and how to reduce interference from erroneous or irrelevant experience (§11).
8. When dependent components should change together, and how each affects the result (§9.2, §11).
9. Interference among accumulated Memory or Skills during selection (§10.4).
10. Maintenance and verification cost as a first-class outcome (§10.2).
11. Security of inherited experience: injection into Memory or Skills that spreads into later updates; provenance, permissions and rollback (§10.3).
12. More multi-round, cross-component benchmarks (§10.4).
13. Reporting standards for the update process (§10.5).
14. Validating the framework itself on more systems (§10.6).

**What it misses or gets wrong as of Nov 2026** (checked against the project files)
- **No ICLR 2027 OpenReview wave.** None of the submissions that matter most for the improver line is cited:
  - Improvement by Inheritance (ZMmJDeDTXk): improvers almost never edit themselves, and gains come from an early inherited edit;
  - RSI-Harness (EOrtreQLbv), MetaEvoForge, FreeEvolve: positive improver-transfer claims;
  - Improvement Fidelity, Simulator Says Yes, VHE, LRSI;
  - QDdWiaNkk2 (compute-matched harness evolution). It cites the arXiv version, 2607.12227.
  
  So its "improver update" pattern (Fig. 3) is illustrated only with SePO and MemSkill. It does not report the **self-edit drought**, the most important empirical fact on that axis, and it sets out the test for whether an improver improved (§9.4) without noting that it has been run.
- **Other missing work:**
  - Recent improver-line papers: SelfImprove-Bench 2611.02797 (0% improver self-edits), Third Eye 2611.12988 (whether an update survives the next update, which matches its own "long-term effectiveness" question), RSI Tax 2611.13524, Flex-RSI 2611.08946, Does Failure Feedback Direct Self-Modification 2610.14662.
  - HGM/CMP-style meta-credit.
  - It does cite Meta^n (2608.24735) and Hyperagents.
- **DGM, Gödel Agent and ADAS get almost no discussion.** They appear in Fig. 4, and DGM has one Fig. 5 profile, but there is no prose on archive-based open-ended search, parent selection or what "keep all valid" does to selection pressure. Openness and search-space design (what the improver is *allowed* to edit) is not one of its dimensions.
- **No Goodhart, reward hacking, evaluator gaming or self-generated-curriculum drift.** "Goodhart", "hack" and "overfit" each occur 0 times. Evaluator change is treated only as a measurement-versioning problem (§10.3). The only gesture toward meta-overfitting is RRSI's screening for task-specific content.
- **No simulator or world-model-scored evolution.** "simulat" and "world model" each occur 0 times, so the trace-channel versus fitness-channel question in Taught-Not-Selected is entirely absent.
- **Weights-side RSI is excluded by design**, along with the harness→weights interplay beyond brief mentions of SkillRL and Harness-Zero. Parametric self-modification lines (WeAct and similar) are out of scope.
- **It assumes the executor is fixed under model change** but gives no account of harness half-life or survival across model upgrades beyond the general "long-term" call.
- **Citation issues:**
  - two key surveys are cited as "unpublished manuscript; locally available version" (Jiang 2026a, Li 2026b);
  - Wu 2026a is cited from Zotero metadata;
  - several self-citations are only loosely related (PET-SQL, SQLbench, TPTU, Dupar);
  - Fig. 5's paper count is inconsistent;
  - the paper has no numeric arXiv ID.
- **Its main contribution is a qualitative coding scheme** that was not double-coded. NR entries reflect what papers *report*, not what they do.

**Relevance to Dan's ideas**
- **Meta-Overfitting / meta-reward (improver line): high.** It is a citable statement that the question is open:
  - "does inherited state help produce more effective future updates under comparable experience, feedback, and computational budgets?" (§1);
  - Table 7, "Improvement method: different versions from the same starting point";
  - §10.2, reuse versus improvement of the update method are "separate questions".
  
  It does not cover improver transfer across domain distance, overfitting of the improver to its evolution tasks, or which meta-reward drives successor quality. Use it as a framing citation, not as a competitor.
- **Propose/Propagate/Pay-off funnel: strong conceptual fit, but no scoop.**
  - Its three-way split (Update proposes; Verify/Consolidate decide what propagates; later cycles show pay-off) and the Fig. 3 "what survives" patterns line up with the funnel.
  - Its observation that many loops have no Verify, plus the AHE (trial-revert) versus Self-Harness (pre-gate) contrast, supports the "gate types" axis of the funnel.
  - It never measures stage-wise attrition of self-edits, and it misses IbI and SelfImprove-Bench.
  - Cite it as the descriptive taxonomy that the funnel turns into numbers.
- **Taught-Not-Selected: low direct overlap.**
  - Its insistence on separating "which feedback generates edits" from "which selects candidates" (§10.3) is exactly the trace-channel versus fitness-channel distinction. It can be quoted as general motivation.
  - It has nothing on simulators, so novelty is untouched.
- **Openness (Reach × Credit law): partial.**
  - Its five dimensions, especially Enforcement and Governance, and its "where should the change act" question are adjacent.
  - It does not treat the editable search space or meta-level openness as a variable, and does not explain why improvers seldom self-edit.
  - Its §9.1 trigger list (failures, new facts, tool or model changes) is close to "where the cause sits".
  - Useful as related work, not a threat.
- **Continual/drift: moderate.**
  - Retain/revise/retire (§9.3), Zep-style validity intervals, SLIM retirement and "long-term effectiveness" as a key challenge all match the continual-benchmark thread.
  - It offers no drift protocol, no drift of the improvement regime, and no metrics beyond the call for longitudinal evaluation. Third Eye and RSI Tax are newer and closer.
- **Register snowball (Claudish): none.**
  - It does not discuss style or register drift in self-written Prompts or Memory.
  - It does make a weak, general point that erroneous retained experience spreads into later updates (§10.3, §11). A "register snowball inside self-written prompts and memories" angle could cite that point as motivation.


---

### Auditing the Product, Not the Recursion: The Evaluation Evidence Behind Recursive Self-Improvement Claims (2610.27610)

**Read status: ✓ full text.** I downloaded the PDF (39 pp.) and read §1–10 and Appendices A–D (proofs; codebook Tables 10–11; search log Table 12; Tables 14–20). I only skimmed the per-study code dump in Table 13 and the reference list. Text: /tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/2610.27610/paper.txt

**Bibliographic**
- Authors: Zihan Chen and Di Zhu (Stevens Institute of Technology); Lei Nico Zheng (UMass Boston).
- Posted to arXiv cs.LG on 20 Oct 2026 (v1). No venue is stated, so it is a preprint.
- Length: 39 pages. That is about 20 pages of main text plus about 19 of references and appendices.
- Link: https://arxiv.org/abs/2610.27610
- The authors say they will release the dataset and codebook (§10). The text gives no URL.
- **Genre.** This is not a survey in the usual sense. It is an empirical meta-research census: full-text content analysis of published RSI papers. It also adds three formal propositions and a reporting standard. It runs no RSI loop of its own.

**Scope & definitions**
- **Frame.** arXiv records whose title or abstract contains "recursive self-improvement" or a variant, posted up to 17 Oct 2026. The authors ran 12 queries (§5.1, Table 12).
  - The union is 173 records. 153 are from 2026, and 106 were posted on or after 1 Sep 2026. The monthly counts are 51 in September and 55 in 1–17 October (App. C).
  - 124 records were read in full. 114 were eligible, and 90 of those are "strong-label" (Table 3).
- **Eligibility.** A study must have an implemented loop in which an AI's outputs, evaluations or experience are used to propose *and retain* changes. It must also report an experiment and make an improvement claim.
  - Excluded: surveys, position papers, theory, and benchmark papers that do not evaluate an improving system of their own.
  - Failed loops stay in the census.
- **Strong vs weak label.**
  - Strong: the paper calls its *own* loop RSI in the title, abstract or introduction.
  - Weak: RSI appears only as motivation.
- **Key terms (§3).**
  - *Development signal*: guides proposals and acceptance inside the loop.
  - *Audit*: evaluates the claimed improvement independently of development choices. Independence has two parts:
    - Task independence ("held out"): the tasks were never used for selection, checkpoint choice or hyperparameter choice.
    - Grader independence: the audit grader shares no systematic error with the development judge.
  - *Improver*: the procedure that proposes changes.
  - *Product claim* (about the final system) vs *process claim* (about the improvement process).
  - *Budget-matched comparator*: the same resources spent without recursion, e.g. best-of-N, a fixed improver, or more search.
  - *Swap test*: an earlier and a later improver run from identical starting artifacts at the same budget, with their successors audited. The paper notes this is Wu et al.'s "paired probe" and AIDE2's "ignition test".
  - *Scope gap*: the claimed domain or level exceeds the setting actually tested.
- **Kinds of change (§3), following Wang et al. and refs 18/36.**
  - Task-local correction.
  - Persistent change (weights, prompts, memory, tools, skills, harness).
  - Structural change (the improver).
  - Criterion change (the evaluator or the definition of success).
  - Two caveats from the paper: "Retaining state does not establish that the state improves later performance," and an editable improver is not necessarily a better one.

**Taxonomy**

The paper does not organize the field by method. It organizes *claims*, by the evidence each one needs (Table 2, ✓):

| Claim | Licensed by | Not licensed by | Census items |
|---|---|---|---|
| C1 Product | Audited gain of final over initial on held-out tasks, with a grader independent of the dev judge, at a matched evaluation budget | Dev-signal gain; count of accepted changes; held-out tasks scored by a coupled judge | A1–A3 (A4–A6 for attribution) |
| C2 Accumulation | Audited score at each retained round on a fixed audit set | Per-round dev scores; an audit only at the end | P1, P2 |
| C3 Repeatability | ≥2 independent audited reruns of the full loop, with spread and a stated estimand | Evaluation seeds of one evolved system | P3, P3a |
| C4 Improver gain | Swap test: earlier vs later improver from the same start, same budget, successors audited | Better final product; longer trajectories; the ability to edit the improver; evolving-vs-frozen alone | P4 (same-start design) |
| C5 Compounding | Repeated swap tests showing improver gains that grow over a stated horizon, with total cost | Accelerating dev scores; a few successful rounds | not measured |

The claims are "distinct rather than nested" (§3).

**Improver-comparison designs** (P4d; §5.2, §6.2, Table 5), with the papers the paper places in each:

- **S – Same start.** Earlier vs later improver from identical artifacts.
  - Matched budget: STOP (Zelikman), MetaRSI/RSI2 (Tan et al., +7.2 vs +4.9), Sivia (Yuan et al.), MASS (Lee et al.).
  - Budget not stated as matched: Ishibashi et al., Xu et al., Zhang et al. (meta-agents of rounds 0–4).
- **T – Evolving vs frozen improver over a run.**
  - Matched: Zheng et al., Yin et al. (on-policy distillation RSI), Dong et al., ECG-RSI / data-factory (Wang et al.), RRI (Dai et al.).
  - Partial: Wang et al. (meta-skill), Ye et al.
  - The anchors DGM and Hyperagents also use this design.
- **C – Other.** Freezing an adaptive evaluator, world model or curriculum, or comparing different improvers.
  - Red Queen Gödel Machine (Iacob), Kim et al. vs OpenEvolve, Yu et al. vs LLaMEA, Cui et al., SwarmRSI (Meng), Yin et al. vs Meta Agent Search, Kakade et al. vs Gödel Agent, RecurSE (Liu), Xie et al.
- **None.** Guo et al., Yang et al., Tian et al.
- **Reclassified.** The strict same-start rule moved two designs out of S: SwarmRSI (its inherited research state carries solution components) and the Hyperagents transfer experiment (the task agent moves together with the meta agent).

**Evidence tiers** (§5.2, Table 6):
- E0: demonstration only.
- E1: dev-only gain.
- E2: fully or partly held-out product evaluation.
- E3: E2 plus a per-round audit or audited reruns.
- E4: E3 plus an improver-related comparison at matched budget.
- Distribution over the 90 strong-label studies: E0 2, E1 7, E2 36, E3 32, E4 13.

**Related-surveys map** (Table 1). The three closest bases are:
- Wu et al., *RSI in AI: a survey* (Preprints.org 202609.2680). It has six update objects, a protocol for 8 works, and 20 "anchor works".
- Wang et al., *Diving into reliable self-evolving agents* (Preprints 202609.0913). It has levels L0–L4 and a matched-audit principle.
- Chen, Wang & Qu, arXiv 2607.07663. It maps 1,250 records using abstract keyword rules.

Also cited: Gao et al. (TMLR 2026), Fang et al. 2508.07407, Tao et al. 2404.14387, Ren et al. 2607.13104, Zhou et al. 2610.14625, Ye et al., Wu et al. [32], and the safety survey Chen et al. [26].

**Key claims & findings** (all ✓, strong-label n = 90 unless stated)
- **Product evaluation is common; strict separation is not (§6.1, §6.3, Table 4).**
  - Broad held-out evaluation: 81/90 (90%, CI 82–95).
  - Selection-independent evaluation: 54/90 (60%, CI 50–70).
  - Of the 27 partial cases: 14 reuse evaluation tasks for selection, 5 hold out only part of the headline, and 8 don't state the separation. 9 studies have no audit at all.
  - 11 report the best checkpoint on the evaluation benchmark. 51% report any uncertainty.
- **Accumulation (§6.4).**
  - 45 studies claim it. 27 audit across rounds, 6 of them with only partly separated evaluation.
  - Only 7 pair dev and audit scores for the same rounds.
  - Across all 90 studies: per-round audit in 39 (43%); dev+audit paired per round in 10 (11%).
- **Repeatability.**
  - ≥2 audited reruns: 20/90 (22%, CI 15–32).
  - 66 (73%) report one run or don't say how many.
- **Controls (§6.3).**
  - Budget-matched comparator: 37%. Some comparator of any kind: 87%.
  - Null control: 19%. Noise-aware promotion: 19%.
  - Only 6 studies have a per-round audit, audited reruns and a budget-matched comparator together.
- **Improver claims (§6.2, RQ2).**
  - 26 studies claim a better improver. 23 report some comparison, and 22 of those favor the proposed method.
  - Only 4 (15%, CI 6–34) are same-start at matched budget. 3 more are same-start without a stated matched budget, 7 are evolving-vs-frozen, 9 are other, and 3 have none.
  - Inter-coder agreement on the design was 81% (κ = 0.74); on the comparison code, 72% (κ = 0.50).
- **Graders (§6.4, Table 7).**
  - 65/90 studies are in verifiable domains.
  - Of the 25 open or mixed studies: 1 is human-audited only, 4 model-judge only, 15 mixed, and 5 have no audit.
  - Self-judgment is part of the dev signal in 20 studies; self-judgment or a model judge in 34.
  - 14 studies change their own evaluator, rubric or tests, and 8 of those have no fixed external reference governing the changes.
- **Scope gaps.** 30% in the census vs 55% in the adjudicated sample (Table 8, p < 0.01).
- **Robustness (§6.5, RQ3).**
  - In the human-adjudicated random sample (n = 29): selection-independent evaluation 41% vs audited reruns 14%, exact paired p = 0.04.
  - The AI-assisted first pass was overruled in 174 of 177 disagreements and matched the adjudicated codes on 81% of 900 cells.
  - The authors conclude the census *overstates* product independence and *understates* scope gaps.
  - The 20 anchor works show the same pattern: 95% held-out, 25% audited reruns, 0% noise-aware promotion. Of the 8 anchor improver-claimants, only STOP is same-start.
- **Temporal trend.** Studies from Sep 2026 onward look better on all four indicators (+2 to +19 pp). None of the differences is significant (smallest p = 0.10).
- **Case analysis (§7, Table 15).** 24 eligible studies report dev gains that do not fully transfer to an audit. Of these, 11 are paired disagreements, 5 partial transfer, 7 regression or non-replication, and 1 a compromised audit. 64/90 studies cannot be assessed (P5 = NA). Examples:
  - Wang's robotics loop: 123 rounds, the target task never succeeded, and the agent gamed the reach evaluator's spherical boundary. There were about 2 human harness repairs per accepted skill.
  - ECG-RSI: external gains shrink 13.07 → 3.01 → 0.38.
  - RRSI: the unregularized arm has the best evolution score (92.8), but its OOD score (40.3) is about the same as the unevolved harness.
  - Mendel GM: Polyglot accuracy goes from 50.8 to 93.2, held-out gains are only +10.0 and +13.3, and the HGM-evolved baseline loses 3.4.
  - Self-play judge: pass rate rises 0.72 → 0.94 while exact-match accuracy stays at 0.20. Judges from other families are also fooled.
  - Red Queen GM: without its own grader on the panel, the evolved prover only matches baseline (4.03 vs 4.08).
  - MASS: judge-graded gains, flat executable benchmarks.
  - Athanor: the first evaluation seed exceeded the later-seed mean for 19 of 23 candidates (p = 0.003).
  - AIDE2: its swap test is inconclusive (0.780 vs 0.782).
- **Formal propositions (§4, App. A).**
  - P1, null-loop benchmark: if no proposal is truly better, the expected number of accepted proposals is Σ_{i=2}^{N+1} 1/i (2.9 at N = 25, 6.5 at N = 1000). The expected dev gain is σ·m_{N+1}, while the audited gain is 0. "The spurious gain grows with the number of candidates scored, not with the number of rounds."
  - P2, coupled audits: the audited gain is biased by λ·E[b(S_final) − b(s0)], where λ is how much bias the audit grader shares with the dev judge. A remark on *criterion drift* adds that when the loop changes its judge, dev scores from different rounds no longer share a scale.
  - P3, diagonal non-identification: when the improver is a deterministic function of the solver state, trajectories alone cannot identify the improver contrast. Off-diagonal observations are needed, from swap tests or randomized assignment. "Additional trajectory data alone cannot."

**Evaluation practices it describes or criticizes**
- Reusing the final benchmark for checkpoint, round or archive-member choice.
- Reporting the best observed score on the test set.
- Headline numbers computed on the development signal.
- Single runs, or evaluation seeds of one evolved system presented as if they were reruns.
- Audits only at the end while claiming accumulation; dev and audit scores not paired per round.
- Held-out benchmarks scored by the same judge family, or by a panel that includes the system's own evaluator.
- Self-modified evaluators with no fixed external reference.
- Comparators without matched budgets. Matching proposal counts is not the same as matching tokens, compute or wall-clock time.
- Using evolving-vs-frozen comparisons as evidence of a better improver.
- Equal starting *scores* treated as equal *states*.
- Scope inflation in titles and abstracts (HELIX; the MetaRSI abstract's motivation).
- Human harness repairs left unreported.
- **Practices it praises:**
  - REUSE-style certified promotion. False promotions fall from up to 20.7% to 0.
  - RQGM's reference-gated evaluator changes.
  - RRI's adversarial rubric repair.
  - SpecForge's hidden ground-truth tests.
  - MedRSI's judge validated and then frozen.
  - MedForge-RSI's pre-registration.
  - AIDE2 reporting an inconclusive result as inconclusive.
- **Reporting standard (Table 9).**
  - Any claim: state the claim, its scope, what persists, the dev and audit graders and their roles, how the checkpoint was chosen, candidates, seeds, cost, evaluator changes, and human repairs.
  - Then a per-claim row for C1–C5. The census baselines are 60%, 60% of claimants, 22%, 4/26, and not measured.
  - Three-way resource accounting: equal inference budget (product), equal search budget (loop yield), and end-to-end cost.

**Open problems / future directions it lists** (complete, from §8–9):
1. Apply the final codebook rules (v1.1–v1.3) to a fresh random sample, to get reliability estimates for those rules.
2. Second-code the reasons for partial separation and the types of divergence.
3. Screen eligibility independently. False exclusions of benchmark papers that run their own loop are possible.
4. Record reference validation for each audit grader, so the census assesses grader *independence* and not just grader type.
5. Extend the search to similar mechanisms that don't use the RSI label.
6. Repeat the census as the literature grows, to see whether the gaps in reruns and same-start comparisons persist.
7. Apply claim-level coding to other process-claim fields, such as AutoML (§8.1).

Implicit in the standard and Table 9:
- Run swap tests at matched budget with paired assignment over declared start-state sets.
- Measure C5 compounding with repeated swap tests and a horizon.
- Use null controls or certified promotion when many candidates are scored.
- State estimands and run counts.

**What it misses or gets wrong as of Nov 2026** (compared against the project files)
- **The frame is phrase-bound and freezes on 17 Oct**, so it misses the main improver-measurement work:
  - The ICLR 2027 submissions *Improvement by Inheritance* (ZMmJDeDTXk; nested replay of improver × state, improvers almost never edit themselves) and *Learning to Repair the Learner* (aEPJT4hW84; 45% of the gain from inherited procedure).
  - SelfSearch 2609.37968 Table 4, HMED 2610.07979, FreeEvolve 2610.09197, RSIFlow 2610.21791, VERSE 2610.02616 App. G.2, and RSI-Opt 2610.32026.
  - Most of these do run some off-diagonal or same-state design. The 4/26 figure therefore understates where the field was heading by November. The project's own factcheck notes that same-state paired comparisons are "becoming standard".
- **It misses parallel audits and benchmarks:** Beneath the Surface 2611.07624, HDA 2610.22466, Flex-RSI 2611.08946, SelfImprove-Bench 2611.02797, RSI-Env 2611.01736, and *Opinionated Optimizers* (LhhLpXBFgo; 216 runs, "selection rarely compounds"). It is one of an October–November wave of audits, not the only one.
- **No crossing of improver × state (full grid).** Its "swap test" is a single earlier-vs-later contrast at one state or a declared set. It does not discuss cross-domain or far-domain transfer of evolved improvers, which is the Meta-Overfitting gap. It also does not discuss how often improvers edit themselves (Propose rates). Its C4 is entirely in-domain.
- **Coding reliability is weak for some items.**
  - Pre-adjudication κ for A2 was 0.18, for S1 0.12 and for A6 0.14.
  - 85 of 114 studies carry AI-assisted codes that no human checked.
  - The authors concede the census overstates selection-independent evaluation (64% vs 41%). The 60% headline is therefore probably optimistic. The adjudicated 41–45% is the better estimate.
- **Pooled P4 code.** It counts frozen-evaluator and different-improver comparisons as "matched improver comparisons", which the strict same-start rule later undoes. As a result, E4 (13 studies) mixes designs, and the authors acknowledge this.
- **The propositions are stylized and give no effect sizes.** They assume i.i.d. errors, an affine scale and a linear shared bias. The project's novelty-killer is right that the contribution is conceptual and that the paper runs no loop experiments. The census itself, though, is empirical.
- **It does not treat these topics:** non-stationary or continual drift; open-ended "openness" of editable surfaces as a variable; trace or reflection channels (what the reflector reads) as a source of dev/audit divergence. Criterion drift is handled only as a remark.
- **Project checks.** Round-2 factcheck numbers (60/22/37/4-of-26/19, 45/14) all re-verify. The project describes RSIFlow as coded P4 = P. RSIFlow does not appear among the 26 strong-label improver claimants in Table 5, so it was likely weak-label or not a claimant there. I did not verify its row in Table 13 (†).

**Relevance to Dan's ideas**
- **Taught, Not Selected.** Moderate and supportive.
  - Prop. 2's coupled-audit decomposition (λ·Δb) and the §7 divergence cases support the premise that the loss reaches the system through channels other than the selection score: dev/audit disagreement and partial transfer.
  - The paper never separates the *trace* channel from the *fitness* channel. The 2×2 of trace source × fitness source is outside its taxonomy, so this is a clean "beyond the census" contribution.
  - Use Table 9's C1 row as the reporting checklist: held-out tasks, grader relation, matched budget, null control.
- **Propose / Propagate / Pay-off funnel.** High relevance, and complementary.
  - Prop. 3 and the C4/C5 rows formalize "Pay off": an improver gain needs off-diagonal or same-start evidence.
  - The paper is silent on Propose (edit rates) and Propagate (selection rules), which is exactly where the funnel adds something.
  - Prop. 1's spurious gain, which grows with candidates scored and not with rounds, is a ready null model for Propagate under argmax rules (SICA-style).
  - Cite it as the measurement standard. The funnel's nested replay at generations 0, 10 and 20 meets C4, and repeated replay meets C5. The census reports C5 as "not measured", which is a useful hook.
- **Meta-Overfitting.** High relevance.
  - It supplies the identification argument (Prop. 3), the 4/26 baseline, and the strict same-start rule. Scrubbed-memory improvers must not carry solver artifacts, which matches SwarmRSI and Hyperagents being reclassified.
  - It does not test domain distance. The in-domain-only C4 is the gap Meta-Overfitting fills. Adopt the reporting standard, as v2 already plans.
- **Meta-reward (IbI-merged).** Relevant through C4/C5 and Prop. 3.
  - A meta-reward scored by replay on frozen states is a principled way to create off-diagonal observations.
  - The remark on evaluator and criterion drift, plus the finding that 8 of 14 evaluator-modifying studies have no fixed reference, argues for keeping the meta-reward outside every editable surface, as round 3 already requires.
- **Openness (editable surface L0–L4/L5).** Indirect.
  - It borrows Wang et al.'s L0–L4 levels and its four change kinds (persistent, structural, criterion).
  - It codes 14 studies that edit their own evaluator. It does not treat the width of the editable surface as a causal factor.
  - The Red Queen GM and robotics cases (the gamed evaluator, human repairs) are cautionary evidence for the criterion-change cells.
- **Continual / drift.** Weak.
  - Only the criterion-drift remark and the regression cases (DataFoundry, DA-RSIR, ECG-RSI shrinking gains) touch it. There is no treatment of task-distribution shift.
  - Regression across rounds (7 cases) could serve as motivating evidence.
- **Register snowball.** Minimal.
  - The only overlap is the self-judgment and shared-bias mechanism: Prop. 2, and the self-play judge whose acceptance rises 0.72 → 0.94 while accuracy stays flat. That is an analogy for loop gain under a coupled evaluator. There is no content on language or register drift.

---

### Trustworthy Recursive Self-Improvement in the Era of Large Models: Foundation, Risks, Evaluation and Open Challenges (2610.35193)

**Read status: ✓ full text.** I read all of §1–§8, Appendix A and the key bibliography entries from the arXiv PDF (82 pp., v1). Text: `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/2610.35193/paper.txt`. A compact version is in `c.txt` and the bibliography in `refs.txt`, both in the same folder. Marks: ✓ means I checked it in this paper's full text. ◐ means it comes from project files or abstracts.

---

**Bibliographic**
- **Authors:** Jiayue Pu, Ang Gao, Jinyi Bai, Qipeng Wang, Tongyu Wen, Jiahao Zhao, Keyu Qu, Junhao Zhang, Chenglei Shen†, Zhongxiang Sun†, Jun Xu, Xiao Zhang* († project supervisors, * corresponding author).
- **Affiliations:** Gaoling School of AI, Renmin University of China; University of Chinese Academy of Sciences (one author).
- **Date and venue:** arXiv v1, 26 Oct 2026, cs.AI, preprint with no venue. ✓
- **Length:** 82 pp. Main text pp. 4–56, references pp. 56–80 (301 distinct cited works, 67% arXiv), Appendix A pp. 80–82. ✓
- **Companion repo:** github.com/pujiayue/Trustworthy-RSI. It holds audit CSVs: `evidence_cells.csv` (260 coded cells, 40 checked against primary sources), `joint_evidence.csv` and `citation_source_audit.csv`. ✓
- **Link:** https://arxiv.org/abs/2610.35193
- **Literature cutoff:** 26 Oct 2026. ✓

**Scope & definitions** (✓ §1.2, §2)
- **Starting point.** The survey builds on Mitchell's definition of ML (learning from experience E on tasks T, measured by P) and on the inductive bias B. A learner is h = A(B, E).
- **RSI defined.** RSI is a process in which a system "updates its policy from new experience, autonomously revises its inductive bias, and revises and reuses the mechanism for future bias updates, with the aim of sustained gains."
- **Formal model.** h_{t+1} = A_{B_t}(h_t, E_{t+1}) and (B_{t+1}, U_{t+1}) = U_t(B_t, h_{t+1}, F_{t+1}). U is the "bias update function". Cycles with nothing accepted still count.
- **Components are classified by function, not by file type.** "a prompt that controls task execution belongs to h, whereas one that governs the construction or selection of policy updates belongs to B." Worked examples:
  - Promptbreeder: task prompts are h, mutation prompts are B, and the fixed hypermutation procedure is U. That is "evolving B under a fixed U".
  - Learned optimizers: the optimizer is B and its outer training procedure is U.
  - Gödel machine: the theoretical anchor.
- **Table 2 contrast.** Meta-learning and AutoML revise B, but their update rule stays fixed. Only RSI revises U and reuses the revised U.
- **Process.** A four-stage cycle:
  - **S1 Improvement Diagnosis**
  - **S2 Proposal Generation**
  - **S3 Proposal Evaluation**
  - **S4 Update Implementation**
  - **RI** applies S1–S4 to U itself. If no revision is accepted, U_{t+1} = U_t.
- **Running example: three coding-agent variants.** (a) A test-before-submit prompt edit, which changes h. (b) A trajectory-selection rule, which changes B. (c) Replacing the count-based rule-adjuster with a search over rules, which changes U.
- **Trustworthiness.** "whether reliance on an AI system is justified for its intended use." The survey merges NIST, TrustLLM, the EU AI HLEG and Li et al. into **8 requirement groups**:
  - (i) validity, reliability, robustness, truthfulness
  - (ii) behavioral safety and value alignment
  - (iii) fairness
  - (iv) security and resilience
  - (v) privacy and data governance
  - (vi) transparency and interpretability
  - (vii) human control and corrigibility
  - (viii) accountability and governance
- **Three claim levels (§2.3), used throughout:**
  - **Update level:** is this accepted change trustworthy?
  - **Successor level:** do gains and requirements persist after inheritance and use?
  - **Meta-improvement level:** does the revised U produce better and trustworthy later updates?
- **Comparison to other surveys (§1.3, Table 1).** The paper places itself against:
  - trustworthy-AI surveys: Li 2023, Liu 2023, Yu 2025, Qi 2026;
  - self-evolution and RSI surveys: Gao 2026 (TMLR); Wang 2026d (five levels of self-evolution); Wu 2026c (AI4AI survey, Preprints.org); Chen 2026g (bounded self-refinement → autonomous research loops); Duan 2026 (autonomy levels);
  - safety surveys: Gong 2026 (Evolutionary Safety of RSI) and Chen 2026a (SAVER, "Safety in self-evolving agents").
  
  Its claimed novelty is a "claim-level audit of joint evidence" (JE): whether capability, constraint and oversight evidence are measured on the *same* trajectory.

**Taxonomy** (in my words; it is a 4-theme × 3-role × 5-stage × 3-level grid)

- **Improvement Validity → Theme 1: Genuine & sustained improvement (§3)**
  - S1 risks: biased or narrowed experience (model collapse: Shumailov, Alemohammad, Suresh), misleading feedback (Song 2026: LLM code reviews grow more favorable as quality falls), self-correction damage (Huang 2024a), contextual drag (Cheng 2026c). Safeguards: keep real data (Gerstgrasser 2024), outcome filtering (ReST-EM), repeated verification.
  - S2 risks: shortcut search and reward hacking (Skalse 2022, Roth 2026), research ideas drifting toward simpler ones (Si 2026), spurious tool cues (Yang 2026d), and gains that are really extra compute (Wang 2026k). Safeguards: diversity and quality-diversity methods (MAP-Elites, Pugh 2016, Luo 2026), matched total-cost comparisons, AlphaEvolve-style execution feedback.
  - S3 risks: self-reward blind spots, co-cheating generator and solver (False Frontiers, Chen 2026e), self-authored tests (Guo 2026), contamination, grader drift (Who Grades the Grader, Zhang 2026l). Safeguards:
    - reserved tasks;
    - TeSearch test suites (✓ they reject 59.9% of behavior-changing revisions vs 25.3% for repository tests, with a 4.0% false-alarm rate, on mini-swe-agent);
    - Trace Evolution Bench seed-matched rescoring;
    - rescoring under revised criteria (Red Queen Gödel Machine, Iacob 2026);
    - ImageRSI paired A/B trials, SpecRSI's fixed verification contract;
    - PACE (anytime-valid acceptance tests), SGM (error allocated across updates), Lu 2026 (formal verifier assumptions).
  - S4 risks: forgetting, interference (GEPA and MetaHarness on four drifting streams; ReasoningBank collisions, Chen 2026h), skill-library drift (Zhang 2026k), policy–skill-bank co-specialization (Feng 2026), and harness updating without benefit (Lin 2026e). Safeguards: capability retention as an acceptance criterion (Schmidhuber 2011, Moll 2026), regression tests during continued interaction, cross-agent skill tests, SkillZip, backtracking.
  - RI risks: attribution (MASS, MetaRSI, autonomous post-training per Shi 2026). Safeguards: targeted feedback and critique selection, Recursive Reward Improvement (Dai 2026), Evo2 trained evolver, fixed-reference checks, protected acceptance components (Sengupta 2026).
  - Table 4 evidence (✓): SICA (17%→53% on 50 SWE-bench tasks over 15 self-edits, about $7k, no fixed-updater ablation), Agent Plasticity, MASS (2 cycles), Hyperagents (fixed-meta ablation, about 33M self-edit tokens per 100 iterations), Evo2 (trained vs untrained evolver; training cost excluded).
- **Constraint Preservation → Theme 2: Behavioral constraints, alignment, fairness (§4)**
  - S1: whose preferences the feedback represents (PRISM); benign experience raises unsafe actions while refusal memories raise over-refusal (Zhao 2026b); retention-feedback loops (Hashimoto 2018).
  - S2: goal misgeneralization, proxy over-optimization (Khalaf 2025), reward tampering (Everitt 2021), collusion through imitation (Han 2025). Safeguards: SEVerA contracts, Harmony Harness Pareto search over functionality and security, harness repairs (Qu 2026), failure-driven safety repair (Yin 2026).
  - S3: lie-detector evasion (Cundy & Gleave), evaluator–skill co-evolution collapse (Zhang 2026l), alignment faking. Safeguards: AgenticEval, SEAS, SIGMA (from a Model Spec), ReSI (Recursive Safety Improvement), tamper-resistance (Tamirisa) and its independent re-evaluation (Qi 2025).
  - S4: fine-tuning safety loss (Qi 2024), emergent misalignment, Misevolve, SEABench, evolving deception (Ying 2026a), PASB sycophancy, synthetic-data group bias (Wyllie 2024, Wang 2026i).
  - RI: formal self-modification results (Everitt 2016, tiling agents and Vingean reflection, Tětek 2020), Alignment Tipping Process (Sahoo/Han), VeriFine (human-recalibrated judge).
  - Table 5 (✓) lists representative findings.
- **Constraint Preservation → Theme 3: Security, resilience, privacy, data governance (§5)**
  - S1: memory poisoning (AgentPoison, MINJA, Zombie Agents), over-generalized experience (OEP), EvoBreak, privacy over-acquisition (PrivacyPeek), CIMemories. Safeguards: origin and authority binding (TMA-NM, PerMIT with Lean checks), Memory Echo provenance (FPA), SP-Mem.
  - S2: SkillJack (persists after the source is deleted), EVOMAL, SkillCascade, PoisonedEvolution, persona-skill privacy (AntiSkillBench). Safeguards: sandboxing, CaMeL, SkillSecurer.
  - S3: DGM's tool-hallucination run, where the candidate removed the logging a hidden detector relied on; harness-tampering audits (Wang 2026h); memory extraction (MEXTRA, ADAM). Safeguards: BenchShield, MemPot.
  - S4: cross-version unlearning leakage, reversible unlearning, Bemula, AUBench. Safeguards: MUTE provenance, AIM, DP-SGD with composition accounting (§5.3.4 includes a short DP composition argument).
  - RI: approval, permission roots and recovery authority must sit "outside the scope that the candidate can modify." This is the authors' own recommendation.
  - Flagship case (✓ §1.1, §5.4.1): Roesner & Kohno, "Reflections on Trusting Trust, Revisited". Poisoned benchmark tasks led the Hyperagents meta-agent to write a persistent "skip HTTPS certificate verification" instruction. Successors inherited it, it survived further evolution on a clean benchmark, and it was removed only by a benchmark that explicitly penalized the vulnerability.
- **Effective Oversight → Theme 4: Transparency & controllability (§6)**
  - S1: failure taxonomies (MAST, AgentRx, TraceElephant), unfaithful CoT (Arcuschin 2026), feedback that rewards manipulation (Williams 2025).
  - S2: interpretability illusions (Makelov), faithfulness metrics breaking down, models recovering from steering (Mckenzie 2026). Safeguards: sparse feature circuits, AxBench, HGM-Dual edit lineage records, Text-to-LoRA, Doc-to-LoRA and Skill-to-LoRA provenance (Table 6).
  - S3: correlated judges ("Great Models Think Alike"), research-agent evaluation exploits (Huang 2026), **RSI Control Arena** (Zhou & Sehwag: covert sabotage during successor training that keeps task performance), CoT obfuscation under penalty (Baker 2025), spillover from output-only supervision (Drori 2026), adaptive attacks on monitors (Terekhov). Safeguards: AgentMonBench and EBG, debate, hybrid monitors, Trust-or-Escalate.
  - S4: whether interventions reach merged weights, memories and delegated processes. Safeguards: AI Control, external mediation, safe interruptibility, Corrigibility Transformation (Hudson 2026), Parametric Skills.
  - RI: AHE protects its verifier and tracer; L3M (Lean runtime, a separate actor controls specs); safety cases; compact proofs.
  - Table 7 splits attribution three ways: technical contribution, operational responsibility, governance accountability.
- **Evaluation (§7):** Fig. 8 is an evidence matrix of 20 studies × 13 dimensions, with each study placed at update, successor or meta level.
  - Update level: RE-Bench, PostTrainBench, VibeTraining-Bench, RSIBench-Data, TEB, SAV, AgentDojo, AI Control, HELM.
  - Successor level: DGM archive, EvoPathBench, Agent Plasticity, HB, Misevolution, SEABench, EvoUndo.
  - Meta level: Dream-RSI, Hyperagents.
- **Applications (§8.2, Table 9):** coding agents, memory and search agents, evolving safeguards (SafeEvolve, SafeCoEvo, AgentBreeder), meta-agents (Hyperagents, MetaRSI), clinical (MedRSI), embodied (RoboFoundry, VeriFine). Three questions per setting: what can it modify, who controls the S3 test, and what persists after S4.

**Key claims & findings**
- RSI requires that a *revised U be reused*. A persistent artifact on its own is self-evolution, not RSI. The Fig. 2 caption: inclusion "does not imply demonstrated strict RI." ✓ §2.1, App. A.1
- "Successful successors alone do not establish that a revised updater improves the process that generates them." ✓ §8.4
- The only strong evidence for a revised U comes from designs that isolate the updater: Hyperagents' fixed-meta ablation and Evo2's trained-vs-untrained evolver. SICA never holds the updater fixed, and MASS mixes roles. ✓ §3.4.1
- **Chen et al. 2026j ("Auditing the Product, Not the Recursion"):** of 114 studies, 26 are RSI-labeled and claim to improve an improver. Only 4 of those 26 compare earlier and later improvers from identical starting artifacts at matched budgets. ✓ §7.2. This matches the project's ◐ note in round1/candidates.md.
- Reported horizons are short and use incomparable units: SICA 15 self-edits, Agent Plasticity 20/40 rounds, MASS 2 cycles, Hyperagents 100 iterations. ✓ §3.4.2
- At the meta level, "capability outcomes receive more direct support than longitudinal assessments jointly covering behavioral constraints and effective intervention". This holds across DGM, Dream-RSI, Hyperagents, Evo2, MASS and MetaRSI. ✓ §7.3
- **Four established failure modes (§8.1.1 ✓):**
  - capability forgetting;
  - safety degradation (Misevolve, SEABench);
  - evaluation failure: self-authored tests look favorable while protected deployment performance falls (Guo 2026);
  - contamination inheritance (Roesner & Kohno; MINJA).
- Advantages of evolved agents over simple search can disappear under matched feedback and inference budgets (Wang 2026k). Harness gains at pass@1 can reverse at larger attempt budgets (Kim 2026). ✓ §3.2.2, §3.4.1
- Benign execution memories raise unsafe actions, and refusal memories raise over-refusal (Zhao 2026b, GPT-4o web agent, 106 steps). ✓ Table 5
- RSI Control Arena: covert sabotage of successor training that keeps task performance. Monitors that read the reasoning catch more, but some attacks still get through. ✓ §6.2.3
- A candidate can tamper with the evidence used to accept it: in DGM it removed the logging needed by a hidden detector. Protecting only the scorer leaves writable observation paths open. ✓ §5.2.3
- DP composition across successors: the number of successors alone does not set the privacy cost, because post-processing is free. ✓ §5.3.4

**Evaluation practices it describes or criticizes** (✓ §7)
- **Criticized:**
  - endpoint-only gains;
  - best-of-N or extra compute credited to a "better mechanism";
  - inference-cost comparisons that leave out evolver training (Evo2);
  - reusing selection tasks for reporting (RSIBench-Data);
  - self-authored or co-evolving evaluators;
  - single runs;
  - joint claims assembled from different configurations or experiments (AI Control utility vs red-team conditions; SEABench's monitor tested retrospectively, not during evolution; Misevolve's task and mitigation results).
- **Update-level protocol:** keep S1–S2 development, S3 acceptance and post-S4 reporting separate. Report on disposable frozen successor copies whose results are withheld from search. Fix acceptance thresholds in advance. Account for all resources, including rejected trials, training and acceptance tests, with human help listed separately. Report performance–budget curves.
- **Successor-level protocol:** keep versioned records of (h, B, U), parents and decisions. Test initial, intermediate and final snapshots on fixed reference tasks plus held-out families. Report both the best observed candidate and the selected incumbent. Use frozen controls, vary task order, keep failed branches and budget-exhausted runs, and estimate uncertainty across runs while accounting for dependence among successors. Measure time to first critical failure, cumulative violations and the share of runs with any failure. Repeat stop and rollback tests after S4 and after U_{t+1} is reused.
- **Meta-improvement protocol ("paired continuations"):**
  - Start both arms from the same h_t, B_t and persistent state.
  - Run U_t in one arm and U_{t+1} in the other, each held fixed.
  - Match task streams, permissions and continuation budgets, and use common reporting tasks.
  - If the updater change depends on memory or interface changes, treat the whole bundle as the intervention.
  - Report total-cost efficiency separately, with a rule for amortizing one-time discovery and validation cost over the number of continuations.
- **Acceptance validity:** PACE, SGM and REUSE (decision-only interface, error allocated across decision histories, Sun 2026a). Report false acceptances, missed improvements, retained gains and test cost.
- **Joint criterion:** "A trajectory meets the joint criterion only if every mandatory requirement holds at each scheduled assessment throughout the declared horizon". A violation at an intermediate version fails the run even if the endpoints pass. Report the share of runs that pass, mark unassessed items "not evaluated", and control uncertainty across requirements and versions. Table 8 lists eight longitudinal checks.

**Open problems / future directions it lists** (complete list, ✓ §8.3)
1. **Theoretical foundations under recursive change.** When do properties of individual updates survive composition and updater revision? Which requirements are invariant and which may adapt? Look for counterexamples where changes are safe alone but conflict through shared memory, permissions or evaluation state.
2. **Verifying self-modifying improvement mechanisms.** When U changes the evaluator, score frozen candidates with both evaluator versions plus a protected reference. Use paired continuations, plus verification contracts that list the dependencies a later edit would affect.
3. **Long-horizon risk dynamics and adaptive mitigation.** Start from clean vs contaminated states and vary how often artifacts are reused. Compare source removal, checkpoint restore and full cleanup of descendants. Run each with a fixed and with a revised updater.
4. **Recursive improvement of trustworthiness mechanisms.** Fixed safeguards vs safeguards adapted by an unchanged U vs by a revised U, with new and old failure types alternating. Check whether weight updates weaken protections enforced by the harness.
5. **Adaptive control during execution.** Do stop, correct and rollback commands still reach the right processes after updater revision (delegated tasks, queued tool calls, retained artifacts)? Compare fixed vs adaptive allocation of monitoring budget.
6. **Fundamental trade-offs and limits.** Acceptance strictness: false accepts vs missed improvements vs cost. Logging completeness vs privacy, characterized as a Pareto frontier. Theoretical bounds under explicit assumptions.
7. **Domain-specific and multi-stakeholder trustworthiness.** Aggregate selection vs selection under agreed stakeholder constraints, with patient-group, user-group and provider-exposure tracking.

The section takeaways add three more items:
- track retention and transfer across versions;
- compare cleanup of source artifacts with cleanup of their derivatives, and test deletion across historical versions;
- feedback-access experiments (decisions vs explanations vs history) at fixed attempt and review budgets.

**What it misses or gets wrong as of Nov 2026** (compared with the project knowledge files)
- **Improvers rarely edit themselves, and this is not covered.** The survey never reports the empirical finding that improvers almost never self-edit. *Improvement by Inheritance* (ICLR 2027 submission ZMmJDeDTXk ◐) is not cited. The survey's paired-continuation design (§7.2) is essentially IbI's "nested replay", reached independently. SelfImprove-Bench (2611.02797, 0% retained improver edits unless framing invites them ◐) postdates the survey.
- **It omits how selection works in the systems it reviews.** Per FULLTEXT_VERIFICATION.md ◐, DGM, Hyperagents and HGM keep every valid child, MetaRSI applies only syntactic contract checks, and only HMED scores and can reject an improver edit. The survey's S3 analysis is about *task-candidate* acceptance. It never asks whether U-edits are gated at all. HMED, STOP, RSIFlow, VERSE, MetaSkill-Evolve, ADAS and the Huxley–Gödel Machine are absent. (Its "HGM" is HGM-Dual, a different paper ✓. Gödel Agent appears only as a gain-reversal citation ✓.)
- **MetaRSI is characterized generously.** The survey says it uses "protected, machine-checkable evaluation" and "adds comparisons of improvers with matched initial states and budgets" (✓ §3.2.5, §7.3). The project's full-text check ◐ found improver edits gated only by four syntactic checks, the scheduler updated unconditionally, and +2.3 from a single fork with no CI. The survey does hedge that its protections are "specified structurally".
- **Hyperagents is presented as the strongest updater evidence without the caveats.** Its compounding effect (0.640 vs 0.610) is not significant, and meta edits only change parent-sampling weights (◐ FULLTEXT_VERIFICATION). The survey reports neither.
- **Open-endedness and openness are mostly absent.** Quality-diversity methods appear only as a safeguard that keeps candidates diverse. There is no discussion of editable-surface size, the gap between nominal and realized openness, or attractor collapse of LLM mutation (round3/REPORT ◐).
- **No simulator or world-model channel.** Dream-RSI is treated only as exploration-policy revision. There is no analysis of training in a learned or wrong environment and transferring to reality, and no split between a trace channel and a fitness channel.
- **Missing pre-cutoff works:** "Two Perspectives on RSI" 2610.14625 ◐ and the Improvement by Inheritance submission ◐.
- **Post-cutoff work it could not cite:** TRUST 2611.10787 (greedy gate accepts 45.6% inferior updates, cut to 2.8% ◐), "Is RSI Safe?" 2611.10261 ◐, SelfImprove-Bench.
- **No quantitative synthesis.** Fig. 8 and the joint-evidence matrix are qualitative codings. There are no rates, effect sizes or meta-analysis. Most numbers come from single studies.
- **No linguistic or register drift.** Stylistic and register change in the model's own outputs across self-training is not considered. A style paper (Stylitruth) is in the bibliography but never discussed in the text ✓.

**Relevance to Dan's ideas**
- **Taught-Not-Selected.** The survey's stage split maps cleanly onto Dan's two channels: S1 diagnosis (contaminated experience or feedback is "taught") and S3 evaluation (misleading acceptance scores are "selected"). Its own supporting cases are mostly on the teaching side:
  - "Reusing an unchecked account as E_{t+1} or F_{t+1} can preserve a mistaken diagnosis" (§6.2.1);
  - OEP's over-generalized rules and contextual drag (§3.2.1, §5.2.1);
  - Roesner & Kohno, where poisoned benchmark tasks led the meta-agent to *write an instruction* that persisted (§5.4.1).
  
  The survey never separates the two channels causally. Cite it as framing, and position the 2×2 as the causal test its S1-vs-S3 taxonomy implies. Novelty survives.
- **Propose/Propagate/Pay-off funnel.** The survey's meta-improvement claim needs "an observed revision from U_t to U_{t+1}, subsequent reuse of the revised mechanism, and evaluation of the updates it produces" (§3.1.2). Its coding protocol records "modification, retention, reuse, and downstream evaluation separately" (App. A.4). That is conceptually the funnel. **Scoop risk on the framing: medium.** Cite it, and differentiate by *measuring stage rates* and the gate design, which it never does. Two items worth borrowing:
  - its amortized total-cost rule for matched compute (§7.2);
  - the 4-of-26 statistic from Chen 2026j.
- **Meta-Overfitting.** The survey asks for evaluation "beyond development conditions" to separate transferable quality from specialization (§3.3.2), and notes that policies co-specialize to their skill bank (Feng 2026). It has nothing on improvers overfitting to their home domain. Treat it as a supportive citation. Hyperagents' transfer test remains the published counter-result to beat.
- **Meta-reward.** The survey covers Evo2's trained evolver, Recursive Reward Improvement and grader drift. It insists that U-revisions be judged by the quality of later updates through paired continuations, not by task score. That backs scoring at order 1, but it never compares meta-rewards. The gap is open.
- **Openness.** Its "modification boundary" vs "control boundary" (§6.1.2) and the Table 9 axis "what can the system modify?" give useful vocabulary. It has no openness ladder or dose–response analysis. The gap is open.
- **Continual and drift.** Directly useful:
  - S4 interference from GEPA and MetaHarness on four drifting streams (Chen 2026h);
  - EvoPathBench's accumulation, interference and reversal streams;
  - library drift (Zhang 2026k), RealtimeBench, peak-to-final regression reporting.
  
  These support Varying Goals and MetaShift. It never looks at the *rules of improvement* changing. MetaShift's angle stays open.
- **Register snowball.** Not covered (◐ there is only a loose link to PASB sycophancy from retained user claims). It is irrelevant except as an example of retained-state behavioral drift.

**Citable lines:**
- "successful successors alone do not establish that a revised updater improves the process that generates them" (§8.4)
- "A candidate can propose a new safeguard but should not serve as its sole approval authority." (§5.3.5)

---

### From Existing Methods to New Research Ideas: Two Perspectives on Recursive Self-Improvement (2610.14625)

**Read status: ✓ full text.** I read all 46 pages: main text §1–6 (pp. 1–19), the reference list ([1]–[169]), and Appendix A (Table 2 with 107 method rows, Table 3 with about 32 benchmarks, Table 4 with 3 software systems). I scanned the appendix tables rather than reading every row. Local copy: /tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/2610.14625/paper.txt

**Bibliographic**
- **Authors:** Leijun Zhou\*, Yanke Yu\* (equal contribution), Zhihao Liu, Qu Xiang, Tiancheng Hu, Yiru Dong, Yihan Wu, Yangxi Xiang, Xuejun Wu, Xi Chen, Buyue Qian, Yaowei Zheng, Junhao Hu.
- **Organizations:** PrismShadow (most authors; corresponding authors J. Hu and Y. Zheng), Peking University, Zhejiang University, NYU. This is the same group as the GDPevo benchmark [73], which the survey cites as a representative offline benchmark.
- **Date and venue:** arXiv cs.AI v1, 10 Oct 2026 (the manuscript is dated 9 Oct). It is a survey with no venue listed.
- **Length:** 46 pages. About 19 pages of main text, 169 references, and a 16-page catalogue appendix.
- **Links:** https://arxiv.org/abs/2610.14625. A companion "Awesome RSI" site at https://prism-shadow.github.io/awesome-rsi/ lets readers filter methods by dimension.

**Scope & definitions** (✓ §1–2)
- **RSI definition:** a system that "participates in modifying itself, retains these modifications, and uses the revised system in subsequent execution and improvement" (§1). The bar is low: retained memory notes count (A-MEM, Dynamic Cheatsheet), and so do parameter self-edits (SEAL) and code rewrites (DGM).
- **Agentic system:** a language model plus a harness, where the harness is tools, context, memory, skills and executable code (§2.1). The ReAct loop is the task-execution cycle. The RSI loop is a separate outer loop (§2.3).
- **Formalism (§2.3):** A_t = (M_t, H_t), with H_t = (Tools, Context, Memory, Skills, Code).
  - One round is U(A_t; D) = R(A_t, (F∘P)(A_t)).
  - P generates candidates (P for propose, set by d_artifact, d_updater and d_topology).
  - F produces feedback (set by d_source and d_type).
  - R retains (set by d_acceptance).
  - Mode, frequency and scope set when U runs and where its output is reused.
  - D is the 9-tuple of design settings.
- **Recursion:** the "recursive" part appears only in prose. Changes to the models, instructions or code that produce updates "can also alter how subsequent candidates are constructed" [DGM, Promptbreeder, Hyperagents]. Self-referentiality is not a dimension of the taxonomy.
- **Positioning against other surveys:** prior surveys [2609.11873 "Last AI Built by Humans", 2607.07663, Li et al. author-hosted] classify by target, autonomy level or degree of loop closure. The authors argue that a coarse category "does not fully explain its design". The pitch is a tool for generating research ideas, not a field census.

**Taxonomy** (✓ §3, Fig. 2–3, Table 2): 4 groups, 9 dimensions
1. **Update targets**
   - *RSI artifact:*
     - Parametric: SEAL, RISE, TTCS, R-Zero, R-Few, VisPlay, ACE-adversarial, EvoTrainer, G-Zero, MM-Zero, TISD, P²O.
     - Non-parametric, split into:
       - Context: GEPA, ACON, TextGrad, RHI.
       - Memory: A-MEM, ReasoningBank, TTSE, SAGEAgent, KSI, MemRL.
       - Skill: TRACE, Evo-Harness, SkillPivot, SkillAdam, SkillProx, SkillEvoReg.
       - Harness code: DGM, Hyperagents, Gödel Agent, HGM, Ecdysis, RRSI, Meta-Harness, Escher-Loop, ModularRSI.
       - Other artifact: DMoA (router), EnvHarness, EvoTrainer, Harness-of-Harness. This covers data strategies and experiment configurations.
     - Multi-target examples: SkillAdam, SkillSmith (skills + tools), AlgoEvo (memory cards → skills), RRSI (code + context + memory + skills around a frozen model).
2. **Update organization**
   - *Artifact updater:*
     - Self: SEAL, HGM, Gödel Agent, CHIME, DIVE.
     - Teacher (a separate updating role): GEPA's reflective optimizer, Recuris's meta-agent, Hyperagents (classed Teacher in Table 2).
     - Joint (agent proposes, teacher refines or curates): DGM, KSI, ReasoningBank, TTCS.
   - *RSI topology:*
     - Sequential: A-MEM and most others.
     - Tree (one parent per child): DGM, HGM, Red Queen Gödel Machine.
     - Graph (draws on several lineages): Mendel Gödel Machine, GEPA-Merge, Hyperagents, Meta-Harness, Escher-Loop, AIDE².
3. **Update schedule and scope**
   - *RSI mode:*
     - Offline: DGM, SkillAdam.
     - Online: A-MEM, ReasoningBank.
     - Offline→Online: AWM, ACE.
   - *Update frequency:*
     - Step and Event: PILOT's supervisor redirect.
     - Trajectory: ReasoningBank.
     - Batch: SkillAdam, PILOT merges.
   - *Experience scope:*
     - General: DGM's SWE-bench↔Polyglot transfer.
     - Specialized: SkillAdam.
     - Both: AlgoEvo.
4. **Feedback and selection**
   - *Feedback source:*
     - Train/dev set, Benchmark, Environment.
     - Executable verifier: DGM.
     - LLM feedback: SIFT's pairwise comparisons; SkillHEX's hypotheses checked by execution.
     - Human: DemoEvolve, SEEK.
   - *Feedback type:*
     - Score, either Binary or Non-binary.
     - Non-score: Ground truth, LLM-as-a-judge, Other (traces, diagnostics). GEPA uses both score and non-score feedback.
   - *Acceptance criterion:*
     - Artifact validation.
     - Instance result.
     - Benchmark score: GEPA, Ecdysis.
     - Combined metrics: RRSI (gain vs cost vs noise), DGM, ACON.
     - No validation: ACE, A-MEM, Dynamic Cheatsheet.
     - Designer-RSI's rule: admit an edit if at least one replayed case improves and none regress.
- **Rough label counts** (my tally of Table 2 as parsed by pdftotext; ◐ approximate):
  - Updater: Teacher ≈74, Self ≈18, Joint ≈18.
  - Topology: Sequential ≈72, Graph ≈30, Tree ≈6.
  - Acceptance: Benchmark score ≈36, Artifact validation ≈28, No validation ≈28, Combined ≈24, Instance result ≈21.
  - Conclusion: most current "RSI" is a separate teacher updating harness text in a single chain.

**Key claims & findings**
- RSI methods are best read as points in a design space, not as members of coarse categories. Two code-editing methods can differ on who proposes, which versions are reused, and which feedback decides retention (§1, §3).
- **Worked idea (§3.6), the most relevant passage for us.** An acceptance criterion based on immediate task score clashes with harness-code updates that pay off only later. Example: an agent first adds logging (score unchanged), and that later exposes a date-format bug it can then fix.
  - Under Ecdysis's rule (keep a harness only if its training score beats the incumbent), the logging edit "would be rejected before it could support a later repair".
  - Proposed direction: "acceptance criteria that account for a change's contribution to subsequent self-improvement."
  - The section is conceptual only. It has no experiment.
- **§4.1: 11 existing borrowings from deep learning, each with one exemplar.**
  - Optimization → SkillAdam: edit history, plus an edit size that shrinks when gains vary a lot across cases.
  - Backprop → TextGrad.
  - RL (value-based) → MemRL: learned utility values per memory.
  - Meta-learning → SEAL: RL on a self-edit generator, rewarded by post-update performance.
  - Distillation → RISE: extrapolates the before/after RL change to build a teacher.
  - Regularization → RRSI (shrinking number of edits per round, removal of components that keep failing, cost must be justified) and SkillEvoReg (dropout-style masking of skill blocks).
  - Data augmentation → TTCS: a question synthesizer co-trained with the solver.
  - Continual learning → SEEK: replay screening, requiring gains on new cases while capping regression on a fixed historical set.
  - Pruning → SkillProx: ablation-measured utility, then simplification.
  - Ensembles → DMoA: a learned router trained on entropy at test time.
  - Adversarial learning → ACE: solver and test-generator co-evolve.
- **§4.2: new idea seeds, none tested.**
  - Lottery ticket: find the minimal starting resource set that improves as well as the full set.
  - Adversarial dropout: a masking agent that hides the skill content the agent depends on most, to force alternative procedures.
  - Maintaining plasticity: continual backprop, i.e. reinitialize low-utility neurons during parametric RSI on self-verified data.
  - Freeze-Thaw Bayesian optimization: a GP on per-branch improvement curves decides which branches to continue, pause or resume.
  - Evolution of modularity (Kashtan & Alon 2005): switch task goals that share sub-goals so that reusable modules (retrieval vs reporting) emerge.
- **Overall stance:** the paper is constructive and positive. Each idea is argued by analogy plus a toy web-research-agent example. It reports no numbers of its own and no meta-analysis of reported gains.

**Evaluation practices it describes or criticizes** (✓ §5, Table 1, Table 3)
- **Three settings:**
  - Online (improve during evaluation): Evo-Memory, SkillFlow, ContinualSkillBench, AgentStream, PATH-Bench (probes for transfer and forgetting), RSIBench-Data, plus research-agent loops (PostTrainBench, MLS-Bench, AutoLab, EdgeBench, and others) in Table 3.
  - Offline (improve on training tasks, then freeze and test held-out): MemoryBench, GDPevo (rules recombined in held-out workflows), EvoAgentBench, HarnessOpt-Bench, HarnessDev, Evo-Bench (harness-sensitive tasks), S3Gym, VeRO-Bench, SEAGym, SkillLearnBench, Meta-Agent Challenge.
  - Offline→Online: AWM on Mind2Web; Mem²Evolve's memory initialization, including GAIA→other benchmarks.
  - EvoMemBench and Aspire cover both modes.
- **Criticism (§5.4), mild:** studies differ in benchmark, example order and feedback type, so reported gains are not comparable. The authors call for shared protocols within each mode and for reporting task performance together with "the gain over the same system before improvement."
- **Not discussed:**
  - Seed variance, evaluation noise as a selection confound (RRSI is mentioned only in passing).
  - Contamination, reward hacking or evaluator gaming.
  - Cost-normalized comparison (cost appears only as a Table 3 column).
  - Whether improvement compounds across rounds, i.e. evidence of genuine recursion versus one-shot optimization.

**Open problems / future directions it lists** (complete list; the paper has no dedicated section, these are scattered)
1. Acceptance criteria that credit an edit's contribution to later self-improvement, e.g. diagnostic or logging edits (§3.6).
2. More broadly, examine individual dimensions and how they interact to find mismatches (§3, §6).
3. Lottery-ticket-style minimal starting resources for RSI (§4.2.1).
4. Adversarial or targeted masking of skills to expose dependencies and generate curricula (§4.2.1).
5. Plasticity maintenance, e.g. continual backprop, for parametric RSI over many rounds (§4.2.1).
6. Freeze-Thaw Bayesian optimization for allocating effort across branches (§4.2.2).
7. Evolving modular skills and harnesses under goals that vary in modular ways (§4.2.3).
8. Shared benchmarks and protocols per evaluation mode, with gain-over-pre-improvement reporting, aimed at generalization, continued improvement and retention (§5.4).

**What it misses or gets wrong as of Nov 2026** (compared with the project files)
- **No safety or alignment content at all.** The main text has zero mentions of safety, reward hacking, Goodhart, evaluator gaming or drift (✓ grep). Contrast the Trustworthy-RSI survey 2610.35193, "Last AI Built by Humans" 2609.11873 (open problems: autonomy attribution, safe inheritance, reliable verification), Is RSI Safe? 2611.10261, and RSI-Control covert sabotage 2610.27735, all in round1/sweep/frontier-trends.md.
- **Self-referentiality is not a dimension.** The paper cannot separate an edit to the task agent from an edit to the improver. Our core axis (the improver-level, L4 rung in round3/REPORT.md) is folded into Harness code plus a sentence of prose.
  - Labels for meta-capable systems are inconsistent: DGM = Joint, HGM = Self, Gödel Agent = Self, Hyperagents = Teacher, Escher-Loop = Teacher. Under these labels the self/teacher distinction is not usable for meta-editing questions.
- **The §3.6 delayed-credit insight is already known and measured.** The paper gives no quantitative grounding. Round3/REPORT.md documents:
  - HGM's 0.27–0.44 correlation between score-based selection and clade metaproductivity;
  - invalid or do-nothing children scoring the same as others ("Recorded Zero");
  - 49% of validation changes failing to reproduce;
  - the credit-horizon results (MetaSkill-Evolve H-sweep, Gödel Ecology delays).
  
  HGM itself is cited only as a branch-selection method, not as evidence on this point.
- **It misses the empirical failure literature on improvers:**
  - the self-edit drought: SelfImprove-Bench 2611.02797 / IbI ZMmJDeDTXk report about 0% improver edits;
  - meta-overfitting: FreeEvolve 2610.09197, where a Haiku evolver goes 35.0→52.2 on the evolution set but 33.3→32.2 held-out (from our factcheck.md, ✓ there);
  - the ICLR27 wave reporting positive improver transfer (RSI-Harness and others, openreview-iclr27-sweep.md).
  
  The survey treats transfer as one "Experience scope" label (General vs Specialized), not as something to measure.
- **Openness:** it does not cover the openness/constraint crossover (OEO, SimpleEvol, 2610.22289), and it has no "open-ended" or open-ended search dimension (0 mentions).
- **Continual and drift:** continual learning appears only as replay screening (SEEK) and plasticity. There is no treatment of non-stationary environments, concept drift (only CL-Bench's Table 3 entry), or environment-driven improver edits.
- **Coverage skew:** coverage leans to Aug–Oct 2026 harness and skill papers, and the 107 method rows include many minor ones. Teacher-driven, Sequential, non-parametric methods dominate the labels (≈74 of the updater labels and ≈72 of the topology labels by my tally), and the paper does not comment on this imbalance.
- **Feedback channel granularity:** "Feedback type" separates Score from Non-score, but nothing asks which channel carries the improvement or the error. The paper notes GEPA uses both but does not ask which one matters.

**Relevance to Dan's ideas**
- **Taught-Not-Selected:**
  - Support: the paper's own Feedback-type split (Score vs Non-score text/traces, §3.5) is exactly the two channels TNS separates (fitness vs trace). It explicitly cites GEPA as using scores to select and traces to revise.
  - Gap: it never asks which channel transmits simulator or evaluator errors, so TNS fills a gap this taxonomy names but leaves unexamined. TNS can be framed as "the first causal test along the Feedback-type × Feedback-source dimensions".
  - No scoop.
- **Propose/Propagate/Pay-off funnel (proposed→retained→effective):**
  - The P/F/R decomposition (§2.3, eq. 2) is a ready formal vocabulary for the funnel.
  - §3.6's worked example (logging edit rejected by a score gate) is a qualitative version of the delayed-pay-off credit problem (κ_k for Δ^imm≈0 edits, round3).
  - Cite it as independent recognition of the problem. It offers no measurement, so the funnel's quantitative decomposition stays novel.
- **Meta-Overfitting:**
  - Not addressed. "Experience scope" is a design label, not an outcome. §4.1.6 frames overfitting only at the object level (RRSI, SkillEvoReg), i.e. task-set overfitting of skills and harnesses, never overfitting of the improver to its home domain.
  - No scoop. Useful as a foil: "regularization ideas imported so far target object-level overfitting".
- **Meta-reward:**
  - SEAL is framed as meta-learning, i.e. RL on the self-edit generator with post-update performance as reward (§4.1.4). That is the closest the paper gets to meta-reward.
  - §3.6's "acceptance criteria that account for contribution to subsequent self-improvement" is essentially a call for an order-1 meta-reward or credit horizon.
  - Freeze-Thaw BO over branch learning curves (§4.2.2) is close to a HGM-style metaproductivity estimator and could be a baseline arm.
  - No controlled comparison is proposed, so the IBI_GAPS meta-reward paper is not scooped.
- **Openness:** absent. There is no reach/openness axis beyond the artifact list. The Kashtan-Alon modularity idea (§4.2.3) overlaps with the round3 Kashtan & Alon claim (REPORT.md l.1489: modularly varying goals → evolvability and improver self-edits). The survey applies it to skill modularity, not improver edits. Low overlap risk, but cite it.
- **Continual/drift:**
  - Only the replay-acceptance gate (SEEK) and loss of plasticity (Dohare et al.) are covered. The plasticity idea is a possible ablation for parametric self-update threads.
  - The PATH-Bench, AgentStream and CL-Bench entries are useful benchmark pointers for the drift benchmark.
  - No reality-grounded drift benchmark is proposed.
- **Register snowball:** not relevant. Nothing on conversational or linguistic drift.
- **Net:** this is a broad, design-oriented catalogue with no empirical claims, so it creates low scoop risk. Its main use for us is the P/F/R formalism plus the nine dimensions as positioning vocabulary, the 107-method Table 2 as a lookup source, and §3.6 as a citable statement of the delayed-credit problem that our funnel and meta-reward work quantifies.

---

### A Survey of Self-Evolving Agents: What, When, How, and Where to Evolve on the Path to Artificial Super Intelligence (2507.21046)

**Read status: ✓ full text.** I read the whole main body, Sections 1–9, including every table (Tables 1–12) and every figure caption. I checked the reference list with targeted searches but did not read it line by line. PDF from arXiv v4 (16 Jan 2026): `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/2507.21046/paper.txt` (main body only: `.../2507.21046/main.txt`).

**Bibliographic**
- **Authors.** Huan-ang Gao, Jiayi Geng, Wenyue Hua, Mengkang Hu, Xinzhe Juan, Hongzhang Liu, Shilong Liu, Jiahao Qiu, Xuan Qi, Qihan Ren, Yiran Wu, Hongru Wang, Han Xiao, Yuhang Zhou and Shaokun Zhang are equal contributors listed alphabetically. Further authors: Jiayi Zhang, Jinyu Xiang, Yixiong Fang, Qiwen Zhao, Dongrui Liu, Cheng Qian, Zhenhailong Wang, Minda Hu, Huazheng Wang, Qingyun Wu, Heng Ji and Mengdi Wang.
- **Corresponding authors.** Hongru Wang (Edinburgh) and Mengdi Wang (Princeton).
- **Organisations (16).** Princeton / Princeton AI Lab, Tsinghua, CMU, Sydney, SJTU, Penn State, Michigan, Oregon State, CUHK, Fudan, HKUST-GZ, HKU, UCSB, UCSD, Edinburgh and UIUC.
- **Dates and venue.** arXiv v1 2025-07-28; v4 2026-01-16. Published in **TMLR (01/2026)**; OpenReview https://openreview.net/forum?id=CTr3bovS5F.
- **Length.** 77 pages: about 53 pages of body and about 24 of references.
- **Links.** https://arxiv.org/abs/2507.21046 · repo github.com/CharlesQ9/Self-Evolving-Agents

**Scope & definitions (§1–2) ✓**
- **Motivation.** LLMs are "fundamentally static". The authors define self-evolution "not merely by the algorithms used, but by the locus of autonomy": who curates the data and schedules the updates. They frame the work as "a guiding synthesis rather than a review of a fully established paradigm" and claim it is the first such survey. The whole field is placed on a path from LLM to foundation agent to self-evolving agent to ASI (Fig. 1).
- **Formalism (§2.1).**
  - The environment is a goal-conditioned POMDP E = (G,S,A,T,R,Ω,O,γ). The feedback R(s,a,g) can be a scalar or text.
  - An agent system is Π = (Γ, {ψᵢ}, {Cᵢ}, {Wᵢ}): Γ is the architecture/topology, ψ the LLM, C the context (prompt P plus memory M) and W the tools.
  - A **self-evolving strategy** is f(Π, τ, r) = Π′, which takes the trajectory τ and the feedback r as *separate arguments*.
  - The objective is max_f Σⱼ U(Πⱼ, Tⱼ), the cumulative utility over a task sequence, with Πⱼ₊₁ = f(Πⱼ, τⱼ, rⱼ).
- **Operational definition.** An agent "modifies its internal parameters, contextual state, toolset, or architectural topology based on its own trajectories or feedback signals, with the explicit objective of improving future performance." There are three inclusion criteria:
  1. Updates are experience-dependent and target the agent's own limits, not generic data synthesis.
  2. The effect is persistent and changes the policy; transient instruction-following does not count.
  3. There is some autonomous exploration or self-initiated learning.
- **Passive vs active.** Passive learning is triggered only by external data or schedules. Active learning is self-initiated exploration, reflection or structural change. Static distillation is excluded.
- **Spectrum.** Coverage runs from "proto-evolution" (bootstrapping, feedback-driven prompting) to "strong self-evolution" (fully autonomous diagnosis and reconfiguration), with no hard exclusion threshold.
- **Relation to neighbouring fields (§2.2, Table 1).** The authors compare curriculum learning, lifelong learning and model editing/unlearning on seven properties: runtime context, evolving toolset, dynamic tasks, test-time adaptation, active exploration, structural change and self-reflection. Self-evolving agents get ✓ on all seven. Curriculum and lifelong learning are called "problem-setting" paradigms; editing and self-evolving agents are "solution paradigms".
  - Lifelong learning differs in two ways: memory is a training-time replay tool rather than test-time state, and learning is passive rather than active.
  - Self-improving LLMs such as LMSI and Self-Rewarding are classed as "lifelong learning focused on model-centric improvement".

**Taxonomy (Fig. 2, Fig. 3, Tables 2–4), in my words, with the representative papers it assigns**
- **WHAT to evolve (§3).** The "evolutionary loci" are the parts of Π that can be rewritten persistently from experience.
  - **Model**
    - *Policy (weights):* SCA (Self-Challenging Agent), Self-Rewarding Self-Improving, SELF, SCoRe, PAG, TextGrad, AutoRule, SRLM.
    - *Experience (environment/trajectory loops):* SCA, AgentGen, Reflexion, AdaPlanner, SICA, Self-Refine, Learn-by-Interact, RAGEN, DYSTIL.
  - **Context**
    - *Memory evolution:* SAGE (Ebbinghaus forgetting), A-Mem, Mem0 (ADD/UPDATE/DELETE), Memory-R1, MemInsight, REMEMBER, Memento, MemGen, ExpeL, ReasoningBank, Agent Workflow Memory, MUSE, Richelieu, ICE.
    - *Prompt optimization:* APE, OPRO ("ORPO" in the paper), ADO, ProTeGi, PromptAgent, PromptBreeder, REVOLVE, SPO and ACE (Agentic Context Engineering). Multi-node variants: DSPy, Trace, TextGrad, LLM-AutoDiff, MASS, MAS-Zero, EvoAgent, AgentSquare.
  - **Tools**
    - *Creation:* Voyager, Alita, ATLASS, Live-SWE-Agent, CREATOR, SkillWeaver, CRAFT, RL-GPT.
    - *Mastery:* LearnAct, DRAFT ("From Exploration to Mastery"), ToolLLM, Toolformer, Gorilla.
    - *Selection/management:* ToolGen (tools as vocabulary tokens), TOOLMEM, AgentSquare, DGM, COLT, ToolRet, ToolRerank, PTR, SSO.
  - **Architecture**
    - *Single-agent:* node-level (TextGrad, MASS, EvoFlow) and whole-agent (AgentSquare, Darwin Gödel Machine, AlphaEvolve, Gödel Agent, MemEvolve, which meta-evolves the memory architecture).
    - *Multi-agent workflow search:* AutoFlow, GPTSwarm, ADAS, AFlow, MaAS, ScoreFlow, FlowReasoner, plus Agentic Predictor as a cheap evaluation proxy.
    - *Multi-agent policy co-evolution (MARL):* ReMA, GiGPO, MARTI.
- **WHEN to evolve (§4, Fig. 5).**
  - **Intra-test-time** (during the task, aimed at the current instance):
    - ICL: Reflexion, Self-Refine, AdaPlanner, TrustAgent.
    - SFT: SEAL ("Self-Adaptive LM", self-edits trained by RL on downstream reward), TTT-NN, SIFT, TT-SI.
    - RL: LADDER/TTRL, TTRL.
  - **Inter-test-time** (between tasks, aimed at the task distribution; called "the predominant" mode):
    - ICL: AWM, in-context RL.
    - SFT: SELF, STaR, Quiet-STaR, SiriuS, ARIA.
    - RL: RAGEN, DYSTIL, Learning-Like-Humans, WebRL, DigiRL.
- **HOW to evolve (§5, Table 3).** The paradigms are presented as a historical progression: reward-based, then imitation, then population-based.
  - **Reward-based (§5.1, Fig. 6)**
    - *Textual feedback:* Reflexion, AdaPlanner, Agent S2, SELF, Self-Refine, SCoRe, PAG, TextGrad.
    - *Internal/confidence:* CISC, Self-Ensemble, SRSI, self-certainty best-of-N, Self-Rewarding LMs, AgentEvolver.
    - *External:* majority vote (TTRL-style Self-Train LM, MM-UPT, CoVo); environment/tool feedback (SWE-Dev, SICA, Feedback Friction, USEagent, DYSTIL); rule-based (OTC-PO, AutoRule, EGSR, LADDER, RAGEN, SPIRAL).
    - *Implicit:* "Reward Is Enough" (in-context RL from scalar signals), Endogenous Reward, PIT.
  - **Imitation/demonstration (§5.2)**
    - *Self-generated:* STaR, V-STaR, AdaSTaR, Explore-to-Evolve; multimodal STIC and Genixer.
    - *Cross-agent:* SiriuS, SOFT.
    - *Hybrid:* RISE, IoE (confidence-guided selection).
  - **Population/evolutionary (§5.3)**
    - *Single-agent "learning from evolution":* DGM, LLM_GP, CodeEvolve, Self-Referential Graph HyperNetworks, GENOME/GENOME+, EvoLLM-JP (model merging), SOAR.
    - *Single-agent self-play:* Absolute Zero, R-Zero, Socratic-Zero, R-Few, SCA, Sol-Ver, SPELL, SPIN, SPC, STL, EvoTest.
    - *Multi-agent architecture evolution:* EvoMAC, FELA, Puppeteer, Agent0.
    - *Multi-agent knowledge-based evolution:* MDTeamGPT, MedAgentSim, PiFlow.
  - **Cross-cutting axes (§5.4–5.5, Fig. 7, Table 4)**
    - Online vs offline: offline examples are Self-Instruct, WizardLM, OS-Genesis, UI-Genie, GUI-R1, InfiGUI-R1; online examples are Voyager, AdaPlanner, SwiftSage, DigiRL, DistRL, MobileGUI-RL.
    - On- vs off-policy: on-policy examples are Reflexion, GRPO, DAPO; off-policy examples are iterative DPO, RRHF, SiriuS.
    - Reward granularity: outcome (DPO, RRHF, AutoWebGLM, DigiRL, WebRL ORM), process (PRM, Math-Shepherd, AlphaMath, rStar-Math, AgentPRM, Agent Q) and hybrid (GiGPO, SPA-RL).
    - Further axes: feedback type, data source, sample efficiency, stability and scalability.
- **WHERE to evolve (§6, Fig. 8)**
  - **General domain:** memory (Mobile-Agent-E, MobileSteward, Generative Agents), model–agent co-evolution (UI-Genie, WebEvolver, Absolute Zero) and curriculum (WebRL, Voyager).
  - **Specialized domains:**
    - Coding: SICA, EvoMAC, AgentCoder.
    - GUI: Navi/WindowsAgentArena, WebVoyager, ReAP, AutoGUI, MobileUse.
    - Finance: QuantAgent, TradingAgents.
    - Medical: Agent Hospital, MedAgentSim, EvoPatient, DoctorAgent-RL, Learning-to-be-a-Doctor, OriGene, STELLA.
    - Education: PACE, MathVC, i-vip, EduPlanner, SEFL.
    - Other: Arxiv Copilot, Voyager, Agents-of-Change (Catan), Richelieu.
- **Evaluation (§7).**
  - Five goals: adaptivity, retention, generalization, efficiency and safety.
  - Three paradigms by time scale: static (external task-solving or internal-component benchmarks), short-horizon adaptive (augmented traditional benchmarks or built-in dynamic ones) and long-horizon lifelong.

**Key claims & findings**
- **Locus, not algorithm (§1, §3 intro).** A self-evolving agent differs "not by what components it contains, but by which internal states can be autonomously modified."
- **Paradigm trade-offs (§5 intro, end of §5.1–5.3, Table 4).**
  - Reward-based methods "remains sensitive to reward design, often trading stability and safety for adaptability and openness."
  - Imitation "trades exploration and generalization for reliability and sample efficiency."
  - Population/self-play methods buy diversity and open-ended discovery at "higher computational cost and lower interpretability."
  - Sample efficiency ranks imitation (high) > reward (moderate) > population (low).
- **Credit assignment (§3.3–3.4).** The authors name it the core difficulty for tool mastery and for optimizing a node inside a workflow.
- **Tools (§3.3).** As libraries grow there is a "curse of abundance"; DGM-style self-rewriting "blurs" the line between agent and tool.
- **Self-play vs STaR (§5.3.1).** STaR filters static successful trajectories, while self-play learns from relative success "even when no perfect exemplar exists."
- **Numbers the survey cites** (✓ that the survey states them; the underlying papers were not checked):
  - §7.1.6: WebRL 4.8% → 42.4%; SEAgent 11.3% → 34.5% (+23.2 pp).
  - §6.2: WebVoyager self-fine-tuning 30% → 59%; ReAP recovers +29 pp on previously failed queries; Navi "doubling" over 150 Windows tasks.
  - Table 5: SWE-agent with Qwen3-32B uses 440K tokens and 35 calls at 28% success, against GPT-4o-mini at 8.1M tokens and 181 calls at 10%.
- **Self-directedness risk (§7.1.6).** The survey says "Alignment faking rates escalated from 12% to 78% when agents autonomously evolved under conflicting objectives," citing Greenblatt et al. 2024. † My recollection is that the 78% comes from RL training of Claude 3 Opus toward a conflicting objective, not autonomous agent evolution, so this looks like a misframing.
- **Table 11 trends (§7.3.2, ✓ numbers).** The survey calls the comparison "an illustrative snapshot rather than a definitive comparison."
  - Coding/web (Gemini-1.5-pro / Claude-3.5-sonnet): Reflexion vs Learn-by-Interact is 14.3/18.7 and 54.4/60.0 on SWE, and 20.2/25.6 and 40.4/48.0 on WebArena.
  - WebArena-Lite: DigiRL 31.5 vs WebRL 43.0 with GLM-4-9B, and 30.3 vs 42.4 with Llama3.1-8B.
  - GSM8K: ADAS 90.5, AFlow 90.8, ScoreFlow 94.6.
  - MATH: ADAS 80.0, AFlow 76.0, MaAS 84.7.
  - The qualitative reading: architecture-level search with inter-test mechanisms gains more, while intra-test reflection is cheaper but gains less.

**Evaluation practices it describes or criticizes (§7)**
- **Metrics (Table 6).**
  - Adaptivity: success-by-iteration, adaptation speed.
  - Retention: FGT and BWT, defined with formulas.
  - Generalization: aggregate multi-domain score, OOD score.
  - Efficiency: tokens, time, steps, Tool Productivity = Δscore/Σtool cost.
  - Safety: Safety Score, Harm Score, Completion-under-Policy, Risk Ratio, Refusal Rate, Leakage Rate.
- **Proposed cost metrics.** Cost-per-Gain, CPGₜ = Total Costₜ/(Gainₜ+ε), plus a six-way cost taxonomy (Table 5) that adds memory growth and human oversight.
- **Coverage gaps it names (§7.1, Table 8).**
  - Adaptivity benchmarks are narrow and static, and measure improvement "under pre-specified learning protocols" rather than adaptation strategies the agent finds itself.
  - **Retention is "the most underserved dimension".** "The overwhelming majority of existing benchmarks adopt episodic evaluation where agent state resets between tasks," which rules out measuring accumulation or forgetting. Growing a replay buffer past an optimum hurts through context overflow.
  - Generalization is measured on static snapshots: "No existing benchmark examines whether agents preserve generalization breadth as they specialize within domains." Old tasks lose discriminative power, and contamination is a problem.
  - Efficiency: evolution-specific costs (reflection tokens, search time) are rarely broken out, and protocols allow unconstrained optimization with no hard budgets.
  - Safety is checked only per episode, with no tracking of safety drift over evolution.
- **Self-directedness reporting (§7.1.6).** It asks authors to report three things:
  1. Whether tasks and strategies are predetermined, procedurally sampled or self-generated.
  2. The source of the feedback.
  3. How often humans intervene.
- **Standardized protocols (§7.2.4, Table 10).**
  - *Short horizon:* no persistence across tasks, a per-task budget K_short, full per-iteration logs, success-by-iteration curves with AULC, and tokens-per-success.
  - *Long horizon:* full persistence of the system, stage and total budgets (K_stage, K_total), an explicit memory/tool growth policy, replayable trajectories and checkpoints, scheduled retention probes, "evolution decision logs", human-in-loop statistics, FGT/BWT curves, temporal and cluster OOD, CPG and token drift, and safety-drift probes.
  - *Worked example:* EvoAgent meets part of the long-horizon protocol but reports no BWT/FGT, CPG or safety drift.
- **Underserved combinations (§7.3.1).** Long-horizon retention under privacy constraints; architecture adaptation under real-time budgets; the full tool-ecosystem lifecycle; and multi-agent safety under co-evolution, including "social contagion" of unsafe behaviour.
- **Fair-comparison obstacles (§7.3.2).** Inconsistent reporting, different pipelines and rollout budgets, different backbones, and architectures that cannot be compared directly.
- **Benchmarks catalogued (Table 7).** About 32 benchmarks. Only LifelongAgentBench, EvaLearn, LTMBenchmark and (partly) StoryBench are long-horizon. AutoEnv/AUTOENV-36, Benchmark Self-Evolving and TRACE are mentioned as dynamic benchmarks.

**Open problems / future directions it lists (§8 complete, plus §9 and the gaps above)**
1. **Personalization (§8.1).**
   - The cold-start problem, user profiling, personalized long-term memory, tools and generation, and avoiding reinforcing bias.
   - Data governance: data minimization, on-device personalization (Rescriber), memory decay and "right-to-be-forgotten" unlearning, bias/fairness audit loops, and guards against misevolution.
   - Proposed metrics: Personal Adaptation Gain, a retention/forgetting balance including selective-forgetting efficacy, a privacy–utility trade-off with a Data Minimization Score, the share of learning done on-device, Fairness-Drift and Safety-Drift monitors ("standardized versions remain to be developed"), and longitudinal user outcomes (StuLife).
2. **Generalization (§8.2).**
   - Scalable architectures that handle the specialization-vs-generality trade-off at non-linear compute cost.
   - Cross-domain adaptation via test-time scaling and meta-learning, which depends on knowing *when* to spend extra compute.
   - Continual learning, catastrophic forgetting and the stability–plasticity balance under resource and privacy limits ("preventing model drift remains a significant open challenge").
   - Knowledge transfer between agents: agents fail to pass newly learned knowledge to other agents, and models rely on shallow pattern matching instead of world models. Three sub-directions: when transfer works, quantifying its limits, and mechanisms that encourage robust world models.
3. **Safe and controllable self-evolution (§8.3).**
   - Emergent risks:
     - Behaviour drift in the model ("misevolution": self-training forgets safety alignment).
     - Deployment-time reward hacking through memory (e.g. learning to issue unneeded refunds because they correlate with satisfaction; the "Alignment Tipping Process").
     - Unsafe self-created or ingested tools, privacy leaks and offensive-cyber capability.
   - Guardrails:
     - Sandboxing and static/vulnerability verification of tools, plus runtime defence for MCP-style protocols.
     - Immutable audit trails, versioned safe states and tested rollback.
     - Memory-poisoning defence (A-MemGuard).
     - Continuous monitoring and red-teaming for long-horizon drift (OS-Sentinel).
     - Human approval gates and privacy protection.
     - Pre-update validation against a "golden dataset" of safety prompts.
     - Reward-hacking alarms and goal guardrails.
   - Table 12 turns these into a compliance checklist.
4. **Multi-agent ecosystems (§8.4).** Balancing individual and collective reasoning (over-reliance on consensus); efficient frameworks with dynamic knowledge updating; and dynamic multi-agent benchmarks, since current ones are static.
5. **Conclusion (§9).** Catastrophic forgetting, preference alignment during autonomous evolution, and **co-evolution of agents and environments**.
6. **Evaluation agenda (§7).** Open-ended exploration without fixed objectives; long-horizon adaptation under non-stationary distributions; temporal robustness under drift; adversarial co-evolutionary assessment; budgets enforced during evolution; and long-horizon and co-evolutionary safety.

**What it misses or gets wrong as of Nov 2026**
- **Missing classics** (checked against the reference list):
  - STOP (Self-Taught Optimizer) is absent.
  - GEPA / reflective prompt evolution is absent, even though it became the default harness-evolution baseline in 2026 work (the project files list 2607.14004, 2610.25858 and 2611.07624, all built around GEPA).
  - Huxley–Gödel Machine is absent.
  - The lineage on open-endedness in the Clune/POET/OMNI tradition is absent.
  - Goodhart and model collapse are never named; reward hacking is discussed only under safety.
- **No selection or gate concept.** The words "acceptance" and "winner" never appear, and "gate" appears only as a human approval gate. There is nothing on selection dynamics around accepting a self-edit: winner's curse, gate resolution, seed variance, or matched-compute baselines against plain test-time scaling. The project's selection-dynamics thread (FINAL_REPORT, "Selection dynamics & evaluation rigor") is entirely outside its frame.
- **Object level vs improver level.** It never separates object-level improvement from the improver improving itself. DGM, Gödel Agent, MemEvolve and Self-Referential Graph HyperNetworks are listed, but there is no nested ("improve-the-improver") evaluation. SelfImprove-Bench 2611.02797 (Nov 2026, per project files) and the round-3 finding of a 0% improver-edit "drought" postdate the survey and fall into this gap.
- **No harness-vs-weights or simulator-as-substrate analysis.** World models appear only as co-evolved helpers (WebEvolver, AutoEnv). The survey never asks whether evolving inside a simulator transfers.
- **Already superseded on scope.** Later 2026 surveys cover safety and RSI specifically: 2610.00093 (Safety in Self-Evolving Agents), 2610.35193 (Trustworthy RSI), 2610.14625 (Two Perspectives on RSI) and 2610.29677 / 2610.38266 (Physical-AI RSI), per round-1 LITERATURE.md (◐). The 2026 harness-evolution and continual-drift wave is also missing: Meta-Harness, AHE, HyperAgents, OEO 2608.09629, Are Self-Evolving Agents Continual Learners? 2610.25858.
- **Factual and internal errors** (✓ from the text):
  - Table 10 gives EvoAgent Overall SR 21.80% → 30.29% as a "relative gain ≈ 105.9%". The actual relative gain is 30.29/21.80 − 1 ≈ 38.9%.
  - The alignment-faking 12% → 78% is attributed to "agents autonomously evolved" († likely misframed; see above).
  - Table 3 labels Du et al. 2025 as "SWE-agent" when the paper is SWE-Dev.
  - Table 11 tags ADAS as "Inter-test (RL / SFT hybrid)" although ADAS is meta-agent code search, and labels MaAS as "Mass".
  - Fig. 2 attributes AgentCoder to Dang et al. while the text cites Huang et al., and contains the typo "Finalicial".
  - The introduction maps evaluation to §6 and applications to §7; the actual order is the reverse.
  - The abstract says three dimensions while the title and Fig. 2 have four.
- **Shallow on evidence.** No meta-analysis. Table 11 is explicitly illustrative, and most "Key claims" are characterizations of single papers. The paradigm trade-offs in Table 4 are asserted, not measured.

**Relevance to Dan's ideas**
- **Taught-Not-Selected.** This is the strongest framing hook. Eq. 1, f(Π, τ, r), already separates the trajectory channel τ from the feedback channel r, and TNS's 2×2 (trace source × fitness source) asks which argument carries a simulator's error.
  - The survey's own taxonomy blurs this distinction: it files TextGrad and Reflexion-style reflection under "reward-based / textual feedback", and DGM-style selection under "population".
  - TNS can cite the survey as the standard formalism and show that its "how" taxonomy has no axis for *what enters through traces vs through selection*.
  - It never studies evolution inside simulators, so TNS's substrate question is open.
- **Propose / Propagate / Pay-off funnel.** The survey asks for "evolution decision logs" and for reporting self-directedness and intervention frequency (§7.1.6, Table 10). It never splits an edit's life into proposed → retained → effective, and it has no gate concept.
  - The funnel can be pitched as the measurement that makes its long-horizon protocol actually usable.
  - CPG (§7.1.4) is a ready-made cost denominator for the pay-off stage.
- **Meta-Overfitting.** §7.1.3 says outright: "No existing benchmark examines whether agents preserve generalization breadth as they specialize within domains, or whether knowledge acquired in one domain continues to transfer after hundreds of learning episodes." Quote this in the motivation.
  - Meta-Overfitting extends the point one level up, to whether the *improver's procedure* transfers. The survey never separates the object level from the meta level, which is the delta.
  - §8.2 on the specialization-vs-generality tension also supports the motivation.
- **Meta-reward.** Every reward in the survey's §5.1 taxonomy is order-0, i.e. task performance. The nearest meta-level signals are:
  - SEAL, where the self-edit is rewarded by the downstream performance of the updated model;
  - DGM, whose parent selection mixes score with a novelty bonus;
  - Self-Referential Graph HyperNetworks, where the mutation rate can itself be selected;
  - MemEvolve's bilevel optimization of the memory architecture.
  Nobody rewards successor or improver quality, so the survey reinforces that the "Order-0 Rewards Breed Routers" variant fills a real gap. The "Alignment Tipping Process" and memory reward-hacking risks (§8.3.1) belong in a threats paragraph.
- **Openness (editable-surface ladder).** The Model → Context (prompt, memory) → Tools → Architecture loci, the Table 2 per-method markers and the proto-to-strong spectrum (§2.1) give a citable precedent for ordering editable surfaces.
  - The survey makes no claim about *when* more openness pays. Table 1 even gives self-evolving agents a blanket ✓ for "structural change".
  - The evaluator/gate rung (L5) is missing apart from reward-model co-evolution (UI-Genie, Absolute Zero).
  - The reach × realization × credit law is therefore new relative to this survey. Its §7.3.1 gap "architecture adaptation under operational constraints" is a near-match for the matched-token openness ladder.
- **Continual / drift.** This is direct support for a drift benchmark:
  - "Retention remains the most underserved dimension".
  - The episodic-reset critique (§7.1.2).
  - Table 8 gaps: "Long-horizon adaptation under non-stationary distributions" and "Temporal robustness under distribution drift".
  - The long-horizon protocol's "streamed sequences with non-stationary distributions; versioned tasks" plus scheduled retention probes (Table 10).
  - Its FGT/BWT definitions and the Table 10 protocol can be adopted wholesale as the reporting standard; reviewers will recognize them. It does not separate drift in the *improvement regime* from drift in task content.
- **Register snowball.** Weak relevance. Its "token snowball effect" (Table 5) means context and memory growth amplifying invalid context, not convergence of linguistic register. The closest hooks are in §8.1: "Longitudinal User Outcomes" and the proposed Fairness-Drift/Safety-Drift indices "remain to be developed". At most, cite it as evidence that the survey literature has no metric for longitudinal style or register drift in human–agent dialogue.

---

### A Comprehensive Survey of Self-Evolving AI Agents: A New Paradigm Bridging Foundation Models and Lifelong Agentic Systems (2508.07407)

**Read status: ✓ full text.** I read v2 (31 Aug 2025) end to end: every section from §1 to §9 plus the Figure 2/5 taxonomies, and I spot-checked the reference list. Text is at /tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/2508.07407/paper.txt.

**Bibliographic**
- Authors: Jinyuan Fang\*, Yanwen Peng\*, Xi Zhang\* (equal contribution), Yingxu Wang, Xinhao Yi, Guibin Zhang, Yi Xu, Bin Wu, Siwei Liu, Zihao Li, Zhaochun Ren, Nikos Aletras, Xi Wang, Han Zhou, and Zaiqiao Meng (corresponding author).
- Orgs: Glasgow, Sheffield, MBZUAI, NUS, Cambridge, UCL, Aberdeen, Leiden. The group is tied to the EvoAgentX framework.
- Dates: v1 on 2025-08-10, v2 on 2025-08-31. arXiv lists only these two versions, so there has been no update since Aug 2025.
- Venue: arXiv preprint (cs.AI). No venue is listed.
- Length: 55 pages, of which about 33 are main text and the rest references.
- Companion list: github.com/EvoAgentX/Awesome-Self-Evolving-Agents
- Link: https://arxiv.org/abs/2508.07407

**Scope & definitions**
- **Definition box (§1):** "Self-evolving AI agents are autonomous systems that continuously and systematically optimise their internal components through interaction with environments, with the goal of adapting to changing tasks, contexts and resources while preserving safety and enhancing performance."
- **"Three Laws of Self-Evolving AI Agents" (§1), ranked like Asimov's laws:**
  - I. **Endure**: stay safe and stable during any modification.
  - II. **Excel**: subject to I, preserve or improve existing task performance.
  - III. **Evolve**: subject to I and II, autonomously optimise internal components as tasks, environments or resources change.
- **Four-stage paradigm history (§1, Table 1, Fig. 1):**
  - MOP (Model Offline Pretraining): the model learns from static data through loss.
  - MOA (Model Online Adaptation): SFT, LoRA and RLHF on labels or rewards.
  - MAO (Multi-Agent Orchestration): agents exchange messages and debate with frozen weights.
  - MASE (Multi-Agent Self-Evolving): a population of agents refines prompts, memory, tools and topology "based on environmental feedback and meta-rewards", citing AlphaEvolve and DGM. "Meta-reward" is never defined anywhere in the paper.
- **Self-assessment (§1):** the authors say current systems are "still far from exhibiting the full capabilities required for safe, robust and open-ended self-evolution". In practice the field it surveys is *agent optimisation techniques*.
- **Agent anatomy (§2):** a foundation model plus perception, planning, memory and tool modules. MAS architectures are hierarchical, centralised or decentralised. Communication is structured output, natural language, or standard protocols (A2A, ANP, MCP, Agora).
- **Lifelong vision (§2.3):** the system should (I) monitor its own performance and safety, (II) improve through controlled incremental updates, and (III) adapt prompts, memory, tools and topology on its own.
- **Unified framework (§3, Fig. 3):** an iterative loop with four parts:
  - System Inputs *I*: either task-level {T, D_train}, possibly with synthetic data when no labels exist, or instance-level {x, y, C}.
  - Agent System *A*: single or multi-agent, made of the LLM, prompt, memory, tools, topology and communication.
  - Environment: supplies feedback as task metrics (accuracy, F1, success rate) or LLM-judge proxy scores and textual feedback.
  - Optimiser *P* = (search space S, algorithm H), where H can be rule heuristics, gradient descent, Bayesian optimisation, MCTS, RL, evolutionary search or a learned policy.
- **Formal objective (Eq. 1):** A\* = argmax_{A∈S} O(A; I). The loop "terminates once a predefined performance threshold is reached or convergence criteria are satisfied." EvoAgentX is called "the first open-source framework" to implement this loop, which is the authors' own system.
- **Positioning:** the paper names Gao et al. 2025 (the what/when/how-to-evolve survey) as concurrent work and claims to be more "integrative".

**Taxonomy** (Fig. 5; Fig. 2 places the same branches on a 2023–2025 timeline)

1. **Single-agent optimisation (§4)**
   - 1.1 LLM behaviour optimisation (§4.1)
     - Training-based, SFT: STaR, ToRA, NExT, DeepSeek-Prover, and distillation of o1-style traces.
     - Training-based, RL: Self-Rewarding LMs, Agent Q (MCTS+DPO), Tülu 3 (RLVR), DeepSeek-R1/GRPO, DeepSeek-Prover-v1.5, MSTAR, Absolute Zero, R-Zero.
     - Test-time, feedback-based (outcome or step-level verifiers): CodeT, LEVER, START, CoRT, Baldur, Math-Shepherd, Rewarding Progress (PRMs).
     - Test-time, search-based: CoT-SC, DBS, CoRe, Tree-of-Thoughts, Forest-of-Thought, Graph-of-Thoughts, Buffer-of-Thoughts.
   - 1.2 Prompt optimisation (§4.2)
     - Edit-based: GPS, GrIPS, Plum, TEMPERA.
     - Generative: APE, OPRO, PromptAgent (MCTS), MIPRO (Bayesian optimisation), APOHF (bandits), StraGo, SPO, and Retroformer (an RL-trained prompt policy).
     - Text-gradient: ProTeGi, TextGrad, agent symbolic learning, compound-AI configuration optimisation.
     - Evolutionary: EvoPrompt (GA/DE), Promptbreeder (self-generated mutation prompts).
   - 1.3 Memory optimisation (§4.3), inference-time only by design
     - Short-term: recursive summarisation, MemoChat, COMEDY, ReadAgent, MoT, StructRAG, MemoryBank, Reflexion.
     - Long-term: EWE, A-MEM, MemGPT, NeuroCache, G-Memory, AWESOME, Mem0/Mem0g, HippoRAG, GraphReader, ChatDB, MATTER, AWM, MyAgent, MEM1 (RL), MrSteve, MIRIX, Agent KB.
   - 1.4 Tool optimisation (§4.4)
     - Training-based SFT: ToolLLM, GPT4Tools, STE, TOOLEVO, T3-Agent, Magnet, BUTTON, APIGen-MT, Confucius, Gorilla.
     - Training-based RL: ReTool, Tool-N1, Tool-Star, SPORT, ARPO, ToolRL, SWiRL.
     - Inference-time, prompt-based: EASYTOOL, DRAFT, PLAY2PROMPT, joint prompt+tool-doc optimisation.
     - Inference-time, reasoning-based: ToolLLM DFS, ToolChain\*, Tool-Planner, MCP-Zero.
     - Tool creation and functionality: CREATOR, LATM, CRAFT, AgentOptimizer, Alita, CLOVA.
2. **Multi-agent optimisation (§5)**
   - Manually designed baselines (§5.1): parallel/voting, hierarchical (MetaGPT and similar), and debate, including confidence-gated debate.
   - Multi-agent prompt optimisation (§5.2.1): DSPy Assertions, AutoAgents, MIPRO, PromptWizard.
   - Topology optimisation (§5.2.2)
     - Code-level workflows: AutoFlow, AFlow (MCTS), ScoreFlow (Score-DPO), MAS-GPT (SFT one-shot).
     - Communication graphs: GPTSwarm, DynaSwarm, G-Designer, MermaidFlow, DyLAN, Captain Agent, Flow.
     - Pruning: AgentPrune, AGP, plus G-Safeguard and NetSafe for safety.
   - Unified optimisation of prompt and topology (§5.2.3)
     - Code-based: ADAS, FlowReasoner.
     - Search-based: EvoAgent, EvoFlow, MASS, DebFlow, MAS-ZERO.
     - Learning-based: MaAS (agentic supernet), ANN.
   - LLM-backbone optimisation (§5.2.4)
     - Reasoning-oriented: multi-agent fine-tuning, Sirius, MALT, MaPoRL, MARFT, MARTI.
     - Collaboration-oriented: COPPER, OPTIMA, MaPoRL.
   - Fig. 6 also crosses optimisation space (text, code, graph, distribution) with method (LLM, tree search, RL, gradient) and target (accuracy, efficiency, safety).
3. **Domain-specific optimisation (§6)**
   - Biomedicine
     - Diagnosis: MedAgentSim, PathFinder, MDAgents, MDTeamGPT, MMedAgent, MedAgent-Pro, a graph-feedback diagnosis agent.
     - Molecular discovery: CACTUS, LLM-RDF, ChemAgent, OSDA Agent, DrugAgent, LIDDIA.
   - Programming
     - Code refinement: Self-Refine, CodeCriticBench, LLM-Surgeon, AgentCoder, CodeAgent, CodeCoR, OpenHands, VFlow.
     - Debugging: Self-Debugging, Self-Edit, PyCapsule, Self-Collaboration, RGD, FixAgent.
   - Finance and law
     - Finance: FinCon, PEER, FinRobot, heterogeneous sentiment agents, template-based reporting.
     - Law: LawLuo, Multi-Agent Justice Simulation, AgentCourt (lawyers that evolve through self-play), LegalGPT, AgentsCourt.

**Key claims & findings** (the survey is descriptive and contains no new experiments)
- Real environments drift (user intents, policies, tools), so manually reconfiguring static agents does not scale. This is the stated motivation for self-evolution (§1).
- Most existing work optimises **one component** of A. Joint optimisation is recent: LLM+prompt (BetterTogether) and prompt+topology (MASS, AFlow) (§3.3).
- Outcome-only feedback invites unfaithful reasoning. Step-level PRMs "generally" do better (§4.1.2).
- Generative prompt optimisers explore more widely than edit-based ones. Text gradients turn critique into update directions (§4.2).
- Memory work is shifting from static formats to *dynamic memory control*: deciding what, when and how to store or discard (§4.3).
- SFT on tool use is limited by trajectory quality and coverage. RL generalises better to unseen tools. Tool *creation* is "still emerging" (§4.4).
- For multi-agent systems, a single LLM with a good prompt can match multi-agent debate (citing Pan et al. 2025a). Together with engineering cost, this motivates automated MAS design (§5.1).
- On topology: "even the best prompts cannot compensate for poor architectural choices". Prompts and topology are interdependent, so they should be optimised jointly (§5.2.2–5.2.3).
- As search improves, "the core challenge shifts from finding optimal solutions to defining what optimality means in dynamic multi-agent contexts" (§5.2).
- OPTIMA is reported at "2.8× performance gain with less than 10% of the token cost" (§5.2.4). This is the only quantitative result quoted in the body, and it is a secondary citation.
- LLMs are "not inherently effective team players", so collaboration needs targeted training (MaPoRL) (§5.2.4).
- Domain work has converged on specialised roles, tool grounding, memory of past solutions and reflective self-play (§6).

**Evaluation practices it describes or criticizes (§7)**
- **Evaluation as a training signal (§7 intro):** evaluation is described as a "dynamic feedback mechanism" that feeds optimisation, not an endpoint.
- **Benchmark families (§7.1):**
  - Tool/API: ToolBench, API-Bank, MetaTool, ToolQA, then GTA and AppWorld. Critique: agents **overfit to specific tool schemas** and generalise poorly to unseen APIs.
  - Web: BrowseComp, MM-BrowseComp, WebArena, VisualWebArena, WebCanvas, WebWalker, AgentBench. Critique: reproducibility and generalisation suffer because the web keeps changing.
  - Multi-agent and generalist: MultiAgentBench, SwarmBench, GAIA, AgentBench, Efficient Agents. Critique: metrics are hard to aggregate across tasks, benchmarks risk "overfitting to narrow scenarios", and the field needs holistic leaderboards.
  - GUI: Mobile-Bench, AndroidWorld, CRAB, GUI-World, OSWorld. Critique: hard to standardise, and agents break when interfaces change.
  - Domain: SWE-bench, DataSciBench, MLGym, WorkBench, OpenAGI, SUPER, AgentClinic. Critique: metric definitions are inconsistent.
- **LLM-as-judge (§7.2.1):** pointwise and pairwise modes. It can match inter-annotator agreement, but it is prompt-sensitive and biased, and it misses multi-step reasoning. Proposed fixes are CollabEval and meta-evaluation benchmarks.
- **Agent-as-judge (§7.2.2):** judges the whole trajectory (DevAI). It agrees better with experts and is cheaper, but it is complex to build and has not been shown beyond code.
- **Safety (§7.3):** AgentHarm, RedCode, MobileSafetyBench, MACHIAVELLI, R-Judge, AgentEval, SafeLawBench. The central critique: "most current evaluations are snapshot-based". It calls for "longitudinal, evolution-aware benchmarks" that check safety at every evolution step, and warns that correctness metrics alone can "conceal epistemic risks".

**Open problems / future directions it lists** (complete list from §8)
- Challenges under **Endure** (§8.1.1):
  1. Safety, regulation and alignment. Optimisers put task metrics ahead of safety. Laws such as the EU AI Act and GDPR assume static models. The authors want evolution-aware audits, adaptive licences, provable-safety sandboxes, and legal protocols that track an agent's evolutionary path.
  2. Reward modelling and optimisation instability. Step-level reward models suffer from scarce data, noisy supervision and inconsistent feedback, which leads to divergence. Small changes to inputs or update rules can undermine trust.
- Challenges under **Excel** (§8.1.2):
  3. Evaluation in scientific and specialised domains, where ground truth is missing or disputed.
  4. The efficiency–effectiveness trade-off in MAS optimisation: cost, latency and instability.
  5. Transferability of optimised prompts and topologies, which are brittle across LLM backbones.
- Challenges under **Evolve** (§8.1.3):
  6. Optimisation in multimodal and spatial environments. Current methods are text-only, and the authors say world models are needed.
  7. Tool use and creation. Methods assume a fixed toolset rather than tools that are discovered and co-evolve with the agent.
- Future directions (§8.2):
  - D1. Open-ended simulated environments for fully autonomous closed-loop evolution (MASE).
  - D2. Adaptive tool selection, composition and creation using RL (MAO→MASE).
  - D3. Real-world, interaction-based, **longitudinal** benchmarks aligned with long-term improvement signals (cross-stage).
  - D4. MAS optimisers aware of effectiveness–efficiency trade-offs under latency, cost and energy budgets (MAO).
  - D5. Domain-aware evolution for science, medicine, law and education, with regulatory compliance (MASE).
- Conclusion (§9) adds: scalable optimisation algorithms, lifelong evaluation protocols, safe coordination among heterogeneous agents, and adaptation to unforeseen domains.

**What it misses or gets wrong as of Nov 2026** (gaps are judged against the project files; items marked † are not re-verified this session)
- **Self-referential improvement is almost entirely missing.**
  - DGM (2505.22954) appears only as a citation for "meta-rewards" (§1, §2.3). It is never discussed.
  - Gödel Agent, STOP, SICA and GEPA are absent (grep: 0 hits).
  - The optimiser P is treated as fixed and external, so the survey's framework cannot express improving the improver (order 1). The project's whole line of work (Improvement by Inheritance, HGM, Hyperagents, the ICLR-27 RSI-Harness, MetaEvoForge, FreeEvolve and Looped-RSI wave†; see round2/verify/openreview-iclr27-sweep.md) sits outside its taxonomy.
- **No gate or acceptance-test analysis.** Eq. 1 assumes argmax over a scalar and stopping at a threshold. It does not discuss keep-if-better as adaptive multiple testing (PACE 2606.08106†, Reliable Self-Evolution theory 2609.08175†). It also misses that most RSI systems keep every valid child or force edits (FULLTEXT_VERIFICATION.md).
- **No reward hacking or Goodhart discussion** (grep: 0 hits). The closest it gets is "reward modelling instability" (§8.1.1) and MACHIAVELLI.
- **"Excel" (performance preservation) is stated as a law but never tested.** The survey cites no erosion or forgetting study of evolving agents. Later work fills this: capability erosion 2605.09315†, "Are Self-Evolving Agents Continual Learners?" 2610.25858† (plasticity–stability r = −0.72, interference), and the repair–generalisation gap 2610.22984†.
- **Held-out generalisation of evolved artefacts is barely covered.** Only tool-schema overfitting (§7.1) and backbone transfer (§8.1.2-3) appear. Later results show improve-on-train / lose-on-held-out at the harness level (RSIBench, RRSI, "Do Self-Evolving Skills Generalize?"†).
- **Factual and internal errors** (✓, from the text):
  - §9 lists the framework's parts as "Inputs, Agent System, Objectives, and Optimisers", which contradicts §3, where the third part is Environment.
  - Alita's MCP is expanded as "Multi-Component Program" (§4.4.3). In context this should be Model Context Protocol.
  - Tree-of-Thoughts and CoRe are said to use MCTS (§4.1.2, also misspelled "MCST"). ToT uses BFS/DFS.
  - OPRO is written "ORPO" (§4.2.2).
  - EvoFlow and FlowReasoner share the citation key "Gao et al., 2025a".
  - In Fig. 5, AutoFlow, AFlow, ScoreFlow and MAS-GPT are repeated under "LLM Backbone → Reasoning-Oriented".
  - The survey promotes its authors' own EvoAgentX as "the first" framework.
- **"Open-ended" is a slogan, not an analysis.** There is no treatment of novelty or diversity archives, and no comparison of how open the search space is. AlphaEvolve and DGM are mentioned only in passing.
- **Out of date by about 15 months.** It has none of the 2026 harness/RSI benchmarks, the safety surveys (2610.00093, Trustworthy RSI 2610.35193†) or ACE-style context evolution.

**Relevance to Dan's ideas**
- **Taught-Not-Selected (#1):** Eq. 1 (argmax over O) shows the field's default view: the score channel *selects*. Yet §4.2 is full of methods that *read* traces: StraGo uses success and failure cases, Retroformer summarises failure root causes, ProTeGi and TextGrad use critique as a gradient, and Reflexion stores episodic insights. The survey never separates these two channels, and its safety section (§7.3) only considers malicious *requests*, not corrupted environments teaching the reflector. Cite it as the canonical framing that the idea challenges. ✓
- **Propose/Propagate/Pay-off funnel (#2+3):** this is absent. The optimiser is a fixed (S, H) with no self-edits, and it does not discuss whether proposed edits survive selection or pay off later. Its "loop terminates at threshold" framing hides delayed payoff. This is a clear gap the funnel paper can cite.
- **Meta-Overfitting (#6):** the nearest points are §8.1.2-(3) (optimised prompts and topologies are brittle across backbones) and §7.1 (tool-schema overfitting, narrow-scenario overfitting). This is support at the *artefact* level only. It never asks whether an *improver* transfers across domains, so it is a weak prior and neither scoops nor contradicts the idea. ✓
- **Meta-reward (#5):** useful as an introduction hook. The survey defines MASE partly by "meta-rewards" (citing AlphaEvolve and DGM) but never says what a meta-reward is or what order it scores at. That gap is the one the order-0 vs order-1 comparison fills. ✓
- **Openness ladder (#8):** the survey's "search space S" granularity (prompt → tools → LLM parameters → architecture) matches the ladder's axis. It recognises single-component vs joint optimisation (§3.3) and calls for "open-ended" simulators (D1). But it treats the editable surface only as a design choice and gives no controlled comparison across rungs. Use it as a citation for the axis.
- **Continual/drift (#7 Varying Goals, #9 MetaShift):** drift is the stated motivation (§1) and the "Excel" law is effectively a no-forgetting constraint. Yet the survey admits evaluation is snapshot-based and calls for "longitudinal, evolution-aware benchmarks" (§7.3, D3). This supports the claim that static benchmarks undervalue self-editing. Pair it with 2610.25858† as the empirical follow-up.
- **Register snowball (#10):** tangential.
  - §2.2.2 contrasts natural-language and structured inter-agent messages, and Agora lets agents "evolve their communication methods".
  - OPTIMA (§5.2.4) trains agents for token efficiency *and readability*. It is the closest hook for register and drift under optimisation pressure between models, but the survey does not consider drift away from human readability.
  - Low priority as a citation.

---

### From Question Answering to Task Completion: A Survey on Agent System and Harness Design (2606.20683)

**Read status: ✓ full text.** I downloaded the arXiv PDF (v1, 29 pp.), extracted it with pdftotext, and read §1–§9, Tables 1–10, the figure captions and the reference entries the digest relies on. Text at `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/2606.20683/raw.txt` (layout version: `.../paper.txt`). Table 9 (WebArena) came out with its columns scrambled, so the WebArena numbers below come from the §7.5 prose, not the table.

**Bibliographic**
- **Authors (17):** Jianyuan Guo, Zhiwei Hao, Chengcheng Wang, Cheng Fan, Tingzhang Luo, Hongguang Li, Ying Gao, Hefei Mei, Jiankun Peng, Rongjian Xu, Minjing Dong, Han Wu, Mengyu Zheng, Kai Han, Shiqi Wang, Chang Xu, Yunhe Wang. Corresponding authors: Chang Xu and Yunhe Wang.
- **Orgs:** City University of Hong Kong, University of Sydney, Peking University, and TokenRhythm Technologies (Zheng, Han and Wang).
- **Release:** arXiv v1, 14 Jun 2026, cs.AI. IEEE-journal format. No venue stated, so it is a preprint.
- **Size:** 29 pages, about 262 references.
- **Companion repo:** github.com/ggjy/Awesome-Agent-Engineering
- **Link:** https://arxiv.org/abs/2606.20683
- **Conflict of interest:** the same group (Zheng, Han and others) wrote Claw-SWE-Bench [227], 2606.12344, which the survey cites.

**Scope & definitions** (§1.4, §2)
- **Coverage:** LLM agent systems from 2020 to 2026, including:
  - prompting and workflows;
  - harness and runtime design, plus multi-model orchestration;
  - agent-native training and model–harness co-evolution;
  - domain deployments and evaluation.
  - Neuro-symbolic planning, classical embodied control and non-LLM agents are explicitly excluded.
- **Two levels of definition** (§2):
  - *Functional:* an agent is a goal-conditioned closed loop with five operations: perception, state maintenance, reasoning/decision, action and feedback adaptation. The goal and the environment are not parts of the agent.
  - *Implementation:* A_LLM = ⟨M, H⟩ = ⟨M, I_obs, C, L, I_act, S, V⟩ (Eq. 1). M may be a set of models {M1..Mk}. When |M|>1, the harness also decides which model acts at each step.
- **Harness:** "the runtime infrastructure that shapes what the model perceives, how it acts, and whether its errors are detected and recovered" (§1.1). The term is credited to Hashimoto's blog [22] and OpenAI [7]. The survey says SWE-agent's ACI was the first empirical demonstration that the harness matters.
- **Infrastructure primitives (§2.5).** These are framed as *instances* of harness components, not as separate concepts:
  - tool/function calling → I_act and C;
  - MCP → the C/I_act boundary;
  - A2A → L and I_act;
  - sandboxing/approval → V;
  - agent SDKs and tracing.
- **Table 2 (functional → harness mapping, explicitly many-to-many):**
  - perception → I_obs, C
  - state → S, C
  - reasoning → M, C, L
  - action → I_act, V
  - feedback → V, L, S
- **Central thesis:** quality "emerges from the interaction between model capability, runtime infrastructure, task structure, and evaluation design" (abstract).

**Taxonomy** (three organizing structures)

*A. Four paradigms of agent engineering (§1.2, §4, Fig. 5).* The paper presents this as an evolutionary lens in which all four phases coexist, not as a strict timeline.

1. **Prompt engineering.** Solves "an expression problem: how to ask", not the information problem.
   - Representative: ICL/few-shot [1], CoT [26], self-consistency [27], ToT [28], Self-Refine [97], ReAct [36], automatic prompt optimization (cited only via a survey, [98]).
   - Agentic-reasoning extensions: SGAgent-style collaboration [99–102], reasoning gates and rollback for web agents [103–105], intent-level skills [106].
2. **Workflows and context engineering.** Context is treated as an assembled object, C = A(c1..cn) (Eq. 3), with the goal of optimizing the context-construction functions F (Eq. 4). The paper calls this phase "fundamentally feedforward". Three sub-directions:
   - (a) external information access: RAG [30], FiD, RETRO, Atlas, RAPTOR, GraphRAG, HippoRAG;
   - (b) systematic context management: ACON [125], ARC [126], ContextBudget [127], CAT [128], code-context compression [129];
   - (c) context as an evaluation/optimization target: ContextBench, SWE Context Bench, LoCoBench-Agent, AgentLongBench [130–133], ACE [134], MCE [135].
   - Workflow examples: SGAgent [113], coding-tool configuration [114], tool-trajectory synthesis [115, 116].
3. **Harness engineering.** "closes the loop" with feedback-driven execution.
   - Evidence that the harness matters: SWE-agent [21], NLAH [23] (2603.25723), Meta-Harness [24] (2603.28052), and orchestration/formal-control work [136–138, 139–144].
   - Industry sources: Anthropic [6, 64, 145], OpenAI [7, 39, 146], Magentic-One [37], OpenHands [10]. MCP benchmarks: MCPWorld, MCP-Atlas, MCPAgentBench, OSWorld-MCP [147–150].
   - Design principles: legibility, mechanical enforcement, verification in the loop, explicit artifacts.
   - Two sub-trends:
     - *Multi-model harnesses* (planner–executor–verifier, specialist routing, debate/committee, handoffs [37, 113, 115, 151]);
     - *Learnable harnesses*: NLAH (manual editing), Meta-Harness (search), AHE [40] (2604.25850; trace-driven evolution with the model frozen).
4. **Agent-native training and co-evolution.**
   - *Internalization:*
     - RL for reasoning-to-action: DeepSeekMath, R1, DAPO;
     - training in environments close to deployment: ProRL, WebRL, ComputerRL, Environment Tuning, daVinci-Dev, Kimi-Dev.
   - *Co-evolution / self-improvement:*
     - experience-driven: EvolveR [44], AgentEvolver [45];
     - online / reward-free: Continual Harness [162] (2605.09998), reward-free self-evolution [163] (2604.18131);
     - harness-side: AHE;
     - recursive: SICA [164], DGM [46], Hyperagents [165].
   - Three layers often conflated under "self-evolve" (§4.4): **multi-model** = *who* performs each role; **learnable** = *how* runtime policies are optimized; **co-evolution** = *when/how* the model, harness and improvement loop are jointly updated.

*B. Harness anatomy, six coupled components (§5, Fig. 3).* Each component has a stated dominant trade-off and open problem.

| Component | Responsibility | Representative work | Dominant trade-off |
|---|---|---|---|
| I_obs, observation (§5.1) | Which signals, at what abstraction, how often refreshed | SWE-agent ACI, WebArena, OSWorld | richness vs tractability |
| C, context manager (§5.2) | inclusion, representation, refresh, shared-state scope | MemGPT [31], OpenAI artifact externalization [146], [142, 166–171] | fidelity vs manageability ("monolithic vs managed context") |
| L, control loop (§5.3) | ReAct, plan–execute–verify, hierarchical/multi-agent; stopping | NLAH, Meta-Harness, [38, 108, 136–138] | adaptability vs stability |
| I_act, action interface (§5.4) | granularity, schema, routing (MCP), governance | SWE-agent, MCP, tool-failure diagnostics [173], ET-Agent [174], ToolTok [175] | flexibility vs controllability |
| S, state/artifact store (§5.5) | granularity, scope, form, update policy | MemGPT, checkpoint/diff stores, LoCoMo [172] | completeness vs usability |
| V, verification & governance (§5.6) | tests, verifier models, gates, sandbox, rollback, budgets | AI Committee [151], act-or-refuse [176] | autonomy vs robustness |

§5.7 covers cross-layer coupling:
- I_obs↔C: richer observations raise compression cost.
- I_act↔V: more expressive actions need stronger governance.
- S → C and V: what is stored determines what can be resurfaced and what evidence verification has.

*C. Task pressure profiles (§6).*
- **Table 4, complexity levels and bottlenecks:**
  - L1 single-step → C, V
  - L2 multi-step → I_act, S, V
  - L3 long-horizon → C, S, L. L2→L3 is called "the most consequential transition".
  - L4 open-ended → V, L
- **Other axes:** environment type (terminal/repo, browser/desktop, knowledge, physical, social) and autonomy level. Autonomy is called a "multiplier on harness requirements".
- **Domains (§6.2):**
  - SE = verification-dominant;
  - web/GUI = grounding-dominant;
  - science = synthesis-dominant (ChemCrow, BioDiscoveryAgent);
  - medicine = safety-dominant;
  - embodied = control-dominant.
- **Table 5 rules:**
  - long horizon → checkpoints and artifacts (C, S, L);
  - partial observability → structured observations (I_obs, C, I_act);
  - strong oracle → verifier/repair loops (V, L);
  - weak oracle → provenance/review/approval (V, C, S);
  - irreversible actions → sandbox/gates/rollback (V, I_act);
  - high autonomy → logging/budgets/controllers (V, L, I_obs).

**Key claims & findings** (✓ = checked in the full text)
- ✓ §3.1 Diminishing returns. Llama-3.1 70B→405B raised training compute from 7.0M to 30.84M H100-hours but gained only +2.6 MMLU, +2.6 MBPP-EvalPlus and +1.7 GSM8K. Frontier GPT/Claude/Gemini scores bunch together on MMLU-Pro and GPQA-D (Fig. 4).
- ✓ §3.2 Measurement boundary. Static, short-horizon benchmarks cannot capture trajectory-level properties. The survey cites the METR time-horizon metric [95].
- ✓ §7.3 SWE-bench Verified (Table 7), observational synthesis:
  - *Same harness, model upgrade:* SWE-agent+tools goes from 49.0 (Claude 3.5 Sonnet) to 73.2 (Opus 4). mini-SWE-agent goes from 52.8 (3.7 Sonnet) to 76.8 (Opus 4.5).
  - *Same model, different harness:* GPT-4o ranges 23.2 (SWE-agent) to 38.8 (Agentless). Claude 3.5 Sonnet ranges 33.6 to 53.6 (PatchPilot).
  - *Scaffold complexity does not predict effectiveness:* mini-SWE-agent ("roughly 100 lines") scores 76.8 against 77.6 for OpenHands with Opus 4.5.
  - *Vendor gap:* vendor scaffolds beat the best open scaffold by about 2–4 pts (Opus 4.5: 77.6 vs 80.9; Gemini 3 Pro: 74.2 vs 76.2). The authors attribute this to scaffold decisions.
- ✓ §7.4 Terminal-Bench 2.0. Data: the leaderboard plus 75 public HF submissions (32,604 trials), of which 48 strictly match visible entries.
  - Cost-field coverage is thin: token fields cover 45.0% of trials and dollar-cost fields 15.2%.
  - Same model, different harness:
    - GPT-5.3-Codex: 64.7 (Terminus 2) to 78.4 (SageAgent);
    - Opus 4.6: 58.0 (Claude Code) to 76.4 (Meta-Harness);
    - Gemini 3.1 Pro: 59.4 (Gemini CLI) to 80.2 (TongAgents).
  - Across the 20 models with ≥3 harness results, the median within-model range is 13.6 pts, and 14/20 vary by ≥10 pts.
  - Model upgrades also lift scores within a fixed harness (Codex CLI: GPT-5 49.6 → GPT-5.5 82.2).
  - Resource profiles (Table 8): Meta-Harness on Opus 4.6 uses a median 755K input tokens per trial against 79.4K for Terminus 2, while cutting the timeout rate from 18.2% to 7.9%.
- ✓ §7.5 WebArena.
  - GPT-4o goes from 13.1 model-only to 54.6 with WebOperator, a 41.5-pt span.
  - Llama-3.1-8B goes from 5.6 to 48.5 (AgentSymbiotic).
  - Best reported: WebTactix + DeepSeek V3.2 at 74.3.
  - Fixed BrowserGym harness: Gemini 3 Pro 51.2 down to GPT-4o-mini 13.6.
  - The survey recommends WebArena Verified for high-confidence claims.
- ✓ §7.6 Cross-benchmark lessons:
  - match the harness to the oracle;
  - "Autonomy and complexity are not monotonic goods";
  - model–harness compatibility is an empirical property of the pair;
  - scores are conditional on runtime configuration;
  - move toward value-aware evaluation.
- ✓ §4.4 / §8.2 position: internalization "shifts the division of labor rather than removing the harness". The harness becomes "a training environment, evidence pipeline, verifier, and governance layer".

**Evaluation practices it describes or criticizes**
- **Evaluation pipeline (§7.1):**
  - benchmarks as task specifications (Table 6, 17 benchmarks including OS-Harm, LoCoMo, MLE-Bench, GAIA and Claw-SWE-Bench);
  - execution and trace infrastructure (SWE-agent, OpenHands, Repo2Run, R2E-Gym, HAL);
  - judgment and attribution (G-Eval, MT-Bench, LLM-as-judge risks: bias, inconsistency, evaluator drift);
  - continuous evaluation tooling (LangChain eval, DeepEval, RAGAS, lm-eval-harness).
- **Main criticism:** leaderboards are "score-centric".
  - Six dimensions it proposes (§7.2): task success, reliability, efficiency, latency, safety, process quality.
  - Supporting work it cites: Kapoor et al. "AI agents that matter" [230], CLEAR [231], ReliabilityBench [232], procedure-aware "corrupt success" evaluation [233], CostBench [260].
- **Reporting requirement (§7.6):** at minimum, report model version, harness identity, tool privileges, retry/timeout policy, environment, token/API usage, and trace/verifier metadata. §8.1 adds tool calls, retries, P95 latency, recovery behavior and policy violations.
- **Formal proposals (§8.1):**
  - a constrained value-aware objective (Eq. 6): maximize E[V(τ)·P_succ·Q̄_proc] subject to a cost budget, a latency quantile, a risk bound ε and a k-run reliability floor ρ;
  - a "value-density" family VD_{α,β,γ} (Eq. 7) with multiplicative cost, latency and risk penalties;
  - simple derived reports such as "cost per effective success".
  - None of these is applied to data in the paper.
- **Honest caveats about its own §7 analysis:** the comparisons are observational, not factorial. Vendor rows are "upper-envelope" evidence. Reasoning-effort settings differ across rows. Monetary cost is not used for cross-harness claims.
- **Not covered:** contamination is mentioned only in passing (§1). Reward hacking appears only as a TB2 leaderboard integrity rule. There is no discussion of seeds or variance beyond TB2's k=5, and no compute-matched comparison.

**Open problems / future directions it lists** (complete)

*Per-component open problems (§5):*
1. Observation interfaces that stay faithful and decision-useful under partial observability, multimodal state and long trajectories (§5.1).
2. Keeping summaries faithful and state intact at bounded context cost, especially when context repair interacts with verification and recovery (§5.2).
3. Control policies that stay robust across horizons and domains without making delegation, verification and recovery prohibitively expensive (§5.3).
4. Action abstractions that stay expressive and governable when coupled with verification, sandboxing and recovery over long horizons (§5.4).
5. State fidelity under rollback, delegation and memory reuse without accumulating drift or stale information (§5.5).
6. Selective, cost-aware verification and governance that tells recoverable local errors apart from task-level collapse without over-triggering (§5.6).

*§8 Outlook* (the abstract lists four challenges, "value-aware evaluation, safety, harness generalization, and model–harness co-evolution", but the body has three subsections; safety is folded into 8.1 and 8.2):

7. **§8.1 From score to value-aware optimization.** Harness engineering framed as resource allocation. Benchmarks should report cost, tool calls, retries, P95 latency, recovery, policy violations and auditability.
8. **§8.2 Learning to verify, recover and adapt.**
   - A constrained self-evolution loop: (θ_{t+1}, φ_{t+1}) = VerifyRetain(U(θ_t, φ_t, E_t)) (Eq. 8). VerifyRetain keeps an update only if it passes held-out tasks, regression tests, process checks and safety constraints.
   - Self-evolution must be "observable and falsifiable" (the AHE lesson).
   - Model–harness co-evolution: better harness → better traces → the model internalizes patterns → a different harness becomes optimal.
   - Named risks: "benchmark overfitting, incorrect failure attribution, stale memory, and unsafe runtime modification".
   - Prescribed safeguards: held-out evaluation, ablations, audit logs, rollback, and human approval for high-impact changes.
9. **§8.3 Harness generalization vs specialization.**
   - Design: a reusable substrate (tracing, sandbox, permissions, artifacts, budgets, routing, protocols) plus domain adapters (observations, actions, verifiers, memory policy, recovery). "protocol standardization is not the same as harness generalization."
   - Evaluate transfer across task distributions, model families and adapters using:
     - same-model / different-harness runs;
     - component ablations and adapter swaps;
     - held-out and cross-domain tasks;
     - runtime profiles.
   - For self-evolving harnesses, separate benchmark-specific tuning from reusable gains.
10. **§9 Conclusion.**
    - Reliability is still below deployment needs.
    - Evaluation is only partly aligned with real use.
    - The boundary between model design and system design is "still being renegotiated".

**What it misses or gets wrong as of Nov 2026**
- **Errors and overclaims (✓ checked):**
  - The abstract promises four open challenges; §8 delivers three (no dedicated safety section).
  - Broken citation "Ning et al. [?]" in §1.3.
  - §7.3 calls 49.0→73.2 "a 24% increase"; it is 24.2 *points*.
  - Table 7 merges "Opus 4 / 4.5" into one row family, yet §7.3 uses it for within-family claims.
  - Attributing the 2–4 pt vendor gap to "scaffold-level decisions … not to differences in model capability" contradicts its own caveat that vendor and reasoning settings are not controlled.
  - The §8.1 objectives are proposed but never computed on the TB2 logs it collected.
- **Missing pre-June prior art** (zero hits in the full text): GEPA, ADAS, Gödel Agent, DSPy, TextGrad, AFlow and Promptbreeder are not cited. The learnable-harness branch is therefore only NLAH, Meta-Harness and AHE, and the prompt/workflow-optimization lineage that most 2026 harness evolvers build on is invisible.
- **Missing companion surveys:** it does compare itself with three harness surveys (Meng et al. [55]; Li et al. "Agent Harness Engineering: A Survey" (ETCLOVG) [56], OpenReview; Ning et al. "Code as Agent Harness" [57], 2605.18747). These are the other "survey papers already in the field" the user asked about, and worth reading next.
- **Gate assumption:** the VerifyRetain abstraction assumes self-evolution is gated. The project's full-text check (FULLTEXT_VERIFICATION.md, Meta-Harness 2603.28052) found that Meta-Harness's only gate is interface validation and every candidate is appended. ONE_PAGERS.md notes that most published systems keep every valid candidate and only bias parent sampling. The survey does not see that "keep-if-better" is mostly a fiction.
- **Post-June evidence, by theme** (from round1/LITERATURE.md and round3/REPORT.md; ◐ unless verified there):
  - *Controlled model–harness factorials.* Finding the Right Fit 2610.00917 tests 66 model×harness configurations and finds model rankings reverse across harnesses. This is the controlled version of §7's observational claim.
  - *Gains decomposed.* HDA 2610.22466 attributes 54% of harness-optimization gains to overfitting (O) and 14% to test-time compute (T). Elicitation or Expansion 2610.27889 finds the gains are mostly elicitation. These contradict the survey's reading of the Meta-Harness 76.4 on TB2 as a harness-design win.
  - *Cost.* The RSI Tax 2611.13524 finds a bounded-note baseline matches Meta-Harness, EvoSkill and RRSI at 66–71% fewer tokens, and verification is 75–94% of RSI cost. This is a concrete value-aware result the survey only hypothesizes.
  - *Continual / drift.* Harness Continual Learning 2608.19013, Are Self-Evolving Agents Continual Learners? 2610.25858 (plasticity–stability r = −0.72), Can Coding Agents Sustain Continual Evolution 2611.03219, Do Agent Optimizers Compound? 2607.14004 (gains stall in round 2), and Masked Erosion 2610.38392 (gates miss deleted safeguards). The survey has no notion of harness-level forgetting at all ("forgetting": 0 hits).
  - *Portability and upgrades.* One Recipe Many Harnesses 2608.10178, Memory-survives-upgrade 2609.05339, CHART 2609.22247, MaD-RSI 2610.28201. These speak directly to §8.3 generalization but post-date it.
  - *Co-evolution methods.* VACE 2609.37105, Self-HPO 2610.22791, CoHARL 2610.36469, HERD 2610.31401, and the theory paper Your Policy Is Secretly a Harness 2610.37255. Its Phase 4 is now a crowded method space, not "emerging".
  - *Data scaling for evolvers.* SSR 2610.28815 finds evolvers degrade as the task pool grows; ActiveSaddler 2610.00906 covers curriculum.
- **Absent topics:** simulators or world models as a harness training or evaluation substrate; improver-level (meta) self-modification measurement; and the linguistic or register side effects of agent loops.

**Relevance to Dan's ideas**
- **Taught-Not-Selected (simulator-scored reflective evolution; trace channel vs fitness channel).**
  - *Not scooped:* the survey has no simulator or world-model scoring of harness evolution.
  - *Useful framing:* "traces should be distilled into evidence" (AHE lesson, §8.2) and the E_t evidence set in Eq. 8 split naturally into the trace (teaching) channel vs the VerifyRetain (selection) channel. TNS can cite Eq. 8 as the standard abstraction and show the two channels are separable and differently damaged by simulator error.
  - *Risk named but unmeasured:* "benchmark overfitting … incorrect failure attribution" (§8.2).
- **Propose / Propagate / Pay-off funnel (RSI self-edits).** Eq. 8 collapses the funnel into one retain step. The survey names SICA, DGM and Hyperagents as "the improvement mechanism itself may become modifiable" but gives no edit-rate or propagation data. The funnel is a direct empirical decomposition of its U → VerifyRetain loop. Cite §4.4's three-layer distinction (who/how/when) to place the funnel at the "co-evolution of the improvement loop" layer.
- **Meta-Overfitting.** The survey flags overfitting twice: Table 10 says Meta-Harness "can overfit to benchmark-specific feedback", and §8.3 warns that a specialized harness may "overfit to a benchmark-specific surface". Neither is quantified, and both are at the harness level, not the improver level. Meta-Overfitting supplies the improver-level version. Pair it with HDA 2610.22466 as the quantitative anchor the survey lacks.
- **Meta-reward.** §8.1 (Eq. 6/7) is a hand-specified deployment utility, not a learned or evolving reward for the improver. It is a ready-made "outer objective" that a meta-reward paper can contrast with: fixed V/C/L/R weights vs an improver-side reward that changes with the harness. The Q_proc term (verifier use, recovery, provenance) is close to a process meta-reward and is worth borrowing as a baseline.
- **Openness (editable-surface breadth law).**
  - *For:* §8.3's substrate-vs-adapter split is a qualitative statement of which surfaces should be open (adapters) vs fixed (substrate). §6's pressure profiles predict *where* openness should pay: on the bottleneck component for the task. This matches the round-3 claim that "openness pays exactly where the environment places the cause" and can be cited as prior qualitative support.
  - *Against:* the survey offers no randomized test, so the openness law's experimental design stays novel.
- **Continual / drift.** The survey uses "drift" only for within-trajectory state drift (§5.3, §5.5), never for distribution shift across deployments. Its "co-evolution over deployment" (§4.4) assumes compounding without evidence. The continual-benchmark thread fills that gap, and 2607.14004, 2610.25858 and 2611.03219 already show the assumption fails. Cite the survey as the stated optimistic prior.
- **Register snowball (Claudish).** Not relevant. It covers no language, register or style effects of agent loops and does not touch human–model chat dynamics.
- **Practical use:**
  - Cite it as the mainstream June-2026 framing (model+harness, the four phases, VerifyRetain).
  - Reuse its TB2 public-log methodology (75 submissions, 48 strict matches, a 13.6-pt median within-model spread) as a motivating statistic for harness sensitivity. It is observational only.
  - Use its reporting checklist (§7.6/§8.1) as the minimum reporting standard reviewers will expect.

---

### Safety in Self-Evolving Agents: A Survey (2610.00093)

**Read status: ✓ full text.** I read all of it: sections 1–10, Appendices A–E, and every table (1–13). The extracted text is at /tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/2610.00093/paper.txt. All numbers below come from that text and carry section or table references. Anything taken from project files is marked as such.

**Bibliographic**
- **Authors:** Jiahao Chen, Zhou Feng, Oubo Ma, Yichen Yan, Ruixiao Lin and others, 35 authors in total. The last author and corresponding author is Shouling Ji (sji@zju.edu.cn). Tianyu Du is second to last.
- **Organizations:** led by Zhejiang University, with HUST, Xi'an Jiaotong, NUDT, SJTU, Georgia Tech, USTC, NTU, OPPO Research, Tianjin, Chongqing, Alibaba Group and Rakuten.
- **Dates:** arXiv v1 is dated 8 Sep 2026 (cs.CR). The title page says "Date: October 2, 2026". The corpus was frozen on 4 Sep 2026.
- **Venue:** preprint only; no venue is stated.
- **Length:** 80 PDF pages. The main text is about 56 pages, Appendices A–E run to p. 62, and 272 references follow.
- **Corpus:** a "systematic scoping review" with a coded pool of 683 papers (App. A). It was not preregistered and had no dual coding. Only 51 of the 683 placements are high-confidence.
- **Related work by the same group:** the MLAS survey [107] (Lin … Ji, "Safety in self-evolving LLM agent systems: Threats, amplification…").
- **Link:** https://arxiv.org/abs/2610.00093

**Scope & definitions**
- **Self-evolving agent.** An agent is self-evolving when its own observations, actions, feedback or execution traces update reusable state *inside the agent loop or runtime*, so that later behavior changes (§1, §2.1).
  - Stateful agent < adaptive agent < self-evolving agent.
  - A RAG system over a static corpus is "adjacent". An agent that "writes, revises, selects, or inherits state that later steers action is central."
- **Core reframing.** "When experience becomes reusable state, past events become future causes" (Abstract). The object of safety moves from a single response, to a single action, to "the evolution of influence across state transitions" (§1, Fig. 3).
- **State and reusable influence.** *State* is any information-bearing object that can be reactivated later. *Reusable influence* is that object's capacity to condition later behavior (§2.1).
- **Formal loop (§2.2).** The agent is 𝒜_t = (Θ model, ℳ memory/context, 𝒯 tools/skills, 𝒲 workflow/routing). Each step runs observe → trace τ → feedback f → proposal set Δ_t → evolution policy π^evo picks δ★ using a utility proxy û_t and an authorization condition c_t → update operator 𝒰 commits, revises, ignores or quarantines.
- **Selection semantics.** Selection is the pre-transition choice of which candidate to retain. *Feedback semantics* means a response reshapes later candidate sets.
- **Activation authority.** The right of a stored object to influence a later decision. This is distinct from storage permission (§2.3). The authors relate it to privilege escalation and TOCTOU.
- **Transition test.** Does the operation preserve, or explicitly downgrade, the attributes that downstream components need?
- **Promotion test.** Moving episode evidence into a more persistent, general or action-bearing role is safe only if all of these hold: c_t authorizes the new role, the attributes stay valid in the target context, counterevidence was not discarded, and source→descendant lineage can still be recovered.
- **Safety attributes (App. B):** provenance, authority, scope, privacy, freshness, budget/liveness, auditability, recoverability.
- **Authority ladder (App. B, Fig. 6):** Observation → Evidence → Task Context → Procedure → Action. Crossing upward without a gate is "safety attribute laundering".
- **Running example:** the July 2026 OpenAI–Hugging Face agent intrusion during an ExploitGym evaluation (§2.3, App. C Table 12). The authors explicitly call it "an adjacent infrastructure case, not direct evidence of an endogenous agent-state update."

**Taxonomy: SAVER, S × A → V → E → R, with a feedback arrow from R back to selection (§2.4, Fig. 2)**

- **S: Substrate**, where reusable influence lives (§3; compared on persistence, observability, authority, scope, reversibility).
  - Prompt/context: ReAct, AgentDojo, Signed-Prompt, When Context Gets Root [62], Agent Context CPE [105] (M-CPE role raise vs X-CPE scope raise), Trojan Knowledge, One Turn Too Late, MT-AgentRisk.
  - Long-term memory: MemGPT, SimpleMem, Panini, A-Mem, Darwinian Memory, stealth memory injection [260], GhostWriter [193], FragFuse [163], AgentPoison [25], PASB [131], AuthMem-Bench [249], PS-Bench [53], EvoBreak [229], PersistBench [155], RBI-Eval, MemConflict, SSGM, Governed Memory, GLOVE.
  - Retrieval/knowledge: agentic RAG, AgentPoison, FragFuse, InjecAgent.
  - Tool/API/interface: Progent, MCPShield, MCP Safety Audit, prompt tool selection, STAC, LivePI, IPIGuard.
  - Skill/procedure: Voyager, PoisonedEvolution [21], SkillJack [240], ColluSkill [246], SkillMisevo-Bench [132], EVOMAL [218].
  - Workflow/planner/topology: self-evolving-agent surveys [8], EvoMap, Traversal-as-Policy, HarnessSafe [255], MADRA, ControlValve, Agents of Chaos.
  - External artifacts/shared state: MAPLE-Guard [224].
  - Model/policy: LoRA/PEFT, fine-tuning safety [159], Safe LoRA, KVEraser, PEFT Trojan, feedback backdoors, dormant behavior, backdoor survey.
  - Governance band: audit logs, quarantine lists and rollback receipts are themselves adaptive state that needs governing (§3.9).
- **A: Adaptation**, seven operation families (§4).
  - Add: Reflexion, MemGPT, Trajectory-Informed Memory, FORGE, PASB, GhostWriter.
  - Abstract: AuthMem-Bench, "Safe to Check, Unsafe to Use"/Relink [119], OEP [202].
  - Activate: PS-Bench, PerMemSafe, InjecAgent, CaMeL, DRIFT, ControlValve.
  - Modify: Beyond Heuristics, EvolveMem, GLOVE, SRM, MemGovern.
  - Forget: memory privacy, sleeper memory [156], memory poisoning [184].
  - Propagate: ACIArena, MAPLE-Guard, EVOMAL, Agents of Chaos, Triedman et al. MAS hijack [194], Collective Misremembering [226], Devil's Moltbook [198].
  - Migrate: When Context Gets Root, CPE, PoisonedEvolution, SkillJack, SkillMisevo-Bench, Library Drift, Tool-Drift.
  - Composition paths (§4.8), "local-success generalization": OEP (one promoted experience) → experience-driven safety [266] (aggregate action bias) → EvoBreak (adversarial composition), with AgentPoison as a fourth mechanism. Model-side operations: fine-tune, post-train, merge, edit, unlearn.
- **V: Violation**, six families (§5).
  1. Provenance loss / attribute laundering: AgentPoison, GhostWriter, PASB.
  2. Authority, actionability or boundary escalation, including "promotion failures": AgentDojo, Progent, FragFuse, AuthMem-Bench, harness CPE, state-bound authorization [262], OEP, EvoBreak, PoisonedEvolution, SkillJack, SkillMisevo.
  3. Privacy/purpose failure: memory privacy [197], PerMemSafe, Differential Harm Propensity, PS-Bench.
  4. Persistent descendants / incomplete rollback: SkillJack, sleeper memory, OEP.
  5. Operational integrity (budget, liveness, audit): OWASP, ASB, Agent-SafetyBench.
  6. Model safety regression: fine-tuning safety, PEFT Trojan, dormant behavior, Safe LoRA.
- **E: Exposure**, seven surfaces (§6).
  - Memory retrieval: FragFuse, GhostWriter, AgentPoison, MemoryGraft, OEP, Tool-Drift, PASB, PS-Bench.
  - Tool/action, plus "context-horizon" exposure: AgentDojo, InjecAgent, AuthMem-Bench, HarnessSafe, LivePI, SnapGuard, Task Shield.
  - External artifact.
  - Propagation across agents: ACIArena, Agents of Chaos, Triedman et al.
  - User-facing misattribution: Misattribution Gap [2].
  - Governance blind spots: R-Judge, TrustAgent, GuardAgent, LlamaFirewall, EDDOps.
  - Model deployment.
- **R: Response**, four stages (§7).
  - Preventive governance: admission control (AM-Sentry, ChainGuard, AuthMem labels), promotion gates and longitudinal reconciliation (PoisonedEvolution provenance-diversity gate, SafeEvolve, EVOMAL counter-prompt and signed quarantine), and counterfactual testing.
  - Transition and activation gates: migration control, Progent, Traversal-as-Policy, CaMeL, DRIFT, state-bound authorization, AID-Guard [191], ControlValve, MAPLE-Guard, PS-Bench detection-reflection, TurnGate, KBRA, ToolShield.
  - Monitoring and containment: GuardAgent, TrustAgent, MAGE, EDDOps, HarnessSafe, Safe LoRA, KVEraser.
  - Recovery and contestable closure: descendant rollback, deletion verification, contestability. Response feedback (DARWIN, BraveGuard guard-update loops) is also covered here.
- **Corpus shape (Fig. 1, 577-paper dated subset):** Memory 201, Workflow 224, Tools & Skills 108, Model 44. By terminal family: Provenance Loss 97, Authority Escalation 77, Purpose Failure 55, Incomplete Rollback 9, Integrity 76, Safety Regression 25; Preventive 61, Transition & Activation 20, Monitoring & Containment 136, Recovery & Contestability 21.

**Key claims & findings**
- **Takeaways 1.1 and 10.1.** Unsafe influence need not begin as harmful content. Legitimate state becomes unsafe when adaptation broadens its persistence, authority or scope. Safety must therefore follow changes in permitted use.
- **Takeaway 1.2.** Attack and recovery carry different proof burdens. One path establishes an attack; recovery must cover all future activation sites.
- **Takeaways 1.3, 8.3 and 6.3.** Endpoint ASR, recognition accuracy and block rates cannot tell a stopped event from a repaired cause. Similar endpoint ASRs can stop at different stages (HarnessSafe: OpenCode vs Hermes, §8.4).
- **Takeaway 2.1.** "Utility Does Not Authorize Promotion." Omitting constraints or counterexamples during selection is "a possible mechanism of endogenous drift, not a claim that utility and safety are necessarily opposed."
- **Takeaways 2.2 and 4.2.** Storage and activation need separate authority, and "Relevance Is Not Permission."
- **Takeaway 2.4.** Capability evidence is not safety evidence.
- **Takeaway 4.1 (§4.2).** Abstraction needs a new safety check.
  - AuthMem-Bench: consolidation can preserve a proposition while erasing its source-use constraint ("authority collapse").
  - Relink: a summarizer recombines individually benign fragments into an actionable instruction.
- **Takeaways 4.3 and 4.4.** Propagation multiplies activation sites; migration changes the authority regime. The two are distinct, and one transfer can do both.
- **§4.6, EVOMAL.** After the 8 planted seeds are removed, Qwen3 reaches 68% agent self-poisoning rate (ASPR) at round 5, Gemma4 29.4%, and Devstral stays infected. DeepSeek-V4-Pro and MiniMax return to baseline, and GPT-OSS decays. The authors call this "conditional evidence of self-sustaining propagation… not a universal worm claim."
- **§5.2, SkillMisevo-Bench.** Of 21 evolved configurations, 21 author unsafe artifacts, 19 retrieve unsafe skills, 19 show benign-task contamination and 15 retain fresh-session harm.
- **§5.4, SkillJack.** Per its abstract, 80.0% of skill-mediated attacks persist after the source records are deleted.
- **§8.3, SkillJack.** In SkillX, safety detection falls from 98.5% on trajectories to 11.4% on extracted skills.
- **Takeaway 5.1.** Correct content can carry unjustified authority (AuthMem-Bench; harness privilege escalation [62]).
- **Takeaway 5.2.** Privacy can fail at state level before any disclosure.
- **Takeaway 5.3.** A missing trigger does not establish a safe state.
- **Takeaway 6.2 (§6.4).** Content lineage and invocation lineage are different things. Triedman et al. report arbitrary-code-execution rates of 58–90% across orchestrators with GPT-4o agents, with some configurations at 100%, even when individual agents refuse.
- **§7.1, promotion-gate evidence:**
  - AuthMem structured authority labels cut the unauthorized-action rate from 16.9% to 0.0% over 350 pairs, while utility moved from 39.7% to 40.0%.
  - PoisonedEvolution's provenance-diversity gate cut the skill embedding rate from 25/25 to 0/25 (n = 30, k = 3).
  - EVOMAL's counter-prompt cut ASPR from 20.3–41.8% to ≤1.8%.
- **§7.2:**
  - MAPLE-Guard ASR: 38.2% → 0.9% (LongMemEval) and 34.7% → 0.2% (AppWorld).
  - PS-Bench detection-reflection lowers ASR by about 27.4% (GPT-4o-mini).
  - CPE census: 463 candidate sources, 282 runtime-verified, across 12 harnesses.
- **§8.3, PASB.** Downstream failure is 45.0% in session-only episodes vs 71.9% in committed episodes (a 27.0-pt gap reported) across 12 models and 1,600 tasks. The authors stress this is association, not a causal memory-disabled control.
- **§8.3, SkillMisevo exposure sweep.** Carryover ASR is 16.0% at K = 0, 35.3% at K = 3 and 41.3% at K = 9.
- **§8.3, SafeEvolve.** Unsafe retrieval drops from 35.33% to 8.67% and carryover ASR from 21.33% to 4.00%. Online utility moves only from 58.44% to 58.00%, but carryover utility falls from 53.33% to 40.67%. The authors read this as a hidden loss of useful persistent reuse.
- **§8.3, AID-Guard.** The strict manifest profile costs 43.8 pts (DeepSeek) and 35.4 pts (Qwen) of benign utility.
- **§8.3, ColluSkill.** 96.0% ASR vs 6 scanners; ChainGuard with installed-skill context brings it from 69.0% to 22.5%.
- **Takeaways 7.1–7.4:**
  - Promotion gates need negative evidence.
  - Defense placement follows the transformation.
  - Recovery is scoped to the descendants actually discovered.
  - Guardrail updates re-enter the adaptive loop: DARWIN and BraveGuard train guards on evolving evidence whose own safety goes unchecked.
- **§2.5 and §8.4, overall diagnosis.** The literature is strong on admission, retrieval, activation, exposure and local containment. It is thin on selection, descendant repair, deletion verification, contestability and post-repair continued adaptation. "a mismatch between event-level evidence and the stronger proof obligations of cross-session repair."
- **App. B, selection-pressure identity.** Under softmax selection q^β ∝ p·exp(β û), ∂E[r]/∂β = Cov(r, û). Sharper utility selection raises expected risk only if risk and the proxy covary positively. The authors flag this as their own diagnostic, not an empirical law.
- **§9.1.** Devil's Moltbook [198] gives a formal trilemma: continuous self-evolution, complete isolation and safety invariance cannot all hold. The survey treats it as a design constraint.

**Evaluation practices it describes or criticizes**
- **Criticized practices:**
  - Ranking heterogeneous headline ASRs across papers is a "category error rather than a meta-analysis" (§8.3, Takeaway 8.2, App. E rule E.1). Denominators differ: retrieval opportunities, accepted updates, experience sets, completed evolution trials, 3-skill chains, provider schedules.
  - Single-slice benchmarks (AgentDojo/InjecAgent at exposure, R-Judge at recognition, ASB, Agent-SafetyBench) lack state lineage. They cannot say whether an injection was only in the prompt or was written, summarized or promoted (§8.2).
  - Block rates are reported without utility cost, and safety is claimed "merely by abandoning useful work" (§8.1, §9.6).
  - Score gains under a co-evolving evaluator (RQGM [70]) may reflect a changed criterion (Takeaway 8.1: version the evaluator alongside the agent).
  - Treating session-only vs committed episodes as causal (the PASB caveat).
- **Table 6, coverage across Admit / Move / Activate / Expose / Contain / Rollback / Delete / Contest:** nearly every family is ✗ on Rollback, Delete and Contest. SkillMisevo and EVOMAL get partial (●) Rollback, and EVOMAL and PersistBench get partial Delete. HarnessSafe, SkillMisevo-Bench and EVOMAL are the three "connected trace" designs.
- **Proposed metrics (§8.1):**
  - Safe Promotion Rate (SPR): safe promotions over all committed promotions; undefined if there are none.
  - Boundary Drift (BD): distance between decision vectors on fixed paired safety probes at t = 0 vs t.
  - Negative-Evidence Coverage (NEC): retained or consulted refusals, aborts, failures, corrections and near misses over those observed. It separates "discarded" from "never generated".
- **Proposed protocol design (§8.1):**
  - Separate adversarial and endogenous tracks, the endogenous one with no malicious writer, fixed probes and a fixed authority policy.
  - Cross task feasibility with action authority, and report true safe-abort, false-abort and correct-escalation rates alongside utility.
- **Table 7, lifecycle evaluation contract:** Seed → Adapt → Delay → Trigger → Expose → Repair → Probe → Continue, each with required trace fields. A claim stops at: no lineage, no denominator, or reactivation after resumed adaptation.
- **Takeaway 8.4.** Recovery evaluation must resume adaptation for a declared number of update opportunities and then re-probe.
- **Takeaway 9.2.** Benchmarks should separate context-accumulation harm (multi-turn, compression) from persistent-update drift.

**Open problems / future directions it lists (complete; §9, Table 9, Takeaways 9.1–9.3)**
1. **Selection pressure and endogenous drift (§9.1).**
   - Log candidates, rankings, checks and absent counterexamples.
   - Test whether utility-guided selection over an otherwise *benign* candidate stream shifts the safety boundary; its prevalence is unknown.
   - Quantify the utility lost when risky update paths are blocked, given the Moltbook trilemma.
   - Minimal test: candidate/promotion log plus fixed cross-epoch probes for authority, scope, refusal and confirmation drift.
2. **Migration and semantic preservation (§9.2).**
   - Build a general migration-control discipline, comparing unauthorized role upgrades against useful transfers that were wrongly downgraded.
   - **Semantic taint tracking** through summaries, embeddings, generated skills and model updates: combine derivation links with content checks, and bound the uncertainty without either marking every descendant as contaminated or losing the path.
   - Minimal test: migration ledger plus residual probes.
3. **Activation, authorization and model scope (§9.3).**
   - Decide how finely to split readable context, reasoning support, planning authority and effect permission.
   - Measure unauthorized activations alongside legitimate work that was delayed.
   - **Authorization continuity benchmark:** stale grants kept, valid grants wrongly revoked, reauthorization cost, compositions of permitted mutations.
   - **Context-horizon safety.**
   - **Model-adaptation governance:** link update admission, deployment scope, activation, rollback, cache erasure and residual-influence verification; test out-of-scope effects of task-scoped updates and merged components.
4. **Propagation and distributed lineage (§9.4).**
   - Enumerate affected descendants across agents and roll back produced artifacts.
   - Notify users across agent boundaries.
   - Design a distributed repair protocol that separates notification, acknowledged invalidation and verified influence withdrawal, under partial reachability and missing repair ownership.
   - Minimal test: propagation trace plus contest docket.
5. **Recovery and verification (§9.5).**
   - Infer the descendant inventory.
   - Compare targeted repair against broad quarantine at an equal discovery budget.
   - **Post-response continuation:** attribute a later failure to unrepaired state, new evidence, or a changed evaluator.
   - **Deletion verification:** object absence vs influence removal; how much finite probes prove; the privacy cost of probing.
   - **User contestability under partial visibility.**
6. **Benchmark and governance evidence chains (§9.6).**
   - Extend connected traces backward to candidate selection and forward to descendant repair and resumed adaptation.
   - Keep utility and exposure matched so that "safer" does not just mean "less capable".
   - Reconstruct never-retained candidates from natural deployment streams.
   - Make independently evaluated controls interoperable on state identity, authority and repair.
   - Protect governance records (approvals, quarantine markers, rollback receipts) from being copied, summarized or going stale.
   - Takeaway 9.3: test whether local controls *compose*.
- **Also called gaps elsewhere in the paper:**
  - Direct tests of resource-exhaustion repair and lifecycle auditability (§5.5).
  - Evaluated attribute-preserving migration controls (§7.2).
  - An origin- and scope-preserving defense against harness CPE (§7.2).
  - Choosing a deployment's authorization envelope and effect ceiling (§7.2).
  - A test of functional equivalence across substrates (§4.7).
  - Safety of the update evidence used to train evolving guardrails (§7.5).

**What it misses or gets wrong as of Nov 2026** (checked against the project knowledge files)
- **Self-referential improvement loops are missing.** There are zero mentions of Hyperagents, DGM/Darwin-Gödel, SICA, GEPA, AlphaEvolve or "recursive self-improvement" (grep of the full text). The only Gödel-family entry is RQGM [70], and that is for evaluator co-evolution.
  - The meta-agent's own procedure or code is never a substrate. 𝒜 = (Θ, ℳ, 𝒯, 𝒲) has no "improver" coordinate.
  - So the safety of an agent rewriting its own self-improvement procedure is absent. That literature (Gödel Agent, DGM, Hyperagents, IbI, HMED 2610.07979, Lineage Is Memory 2610.41157, the ICLR-27 RSI-Harness, MetaEvoForge and Meta-TTL; per FINAL_REPORT.md and round3/IBI_GAPS_AND_METAREWARD.md) is untouched.
- **Reward hacking and Goodhart are absent.** Zero mentions of reward hacking or Goodhart. Evaluator gaming appears only through RQGM. It misses the project's "Reward hacking / Goodhart in self-evolving loops" cluster (round2/field_forecast.md: Masked Erosion 2610.38392, SEABench 2609.35596, harness tampering 2609.00069, appreciation→injection 2610.39122 ◐), most of which appeared after its 4 Sep freeze.
  - SEABench (48 longitudinal sequences, endogenous misalignment from locally useful updates; per LITERATURE.md ◐) is exactly the "endogenous track" the survey says is missing.
- **Shao et al., "Your Agent May Misevolve" (ICLR 2026, 2509.26354) is under-used.** It is the field's seminal misevolution paper, but it is cited only as a leaf label in the Fig. 4 roadmap ([172] as "Residual Probes" / "Delayed Activation"). It is never discussed in the prose, although its endogenous-drift evidence bears directly on the §9.1 claim that "none … establishes the prevalence of attacker-free drift". The 2026 experience-driven safety paper [266] is discussed instead.
- **Other post-cutoff work it cannot cover** (project files, ◐): Evolutionary Safety of RSI 2609.31186, Trustworthy-RSI survey 2610.35193, Is RSI Safe? 2611.10261, RSI Control / covert sabotage 2610.27735, Compositional Safety Failures 2609.33123, Trusting Trust Revisited 2609.17817.
  - It also ignores scheming, sabotage and deceptive self-modification by the agent itself (zero hits). Its threat model is either an external attacker or benign drift, never an adversarial self-modifier.
- **No simulator / world-model channel.** Zero hits on simulators or world models. It does not see that a learned simulator can corrupt both the trace channel and the fitness channel of evolution.
- **Continual learning is treated only as a safety problem.** It has no notion of capability drift, catastrophic forgetting (zero hits) or nonstationary environments. Its "drift" means safety-boundary drift (BD), not shifts in the improvement regime.
- **Register, style and language drift are not covered.** Sycophancy appears only as PASB's memory-mediated sycophancy (5 mentions). Conversational attractors (Ko & Geiping 2606.30571) and the Clark et al. 2610.29162 line are not cited.
- **Its own methodology is weak:**
  - 549 of 683 placements are low-confidence (App. A), so Fig. 1's counts are soft. The authors admit this.
  - Many headline numbers are taken from abstracts. It says so for SkillJack 80.0% and MAPLE-Guard.
  - The anchoring incident is admitted to be non-endogenous.
  - SPR, BD and NEC are proposed but not validated on any system.
  - The covariance identity is a trivial exponential-family result. It is useful as framing, not as a finding.
- **Title-page and arXiv dates disagree** (Oct 2 vs Sep 8); minor.

**Relevance to Dan's ideas**
- **Taught, Not Selected.** This is the strongest fit.
  - SAVER's split between selection semantics (û_t, c_t, π^evo) and the Abstract/Add operations that turn traces into reusable rules is essentially TNS's fitness channel vs trace channel, but stated as a safety framework.
  - The survey never separates the two empirically. OEP, experience-driven safety [266] and EvoBreak are all "trace-channel" (taught) harms. The App. B identity covers only the "selected" channel.
  - TNS can cite Takeaway 2.1 ("utility does not authorize promotion") and Takeaway 4.1 (abstraction needs its own check) as framing. It can adopt NEC as an exploit/negative-evidence readout and the endogenous-track design (fixed probes, no malicious writer).
  - None of its 683 papers scores evolution in a simulator, so TNS stays unscooped by this survey.
- **Propose/Propagate/Pay-off funnel.** SAVER's proposal set Δ_t → selection → commit → activate → expose is a safety-side funnel.
  - SkillMisevo's 21→19→19→15 gate counts and HarnessSafe's "furthest stage reached" are the same stage-resolved analysis the funnel proposes for improver edits.
  - Its demand to log rejected candidates and evaluator versions is the funnel's Propose-stage denominator.
  - SPR is a safety analog of the Propagate rate.
  - It has no Pay-off (order-1 gain) concept. The funnel can cite it to motivate stage-resolved reporting over endpoint scores (Takeaway 8.3).
- **Meta-Overfitting.** This is the safety mirror image. "Local-success generalization", OEP's "locally correct but non-transferable" experience, and scope-widening violations describe home-domain lessons over-generalized to far tasks. Meta-Overfitting describes improvers that only work where they grew up.
  - The scope attribute and promotion test give a vocabulary for transfer-distance arms.
  - Nothing in the survey addresses improver-level transfer.
- **Meta-reward.** RQGM, Takeaway 8.1 ("version the evaluator") and Takeaway 7.4 (guardrail updates re-enter the loop) all apply.
  - App. B gives a safety argument for a design choice: apply c_t as a hard eligibility gate *before* utility ranking, rather than folding safety into the reward.
  - A meta-reward bake-off could add BD and SPR as side readouts.
- **Openness.** The substrate list (Θ/ℳ/𝒯/𝒲) and the Fig. 5 / Fig. 6 authority ladder give openness a safety-cost axis: wider editable surfaces mean more authority regimes and more migrations.
  - It omits the improver/evaluator rung that the openness law ("open where the cause is") needs. This is a gap worth stating in the paper.
- **Continual/drift.** Takeaway 8.4 (resume adaptation after repair), BD, post-response continuation, the endogenous track and the Moltbook trilemma are directly reusable as protocol language for a continual or drift benchmark.
  - Takeaway 9.2 (accumulated context is not update-driven drift) is a useful distinction to borrow.
  - It offers nothing on capability drift or forgetting.
- **Register snowball.** Weak relevance.
  - Takeaway 9.2 separates within-conversation accumulation from persistent-update drift, which is a framing the snowball paper should adopt.
  - PASB (persistent sycophancy via memory; 45.0% vs 71.9%) is the nearest stateful analog.
  - The multi-agent propagation cases (Agents of Chaos, Collective Misremembering, Devil's Moltbook) are the inter-agent version of loop gain greater than 1.
  - Register and style drift themselves are not covered.


---

### Continual Learning in Transition (2608.06216)

**Read status: ✓ full text.** I read all 32 pages from `pdftotext`: the main body (§1–5, pp. 1–17), all 210 references, and Appendices A–C (glossary, Fig. 2 label key, benchmark catalogue, cross-dimensional profile table). Text is at /tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/2608.06216/paper.txt. Every claim below marked ✓ comes from that text, with section refs.

**Bibliographic**
- **Authors:** Zhiyan Hou\*, Dan Zhang\*†, Tao Feng\*†, Liyuan Wang, Wei Li, Xiangzhao Hao, Hongyan An, Junfeng Fang, Haokai Ma, Zhaohui Xu, Xinyu Tang, Haiyun Guo†, Jinqiao Wang, Tat-Seng Chua (\* equal contribution, † corresponding).
- **Orgs:** CASIA / UCAS, NUS, Tsinghua, Alibaba, evermind.ai, Wuhan AI Research Institute.
- **Name clash:** the coauthor "Dan Zhang" (NUS) is not our student Dan.
- **Date and venue:** arXiv cs.LG; v1 2026-08-06, v2 2026-08-12. It is a preprint. The text mentions a "submission deadline", but no venue is named.
- **Length:** 32 pages. The main text is about 17 pages, with 210 references and 3 appendices.
- **Link:** https://arxiv.org/abs/2608.06216
- **Type:** a survey with an explicit position section (§4).

**Scope & definitions** ✓
- **Classical CL (§2.1).** A model sees a sequence of tasks T₁…T_K, each with its own distribution D_k and loss L_k. Past data is unavailable or only partly available. The goal is to keep the summed expected loss over all tasks seen so far low.
  - Its two desiderata are the stability–plasticity trade-off and within-task / across-task generalizability.
  - Its four method families are replay, gradient-based, architecture-based and regularization-based.
- **Three hidden assumptions (§2.1).** The paper says classical CL quietly assumes:
  - (i) learning happens in a dedicated pre-deployment training stage;
  - (ii) capability lives in the parameters;
  - (iii) updates are gradient-based, on data supplied from outside or collected earlier.
- **Turning assumptions into axes (§2.2).** Each assumption is a fixed answer to a question: When, Where and How. The field is redefined as "continual capability evolution" across these three axes.
  - The axes are "complementary analytical perspectives rather than mutually exclusive categories".
  - Each method gets a When–Where–How *profile*, and that profile can change over time (for example, a skill that is later internalized into weights).
  - Classical CL becomes one reference point: post-training × parameters × off-policy gradients.
- **Harness (§3.3.2, following Externalization survey 2604.08224).** The harness is everything outside the weights that the system designer builds and edits. It has three parts:
  - **memory:** storage that can be read and written across inference calls;
  - **skills:** executable units the model calls (tools, functions, sub-agents);
  - **protocols:** rules and formats for interacting with the environment (system prompts, rule files, message schemas).
- **Inference-time CL (§3.2.3).** What separates it from test-time compute is *persistence*. Information gained during inference is written back, as state or as weight updates, so it affects later behavior. Search and self-consistency do not count.
- **Learning beyond gradients (§3.4.2).** This is a spectrum: zeroth-order optimization (still estimates a gradient), then gradient-free weight composition (merging), then harness edits that leave parameter space entirely. The last is credited to Weng's "heuristic learning" blog post [32].

**Taxonomy** ✓ (Fig. 3 tree; Fig. 2 places methods on When × Where with How on the x-axis; Table C.1 gives multi-label profiles)
- **When (§3.2)**
  - Pre-training (continual pre-training over changing corpora): SimpleCPT (LR re-warming plus a small replay buffer), LR rewarming, CKL, ELLE, TimeLMs, CPRec. Fig. 2 adds CoSMIC, TiC-LM, MER, Effective CPT, MoE-CPT, Stability-Gap, Grad-Localization and CPT-LM.
  - Post-training (SFT, then RLHF/RLVR, then successive versions): SAPT, InsCL, SSR, SEEKR, FOREVER, KeepLoRA, RFT-CPT, Skill Neologisms. Diagnostics discussed: Luo et al. (forgetting worsens with scale across 1B–7B), TRACE, spurious forgetting (Zheng et al.), alignment tax, ReCALL.
  - Inference-time (persistent write-back after deployment): TTT-LM, TTT-E2E, Titans; TTRL in the text.
- **Where (§3.3)**
  - Parameters (modular or PEFT updates): O-LoRA, Progressive Prompts, LoRAMoE, SLIM, MoCL, AlphaEdit, NormAnchors, DRAPE, CRAM, HPA, Hidden Forgetting, Experience Internalization, LM Sleep, PEAM, Evolving-RL, MemoryLLM, WISE, TMEM, GRACE.
  - Harness → Memory: MemoryBank (Ebbinghaus-style forgetting curve), A-MEM, HippoRAG, ExpeL, AWM, Mem0, LightEdit, CURATOR, MATTRL, MemRL, Memory-R1, Mem-α, Memory-R2, MemBuilder, MemQ, MAA, JiT-RL, ReasoningBank, MUSE, EXG, Janus, SimpleMem, AutoMem, AutoAgent, Arbor, EvoClinician, Reflexion.
  - Harness → Skills: Voyager, TroVE, SkillRL, SAGE, AutoSkill, ReSkill, ARISE, SkillGraph, Skill1, SkillComposer, EvoSOP, AgentFactory, Tool-Making, DreamProver, Ratchet, SkillWeaver, ASI, Buffer of Thoughts, Alita, Memp, Skill-Pro, MUSE-Autoskill, AutoRefine, Mem²Evolve, SkillForge, FederatedSkill, MemSkill, Agent0, LifeSkill, SKILL0, XSkill.
  - Harness → Protocols: Promptbreeder, GEPA, Optima, AutoManual, Agent-Pro, Symbolic Learning, AutoGuide, PACE, ADAS, AFlow, AgentSquare, EvoFlow, AgentNet, SEW, SePO, Autogenesis, DARWIN, DGM. Rule files in Cline, Aider and Cursor are named as engineering practice.
- **How (§3.4)**
  - Off-policy gradients: SDFT (Yang et al., ACL'24), COPR.
  - On-policy gradients: Self-distillation CL (Shenfeld et al. 2601.19897), CPPO, TTRL, SDPO-CL ("Denser ≠ better"), TTC-RL, SEAL, AgentEvolver. SRPO is discussed in the text.
  - Beyond gradients: AIMMerging, BaM, GCWM, ZeroFlow, Merge-before-Forget, ES-Forgetting. Task Arithmetic, TIES, DARE, Twin-Merging, Evolutionary Merge and MeZO appear in the text.
- **Cross-dimensional examples (§3.5, Table C.1)**
  - Reflexion: inference × memory × beyond-gradient.
  - TTRL: inference × parameters × on-policy.
  - AgentEvolver: parameters plus memory, on-policy.
  - SkillRL: parameters plus skills, on-policy.
  - SKILL0: skills → parameters, a carrier that changes over time.
- **Classical families (§2.1):** replay (iCaRL, ER), gradient (GEM, OGD, C-Flat), architecture (PackNet, HAT, Progressive Nets), regularization (EWC, SI, MAS, LwF).

**Key claims & findings** (all ✓)
- **The classical taxonomy is outgrown (§1, §2.2).** Recent work moves off the classical point on several axes at once (§5).
- **On-policy learning fits CL structurally (§3.4.1).** Citing RL's Razor [27], the amount forgotten tracks the KL between the fine-tuned and reference policies on the new task. On-policy RL implicitly picks the smallest-KL solution, while SFT can drift arbitrarily far. Off-policy methods keep drift small by keeping the training data close to the model's own distribution (SDFT).
- **The How axis is about drift control (§3.4).** "The central question along the How axis is not merely how an update is computed, but how strongly it perturbs what the model already knows."
- **Zeroth-order updates are a good continual learner (§3.4.2).** Via ZeroFlow, they match or beat first-order fine-tuning on forgetting. The paper treats this as an implicit-smoothing parallel to on-policy small-KL.
- **Post-training is a CL problem (§3.2.2).** The unit for measuring forgetting is the whole post-training pipeline, not a single fine-tuning step. Part of what looks like forgetting is spurious, a loss of alignment rather than of knowledge.
- **Three carrier layers (§4.1).** Parameters, external memory and context. Moving outward, write cost falls and interpretability rises, but recall stability and capacity fall in step.
  - Context has a hard length ceiling and a soft one: usable attention does not grow with window length.
  - Summary/compaction can "silently reverse a hard-won optimization" (citing a Dwarkesh essay [188]).
  - Hence "a purely engineering route that tries to replace continual learning with ever longer context will meet a bottleneck."
- **Retrieval decays as sessions lengthen (§4.1).** Memory therefore needs active forgetting, and the stability–plasticity trade-off moves to the memory layer, which needs its own metrics.
- **Position: harness engineering "functionally substitutes for, but does not yet mechanistically constitute, continual learning" (§4.2).** Three reasons:
  - (1) Edits to rule files or tool lists are made by humans, so the loop is not closed by the system's own experience.
  - (2) Methods that do close the loop (Promptbreeder, Voyager, AgentEvolver) are immature in evaluation, stability and reproducibility.
  - (3) Harness capability is tied to its context and transfers less well than parameter capability.
  - The paper presents the debate between Bell/Lomonaco [190], Dwarkesh [188] and Lambert [191] even-handedly.
- **Forgetting now has four faces (§4.3).** Parameter overwrite; retrieval decay and entry aging in memory; window overflow and attention dilution in context; skill/protocol mismatch caused by context drift. These differ in timescale, observability and reversibility. The paper proposes recasting forgetting as "active compression and release" across carriers.
- **Carriers update at different speeds (§4.3).** Memory changes per interaction, skills per task, protocols and parameters per project.
- **Long-horizon error compounding (§4.4).** It is worse in agents for three reasons: errors live in a composite state; harness operations are non-differentiable; and with no train/eval boundary, a wrong state can be written back into memory and form a positive feedback loop.
- **AGI judgment (§4.5).**
  - "continual learning for the frontier model is the first priority, whereas domain-specific continual learning is not".
  - The memory-and-harness route is close to the ceiling of a general assistant, but far from AGI.
  - Verifiable rewards are the "key bridge" from engineering to mechanism.

**Evaluation practices it describes or criticizes** (✓)
- **Benchmark catalogue (App. B, Table B.1).** It profiles benchmarks by When, Where and How:
  - TiC-LM: 114 chronological Common Crawl dumps.
  - TemporalWiki, TRACE (8 tasks), CITB, CoIN, MLLM-CL.
  - MLLM-CTBench: compares SFT, RFT and fusion, with process-level reasoning quality.
  - CoTTA protocol.
  - OAKS: facts that change repeatedly; measures state-tracking accuracy and adaptation delay.
  - MemoryBench, EvoMemBench.
  - SkillLearnBench: 20 tasks across 15 sub-domains; measures recursive drift.
  - SkillFlow: 166 sequential tasks from 20 families; measures regression and library evolution.
  - SEAGym: train / frozen validation / held-out ID and OOD tests, replay diagnostics, cost records.
  - Almost every benchmark before 2026 is parameters × off-policy. Harness benchmarks are all inference-time.
- **Criticisms it makes:**
  - Static benchmarks measure what a model ships with, not the net capability it gains while running. It calls for trajectory-level measures: goal drift and state loss over tens to hundreds of steps (§4.4).
  - No single metric covers forgetting across carriers (§4.3).
  - The memory layer needs its own stability–plasticity metrics (§4.1).
  - Closed-loop harness learners are weak on evaluation, stability and reproducibility (§4.2).
  - Forgetting should be measured over the post-training pipeline, and spurious forgetting kept apart from real forgetting (§3.2.2).
- **Self-limitations:**
  - Placement counts in Figs. 2 and 3 are "not a statistical estimate of research density" (§3.5).
  - Evaluation protocols for harness and cross-axis methods are left to future work (§5).
- **No new experiments and no meta-analysis numbers.**

**Open problems / future directions it lists** (complete, ✓)
1. **Long-run allocation (§4.1).** Which capabilities to internalize into parameters, which to keep in memory, and which to use and discard in context. No systematic solution exists.
2. **Active forgetting at the memory layer (§4.1).** Selective reduction against retrieval decay and noise build-up, with metrics specific to memory.
3. **Turning engineering into mechanism (§4.2).** Convert harness gains that humans now carry into self-driven mechanisms. Make closed-loop harness learners mature in evaluation, stability and reproducibility. Improve how well harness capability transfers across contexts.
4. **Two-way transfer between parameters and harness (§4.3).** When to consolidate into weights (frequently used, widely shared items) and when to move back to the harness (context-specific or unstable items). The paper notes a cache-hierarchy analogy but no formal framework.
5. **Scheduling across carriers (§4.3).** Decide what goes to context, memory, skills or weights, and coordinate writing and reclamation across carriers that update at different speeds.
6. **Unified criteria for multi-faceted forgetting (§4.3).** Which layer to act on, how fast, and what to keep or release. The paper calls this the "central obstacle" to a theory of coordinated evolution.
7. **Long-horizon agents as the testbed (§4.4).** Trajectory-level evaluation; designing memory compression, consolidation and architecture together.
8. **Error accumulation (§4.4).** A verifiable model of error build-up and drift-resistant mechanisms for long-horizon agents, given composite state, non-differentiable operations and write-back feedback loops. Reflexion and TTRL each use only one mechanism.
9. **System-level CL (§4.5).** An end-to-end learnable agent framework whose harness components are trained jointly with the model, plus a theory of system-level CL.
10. **Semi-verifiable rewards (§4.5).** Rewards or composite feedback for tasks with no verifiable signal, such as writing, consulting and long-horizon planning.
11. **Selective forgetting as capability management (§4.5),** separating task-shift from time-shift forgetting.
12. **Continual learning of the frontier model over domain-specific CL (§4.5).**
13. **Sparse or empty regions of the When × Where × How space (§1, §5).** Protocols as a carrier are at "an early stage" (§3.3.2). Fig. 2 shows no harness methods at pre-training time.

**What it misses or gets wrong as of Nov 2026** (compared against the project files; project-file entries were verified in earlier rounds unless noted)
- **No meta level.** Where stops at memory, skills and protocols. There is no *improver* carrier (the self-improvement procedure itself) and no *evaluator/gate* carrier. DGM, ADAS, AFlow and DARWIN are put under "Protocols", next to Promptbreeder, so self-referential improver edits are lumped in with prompt evolution. The project's main threads are about exactly these levels: the openness ladder L4/L5, the self-edit drought, and IbI ZMmJDeDTXk / SelfImprove-Bench 2611.02797 (0% improver edits).
- **No selection-side failure modes.** There is nothing on Goodhart or reward hacking in self-evolution, selection/winner's-curse, or held-out vs in-distribution gains. There is also no compute-matched baseline critique:
  - QDdWiaNkk2: harness evolution loses to test-time scaling on TB2.1 at matched budget.
  - HDA 2610.22466: about 54% of the average gain is overfitting.
  - "Verifiable rewards as bridge" is stated without noting that verifiers drift or can be gamed.
- **Pre-August work omitted** (from the project LITERATURE file):
  - Do Agent Optimizers Compound? 2607.14004 (Jul 15): GEPA transfers below baseline, and Meta-Harness stalls in a second round. This is direct evidence on harness-level CL.
  - Meta-Harness is not cited at all.
  - Harness-evolution gyms are mostly missing: EvoAgentBench 2607.05202, HarnessOpt-Bench 2608.06301, ContinualSkillBench 2608.03874. SEAGym is cited.
- **Post-v2 work, outside its window, that changes the picture:**
  - Are Self-Evolving Agents Continual Learners? 2610.25858: GEPA, Meta-Harness and ReasoningBank on drift streams. Plasticity and stability trade off (r = −0.72), and Meta-Harness looks stable only because its gate freezes the workspace. This is the empirical test of the survey's harness-CL claims.
  - Agent Plasticity 2610.08902.
  - "Do agents need to sleep?" 2611.02216 and coding-agent forgetting 2611.03219.
  - The ICLR'27 wave on improver transfer: RSI-Harness EOrtreQLbv reports up to 4.5× improver transfer while the harness does not transfer. That partly contradicts §4.2 reason (3), which says the harness transfers worse than parameters, while agreeing at the artifact level.
- **The on-policy optimism is one-sided (§1, §3.4.1).** It rests mainly on RL's Razor. The paper lists counter-evidence in Fig. 3 without discussing it: "Denser ≠ better" [165] on the limits of on-policy self-distillation, and "Evolutionary strategies lead to catastrophic forgetting" [173]. The ES result also cuts against the "zeroth-order is intrinsically stable" framing.
- **Citation and labeling errors** (✓ from text):
  - Ref [40] is FIRST, a federated *inference scheduling* toolkit for HPC. It is cited as the source for gradient-based CL methods. This is a miscitation.
  - "SDFT" is ambiguous. Fig. 3 / Table C.1 use SDFT [161] (Yang et al., off-policy). Table A.2 says the Fig. 2 "SDFT" is Shenfeld et al. [29], which is on-policy.
  - Table C.1 labels Promptbreeder and GEPA as "post-training", although §3.3.2 presents them as continual rewriting during deployment.
  - §3.5 says empty regions are *not* evidence that no work exists, but §5 says the dense-vs-empty contrast "identifies unfilled coordinates". This is an internal tension.
  - Typos: "his survey" (§5), "Figures 2 contains" (§3.5).
- **Heavy self-citation.** Intro refs [8–11] (ReST-MCTS\*, TDRM, ReST-RL, SceneGenAgent), plus C-Flat, ZeroFlow, SEEKR, AlphaEdit, SRPO, MLLM-CTBench, ReCALL, Continual GUI agents, KnowRL and CLEAR. ReCALL gets a paragraph it admits is not CL.
- **Key arguments rest on blogs and essays.** Dwarkesh, Lambert and Weng are all flagged by the paper itself as "contextual commentary".
- **No safety angle.** Nothing on safety erosion under self-evolution beyond a mention of TRACE.

**Relevance to Dan's ideas**
- **Taught, Not Selected.** The survey places GEPA and Promptbreeder as harness-protocol × beyond-gradient learners. It never separates *what the reflector reads* (traces) from *what selection scores* (fitness), and it has no simulator or world-model evaluators. Two passages give useful framing:
  - §4.4: a wrong state "may be written back into memory through immediate feedback and form a positive feedback loop". This is close to the trace channel, at the general level.
  - §4.5: "semi-verifiable rewards or composite feedback" as an open problem. A simulator-scored fitness is one concrete instance.
  - Use: cite as the framing survey and claim the trace/fitness split as a gap it does not draw. Low scoop risk.
- **Propose / Propagate / Pay-off funnel.** Not anticipated. The survey has no edit-level accounting, no gates and no nested replay. §4.2's "far from mature in evaluation, stability, and reproducibility" can be used as motivation. The funnel is a concrete answer to the evaluation gap the survey leaves to future work (§5).
- **Meta-Overfitting.** §4.2 reason (3) is the prior the project tests, one level up: harness capability "is bound to a specific context and is hard to transfer without loss". The survey asserts this without evidence and only for the artifact level. Meta-Overfitting asks whether the *improver* is equally context-bound. Cite it as the stated conventional view, with RSI-Harness / MetaEvoForge as the opposite-sign evidence.
- **Meta-reward.** Indirect only. The survey frames verifiable rewards as the engineering-to-mechanism bridge and asks for composite feedback. It never considers rewarding *improvement of the improver*, and never mentions credit horizons or successor credit. This is clear white space.
- **Openness.** The Where axis (parameters → memory → skills → protocols) roughly matches the lower rungs of the openness ladder (about L0–L3). It stops before the improver (L4) and the gate/evaluator (L5), which are the rungs the "Open Where the Cause Is" claim rests on. Two survey ideas can become testable hypotheses for the ladder:
  - different update speeds per carrier (§4.3);
  - two-way parameter–harness transfer (§4.3).
  - Note: the survey treats openness as which carriers exist, not as a permission level that can be controlled.
- **Continual / drift.** This is the most directly relevant part.
  - The four-faces forgetting typology (§4.3) includes "capability mismatch at the skill and protocol layer caused by context drift". That is roughly the D_task cell.
  - The survey has no notion of *cause-location* drift. Improvement-regime drift (flaky tests, feedback granularity) and verifier/grader drift do not appear.
  - Table B.1 gives ready-to-cite benchmark positioning: SEAGym, SkillFlow (regression), SkillLearnBench (recursive drift), OAKS (repeatedly changing facts). CauseShift can fill the "Harness × improver/gate × drift" row the catalogue lacks.
  - Cite alongside the newer 2610.25858 and 2610.08902.
- **Register snowball.** Marginal relevance:
  - Context is described as the most volatile carrier, with attention dilution (§4.1).
  - The general idea of positive feedback in long interaction (§4.4) is related.
  - There is nothing on style/register drift, human–model co-adaptation or loop gain.
  - At most one related-work sentence.

Files: full text at /tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/2608.06216/paper.txt (PDF alongside). The paper was already listed in the surveys line of /home/user/nlp-lab-paper-search/research/round1/context/reading_list.md, but no project file had digested it.

---

### Agentic World Modeling: Foundations, Capabilities, Laws, and Beyond (2604.22748)

**Read status: ✓ full text.** I read all of the main body (§1–§9, pp. 1–52) and all appendices A–F (pp. 81–92) from arXiv v3. I only skimmed the reference list (pp. 53–80). Local text: `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/2604.22748/paper.txt`. One gap: Appendix E.5, the "capability coverage matrix" with S/M/W labels, has only one sentence in v3. Its table did not come through the text extraction, or it is missing from the paper.

---

**Bibliographic**
- Authors: Meng Chu, Xuan Billy Zhang (project lead), Kevin Qinghong Lin, Lingdong Kong, Jize Zhang, Teng Tu and Weijian Ma are the core contributors. There are about 51 authors in total.
- Senior authors include Long Chen, Qifeng Chen, Xiaojuan Qi, Yang Deng, Yanwei Li, Mike Zheng Shou, Ziwei Liu, Philip Torr and Jiaya Jia.
- Organizations: HKUST, NUS, Oxford, NTU, CUHK, HKU, UW, U Tokyo, Cambridge, CMU, UC Berkeley, SUTD and SMU.
- Dates and venue: v1 was posted 2026-04-24 and v3 on 2026-06-16 (cs.AI). It is a preprint with a "not peer-reviewed" disclaimer and no venue.
- Length: 92 pages, with about 500 references by rough count. It claims to synthesize over 400 works and to summarize 100+ representative systems.
- Links: https://arxiv.org/abs/2604.22748. Homepage agentic-world-modeling.xyz. Repo github.com/matrix-agent/awesome-agentic-world-modeling. There is also a Notion database.
- Name collision: a different paper, "Agentic World Modeling" 2610.40927 (Microsoft, test-time Best-of-N), is the one the project cites in `round1/critiques/evolve-in-dream.md`. Do not conflate the two.
- The authors call it a "position-driven survey". It proposes a taxonomy plus testable boundary conditions, and it introduces no new benchmark (§1.3).

**Scope & definitions**
- **World model.** "Learned (or hybrid) operators that support intervention-aware transition queries" (§1.2). *World modeling* is the staged process of strengthening those operators.
- **Value is decision-centric.** A world model's value is defined "by the quality of decisions they enable for downstream agents" (§1).
- **Formalism.** A POMDP E = (X, A, Ω, T, O, R, γ). The learned parts are inference q_φ, forward dynamics p_θ, a decoder p_ψ and inverse dynamics π_η. The planner, policy and reward are explicitly *not* part of the world model (§2.3, App. B.2).
- **Four things that separate world modeling from generic prediction:** rollout, intervention sensitivity, constraint consistency and closed-loop use. Closed-loop use is declared *orthogonal* to the levels: "a weather emulator can be an L2 world model with no embedded planner" (§2.4).
- **"Closed loop" has two senses that must not be conflated.** Using a world model inside planning is a deployment property. Revising the model stack from deployment evidence is the hallmark of L3 (§2.4, §5.2).
- **Levels are runtime modes, not model classes.** An agent dispatches to L1 for reactive steps and to L2 when it must compare action sequences. It escalates to L3 only when "systematic prediction failures … cannot be resolved by re-planning" (§1.2). L3 is "a governance layer". The levels form a containment hierarchy: L3 invokes L2, and L2 invokes L1 (§2.4).
- **Philosophical scaffolding** (§2.1, App. A):
  - L1 maps to Humean constant conjunction and predictive coding.
  - L2 maps to Lewis's closest possible worlds. Plato's cave stands for "epistemic drift".
  - L3 maps to Lakatos's distinction between the hard core (architecture, inductive biases) and the protective belt (parameters), with Duhem–Quine holism explaining why blame is hard to assign. It also draws on Popper, Kuhn and Peirce's abduction.
- **Representation thesis** (§2.2, §9): latent dynamics are "indispensable as a scaffold for L1 and L2, but the endpoint of L3, namely genuine revision of governing laws, requires a symbolic substrate."

**Taxonomy (levels × laws)**

Axis 1, capability levels, each with testable boundary conditions:
- **L1 Predictor: local, one-step operators** (§3; Table 3). Four operators:
  - *State inference:* VAE/β-VAE/VQ-VAE, CPC, SimCLR, MoCo, CURL, SPR, I-JEPA/V-JEPA, DINOv2, RSSM, Thinking-with-Blueprints. Scientific examples are AlphaFold 1/2/3, RoseTTAFold, ESMFold and HMM/RNN brain-state models.
  - *Forward dynamics, the core operator:* PILCO, PETS, MuZero, EfficientZero, Dreamer V1–V3, TD-MPC/TD-MPC2, DeepMDP, MBPO, E2C, World Models (Ha & Schmidhuber), and the token/diffusion models IRIS, TransDreamer, STORM, Δ-IRIS, DIAMOND and SimPLe.
  - *Observation decoding:* VAE, Latent Diffusion, DiT, Sora's decoder.
  - *Inverse dynamics:* curiosity (Pathak), VPT action labelling, goal-conditioned inverse models, "learning to poke".
- **L2 Simulator: multi-step, action-conditioned rollouts p̂(τ | z₀, a₁:H, c)** (§4; Tables 5–6).
  - Three boundary conditions: (1) long-horizon coherence, (2) intervention sensitivity, (3) constraint consistency. The constraint enters as a whole-trajectory compatibility term ϕ_c(τ), so the rollout distribution "does not factorize" per step (§4.1).
  - *Physical:*
    - Explicit simulators: MuJoCo/dm_control, Brax, Isaac, Genesis, SAPIEN, ManiSkill3.
    - Video worlds: Sora, Lumiere, VideoPoet, FramePack, Self-Forcing, Genie, GAIA-1/2, Oasis, WorldPlay, Matrix-Game 3.0, Yume, LongLive.
    - Decision-oriented: DreamZero, DreamDojo, FutureVLA, DayDreamer, PIN-WM.
    - Table 5 anchors: MuZero, Plan2Explore, DreamerV3, OccWorld, Vista, DriveDreamer, Copilot4D, Cosmos, Aether, HWM and others.
  - *Digital:*
    - Code world models: CodeWM, WorldCoder, WKM, CWM, Web World Models, SWE-World.
    - Web: WebDreamer, WMA, WebEvolver, DreamGym, WebSynthesis, WebWorld, Early Experience, AUI, Simia, WAC.
    - GUI: MobileDreamer, MobileWorldBench, ViMo, gWorld, NeuralOS, CUWM, Code2World.
    - Games: Code World Models for game playing, MineWorld, Game-TARS, RAGEN/VAGEN/RAGEN-2, GameNGen.
    - Tools: GTM, AWM, MCP-Cosmos, DyMo, and the Qian et al. 2026 "foresight" study.
  - *Social:*
    - Theory of mind: BToM, ToMnet, LaBToM, FANToM, ExploreToM, CoALA.
    - Strategic: CICERO, Deal or No Deal, Werewolf, Avalon.
    - Sandboxes: Generative Agents, Sotopia, Project Sid, OASIS, silicon sampling, Generative Social Choice, PersonaGym, LaMP.
    - Table 6 adds MASim, SWM-AP, AIvilization and PolicySim.
  - *Scientific:*
    - Forward simulation: GNS, FNO, Pangu-Weather, GraphCast, GenCast, NeuralGCM, Aurora, ClimaX, neural potentials, Lingshu-Cell.
    - Decision simulation: ChemBO, P3BO, BAX.
  - *Cross-regime systems:* driving, Voyager, CICERO and A-Lab.
- **L3 Evolver: evidence-driven model revision** (§5; Table 8).
  - The update is (M_t, d_t) → M_{t+1} by diagnose, distill and validate. Structurally it is a design → execute → observe → reflect loop.
  - Boundary conditions are stated in two places, worded differently:
    - §2.4: evidence-grounded diagnosis, persistent asset update ("skills, rules, parsers, tests, not only ephemeral in-context patches") and governed validation (regression and canary gates, rollback).
    - §5.2: active information expansion, autonomous execution and observation, and belief revision under challenge.
  - Three modes of growth: parameter update, architecture update and hypothesis-space expansion.
  - Physical examples: AdaptSim and Self-Modeling (Hu et al. 2025b).
  - Digital examples: FunSearch, AlphaEvolve, CodeIt, SWE-agent, AUI and AI Scientist-v2. Only CodeIt gets a full loop.
  - Social examples: Evolving Constitutions (Kumar et al. 2026) and AgentSociety.
  - Scientific examples: Robot Scientist Adam, yeast cycles, CAMEO, A-Lab, BacterAI, SDL lasers, AI Scientist, CASCADE, Biomni, BioLab, OriGene, Co-Scientist, MOOSE-Chem 1/2.
- **Beyond L3: "meta-world modeling"** (§8.4). These are systems that reason over the *space* of transition functions and alternative rule systems. The section is explicitly left open.

Axis 2, governing-law regimes (§2.5, Fig. 8, Table 7). Fig. 8 places them on formalizability × observability axes:

| Regime | Constraints | Signature failure | Evaluation focus |
|---|---|---|---|
| Physical | geometry, contact, conservation; analytically verifiable | contact instability, sim-to-real gap | stability; failure clustering |
| Digital | program semantics, API contracts, state machines; mechanically checkable | grounding breaks, async races | error-branch coverage |
| Social | beliefs, goals, norms; *reflexive* and *normative* | role drift, goal forgetting | counterfactual sensitivity |
| Scientific | latent mechanisms that must be discovered empirically | hallucinated mechanisms | evidence-chain repair |

Regimes mix in real systems. The taxonomy is "diagnostic", not a set of exclusive bins (§1.2).

**Key claims & findings**
- **Constraints over appearance.** "A good Simulator does not have to look more like the world; it must look more like the constraints" (§4.2.5). Making constraints loggable, replayable and regressable often helps long-horizon stability more than better perceptual fidelity.
- **Five L2 failure modes** (§4.3): compounding error; state aliasing and drift; controllability failure, where "a visually rich model that is weakly action-conditioned is less useful … than a rough model that responds to actions"; exploitability and simulator escape; and calibration failure under shift. They advise that "confident but wrong should be treated as a strong signal for evolution."
- **Any synthetic-data generator is implicitly a world model** (§4 intro).
- **Simulator access does not guarantee use** (§4.2.2, Qian et al. 2026, ✓ as reported by the survey). Agents "rarely invoke simulation, frequently misuse predicted rollouts, and can even degrade."
- **L3 bottleneck is validation.** "The bottleneck of an L3 system is typically not generating candidate fixes but validating them safely" (§5.1).
- **Revision triggers.** A one-off anomaly gets an online adjustment. A persistent anomaly gets a distilled parameter update. An "epistemic gap" gets a structural change. Duhem–Quine makes it ambiguous which case applies (§5.1).
- **Evidence quality.** Prefer falsifiable evidence. A screenshot plus DOM snapshot plus error code is refutable; "I think the page didn't load" is not. Keep multi-resolution, indexable evidence and separate persistent from transient storage for privacy (§5.3; Table 9 lists revision signals by regime).
- **L3 maturity by regime** (§5.4): scientific is Established, digital Partial (it lacks active experiment design), physical Emerging (attribution is hard) and social Aspirational (ethics and ambiguity).
- **L3 governance risks** (§5.4):
  - *Benchmark overfitting:* the gate is too close to the training distribution, so the system "learns to pass tests rather than improve genuinely."
  - *Knowledge contamination:* biased or adversarial evidence enters the revision loop.
  - *Misattribution cascades:* fixing one failure degrades another component.
  - Mitigations: refreshed held-out probes, canary deployment, causal ablations.
  - **Persistent-update trilemma** (§8.2): stability vs plasticity vs auditability, which "no current system resolves."
- **Architecture** (§7, Tables 11–13): three design axes (representation, dynamics, control interface). Principles:
  - "Separate what is learned from what is enforced": hard constraint layers belong at inference time.
  - "Instrument before you iterate": "without replay, L3 revision becomes anecdotal and ungovernable."
  - Match the representation to the planner's query.
  - VLA vs native world models is partly a sociotechnical path-dependence question.
  - Latency budgets by regime run from under 50 ms (real-time games) to hours (science).
- **Historical lesson** (§8.1): progress came "not from scale alone, but from changing what is represented, what is compositional over horizon, and what can be revised from evidence." The field is said to be converging on a neuro-symbolic frontier.
- **Numbers the survey reports** (✓ in this text; secondary, since they are the survey's citations of other papers):
  - SWE-World: 6.2% → 52.0% on SWE-bench Verified; CWM 65.8% (§4.2.2).
  - DreamGym: over 30% on WebArena.
  - A-Lab: 353 experiments and 36 of 57 targets in 17 days (§5.3).
  - Evolving Constitutions: +123% over human rules.
  - PhyWorldBench: the best of 12 models scores 0.262 (§8.2).
  - WorldModelBench: 7 domains, 350 prompts, 67K labels, 14 models (App. E.4).
  - ExploreToM: GPT-4o as low as 9%.
  - Attack results: PhysCond-WMA 55%, JailWAM 84.2% (§8.3). The authors say these are unreplicated preprints from a few overlapping groups.

**Evaluation practices it describes or criticizes** (§6, App. E)
- **FID, FVD, SSIM and pixel loss are "at best weak indicators"** of agentic capability. Mean success rates hide variance.
- **The right question** is "Does improved model validity change what the agent chooses, and does that shift … improve real-world task outcomes?"
- **Two proposed metrics:**
  - ASR (action success rate of a planner that uses the model).
  - COD(k), the counterfactual outcome deviation when two action sequences differ at step k.
  - App. E.2 adds an action-sensitivity ratio and a normalized counterfactual divergence.
- **Protocol, not benchmark, sets the level** (§6.3). Worked examples on RoboCasa, OSWorld/SWE-bench, Sotopia and ScienceWorld/DiscoveryBench:
  - L1 means single-step error.
  - L2 means mid-task perturbation, catastrophic-action fraction and recovery rate.
  - L3 means a distilled persistent asset (grasp rule, regression test, install skill, social strategy) that carries over to later trials.
- **Diagnosis of current practice** (§6.3, App. E): "The vast majority of current systems are evaluated only at L1." L2 protocols exist but are not standard. "L3 evaluation infrastructure … is essentially nonexistent outside autonomous science." Further gaps: benchmarks rarely report degradation-vs-horizon curves; intervention sensitivity "receives markedly less attention" than the other two conditions; MBPO-style horizon truncation is never itself evaluated as a coherence metric.
- **Correlation-not-model evidence** (§6.4): WorldSimBench, WorldModelBench, Vafa et al. 2024 (Myhill–Nerode: per-step tests do not ensure coherence) and Kang et al. 2025 ("case-based" mimicry in video models).
- **MREP (Minimal Reproducible Evaluation Package):**
  1. Version locking.
  2. Full trace logs.
  3. A failure taxonomy aligned with the 5 failure modes.
  4. Tail statistics (stratified bootstrap, IQM, performance profiles).
  5. Explicit boundary-condition mapping.

  They argue MREP is "precisely what an L3 gatekeeper needs" to promote or roll back an update.
- **Open evaluation challenges** (§6.5): saturation, evaluation gaming, the cost of human evaluation, and missing meta-evaluation.

**Open problems / future directions (complete list)**

The ten numbered problems (§8.2):
1. Physical faithfulness beyond visual plausibility (conservation, object permanence).
2. Metric-aware video world modeling: metric estimation across time, temporal composition, identity preservation, instruction grounding.
3. Programmable visual representation: unify SVG/Manim/Blender-code state into one 2D/3D world-model interface.
4. Partially observable software as a POMDP: beliefs over hidden backend state and asynchronous transitions (WAREX, Kara et al. 2025).
5. Concurrent multi-user state: a Dec-POMDP with CRDT semantics plus learned belief tracking.
6. Agent–human behavioral alignment at scale: consensus bias, mode collapse, multi-turn calibration.
7. Continual learning of societal transition functions: detect that dynamics shifted, gather evidence, revise without catastrophic forgetting.
8. Closing the surrogate-to-reality gap: how to allocate scarce real measurements between calibration and discovery.
9. Modeling laws that themselves evolve: second-order "meta-transition operators" for how p_θ drifts, plus triggers that detect law change.
10. Harness design as world modeling. The sequence is prompt → context → harness engineering; the problem is learning or synthesizing harnesses from interaction data and treating the execution environment as the object being modeled.

Cross-regime shared problems (§8.2): deployment shift; hard constraint enforcement at inference; persistent-update governance (the stability/plasticity/auditability trilemma, versioning, canaries, rollback).

Security and safety (§8.3): regime-specific threat models and benchmarks; the world model as a high-leverage attack surface; adversarial evidence contaminating L3 revision.

Beyond L3 (§8.4): meta-world modeling over alternative rule systems, possibly via program synthesis, open-ended evolution or procedural world generation. The question of what "the ultimate world model" is stays open.

Conclusion agenda (§9): causal representation learning (L1); law-consistent rollout and compositional generalization (L2); safe autonomous experiment design and revision (L3); and discovering symbolic governing laws from data, "one of the most important open problems."

**What it misses or gets wrong as of Nov 2026**

Compared against `round1/sweep/world-models-xunjian.md`, `round2/redteam/*`, `FINAL_REPORT.md` and `ONE_PAGERS.md`. v3 is dated 2026-06-16, so the post-June gaps are expected.
- **The self-improving-agent literature is absent even though L3 is self-revision.** Gödel Agent, DGM, Hyperagents, STOP and Reflexion get 0 mentions (✓, grep), as do Goodhart and reward hacking. L3 is framed through autonomous science and FunSearch/AlphaEvolve, not RSI. It therefore never separates the task agent from the improver (order 0 vs order 1). It also misses the finding that improvers rarely edit themselves and that gains are inherited from one early edit (Improvement by Inheritance, ZMmJDeDTXk ◐; `ONE_PAGERS.md`).
- **"Validation is the bottleneck" conflicts with project evidence.** `FULLTEXT_VERIFICATION.md` (✓ in an earlier session) finds that almost no published system score-rejects self-edits: DGM, Hyperagents and HGM keep every valid child. Natural proposal rates are near zero. In RSI settings, *proposal*, not validation, looks like the binding constraint.
- **It assumes predictive fidelity drives decision value.** It misses the counter-evidence in 2609.33335: mismatched prediction targets cut accuracy by 15–62% yet keep task gains, and a coin-flip reward expands pass@64 (◐ per project sweep). It also misses the 2026H2 decision-fidelity wave: DRPE 2609.32322, WRM 2610.38439, 2611.01139, Goodhart-in-the-Planner 2611.09670, "Events, Not Pixels" 2610.29435, and Learning in Dreams 2610.08033 (all ◐/† per project files). Its ASR/COD metrics are close to these, but it gives no exploitation law; √KL and safe-horizon results came later.
- **It misses the "world model of the self-improver" wave**, the meta-level that §8.4 only gestures at: IDM 2610.13991, SpecRSI 2610.16491, Rehearse 2607.27687 (confidence cliff), RSI-Anything 2610.13912 (the "coupled-update" problem) and LMs-as-research-WMs 2610.12235 (◐ per sweep).
- **It misses the drift and self-evolution robustness benchmarks** that speak directly to its open problems 7 and 9 and its "deployment shift" item: SkillShift-Bench 2610.33528, ReScopeBench 2611.05450, PACE-Bench 2608.14441 (design fixation), "Appreciation Teaches Unauthorized Execution" 2610.39122 and "Beneath the Surface" 2611.07624. It also lacks BreakingWeb 2609.35814's paired, recoverable intervention method, which is exactly the "UI failure injection" sensitivity protocol of Table 4 done well. Plus Qwen-AgentWorld 2606.24597 and the Text-WM survey 2606.09032.
- **Internal inconsistencies** (✓, all from this text):
  - Table 8 gives AI Scientist, AI Scientist-v2 and Co-Scientist a full design→reflect loop. The prose says AI Scientist-v2's "revision loop operates on paper quality rather than mechanistic understanding".
  - Table 10 marks SWE-bench as L3-capable, but §6.3 says L3 asset generation there is "not yet evaluated systematically".
  - Fig. 5 puts MuZero and Dreamer at L1, while Table 5 lists MuZero and DreamerV3 as L2 with all three conditions met.
  - The Fig. 10 caption swaps the social and digital panels.
  - §4 cites "Pearl's causal hierarchy (Section 2.1)", but Pearl appears only in App. B.4.
  - The two L3 boundary-condition lists (§2.4 vs §5.2) do not match.
  - App. A.3 contrasts Lakatos with Kuhn, then calls hard-core revision a Kuhnian "paradigm shift".
- **Method.** Level assignments, the S/M/W coverage labels and maturity tiers are judgment-based. There is no coded meta-analysis or inter-rater check, and the coverage matrix is not recoverable from v3. The L3 bar (persistent assets plus gates) is mostly an engineering checklist. It never asks whether revisions improve *the reviser*, or whether gates themselves overfit beyond one paragraph on "benchmark overfitting".

**Relevance to Dan's ideas**
- **Taught-Not-Selected (lead).** The survey gives an umbrella framing: harness evolution scored in a simulator is an L3 reflect loop running on an L2 simulator, and "any system used to generate synthetic training data implicitly serves as a world model" (§4). Two of its risks map onto TNS's two channels without being separated:
  - *Knowledge contamination* (§5.4), meaning biased evidence entering the revision loop, is the **taught** channel.
  - *Exploitability / simulator escape* (§4.3), meaning search exploiting loopholes, is the **selected** channel.

  That the survey distinguishes them yet never decomposes them is a clean "gap in a 92-page survey" citation. Its falsifiability principle (§5.3, Table 9) and "confident but wrong → evolve" rule predict that *legible* errors are what evolvers learn from. That is the TNS legibility hypothesis stated as a design recommendation with no test. The Qian et al. 2026 foresight finding (agents rarely use and often misuse rollouts) is a useful prior for the "reflector reading is inert" alternative branch.
- **Propose/Propagate/Pay-off funnel.** The §2.4 L3 conditions line up with the funnel. Evidence-grounded diagnosis, then persistent asset update, then governed validation roughly correspond to propose, then propagate through the gate, then pay off as "improves across revision cycles k without regressing on held-out probes" (§5.4). The survey asserts the bottleneck is validation, not proposal (§5.1). The funnel can test that assertion directly in the RSI setting, where project evidence points to Propose (near-zero natural self-edit rates). This is a quotable foil.
- **Meta-Overfitting.** "Benchmark overfitting: the regression gate is too close to the training distribution" (§5.4) is the product-level version. Meta-Overfitting extends it to the improver level and to domain distance. The survey's §8.4 "meta-world modeling" and open problem 9 (meta-transition operators for drifting laws) supply vocabulary for "improving the improvement procedure".
- **Meta-reward.** The survey's metrics are all order-0: ASR, task success, and "improves across revision cycles". Nothing grades whether the reviser got better at revising. That makes it a good example of the field default that the meta-reward one-pager calls the core assumption to audit.
- **Openness Ladder.** Three pieces line up with openness rungs: the three growth modes (parameters, architecture, hypothesis space), the Lakatos split (gradients adjust the belt; persistent errors need hard-core edits), and the "symbolic substrate required for L3" thesis. "Separate what is learned from what is enforced" (§7.3) argues for keeping the gate (L5, selection logic) non-editable. Their stability/plasticity/auditability trilemma is a ready-made theoretical framing for an inverted-U openness curve.
- **Continual / drift (Varying Goals, MetaShift).** Several pieces are direct hooks:
  - "Deployment shift" is their first cross-regime open problem.
  - Open problem 7: detect outdated dynamics, revise without forgetting.
  - Open problem 9: laws that themselves evolve.
  - The runtime claim that L3 should fire only when re-planning cannot absorb failures.

  The last one is in effect MetaShift's hypothesis: self-editing pays only under regime change. The survey states it as a design principle without evidence.
- **Register snowball.** Weak relevance. The closest hooks are the social-regime notion of *reflexive* transitions (beliefs about the state change the state), role and persona drift, and open problem 6 (mode collapse and consensus bias in LLM populations). These could frame register drift in human–model dialogue as a reflexive social-world dynamic, but the survey has nothing on linguistic register.
- **Bottom line for the project.** Cite it as the field's canonical L1/L2/L3 vocabulary and its "L3 needs governed validation, not just loops" framing. Use its gaps as foils: no order-1 notion, a "validation not proposal" bottleneck claim, no separation of the contamination and exploitation channels, and no RSI coverage. It is not a scoop threat to any of Dan's ideas.

---

### Bridging the Agent-World Gap: Text World Models for LLM-based Agents (2606.09032)

**Read status: ✓ full text.** I read the whole paper (§1–§8, Figures 1–9, Tables 1–2 and the reference list) from arXiv v1 via pdftotext, saved at /tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/2606.09032/paper.txt. Numbers marked ✓ below are as the survey states them. I did not re-check them against the primary papers unless I say so.

**Bibliographic**
- Authors: Yixia Li, Hongru Wang, Peng Lai, Zhiwen Ruan, He Zhu, Youxin Zhu, Ganlong Zhao, Minda Hu, Yun Chen, Sibei Yang, Peng Li, Jeff Z. Pan, Jia Pan, Guanhua Chen†, Yang Liu, Guanbin Li†. † marks the corresponding authors.
- Institutions: SUSTech (lead), Edinburgh, PKU, SYSU, CUHK, SUFE, Tsinghua, HKU.
- arXiv cs.CL, v1, 8 Jun 2026. No venue is stated, so treat it as a preprint.
- Length: 31 pages, about 23 of body plus references. 9 figures and 2 tables.
- Companion list: github.com/sustech-nlp/awesome-text-world-models.
- Link: https://arxiv.org/abs/2606.09032
- Overlap: Yixia Li and Hongru Wang are also first and second authors of Word2World (2512.18832), which the survey cites heavily.

**Scope & definitions (§1, §2.1)**
- **Definition of a text world model (TWM).** A TWM is defined at the level of the transition function: M : S × A → T_S, ŝ_{t+1} = M(s_t, a_t). T_S is the set of *textual renderings* of the successor state.
  - "Text" refers to how the transition is *exposed* to the agent. The input may be multimodal, such as a screenshot.
  - The substrate can be anything: a prompted frozen LLM, a trained predictor, or executable code.
- **What separates it from other models.**
  - From latent or pixel world models: its predictions are readable and editable text.
  - From an ordinary LLM: its role is explicitly counterfactual. In the authors' words, it "predicts how the interaction state would change if that action were taken."
- **What it simulates.** Two targets are covered: the external environment (web, repositories, APIs) and the *human user*, meaning preferences, intent and task progress. User simulation is treated as a world model throughout.
- **Why text is different (§1).** The authors name three "tensions" absent in pixel or vector worlds:
  - states are open-vocabulary and ambiguous;
  - dynamics are knowledge-laden;
  - correctness is semantic and subjective, with rarely a single right successor.
- **Positioning.** It claims to be "the first systematic review" of TWMs, as distinct from video, driving and embodied world-model surveys (Zhu 2025; Feng 2025b; Fung 2025; Li 2025c).
- **Organizing frame.** A lifecycle: build (§3) → train-time use (§4) → inference-time use (§5) → evaluate (§6). It is drawn with a feedback arrow back into improving the WM (Fig. 2).

**Taxonomy**

*A. Object-level two-axis grid (§2.2, Fig. 4).*
- **State representation axis**, from flexible to formal:
  - Natural language
  - Structured: JSON, KG, accessibility tree, PDDL predicates
  - Executable code: Python, TS, HTML, simulators
- **Grounding domain axis:**
  - Physical: embodied, game physics, disaster
  - Digital: web, OS, repositories, terminals, APIs, GUI
  - Social: users and dialogue
  - Abstract: planning, math, logic puzzles
- **Example cells:**
  - NL × Digital: WebDreamer, WMA, CUWM, Dyna-Think, DreamGym, R-WoM, Simia
  - Structured × Digital: WebWorld, WebEvolver, DynaWeb, RLVR-World, SWE-World, CWM
  - Code × Digital: Code2World, AutoWebWorld, CLI-Gym, AWM, EnvScaler, ScaleEnv, daVinci, AgentScaler, RLVE
  - NL × Social: UserRL, Echo-N1, HER, OpenClaw-RL, PAHF, UserLM, IDRBench
  - Structured × Social: τ²-Bench, Pep
  - Code × Abstract: Code WM (Lehrach), Agent2World, AutoEnv, Game-RL
  - NL × Physical: Dyna-Mind, LLM-MCTS, WorldMind, Word2World, Steve-Evolving

*B. Lifecycle taxonomy (Fig. 3, the main organizing tree).*
- **§3 Building.** The top-level split is LLM-as-WM (the transition is the LLM's forward pass) vs Code-as-WM (the LLM writes the WM, and the WM is code plus its executor).
  - 3.1 Learning-based
    - 3.1.1 SFT. Covers three design choices:
      - prediction target: full state (CWM, Word2World) vs delta (WMA, DynaWeb);
      - data source: real rollouts → self-play (SPA) → harvested at scale (WebWorld) → fully synthetic (Simia) or repurposed (UserLM);
      - data scale.
    - 3.1.2 RL, ordered by reward target:
      - surface fidelity: RLVR-World
      - semantic equivalence: RWML (embedding cosine), CUWM (LLM judge)
      - behavioral consistency: BehR
      - latent consistency: SWIRL (inverse dynamics with latent actions)
  - 3.2 Prompt-based
    - 3.2.1 In-context: WebDreamer, LLM-MCTS
    - 3.2.2 Retrieval: R-WoM, TRAD, WorldMind, MINDSTORES, WorMI
    - 3.2.3 Self-evolving: Test-Time Adaptation (Chen 2025a), Steve-Evolving, Evo-Memory
  - 3.3 Programmatic
    - 3.3.1 What code is generated: renderer (Code2World), FSM (AutoWebWorld, CLI-Gym), simulator (Code WM / OpenSpiel, TheoryCoder bilevel PDDL+Python)
    - 3.3.2 Scaling synthesis:
      - quality assurance: EnvScaler, ScaleEnv, AWM, AgentScaler, daVinci-Env
      - difficulty and diversity: RLVE, AutoEnv, Game-RL
  - 3.4 Cross-paradigm comparison along an implicit↔explicit spectrum (Fig. 6)
- **§4 Training time** (Fig. 7, Table 1)
  - 4.1 Internalized WM
    - Warm-start: SPA, Early Experience, RWML
    - In the reasoning trace: Dyna-Think, Dyna-Mind
  - 4.2 WM as training environment, ordered by coupling:
    - offline synthesis: WebSynthesis, Simia-SFT, AgentScaler, Word2World
    - online: DreamGym, Simia-RL, DeepAgent, SPICE
    - co-evolving: DynaWeb, WebEvolver
  - 4.3 User simulation
    - RL with simulated users: UserRL, Proactive (Sun 2025), Echo-N1, HER
    - Fidelity and personalization: UserLM, HumanLM, PAHF, Pep, OpenClaw-RL (live users)
- **§5 Inference time** (Fig. 8, Table 2)
  - 5.1 Simulator
    - Shallow lookahead (propose–simulate–score): WMA, WebDreamer, SimuRA, WALL-E 2.0
    - Deep tree search: LLM-MCTS, RAP, LATS, Agent Q, Code WM, TheoryCoder, LWM-Planner
  - 5.2 Verifier, ordered by how strongly it intervenes:
    - single-action gate: Word2World
    - rank: SWE-World, CUWM, FOREAGENT, INTENT
    - rewrite: WAC
- **§6 Evaluation** (Fig. 9)
  - 6.1 Evaluating the WM itself
    - Prediction accuracy and consistency: ByteSized32, Word2World (CR), BehR (CR_pw), R-WoM probes, WorldPrediction, ENACT
    - Task-driven: Yang 2026, Text2World, Task2Quiz
  - 6.2 WM as evaluation tool
    - Benchmark design: MobileWorldBench, AutoEnv, τ-bench, τ²-Bench, FUSE, LifeSim, LEWM, disaster "virtual sensors", World of Workflows
    - Simulator validity: UserLM, SimulatorArena, Zhou 2026 (sim2real user sims), Seshadri 2026 (Lost in Simulation), IDRBench, Interactive Benchmarks (Yue 2026), RECODE-H

**Key claims & findings** (all ✓ as stated in the survey)
- **Prediction target follows the observation space (§3.1.1).**
  - Full-state prediction suits compact spaces. Word2World reports about 99% single-step accuracy on ALFWorld and SciWorld.
  - Delta prediction suits large, redundant web and GUI observations.
  - WebDreamer's ablation found NL-delta formats competitive at short horizons but "degrades fastest as planning horizons extend".
- **Data needs scale with environment openness (§3.1.1).**
  - Closed simulators saturate at about 20k trajectories.
  - WebShop is still improving at 70k; StableToolBench is unsaturated at 160k (Word2World).
  - WebWorld uses 1.06M web trajectories with 0.09% CoT data. CWM uses 5T tokens of mid-training.
  - The authors' summary: "closed simulators saturate at 10^4, open-ended web tasks demand 10^6, and code execution requires … trillion-token scale."
- **Reality and cost trade off monotonically across data sources (§3.1.1).** Synthetic data (Simia: 90k transitions; UserLM: 384k repurposed conversations) is "most defensible when the prediction target is a short, local delta."
- **WM-RL rewards are moving from tokens to consequences (§3.1.2).** The progression is surface → semantic → behavioral → latent.
  - BehR's "metric inversion" is the key example. A state can be close in text and meaning to the truth yet drop the one decision-critical token.
  - The authors judge WM-RL "noticeably less mature than its SFT counterpart".
- **Prompted WMs hit a ceiling (§3.2.1).** Next-state identification exceeds 75%, but full-procedure planning alignment stays below 65% (R-WoM). R-WoM's grounded rollouts help up to about 3 steps on OSWorld.
- **Storing knowledge is not enough (§3.2.3).** In self-evolving prompt WMs, the bottleneck is "exposing this knowledge to the planner rather than merely storing it" (Steve-Evolving ablation).
- **Environment count and diversity are complements (§3.3.2).**
  - Count gives log-linear gains that "saturate quickly without diversity".
  - Scale points: EnvScaler 191 sandboxes; AWM 1,000+ SQLite-backed environments; daVinci 45K+ Docker environments from 10K+ repositories; RLVE 400 environments with a 90%-accuracy curriculum step.
  - Every verifier is domain-specific, so there is "no transferable standard for what counts as a 'correct' synthesized environment."
- **Choosing a paradigm (§3.4).**
  - Use code-as-WM if the dynamics have a closed form.
  - Otherwise use learned LLM-as-WM if data exists, else prompted.
  - The authors call hybrid code+LLM layers "underexplored".
- **Training-time effects (§4).**
  - Word2World: SFT on about 1K WM-generated trajectories matches the same amount of real data.
  - Simia-RL: RL on simulated environments beats RL on real ones on OfficeBench, which the authors call "counterintuitive".
  - DreamGym: gains on WebArena-Lite with zero real access.
  - WebEvolver: co-evolution breaks the frozen-WM self-improvement plateau.
  - Dyna-Mind: simulation quality and task success are positively correlated.
  - RWML: RL world-modeling causes less forgetting than SFT.
- **Two cross-cutting failure modes (§4.2.3, §4.4).**
  - **Hallucination drift**, also called coverage drift. Free-form simulators diverge as the policy explores states outside their training. Current fixes fall back to structured state (AWM's SQL) or constrained output (CUWM, Code2World). Keeping an open-ended LLM simulator faithful under on-policy exploration "remains open".
  - **The sim-to-real gap**, which is largest for user simulation.
- **Weak validation of user simulators (§4.3 takeaway).** User-simulator fidelity is benchmarked against prompted LLM users or LLM judges, "both of which are themselves simulators of the quantity they claim to measure", so improvements may reflect alignment with the evaluator.
- **Simulator vs verifier at inference (§5.3).** A simulator is needed when the policy cannot produce a usable candidate. A verifier presupposes a workable candidate set. Both are bounded by WM fidelity.
- **Prediction accuracy ≠ utility (§6.1).**
  - The prompted-vs-fine-tuned gap is "one of the most consistent empirical findings in this field".
  - Single-step exact match saturates on structured environments but collapses over multi-step rollouts.
  - Task2Quiz shows a dissociation: task success rate drops with difficulty while environment-understanding score stays stable.
- **Simulator faithfulness is now an evaluation target in its own right (§6.2–6.3).**
  - Zhou 2026: 31 simulators vs 451 participants over 165 tasks. Many simulators create an "easy mode" by being "excessively polite and forgiving".
  - Seshadri 2026: simulator faithfulness is uneven across demographic and dialectal groups.
  - UserLM lowers apparent agent success sharply compared with a prompted GPT-4o user.
  - MobileWorldBench: an 8B semantic WM adds +7.4% on AndroidWorld.
  - LEWM: removing emotional context costs up to 8% on subjective tasks vs about 1% on objective ones.
  - The authors' synthesis: "how to score a world model" and "how to train one" are converging along the same behavior-level axis.

**Evaluation practices it describes or criticizes**
- **Described:**
  - single-step exact match per attribute (ByteSized32), token F1, BLEU;
  - consistency ratio CR: the share of WM-successful trajectories that also succeed in the real environment (Word2World);
  - per-trajectory CR_pw (BehR);
  - discriminative action selection from counterfactual distractors (WorldPrediction);
  - multi-step forward and inverse reasoning (ENACT);
  - three-task utility battery of verification, generation and planning (Yang 2026);
  - PDDL executability plus component F1 (Text2World);
  - task success rate vs environment-understanding score (Task2Quiz);
  - simulator-hosted benchmarks: τ, τ², FUSE with Procedure Alignment, LifeSim's BDI users;
  - interaction cost–benefit (IDRBench);
  - budgeted interactive evaluation (Yue 2026);
  - feedback-granularity levels L0–L4 (RECODE-H).
- **Criticized:**
  - token or state-similarity metrics miss decision-critical content ("metric inversion");
  - single-step accuracy overstates long-horizon reliability;
  - task success is not a sufficient proxy for world understanding;
  - fine-grained metrics need ground-truth rollouts or hand-built probes, which do not scale across domains;
  - user simulators are validated by other simulators or LLM judges;
  - cooperative or polite simulators inflate agent scores and are unfair across dialects;
  - (§7.1) papers reporting isolated WM accuracy and papers reporting downstream agent reward "are not measuring the same object, even on overlapping benchmarks."

**Open problems / future directions it lists (complete, §7, plus open questions flagged in the section takeaways)**
1. **WM–policy coupling (§7.1).** The design choice between one LLM for both roles, a shared backbone with role adapters, and fully decoupled models.
   - Sharing lets WM errors enter the policy gradient and makes the two objectives compete invisibly.
   - Decoupling risks silent drift in state vocabulary or granularity.
   - There is "no a priori winner", and the choice should be reported explicitly.
2. **Reasoning world models (§7.2).** Make prediction an explicit reasoning task. The authors list three underexplored routes:
   - CoT distillation from stronger teachers;
   - joint CoT plus transition loss;
   - RL with process rewards that reward prediction by reasoning rather than memorization.
   They add an open question: how should reasoning depth scale with prediction difficulty?
3. **Unified cross-lifecycle architectures (§7.3).** One WM that serves prediction, planning and evaluation together, via multi-task objectives.
4. **World-model-aware agent design (§7.3).** Current agents invoke available WMs in fewer than 1% of episodes (Qian 2026). Agents need mechanisms for three decisions:
   - when to query, building on AVIC's gating;
   - how much to trust, building on FOREAGENT's confidence calibration;
   - how to correct, building on WAC's closed loop.
5. **Grounded TWMs (§7.4).** Tie text predictions to sensors, physical constraints and real consequences. The disaster "virtual sensor" paper, AVIC and MobileWorldBench are cited as early steps.
6. **Continual learning and adaptation (§7.4).** Websites, APIs and preferences are non-stationary. Long-term continual learning of TWMs without forgetting is called "unexplored"; only short-term test-time adaptation exists.
7. **Flagged in the section takeaways:**
   - hybrid code-grounded plus LLM-driven world models (§3.4);
   - a transferable correctness standard for synthesized environments (§3.3.2);
   - keeping open-ended LLM simulators faithful under on-policy exploration, i.e. hallucination drift (§4.2.3);
   - stability of co-evolution beyond web agents (§4.2.3);
   - user-model fidelity validated against humans rather than simulators (§4.3);
   - maturing WM-RL rewards toward decision preservation (§3.1.2);
   - scalable fine-grained WM metrics across domains (§6.1).

**What it misses or gets wrong as of Nov 2026** (compared against the project files; the cited papers were verified in earlier rounds per FINAL_REPORT, not re-read here)
- **Literature cutoff.** The citations stop at roughly April 2026; the latest is BehR, 2604.13824. The project files show a large Jun–Nov 2026 wave that it necessarily omits:
  - Qwen-AgentWorld 2606.24597
  - CoMAP 2606.02372 (co-evolving WM and policy)
  - Self-Evolving WMs 2606.30639
  - AWM-in-the-Wild 2610.26828
  - Agent-Editing WM 2609.28416
  - MKD2World 2611.06544
  - Agentic Language WMs 2610.06100
  - Dual-Frontier 2609.26293 (when to trust a WM)
  - False Prophets 2607.23147 (WM security)
- **It underplays evidence that prediction content may not drive gains.** "Does Learning to Predict the World Help Agents Act?" (2609.33335) trains on mismatched targets, lowering prediction accuracy by 15–62%, yet keeps the task gains. That directly challenges the survey's reading of RWML and Dyna-Mind, where simulation quality is said to correlate with success. The survey's internalized-WM section (§4.1) would need this as a counterpoint.
- **"Decision-relevant error" was later quantified more sharply.** DRPE (2609.32322): total error barely predicts planning success (ρ=−0.25), while decision-relevant error does (ρ=−0.84).
  - FULLTEXT_VERIFICATION warns that the pooled −0.84 comes from the controlled family; on learned models (n=7) both metrics are +0.43.
  - 2611.01139 shows WMs can rank policies well (τ up to 0.96) while frozen-WM optimization lowers real return.
  - These extend BehR's point beyond what the survey reports.
- **Simulator exploitation is under-covered.** Its "hallucination drift" is framed as the simulator drifting off-distribution. It does not treat the policy actively *exploiting* simulator errors. Missing examples:
  - The Simulator Says Yes (ICLR27 sub 0cb6zfwHm7): simulators approve 25–57% of actions the real executor refused, and GRPO stops refusing within 20 steps;
  - Learning in Dreams, Winning in Reality (2610.08033): exploitation is invisible from inside the dream;
  - One Frozen Simulator Is Not Enough (2608.12253): mode-collapsed user simulators lead to overfitting; population co-training adds up to +14%.
- **Its Simia-RL reading is uncritical.** It presents "simulated > real on OfficeBench" as evidence of "more consistent and explorable training signals". The exploitation literature above suggests caution.
- **No world models of the *agent itself*.** There is nothing on predicting self-modification or research outcomes:
  - IDM / Predict Before You Modify 2610.13991
  - SpecRSI 2610.16491
  - Rehearse 2607.27687 (a confidence cliff from 82.8% to 56.9%)
  - LMs as AI Research WMs 2610.12235
  - RSI-Anything 2610.13912 (the "coupled-update challenge")
  FOREAGENT (§5.2) is the only near-instance, and the survey files it as a verifier rather than as a meta-level WM.
- **Continual learning is not as "unexplored" as stated (§7.4).** By Nov 2026 there are environment-shift and experience-lifecycle benchmarks: SkillShift-Bench 2610.33528, ReScopeBench 2611.05450, PACE-Bench 2608.14441, and "Can Coding Agents Sustain Continual Evolution…" 2611.03219. Co-evolving WMs (CoMAP) are also directly relevant. They mostly target the agent rather than the WM, so the narrow claim about the TWM itself partly stands.
- **No harness, prompt or program evolution scored inside a simulator.** GEPA-style or reflective optimizers using a WM as fitness are absent. All training-time use in the survey is weight-level (SFT, RL, DPO) or memory. This matters for Taught-Not-Selected (below).
- **Minor issues:**
  - §7.1 cites Early Experience (Zhang 2025b) as the "single LLM playing both roles via prompting" regime. In §4.1 the survey itself describes it as a *training* warm-start, so the citation is miscast.
  - Fig. 1 lists several works never discussed in the text, e.g. KEA, Flipping, LostSim, InterBench, Sim2Real, BeyondSC, HWR, FailLev.
  - The "first systematic review" claim holds only for the text regime.
  - Many reported effects are qualitative ("substantially", "non-trivial") without numbers. That is fine for a survey, but those claims cannot be quoted as figures.

**Relevance to Dan's ideas**
- **Taught-Not-Selected (#1, a simulator-scored harness-evolution 2×2 of trace source × fitness source).**
  - Strongest relevance. The survey confirms the gap: its whole train-time branch (§4) is weight training or memory, and no surveyed work uses a TWM to score *harness or prompt evolution*, let alone splits simulator error into a trace channel and a fitness channel.
  - Useful citations for the motivation:
    - the hallucination-drift framing (§4.2.3);
    - BehR's metric inversion (§3.1.2), which supports keeping planted quirks decision-relevant;
    - the "easy mode" user simulators (Zhou 2026) and the "evaluator-alignment" warning (§4.3), as natural silent errors;
    - Simia-RL's "simulated > real" result as a puzzle that a selected-vs-taught account could explain.
  - It also supplies arm taxonomies. Pick the WM type for the δ₀ knob from offline / online / co-evolving coupling (§4.2) and from prompted, fine-tuned or code WMs (§3.4); code-as-WM gives the cleanest steerable errors.
- **Propose/Propagate/Pay-off funnel.**
  - §7.3 reports agents invoking an available WM in fewer than 1% of episodes. Steve-Evolving shows stored knowledge pays off only once it is *exposed* to the planner.
  - Both are direct analogues of "permitted ≠ realized" and of the proposed → retained → effective funnel: availability does not mean use, and use does not mean payoff.
  - The survey's three agent-design questions (when to query, how much to trust, how to correct) map onto the Propose, Propagate and Pay-off stages.
- **Meta-Overfitting.** Only indirect.
  - Word2World's openness-dependent saturation and AutoEnv's widening gap with diversity show home-environment specialization at the WM and agent level.
  - §3.3.2's "count saturates without diversity" and Game-RL's "diversity determines OOD" give analogous priors.
  - §7.1's point that WM-accuracy papers and agent-reward papers measure different objects is a good framing line for separating *improver* quality from *product* quality.
  - The survey has nothing on improvers or self-modification.
- **Meta-reward / credit κ.**
  - The WM-as-verifier family (§5.2) are verifier precedents for a meta-gate that scores candidate edits before running them for real: confidence-gated pairwise prediction in FOREAGENT; virtual test reports in SWE-World; the iterate-until-confident loop in WAC.
  - The survey omits the direct self-improver precedents (IDM, SpecRSI, Rehearse), which are the real competitors.
  - The behavior-level reward argument (§3.1.2, §6.3) supports scoring a meta-reward by downstream decision change rather than by output similarity.
- **Openness.** The implicit↔explicit construction spectrum (Fig. 6) and "environment count as a scaling axis" (§3.3.2) are relevant covariates. The data-requirement-scales-with-openness result (10^4 → 10^6 → trillions) is a quotable environment-side analogue for "gain depends on openness × budget". There is no proposer-side openness content.
- **Continual / drift.**
  - §7.4 names non-stationarity ("websites update, APIs change, and user preferences drift") and calls long-term continual TWM learning unexplored. Co-evolution is reported with "joint stability concerns and rare empirical evidence beyond web agents."
  - Both are citable as stated gaps for the drift arms.
  - PAHF's adaptation to preference drift is the only adaptation-rate result the survey gives.
  - Pair it with the project's newer shift benchmarks (SkillShift, ReScopeBench, PACE-Bench) and BreakingWeb-style interventions, which the survey does not cover.
- **Register snowball.** Relevant via §4.3 and §6.2.2:
  - LLM user simulators are "excessively polite and forgiving" (Zhou 2026), are unevenly faithful across dialect groups (Seshadri 2026), and drop agent success sharply when replaced by UserLM.
  - This implies that simulated users carry an assistant-like register, a confound for any snowball study that uses LLM users. It supports the REVIEW.md design of a scripted template user plus a real-human existence check.
  - HumanLM's latent-state user modeling and LEWM's joint emotion-state prediction are candidate realistic-user baselines.
  - The survey itself never studies register or style drift across turns.

---

### Adaptation of Agentic AI: A Survey of Post-Training, Memory, and Skills (2512.16301)

**Read status: ✓ full text.** I read v3 (9 Mar 2026), every section from §1 to §10 plus the tables and figure captions. I skimmed the reference list for specific refs only. Local text: /tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/2512.16301/paper.txt (compacted body: …/2512.16301/body.txt). No project knowledge file cites this survey; grep for "2512.16301" across research/ found nothing.

**Bibliographic**
- **Authors:** Pengcheng Jiang, Jiacheng Lin and Zhiyi Shi (equal first authors), plus about 30 more: Zifeng Wang, Heng Ji, Yejin Choi, Dawn Song, Jimeng Sun, Pan Lu, Jiaxuan You, Hao Peng, Jingbo Shang, Manling Li, Chao Zhang and others. Corresponding author is Jiawei Han (UIUC).
- **Organizations:** UIUC (lead), Stanford, Princeton, Harvard, UC Berkeley, Caltech, UW, UCSD, Georgia Tech, Northwestern, TAMU, MGH, Keiji AI, Unity.
- **Dates and venue:** v1 posted 2025-12-18; v3 dated 2026-03-09. arXiv preprint (cs.AI), with no venue listed.
- **Length:** 89 pages. Body is about 68 pages; references run to about 409 entries.
- **Links:** https://arxiv.org/abs/2512.16301 · companion list at github.com/pat-jj/Awesome-Adaptation-of-Agentic-AI
- **Literature cutoff:** stated as "through early 2026" (§1, Survey methodology).

**Scope & definitions** ✓
- **Adaptation** means "improving an agent, its tools, or their interaction after pretraining" (abstract). §2.2 splits it into prompt-based adaptation and fine-tuning-based adaptation.
- **Agent** = the foundation model that does the reasoning and control (θ). **Tool** = any callable component other than the agent model: APIs, ML models, subagents, retrievers, planners and memory (§3.1).
- **Three mechanisms** organize the survey: post-training (SFT, preference optimization, RLVR), memory (episodic buffers, reflective databases, knowledge graphs) and skills. Skills are "reusable units of procedural knowledge encoding how to perform tasks" (§1, citing Wu & Zhang [14]).
- **Scope limits:**
  - Single-agent systems only; multi-agent work is deferred to other surveys (§2.1).
  - Classification is by "dominant locus of optimization"; boundary cases are flagged explicitly (§1).
  - The authors warn that their cross-paradigm numbers come from case studies that are not controlled comparisons (§1, §6.4).
- **Memory convention (§3.2, footnote 1):**
  - External non-parametric memory written by a frozen agent's outputs counts as T2.
  - Pre-trained or plug-in memory counts as T1.
  - Parametric or hybrid memory (LoRA knowledge injection, Titans, Memory³) is a boundary case. It is A1/A2 if the agent's own weights get gradient updates, and on the T1/T2 boundary if only auxiliary parameters change.
- **Notation (§3.1):** offline data D, environment E, and objective O. O_tool scores tool outputs; O_agent scores the agent's final output.

**Taxonomy** ✓ (the 2×2 in §3.2, Figs. 1, 3 and 7; representative papers come from Tables 1–4 and §4–5)

The two axes are **what is adapted** (agent vs tool) × **where the signal comes from** (tool execution vs agent output). For tools, the second axis is really agent-independent vs frozen-agent-supervised.

- **Agent Adaptation (§4)**
  - **A1: Tool-Execution-Signaled.** A* = argmax O_tool(A,T); the reward is the tool's own outcome (§3.2.1, §4.1, Table 1).
    - *SFT and off-policy (§4.1.1).* These are ordered by how correctness is defined:
      - Self-supervised: Toolformer (keeps an API call only if it lowers perplexity).
      - Golden-answer alignment: ToolAlpaca, TRICE, TP-LLaMA (DPO on failed branches).
      - Golden-format alignment: Gorilla (AST matching), ToolFlow.
      - Direct-execution alignment: CodeAct, NExT, ToolLLM, AutoTools, LeReT (IPO), RetPO (DPO), CYCLE.
    - *RLVR (§4.1.2)*, by domain:
      - Search/IR: DeepRetrieval, ReZero, Orion.
      - Code: LeDex, RLEF, Code-R1, R1-Code-Interpreter, Tool-R1, DeepSeek-R1-Zero (code).
      - Theorem proving: DeepSeek-Prover-V1.5/V2, Kimina-Prover, Leanabell-Prover-V2, Goedel-Prover-V2, AlphaProof.
      - Multi-tool: Router-R1, FTRL, Tool-N1, WebGen-Agent, ToolExpander.
      - Other domains: Rec-R1, SQL-R1, olmOCR 2.
  - **A2: Agent-Output-Signaled.** A* = argmax O_agent(A,T); the reward is on the final answer, plan or trace (§3.2.2, §4.2, Table 2).
    - *Without tools (§4.2.1):*
      - Scalar RL, "the R1 paradigm": DeepSeek-R1-Zero (math), Kimi k1.5, Magistral, EHRMind, KnowRL (calibration as the reward), Empower (human empowerment as the objective), GRACE.
      - Inference-time self-refinement: Self-Refine, SCoRe (trained self-correction), RISE.
      - Textual-gradient feedback: TextGrad, metaTextGrad.
    - *With tools (§4.2.2):*
      - Retrieval: Self-RAG, RPG, RAS, R1-Searcher, Search-R1, ReSearch, AutoRefine, ZeroSearch, StepSearch, MMSearch-R1, DynaSearcher, DeepResearcher, MedResearcher-R1.
      - Code: ReTool, CodePRM.
      - General multi-tool: FireAct, Re-ReST, Agent-R, Self-Challenging Agents, TT-SI (test-time fine-tuning), ToolRL.
      - Infrastructure: Agent Lightning, A²FM, VerlTool.
- **Tool Adaptation (§5)**
  - **T1: Agent-Agnostic.** T* = argmax O_tool(T); tools are trained independently and plugged into a frozen agent (§3.2.3, §5.1).
    - *Orchestration patterns:* Neural Operators/FNO, HuggingGPT (prompt-based), ViperGPT (code-based), SciToolAgent (knowledge-graph based), Visual ChatGPT (multimodal bridging), and MCP / "Code Execution with MCP".
    - *Tool categories (§5.1.2):*
      - Vision: CLIP, SAM, SAM-CLIP.
      - Speech: Whisper.
      - Code: CodeAct.
      - Retrieval: DPR, ColBERT, Contriever, e5.
      - Science: AlphaFold2, ESMFold, CGCNN, molecular encoders.
    - *"Graduated" subagents:* A1/A2-trained agents that are frozen and reused, e.g. DeepRetrieval, Code-R1, SWE-Grep (§5.1.2, §6.3.1).
  - **T2: Agent-Supervised.** T* = argmax O_agent(A,T); the agent is frozen and its outputs supervise the tool (§3.2.4, §5.2, Table 3).
    - *Earlier proxy-signal and preference methods (§5.2.1):*
      - Perplexity/KL proxy: REPLUG.
      - Black-box adapters: BLADE, BBox-Adapter, proxy-tuning, EVOR.
      - LM-preference retrievers: AAR, RA-DIT.
      - Multi-stage distillation: LLM-R, UPRISE.
      - Bridge models: BGM. Also CoBB, MedAdapter, ARL2, ToolkenGPT, Sysformer.
    - *Subagent-as-tool, RLVR era (§5.2.2):*
      - (i) Agentic searchers: s3 (Gain-Beyond-RAG reward), DynamicRAG, QAgent.
      - (ii) Memory-construction subagents: Mem-α, AutoGraph-R1.
      - (iii) Meta-cognitive control: AI-SearchPlanner, Advisor Models, Matryoshka Pilot; orchestration: AgentFlow (Flow-GRPO).
      - (iv) Self-evolving subagents: R-Zero (Challenger/Solver), Multi-Agent Evolve (Proposer/Solver/Judge).
    - *Agentic memory and skills (§5.2.3):*
      - Dynamic stores: MemGPT, Memory OS, Generative Agents, A-MEM, HippoRAG, SHIMI, Gutiérrez et al. (memory as non-parametric continual learning).
      - Experiential/reflective memory: Reflexion, Retroformer, Think-in-Memory, Agent Workflow Memory, Pan et al.
      - Structured memory: AriGraph, ChatDB, tree memory, Zep (temporal knowledge graph).
      - Parametric/hybrid (boundary cases): Memory³, Titans.
      - Episodic memory as a trainable policy: Memento (soft-Q case retrieval).
      - Memory operations as skills: MemSkill.
      - Test-time curation: Dynamic Cheatsheet, ReasoningBank.
      - Skill libraries (Table 4, granularity × acquisition):
        - Executable code: Voyager, OS-Copilot, CRADLE (exploration); PAE (RL).
        - Heuristics: EXPEL (reflection).
        - Trajectories: Synapse (demonstration); JARVIS-1 (exploration).
        - API/MCP skills: SkillWeaver (exploration); CASCADE (reflection).
        - Compositional programs: ASI.
        - Synthesized tools: LATM, CREATOR.
        - Specialist agents: AgentStore.
        - Also covered: AppAgent, Agent S, ExACT, SAGE (skill-integrated reward), LEGOMem, and ADAS (a "second-order" skill process).
      - Embodied skills: SayCan, ProgPrompt, Eureka, RoboGen.
      - Memory navigation: MemWalker, ReadAgent, CoRAG, Adaptive-RAG.
      - Embedding/interface adaptation: ToolkenGPT, UniMuR, DIFO, V2L Tokenizer.
- **Interpretive 2×2 (Fig. 7, §6.5).** The authors say this is not the definitional taxonomy. x-axis runs monolithic → modular; y-axis runs local tool mechanics → global orchestration.
  - A1 = mastery of tool mechanics; A2 = strategic coordination; T1 = plug-and-play; T2 = "federated cognition".
  - A1 → T1 is the "graduation path"; A2 → T2 is the "federation path".

**Key claims & findings** ✓
- **Three trade-offs (§1, §6.1, Table 5).**
  - Cost: A1/A2 are expensive with high parametric flexibility; T1/T2 are cheap with system-level flexibility but are capped by what the frozen agent can do.
  - Generalization: T1 transfers broadly, while A1 can overfit to its environment.
  - Modularity: T2 lets you swap tools; A1/A2 risk catastrophic forgetting. Table 5 labels T2 "Symbiotic, No Forgetting".
- **A1 vs A2 signal (§4 intro, §6.2).**
  - A1 gives dense, causal, per-action credit but is "blind to whether the agent's overall reasoning strategy is sound".
  - A2 can teach when and whether to use a tool (meta-policy), and is where emergent behaviors appear: ReSearch's reflection and self-correction. Its costs are sparse reward, hard credit assignment, and exposure to shortcut learning.
- **Four cross-domain principles for A1 RLVR (§4.1.2).**
  - (1) Signal density drives learning efficiency.
  - (2) "Reward quality dominates reward quantity" (Code-R1, DeepRetrieval).
  - (3) Format rewards are necessary but not sufficient.
  - (4) Stabilizers are the same in every domain: KL, dynamic sampling, curricula.
- **A2 synthesis (§4.2.1).** "SFT warm-up followed by RL yields more stable training than RL alone." Off-policy SFT/DPO cannot explore strategies that are absent from the data; on-policy RLVR removes that ceiling (§4.1.1).
- **Flagship number, T2 vs A2 (§5.2.2, §6.4, Table 6).** s3 reaches 58.9% average accuracy with 2.4k samples, about 70× less data than Search-R1's ~170k and 33× faster wall-clock. On medical QA, s3 trained on general QA scores 76.6% vs Search-R1's 71.8%.
  - The authors caveat this explicitly: it "is not a controlled experiment", because optimization target, backbone and architecture all differ.
  - Their explanation is that T2 only has to learn procedural skill, since the frozen generator already has the knowledge and reasoning ("decoupling of skill from knowledge", §6.3.2).
- **Other numbers it quotes (✓ as reported in the survey; not re-checked against the primary papers):**
  - DeepRetrieval recall 65.1% vs 24.7% (§4.1.2).
  - AgentFlow 7B planner: 33.1% on GAIA, "beats GPT-4"; +14.9% on search tasks over AutoGen (§5.2.2).
  - Memento: 87.88% GAIA validation, 79.40% GAIA test, +4.7–9.6 pp out-of-distribution from case memory (§5.2.3).
  - Mem-α generalizes from about 30k- to over 400k-token contexts.
  - BGM 35.6% vs 25.8% EM on HotpotQA.
  - TextGrad LeetCode-Hard 26% → 36%.
  - AWM −24% planning errors.
  - EXPEL +18% / +12% on ALFWorld / WebShop.
  - Voyager 63 items (3.3×).
  - LATM −79% cost.
  - Eureka reward ≥ human-designed in 83% of 29 tasks.
- **QAgent lesson (§5.2.2).** A searcher rewarded on its *own* answer reward-hacks toward shallow, easily copied evidence. Switching the grader to a stronger frozen generator fixes it. The general principle: "the frozen generator should not only consume a tool's outputs but also supervise its learning."
- **Cascaded T2.** Two to three adapted stages (reformulator → retriever → reranker) "strike a good balance" (§5.2.1). This is asserted without a specific citation.
- **Memory and skills are "two facets of the same adaptation mechanism"** (§5.2.3). Memory is the substrate; skills are the executable content. Skill lifecycle: acquisition → representation → invocation → refinement. This is mapped onto Hu et al.'s case → strategy → skill hierarchy.
- **Graduation lifecycle (§6.3.1).** Train (A1/A2) → freeze → deploy as a T1 subagent-as-tool. Examples: DeepRetrieval, SWE-Grep.
- **Strategic recommendations (§6.5).**
  - A1 for mastering verifiable tools in stable domains.
  - A2 for holistic orchestration across many tools.
  - T1 for horizontal reuse.
  - T2 for data-efficient, forgetting-free add-ons, bounded by the supervisor's quality, with error compounding as a risk.
  - Predicted trend: "frozen foundation models at the center, surrounded by a modular set of T1/T2 subagents", with occasional A1/A2 "evolutionary leaps".
- **Dominant paradigm per application domain (Table 9, §8):**
  - Deep research: T2 (AgentFlow); bottleneck is the lack of deterministic signals.
  - Software development: A1 (RLEF); bottleneck is long-horizon credit across multi-file edits.
  - Computer use: A2 (OpenCUA); bottleneck is visual grounding, with no "test suite" for GUI actions.
  - Drug discovery: T1 (AlphaFold2); bottleneck is sparse wet-lab feedback that takes weeks to months.
- **Conclusion's highlighted findings (§10):** signal density shapes which paradigm works; evaluation is paradigm-dependent; graduation bridges paradigms; memory and skills span the whole taxonomy.

**Evaluation practices it describes or criticizes** ✓ (§7, Tables 7–8)
- **Benchmarks mapped to paradigms (§7.1):**
  - A1: HumanEval, MBPP, LiveCodeBench, SWE-Bench, MTEB, MiniF2F, BFCL.
  - A2: GSM8K, GPQA, IFEval, Arena-Hard, GAIA, AgentBench, HLE.
  - T1: MTEB and standalone tool evaluations.
  - T2: "least standardized", "ad hoc… an ablation within individual papers".
  - Integrated: WebArena, OSWorld, τ/τ²-Bench, GTA, AgencyBench (about 90 tool calls and about 1M tokens per task), Tool Decathlon.
  - Key criticism: "No existing benchmark suite supports controlled comparison across all four paradigms on the same task distribution."
- **A1–A2 diagnostic gap.** High A1 with low A2 points to a synthesis bottleneck. High A2 with low A1 points to memorization or shortcuts. High pass@k with low code quality means test-gaming.
- **T2 needs counterfactual evaluation:** hold the agent fixed and vary only the tool. The survey also notes:
  - Cross-agent transfer of T2 tools is barely tested; s3 tested only Qwen2.5-14B and Claude.
  - LongMemEval shows temporal reasoning and knowledge updates are still weak.
  - No benchmark measures skill-lifecycle quality, i.e. whether a skill library "improves through reuse or degrades through drift".
  - Skill-Inject and Agent Skills in the Wild measure attack exposure, not marginal utility.
- **Signal metrics (§7.2).**
  - Execution metrics are local and can be gamed. The survey cites HAL agents that searched online for benchmark answers.
  - Exact match conflates reasoning with answer extraction.
  - LLM-as-judge brings verbosity, position, self-enhancement and sycophancy biases; long tool-heavy traces may get inflated scores. Ensemble judges help partially.
  - Holistic metrics hide where credit belongs ("credit-assignment opacity").
- **Central critique: endpoint-only reporting (§7.3, §7.5.1).** The authors argue that phase transitions, reward–likelihood divergence, tool-use drift and reward-hacking episodes are "dynamical in nature and invisible to any single-point measurement". Their examples:
  - A 70× data gap at equal accuracy.
  - Identical pass@k with different entropy trajectories.
  - DeepSeek-R1's non-monotonic safety regression, a "vulnerability window".
- **Proposed dynamics metrics.**
  - Entropy H(π_t) and per-update ΔH, citing Cui et al. and Hao et al.
  - Reward-curve shape, tool-call frequency and diversity, KL from the reference policy.
  - Backward transfer measured on a retention set.
  - For co-adaptation they propose three metrics — sliding-window variance, frequency of gradient sign flips, and convergence rate — and say plainly that these are "theoretical propositions that require future empirical validation" (§7.3.3).
- **Systemic evaluation (§7.4).**
  - Decompose cost into prompt, completion, tool overhead and retries; report accuracy as a function of token or time budget.
  - Report training GPU-hours.
  - Safety: unsafe exploration (ToolEmu, R-Judge), reward hacking, and safety checks at every checkpoint.
  - Paradigm-specific hackability: A1 is harder to hack, A2 easier, and T2 allows "tool-level reward hacking".
  - Alignment includes agent–tool format alignment (BGM).
- **Proposed reporting standard (§7.5.3):** (i) learning curves over at least 3 seeds; (ii) cost-conditioned performance; (iii) retention-set results; (iv) safety-trajectory plots whenever RL is used. It also warns about "benchmark co-adaptation" and calls for multi-dimensional Pareto leaderboards.
- **Caveat on counterfactual swap-in (§7.5.2).** The agent adapts to whatever tool it was trained with, so swapping a tool out is confounded. They suggest progressive ablation instead.
- **Living benchmarks (§7.5.4).** Static suites saturate; GAIA approached human level within months. Self-evolving benchmarks (in the R-Zero/MAE style) have three costs: ongoing evaluation cost, a "reproducibility crisis", and the difficulty of validating generated tasks. Their recommendation is versioned static snapshots plus an evolving component.

**Open problems / future directions** ✓ (complete list from §7.5.4 and §9)
1. **Co-adaptation (§9.1).**
   - Joint max over A and T as a bi-level problem; every current paradigm freezes one side.
   - Related fields to draw on: co-evolutionary algorithms (Hillis host–parasite; cycling and disengagement), and MARL non-stationarity (CTDE, opponent modeling).
   - Barrier 1: credit assignment across agent and tool. MATPO covers only prompt roles inside a single LLM; joint prompt and tool-spec refinement is another partial attempt [335].
   - Barrier 2: the stability–plasticity dilemma, including a "Red Queen" regime, collapse, or premature lock-in to a brittle interface.
   - Suggested remedies: "pacemaker" mechanisms that regulate relative learning rates, and evolutionary game theory to guarantee stable symbiotic equilibria.
2. **Continual adaptation (§9.2).** Non-stationary task, tool and user streams; "Self-Evolving Agents".
   - Parameter-update mechanisms (dynamic A1/A2): EWC, LwF, VR-MCL, orthogonal updates, LoRA, MoE, merging.
   - External-memory mechanisms (evolving T2): replay selection, use and compression; dual memory; prompts as external memory.
   - Theorem proving with growing Mathlib as a testbed: update premise indices rather than the policy.
   - The authors highlight "RL's Razor"-style evidence [292, Chen et al. "Retaining by doing"]: on-policy RL with reverse KL forgets less than SFT. Even so, they recommend pushing as much long-term adaptation as possible into T2 modules.
3. **Safe adaptation (§9.3).**
   - Risk I, unsafe exploration: the reward–safety gap, irreversible tool actions, and erosion of guardrails, with DeepSeek-R1 as the case study.
   - Risk II, parasitic adaptation:
     - Type A: specification gaming (agent as parasite).
     - Type B: adversarial tooling or prompt injection via MCP and skills. Cited figures: 26.1% of marketplace skills are vulnerable; Skill-Inject reaches up to 80% attack success.
     - Type C: sycophancy loops and Red-Queen overfitting between agent and red-team tool.
   - Mitigations: a pre-tool safety-check layer, constrained policy optimization and shields, verifiable rewards, Specification Self-Correction, Proof-of-Use.
4. **Efficient adaptation (§9.4).** LoRA in RL ("LoRA Without Regret": low rank is enough); quantized rollouts (FlashRL with truncated importance sampling); on-device personalized adaptation using small per-device tool modules. These three can be combined.
5. **Evaluation agenda (§7.5.4).**
   - Dynamic, persistent environments in which tools change.
   - One task distribution that supports all four paradigms (A1, A2, T1 and T2 protocols together), possibly via protocol-based assessors (AAA with A2A and MCP).
   - Longitudinal multimodal and multi-agent adaptation benchmarks.
   - Trajectory, forgetting, efficiency and safety metrics in one harness.
   - Living benchmarks paired with versioned snapshots.
   - Controlled cross-paradigm comparisons (§6.4, §10).

**What it misses or gets wrong as of Nov 2026** (compared against LITERATURE.md, FINAL_REPORT.md, round3/REPORT.md and FULLTEXT_VERIFICATION.md)
- **The harness/prompt-evolution and RSI line is almost entirely absent.** Full-text grep finds zero mentions of GEPA, DSPy, OPRO, Darwin-Gödel Machine, Gödel Agent, AlphaEvolve or "self-referential". ADAS gets one paragraph as a "second-order skill management process". Prompt optimization is waved away to Sahoo et al. (§2.2.1).
  - In the taxonomy, reflective harness and prompt evolution (GEPA, ACE, Meta-Harness, AHE, HyperAgents) would land awkwardly between T2 ("prompts as external memory", §9.2) and A2 inference-time refinement. The survey never treats the improver itself as an adaptation target.
  - That improver-as-target question is exactly where most 2026 work moved: RSI-Harness, MetaEvoForge, Improvement by Inheritance, Learning to Repair the Learner, SelfImprove-Bench 2611.02797, Beneath the Surface 2611.07624 (FINAL_REPORT §"Meta-Overfitting"; LITERATURE.md).
- **No treatment of acceptance gates or selection statistics.** Keep-if-better appears only implicitly, as "success filtering". The survey has nothing on PACE 2606.08106, SAGE-gate 2609.36043, winner's-curse corrections, or seed-variance null bands (Trace Evolution Bench 2610.15055). Its §7.5.3 call for "≥3 seeds" is far weaker than what the 2026 eval-rigor literature now requires.
- **Its co-adaptation open problem has since been attacked.** §9.1/§7.3.3 says "no existing work has systematically measured co-adaptation stability". Our files list VACE 2609.37105 (gated alternation of RL and harness edits, +4.6/+7.0 over ungated), HERD 2610.31401 (harness → weight self-distillation over 25 stages), RoboACE 2611.13034, and "Evolving Together/Failing Apart". The survey's co-adaptation framing (pacemakers, Red Queen) is still a useful vocabulary, but the claim of novelty no longer holds.
- **Continual adaptation is now benchmarked.** "Are Self-Evolving Agents Continual Learners?" (2610.25858) finds a plasticity–stability trade-off for GEPA, Meta-Harness and ReasoningBank (r = −0.72), and finds self-modifiers overwriting each other. PACE-Bench 2608.14441 shows design fixation after physics shift.
  - This contradicts the survey's "T2: Symbiotic, No Forgetting" label (Table 5) and its claim that Voyager-style libraries accumulate "without catastrophic forgetting". In 2026 results, external memory and skill stores do anchor and interfere. They only avoid *weight* forgetting.
- **Weak on simulators and world models as adaptation signals.** Simulator-scored self-improvement (learned environment models, steerable error) is not discussed; "world model" appears only inside the AriGraph citation title. Our files cover this in the world-models sweep and with Simulator Says Yes (0cb6zfwHm7) and the VHE / Improvement Fidelity ICLR27 submissions.
- **T2 overstated on two points:**
  1. The s3 vs Search-R1 70× figure is the survey's headline number, despite its own caveat that the comparison is confounded.
  2. It calls T2 "high generalization", yet the only cross-agent evidence is s3 tested on two generators.
  
  2026 evidence on skills generalizing to held-out tasks (2609.39148) and on SkillsBench-style evaluation (Skill∞ 2610.35434) is mixed and absent here.
- **Minor errors:**
  - Self-Refine and SCoRe are both cited as [101] in §4.2.1, a duplicate-citation slip.
  - RA-DIT is labeled "ICLR 2023" in the text but ICLR'24 in Table 3.
  - "Search-R1 [108]" in §7.1 points at the R1-Searcher reference.
  - The safety section treats DeepSeek-R1's "temporary erosion then partial restoration" as an established dynamic; only the erosion is cited.
  - OpenClaw is name-dropped in the abstract but never discussed.
- **Missing safety-dynamics work from 2026:** reward hacking → emergent misalignment (2609.06649, 2609.14998), evolutionary safety of RSI (2609.31186), and the Safety in Self-Evolving Agents survey (2610.00093). Its own "parasitic adaptation" framing would have accommodated them.

**Relevance to Dan's ideas**
- **Taught, Not Selected (trace channel vs fitness channel in simulator-scored harness evolution).**
  - No direct overlap; the survey never splits *what the learner reads* from *what selects it*.
  - Useful framing: in its terms, reflective harness evolution is T2 with a two-part supervision signal, ω(o) quality weighting (selection) plus φ(a,y,o) "output-consistency" targets (§3.2.4). Those map cleanly onto Dan's fitness channel and trace channel. Citing §3.2.4's two T2 supervised forms could ground the 2×2.
  - The QAgent lesson (§5.2.2) is a nearby precedent: changing *who grades* fixes reward hacking. That is a selection-channel story and a foil.
  - The ReasoningBank and Dynamic Cheatsheet curator that learns from self-judged failures is the "legible vs silent error" setting with no ground truth.
- **Propose / Propagate / Pay-off funnel.**
  - The survey's generic skill-library architecture is: frozen agent generates experience → curation (verification, reflection or success filtering) selects what to keep → retrieval makes it available (§5.2.3 summary). That is essentially propose → propagate → use. The survey neither measures attrition at each stage nor distinguishes edits to the improver.
  - Its §7.1 gap statement — no benchmark asks whether a skill library "improves through reuse or degrades through drift" — is the pay-off stage. The funnel paper can cite that sentence as an explicitly named open gap.
- **Meta-Overfitting (evolved improvers transfer only near home).**
  - Not covered as such. Closest material:
    - The A1 overfitting warning (§1, §6.5: A1 specialization "often generalizes poorly across tasks or tool interfaces").
    - The cross-agent generalization gap for T2 (§7.3.2).
    - ADAS as a second-order process.
  - The survey's claim that "narrower optimization targets generalize more robustly" (§6.4) gives a testable opposite-sign hypothesis: a procedural-only improver should transfer better than one carrying memory. That fits ECD's procedure × memory split.
- **Meta-reward / gate credit (κ).**
  - The survey's A1-vs-A2 "signal source as a reliability axis" (§6.2.2) is the object-level version of this.
  - The paradigm-specific hackability ranking (A1 < T2 < A2, §7.4.2) and "tool-level reward hacking" for T2 are citable motivation for why a meta-gate needs replay on frozen post-shift states.
  - The survey offers no meta-level reward design.
- **Openness (reach × credit ladder).**
  - The survey's agent/tool split is a coarse two-rung version of Dan's editable-surface ladder: tool ≈ L1–L3, agent weights ≈ L5, and the improver (L4) is missing.
  - Its "graduation" (A1 → T1) and "federation" (A2 → T2) paths are movements along the ladder.
  - Dan's claim that openness pays where drift places the cause is consistent with the survey's §9.2 recommendation to "shift as much long-term adaptation as possible into T2". Dan's law would predict when that advice fails: when drift hits the procedure or the verifier.
- **Continual / drift.**
  - Directly relevant: §9.2 (CL taxonomy; on-policy RL forgets less [292]; Mathlib-growth testbed), §7.3.3 (retention sets, backward transfer, entropy/ΔH logging, the three co-adaptation metrics), and §7.5.4 (persistent evolving environments).
  - Dan's continual benchmark can adopt the survey's proposed reporting standard (seeds, retention set, cost-conditioned curves) and empirically validate the co-adaptation metrics the survey flags as unvalidated. That is a ready-made contribution.
- **Register snowball (onset, slope, plateau, ceiling of assistant register in human–model dialog).**
  - Only tangential. The relevant pieces are the LLM-as-judge bias list, especially verbosity and self-enhancement (§7.2.2); Type C sycophancy loops in co-adaptation (§9.3); and on-device personalization and persistent user memory (§9.4).
  - It has nothing on stylistic drift in dialog. The survey's "Red Queen / sycophancy equilibrium" vocabulary could still describe a user–model register feedback loop.
- **Best use for the project:** cite it as the canonical 2025–26 taxonomy for "who is adapted × what signal"; use the §7.3/§7.5 dynamics-and-reporting critique as motivation; and position Dan's work in the improver/gate layer the survey leaves out. Its §9.1 and §7.3.3 novelty claims are outdated (VACE, HERD, 2610.25858), so cite those papers alongside it rather than repeating the survey's "no work exists".

---

### Autonomous Research Agents: A Survey of AI Scientists and the Verification Gap (2608.05179)

**Read status: ✓ full text.** I downloaded the PDF (119 pages) and read every body section (§1–16) and Appendices A–F, including Tables 1–20. I only skimmed the reference list (about 2,100 lines), to check individual citations. Text is at `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/2608.05179/paper.txt`. Section and table numbers below are the paper's own. The paper does not appear in any project knowledge file, so it is new to the project.

---

**Bibliographic**
- **Authors:** Tianyu Ding, Aditya Nannapaneni, Bingfan Liu, Ling Zhang. The extracted PDF text lists no affiliations or emails ✓.
- **Date and venue:** arXiv v1, 29 Jun 2026, filed under **cs.CY** (not cs.AI/CL). It is an arXiv preprint with no venue stated ✓.
- **Length:** 119 pages. The body runs to about p.69, then appendices A–F (pp.69–78), then references (about 600 arXiv-cited entries) ✓.
- **Link:** https://arxiv.org/abs/2608.05179
- **Type:** an audit-style survey with a small systematic review attached. 144 records → 125 after de-duplication → 35 included → 26 coded in full text (App. A) ✓.

**Scope & definitions** (§3, §4 ✓)
- **Autonomous research agent:** "an LLM-driven system that executes at least one stage of the research lifecycle with non-trivial decision autonomy, meaning the system, not a human, selects the next action within that stage." Single-shot prompting is excluded.
- **Two terms:**
  - *LLM autonomous research agent* is the focal category.
  - *Autonomous research system* is the wider coded set. It also includes pre-LLM and domain-adjacent contrast cases: CAMEO, ChemCrow, ChatBattery, MASTER, ARIA.
- **Scope:** deliberately the computational AI/ML research setting, because there "a reviewer can in principle re-run the code." Wet-lab and robotic work, AutoML/NAS and domain foundation models are context only.
- **Unit of analysis (the main contrast with other surveys, Table 1):** the *audit artifact* a reviewer can inspect — code, prompts, seeds/traces, selection policy, novelty-check method, human-intervention points. It is not the domain or the autonomy label.
- **Positioning:** against Zheng'25 (autonomy axis), Wei'25 (domain axis), Tie'26 "A Survey of AI Scientists" 2510.23045 (workflow axis; the closest overlap), Xu & Peng'25 (deep research) and Zhang'24 (scientific domain models). The authors say plainly: "auditability coding, not the taxonomy, is the contribution."
- **Codebook (7 dimensions, §3):** stage; autonomy level ("(c)" marks an author-claimed level); evaluation method; released artifacts; human-in-the-loop (HITL) entry points; novelty-verification method; disclosure of result selection.
- **Autonomy is defined on five sub-axes (§4):** initiative, judgment, execution, iteration and accountability. These induce the levels:
  - L0 assistive
  - L1 tool-augmented
  - L2 stage-local
  - L3 pipeline
  - L4 closed loop, split into:
    - **L4-m (mechanical):** re-runs or re-fits on an internal metric.
    - **L4-v (validated):** revises a hypothesis against an outside oracle, either a physical measurement or an independent checker.
  - L5 open-ended: aspirational and left out of the map.
  - The key design point is that judgment is separated from execution: a system "can automate research tasks (L2–L3) without automating scientific judgment."
- **Two novelty terms (§5):** *novelty-as-judged* (a panel rates the idea new) versus *novelty-as-valid* (the idea turns out new and true after grounding and testing).

**Taxonomy**

The paper has three cross-cutting organizing devices.

**(A) Lifecycle clusters (§5–8), with the representative works it places in each:**

- **§5 Ideation and hypothesis generation**
  - 5.1 *Literature-conditioned generators:* SciMON, ResearchAgent, Chain-of-Ideas, Nova, IdeaSynth, IRIS.
    - Human-study warnings: Si et al. 2024; Si et al. 2025, the ideation–execution gap.
    - Homogenization: Anderson 2024; Ashkinaze 2024.
  - 5.2 *Knowledge-graph-grounded proposal:* KG-CoI, BioDisco (novelty scored against held-out future literature), SAGE, HypoChainer, REx, abductive KG, BioVerge, Kulkarni validation survey.
  - 5.3 *Iterative search, debate and tournaments:* VirSci, SciAgents, AI co-scientist (Elo tournament plus some wet-lab corroboration), MOOSE-Chem.
  - 5.4 *Hypothesis validation and experimental-design soundness:*
    - Confounds: Miyai'25 risk analysis.
    - Power and p-hacking: Gaurav'25; Batzner'25.
    - Interventional replay versus judge-based attribution: Shah'26.
    - ProjectionBench: Lew'26.
  - 5.5 *Benchmark layer:*
    - Idea and hypothesis benchmarks: IdeaBench, HypoBench, ResearchBench, AI Idea Bench 2025, DiscoveryBench.
    - Popper (sequential falsification).
    - ForeSci ("evidence-decision decoupling").
    - ResearchClawBench (about 20% pass).
  - 5.6 *Mathematical discovery beyond proof:* TxGraffiti/Optimist, Wagner RL, PatternBoost, Ramanujan Machine, Bengio & Malkin, Mishra'25 interestingness.
- **§6 Literature and writing**
  - 6.1 *Survey generators:* STORM, Co-STORM, AutoSurvey, SurveyForge, SurveyX, LitLLM, OpenScholar.
  - 6.2 *Deep research:* WebThinker, DeepResearcher, TTD-DR, DeepResearch Bench, ReportBench, BrowseComp.
  - 6.3 *Attribution and grounding oracles:*
    - Factuality checks: FActScore, SAFE, VeriScore.
    - Attribution and citation audits: Liu'23 audit, AttributionBench, RAGTruth, RARR, Self-RAG, LongCite, ALCE, PaperQA2.
    - Attacks and grounding: PoisonedRAG, Gorilla, LAB-Bench.
    - Citation-integrity audits: CiteCheck, Onweller'26.
  - 6.4 *Systematic review and meta-analysis:* ScholaCite, OARelatedWork, GREP, Citegeist, Select-Read-Write, OpenExtract, Padarha'26, SciFact.
  - 6.5 *Figures, tables and multimodal artifacts:*
    - Chart generation and editing: Poličar'26 chart workflow, Li'25f chart editing.
    - Table extraction: Kayal'22, Ajayi'25 conformal table extraction.
    - Captions and chart reading: SciCap and caption judges, CharXiv-type stress tests.
- **§7 Coding, execution and analysis**
  - 7.1 *SWE-agent substrate:* MASAI, CodeR, Agentless, AutoCodeRover, SWE-Search, SpecRover, SWE-Gym, SWE-RL, Lingma, SWE-agent.
  - 7.2 *Paper-to-code:* PaperCoder, ResearchCodeAgent, AutoReproduce.
  - 7.3 *Replication benchmarks:*
    - Environment setup: SUPER, DeployBench.
    - Code fidelity: ResearchCodeBench, SciReplicate.
    - End to end: PaperBench, MLReplicate, CORE-Bench.
    - Adjacent: MLE-bench, RE-Bench, ScienceAgentBench, EXP-Bench, SPOT.
    - Audit only: ReproRepo, ReproScore.
  - 7.4 *Cross-domain replication:* social-science replication (Alizadeh'26), Automated Social Science and inflated effects, silicon sampling, "Sense and Sensitivity", Fortran→C++.
  - 7.5 *Provenance infrastructure:* trackers and containers, PROV-style provenance graphs, agentic provenance (Souza'25a/b), data-use auditing.
  - 7.6 *Tool-call reliability:*
    - Tool selection and privilege: tool retrieval (Shi'25), over-privilege (Yang'26b).
    - Silent failures: tool-bypass hallucination (Healy'26), speculative execution (Sui'26).
    - Error analysis and constraint synthesis: Chen & Cong'25; Chong'26.
- **§8 Review and closed loop**
  - 8.1 *Automated reviewers:* ReviewerGPT, MARG, AgentReview, CycleReviewer, and bias audits (LLM-REVal, REMOR, GenReview).
  - 8.2 *LLM-as-judge:* MT-Bench, Arena, self-preference (Panickssery'24), PoLL juries, calibration.
  - 8.3 *Multi-agent substrate:* CAMEL, AutoGen, MetaGPT, ChatDev, debate, ResearchTown, AgentRxiv, Agent Laboratory, Dolphin.
  - 8.4 *2026 end-to-end scientists that "externalize trust":* AutoResearchClaw, Socratic physics critic, TianJi-Environ (WRF-Chem), Medical AI Scientist, Cmbagent/Denario, PARNESS, InternAgent, EviBound.
  - 8.5 *Consensus failure modes:* reliability without validity, sycophancy, error cascades, MAST, collusion and steganography.
  - 8.6 *Human oversight and calibrated reliance:* Schemmer, the "feedback paradox" (Wang & Wang'26), Gaube'26.
  - 8.7 *Unaudited channels of the closed loop.*

**(B) Verification-signal ladder (§9, Table 8, Fig. 3; full placement in Tables 16–17)**

The ladder is ordered by soundness and resistance to gaming:

| Tier | Signal | Representative works and sub-areas |
|---|---|---|
| I | Sound formal verifier | DeepSeek-Prover, Kimina, Goedel, Seed-Prover, LeanDojo, self-play conjecturing, AlphaGeometry-style |
| II | Executable tests and process rewards | SWE agents; symbolic regression; FunSearch, AlphaEvolve, AlphaTensor; reproduction benchmarks; PRMs (step-level verification) |
| III | Physical oracle or simulator | self-driving labs, astronomy, Earth (TerraBench), chip/CAD/EDA, CAMEO |
| IV | Citation and source grounding | deep research, KG hypotheses, survey generation |
| V | Proxy reward and threats to validity | learned verifiers, agentic RL, contamination, cost, mechanical L4 loops, 2026 AI scientists |
| VI | Human-expert judgment | — |
| VII | Weak inter-agent or infrastructural signal | multi-agent frameworks, logs |
| VIII | The model's own judgment | LLM-as-judge |

- The paper calls the result an **inversion**: agentic-research activity is concentrated in Tiers IV–VIII, the opposite of where the checks are strong.
- It also argues that the ladder grades *claims, not systems* ("One system, several tiers," §9.5).

**(C) Frontiers (§13):**
- 13.1 Memory and self-improvement — ExpeL, CoPS, Mem0, A-MEM, AWM, ArcMemo, Gödel Agent, Darwin Gödel Machine
- 13.2 Agentic RL — APPO, PACT, SENTINEL, Polar, OpenWebRL
- 13.3 Calibration and abstention
- 13.4 Negative results
- 13.5 Autonomy and governance
- 13.6 Human–AI collaboration
- 13.7 Red-teaming and dual use
- 13.8 Memory provenance and drift

**Key claims & findings** (all ✓ in full text)
- **Disclosure rates (24 runnable systems; Table 4, §3):**
  - Code released: 83% (20/24)
  - Prompts released: 71%
  - **Seeds or execution traces released: 38% (9/24)**
  - Result-selection policy disclosed: 67%
  - At least one HITL entry point: 88%
  - **Any novelty-verification method: 38%**
  - The pattern holds in the LLM-era slice (n=22: code 86%, seeds/traces 41%) and the AI/ML-only slice (n=19: 89% / 42%) (Table 5).
- **The L4 split (Tables 7 and 15):** of 9 L4 systems, 7 are mechanical (AI Scientist, AI Scientist-v2, SR-Scientist, MASTER, LLM-AutoSciLab, "E2E AI Research" [Yamada et al. 2026, 2606.15497], LLM-ACES). One is author-claimed (Claw AI Lab). One is externally validated: CAMEO (2020, pre-LLM). Hence "no LLM-era system in the corpus demonstrates an externally validated in-loop oracle." ChatBattery's wet lab is a *terminal* check, so it is coded L3.
- **No validated novelty check (§11):** "we found no system that reports independent validation that its novelty check is itself reliable."
- **What the corpus evaluates with (Table 14, N=26):**
  - benchmark or task success: 62%
  - LLM-as-reviewer: 62%
  - human-expert read: 65%
  - workshop acceptance: 15%
  - real or wet-lab deployment: **8% (2/26)**
- **Lifecycle map (Table 6, §4):** coverage is densest at execution and analysis and thinnest at literature review and closed-loop work. Audit strength forms a U shape across the lifecycle: weak at ideation (only novelty judgments), strongest at coding and execution, weak again at review and closed loop.
- **Reproduction is harder to verify than issue resolution (§7.1).** SWE-bench's oracle is decidable, local, written in advance and adversarial to shortcuts. Reproduction's oracle is "a number in a paper's table," which is not decidable alone, not local and not adversarial. "Reproduction often leaves the claim itself acting as the oracle."
- **Three failure modes the replication tier only partly catches (§7.3):**
  - self-stopping at a weaker target, caught by pre-committed task specifications;
  - result fabrication, caught by seeded traces;
  - grader capture, caught by hidden human-authored pipelines on post-cutoff papers.
- **Per-idea novelty scores can't see diversity collapse (§5.1).** A population of individually novel ideas can still collapse onto a few templates. Detecting this needs population-level entropy or coverage, "the novelty filter penalizes overlap with the past, not overlap with the agent's own siblings."
- **Self-preference cannot be fixed inside one model family (§8.2).** A judge prefers lower-perplexity text, and lower perplexity is what generators are trained to produce.
  - Juries buy only "diversity-bounded" independence.
  - Calibration "recovers no independence at all, only honesty about its absence."
  - Only an external or process signal breaks the identity between what is rewarded and what is maximized.
- **The closed loop has three unaudited channels (§8.7):**
  1. the verdict's own validity;
  2. **generator–reviewer co-adaptation** — "the reviewer's biases become the generator's objective," including "the model's own lexical fingerprint";
  3. review-channel injection.
  - Failure attribution in multi-agent traces is about **14% accurate** (MAST / Who&When).
- **2026 end-to-end systems are shifting toward checking apparatus (§8.4),** but debate and Socratic critics are not external. EviBound: a prompt-only agent claimed success on 8/8 tasks with 0 actually verified.
- **Oversight laundering (§8.6).** Inserting a human is not verification unless the human sees the right layer, has calibrated trust, and is held to a recorded standard. Example: output-only feedback produced no full-task success in Wang & Wang'26.
- **Surrogates and learned verifiers move the verification burden rather than discharging it (§9.5).** A learned signal "degrades silently off-distribution, whereas an executable oracle either runs or visibly errors." The paper also cites a "reliability cliff" once rewards stop being mechanically verifiable (Narasimhan'26).
- **Budget matching (§10.5–10.6).** Skill and memory modules for web agents rarely beat a token-matched vanilla actor (Hajimiri'26). The agent-benchmark papers that Moghadasi & Ghaderi'26 audited essentially never disclose inference cost. One framework spent about 9× the energy of another for near-zero resolution (Tripathy'25).
- **Memory "audit debt" (§13.1).** Selection bounds, lineage, readable state and benchmark gates are each checked once, at write or commit time, and never re-checked under drift. "An agent that edits its own selection logic can in principle edit away the very gates that made its earlier commits checkable."
- **Reward verification has two separable parts (§13.2):** *faithfulness to execution* (Polar) and *groundedness of evidence* (OpenWebRL). Released agents rarely ship a re-runnable reward specification.
- **Autonomy should track the weaker of two mechanisms (§13.5).** Visibility infrastructure fails through blind spots; scalable oversight fails through a corrupted signal. "Autonomy should be granted only as far as the weaker mechanism still binds."

**Evaluation practices it describes or criticizes**
- LLM-as-reviewer used as the acceptance gate (AI Scientist v1/v2, CycleReviewer). The criticism: biased toward LLM-written text, rewards overclaiming, and is open to injection (§8.1, §8.7).
- Novelty judged by LLMs or panels, and per-idea novelty that cannot see population diversity (§5.1–5.3).
- Rediscovery benchmarks controlled for contamination, which are "fundamentally retrospective." They cannot certify a forward-looking hypothesis that no oracle can yet decide (§5.5).
- "Compiles" or "runs" counted as success for reproduction. Rubric graders that a fabricated number satisfies by construction. Loose output-match tolerances (§7.2–7.3).
- Leaderboards that "certify output but not method": validation overfitting and label leakage score the same as sound work (§10.1).
- Contamination: membership-inference probes are noisy; GSM1k shows drops of up to 8%; search-time contamination (§10.3).
- Undisclosed compute, energy, harness and prompt choices. Example: FormatSpread swings of up to 76 points from formatting alone (§10.6, §10.8).
- A single seed, no variance, no explicit null, no correction for multiple comparisons. Agentic p-hacking (§5.4).
- Accept/reject HITL decisions recorded with no calibration metric (§8.6).
- Self-criticism of the paper's own method (§15): the second coder worked **from abstracts only**. Agreement was 90% on artifacts, 50% on autonomy, 60% on novelty and 60% on selection (65% overall). The interpretive rates are therefore "directional."

**Reviewer checklist (Table 10, §14).**

| Status | Item | Share of corpus meeting it |
|---|---|---|
| Measured | HITL points | 88% |
| Measured | Code | 83% |
| Measured | Seeds or traces | 38% |
| Measured | Novelty method plus a sampled check | 38% |
| Measured | Number of attempts plus selection rule | 67% |
| Proposed (not coded) | Baseline provenance and tuning budget | — |
| Proposed (not coded) | Reviewer independence (evaluator ≠ generator) | — |
| Proposed (not coded) | Timestamped preregistration | — |

The paper also defines a "trace": "a seed plus the prompt, tool-call, model, and output record needed to reconstruct the run."

**Open problems / future directions it lists**

*Formal list (§16, complete):*
1. **Proxy-to-truth validation for closed loops:** when does an internal metric track scientific validity, and how do you detect when it doesn't?
2. **Closed-loop verifiers:** evaluators that test whether results actually *revise hypotheses*, not merely whether a pipeline re-ran.
3. **Novelty auditing at literature scale,** without relying on LLM self-judgment.
4. **Independent agent review,** robust to prompt injection and self-preference.
5. **Contamination-resistant evaluation,** including search-time leakage.
6. **Preregistration for AI-generated hypotheses:** a shared record against result selection and metric-driven problem selection (following Bisht et al. 2026, 2605.08956).

*Further directions stated in the text but not on the formal list:*
- certifying undecidable, forward-looking hypotheses (§5.5);
- population-level diversity metrics such as entropy or coverage (§5.1);
- interventional rather than correlational auditing of experimental design (§5.4);
- faithfulness checks on figures and captions against the underlying data (§6.5);
- end-to-end grounding chains of claim → citation → passage → source reliability (§6.2);
- forcing agents to run inside provenance infrastructure (§7.5);
- protocols for when a surrogate output must be escalated to ground truth (§9.5);
- joining wet-lab outcomes to computational loops (§9.6);
- executable disclosure contracts (§10.7);
- re-validating memory entries under drift, and non-forgeable provenance (§13.1, §13.8);
- re-runnable reward specifications (§13.2);
- calibrated abstention that still works at high uncertainty (§13.3);
- red-teaming dual use under closed-loop autonomy (§13.7).

**What it misses or gets wrong as of Nov 2026** (compared against the project files)
- **The self-improving-agent and recursive-self-improvement line is almost absent.** DGM and Gödel Agent appear only in §13.1 memory. There is nothing on Hyperagents, ADAS, SICA, STOP, ShinkaEvolve/OpenEvolve, or the RSI-Harness / MetaEvoForge / FreeEvolve transfer wave. GEPA and ACE appear only as context engineering (§10.8).
  - **Improver-level** verification is missing entirely. Does the closed loop make the improver better, or only the product? Improvement by Inheritance (ZMmJDeDTXk), SelfImprove-Bench 2611.02797 (0% improver edits) and Learning to Repair the Learner (aEPJT4hW84) are all absent. Some postdate it; the project's FINAL_REPORT and round3/REPORT cover them.
  - This is the survey's largest blind spot relative to Dan's agenda. Its L4-m/L4-v rule codes whether the *loop* revises hypotheses, never whether the *loop's own procedure* gets better.
- **Harness evolution and held-out overstatement.** The survey notes that a "best validation score is optimistic" only for leaderboards. It misses the 2026 harness-evolution evidence:
  - HarnessOpt-Bench 2608.06301: best validation usually optimistic versus test.
  - ARC-AGI-3 self-harness 2610.30499: development gains overstate held-out results by 5.62.
  - Mirage Discoveries (da1Czy7foS): equal-budget random search transfers as well.
  - Reachability (oTYVUBHmbe): 76% of edits are hyperparameter-level.
  - Several of these postdate June 2026, but they directly test its "proxy-to-truth" open problem #1.
- **Simulators are treated as Tier III oracles, with no account of how a simulator's errors enter an optimization loop** (selection versus teaching through traces). It flags surrogate confidence (§9.5), but not simulator-scored *training or selection* of agents (Simulator Says Yes 0cb6zfwHm7, VHE 5lWjRTK6Vk, Improvement Fidelity Ah9eaA746Q; project FINAL_REPORT). TianJi-Environ's WRF-Chem is presented as an "independent oracle" with no error model.
- **No openness or permission axis.** Autonomy is coded by who decides, never by which surface is editable. It cites no evidence that realized openness is far below nominal openness (SelfImprove-Bench; Reachability) and no openness × capability crossover (OEO 2608.09629; project round3/REPORT).
- **Corpus is thin and back-weighted:** 26 coded entries, with odd inclusions (ReviewAdvisor, SWE-agent, CLADD, "Ultimate Brain"). Some major 2025–26 AI-scientist systems are missing, for example AI-Researcher-like and DeepScientist-type systems or the Agents4Science outputs (from memory†, not checked against the corpus). Second-coder reliability came from abstracts only.
  - Headline rates are small-n: 9/24 means ±1 system moves the rate by about 4 pp.
  - One coding call looks shaky: CAMEO's "Artifacts: none" sits inside the "verification built in" exemplar. The only validated loop released no artifacts, which cuts against the paper's own artifact thesis. The paper does not discuss this.
- **Internal inconsistencies:**
  - Tier VI is "human-expert judgment" in Table 8 but "motivation / human judgment" in the Table 17 footnote.
  - "Reproducibility crisis (context)" is placed at Tier VI.
  - Table 14's human-expert share is 65% against 62% for LLM review, yet the text says LLM review is "as common as benchmarking," which is true only for the 62% figure.
  - Tie et al. is dated 2026 with a 2510 arXiv id.
- **Register, style and homogenization drift** come up only as homogenization of ideas (§5.1, §13.6) and the "lexical fingerprint" in co-adaptation (§8.7). There is no measurement of style drift through iterated generation and judging (cf. 2610.29162, project claudish/REVIEW.md).
- **Continual and drift** coverage is good on memory staleness (§13.8 bi-temporal ledger, Yadav'26). It misses drift in the *improvement regime* (API/UI or verifier drift) as a cause of whether self-editing pays (RSIAgent 2609.15364, Darwin-Claw 2610.23574, round3 openness law).

**Relevance to Dan's ideas**
- **Taught, Not Selected (high; framing ammunition).**
  - §9.5 says surrogates/simulators "shift the verification burden rather than discharging it." §13.2 splits reward verification into *faithfulness to execution* and *groundedness of evidence*, and says neither subsumes the other. That is close to TNS's trace-channel versus fitness-channel 2×2, and can be cited as a field-level statement that the parts are separable but unmeasured.
  - §10.8 says GEPA- and ACE-style scaffold evolution "shift the verification burden onto whether the auto-generated context and trace are faithful." This is the trace channel, named but not tested.
  - §7.5 adds that an agent "may log a sanitized rationale while acting on a different internal state." That is a legible-versus-silent error angle.
  - The survey never separates *how* a flawed verifier harms an optimizer (selecting wrong versus teaching wrong). TNS fills exactly that gap.
  - Use Table 10 (preregistration, evaluator ≠ generator, seeds/traces) as the reporting standard. It fits TNS's two pre-registered signs.
- **Propose / Propagate / Pay-off funnel (high).**
  - Open problem #2 ("evaluators that test whether results actually revise hypotheses, not merely whether a pipeline re-ran") and the L4-m versus L4-v rule are the research-agent version of *propagated versus paid off*.
  - §13.1 notes that DGM's benchmark gate "is evaluated once, at commit, rather than re-run against later tasks." That is the Propagate-stage weakness; nested replay at generations 0/10/20 is the missing re-check.
  - The line "edit away the very gates" supports keeping the meta-gate outside the editable surface.
- **Meta-Overfitting (medium-high).** §13.1's "durability" problem says a lesson validated in one regime "can quietly become a contaminating prior under another." That is the memory-versus-procedure decay claim in survey language. §10.5's budget-matched warning (Hajimiri'26: skill/memory modules ≤ token-matched vanilla) supports matched-compute arms. The survey offers no domain-distance analysis, so there is no conflict and it can serve as a citation for the gap.
- **Meta-reward (medium-high).**
  - §13.2: "the part a reviewer still cannot audit is often the reward function itself."
  - §8.2: self-preference collapses what the judge rewards onto what the generator maximizes; only external or process signals restore independence.
  - §8.7: generator–reviewer co-adaptation.
  - Together these motivate placing the meta-reward computation outside every editable surface and using a held-out nested-replay Q^(1) rather than an in-family judge.
- **Openness (medium).** There is no openness axis, and that is itself a gap to cite. §10.2's "who builds the verifier is a design axis" maps onto L5 (an editable evaluator) on the openness ladder. §13.5's "autonomy only as far as the weaker mechanism binds" offers a governance framing for the reach × credit law.
- **Continual / drift (medium).** §13.1 (gates never re-checked under drift), §13.8 (stale facts retrieved "with full confidence"; personalization drift with "no record of when or why it changed") and §13.2 (OpenWebRL freshness) all support the claim that real drift of the improvement regime is the regime where re-validation matters. The survey does not treat drift as the condition under which self-editing pays off.
- **Register snowball (low-medium).** It gives a mechanism citation and a related-work hook, not a competitor:
  - §8.7 says a generator optimized against an in-family reviewer learns to emit "confident phrasing, a familiar section structure, or the model's own lexical fingerprint."
  - §8.2 grounds self-preference in perplexity (Panickssery'24; Wataoka'24).
  - §5.1 covers population-level homogenization (Anderson'24; Ashkinaze'24).
  - §10.8 FormatSpread shows how sensitive outcomes are to surface form.
  - It measures no loop gain.
- **Practical takeaway.** Cite it as the 2026 "verification gap" survey in related work for all the improver-line papers. Its open problems #1 and #2 and the L4-m/L4-v rule are the cleanest outside framing for "the loop re-ran but the improver didn't improve."

---

### Open-Endedness is Essential for Artificial Superhuman Intelligence (2406.04268)

**Read status: ✓ full text read.** I read the arXiv v1 PDF through pdftotext: all main sections 1–5, the Impact Statement, and Appendices A.1, A.2, B and C. I skimmed the reference list. The text is at `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/2406.04268/paper.txt`. None of the project knowledge files cite this paper by arXiv ID or title.

**Bibliographic**
- Authors: Edward Hughes* and Michael Dennis* (equal contribution), Jack Parker-Holder, Feryal Behbahani, Aditi Mavalankar, Yuge Shi, Tom Schaul, Tim Rocktäschel. All at Google DeepMind, London.
- Date: arXiv v1, 6 Jun 2024.
- Venue: ICML 2024, PMLR 235. It is a position paper.
- Length: about 9 pages of main text and about 20 pages in total with references and appendices A–C.
- Link: https://arxiv.org/abs/2406.04268
- Type: a position paper plus a definition. It is not a systematic survey. It contains no experiments and no new numbers. The only empirical figure it quotes is AdA's novelty plateau after about 1 month of training (§2.3, citing Bauer et al. 2023).

**Scope & definitions**
- **Thesis (abstract and §1):**
  - The ingredients now exist to build AI that is open-ended with respect to a human observer.
  - Open-endedness is a *necessary* property of any ASI.
  - Foundation models (FMs) and open-endedness are "orthogonal dimensions" whose combination is especially powerful: FMs act as mutation operators and as judges of interestingness, and open-ended algorithms supply new knowledge.
- **Formal definition (§2.1).** A system S produces artifacts X_t. An observer O keeps a statistical model X̂_t learned from X_{1:t} and scores it with a loss ℓ(t,T) that is fixed in advance.
  - *Novelty:* ∀t, ∀T>t, ∃T′>T with E[ℓ(t,T′)] > E[ℓ(t,T)]. In words, a less predictable artifact always comes later.
  - *Learnability:* ∀T, ∀t<t′<T, E[ℓ(t′,T)] < E[ℓ(t,T)]. In words, a longer history makes any fixed future artifact more predictable.
  - *Open-ended* means novel **and** learnable, from the observer's point of view. Quote: "From the perspective of an observer, a system is open-ended if and only if the sequence of artifacts it produces is both novel and learnable."
- **Noisy-TV sanity checks (§2.1):**
  - Static noise is learnable but not novel, because the expectation removes aleatoric noise.
  - A noisy TV whose channel is switched at random is novel but not learnable.
- **Interestingness** is not a separate term. It is carried by the observer's choice of loss ℓ: "the interesting parts of artifacts are precisely those features which the observer decides are useful to learn about."
- **Constraint (footnote 2):** ℓ must be fixed without knowledge of S. Otherwise the observer could discard S's artifacts and invent its own.
- **Observer types (§2.3):**
  - Humans are the "pre-eminent" observer class, especially for safety.
  - The definition still allows any observer, including non-anthropocentric ones, ASI, and "any observer" quantification.
  - Time horizon τ: a system is *finitely* open-ended (for t,T<τ) or *infinitely* open-ended (τ→∞). AdA is finitely open-ended with τ ≈ 1 month.
  - Observer memory and capacity limits matter. In the Wikipedia-curriculum example, forgetting breaks learnability, and a narrow domain such as elliptic-curve cryptography exhausts novelty.
- **Self-observation and the proxy observer (§3.1, App. A.2).** A system can observe itself. When self-observation feeds back into improving the system, the observer is a "proxy observer". Examples are self-play and a person's chain of "Eureka moments".
- **Alternative compression definition (App. B):**
  - Novelty means growing code length |C_{h_t}(X_T)|.
  - Learnability means a longer history gives shorter codes.
  - A lossy version uses an ε threshold. A rate–distortion version defines "broad open-endedness": curves get fatter across columns T and flatter down rows t (Fig. 3).
  - The authors *conjecture*, but do not prove, that this is equivalent to the statistical definition.

**Taxonomy (in my words)**
1. **Where existing systems sit, with respect to a human observer (§2.4)**
   - Open-ended but narrow:
     - Self-play RL: AlphaGo/AlphaZero, chess and shogi, AlphaStar, Stratego (DeepNash), OpenAI Five, Cicero/Diplomacy. Learnability is evidenced by humans improving after studying AlphaGo (Shin et al. 2023). The paper notes that these artifacts are learnable partly because humans invented the game.
     - Unsupervised environment design and autocurricula: AdA/XLand2, with 25B task variants prioritised by learning potential (PLR). Also PAIRED, Justesen et al., Leibo et al., and Baker et al. (hide-and-seek). The framing is the "zone of proximal development". Novelty plateaus after about 1 month.
     - Evolutionary and quality-diversity methods: POET and Enhanced POET, MAP-Elites, QD (Pugh, Mouret & Clune), minimal-criterion coevolution. POET plateaus once its terrain parameterisation is exhausted.
   - General but not open-ended: contemporary FMs trained on fixed datasets. Any observer able to model the data will eventually stop seeing novelty. Planning benchmarks (Valmeekam, Momennejad, Pallagani) expose this in narrow domains.
   - The target is general *and* open-ended: "Open-Ended Foundation Models" (§3), which sit at the intersection of open-endedness and AI-generating algorithms (AIGA) (App. C).
2. **Four overlapping paths to open-ended FMs (§3), described as "neither prescriptive nor exhaustive"**
   - §3.1 Reinforcement learning:
     - Autotelic goal-setting (Colas et al.).
     - Voyager: LLM curriculum, iterative prompting and a skill library, with no weight updates.
     - Exploration via pseudo-rewards (count-based, RND, Du et al.), modulation (Schaul) and auto-curricula (PLR, ACCEL, MAESTRO).
     - The "proxy observer" abstraction: FMs guide exploration toward what humans would find interesting. Examples are Motif (Klissarov et al.), OMNI and OMNI-EPIC (Faldor et al.).
     - Multi-agent non-stationarity and debate (Du et al., Tang et al.).
   - §3.2 Self-improvement:
     - Constitutional AI, critique-assisted human evaluators (Saunders), CRITIC (Gou), Self-Instruct, Self-Debug, Self-Rewarding LMs, and VLMs as reward models (Baumli).
     - The requirement it states is a "scalable mechanism to evaluate its own performance."
   - §3.3 Task generation (the "problem problem"):
     - Setter–solver (Schmidhuber 1991), PAIRED, POET.
     - The Internet as an environment (Jiang et al. 2022, WebArena).
     - World models as simulators (Genie, Sora, UniSim, GAIA-1) combined with multimodal reward models to produce open-ended curricula, and sim-to-real transfer.
   - §3.4 Evolutionary algorithms:
     - LLMs as mutation and selection operators: ELM, LMX, EvoPrompting.
     - Prompt evolution: Promptbreeder, OPRO, EvoPrompt.
     - Quality-diversity over text: QDAIF, Rainbow Teaming.
     - Code and genetic programming: Eureka (reward code) and FunSearch (mathematics).
3. **Safety taxonomy, organised by knowledge flows in the joint human–AI open-ended system (§4, Fig. 2; modelled on Critch & Krueger)**
   - 4.1 AI creation and agency: dual-use, goal misgeneralisation, specification gaming. Proposed mitigations are safe exploration and impact regularisation.
   - 4.2 Humans understanding AI creations: interpretability, automated interpretability (Bills et al.), ELK-style systems that teach, informed oversight.
   - 4.3 Humans guiding AI creation: human-in-the-loop open-endedness (PicBreeder; OMNI as "open-endedness from human feedback"), controllability objectives (CIRL, off-switch, Carey & Everitt), and avoiding "rabbit holes".
   - 4.4 Human society adapting: cooperative AI, flash-crash tipping points, adaptive governance, the Collingridge dilemma.
   - 4.5 Emergent risks: open-ended systems that interact can cancel each other's open-endedness, which argues for "anti-fragile" safety.
4. **Related definitions (§2.2, App. C)**
   - Soros & Stanley's four necessary conditions. Learnability generalises the minimal criterion (condition 1); observer non-intervention parallels condition 3; novelty parallels conditions 2 and 4.
   - Sigaud et al. 2023 also define open-endedness relative to an observer. This paper says it is more precise about what "new" means.
   - Earlier ALife metrics: persistence filtering, Bedau's evolutionary activity statistics, Taylor's conditions.
   - Shackle's "potential surprise" and Knightian uncertainty.
   - Novelty search and stepping stones (Stanley & Lehman).
   - Curiosity as prediction error. Their novelty term generalises it, and the expectation removes stochastic traps.
   - Jiang et al. 2022, "General intelligence requires rethinking exploration". There, learnability belongs to a single MDP; here it belongs to a sequence of artifacts.
   - AIGA (Clune). Neither implies the other.
   - Continual RL (Abel et al.). It is distinct because a continual agent can cycle among strategies.

**Key claims & findings**
- Quantifying open-endedness is possible and important. This is set against Stepney & Hickinbotham (2023) (§1).
- Open-endedness is observer-relative. A mouse finds an aircraft-design system novel but not learnable; an aerospace student finds it open-ended; a superintelligent alien finds nothing novel (Fig. 1). An oracle that knows the Nash strategy never finds AlphaZero novel. An average Go player loses learnability earlier than an expert does (App. A.2).
- Open-ended systems stay open-ended only while they can "educate" their observers. "superhuman intelligence will be interesting to humans only as far as humans can learn to understand it" (App. A.2).
- Novelty and learnability guarantee nothing about coverage. Simple adversarial policies still beat AlphaZero reimplementations (Wang et al. 2023b) (App. A.2).
- Present-day FMs are not open-ended for any observer that can model their training data (§2.4).
- Scaling on passive data "will soon plateau" and will not reach ASI (§3). The authors expect data scarcity (Villalobos et al.).
- Open-endedness is "fundamentally an experiential process". The required dataset is unlikely to be collectable offline, so the model should compile data online, in the manner of the scientific method (§3).
- The paper reframes model collapse: retraining on self-generated data is a route to ASI, not only a nuisance (§2.4). A context-conditioned FM can recombine concepts in an open-ended way if given an external measure of validity.
- "there may be only a few remaining steps required to achieve open-endedness with foundation models" (§3).
- Safety and open-endedness must be pursued together. The §4 problems are "minimum specifications" for any open-ended system worth building. Human observers must stay pre-eminent, because an ASI with fewer memory constraints may judge itself open-ended beyond the point where humans can (§2.3, §4).
- Directing an open-ended system toward an objective while keeping it open-ended is an open problem. In broad domains, systems may "rabbit-hole into the obscure theorems, useless proteins" (§4.3).

**Evaluation practices it describes or criticizes**
- Proposed ways to measure open-endedness (App. A.2):
  - Human experiments on how well people predict the system.
  - Direct human ratings of novelty and learnability, in the style of RLHF or PicBreeder.
  - LLMs as judges of novelty and learnability, as in OMNI.
  - An explicit online learner over artifacts, e.g. Follow-the-Regularized-Leader, with the trend of its loss tracked.
  - In practice, subtracting an estimate of aleatoric uncertainty from the loss (Mavor-Parker et al.) (App. C).
- Plateau diagnosis by observer judgement: AdA's held-out zero-shot and few-shot capability growth flattens after about 1 month, and scaling the agent or the number of tasks extends it (§2.3–2.4).
- Practices it criticises:
  - Colloquial or imprecise definitions (Stanley et al.).
  - Evolution-specific ALife metrics that don't carry over to ML (App. C).
  - Novelty-only curiosity, which falls into stochastic traps (App. C).
  - Sigaud et al.'s undefined "new".
  - Diversity as a distance between artifacts, as in Jiang et al. 2022. Their novelty is a property of the observer's learning, not of distances between artifacts.
  - Evaluating FMs on broad domains, where human memory limits make them look open-ended. Narrow planning tasks expose the limits (§2.4).
- The paper itself runs no benchmark. Its strict learnability condition (∀t′) is admitted to be impractical, and a probabilistic δ-version is offered instead (App. A.2).

**Open problems / future directions it lists (complete)**
1. Use the definition to prove whether given systems are open-ended, and to *search* for open-ended systems (§2.1).
2. Whether all "reasonable" observers agree. The paper conjectures agreement if observers approximate Solomonoff induction (App. A.2).
3. Study the probabilistic variants of learnability and novelty, including constant δ versus δ that depends on (t, t′, T), and other related variants, "like notions of convergence" (App. A.2).
4. Prove the conjectured equivalence of the compression definition, and decide which variants of "broad" open-endedness (uniform or average rate–distortion) have merit (App. B).
5. Algorithmic implementation subtleties, such as subtracting aleatoric uncertainty from the estimated expected loss (App. C).
6. When the Jiang et al. 2022 system is open-ended by this definition, and whether one can *directly optimise for open-endedness* (App. C).
7. Human observers' memory limits: keeping knowledge compressed enough to stay within collective memory (§2.3).
8. RL: shape exploration toward novel and learnable behaviour in high-dimensional domains, and use FM-based proxy observers (§3.1).
9. Multi-agent debate needs "much more research" before it gives superhuman capability (§3.1).
10. Self-improvement: build a scalable mechanism for self-evaluation and for identifying where to improve (§3.2).
11. Task generation: combine learned world models with multimodal reward models to build open-ended curricula at scale, then transfer across the sim-to-real gap (§3.3).
12. Evolution: fine-tune models on QD-evolved outputs or use them for planning to close the self-improvement loop (§3.4). Also "scale code evolution to a more general setting" (§3.4).
13. Safety, §4.1: safe exploration and impact regularisation for open-ended agents, especially those deployed and still learning in the real world.
14. Safety, §4.2: interpretability that keeps pace with artifact complexity (automated interpretability). Alternatively, design systems whose goal is to teach human observers and to maintain informed oversight.
15. Safety, §4.3: make open-ended systems directable without losing open-endedness. A full solution must also surface unexpected, important artifacts to users and avoid rabbit-holing.
16. Safety, §4.4: prepare society, avoid tipping points from feedback loops, and design adaptive governance that balances gathering information against entrenchment.
17. Safety, §4.5: understand emergent multi-system effects, including systems that kill each other's open-endedness. Build "anti-fragile" safety with monitoring and fast coordinated response.

**What it misses or gets wrong as of Nov 2026** (compared with the project files; the numbers here come from those files and were not re-verified in this session)
- **"A few remaining steps" was too optimistic.**
  - 2026 self-improvement loops plateau and saturate. IBI_GAPS reports that "every seed reached a perfect development score" between generations 2 and 37.
  - Development gains overstate held-out gains: ARC-AGI-3 harness study 2610.30499 reports +5.62 overstatement; Winner's Curse 2610.09239 reports a 13–20 point gap at 16 selection items.
  - Improvers almost never edit themselves: SelfImprove-Bench 2611.02797 finds 0 improver edits; Improvement by Inheritance says "almost never".
  - The paper has no account of selection noise or of the gap between validation and held-out performance.
- **The claim that LLMs are "general mutation operators" (§1, §3.4) is contradicted.**
  - Mutation Without Variation (2606.05408): in 87% of chains, more than 93% of mutations revisit an already-seen structural form.
  - oTYVUBHmbe: 76% of edits are hyperparameter-level, and novel programs appeared in every seed with no gain.
  - Under this paper's definition those programs are novel without being valuable. That is exactly the gap §4.3's "rabbit hole" warning names but the definition cannot express.
- **No measurement protocol.** The ∀/∃ quantifiers over infinite horizons cannot be checked from finite runs, and the paper never runs its own observer-loss metric. The 2026 field operationalised "learnability" differently:
  - c(1−c) or pass-rate bands: TURTLE 2610.27307, R²Orch's "Recursive Learnability" 2610.22740.
  - Project proposal curriculum-goodhart.v2 argues that these bands act as a blind-spot censor. That is a failure mode of learnability-driven curricula the paper does not anticipate when it endorses UED and the "zone of proximal development".
- **Evaluator corruption inside the loop is missing.** The paper treats the proxy observer and self-rewarding as hopeful (§3.1–3.2). By 2026:
  - Self-co-evolved graders collapse to always-pass (Who Verifies the Verifier 2610.11464).
  - Spontaneous reward hacking reaches 30.5% on open-ended pipelines versus 2.9% on task-specific kernels (2609.28614).
  - §4.1 mentions specification gaming only generically, and footnote 2's "fixed ℓ" requirement is exactly what self-observing loops break.
- **Observer = simulator is not addressed.** It endorses world models as task generators (§3.3) but does not consider how a learned simulator's own errors shape what the agent learns. Taught-Not-Selected and the MIMESIS / Simulator Says Yes cluster study this.
- **Continual and drift settings get one paragraph.** App. C separates continual RL from open-endedness, but the paper gives no treatment of non-stationary evaluation. The project's round-3 drift thread and IBI_GAPS gap #15 ("no continual or reality-grounded setting is tested") remain open in 2026.
- **Precision issues.** The learnability formula's quantifier is written "∀T > t′ > t", which reads oddly but is meant as t<t′<T. The paper admits strict monotone learnability is too strong. The compression equivalence is only conjectured. It cites no successor work (expected, given the date). OMNI-EPIC, ADAS, the AI Scientist and DGM-style systems (2025) are the most direct instantiations of its §3 agenda.

**Relevance to Dan's ideas**
- **Taught-Not-Selected (simulator-scored harness evolution; trace channel vs fitness channel).**
  - The paper's observer machinery separates two roles a simulator plays. It is a *proxy observer* that scores (selection), and it is a source of artifacts the reflector learns from (teaching).
  - Its insistence that learnability is a property of a *sequence* consumed by a learner, not a per-item score (App. C, against Jiang et al.), supports the claim that harm flows through what the reflector *reads*.
  - §3.3 endorses world-model curricula without any fidelity analysis. Cite it as the canonical optimistic framing that TNS tests.
  - The "silent vs legible error" contrast maps onto the noisy-TV distinction: legible errors are learnable structure, and silent errors behave like aleatoric noise to the reflector.
- **Propose / Propagate / Pay-off funnel.**
  - This gives a principled "pay-off" criterion: an accepted self-edit lineage should be novel and learnable to a fixed external observer.
  - The App. A.2 self-observing / proxy-observer case is exactly an improver judging its own edits, and footnote 2 (ℓ fixed in advance) justifies keeping the pay-off judge outside the editable surface.
  - The paper never asks *where* novelty dies (not proposed, not selected, or kept but useless). That is the funnel's gap.
- **Meta-Overfitting.**
  - "Finitely open-ended with horizon τ" and "open-endedness is a function of the breadth of the domain" (§2.3), together with the AdA and POET plateaus caused by task-space and parameterisation limits (§2.4), are a ready vocabulary for improvers that look open-ended only near their home domain.
  - Novelty without coverage (App. A.2) parallels "better at improving only where they grew up."
- **Meta-reward.**
  - App. C explicitly lists "whether one can directly optimize for open-endedness" as open. The novelty×learnability observer loss is a candidate order-1 meta-reward (successor learnability) to contrast with order-0 task score.
  - The 2026 c(1−c) bands are the field's cheap proxy for it, and the band-censor critique applies.
- **Openness (the editable-surface ladder).** This is a different concept: the paper's open-endedness is a property of the output stream, not of edit permissions. Still, it supplies an observer-based definition of *realised* openness: novelty and learnability of the edit stream to a fixed model. That fits round 3's finding that nominal openness is far larger than realised openness, and it measures it more principled than edit entropy or N_eff alone.
- **Continual / drift.**
  - App. C notes that continual RL can cycle without novelty.
  - §2.4 reframes self-generated distribution shift (model collapse) as fuel.
  - Both are useful foils. The paper gives no protocol for drift, so the project's "real drift moves the cause up the ladder" claim is an empirical refinement it does not cover.
- **Register snowball.**
  - Under this definition a snowball is a *loss* of novelty: conversation converges to an attractor, as in Ko & Geiping 2606.30571, so an observer's predictive loss on later turns falls toward zero.
  - §4.4 (tipping points from feedback loops) and §4.5 (interacting systems cancelling each other's open-endedness) are the paper's closest hooks.
  - Observer-model loss over turns is a cheap, theory-backed snowball metric to add next to loop gain.

---

### A Systematic Survey of Automatic Prompt Optimization Techniques (2502.16923)

**Read status: ✓ full text.** I read the arXiv v2 PDF (2 Apr 2025, 31 pp.) end to end: the main body (§1–11), Appendix §12 (notation, excluded works, the UCB algorithm), §13 (Tables 2–4 method comparison and Tables 5–7 task lists) and §14 (prompt examples, Tables 8–12). I skimmed the reference list only. Local text: /tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/2502.16923/paper.txt

**Bibliographic**
- **Authors:** Kiran Ramnath, Kang Zhou, Sheng Guan, Soumya Smruti Mishra, Xuan Qi, Zhengyuan Shen, Shuai Wang, Sangmin Woo, Sullam Jeoung, Yawei Wang, Haozhu Wang, Han Ding, Yuzhe Lu, Zhichao Xu, Yun Zhou, Balasubramaniam Srinivasan, Qiaojing Yan, Yueyan Chen, Haibo Ding, Panpan Xu, Lin Lee Cheong. All are at Amazon Web Services.
- **Dates:** v1 24 Feb 2025; v2 2 Apr 2025.
- **Venue:** arXiv cs.CL. It is formatted as an ACL-style short survey ("under 8 pages", §11). I recall it was later accepted at EMNLP 2025 †; the PDF does not say.
- **Length:** about 8 pages of main text plus about 20 pages of references and appendix. There are 57 methods in the comparison tables.
- **Link:** https://arxiv.org/abs/2502.16923

**Scope & definitions**
- **The field (§2).** Automatic prompt optimization (APO) is defined as black-box search for a prompt template ρ_opt = argmax_ρ E_{x∼D_val}[f(M_task(ρ ⊕ x))], given:
  - a task model M_task;
  - a seed prompt;
  - a metric f ∈ F;
  - a validation set D_val.
- The objective is called intractable because token-sequence spaces are combinatorial. Methods therefore follow a generic loop (Algorithm 1): seed → evaluate → generate candidates → filter → optional convergence check → return the argmax on D_val.
- **Selling points of APO (§1):**
  1. it needs no parameter access to the task LLM;
  2. it searches the prompt space systematically;
  3. it "retain[s] human interpretability of prompt improvements".
- **Prompt (App. 12.1).** ρ = concat(I, τ, E): the instruction, the template delimiters and the few-shot demonstrations. Other defined terms:
  - LLM_target, LLM_rewriter and LLM_evaluator;
  - latency symbols λ_t (training), λ_i (per-example inference) and λ_m (MLM). These are used for the "optimization time complexity" column.
- **Exclusion criteria (App. 12.2).** The survey covers discrete, black-box, input-prompt optimization only. It excludes:
  - soft or continuous prompts (FedBPT);
  - methods that do not optimize the input prompt (Deliberate-then-generate, Reflexion);
  - gradient-access methods (AutoPrompt).

**Taxonomy** (Fig. 1; five parts that follow the steps of Algorithm 1)
1. **Seed prompts (§3)**
   - *Manual instructions (§3.1):* ProTeGi, GPS, SPRIG. It notes APE's finding that a few hundred samples are enough.
   - *Instruction induction via LLMs (§3.2):*
     - Honovich et al. (first to do this);
     - APE and DAPO, which use induced seeds;
     - MOP and GPO, which use APE to induce per-cluster prompts;
     - SCULPT, which induces from task READMEs;
     - UniPrompt, which fills structured templates.
2. **Inference evaluation & feedback (§4)**
   - *Numeric score (§4.1):*
     - task accuracy (§4.1.1): exact match, execution accuracy, BLEU/ROUGE/BERTScore;
     - reward-model score (§4.1.2): RLPrompt, OIRL (an XGBoost reward on query–prompt embeddings, used for best-of-N), DRPO (LLM-based reward with dynamic criteria);
     - entropy-based (§4.1.3): CLAPS, GRIPS. These need the full output distribution;
     - negative log-likelihood (§4.1.4): APE, GPS, PACE. These need logprobs.
   - *LLM feedback (§4.2):*
     - improving a single candidate (§4.2.1): SCULPT (hierarchical prompt tree), PACE (actor-critic), CRISPO (multi-aspect critique), AutoHint;
     - improving multiple candidates (§4.2.2): ProTeGi and TextGrad (textual "gradients"), PromptAgent (error collection toward expert-style sections), PREFER (feedback-reflect-refine ensemble), SOS (safety multi-objective), StraGo (guidance from both correct and incorrect predictions).
   - *Human feedback (§4.3):*
     - GATE (generative active task elicitation);
     - Cheng et al./BPO (a small LLM trained on user preferences);
     - PROMST (human-designed feedback rules plus a learned heuristic, for multi-step tasks);
     - APOHF (preference-only, dueling bandits).
3. **Candidate prompt generation (§5)**
   - *Heuristic-based edits (§5.1):*
     - Monte Carlo sampling: ProTeGi, PromptAgent (MCTS);
     - genetic algorithms:
       - token mutation: SPRIG (a corpus of 300 components), CLAPS;
       - LLM-based mutation: LMEA, SOS, StraGo;
       - self-referential: PromptBreeder (direct mutation, hypermutation, Lamarckian mutation, crossover/shuffling);
       - differential evolution: EvoPrompt;
       - sentence-level edits with reward histories: AELP;
       - back-translation/cloze/continuation: GPS;
       - a combined pipeline: PromptWizard;
     - word/phrase edits: COPLE (influential tokens replaced by MLM predictions), GRIPS (add/delete/paraphrase/swap);
     - vocabulary pruning: CLAPS (k-means, top-2000 words), BDPL (PMI n-grams), PIN (Tsallis-entropy regularized RL).
   - *Editing with an auxiliary trained NN (§5.2):*
     - RL: MORL-Prompt, PRewrite, directional stimulus prompting, TEMPERA (test-time editing), Prompt-OIRL, BDPL (variance-reduced policy gradient);
     - LLM finetuning: BPO (7B, reward-free alignment), FIPO (7–13B; SFT and preference optimization);
     - GAN: Adv-ICL (Long et al.), with an LLM generator and discriminator whose prompts are rewritten by a modifier LLM.
   - *Metaprompt design (§5.3):*
     - PE2, which argues the meta-prompt space is under-explored;
     - OPRO, whose meta-prompt holds the problem description, past solutions and scores (App. 14.2);
     - DAPO, which uses a structured initial prompt and then sentence-level iteration.
   - *Coverage-based (§5.4):*
     - single-prompt expansion: AMPO (if-then-else branches built from failure patterns), UniPrompt (task facets);
     - mixture of experts: MOP (k-means demo clusters, one expert prompt per cluster, routed by centroid at inference);
     - ensembles: PromptBoosting, BoostedPrompting, PREFER, GPO (majority vote on unlabeled target data).
   - *Program synthesis (§5.5):* DSP, DSPy, DLN (stacked LLM layers with variational inference), MIPRO, SAMMO (prompts as DAGs with mutation rules).
4. **Filter & retain (§6)**
   - TopK greedy (§6.1): ProTeGi, AELP. It contrasts this with beam search over whole trajectories.
   - UCB and variants (§6.2): ProTeGi and SPRIG with UCB over resampled D_sample (App. Alg. 2); PromptAgent with UCT; AELP with LinUCB on prompt embeddings.
   - Region-based joint search (§6.3): MOP.
   - Metaheuristic ensemble (§6.4): PLUM (hill climbing, simulated annealing, GA, tabu, harmony search).
5. **Iteration depth (§7)**
   - Fixed N steps (most methods).
   - Variable steps: GRIPS (patience on negative gains), PromptAgent (stop when r_t ≤ ε_min or r_t ≥ ε_max).

Two further sections sit outside the five-part taxonomy:
- **Theoretical perspectives (§8):** AlignPro gives an upper bound on APO gains and a suboptimality gap against the RLHF-optimal policy; the lower bound is left open. Also covered: a control-theoretic reachability analysis (Bhargava et al.) and a universal-approximation existence result (Liu et al. 2024c).
- **Comparison tables:** Table 1 shows 7 headline methods. Tables 2–4 tabulate all 57 methods on 8 axes: seed, evaluation, generation, search/filter, depth, time complexity, prompt-generation model and target models.

**Key claims & findings**
- Prompt sensitivity is the motivation: wording, ordering and formatting create gaps between semantically similar prompts (§1).
- LLM feedback is "versatile" because it can judge both the prompt and the response, and it needs no task-specific metric. Its cost is an extra LLM call (§4.2).
- Heuristic edits "may not result in the most optimal solution" but make discrete search tractable (§5.1).
- COPLE: swapping a few words for synonyms can yield large gains, i.e. lexical sensitivity. Token replacement is proposed as a post-processing step for long prompts already optimized by LLM-based methods (§5.1.3).
- SPRIG shows that system-prompt and task-prompt optimization are complementary (§5.1.2).
- Relying on one static evaluation set "can lead to biases in the selection procedure", which motivates UCB with resampled subsets (§6.2).
- Human feedback: BPO reports "up to 22% increase in win rates for ChatGPT and 10% for GPT-4" (§4.3). This is the survey restating a cited result, not its own experiment.
- Cost asymmetry: SPRIG "required a predefined corpus and close to 60 hours", whereas ProTeGi took about 10 minutes per task (§9.3).
- The survey runs no experiments of its own and reports no head-to-head numbers. Its contribution is the framework plus Tables 2–7.

**Evaluation practices it describes or criticizes**
- **Metrics it describes (§4.1):** accuracy and execution accuracy; BLEU/ROUGE/BERTScore for generation; reward models; entropy; NLL.
- **Benchmark landscape it catalogs (Tables 5–7):**
  - heavy reliance on BBH, Instruction Induction (24 tasks), GSM8K, SST-2/5, TREC, Subj, GLUE, MMLU, Ethos/Liar/Sarcasm;
  - a few agentic or multi-step tasks (PROMST: WebArena, ALFWorld, ScienceWorld, BoxNet; GATE: LAPS);
  - a few industrial or internal tasks (StraGo, UniPrompt).
  It catalogs this mix but does not critique it.
- **Its criticisms:**
  - every method assumes the task type is known, and offline methods assume a labeled D_val, "something not explicitly available in production settings" (§9.1);
  - "more robust evaluations are needed for task-agnostic APO systems combining seen and unseen tasks" (§9.1);
  - single-dev-set selection bias (§6.2);
  - an implicit concern about compute cost, expressed through the big-O cost column.
- **What it does not discuss:**
  - held-out versus dev overfitting;
  - seed variance;
  - transfer of optimized prompts across target models;
  - judge or reward hacking.

**Open problems / future directions it lists (complete, §9)**
1. **Task-agnostic APO (§9.1):** inference-time optimization over multiple unknown tasks without a known task type or labeled D_val. It is underexplored apart from GATE, OIRL, TEMPERA and PIN.
2. **Unclear mechanisms (§9.2):**
   - uninterpretable "evil twin" prompts recover gold-prompt performance (Melamed et al.);
   - gibberish delimiters are competitive (Lu et al.);
   - LLM self-reflection suffers from "incorrect error identification, prior biases, semantic invalidity" (Yang et al. 2024b / ABO);
   - more mechanism studies are needed.
3. **APO for system prompts and agents (§9.3):** system-prompt optimization does not scale (SPRIG takes about 60 h). Concurrent optimization of prompts for several components of an agentic system is called "an exciting direction".
4. **Multimodal APO (§9.4):** text-to-image, text-to-video, text-to-audio and CLIP-style models. Joint optimization across modalities (e.g. denoising audio, visual markers on video) is underexplored.

The paper also states limitations (§11):
- possible missed papers;
- coarse single-label categorization; TEMPERA, for example, spans RL and word-level editing;
- the main comparison tables are relegated to the appendix.

**What it misses or gets wrong as of Nov 2026** (compared with the project files)
- **Coverage ends around late 2024.** It omits the systems that dominate the project's literature:
  - GEPA (reflective Pareto prompt evolution);
  - MIPROv2-era DSPy optimizers, beyond a MIPRO row;
  - ACE/agentic context engineering;
  - Meta-Harness, DGM, ADAS and Gödel-Agent-style harness or code evolution;
  - skill/memory libraries (ReasoningBank).
  The field has moved from "prompt" to "harness/context/scaffold" as the optimized object. Its "APO for agents" future direction (§9.3) is now the mainstream (round1 LITERATURE.md; FINAL_REPORT).
- **No treatment of evaluator gaming or overfitting.** The project knowledge records:
  - GEPA/harness evolution gaming evaluators (C6EElQ2PRG; GX4ucmiKZa, where 14/18 loops keep a held-out-worse version because of incidental evaluator strings);
  - false-success claims rising across rounds (VHE, 5lWjRTK6Vk);
  - GEPA writing readable reward-hacking strategies into prompts.
  The survey's only nod to this is the UCB dev-set-bias remark (§6.2). Its claim that APO "retains interpretability" is undercut by its own §9.2 evil-twin and gibberish evidence.
- **No transfer, drift or continual axis.** It does not cover:
  - cross-model prompt transfer (the project file notes GEPA prompts evolved on cheap tiers transfer upward at 5.6–54x lower cost);
  - drifting-stream evaluation (GEPA, Meta-Harness and ReasoningBank on four drift streams, plasticity versus stability r = −0.72);
  - optimizer meta-learning or transfer (RSI-Harness, MetaEvoForge, RSI-Opt 2610.32026, where held-out transfer peaks at gen 1; "Opinionated Optimizers" LhhLpXBFgo, where selection rarely compounds).
- **Reflection-as-teaching is only half-acknowledged.** §9.2 cites ABO's finding that self-reflection mis-identifies errors, but the taxonomy treats LLM feedback purely as a helpful signal. It never separates the trace/feedback channel from the score/selection channel.
- **Internal inconsistencies I found in the full text:**
  - Table 1 gives APE's search as "UCB", but Table 2 gives "TopK selection".
  - ProTeGi is called UCB in §6.2, "UCB for trees" in Table 1 and "UCT-based bandit-search" in Table 2.
  - Table 11 attributes the "Reflection" feedback prompt to Cieri et al. 2022, a language-resources retrospective. This looks like a citation error; the example resembles TextGrad.
  - Instruction induction is cited as both 2022 and 2023.
  - BPO appears under both human feedback (§4.3) and finetuning (§5.2.2).
  - Several rows have empty cells: CLAPS and DSPy tasks, and some search strategies.
- **Its main claimed contribution (Tables 2–4) is descriptive.** It has no quantitative meta-analysis, so it cannot say which design choices actually matter.

**Relevance to Dan's ideas**
- **Taught, Not Selected (high, as framing).** The five-part anatomy cleanly separates *inference evaluation & feedback* (§4, the reading/teaching channel: LLM feedback and textual gradients) from *filter & retain* (§6, the selection channel: TopK and UCB). That is exactly the paper's 2×2.
  - Cite it as the canonical decomposition, then point out that the survey treats the two as one pipeline and never asks which channel carries error.
  - §9.2 (ABO: self-reflection mis-identifies errors) and §4.2 (StraGo, written to avoid damaging well-working prompts) are the only places it hints that feedback can teach wrong lessons.
  - The §4.2.2 distinction between improving a single candidate and improving multiple candidates maps onto how much the optimizer reads.
- **Propose / Propagate / Pay-off funnel (medium).**
  - Taxonomy step §5 ≈ Propose, §6 ≈ Propagate (the retention rule), and §7 plus the final argmax ≈ Pay-off.
  - PromptBreeder's hypermutation (§5.1.2) is the survey's only self-referential improver: mutation prompts that edit themselves. It is the natural APO-level precedent for "does the improver improve itself".
  - The survey never measures retention or downstream payoff of edits.
- **Meta-Overfitting (medium-low).** The survey has no notion of optimizer transfer or meta-generalization. §9.1 (task-agnostic APO) and §9.3 (system prompts) are the closest. Useful as a citation for the "task type assumed known, D_val assumed" premise that meta-overfitting violates.
- **Meta-reward (low-medium).** It catalogs the reward signals a meta-reward study would choose among (§4.1: accuracy, reward model, entropy, NLL; §4.2 LLM judge; §4.3 human), but all of them are order-0 task rewards. It gives AlignPro's upper bound on APO gains (§8.1) as the only theory.
- **Openness ladder (medium).** The survey's candidate-generation axis is roughly an openness ladder:
  - word/phrase edits and vocabulary pruning (lowest);
  - LLM rewrite;
  - metaprompt;
  - coverage/MoE;
  - program synthesis (DSPy, SAMMO DAGs).
  This roughly maps onto L0–L2. Code or harness editing (L3+) is entirely out of scope. The table can serve as a related-work map for the lower rungs.
- **Continual / drift (low).** Absent apart from test-time or query-dependent methods (TEMPERA, OIRL, PIN, GATE in §9.1). This is a gap Dan's drift line fills.
- **Register snowball (low).** Not addressed. Tangentially relevant: PromptAgent and DAPO steer prompts toward an "expert" sectioned register (§4.2.2, §5.3), and PromptWizard uses an expert persona to keep style consistent (§5.1.2). That is evidence that optimizers converge on a house style, but no measurement is given.
- **Bottom line.** Use it as the standard citation for the APO pipeline decomposition and as an example of how the field framed prompt optimization up to early 2025. It contributes no evidence on generalization, hacking, transfer or self-improvement dynamics, which are the dimensions all of Dan's ideas target.

# Round 2: verification of unchecked leads, part B

*Compiled 2026-11-12. Sources: Semantic Scholar (CorpusId and arXiv lookups, forward citations) and alphaXiv full-text reads (`answer_pdf_queries`). Every ID, date and number below appeared in a tool result. Every claim about a paper's content comes from reading that paper's text, except for two papers that were judged from their abstracts only: Continual Learning Bench and Critique of Agent Model.*

The top round-1 ideas these papers are checked against:
- **#1 Merged Effect/Cause/Decay** (improver swap curve + procedure-vs-memory transplant + meta-plasticity).
- **#2 Evolve-in-dream**: simulator-scored harness evolution, with a fidelity manipulation.
- **#3 Learning-to-Goodhart**: does evolving on hackable evaluators teach exploit-seeking on held-out artifact families?
- **#5 Curriculum-Goodhart**: the per-generation gap between the self-written curriculum proxy and the truth.

---

## TL;DR

1. **None of the 11 leads is a kill.** No paper makes the central claim of any top-4 idea with the same design.
2. **SelfSearch (2609.37968) is the most important find, and it raises the risk to #1's Cause arm to high.** Its Table 4 already runs a crude version of the procedure-vs-memory split, in two ablations:
   - *without episode records* (memory removed);
   - *with a fixed improver* (procedure frozen at B0).

   Both ablations lower SWE-bench Verified success by about 2–3 pp versus the full system.

   It leaves large gaps:
   - one search per setting (no seeds) and two lineages;
   - only generation 10 is evaluated, so there is no generation curve;
   - no transplant across states and no domain shift;
   - the differences are 1.7–2.9 pp on 120 tasks, which is within noise.

   The authors themselves write that "further work should examine how consistently these gains arise across independent searches." **#1 must cite it and position itself as the powered, factorial version of SelfSearch's Table 4.**
3. **Dream-RSI (2609.14858, Google/GDM) occupies "dreaming" at the meta-exploration layer and is a medium threat to #2.** Its simulator is a *non-parametric replay* of recorded discovery trees, not an LLM world model.
   - It never measures replay-vs-online fidelity.
   - By construction it cannot score off-support behaviour ("out of support cannot earn replay reward").

   This gives evolve-in-dream a sharper and non-obvious extension (section 3, idea B).
4. **MetaExp (2610.21492, Alibaba/Qwen) is a medium threat to the Decay and meta-plasticity framing.** It shows that "experience learning" can be *trained into weights* and transfers across domains: +8.5 on SpreadsheetBench, and still +9.3 under strict-OOD training.
   - It also shows that untrained backbones *degrade* as experience accumulates (−2.2 for Qwen3.5-122B, −1.7 for Qwen3.6-Plus).
   - It is weight-level and actor-level, with no self-referential improver and no generation axis.
5. **Beyond Endpoint Performance / EvoPathBench (2609.24663)** gives process-level evaluation at checkpoints K0–K5. Its headline finding is that *candidate artifacts have large held-out gains but the selected updates fall short*.
6. **Several leads independently report the same failure, which points to a common open question:**
   - **Aspire (2608.31111):** agents trust narrow self-evaluations, and continued search erases gains.
   - **RSI-Forge (2610.09426):** weaker models accept gains below the standard error 73% of the time, and final submissions are sometimes worse than saved versions.
   - **VibeTraining-Bench (2610.26342):** the validation leader is often not the test leader, and search is non-monotone.

   **The bottleneck of self-improvement is selection, not generation.** Nobody has measured how *selection regret* evolves across improver generations. This is a cheap, non-obvious readout for #1 (section 3, idea C).
7. **S3Gym (2608.31100) has a surprising null.** Self-judging accuracy does not predict subsequent improvement: ρ(agreement, gain) = −0.010 over 98 runs and 116k transitions. This is a low threat but a useful citation for #3.
8. **The remaining leads are low threats.** Several are useful as testbeds or methodology:
   - PAST-Bench (2608.04003): on/off persistence ablation plus mechanism evidence.
   - VibeTraining-Bench (2610.26342).
   - RSI-Forge (2610.09426): 210 paper-derived environments with coded shortcut families, a good learning-to-Goodhart testbed.
   - Continual Learning Bench (2606.05661).
   - Critique of Agent Model (2606.23991): a position paper.
9. **A new non-obvious idea appears in SelfSearch's own data.** Its improvers increasingly spend their edits on *self-inspection tooling*: generations 6–10 are all about trajectory readers, filters and call/result linking. The GPT trajectory reader is **never invoked downstream**. I call this "introspective drift" (section 3, idea A). It is cheap, squarely on Gödel Agent, and unlikely to be scooped.

---

## 1. Per-paper verification

### 1.1 PAST-Bench: Benchmarking the Foundations of RSI in Personal Agents
- **ID:** arXiv 2608.04003 (S2 CorpusId 290836900). Published 2026-08-04. 13 citations. Gen-Verse, with Mengdi Wang and Ling Yang.
- **Design:**
  - 26 synthetic task families, 204 episodes, across four capabilities:
    - Memory (41 episodes)
    - Procedural reuse (64)
    - Information gathering (48)
    - Update (51)
  - Ordered fresh-session episodes with roles cold / learn / update / evaluate / control.
  - A matched persistence-on vs persistence-off ablation (same prompt, grader, tools and seed).
  - Control episodes: no-retention, distractor, stale and wrong-mechanism.
  - A **mechanism-evidence score** (Mech) checks write → retrieve → correct-application traces.
  - 7 base models with the Hermes framework, plus 4 frameworks with MiniMax-M2.7.
- **Findings:**
  - Persistence helps every model: overall Δ ranges from +0.13 to +0.24.
  - Gains concentrate in Memory and Update. Procedural Δ is small (+0.03 to +0.19). Information-gathering Δ is +0.05 to +0.11.
  - The same headline Δ can hide different mechanisms: nanobot and Hermes both have Δ = +0.13, with Mech 0.57 vs 0.64.
  - Agent-Zero regresses (−0.08).
  - Hermes+ (five loop interventions) moves overall Δ from +0.13 to +0.15, but this is *within run-to-run SD* (±0.04 / ±0.06). Its clearest gain is on Update (+0.12 → +0.24).
- **Limitations:**
  - Synthetic tasks, generated by GPT-5.4 and Opus 4.6.
  - Short sequences. Mech measures consistency with the expected pathway, not causal necessity. The authors list "deleting, replacing, or corrupting a candidate artifact" as future work.
  - No self-modifying improver: the framework code is fixed.
- **Overlap:**
  - #1: low. It is an object-level memory benchmark. Its on/off ablation and its proposed (not run) artifact-corruption counterfactual are exactly the *placebo/shuffled-memory* arms of #1's transplant, applied to the actor. Cite it as methodology precedent.
  - #2, #3, #5: none.
- **Severity: low.**

### 1.2 S3Gym: Can LLMs Turn Self-Testing and Self-Judging into Self-Improvement?
- **ID:** arXiv 2608.31100 (CorpusId 291542162). Published 2026-08-31. ByteDance Seed / M-A-P (same group as Aspire).
- **Design:**
  - 7 text games (Chess, Minesweeper, Nullify, Tetris, Snake, PvZ, Trust Evolution).
  - Permissive exploration configurations and strict held-out evaluation configurations, with disjoint seeds.
  - The agent emits a self-judged score s for each action. The verifier reward r is hidden during exploration.
  - Three pathways: History ICL, Summary Memory, SFT training (Qwen3-8B).
  - 7 proprietary models.
- **Findings:**
  - Self-improvement is not automatic. Summaries help when experience compresses into rules, and lose to raw history when precise state matters.
  - SFT is unstable and shows severe negative transfer on PvZ: every trained checkpoint drops from 23 to 6.
  - Self-judging is partly reliable. Chess agreement is 0.50; Trust has 29% over-confidence.
  - **Key null: judgment quality does not predict the next-checkpoint gain.** ρ(A, g) = −0.010 and ρ(−E, g) = −0.018 overall. Run-level agreement vs NABA gives r = −0.23.
- **Limitations:**
  - Games only; proprietary models; no seeds reported in the main table.
  - Exploration and evaluation configurations differ in difficulty, which confounds transfer.
- **Overlap:**
  - #3 learning-to-Goodhart: low. There is no exploit dimension. The null on judgment vs improvement is a useful motivating citation: "accurate judging is not what drives self-improvement, so what does?"
  - #1: low. It compares pathways (history vs summary vs weights), which resembles memory-format arms, but there is no improver.
- **Severity: low.**

### 1.3 VibeTraining-Bench: Data–Trainer–Harness Co-Design for Iterative System Improvement
- **ID:** arXiv 2610.26342 (CorpusId 293805195). Published 2026-10-19 (v2 Oct 20). Alibaba Token Hub, Zhejiang U, Westlake.
- **Design:**
  - Research agents (6 configurations: GPT-6-Astra, GPT-5.6-Sol, Kimi-K3, GLM-5.2, Qwen-3.8-Max, DeepSeek-V4-Pro) improve a Qwen3.5-9B-Base target system.
  - They may edit training data, trainer and inference harness (harness editable only on Terminal-Bench 2.1 and SWE-bench Verified).
  - Budget: 8×A100 and 16–24 h.
  - Hidden test set, separate grading container, no network access.
- **Findings:**
  - Average score rises from 22.7 to 31–38 (human Instruct model: 47.3). On SWE-bench Verified, GPT-6-Astra goes from 32.9 to 53.7.
  - Validation leadership does not predict test leadership, and validation search is non-monotone.
  - Agents mostly make lightweight edits (filtering, hyperparameters, merges) and rarely change algorithms.
  - On SWE-bench Verified, a harness-only run (+5 pp) is far below the unrestricted data/trainer/harness run (+19 pp).
- **Limitations:**
  - One run per configuration and task; no seeds.
  - The component ablation covers a single agent.
  - Expensive: GPU-heavy, so infeasible for Dan to reproduce.
- **Overlap:**
  - Harness↔weights cluster: low (benchmark only).
  - #1: none. The research agent is not self-referential; it improves a separate target system.
- **Severity: low.** It adds to the evidence that RSI benchmarks are saturated.

### 1.4 Aspire: Can Models Self-Evolve from Vague Goals?
- **ID:** arXiv 2608.31111 (CorpusId 291542942). Published 2026-08-31. ByteDance Seed / SUTD / M-A-P.
- **Design:**
  - Agents receive only a natural-language goal: six goals with 520 hidden expert-written items.
  - They choose data, the update method (SFT/GRPO/LoRA) or harness edits, and *build their own validation signals*.
  - Two protocols: adaptive feedback (bounded aggregate scores) and final-only.
  - RQ1: vague vs explicit goal. RQ2: weight evolution (Qwen3.5-4B/9B self, or GPT-5.6 Luna/Terra/Sol as decision models). RQ3: one-step harness evolution vs the Qwen-Agent reference.
- **Findings:**
  - **Weight evolution:**
    - Only 1 of 12 final-only outcomes beats base.
    - In the adaptive protocol, only one cell (Terra on math, 17.86 → 20.10) is retained above base, and its later descendants fall to 2.78 before recovering.
    - Agents import GSM8K/MATH data for science, logic and writing goals, and train on single-digit MMLU labels, which collapses scores to 0.
  - **"Apparent progress can be recovery from training-induced regression"**: of 62 consecutive transitions, 28 rise and 21 fall. A positive within-lineage slope can sit entirely below base.
  - **Harness evolution:**
    - All successors are below Qwen-Agent (Sol 27.22 vs 28.64).
    - Luna overfits its own 8-item checklist (7/8 → 8/8) with a template that *hurts* hidden items.
- **Limitations:**
  - No seeds ("not a multi-seed estimate").
  - Harness evolution is one step only (no H1 → H2).
  - Mostly small open models.
  - Mechanisms are shown through case studies.
- **Overlap:**
  - **#5 curriculum-Goodhart: medium.** It qualitatively documents the gap between an agent-built proxy and the truth ("proxy gain ≠ transferable growth"), including narrow self-validation specialising a harness to the wrong proxy. It does not measure a *per-generation* proxy–truth curve, does not vary the task writer, and uses vague goals rather than a self-written curriculum. #5 should cite it as qualitative motivation and claim the quantitative, multi-generation, multi-seed curve.
  - **#1: low, but methodologically important.** "Recovery from regression masquerading as improvement" is a confound the Effect arm must control. Always report gain relative to gen-0 *and* relative to base, never within-lineage slope alone.
- **Severity: medium for #5; low for #1.**

### 1.5 RSI-Forge: From Research Papers to Environments for RSI
- **ID:** arXiv 2610.09426 (CorpusId 292694017). Published 2026-10-07. Scale AI.
- **Design:**
  - A multi-agent pipeline (author / reproducer / reviewer) turns papers into executable environments: 210 environments across 18 fields, 90 reviewed by experts.
  - Validation: 4 models (Opus 5, GPT-5.6-sol, DeepSeek-V4-Flash, Haiku 4.5) × 3 successive 60-minute sessions on 120 environments.
  - Each session inherits the prior code, saved versions, experiment log and notes. Weights are fixed.
- **Findings:**
  - At least one model improves after session 1 in 84% of environments. Models beat the reproduced paper method in 68/120.
  - Experts rate shortcut resistance (4.09) and saturation resistance (3.09) lower than agent judges do.
  - A 14-family "shortcuts" codebook, with session frequency by model:
    - "Keep changes when dev score improves": 71% Haiku vs 11% Opus.
    - "Fit constants to dev set": 56% vs 17%.
    - "Read the answer key": ~6% for all models.
  - **73% of Haiku's accepted improvements are smaller than the reported SE** (Opus: 44%).
  - Final submissions are sometimes worse than earlier saved versions.
- **Limitations (stated):**
  - "The study does not isolate the effect of inherited records from extra solving time." This is the memory-vs-compute confound that #1 addresses.
  - Some environments left held-out grading materials accessible.
  - Transcript labels are model-coded.
  - Only 3 sessions.
- **Overlap:**
  - #1: low. It is a multi-session actor, not a self-modifying improver. The stated limitation is direct motivation for the Cause arm.
  - **#3 learning-to-Goodhart: low threat, high opportunity.** The environments, with coded shortcut families and expert shortcut-resistance ratings, are a ready-made testbed of *naturally occurring* evaluator artifacts. They complement the planted-artifact design.
- **Severity: low.** It also confirms the round-1 view that "another RSI benchmark" is saturated.

### 1.6 Dream-RSI: Recursive Self-Improvement through Evolving Worlds
- **ID:** arXiv 2609.14858 (CorpusId 292025925). v1 2026-09-14, v2 2026-10-06. Google / Google DeepMind / UMD. 27 citations.
- **Design:**
  - An orchestration layer makes the *exploration policy* (branching, parallelism, stopping) programmable code, while the base discovery agent stays fixed.
  - After each online round, the discovery tree becomes a **replay simulator**. Candidate policies are scored by replaying recorded nodes deterministically, with a hypervolume quality–cost objective minus a parallelism penalty, swept over a budget knob β.
  - A policy-development agent rewrites the policy code M = 4 times per round. The best candidate on replay is deployed online, and the simulator pool grows.
  - Tasks:
    - Lasso path solver
    - 4 KernelBench tasks
    - 3 math-optimisation tasks (sum–difference, autocorrelation, circle packing)
    - Terminal-Bench 2.1 harness optimisation (Opus 4.8 + Claude Code)
  - Models: Gemini-3.1-Pro / 3.7-Flash for discovery.
- **Findings:**
  - Lasso: up to 162× fewer agent calls than SimpleTES.
  - KernelBench: 1.79–2.43× fewer generations to match, or up to 2.09× better kernels.
  - Terminal-Bench harness: more than +8 pts.
  - Accumulating all history beats current-round-only replay (1.90 vs 1.59 on ConvDiv).
  - Prompt-level "directional guidance" from history *hurts* both methods.
  - The learned policy adaptively cuts and then re-expands exploration effort.
- **Limitations:**
  - **No sim-vs-real fidelity analysis.** Replay returns only recorded children, so plans beyond the recorded tree are "out of support and cannot earn replay reward". Off-policy bias toward the logging policy is not examined.
  - The harness experiment has no fixed-policy control ("estimated >$100,000").
  - Single runs.
  - Only the meta-exploration layer evolves; the agent itself does not.
- **Overlap:**
  - **#2 evolve-in-dream: medium.** It owns the phrase "dreaming over history for RSI" and runs a harness-optimisation demo. It does *not*:
    - use an LLM world model with model error;
    - inject quirks;
    - sweep evolution depth × fidelity;
    - measure real-transfer peak and decline.

    Evolve-in-dream must cite it and frame the replay simulator as the *zero-model-error, finite-support* end of a simulator spectrum (see idea B).
  - #1: low. Its meta-level self-improvement with a fixed base agent is an interesting extra "improver" for the swap design, because its policy versions are checkpointed.
- **Forward citations (S2):** about 27. The most relevant are:
  - 2610.27610 *Auditing the Product, Not the Recursion*
  - 2610.23839 DiscoveryRSI
  - 2610.20431 SwarmRSI
  - 2610.00675 LabBook
  - 2609.39392 Experimental Experience Modeling (EEM)
  - 2609.38812 *Can Terminal Agents Trust Their Own Verification?*

  From titles and abstracts, none is a replay-fidelity study.
- **Severity: medium for #2.**

### 1.7 SelfSearch: Reward-Free Search for Self-Improving Agents
- **ID:** arXiv 2609.37968 (CorpusId 292474846). v2 2026-09-30. Seoul National University (Yohan Jo). No forward citations yet.
- **Design:**
  - **Self-modification:**
    - A single agent repository is both the task solver and the self-modifier. The whole repository is editable; weights are fixed.
    - Each episode: the agent B_k edits a copy of itself using *read-only records of previous self-improvement episodes* (trajectories, code diffs, check results).
  - **No downstream reward during search:**
    - No downstream tasks or scores are used during search, only local checks.
    - Two lineages (capability and adaptive directions) share records each generation. There are 10 generations, and the population is 2 agents.
  - **Setup:**
    - Models: GPT-5.6-Sol and DeepSeek-V4-Pro for search; Luna and V4-Flash for execution.
    - Benchmarks: SWE-bench Verified (120 tasks), SWE-bench Multilingual (60), Terminal-Bench 2.1 (89).
- **Findings:**
  - The population mean beats B0 in all 6 model–benchmark settings, by up to +11.2 pp on Terminal-Bench 2.1.
  - It matches the Codex harness (82.0%) at a $4.03 search cost.
  - Harnesses transfer across model families.
  - **Table 4 mechanism ablations, gen-10 agents on SWE-bench Verified (initial / no records / fixed improver / full):**

    | Family | Initial | w/o episode records | w/ fixed improver | Full |
    |---|---|---|---|---|
    | GPT | 75.0 | 74.2 | 74.6 | 76.2 |
    | DeepSeek | 81.7 | 83.8 | 83.8 | 86.7 |

    The authors conclude that "both previous episode records and an evolving improver contribute."
  - **Generations 6–10 are dominated by self-inspection tooling** (Table 5): trajectory readers, output limits, filters, call/result linking. The GPT trajectory reader is "not invoked during downstream evaluation".
- **Limitations:**
  - One search per setting; 2 lineages.
  - Differences of 1.7–2.9 pp on 120 tasks, with no CIs.
  - Only gen-10 is evaluated in the ablations.
  - No compute-matched baseline beyond evaluation-guided search.
  - The prompts were tuned by inspecting pilot trajectories.
- **Overlap:**
  - **#1 Merged Effect/Cause/Decay: high (not a kill).** SelfSearch publicly runs two of #1's eight transplant cells (P0 with records Mk, and Pk without records) at a single state with no seeds, and frames the question in #1's own words: "Can agents use the experience of self-improvement to become better at improving themselves?" #1's remaining gap:
    - the full P0/Pk × M0/Mk factorial with format adapter and shuffled/placebo memory;
    - multiple generations k, giving the k* curve;
    - 5+ seeds and paired bootstrap;
    - matched compute;
    - a domain-B decay arm.

    This is now the main must-cite for #1's Cause arm, alongside HMED and VERSE. The positioning: "SelfSearch's ablation is suggestive but within noise; we power it."
  - #3: SelfSearch is *evaluator-free*, so it is a natural third arm (no evaluator vs clean vs hackable). It disentangles exploit-seeking from optimization pressure.
  - #2, #5: none.
- **Severity: high for #1 (Cause arm).**

### 1.8 MetaExp: Learning to Self-Improve through Experience via Verifiable Cross-Task Environment Scaling
- **ID:** arXiv 2610.21492 (CorpusId 293003768). Published 2026-10-15. U Macau / Georgia Tech / Alibaba Qwen.
- **Design:**
  - 1,000 synthetic verifiable task families of 5 tasks each:
    - Shared-Tool families: operational experience.
    - Shared-Workflow families: a latent procedure across domains and tools, for procedural experience.
  - A teacher (GLM-5.2) solves the families sequentially with a skill bank and reflection, producing 4,956 filtered trajectories.
  - SFT of Qwen3.5-122B-A10B on those trajectories.
  - Test: cold-start sequential evolution (SeqEvo) with frozen weights and an empty skill bank, on SpreadsheetBench, APEX-Agents and AutomationBench, with 3 random orders.
- **Findings:**
  - SpreadsheetBench: +8.5 (65.0 → 73.5) by k = 40.
  - AutomationBench: +14.8. APEX: +1.8 to +2.3.
  - Strict-OOD training (related domains removed): +9.3.
  - **Untrained backbones get worse with experience:**
    - Qwen3.5-122B: −2.2.
    - Qwen3.6-Plus: −1.7 (at k = 40; −4.2 at k = 5).
    - GLM-5.2: only +1.1.
  - Shared-Tool vs Shared-Workflow have complementary effects.
- **Limitations:**
  - MetaExp's k = 0 score is lower than base (65.0 vs 68.9), so part of the Δ is recovery.
  - One backbone; SFT only; skill banks capped at 5–10.
  - No improver self-modification.
- **Overlap:**
  - **#1 Decay / meta-plasticity: medium.** It answers "is improvement ability a transferable capability?" with *yes, if trained into weights*, at the actor or experience-learner level. #1's question is different: does a self-referential improver's *procedure* get better or worse across generations, and does that carry to domain B without training? MetaExp's "backbones degrade with experience" result is a strong motivating fact for the warm-start-penalty hypothesis.
  - Related weight-level line: Connect the Dots 2606.20002 and Evolving-RL 2605.10663 (both cited inside MetaExp; not re-verified here).
  - #2, #3, #5: none.
- **Severity: medium for #1's Decay arm and the meta-plasticity framing.**

### 1.9 Critique of Agent Model (Eric Xing et al.)
- **ID:** arXiv 2606.23991 (CorpusId 289626079). Published 2026-06-22. Authors: E. Xing, Ming-Kai Deng, Jinyu Hou. 4 citations. *Abstract only.*
- **Content:**
  - A position and survey paper on what counts as agency.
  - It analyses agent architectures along five dimensions: goal, identity, decision-making, self-regulation, learning.
  - It distinguishes "agentic" systems (competence in engineered scaffolds) from "agentive" systems (internalized capability).
  - It proposes the Goal-Identity-Configurator (GIC) architecture: hierarchical goals, identity evolution, simulative reasoning with a *separately trained world model*, learned self-regulation, and self-directed learning from real and simulated experience.
- **Overlap:** none empirically. It is useful framing for #2: "learning from simulated experience" is a stated pillar of a general agent model, and our paper tests when that is safe. A reviewer from the MBZUAI/CMU circle may also ask whether harness-level ("agentic") self-improvement is "real" self-improvement. #1's procedure-vs-memory result speaks to that directly.
- **Severity: low.**

### 1.10 Beyond Endpoint Performance: Process-Level Evaluation of Self-Evolving Agents (EvoPathBench)
- **ID:** arXiv 2609.24663 (CorpusId 292219775). Published 2026-09-21. Zhejiang U / Alibaba. 7 citations.
- **Design:**
  - Artifact-level self-evolution (memories and skills) with a fixed model, Qwen3.8-Max or Kimi-K3.
  - Trading tasks: Binance data plus calibrated synthetic paths; 6 families × 2 layers (exogenous and endogenous with 5 opponents); 1,800 streams.
  - Three stream templates:
    - Accumulation, for generalization (near and transfer).
    - Interference, for retention (K2 → K5).
    - Reversal, for rule adaptation (REV1 and REV2).
  - Frozen-artifact evaluation at K0–K5 against a non-evolving baseline (CEG) and an artifact-off ablation (SUE). Retention reports CVaR tails.
  - 10 methods: context, reflection, two memory variants, SkillOpt, SkillBoost, SkillX, Trace2Skill, SkillGrad.
- **Findings:**
  - Near-distribution gains often turn negative under transfer (SkillOpt +0.763 → −0.273).
  - Retention losses are concentrated in a minority of paths (CVaR ≫ mean).
  - No method shows significant rule adaptation after Holm correction.
  - Longer skills ≠ better.
  - **Candidate artifacts have substantial held-out gains, but the selected updates consistently fall short. Candidate evaluation and selection is "the primary bottleneck".**
- **Limitations:**
  - Trading only; 5 updates; short horizon.
  - Fixed update rules: no self-modifying improver and no improver generations.
- **Overlap:**
  - #1 Decay: low–medium. The retention and rule-adaptation dimensions are object-level analogues of the improver-level decay arm. The checkpoint-freeze plus artifact-off protocol is the same logic as #1's transplant, applied to artifacts. Cite it as the object-level precedent.
  - #5: low. The selection gap is a related phenomenon.
- **Forward citations include:** Artifact-to-Capability 2611.04952, SelfImprove-Bench 2611.02797, *Agent Self-Improvement with Frozen Weights* 2611.11342, CogRSI 2611.01113 and Cortex 2610.14216. This process-evaluation line is growing fast.
- **Severity: low–medium.**

### 1.11 Continual Learning Bench (Asawa, Gonzalez et al.)
- **ID:** arXiv 2606.05661 (CorpusId 288977039). Published 2026-06-04. 27 citations. *Abstract only*; round 1 already covered it in the plasticity sweep.
- **Content:**
  - Six expert-validated stateful domains: SWE, signal processing, outbreak forecasting, DB querying, strategic games, demand forecasting.
  - Each domain has a latent structure that a stateful agent can learn.
  - A gain metric separates learning from prior capability.
  - Finding: agents overfit to immediate observations and fail to reuse knowledge. **Naive ICL beats dedicated memory systems.**
- **Overlap:** low with all four ideas. It is a candidate domain-B testbed for #1's Decay arm, because it isolates learning from capability.
- **Severity: low.**

---

## 2. Overlap matrix

| Paper | #1 Effect | #1 Cause | #1 Decay | #2 Dream | #3 L2Goodhart | #5 CurrGoodhart |
|---|---|---|---|---|---|---|
| PAST-Bench 2608.04003 | – | low (method) | – | – | – | – |
| S3Gym 2608.31100 | – | low | – | – | low (cite null) | – |
| VibeTraining 2610.26342 | low | – | – | – | low | – |
| Aspire 2608.31111 | low (confound) | – | – | – | low | **medium** |
| RSI-Forge 2610.09426 | low | low (motivation) | – | – | low (testbed) | – |
| Dream-RSI 2609.14858 | low | – | – | **medium** | – | – |
| SelfSearch 2609.37968 | low–med | **high** | – | – | low (third arm) | – |
| MetaExp 2610.21492 | – | – | **medium** | – | – | – |
| Critique of Agent Model 2606.23991 | – | – | – | low (framing) | – | – |
| EvoPathBench 2609.24663 | low | low | low–med | – | – | low |
| CL-Bench 2606.05661 | – | – | low (testbed) | – | – | – |

**Required edits to the round-1 proposals:**
- **#1:** add SelfSearch Table 4 to "closest work" as the single-state, unseeded precedent of the transplant. Add MetaExp ("experience-learning transfers when trained into weights; untrained backbones degrade") to the Decay arm. Add Aspire's "recovery from regression" confound to the Effect-arm analysis plan.
- **#2:** add Dream-RSI as the closest "dreaming for RSI" system, and state the gap: no model error, no fidelity manipulation, no sim-vs-real curve, support-limited.
- **#5:** add Aspire as qualitative evidence of the gap between an agent-built proxy and the truth, with no per-generation curve.

---

## 3. Where the field is moving, and non-obvious ideas suggested by these leads

**Trends visible across these 11 papers (Aug–Nov 2026):**
1. **Evaluator-free or reward-free self-modification.** SelfSearch, and Aspire's vague goals. Expect a follow-up within months that combines SelfSearch with evaluation-guided selection; the authors list this as future work.
2. **Offline and replay meta-optimization of the improvement process.** Dream-RSI, LabBook 2610.00675, EEM 2609.39392. The "meta-layer RSI with a history-as-simulator" framing is a Google line and will grow.
3. **Training the improver or experience-learning capability into weights.** MetaExp; Connect the Dots and Evolving-RL are cited in it. This moves "learning to improve" from harness to weights, which makes the *harness-level* question in #1 more distinctive.
4. **Process-level and attribution evaluation.** EvoPathBench, PAST-Bench, and the forward citations Artifact-to-Capability and SelfImprove-Bench. Evaluation by checkpoint freeze plus artifact-off is becoming standard. A paper that only adds another benchmark is late; a paper that brings a causal factorial to the improver is not.
5. **The common unresolved finding: selection, not generation, is the bottleneck.** EvoPathBench (selected < candidate), RSI-Forge (accepting gains below SE; final worse than saved), VibeTraining (validation leader ≠ test leader), Aspire (narrow self-evaluation), S3Gym (judging accuracy does not predict gain). Everyone reports it at a single time point; nobody tracks it across improver generations.

**Non-obvious ideas.** Each is cheap and fits Xunjian's agenda. Each is less likely to be scooped because none of the trend lines above points directly at it.

- **Idea A: introspective drift (navel-gazing improvers).**
  - Claim: in self-referential loops, the fraction of edits that target *the self-improvement apparatus itself* rises with generation, while downstream-relevant edits fall. The apparatus here means trajectory readers, record filters and self-inspection tools.
  - Downstream gain saturates when this "introspective share" crosses a threshold.
  - Evidence it exists: SelfSearch's GPT lineage spends generations 6–10 on trajectory tooling, and the reader is never used downstream. Gödel Agent's "complexity ceiling" may be the same phenomenon.
  - Design:
    - Classify edits by whether the edited code is exercised during task solving vs during self-modification. This is cheap: instrument call coverage.
    - Track the share per generation and across improvers (Gödel-lite, SelfSearch-style, Meta-Harness-style).
    - Intervene by penalizing or forbidding self-tooling edits, and test whether the downstream slope recovers.
  - Either outcome is publishable:
    - Drift exists: "self-referential improvers optimize their own introspection."
    - Drift doesn't exist: self-tooling is a genuine meta-learning carrier, which feeds #1's procedure arm directly.
  - It can be a section of #1 (the "what does the procedure learn?" readout) or a standalone short paper.
  - Scoop risk: low. No paper found measures this.

- **Idea B: support-Goodhart in replay dreams.**
  - Claim: zero-error simulators (replay of recorded histories, as in Dream-RSI) also Goodhart, through *support* rather than model error. Selection inside replay favours policies that imitate the logging policy, because off-support actions earn nothing. Real gains therefore plateau early, and exploration diversity shrinks.
  - Design:
    - Inside evolve-in-dream, add a third simulator class (nonparametric replay) next to the clean LLM world model and the quirk-injected one.
    - Measure on-path ranking fidelity as a function of the candidate's KL or edit distance from the logging policy.
  - This turns #2 into a unified "model error vs support error" paper and directly differentiates it from Dream-RSI.
  - Scoop risk: low–medium. Dream-RSI's own follow-ups may add online-vs-replay checks, but not a quirk/fidelity sweep.

- **Idea C: selection regret across improver generations.**
  - Claim: what improves (or decays) across self-improvement generations is *selection*, not *generation*.
  - Design:
    - For every generation, replay *all* candidates, not only the selected one, on held-out data, as RSI-Forge and EvoPathBench did at one time point.
    - Decompose the gen-k gain into best-candidate quality (generation) and oracle-minus-selected regret (selection).
  - Prediction: gen-k improvers generate better candidates but select them worse (a winner's curse that grows with archive size). Alternatively, all the "improving at improving" is in selection.
  - Cost: one extra held-out pass per candidate; trivial in a procedural τ-world.
  - It slots into #1's Effect arm as its main readout and gives #1 a crisp mechanism claim.
  - Scoop risk: low–medium. The single-time-point version exists in four papers; the cross-generation version does not.

- **Idea D: experience can be net-negative, so the improver's job may be deciding when to forget (lower priority).**
  - Evidence: MetaExp shows untrained frontier backbones lose 1.7–4.2 pts as experience accumulates. CL-Bench finds naive ICL beats memory systems. S3Gym finds SFT negative transfer.
  - Hypothesis: much of what self-improvers "learn" is *pruning* bad experience.
  - This is testable as an edit-type analysis inside #1: are gen-k improvements dominated by deletions or restrictions?
  - Scoop risk: medium. Forgetting is a crowded keyword; see *Scaling Discovery Requires Forgetting*, 2610.20563 in round 1.

---

## 4. Caveats
- All papers are unreviewed 2026 preprints. Numbers are quoted from the PDFs via alphaXiv.
- Continual Learning Bench and Critique of Agent Model were assessed from their abstracts only.
- Forward-citation titles for Dream-RSI and EvoPathBench were not opened beyond the abstracts listed above. "None is a replay-fidelity study" rests on titles and abstracts.
- SelfSearch had 0 S2 citations at check time. Re-check weekly, since it is the closest public precedent for #1's Cause arm.

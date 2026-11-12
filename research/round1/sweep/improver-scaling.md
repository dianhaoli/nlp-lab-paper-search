# Sweep: Does improving the improver help? Meta-level ability over generations

Date: 2026-11-12. Scope: improver/meta-agent checkpoint evaluation, imp@k, metaproductivity (HGM), ignition tests (AIDE²/Weco), complexity ceilings (Gödel Agent), Metaⁿ, STOP, Hyperagents follow-ups, "improving at improving" metrics, and splitting gains into compute, search and meta-learning.

Method: I read the context files. I ran 8 alphaXiv discover searches (recency and historical) and 6 Semantic Scholar keyword searches. I pulled S2 forward citations for Gödel Agent (2410.04444, 26 citing papers), Hyperagents (2603.19461, 60), HGM (2510.21614, 69), AIDE² (2609.26457, 23), Metaⁿ (2608.24735, 7) and Agent Plasticity (2610.08902, 3), and filtered them to Jun–Nov 2026, which left about 120 citing papers. I deep-read 8 papers: Auditing the Product (2610.27610, full text), FreeEvolve (2610.09197, full text), RSIFlow, VERSE, Agent Plasticity, AIDE², Beneath the Surface, Physical-AI RSI, Skill∞ and Flex-RSI (targeted PDF queries). Every arXiv ID below appeared in a tool result. Two items marked "alphaXiv-only" have non-standard alphaXiv IDs and no arXiv ID that I saw.

---

## 1. Summary of the landscape (what changed since the reading list)

1. **The question now has a name, a formal estimand and a census.** *Auditing the Product, Not the Recursion* (2610.27610, Oct 20) defines five claims:
   - C1 product.
   - C2 accumulation.
   - C3 repeatability.
   - **C4 improver gain**, tested by a *swap test*: earlier and later improvers applied to the same starting artifacts at a matched budget.
   - **C5 compounding**: improver gains themselves grow.

   Its census of 114 studies (90 of them RSI) reports two results:
   - Of the **26 studies that claim a better improver, only 4 run a same-start, matched-budget swap test.** A further 3 run same-start without a matched budget, 7 compare evolving against frozen, and the rest make other comparisons.
   - **Proposition 3 (non-identification):** along a single trajectory, solver state and improver change together. "Gains can shrink as the solver approaches its performance ceiling even while the improver gets better. They can also grow when a fixed improver reaches states that are easier to improve." Identifying the improver effect needs *off-diagonal* observations (improver × state), and "additional trajectory data alone cannot" supply them. The evolving-vs-frozen control (Hyperagents, DGM style) answers a *different* question.

   The paper also flags the Hyperagents transfer experiment as failing the strict same-start rule, because the task agent moves together with the meta agent.

   Three other works use the same framing: Wu et al.'s survey *Recursive Self-Improvement in AI* (preprints.org, unverified ID) calls it a "paired probe", AIDE² calls it an "ignition test", and the Physical-AI RSI survey (2610.29677) calls it the R1/R2 links.
2. **Two-point swap tests now exist in several method papers, all of them weak.** RSIFlow (2610.21791) runs Meta0…Meta4 on a common initial task agent, from a single run with partial budget matching, and the result is non-monotone. FreeEvolve (2610.09197) compares a seed evolution skill against a meta-evolved one on a fresh worker at the same budget: +6.9 pts, with a "no-data rewrite" control. Skill∞ (2610.35434) compares seed against refined meta-skill: 55.9 vs 60.7 on SkillsBench. MetaRSI (2609.06396) has the original improver add +4.9 and the meta-updated one +7.2 from the same released system. Others: MASS (2610.12176, gen-0 vs gen-1 as workflow optimizer), SpecRSI (2610.16491), Sivia (2609.33386), and the older STOP (mixed result). AIDE²'s ignition test is AIDE₄₇ vs **AIDE_human** (not vs AIDE₀) as outer-loop driver, with 3 seeds. It was **inconclusive**: 0.780 vs 0.782, and the authors call more seeds "prohibitively costly".
3. **Evolving-vs-frozen evidence is mixed and statistically thin.**
   - VERSE (2610.02616): letting the optimizer self-evolve **without execution verification made it worse** (validation picked the initial harness, 31.8 vs 34.0 blank). With verification it gave the best result (41.1), but from **one evolution run per row**, and in several host/benchmark cells the self-evolve increment is within noise or negative.
   - Flex-RSI (2611.08946): improver on vs off, 6 of 7 pairs improve, single seed per pair.
   - Hyperagents compounding: 0.640 vs 0.610, not significant (already in the reading list).
4. **Measurement papers now crowd the confound space.** Each of these takes part of Dan's "confound battery":
   - Harness-Delta Attribution (2610.22466) splits harness gains into overfitting, test-time scaling and a residual.
   - Beneath the Surface (2611.07624) re-runs AHE, HyperAgents, A-Evolve and GEPA with 8-pass evaluation, coverage flow and transfer.
   - Winner's Curse (2610.09239).
   - RSI Tax (2611.13524): a bounded-note baseline matches Meta-Harness, EvoSkill and RRSI at 65–71% fewer tokens.
   - Third Eye (2611.12988): does a self-update stay useful after the *next* update?
   - Does Failure Feedback Direct Self-Modification? (2610.14662): no target-specific repair advantage across 145 DGM modifications.
   - Agent Self-Improvement with Frozen Weights (2611.11342): the trial-and-keep process alone gives about 60% of the total gain.
   - REUSE (2609.33180), PACE (2606.08106) and TRUST (2611.10787) are statistical acceptance gates.

   All of them work at the **task/product level**. None applies the confound battery to the **improver level**.
5. **Plasticity has arrived, but only at the object level.** Agent Plasticity (2610.08902) measures held-out gain per $ for *fixed-weight agents improving their own artifacts* across 20–40 checkpoints. It separates endpoint from acquisition efficiency and diagnoses artifact reuse. It does **not** measure whether the *improver* stays plastic, does not study plasticity loss over generations, and leaves "post-training for plasticity" as future work.
6. **Theory for a complexity ceiling has appeared.**
   - *Audit the Scaffold, Not the Checkpoint* (2609.34924) states a stationarity dichotomy: self-modification hits strict diminishing returns whenever the reachable edit set stays fixed, and scaffold rewriting can push the ceiling out.
   - *Adapting the Process of Adaptation* (2611.07048) models Darwinian, Lamarckian and agent-directed evolution, and shows that extra search accelerates convergence to a misspecified target.
   - *Verification and Self-Improvement: Foundations and Limits* (2610.10611) separates search gains from changes in accepted-task coverage.

   None tests a ceiling empirically on a meta-agent.

**Bottom line.** The *conceptual* framing of Dan's improver-scaling and ignition ideas has been published (Oct 2026). Two-checkpoint swap tests appear as side experiments in about 8 method papers, mostly single-run. **What is still missing:**
- a multi-checkpoint improver-scaling curve with seeds and confidence intervals;
- an improver × state factorial that actually resolves the Proposition-3 confound;
- procedure-vs-memory (frozen-knowledge) controls for meta-agents;
- compute-matched baselines at the meta level;
- a meta-level winner's-curse analysis;
- meta-agent plasticity, or its loss, over generations.

---

## 2. Key papers

| # | Paper | ID | Date | Finding | Critique / limitation / future work | Relevance |
|---|---|---|---|---|---|---|
| 1 | Auditing the Product, Not the Recursion | 2610.27610 | 2026-10-20 | Census of 114 studies: only 4/26 better-improver claims use a same-start matched-budget swap test; 22% audit ≥2 reruns. Proposition 3: improver and state are not identified from trajectories. | Meta-research only, no new experiments. LLM-assisted coding. Calls for swap tests over "a declared distribution of starting states with paired or randomized assignment", and for compounding (C5) tests "over a stated horizon". | **Most important.** Raises the bar and hands Dan the estimand. A paper that *runs* the factorial swap test at scale answers it directly. Threat: makes a 2-checkpoint test look routine. |
| 2 | RSIFlow | 2610.21791 | 2026-10-15 | Meta-agent evolves its own harness (skills/principles). Table 3: Meta0..Meta4 on a common initial task agent; Meta4 beats Meta0 by +20.6 (Code) and +9.5 (QA); non-monotone (Meta1 < Meta0 on Code; Meta3 best on Tool). | Single run, no CIs. Budget "partial" per the audit. Meta-agent is GPT-5.6-Sol driving a Qwen3-4B task agent, so model, harness and artifacts change together. No compute-matched baseline. | **Closest scoop of improver scaling (#1).** Still leaves seeds, a curve with more than 5 points, matched budget, and attribution. |
| 3 | FreeEvolve | 2610.09197 | 2026-10-06 | Evolver controls its own campaign. Meta-evolved skill vs seed on a fresh worker at the same budget: +6.9 pts mean. A *no-data rewrite* control (LLM rewrites the skill without execution evidence) is worse in every environment. Transfer across environments is uneven. | Two endpoints only (seed vs final), no curve. Large meta budget (24 h). Seeds not reported per meta-run. Single-source skills hurt the TB2.1 mean. | Clean swap test plus a "rewrite placebo". Threatens the "editable beats frozen" framing, not a scaling curve. |
| 4 | Skill∞ | 2610.35434 | 2026-10-26 | Meta-skill optimizes task skills and itself. Refined vs seed meta-skill: 60.7 vs 55.9 on SkillsBench (5 runs). Self-improvement costs about 11 task-skill optimizations, amortized. | Two checkpoints only. Meta-score uses a small set of skill-optimization tasks, so a meta-level winner's curse is possible. No budget-matched "spend the meta budget on more inner search" baseline. | Gives the **amortization** framing (break-even uses). Meta-skill is a cheap, portable object for Dan's experiments. |
| 5 | MetaRSI / RSI² | 2609.06396 | 2026-09-06 | Composes data, harness and model RSI operators under a meta-policy. Same-start: original improver +4.9 vs meta-updated +7.2 (per 2610.27610). | Audit notes a scope gap: claims open-ended science, tests executable coding and closed-form science. | One of the 4 "strict" swap tests. Single comparison. |
| 6 | AIDE² (Weco) | 2609.26457 | 2026-09-22 | 8-day RSI of a research agent; gains transfer to 4 held-out benchmarks. Ignition test: AIDE₄₇ vs AIDE_human as outer-loop driver, 3 seeds × 50 steps, endpoints 0.780 vs 0.782, **inconclusive**. | Compares against a *human* agent rather than AIDE₀, so it is not a clean earlier-vs-later swap. Authors: more seeds plus external evaluation are "prohibitively costly". Accepted components may be unused artifacts. | Explicit open problem. Dan's ignition idea is the **execution** of this test with power. |
| 7 | VERSE | 2610.02616 | 2026-10-02 | Optimizer self-evolution **hurts without execution verification** (round 0 selected; 31.8 test) and helps with it (41.1). Self-evolving optimizers build attribution, verification and audit tools; they add regression checks in 34/81 drafts vs 2/89 for fixed optimizers. | One evolution run per row. In Table 6 the self-evolve increment is negative in some cells (Meta-Harness SWE-rebench "Last" 28.4 < 33.0). No swap test of optimizer checkpoints. | Supplies a **moderator** (verification access) for improver-scaling curves. Suggests the hypothesis "meta-improvement requires grounded evidence". |
| 8 | Agent Plasticity | 2610.08902 | 2026-10-06 | Plasticity = held-out gain per $ of learning cost. Measured over 20–40 checkpoints in chess, Go, Hex and NetHack for frozen frontier models with self-built artifacts. Endpoint ≠ acquisition efficiency. Low-plasticity agents fail to *reuse* artifacts. | Object-level only. Observational artifact diagnostics. Depends on pricing and saturation fit. Future work: train *for* plasticity. Does not cover meta-agents or plasticity loss. | Base for **meta-agent plasticity** (Dan's interest). The meta-level version is unclaimed. |
| 9 | Beneath the Surface | 2611.07624 | 2026-11-05 | Re-runs AHE, HyperAgents, A-Evolve and GEPA (GPT-5.4; MiniSWE/Codex; τ³-Banking/TB2), 8 passes. All 16 champions gain and lose tasks. **The highest-scoring parent does not yield the largest further improvement** (native select is below random on τ³). Transfer is selective. | Single evolution run per lineage. Champion-level only, so it does not test whether the HyperAgents meta-agent improved. | Effectively a HyperAgents partial replication: modest gains (+1.8 to +6.6 Avg@8). Supports the HGM "metaproductivity ≠ score" premise. |
| 10 | Do Agent Optimizers Compound? | 2607.14004 | 2026-07-15 | Two-phase continual evaluation on TB2.0: GEPA transfers below baseline; Meta-Harness transfers but **fails to improve further** with a second budget; RELAI-VCL keeps improving. | Two phases only, an industry method (RELAI) is the winner, single benchmark. | Product-level compounding: "optimizer gains stall in round 2". Motivates the plasticity-loss hypothesis. |
| 11 | Winner's Curse in Self-Improvement Loops | 2610.09239 | 2026-10-07 | Keep-if-better with 16 selection items overstates by 13–20 pts (1–5 with 256). Most proposals after the first are harmful. Tested acceptance rules do not beat greedy. | Instruction-level loops on small Qwen models, QA tasks only. | Threatens a confound battery at task level. **The meta level (selecting improvers by meta-score) is untouched.** |
| 12 | Harness-Delta Attribution | 2610.22466 | 2026-10-15 | Splits evolution gain into artifact overfitting, test-time scaling and a residual (upper bound on generalizable gain). In 21 significant settings, a large share is overfitting or TTS. | Task-level harness gains, not improver gains. | Template for a **meta-level** compute/search/meta-learning split. Scoops the task-level version. |
| 13 | RSI Tax | 2611.13524 | 2026-11-10 | Bounded-note baseline matches Meta-Harness, EvoSkill and RRSI with 65.6–71% fewer total tokens. Verification is 75–94% of the RSI tax. Many artifacts are never reused. | Continual streams. Product level. | Cost accounting that meta-level work must adopt. A strong "cheap baseline". |
| 14 | Physical-AI RSI survey | 2610.29677 | 2026-10-21 | Formal R1 (repair knowledge improves the improver) and R2 (the improved improver improves itself, vs a **frozen-knowledge control** revising the same parent). Proposes a two-round test. Finds no physical study isolates a recursive gain. | Survey, no experiments. Robot cost estimates. | **Gives the procedure-vs-memory control design** (frozen-knowledge vs acquired-knowledge) that no digital paper has run at scale. |
| 15 | Third Eye | 2611.12988 | 2026-11-08 | 64 runs × 3 candidates × 3 continuations: immediate-selected utility ≈ 0, continuation lowers it by 1.78 pts. | Weight-update RSI on 3–8B models, math and code. | "Usefulness for the next update" is a metaproductivity-like quantity: a second-order metric. |
| 16 | Audit the Scaffold, Not the Checkpoint | 2609.34924 | 2026-09-28 | Stationarity dichotomy: diminishing returns are strict when the reachable edit set is fixed. Scaffold rewriting expands it, with an eventual ceiling. Best-of-k "width buys rate, not budget". | Theory plus small worker-pool experiments. | Formal basis for a **complexity-ceiling** test: measure the reachable-set expansion of meta-agents. |
| 17 | Does Failure Feedback Direct Self-Modification? | 2610.14662 | 2026-10-10 | Across 145 public DGM modifications: no target-specific repair advantage. In controlled small-model studies, target feedback helps Qwen3-4B repair (+20 pp) but not held-out accuracy. | DGM logs plus small open models. | Calls DGM-lineage "directed improvement" into question, and with it any claim that the improver learns from failures. |
| 18 | Agent Self-Improvement with Frozen Weights | 2611.11342 | 2026-11-08 | Failure-trace notes add +4.0 pp over matched search without traces; the search process alone gives about 60% of total gain; no gain in full conversations. | Single 9B model, customer-service domain. | Direct template for the **search-vs-learning** split, at the object level. |
| 19 | Rethinking Self-Evolving Agents (OEO) | 2608.09629 | 2026-08-10 | A frontier optimizer composing its own process online beats SkillOpt and GEPA in 12/14, with a zero-interaction one-shot control. A capability boundary exists for weaker models. | Single frontier optimizer (GPT-5.5). | Shows the improver's *base capability* dominates. A confound for improver scaling across model tiers. |
| 20 | Predict Before You Modify (IDM-Hyperagents) | 2610.13991 | 2026-09-26 | Editable improvement-dynamics model on top of HyperAgents, up to 46% higher; holds under matched token budgets. | Method paper. | Matched-token comparison against HyperAgents. A candidate improver family. |
| 21 | Hindsight Meta-Experience Distillation (HMED) | 2610.07979 | 2026-10-06 | Re-executes incumbent and revised meta-skills from the same restored state to isolate a revision's effect. | Method paper. | Uses a mini *swap test* as a training signal, a sign the community is converging on paired same-state comparisons. |
| 22 | Flex-RSI | 2611.08946 | 2026-11-06 | 18 models × 24 vision/robotics tasks. Improver on vs off: 6/7 pairs gain. A stronger model's harness helps a weaker model in only 2/4. Gains arrive by round 3–4. | One run per model-task. No swap of improver checkpoints. | Benchmark crowding. "Gains arrive early" means saturation, which supports the plasticity-loss framing. |
| 23 | Skill/process meta-evolution: MetaSkill-Evolve | 2607.05297 | 2026-07-06 | Two-timescale evolution of task skill and meta-skill. Evolving vs frozen meta-skill (partial budget per audit). | Branch-bound meta-skill. | Another improver family. |
| 24 | Speculative RSI (SpecRSI) | 2610.16491 | 2026-10-12 | Outer loop trains the improver (GVO). Original vs updated improver on the same artifact. | Partial budget matching. | Swap-test instance. Weight-level improver. |
| 25 | MASS (RSI via multi-agent self-supervision) | 2610.12176 | 2026-10-08 | Gen-0 vs gen-1 model as workflow optimizer on the same tasks (matched, per audit). | Two cycles only. | Swap-test instance (weights). |
| 26 | Generalized Agent Iteration (GAI) | 2609.13406 | 2026-09-11 | Formal framework with two dials (is the improver inside the agent; is the standard external). | Theory. | Framing for related work. |
| 27 | Adapting the Process of Adaptation | 2611.07048 | 2026-11-04 | Theory: agent-directed evolution adapts faster, but extra search speeds convergence to a misspecified objective. | Theory. | Predicts a meta-level Goodhart curve. |
| 28 | Most Self-Improving LLM-Agent Papers Fail ≥2 Evaluation Traps (41-paper methods audit) | alphaXiv-only (2609.self-improving-llm-agent-evaluation-audits) | 2026-09-25 | Methods audit of 41 papers. | Not deep-read. | Second meta-research audit; adds to the crowding. |
| 29 | When, Where, and How Experience Becomes Structure | alphaXiv-only (2611.recursive-self-improvement-llm-agents) | 2026-11-04 | Mechanisms of how experience turns into structure in RSI LLM agents. | Not deep-read. | Possible overlap with procedure-vs-memory. **Check before committing.** |

Anchors already in the reading list and used here: Gödel Agent 2410.04444, Hyperagents 2603.19461, HGM 2510.21614, Metaⁿ 2608.24735, STOP 2310.02304 (confirmed via alphaXiv), Rethinking Eval 2607.12227, Harness Updating ≠ Benefit 2605.30621.

---

## 3. New since the reading list (not in reading_list.md / extra_leads.md)

Auditing the Product (2610.27610); RSIFlow (2610.21791); FreeEvolve (2610.09197); Skill∞ (2610.35434); MetaRSI (2609.06396); VERSE (2610.02616); Beneath the Surface (2611.07624); Do Agent Optimizers Compound? (2607.14004); Harness-Delta Attribution (2610.22466); RSI Tax (2611.13524); Physical-AI RSI (2610.29677); Third Eye (2611.12988); Audit the Scaffold (2609.34924); Does Failure Feedback Direct Self-Modification? (2610.14662); Agent Self-Improvement with Frozen Weights (2611.11342); OEO (2608.09629); HMED (2610.07979); Flex-RSI (2611.08946); SpecRSI (2610.16491); MASS (2610.12176); GAI (2609.13406); Adapting the Process of Adaptation (2611.07048); Trustworthy RSI / TRUST (2611.10787); REUSE (2609.33180); PACE (2606.08106); Verification and Self-Improvement limits (2610.10611); Artifact-to-Capability (2611.04952); Beyond Endpoint Performance / EvoPathBench (2609.24663); SelfImprove-Bench (2611.02797); RSI-Env (2611.01736); Sivia (2609.33386); RSIHub (2610.32253); Gödel Forest (2609.36675); Mendel Gödel Machine (2608.07645); HGM-Dual (2610.30042); RSI on a Budget (2610.29121); Fragility of Self-Improving Agents (2608.18066).

Already listed as leads but now verified with IDs: Agent Plasticity = 2610.08902; Winner's Curse = 2610.09239; MetaSkill-Evolve = 2607.05297; Physical AI RSI = 2610.29677; Dream-RSI = 2609.14858; Self-Evolving Coding Agents = 2608.03392; SBCO = 2608.10157; Beyond Endpoint Performance = 2609.24663; RSI of research agents = 2609.26457 (AIDE²).

---

## 4. Critiques and limitations that recur across papers

1. **Single runs.** RSIFlow, VERSE (one evolution run per row), Flex-RSI (one run per model-task), Beneath the Surface (one lineage per method), MetaRSI and FreeEvolve (meta-run seeds unreported). The audit: only 22% of RSI studies rerun the full loop at least twice. AIDE² says the seeds needed for an ignition test are "prohibitively costly".
2. **Two endpoints instead of a curve.** Every swap test compares seed vs final (FreeEvolve, Skill∞, MetaRSI, MASS) or a handful of non-monotone checkpoints (RSIFlow). Nobody estimates a *slope* of improver gain against generation, which is what C5 compounding requires.
3. **State and improver are confounded.** Most "the improver got better" evidence comes from trajectories or evolving-vs-frozen runs (Hyperagents, DGM, VERSE, Flex-RSI, MetaSkill-Evolve), which Proposition 3 shows cannot identify an improver effect. The Hyperagents transfer moves the task agent along with the meta agent.
4. **No budget-matched meta-level comparator.** Meta-evolution budgets (FreeEvolve 24 h; Skill∞ about 11 optimizations) are rarely matched against "spend the same tokens on more inner-loop search, more seeds, or best-of-N with the seed improver". RSI Tax and Rethinking Eval show this kind of baseline often wins at the task level.
5. **Memory and information confound.** Improvers accumulate task-specific notes ("training audits", lessons, skills: VERSE Fig. 3; RSIFlow skill library; Agent Plasticity artifacts). A "better improver" may just know more about *this* benchmark. FreeEvolve's no-data rewrite control removes execution evidence but not task information. Nobody runs a frozen-knowledge control (Physical-AI R2) or a memory-transplant control.
6. **Noisy selection at the meta level.** Meta-scores are averages over very few meta-tasks (each one a full campaign), so the winner's curse should be severe. Yet Winner's Curse, PACE, REUSE and TRUST address only task-level acceptance.
7. **The improver's base model dominates.** RSIFlow's backbone sensitivity (GPT-5.6-Sol +24 vs DeepSeek +4.9), OEO's capability boundary, and the reading-list point that open models reportedly cannot drive Hyperagents. Improver-scaling results may not hold across tiers.
8. **Gains are small and redistributive.** Beneath the Surface finds that champions both gain and lose tasks. Harness-Delta finds much of the gain is overfitting or TTS. Failure-Feedback finds DGM modifications are not directed.
9. **Saturation and early gains.** Flex-RSI: 90% of the gain by round 3–4. Do Agent Optimizers Compound: Meta-Harness stalls in round 2. AIDE²: non-monotone checkpoints. This points to diminishing returns (complexity ceiling or plasticity loss) that nobody measures at the meta level.

---

## 5. Open gaps

- **G1. An improver-scaling curve with power.** Improver gain on fresh, held-out starting states as a function of meta-generation, for at least 4 checkpoints, at least 3 seeds per arm, at matched budget, across at least 2 improver families. Neither the audit nor RSIFlow, FreeEvolve or Skill∞ provides this.
- **G2. The improver × state factorial.** Resolve Proposition 3 empirically: cross improver checkpoints I_0..I_k with solver states S_0..S_k (including "easy" and "near-ceiling" states) and estimate main effects plus the interaction (headroom). Nobody has done this.
- **G3. Procedure vs memory.** Split the improver gain into (a) generic procedure (prompts, tools, workflow), (b) accumulated task-distribution knowledge (notes, lessons, skill library), and (c) model capability. Controls: frozen-knowledge (R2), memory transplant into the seed improver, procedure transplant with memory wiped, and cross-domain swap tests. The design exists only on paper (Physical-AI survey).
- **G4. Compute, search, meta-learning at the meta level.** A Harness-Delta-style split for improver gains: (i) extra compute (budget-matched best-of-N or longer inner search with I_0), (ii) search structure (archive or gating changes alone), (iii) residual meta-learning.
- **G5. Meta-level winner's curse.** How much do meta-scores overstate improver quality, and how many meta-tasks or seeds are needed? This needs a preregistered meta-level held-out estimator.
- **G6. Meta-agent plasticity and its loss.** Improver-gain per $ across generations. Does the self-modifying improver lose plasticity (bloat, note accumulation, code complexity; Gödel Agent's complexity ceiling; "Audit the Scaffold" theory)? Can pruning or consolidation interventions restore it?
- **G7. Do metaproductivity estimates predict swap-test improver gain?** HGM's CMP, Beneath the Surface's "parent improvement potential", and Third Eye's continuation utility are all proxies for "this agent produces good descendants". Nobody has validated them against a controlled swap test.
- **G8. Moderators of meta-improvement.** VERSE suggests that execution verification is necessary. Other candidates are evaluator noise, task diversity of the meta-training set, and improver base-model tier. No factorial study exists.

---

## 6. Threats to Dan's ideas

| Dan's idea | Status | Why |
|---|---|---|
| #1 Improver scaling (checkpoints 0/10/20/40, imp@k, fixed compute) | **Alive but pressured. Must be upgraded.** | Two-point and five-point same-start comparisons exist (RSIFlow Table 3 with Meta0–4; FreeEvolve; Skill∞; MetaRSI; MASS). The audit paper names the design and states the estimand. Still open: seeds and CIs, a slope or C5 test, budget matching, the factorial (G2), and attribution (G3/G4). A reviewer will cite 2610.27610 and RSIFlow, so the paper must deliver the *rigorous, multi-family, factorial* version. "Does a later checkpoint help?" is no longer enough. |
| Ignition test across Gödel Agent / AIDE² / Hyperagents / Metaⁿ | **Alive as an execution contribution.** | AIDE² is inconclusive (3 seeds; compared against a human agent, not AIDE₀). VERSE and Flex-RSI are single-seed evolving-vs-frozen. The framing is published by Weco and the audit, so the contribution is a powered, preregistered execution plus the factorial. Risk: Weco or Hyperagents authors publish a powered ignition test first. |
| #2 Confound battery (matched budget, placebo, length, seeds, OOD) | **Largely scooped as a standalone (task level).** | Harness-Delta Attribution, Beneath the Surface, Winner's Curse, RSI Tax, Rethinking Eval, Failure-Feedback directedness, Frozen-Weights failure traces, Third Eye, plus two meta-research audits. It survives only if moved to the *improver level* (G4/G5). |
| Hyperagents meta-transfer replication | **Partially scooped / narrowed.** | Beneath the Surface re-runs HyperAgents (modest gains, mixed transfer). The audit flags the transfer design as non-same-start. A targeted replication of imp@50 = 0.630 with a proper same-start swap is still unpublished but is workshop-sized on its own. Fold it in as one improver family in G1/G2. |
| Procedure vs memory arm | **Open. Strongest novelty.** | Only proposed (Physical-AI R1/R2 frozen-knowledge control). FreeEvolve's no-data rewrite is adjacent. *Check 2611.recursive-self-improvement-llm-agents (alphaXiv-only) before committing.* |
| Compute vs search vs meta-learning | **Open at the meta level, scooped at the task level** | Harness-Delta Attribution (task level) and Frozen-Weights (about 60% of the gain is search). |
| Gödel Agent complexity ceiling | **Open empirically. Theory exists** | 2609.34924 gives the stationarity dichotomy. AIDE² shows that context-compression rewrites are what the loop discovers (a symptom of a ceiling). No meta-agent study measures improver complexity against improver gain. |
| Meta-agent plasticity | **Open** | Agent Plasticity (2610.08902) is object-level only. Its future work is training for plasticity, not meta-level measurement. |

---

## 7. Proposed ideas (new, grounded in the gaps)

### Idea A: "Is it the improver or the state? A factorial swap test for recursive self-improvement" (G1+G2+G4)
- **Core question.** When a self-modifying meta-agent appears to get better at improving, how much of the later gain is due to the improver, how much to the solver state it happens to be applied to (headroom, ease of improvement), and how much to extra compute?
- **Claim.** In at least 3 meta-agent families, most of the apparent across-generation gain in "improvement rate" is a *state effect*, not an improver effect. Once improver × state is crossed at matched budget, improver gains are small, appear early (by generation ~5–10), and do not compound (C5 fails).
  - Either outcome is publishable. If improver effects are large and growing, that is the first powered evidence of compounding RSI.
- **Key experiment.**
  - Pick 3 families: a Hyperagents-style self-editing meta-agent, a FreeEvolve/Skill∞-style meta-skill, and an AIDE-style research-agent improver.
  - Run each for 40 meta-generations. Checkpoint improvers I_{0,5,10,20,40} and solver states S_{0,5,10,20,40}.
  - Run the full 5×5 cross, 3–5 seeds per cell, a fixed inner budget, and fresh held-out audit tasks.
  - Add a budget-matched comparator: I_0 given the extra tokens that the meta-evolution consumed, as best-of-N plus a longer inner search.
  - Fit a two-way model with an interaction (headroom).
  - Report the improver main effect per generation (the improver-scaling curve), the state effect, the interaction, and the meta-level RSI tax / break-even uses.
  - Use cheap models (Haiku / GPT-mini / Qwen3.5-9B as executor) on τ-style procedural tasks or SkillsBench subsets.
- **Why new.** The audit (2610.27610) proves the confound and asks for this design but runs none. Existing swap tests are 2-point, single-run, one family, and none crosses states.
- **Closest prior.** 2610.27610 (Proposition 3, swap test); RSIFlow 2610.21791 (Meta0–4 on one state); FreeEvolve 2610.09197; AIDE² 2609.26457 (ignition).
- **Estimated cost.** About $600–1,500 API with small models (25 cells × 4 seeds × 3 families × small inner budgets). A pilot on one family and a 3×3 grid costs under $60.

### Idea B: "Procedure or Memory? Frozen-knowledge controls for meta-agents" (G3)
- **Core question.** When an improver gets better, is it because its *procedure* improved (how it diagnoses, tests and edits), or because it accumulated *knowledge about the task distribution* (notes, lessons, skill library) that a better-informed but procedurally identical improver would also exploit?
- **Claim.** Most measured improver gain comes from accumulated task-specific knowledge, not procedure.
  - The memory-stripped evolved procedure gives at most ~30% of the improver gain.
  - The evolved memory transplanted into the seed procedure recovers most of it.
  - Cross-domain swap tests (improver evolved on domain X, applied to domain Y) erase it.
  - So "learning to improve" in current meta-agents is mostly "learning the benchmark".
- **Key experiment.** For each evolved improver I_t, build 4 arms at a fixed state and budget:
  - (i) full I_t;
  - (ii) the procedure of I_t with memory/notes wiped;
  - (iii) I_0 plus the memory of I_t (transplant);
  - (iv) I_0 with an equal-length *placebo* memory.

  Run in-domain and cross-domain. Implement the Physical-AI R2 control (self-revision with acquired vs frozen knowledge) in a digital setting. Measure across generations to see whether the procedure share grows.
- **Why new.** This was proposed only as a design for robots (2610.29677 §4.4). FreeEvolve's no-data rewrite and VERSE's artifact audit hint at it. No digital meta-agent study separates the two.
- **Closest prior.** 2610.29677; FreeEvolve 2610.09197; VERSE 2610.02616 (self-built notes and audits); Agent Plasticity 2610.08902 (artifact reuse diagnostics).
- **Estimated cost.** $300–800. It reuses Idea A's checkpoints, so A and B are best written as one paper.

### Idea C: "Meta-agent plasticity: do self-improvers lose the ability to improve?" (G6, with the complexity ceiling)
- **Core question.** Does a self-modifying improver's *meta-plasticity* (improver-gain per $ on fresh states) decline over generations as its code, notes and prompts grow? Do interventions borrowed from loss-of-plasticity research restore it?
- **Claim.** Self-modifying improvers show meta-level plasticity loss. Improver gain peaks within ~10 generations and then falls, tracking improver complexity (tokens, number of tools, rule count): this is the empirical form of Gödel Agent's complexity ceiling.
  - Periodic consolidation/pruning ("sleep", reset-and-distill, as in 2611.02216) restores plasticity at matched compute.
  - A null result (no decline) would refute the ceiling hypothesis, which is also informative.
- **Key experiment.**
  - Measure meta-plasticity P_meta(t), defined as the swap-test improver gain per $, at checkpoints along long runs (60+ generations) for 2 families.
  - Regress it on complexity metrics.
  - Intervention arms at matched budget: periodic pruning to a token budget, distill-and-reset of notes, and a "shrink-only" self-edit round.
  - Test the "Audit the Scaffold" prediction by measuring reachable-edit-set expansion (new tool types).
- **Why new.** Agent Plasticity is object-level. The complexity ceiling has theory (2609.34924) but no meta-agent measurement. "Do Agent Optimizers Compound?" and Flex-RSI show product-level stalls only.
- **Closest prior.** Agent Plasticity 2610.08902; Audit the Scaffold 2609.34924; Do Agent Optimizers Compound? 2607.14004; Gödel Agent 2410.04444; Do agents need to sleep? 2611.02216.
- **Estimated cost.** $500–1,500; long runs dominate.

### Idea D: "The meta-level winner's curse: how much do meta-scores overstate better improvers?" (G5+G7)
- **Core question.** Meta-evolution selects improvers on a handful of expensive meta-tasks. How large is the overstatement? Do metaproductivity proxies (HGM CMP, parent improvement potential, Third-Eye continuation utility) predict the true held-out improver gain?
- **Claim.** Meta-selection with fewer than about 10 meta-tasks overstates improver gains by more than 2× the true held-out swap-test gain. HGM-style metaproductivity correlates weakly (ρ < 0.3) with the swap-test improver effect. A meta-level held-out split plus paired swap scoring fixes most of the bias.
- **Key experiment.** Generate many candidate improver revisions per generation. Score each on its meta-selection tasks and on a large held-out meta-task pool via paired swap tests. Fit a winner's-curse model (as in 2610.09239) one level up. Compare against the CMP proxies.
- **Why new.** Winner's Curse, PACE, REUSE and TRUST work only at the task level. HGM's CMP has never been validated against a controlled improver estimand.
- **Closest prior.** 2610.09239; HGM 2510.21614; Beneath the Surface 2611.07624 (parent potential); Third Eye 2611.12988.
- **Estimated cost.** $400–1,000. This is a strong *section* inside A rather than a standalone paper.

### Idea E (moderator study, smaller): "When can meta-agents learn to improve? Verification, feedback granularity and base-model tier"
- **Core question.** Under what conditions does improver self-modification produce positive swap-test gains?
- **Claim.** Meta-improvement is positive only with execution-grounded verification and graded (non-binary) feedback, and only above a base-model capability threshold. Below it, self-modification harms (VERSE without verification; Flex-RSI open-weight binary feedback).
- **Key experiment.** A 2×2×3 factorial (verification on/off × binary/graded feedback × 3 model tiers) on the swap-test improver gain at matched budget, with seeds.
- **Closest prior.** VERSE 2610.02616; Flex-RSI 2611.08946; OEO 2608.09629.
- **Estimated cost.** $400–900.

**Recommendation.** Merge A + B (+ D as a section) into one NeurIPS paper: *"Measuring Recursive Self-Improvement: a powered factorial swap test separating improver, state, compute, and memory."* It answers the published call in 2610.27610 and directly extends the mentor's Gödel Agent line. Its claim is surprising either way: improver gains are mostly state or memory effects, or they genuinely compound. Idea C (meta-plasticity) is the best second paper, or the "long-horizon" section if budget allows. Before committing: (1) read 2611.recursive-self-improvement-llm-agents and the 41-paper methods audit (both alphaXiv-only, not deep-read); (2) re-run an S2 forward-citation check on 2610.27610 in Jan–Mar 2027, because a powered swap-test paper is the obvious follow-up for the auditing authors or for Weco.

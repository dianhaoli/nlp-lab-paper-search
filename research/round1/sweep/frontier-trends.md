# Frontier trends sweep (Sep–Nov 2026): what is hot next to self-improving agents

*Sweep date: 2026-11-12. Sources: the alphaXiv hot feed (30-day), alphaXiv discover (recency, popular), forward citations in Semantic Scholar of 5 anchors (Agent Plasticity 2610.08902, AIDE² 2609.26457, Rethinking Eval 2607.12227, Hyperagents 2603.19461, The Last AI Built by Humans 2609.11873), plus citation files that sibling agents had already pulled into the scratchpad (Gödel Agent, Harness Updating ≠ Benefit, RRSI, etc.). I deep-read 12 papers (full report or targeted PDF queries). Every arXiv ID below appeared in a tool result. Sibling sweep files (eval-rigor, plasticity, improver-scaling, harness-evolution, curriculum, world-models-xunjian) already cover most of the RSI-evaluation "skeptic" papers, so this file covers what they missed: autoresearch search dynamics, research world models, continual-learning streams, RSI economics, and human co-improvement.*

---

## 1. Summary of the landscape (what is hot now)

**"RSI" became a named subfield in Sep–Oct 2026, and it is now in a land-grab phase.** In 8 weeks the field produced:
- **Many RSI benchmarks and environments.** Examples: RSIGym 2610.10310 (93 votes), RSI-Forge 2610.09426 (Scale), RSI-Env 2611.01736, Flex-RSI 2611.08946, SelfImprove-Bench 2611.02797, RSI-Dojo 2611.03698, ParetoRSI-Gym 2610.34386, RSI-Index 2609.rsi-index, RSI Arena 2610.33624, Continual Learning Exam 2610.33956. **Writing another benchmark is saturated.**
- **Large position papers and surveys.** "The Last AI Built by Humans" 2609.11873 is the most-viewed RSI paper (352 votes, ~12k views). It proposes an L1–L5 autonomy ladder and names *autonomy attribution*, *safe inheritance* and *reliable verification* as the three open problems. Others: the Trustworthy-RSI survey 2610.35193 and "Two Perspectives on RSI" 2610.14625.
- **Skeptic and accounting papers.** These are the strongest trend for Dan's lane, and sibling files cover them. Examples: the RSI Tax 2611.13524, When Is Enough Enough 2610.04756, Auditing the Product 2610.27610, Harness-Delta Attribution 2610.22466, Elicitation or Expansion 2610.27889 and Selection Is Not Learning 2610.22029.

**Five adjacent trends that are hot but not yet saturated:**

1. **Autoresearch search dynamics: idea basins, forgetting and false floors.** This is the hottest new mechanism question.
   - Fork-and-Flush 2610.07447 (MSR) shows that independent runs sit in "idea basins".
   - Scaling Discovery Requires Forgetting 2610.20563 (Berkeley; 90 votes, 2k views) finds that agents anchor on their own history: edit sizes shrink and reasoning tokens drop to a third. Wiping the session state beats parallel agents at matched dollars.
   - A rebuttal two weeks later, "Scaling Discovery Does *Not* Require Forgetting" (2611.verified-archives-…), notes that Tan et al. never separated *conversation* from *scored archive*. A verified shared archive with short conversations wins, and the authors name the "false floor" pathology (agents declare optimality and stop).
   - Related: Stateless Language Agents 2610.07625, the Interaction Tax 2608.23541 (communication homogenizes), and Beneath the Diff (algorithmic mode collapse) 2609.00077.
   - **All of this work is on single-objective optimization tasks (kernels, AtCoder, nanochat). None of it is on meta-agents that evolve harnesses and are scored on held-out tasks.**
2. **Research world models, or "predict before you run".**
   - "Can LLMs predict the outcomes of ML experiments?" 2610.22599: the best model reaches R²=0.49, every model has slope <1 (underestimates large effects), and there is no self-preference.
   - "Language Models as AI Research World Models" 2610.12235 (Amazon; 2,653 records, 171k H100-h): experimental records lift Spearman correlation from 0.51 to 0.77 and transfer across environments. Records matter more than model choice or reasoning effort.
   - Related: Predict Before You Modify 2610.13991, RSI on a Budget (scoring code edits before they run) 2610.29121, Rehearse 2607.27687, and LLM self-modeling (Anthropic co-authors) 2608.30980.
   - This connects directly to Xunjian's *world models* agenda. Nobody has measured whether a self-improver's forecast skill *improves across generations*.
3. **Continual learning for harness learners: the plasticity–stability tradeoff.**
   - Are Self-Evolving Agents Continual Learners? 2610.25858 (Google DeepMind/Mila) finds that GEPA, Meta-Harness and ReasoningBank trade stability for plasticity (r=−0.72). Global prompt edits interfere with unseen task groups and get overwritten.
   - Can Coding Agents Sustain Continual Evolution without Forgetting? 2611.03219 (Tsinghua): Meta-Harness on SWE-bench falls from 94% to 32% after further evolution on FeatureBench, driven mostly by verification and artifact conflicts.
   - Continual-ARC 2611.14505: harness learners do not beat a demo-cache floor and gain *nothing* on unseen tasks, while weight learning (DAgger) does.
   - Continual AutoResearch 2611.05108: persistent state speeds research up but does *not* improve final solutions. Its matched / shuffled / no-memory card ablation is a clean template.
   - Related: Recurrent Harness 2610.12668, HERD 2610.31401, Memory-Centric CL 2610.29357, Routed CL (Meta) 2610.38796.
4. **On-policy self-distillation (OPSD) to internalize experience.** Latent OPSD 2608.13040 (187 votes), Speculative Self-Distillation 2611.12016 (hot feed), Privileged Retrospective Distillation (Meta) 2610.27495, Off-Policy Merging Beats OPSD for CL 2610.05872, MDSKILL 2611.05313 and ASCENT 2610.05303. This is the default route for moving harness gains into weights, and it raises the stakes for Dan's cluster 3.
5. **The economics and macro-modeling of RSI.**
   - The Economics of RSI 2609.15802 (METR / Epoch / Stanford) argues that the key quantity is a product of elasticities. It wants ε_{R,C}, the research-effort elasticity to capability, estimated and tracked over time, and calibrates current returns at ~9% against a 15% threshold.
   - Related: GovAI's "What if automating AI R&D triggers an intelligence explosion?" 2609.36054 and "Efficiency vs Capability" 2610.33447; Oxford's Dynamics of Intelligence Explosions 2608.14426; Recursive Criticality 2609.00137.
   - **These models explicitly ask for empirical elasticities that micro-RSI experiments could supply. No ML paper has estimated one from a self-improvement loop.**

**Big-lab signals.**
- Google DeepMind co-authors the continual-learner paper.
- Meta FAIR (Weston) published "Aligning Autoresearch: Co-Improvement" 2610.38882. Agents running AutoBenchmark unaided produce near-saturated benchmarks (solvers score >80). Only *specific* human direction halves solver scores; a one-line intent barely helps.
- Microsoft Research published Fork-and-Flush.
- Sakana published The Agentic Scientist 2610.18933.
- Anthropic co-authors the self-modeling paper 2608.30980, and its kernel take-home is the community testbed.
- METR runs the economics paper. Lilian Weng's blog post "Harness engineering for self-improvement" (Jul 2026) is cited as a framing piece (blog, not verified as a paper).
- No OpenAI evaluation paper on RSI was found. Its GPT-5.6 post is cited for "research compute on internal coding inference grew 100×".

---

## 2. Key papers table

| # | Title | ID | Date | Key finding | Critique / limitation | Relevance |
|---|---|---|---|---|---|---|
| 1 | The RSI Tax: When RSI Is Not Worth the Cost | 2611.13524 | 2026-11-10 | A bounded-note baseline (SRN) matches or beats Meta-Harness, EvoSkill and RRSI on task streams, at 65–71% fewer total tokens. Verification is 75–94% of the RSI Tax, and 24–64% of the tax goes to artifacts never used later. | 2 models, 7 repo chains, one ordering, one run. The cost attribution is descriptive, not interventional. | Scoops "matched-cost" framing for Dan's confound battery. Its own gap is the interventional version. |
| 2 | Fork-and-Flush: Escaping Idea Basins in Autoresearch Agents | 2610.07447 | 2026-10-05 | Independent runs end in different basins (LLM-triplet t-STE maps). Fork plus fresh context at 33%/66% of budget scores 0.78 normalized vs 0.54 for best-of-4. Both components are needed. Variance peaks at intermediate difficulty (MAX-3-SAT). | 13 tasks, frontier closed models, min–max normalization, LLM-judge embeddings. A fixed schedule only: plateau triggers failed. | Method and metrics template (basin maps, matched budget) for meta-agent plasticity. |
| 3 | Scaling Discovery Requires Forgetting | 2610.20563 | 2026-10-14 | All scaling modes plateau because agents anchor: edit lines fall 150→30 and thinking tokens 28k→7k. Forgetting beats parallel agents by up to 15% at matched dollars. Forgetting from the same plateaued state beats continuing at ~half the cost. | 4 tasks, 1 model (DeepSeek V4.1 Flash), 3 seeds. Conversation and archive are confounded. "When to forget" is left open. | Core evidence for "loss of plasticity through context". Not yet tested on meta-agents. |
| 4 | Scaling Discovery Does Not Require Forgetting: Verified Shared Archives | 2611.verified-archives-multi-agent-search | 2026-11-03 | Separates conversation from archive. Keeping every scored attempt and resetting only the conversation wins (914 vs 948 cycles). It identifies the **false floor** (7 of 12 CORAL agents declare optimality and stop). | One task for the main comparison, 3 runs, ~$842/run, a single small lab. | Shows the confound is live. A factorial on meta-agents is open. |
| 5 | Are Self-Evolving Agents Continual Learners? | 2610.25858 | 2026-10-19 | GEPA, Meta-Harness and ReasoningBank on drifting streams show a plasticity–stability tradeoff (r=−0.72). Self-modification causes pre-block interference and overwriting, and retrieval collides when workflows conflict. | Gemini 3.6 Flash only. 4 streams of 50-task blocks. No meta-level (improver) plasticity. | Partly scoops "plasticity for harness learners". Leaves "plasticity of the improver" open. |
| 6 | Can Coding Agents Sustain Continual Evolution without Forgetting? | 2611.03219 | 2026-11-02 | Continual harness evolution causes catastrophic forgetting (Meta-Harness on SWE: 94→32%). 87% of pass→fail cases are verification or artifact conflicts. | Small samples (50/50/30), 1 model, temperature 0, no mitigation. | Same as #5. Supports "harness edits are task-specific". |
| 7 | Continual-ARC: Measuring Skill Acquisition over a Working Lifetime | 2611.14505 | 2026-11-10 | Fresh-instance re-serving separates memorization (S_seen) from generalization (S_held). Harness learners barely beat a demo cache and gain 0 on unseen tasks. DAgger weight learning gains +0.29 on S_held. | Small models, ARC-AGI-1 track only, single author. | Template for a "memorize vs generalize" split of harness-evolution gains. Supports cluster 3. |
| 8 | From AutoResearch to Continual AutoResearch: Do Agents Learn Across Tasks? | 2611.05108 | 2026-11-03 | Persistent state raises time-to-good-solution for all 5 frontier agents but not final outcomes. The advantage shrinks from +24 to +1.2 points by the deadline. Matched vs **shuffled** insight cards give no consistent final gain. | 3–5 tasks per stream, 30-minute budgets, mostly a single run pair. Native CLI scaffolds confound model and harness. | Gives a ready-made *shuffled-memory* control for "procedure vs memory". Partial scoop on the improver-transfer question. |
| 9 | Can LLMs predict the outcomes of ML experiments? | 2610.22599 | 2026-10-15 | On 288 ideas (MLE-bench), best R²=0.49 (Opus 5.5) and all slopes 0.21–0.61 (magnitudes underestimated). Direction is right 69–82% of the time. The implementer barely matters, and there is no self-preference. | One seed per idea (noise ceiling unknown), small tasks, single-step only. Future work: sequences of ideas. | Basis for a "self-forecast" probe of improvers (Idea 2). |
| 10 | Language Models as AI Research World Models | 2610.12235 | 2026-10-15 | Records lift Spearman correlation 0.51→0.77 in-environment and 0.63→0.73 cross-environment, and cut Regret@3 by 52%. The worst model with records beats the best model without them. | In-context only, cross-environment limited to pretraining, 171k GPU-h cost (not reproducible by students). | Ties to Xunjian's world models. Leaves "forecast skill over a self-improvement run" open. |
| 11 | When Is Enough Enough in Self-Evolving LLM Systems? | 2610.04756 | 2026-10-03 | An anytime-valid e-detector on paired outcomes stops SkillOpt/GEPA at rounds 4–12 of 40 (49–92% tokens saved) with no loss. It also avoids a late reward-hacking artifact ("answer A"). | Binary rewards, i.i.d. assumptions, small validation sets (n=18–24) in some settings. | Scoops "Goodhart late-artifact" and stopping parts of the confound battery. A usable tool. |
| 12 | Aligning Autoresearch: Co-Improvement between Agents & Humans (Meta FAIR) | 2610.38882 | 2026-10-28 | Unaided autoresearch yields near-saturated benchmarks. Fine-grained human direction cuts solver scores by 28–46 points, and coarse direction barely helps. HackBench: agents fail at hack localization. | Muse Spark models only, few benchmarks, human effort not quantified in compute units. | Sharp risk for Dan's self-generated-curriculum idea (#3). Opens an "exchange rate of guidance" question. |
| 13 | The Economics of Recursive Self-Improvement (METR et al.) | 2609.15802 | 2026-09-14 | Self-sustaining acceleration needs a product of elasticities above 1. The threshold is ε_{R,C}>0.15 and current estimates are ~0.09. It gives a "wish list" of measurable elasticities. | Macro calibration on very rough data. No micro-experiments. | Gives Dan's improver-scaling idea an economics-grounded estimand (Idea 3). |
| 14 | What if automating AI R&D triggers an intelligence explosion? (GovAI) | 2609.36054 | 2026-09-28 | Policy and scenario analysis of software intelligence explosion. | Not empirical. | Citation hook for "why measure returns". |
| 15 | The Last AI Built by Humans: Toward Genuine RSI | 2609.11873 | 2026-09-10 (v3 09-22) | L1–L5 autonomy ladder. Open problems: safe inheritance, autonomy attribution, reliable verification. "Decisive evidence = inherited changes help later rounds… under explicit resource constraints." | Survey/position paper, no experiments. | Most-cited framing. Dan's measurement ideas map onto "autonomy attribution". |
| 16 | Agent Plasticity: Measuring Self-Improvement Through Experience | 2610.08902 | 2026-10-06 | Plasticity = cost-normalized conversion of experience into persistent held-out gains. | Object-level agents only (see plasticity.md). | Anchor. Meta-level plasticity remains open. |
| 17 | Recursive self-improvement of AI research agents (AIDE², Weco) | 2609.26457 | 2026-09-22 | Recursive improvement of research agents; ignition test inconclusive at 3 seeds. | Few seeds. | Anchor (in the reading list). |
| 18 | Mixture of Self-Improving Branches for Harness Optimization (Meta/Duke) | 2609.37834 | 2026-09-29 | Branches with evolving dev subsets and proposal guidance beat Meta-Harness (+34.8% on Math-Gemini). The authors argue a single trajectory converges to a local optimum. | Unequal tokens (2.2× Meta-Harness on Math), few repeats. | Indirect evidence of meta-agent basin effects, but the comparison is not compute-matched. |
| 19 | Meta²: Precomputing Reusable Knowledge for Online Harness Evolution | 2611.05139 | 2026-11-03 | Hot (166 votes): offline knowledge enables online harness evolution. | Not deep-read. | Crowds harness-evolution methods. |
| 20 | Stateless Language Agents: Scaling Long-Horizon Automated Research | 2610.07625 | 2026 (cited) | A stateless advisor plus workers; no long conversations. | Seen only via citation. | Another design point for the context-debt factorial. |
| 21 | The Interaction Tax: When communication erases diversity in multi-agent teams | 2608.23541 | 2026 (cited) | Reading peers' full solutions homogenizes proposals. | Seen via citation. | Diversity-collapse mechanism. |
| 22 | Third Eye: Beyond Immediate Gains in RSI | 2611.12988 | 2026-11-08 | Does a useful update remain useful after the next update? Tested on 3–8B models. | Small models. | Partial overlap with improver-scaling / compounding. |
| 23 | Agent Self-Improvement with Frozen Weights: Added Value and Limits of Failure Traces | 2611.11342 | 2026-11-08 | Cites Agent Plasticity and Rethinking Eval; failure traces have limited value. | Not deep-read (siblings cover it). | Threat to the "procedure vs memory" idea. |
| 24 | Latent On-Policy Self-Distillation | 2608.13040 | 2026-08-13 | Internalizing experience via OPSD in latent space (187 votes). | — | Default harness→weights tool. Crowds cluster 3. |
| 25 | Speculative Self-Distillation enables Efficient Knowledge Internalization | 2611.12016 | 2026-11-09 | Hot-feed paper (Stanford/ETH). | Not deep-read. | Same as #24. |
| 26 | Off-Policy Merging Beats On-Policy Self-Distillation for Continual Learning (CMU) | 2610.05872 | 2026-10-05 | Merging beats OPSD for continual learning. | — | Baseline for any weight-side CL. |
| 27 | Evaluating and Improving LLM Self-Modeling (UIUC/Anthropic) | 2608.30980 | 2026-08-31 | Measures whether an LLM predicts its own behavior, e.g. whether a prompt edit changes its answer. | Not deep-read. | Self-forecasting at the object level. Idea 2 lifts it to the improver level. |
| 28 | RSIGym / RSI-Forge / RSI-Env / Flex-RSI / SelfImprove-Bench / RSI-Dojo / ParetoRSI-Gym | 2610.10310 / 2610.09426 / 2611.01736 / 2611.08946 / 2611.02797 / 2611.03698 / 2610.34386 | Oct–Nov 2026 | Environments and benchmarks for RSI. | Benchmarks largely lack matched-compute baselines (per siblings). | Evidence that "another benchmark" is saturated. Usable as testbeds. |
| 29 | "Don't Just Trust Your Memory": Evidence-Linked Memory Reduces Memory-Induced Degradation | 2611.14320 | 2026-11 (hot) | Memory can degrade agents; linking memories to evidence helps. | Not deep-read. | Supports "accumulated state can hurt", the memory analogue of context debt. |
| 30 | Can AI agents conduct open-ended AI research? Early evidence (Princeton) | 2607.27191 | 2026-07-29 | Two case studies of open-ended research. | Case studies. | Context for the autoresearch trend. |

---

## 3. New since the reading list

None of these appear in reading_list.md, and most are also absent from the sibling sweep files:
- **Autoresearch dynamics:** 2610.07447, 2610.20563, 2611.verified-archives…, 2610.07625, 2608.23541, 2609.00077, 2607.27687 (Rehearse), 2608.10424 (Recovering Wasted Compute), 2609.18094 (Agora git memory, NVIDIA), 2610.28887 (AI research teams).
- **Research world models:** 2610.22599, 2610.12235, 2608.30980, 2610.29121 (siblings have it), 2610.13991 (in the reading list as IDM-guided Hyperagents).
- **Continual learning streams:** 2610.25858, 2611.03219, 2611.14505, 2611.05108, 2610.12668, 2610.38796, 2610.29357, 2610.33956, 2610.34724 (harness–model collaboration for long-horizon CL), 2610.22897 (When to Forget, risk-controlled context adaptation).
- **OPSD:** 2608.13040, 2611.12016, 2610.27495, 2610.05872, 2611.05313, 2610.05303, 2610.38637, 2611.13680.
- **Economics and macro:** 2609.15802, 2609.36054, 2610.33447, 2608.14426, 2609.00137, 2610.18445.
- **Human co-improvement:** 2610.38882 (Meta FAIR), 2610.33624 (RSI Arena), 2610.06824 (TasteVal).
- **Stopping and cost:** 2611.13524, 2610.04756.
- **Positions:** 2609.11873, 2610.35193, 2610.14625, 2611.10261 (Is RSI Safe?), 2610.27735 (RSI Control: covert sabotage, Scale AI).

---

## 4. Critiques and limitations that recur across papers

1. **Accumulated state is confounded.** Conversation, scored archive, unverified notes and memory cards are added or removed together (Tan et al.; Fork-and-Flush; Continual AutoResearch admits the same). The verified-archive rebuttal shows the answer flips depending on what is kept. No one has run the full factorial, and no one has run it on *meta-agents*.
2. **Single model, few seeds, single tasks.** Forgetting uses 1 model and 3 seeds. The verified archive uses 1 task and 3 runs. The RSI Tax uses one ordering. Continual AutoResearch uses a single run pair. The DeepMind continual-learner paper uses only Gemini Flash.
3. **Compute is not matched.** Mixture-of-Branches spends 2.2× the tokens of its Meta-Harness baseline. Most RSI benchmarks report endpoint scores only.
4. **Descriptive rather than interventional.** The RSI Tax says verification is 75–94% of cost but does not intervene. The DeepMind paper describes prompt-share displacement but not causes.
5. **Forecasts are judged once.** The ML-prediction paper has one seed per idea, so the noise ceiling is unknown. RWM is in-context only, and no one tracks forecast skill *over time inside a loop*.
6. **Memorization vs generalization is rarely split.** Only Continual-ARC re-serves fresh instances. Most harness-evolution papers evaluate on the search distribution.
7. **Macro models lack micro estimates.** The economics papers explicitly lack measured elasticities; ε_{R,C} has "almost no evidence".
8. **Human input is unpriced.** Meta FAIR shows that specificity matters but does not express the value of a hint in compute.

---

## 5. Open gaps (ranked by fit to Dan and Xunjian)

- **G1. Meta-agent plasticity through context debt.** Do harness-evolving meta-agents (Meta-Harness, GEPA, Hyperagents-style, Gödel Agent) show the same anchoring and idea-basin collapse as autoresearch agents? Is the plateau in harness evolution (HarnessX post-peak collapse, "Harness Evolution Hits a Ceiling") caused by *anchoring* rather than *saturation*? Which state component (conversation, archive, notes, memory) carries the effect, at matched compute and judged on held-out tasks?
- **G2. Does the improver learn a world model of its target?** Track the improver's pre-evaluation forecast of each edit's Δ (R², slope, sign accuracy) across generations, improver checkpoints and frozen vs self-modifying improvers. Rising forecast skill is a direct, cheap operationalization of "improving at improving", and it separates meta-learning from search.
- **G3. Micro-estimates of RSI elasticities.** Estimate the elasticity of improvement rate with respect to accumulated improvement (the analogue of ε_{Ȧ,A}, or "returns to research") inside controlled self-improvement loops, with CIs, across systems.
- **G4. Seen-vs-held decomposition of harness-evolution gains**, using fresh-instance generators (a Continual-ARC-style S_seen / S_held split) on τ-style procedural tasks.
- **G5. Price human guidance in compute**, as a dose–response of hint specificity against evolver iterations at matched outcome.
- **G6. When to forget, for meta-agents.** Anytime-valid detectors (2610.04756) could trigger resets instead of a fixed schedule. Fork-and-Flush reported that plateau triggers failed.

---

## 6. Threats to Dan's ideas

| Dan's idea | Status | Threats |
|---|---|---|
| **#2 Confound battery** | **Largely scooped as a standalone paper** | RSI Tax 2611.13524 (full-cost accounting, simple baseline wins); When Is Enough Enough 2610.04756 (late-artifact Goodhart, stopping); Auditing the Product 2610.27610; Harness-Delta Attribution 2610.22466; Elicitation or Expansion 2610.27889; Beneath the Surface 2611.07624. Keep it only as a *control suite* inside another paper. |
| **#1 Improver scaling / ignition** | **Alive, but needs a sharper estimand** | Continual AutoResearch 2611.05108 (persistence ≠ final gain; shuffled-memory control) partially covers "does experience transfer"; Third Eye 2611.12988; AIDE² 2609.26457. Reframe as elasticity estimation (Idea 3) or forecast-skill trajectory (Idea 2). |
| **#3 Self-generated curriculum for harness learners** | **Alive but at risk** | Meta FAIR 2610.38882 finds unaided agent-built benchmarks near-saturated (tasks too easy). Continual-ARC finds harness learners don't generalize. RSI-Forge and RSI-Env build environments. The decomposition (n_eff) is still novel, but the "generated tasks too easy" null is now *expected*. Anchoring with real tasks is essential. |
| **Cluster 3 (harness→weights compounding, 2×2, which lever)** | **Crowding fast** | Continual-ARC (weights generalize, harness doesn't); the OPSD wave (2608.13040, 2610.27495, 2611.05313); HERD 2610.31401; Harness–Model Collaboration 2610.34724; Ceiling 2610.11655; CoEvo-RSI 2610.25078. |
| **Meta-agent plasticity** | **Open at the meta level** | Object-level plasticity is taken by Agent Plasticity 2610.08902, DeepMind 2610.25858 and Tsinghua 2611.03219. *Plasticity of the improver* (loss of exploratory capacity as the meta-agent accumulates history) is unclaimed. See Idea 1. |
| **Procedure vs memory arm** | **Partially scooped** | The Continual AutoResearch memory-card ablation (matched / shuffled / none), 2611.11342 and Selection Is Not Learning 2610.22029. Still open for *self-modifying* improvers. |
| **Procedural τ generator (infra)** | **Alive, and more valuable now** | Needed for G4. Continual-ARC shows fresh-instance re-serving is the right design. |

---

## 7. Proposed ideas (new, grounded in the gaps above)

### Idea 1: Context debt: do meta-agents lose plasticity through their own history? (best fit)
- **Core question.** In harness-evolving meta-agents, is the late-stage plateau caused by anchoring on accumulated conversation, archive or notes (lost plasticity of the *improver*), or by genuine saturation of the target?
- **Claim.** "Self-improvers stall because they remember: at matched compute, resetting the meta-agent's conversation (but not its scored archive) recovers most of the late-stage held-out gain, so reported harness-evolution ceilings partly measure the improver's anchoring, not the harness's limit." The opposite result (no recovery) supports the saturation reading of the Ceiling and HarnessX papers, so either outcome is publishable.
- **Key experiment.** Two meta-agents (a GEPA-style prompt evolver and a Meta-Harness-style code evolver, plus a Gödel-Agent-style self-modifier if budget allows) on 2 benchmarks (a τ-bench subset plus the procedural τ generator, and AppWorld-lite or a BIRD stream), with Haiku-class or DeepSeek-Flash models.
  - **Factorial at matched tokens:** keep or drop {conversation} × {scored archive} × {unverified notes}, plus fork-and-flush and forget-to-best arms.
  - **Measures:**
    - the edit-size and novelty decay curve (AST diff, LLM-triplet t-STE as in Fork-and-Flush);
    - "false floor" declarations;
    - held-out (not search-set) gain per $;
    - an anytime-valid detector (2610.04756) used as a *reset trigger* vs a fixed schedule.
  - **Plasticity metric for meta-agents:** the ratio of held-out gain per unit compute in rounds t..t+k to that in rounds 0..k.
  - Seeds ≥5.
- **Why new.** Every anchoring and forgetting paper (2610.07447, 2610.20563, 2611.verified-archives) is on single-objective optimization with a scored evaluator, not on meta-agents scored on held-out tasks. The only factorial-ish comparison (verified archive) uses 1 task. The DeepMind and Tsinghua papers study object-level forgetting, not improver plasticity.
- **Closest prior.** Scaling Discovery Requires Forgetting 2610.20563; Verified Shared Archives 2611.verified-archives-…; Fork-and-Flush 2610.07447.
- **Cost.** A pilot is ~$30–50 (one evolver, one benchmark, keep vs reset). The full study is ~$600–1,200 on cheap models.

### Idea 2: Does the improver learn a world model of its target? Self-forecast skill as a probe of "improving at improving"
- **Core question.** Before each edit is evaluated, the meta-agent predicts its Δ on held-out tasks. Does forecast skill (R², slope, sign accuracy) rise across generations? Is it higher for self-modifying than frozen improvers? Does it predict the future improvement rate?
- **Claim.** "Self-improvers get better at *finding* improvements without getting better at *predicting* them. Forecast R² stays flat while accepted gains continue, so current RSI gains come from search, not from a learned model of the target." If R² does rise, that is the first direct evidence of meta-learning in RSI loops. Forecast slope <1 also predicts winner's-curse magnitude.
- **Key experiment.**
  - Instrument 2–3 self-improvement loops (GEPA-style, Hyperagents-lite, Gödel Agent) to log a predicted Δ for every candidate.
  - Evaluate each candidate on a held-out split, with ≥3 seeds per candidate to set the noise ceiling (fixing the limitation of 2610.22599).
  - **Controls:**
    - a fresh-context forecaster given the same history (does the *improver* know more than an outside model?);
    - shuffled history (as in Continual AutoResearch);
    - improver checkpoints at generations 0/10/20/40 (Dan's #1).
  - **Analysis:** regress the future gain rate on forecast skill. This yields a cheap plasticity diagnostic.
- **Why new.** 2610.22599 and 2610.12235 measure forecast skill of static models on fixed idea sets. Predict-Before-You-Modify 2610.13991 and RSI-on-a-Budget 2610.29121 *use* forecasts to save compute. None treats forecast skill *over a self-improvement trajectory* as the measurement of meta-learning. It fits Xunjian's world-model plus self-referential agenda directly.
- **Closest prior.** 2610.22599; 2610.12235; 2610.13991; LLM self-modeling 2608.30980.
- **Cost.** Forecasts are nearly free; the cost is in multi-seed held-out evaluation. ~$300–800 total, and the pilot can reuse the Idea 1 runs.

### Idea 3: Measuring the core feedback loop in miniature: elasticity estimates from controlled self-improvement loops
- **Core question.** In controlled RSI loops, what is the elasticity of the improvement rate with respect to the improver's own accumulated improvement, holding compute fixed? This is the micro-analogue of ε_{Ȧ,A} and the returns-to-research term in the METR/Epoch framework. Is it <1 everywhere, and does it differ by lever (prompt, code, memory)?
- **Claim.** "Across N self-improvement systems, the measured self-feedback elasticity is well below 1 (diminishing returns), with CIs, and it is lower on held-out than on search tasks. Today's RSI loops are not self-sustaining even in miniature." A system with elasticity ≥1 would be a headline.
- **Key experiment.**
  - Take improver checkpoints from Dan's #1 (generations 0/10/20/40).
  - Give each a fixed compute budget on the same fresh starting artifact and fresh tasks (procedural τ generator), and measure Δ per unit compute.
  - Fit log(gain rate) ~ log(accumulated improvement), with bootstrap CIs across ≥5 seeds × 3 systems.
  - Report the narrow (search-set) vs broad (held-out, OOD) elasticity, mirroring the paper's narrow/broad distinction.
- **Why new.** The economics papers (2609.15802, 2609.36054, 2610.33447) explicitly lack these elasticities, and the ML papers report imp@k or ignition tests without an economics estimand. This reframes Dan's improver-scaling idea so that policy and forecasting readers care and reviewers see a principled quantity.
- **Closest prior.** Economics of RSI 2609.15802; AIDE² ignition test 2609.26457; Hyperagents imp@k 2603.19461.
- **Cost.** ~$300–1,000, the same budget as Dan's #1.

### Idea 4: Memorize or generalize? A fresh-instance decomposition of self-improvement gains
- **Core question.** What share of harness or meta-agent evolution gains is task-specific memorization (fixes keyed to seen task templates) vs transferable skill?
- **Claim.** "Most accepted harness edits buy S_seen, not S_held: on fresh instances of seen task templates the evolved harness keeps X%, and on unseen templates ~0." This would generalize Continual-ARC's finding to mainstream harness evolvers and give the field a standard split.
- **Key experiment.** Use the procedural τ generator to re-serve fresh instances of seen templates (memorization) and unseen templates (generalization). Apply Continual-ARC's L, S_seen and S_held metrics to GEPA, Meta-Harness and an SRN-style bounded-note baseline (from the RSI Tax paper) at matched cost.
- **Why new.** Continual-ARC does this only on ARC with small models and off-the-shelf harnesses. Elicitation or Expansion 2610.27889 and SelfImprove-Bench 2611.02797 test transfer but not with fresh-instance re-serving.
- **Closest prior.** Continual-ARC 2611.14505; Elicitation or Expansion 2610.27889.
- **Cost.** ~$200–500 once the generator exists. **Scoop risk: medium.**

### Idea 5 (secondary): The exchange rate of guidance: pricing a hint in self-improvement compute
- **Core question.** How many evolver iterations (or $) is one hint worth, as a function of hint specificity? Use a stronger-model or oracle hint as a proxy for a human, plus a small human calibration.
- **Claim.** "One specific hint is worth k× the evolver's budget, but coarse hints are worth ~0. The guidance curve is a step, not a slope."
- **Key experiment.** Inject hints of 3 specificity levels at a plateau in GEPA and Meta-Harness loops. Measure the compute the unguided loop needs to match (a censored estimate).
- **Why new.** Meta FAIR 2610.38882 shows the effect of specificity but never converts it to compute.
- **Closest prior.** 2610.38882; RSI Arena 2610.33624.
- **Cost.** ~$200. **Relevance to Xunjian: lower.**

**Recommended combination for NeurIPS 2027:** Idea 1 + Idea 2 as one paper, "Plasticity of the improver". Context debt is the intervention and self-forecast skill is the diagnostic. Use Idea 3's elasticity as the summary estimand, the procedural τ generator for held-out power, and the RSI Tax SRN baseline plus the anytime-valid stopper as controls. This rides three hot trends (forgetting and idea basins, research world models, agent plasticity) while staying in Dan's cheap-measurement lane and Xunjian's self-referential and world-model agenda.

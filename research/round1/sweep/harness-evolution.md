# Sweep: Harness evolution with frozen models and harness <-> weights loops

Date of sweep: 2026-11-12. Sources: alphaXiv discover (4 recency searches plus 1 historical/bitter-lesson search), Semantic Scholar keyword search (8 queries), and S2 forward-citation expansion of Meta-Harness 2603.28052 (382 citers), Rethinking-Eval 2607.12227 (68), Harness-Updating≠Benefit 2605.30621 (53), Harness-Zero 2609.24974, Ceiling 2610.11655, SIA 2605.27276. I deep-read (targeted PDF queries) 12 papers: Ceiling, CoEvo-RSI, JoMoHa, HERD, Skill–Policy Co-evolution Trap (CODA), Third Eye, Where Should Learning Live, Finding the Right Fit, Memory Portability, Beneath the Surface, Harness-Delta Attribution, MaD-RSI, and Agent Plasticity. Every ID below appeared in a tool result. I took the abstract-level claims from S2 abstracts and the deep-read claims from the PDF text.

---

## 1. Summary of the landscape

- **The lane is more crowded than the reading list shows.** Meta-Harness alone has about 380 citers on S2. Between Sep 1 and Nov 10, 2026, roughly 150 papers that cite it propose a harness-evolution method or a domain application (video, robotics, CAD, SQL, security, recsys, and others). New method papers keep appearing: HarnessFlow (MCTS), HGM-Dual, Harmony Harness (multi-objective), HarnessPro (Pareto), HarnessMM (token efficiency), Meta² (precomputed knowledge), Harness² (recursive, Google), RuHarness (rubric co-evolution), Successive Scale Racing, and MESH-Harness (bandit). A new method alone is not a NeurIPS contribution in this lane.
- **The evaluation-critique sub-lane exploded in Oct–Nov 2026.** After Rethinking-Eval (2607.12227) came:
  - Harness-Delta Attribution (gains = overfitting + test-time scaling + residual; on average 54% overfitting and 14% TTS)
  - Beneath the Surface (all 16 evolved agents both gain and lose tasks; the top-scoring agent is not the best improver)
  - The RSI Tax (a bounded-note baseline matches Meta-Harness, EvoSkill and RRSI at 65–71% fewer tokens)
  - Elicitation or Expansion (pass@k analysis of harness gains)
  - Frozen-weights failure traces (2611.11342; about 60% of the gain comes from try-and-keep search without seeing failures)
  - Does Failure Feedback Direct Self-Modification (no target-specific repair in DGM logs)
  - Can LLMs Improve Their Own Harness (ARC-AGI-3), the Winner's Curse (2610.09239), and Masked Erosion (2610.38392)
  - Can Coding Agents Sustain Continual Evolution (forgetting across task streams)

  A "confound battery" for frozen-model harness evolution is now largely done.
- **The harness <-> weights lane also exploded.** Beyond the reading-list set (SIA, HASE, Co-Harness, HELIX, WHALE, Harness-Zero, Ceiling, Evo², CoEvo-RSI), new entries are:
  - Joint training: JoMoHa (joint RL of the harness-writer and the solver), CoHARL (population RL), HarnessAdapt-RL, Self-HPO (MSR), VACE (validation-gated alternation), CoTrace (data recipes)
  - Distill and internalize: HERD (25-stage recursive distillation), EvoIn, Harness Annealing, Latent Harness (activation-shift compilation), Harness-Aware Distillation, Harness-S (sibling harnesses)
  - Theory and selection: Where Should Learning Live (regret analysis), Your Policy Is Secretly a Harness (theory)
  - Lever choice: Not All Experience Belongs in the Weights (component routing rule), Harness-Model Collaboration (HaMo)

  "We co-evolve both" is dead as a contribution (K4).
- **Three empirical regularities are now established:**
  1. Alternating harness/weight updates can **collapse**. JoMoHa: 83→90.5→78.5→48.5→31.5% on ALFWorld, decomposed into solver decline (-34.5) plus compatibility loss (-13.0).
  2. Co-evolved pairs become **mutually specialized and fragile**. CODA: a peak-to-worst drift drop of 5–13 pts erases 52–93% of the skill gain, and fragility grows with training. CoEvo-RSI: brain–body mismatch.
  3. Most frozen-model harness gain is **overfitting, test-time compute or redistribution** (HDA, Beneath the Surface, RSI Tax, Rethinking-Eval).
- **What is still thin:**
  - Longitudinal behavior **across model generations** (upgrade survival). Only static cross-model grids exist (Finding the Right Fit, MaD-RSI, Optimize-Cheap-Deploy-Strong, Memory Portability).
  - **Compute-matched, multi-seed compounding curves** for co-evolution with a per-round 2×2 and placebo. CoEvo-RSI has 10 rounds × 3 runs but no compute-matched decomposition. JoMoHa shows collapse in a single alternating run. HERD has 25 stages but a fixed writer.
  - **Meta-agent (improver) plasticity**. Agent Plasticity ties actor and reflector to the same model and does not study plasticity decay over generations.
  - **Oversight and safeguard survival after internalization**.

---

## 2. Key papers

| ID | Date | Title (short) | Finding | Critique / limitation | Relevance |
|---|---|---|---|---|---|
| 2610.11655 | 2026-10-08 | Harness Evolution Hits a Ceiling | AHE-style loop on DeepPlanning: 4B 0.16→0.30, 9B 0.32→0.44 held-out. 2×2 of seed/evolved harness × base/LoRA. LoRA on evolved trajectories adds +0.13. Stacks on 4B, substitutes on 9B. A placebo (answer-shuffled) LoRA falls below base. Rule: harness for process failures (F2/F3), weights for content failures (F4). Edits to what the model *writes* train into weights; edits to what it *sees* stay in the harness (WebArena-Lite). | Single round of harness→weights (no multi-round loop). 2-rollout cells in ablations. Placebo only for 9B. No matched-budget test-time-search baseline (admitted). Cross-night drift up to 0.05. | Scoops the single-round 2×2 and the basic "which lever" rule. Leaves the multi-round loop, upgrade survival and prospective lever prediction open. |
| 2610.25078 | 2026-10-18 | CoEvo-RSI (Which models should follow?) | Brain–body mismatch. TIMR metric. Archive with checkpoint selection/merging. 10 rounds, K=8, 3 runs. Qwen3.5-9B beats DeepSeek-V4-Pro on TraceElephant (+14.16). Ablations: harness-only, model-only, latest-only (regresses at round 4). Old checkpoints re-selected (round-3 weights reused at round 10). | No compute-matched decomposition. Same-domain held-out only ("RSI or overfit?" admitted). Ablations on one benchmark. No upgrade test. | Highest threat to Dan #6 (multi-round). Its "latest-only" ablation partially answers HELIX held-vs-rebuilt. |
| 2611.11499 | 2026-11-08 | JoMoHa | Joint RL of Engineer (harness writer) and Solver. Naive alternation collapses 90.5→31.5%. The fixed-harness cross-eval splits decline into Solver decline (-34.5) plus compatibility loss (-13.0). Program × Solver cross-eval table. ALFWorld 97.0%, beats WHALE by 7.2 avg. | Mostly single-run. Baselines copied from Harness-R1. No seeds and no compute matching against RL alone. | Partially scoops the 2×2 / compatibility idea, but only as a motivating diagnostic. A systematic multi-seed version is open. |
| 2610.31401 | 2026-10-22 | HERD | Harness evolves (skills plus high-residual memory), then drift-constrained self-distillation into weights every stage. 25-stage TRACE+17, 3 seeds. Acquisition matches Seq-LoRA, forgetting is 0.010, deploys bare weights. Attaching the harness after distillation changes ≤0.2 pts (fully internalized). | One base model (Qwen3.5-4B), one stream. Ablations single-seed. The skill writer is frozen θ0, and co-evolving the writer is left as future work (App. E). Continual-learning framing, not compounding of one task. | Threatens Dan #6 (harness→weights for ≥5 rounds). Its open item is a writer that improves with the internalized weights, which is exactly the "does it compound" question. |
| 2610.20845 | 2026-10-14 | Skill–Policy Co-evolution Trap (CODA) | Cooperative acceptance makes the delivered (policy, bank) a local peak. Fragility to bank rewrites grows along training in 5/6 settings. Worst-case drift erases 52–93% of the skill gain. The co-evolved policy without its bank is worse than a never-skilled RL policy (57.6 vs 70.6). | Diagnoses but does not fix (L2). Rewriter is non-deterministic. Scale is unstudied. Skills only, not harness code or a model upgrade. | Partially scoops co-evolutionary Goodhart (Dan #7) and the lock-in narrative. It is the mirror of upgrade survival (bank drifts, policy fixed); the dual (policy upgraded, harness fixed) is not studied. |
| 2611.12988 | 2026-11-08 | Third Eye | A self-update's value after one further native update (H2). In 5 backbones (3–8B), continuation lowers utility (-1.78 pp pooled). Selection explains part of the neutral first step. | Only 2 horizons, LoRA weight updates, math and code. No harness. | Method template for "is a gain still a gain after the next update". The harness analogue (evolved harness after the model updates) is open. |
| 2611.09701 | 2026-11-06 | Where Should Learning Live? | Regret decomposition into selection, family restriction and signed state effect for text-only, parameter-only and joint configurations. Text-only has lower selection regret but higher deployed loss (Spider, Qwen3.5-9B). Interaction term ∆(z,w). | Selects among *pre-constructed* candidates. No construction of edits or adapters. Small empirical study. | Theoretical framing for "which lever when". A real scheduler that constructs edits and weight updates under matched compute is still open. |
| 2610.01787 | 2026-10-01 | Not All Experience Belongs in the Weights | Splits experience into locators, procedures, state facts and lessons. Locators and lessons go to weights; procedures and state facts go to context. A pre-training rule (recurrence × state-conditionality) predicts the destination on a held-out backbone. 3 families, 3 seeds. | GUI agents only. Experience routing, not harness code. | Second "which lever" rule (with Ceiling). Raises the bar for Dan's lever-scheduler idea. |
| 2610.00917 | 2026-10-01 | Finding the Right Fit | 66 model×harness configurations on TUA, ALE-CLI and TB4. Model rankings reverse across harnesses (Claude−GPT gap from +7.9 to −30.2). The best harness changes across tasks for 4/5 models. Native harness not reliably best. | One run per task, no variance. Hand-built (not evolved) harnesses. Settings not uniformly matched. | Static evidence that harness value is model-specific, which supports the upgrade-survival hypothesis. It does not test evolved harnesses or successive versions. |
| 2610.28201 | 2026-10-20 | MaD-RSI (Model-as-Data) | Single-model RSI harnesses couple to the target LLM. Optimizing a shared harness from 4 models' disagreement raises Pass^4 and transfers to 2 unseen models. | "Work in progress" (admitted). 3 rounds. No version-upgrade test. | Partial scoop of "harness portability". It optimizes *for* portability rather than measuring survival or absorption over generations. |
| 2608.10694 | 2026-08-11 | Optimize Cheap, Deploy Strong | GEPA prompts evolved on cheap tiers transfer upward to stronger targets across 11 models and 4 families. Characterizes when cheap-tier search substitutes. | Prompts only (not agentic harness code). Not successive releases. | Partial scoop for upgrade survival (upward transfer). Shows the question is live and that Dan must use agentic harness code and successive versions. |
| 2609.05339 | 2026-09-04 | Does Your Agent's Memory Survive a Model Upgrade? | NOTES memory transfers asymmetrically (+9.9 / −13.3 pts by direction). Fixed-schema KG is portable. A mixed embedding index loses about 60% of the upgrade gain. A pre-registered style lock. | 2 models of about 8B, a cross-family swap (not a successor version), memory not harness. | Closest *methodological* template for upgrade survival (RPAS metric, direction-specific). Extend from memory to evolved harnesses. |
| 2608.10178 | 2026-08-10 | One Recipe, Many Harnesses | Fixed evolution recipe × 8 languages × 3 base models. Asks whether evolved artifacts encode benchmark, language or model-compensation knowledge. | Abstract-level only (not deep-read). Coding only. | Directly relevant: "model-compensation" edits are exactly what an upgrade should absorb. Cite and differentiate. |
| 2610.22466 | 2026-10-15 | Harness-Delta Attribution | Gain = Overfitting (54% avg) + TTS (14%) + residual G (upper bound). 11 executors, 3 frameworks. ALFWorld harnesses for 0.8B/2B become LLM-free controllers. Validation selection does not close the gap. | Overfitting detection is agent-plus-human audited, so it is a lower bound. Single runs. | Kills a generic confound-battery paper. HDA should be a standard control in any new harness paper. |
| 2611.07624 | 2026-11-05 | Beneath the Surface | 4 methods (AHE, HyperAgents, A-Evolve, GEPA) × 2 harnesses × 2 domains. Every evolved agent gains *and* loses tasks. Cost rises. Transfer to τ²/DeepSWE is mixed. The top scorer is not the best parent. | Single model per harness (no model crossing). 8 passes. | Further saturates the eval-critique lane. Uses frozen champions, so no upgrade test. |
| 2611.13524 | 2026-11-10 | The RSI Tax | Continual stream with score-before-update. A bounded-note baseline matches Meta-Harness, EvoSkill and RRSI at 65.6–71% fewer tokens. 74.8–93.6% of RSI cost is verification. | Abstract-level only. | A must-include baseline for any harness paper (Dan's curriculum work too). |
| 2610.08902 | 2026-10-06 | Agent Plasticity (Singh…Goyal; Meta) | Plasticity = held-out gain per $ of learning cost, with fitted saturation. Frontier models differ sharply. Artifact-reuse diagnostics (absent / missed / used-but-failed). | Actor and reflector are the same model. No improver×executor crossing. No loss-of-plasticity across many generations (explicitly distinguished from deep-RL loss of plasticity). Post-training for plasticity is future work. | Defines the metric Dan wants to apply to meta-agents. The open gaps are a cross-model plasticity matrix and plasticity decay. |
| 2609.37105 | 2026-09-29 | VACE | Validation-gated alternation of agentic RL and harness revision. +6.4/+9.1 over weight-only RL, +4.6/+7.0 over ungated alternation. | Abstract-level. | Another co-evolution method. Confirms that gating alternation is now "known". |
| 2610.22791 | 2026-10-16 | Self-HPO (MSR/CMU) | The policy proposes its own harness candidates, then RL under the selected harness, alternating. | Abstract-level. | Self-referential co-evolution (Gödel-like) is now taken as a method. |
| 2610.36469 | 2026-10-27 | CoHARL | Population of executable harnesses co-evolved with a shared policy during RL. +6.4 pts over harness-only on SWE-rebench. | Abstract-level. | Crowds co-evolution further. |
| 2611.04277 | 2026-11-03 | Harness-S | Sibling harnesses rescue failures, then transfer into one policy that works under the base harness. Names Locality, Complementarity and Nonstationarity of harness variants. | Abstract-level. | Distillation of harness gains is crowded. |
| 2611.01415 / 2610.01235 / 2609.35290 | Oct–Nov 2026 | Latent Harness / Harness Annealing / EvoIn | Internalize harness behavior into activation shifts (8/10 pre-registered settings recover the gain), into weights with a curriculum of weakening harness support, or into rewritten reasoning traces. | None test whether *safeguard* or *audit* properties survive internalization. | Background for "oversight after distillation". |
| 2610.38392 | 2026-10-28 | Masked Erosion | Acceptance gates silently delete redundant safeguards (a deletion behind another working safeguard adds no observable harm). 411 loops, 10 gate rules. | Frozen-model harness only. | Mechanism that could compound when the harness is distilled. Adjacent to the oversight idea. |
| 2610.21458 | 2026-10-15 | What Survives Self-Evolution: Provenance Marks | Watermarks in harnesses rarely survive evolver rewrites (KGW 0/12 under GEPA). | Provenance, not safety behavior. | Template for "does property X survive transformation T". |
| 2610.37255 | 2026-10-27 | Your Policy Is Secretly a Harness | Under KL-regularized control, a trained policy encodes its harness's action preferences. Return = harness ceiling − policy mismatch. Training changes only the mismatch. | Theory with small experiments. | Theory for why a harness tuned to model v_n mismatches v_{n+1}. A useful framing citation. |
| 2609.22247 | 2026-09-05 | CHART | Behaviors learned under one harness are harness-local and vanish when the harness changes. Naive harness augmentation does not fix it. | Search agents only. | The mirror of upgrade survival (harness changes, model fixed). |
| 2610.30499 | 2026-10-21 | Can LLMs Improve Their Own Harness? (ARC-AGI-3) | Controlled study: frontier models build and revise a harness twice. | Abstract-level. | Eval-critique lane. |
| 2611.11342 | 2026-11-08 | Frozen weights: value and limits of failure traces | Failure-trace notes add +4.0 pts over a matched search that never sees the failures. About 60% of the gain comes from try-and-keep alone. No demonstrated gain in full conversations. | 9B model, τ-style CS tasks. | Directly overlaps Dan's confound battery and τ-bench plans. |
| 2611.08946 | 2026-11-06 | Flex-RSI | Benchmark: the improver revises the harness *including itself*, on vision and robotics, under a fixed budget. | Abstract-level. | Crowds the "RSI benchmark" space. |
| 2610.14224 | 2026-10-09 | Harness² (Google) | Recursive harnessing for an open world. 140 votes. | Not read. | A high-visibility method, likely a baseline. |
| 2611.05139 | 2026-11-03 | Meta² (UMD) | Precomputes reusable knowledge for *online* harness evolution in long-horizon agents. 166 votes. | Not read. | High-visibility new method. |

---

## 3. New since the reading list (not in reading_list.md)

The co-evolution and internalization entries are already listed in Section 1 (JoMoHa, HERD, CoHARL, HarnessAdapt-RL, Self-HPO, VACE, CoTrace, Harness-S, EvoIn, Harness Annealing, Latent Harness, Harness-Aware Distillation, HaMo, and the component-routing rule). Other new items:

- **Measurement / critique:** Harness-Delta Attribution 2610.22466; Beneath the Surface 2611.07624; RSI Tax 2611.13524; Elicitation or Expansion 2610.27889; Frozen-weights failure traces 2611.11342; Does Failure Feedback Direct Self-Modification 2610.14662; ARC-AGI-3 controlled study 2610.30499; Can Coding Agents Sustain Continual Evolution 2611.03219; Masked Erosion 2610.38392; Provenance Marks 2610.21458; When Agents Fail: Model or Harness 2610.32483; Finding the Right Fit 2610.00917; Theory of Reliable Self-Evolution 2609.08175; Winner's Curse 2610.09239 (confirmed ID).
- **Co-evolution and lever choice (not named in Section 1):** Third Eye 2611.12988; Skill–Policy Co-evolution Trap 2610.20845; Where Should Learning Live 2611.09701; Your Policy Is Secretly a Harness 2610.37255; Multi-Harness RL 2609.04518; Interplay of Harness Design and Post-Training 2606.25447; CHART 2609.22247; Harness Policy Distillation 2610.20261; Agentic Knowledge-Model Co-Evolution 2610.30790; Verifier-Harness Co-Evolution 2610.17609; MetaRSI 2609.06396; RSIFlow 2610.21791.
- **Portability / upgrade:** Memory Portability 2609.05339; MaD-RSI 2610.28201; Optimize Cheap Deploy Strong 2608.10694; One Recipe Many Harnesses 2608.10178; Evo2Team 2609.34135; AI4AI Strong-to-Weak 2608.12307; Teaching the Harness (SAT) 2611.13144.
- **New frozen-model methods:** Harness² 2610.14224; Meta² 2611.05139; HarnessFlow 2611.06254; Harmony Harness 2610.15150; HarnessPro 2610.26770; HarnessMM 2610.32813; HGM-Dual 2610.30042; RuHarness 2610.36826; SSR 2610.28815; MESH-Harness 2610.05300; LiteEvo 2609.33146; HarnessCompass 2608.01918; ModularRSI 2609.14857; VERSE 2610.02616; Causal Improvement Graph 2610.05039; Mixture of Self-Improving Branches 2609.37834; Harness Evolution as Learning (theory) 2609.36892; Conservative Meta-Agent Search 2610.31436; Scaling Discovery Requires Forgetting 2610.20563.
- **Benchmarks:** Flex-RSI 2611.08946; RSI-Env 2611.01736; HarnessDev 2609.01437; Doc2Harness 2610.20344; ParetoRSI-Gym 2610.34386.

---

## 4. Critiques and limitations that recur across papers

1. **Single base model or single stream** (HERD, Ceiling's main lines, Third Eye's tasks, Memory Portability has 2 models). Reviewers will ask about generality across model families and sizes.
2. **Few or no seeds for the loop itself.** CoEvo-RSI runs 3; JoMoHa's collapse diagnosis is one run; ablations are single-seed in HERD and Ceiling. Run-to-run variance of whole evolution lines is rarely reported. Beneath the Surface and Ceiling show that per-rollout noise (σ≈0.023–0.045) is comparable to many claimed gains.
3. **No compute-matched alternative.** Ceiling admits it has no matched test-time-search baseline. Co-evolution papers compare to GRPO at a "sample budget" but not to harness search plus RL at equal GPU+API cost. HDA and RSI Tax show this changes conclusions.
4. **Overfitting to benchmark artifacts.** HDA reports ALFWorld controllers with zero LLM calls and the LiveMath meta-option. ALFWorld is used by JoMoHa, CODA and HDA alike, so headline ALFWorld numbers (97%) are suspect.
5. **Evaluation on the delivered pair only.** CODA and CoEvo-RSI show that co-evolved pairs are mutually specialized. Nobody evaluates when either component is swapped for an exogenous upgrade.
6. **Writer/improver held fixed.** HERD's writer is θ0. Agent Plasticity uses the same model for act and reflect. Whether the *improver* gets better as weights internalize is untested (this connects directly to Gödel Agent).
7. **Diagnostics without fixes.** CODA, Third Eye and Masked Erosion measure failure but offer no reliable mitigation.
8. **Rules fitted post hoc.** Ceiling's process/content rule and component routing's recurrence rule are derived on the same lines they explain. Only component routing tests on a held-out backbone. No prospective, pre-registered test of a lever-choice rule exists.

---

## 5. Open gaps (what is still genuinely open as of 2026-11-12)

- **G1. Upgrade survival / harness half-life.** No paper evolves a harness on model v_n and measures how much of the gain survives, is absorbed (redundant), or turns harmful on v_{n+1} in the same family. Related work stops short:
  - Finding the Right Fit and MaD-RSI cross *different* models statically.
  - Optimize-Cheap-Deploy-Strong covers prompts only, upward across tiers.
  - Memory Portability covers memory, cross-family.
  - Third Eye covers weights only.

  No paper tests warm-starting re-evolution from the old harness vs re-evolving cold on the new model at matched compute. Ceiling's taxonomies predict which edits should be absorbed (process vs content; "sees" vs "writes"), but nobody has tested them.
- **G2. Compute-matched, multi-seed compounding curves for harness <-> weights loops, with a per-round 2×2 plus placebo.** CoEvo-RSI and HERD give curves but no per-round decomposition. JoMoHa's decomposition is one-shot and single-run. The open question is whether Δ_t grows or shrinks, and whether the interaction term (harness×weights) turns negative over rounds (lock-in, as CODA's fragility growth suggests).
- **G3. Improver plasticity.** Does the meta-agent's ability to convert experience into held-out gain decay as its archive or harness grows (a Gödel-Agent complexity ceiling), and can resets or forgetting restore it? Also, does a stronger improver model rescue a weak executor's low plasticity? This needs an improver×executor plasticity matrix. Agent Plasticity explicitly leaves both questions open.
- **G4. Prospective lever choice.** Ceiling, component routing and Where Should Learning Live give rules or theory. None pre-registers a lever-choice predictor and tests it on new task×model cells at matched compute.
- **G5. Oversight after internalization.** HERD, Latent Harness, Harness Annealing and Harness-Zero move behavior into weights; Masked Erosion shows safeguards erode under gates. Whether safeguard rules survive distillation, and whether harness-level audit trails still detect violations, is unmeasured.
- **G6. Writer that co-evolves with internalized weights** (HERD App. E; the Gödel-style self-referential loop). JoMoHa and Self-HPO train the writer jointly, but nobody measures whether the writer gets better at writing (improver scaling for co-evolution).

---

## 6. Threats to Dan's ideas

- **#6 Multi-round harness→weights compounding: heavily threatened.** HERD (25 stages, 3 seeds), CoEvo-RSI (10 rounds, 3 runs), JoMoHa (alternation collapse), VACE, Self-HPO, CoHARL, HarnessAdapt-RL and Third Eye (H2) all touch it. It survives only as a measurement paper with three parts: (a) a compute-matched comparison against harness-only, weights-only and test-time scaling; (b) a per-round 2×2 plus placebo; and (c) ≥3 seeds and ≥2 model families. The headline must be "does it compound / when does it lock in", not a method.
- **2×2 crossing decomposition: partially scooped.** Ceiling ran the single-round 2×2 with a placebo. JoMoHa ran a fixed-harness × solver-checkpoint cross. CoEvo-RSI's Figure 8 is a harness × checkpoint TIMR matrix. The longitudinal per-round version with seeds is still open. HELIX held-vs-rebuilt is partly answered by CoEvo-RSI's latest-only vs archive ablation.
- **Survival under model upgrades: alive (best of Dan's cluster-3 ideas).** It is adjacent to Finding the Right Fit, MaD-RSI, Optimize-Cheap-Deploy-Strong, Memory Portability and One Recipe Many Harnesses, but no paper is longitudinal, within-family, about evolved harness code, measures an absorption fraction, or compares warm vs cold re-evolution. Scoop risk is medium; something could appear by Feb 2027.
- **Which lever when: medium-high scoop.** Ceiling, component routing, Where Should Learning Live, MetaRSI and RSIFlow already exist. It is alive only as a prospective, pre-registered validation benchmark of lever rules.
- **Oversight after distillation: alive** and adjacent to Masked Erosion and Provenance Marks. Mentor relevance is moderate and it fits a safety venue better.
- **#7 Co-evolutionary Goodhart: partially scooped.** CODA covers fragility growth during co-evolution, and RuHarness covers critics falling behind. Train-vs-true drift for a generator+solver remains open but is expensive.
- **#2 Confound battery: largely scooped** by HDA, Beneath the Surface, RSI Tax, frozen-weights failure traces and Rethinking-Eval. Reuse these as controls; do not write the paper.
- **Curriculum (#3) on τ-bench: note** that 2611.11342 (τ-style customer-service, 9B frozen) and RSI Tax (continual stream) are close neighbors. The "S_real(n)" data-scaling question is partially touched by SSR (2610.28815: performance *degrades* as the task pool grows, an important surprise to cite or replicate).

---

## 7. Proposed ideas

### Idea A: Harness Half-Life: do evolved harnesses survive model upgrades? (recommended)
- **Core question:** When a harness evolved for model v_n is carried to its successor v_{n+1} in the same family, what fraction of the gain survives, what fraction is absorbed (redundant), and what fraction becomes harmful? Should one keep, re-evolve warm, or re-evolve cold?
- **Claim:** "Most evolved-harness gain is model compensation that the next model absorbs. Only edits that change what the model *sees* (information and interface) survive upgrades, and warm-starting re-evolution from an old evolved harness is no better than, or worse than, cold re-evolution at matched compute." Either outcome is publishable: high survival means evolved harnesses are durable assets; low survival means a "half-life" metric and practice change.
- **Key experiment:**
  - Model ladders: Qwen3→Qwen3.5→Qwen3.6 at matched sizes, plus a size ladder and one other family.
  - Evolution: Meta-Harness, AHE and GEPA-style evolvers on 2–3 low-artifact benchmarks (τ²/τ³ procedurally generated tasks, Terminal-Bench subset, DeepPlanning), with ≥3 seeds per line.
  - Metrics: retained-gain ratio (RPAS-style, from Memory Portability); absorption = gain(v_{n+1}+H*) − gain(v_{n+1}+H0) relative to gain(v_n); edit-level ablation tagged by Ceiling's process/content and "sees/writes" taxonomy; HDA overfitting audit.
  - Re-evolution arms: keep / warm / cold at matched API budget.
- **Why new:** Prior cross-model work is static (different vendors at one time point) or covers prompts or memory. None measures within-family successive versions, absorption, or warm vs cold re-evolution of harness code. It also reuses the existing anchor-harness methodology.
- **Closest prior:** 2610.00917 Finding the Right Fit; 2610.28201 MaD-RSI; 2608.10694 Optimize Cheap Deploy Strong; 2609.05339 Memory Portability; 2608.10178 One Recipe Many Harnesses; 2610.11655 Ceiling (taxonomy).
- **Cost:** About $800–1.5k API with open Qwen models via cheap endpoints. No GPU needed beyond inference.

### Idea B: Does co-evolution compound? Per-round 2×2 accounting of harness <-> weights loops
- **Core question:** In alternating harness-evolution plus LoRA distillation loops, does the per-round gain Δ_t grow, stay flat or shrink? How do the harness, weight and interaction terms evolve, under compute matching against harness-only, weights-only RL and test-time scaling?
- **Claim:** "Co-evolution's apparent compounding is front-loaded. After 2–3 rounds the harness×weights interaction term turns negative (mutual specialization), and a compute-matched weights-only or harness-only arm catches up." Alternatively: compounding is real, and the paper shows where it comes from.
- **Key experiment:**
  - 6–8 rounds × 3 seeds on 2 small models (Qwen3.5-2B/4B, LoRA).
  - At every round, evaluate all four cells {H_{t−1},H_t}×{θ_{t−1},θ_t} plus a placebo adapter (answer-shuffled, as in Ceiling) on fresh held-out tasks from a procedural generator. JoMoHa's fixed-harness cross is a cheap per-round compatibility metric.
  - Compute-matched curves (GPU-h + $).
  - Interventions: archive-based inheritance (CoEvo-RSI) vs latest-only; drift anchors (HERD).
- **Why new:** CoEvo-RSI, HERD and JoMoHa give curves or one-shot decompositions without a longitudinal 2×2, seeds and compute matching together. It turns three method papers' anecdotes (collapse, mismatch, fragility) into one quantified law.
- **Closest prior:** 2610.25078 CoEvo-RSI; 2611.11499 JoMoHa; 2610.31401 HERD; 2610.11655 Ceiling; 2610.20845 CODA.
- **Cost:** $2–6k GPU (small models) plus about $500 API. This is the expensive tier, but feasible with 4B models and LoRA.

### Idea C: Meta-agent plasticity: does the improver lose plasticity, and can forgetting restore it?
- **Core question:** Apply Agent Plasticity to the *improver*. Build a cross matrix of improver model × executor model, and track how plasticity (held-out gain per $) evolves over many generations of a self-referential improver (Gödel-Agent-style, where the improver edits its own procedure) vs a fixed improver.
- **Claim:** "Meta-agents lose plasticity as their archive and procedure grow (the Gödel-Agent complexity ceiling is a loss-of-plasticity phenomenon), and simple resets (archive pruning, procedure re-initialization with retained artifacts) restore it, mirroring deep-RL plasticity loss." The null (no decay) is also informative and refutes the complexity-ceiling story.
- **Key experiment:**
  - 3 improver models × 3 executor models on 2 environments with procedural held-out sets.
  - 30–50 generations, 3 seeds.
  - Plasticity measured in sliding windows.
  - Interventions at generation k: reset, prune, "forgetting" (2610.20563), and swap to a stronger improver.
  - Separate procedure from memory: freeze the procedure and keep the archive, and vice versa.
- **Why new:** Agent Plasticity ties actor = reflector, studies about 20 checkpoints, and explicitly distinguishes itself from loss-of-plasticity. No paper measures improver plasticity decay or restoration. This is closest to the mentor's Gödel Agent work.
- **Closest prior:** 2610.08902 Agent Plasticity; 2410.04444 Gödel Agent; 2603.19461 Hyperagents; 2609.26457 AIDE²; 2610.20563 Scaling Discovery Requires Forgetting.
- **Cost:** About $600–1.5k API.

### Idea D: Pre-registered lever-choice benchmark (harness vs weights vs both at matched compute)
- **Core question:** Can diagnostics measured *before* any intervention predict which lever wins at matched compute on unseen (task, model) cells? Candidate diagnostics: Ceiling's failure composition, component routing's recurrence and state-conditionality, and pass@k elicitation vs expansion.
- **Claim:** "Published lever rules predict the winning lever at ≤chance on new cells", or "rule X generalizes, with AUROC Y." Either outcome is publishable, and it releases a dataset of lever outcomes.
- **Key experiment:**
  - About 30 task families × 4 models.
  - Per cell: harness evolution, LoRA on self-trajectories, and the sequence, each at a fixed GPU+API budget.
  - Pre-register the predictors, then evaluate.
- **Why new:** Existing rules are post hoc or tested on one held-out backbone. No prospective validation exists.
- **Closest prior:** 2610.11655 Ceiling; 2610.01787 component routing; 2611.09701 Where Should Learning Live; 2610.27889 Elicitation or Expansion.
- **Cost:** $3–8k (GPU-heavy). It is the riskiest on budget and could be shrunk to 10 cells.

### Idea E: Do safeguards survive internalization? Oversight after harness→weights distillation
- **Core question:** When a harness containing safety rules (refusals, permission checks, logging duties) is distilled into weights (HERD-, Harness-Zero- or Latent-Harness-style), do the safeguards persist, especially redundant ones masked by others? Can harness-level audit trails still detect violations?
- **Claim:** "Task-relevant harness behaviors internalize, but masked or redundant safeguards are dropped by distillation at much higher rates, and post-distillation audits lose their signal." This mirrors Masked Erosion but for weights.
- **Key experiment:**
  - Factorial safeguard subsets (as in Masked Erosion) in 2 agent environments (τ-style permissions, RBAC SQL).
  - Distill with 3 internalization methods.
  - Measure per-safeguard retention after harness removal, and the detection rate of harness-level monitors.
- **Why new:** Internalization papers measure task gain only. Erosion and provenance papers study frozen-model harnesses only.
- **Closest prior:** 2610.38392 Masked Erosion; 2610.31401 HERD; 2611.01415 Latent Harness; 2610.01235 Harness Annealing; 2610.21458 Provenance Marks.
- **Cost:** $1–3k GPU.

**Recommendation:** Idea A is the strongest on novelty × feasibility × surprise, and it can absorb Dan's 2×2 and "survival" threads as a sub-analysis. Pair it with Idea C if the mentor wants the self-referential angle.

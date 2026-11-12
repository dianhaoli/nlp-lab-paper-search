# Round 1 — NeurIPS 2027 idea search: self-improving / meta-agents

*Nov 12, 2026 · Dan (mentor: Xunjian Yin). Produced by a 34-agent workflow: 8 topic sweeps → 18 candidates (16 parked) → 18 adversarial scoop-search + mock-review critiques → 6 full proposals → 3-persona panel.*

Files: `sweep/*.md` (topic lit reviews) · `candidates.md` · `critiques/*.md` (one per idea, with scoop papers) · `proposals/*.md` (full proposals) · `LITERATURE.md` (~380 papers new vs. your reading list) · `context/rubric.md`.

---

## TL;DR

1. **The field moved a lot in Oct–Nov 2026.** Several of your ideas are now scooped or close to it:
   - **Confound battery as a standalone paper: scooped.** HDA 2610.22466 and *Beneath the Surface* 2611.07624 cover it, and 2611.07624 already re-ran HyperAgents.
   - **Task-selection baselines: scooped.** ActiveSaddler 2610.00906 and Task-CoEvolve 2608.20169 cover them.
   - **Anchoring ablation: done for weight learners.** Anchored Self-Play 2607.03523 ran it.
   - **Multi-round harness→weights compounding: heavily crowded.** HERD 2610.31401, CoEvo-RSI 2610.25078 and JoMoHa 2611.11499 all occupy it.
   - **Ignition-test framing: taken.** AIDE² and *Auditing the Product, Not the Recursion* 2610.27610 both use it. 2610.27610 is a meta-analysis that names your improver × state swap test as the missing experiment but runs none, so it is both your strongest motivation and your biggest scoop risk.
2. **The open lane is narrower and sharper:** *what exactly does a self-referential improver learn, and does its ability to improve transfer or decay?* Nobody has run a powered, compute-matched, multi-seed **procedure × memory transplant** across generations and domains.
3. **Top recommendation is one merged paper (panel score 75.8):** **"Do Self-Referential Improvers Get Better at Improving? Effect, Cause, and Decay."**
   - **Cause:** procedure-vs-memory transplant.
   - **Decay:** meta-plasticity / warm-start penalty on a new domain.
   - **Effect:** a slim same-start swap curve.

   It tests Gödel Agent's own stated limitation, is squarely Xunjian's agenda, needs no GPU (about $1.5–2k API), and every outcome is publishable.
4. **Runners-up, each a strong standalone paper:**
   - **Evolving in a Dream (74.6):** the best fit for Xunjian's world-model interest.
   - **Learning to Goodhart (74.2):** the cleanest causal design and the most "surprising" headline.
   - **Curriculum Goodhart (72.1):** what your current lead (#3) becomes after the critique. Still viable, but heavier and more crowded than the top idea.

---

## Rubric (full version: `context/rubric.md`)
Total = 20 × (3·**N**ovelty + 2.5·**S**urprise + 2·**R**elevance to mentor + 2·**C**ompetitiveness/rigor + 1.5·**F**easibility + 1·**T**iming) / 12. A score of 75 or more is strong, 60–75 is viable with fixes, and below 60 means park.

Kill flags:
- **K1:** same claim and design already published in 2026.
- **K2:** a null result would be unpublishable.
- **K3:** needs closed-model budget we can't afford.
- **K4:** "we combine X+Y" is the whole contribution.

---

## Final ranking

| # | Idea | Topic | Critique | After proposal / panel | Verdict |
|---|---|---|---|---|---|
| 1 | **Merged: Effect, Cause, Decay** (procedure-vs-memory + meta-plasticity + slim swap curve) | T1/T2 | — | **75.8** | **Lead paper.** |
| 2 | [evolve-in-dream](proposals/evolve-in-dream.md) | T6 world models | 71 | 74.6 | Best mentor fit. Squeezed by 2611.01139, 2608.12253 and WMRL. Survives only with a causal fidelity manipulation. |
| 3 | [learning-to-goodhart](proposals/learning-to-goodhart.md) | T3 Goodhart | 71 | 74.2 (proposal self-score 76) | Cleanest causal 2×2 and most surprising claim. Relevance is a bit lower (R 3.5). |
| 4 | [meta-plasticity](proposals/meta-plasticity.md) (standalone) | T2 | 68 | 72.9 | Fold into #1. Biggest threat: Agent Plasticity (2610.08902) lists "transfer of improvement strategies" as future work. |
| 5 | [curriculum-goodhart](proposals/curriculum-goodhart.md) | T4 | 71 | 72.1 | Your lead, reframed as measurement science plus a harness-vs-LoRA substrate contrast. Heavy (F 2.5). |
| 6 | [procedure-vs-memory](proposals/procedure-vs-memory.md) (standalone) | T1 | 72 | 71.3 | Fold into #1. HMED 2610.07979 and VERSE 2610.02616 each ran one cell. |
| 7 | [harness-half-life](proposals/harness-half-life.md) | T5 | 65 | 69.6 | Most feasible, but off-agenda. A good second or side paper. |
| 8 | swap-factorial | T1 | 72 | → #1 | Becomes the "Effect" section. 2610.27610 publicly names this design. |
| 9 | breaking-the-improver | T6 | 68 | — | Uses BreakingWeb (Xunjian). Good follow-up or shift probe. |
| 10 | parametric-godel | T6 | 67 | — | Extends WeAct. Next paper (needs GPU). |
| 11 | self-forecast | T6 | 66 | → #1 readout | |
| 12 | rsi-elasticity | T1 | 65 | → #1 estimand | |
| 13 | context-debt | T2 | 65 | → #1 reset arms | |
| 14 | safeguards-internalization | T5 | 64 | park | Safety venue. |
| 15 | gate-dial | T2 | 63 | park | TRUST 2611.10787 and Winner's Curse 2610.09239 are close. |
| 16 | teacher-substrate | T4 | 62 | park | |
| 17 | saturation-law | T4 | 61 | park | Could be a section inside #5. |
| 18 | meta-winners-curse | T3 | 60 | → #1 placebo appendix | |
| 19 | improver-erosion | T3 | 55 | park | Masked Erosion 2610.38392 and RSI Tax 2611.13524 cover it. |

**Parked at clustering (K1 or infeasible):**
- Confound battery as a standalone paper.
- Hyperagents replication as a standalone paper.
- Task-selection baselines.
- Anchoring ablation.
- Harness→weights compounding with the 2×2 crossing.
- Co-evolutionary Goodhart with weights ($4–15k; CODA 2610.20845 and False Frontiers 2609.39102 overlap).
- PAIRED-style generator (SPADE 2608.19197).
- Which-lever-when (Ceiling 2610.11655, Component Routing 2610.01787).
- Procedural τ-generator as a benchmark paper. The benchmark space is saturated, so build it as infrastructure instead.
- Actor-vs-reflector plasticity (Agent Plasticity future work).
- A new world-model-guided self-modification method (IDM 2610.13991, SpecRSI 2610.16491, Rehearse 2607.27687).
- Ignition test as framing.

---

## Top idea in detail — "Do Self-Referential Improvers Get Better at Improving? Effect, Cause, and Decay"

**Core questions**
1. **Effect:** at matched dollar budget, does a generation-k improver produce a larger held-out gain than generation 0 from the *same* starting agent, and does the gap grow with k?
2. **Cause:** is any advantage carried by the improver's *procedure* (its own code or prompts) or its *memory* (archive, notes, scored history)? This uses the eight-arm transplant: P0/Pk × M0/Mk, plus a format adapter and shuffled/placebo memory.
3. **Decay:** on a *new* domain B, does a warm generation-k improver improve a fresh agent less per dollar than generation 0 (the "warm-start penalty")? Does the history-wipe "forgetting" reset fix it, or only a procedure reset?

**Headline claims.** All branches are publishable.
- **Memory wins:** "learning to improve" is mostly learning the task distribution. The improvement vanishes after a domain change.
- **Procedure wins:** this would be the first controlled evidence of procedural meta-learning in LLM improvers. We locate the generation k\* where it appears and the edit types that carry it.
- **Penalty:** improvers lose plasticity with k, the procedure carries the loss, and forgetting doesn't fix it. This is a sharp, surprising result.

**Design sketch**
- **Improvers:** Gödel-lite (code self-modifier), a VERSE- or Meta-Harness-style harness optimizer, and a meta-skill evolver.
- **Models:** about 27B open improver with a 9B executor, plus one cheap API model.
- **Tasks:** procedurally generated τ-style worlds for unlimited held-out tasks and near/far domain B.
- **Statistics:** 5+ seeds, paired task streams, a compute-matched parallel-sampling baseline (Rethinking-Eval style), and a paired bootstrap (RRSI style).

**Why it's novel (closest work and the gap each leaves)**
- **2610.27610 Auditing the Product, Not the Recursion:** names the swap test but runs no experiments.
- **MetaRSI 2609.06396, RSIFlow 2610.21791, FreeEvolve 2610.09197:** each has a single off-diagonal swap at one state, no seeds and no crossing. FreeEvolve also suggests Haiku-class improvers don't move, so the improver must be capable enough.
- **Agent Plasticity 2610.08902:** actor-level only, with improver-strategy transfer listed as future work.
- **Scaling Discovery Requires Forgetting 2610.20563:** object-level only, with no self-modifying improver.
- **Continual AutoResearch 2611.05108:** fixed agent procedure.
- **HMED 2610.07979 and VERSE 2610.02616:** one cell each.

**Main risks**
- A null effect from weak improvers, which K2 makes uninterpretable. Mitigate with a manipulation check that checkpoints actually differ, and a testbed with a known gain.
- A fast follow-up to 2610.27610. Mitigate by posting a preprint around Feb–Mar 2027.

---

## Literature landscape by topic (full reviews in `sweep/`)

- **T1 Improver scaling / meta-level ability** ([sweep](sweep/improver-scaling.md)). An Oct–Nov 2026 wave of RSI-evaluation meta-research (2610.27610, Beneath the Surface 2611.07624, RSI Tax 2611.13524, Third Eye 2611.12988) all say current "improving at improving" claims audit the product, not the recursion. Recurring critique: no same-start swaps, no matched compute, a single k.
- **T2 Plasticity** ([sweep](sweep/plasticity.md)). Agent Plasticity (Meta FAIR, 2610.08902) defines a cost-normalized plasticity metric at the actor level. "Are Self-Evolving Agents Continual Learners?" (2610.25858) owns the stability-plasticity framing at the object level. Nobody measures plasticity of the *improver* across generations.
- **T3 Eval rigor / Goodhart** ([sweep](sweep/eval-rigor.md)).
  - Selection-noise and gate papers: Winner's Curse 2610.09239, TRUST 2611.10787, Masked Erosion 2610.38392.
  - Hacking-transfer papers: 2611.12865 shows shortcut training transfers for weights, and 2611.08318 asks whether a verifier was learned in embodied settings.
  - Recurring gap: no tests of whether *harness-level selection* learns generic exploit-seeking.
- **T4 Curricula** ([sweep](sweep/curriculum.md)).
  - Harness-learner curricula exist now (ActiveSaddler, GEPA-Curator 2610.37651, DAEDALUS 2610.08048, RoboACE 2611.13034).
  - Weight-side Goodhart is documented (Anchored Self-Play, False Frontiers).
  - Open: a per-generation proxy–truth gap when a frozen-weight meta-agent edits its own task writer.
- **T5 Harness↔weights** ([sweep](sweep/harness-evolution.md)). Saturated with methods. Open problems are measurement ones: upgrade survival (2608.10694 vs 2609.00468 disagree), and oversight after distillation.
- **T6 World models / interventions** ([sweep](sweep/world-models-xunjian.md)).
  - Simulator-scored self-improvement went mainstream in 2026.
  - Simulator collapse is shown for weight RL: 2608.12253, and 2611.01139 "model-blind collapse".
  - JEPA work (2610.19375) shows one-step fidelity is a poor proxy for planning.
  - No one sweeps evolution depth × simulator fidelity for harness evolution.
- **Benchmarks / failures** ([sweep](sweep/benchmarks-failures.md)) and **trends** ([sweep](sweep/frontier-trends.md)). A standalone self-improvement benchmark would be late: SelfImprove-Bench 2611.02797, Δ-τ-Bench, GRADUS, AutoGym and others already exist.

## Papers that most threaten novelty
| Paper | Threatens | Gap it leaves |
|---|---|---|
| 2610.27610 Auditing the Product, Not the Recursion | #1 | No experiments. |
| 2610.08902 Agent Plasticity (Meta FAIR) | #1 decay arm | Actor-level only. Improver-strategy transfer is its stated future work. |
| 2609.06396 MetaRSI / 2610.21791 RSIFlow / 2610.09197 FreeEvolve | #1 effect arm | One state, one off-diagonal cell, no seeds. |
| 2611.08318 / 2609.17817 / 2610.12822 | Learning-to-Goodhart | Task-level only, or adversarial, or weights only. No held-out-family dose-response for improvers. |
| 2611.13034 RoboACE / 2609.39102 False Frontiers | Curriculum Goodhart | Start-vs-end only, or weight RL. |
| 2608.12253 / 2611.01139 | Evolve-in-dream | Weight RL against a simulator, with no harness evolution and no fidelity sweep. |

---

## Next two weeks (~$190)
- **Days 1–3:** build a shared procedural τ-world and a budget-accounting runner (logs tokens and dollars per arm). All pilots reuse it.
- **Pilot A (merged paper, ~$80):** Gödel-lite with a 27B improver and 9B executor. Evolve 10 generations, then run the transplant cells.
  - **Manipulation check:** do generation-0 and generation-10 differ at all at matched dollars?
  - **Go:** more than 3pp difference with non-overlapping bootstrap CIs.
  - **Pivot:** if checkpoints are indistinguishable, switch to a stronger improver.
- **Pilot B (Learning to Goodhart, ~$50):** a code sandbox with planted artifacts. Check whether evolution on hackable evaluators exploits the *seen* artifact family; this is the manipulation check.
- **Pilot C (Evolve-in-dream, ~$50):** a τ-retail simulator with injected quirks. Measure on-path ranking fidelity.
- **~Nov 26:** decide with Xunjian. Pre-register by Dec 20, aim for a preprint in Feb–Mar 2027, then NeurIPS in May.
- **Combining:** #1 is the lead. Learning-to-Goodhart or Evolve-in-dream is a second paper if Pilot A is flat. The procedural τ-world is shared infrastructure for all of them.

## Round-2 search list (gaps round 1 missed)
1. **ICLR 2027 OpenReview submissions (Oct 2026)** were never searched. This is the most likely place for an unseen scoop.
2. **About 15 extra leads were never checked:** Intrinsic Metacognitive Learning (ICML'25), Held-Out Selection, Group-Evolving Agents, Solo-to-Social, Evolving Agents in the Dark, MetaSkill-Evolve, Faithful Self-Evolvers, PAST-Bench, S3Gym, VibeTraining-Bench and others.
3. **SelfImprove-Bench 2611.02797:** conflicting numbers (1.433 vs 1.326). Re-read the paper.
4. **Verify "open models cannot drive Hyperagents"** (attributed to 2609.17817). Four proposals depend on it.
5. **Find IDs for ID-less items:** "When, Where, and How Experience Becomes Structure" and "Scaling Discovery Does Not Require Forgetting (verified shared archives)".
6. **Re-check the top 3 ideas weekly** against new arXiv submissions with alphaXiv recency search, especially follow-ups to 2610.27610 and Agent Plasticity.

## Caveats
- Most 2026 entries are **unreviewed preprints**.
- arXiv IDs come from agent tool results (alphaXiv/S2), but a few were auto-extracted in `LITERATURE.md` Part 2. **Verify every ID before citing.**
- Scores are model judgments under the rubric. Treat them as a ranking aid, not ground truth.
- Running ~95 minutes in round 1 was faster than planned. Round 2 should be the targeted verification pass above.

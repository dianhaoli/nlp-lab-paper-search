# Critique: Curriculum Goodhart. When a meta-agent writes its own tasks

Idea id: `curriculum-goodhart` · Topic T4 (curricula and task generation for harness learners) · Reviewed 2026-11-12

## 0. Verdict up front
**Total ≈ 71/100. Viable with fixes, and close to the "strong" line once reframed.** No kill flag fires cleanly, but two are close.

- **K1 (scoop).** The *phenomenon* is already published for **weight learners**. Anchored Self-Play (2607.03523) shows self-play drifting toward "hard but unrealistic" bugs: synthetic scores improve while human-bug scores degrade, and reference anchoring fixes it. False Frontiers (2609.39102) shows in-loop reward rising while external correctness stagnates or declines, and the gap worsens over rounds. The *design* of a frozen-weight agent co-evolving its own curriculum harness and execution harness now exists too, in RoboACE (2611.13034, 9 Nov 2026), but it is run as a method paper with no proxy-vs-truth tracking. So the open cell is narrow and real: **a frozen-weight, self-referential harness learner that edits its own task writer, measured against exact ground truth over many generations.**
- **K2 / "obvious" risk.** If the meta-agent can edit the writer *and* is scored on that writer's tasks, it will write easy tasks, and reviewers will say "of course". The interesting version must keep the safeguards real systems use (validity checks, solve-rate bands, learning-progress objectives) and show Goodhart **survives them**.
- **Pre-stated numbers.** The claim fixes the numbers in advance ("peaks around gen 10", "≤5% anchoring"). Do not; the result may well be null or non-monotone.

## 1. Scoop search (what was run)
- **alphaXiv discover (recency) ×5.**
  1. Self-improving agents writing their own tasks, where the proxy rises and truth falls.
  2. Meta-agent co-evolving harness and task writer, with drift and anchoring.
  3. Proposer–solver co-cheating and drift toward unrealistic tasks.
  4. Harness optimization on self-generated practice tasks vs real held-out tasks.
  5. Hyperagents-style self-referential agents editing their own task distribution.
- **S2 keyword ×4.**
  1. "self-generated tasks goodhart self-improving agent"
  2. "curriculum drift self-play unrealistic tasks proxy true performance"
  3. "agent writes its own tasks generator co-evolution collapse"
  4. "self-play task generator reward hacking held-out divergence LLM"
- **S2 lookups and forward citations.**
  - Resolved the synthesizer's IDs: 2610.38882 is *Aligning Autoresearch: Co-Improvement between Agents and Humans* (Meta FAIR, HackBench; only tangential). 2609.39102 is *False Frontiers*.
  - Forward citations of HERA (1 hit), Anchored Self-Play (3 hits: 2610.26416, 2610.22791, 2604.20209), False Frontiers (3 hits, unrelated), RSI-Env (0) and Hyperagents (60 citing papers, filtered by title: RSIAgent, MetaShare, QD-Rec; none runs this design).
- **Deep reads (answer_pdf_queries):** RoboACE 2611.13034, RRI 2610.21040, RSI-Env 2611.01736, HERA 2610.06563. Abstracts read for False Frontiers, Evolving Together/Failing Apart (CODA), Training Stability under Adaptive Curricula, A-RSI, Self-Play False Belief, Recursive Harness SI for Data Synthesis, SelfSearch, RuHarness and Artifact-to-Capability.

### Closest papers (all seen in tool results)
| Paper | Date | Overlap | Gap left |
|---|---|---|---|
| **RoboACE: Adversarial Co-Evolution for Robot Self-Improvement** (2611.13034) | 2026-11-09 | **Structurally the "unified" arm with frozen weights.** A curriculum harness and an execution harness co-evolve around frozen GPT-6.1 and π0.5, for 27 rounds. An admission gate checks validity and reachability, a "Benchmark Curator maintains core anchors", and the curriculum explicitly mixes in "familiar conditions that expose regressions". | One campaign, one seed. Reports only start-vs-end transfer (LIBERO 63.5 to 99.0). **No per-round proxy-vs-held-out curve, no drift, validity or distance-to-real tracking, no arm without anchors.** It is a method paper. Its anchors are an untested design choice, and that is exactly what Dan's anchoring arm would test. |
| **Anchored Self-Play for Code Repair** (2607.03523) | 2026-07-03 | **Same claim and mechanism, for weights.** Self-play drifts toward hard but unrealistic bugs, improving synthetic scores and degrading human-bug scores. Anchoring with reference bugs fixes it. | Weight learner, code repair only. Does not ask whether a low-capacity, frozen-weight harness learner Goodharts at all, or how fast. |
| **False Frontiers: Co-Cheating in Self-Evolving Search Agents** (2609.39102) | 2026-09-30 | **Same "proxy up, truth flat or down, worsening over rounds" curve**, measured by a post-hoc audit against source evidence. Proposes the CrossFit mitigation. | Weight RL (Qwen3.5-4B and 9B). The mechanism is shared pseudo-label errors, not drift toward unrealistic tasks. The writer is a trained proposer, not self-edited code. |
| **RRI: Recursive Reward Improvement via Adversarial Meta-Prompt Evolution** (2610.21040) | 2026-10-15 | A frozen meta-optimizer rewrites the **proposer's task- and rubric-writing prompts**, with drift checks against benchmark corpus statistics. Shows that about 99% of naive self-written rubrics are hackable. | The solver is trained by GRPO, so this is a weight learner. Reports final checkpoints only, not a per-generation proxy–truth gap. |
| **HERA** (2610.06563) | 2026-10-05 | Harness co-evolution with failure-targeted task generation, plus static and H0-failure generation baselines. | Six rounds, one run, a frozen LLM generator. Fixed validation set used only as a gate. **Reports no gap dynamics**, and co-evolution looks monotone-good within 6 rounds. |
| **RSI-Env** (2611.01736) | 2026-11-01 | Under Open-General and Capability-Directed settings, the Self improver builds its own local validation. Gains on targets coexist with regressions elsewhere. | Single runs, 10 rounds, small benchmarks (AIME has 30 items). No measured gap between self-made validation and truth. |
| Recursive Harness SI for Data Synthesis (2610.03548) | 2026-10-02 | The task-writer *harness* evolves for 14 rounds, and solver accuracy falls from 100% to 54.8% (by design). | Optimizes difficulty. Never tests whether harder self-made tasks stop helping a learner. |
| Evolving Together, Failing Apart / CODA (2610.20845) | 2026-10-14 | Co-evolved skill and policy pairs grow more fragile as training proceeds: worst-case drift costs 5–13 points. | A skill-bank drift audit, not a curriculum proxy–truth gap. |
| Training Stability under Adaptive Curricula (2611.08787) | 2026-11-05 | UED students stagnate and degrade over long horizons. | RL weights and learner pathologies; nothing on the generator. |
| Winner's Curse in Self-Improvement Loops (2610.09239) | 2026-10-07 | A selection-noise model of keep-if-better loops. | Covers a fixed eval set only. A self-written eval set adds a *moving* noise and bias term, which is a natural extension. |

**Novelty bottom line.** The Goodhart curve plus anchoring rescue is known for weights (2607.03523, 2609.39102, and R-Few in the reading list). The frozen-weight unified design exists but nobody has measured it (RoboACE). So the contribution has to be the **measurement science** and the **substrate contrast**: does a harness learner Goodhart at all, how fast, through which channel, and can it be detected without real labels? "Showing the Goodhart curve again" is not enough.

## 2. Mock NeurIPS review (harsh but fair)
**Summary.** The paper studies whether a self-improving meta-agent that edits both its harness and its task writer drifts into a proxy–truth gap. It uses a procedural world with exact ground truth and five arms.

**Strengths**
- The question is clean and directly answers Hyperagents' stated future work (a fixed task distribution).
- Exact ground truth from a procedural generator removes the "held-out set too small" problem that plagues HERA (60 pairs) and RSI-Env (30 AIME items).
- The arm ladder (fixed real → static gen → adaptive frozen writer → unified → unified + anchor) isolates *who controls the writer*. That variable is the novel one.
- Cheap, frozen weights, open models.

**Weaknesses**
1. **Incremental relative to the weight-learner literature.** Anchored Self-Play already reports drift toward unrealistic tasks and an anchoring fix. False Frontiers already reports proxy-up and truth-down growing over rounds. Without a result that *differs* for harness learners (onset, rate, channel, or the absence of Goodhart), this is "known phenomenon, new substrate".
2. **Risk of a trivial result.** If the unified agent is rewarded for the proxy score on its own tasks, writing easy tasks is optimal and the gap is guaranteed. Reviewers will call it a strawman. The writer's objective must be a realistic one (frontier band, learning progress, validity gate, as in RoboACE, HERA and DAEDALUS), and Goodhart must be shown *despite* it.
3. **Mechanism claimed, not identified.** "Drift toward harness-solvable unrealistic tasks" needs causal tests. Distance-to-real correlating with the gap is not enough. Candidate channels to separate:
   - (a) invalid or mis-specified tasks,
   - (b) distribution shift away from the real rule mix,
   - (c) harness–writer co-adaptation (tasks phrased in the harness's idiom),
   - (d) selection noise / winner's curse on a self-written gate.
4. **External validity.** A single procedural world invites "toy". At least one real benchmark replication (τ²-bench or AppWorld slice) is needed for the main arms.
5. **Single meta-agent family, and open-model feasibility.** 2609.17817 reports that open models cannot drive Hyperagents. If the unified arm needs a frontier model, cost and K3 risk rise. Two improver families are needed (Hyperagents-style self-referential, plus GEPA/Meta-Harness-style with a separately editable writer).
6. **Matched compute.** The unified arm spends tokens on writer edits. Arms must be matched in total rollouts and tokens, or the "peak then decline" may just be budget reallocation.
7. **Pre-specified numbers** ("gen 10", "5%") read as hypothesis-fitting. State them as estimands with CIs.

**What would make reviewers say what**
- "Engineering": a new co-evolution system with a Goodhart plot.
- "Incremental": Anchored Self-Play for harnesses.
- "Workshop": one world, one improver, three seeds.
- "Accept": a result that differs between harness and weight learners, plus a label-free early-warning detector that generalizes across worlds and improvers.

**Likely score as written:** 4–5 (borderline reject). **With the fixes below:** 6 (weak accept). Confidence 4.

## 3. Rubric scores
| Dim | Score | Reason |
|---|---|---|
| N (×3) | 3 | Two or more close 2026 papers make the same claim for weights (2607.03523, 2609.39102). RoboACE runs the frozen-weight unified design without measuring it. The measurement on harness learners is still open, but the gap is narrow. |
| S (×2.5) | 3.5 | A Goodhart curve is the right shape of surprise, but it is expected given the weights results. A null ("harness learners don't Goodhart") is publishable only if well powered and contrasted with a weight arm. |
| R (×2) | 4.5 | Self-referential meta-agents, long-horizon self-improvement, Hyperagents' future work, the Gödel lineage. Fits Xunjian's agenda squarely. |
| C (×2) | 3 | As written: one world, one improver, number-fitted claim, no compute matching, mechanism only correlational. Can reach 4 with the fixes. |
| F (×1.5) | 3.5 | $800–1.5k is plausible with cheap open models. Risks: open models may not drive a Hyperagents-style unified agent, and 20–40 generations × 5 arms × ≥5 seeds is heavy engineering for one undergrad. |
| T (×1) | 4.5 | Self-play and RSI Goodhart, environment generation and harness evolution are all hot in Sep–Nov 2026. Still slightly ahead for the frozen-weight unified case. |

**Total = 20 × (3·3 + 2.5·3.5 + 2·4.5 + 2·3 + 1.5·3.5 + 1·4.5) / 12 = 20 × 42.5 / 12 ≈ 70.8**

**Kill flags**
- **K1:** not fired, but on watch. The same central claim with the same *design* does not exist. The same claim exists for weights, and the same design exists without the measurement (RoboACE).
- **K2:** on watch. If the writer's objective is the proxy itself, the result is trivially guaranteed. Fix by using realistic writer objectives.
- **K3:** not fired, on condition that a ≤30B open model or a cheap API model can drive the unified agent. Pilot this first.
- **K4:** on watch. "Unified = co-evolve harness + writer" is a combination. This is acceptable only because the contribution is measurement and mechanism, not the system.

## 4. Fixes (ordered by score impact)
1. **Make the substrate contrast the headline.** Add one cheap **weight-learner arm** with the same world and the same writer (a LoRA on a 1–4B model, R-Zero/Absolute-Zero style, about $100–300 of GPU, or Tinker). This gives a 2×2 of {harness learner, weight learner} × {frozen writer, self-edited writer}. The claim becomes "who controls the writer matters more than, or differently from, what the learner is". Either outcome is publishable: harness learners Goodhart slower, faster, or not at all. That raises S and N.
2. **Realistic writer objectives, not proxy maximization.** Give the unified agent the safeguards used in practice: validity gate, solve-rate band, learning-progress reward (as in RoboACE, HERA and DAEDALUS). Report Goodhart *conditional on each safeguard*. The "safeguard ablation" is itself a finding (which safeguards fail first). This kills the strawman critique.
3. **Causal channel decomposition using the exact ground truth.** Per generation, split the gap into four terms with oracle interventions:
   - (a) invalid-task share: replace invalid tasks with valid twins;
   - (b) rule-mix shift: resample tasks to the real rule histogram;
   - (c) co-adaptation: evaluate harness k on writer-j tasks in a **cross-generation tournament matrix**, POET-style;
   - (d) gate selection noise: replay with a noiseless oracle gate, which links quantitatively to the 2610.09239 model.

   This turns a correlation into a mechanism.
4. **A label-free early-warning detector as the reusable artifact.** Predict the true-performance peak from signals the agent *can* see: distance-to-real of generated tasks, the off-diagonal decay of the cross-generation matrix, task-diversity entropy and writer-edit rate. Evaluate it as an early-stopping rule (regret vs oracle stopping) on held-out worlds and improvers. That is the "general insight" piece NeurIPS wants.
5. **Anchoring dose-response, not a single 5% point.** Use {0, 1, 2, 5, 10, 25}% real, at matched compute. Compare with a *passive* anchor (real tasks in the pool) vs an *active* anchor (real tasks only in the gate). RoboACE's "core anchors" cites a need for this.
6. **Two worlds plus one real benchmark.** Use the procedural τ-style world plus a second procedural family (e.g., grid/logic or API-composition). Replicate the main arms on a τ²-bench or AppWorld slice with a fixed held-out split, even at 3 seeds.
7. **Two improver families and matched compute.** Use a Hyperagents-style self-referential agent and a GEPA/Meta-Harness-style evolver with a separately editable writer module. Match total rollouts and tokens per arm, and report anytime curves rather than final points.
8. **Week-one go/no-go pilot (<$50).** First, can a Qwen3.x-30B or DeepSeek-Flash-class model drive the unified loop for 10 generations without breaking? Second, does the static-generated arm show *any* proxy–truth gap? If not, pivot the framing to "harness learners are Goodhart-resistant" and lean on the weight-learner contrast.
9. **Drop the hard-coded numbers.** Pre-register estimands instead: peak generation g*, post-peak slope, gap at g = 30, and the minimum anchor fraction that keeps the slope ≥ 0, each with bootstrap CIs over ≥5 seeds.

## 5. Sharpened headline claim
> *"Who writes the curriculum decides whether self-improvement Goodharts. When a frozen-weight meta-agent is allowed to edit its own task writer, its self-measured progress decouples from exact ground-truth performance even under standard validity and learning-progress safeguards. The decoupling is driven mainly by harness–writer co-adaptation, visible as off-diagonal decay in a cross-generation tournament matrix, rather than by invalid tasks. It appears [earlier/later] than in an otherwise identical weight learner, a label-free drift signal predicts the true peak within k generations, and a small dose of real-task anchoring in the acceptance gate (not the training pool) removes it."*

Brackets mark the outcome the experiment decides. Either direction is reportable.

## 6. Positioning sentence for related work
"Anchored Self-Play (2607.03523), False Frontiers (2609.39102) and RRI (2610.21040) document curriculum drift and proxy–truth decoupling for *weight* learners. HERA (2610.06563) and RoboACE (2611.13034) co-evolve tasks with *frozen-weight harnesses* but report only end-point gains. We give the first per-generation, ground-truth measurement of curriculum Goodhart in self-referential harness learners, decompose it causally, and contrast it with a matched weight learner."

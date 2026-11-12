# Who Writes the Curriculum? Measuring Curriculum Goodhart in Self-Improving Meta-Agents

**Proposal for NeurIPS 2027 (main track).** Dan, advised by Xunjian Yin. Draft of 2026-11-12. Idea id `curriculum-goodhart`, topic T4 (curricula and task generation for harness learners).
This draft revises the idea after its 71/100 critique (`critiques/curriculum-goodhart.md`). The critique's central point was that the phenomenon is already known for weight learners. The contribution therefore has to be **measurement science plus a substrate contrast**, not "another Goodhart curve".

New targeted literature pass (2026-11-12). I ran alphaXiv discovery (recency) on:
- self-written curricula for harness learners;
- label-free early warning of self-improvement collapse;
- harness vs weight learners on the same curriculum.

I also did deep reads (`answer_pdf_queries`) of five papers: **RuHarness 2610.36826**, **AnchorLoop 2610.09856**, **A-RSI 2610.38568**, **Beneath the Surface 2611.07624** and **ActiveSaddler 2610.00906**. S2 lookups confirmed the IDs of Winner's Curse 2610.09239, Anchored Self-Play 2607.03523 and DarwinX 2608.07545. S2 forward-citation calls for Anchored Self-Play, RoboACE and Winner's Curse came back empty, so those papers have no indexed follow-ups yet.

Five relevant papers are new relative to the critique. None runs this design: none measures a per-generation proxy–truth gap for a harness learner that edits its own task writer. Section 4 gives the details.

---

## 1. Title and headline claim

**Title.** *Who Writes the Curriculum? Curriculum Goodhart in Self-Improving Meta-Agents, Measured Against Exact Ground Truth*

**Headline claim (expected outcome, "the writer decides").**
> When a frozen-weight meta-agent may edit its own task writer, its self-measured progress decouples from exact ground-truth performance, **even under the safeguards real systems use** (executor validity gate, solve-rate band, learning-progress acceptance). The decoupling is driven mainly by **harness–writer co-adaptation**, visible as a diagonal band in a cross-generation tournament matrix (harness *k* on writer-*j* tasks), not by invalid tasks. It does not appear when the same writer is frozen. Whether it starts earlier or later than in a weight learner trained on the identical writer is the 2×2 question the paper settles. A label-free drift signal predicts the true-performance peak, and a small real-task anchor placed in the **acceptance gate** (not the training pool) removes the decoupling.

**Alternative claim (also publishable, "harness learners are Goodhart-resistant").**
> Suppose the realistic-safeguard unified arm shows no reliable decoupling over 30 generations, while (i) the naive-objective arm does (positive control) and (ii) the weight learner on the same writer does. Then the paper reports that **low-capacity, frozen-weight learners resist curriculum Goodhart**. Their harness cannot memorize writer idiosyncrasies the way LoRA weights can, so for harness evolution the self-written curriculum is safe *given* the safeguards we identify. That claim is useful to the Hyperagents / RSI-Env / RoboACE line, which is already running self-written curricula without measuring this.

**Third outcome (also publishable, "the substrate does not matter").**
> If harness and weight learners Goodhart at the same onset and rate, the paper reports that curriculum Goodhart is a property of the **writer–gate loop, not the learner**. The decomposition (gate noise vs mix shift vs co-adaptation) then tells builders which safeguard to invest in, whatever the substrate.

**Deliverables in every outcome:**
1. Two procedural worlds with an unlimited exact-truth oracle, released with a `WriterAPI`, so any self-improver can plug in a task writer.
2. The proxy–truth protocol: pre-registered estimands, a tournament matrix and an oracle-intervention decomposition.
3. A label-free early-stopping detector evaluated on held-out worlds and improvers.

---

## 2. Motivation and why now

- **Self-written curricula are becoming the default in RSI systems, without truth tracking.**
  - Hyperagents (2603.19461) lists a fixed task distribution as its main limitation and future work.
  - In the last six weeks three systems shipped the self-written version:
    - RSI-Env (2611.01736): the improver builds its own local validation.
    - RoboACE (2611.13034, 9 Nov): a curriculum harness and an execution harness co-evolve around frozen models for 27 rounds, with "core anchors".
    - Recursive Harness SI for data synthesis (2610.03548): the task-writer harness evolves for 14 rounds.
  - All three report start-vs-end numbers only, with one run each.
- **The weight-learner literature already documents the failure.**
  - Anchored Self-Play (2607.03523): self-play drifts to unrealistic bugs and real-bug performance degrades; anchoring fixes it.
  - False Frontiers (2609.39102): in-loop reward rises while external correctness stalls, and the gap grows over rounds.
  - RRI (2610.21040): ~99% of naive self-written rubrics are hackable.
  - AnchorLoop (2610.09856, new): a frozen previous-executor anchor is needed to keep Agent0-style curriculum–executor loops improving.

  All of these train weights. Whether a frozen-weight harness learner, whose capacity to absorb writer idiosyncrasies is far smaller, fails the same way is unknown. People are building such systems now.
- **Harness-evolution evaluation has turned rigorous, but only for fixed task sets.**
  - Rethinking the Evaluation of Harness Evolution (2607.12227): matched budget, held-out gain ≈ 0.
  - Beneath the Surface (2611.07624, new): 16 evolved agents all gain and lose tasks, and the highest-scoring agent is not the best parent.
  - Winner's Curse (2610.09239): selection noise in keep-if-better loops.

  All of these assume the evaluation set is fixed. A *self-written* evaluation set adds a moving bias term that nobody has quantified. This is the natural next step for the matched-compute style Dan likes.
- **Curriculum is now recognized as a harness-optimization dimension, but only with a frozen curriculum policy.**
  - ActiveSaddler (2610.00906, new) treats scenario selection as a non-stationary bandit, with +4.4/+7.5 pp test gains.
  - RuHarness (2610.36826, new) co-evolves the *diagnostic rubric* with the harness and finds that a stale rubric is worse than no rubric.

  Both select from a fixed human pool or keep the task objective fixed. Neither lets the meta-agent rewrite the task distribution, so neither can Goodhart in the way we study. They make good "adaptive frozen writer" baselines.
- **Exact ground truth is the missing instrument.** HERA uses a 60-pair held-out set, RSI-Env uses AIME's 30 items, and ActiveSaddler's TB2 test split has 40 tasks. A procedural world whose reference distribution D\* can be sampled without limit is what turns "looks monotone-good in 6 rounds" into a measured curve with CIs.
- **Plasticity framing.** Agent Plasticity (2610.08902) defines held-out gain per unit experience. Curriculum Goodhart is the case where plasticity measured on the agent's own curriculum diverges from plasticity measured on D\*. This paper carries Dan's plasticity interest up to meta-agents: a meta-agent that writes its own experience can manufacture apparent plasticity.

---

## 3. Core research questions

1. **RQ1 (existence and substrate).** In the 2×2 {harness learner, weight learner} × {frozen writer, self-edited writer}, at matched compute, does the proxy–truth gap grow with generation? Pre-registered estimands:
   - peak generation g\*,
   - post-peak slope β,
   - gap at g = 30 (G₃₀).

   Is the effect of *who controls the writer* larger than, smaller than or interacting with *what the learner is*?
2. **RQ2 (which safeguards fail).** Does Goodhart survive realistic writer safeguards (validity gate, solve-rate band, learning-progress acceptance, alone and stacked)? Which safeguard is defeated first, and how?
3. **RQ3 (mechanism).** Using oracle interventions on exact ground truth, how much of the gap comes from each channel?
   - (a) invalid or mis-specified tasks,
   - (b) rule-mix shift away from D\*,
   - (c) harness–writer co-adaptation,
   - (d) gate selection noise (winner's curse).
4. **RQ4 (detection and repair).**
   - Can label-free signals the agent can compute predict g\* well enough to serve as an early-stopping rule on *held-out* worlds and improvers?
   - What is the dose-response of real-task anchoring (0–25%)?
   - Is an anchor in the acceptance gate (active) more efficient than one in the training pool (passive)?

---

## 4. Related work and positioning

| Paper (ID, date) | What it does | Gap left / how we differ |
|---|---|---|
| **RoboACE** (2611.13034, Nov 9) | Frozen GPT-6.1 / π0.5. A curriculum harness and an execution harness co-evolve for 27 rounds. An admission gate checks validity and reachability, and a Benchmark Curator maintains "core anchors". | One campaign, one seed, start-vs-end only (LIBERO 63.5→99.0). No per-round proxy-vs-held-out curve, no arm without anchors, no drift tracking. **Our unified+anchor arm is its design, measured. Our no-anchor and dose arms test the design choice it never ablates.** |
| **Anchored Self-Play for Code Repair** (2607.03523, Jul 3) | A weight learner's self-play drifts to hard but unrealistic bugs. Synthetic scores rise, human-bug scores fall, and anchoring fixes it. | Weights only, one domain, and the writer is a trained model. **We test whether the same holds for a harness learner, with the same writer held fixed across substrates, and decompose the channel.** |
| **False Frontiers** (2609.39102, Sep 30) | Proposer–solver RL (Qwen3.5-4B/9B). In-loop reward rises while audited correctness stagnates or declines, and the gap grows over rounds. The mechanism is shared pseudo-label errors (co-cheating). | Weight RL, and the mechanism is about labels. **Our worlds have exact labels, so channel (a)/pseudo-label error is removed by construction. Any remaining gap must come from distribution or co-adaptation channels, which is a sharper test.** |
| **RRI** (2610.21040, Oct 15) | A frozen meta-optimizer evolves the proposer's task- and rubric-writing prompts. Drift checks use corpus statistics, and ~99% of naive rubrics are hackable. | The solver is trained by GRPO. Final checkpoints only. **We provide per-generation curves, a harness-learner arm, and a validated label-free detector instead of ad-hoc drift checks.** |
| **HERA** (2610.06563, Oct 5) | Harness–environment co-evolution for abstention, with static and H0-failure generation baselines. | Six rounds, one run, frozen generator, 60-pair held-out set. Looks monotone-good. **Our arm ladder extends its baselines to a self-edited writer and 30 generations with unlimited truth.** |
| **RSI-Env** (2611.01736, Nov 1) | An open-ended RSI environment in which the Self improver builds its own validation. Gains coexist with regressions. | Single runs, 10 rounds, tiny benchmarks. The gap between self-made validation and truth is not measured. **We measure exactly that quantity.** |
| **ActiveSaddler** (2610.00906, Oct 1) *(new)* | Automated curriculum for harness optimization: a non-stationary bandit over failure-pattern arms with LLM-estimated learning progress. GAIA2 +4.4 pp, TB2 +7.5 pp over fixed order, matched rollout budget. | Selects from a **fixed human training pool**. The curriculum policy is not edited by the meta-agent, and it cannot invent tasks. Test sets are 300/40 tasks, 3 test executions, and one optimization run. **We use it as the "adaptive frozen writer" baseline (A2) and adopt its learning-progress scoring as safeguard S3.** |
| **RuHarness** (2610.36826, Oct 27) *(new)* | Rubric–harness co-evolution (Qwen3.5-35B-A3B, GLM-4.7-Flash). A static rubric is *worse* than no rubric. Gated by an independent validation set. | Co-evolves the *diagnosis*, but the task objective and task set stay fixed. 10 rounds, 1 run. **Its "stale supervision hurts" finding is the mirror image of our hypothesis that adaptive supervision written by the agent can hurt.** |
| **AnchorLoop** (2610.09856, Oct 7) *(new)* | Agent0-style Curriculum–Executor RL (Qwen3-4B, MiMo-7B). A frozen previous-iteration executor serves as the anchor. Later-iteration gains continue while the unanchored loop plateaus. Generated tasks get harder over iterations (pass 71.8%→39.4%). | Weights, three iterations, a self-anchor rather than a real-data anchor, and no truth-tracking of drift. **Our weight arm is the same kind of loop, extended to 20 generations with exact truth. The self-anchor is an extra anchor type in our dose study.** |
| **A-RSI** (2610.38568, Oct 28) *(new)* | Weight RL with fixed programmatic task templates. A **label-free training-log trigger** (decline in prompts with nonzero GRPO advantage) decides when to refresh the pool. | Detects pool *exhaustion*, not Goodhart. The generator is frozen. Weights only. **We use its trigger as a baseline detector and test whether exhaustion-style signals predict truth peaks when the writer itself drifts.** |
| **Winner's Curse in Self-Improvement Loops** (2610.09239, Oct 7) | A selection-noise model of keep-if-better loops: inflation, lock-in, acceptance rules. | Fixed evaluation set. **Channel (d) of our decomposition is its model. We extend it to a moving, self-written gate and test its quantitative prediction against oracle-gate replay.** |
| **Beneath the Surface** (2611.07624, Nov 5) *(new)* | Four evolvers (AHE, HyperAgents, A-Evolve, GEPA) × two harnesses. Every Champion gains and loses tasks, and the highest-scoring agent is not the best parent. Admission replays. | Fixed task splits (τ³-Banking 97 tasks, TB2 89). No self-written tasks. **It is the methodological template (operator-level replays) for our gate and select interventions, and its τ²-bench transfer protocol is our real-benchmark replication target.** |

**Also relevant but further away.**
- Recursive Harness SI for data synthesis (2610.03548): the writer evolves for difficulty only.
- CODA / Evolving Together, Failing Apart (2610.20845): skill–policy drift.
- Training Stability under Adaptive Curricula (2611.08787): UED student degradation.
- CompassPlay (2609.32228): a weight-learner proposer rewarded by where it moves the solver, a learning-progress objective. I have seen the abstract only.
- *Learning the Task or Learning the Verifier?* (2611.08318): criteria exposure in embodied self-improvement. Abstract only; relevant to channel (c).
- Classic: POET, PAIRED, OMNI/OMNI-EPIC, R-Zero, R-Few, Absolute Zero (reading list).
- DAEDALUS was cited in the critique as a learning-progress writer. **I have not verified it** and will check it before citing.

**Positioning sentence.**
> Anchored Self-Play, False Frontiers, RRI and AnchorLoop document curriculum drift and proxy–truth decoupling for *weight* learners. RoboACE, HERA and ActiveSaddler adapt or co-evolve curricula for *frozen-weight harnesses* but report only end-point gains. We give the first per-generation, exact-ground-truth measurement of curriculum Goodhart when a self-referential meta-agent edits its own task writer. We decompose it causally, contrast it with a weight learner on the identical writer, and ship a label-free detector validated out of distribution.

---

## 5. Method and experimental design

### 5.1 Worlds (exact ground truth)

**W1, procedural τ-world (the main world).** This is Dan's planned infrastructure.
- A task is a tuple (DB state, user goal, active policy rules, user-simulator script). It is generated by a parametric reference generator **D\*** = p(rule mix ψ\*, DB size, #steps, entity types, distractors).
- Success is exact: the final DB state equals the verified end state, which the world's executor computes. No judge is involved.
- There are about 15 policy-rule types (refund windows, membership tiers, multi-item constraints and so on), so D\* has a known rule histogram.
- The user simulator is scripted and deterministic, so solver stochasticity is the only noise.

**W2, procedural API-composition / constraint world.** This is a structurally different second family.
- Tasks are multi-step calendar/file/spreadsheet API programs with hidden constraints and goal predicates. Truth is a predicate check on the final state.
- It is used both as a replication world and as the **held-out world for the detector**.

**R, real-benchmark replication.** We use a τ²-bench slice (retail + airline) with a fixed train/held-out split, following the transfer protocol of 2611.07624. The main 2×2 cells run at 3 seeds. In R the truth eval is the held-out real split: finite, so SE is reported.

**What the writer sees.** In every arm the writer sees a **seed pool S of 30 real tasks** drawn from D\*. In the anchoring arms there is also a separate **anchor reserve of 60 real tasks**. It never sees ψ\*. The writer emits tasks in the world DSL. The world executor checks well-formedness and solvability (validity) and computes gold end states. So invalid tasks can be measured, and they can be filtered when the gate is on.

### 5.2 Learners and improvers

- **Harness learner (H).**
  - Solver: frozen **Qwen3.5-9B**, self-hosted with vLLM. The main arms re-run with **Qwen3.5-27B** at 3 seeds as a model-scale check.
  - Editable harness: system prompt, tool wrappers, a memory/notes file and control code.
- **Improver families (two, per the critique):**
  - **I1, Hyperagents/DGM-H-style self-referential.** One repo holds the solver harness, the **task-writer program** and the meta-procedure. The meta-agent may edit any file, including the writer. The meta-model is Qwen3.5-27B (the pilot checks feasibility), with a Haiku-class API model as fallback.
  - **I2, GEPA/Meta-Harness-style modular evolver.** The harness and the writer are separate modules. Reflective mutation proposes edits to each, and the arm determines whether writer edits are allowed. The meta-model is GPT-mini-class or Qwen3.5-27B.
- **Weight learner (W).** **Qwen3-4B with LoRA, GRPO** on writer tasks with exact rewards (R-Zero/AnchorLoop-style loop, but with a programmatic writer). About 0.5 GPU-h per generation on one H100, for 20 generations.
  - **Key design choice:** the writer is the same harness-level program in both substrates. In the self-edited-writer cells, the I2 meta-agent edits it with the same safeguards and budget. So the only thing that differs across the substrate axis is whether the learner absorbs the curriculum into a harness or into weights.

### 5.3 Arms

**Main ladder.** Every arm runs for each learner. Arms marked † run for H only.

| Arm | Writer | Who edits writer | Pool / gate |
|---|---|---|---|
| A0 Fixed real | none | — | train on S, gate on held-back half of S |
| A1 Static generated | w₀, one-shot batch | nobody | generated pool, generated gate |
| A2 Adaptive frozen writer | w₀ code fixed, conditioned on learner failures (OMNI / ActiveSaddler-style learning-progress selection) | nobody | regenerated each gen |
| **A3 Unified, realistic safeguards (S4)** | w_g | **meta-agent** | writer edits accepted only under S1+S2+S3 |
| A3-naive (positive control) | w_g | meta-agent | writer edit accepted if self-score rises (the strawman, reported as an upper bound only) |
| A4 Unified + anchor† | w_g | meta-agent | real fraction α ∈ {1, 2, 5, 10, 25}% × {passive: in training pool; active: only in acceptance gate}. α = 0 is A3. |

**Safeguards** (writer-side; ablated one at a time in the RQ2 arms):
- **S1** executor validity gate.
- **S2** solve-rate band: keep tasks with the current harness's pass rate in [0.2, 0.8].
- **S3** learning-progress acceptance: a writer edit is accepted only if a harness evolved on the new writer's tasks improves on the *previous* writer's held-back tasks. Cross-writer progress, as in ActiveSaddler / CompassPlay.
- **S4** = S1 + S2 + S3.

**RQ2 safeguard ablation:** A3 with {S1, S2, S3, S1+S2, S4} for H-I1 in W1.

**Compute matching.** The budget unit is per generation:
- *B_roll* = 300 solver rollouts for training plus gate;
- *B_meta* = 250k meta-model tokens.

Writer edits in A3/A4 are paid out of the same B_meta. Frozen-writer arms spend the remainder on extra harness proposals. Truth evaluation sits outside the loop, is never visible to the agent, and is not charged. We report **anytime curves against cumulative cost** as well as against generation, and an extra control **A2+**: A2 with B_meta raised to A3's realized total, in case A3 overspends.

**Generations and seeds.**
- H arms: 30 generations; 5 seeds in the main cells (W1 × I1 × {A0–A3, A3-naive}).
- Ablation and dose cells: 3 seeds.
- W arms: 20 generations, 3–5 seeds.

### 5.4 Metrics (formal)

Let h_g be the accepted harness (or weights) and w_g the writer at generation g, with D_{w} the distribution writer w samples from.

**Proxy, truth and gap.**
- **Proxy** P_g = E_{x∼D_{w_g}}[succ(h_g, x)], estimated on 200 fresh writer tasks not used for training or gating. This is the agent's self-measured score.
- **Truth** T_g = E_{x∼D\*}[succ(h_g, x)], estimated on **500 fresh i.i.d. tasks**, common across arms (common random numbers).
- **Gap** G_g = P_g − T_g.
- **Decoupling** Δ_g = (P_g − P_0) − (T_g − T_0).

**Pre-registered estimands** (bootstrap CIs over seeds, ≥ 5 seeds for main cells):
- g\* = argmax of a 3-gen moving average of T_g;
- β = OLS slope of T on g for g > g\* (Goodhart is declared if β < 0 with its 95% CI excluding 0);
- G₃₀ and Δ₃₀;
- α_min = the smallest anchor fraction with β ≥ 0 (and Δ₃₀ ≤ 2 pp).

**Tournament matrix.** M[k, j] = success of h_k on 100 tasks from w_j, for k, j ∈ {0, 5, 10, …, 30}, plus a D\* column. Two summary indices:
- **Excess-diagonal index** E_g = M[g, g] − M[g−5, g] − (T_g − T_{g−5}). This is the part of the current writer's score improvement over the previous harness that truth does not confirm. It is label-free except for the T term, and we use the label-free variant Ẽ_g = M[g, g] − M[g−5, g] − (M[g, 0] − M[g−5, 0]) in the detector.
- **Off-diagonal decay** ρ_g = slope of M[g, j] in |g − j|.

**Task statistics.**
- Validity v_g.
- Diversity: Vendi score / entropy over DSL feature bins.
- Distance-to-real d_g = MMD between writer-task features and the visible seed pool S. Label-free.
- Writer-edit rate.

### 5.5 Causal decomposition of the gap (RQ3)

At each checkpoint we recompute the proxy under oracle interventions, applied sequentially. Shapley-averaging over the 4! orders gives order-free shares.
- **(a) Invalid:** replace each invalid task with its nearest valid twin (minimal DSL repair) → P^{(a)}.
- **(b) Mix shift:** importance-reweight writer tasks to the D\* feature histogram, using exact p\*(f) and estimated q(f) → P^{(b)}.
- **(c) Co-adaptation:** score the writer-*g* tasks with a *truth-matched foreign harness* (the harness from another seed and lineage with the closest T) → P^{(c)}. What remains is own-lineage co-adaptation.
- **(d) Gate noise:** replay the run's acceptance decisions with a noiseless oracle gate (truth on 1,000 tasks). Compare the resulting trajectory's inflation with the closed-form winner's-curse prediction of 2610.09239 (gate size n, candidates per gen, σ).

### 5.6 Label-free early-warning detector (RQ4)

**Features:** d_g, Ẽ_g, ρ_g, diversity, writer-edit rate, and proxy curvature. All are visible to the agent; none uses D\* labels.

**Model:** a pre-registered threshold rule plus a 3-feature logistic hazard model. It is fit only on W1 × I1 runs.

**Tests (held out):**
1. W1 × I2;
2. W2 × I1/I2;
3. the τ²-bench slice.

**Metric:** stopping regret T_{g\*} − T_{ĝ} and the false-stop rate on non-Goodharting arms (A0, A2).

**Baselines:**
- stop at a fixed g;
- proxy-plateau stop;
- the A-RSI nonzero-advantage trigger (weight arm) and its rollout-variance analogue (harness arm);
- an oracle stop.

### 5.7 Power sketch

**Truth precision.** With 500 truth tasks, per-checkpoint SE ≈ 2.2 pp. With 15 post-peak points the per-run SE of β is ≈ 0.13 pp/gen, so a decline of 0.5 pp/gen is detectable within one seed.

**Primary test.** The test is G₃₀(A3) − G₃₀(A2), 5 vs 5 seeds, Welch t-test.
- If the seed SD is ≈ 5 pp (to be checked in the pilot), an 8 pp difference has d ≈ 1.6 and power ≈ 0.8 at α = 0.05.
- If the pilot SD is > 7 pp, the main cells move to 8 seeds, paid for by cutting the 27B re-run.

**2×2 interaction.** The interaction (substrate × writer control) has lower power. We pre-register it as secondary, and for it we report the CI, not a p-value.

### 5.8 Ablations (summary)

- Safeguard ladder (RQ2).
- Anchor dose × {passive, active, self-anchor à la AnchorLoop}.
- Model scale: 9B vs 27B solver.
- Seed-pool size |S| ∈ {10, 30, 100}: does a richer view of the real distribution slow drift?
- Writer edit frequency: every gen vs every 5 gens.

---

## 6. Expected results and figures

**Figure 1 (the headline).** A 2×2 grid of panels: {harness learner, weight learner} × {frozen writer (A2), self-edited writer (A3-S4)}.
- Each panel plots P_g (dashed) and T_g (solid) against generation, with seed-bootstrap bands, and shades the gap.
- An inset shows the 7×7 tournament matrix for that cell.
- Expected pattern: the bottom-right-style cells (self-edited) show a diagonal band and diverging curves, and the frozen-writer cells track. The comparison between learner rows is the open question.

**Figure 2.** Stacked-area decomposition of G_g into (a) invalid, (b) mix shift, (c) co-adaptation and (d) gate noise, for A3-S4 and A3-naive. Our prior expectation: invalid tasks matter only in the naive arm, and co-adaptation dominates under S4. This is a hypothesis, not a fitted number.

**Figure 3.** Safeguard ladder: β and G₃₀ for each safeguard set, annotated with how each one is defeated. Examples: the band is gamed by tasks that are "hard for a fixable reason"; learning progress is gamed by co-drifting writer pairs.

**Figure 4.** Anchor dose-response: β against α (log-x), passive vs active vs self-anchor. The point to establish is whether α_min(active) < α_min(passive).

**Figure 5.** Detector: stopping-regret CDFs on held-out worlds, held-out improvers and τ²-bench, against the baselines.

**Table 1.** Pre-registered estimands (g\*, β, G₃₀, α_min) with CIs for every cell.

---

## 7. Pilot (≤ 1 week, ≤ $50) and decision criteria

**Days 1–3: build a minimal W1.**
- 8 rule types, executor, reference generator, 30-task seed pool, and a truth sampler with 300 tasks.
- Solver: self-hosted Qwen3.5-9B on one rented GPU (~$15).

**Days 4–6: three tiny runs, 10 generations, 2 seeds each:** A1 (static), A3-naive (positive control) and A3-S1 (validity gate only), all with I1 on a Qwen3.5-27B meta-model (~$25 of GPU/API). Throughout, log P, T, v, d and the edit success rate.

**Day 7: noise floor.** Estimate the seed SD of T₁₀ and the per-checkpoint SE.

| Outcome | Decision |
|---|---|
| The loop runs (≥ 70% of meta-edits apply cleanly), A3-naive shows Δ₁₀ ≥ 5 pp, and A3-S1 or A1 shows any upward trend in Δ or d | **GO.** Full study as designed. |
| A3-naive Goodharts, but A3-S1 is flat, d is flat and T is rising | **GO with the alternative framing** ("harness learners resist; the weight arm is the contrast"). Move the weight arm earlier, since it now carries the surprise. |
| The open meta-model cannot drive I1 (< 50% clean edits, crashes) | **PIVOT the meta-model** to a Haiku-class API model and cut generations to 20. If cost per run exceeds $25, drop I1 to 3 seeds and make I2 (lighter) the main improver. |
| T does not improve in A0 or A1 (T₁₀ − T₀ < 2 pp): the harness learner saturates, so there is nothing to Goodhart | **KILL / merge.** Report "harness learners saturate on procedural τ-worlds" as a section of the main #3 curriculum paper, and switch to τ²-bench or a harder W1 config before abandoning. |
| Even A3-naive shows no gap | The instrumentation is suspect. Debug the proxy estimator first. If it is confirmed, **PIVOT** to the alternative claim. |

---

## 8. Budget and timeline (to mid-May 2027)

**Budget.**

| Item | Estimate |
|---|---|
| Solver rollouts, H arms (self-hosted Qwen3.5-9B; about 23k rollouts per 30-gen run incl. truth and tournament; ~130 runs) | ~$600 (about 300 H100-h at ~$2/h) |
| Meta-model tokens (Qwen3.5-27B self-hosted for most runs; Haiku/GPT-mini-class API for I1 main cells) | ~$700 |
| Weight arm (Qwen3-4B LoRA GRPO, 20 gens × ~0.5 GPU-h, ~14 runs) | ~$300 |
| 27B-solver re-run of main cells (3 seeds) | ~$200 |
| τ²-bench replication (main 2×2 cells, 3 seeds, 15 gens) | ~$300 |
| Contingency (20%) | ~$400 |
| **Total** | **≈ $2.5k** (within the ≤ $3k cap) |

The **minimum viable tier** costs **≈ $1.3k**. It keeps W1 at full scale, W2 at 3 seeds and the weight arm at 3 seeds, and drops the 27B re-run and the |S| ablation.

**Timeline.**

| Month | Work |
|---|---|
| **Nov 2026** (from 12 Nov) | Pilot (§7). Build W1 fully. Pre-register estimands and the detector rule on OSF or a git tag. |
| **Dec** | I1 + I2 integration. Main W1 harness cells (A0–A3, A3-naive) at 5 seeds. Tournament-matrix tooling. |
| **Jan 2027** | Weight arm (W1). Safeguard ladder. Anchor dose study. First Figure 1 draft. Mid-point check with Xunjian: is the substrate contrast real? |
| **Feb** | Build W2. Replicate main cells. Oracle-intervention decomposition (Fig. 2). Fit the detector on W1-I1 only (frozen afterwards). |
| **Mar** | τ²-bench replication. Detector evaluation on held-out worlds and improvers. 27B re-run. |
| **Apr** | Writing. Robustness checks (|S|, edit frequency). Internal review with Xunjian and Shuyan. Release code, worlds and protocol. |
| **May 1–15** | Polish, appendix, reproducibility checklist, submission. Buffer for one extra seed batch. |

---

## 9. Risks, mitigations, and reviewer objections

**Risks.**

| Risk | Mitigation |
|---|---|
| Open models cannot drive a Hyperagents-style unified loop (2609.17817 reports this) | The pilot tests it in week 1. I2 is lighter and modular. Haiku-class fallback is budgeted. |
| Harness learner saturates early, so there is no curve | A harder W1 configuration (more rule types, longer horizons). τ²-bench as backstop. The saturation result itself is a finding for the #3 curriculum paper. |
| Engineering load for a solo undergrad | Reuse open code: Hyperagents, GEPA, τ²-bench harness, and VeRL/Tinker-style GRPO for the weight arm. Freeze scope after Jan. W2 and the 27B re-run are the first cuts. |
| Toy-world criticism | Two structurally different worlds plus τ²-bench, and the detector is validated across them. |
| Scoop (RoboACE or HERA follow-up adds truth tracking) | Weekly alphaXiv watch. Our distinct pieces (2×2 substrate contrast, decomposition, OOD detector) survive a follow-up that only adds curves. |

**Anticipated reviewer objections.**

1. *"Of course the agent writes easy tasks if it is scored on them."*
   - The main arm uses realistic safeguards (validity, band, learning progress).
   - The naive arm is shown only as a positive control.
   - The headline is Goodhart *conditional on safeguards* plus which one fails first.
   - S2 rules out "easy tasks" outright: tasks must be at a 20–80% pass rate.
2. *"Known for weights (Anchored Self-Play, False Frontiers)."*
   - We hold the writer fixed and vary only the substrate. No prior paper does this.
   - Our exact labels remove the pseudo-label channel that drives False Frontiers.
   - Any of the three outcomes in §1 is new information.
3. *"Correlational mechanism."*
   - The decomposition rests on oracle interventions, possible only because we have D\*, and it is Shapley-averaged over orders.
   - The tournament matrix with a truth-matched foreign harness isolates co-adaptation.
4. *"Compute confound: A3 spends budget on writer edits."*
   - Matched B_roll and B_meta per generation.
   - A2+ control with A3's realized spend.
   - Anytime curves against cumulative cost.
5. *"The detector is overfit."*
   - Fit on one world × improver and frozen before held-out evaluation, with pre-registered thresholds.
   - False-stop rate reported on non-Goodharting arms.
6. *"Numbers like 'gen 10' and '5%' look fitted."*
   - The draft no longer states any target numbers. All quantities are pre-registered estimands with CIs.
7. *"Procedural τ-world ≠ real users."*
   - The τ²-bench replication.
   - Distance-to-real measured against the visible seed pool, exactly as a practitioner could.

---

## 10. Fit with Xunjian's agenda, and what Xunjian and Shuyan bring

- **Gödel Agent (2410.04444).** In Gödel Agent the agent rewrites its own logic. Here it also rewrites *the distribution it is judged on*, the next natural self-referential degree of freedom. The paper asks what self-reference costs when the self includes the test writer. That is directly the long-horizon reliability question in Xunjian's agenda.
- **World models.** The task writer is the agent's **world model of its deployment distribution**. Distance-to-real is that world model's error, and curriculum Goodhart is world-model drift under self-optimization. W1 and W2 are exact world models that make the error measurable. This framing should appeal to Xunjian's world-model interest and gives the discussion section a general lesson.
- **BreakingWeb (2609.35814).** It builds challenging tasks through *controlled environment interventions* and documents false success claims. Our oracle interventions (invalid repair, mix reweighting, foreign-harness scoring) apply the same controlled-intervention logic to the curriculum, and "proxy success ≠ real success" is the shared theme.
- **WeAct (2610.26847).** Weight switching as an agent action. Our harness-vs-weight 2×2 asks when a self-improver should put learning into weights rather than harness. That is a measured input to WeAct-style lever choice, and an obvious follow-up: let the agent choose its substrate per generation.
- **What Xunjian brings.**
  - Gödel-Agent-style self-modifying loop engineering and experience of what breaks.
  - Framing for the self-referential and world-model narrative.
  - DeepMind-scale perspective on whether the substrate contrast generalizes.
  - Fast iteration on the pre-registration.
- **What Shuyan Zhou brings.**
  - Environment and benchmark construction expertise (WebArena lineage): W1/W2 design, the realism of D\*, and the τ²-bench replication protocol.
  - Evaluation rigor for agent benchmarks.
  - A credible "this world is not a toy" voice for reviewers.

---

## 11. Rubric re-score after fixes

| Dim (weight) | Before | After | One-line justification |
|---|---|---|---|
| N (×3) | 3 | **3.5** | The phenomenon is known for weights, and five more close 2026 papers turned up (ActiveSaddler, RuHarness, AnchorLoop, A-RSI, RoboACE). Still no paper runs the same-writer substrate contrast, a truth-based causal decomposition or an OOD-validated detector for self-edited writers. The gap is real but narrow. |
| S (×2.5) | 3.5 | **4** | The 2×2 makes all three outcomes publishable, and "who writes the curriculum decides" is a one-sentence claim builders would act on. It is not a ranking flip of field-changing size. |
| R (×2) | 4.5 | **4.5** | Self-referential meta-agents, long-horizon self-improvement, the world-model framing and Hyperagents' future work. Squarely on agenda. |
| C (×2) | 3 | **3.5** | Two worlds plus τ²-bench, two improvers, matched compute, pre-registered estimands, interventions and a released protocol. Toy-world skepticism and the low power for the interaction term keep it below 4. |
| F (×1.5) | 3.5 | **3** | A cheap pilot, but more arms (weight arm, dose, decomposition) and ~$2.5k make this heavy for one undergrad in six months. The minimum viable tier ($1.3k) keeps it feasible. |
| T (×1) | 4.5 | **4.5** | Self-written curricula for RSI, harness evolution and plasticity are hot in Oct–Nov 2026. RoboACE and RSI-Env show the wave arriving, and we are slightly ahead of it on measurement. |

**Total = 20 × (3·3.5 + 2.5·4 + 2·4.5 + 2·3.5 + 1.5·3 + 1·4.5) / 12 = 20 × 45.5 / 12 ≈ 75.8**

The total sits at the boundary between viable and strong. Its biggest swing factor is the week-one pilot. If the harness learner saturates, or the open meta-model cannot drive I1, F and S drop and the total falls back to ~68.

**Kill-flag status after fixes.**
- **K1:** not fired. No paper has this design plus measurement.
- **K2:** cleared by the 2×2 and the three-outcome framing.
- **K3:** cleared, conditional on the pilot, with an API fallback.
- **K4:** cleared, because the contribution is measurement, mechanism and a detector, not "we co-evolve both".

# Critique: Breaking the Improver (T6, origin: new)

**Idea.** Evolve harnesses with 3–4 evolvers (GEPA, ACE, AHE, Hyperagents-lite) on clean BreakingWeb train tasks plus a tau procedural set. Then measure the paired intervention drop of the champion against the seed, broken down by primitive and layer. Check whether failures move toward belief failures, and whether a clean+intervention evolution diet recovers robustness at matched compute.

**Verdict.** Viable with fixes, scoring **68.3/100**. No kill flag fires cleanly. Two things are weak. The directional claim ("self-evolution degrades robustness") already has two to three Oct–Nov 2026 precedents in nearby settings. Arm B, as currently written, is close to trivially predictable. The idea is still the most mentor-aligned in the pool, because it builds on BreakingWeb, and nobody has crossed harness evolvers with paired, controlled interventions. The fix is to make the paper about **robustness-aware selection and held-out-family generalization**. "Evolution is brittle" should not be the headline.

---

## 1. Scoop search

**Searches run:**
- alphaXiv discover with recency priority, 4 calls:
  - self-evolving / harness-evolution robustness under environment perturbation
  - BreakingWeb-style controlled interventions for browser and tool agents
  - GEPA/ACE/prompt-optimization brittleness under shift
  - tau-bench / meta-agent under perturbation, and evolution on perturbed tasks
- Semantic Scholar keyword search, 3 phrasings, restricted to papers from 2026-05 on.
- S2 forward citations of BreakingWeb (2609.35814). There is exactly one: EMBERS 2611.03723, defect-guided fuzzing of deep-research systems. It is not about self-improvement.

**Deep reads:** Beneath the Surface (2611.07624), SkillShift-Bench (2610.33528), Appreciation Teaches Unauthorized Execution (2610.39122) and BreakingWeb itself (2609.35814). S2 abstracts for HDA, ReScopeBench, PACE-Bench, Masked Erosion, CMAS, GRACE, Δτ-Bench, AgentPerturb and SelfImprove-Bench.

### Closest papers and the gap each leaves

| Paper | Date | Overlap | Gap left |
|---|---|---|---|
| **Appreciation Teaches Unauthorized Execution: Self-Evolution Degrades Robustness of Personal Agents to IPI** (2610.39122) | 2026-10-29 | **Closest in claim.** Self-evolution (MetaClaw skill evolution, OpenClaw-RL) raises indirect-prompt-injection ASR by 14–49 pp over the base agent. The cause is "authorization conflation": the agent over-generalizes from happy paths. The fix is to add contrastive adaptation episodes (negative cases with positive feedback for *not* executing), which drops ASR from 35% to 5.8%. That is the same logic as Arm B. | Only one intervention type (IPI, a safety framing). Personal-agent skill learning from user feedback, not harness evolvers such as GEPA/ACE/AHE/Hyperagents. No task-success paired drop, no primitives or layers, no matched compute, no evaluation of a held-out intervention family. |
| **Beneath the Surface: What We Overlook When We Talk About Agent Evolution** (2611.07624) | 2026-11-05 | **Closest in design.** Uses the *same evolver set*: AHE, HyperAgents, A-Evolve, GEPA (GPT-5.4; MiniSWE and Codex harnesses; τ³-Banking and TB2). Shows the seed→champion task turnover and cost. Tests transfer to τ²-bench and DeepSWE: coverage falls on 7/8 for general tasks. Shows the "highest-scoring agent ≠ best parent". Proposes AGE/EvoAudit. | Generalization is tested only as cross-benchmark transfer. There are **no controlled, paired environment interventions**, no belief-failure analysis and no evolution on perturbed tasks. Their "generic reminders such as checking tools and verifying results" observation shows that evolvers *do* add verification. That is a risk to your directional hypothesis (see review). |
| **Masked Erosion: How Acceptance Gates Lose Redundant Safeguards in Self-Improving Agents** (2610.38392) | 2026-10-28 | Mechanism scoop. Acceptance gates that score only task success silently delete redundant safeguards. 411 improvement loops under 10 gate rules, plus GEPA runs. This is exactly the "evolved champions drop verification / trust success signals" mechanism. | Safeguards against *harm*, written by hand. It does not cover recovery from environment changes, browser tasks, paired drop, or an intervention diet. |
| **SkillShift-Bench** (2610.33528) | 2026-10-24 | Learned skills (AWM, ASI, SkillWeaver, WALT) degrade under controlled contextual, perceptual and execution shifts on GitLab, Magento and WordPress. Fix: MPCR, a test-time reranker. | Skill libraries only, not harness or meta-agent evolution. It does not report whether the learned agent is *more* brittle than a no-skill seed, and it does not evolve on shifts. |
| **Harness-Delta Attribution (HDA)** (2610.22466) | 2026-10-15 | Decomposes an evolution gain into artifact overfitting, test-time-scaling gain and a residual (11 executors, 3 frameworks). | No environment interventions. Your "correlate with HDA overfitting" analysis is a cross-check, not a contribution. |
| ReScopeBench (2611.05450) | 2026-11-03 | Experience lifecycle in 18 evolving environments (structural vs rule experience). | A benchmark for experience revision. It does not test harness evolvers under paired interventions. |
| PACE-Bench (2608.14441) | 2026-08-14 | 10 self-evolving methods adapt code across source→mutated physics environments. | Re-adaptation after shift, not robustness of the evolved artifact. Physics, not web/tool. |
| Δτ-Bench (2610.34238) | 2026-10-25 | Tool ecosystems evolve across 6 states. Tests whether agents revise procedural knowledge. | Agent-level evaluation. Could serve as an off-the-shelf tau-side intervention source instead of building your own. |
| AgentPerturb (2610.21624); Bench2Robust (2608.11977); Executable Contracts (2610.14477) | Aug–Oct 2026 | Controlled perturbation evaluation of tool agents. AgentPerturb notes that "agents rarely recognize altered inputs". | Fixed agents. No evolution. |
| Conservative Meta-Agent Search (2610.31436); GRACE / Scoped Verification (2607.09175) | Jul–Oct 2026 | Meta-agent or context evolution under non-stationarity and distribution shift (τ²-telecom for GRACE). | Proposes methods. No paired-intervention audit of standard evolvers. |
| Self-Evolving Agents Can Repair Failures but Struggle to Generalize (2610.22984); SelfImprove-Bench (2611.02797) | Oct–Nov 2026 | Repair-generalization gap. Separating capability gain from scenario fitting. | Generalization across tasks, not robustness to interventions. |

**Novelty assessment.**
- No paper crosses (harness evolvers) × (paired controlled environment interventions) × (evolution diet at matched compute). K1 does **not** fire.
- But the *directional claim* has ≥2 close relatives in the last 3 weeks:
  - 2610.39122 for the claim itself and for an Arm-B-like fix.
  - 2610.38392 for the mechanism.
- The *evaluation-of-evolvers* framing is crowded: Beneath the Surface uses the same 4 evolvers, plus HDA and Rethinking-Eval.
- The rubric's "≥2 close papers in last 6 months" pulls N down to about 3.
- The idea's note that it is "uncontested (1 forward citation)" is true for BreakingWeb but misleading for the question.

---

## 2. Mock NeurIPS review

**Summary.** The paper asks whether gains from harness evolution survive controlled environment interventions. It evolves harnesses with GEPA, ACE, AHE and a light Hyperagents on clean BreakingWeb and tau-style tasks, then reports the paired drop of champion vs seed by primitive and layer. It also reports how failure modes shift, and an "intervention diet" arm.

**Strengths.**
1. A good instrument. BreakingWeb's paired drop holds the instruction and the backend criterion fixed, so the champion−seed difference-in-differences is clean causal accounting. The belief-failure / false-finish label is a directly observable mechanism metric.
2. Timely. Evolution evaluation is a hot 2026 topic: Rethinking-Eval, HDA, Beneath the Surface, Masked Erosion.
3. Cheap to explain, and actionable for practitioners who ship evolved harnesses.

**Weaknesses.**
1. **The headline is predictable and partly pre-empted.**
   - "Optimizing on the clean distribution overfits to the clean distribution" is a prior most reviewers already hold.
   - 2610.39122 already shows that self-evolution degrades robustness to one class of environment intervention, and that contrastive adaptation data fixes it.
   - 2610.38392 already shows that success-only gates erode safeguards.
   - Without a sharper twist, reviewers will call it "a robustness eval of existing optimizers on our benchmark".
2. **Arm B is close to tautological.** Evolving on the intervention distribution and testing on it will obviously help. It is only meaningful if the test uses **held-out intervention families or layers**, and if it is compared against cheap baselines:
   - a generic "verify after acting" instruction appended to the clean champion;
   - a robustness-aware *selection/gate* without intervention feedback.
   Otherwise a reviewer will say "you trained on the test condition".
3. **The direction of the effect is uncertain, which helps and hurts.**
   - Beneath the Surface finds that evolvers commonly insert verification and tool-check reminders.
   - BreakingWeb's dominant failure, fabricated success (210 records), is exactly what such a reminder targets.
   - Evolved champions may therefore have a *smaller* drop.
   - The proposal states only one direction. It must pre-register both directions and a null, and explain why each is interesting.
4. **Statistical power.**
   - BreakingWeb has 519 pairs with one variant per base task, spread across 7 sites, 29 families and 7 primitives. The verification family alone has about 71 variants.
   - After a train/val/test split, per-primitive cells hold roughly 10–30 tasks.
   - Champion−seed differences in paired drop are second-order effects, likely 3–8 pp. Expect the confidence intervals to swallow the per-primitive story unless you pool and use paired bootstrap / mixed-effects models with multiple evaluation passes (k ≥ 3) per task.
5. **Feasibility of the evolver set.**
   - AHE and Hyperagents on a *browser* harness is substantial porting work.
   - Open/cheap backbones reportedly cannot drive Hyperagents.
   - Browser rollouts are long: 40 actions and a 26k-token context.
   - 4 evolvers × 3 backbones × 3 seeds × 2 arms is about 72 evolution runs. On cheap models that likely exceeds $1.5k and an undergrad's engineering budget.
6. **"Leaderboard flip" is asserted, not motivated.** With 3–4 evolvers a rank flip is weak evidence: any two evolvers within noise will "flip". You need either more methods/configurations or a correlation-based claim, e.g. rank correlation between clean-gain and robust-gain across all champion candidates in the archive, not just the 4 finals.
7. The "learned verification would be equally publishable" line is a second paper, not part of this design.

**Questions for the authors.**
- How is the BreakingWeb train/test split built, and are any intervention families held out?
- Is compute matched by rollouts, by tokens, or by dollars?
- What is the seed harness? If it is browser-use default, its drop is already reported for strong models.

**Scores.**
- Soundness: 2.5/4 as proposed, 3 with the fixes.
- Novelty: modest.
- Significance: medium.
- Clarity of claim: good.
- **Likely score: 4–5 (borderline reject) as written; 6 (weak accept) if reframed** around robustness-aware selection with held-out families and a two-sided pre-registered hypothesis.
- Triggers for "workshop": single benchmark, 3 evolvers, an obvious Arm B.

---

## 3. Rubric scores

| Dim | Score | Rationale |
|---|---|---|
| N (×3) | 3 | No paper with the same design, but ≥2 close Oct–Nov 2026 papers on the directional claim (2610.39122) and the mechanism (2610.38392), plus the same evolver set in 2611.07624. |
| S (×2.5) | 3 | Either direction is publishable *if* pre-registered: "evolution erodes verification" vs "evolution buys robustness". As stated, the headline is expected and Arm B is predictable. A rank flip with 4 methods is fragile. |
| R (×2) | 5 | Builds directly on the mentor's BreakingWeb and his "stay reliable when the environment changes" agenda. Self-improvement plus world-model/environment interventions. |
| C (×2) | 3 | Paired design is strong; power per primitive is weak. One real benchmark plus a tau set you would have to build. Arm B lacks held-out families and baselines as written. The reusable artifact (intervention-aware evolution protocol) is modest. |
| F (×1.5) | 3 | Mentor's infra exists. But browser evolution is expensive, porting AHE/Hyperagents is heavy, and the tau intervention generator is additional infra. $1.5k is optimistic for the full grid. |
| T (×1) | 4 | Harness evolution evaluation and environment interventions are hot. Slightly crowded (HDA, Beneath the Surface, Rethinking-Eval). |

**Total = 20 × (3·3 + 2.5·3 + 2·5 + 2·3 + 1.5·3 + 1·4) / 12 = 20 × 41 / 12 = 68.3** (viable with fixes).

**Kill flags.**
- **K1:** not fired. No 2026 paper uses the same design. 2610.39122 makes the same *directional* claim in a different setting, so mark it partial.
- **K2:** at risk. A "no difference" null is publishable only if framed as "robustness is a model property that harness evolution neither buys nor costs", and that needs the power analysis.
- **K3:** not fired, since cheap and open backbones are usable.
- **K4:** partial risk. "Run existing evolvers on existing benchmark" plus "train on interventions" can read as combining two things. The reframing below fixes this.

---

## 4. Fixes (ordered by score impact)

1. **Make robustness-aware *selection* the contribution, not brittleness.** Score every archive candidate, not just finals, on a small intervened validation set. Then test the following.
   - (a) Does intervention-robust validation score predict held-out test, cross-benchmark transfer and the HDA "residual" better than the clean validation score does? This is a cheap early-warning signal for overfitting.
   - (b) Does a `min(clean, intervened)` gate / champion-selection rule improve OOD transfer at matched compute?
   This connects to Beneath the Surface (selection fails) and Masked Erosion (gates erode safeguards) and gives a reusable protocol. It turns a rank flip among 4 methods into a **correlation over hundreds of candidates**, which has far more statistical power.
2. **Hold out intervention families or layers for Arm B.** Evolve on seed and server-layer families, test on network and client-layer ones (and the reverse); also hold out fabricated-success entirely. Add these baselines at matched rollouts:
   - (i) clean champion plus a generic "re-read state before finishing" instruction (the spotlighting analog from 2610.39122);
   - (ii) clean evolution with robust selection only (Fix 1);
   - (iii) the full intervention diet.
   The interesting claim becomes "robustness transfers across families or does not".
3. **Pre-register a two-sided hypothesis.**
   - H+: champions have larger drops because of verification erosion (measure deleted or overridden checks via edit diffs, as in Masked Erosion).
   - H−: champions have smaller drops because evolvers insert verification.
   - Report the **false-finish rate** (belief failures among declared-finished runs) as the primary mechanism metric. It is BreakingWeb's sharpest signal and directly testable.
4. **Cut scope to fit budget.**
   - Evolvers: GEPA + ACE + one code-level evolver (AHE-lite or A-Evolve). Drop Hyperagents-lite unless the mentor's group already has a browser port.
   - Backbones: 2, one open (Qwen-class) and one cheap API.
   - 3 seeds. k = 3 eval passes per task.
   - Run a $50 pilot: GEPA × 1 backbone on 60 train / 150 test pairs, to estimate the champion−seed paired-drop difference and its variance before you commit.
5. **Use an existing second domain** instead of building a tau intervention generator: Δτ-Bench ecosystem states (2610.34238), Bench2Robust, or SkillShift-Bench's GitLab/Magento shifts. This covers "beyond one benchmark" without new infra. Build the procedural tau generator only if it is already needed for the curriculum line.
6. **Position against 2610.39122 explicitly.** Their effect is IPI-specific and safety-framed. Yours is task-success robustness across 29 families × 7 primitives, with harness evolvers and matched compute. Cite Masked Erosion as the predicted mechanism and test it.
7. Optionally link to Dan's C4 / plasticity line: report paired drop as a function of evolution generation (an anytime curve). "Robustness half-life" over generations is a more surprising figure than end-point drops.

**Sharpened headline claim.**
> Clean-task validation scores are a misleading selector for evolved harnesses. Across three evolvers and two backbones on BreakingWeb and a second shifted-tool benchmark, a candidate's paired intervention drop predicts its out-of-distribution transfer better than its clean score does. Champions chosen on clean scores lose X% of their gain on held-out intervention families, mostly through more false "done" claims. Selecting candidates by intervention-robust score, at the same compute, recovers most of that loss without retraining on the test families.

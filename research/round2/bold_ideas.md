# Round 2: bold ideas (4 sharp + 3 non-obvious)

*2026-11-12. These ideas build on round 1 (REPORT.md, sweeps), the round-2 verification files and field_forecast.md. None repeats the round-1 list: not the Effect/Cause/Decay study, evolve-in-dream, learning-to-goodhart, curriculum-goodhart, meta-plasticity, harness-half-life, the confound battery, the placebo evaluator or self-forecast. Every paper ID below appeared in a tool result this session or in a round-1/2 file. Quick scoop searches were run on 2026-11-12: 6 alphaXiv recency searches and S2 abstract lookups. OpenReview (ICLR 2027) is still unchecked because the host is blocked.*

**How the two groups split**
- **SHARP 1-4** are aimed at where the field is about to move:
  - the RSI plateau
  - self-reference as a selling point
  - world models inside agents
  - verifier co-evolution
- **NON-OBVIOUS 5-7** use evolutionary-biology lenses that the forecast puts in the low-scoop zone. Prediction 8 (75%) says these lenses stay metaphors, with no tested predictions. None of the three is the "next obvious paper" in any lane.

Shared infrastructure for all seven:
- the round-1 procedural τ-world (unlimited held-out tasks)
- a Gödel-lite self-modifier plus one fixed-improver baseline (GEPA- or Meta-Harness-style)
- a cheap open or flash backbone. DeepSeek V4 Flash ran forgetting studies at $20-40/run (2610.20563); Qwen3.5-27B with a 9B executor also works.

---

## SHARP-1: `sharp-interference-plateau`: "RSI plateaus from interference, not exhaustion"

**Claim.** The plateau every self-improvement lineage hits is caused by **negative epistasis among the edits it has already accepted**, not by the improver running out of good ideas. Two signatures follow:
1. An edit accepted at generation k ≥ 10 is worth **more** when grafted onto the generation-0 agent than in the lineage where it was found.
2. "Rebasing" the late edits onto an ancestor and dropping the early ones beats the lineage tip at matched compute.

**Falsified if:**
- late-edit effects on gen-0 are ≤ their in-lineage effects (TOST within ±1 pp), *and*
- the pairwise-knockout interaction variance is under 10% of total gain.

That outcome means edits are additive, which supports the "exhaustion" story. It is publishable either way, because it would tell the field whether to fund better proposers or better merge and rebase operators.

**Why surprising.** Every RSI paper reads a flat curve as "the improver has run dry", and methods respond with more exploration (Harness², Mixture-of-Branches, MESH-Harness). If the plateau is interference, more exploration is the wrong fix: the gains are already found, but they are masked by the stack they sit on. This is a direct, mechanistic account of Gödel Agent's "complexity ceiling".

**Key experiment.**
1. Run 6 lineages × 30 generations and log every accepted edit as a diff.
2. Build an **edit-graft matrix**: apply each accepted edit e_j (j = 1..J) alone to gen-0, to gen-k/2 and to its own parent. Measure Δ on a large fresh held-out pool, using paired tasks and 3 seeds.
3. Run pairwise knockouts on the final tip for the top 15 edits (105 pairs), and fit an additive versus pairwise-epistasis model, as in fitness-landscape biology.
4. **Intervention arm.** A "rebase" operator replays the top-m edits by graft value onto gen-0 in a new order. Compare it with the tip and with a parallel bag-of-edits from gen-0 at matched tokens.
- **Readouts:** the epistasis share; the late-edit graft ratio; the rebase gain.
- **Cost.** About $600-900 API. Grafting is evaluation only, no search.

**Closest prior.**
- **Harness-S 2611.04277** observes locality and complementarity among *sibling* harnesses, but builds a training method and does no in-lineage epistasis analysis.
- **Masked Erosion 2610.38392** measures subset structure among 4 *safeguards*. This is the same combinatorial logic applied to safety, not to gains.
- **HDA 2610.22466** splits gains into overfitting, test-time scaling and residual, with no edit-level interaction.
- **2611.03219** shows forgetting when evolution continues on a new task (between tasks, not within a lineage).
- **Mutation Without Variation 2606.05408** studies mutation chains without selection.

No paper grafts accepted edits back onto ancestors.

---

## SHARP-2: `sharp-no-privileged-self-access`: "Self-referential agents don't know themselves"

**Claim.** The premise of Gödel-style self-reference is that the agent inspects its own code and state and so knows where it fails. That premise is false in a measurable sense:
- A self-referential improver predicts its *own* agent's per-task failures **no better than a foreign improver** (another lineage, same base model) reading the same code and traces.
- Its advantage in **repair** (Δ from a targeted edit) over the foreign reader is ≤0 and *shrinks* with generation, because its notes and self-description go stale relative to its code. This is self-model drift.

**Falsified if:** self-prediction AUROC minus foreign AUROC is ≥ +0.05 at generation 0 *and* does not fall with k. That would be the first positive evidence of privileged self-access for code-level self-reference, and a strong pro-Gödel result.

**Why surprising.** Weight-level introspection work finds *some* privileged access, and 2610.07186 shows it is hard to identify from the inside. The self-referential agent literature simply assumes it at the code level. If a stranger can repair your agent as well as you can, "self-reference" reduces to "an improver that happens to share weights". A related result: the Zero-Call Verification paper (alphaXiv 2611.end-state-agreement…) reports that Opus 5.5 accepted 65 of 66 failed attempts by its own agent, a hint that self-judgment is worse, not better.

**Key experiment.**
1. Evolve 8 Gödel-lite lineages for 20 generations.
2. At generations {0, 5, 10, 20}, give every lineage's improver the code, memory and last 50 traces of (a) its own agent and (b) a different lineage's agent at the same generation. Use a full round-robin, so the comparison is within the same model.
3. Tasks:
   - **Predict** pass/fail on 200 unseen tasks.
   - **Repair** with one edit, scored by matched same-state Δ (HMED-style).
   - **Describe** what the agent does, scored by a behavior-prediction probe.
4. Add a **drift probe**: does the improver's stored self-description predict current behavior worse than a fresh read of the code?
- **Cost.** About $500. All prediction and repair calls are short.

**Closest prior.**
- Gödel Agent 2410.04444 (assumes self-awareness).
- LLM introspection work: 2610.07186, 2610.38911, self-modeling 2608.30980. All are weight-level.
- HMED 2610.07979 (sign disagreement in credit, but no self vs other).
- Does Failure Feedback Direct Self-Modification? 2610.14662 (directedness, no self vs other).

No self-versus-foreign comparison was found in the searches.

---

## SHARP-3: `sharp-useful-fictions`: "Selection makes an agent's world model less true as the agent gets better"

**Claim.** Give the agent an explicit, evolvable world model: a belief and state tracker plus a "did my action succeed?" predictor that the harness writes and consults. Then select harness edits on task outcome only. Two things happen together:
- **task success rises**
- **the belief tracker's accuracy falls**, measured against the backend's ground-truth state. It drifts toward optimistic fictions such as "the form was submitted".

Under BreakingWeb-style interventions, the evolved agents' *belief failures* (declaring done while the backend disagrees) rise with generation, even though clean success is higher.

**Falsified if:** belief accuracy is flat or rising across generations, with its correlation to generation ≥0 and a CI that excludes −0.1. That outcome would mean outcome selection keeps internal models honest, which is good news for world-model-based agents and still publishable.

**Why surprising.** The field assumes better agents have better world models, and both RWML-style training and Dream-RSI rely on it. 2609.33335 showed that *wrong* prediction targets still help at the weight level. This idea makes the claim causal and dynamic at the harness level: selection *actively* trades truth for usefulness. The direct consequence is long-horizon unreliability, because fictions compound across steps. That is Xunjian's reliability agenda, and BreakingWeb already found that 75% of failures are belief failures.

**Key experiment.**
1. Use self-hosted BreakingWeb sites, or the τ-world where backend state is exactly known.
2. The harness has a typed belief block that is logged every step.
3. Run 3 arms × 6 lineages × 20 generations:
   - **(a)** outcome-only selection
   - **(b)** outcome + belief-accuracy term (honesty pressure)
   - **(c)** belief block frozen, rest evolves
4. **Readouts per generation:** clean success; per-step belief accuracy; false-done rate; success under the 4 BreakingWeb intervention layers.
5. **Mechanism probe.** Swap the gen-k belief block into the gen-0 agent and vice versa, to show the fiction is *used* and not just written.
- **Cost.** About $700-1,000.

**Closest prior.**
- 2609.33335 (mismatched world-model targets still help; weights, static).
- BreakingWeb 2609.35814 (belief failures in static agents).
- Self-Report Leakage 2610.29609; Done Is Not Correct 2611.10063 (static calibration).
- Dream-RSI 2609.14858.

No paper tracks belief accuracy across self-improvement generations.

---

## SHARP-4: `sharp-cross-play-collusion`: "Co-evolved verifier-agent pairs speak a private language" *(anticipates Forecast #4)*

**Claim.** In verifier-harness co-evolution, the hottest method lane, part of the reported gain is **pair-specific**. Evaluate a co-evolved agent with an *independently* co-evolved verifier of equal accuracy, or the verifier with a foreign agent. The cross-play measured gain falls by ≥30% of the self-play gain, while ground-truth success is unchanged. The diagonal of the self-play/cross-play matrix inflates, and the inflation grows with co-evolution rounds.

**Falsified if:** the cross-play gain is within ±10% of the self-play gain (TOST). That outcome would be a reassuring "co-evolution is honest" paper, which the lane currently lacks.

**Why surprising.** Co-evolution is sold as the *fix* for Goodhart: "who verifies the verifier". A mutual-adaptation artifact that only shows up off-diagonal would make every co-evolution leaderboard number partly unidentifiable. Forecast #4 (65%) predicts the "collusion" critique arrives in 2027. This paper would be the first quantitative and causal version of it.

**Key experiment.**
1. Run 2 open co-evolution methods: the rubric-harness approach of 2610.36826 and verifier-harness co-evolution as in 2610.17609, or minimal re-implementations. Use 5 seeds each.
2. Build the full 10×10 agent × verifier matrix at rounds {0, 3, 6, 10}, scored by verifier acceptance *and* ground truth.
3. Decompose acceptance into ground truth + pair-specific inflation. Fit a mixed model, as in matrix factorization of a cross-play table.
4. **Mechanism:** identify the "dialect" features the verifier rewards, such as format and self-report phrases, by ablating them from agent outputs.
- **Cost.** About $800.

**Closest prior.**
- **Evolving Together, Failing Apart 2610.20845** shows skill-policy co-evolution fragility under bank drift. That is weight-level policy and skills, not verifier collusion, but it must be cited.
- Who Verifies the Verifier 2610.11464 (a vacuous verifier still trains).
- 2611.01742 (meta-RL partner-swap "evaluation illusion"). This is the closest design precedent, but in MARL.
- Recoverability & Verifier Strength 2610.27465 (judge gives constant scores in 9/37 settings).

**Scoop risk.** Medium, and it rises by summer. Post a preprint by Feb 2027.

---

## NONOBV-5: `nonobv-error-threshold`: "Self-referential agents hit an Eigen error catastrophe, earlier than fixed improvers" *(forecast Q1)*

**Claim.** Retained gains collapse once the self-modified code length L passes L* ≈ ln(s)/ε:
- ε is the per-token defect rate of edits (unintended changes)
- s is selective superiority, set by gate size and evaluator noise.

Beyond L*, performance random-walks or decays: an error catastrophe. The sharp, new prediction is that **a self-referential improver has a lower L* than a fixed improver with the same editor**. When defects hit the editor's own code, they raise its future ε, which is the "mutator allele" effect from population genetics. This gives a quantitative law for Gödel Agent's complexity ceiling, set by the evaluator and the editor, not by the model's intelligence.

**Falsified if:** there is no transition, meaning equilibrium performance is a smooth function of ε·L with no knee (the change-point model loses to a linear one), or L* does not shift with s as ln(s) predicts.

**Why surprising.** The field reads the ceiling as harness bloat or a lack of ideas. A threshold law predicts the *exact* evaluator size needed to support a harness of a given length. It also predicts that cheaper and noisier editors cap complexity by themselves, and that self-reference is costly in a quantifiable way.

**Key experiment.**
1. Measure natural ε per backbone (3 models) from diff audits of unintended edits.
2. Sweep injected edit noise (5 levels) × gate size (4 levels) × {self-referential, fixed improver}, 3 seeds, 25 generations.
3. Track L, held-out score and editor-code integrity.
4. Fit the Eigen threshold; check L* ∝ ln(s)/ε; predict each real model's ceiling and validate on unperturbed runs.
- **Cost.** About $900. It runs on a cheap backbone, and the noise injection is programmatic.

**Closest prior.**
- A Theory of Reliable Self-Evolution 2609.08175: the ceiling is set by verification and evaluation cost. This is theory with no length × defect phase transition and no self-referential versus fixed comparison.
- Kinetic Theory of Gated Self-Evolution 2610.03243: plugin-population theory, with the evaluator kept outside the population.
- EvoUndo 2608.28363; Mutation Without Variation 2606.05408.

No error-threshold test was found.

---

## NONOBV-6: `nonobv-canalization`: "Selection manufactures fragility" *(forecast Q2, uses BreakingWeb)*

**Claim.** Harness evolution on a fixed environment **decanalizes** the agent. Each generation raises clean success while *lowering* robustness to controlled environment interventions. At matched clean accuracy, an evolved harness is less robust than a hand-written or less-evolved one. The robustness slope per generation is negative, and the effect is driven by edits that exploit incidental regularities in the environment (selector shapes, response timing, default values).

**Corollary, the Waddington genetic-assimilation arm.** Inject a small rate (10%) of *random* interventions into the selection gate. This restores robustness at little clean cost and transfers to *held-out intervention families*. That transfer is the second falsifiable prediction.

**Falsified if:** the robustness gap at matched clean accuracy is ≥0, or there is no negative generation slope (CI excludes −1 pp per 5 generations).

**Why surprising.** RSIAgent, Darwin-Claw and SkillShift-Bench 2610.33528 treat robustness as something to *repair after* drift. This idea says the self-improvement process itself *creates* the fragility, in a dose-response with generations. That reframes reliability under environment change, Xunjian's stated agenda, as a cost of selection.

**Key experiment.**
1. 6 lineages × 20 generations on BreakingWeb-hosted sites, or τ-world with API/UI perturbation operators.
2. Evaluate every 2 generations on the paired clean + 4-layer intervention suite (seed, server, network, client).
3. Build a matched-accuracy comparison: hand-tuned harnesses and early checkpoints with equal clean success.
4. Run the assimilation arm with 3 of the 7 cognitive-primitive families in the gate and 4 held out.
- **Cost.** About $800-1,100.

**Closest prior.**
- **Appreciation Teaches Unauthorized Execution 2610.39122**: self-evolution raises indirect prompt injection success by 14-49 pp. This is adversarial and safety-focused, with no generation dose-response and no environment interventions.
- BreakingWeb 2609.35814 (static agents only; no forward citations evaluate self-improvers).
- 2611.03219 (forgetting across tasks, not robustness within a task).
- A Jagged Frontier 2608.18389 (static code agents).

---

## NONOBV-7: `nonobv-baldwin-hiding`: "The smarter the harness, the less the weights learn" *(forecast Q3; modest GPU)*

**Claim.** In biology, plasticity *shields* genotypes from selection. The prediction here: GRPO on a small model wrapped in an **adaptive, self-improving harness** (memory + reflection that evolves during training) yields weights that are **worse when evaluated harness-free** than the same GRPO run with a frozen harness, at matched steps and matched rollouts. The weight-learning deficit grows with harness adaptivity. This is the hiding effect: the harness solves the failures the gradient would otherwise fix.

The ranking flips by evaluation mode:
- adaptive-harness RL wins **with** its harness
- frozen-harness RL wins **without** it, and on model upgrade or transfer

**Falsified if:** the harness-free score of adaptive-RL weights is ≥ frozen-RL weights (TOST ±2 pp) at all three adaptivity levels. That outcome would be a positive "harness scaffolds learning" result (a Baldwin *assist*), which is also publishable.

**Why surprising.** The amplify-then-distill intuition behind HERD, CoEvo-RSI and Evo² is that a stronger harness produces better training signal. A hiding effect would mean co-evolution *steals* learning from the weights. That matters whenever the harness is later changed or removed, which harness churn guarantees (2610.20845).

**Key experiment.**
1. Qwen3-4B, GRPO on ALFWorld or TextCraft plus the τ-world.
2. Harness adaptivity in 3 levels: frozen; memory-only; memory + reflective self-edit every N steps.
3. 3 seeds, matched rollouts and steps.
4. **Readouts:** harness-free score; score under a *foreign* harness; the gradient-signal fraction (the share of rollouts where the harness rescued a failure, which predicts the deficit).
- **Cost.** About $1.5-2.5k GPU (about 4×A100-days per arm) plus about $200 API.

**Closest prior.**
- Harness-S 2611.04277 (transfers sibling-harness rescues *into* a policy that stays effective under the original harness, which is the opposite design).
- Evolving Together, Failing Apart 2610.20845 (the co-evolved policy is tied to its bank).
- Harness Evolution Hits a Ceiling 2610.11655.
- HERD 2610.31401; Evo² 2610.13025.

None of these evaluates harness-ablated weights as a function of harness adaptivity during RL.

---

## Recommendation

1. **Strongest overall:** SHARP-1 (interference plateau). It has a crisp ranking flip and its own actionable operator (rebase). It is cheap because it is mostly evaluation, and it fits Gödel Agent's ceiling directly.
2. **Best fit for Xunjian:** SHARP-3 (useful fictions) or NONOBV-6 (canalization). Both reuse BreakingWeb, and together they would make a two-part "selection vs truth / selection vs robustness" story.
3. **Highest timing value and highest scoop risk:** SHARP-4. Preprint by Feb 2027.
4. **Cheap add-ons inside any run:** the NONOBV-5 diff audit for ε, and the SHARP-2 round-robin prediction probe.

**Unchecked:** OpenReview ICLR 2027 submissions and the NeurIPS 2026 workshop accepted lists (re-check after Dec 4).

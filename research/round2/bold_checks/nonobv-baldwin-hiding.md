# Bold check: nonobv-baldwin-hiding

*"The smarter the harness, the less the weights learn."* Skeptical area-chair check, 2026-11-12.

**Verdict: PROMISING, about 71/100, but only after re-framing.**

- **No K1 scoop.** No paper sweeps harness adaptivity as a dose and measures the harness-free quality of the weights at matched rollouts.
- **The headline phenomenon has already been observed, twice, as side results:**
  - CODA (2610.20845, App. H.7) on co-evolved skill banks.
  - HarnessAdapt-RL (2610.28069) on near-answer hints.
- **The opposite sign (a Baldwin *assist*) has been shown three times:**
  - Harness-S (2611.04277): rescue fires only when every base rollout fails.
  - HarnessAdapt-RL's evolved tools and hooks.
  - Self-HPO (2610.22791): its trained policy keeps its gains under the seed harness.
- **So "adaptive harness ⇒ hiding" as stated is likely false as a universal claim.** A reviewer will also point to the zero-advantage-group explanation, which is trivial.
- **The sharp, still-open claim is a two-regime law.** Hiding versus assist is set by *which* failures the harness rescues, not by how adaptive it is.
  - Rescuing all-fail groups creates contrast, which gives an assist.
  - Rescuing mixed groups collapses contrast and conditions the gradient on harness context, which gives hiding.
  - This law reconciles the five conflicting data points above, and nobody has stated or tested it.

---

## 1. Scoop searches run

| # | Channel | Query / target | Relevant hits |
|---|---|---|---|
| 1 | alphaXiv (recency) | RL inside a harness with memory or reflection; harness-free evaluation; crutch effect | ClawLoop 2610.14118, JoMoHa 2611.11499, **Self-HPO 2610.22791**, CoTrace 2610.10426, **Harness Annealing 2610.01235**, On-Policy Correction 2609.09134, Elicitation-or-Expansion 2610.27889, Your Policy Is Secretly a Harness 2610.37255, HDA 2610.22466 |
| 2 | alphaXiv (recency) | Baldwin / hiding effect; in-context plasticity shields weights | Agentic ICL 2610.23051, ICRL≈ICL 2610.11152, Are Self-Evolving Agents Continual Learners 2610.25858, Bidirectional Context Self-Distillation 2608.09555, SkillForge 2608.24747. Only neuroevolution Baldwin papers matched (2604.03565, 1806.07917). **No LLM paper uses the Baldwin or hiding framing.** |
| 3 | alphaXiv (recency) | GRPO with experience memory; internalization when the memory is removed | MDSKILL 2611.05313, ENGRAM 2611.09472, **Explore with Memory, Learn without It (LEAF) 2610.17734**, Factorized Skill Memory 2610.15492, **Externalise the Memory 2610.30478**, ReTTT 2610.23942 |
| 4 | alphaXiv (recency) | scaffolding rescues failures and removes gradient signal; harness removal | **Training the Whole Agent (HarnessAdapt-RL) 2610.28069**, Harness Annealing, **Harness-S 2611.04277**, Harness Scaling 2610.29458, Masked Erosion 2610.38392, Recurrent Harness 2610.12668, HarnessRL 2610.26426 |
| 5 | alphaXiv (recency) | hint dependence in RLVR; guidance makes groups all-correct | **Understanding Enrichment in RL 2610.02846**, Hint Annealing 2609.34975, Guided-RLVR 2610.37041, FADE 2611.03625, MagicBeanStalk 2610.12659, Stabilizing On-Policy Targeted Guidance 2611.05266, Per-Context Reverse Curricula 2607.26417 |
| 6 | Semantic Scholar (4 queries) | harness dependence, weights without the harness / scaffold-removal crutch / Baldwin effect in LLM agents / assistance hides learning signal | **Multi-Harness RL: Training Gains and Harness Dependence 2609.04518**, LEGO-RL 2608.17393, HASE 2607.03935, Harness-R1 2608.02276. Nothing on hiding. |
| 7 | S2 forward citations | 2610.20845 (CODA), 2611.04277 (Harness-S), 2610.28069, 2610.01235 | 0 citers each; all are too new to be indexed. |
| 8 | WebSearch (OpenReview / ICLR 2027) | harness co-evolution, "without harness", weights learn less | No ICLR 2027 OpenReview page found. Surfaced CoHARL, HarnessX, HarnessForge and EvoHarness-RL 2608.05446 ("shifts agents from frequent scaffold use toward selective harness access"). None runs a harness-ablated, adaptivity-dosed comparison. |
| 9 | WebSearch | "Baldwin effect" OR "hiding effect" LLM agent harness RL | No LLM use of the term. A Stanford CS224R report cites a scaffold-dependency hypothesis (per-step valid-action list in BabyAI) that its own controlled runs did not support. Unverified as a paper. |

## 2. Deep reads

### 2610.20845, Evolving Together, Failing Apart (CODA; Alibaba, Oct 2026). Closest prior on the *phenomenon*.
- **Main paper.** Skill-bank drift fragility grows along co-evolution training in 5 of 6 settings. Worst-case drift erases 52–93% of the bank's gain.
- **App. H.4.** Fragility tracks "dependence", measured as the success-rate drop when the bank is removed: ρ = +0.82 over 32 checkpoints.
- **App. H.7 is the dangerous part.** On SkillRL-ALFWorld:
  - the co-evolved policy *with its bank removed* scores **57.6%**;
  - a never-skilled pure-RL policy scores **70.6%** (mean of its last five checkpoints; peak 87.5%).
  - The authors write: *"co-evolution does not merely add a removable crutch, it leaves the underlying policy worse at acting alone than one that was never given the crutch… standalone ability traded away."*
  - That is the hiding effect, stated in words.
- **Why it is not a K1 scoop.**
  - One setting, n = 1 seed.
  - Different ALFWorld backends and sample counts between the two arms; the authors call it "a supporting observation rather than a headline result".
  - Not compute-matched.
  - No adaptivity dose, no mechanism, no foreign-harness evaluation.
  - It covers skill banks only, not memory or reflective self-edit harnesses.
- **Effect on novelty.** It kills "we discover that co-evolution hurts standalone weights" as a headline. Our paper must be the controlled, powered, mechanistic version, and must cite H.7 prominently.

### 2610.28069, Training the Whole Agent, Not Just the Model (HarnessAdapt-RL; ZJU, Oct 2026). Closest prior on the *mechanism*, and evidence for *both* signs.
- **Hiding evidence.** GRPO with **near-answer hints** gives weights that score *below* plain GRPO once the hints are removed: 32.7 vs 35.3 average across SWE-bench Verified, Multilingual and Pro.
  - Their stated mechanism is exactly the rescue-fraction story: hints create more all-success groups, so only 39.0% of groups are mixed versus 46.5% with mechanism-level skills.
  - The authors: *"Higher success during training does not necessarily lead to better performance after training."*
- **Assist evidence.**
  - *Mechanism-level* skills beat GRPO after removal (38.3).
  - The policy trained under the *co-evolving* tools and hooks (H₀→H₁→H₂), evaluated under the **seed harness H₀**, scores 52.8 / 29.6 / 35.0. That matches or slightly beats the Skill-Mechanism policy trained under a frozen H₀ (52.2 / 28.0 / 34.6).
  - In other words: an adaptive harness, evaluated harness-ablated, shows **no hiding** in their one-seed data.
- **Caveats.**
  - One training run per configuration.
  - GRPO gets 2× rollouts.
  - Harness acceptance explicitly optimizes the "trainable rate". This is an *anti-hiding* gate: it rejects edits that collapse contrast.
- **Effect on novelty.**
  - The "dose = adaptivity" version of our claim is contradicted by their single point.
  - The "dose = how much the help collapses group contrast" version is *supported* by their hint result.
  - They never frame or test this as a law, a crossover or a dose-response, and they have no ALFWorld, memory or reflection harness.

### Other key reads (targeted PDF queries)
- **Harness-S 2611.04277** (SJTU, Nov 2026).
  - Rescue rollouts under sibling harnesses are injected **only when all 8 base rollouts fail**. Evaluated under H₀, the result beats GRPO by large margins (4B AppWorld: 56.9 vs 49.0 Avg@3).
  - Fixed *mixed-harness* GRPO (always sampling under a helper harness) is **worse than plain GRPO under H₀** (4B AppWorld: 46.7 vs 49.0).
  - That contrast, always-on help hurting while help only on all-fail groups improves, is the two-regime law in miniature. Nobody has named or swept it.
- **Self-HPO 2610.22791** (MSR, Oct 2026).
  - In the policy × harness cross table, P₂ (trained under the evolving H₁/H₂) scores 64.4 under the seed harness H₀, versus 61.0 for the base policy.
  - It uses BAR (reward-balanced groups), which removes zero-variance groups and should suppress hiding.
  - It does not report a frozen-harness-trained policy under H₀ as a controlled comparison.
- **Harness Annealing 2610.01235** (SJTU/Berkeley).
  - Direct SFT on harness trajectories collapses under the tools-only harness: 9B SWE-QA scores 16.5 vs 59.9 for base.
  - This is harness dependence via SFT format conditioning, not RL. Their fix is a strong-to-weak curriculum.
  - It shows the *conditional-learning* channel of hiding exists independently of contrast.
- **Understanding Enrichment in RL 2610.02846** (UCSD/MSR).
  - Uncorrected hinted rollouts implicitly reweight the reward by q/π, which can be decomposed into scale, rotation and variance.
  - In a sparse regime, enrichment *helps* by preventing collapse.
  - This gives the theory hook: help that rotates the gradient toward successes the base policy cannot produce acts as an assist. Help that up-weights successes the base already produces acts as hiding.
- **Multi-Harness RL 2609.04518.** Harness-to-harness variance (7 pp) exceeds RL gains (<1 pp). This is useful context for the foreign-harness evaluation, but it is not a scoop.

## 3. Area-chair assessment

**Sharp?** The original claim is sharp but likely *wrong as stated*.
- Prior data points split by help type, not by adaptivity. Adaptive harnesses with trainable-rate gates (HarnessAdapt-RL) or balanced rollouts (Self-HPO) show no hiding.
- The "rescue fraction predicts the deficit" regression is close to a tautology. Reviewers will say "rescued groups go all-success and give zero advantage; use DAPO dynamic sampling."
- A sharper claim must also predict when the sign *flips*.

**Novel?** Partially.
- The phenomenon was observed by CODA H.7 (as a side note) and by HarnessAdapt-RL (hints).
- The opposite was observed by Harness-S, HarnessAdapt-RL and Self-HPO.
- **Not done anywhere:**
  1. a controlled, multi-seed, matched-rollout dose-response;
  2. a decomposition into *contrast starvation* vs *context-conditional learning*;
  3. a predicted and observed crossover from hiding to assist under the *same* harness, just by changing when it fires;
  4. foreign-harness evaluation of the resulting weights.

  The Baldwin framing itself is unclaimed in LLM work.

**Feasible?** Borderline-OK.
- Qwen3-4B (or 1.7B for the pilot) GRPO on ALFWorld plus TextCraft, using a verl-agent/GiGPO-style stack.
- Arms: 3 to 5 harness arms × 3 seeds ≈ 12–15 agentic RL runs.
- About $1.5–2.5k GPU is realistic if each run is ≤ 40 H100-hours.
- The reflective self-edit loop can use the policy itself, so no API spend is needed.
- Matching compute is subtle. Match environment steps, policy-gradient tokens *and* informative-group count, and report all three.

**Mentor-relevant?** Good.
- It is about what a self-improving outer loop does to the inner learner. That is the Gödel-Agent question pushed down to weights.
- It feeds directly into "long-horizon self-improvement" (weights vs. scaffold division of labour) and into harness churn and robustness.
- It is less about world models.

## 4. Sharpened claim

> **Baldwin hiding versus Baldwin assist is decided by *where* the harness helps, not by *how much* it adapts.**
>
> - **Hiding.** When an adaptive harness converts the policy's *already-mixed* GRPO groups into all-success groups, the weights learn less than under a frozen harness at matched rollouts. Measured harness-free and under a foreign harness, the deficit is ≥ 3 pp.
> - **Assist.** When the *same* harness is gated to fire only on all-fail groups, it converts dead groups into informative ones, and the harness-free weights beat frozen-harness RL.
> - **Predictor.** Across all arms, the harness-free delta is predicted (R² ≥ 0.6) by Δ*I*: the net change in the fraction of informative (mixed) groups between the harness-on and harness-free rollouts of the same policy. "Harness adaptivity" adds no predictive power once Δ*I* is controlled.
> - **Residual mechanism.** With Δ*I* equalized (dynamic sampling or BAR), any remaining deficit isolates *context-conditional learning*: the gradient lands on harness-conditioned behavior.
>
> **Falsified if:**
> - the always-on and fail-gated arms of the same harness do not differ in sign (TOST ±2 pp); or
> - Δ*I* does not out-predict adaptivity level.

**Why this is better.**
- Either outcome is publishable:
  - a clean crossover law gives a design rule ("gate your harness on all-fail groups");
  - if the gating does nothing, hiding is pure context-conditioning, which contradicts the HarnessAdapt-RL account.
- It reconciles five published data points that currently disagree.
- It turns the trivial-looking "zero-advantage groups" objection into the measured variable instead of a confound.

## 5. Minimal design (revised)

- **Model and environments.**
  - Model: Qwen3-4B; Qwen3-1.7B for the pilot and the scale check.
  - Environments: ALFWorld, TextCraft, and the τ-world generator as the held-out domain.
- **Harness arms** (all using the same memory plus reflective self-edit harness unless noted):

  | # | Arm | What it isolates |
  |---|---|---|
  | (a) | Frozen harness | Baseline |
  | (b) | Memory-only adaptive | The adaptivity dose |
  | (c) | Memory + reflective self-edit, always on | The original claim's arm |
  | (d) | (c) fired only on all-fail groups (Harness-S-style gating) | The predicted crossover |
  | (e) | (c) + DAPO dynamic sampling / BAR to equalize Δ*I* | The conditional-learning residual |

  Run 3 seeds each.
- **Evaluation of every final checkpoint.**
  - Harness-free.
  - Under a foreign harness: a ReAct variant plus one evolved by a different seed.
  - In-harness.
- **Logged per step.**
  - Fractions of informative, all-success and all-fail groups, both harness-on and harness-free on a probe set.
  - Then regress the harness-free Δ on Δ*I*, adaptivity level and rescue fraction.
- **Required controls.**
  - Match environment steps and gradient tokens.
  - Add a "harness-on rollouts, harness-free re-scoring" arm (importance-corrected per 2610.02846). This separates *what is sampled* from *what context the gradient conditions on*.
- **Pilot (≤ 1 week, < $100).** One ALFWorld split with Qwen3-1.7B, arms (a), (c) and (d), 1 seed. Check that the sign of the harness-free Δ differs between (c) and (d).

## 6. Scores (rubric)

| Dim | Weight | Score | Note |
|---|---|---|---|
| N Novelty | 3 | 3.5 | The phenomenon was observed (CODA H.7, HarnessAdapt-RL hints) and the opposite was observed (Harness-S, Self-HPO). The two-regime law, dose-response, crossover and decomposition are unclaimed. The original framing alone would score 2.5–3. |
| S Surprise / falsifiable | 2.5 | 4 | Same harness, opposite effect on the weights depending only on gating: a clean ranking flip, and either outcome is publishable. |
| R Mentor relevance | 2 | 3.5 | Self-improving outer loop vs. inner weight learning; long-horizon self-improvement. Not world models. |
| C Competitiveness / rigor | 2 | 3.5 | Multi-seed, matched compute, two model sizes, three environments, a predictive regression, and a reusable Δ*I* diagnostic. Agentic RL variance is a risk. |
| F Feasibility | 1.5 | 3 | About 12–15 agentic GRPO runs; GPU-bound but within the stated budget. |
| T Timing | 1 | 4.5 | Harness–policy co-evolution is the hottest lane of Oct–Nov 2026; this audits it. |

**Total = 20 × (3·3.5 + 2.5·4 + 2·3.5 + 2·3.5 + 1.5·3 + 1·4.5) / 12 = 20 × 43.5 / 12 ≈ 72.5.** Rounding down for execution risk gives **≈ 71**: viable with the re-framing.

**Kill flags.**
- **K1:** none. No paper has this design.
- **K2:** none once re-framed.
- **K3:** none; open models only.
- **K4:** low risk, since the contribution is a law, not a method.

## 7. Biggest risks
1. **The "it's just zero-advantage groups" objection.** Mitigated by arm (e) and by making Δ*I* the explicit predictor. If (e) removes all hiding, the paper becomes "hiding is a contrast artifact; here is the one-line fix". That is still useful, but it lands closer to 65.
2. **Fast-follow risk.** CODA's authors (Alibaba) or the HarnessAdapt-RL group could run this as a follow-up; CODA's H.7 explicitly gestures at it. Move fast: get the pilot out by January 2027.
3. **Agentic-RL seed variance.** CODA's pure-RL baseline swung from 87.5% to 53.1% within 25 steps. Use last-k-checkpoint averaging and pre-register the evaluation step.

## Sources (web)
- OpenReview / ICLR 2027 search: no matching submission found. Surfaced [Self-HPO](https://www.emergentmind.com/papers/2610.22791), [CoHARL](https://deeplearn.org/arxiv/857986/coharl:-co-evolving-agent-policies-and-executable-harnesses-with-reinforcement-learning), [HarnessForge review](https://www.themoonlight.io/de/review/harnessforge-joint-harness-and-policy-evolution-for-adaptive-agent-systems), [HarnessRL](https://www.opentrain.ai/papers/harnessrl-co-evolving-policies-with-a-just-in-time-training-harness-for-open-end--arxiv-2610.26426/), [HASE](https://huggingface.co/papers/2607.03935.md).
- Baldwin search: [EvoHarness-RL](https://www.alphaxiv.org/abs/2608.05446), [Where Does Harness-Optimization Value Live?](https://www.alphaxiv.org/abs/2609.02889), [CS224R report](https://cs224r.stanford.edu/projects/pdfs/Alfred%20Sven%20Bertil%20Sjoeqivst%20Hana%20Mengyao%20Liu%20submission_416271147/CS224R_Final_Report_Hana_Liu_Alfred_Sjoqvist.pdf) (unverified secondary).

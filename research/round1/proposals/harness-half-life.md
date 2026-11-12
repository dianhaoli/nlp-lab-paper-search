# Proposal: Do Harness Gains Outlive Their Model? Forecasting the Depreciation of Evolved Harnesses Across Model Upgrades

*Candidate id: `harness-half-life` (renamed: 2609.00468 and 2608.20918 already use "half-life"). Pitch from Dan to Xunjian Yin, 2026-11-12. Target: NeurIPS 2027 main track (deadline about mid-May 2027).*

---

## 1. Title and headline claim

**Working title:** *Do Harness Gains Outlive Their Model? Forecasting Which Evolved-Harness Edits Survive a Model Upgrade, and Whether to Keep, Warm-Start or Reset Re-Evolution*

**Headline claim (predicted outcome).**
When a harness evolved for model v_n is carried to its same-family successor v_{n+1}, the gains it got by *elicitation* are absorbed, and sometimes reversed, while its *expansion* gains survive. Elicitation here means a higher pass@1 on tasks v_n could already solve. Expansion means a higher pass@k or new information that widens the set of reachable tasks. The survival of each edit can be forecast from **v_n alone**, using its pass@1-vs-pass@k profile, and the forecast is preregistered before v_{n+1} is evaluated. At matched budget, re-evolving from the carried harness and its archive (warm start) shows a measurable **plasticity deficit** compared with re-evolving from the seed plus only the expansion edits (reset).

**Alternative-outcome claims (also publishable):**
- **A1: gains are durable.** If carried harnesses keep ≥80% of their HDA-neutralized gain across ~10 transitions, the paper becomes "evolved harnesses are durable assets, and what depreciates is only the overfitting and test-time-scaling share". That directly reconciles 2608.10694 (positive upward transfer of prompts) with 2609.00468 / 2608.24641 (hand techniques decay), and the depreciation-law fit is still the contribution.
- **A2: depreciation is unforecastable.** If the pass@k profile on v_n does not predict survival (Spearman CI includes 0, and it is no better than edit-size or "sees/writes" baselines), the finding is that a harness's depreciation cannot be forecast from the source model. Every harness-evolution paper would then need to re-validate on each model release, which is a concrete warning for the evaluation literature.
- **A3: no warm/reset difference.** If warm, cold and reset are statistically *equivalent* under a TOST test with a preregistered ±3 pp margin, the paper reports that evolver lineages carry no plasticity cost across substrate changes. That is an informative equivalence result, not a noisy null.

---

## 2. Motivation and why now

1. **Harness evolution became the default way to improve agents without training in 2026.** Examples include Meta-Harness 2603.28052, AHE 2604.25850, GEPA, SkillOpt, Harness² 2610.14224 and Object-Oriented Harness 2610.28380. Every one of these papers implicitly assumes the evolved artifact is worth its search cost. The most common way that assumption breaks in practice is that **the model underneath changes**. Qwen shipped 3 → 3.5 → 3.6 within about a year, plus refreshes such as Qwen3-4B → 4B-Instruct-2507.
2. **The 2026 evaluation wave showed harness gains are fragile, but only for a fixed model.**
   - Rethinking-Eval 2607.12227: parallel sampling beats evolution at matched budget, and held-out gain is about 0.
   - HDA 2610.22466: about 54% of gains are overfitting and about 14% are test-time scaling (TTS).
   - Elicitation-or-Expansion 2610.27889: gains are mostly elicitation and vanish at pass@k.
   - Beneath the Surface 2611.07624: gains redistribute task success.

   None of these papers asks what happens to the artifact when the model is upgraded.
3. **Two theory/measurement pieces now make a *forecast* possible rather than a post-hoc description.**
   - *Your Policy Is Secretly a Harness* (2610.37255, Oct 27) shows deployed return = **ceiling(H) − mismatch(model, H)**. A harness change moves both terms. A weight change moves only the mismatch. Read in reverse (fixed harness, model upgraded): an evolved harness's ceiling gain is model-independent and should survive. Its mismatch-reduction gain is specific to v_n and should be absorbed as v_{n+1} has lower mismatch, or reversed through readout drift.
   - 2610.27889 supplies an operational proxy: pass@k gain ≈ ceiling/coverage (expansion), and pass@1-only gain ≈ mismatch reduction (elicitation).

   Combined, these give a falsifiable, edit-level forecast that nobody has run.
4. **Adjacent fields have just measured the same decision for other assets, which legitimizes the question and leaves agent harnesses explicitly open.**
   - UpgradeBench 2608.20918 (Aug 2026) measures freeze/port/refresh/retrain for **LoRA specialists** across 4 Qwen releases plus OLMo checkpoints. It explicitly lists "agentic harnesses with tool scaffolds" as an extension it did not do, and its prompt-side pilot is one task, one hop, best-of-6 prompts.
   - Heirloom 2611.00650 covers personal adapters across Qwen3→3.5 and Phi upgrades.
   - Identity-under-transition 2611.10628 covers persona and memory.
5. **Mentor/agenda timing.** Agent Plasticity 2610.08902 (Oct 2026) defined cost-normalized gain-per-$ for self-improving lineages but held the model fixed. A model upgrade is the cleanest real-world "environment change" for a self-improving lineage. Measuring how plastic the lineage is under that change brings the plasticity line into meta-agents, which is Dan's stated interest.

---

## 3. Core research questions

- **RQ1 (Survival and depreciation law).** What fraction of an evolved harness's *neutralized* gain survives a same-family upgrade? How does the retained fraction scale with the seed-harness capability delta of the upgrade? Is there a reusable depreciation law, and does it differ between prompt-only (GEPA) and code/control-flow (Meta-Harness/AHE-style) harnesses?
- **RQ2 (Forecast).** Can each edit's survival be predicted *from v_n alone* by its elicitation/expansion profile (pass@1 vs pass@k contribution)? Does this predictor beat simpler baselines: edit size, a sees-vs-writes tag (2610.11655), and an LLM-judge "model-specific?" rating?
- **RQ3 (What survives spuriously).** How do the HDA components (overfitting O, test-time scaling T, residual G) each survive? Is "harness survival" largely survival of benchmark shortcuts?
- **RQ4 (Plasticity of the evolver lineage).** At matched budget on v_{n+1}, how do these options compare on anytime gain and on gain per dollar (Agent-Plasticity metric)?
  - keep the harness;
  - keep it plus spend the budget on parallel sampling;
  - cold restart;
  - warm restart with the full archive;
  - warm restart with the artifact only;
  - reset to expansion-only edits.

  Is there a plasticity deficit from inheriting a lineage?

---

## 4. Related work and positioning

| Paper (verified ID) | What it does | Gap it leaves / how we differ |
|---|---|---|
| **Optimize Cheap, Deploy Strong** 2608.10694 (Aug 2026) | GEPA prompts evolved on a cheap tier and deployed up price tiers (Claude/GPT/Gemini plus Qwen3-8B). Each tier also gets its own re-optimization. Transfer ≥ own-tier in 36/48 setups. | Prompts only, price *tiers* not same-family *versions*, no warm-start arm, no edit-level attribution. **We include a GEPA prompt-only arm to replicate it on versions** and test whether *control-flow* compensation depreciates while instructions do not. |
| **UpgradeBench** 2608.20918 (Aug 2026) | Freeze/port/refresh/retrain for LoRA specialists over 4 Qwen releases and documented OLMo checkpoints. Defines retention, H50 half-life and coupling elasticity β. CKA predicts portability (ρ = 0.74, n = 8). | **Weights, not harnesses.** Its prompt pilot is best-of-6 prompts on one hop, and it lists agent harnesses as future work. We borrow its rigor (prospective predictions, OLMo distance control, flip rates) for evolved agent harnesses, plus re-evolution arms. It is also the main scoop risk if its authors extend it. |
| **Harness Evolution Hits a Ceiling** 2610.11655 | 2×2 harness × LoRA. Weights trained on evolved-harness trajectories absorb the gain. Rule: "writes" edits train in, "sees" edits stay. | Absorption by *deliberate distillation*, not by an independent vendor successor. We use its sees/writes tag only as a **baseline predictor**, not as our mechanism. |
| **Elicitation or Expansion?** 2610.27889 | pass@k analysis of Meta-Harness gains (Qwen3-8B vs 32B, GPT-OSS-20B vs 120B, AppWorld effort). Gains are mostly elicitation, and scaling wins on hard tasks. | Verified: it **never runs the optimized harness on the larger model**. It compares harness gain with scaling gain only. We use its pass@k lens *per edit* as a v_n-only predictor of survival on v_{n+1}. |
| **Your Policy Is Secretly a Harness** 2610.37255 (Oct 2026) | Return = ceiling − mismatch. Post-training makes preferences harness-specific, and ceiling/mismatch predict adaptation gains (ρ = 0.95, MiniWoB++). | Studies **changing the harness under fixed/adapted weights**. We study the dual: **fixed harness, changed weights** (vendor upgrade). Its decomposition motivates our elicitation→absorbed / expansion→survives prediction. |
| **Harness-Delta Attribution (HDA)** 2610.22466 | O/T/G decomposition across 11 executors including Qwen3-4B-2507, Qwen3-8B, Qwen3.5-0.8–4B and Qwen3.6-27B/35B-A3B. Code released. | Each model is evolved separately, with no carry-over. We **run HDA on every v_n harness and track O/T/G survival separately**. Its released harnesses enable the pilot. |
| **What Survives the Next Model?** 2609.00468 | 35 hand-engineered SE techniques vs a single prompt on newer models. 37–63% are absorbed, and "added-information" techniques survive. | Hand techniques, no re-optimization, no controlled ladder. We study *evolved* artifacts with an *edit-level forecast*. |
| **Aging of PE Techniques** 2608.24641 | Hand PETs across GPT-3.5→4o, Qwen2→2.5, Mistral. GPT absorbs and Qwen does not. | It warns that absorption is family-dependent, so we add a second family and controlled OLMo checkpoints. |
| **Agent Plasticity** 2610.08902 (Oct 2026) | Plasticity = held-out gain per $ of learning cost for fixed-weight lineages that build artifacts. | Model held fixed throughout. We measure **plasticity of an evolver lineage after its model is swapped** (warm vs reset), which is a new axis. |
| **Rethinking the Evaluation of Harness Evolution** 2607.12227 | Matched-budget comparison: parallel sampling 72.3 vs evolution 67.4, and held-out gain ≈ 0. | Fixed model. We adopt its matched-budget parallel-sampling control as the "carry nothing, spend budget on sampling" arm on v_{n+1}. |
| **Evo2Team** 2609.34135 (Sep 2026) | Evolved routing/communication skill banks transferred across team sizes and GPT/Qwen ladders (e.g. Qwen3-8B / 3.5-35B-A3B tiers). Adapting is cheaper than re-evolving. | Multi-agent routing skills across *ladder swaps*, not same-family successors of one executor. No elicitation/expansion forecast and no warm-vs-reset plasticity. |
| **MaD-RSI** 2610.28201; **Finding the Right Fit** 2610.00917; **Beneath the Surface** 2611.07624; **Third Eye** 2611.12988 | Harness coupled to its LLM (MaD-RSI). Model×harness ranking reversals (Right Fit). Cross-benchmark transfer of evolved harnesses (Beneath the Surface). Value of a self-update after a further weight update (Third Eye). | None carries an evolved harness through a vendor upgrade with re-evolution arms and a prospective forecast. |

**How we differ in one sentence:** this is the first study to (i) carry *evolved code and prompt* harnesses through *same-family* model upgrades on a dose-response ladder, (ii) forecast each edit's survival *before* seeing the new model, and (iii) treat re-evolution after the upgrade as a *plasticity* question with matched-budget, equivalence-tested arms.

*Search log (this round):*
- alphaXiv discover with recency: (a) harness transfer to successor model / upgrade; (b) plasticity, warm-start, reset; (c) prompt re-optimization after model update; (d) scaffold bitter lesson and checkpoints.
- Semantic Scholar: 4 keyword queries plus forward citations of 2610.27889, 2608.10694, 2610.22466, 2610.11655, 2608.20918, 2610.37255 and 2609.00468.
- New items found beyond the critique: UpgradeBench 2608.20918, Policy-is-a-Harness 2610.37255, Evo2Team 2609.34135 and Heirloom 2611.00650.
- None runs evolved harnesses through successor models with keep/warm/cold/reset arms. Deep reads confirmed that 2610.27889 has no carry-over experiment and that 2610.08902 never changes the model.

---

## 5. Method and experimental design

### 5.1 Setup and controls for version confounds
- **Unified execution layer.** All models run behind one **neutral tool adapter**: JSON tool schema in, parsed tool call out, with the same retry/parse policy. **Chat templates are pinned** to each model's official template, and **thinking mode is fixed** (non-thinking for all Qwen3.x; "thinking budget off" where the knob exists). Context is capped at 32k for all models.
- **Interface-mismatch rate (IMR).** Fraction of episodes ending in a parse, format or tool-schema failure, from adapter logs. Reported separately. "Behavioral harm" is defined only on episodes with no interface failure, so that "harmful carry-over" is never just template breakage (CoEvo-RSI's TIMR concern).
- **Fixed proposer** across all evolution runs: one mid-cost model, e.g. GPT-5.4-mini-class or Claude Haiku-class; final choice in the pilot by price. Harness Updating ≠ Benefit 2605.30621 reports evolver capability matters ≤3.1 pp, so a cheap fixed proposer is defensible and removes a confound.

### 5.2 Models: transition ladder (the unit of analysis is the transition)
| Ladder | Transitions | Why |
|---|---|---|
| L1 dense | Qwen3-32B → Qwen3.5-27B → Qwen3.6-27B | Main same-family ladder, about 27–32B. |
| L2 MoE | Qwen3-30B-A3B-Instruct-2507 → Qwen3.5-35B-A3B → Qwen3.6-35B-A3B | Second Qwen ladder. HDA used these exact executors. |
| L3 small | Qwen3-4B → Qwen3-4B-Instruct-2507 (refresh, same arch) → Qwen3.5-4B; Qwen3-8B → Qwen3.5-9B | Refresh vs generation contrast, as in UpgradeBench/Heirloom. Cheap. |
| F2 second family | Mistral-Small-3.1 → 3.2 (refresh); Gemma 3 → successor if open weights with tool use exist *(unverified, check in Dec)* | Addresses "Qwen-specific absorption" (2608.24641). |
| C controlled | OLMo-3-7B SFT → DPO → RL stage checkpoints *(availability and tool-use competence to verify in pilot)* | Capability delta is a manipulated variable with documented training, following UpgradeBench's OLMo design. |
| S size controls (non-successor) | Qwen3.5-9B → Qwen3.5-27B; Qwen3.6-27B ← Qwen3.5-27B handled in L1 | Separates "stronger model" from "same lineage, new post-training". |

That gives about 10–12 transitions, which supports a mixed-effects dose-response fit instead of an n = 2 anecdote. Optional closed-model check: one Haiku-class version step if a successor is released before March, budget-capped at $100.

### 5.3 Benchmarks (low-artifact, with pool sizes)
| Benchmark | Search / Val / Test | Notes |
|---|---|---|
| **AppWorld** | 60 (train) / 57 (dev) / 168 (test-normal) + 200 sampled from test-challenge as OOD | Used by 2610.27889, so the setting is comparable. Has difficulty labels. Execution-checked. |
| **τ²-bench** (retail + telecom; airline as OOD domain) | 60 / 40 / 128 (+50 airline OOD) | Low artifact. The user simulator is fixed to one cheap model across all arms. |
| **SWE-bench Verified** (HDA's 48/24/50 split) | secondary/pilot only | Reuses HDA's released harnesses. Low power, so used for corroboration only. |

ALFWorld and LiveMath are excluded as main benchmarks (HDA: 89–100% overfitting). The pilot uses ALFWorld only as a **positive control**: a zero-LLM-call harness "survives" any upgrade, which shows why neutralization is mandatory.

### 5.4 Evolvers
- **Code harness:** Meta-Harness-style filesystem proposer (prompts, tools, control flow, memory). The AHE-style multi-component evolver is an optional third arm if budget allows.
- **Prompt-only:** GEPA (system prompt plus few-shot), which replicates 2608.10694 on versions.
- Each source run uses 20 iterations, ≤2 candidates per iteration, validation-based selection (HDA showed validation selection helps on average), and **3 seeds**.

### 5.5 Formal metrics
Let S_m(H) be the mean pass@1 of model m with harness H on the held-out test set, estimated from r = 8 rollouts per task. Let H₀ be the seed harness and H*_n the harness evolved on v_n.

- **Source gain:** Δ_n = S_{v_n}(H*_n) − S_{v_n}(H₀).
- **Carried gain:** Δ_{n→n+1} = S_{v_{n+1}}(H*_n) − S_{v_{n+1}}(H₀).
- **Retained-gain fraction:** ρ = Δ_{n→n+1} / Δ_n. Computed only when Δ_n is significant (paired bootstrap, 10k resamples over tasks), and reported with a BCa CI. **Absorption** is α = 1 − ρ for ρ ∈ [0, 1]. **Harmful carry-over** means ρ < 0 with CI excluding 0, measured on interface-clean episodes.
- **Upgrade dose:** D = S_{v_{n+1}}(H₀) − S_{v_n}(H₀) (seed-harness capability delta). Also the headroom-normalized form D̃ = D / (1 − S_{v_n}(H₀)).
- **Depreciation law:** a hierarchical model ρ_{t,b,e,s} = β₀ + β₁·D̃_t + β₂·[code harness] + β₃·D̃_t·[code] + u_b + u_e + ε, with random intercepts for benchmark b and evolver e. An exponential form ρ = exp(−λ·D̃) is the alternative, compared by LOO-CV. The reported "harness depreciation rate" is λ, with a separate λ for code and prompt harnesses.
- **HDA components on v_n and v_{n+1}:** Δ = O + T + G, where O uses H*_neutral (detected shortcuts minimally disabled by an audit agent plus human check) and T uses the call-matched H₀^scale. Survival is ρ_O, ρ_T and ρ_G.
- **Edit-level elicitation/expansion profile (computed on v_n only).** Segment H*_n into K ≤ 6 edit clusters e_j using the proposer's commit lineage, merged by diff locality. Leave-one-cluster-out:
  - c_j^m(k) = pass@k_m(H*) − pass@k_m(H* \ e_j), using the unbiased estimator of Chen et al. with n = 8 and k ∈ {1, 8}.
  - **Expansion score:** ξ_j = c_j^{v_n}(8) / (|c_j^{v_n}(1)| + |c_j^{v_n}(8)| + ε) ∈ [−1, 1]. Near 0 or negative means elicitation-type; positive means expansion-type.
  - **Edit survival:** s_j = c_j^{v_{n+1}}(1) / c_j^{v_n}(1), shrunk with an empirical-Bayes prior for small denominators.
  - **Forecast test:** Spearman(ξ_j, s_j) pooled with a cluster permutation test (permute within transition). The headline is **leave-one-transition-out prediction of Δ_{n→n+1}** from Σ_j c_j^{v_n}(1)·ŝ(ξ_j), compared with:
    - (b1) ρ = 1, "keep everything";
    - (b2) the mean ρ;
    - (b3) the sees/writes tag (2610.11655);
    - (b4) edit size in tokens or LOC;
    - (b5) an LLM-judge prediction of model-specificity.
- **Plasticity on v_{n+1}:** for an arm a with cumulative re-evolution cost C (in $, including rejected candidates, following 2610.08902):
  - P_a(C) = [S_{v_{n+1}}(H_a(C)) − S_{v_{n+1}}(H_a(0))] / C
  - **net value over keep:** V_a(C) = S_{v_{n+1}}(H_a(C)) − S_{v_{n+1}}(H*_n)
  - anytime AUC of V_a over C ∈ [0, B]
  - **plasticity deficit:** Π = AUC_reset − AUC_warm.
- **Flip rates** (from UpgradeBench): the negative-flip rate of carried vs re-evolved harnesses, i.e. tasks solved by H*_n on v_n but not on v_{n+1}.

### 5.6 Arms on v_{n+1} (matched budget B = 50% of the source evolution cost, in $ and in rollouts)
| Arm | Start | History given to proposer |
|---|---|---|
| A0 Seed | H₀, no evolution | — |
| A1 Keep | H*_n | — |
| A2 Keep + TTS | H*_n, with B spent on parallel sampling / self-consistency (2607.12227 protocol) | — |
| A2' Seed + TTS | H₀ + B on parallel sampling ("carry nothing") | — |
| A3 Cold | H₀ | none |
| A4 Warm-lineage | H*_n | full v_n archive and traces |
| A5 Warm-artifact | H*_n | none (separates code inheritance from archive inheritance) |
| A6 Reset-to-expansion | H₀ + edits with ξ_j > τ (τ preregistered at 0.25) | none |
| A7 Reset-to-neutral | H*_neutral (HDA shortcuts removed) | none |

Re-evolution grid: 4 core transitions (L1 ×2, L2 ×2) × arms A3–A6 × 3 seeds on AppWorld (48 runs), plus τ²-bench on 2 transitions (24 runs), using the Meta-Harness-style evolver. A2, A2' and A7 are evaluation-only or cheap.

### 5.7 Preregistration and the prospective test
Before any v_{n+1} evaluation, we post an OSF registration of the ξ-based forecasts, the τ threshold, the TOST margin (±3 pp) and the depreciation-law form. The Qwen3.6 targets stay **sealed** until the forecasts for L1/L2 are registered. If a new open-weight successor (e.g. a Qwen3.7 open release or a Gemma successor) ships between January and April 2027, it becomes the **truly out-of-sample** test, forecast before release.

### 5.8 Power sketch
- **Source gains.** With N = 168 test tasks and r = 8, the SD of the paired per-task difference is about 0.25, so SE(Δ) ≈ 0.02. Gains ≥ 5 pp are detectable. 2610.27889 and HDA report 5–25 pp on AppWorld/SWE-type tasks for this model size.
- **Forecast (RQ2).** About 6 clusters × 2 evolvers × 2 benchmarks × 1 seed per cell × ~6 transitions with edit ablations ≈ 140 edit-transition pairs. With a clustering design effect of about 1.7, the effective n is about 80, which gives 80% power for Spearman ρ ≥ 0.31 at α = 0.05. The leave-one-transition-out regression has ~10 folds.
- **Plasticity (RQ4).** Evolution-run SD is about 3–4 pp (2610.27889, App. C.1). With 12 runs per arm pooled over 4 transitions in a mixed model, SE(diff) ≈ 1.5 pp, so a ≥4.5 pp AUC difference is detectable. TOST with a ±3 pp margin reaches 80% power if the true difference is ≤1 pp. A3-style nulls are therefore reportable as equivalence.
- **Depreciation law (RQ1).** 10–12 transitions × ~4 harness types × 3 seeds gives about 120 ρ estimates. That is enough to estimate the slope β₁ with an SE of about 0.08 if the spread in ρ is about 0.3.

### 5.9 Ablations and robustness
- Prompt-only vs code (GEPA vs Meta-Harness).
- Source-selection rule (search-best vs val-best).
- Thinking on vs off for one ladder.
- Neutral adapter vs native tool format (quantifies how much "harm" is interface).
- r = 4 vs 8 rollouts for ξ stability.
- Difficulty-stratified ρ (hard tasks expected to retain less, per 2610.27889).
- Swapping the user-simulator model on τ².

---

## 6. Expected results and figures

- **Figure 1 (teaser, two panels).**
  - Left: schematic. H*_n is decomposed into edits on v_n, each edit gets an elicitation/expansion score from pass@1 vs pass@8, and then it is carried to v_{n+1}.
  - Right: **the key scatter.** Per-edit survival s_j on v_{n+1} (y) against the expansion score ξ_j measured on v_n only (x). Points are colored by HDA label (O, T, G) and marker shape is the evolver.
  - Prediction: a positive monotone trend (Spearman ≈ 0.4–0.5). O-labeled edits sit at s ≈ 1 regardless of ξ ("shortcuts survive"). Many low-ξ code edits fall at s ≤ 0 ("compensation turns harmful").
- **Figure 2 (depreciation law).** Retained fraction ρ (y) against upgrade dose D̃ (x) across ~10–12 transitions. Prediction: separate fitted curves for code harnesses (steep, λ_code ≫ 0) and prompt-only GEPA harnesses (flat, consistent with 2608.10694). The OLMo controlled points lie on the same curve, which shows dose and not "Qwen-ness" drives depreciation.
- **Figure 3 (plasticity).** Anytime V_a(C) curves on v_{n+1} for keep, keep+TTS, cold, warm-lineage, warm-artifact and reset-to-expansion, with a P_sat inset. Predictions:
  - reset-to-expansion ≥ cold > warm-lineage early in the budget;
  - warm-artifact sits between them;
  - keep+TTS is competitive at low budget on v_{n+1}, echoing 2607.12227.
- **Figure 4.** Stacked bars of O/T/G on v_n and their survival on v_{n+1}. Prediction: O survives at ρ ≈ 1, T is partly absorbed, and G splits by ξ.
- **Table.** A decision rule ("keep / re-evolve / reset") with expected regret against oracle per-transition choice, mirroring UpgradeBench's decision-policy replay.

---

## 7. Pilot (≤1 week, ≤$50) and go/pivot/kill criteria

**P1: HDA cross-application (about $25).**
- Take HDA's released Meta-Harness SWE-bench harnesses. *Verify the repo contains per-model harness code; the paper says "full harnesses ... in supplementary materials".* Run two carries:
  - Qwen3-4B-Instruct-2507 harness → Qwen3.5-4B;
  - Qwen3-30B-A3B-Instruct-2507 harness → Qwen3.5-35B-A3B and → Qwen3.6-35B-A3B.
- Use HDA's 50-instance test split, 2 rollouts, H₀ vs H*, with neutral adapter and IMR logging.
- Also run the ALFWorld Qwen3.5-4B harness → Qwen3.6-35B-A3B as the overfitting positive control (cheap).

**P2: Elicitation/expansion predictor at candidate level (about $20).**
- 2610.27889 released code and trajectories; *verify the 40 Qwen3-8B HotpotQA candidate harnesses are recoverable.* Their pass@1 and pass@128 on Qwen3-8B are already measured.
- Run all 40 candidates (or the 15 most distinct) on **Qwen3.5-9B** and **Qwen3-32B**, 300 tasks × 4 rollouts. HotpotQA calls are short, about 3k tokens.
- Test Spearman(candidate ξ on 8B, candidate retained gain on 9B/32B). This is a free first look at RQ2 at the harness level.

**P3 (no cost):** confirm checkpoints and tool-calling for OLMo-3 stages and the second family; price out the proposer.

**Decision criteria (end of pilot week):**
- **GO** if both of these hold:
  - (a) at least one transition has a CI for ρ that excludes 1 (≥ 1/3 of the gain lost) on interface-clean episodes, **or** P2 gives |Spearman| ≥ 0.3 with p < 0.05;
  - (b) IMR ≤ 10% after the neutral adapter.
- **PIVOT to A1 (durability plus decomposition)** if ρ ≈ 1 everywhere and P2 is flat. The paper then becomes "evolved harnesses are durable; what survives is shortcuts and TTS", with the warm/cold/reset plasticity arms kept as the second contribution.
- **PIVOT to RQ4-only** (plasticity of evolver lineages under substrate change) if survival results are noisy but the source evolutions are clean.
- **KILL** if neither HDA harnesses nor E/E candidates are runnable **and** a $30 self-run Meta-Harness evolution on AppWorld with Qwen3.5-27B shows no significant source gain (no denominator to depreciate). Fall back to the curriculum idea (#3).

---

## 8. Budget and timeline (to mid-May 2027)

**Cost assumptions** (to verify in pilot): an open ≤35B executor via a cheap provider with prompt caching costs about $0.004–0.008 per AppWorld/τ² episode. The proposer costs about $1.5 per evolution run.

| Item | Runs / episodes | API-only | With self-hosted Qwen executors (Duke/lab GPUs, vLLM) |
|---|---|---|---|
| Pilot | — | $50 | $50 |
| Source evolutions (core: 4 sources × 2 evolvers × 2 benchmarks × 3 seeds = 48; extension: 6 sources × 1 evolver × AppWorld × 2 seeds = 12) | 60 runs | $950 | $150 (proposer only) |
| Carry-over evals (≈80 harness→target pairs × 168 tasks × 8 rollouts) | ≈110k episodes | $550 | ≈$0 + GPU |
| HDA audits + neutralized evals | 60 harnesses | $200 | $120 |
| Edit ablations, pass@8 (24 harnesses × ≤6 clusters × 2 models × 100 tasks × 8) | ≈230k short episodes | $500 | ≈$0 + GPU |
| Re-evolution arms (72 runs at 0.5B) | — | $650 | $110 |
| τ² user simulator (fixed cheap model) | — | $150 | $150 |
| Contingency (15%) | — | $450 | $90 |
| **Total** | | **≈$3.5k** | **≈$0.7k API + ~1.5–2k H100-hours** |

The API-only line exceeds the ≤$3k ceiling. **The plan assumes self-hosted executors.** If no GPU is available, cut to 1 benchmark (AppWorld), 2 seeds for source runs and 3 for re-evolution, and K = 4 clusters, giving about $1.8k.

**Timeline**
- **Nov 13–20:** pilot P1–P3, then go/pivot/kill. Draft the OSF preregistration skeleton.
- **Nov 21–Dec 31:**
  - Infrastructure: neutral adapter, pinned templates, IMR logger, AppWorld/τ² splits, cost accounting.
  - Run core source evolutions on L1/L2 sources (Qwen3-32B, Qwen3.5-27B, Qwen3-30B-A3B-2507, Qwen3.5-35B-A3B).
  - Confirm F2/C checkpoints.
- **Jan 2027:**
  - HDA neutralization and edit segmentation; v_n-only edit ablations to get ξ.
  - **Register forecasts on OSF (by Jan 31)**, then unseal Qwen3.5/3.6 target evaluations.
- **Feb:**
  - Carry-over evals on all transitions, then the depreciation-law fit.
  - Extension sources (L3, F2, OLMo).
  - Interim writeup to Xunjian.
- **Mar:** re-evolution arms A2–A7 and plasticity analysis (TOST).
- **Apr:**
  - Robustness ablations.
  - Prospective test on any newly released successor; if none, use Mistral/OLMo as the held-out family.
  - Figures; full draft by Apr 25.
- **May 1–15:** internal review (Xunjian, Shuyan Zhou), polish, code/data release prep, submit.

---

## 9. Risks, mitigations and reviewer objections

**Risks**
1. **Scoop.**
   - UpgradeBench's authors flagged harnesses as their extension.
   - The Elicitation-or-Expansion authors (FuriosaAI/Yonsei/Berkeley) could carry their harness to 32B.
   - *Mitigation:* the pilot and an early arXiv note (Feb) on the depreciation law. Our distinct pieces are the prospective edit-level forecast plus the plasticity arms, which neither group is set up for.
2. **Small or insignificant source gains on clean benchmarks** (Rethinking-Eval reports held-out gain ≈ 0).
   - *Mitigation:* choose models where E/E and HDA reported gains (AppWorld; 4–35B Qwen). Use val-best selection. Compute ρ only where Δ_n is significant. The absence of durable gains is itself an A1-type finding.
3. **Edit segmentation is ill-defined for code harnesses.**
   - *Mitigation:* use proposer commit lineage, cap clusters at K ≤ 6, and run a robustness check with random-partition clusters (the predictor should degrade).
4. **Second family or OLMo checkpoints are unusable for tool use.**
   - *Mitigation:* fall back to Qwen size-step controls and Qwen3-2507 refreshes, and say plainly that family generality is limited.
5. **Compute overrun.**
   - *Mitigation:* the trimmed plan above, plus GEPA arms, which are cheap.

**Anticipated reviewer objections and rebuttals**
- *"This is 2608.10694 on a new axis."* That paper covers prompts across price tiers with no warm arm and no attribution. We replicate its prompt result on versions (GEPA arm) and show the *code/control-flow* component behaves differently. The contribution is the edit-level forecast and the plasticity arms.
- *"The mechanism is Ceiling's sees/writes rule."* The sees/writes tag is a *baseline* we try to beat. Our predictor is computed from v_n behavior (pass@k), grounded in the ceiling/mismatch identity of 2610.37255, and evaluated prospectively.
- *"Qwen idiosyncrasy / n = 2 transitions."* We use ~10–12 transitions, including refreshes, size steps, a second family and controlled OLMo stage checkpoints, with a mixed-effects fit and dose as the regressor.
- *"Harm is just template mismatch."* We use pinned templates and a neutral adapter, report IMR separately, define harm on interface-clean episodes, and add a native-format ablation.
- *"Warm = cold is noise."* The power analysis is in §5.8. TOST equivalence uses a preregistered ±3 pp margin and anytime AUC, not single endpoints.
- *"Survival just measures overfitting."* HDA neutralization runs on every v_n harness, and survival is reported per O/T/G component. The ALFWorld positive control shows the failure mode explicitly.
- *"The predictor was fit post hoc."* The OSF registration precedes target evaluation, Qwen3.6 targets are sealed, and we run a leave-one-transition-out test plus a prospective test on any new release.
- *"Practical but not NeurIPS-level insight."* We deliver a reusable depreciation law (λ by harness type), a v_n-only forecasting protocol, and a theory link (fixed harness, changing weights as the dual of 2610.37255). Plasticity of self-improving lineages under substrate change is a general ML question.

---

## 10. Fit with Xunjian's agenda and what the mentors bring

- **Gödel Agent (2410.04444) and self-referential agents.** A self-improving agent's harness *is* its accumulated "self". A model upgrade is the most realistic long-horizon event that changes its substrate. RQ4 asks whether a self-improving lineage stays plastic after the substrate changes, or whether inheriting its own history becomes a liability. That is the long-horizon self-improvement question in Xunjian's agenda, measured with the Agent-Plasticity metric. An optional extension, if time allows: a Gödel-Agent-style arm where the evolver's own proposal policy is part of the inherited state.
- **WeAct (2610.26847, weights switched as agent actions).** If weights can be swapped at runtime, the harness has to be the part that is robust to weight changes. Our forecast says *which* harness edits are weight-agnostic (expansion/information) and which are tied to specific weights (elicitation). That is a design rule for WeAct-style systems.
- **BreakingWeb (2609.35814, controlled environment interventions).** The same methodology applied to the model axis. The OLMo stage checkpoints and pinned-template controls are controlled interventions on the substrate instead of the web environment.
- **World models.** Edits that supply information or state tracking (world-model-like observation summaries) are predicted to survive because they raise the ceiling. Edits that patch a model's reasoning habits are predicted to depreciate. This gives a measurable argument for investing in harness-side world models.
- **What Xunjian brings:** evolver/self-modification expertise for the warm/reset arms, framing toward the self-improvement community, and possibly DeepMind-side perspective on how model upgrades are actually rolled out.
- **What Shuyan Zhou brings:** agent-benchmark rigor (WebArena lineage) for AppWorld/τ² split design and artifact auditing. Bhuwan Dhingra's group co-authored automated benchmark auditing (2605.26079, cited by HDA), which helps the HDA-style neutralization step.

---

## 11. Rubric re-score after fixes (honest)

| Dim | Before | After | Justification |
|---|---|---|---|
| N (×3) | 3.0 | **3.5** | The exact design (evolved code harnesses × successor ladder × v_n-only edit forecast × plasticity arms) is unrun, and deep reads confirm 2610.27889 and 2610.08902 leave the gap. But this round found two more close papers (UpgradeBench for adapters, Policy-is-a-Harness for the dual), so the neighborhood is crowded. |
| S (×2.5) | 3.0 | **3.5** | A preregistered forecast and depreciation law make both outcomes publishable (A1/A2/A3), and the "shortcuts survive" split is a likely surprise. Still, "elicitation is absorbed" will feel half-expected to readers of 2610.27889. |
| R (×2) | 3.0 | **3.5** | The plasticity-of-lineage framing ties it to long-horizon self-improvement, Gödel Agent and WeAct. It is still mainly an evaluation of artifacts, not a new self-referential method. |
| C (×2) | 3.5 | **4.0** | 10–12 transitions with a controlled-checkpoint arm, matched budget, TOST, HDA neutralization, preregistration, IMR control and a reusable protocol/law. It loses a point for depending on benchmark gains being significant on clean tasks. |
| F (×1.5) | 3.5 | **3.5** | The pilot is ≤$50 with released artifacts. The full study needs self-hosted GPU inference to stay under $3k (API-only ≈ $3.5k untrimmed, ≈ $1.8k trimmed). |
| T (×1) | 4.0 | **4.0** | Harness evaluation and plasticity are hot topics in Oct–Nov 2026, but the area is crowded. |

**Total = 20 × (3·3.5 + 2.5·3.5 + 2·3.5 + 2·4.0 + 1.5·3.5 + 1·4.0) / 12 = 20 × 43.5 / 12 = 72.5** (up from 64.6). This is the upper end of "viable with fixes". The largest remaining risks are scoop by the UpgradeBench or E/E authors and the possibility of weak source gains on clean benchmarks.

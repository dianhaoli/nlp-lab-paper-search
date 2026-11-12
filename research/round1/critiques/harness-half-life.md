# Critique: harness-half-life ("Do evolved harnesses survive model upgrades?")

**Idea (T5, Dan-modified).** Take a harness evolved for model v_n and run it on the same-family successor v_{n+1}. Measure how much of the gain survives, how much the new model absorbs, and how much turns harmful. Then compare three options: keep the harness, warm-start re-evolution, or cold-start re-evolution.
**Stated claim.** Most of the evolved-harness gain is compensation for the old model, which the next model absorbs; some of it turns harmful. Only edits that change what the model sees survive. At matched compute, warm-start is no better than cold-start.
**Reviewer stance.** Skeptical NeurIPS AC plus related-work search. Date: 2026-11-12.

---

## 1. Scoop search: what I ran

- **alphaXiv discover (recency / since 2026-06):** (a) evolved-harness transfer under model upgrade or version change, Meta-Harness/GEPA; (b) scaffold "bitter lesson", absorption, warm vs cold re-optimization; (c) Qwen3→3.5 successor, re-optimization, transferability; (d) optimize-cheap/deploy-strong, memory/skill portability; (e) model-specific vs model-agnostic harness components; (f) prompt/scaffold obsolescence across model releases.
- **Semantic Scholar keyword search (2026):** 7 phrasings, including "evolved harness model upgrade transfer", "harness gain absorbed stronger model", "harness transfer across model generations", "re-optimize harness new model warm start" and "prompt optimization transfer newer model version". All returned noise or the papers below. None ran the keep/warm/cold design.
- **Forward citations (S2):** Finding the Right Fit 2610.00917 (4 citing, none relevant); Optimize Cheap Deploy Strong 2608.10694 (0); Memory Portability 2609.05339 (2: 2611.10628 identity under model transition, 2610.02764); MaD-RSI 2610.28201 (0); Ceiling 2610.11655 (1: RSI-Opt); Elicitation-or-Expansion 2610.27889 (0); Meta-Harness 2603.28052 (382 citing, filtered for upgrade/transfer/portability/version/bitter: 2611.13144 student-aware skill transfer, 2610.29823 MemoryTax, 2608.12307 strong-to-weak via harnesses).
- **Deep reads (answer_pdf_queries):** 2608.10694, 2610.11655, 2610.22466 (HDA), 2610.27889, 2611.07624, 2610.28201, 2610.22289, 2610.25078, 2611.12988, 2608.24641, 2609.00468.

## 2. Closest prior work (all verified in tool results)

| Paper | What it does | Gap it leaves |
|---|---|---|
| **Optimize Cheap, Deploy Strong** 2608.10694 (IBM, Aug 2026) | Evolves GEPA prompts on a cheap tier, deploys them zero-shot up the tier ladder (Claude nano→mini→Haiku→Sonnet, GPT nano→mini→luna, Gemini flash-lite→3.5-flash→2.5-pro, plus local Qwen3-8B), 3 seeds. Each tier also gets its *own* full-cost optimization, and they report a seed-prompt row on every tier. **Finding: upward transfer is positive.** The transferred prompt matches or beats own-tier optimization in 36/48 setups (mean +2.8%), and the worst shortfall is 6.1%. Lateral transfer is known to degrade. On "prompt-insensitive" top tiers the gain over the seed shrinks (e.g. IFBench@luna seed 64.3 vs 67.9). | **This is the closest scoop for the "keep vs cold re-evolve" arm**, but only for single-module GEPA *prompts*, across price *tiers* rather than same-family *versions*. There is no warm-start arm, no code harnesses, and no edit-level attribution. **Its result points against the idea's predicted outcome**: transferred prompts are not harmful and are about as good as re-optimizing. |
| **Harness Evolution Hits a Ceiling** 2610.11655 | 2×2 of seed/evolved harness × base/LoRA weights on DeepPlanning and WebArena-Lite (Qwen3.5-4B/9B, 9 lines across 6 families). Adapters trained on evolved-harness trajectories *absorb* the harness gain (9B: adapter alone equals full evolution, stacking adds +0.007). Its rule: gains from edits to what the model *writes/does* train in, while gains from edits to what it *sees* (two pages of context) stay in the harness. | Absorption happens through *deliberate distillation of the harness's own trajectories*, not through an independently trained successor. **The idea's mechanism claim ("only see-edits survive") is the Ceiling rule moved over to upgrades**, so reviewers will call it predicted. Whether the rule holds for vendor upgrades is still open. |
| **Harness-Delta Attribution (HDA)** 2610.22466 (Amazon/NYU) | Splits evolution gains into overfitting, test-time scaling and residual. Meta-Harness/SkillOpt/AHE across 11 executors, **including Qwen3-8B, Qwen3.5-0.8/2/4B and Qwen3.6-27B/35B-A3B, each evolved separately**. 54% of gains come from overfitting and 14% from TTS. Held-out gains are often below search gains. Code and harnesses are released. | No cross-model carry-over. **Its released per-model harnesses on a Qwen3→3.5→3.6 ladder make a <$50 pilot possible.** It also warns that ALFWorld/LiveMath are artifact-ridden, and benchmark shortcuts are model-agnostic, so they would "survive" an upgrade spuriously. |
| **Elicitation or Expansion?** 2610.27889 (Oct 2026) | Meta-Harness on Qwen3-8B vs a base-harness Qwen3-32B reference (and GPT-OSS-20B vs 120B, AppWorld effort). Harness pass@1 gains shrink at pass@k and are mostly elicitation. On hard tasks, model scaling beats the optimized harness at every k. | Compares harness gain with model-scaling gain, but never carries the harness *to* the bigger model. **It supplies the mechanism the idea needs**: elicitation gains should be absorbed by stronger models and expansion gains should survive. |
| **Beneath the Surface** 2611.07624 (Nov 2026) | AHE/HyperAgents/A-Evolve/GEPA × MiniSWE/Codex on τ³-Banking and TB2. Transfer to τ²-bench and DeepSWE: coverage falls on general tasks and rises on coding. | Transfer is across *benchmarks*, not models. |
| **MaD-RSI** 2610.28201 | Optimizes a shared harness across 4 heterogeneous LLMs (DeepSeek-V4/V3.2, Qwen3.8-Flash/27B) and tests 2 unseen models. Claims single-model evolution couples the harness to its model. | A method paper with no successor-model design, no keep/warm/cold arms and 1 run. It is useful motivation ("harness coupled to target LLM"). |
| **What Survives the Next Model?** 2609.00468 (UVA, Aug 2026) | 35 ICSE-2026 LLM techniques vs a single prompt on a newer model: 37–63% are replaced, with a "half-life <1 year". Search/selection and structured-reasoning techniques are most absorbed; knowledge grounding and domain processing survive. | **It already uses the "half-life / bitter lesson" framing**, but for *hand-engineered SE tools*, not evolved harnesses, with no re-optimization arms. Its "provide additional insights survives" result anticipates the "what the model sees survives" claim. |
| **Aging of Prompt Engineering Techniques** 2608.24641 (ICSME 2026) | 5 hand PETs × GPT-3.5→4o, Qwen2→2.5, Mistral-7B→Large. Aging depends on the family: GPT absorbs PETs, while Qwen keeps or grows the benefit. | Hand PETs, 3 seeds, one code benchmark. This is a warning that the Qwen family may *not* absorb. |
| Improving Environment-Agnostic Agents 2610.22289 | Hand scaffold gap on ARC-AGI-3 shrinks from GPT-5.5-high to GPT-5.6-sol-max (25 → 7 RHAE) but persists. | Hand scaffolds, one model ladder. It does show partial absorption. |
| Finding the Right Fit 2610.00917; Memory Portability 2609.05339; CoEvo-RSI 2610.25078; Third Eye 2611.12988 | Fixed harness × model interactions (rankings reverse); memory stores across model upgrades; brain-body interface mismatch in co-evolution (TIMR); self-update value after a *further weight update*. | None of these carries an *evolved* harness through a vendor upgrade with re-evolution arms. Third Eye has the closest "survive the next update" framing, but for LoRA self-updates. |

**Verdict on novelty:** nobody has run the exact design (evolved code/prompt harness, same-family successor, keep/warm/cold at matched budget, edit-level attribution). But four pieces are already out: (i) prompt-level keep-vs-reoptimize across tiers (2608.10694, which found survival); (ii) the sees-vs-writes mechanism (2610.11655); (iii) "half-life" and bitter-lesson framing for hand techniques (2609.00468, 2608.24641); (iv) harness gain vs model-scaling gain (2610.27889). So scoop risk is **medium**: no K1, but two to three close papers in the last four months.

## 3. Mock NeurIPS review

**Summary.** The paper asks whether harness-evolution gains persist across same-family model upgrades. It proposes retained-gain and absorption metrics, edit-level ablations and keep/warm/cold re-evolution arms.

**Strengths.**
- It is a practical and timely question. Every harness-evolution paper silently assumes the artifact is worth its search cost, and a model upgrade is the most common real-world way that assumption breaks.
- Matched-budget keep/warm/cold arms are clean, and the HDA-style overfitting audit is a strong control.
- It is cheap: open-weight models and no training.

**Weaknesses.**
1. **Novelty is eroded on three sides.** 2608.10694 already compares transfer with re-optimization at cost for GEPA prompts, and finds positive upward transfer. The "sees vs writes" survival rule is from 2610.11655. The "half-life" framing is from 2609.00468. A reviewer will write: "a careful replication of known effects on a new axis (versions)".
2. **The statistical unit is the transition, and there are very few of them.** Qwen3→3.5→3.6 gives two transitions per size. A reviewer will ask whether "most gain is absorbed" is a law or a fact about Qwen's 2026 post-training. Note that Aging-PETs found *Qwen* the family that does *not* absorb prompt techniques.
3. **Version changes are confounded.** Qwen3→3.5→3.6 changes tool-call format, thinking defaults, context length and chat template. "Harmful" carry-over may be interface mismatch (CoEvo-RSI's TIMR), which is trivial. It does not show that compensation turned into a liability.
4. **The predicted outcome may simply be false.** OCDS shows prompts transferring *upward* without loss. If the result is "gains mostly survive", the paper needs a framing that survives that outcome.
5. **"Warm ≈ cold" is a null claim.** Without a power analysis and a plasticity mechanism, a null warm-vs-cold result reads as noise. Harness-evolution runs are high-variance (HDA, Beneath the Surface).
6. **Meta-agent fit is weak.** Nothing self-referential is studied. It is an evaluation-of-artifacts paper.
7. **Benchmark artifacts make survival look high.** HDA showed that evolvers rediscover benchmark shortcuts, which are model-agnostic and so would "survive" every upgrade. Without neutralization, "survival" measures overfitting.

**Likely reviewer phrases:** "useful empirical study / workshop-ready", "incremental over 2608.10694 and 2610.11655", "engineering study of model versions". Likely scores as proposed: 4/4/5 (borderline reject). With the fixes below: 5/6/6 (borderline to weak accept).

## 4. Scores (rubric)

| Dim | Score | Reason |
|---|---|---|
| N | 3.0 | Exact design unrun, but 2608.10694 (keep vs re-optimize, prompts, tiers), 2610.11655 (absorption by weights; sees-vs-writes) and 2609.00468/2608.24641 (half-life of hand techniques across versions) are all within the last 4 months. |
| S | 3.0 | Both outcomes are publishable ("harness investment depreciates" vs "it is durable"), but the headline is close to a bitter-lesson cliché and the mechanism is half-predicted by Ceiling. A null warm=cold result is weak. |
| R | 3.0 | It concerns long-horizon self-improvement under changing conditions, but not self-referential meta-agents. It improves to about 4 if framed as plasticity of the evolver lineage (see fixes). |
| C | 3.5 | Matched budget, seeds, 2–3 benchmarks and audits are good. The few transitions and the version confounds limit generality. |
| F | 3.5 | Open models, no training, and a pilot under $50 using HDA's released harnesses. But the full grid (3 evolvers × ~6 models × 3 benchmarks × 3 seeds × keep/warm/cold) likely exceeds $1.5k with an Opus-class proposer, so it must be trimmed. |
| T | 4.0 | Harness evolution and bitter-lesson-for-scaffolds are hot in Oct–Nov 2026, so the idea rides the trend but is crowded. |

**Total = 20 × (3·3 + 2.5·3 + 2·3 + 2·3.5 + 1.5·3.5 + 1·4) / 12 = 20 × 38.75 / 12 ≈ 64.6.** This sits in the "viable with fixes" band.

**Kill flags:** K1 no (no 2026 paper has the same central claim and design). K2 no (survival and absorption are both reportable, though warm=cold alone is weak). K3 no. K4 no. **Soft flags:** the framing collides with 2609.00468 ("half-life"), the mechanism claim collides with 2610.11655, and OCDS evidence contradicts the predicted direction.

## 5. Fixes that raise the score

1. **Replace the Ceiling-taxonomy mechanism with elicitation vs expansion as the predictor (largest gain in S and N).** Hypothesis: gains that raise pass@1 but not pass@k on v_n (elicitation, 2610.27889) are absorbed by v_{n+1}. Gains that expand pass@k coverage or add information or observations survive. Test it *prospectively*. Compute each edit's elicitation/expansion score on v_n only, predict its survival on v_{n+1}, and report rank correlation. That is a falsifiable, v_n-only forecast of harness depreciation, and it is new.
2. **Turn n=2 transitions into a dose-response curve.** Add many "upgrades" with known capability deltas: Qwen3→3.5→3.6 at 2–3 sizes; one more family (Gemma 3→4 or Llama 3.1→3.3 / GPT-OSS); plus *controlled* upgrades, i.e. intermediate RL/SFT checkpoints of one open model (OLMo-3 or similar released checkpoints). Plot retained-gain fraction against the capability delta of the *seed* harness. A slope ("harness half-life per point of base-model gain") is a reusable law. A single Qwen anecdote is not.
3. **Recast warm vs cold as plasticity**, which brings it to the mentor's agenda and Dan's "meta-agent plasticity" line. A warm-start lineage carries archive, lessons and accreted code. Test whether it shows loss of plasticity, i.e. lower gain per unit budget on v_{n+1}. Add a "reset" arm in the shrink-and-perturb spirit: keep only edits flagged as expansion or observation, drop the rest, then re-evolve. Report plasticity as gain per budget, using the Agent Plasticity cost-normalized metric (Singh et al. 2026). This turns a null warm=cold result into a measured plasticity gap and gives an actionable recipe.
4. **Control version confounds.** Pin chat templates, use a neutral tool adapter (as in 2611.07624), and fix thinking mode. Report interface-mismatch rate (CoEvo-RSI's TIMR) separately, so that "harmful" means behavioral harm, not format breakage.
5. **Neutralize artifacts before measuring survival.** Run HDA on every v_n harness. Report survival of O (overfit), T (test-time scaling) and G (residual) separately. Prediction: O survives (model-agnostic shortcuts), T is partly absorbed, and G splits by elicitation/expansion. That separation would itself be a headline.
6. **Add a "keep + matched test-time scaling" baseline.** After Rethinking-Eval (2607.12227), carrying no harness and spending the re-evolution budget on parallel sampling with v_{n+1} is the fair alternative.
7. **Position explicitly against 2608.10694.** Reconcile tier transfer (positive, prompts) with version transfer (code harnesses). If the prompt-only arm (GEPA) survives while code harnesses (Meta-Harness/AHE) depreciate, the paper has a clean story: "what depreciates is control-flow compensation, not instructions".
8. **Trim the grid to fit the budget.** Use 2 evolvers (Meta-Harness-style code + GEPA prompt), 2 low-artifact benchmarks (e.g. τ²/τ³-bench and a SWE-bench Verified subset, or AppWorld), 3 seeds, and one fixed mid-cost proposer across all arms. Pilot first by cross-applying HDA's released Qwen3/3.5/3.6 harnesses (<$50).
9. **Rename.** Avoid "half-life" as the title hook, since 2609.00468 owns it. Something like "Depreciation of evolved harnesses" or "Do harness gains outlive their model?".

## 6. Sharpened headline claim

> When a harness evolved for v_n is moved to its same-family successor, the gains it obtained by *elicitation* are absorbed by the new model and the *expansion* gains survive. This can be forecast from v_n alone: an edit's pass@1-vs-pass@k profile on v_n predicts its survival. At matched budget, warm-started re-evolution shows a measurable plasticity deficit relative to a reset-to-expansion-edits restart.

Both directions are publishable. If elicitation and expansion do not predict survival, the paper reports that harness depreciation is unpredictable from the source model, which is a practical warning for harness-evolution evaluation.

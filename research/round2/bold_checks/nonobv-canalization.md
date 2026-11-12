# Bold check: nonobv-canalization ("Selection manufactures fragility")

**Verdict: WEAK as written (65.8/100). It is viable only after a reframe, and should probably merge with round-1 `breaking-the-improver`.**

The headline claim has a dose-response form: "self-improvement raises clean success while fragility rises with generations." Three neighbouring papers from Oct 2026 already own that claim:

- **CODA**, which states the dose-response explicitly.
- **2610.39122**, which tracks injection success (ASR) over self-evolution steps.
- **Masked Erosion**, which tracks margin loss over harness generations.

What is still open is narrower:

- harness evolution, measured against **environment-side controlled interventions** (BreakingWeb layers and primitives);
- a **matched-accuracy** comparison against hand-written harnesses;
- above all, **whether gate-time intervention exposure generalizes to held-out intervention families**. CODA lists this as future work and nobody has tested it.

## 1. Scoop searches run

1. alphaXiv (recency): self-evolving / harness evolution, robustness, generations, environment perturbation.
2. alphaXiv (recency): canalization, genetic assimilation, robustness, LLM agents, evolution. Nothing found in the ML-agent context; only biology papers came back (e.g. 2607.20915).
3. alphaXiv (recency): prompt optimization / GEPA robustness vs. perturbation and overfitting.
4. alphaXiv (recency): robustness-aware gates, Masked Erosion, HarnessCompass, EvoArena.
5. Semantic Scholar keyword searches (3 phrasings, 2026-05 onward). These surfaced HarnessCompass 2608.01918, EvoArena 2606.13681 and SPrPO 2609.34064, none of which is a scoop.
6. S2 forward citations:
   - 2610.39122: 0 citers.
   - BreakingWeb 2609.35814: 1 citer (EMBERS 2611.03723, deep-research fuzzing, not a scoop).
   - Fragility of Self-Improving Agents 2608.18066: 6 citers (CARE, Lessons from Luck, Reels-RSI, Winner's Curse 2610.09239, Component Routing, S3Gym). None makes a dose-response robustness claim.
7. WebSearch for OpenReview / ICLR 2027 submissions. No OpenReview hit. It surfaced 2605.24539 (Liar's Dice harness evolution unreliable), SafeEvolve 2609.02786 and an EvoUndo news item (unverified).

**Deep reads (answer_pdf_queries):** 2610.20845 CODA, 2610.38392 Masked Erosion, 2610.39122 Appreciation/IPI.

## 2. Closest prior and the gap each leaves

| Paper | What it already claims | Gap left for this idea |
|---|---|---|
| **2610.20845 CODA: Skill–Policy Co-evolution Trap** (Oct 14) | **Biggest threat to the framing.** "Process that raises peak performance also creates sensitivity." Fragility curves along training show a significantly positive drop slope in 5 of 6 settings, plus a linear-Gaussian **"Proposition 3 (Dose–response)"**: clean return and fragility both rise with the dose. Fragility tracks *dependence* (ρ=0.82). Cause: cooperative acceptance (selection) puts the delivered pair at a local peak. Its L2 limitation explicitly leaves the fix ("training against a distribution of bank perturbations") as **future work**. | Perturbs the agent's *own skill bank*, not the environment. RL weights plus skills, not harness evolution. No fix, no held-out perturbation family, no matched-accuracy hand-written control. |
| **2610.39122 Appreciation Teaches Unauthorized Execution** (Oct 29) | Self-evolution (skills, RL) raises IPI ASR by 14–49 pp. Figure 3 gives **ASR vs updates/conversations**, a step-like rise. Clean task completion stays ≥85% ("measuring task completion alone does not reveal the loss of robustness"). The fix, contrastive authorization-aware feedback, drops executed injections from 43/120 to 7/120. Transfers across injection *forms*. | Adversarial, single intervention class (IPI), personal-agent skills. Not harness evolution or environment-dynamics change. No held-out *family* test for the fix. |
| **2610.38392 Masked Erosion** (Oct 28) | Harness self-improvement gates delete redundant safeguards generation by generation (margin falls by generation). Lost depth turns into harm under a **model swap** (an intervention). Gate fixes: paired test, margin-aware, read-only text. | Harm and safeguards, not task robustness to environment change. Interventions are model swaps, not environment changes. |
| 2611.07624 Beneath the Surface | Same evolver set (AHE, HyperAgents, A-Evolve, GEPA). Cross-benchmark transfer is weak. **Evolvers insert "verify results" reminders.** | Shows the direction could be *opposite* on BreakingWeb's dominant false-success failure. |
| 2608.18066 Fragility of Self-Improving Agents | Variance and task-order fragility of memory agents. | Not environment interventions. |
| 2609.35814 BreakingWeb | Paired intervention suite. | Static agents only, 1 citer. Still uncontested as an evaluator of self-improvers. |

No paper runs (harness evolution) × (paired environment interventions) × (per-generation slope) × (matched-accuracy hand-written control) × (gate-time intervention injection evaluated on held-out families). K1 does not fire. But CODA, IPI and Masked Erosion together pre-empt the **surprise** of "selection manufactures fragility." A reviewer will cite CODA's Proposition 3 against the headline.

## 3. Area-chair critique

- **Sharpness.** The falsification criterion is crisp: a robustness gap ≥0 at matched accuracy, or no negative slope. The "Waddington assimilation" arm is the interesting part. The biology analogy is a label, not a mechanism; avoid leaning on it in the title.
- **Direction risk.** Beneath the Surface found that evolvers add verification. BreakingWeb's largest failure class is fabricated success. Evolved harnesses may therefore become *more* robust on the verification primitive and less robust on others. Preregister *per-primitive* signs. A sign-split result (gains on verification, losses on belief revision and stale-world-model primitives) is more interesting than a uniform decline.
- **Arm B is predictable unless it uses held-out families.** "Putting interventions in the gate helps on those interventions" is trivial. The publishable question is whether assimilation **generalizes across primitives**: does it produce general world-state-checking routines, or memorized patches per family? Either outcome is publishable:
  - Generalizes: a cheap recipe for robust self-improvement.
  - Does not generalize: "gate exposure buys family-specific patches", a negative result on robust RSI.
- **Matched-accuracy control.** Good and novel; neither CODA nor IPI has it. To build it, use early checkpoints plus hand-tuned harnesses, then fit robustness against clean accuracy on the pooled set and test lineage-generation as an extra predictor. This is the cleanest way to separate "selection does it" from "accuracy does it."
- **Power.** BreakingWeb has 519 pairs across 7 primitives. Held-out 4-family cells hold about 30–70 pairs each. A slope over 10 checkpoints × 6 lineages needs a mixed-effects model with lineage random effects and k≥3 evaluation passes. That is feasible, but evaluation cost dominates.
- **Cost realism.** 6 lineages × 20 generations × (gate ~40 tasks) plus 10 checkpoints × 6 lineages × (clean + intervened, roughly 2×300 tasks × 3 passes) comes to about 100k browser rollouts of 30–40 steps each. **$800–1.1k is only plausible with a cheap open model** (Qwen3-class via cheap API or a local vLLM) and a τ-world procedural environment for most of the runs, with BreakingWeb used for a confirmatory subset. Plan the τ-world as the primary environment.
- **Mentor fit.** High. It uses Xunjian's BreakingWeb as the evaluator of self-improvers (Gödel-agent lineage) and frames long-horizon self-improvement cost.

## 4. Rubric

| Dim | Score | Rationale |
|---|---|---|
| N (×3) | 2.5 | No exact scoop. But the dose-response fragility claim is made by CODA (with a proposition), by IPI (ASR over steps) and by Masked Erosion (margin over generations), all in the last month. The open slice is environment-side, held-out assimilation and matched accuracy. |
| S (×2.5) | 3 | The headline is now expected. The held-out assimilation question is still two-sided and publishable. |
| R (×2) | 4.5 | BreakingWeb plus self-improvement plus long-horizon. |
| C (×2) | 3.5 | Good controls (matched accuracy, held-out families, lineages). Power and single-evolver risk. |
| F (×1.5) | 3 | Browser rollouts are heavy; needs an open model and τ-world as the primary environment. |
| T (×1) | 4 | Hot: evolution evaluation and robustness. |

**Total = 20 × (7.5 + 7.5 + 9 + 7 + 4.5 + 4) / 12 = 65.8** (viable with fixes, below the 75 bar). No kill flag fires. K4 is borderline: "BreakingWeb + harness evolution" is a combination unless the assimilation result carries the paper.

## 5. Sharpened claim (recommended reframe)

> **"Robustness does not assimilate through the gate."** In harness evolution, adding controlled environment interventions from k primitive families to the acceptance gate makes evolved harnesses robust to *those* families. However, robustness to *held-out* families keeps the same negative per-generation slope as clean-only evolution (difference in slopes ≈ 0), and at matched clean accuracy both are less robust than hand-written harnesses.
>
> **Falsified if** gate-exposed lineages show a held-out-family slope significantly less negative than clean lineages (true assimilation). That would be the positive, method-paper outcome.
>
> **Secondary test:** whether the held-out transfer depends on gating by *cognitive primitive* (belief revision, verification) rather than by *surface layer* (DOM, API, content). This is where BreakingWeb's taxonomy gives a unique handle that CODA, IPI and Masked Erosion lack.

Positioning sentence for the paper: "CODA shows co-evolution grows fragility to the agent's *own* skills; IPI and Masked Erosion show it for attacks and safeguards. We show that harness evolution grows fragility to *the world changing*, and we test whether selection-time exposure can buy general rather than family-specific robustness."

Cheapest pilot (<$50): 2 lineages × 8 generations of GEPA-style harness evolution on a τ-world procedural environment with 2 gate families and 2 held-out families, on an open model. Check the sign of the per-primitive slope before committing.

## 6. Papers cited (all seen in tool results this session)
2610.20845, 2610.39122, 2610.38392, 2611.07624, 2608.18066, 2609.35814, 2611.03723, 2610.09239, 2608.01918, 2606.13681, 2609.34064, 2605.24539, 2609.02786, 2607.12227. EvoUndo is unverified (news writeup only).

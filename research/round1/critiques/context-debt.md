# Critique: Context debt. Do meta-agents plateau because they anchor on their own history?

Idea id: `context-debt` (T2, meta-agent plasticity). Reviewed 2026-11-12.
Verdict: **viable with fixes (65/100)**. The question has become crowded since October 2026. Four papers in six weeks attribute agent plateaus to accumulated history and reset it. Every one of them uses **discovery tasks**, where the search score is the objective. None of them uses **harness or meta-agent evolution judged on held-out tasks**, and none runs a clean keep/drop factorial over the components of state. The idea survives only if it moves to that ground and makes a held-out claim. A claim about search score alone is not enough.

## 1. Scoop search

Searches run:
- alphaXiv discover (recency), 4 calls: forgetting/reset/anchoring in meta-agents; the Scaling Discovery Requires Forgetting, Fork-and-Flush and plasticity cluster; optimizer-history ablations (GEPA, Meta-Harness, stateless agents, Hill Sampling); false floor, premature convergence and anytime-valid stopping.
- Semantic Scholar, 4 keyword phrasings for 2026 (context reset/anchoring/plateau; evolver history anchoring in harness evolution; LLM evolutionary search stagnation with history ablation; meta-agent plateau and context accumulation).
- S2 forward citations of 2610.20563 and 2610.07447. The index is sparse: it lists only RSIHub 2610.32253, Athanor 2610.21055 and the Forgetting paper itself.
- Deep reads: 2610.20563 (report), verified-archives rebuttal (full text), 2610.07447, 2610.07625, 2610.24291, 2610.41157, 2611.03219 (targeted PDF queries).

### Closest papers (all seen in tool results)

| Paper | Date | What it does | Gap it leaves |
|---|---|---|---|
| **Scaling Discovery Requires Forgetting** (Tan, ..., Dimakis, Zaharia), 2610.20563 | 2026-10-14 | 4 DeepSeek-V4.1-Flash agents share commits. Three times per run they forget the session *and* the candidate history and keep only the best candidate. Wins on AHC014, Frontier-CS Q171, VLIW and nanochat, 3 runs each. A continuation test from a plateau: forget gives +4.5–9.2% vs continue +1.4–1.9%. Edit size is 40 vs 6 lines after a reset. Names "anchoring" as the mechanism. | Keeps both or drops both, so it cannot attribute the plateau to a component. Fixed schedule, no trigger study. Discovery tasks only (score = objective). No held-out set, one model, 3 seeds. The authors list *when to reset* and *what to retain* as future work. |
| **Scaling Discovery Does Not Require Forgetting: Verified Shared Archives** (Cumes, QWNTL), alphaXiv `2611.verified-archives-multi-agent-search` | 2026-11-03 | Makes exactly our core critique: forgetting deletes two things at once, the *scored attempts* and the *conversation with unverified conclusions*. Design: keep every scored attempt, keep conversations short (at most 3 continue prompts, then a fresh session from an archive summary), and mark the agent's own proposals UNVERIFIED. Opus 5.5 on the Anthropic take-home: 914 vs 948 (CORAL port) and 946 (isolated agents), 3 runs, matched hours. Coins the term **"false floor"**: agents declare roughly 951 cycles near-optimal and stop. A lead-agent variant with one conversation for the whole run still wins. | Compares system designs; it is **not a factorial**. Conversation length, archive sharing, tool sharing and the insight compiler change together. One main task plus open problems, Claude Opus only, 3 runs, no held-out generalization, no harness evolution. Not on arXiv (alphaXiv-only), so its visibility and peer-review status are unclear. |
| **Fork-and-Flush** (Cai, Ziakas, ..., Papailiopoulos; MSR), 2610.07447 | 2026-10-05 | Fork K=4 from the workspace with a fresh chat, keep the best. 13 tasks, matched credits, 5 runs. 2×2 ablation: flush-only 0.55, fork-only 0.57, both 0.71, single 0.45. Notes that **compaction is insufficient**. Plateau-detection triggers "rarely fired" (15/25 runs never triggered), so they use a fixed schedule. | Has a flush-vs-fork ablation but never separates workspace from notes from conversation. Discovery tasks only, no held-out set. The trigger question is open and their negative result on detectors is a direct warning for our "anytime-valid trigger" arm. |
| **Stateless Language Agents** (Zhang, ..., Olukotun; Stanford), 2610.07625 | 2026-10-06 | No agent carries a conversation; the harness owns the research state; an Advisor assigns work. Checkpoint-continuation ablations of what agents see. Up to 1B tokens; rankings flip with horizon. | The authors say: "Our ablations change what each agent sees, not whether it keeps its conversation across invocations." That is precisely our factor. Discovery/SWE tasks, mostly single runs. |
| **Principle-Level Local Optima** 2610.24291 | 2026-10-16 | 161 runs: 78% show principle-level collapse with headroom. Diversity injection fails; a reference-principle hint helps only inconsistently. Uses a decision tree (headroom ∧ narrowing ∧ plateau) to separate **benign convergence from collapse**. | Has no reset/history arm. Its headroom-vs-saturation diagnostic is exactly the "saturation reading" our idea needs to operationalize. Reuse it. |
| **Lineage Is Memory** 2610.41157 | 2026-10-30 | 1,750 preregistered runs on GEPA-like loops. Inheriting from the parent beats seed-reset by +0.24 held-out, even when the reset proposer gets all failures clustered. | Opposite direction: state carried *in the candidate* is the main source of progress. Short budgets (8–12 evals), where plateaus do not occur. This sets the null for our "forget-to-best" arm in prompt evolution. |
| Can Coding Agents Sustain Continual Evolution w/o Forgetting? 2611.03219 | 2026-11-02 | Catastrophic forgetting across task types in AHE and Meta-Harness. | Different "forgetting" (of the target, not of the improver). Not a scoop. |
| Also relevant | | When Is Enough Enough 2610.04756 (stopping); Agent Plasticity 2610.08902; Do agents need to sleep? 2611.02216; Context Rot in long-horizon search 2606.29718; Meta² 2611.05139; DREvo 2607.26722; HarnessX 2606.14249 (post-peak collapse); Rethinking Eval 2607.12227. Unverified, seen only as references: Hill Sampling 2609.25510 (one thread conditioned on the best program); CORAL; "Liu et al." (long coding-agent sessions fall below independent sampling as they grow). | |

**What all of them leave open:**
- (a) No paper runs a **factorial over conversation × scored archive × unverified notes** with matched compute.
- (b) No paper studies plateaus in **harness or meta-agent evolution judged on held-out tasks**. There, a "plateau" can also be the evolver overfitting the search split, the winner's curse, or saturation of the target model rather than anchoring.
- (c) No paper studies **self-referential (Gödel-style) improvers**. There, "history" also includes the improver's *own modified code*: a fourth state component that is unique to the mentor's agenda.
- (d) No paper gives a pre-registered **anchoring vs saturation** decision rule. The verified-archives paper's "false floor" is anecdotal.

## 2. Mock NeurIPS review

**Summary.** The paper asks whether late plateaus of harness-evolving meta-agents come from the improver anchoring on accumulated state or from saturation. It runs a keep/drop factorial over conversation, scored archive and unverified notes for three evolver families, adds fork-and-flush and forget-to-best arms, and compares an anytime-valid reset trigger with a fixed schedule.

**Strengths.**
- Separates a confound that the immediate predecessor (2610.20563) admits it cannot separate. That paper's own rebuttal (verified archives) argues the point but never tests it factorially.
- Matched compute, 5 seeds, multiple evolvers and held-out evaluation fit the field's current standards (2607.12227).
- Clean behavioural mechanism measures: edit size, novelty decay, false-floor declarations.

**Weaknesses.**
1. **Novelty is thin and shrinking.** Tan et al., Fork-and-Flush, SLA and the verified-archives paper all appeared within 6 weeks. A reviewer will say: "the community already knows resets help, and the verified-archives paper already argues the archive should be kept. This is a factorial ablation of known components." Without a twist the paper reads as *incremental / analysis-only*.
2. **The harness-evolution setting may have no plateau to explain.** Harness evolution usually runs about 10–50 iterations, and Lineage Is Memory shows short-budget loops gain most from carried state. If the evolvers are not run long enough to plateau with headroom, the experiment has no effect to measure. The paper must *show* a late plateau with demonstrable headroom first.
3. **"Recovering held-out gain" conflates several mechanisms.** A reset also changes how proposals are sampled: it raises exploration and acts like a restart in stochastic search. Without a *matched-diversity control* (e.g. continuing with a higher temperature or "propose a radically different edit" prompts, at the same budget), a gain from resetting does not identify *anchoring*. 2610.24291 shows diversity injection often fails, which helps the argument, but it has to be shown in this setting.
4. **The saturation reading is under-specified.** "No recovery" can also mean the reset arm is too weak. Stating saturation needs an independent headroom witness: a stronger improver, a human-written harness, or an oracle best-of-many-harness ceiling on held-out tasks.
5. **Trigger arm.** Fork-and-Flush reports that plateau triggers rarely fire. The anytime-valid trigger may never fire either, which yields a null with no interpretation.
6. **Scale/statistics.** Five seeds × 3 evolvers × (8 factorial + 3 extra arms) is about 165 runs. At $600–1.2k that is only feasible with cheap models and short runs, which in turn makes weakness 2 more likely.

**What would make reviewers say "workshop".** A single benchmark; only a search-score metric; no saturation witness; an effect that appears only in GEPA-style prompt evolution.

**Likely score.** Borderline: 4–5 (weak reject to borderline accept) as proposed. A 6 is possible if the held-out twist (below) produces a clean dissociation and the paper ships a diagnostic protocol.

## 3. Rubric scores

| Dim | Score | Reason |
|---|---|---|
| N | 2.0 | Four close papers within 6 weeks (Tan et al.; Fork-and-Flush; SLA; verified archives), and the verified-archives paper makes the conversation/archive split explicitly. The remaining gap is real but narrow: a factorial, harness evolution on held-out tasks, and self-referential improvers. |
| S | 3.0 | Either outcome is reportable, but "resetting helps" is now expected. The surprise must come from dissociating search score from held-out score, or from the claim that the improver's self-modified code is the anchor. |
| R | 4.0 | Long-horizon self-improvement and meta-agent plasticity; a Gödel arm makes it core to the mentor's agenda. |
| C | 3.5 | A factorial with matched compute and held-out evaluation is rigorous. Risks: no plateau in short harness runs, no diversity control, no saturation witness. |
| F | 4.0 | Cheap open models (Qwen3.5-9B/27B, DeepSeek V4 Flash) run GEPA and Meta-Harness loops; the pilot needs under $50. |
| T | 4.5 | The hottest Oct–Nov 2026 thread. A fast follow by the Berkeley group, MSR or QWNTL is likely, so the window is short. |

**Total = 20 × (3·2 + 2.5·3 + 2·4 + 2·3.5 + 1.5·4 + 1·4.5) / 12 = 20 × 39 / 12 = 65.0**

Kill flags:
- **K1: partial risk, not triggered.** No paper makes the same central claim with the same design (factorial + harness evolution + held-out). But the verified-archives paper makes the *same qualitative claim* ("keep scored attempts, drop the conversation and unverified conclusions") in discovery tasks. Check again in Jan and Mar 2027.
- K2: not triggered if the saturation witness is added; triggered as written (see weakness 4).
- K3: not triggered.
- K4: borderline. "Factorial of known reset components" combines existing things unless there is a new mechanism, new setting or new metric.

## 4. Fixes (ordered by impact)

1. **Make the held-out dissociation the headline.**
   - Measure the effect of each state component on *search-split score* and on *held-out score* separately.
   - Hypothesis: in harness evolution, the conversation/notes carry anchoring that hurts search progress, while the scored archive carries *overfitting* (winner's curse) that hurts held-out generalization.
   - A dissociation such as "dropping the conversation recovers search progress but not held-out gain; dropping the archive does the reverse" is a result discovery-task papers cannot get, because they have no held-out split.
2. **Add the self-referential state component (Gödel arm).** For improvers that edit their own code, add a fourth factor: *improver code* (keep the self-modified improver vs revert it to the initial improver while keeping the archive). This tests "context debt" against "code debt", connects to Gödel Agent's complexity ceiling and to the mentor's agenda, and no prior paper has it.
3. **Add a matched-diversity control.** Continue the run with exploration forced through temperature, "propose a structurally different edit" prompts or random-parent sampling, at the same budget. A reset effect beyond this control is evidence for anchoring *specifically*.
4. **Add an independent saturation witness.**
   - Estimate held-out headroom independently: an oracle ceiling from a stronger improver or from best-of-many independent evolutions at 4× budget, and a human or strong-model harness.
   - Adopt the headroom ∧ narrowing ∧ plateau decision tree from 2610.24291 so that "no recovery" can be read as saturation only when headroom is absent. This also fixes K2.
5. **Establish the plateau first (pilot gate).** Run each evolver 3–4× longer than its default (e.g. GEPA to 150+ proposals, Meta-Harness to 40 iterations) on 2 benchmarks and check for a late plateau with headroom. If none exists in harness evolution, pivot the claim: "short-horizon harness evolution does not plateau from anchoring; reported ceilings are saturation or overfitting". That result is still publishable and contradicts the trend.
6. **Use checkpoint-continuation from shared states, as SLA does.** Fork every arm from the *same* plateaued checkpoint, which gives paired comparisons and lower variance. This is cheaper than full runs and lets the study use 5–8 seeds per cell.
7. **Recast the trigger arm as a measurement.** Report how often the anytime-valid detector (2610.04756) fires vs a fixed schedule, plus the rate of false-floor declarations. Do not promise that the detector wins.
8. **Release an artifact.** A "state-ablation harness" wrapper for GEPA, Meta-Harness and Gödel-style loops, with plug-in keep/drop of conversation, archive, notes and improver code, plus a false-floor detector. This raises C and gives reviewers something reusable.
9. **Scope and timing.** Two benchmarks (e.g. τ²-bench retail plus one held-out-capable coding or reasoning suite), two open models, 3 evolvers. Write the paper by Feb–Mar 2027 and post it to arXiv early, because the fast-follow risk is high.

With fixes 1, 2, 4 and 5, the expected scores are N 3, S 4, C 4 → 20 × (9 + 10 + 8 + 8 + 6 + 4.5) / 12 ≈ 76.

## 5. Sharpened claim

> In harness and self-referential evolution, plateaus come from *different* state components depending on what you measure:
> - The improver's conversation and unverified notes cause search-score stalls (anchoring, false floors).
> - The scored archive and the improver's self-modified code cause held-out stalls (overfitting and code debt).
> - Resetting all state, as discovery-task "forgetting" papers prescribe, fixes the first and can hurt the second.
>
> At matched compute, "flush the conversation, keep the verified archive" recovers most late search gain. Held-out gain is recovered only when the archive is re-validated on fresh tasks. Reported harness-evolution ceilings therefore mix improver anchoring, selection overfitting and true saturation, and a three-way diagnostic separates them.

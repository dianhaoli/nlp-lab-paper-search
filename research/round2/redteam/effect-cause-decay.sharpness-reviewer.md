# Red team (sharpness and soundness reviewer): "Do Self-Referential Improvers Get Better at Improving? Effect, Cause, and Decay"

*Reviewer persona: a harsh NeurIPS reviewer who cares about one crisp, falsifiable claim, causal cleanliness, and whether a null result can be read. Date 2026-11-12.*

*Sources: round-1 REPORT, proposals `meta-plasticity.md` and `procedure-vs-memory.md`, critique `swap-factorial.md`, and the round-2 verification digest (SelfSearch, MetaRSI, RSI-Opt, FreeEvolve, HMED, SelfImprove-Bench corrections).*

*Two new alphaXiv recency searches in this pass turned up nothing beyond the digest that runs a gen-k vs gen-0 improver-on-new-domain probe. I saw RSI-Env 2611.01736 (Fudan/HKUST, Nov 1) by title only, so whether it contains a swap or transfer table is **unverified**.*

---

## 0. Verdict in one paragraph

**Major revision. Score 67.9 as merged; projected about 76 after the cuts below.**

The merged idea is three papers stapled together:
- **Effect:** a same-start swap.
- **Cause:** an 8-arm procedure×memory transplant.
- **Decay:** a warm-start penalty on a new domain.

On top of those sit 5 reset arms, 5 mechanism readouts, 2–3 improver families, 2–3 domains, a 40-generation horizon and a robustness replicate. The "every branch is publishable" framing (memory wins / procedure wins / interaction wins / penalty / transfer / mixed) is hedging, not a claim. A reviewer reads six pre-announced outcomes as "the authors don't know what they expect".

Novelty is also no longer evenly spread across the three parts:
- **Effect is effectively taken.** MetaRSI runs a same-start, same-budget swap with 5 seeds on an open model. RSIFlow, FreeEvolve and Skill∞ each have one S0 swap, and 2610.27610 publicly asks for exactly this.
- **Cause is half-taken.** SelfSearch Table 4 has "w/o episode records" vs "fixed improver", and HMED and VERSE each have one cell.
- **Decay is the only clearly open piece.** Even there, RSI-Opt (2610.32026) already shows evolved-policy transfer peaking at generation 1, and Organon (2610.33046) and Agent Plasticity (2610.08902) name cross-task transfer as future work.

The paper should be **cut down to Decay, with exactly one causal contrast that explains it**. Its headline should be a directional, surprising prediction: **self-modification makes improvers meta-overfit.**

---

## 1. Is there one crisp, surprising, falsifiable headline? Currently: no.

| Symptom | Where | Why a reviewer punishes it |
|---|---|---|
| Three RQ families plus a reset zoo | REPORT "Core questions" 1–3; meta-plasticity RQ3 has 6 reset arms | "Three half-papers" is already the swap-factorial critique's verdict (weakness 7), and the merge made it worse. |
| Six outcome branches, all called publishable | Both proposals, §1 | No branch is a *prediction*. Falsifiability needs a direction the authors commit to before the data. |
| The penalty claim stacks 4 sub-claims | meta-plasticity §1: penalty exists, grows with log k, is larger for far B, is carried by procedure, and forgetting doesn't fix it | Each "and" lowers the chance the headline survives as stated. Reviewers will find whichever conjunct fails. |
| "Memory wins" is not surprising | procedure-vs-memory §1 | Faithful Self-Evolvers (2601.22436), RSI Tax (2611.13524), More Memory Worse Agents (2610.27406) and MetaSkill-Evolve's σx ablation already lean "memory carries it / condensed experience is inert". A "memory wins" result reads as confirmation. |
| A penalty from Pk+Mk on a new domain is trivial if memory drives it | meta-plasticity T1 | "Irrelevant domain-A notes distract the improver on B" is already known at the object level (2610.27406, 2610.25858 warm harness < seed). Only a penalty carried by the **memory-wiped, content-scrubbed procedure** is surprising. |

**What is actually surprising here.** The field's implicit belief, which Hyperagents' imp@50 transfer, FreeEvolve's fresh-worker gain and the whole "learning to improve" pitch rely on, is that *edits to the improver are domain-general* (better verification, better selection, better budget allocation). The surprising, falsifiable claim is the opposite: **the procedure edits that make an improver better at home are the ones that make it worse abroad.** This is meta-level overfitting, a specialization trade-off. One sign test on one contrast falsifies it.

---

## 2. Causal soundness: specific holes

### 2.1 Precondition missing: is there anything to decay?
The design has to start from an improver that really did get better at improving at home. If the gen-k procedure gives no gain on A over gen-0, then "no penalty on B" and "penalty on B" both mean nothing. The digest's evidence that open-model improvers barely self-modify:
- SelfImprove-Bench: 87.9% of retained edits touch prompts or memory, and **0% touch the improver** unless forced.
- 2609.17817: no Ollama model could drive Hyperagents.
- FreeEvolve: Haiku-class evolvers overfit the evolution set (35.0→52.2) while held-out goes 33.3→32.2.

**Fix: make in-domain Effect a gate, not a result.** Report the improver's home gain G_A(Pk,∅) − G_A(P0,∅) and require it to be above the SESOI before the Decay test is interpreted. FreeEvolve's Haiku pattern is the Decay hypothesis seen at a single point, so cite it as motivation. It is not a confound.

### 2.2 Cost, context and headroom confounds
- **Matched-$ penalizes long memory mechanically.** A warm Pk+Mk carries a longer context, so at matched dollars it gets fewer iterations. "Penalty under matched-$" can be pure token tax (RSI Tax).
- **Fix:** make the primary contrast memory-free (Pk^s+∅ vs P0+∅). Context length is then roughly equal and matched-$ and matched-calls coincide. Report procedure token length as a covariate.
- **Pk may be longer too.** Bloated code and prompts cost more per call. Add a length-matched P0 control (P0 padded with neutral generic text to |Pk|) so that "bigger prompt" and "evolved prompt" are not confounded.
- **Headroom.** On a fresh B seed agent, headroom is identical across arms by construction, which is one real strength of Decay over Effect. Keep that. Do not reintroduce a state crossing.

### 2.3 Domain content leaking into the procedure
If Pk contains airline-specific rules, a penalty on retail or games is plain specialization: obvious and not publishable. The procedure-vs-memory proposal already has the right tool, the **knowledge scrubber plus leak audit (§5.4)**, but it sits in the other half of the merge.

**Fix: Pk^s (scrubbed) is the primary arm.** The headline claim is that the penalty *survives* scrubbing. Report the scrub-aggressiveness curve. If the penalty disappears under light scrubbing, the honest paper is "the improver smuggles domain content into its code", which is still a decent finding.

### 2.4 Which Pk?
Incumbent, archive best, or last accepted? HMED shows branch credit disagrees in sign with matched Δ in 45–75% of revisions. Choosing the "best" Pk by A-score would build a winner's curse into the Effect gate.

**Fix:** pre-register "the incumbent improver at generation k (the one actually used to produce generation k+1)". Index generations by **accepted improver edits**, not loop iterations (the accepted-edit clock idea from leads-a, from Feedback Dynamics 2608.02636), so a lineage with three no-op generations is not called "older".

### 2.5 Power is overstated
meta-plasticity §5.7 computes SE(b) from N = 80 probes as if they were independent. They come from **5 lineages per family**, and probes in the same lineage share Pk. The effective n for a lineage-level effect is about 10 lineages across both families, or 5 per family. Between-lineage variance (SelfSearch has 2 lineages; Aspire documents regression-recovery artifacts) is likely larger than probe noise.

**Fix:** one family, **≥10 independent lineages**, 2 probe reps each. Primary test is a paired sign test or a lineage-level t-test on D = G_B(Pk^s,∅) − G_B(P0,∅) at a single pre-registered k, plus a lineage-level t on the home gain. With σ_lineage ≈ 4 pp and n = 10, the MDE is about 3.8 pp. That is honest and achievable. Drop the ln k slope as the primary test: a 5-point curve from correlated checkpoints is a figure, not a test.

### 2.6 Can a null be read?
A null is interpretable only if three conditions all hold:
1. the home gain passes the gate;
2. the manipulation check shows Pk ≠ P0 behaviourally, not just textually: proposal-category distribution shift on a fixed probe state, with a JS divergence CI excluding 0;
3. the MDE is reported.

Under those conditions, "evolved improver procedures carry their home gain to new domains with no measurable loss (|D| < 3.8 pp)" is a real positive result: the first controlled evidence that procedural meta-learning is domain-general. If the gate fails, the paper is dead. Say so in the pilot kill rule. Do not keep the "memory story" fallback, which is a different, weaker paper.

### 2.7 Near/far is uncalibrated
"Near vs far" with one pair of domains is anecdote. Either drop the distance axis, or, better and cheaper, make it a **dose**: 3–4 B domains ordered by a pre-registered distance measure (the zero-shot transfer gap of π_k^A). Then predict that D goes monotonically down with distance. A dose-response is what turns "we found a penalty" into "we found a law".

---

## 3. Sprawl: what an undergrad can do by May 2027

Current load: 2–3 families (Gödel-lite, HyperLite with forced meta edits, a meta-skill evolver), a τ-style generator still to be built, 40 generations, about 340 probes, 6 reset arms, 5 mechanism readouts, an LLM edit coder validated on 100 human labels, and a robustness replicate. That is a lab-year. F = 2.5 is generous.

**Cut list:**

| Cut | Reason |
|---|---|
| Effect as a section | MetaRSI and others own it. Keep only the home-gain gate (one number per lineage). |
| 8-arm transplant → 4 arms | Keep P0+∅, Pk^s+∅, P0+Mk, Pk+Mk. Shuffled, placebo, adapter and dose go to the appendix only if money is left. |
| 6 reset arms → 0 new arms | "Forgetting" (Pk+∅) and "procedure reset" (P0+Mk) are already transplant cells. Distill, prune, selectionist and instructionist are a methods paper. |
| Families 3 → 1 (+1 small replicate) | Gödel-lite is the mentor's home turf and self-modifies by construction. A replicate on one VERSE-style harness optimizer at a single k with 4 lineages answers "is this one system?". |
| 40 generations → k ∈ {0, k\*} | k\* is pre-registered as a fixed accepted-edit count (e.g. 15 accepted improver edits). A curve over 4 checkpoints is a secondary figure. |
| Build the τ generator → use existing generators | ALFWorld / ScienceWorld / SelfImprove-Bench games / Reasoning Gym all have seeded generators. Building the generator is a month of infra. |
| Mechanism readouts 5 → 1 | Keep the proposal-category distribution (it also serves as the manipulation check). |

The slim design is 1 family × 10 lineages × 4 arms × (A + 3 B domains) × 2 reps, about 320 short probes plus 10 lineages. That fits the ≈$1.5k budget and is runnable by March.

---

## 4. "Just measurement"?

As written, yes: an audit protocol plus a decomposition table, which reviewers call TMLR-style. Two things lift it:
1. **A directional law with a dose:** home improvability up, away improvability down, monotone in domain distance. This is a *trade-off*, which is a scientific claim, not a measurement.
2. **One actionable consequence, tested:** "keep memory, reset procedure" (P0+Mk) beats the object-level "forgetting" reset (Pk+∅) on a new domain. That gives Gödel-Agent-style systems a design rule. It costs nothing extra because both are transplant cells.

---

## 5. Where the field is moving (anticipation) and timing

- **Swap and transplant ablations are spreading into method papers:** SelfSearch, MetaRSI, RSIFlow, FreeEvolve, RSI-Opt. Expect a powered in-domain swap paper by the ICML 2027 deadline (late Jan). **That is why Effect must not be the anchor.**
- **Cross-task carryover is named future work** by Organon, Agent Plasticity and Forgetting (2610.20563). The groups most likely to scoop Decay are those three. Their natural version is object-level or product transfer (like RSI-Opt), not a *scrubbed-procedure* improver probe. The scrub and the distance dose are the moat.
- **Reward-free and self-judged RSI** (SelfSearch, RHO) is the hot lane. A one-paragraph aside showing that the penalty also holds for a reward-free lineage would make the result look current. This is optional.
- **Timing:** get the pilot done by Dec, and post a preprint of the core result by **mid-Feb 2027**, before ICML-cycle follow-ups appear on arXiv.

## 6. Non-obvious, low-scoop add-ons (pick at most ONE as a section)

1. **Home-model penalty** (from RSI-Opt tables, per the digest). An improver procedure evolved with executor model X helps other executors but hurts X itself on held-out tasks. It is cheap as one extra executor swap at k\* and fits the meta-overfitting story: overfitting to the executor as well as the domain.
2. **Introspective drift** (from the leads-b digest). This is SelfSearch's pattern where generations 6–10 are trajectory readers that are never used downstream. Measure the share of improver edits aimed at self-inspection tooling, using call-coverage instrumentation. It is a mechanism for the penalty: procedure capacity goes into home-specific self-analysis. Gödel-native.
3. **Placebo evaluator** (N1). Run a lineage under a shuffled or always-pass evaluator. If the "home gain" survives a placebo evaluator, the Effect gate is measuring something other than learning. This is a strong control, but it is its own paper, so do not merge it here.

---

## 7. Rubric score (as merged and written)

| Dim | Weight | Score | Justification |
|---|---|---|---|
| N | 3 | 3.0 | Effect is taken (MetaRSI etc.). Cause is half-taken (SelfSearch T4, HMED, VERSE). Decay is open, but RSI-Opt's gen-1 transfer peak and three "future work" mentions put it on the menu. |
| S | 2.5 | 3.0 | Six hedged branches. The most likely branch ("memory wins") confirms existing evidence. Surprise exists only in the scrubbed-procedure penalty, which isn't the headline. |
| R | 2 | 5.0 | It is the Gödel Agent limitation verbatim. |
| C | 2 | 3.0 | Good ingredients (pairing, matched $, pre-registration), but the power calculation counts probes, not lineages. Several cost and content confounds are open. Sprawl dilutes everything. |
| F | 1.5 | 2.5 | 2–3 re-implemented families, a generator build, 40 open-model generations, 340+ probes. The open-model drivability risk is unresolved. |
| T | 1 | 4.5 | The hottest sub-trend, about 3 months ahead. |

**Total = 20 × (9 + 7.5 + 10 + 6 + 3.75 + 4.5) / 12 = 20 × 40.75 / 12 ≈ 67.9.** That is viable with fixes, not the 75.8 the panel gave.

**Projected after the fixes** (N 3.5, S 4.0, R 5, C 4.0, F 3.5, T 4.5): 20 × (10.5 + 10 + 10 + 8 + 5.25 + 4.5) / 12 ≈ **80.4** if the pilot passes the gate. Realistically about 76, allowing for scoop drift.

No kill flags are triggered:
- **K1:** no.
- **K2:** a null is interpretable only with the gate (fix 2.6).
- **K3:** the pilot must confirm that a Qwen3.5-27B Gödel-lite self-modifies. Have a GPT-mini-class fallback.
- **K4:** the risk is "we merged three audits", which the fix removes.

---

## 8. The sharper paper

**Title:** *Meta-Overfitting: Self-Referential Improvers Get Better at Improving Only Where They Grew Up*

**Single claim (pre-registered, directional):** After k accepted self-edits on domain A, a Gödel-style improver's own procedure, with memory wiped and domain content scrubbed, improves a *fresh* agent more than its generation-0 procedure on A (gate). On new domains it does so *less*, and the deficit grows with A→B distance. Restoring the gen-0 procedure while keeping gen-k memory recovers the loss. The object-level "forgetting" reset (wipe memory, keep the procedure) does not.

**Falsified if:** D_B = G_B(Pk^s,∅) − G_B(P0,∅) ≥ 0 on the far domains (lineage-level CI, n ≥ 10) while the home gain is above the SESOI. That outcome is itself publishable as "procedural meta-learning is domain-general", and it would be the first controlled evidence of it.

**Minimal design:**
- **Lineages:** Gödel-lite, Qwen3.5-27B improver, 9B executor, 10 lineages on A (ALFWorld or Reasoning-Gym family).
- **Checkpoint:** k\* = a fixed accepted-edit count.
- **Arms:** 4 (P0+∅, Pk^s+∅, P0+Mk, Pk+Mk).
- **Domains:** A plus 3 B domains at pre-registered distance; matched calls (≈ matched $ for the memory-free arms).
- **Controls:** a length-matched P0 control and a behavioural manipulation check.
- **Replicate:** a 4-lineage replicate on a second harness-optimizer family.
- **Budget:** about $1.2k. Pilot ≤ $50 by Dec 1, with a kill if the home gain is below SESOI after one pivot.

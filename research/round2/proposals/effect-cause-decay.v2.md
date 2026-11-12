# Meta-Overfitting: Self-Referential Improvers Get Better at Improving Only Where They Grew Up

*Proposal v2 (round 2), 2026-11-12. Dan, advised by Xunjian Yin. Target: NeurIPS 2027 main track (deadline about mid-May 2027).*

*This merges and replaces `round1/proposals/meta-plasticity.md`, `round1/proposals/procedure-vs-memory.md` and `round1/critiques/swap-factorial.md`. It answers both round-2 red teams (`round2/redteam/effect-cause-decay.novelty-killer.md`, 67.5, and `.sharpness-reviewer.md`, 67.9).*

**Status: GO-WITH-CHANGES.** The original three-part paper (Effect, Cause, Decay) is cut down to a single directional claim about Decay. Effect becomes a pre-registered gate. Cause becomes two contrasts that make the Decay result interpretable: a fact-scrub (κ) and a "reset procedure, keep memory" design rule.

---

## 0. What changed from v1, in response to the red team

| Red-team attack | v2 response |
|---|---|
| Effect is owned by MetaRSI §5.4, RSIFlow, FreeEvolve and Skill∞ | Effect is no longer a result section. It is now a **gate**: the home gain H must exceed the SESOI, or the paper is killed. |
| Cause is half-done in SelfSearch Table 4. "Memory wins" is expected (Lineage Is Memory). | The 8-arm transplant is cut to **5 arms**. The headline no longer depends on "memory wins". Memory enters only through the design-rule contrast P0+Mk vs Pk+∅. |
| Procedure and memory are not identifiable without a fact-scrub | The **primary arm is the scrubbed, memory-wiped procedure Pk^s+∅**. κ, the share of home gain that is domain knowledge hidden inside the procedure, is a reported estimand. A P0^s control covers damage done by the scrubber itself. |
| VERSE G.2 weakly contradicts a warm-start penalty | VERSE G.2 is treated as the **motivating anomaly**. We predict a **distance dose-response that crosses zero**: positive transfer for near domains (where SWE↔Terminal-Bench sits) and a deficit for far ones. A 4-lineage port of VERSE's optimizer, with notes split from tools, tests the explanation directly. |
| Six hedged outcome branches | **One committed directional prediction** with a stated falsifier. Each pre-registered alternative has exactly one interpretation (§1). |
| Three families, 40 generations, 6 reset arms, generator build: a lab-year | **One family** (Gödel-lite, the mentor's own lineage) with 10 lineages and one checkpoint k\*, defined by accepted improver edits. Existing seeded environments only. One small replicate family. |
| Power counted probes rather than lineages | **Lineage is the unit.** n = 10, paired. MDE about 4 pp for σ_lineage = 4 pp (§4.7). |
| Cost/context confound under matched dollars | Primary arms carry no memory, so matched calls ≈ matched tokens. A length-padded P0 control is included and procedure length is a covariate. |
| Which Pk? Winner's curse (HMED) | Pk is pre-registered as the **incumbent** improver used to produce generation k+1, not the archive best. |
| A null cannot be read with weak open improvers | Three conditions make the null readable: the home-gain gate, a behavioural manipulation check (proposal-category JS divergence), and a reported MDE. The pilot kill rule fires if the gate fails. |

---

## 1. Title, headline claim, alternative claim

**Title.** *Meta-Overfitting: Self-Referential Improvers Get Better at Improving Only Where They Grew Up*

**Headline claim (one sentence, pre-registered, directional).**
> The self-edits that make a Gödel-style improver better at improving on its home domain make it *worse* than its own generation-0 procedure at improving a fresh agent on distant domains. The deficit survives wiping its memory and scrubbing domain facts from its code, and it grows monotonically with A→B distance.

Operationally, at a pre-registered checkpoint k\* (a fixed number of accepted improver self-edits on domain A), with 10 independent lineages:
- **Gate (home):** H = G_A(Pk^s, ∅) − G_A(P0, ∅) > SESOI (3 pp).
- **Primary (far):** D_far = G_far(Pk^s, ∅) − G_far(P0, ∅) < 0, lineage-level 95% CI excluding 0.
- **Dose:** the slope of D_B on pre-registered distance d(A,B) is negative across A plus 3 B domains. The near domain may be positive, which reconciles the result with VERSE G.2.
- **Design rule (secondary):** on far B, "reset procedure, keep memory" (P0+Mk) ≥ "forget memory, keep procedure" (Pk+∅).

**Why this is surprising.** The "learning to improve" pitch behind Hyperagents' imp@50 transfer, FreeEvolve's fresh-worker gain, Skill∞'s cross-domain meta-skill and VERSE's "methodology crosses domains" assumes that edits to the improver (better verification, selection, budget allocation) are domain-general, so improving the improver is pure upside. Meta-overfitting says improver self-modification is a specialization trade-off, the meta-level version of the stability–plasticity dilemma. If true, every RSI system that carries an evolved improver into a new task should reset or regularize the procedure, not the memory. That is the reverse of current practice: Forgetting 2610.20563 wipes context and keeps the procedure.

**Falsifier and publishable alternative claim.**
> If the gate holds and D_far ≥ 0 (CI lower bound above −MDE), we report the **first controlled evidence of domain-general procedural meta-learning in LLM improvers**: evolved improver procedures, stripped of memory and domain facts, keep their advantage on distant domains. This would vindicate VERSE's "methodology transfers" reading with the controls VERSE lacked.

**Second pre-registered alternative (from κ).**
> If the unscrubbed Pk passes the gate but Pk^s does not (κ ≥ 0.7), the result is *"evolved improver procedures are memory in disguise"*. This is the meta-level version of Lineage Is Memory 2610.41157 and the first measurement of how much domain knowledge self-referential improvers write into their own code.

**Kill.** If even the unscrubbed Pk fails the home gate after one pilot pivot, there is nothing to decay and we stop (§6).

---

## 2. Why now, why novel

### 2.1 Field trajectory (Nov 2026) and where it is heading

1. **Same-start swaps are now standard in method papers** (MetaRSI, RSIFlow, FreeEvolve, HMED, SelfSearch). A powered in-domain swap paper is likely by the ICML 2027 deadline. **Do not anchor on Effect.**
2. **Cross-task carryover of evolved improvers is named future work** by Organon 2610.33046, Agent Plasticity 2610.08902 and SelfSearch 2609.37968, and is called for by the Experience-Becomes-Structure survey and 2610.27610. These groups will most naturally run *product* transfer (as RSI-Opt did) or *unscrubbed* optimizer transfer (as VERSE G.2 did), within roughly 3–6 months. Our moat is three things such follow-ups are unlikely to combine: the **scrubbed, memory-wiped procedure**, the **distance dose crossing zero**, and the **lineage-level power**.
3. **Reward-free and self-judged RSI** (SelfSearch, RHO) and **verifier co-evolution** are the hot method lanes. One appendix lineage set under SelfSearch-style reward-free selection would make the result look current (optional, §4.9).
4. **Rigor bar:** pre-registration with n ≈ 10 per cell (Lineage Is Memory) is now what reviewers expect from audit-style RSI papers. We meet it.

**Non-obvious, low-scoop pieces kept in v2** (no paper's future-work list names them):
- (a) κ, the knowledge-in-procedure estimand.
- (b) The zero-crossing distance law that reconciles VERSE with the penalty.
- (c) **Introspective drift** as the mechanism readout. SelfSearch generations 6–10 build self-inspection tooling that is never used downstream. We test whether the share of edits spent on self-inspection predicts D_far across lineages.

**Deferred** to keep scope (each is a possible follow-up paper): the home-model penalty (RSI-Opt), the placebo evaluator, improver monoculture.

### 2.2 Closest work and the gap each leaves

| # | Paper | What it shows | Gap we fill |
|---|---|---|---|
| 1 | **VERSE** 2610.02616 (MIT/Amazon, Oct 2026), App. G.2 Table 9 | Evolved optimizer harness moves SWE-rebench ↔ Terminal-Bench. On SWE→TB the inherited optimizer (40.07, round 1) beats frozen-optimizer entries. Authors: "what crosses domains appears to be the optimizer's methodology, its skills, notes, and tools". | Single runs. Only 2 rounds on B. **Notes (memory) are lumped with tools/prompts (procedure).** No scrub. One near-ish domain pair, so no distance axis. We replicate it with these separated (§4.9) and place it on our distance axis. |
| 2 | **SelfSearch** 2609.37968 (SNU, Sep 2026), Table 4 | Ablates "w/o episode records" vs "fixed improver" at gen 10 (GPT 76.2/74.2/74.6; DeepSeek 86.7/83.8/83.8). Asks whether agents get "better at improving themselves". | One search per cell, 2 lineages, n = 120 tasks, 1.7–2.9 pp gaps inside noise, in-domain only. No cross-domain, scrub or distance. |
| 3 | **MetaRSI** 2609.06396 (Sep 2026), §5.4 | Same-start, same-budget fork: +7.2 vs +4.9. Five-term curve +26.2 vs +20.7, 5 seeds, open model. | In-domain only. One bespoke system, no off-diagonal state grid, no transfer. **We cite it as evidence that the gate is attainable.** |
| 4 | **RSI-Opt** 2610.32026 (Oct 2026) | Held-out transfer of an evolved improver *policy* peaks at generation 1, then specializes. Includes a cross-model transplant. | The meta-agent's own procedure is fixed. Product-level transfer, 1 run / 3 seeds, no matched gen-0 control. It is the closest precedent for the *shape* of our claim, at the product level. |
| 5 | **Lineage Is Memory** 2610.41157 (Oct 2026) | 1,750 pre-registered runs. Inheritance dominates (Δ = +0.238 [0.224, 0.252]). A "carry facts" instruction moves banking77 from 0.372 to 0.856. | Object-level with a fixed proposer. **We take its mechanism (facts migrate into artifacts) up to the improver level via κ** and adopt its rigor standard. |
| 6 | **HMED** 2610.07979 (Oct 2026) | Restores the same discovery state x to compare meta-skills. Branch credit and matched Δ disagree in sign in 45.5% / 75% of revisions. Table 4 freezes the meta-skill and resets the library. | Their reuse test equals our Pk+∅ cell, in-domain, with no P0+Mk, no scrub and no distance. Their winner's-curse evidence is why we pre-register the incumbent Pk. |
| 7 | **FreeEvolve** 2610.09197 (AWS, Oct 2026) | Fresh-worker gain +6.9. A Haiku 4.5 evolver overfits: evolution set 35.0→52.2 while held-out goes 33.3→32.2. | A single evolver-model ablation, in-domain. Its Haiku result is meta-overfitting to the *task set* at one point. We test overfitting to the *domain*, with a dose. |
| 8 | **Agent Plasticity** 2610.08902 (Meta, Oct 2026) | Defines plasticity as held-out gain per learning cost for object-level self-improvement. | Lists improver-strategy transfer as future work. **Meta-level plasticity is unmeasured.** |
| 9 | **Auditing the Product, Not the Recursion** 2610.27610 (Oct 2026) | Census: 4 of 26 improver claims are same-start and matched-budget. Proposes a swap test. C5 compounding is unmeasured. | No experiments, and the proposal is in-domain. We adopt its reporting standard. |
| 10 | **MetaExp** 2610.21492 (Qwen, Oct 2026) | Experience-learning trained into weights transfers across domains. Untrained backbones degrade as experience accumulates (−2.2, −1.7). | Weight- and actor-level, with no self-referential improver procedure. Motivates our question at the harness/meta level. |
| 11 | **Organon** 2610.33046 (Oct 2026); **Experience-Becomes-Structure** survey (alphaXiv slug `2611.recursive-self-improvement-llm-agents`) | Name cross-task carryover, and inherited state vs improvement procedure at matched budget, as open questions. | No experiments. They show the question is public, which sets our timing (preprint by mid-Feb). |
| 12 | **Faithful Self-Evolvers** 2601.22436 | Condensed experience is causally inert while raw experience is used. | Object-level only. Its method template gives our optional shuffled-history arm. |
| 13 | Abstract-only, seen this pass (**not read in full; treat the gaps as unverified**): *Does Failure Feedback Direct Agent Self-Modification?* 2610.14662; *Conservative Meta-Agent Search under Non-Stationarity* 2610.31436; *Beneath the Surface* 2611.07624 | From the abstracts: directedness of self-edits; meta-agent search under shift; whether harness gains are transferable. | Must be read before the pre-registration is frozen (week of Nov 16). None appears, from its abstract, to run a scrubbed improver-procedure probe on graded-distance domains. |

**Bottom line.** Every ingredient has a one-cell precedent. The *claim* does not: that the improver's own scrubbed procedure gets worse at improving with distance from home, crossing zero. Nor does a powered, scrubbed, distance-dosed test of it. K1 is not triggered. K4 is avoided because the contribution is a directional law and a design rule, not "we combined three ablations".

---

## 3. Research questions

- **RQ1 (primary, Decay).** After k\* accepted self-edits on A, does the memory-wiped, scrubbed procedure Pk^s improve a fresh agent on a distant domain *less* than P0 does, at matched improver calls?
- **RQ2 (dose).** Is D_B monotone decreasing in a pre-registered A→B distance, and does it cross zero between near and far?
- **RQ3 (κ: what is in the procedure).** What fraction of the home gain of Pk is removed by scrubbing domain facts from its code and prompts?
- **RQ4 (design rule).** On far B, does resetting the procedure while keeping memory (P0+Mk) beat the forgetting reset (Pk+∅)?
- **RQ5 (mechanism, exploratory).** Across lineages, does the share of improver edits spent on self-inspection tooling (introspective drift) predict D_far?

---

## 4. Design

### 4.1 Improver family (primary): Gödel-lite

A minimal Gödel-Agent-style self-referential loop, built from Xunjian's Gödel Agent code and stripped down:
- **Improver.** Python plus prompt modules: proposer, verifier/selector, budget allocator and history reader. It can edit its own modules.
- **Executor agent.** Its harness (prompt, tools, memory hooks) is what the improver improves.
- **Memory M.** The improver's archive: scored edit history, notes and trajectory summaries.
- **Accepted edit.** An improver self-edit is accepted if the self-modified improver's next-generation executor beats the incumbent on a fixed dev split (30 tasks) by more than one paired SE.
- **Generation clock.** Generations are counted in **accepted improver edits** (the Feedback Dynamics 2608.02636 lesson), not loop iterations.
- **k\*.** Pre-registered at 12 accepted improver edits (to be adjusted once from pilot rates, then frozen in the pre-registration). This is roughly 25–40 loop iterations.

### 4.2 Models

| Role | Model | Notes |
|---|---|---|
| Improver | One open ~30B-class model, e.g. Qwen3.5-35B-A3B (MetaRSI ran self-modifying improvers on it) served locally | **Fallback:** DeepSeek V4 Flash API, the cheap model used by Forgetting 2610.20563 ($20–40/run) and HMED. |
| Executor | An 8–9B open model, served locally with vLLM | |
| Scrubber and leak judge | A *different* model family from the improver, so the scrubber does not share the improver's priors | |

The fact-check showed open models can drive Gödel/DGM-style loops (VERSE, MetaRSI, HMED, SelfImprove-Bench's HyperDSH). The pilot verifies that our loop does, before any money is committed.

### 4.3 Domains (existing seeded generators only; no generator build)

| Role | Domain | Notes |
|---|---|---|
| Home A | ALFWorld | Seeded, used by HMED and MetaSkill-Evolve, so numbers are comparable |
| B_near | ScienceWorld | Text-embodied, different task semantics |
| B_mid | SelfImprove-Bench text games (2611.02797) | Procedural, held-out ladder |
| B_far | Reasoning Gym task families | Non-embodied symbolic puzzles; still multi-step agentic with tool calls |

**Distance d(A,B) is pre-registered before any probe runs.** It is the *zero-shot transfer gap* of the gen-k\* executor harness evolved on A: 1 − (its gain on B over the B seed harness) / (its gain on A). The ordering is checked against a secondary embedding distance between task-description corpora. If the two orderings disagree, we report both and use the transfer gap.

### 4.4 Arms (probes at k\*, applied to a *fresh* seed executor S0^D on each domain D)

| Arm | Procedure | Memory | Purpose |
|---|---|---|---|
| **P0+∅** | gen-0 | none | baseline |
| **Pk^s+∅** | gen-k\*, scrubbed | none | **primary** |
| Pk+∅ | gen-k\*, unscrubbed | none | κ; the "forgetting" reset |
| P0+Mk | gen-0 | gen-k\* archive | "reset procedure, keep memory" |
| Pk+Mk | gen-k\* | gen-k\* archive | what deployed systems do (VERSE-style warm start) |

Controls:
- **P0^s+∅ (scrubber control, on A and B_far).** The same scrubber applied to P0. It should be a no-op; any loss estimates the damage scrubbing does in itself.
- **P0^pad+∅ (length control, on A and B_far).** P0 padded with neutral generic text to the token length of Pk.
- **Optional, if budget allows (on A only): Pk+shuffle(Mk).** Faithful-Self-Evolvers-style shuffled history.

Each probe runs **R = 5 improvement rounds** at a fixed **improver-call budget** (same number of improver LLM calls and executor dev evaluations per round). Tokens and dollars are logged and reported. Memory-free arms have roughly equal context, so matched calls ≈ matched tokens. For the memory arms, the token excess is reported, and a matched-tokens re-analysis truncates Pk+Mk and P0+Mk runs at P0+∅'s token count.

### 4.5 Scrubber and leak audit (defines Pk^s)

The scrubber is an LLM rewrite of every module and prompt of Pk. It removes domain entities (objects, receptacles, action verbs specific to A), task-specific rules and examples, and keeps control flow, function signatures and generic instructions. A diff check ensures no code paths are deleted.

The leak audit has two parts:
1. A regex count of A-vocabulary tokens, taken from the ALFWorld lexicon.
2. A blind LLM judge asked to identify the source domain from Pk^s among 6 options. The target is chance-level accuracy across 10 lineages.

Scrub strength comes in three levels (light/medium/heavy). The κ curve on A is reported across them. The primary analysis uses **medium**, pre-registered.

### 4.6 Metrics (formal)

- **Improvement gain.** For domain D, improver configuration (P, M), and task-stream seed s: G_D(P,M; s) = Acc_D^held-out(executor after R rounds) − Acc_D^held-out(S0^D). Held-out is 100 tasks per domain, disjoint from the dev split used inside the loop.
- **Lineage-level contrasts** for lineage i, averaging 2 paired task-stream seeds (P0 arms use the same seeds as lineage i, so contrasts are paired):
  - H_i = G_A(Pk_i^s,∅) − G_A(P0,∅) (gate)
  - D_{B,i} = G_B(Pk_i^s,∅) − G_B(P0,∅) (primary for B = far)
  - κ_i = 1 − [G_A(Pk_i^s,∅) − G_A(P0,∅)] / [G_A(Pk_i,∅) − G_A(P0,∅)], computed on the lineage mean (ratio of means) to avoid division by noise
  - R_{B,i} = G_B(P0,Mk_i) − G_B(Pk_i,∅) (design rule)
- **Manipulation check.** On a fixed probe state, each improver proposes 50 edits. An LLM coder (validated on 100 human labels) assigns each to a category: verification / selection / budget / prompt-content / tool / self-inspection. The JS divergence between the Pk^s and P0 category distributions must have a 95% bootstrap CI excluding 0.
- **Introspective drift.** Of the accepted improver edits up to k\*, the fraction that add or modify self-inspection tooling (trajectory readers, history filters, self-analysis prompts). The call-coverage share is the fraction of those tools invoked during probes on B.

### 4.7 Statistics and power

**Unit:** lineage, n = 10. All lineages start from the same gen-0, with independent seeds for the improver and the dev split.

**Tests:**
- Gate: one-sided lineage-level t-test, H > 3 pp.
- Primary: two-sided paired t-test on D_far, plus a sign test as a robustness check.
- Dose: linear mixed model D_{B,i} ~ d(A,B) + (1 | lineage) over the four domains, which include A. We test slope < 0 and report the zero-crossing distance with a bootstrap CI.
- Holm correction across the primary, dose and design-rule tests. κ and drift are reported as estimates with CIs, not tests.

**Power:** we assume σ_lineage ≈ 4 pp. SelfSearch's between-lineage spread of 1–3 pp and Lineage Is Memory's per-cell spread suggest 3–5 pp; the pilot will estimate it. With n = 10, α = 0.05 and power 0.8, the MDE is (2.26 + 0.88) × 4/√10 ≈ **4.0 pp**. If the pilot finds σ > 5 pp, lineages go to 14 (MDE ≈ 4.2 pp at σ = 5); the budget has room. Power for the dose slope is computed by simulation from pilot variance components before the pre-registration is frozen.

**Pre-registration:** OSF, frozen before the first k\* probe. It covers k\*, the distance ordering, scrub level, SESOI, held-out sets, the analysis plan and the kill rules.

### 4.8 Seeds and replication summary

- 10 Gödel-lite lineages on A.
- At k\*: 5 arms × 4 domains × 2 task-stream seeds, plus the 2 controls on 2 domains.
- About 10 × (5 × 4 + 4) × 2 ≈ **480 probes**. P0 arms are shared but evaluated on each lineage's seeds.

### 4.9 Replicate family (answers "is this one system?" and VERSE G.2 directly)

VERSE's released optimizer (five slots: prompts, skills, tools, hooks, notes) is ported to the same executor and domains A and B_far, with **4 lineages at k\* only**. Arms on B_far:
1. Frozen optimizer
2. Evolved full (VERSE warm)
3. Evolved **notes only**
4. Evolved prompts+skills+tools+hooks, scrubbed, no notes

**Prediction:** VERSE's transfer is carried by notes and generic tools at near distance, and its scrubbed procedure shows the same far-domain deficit.

**Drop rule:** if porting takes more than 7 working days in January, we replace it with a 4-lineage Gödel-lite run under a second improver model (DeepSeek V4 Flash). That still answers the one-model objection.

**Optional (only if more than $300 remains in March):** 4 lineages under SelfSearch-style reward-free selection, testing A and B_far only.

### 4.10 Budget (API dollars; executor and optionally improver served locally on lab GPUs)

| Item | Estimate |
|---|---|
| Pilot (§6) | ≤ $50 |
| 10 Gödel-lite lineages to k\* (anchored on Forgetting's $20–40/run with a Flash-class model; improver API fallback included) | $300–400 |
| ~480 probes × ~$1.5–2 (5 rounds + 100 held-out episodes, executor local) | $700–950 |
| Scrubber, leak judge, edit coder | $60 |
| VERSE-port replicate | $250 |
| **Total** | **≈ $1.4–1.7k** (≤ $2.2k with 30% buffer; under the $3k cap) |

If the improver is served locally, the API cost roughly halves. GPU need: one 4–8×A100/H100-class node, part-time, for vLLM serving. This must be confirmed with Xunjian (open question).

---

## 5. Figure 1 and key plots

**Figure 1 (the paper in one picture): "Meta-overfitting crosses zero."**
- x-axis: pre-registered distance d(A,B), at A (0), ScienceWorld, SI-Bench games, Reasoning Gym.
- y-axis: G_D(Pk^s,∅) − G_D(P0,∅) in pp.
- Ten thin lines, one per lineage, with a thick mean line and a 95% CI band, a horizontal zero line, and the zero-crossing distance with its CI.
- Inset: VERSE G.2's SWE→TB point, placed by the same transfer-gap metric (marked approximate; different executor).
- **Predicted:** positive at A (the gate), near zero or positive at near, negative at far.

Other figures:
- **Fig 2, κ curve.** Home gain of Pk vs scrub strength (none/light/medium/heavy), with the P0^s control flat at 0 and the leak-judge accuracy on a secondary axis.
- **Fig 3, design rule on B_far.** Bars for the 5 arms. The key comparison is P0+Mk vs Pk+∅, with a matched-tokens re-analysis overlaid.
- **Fig 4, manipulation check and mechanism.** Proposal-category distributions for P0 vs Pk^s (stacked bars). Scatter of introspective-drift share vs D_far across 10 lineages (exploratory, with Spearman ρ and CI).
- **Fig 5, replicate.** VERSE-port arms on B_far (frozen / full / notes-only / scrubbed procedure).

---

## 6. One-week pilot (Nov 16–23, ≤ $50)

**Setup:**
- 2 Gödel-lite lineages on an ALFWorld subset (60 dev / 50 held-out tasks), each run to 6 accepted improver edits.
- Then probes P0+∅, Pk+∅ and Pk^s+∅ on A and on Reasoning Gym (B_far), R = 3, 1 seed.
- Also: read the three abstract-only papers in row 13 of §2.2 in full.

**Measurements:**
1. Accepted improver-edit rate (edits that touch the improver itself, not just the executor).
2. Home gain of Pk and Pk^s.
3. Between-lineage and between-seed spread.
4. Scrubber leak-judge accuracy.
5. Dollars per probe.

**Thresholds:**

| Outcome | Condition | Action |
|---|---|---|
| **GO** | ≥ 1 accepted *improver* edit per 4 iterations; home gain of Pk ≥ 3 pp in both lineages; leak judge ≤ 40% (6-way); probe cost ≤ $2.5 | Freeze k\* and the pre-registration by Dec 20 |
| **PIVOT (once)** | Improver-edit rate too low, or home gain in [0, 3) pp | Switch the improver to DeepSeek V4 Flash (stronger), or force improver-module edits (as SelfImprove-Bench did, raising meta edits to 20–45%); or move A to a Reasoning Gym family with more headroom. Re-pilot within one week. |
| **KILL** | After the pivot, home gain of unscrubbed Pk < SESOI | No decay to measure. Fall back to round-1 #2 (evolve-in-dream) or #3 (learning-to-goodhart), which share the Gödel-lite infrastructure. |
| **Early signal (not a gate)** | Sign of D_far in the 2 pilot lineages | Recorded but not used to change hypotheses. The directional prediction is fixed now, in this document. |

---

## 7. Timeline (Nov 2026 – May 2027)

| Dates | Milestone |
|---|---|
| Nov 13–15 | Strip Gödel Agent code into Gödel-lite; ALFWorld + Reasoning Gym wrappers |
| Nov 16–23 | **Pilot** (§6); read the 3 abstract-only papers; check OpenReview ICLR 2027 by hand (still unchecked) |
| Nov 24 – Dec 20 | ScienceWorld + SI-Bench wrappers; scrubber, leak judge, edit coder (100 human labels); distance metric; **OSF pre-registration frozen Dec 20** |
| Dec 21 – Jan 20 | Run 10 lineages to k\* (in parallel); begin VERSE port (7-day drop rule) |
| Jan 20 – Feb 10 | Probes on A and B_far (primary + gate + κ + design rule) |
| **Feb 15** | **arXiv preprint v1** with the primary result (A + B_far), ahead of ICML-cycle follow-ups |
| Feb 15 – Mar 31 | Near and mid domains (dose), controls, VERSE replicate, optional reward-free lineages |
| Apr 1 – May 1 | Analysis, figures, writing; NeurIPS Datasets/Code release of Gödel-lite + scrubber + probe protocol |
| May 1–15 | Internal review with Xunjian; submit |

**Ongoing:** weekly check of new citers of SelfSearch, VERSE, MetaRSI, RSI-Opt, 2610.27610 and Agent Plasticity. Check the NeurIPS 2026 workshop accepted lists after Dec 4.

---

## 8. Reviewer objections and rebuttals

1. **"This is SelfSearch Table 4 + MetaRSI §5.4 + VERSE G.2 with more seeds."** None of them tests the *scrubbed, memory-wiped* procedure on a new domain, or a distance dose. Our headline is the reverse of what VERSE concludes, and we reconcile the two with the zero-crossing law. The in-domain swap is only a gate, and we credit MetaRSI for it.
2. **"A penalty on a new domain is trivial: the improver specialized, or A-notes distract it."** That is why the primary arm has no memory (no distraction) and is scrubbed (no domain content), with a leak audit and a P0^s control. If the scrubbed procedure still underperforms P0 far away, what transfers negatively is *how it improves*, not *what it knows*.
3. **"Your scrubber destroys useful generic structure, so Pk^s is handicapped."** Three checks address this: (i) P0^s control; (ii) Pk^s must still pass the home gate, so it is not broken; (iii) a diff check confirms no control-flow paths are deleted; (iv) the κ curve over scrub strengths.
4. **"Matched dollars penalize long contexts."** The primary arms carry no memory, a padded-P0 control is included, and procedure length is a covariate. The memory arms get a matched-token re-analysis.
5. **"One family; maybe Gödel-lite is idiosyncratic."** A VERSE-port replicate (or a second-model Gödel-lite), with its drop rule declared in advance. The claim is stated for Gödel-style improvers; generality beyond that is presented as evidence, not proven.
6. **"Open models are too weak; this is about weak improvers."** The pilot gate requires a real home gain. FreeEvolve shows weak evolvers overfit *in-domain*; we gate that out and test overfitting *across* domains. A DeepSeek V4 Flash fallback is included.
7. **"n = 10 is small."** Lineage is the right unit, which earlier papers got wrong. The MDE (4 pp) is stated in advance and the test is pre-registered. Effect sizes in the literature (MetaRSI +2.3 pp per term, VERSE about 2–7 pp) are on this scale; anything smaller is not practically relevant.
8. **"The distance metric is circular: it uses the evolved artifact's transfer."** It uses the *executor harness's* zero-shot transfer, not the improver's. It is computed and frozen before probes, and checked against an independent embedding distance.
9. **"Why not 40 generations and a curve?"** A curve from correlated checkpoints is a figure, not a test. If budget remains, k\*/2 is a secondary checkpoint.
10. **"Measurement paper."** The contribution is a directional law (a meta-level stability–plasticity trade-off with a zero crossing) and a tested design rule (reset the procedure, keep memory) that runs against current practice.

---

## 9. Fit with Xunjian Yin's agenda

- **Gödel Agent:** this tests whether Gödel Agent's self-referential edits are *meta-learning* or *meta-specialization*, the open question its limitations section raises about long-horizon self-improvement. Gödel-lite is built from his code, so engineering risk is lowest here.
- **Long-horizon self-improvement:** the design rule (reset procedure, keep memory) applies directly to any agent that improves itself across task shifts.
- **World models:** the distance dose and the self-inspection-drift readout suggest that improvers build an implicit model of *their home environment* into their procedure. A natural follow-up uses round-1's evolve-in-dream to test whether improver edits made against a world model of A transfer worse than those made against real A.
- **WeAct / BreakingWeb:** BreakingWeb-style environment perturbations are a cheap way to build extra B domains at controlled distance from a web home domain, an obvious extension if a web A is wanted for the camera-ready.

---

## 10. Rubric rescore (honest)

| Dim | Wt | v1 (red team) | v2 | Why |
|---|---|---|---|---|
| N | 3 | 2.5–3.0 | **3.5** | The scrubbed-procedure decay claim and the zero-crossing distance law are unclaimed. VERSE G.2, RSI-Opt and three future-work flags keep it from 4+. Scoop probability for this exact design before May is about 25–30%. |
| S | 2.5 | 3.0 | **4.0** | One directional claim that runs against the field's "improver edits are domain-general" assumption, with a clean falsifier. Both alternatives (domain-general procedural meta-learning; procedures-are-memory) are notable. |
| R | 2 | 5.0 | **5.0** | Gödel Agent's own question, on his code. |
| C | 2 | 3.0–3.5 | **3.5** | Pre-registered, lineage-powered, scrub and length controls, matched calls, a dose design. But one primary family, and the replicate is small. |
| F | 1.5 | 2.5–3.0 | **3.5** | One family, existing environments, about $1.5k, pilot under $50. Risks: open-model drivability (gated) and GPU serving time. |
| T | 1 | 4.0–4.5 | **4.0** | Hot, 3–6 months ahead of the predicted follow-ups. The audit sub-lane is starting to saturate. |

**Total = 20 × (3·3.5 + 2.5·4.0 + 2·5 + 2·3.5 + 1.5·3.5 + 1·4.0) / 12 = 20 × 46.75 / 12 ≈ 77.9.**

This is "strong" only if the pilot passes the gate. If the pivot is needed, F drops to 3.0 (total ≈ 76.9). If a powered cross-domain improver probe appears before Feb 15, N drops to 2.5 (total ≈ 72.9), and the paper falls back to κ plus the zero crossing as its distinct contribution.

**Kill flags:**
- K1: no.
- K2: avoided by the gate, the manipulation check and the MDE.
- K3: no (open models; Flash-class fallback).
- K4: avoided (a law plus a design rule, not a combination).

---

### Sources

Verified in round-2 digests or red-team deep reads: 2610.02616, 2609.37968, 2609.06396, 2610.32026, 2610.41157, 2610.07979, 2610.09197, 2610.08902, 2610.27610, 2610.21492, 2610.33046, 2601.22436, 2611.02797, 2610.20563, 2608.02636, and alphaXiv slug 2611.recursive-self-improvement-llm-agents.

Seen as abstracts only in this pass's alphaXiv recency search (unverified detail): 2610.14662, 2610.31436, 2611.07624, 2611.05139.

Unchecked: OpenReview-only ICLR 2027 submissions; NeurIPS 2026 workshop accepted lists.

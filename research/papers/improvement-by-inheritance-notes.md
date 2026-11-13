# Notes: "Improvement by Inheritance: Measuring Recursive Self-Improvement in Coding Agents"
ICLR 2027 submission #50566 (anonymous), https://openreview.net/forum?id=ZMmJDeDTXk. Read in full (21 pp.) on 2026-11-13.

## What it is
A measurement and identification paper about recursive self-improvement (RSI), with theory, an audit of released run histories and new controlled experiments. It is much broader than a Hyperagents re-analysis.

## Theory
- **Child quality vs successor quality.**
  - Q^(0) is child quality: how good the task agents an improver writes are.
  - Q^(1) is successor quality: how good the improvers it writes are.
  - The definition is recursive to Q^(r).
- **Thm 1 / Cor 1 (inheritance dominance).** A self-lineage contrast equals inheritance Δ0 plus an edit term bounded by the self-edit rates p11 + p00. When self-edits are rare, lineage gains are inheritance.
- **Thm 2.** Nested replay identifies successor quality. The design crosses modifiers on fixed meta-states and scores every successor on a common bank. The unit of inference is the meta-state.
- **Thm 3.** Lower orders do not identify higher orders: Δ0 says nothing about the sign of Δ1, and an archive trajectory identifies neither.
- **Prop 1.** Order reversal is possible (Δ0 > 0 with Δ1 < 0).
- **Prop 7.** Huxley-Gödel clade metaproductivity ≠ successor quality.

## Audit of released histories
- **Scope.** All 71 Hyperagents runs across 15 configurations, with 1,550 improver transcripts, plus the DGM and ADAS histories.
- **Archive curves.** The best-so-far curve sits inside a permutation null band at 119 of 141 checkpoints. A stationary-operator null covers 64 of 69.
- **Hyperagents paper review.** The task suffers label collapse: 163 of 369 evaluated nodes predict one label. Mean child-minus-parent change is ≤ 0 in every family.
- **DGM.** 45–62% of SWE-bench children were never scored. Improvements and regressions are about equal.
- **ADAS.** The best-by-search agent ranks 1st–10th on test.
- **Metaⁿ.** The selected gain of 0.309 comes from replacing unparseable programs. Among executable starts it is 0.014.
- **Improver-edit rates.** Self-improving arms edited the improver in 1–15% of generations. Only one such edit has descendants: the generation-1 "briefing". Self-improving improvers read the eval report before their first edit in 87–93% of generations; fixed improvers did so in 0 of 800.

## New experiments (main backbone MiMo-V2.5, plus 5 other models)
- **Order 0.** M1, the selected gen-1 edit that tells the improver to read eval results, raises child quality from 0.476 to 0.858 on 12 states. It also helps on SWE-bench Verified (0.50 → 0.80, 10 instances) and on 14-step state-tracking (+0.26).
- **Prospective loops.** 5 seeds × 80 generations on 8 new problem families. 6 versions were selected; only 1 (again evidence routing) raised child quality. Every archive hit a perfect dev score while still mostly running M0.
- **Order 1 (nested replay).** 6 meta-states × 6 replicates.
  - δ0 = +0.031 [−0.044, 0.126]; δ1 = +0.003. The self-lineage contrast is +0.293, almost all inheritance.
  - M1 never edits a copy of itself (0 of 36 runs).
  - Positive control: a modifier explicitly told to diagnose the improvement procedure edits the improver in 35 of 42 runs and gets +0.171 [0.099, 0.240] successor quality.
  - Replication: an independent evidence-routing edit edits improvers in 0 of 72 runs, with no successor gain.
- **Mechanism: evidence routing.** Gain is large only when the eval report adds information the spec lacks and the model wouldn't read it unprompted.
  - Information-dose experiment: +0.372, +0.046, −0.067.
  - 6-model panel: forced reading helps MiMo, GLM, Kimi and Grok; it adds nothing for DeepSeek and Gemini, which read the report anyway.
  - SWE-bench 2×2 with and without the issue text: interaction +0.24.
- **Stated limitations.**
  - The meta-state bank is small, and the order-1 experiments use one backbone, so only large changes are resolvable.
  - Order ≥ 2 is unmeasured.
  - Coding loops only.
  - Short horizon.

## What this takes from us, and what it leaves open
**Taken.** We should cite it and build on it rather than re-derive any of these:
- the improver-swap / "do improvers get better at improving" assay, in-domain
- the finding that improvers rarely self-edit
- the archive-curve critique
- the first behavioural mechanism (evidence routing)
- a first "force a meta-edit and it helps" result: the positive control, +0.171

**Open:**
- **Why** self-edits are rare. They document the rarity but do not test causes: the gate's credit assignment, effects below gate resolution, model reluctance, framing.
- Forcing mechanisms compared **at matched compute** and over long horizons. Their positive control is a single prompt on 7 states, with no cost accounting and no comparison against meta-fitness, quotas or two-timescale designs.
- **Cross-domain transfer** of improver edits (Meta-Overfitting): not tested.
- The **openness law** (editable surface vs gain): not tested.
- A reality-grounded continual benchmark: not tested.
- Order ≥ 2.
- Gödel Agent (cited, not analysed) and open-model improver families beyond MiMo.

**Method implications for us:**
- Any "improver got better" claim must be an order-1 nested-replay contrast at the meta-state level.
- Archive curves are not evidence.
- Their 1–15% edit rates and ~6 meta-states × 6 replicates give realistic power numbers for our pilots.

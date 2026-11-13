# One-pagers: top NeurIPS 2027 ideas

*Dan, for Xunjian · 2026-11-13 · Each idea in the same short format. Details and sources are in `ACCEPTANCE_RANKING.md`, `FINAL_REPORT.md`, `round3/REPORT.md` and `round3/IBI_GAPS_AND_METAREWARD.md`.*

## Shared vocabulary

- **Task agent:** the agent that solves tasks.
- **Improver (meta-agent):** an LLM loop that rewrites the task agent's prompt and code. Sometimes it can also rewrite itself.
- **Reflector:** the part of the improver that reads transcripts and proposes an edit.
- **Gate:** the rule that keeps or rejects an edit, usually "keep it if the score went up".
- **Order 0:** does an improver make better *task agents*?
- **Order 1:** does an improver make better *improvers*? This is what "getting better at getting better" means.
- **Nested replay:** the method from *Improvement by Inheritance* (ICLR 2027 submission ZMmJDeDTXk) for measuring order 1 cleanly. Fix the starting states, swap only the improver, and score whatever it produces on a common test bank.
- **Background finding:** improvers rarely edit themselves, and their gains are mostly one good early edit that later generations inherit (Improvement by Inheritance). Few papers measure self-edit rates at all: SelfImprove-Bench reports 0% retained improver changes unless the framing invites them, and then 20–45%.
- **Verified Nov 13 (full text, `FULLTEXT_VERIFICATION.md`):** almost no published system rejects self-edits with a score. DGM, Hyperagents and HGM keep every valid child. STOP, RSIFlow, VERSE and MetaSkill-Evolve force self-edits. MetaRSI only runs syntax checks. Gödel Agent leaves it to the LLM. Only HMED scores an improver edit separately and can reject it.

## At a glance

| # | Idea | One-line claim | Est. P(accept)* | Pilot |
|---|---|---|---|---|
| 1 | **Taught, Not Selected** | Bad environments corrupt self-improving agents mostly through *what the reflector reads*, not through what the score selects | 12.9% | ≤ $60 |
| 2 | **Forced-Quota Dose** | Forcing improvers to rewrite themselves helps up to a point, then hurts | 11.2% | ~$50 |
| 3 | **Gate, Not Willingness** | Improvers *propose* self-edits; the gate throws them away | 10.5% | ~$45 |
| 4 | **Order-2 Audit** | Claims of "deeper" recursive improvement do not survive controlled replication | 10.3% | small |
| 5 | **Order-0 Rewards Breed Routers** | Grading improvers on task score yields routing tricks, not better improvers | 10.2% | ~$80 |
| 6 | **Meta-Overfitting** | Self-edited improvers get better at home and worse on far domains | 9.9% | ≤ $50 |
| 7 | **Varying Goals** | Self-editing pays off only when the world keeps changing | 9.0% | ~$45 |
| 8 | **Openness Ladder** | The best amount of editable surface is in the middle, and it moves | 8.5% | ~$40 |
| 9 | **MetaShift** | Self-editing helps when the *rules of improving* change, not when the tasks change | 8.3% | ~$55 |

\*P(accept) = P(finish) × novelty factor × P(accept | done), the median of 3 calibrated judges. It is a judgment estimate, so compare ranks, not decimals. Gaps under ~1 point are ties.

---

## 1. Taught, Not Selected
**How simulator and benchmark errors get into self-improving agents**

**RQs**
1. When an agent is improved in a slightly wrong environment (a simulator, an LLM judge, a subtly broken benchmark), how does the error get in? Is a bad candidate *selected* by an inflated score, or is the reflector *taught* a false rule by reading misleading transcripts?
2. Does a bug that is visible in the transcript get exploited faster than the same bug when it is silent?
3. Does the answer depend on how much the optimizer reads (GEPA > DGM-style > ADAS-style)?

**Core idea.** Reflective optimizers have two input channels: the **traces** they read and the **scores** that select. An aligned reflector cannot tell that the environment is wrong. It honestly writes down what it sees ("ID checks aren't needed for refunds"). We predict most damage comes through *reading*. If so, the field's fixes (better scoring, anchors, ensembles) aim at the wrong channel, and a cheap fix exists: **trace hygiene**, i.e. controlling what the reflector sees.

**Experiments**
- **2×2.** Trace source {sim, real} × score source {sim, real}. Each candidate runs in both environments on the same tasks. The reflector reads one source and the gate uses the other. Everything is tested on held-out real tasks.
  - **SR** has clean reading and dirty scores, so any loss in SR is *selection* damage.
  - **RS** has dirty reading and clean scores, so any loss in RS is *teaching* damage.
- **Legibility.** Each bug comes in a visible version and a silent version at the same error rate.
- **Rule out "sim traces are just less useful".** Use four checks:
  - the exploit-specific behavior rate;
  - bug-shaped rules found inside the evolved harness;
  - damage localized to tasks where the bug applies;
  - surgery: delete the rule and see whether performance recovers.
- **Benchmarks.** Web tasks with BreakingWeb-style interventions (Xunjian's tool, which has frontier headroom), plus SWE-bench with weak tests, which are a natural, real bug. τ²-retail is used only for debugging.
- **Generality.** Run the full grid on one optimizer and the key SR-vs-RS contrast on 3 optimizers. Use the reflector's strength (weak vs frontier) as a factor.

**What each result means**
- RS ≪ SR: **taught.** Protect the reflector's inputs.
- SR ≪ RS: **selected.** Spend real-environment budget on scoring.
- Neither shows loss but a lenient-judge control does: harness search exploits lies about *success*, not lies about *dynamics*.

**Contributions.** A channel decomposition of proxy exploitation for reflective optimizers; a legibility test; a "teaching share grows with how much the optimizer reads" result; and a trace-hygiene recommendation.

**Central claim.** Self-improving agents do not just get fooled by bad scores. They *learn* the environment's mistakes by reading them.

**Biggest risk.** Harness evolution may have too little real headroom to damage. The pilot gate requires ≥ 8 points of real gain.

**After full-text check.**
- Novelty holds: no paper crosses trace source with fitness source under errors.
- The prior leans more toward *selected*. "Does Failure Feedback Direct Self-Modification?" finds DGM edits fix their targeted failure *less* often than other failures, and random selection reproduces DGM's trajectories. So frame the result as a measured teaching share, not a bet on "taught".
- Drop CHASE as support.
- Meta-Harness Table 3 (raw traces 56.7 vs scores only 41.3) is the precedent for the trace axis.
- Ready-made error sources: BreakingWeb fabricated-success, CHASE Syn-Ledger shortcuts, HackProbe channels.

---

## 2. Forced-Quota Dose
**How much should an improver be forced to rewrite itself?**

**RQs**
1. If we force the improver to edit itself in a fraction *q* of generations, does successor quality rise?
2. Is there a sweet spot? At q = 0 nothing happens; at q = 1 there may be constant churn.
3. Does the sweet spot move with evaluation noise?
4. Are forced edits *useful*, or just kept but inert?

**Core idea.** Improvers naturally almost never edit themselves, so the "self-improving" part is mostly idle. The simplest fix is to make self-editing mandatory some of the time. We predict an **inverted U**: some forcing helps, and too much forcing makes noisy, inert or harmful edits.

**Experiments**
- **Setup.** A Gödel-style loop on code tasks, with q ∈ {0, 0.1, 0.25, 0.5, 1} and matched compute.
- **Placebo quota.** The forced step "edits" but renders the original improver. This separates *being forced to edit* from *spending a generation on it*.
- **Measurement.** Every improver is scored by nested replay (order 1). Report the funnel **proposed → retained → effective**.
- **Noise.** Cross q with low and high evaluation noise.

**What each result means**
- Inverted U: there is a quantitative recipe for structurally forcing self-update.
- Monotone gain: just force it.
- No gain: forced self-edits are churn, so the bottleneck is *what* improvers write, not *whether* they write it.

**Contributions.** The first dose-response for mandated meta-edits at matched compute, with a placebo and an order-1 measurement.

**Central claim.** You can make an improver improve itself, but only in the right dose.

**Recommended merge.** Combine with #3 (its funnel and gate factor). The judges estimate this raises P(accept | done) to about 0.32.

---

## 3. Gate, Not Willingness
**Proposed, retained, inert: why improvers rarely improve themselves**

**RQs**
1. Do improvers rarely *propose* self-edits, or do they propose them and have the gate reject them?
2. How much do framing ("you may edit improver.py"), the gate type, and the model family each explain?
3. Of the self-edits that are kept, how many actually make better successors?

**Reframe after full-text check.** Published systems mostly *don't* reject self-edits. They keep everything valid and just sample good parents more often, or they force edits. So "the gate throws them away" is not how most systems work. The question becomes: under each real gate type (none / contract check / task-score argmax / replay score as in HMED), where do self-edits die: never proposed, not propagated by selection, or kept but useless? No paper measures that funnel.

**Core idea (original framing).** There are two explanations for the self-edit drought:
- **willingness:** the model avoids touching its own code;
- **gate:** a keep-if-better gate only sees the next task score, and a self-edit pays off later, so the gate rejects it.

Known data points: framing alone raises self-edits sharply (Improvement by Inheritance: 11/42 → 35/42; SelfImprove-Bench: 20–45% retained). So the real question is where in the funnel the edits are lost.

**Experiments**
- **Factorial.** Framing {neutral, explicit} × gate {next-step, delayed-credit next-step, meta-gate that scores successors by nested replay} × 2–4 open models, with 5 seeds.
- **Outcome.** Proposed rate → retained rate → effective rate (successor quality).
- **Credit Horizon.** The earlier "Credit Horizon" idea lives here: the delayed-credit gate tests whether *credit timing* explains retention.

**What each result means**
- Gate dominates retention: redesign gates.
- Framing or model dominates: it is a disposition (likely from post-training), so fix it with prompts or fine-tuning.
- Retained-but-inert: self-edits survive but do nothing, so the bottleneck is edit quality.

**Contributions.** The first causal decomposition of the self-edit drought.

**Central claim.** Improvers don't fail to self-improve because they refuse. We can measure exactly where in the pipeline self-improvement dies.

**Biggest risk.** "A next-step gate can't see delayed value" is near-tautological. Fix: include the delayed-credit next-step gate, and make the headline the *funnel*, not "gate wins".

---

## 4. Order-2 Audit
**Do claims of deeper recursive improvement hold up?**

**RQs**
1. MetaRSI reports a "meta-update advantage", and Metaⁿ reports emergent depth. Do these survive replication with seeds and controlled edit rates?
2. If level-1 edits are rare (p) and level-2 edits rarer still (p²), can order 2 even be measured without forcing?

**Core idea.** Recursive self-improvement claims keep stacking levels: improve the improver of the improver. If each level rarely edits, deeper levels are statistically invisible unless forced. Improvement by Inheritance already found that Metaⁿ's gain came mostly from replacing unparseable programs.

**Experiments**
- Replicate the MetaRSI-style term comparison on an open model with ≥ 5 seeds.
- Cross forced vs natural edits at levels 1 and 2.
- Run a doubly nested replay.
- Derive a simple futility rule: add a meta-level only if p·|gain| exceeds what your budget can detect.

**Contributions.** A replicated order-2 measurement and a practical "is another meta-level worth it?" rule.

**Central claim.** Most "deeper recursion" is undetectable or inherited. Here is how to tell.

**Risk.** A measurement and audit paper has a lower acceptance ceiling. It is cheap and a good fallback.

---

## 5. Order-0 Rewards Breed Routers, Not Improvers
**What should a meta-agent be rewarded for?**

**RQs**
1. When the meta-agent is optimized separately from the task agent (Looped-RSI, RSI-Harness, MetaEvoForge), which meta-reward actually produces better improvers?
2. Do common rewards (task score, improvement over parent, descendant quality) mostly select "routing" tricks such as "read the eval report"?
3. Does a reward that directly measures successor quality fix this, and at what cost?

**Core idea.** Current systems grade the meta-agent at order 0 (how good are its task agents). Improvement by Inheritance shows this rewards edits that route evidence to the task agent, not edits that make the improver better at improving. We compare meta-rewards head to head and score every arm at order 1.

**Experiments**
- **Seven arms at matched cost:**
  - child score;
  - improvement over parent;
  - descendant quality;
  - cheap order-1 estimate;
  - IFR-style replay credit;
  - random promotion;
  - forced quota with a placebo twin.
- **Measurement.** Nested replay on held-out starting states that the selection pool never saw. Report edit rate *p* and edit quality *μ* separately.
- **Calibration gate.** First replicate Improvement by Inheritance's +0.17 positive control on an open model.

**Contributions.** The first controlled meta-reward comparison measured at order 1, which audits the core assumption of the new dual-loop systems.

**Central claim.** Rewarding an improver for good task agents makes it a better router, not a better improver.

**Risk.** IFR and HMED own replay credit. The novelty is the head-to-head comparison and the p/μ split.

---

## 6. Meta-Overfitting
**Self-improvers get better at improving only where they grew up**

**RQs**
1. After an improver edits itself on domain A, is it better or worse than its original self at improving a fresh agent on a far domain B?
2. Does the effect grow with the distance between A and B?
3. Is it the improver's *procedure* (code) or its *memory* (notes) that carries over?

**Core idea.** Several ICLR 2027 papers (RSI-Harness, MetaEvoForge, FreeEvolve) report that evolved improvers transfer. None of them wipes memory or grades domain distance. We predict their transfer is near-domain or carried by memory, and that it **flips negative** on far domains. Design rule: on a new domain, *reset the procedure and keep the memory*.

**Experiments**
- **Lineages.** 10 lineages on home domain A, with memory wiped and domain facts scrubbed from the code.
- **Arms.** Original vs evolved procedure, each × with or without memory, on near, mid and far domains.
- **Comparison.** Reproduce one positive near-domain cell as a direct comparison to the existing papers.
- **Edit-rate fix.** Improvers barely self-edit. Fix this with the forced quota from #2.

**What each result means**
- Gain < 0 far away: meta-overfitting.
- Gain ≥ 0 far away: the first controlled evidence of general improvement skill.
- Scrubbing erases the gain: "procedures were memory in disguise".

**Central claim.** Self-improvement skill is specialization, not general learning to learn.

**Note.** This was "second-order plasticity" (meta-plasticity) in round 1, renamed and narrowed.

**After full-text check, novelty is reduced.**
- Hyperagents already reports *positive* far-ish transfer of an evolved meta agent (imp@50 = 0.630).
- FreeEvolve already reports *negative* cross-environment transfer of evolved improver skills, carrying the procedure only.
- What remains open: graded distance, a procedure × memory 2×2, multiple lineages, and reproducing both of those results in one setup.

---

## 7. Varying Goals
**Self-editing pays off only when the world keeps changing**

**RQs**
1. Is an improver that can edit itself better than a fixed one on a static task stream?
2. Does it win when the environment switches between real library versions on a schedule?
3. Is the advantage largest at medium switching speed, as in evolutionary biology?

**Core idea.** This borrows from biology (Kashtan & Alon): organisms evolve adaptability mainly when goals keep switching. Applied here, self-editing is wasted on static benchmarks, which is a second explanation for the self-edit drought. It should pay off under repeated real change.

**Experiments**
- **DriftStream.** Executable coding tasks pinned to real breaking changes (pandas 1→2, NumPy 2, pydantic v2, SQLAlchemy 2), switching every P epochs, P ∈ {1, 3, 10, never}.
- **Arms.** Fixed vs editable improver, plus a memory-only baseline.
- **Test.** A future library version never seen during training.

**Contributions.** A grounded continual benchmark and an evolvability law for meta-agents.

**Central claim.** Self-modification is a tool for changing worlds. Static benchmarks can't show its value.

**Risks.** API drift can often be fixed without self-editing (add "check the version" to the task agent). The proposed formula must actually predict a peak. Four switching speeds is too few to show a curve, so add more.

---

## 8. Openness Ladder
**How much of itself should a self-improver be allowed to edit?**

**RQs**
1. Holding everything else fixed, how does held-out gain change as more becomes editable?
2. Is the best level in the middle?
3. Does the best level move with model strength, validation-set size and evaluation noise?

**Core idea.** Systems differ mostly in how constrained their search space is, but every comparison changes the whole system at once. Change *only* the editable surface, one rung at a time:
- **L0:** number slots
- **L1:** prompt
- **L2:** building-block DSL
- **L3:** harness code
- **L4:** improver code
- **L5:** selection logic

More openness gives more reach but also more overfitting to the validation set, so we expect an inverted U.

**Experiments**
- **Grid.** 3 model sizes × 6 rungs × 5 seeds at matched tokens.
- **Tasks.** One exactly graded task, one noisy agentic task, one transfer task.
- **Control.** A random proposer at each rung, to separate "the space is good" from "the model searches well".

**Contributions.** The first randomized openness-vs-gain curve, and a law for where to set the dial.

**Central claim.** "More open" is not better. The right openness depends on how strong your model is and how well you can measure.

**Risks.**
- Finding the best rung with 5 noisy seeds is unstable.
- The rungs differ in *kind*, not just amount.
- RobustSGPO already shows a 3-rung version.

The judges suggest narrowing to fewer rungs with more seeds.

---

## 9. MetaShift
**A benchmark where the improvement procedure itself must change**

**RQs**
1. When the *rules of improving* change (flaky tests, coarser feedback, smaller eval budget), do self-editing improvers recover faster than fixed ones?
2. When only the *tasks* change (library API drift), is there no advantage?
3. Can a benchmark separate "the agent must change" from "the improver must change"?

**Core idea.** Two kinds of change are possible:
- **Task change:** fixable by editing the task agent.
- **Regime change:** the improver's own method becomes wrong. For example, with 15% flaky tests it should start re-running tests.

Self-editing should only help for regime change.

**Experiments**
- **Shifts.** Real CI-style regime shifts (measured flake rates, granularity loss, budget cuts) vs real API drift, with 12 generations before and 12 after.
- **Arms.** Fixed improver, editable improver, editable improver with a meta-gate.
- **Metric.** Recovery time.
- **Manipulation check.** A hand-written "right procedure" (e.g. majority-vote re-evaluation) must beat the fixed improver after the shift.

**Contributions.** A grounded benchmark with a built-in crossover test.

**Central claim.** Self-referential improvers are only worth it when the improvement problem itself changes.

**Risks.**
- With natural self-edit rates near 0, the editable arm may behave exactly like the fixed one. Add the forced quota from #2.
- "Equal difficulty" between the two shift types is undefined.

---

## How these fit together

- **#1 stands alone.** It is about how environment errors enter any reflective optimizer. It is the current lead and fits Xunjian's BreakingWeb and world-model line.
- **#2, #3, #5, #6 share one testbed** (a Gödel-style loop plus nested replay). The forced quota (#2) is the common fix for "improvers barely self-edit", which every one of them needs.
- **#7, #8, #9 are about *when* self-editing should help:** openness, drift rate and drift type. Round 3's combined version ("Open Where the Cause Is") scored lower because the build is too big for one person.

**Plan**
- **Week 1:** pilot #1.
- **Weeks 1–2:** pilot #2. It also measures the natural self-edit rate and the cost of nested replay, which de-risks #3, #5 and #6.
- **Around Nov 30:** decide which to pursue.

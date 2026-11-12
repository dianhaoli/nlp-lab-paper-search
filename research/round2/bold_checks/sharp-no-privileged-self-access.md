# Bold check: sharp-no-privileged-self-access — "Self-referential agents don't know themselves"

Reviewer stance: skeptical AC plus related-work searcher. Date: 2026-11-12.
**Verdict: PROMISING. Score 73/100 as pitched, about 77 after the fixes in §5.**

The lineage-level question is unscooped. However, the *model-level* version of the claim is already an established null, so the paper must be built so that the null is not trivially guaranteed.

---

## 0. One-paragraph verdict

No paper I found compares a self-referential improver diagnosing or repairing **its own lineage's agent** against a **foreign improver** (same base model, different lineage). That gap is real, and the question goes straight at Gödel Agent's founding premise, which makes it ideal for this mentor.

The problem is that the *model-level* analogue has been settled, mostly negatively, by four 2026 papers:
- *Strangers to Themselves* (2609.09899): an outside analyst given the same behavioral history predicts a model better than the model predicts itself. This includes τ² agentic evals.
- *Masked by Consensus* (2604.12373): there is no self-premium for correctness, except a ~0.02 AUC premium in factual QA on disagreement subsets.
- *Evaluating & Improving LLM Self-Modeling* (2608.30980): there is no consistent own-model advantage.
- *Repairs for Whom?* (2610.38610): the deployed model judging itself is the worst repair evaluator.

So "no privileged self-access" is the **expected** outcome, not a surprise. The paper survives only if it:
1. makes the **lineage**, not the weights, the locus of "self";
2. uses a design in which a self-advantage *could* show up. That means within-task/disagreement scoring and a memory-transplant arm, because otherwise pooled AUROC guarantees a tie;
3. turns the null into a positive, mechanistic statement: *self-knowledge in self-referential agents lives in transferable memory, not in the act of self-reference, and it goes stale.*

The pitch also has a logical bug: at gen-0 every lineage is the same agent, so "self vs foreign AUROC at gen-0" is undefined (see §3, W1).

---

## 1. Scoop searches run

| # | Channel | Query / target | Result |
|---|---|---|---|
| 1 | alphaXiv discover (recency) | self-prediction / privileged access / introspection / agent failures, self vs other | Found 2609.09899, 2604.12373, 2608.30980, 2610.05572, 2610.38610, 2610.07186, 2610.38911, Zero-Call Verification (2611.end-state-agreement…) |
| 2 | alphaXiv discover (recency) | self-modification + self-model + Gödel Agent + self-awareness; self-model drift | Only RSI system papers and audits (2610.14662, 2610.21646, 2611.12988, 2608.07645, 2609.37968…); none compares self vs foreign improvers |
| 3 | alphaXiv discover (recency) | self-referential vs external meta-agent improving the same agent | VERSE 2610.02616, RSIFlow 2610.21791, RSI Tax 2611.13524, SpecRSI 2610.16491, RSI-Env 2611.01736. None runs a self-vs-foreign improver crossing |
| 4 | alphaXiv discover | self vs cross-model correctness/failure prediction | Kadavath 2207.05221, 2512.20578, 2609.33886, 2606.30653, 2611.02253. All are model-level |
| 5 | alphaXiv discover | self-authored vs other-authored code repair | *Style, Not Self* 2609.30048 (code attribution is surface style); *Self-Preference Is Weak or Absent…Genuine Authorship* 2606.20093; self-repair placebo papers 2606.31511, 2607.26117, 2611.06863. None works at the lineage level |
| 6 | Semantic Scholar keyword (5 phrasings, 2025–26) | privileged self-knowledge agent; self-model drift; self-referential vs foreign improver; self vs cross prediction | Noise only. Nearest: *Layered Mutability* 2604.14717 (governance), *MicroVerse* 2608.15844 (identity drift in multi-agent LMs). Neither tests diagnostic self-knowledge |
| 7 | S2 forward citations of Gödel Agent 2410.04444 | (index returned only 26 citers) | Exo 2611.00463, EvoUndo 2608.28363, PACE 2606.08106, SkillRevise 2606.01139, Polaris 2603.23129, HGM 2510.21614… none tests self-knowledge |
| 8 | WebSearch ×2 (incl. OpenReview / ICLR 2027) | self vs other diagnosis; privileged access in self-modification | Nothing at ICLR 2027 found. Surfaced SkillMentor 2607.27360 (a trained external mentor diagnoses an executor's blind spots, an "other diagnoses" baseline) |

**Deep reads (answer_pdf_queries):** 2609.09899 (full design and results), 2604.12373 (full), 2610.38610 (full), 2608.30980 (cross-model section), 2610.05572 (AUROC decomposition). I grepped Zero-Call Verification for a self-vs-foreign judge comparison: there is none. It reports only that Opus 5.5 accepts 65/66 of its own agent's always-failed AppWorld attempts, plus cross-model checks.

---

## 2. Closest prior and the gap each leaves

| Paper | What it shows | Gap left for this idea |
|---|---|---|
| **Strangers to Themselves** 2609.09899 (Sep 2026) | 9 behavioral evals, 12 models, including τ² agentic. Self-report r=+0.04. A third-party analyst given the model's *own* history beats self (+0.28 vs +0.19). Models do not predict themselves better from their own history than from another model's. Fine-tuning creates a "fixed-point problem": the self-prediction describes the model it *used to be* (behavioral drift). | **Closest conceptual scoop.** Its subject is model dispositions measured as rates, not a *self-modifying lineage* reading its own code. It has no repair outcome and no generational dynamics. Its "drift" is fine-tuning-induced, not self-modification-induced. It already predicts our null, which lowers Surprise. |
| **Masked by Consensus** 2604.12373 (Apr 2026) | Self vs peer hidden-state probes for correctness. No premium on full sets, because ~75–80% inter-model agreement masks any premium. On disagreement subsets there is a ~0.015–0.035 AUC premium in factual QA and none in math. | **Methodological must-cite.** Pooled self-vs-other comparisons are confounded by agreement. Our 200-task AUROC will tie by construction unless we score on lineage-disagreement tasks. Weight-level only. |
| **Evaluating & Improving LLM Self-Modeling** 2608.30980 (Aug 2026, Anthropic Fellows) | Cross-model explainer transfer shows no consistent own-model advantage; a stronger explainer wins on the other model's labels. | Single-turn prompts plus BLOOM multi-turn. No lineage, no code-level self-modification. |
| **Repairs for Whom?** 2610.38610 (Oct 2026) | Repair value depends on the executor. Selecting repairs with the deployed agent's own *rollouts* is best, while the deployed model *as a judge reading its log* is among the worst (loss 0.59–0.74). | Shows that "self" matters through **execution**, not through **reading**. This is a useful distinction for our repair arm, which must separate self-read from self-rollout. Not about improvers. |
| **Disentangling Task Difficulty…** 2610.05572 (Oct 2026) | Over 99.9% of pooled-AUROC pairs are cross-task. A difficulty oracle gets 0.945 pooled AUROC but 0.5 within-task. | Kills the proposed metric as written. Pass/fail AUROC on 200 tasks is mostly a difficulty measure, which both readers share. We need within-task (repeated runs) or disagreement-subset AUROC. |
| Gödel Agent 2410.04444; Does Failure Feedback Direct Self-Modification? 2610.14662; HMED 2610.07979; introspection papers 2610.07186, 2610.38911 | Gödel Agent assumes self-awareness. 2610.14662 finds DGM edits are not target-directed. Introspection papers work at the activation level. | None compares self with a foreign reader of the same lineage. 2610.14662 is a natural companion: undirected edits and no self-advantage both point to "selection, not self-knowledge". |
| SkillMentor 2607.27360 (Jul 2026, via WebSearch; not deep-read) | A trained external mentor diagnoses a frozen executor's blind spots. | Evidence that "other diagnoses" works. It does not compare with self. |

**Bottom line on novelty:** the *object* is new (self-referential lineages, generational drift, code-level self-description, repair outcome). The *question* (privileged self-access vs a foreign observer) and its likely *answer* (none) are well established at the model level. Reviewers will say "known null, transplanted to agents" unless the design adds something those papers cannot test: the **lineage-memory decomposition** and the **drift dynamics**.

---

## 3. Mock review (as pitched)

**Strengths**
- Strikes at the core premise of the mentor's own line of work (Gödel Agent self-awareness). It is crisp, falsifiable and cheap.
- A round-robin improver×agent design is the right shape. It cleanly separates improver skill from agent diagnosability.
- Prediction, repair and description probes triangulate "self-knowledge" behaviorally.

**Weaknesses**
- **W1 (logic bug).** At gen-0 all lineages share the same agent, so self = foreign identically. The falsifier "≥0.05 at gen-0, not declining with k" cannot be evaluated. The self-advantage can only *emerge* after divergence. The correct prediction is therefore "rises then falls" or "never appears", not "shrinks from gen-0".
- **W2 (guaranteed tie).** Pooled pass/fail AUROC is dominated by shared task difficulty (2610.05572) and inter-lineage agreement (2604.12373). Same-base-model lineages will agree on most tasks. Without disagreement-subset or within-task scoring, the null is an artifact, not a finding (K2 risk).
- **W3 (what is "self"?).** With identical weights, any self-advantage must come from (a) the lineage's memory, notes and stored self-description, or (b) its evolved improver procedure being co-adapted to its own agent. "Reading the same code and traces" already gives the foreigner most of (a). The design must manipulate memory access explicitly, or reviewers cannot interpret either outcome.
- **W4 (prior null).** 2609.09899 already shows that an outside analyst with the same record beats self, including on agentic τ². A null here is the expected result. The surprise must come from the decomposition and the drift curve.
- **W5 (lineage-quality confound).** Better-performing lineages produce better improvers *and* more predictable agents. This needs a crossed random-effects (Social Relations Model) analysis, not a raw self-vs-foreign difference.
- **W6 (repair metric).** Repair should be scored by the *same* executor (base model plus target agent code) with matched rollouts, following 2610.38610. Also separate "self proposes, foreign proposes" from "who verifies".
- **W7 (scope).** Eight lineages × 20 gens of Gödel-style self-modification plus 4 checkpoints × 8×8 crossings is fine for prediction (cheap), but repair with rollouts can blow past $500. Budget about $1–1.5k.

**Likely score as pitched:** 4–5 (borderline). With the fixes below: 6 (weak accept). Reviewer words to expect: "known null (Strangers to Themselves)", "AUROC confounded by difficulty", "what is the self if weights are shared?".

---

## 4. Rubric scores (as pitched)

| Dim | Score | Reason |
|---|---|---|
| N (×3) | 3.5 | No lineage-level self-vs-foreign study exists. However, 3–4 model-level papers from 2026 ask the same question and answer it, which caps novelty. |
| S (×2.5) | 3.0 | A null is expected given 2609.09899 and 2608.30980. A positive self-advantage, or a memory-vs-procedure split with drift, would be surprising to the RSI community. |
| R (×2) | 5.0 | Directly tests Gödel Agent's self-awareness premise; self-referential agents plus a self-model, i.e. a world model of oneself. |
| C (×2) | 3.0 | The round-robin is good, but the gen-0 bug, pooled-AUROC confound and missing memory arm would be caught by reviewers. Rises to 4.0 with the fixes. |
| F (×1.5) | 4.0 | Open 27B/9B models. Prediction arms are cheap; repair arms are moderate. Realistic budget $1–1.5k. Pilot within a week. |
| T (×1) | 4.0 | Introspection/self-modeling is hot in Aug–Nov 2026, as is RSI auditing. Rising, and not yet applied to self-modifying lineages. |

**Total = 20 × (3·3.5 + 2.5·3 + 2·5 + 2·3 + 1.5·4 + 4)/12 = 20 × 44/12 ≈ 73.3** → viable/promising.
With the fixes (C→4, S→3.5): 20 × 46.25/12 ≈ **77**.

**Kill flags:**
- **K1:** no. No 2026 paper runs this on self-modifying lineages.
- **K2:** at risk as pitched, because a pooled-AUROC null is uninformative. Fixed by disagreement scoring and the memory transplant, after which either outcome is informative.
- **K3:** no. **K4:** no.

---

## 5. Fixes that make it sharp

1. **Redefine "self" as lineage, and decompose it.** Use a 2×2 per (improver, target agent) pair: *reader identity* {own lineage, foreign lineage} × *memory access* {own lineage memory/self-description, none}. A foreign reader given the target lineage's memory is the decisive arm.
   - If self ≈ foreign+memory > foreign−memory, then self-knowledge is just *transferable memory* and self-reference adds nothing.
   - If self > foreign+memory, the self-advantage lives in the co-adapted procedure. That would be the first evidence of procedure-level privileged access.
2. **Social Relations Model analysis** (Kenny's round-robin model from social psychology; a non-obvious import, unlikely to be scooped). Run an 8×8 improver×agent matrix at each checkpoint. Decompose diagnostic accuracy into actor (improver skill), partner (agent legibility) and **relationship/self** effects. The self effect is the diagonal relationship variance. This fixes the lineage-quality confound (W5) and gives a reusable estimand.
3. **Score where self *could* win.** Report within-task AUROC on repeated runs plus AUROC on the **lineage-disagreement subset** (tasks where own-agent and foreign-agent outcomes differ), as in 2604.12373 and 2610.05572. Report pooled AUROC only as a contrast, and show that a difficulty oracle ties it.
4. **Fix the generational claim.** Checkpoints at gens {5,10,20,40} after verified divergence (a manipulation check that agent code differs and behavior disagrees on at least 20% of tasks). The prediction is that the self-advantage *never appears*, or *appears and then decays*.
5. **Drift as the headline dynamic.** Have the improver *write* a self-description at gen k. Score it as a predictor of the gen-k+j agent's behavior, against a fresh description written by a foreign reader from current code. The **staleness curve** (accuracy versus j) is new: 2609.09899's drift is fine-tuning-induced, and here it is self-modification-induced. Link it to Gödel Agent: does stale self-description predict bad edits?
6. **Repair arm per 2610.38610.** Fix candidate edits. Score each by matched same-state rollouts of the target agent. Compare self-selected vs foreign-selected edits (selection regret), plus self-proposed vs foreign-proposed (proposal quality) at matched budgets.
7. **Pilot (<$50, 1 week).** Run 4 Gödel-lite lineages for 10 gens on one task family with Qwen-class models. Measure the inter-lineage disagreement rate, because if it is under ~10% the study has no power. Then run one 4×4 prediction crossing on the disagreement subset.
8. **Framing.** Position it as the *agentic, self-modifying* successor to 2609.09899 and 2604.12373, and as a mechanism study for Gödel Agent. It can also slot in as the "Cause" (procedure-vs-memory) section of the round-1 merged lead paper. Note the overlap and decide early whether this is a standalone paper.

---

## 6. Sharpened claim

> **"Self-reference buys no self-knowledge.** After self-modifying lineages diverge, a *foreign* improver with the same base model, given the target lineage's code, traces and memory, predicts the agent's failures (within-task and disagreement-subset AUROC) and selects repairs as well as the lineage's own improver. The Social-Relations 'self' effect is about zero once memory is transplanted. Removing memory, not changing the reader, is what hurts. The improver's stored self-description goes stale faster than a stranger's fresh reading of the current code (self-model drift). **Falsified** if the diagonal self effect exceeds 0.05 AUROC on the disagreement subset *and* survives a full memory transplant to the foreign reader. That outcome would be the first evidence of procedure-level privileged self-access in self-referential agents."

---

## 7. Sources seen
- alphaXiv: 2609.09899, 2604.12373, 2608.30980, 2610.38610, 2610.05572, 2610.07186, 2610.38911, 2611.end-state-agreement-compiled-verification-checks, 2610.14662, 2610.22984, 2609.30048, 2606.20093, 2610.02616, 2611.13524, 2610.21646, 2611.12988, 2512.20578, 2609.33886.
- WebSearch: SkillMentor https://arxiv.org/pdf/2607.27360 (not deep-read); DAIR summary of 2608.30980 https://academy.dair.ai/papers/evaluating-and-improving-llm-self-modeling-2608.30980; DGM ICLR 2026 notes https://en.papernotes.org/ICLR2026/llm_agent/darwin_g%C3%B6del_machine_open-ended_evolution_of_self-improving_agents/. No ICLR 2027 OpenReview hit.
- Unverified: the claim in the idea card that the Zero-Call result involves "its own agent's failed attempts" was confirmed only from the abstract ("accepting 65 of the 66 attempts at AppWorld tasks the agent always fails"). That paper has no foreign-judge comparison.

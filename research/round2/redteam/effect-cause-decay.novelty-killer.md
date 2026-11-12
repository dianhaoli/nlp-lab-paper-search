# Novelty-killer red team: "Do Self-Referential Improvers Get Better at Improving? Effect, Cause, and Decay"

*Round 2, 2026-11-12. Stance: try to prove the idea is already done, or will obviously be done before mid-May 2027. Sources merged: proposals/meta-plasticity.md, proposals/procedure-vs-memory.md, critiques/swap-factorial.md, REPORT.md, plus the round-2 verification digest.*

## Verdict: MAJOR REVISION. Score 67.5/100 (down from the panel's 75.8)

K1 is not triggered. No paper runs the full P0/Pk × M0/Mk crossing with placebo memory and a domain-B decay arm at matched compute and multiple seeds.

The problem is that **each of the three arms now has a published one-cell precedent**:

| Arm | Published one-cell precedent |
|---|---|
| Effect | MetaRSI Term-2 fork |
| Cause | SelfSearch Table 4 |
| Decay | VERSE Appendix G.2, cross-domain warm vs cold optimizer transfer. Round 1 called VERSE a "one-cell" paper, which undersold it. |

Three groups (SelfSearch, VERSE, Organon) and the Auditing census (2610.27610) either name the remaining experiment as future work or ask for it outright.

The merged paper is therefore at high risk of being read as "the powered version of three ablations that already exist". Reviewers would call that a design, not an idea, and see a K4-flavoured combination. One more risk: the most surprising branch, the warm-start penalty, is weakly contradicted by VERSE G.2, where the inherited optimizer helps on SWE→Terminal-Bench.

---

## 1. Searches run (fresh, this pass)

1. **alphaXiv, recency.** Two queries:
   - "later-generation improver better at improving / same-start swap / procedure vs memory transplant"
   - "self-improving agents lose plasticity / transfer improvement ability to new domain"
2. **alphaXiv, published after 2026-10-25, recency.** Two queries:
   - "what does an evolved improver learn: procedure vs memory/archive"
   - "meta-level plasticity loss / improvement rate on new domain"
3. **Semantic Scholar keyword search, publicationDate ≥ 2026-07-01.** Five queries: transplant memory procedure; "improver get better at improving"; meta-agent warm start new domain; improver generation swap matched budget; evolved optimizer transfer new task.
4. **Semantic Scholar forward citations** of 2610.27610 (0 indexed), Agent Plasticity 2610.08902 (3: RSI Tax, Frozen-Weights failure traces, Artifact-to-Capability), SelfSearch 2609.37968 (0 indexed) and MetaRSI 2609.06396 (Veritas, RSI Arena, Auditing, Harness-Policy Optimization, DuplexAgent-RSI, VERSE; none run the crossing).
5. **WebSearch.** Two queries:
   - The ICLR 2027 template header plus "improver / better at improving". Only R2Flow 2609.33867 surfaced.
   - "openreview ICLR 2027 recursive self-improvement meta-agent improver transfer procedure memory ablation". Nothing new; Recuris 2608.24876 keeps its meta-agent fixed.
   - OpenReview itself is still unreachable from this session. **OpenReview-only ICLR 2027 submissions remain UNCHECKED.**
6. **Deep reads** with answer_pdf_queries:
   - Lineage Is Memory 2610.41157
   - SelfSearch 2609.37968
   - RSI Tax 2611.13524
   - "When, Where, and How Experience Becomes Structure" (alphaXiv slug 2611.recursive-self-improvement-llm-agents)
   - MetaRSI 2609.06396
   - VERSE 2610.02616 (Appendix G)

## 2. Strongest kill evidence, arm by arm

### Effect (does a gen-k improver beat gen-0 from the same start at the same budget?)

- **MetaRSI 2609.06396 §5.4 (verified this pass).** Term 2 forks S1:
  - Original improver: +4.9. Meta-updated improver: +7.2 at the same budget.
  - Over a five-term curve: +26.2 vs +20.7, with the gap widening every term.
  - "Policy carried to next term": 68% vs 38%.
  - This is a positive answer to the Effect question at one state.
- **Other positive same-start answers:** RSIFlow 2610.21791 (Meta0..Meta4 on a common task agent), FreeEvolve 2610.09197 and Skill∞ 2610.35434.
- **Field status.** "Effect" is no longer a question the field sees as open. It is a reported result. All that remains is the claim that the existing tests are underpowered or confounded by headroom. 2610.27610 makes that claim too, but its contribution is conceptual: it formalizes Prop. 3 and runs no experiments.

### Cause (procedure vs memory)

- **SelfSearch 2609.37968 Table 4 (verified).** On SWE-bench Verified at generation 10, two lineages:

  | Family | Full SelfSearch | w/o episode records | w/ fixed improver |
  |---|---|---|---|
  | GPT | 76.2 | 74.2 | 74.6 |
  | DeepSeek | 86.7 | 83.8 | 83.8 |

  - The authors frame the question in our words: "Can agents likewise use the experience of self-improvement to become better at improving themselves?"
  - They state the next step: "Further work should examine how consistently these gains arise across independent searches."
  - What is still missing: no crossing, no placebo memory, a single search per cell, n=120, and gaps of 1.7–2.9 pp, which are inside the noise.
- **Lineage Is Memory 2610.41157 (verified).** 1,750 pre-registered runs with n=10 per cell and bootstrap CIs.
  - Parent inheritance gives Δ = +0.238 [0.224, 0.252], which dominates feedback breadth.
  - A one-sentence "carry facts" instruction moves banking77 from 0.372 to 0.856. Domain facts copied into the artifact carry the gain.
  - This is object-level: the proposer is fixed. But it sets the rigor bar reviewers will expect: pre-registration, n=10, about 25 hypothesis tests.
  - It also strongly predicts the "memory wins" branch, which makes that branch less surprising.
- **The survey (alphaXiv-only)** explicitly calls for testing "does inherited state help produce more effective future updates under comparable experience, feedback, and computational budgets?" and for separating "the retained object and the improvement process". The question is now public.
- **Other partial results:**
  - Continual AutoResearch 2611.05108: matched, shuffled and no-memory cards, actor level.
  - MetaSkill-Evolve: its σx ablation hints that memory carries the gain.
  - Faithful Self-Evolvers 2601.22436: condensed experience is inert.

### Decay (warm gen-k improver on a new domain B)

- **VERSE 2610.02616 Appendix G.2, Table 9 (verified; underweighted in round 1).** This is the single most damaging new finding. Evolved harnesses move SWE-rebench ↔ Terminal-Bench, and three arms are compared: frozen optimizer with warm executor harness, evolved optimizer with warm executor harness, and evolved optimizer with cold executor harness. That is already a 2×2-ish split of optimizer-procedure+notes vs executor state, on a new domain.
  - The authors conclude: "what crosses domains appears to be the optimizer's methodology, its skills, notes, and tools … while the executor harness stays specific to its source domain."
  - SWE→Terminal-Bench, round 1: VERSE with inherited optimizer scores 40.07, above every frozen-optimizer entry.
  - Gaps: single runs (SEM is over test evaluations only), only two rounds on B, and notes (memory) are lumped with tools and prompts (procedure).
  - **Implication:** the existing data point weakly argues *against* the warm-start penalty. In the reverse direction the result is flat or mixed.
- **RSI-Opt 2610.32026.** Held-out transfer of the evolved policy peaks at generation 1, then specializes. This is a precedent for the decay shape, at product level.
- **MetaExp 2610.21492.** Untrained backbones get worse as experience accumulates; experience learning trained into weights transfers. Weight-level.
- **Future-work flags.** Agent Plasticity 2610.08902 lists improver-strategy transfer as future work. Organon 2610.33046 lists cross-task carryover as future work.

### Not threats (checked this pass)

- RSI Tax 2611.13524: streaming cost; no improver swap.
- Third Eye 2611.12988: weights, next-update utility.
- RSI-Env 2611.01736, ModularRSI 2609.14857, Hippo 2610.24453, Reglos 2611.06049 and XferAgent 2610.19581 (object-level experience transfer): none run improver-level transplants.

## 3. Will it obviously be done before May 2027?

**Probability that a paper with a powered (≥3 seeds or lineages) improver-procedure × memory crossing appears on arXiv before 2027-05-15: about 55–65%.**

Candidates likely to do it:
1. SelfSearch v2 or a camera-ready version (the authors flag replication across independent searches).
2. A VERSE follow-up that splits notes from tools.
3. A follow-up to Auditing 2610.27610 or Flex-RSI.
4. Agent Plasticity or Meta FAIR extending to the improver level.
5. ICML 2027 submissions, which go public around late January to February.

**Probability that the cross-domain decay arm with a forgetting/reset factorial is also covered: about 30%.** Decay is the least-covered arm, but VERSE G.2 has already shown the design is cheap and natural.

## 4. Rubric scores

| Dim | Score | Why |
|---|---|---|
| N (×3) | 2.5 | Each arm has a one-cell 2026 precedent: MetaRSI (Effect), SelfSearch Table 4 (Cause), VERSE G.2 (Decay). Three or more groups name the full experiment. No K1, but ≥2 close papers within 6 months matches the rubric's "1" anchor for single arms. The integration and power are new. |
| S (×2.5) | 3.0 | "Memory wins" is now predicted by Lineage Is Memory, SelfSearch and MetaSkill σx, so it is less surprising. "Penalty" is the surprising branch, but VERSE G.2 hints the other way. "Procedure wins" repeats MetaRSI's claim with more seeds. Only an outcome that overturns VERSE or MetaRSI would be headline-grade. |
| R (×2) | 5.0 | Gödel Agent's own question, and Xunjian's agenda exactly. |
| C (×2) | 3.5 | A clean factorial, but three improver families × factorial × domain B × seeds is heavy. The comparison bar is now pre-registered n=10 designs like Lineage Is Memory. |
| F (×1.5) | 3.0 | Open-model improvers do run (VERSE uses Qwen3.8; SelfSearch DeepSeek runs cost $4–9 per search). The full grid is still $1.5–3k. A weak-improver null remains a K2 risk. |
| T (×1) | 4.0 | Hot, and arguably peaking: the RSI-evaluation meta-research wave is saturating, with 10+ audit papers in Oct–Nov. |

**Total = 20 × (3·2.5 + 2.5·3.0 + 2·5 + 2·3.5 + 1.5·3.0 + 1·4.0) / 12 = 20 × 40.5 / 12 = 67.5.**

## 5. Attacks a reviewer will make

1. "This is SelfSearch Table 4, MetaRSI §5.4 and VERSE Table 9 run with more seeds."
2. "Memory-dominance is already established at object level by Lineage Is Memory (1,750 pre-registered runs), so the improver-level version is expected."
3. "Your penalty hypothesis is contradicted by VERSE G.2, where the inherited optimizer methodology transfers SWE→TB."
4. "Procedure and memory are not separable: VERSE's optimizer 'notes' and SelfSearch's tools are both written from history. Lineage Is Memory shows that facts migrate into artifacts, so Pk contains memory." Without a fact-scrubbing control (κ), the Cause decomposition is not identified.
5. "Weak open improvers will show no Effect (FreeEvolve: Haiku-class overfits), so a null is uninterpretable."
6. "Three improver families × 8 arms × domain B × 5 seeds at $1.5k means each cell is underpowered. Lineage, not seed, is the unit."

## 6. Fixes (to get back above 72)

1. **Make Cause plus "knowledge-in-procedure" the headline, not Effect.** Drop Effect to a manipulation check, since MetaRSI and others own it. The new and sharp estimand is κ: how much of an evolved improver's *procedural* advantage disappears when domain facts are scrubbed out of its code and prompts. This applies Lineage Is Memory's "carry facts" mechanism to the meta level. Prediction: evolved improver procedures are mostly memory in disguise.
2. **Make Decay a direct, pre-registered replication-and-extension of VERSE G.2.** Split notes from tools, use ≥3 lineages, run 5+ rounds on B, and add forgetting/reset arms. Either outcome is publishable: "VERSE's methodology transfer is notes, not tools" or "it holds up but decays with k". Cite VERSE as the motivating anomaly instead of hiding it.
3. **Add a placebo/shuffled-memory and "vacuous-history" arm** (Faithful Self-Evolvers method, extended to the improver's archive). The finding that a vacuous always-pass verifier still trains skills (2610.11464) hints that shuffled improver history may work as well as real history, which would be a surprising result.
4. **Use one improver family that you control (Gödel-lite / SelfSearch-style), plus VERSE's open code as a second.** Make lineages the replication unit (≥4). Pre-register at Lineage Is Memory's standard.
5. **Post the preprint by early February 2027**, ahead of the ICML-cycle wave. Re-check SelfSearch v3, VERSE v2 and the 2610.27610 citers weekly. Check OpenReview ICLR 2027 by hand; it is still unchecked.

## 7. Non-obvious, lower-scoop angles (field anticipation)

These are less likely to be scooped because nobody's future-work list names them.

- **A. "Procedures are memory in disguise" (κ-scrub).** Covered in fix 1. Mechanism-level, cheap: an LLM rewrites Pk to remove task-specific tokens while keeping control flow. No one does this at the improver level.
- **B. Introspective drift.** In SelfSearch, generations 6–10 build trajectory readers and filters that are never used downstream. Does improver evolution spend more and more of its edits on its own inspection tooling, and does that track the warm-start penalty? Measure with call-coverage instrumentation. Low scoop risk; squarely Gödel Agent.
- **C. Home-model penalty / improver–executor coupling.** In RSI-Opt, the evolved policy helps other models but slightly hurts its own; in VERSE G.3, model-quirk fixes do not transfer. Test whether an evolved *improver* transplanted to a new executor beats one evolved with that executor. Cheap transplant arm.
- **D. Shuffled-history improver.** Covered in fix 3. A placebo test of whether improvers read their own history at all.
- **E. Accepted-edit clock.** Index "generation" by accepted edits rather than rounds. Feedback Dynamics 2608.02636 found only 55 of 388 candidates became new bests. This changes the Effect/Decay curves and removes a confound every prior paper has.

## 8. Sharper claim (replacement headline)

> "What a self-referential improver learns is mostly the domain, not how to improve. Across ≥4 lineages per family, scrubbing domain facts from an evolved improver's own code and prompts removes most of its same-start advantage. Its archive or notes carry the rest, and that advantage turns negative on a new domain. A shuffled-history placebo matches real history on domain A but not on domain B."

If the opposite happens (the scrubbed procedure keeps its advantage and transfers), that is the first controlled evidence of domain-general procedural meta-learning in LLM improvers. Either outcome is publishable.

---

### Verified sources (this pass)

alphaXiv: [2609.37968](https://www.alphaxiv.org/abs/2609.37968) · [2610.41157](https://www.alphaxiv.org/abs/2610.41157) · [2609.06396](https://www.alphaxiv.org/abs/2609.06396) · [2610.02616](https://www.alphaxiv.org/abs/2610.02616) · [2611.13524](https://www.alphaxiv.org/abs/2611.13524) · [2611.recursive-self-improvement-llm-agents](https://www.alphaxiv.org/abs/2611.recursive-self-improvement-llm-agents) · [2610.27610](https://www.alphaxiv.org/abs/2610.27610) · [2610.08902](https://www.alphaxiv.org/abs/2610.08902) · [2611.05108](https://www.alphaxiv.org/abs/2611.05108)

Web: [R2Flow 2609.33867 (ICLR 2027 header)](https://arxiv.org/pdf/2609.33867) · [Recuris 2608.24876](https://www.alphaxiv.org/abs/2608.24876) · [Awesome-Harness-Self-Improvement](https://github.com/leezythu/Awesome-Harness-Self-Improvement)

Unchecked: OpenReview-only ICLR 2027 submissions; NeurIPS 2026 workshop accepted lists.

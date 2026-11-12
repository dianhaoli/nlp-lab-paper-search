# Procedure or Memory? A Transplant Test for Self-Referential Improvers

**Proposal for NeurIPS 2027 (main track).** Dan, advised by Xunjian Yin. Draft of 2026-11-12. Idea id `procedure-vs-memory`, topic T1 (improver scaling and meta-level ability).
Revised after the 72.5/100 critique (`critiques/procedure-vs-memory.md`). I also ran a new targeted literature pass (alphaXiv plus S2 forward citations of MetaSkill-Evolve, Skill∞, VERSE and HMED), which found two more close papers: **HMED 2610.07979** and **VERSE 2610.02616**. Section 4 covers them.

---

## 1. Title and headline claim

**Title.** *Procedure or Memory? A Transplant Test Shows What Self-Referential Improvers Actually Learn*

**Headline claim (expected outcome, "memory wins").**
> We studied three families of self-referential improvers: meta-skill evolvers, self-evolving harness optimizers, and Gödel-Agent-style code self-modifiers. In all three, most of a later-generation improver's measured advantage at matched budget comes from its inherited memory (archive, notes, scored history), not from a better procedure. Transplanting generation-k memory into the generation-0 procedure recovers most of the gain. The memory-wiped generation-k procedure keeps little. What procedure advantage remains disappears once domain content is scrubbed from the evolved procedure, or when the improver is moved to a fresh domain. So far, "learning to improve" is learning the task distribution.

**Alternative claim (also publishable, "procedure wins").**
> A procedure share that survives knowledge scrubbing and a domain change, and grows with generation, would be the first controlled evidence of *procedural meta-learning* in LLM improvers. We would report the generation k* at which it appears and the edit types that carry it (verification, selection, budget-allocation rules).

**Third possible outcome ("interaction wins"), also publishable.**
> If the gain is mostly interaction (Pk only pays off when it has Mk, and the format adapter does not rescue P0+Mk), then the improver has learned to *use its own memory*. That is a third kind of meta-learning, distinct from both a "better procedure" and "more knowledge", and it has direct consequences for how memory and harness transfer should be designed.

Deliverable regardless of outcome: the **Transplant Test**, a pre-registered eight-arm reporting control with a small open library, for any "our improver got better at improving" claim.

---

## 2. Motivation and why now

- **Claims that "improvers improve" have outrun their controls.** Gödel Agent, DGM/DGM-H, Hyperagents, Metaⁿ, AIDE², MetaSkill-Evolve, Skill∞, VERSE, RSIFlow and Harness² all report that editing the improver helps. The October 2026 census *Auditing the Product, Not the Recursion* (2610.27610) finds that only **4 of 26** improver-gain claims compare an earlier and a later improver from identical starting artifacts at matched budget. Even those four do not ask *why* the later improver is better.
- **Every improver carries state.** Hyperagents' transfer result (imp@50 = 0.630) moves the entire hyperagent, including its memory and tracking infrastructure. VERSE's optimizer edits five slots: prompts, skills, tools, hooks and **notes**. MetaSkill-Evolve's meta-skill reads a SQLite evolution graph through a Retriever. Skill∞'s meta agent reads an archive. A later improver therefore differs from an earlier one in two ways at once: its *procedure* and its *accumulated knowledge*. No paper crosses the two.
- **2026 work points both ways, which makes the question live.**
  - *Memory side.* *Lineage Is Memory* (2610.41157; 1,750 pre-registered runs) shows that optimizer progress comes mostly from inherited task facts. *Continual AutoResearch* (2611.05108) finds that persistent memory speeds up initialization but does not improve final outcomes. *Rethinking the Evaluation of Harness Evolution* (2607.12227) shows that at matched budget, evolution (67.4) loses to parallel sampling (72.3).
  - *Procedure side.* VERSE reports that an evolved optimizer harness helps after a domain change (SWE→Terminal-Bench) even when the executor harness is reset, and concludes that "what crosses domains appears to be the optimizer's methodology, its skills, notes, and tools". Notes are memory, so this conclusion is confounded. HMED (2610.07979) and Skill∞ (2610.35434) report meta-skills that stay useful after the task-skill library is reset.
- **The question is now being asked publicly, but nobody has run it.**
  - The 2026-11-04 survey *When, Where, and How Experience Becomes Structure* says that "reuse across cycles and improvement of the update method are separate questions".
  - *Toward RSI in Physical AI* (2610.29677) designs a frozen-knowledge control (R1/R2) but runs no experiment.
  - The Auditing paper proposes a swap test of improvers × solver states.
  - We are the first empirical run of these proposals, and we add the missing off-diagonal arm (old procedure + new memory), placebo controls, a scrubbing control and cross-domain arbitration.
- **Agent Plasticity** (2610.08902) gives the vocabulary. Plasticity is the held-out gain per unit learning cost for a fixed learner. For an improver, the procedure share asks whether the *conversion rate* got better, and the memory share asks whether the *stock of experience* grew. Dan wants to carry plasticity up to the meta level, and this paper does that cleanly.

---

## 3. Core research questions

1. **RQ1 (decomposition).** At matched budget, how much of a later improver's advantage (Ik − I0) is procedure, how much is memory, and how much is interaction? Does the answer hold across three improver families?
2. **RQ2 (what "procedure" really holds).** How much of the apparent procedure effect is *domain knowledge written into the evolved code or prompts*? We measure it with a knowledge-scrubbed procedure and a leak audit.
3. **RQ3 (transfer as arbiter).** On a fresh domain B, does Pk with empty memory beat P0 with empty memory? Does A-memory help, hurt, or do nothing on B?
4. **RQ4 (dynamics).** How do the procedure share, memory share and interaction change over generations k ∈ {0, 5, 10, 20, 40}? Does procedural meta-learning appear late, never, or only in some families? What is the "memory exchange rate", meaning how much extra budget I0 needs to match a memory transplant?

---

## 4. Related work and positioning

| Paper (ID, date) | What it does | Gap it leaves / how we differ |
|---|---|---|
| **HMED** *Learning from Revision Consequences* (2610.07979, Oct 6) | Writes the branch outcome as f(x, m), where x is the discovery state (skill library and history) and m is the meta-skill. Re-runs the incumbent and revised meta-skills from the same restored x and uses the difference as a **training signal**. Table 4 freezes m, resets the task-skill library and re-runs discovery on new origins (HMED 50.4 vs MetaSkill-Evolve 49.3 on ALFWorld/Qwen3-32B, 3 runs). | **This is the closest conceptual neighbour, found in this pass.** Their reuse test is just our Pk+∅ cell, with no P0+Mk cell, no budget matching against the seed meta-skill, no placebo or scrub, and no cross-domain test. Matched x is used to *select* revisions, not to *decompose* an improver's gain. We adopt their f(x, m) notation and cite them as the method-side motivation. |
| **VERSE** (2610.02616, Oct 2; MIT/Amazon; code released) | A self-evolving harness optimizer whose own workspace has five editable slots: prompts, skills, tools, hooks, notes. Cross-domain (SWE↔Terminal-Bench) tests with warm and cold executor starts show that the inherited optimizer harness helps. | Notes (memory) and procedure (prompts/skills/tools/hooks) are always transplanted **together**. Its "methodology crosses domains" conclusion is exactly the claim our test can confirm or refute. Its slot design gives us a natural, separable family-(b) testbed. |
| **Skill∞** (2610.35434, Oct 26) | A meta-skill (SKILL.md, references, archive script) self-improves on skill-optimization tasks and is reused across 8 SkillsBench domains: 55.9 → 60.7 (5 runs, GPT-6 Luna). | The meta-skill has **no separate lessons store**, so accumulated knowledge can only live *inside the procedure*. That is exactly the knowledge-in-code confound, measurable only with our scrub arm. No transplant, placebo or CI. It needs a frontier model, so we re-implement its loop on open models instead of decomposing its numbers. |
| **MetaSkill-Evolve** (2607.05297, Jul 6) | Two-timescale co-evolution of a task skill and a five-file meta-skill (ψ, σ, α, π, ε) on Gemma-4 31B. Ablating meta-updates costs −6.4/−8.1 pts on OfficeQA/SealQA. | The meta-skill reads a cross-branch evolution graph through σ (memory). Its "no meta-updates" ablation removes *both* procedure evolution and the meta-level state. Our family-(a) testbed, which runs on open weights. |
| **Toward RSI in Physical AI** (2610.29677, Oct 21) | A position paper with formal R1/R2 recursion tests and a frozen-knowledge control (I^{K1} vs I^{K0} on held-out failure families). | Design only. It has no off-diagonal arm, no placebo or scrub, and no runs. We are its first digital instantiation. |
| **Auditing the Product, Not the Recursion** (2610.27610, Oct 20) | Census of 114 RSI studies: 4 of 26 improver claims use same-start, matched-budget comparisons. Proposes a swap test (improver × solver state). | A meta-analysis. Its swap crosses the improver with the *solver* state. We cross the improver's procedure with *its own* memory. We adopt its reporting standard. |
| **When, Where, and How Experience Becomes Structure** (alphaXiv `2611.recursive-self-improvement-llm-agents`, Nov 4) | Survey. Its evaluation agenda asks whether inherited state helps future updates under matched budgets, and separates reuse from improvement of the update method. | No experiments. We cite it as the field's statement of the open question. |
| **Lineage Is Memory** (2610.41157, Oct 30) | Pre-registered: parent inheritance (task facts) drives most of a GEPA-style optimizer's progress (+0.24). | Task-artifact level; the optimizer procedure never evolves. It is our prior for "memory wins", and we follow its pre-registration style. |
| **Continual AutoResearch** (2611.05108, Nov 3) | Stateful vs. reset research agents with matched, shuffled and no memory. | Actor level with a fixed procedure. We copy its shuffled-memory control and lift it one level, to the improver. |
| **Flex-RSI** (2611.08946, Nov 6) | Benchmark: paired frozen vs. self-improving harness, where the improver edits its own instructions and "accumulated lessons". Includes a cross-model harness transplant. | Lessons and procedure are edited and moved together. |
| **Agent Plasticity** (2610.08902, Oct 6; Meta) | Plasticity = held-out gain per $ for fixed-weight agents inheriting tools, skills and memory. Diagnoses absent, missed and used artifacts. | Actor level, with the learning interface held fixed. We define improver plasticity and split it into stored experience (memory) and conversion efficiency (procedure). |
| **Hyperagents / DGM-H** (2603.19461) | imp@k; meta-transfer 0.630; compounding 0.640 vs 0.610 n.s. | The transfer moves memory and procedure together and has not been replicated. Family (c) is a small-scale DGM-H/Gödel-Agent analogue. |

Also seen; I read only the abstracts, so these are less close:
- *Beneath the Surface* 2611.07624 (task-agent-level replays);
- *Harness-Delta Attribution* 2610.22466;
- *Does Failure Feedback Direct Agent Self-Modification?* 2610.14662;
- *One Recipe, Many Harnesses: What Self-Evolution Encodes* 2608.10178 (relevant to RQ2);
- *Bad Genius* 2609.18366 (task-specific shortcuts in harnesses; relevant to the scrub arm);
- RSIFlow 2610.21791; Harness² 2610.14224; Meta² 2611.05139;
- the second Physical-AI RSI survey 2610.38266.

**How we differ, in one sentence.** Prior work either evolves the improver and transplants it whole (VERSE, Skill∞, Hyperagents, Flex-RSI), fixes the procedure and ablates memory (Lineage, Continual AutoResearch, Plasticity), or uses matched-state comparisons as a training signal (HMED). We are the first to **cross** an evolved improver's procedure with its own memory, adding placebo, scrub and adapter controls, matched budget, cross-domain arbitration and a generation trajectory, across three improver families.

**Scoop status as of 2026-11-12.** No paper runs the {P0, Pk} × {∅, Mk} crossing. HMED (Table 4) and VERSE (Table 9) each ran one cell of it. This raises the scoop risk compared with the critique: expect a direct scoop within about 3–5 months. The most likely sources are the VERSE authors (they already have the slots), the HMED group, RSIHub/BIGAI, or the Auditing authors. Hence the February preprint (§8).

---

## 5. Method and experimental design

### 5.1 Formal setup

- An **improver** is I = (P, M).
  - **P (procedure)** is everything the improver *executes as instructions*: its source code, system prompts, meta-skill files, tools and hooks.
  - **M (memory)** is *all improver-readable state outside P*: the archive of candidate artifacts and their scores, notes and lessons files, logs, meta-experience records and the evolution graph.
  - This is an operational partition, fixed per family before any data is seen (§5.3), and both directions of leakage are measured (§5.4).
- An **improvement run** R(P, M; D, A0, B, s) starts from a fixed seed artifact A0 (a task skill or task-agent harness) on domain D. It may use B units of budget: a token cap plus a fixed number of candidate evaluations on D's training split. Tokens spent reading memory count toward B. The run returns a final artifact A*, and s is the run seed.
- **Outcome.** Y = Score(A*; D_test) − Score(A0; D_test), on a held-out test pool that the improver never sees. Both terms are scored on the same tasks with the same trial seeds (paired).
- **Cell means.** Y_{pm} = E[Y | P = P_p, M = M_m], with p ∈ {0, k} and m ∈ {∅, k}.
- **Estimands.** All are pre-registered, with hierarchical bootstrap CIs that resample lineages, then runs, then tasks.
  - Total improver gain: **G = Y_kk − Y_00**
  - Memory effect at the old procedure: **μ = Y_0k − Y_00**
  - Procedure effect without memory: **π = Y_k∅ − Y_00** (written Y_k0 below)
  - Interaction: **ι = G − μ − π = Y_kk − Y_k0 − Y_0k + Y_00**
  - Knowledge-in-procedure: **κ = Y_k0 − Y_{k^s 0}**, where P_k^s is the scrubbed procedure. Pure procedural effect: **π_s = Y_{k^s 0} − Y_00**.
  - Cross-domain procedure effect on fresh domain B: **π_B = Y^B_k0 − Y^B_00**. Cross-domain memory effect: **μ_{A→B} = Y^B_{0,M^A_k} − Y^B_00**, which can be negative.
  - Shares are reported only if G's CI excludes 0: memory share μ/G, procedure share π/G, and pure-procedure share π_s/G. The 2-player Shapley values (μ + ι/2 and π + ι/2) go in the appendix. As the critique notes, with two players Shapley is just the 2x2 with the interaction split evenly, so **we report μ, π and ι separately** and treat ι as a result in its own right.
  - **Memory exchange rate** b*: the extra budget for which Y(P0, ∅; B + b*) = Y_0k. It is read off a budget curve for P0 at B × {1, 1.5, 2, 3, 4}, and says how much compute the memory is "worth".
  - **Improver plasticity** (following Agent Plasticity): Π_k = (Y_kk − Y_00) / C_k, where C_k is the cost of producing generation k. Its procedure and memory parts are π/C_k and μ/C_k.

### 5.2 Arms, evaluated per family at checkpoint k

| # | Arm | Procedure | Memory | Purpose |
|---|---|---|---|---|
| 1 | Base | P0 | ∅ | Y_00 |
| 2 | Full later improver | Pk | Mk | Y_kk |
| 3 | Memory-wiped | Pk | ∅ | π |
| 4 | **Transplant (off-diagonal)** | P0 | Mk | μ, the arm R1 lacks |
| 5 | **Format-adapted transplant** | P0 | Ad(Mk) | Rules out an interface confound. An LLM re-renders Mk into P0's native memory schema; the adapter is validated by round-trip QA. |
| 6 | **Knowledge-scrubbed** | Pk^s | ∅ (and Mk) | κ and π_s |
| 7 | **Length-matched placebo** | P0 | M_plac | Rules out "more context helps". Same format and token count; taken from an equal-generation lineage on an unrelated domain. |
| 8 | **Shuffled memory** | P0 and Pk | M_shuf | Rules out "memory as generic exemplars". Scores are permuted across archive entries and lessons are detached from their failure contexts. Tokens are kept; the content–outcome link is broken. |
| — | Dose (appendix) | P0 | 0/25/50/100% of Mk, chronological vs. random | Shows how memory value accumulates |
| — | Scrub control | P0^s | ∅ | Checks that the scrubber does not damage a generic procedure (expect Y ≈ Y_00) |
| — | Memory abstraction (ablation) | P0 | Abs(Mk) | Task facts removed, edit-type statistics kept. Separates *procedural information stored in memory* from *task facts*. |

**Cross-domain block (RQ3, the arbiter).**
- Ik is trained on domain A and evaluated on fresh domain B.
- Arms: P0+∅, Pk+∅, Pk^s+∅, P0+M^A_k, Pk+M^A_k, and P0+M^B_native.
- M^B_native is produced by running I0 on B's *training* split for a budget matched to the cost of k generations.
- Procedural meta-learning is claimed **only if** π_B > 0 and π_s > 0, with their CIs excluding 0 at the pre-registered SESOI (§5.6).

**Compute-matched baselines** (following 2607.12227 and the Hyperagents protocol):
- (i) frozen improver P0+∅ given Ik's *total* lineage cost as extra budget;
- (ii) parallel sampling: best-of-N candidates from P0 with the same number of evaluations;
- (iii) random-restart lineage: k generations in which the improver edits are replaced by no-op edits of matched length. This is a "generation placebo" that keeps the archive growth but not the procedure edits.

### 5.3 Improver families (fixed before data)

| Family | Implementation | P | M | Cost driver |
|---|---|---|---|---|
| **(a) Meta-skill evolver** | Re-implementation of MetaSkill-Evolve/Skill∞-lite: an agent-led loop following a meta-skill package, which self-improves on skill-optimization tasks | SKILL.md, references and scripts of the meta-skill | Evolution graph, archive, decision logs, HMED-style meta-experience records | Task episodes |
| **(b) Self-evolving harness optimizer** | VERSE-style optimizer workspace (released code), scaled down to cheap domains | Prompt files, skills directory, tools/hooks Python file | `notes.md` and the audit/training-history files | Task episodes |
| **(c) Self-referential code modifier** | Gödel-Agent/DGM-H-lite: one Python program holding a task agent and a meta agent; the meta agent may rewrite both | Meta-agent source and prompts | Archive of agent variants, scores and logs | Improver tokens and episodes |

Family (c) is Xunjian's home turf and reuses **C1's checkpoints**, where C1 is the improver-scaling study (idea A1). Families (a) and (b) have naturally separable memory, so they protect us if family (c)'s partition looks arbitrary to reviewers.

### 5.4 Knowledge-scrubbing protocol and leak audit (fix 1)

1. **Scrubber.** A fixed-prompt LLM, different from the improver model, rewrites Pk:
   - it removes domain entities, task-specific heuristics, few-shot examples and hard-coded constants that refer to tasks;
   - it keeps control flow, tool signatures and generic rules (for example "verify a draft on 2 failing and 1 passing task before submitting").
   The diff is logged.
2. **Leak audit.** A scrub passes only if all three checks hold:
   - (i) a domain classifier trained on procedure texts from A and B lineages has AUROC ≤ 0.6 on the scrubbed texts;
   - (ii) a "knowledge probe" (an LLM answers 30 domain-fact questions from the procedure text alone) scores ≤ the P0 baseline + 1 SE;
   - (iii) Dan and one labmate hand-audit 20 random scrubs, with Cohen's κ reported.
3. **Sensitivity.** We vary scrub aggressiveness (light, medium, heavy) and report κ as a curve, not a point. Heavy scrubbing that also hurts P0^s marks the upper bound.

### 5.5 Domains, models and pools

- **Domains.**
  - **A = ALFWorld**: 3,553 training games and 274 held-out evaluation games (seen plus unseen splits). Used by HMED and MetaSkill-Evolve, open source, and free apart from LLM tokens.
  - **B = ScienceWorld**: 30 task types with thousands of parametric variations, so held-out pools can be drawn freely.
  - **C = procedural τ-style tasks** from Dan's planned generator (DB state + goal + policy rule, with the end state verified). It gives an unlimited held-out pool and a controllable domain shift (a new schema or policy family). If the generator slips, C is OfficeQA (used by MetaSkill-Evolve).
  - **Pairings:** A→A and C→C in-domain; A→B and C→B cross-domain; B→A for symmetry on one family.
- **Models.**
  - Primary: **Qwen3.5-9B** for task agent and improver in the pilot and the bulk runs, and **Qwen3.5-27B** for the main results. Served by a cheap API or local vLLM.
  - Robustness: a **Haiku-class** model as improver on family (a) only, k = Kmax.
  - The task-agent model is the same across all arms within a family. The improver model is fixed within a lineage.
- **Lineages.** 3 independent lineages per family and domain. One lineage is one draw of (Pk, Mk), so lineage-to-lineage variance is part of the CI. Generations run up to 40 for family (c) (from C1) and up to 20 for (a) and (b).
- **Run seeds.** 3 improvement runs per (arm, lineage), so 9 runs per cell. Test evaluation uses 150–200 held-out tasks × 2 trials, with the task set and trial seeds shared across arms (common random numbers).

### 5.6 Power sketch and pre-registered interpretation rules

- **Assumptions** (pilot-checked). Between-run SD of Y is σ ≈ 4 pp, consistent with the ±1–3 pp SEMs and seed spreads in VERSE, HMED and Skill∞. Paired correlation between arms sharing lineage, tasks and seeds is ρ ≈ 0.5, so SD of a paired difference ≈ 4 pp.
- **Detectable effects.** With 9 runs per cell, an arm difference of ≈ 4 pp is detectable at α = 0.05 with 80% power (n ≈ (2.8σ_d/Δ)² ≈ 8). Interaction contrasts have about 2× the variance, so the MDE for ι is ≈ 5.5 pp.
- **Consequence.** Shares are interpretable only when G ≳ 8 pp. MetaSkill-Evolve's meta-gain was +6–8 pp and VERSE's was +3–8 pp, which is borderline. Where G is smaller, we report effects in pp with CIs, **not** shares.
- **SESOI.** δ = max(2 pp, 0.5 × SD of Y_00 across seeds), fixed after the pilot and before the main runs.
- **Interpretation rules** (written into the pre-registration; no 70/30 cut-offs):
  - **Memory-dominant:** μ > δ (CI) **and** π_s is equivalent to 0 under TOST at ±δ.
  - **Procedural meta-learning:** π_s > δ **and** π_B > δ (CIs excluding 0).
  - **Interaction-dominant:** ι > δ **and** neither main effect > δ **and** the adapter arm does not close the gap (Y_{0,Ad(Mk)} − Y_0k < δ).
  - **Knowledge-in-code:** κ > δ.
  - **Inconclusive:** otherwise, reported as such, with the MDE.
- **Gate (critique weakness 3).** If G is not > δ at matched budget, the decomposition is not run as headline. The paper then reports:
  - (i) the null on G across families, itself a contribution in the 2607.12227 style;
  - (ii) μ, the value of the transplanted memory to I0, which can be positive even when G is null;
  - (iii) the exchange rate b*.

### 5.7 Metrics recorded per run

- Y on in-distribution and OOD test pools.
- Training-split score of A*, to measure the train–test gap (Goodhart).
- Tokens and $ spent, broken into memory reads vs. generation.
- Number of accepted edits.
- Memory-use telemetry: which entries were read and cited in proposals, following Plasticity's absent/missed/used split.
- An edit-type taxonomy of Pk − P0 diffs: verification, selection, budget, diagnosis, domain content. This links the procedure share to *which* procedural edits carry it.

---

## 6. Expected results and figures

- **Figure 1, the Transplant Test.**
  - Left: a schematic of the eight arms. Procedure and memory are drawn as two separable blocks that can be swapped, scrubbed or adapted, with the cross-domain arbiter on the right.
  - Right: per family, stacked bars of G split into μ, π_s, κ (knowledge-in-code) and ι, in-domain vs. cross-domain, with 95% CIs.
  - Expected pattern: in-domain, μ ≈ 50–80% of G, κ eats most of π, and π_s ≈ 0. Cross-domain, μ_{A→B} ≤ 0 and π_B ≈ 0 for families (a) and (c). Family (b) might show a small positive π_B through verification tools, which would be the VERSE effect surviving without notes.
- **Figure 2, the key plot.** Procedure share π_s/G and memory share μ/G against generation k ∈ {0, 5, 10, 20, 40}, one panel per family, in-domain and cross-domain lines, with the memory exchange rate b* on a secondary axis. Either the procedure share stays flat at ~0, meaning no meta-learning at any horizon tested, or it rises after some k*, meaning late-emerging procedural meta-learning. Both are headline results.
- **Figure 3.** Dose curves (chronological vs. random) and placebo/shuffled arms. Expected: shuffled ≈ placebo ≈ base, so memory helps through *content tied to outcomes*, not through length or exemplars.
- **Figure 4.** Edit-type attribution: which procedure-diff categories survive scrubbing. Expected survivors are verification and selection rules, which matches VERSE's and Skill∞'s qualitative case studies.
- **Table.** The Transplant Test applied as a reporting audit: our numbers next to the improver claims in Hyperagents, VERSE, Skill∞ and MetaSkill-Evolve, showing which arms each paper ran (mostly 1–2 of 8).

---

## 7. Pilot (≤ 1 week, ≤ $50) and decision criteria

**Pilot setup.**
- Family (a)-lite on ALFWorld. Qwen3.5-9B for both roles; ≈ 5–8M tokens per run at a small-model API price, so roughly $1 per run, but the price is to be checked.
- 1 lineage to k = 10 at about $8.
- Arms 1–5 and 7 with 3 run seeds each, using 100 held-out games × 1 trial: about 18 runs at $1–1.5 each, so about $25.
- Scrubber and leak-audit dry run on Pk: about $2.
- Noise floor: 5 extra seeds of arm 1, about $6.
- **Total ≈ $40.**

**Checks and decisions.**

| Check | Go | Pivot | Kill |
|---|---|---|---|
| Lineage produces any G at matched budget (Y_kk − Y_00) | ≥ 2 × SE of Y_00 | G ≈ 0: move to the C1 null story plus μ-only result; try Qwen3.5-27B or family (b) | G ≈ 0 for both 9B and 27B on two families → fold into C1 as a section |
| Noise: SD of Y_00 across 5 seeds | ≤ 5 pp | 5–8 pp: raise to 200 tasks × 2 trials, ~+60% cost | > 8 pp and not fixable by pool size |
| Transplant feasible: P0 reads Mk without crashes; the adapter's round-trip QA ≥ 90% | yes | adapter needed for all transplants (fine) | — |
| Scrub leak audit passes at medium aggressiveness | AUROC ≤ 0.6 | only heavy scrub passes: report the κ curve | scrub destroys P0^s (> δ drop) with no usable setting |
| Scoop watch (weekly alphaXiv/S2 alert on "transplant", "improver memory", "procedure vs memory") | none | a paper runs the 2x2 on one family → reposition as multi-family + trajectory + cross-domain | a paper runs the 2x2 + cross-domain + scrub on ≥ 2 families |

---

## 8. Budget and timeline (to mid-May 2027)

**Budget** (API $, estimates; local vLLM on lab GPUs lowers this):

| Item | Runs | $ |
|---|---|---|
| Pilot | ~25 | 50 |
| Lineage generation: 3 families × 3 lineages × 2 domains (family c reuses C1) | 12 new lineages | 250 |
| Core arms at k = Kmax: ~14 arm cells × 9 runs × 3 families | ~380 | 650 |
| Generation trajectory, family (a) and (c): 4 core arms × 4 extra k × 9 | ~290 | 400 |
| Dose, abstraction, budget curves for b* | ~120 | 150 |
| Haiku-class robustness (family a) | ~40 | 200 |
| Scrubber, leak probes, adapters, judges | — | 100 |
| Contingency (re-runs, 27B upgrade) | — | 300 |
| **Total** | | **≈ $2.1k** (floor ≈ $1.3k if 9B suffices) |

**Timeline.**
- **Nov 12–30, 2026.** Write and pre-register estimands, SESOI rule and scrub protocol. Implement the family (a)-lite loop and the run harness (common random numbers, budget accounting). Run the pilot. Go/pivot decision by **Nov 30**.
- **December.** Family (a) and (b) lineages on ALFWorld (A) and ScienceWorld (B). Build the arm runner (P/M swap, adapter, placebo, shuffle). Agree the C1 checkpoint format with family (c).
- **January 2027.** Core in-domain and cross-domain arms for families (a) and (b). Scrubber with leak audit. First full Figure 1.
- **February.** **arXiv preprint v1** (families a and b, in-domain and cross-domain, scrub) to stake the claim. Start family (c) on C1 checkpoints.
- **March.** Generation trajectory (k ∈ {0, 5, 10, 20, 40}). Domain C (procedural τ-generator, or OfficeQA). Dose, abstraction and b* curves.
- **April.** Haiku-class robustness. Reporting-audit table over published improvers. Release the Transplant Test library. Full draft by **Apr 20**, then internal review with Xunjian and Shuyan.
- **May 1–15.** Polish, appendix (pre-registration, deviations log), submit.

---

## 9. Risks and mitigations; anticipated reviewer objections

**Risks.**

| Risk | Likelihood | Mitigation |
|---|---|---|
| No significant G on open models (Hyperagents compounding n.s.; AIDE² inconclusive) | Medium-high | Gate in §5.6; report μ and b* anyway; three families raise the odds that one shows G. The null on G is itself C1's finding. |
| Interaction dominates and shares are uninterpretable | Medium | Pre-registered "interaction-dominant" reading; adapter arm separates "can't read the format" from "learned to use memory"; memory telemetry shows *how* Pk uses Mk differently. |
| Scrubber is itself a confound (removes procedure too) | Medium | P0^s control; aggressiveness curve; human audit; report κ as a range. |
| Partition is contestable in family (c) | Medium | Families (a) and (b) have built-in slots. Sensitivity analysis moving borderline files (e.g. a "tracking" helper) between P and M. |
| Scoop within 3–5 months | Medium-high | February preprint; the multi-family, trajectory and scrub combination is hard to replicate quickly. |
| Open models can't drive DGM-H-style code self-modification | Medium | Family (c) uses Gödel-Agent-lite with constrained edit APIs. If it fails, use Qwen3.5-27B for the improver only, or drop (c) to an appendix and keep (a) and (b). |
| Cost overrun from long ALFWorld episodes | Low-medium | Step caps; 9B for bulk; local vLLM. |

**Anticipated reviewer objections, with rebuttals.**

1. *"This is an ablation of Hyperagents/VERSE."*
   - It covers three families, two cross-domain pairs and a 40-generation trajectory, and releases a protocol.
   - The headline is a cross-family regularity (or its absence), not a component ablation of one system.
   - The audit table shows that no published improver claim ran more than two of the eight arms.
2. *"The P/M split is arbitrary."*
   - It is operational and fixed before data.
   - Leakage in both directions is measured: knowledge in P by scrubbing (κ), procedure in M by the abstraction arm.
   - Results are reported under alternative partitions.
3. *"HMED already does matched-state comparisons, and VERSE already shows that methodology transfers."*
   - HMED uses f(x, m) differences to *train* a meta-skill and ran only the Pk+∅ reuse cell.
   - VERSE transplants notes together with procedure.
   - Neither has the off-diagonal arm, matched budget against the seed improver, placebo or scrub controls, or a trajectory. Our test is exactly what can confirm or refute VERSE's "methodology crosses domains" claim.
4. *"Shapley with two players is trivial."* Agreed. We report μ, π, ι and κ directly; Shapley appears only in the appendix.
5. *"Open 9B/27B improvers are too weak; frontier improvers might really meta-learn."*
   - We include a Haiku-class replication and report the trend with improver strength.
   - We frame the claim as "so far, at this scale".
   - The protocol is the artifact frontier labs can run.
6. *"Placebo memory from another domain is not neutral; it might mislead."* This is why we have both a length-matched placebo and a shuffled same-domain arm. Harm from foreign memory is reported as μ_{A→B}.
7. *"Matched budget is unfair to memory-reading improvers."* We report both conventions: memory-read tokens inside the budget (main) and outside it (appendix). We also give b* so readers can convert between them.

---

## 10. Fit with Xunjian's agenda, and what the mentors bring

- **Gödel Agent (2410.04444).** This paper answers the question Gödel Agent raises but does not test: *when a self-referential agent rewrites its own logic, is the rewritten logic the source of later gains, or the experience it has carried along?* Family (c) is a Gödel-Agent descendant, and the "complexity ceiling" Gödel Agent observed can be re-read through κ: evolved code fills up with task knowledge rather than procedure.
- **WeAct (2610.26847).** WeAct treats weight switching as an action, separating "which knowledge store is active" from "the policy that chooses it". Our P/M split is the harness-level analogue. A natural follow-up is to transplant memory *into weights* (distil Mk into a LoRA and cross it with P0/Pk), which connects to cluster 3 of Dan's ideas.
- **BreakingWeb (2609.35814).** The same philosophy: *controlled interventions* on the environment show what an agent really relies on. Here the intervention is on the improver's own state. The leak audit and placebo design follow the same "construct the counterfactual" approach.
- **World models.** Memory Mk can be read as the improver's *world model of the task distribution*: which edits work where, and what the verifier rewards. P is its planning procedure. "Memory wins" then means current improvers gain by learning a better world model of the benchmark, not a better planner. That framing fits Xunjian's self-referential agents + world models agenda.
- **What Xunjian brings.** Gödel Agent code and intuitions for family (c); judgment on which partition reviewers will accept; DeepMind exposure to how frontier groups would run this test; and positioning against the Hyperagents, DGM and VERSE crowd.
- **What Shuyan Zhou brings.** Benchmark and evaluation rigour from WebArena-style work: held-out pool design, the procedural τ-generator (domain C), and reporting standards. Bhuwan Dhingra's knowledge and memorization background is directly relevant to the scrub and leak-audit design (κ is a memorization measurement).

---

## 11. Rubric re-score after the fixes (honest)

| Dim | Old | New | One-line justification |
|---|---|---|---|
| N (×3) | 3.0 | **2.5** | The crossing is still unrun, but this pass found HMED (Pk+∅ reuse cell; f(x, m) framing) and VERSE (procedure and notes transplanted cross-domain) on top of the four Oct–Nov papers the critique flagged. Novelty now rests on the off-diagonal, scrub and cross-domain arms, the multiple families and the trajectory, not on the question itself. |
| S (×2.5) | 3.5 | **4.0** | Three pre-registered outcomes (memory, procedure, interaction) are all publishable. The scrub arm and cross-domain arbiter turn the partition weakness into a measured finding (κ). Risk remains that G is too small to decompose. |
| R (×2) | 5.0 | **5.0** | This is the core Gödel Agent question, and it connects to WeAct, BreakingWeb and world models. |
| C (×2) | 3.0 | **4.0** | Three families, matched budget, placebo/shuffle/adapter/scrub arms, hierarchical CIs, pre-registered SESOI rules and a reusable protocol. Not 5, because family (c)'s partition and open-model improver strength remain attackable. |
| F (×1.5) | 3.5 | **3.0** | Pilot ≈ $40 in a week, but the full study is ≈ $1.3–2.1k with three re-implemented improver families. Reproducing working gen-k improvers on open models is the real risk. |
| T (×1) | 4.5 | **4.5** | RSI-evaluation rigour is the hottest Oct–Nov 2026 sub-trend, and we are slightly ahead of it. The window is about 3–5 months. |

**Total = 20 × (3·2.5 + 2.5·4.0 + 2·5.0 + 2·4.0 + 1.5·3.0 + 1·4.5) / 12 = 20 × 44.5 / 12 ≈ 74.2.** Viable with fixes, at the top of that band.

**Recommendation.** Ship it as the causal core of C1 (improver scaling), not as a standalone paper. C1 supplies the effect (or the null) and this supplies the cause. Post a preprint of families (a) and (b) in February 2027. Kill it if a paper appears that runs the crossing with cross-domain and scrub arms on at least two families.

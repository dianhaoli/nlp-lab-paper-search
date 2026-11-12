# Critique: Procedure or Memory? Transplant controls for self-referential improvers

Idea id: `procedure-vs-memory` · Topic T1 (improver scaling and meta-level ability) · Dan-original · Reviewed 2026-11-12

**Verdict: viable with fixes (72.5/100).** Nobody has run the improver-level 2x2 transplant yet. But in Oct–Nov 2026 at least four papers posed the question, proposed this control, or ran its actor-level analogue. The window is open but closing. As written, the partition between "procedure" and "memory" is not sound, and the 70%/30% thresholds are arbitrary. The decomposition only means something if C1 first shows a significant improver gain.

---

## 1. Scoop search

Searches run:
- alphaXiv discover (recency), 4 calls:
  - procedure vs. memory/archive in self-referential improvers;
  - "Experience Becomes Structure" / Lineage Is Memory;
  - "does the meta-agent learn to improve or memorize";
  - SEAR / faithful self-evolvers / artifact-to-capability.
- S2 keyword search, 4 phrasings (improver memory vs procedure ablation; meta-agent archive transplant; disentangling accumulated experience from an improved improver; Hyperagents meta-level transfer controls).
- S2 forward citations of Hyperagents 2603.19461 (60 citing papers retrieved), filtered for memory/archive/procedure + control/ablation.

Deep reads, via full text or targeted PDF queries:
- 2611.recursive-self-improvement-llm-agents
- 2611.07624
- 2610.41157
- 2611.05108
- 2610.35434
- 2610.29677
- 2610.27610
- 2611.08946

### Closest work, and the gap each leaves

| Paper | Date | What it actually does | Gap left for us |
|---|---|---|---|
| **When, Where, and How Experience Becomes Structure** (Dai, Ruan et al.; alphaXiv-only id `2611.recursive-self-improvement-llm-agents`) | 2026-11-04 | **Survey** of Prompt/Memory/Skill/Harness update loops. Its evaluation agenda explicitly asks: "does inherited state help produce more effective future updates under comparable experience, feedback, and computational budgets?" It also says "reuse across cycles and improvement of the update method are also separate questions" (Harness² library vs. VERSE improver edits). | No experiments. It confirms the question is open, and it also shows the question is being asked publicly. Cite it as motivation. |
| **Toward RSI in Physical AI** 2610.29677 | 2026-10-21 | Position/survey. Formal R1/R2 links. R1 uses a **frozen-knowledge control** (I^{K1} vs I^{K0} on new failures). R2 compares self-revision with acquired vs. frozen knowledge. It notes that freezing is easy when knowledge sits in a memory snapshot and that held-out failure families are needed "otherwise the comparison measures memorization". | Design only. It says no physical study isolates R1/R2 and that "digital precedents run close analogues", but it does not run a procedure x memory factorial. Our 2x2 is the digital instantiation of its R1 control, plus the off-diagonal arm it lacks (old procedure + new memory). |
| **Auditing the Product, Not the Recursion** 2610.27610 | 2026-10-20 | Census of 114 RSI studies. Only **4 of 26** studies that claim a better improver compare an earlier and a later improver from identical starting artifacts at matched budget. It proposes a **swap test** (improvers assigned to solver states) and says frozen-vs-evolving arms "visit different solver states". | It is a meta-analysis. Its swap crosses improver x solver state, not improver procedure x improver memory. Good for citing the gap and the reporting standard; it also partly anticipates the crossing design. |
| **Continual AutoResearch** 2611.05108 | 2026-11-03 | Stateful vs. reset research agents (5 frontier CLIs). The **memory ablation uses matched, shuffled and no insight cards**. Persistence speeds up initialization but not final outcome. | Actor-level, not improver-level. No procedure dimension: the agent code is fixed. Our matched/shuffled memory arms copy their design, so cite them as the source of those controls. |
| **Lineage Is Memory** 2610.41157 | 2026-10-30 | 1,750 pre-registered runs on GEPA-style optimizers. Parent inheritance vs. seed-reset x feedback disclosure. Lineage gives +0.24 held-out, and the gains come from accumulating task facts (the "carry-facts" result). | Task-artifact level: the prompt carries the knowledge. The improver procedure is never evolved. Still, it strongly supports the "memory" hypothesis: accumulated task facts drive optimizer progress. Use it as the prior. |
| **Skill∞** 2610.35434 | 2026-10-26 | A meta-skill (the optimizer procedure) self-improves on separate skill-optimization tasks, then is reused across domains. Seed vs. refined meta-skill: 55.9 → 60.7 on SkillsBench (5 runs). | This is the closest thing to evidence of *procedure* transfer, because the meta-skill is decoupled from task skills. But the meta-skill bundles references/lessons with the procedure. There is no transplant, placebo or length control, and no CI on the +4.8. It is an ideal **testbed** for this idea. |
| **Flex-RSI** 2611.08946 | 2026-11-06 | Benchmark. Paired frozen- vs. self-improving harness (the improver also edits its own instructions and "accumulated lessons"). Cross-model harness transplant helps on 2 of 4 tasks. | Lessons and procedure are edited together and never separated. It shows that harness transplant is in the air. |
| **Beneath the Surface** 2611.07624 | 2026-11-05 | 16 archived runs (AHE, HyperAgents, A-Evolve, GEPA): coverage trade-offs, component interventions, admission replays, transfer. | Task-agent level. No improver-memory split. |
| Hyperagents 2603.19461 | 2026-03 | The meta-transfer result (imp@50 = 0.630) carries the whole hyperagent, including generic memory/tracking infrastructure. | This is exactly the confound our idea targets, and it is still unreplicated. |
| Also seen, less close | | Agent Plasticity 2610.08902; MC-EvoBench 2610.13829 (no-prior/unrelated/shuffled sources); Artifact-to-Capability 2611.04952; SEAR 2610.18792; The RSI Tax 2611.13524; RSI-Env 2611.01736; VERSE 2610.02616; MetaShare 2610.21887. | All are actor/artifact-level or cost-focused. None crosses the improver's procedure with its memory. |

**Scoop status.** No paper found runs {early, late improver procedure} x {empty, late memory} with placebo, shuffled and cross-domain arms. At least four Oct–Nov 2026 papers explicitly call for or partly anticipate it: Experience-Becomes-Structure, Physical-AI R1/R2, the Auditing swap test, and Continual AutoResearch's matched/shuffled memory. Expect a direct scoop within ~3–6 months, most likely from the RSIHub/BIGAI group (2610.32253 + 2611.07624), the Auditing authors, or a Hyperagents follow-up. **K1 is not triggered yet.**

---

## 2. Mock NeurIPS review (harsh)

**Summary.** The paper asks whether a later-generation self-referential improver is better because its *procedure* improved or because it accumulated *memory* (archive, lessons, logs) about the task distribution. It runs a 2x2 transplant (I0/Ik procedure x empty/Ik memory), adds length-matched placebo and shuffled-memory arms, and evaluates in-domain and cross-domain. It reports a Shapley split.

**Strengths**
- It asks the right question at the right time. The census in 2610.27610 shows that almost no RSI paper isolates improver gains. The causal design (transplant, placebo, shuffled, cross-domain) is cleaner than anything in the improver literature.
- Either outcome is interesting in principle. "Learning to improve = learning the benchmark" would deflate a hot claim. A dominant procedure share would be the first controlled evidence of procedural meta-learning in LLM improvers.
- It is cheap once checkpoints exist, and the protocol could become a standard reporting control.

**Weaknesses**
1. **The partition is ill-defined (main soundness issue).** In Gödel Agent, DGM-H and Hyperagents, task knowledge leaks into the *code/prompt* of the procedure: hard-coded heuristics, domain-specific instructions, few-shot examples. A "memory-wiped Ik procedure" can therefore still hold memorized benchmark knowledge, which biases the result toward "procedure". Conversely, an archive holds *procedural* information, such as which edit types worked, which biases toward "memory". Without an operational definition and a knowledge-scrubbing check, the Shapley shares are not identifiable.
2. **Transplant compatibility.** Ik memory may be formatted for Ik's retrieval code, so I0 cannot read it. A low memory share could then be an interface artifact. A format-adapter arm is needed.
3. **The base effect may not exist.** Hyperagents' compounding result is n.s. (0.640 vs 0.610), and AIDE²'s ignition test is inconclusive. Splitting a gain that does not survive matched compute is meaningless. The idea is fully conditional on C1.
4. **The 70/30 thresholds are arbitrary.** A reviewer will ask why 70%. The estimand and its CI should be pre-registered, not a story with fixed cutoffs.
5. **Scope.** If this runs on one improver system and one benchmark family, it reads as "an ablation of Hyperagents", which is workshop level. It needs at least 2–3 improver families (code-level self-referential, meta-prompt/meta-skill, archive-based evolutionary) and at least 2 domains.
6. **Shapley with 2 players is just a 2x2 ANOVA with an interaction term.** Calling it Shapley adds no insight. Interaction is likely to dominate (procedure that knows how to use memory), and the paper needs a plan for interpreting a large interaction.
7. **Novelty optics.** Reviewers will cite Continual AutoResearch (matched/shuffled memory), Lineage Is Memory, and the Physical-AI frozen-knowledge control, and call it "a known control applied one level up". The contribution must be the *finding*, plus a generalized protocol and a tracked trajectory, not the design.

**What would make reviewers say…**
- "Engineering": one system, one benchmark, no CIs, a gain decomposition of a gain that is itself n.s.
- "Incremental": framed as "we add a memory ablation to Hyperagents".
- "Workshop": a standalone 4-page result without the procedure-share-over-generations curve or cross-domain arbitration.

**Likely score as proposed: 4–5 (borderline reject)** as a standalone paper. **6 (weak accept)** as the central causal section of C1 (improver scaling), if the fixes below are made and the result is crisp.

---

## 3. Scores (rubric)

| Dim | Score | Rationale |
|---|---|---|
| N (×3) | 3.0 | Not run yet. But at least 4 close Oct–Nov 2026 papers pose the question, propose the control, or run the actor-level analogue, and the scoop risk is high. |
| S (×2.5) | 3.5 | A crisp falsifiable claim with both outcomes publishable. But the effect may be too small to decompose, and the partition issue weakens the surprise. |
| R (×2) | 5.0 | It is the core Gödel Agent question: is self-reference buying a better improver or just memory? |
| C (×2) | 3.0 | Good causal arms. Soundness hole in the partition; risk of a single system or benchmark; arbitrary thresholds. |
| F (×1.5) | 3.5 | $300–800 on top of C1 checkpoints. Generating multi-seed gen-k improvers on open models is the real cost and risk (Hyperagents is reportedly hard to drive with open models). |
| T (×1) | 4.5 | RSI-evaluation rigor is the hottest sub-trend of Oct–Nov 2026, and the idea is still slightly ahead of it. |

**Total = 20 × (3·3.0 + 2.5·3.5 + 2·5 + 2·3.0 + 1.5·3.5 + 4.5) / 12 = 20 × 43.5 / 12 = 72.5.** Viable with fixes.

Kill flags:
- K1 (scooped): not triggered.
- **K2-risk (conditional):** if the C1 improver gain is not significant at matched compute, the decomposition is uninterpretable. Mitigation: make the paper's claim "the measured improver gain, *whatever its size*, is X% memory", with a power analysis, or pivot to the "no gain to decompose" null inside C1.
- K3: not triggered if run with open or cheap models.
- K4: not triggered.

---

## 4. Fixes that raise the score

1. **Operationalize "memory" as all improver-readable state outside the procedure's source.** That covers the archive, lessons files, logs and scores. Then add a **knowledge-scrubbed procedure** arm: an LLM (or a human) removes domain-specific content from Ik's code and prompts, and a leak audit checks that a classifier cannot identify the domain. The gap between Ik-raw and Ik-scrubbed is a third factor, *procedural code that encodes task knowledge*. This turns the soundness hole into a finding. (+C, +S)
2. **Make cross-domain the arbiter, not an add-on.** Run the full 2x2 with Ik trained on domain A and evaluated on a fresh domain B, using B-native memory vs. A-memory vs. empty. Procedural meta-learning predicts that Ik+empty beats I0+empty on B. Without that, "procedure" is unproven.
3. **Use at least 3 improver families**, where memory is naturally separable:
   - (a) Skill∞-style meta-skill (SKILL.md vs references/lessons);
   - (b) a GEPA/meta-prompt evolver with an archive;
   - (c) a Gödel-Agent/DGM-H-style code-self-modifier on a small benchmark.
   Run 5 seeds each, with the frozen-improver and matched-compute parallel-sampling baselines from 2607.12227.
4. **Add a format-adapter arm.** An LLM re-renders Ik memory into I0's memory schema, which removes the interface confound. Add **memory-dose curves** (0, 25, 50, 100% of the archive, chronological vs. random) to show how memory value accumulates.
5. **Track the procedure share over generations k ∈ {0, 5, 10, 20, 40}** (reusing C1 checkpoints). The headline figure is a **"procedure share vs. generation" curve**: does meta-learning emerge late, or is memory always dominant? That turns a 2x2 table into a dynamics result. It also connects to Agent Plasticity: memory share is the stored experience, and procedure share is the improved conversion rate.
6. **Replace the 70/30 thresholds with pre-registered estimands:** memory share, procedure share and interaction, each with bootstrap CIs. State in advance which CI pattern supports which reading. Follow the Lineage-Is-Memory pre-registration style.
7. **Frame it as a reusable protocol.** "Transplant Test": a 6-arm reporting control for any improver claim, with a small library. Explicitly cite 2610.27610 (4/26 statistic) and 2610.29677 R1, and position the work as the first empirical execution of their control with the missing off-diagonal arm.
8. **Ship it inside C1.** C1 gives the effect, and this gives its cause. Keep a fallback: if C1's improver gain is null, report the transplant on Skill∞ (which has a +4.8 gain to decompose) and on Hyperagents' published transfer setting.
9. **Move fast.** Draft an arXiv preprint of the Skill∞/GEPA transplant result by Feb 2027 to plant the flag before the expected scoop.

### Sharpened headline claim
> "Across three families of self-referential improvers, most of a later-generation improver's measured advantage comes from inherited memory (its archive and lessons): transplanting that memory into the generation-0 procedure recovers most of the gain, while the memory-wiped later procedure keeps little of it. The remaining procedure share vanishes once domain knowledge is scrubbed from the evolved code or the domain changes. Self-referential 'learning to improve' is, so far, learning the task distribution. We release the six-arm Transplant Test as a reporting control for improver claims."

(If the data instead show a procedure share that survives scrubbing and a domain change and grows with generation, the headline becomes: "the first controlled evidence of procedural meta-learning in LLM improvers, emerging only after generation ~k*".)

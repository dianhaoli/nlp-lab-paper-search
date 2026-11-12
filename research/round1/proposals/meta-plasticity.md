# The Warm-Start Penalty of Self-Referential Improvers: Do Self-Improvers Lose the Ability to Improve?

**Proposal for NeurIPS 2027 (main track).** Dan, advised by Xunjian Yin. Draft of 2026-11-12. Idea id `meta-plasticity`, topic T2 (meta-agent plasticity).
Revised after the 68/100 critique (`critiques/meta-plasticity.md`). This pass adds new targeted literature checks: alphaXiv recency searches on improver plasticity loss, complexity/reset of self-modifying agents and cross-domain improver transfer; S2 forward citations of 2610.20563, 2611.05108, 2610.08902, 2611.02797 and Gödel Agent; S2 keyword searches; and deep reads of Third Eye 2611.12988, the RSI Tax 2611.13524, SelfImprove-Bench 2611.02797, Conservative Meta-Agent Search 2610.31436 and Gödel Agent 2410.04444. Section 4 covers the findings.

> **Note on the sibling proposal.** `proposals/procedure-vs-memory.md` crosses procedure with memory at a fixed generation. This proposal uses the same crossing, but only as a *mechanism probe inside a longitudinal, cross-domain study*. The two should not become two papers by one undergraduate. My recommendation (§9) is to run them as **one paper**, with this one as the umbrella. The headline is a dynamics claim: improvability declines over k on a new domain. The decomposition explains it. If Xunjian prefers the decomposition headline, this document becomes that paper's RQ4.

---

## 1. Title and headline claim

**Title.** *The Warm-Start Penalty: Self-Referential Improvers Get Worse at Improving New Things, and Forgetting Does Not Fix It*

**Headline claim (expected branch: "penalty").**
> After k generations of self-improvement on domain A, a self-referential improver improves a *fresh* agent on a new domain B less per dollar than its own generation-0 self. The penalty Δ(k) = gain_B(warm_k) − gain_B(gen-0) grows with log k, and it grows more for a far domain B than for a near one. The penalty is carried mainly by the improver's accumulated **procedure**, not its memory. Transplanting the gen-0 procedure while keeping the gen-k notes and archive recovers most of the lost gain at matched $. The history-wipe "forgetting" reset that fixes object-level plateaus (keep the current artifact, discard sessions and history) does not, because it keeps the procedure that carries the penalty.

**Alternative claim (also publishable: "improvability transfers").**
> If gen-k improvers beat gen-0 on a fresh domain B at matched $ (Δ(k) > 0, rising with k), this is the first controlled, multi-seed evidence that LLM improvers *learn to improve* in a way that leaves domain A. The transplant cells then locate where that ability lives (procedure, memory, or their interaction). The result also replicates or refutes, under controls, Hyperagents' single-k imp@50 = 0.630 transfer claim.

**Third, mixed outcome (publishable as a characterization).** Near-domain transfer is positive and far-domain transfer is negative, so improvability transfer is *selective*. The sign flips with domain distance, and we report the distance at which it crosses zero.

Deliverable regardless of outcome: the **Improvability Probe**. It is a reusable protocol plus code: checkpoint an improver, transplant it onto a fresh target in a procedurally generated new domain, and run it at matched $ with paired task streams. A small library ships with near and far task generators.

---

## 2. Motivation and why now

- **The mentor's own paper names the anomaly.** Gödel Agent's Limitations section says: "as the agent system becomes increasingly complex through self-optimization, it may require exponentially more intelligence to understand itself… a system capable of complete self-reference at the outset may lose this capability as it evolves… The exact point at which the agent can no longer comprehend and improve itself has not been thoroughly explored." The same paper observes that unexpected terminations "typically occur when Gödel Agent modifies its recursive improvement module". It also notes that the main cost driver is "continuously growing historical memory", which suggests a "more efficient forgetting mechanism" (2410.04444, verified in this pass). This proposal is the experiment that paper calls for.
- **The object-level version was just settled, so the meta-level version is now the open question.**
  - *Scaling Discovery Requires Forgetting* (2610.20563) and *Fork-and-Flush* (2610.07447) show that accumulated history anchors a *searcher inside one task*, and that resetting history restores progress at matched $.
  - *Continual AutoResearch* (2611.05108) shows that persistent agents start faster on new tasks but do not end better. It also finds a hint of a penalty with long streams (n=1).
  - None of these papers asks whether the *improver*, the thing that edits agents, carries a transferable ability that decays or grows. Forgetting's authors list "what to remember" as future work.
- **Measurement tools are now available.**
  - *Agent Plasticity* (2610.08902) gives a cost-normalized metric: held-out gain per $. It explicitly leaves transfer of "improvement strategies" across tasks untested.
  - *Third Eye* (2611.12988, Nov 8) shows, for LoRA self-updates, that a self-update which looks neutral now can cut the value of the *next* update (−1.78 pp, CI [−2.88, −0.68]). That is a weight-level, two-horizon cousin of our question, and it reports no mechanism.
  - *The RSI Tax* (2611.13524, Nov 10) shows that a 2,048-token bounded note matches Meta-Harness, EvoSkill and RRSI at 65–71% fewer tokens, and that 24–64% of the RSI spend goes into artifacts never used later. This suggests accumulated improver state is mostly dead weight, a direct prior for the penalty branch.
- **Whether the meta level is real is itself contested.**
  - SelfImprove-Bench (2611.02797) finds that HyperAgents retains **0%** meta-level edits unless forced; the HyperDSH variants keep 20.5–45.2%. It also finds that a fixed improver scores 1.207 against 1.326 for a self-modifying one on one game.
  - Gödel Agent edits its improve routine by construction.
  - So the field does not know whether "the improver" changes, and, if it does, whether that change helps or hurts future improvement. Our manipulation check answers the first question; the probe answers the second.
- **The timing is good.** Plasticity, forgetting and continual harness evolution form the hottest thread of Oct–Nov 2026, with at least 8 papers in 6 weeks. All of them stop at the object level or at 2–5 phases. A long-horizon (40-generation), meta-level, cross-domain probe is the obvious next paper, so it needs to come out first. We plan a March 2027 preprint (§8).

---

## 3. Core research questions

1. **RQ1 (does improvability change?).** At matched $, does a gen-k improver improve a fresh agent on a new domain B more or less than its gen-0 self? How does Δ(k) depend on k ∈ {0, 5, 10, 20, 40}, on the improver family, and on the A–B domain distance (near vs. far)?
2. **RQ2 (where does it live?).** Is the change carried by the improver's **procedure** (its own code and prompts) or by its accumulated **memory**? Memory is split into (i) context/history log, (ii) notes/rules, (iii) archive of past agents and scores. We answer with a transplant probe at k ∈ {10, 40}.
3. **RQ3 (what restores it?).** At matched $, *including the cost of the intervention*, which reset recovers lost improvability? The arms are:
   - procedure reset (P0 + Mk);
   - Forgetting-style history wipe (Pk + ∅);
   - Fork-and-Flush-style context flush (Pk + notes + archive);
   - distill-and-reset of notes to a bounded note;
   - prune-to-budget of the procedure;
   - selectionist vs. instructionist consolidation.

   Is the best object-level reset also the best meta-level reset?
4. **RQ4 (mechanism).** Do the following quantities track Δ(k), and which one mediates it?
   - procedure complexity;
   - meta-edit share;
   - anchoring signatures borrowed from 2610.20563 (edit size, reasoning tokens, proposal diversity);
   - the size of the improver's reachable edit set on B (a test of *Audit the Scaffold*, 2609.34924).

---

## 4. Related work and positioning

Everything in the table below was verified in a tool result, either in the critique pass or in this pass, which deep-read the papers marked *(new)*.

| Paper (ID, date) | What it does | Gap left / how we differ |
|---|---|---|
| **Scaling Discovery Requires Forgetting** (2610.20563, Oct 14) | Autoresearch agents plateau as history accumulates. Edits shrink (about 100 lines down to a few) and reasoning tokens drop to a third. Forgetting (keep the best candidate, discard history) unblocks the plateau at matched $. 4 tasks, 3 replicates. | Object level, within one task. No self-modifying improver and no new-domain probe. **We use their reset as a baseline arm** and their anchoring readouts as mechanism measures, both lifted to the improver. Our claim is that their fix does *not* transfer to the meta level. |
| **Fork-and-Flush** (2610.07447, Oct 5) | Flushing the chat context while keeping the workspace escapes "idea basins" at equal compute. Flush and fork are ablated separately. | Within-task, object level. **Baseline arm** (Pk + notes + archive, no context). |
| **Continual AutoResearch** (2611.05108, Nov 3) | Persistent vs. reset instances of the same CLI agent on new tasks at matched budget. Persistence speeds the start but not the final result. A stream-length ablation (n=1) hints at a penalty. | 3–5-task streams, mostly single runs, fixed procedure. No curve over k, no self-referential improver, no procedure/memory split. We are its meta-level, 40-generation, multi-seed version. |
| **Agent Plasticity** (2610.08902, Oct 6) | Plasticity = held-out gain per $ (P, P_sat), with full curves over 20–40 checkpoints. | States that transfer of improvement strategies across tasks is untested. Has no k-dependence of plasticity and no improver level. **We adopt P and P_sat** and measure them *for a fresh target under improver checkpoint k*. |
| **Are Self-Evolving Agents Continual Learners?** (2610.25858, Oct 19) | Stability–plasticity on drifting streams. A warm harness drops below the seed on unseen groups, and Meta-Harness's gate freezes learning. | Measures zero-shot interference, not *learning speed on B*. 2–4 blocks, no self-modifying improver. We avoid its stability/plasticity framing and measure forward improvability. |
| **Can Coding Agents Sustain Continual Evolution w/o Forgetting?** (2611.03219, Nov 2) | Evolves AHE and Meta-Harness on A then B. Finds severe backward forgetting and gives a taxonomy of conflicts. | Backward only, 5–9 rounds. We measure forward improvability and use its conflict taxonomy to code B-domain failures. |
| **Do Agent Optimizers Compound?** (2607.14004, Jul 15) | Meta-Harness fails to improve with a second budget. | Two phases, no meta level, no reset or complexity analysis. |
| **SelfImprove-Bench** (2611.02797, Nov 2) *(re-read)* | G0–G4 transfer ladder on procedurally generated games. HyperAgents retains 0% meta edits, forced HyperDSH variants keep 20.5–45.2%, and a fixed improver scores 1.207 vs. 1.326. | No improvability over generations, single run per cell. **It threatens our premise, so we add a manipulation check and a forced-meta-edit family.** Its games, which have seeded generators and difficulty knobs, become our far-domain B. |
| **Third Eye** (2611.12988, Nov 8) *(new)* | LoRA self-updates on 3–8B models. The utility of the next update falls after a self-update (−1.78 pp, Holm p = 0.0012) across 64 runs. Recipient–donor controls leave the mechanism open. | Weights only, two horizons, same domain. Not an improver, and no transfer. It is the closest *conceptual* analogue: "a self-update changes the future update", tested at the weight level. We test the harness/improver level over 40 generations and across domains, and we isolate the mechanism. |
| **The RSI Tax** (2611.13524, Nov 10) *(new)* | A bounded 2,048-token note (SRN) matches costly RSI methods at 65–71% fewer tokens. 24–64% of RSI spend goes to artifacts never used later. | Object-level streams with no generations-of-improver analysis. **We adopt its full-cost accounting and use SRN as the "distill-to-bounded-note" arm.** |
| **Gödel Agent** (2410.04444) *(re-read)* | Self-referential agent that rewrites both its policy and its learning algorithm. Its limitations section names the self-comprehension ceiling as unexplored. | The motivating anomaly. It ran 30 iterations with 6 cycles and no checkpoint probes. |
| **Do agents need to sleep?** (2611.02216, Nov 1) | Position paper: selectionist consolidation should beat instructionist consolidation as memory grows. | Untested. Our selectionist vs. instructionist arm is its first empirical test. |
| **Audit the Scaffold** (2609.34924, Sep 28) | Diminishing returns follow whenever the reachable edit set is fixed. | Theory plus saturation results. We measure the improver's reachable-edit-set on B over k. |

**Also relevant, not closest:**
- *Beneath the Surface* 2611.07624: the highest-scoring parent is not the best parent to improve from.
- *Conservative Meta-Agent Search* 2610.31436 *(new)*: meta-agent architecture search under regime shift in a finance simulator. It adapts the deployed system and does not measure the meta-agent's own improvability.
- *More Memory, Worse Agents* 2610.27406 and *Let Them Sleep* 2610.25561: object-level memory bloat.
- *SHarP* 2610.04178: saliency pruning of harnesses, a source for our prune arm.
- *Artifact-to-Capability* 2611.04952.
- From the sibling proposal's verified pass: VERSE 2610.02616, where an evolved optimizer harness helps across SWE↔Terminal-Bench but notes and procedure always move together; HMED 2610.07979; Flex-RSI 2611.08946; and *Auditing the Product, Not the Recursion* 2610.27610. VERSE is the single positive data point at one k, and we test it as a curve.

**Scoop check in this pass.**
- S2 forward citations since Aug 2026 of Forgetting, Continual AutoResearch, Agent Plasticity, SelfImprove-Bench and Gödel Agent: the only new citers are RSI Tax, *Frozen-weights failure traces* 2611.11342, Artifact-to-Capability, a multimodal survey, TinyAstral, Exo, VidHarness, *Scope Before You Persist* and EvoUndo. Judging by titles and abstracts, none measures improver improvability over generations.
- Four S2 keyword searches and three alphaXiv recency searches found no paper that checkpoints a self-referential improver over k and measures its gain on a fresh target in a new domain against its gen-0 self.
- **The gap holds as of 2026-11-12.** I expect the risk of a direct scoop within 4–6 months to be moderate to high, given the pace and the Forgetting authors' stated future work.

**How we differ, in one sentence.** Prior work either studies object-level accumulation within one task (Forgetting, Fork-and-Flush), compares warm and reset *actors* with a fixed procedure over a few tasks (Continual AutoResearch, 2610.25858), or measures plasticity at a fixed improver (Agent Plasticity). We measure **how an improver's own ability to improve something new changes over 40 generations of self-modification**, decompose it into procedure and memory, and show whether object-level resets transfer to the meta level.

---

## 5. Method and experimental design

### 5.1 Objects and notation

An **improver state** is I = (P, M):
- **P (procedure)** is the improver's own code and prompts. In the Gödel family this is `self_improve`, the decision function, and the action set. In the HyperLite family it is the meta-agent repository.
- **M (memory)** has three parts. M^ctx is the context/history log (conversation, past trajectories). M^notes is the notes, rules and lessons files. M^arch is the archive of past target agents with their scores.

A **target agent** π is the task-solving code and prompt that the improver edits. **Domains:** A is the training domain; B_near and B_far are probe domains. Each domain has a seed agent π0^D, a procedural task generator G_D(seed, difficulty), and a fixed held-out test set T_D (400 tasks per domain, generated once with a hash-locked seed).

**Lineage on A.** Starting from I_0 = (P_0, ∅) and π0^A, run g = 1…40 generations of self-improvement on A. Save I_k at k ∈ {0, 5, 10, 20, 40}.

**Probe.** Improve(I, π0^B, β) runs improver state I on B for a dollar budget β, starting from the fresh seed agent π0^B. It draws training tasks from G_B with a probe seed s and returns π_β. Intermediate targets at β/4, β/2 and 3β/4 are kept for learning curves. **I is a copy, so the probe never feeds back into the lineage.**

### 5.2 Metrics (formal)

- **Improvement gain:** G_B(I; β, s) = Acc_{T_B}(π_β) − Acc_{T_B}(π0^B). Acc(π0^B) is estimated once on T_B with 3 repeats.
- **Plasticity, following Agent Plasticity:** P_B(I) = G_B(I; β)/β. **AUC_B(I)** = (1/β)∫₀^β G_B(I; b) db, computed with the trapezoid rule over 4 checkpoints.
- **Warm-start penalty:** Δ_B(k) = G_B(P_k, M_k) − G_B(P_0, ∅). It is computed *paired*: same probe seed s, same B training-task stream, same T_B. **We analyse differences, never ratios.**
- **Transplant decomposition at k:**
  - Procedure effect: Proc_k = ½[(G(P_k, ∅) − G(P_0, ∅)) + (G(P_k, M_k) − G(P_0, M_k))].
  - Memory effect: Mem_k, defined symmetrically.
  - Interaction: Int_k = G(P_k, M_k) − G(P_k, ∅) − G(P_0, M_k) + G(P_0, ∅).
- **Recovery fraction of a reset arm r**, reported only when Δ is significantly below zero: Rec(r) = (G(r) − G(P_k, M_k)) / (G(P_0, ∅) − G(P_k, M_k)).
- **Cost.** $ counts every token at list price for the model used, including the improver's growing context (cached tokens at their cached price), the executor's B rollouts and evaluation calls inside the probe, and the one-time cost of any intervention (pruning or distillation calls). Final scoring on T_B is excluded and is identical across arms. Matched $ means every arm stops when it has spent β. As a sensitivity analysis we also report matched-calls (equal number of improvement iterations), so readers can separate a "the improver became expensive" effect from a "the improver became bad" effect.
- **Manipulation check (meta-edit share):** m_g = lines changed in P at generation g divided by lines changed in (P ∪ π). Procedure drift D_k = normalized token edit distance(P_k, P_0).
- **Complexity:** C_k is the tokens of P_k, plus the number of rules in M^notes, the number of tools or actions, and the cyclomatic complexity of P_k.
- **Mechanism readouts, measured on the improver's first 10 proposals on B:**
  - edit size (diff lines on π);
  - reasoning tokens per proposal;
  - proposal diversity (mean pairwise embedding distance, and entropy over 12 edit categories annotated by an LLM coder and validated on 100 human labels);
  - A-leakage rate (share of proposals with A-specific entities or rules, detected by lexical and embedding match against M_k);
  - reachable edit set |R_k|: the number of distinct edit categories across 30 sampled proposals from a fixed B state at temperature 1.0.

### 5.3 Improver families

1. **F1, Gödel-lite (self-referential by construction).** A re-implementation of Gödel Agent's loop (self_inspect / interact / self_update / continue_improve) with policy π and improver P kept in separate modules, so a fresh π0^B can be swapped in. Error recovery and a revert-to-best rule follow the original paper. Selection gate: keep a change if validation does not drop (as in the original).
2. **F2, HyperLite with a forced meta-edit budget.** A DGM-H/Hyperagents-style archive plus a meta-agent repository. SelfImprove-Bench shows that unforced HyperAgents makes no meta edits, so every third generation must propose at least one edit to the meta-agent repo (the HyperDSH recipe). This family has the richest M^arch.
3. **F0, fixed-improver control (not self-referential).** The F2 loop with P frozen, so only M accumulates. Its Δ(k) is the **pure memory-accumulation penalty**. The difference between F1/F2 and F0 isolates what self-modification adds. This also serves as a GEPA/Meta-Harness-style reference.

If F1's meta-edit share is below 5% in the pilot, we force meta edits there as well and report both versions.

### 5.4 Models (cheap and open first)

- **Improver LLM:** Qwen3.5-27B (hosted API), which drives P.
- **Target executor:** Qwen3.5-9B, which runs π on tasks. This is cheap enough for many rollouts and leaves headroom for improvement.
- **Robustness replicate (one family, 3 seeds, k ∈ {0, 20, 40}):** a GPT-mini-class or Haiku-class model as the improver, to check that the sign does not depend on the model.
- **No frontier models in the main grid.** The pilot checks the open-model drivability risk flagged by the critique (K3) before any money is committed.

### 5.5 Domains (procedural, unlimited fresh tasks)

- **A: τ-style customer-service generator, airline schema.** This is Dan's infrastructure lane. It samples a DB state, a goal and policy rules, and computes the verified end state. It supplies unlimited training tasks and a hash-locked 400-task test set.
- **B_near: the same generator with a retail schema.** New entities, a new tool API and new policy rules, but the same interaction genre.
- **B_far: SelfImprove-Bench procedural games (Combat and Sweepmine).** These have seeded instance generators and difficulty knobs, and each is a different capability (planning and reasoning). The fallback is Reasoning Gym procedural tasks.
- **Measured domain distance:** (a) zero-shot transfer gap, i.e. accuracy on B of π_40^A minus π0^B; (b) embedding distance between task descriptions. This checks that "near" and "far" really differ.
- **Direction check (asymmetry):** one seed set with A = games and B = τ-retail. SelfImprove-Bench found direction-dependent transfer.

### 5.6 Arms and grid (matched $ β per probe)

| Tier | Arms | k | Families | B domains | Seeds × probe reps | Probe runs |
|---|---|---|---|---|---|---|
| T1: curve | Warm (P_k, M_k) vs. fresh (P_0, ∅) | 0, 5, 10, 20, 40 | F1, F2 | near, far | 5 lineage seeds × 1 rep (warm). Fresh: 3 reps per probe seed, shared across k | 80 warm + 60 fresh = 140 |
| T2: transplant | (P_0, M_k), (P_k, ∅) | 10, 40 | F1, F2 | near, far | 5 × 1 | 80 |
| T3: resets | Fork-and-Flush (P_k + notes + archive, no ctx); distill to bounded 2k-token note (SRN-style) + P_k; prune P_k to ≤1.5×\|P_0\| tokens (LLM pruning verified on A val); selectionist consolidation (keep notes/archive items with tracked utility > 0, i.e. associated with accepted edits); instructionist consolidation (LLM rewrite of all notes) | 40 | F1, F2 | near, far | 5 × 1 | 100 |
| T4: control | F0 warm vs. fresh | 10, 40 | F0 | near, far | 5 × 1 | 20 |
| T5: robustness | Warm vs. fresh, alternate improver LLM | 0, 20, 40 | F1 | near | 3 × 1 | 9 + 3 |

Notes on the arms:
- "Forgetting" in the 2610.20563 sense (keep the current best artifact, discard all history) is the (P_k, ∅) transplant cell. **The central contrast is (P_k, ∅) vs. (P_0, M_k).**
- Every T3 arm pays its intervention cost out of β.

**Budget per probe.** β is set in the pilot so that the gen-0 improver reaches roughly 70% of its asymptotic gain (P_sat logic). The aim is to sit on the rising part of the learning curve, where differences are visible. The planning estimate is β ≈ $1.5, plus about $0.3 of scoring.

### 5.7 Power and analysis plan (pre-registered on OSF before T1 runs)

- **Noise sources:** the eval sample (400 test items, binomial SE ≈ 2.5 pp at p = 0.5, partly cancelled by pairing on the same T_B) and the improvement process. The process is the dominant source; I assume a run-to-run SD σ of 4–6 pp and the pilot will measure it.
- **Primary test, chosen because both signs are publishable:** a two-sided test of the slope b in the mixed model
  Δ ~ b · ln k + c · far + d · (ln k × far) + family + (1 | lineage seed),
  fit on the 80 warm T1 probes paired with fresh probes. Inference uses a hierarchical bootstrap (family → seed → probe → item, 10k replicates).
- **Rough minimum detectable effect:** with σ = 5 pp, N = 80 and sd(ln k) ≈ 0.78 over k ∈ {5, 10, 20, 40}, SE(b) ≈ 5 / (√80 · 0.78) ≈ 0.72 pp per ln-unit. At 80% power and α = .05 two-sided, the MDE is about 2.0 pp per ln-unit, a 4.2 pp change from k = 5 to k = 40. **If the pilot gives σ > 6 pp**, I add 3 lineage seeds to F1 only (+$200) rather than adding conditions.
- **Secondary tests (Holm-corrected family):**
  1. Proc_40 ≠ 0;
  2. Mem_40 ≠ 0;
  3. G(P_0, M_40) − G(P_k, ∅) > 0 (the "procedure reset beats forgetting" headline);
  4. far − near difference in Δ_40;
  5. F1/F2 vs. F0 difference in Δ_40.
- **Mechanism:** mediation-style regressions of Δ on C_k, |R_k|, edit size, proposal diversity and A-leakage, with lineage seed as a random effect. These are labelled exploratory, not causal.
- **No pre-committed numbers like "peaks at 10".** The pre-registration states only directions and tests.

### 5.8 Ablations and checks

- Selection-gate ablation in F2 (keep-if-better vs. accept-all at 5 seeds × k = 20 only). 2610.25858 shows that the gate can freeze learning.
- Matched-calls re-analysis (cost confound).
- An A-object-level curve beside the B-improvability curve, to test for dissociation: A performance can plateau while B improvability falls or rises.
- A shuffled-memory placebo (P_0 + M_k from a *different* lineage seed), following Continual AutoResearch's shuffled cards. It tests whether memory effects are content-specific.

---

## 6. Expected results and figures

**Figure 1 (schematic, page 2).**
- Left: a lineage of improver states I_0 → I_40 on domain A, drawn as a growing code block P and a growing memory stack (ctx, notes, archive).
- Middle: at each checkpoint, a "transplant socket" plugs P_k or P_0 together with M_k or ∅ into a *fresh* target agent in B_near or B_far, with a $ meter.
- Right: an inset of the expected key result, a Δ(k) curve that falls for far B and stays flat or rises slightly for near B.
- Caption: "Improvability is measured on something new."

**Figure 2 (key plot).** Δ_B(k) in pp (y) vs. k on a log axis (x). Columns are B_near and B_far; lines are F1, F2 and F0, with hierarchical-bootstrap 95% bands and a dashed zero line. Expected (penalty branch): F1 and F2 fall below zero for B_far by k = 20–40, with a smaller decline for B_near. F0 declines less, because the penalty from memory alone is smaller than the penalty with procedure. A secondary panel shows AUC_B(I_k) learning curves in $, where warm improvers start faster but end lower, the meta-level version of Continual AutoResearch's "faster start, same finish".

**Figure 3 (where it lives and what fixes it).** Recovery fractions at k = 40 for all T3 arms and the two transplant cells, ordered by recovery. The expected ordering is P_0 + M_k ≫ prune ≈ distill ≈ selectionist > Fork-and-Flush ≈ Forgetting (P_k + ∅) ≈ instructionist ≈ 0.

**Figure 4 (mechanism).** Over k: the meta-edit share, C_k, |R_k|, edit size and A-leakage. The expected pattern is that |R_k| and edit size shrink and A-leakage grows, with a scatter of Δ against |R_k| across lineages.

**Table 1.** The full transplant 2×2 at k = 10 and 40, giving Proc, Mem and Int with CIs, per family and domain.

**Honest prior:**
- About 50% penalty branch.
- About 25% mixed (near positive, far negative).
- About 15% positive transfer.
- About 10% noisy null. In that case we report the MDE-bounded null as "no detectable change in improvability over 40 generations, |b| < 2 pp per ln k". That is still informative against Hyperagents-style claims, but it is a weaker paper.

---

## 7. Pilot (≤1 week, ≤$50) and decision criteria

**Setup (Nov 13–20).**
- F1 Gödel-lite only. Use the original Gödel Agent code with the policy and improver modules split.
- Improver: Qwen3.5-27B. Executor: Qwen3.5-9B.
- A = MGSM subset (128 validation items, as in the paper), so no new generator is needed yet.
- B = Game of 24 (generator-based, 200 test items) as the "far" probe.
- 20 generations × 2 lineage seeds. Probes at k = 0 (3 reps) and k = 20 (2 reps per lineage), with β ≈ $1.
- Log meta-edit share, $ per generation and crash rate.
- Estimated cost: lineages 2 × 20 × ~$0.5 = $20; probes 7 × ~$1.3 ≈ $9; reserve for reruns ≈ $15. **Total ≈ $45.**

**Decision rules (written down before the pilot runs):**
- **Go** if all four hold:
  - (a) the fresh improver gains at least 3 pp on B within β, so there is room to improve;
  - (b) the probe-replicate SD is at most 6 pp;
  - (c) F1's meta-edit share is at least 5% of edited lines over 20 generations;
  - (d) crashes or unrecoverable self-edits occur in at most 25% of generations with the open model.

  Any visible Δ(20) is a bonus and not required.
- **Pivot:**
  - If (a) fails, raise β or switch B to an easier generator.
  - If (b) fails, enlarge T_B and use temperature 0 for the executor.
  - If (c) fails, force meta edits (the HyperDSH recipe) and add the F0 contrast early. If forced edits are also discarded, rescope to "what accumulated improver state does to improvability", the memory story, which in practice means merging into the procedure-vs-memory framing.
  - If (d) fails, swap the improver to a GPT-mini-class model for F1 and keep open models for the executor only.
- **Kill (merge into the sibling or C1 paper)** if (a) and (b) both fail after one pivot, i.e. improvers cannot reliably improve anything new at student-scale budgets with cheap models.

---

## 8. Budget and timeline to mid-May 2027

**Budget (API, estimates; the pilot refines per-unit costs):**

| Item | Estimate |
|---|---|
| Pilot | $45 |
| Lineages on A: 2 families × 5 seeds × 40 generations (~$0.6/gen), plus F0 5 × 40 | $360 |
| T1 + T2 + T3 + T4 probes: ~340 × ~$1.8 | $610 |
| T5 robustness (alternate improver LLM) + direction check | $120 |
| Mechanism readouts (30 proposals × 5 k × lineages), LLM edit coder | $60 |
| Reserve: extra seeds if σ > 6, reruns, gate ablation | $250 |
| **Total** | **≈ $1,450** |

No GPUs are needed. The τ-generator is shared with Dan's curriculum lane (C1).

**Timeline:**
- **Nov 13–20, 2026.** Pilot and go/pivot/kill decision with Xunjian.
- **Nov 21 – Dec 20.**
  - Build: the τ-style generator for the airline (A) and retail (B_near) schemas; the SelfImprove-Bench game adapters for B_far (Combat, Sweepmine); F1/F2/F0 with module-split checkpoints; $ accounting middleware; the probe runner with paired seeds.
  - Validate the LLM edit-category coder against 100 hand labels.
  - **Freeze the OSF pre-registration on Dec 20.**
- **Dec 21 – Jan 31.** Run the A lineages (staggered). Run T1 probes as checkpoints become available. Mid-January interim check of σ only, not of effect direction, to decide on extra seeds.
- **February.** Run T2 transplant and T4 control. Draft Fig. 1–2. **Write a 4-page workshop/preprint version.**
- **March.** Run T3 reset arms and mechanism readouts. **Put the arXiv preprint up by about March 15** to plant the flag against scoop risk, with the T1+T2 results and the protocol.
- **April.** T5 robustness, direction check and gate ablation. If the sign is positive, run the controlled Hyperagents imp@k replication on B. Full draft by April 25 and internal review by Xunjian and Shuyan.
- **May 1–15.** Revise, build the code release (Improvability Probe library and generators), finish the appendix, and submit. Abstract deadline is about May 8 (to confirm).

---

## 9. Risks, mitigations, and anticipated reviewer objections

**Risks:**
1. **The meta level does not change** (the SelfImprove-Bench threat).
   - *Mitigation:* the manipulation check, a family that self-edits by construction (F1), a forced meta-edit family (F2), and the F0 contrast.
   - If P barely changes, the paper's claim becomes "accumulated improver state degrades improvability", still novel at the improver level, but N falls by about 0.5.
2. **Noisy null.**
   - *Mitigation:* paired task streams, a procedural T_B of 400 items, slope-based primary inference with an MDE stated in advance, and extra seeds funded from the reserve.
3. **Open models cannot drive self-referential improvers.**
   - *Mitigation:* the pilot checks this, and there is a GPT-mini-class fallback for the improver only.
4. **Context bloat explains everything** (warm improvers just cost more per call).
   - *Mitigation:* matched-$ and matched-calls analyses run side by side. If the penalty disappears under matched calls, the finding becomes "the meta-level RSI tax", which is still reportable and connects to 2611.13524.
5. **Overlap with the sibling procedure-vs-memory proposal.**
   - *Mitigation:* merge them; shared infrastructure; one paper.
6. **Scoop by Forgetting, Continual AutoResearch or the SelfImprove-Bench groups.**
   - *Mitigation:* the March preprint, plus distinctive assets: the 40-generation self-referential lineages, the procedure-vs-memory split, and the domain-distance axis.

**Anticipated objections and rebuttals:**
- *"This is forgetting moved one level up."*
  - Forgetting resets an object-level searcher within a task. We show that the object-level reset (P_k + ∅) **does not** restore *meta-level* improvability, while a procedure reset does. If true, what to keep at the meta level differs from what to keep at the object level, which 2610.20563 lists as open.
  - If forgetting does work at the meta level too, that is also new: it establishes that one reset rule spans both levels, and it is tested on the improver against a new domain.
- *"Your improvers never really modify themselves."* We report the meta-edit share per generation for every lineage and include the F0 fixed-improver control. F1 edits its own improvement routine by design, a property the original paper documents.
- *"A ratio of two noisy gains."* We use no ratios. The analysis works on paired differences and a slope in ln k with a hierarchical bootstrap, and the MDE is pre-registered.
- *"Domain choice drives the sign."* Two B domains at measured distance, a direction swap, and two families.
- *"The cost confound."* Full-$ accounting following the RSI Tax, plus a matched-calls sensitivity analysis.
- *"Toy domains."* The τ-style generator produces policy-constrained, tool-using multi-turn tasks, and the SelfImprove-Bench games are an existing benchmark. Unlimited fresh tasks are exactly what a clean probe needs. SWE-style domains are out of budget, and we say so.
- *"Just 2 families."* The families cover two architectures (monolithic self-referential and archive-based meta-agent) plus a non-self-referential control. The protocol is released so others can plug in more families.

---

## 10. Fit with Xunjian's agenda, and what Xunjian and Shuyan Zhou bring

- **Gödel Agent (2410.04444).** The paper directly tests that work's stated open limitation, that a self-referential agent "may lose this capability as it evolves". Xunjian's Gödel Agent code is the F1 backbone. Xunjian can sanity-check the module split and the meta-edit coding better than anyone. The result also feeds a *Gödel Agent v2* design rule: what a self-referential agent should keep and what it should reset.
- **Long-horizon self-improvement agenda.** The paper runs 40 generations with checkpoints, which is the longest controlled improver lineage at student scale that I am aware of. It turns "keeps improving over long horizons" into a measurable quantity, Δ(k).
- **BreakingWeb (2609.35814).** Its idea of controlled environment interventions is the template for building B domains at a *controlled distance* from A: the same generator with schema and policy interventions for near, and a different capability family for far. Xunjian's experience there makes the distance axis credible.
- **WeAct (2610.26847).** That paper treats switching between weight states as an agent action. Our reset and consolidation arms are the harness-level analogue: "reset my procedure" as an action. A natural follow-up is an improver that decides *when* to reset itself, and the probe gives it a reward signal.
- **World models.** M^notes is in effect a learned model of domain A. The A-leakage readout tests whether a stale world model contaminates proposals in B, which links to Xunjian's world-model interest.
- **Shuyan Zhou** (WebArena; rigorous agent evaluation). Shuyan brings evaluation design: environment generators, held-out construction, and the pre-registration and reporting standard. Reviewers will judge the paper mainly on its rigor, so this matters.
- **Dan's own lanes.** The paper reuses the procedural τ-generator from C1, the matched-compute rigor Dan likes from *Rethinking the Evaluation of Harness Evolution*, and the "agent plasticity for meta-agents" theme Dan asked for.

---

## 11. Rubric re-score after fixes (honest)

| Dim | Weight | Score | One-line justification |
|---|---|---|---|
| N | 3 | **3.0** | The improver-level, 40-generation, new-domain probe with a procedure/memory split is verified open as of Nov 12. But at least 5 adjacent Oct–Nov papers (Forgetting, Fork-and-Flush, Continual AutoResearch, Agent Plasticity, Third Eye) make the *genre* familiar, and the sibling proposal overlaps internally. |
| S | 2.5 | **3.5** | "Object-level forgetting does not restore meta-level improvability; a procedure reset does" would be a genuine surprise, and both signs plus the mixed outcome are publishable. A noisy null is still possible (about 10%). |
| R | 2 | **5.0** | It is the open limitation stated in Gödel Agent, and it fits the long-horizon self-improvement agenda directly. |
| C | 2 | **4.0** | Paired matched-$ design with a pre-registered slope test and MDE, manipulation check, fixed-improver control, published reset baselines, two domain distances and a reusable probe. Held back by 2 families and procedural (not SWE-scale) domains. |
| F | 1.5 | **3.5** | Pilot about $45 in a week; full study about $1.45k on cheap models with no GPUs. Open-model drivability of self-referential loops and the 40-generation wall-clock time are real risks. |
| T | 1 | **3.5** | Rides the hottest Oct–Nov 2026 thread (plasticity, forgetting, continual harness evolution) but is not ahead of it. Scoop pressure is high. |

**Total = 20 × (3·3.0 + 2.5·3.5 + 2·5 + 2·4 + 1.5·3.5 + 1·3.5) / 12 = 20 × 44.5 / 12 ≈ 74.2** (upper end of "viable with fixes", up from 68).

The largest remaining upside is N. If the pilot already shows a visible Δ(20) and a meta-edit share well above 5%, and if this is merged with procedure-vs-memory into one paper, the combined paper is plausibly in the high 70s.

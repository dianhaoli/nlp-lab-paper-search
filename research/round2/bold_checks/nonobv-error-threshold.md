# Bold check: nonobv-error-threshold

*"Self-referential agents hit an Eigen error catastrophe, earlier than fixed improvers."* Skeptical area-chair check, 2026-11-12.

**Verdict: PROMISING, about 74/100. Not scooped, but the biological analogy is the wrong one, and the claim needs to be re-derived before it is sharp.**

- **No K1 scoop.** I found no paper that:
  - fits a defect-rate × length threshold law to an LLM self-improver; or
  - compares self-referential and fixed improvers on how fast defects accumulate; or
  - measures whether the editor's own code stays intact across generations.
- **Partial pre-emption of the headline.**
  - *A size knee already exists.* "When Self-Evolution Backfires" (2608.05810, Tencent, Aug 2026) already reports a **"capability-contamination phase transition"**: performance rises, peaks at a critical skill-pool size k*, then collapses. It also shows that defects are inherited through lineage. So "there is a knee in size" is no longer surprising on its own.
  - *Self-reference already costs stability, qualitatively.* Meta^n (2608.24735) argues in prose that every self-editing system "must hold some driver fixed to keep from corrupting itself". The Kinetic Theory paper (2610.03243, under review at ICLR 2027) proves a separation theorem: a self-referential evaluator voids the convergence certificate.
  - *What is left.* The **quantitative law** and the **editor-integrity (mutator) measurement** are still open.
- **The core technical problem.** Eigen's error threshold is a property of an *un-gated population in which every replication copies all L sites*. Gödel-Agent-style loops are gated hill-climbers with rollback, and most edits are patches, not full copies.
  - With a perfect gate and elitism there is no catastrophe at all: the master copy is never lost.
  - The analogy that fits is the **drift barrier / Muller's ratchet with a noisy gate**. A defect whose effect is smaller than the gate's resolution is invisible to selection, so it ratchets in.
  - Editor code is selected only *indirectly*, through what later edits do. So editor defects fall below the barrier more often. That is the real mutator mechanism, and it can be measured directly.

---

## 1. Scoop searches run

| # | Channel | Query / target | Relevant hits |
|---|---|---|---|
| 1 | alphaXiv (recency) | Eigen error threshold / error catastrophe in self-modifying LLM agents; mutation rate × genome length | EvoUndo 2608.28363, Mendel Gödel Machine 2608.07645, SEABench 2609.35596, Self-Authored Verification Unreliable 2607.24300, **Mutation Without Variation 2606.05408**, **Darwinian/Lamarckian/Agent-Directed (Lo & Zhao) 2611.07048**, World-Model Collapse as Phase Transition 2606.31399 (this one is about belief dynamics, not code) |
| 2 | alphaXiv (recency) | self-referential agents that modify their own editor; mutator effect; complexity ceiling | **Meta^n 2608.24735**, Huxley-Gödel Machine 2510.21614, RSI on a Budget 2610.29121, Mixture of Self-Improving Branches 2609.37834, Reflections on Trusting Trust Revisited 2609.17817, Red Queen GM 2606.26294 |
| 3 | alphaXiv (recency) | harness bloat / length growth → collapse; complexity ceiling | **Harness Evolution Hits a Ceiling 2610.11655**, Masked Erosion 2610.38392, Artifact-to-Capability 2611.04952, What Stops RSI in Robotics 2609.31760, HarnessMM 2610.32813 |
| 4 | alphaXiv (recency) | population-genetics theory (mutation load, Eigen, flattest) tested on LLM evolution | Evolution of Sex for AI 2609.18560 (weight-merging populations; not relevant), Universal statistical signatures 2604.10571, Eigen-model fidelity catastrophe 2508.12871 (pure biology). None tests an error threshold on LLM improvers. |
| 5 | alphaXiv (recency), a follow-up lead from the WebSearch | "contamination threshold" in skill pools | **When Self-Evolution Backfires 2608.05810** (closest empirical threat), Blind Curator 2607.07436, Practice Makes Unsafe 2608.12851 |
| 6 | Semantic Scholar (4 queries) | error threshold LLM self-improvement; error catastrophe self-modifying agent; mutation rate code length threshold; self-referential improver corrupts editor | Winner's Curse 2610.09239, HarnessEvolve 2609.00829, Mutation Without Variation. Nothing on error thresholds. |
| 7 | S2 forward citations | 2609.08175 (Reliable Self-Evolution) | 3 citers: Self-Evolving Agent Harness 2610.18813, Bad Genius 2609.18366, SGM 2510.10232. None is about length or defect thresholds. |
| 8 | S2 forward citations | 2610.03243 (Kinetic Theory) | 0 citers |
| 9 | WebSearch (OpenReview / ICLR 2027) | "error threshold" OR "error catastrophe" self-improving agent | No ICLR 2027 submission found. One secondary news summary of a "contamination threshold" paper led to 2608.05810. That news host (themodelwire.com) was blocked in this session, so I could not open the article itself. |

## 2. Deep reads

### 2609.08175, A Theory of Reliable Self-Evolution for Agent Harnesses (USTC/HKUST, Sep 2026). The closest prior named in the proposal.
- **The ceiling.** It is J < 1 − (1−Z)δ − ε_Σ under their Two-Gate rule. It is set by the allowed retained-task loss δ and the evaluation error. A measured-margin rule can go past it.
  - Validation cost scales as Õ(u⁻²) as the remaining gap u → 0.
- **What it lacks.**
  - No harness-length variable.
  - No per-edit defect rate.
  - No phase transition.
  - No self-referential vs fixed comparison.
- **Experiments.** DS-1000 only, one model (gpt-5.4-mini), 24 pools. The experiments are illustrative, not a sweep.
- **Gap left open.** The gap is clean: their ceiling is a property of the *gate*. The proposal's ceiling would be a property of *gate × editor × length*.

### 2610.03243, A Kinetic Theory of the Gated Self-Evolving LLM Agent (Neusoft; under review at ICLR 2027)
- **Overlapping vocabulary.**
  - Gate strictness = selection strength.
  - Without a gate, the population drifts to the LLM prior (a "replicator–mutator system").
  - Theorem 4.5: if evaluator and evaluated are merged, the certificate is void. Self-reference is "the daemon becoming part of the gas."
- **Scope.** It explicitly applies only to plugin populations that meet axioms A1–A4. It says *"a monolithic self-modifying agent satisfies none of A1–A4, and for it the kinetic description is undefined."*
- **What it does not model.** Defect accumulation, length, or editor integrity.
- **Effect on novelty.** It erodes the "self-reference has a cost" novelty only rhetorically. It leaves Gödel-Agent-type monolithic self-modifiers explicitly uncovered, which is exactly our territory.

### 2608.05810, When Self-Evolution Backfires (Tencent, Aug 2026). Most dangerous empirical neighbor.
- **Findings.**
  - On Terminal-Bench 2, ungated skill accumulation goes 48 → 60 → 62 → 52 → 50% as the pool grows 35 → 179 skills. They call the peak k* a "capability–contamination phase transition."
  - Defects propagate because new skills are distilled while the defective ones sit in context. This is lineage inheritance, close to a mutator effect.
  - Rollback recovers only 1.7 of the 12.3 points lost.
- **Limits.**
  - Skills, not self-modifying code.
  - One backbone, one benchmark, 5 rounds, n=50 tasks, CIs about ±15pp.
  - k* is defined as an argmax, not as a law. There is no ε, no gate-size scaling, and no editor-vs-task distinction.
- **Effect on our paper.** We must cite it and position against it. Their result is the qualitative phenomenon; ours would be the *quantitative law and its mechanism*.

### Also checked
- **Meta^n 2608.24735.** It assumes, without measuring, that editing the improver destabilizes, and freezes Ω for that reason. A measured editor-integrity curve would directly test the premise of their design, which is a nice hook.
- **Harness Evolution Hits a Ceiling 2610.11655.** It traces the ceiling to the proposer's evidence package, not to defects. It also reports "inert accepts" (an edit that silently removed a 20-call brake). Those are anecdotal sub-resolution defects slipping through the gate, which is supportive evidence for the drift-barrier framing.
- **Mutation Without Variation 2606.05408.** LLM mutation without selection converges to attractors. It does not measure defects or length.
- **Lo & Zhao 2611.07048.** An evolutionary-theory comparison of Darwinian, Lamarckian and agent-directed evolution. It has no error threshold, no mutator, and no LLM experiments.

## 3. Area-chair critique

### Is the claim sharp? Partly. The mathematics borrowed from Eigen does not transfer.

1. **Eigen requires full replication.** The L in ε·L comes from copying every site in every generation.
   - In Gödel Agent, an edit is usually a patch to one function. The defect probability per generation then scales with *diff size* (and perhaps with ε(L) through long-context degradation), not with L.
   - **Fix:** make *edit mode* the manipulated variable. Under full-file rewrite, defects per generation are ∝ L and an L* should exist. Under patch/diff editing it should vanish or move a lot.
   - This is a crisp, cheap mechanism test that a reviewer cannot dismiss as curve-fitting.
2. **Gated elitist loops do not lose the master copy.** A catastrophe needs *false accepts*: neutral-looking edits that carry hidden defects.
   - The relevant theory is the **drift barrier (Lynch) / Muller's ratchet under noisy selection**. A defect of effect d is purged only if d is greater than the gate's resolution, which is about σ/√n for gate size n.
   - So the law should be stated in terms of gate resolution. A ln(s) form needs a derivation that matches the actual accept rule.
   - As written, "s set by gate size and evaluator noise" is hand-waved, and reviewers who know population genetics will catch it.
3. **The mutator claim is the most novel part, and it is under-specified.**
   - The sharp and measurable version: *selection on editor code is second-order*. An editor defect changes the score only through later edits, so it is effectively invisible to a one-step gate.
   - **Prediction:** at matched edit volume, the retained-defect rate per line in editor regions is greater than in task regions. Only self-referential agents have editor regions to corrupt, which gives the lower L*.
   - This can be measured with a per-region **defect audit**: unit tests on editor functions, plus a frozen "editor probe suite" of standard edit requests scored for correctness.
4. **Injected token noise is artificial.** A reviewer will say "random corruption breaks code, of course."
   - Natural ε must vary through real knobs: backbone, temperature, edit mode, context length.
   - Injected noise should only calibrate the curve.
5. **Power.**
   - The proposed design is 5 noise levels × 4 gate sizes × 2 improver types × 3 seeds × 25 generations. A change-point fit on that grid with 3 seeds is fragile.
   - It is also unclear whether harnesses naturally *reach* the lengths where the knee sits.
   - **Fix:** manipulate L directly by seeding harnesses at about 1k / 4k / 16k / 64k tokens, using functional but redundant code (inert helper modules that are still executed and tested). Then measure the retained gain over 15 generations.

### Novelty: 3.5/5
No paper fits a threshold law or measures editor integrity. Still, 2608.05810 owns "a phase transition in accumulation size," and Meta^n plus the Kinetic Theory paper own "self-reference costs stability" as a qualitative statement.

### Surprise: 3.5/5
- If true, the result gives a quantitative design rule: "gate size needed to sustain an L-token self-modifying harness."
- If false (patch editors show no knee), the paper can still say that self-modifying LLM agents escape the error threshold because they are patch-based, not replicators. That is publishable but weaker.
- The self-referential < fixed ordering is fairly expected; the editor-region drift barrier is the surprising part.

### Relevance: 5/5
It is literally Gödel Agent's failure mode, quantified. The mentor co-authored the base system.

### Rigor / competitiveness: 3/5 as written, 4 if reframed
It needs:
- an L manipulation;
- an edit-mode manipulation;
- per-region defect audits;
- ≥2 task domains;
- ≥2 backbones;
- an a-priori law that predicts each backbone's natural ceiling out of sample. This last piece is the killer figure.

### Feasibility: 3.5/5
About $900 is plausible for a cheap backbone. The real cost is 25-generation runs with held-out evaluation in each generation. Seeding L directly cuts generations. A pilot (one backbone, rewrite vs patch, 3 seed lengths) costs under $60.

### Timing: 4/5
Harness evolution and RSI-ceiling papers are booming (Sep–Nov 2026). This is a theory-plus-measurement angle that few groups take, so it is less likely to be scooped than another method paper.

**Score = 20 × (3·3.5 + 2.5·3.5 + 2·5 + 2·3 + 1.5·3.5 + 1·4) / 12 = 20 × 44.5 / 12 ≈ 74.2 → PROMISING (viable with fixes).**

## 4. Sharpened claim

> **The self-modification drift barrier.** In a gated self-modifying agent, a defect is purged only if its effect on the gate score is larger than the gate's resolution (about σ/√n). Defects below that resolution ratchet into the code.
>
> Code that is selected only *through later edits* (the agent's own editor) almost always sits below the barrier. So a self-referential improver accumulates editor defects faster than task defects. Its retained gain then collapses at a critical length L*, which:
> - (i) rises with gate size n and falls with the per-generation defect load;
> - (ii) exists for full-rewrite editors but moves sharply for patch editors;
> - (iii) is strictly lower than that of a fixed-editor improver with the same model.
>
> **Falsifiers:**
> - per-line editor-region and task-region defect retention are equal;
> - L* does not move with n;
> - patch and rewrite editors show the same L-dependence.

**Killer figure.** Retained held-out gain vs seeded harness length L, for {self-referential, fixed} × {rewrite, patch}, with gate size as color. Overlay the law's out-of-sample prediction of each backbone's natural ceiling.

## 5. Minimal experiment (revised)
1. **Seed lengths.** Seed harnesses at L ∈ {1k, 4k, 16k, 48k} tokens. Pad with executed-but-redundant modules, so that length is real code and not comments.
2. **Edit mode.** Full-rewrite vs unified-diff patch.
3. **Improver type.** Self-referential (Gödel Agent: the editor lives in the editable code) vs fixed (the identical editor is frozen outside the editable surface).
4. **Gate size.** n ∈ {8, 32, 128} tasks.
5. **Runs.** 2 backbones (cheap open model + one cheap API model) × 3 seeds × 15 generations, on 2 domains (e.g., a DS-1000 subset and a math or text-classification benchmark).
6. **Defect audit.**
   - Each generation, run a frozen editor-probe suite: 20 standard edit requests with known correct diffs. Its success rate is the editor-integrity score.
   - Also run region-tagged unit tests, and do an LLM-plus-static diff audit to classify each accepted hunk as beneficial, neutral or defective.
7. **Fitting.** Fit the drift-barrier law on the injected-noise calibration runs only. Then predict the natural-ε runs out of sample.

Pilot: 1 backbone, rewrite vs patch, L ∈ {1k, 16k}, n=32, 8 generations. Cost under $60.

## 6. Must-cite / must-differentiate
- 2608.05810 (contamination phase transition; lineage inheritance of defects)
- 2609.08175 (verification-cost ceiling)
- 2610.03243 (gate as selection strength; separation theorem; excludes monolithic self-modifiers)
- 2608.24735 Meta^n (frozen driver for stability; assumed, not measured)
- 2610.11655 (harness ceiling; inert accepts)
- 2606.05408 (LLM mutation attractors)
- 2611.07048 (evolutionary RSI theory)
- 2610.09239 (winner's curse / selection noise)
- 2610.38392 (masked erosion of safeguards; adjacent "gate cannot see" mechanism)
- 2608.28363 EvoUndo

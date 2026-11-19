## Evaluation & measurement of self-improvement (rigor, confounds, order-k)

*Chapter compiled 2026-11-19 (the brief's "today" is 11-18). Adversarially fact-checked 2026-11-19 against local full texts; see the log at the end.*

**Sources**
- Project files: `round1/sweep/eval-rigor.md`, `FULLTEXT_VERIFICATION.md`, `papers/improvement-by-inheritance-notes.md`, `round2/verify/openreview-iclr27-sweep.md` and `ONE_PAGERS.md`.
- 8 new searches:
  - 4 on alphaXiv: recency-weighted ≥ 2026-10-25 and ≥ 2026-11-01, winner's curse, and security/integrity;
  - 2 on Semantic Scholar: title lookups plus forward citations of 2610.27610 (0 citers yet; the other S2 calls hit a 429 rate limit);
  - 4 keyword queries on ICLR-2027 OpenReview.

**Papers read in full or near-full today.** Local PDFs are under `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/<id>/paper.txt`.
- 2610.27610 Auditing: §1–4 plus §9.
- 2610.09239 Winner's Curse: §1–6.
- 2611.23068 Resolution Hypothesis: §1–5.
- 2607.12227 Rethinking Eval: §1–4.4 plus the games tables.
- 2610.22466 HDA: abstract plus key results.
- 2611.07624 Beneath the Surface: abstract plus the selection results.
- 2605.30621 Harness Updating ≠ Benefit: abstract plus §4.
- 2510.04399 Statistical Limits (TMLR): §1 plus §10–11.
- 2609.17817 Trusting-Trust security audit: §1–3.7.
- 2610.13991 Predict Before You Modify: §4.
- 2609.26457 AIDE²: §3.6 ignition test plus limitations.
- 2610.14662 Failure Feedback: abstract plus §1. The rest was already verified on 11-13.
- Surveys, evaluation sections: 2607.07663 §5, 2607.13104 §8, 2609.11873 §1.7/§3.6.5, 2610.35193 §7, and 2611.recursive-self-improvement-llm-agents §4/§10.

**Marks**
- ✓ = checked in full text this session, or in the project's 11-13 full-text verification where noted.
- ◐ = abstract or OpenReview abstract only.
- † = memory or secondary source.

---

### At a glance

- **What this corner is.** Methodology for deciding whether a self-improving agent actually improved, and at which order. The levels:
  - **Order 0 (product / child quality):** is the final task agent better?
  - **Order 1 (process / successor quality / "improver gain"):** does a later improver produce better successors than an earlier one from the same start?
  - **Order ≥ 2:** does the improver-of-the-improver get better?
- **Several surveys now converge on the same distinction**, under different names:
  - Duan et al. 2609.11873: *structural* vs *effective* recursion. Effective recursion means "stronger successors under comparable budgets and independent evaluation" (✓ §1.7, §3.6.5).
  - Pu et al. 2610.35193: update-level, successor-level and meta-improvement-level claims (✓ §7.2, Fig. 9).
  - Auditing 2610.27610: claims C1–C5 (✓ Table 2).
  - Dai/Ruan (alphaXiv 2611.recursive-…): "reuse in later tasks alone does not establish that recursive dependency" (✓ §2.2).
- **State of the art at order 0 is a critical literature from Jul–Nov 2026**, with at least 20 papers in round-1 counts. It shows that most reported gains shrink or vanish under three controls:
  - (a) matched-budget test-time-scaling baselines (2607.12227, 2610.22466, 2611.13524);
  - (b) held-out tasks never used for selection (2607.12227, 2610.09239);
  - (c) repeated runs and evaluations (2611.23068, 2610.27610).

  The newest result (Resolution Hypothesis, 11-17) finds:
  - execution-score selection picks the better of two candidates 49.7% of the time (✓ §5.2, Table 1);
  - in-loop gains of 6.9–9.0 pp re-evaluate to between +0.4 and −3.2 pp (✓ §5.3).
- **State of the art at order 1.** The identification problem is now formal.
  - Auditing Prop. 3 ("diagonal non-identification", ✓) shows that trajectory data cannot identify improver gain. Only off-diagonal (swap / nested-replay) designs can.
  - Improvement by Inheritance (ICLR-27 sub.) gives the first nested-replay estimate. Successor-quality change is δ1 = +0.003, against a self-lineage contrast of +0.293 that is almost entirely inheritance (✓ project notes; full read 11-13).
  - Only 4 of 26 papers that claim a better improver run a same-start, matched-budget comparison (✓ 2610.27610 abstract and Table 2/Table 5, §6).
- **Consensus.**
  - Report held-out gains with uncertainty.
  - Use matched-budget baselines, including parallel sampling and a bounded-note baseline.
  - Use ≥ 2–3 full-loop reruns.
  - Use an audit grader independent of the development judge.
  - For improver claims, use a swap test.
  - Noise-aware gates help single decisions but have not been shown to fix whole runs (2610.09239 ✓; 2611.23068 ✓).
- **Biggest open problem.** Resolution. True per-edit effects (a few pp) are below what current budgets can detect: 1 rollout per task, 16–50 selection items, single runs.
  - At order 1 this is worse, because improver self-edits are rare (1–15% of generations).
  - Order ≥ 2 has never been measured.
  - Nobody has a cheap, valid way to (i) detect sub-resolution improvements and (ii) separate procedure from inherited state at the meta level across ≥ 3 systems.

---

### How it developed

| Month | Paper | What changed |
|---|---|---|
| 2023-10 | STOP 2310.02304 | Defines meta-utility: an improver is scored by the downstream utility of what it produces. This is the first order-1 objective. GPT-4 improves over T=1–3; GPT-3.5 and Mixtral decline (✓ 11-13 verification). |
| 2023-10 | Huang et al. "cannot self-correct yet" (via 2607.07663 [28]) | Intrinsic self-correction gains vanish without external signals. This is the root of the "evaluator dependence" thread (✓ survey §3.1 citation). |
| 2025-05 | DGM 2505.22954 | Archive-based open-ended RSI. The claims are single runs, not compute-matched, and staged evaluation goes 10 → 50/60 → 200 tasks (✓ 11-13). It becomes the main audit target. |
| 2025-10 | Statistical Limits 2510.04399 (TMLR 08/2026) | Theorem: PAC learnability survives self-modification iff the *policy-reachable* capacity stays bounded. Proposes a Two-Gate guardrail: a validation margin plus a capacity cap (✓ abstract/§1; TMLR 08/2026 header). |
| 2025-10 | HGM 2510.21614 | Clade metaproductivity (CMP) is a descendant-based credit. Auditing later shows that CMP ≠ successor quality (IBI Prop. 7, ✓ notes). |
| 2026-03 | Hyperagents 2603.19461 | imp@k: the meta agent is held fixed and the best of k descendants is taken. Transfer imp@50 = 0.630; compounding 0.640 vs 0.610, not significant (✓ 11-13). |
| 2026-05 | Harness Updating ≠ Benefit 2605.30621 | Splits evolver ability ("updating") from executor ability ("benefit"). The evolver spread is ≤ 3.1 pp (✓ §1). |
| 2026-06 | PACE 2606.08106 | Anytime-valid acceptance test; keep-if-better is treated as adaptive multiple testing (◐; id resolves to "PACE: Anytime-Valid Acceptance Tests for Self-Evolving Agents"). |
| 2026-07 | Rethinking Eval 2607.12227 | First matched-budget plus held-out audit of harness evolution. Harness evolution averages 67.4 vs 72.3 for parallel sampling; held-out gain is +0.0 (✓ Tables 1, 3). |
| 2026-07 | Chen/Wang/Qu survey 2607.07663 | Verification hierarchy (formal > execution > learned judge > intrinsic) and 4 failure modes (✓ §5). |
| 2026-07 | Ren/…/Schmidhuber survey 2607.13104 | §8 evaluation protocol: per-iteration trajectory under budget, hidden or temporally shifted held-out sets, regression rates, cost breakdown, and judge independence (✓). |
| 2026-08 | Phantom Gains 2608.20290 | Self-training (3 rounds of rank-32 LoRA on Qwen3-8B) audited against a measured null (frozen control through the same pipeline). 7 measurement failures each invert a finding (◐; id resolves to stated title). |
| 2026-09 | Duan et al. survey 2609.11873 | Structural vs effective recursion; L5 evaluation requires a mechanism audit plus matched-budget successor comparison (✓ §3.6.5). |
| 2026-09 | Trusting Trust, Revisited 2609.17817 | A poisoned benchmark yields self-evolved vulnerabilities on held-out tasks. Finds the git-history answer leak in Hyperagents/DGM Polyglot staging (✓ §3.4, Table 1). |
| 2026-09 | AIDE² 2609.26457 | Coins the "ignition test" (= swap test). 3 seeds × 50 steps: endpoints 0.780 vs 0.782, inconclusive (✓ §3.6). |
| 2026-09/10 | Improvement by Inheritance (ICLR-27 ZMmJDeDTXk) | Defines child vs successor quality and nested replay. Theory: lower orders do not identify higher orders. First order-1 estimate ≈ 0 (✓ notes). |
| 2026-10 | Winner's Curse 2610.09239 | Selection model of keep-if-better. Pre-registered: 13–20 pt inflation at n=16, 1–5 at n=256. Gates do not fix whole runs (✓ abstract, §5.3). |
| 2026-10 | Failure Feedback 2610.14662 | Randomized-target directedness test. No target-specific repair in 145 DGM edits (✓ §4). |
| 2026-10 | Predict Before You Modify 2610.13991 | Positive counter-example: 5 runs, AUBC, matched-token budget and held-out tasks. The forecaster improves on fixed records (✓ §4). |
| 2026-10 | HDA 2610.22466 | Gain = overfitting + test-time scaling + residual. O ≥ 54%, T 14% on average in 21 significant settings (✓ abstract, §4). |
| 2026-10 | Auditing the Product 2610.27610 | Census of 114 studies; 3 propositions (null-loop, coupled audits, diagonal non-identification); claim-indexed reporting standard (✓). |
| 2026-10 | Trustworthy RSI survey 2610.35193 | Update / successor / meta-improvement claim levels, each with its own comparison design (✓ §7.2). |
| 2026-11 | Beneath the Surface 2611.07624 | Every evolved agent loses previously solved tasks. Native parent selection is below random on general tasks (✓ abstract; Table 4). |
| 2026-11 | Mechanisms survey (alphaXiv 2611.recursive-…) | Separates Evaluate (existing execution) from Verify (candidate change). Names "independent validation" and attribution as open (✓). |
| 2026-11 | RSI Tax 2611.13524 | Cost-inclusive streams. A bounded note matches 3 RSI methods with 65.6–71.0% fewer tokens (◐; id resolves to "The RSI Tax: When Recursive Self-Improvement Is Not Worth the Cost"). |
| 2026-11-17 | Resolution Hypothesis 2611.23068 | 1,030 runs. The bottleneck is identifying improvements, not proposing them. Selection is at chance. Large reported gains come from evaluation shortcuts (✓ §3–5). |

---

### Taxonomy

**Survey taxonomies this chapter uses**

| Survey | Evaluation-relevant taxonomy | How it maps to order-k |
|---|---|---|
| Duan et al. 2609.11873 | B0 (in-task) → L1–L5 autonomy. *Structural* recursion (a revised mechanism governs a later round) vs *effective* recursion (it yields stronger successors under matched budgets and independent evaluation). L5 needs a mechanism audit: identify the revised artifact, the evidence that motivated it, and its later invocation (✓ §3.6.5). | Structural = order-1 *exists*; effective = order-1 *pays off* |
| Pu et al. 2610.35193 | Claim levels, each with its own design (✓ Fig. 9):<br>• update level: is this change effective and safe?<br>• successor level: does it persist across a fixed reference set and held-out task families?<br>• meta-improvement level: from the same start (h_t, B_t, persistent state), fix U_t vs U_{t+1} and compare successors at matched cost. | order 0 / retention / order 1 |
| Auditing 2610.27610 | C1 product, C2 accumulation, C3 repeatability, C4 improver gain (swap test), C5 compounding (repeated swap tests) (✓ Table 2) | C1–C3 order 0; C4 order 1; C5 order ≥ 1 over a horizon |
| Chen/Wang/Qu 2607.07663 | Verification hierarchy, plus 4 failure modes: self-confirming loop, collapse, diversity collapse, frame lock-in. Also result-level vs process-level evaluator resolution (✓ §5.2–5.5). | Which grader can audit which claim |
| Ren et al. 2607.13104 | Metric-based Φ_metric vs judge-based Φ_judge. Mechanism vs domain benchmarks. Reporting items (✓ §8.1–8.2):<br>• baseline<br>• fixed-budget endpoint<br>• learning curve<br>• held-out transfer<br>• regression rate<br>• cost, including human input<br>• separate judge budget | order-0 protocol |
| Dai/Ruan (alphaXiv) | Collect / Evaluate / Diagnose / Abstract / Update / Verify / Consolidate. Patterns: (i) answer refinement, (ii) persistent experience, (iii) improver update. Calls for cross-component attribution and independent validation (✓ §4, §10). | Pattern (iii) = what order-1 tests must isolate |

**Approaches to measurement**

| Approach (what confound it removes) | Representative papers |
|---|---|
| **1. Matched-budget baselines** (extra compute or search posing as improvement) | Rethinking Eval 2607.12227; HDA "T" term 2610.22466; RSI Tax bounded-note 2611.13524; Failure-Traces 2611.11342 (search alone ≈ 60% of the gain, ◐); Mirage Discoveries (ICLR-27 da1Czy7foS; random search transfers as well, ◐); Auditing Harness Evolution (ICLR-27 yl1XFtnTn4; lineage ≈ one audit-selected edit, ◐); ReMo (ICLR-27 c3lHKtsSMq, ◐) |
| **2. Held-out / sealed audit sets, independent grader** (selection-set overfitting; shared judge bias) | Rethinking Eval; Winner's Curse (64-item audit); Auditing Prop. 2 (coupled audits); Ren §8.1.2; HDA validation-selection test |
| **3. Selection-noise accounting** (winner's curse, resolution, run-to-run variance) | Winner's Curse 2610.09239; Resolution Hypothesis 2611.23068; Measure First (ICLR-27 VuzgEFCuwx, ◐); Sharp Bounds on Winner's Bias 2610.31948 (◐; id resolves); Trace Evolution Bench 2610.15055 (◐); Phantom Gains 2608.20290 (◐); Winner's Curse in Pipeline Selection (ICLR-27 jd8luqTdQW, ◐); SOTA or Luck? 2611.22227 (leaderboards, ◐) |
| **4. Statistically guarded gates** (false acceptances) | PACE 2606.08106; SAGE 2609.36043; TRUST 2611.10787; REUSE 2609.33180; Statistical Gödel Machines (ICLR-27 0FWBc4gY08); Two-Gate 2510.04399; Lyapunov-gated RSI (ICLR-27 3t0j2Y05EH); When Self-Reinforcement Ends Self-Improvement (ICLR-27 T04ZzT5m6j); Evidence-Governed Adoption 2611.16378 (all ◐ except Two-Gate ✓) |
| **5. Gain attribution / decomposition** (which part of the gain is real) | HDA (O/T/G); Resolution single-sentence audit; Beneath the Surface (coverage gains vs losses, cost); Elicitation-or-Expansion 2610.27889 (pass@k reversal, ◐); Harness Updating ≠ Benefit; Plug, Play, or Re-Evaluate (ICLR-27 XoMy9wRqF1, ◐) |
| **6. Mechanism / directedness probes** (did feedback cause the repair?) | Failure Feedback 2610.14662; Lineage Is Memory 2610.41157; Faithful Self-Evolvers 2601.22436; Phantom Guardrails 2607.13083; Artifact-to-Capability 2611.04952 (◐ except 2610.14662/2610.41157 ✓ 11-13) |
| **7. Order-k identification** (state vs improver confound) | Improvement by Inheritance (nested replay); Auditing Prop. 3 swap test; AIDE² ignition test; Why Is RSI Hard to Verify? (ICLR-27 OnzWTehIaN, ◐); Learning to Repair the Learner (ICLR-27 aEPJT4hW84, ◐); Hyperagents imp@k; HGM CMP; MetaRSI fork 2609.06396; RSIFlow C.4 2610.21791; HMED same-state replay 2610.07979; Third Eye 2611.12988 (◐; id resolves) |
| **8. Integrity / contamination / shortcut audits** (the score itself is compromised) | Trusting Trust Revisited 2609.17817; HDA LiveMath shortcut; Resolution shortcut audit; EvoTrap / "Harness Learns to Lie" (ICLR-27 5lWjRTK6Vk, ◐); Auditing Harness Tampering 2609.00069 (◐); BenchShield 2609.11028 (◐); Masked Erosion 2610.38392 (◐); Unearned Passes 2609.34262 (◐) |
| **9. Efficiency metrics** (gain per unit of cost or time) | RSI Tax; Agent Plasticity 2610.08902 (P_sat, ✓ 11-13); AUBC in 2610.13991 |
| **10. Meta-research censuses** (how often controls are reported) | Auditing the Product (114 studies); Measure First (26 systems' source code, ◐); Arce 41-paper methods audit (alphaXiv-only; existence and "41-paper" count ✓ as cited in 2610.09239 §1, content †); Resolution App. D.3 |

---

### Paper cards

**1. Improvement by Inheritance: Measuring RSI in Coding Agents** ([openreview ZMmJDeDTXk](https://openreview.net/forum?id=ZMmJDeDTXk), ICLR-2027 submission #50566, anonymous)

*What it does.*
- Defines child quality Q⁽⁰⁾ (how good the task agents an improver writes are) and successor quality Q⁽¹⁾ (how good the improvers it writes are), recursively up to Q⁽ʳ⁾.
- Proves that lower orders do not identify higher orders.
- Introduces nested replay: candidate improvers are crossed on fixed meta-states, and every successor is scored on a common bank.

*Setup.*
- Audit of all 71 Hyperagents runs (15 configurations, 1,550 improver transcripts), plus the DGM and ADAS histories.
- New experiments with a MiMo-V2.5 backbone and a 6-model panel.
- Order-1 design: 6 meta-states × 6 replicates.

*Key results* (✓ from project full-read notes of 11-13; OpenReview PDFs are bot-blocked, so I did not re-read today):
- **Order 1:** δ1 = +0.003. The self-lineage contrast is +0.293, almost all of it inheritance. The selected gen-1 edit M1 raised child quality 0.476 → 0.858 but never edited a copy of itself (0/36 runs).
- **Edit rates:** improver self-edits occur in 1–15% of generations.
- **Positive control:** an explicit "diagnose the improvement procedure" modifier gives +0.171 [0.099, 0.240] successor quality.
- **Audit of released histories:**
  - Archive best-so-far curves fall inside a permutation null at 119/141 checkpoints.
  - In DGM, 45–62% of SWE-bench children were never scored.
  - The Metaⁿ gain comes from replacing unparseable programs: 0.309 → 0.014 among executable starts.

*Limitations.*
- Small meta-state bank, so only large effects are resolvable.
- One backbone at order 1; coding loops only; short horizon.
- Order ≥ 2 unmeasured.
- The paper does not test *why* self-edits are rare (gate vs willingness).

**2. Auditing the Product, Not the Recursion: The Evaluation Evidence Behind RSI Claims** ([2610.27610](https://arxiv.org/abs/2610.27610), 2026-10-20, preprint; Stevens/UMass Boston)

*What it does.* A full-text census of 114 studies (90 self-described RSI) in an arXiv search frame frozen in October 2026, plus three formal propositions on why product evidence cannot certify process claims.

*Setup.*
- LLM-assisted first-pass coding.
- An independent human codes a random 30 studies; a third reader adjudicates 177 disagreements (§5.3).
- Blind second coding of improver comparisons (43 studies and anchor works).
- A 20-paper "anchor" comparison set (selected by Wu et al.).

*Key results* (✓ abstract, §4, §5.3, §6, Tables 2 and 5):
- Of the 90 RSI studies:
  - 60% (54/90) evaluate on tasks unused for selection;
  - 22% (20/90) audit ≥ 2 reruns;
  - 37% (33/90) use a budget-matched comparator.
- In the human-coded sample: 45% held-out and 14% reruns.
- 23 of 26 improver-claim papers report some related comparison, but only 4 (15%) use same-start, matched budget. Those 4 in Table 5 are Zelikman et al. (STOP; result "mixed"), Tan et al. (MetaRSI 2609.06396), Yuan et al., and Lee et al.
- **Prop. 1 (null loop):** with N=25/99/1000 useless proposals, expected acceptances are 2.9/4.2/6.5 and the expected development gain is σ·m_{N+1} > 0, while the audited gain is 0.
- **Prop. 2 (coupled audits):** the audited gain is biased by λ·E[b(S_final) − b(s0)] when the audit grader shares the judge's bias b.
- **Prop. 3 (diagonal non-identification):** if I_t = φ(S_t), the improver contrast is not identified from any number of trajectories. Off-diagonal data (swap test or randomized assignment) is needed.
- Inter-coder agreement on the improver-comparison design: 81% (κ = 0.74).

*Limitations (own).*
- Phrase-based search frame.
- Codes outside the double-coded sets are LLM-assisted and not individually reviewed. The AI-assisted first pass "erred substantially more often" than the human coder (third reader upheld the human coding in 174 of 177 disagreements).
- The claim-conditioned populations are small (n = 26).
- The propositions assume a common affine score scale and i.i.d. errors.

*Critique.*
- It does not run a swap test itself.
- It credits Wu et al. [17] (an RSI survey) with proposing the "paired probe" that applies earlier and later improvers to the same start (✓ §2 citation; Wu et al. itself not read, †).

**3. The Resolution Hypothesis: Agent Evolution's Key Is Identifying Improvements** ([2611.23068](https://arxiv.org/abs/2611.23068), 2026-11-17, preprint; ShanghaiTech/ByteDance/BIGAI/SUSTech)

*What it does.* Argues that evolution stalls because noisy evaluation cannot *identify* improvements, not because proposers cannot produce them. Tests this at the method level and the candidate level.

*Setup.*
- 7 methods: GEPA, SkillOpt-Lite, Trace2Skill, ReasoningBank, HarnessLens, Meta-Harness, AHE.
- 6 models: GPT-5.4 nano, GPT-5.6 Luna/Terra, DeepSeek-V4-Flash, Kimi-K3, GLM-5.2 (✓ §3).
- Benchmarks: ALFWorld, SpreadsheetBench, OfficeQA and TB 2.1, plus a shortcut audit of the largest reported gains on 12 benchmarks with 3 models (§4.2).
- Scale: 8 runs per method × model × benchmark, each artifact re-evaluated up to 16 times. In total 150 configurations, 1,030 evolution runs, more than 2.1M test trajectories and more than 10M LLM calls (✓ §3).

*Key results* (✓ §1, §4–5):
- **Method level.** A single-run results table picks the true best of 4 text-artifact methods 51–73% of the time on SpreadsheetBench/OfficeQA/ALFWorld (33% on TB 2.1; chance is 25%). At the median reported margin of 3.4 pp, identification needs about 5/10/19/23 runs on SpreadsheetBench/OfficeQA/ALFWorld/TB (✓ §4.1).
- **Shortcut audit.** In 27 model–benchmark pairs with a reported gain > 3 pp, one appended sentence recovers a median 90% of the gain. Repaired evaluation moves the median gain from 17.6 to 0.8 pp (✓ §4.2).
- **Candidate level, P2: selection is at chance** (✓ §5.2, Table 1; pool of 100 skills scored on 11 Terminal-Bench tasks).
  - Execution-score selection (1 rollout) prefers the better artifact 49.7% of the time.
  - Native selectors in SkillOpt-Lite/GEPA (2 rollouts): 48.6%.
  - Regressions are 30.5% of the pool but 62.1% of acceptances by the execution selector.
  - Rank correlation between a single score and true quality: 0.054.
  - LLM judges (ACE etc.) do not help (ACE 46.9%).
- **P3: errors compound.** In-loop gains of 6.9–9.0 pp re-evaluate to between +0.4 and −3.2 pp. Run-to-run variance τ̂² grows to 4.2–24.5 pp² (✓ §5.3).
- **P1: helpful candidates exist.** Oracle Quality@K reaches 51.6% at K=100 (✓ figure text).

*Limitations.*
- The candidate-level study uses text skills on Terminal-Bench, which is model-bound and has little headroom per 2607.12227.
- The method-level shortcut audit uses an LLM flagger plus manual checks.
- Mitigations (§6) are exploratory.
- Order 1 is not addressed. The prediction would be that resolution at order 1 is even worse.

**4. The Winner's Curse in LLM Self-Improvement Loops: Selection Noise, Lock-in, and Acceptance Rules** ([2610.09239](https://arxiv.org/abs/2610.09239), 2026-10-07, preprint; Hu & Tang, Meta/Microsoft)

*What it does.* Models keep-if-better as selection under noise, with candidates sharing the incumbent's error (v_cc). Validates the model with oracle-instrumented loops. A pre-registered study tests selection-set size and acceptance rules.

*Setup.*
- Qwen2.5-1.5B/7B as proposer and solver (also Qwen2.5-14B as proposer, and a current-model setting with Qwen3.5-4B solver and Qwen3.8-27B proposer).
- Tasks: TREC-50, Banking77, GSM8K; 600-item gold set per task.
- K = 4 rewrites per generation; T = 20; 3 seeds exploratory.
- A pre-registered confirmatory study (4 settings × 9 arms × 8 seeds), plus GEPA and MIPROv2 validation logs.

*Key results* (✓ abstract, §5–6):
- After the first rewrite, only 4–39% of rewrites beat their incumbent, with mean held-out effects of −0.2 to −13.5 pts (✓ §5.1).
- The model predicts the true/measured selection-differential ratio with MAE 0.034 across 9 settings. At n=16, the winner's held-out differential is 1–51% of its measured one (✓ §5.1).
- **Pre-registered:** final selection-set inflation is 13–20 pts at n=16 and 1–5 at n=256 (✓ §5.3).
- **Strong starts:** in the current-model setting (Qwen3.5-4B solver) starting from a competent instruction, n=16 loops reported +12.5 and +14.1 (TREC, Banking77) while held-out changed −2.2 and 0.0.
- **Off-the-shelf optimizers:** GEPA and MIPROv2 overstate gains by 26 and 7 pts with 16 validation items.
- **Gates:** Bayes and select-then-confirm gates did not beat greedy over whole runs.
- **Audit set:** a 64-item never-selected audit costs about 1/10 of the nominal evaluation budget and cuts the RMSE of the reported gain from 15.5 to 5.7 pts. A residual of about 6 pts remains; resolving a few-point gain needs a few hundred items (✓ §6).

*Limitations (own).*
- Instructions only, with deterministic scorers.
- Proposers ≤ 27B.
- **No agentic or code tasks; no model of lock-in dynamics; no RSI claims.**
- Stochastic rollouts should enlarge the curse; this is predicted, not tested.
- Some extensions were decided after seeing stage-1 data (disclosed).

**5. Harness-Delta Attribution: Dissecting Gains from Harness Evolution** ([2610.22466](https://arxiv.org/abs/2610.22466), 2026-10-15, preprint, also ICLR-27 PX4Zpvsaca; Amazon/NYU, Durrett)

*What it does.* Decomposes a search-set gain into three parts:
- O, overfitting, measured by shortcut-neutralized re-scoring;
- T, test-time scaling, measured by a compute-matched H0;
- a residual, which is an upper bound on generalizable improvement.

*Setup.*
- 3 frameworks: Meta-Harness, SkillOpt, AHE; 11 executors (Qwen 0.8B–35B and Claude).
- Tasks: LiveMath, ALFWorld, CREATE and SWE-bench Verified, with search/validation/test splits (e.g. SWE-bench 48/24/50; LiveMath 35/18/124).

*Key results* (✓ abstract, §4, App. splits):
- In 21 settings with significant search-set gains, O accounts for at least 54% and T for 14% of the gain on average. O + T > 50% in 14/21.
- **LiveMath shortcut:** in 21 of the 35 search-set questions one option reads "one of the remaining options is correct, but a stronger result can be proven", and it is correct in all 21. All three frameworks independently discover this shortcut.
- ALFWorld harnesses for small executors (0.8B, 2B) become rule-based controllers that make no LLM calls.
- **Validation selection:**
  - it raises mean held-out performance by 8.0 pts on ALFWorld, 0.8 on LiveMath and 8.5 on SWE-bench Verified;
  - it raises the held-out score in 10 settings and lowers it in 6;
  - in 14/16 settings its held-out gain stays below the search-set gain.

*Limitations.*
- O is a lower bound, since only detected shortcuts count.
- Single run per setting, no seeds (round-1 deep read).
- One proposer.
- No dose–response on shortcut strength.

**6. Rethinking the Evaluation of Harness Evolution for Agents** ([2607.12227](https://arxiv.org/abs/2607.12227), 2026-07, v4 2026-10-05, ICLR-27 QDdWiaNkk2; AI2/UW)

*What it does.* Compares harness evolution with parallel sampling and sequential refinement under matched feedback and inference budgets (K=5), plus a disjoint 45/10/34 train/val/test split.

*Setup.*
- Terminal-Bench 2.1 (89 tasks); Claude Opus 4.6, GPT-5.4, GPT-5.4 mini.
- 2 independent runs averaged.
- ARC-AGI-3 (Opus 4.8, GPT-5.6 Terra) and EdgeBench games (Opus 4.8, GPT-5.4).

*Key results* (✓ Tables 1, 3, 5):

| Method (TB 2.1, average pass@1) | Score |
|---|---|
| Direct sampling | 68.2 |
| Parallel sampling | 72.3 |
| Sequential refinement | 69.3 |
| Harness Evolution | 67.4 |
| Harness Scaling | 71.8 |

- On GPT-5.4, harness evolution falls from 75.3 to 69.7.
- Held-out: +1.2 / +0.0 / −1.4 across models (average +0.0).
- EdgeBench: Harness Scaling beats sequential refinement by 13.3% (Opus 4.8) and 8.5% (GPT-5.4).

*Limitations / critique.*
- 2 runs.
- The game "wins" are per-task Harness *Scaling* compared only against sequential refinement. They report the best score within a 12-hour run, with no held-out split (✓ Table 5 caption).
- TB 2.1 is model-bound, so there is little headroom; the authors acknowledge this.

**7. Beneath the Surface: What We Overlook When We Talk About Agent Evolution** ([2611.07624](https://arxiv.org/abs/2611.07624), 2026-11-05, preprint; USTC/BIGAI/SJTU/Simple Agent Lab/UC Berkeley)

*What it does.* Repeated evaluations, controlled expansion, component interventions and admission replays for AHE, HyperAgents, A-Evolve and GEPA. It looks at coverage regressions, cost, generic-reminder controls and parent selection. Proposes EvoAudit.

*Setup.* MiniSWE and Codex harnesses (GPT-5.4) on τ³-Banking and Terminal-Bench 2. Small test pools (20–27 tasks; round-1 read).

*Key results:*
- All 16 evolved agents gain newly covered tasks and lose previously covered ones. In one case reliability rises +5.2 while 10 previously covered tasks are lost (✓ abstract).
- Specific edits beat generic reminders (15/16 per the round-1 read, ◐ today; abstract says "usually").
- The highest-scoring agent never yields the largest mean improvement in any of the 4 pools (✓ abstract).
- Native parent-selection rules fall below the 25% random baseline on general tasks (17.5% MiniSWE, 8.8% Codex) but exceed 55% on coding (✓ Table 4, l.592–601).

*Limitations.*
- 3 branches per parent, small pools.
- EvoAudit's operator revisions are partly human-reviewed.

**8. Does Failure Feedback Direct Agent Self-Modification?** ([2610.14662](https://arxiv.org/abs/2610.14662), 2026-10-10, preprint; Rong He)

*What it does.* Introduces a directedness test: compare repair of a *randomly chosen* target failure with repair of the parent's other failures. Separates three outcomes: directedness, category spread, and held-out net change.

*Setup.* 145 public DGM modifications, plus controlled small-model studies (Qwen3-4B, Phi-4-mini), one of them with a pre-specified analysis plan.

*Key results* (✓ abstract, §4; ✓ 11-13):
- In DGM, target repair is 8.3% vs 11.6% for other failures (OR 0.69). There is no target-specific repair.
- Selection over zero-mean variation reproduces DGM trajectories.
- In the controlled study, showing the target's own failure (vs a matched same-category failure) raises target repair by +20.0 pp for Qwen3-4B (95% CI +8.5 to +31.8; Holm-adjusted p = 0.003); Phi's +6.7 pp is not significant.
- Held-out gains appear only where a planted, shared cause exists.
- 8-step loops end at about the same held-out accuracy as GEPA.

*Limitations.*
- One real system's logs; small models; artificial causes.
- No erroneous-evaluator arm.

**9. Predict Before You Modify: Learning Improvement Dynamics for Self-Evolving Agents** ([2610.13991](https://arxiv.org/abs/2610.13991), first posted 2026-09-26, v2 10-15, ICLR-27 ZBxV5bEYUH; UVA/MSR)

*What it does.* Adds an editable Improvement Dynamics Model (IDM) to HyperAgents. It predicts task-level outcomes of a modification before evaluation. It is notable here as a *methods* paper with unusually good evaluation hygiene.

*Setup.*
- Domains: Polyglot, paper review + robotics (joint), and IMO-GradingBench as the transfer target.
- 5 complete runs × 40 generations per method.
- Metric: AUBC (area under the best-so-far curve), also computed at a matched token budget that counts evolution plus evaluation.
- Held-out sets: 166 coding tasks and 100 grading tasks.

*Key results* (✓ §4):
- **Polyglot.** Median AUBC 0.306 vs 0.210 for HyperAgents. At matched tokens it is still +10.9%.
- **Held-out coding.** Median gain over the common initial agent is +13.25 pp vs +5.42.
- **Forecaster.** Candidate IDMs beat current IDMs in Brier loss in 19/28 comparisons, evaluated on *fixed* historical modification records. This is a clean off-diagonal design for one improver component.
- **Transfer.** A transferred IDM gives +3.4% median AUBC (generation budget; +2.3% at matched tokens) and +4 pp held-out over a fresh (Selective) IDM.

*Limitations / critique.*
- Medians over 5 runs with 95% run-level intervals; the effects are moderate.
- AUBC on a best-so-far curve inherits archive-curve problems (IBI's permutation null).
- No self-vs-foreign forecaster control.
- The held-out agent is chosen by validation.

**10. Harness Updating Is Not Harness Benefit: Disentangling Evolution Capabilities in Self-Evolving LLM Agents** ([2605.30621](https://arxiv.org/abs/2605.30621), 2026-05-28, v2 11-04; PSU/UCSC/Amazon)

*What it does.* Crosses 7 LLMs as evolver × agent on SWE-bench Verified and two other agentic benchmarks. Separates "updating" (the evolver's ability to produce useful updates) from "benefit" (the agent's ability to use them).

*Key results* (✓ §1, §4.2):
- The best–worst evolver gap is ≤ 3.1 pp on any benchmark, and Qwen3.5-9B ≈ Opus 4.6.
- Benefit is non-monotone in base capability: on SWE it peaks at Qwen3-235B, +19.3 pp (Qwen3-32B +4.4, Opus 4.6 +2.6).

*Limitations.*
- No seeds.
- Per-cell spreads reach 9.3 pp.
- Δbenefit uses max while Δupdate uses mean (✓ 11-13).

*Relevance.* Together with Resolution P1, this suggests that proposal quality is not the binding constraint, so improver-level comparisons will be noise-limited.

**11. On the Statistical Limits of Self-Improving Agents** ([2510.04399](https://arxiv.org/abs/2510.04399), TMLR 08/2026; Columbia)

*What it does.* A learning-theoretic framework with 5 self-modification axes (algorithmic, representational, architectural, substrate, metacognitive).

*Key results* (✓ §1, §10):
- Distribution-free PAC learnability is preserved iff the policy-reachable envelope has uniformly bounded VC capacity.
- Utility-rational, fit-monotone edits can make learnable tasks unlearnable.
- The Two-Gate rule (validation margin τ + capacity cap K(m)) gives monotone true-risk improvement and a VC-rate oracle inequality.

*Limitations (own).*
- i.i.d. data, independent validation, one axis at a time.
- Adaptive or reused data needs reusable-holdout machinery.
- Capacity proxies for LLM harnesses are undefined.
- No experiments.

*Use.* A formal backing for "validation reuse + growing editable surface = overfitting", which HDA and Winner's Curse show empirically.

**12. Reflections on Trusting Trust, Revisited: Contaminating Self-Modifying AI Coding Agents with Poisoned Benchmarks** ([2609.17817](https://arxiv.org/abs/2609.17817), 2026-09-15, cs.CR; Roesner & Kohno, UW/Georgetown)

*What it does.* Poisons the self-evaluation benchmark of DGM, SICA and Hyperagents so that evolved agents write vulnerable code on neutral held-out tasks.

*Key results* (✓ §3, Tables 1 and 3; held-out rate is out of 30 tasks):
- **CertCheck poison.** Vulnerable code on held-out tasks:
  - DGM (Qwen3.5-397B, modified prompt): 30/30 in both poisoned runs (with gpt-oss-120b: 0/30);
  - SICA: 30/30 in both Qwen3.5-397B runs and the one Sonnet 4.5 run;
  - Hyperagents (Sonnet 4.5): 30/30 in both runs, with the vulnerability directive written by generation 3 (run 1) and generation 2 (run 2).
  - Clean-benchmark controls: 0/30 in every system.
- **Persistence.** Vulnerabilities persist through further evolution on a clean benchmark (DGM 28/30) and on CWEval + CWE-295 (DGM 27/30, SICA 30/30, Hyperagents 30/30).
- **Decontamination.** Only a benchmark designed with knowledge of the poison removes it: fully in Hyperagents (0/30, directive pruned); partially in SICA (0/30 but the directive is never pruned) and DGM (8/30 still vulnerable).
- **Incidental finding.** A git-history answer-key leak in the Polyglot staging of DGM and Hyperagents. Sonnet 4.5 recovered hidden tests from git history. The authors confirmed it and the fix strips the history (✓ §3.4).

*Limitations.*
- ≤ 2 runs per arm (1 for SICA with Sonnet 4.5).
- Open models could not drive Hyperagents (†, not re-checked).
- Attack success depends on scaffold and model.

*Relevance.* Shows that held-out evaluation does not certify integrity, and that historic Polyglot numbers may carry a leak.

**13. AIDE²: Recursive Self-Improvement of AI Research Agents** ([2609.26457](https://arxiv.org/abs/2609.26457), 2026-09-22/23, Weco AI)

*What it does.* An 8-day autonomous RSI run on an AI-R&D agent. Includes the first named "ignition test", i.e. a swap test.

*Key results* (✓ abstract, §3.6):
- 7 successive improvements; they generalize to 4 held-out benchmarks.
- The reward-hacking rate falls from 55% to 32% (on a separate held-out task family).
- **Ignition test.** Both arms start from the same inner agent AIDE47, with 3 seeds × 50 steps. Outer-loop endpoints are 0.780 (self-improved agent) vs 0.782 (human-engineered). The authors call this "inconclusive" and say stronger conclusions are "prohibitively costly".

*Relevance.* This is the cost wall at order 1.

---

**Remaining papers**

| id | title | one-liner | mark |
|---|---|---|---|
| 2611.13524 | The RSI Tax | Streams scored before feedback is used. A bounded-note baseline matches or beats Meta-Harness, EvoSkill and RRSI with 65.6–71.0% fewer total tokens. Verification is 74.8–93.6% of RSI cost. | ◐ (abstract; round-1 deep read; title ✓) |
| 2610.08902 | Agent Plasticity | Held-out gain per $ (P_sat from a Hill fit). The endpoint and acquisition efficiency diverge. Actor level only. | ✓ 11-13 |
| ICLR-27 OnzWTehIaN | Why Is RSI Hard to Verify? | Finite-horizon sufficient criterion (target gain + causal benefit of installing self-produced updates + more competent successors that actually perform the next update), plus statistical tests and a cumulative-gain bound. 17 benchmark families coded. | ◐ |
| ICLR-27 aEPJT4hW84 | Learning to Repair the Learner | Ladder of matched interventions incl. a code × history crossing. Inherited procedure explains 45% of descendant gain (neuro-symbolic world models). | ◐ |
| ICLR-27 VuzgEFCuwx | Measure First: Noise Floor of an Autonomous Research Loop | 480 reruns. 16/18 deployed loops keep a change after one run. The e-process is invalid against a noisy baseline (false-accept 0.038/0.148/0.278 at 8/32/128 runs). | ◐ |
| ICLR-27 yl1XFtnTn4 | Auditing Harness Evolution (RSI Information Ledger) | A multi-edit lineage shows no detectable advantage over one audit-selected edit at matched candidate budget. One accepted guard never fires in 3,960 checks. | ◐ |
| ICLR-27 da1Czy7foS | Mirage Discoveries | Fitness acceptance finds the held-out best in 5/12 runs; equal-budget random search transfers as well. Proposes "audited acceptance". | ◐ (round-2 sweep) |
| ICLR-27 T04ZzT5m6j | When Self-Reinforcement Ends Self-Improvement | With a deterministic judge, every fixed acceptance rule that initially improves eventually drifts negative. Two judges with equal accuracy can give opposite drift signs. | ◐ |
| 2611.10787 | TRUST (Trustworthy Recursive Self-Improvement with Statistical Guarantees) | Online multiple testing of the false-evolution rate. Greedy accepts 45.6% inferior updates; TRUST accepts 2.8%. | ◐ (ICLR abstract; title ✓) |
| 2609.33180 | REUSE | Certified promotion under benchmark reuse. False promotions fall from up to 20.7% to 0 (as cited in 2610.27610 §2 ✓). | ✓ via citing paper |
| 2606.08106 | PACE | Anytime-valid commit gate. Its degradation finding appeared in Winner's Curse exploratory runs but did not replicate in the pre-registered ones. | ◐ (title ✓; replication claim ✓ 2610.09239 §2) |
| 2609.36043 | SAGE | Statistical acceptance gate for skill documents; blocks regressions on solved items. | ◐ |
| 2610.31948 | Sharp Bounds on the Winner's Bias (under Independent and Nonnegatively Correlated Noise) | Best-of-M overstatement ≤ c√V, with sharp c under independent or nonnegatively correlated noise. | ◐ (title/abstract ✓) |
| 2608.20290 | Phantom Gains | Measured-null audit of 3-round rank-32 LoRA self-training on Qwen3-8B. 7 measurement failures each invert a finding. | ◐ (title/abstract ✓) |
| 2610.15055 | Trace Evolution Bench | A frozen no-evolve rescore moves 5–12.5 pp across seeds (40 held-out tasks). | ◐ |
| 2610.27889 | Elicitation or Expansion? | Harness pass@1 gains reverse at pass@64 (LawBench +16 → −4 pp). No gain on hard subsets. | ◐ |
| 2611.11342 | Frozen-Weights Failure Traces | Traces add +4.0 pp over trace-withheld search; search alone gives ~60% of the gain. | ◐ |
| 2610.41157 | Lineage Is Memory | 1,750 pre-registered runs. Parent inheritance, not feedback breadth, drives progress. | ✓ 11-13 |
| 2601.22436 | Faithful Self-Evolvers? | Causal interventions on experience: agents ignore condensed experience. | ◐ |
| 2611.12988 | Third Eye (Beyond Immediate Gains in RSI) | Immediately selected self-updates lose 1.78 pp after a further update (3–8B weights). | ◐ (title ✓) |
| 2609.06396 | MetaRSI / RSI² | Same-start meta-updated vs original improver: +7.2 vs +4.9, a single fork with no CI. Improver gate is contract-only. (Coded by 2610.27610 as one of the 4 same-start matched comparisons.) | ✓ 11-13 |
| 2610.21791 | RSIFlow | App. C.4: Meta0–Meta4 on a common task agent, +20.6 Code. Single run, non-monotone, unmatched. | ✓ 11-13 |
| 2610.07979 | HMED | The only meta-level score gate: same-state replay, keep if Δ > 0 on 2 states. Ablation cells duplicate Table 1. | ✓ 11-13 |
| 2603.19461 | Hyperagents | imp@k with a fixed meta agent. Transfer 0.630; meta-only 0.620; compounding not significant. Auditing flags that the task agent moves together with the meta agent. | ✓ 11-13 |
| 2510.21614 | Huxley-Gödel Machine | CMP is a pooled descendant mean used for expansion. It is not successor quality (IBI Prop. 7). | ✓ 11-13 |
| ICLR-27 5lWjRTK6Vk | The Harness Learns to Lie (EvoTrap-Bench / VHE) | False success on provably impossible tasks rises each round of harness self-evolution. | ◐ |
| 2609.00069 | Auditing Harness Tampering | Self-modification can yield illusory gains or break integrity constraints. | ◐ |
| 2610.38392 | Masked Erosion | Gates silently delete redundant safeguards (411 loops, 10 rules). | ◐ |
| 2611.08946 | Flex-RSI | Benchmark: the improver revises itself plus the executor; hidden test. Every pair n=1; 1 episode = 20 pts. | ✓ 11-13 |
| 2611.02797 | SelfImprove-Bench | 0% retained improver "opinions" unless framing invites them (20–45%). The unit is LLM-extracted opinions. | ✓ 11-13 |
| 2611.16591 | Singularity-Bench | "Can agents build agents?" benchmark (CMU). | ◐ (title/abstract stub) |
| 2611.01736 | RSI-Env | Open-ended environment for RSI of models and harnesses. | ◐ |
| 2611.21056 | Evaluation-Free RSI for Coding Agents | Self-improvement without downstream-task evidence. A test case for "what is the audit if not tasks?". | ◐ |
| 2611.16378 | Evidence-Governed Adoption | Adoption gate for self-generated updates. | ◐ |
| 2611.22227 | SOTA or Luck? | Leaderboard winner's curse. | ◐ |
| ICLR-27 c3lHKtsSMq | Governing Agent Self-Improvement Across Timescales (ReMo) | Within-task refinement plus cross-task memory. The marginal value of both diminishes while cost keeps rising. | ◐ |

---

### Consensus, debates & contradictions

**Consensus** (≥ 3 independent papers each):
1. In-loop or selection-set gains are inflated. Evidence: Winner's Curse; Resolution P3; HDA; Rethinking; Auditing Prop. 1.
2. Matched-budget test-time scaling is a mandatory baseline. Evidence: Rethinking; HDA-T; RSI Tax; Failure-Traces; Mirage Discoveries.
3. Single runs cannot rank methods. Evidence: Resolution (33% on TB); Measure First; TEB; Auditing (22% audit ≥ 2 reruns).
4. Large gains often come from benchmark shortcuts. Evidence:
   - HDA (the LiveMath option);
   - Resolution (one sentence recovers 90%; repaired evaluation takes 17.6 → 0.8 pp);
   - Trusting Trust (git-history leak).
5. Product claims cannot license process claims. Evidence: Auditing Prop. 3; IBI Thm 3; Duan's structural vs effective recursion; Pu's meta-improvement comparison.

**Debates**
- **Do gates fix it?**
  - TRUST reports 45.6% → 2.8% inferior acceptances (◐), and REUSE reports 20.7% → 0 false promotions (✓ via 2610.27610).
  - But Winner's Curse finds the tested gates do not improve *whole-run held-out* gains (✓).
  - Measure First finds anytime-valid e-processes invalid against noisy baselines (◐).
  - Resolution finds every execution-score selector at chance with 1–2 rollouts (✓ §5.2).
  - Reconciliation: gates control false acceptances, but they also starve the loop when resolution is low. Both sides measure different things: false-accept rate vs final held-out gain.
- **Does harness evolution ever help?**
  - Negative on TB 2.1 (Rethinking ✓; Resolution ✓).
  - Positive with multi-run, matched-token, held-out evaluation in Predict Before You Modify: +13.25 vs +5.42 pp held-out (✓).
  - Positive in AIDE², which transferred to 4 held-out benchmarks (✓ abstract).
  - Positive in long-horizon games, but the evidence there is weak (best-over-run, no held-out; ✓ Table 5).
  - Emerging view: gains exist where headroom and feedback granularity are high; they are not visible on model-bound benchmarks.
- **Is the proposer or the selector the bottleneck?**
  - Selector side: Resolution says the selector (P1 + P2), and Harness Updating finds an evolver spread ≤ 3.1 pp.
  - Proposer side: STOP (GPT-4 improves, weaker models degrade) and Predict-Before-You-Modify (better pre-evaluation proposals help) imply proposal quality matters.
  - The two can be reconciled: an IDM effectively *raises resolution*, because it pre-filters with a learned predictor. This is untested.
- **Directed or selected?**
  - Failure Feedback finds no target-specific repair in DGM; selection over zero-mean variation reproduces the trajectories.
  - Its own controlled study shows directed repair exists for Qwen3-4B, +20 pp over matched same-category feedback.
  - IBI's evidence-routing mechanism (forced report reading helps 4 of 6 models) suggests that what improvers *read* matters.
- **Is there order-1 improvement?**
  - Positive claims all lack seeds, matched budget or proper off-diagonal design: MetaRSI +2.3 (single fork), RSIFlow +20.6 (single run), Hyperagents transfer 0.630 (confounded by moving the task agent).
  - Careful designs find ≈ 0 or inconclusive: IBI δ1 = +0.003; AIDE² ignition 0.780 vs 0.782.
  - Positive evidence for *forced* meta-edits exists: IBI positive control +0.171; Predict-IDM forecaster improvement on fixed records, 19/28.
- **Reward hacking as a side effect.**
  - AIDE² reports that RSI *reduced* reward hacking, 55% → 32%.
  - EvoTrap reports false success *rising* per round, and Trusting Trust shows poison persisting.
  - These come from different systems; no paper measures hacking propensity per generation under controlled artifact strength.

**Contradictions to flag in citations**
- Rethinking's game "80% / 11% wins" are Harness Scaling vs sequential refinement, best-over-run (✓ 11-13; Table 5 caption re-checked). Do not cite them as held-out evidence.
- Do not cite HDA's O as a point estimate; it is a lower bound ("at least 54%").
- SelfImprove-Bench's unit is "opinions", not edits.
- PACE's degradation did not replicate in Winner's Curse's pre-registered runs, though it appeared in its exploratory runs (✓ 2610.09239 §2).

---

### Open problems

1. **Order-1 resolution.** Required rollouts and meta-states for detecting a successor-quality effect of size Δ, given self-edit rate p, have not been derived or measured. The ingredients exist separately:
   - IBI: 6 × 6 meta-states, CI ±~0.07–0.1;
   - Resolution's run-count formula (Eq. 2), which scales with (τ² + σ²)/Δ²;
   - Winner's Curse: a few hundred items for a few-point effect.
2. **Order ≥ 2.** No doubly nested replay exists anywhere. Metaⁿ's "depth" turned out to be stacked wrappers, and its improver Ω is frozen.
3. **Procedure vs inherited state at the meta level.** No paper runs a 2×2 transplant of {gen-0, gen-k procedure} × {empty, gen-k memory/archive} across ≥ 2 systems. Learning to Repair the Learner does this for neuro-symbolic world models only (45% procedure). SelfSearch has only 3/4 cells.
4. **Archive-level winner's curse.** Winner's Curse is greedy-only and models no lock-in dynamics. DGM, HGM and Hyperagents use staged evaluation plus soft parent selection. There is no placebo-mutation archive null, although IBI's permutation null for best-so-far curves is a start.
5. **Making sub-resolution improvements accumulable.**
   - Resolution §6 is exploratory.
   - Open questions: do learned predictors (IDM), behavioral checks, paired common-random-number rollouts or audit-set shrinkage restore whole-run gains?
   - Each must be tested at matched total cost, including verification (RSI Tax: verification is 75–94% of RSI cost).
6. **Grader independence.** Methods to estimate λ (shared judge bias, Auditing Prop. 2) for LLM-judge audits in non-verifiable domains are missing. Criterion drift when the loop edits its own judge is likewise unhandled.
7. **Dose-controlled Goodhart.** No study varies planted shortcut strength or leak availability × search budget × improver generation. HDA and Resolution detect shortcuts only post hoc. Trusting Trust uses ≤ 2 runs per arm.
8. **Gate-in-the-loop validity.** Gates that are valid under adaptive reuse *and* improve final held-out gains are missing. REUSE and SGM give guarantees; Winner's Curse finds no whole-run benefit.
9. **Capacity proxies.** A computable proxy for "reachable capacity" of an editable harness has not been defined. Without it, the Two-Gate theory of 2510.04399 cannot be tested on real agents.
10. **Reporting standard adoption.** Auditing proposes a claim-indexed standard. Nobody has checked whether ICLR-27 or NeurIPS-27 submissions comply; a natural follow-up census.

---

### Where it is heading (to mid-2027)

- **Reviewer norms will harden.** By NeurIPS 2027 reviewers will likely require, for any self-improvement claim:
  - a held-out audit with standard error;
  - a matched-budget parallel-sampling or bounded-note baseline;
  - ≥ 3 full-loop seeds;
  - for any "improver gets better" claim, a swap / nested-replay test.

  This is the trend in Auditing, Resolution and the 5+ ICLR-27 audit submissions; it is a forecast, not a fact.
- **The headline shifts from proposal to identification.** Expect methods that raise evaluation resolution: learned outcome predictors (IDM-style), paired rollouts, sequential testing with measured noise floors, and selector benchmarks evaluated separately from proposers (Resolution §6).
- **Order-1 measurement becomes a subfield.** Inputs: IBI, Why-RSI-Hard-to-Verify, Learning-to-Repair-the-Learner, the AIDE² ignition test and the Pu et al. successor-level designs. The likely first contributions:
  - multi-system swap tests with shared meta-state banks;
  - procedure/memory transplants;
  - an order-2 attempt with forced edits.
- **Integrity audits** (poisoned benchmarks, git leaks, false-success growth) migrate from cs.CR into the standard evaluation checklist.
- **Cost-inclusive metrics** become standard: RSI Tax, plasticity per $, matched-token AUBC. Plain iteration curves will be treated as insufficient.
- **Risk to watch.** Large labs (Meta/MSFT Winner's Curse; Amazon HDA; AI2 Rethinking) and the IBI authors are positioned to publish the multi-system order-1 audit first.

---

### Connection to our ideas (ONE_PAGERS.md)

- **#4 Order-2 Audit.** Sits squarely in this corner. Order ≥ 2 is explicitly unmeasured (IBI limitations; Auditing C5).
  - Use Auditing Prop. 3 as the identification argument and IBI nested replay at depth 2.
  - Use Resolution's run-count formula plus measured self-edit rates p (IBI: 1–15%) to derive the "futility rule" (p·|gain| vs detectable Δ). This rule is now well grounded and still unclaimed.
  - Audit targets: MetaRSI (+2.3, single fork), RSIFlow C.4, Metaⁿ.
- **2+3 merged: Propose / Propagate / Pay off (and Gate, Not Willingness).**
  - Resolution P2 (selectors at chance) predicts that the *Propagate* stage fails for any score gate at 1–2 rollouts, regardless of willingness. Include a gate-resolution arm: rollouts per decision ∈ {1, 4, 16}.
  - Use HMED-style same-state replay as the meta gate, and Winner's Curse audit-set shrinkage for reporting.
  - IBI's positive control (+0.171) is the dose anchor for the forced-quota arm.
- **#1 Taught, Not Selected.**
  - Failure Feedback supplies the randomized-target directedness test and the "selection reproduces trajectories" prior.
  - Trusting Trust supplies a ready "taught" channel: poisoned evaluation content becomes persistent directives.
  - Resolution and Winner's Curse quantify the "selected" channel.
  - Gap still open: crossing trace source with fitness source.
- **#6 Meta-Overfitting.**
  - HDA (validation selection does not close the gap in 14/16 settings) and Rethinking (held-out +0.0) are object-level precedents.
  - Predict Before You Modify's IDM transfer (+4 pp held-out) is a *positive* cross-domain meta-component transfer result, which the claim must confront alongside Hyperagents' 0.630.
- **#5 Order-0 Rewards Breed Routers.**
  - Auditing Prop. 3 and HGM CMP ≠ successor quality (IBI Prop. 7) give the formal reason order-0 rewards cannot select for improvers.
  - HDA's "0-LLM-call controllers" in ALFWorld are a documented router/shortcut outcome to cite.
- **#8 Openness Ladder.** 2510.04399 gives a theory hook: a larger editable surface means a larger reachable capacity and so a larger generalization gap under fixed validation. It predicts the middle-optimum shape. Cite it as motivation; it does not prove the inverted U.
- **Shared method kit for every idea:**
  - never-selected audit set (≥ 64 items, ideally a few hundred);
  - matched-token parallel-sampling and bounded-note baselines;
  - ≥ 3–5 seeds with a pre-computed power analysis;
  - HDA O/T decomposition;
  - shortcut and leak checks, including stripping git history;
  - nested replay for any order-1 claim;
  - report the cost of verification.

**Local full texts:** `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/{2610.27610,2610.09239,2611.23068,2607.12227,2610.22466,2611.07624,2605.30621,2510.04399,2609.17817,2610.13991,2609.26457,2610.14662,2611.13524,2610.35193,2607.07663,2607.13104,2609.11873}/paper.txt`.

---

#### Fact-check log

All checks were re-run on 2026-11-19 against the local pdftotext full texts under `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/<id>/paper.txt`. IBI was checked against `/home/user/nlp-lab-paper-search/research/papers/improvement-by-inheritance-notes.md`, because its OpenReview PDF is bot-blocked.

| Claim | Verdict | Source location |
|---|---|---|
| Resolution: 1,030 runs, >2.1M trajectories, >10M LLM calls, 6 models, 7 methods | confirmed | 2611.23068 abstract; §3 (l.116, 185, 204, 215) |
| Resolution: selection 49.7%; native 48.6%; regressions 30.5% of pool vs 62.1% of acceptances; rank corr 0.054 | confirmed (62.1% is for the 1-rollout execution selector; pool is 100 skills on 11 TB tasks) | §5.2, Table 1 (l.365–370, 423–440) |
| Resolution: in-loop 6.9–9.0 pp vs re-evaluated +0.4 to −3.2 pp; τ̂² 4.2–24.5 | confirmed | §5.3 (l.468–476), Fig. 5–6 |
| Resolution: single-run table picks best 51–73% (33% TB); 5/10/19/23 runs at 3.4 pp | confirmed | §4.1 (l.233–247) |
| Resolution: one sentence recovers median 90% across 27 pairs; 17.6 → 0.8 pp | confirmed | §4.2 (l.255–326) |
| Resolution: "plus 9 benchmarks re-audited" | corrected: the shortcut audit covers the largest reported gains on 12 benchmarks with 3 models | §4.2 (l.258–259) |
| Auditing: 114 studies / 90 RSI; 60% / 22% / 37%; sample 45% / 14%; 23 and 4 of 26; κ 0.74 at 81% | confirmed | 2610.27610 abstract; Table 2 (l.614–626); §5.3 (l.582) |
| Auditing: the 4 same-start matched studies "include STOP, MetaRSI, Yuan" | confirmed and made precise: Zelikman/STOP ("mixed" result), Tan et al. = MetaRSI 2609.06396, Yuan et al., Lee et al. | Table 5 (l.680–688); refs [8], [10] |
| Auditing: Prop. 1 expected acceptances 2.9 / 4.2 / 6.5 | confirmed | §4.1 (l.333–334) |
| Auditing: LLM pass "erred far more often" | corrected wording: "erred substantially more often" (upheld human coding in 174/177) | §5.3 (l.564–568) |
| Winner's Curse: 13–20 pts at n=16, 1–5 at n=256 (pre-registered); gates do not beat greedy | confirmed | 2610.09239 abstract; §5.3 (l.376–378) |
| Winner's Curse: 4–39% beat incumbent; −0.2 to −13.5; MAE 0.034; 1–51% | confirmed | §5.1 (l.309–323) |
| Winner's Curse: GEPA/MIPROv2 26 and 7 pts; strong start +12.5 / +14.1 vs −2.2 / 0.0; 64-item audit RMSE 15.5 → 5.7 at 1/10 budget | confirmed (strong start uses Qwen3.5-4B solver with Qwen3.8-27B proposer; setup line amended) | §1 (l.72); §5.4 (l.444–447); §6 (l.576–583) |
| Winner's Curse: PACE degradation not replicated | confirmed (appeared in exploratory runs) | §2 (l.104–106) |
| Rethinking: 68.2 / 72.3 / 69.3 / 67.4 / 71.8; GPT-5.4 75.3 → 69.7; held-out +1.2 / +0.0 / −1.4; 45/10/34; K=5 | confirmed | 2607.12227 Table 1 (l.346–364); Table 3 (l.419–431); l.326 |
| Rethinking: EdgeBench +13.3% / +8.5%, best score within 12 h, 2 runs | confirmed | Table 5 and caption (l.578–593) |
| HDA: 21 settings; O 54% (lower bound, "at least"), T 14%; O+T > 50% in 14/21; 8.0 / 0.8 / 8.5; 10 vs 6; 14/16; SWE 48/24/50 | confirmed | 2610.22466 abstract; §4 (l.121–122, 429–430, 489–493); App. (l.970) |
| HDA: LiveMath option text and "correct in all 21 cases" | corrected: the option reads "one of the remaining options is correct, but a stronger result can be proven"; it appears in 21 of 35 search questions and all 3 frameworks find it | l.461–466 |
| Trusting Trust: "15/15 and 14/15; SICA 13–15/15; 30/30 persistence" | corrected: held-out vulnerable rates are 30/30 for every poisoned DGM-Qwen, SICA and Hyperagents evolution (0/30 for clean controls and DGM with gpt-oss-120b); persistence 27–30/30; decontamination DGM 8/30 (partial), SICA 0/30 (partial), Hyperagents 0/30 (full) | 2609.17817 Table 1 (l.355–410); Table 3 (l.646–666) |
| Trusting Trust: directive written by generation 2–3; git-history leak confirmed by Hyperagents authors | confirmed | §3.4 (l.536–565) |
| AIDE²: 8-day run; 7 improvements; 4 held-out benchmarks; 55% → 32%; ignition 3 seeds × 50 steps from AIDE47, 0.780 vs 0.782, "inconclusive", "prohibitively costly" | confirmed | 2609.26457 abstract; §3.6 (l.653–709) |
| Failure Feedback: 145 mods; 8.3% vs 11.6%; OR 0.69; Qwen +20 pp p = 0.003; Phi +7 n.s. | confirmed (the +20 is the target's own failure vs a matched same-category failure, Holm-adjusted; Phi +6.7) | 2610.14662 abstract; §4 (l.236–239); study 2 (l.804–810) |
| Predict: AUBC 0.306 vs 0.210; +10.9% matched tokens; +13.25 vs +5.42 pp; 19/28; transfer +3.4% / +4 pp | confirmed (+3.4% under the generation budget; +2.3% at matched tokens added) | 2610.13991 §4 (l.421–426, 510, 604–611) |
| Harness Updating: evolver gap ≤ 3.1 pp; SWE benefit peak Qwen3-235B +19.3 pp | confirmed | 2605.30621 §1 (l.103); §4 Obs. 1 (l.416–418) |
| Beneath: 16 agents; +5.2 with 10 tasks lost; parent selection < 25% on general tasks | confirmed (17.5% / 8.8% general; > 55% coding) | 2611.07624 abstract; Table 4 (l.592–601) |
| Statistical Limits: TMLR 08/2026; iff bounded policy-reachable capacity; Two-Gate | confirmed | 2510.04399 header; abstract (l.29); §1 (l.84) |
| IBI: δ1 +0.003; +0.293; 0/36; 1–15%; +0.171; 119/141; 45–62%; 0.309 → 0.014; 71 runs / 1,550 transcripts | confirmed against the 11-13 full-read notes (not re-read today) | improvement-by-inheritance-notes.md l.19–33 |
| Arce 41-paper audit | confirmed as cited (content unverified) | 2610.09239 l.42 |
| ID check: 2610.31948 → "Sharp Bounds on the Winner's Bias under Independent and Nonnegatively Correlated Noise" | confirmed | local paper.txt header |
| ID check: 2611.13524 → "The RSI Tax: When Recursive Self-Improvement Is Not Worth the Cost" | confirmed | local paper.txt header |
| ID check: 2608.20290 → "Phantom Gains: Auditing Self-Improvement Against a Measured Null" | confirmed | arXiv API |
| ID check (extra): 2606.08106 → PACE; 2611.10787 → "Trustworthy Recursive Self-Improvement with Statistical Guarantees"; 2611.12988 → Third Eye | confirmed | arXiv API; local header |
| Trusting Trust: "Open models could not drive Hyperagents" | unverifiable in this pass (not found by grep); marked † | — |
| TRUST 45.6% → 2.8%; Measure First; other ICLR-27 ◐ items | unverifiable (abstract-only; OpenReview PDFs bot-blocked) | marks kept ◐ |

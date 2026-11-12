# Critique: gate-dial — "The acceptance gate is the stability-plasticity dial of self-improving agents"

Reviewer stance: skeptical NeurIPS AC + related-work search. Date: 2026-11-12.

## 1. Scoop search

**What I searched:**
- alphaXiv discover with recency (3 calls): acceptance gate / keep-if-better / winner's curse / plasticity / lock-in / admission control / adaptive threshold under non-stationarity.
- Semantic Scholar keyword search with 4 phrasings ("acceptance gate self-improving agent plasticity", "keep-if-better acceptance rule long horizon", "stability plasticity self-evolving agent harness gate", "admission control self-modification agent freeze").
- S2 forward citations of 2610.09239 (Winner's Curse), 2610.25858 and 2610.38392 (Masked Erosion). The index is sparse: Winner's Curse is cited only by Masked Erosion and 2611.09699.
- Deep reads (answer_pdf_queries) of 2610.09239, 2610.25858, 2609.10873, 2610.38392, 2611.10787 (TRUST), 2610.16129 (PID gates), 2611.03219 and 2610.03243.

### Closest papers (all verified in tool results)

| ID | Title (date) | What they did | Gap left for us |
|---|---|---|---|
| 2610.09239 | The Winner's Curse in LLM Self-Improvement Loops: Selection Noise, Lock-in, and Acceptance Rules (Oct 7 2026; Meta/MSR) | They compared greedy, McNemar, PACE, select-then-confirm and Bayes gates on instruction rewriting (TREC/Banking77/GSM8K, Qwen 1.5B–14B) for T=20 generations, with 8 pre-registered seeds and a 600-item gold set. Selection-vs-held-out inflation is 13–20 pts at n=16. **No tested rule beat greedy over whole runs.** Fixed-alpha McNemar "gave up most of the available gain" because it rarely commits. They named "lock-in" but explicitly do **not model its dynamics** and make no RSI claims. | The task distribution is stationary, the horizon is 20, there are no agentic or code tasks, and they never measure forgetting or plasticity. |
| 2610.38392 | Masked Erosion: How Acceptance Gates Lose Redundant Safeguards (Oct 28 2026) | Ten gate rules (greedy, split-half, paired McNemar, read-only, veto, audit-veto, margin-aware, IUT, asymmetric) over 411 loops of 8 generations, plus a frozen statistical simulator. It gives a break-cost law c*·√n ≈ const and projects analytically to 200 generations. Figure 7: "Significance gates are slow, not frozen." | Their lens is safety. The tasks are stationary, plasticity and forgetting are not measured, and the long horizon is analytic only. |
| 2611.10787 | TRUST: Trustworthy RSI with Statistical Guarantees (Nov 7 2026) | The abstract states the exact tradeoff: "greedy gate … leads to performance degeneration, whereas overly conservative statistical gates may reject beneficial updates." Over 30 rounds, SGM-B/H (FWER gates) **never accept, so gain is 0**. Greedy gains early then decays. TRUST (online FDR with e-values) wins. | The tasks are stationary and the horizon is 30. It is a method paper, and it already occupies the "a better adaptive gate dominates both" slot. |
| 2610.25858 | Are Self-Evolving Agents Continual Learners? (Oct 19 2026; Mila/GDM; 70 votes) | Four drifting streams (BIRD/BFCL/ScienceWorld, at most 4 blocks, T ≤ 200) with ReasoningBank, GEPA and Meta-Harness. Plasticity–stability correlation r = −0.72. Meta-Harness "appears stable … only because its validation gate rejects half or more of the candidate updates, freezing the workspace … at the cost of low plasticity". Removing the gate drops stability. | Gate freezing appears as **one ablation**. Gate strictness is never varied, and the streams are short (≤4 switches). |
| 2609.10873 | When Validation Stops Learning: Auditing Update Admission for Continual Embodied Agents (Sep 2026) | Position paper plus a toy simulator: "a valid rule can freeze learning". Hoeffding and empirical-Bernstein gates admit 0% of updates and the paired gate 37%. It adds a **missed-opportunity** metric, and replay beats all gates. | The setting is embodied, a scalar toy with 8 updates and no LLM. Its missed-opportunity metric is something we should borrow. |
| 2610.16129 | PID-Gated Continual Prompt Adaptation Under Noisy Feedback (Oct 2026; NeurIPS'26 CL workshop) | Gates that decide *when* to edit (P / I / D-with-rollback) under drift (rule flips, T=120) and label noise. The integral gate is Pareto-best against TextGrad and GEPA. | This is close to our "noise-adaptive gate under drift" arm, but the tasks are toys, there are no statistical gates, no multi-family forgetting, and only 3 drift events. It shows the workshop-level version of our idea already exists. |
| 2611.03219 | Can Coding Agents Sustain Continual Evolution without Forgetting? (Nov 2 2026) | AHE and Meta-Harness evolve across SWE→SWT→Feature, with forgetting of up to 100%. | No gate manipulation. It is a forgetting testbed only. |
| 2610.03243 | A Kinetic Theory of the Gated Self-Evolving LLM Agent (Oct 2026) | Theory in which the gate is the only selection signal. "Escaping a plateau requires changing … gate strictness". It distinguishes proposal exhaustion from information exhaustion (Prop. 4.3) and runs 500-round WebShop plateaus. | Qualitative claims and a single task family. Their Prop. 4.3 is a useful diagnostic for us. |
| Others | SAGE 2609.36043, PACE 2606.08106, Self-Healing Harness 2609.24130, Theory of Reliable Self-Evolution 2609.08175, Self-Evolving Agents with Anytime-Valid Certificates 2607.00871, SkillEvoReg 2609.30861, Agent Plasticity 2610.08902 | The gate-design and plasticity-metric space is crowded. At least 6 gate-method papers appeared between Jun and Nov 2026. | — |

REUSE 2609.33180 was named by the synthesizer, but I did not see it in this round's tool results, so it is unverified here.

### Verdict on novelty
- **The qualitative tradeoff is not new.** "Strict gates freeze, lax gates degrade/forget" is stated in TRUST's abstract, shown empirically by TRUST (SGM gets 0 gain), Winner's Curse (McNemar loses gain), 2609.10873 (Hoeffding admits 0%) and 2610.25858 (the Meta-Harness gate freezes the workspace). A paper whose headline is the tradeoff would be scooped in substance.
- **What remains open:**
  - (a) The *horizon dependence* of gate rankings under **non-stationary** streams with many switches. Every gate comparison so far uses T ≤ 30 and stationary tasks. 2610.25858 is non-stationary but uses ≤4 blocks and does not vary the gate.
  - (b) A *predictive law* for when a gate freezes. Masked Erosion has a break-cost law for safety deletions only.
  - (c) *Attribution* of loss of plasticity to the gate versus the proposer in meta-agents. Nobody separates "gate rejects good edits" from "the proposer stops producing good edits".
  - (d) Forgetting as a function of gate *scope*: what set the gate validates on.
- No 2026 paper makes the central "ranking flips with horizon under drift" claim with this design, so K1 is not triggered. But two to three papers each cover large slices of the design.

## 2. Mock NeurIPS review

**Summary.** The authors run roughly ten acceptance rules with two harness evolvers on a long procedural task stream with hundreds of family switches. They report that strict statistical gates win short-horizon but freeze and lose plasticity long-horizon, that lax gates forget, and that a noise-adaptive gate dominates both.

**Strengths**
- The question is timely and practically relevant. Every self-improving agent has a gate, and the gate is rarely ablated.
- It covers horizons an order of magnitude longer than prior gate comparisons (T = 20–30), under non-stationarity.
- Procedural tasks give unlimited held-out data, so selection-versus-audit inflation can be measured cleanly.

**Weaknesses**
1. **Novelty of the core tradeoff.** TRUST, Winner's Curse, Masked Erosion, 2609.10873 and 2610.25858 already show that strict gates under-accept or freeze and lax gates accumulate harmful commits. Without a new mechanism or law, the contribution reduces to "the known tradeoff, at a longer horizon".
2. **The flip may be trivial or artifactual.** Under family switches, a new family offers *large* early gains, which even strict gates accept. Freezing occurs only when per-edit effects fall below the gate's resolution (≈ z_α·σ/√n). So any "flip" is mostly a statement about effect-size decay versus validation-batch size n, which is a tunable hyperparameter. A reviewer will ask: "did you just pick n so that McNemar freezes?" The paper needs a resolution-matched control (n grows with the round, as in EB+RM in Winner's Curse) and a sweep over n.
3. **Confounded evaluation budgets.** Gates differ in evaluations per decision (sequential e-processes stop early, audits add runs). Unless total evaluation compute is matched, rankings are uninterpretable. This is exactly the "Rethinking the Evaluation of Harness Evolution" critique.
4. **"Noise-adaptive gate dominates" competes with TRUST, SAGE, PACE and PID gates.** It becomes a method paper against 4–6 concurrent methods with 3 seeds. It is underpowered: Winner's Curse needed 8 pre-registered seeds and still saw differences within seed noise.
5. **External validity.** One procedural generator and two evolvers is close to a single benchmark. Do the results hold on a real drifting stream such as 2610.25858's BIRD/BFCL/ScienceWorld or 2611.03219's SWE/SWT/Feature?
6. **Plasticity is not attributed.** "Plasticity collapses" could come from the gate rejecting, the proposer degenerating (Kinetic Theory Prop. 4.3: proposal versus information exhaustion), or workspace bloat. Without counterfactual re-gating of the same candidate stream, the paper cannot say the gate is the dial.

**Likely verdict.** As pitched, this is borderline reject: scores of 4–5 out of 10 with comments like "hyperparameter study of gate strictness", "incremental over TRUST/Winner's Curse", "the ranking flip is a property of n", and "workshop". With the fixes below, especially the predictive resolution law, the gate-versus-proposer attribution and matched compute, it could reach weak accept (6) as a rigorous evaluation/analysis paper.

## 3. Scores (rubric)

| Dim | Score | Rationale |
|---|---|---|
| N | 2.5 | No exact scoop, but 3–5 close Sep–Nov 2026 papers hold every component. The gap is narrow: horizon × non-stationarity × attribution. |
| S | 3.0 | A horizon-dependent ranking flip would be a real surprise, but it is partly predictable from effect-size decay. If there is no flip, the null ("paired gates are fine") repeats existing recommendations. |
| R | 3.5 | Long-horizon self-improvement and meta-agent plasticity fit Xunjian's agenda. It is not directly about self-reference, though a Gödel-style evolver arm can make it so. |
| C | 3.0 | Clean in principle, but there is a hyperparameter-study risk, 3 seeds, one generator, and budgets that are not compute-matched as pitched. |
| F | 3.5 | Hundreds of rounds × 10 gates × 2 evolvers × seeds is fine on a procedural generator with cheap models. Agentic tasks would blow the $800 budget. Counterfactual re-gating is cheap. |
| T | 4.5 | Gates, RSI evaluation and agent plasticity are the hottest Oct–Nov 2026 cluster, which also explains the crowding. |

Total = 20 × (3·2.5 + 2.5·3 + 2·3.5 + 2·3 + 1.5·3.5 + 4.5)/12 = **62.9**. That is "viable with fixes", at the low end.

**Kill flags**
- K1: not triggered. No paper shares the central claim and design. It is *near*: TRUST, Winner's Curse and 2610.25858 state the tradeoff.
- K2: partial risk. If no flip appears, the result restates existing "use paired gates / report held-out" advice. The fixes below make both outcomes informative.
- K3: not triggered.
- K4: not triggered, but the "noise-adaptive gate" arm reads as recombining TRUST, PID and resolution matching.

## 4. Fixes (what would raise the score)

1. **Replace "gate strictness" with a predictive resolution law.** Predict each gate's *freeze round* from (i) the measured per-edit effect-size distribution, which decays as the incumbent improves and jumps at family switches, and (ii) the gate's resolution, z_α·σ/√n. Pre-register the predicted freeze rounds and ranking-flip horizon from a pilot, as Masked Erosion did, and then test them. This turns a hyperparameter sweep into a falsifiable law: "plasticity half-life of a gate". (+N, +S, +C)
2. **Attribute plasticity loss to gate versus proposer with counterfactual re-gating.** Log every candidate. Re-score each frozen-gate trajectory's candidate stream under an oracle gate, which accepts iff the true held-out gain is positive, and under every other gate. Use Kinetic Theory Prop. 4.3: acceptance → 0 with good candidates still present means a gate freeze; no good candidates means proposer exhaustion. This is the genuinely new "meta-agent plasticity" contribution and connects to Agent Plasticity 2610.08902. (+N, +R)
3. **Factorial on what the gate sees, not just how strict it is:** strictness (greedy / paired α / e-process / FDR-TRUST) × scope (current-family validation versus replay over all seen families) × freshness (reused versus re-drawn selection set). Hypothesis: replay-scoped strict gates freeze, current-scoped lax gates forget, and scope matters more than α. This links to Masked Erosion's point that a gate only sees what it measures. (+S)
4. **Match total evaluation compute across gates** and include an oracle-gate upper bound and an always-accept lower bound. Report gate regret decomposed into a false-accept part (inflation and forgetting) and a false-reject part, using the missed-opportunity metric from 2609.10873. (+C)
5. **Demote "noise-adaptive gate dominates" to a simple baseline,** such as resolution matching (n grows with the round, from Winner's Curse EB+RM) or periodic re-opening/annealing of α after detected switches. TRUST and PID gates go in as competitors. The paper should be an analysis paper, not a method paper. (−K4 risk)
6. **External validity.** Use the procedural τ-style generator (Dan's infra) as the main testbed. Add one real drifting stream: replicate 2610.25858's compound drift with a GEPA-style evolver and a Gödel/Meta-Harness-style code evolver, for at least 2 evolvers × 2 domains. Run 5–8 seeds on the main contrasts and pre-register the flip hypothesis. (+C)
7. **Horizon sweep with checkpoints** at 10/30/100/300 rounds, using anytime curves rather than a single endpoint, so the flip, or its absence, is visible directly.

**Expected score after fixes:** N 3.5, S 3.5, R 4, C 4, F 3.5, T 4.5, giving a total of about 76.

## 5. Sharpened headline claim

"In long, non-stationary self-improvement, most of a meta-agent's lost plasticity is caused by its acceptance gate, not its proposer: a gate stops learning when the per-edit effect size drops below its statistical resolution (z_α·σ/√n), and that resolution law predicts in advance the round at which strict gates freeze and lose to lax ones. Changing what the gate validates on (current-family versus replay, fresh versus reused evidence) moves the stability-plasticity frontier more than changing α."

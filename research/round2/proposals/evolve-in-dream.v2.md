# v2: Taught, Not Selected? How Reflective Harness Evolution Absorbs a Simulator's Errors

*(Formerly "Evolving in a Dream". Renamed because Dream-RSI 2609.14858 and 2610.08033 own the "dream" framing.)*

*Proposal: Dan, for Xunjian Yin. Target: NeurIPS 2027 main track (deadline ~mid-May 2027). v2 drafted 2026-11-12. It answers both round-2 red teams: novelty-killer (63.8) and sharpness reviewer (67.9).*

**Status recommendation: GO-WITH-CHANGES (pivot).** The v1 headline is dead. The red team showed that "error type, not rate", "iso-accuracy pairs" and "decision fidelity beats prediction fidelity" are pre-empted in planning and control: DRPE 2609.32322, Goodhart-in-the-Planner 2611.09670, WRM 2610.38439, 2611.01139. What survives is a mechanism question that exists only for optimizers that *read their own rollouts*. That covers GEPA, Meta-Harness, DGM and Gödel Agent, and it does not apply to planners or weight RL.

- v1 asked: does simulator-scored harness evolution Goodhart, and which errors drive it?
- v2 asks: **through which channel does a simulator's error enter the harness — what the reflector reads, or what the score selects?**

---

## 1. Title, headline claim, alternative claim

**Title:** *Taught, Not Selected: Reflective Harness Evolution Absorbs a Simulator's Errors Through Its Traces, Not Its Scores*

**Headline claim (one sentence, directional, falsifiable).**

> In reflective harness evolution scored inside a simulator, most of the real-environment loss is *taught* rather than *selected*. At a matched base outcome-error rate:
> - giving the reflector real-environment traces, while selection stays simulated, recovers most of the loss;
> - giving it real-environment fitness, while it keeps reading simulated traces, does not;
> - and an identical simulator error that leaves a textual trace is exploited markedly faster than the same error made silent.

Formally, the pre-registered primary contrast is **G_SR − G_RS > 0** in the fitness-source × trace-source 2×2 (§4.5). Here G_FT is real held-out gain with fitness from F and reflector traces from T, and S/R mean simulator/real.

**Why it is surprising.** Every 2026 account of simulator or proxy exploitation is a *selection-pressure* story:
- √KL from the proposal distribution: 2611.09670
- best-of-n / optimizer's curse: 2611.01139 Remark 6, 2609.25848
- the inevitability theorem: 2605.15960
- weight-RL collapse: 2608.12253, 2610.08033

If reflective evolvers instead absorb errors mainly by *reading* them, three things follow:
- The standard mitigations aim at the wrong channel. Those mitigations are ensembles, anchors, KL budgets and calibrated selectors.
- A zero-cost mitigation exists: **trace hygiene**, meaning control over what the reflector sees.
- "Readable artifacts are safer", the usual argument for harness over weight optimization, has a mirror image. A readable *optimizer input* is a new attack surface.

**Alternative claim (pre-registered as a first-class hypothesis, equally publishable).**

> *Selected, not taught.* G_RS ≥ G_SR, and legibility does not change exploitation speed. Reflective harness evolution Goodharts exactly like best-of-n selection. The reflector's reading is causally inert for exploitation.

This branch would be a clean simulator-side replication of two observations: 2610.14662 found DGM self-modifications are *not directed* by the failure shown, and Melcer et al. found prompt optimization works about as well with deliberately wrong feedback (cited inside 2610.14662; ID not verified, possibly 2512.13598). The practical consequence is that trace hygiene is useless, and every real-evaluation dollar should go to selection, i.e. anchors and WRM-style direct selectors.

**Null branch (interpretable thanks to a positive control).** Suppose no arm shows real loss under dynamics quirks (Q1/Q2), but the Q5 lenient-judge positive control does. Then: *"harness search exploits lies about success, not lies about consequences"*. That is the opposite of weight RL against frozen simulators (2608.12253, 2610.08033), and it licenses dynamics-imprecise simulators for harness search.

Each branch is a single sign test on one pre-registered contrast. None is a hedge, and each maps to a different engineering recommendation.

---

## 2. Why now and why novel

### 2.1 Where the field is moving (Nov 2026 → NeurIPS 2027 review, July 2027)

1. **Simulator-scored improvement is being productized.** Examples: WMRL 2608.12564, AdvSim2Real 2610.08773, Trace2Env 2610.06100, Agentic WM test-time BoN 2610.40927, WRM 2610.38439, EnvOpt 2611.07372, LegoEnv 2611.01834. The novelty-killer gives ~50–60% that a loose "GEPA/harness evolution in an AWM/Trace2Env simulator transfers to real τ²/WebArena" method paper appears before May. **v2 is designed to survive that paper.** A method paper will not run the channel decomposition.
2. **"Decision-relevant fidelity" is becoming folklore.** Examples: DRPE 2609.32322, 2610.29435 (abstract only), SIRIS 2611.10329, 2606.15032 (position; unverified). By July 2027, "type not rate" is not a contribution, so v2 keeps it only as a design control.
3. **Exploitation-vs-search-pressure laws are being written for control and RL.** Examples: √KL in 2611.09670, safe horizon in 2605.15960. The *proposal channel* has no law, because planners and RL do not have one.
4. **Audits of reflective optimizers are turning to what the reflector sees, and whether it matters.** Examples: 2609.32452 (what should the reflector see?), 2610.41157 (lineage vs feedback disclosure), 2610.14662 (does failure feedback direct self-modification?), 2609.25848 (a reward-hacking framework across weights/selection/prompts, theory only). **This is the moving wave v2 rides.** Nobody has crossed it with an *erroneous* fitness source, which is where it matters most.
5. **Monitors and defenses for self-evolving loops.** Examples: HackProbe 2609.04665, CHASE/Bad Genius 2609.18366, verifier co-evolution 2610.11464 / 2610.36826. All of them act on selection. None asks whether the *proposer's input* is the leak.

### 2.2 Closest papers and the gap each leaves

"Read" means I queried the PDF this session. "RT" means read by the round-2 red team or the digest.

| # | Paper | What it shows | Gap left for v2 |
|---|---|---|---|
| 1 | **DRPE** 2609.32322, *Not All Errors Matter* (RT) | Iso-error protocol in a gridworld with an MPC planner. Decision-relevant error predicts planning (ρ=−0.84 vs −0.25 for total error). | No iterative optimizer and no reader. It pre-empts v1's "type not rate", so **v2 does not claim it.** All v2 conditions are decision-relevant and matched on δ₀. |
| 2 | **Goodhart in the Planner** 2611.09670 (RT, abstract) | Harder search cuts true return by up to 76% at constant label accuracy. Return tracks √KL. | A pure selection-pressure law with no proposal channel. **v2 reports edit distance from the seed (a √KL proxy) as a covariate** and tests whether the trace source matters *beyond* it. |
| 3 | **WRM** 2610.38439 (RT) | Predict-then-decide ≤ a direct selector. A near-chance selector hurts LLM ML-engineering search agents. | Selection only, one run per task. v2's "selected" branch would extend it, and the "taught" branch would show the selector is not the whole story. |
| 4 | **2611.01139** *WMs need not be right to rank right* (RT) | High static ranking τ, yet optimization against a frozen WM collapses (control, DMC). | Fixed policy pools plus SAC/TD3. No LLM reader. Its optimizer's-curse remark motivates v2's NOISE/UNSTEER controls. |
| 5 | **2610.08033** *Learning in Dreams, Winning in Reality* (RT) | MOBA weight RL. The policy inherits the WM's fidelity profile mechanic by mechanic. The causal intervention is named as the authors' future work. | Correlational, weights, no reader. Their likely follow-up is a per-mechanic RL intervention, which cannot produce a trace-vs-score split. |
| 6 | **Dream-RSI** 2609.14858 (RT) | RSI by replaying discovery trees. | Replay simulator with zero model error, no fidelity axis. Naming threat only. |
| 7 | **Simulator Collapse** 2608.12253 (v1) | RL against one frozen user simulator. Held-out performance peaks, then falls below base. | Weights. v2's null branch is the contrast to it. |
| 8 | **WMRL** 2608.12564 (v1) | Prompted WM replaces execution. ~10% real anchors plus isotonic debiasing. | Weights and selection-side correction only. v2 tests whether anchors are aimed at the wrong channel. |
| 9 | **2610.14662** *Does Failure Feedback Direct Agent Self-Modification?* (read) | 145 DGM edits: no target-specific repair (8.3% vs 11.6%). In controlled Qwen3-4B runs, target feedback raises repair by +20 pp. Cites Melcer et al.: wrong feedback works about as well. | The closest *conceptual* neighbour. It measures whether feedback *directs* edits under a correct evaluator. v2 asks whether *erroneous* feedback directs edits toward exploitation, with real ground truth and an across-depth outcome. It supplies both priors, so the outcome is genuinely uncertain. |
| 10 | **2609.32452** *What Should the Reflector See?* (read) | Nine evidence strategies, Qwen3.5-9B. Failures-only gives +8.0 pp. Calibration gains overstate held-out gains. | Correct evaluators only, single-turn QA, no simulator. v2's REDACT arm borrows its "no-examples" and "outcome-only" evidence controls. |
| 11 | **2610.41157** *Lineage Is Memory* (read) | 1,750 pre-registered runs. Parent inheritance outweighs feedback disclosure, and reflection is 2–15% of GEPA tokens. | Correct evaluators. It shows disclosure matters less than lineage *for gains*. v2 asks whether disclosure matters *for exploitation*. Its pre-registration style is our template. |
| 12 | **2609.25848** *Reward Hacking Across Weights, Selection, and Prompts* (read) | A framework and bounds. It argues prompt optimizers need not "understand" the defect, because selection suffices. **No LLM experiments.** | v2 is the first experiment that separates "understanding via traces" from "selection" for prompt/harness substrates. |
| 13 | **HackProbe** 2609.04665 (read) | Black-box reward-hacking monitor for a prompt-evolution host (Qwen2.5-7B) with 4 injected channels. Bandwidth-limited reselection. | Detection and reselection, i.e. selection-side. No proposal-channel manipulation, no simulated dynamics. |
| 14 | **CHASE / Bad Genius** 2609.18366 (read) | A Proposer writes benchmark-wide shortcuts into harnesses; a Challenger builds counterfactual benchmarks. | Real benchmark protocol shortcuts. No simulator, no channel split. It shows harness proposers *do* write shortcuts from traces, which supports the "taught" prior. |
| 15 | **2611.08318** *Learning the Task or the Verifier?* (digest) | Embodied RoboDojo. Exposing verifier criteria shifts behaviour, and the gap shrinks by 25.9 pts. | Verifier *criteria* exposure to the agent, not simulator-trace exposure to an evolver. Closest prior for "legibility". |
| 16 | **SIRIS** 2611.10329 (RT) | Perturbs a simulator to ask whether an error overturns candidate selection. | No evolution loop. A methodological cousin of the quirk layer. |

**K1 check: no paper found** that runs harness or prompt evolution with simulated fitness and crosses the reflector's trace source with the fitness source, or manipulates the textual legibility of a simulator error at matched outcome-error rate. Searches this session (alphaXiv, recency): reflective-optimizer exploitation of feedback text; proposal vs selection decomposition of reward hacking; harness evolution in simulated environments. Plus the four PDF deep reads above. **K4 check:** v2 is not "X + Y combined". It introduces a manipulation (the trace-source swap) that has no meaning outside reading optimizers.

**Unchecked:** OpenReview-only ICLR 2027 submissions; openreview.net is blocked from this session. Re-check search terms: "reflector" × "simulator"/"world model"; "prompt optimization" × "simulated environment"; "proposal" × "selection" × "reward hacking".

---

## 3. Research questions

- **RQ1 (primary, channel decomposition).** Under a simulator with steerable, decision-relevant errors, how much real held-out loss comes from the trace channel (what the reflector reads) versus the selection channel (what the simulated score picks)? Primary contrast: G_SR − G_RS.
- **RQ2 (causal lever: legibility).** Holding error mechanics, rate and δ₀ fixed, does a quirk that leaves a textual marker in the trace get exploited faster than the identical silent quirk? Readouts: real loss, and the quirk-exploitation rate (QER) slope over depth.
- **RQ3 (controls: is it exploitation at all?).** Does the loss exceed what the optimizer's curse alone produces? Compare against NOISE (symmetric label flips) and UNSTEER (random one-sided false passes), both matched on δ₀. Positive control: Q5 lenient judge.
- **RQ4 (practical, small).** Does REDACT recover the loss at near-zero cost, in two variants: (a) the reflector sees actions and binary outcomes only; (b) it sees real-env traces on a 3-task reflection minibatch. Benchmark against a 10% real-anchor selector (WMRL/WRM-style). And does it transfer to one natural LLM simulator?

**Dropped from v1:**
- "type not rate" / iso-accuracy pairs
- τ_path as a predictor (circular and dose-confounded, per the sharpness reviewer §2.3)
- the 6-metric shoot-out
- D2 web
- natural simulators S2/S3
- mitigations M1/M2/M4
- the API-agent replication

---

## 4. Design

### 4.1 Setup (one domain, deterministic ground truth)

- **Domain: procedural τ-retail.** Python DB environment with τ²-bench retail-style tools. Dan's planned generator samples a DB state, a goal and a policy rule, and yields a verified goal end-state.
  - **Single-instruction mode:** the full goal is given up front, with no LLM user simulator. The "real" environment is then fully deterministic and contains no simulated component, which removes the "real env already has an LLM user" confound.
  - Pool: 150 evolution tasks, 50 validation tasks (GEPA Pareto set), and **300 held-out tasks** used only for real checkpoint evaluation.
  - Fallback if the generator slips: the existing τ²-bench retail tasks with goals made explicit. The pilot uses this.
- **Task agent:** Qwen3.5-9B (vLLM, self-hosted). Seed harness deliberately minimal: a one-paragraph system prompt and no verification routines, so there is headroom.
- **Evolver E1 (primary):** GEPA-style reflective evolution with a Pareto archive and minibatch acceptance gate. Reflector: Haiku-class API model. Depth 40 iterations, with extension to 60 for the 2×2 cells only if the pilot shows late onset.
- **Evolver E2 (replication):** a Meta-Harness/DGM-lite code-level evolver that edits a Python harness of tool wrappers and checks. Coder: Haiku-class. Two conditions × 3 seeds.
- **Simulator = quirk layer over the real environment.** The oracle transition function is the real environment, so the ORACLE simulator is identical to REAL. A wrapper injects labelled quirks, and every firing is logged. This is the BreakingWeb intervention idea applied to the fitness source.

### 4.2 Quirks (all steerable and decision-relevant; cosmetic quirks dropped)

| Quirk | Mechanics in sim (real env errors) | Legible variant (marker in tool observation) | Silent variant |
|---|---|---|---|
| **Q1 lenient validation** | `exchange/modify_items` with an out-of-stock or invalid item, or an unverified payment method, succeeds and updates the DB | `{"status":"ok","note":"availability check skipped"}` | `{"status":"ok"}`, byte-identical to a valid success |
| **Q2 missing error state** | A policy-violating action (cancel non-pending order, refund to wrong method) returns success | `{"status":"ok","warning":"policy check bypassed"}` | `{"status":"ok"}` |
| **Q5 lenient judge** (positive control only) | Sim success = end state within edit distance k of the goal | Judge feedback "partial match accepted" | — |

- **Steerability** is satisfied by construction. A quirk fires only when the agent takes the eligible (invalid) action, so a harness edit such as "skip availability checks; submit directly" raises its trigger rate.
- **Matching variable: δ₀ = P(sim success ≠ real success | h₀)**, the base-policy outcome disagreement. This follows the sharpness reviewer §2.2. Eligibility sets are tuned so that δ₀ ≈ 0.15 for every quirk condition, NOISE and UNSTEER (±0.02, verified on 200 base rollouts). One dose condition sits at δ₀ ≈ 0.30.
- Legible and silent variants may shift agent behaviour at d=0. δ₀ is therefore matched *per variant*, and the agent-level reaction to markers is reported separately.

### 4.3 The 2×2 instrument (fitness source F × reflector trace source T)

In GEPA, each iteration (i) runs the parent on a 3-task reflection minibatch and shows those traces to the reflector, and (ii) scores the child on the gate minibatch and the Pareto validation set. We route (i) and (ii) independently:

| Cell | Reflector reads traces from | Fitness (gate + Pareto validation) from | What it isolates |
|---|---|---|---|
| **SS** | quirk sim | quirk sim | the standard "evolve in a simulator" setup |
| **SR** | **real** env (3 tasks/iter) | quirk sim | selection channel only |
| **RS** | quirk sim | **real** env | trace (teaching) channel only |
| **RR** | real | real | real-env evolution (= ORACLE = REAL-count) |

Real gains: G_FT(d) = S_real(h*_d; T_h) − S_real(h₀; T_h), where h*_d is the harness the loop would ship at depth d.
- **Loss:** L(d) = G_RR(d) − G_SS(d).
- **Teaching effect:** τ_teach = ½[(G_SR − G_SS) + (G_RR − G_RS)]. **Selection effect:** τ_sel = ½[(G_RS − G_SS) + (G_RR − G_SR)].
- The difference τ_teach − τ_sel equals **G_SR − G_RS** algebraically. The primary hypothesis is therefore a single two-cell contrast.
- Teach share = τ_teach / (τ_teach + τ_sel), reported with a bootstrap CI and only when L's CI excludes 0.

**Information confound and its fix.** In T=R cells the reflector also sees *genuine* error messages, such as "out of stock", which are informative independently of exploitation. That is part of "teaching" broadly. Sim traces mis-teach in two ways: by **concealing** real errors and by **revealing** exploits. RQ2 separates the two:
- Legible and silent sim traces conceal real errors equally. They differ only in revealing the exploit.
- So G_SS(silent) − G_SS(legible) measures *exploit revelation* at fixed concealment.

### 4.4 Arms, seeds, budget

| Arm | Seeds | Purpose |
|---|---|---|
| SS-legible (Q1+Q2 mixture, δ₀≈0.15) | 8 | 2×2 cell / RQ1 |
| SR | 8 | 2×2 cell |
| RS-legible | 8 | 2×2 cell |
| RR (= ORACLE) | 8 | 2×2 cell; headroom |
| SS-silent | 8 | RQ2 legibility contrast |
| SS-REDACT (reflector sees actions + binary sim outcome only) | 5 | RQ4 zero-cost mitigation |
| RR-REDACT | 4 | Cost of redaction when the evaluator is correct |
| SS-ANCHOR10 (10% of Pareto evals in real; selection by anchored score) | 4 | RQ4 selection-side comparator (WMRL/WRM-style) |
| NOISE (symmetric i.i.d. label flips, δ₀-matched) | 4 | RQ3 optimizer's-curse control |
| UNSTEER (one-sided false passes on random episodes, δ₀-matched) | 4 | RQ3 unsteerable-error control |
| Q5 lenient judge (legible) | 4 | Positive control: the evolver *can* exploit |
| SS-legible at δ₀≈0.30 | 4 | Dose check |
| **Subtotal E1 (Q-quirk sim)** | **73 runs** | |
| Natural sim: prompted Qwen3.5-27B world model (same family as agent: "self-dreaming"); cells SS / SR / SS-REDACT | 3 each = 9 | Ecological check of RQ1/RQ4 |
| E2 code-level evolver: SS-legible vs SR | 3 each = 6 | Evolver generality, "steering budget" |
| **Total** | **88 runs** | |

**Real checkpoint evaluation.** h*_d is evaluated at d ∈ {0, 10, 20, 30, 40} on all 300 held-out tasks: 1 rollout at intermediate checkpoints, 2 at d=40. d=0 is shared across runs.

### 4.5 Metrics (formal)

- G_FT(d; T_h), L(d), τ_teach, τ_sel: as defined in §4.3. Also G_sim(d) = S_sim(h*_d) − S_sim(h₀).
- **QER(h)** = P(sim success of h involved ≥1 logged quirk firing) − the same quantity for h₀. This is a mechanism readout, logged rather than judged. Prediction under "taught": d(QER)/dd is larger in SS-legible than SS-silent, and ≈0 in SR until selection finds exploits by chance.
- **Quirk-citation rate (QCR):** the fraction of accepted edits whose text references the quirk behaviour. Measured by regex (e.g. skip/bypass/without checking availability) plus an LLM judge with 100 human-checked spot labels. Reported with κ. This is the directedness readout, a cousin of 2610.14662's "directedness" measure.
- **Edit distance from seed** (normalized token Levenshtein on harness text) as a √KL proxy, following 2611.09670. Used as a covariate: does trace source predict loss beyond how far the harness moved?
- **Cost:** API $ and task-agent rollouts per cell, reported for every arm. SR costs about 3 real rollouts per iteration (~10–15% of RR's real rollouts). ANCHOR10 is cost-matched to SR within ±20%, so the RQ4 comparison is compute-matched.

### 4.6 Pre-registered hypotheses and analysis

Registered on OSF before the main runs (target 20 Dec 2026).

- **H0 (gate):** L(d) > 0 at pooled d ∈ {20,30,40}. If not, H1 is not tested, and we report the null branch with Q5 as the positive control.
- **H1 (primary):** G_SR − G_RS > 0. Mixed model: G_real ~ cell × depth + (1|seed) + (1|task) on task-level paired outcomes, d ∈ {20,30,40}. One-sided α=0.05. Alternative-branch test: G_RS − G_SR > 0, two one-sided tests. If both are non-significant, TOST equivalence with margin ±4 pp gives the "both channels" report.
- **H2:** G_SS(silent) − G_SS(legible) > 0, and the QER slope is greater in legible (Holm-corrected across the two).
- **H3:** the decline in SS-legible exceeds NOISE and UNSTEER, measured as the [G(40) − G(10)] contrast. If NOISE and UNSTEER decline equally, the "loss" is the optimizer's curse and is reported as such.
- **H4 (secondary):** SS-REDACT recovers ≥50% of L, and its recovery ≥ ANCHOR10's at matched cost.
- All other analyses are labelled exploratory: E2, natural simulator, dose, QCR, and the edit-distance covariate.
- No argmax over d_peak and no uncorrected per-checkpoint tests (fixes sharpness §2.5).

### 4.7 Power analysis

Assumptions, to be confirmed by the pilot:
- base success ≈ 0.35
- per-run real-gain SD from seed variance σ_seed ≈ 4 pp
- task-sampling SE of a paired gain on 300 tasks × 1 rollout ≈ 3 pp (discordant fraction ≈ 0.3)
- per-run σ_run ≈ 5 pp, falling to ≈ 4.2 pp when pooling 3 correlated checkpoints in the mixed model

| Contrast | n per cell | SE | MDE (80% power, one-sided α=.05) |
|---|---|---|---|
| H1: G_SR − G_RS | 8 vs 8 | 4.2·√(2/8) = 2.1 pp | **≈ 5.2 pp** |
| H2: SS-silent − SS-legible | 8 vs 8 | 2.1 pp | ≈ 5.2 pp |
| H3: SS-legible decline vs NOISE | 8 vs 4 | 4.2·√(1/8+1/4) = 2.6 pp | ≈ 6.4 pp |
| H4: REDACT vs SS | 5 vs 8 | 2.4 pp | ≈ 6.0 pp |

Interpretation:
- If L ≈ 15 pp (Q5-like exploitation), a pure "taught" world gives G_SR − G_RS ≈ 15 pp, and a 70/30 teach split gives ≈ 6 pp.
- So **the design detects a ≥70/30 split when L ≥ 15 pp.** The pilot gate (§6) requires L ≥ 8 pp at d=20, i.e. an expected L ≈ 12–20 pp by d=40.
- If the pilot shows σ_seed > 5 pp, add seeds to the four 2×2 cells (8 → 12, +16 runs, ~+$150), not new conditions.

### 4.8 Budget

| Item | Estimate |
|---|---|
| Rollouts per run | ~1,050 in-loop (≈26/iter × 40) + ~1,800 real checkpoint ≈ 2,900 |
| Total rollouts | 88 runs × 2,900 ≈ 255k (τ-retail, ~15 turns, ~40k cumulative input tokens with prefix caching) |
| Task agent (Qwen3.5-9B) | ≈ 10B input / 0.5B output tokens. Self-hosted: ≈ 200 H100-h. Or hosted at ≈ $0.05/$0.20 per M: ≈ $600 |
| Reflector/coder (Haiku-class) | 88 × 40 iters × ~25k in / 2k out ≈ 90M in / 7M out ≈ **$150** (with slack) |
| Natural simulator (Qwen3.5-27B self-hosted) | ≈ 40 H100-h |
| QCR judge + spot checks | ≈ $50 |
| Pilot | ≤ $60 |
| **Total** | **≈ $300 API + ≈ 250 GPU-h** (self-hosted), or **≈ $900 API** fully hosted. Within the ≤$3k envelope with large slack for reruns |

---

## 5. Figure 1 and key plots

- **Figure 1 (concept + headline).**
  - Left panel: the GEPA loop drawn with two coloured pipes from the simulator to the evolver: the *trace pipe* into the reflector and the *score pipe* into selection. A switch on each pipe can be flipped to the real environment, giving the four cells.
  - Right panel: the key result. Real held-out gain vs depth (0–40) for SS, SR, RS and RR with 95% CIs.
  - Predicted under "taught": SR hugs RR while RS sinks toward SS. Inset bar: τ_teach vs τ_sel with CI.
  - Under "selected" the lines swap pairs. The figure reads correctly either way.
- **Figure 2 (legibility).** QER vs depth for SS-legible, SS-silent, SR, NOISE and UNSTEER. A secondary axis shows the quirk-citation rate of accepted edits. Prediction: legible rises within ~10 iterations, silent rises later or not at all, and NOISE/UNSTEER stay flat.
- **Figure 3 (is it exploitation?).** [G(40) − G(10)] for SS-legible, SS-silent, NOISE, UNSTEER and Q5. Separates exploitation from the optimizer's curse, with Q5 marking "the evolver can exploit".
- **Figure 4 (practitioner Pareto).** Real held-out gain vs real-env cost (fraction of RR) for SS, SS-REDACT, SR, ANCHOR10 and RR, with the natural-simulator points overlaid as hollow markers.
- **Table 1.** The 2×2 decomposition with CIs at d ∈ {20, 30, 40}, plus the edit-distance-adjusted estimate. **Table 2.** E2 replication and natural-simulator replication of the primary contrast.

---

## 6. One-week pilot (≤ $60) with go / pivot / kill

**Setup.**
- Existing τ²-bench retail tasks, made single-instruction: ~60 evolution/validation and 100 held-out.
- Qwen3.5-9B agent, GEPA, Haiku-class reflector. Depth 20, 1 seed per arm.
- Arms: RR (×3 seeds, to estimate σ_seed), SS-legible, SS-silent, SR and Q5.
- Day 1–2: build the quirk layer plus the trace/fitness router, and verify δ₀ matching on 200 base rollouts.
- Day 3–6: runs. Day 7: analysis.

| Signal | Go | Pivot | Kill |
|---|---|---|---|
| Headroom: G_RR(20) | ≥ 8 pp with σ_seed ≤ 4 pp | 4–8 pp → weaker seed harness, harder procedural tasks, or depth 60 | < 4 pp after one fix → no headroom; drop the harness-evolution framing (see below) |
| Loss: L(20) = G_RR − G_SS-legible | ≥ 8 pp | 3–8 pp → raise δ₀ to 0.25 or widen eligibility | ≈ 0 **and** Q5 also ≈ 0 → evolver too weak; switch to E2 or a stronger reflector before concluding |
| Positive control Q5 | real gain declines vs RR by ≥ 8 pp | — | No decline → fix the Q5 design (the evolver must be able to exploit something) |
| Channel hint | SR vs SS-legible gap ≥ 5 pp, or ≤ −5 pp: either sign is fine (it tells us which branch to power) | \|gap\| < 3 pp → still go, add seeds to the 2×2 | — |
| Mechanism | QER rising in SS-legible | QER flat → quirks not reached; raise eligibility | — |
| δ₀ matching | all conditions within ±0.02 | ±0.05 → retune eligibility | cannot match (legible markers change base behaviour by >0.1) → move legibility from the observation to the evaluator-feedback text only (reflector-only channel) |

**Fallback pivot if headroom is killed:** "Proposal vs selection in reflective prompt optimization with corrupted evaluators" on single-turn tasks, where GEPA has known headroom. Use the same 2×2 with LLM-judge errors instead of simulator dynamics. Novelty would be lower (closer to HackProbe and 2609.32452), but it can still be finished.

---

## 7. Timeline (Nov 2026 – May 2027)

| When | Milestones |
|---|---|
| Nov 13–30 | Pilot (§6). Quirk layer plus router. Decide go/pivot. Meet Xunjian: BreakingWeb intervention code, OSF pre-registration review. |
| Dec 1–20 | Procedural τ-retail generator (500+ tasks with verifiers). δ₀ calibration for all conditions. **Pre-register on OSF (Dec 20).** |
| Dec 21 – Jan 31 | The 2×2 cells plus SS-silent (40 runs). Interim blinded check of H0 only (gate). |
| Feb 1–28 | Controls (NOISE, UNSTEER, Q5, dose), REDACT, ANCHOR10 (25 runs). Primary analysis frozen. |
| **~Mar 5** | **Preprint v1 on arXiv** with the core 2×2 plus legibility result. This stakes the claim ahead of the expected "simulator-scored GEPA" method papers. |
| Mar | Natural Qwen3.5-27B simulator (9 runs). E2 replication (6 runs). QCR judge validation. |
| Apr | Figures, writing, internal review (Xunjian, Shuyan). Re-check OpenReview/ICLR 2027 and the citers of 2610.14662, 2611.01139 and 2610.08033. |
| May 1–15 | Robustness reruns (extra seeds on any borderline contrast). Release the quirk-layer + router toolkit. Submit. |

Slack: about 3 weeks. If the schedule slips, cut E2 first, then the natural simulator. The 2×2 + legibility + controls alone are the paper.

---

## 8. Reviewer objections and rebuttals

1. ***"This is DRPE / 2611.01139 / 2610.08033 for harnesses."*** No. We make no claim about error type or ranking fidelity, and every condition is decision-relevant and δ₀-matched. Our manipulated variable, *which channel carries the simulator's output into the optimizer*, does not exist for planners, best-of-n or weight RL. The trace-source swap is undefined for them.
2. ***"Quirks are artificial bugs, not world-model errors."*** They are the causal instrument, and they are modelled on documented LLM-simulator failures: fictitious success (Trace2Env, replay 3% vs 93% real) and over-cooperative environments (StressSim 2610.35426). The natural Qwen3.5-27B simulator replicates the primary contrast ecologically. Its legibility is natural, since LLM simulators narrate.
3. ***"The decline is just the optimizer's curse."*** That is what NOISE and UNSTEER control for at matched δ₀ (H3). If they decline equally, we report that, and it is a finding.
4. ***"T=R helps because real traces are more informative, not because sim traces teach exploits."*** Correct, and we split it. Legible vs silent holds error concealment fixed and varies only exploit revelation (RQ2). The edit-distance covariate controls for "the harness simply moved further".
5. ***"RS and SR need real evaluations, so this is impractical."*** They are an instrument, not a method. The practical outputs are REDACT (zero cost) and the REDACT vs ANCHOR10 comparison at matched cost.
6. ***"GEPA with a Haiku reflector is too weak, so any null is uninterpretable."*** The Q5 positive control must show exploitation (pilot gate). E2, a code-level evolver, is the strength replication.
7. ***"One domain and one agent model."*** Accepted as a scope limit and stated in the title's scope ("reflective harness evolution"). The claim is about a channel inside the optimizer, which is domain-agnostic by construction. E2 and the natural simulator are within-paper generality checks. A second domain (BreakingWeb sandboxes) is follow-up work with Shuyan.
8. ***"2610.14662 already shows feedback doesn't direct DGM edits."*** It studies a *correct* evaluator, measures single-step repair, and its controlled study finds directedness in Qwen3-4B. Its mixed result is exactly why our outcome is uncertain. We test the case that matters for safety and cost: *erroneous* feedback over many iterations, with real ground truth.
9. ***"Pre-registration with ~88 runs from an undergrad?"*** The design is one primary contrast, a pilot-calibrated power analysis and a frozen analysis script, following the template of 2610.41157 (1,750 pre-registered runs).

---

## 9. Fit with Xunjian (Gödel Agent, WeAct, BreakingWeb, world models)

- **Gödel Agent (2410.04444)** is the archetypal *reading* improver: it inspects its own code and traces and rewrites itself. v2 asks the question that self-referential improvers uniquely raise: when the world it reads is a model, does the improver learn the model's lies by reading them? The "taught vs selected" decomposition is a direct diagnostic for Gödel-style loops, and E2 is Gödel-adjacent.
- **BreakingWeb (2609.35814):** controlled, labelled environment interventions. v2 points that methodology at the *fitness source*. The legible/silent manipulation is a BreakingWeb-style perturbation of what the agent and the evolver can observe. Xunjian's false-success phenomenon (agents claiming success, 54–85%) is the Q5 channel.
- **World models:** the object of study is the world model used as an improvement signal, which the field is productizing now. v2 gives world-model builders a concrete rule: *fidelity only matters on the channels the optimizer consumes*. If "taught" holds, hide simulator narration from reflectors. If "selected" holds, spend fidelity on outcome correctness of agent-reachable transitions.
- **WeAct (2610.26847):** future work. When weight-switching is an action, does a simulator-scored loop learn to switch toward weights that exploit the simulator? Out of scope.
- **What Xunjian brings:** intervention design (BreakingWeb), loop design (Gödel Agent), pre-registration review. **What Shuyan brings:** the follow-up web domain and verifier design.

---

## 10. Rubric rescore (honest)

| Dim | W | v1 self | Red team (avg) | **v2** | Justification |
|---|---|---|---|---|---|
| N | 3 | 3.5 | 2.5–3.5 | **3.5** | No paper crosses trace source × fitness source, or manipulates legibility at matched δ₀, in a simulator-scored loop. But the neighbourhood is dense and moving fast: 2610.14662, 2609.32452, 2610.41157, 2609.25848, HackProbe, CHASE. A fast follower from the "reflector evidence" line is plausible (~20–25% before May). Not a 4 until the preprint is out. |
| S | 2.5 | 3.5 | 2.5–3.0 | **4.0** | One sign test, and both signs overturn something. "Taught" overturns the selection-pressure account that every 2026 paper assumes. "Selected" says reflection is inert for exploitation, contradicting the GEPA-style rationale. The null branch contrasts with weight-RL collapse. Priors are genuinely split (2610.14662 vs Melcer). |
| R | 2 | 4.5 | 4.5 | **4.5** | Self-referential improvers reading their world, world models, BreakingWeb interventions. |
| C | 2 | 4.0 | 3.0–3.5 | **3.5** | Pre-registered single primary contrast, 8 seeds per core cell, δ₀ matching, NOISE/UNSTEER/positive controls, compute-matched mitigation comparison. Held back by one domain and one agent model, and by a reviewer who may still call it a "measurement paper". |
| F | 1.5 | 3.0 | 2.5–3.0 | **3.5** | 88 runs, one domain, ≈$300–900 API, ≈250 GPU-h. Pilot in a week for <$60. Main risk: the procedural generator and the headroom gate. |
| T | 1 | 4.0 | 4.0 | **4.0** | Simulator-scored improvement and audits of reflective optimizers are both rising. Pre-empts the "simulator-scored GEPA" wave rather than riding it. |

**Total = 20 × (3·3.5 + 2.5·4.0 + 2·4.5 + 2·3.5 + 1.5·3.5 + 1·4.0) / 12 = 20 × 45.75 / 12 ≈ 76.3.** This is the upper edge of "viable with fixes" and just into "strong".

Downside case: the pilot shows L < 8 pp, or Q5 fails. Then the paper falls back to the corrupted-judge prompt-optimization pivot (§6), which scores ≈ 62–65 (N 2.5, S 3.5).

Upside case: a clean "taught" result with REDACT recovering most of the loss and replicating on the natural simulator. That reads as a 6–7/10 NeurIPS paper with an immediately adoptable recommendation.

**Kill flags:**
- K1: none found (OpenReview unchecked).
- K2: cleared; every branch is publishable and a positive control makes the null readable.
- K3: cleared; open 9B agent plus a Haiku-class reflector.
- K4: cleared; the trace-source swap is a new manipulation, not a combination.

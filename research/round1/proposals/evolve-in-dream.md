# Evolving in a Dream: When Does Simulator-Scored Harness Evolution Transfer to Real Environments?

*Proposal: Dan, for Xunjian Yin. Target: NeurIPS 2027 main track (deadline ~mid-May 2027). Drafted 2026-11-12 after critique round 1 (71/100).*

---

## 1. Title and headline claim

**Working title:** *Evolving in a Dream: Controlled Simulator Errors Reveal When Harness Evolution Exploits World Models*

**Headline claim (expected outcome).** When harness evolution is scored inside an LLM world model, real-environment gain rises, peaks and then declines as evolution goes deeper. Which simulator errors cause the decline is set by their *type*, not by how often they occur:
- Decision-relevant errors (lenient validation, missing error states, a lenient success judge) are exploited.
- Cosmetic errors at the same next-observation accuracy are not.

So two simulators with *identical* next-observation accuracy can produce transfer curves that differ by more than 10 pp. On-path harness-ranking fidelity predicts where the real gain peaks; next-observation accuracy does not. On-path ranking fidelity is the sim–real Kendall τ over the candidates the evolver is producing *at that depth*. Simulator ensembles with sparse real anchors recover most of real-environment evolution at a fraction of its cost.

**Alternative-outcome claim (also publishable).** Harness evolution does *not* Goodhart on LLM simulators within practical depths (≤60 iterations), even at 20% injected decision-relevant error rates. This would be a sharp contrast with weight-level RL against frozen simulators (2608.12253 simulator collapse; 2611.01139 "model-blind collapse"). One explanation would be that the harness edit space is too coarse, or too constrained by the frozen base model, to find quirk-exploiting policies. In that case the paper becomes a validated licence for cheap simulator-scored harness search, plus the audit protocol (quirk injection + on-path ranking fidelity) that certifies when the licence holds. The quirk-trigger logs then show whether evolved harnesses *encounter* quirks without *selecting for* them.

A third possibility is a mixed result: some quirk types are exploited and others are not. That is the most informative outcome, and the dose-response design is built to resolve it.

---

## 2. Motivation and why now

1. **Simulator-scored self-improvement became mainstream in 2026.**
   - DreamGym (2511.03773), Agent World Model (2602.10090) and Qwen-AgentWorld train agents inside LLM world models.
   - AdvSim2Real (2610.08773) trains web agents inside a frozen WebWorld-14B.
   - WMRL (2608.12564, Amazon/UIUC) replaces execution with a prompted world model for RL of research agents and reports a 3–4× speedup.
   - Agentic World Modeling (2610.40927) uses a world model for test-time Best-of-N selection.
   - Trace2Env (2610.06100) builds "agentic language world models" from traces as replacements for real environments.
   The simulator is no longer a toy. It is becoming the fitness function.
2. **Harness evolution is the cheapest self-improvement lever, and its cost is environment calls.** GEPA, Meta-Harness, DarwinX, AHE, HarnessX and others spend most of their budget evaluating candidates. Scoring candidates in a world model is the obvious next step (DarwinX already scores with an LLM judge on synthetic intents). Nobody has asked what happens to *harness search* when the environment dynamics themselves are simulated.
3. **The 2026 papers frame the open question but do not answer it.**
   - 2608.12253 shows a Goodhart curve for RL against one frozen *user* simulator.
   - 2611.01139 (31 Oct 2026) shows, for continuous control, that a world model can rank fixed policies well (τ=0.80–0.96) while optimisation against it still lowers real return. It also shows ranking, calibration and "audit utility" are distinct properties.
   - 2605.15960 proves that exploitation is *inevitable* on rich policy sets and gives a "safe horizon".
   - Trace2Env shows prompted language world models let agents "succeed" in fictitious states: replay success is 3% versus 93% real on ALFWorld.
   - AdvSim2Real and DarwinX show *positive* sim/synthetic-to-real transfer at shallow depth.

   Theory says exploitation is inevitable eventually. Empirics say transfer is good early. The open question is *when and through which errors* harness search exploits a simulator, and how a practitioner can tell beforehand. That is a depth × error-type question, and no current paper runs it.
4. **Evaluation-rigor momentum.** Rethinking Harness Evolution (2607.12227) and RRSI (2609.24972) show that harness evolution often fails to beat matched-compute baselines and keeps little held-out gain *even in real environments*. Any sim-to-real claim therefore has to separate simulator Goodhart from ordinary task-set Goodhart. Dan's preferred style (matched compute, attribution arms) fits this exactly.

---

## 3. Core research questions

- **RQ1 (Goodhart curve).** As evolution depth increases, how does real-environment gain of simulator-evolved harnesses evolve relative to simulator gain? Is there a peak, and where?
- **RQ2 (causal error attribution).** Which simulator error types does harness evolution exploit? Holding next-observation accuracy fixed, does error *type* change transfer? Is the dose-response in error *rate* monotone?
- **RQ3 (predictive fidelity metric).** Does on-path harness-ranking fidelity (sim–real τ over the candidates the evolver currently generates) predict the transfer peak better than next-observation accuracy, static ranking fidelity over hand-written harnesses, or simulator task-success agreement?
- **RQ4 (cheap mitigation).** At matched dollar cost, how much of real-environment evolution do these mitigations recover?
  - simulator ensembles
  - ensemble-disagreement early stopping
  - sparse real anchors (including WMRL-style isotonic debiasing)
  - quirk randomisation

---

## 4. Related work and positioning

| Paper | What it does | Gap it leaves / how we differ |
|---|---|---|
| **2608.12253** Simulator Collapse: One Frozen Simulator Is Not Enough (Aug 2026) | RL of a 4B policy against one frozen LLM *user* simulator. Training reward rises while held-out and human performance peak and then fall below base. Fixed with Verbalized Sampling and population co-training. | Weights, user simulator, no fidelity manipulation. **We:** harness evolution against an *environment* world model, causal error injection, and the question of whether harness search Goodharts earlier, later or not at all. Their population fix is one of our mitigation arms. |
| **2611.01139** World Models Need Not Be Right to Rank Policies Right (31 Oct 2026) | DMC control with 45 SAC/TD3/PPO policies. World model ranks fixed policies well (τ=0.96/0.80), but optimising against the frozen model lowers real return ("model-blind collapse"). RAVEL uses real audits for selection. Separates ranking, calibration and audit utility. | *Closest conceptual pre-emption of "ranking fidelity"*. But: fixed policy pools rather than an adaptive search trajectory, continuous control, no LLM agents, no error-type manipulation. **We:** measure *on-path* ranking fidelity along a harness-evolution trajectory and test whether it predicts the transfer peak. We cite their three-way distinction and extend it to LLM self-improvement. |
| **2605.15960** Imperfect World Models are Exploitable (May 2026, v3 Nov) | Ordinal definition of model exploitation, proof that it is unavoidable on rich policy sets, and an ε-safe horizon. | Theory only (finite MDPs). **We:** give an empirical measurement of *when* exploitation shows up for a discrete, LLM-generated harness search. Its inevitability theorem is our prior. |
| **2610.08773** AdvSim2Real (Oct 2026) | Co-evolves curriculum, adversary and a 4B executor inside frozen WebWorld-14B. Transfers to real Chromium (25.6→44.4), monotone over 3 rounds. | Weights. Three rounds (no depth sweep), one simulator (no fidelity axis). Its positive shallow transfer is a foil: we predict agreement at shallow depth and divergence later. |
| **2608.07545** DarwinX (Jul 2026) | Harness evolution on 300 synthetic intents with an LLM-judge fitness. Transfers to 1,260 real WebArena-Infinity tasks (43.5→93.0). Notes a proxy-overfit gap on TerminalWorld. | Tasks and judge are synthetic, but the environment dynamics are real. No depth-dependent transfer curve, no simulated dynamics. Our "judge leniency" quirk isolates its proxy channel. |
| **2608.12564** WMRL: Scaling AutoResearch Agents via World Models (Aug 2026) | Prompted world model replaces execution for GRPO. About 10% anchor groups graded in real; isotonic online debiasing plus inverse-variance denoising. Matches real-execution RL at 3–4× less compute. | Weights; bias/noise model of reward error with no error *types*; no depth curve. **We:** reuse their anchor + isotonic debiasing as a mitigation baseline for harness search. Our quirk injection tests their assumption that monotone recalibration suffices, which fails if errors are policy-specific. |
| **2610.06100** Trace2Env (Oct 2026) | Agentic language world model built from traces. Improves next-observation fidelity. World-to-real replay consistency ratio (CR) rises from 0.03 to 0.91 on ALFWorld. | Evaluates fixed task agents, with no optimisation loop against the simulator. **We:** use Trace2Env-style worldbook simulators as a natural fidelity rung, and their W2R/CR replay metric as one of our competing fidelity metrics. |
| **2610.40927** Agentic World Modeling for Test-time Scaling (Oct 2026) | Read-grounded LLM simulator for Best-of-N without a replica. Compares SIMIA, Qwen-AgentWorld and AWM against real outcomes. | Uses the world model for selection at test time, with no iterative optimisation. **We:** use its ladder as natural simulators. |
| **2610.19375** One-Step Prediction Fidelity Is a Poor Proxy for Planning (Oct 2026); **2609.33335** WM post-training audit (Sep 2026); Lambert et al. 2020 objective mismatch | Prediction accuracy ≠ downstream usefulness, shown for JEPA/MPC and for WM post-training of weights. | *We do not claim this as new.* Our contribution is the **iso-accuracy causal test** (same next-observation accuracy, different error types) inside an LLM self-improvement loop, and a *replacement* metric validated against transfer peaks. |
| **2608.21898** Training Needs Trustworthy Worlds (Aug 2026) | Verified versus raw synthetic web environments; verified ones transfer better (PPO). | Binary validity, weights. We make validity a graded, typed dose. |
| **2607.12227** Rethinking the Evaluation of Harness Evolution; **2609.24972** RRSI | Real-environment harness evolution loses to matched-budget sampling, and held-out gain is about 0. | Shows task-set Goodhart *without* simulators. **We:** include real-environment matched-count and matched-cost arms so simulator Goodhart and task-set Goodhart are separated. |
| **2610.18309** Prompt or Policy? (Oct 2026) | GEPA versus GRPO against LLM patient simulators. Gains persist under an unseen judge. | Single-turn, a user simulator rather than environment dynamics, a single optimisation budget. A small positive data point for prompt-level transfer. |

Also relevant, read at abstract level or via others' reference lists:
- GAUGE (2609.12191): when not to trust LLM-judge plus user-simulator gates for agent selection.
- StressSim (2610.35426): over-cooperative user simulators overestimate agents. This is our "over-cooperative UI" quirk on the user side.
- "How should world models be evaluated? A decision-making-centric position" (2606.15032): **unverified**, seen only in 2611.01139's references.
- "Beyond state consistency: behavior consistency in text-based WMs" (2604.13824): **unverified**, seen only in Trace2Env's references.

**Scoop status (searched 2026-11-12):**
- alphaXiv recency searches:
  - harness/prompt optimisation scored by world models or simulators
  - ranking fidelity of world models
  - self-evolving agents with simulated experience
  - world-model exploitation
- S2 keyword search and the citations of 2608.12253.

No paper evolves a harness or prompt against an environment world model and sweeps depth × error type/rate against real ground truth. Mid-2026 was dense in this area, so the risk of a scoop by May 2027 is moderate (see §9).

**How we differ, in one line:** prior work either *optimises weights* against simulators (2608.12253, AdvSim2Real, WMRL), *evaluates fixed policies* with simulators (2611.01139, 2610.40927, Trace2Env), or *evolves harnesses* against real environments with synthetic tasks (DarwinX). We are the first to run *harness evolution × simulated dynamics × controlled typed error dose × depth*, with attribution arms and a fidelity metric validated against the transfer peak.

---

## 5. Method and experimental design

### 5.1 Environments (ground truth must be deterministic and self-hosted)

| Domain | Real ground truth | Task pool | Why |
|---|---|---|---|
| **D1: Tool-use / customer service** | τ²-bench-style retail and airline Python DB environments, extended with Dan's planned procedural τ-style task generator (sample DB state + goal + policy rule → verified end state). | 300 tasks (150 evolution-pool "seen" + 150 held-out), more generated as needed. | Cheap and deterministic. Existing simulators (SIMIA-style, AWM-style) are already in the literature. Reuses Dan's infrastructure plan from ideas_so_far. |
| **D2: Web** | 2 self-hosted web apps in text/accessibility-tree mode, ideally Xunjian's BreakingWeb sandboxes (2609.35814), or WebArena-style shopping/forum apps as fallback. | ≥150 tasks per site with deterministic DB-state verifiers (75 seen + 75 held-out per site, 300 total). | Mentor fit. Web dynamics are where LLM simulators are being deployed (AdvSim2Real, WebWorld). |

Live sites appear only as a one-figure sanity check (20 tasks), never as ground truth.

### 5.2 Simulators: two families

**(a) Controlled quirk simulators (the causal instrument).** Start from an *oracle simulator*: the real environment's own transition function, which by construction has 100% next-observation accuracy. Wrap it with a **quirk layer** that fires labelled errors at rate ρ on the eligible transitions. This is BreakingWeb's controlled-intervention method turned on the simulator rather than on the task.

| Quirk type | Class | Implementation |
|---|---|---|
| Q1 lenient validation | decision-relevant | Invalid form/tool arguments are accepted and the state is updated as if valid. |
| Q2 missing error state | decision-relevant | Errors (out of stock, auth failure, policy violation) return a generic success page. |
| Q3 stale state after write | decision-relevant | Reads after writes return pre-write state with probability ρ. |
| Q4 over-cooperative UI / user | decision-relevant | Ambiguous requests auto-resolve in the agent's favour; confirmation dialogs vanish. |
| Q5 lenient success judge | reward channel | The terminal check passes when the end state is "close" (edit distance ≤ k). |
| C1 cosmetic paraphrase | cosmetic | Rewrites observation text and labels, with information preserved. |
| C2 cosmetic noise | cosmetic | Adds irrelevant DOM nodes or log lines. |

Each quirk's effect on next-observation accuracy is known analytically (accuracy = 1 − ρ·p_eligible). This lets us build **iso-accuracy pairs**, for example Q2 at ρ=0.1 versus C1 at the ρ that gives the same accuracy. The pairs test causally whether accuracy predicts transfer. That is the key fix for critique weakness (1)/(2).

**(b) Natural LLM simulators (ecological validity).**
- S1: prompted Qwen3.5-27B, AgentWorldBench-style direct prompting.
- S2: worldbook/trace-grounded agentic LWM, Trace2Env-style.
- S3: one trained world model *if* open weights are servable on our GPU, e.g. a Qwen-AgentWorld or WebWorld-class checkpoint. **Availability is unverified**; fallback is S1 with a different base model.

For each natural simulator we **audit its quirk profile**: classify its transition errors on 500 held-out transitions into Q1–Q5/C1–C2 with an LLM judge plus spot checks. We then ask whether the dose-response fitted on family (a) predicts its observed transfer curve. That is the bridge from the controlled instrument to real simulators.

### 5.3 Evolvers and agents (cheap tier)

- **Task agent:** Qwen3.5-9B, served with vLLM on one GPU (cheap and open). Replicated with one API model (GPT-mini or Haiku-class) on D1 only.
- **Evolver E1 (prompt level):** GEPA-style reflective prompt evolution with a Pareto archive. Reflector: Haiku-class.
- **Evolver E2 (code level):** a Meta-Harness/DGM-lite evolver. It edits a Python harness (tool wrappers, verification steps, memory, retry logic), with an archive and a parent-selection rule. Coding model: Haiku-class.
- **Depth:** up to 60 iterations. Checkpoints d ∈ {0, 5, 10, 20, 30, 45, 60}. At each checkpoint the sim-best harness h*_d is evaluated in the real environment on seen and held-out tasks (k=2 rollouts per task).
- **Seeds:** 3 per condition (evolver seed + minibatch order). The main GEPA × D1 curve gets 5 seeds.

### 5.4 Arms

| Arm | Fitness signal | Purpose |
|---|---|---|
| **SIM(ρ, q)** | Quirk simulator with type q at rate ρ | Causal dose-response (RQ2). |
| **SIM-nat(Sk)** | Natural LLM simulator k | Ecological curve; tests the quirk-profile bridge. |
| **REAL-count** | Real environment, same number of candidate evaluations as SIM | Separates simulator Goodhart from task-set Goodhart. |
| **REAL-cost** | Real environment at the *same dollar/GPU cost* as SIM (fewer evaluations) | The practitioner's question: is the simulator worth it? |
| **SAMPLE-matched** | Parallel sampling / random-restart baseline at matched budget (Rethinking-Eval style) | Is any evolution beyond noise? |
| **M1 ensemble** | Mean (and min) over 3 heterogeneous simulators | Mitigation (2608.12253-style population). |
| **M2 disagreement stop** | Stop when ensemble rank-disagreement on the current frontier exceeds a threshold | Mitigation / early-stopping rule. |
| **M3 anchors** | k ∈ {5%, 10%, 20%} of candidates also evaluated in real; WMRL isotonic debiasing as a variant; selection by anchored score | Mitigation; compares against WMRL's recipe. |
| **M4 quirk randomisation** | Sample a quirk mixture per evaluation (domain randomisation) | Mitigation from sim-to-real robotics. |

**Condition budget.** Family (a) uses a reduced factorial:
- Type sweep at fixed ρ=0.2: Q1–Q5 and C1 (6 conditions).
- Rate sweep for a Q-mixture: ρ ∈ {0, 0.05, 0.1, 0.2, 0.4} (5 conditions).
- 2 iso-accuracy pairs.

That gives 13 conditions × 3 seeds × 2 domains × E1 = 78 runs. E2 runs on 5 key conditions × 3 seeds × D1 = 15 runs. Natural simulators: 3 × 3 seeds × 2 domains × E1 = 18 runs. Real arms and mitigations: about 30 runs.

### 5.5 Metrics (formal)

Let *h₀* be the seed harness, *h*_d* the harness the evolver would ship at depth *d* (best by its fitness signal), *T_s* the seen tasks and *T_h* the held-out tasks.

- **Sim gain:** G_sim(d) = S_sim(h*_d) − S_sim(h₀).
- **Real gain:** G_real(d; T) = S_real(h*_d; T) − S_real(h₀; T), for T ∈ {T_s, T_h}.
- **Transfer gap:** Γ(d) = G_sim(d) − G_real(d; T_h).
  - Reported with paired-bootstrap 95% CIs over tasks × seeds.
  - The ratio G_real/G_sim is reported *only* when the G_sim CI excludes 0 (fix for critique weakness 3).
- **Peak depth:** d_peak = argmax_d G_real(d; T_h).
- **Exploitation onset:** d_onset = the smallest d > d_peak such that G_real(d_peak) − G_real(d) > 0, with one-sided bootstrap p < 0.05.
- **Goodhart decomposition**, at matched evaluation count:
  - *simulator Goodhart* = G_real^{REAL-count}(d; T_h) − G_real^{SIM}(d; T_h)
  - *task-set Goodhart* = G_real^{REAL-count}(d; T_s) − G_real^{REAL-count}(d; T_h)
- **Quirk exploitation rate:** QER(h) = the fraction of simulator successes of h whose trajectory triggered ≥1 quirk, minus the same fraction for h₀. Quirk firings are logged, so this is a mechanistic attribution rather than a judge-based one.
- **Fidelity metrics competing to predict d_peak and Γ(d):**
  1. *Next-observation accuracy* on base-agent trajectories (AgentWorldBench-style judge score and exact state-diff match).
  2. *Next-observation accuracy on evolved-agent trajectories* (a stronger baseline).
  3. *Static ranking fidelity* τ_static: Kendall τ of sim versus real success over a fixed pool of 30 hand-designed or random harnesses.
  4. *Replay consistency* CR = W2R/Real (Trace2Env).
  5. **On-path ranking fidelity** τ_path(d): Kendall τ between sim and real scores over the candidates generated in the window [d−w, d], with w=5 (about 15–25 candidates). Its cheap online estimator τ̂_path uses only the anchor subset.
  6. *Top-1 selection regret* (following 2611.01139): the real-score gap between the real-best and the sim-best candidate in the window.

  **Predictive test.** Across all conditions (about 40 simulator conditions × seeds), regress or rank-correlate each fidelity metric (measured at d ≤ 10, i.e. early) against the eventual Γ(60) and d_peak. Compare by Spearman ρ and leave-one-condition-out prediction error.
- **Cost-normalised recovery** for mitigations: (G_real^{M}(T_h) − G_real^{SIM}(T_h)) / (G_real^{REAL-count}(T_h) − G_real^{SIM}(T_h)), plotted against cost as a fraction of REAL-count.

### 5.6 Power analysis sketch

- Binary task success, base rate about 0.4. Paired evaluation of two harnesses on the same tasks with a discordant-pair fraction of about 0.25.
- With 150 held-out tasks × 2 rollouts per seed, the SE of the paired difference is about √(0.25/300) ≈ 0.029. Pooling 3 seeds (seed variance included, assumed SD about 3 pp) gives SE ≈ 0.022, so MDE ≈ 6 pp at α=0.05 and 80% power.
- The main D1 GEPA curve with 5 seeds and 300 held-out procedurally generated tasks reaches MDE ≈ 4 pp.
- HarnessX/DarwinX-scale harness gains are 5–40 pp, so the design is powered to detect a decline of half the peak gain if the peak is ≥10 pp. If the pilot shows peak gains under 8 pp, we increase pool size through the procedural generator (D1) instead of adding seeds.

### 5.7 Ablations

- **Evolver selection pressure:** archive size and number of offspring per iteration. More candidates per step means a stronger optimizer's curse (2611.01139 Remark 6).
- **Minibatch size of fitness evaluation:** noise versus exploitation.
- **Task-agent strength:** 9B versus API model. Does a stronger agent exploit quirks sooner?
- **Harness edit-space capacity:** E1 versus E2. The hypothesis is that the code-level evolver exploits Q1/Q3 earlier.

---

## 6. Expected results and figures

- **Figure 1 (concept + main result), two panels.**
  - Left: a schematic. The evolver loop scores candidates in a dream (simulator) built from the real environment plus a typed quirk layer. Shipped harnesses are evaluated in the real environment on held-out tasks.
  - Right: the key plot. x = evolution depth (0–60). y = gain in pp. Three curves for one representative condition (Q-mixture ρ=0.2, D1, GEPA): G_sim (rising monotonically), G_real held-out from SIM (rising, peaking around d≈10–20, then declining), and G_real held-out from REAL-count (rising more slowly and plateauing). Shaded bootstrap CIs. Vertical dashed lines mark d_peak and the depth at which τ̂_path first drops below 0.3.
- **Figure 2 (dose-response).** Γ(60) against ρ for each quirk type (lines). Expected: steep for Q1/Q2/Q5, flat for C1/C2. An inset shows the iso-accuracy pairs as bars: same next-observation accuracy, different Γ.
- **Figure 3 (metric shoot-out).** A scatter of each fidelity metric (measured early) against eventual Γ(60) across all simulator conditions, with natural simulators highlighted. Expected: τ_path ρ≈0.7+, next-observation accuracy ρ≈0.2, τ_static intermediate.
- **Figure 4 (mitigation Pareto).** Real held-out gain against cost (fraction of REAL-count) for SIM, M1–M4, REAL-cost and REAL-count. Expected: M3 anchors at 10% plus disagreement stopping reach ≥80% of REAL-count gain at ≤30% of its cost. WMRL isotonic debiasing is expected to help less than rank-based anchor selection on decision-relevant quirks, because those errors are policy-specific rather than monotone.
- **Table 1.** Goodhart decomposition (simulator versus task-set) per domain and evolver.
- **Table 2.** Quirk profile of natural simulators and predicted versus observed Γ.

---

## 7. Pilot (≤1 week, ≤$50) and decision criteria

**Setup.**
- Domain: D1 retail (τ²-bench-style). 50 seen + 50 held-out tasks.
- Task agent: Qwen3.5-9B through a cheap hosted endpoint, or self-hosted if a GPU is free.
- Evolver: GEPA with a Haiku-class reflector.

**Conditions.** Three evolutions × 20 iterations × 1 seed:
1. Oracle simulator + Q-mixture at ρ=0.2 (Q1+Q2+Q5).
2. Prompted LLM simulator (Qwen3.5-27B or a mini API model).
3. REAL-count.

**Measurements.**
- Real evaluation at d ∈ {0, 5, 10, 20} on held-out tasks.
- τ_path over the archive (about 20 candidates per run, each scored in sim and real on 30 tasks).
- Next-observation accuracy of the LLM simulator on 200 transitions.

**Estimated cost.** About 3 runs × 20 iterations × 10-task minibatches, plus 4 checkpoints × 3 × 50 tasks × 2, plus the archive cross-scoring. That is roughly 8–10k agent episodes of 9B tokens (about $15–25 hosted) plus reflector calls (about $5–10).

**Decision criteria.**

| Signal | Go | Pivot | Kill |
|---|---|---|---|
| G_sim in condition (1) | ≥10 pp by d=20 | 4–10 pp → raise ρ or task difficulty | <4 pp → simulator is not a usable fitness signal at all |
| Γ(20) in condition (1) vs Γ(5) | grows by ≥5 pp → **Goodhart framing** | flat but G_real ≈ G_sim → **"harnesses are robust" framing** (still go, with emphasis on the audit protocol and the cost savings) | — |
| Real-env noise floor | seed-to-seed SD of G_real ≤ 4 pp | 4–8 pp → move to procedural task generator with a 300-task pool before scaling | >8 pp with 100 tasks → stop; redesign the benchmark |
| QER in condition (1) | >0 and rising with d (mechanism visible) | ≈0 → quirks are not reached; increase eligibility | — |
| Infrastructure | quirk layer + simulator wrapper built in ≤3 days | — | Natural LLM simulator cannot keep state over 15+ turns → drop S1, use only quirk sims + Trace2Env-style S2 |

---

## 8. Budget and timeline (to mid-May 2027)

**Budget estimate**

| Item | Estimate |
|---|---|
| Pilot | $50 |
| Family (a) quirk-simulator runs (78 E1 + 15 E2 runs). Real-env verification is the dominant cost; the simulator itself is free | API reflector/coder ≈ $500; agent tokens self-hosted (≈ 250 GPU-h on 1×A100/H100) |
| Natural LLM simulators (18 runs; the 27B simulator is self-hosted) | ≈ $150 API + ≈ 200 GPU-h |
| Real arms + mitigations (≈30 runs) | ≈ $300 |
| API-agent replication on D1 (GPT-mini/Haiku-class) | ≈ $400 |
| Quirk-profile audits, judge calls, slack | ≈ $300 |
| **Total** | **≈ $1.7k API + ≈ 500 GPU-h** (lab or Duke cluster). Fits the ≤$3k envelope. If no GPU is available, a hosted 9B adds about $600. |

**Timeline**

| Month | Milestones |
|---|---|
| **Nov 2026 (2nd half)** | Pilot. Build the quirk layer for D1. Decide go/pivot. Meet Xunjian about BreakingWeb sandbox access. |
| **Dec 2026** | D1 procedural task generator (300+ tasks, verifiers). GEPA + E2 evolvers instrumented with depth checkpoints and quirk logging. Main D1 curve, 5 seeds. |
| **Jan 2027** | D1 full family (a) sweep (type, rate, iso-accuracy). D2 web sandboxes: quirk layer at the backend/API level, 300 tasks with DB verifiers. |
| **Feb 2027** | D2 sweeps. Natural simulators S1/S2 (and S3 if available) on both domains. Quirk-profile audits. |
| **Mar 2027** | Real arms (REAL-count, REAL-cost, SAMPLE). Mitigations M1–M4. E2 subset. API-agent replication. |
| **Apr 2027** | Metric shoot-out analysis, leave-one-condition-out prediction, figures. Write the draft. Internal review by Xunjian and Shuyan. |
| **May 2027 (1st half)** | Robustness reruns for any reviewer-bait gaps. Release the code (quirk layer as a reusable "dream audit" toolkit). Submit. |

---

## 9. Risks, mitigations and anticipated reviewer objections

**Risks**

| Risk | Likelihood | Mitigation |
|---|---|---|
| Harness gains in the real environment are tiny (Rethinking-Eval: held-out ≈ 0), so there is nothing to transfer | Medium | The pilot measures it first. Pick seed harnesses that are deliberately weak (minimal tool-use prompt) so evolution has headroom. The procedural generator increases power. |
| No Goodhart curve at all | Medium | The "harnesses are robust" outcome is pre-registered as publishable. Q5 judge leniency almost guarantees *some* exploitation (DarwinX's TerminalWorld proxy saturation), which serves as a positive control. |
| Natural LLM simulators are too weak to sustain long episodes (Trace2Env: direct prompting CR=0.03) | Medium | Then they sit at the far end of the curve, which is still informative. Add the S2 worldbook-grounded simulator as a mid-fidelity rung. |
| Web sandbox engineering eats time | Medium–High | D1 alone carries the paper's core (causal + metric + mitigation). D2 is generality. Fallback for D2 is WebArena-style apps with backend-level quirks. |
| Scoop by an AWM/AgentWorld group or the 2611.01139 authors extending to LLMs | Medium | Monthly alphaXiv/S2 watch on citers of 2608.12253, 2611.01139 and DarwinX. The quirk-injection causal design plus the attribution arms is the defensible core even if someone posts a simulator-scored GEPA paper. |
| Trained world model (S3) weights unavailable or too big | Medium | Optional. S1/S2 plus family (a) suffice. |

**Anticipated reviewer objections and rebuttals**

1. *"Prediction accuracy ≠ usefulness is known (Lambert 2020; 2610.19375; 2609.33335; 2611.01139)."*
   **Rebuttal:** we agree and cite all of them. Our claims are (i) the first *causal iso-accuracy* test inside an LLM self-improvement loop, (ii) a replacement metric (on-path ranking fidelity) validated as a *predictor of the transfer peak*, and (iii) error-*type* attribution. 2611.01139 shows static ranking can be high while optimisation still fails. We show that measuring ranking *on the optimizer's current frontier* closes that gap. This is a direct extension, and we test it against τ_static.
2. *"Your quirk simulators aren't world models; they're buggy real environments."*
   **Rebuttal:** that is intentional. They are the controlled instrument, in the spirit of BreakingWeb's controlled interventions. Ecological validity comes from three natural LLM simulators whose *measured* quirk profiles we feed through the fitted dose-response to predict their transfer (Table 2).
3. *"This is just 2608.12253 for harnesses."*
   **Rebuttal:** different optimiser (discrete LLM-driven harness search versus RL on weights), different simulator (environment dynamics versus user), and a causal error manipulation they lack. Comparing *when* each Goodharts is itself a finding. If harnesses do not collapse, the contrast is the headline.
4. *"The decline is ordinary overfitting to the task set."*
   **Rebuttal:** the REAL-count arm and the seen/held-out split give an explicit decomposition into simulator Goodhart and task-set Goodhart (Table 1). QER gives a mechanistic trace.
5. *"Single benchmark / engineering."*
   **Rebuttal:** two domains (tool-use and web), two evolvers (prompt-level and code-level), two agent models, at least 3 seeds, paired-bootstrap CIs, and a released toolkit.
6. *"Why not just use real environments?"*
   **Rebuttal:** the REAL-cost arm answers this quantitatively. The mitigation Pareto (Figure 4) tells practitioners exactly when simulators pay off.

---

## 10. Fit with Xunjian's agenda, and what Xunjian and Shuyan bring

- **Gödel Agent (2410.04444):** self-referential improvement needs a fitness signal. This paper asks what happens when that signal is a dream. It is a direct measurement study for the self-improvement loops Xunjian builds, and E2 (code-level self-modification) is Gödel-Agent-adjacent.
- **BreakingWeb (2609.35814):** the core method, controlled environment interventions with labelled perturbations, is BreakingWeb's idea applied to the *simulator* rather than to the test task. Xunjian's sandboxes are the natural D2 ground truth, and the false-success phenomenon (agents claiming success, 54–85%) maps onto our Q5 judge-leniency channel.
- **World models:** the paper's object is the world model used as a fitness function, which is squarely on his "self-referential agents and world models" agenda.
- **WeAct (2610.26847):** a natural follow-up is weight switching as an action inside a simulator-scored loop. Out of scope here; listed as future work.
- **What Xunjian brings:** BreakingWeb sandbox infrastructure and intervention know-how, self-improvement loop design (Gödel Agent), world-model framing, and possibly GDM-side perspective on simulator-based agent training.
- **What Shuyan Zhou brings:** WebArena authorship, so self-hostable web environments, verifier design and credibility on web-agent evaluation. Ideal for D2 and for the "ground truth must be deterministic" design choice.
- **Dan's existing plans reused:** the procedural τ-style task generator (power), the matched-compute rigor (Rethinking-Eval style), and the confound battery (REAL-count, SAMPLE arms).

---

## 11. Rubric re-score after fixes

| Dim | Weight | Score | One-line justification |
|---|---|---|---|
| **N** novelty | 3 | **3.5** | No paper runs harness evolution × simulated dynamics × typed error dose × depth. But the new 2611.01139 (ranking ≠ calibration; optimisation against a frozen WM collapses) and WMRL (anchors) plus 2608.12253 tightly surround the seam, so novelty is in the combination and the causal design, not in any single idea. |
| **S** surprise | 2.5 | **3.5** | The iso-accuracy result and "type, not rate" are crisp and either outcome is publishable. However, "simulators get exploited" is the prior, so only a no-Goodhart result or a strong τ_path predictor would really surprise. |
| **R** relevance | 2 | **4.5** | World models + BreakingWeb-style interventions + self-improvement loops: the mentor's core agenda. |
| **C** competitiveness/rigor | 2 | **4.0** | Causal manipulation, matched-count and matched-cost real arms, held-out split, 2 domains × 2 evolvers × 2 agents, bootstrap CIs, reusable toolkit. Still somewhat "measurement paper". |
| **F** feasibility | 1.5 | **3.0** | API cost is fine (≈$1.7k), and quirk simulators are cheap. But about 140 evolution runs, a web sandbox build and self-hosting 9B/27B models are a lot for a solo undergrad in 6 months. D1-only is the fallback. |
| **T** trend | 1 | **4.0** | World models as environments and harness evolution are both peak-2026 topics. |

**Total = 20 × (3·3.5 + 2.5·3.5 + 2·4.5 + 2·4.0 + 1.5·3.0 + 1·4.0) / 12 = 20 × 44.75 / 12 ≈ 74.6** (upper "viable with fixes", up from 71). This is an honest estimate. The main limiter is novelty pressure from 2611.01139 and 2608.12253. The upside case (clean iso-accuracy separation plus τ_path predicting d_peak across natural simulators) would read as a 6/10 to 7/10 NeurIPS paper.

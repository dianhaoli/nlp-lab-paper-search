# Critique: evolve-in-dream — does harness evolution inside world-model simulators transfer to real environments?

**Verdict:** viable with fixes (about 71/100). Nobody has run this exact design, but every part of the claim already has a precedent. As written, the headline ("prediction accuracy is the wrong fidelity metric") is the old objective-mismatch result (Lambert et al. 2020), re-confirmed in Sep–Oct 2026 by two papers. What is new is applying it to harness evolution and showing a depth-dependent Goodhart curve. To keep that new part, the paper must manipulate fidelity causally and offer a better fidelity metric.

## 1. Scoop search (Nov 12, 2026)
Searches run:
- alphaXiv (recency):
  - "harness or prompt evolution inside LLM-simulated or learned web world models, with sim-to-real transfer"
  - "does WM next-state prediction accuracy predict downstream agent performance / simulator exploitation"
  - "self-evolving agents overfitting simulator artifacts"
  - "web agents self-improving in dream / imagined rollouts"
  - "GEPA / prompt optimization scored by a simulator and real-env survival"
- S2 keyword search (4 phrasings, 2026+).
- S2 forward citations of: Agent World Model 2602.10090 (~60 citers since Jun 2026), DarwinX 2608.07545, AdvSim2Real 2610.08773, WM audit 2609.33335, and Mind the Sim2Real Gap 2603.11245.

**Result:** none of these searches found a paper that evolves a harness or prompt against a world-model simulator and measures how much of the gain transfers to the real environment as a function of evolution depth and simulator fidelity. The nearest work, with the gap each one leaves:

| Paper | What it does | Gap left |
|---|---|---|
| **Simulator Collapse: One Frozen Simulator Is Not Enough** 2608.12253 (Aug 2026; Levine, Manning, Shi et al.) | RL training of a 4B policy against one frozen LLM *user* simulator. Training reward rises while held-out reward peaks early and then falls below base (Goodhart-in-sim curve). Fixed with Verbalized Sampling and population co-training. Includes a human study. | This is the closest conceptual scoop: it already shows the sim-overfitting curve. But it updates weights by RL against a *user* simulator, not harness evolution against an *environment* world model, and it does not study fidelity metrics. The reviewer line is "this is 2608.12253 for harnesses". |
| **AdvSim2Real** 2610.08773 (Oct 2026) | Co-evolves curriculum, adversary and a 4B executor (RL, LoRA) inside frozen WebWorld-14B. Stage-1 gains *do* transfer to a real Chromium browser (strict success 25.6→44.4, monotone over 3 rounds). Agreement per task: "WM-only success" falls from 51.8% to 36.4%. | Weights, not harness. Only 3 rounds, so no depth sweep. One simulator, so no fidelity axis. Their result is evidence *against* the "transfer falls with depth" claim, at least at shallow depth. |
| **DarwinX** 2608.07545 (Jul 2026; Salesforce) | Harness evolution on 300 synthetic intents with an LLM-judge fitness. Evaluated on 1,260 real WebArena-Infinity tasks: 43.5→93.0% (huge transfer). TerminalWorld: proxy saturates at 1.0 while held-out pass@1 is 68.3%, a proxy-overfit gap. | The *tasks and judge* are synthetic, but the environment is real. No simulated environment dynamics, no depth curve of transfer, no fidelity manipulation. Again it shows strong transfer, which weakens the "Goodhart" prior. |
| **One-Step Prediction Fidelity Is a Poor Proxy for Planning** 2610.19375 (Oct 2026) | JEPA world models. One-step error keeps improving while planning goes flat (r=−0.01). Matched continued-training control. | Continuous control, planning with MPC. No LLM agents, no self-improvement. It pre-empts the *metric* half of the claim. |
| **Does Learning to Predict the World Help Agents Act?** 2609.33335 (Sep 2026) | Mismatched next-observation targets cut prediction accuracy by 15–62% yet keep most of the task gains. Random rewards expand pass@64. | WM *post-training of weights*, not WM-as-simulator for evaluation. A second pre-emption of "prediction accuracy ≠ usefulness". |
| **Training Needs Trustworthy Worlds** 2608.21898 (Aug 2026) | Verified versus raw synthetic web environments. PPO policies from verified environments transfer better to WebArena, WebShop and MiniWoB. | Environment *validity* (binary verified/raw), small PPO policies, no harness evolution, no depth axis. |
| **Agentic World Modeling** 2610.40927 (Oct 2026; Microsoft) | Read-grounded LLM simulator for test-time Best-of-N. Fidelity grows with M and the gap to real-env BoN shrinks. Compares SIMIA, Qwen-AgentWorld and AWM. | Uses the WM for selection at test time, not for an evolution loop. Useful as a ready-made "fidelity ladder" (SIMIA < trained WM < AWM). |
| Also relevant | **Simulator collapse** is the strongest. Others: Mind the Sim2Real Gap in User Simulation 2603.11245; DynaWeb 2601.22149 and DreamGym 2511.03773 (RL in WM, transfer reported); Echoverse 2607.28074; AWM-in-the-Wild 2610.26828; Dual Exam 2610.31066; Beyond Next-Screen Prediction (decision value of GUI WMs) 2610.24372; Dual-Frontier: when can an agent trust its WM 2609.26293; RENEW (model exploitation) 2607.14180; Reflective Prompt Optimization overfitting protocol 2610.12601. | None crosses harness evolution × simulated environment × depth/fidelity sweep. |

Classic prior that reviewers will cite immediately: Lambert et al. 2020, "Objective Mismatch in MBRL", and the value-equivalence and value-aware model-learning line (Farahmand 2017; Grimm 2020). The claim that "prediction accuracy is the wrong fidelity metric" is well known in MBRL. Only its harness-evolution instance is new.

**Scoop status:** not scooped (K1 does not fire), but closely surrounded. Two 2026 papers already show sim-trained gains transferring well (AdvSim2Real, DarwinX), and one shows a Goodhart-in-sim curve (2608.12253). The novelty sits in a narrow seam: (harness/meta-agent evolution) × (environment world model) × (depth × fidelity sweep).

## 2. Mock NeurIPS review
**Summary.** The paper evolves agent harnesses (GEPA-style) with fitness computed inside LLM-prompted or trained web world models of 2–3 sites. It then measures the transfer fraction (real gain ÷ sim gain) against evolution depth and simulator fidelity, and claims that transfer decays with depth even when next-state accuracy is high.

**Strengths.**
- A timely, practically important question. Simulator-scored self-improvement is becoming the default route to cheap evolution (DreamGym, AWM, Qwen-AgentWorld, AdvSim2Real), and nobody has measured the Goodhart curve *for harness search*.
- Either outcome can be published. Decay gives a Goodhart curve. No decay ("harnesses don't overfit simulators the way weights do") is also interesting, and it contrasts sharply with 2608.12253.
- Fits the mentor well: world models plus BreakingWeb-style controlled environments plus self-improvement.

**Weaknesses (as currently scoped).**
1. **The novelty of the metric claim is low.** Objective mismatch (Lambert 2020), 2610.19375 and 2609.33335 already show prediction accuracy ≠ downstream usefulness. A reviewer will write: "Known in MBRL. The contribution is applying it to prompts."
2. **Fidelity is not manipulated causally.** Comparing "prompted LLM vs AWM-style simulator" confounds fidelity with many other differences between the simulators (style, determinism, mode collapse, observation format). Two or three simulators give two or three points on a fidelity axis, which is not a scaling curve.
3. **"Real sites" are a reproducibility hazard.** Live sites drift, rate-limit and change. With only 2–3 sites and small task pools, transfer-fraction confidence intervals will be huge (the 41-task TerminalWorld and the 26-pp seed spread in 2610.19375 are warnings). The transfer *fraction* is a ratio of two noisy differences and is unstable when the sim gain is small.
4. **The confound battery is missing.** Is real-env decay from sim-quirk exploitation, or does evolution overfit any fixed task set? The latter happens in real environments too (Rethinking Harness Eval 2607.12227: held-out gain ≈ 0; RRSI). This needs a *real-env evolution arm on the same tasks at matched evaluation count*, plus a held-out-task real arm. Without them the Goodhart is not attributable to the simulator.
5. **Too few sites and too little evolver diversity.** One GEPA-style evolver and 2–3 sites, so "engineering, single benchmark" applies.
6. **No constructive output.** What should practitioners use instead of prediction accuracy? Without a better fidelity metric, a validated early-stopping rule or a mitigation (simulator ensembles à la 2608.12253), it reads as an observation-only workshop paper.

**Likely score:** 4–5/10 as written (borderline reject: "interesting question, results in a narrow setting, metric claim known"). With the fixes below, 6/10 (weak accept) is realistic.

## 3. Scores (rubric)
| Dim | Score | Rationale |
|---|---|---|
| N (×3) | 3.5 | No direct scoop. But 2608.12253 (Goodhart-in-sim curve), 2610.19375 and 2609.33335 (prediction-accuracy ≠ usefulness), and AdvSim2Real/DarwinX (sim-to-real transfer) cover each half. |
| S (×2.5) | 3.5 | Either outcome is publishable. The metric part of the headline is unsurprising, while "harness evolution has a sim-Goodhart peak whose location is predictable" would surprise. |
| R (×2) | 4.5 | World models + BreakingWeb + self-improvement: a direct fit to the mentor's agenda. |
| C (×2) | 3.0 | Simulator-engineering-heavy. Few sites, no causal fidelity manipulation, ratio metric is noisy. |
| F (×1.5) | 3.0 | API cost is fine ($0.5–1.2k). Trained WMs (WebWorld-14B, Qwen-AgentWorld-35B-A3B) need GPU serving. Building 2–3 faithful simulators and stable real targets is real overhead for a solo undergrad. |
| T (×1) | 4.0 | Environment synthesis and world models for agents are among the hottest 2026 topics (AWM has 60+ citers in 5 months). |

**Total = 20×(3·3.5 + 2.5·3.5 + 2·4.5 + 2·3 + 1.5·3 + 4)/12 = 71.3** (viable with fixes)

**Kill flags:**
- K1: no. No paper runs this design.
- K2: no. Both outcomes are interesting.
- K3: no. Open WMs and open or cheap agent models are available.
- K4: **partial risk**. As framed, it reads as "harness evolution + WM sim2real". It escapes K4 only if the paper contributes a new fidelity metric or protocol and a causal fidelity manipulation.

## 4. Fixes that would raise the score
1. **Manipulate fidelity causally, using BreakingWeb-style interventions.** Start from a high-fidelity simulator (a self-hosted site replayed as a deterministic oracle, or AWM read-grounded). Inject controlled, *labelled* quirks at known rates:
   - lenient form validation
   - a missing error page
   - an overly cooperative modal
   - stale state after a write
   - judge leniency

   This gives a dose–response curve of transfer against quirk rate and quirk type. It is the mentor's BreakingWeb methodology applied to simulators, and it gives the scaling axis a real dial rather than 2–3 simulators.
2. **Replace "real sites" with self-hosted real environments** (WebArena/WebArena-Infinity-style apps or BreakingWeb sandboxes) as ground truth. Use many tasks (≥150 per site) with deterministic verifiers. Keep one live-site sanity check.
3. **Add the attribution arms.**
   - (a) Real-env evolution on the same task pool at matched *evaluation count*.
   - (b) Real-env evolution at matched *dollar cost*, since the simulator is cheaper and that is its selling point.
   - (c) Sim evolution evaluated on held-out real tasks versus the same tasks.

   Together these separate "simulator Goodhart" from "task-set Goodhart" (Rethinking Harness Eval and RRSI show the latter exists in real environments).
4. **Propose the constructive metric.** Measure **harness-ranking fidelity**: the Kendall τ between simulator and real scores over a fixed pool of candidate harnesses, *including ones from late evolution*. Show that it predicts the transfer peak and next-state accuracy does not. Evaluating the ranking on *evolved* harnesses captures distribution shift that one-step accuracy on base-agent trajectories cannot. This is the reusable artifact.
5. **Add a mitigation arm.** Test three mitigations at matched cost:
   - simulator ensembles or population (borrowing 2608.12253's fix)
   - early stopping by sim-ensemble disagreement
   - "anchor" real evaluations on k% of candidates

   Report how much real-env-evolution performance each recovers, and at what fraction of the cost.
6. **Generality.** Add one non-web domain where simulators already exist (τ-bench or AppWorld via AWM/SIMIA/Qwen-AgentWorld), plus two evolvers (GEPA-style prompt evolution and a code-level harness evolver such as a DGM/Hyperagents-lite). Use paired bootstrap over ≥3 seeds and report the transfer *gap* (real Δ vs sim Δ) with confidence intervals, not just a ratio.
7. **Frame against prior work directly.** Use 2608.12253 (weights + user simulator) and AdvSim2Real/DarwinX (positive transfer) as foils. Either "harness evolution Goodharts on simulators earlier or later than RL does", or "it does not, because the harness edit space is too coarse to exploit quirks". Both answers are publishable.

**Sharpened headline claim:**
> When harness evolution is scored inside an LLM world model, real-environment gain peaks and then declines with evolution depth. The location of the peak is predicted by the simulator's *harness-ranking fidelity*, not by its next-observation accuracy. Controlled quirk injection shows which simulator errors evolution exploits, and simulator ensembles with sparse real anchors recover most of real-environment evolution at a fraction of the cost.

**Pilot (≤$50, 1 week):**
1. Pick one self-hosted site with 50 tasks and a prompted-LLM simulator of it.
2. Run 20 GEPA iterations in sim. At iterations 0/5/10/20, evaluate in both sim and real.
3. Over the 20 archive harnesses, compute ranking fidelity (Kendall τ) and next-state accuracy.

If the sim–real gap does not grow with depth, pivot to the "harnesses are robust to simulator quirks" framing. That outcome is still publishable when contrasted with 2608.12253.

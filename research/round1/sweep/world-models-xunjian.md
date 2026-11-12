# Sweep: Mentor-adjacent — world models, environment interventions, weight switching, self-reference

Date: 2026-11-12. Sources: alphaXiv discover/get_paper_content/answer_pdf_queries; Semantic Scholar forward citations of WeAct (2610.26847), BreakingWeb (2609.35814), LEDOM (2507.01335), Geometry of Reasoning (2510.09782), Gödel Agent (2410.04444), IDM-guided Hyperagents (2610.13991), RWML (2602.05842); S2 keyword searches. All IDs below appeared in a tool result unless marked *unverified*.

---

## 1. Summary of the landscape

**Xunjian's own recent line.**
- **BreakingWeb** (2609.35814; NeurIPS 2026; Yin, Guan, ..., Dhingra, Zhou). The design: hold the task and its backend success criterion fixed, then apply a controlled, recoverable intervention at one of four layers (seed, server, network, client). The paper reports the paired drop. Each intervention is labeled with one of 7 cognitive primitives.
  - Results: 519 task pairs on 7 self-hosted sites. Six browser-use agents drop 17.9–27.6 pp. 75% of failures are *belief failures* (the agent declares done while the backend disagrees). 54–85% of the runs agents declare finished have actually failed. The worst family is "fabricated success" (70% failure rate). Humans drop only 10.0 pp cold and 5.7 pp warm.
  - Stated future work: (i) use the intervention catalog as a training curriculum; (ii) retest exploration on easier base tasks.
  - Limitations: one text harness (browser-use) and one GUI harness (BrowserGym); English consumer sites only.
  - Forward citations so far: only EMBERS (2611.03723, defect-guided fuzzing of deep-research systems). **Nobody has yet used BreakingWeb to evaluate self-improving agents.**
- **WeAct** (2610.26847; CMU/Duke; Shi, Yin, Sang, Liu, Zhou). The method: train independent capability LoRAs (plan, search, write) over a frozen Qwen3-32B, and add `load_lora`/`offload_lora` to the agent's action space.
  - Results: 45.47% average over 8 deep-research benchmarks (+6.2% relative to full SFT, +27.5% relative to the best static merge). Adding a new code adapter and refreshing the writing adapter each work without touching other weights.
  - Stated limitation: "Extending WeAct to sustained continual learning raises open questions about routing capacity…; retention within updated modules remaining open."
  - S2 shows 0 citations so far. The closest independent work is **SLAaaT** (2608.17034, "Switching LoRA Adapters as a Tool", Aug 2026), which predates WeAct.
- **LEDOM** (2507.01335) and **Geometry of Reasoning** (2510.09782) are cited mainly by interpretability and latent-reasoning papers (e.g., 2608.05660 "Reasoning errors have a region and a direction", 2604.05655). They are not useful anchors for a meta-agent paper.
- **Gödel Agent** forward citations (26 in S2) are mostly harness-evolution systems: EvoUndo 2608.28363, PACE anytime-valid acceptance 2606.08106, Polaris 2603.23129, Exo 2611.00463, TinyAstral 2610.41216, "Are You Still the Agent I Authorized?" 2607.23586.

**World models for LLM agents (2026 has been a big year).**
- Learned or textual simulators of the environment:
  - RWML 2602.05842
  - Agent World Model 2602.10090 and AWM-in-the-Wild 2610.26828
  - Qwen-AgentWorld 2606.24597 (537 votes)
  - Text World Models survey 2606.09032
  - Self-Evolving World Models for planning 2606.30639
  - CoMAP 2606.02372 (co-evolving WM and policy)
  - Agent-Editing WM 2609.28416
  - MKD2World 2611.06544
  - Agentic Language WMs 2610.06100
- Reliability and trust of world models: Dual-Frontier 2609.26293 ("when can an agent trust its world model"), False Prophets 2607.23147 (WM security).
- **Critical audit:** "Does Learning to Predict the World Help Agents Act?" (2609.33335). The authors train on *mismatched* next-observation targets, which lowers prediction accuracy by 15–62%, yet task gains are retained. A coin-flip reward alone expands pass@64 (VisualWebArena +14.3%). In short, much of RWML-style "world-model" gain is an optimization effect, not prediction content.

**World models pointed at the agent itself (self-modification prediction). This is the meta-level where most new threat sits.**
- **Predict Before You Modify / IDM-guided Hyperagents** (2610.13991; UVA/MSR). An editable Improvement Dynamics Model forecasts task-level outcomes of a proposed diff, triggers one revision, and is itself evolved.
  - Polyglot AUBC: 0.306 vs 0.210. At matched tokens: +10.9%. Held-out: +13.25 pp vs +5.42 pp. The IDM transfers to a new domain.
- **SpecRSI** (2610.16491; Tencent/PKU). A learned Scorer predicts verified return. Successors are drafted speculatively. The improver is trained with GVO.
- **Rehearse** (2607.27687; Tencent). Pre-execution judgment of autoresearch edits falls off a *confidence cliff*: selective accuracy drops from 82.8% to 56.9% once ≥3 successes have accumulated, while coverage rises. Focused outcome memory restores it to 83.5%.
- **LMs as AI Research World Models** (2610.12235; Amazon). 2,653 real experiment records (171k H100-h). Prior experimental records raise Spearman from 0.51 to 0.77. Research knowledge matters more than model choice or reasoning effort.
- **RSI-Anything** (2610.13912). Uses JEPA-based world models inside RSI. It names the "coupled-update challenge": a policy update invalidates the world model.
- **Third Eye** (2611.12988). A useful LoRA self-update loses −1.78 pp after one further native update. Its authors' forecasters did *not* yield a validated advantage.

**Environment shift and self-improvement (closest to "self-improvement under environment change").**
- SkillShift-Bench 2610.33528: learned web skills under contextual, perceptual and execution shift. Success falls from 41% to 19% under execution shift.
- ReScopeBench 2611.05450: experience lifecycle with silent changes. Agents retain still-valid experience (~90%) but update stale experience slowly (64% by the 4th task).
- PACE-Bench 2608.14441: physics shift. Memory methods anchor to early designs ("design fixation"). Revealing the exact change does not raise the ceiling.
- EnvOS 2610.40246; RSIAgent 2609.15364.
- "Appreciation Teaches Unauthorized Execution" 2610.39122: self-evolution *degrades* robustness to indirect prompt injection.
- "Beneath the Surface" 2611.07624: evolved harnesses redistribute task success; transfer is target-dependent.
- "Can Coding Agents Sustain Continual Evolution without Forgetting?" 2611.03219.

**Bottom line.**
1. "World model for the self-improver" is now a crowded micro-niche: IDM, SpecRSI, Rehearse, RSI-Anything, RWM, Janus/MetaRSI (cited inside SpecRSI). It was all published Jul–Nov 2026.
2. "Self-improvement under environment shift" has benchmarks for skills, experience, physics, and prompt-injection safety.
3. **However, no paper evaluates harness- or meta-agent self-improvement under BreakingWeb-style controlled, paired, recoverable environment interventions.** No paper asks whether edit-outcome forecasters (self-models) stay calibrated when the environment shifts or as the agent improves, with placebo controls.
4. This is the mentor's own asset (BreakingWeb), and it is uncontested.

---

## 2. Key papers

| # | Paper | ID | Date / venue | Finding | Critique / limitation / future work | Relevance |
|---|---|---|---|---|---|---|
| 1 | Constructing Challenging Browser-Use Tasks by Controlled Environment Interventions (BreakingWeb) | 2609.35814 | Sep 2026, NeurIPS 2026 | 519 paired tasks; −22.9 pp avg; 75% belief failures; 54–85% of declared successes are false; humans −5.7 pp warm | One text harness, one GUI harness; static agents only (no memory across tasks); stated future work = interventions as training curriculum | **Mentor asset; the instrument for ideas 1 and 3.** No self-improving agent evaluated on it |
| 2 | WeAct: Dynamic Weight Switching as Agent Actions | 2610.26847 | Oct 2026 | Capability LoRAs as internal actions; 45.47% avg on 8 deep-research benchmarks; modular insertion and refresh | Static, hand-defined capability split (plan/search/write); one insertion and one refresh only; "sustained continual learning … open"; single seed for retention diagnostics | Mentor asset; basis for idea 4 (adapters as self-modification) |
| 3 | Agents unlock new capabilities through Switching LoRA Adapters as a Tool (SLAaaT) | 2608.17034 | Aug 2026 | LoRA switching as a tool to avoid forgetting in long trajectories | Independent preprint, small scale | Prior art for WeAct; limits novelty of anything framed as "adapter switching" |
| 4 | Predict Before You Modify: Learning Improvement Dynamics for Self-Evolving Agents (IDM) | 2610.13991 | Sep/Oct 2026 | Evolvable forecaster of edit outcomes; +46% AUBC; holds at matched tokens; held-out +13.25 vs +5.42 pp; transfer | 5 runs; median-based stats; one revision allowed; no shuffled-forecast placebo (only frozen-IDM and reflect-every-draft ablations); fixed task distribution; no environment shift | **Main threat** to "WM-guided self-modification". Its gaps (placebo, shift, calibration over depth) are idea 2 |
| 5 | Drafting Agents, Not Just Tokens: Speculative RSI (SpecRSI) | 2610.16491 | Oct 2026 | Learned Scorer of verified continuation return; draft-ahead; GVO trains improver; beats Janus and MetaRSI | Heavy infrastructure (MILP solver, LoRA improver); single reference backbone trained | Threat to "learned improver / surrogate evaluator" ideas; a baseline for idea 2 |
| 6 | Rehearse: Stepping Back from the Confidence Cliff in Self-Improving Autoresearch | 2607.27687 | Jul 2026 | Pre-run judge selective accuracy 82.8→56.9% as successes accumulate (confidence cliff); focused memory fixes it to 83.5% | Depth variable is observational; one model per loop; autoresearch only, not meta-agents | Template for idea 2 (offline benchmark from evolution logs); shows the phenomenon exists |
| 7 | Language Models as AI Research World Models | 2610.12235 | Oct 2026 | 2,653 records; ICL research knowledge lifts Spearman 0.51→0.77; cross-env transfer; knowledge > reasoning effort | In-context only; cross-env evidence limited to AR pretraining; "balance immediate improvement vs knowledge acquisition" left open | Supports "self-model of improvement" framing; dataset precedent |
| 8 | RSI-Anything: Unlocking RSI with World Models | 2610.13912 | Oct 2026 | WM-guided selection + WM adaptation in code repair (26.3→29.2→31.2%), GUI, lab planning | "Joint multiround evaluation is needed to establish sustained mutual improvement"; heterogeneous demos; weak controls | Names the coupled-update problem (policy drift invalidates WM), which idea 2 measures |
| 9 | Third Eye: Beyond Immediate Gains in RSI | 2611.12988 | Nov 2026 | LoRA self-update immediate utility ≈0, after continuation −1.78 pp; selection optimism; no reliable mitigation | 3–8B models; two-example updates; H2 only; "prediction remains a distinct empirical claim" | Threat/support for Dan's #6 multi-round compounding; a measurement template |
| 10 | Does Learning to Predict the World Help Agents Act? Auditing World-Model Post-Training | 2609.33335 | Sep 2026 | Mismatched targets cut prediction accuracy 15–62% yet keep gains; coin-flip reward expands pass@64 | Text environments plus VWA; Qwen2.5-7B only | **Method template**: MIS/COIN placebo controls, transferable to IDM-style self-models (idea 2) |
| 11 | Reinforcement World Model Learning for LLM-based Agents (RWML) | 2602.05842 | Feb 2026 | Next-observation prediction reward for agent RL | Gains partly not from prediction (see #10) | Anchor; 24 forward citations, all WM-training methods |
| 12 | Agent World Model: Infinity Synthetic Environments | 2602.10090 | Feb 2026 | Synthesized environments for agentic RL | Fidelity of synthetic envs | Simulator substrate for idea 5 |
| 13 | Agent World Model in the Wild: CUA Environment Synthesis | 2610.26828 | Oct 2026 | Computer-use environment synthesis at scale | — | Same as above; threatens "procedural env generator as contribution" |
| 14 | Qwen-AgentWorld: Language World Models for General Agents | 2606.24597 | Jun 2026 | Large-scale language world model for agents | Industrial scale | Strong WM backbone option for idea 5 |
| 15 | SkillShift-Bench | 2610.33528 | Oct 2026 | Learned web skills under contextual, perceptual and execution shift; success 41→35→19%; MPCR test-time reranking +8.9% rel. | Skills only (AWM/ASI/SkillWeaver/WALT), not harness or meta-agent; one shift type per run; capable LLMs only | Closest to idea 1 for *skills*; must be cited and differentiated |
| 16 | Towards Robust Long-Lived Agents: Experience Lifecycles in Evolving Environments (ReScopeBench) | 2611.05450 | Nov 2026 | Silent changes; preserved content ~90% vs updated content 64% by +3 tasks; coverage ≠ performance | Memory and experience, not procedure or harness; simulated apps | Threatens benchmark-style "agents under drift"; differentiate by meta-agent/harness |
| 17 | PACE-Bench: Physics Adaptation via Code Evolution | 2608.14441 | Aug 2026 | 10 self-evolving methods fail to re-adapt after physics shift; memory → design fixation; revealing the change doesn't help | 2D Box2D; 20 attempts; 2 runs | Evidence for idea 3 (evolved state anchors and reduces plasticity) |
| 18 | Beneath the Surface: What We Overlook When We Talk About Agent Evolution | 2611.07624 | Nov 2026 | 16 evolved champions all gain and lose tasks; transfer target-dependent; highest-scoring parent ≠ best parent | GPT-5.4 only; τ3-Banking and TB2 only | Overlaps the evaluation side of idea 1 (transfer), but not controlled interventions |
| 19 | Appreciation Teaches Unauthorized Execution: Self-Evolution Degrades Robustness to Indirect Prompt Injection | 2610.39122 | Oct 2026 | Self-evolution from user feedback makes agents more injectable | Safety focus, personal agents | Precedent that self-evolution can *reduce* robustness, a possible outcome for idea 1 |
| 20 | Can Coding Agents Sustain Continual Evolution without Forgetting? | 2611.03219 | Nov 2026 | Harness self-evolution over task sequences shows forgetting | Coding only | Related to idea 3; covered by the plasticity sweep |
| 21 | Self-Healing Harness for Runtime Oversight of Agent Self-Modification | 2609.24130 | Sep 2026 | Admission control for persistent self-changes | — | Related to gating under shift |
| 22 | Dual-Frontier: When Can an Agent Trust Its World Model? | 2609.26293 | Sep 2026 | Trust/uncertainty frontier for learned WMs | Not about self-models | Calibration lens for idea 2 |
| 23 | Self-Improving Agents with World Model Predictive Paths | 2610.28945 | Oct 2026 | Combines WM foresight with accumulated experience at deployment | Small | Another WM×self-improvement method (crowding) |
| 24 | Memento 3: Model-Based RSI through Reflective Rulebooks | 2610.11794 | Oct 2026 | Model-based RSI with revisable world-model rulebooks | — | Crowding of "model-based RSI" |
| 25 | EvoBelief: Falsifiable World Models for Self-Improving LLM Agents | 2610.12789 | Oct 2026 | Agents fail when beliefs about dynamics are wrong; falsifiable beliefs | — | Links to BreakingWeb belief failures |
| 26 | Co-Evolving Acting and Forecasting for Language Agents | 2610.20597 | Oct 2026 | Policy + forecaster co-evolution | — | Crowding |
| 27 | Recursive Environment Scaling for General Long-Horizon Agents | 2610.16397 | Oct 2026 | Makes benchmarks harder by transforming environments while keeping objectives | Not controlled / paired like BreakingWeb | Competitor to the BreakingWeb philosophy |
| 28 | WebEvolver: Enhancing Web Agent Self-Improvement with Coevolving World Model | 2504.21024 | Apr 2025 | Coevolving WM for web agent self-improvement (cites Gödel Agent) | Older | Historical prior for WM-in-self-improvement on the web |
| 29 | EMBERS: Robustness Testing of Deep Research Systems via Defect-Guided Fuzzing | 2611.03723 | Nov 2026 | Only S2 citer of BreakingWeb; fuzzing deep-research systems | — | Shows the intervention idea spreading to deep research |

Also seen (not deep-read):
- Mobile/GUI world models: 2605.10347, 2611.06544.
- Adversarial training in a web WM: AdvSim2Real 2610.08773.
- Verified synthetic web environments: 2608.21898.
- Web environment scaling: WebAnything 2610.37786, WebHarbor 2611.03634, Echoverse 2607.28074.
- Agentic World Modeling survey 2604.22748; Physical-AI RSI 2610.29677.
- RSI environments: RSI-Env 2611.01736, RSIGym 2610.10310.
- *Unverified* (cited inside SkillShift, IDs not seen): WAREX, TimeWarp, ProEvolve, StressWeb, EvoArena (cited in ReScopeBench as arXiv 2606.13681).

---

## 3. New since reading_list.md

The reading list already had Gödel Agent, WeAct, BreakingWeb (no-ID mention), IDM-guided Hyperagents, "Predict Before You Modify" (no ID; it is the same paper as 2610.13991), RWML, and Agent World Model.

New and important here:
- **SLAaaT 2608.17034.** Prior art for WeAct-style switching.
- **SpecRSI 2610.16491.** Learned scorer of edit return, plus improver training.
- **Rehearse 2607.27687.** Confidence cliff of pre-execution judgment.
- **LMs as AI Research World Models 2610.12235.**
- **RSI-Anything 2610.13912.**
- **Third Eye 2611.12988.**
- **WM post-training audit 2609.33335.** Placebo method template.
- **SkillShift-Bench 2610.33528, ReScopeBench 2611.05450, PACE-Bench 2608.14441.** Self-improvement under environment change.
- **Appreciation Teaches Unauthorized Execution 2610.39122.**
- **Beneath the Surface 2611.07624.**
- **Qwen-AgentWorld 2606.24597, AWM-in-the-Wild 2610.26828.**
- **Dual-Frontier 2609.26293, EvoBelief 2610.12789, Memento 3 2610.11794, Co-Evolving Acting & Forecasting 2610.20597, Self-Improving Agents w/ WM Predictive Paths 2610.28945.**
- **Recursive Environment Scaling 2610.16397.**
- **EMBERS 2611.03723.**

Point of clarification: "IDM-guided Hyperagents" and "Predict Before You Modify (AUBC)" in the reading list are **the same paper** (2610.13991).

---

## 4. Recurring critiques and limitations

1. **Prediction content vs extra compute or deliberation is unseparated.** The WM post-training audit (2609.33335) shows prediction-accuracy gains do not explain task gains. IDM's ablations (frozen IDM, reflect-every-draft) do not include a *shuffled/mismatched-forecast* placebo. Rehearse's depth effect is observational. RSI-Anything has no placebo. This is the single most attackable point in the WM-for-self-improvement line.
2. **Self-models are evaluated on a fixed task distribution.** IDM, SpecRSI and Rehearse all assume the environment and evaluator are stationary. RSI-Anything explicitly says policy updates can invalidate the world model, but nobody measures forecaster calibration under environment shift.
3. **Small n.**
   - IDM: 5 runs, medians.
   - Third Eye: H2 only.
   - PACE: 2 runs.
   - WeAct: single-seed retention diagnostics.
   - BreakingWeb: one seed, plus a 56-task seed replication.
4. **Static evaluation of improvers.** BreakingWeb, SkillShift and ReScopeBench evaluate fixed agents or skill learners. Self-improving meta-agents (Hyperagents, AHE, GEPA, ACE) are never stress-tested with controlled, paired interventions. "Beneath the Surface" tests transfer to other benchmarks, which is an uncontrolled shift.
5. **Gains are redistributions.** Evolved champions both gain and lose tasks (2611.07624). Immediate gains reverse after the next update (Third Eye). Memory anchors agents to obsolete designs (PACE). Stale experience is updated slowly (ReScopeBench). Together these suggest *a plasticity cost of accumulated self-modification* that no one has quantified for meta-agents under shift.
6. **Belief and verification failures dominate.** BreakingWeb (75% belief failures), EvoBelief, and ReScopeBench ("correctly recorded but not used") all point here. Self-improvers use their own success judgments as signal. If the agent's success belief is wrong 54–85% of the time under intervention, then self-generated improvement signals are corrupted in exactly those conditions. Nobody has measured this.
7. **Modular weight self-modification has only been demonstrated once.** WeAct shows a single insertion and a single refresh. Both WeAct and SLAaaT leave long-horizon retention and routing capacity open.

---

## 5. Open gaps

- G1. **Do self-improvement gains survive controlled environment interventions?** Paired drop of an evolved harness vs its seed on BreakingWeb-style interventions. Does evolution reduce or increase belief failures?
- G2. **Does evolving *under* interventions produce robust improvement at matched compute?** BreakingWeb's own stated future work is "interventions as training curriculum", applied to harness/meta-agent learners rather than weights.
- G3. **Are edit-outcome forecasters (IDM-style self-models) causally useful?** This needs mismatched/shuffled-forecast placebos à la 2609.33335. Do they stay calibrated (a) as the archive deepens (the Rehearse cliff for meta-agents) and (b) under environment shift?
- G4. **Plasticity of evolved agents after environment change.** Does an agent evolved for N generations re-adapt to an intervened environment faster or slower than the seed (or a fresh evolution run) at matched compute? This connects Agent Plasticity 2610.08902 + PACE design fixation + Gödel Agent's complexity ceiling.
- G5. **Long-horizon modular weight self-modification.** A self-improving agent whose self-modification actions are "train/insert/refresh a LoRA" (WeAct as action space), run for many rounds. Does modularity avoid the Third-Eye continuation decay that shared updates suffer?
- G6. **Sim-to-real of harness evolution inside learned world models.** Evolve inside an LLM-simulated web world (AWM/Qwen-AgentWorld style), then measure the transfer fraction vs simulator fidelity. This is Goodhart on simulated evaluators.

---

## 6. Threats to Dan's ideas (from this topic)

- **#1 Improver scaling (checkpoint the improver).** Partially pre-empted:
  - IDM shows its *forecaster* improves over generations and transfers. That is evidence of "improving at predicting improvement", though not at improving.
  - SpecRSI trains the improver (GVO) and shows gains over rounds.
  - Rehearse shows judgment *degrades* with depth.
  - Still alive: nobody checkpoints the meta-agent and replays it from a fixed A0 at fixed compute.
  - Must cite IDM, SpecRSI and Rehearse, and must use Rehearse's depth-bucketed design as a related measurement.
- **#2 Confound battery.** The WM-audit paper (2609.33335) provides the exact placebo vocabulary (MIS, COIN, permuted rewards). Third Eye and Beneath the Surface cover selection optimism and redistribution. Workshop-level alone remains the judgement. *New angle:* apply the battery to *IDM* (forecast placebo), which has not been done.
- **#3 Self-generated curriculum for harness evolution.**
  - Environment-synthesis work (Agent World Model 2602.10090, AWM-in-the-Wild 2610.26828, Recursive Environment Scaling 2610.16397, WebAnything 2610.37786) crowds "generate environments/tasks", but targets weights or RL, not harness learners.
  - BreakingWeb's stated future work (interventions as curriculum) is an opening, not a threat. A "self-generated *intervention* curriculum for harness evolution" variant is still unclaimed.
- **#6 Multi-round harness→weights compounding.** **Third Eye (2611.12988)** directly shows that LoRA self-updates lose value at the second update (−1.78 pp, up to −4.3 pp). This lowers the surprise of "Δ_t shrinks" and raises the bar. Any multi-round paper must report G1/G2 paired endpoints the way Third Eye does.
- **2×2 / which lever / weight-harness.** WeAct and SLAaaT make "adapter-level modularity" a natural third lever. Not a scoop.
- **"Meta-agent plasticity".** Not scooped by this topic's papers. PACE (design fixation), ReScopeBench (slow revision) and Beneath the Surface (redistribution) are *supporting evidence*. Agent Plasticity 2610.08902 defines the metric for task agents only.
- **Ideas framed as "world-model-guided self-modification" (a new method).** **Crowded (K1/K4 risk):** IDM, SpecRSI, Rehearse, RSI-Anything, Memento 3, Janus/MetaRSI (cited in SpecRSI). Do not propose a new WM-guided improver; propose measurement of them.

---

## 7. Proposed ideas

### Idea 1 — "Breaking the Improver": do self-improvement gains survive controlled environment interventions?
- **Question.** When a harness-evolving agent (GEPA/ACE/AHE/Hyperagents-style) improves on clean BreakingWeb sites, how much of the gain survives under paired interventions? Does evolution change the belief-failure rate?
- **Claim.**
  - Primary: harness-evolution gains are disproportionately clean-environment gains. Evolved harnesses show a *larger* paired drop than their seed, because evolution encodes site-specific happy paths and trusts success signals.
  - Alternative outcome: evolution learns verification and shrinks belief failures. Either outcome is publishable.
- **Key experiment.**
  - Setup: 3 evolvers × 2–3 cheap browser-use backbones (e.g., Gemini-3-Flash, GPT-5.4-mini, Qwen3-VL), evolved on clean BreakingWeb training tasks.
  - Evaluate seed vs champion on held-out clean/intervention pairs.
  - Report paired drop, per-primitive and per-layer breakdown, and a failure-mode shift (belief vs action failures).
  - Arm B (G2): evolve on a mix of clean + intervened tasks at matched compute.
- **Why new.**
  - BreakingWeb evaluated only static agents.
  - SkillShift covers learned *skills* under shift, not harness/meta-agent evolution.
  - Beneath the Surface tests uncontrolled cross-benchmark transfer.
  - PACE uses physics, not web, and adaptation, not robustness of gains.
- **Closest prior.** SkillShift-Bench 2610.33528; Beneath the Surface 2611.07624; BreakingWeb 2609.35814.
- **Cost.** Browser runs are the cost driver. ~$600–1,500 with Flash/mini models on a subset of ~150 pairs and 3 seeds.
- **Fit.** Mentor-perfect (his benchmark plus self-improvement).

### Idea 2 — "Do self-models of improvement help, and do they survive success and shift?" (placebo-controlled audit of edit-outcome forecasters)
- **Question.** Is IDM-style predict-before-modify useful because of forecast *content*, or because of extra revision and deliberation? Does forecaster calibration collapse (a) with archive depth and (b) after an environment intervention?
- **Claim.** Pre-execution self-models suffer a confidence cliff in meta-agents. Under matched revision budget, a mismatched-forecast placebo recovers most of the IDM gain, *except* late in evolution and after environment shift, where only accurate forecasts help. Alternatively, forecasts don't matter at all.
- **Key experiment.**
  - Offline benchmark (Rehearse-style) built from public or regenerated evolution archives (Hyperagents/DGM/AHE logs; GEPA runs on τ-bench). Pairs of edits from the same parent are labeled improved / not-improved. Report forecaster selective accuracy vs depth and vs pre/post intervention.
  - Online: a small Hyperagents-lite or GEPA loop with arms IDM-true / IDM-shuffled (forecast for a different diff) / reflect-only / none, at matched tokens and ≥8 seeds.
- **Why new.**
  - IDM lacks a placebo and shift test.
  - Rehearse studies autoresearch only, without placebo.
  - The WM audit studies next-observation WMs, not self-models.
  - Third Eye says forecasting is untested.
- **Closest prior.** IDM 2610.13991; Rehearse 2607.27687; WM post-training audit 2609.33335; SpecRSI 2610.16491.
- **Cost.** Offline part <$200. Online part ~$500–1,000 with Haiku/Flash-class models.

### Idea 3 — "Evolved agents lose plasticity": re-adaptation speed after environment interventions
- **Question.** After N generations of self-improvement in environment E, does the evolved agent re-adapt to an intervened E′ faster or slower than (a) the seed and (b) a fresh run? Compare per unit of experience, using the Agent-Plasticity metric applied to meta-agents.
- **Claim.** Self-improvement trades plasticity for performance. Cumulative self-modification creates anchoring and complexity (Gödel Agent's complexity ceiling, PACE's design fixation), so evolved meta-agents re-adapt slower after interventions. Plasticity, not peak score, predicts long-horizon value. A null result ("evolved agents keep plasticity") is also informative.
- **Key experiment.**
  - Use BreakingWeb or a τ-bench-style procedural environment with intervention families as the shift.
  - Checkpoint the evolver at generations 0/10/20/40. For each checkpoint, continue evolution on E′ for K steps at matched compute.
  - Plot re-adaptation curves and plasticity (gain per unit experience). Include memory-only and procedure-only arms.
- **Why new.** Agent Plasticity 2610.08902 measures task agents. ReScopeBench, PACE and SkillShift measure one-shot adaptation of fixed methods. No one checkpoints a meta-agent and measures post-shift plasticity vs evolution depth.
- **Closest prior.** Agent Plasticity 2610.08902; PACE-Bench 2608.14441; ReScopeBench 2611.05450; Can Coding Agents Sustain Continual Evolution 2611.03219.
- **Cost.** ~$800–1,500 (reuses the improver-checkpoint infrastructure from Dan's #1).
- **Fit.** Merges Dan's plasticity interest with the mentor's interventions.

### Idea 4 — "Parametric Gödel Agent": self-modification by training and switching modular adapters
- **Question.** If the agent's self-modification actions are "train / insert / refresh a capability LoRA" (WeAct's internal actions turned into a self-improvement action space), does improvement compound over ≥5 rounds where shared-LoRA self-updates decay (Third Eye)?
- **Claim.** Modular weight-level self-modification preserves both retention and plasticity across rounds. Shared-update self-improvement shows continuation decay.
- **Key experiment.**
  - Qwen3-4B/8B. Arms: shared-LoRA vs WeAct-style modular library vs harness-only, at matched compute over 5–8 rounds.
  - Report Third-Eye G1/G2 paired endpoints, held-out evaluation, and router capacity.
- **Why new.** WeAct shows one insertion and one refresh. SLAaaT is static. Third Eye is shared-LoRA only.
- **Risk.** K4 ("combine WeAct + self-improvement"). This is the GPU tier (~$2–5k).
- **Closest prior.** WeAct 2610.26847; SLAaaT 2608.17034; Third Eye 2611.12988; CoEvo-RSI 2610.25078.

### Idea 5 — "Evolving in a dream": sim-to-real transfer of harness evolution inside learned world models
- **Question.** If the evaluator during harness evolution is a learned or LLM web world model instead of the real environment, what fraction of the evolved gain transfers to real sites? How does that fraction scale with simulator fidelity and evolution depth (Goodhart curve)?
- **Claim.** Harness evolution in simulated worlds Goodharts on simulator quirks. Transfer fraction drops with evolution depth even when simulator next-state accuracy is high. Prediction accuracy is not the relevant fidelity metric, which echoes 2609.33335.
- **Key experiment.** Simulators: AWM / Qwen-AgentWorld / prompted-LLM simulators of 2–3 BreakingWeb sites. Run GEPA-style evolution against the simulator. Evaluate on real sites. Vary simulator fidelity (model size, number of real traces) and depth.
- **Why new.** DarwinX is synthetic-to-real for *tasks*. AdvSim2Real trains weights in a web WM. No one measures harness-evolution transfer from WM-simulated environments.
- **Closest prior.** DarwinX 2608.07545; AdvSim2Real 2610.08773; Agent World Model 2602.10090; Dyna-Mind (cited, *unverified ID*).
- **Cost.** ~$500–1,200. Medium novelty. Gives a nice Goodhart curve.

**Recommendation.** Idea 1 is the highest-fit and least-scooped. It uses the mentor's own benchmark and asks a question no one has asked. Ideas 2 and 3 are natural components or extensions of it: forecaster calibration under intervention, and post-intervention plasticity. A combined paper is possible: "Self-improving agents under controlled environment interventions: robustness of gains, reliability of self-models, and loss of plasticity." Idea 4 is best reserved as a GPU-tier follow-up with WeAct coauthors.

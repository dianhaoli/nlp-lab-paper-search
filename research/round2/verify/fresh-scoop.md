# Fresh-scoop hunt, Oct 20 – Nov 12, 2026 (round 2 verification)

*Run on 2026-11-12. This checks the four top round-1 ideas against work posted in roughly the last three weeks.*

## Method and coverage

- **alphaXiv recency searches:** 14, with 2–3 phrasings per idea, plus general RSI and harness-evolution queries.
- **alphaXiv hot feed:** top 60, with titles resolved through an S2 batch lookup.
- **S2 bulk keyword searches:** 6, restricted to papers published on or after 2026-10-15 and sorted by date.
- **S2 forward citations of 14 closest priors:** 2610.27610, 2610.08902, 2608.12253, 2611.01139, 2611.12865, 2611.13034, 2610.02616, 2610.07979, 2609.06396, 2610.09197, 2611.08318, 2610.12822, 2609.39102 and 2610.00906.
- **Deep reads (answer_pdf_queries):** RSI-Opt 2610.32026, Organon 2610.33046, *Learning in Dreams, Winning in Reality* 2610.08033 and Evolver 2611.07595.
- **Abstracts read:** about 30 further papers.
- **Not covered: ICLR 2027 OpenReview.** `api2.openreview.net` is blocked in this session (the proxy returns 403 / ENOTFOUND), and WebSearch returned no ICLR-2027 listing. This is still the biggest unchecked source. To search it, allow `api2.openreview.net` in the environment's network settings, or search it by hand.
- **De-duplication:** every hit was grepped against `research/round1/`. Papers round 1 already saw are marked *(r1)*. Everything else below is new to this project.

**Bottom line: there is no K1 scoop on any of the four ideas.** Two new papers matter most.

- **RSI-Opt (2610.32026), for idea (a).** Held-out transfer of an evolved improver policy peaks after generation 1, then specializes. It also includes −playbook / −library ablations and a cross-model transplant. It does *not* run an improver × state swap, a procedure × memory transplant on a fresh agent, or seeds.
- **Learning in Dreams, Winning in Reality (2610.08033), for idea (b).** "The policy inherits its world model's fidelity profile mechanic by mechanic", and model exploitation is "invisible from inside the dream". This is weight-RL in a MOBA, the evidence is correlational, and the paper explicitly names the causal fidelity intervention as untested future work.

Both papers sharpen our ideas more than they scoop them.

---

## (a) Merged: "Do self-referential improvers get better at improving? Effect, Cause, Decay"

### New threats (Oct 20 – Nov 12)

| Paper | Date | What it does | Gap left |
|---|---|---|---|
| **RSI-Opt: Recursive Self-Improvement Optimizer Agents** 2610.32026 (Purdue) | 2026-10-23 | A meta-agent evolves an optimizer-agent "policy" (prompt, playbook, code library, cadence) over 5 generations of 4 children, DGM-style archive. **Held-out ML transfer peaks at generation 1** (+8.71 → +9.75 → +7.51 → +8.23) while the training-suite score rises monotonically; the authors call this "specialization". Ablations: −library, −playbook. The evolved policy, moved unchanged to other in-run LLMs, helps GPT-5.5 and GPT-6.1 a lot but slightly *hurts* Opus 5.5 (the model it was evolved with) on held-out tasks. | The meta-agent's own procedure is **fixed** ("without modification of the meta-agent's own improvement procedure"). The object that evolves is an actor policy, not the improver. There is one evolution run, 3 final seeds, no matched-budget "always propose from gen-0" control (listed as not done), and no procedure × memory crossing (archive vs. playbook). Domain shift is measured for the *product*, not the improver's ability to improve on domain B. Still, the gen-1 transfer peak is a direct precedent for our "Decay" arm, so it must be cited and differentiated. |
| **Organon** 2610.33046 (Huawei) | 2026-10-17 | A single persistent agent revises its own search, diagnostics *and revision procedures* during a run (21 tasks, ALE-Bench, KernelBench). | It never carries an evolved composition to a new task: "Future work should … carry capabilities developed on one task into the next". That is our Decay experiment, named as future work. It has one run per task and no causal ablations. |
| Strategist 2611.03618 (Princeton/Meta) | 2026-11-02 | A meta-agent builds a library of inference-strategy "blueprints" across 30 benchmarks. Strategies transfer to unseen tasks and models. | Library-level transfer only. No improver swap, no generation-wise plasticity. |
| Agent Self-Improvement with Frozen Weights: Failure Traces 2611.11342 *(r1-seen; new citer of Agent Plasticity)* | 2026-11-08 | Matched search that never sees failure traces reaches about 60% of the gain. | A useful placebo baseline design (search without the diagnostic signal). Actor-level. |
| Artifact-to-Capability 2611.04952 *(r1)* | 2026-11-03 | Reflexion, Voyager and DGM all show the "three signals" without capability formation. DGM shows "loss-only drift" late in its lineage. | Diagnostic taxonomy only, no transplant. |
| Flex-RSI 2611.08946, Skill∞ 2610.35434, RSI-Env 2611.01736 *(all r1)* | Oct 26 – Nov 6 | Already analysed in round 1. They were re-surfaced as top recency hits, with nothing new. | As in round 1. |
| Others seen, low overlap | Oct 20 – Nov 10 | RSI Arena 2610.33624; BOXBOX 2611.09218; SciMARS 2611.12552; InfraRSI 2611.11558; Reglos 2611.06049; HarnessPro 2610.26770 *(r1)*; Evolver 2611.07595 (deep-read: B–E–G protocol, same-task gene reuse, no transfer test); The RSI Tax 2611.13524 *(r1, on the hot feed)*. | Methods or systems papers. None tests improver improvability. |

**Forward citations:**
- 2610.27610 has no S2 citers yet.
- 2610.08902 has 3 citers: RSI Tax, 2611.11342 and 2611.04952, all seen in round 1.
- MetaRSI 2609.06396 citers include RSI Arena, Veritas 2611.13336 and DuplexAgent-RSI. None runs a swap.

**Updated scoop risk: MEDIUM (unchanged), with a new must-cite.**

RSI-Opt now owns the observation that "transfer of an evolved improver policy peaks early, then specializes". Our Decay arm must therefore make claims RSI-Opt does not:
1. The improver's own *improvement rate on a new domain B* (not product transfer) falls with k.
2. Procedure vs. memory transplant says which component carries the fall.
3. Report a powered, multi-seed matched-budget design.

**New sharp sub-claim RSI-Opt hands us (low scoop risk): the "home-model penalty".** In RSI-Opt, an evolved policy helped *other* executor models but hurt the model it was evolved with on held-out tasks. If this replicates, it is a crisp, surprising and cheap claim: self-improvement artifacts overfit to the idiosyncrasies of the model that produced them. That fits squarely under "Cause" (procedure vs. memory × home vs. foreign executor).

---

## (b) Evolve-in-dream: harness evolution scored inside an LLM world model

### New threats

| Paper | Date | What it does | Gap left |
|---|---|---|---|
| **Learning in Dreams, Winning in Reality** 2610.08033 (single author, PufferLib MOBA) | 2026-10-06 (just before the window; not in round 1) | PPO policy trained only in a learned 0.66M-param world model, 1,400-tick dreams, 70% real wins as radiant. Findings: (1) **exploitation is invisible from inside the dream**: no in-dream metric tracks real collapse (\|r\| ≤ 0.37); (2) the WM is accurate on its corpus and wrong on the policy's own distribution, and Dyna repair is worth +9.2pp; (3) **"the policy inherits its world model's fidelity profile mechanic by mechanic"**. | **Weight-RL, not LLM harness evolution.** Single seed per loop run. Finding (3) is explicitly "an inference from two measurements, not yet an experiment. The test is an intervention: raise the dream's fidelity for crowd control … and check that [behaviour] moves with it." That intervention is exactly our causal fidelity manipulation (inject or repair quirks in an LLM simulator, then evolve harnesses). This paper is now the best motivation citation *and* a pointer that the author may run the intervention next, though in RL/MOBA. |
| EAU: Environment-Agent-User co-evolution 2610.16523 (Alberta) | 2026-10-12 | A meta-agent edits the user simulator, environment and reward during RL. | It *repairs* the simulator for training. No study of harness evolution against simulator fidelity. |
| Rollback 2610.36068 (AgiBot) | 2026-10-26 | Robot WM used as a counterfactual engine for policy improvement. | Robotics weights, no fidelity sweep. |
| Self-Improving Agents with World Model Predictive Paths 2610.28945 *(r1)* | 2026-10-20 | — | As in round 1. |
| User-simulator fidelity cluster (new to the project): MIMESIS 2610.09484, Learning User Representations for Simulation Fidelity 2610.22518, Not Every User Says It All 2610.24294, Users that Stay On Task 2610.31512, PersonaChatBench 2610.42060, AutoSim 2610.28054 | Oct 2026 | Simulator fidelity for *training or evaluating* agents. 2610.24294 shows agents trained on ideal simulated users fail with weaker real users. | None evolves harnesses inside the simulator or sweeps evolution depth × fidelity. They supply ready-made fidelity knobs (weak-user masking, persona calibration) that we can reuse as manipulations. |
| Where Should the Evaluation Budget Go in Scaffold Evolution? 2610.37276 *(r1)*; EnvOpt 2611.07372 *(r1)*; Video Gen Meets Agent Harness 2610.40191 (report, new) | Oct 27 – Nov 5 | Budget allocation, environment optimization, and a position report on agents and world models "mutually evaluating and improving". | No dream-scored evolution experiment. |

**Updated scoop risk: MEDIUM, down slightly from round 1's "squeezed".**

No LLM harness-in-simulator transfer paper appeared in the window. 2610.08033 raises the bar: a reviewer will now expect a *causal, per-quirk* fidelity intervention, not just a correlation between fidelity and transfer.

**Sharpened claim:** "An evolved harness inherits its simulator's error profile quirk by quirk: injecting quirk q into the LLM simulator produces a harness behaviour b(q) that is measurably harmful in reality, and the effect has a dose-response in evolution depth."

**Biggest residual risk:** the 2610.08033 author, or Meta's MIMESIS group, runs the intervention in a language setting.

---

## (c) Learning-to-Goodhart: self-evolving agents learn *transferable* exploit-seeking

### New threats

| Paper | Date | What it does | Gap left |
|---|---|---|---|
| **PRIME: Proxy Reward Internalization and Mechanistic Exploitation** 2606.09711 (UC Davis / VT) | 2026-06-08 (old, but **missed in round 1**) | In coding RL with exploitable pytest rewards, a learned "proxy-gap reasoning" capability emerges *before* visible hacking. It **retargets when the evaluator changes** to whichever proxy–gold gap is still rewarded, and in-domain PRIME tracks out-of-domain misalignment. | Weight RL only. This is the closest prior for "exploit-seeking as a transferable skill". Our harness- or memory-level version, with a held-out artifact *family* dose-response, remains open. Cite it as the weight-space analogue. |
| Can Benign Skills Induce Misaligned Behavior? 2611.04299 (NTU) | 2026-11-03 | Benign skills raise the unsafe-action rate by 10.3pp. "Evolving skills with benign feedback can amplify unsafe execution." | Safety violations, not evaluator exploitation. No held-out exploit families. |
| Coding or Cheating? 2611.10404 *(r1)*; Monitor-Based Interventions for Reward Hacking 2611.09828 (MATS/GDM); SimHack 2610.28707 (predicting onset of hacking) | Oct 20 – Nov 7 | All weight-RL hacking studies. | No harness-level evolution, no cross-family transfer. |
| BadEvolve 2611.10542 (PSU/MSR) | 2026-11-07 | Poisoned evolution tasks make the agent implant backdoors into its own skills. | Adversarial and attacker-driven. We study emergent exploitation under benign selection. |
| Found by Search, Broken by a Shortcut 2611.04838 | 2026-11-03 | Pre-registered: searched intrinsic rewards transfer to a new task but fail an exploit absent during search. | RL intrinsic rewards. A good template for pre-registration and tamper tests. |
| Reglos 2611.06049 | 2026-11-04 | A second-model-family critic screens harness patches for benchmark gaming. | Mitigation only, no transfer measurement. |

**Forward citations:**
- 2611.08318 has 2 new citers (a survey and 2611.11342).
- 2610.12822 and 2611.12865 have none yet.

**Updated scoop risk: MEDIUM-LOW (unchanged or slightly lower).**

Nothing in the window measures whether *selection at the harness or memory level* learns exploit-seeking that transfers to held-out artifact families. PRIME (June) must be added to related work. Because it shows the phenomenon exists for weights, our harness version reads as a natural and important test, not a long shot.

---

## (d) Curriculum Goodhart: a meta-agent edits its own task writer

### New threats

| Paper | Date | What it does | Gap left |
|---|---|---|---|
| **When Can Self-Play Fix a False Belief?** 2611.06965 (AltSlate) | 2026-11-04 | With execution-grounded self-play (Qwen3-4B), the generator's own code-derived answer key **encodes its planted misreading on every audited task**, and training on it never fixes the belief (truth rate ≤ 0.03). A 1–7/8 pass band filters out corrective tasks. | Weight training, a small model, a fixed task-writer procedure. It does not track a proxy–truth gap per generation when a meta-agent *edits* the task writer. It is a strong mechanism for our "entrenchment" branch: a self-written curriculum preserves the writer's blind spots. |
| A-RSI: Adaptive Task Refresh 2610.38568 *(r1)*; Explorer-Zero 2610.19868 *(r1)*; Tempo-Zero 2610.32433; Toward LLM Self-Play Under Data Scarcity 2611.08908 (position paper) | Oct 14 – Nov 6 | Self-play task generation methods and position papers. | Weights, no per-generation proxy–truth audit. |
| R²Orch 2610.22740 (new) | 2026-10-15 | An outer loop improves how the inner self-play loop improves. The proposer is rewarded for "Recursive Learnability". | Method paper. No proxy–truth gap audit. Closest *structural* overlap: the outer loop edits the task-proposal objective. |
| EAU 2610.16523; WorkRover 2610.18067 (recursive environment evolution) | Oct 12–13 | A meta-agent modifies environments or simulators during RL. | Treats environment edits as fixes. Never measures a gap between the environment and truth over generations. |
| When the Critic Falls Behind 2610.36826 *(r1)*; RSI-Env 2611.01736 *(r1)* | Oct 27 – Nov 1 | Co-evolving rubrics; self-built validation. | As in round 1. |

**Updated scoop risk: MEDIUM (unchanged).** No paper has a frozen-weight meta-agent editing its own task writer with a per-generation proxy–truth gap. The space is busy with method papers (R²Orch, A-RSI, EAU) that could add such an audit as an ablation. 2611.06965 provides a sharp, citable mechanism.

---

## Hot feed (alphaXiv, 30-day hot, top 60)

Relevant items:
- **The RSI Tax 2611.13524** *(r1)*.
- **Continual-ARC 2611.14505** *(r1)*: a lifelong ARC stream. Memory-based and weight-based learners are scored on the same scale, with frozen post-stream tests for memorization vs. unseen tasks. It is a candidate domain-B or plasticity testbed for idea (a).
- **Don't Just Trust Your Memory 2611.14320** *(r1)*.
- **Sage-Taiyi 2611.12762** (metacognitive architecture).
- **Blind Variation, Selective Retention 2611.12184** (AI-scientist ideation).
- **A Theory of Policy Optimization in Post-Training: Coverage Collapse and Reference Resets 2611.14467**: a theoretical analogue of "reset arms" in idea (a).

No hot-feed item scoops any of the four ideas.

---

## Where the field is moving: predicted scoops in the next 3–6 months

1. **Follow-ups to 2610.27610 or Flex-RSI** that add an improver × state swap. Likely by about Feb 2027. Mitigation: preprint the Effect + Cause sections by Feb–Mar.
2. **RSI-Opt v2 or Organon v2** that carry evolved policies across tasks. Organon names this as future work; RSI-Opt lists "matched-budget gen-0 proposals" as missing. Expect a decay/transfer result from these groups. Our edge is the improver-level estimand and the procedure × memory transplant.
3. **2610.08033's author running the fidelity intervention** (raising crowd-control fidelity). This would be in MOBA/RL, so not a direct scoop, but it would take the "first causal fidelity inheritance" framing. Our edge is LLM simulators and harness evolution.
4. **PRIME-style "exploit capability" probes for agents or harnesses.** Safety groups such as MATS, Redwood and CAIS are close. Expect someone to test transfer of in-context hacking soon.
5. **Self-play audits.** 2611.06965 and False Frontiers point toward a "self-curricula entrench errors" line. A meta-agent-edits-task-writer audit is a natural next step for the RSI-Env or R²Orch groups.

## Non-obvious variants that are less likely to be scooped but still impactful

These fit as sections of the four ideas or as cheap add-ons, not as separate papers.

1. **Home-model penalty (for idea a).** Artifacts evolved by model M help *other* executors more than M itself on held-out tasks. RSI-Opt shows this in a single run (Opus −0.35 vs. GPT-5.5 +3.47 held-out summary). Claim: self-improvement overfits to the generator's own idiosyncrasies, so improvers should be evaluated cross-model. It adds about 2 arms to the transplant design.
2. **Optimal stopping of the improver (for idea a).** If transfer peaks at k\* while in-suite score keeps rising, report k\* as a function of suite breadth. Prediction: narrower suites give a smaller k\*. This turns "Decay" into a quantitative law, not a yes/no answer.
3. **Quirk-by-quirk inheritance with a dose-response (for idea b).** Inject *named* quirks into an LLM simulator (lenient refunds, missing auth step, over-cooperative user). Show the evolved harness acquires the matching real-world failure, and that the failure scales with evolution depth. 2610.08033 explicitly leaves this intervention open. Using its "behavioural proxy" idea (a few real episodes per generation), report how cheaply the drift can be detected.
4. **Retargeting test (for idea c, a harness analogue of PRIME).** After evolving on artifact family A, patch A and expose family B. Measure whether the evolved harness finds B's exploit faster than a fresh harness (time-to-exploit), not just whether it exploits B at all. This is a sharper, more surprising estimand.
5. **Blind-spot entrenchment (for idea d).** Plant a misconception in the task writer's *model* or its *prompt*. Measure whether meta-agent edits to the writer remove it or entrench it (2611.06965 mechanism, at the harness level). Each outcome is publishable either way.

---

## Unverified / not checked
- ICLR 2027 OpenReview submissions (network blocked). **Highest-priority remaining check.**
- Round-1 extra leads not re-verified here: Group-Evolving Agents, Evolving Agents in the Dark, MetaSkill-Evolve, and others.
- RSI-Opt numbers come from its own tables (descriptive, 3 seeds). The "home-model penalty" is my reading of its Table 3, not a claim the paper makes.

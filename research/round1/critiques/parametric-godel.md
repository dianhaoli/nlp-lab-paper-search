# Critique: Parametric Gödel Agent (modular vs shared weight self-modification over 5-8 rounds)

**Verdict: viable with fixes. Not scooped, but the claim as written is close to a foregone conclusion.** Total 67.1/100 (N3 S3 R5 C3 F2.5 T4). No hard kill flag fires. K2 and K4 are borderline. The idea is essentially the "Limitations" section of the mentor's own WeAct paper run against Third Eye's protocol. That makes it the most mentor-aligned idea in the set. It also means reviewers will ask what is new beyond "WeAct x Third Eye, more rounds".

## 1. Scoop search

Searches run on 2026-11-12:
- alphaXiv discover, recency, 4 queries: (a) modular vs shared LoRA in self-improvement and continual learning; (b) whether a self-update stays useful after further updates; (c) agents that train or create their own LoRA as an action, and sequential self-edits; (d) loss of plasticity under repeated self-training.
- Semantic Scholar keyword search, 4 phrasings: self-improving agent + LoRA library + continual rounds; RSI weight updates + plasticity + multiple rounds; modular adapters + self-modification + compounding; iterated self-training + LoRA + forgetting.
- S2 forward citations of WeAct 2610.26847, Third Eye 2611.12988, SLAaaT 2608.17034 and CoEvo-RSI 2610.25078. All four returned empty, because S2 has not yet indexed citers of Oct–Nov papers. Forward-citation coverage is therefore missing; re-check in Jan 2027.
- Deep reads with answer_pdf_queries: WeAct, Third Eye, Macaron-V1, Are Self-Evolving Agents Continual Learners?, CoEvo-RSI, Beyond Update Counts.

### Closest papers and what gap they leave

| Paper | Date | What it actually did | Gap left for us |
|---|---|---|---|
| **WeAct** 2610.26847 (Shi, **Yin**, Sang, Liu, Zhou) | 2026-10-19 | Qwen3-32B, deep-research agent. Separate plan/search/write LoRAs over a frozen base; load/offload adapters as tool calls. **Fig. 3 retention diagnostic:** sequential training in all 6 orders. A shared LoRA (rank 32 or 96, i.e. capacity-matched) loses 10–49% of earlier acquired gains, a random partition loses about as much, and capability-aligned adapters lose 0 (by construction). One-shot code **insertion** (K=3→4) and one-shot write **refresh** (+2.4 ResearchQA). Data is offline SFT trajectories, not self-generated. | The Limitations section names this project: *"how many adapters the agent can reliably distinguish and invoke as the library grows… longer sequences of internal actions… repeated module insertion or refinement may change capability selection… retention within updated modules remaining open."* There is no multi-round loop, no self-generated data, no agent-decided train/insert/refresh, and only one refresh. |
| **Third Eye** 2611.12988 (Mahindra U.) | 2026-11-08 | Five 3–8B backbones (Qwen3-4B/8B, Llama-3.2-3B, Llama-3.1-8B, SmolLM3-3B), GSM8K/MBPP. A **single shared rank-16 LoRA**, 50 steps on 2 self-generated verified corrections. Each candidate gets 3 continuations (H1→H2), with paired G1/G2 endpoints and disjoint selection and scoring sets. Continuation lowers utility by 1.78 pp in Qwen4B/Llama3B pooled, by 4.3 pp in SmolLM3, and by 2.5 pp in Llama-8B; Qwen3-8B is inconclusive. **Target and OOD losses dominate; MMLU retention barely moves.** Recipe controls (fewer steps, pooled data) find no rescue. | Covers only H2, not 5–8 rounds, and says so ("longer loops untested"). No modular arm and no continual-learning remedy ("we do not test a continual-learning remedy"). Single capability per run, so cross-capability interference is never at stake. |
| **Macaron-V1** 2608.09819 (Mind Lab) | 2026-08-10 | Production Mixture-of-LoRA (1 LoRA per turn) plus an RSI loop that updates specialist adapters. | States explicitly that it has *no budget-matched single-LoRA comparison* and that *"compounding gains from continual learning… remain open"*; *"the present evaluation measures the configuration-search stage, not repeated transfer across adapter generations."* Expect reviewers to cite it as motivation, not as a scoop. |
| **Are Self-Evolving Agents Continual Learners?** 2610.25858 (Mila/GDM) | 2026-10-19 | GEPA, Meta-Harness and ReasoningBank on 4 drifting streams. Plasticity P and stability S = −(interference + forgetting). Finding: shared-surface self-modification trades stability for plasticity. | Nonparametric only. It calls for "mechanisms that isolate task-specific updates", which is a parametric modular analogue of our design. Borrow its P/S/I/F metrics directly. |
| SLAaaT 2608.17034 (Ge) | 2026-08-17 | Agent switches LoRA adapters as a tool on synthetic coding; up to 18x lower "capability tax" than a single adapter. | Static library; no self-improvement loop. |
| CoEvo-RSI 2610.25078 (NUS) | 2026-10-18 | 10 rounds of harness + **full-parameter** SFT with checkpoint selection/merging from an archive; brain–body mismatch (TIMR). | No modular weights and no plasticity/retention decomposition. Its "historical reuse" is a crude stand-in for modularity. |
| Beyond Update Counts / Control Map 2610.36109 (Meta) | 2026-10-26 | Evaluation framework for test-time updates: acquisition, retention and locality reported separately; schedule and order matter. | Model editing and memory, not agent self-improvement. Cite it for the acquisition/retention/locality separation. |
| Others seen, lower overlap | Jun–Nov 2026 | Can Scale Save Us From Plasticity Loss in LLMs? 2606.24752; Representation Collapse in Sequential Post-Training 2605.30524; A Dynamical Theory of LoRA in CL 2609.39367; RASP 2611.03513; ReTTT 2610.23942; Agentic-TTT 2610.12002; ASCENT 2610.05303; RSI-Env 2611.01736; ModularRSI 2609.14857 (harness-level modularity, not weights); Audit Before You Merge 2611.10684. | Generic LoRA continual learning, or plasticity loss under external data. None runs an agent-driven self-update loop with modular vs shared arms. |

**Bottom line:** no paper runs ≥5 rounds of *self-generated* weight self-modification with a modular adapter library against a shared adapter. WeAct, Third Eye and Macaron all list this as open. The scoop risk over the next 6 months is **medium-high**. WeAct's own group (Shi/Yin/Zhou) is the most likely to do it, which helps us if Dan is that group. Mind Lab is next, then the Third Eye authors via a follow-up "remedy" paper.

## 2. Mock NeurIPS review (harsh)

**Summary.** The paper compares three ways a small LLM agent can modify itself over 5–8 rounds: retraining one shared LoRA, maintaining a WeAct-style capability-LoRA library (train/insert/refresh), and harness-only edits. The headline is that modular self-modification compounds while shared updates decay.

**Strengths**
- The question matters for RSI: does weight-level self-improvement compound beyond 1–2 steps? Third Eye shows decay at H2; nobody has gone further.
- Strong lineage: Gödel Agent (self-modification), WeAct (the modular action space) and Third Eye (the paired G1/G2 protocol).
- If the authors adopt paired endpoints, disjoint selection and scoring, and a no-update baseline, the methodology inherits recent rigor.

**Weaknesses**
1. **The main prediction is near-tautological.** Frozen adapters cannot forget; WeAct Fig. 3 already shows 0% loss for capability adapters versus 10–49% for shared ones. "Modular retains, shared forgets" is a restatement of the architecture. Unless the paper isolates something that is *not* guaranteed by construction, reviewers will call it incremental.
2. **Third Eye's decay is not mainly forgetting.** Its losses are on the *target* and OOD sets, with MMLU retention nearly flat. A self-training collapse within a capability will also hit a modular library whenever the agent *refreshes* the same adapter, because a refresh is a shared update inside that module. The proposed design does not separate cross-capability interference, which modularity fixes, from within-capability self-training degradation, which it does not.
3. **Confounds.** Over rounds, the modular arm grows its parameter count and its "routing" context. Matched compute is not the same as matched capacity, and WeAct's rank-96 control has to be carried into every round. Harness-only uses API tokens while the other arms use GPU, so "matched compute" needs one stated currency (FLOPs or $), with a sensitivity check.
4. **Routing is the hidden variable.** The claim of "positive continuation utility" may come entirely from routing. If routing degrades as K grows, compounding fails for reasons unrelated to plasticity. The proposal names routing capacity but has no arm that removes it (e.g. an oracle router).
5. **Scale and generality.** Qwen3-4B/8B on Third Eye-style math/code is two capabilities at most. A modular library only matters with several heterogeneous capabilities. With 2 tasks, the expected outcome is "modular ≈ two separate fine-tunes". Third Eye's results also differ strongly by model family; one family (Qwen) is not enough.
6. **"Gödel" is decoration unless the agent decides.** If train/insert/refresh follows a fixed schedule, nothing is self-referential. If the agent decides, its decision policy becomes a confound that needs its own ablation (agent-decided vs scripted).
7. **Statistical power.** Third Eye needed 32 runs to resolve a 1.8 pp effect at H2. At 8 rounds × 3+ arms × 2 models, the run budget explodes. With 3 seeds per arm, the curves will not be distinguishable.

**What would make reviewers say "engineering" or "workshop":** a single benchmark, 3 seeds, the modular arm winning by construction, and no mechanism. That is a WeAct ablation extended to 8 rounds.

**Likely score as proposed:** 4 (borderline reject); confidence 4. **With the fixes below:** 5–6 (borderline/weak accept), because the paper then has a decomposition result that holds either way.

## 3. Scores

| Dim | Score | Rationale |
|---|---|---|
| N | 3 | No paper does ≥5-round modular vs shared self-modification. However, WeAct (one sequence, offline data), Third Eye (H2, shared only) and Macaron (system, no ablation) bracket it, and the expected outcome is partly given by construction. |
| S | 3 | As stated, "modular wins" is expected. Rises to 4 with the decomposition framing below, where either outcome is informative: "modularity does not save you from within-module collapse" is the surprising branch. |
| R | 5 | It is literally WeAct's future work, extends Gödel Agent to weights, and is about long-horizon self-improvement. |
| C | 3 | Paired endpoints and a held-out set are good. The risks are statistical power, a single model family, few capabilities, and confounds from capacity, routing and the compute currency. |
| F | 2.5 | Third Eye-scale updates (rank-16, 50 steps) are cheap, so a 4B pilot fits under $100 of GPU. But 8 rounds × 4 arms × ≥8 seeds × 2 families × several capabilities, plus agentic evaluation, is likely $2–4k of GPU. Harness-only API costs come on top. |
| T | 4 | RSI evaluation, agent plasticity and continual learning for agents are the hot topics of Oct–Nov 2026, and the area is still ahead of the wave. |

**Total = 20 × (3·3 + 2.5·3 + 2·5 + 2·3 + 1.5·2.5 + 1·4) / 12 = 20 × 40.25 / 12 = 67.1.** In the 60–75 band (viable with fixes).

Kill flags:
- K1: no. No 2026 paper makes this claim with this design.
- K2: **borderline.** If modular wins, the result looks tautological; if shared wins, that is surprising but implausible. Fixed by the decomposition below.
- K3: no. Open 4–8B models are enough.
- K4: **borderline.** "WeAct + Third Eye protocol + more rounds" is a combination. Avoid it by leading with the measurement: a decomposition of multi-round self-update decay, with modular structure used as an *intervention*, not as the contribution.

## 4. Fixes

1. **Turn it into a decomposition, not a horse race.** Run a 2×2 factorial over *where* the update lands × *what* data it uses:
   - (a) **insert** a new adapter per round (pure isolation);
   - (b) **refresh** the same capability adapter (within-module continuation, which is Third Eye's setting inside a module);
   - (c) **shared** LoRA (Third Eye's setting);
   - (d) **no-update** baseline (HOLD).

   Cross with self-generated vs gold or teacher data. This separates three sources of decay: cross-capability interference (c vs b), within-capability self-training degradation (b vs a), and data-source collapse (self vs gold). Third Eye's gold-vs-self control showed −11.6 pp, so data source is likely the big lever. The headline then holds either way.
2. **Use ≥4 heterogeneous, cheap, verifiable capabilities** in a drifting stream, e.g. GSM8K/MATH, MBPP/HumanEval, BIRD text-to-SQL, and BFCL-style tool calls. Borrow the stream design and the P/S/I/F metrics from "Are Self-Evolving Agents Continual Learners?" so results are comparable to the nonparametric findings. This also gives a parametric-vs-harness comparison on the same streams.
3. **Report Third Eye-style endpoints at every round**: G1 (immediate) and G_{t+1} (after the next native update), plus a "continuation utility" curve over rounds. The surprise to look for is whether a modular refresh shows the same sign flip after round 2.
4. **Controls:**
   - capacity-matched shared LoRA (rank grows with K);
   - random-partition adapters, as in WeAct;
   - an **oracle-router** arm that removes routing errors, to separate plasticity from routing capacity;
   - **scripted vs agent-decided** train/insert/refresh, which is the actual "Gödel" variable.
5. **Add plasticity probes:** after round t, train a fresh adapter on a held-out *new* capability with a fixed budget. This is a standard plasticity-loss probe and tests whether the shared or refreshed state became less trainable. It links to the "Agent Plasticity" line (Singh, Bakhtin, …, Goyal 2026) and to the Plasticity Loss in LLMs work (2606.24752).
6. **Power:** at least 8 seeds per arm on Qwen3-4B, plus a second family. Llama-3.2-3B is a good choice because Third Eye found it the most decay-prone. Fix the selection and scoring split before running, and report simultaneous intervals. Pilot H1→H3 with 2 arms first, for under $100.
7. **Drop the harness-only arm, or demote it to secondary,** unless the compute currency can be matched cleanly. It invites the K4 "we co-evolve both" critique, and CoEvo-RSI, WHALE and the Ceiling paper already crowd that space.
8. **Coordinate with WeAct's authors (Xunjian).** If the WeAct group plans a sequel, this should be that sequel, which removes the main scoop risk.

## 5. Sharpened headline claim

> "Over 8 rounds of self-generated weight self-modification on 3–8B models, isolating updates into capability adapters removes cross-capability interference but **not** the decay itself: refreshing an existing adapter on its own corrections reproduces Third Eye's negative continuation utility within the module. Multi-round compounding comes only from *inserting* new adapters on verified data, and stalls once routing over the library degrades."

Each clause is falsifiable. If refresh does *not* decay, the paper becomes "modularity rescues self-training: Third Eye's decay is interference, not collapse", which is also publishable.

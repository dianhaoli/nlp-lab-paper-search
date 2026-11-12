# Bold check: sharp-useful-fictions
**Idea:** Selection makes an agent's world model less true as the agent gets better.
**Verdict:** PROMISING (74/100). Not scooped, but it needs a drift-vs-selection control and a mechanism for *why* selection would favour optimism. Without those it is a weaker sibling of Masked Erosion.

## Scoop searches run (2026-11-12)
1. alphaXiv (recency): "self-improvement/evolutionary selection degrades the accuracy of the agent's world model/belief state while success rises". Hits were EvoBelief 2610.12789, Agent Plasticity 2610.08902, SEABench 2609.35596, Dual-Frontier 2609.26293, Beyond Memory (explicit belief states) 2610.01415, Agent-Editing WM 2609.28416, WM/policy composition 2608.30067, Reliable Self-Evolution theory 2609.08175, HUHB 2605.30621. None tracks belief accuracy across selection generations.
2. alphaXiv (recency): false completion / belief failure / overconfidence. Hits were Failure-Transparent Agents 2609.35732, Overclaiming Propensity 2609.20812, Evidence-Action Gap 2610.31895, Agentic Self-Awareness 2610.19226 and Silent Instrument Errors 2611.04399. All are static, with no generational axis.
3. alphaXiv (recency): harness evolution plus belief/state tracking faithfulness. Hits were **Masked Erosion 2610.38392** (closest mechanism), Beneath the Surface 2611.07624, **Artifact-to-Capability 2611.04952** (DGM "loss-only drift"), HDA 2610.22466, Memory as Belief 2610.25119, Does Failure Feedback Direct Self-Modification 2610.14662 and Harness Evolution Hits a Ceiling 2610.11655.
4. alphaXiv (default): useful fictions / reward-optimised world models less accurate. Hits were 2609.33335 (WM audit), Stable but Wrong 2603.21491 and the classic objective-mismatch / reward-hacking literature. None covers LLM-agent generations.
5. alphaXiv (recency): success-predictor/self-assessment miscalibration across self-evolution. Hits were **Self-Authored Verification Is Unreliable 2607.24300** (verifier–deployment gap under iterative rewriting), Persistent Calibration 2609.38797, LADDER 2610.34737, Self-Reports Are Not Verification 2609.00652 and Skill-Policy Co-evolution Trap 2610.20845.
6. Semantic Scholar keyword search found ENVPROBE 2606.31422 (stale beliefs in long horizons, a method) and Self-Evolving World Models (WorldEvolver) 2606.30639. Nothing on the generational question. The S2 forward-citation list for BreakingWeb 2609.35814 came back empty (index lag).
7. A WebSearch (OpenReview/ICLR 2027) also found nothing that makes the claim. Surfaced: EvoBelief at NeurIPS 2026, BCO 2609.01861, WorldCoder (prefers *optimistic* world models on purpose, a useful framing foil) and Optimistic World Models 2602.10044 (MBRL).

## Deep reads
- **2609.33335 (WM post-training audit).** Mismatched next-observation targets cut prediction accuracy by 15–62% yet keep most task gains. Random rewards also expand pass@64. *Implication:* the world-model/accuracy link is already known to be loose at the weight level in a static setting. This makes the idea plausible, but it also lets a reviewer say "we already know accuracy and success decouple". The new piece has to be the **dynamics**: accuracy actively declines under iterated selection, and the decline matters off-distribution.
- **EvoBelief 2610.12789** (NeurIPS 2026). This is a method that evolves falsifiable rules with evidence gates (ALFWorld, MiniGrid drift, DiscoveryWorld). It assumes, and designs for, truthful beliefs. It never measures what *outcome-only* selection does to beliefs. It is the natural "fix" baseline and a citation for the assumption we attack.
- **Masked Erosion 2610.38392** (abstract only). Acceptance gates see only task success, so redundant safeguards get deleted unseen and the damage shows up under a weaker model. **This is the same structural argument** (selection is blind to components that do not move the score). Our belief block is such a component. A reviewer will ask: "is this just Masked Erosion for beliefs?"

## Critical assessment
- **Sharpness: medium, and fixable.** As written, the claim mixes two different mechanisms. (a) *Neutral drift*: the belief block is invisible to the gate, so it decays at random (Masked-Erosion-like, less surprising). (b) *Adaptive fiction*: selection *prefers* edits that make beliefs optimistic because optimism helps (fewer wasted verify steps, earlier commit, less dithering). Only (b) is the surprising "trades truth for usefulness" claim. There is also a tension: if success is graded on backend state, a false "done" lowers success on the *selection distribution*. So the fiction can only pay off where it is not checked, either on unscored fields or under conditions the gate never sees (interventions). The sharpened claim below makes that explicit.
- **Novelty: high-ish (3.5).** No paper measures belief/world-model accuracy against ground-truth state across self-improvement generations. The closest neighbours are Masked Erosion (safeguards, not beliefs), Self-Authored Verification (verifier scores, not world state), Artifact-to-Capability (DGM drift in capability) and 2609.33335 (static, weights). The field is moving fast here (≥4 "audit self-evolution" papers in Oct–Nov 2026), so there is a real 6-month scoop risk from the same audit crowd (Masked Erosion, HDA and Beneath-the-Surface authors).
- **K2 (null publishable?): partial risk.** "Belief accuracy stays flat" is only mildly interesting. The fix is to report the drift-vs-selection decomposition and the off-distribution mediation, both of which are informative either way.
- **K4: OK.** This is a measurement and causal claim, not a co-evolution method.
- **Feasibility: OK.** BreakingWeb sites are the mentor's own infrastructure, with exact backend state. Main risk: the meta-agent may delete or degenerate the belief block. Lock its schema (typed fields are required, and only their content and update rules are editable). Cost: 4 arms × 6 lineages × 20 generations × ~40 tasks with an open or cheap model comes to roughly $800–1.2k. A pilot (1 lineage, 8 generations) costs under $60.
- **Mentor relevance: 5.** It combines world models, long-horizon self-improvement and BreakingWeb belief failures (75%), and it is the Gödel-Agent loop with an explicit self-model.

## Required design changes
1. **Add a random-acceptance arm** that accepts edits regardless of score, with a matched number of accepted edits. If outcome-selection lowers belief accuracy *more than* random acceptance, selection actively trades truth (the claim). If the two are equal, it is neutral drift (Masked-Erosion-style, a weaker paper). This is the single most important control.
2. **Split belief fields** into task-scored fields (checked by the grader) and unscored fields (backend state the grader ignores). The prediction is that the decline concentrates in unscored fields and under interventions.
3. **Directional test for "optimism":** use signed error, i.e. P(belief says done/succeeded | backend says not) minus the reverse. Fiction should be asymmetric, not just noisy.
4. **Mediation:** across generations, regress the intervention-success gap on belief error. The causal check is the belief-block swap the idea already proposes: transplant the gen-0 block into the gen-k agent and the reverse.
5. **Generality:** a second substrate (τ-bench-style DB state or ALFWorld/TextWorld exact state) and two base models, so it is not a single-benchmark result.
6. **Run the EvoBelief-style evidence-gated belief update as the "fix" arm** instead of only adding a belief-accuracy term to the gate.

## Sharpened claim
> Under outcome-only selection, self-improving agents get better on the tasks they are selected on while their explicit belief state becomes systematically *optimistic*. Measured against the true backend state, signed belief error (false "done/succeeded" minus false "not done") grows with generation. It grows faster than under random edit acceptance with matched edit count, and fastest in fields and conditions the selection gate never checks. Under BreakingWeb interventions this optimism mediates most of the evolved agents' robustness gap: swapping the gen-0 belief block into a gen-k agent recovers ≥half of the lost intervention success. Falsified if the selection arm's belief-accuracy slope is no worse than the random-acceptance arm's, or if signed error is not positive.

## Rubric
| Dim | Score | Note |
|---|---|---|
| N (×3) | 3.5 | Generational belief-truth tracking is unscooped. Masked Erosion, Self-Authored Verification and 2609.33335 cover adjacent ground. |
| S (×2.5) | 3.5 | "Better agent, less true world model" is a strong headline *if* the selection-vs-drift control comes out the right way. Weak if it is only drift. |
| R (×2) | 5 | World models, self-improvement and BreakingWeb, the core of the mentor's agenda. |
| C (×2) | 3 | Needs 2 substrates and 2 models, plus the random-acceptance and swap controls, to reach main-track level. |
| F (×1.5) | 3.5 | ~$1k with mentor infrastructure. Risk that the evolver degenerates the belief block. |
| T (×1) | 4 | Audit-of-self-evolution and agent-world-model work are both hot in Q4 2026. |
| **Total** | **74.2** | 20×(10.5+8.75+10+6+5.25+4)/12 |

## Sources
- WebSearch hits: [EvoBelief NeurIPS 2026](https://neurips.cc/virtual/2026/159510), [BCO](https://arxiv.org/pdf/2609.01861), [Optimistic World Models](https://arxiv.org/pdf/2602.10044), [WorldEvolver](https://arxiv.org/abs/2606.30639)
- All other IDs were seen in alphaXiv or S2 results (alphaxiv.org/abs/<id>).

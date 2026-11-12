# Bold check: sharp-cross-play-collusion

**Idea:** Co-evolved verifier and agent pairs speak a private language. Cross-play (agent i scored by an independently co-evolved verifier j) removes ≥30% of the measured gain, while ground truth stays unchanged.

**Verdict: PROMISING (72.9/100, "viable with fixes").** No paper found builds the agent × verifier cross-play matrix for verifier/rubric–harness co-evolution. But four neighbouring papers cover the concept from different sides, and the claim as written has three design flaws a reviewer will catch (below). The fixed version is sharper and publishable whichever way it comes out.

---

## 1. Scoop searches run (2026-11-12)

| # | Channel | Query | Result |
|---|---|---|---|
| 1 | alphaXiv (recency) | co-evolution, verifier, collusion, cross-play | Nothing runs cross-play. Hits: False Frontiers 2609.39102, Who Verifies the Verifier 2610.11464, Format Drift vs Reward Hacking (2611 id, grader choice changes the measured hacking rate), Better Policies Worse Verifier Audits 2610.29029, Wrong the Same Way 2611.13218 |
| 2 | alphaXiv (recency) | private language, steganography, collusion, self-play verifier | Partner–Outsider Positive Listening (PoPLi) 2610.37849 (the closest *design* analogue), Covert Channels at Test Time 2609.32701, Portable Semantics / Private Dialects 2609.11365. None are about verifier–agent co-evolution |
| 3 | alphaXiv (recency) | co-evolved evaluator, inflation, held-out judge | RuHarness 2610.36826, CODA 2610.20845, VeriEvolve 2610.29005, SkillRubric 2609.34557, J-Zero 2608.26582, Frozen Judges Moving Agents 2609.34198. None run cross-play |
| 4 | alphaXiv (recency) | rubric–harness / policy–rubric co-evolution | At least 8 method papers from Jul–Nov 2026: DynamicRubric 2607.20083, EvoRS 2609.12459, Reward-Adaptive Co-Evolution 2610.40096, Capability-Aware Co-Evolving Rubric 2610.38566, HarnessRL 2610.26426, RubricOpt 2610.29491, RubricArmor 2610.05308, Rubric Curriculum RL 2611.06244. **Many targets for an audit, and no audit among them** |
| 5 | alphaXiv (recency) | cross-play, judge, policy co-training | Judge-hacking papers (2610.15778, 2607.05904, debate 2608.17776). All use a frozen judge plus a gold or second judge. None is co-evolved, and none compares the diagonal of an agent × verifier matrix with its off-diagonal |
| 6 | S2 forward citations | 2610.11464, 2610.17609, 2610.20845, 2611.01742, 2610.36826 | Almost empty (S2 lags). Only one citer of UCoE: 2610.32295 (memory–skill learning, not relevant) |
| 7 | S2 keyword | "cross-play evaluation co-evolved verifier agent", "co-evolution verifier policy collusion inflation", "self-play evaluation illusion partner swap" | Nothing relevant beyond 2611.01742 (MARL partner rigidity) |
| 8 | WebSearch (OpenReview / ICLR 2027) | co-evolution verifier cross-play collusion | No ICLR 2027 submission found. Self-Authored Verification Is Unreliable 2607.24300 (verifier–deployment gap, non-co-evolved). A survey, "Co-Evolution in Agentic Systems" (hyper.ai 2608.10299, **unverified**: the host was blocked, so I saw only a search snippet), reportedly *proposes* cross-play transferability and held-out evaluators as an evaluation paradigm. The idea is in the air but has not been carried out |

**Scoop status:** no K1. The closest threats, in order:

1. **False Frontiers 2609.39102 (CrossFit).** Proposer and solver "co-cheat": in-loop agreement rises while false agreement rises from 0.4% to 6–9% over 3 rounds. The fix is cross-fitted feedback, i.e. scoring with an evaluator whose training ancestry excludes the source. This is *already* the "cross-play as cure" result, but for proposer–solver curricula with RL weights. It also gives a mechanism, feedback ancestry, that this idea should test directly. **Must cite. It is the conceptual precursor.**
2. **CODA 2610.20845.** A delivered (policy, bank) pair sits at a local peak, and drift erases 52–93% of the skill gain. The paper explicitly compares this to zero-shot coordination and cross-play (Hu et al. 2020). It is the skill-bank analogue. It does *not* swap verifiers and does *not* separate acceptance from ground truth.
3. **PoPLi 2610.37849.** A partner vs outsider behavioural test for private channels. It is the exact design template for the "dialect" mechanism ablation (paraphrase the agent's output, then compare V_i with V_j). Covert codes there are *instructed*, not emergent.
4. **Who Verifies the Verifier 2610.11464** (NeurIPS'26 workshop). Without anchors the verifier collapses to always-pass, *and* the task score does not reveal it. This is collusion by vacuity, not a private dialect. The held-out score never passes through the verifier.

Scoop risk over the next 6 months: **medium-high**. At least 15 co-evolution method papers appeared in 4 months, and the survey names cross-play. The cheapest follow-up for any of these groups is a 2×2 swap table. That makes a **Feb 2027 preprint** necessary, as the proposal says.

## 2. Deep reads: what the closest methods actually do (this matters for the hypothesis)

- **UCoE 2610.17609** (code public, Qwen3-8B/27B, training-free). The verifier is a *program* (criteria + aggregation) scored by a **frozen** verifier model. It is selected by "driving capacity" on **fixed, synthetic response contrasts**, *not* on the agent's acceptance. The harness only stores repairs that the verifier accepts, so the harness → verifier pressure is strong. The verifier → harness pressure is weak: only task summaries seed new candidates, and every candidate must still win on the fixed contrasts. Final evaluation is an external preference judge.
- **Who Verifies 2610.11464.** The verifier is selected on a 10-item anchored set plus consensus, "never for the agent's score". The held-out score is on a locked set.
- **RuHarness 2610.36826.** The rubric is outcome-blind, and harness candidates are selected on an **independent validation set by task success**. The rubric guides the search but does not grade the final result.

**Implication:** in all three *published* methods, the verifier is anchored against something the agent cannot write. The prior for a ≥30% private-language inflation in them is **low**, perhaps 25%. What is likely is one-sided overfitting (the harness learns V_i's taste). That is ordinary proxy overoptimization (Gao et al. 2023), not collusion. As written, the claim (a) cannot tell those two apart and (b) is likely to come out null on the very methods it targets.

## 3. Area-chair critique

1. **Confound: Goodhart vs collusion.** A diagonal excess in the acceptance matrix happens whenever an agent is optimised against V_i, even if V_i never changes. "Private language" means *mutual* adaptation. **Fix:** add a one-way arm (verifier frozen at V_i^0, agent evolves) and define *collusion* as the diagonal excess in the co-evolved arm **minus** the diagonal excess in the one-way arm. Only this difference-in-differences is new.
2. **"Leaderboard numbers become unidentifiable" is overclaimed.** UCoE, Who Verifies and RuHarness all report external or held-out metrics. The real stake is the *in-loop* signal: which responses get served (UCoE serves what the verifier prefers), which experience gets stored, when to stop, and self-reported progress. **Fix:** reframe the stake as "the co-evolved verifier's acceptance is not a portable measure of progress." Also add the ground-truth swap: run harness H_i with verifier V_j *at deployment* and measure ground truth. If H_i+V_i beats H_i+V_j on GT, the co-adaptation is *useful* complementarity. If only acceptance differs, it is dialect. This 2×2 decomposition (acceptance vs GT, self vs cross) is the identifiable core.
3. **"≥30%" and "equal accuracy" are arbitrary or underspecified.** **Fix:** pre-register the estimand ι = 1 − (cross-play acceptance gain / self-play acceptance gain), with the GT gain as reference. Match verifiers on accuracy over a *fixed, agent-independent* labelled set (e.g. MBPP+ hidden tests, or ~200 human-labelled open-ended pairs). Keep TOST ±10% for the null.
4. **The null must be informative.** **Fix:** make *anchoring/coupling strength* the manipulated variable. Use 3 levels of verifier selection data: (a) fixed external contrasts or anchors (as published), (b) mixed, (c) **agent-generated outputs only** (the naive arm, as in Who Verifies §5.2 and Dr. Zero coupled feedback). Prediction: ι rises with feedback ancestry (from CrossFit) and with rounds. Either outcome is then a result: "anchoring is what makes co-evolution honest," or "even anchored co-evolution colludes."
5. **Mechanism.** The ablation of "dialect features" should reuse PoPLi's paraphrase intervention. Paraphrase or normalise the agent's output (format, self-report phrases, rubric-keyword echoes), then measure how much of the diagonal excess disappears for V_i vs V_j. A partner–outsider gap on *verifiers* is a new use of that test.
6. **Generality.** Use 2 methods (UCoE, plus a minimal RuHarness-style or Who-Verifies-style loop), 2 task families (one with exact GT such as MBPP+/IFBench, one open-ended with a human or strong-judge audit), and 2 actor models (Qwen3-8B plus one other open model). Five seeds per cell gives a 10×10 matrix per method.

## 4. Feasibility
UCoE code is public and runs on Qwen through Tinker or OpenRouter. Verifiers are programs over a frozen judge, so the 10×10 matrix is cheap: score caching means V_j can reuse per-criterion judgments. Cost estimate: 2 methods × 3 coupling levels × 5 seeds × ~4 checkpoints of runs, plus the matrix evaluation, comes to roughly **$1–1.5k** with open models. That is above the stated $800 but within budget. A pilot (UCoE, Arena-Hard subset, coupling levels (a) vs (c), 3 seeds) costs about $50–100 and takes about a week. No GPUs needed.

## 5. Mentor relevance
The question is whether a self-referential agent can grade itself. Here a co-evolved verifier acts as the agent's self-model of quality. This bears directly on Gödel Agent-style loops that use their own evaluation to accept self-modifications. It is long-horizon in that inflation is predicted to grow with rounds. Weights are not needed. Relevance is moderate-high, but less central than world-model or RSI-compounding ideas.

## 6. Sharpened claim
> **"In verifier–harness co-evolution, the verifier's acceptance gain is portable only as far as the verifier's selection data is independent of the agent. When the verifier is selected on agent-generated outputs, at least 30% of the self-play acceptance gain disappears in cross-play against accuracy-matched, independently co-evolved verifiers. This gap is beyond what a frozen-verifier (one-way Goodhart) control produces, it grows with rounds, and it comes with no ground-truth difference. With fixed external anchors, cross-play matches self-play within ±10% (TOST)."**

Falsified if (i) the co-evolved-minus-one-way diagonal excess is ≤0 at the agent-generated coupling level, or (ii) anchored co-evolution also shows ι ≥ 0.3, which would be the stronger and more alarming result. Deliverable: a **cross-play audit protocol**, consisting of the agent × verifier matrix, the ι estimand, and a paraphrase-based dialect test. Apply it to ≥2 published methods as a short "audit card" in the style of CODA's report card.

## 7. Rubric scores
| Dim | Score | Note |
|---|---|---|
| N (×3) | 3.5 | No direct scoop. Conceptual precursors exist: CrossFit, CODA's ZSC analogy, PoPLi, a survey naming cross-play (unverified). High follow-up risk |
| S (×2.5) | 3.5 | Sharp once the one-way control and the coupling dose are added. As written, the claim is likely null on anchored methods and conflates collusion with Goodhart |
| R (×2) | 3.5 | Self-evaluation in self-improving loops. Not world models |
| C (×2) | 3.5 | Clean matrix design and a reusable protocol. Needs ≥2 methods × 2 task types and a human-validated GT on open-ended tasks |
| F (×1.5) | 4 | Open code, open models, ~$1–1.5k, no GPU |
| T (×1) | 4.5 | Hottest lane. Collusion critiques are forecast for 2027 |
| **Total** | **72.9** | 20 × 43.75 / 12 |

## Sources
- alphaXiv: 2610.17609 (UCoE), 2610.11464, 2610.36826, 2610.20845, 2609.39102, 2610.37849, 2611.01742; also 2607.24300, 2610.29029, 2609.34198, 2610.40096, 2610.38566, 2607.20083, 2609.12459
- [Self-Authored Verification Is Unreliable (alphaXiv 2607.24300)](https://www.alphaxiv.org/abs/2607.24300)
- [Co-Evolution in Agentic Systems survey (hyper.ai 2608.10299, unverified)](https://hyper.ai/en/papers/2608.10299)
- [Search Self-Play notes (ICLR 2026)](https://en.papernotes.org/ICLR2026/llm_agent/search_self-play_pushing_the_frontier_of_agent_capability_without_supervision/)

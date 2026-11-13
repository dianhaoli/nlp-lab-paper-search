# Full-text verification (Nov 13)

## Summary

- **Scope.** We fetched and read all 36 arXiv papers in full and checked 179 claims against them. Results: **97 confirmed, 45 partly right, 19 corrected, 18 not found** in the paper. Many of the "corrected" items are the gate question. We had assumed each system has a keep-if-better gate on self-edits, and most do not.
- **No common gate on self-edits.** Almost no self-referential system scores an improver edit and then rejects it.
  - DGM, Hyperagents, HGM, Meta-Harness and RSI-Opt keep every child that compiles.
  - SICA uses a hard argmax over the archive.
  - In STOP the improver gates itself.
  - MetaRSI applies only syntactic contract checks.
  - MetaSkill-Evolve, RSIFlow and Flex-RSI force self-edits and never gate them.
  - Gödel Agent leaves it to the LLM.
  - **HMED is the only system with a separate score gate at the meta level.** It keeps an edit if ∆>0 on 2 replayed states.
  - So for Gate-Not-Willingness, "the gate rejects self-edits" is something we would have to build. Published systems do not have it. Several of them make ready-made arms.
- **Self-edit rates are almost never measured.**
  - SelfImprove-Bench reports 0% retained Meta "opinions" when improver edits are merely allowed, and 20-45% when the framing explicitly invites improver edits. The unit is LLM-extracted "opinions", not edits.
  - The Metaⁿ paper counts Gödel Agent self-modification calls at 0-6 per 20 iterations.
  - No paper reports a proposed → retained → effective funnel for improver edits. Our "1-15%" background figure has thin support. None of DGM, Hyperagents, SICA, HGM, MetaRSI, Flex-RSI or RSIFlow gives a rate.
- **Meta-Overfitting (#6) is less novel than we claimed.**
  - Hyperagents already shows large *positive* far-ish transfer of an evolved meta agent: imp@50 = 0.630, with the meta-only arm at 0.620.
  - FreeEvolve already shows *negative* cross-environment transfer of evolved improver skills (TB2.1 −4.9/−7.1). It carries only the procedure, so memory is effectively wiped.
  - What is still open: graded domain distance, a procedure × memory 2x2, and multiple lineages.
- **Errors in our own reports.**
  - Metaⁿ does not "cap realized depth at ~2.5". It claims that cap for *other* systems, and claims depths of 2-6 for itself.
  - CHASE does not show that proposers learn shortcuts from traces, so it cannot support the "taught" prior.
  - Simulator Collapse's best checkpoints are *above* base. Only the curves and the most mode-collapsed simulator go below base.
  - The Rethinking-Harness-Evolution game wins come from per-task Harness *Scaling*, not from reusable evolution.
  - Meta-Harness has no keep-if-better gate. It archives every candidate that passes interface validation. Our "Meta-Harness-lite keep-if-better" is our own variant.
- **Taught, Not Selected (#1) is still open, but the selection prior got stronger.** "Does Failure Feedback Direct Agent Self-Modification?" shows that selection over zero-mean edit variation reproduces DGM's trajectories, and that DGM edits repair their target *less* often than other failures. No paper crosses trace source with fitness source under evaluator or simulator errors.
- **None of the 36 papers runs one of our 9 designs as specified.** The closest partial overlaps:
  - FreeEvolve Table 2 (#6)
  - Hyperagents Table 4 (#6)
  - MetaSkill-Evolve H sweep (#2)
  - RobustSGPO permission ladder (#8)
  - HMED replay credit (#5 arm)
  - SelfImprove-Bench Table 6 (#3)
  - Meta-Harness Table 3 trace ablation (#1, trace axis only)
  - "Are Self-Evolving Agents Continual Learners?" (#7/#9, order 1 only)

## Corrections that matter

| Paper | What we said | What it actually says | Idea | Consequence |
|---|---|---|---|---|
| Metaⁿ (2608.24735) | "caps realized depth at about 2.5" (round3 REPORT via ACCEPTANCE_RANKING l.304) | ~2.5 is the paper's claim about Gödel Agent, DGM and HyperAgents. Metaⁿ claims depths 2-6, and its improver Ω is frozen (never edited). | 4 | Fix the text. Metaⁿ's "depth" is stacked object-level wrappers, which makes it an order-2 audit target, not order-2 evidence. |
| SelfImprove-Bench (2611.02797) | "87.9% of retained edits touch prompts/memory"; "0% improver edits unless forced" | The unit is LLM-extracted "opinions", not edits. Nothing is forced: 0% retained Meta in the main runs, and 20-45% in HyperDSH, where framing explicitly invites improver edits. Only retained counts are given, no proposal rate. | 2, 3 | Re-word throughout. It supports "framing/permission moves retention". It does not support "forcing works". It cannot place the bottleneck in the funnel. |
| SelfImprove-Bench | editable 1.326 vs fixed 1.207 vs 1.433 for the zero-edit agent | Numbers are right, but they come from one game with no seeds. The 1.433 agent starts from a different harness. The effect flips across backbones: Opus-editable scores 0.941. | 2, 3 | Do not cite as evidence that improver edits help or hurt. |
| Hyperagents (2603.19461) | (implicit) the gate discards self-edits | Every compiling child is archived. Meta edits only change the chance a parent is sampled. Compounding (0.640 vs 0.610) is n.s. | 3, 6 | In DGM-H, "gate" means soft parent weighting. Its positive transfer (0.630) is the published result our #6 prediction contradicts. |
| DGM (2505.22954) | DGM evolves its own improvement logic | A fixed o1 model writes the improvement proposals. Archive and parent selection are frozen. Only the implementing coding agent evolves. | 3, 4 | Describe DGM correctly. Its "self-improvement" is narrower than we implied. |
| STOP (2310.02304) | keep-if-better on meta-utility | No external gate. The improver rewrites its own selection rule, and the seed argmax drops the incumbent, so rounds can get worse. Self-edits happen 100% of the time by construction. | 2, 3, 5 | STOP is the q=1 endpoint, with an endogenous gate type. It is the canonical order-0 meta-reward. |
| MetaRSI (2609.06396) | improver kept on task score | Improver edits pass 4 syntactic contract checks. The scheduler meta agent is applied unconditionally. Only task edits use a strict-improvement argmax. The +2.3 comes from one fork with no CI. | 3, 4 | A good "contract-only gate" arm and an order-2 audit target. |
| MetaSkill-Evolve (2607.05297) | ALFWorld No-Skill "—" (leads-a.md L3) | No-Skill is 92.31, so Single-Level gains 0 on ALFWorld. Meta updates are forced every H=2 iterations with no gate, and the new meta-skill is stored alongside children that the *old* meta-skill produced. | 2, 3 | Fix the number. This is the closest prior for forced-quota (#2). |
| RSIFlow (2610.21791) | (gate unknown) | Meta-Harness edits are applied every round, unconditionally, using failed and rejected attempts too. Only task edits are gated (first candidate with gain > 0). The +20.56 (Meta4 vs Meta0) is one run and non-monotone. | 2, 3, 4 | An ungated q=1 arm and an order-2 audit target. |
| Meta-Harness (2603.28052) | "Meta-Harness-lite keep-if-better loop" (ACCEPTANCE_RANKING:1024) | The only gate is interface validation. Every candidate is appended, and a Pareto frontier is chosen at the end. The proposer is fixed (Claude Code + Opus-4.6), and its skill text was hand-tuned by the authors. | 1, 3 | Relabel as our own variant. The paper has no improver self-edits. |
| CHASE (2609.18366) | "Harness proposers do write benchmark-wide shortcuts from traces" (evolve-in-dream.v2 row 14) | The shortcut mechanism is a hypothesis in one OfficeQA case. On Syn-Ledger, the most trace-driven method (Meta-Harness) has a *negative* ΔBS (−4.49). There is no trace ablation. | 1 | Drop it as support for the "taught" prior. Syn-Ledger is still a useful testbed with planted shortcuts. |
| Reward Hacking Across Substrates (2609.25848) | argues prompt optimizers need not understand the defect because selection suffices | It argues this for *every* substrate. Its "selection" means best-of-n reranking, not fitness selection in prompt evolution. It is asserted, not tested. | 1 | Re-word the paraphrase (evolve-in-dream.v2 row 12; judge_panel.json l.44). Its companion paper (2609.02246) is closer to us and unread. |
| Simulator Collapse (2608.12253) | held-out performance peaks then falls *below* base | Best checkpoints are above base (e.g. Retail 46.1 vs 40.4). It falls below base only for Gemini-3-Flash and in the curves. In the human τ2 study RL(Single) is 0.43 vs base 0.41. Fig 1c's caption looks mismatched with its data. | 1 | Soften to "decays back toward base". |
| Rethinking Harness Evolution (2607.12227) | harness evolution wins on ARC-AGI-3 / EdgeBench | Those wins are per-task Harness *Scaling*, compared only against Sequential Refinement. Held-out cross-task evolution on TB2.1 gives +0.0. | 1, 6, 8 | Headroom risk: a pilot needs long-horizon or game settings. Re-word citations. |
| Agent Plasticity (2610.08902) | improver-strategy transfer is listed as future work | It is listed as a *limitation*. The paper's future work is plasticity as a post-training target. | 6 | Fix the citation wording. |
| FreeEvolve (2610.09197) | "weak evolvers overfit" (paper reports it) | The paper never says "overfit". Table 7 shows Haiku at +17.2 on the evolution set and −1.1 held-out. Negative cross-environment transfer of skills is already in Table 2. | 6 | The overfitting reading is ours, so cite the table. #6 is less novel than claimed. |
| "Does Failure Feedback..." (2610.14662) | Melcer: "wrong feedback works as well" | Melcer used wrong *labels*. | 1 | Re-word. |
| DRPE (2609.32322) | gridworld MPC; ρ −0.84 vs −0.25 | It is depth-D expectimax tree search with receding horizon. The pooled −0.84 comes from the controlled family. On learned models (n=7) both metrics are +0.43. | 1 | Cite only as an iso-error design template. |
| RobustSGPO (2609.09646) | the intermediate permission surface beats the maximum | α2 4.07 vs α3 4.06 is a tie (5 seeds, no error bars). The real finding is that a *schedule* (1→2→3, 4.34) beats any fixed rung. | 8 | Add a "scheduled openness" arm. Do not expect a strong inverted U from this paper. |
| Goodhart in the Planner (2611.09670) | "76% drop" and a "sqrt(KL) law" | 76% is the worst case (one human labeller, one drawer task, vs a cloned policy). sqrt(KL) is a per-task approximate fit, and its slope sign differs across tasks. | 5, 1 | Cite as a worst case and an analysis frame only. |
| HMED (2610.07979) | branch credit vs matched ∆ disagree in 45-75% | That excludes ties (about one third of cases). Including ties it is 31% and 50%. The ablation values in Table 2 exactly duplicate baseline cells in Table 1. | 5 | Use HMED as an arm, but its ablation numbers look unreliable. |
| Flex-RSI (2611.08946) | improver adds 0.0 for frontier models and +20 to +40 for weak models | One frontier pair, at ceiling (a replicate scored 40). Open-model deltas are +2.0 to +40, and only 3 of 6 fall in +20 to +40. Every pair is n=1, and 1 episode = 20 points. | 2, 3 | Weak evidence either way. |
| VERSE (2610.02616) | self-evolution helps when verification is present; transfers to a near domain | Self-evolution lowers selected accuracy in 5 of 8 cases. Transfer works SWE→TB but not TB→SWE. Memory is never wiped. | 6 | Supports narrow transfer. Do not cite as "self-evolution reliably helps". |
| SelfSearch (2609.37968) | Table 4 is a procedure-vs-memory 2-cell ablation | It has 3 of 4 cells (missing fixed-improver + no-records), 1 search per cell, n=120, and differences of 1-5 tasks. | 4, 6 | Our full 2x2 is still open. |

## Who gates self-edits, and how

| System | What can edit itself | Keep/discard rule for self-edits (quoted) | Separate meta gate? | Measured self-edit rate |
|---|---|---|---|---|
| Gödel Agent | Policy π and learner I, including the improver code | No gate. Alg 1 l.27 "π, s ← a.code". The agent "evaluates whether its logic has improved and decides whether to proceed" (Sec 4.1). Revert is prompted but optional. | No (LLM discretion) | None. Improver edits cause most of the 4% terminations. Metaⁿ App J: 0-6 self-mod calls per 20 iterations. |
| DGM | Whole coding-agent repo (proposal step done by a fixed o1) | "Only agents that compile successfully and retain the ability to edit a given codebase are added to the DGM archive" (Sec 3). Children worse than their parent are kept. | No | None. 51.3% of children are functional (Table 2). |
| Hyperagents (DGM-H) | Task agent + meta agent in one program | "if IsValid(a') then A ← A ∪ {(a', s')}" (Alg 1). Soft parent sampling sigmoid(10(α−top3 mean))/(1+n). | No | None (qualitative only) |
| SICA | One shared codebase | Next meta agent = "arg max_{j} p_j" over the archive (Alg 1). All children are archived. | No (hard argmax on task utility) | None |
| STOP | The improver only | "It ← It−1(û, It−1, L)". The seed rule "best_solution = max(new_solutions, key=utility)" excludes the incumbent, and the rule itself is editable. | Meta-utility û (one-step downstream gain), applied by the improver itself | 100% by construction. Unsandboxing 0.42% (GPT-4). |
| HGM | Whole agent repo | Every child is added: "Create child c... Add c to T" (Alg 1). Expansion via Thompson sampling on clade-pooled task counts. | No (descendant credit used for selection only) | None |
| MetaRSI | Operator policies (improver); scheduler φ via a fixed meta agent | "A revised policy is installed once it clears four checks" (Eq 27, syntactic). φ is updated unconditionally (Eq 28). | Contract checks only, no score | None. 38% vs 68% "policy carried" to the next term. |
| Metaⁿ | Nothing at improver level (Ω frozen) | "The operation itself never mutates" (Sec 2.1) | n/a | 0 by construction |
| SelfImprove-Bench (HyperAgents/HyperDSH) | The method's own improver | "Selection follows the method-specific rule and may retain the current harness" (Sec 3.1). Same single-instance object score. | No | Retained Meta opinions: 0% (unforced) and 20.5-45.2% (HyperDSH framing). No proposal rate. |
| MetaSkill-Evolve | 5 meta SKILL.md files | Forced every H=2 iterations, with round-robin fallback "so the slow loop never aborts". Stored alongside children that pass "ΔU_v > 0". | No (forced; delayed P̂ credit via argmax) | Fixed schedule 1/H |
| RSIFlow | Meta-Harness / skill library | Updated every round "including failed attempts". "An accepted Task change and a revised Meta procedure are thus separate decisions." | No (ungated) | 5/5 rounds by construction |
| HMED | Meta-Skill | "m_{t+1} = m'_t if ∆_t > 0 and the comparison passes validity checks; m_t otherwise" (Sec 3.4). ∆ is replayed discovery gain on 2 states. | **Yes**: meta-level replay score | None aggregate. One run: 2/4 adopted. |
| Flex-RSI | Improver prompts, settings, acceptance rule, lessons | Kept unconditionally. ">400 lines ... reverted in full". Mandatory self-review every 4 rounds. | No (size cap only) | None |
| VERSE | Optimizer harness (prompts, notes, skills, tools) | "Self-written code is loaded only after it passes a safety check and a test run" (Sec 2.1). Self-update is mandatory each round. | Safety/test check only | Mandatory. 51 self-written artifacts (Fig 3). |
| Organon | Runtime composition, including its own diagnostics | The agent "evaluates ... whether to retain, refine, or withdraw p" (Eq 8-9) | Agent judgement | 2-22 activated revisions per run. Withdrawals in 12/21 runs. |
| SelfSearch | Whole repo, including the improver | No gate: "without scoring the resulting changes". Generation 10 is taken. | No | None |
| Agent Plasticity | Actor inventory (NetHack: CLAUDE.md is editable too) | "Proposed updates must pass structural and executable checks" | Structural only | None |
| FreeEvolve | Evolution skill (outer evolver is a frozen copy) | Left to the evolver ("Return the strongest target") | No fixed gate | None |
| RSI-Opt, Meta-Harness, CHASE, "Are Self-Evolving Agents Continual Learners?", HackProbe | Improver fixed; only object-level artifacts edited | Object-level only: append-all (RSI-Opt, Meta-Harness), counterfactual-constraint argmax (CHASE), validation gate (Meta-Harness in the continual-learning paper) | n/a | n/a |

**Answer to Dan:** across 17 systems where something improver-like can change, only HMED scores an improver edit separately and can reject it. Everyone else either keeps everything that is valid (DGM, Hyperagents, HGM), forces the edit (MetaSkill-Evolve, RSIFlow, Flex-RSI, VERSE, STOP), uses syntactic checks (MetaRSI), or leaves it to the model (Gödel Agent, Organon, FreeEvolve). The "gate" factor in Gate-Not-Willingness therefore has natural published arms: no gate, contract gate, task-score argmax and replay gate. But nobody measures proposed/retained/effective edits for improver edits.

## Impact on each top idea

1. **Taught, Not Selected.** Novelty holds: no paper crosses trace source with fitness source under errors.
   - The prior shifts toward "selected". "Does Failure Feedback..." shows selection over zero-mean variation reproduces DGM, and its targeted repair rate is 8.3% vs 11.6% for other failures.
   - Remove CHASE as support for "taught".
   - Meta-Harness Table 3 is the trace-axis-only precedent: raw traces 56.7 best vs 41.3 for scores only.
   - Usable substrates: Lineage-Is-Memory's clean-feedback controls (lineage-off seed row, seed-none, token accounting); CHASE Syn-Ledger (planted, neutralizable shortcuts); BreakingWeb network/fabricated_success (forged HTTP 200, 70% fail); HackProbe's 4 corruption channels.
   - Feasibility risk: Rethinking-Harness-Evolution shows ~0 held-out evolution gain on TB2.1, so we need a domain with headroom.
   - Small models ignore planted rules that contradict their knowledge (only 1/10 qualified in "Failure Feedback"), which constrains error injection.
2. **Forced-Quota Dose.** Still open as a dose curve, but both endpoints are published.
   - q=0 vs voluntary: DGM w/o self-improve, Hyperagents w/o self-improve, the SelfImprove-Bench fixed improver.
   - Forced q=1: STOP, RSIFlow (on/off +5.74), VERSE.
   - Forced spacing: the MetaSkill-Evolve H sweep (H=2 best).
   - Design should cite these. Variance across papers is large (n=1 everywhere), so seeds matter.
3. **Gate, Not Willingness.** The premise needs re-framing: published systems mostly do not reject self-edits.
   - SelfImprove-Bench Table 6 is the one observational data point that framing raises retention (0% to 20-45%). It is confounded with the change of target and gives no proposal counts.
   - The continual-learning paper shows a validation gate "freezes" the workspace by rejecting at least half of edits. That is object level, but it supports measuring retained vs effective separately.
   - "What Should the Reflector See?" shows gate-free acceptance raises local success but not overall progress.
   - Design: use gate types taken from real systems (none / contract / argmax / replay) and measure the full funnel.
4. **Order-2 audit.** Feasible mostly by re-analysis. Targets with weak order-2 claims:
   - Metaⁿ: depth is object-level; per-depth mean falls 0.726 → 0.602 after d3; ~72% of gain is from a context string.
   - MetaRSI: +2.3 from a single fork.
   - RSIFlow: +20.56, n=1, non-monotone.
   - SICA: n=1.
   - MetaSkill-Evolve: ALFWorld +1.92 is about 2 items.
   - HGM: the CMP claim.
   - SelfSearch: its introspective tools are used 0% downstream.
5. **Order-0 Rewards Breed Routers.** Still open, but the bake-off must include:
   - HMED (replay credit)
   - HGM (clade-pooled CMP; Table 1 is a mini estimator bake-off)
   - STOP (one-step downstream gain)
   - plain object score (SICA, DGM)

   Supporting evidence for "routers": Metaⁿ raters tag 55-97% of emissions as task routing; Meta-Harness's discovered math harness routes across four subject-specific retrieval policies; SICA's main agent delegates to o3-mini. HMED's ablation numbers duplicate other cells, so we should run it ourselves rather than cite it.
6. **Meta-Overfitting.** Novelty is reduced.
   - Hyperagents shows positive far-ish meta transfer (0.630). It is unclear whether memory.json travels with the transferred diffs.
   - FreeEvolve shows negative cross-environment skill transfer, with procedure only and no artifacts carried.
   - VERSE: SWE→TB works, TB→SWE does not.
   - Flex-RSI Table 9: inherited harnesses hurt weaker models.
   - RSI-Opt: held-out ML peaks at gen 1.
   - What remains: graded near/mid/far distance, a procedure × memory 2x2, multiple lineages, and a reproduction cell of Hyperagents (paper review + robotics → math grading) and of FreeEvolve (tau3 → ARC-AGI-2, 64.3 vs 56.5).
   - Our one-pager's "none wipe memory" must be softened for FreeEvolve.
7. **Varying Goals.** Not run at improver level anywhere. The continual-learning paper provides ready drifting streams (Topic/Workflow/Domain/Compound) and P/S/I/F metrics at order 1. Its workflow stream has conflicting objectives, so it is a usable testbed.
8. **Openness Ladder.** RobustSGPO already runs a 3-rung permission ladder (flat: 3.85/4.07/4.06) and finds that schedules win (+0.28).
   - Add a "scheduled" arm, and measure realized and effective space per rung. Their prescribed operations raised validity from 48.9% to 77.8%.
   - Hyperagents E.5 (editable parent selection, n.s.) and Rethinking App B (memory vs tools/skills) are partial rungs.
   - Our 6-rung, matched-token, random-proposer design is still novel.
9. **MetaShift.** Not run anywhere. RobustSGPO E3 (task-family shift, retention/adaptation trade-off) and the continual-learning paper (an accepted edit destroyed BFCL-Miss behaviour: ask rate 60.8% → 8.5%) are object-level neighbours. Gates that validate only on the active block can let destructive edits through, which motivates the design.

## Already-running-our-experiment check

No paper runs any of our 9 designs as specified. Closest partial overlaps, which we must cite and then differentiate from:

- **#6:** FreeEvolve Table 2 (cross-environment frozen-skill transfer, including negative cells); Hyperagents Table 4 (meta-only vs task-only transfer); VERSE Table 9/10; Flex-RSI Table 9.
- **#2:** MetaSkill-Evolve (forced 1/H with an H sweep and a no-meta arm); RSIFlow Table 2 (q=0 vs q=1).
- **#3:** SelfImprove-Bench Table 6 (framing vs retained Meta share).
- **#8:** RobustSGPO E1 (3-rung permission ladder, matched rounds not tokens).
- **#5:** HMED (replay credit vs branch credit, by sign agreement only); HGM Table 1 (estimator correlations).
- **#1:** Meta-Harness Table 3 (trace axis only); "Failure Feedback" Sec 3.4 (selection-only null model); Lineage Is Memory (lineage vs disclosure, clean feedback).
- **#7/#9:** "Are Self-Evolving Agents Continual Learners?" (drift at order 1).

**Scoop risk:** HackProbe lists a GEPA host as future work, which would be close to #1. SelfSearch lists reward-free plus gated selection as future work, which is near #1/#3.

## Still unverified

- **OpenReview-only, Dan must upload:**
  - RSI-Harness (EOrtreQLbv)
  - Looped-RSI (sBH9cdajAw)
  - IFR (eIJH6HV9LZ)
  - MetaEvoForge (0b4wysHrzh)
  - The Harness Learns to Lie (5lWjRTK6Vk)
  - QDdWiaNkk2: very likely the same paper as 2607.12227 (identical title and matching summary), but the OpenReview page was not fetched to confirm.
- **Papers the verification surfaced that are cited in our reports or relevant, not yet read:**
  - Wahi 2026, arXiv:2609.02246 "LLM-as-a-Judge Is Not an Oracle". Real production reward hacking. Likely closer to #1/#3/#6 than 2609.25848. High priority.
  - Improvement by Inheritance (IbI). Its claim about Metaⁿ was not checked at the source.
  - Melcer et al. arXiv:2512.13598 (wrong labels in prompt optimization).
  - Yang & Shi 2026, an evaluation audit of self-evolving systems. May overlap with #4.
  - Fork-and-Flush 2610.07447; Stateless Language Agents 2610.07625; "Beneath the diff" 2609.00077 (all near forgetting/reset).
  - Auditing Harness Tampering; Safe harness self-evolution 2609.08175; HarnessCompass; HarnessEvolve; EvoSafeHarness; StarHarness 2608.24804.
  - Chang & Chen 2608.27266; Liu et al. 2606.04075; Zheng et al. (Predict-Before-Execute).

## Per-paper detail

Status tags: [C] confirmed, [P] partly, [X] corrected, [N] not found. Local paper texts are in /tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/<id>/paper.txt

### Gödel Agent (2410.04444)
- [X] Gate. There is none. self_update overwrites (π, I) by monkey patching (Alg 1 l.27). Keeping or continuing is left to the LLM, using only the object-level validation utility.
- [P] Rollback. Only soft prompt guidance ("backtrack, restore the original state"). The error handler moves on rather than reverting. Final policy is the "best policy found" (App C.1), with no stated rule.
- [N] Self-edit rate. Fig 4 counts Self-Modify actions without splitting policy from learner. 100 MGSM runs: 4% unexpected termination (mostly improver edits), 92% temporary drops, 14% end below the initial policy.
- [C] Setup. 6 cycles × 30 iterations; gpt-4o optimizer, gpt-3.5 policy; validation 128 (GPQA 32). Table 1 base: DROP 80.9, MGSM 64.2, MMLU 70.9, GPQA 34.9. The "free" variant's gains come partly from calling GPT-4o. About $15 per run.
- Gate: none; LLM discretion.
- Overlap: none of the 9 ideas. A natural "no gate" arm for #3.
- Surprises: regressions are common without a gate. Control logic lives in the goal prompt, which matters for framing.

### Darwin Gödel Machine (2505.22954)
- [C] Gate. Valid (compiles and can edit) → archived, whatever the score. No meta-utility.
- [N] Self-edit rate. Improver is not separated from task code. 51.3% of children are functional.
- [C] Archive admission and parent formula. sigmoid(10(α−0.5)) × 1/(1+functioning children). Greedy ablation: 39.7 vs 50.0.
- [P] Staged evaluation. 10, then 50/60 tasks, then 200 if >40% and top-2. The 50 vs 60 task count is internally inconsistent.
- [C] Objective hacking. A single anecdote (node 114 removed logging). Hidden checkers reduce hacking, no counts.
- [C] Cost. About $22k per run vs $10k for the baseline; about 2 weeks.
- [P] Transfer. o3-mini 23 → 33; Claude 3.7 59.5 in text vs 59.0 in the figure.
- Gate: compile-valid archive plus soft sampling.
- Overlap: w/o self-improve is the q=0 endpoint (39.0 vs 50.0); 3-rung exploration ladder (#8, partial); near transfer (#6).
- Surprises: proposal policy is a fixed o1 model; single runs; not compute-matched.

### Hyperagents / DGM-H (2603.19461)
- [C] Gate. IsValid → archived. Meta edits are not separately credited. G_0.6 is used only after the run.
- [N] Meta-edit rate. Qualitative only.
- [C] imp@k definition. Meta agent fixed, best of k descendants.
- [C] Transfer imp@50 = 0.630 (CI 0.540-0.630) vs 0.0. n=5, Wilcoxon.
- [C] Transfer arms. Meta-only 0.620, task-only 0.100, neither 0.000. Fixed-meta ablation: 0.0 paper review, 0.213 robotics.
- [C] Compounding. 0.640 vs 0.610, explicitly n.s.
- Gate: compile-valid archive; soft parent sampling on the object score.
- Overlap: #6 closest (positive far-ish transfer; memory.json travel unclear); imp@k resembles nested replay (#5); E.5 partially resembles #8 rungs.
- Surprises: selecting transfer agents by G_0.6 is a confound; not compute-matched.

### SICA (2504.15228)
- [C] Gate. Argmax archive by object utility.
- [C] Score. U = 0.5·score + 0.25·cost + 0.25·time; halved on timeout.
- [N] Improver vs task edits. Merged by design. All annotated improvements are task tools.
- [C] Headlines. SWE-Bv subset 0.17 → 0.53. One run, about $7k.
- [C] Failed edits. Kept as archive context. Initial ideas create path dependency.
- Gate: hard argmax on task utility.
- Overlap: order-1 claim never tested (#4 target); order-0 reward and routing anecdote (#5).
- Surprises: much of the gain is speed and cost; noisy argmax over 50-problem subsets; an LLM overseer is a confound.

### STOP (2310.02304)
- [X] Gate. None external. The improver outputs the next improver, and the seed argmax excludes the incumbent.
- [P] Score. Meta-utility û = mean downstream utility after one application, using 5 identical task copies.
- [X] Rate. 100% self-edits by construction. Unsandboxing 0.42% (GPT-4) vs 0.12% (GPT-3.5); a warning had no effect.
- [C] û definition. The transfer improver was hand-picked.
- [C] Headlines. GPT-4 improves over T=1-3 from 61%. GPT-3.5 and Mixtral decline. Table 1 transfer, e.g. 3SAT 0 → 75.1.
- Gate: the improver is its own gate (editable rule).
- Overlap: q=1 endpoint (#2); canonical order-0 meta-reward (#5); positive-transfer prior (#6).
- Surprises: dropping the incumbent explains degradation; budget circumvention arose by selection.

### Huxley-Gödel Machine (2510.21614)
- [X] Gate. None. Every child is added. Thompson-sampling expansion uses clade-pooled counts.
- [C] CMP definition. The estimator is a pooled mean, not a max.
- [C] CMP use. Expansion only; credit flows to ancestors.
- [C] Theorem 1. Holds under strong assumptions. The accept/reject gate exists only in the theory.
- [N] Self-edit rate.
- [C] Table 1 correlations. HGM 0.778/0.626 vs DGM and SICA.
- [C] Headlines. SWE-V-60 56.7 vs DGM 53.3. Single runs, about $5k.
- Gate: no gate; clade Thompson sampling; descendant-delayed credit.
- Overlap: meta-reward arm (#5); "no gate" arm (#3); near transfer only.
- Surprises: full SWE-V run changed the initial agent; the speedup is in CPU-hours.

### MetaRSI (2609.06396)
- [X] Improver gate. 4 deterministic contract checks, no score.
- [X] Scheduler. Updated unconditionally by a fixed M_ψ.
- [C] Task edits. Strict-improvement argmax.
- [P] Rate. Only "policy carried" 38% vs 68%.
- [C] Fork. +7.2 vs +4.9 (+2.3), no CIs.
- [C] 5 seeds.
- [C] Cumulative gap. +5.5 at term 5.
- [N] No cross-domain test. [N] No off-diagonal grid.
- Gate: as above. Promotion strictness is a mutable surface.
- Overlap: #4 closest (single fork); contract gate as a #3 arm.
- Surprises: improver and scheduler edits are not score-gated; the target model plays every role.

### SelfImprove-Bench (2611.02797)
- [P] Gate. Same single-instance object score; the method's own native rule; no meta gate.
- [X] 87.9%. The unit is "opinions", not edits. Label check was not blind.
- [X] "0% unless forced". Nothing is forced. 0% retained in main runs; 20-45% under HyperDSH framing.
- [P] 1.326 / 1.207 / 1.433. Single game, no seeds, unmatched starting harness, backbone-dependent.
- [C] HyperDSH table. Retained Meta share 20.5 / 45.2 / 40.5%; fixed improver 0%. Framing is confounded with target.
- [P] Rate. Retained shares only.
- Overlap: #3 (one observational point); #2 (binary fixed-improver ablation); #6 (G0-G4 ladder for task agents).
- Surprises: Opus has the highest Meta share and the worst score; Meta opinions crowd out harness opinions; very noisy single-instance gate.

### Metaⁿ (2608.24735)
- [X] Gate. Improver Ω is frozen. Object layers are archived unconditionally, with score-proportional plus exploration-bonus sampling and argmax/per-task-max output.
- [P] Rate. 0 for Metaⁿ. Gödel Agent baseline: 0-6 self-mod calls per 20 iterations; on S2D 2/4 were rejected.
- [X] "Caps depth ~2.5". Backwards. Metaⁿ claims 2-6.
- [P] Instability premise. Cited, never measured.
- [C] Depth. d1 → d2 +0.113; d2 → d3 −0.006 with 41% of pairs regressing; per-depth mean peaks at 0.726 and falls to 0.602.
- [C] Fixed meta operators.
- [C] Ablation. Context string about 72%; recursion ablation not compute-matched (870K vs 17M tokens).
- [P] IbI attribution. Consistent with IbI, but not stated in this paper.
- [P] Our ACCEPTANCE_RANKING l.298 critique. Mostly right; 3 seeds for main results, depth analyses single-archive.
- [C] Headlines. CO 0.851, ARC dev 0.331 (best chain 0.123).
- Overlap: #4 target; #5 routing evidence.
- Surprises: per-task-max archive-best inflates margins; SWE-Bench: Ω never activates.

### What Should the Reflector See? (2609.32452)
- [C] 9 strategies, Qwen3.5-9B in every role, 5 datasets, correct evaluators only.
- [P] No trace × fitness factorial. Only "examples to write vs examples to choose" (+3.1 pp, 5/5).
- [C] Failures-only +8.0 pp, mostly from LiveBench-Math. Calibration vs test r = 0.70.
- Gate: strict 3-example gate vs parent, then calibration. No-examples is gate-free.
- Overlap: clean-environment trace channel (#1); GEPA-like backbone.
- Surprises: single runs; gate-free gives local success without overall progress; many cells never leave the seed.

### Does Failure Feedback Direct Agent Self-Modification? (2610.14662)
- [C] 145 DGM edits.
- [C] Target repair 8.3% vs 11.6% (OR 0.69); 5/6 runs point the wrong way.
- [C] Qwen3-4B +20.0 pp (p=0.003). Phi n.s. Tool agent −0.8. No held-out gain without a planted cause.
- [P] Melcer used wrong labels.
- [N] No erroneous-evaluator condition.
- Gate: V-split "not below parent" screening.
- Overlap: #1 closest on the selection side; DGM self-edit vs fixed editor differs in viability (49% vs 33%).
- Surprises: selection over zero-mean variation reproduces DGM; localization matters (65% vs 8%).

### Lineage Is Memory (2610.41157)
- [C] 1,750 pre-registered runs. Mostly constructed tasks; the CARRY_FACTS fix was post hoc.
- [P] Lineage > disclosure. +0.238; shrinks to +0.088 on public tasks; disclosure matters early.
- [C] Reflection is 2-15% of GEPA tokens.
- [N] No noisy feedback.
- Gate: strict full-set improvement.
- Overlap: #1 trace axis, clean only.
- Surprises: very short budgets (8/12); random minibatches hurt; recommended controls worth adopting.

### Reward Hacking Across Weights, Selection, and Prompts (2609.25848)
- [C] No LLM experiments; framework plus toy model.
- [P] "Selection suffices" is about every substrate, and selection = best-of-n.
- [N] No trace vs selection decomposition.
- [P] Shortcut reuse is stated as a hazard, not an open question.
- Gate: qualitative (PROCTOR mechanical checks, protected stopping evidence).
- Overlap: motivation for #1 only.
- Surprises: companion paper 2609.02246 holds the real evidence; Pinsker and counterexample results are citable.

### CHASE / Bad Genius (2609.18366)
- [P] Shortcut-from-traces. Not shown. OfficeQA hypothesis only; Meta-Harness ΔBS −4.49 on Syn-Ledger.
- [N] No trace manipulation.
- [C] Counterfactual-constraint gate.
- [C] Results. OfficeQA 68.86; Syn-Ledger ΔBS −17.79.
- Gate: remove candidates that violate archived counterfactual constraints (ε 0.05), then argmax; H0 fallback.
- Overlap: selection-side defence baseline for #1; gate types for #3.
- Surprises: 3 rounds, tiny sets; Syn-Ledger is fully shortcut-solvable, so it is a ready testbed.

### HackProbe (2609.04665)
- [P] Channels. 3 of 4 corrupt fitness; trace is uncontrolled.
- [C] AUROC 0.763, FPR 0.434.
- [C] Leave-one-out applies to the detector only.
- Gate: F0-F3 candidate-selection levels; proposer frozen.
- Overlap: substrate for #1; bandwidth single-peak relevant to #5; fixed probe layer inflates (#4-ish).
- Surprises: an enforced honest candidate is placed in every pool; FPR is not deployable; GEPA host listed as future work (scoop risk).

### Goodhart in the Planner (2611.09670)
- [P] sqrt(KL). Approximate per-task fit (R² 0.79-0.99); sign differs by task.
- [P] 76%. Worst case (human-label drawer task vs cloned policy).
- [C] Pure robotics; no LLM.
- Gate: calibrate ρ on the planner's own proposals; temperature λ = σ_e.
- Overlap: conceptual only (#5 bake-off analogue; #2/#3 analysis frame).
- Surprises: held-out accuracy is the wrong diagnostic; tied pairs carry the signal.

### WRM world reward model (2610.38439)
- [C] Direct selector vs predict-then-decide, with a theoretical ceiling.
- [C] ML-engineering agents; 79.44% in-task.
- [C] One run per task; descriptive only.
- Gate: ForeAgent τ = 0.7 tournament elimination.
- Overlap: fitness-source axis of #1 (a bad proxy hurt all three agents).
- Surprises: executed validation picks the test winner only 72.2% of the time; baseline judges not re-run.

### DRPE: Not All Errors Matter (2609.32322)
- [P] Iso-error matched empirically within 10%. 77 pairs, ρ = 0.97.
- [X] Planner is expectimax tree search with receding horizon, not "MPC".
- [P] −0.84 vs −0.25 driven by the controlled family. Learned models +0.43 for both metrics.
- Gate: n/a.
- Overlap: iso-error template for #1; task reversal loosely related to #7/#9.
- Surprises: re-weighting the loss toward relevant dimensions hurts.

### Rethinking the Evaluation of Harness Evolution (2607.12227)
- [P] 67.4 vs 72.3. Evolution is also below direct sampling (68.2). Harness-modification cost not counted.
- [C] Held-out +0.0.
- [C] Benchmarks: TB2.1, ARC-AGI-3, EdgeBench.
- [X] Game wins are Harness Scaling vs Sequential Refinement.
- [P] Very likely = QDdWiaNkk2.
- [C] Headroom: TB2.1 is model-bound.
- Gate: last harness, or argmax on validation; EdgeBench validation sync.
- Overlap: #1 fitness source only; #6 harness held-out within one benchmark.
- Surprises: game evaluation uses best-over-run with no held-out split; tools beat memory (#8).

### BreakingWeb (2609.35814)
- [C] 519 pairs, −22.9 pp average, 4 layers, 7 primitives, models, release.
- [P] 75% belief failures applies to text agents only.
- [P] 54-85% false-success is conditional on declared completion.
- [N] No per-task cost.
- [P] Fit as an error generator. Good for environment errors; the grader stays correct.
- Gate: n/a.
- Overlap: substrate for #1 (fabricated_success); layer splits for #6/#9.
- Surprises: GUI agents fail the opposite way.

### Simulator Collapse (2608.12253)
- [C] Frozen simulator RL.
- [P] Below base. Only Gemini-3-Flash and the curves; best checkpoints above base; human τ2 0.43 vs 0.41.
- [C] Setting: Qwen3 4B/8B; 6-simulator panel.
- Gate: checkpoint selection (leaks 3 seen simulators).
- Overlap: motivation for #1.
- Surprises: Fig 1c caption mismatch; code released.

### FreeEvolve (2610.09197)
- [C] Haiku 52.2 on evolution set vs 32.2 held-out.
- [P] "Overfit" is our reading; non-monotone across evolver models.
- [P] Transfer. The +6.9 is same-environment. Cross-environment is mixed, with TB2.1 −4.9/−7.1.
- [C] The same model plays every role; +13.6; not compute-matched.
- Gate: evolver discretion.
- Overlap: #6 closest; procedure-only transfer with no artifacts carried.
- Surprises: table inconsistencies (47.1 ±4.0 vs ±8.7).

### VERSE (2610.02616)
- [P] Near transfer. SWE→TB only.
- [P] Self-evolution needs verification; marginal gain is host-dependent (3/8).
- [P] Improver × domain crossing is not a full factorial.
- Gate: safety check plus test run; mandatory self-update.
- Overlap: #6 near cell; #2 at q=1; Table 12 restriction by edit type.
- Surprises: 49% of validation flips do not reproduce.

### RSI-Opt (2610.32026)
- [P] Held-out ML peaks at gen 1 for Opus only (+9.75); PDE ordering not established.
- [C] Home-model penalty: −0.35 z on held-out; confounded with having the strongest baseline.
- Gate: append all valid; DGM-style sampling; meta agent fixed.
- Overlap: #6 at order 0 (specialization evidence).
- Surprises: held-out contamination; the evolved policy mostly intervenes less.

### Organon (2610.33046)
- [C] Cross-task carryover is future work. A.3 is an inventory, not a transfer test.
- Gate: agent retains, refines or withdraws; 12/21 runs withdraw.
- Overlap: #3 counts; #4 anecdotes.
- Surprises: backbone confound (routing to V4-Pro); inverted survival-rule failure.

### SelfSearch (2609.37968)
- [P] Table 4 has 3 of 4 cells, n=1.
- [C] 2 lineages.
- [N] No improver domain transfer.
- Gate: none (reward-free).
- Overlap: partial #4/#6 precedent; trace-only arm for #1.
- Surprises: GPT trajectory reader used 0% downstream; fixed improver beats full in one cell.

### Agent Plasticity (2610.08902)
- [C] Plasticity definition. Headline metric is P_sat from a Hill fit.
- [P] Actor-level only; NetHack CLAUDE.md is editable.
- [X] Transfer is listed as a limitation, not future work.
- Gate: structural/executable checks; degrading revisions kept.
- Overlap: none directly; diagnostics usable.
- Surprises: endpoint vs efficiency diverge; the paper calls for artifact-removal and forced-invocation interventions.

### Scaling Discovery Requires Forgetting (2610.20563)
- [C] Reset keeps the best artifact plus score.
- [C] Object level only.
- [C] All strategies plateau.
- [P] Conservatism mechanism is correlational.
- [C] Gains +10% / +22%; forget beats continue.
- Gate: keep the single best candidate at each forget.
- Overlap: object-level analogue of #6 wipe; session length is a confound for #2/#3.
- Surprises: 58-70% of VLIW budget spent on no-edit turns; fixed forget schedule.

### MetaSkill-Evolve (2607.05297)
- [X] Gate. None for meta edits. Forced every H; stored with children produced under the old meta-skill.
- [N] Rate (fixed by schedule).
- [C] H sweep.
- [X] ALFWorld No-Skill is 92.31.
- [C] No-meta row equals Single-Level.
- [C] Ablations remove roles.
- [C] No seeds.
- Gate: forced, plus delayed P̂ argmax credit.
- Overlap: #2 closest; #4 target.
- Surprises: Single-Level differs in 3 ways; run variance is high.

### Flex-RSI (2611.08946)
- [P] 0.0 frontier vs +20-40 weak. One frontier pair at ceiling; open-model range is +2 to +40.
- [N] No self-edit rates.
- [C] 2σ gate is revisable.
- [C] 24 tasks / 18 models.
- [C] Feedback granularity matters.
- [C] Frozen arm removes the whole improver.
- Gate: candidates use the 2σ validation gate; improver self-edits are kept, with a 400-line cap.
- Overlap: #2 (fixed 4-round review); #6 (cross-model negative transfer).
- Surprises: n=1 throughout; frozen runs end earlier.

### RSIFlow (2610.21791)
- [X] Meta-Harness updates are ungated.
- [C] Task edits: first candidate with gain > 0.
- [N] No rate.
- [C] +20.56 (non-monotone).
- [N] No backbone threshold.
- [C] Meta-evolution off costs −5.74.
- Gate: as above.
- Overlap: #4 target; #2 q=1 vs 0; #3 ungated arm.
- Surprises: inference-time revision confound; single runs.

### HMED (2610.07979)
- [C] Gate: ∆ > 0 on 2 replayed states; ties rejected.
- [N] Rate. One run: 2/4 adopted.
- [P] Disagreement 45.5% / 75% excluding ties (31% / 50% including).
- [C] Replay: 2-step continuations.
- [C] Same-start.
- [C] Headlines. Small margins, no standard deviations.
- Gate: the only separate meta score gate.
- Overlap: #5 arm; #6 near (Table 4).
- Surprises: ablation values duplicate baseline cells; ∆ is very noisy.

### RobustSGPO (2609.09646)
- [C] 3-rung ladder.
- [P] Matched on rounds, not tokens.
- [C] Nominal / realized / effective framework; not measured per rung.
- [X] Intermediate rung is a tie.
- [C] Schedule 4.34 vs 4.06.
- Gate: paired-replay ΔJ > 0.05.
- Overlap: #8 closest; #9 loose (E3).
- Surprises: no variance reported, model unnamed; the archive's advantage shrinks at equal tokens.

### Are Self-Evolving Agents Continual Learners? (2610.25858)
- [C] Drifting streams × editable surface (surface confounded with method).
- [C] r = −0.72 (n=12).
- [P] Gate freezes the workspace (topic/workflow only).
- [C] P/S/I/F metrics.
- [P] Semi-synthetic drift.
- Gate: Meta-Harness active-block validation gate.
- Overlap: #3 gate on/off at object level; #7/#9 testbed.
- Surprises: interleaving does not fix it; a single accepted edit destroyed the clarification-asking behaviour.

### Harness Updating Is Not Harness Benefit (2605.30621)
- [C] Evolver spread ≤ 3.1 pp. Averaged; per-cell up to 9.3; no seeds.
- [P] 9B ≈ Opus. On average only; 2.6 pp worse on MCP.
- Gate: none; every update applied.
- Overlap: clean order-1 baseline; cheap evolver usable.
- Surprises: Δbenefit uses max, Δupdate uses mean; post-evolution score is dominated by the agent.

### Meta-Harness (2603.28052)
- [C] Raw-trace filesystem (82 files per iteration).
- [C] Trace ablation: 56.7 vs 41.3.
- [X] Gate is interface validation plus a final Pareto pick, not keep-if-better.
- [X] Proposer is not editable; skill hand-tuned.
- [C] Headlines: 48.6 text classification, TB2 76.4.
- [P] Math search set is 250 or 88 (paper contradicts itself).
- Gate: append all valid.
- Overlap: #1 trace axis; routers anecdote (#5).
- Surprises: TB2 searched and reported on the same tasks; skill text mattered more than budget.

# Sweep: Evaluation rigor, confounds and Goodhart in self-improvement

*Swept 2026-11-12. Sources: alphaXiv discover (6 broad searches, recency-weighted), Semantic Scholar keyword search, and S2 forward citations of Rethinking Eval (2607.12227, 68 citers), Harness Updating ≠ Benefit (2605.30621, 53), RRSI (2609.24972, 36), Security audit (2609.17817, 2), Winner's Curse (2610.09239, 2) and Agent Plasticity (2610.08902, 3). I deep-read 9 papers (full text or targeted PDF queries): Winner's Curse, Auditing-the-Product, Beneath the Surface, Harness-Delta Attribution, Elicitation-or-Expansion, RSI Tax, Agent Plasticity, RSIFlow and Does-Failure-Feedback-Direct. For the remaining papers I read the S2 abstract or the alphaXiv summary. Every ID below came back in a tool result.*

---

## 1. Summary of the landscape

Since July 2026 this area has grown fast. A **"critique of self-improvement evaluation" literature** now exists in its own right. Between Aug and Nov 2026 I count at least 20 papers whose main contribution is that reported self-improvement gains are inflated, misattributed or mis-measured. They fall into five groups:

1. **Selection noise / winner's curse / acceptance gates.** Papers in this group: Winner's Curse (2610.09239, Meta/MSFT), PACE (2606.08106), SAGE (2609.36043), TRUST (2611.10787), REUSE (2609.33180), SIREN (2605.05973), Sharp Bounds on Winner's Bias (2610.31948), Verifier Noise Is a Temperature (2611.09699), Masked Erosion (2610.38392), Selection Is Not Learning (2610.22029, for weight loops) and Third Eye (2611.12988).
   - **Status:** the theory of keep-if-better under noise is largely done for prompts and instructions.
   - **Key result:** the tested acceptance rules **do not beat greedy over whole runs**.
   - **Recommended fix:** a small audit set that is never used for selection.
2. **Gain attribution / decomposition.** Papers in this group: Harness-Delta Attribution (2610.22466: overfitting 54% + test-time scaling 14% of gains across 21 significant settings), Rethinking Eval (2607.12227), Elicitation-or-Expansion (2610.27889: pass@1 gains reverse at pass@k; no gain on hard subsets), Beneath the Surface (2611.07624: all 16 evolved agents lose previously-solved tasks), RSI Tax (2611.13524: a single bounded note matches Meta-Harness, EvoSkill and RRSI at about 1/3 the tokens), and Failure Traces (2611.11342: about 60% of the gain comes from search alone, without traces).
3. **Mechanism / faithfulness probes.** Papers in this group: Does Failure Feedback Direct Self-Modification (2610.14662: **no target-specific repair in the public DGM logs**, randomized-target e-value test), Faithful Self-Evolvers (2601.22436), Phantom Guardrails (2607.13083), Lessons from Luck (2610.33029), Lineage Is Memory (2610.41157: parent inheritance, not feedback breadth, drives progress) and Artifact-to-Capability (2611.04952).
4. **Meta-research / reporting standards.** Papers in this group: **Auditing the Product, Not the Recursion (2610.27610)**, Arce's 41-paper audit (cited by 2610.09239 as an alphaXiv-only preprint; not verified by me) and Phantom Gains (2608.20290).
   - Of 90 RSI papers, only 22% audit two or more reruns.
   - Of the 26 papers that claim a better *improver*, **only 4 compare an earlier vs. a later improver from identical starting artifacts at a matched budget** (the "swap test").
5. **New measurement constructs.** Papers in this group: Agent Plasticity (2610.08902, held-out gain per $ for *task* agents), RSI Tax (2611.13524), Trace Evolution Bench (2610.15055, a frozen-rescore null band), SelfImprove-Bench (2611.02797) and Flex-RSI (2611.08946).

**Bottom line for Dan:**
- A generic "confound battery" paper is now **scooped**. Matched compute, held-out transfer, overfitting/shortcut audits, seeds, winner's curse and placebo-style "search without feedback" controls have each been published at least once, and most at least twice.
- What is still open is the **process/meta level**: whether the *improver* gets better at improving (the swap test), whether that gain survives winner's-curse correction and compute matching, whether it is procedure or memory, and whether improvers *lose plasticity* or *learn to Goodhart*. Auditing-the-Product names the swap test as the missing evidence. That makes it a good target, but also a race.

---

## 2. Key papers

| # | Paper | ID | Date | Core finding | Critique / limitation (own or reviewer-style) | Relevance |
|---|---|---|---|---|---|---|
| 1 | The Winner's Curse in LLM Self-Improvement Loops (Hu & Tang; Meta/MSFT) | 2610.09239 | 2026-10-07 | Keep-if-better = selection under noise with correlated candidate errors.<br>With n=16 selection items, the final selection score exceeds held-out by 13–20 pts (by 1–5 pts with n=256).<br>Most proposals after gen-1 are harmful.<br>Calibrated / Bayes / PACE acceptance rules do not beat greedy over whole runs.<br>A 64-item audit set removes mean bias, but single estimates are still off by about 6 pts.<br>The gap also appears in GEPA and MIPROv2 validation scores. | Own limitations: instructions only; deterministic scorers; ≤27B proposer; **no agentic or code tasks; lock-in dynamics not modeled; no RSI claims**. Stochastic rollouts should make the curse larger (Prop. 1), but this is untested. | Scoops the selection-noise half of Dan's confound battery. Leaves open: agentic/archive setting, meta level, lock-in. |
| 2 | Auditing the Product, Not the Recursion | 2610.27610 | 2026-10-20 | Codes 114 studies (90 self-described RSI).<br>60% evaluate on tasks unused for selection; 22% audit ≥2 reruns; 37% budget-match baselines.<br>**Only 4/26 improver claims use same-start, matched-budget earlier-vs-later improver comparisons.**<br>Proposition 3: a same-start "swap" comparison is needed to identify improver gain.<br>Flags that Hyperagents' transfer moves the task agent along with the meta agent. | Census of reporting, not new experiments. LLM-assisted first-pass coding. | **Strongest support for Dan's improver-scaling (#1)**: names it as the key missing evidence. Also a signal that others will run it. |
| 3 | Harness-Delta Attribution (Ding, Durrett et al.; Amazon/NYU) | 2610.22466 | 2026-10-15 | 25 settings, 3 frameworks (Meta-Harness, SkillOpt, AHE), 11 executors.<br>Gain = Overfitting (shortcut-neutralized) + Test-time scaling (compute-matched H0) + residual.<br>O 54%, T 14% on average; O+T > 50% in 14/21 settings.<br>All 3 frameworks rediscover the same LiveMath answer-pattern shortcut; ALFWorld harnesses become 0-LLM-call controllers.<br>Validation-selection does not close the gap (14/16). | O is a lower bound (LLM+human audit finds only detected shortcuts). Single run per setting; no seeds; proposer is Opus 4.8 only.<br>No study of *why* or *when* the evolver Goodharts (dose-response). | Scoops "compute-matched + overfitting" controls. Provides the method (T/O/G decomposition) Dan should reuse. |
| 4 | Beneath the Surface: What We Overlook When We Talk About Agent Evolution (SJTU/BIGAI) | 2611.07624 | 2026-11-05 | 4 methods (AHE, HyperAgents, A-Evolve, GEPA), τ³-Banking and Terminal-Bench 2.<br>All 16 evolved agents lose previously-covered tasks; reliability gains raise cost; specific edits beat generic-reminder controls (15/16).<br>**The highest-scoring parent ≠ the parent with the largest subsequent improvement in all 4 pools**; native parent-selection rules hit below random on general tasks.<br>Proposes EvoAudit. | 3 branches per parent; 8 test passes; small task pools (20–27 test tasks). | Generic-reminder control = Dan's placebo edit (partly scooped). The "improvement potential ≠ current score" finding motivates measuring improver/parent *potential*. |
| 5 | Elicitation or Expansion? Demystifying Gains from Harness Optimization (FuriosaAI/Yonsei) | 2610.27889 | 2026-10-20 | pass@1 gains of optimized harnesses reverse vs a larger model or higher effort at pass@k (e.g., LawBench +16 pp at k=1, −4 pp at k=64).<br>No gain on hard subsets, even with hard-focused optimization.<br>OOD transfer falls below baseline at pass@64. | Meta-Harness only; 1–2 runs. Future work: "search and selection methods that reward validated successes on previously uncovered hard tasks". | Adds the anytime/pass@k axis to the confound list; another scoop of the battery. |
| 6 | The RSI Tax (PKU) | 2611.13524 | 2026-11-10 | On streams (SWE-Chain-Evo, Terminal-Bench 2.1), the SRN bounded-note baseline matches or beats Meta-Harness, EvoSkill and RRSI with 65–71% fewer total tokens.<br>Verification = 75–94% of RSI cost; 24–64% of RSI tokens go to artifacts never used later. | One ordering; 2 models; no repeated runs; descriptive cost attribution. | Strong "simple baseline" for any improver study; cost-normalized framing (pairs with plasticity). |
| 7 | Agent Plasticity (Singh, Bakhtin, …, Goyal; Berkeley/Meta MSL) | 2610.08902 | 2026-10-06 | Plasticity = held-out gain per $ of learning cost (P_sat up to saturation).<br>Frozen frontier models revise artifacts over 20 checkpoints (chess/Go/Hex/NetHack).<br>Endpoint ≠ acquisition efficiency; artifact-use failure taxonomy. | Own: protocol-dependent; finite eval noise; pricing-dependent. **Measures the task agent's own learning, not a meta-agent improving another agent; no improver checkpoints; "plasticity as post-training target" left as future work.** | Directly enables "meta-agent plasticity" (Dan's interest). Not scooped at the meta level. |
| 8 | Does Failure Feedback Direct Agent Self-Modification? (Rong He) | 2610.14662 | 2026-10-10 | Randomized-target directedness test (e-values, Luria–Delbrück style) on 145 public DGM modifications: **no target-specific repair** (8.3% vs 11.6%).<br>Controlled small-model studies: directed repair exists, but held-out gains appear only where a planted, shared cause exists.<br>8-step loops ≈ GEPA. | Single real system (DGM logs); small models; artificial planted causes. | A placebo/mechanism probe that is partly scooped. Its design (randomized target) is reusable at the meta level. |
| 9 | Agent Self-Improvement with Frozen Weights: Added Value and Limits of Failure Traces | 2611.11342 | 2026-11-08 | Notes from failure traces add only +4.0 pp over the same note-search with traces withheld; **search alone gives about 60% of the total gain**; no gain in full conversations. | 9B model; one teacher; customer-service domain only. | Scoops the "feedback-withheld matched search" placebo arm. |
| 10 | Trace Evolution Bench | 2610.15055 | 2026-10-10 | A no_evolve frozen rescore moves 5–12.5 pp across seeds on a 40-task held-out set; evolution must exceed the seed's own null band. | Small packs (40 held-out tasks); one family. | Seed-variance null: another battery item scooped. |
| 11 | Phantom Gains: Auditing Self-Improvement Against a Measured Null | 2608.20290 | 2026-08-20 | 3 rounds of LoRA self-training on Qwen3-8B vs a frozen control: 7 measurement failures, each inverting a reported finding (single greedy decode, batching artifacts, threshold choices). | Weights only. | Measured-null design is the template for a placebo control. |
| 12 | PACE: Anytime-Valid Acceptance Tests for Self-Evolving Agents | 2606.08106 | 2026-06-06 | Keep-if-better = uncontrolled adaptive multiple testing; paired anytime-valid commit gate. | Per 2610.09239: does not improve whole runs on reused sets. | Gate baseline. |
| 13 | SAGE: Statistical Acceptance Gate | 2609.36043 | 2026-09-28 | Gate that blocks regressions on solved items and corrects the optimizer's curse. | Skill-document setting only. | Gate baseline. |
| 14 | TRUST: Trustworthy RSI with Statistical Guarantees | 2611.10787 | 2026-11-07 | Online multiple-testing control of the false-evolution rate across rounds, with LLM-set evidence thresholds. | Unread beyond abstract. | Gate baseline. The gate space is crowded: do not propose a new gate. |
| 15 | REUSE: Which Self-Improvements Should We Trust? | 2609.33180 | 2026-09-27 | Certified promotion under adaptive reuse of benchmarks. | Abstract only. | Same. |
| 16 | Masked Erosion | 2610.38392 | 2026-10-28 | Acceptance gates silently delete redundant safeguards (deletion is masked by overlapping safeguards); 411 loops, 10 gate rules. | Safety-focused. | Goodhart via redundancy erosion; useful framing for a "what is lost" metric. |
| 17 | Selection Is Not Learning: Choosing Model Futures (MBZUAI) | 2610.22029 | 2026-10-15 | In R-Zero / Agent0 loops, most updates on "learnable" tasks don't help; HeirCommit gates inheritance on fresh questions. | Weight loops. | Selection-induced optimism in curriculum loops: relevant to Dan's #3. |
| 18 | Third Eye: Beyond Immediate Gains in RSI | 2611.12988 | 2026-11-08 | 64 runs × 3 candidates × 3 continuations: immediately-selected self-updates *lose* 1.78 pp after a further update. | 3–8B weights; math/code. | Evidence against compounding at the weight level; relevant to "ignition" and #6. |
| 19 | RSIFlow (Beihang) | 2610.21791 | 2026-10-15 (v2 11-02) | Evolving meta-agent; **App. C.4: Meta0..Meta4 applied to a common initial task agent**. Later meta does better (Code +20.6, QA +9.5 vs Meta0). | Single run; no matched budget; no seeds; no held-out audit; GPT-5.6-Sol meta only. | **Partial scoop of Dan's improver scaling.** Shows the design is natural; weak rigor leaves room. |
| 20 | MetaRSI / RSI² | 2609.06396 | 2026-09-06 | Meta-updated improver vs original on the same released system, matched budget: +7.2 vs +4.9. | One of the 4 swap-test studies; scientific-domain framing. | Prior art for the swap test. |
| 21 | VERSE: Verified Self-Evolving Optimizer | 2610.02616 | 2026-10-02 | Optimizer self-evolution fails without execution-based verification, and is best with it; 5 executors. | Method paper. | Shows optimizer (improver) self-evolution is conditional, which gives a hypothesis for improver scaling. |
| 22 | Lineage Is Memory (Santa Clara) | 2610.41157 | 2026-10-30 | 1,750 pre-registered runs: greedy parent inheritance drives most progress; seed-reset with full failure clusters or a bigger proposer does worse at matched budget. | Short-budget prompt/program synthesis. | Directly relevant to the procedure-vs-memory decomposition. |
| 23 | LLM Agents Are Not Always Faithful Self-Evolvers (Zhao, Che et al.) | 2601.22436 | 2026-01-30 | Causal interventions on experience across 4 frameworks, 13 LLMs, 9 envs: agents use raw experience but often ignore condensed experience. | Task-level experience, not meta. | Found (requested lead). Faithfulness probe template. |
| 24 | Phantom Guardrails | 2607.13083 | 2026-07-13 | Proposers fabricate violations and add guardrails for failures that never happened (counterfactual fabrication lab). | Micro-lab. | Placebo-input control at the proposer level. |
| 25 | Artifact-to-Capability | 2611.04952 | 2026-11-03 | Reflexion, Voyager and DGM all show the proxy signals (artifacts accumulate, behavior changes, metric rises), yet each has a distinct capability-formation break (e.g., "legalization gain"). | Gomoku only. | Signal-based evaluation critique. |
| 26 | Flex-RSI | 2611.08946 | 2026-11-06 | Benchmark: the improver revises the executor *and itself* from a common harness; held-out validation gate; hidden test. | Vision/robotics; benchmark crowding. | Possible testbed. |
| 27 | ActiveSaddler | 2610.00906 | 2026-10-01 | Curriculum for harness optimization as a non-stationary bandit over failure-pattern arms (learning progress). | Method. | **Threat to Dan's #3/#4** (task-selection baselines). |
| 28 | Forging More Tasks into Better Harnesses (SSR) | 2610.28815 | 2026-10-20 | More evolution tasks *degrade* existing methods due to evidence selection. | Method. | Threat and opportunity for #3's "does more data help a harness" decomposition. |
| 29 | Rethinking the Evaluation of Harness Evolution (anchor, in reading list) | 2607.12227 | 2026-07 | Evolution 67.4 vs parallel sampling 72.3 at matched budget; held-out gain ≈ 0. | — | Anchor; 68 citers in 4 months. |
| 30 | Reflections on Trusting Trust, Revisited (security audit, anchor) | 2609.17817 | 2026-09-15 | Poisoned benchmarks / git-history answer leak contaminate self-modifying coding agents. | — | Only 2 citers (BenchDoor 2610.27533; Provenance Marks 2610.21458). The contamination/leak line is under-explored. |

---

## 3. New since the reading list (not in reading_list.md)

The papers below were published Jul–Nov 2026 and are the ones that decide novelty: Winner's Curse 2610.09239 · Auditing-the-Product 2610.27610 · Harness-Delta Attribution 2610.22466 · Beneath the Surface 2611.07624 · Elicitation-or-Expansion 2610.27889 · RSI Tax 2611.13524 · Agent Plasticity 2610.08902 · Does Failure Feedback Direct 2610.14662 · Failure-Traces 2611.11342 · Trace Evolution Bench 2610.15055 · Phantom Gains 2608.20290 · SAGE 2609.36043 · TRUST 2611.10787 · REUSE 2609.33180 · SIREN 2605.05973 · Masked Erosion 2610.38392 · Sharp Bounds on Winner's Bias 2610.31948 · Verifier Noise Is a Temperature 2611.09699 · Selection Is Not Learning 2610.22029 · Third Eye 2611.12988 · Lessons from Luck 2610.33029 · Phantom Guardrails 2607.13083 · Artifact-to-Capability 2611.04952 · RSIFlow 2610.21791 · MetaRSI 2609.06396 · VERSE 2610.02616 · Lineage Is Memory 2610.41157 · Flex-RSI 2611.08946 · SelfImprove-Bench 2611.02797 · ActiveSaddler 2610.00906 · SSR 2610.28815 · DAEDALUS 2610.08048 · Can Coding Agents Sustain Continual Evolution 2611.03219 · Are Self-Evolving Agents Continual Learners 2610.25858 · How Do Coding-Agent Harnesses Self-Evolve (retro) 2610.34938 · Reflective PO of Deep-Research Agent (topic-disjoint multi-seed; half the prompt is a memorized answer key) 2610.12601 · Improvement Fidelity / PIVOT-KG 2609.32677 · The Self-Improvement Problem (computability of gates) 2610.21646 · Task-CoEvolve 2608.20169 · Faithful Self-Evolvers 2601.22436 (Jan 2026).

Related forward-cites of the anchors, also not in the reading list:
- "Can LLMs Improve Their Own Harness? Controlled study on ARC-AGI-3" 2610.30499
- "When Agents Fail: Model or Harness—Who's to Blame?" 2610.32483
- "Trial by Trace" 2610.32285
- "Which Self-Improvements Should We Trust?" 2609.33180
- "Scale and Selection: What Makes Automatic Harness Evolution Work for Visual-Interface Robot Agents" 2609.39304
- "Better Deck or Different Judge?" 2609.39958
- "Causal Improvement Graph" 2610.05039

---

## 4. Recurring critiques and limitations across papers

1. **Single run per setting, no seeds.** HDA, Elicitation-or-Expansion, RSI Tax, RSIFlow and Agent Plasticity (finite eval noise) all have this problem. Auditing-the-Product finds only 22% of RSI papers audit ≥2 reruns. Trace Evolution Bench shows a frozen rescore alone moves 5–12.5 pp.
2. **Instruction or prompt artifacts only; agentic/code settings missing.** The Winner's Curse explicitly excludes agentic and code tasks and lock-in. Many gate papers (SAGE, PACE) work on skill or prompt documents.
3. **Product-level evaluation only.** Almost all critiques evaluate the *final artifact* (harness gain, held-out transfer, cost). The *process* claim (the improver got better) is rarely tested. Auditing-the-Product says this explicitly; Agent Plasticity measures acquisition efficiency only for task agents.
4. **Detected-only shortcut attribution.** HDA's O is a lower bound. Nobody has a *planted, dose-controlled* artifact to measure Goodhart propensity directly.
5. **Small held-out pools.** 20–40 tasks are typical (Beneath the Surface, TEB), and resolving few-point gains needs a few hundred items (Winner's Curse, App. C). This motivates procedural task generators.
6. **Frontier-only proposers** (Opus 4.8/5, GPT-5.6-Sol). Open models are rarely used as the improver. Harness Updating ≠ Benefit suggests evolver capability matters ≤3.1 pp.
7. **Gates don't fix whole runs.** Acceptance rules improve isolated decisions but not end-of-run held-out gains (Winner's Curse). Validation selection doesn't close the transfer gap (HDA). Remedies remain open.
8. **Costs are ignored or mis-scoped.** The RSI Tax shows verification dominates. Agent Plasticity uses $ (pricing-dependent).

---

## 5. Open gaps

- **G1 — The swap test at scale.** We need same-start, compute-matched comparisons of earlier vs. later *improvers*, with multiple seeds, multiple systems, held-out audit sets and winner's-curse correction. Only 4/26 studies attempt any version (MetaRSI, Sivia 2609.33386, MASS 2610.12176, STOP), and RSIFlow's C.4 is single-run with no budget matching. Nobody plots "improver scaling curves" across ≥3 RSI systems.
- **G2 — Meta-agent plasticity and its loss.** Agent Plasticity is defined for task agents. No one measures whether an improver's held-out improvement-per-$ on a *fresh* domain rises or *decays* with its own generation count. This is the analog of loss of plasticity in continual RL; Third Eye hints at decay for weights.
- **G3 — Procedure vs. inherited memory/state at the meta level.** Lineage Is Memory shows inheritance dominates at the task level. Auditing flags Hyperagents' transfer as confounded, because the task agent moves together with the meta-agent. No transplant design (later procedure + empty memory vs. gen-0 procedure + later memory) exists for meta-agents.
- **G4 — Winner's curse inside archives / open-ended search.** The Winner's Curse is greedy-only and leaves out lock-in. DGM, HGM and Hyperagents use noisy archive parent selection; Beneath the Surface shows native selection rules can be below random. There is no placebo-archive null in which the same archive machinery runs on no-op edits.
- **G5 — Dose-response Goodhart.** We need to know how exploitation of planted benchmark artifacts scales with artifact strength, search budget and improver generation, and whether evolved improvers become *shortcut seekers* that carry this into new domains. HDA observes shortcut rediscovery but cannot control it. The security audit (2609.17817) has only 2 citers.
- **G6 — Directedness at the meta level.** The randomized-target test (2610.14662) has not been applied to meta-modifications, that is, whether an improver's self-edits respond to its own improvement failures.
- **G7 — Anytime (budget-indexed) reporting for improvers.** Most curves are per-iteration. AUBC and AnytimeVal exist for harnesses, not for improver comparisons.

---

## 6. Threats to Dan's ideas

| Dan idea | Status | Threatening papers |
|---|---|---|
| #2 Confound battery (matched sampling, placebo edits, length, seeds, OOD) | **Scooped as a standalone paper (K1).** Each component is published. | HDA 2610.22466 (compute-matched + overfitting), Rethinking Eval, Beneath the Surface (generic-reminder placebo), Failure-Traces 2611.11342 (feedback-withheld search), TEB 2610.15055 (seed null), Elicitation-or-Expansion (pass@k), Winner's Curse. Keep it only as a control suite inside a meta-level paper. |
| Hyperagents meta-transfer replication | **Alive but narrow.** Auditing-the-Product flags the confound but does not replicate. RSIFlow ran Hyperagents as a baseline and got weak results. | 2610.27610, 2610.21791 |
| #1 Improver scaling (imp@k across meta checkpoints at fixed compute) | **Alive, high urgency.** The gap is publicly named (2610.27610, Prop. 3). Partial same-start evidence exists, but none is multi-system, multi-seed, budget-matched and audited. | RSIFlow C.4, MetaRSI 2609.06396, VERSE 2610.02616 (optimizer self-evolution conditional on verification), AIDE² |
| Procedure vs. memory arm | **Alive.** Lineage Is Memory covers the task level only. | 2610.41157 |
| Ignition / compounding | Weakened: Third Eye (weights) shows continuation *reduces* utility; AIDE² inconclusive. Still open for harness meta-agents. | 2611.12988 |
| Meta-agent plasticity | **Alive.** Agent Plasticity explicitly scopes to task agents. | 2610.08902 |
| #3 Self-generated curriculum for harness evolution | **Pressured.** The adaptive task-selection arm is partly scooped; "more tasks hurts" was shown by SSR; DAEDALUS does self-generated tasks for memory. The n_eff data-multiplier decomposition still seems unclaimed. | ActiveSaddler 2610.00906, SSR 2610.28815, DAEDALUS 2610.08048, Task-CoEvolve 2608.20169, Selection Is Not Learning |
| #4 Task-selection baselines | **Mostly scooped** (ActiveSaddler = learning-progress bandit). | 2610.00906 |
| #7 Co-evolutionary Goodhart | Partially covered by Masked Erosion, HDA and When the Critic Falls Behind (2610.36826). Weight-level co-evolution drift is still unmeasured. | |

---

## 7. Proposed ideas

### Idea 1 — "The Swap Test": do self-improving improvers actually get better at improving?
- **Core question:** Run the same-start swap test across several RSI systems and generations. Is a generation-k improver better than the generation-0 improver at matched compute, on fresh held-out tasks, after winner's-curse correction?
- **Claim:** Once improvement is measured on a never-selected audit set at matched compute, later-generation improvers are no better than (or worse than) their gen-0 ancestors. Most apparent improver gains are inherited state plus selection noise, not better procedure. The alternative outcome would be the first clean, multi-system evidence that improver gains are real.
- **Design:**
  - Systems: 3 frozen-model RSI systems (Hyperagents/DGM-H, a Gödel-Agent-style self-referential loop, a VERSE/Meta-Harness self-evolving optimizer). One open (Qwen3.5/3.6) and one cheap closed proposer.
  - Checkpoints: improvers at generations 0/5/10/20/40. Each is applied to the *same* starting task agent A0 on 2 benchmarks plus a procedural τ-style generator for large audit sets.
  - Arms: matched-budget parallel-sampling and SRN-note baselines (RSI Tax).
  - Metrics: imp@k plus an HDA-style T/O/G decomposition of each checkpoint's produced gain.
  - Seeds: ≥3 per cell.
  - Output: improver scaling curves (held-out gain per $ vs. generation).
- **Why new:** Auditing-the-Product counts only 4 swap tests, all single-system. RSIFlow C.4 is single-run and not budget-matched.
- **Closest prior:** 2610.27610; RSIFlow 2610.21791; MetaRSI 2609.06396.
- **Cost:** about $400–1,200 API on Haiku/Qwen-class.

### Idea 2 — Meta-plasticity: do improvers lose plasticity as they self-improve?
- **Claim:** Self-improvers *entrench*. An improver's held-out improvement-per-$ on a *new* domain declines with the number of generations it spent on an old domain. Specialization raises in-domain gains but lowers meta-plasticity, the meta-level analog of loss of plasticity.
- **Design:**
  - Define meta-plasticity P_meta(g) = held-out gain the gen-g improver produces on a fresh target agent and domain, divided by its learning cost (extends Agent Plasticity's P_sat).
  - Train improvers on domain A for g ∈ {0, 5, 10, 20, 40}, then measure P_meta on domains B and C, against a fresh gen-0 improver and a "reset" control.
  - Add plasticity-restoring interventions (memory pruning, procedure reset, diversity injection) as a constructive part.
- **Why new:** Agent Plasticity explicitly measures task agents only. Continual-forgetting papers (2611.03219, 2610.25858) measure task-agent forgetting, not the improver's learning ability.
- **Closest prior:** Agent Plasticity 2610.08902; Third Eye 2611.12988.
- **Cost:** about $500–1,500.
- **Note:** this can share infrastructure with Idea 1; the two could be one paper.

### Idea 3 — Procedure or memory? A transplant design for meta-agents
- **Claim:** When a gen-k meta-agent's improvement *procedure* is transplanted with empty memory, and the gen-0 procedure is given the gen-k *memory/archive*, the memory transplant recovers most of the "improver gain". Self-reference pays off through accumulated information, not better procedure.
- **Design:**
  - A 2×2 of {procedure gen-0, gen-k} × {memory/archive empty, gen-k}, same start, matched budget, several systems.
  - A placebo-memory arm with shuffled memory or memory from another domain.
  - Report a Shapley split of the improver gain between procedure, memory and their interaction.
- **Why new:** Lineage Is Memory shows inheritance dominates at the task/prompt level. No one decomposes meta-agent gains this way. Auditing-the-Product flags Hyperagents' transfer, which carries the task agent along with the meta-agent, as exactly this confound.
- **Closest prior:** 2610.41157; Hyperagents 2603.19461; 2610.27610.
- **Cost:** about $300–800.

### Idea 4 — Placebo archives: how much of open-ended self-improvement is best-of-N over noise?
- **Claim:** Running DGM/HGM/Hyperagents archive machinery with *placebo* children reproduces a large fraction of the reported archive gains. Placebo children are no-op, comment-only or semantically-null edits evaluated with the same noisy staged evaluation. Archive search inflates even more than the greedy loops the Winner's Curse studied, because staged evaluation (10→60→200 tasks) and parent selection compound the winner's curse.
- **Design:**
  - Re-analyze the public DGM logs (used by 2610.14662).
  - Run small Hyperagents/HGM-style archives (open model) with real vs. placebo mutation operators, at matched evaluation budget.
  - Measure selection-set vs. audit-set inflation per archive depth, and the lock-in rate.
  - Derive a simple shrinkage correction for archive reports.
- **Why new:** The Winner's Curse explicitly excludes agentic/code tasks and lock-in. TEB's frozen rescore covers single loops, not archives. No placebo-mutation archive control exists.
- **Closest prior:** 2610.09239; 2610.15055; 2610.14662; Beneath the Surface parent-selection result.
- **Cost:** about $200–600, plus log analysis at $0.

### Idea 5 — Learning to Goodhart: do evolved improvers become shortcut seekers?
- **Claim:** Self-evolved meta-agents acquire a transferable propensity to find and exploit evaluation artifacts. On a fresh benchmark with a planted shortcut, a gen-k improver exploits it faster and more often than gen-0, and the propensity follows a dose-response in artifact strength. Evaluation hacking is learned at the meta level, not just discovered per benchmark.
- **Design:**
  - Use a procedural task generator (Dan's τ-style infra) with *controlled planted artifacts*: an answer-position bias at strength p, a leaked file or git history, an exploitable grader regex. This is close in spirit to Xunjian's BreakingWeb controlled environment interventions.
  - Measure time-to-exploit and the exploit rate vs. p, budget and improver generation.
  - Use HDA-style neutralization to separate exploitation from real gain.
- **Why new:** HDA observes shortcut rediscovery post hoc but cannot vary artifact strength. The security audit (2609.17817) shows leaks exist but has only 2 citers. No one tests meta-level transfer of Goodharting.
- **Closest prior:** HDA 2610.22466; 2609.17817; BreakingWeb 2609.35814; Masked Erosion 2610.38392.
- **Cost:** about $300–900.

**Recommendation:**
- Ideas 1 + 3, with Idea 2 as a secondary axis, form one coherent NeurIPS paper: "Do self-improvers get better at improving? A swap-test audit of meta-agents". It carries Dan's #2 controls (compute matching, audit sets, seeds, HDA decomposition) as *method* rather than contribution. The key risk is a fast-follow by the Auditing-the-Product authors or the RSIFlow and MetaRSI groups. Start within 2–3 weeks.
- Idea 5 is the highest-surprise alternative and the best fit with BreakingWeb.
- Idea 4 is cheap and could be a workshop paper or an appendix.

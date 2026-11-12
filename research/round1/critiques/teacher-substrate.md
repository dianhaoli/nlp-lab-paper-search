# Critique: teacher-substrate — "Same teacher, different learner: do curriculum principles transfer from weights to harnesses?"

Reviewer stance: skeptical NeurIPS area chair plus related-work search. Date: 2026-11-12.

## 0. Idea under review
- **Claim:** teacher choice and learner substrate interact strongly, and the ranking flips. Frontier-band and learning-progress (LP) teachers are best for LoRA-GRPO but among the worst for GEPA/ACE harness learners, which instead need rule coverage and contrastive pairs.
- **Design:** 5 teachers (random, frontier band, LP bandit, hint-regret, coverage ledger) x 2 learners (harness on a frozen 8B vs LoRA-GRPO on the same 8B), matched teacher compute, 5 seeds. Budget is about $500 API plus 200–400 GPU-hours.

## 1. Scoop search (what was run)
- alphaXiv discover with recency priority, 4 queries:
  - curriculum/task selection for GEPA/ACE/harness learners
  - teacher-student curricula across learner types
  - the same curriculum for prompt optimization vs GRPO
  - self-generated tasks for memory/playbook learners
- Semantic Scholar keyword search, 5 phrasings (curriculum for prompt optimization vs RL; harness-optimization curriculum; data selection for prompt optimization; and others). Results were mostly noise. No paper with a teacher x learner-type factorial turned up.
- Forward citations (S2):
  - SPADE 2608.19197: 14 citing papers, all weight-training env/self-play work (LegoEnv, Environment Duels, DAEDALUS, FrogNano, EnvHarness, ...). None crosses substrates.
  - ActiveSaddler 2610.00906, DEO 2609.34279, GRADUS 2610.27781, GEPA-Curator 2610.37651: no indexed citations yet.
- Deep reads (answer_pdf_queries): ActiveSaddler, GEPA-Curator, GRADUS, DEO, DAEDALUS, CLR-Bench.

### Closest papers and the gap each leaves

| Paper | What it does | Overlap | Gap left |
|---|---|---|---|
| **ActiveSaddler** 2610.00906 (Oct 1 2026, POSTECH/KAIST/MSR) | Automated curriculum for *harness* optimization. A non-stationary bandit over LLM-extracted **failure-pattern arms**, scored by estimated **learning progress**, with an exploration controller. Beats a random fixed order by +4.4 (GAIA2) and +7.5 (TB2). Beats fixed **easy-to-hard difficulty** curricula by 3.9–9.2 pp. Also lifts GEPA by +3.0. Count-based UCB and EMA prioritizers do 3–4 pp worse. Category- or scenario-level arms lose 3–11 pp. | Directly tests curriculum *for harness learners*. Includes difficulty-ordered and LP-style teachers. | Harness-only; no gradient learner. gpt-5.5 only, n of about 2 runs. **Its winning teacher is an LP bandit**, which contradicts the idea's predicted flip ("LP is among the worst for harness"). What matters there is the *arm representation* (failure pattern rather than scenario/category), not LP vs non-LP. |
| **GEPA-Curator** 2610.37651 (Oct 27 2026, Berkeley; Gholami/Keutzer/Zaharia) | An LLM curator selects GEPA minibatch tasks: it skips mastered, impossible and flaky tasks and keeps "solvable but not reliable" ones. It also adds **prior-success retrieval**, which gives the reflector a contrastive success/failure pair. Gains up to +20.7 over GEPA and +21.2 over ACE on WebArena/GoBrowse, ALFWorld, AppWorld and τ-bench, with 5 repeats. Table 4: a **numerical success/failure curriculum underperforms vanilla GEPA (62.5 vs 66.2)**. | Already shows (a) that pass-rate-only curricula can hurt reflective learners and (b) that contrastive pairs help them. Together these are the "mechanism" half of the claim. | No weight learner and no factorial. The "frontier" notion is qualitative (an LLM judge of informativeness). It never asks whether the same teacher would help GRPO. |
| **GRADUS** 2610.27781 (Oct 20 2026, AWS) | Coverage-ledger task synthesis (guarded operators, policy x outcome ledger) for τ²-Bench and CAR-Bench. The learner is **SFT + GRPO on Qwen3-8B/30B**. Removing the global ledger costs up to 21 pp. | This is the "coverage ledger" teacher, and **it wins for a gradient learner**. | It directly undercuts "coverage is a harness thing and frontier is a weights thing". No harness learner. |
| **DAEDALUS** 2610.08048 (Oct 6 2026) | Explorer–solver self-generated tasks, **calibrated to the solver's capability edge** (too easy → harder, too hard → easier), with a coverage Surveyor. The learner is a **memory/heuristic bank** (frozen model). It beats PREPING and matches ACE trained on real training tasks. | A frontier-calibrated teacher *works* for a non-parametric learner. | No teacher ablation across substrates. Frontier and coverage are fused. |
| **DEO** 2609.34279 (Sep 28 2026) | A frozen-LLM Metropolis walk toward a solver-frontier task distribution; the learner is GRPO on math. | Frontier teacher for weights. | Weights-only, math. |
| **SPADE** 2608.19197 (Aug 2026) | Self-play over adaptive synthetic executable environments, for weight learners. | Teacher side. | Weights-only. |
| **CLR-Bench** 2610.40758 (Oct 29 2026) | A hidden-rule curriculum benchmark for six harness learners (ACE, GEPA, DC, TF-GRPO, EvoSkill, ReasoningBank). Harness learners store **exceptions, not rules** (≤5 pp over frozen). | Harness learners struggle with rule induction, so "rule coverage" is subtle. | No teacher comparison. No weight learners. |
| GRPO-curriculum analyses: 2610.13669 ("A Step Harder, Not a Step Too Far"), 2610.19349 (curricula under GRPO for long-horizon agents), 2610.39056 ("When does closed-loop curriculum adaptation help GRPO?"), 2608.09217 (learnability prior) | Frontier/medium-difficulty selection for GRPO gives mixed results; adaptive frontiers can forget. | Even the "gradient side" premise (frontier/LP is best for GRPO) is not settled. | Weights-only. |
| Related mechanism: "LLMs learn better in-context from rules than from examples" 2609.03213 | Rules beat examples for in-context learners. | Supports the hypothesized mechanism. | No curriculum or teacher study. |

**Verdict on scooping:** no paper runs the teacher x substrate factorial or makes the cross-substrate transfer claim, so K1 is **not** triggered. However, every individual cell of the proposed table now has a 2026 paper, and the evidence that exists points *against* the specific predicted flip:
- an LP bandit wins for harness learners (ActiveSaddler)
- a frontier-calibrated generator works for memory learners (DAEDALUS)
- a coverage ledger wins for GRPO (GRADUS)

The novelty that survives is the *interaction measurement itself*, not the direction of the flip.

## 2. Mock NeurIPS review

**Summary.** The paper asks whether automatic-curriculum teachers that work for gradient learners also work for reflective harness learners (GEPA/ACE). It crosses 5 teachers with 2 learners on a frozen or LoRA-tuned 8B model and reports a teacher x substrate interaction.

**Strengths**
1. The question is clean and general. "Is task value learner-relative?" matters now that harness optimization (GEPA, ACE, Meta-Harness, AutoSaddler) competes with RL.
2. A factorial design with matched teacher compute and seeds is the right shape. A significant interaction would be a portable lesson for people building self-improvement loops.
3. It is cheap relative to typical RL papers.

**Weaknesses**
1. **The headline prediction is already contradicted by recent work.** ActiveSaddler's best harness teacher *is* an LP bandit, and DAEDALUS's frontier-calibrated generator works for a memory learner. GRADUS shows coverage wins for GRPO. Unless the teachers are carefully operationalized, the likely outcome is "coverage-style teachers win for both; frontier is mediocre for both", which is a main effect rather than an interaction. Reviewers will cite these papers.
2. **Confounded teacher definitions.** "LP bandit" over *scenarios* is not the same as LP over *failure patterns*, and ActiveSaddler shows the arm representation swings results by 3–11 pp. Any flip you find may be an artifact of arm granularity, not substrate. You need to separate *what signal ranks tasks* (pass-rate variance, LP, coverage gain) from *what unit is ranked* (task, rule, failure pattern).
3. **Matching compute across substrates is ill-defined.** Teacher compute can be matched, but the learners consume tasks completely differently. GRPO needs thousands of rollouts; GEPA saturates in hundreds (GEPA-Curator ran up to 5,000 rollouts). A flip could come from **budget regime** (harness learners saturate, so all teachers converge) rather than substrate. You need learning curves over budget, not one endpoint.
4. **Statistical power.** 5 seeds x 5 teachers x 2 learners, on a small agent benchmark (τ-bench retail ≈ 115 tasks; GEPA-Curator used 35 test tasks), gives CIs of ±4–6 pp. Interaction effects are typically smaller than main effects. Expect an underpowered "no significant interaction". With the null result, the paper reads as "workshop: a negative replication".
5. **Single benchmark, single model.** "Engineering / benchmark-specific" critique. Ranking flips found on one environment rarely generalize.
6. **No mechanism.** Even with a flip, reviewers will ask *why*. Without a measured per-task "informativeness to learner L" quantity that predicts teacher rankings, it reads as an empirical curiosity.
7. **Relevance to self-improving agents.** The teacher is external and fixed; nothing is self-referential. The link to meta-agents and Gödel-Agent-style RSI is thin.

**Likely scores:** soundness 2–3, novelty 2–3, significance 2–3. Likely decision: **borderline reject (4/10)** as specified. A clean, preregistered interaction with a mechanism and at least 2 environments could reach **weak accept (6)**.

## 3. Rubric scores

| Dim | Score | Rationale |
|---|---|---|
| N (×3) | **3.0** | No factorial exists, but ≥4 Aug–Oct 2026 papers occupy every cell: ActiveSaddler, GEPA-Curator and DAEDALUS (harness/memory side); GRADUS, DEO and SPADE (weights side). The cross-substrate question is open; the predicted direction is partly pre-empted or contradicted. |
| S (×2.5) | **3.5** | A real ranking flip would be a striking, quotable result. But priors now favor "no flip / asymmetric", and the null ("teachers transfer") is only mildly publishable. |
| R (×2) | **2.5** | Curricula for harness learners connect to Dan's lead (#3), but the design is not self-referential, not a meta-agent and not long-horizon RSI. Tangential to the mentor's Gödel Agent / world-model agenda. |
| C (×2) | **3.0** | The factorial and seeds are good. Weaknesses: one benchmark, one model, ill-defined matched compute across substrates, and an underpowered interaction test. |
| F (×1.5) | **3.0** | $500 API is fine. Multi-turn LoRA-GRPO on an 8B agent across 25 runs is real infra work for a solo undergrad, and 200–400 GPU-hours is optimistic for agentic rollouts. A procedural environment would fix this. |
| T (×1) | **4.0** | Harness evolution and curricula are hot (4 papers in Oct 2026 alone). Risk of saturation by spring 2027. |

**Total = 20 × (3·3 + 2.5·3.5 + 2·2.5 + 2·3 + 1.5·3 + 1·4) / 12 = 20 × 37.25 / 12 ≈ 62.1** (viable only with fixes).

**Kill flags**
- **K1: not triggered.** No 2026 paper makes the same claim with the same design. ActiveSaddler and GEPA-Curator are harness-only; GRADUS, DEO and SPADE are weights-only.
- **K2: partially at risk.** If the result is "no interaction" on one benchmark, it is hard to publish at the main track. Mitigate by designing it so both outcomes are informative (see fixes).
- **K3: not triggered.** Open 8B models plus cheap API.
- **K4: borderline.** "Run known teachers on two known learners" can read as combining existing things. It escapes K4 only if a mechanism or predictive quantity is contributed.

## 4. Fixes that would raise the score

1. **Reframe from "ranking flip" to "task value is learner-relative, and here is the quantity that predicts it".**
   - Define a per-task informativeness for each learner. For GRPO: reward variance or advantage norm. For harness learners: the probability that a task produces an *accepted, attributable edit*. ActiveSaddler's "fix rate" and GEPA-Curator's "prompt-sensitive" label are proxies.
   - Show that teacher rankings on each substrate are predicted by how well each teacher's selection rule correlates with that learner's informativeness.
   - This turns an empirical table into a mechanism, and both outcomes (flip or no flip) are explained. Raises S, C and N.
2. **Factorize the teachers** instead of using 5 named methods. Use a 2×2(+1) design:
   - ranking *signal*: pass-rate frontier vs learning progress vs coverage gain
   - crossed with ranking *unit*: task vs rule/failure-pattern

   This directly tests ActiveSaddler's confound and lets you attribute any interaction. Include ActiveSaddler-style failure-pattern LP as an arm.
3. **Use a procedural, rule-grounded environment with known latent rules.** Options: a τ-bench-like policy generator, or CLR-Bench-style hidden rules. Coverage is then measurable exactly, power is unlimited (fresh held-out tasks), and GRPO is cheap. Keep one real benchmark (τ²-retail or AppWorld) as an external-validity check. Raises C and F.
4. **Add a third substrate** so "harness vs weights" is not a single contrast: GEPA (prompt), ACE/DAEDALUS-style memory (playbook), LoRA-GRPO (weights), and optionally SFT. Expect a different interaction with memory learners. GEPA-Curator already shows GEPA and ACE respond differently.
5. **Report full budget curves** (performance vs learner rollouts) so the interaction is not just a saturation artifact. Test the interaction at 2–3 budget levels.
6. **Teacher-transfer test.** Tune or select the teacher on substrate A and deploy it on substrate B, and measure regret. This gives a practical, quotable number, e.g. "using a GRPO-tuned curriculum for GEPA wastes X% of the budget".
7. **Power analysis up front.** Pilot for under $50 to estimate the interaction effect size, then set seeds and tasks accordingly. Preregister the predicted direction. Given the current evidence, predict **asymmetric transfer**, not a symmetric flip.
8. **Tie to the mentor's agenda.** Make the teacher choice itself a meta-agent decision: a self-referential improver that picks its own curriculum given its substrate. Or frame it as "a Gödel-Agent-style self-modifier needs a different curriculum from an RL-tuned agent". A short "meta-teacher" arm that learns which teacher to use per substrate would raise R.

## 5. Sharpened headline claim
> "Task value is learner-relative: a task's training value is predicted by outcome variance for gradient learners but by *coverage of unresolved, attributable failure rules* for reflective harness learners. As a result, pass-rate-frontier teachers lose most of their advantage when moved from LoRA-GRPO to GEPA/ACE learners, while rule-coverage teachers transfer in both directions (asymmetric transfer). Using a GRPO-optimal curriculum for a harness learner wastes X% of its rollout budget."

Every part of this is falsifiable. It is consistent with the existing evidence (GRADUS: coverage helps weights; GEPA-Curator: a pass-rate curriculum hurts GEPA; ActiveSaddler: failure-pattern units matter more than the scheduler). A null result would still be informative: "teachers transfer once the ranking unit is matched".

## 6. Bottom line
Not scooped on the factorial question, but the predicted flip is at odds with three Oct 2026 papers. As stated, it scores about 62, which means park unless fixed. With fixes 1–3 (mechanism, factorized teachers, procedural environment) it plausibly reaches about 70–72. It remains weaker on mentor fit than the meta-agent ideas. It is best used as a **section or ablation inside Dan's lead idea #3** (self-generated curricula for harness learners) rather than as a standalone paper.

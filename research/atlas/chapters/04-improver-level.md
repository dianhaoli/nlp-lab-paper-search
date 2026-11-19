## Improver-level RSI: evolving the evolver, meta-rewards, dual loops

*Scope: systems where the thing that produces improvements (search strategy, mutation prompt, meta-skill, improver code, improver memory or weights, acceptance gate) is itself edited, credited or learned. Duan et al. 2609.11873 call this level **L5, "recursive inheritance"**. Dai et al. (2611.recursive-self-improvement-llm-agents) call it the **"improver update"** pattern. Ren et al. 2607.13104 call it **meta-level skills**, where "the operator can become part of its own operand". Chen et al. 2607.07663 call it **open-ended RSI**, which "modifies the system and the criteria or machinery of improvement itself".*

*Marks: ✓ = checked in full text in this session; for some papers this means the Nov-13 full-text pass recorded in `research/FULLTEXT_VERIFICATION.md` (FV), with local texts under `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/<id>/paper.txt`. ◐ = abstract only. † = secondary or unverified.*

*Searches run (14+):*
- Semantic Scholar: 8 queries, all date-filtered from Jun 2026.
- ICLR 2027 OpenReview: 8 queries. Abstracts came through the search endpoint because `/notes?id=` is behind a bot check.
- alphaXiv: 3 discovery queries.
- arXiv export API: abstracts for 17 IDs.

*Newly downloaded and read this pass:* EvoX, Skill∞, SIM (2611.18856), Promptbreeder, Meta-TTL (2604.00830). I also read the abstract and intro of ELITE, TRUST, MetaEvolve, Harnesses-That-Improve-Themselves and SpecRSI. (The fact-check pass also checked headline numbers for ELITE, TRUST, MetaEvolve and SpecRSI in their local full texts. Those are marked ✓ below.)

### At a glance
- **What this corner is.** It has two loops.
  - The inner loop improves a task artifact (prompt, skill, harness, program).
  - The outer loop changes the improver, either by **editing** it (Promptbreeder, STOP, Gödel Agent, Hyperagents, MetaSkill-Evolve, Skill∞, FreeEvolve, RSIFlow, VERSE, Organon, EvoX) or by **training** it (Meta-TTL, MetaEvolve, LSE, Evo², SpecRSI-GVO).
  - The central design question is the **meta-reward**: how an improver edit is credited when its value only shows up in the successors it later produces.
- **State of the art (product level).** Improver-editing methods win most in-domain comparisons when the evaluator is exact or execution-based:
  - EvoX on 196 optimization tasks: Frontier-CS median 75.5 vs 56.2 for the best baselines (OpenEvolve and GEPA) ✓.
  - Skill∞: 60.7 vs 55.9 for its own seed meta-skill on SkillsBench ✓.
  - VERSE on SWE-rebench: 42.3 vs 39.2 ✓.
  - FreeEvolve: +6.9 for meta-evolved over seed skill ✓ FV.
  - HMED: ALFWorld 54.11 vs 51.49 for MetaSkill-Evolve ✓.
- **State of the art (process level) is thin.**
  - Of 26 studies that claim a better improvement *procedure*, only **4** compare earlier and later procedures from identical starting artifacts at a matched budget (Auditing the Product 2610.27610 ◐).
  - Nested replay (Improvement by Inheritance, ZMmJDeDTXk ✓) finds successor quality δ1 = +0.003 for the selected improver edit. Self-lineage gains are almost entirely inheritance.
- **Consensus 1: realized self-editing is rare.** When systems are merely *allowed* to edit their improver, they seldom do:
  - 0% retained Meta "opinions" in SelfImprove-Bench, and 20.5–45.2% only under explicit framing ✓ FV.
  - 1–15% of generations, and 0/36 self-copies edited, in IbI ✓.
  - 0–6 Gödel Agent self-modification calls per 20 iterations (Metaⁿ App. J) ✓ FV.
  - So most of the field now **forces** meta-updates on a schedule (MetaSkill-Evolve H=2, RSIFlow every round, VERSE every round, Flex-RSI every 4 rounds).
- **Consensus 2: gating meta-edits is rare and verification is a precondition.**
  - Among the 17 improver-editable systems tabulated in FV (Nov 13), only HMED scores an improver edit separately and can reject it ✓ FV. Outside that table there are two more exceptions. Skill∞ scores each meta-skill candidate by g(m,q), and a candidate becomes eligible only after an independent review (§3.3–3.4) ✓. Meta-TTL adopts a meta-policy only if BIS>0, but its optimizer is not self-referential ✓. So gating is rare, not unique to HMED.
  - VERSE finds optimizer self-evolution fails without execution-based verification ✓. Skill∞ attributes its gain to "a stricter standard of evidence" written into the meta-skill ✓.
- **The meta-reward menu has converged on a few forms:**
  - one-step downstream utility (STOP û; Skill∞ g(m,q); FreeEvolve "fresh target agent"; Meta-TTL BIS);
  - window progress (EvoX J);
  - descendant/clade credit (HGM CMP, Looped-RSI);
  - matched-start replay (HMED, RSI-Harness, IFR; RobustSGPO uses paired replay at the harness level, not on improver edits);
  - lookahead/knockout (LRSI);
  - stability certificates (EvoLyapunov);
  - statistical error control (TRUST);
  - no reward at all (SelfSearch, ELITE).
  - No head-to-head comparison of these exists.
- **Biggest open problem.** It is not shown that any meta-reward raises **successor quality (order 1)** on held-out states rather than child quality (order 0). It is also not shown that the gain survives memory removal and domain shift. Positive-transfer claims (RSI-Harness up to 4.5× ◐, Hyperagents imp@50 0.630 ✓ FV, Meta-TTL +5–27% OOD ✓) and negative ones (FreeEvolve TB2.1 −4.9/−7.1 ✓ FV; RSIBench −8.3 pp ◐) coexist. None includes a graded-distance or memory-wiped test.

### How it developed

| Month | Paper | What changed |
|---|---|---|
| 2003 † | Gödel Machine (Schmidhuber) | Formal ideal: rewrite your own code only once the rewrite is proven useful. Every LLM paper below replaces "proof" with an empirical gate. |
| 2023-09 | Promptbreeder 2309.16797 | The first LLM system where the **mutation prompts** evolve alongside task prompts ("hyper-mutation"). Zero-order hyper-mutation produced an improvement 42% of the time it was applied ✓ (App. J.4, Table 8). |
| 2023-10 | STOP 2310.02304 | The improver program rewrites itself, scored by meta-utility û = mean downstream utility after one application. It is the canonical order-0 meta-reward. Gains appear with GPT-4 and regress with GPT-3.5/Mixtral ✓ FV. |
| 2024-08 | ADAS 2408.08435 † | A meta-agent searches agent designs, but the meta-agent itself is fixed. This is the L2 baseline that later work pushes past. |
| 2024-10 | Gödel Agent 2410.04444 (Yin et al., ACL 2025) | Task policy and update logic share one runtime-patchable program, so self-referential code edits come with no gate ✓ FV. This is the mentor's lineage. |
| 2025-04/05 | SICA 2504.15228; DGM 2505.22954 | Archive-based self-improving coding agents. In DGM the proposal step is a fixed o1 model and archive/parent selection are frozen ✓ FV. |
| 2025-10 | HGM 2510.21614 | Clade metaproductivity (CMP) is the first **descendant-credit** signal, used for expansion only ✓ FV. |
| 2026-02 | EvoX 2602.23413 | An explicit **dual loop**: solution evolution plus search-strategy evolution, triggered on stagnation, with window-progress credit J ✓. |
| 2026-03 | Hyperagents / DGM-H 2603.19461; Meta-Harness 2603.28052 | Task agent and meta-agent live in one editable program. The meta-only transfer arm scores imp@50 0.620 ✓ FV. Meta-Harness's fixed proposer becomes the object-level baseline. |
| 2026-04 | Meta-TTL 2604.00830 | Bi-level **meta-learning** of a natural-language self-improvement policy, not self-referential ✓. |
| 2026-07 | MetaSkill-Evolve 2607.05297; surveys 2607.07663, 2607.13104; MetaEvolve 2607.21971 (COLM 2026) | A two-timescale, branch-local meta-skill (5 role parameters) with forced updates every H. The surveys name "open-ended RSI" and "meta-level skills". MetaEvolve uses RL to teach self-evolution meta-skills ✓ (abstract numbers checked in full text). |
| 2026-08 | Metaⁿ 2608.24735; HSI 2608.08466 | Metaⁿ freezes the operator and recurses on its input. HSI adds a three-scope task/evolver/meta-evolver stack under a frozen outer anchor ◐. |
| 2026-09 | MetaRSI 2609.06396; Duan survey 2609.11873; SelfSearch 2609.37968; R² Flow 2609.33867; RobustSGPO 2609.09646 | MetaRSI's meta-policy revises the operator schedule. Duan splits L5 into "structural" vs "effective". SelfSearch does reward-free self-modification from episode records. |
| 2026-10 | VERSE 2610.02616; HMED 2610.07979; FreeEvolve 2610.09197; RSIFlow 2610.21791; SpecRSI 2610.16491; RSI-Opt 2610.32026; Organon 2610.33046; Skill∞ 2610.35434; Self-Improvement Problem 2610.21646; Auditing the Product 2610.27610 | The month of the boom: verification-gated optimizer self-evolution; the first **matched-start replay meta-gate** (HMED); a meta-evolved evolution skill scored on fresh targets; a meta-skill optimizing itself; a trained improver (GVO); formal limits of gates; and an evidence audit (4/26). |
| 2026-09/10 (ICLR27 subs) | RSI-Harness EOrtreQLbv; IbI ZMmJDeDTXk; IFR eIJH6HV9LZ; Looped-RSI sBH9cdajAw; MetaEvoForge 0b4wysHrzh; LRSI 71wzcahKcb; TRACE V7vjimAYGi; Evo² Ppb1YoSRol; EvoLyapunov 3t0j2Y05EH; MEvo IrNv2E8nJO; LSE RTN1lPAkIm | Meta-credit designs multiply: a matched-start outer gate, replay on frozen past states, descendant consolidation, knockout lookahead and Lyapunov gating. IbI provides the nested-replay assay and the "inheritance dominance" null. |
| 2026-11 (1–18) | Flex-RSI 2611.08946; SelfImprove-Bench 2611.02797; TRUST 2611.10787; RSI Tax 2611.13524; SIM 2611.18856; ELITE 2611.21056; Do LLM Agents Still Need to Be Told 2611.23219; Harnesses That Improve Themselves 2611.22299; RSI-Env 2611.01736; Dai et al. survey | Benchmarking and skepticism: the frozen-improver ablation, the 0% unforced meta-edit rate, false-evolution-rate control, a cheap note baseline that matches RSI, **improver-side memory** (SIM), evaluation-free integration, and agents inventing their own self-learning loop. |

### Taxonomy

The corner divides along three axes: **(A) what part of the improver changes**, **(B) how improver changes are credited (the meta-reward)**, and **(C) loop topology**. The surveys map onto these axes as follows:
- Duan's L5 sub-families map to A1/A4/A7.
- Dai's "improver update" pattern covers A1–A6.
- Ren's "meta-level skill" is A3/A4.
- Chen's closed loop with evaluator edits is A7.

**A. Object of the outer loop**

| Approach | What is edited or learned | Representative papers |
|---|---|---|
| A1 Search/selection strategy | parent selection, variation-operator mix, population management | EvoX ✓, Organon ✓ FV, Mixture of Self-Improving Branches 2609.37834 ◐, MetaRSI scheduler ✓ FV |
| A2 Mutation/proposal prompts | instructions that rewrite task prompts | Promptbreeder ✓, SePO 2606.04465 †, Hyperagents meta-prompt ✓ FV |
| A3 Meta-skill (text package) | the SKILL.md that encodes the optimizer | MetaSkill-Evolve ✓, Skill∞ ✓, FreeEvolve ✓ FV, HMED ✓, SkillEvolver stf7CbLpUY ◐ (fixed), Learning Meta-Skills for Harness Design XYhCP2sgxW ◐ |
| A4 Improver code / whole repo | the improver's source, including its own update logic | STOP ✓ FV, Gödel Agent ✓ FV, Hyperagents ✓ FV, SICA ✓ FV, SelfSearch ✓ FV, VERSE ✓ FV, RSIFlow ✓ FV, Flex-RSI ✓ FV, HSI ◐, Hyper Algorithm Design Agent lyZgSaxgoN ◐, RSI-Harness ◐ |
| A5 Improver weights (learned improver) | the policy that proposes edits, via RL or SFT | MetaEvolve 2607.21971 ✓ (headline numbers), LSE RTN1lPAkIm ◐, Evo² Ppb1YoSRol ◐, SpecRSI-GVO ✓ (headline numbers), Meta-TTL (outer loop is reflective text optimization; compared with GRPO) ✓ |
| A6 Improver memory / meta-experience | records of past edits, rationales and outcomes that condition the proposer | SIM ✓, HMED meta-experience ✓, SelfSearch records ✓ FV, IFR ◐, SelfLineage nVl2qz74V7 ◐ |
| A7 Gate / evaluator | acceptance rule, rubric, successor evaluator | RQGM 2606.26294 † (via Duan Table 7), RuHarness 2610.36826 ◐, Flex-RSI revisable 2σ gate ✓ FV, TRUST ✓ (headline numbers), EvoLyapunov ◐, MEvo Preserve-and-Extend gate ◐ |

**B. Meta-reward / credit for an improver change**

| Credit form | Definition | Papers | Known weakness |
|---|---|---|---|
| None / forced | the meta-update is applied on a schedule with no score | MetaSkill-Evolve (every H), RSIFlow (every round), VERSE (mandatory + safety/test check), Flex-RSI (every 4 rounds, 400-line cap) ✓ FV | inert or harmful edits are kept; the dose is untested |
| Contract / syntactic | the edit passes validity checks | MetaRSI (4 checks), DGM/Hyperagents IsValid ✓ FV | no quality signal |
| Agent discretion | the LLM decides whether to keep or withdraw | Gödel Agent, Organon (withdrawals in 12/21 runs), FreeEvolve ✓ FV | regressions: 14/100 Gödel Agent MGSM runs ended below the initial policy ✓ FV |
| One-step downstream utility | score of the artifacts the new improver produces | STOP û ✓ FV; Skill∞ g(m,q) plus independent review ✓; FreeEvolve fresh target ✓ FV; Meta-TTL BIS>0 ✓ | order-0 evidence; overfits the meta-train pool |
| Window progress | Δbest over W steps × log(1+s_start)/√W | EvoX ✓ | confounded with population state; no held-out test |
| Descendant / clade | pooled success of descendants | HGM CMP ✓ FV; Looped-RSI ◐ | IbI Prop 7: CMP ≠ successor quality ✓ |
| Matched-start replay | re-run incumbent vs revised improver from the same restored state | HMED (Δ>0 on 2 states) ✓; RSI-Harness (same start harness) ◐; IFR (frozen historical states + floor) ◐. Object-level analogue: RobustSGPO admits *harness* candidates by paired replay (ΔJ>ε=0.05) ✓ FV, so its gate does not apply to improver edits | very noisy at 2 states; sign disagreement 45.5% on ALFWorld (75.0% on ScienceWorld) excluding ties ✓ |
| Lookahead / knockout | toggle an edit in sampled future versions | LRSI ◐; SelfLineage ◐ | cost grows with horizon |
| Stability / statistics | Lyapunov drift condition; online FDR-style e-values | EvoLyapunov ◐; TRUST ✓ (headline numbers) | conservative; proven only under stated assumptions |
| Reward-free | no downstream score in the loop | SelfSearch ✓ FV; ELITE ✓ (headline numbers); RepRSI-style internal progress ◐ | relies on model judgement |

**C. Loop topology**
- **Single self-referential program.** The improver edits itself in place: Promptbreeder, STOP, Gödel Agent, Hyperagents, Organon.
- **Two-timescale dual loop.**
  - The slow loop updates the improver every H inner steps or on stagnation: MetaSkill-Evolve, EvoX, RSI-Harness, RSIFlow.
  - The slow loop runs offline on a meta-train pool and the result is then frozen for reuse: Skill∞, FreeEvolve, Meta-TTL, MetaEvoForge.
- **Stacked levels with a frozen anchor:** HSI (meta-evolver over evolver), Metaⁿ (fixed Ω recursing on its input), MetaRSI (fixed M_ψ revises the schedule).
- **Free-form / agent-led.** The agent is given primitives and invents the loop: FreeEvolve, Do LLM Agents Still Need to Be Told (2611.23219), RSI-Env.
- **Measurement layer (not methods):** IbI, SelfImprove-Bench, Flex-RSI, Auditing the Product, RSI Tax, Self-Improvement Problem, Harness Updating ≠ Benefit 2605.30621.

### Paper cards

**Improvement by Inheritance: Measuring RSI in Coding Agents** ([ZMmJDeDTXk](https://openreview.net/forum?id=ZMmJDeDTXk), Sep 2026, ICLR 2027 submission)
- *What it does.* It separates child quality Q⁽⁰⁾ from successor quality Q⁽¹⁾. It proves lower orders do not identify higher ones (Thm 3) and introduces **nested replay**: cross the improvers on fixed meta-states and score their successors on a common bank.
- *Setup ✓.*
  - Audits all 71 Hyperagents runs (15 configurations, 1,550 improver transcripts), plus the DGM and ADAS histories.
  - New experiments use a MiMo-V2.5 backbone, plus 5 other models in the routing panel.
  - Prospective loops: 5 seeds × 80 generations. Nested replay: 6 meta-states × 6 replicates.
- *Key results ✓.*
  - The selected gen-1 edit, M1 ("read the eval report"), raises child quality from 0.476 to 0.858 on 12 states.
  - δ1 = +0.003 and δ0 = +0.031 [−0.044, 0.126]. The self-lineage contrast of +0.293 is almost all inheritance.
  - M1 edits a copy of itself in 0/36 runs.
  - Positive control: a modifier told to diagnose the improvement procedure edits the improver 35/42 times (vs 11/42 for M0), for +0.171 [0.099, 0.240] successor quality.
  - Archive best-so-far curves fall inside a permutation null at 119/141 checkpoints.
- *Limitations.* Small meta-state bank (only large effects are resolvable), one backbone for order 1, coding only, order ≥2 unmeasured. Our full read (IBI_GAPS) adds two more: there is no equivalence margin, and Thm 3 does not cover diff-reading selectors.
- *Why it matters here.* It is the measurement standard against which every card below should be read. Most "improved improver" claims in this chapter are order-0.

**RSI-Harness: Optimizing the Harness Optimizer via RSI** ([EOrtreQLbv](https://openreview.net/forum?id=EOrtreQLbv), Sep–Oct 2026, ICLR 2027 submission; PDF bot-blocked)
- *What it does.* A dual loop. The inner loop refines the task harness with the current improver. The outer loop revises the improver, and adopts the revision only if it yields a better harness **from the same start harness** under matched tasks and budget.
- *Setup.* Not readable (abstract only).
- *Key results ◐.*
  - Improver same-start gain grows from 1.3 to 6.2 points.
  - The final improver transfers to unseen models, benchmarks and domains, reaching up to 4.5× the initial improver's gain, while the task harness transfers poorly.
  - A round-3 table lists "+7.7 avg, 69% fewer tokens" †; the source is unclear.
- *Limitations.* The matched-start gate is order-0 credit on one start state. There is no memory wipe, no code scrub and no graded domain distance. This is the strongest published claim that a meta-gate works, and the main foil for positive transfer.

**HMED: Hindsight Meta-Experience Distillation** ([2610.07979](https://arxiv.org/abs/2610.07979) / [AnN0NpAVWL](https://openreview.net/forum?id=AnN0NpAVWL), 2026-10-06, ICLR 2027 submission)
- *What it does.* It re-executes the incumbent and revised meta-skills from the **same restored discovery state**. The revision is adopted only if the replay gain Δ>0. Each comparison, adopted or not, is distilled into a "Meta-Experience" record.
- *Setup.* Qwen3-32B and DeepSeek-v4-Flash on ALFWorld, ScienceWorld and BiomniBench-DA. Replay uses 2-step continuations from 2 states ✓ FV.
- *Key results ✓ (Table 1).*
  - Qwen3-32B ALFWorld: 54.11 vs 51.49 for MetaSkill-Evolve and 49.25 for SkillEvolver.
  - DeepSeek-v4-Flash: ALFWorld 80.60; BiomniBench-DA 43.19 vs 36.80 for MetaSkill-Evolve.
  - Branch outcome and matched Δ have opposite signs in 45.5% of comparable ALFWorld cases (15/33, Table 3). That is 31% if the 15 ties are counted. On ScienceWorld the figure is 75.0% ✓.
  - In the one reported run, 2/4 meta-revisions were adopted ✓ FV.
- *Limitations.* No standard deviations. The Table 2 ablation values exactly duplicate Table 1 baseline cells: Comparison-only 49.25 equals SkillEvolver, and Outcome-only 51.49 equals MetaSkill-Evolve ✓. Δ on 2 states is very noisy. Among the systems in FV's Nov-13 gate table, it is the **only one with a separate score-based gate on improver edits** ✓ FV. Skill∞'s scored and reviewed meta-skill selection is a later second case.

**Skill∞: RSI for Agent Skill Optimization** ([2610.35434](https://arxiv.org/abs/2610.35434), 2026-10-26, preprint)
- *What it does.* A meta agent follows a meta-skill (a standard skill package) to optimize task skills, and applies **the same procedure to the meta-skill itself**. The meta-skill is refined on a pool of skill-optimization tasks and then frozen and reused.
- *Meta-reward ✓ §3.2–3.4.* g(m,q) is the held-out gain of the task skill that m produces on q, averaged over Q. This is an order-0 meta-score. An isolated reviewer rejects null, never-activated or unusable candidates. For meta-skill candidates, the reviewer also checks that core responsibilities such as the review itself remain intact. Scoring is placed "beyond the reach of self-improvement".
- *Setup ✓.*
  - Codex CLI with GPT-6 Luna. The search budget is capped at four candidates per task skill.
  - 83 SkillsBench tasks across 8 domains, plus 3 KernelBench level-3 problems.
  - Test results are avg@5. Also run with GPT-5.6 Luna and with OpenCode.
- *Key results ✓ (Table 1, Fig. 3–5, §4.6).*
  - Pass rate is 60.7, vs 55.9 without self-improvement (seed meta-skill), 56.4 human-curated, 54.5 SkillEvolver and 45.5 with no skill.
  - The self-improvement gain concentrates in Math (+24.0), Media (+22.2) and SWE (+11.4).
  - KernelBench mean speedup is 1.62, vs 1.18 for SkillEvolver and 1.02 with no skill. No seed-meta-skill arm is reported on KernelBench.
  - On a subset, pass rate rises from 47.5% at budget 1 to 82.5% at budget 8.
  - Self-improvement costs about 11 task-skill optimizations. Amortized over 83 skills, that adds <15% per skill.
  - Other harness or model (Fig. 4): 66.5 vs 61.2 for human-curated with GPT-5.6 Luna, and 56.9 vs 53.3 with OpenCode. The comparison is against human-curated skills, not against the seed meta-skill.
- *Limitations.* One meta-skill lineage, so variance over meta-refinement is unknown. The gain is not uniform: Cyber (51.4 vs 54.3), Science (57.1 vs 60.0) and Industrial (47.3 vs 52.7) are *lower* with the refined meta-skill than with the seed, while Finance is higher (26.7 vs 22.2). The authors note that adherence to long meta-skills is unenforced and that verifiable rewards are needed. There is no successor-of-successor (order-1) test.

**EvoX: Meta-Evolution for Automated Discovery** ([2602.23413](https://arxiv.org/abs/2602.23413), 2026-02, v2 2026-03-16, preprint; UC Berkeley / Stanford / Bespoke Labs, SkyDiscover code)
- *What it does ✓ (Alg. 1).*
  - The inner loop evolves solutions under strategy S.
  - Every window W (10% of budget), it computes J = Δbest·log(1+s_start)/√W.
  - On stagnation (Δ<τ), an LLM mutates a high-J strategy from the strategy database H, conditioned on a population descriptor φ. A valid strategy replaces the old one without resetting the population.
- *Setup ✓.*
  - 196 tasks: 8 math, 6 systems (ADRS), 10 ALE-Bench-Lite, 172 Frontier-CS.
  - Generators are GPT-5 and Gemini-3.0-Pro. **GPT-5 generates the search strategies in all runs** (§6.1).
  - 100 iterations and 3 runs, always starting from random-sampling search.
- *Key results ✓.*
  - Best or tied-best on 7/8 (GPT-5) and 8/8 (Gemini) math tasks (Table 2).
  - Systems tasks exceed human-best on all 6 with Gemini (Table 3).
  - Frontier-CS mean 62.6, median 75.5 vs 56.2 for the best baselines, OpenEvolve and GEPA (Fig. 3b).
  - ALE-Bench-Lite: 1958.2 vs 1914.6 for ShinkaEvolve.
  - Under GPT-5, exceeding Heilbronn 0.031 costs <$1 for EvoX (and for GEPA), vs $7.6 for Shinka and $15.4 for OpenEvolve. GEPA plateaus at 0.0323; EvoX reaches 0.0339 (Fig. 4b).
- *Limitations (ours; the paper has no limitations section).*
  - The strategy LLM is GPT-5 even in the Gemini runs (a confound).
  - No ablation that holds LLM calls fixed and replaces strategy evolution with random strategy switching.
  - Optimization only, with no held-out generalization test.
  - Organon later reports a +20.5% mean held-out gain over EvoX on ALE-Bench Lite ✓ (Organon abstract, full text).
- *Fit.* It is the cleanest pro-openness result, in the regime where openness should win: an exact evaluator and no generalization gap.

**FreeEvolve** ([2610.09197](https://arxiv.org/abs/2610.09197) / [8UyLPIcKqX](https://openreview.net/forum?id=8UyLPIcKqX), 2026-10, ICLR 2027 submission)
- *What it does.* The evolver controls the whole campaign under an editable **evolution skill**: what to test, how much evidence to collect, when to stop. The skill is meta-evolved by scoring each candidate on the fresh target agent it produces. The outer evolver is a frozen copy.
- *Setup ✓ FV.* Claude Sonnet 4.6, Haiku 4.5 and Opus 4.6–4.8 on τ³-bench, ARC-AGI-2, ARC-AGI-3 and TB2.1. The same model plays every role.
- *Key results ✓ FV.*
  - +13.6 average held-out improvement, matching or beating hand-designed evolvers.
  - Meta-evolved skills add +6.9 over the seed skill, but that is a **same-environment** result.
  - Cross-environment transfer is mixed: TB2.1 −4.9/−7.1 (Table 2).
  - Haiku as evolver gains +17.2 on the evolution set and −1.1 held-out (Table 7).
- *Limitations.* No compute matching. The tables are inconsistent (47.1 ±4.0 vs ±8.7). The "overfit" reading is ours, not the paper's ✓ FV. It is the main **negative-transfer** datum for evolved improvers.

**VERSE: Verified Self-Evolving Optimizer** ([2610.02616](https://arxiv.org/abs/2610.02616) / [9WwpxPfcji](https://openreview.net/forum?id=9WwpxPfcji), 2026-10-02, ICLR 2027 submission)
- *What it does.* The optimizer revises both the executor harness and **its own** prompts, skills, tools, hooks and notes. It can test draft edits, replay failures and perturb steps before submission. Self-written code loads only after a safety check and a test run. Self-update is mandatory.
- *Setup ✓.* Optimizers include Qwen3.8-Flash-Next (main studies), Qwen3-Coder-480B and GLM-5, with 5 executors (Qwen3.8-27B is the main one). Train, validation and test splits on SWE-rebench are disjoint, plus newer out-of-distribution tasks in 5 languages.
- *Key results.*
  - Best validation-selected harness: 42.3% (SWE-rebench held-out) and 37.7% (OOD), vs 39.2 / 29.3 ✓ abstract.
  - In the controlled study, self-evolution fails without execution verification and is best with it ✓.
  - Across five executors, optimizers built 51 self-written artifacts ✓.
- *Limitations ✓ FV.* Self-evolution *lowers* selected accuracy in 5 of 8 host cases. 49% of validation flips do not reproduce (557 changes rerun once). Transfer works SWE→TB but not TB→SWE. Memory is never wiped.

**MetaSkill-Evolve** ([2607.05297](https://arxiv.org/abs/2607.05297), 2026-07-06, preprint)
- *What it does.* Each branch carries a task skill s and a meta-skill m = (ψ, σ, α, π, ε) that parameterizes the Analyzer, Retriever, Allocator, Proposer and Evolver agents. Task skills evolve every iteration; m is **forced** to update every H iterations with no gate.
- *Setup ✓.* Gemma-4 31B on OfficeQA, SealQA and ALFWorld. No seeds.
- *Key results ✓ (Table 2, Fig. 5).*
  - OfficeQA: 55.32, vs 48.94 single-level and 31.78 with no skill.
  - SealQA: 45.26 vs 37.21.
  - ALFWorld: 94.23 vs 92.31, where the no-skill baseline is already 92.31.
  - H=2 is best on every benchmark. Widening from H=2 to H=8 costs −9.1 pt on OfficeQA (48.94→39.84).
- *Limitations ✓ FV.* The new meta-skill is stored alongside children produced by the *old* meta-skill (credit leakage). Single-Level differs from the full method in 3 ways. Run variance is high. Skill∞ and HMED both beat it.

**MetaRSI-v1 / RSI²** ([2609.06396](https://arxiv.org/abs/2609.06396), 2026-09-06, preprint)
- *What it does.* It composes three typed operators (Data-, Harness- and Model-RSI) with a two-axis optimizer that picks operator order and proposal policy. A meta-level policy revises the schedule across "terms". The target model plays every role.
- *Setup ✓ FV.* Claude Opus 5, Gemini 3.1 Pro, GPT-5.6, GLM-5.2 and Qwen3.5-35B on GPQA, AIME, TB2.1, MLE-bench, ALFWorld and GAIA. 5 seeds.
- *Key results ✓ FV.*
  - The meta-updated-improver fork gives +7.2 vs +4.9 (+2.3), from one fork with no CI.
  - The cumulative gap is +5.5 at term 5.
  - "Policy carried" to the next term: 38% vs 68%.
- *Limitations ✓ FV.* Improver edits pass only 4 syntactic contract checks. The scheduler is updated unconditionally by a fixed M_ψ. No cross-domain test. SpecRSI later reports beating it by 1.30–2.70 pp across 20 backbones ✓ (SpecRSI abstract and §5, full text).

**RSIFlow** ([2610.21791](https://arxiv.org/abs/2610.21791), 2026-10-15, v2 2026-11-02, preprint)
- *What it does.* An evolving meta-agent orchestrates adaptive improvement workflows over model, harness and artifacts. It updates its meta-procedure **every round, including from failed or rejected attempts**. Only task edits are gated (first candidate with gain > 0).
- *Setup ✓ FV.* Qwen3-4B/8B, GPT-5.6 and Claude Sonnet on HumanEval, HotpotQA and others.
- *Key results ✓ FV.*
  - Meta4 beats Meta0 by +20.56% while selecting the same harness.
  - Turning meta-evolution off costs −5.74%.
- *Limitations ✓ FV.* Single runs; the +20.56 is non-monotone across rounds; inference-time revision is a confound.

**SIM: Self-Improvement Memory for Continual RSI** ([2611.18856](https://arxiv.org/abs/2611.18856), 2026-11-13, preprint; Google Cloud AI / Google DeepMind)
- *What it does.* It gives the **harness optimizer** (not the task policy) a structured memory. Edits are grouped by root cause, each with its rationale, pre-registered predicted outcome, observed outcome and provenance. The memory carries across benchmark or policy-model changes. The optimizer's planning and search procedure stays fixed.
- *Setup ✓ §4.1.*
  - Four domains: TB2.1→TB4; text classification from a 3-dataset to an 8-dataset suite; Harvey LAB→JobBench/GDPval/APEX-Agents; EngDesign→Frontier-Eng.
  - Gemini 3.8 Flash acts as policy, proposer and selector; Opus 5.5 is the source in cross-model runs.
  - N=10 rounds, k=5 candidates per round, matched budgets.
- *Key results ✓.*
  - Full SIM beats harness-only continuation in all 6 cross-dataset and all 4 cross-model settings.
  - Ten paired coding source runs: +2.36 mean, 9/10 improved, p=0.003.
  - Cross-model workspace: 84.75 with SIM vs 73.42 without, from an Opus-evolved start of 68.41 that is *below* the seed's 73.89.
- *Limitations ✓ (stated).* Only coding has repeated runs; the other domains are n=1. Interfaces must be compatible. SIM is tested only as a whole procedure.
- *Why it is here.* It is the first controlled evidence that **improver-side state** (A6) transfers. That bears directly on Meta-Overfitting's "procedure vs memory" question.

**Meta-TTL: Meta-Learning Self-Improvement Policies** ([2604.00830](https://arxiv.org/abs/2604.00830) / [nw2tokNgb2](https://openreview.net/forum?id=nw2tokNgb2), 2026-04, v4 2026-09-28, ICLR 2027 submission)
- *What it does.* Bi-level optimization. The inner loop runs test-time learning with a candidate natural-language self-improvement policy. The outer loop does reflective meta-training across tasks and replaces the incumbent only if the **balanced improvement score (BIS)** is positive.
- *Setup ✓.* Jericho, WebArena-Lite and τ²-bench, with in-distribution and held-out-domain OOD splits. The metric is W-AUC, averaged over 5 runs on Jericho and 3 runs on WebArena-Lite (Tables 1–2).
- *Key results ✓ (§4).* +10–23% on ID and +5–27% on OOD over the strongest baseline. With a Qwen3-4B meta-agent and a frozen Gemini-3-Flash actor, the outer loop matches or exceeds GRPO on Jericho (Table 5).
- *Limitations.* Not self-referential: the meta-optimizer is fixed. OOD means held-out games or domains within the same benchmark family.

**Promptbreeder** ([2309.16797](https://arxiv.org/abs/2309.16797), 2023-09, ICML 2024 †)
- *What it does.* It evolves task prompts **and** the mutation prompts that rewrite them. The operators are zero- and first-order hyper-mutation plus Lamarckian and EDA mutation, with binary tournament fitness.
- *Setup ✓.* PaLM 2-L; arithmetic and commonsense benchmarks plus ETHOS.
- *Key results ✓.*
  - GSM8K reaches 83.9 zero-shot (Table 1).
  - Share of applications producing an improvement (App. J.4, Table 8): zero-order hyper-mutation 42%, lineage mutation 26%, first-order hyper-mutation 23%.
  - ETHOS ablation (App. L) under the under-specified "Solve the Problem" spec: 81.6 overall; 64.6 without the Lamarckian mutation; 68.7 when hyper-mutation and the random initializations are removed *jointly*.
- *Limitations.* The hyper-mutation ablation is confounded with initialization, and fitness is the order-0 task score. Duan's survey files it under L2 (strategy autonomy) with "fixed task fitness" ✓.

**Remaining papers**

| id | title | one-liner | mark |
|---|---|---|---|
| [2310.02304](https://arxiv.org/abs/2310.02304) | STOP | Improver rewrites itself on one-step meta-utility û. Self-edits happen 100% by construction. The seed argmax drops the incumbent, so rounds can degrade. Unsandboxing 0.42% (GPT-4). | ✓ FV |
| [2410.04444](https://arxiv.org/abs/2410.04444) | Gödel Agent | Monkey-patched self-update of policy and learner, with no gate. 100 MGSM runs: 4% terminate, 14% end below initial. About $15/run. | ✓ FV |
| [2603.19461](https://arxiv.org/abs/2603.19461) | Hyperagents / DGM-H | Task agent and meta-agent in one program; every valid child is archived. Transfer imp@50 0.630 vs 0.0, meta-only 0.620; compounding 0.640 vs 0.610 n.s. (n=5). | ✓ FV |
| [2510.21614](https://arxiv.org/abs/2510.21614) | Huxley-Gödel Machine | CMP clade credit drives Thompson-sampling expansion. SWE-V-60 56.7 vs DGM 53.3, single runs. | ✓ FV |
| [2608.24735](https://arxiv.org/abs/2608.24735) | Metaⁿ | Frozen operator Ω recursing on its input. d1→d2 +0.113, d2→d3 −0.006 (41% of pairs regress). About 72% of the gain comes from a context string. IbI: the selected gain of 0.309 falls to 0.014 among executable starts. | ✓ FV |
| [2609.37968](https://arxiv.org/abs/2609.37968) | SelfSearch | Reward-free self-modification from episode records. Up to +11.2 pp on TB2.1; a $4.03 search reaches 82.0% on TB2.1 (DeepSeek V4 Flash). The procedure-vs-memory table has 3/4 cells, n=1. | ✓ FV |
| [2610.33046](https://arxiv.org/abs/2610.33046) | Organon | One persistent agent revises its search, population, diagnostics and memory at runtime. 21 tasks; +20.5% mean held-out over EvoX on ALE-Lite; withdrawals in 12/21 runs. Backbone-routing confound. | ✓ FV |
| [2610.32026](https://arxiv.org/abs/2610.32026) | RSI-Opt | A meta-agent evolves an optimizer-agent policy (prompt, playbook, code library, cadence). Held-out z −0.36→+3.11 (GPT-5.5). The meta-agent itself is fixed; held-out contamination. | ✓ FV |
| [2611.08946](https://arxiv.org/abs/2611.08946) | Flex-RSI | Benchmark of 24 tasks × 18 models with a frozen-improver arm. Open-model deltas +2 to +40, every pair n=1. Improver self-edits are kept under a 400-line cap. | ✓ FV |
| [2611.02797](https://arxiv.org/abs/2611.02797) | SelfImprove-Bench | G0–G4 ladder. 0% retained Meta opinions unforced; 20.5–45.2% under HyperDSH framing. One-game, unseeded score contrasts. | ✓ FV |
| [2609.09646](https://arxiv.org/abs/2609.09646) | RobustSGPO | Permission ladder with a paired-replay gate on harness candidates (ε=0.05). Rungs α2 4.07 vs α3 4.06 tie; the schedule 1→2→3 reaches 4.34. | ✓ FV |
| [2605.30621](https://arxiv.org/abs/2605.30621) | Harness Updating Is Not Harness Benefit | Evolver identity spreads results by ≤3.1 pp; a 9B evolver ≈ Opus on average. Swapping the improver barely matters at order 0. | ✓ FV |
| [eIJH6HV9LZ](https://openreview.net/forum?id=eIJH6HV9LZ) | IFR: Learning Not to Forget How to Improve | Scores improvement-strategy updates on frozen historical states with a decline floor; names "meta-forgetting". +2.3–2.6 pp (Qwen3-32B). | ◐ |
| [sBH9cdajAw](https://openreview.net/forum?id=sBH9cdajAw) | Looped-RSI | Branching RSI; the improver is consolidated separately by descendant quality; claims transfer to unseen environments. | ◐ |
| [0b4wysHrzh](https://openreview.net/forum?id=0b4wysHrzh) | MetaEvoForge | An independent MetaAgent evolves an improvement policy from 768 improvement pairs across 62 environments; task-dependent held-out gains from the same root. | ◐ |
| [71wzcahKcb](https://openreview.net/forum?id=71wzcahKcb) | LRSI (Lookahead-Guided RSI) | Knocks out each edit in sampled future versions and keeps only edits that stay non-negative. +2.4–4.4 pp on 4 benchmarks. | ◐ |
| [V7vjimAYGi](https://openreview.net/forum?id=V7vjimAYGi) | Learning to Improve / TRACE | Argues task gains ≠ improvement-ability gains; the agent edits its diagnosis, repair and self-modification programs under regression tests. | ◐ |
| [Ppb1YoSRol](https://openreview.net/forum?id=Ppb1YoSRol) | Evo²: Evolving the Evolver via Weight-Harness Co-Evolution | One policy is both Proposer and Reflector, trained with SFT+GRPO on harness-edit outcomes. Polyglot 82.1 / SWE-V 48.3 vs 62.6 / 41.0. | ◐ |
| [RTN1lPAkIm](https://openreview.net/forum?id=RTN1lPAkIm) | LSE: Learning to Self-Evolve | RL with a per-edit reward equal to downstream improvement; a 4B model beats GPT-5- and Sonnet-4.5-driven self-evolution and GEPA on BIRD and SuperGPQA. | ◐ |
| [2607.21971](https://arxiv.org/abs/2607.21971) | MetaEvolve (COLM 2026) | RL on synthesized evolution trajectories to teach self-evolution meta-skills. +10.01 ID, +24.12 OOD (absolute, over the strongest baseline); +46.9% relative on AlgoTune (8 tasks). | ✓ (abstract/§ numbers in full text) |
| [2610.16491](https://arxiv.org/abs/2610.16491) | SpecRSI | Draft-verify search (VATS) plus outer-loop improver training (GVO) with a fixed evaluator. +1.30–2.70 pp over MetaRSI-v1 across 20 backbones. | ✓ (abstract/§ numbers in full text) |
| [3t0j2Y05EH](https://openreview.net/forum?id=3t0j2Y05EH) | EvoLyapunov | Admits improver self-edits only under a Lyapunov risk-drift condition; defines a Recursive Amplification Factor. Qwen2.5-7B models. | ◐ |
| [2611.10787](https://arxiv.org/abs/2611.10787) | TRUST | Online multiple-testing gate controlling the false-evolution rate. Accepted inferior updates fall from 45.59% (greedy) to 2.78%; mean max drawdown 4.963→0.047 pp. | ✓ (abstract/intro numbers in full text) |
| [2611.21056](https://arxiv.org/abs/2611.21056) | ELITE: Evaluation-Free RSI (Jilin Univ. / KAUST, incl. Schmidhuber) | Explore-then-integrate with zero candidate task evaluations. SWE-bench Pro subset 60.0→85.6 (DeepSeek-V4-Flash) vs DGM 86.7 and HGM 88.3; 8.8× / 3.2× less wall-clock. | ✓ (§5.2 in full text) |
| [IrNv2E8nJO](https://openreview.net/forum?id=IrNv2E8nJO) | MEvo | k evolvers under a write-authority protocol, 5 seeds on 608 tasks. No bare protocol beats the seed within 3 iterations; a Preserve-and-Extend gate does at 5. | ◐ |
| [2608.08466](https://arxiv.org/abs/2608.08466) | HSI | Task harness, evolver, and a meta-evolver that rewrites the evolver's strategy code under a frozen outer anchor. | ◐ |
| [2609.37834](https://arxiv.org/abs/2609.37834) | Mixture of Self-Improving Branches | Branches with evolving dev subsets and proposal policies revised from their own history. | ◐ |
| [2609.33867](https://arxiv.org/abs/2609.33867) | R² Flow | Flow-trained skill orchestration plus independent verification for versioned library edits; separates credit from usage frequency. | ◐ |
| [lyZgSaxgoN](https://openreview.net/forum?id=lyZgSaxgoN) | Hyper Algorithm Design Agent | A hyper agent modifies the task agent and itself to design MetaBBO optimizers. | ◐ |
| [stf7CbLpUY](https://openreview.net/forum?id=stf7CbLpUY) | SkillEvolver | A fixed meta-skill authors and refines a skill per task; the baseline that Skill∞ makes self-improving. | ◐ |
| [2611.23219](https://arxiv.org/abs/2611.23219) / [G2nWulnSp3](https://openreview.net/forum?id=G2nWulnSp3) | Do LLM Agents Still Need to Be Told How to Self-Improve? | Given only primitives, capable frozen models invent self-regulated-learning loops in gameplay; gains plateau within 20 updates. | ◐ |
| [2610.21646](https://arxiv.org/abs/2610.21646) | The Self-Improvement Problem | A computable gate cannot decide semantic improvement in general; formal limits for certifiers and searchers. | ◐ |
| [2610.27610](https://arxiv.org/abs/2610.27610) | Auditing the Product, Not the Recursion | Codes 114 studies. Only 22% audit ≥2 reruns; 4/26 procedure claims use matched-start, matched-budget comparisons. | ◐ (PDF abstract) |
| [2611.13524](https://arxiv.org/abs/2611.13524) | The RSI Tax | A bounded-note baseline matches Meta-Harness, EvoSkill and RRSI at 65.6–71.0% fewer tokens; verification is 74.8–93.6% of RSI cost. | ◐ |
| [2611.22299](https://arxiv.org/abs/2611.22299) | Harnesses That Improve Themselves (Google) | Unifying framework of 4 editable components × a 4-stage loop. Gated modular evolution beats full rewrites on SWE-bench Lite. | ◐ |
| [2611.01736](https://arxiv.org/abs/2611.01736) | RSI-Env | Persistent environment where each accepted model-harness version runs the next round (Self) vs fixed external improvers, across 3 goal-specificity settings. | ◐ |
| [2610.36826](https://arxiv.org/abs/2610.36826) | RuHarness | Co-evolves the rubric (critic) with the harness. | ◐ |
| [XYhCP2sgxW](https://openreview.net/forum?id=XYhCP2sgxW) | Learning Meta-Skills for Harness Design (Test-Time AI4AI) | A Builder learns meta-skills from Target feedback: +8.95 pp over no skill and +12.02 over handing the bank to the Target directly. | ◐ |
| [x4AF6x9znL](https://openreview.net/forum?id=x4AF6x9znL) | Replay-to-Online Transport Protocol | Registration protocol for replay gain ΔR vs live gain ΔL; shows replay winners need not win live. | ◐ |
| [zxN7uhwRY0](https://openreview.net/forum?id=zxN7uhwRY0) | RepRSI | Representation-progress reward for a curriculum teacher beats behavioural rewards at matched compute. | ◐ |
| [2609.11873](https://arxiv.org/abs/2609.11873) | The Last AI Built by Humans (survey) | L0–L5 autonomy ladder. L5 is "recursive inheritance", split into structural vs effective; Table 7 lists L5 loops and their external controls. | ✓ |
| [2607.07663](https://arxiv.org/abs/2607.07663) | RSI: From Bounded Self-Refinement to Autonomous Research Loops (survey) | Two axes, substrate × loop closure; "nearly all" of 1,250 papers are bounded, human-on-the-loop. | ✓ |
| [2607.13104](https://arxiv.org/abs/2607.13104) | Self-Improvements in Modern Agentic Systems (survey) | (θ, Σ) formalism; meta-level skills make the operator part of its operand. | ✓ |
| alphaXiv 2611.recursive-self-improvement-llm-agents | When, Where, and How Experience Becomes Structure (survey) | Three persistence patterns; "reuse in later tasks alone does not establish that recursive dependency"; SePO and MemSkill as improver-update exemplars. | ✓ |

### Consensus, debates & contradictions
- **Consensus: L4 permission ≠ L5 behaviour.**
  - Four results say the same thing: Duan's structural vs effective L5, IbI's inheritance dominance, SelfImprove-Bench's 0% and Auditing the Product's 4/26. Most "improved improver" evidence is order-0.
  - Duan's own L5 table lists only STOP, Gödel Agent, DGM, HyperAgents, RQGM and a few research harnesses. It qualifies every L5 claim ("L5 claims are qualified in the text") ✓.
- **Consensus: verification gates the value of self-evolution.** VERSE is the cleanest case ✓. Skill∞'s reviewer and its "rollout must show progress on the targeted failure" rule ✓, TRUST ✓ and Flex-RSI's 2σ gate ✓ FV point the same way.
- **Debate 1: force or gate?** Two camps:
  - *Forced schedules:* MetaSkill-Evolve, RSIFlow, VERSE, Flex-RSI. MetaSkill's H=2 is best, meaning more frequent forcing helped ✓.
  - *Gated:* HMED, Skill∞ (scored and reviewed meta-skill candidates), RSI-Harness, IFR, LRSI, EvoLyapunov.
  - No paper compares the two at matched compute. IbI's positive control (forced framing, +0.171) is one prompt on 7 states.
- **Debate 2: does the evolved improver transfer?**
  - Positive: RSI-Harness ◐, Hyperagents 0.630 ✓ FV, Meta-TTL OOD ✓, MetaEvoForge ◐, SIM cross-model ✓, Skill∞ cross-harness/model (vs human-curated skills, not vs the seed meta-skill) ✓.
  - Negative or mixed: FreeEvolve cross-environment ✓ FV, VERSE one-directional ✓ FV, RSIBench ◐, Harness-Updating ≠ Benefit ✓ FV.
  - The positive results are mostly near-domain, memory-carrying, or within one benchmark family.
- **Debate 3: is the task signal needed in the loop?**
  - SelfSearch ✓ FV and ELITE ✓ are reward-free and come close to DGM and HGM at a fraction of the cost. ELITE reaches 85.6 vs 86.7/88.3.
  - The RSI Tax finds verification is 74.8–93.6% of RSI cost and that a note baseline matches RSI ◐.
  - Against this, VERSE and TRUST argue that more, and more rigorous, evaluation is what makes self-evolution work. **This is a direct contradiction and it is unresolved.** The likely resolution is the domain: coding, where task skill ≈ improver skill, vs open-ended tasks.
- **Debate 4: edit the improver or enrich its input?**
  - Three results suggest that the improver's *information* matters more than its code: Metaⁿ (frozen Ω, about 72% of the gain from a context string ✓ FV), IbI's "evidence routing" mechanism ✓ and SIM (improver memory with a fixed procedure ✓).
  - Hyperagents and Skill∞ claim that procedure edits matter. Skill∞'s case study says self-improvement "changes how the meta agent acts" ✓, but it has no information-matched control.
- **Contradictions inside papers.**
  - HMED's ablation table duplicates baseline cells ✓.
  - FreeEvolve reports ±4.0 vs ±8.7 for the same cell ✓ FV.
  - EvoX uses GPT-5 as the strategy generator in its "Gemini" runs ✓.
  - Skill∞'s refined meta-skill is worse than its seed on Cyber, Science and Industrial ✓.

### Open problems
1. **No meta-reward bake-off on order-1 outcomes.** Compare child score, matched-start Δ0 (RSI-Harness/HMED), descendant credit (HGM/Looped-RSI), replay-with-floor (IFR), lookahead (LRSI), Lyapunov (EvoLyapunov) and FDR (TRUST). Use one testbed, matched dollars, and nested-replay Q⁽¹⁾ on held-out meta-states disjoint from the selection pool.
2. **The proposed → retained → effective funnel for improver edits is unmeasured** under each real gate type (none / contract / argmax / replay) ✓ FV. Every paper reports at most one stage.
3. **Forcing dose at matched compute.** MetaSkill-Evolve's H sweep holds the meta-update count fixed at 3, so it changes total iterations (6/12/24) and never tests q at fixed cost ✓. What is needed: q ∈ {0, …, 1} × gate on/off, with a placebo (null-AST) forced-edit arm.
4. **Order ≥2 is unmeasured everywhere.** None of MetaRSI's scheduler, HSI's meta-evolver, Metaⁿ's depth or Skill∞'s meta-skill-of-meta-skill is evaluated by successor-of-successor replay. If level-1 edits occur at rate p, level-2 effects need forcing to be visible at all.
5. **Procedure vs memory decomposition of transfer.** SIM shows that memory transfers with the procedure fixed. FreeEvolve and Skill∞ transfer the procedure, with memory dropped or carried implicitly. Nobody runs the 2×2 (evolved procedure × evolved memory) across graded domain distance. SelfSearch Table 4 has 3/4 cells at n=1 ✓ FV.
6. **Is the strategy edit causal, or just a change in the information given to the generator?** Needed: an information-matched control (the same φ/H context delivered to a fixed strategy) for EvoX, Skill∞ and Hyperagents.
7. **Meta-reward Goodhart.** If improvers are selected on order-0 signals, do they converge to routing tricks ("read the eval report", IbI) rather than better diagnosis? This needs an edit-content codebook with κ.
8. **Noise floor of meta-gates.** HMED uses 2 replay states, VERSE has 49% non-reproducible flips, and TRUST reports 45.59% inferior acceptances under a greedy gate. A power analysis is needed: how many replay states per meta-decision does a meta-gate need to beat chance?
9. **Non-stationarity as the reason for improver editing.** EvoX shows that strategy value changes *within* a run ✓. SIM and RSI-Env show continuation across settings. No study manipulates drift of the *improvement regime* (evaluator granularity, flake rate, budget) to test when L5 beats L4.
10. **Learned (weight-level) improvers vs edited (text/code) improvers at matched compute.** LSE, MetaEvolve, Evo² and SpecRSI-GVO have never been compared with Skill∞/HMED-style text meta-evolution on the same tasks.
11. **Evaluator-level recursion with anchors.** RQGM, RuHarness and Flex-RSI's revisable gate open up A7. The Self-Improvement Problem says gates cannot be complete. No empirical study measures how far a self-revised gate drifts from an external anchor.

### Where it is heading (to mid-2027)
- **ICLR 2027 rebuttals and decisions (late Jan 2027).** Expect "fixed-improver", "forced-edit" and "matched-start" arms to be added to RSI-Harness, IFR, VERSE, IbI, Looped-RSI and MetaEvoForge. Expect the 0%-unforced result to replicate, and forced or gated edits to raise retention with mixed held-out effect. High likelihood.
- **Meta-gates become standard.** A matched-start replay or statistical gate (HMED, RSI-Harness, TRUST, EvoLyapunov) will become the expected baseline, as keep-if-better was in 2025. Reviewers will ask papers that use ungated forced meta-updates for gate ablations.
- **Improver-side memory and meta-experience** (SIM, HMED, IFR) will merge with procedure editing. The "continual RSI across model releases" framing (SIM, from Google) will spread, because model churn is a real and cheap source of drift.
- **Learned improvers.** RL on improvement trajectories (LSE, MetaEvolve, Evo², GVO) will be the main competitor to text meta-evolution. Expect claims that small (4–8B) trained improvers beat frontier-driven meta-harnesses on narrow domains.
- **Skepticism.** Follow-ups to the RSI Tax, Auditing the Product and IbI will push claim-indexed reporting: matched start, matched budget, ≥2 reruns. NeurIPS 2027 reviewers will likely require order-1 evidence for any "improves the improver" claim.
- **Agent-led loops.** FreeEvolve, Organon, Do LLM Agents Still Need to Be Told and RSI-Env suggest that frontier models given primitives will invent their own loop. The research question then shifts from "design the meta-loop" to "measure and constrain what the agent built".

### Connection to our ideas
(Numbering follows `/home/user/nlp-lab-paper-search/research/ONE_PAGERS.md`.)
- **#2 Forced-Quota Dose** and **#3 Gate, Not Willingness**, merged as "Propose, Propagate, Pay Off". This is the corner's central gap (open problems 2–3).
  - Ready-made arms: no gate (DGM/Hyperagents), contract gate (MetaRSI), task argmax (SICA), replay gate (HMED), forced q=1 (RSIFlow/VERSE), forced 1/H (MetaSkill-Evolve).
  - New threats to cite and beat: TRUST (statistical gate), EvoLyapunov (gated self-edits), MEvo (a Preserve-and-Extend gate is what made gains appear), Skill∞ (score plus reviewer gate on meta-skill edits).
  - None of these measures the funnel. Our delta stands, but the gate arms should include a TRUST-style statistical gate and a Skill∞-style scored and reviewed meta-gate.
- **#5 Order-0 Rewards Breed Routers.** The meta-reward table (Taxonomy B) is the arm list.
  - New candidate arms: Skill∞'s g(m,q) (gain on skill-optimization tasks, a clean order-0 meta-score), FreeEvolve's fresh-target score, EvoX's window J, LRSI knockout, TRUST.
  - Scoop risk is rising. IFR and HMED own replay credit, and the IbI authors list "select on Q⁽¹⁾" as next work. Pre-register and preprint early (the late-Feb plan holds).
- **#6 Meta-Overfitting.** Its evidence base has shifted:
  - SIM ✓ shows that improver *memory* transfers across models and datasets with the procedure fixed (+2.36, p=0.003).
  - FreeEvolve ✓ FV shows that procedure-only transfer can be negative across environments.
  - Together these make the **procedure × memory 2×2 across graded distance** the defensible core (open problem 5). Cite SIM as the memory-only cell.
- **#4 Order-2 Audit.** Auditing the Product (4/26) supplies the base rate. The audit targets are MetaRSI (+2.3 from one fork), RSIFlow (+20.56 from one run), Skill∞ (one meta-skill lineage) and Metaⁿ. HSI and SpecRSI are new order-2-style claims.
- **#7 Varying Goals / #9 MetaShift.** EvoX's within-run strategy non-stationarity ✓ and SIM's cross-model continuation ✓ are the closest priors. Neither manipulates *improvement-regime* drift, so the claim that L5 pays off only when the rules of improving change is still open (open problem 9).
- **#8 Openness Ladder.** Taxonomy A is a ready list of rungs: prompt → meta-skill → improver code → improver memory → gate. RobustSGPO's schedule result (4.34 vs a 4.06–4.07 tie) and HSI's frozen anchor are the nearest designs.
- **#1 Taught, Not Selected.** This idea touches the corner only through improver inputs. Metaⁿ, IbI and SIM all suggest that what the improver *reads* drives gains, which strengthens the "taught" channel at the meta level.
- **Mentor fit.** Gödel Agent (Yin et al.) is the no-gate, LLM-discretion endpoint in every table here. A Gödel-Agent-based funnel/gate study with nested-replay scoring is the most direct contribution that continues the lineage.

*Local texts read this pass (absolute paths):*
- `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/2602.23413/paper.txt`
- `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/2610.35434/paper.txt`
- `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/2611.18856/paper.txt`
- `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/2309.16797/paper.txt`
- `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/2604.00830/paper.txt`
- `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/2611.21056/paper.txt`
- `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/2611.10787/paper.txt`
- `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/2607.21971/paper.txt`
- `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/2611.22299/paper.txt`
- `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/2610.16491/paper.txt`
- `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/2611.23219/paper.txt`
- Fact-check pass also re-opened: `.../papers/2610.07979/paper.txt`, `.../papers/2610.02616/paper.txt`, `.../papers/2607.05297/paper.txt`, `.../papers/2610.33046/paper.txt`, `.../papers/2609.09646/paper.txt`, `.../papers/2610.09197/paper.txt` (same scratchpad prefix).

Prior full-text verifications reused from `/home/user/nlp-lab-paper-search/research/FULLTEXT_VERIFICATION.md` and `/home/user/nlp-lab-paper-search/research/papers/improvement-by-inheritance-notes.md`.

#### Fact-check log

| Claim | Verdict | Source location |
|---|---|---|
| Skill∞ 60.7 vs 55.9 (seed), 56.4 human, 54.5 SkillEvolver, 45.5 no skill | confirmed | 2610.35434 Table 1 |
| Skill∞ gains Math +24.0, Media +22.2, SWE +11.4 | confirmed | Table 1 (92.0−68.0, 84.4−62.2, 65.7−54.3); §4.2 text |
| Skill∞ "Science, Finance and Cyber lower" | **corrected**: Cyber 51.4<54.3, Science 57.1<60.0, Industrial 47.3<52.7 are lower; Finance is higher (26.7>22.2) | Table 1 |
| Skill∞ KernelBench 1.62 vs 1.18 (implied seed) | **corrected**: 1.18 is SkillEvolver; 1.02 is no skill; no seed-meta-skill KernelBench arm | §4.2, Fig. 3 |
| Skill∞ budget 4 candidates; 47.5→82.5 at budget 1→8; ~11 optimizations, <15% | confirmed | §3.4 (l.348 cap at four), §4.6, Fig. 5 |
| Skill∞ cross-harness transfer | clarified: 66.5/56.9 vs human-curated 61.2/53.3, not vs seed | §4.4, Fig. 4 |
| "Only HMED scores and can reject an improver edit" | **corrected (scoped)**: true for FV's 17-system table; Skill∞ also scores (g) and reviews meta-skill candidates; Meta-TTL gates on BIS>0 (non-self-referential) | FV §"Who gates self-edits"; 2610.35434 §3.3–3.4; 2604.00830 §3 Eq. 7 |
| RobustSGPO listed as meta-level matched-start replay | **corrected**: its paired-replay gate (ε=0.05) admits harness candidates, not improver edits | 2609.09646 §6.1; FV |
| EvoX 196 tasks (8/6/10/172); W=10%; GPT-5 strategy generator in all runs; 100 iters; 3 runs | confirmed | 2602.23413 §6.1 (l.475–496) |
| EvoX Frontier-CS mean 62.6 / median 75.5 vs 56.2; ALE 1958.2 vs 1914.6; Heilbronn <$1 vs $7.6/$15.4; GEPA 0.0323, EvoX 0.0339 | confirmed (56.2 is OpenEvolve and GEPA median) | Fig. 3b, §6.3, Fig. 4b, §6.4 |
| EvoX affiliation "UC Berkeley Sky" | **corrected**: UC Berkeley, Stanford, Bespoke Labs (SkyDiscover code repo) | 2602.23413 title page |
| Organon +20.5% mean held-out over EvoX on ALE-Lite; 21 tasks | confirmed | 2610.33046 abstract |
| HMED Table 1 numbers (54.11/51.49/49.25; 80.60; 43.19 vs 36.80) | confirmed | 2610.07979 Table 1 |
| HMED 45.5% disagreement (31% with ties) | confirmed (15/33; 15/48); ScienceWorld 75.0% added | Table 3, §4.2 |
| HMED ablation duplicates baseline cells | confirmed (49.25, 51.49) | Table 2 vs Table 1 |
| SIM +2.36, 9/10, p=0.003; 84.75 vs 73.42 from 68.41 (< seed 73.89); N=10, k=5; four domain transitions; all 6 / all 4 settings | confirmed | 2611.18856 abstract, §4.1, §4.2, Fig. 4 |
| SIM affiliation Google Cloud AI / GDM | confirmed | title page |
| Meta-TTL +10–23% ID, +5–27% OOD; BIS; outer loop ≥ GRPO | confirmed; GRPO setup detail added (Qwen3-4B meta-agent, frozen Gemini-3-Flash actor) | 2604.00830 §1, §3, Table 5 |
| Meta-TTL "W-AUC over 5 runs" | **corrected**: 5 runs Jericho, 3 runs WebArena-Lite | Table 1 / Table 2 captions |
| Promptbreeder J.4: 42% / 26% / 23% | confirmed (share of applications yielding improvement) | 2309.16797 App. J.4 Table 8, §5 |
| Promptbreeder GSM8K 83.9; ETHOS 81.6 / 64.6 / 68.7 | confirmed | Table 1; App. L |
| VERSE 42.3/37.7 vs 39.2/29.3; 51 artifacts; 49% non-reproduced | confirmed; optimizer list corrected to include Qwen3.8-Flash-Next (main optimizer) | 2610.02616 abstract, §2 Fig. 3, Table (l.569), App. (l.1473) |
| MetaSkill-Evolve 55.32/48.94/31.78; 45.26/37.21; 94.23/92.31; H=2 best; −9.1 pt OfficeQA | confirmed (−9.1 = H2→H8, 48.94→39.84) | 2607.05297 Table 2, Fig. 5 |
| IbI 0.476→0.858; δ1 +0.003; δ0 +0.031; 0/36; 35/42, +0.171; 119/141; 71 runs/1,550 transcripts | confirmed (prior full read) | improvement-by-inheritance-notes.md; IBI_GAPS |
| Hyperagents imp@50 0.630, meta-only 0.620, compounding 0.640 vs 0.610 n.s. | confirmed (prior FV) | FV §Hyperagents |
| TRUST 45.59%→2.78% | confirmed (upgraded from ◐) | 2611.10787 abstract, §1 |
| ELITE 60.0→85.6 vs 86.7/88.3; 8.8×/3.2× | confirmed (upgraded); benchmark is a SWE-bench Pro subset; affiliations Jilin + KAUST | 2611.21056 abstract, §5.2, title page |
| MetaEvolve +10.01 / +24.12 / +46.9% AlgoTune; COLM 2026 | confirmed (upgraded) | 2607.21971 header, abstract |
| SpecRSI +1.30–2.70 pp over MetaRSI-v1 across 20 backbones | confirmed (upgraded) | 2610.16491 abstract, §5 (l.763) |
| Promptbreeder at ICML 2024 | unverifiable here (not stated in arXiv text); marked † | — |
| RSI-Harness 1.3→6.2, up to 4.5× | unverifiable (PDF bot-blocked; abstract only) | OpenReview abstract |
| ID→title resolution: 2602.23413 = EvoX; 2610.35434 = Skill∞; 2611.18856 = Self-Improvement Memory…Continual RSI (also 2610.07979 HMED, 2610.02616 VERSE, 2607.05297 MetaSkill-Evolve, 2610.21791 RSIFlow, 2611.08946 Flex-RSI, 2609.37968 SelfSearch, 2609.09646 RobustSGPO) | confirmed | first lines of each local paper.txt |
| arXiv dates (MetaSkill 6 Jul; HMED 6 Oct; VERSE 2 Oct; Skill∞ 26 Oct; SIM 13 Nov; Meta-TTL v4 28 Sep; EvoX v2 16 Mar) | confirmed | arXiv stamps in local texts |

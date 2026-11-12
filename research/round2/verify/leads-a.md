# Round 2 — Verification of unchecked leads (part A)

*2026-11-12. All eight leads were found and read: full text or the alphaXiv report via `get_paper_content`, plus targeted `answer_pdf_queries`. IDs and dates come from Semantic Scholar batch lookups. The Semantic Scholar forward-citation lists turned up **two unseen papers that matter more than any of the eight leads**: SelfSearch 2609.37968 and Feedback Dynamics 2608.02636. See §2.*

Top round-1 ideas checked against: **(1)** merged *Effect / Cause / Decay* (procedure-vs-memory transplant, meta-plasticity, swap curve); **(2)** evolve-in-dream; **(3)** learning-to-goodhart; **(4)** curriculum-goodhart.

---

## 0. TL;DR

| # | Lead | ID / date / venue | Overlap with top ideas | Severity |
|---|---|---|---|---|
| L1 | Truly Self-Improving Agents Require Intrinsic Metacognitive Learning (Liu & van der Schaar) | 2506.05109 · 2025-06-05 · ICML 2025 (position) | Motivation only. It lists "evaluating intrinsic metacognition" as an open question. | low (cite as framing) |
| L2 | Recursive Self-Evolving Agents via Held-Out Selection (RSEA; Nguyen, Nguyen, Vuong, Monash Malaysia) | 2606.28374 · 2026-06-17 · arXiv | Gate-as-active-ingredient claim. Removing the gate gives a 33-pt overfit, which is a Goodhart data point. | low–medium (#3/#4: baseline/citation) |
| L3 | MetaSkill-Evolve (Wang, Yan, …, Tresp, Ma; LMU) | 2607.05297 · 2026-07-06 · arXiv | Already known in round 1. Its "freeze slow loop" ablation removes procedure *and* meta-state together. | medium (#1, known) |
| L4 | Group-Evolving Agents (GEA; Weng, …, Xin Eric Wang; UCSB) | 2602.04837 · 2026-02-04 · arXiv | Shared experience across a group. No improver/memory separation and no seeds. | low–medium (#1) |
| L5 | From Solo to Social Learning: Recursive Social Improvement (Jha, Kleiman-Weiner, Jaques; UW) | 2609.38516 · 2026-09-29 (v2 Oct 1) · arXiv | Token-matched social vs solo. Lineage concentration. | low (source of a non-obvious idea, §3) |
| L6 | Evolving Agents in the Dark: Retrospective Harness Optimization via Self-Preference (RHO; Pan et al., MSRA/CityU) | 2606.05922 · 2026-06-04 · EMNLP 2026 | Label-free, self-preference-gated harness optimization, single round. | medium (#3 learning-to-goodhart; low for #2) |
| L7 | LLM Agents Are Not Always Faithful Self-Evolvers (Zhao, …, Che; HIT/SMU) | 2601.22436 · 2026-01-30 · arXiv; the PDF header says ICML 2026 (PMLR 306), unverified on the proceedings site | Provides the causal-intervention battery (empty/shuffle/irrelevant/filler) that #1's placebo-memory arm needs. Object level only. | medium (#1: must cite; method template) |
| L8 | Rethinking Experience Utilization in Self-Evolving LM Agents (ExpWeaver; Zhao et al., HIT/Huawei) | 2605.07164 · 2026-05-08 · arXiv | Runtime *when*-to-use experience. Object level. | low |
| **N1** | **SelfSearch: Reward-Free Search for Self-Improving Agents** (Yang, Kong, Jo; SNU) | **2609.37968** · 2026-09-29 · arXiv | **Runs a crude 2-cell version of the Cause arm:** "w/o episode records" vs "fixed improver (B0 reads records)" vs full. Single run, n=120, and the gaps are only 1.7–2.9 pp. | **high for #1 (Cause arm)** |
| **N2** | **Rethinking Self-Evolving Agent Skills: Feedback Dynamics over Multiple Rounds** (Liu, …, Song; HKUST) | **2608.02636** · 2026-07-31 · arXiv | 10-round dynamics. Only 55/388 candidates are new bests. Includes parallel-sampling and sequential-refinement controls. | medium (#1 Effect arm baseline; #4) |

**Net effect on the round-1 ranking:** no kill. The merged paper (#1) needs to absorb SelfSearch, cite Faithful Self-Evolvers as the method source, and sharpen its claim from "nobody separates procedure from memory" to "nobody does it **powered, same-start, crossed, and across a domain shift**". Learning-to-goodhart gains a new and timely target: label-free, self-preference-gated loops (RHO, SelfSearch, S3Gym). That supports a sharper variant (§3, idea A).

---

## 1. Lead-by-lead

### L1 — Truly Self-Improving Agents Require Intrinsic Metacognitive Learning
- **ID** 2506.05109 (S2 CorpusId 279244233). Tennison Liu and Mihaela van der Schaar (Cambridge DAMTP). ICML 2025 position track.
- **Design.** Position paper with no experiments. It proposes a three-part framework of metacognitive knowledge (self-assessment), metacognitive planning (what and how to learn) and metacognitive evaluation (reflecting on learning to improve future learning). It separates *intrinsic* metacognition (owned by the agent) from *extrinsic* (a fixed, human-designed meta-loop). It audits STaR (extrinsic), Voyager (partly intrinsic, but its learning strategy is fixed) and Generative Agents (reflection).
- **Claims.** Fixed meta-processes fail under (a) a domain or task distribution shift and (b) a capability–mechanism mismatch, such as the generation–verification gap. Open problems it lists: shared human–agent metacognition, fine-tuning metacognition, **evaluating intrinsic metacognition** (non-stationarity, transparency), and scalable oversight.
- **Limitations.** Purely conceptual, with no measurement protocol.
- **Overlap.**
  - #1: this is *framing ammunition*, not a scoop. Its claim (a) is exactly the Decay hypothesis stated as a conjecture: a learned improver should transfer across domains, and a fixed one won't. #1 can be pitched as "the first measurement of whether evolved (intrinsic) improvement procedures survive the domain shift that Liu & van der Schaar say extrinsic ones fail on". Its "evaluating intrinsic metacognition" open question is a citable gap.
  - #2, #3, #4: none. Its oversight and reward-hacking remarks are generic.
- **Severity: low.** Cite it in the intro.

### L2 — Recursive Self-Evolving Agents via Held-Out Selection (RSEA)
- **ID** 2606.28374, 2026-06-17. Monash University Malaysia.
- **Design.** A frozen LLM evolves a three-layer natural-language state (strategy, skills, playbook). Each generation rolls out on an evolve set, has the same model rewrite all three layers, and **commits a candidate only if it does not regress on a disjoint validation split**. That is lateral accept plus a strict best-update, so it falls back to ReAct when nothing helps. Six re-implemented baselines (ReAct, Reflexion, GEPA, AWM, ACE, Dynamic Cheatsheet) share one backbone: Qwen2.5-7B on ALFWorld and Qwen3-30B-A3B on GAIA, τ-bench and WebShop. Five seeds on ALFWorld, McNemar tests.
- **Findings.**
  - No artifact wins everywhere.
  - RSEA beats ReAct on ALFWorld: 69.3 vs 64.6 (p=0.015), and 79.4 with retry.
  - It ties ReAct on the tool-use benchmarks. AWM gives the only consistent lift there.
  - Dynamic Cheatsheet, which has no gate, collapses on WebShop: 0.14 vs 0.43. The authors call this "context distraction".
  - **Removing held-out selection gives 100% in-sample and a 33-point drop on test.**
  - Conclusion: "reliability comes from the selection gate, not the artifact."
- **Limitations.**
  - The "recursive" label is loose. The rewrite prompt (the improver) is fixed, so this is not self-referential.
  - Small held-out sets on GAIA (30) and τ-bench (60).
  - The related-work section is padded with unrelated citations, which is a quality signal.
- **Overlap.**
  - #3/#4 Goodhart: the gate-removal ablation is a one-point Goodhart observation (evolve-set fit vs test). It has no dose-response, no held-out family and no improver learning. Use it as motivation.
  - #1: it reinforces that **the gate, not the improver, may carry the gains**. The Effect arm must hold the gate fixed across generation-k and generation-0 improvers.
- **Severity: low–medium.** A baseline and citation, not a scoop.

### L3 — MetaSkill-Evolve
- **ID** 2607.05297, 2026-07-06. LMU Munich (Tresp, Ma).
- **Design.** Each branch carries a task skill s and a five-file meta-skill m = (ψ diagnosis, σ retrieval/sharing, α allocation, π proposal, ε execution). The fast loop evolves s. Every H iterations the slow loop runs the *same* pipeline on a synthetic "meta-failure trace" built from meta-productivity P̂(m|s), which rewrites m. History is stored in a SQLite DAG. Frontier selection uses U + P̂ (meta-productivity) + novelty. Gemma-4 31B is frozen throughout.
- **Findings (held-out test).**

  | Benchmark | Single-level | MetaSkill-Evolve | No-skill backbone |
  |---|---|---|---|
  | OfficeQA | 48.9 | 55.3 | 31.8 |
  | SealQA | 37.2 | 45.3 | 29.2 |
  | ALFWorld | 92.3 | 94.2 | — |

  - Freezing the slow loop returns exactly the single-level numbers.
  - Component ablations: α matters most on OfficeQA (−19.7 pt) and π on SealQA and ALFWorld. Removing cross-branch retrieval σx alone wipes out the whole ALFWorld gain.
  - Smaller H is better.
- **Limitations.**
  - It does not say how many seeds it ran (none reported in the report).
  - ALFWorld runs near ceiling.
  - The pipeline wiring is fixed.
  - The "no meta-update" ablation removes procedure evolution *and* the meta-level state (σ reads the evolution graph) at the same time, so it confounds procedure and memory.
  - The σx result suggests the gain is **memory (cross-branch retrieval), not procedure**. That is preliminary support for the "memory wins" branch of #1.
- **Overlap.** #1: already covered in round 1. It is a testbed family and a motivating confound. The σx finding is worth quoting.
- **Severity: medium (known).** Forward citations include Skill∞ 2610.35434, HMED 2610.07979, RSIFlow 2610.21791, VERSE 2610.02616, MetaRSI 2609.06396 and Agent Plasticity 2610.08902, all already tracked.

### L4 — Group-Evolving Agents (GEA)
- **ID** 2602.04837, 2026-02-04. UCSB (Xin Eric Wang).
- **Design.** A DGM-style open-ended coding-agent evolution, except the *group* is the unit of evolution.
  - Parents are chosen by performance × √novelty, where novelty is cosine distance between task-success vectors.
  - Every parent's traces (patches, task logs, outcomes) go into one shared pool. Every agent reflects on that pool and patches itself.
- **Findings.**
  - SWE-bench Verified: 71.0 vs DGM 56.7. Polyglot: 88.3 vs 68.3.
  - The best GEA agent consolidates 8 of 9 key tool innovations (DGM: 5) and has 17 ancestors (DGM: 9).
  - Worst-of-top-5 GEA (58.3) beats the best DGM (56.7).
  - Gains transfer across GPT and Claude backbones.
  - Injected bugs are repaired in 1.4 iterations vs 5.
- **Limitations.**
  - Single runs. Matching DGM's compute is defined only by the number of evolved agents.
  - The meta-level (reflection and evolution modules) is part of the editable agent, so procedure and shared memory are entangled again.
  - No held-out task family. Evaluated on SWE-bench Verified, which is contamination-prone.
- **Overlap.**
  - #1: the shared pool S is a strong form of "improver memory". GEA's gain over DGM is effectively a "memory sharing" effect, so it is another uncontrolled data point for the "memory carries it" hypothesis.
  - Forward-citation descendants:
    - MetaShare 2610.21887 (failure-aware sharing; known).
    - **The View from Nowhere: Population-Centered Agent Evolution** 2610.36660 (known in 2 files).
    - Evo² 2610.13025 ("evolving the evolver through weight–harness co-evolution"; known in 1 file).
    - Harness Updating Is Not Harness Benefit 2605.30621 (known).
- **Severity: low–medium.**

### L5 — From Solo to Social Learning: Characterizing Recursive Social Improvement
- **ID** 2609.38516, 2026-09-29 (v2 Oct 1). UW: Kunal Jha, Max Kleiman-Weiner, Natasha Jaques.
- **Design.** A population of self-interested agents. Each round, every agent chooses whether to revise its own skill file, copy a peer's, or act, all under **one token budget that counts every token, including the search behind copied skills**. Two settings:
  - A controlled bandit-style environment: 10 agents × 100 rounds, 8 seeds, Qwen3-14B, Ministral-3-14B and GPT-OSS-20B, compared against solo/social UCB.
  - Skill-file evolution on IFBench: 100 training and 200 held-out examples, 49 rounds, GPT-OSS-120B and GLM-5.3-Flash, 6 seeds.
- **Findings.**
  - In the controlled environment, every LLM earns **30–52% less reward per token** when it can observe peers. Hand-designed social UCB does benefit.
  - LLM social populations explore too narrowly or run out of tokens before acting.
  - Neither "reserve tokens for acting" nor "require private search" closes the gap.
  - On IFBench, social access helps GLM reach good skills sooner (+1.31 pp averaged over learning) and makes GPT-OSS cheaper (about a third fewer learning tokens). **Neither beats solo at matched cost.**
  - **Most agents' final skills trace back to only one or two of the population's earliest revisions** (lineage concentration).
- **Limitations.**
  - Small models and one instruction-following benchmark.
  - No self-referential improver: the learning policy is the base LLM prompt.
- **Overlap.**
  - None of the four top ideas directly.
  - Its token-matched accounting is the right methodology for GEA-style claims and a model for #1's budget accounting.
  - Lineage concentration is the seed of a non-obvious idea (§3, idea C).
- **Severity: low.** Only one citing paper so far (s³ 2611.08615), so the area is under-explored.

### L6 — Evolving Agents in the Dark: Retrospective Harness Optimization via Self-Preference (RHO)
- **ID** 2606.05922, 2026-06-04. **EMNLP 2026** (S2 venue). MSRA and CityU (Wenbo Pan, Chin-Yew Lin et al.).
- **Design.** A label-free, single-pass harness optimizer working from past trajectories:
  1. A DPP coreset over a judge-estimated difficulty × diversity "fingerprint" (k=10).
  2. G=3 re-rollouts per task, analysed by self-validation and self-consistency.
  3. N=3 candidate harness edits, which can change tools, skills and instructions.
  4. Pairwise **self-preference** of each candidate's trajectories against the old harness. Accept the best candidate only if its relative advantage is greater than 0.

  Everything runs on a GPT-5.5 Codex agent.
- **Findings.**
  - SWE-Bench Pro 59→78, Terminal-Bench 2 71→76, GAIA-2 29→37.
  - Beats Dynamic Cheatsheet, ReasoningBank and Sleep-time Compute.
  - One RHO round (103 calls, no labels) gives 0.78. Meta-Harness gives 0.62 with one round and labels, and 0.80 with 10 rounds and 320 calls.
  - The self-preference selector "never chose a harness worse than baseline".
  - Both signals, and the explicit diagnosis step, matter.
- **Limitations (stated).**
  - **Single round only.**
  - One backbone.
  - Needs resettable environments.
  - "could amplify mistaken preferences … if the evaluator exhibits these traits".
  - Self-preference was never stress-tested across rounds or against a weaker or biased judge.
- **Overlap.**
  - **#3 learning-to-goodhart: medium.** RHO is the canonical example of a *self-judged* improvement loop, and the field is now iterating on it: 29 citing papers by Nov, including RuHarness 2610.36826 (rubric–harness co-evolution), VEvo-Harness 2610.39158, ParetoSaddler 2611.02079 and HarnessPro 2610.26770. Nobody has measured what happens when self-preference gating is *iterated*. Does the harness drift toward what the agent's judge likes (verbosity, verification theatre, its own style) rather than ground truth? That is an untested consequence of RHO's own stated risk, and a direct extension of #3 (see §3, idea A).
  - **#2 evolve-in-dream: low.** RHO re-solves real past tasks in the real environment, with no learned world model. It is evidence that "evolving without ground truth" is publishable at a main venue, and it is a natural baseline: "self-judged on real re-rollouts" vs "simulator-scored".
- **Severity: medium for #3, low for #2.**

### L7 — LLM Agents Are Not Always Faithful Self-Evolvers
- **ID** 2601.22436 (CorpusId 285240886), 2026-01-30. HIT (Wanxiang Che, Ting Liu) and SMU (Yang Deng). The alphaXiv report says ICML 2026 (PMLR 306); S2 lists only arXiv. Treat the venue as **unverified**.
- **Design.** Causal interventions on the experience given to four frameworks (ExpeL, Dynamic Cheatsheet, ReasoningBank, G-Memory), across 13 backbones (Qwen3 1.7B–235B, GPT-4o/5.2, Gemini, Claude Sonnet 4.6) and 9 environments (QA, math, ALFWorld, WebArena, WebShop).
  - Raw-experience interventions: Empty / Shuffle / Irrelevant.
  - Condensed-experience interventions: Empty / Corrupt / Irrelevant / Filler.
  - Also a "no memory section at all" condition, plus gradient-norm attribution by prompt segment.
- **Findings.**
  - Strong dependence on raw trajectories: ExpeL on ALFWorld drops from about 75% to about 0 when raw experience is emptied.
  - **Condensed experience is often causally inert**: corrupt, filler or irrelevant summaries perform close to the real ones.
  - In ReasoningBank, *removing* the summaries hurts but corrupting them does not. The benefit therefore comes from the presence or format of the memory block, not its semantics.
  - The gap persists across scale and in multi-agent setups. Attribution shows condensed experience gets low influence at every layer.
- **Limitations.** Object-level experience only: task trajectories and lessons, never an improver's meta-experience. Success-rate-based. Some small subsets.
- **Overlap.**
  - **#1: medium.** Not a scoop, but a **required citation and method source**. The #1 placebo/shuffled-memory arms are this paper's intervention battery lifted to the meta level. It also makes a sharp, testable prediction for #1: improver memory stored as *condensed* lessons (HMED meta-experience, MetaSkill σ summaries, VERSE notes) will be causally inert, while *raw* archives of past edits and scores will carry the effect. That gives #1 a "raw vs condensed improver memory" sub-factor at almost no extra cost.
  - Descendants worth knowing:
    - Phantom Gains 2608.20290 (known).
    - Evolving Together, Failing Apart 2610.20845 (= CODA, known).
    - Don't Just Trust Your Memory 2611.14320 (known).
    - Hippo 2610.24453 (new, minor).
    - Beyond the Shadows of Plato's Cave (false memory) 2609.39473 (new, minor).
    - Beyond Success Rate: Interventional Evaluation of Memory in VLA 2610.23372 (new; the same idea for robots).
- **Severity: medium (cite and adopt).**

### L8 — Rethinking Experience Utilization in Self-Evolving LM Agents (ExpWeaver)
- **ID** 2605.07164, 2026-05-08. Same HIT group plus Huawei.
- **Design.** Experience construction is held fixed. Only *when* experience is injected changes: init-only vs always-on vs agent-triggered (`[Retrieve]` token). Tested in ReasoningBank, AWM, SkillRL and G-Memory, across 7 backbones, on ALFWorld, WebShop and 6 QA sets. A GRPO variant trains the trigger.
- **Findings.**
  - Agent-triggered use wins consistently.
  - Retrieval clusters at early steps and at high-entropy decision points.
  - Random-timing retrieval does not reproduce the gain.
  - Trained triggering amplifies the benefit.
- **Limitations.** Object level only. Prompting depends on instruction-following.
- **Overlap.** None with #2, #3 or #4. For #1 it is a minor design note: improvers that *choose* when to consult their archive are a different memory regime from always-on. Fix the regime across arms.
- **Severity: low.**

---

## 2. Unseen papers surfaced from forward citations (not in round-1 files)

### N1 — SelfSearch: Reward-Free Search for Self-Improving Agents — **HIGH threat to #1's Cause arm**
- **ID** 2609.37968, 2026-09-29 (v2 Sep 30). Seoul National University (Yohan Jo). It is absent from all round-1 files (grep: 0 hits).
- **Design.** A single agent repository both solves tasks and modifies itself, starting from a DGM-derived B0. In episode k, agent B_k edits a copy of itself using **read-only records of previous self-improvement episodes**: trajectories, code changes and check results. The successor B_{k+1} then becomes the *next improver*.
  - Two lineages, "capability" and "adaptive", share records. Ten generations.
  - **No downstream reward during search.**
  - Search models: GPT-5.6-sol and DeepSeek-V4-pro. Executors: GPT-5.6-luna and DeepSeek-V4-flash.
  - Evaluation: SWE-bench Verified (120), SWE-bench Multilingual (60), Terminal-Bench 2.1 (89).
- **Findings.**
  - The population mean rises over B0 in all 6 model–benchmark settings, by up to +11.2 pp on Terminal-Bench.
  - It is competitive with linear and archive evaluation-guided search at lower cost.
  - Evolved harnesses transfer across model families.
  - **Table 4 mechanism ablations (SWE-bench Verified, gen-10):**

    | Condition | GPT mean | DeepSeek mean |
    |---|---|---|
    | Initial B0 | 75.0 | 81.7 |
    | **w/o episode records** (memory removed; implementation and improver still evolve) | 74.2 | 83.8 |
    | **fixed improver** (B0 reads the records and edits the latest implementation) | 74.6 | 83.8 |
    | Full | 76.2 | 86.7 |

    The authors conclude that "both previous episode records and an evolving improver contribute".
  - Their Table 13 claims they are the only method with an evolving meta-agent, unified roles and reward-free search.
- **Why it matters for #1.** This is the first published paper we have seen that **ablates improver memory and improver procedure separately in one self-referential loop**. A reviewer will cite it against "nobody separates procedure from memory".
- **Gap it leaves.** Large, and #1 should state it explicitly:
  1. **Statistical power.** One search per condition and n=120 tasks. All differences are 1.7–2.9 pp, inside binomial noise of about ±4 pp at 95%. No seeds, no CIs.
  2. **It measures the product, not the improver.** The outcome is the evolved *task agent*, so the "fixed improver" cell changes both who edits and what accumulates. There is no same-start test of improver quality: no generation-k improver applied to a fresh B0 at matched budget.
  3. **No crossing.** There is no Pk + M0 vs P0 + Mk 2×2, no placebo or shuffled records, and no raw-vs-condensed contrast.
  4. **No domain shift.** It has no Decay arm. The transfer it reports is across executor models, not across task domains.
  5. **Reward-free search.** Its "improvement" is undirected self-repair of the agent's own tooling. Most evolved tools are inspection utilities (trajectory reader, line-range viewing), and some are never used downstream.
- **Action.**
  - Add it to #1's related-work table as the "closest two-cell precedent".
  - Optionally use SelfSearch as one of #1's improver families. It is open, cheap at about $4–12 per search, and self-referential by construction.
  - Frame #1's contribution as "SelfSearch-style ablations, done powered, crossed, same-start, and under domain shift".

### N2 — Rethinking Self-Evolving Agent Skills: Feedback Dynamics over Multiple Rounds — medium (#1 Effect arm, #4)
- **ID** 2608.02636, 2026-07-31. HKUST (Yangqiu Song). Absent from round-1 files.
- **Design.**
  - 42 runs over 14 model–benchmark settings: GPT-5.5, Gemini 3.1 Pro and DeepSeek V4-Pro on SearchQA, OfficeQA, SpreadsheetBench, LiveMath and DocVQA.
  - Ten rounds, lateral-accept / strict-best gate (same as RSEA).
  - Feedback views vary: Normal, Fail-only, Success-only.
  - Robustness probes R1–R3 and transfer probes T1–T3.
  - Test-time-scaling controls: oracle parallel sampling and sequential refinement.
- **Findings.**
  - Only **55 of 388 candidates** are byte-distinct new validation bests, and 38 of those 55 come in rounds 1–4. Yet 6 of 11 final selections first appear in rounds 6–9.
  - Success-only is never selected.
  - Oracle parallel sampling comes within 0.43 pt of the evolved skill on SearchQA but trails it by 31 pt on SpreadsheetBench. Sequential refinement recovers neither gain.
  - GPT-5.5 on LiveMath shows a validation-to-test reversal (−6.6).
- **Overlap.**
  - #1 Effect arm: it already runs the compute-matched test-time-scaling baseline at the object level. #1 must cite it, and should adopt "accepted-edit count" rather than "round k" as the generation clock, because rounds are mostly no-ops.
  - #4: a sparse-acceptance view of evolution, but with fixed tasks and no curriculum.
- **Severity: medium.** A methodological precedent, not a scoop.

### Other new or under-tracked items seen (abstract only)

| ID | Paper | Relevance |
|---|---|---|
| 2610.18427 | Reliable Memory from Unstable Growth: Randomized Block Curation | Randomized include/exclude experiments on playbook lessons. The same procedure gives a playbook anywhere from helpful to −69.6 pt. A method template for the #1 memory arms. **New.** |
| 2607.22520 | The Regression Tax | About 6,000 runs. Skills win mainly by *regressing less*. Names "skill description osmosis" (presence effect, echoing L7). **New.** |
| 2611.14326 | When Should Agents Remember? Falsification-Gated Self-Evolution | Gated memory writes. **New.** |
| 2610.36826 | RuHarness | Co-evolving rubrics and harnesses. Relevant to #3/#4. Known. |
| 2610.11464 / 2610.17609 | Who Verifies the Verifier? / Verifier–Harness Co-Evolution | Self-judged loops. #3. Known. |
| 2608.31100 | S3Gym | Self-testing and self-judging into self-improvement, with strict held-out evaluation. **New in files** (it was listed as an unverified lead). |
| 2610.20809 | Self-Improving Agent that Grows its Own DSL: Oracle-Free Operator Induction | Points out that loops keep whatever "convinces a judge model". #3. **New.** |
| 2611.01113 | CogRSI | Evolving and *auditing* reasoning strategies. Asks whether the learned strategies are actually right. **New.** |
| — | "When, Where, and How Experience Becomes Structure" | alphaXiv indexes it under the non-standard ID `2611.recursive-self-improvement-llm-agents` (2026-11-04). It still has no proper arXiv ID. |

---

## 3. Where the field is moving, and non-obvious ideas these leads open

**Trend read (from the citation fan-outs):**
1. **Label-free and reward-free self-improvement is the hottest sub-lane.** Examples: RHO (EMNLP'26) with 29 citers in 5 months, SelfSearch, S3Gym, RuHarness, the Verifier–Harness co-evolution papers and SERPO. Expect several "multi-round RHO" or "self-judged RSI" method papers before May 2027. A *method* paper here will be crowded. A *measurement* paper on what self-judged loops converge to is still open.
2. **Faithfulness and causal-intervention evaluation is moving from object-level memory to everything:** VLA memory 2610.23372, the false-memory paper 2609.39473, Does Failure Feedback Direct 2610.14662, randomized curation 2610.18427. **The meta level (improver memory) is the obvious next target.** Someone will apply L7's battery to Hyperagents or DGM archives. Plan for that within 3–6 months.
3. **Population and social evolution** (GEA → MetaShare, View from Nowhere, Evo², Solo-to-Social) is growing, but almost all of it is method papers claiming gains without token-matched controls. Solo-to-Social is the lone rigorous paper, and it finds *no* matched-cost advantage.
4. **"Gate > artifact"** (RSEA, Feedback Dynamics, TRUST, Winner's Curse) is becoming a consensus. Any improver-scaling claim must hold the gate fixed.

### Idea A — "Self-Preference Drift": what do label-free improvement loops converge to? (extends #3; timely)
- **Claim (falsifiable).** Iterating RHO-style self-preference-gated harness evolution for K rounds produces monotone gains under the agent's *own* judge while ground-truth gain peaks and then declines. The divergence is predictable from the judge's measurable self-preference bias, for example how much it prefers trajectories written in its own style or with more verification steps.
- **Causal design.** Swap the judge (same model, a different-family model, or a deliberately biased judge) while holding the proposer fixed. Measure the true-vs-self gap per round, on held-out tasks with real verifiers (SWE-bench-lite or τ-world).
- **Why less likely to be scooped.** RHO, SelfSearch and RuHarness all report single-round or endpoint gains. Method authors are not incentivized to show drift.
- **Cost.** Moderate: an open 27B model as proposer and judge.
- **Risk.** RHO's "never worse than baseline" claim may hold, i.e. no drift. That null is still publishable: "self-preference gating is surprisingly safe up to K".

### Idea B — "Do Improvers Read Their Own History?" Faithfulness of meta-memory (cheap add-on to #1, or standalone)
- **Claim.** Apply L7's intervention battery (empty, shuffle, irrelevant, filler, corrupt) to the *improver's* memory: SelfSearch episode records, DGM/Hyperagents archives, MetaSkill σ, HMED meta-experience.
- **Prediction.** Raw edit-and-score archives are causally used. Condensed meta-lessons are inert. A large share of the reported "evolving improver" benefit survives filler memory, i.e. it is a presence or format effect.
- **Why sharp.** A filler-memory improver matching the real one would be a striking headline: "recursive self-improvement without reading the recursion".
- **Cost.** Almost free if #1 is already running the transplant cells, since it reuses the same checkpoints. The scoop risk is real (trend 2), so fold it into #1 rather than spin it out.

### Idea C — "Improver Monoculture": does experience sharing destroy meta-level diversity and plasticity? (non-obvious; extends #1 Decay)
- **Claim.** In group- or social-evolving systems (GEA, MetaShare, Solo-to-Social populations), sharing collapses the *improvement procedures* onto one or two ancestral lineages. This mirrors Solo-to-Social's object-level finding that final skills trace back to only one or two early revisions.
- **Prediction.** That monoculture lowers population plasticity after a domain shift, relative to isolated lineages at matched tokens: high pre-shift score, low post-shift gain.
- **Why non-obvious.** Every GEA-line paper reports pre-shift gains from sharing. Nobody measures the post-shift cost.
- **Design.** Isolated vs shared populations at matched total tokens (Solo-to-Social accounting), evolve on domain A, then switch to domain B. Measure improver-procedure diversity (edit-type entropy, lineage coalescence) and B-gain per dollar.
- **Risk.** Needs a population of at least 8 improvers × seeds, which costs more than #1. It could become #1's Decay-arm extension.

### Idea D (smaller) — "Generation is the wrong clock"
Feedback Dynamics shows that most rounds are no-ops. Re-index improver-scaling curves by *accepted edits* (or bits of validation information consumed) instead of generation k. **Prediction:** curves that look like plateaus in k collapse onto a single power law in accepted edits across improver families. This is a methodological hook for #1's Effect arm. It costs nothing extra and makes the paper sharper.

---

## 4. Recommended edits to the round-1 proposals

1. **#1 merged paper.**
   - Add SelfSearch 2609.37968 and Feedback Dynamics 2608.02636 to the closest-work table.
   - Re-word the novelty claim to "first powered, same-start, crossed transplant with a domain shift".
   - Add L7-style placebo/filler cells and a raw-vs-condensed improver-memory contrast (idea B).
   - Add an accepted-edit clock (idea D).
   - Consider SelfSearch as an improver family.
2. **#3 learning-to-goodhart.**
   - Add the self-preference / label-free variant (idea A) as either the second substrate or a pivot.
   - Cite RHO (EMNLP'26), RSEA's 33-pt gate-removal overfit, 2610.20809 and RuHarness.
3. **#2 evolve-in-dream.** Add RHO as a "label-free on real re-rollouts" baseline arm vs "simulator-scored". No new threat.
4. **#4 curriculum-goodhart.** Cite Feedback Dynamics for sparse acceptance. No new threat.
5. **Weekly watch list:** citers of 2606.05922 (RHO), 2609.37968 (SelfSearch) and 2601.22436 (Faithful).

# Round 2 verify: ICLR 2027 / OpenReview / workshop scoop check

*2026-11-12. Task: search ICLR 2027 submissions and other open-review venues (TMLR, COLM/NeurIPS 2026 workshops) for scoops of the round-1 ideas. The user also asked to anticipate where the field is moving and to suggest non-obvious angles.*

## 0. Access status: OpenReview was unreachable

- `https://api2.openreview.net/notes/search?...&group=ICLR.cc/2027/Conference`: **blocked**. The egress proxy refused the connection with CONNECT 403.
- `https://openreview.net/search?...`: **blocked** (WebFetch returned ENOTFOUND).
- `neurips.cc` and `meta-agents-workshop.github.io` also failed (ENOTFOUND), so I could not read workshop accepted-paper lists or NeurIPS virtual pages directly.
- **To search ICLR 2027 directly, add `openreview.net`, `api2.openreview.net` and `neurips.cc` to the session's network allowlist.** Until then, this report is not a census of the ICLR 2027 pool. ICLR submissions are anonymous and poorly indexed by search engines, so OpenReview-only submissions (not posted on arXiv) **remain unchecked.**

**Fallback used:**
1. WebSearch for the ICLR template header `"Under review as a conference paper at ICLR 2027"` combined with topic words. This finds ICLR 2027 submissions that were also posted to arXiv. I ran 7 header queries.
2. WebSearch for NeurIPS 2026 workshop pages, which were reachable only through search snippets.
3. Five alphaXiv `discover_papers` recency sweeps over every topic in the brief.
4. WebFetch on two GitHub "awesome" lists: lobehub/awesome-rsi and leezythu/Awesome-Harness-Self-Improvement. Neither had any ICLR 2027 tags.
5. Every hit was grepped against `research/round1/`. Hits marked **NEW** were not mentioned anywhere in round 1.

## 1. Confirmed ICLR 2027 submissions (header seen in the PDF or in search snippets)

| Paper | Link | Gist | Threatens | Level |
|---|---|---|---|---|
| **R²Flow: Recursive Self-Improvement via Recursive Skill Evolution** (CUHK-SZ/Fudan/NUS). **NEW.** Header verified in the full text. | https://www.alphaxiv.org/abs/2609.33867 | GFlowNet-style flow training over a skill-orchestration graph with verifier-gated, versioned library edits (inner policy loop, outer library loop). Reports "0.89 held-out edit precision", that the method "transfers across executors", and an RQ6 showing that injected harmful skills raise training reward while lowering held-out verified score. | #1 Effect/Cause, weakly. Learning-to-Goodhart, weakly. | **Low.** Method paper. It never asks whether the improver gets better at improving, and has no procedure/memory transplant. Its executor-transfer result and its "harmful skill raises train reward, lowers held-out" result are useful citations. |
| **Skill overfitting in self-evolving agents** | https://www.alphaxiv.org/abs/2609.39148 | Defines and measures "skill overfitting": self-evolved skills keep less of their train gain on test. Six methods, six benchmarks. | Curriculum-Goodhart; #1 Decay (object level). | **Low–medium.** Already in round 1 (benchmarks-failures sweep, saturation-law critique). Object-level only. |
| **Autogenesis System** | https://www.alphaxiv.org/abs/2604.15034 | Self-evolving multi-agent system that separates *what* evolves from *how* it evolves. Includes an executable transition model that the agent corrects (section A.2). GAIA 89.70 → 93.33. | Evolve-in-dream, weakly. | **Low.** Method paper. It repairs a world model but has no fidelity sweep. |
| **SWE-Duel: Towards a Self-Scaling Arena for Coding Agents** | https://www.alphaxiv.org/abs/2610.12569 | Coding agents write tasks for each other and evaluate each other. 11 model-harness pairs, 15 repositories, 1,500 turns. | Curriculum-Goodhart, weakly. | **Low.** Arena/benchmark. No improver dynamics. |
| **OpenForge RL** | https://www.alphaxiv.org/abs/2607.21557 | RL across arbitrary harnesses and environments in cloud containers. Studies how harness × environment training changes agent behavior. | Harness-half-life, weakly. | **Low.** |
| **Science or Slop? (SciSlopBench / SciSlopHarness)** | https://www.alphaxiv.org/abs/2610.00531 | Benchmark for AI-generated papers. Notes that "direct slop-aware prompting triggers reward hacking" in a harness-level reviser. | None directly. | **None.** Off-topic. |

The header search also returned off-topic ICLR 2027 submissions: VAULT (2606.06219, driving), CHERRL (2606.04923, a reward-hacking testbed for rubric RL), "Just Ask JEV" (2609.29429) and TSR (2602.11767). None threatens a round-1 idea. CHERRL is a weight-RL rubric-hacking testbed and could be a design reference for Learning-to-Goodhart.

**Bottom line for ICLR 2027 (arXiv-visible part):** no submission I found runs an improver-checkpoint swap, a procedure × memory transplant, a meta-plasticity or warm-start test, a fidelity sweep for harness evolution in a simulator, or a held-out-family dose-response for exploit learning. This **does not** cover OpenReview-only submissions.

## 2. NEW papers outside round 1 that matter (arXiv; venue unknown or workshop)

### 2.1 SelfSearch: Reward-Free Search for Self-Improving Agents (SNU), 2609.37968. **Medium threat to #1 Cause.**
- Link: https://www.alphaxiv.org/abs/2609.37968 (preprint header; venue not stated). Round-1 `extra_leads.md` listed it as a lead without an ID; it was never read.
- **Gist:** agents modify their own code using *records of previous self-improvement episodes*, with no downstream reward during search. Two lineages, 10 generations. Up to +11.2pp on Terminal-Bench 2.1, and a $4.03 search producing an 82.0% Terminal-Bench 2.1 harness. Gains transfer across model families.
- **Why it matters:** Table 4 is a **2-cell version of the procedure × memory transplant.**
  - "w/o episode records" removes memory but keeps an evolving procedure.
  - "w/ fixed improver" keeps B0 as the improver but gives it the records.
  - On SWE-bench Verified, removing records costs −2.1pp (GPT) and −2.9pp (DeepSeek). Fixing the improver costs −1.7pp and −2.9pp.
- **Gap it leaves:**
  - One domain, one generation (10), two lineages, no seeds or CIs. The effects are 1–3pp on about 120 tasks, so they are within noise.
  - No cross-domain test, so nothing on Decay.
  - No compute matching, and no full factorial (it lacks the P0+M0 cell under the same budget and the Pk+Mk cell on a new domain).
- **Action:**
  - Add SelfSearch to #1's related work as "first ablation of records vs improver, unpowered".
  - Treat it as **evidence that the factorial question is now visible** to a second group.
  - Its "reward-free" result also motivates a **placebo/no-feedback control** (see §4, idea N1).

### 2.2 Found by Search, Broken by a Shortcut (Northeastern), 2611.04838. **Low–medium threat to Learning-to-Goodhart.**
- Link: https://www.alphaxiv.org/abs/2611.04838. Pre-registered, with a public commit.
- **Gist:** CMA-ES search over intrinsic-reward "drives" for PPO in MiniGrid, with 1,560 search runs. The searched reward transfers to a held-out task. A **free reset exploit absent during the search** collapses it from 0.829 to 0.136 area. The paper concludes that "selection gave E no protection against an exploit absent from the search".
- **Gap it leaves:** weight RL with a searched *reward*, not an LLM harness or improver. It runs one exploit and no dose-response over exploit families. It tests the *opposite* direction to Learning-to-Goodhart: whether search confers *resistance* to unseen exploits, not whether it confers a *propensity* to find them.
- **Action:** cite it as the RL analogue. The pre-registration style (locked protocol and size rule) is a good template for Dan's pre-registration.

### 2.3 From Single-Agent Self-Discovery to Multi-Agent Adaptation (Cambridge/Models2AI), 2611.01742. **Low threat. Methods idea for #1.**
- Link: https://www.alphaxiv.org/abs/2611.01742
- **Gist:** about 1,800 runs on meta-learned update rules (Disco103, OPEN). **"Four-cell counterfactual partner swaps"** expose an "evaluation illusion": a team gain of +1.066 hides a focal loss of −0.119. It also includes "48 multi-lifetime recursively meta-evolved rules".
- **Threat:** none directly, since it is meta-RL. However, the 4-cell swap is the same logic as #1's Effect arm, so cite it as cross-field precedent for swap designs in RSI.

### 2.4 NeurIPS 2026 workshop papers (from search snippets; the pages themselves were unreachable)
- **Evolving Verifiers for Principled, Training-Free Open-Ended Self-Improvement** (Xu & Wang). Listed at https://neurips.cc/virtual/2026/165587, which is a test-time continual-learning agents workshop. UCoE selects and evolves verifiers: 57.2% tie-adjusted vs direct generation on Arena-Hard-v2 with Qwen3-8B. **Low threat.** Method only.
- **Who Verifies the Verifier? Co-Evolving Inspectable Graders with Self-Improving Agents** (arXiv 2610.11464; NeurIPS 2026 listing https://neurips.cc/virtual/2026/172936). Round 1 mentions it in 2 files.
  - **Key finding (from snippets):** with anchor guards removed, the verifier collapses to a vacuous always-pass grader, *yet that collapsed verifier still trains skills just as well*. The authors argue downstream task score cannot certify a self-evolved verifier.
  - **Medium relevance** to Curriculum-Goodhart and Learning-to-Goodhart. It is evidence that *feedback quality may barely matter* to skill loops, which supports idea N1 below.
- **MetaShare** (2610.21887; accepted at the NeurIPS 2026 Meta-Agents workshop). Failure-aware experience sharing across lineages; matches DGM-Hyperagents with fewer iterations. Known in round 1. **Low threat.**
- **MELD: Recursive Self-Improvement via Hierarchical Coordination for Multi-Agent Scientific Discovery** (title only, from the TTCL workshop schedule snippet). Unverified, no ID. **Low threat** by title.
- **SkillOpt: Executive Strategy for Self-Evolving Agent Skills** (NeurIPS 2026 main poster). Harness-agnostic optimizer over a single skill document. Its ablations name "validation-gated edits and update memory" as keys to portability. Known in round 1. **Low.**
- **Relevant workshops whose accepted lists I could not read** (Dec 2026, Sydney):
  - Meta-Agents ("Managing Agents that Manage Agents"; camera-ready due Nov 20).
  - AIWILD (accepted papers go public on OpenReview after Dec 4).
  - Verify-Agents, TTCL, CL4FMAgents and CLEA.
  - **Re-check these after Dec 4 with OpenReview access.** They are the most likely home for small, fast swap or transplant studies.

### 2.5 Other new IDs seen; none threatens a round-1 idea
- AReaL-RSI 2610.18083 (scaling RSI researcher agents).
- PyRUA-REAL 2611.06768 and Compounding Physical RSI 2611.06654 (robot RSI).
- A Cross-Scenario Survey of Self-Evolving Multimodal Agents 2611.14256.
- SimHack 2610.28707 (predicting reward hacking before RL training; weights).
- Reglos 2611.06049.
- MetaVul 2610.40721.
- EvoRS 2609.12459; Evaluator Ensembles Under Reward Hacking 2608.08002.

## 3. Threat summary vs round-1 ideas

| Round-1 idea | Strongest new threat found here | Net change vs round 1 |
|---|---|---|
| #1 Effect/Cause/Decay | SelfSearch 2609.37968 (2-cell records-vs-improver ablation); R²Flow (executor transfer) | **Slightly higher risk.** The *question* now appears as an ablation in a second paper, after MetaRSI, RSIFlow and FreeEvolve. Still nobody runs a powered factorial or a cross-domain warm-start test. Keep the preprint target of Feb–Mar 2027 and consider moving it earlier. |
| Evolve-in-dream | Autogenesis (repairs a transition model). No new fidelity-sweep paper. | Unchanged. |
| Learning-to-Goodhart | 2611.04838 (RL, single unseen exploit); Who-Verifies-the-Verifier (vacuous grader still trains) | Unchanged to slightly better. The held-out-family dose-response for harness or improver search is still open. |
| Meta-plasticity | none new | Unchanged. Agent Plasticity's future-work line is still the main risk. |
| Curriculum-Goodhart | Skill overfitting 2609.39148 (known); SWE-Duel | Unchanged. |
| Harness-half-life | OpenForge RL (weak) | Unchanged. |

## 4. Where the field is moving, and non-obvious angles (per Dan's request)

### 4.1 Trajectory I expect by the ICML 2027 and NeurIPS 2027 deadlines
1. **From audit essays to minimal swap ablations.** The Oct–Nov 2026 wave was mostly audits: 2610.27610, Beneath the Surface, the RSI Tax and Third Eye. Method papers are now adding "fixed improver" and "no records" cells (SelfSearch, MetaRSI, RSIFlow), and the meta-RL world is doing 4-cell swaps (2611.01742). **Expect at least one powered same-start swap or transplant paper by ICML 2027 (late Jan).** #1's novelty must therefore rest on **Decay (cross-domain warm-start penalty) plus the full factorial with compute matching**, not on "we ran a swap".
2. **Verifier/evaluator co-evolution is the hot method lane.** Examples: UCoE, Who-Verifies-the-Verifier, Critic Falls Behind 2610.36826, Red Queen Gödel Machine 2606.26294 and EvoRS. The measurement gap here is whether *any* feedback signal is doing work.
3. **Reward-free or self-supervised improvement** (SelfSearch, SBCO). This is growing, and it quietly undermines the assumption that evaluation-guided selection drives the gains.
4. **Executor and model transfer of evolved harnesses** is now a standard table (R²Flow, SelfSearch, TinyAstral). Upgrade survival is crowded, so stay away from it as a standalone topic.
5. **Embodied and physical RSI** (PyRUA-REAL, Compounding Physical RSI, RSI-Anything) is new in November. World-model and RSI framing is going mainstream, so a fidelity-causal design is what keeps Evolve-in-dream distinct.

### 4.2 Non-obvious angles, less likely to be scooped but still impactful

These are small and sharp. Each can be added as an arm of #1 or written as a standalone short paper.
- **N1 — "Placebo evaluator" test for self-referential improvement.**
  - **Claim to falsify:** an improver's gains come from the feedback it selects on.
  - **Design:** run the same improver with (a) the real evaluator, (b) a shuffled-label evaluator, (c) an always-pass evaluator and (d) no evaluator (SelfSearch-style), all with matched compute.
  - **Evidence that the surprising outcome is plausible:** SelfSearch gets gains with no reward, and Who-Verifies-the-Verifier finds that a vacuous grader trains skills "just as well".
  - **Payoff:** if (b), (c) and (d) roughly match (a), most "self-improvement" is prior-driven refactoring, not learning from feedback. That is a sharp, surprising headline, and it costs about the same as #1's pilot.
  - **Scoop risk is low:** nobody frames it as a placebo control for improvers. It also fits #1 as a control arm.
- **N2 — Improver–executor coupling.** Does a generation-k improver's procedure encode its executor? Swap only the *executor* under a fixed evolved improver, and measure how much of the improver's advantage survives. R²Flow and SelfSearch report object-level executor transfer, not *improver*-level transfer. This is cheap and orthogonal to the crowded upgrade-survival lane.
- **N3 — "Selection confers no immunity" for harness search.** This is the LLM-harness analogue of 2611.04838. After evolving on a clean evaluator, introduce a new exploit (reset, test-file write, or grader leak). Does evolution history raise or lower the rate of discovering the new exploit, relative to a fresh agent? This is the "resistance" mirror of Learning-to-Goodhart. Both directions together make the 2×2 more surprising.
- **N4 — Population swap illusion.** Shared-archive and multi-lineage methods include GEA, MetaShare and SelfSearch's two lineages sharing records. Use 2611.01742's 4-cell partner-swap design to test whether lineage gains are carried by the *shared archive* rather than by any lineage's own procedure. This is a natural sub-arm of #1's memory transplant.
- **N5 — Decay as a deadline hedge.** Make the cross-domain warm-start penalty the headline of #1. It is the arm nobody touches: SelfSearch, MetaRSI and RSIFlow are all single-domain. It is the most likely "surprising" outcome, and it falls most directly out of Gödel Agent's stated limitation.

## 5. Follow-ups
1. **Re-run this search directly on OpenReview** once `openreview.net` and `api2.openreview.net` are allowlisted. Use group `ICLR.cc/2027/Conference` and the terms: recursive self-improvement, meta-agent, self-referential, harness evolution, plasticity, improver, reward hacking self-evolving, world model agent evolution.
2. After Dec 4, read the accepted lists of the NeurIPS 2026 Meta-Agents, AIWILD, TTCL and Verify-Agents workshops.
3. Read SelfSearch Table 4 and its appendix closely (task count, number of runs) before citing effect sizes.

## Sources consulted (all from tool results)
- WebSearch hits for the ICLR 2027 header: arXiv 2609.39148, 2609.33867, 2607.21557, 2610.12569, 2604.15034, 2610.00531, 2606.06219, 2606.04923, 2609.29429, 2602.11767.
- alphaXiv: 2609.37968, 2611.04838, 2611.01742, 2609.33867, 2610.00531 (full text read); the other IDs from discover_papers listings only.
- Workshop pages via search snippets: neurips.cc/virtual/2026/165587, neurips.cc/virtual/2026/172936, agentwild-workshop.github.io/neurips2026, meta-agents-workshop.github.io, github.com/YLfan99/neurips2026-workshops (fetched).

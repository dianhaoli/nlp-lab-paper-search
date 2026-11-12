# Round-2 fact-check of load-bearing round-1 claims

*Checked 2026-11-12. Every quote and number below was read from the paper text, using alphaXiv `answer_pdf_queries` on the PDF pages, plus a Semantic Scholar title search for the two papers without IDs. Anything not seen in a tool result is marked "unverified".*

## Verdict table

| # | Claim / paper | Verdict | One-line correction |
|---|---|---|---|
| 1 | "Open models cannot drive Hyperagents" (attributed to 2609.17817) | **VERIFIED, but narrower than stated** | It is a *preliminary* observation about stock Hyperagents code with Ollama-Cloud open models. The same paper shows Qwen3.5-397B **can** drive DGM and SICA. Several other papers run self-evolving improvers on open models (see §1). |
| 1b | Hyperagents 2603.19461: which models it used | **VERIFIED** | Self-modification used Claude 3.5 Sonnet (Polyglot) and Claude 4.5 Sonnet (all other domains). Evaluation used o3-mini, GPT-4o, Sonnet 4.5 and o4-mini. No open-weight model was used. |
| 2 | SelfImprove-Bench 2611.02797: "fixed-improver 1.207 vs editable 1.433" | **MISREPRESENTED** | The matched pair is **1.207 (fixed improver) vs 1.326 (editable improver)**, both HyperDSH with a DeepSeek backbone. 1.433 is a *different system*: the main-experiment HyperAgents agent, which retained **zero** improver edits. |
| 3a | "When, Where, and How Experience Becomes Structure" | **EXISTS; no arXiv ID** | alphaXiv-only, slug `2611.recursive-self-improvement-llm-agents`. Dai, Ruan, Zhang et al. (CASIA/Meituan). It is a **survey** with no experiments. Semantic Scholar does not return it. |
| 3b | "Scaling Discovery Does Not Require Forgetting: Verified Shared Archives…" | **EXISTS; no arXiv ID** | alphaXiv-only, slug `2611.verified-archives-multi-agent-search`. Single author, Lindell S. Cumes (QWNTL Labs). Full title ends "…Break the Multi-Agent Plateau on Anthropic's Performance Take-Home". Semantic Scholar does not return it. Round-1 numbers are correct. |
| 4 | 2610.27610 Auditing the Product | **VERIFIED** | Round-1 numbers are all correct (60% / 22% / 37% / 4 of 26). |
| 4 | 2610.08902 Agent Plasticity | **VERIFIED** | Its future-work section names **post-training for plasticity**, **artifact removal/replacement/forced-invocation interventions**, **budget-matched no-feedback reflection** and **actor–artifact swaps**. All of these are now publicly flagged directions (scoop risk). |
| 4 | 2609.06396 MetaRSI | **VERIFIED, and stronger than round 1 said** | It is a same-start, same-budget swap (+7.2 vs +4.9). It also has a **5-term compounding curve** (+26.2 vs +20.7) and 5 outer seeds. It runs on an **open model** (Qwen3.5-35B-A3B) with no teacher. |
| 4 | 2610.21791 RSIFlow | **VERIFIED** | Backbone sensitivity: GPT-5.6-Sol +24.39, Claude-Sonnet-5 +11.59, DeepSeek-V4.1-Flash +4.88 (HumanEval+). Its Meta0…Meta4 same-start table is a single run. |
| 4 | 2610.09197 FreeEvolve | **VERIFIED, with an important nuance** | Haiku 4.5 as evolver does not *fail to move*. It **overfits**: evolution-set score is 52.2 against 35.0 for no meta-evolution, while held-out is 32.2 against 33.3. Gains start at Sonnet 4.6. Opus 4.6 (40.2) is worse than Sonnet 4.6 (47.1). |
| 4 | 2610.07979 HMED | **VERIFIED** | Branch outcome and matched-continuation Δ **disagree in sign in 45.5% (ALFWorld) and 75.0% (ScienceWorld)** of comparable revisions. Models: Qwen3-32B and DeepSeek-v4-Flash. |
| 4 | 2610.02616 VERSE | **VERIFIED** | Uses open models throughout: optimizer Qwen3.8-Flash-Next, executor Qwen3.8-27B. Self-evolution without verification scores 31.79, *below* the blank harness at 33.95. With verification it scores 41.05. One evolution run per row. |
| 4 | 2610.20563 Scaling Discovery Requires Forgetting | **VERIFIED** | Every number checks out. Model: **DeepSeek V4.1 Flash** (cheap, open weights), Pi agent, 3 runs per configuration. |
| 4 | 2611.12865 Shortcuts and Boundaries | **VERIFIED, with a caveat** | Weight-level GRPO on Qwen3.5-4B. The "transfer to external hacking benchmarks" evidence is *illustrative*: 2 BaitBench submissions and a 25-task HVTA subset. |
| 4 | 2611.08318 Learning the Task or the Verifier? | **VERIFIED** | Score under the original criteria: exposed 51.6 vs success-only 27.8. Under instruction-faithful criteria: 45.0 vs 47.2. In the swap, the library trained on faithful criteria scores 65.0 under them and 13.5 under the original criteria. |
| 4 | 2608.12253 One Frozen Simulator Is Not Enough | **VERIFIED, with a caveat** | Held-out reward "falls below base" only for the most modal simulator (Gemini-3-Flash) and in the P4G human study. With GPT-5-mini the decline is gentler. |
| 4 | 2611.01139 World Models Need Not Be Right | **VERIFIED** | "Model-blind collapse" is the authors' own term. Two DMC tasks only. The authors are from a school (Ideal School and College) and UWE Bristol. |
| 4 | 2611.13034 RoboACE | **VERIFIED** | Co-evolves a curriculum harness and an execution harness. **No proxy-vs-truth tracking. One campaign.** Its initial harness (63.5 on LIBERO) is far *below* the bare π0.5 controller (96.9). |

---

## 1. "Open models cannot drive Hyperagents"

**Source found: 2609.17817 ("Reflections on Trusting Trust, Revisited", Roesner & Kohno, 15 Sep 2026), §3.4.1.** Exact quote:

> "In the original Hyperagents paper, the system was run with Sonnet 3.5 for the Polyglot benchmark [16] and Sonnet 4.5 for others. In our preliminary experiments, we found that none of our Ollama models were powerful enough to drive Hyperagents: neither gpt-oss-120b, Qwen3.5-397B, nor even DeepSeek V4 Pro (a frontier-class model) were able to converge on useful self-improvements, producing only empty or broken patches. Thus, we ran all of our Hyperagents experiments with Sonnet 4.5."

**The same paper's other model findings:**
- **DGM:** "gpt-oss-120b or Qwen3.5-397B via Ollama Cloud … were sufficiently powerful to handle the DGM's self-improvement step (whereas weaker models, like gpt-oss:20b, produced broken patches)."
- **SICA:** "gpt-oss-120b was not powerful enough to drive the self-improvement step … Thus, we conduct our SICA experiments with Qwen3.5-397B and Sonnet 4.5." The authors attribute the difference to prompt scaffolding. DGM's self-improvement prompt "enumerates specific pitfalls to avoid", whereas SICA's prompt is open-ended.
- Hyperagents' self-improvement scaffolding is likewise open-ended: the initial meta prompt is "Modify any part of the codebase at `{repo_path}`" (Hyperagents App. A.1).

**What Hyperagents (2603.19461 v2, 1 Nov 2026) used (Table 3, App. D.1):**

| Domain | Self-modification | Evaluation |
|---|---|---|
| Polyglot | Claude 3.5 Sonnet (New) | o3-mini |
| Paper review | Claude 4.5 Sonnet | GPT-4o |
| Robotics reward | Claude 4.5 Sonnet | Claude 4.5 Sonnet |
| IMO grading | Claude 4.5 Sonnet | o4-mini |

The cost is about 33M tokens for 100 iterations of self-modification alone, and about 83.6M tokens total for paper review plus robotics. The paper uses no open-weight models. The v2 text also hedges its own scope: results "do not yet establish unbounded or self-accelerating improvement."

**Counter-evidence: open models driving self-modifying improvers elsewhere.**
- **SelfImprove-Bench 2611.02797** runs HyperDSH, "a variant that applies HyperAgent's improvement mechanisms to Deepseek Harness". With a DeepSeek backbone (Fig. 1 labels it deepseek-v4.1-flash) it scores 1.326, beats the original DeepSeek Harness (0.751) and retains 40.5% Meta (improver) opinions.
- **VERSE 2610.02616**: a self-evolving optimizer on Qwen3.8-Flash-Next with a Qwen3.8-27B executor. It works *only with execution-based verification*.
- **MetaRSI 2609.06396**: Qwen3.5-35B-A3B fills every role, including the meta-level scheduler (+10.9 average).
- **HMED 2610.07979** (Qwen3-32B, DeepSeek-v4-Flash) and **RSIFlow** (DeepSeek-V4.1-Flash as meta-agent, but only +4.88).
- **Forgetting 2610.20563**: DeepSeek V4.1 Flash, about $20–40 per run.

**Implication.** The correct statement is: "Stock Hyperagents with its open-ended meta prompt did not converge with Ollama-hosted gpt-oss-120b, Qwen3.5-397B or DeepSeek V4 Pro in one group's preliminary runs (2609.17817)." This is a **scaffold × capability interaction**, not a hard capability floor. Proposals should (a) budget a Sonnet-class improver for any stock-Hyperagents arm, or (b) treat "the open-endedness of the meta prompt sets the capability needed to self-modify" as a *measurable variable*. Option (b) is a non-obvious, cheap finding that nobody has run systematically. The DGM-vs-SICA-vs-Hyperagents contrast in 2609.17817 is anecdotal evidence for it.

**Bonus finding from 2609.17817 (non-obvious opportunity).** Polyglot staging in both DGM and Hyperagents had a **git-history answer leak**: "hidden tests and reference solution … remained in the repository's history … reachable via git-history commands. On some preliminary solution traces, we observed Sonnet 4.5 exploiting this to 'cheat'." The Hyperagents authors "confirmed the issue and our proposed fix." Whether o3-mini or Sonnet 3.5 exploited the leak in the *published* DGM and Hyperagents Polyglot curves is explicitly not known ("may not have exploited it"). That makes an audit of published Polyglot self-improvement gains a cheap, quotable check.

---

## 2. SelfImprove-Bench 2611.02797: 1.433 vs 1.326

**What the benchmark measures.** It covers six text games: Farm and Combat (planning), Sweepmine and Hero (reasoning), and MC and Astray (exploring, from GameWorld). Tasks come from templates with difficulty coefficients 1–5. Its *generalization ladder* has five levels:
- G0: replay of seen instances.
- G1: new instances at the same difficulty.
- G2: harder difficulty.
- G3: a different game with the same capability.
- G4: a different capability.

The metric is the **Headroom-Closed Index**, HCI = (s_a − s_b)/(s_r − s_b). Raw "scores" are ratios to a pseudo ground-truth solver. Because the reference is a heuristic search, scores can exceed 1. The methods evaluated are ACE (experience level), AHE (harness level) and HyperAgents (meta level). The paper's stated finding is that "their effectiveness varies across the generalization ladder, with their relative rankings reversing."

**Where the numbers come from (Table 5, Medium held-out Combat):**

| Configuration | Score |
|---|---|
| DeepSeek Harness (original) | 0.751 |
| HyperDSH (GPT-5.6 Sol) | 1.274 |
| HyperDSH (Claude Opus 5.5) | 0.941 |
| **HyperDSH (DeepSeek)** | **1.326** |
| **HyperDSH (DeepSeek, fixed improver)** | **1.207** |
| **HyperAgents (main experiment)** | **1.433** |

Quote: "the DeepSeek variant performs best among them at 1.326, below HyperAgents at 1.433. HyperDSH (DeepSeek, fixed improver), which keeps the improver unchanged while continuing to refine DeepSeek Harness, scores 1.207 on Medium, compared with 1.326 for HyperDSH (DeepSeek)."

**Why round 1 was wrong.** The round-1 sweep (benchmarks-failures.md) wrote "A fixed-improver HyperAgents variant scores 1.207 vs 1.433 for the editable one." That compares two different systems. The like-for-like editable-vs-fixed contrast is **1.326 vs 1.207 (+0.119)**. It is a single, unreplicated, evolving-vs-frozen comparison, which 2610.27610 says does *not* identify an improver gain.

**The non-obvious part.** The top scorer, 1.433, comes from the main HyperAgents agent, and that agent retained **zero Meta (improver) opinions**:
- "Despite permission to modify the entire repository, the main-experiment HyperAgents retains no Meta opinions, suggesting a limited tendency to modify its own improver spontaneously."
- Table 6: HyperAgents retained 46.2% Prompt/Memory, 53.8% Harness and 0.0% Meta opinions on Combat.
- Across the main experiments: "87.9% of retained opinions target prompts and memory and 12.1% the execution harness; none modify the improver."
- When improver self-evolution is explicitly allowed (HyperDSH), Meta makes up 20.5–45.2% of retained opinions.

Across this one table, then, the best result has no meta edits, and meta edits buy about +0.12 within one setup. 1.433 is also the score of the "random medium" training schedule in Table 4; the fixed-instance schedule scores 0.772.

**Implication.** Treat SelfImprove-Bench as evidence that (i) improvers rarely self-modify unless prompted to, and (ii) *prompting* them to self-modify changes the edit distribution heavily (0% → 20–45% Meta) with only a modest score effect. That is a clean motivation for a "does meta-editing matter at all?" question. It is not evidence for a big editable-improver gain.

---

## 3. The two papers without IDs

### 3a. "When, Where, and How Experience Becomes Structure: Mechanisms for Recursive Self-Improvement in LLM Agents"

- **Where it lives:** alphaXiv-only, id `2611.recursive-self-improvement-llm-agents` (v1). No arXiv number was found, and the Semantic Scholar title search did not return it.
- **Authors:** Siqi Dai, Jingqing Ruan, Bin Zhang (corresponding), Zhiwei Xu, … Guoliang Fan. Affiliations include CASIA, Meituan and Shandong University.
- **Type:** a **survey** with no experiments. It covers 20 update loops from 20 studies, organized as 4 components (Prompt/Memory/Skill/Harness) × 7 functions (Collect, Evaluate, Diagnose, Abstract, Update, Verify, Consolidate).
- **Relevant quotes:**
  - "does inherited state help produce more effective future updates under comparable experience, feedback, and computational budgets?"
  - "Reuse across cycles and improvement of the update method are also separate questions. Harness² retains components and experience in an improvement library … VERSE additionally modifies some of the content used by its improver."
  - "Common starting conditions and reported feedback and compute budgets help distinguish benefits of accumulated experience from those of additional search."
- **Implication:** cite it as motivation. It publicly poses the procedure-vs-memory question, so the *question* is not novel. The *controlled answer* is still open.

### 3b. "Scaling Discovery Does Not Require Forgetting: Verified Shared Archives Break the Multi-Agent Plateau on Anthropic's Performance Take-Home"

- **Where it lives:** alphaXiv-only, id `2611.verified-archives-multi-agent-search`. Project page qwntl.com/paper-societies-horizon. No arXiv number, and not in Semantic Scholar.
- **Author:** Lindell S. Cumes (QWNTL Labs), single author.
- **Numbers, all verified:**
  - Four Claude Opus 5.5 agents, 8 h, 3 runs per design.
  - Archive median **914** cycles [913–916]; CORAL port **948** [925–950]; isolated agents **946** [921–950].
  - Permutation p = 0.05 with 3 runs vs 3 runs; pooled p = 0.008 and 0.004.
  - Agent spend: $842 (archive), $1,237 (CORAL port), $553 (isolated) per run.
- **False floor:** "Several agents of our port conclude that about 951 cycles is near the best possible score and stop submitting attempts." **7 of 12 CORAL-port agents** are in a loop of 50 or more sessions under 60 s that make no attempt. No archive agent or isolated agent shows this.
- **The critique of Forgetting, verbatim:** "Forgetting … deletes two things this comparison never separates: the scored attempts and each agent's conversation."
- **Caveats:** one task for the main comparison, Claude-only, 3 runs, no held-out generalization, no harness or improver evolution.

---

## 4. Threat papers: verified claims

### 2610.27610 Auditing the Product, Not the Recursion (Chen, Zhu, Zheng; Stevens/UMass Boston; 20 Oct 2026)

**Census (all VERIFIED).**
- 114 eligible studies, 90 of them strong-label RSI.
- 90% report broad product evaluation; 60% evaluate on tasks unused for selection; 22% audit two or more reruns; 37% have a budget-matched comparator; 19% a null control.
- "Of the 26 studies claiming a better improvement procedure, 23 report a related comparison, but only 4 compare earlier and later procedures from identical starting artifacts at a matched budget."
- Six strong-label studies have a same-start design; 4 of them claim a better improver.
- In the human-coded sample, the rates are 45% and 14%.

**Claim taxonomy (Table 2).** C1 Product, C2 Accumulation, C3 Repeatability, C4 Improver gain (a "swap test"), C5 Compounding: "Repeated swap tests showing growing improver gains over a stated horizon, with total cost". C5 is listed as "not measured" in the census.

**What it says about Hyperagents' frozen-improver control:** it "answers a different question … its total effect measures the value of letting the improver change over a run."

**Self-reported limits.**
- An LLM assistant did the first coding pass; individual codes outside the adjudicated sets were not human-checked.
- The search is phrase-based ("RSI").
- RSIFlow is coded P4 = P (partial).

**Not verified:** the round-1 list "MetaRSI, Sivia, MASS, STOP" as *the* 4 same-start studies. I did not see the paper's list of those four.

**Implication:** a census-level scoop of the "RSI evaluation is weak" framing. It does not run any swap test itself. C5, repeated swaps with total cost, is explicitly unmeasured. MetaRSI claims it (below), but with a single protocol.

### 2610.08902 Agent Plasticity (Singh, …, Weston, Mahajan, Goyal; Berkeley/Meta MSL; 6 Oct 2026)

**What is verified.**
- P = held-out gain per $1,000 of learning cost. Frozen weights; self-constructed artifacts (tools, skills, memory). Chess, Go and Hex use 20 checkpoints; NetHack uses 40. The same model does acting and reflection.
- Fable 5 reaches the highest endpoint, while GPT-5.6 Sol has the highest P_sat.
- In NetHack only Opus 5.5 shows a reliable rise.

**Exact future-work and limitation statements (load-bearing for scoop risk):**
- "Controlled interventions—such as artifact removal, replacement, or forced invocation—could help distinguish these mechanisms."
- "Budget-matched reflection without feedback, as well as controlled actor–artifact comparisons, could disentangle these effects."
- "separately trained lineages do not establish transfer of learned artifacts or improvement strategies across tasks."
- "**Plasticity may itself be a target for post-training.** … reinforcement-learning objectives could reward not only immediate task performance, but also the ability to construct, retain, and effectively use knowledge that improves future behavior … Studying which training objectives, feedback signals, and curricula improve such plasticity is an important direction for future work."
- "Plasticity is therefore a property of an agent under a particular learning protocol rather than an intrinsic scalar property of the underlying model."

**Implication.**
- A Meta MSL team has publicly flagged RL-for-plasticity, actor×artifact swaps and no-feedback reflection controls. Expect fast follow-ups from that team. **Do not build on "train for plasticity"** as a novelty claim.
- Still open in this paper: plasticity of a *separate improver*, i.e. meta-level plasticity; anything about the improver's own history or erosion; and cross-task transfer of improvement strategies.

### 2609.06396 MetaRSI-v1 (Tan, Sun, … Wan; ~31 authors; v2 9 Sep 2026)

**Setup.** Target Qwen3.5-35B-A3B. "Every model-driven role is instantiated on the target model itself … No stronger external model proposes, synthesizes, judges, or schedules." Five outer seeds. Equal budgets in tokens, GPU-hours, wall-clock, operator invocations, candidates and verifier queries.

**Main results (Table 3).**
- MetaRSI +10.9 average, against Harness-RSI +6.6 (the best single operator) and human-fixed D→M→H +7.3.
- TB2.1 23.6→31.9; SWE-bench Pro 10.3→19.5; GPQA-D-hard100 71.2→83.8; AIME 55.0→68.3.

**Swap test (§5.4).**
- "Term 2 forks that release. One branch improves S1 again with the original improver; the other improves the same S1 with the improver the meta layer has since rewritten, under the same budget … The meta-updated improver adds 7.2 average points where the original adds 4.9."
- **Compounding:** cumulative gain after five terms is +26.2 vs +20.7. Per-term gain decays in both arms: from +4.9 to +0.7 without the meta-update and from +7.2 to +1.4 with it. The plateau comes at term 4 vs term 5.
- **Policy carry-over:** 68% vs 38%. **Scheduler regret:** 8.2% vs 16.5%.

**Frontier harness route.** Six frontier models gain +5.6 to +9.2 on TB2.1 (mean +7.3), then +2.8 and +1.2 in later terms. Retention correlates with headroom (ρ = −0.84).

**"Five Laws" (refutable claims).** These include "No loop creates capability; every gain is imported", and "Verification, not capability, sets the frontier" (ρ(N,R) = −0.75 across 22 domains).

**Implication.**
- MetaRSI **already reports a same-start swap and a 5-term compounding curve on an open model**. Any "improver scaling / swap factorial" pitch must position against it explicitly. The gap left is not the existence of a swap test. It is:
  - generality across loop families (MetaRSI is a single bespoke system);
  - **off-diagonal improver × state grids**, which MetaRSI does not have: one S1, one fork;
  - independence of the audit;
  - a test of whether its very clean numbers replicate.
- The TB2.1 gains (+7.3 to +8.3) are large compared with FreeEvolve's TB2.1 meta-evolved gain (+1.7, Table 6) and Hyperagents' TB2.1 result in FreeEvolve (82.5 vs base 80.9). That inconsistency is itself a reproducibility target.

### 2610.21791 RSIFlow (Zhang, Chen, …, Zhou; Beihang/BAAI; v2 2 Nov 2026)

**Setup.** Task agent Qwen3-4B/8B, meta-agent GPT-5.6 Sol, R = 5 rounds, at most 2 attempts per round.

**Main results.** Overall 51.21 (4B) and 59.95 (8B), compared with the best baseline at 45.15 (SIA) and 56.04. Hyperagents as a baseline scores 41.26 and 53.57.

**Backbone sensitivity, HumanEval+ (VERIFIED).** The table gives the gain over five rounds and the total meta-agent tokens used.

| Meta-agent backbone | Gain over 5 rounds | Cumulative meta tokens |
|---|---|---|
| GPT-5.6-Sol | 43.90→68.29 (+24.39) | 252.6M |
| Claude-Sonnet-5 | +11.59 | 177.7M |
| DeepSeek-V4.1-Flash | 44.51→49.39 (+4.88) | 211.2M |

The fifth GPT-5.6-Sol round alone costs 160.8M tokens for +7.93.

**Meta-version comparison (Table 3/12).** Meta0…Meta4 act on the same initial task agent. Meta4 beats Meta0 by +20.56 on Code and +9.51 on QA. This is a single run with no repeated seeds. The authors state that equal per-task budgets "do not imply equal total evolution cost".

**Implication:** a weak but visible same-start meta-version comparison. The backbone result supports a capability-threshold story for improvers.

### 2610.09197 FreeEvolve (Kong, Hui, …; AWS AI Labs; v2 21 Oct 2026)

**Fresh-worker test (VERIFIED, Fig. 3/Table 6).**
- "To answer Q2, every skill starts from the same fresh worker and receives the same resource budget."
- The seed skill adds +0.6 on average. The meta-evolved skill adds a further **+6.9** on average: Δ = +13.8 (τ³), +3.3 (ARC-AGI-2), +8.6 (ARC-AGI-3 RHAE), +1.7 (TB2.1).
- It beats a no-data LLM rewrite in all four environments.
- Three held-out evaluation repetitions; the search runs once.

**Evolver-capability ablation on τ³-bench (Table 7, exact numbers).**

| Meta-evolution model | Evolution set | Held-out mean | Pass@3 |
|---|---|---|---|
| None | 35.0 | 33.3 | 48.3 |
| **Haiku 4.5** | **52.2** | **32.2 ± 8.7** | **48.3** |
| Sonnet 4.6 | 56.4 | 47.1 | 62.1 |
| Opus 4.6 | 48.0 | 40.2 | 55.2 |
| Opus 4.7 | 59.5 | 49.4 | 69.0 |
| Opus 4.8 | 51.5 | 51.7 | 69.0 |

Quote: "With Haiku 4.5 as the evolver, meta-evolution improves neither held-out metric. From Sonnet 4.6 onward it consistently improves held-out performance … although Sonnet 4.6 outperforms Opus 4.6."

**Correction to round 1.** Haiku *does* move the evolution-set score, by +17 points. It just does not generalize. The weak-evolver failure mode is **meta-overfitting, not inertness**. This matters for swap-factorial designs, because a weak improver produces real but non-transferring edits.

**Also verified.**
- Hyperagents as a baseline: τ³ 36.8, ARC-AGI-2 59.6, ARC-AGI-3 72.1, TB2.1 82.5.
- Frozen-harness transfer to Sonnet 4.6 and Haiku 4.5 keeps an average +5.3 for FreeEvolve.

### 2610.07979 HMED (Feng, Huang, Zhu, Zhang, Dou; CUHK/SJTU; 6 Oct 2026)

**Method.** It re-runs the incumbent and the revised Meta-Skill "from the same restored discovery state" and distills the paired traces into a Meta-Experience record.

**Main results.**
- Best in all 6 model×benchmark cells. For example, ALFWorld on Qwen3-32B: 54.11 vs MetaSkill-Evolve 51.49.
- DeepSeek-v4-Flash on BiomniBench-DA: 43.19 vs 36.80. BiomniBench is scored by DeepSeek-v4-Flash as judge.
- Three runs.

**Key number (Table 3), branch outcome vs matched Δ.**

| Benchmark | Agree | Disagree | Agreement rate |
|---|---|---|---|
| ALFWorld | 18 | 15 | 54.5% |
| ScienceWorld | 6 | 18 | 25.0% |

**Ablation.** Comparison-only scores 49.25 and full HMED 54.11, so the gain comes from feeding the consequence back, not only from selection.

**Implication.** This is published, quantitative evidence that **branch-level credit for improver revisions is near coin-flip or anti-correlated** with same-state effects. It directly supports a meta-level winner's-curse or "lineage credit is noise" thesis, and it partly scoops a "measure it" paper. What remains open is the scaling law: how the disagreement grows with search width and how much it costs in final regret.

### 2610.02616 VERSE (Wang, Ge, …, Reddy; MIT/Amazon; 2 Oct 2026)

**Observation 1 (Table 1, SWE-rebench avg@3, one evolution run per row).**

| Configuration | Score |
|---|---|
| Blank harness | 33.95 |
| Meta-Harness | 39.20 |
| Self-evolving, no verification | **31.79** (validation selected round 0) |
| Verification only | 39.81 |
| Self-evolving + verification | **41.05** |

**Main result.** VERSE improves all four host optimizers: Meta-Harness, AHE, Self-Harness and HarnessX. The best harness reaches 42.3% in-distribution and 37.7% OOD, against 39.2% and 29.3% for the strongest baselines.

**Models.** Optimizer Qwen3.8-Flash-Next (176B total, 6B active), executor Qwen3.8-27B. This is open-weight; an appendix repeats the study with anonymized closed models.

**Self-reported cost caveat.** Comparisons "match candidate count, proposal-turn limits, and the evaluation protocol, but not total execution or token cost."

**Implication.** Evidence that improver self-modification *without* verification hurts. The untested follow-up is whether verification degrades over time.

### 2610.20563 Scaling Discovery Requires Forgetting (Tan, Zhou, Agrawal, …, Dimakis, Zaharia; Berkeley; 14 Oct 2026)

**All round-1 numbers VERIFIED.**
- Up to 15% over parallel agents and up to 39% over a single agent, at matched dollars.
- In the team, edit size falls "from about 150 lines to 30 and reasoning from about 28k tokens to 7k."
- Forgetting vs continuing from the same plateaued state: +9.2% vs +1.4% for the team, at $15.32 vs $24.67.
- Single-agent VLIW: "532 of 763 completed turns produce no candidate … ending its turn by declaring a plateau."

**Models and tasks.** DeepSeek V4.1 Flash, Pi agent, 3 replicates, 4 tasks: AHC014, Frontier-CS Q171, VLIW, nanochat. Budgets are $20–40 per replicate. The authors' stated next step: "A natural next step is to decide when to forget and what to remember."

**Implication.** Very cheap to replicate. The "what to remember" factorial is explicitly open, and 3b names the confound. Neither paper applies this to a *meta-agent* or improver.

### 2611.12865 Shortcuts and Boundaries (Xinjie Shen; single author; 9 Nov 2026)

**Setup.** Weight-level GRPO on Qwen3.5-4B in generated environments with certified reward–context conflicts.

**Results.**
- With a 25% conflict mixture, certified profitable violations reach 97.9% on the API task after 128 updates.
- With the safety penalty, safe completion on 32 unseen changing-evidence tasks rises from 53.1% to 89.1%.

**External transfer (verified, but small).**
- **BaitBench:** two submissions leak test labels.
- **HVTA:** a "25-task illustrative subset". Native hack flags appear in 32% of Base runs vs 68% of Task-trained runs; "These statistics describe the illustrated cases only."

**Implication.** The weight-level "hacking transfers" claim exists but rests on anecdotes. A *harness or improver-level* analogue (does an evolved harness learn generic exploit-seeking?) is not addressed.

### 2611.08318 Learning the Task or Learning the Verifier? (Huang, …, Daquan Zhou; PKU/NUS; 5 Nov 2026)

**Setup.** Actor GPT-6 Astra (frozen); learner Claude Code with Opus 5.5. RoboDojo, 25 tasks, of which 16 are criteria-limited. Five rounds; 20 test layouts per task.

**Results.**
- Under the original criteria: success-only 27.8 vs exposed 51.6.
- Under instruction-faithful criteria, the same episodes score 47.2 vs 45.0. "the gap shrinks by 25.9 points (95% bootstrap interval over tasks 4.7–54.4)".
- In the swap, the library trained on faithful criteria scores 65.0 under them but only 13.5 under the original criteria.

**Cross-benchmark result.** "neither the exposed nor the success-only library gives a clear gain over the empty library; only experience gathered within the target benchmark does."

**Implication.** Shows that verifier-reading turns criteria–instruction gaps into score in embodied self-improvement. The same design, run in *software or harness* self-improvement (exposing test code or grader source to the improver), is an open, cheap transfer.

### 2608.12253 One Frozen Simulator Is Not Enough (Yu, Tomlin, Abdulhai, Lu, Chong, Hou, Soylu, Levine, Manning, Shi; v2 17 Aug 2026)

**Setup.** RL (GRPO/REINFORCE) of Qwen3-4B-Instruct or 8B against one frozen LLM user simulator.

**Results.**
- "Training reward climbs in every run (a), OOD eval peaks early and declines (b), and policy entropy collapses (c)."
- The most modal simulator (Gemini-3-Flash) "crashes below the untrained baseline". GPT-5-mini shows "a gentler but still clear decline."
- Fixes: Verbalized Sampling and Population Co-Training. On τ²-Retail at 4B: RL-Single 46.1, VS 55.5, PopCoTrain 62.2.
- Human study, N = 40 per cell.

**Implication.** Weight-level only. A harness-level version is untested: does an *evolved harness or prompt* overfit to a single frozen user simulator in the same way? That makes it a plausible non-obvious transfer.

### 2611.01139 World Models Need Not Be Right to Rank Policies Right (Ariq, Ahmad; 31 Oct 2026)

**Results.**
- At horizon 500, Kendall τ is 0.962 (cheetah-run) and 0.800 (walker-walk).
- Paired audit-error correlation is only 0.007 and 0.090, so pairing gives essentially no variance reduction.
- **Model-blind collapse:** after 500 gradient steps against the frozen model, real-return losses are 22.43 and 28.50, while model-return changes are also negative (−4.56, −17.45). "degradation in the environment is substantially larger than, and sometimes invisible to, the model."
- The precommitted 2× audit-saving target (H2) fails.

**Scope.** Two DMC tasks, 45 policies per task. Not LLM agents.

**Implication.** Small but clean. The phrase "ranking fidelity ≠ audit utility ≠ exploitation" can be borrowed for LLM simulators used to rank harnesses.

### 2611.13034 RoboACE (Zhao, Yue, He, Yali Wang; SIAT; 9 Nov 2026)

**Setup.** GPT-6.1 Terra, SAM 3, frozen π0.5. Alternating curriculum-harness and execution-harness updates with a Strategic Advisor. 27 saved rounds.

**Results.** LIBERO-PRO 74.0 (+15.0 over the initial deployment, +13.5 over π0.5); LIBERO-X 30.5.

**Weaknesses (verified).**
- It is a single campaign: "Establishing its behavior across independent campaigns … is the next empirical step."
- The initial deployment scores 63.5 on LIBERO, against 96.9 for bare π0.5. Part of the "gain" therefore repairs a weak starting harness.
- No measurement of curriculum Goodhart or proxy-vs-truth drift. The internal benchmark is curated by the system itself (a "Benchmark Curator").

**Implication.** The method claim for curriculum–execution co-evolution is taken. The measurement claim is open: does the self-curated curriculum drift from the true distribution?

---

## 5. Where the field is moving, and non-obvious openings (from these reads)

**Converging trends (expect fast follows):**
1. **Same-state paired comparisons are becoming standard.** Examples: HMED's restored-state re-runs, MetaRSI's forked-S1 swap, FreeEvolve's fresh worker, RSIFlow's same-initial Meta versions, and 2610.27610's C4/C5 standard. A paper whose only contribution is "we do a swap test" will look incremental by mid-2027.
2. **Improver capability thresholds.** Examples: FreeEvolve (no Haiku gain, Sonnet+ works), RSIFlow (GPT +24 vs DeepSeek +5), 2609.17817 (Hyperagents needs Sonnet) and VERSE (open models work with verification).
3. **Memory and forgetting for long-running searchers.** Forgetting (Berkeley) vs the verified archive (QWNTL) is an active debate, and Fork-and-Flush 2610.07447 and Stateless Language Agents 2610.07625 are cited in both.
4. **Verifier exposure and evaluator integrity.** Examples: 2611.08318, 2611.12865, BenchShield, and the git-leak in 2609.17817.

**Non-obvious, still-open angles that these papers expose but do not run:**
- **Weak improvers overfit rather than stall.** Haiku's evolution-set score jumps 35.0→52.2 while held-out stays at 33.3→32.2. A "meta-overfitting gap vs improver capability" curve across 4–5 model tiers would be new. FreeEvolve shows a single point, and it is non-monotone (Opus 4.6 < Sonnet 4.6).
- **Branch credit for improver revisions is mostly noise.** HMED disagreement is 45–75%. No paper relates this to search width, number of children or selection regret. That is a meta-level optimizer's-curse law.
- **Scaffold open-endedness × capability.** The 2609.17817 anecdote (DGM works on Qwen3.5-397B, SICA works, Hyperagents fails) suggests that the *specificity of the meta prompt* substitutes for model capability. A controlled sweep on cheap open models is untested.
- **Forgetting vs archive, factorially.** Both papers name the confound: conversation vs scored attempts vs tools. Neither applies it to an *improver or meta-agent* that edits another agent's harness. DeepSeek V4.1 Flash at $20–40 per run makes this feasible.
- **Harness-level simulator collapse.** Single-frozen-simulator overfitting is shown for weights (2608.12253), not for evolved prompts or harnesses (e.g. τ²-bench harness evolution against one user simulator).
- **Criteria exposure for software or harness evolvers.** The RoboDojo C_orig/C_faith design ports directly to coding benchmarks where the improver can read test files.
- **Reproducibility of TB2.1 self-improvement.** MetaRSI reports +7.3 to +8.3; FreeEvolve reports +1.7 (meta) and Hyperagents +1.6. The field's numbers on the same benchmark disagree by about 5×.
- **SelfImprove-Bench's zero-meta winner.** The best agent there retained no improver edits. Asking when improvers spontaneously self-modify, and whether it helps when they do, is unmeasured beyond one table.
- **Polyglot git-leak audit** of the published DGM and Hyperagents curves.

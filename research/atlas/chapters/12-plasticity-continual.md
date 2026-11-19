## Agent plasticity, continual learning & forgetting

Verification marks used below:
- ✓ means the number or claim was checked in the paper's full text, either in this session or earlier in FULLTEXT_VERIFICATION.md. The section or table is cited.
- ◐ means it comes from the abstract only. For ICLR-27 OpenReview-only papers this is always the case, because their PDFs are behind a bot check.
- † means it is unverified, from memory or from earlier project notes.

Local full texts are at `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/<id>/paper.txt`. The prior sweep this chapter builds on is `/home/user/nlp-lab-paper-search/research/round1/sweep/plasticity.md`.

### At a glance

- **What this corner is.** It asks whether agents that keep state across tasks get better at *new* tasks over time (plasticity, forward transfer), and whether they keep what they had (stability, backward transfer). The state can be session context, memory, skills, prompts, harness code or weights.
  - The weight-level ancestors are loss of plasticity (Dohare et al. 2024 Nature†; Lyle et al. 2023†; Nikishin et al. 2022 primacy bias†) and catastrophic forgetting.
  - The 2026 twist is that most "learning" happens in frozen-weight harnesses. The surveys classify continual learning as a neighbouring paradigm without "mechanism revision/reuse" (Duan 2609.11873, Table 1, per digest ✓). They note that scaffold adaptation "does not eliminate forgetting" and adds memory poisoning, tool-semantic drift and template brittleness (Ren 2607.13104 §3.3, per digest ✓).
- **State of the art in measurement.**
  - Gain is measured against a stateless or reset twin: CL-Bench gain g_t = r_sf − r_sl (2606.05661 §4.2 ✓); Continual AutoResearch persistent vs reset; Continual-ARC "share saved vs reset".
  - Plasticity is measured as gain per $: P_sat (Agent Plasticity 2610.08902 §2.2 ✓).
  - The CL-style split is plasticity P / stability S = −(I+F) (2610.25858 Eq. 9–11 ✓).
- **Consensus 1: frozen-weight self-evolution is a weak continual learner.**
  - Self-modifying methods that edit a shared global prompt or workspace trade stability for plasticity (r = −0.72, p = 0.008, across n = 12 method×stream cells, 2610.25858 Fig. 2 ✓).
  - Continual harness evolution on SE tasks shows catastrophic forgetting: SWE-bench subset 94.0% → 32.0% (2611.03219 abstract ✓).
  - Persistence speeds research but does not improve final quality (2611.05108 §5.2–5.3 ✓).
  - On ARC, harnesses as shipped do not beat a demo cache and give ~0 on unseen tasks, while weight updates do generalize (2611.14505 §4, Table 1 ✓). With expert guidance, GLM-5.3 Flash's notes do beat the demo cache on seen tasks (S_seen 0.74 vs 0.44), but still save nothing measurable on unseen ones.
- **Consensus 2: accumulated state can become net negative.**
  - Session anchoring stalls search. Discarding the session beats continuing: +9.2% vs +1.4% at $15.32 vs $24.67 (2610.20563 Table 3 ✓).
  - Consolidated memory degrades below the no-memory baseline (2605.12978 abstract, §4 ✓).
  - Stale experience after a silent tool change: 23% success vs 88–100% for agents without experience (2611.20922 §1, §4 ✓).
  - Marginal value of refinement and memory diminishes, so self-improvement should be governed (c3lHKtsSMq ◐).
- **Plasticity is a protocol × model property, not a model scalar.**
  - The agent that ends best is not the one that learns most efficiently. GPT-5.6 Sol has P_sat 298 vs Claude Fable 5 at 57, but Fable has the highest fitted endpoint (2610.08902 §3.4 ✓).
  - Weaker models gain more from persistence (2611.05108 §5.3 ✓).
  - There is a capability floor below which harness refinement hurts (Flash-Lite, 2605.09998 §4.4 ✓).
- **Experience-learning can be trained into weights and transfers.**
  - MetaExp: +8.5 on SpreadsheetBench while the matched backbone loses 2.2 under the same protocol, and +9.3 under strict-OOD training (2610.21492 Tables 1–2 ✓).
  - This undercuts "plasticity is fixed by the base model".
- **Biggest open problem: nobody has measured *forward* loss of plasticity.** The question is whether an agent or improver that has already self-improved for k rounds learns a *new* family worse than a fresh one at matched budget.
  - It is untested at both the actor and the improver (meta) level (judgment, from the papers read; not an exhaustive search).
  - The only evidence is incidental:
    - Harness transfer finishes below a cold start in 3 of 4 cross-model domains (2611.18856 §4.3.1 ✓).
    - Flex-RSI warm start 49.2 vs 69.0 own (2611.08946 Table 9 ✓).
    - The Meta-Harness gate "freezing the workspace" (2610.25858 §4.2 ✓).
  - Agent Plasticity explicitly sets its usage apart from the loss of plasticity studied in continual neural-network training, and never measures plasticity after prior learning (2610.08902 appendix ✓).

### How it developed

| Month | Paper | What changed |
|---|---|---|
| 2022–24 | Nikishin primacy bias (2205.07802)†; Lyle "Understanding plasticity" (2303.01486)†; Dohare et al. Nature 2024† | Weight-level LoP established; remedies: resets, continual backprop, shrink-and-perturb |
| 2025 | StreamBench (2406.08747)†, LifelongAgentBench (2505.11942)†, EvaLearn (2506.02672)† | First "agent learns over a stream" benchmarks (memory-centric) |
| May 2026 | Do SE Agents Forget? 2605.09315; Useful Memories Become Faulty 2605.12978; Continual Harness 2605.09998 | "Capability erosion" across workflow/skill/model/memory evolution; consolidation rot; reset-free embodied harness self-refinement |
| Jun 2026 | Continual Learning Bench 2606.05661; AgentCL 2606.02461†; Zyphra LoP-at-scale 2606.24752; Rejuvenation 2606.09932 | Gain vs stateless twin; naive full-context ICL beats memory systems; LoP onset follows a sublinear power law in LMs; LoP framing enters SFT→RL |
| Aug 2026 | Harness Continual Learning 2608.19013; PATH-Bench 2608.01149† | "Harness-level forgetting" named; guarded commit with a retention check |
| Sep 2026 | Learning Dynamics of CL (Oxford/GDM) 2609.33620◐; Steady-state BIRD 2609.31640† | Unified weight-level theory; short-horizon learning metrics mislead |
| Oct 6–9 | Agent Plasticity 2610.08902 (Meta MSL; ICLR27 Jjh60xpFVY); EmbodiedCLBench 2610.13021; ReHarness 2610.12668 | "Agent plasticity" coined (gain per $); embodied compositional CL; backward revision of harness state |
| Oct 14–16 | Scaling Discovery Requires Forgetting 2610.20563; MetaExp 2610.21492 | Deliberate forgetting (session reset) as a feature; experience-learning trained into weights |
| Oct 19–25 | Are SE Agents CL? 2610.25858 (Mila/GDM); HERD 2610.31401; CLE 2610.33956 | P/S/I/F decomposition for harness evolvers, r = −0.72; harness→weights distillation; 600-level professional CL ladder |
| Nov 1–5 | Do agents need to sleep? 2611.02216; coding-agent continual forgetting 2611.03219; Continual AutoResearch 2611.05108; Beneath the Surface 2611.07624 | Sleep and selectionist consolidation prediction; SE harness forgetting up to 100% from a single injected conflict type; persistent vs reset research agents |
| Nov 6–10 | Flex-RSI 2611.08946; Third Eye 2611.12988; Continual-ARC 2611.14505; FGSE 2611.14326 | Warm-start penalty (incidental); 2-step degradation of self-updates; memory and weight learners on one scale; falsification-gated memory commits |
| Nov 13–17 | Self-Improvement Memory / CRSI 2611.18856 (Google); Where should agents adapt 2611.19700 (Apple); Experience Rot 2611.20922; Skills Go Stale 2611.22525 | Continual RSI of the *optimizer*; ICL vs GRPO vs SDPO under one stream; stale experience under silent drift |
| ICLR-27 subs (Oct–Nov) | TenureBench f4TzyqXvHT; Seven-Year Itch 2e5z57jIPK; TCF-Bench udjtWhlqMB; SkillFlow j2RMOsmge4; Governing Timescales c3lHKtsSMq; gradual-change LoP hip2PfZSRg | Decade-long replay where agents get *worse* over a lifetime; memory authority vs recency; textual forgetting in skill libraries |

### Taxonomy

The corner splits along two axes:
- (i) **which direction of learning is measured:** forward (plasticity/acquisition), backward (forgetting/stability), or state pathologies;
- (ii) **which substrate carries the state:** session context, memory store, skill library, global prompt/harness code, the optimizer's own memory, or weights.

The organizing questions follow the Dai/Ruan survey (alphaxiv 2611.recursive-self-improvement-llm-agents; digest ✓): "When / Where / How". That survey's distinction between "persistent experience" and "improver update" maps onto actor-level vs improver-level plasticity.

| Approach / lens | Representative papers | Note |
|---|---|---|
| **A. Measuring forward learning (actor)**: gain vs stateless or reset twin | CL-Bench 2606.05661; Continual AutoResearch 2611.05108; Continual-ARC 2611.14505; Agent Plasticity 2610.08902 (gain/$); EmbodiedCLBench 2610.13021; CLE 2610.33956; Learn2Play 2610.08215†; ServeLearnBench 2610.07792†; SkillFlow j2RMOsmge4◐; TenureBench f4TzyqXvHT◐; SCLATE ionMcwU5xU◐ | Crowded; all measure the actor's learning from a seed, none after prior learning |
| **B. Backward forgetting in harness/prompt evolution** | Are SE Agents CL 2610.25858; 2611.03219; Do SE Agents Forget 2605.09315; HCL 2608.19013; REpertoires 2610.11373†; Beneath the Surface 2611.07624; TCF-Bench udjtWhlqMB◐ | Cause = global shared state + conflicting workflows; well covered |
| **C1. Session/context anchoring** | Scaling Discovery Requires Forgetting 2610.20563; Verified-archive rebuttal (alphaxiv 2611.verified-archives-multi-agent-search; factcheck ✓); Fork-and-Flush 2610.07447†; Stateless Language Agents 2610.07625† | Within-run plasticity loss of the *context*; remedy = reset + keep best artifact |
| **C2. Memory consolidation and accumulation pathologies** | Useful Memories Become Faulty 2605.12978; Do agents need to sleep? 2611.02216; Let Them Sleep 2610.25561†; Memory Tax 2610.34073†; Seven-Year Itch 2e5z57jIPK◐; Demotion Beats Deletion hrBgrRlbjW◐; Governing Timescales c3lHKtsSMq◐ | Utility rises then falls; "unchecked plasticity" |
| **C3. Stale experience under environment drift** | Experience Rot 2611.20922; Skills Go Stale 2611.22525; EvoHarnessBench 2609.04280†; ReScopeBench 2611.05450◐ | Old state actively harms after silent change |
| **D1. Remedy: localize updates** | ReasoningBank (in 2610.25858); ReHarness 2610.12668; AGSE Pe4LarfEaD◐ | Retrieval localizes but collides on similar queries (2610.25858 §4.4 ✓) |
| **D2. Remedy: gate commits on retention/falsification** | HCL guarded evolution 2608.19013; FGSE 2611.14326; CPE 2605.09315; Masked Erosion 2610.38392 (gates erode safeguards) | Gate strictness drives the freeze-vs-forget dial; never studied as a plasticity knob |
| **D3. Remedy: deliberate forgetting / reset / graded forgetting** | 2610.20563; hrBgrRlbjW◐; ACTF rcXmJY7QVs◐ | Classical LoP remedies (resets, shrink-and-perturb, continual backprop) not ported to harness state |
| **D4. Remedy: optimizer-side memory for continual RSI** | SIM/CRSI 2611.18856 | First improver-level continual setting; positive transfer only with provenance memory |
| **D5. Remedy: train experience-learning into weights / distil harness→weights** | MetaExp 2610.21492; HERD 2610.31401; Continual Harness co-learning 2605.09998; Continual-ARC DAgger arm; 2611.19700 (ICL+GRPO); Meta-TTL nw2tokNgb2◐ | "Plasticity as a post-training target" (Agent Plasticity §4 future work ✓) is already being done |
| **E. Weight-level LoP background** | Zyphra 2606.24752 (ICLR27 0H9Qwt6mOj); Rejuvenation 2606.09932 (CC4ksJJPIN◐); gradual-change LoP hip2PfZSRg◐; Learning Dynamics 2609.33620◐; Third Eye 2611.12988 | LoP in LMs exists and is delayed, not removed, by scale |

### Paper cards

**1. Agent Plasticity: Measuring Self-Improvement Through Experience** ([2610.08902](https://arxiv.org/abs/2610.08902), 2026-10-06; ICLR-27 submission [Jjh60xpFVY](https://openreview.net/forum?id=Jjh60xpFVY); Berkeley + Meta MSL: Singh, Bakhtin, Synnaeve, Weston, Mahajan, Goyal…)
- *What it does.* Defines agent plasticity as the efficiency with which a frozen-weight agent turns experience into held-out gains. The agent works through inherited artifacts (tools, skills, strategies, memory) that the same model revises between checkpoints.
- *Setup.* (§3.1 ✓)
  - Environments: chess (Hard/Easy), 5×5 Go, 6×6 Hex, NetHack (40 checkpoints).
  - Models: Claude Fable 5, Opus 5/5.5/4.8/4.6, Sonnet 5, GPT-5.5, GPT-5.6 Sol/Luna, Gemini 3.1 Pro.
  - Revisions must pass structural and executable validation (degrading revisions are kept, FULLTEXT_VERIFICATION ✓).
  - Per checkpoint in Hard chess: 16 training, 12 held-out ID and 12 held-out OOD games (§3.1 ✓).
  - Metric: P_sat is a saturation-curve fit, giving gain up to the 90%-rise point per $1k. P_sat is set to 0 if the fitted rise is under 10 pp (§2.2 ✓).
- *Key results.*
  - Hard chess (§3.2 ✓): Fable 5 goes 37.5→73.3% and Opus 5 25.0→66.4% (checkpoints 16–20). GPT-5.6 Sol goes ~0→36.9%.
  - Combined board games (§3.4 ✓): P_sat is 298 (Sol), 233 (Opus 5), 57 (Fable 5), 14 (Opus 4.8) and 0 (Luna). GPT-5.5 has not saturated, so it has no P_sat. Endpoint and efficiency diverge: Fable 5 reaches the highest fitted performance level.
  - NetHack (§3.3 ✓): Opus 5.5 goes ~2k→6k with P_sat 61.75. Sol declines (~570→420). The other five models show no reliable rise.
  - Reuse (§3.5 ✓): Luna makes ~98% of relevant chess decisions without using an artifact. Strong improvers (Fable 5, Opus 5, Opus 4.8, Sol) reuse at 94–98%, yet 83–99% of their remaining failures occur *while* using a relevant artifact. Reuse can rise to 93–99% (Sol, Opus 4.8, by checkpoint 3) before the score peaks (checkpoints 12 and 5).
- *Limitations / critiques.*
  - Self-stated in §4 ✓:
    - experience is not separated from extra reflection compute (they propose budget-matched no-feedback reflection);
    - artifact diagnostics are observational (they propose removal, replacement or forced invocation);
    - separately trained lineages do not establish transfer of artifacts or "improvement strategies";
    - P depends on headroom, horizon, fit and pricing;
    - plasticity as a post-training target is future work.
  - Other critiques:
    - Actor and reflector are the same model.
    - Closed models only.
    - Small per-checkpoint game counts (12 held-out ID games per checkpoint in Hard chess, §3.1 ✓).
    - It explicitly sets aside the Dohare/Lyle LoP reading (appendix ✓), so it is never measured after prior learning.
    - 2611.11342 (failure traces vs matched search) partially runs the no-feedback control: about 60% of the gain comes from search alone (◐, r1).

**2. Are Self-Evolving Agents Continual Learners?** ([2610.25858](https://arxiv.org/abs/2610.25858), 2026-10-19; Mila + Google DeepMind; preprint)
- *What it does.* Tests ReasoningBank (retrieval memory), GEPA (shared prompt rewrite) and Meta-Harness (coding-agent workspace edits behind a validation gate) on four blocked drift streams. Decomposes outcomes into plasticity P, stability S = −(I+F), and terminal/average advantage over the seed.
- *Setup.* (§3.1, §4.1 ✓)
  - Streams built from BIRD/BFCL/ScienceWorld: topic (T=150), workflow, domain (T=150, K=3) and compound (T=200, K=4).
  - Single model: Gemini 3.6 Flash in a ReAct loop.
  - Binary feedback; periodic checkpoints (every 50 positions in the reported figures); 8 rollouts per hold-out instance.
- *Key results.*
  - Overall terminal advantage (Table 1, §4.2 ✓): GEPA +3.7, ReasoningBank +2.5, Meta-Harness +1.4.
  - Plasticity: GEPA has the highest P (+6.2) and worst S (−4.3); ReasoningBank has the best S (−1.9) at about half the P (+3.3).
  - P–S correlation r = −0.72 (p = 0.008, n = 12) (Fig. 2 ✓).
  - Meta-Harness looks stable on topic and workflow drift "only because its validation gate rejects half or more of the candidate updates, freezing the workspace" (§4.2 ✓).
  - GEPA displaces old rules. On topic drift, BIRD-F1's share of the prompt goes 100%→75.2%→40.2%, and the BIRD-F1 gain goes +7.5→+3.5→−2.0 (§4.3, Fig. 3 ✓).
  - Interleaving does not fix it: GEPA terminal advantage drops from +5.8 (blocked) to +0.9 (Mix) (§4.3, Appendix A ✓).
  - Accepted edits reversed the clarification behaviour on BFCL-Miss: under GEPA on workflow drift the ask rate fell 29.8→11.2%, and under Meta-Harness on compound drift 60.8→8.5% (§4.3 ✓).
- *Limitations.*
  - One model; no repeated stream seeds in the main table.
  - Editable surface is confounded with method.
  - Semi-synthetic drift.
  - P is within-block only; there is no reset-agent comparison of *later-block* learning speed, i.e. no forward-LoP test.

**3. Scaling Discovery Requires Forgetting** ([2610.20563](https://arxiv.org/abs/2610.20563), 2026-10-14; UC Berkeley: Tan, …, Dimakis, Zaharia; preprint)
- *What it does.* Autoresearch agents plateau because the growing session anchors them. "Forgetting" discards session and search state at round ends and keeps only the best candidate plus its score (§3.1 ✓). It is combined with a 4-agent team that shares candidates as commits.
- *Setup.* (§4, Appendix A ✓)
  - Tasks: AHC014, Frontier-CS Q171, the Anthropic VLIW kernel, nanochat-autoresearch.
  - Pi agent with DeepSeek V4.1 Flash.
  - 3 runs per configuration at the same total token cost/money; per-replicate budgets of $20 (AHC014, Frontier-CS, nanochat) to $40 (VLIW).
- *Key results.*
  - Team with forgetting is best on all four tasks (Table 1 ✓). AHC014 mean is +39% over a single agent; VLIW is −23% cycles.
  - Most gain comes from the first forget: AHC014 +11.2%, then +4.8%, then +1.7% (Table 2 ✓).
  - From the same plateaued state, the team gains +9.2% ± 6.9 with forgetting ($15.32) vs +1.4% ± 1.3 continuing ($24.67) (Table 3 ✓).
  - Mechanism (§5 ✓): edits shrink ~150→30 lines and reasoning ~28k→7k tokens before a forget. On VLIW, 532 of 763 single-agent turns produce no candidate, consuming 66.2% of spend.
- *Limitations / critiques.*
  - The forget schedule is fixed. "When to forget and what to remember" is stated as next step (§6 ✓).
  - Object-level only; the conservatism mechanism is correlational (FULLTEXT_VERIFICATION ✓).
  - Cumes (alphaXiv-only rebuttal, prior factcheck ✓): forgetting "deletes two things this comparison never separates: the scored attempts and each agent's conversation." A verified shared archive (914 cycles median) matches or beats the CORAL-port variants. In that study 7 of 12 CORAL-port agents fell into a "false floor" loop. Caveat: 1 task, 3 runs.

**4. Self-Improvement Memory Enables Continual Recursive Self-Improvement** ([2611.18856](https://arxiv.org/abs/2611.18856), 2026-11-13; Google Cloud AI / GDM / PSU; preprint)
- *What it does.* Defines Continual RSI (CRSI): harness optimization resumes after the benchmark or the policy model changes. Adds SIM to the *optimizer*: edits grouped by root cause, with rationale, predicted vs observed outcomes and provenance. Compared against Cold Start, Harness Transfer, Problem-Only Memory and Full SIM (§3.3 ✓).
- *Setup.* (§4.1 ✓)
  - Four domains: TB2.1→TB4; 3→8 classification sets; Harvey LAB→JobBench/GDPval/APEX-Agents; EngDesign→Frontier-Eng.
  - Gemini 3.8 Flash is the policy, proposer and selector. The cross-model setting goes Claude Opus 5.5→Gemini 3.8 Flash.
  - N = 10 rounds, k = 5 candidates per round, 5 rollouts per task.
  - One complete RSI run per condition except a ten-pair coding study.
- *Key results.*
  - SIM vs harness-only continuation: +2.36 mean (95% CI [1.05, 3.68], paired p = 0.003; SIM higher in 9 of 10 pairs; §4.4, Table 3 ✓).
  - Full SIM is best in all six cross-dataset settings, +2.43–4.35 over Harness Transfer (§4.2 ✓).
  - **Warm-start penalty** (§4.3.1 ✓):
    - The Opus-evolved harness run on Gemini starts *below Gemini's seed* in every domain (−0.60 to −5.48).
    - "Harness Transfer still finishes below Cold Start in three domains."
    - Full SIM recovers it: workspace 68.41→84.75 vs 73.42 for Harness Transfer.
  - Cross-dataset: the FewShotAll evolved harness starts below seed (72.92 vs 75.72), and harness-only continuation stays below Cold Start (74.12 vs 75.92) (§4.2 ✓).
  - Generation chain (Table 1, §4.3.2 ✓): CRSI beats fresh RSI on 3.5 Flash (+3.5) and 3.8 Flash (+1.6), i.e. 2 of the 3 subsequent generations, but finishes 0.8 lower on Gemini 4 Argon (94.8 vs 95.6).
- *Limitations.*
  - Single runs outside coding; compatible interfaces only (Limitations ✓).
  - The cross-model warm-start penalty confounds model mismatch with accumulated-state rigidity.
  - Only one transition; no k-step dose–response.

**5. From AutoResearch to Continual AutoResearch: Do Agents Learn Across Tasks?** ([2611.05108](https://arxiv.org/abs/2611.05108), 2026-11-03; RBC Borealis / Tübingen; preprint)
- *What it does.* Compares persistent vs reset instances of the same CLI agent on matched task streams. Adds a memory ablation using agent-written "insight cards": matched vs shuffled vs none.
- *Setup.* (§5.1 ✓)
  - Agents: Opus 5 and Sonnet 4.6 (Claude Code), GPT-5.4 and GPT-5.6-Sol (Codex), Kimi K3 (OpenCode).
  - 9 task families, 3–5 tasks each, 30 min per task by default, 32 transfer tasks per agent.
  - A single matched pair per agent, except 3 repeats for Sol. Across those repeats, outcome and initialization gains varied by at most 0.75 points and time-convergence gains by at most 7.14.
- *Key results.*
  - Time-convergence gain is positive for all five agents: +21.3 (Kimi), +20.9 (Sonnet), +7.5 (Opus), +6.1 (Sol), +3.0 (GPT-5.4) (§5.2, Table 2 ✓).
  - The persistent-minus-reset lead shrinks from +24.0 at 25% budget to +1.2 at the deadline.
  - Final outcome gain is only substantial for Sonnet (+7.6); Opus is −0.9 and Sol −0.8 (§5.3 ✓).
  - After subtracting first-valid reward, the gain is negative for every agent (§5.2, App. B.1 ✓).
  - Matched cards lift initialization vs shuffled cards (+2.6 to +5.9 for Opus, GPT-5.4 and Sol) but *reduce* time-convergence for those 3 agents. Highly specific cards anchor Sonnet to a weaker model family (§5.5 ✓).
  - For Sol on Shared Regime, the final outcome gain shrinks as the per-task budget grows from 15 to 60 min, while the initialization gain grows (§5.4 ✓).
- *Limitations.* (§6.3 ✓) Model is confounded with scaffold; streams are 3–5 tasks long; the ablation covers one stream. The authors call for separating insights, code, transcripts and execution state, and for explicit forgetting.

**6. Can Coding Agents Sustain Continual Evolution without Forgetting?** ([2611.03219](https://arxiv.org/abs/2611.03219), 2026-11-02; Tsinghua; preprint)
- *What it does.* Runs AHE and Meta-Harness along six continual evolution directions (ordered pairs) over SWE-bench Verified, SWT-Bench and FeatureBench, starting from a Mini-SWE-Agent seed. Categorizes the harness conflicts that cause pass→fail transitions.
- *Setup.* DeepSeek-V4-Flash-0731 for both execution and evolution; 50/50/30 sampled instances (§3.1 ✓). Following their original papers, AHE runs 9 evolution rounds and Meta-Harness 5; forgetting is read after five rounds of continual evolution (§3.4, §4.1 ✓).
- *Key results.*
  - Meta-Harness reaches 94.0% on SWE-bench and then falls to 32.0% after FeatureBench evolution, a 66% relative drop (abstract ✓). SWE→SWT is worse: AHE and Meta-Harness lose 100% and 98% of SWE performance (§4.1 ✓).
  - Of the pass→fail cases found among 520 analyzed trajectories, 87.1% are verification conflicts, 8.2% workflow conflicts and 0.6% budget conflicts; the rest have no specific type (§4.2.1 ✓).
  - Injecting one verification conflict into the best harness drops SWE from 96.0% to 0.0% (Table 1 ✓). A workflow conflict drops it to 66.0%.
- *Limitations.* One model, small subsets, temperature 0 with no seeds. The SWE score is measured on the sampled evolution subset. Forgetting only; plasticity is not measured.

**7. MetaExp: Learning to Self-Improve through Experience via Verifiable Cross-Task Environment Scaling** ([2610.21492](https://arxiv.org/abs/2610.21492), 2026-10-15; Alibaba Qwen / Macau / Georgia Tech; preprint)
- *What it does.* Fine-tunes a model on sequential cross-task trajectories from 1,000 synthetic families, so that frozen-weight experience accumulation *after* deployment works better. Uses Shared-Tool and Shared-Workflow families.
- *Setup.* Qwen3.5-122B-A10B trained on 4,956 filtered trajectories with a GLM-5.2 teacher (§5.1 ✓). Cold-start sequential evolution (SeqEvo) to k = 40 on SpreadsheetBench, APEX-Agents and AutomationBench.
- *Key results.*
  - SpreadsheetBench: 65.0→73.5 (+8.5) vs the matched backbone 68.9→66.7 (−2.2) (Table 1 ✓). Qwen3.6-Plus also ends below k=0 (−1.7).
  - Strict-OOD training gives +9.3 at k=40 (62.7→72.0; Table 2, §5.3 ✓). AutomationBench gives +14.8 (abstract ✓).
- *Limitations / critiques.*
  - The trained model starts *lower* at k=0 (65.0 vs 68.9), so the net endpoint gain over the backbone is about +6.8 (73.5 vs 66.7), and +4.6 over the backbone's own k=0 score.
  - No seeds over training. A single teacher.
  - Actor-level only. Medium threat to "plasticity is a fixed model property" framings (leads-b ✓).

**8. Continual Learning Bench (CL-Bench)** ([2606.05661](https://arxiv.org/abs/2606.05661), 2026-06-04; Berkeley / Snorkel / UW-Madison; preprint)
- *What it does.* Six expert-validated stateful domains with learnable latent structure: codebase adaptation, spectrum monitoring, cohort studies, database exploration, exploitable poker and sales prediction. The gain metric is stateful minus the same system stateless (§4.2 ✓).
- *Key results.*
  - Full-context ICL with Sonnet 4.6 has the highest normalized reward (22.3%) and gain (25.4%) (Table 1 ✓).
  - ACE ranks 10th by gain (8.6%) at the highest cost ($62.8).
  - Mem0 + GPT-5.4 has gain 20.2.
  - Gemini 3.1 Pro ICL normalized reward is −5.6.
- *Limitations.* (§6 ✓) Tens of instances per sequence; no parametric methods; frontier-only.
  - Its "ICL is best" result *contrasts* with Forgetting's "session accumulation stalls search". The horizons differ: tens of instances here vs hundreds of turns there.

**9. Continual-ARC: Measuring Skill Acquisition over a Working Lifetime** ([2611.14505](https://arxiv.org/abs/2611.14505), 2026-11-10, v3 Nov 15; Unify; under review)
- *What it does.* A lifelong ARC stream in which tasks recur after dormancy and new ones arrive. Every action draws on a feedback budget. After the stream the learner is frozen and tested on seen tasks (memorization) and unseen tasks (generalization). Memory-based and weight-based learners are scored on the same "share saved vs reset" scale.
- *Key results.* (§4, Table 1 ✓)
  - Demo-cache floors save S_seen = 0.37 / 0.44 and S_held ≈ 0 (−0.01 / −0.02).
  - Program libraries save S_seen 0.65 / 0.81 but S_held only 0.03 / 0.05.
  - Harnesses as shipped (Q, G) do not beat their demo-cache floor. Qwen's persistence saves −0.01 on both tests; GLM's saves a little on seen tasks.
  - With expert guidance, GLM-5.3 Flash's notes reach S_seen 0.74 [0.64, 0.84], above its demo-cache floor, but S_held only 0.07 [−0.02, 0.16].
  - The weight-based DAgger arm (Qwen3.5-9B imitating GLM-5.3 Flash, with expert guidance) gets S_held = +0.29 [0.23, 0.35].
- *Limitations.* The DAgger arm uses a stronger teacher, so it is not a pure self-learning comparison. Early ARC-2/3 results only; two main models.

**10. Continual Harness: Online Adaptation for Self-Improving Foundation Agents** ([2605.09998](https://arxiv.org/abs/2605.09998), 2026-05-11; Princeton / GDM; preprint)
- *What it does.* A reset-free agent alternates acting with refining its own prompt, sub-agents, skills and memory in Pokémon Red/Emerald. It grew out of Gemini Plays Pokémon. Adds an online model–harness co-learning loop: an open model is trained on teacher-relabelled rollouts.
- *Key results.* (§4.3–4.4 ✓)
  - Recovers "a majority" of the gap to an expert harness.
  - On Emerald, Pro from scratch reaches 100% of milestones at a $130 median vs H_min at 98% for $215.
  - On Red, the bootstrap-updating variant is more efficient than from-scratch at every milestone, i.e. positive warm start within the same game.
  - Flash-Lite drops from 20% with H_min to 3–13% with every CH variant. There is a capability floor.
- *Limitations.* (§6 ✓) Co-learning is not run to convergence. Reset vs reset-free regimes are not compared head to head. Seeds are few (medians shown).

**11. EmbodiedCLBench** ([2610.13021](https://arxiv.org/abs/2610.13021), 2026-10-09; PKU / BIGAI / HKU; ICLR-27 sub kODZPTXW7U)
- *What it does.* 168 basic and 100 advanced household tasks. The agent learns from basic tasks, by ICL over trajectories or by written skills, and must compose them into advanced tasks.
- *Key results.*
  - Advanced-task zero-shot SR is 3–12%; after learning from two basic tasks, ICL reaches 16–52% and skill learning 22–38% across 4 harnesses × 2 models (Table 1 ✓).
  - Neither medium dominates.
  - Most gain comes from the first sample: k = 0→1 takes SR from 8–10% to 28–40% (Hermes and OpenClaw with Qwen3.8-Flash), and more samples add no consistent gain (§5.2, Finding 3 ✓).
- *Limitations.* Synthetic graph environment; short sequences; no forgetting test.

**12. Useful Memories Become Faulty When Continuously Updated by LLMs** ([2605.12978](https://arxiv.org/abs/2605.12978), 2026-05-13, v2 Aug 29; UIUC / Tsinghua)
- *Key results.* (abstract, §4 ✓)
  - Consolidated-memory utility rises, then falls, and can drop below the no-memory baseline.
  - Consolidating even from ground-truth solutions makes GPT-5.4 fail 54% of ARC-AGI problems it previously solved.
  - The fault lies in the consolidation step: an episodic-only control stays competitive.
  - Agents allowed to choose Retain/Delete/Consolidate preserve raw episodes and double the accuracy of forced consolidation.
- *Relevance.* It is the memory-store analogue of plasticity decay. It supports Haber & Tononi's "unchecked plasticity" framing (2611.02216 ✓), whose selectionist > instructionist prediction remains untested.

**13. Experience Rot: Experienced Agents Fail Where Novices Succeed** ([2611.20922](https://arxiv.org/abs/2611.20922), 2026-11-16; UChicago / eBay / UF)
- *What it does.* Silently changes MCP tool definitions, e.g. renaming `attendees` to `guests`.
- *Key results.* (§1, §4–§8 ✓)
  - With incomplete docs, valid experience lifts success from 63% to 100%. Out-of-date experience drops it to 23%, far below agents without experience (88–100% across changes).
  - Experienced agents copy the earlier call even when the new tool definition shows the change, then report success.
  - The failure appears in every memory form tested and grows with the amount of experience.
  - Announcing the update barely helps. A check-the-docs instruction in the system prompt lowers success to 29%; the same instruction appended to the final task message raises it to 99%.
  - A hand-written earlier task that contains a read-back step gives 88% success with out-of-date experience, vs 46% after the agent's own earlier task (58% without the read-back step). The paper presents this as an evaluation confound ("hand-written histories hide the problem", §8), not as a deployable remedy.
- *Relevance.* A clean demonstration that prior learning can *reduce* adaptability to change. It is a behavioural LoP analogue at the context and memory level, and directly relevant to drift-based designs (MetaShift / Varying Goals). It also warns that evaluations using hand-written histories will understate the effect.

**14. Can Scale Save Us From Plasticity Loss in Large Language Models?** ([2606.24752](https://arxiv.org/abs/2606.24752), 2026-06-23; Zyphra; ICLR-27 sub 0H9Qwt6mOj)
- *Key results.*
  - GPT-style pre-LN transformers from 5M to 314M non-embedding parameters, trained on a cycling multilingual stream, all lose plasticity on a held-out Vietnamese probe (abstract ✓).
  - Onset follows T = 1.3×10⁻⁵ · P^0.8269 (Eq. 2, Fig. 3 ✓), i.e. sublinear in size.
  - LoP also appears under stationary multilingual training (abstract ✓).
- *Relevance.* Weight-level anchor: scale delays LoP but does not remove it. The plasticity-as-gain-per-$ literature for agents has no analogue of an onset law.

**15. Governing Agent Self-Improvement Across Timescales** ([OpenReview c3lHKtsSMq](https://openreview.net/forum?id=c3lHKtsSMq), ICLR-27 submission; anonymous)
- *What it does.* (◐) ReMo is a controlled dual-timescale framework: within-task refinement plus cross-task memory. Both mechanisms help and are complementary, but their marginal value diminishes, and memory keeps growing after it stops helping.
- *Key results.* AutoGovern (online stop rules) cuts tokens 38% on average and up to 72% "while maintaining comparable task performance" on AppWorld and Formula (◐).
- *Limitations.* PDF not accessible. It treats saturation as a cost problem, not as loss of future learnability.

**Remaining papers**

| id | title | one-liner | mark |
|---|---|---|---|
| [2605.09315](https://arxiv.org/abs/2605.09315) | Do Self-Evolving Agents Forget? | "Capability erosion" across workflow, skill, model and memory evolution. CPE raises retained simple-task score from 41.8% to 52.8% (GPT-5.1) | ✓ abstract-level numbers from full text |
| [2608.19013](https://arxiv.org/abs/2608.19013) | Harness Continual Learning (ICLR27 dVv1MBkavP) | Names harness-level forgetting. A guarded commit checks current gain, historical retention and validity | ✓ abstract |
| [2610.12668](https://arxiv.org/abs/2610.12668) | ReHarness (MIT / Meta) | Backward revision of retrieved reflections in a strictly online stream. Claims better stability and plasticity | ◐ |
| [2610.31401](https://arxiv.org/abs/2610.31401) | HERD | Harness skills plus bounded residual memory, with drift-constrained dual-anchor distillation into weights (TRACE+17) | ◐ |
| [2610.33956](https://arxiv.org/abs/2610.33956) | Continual Learning Exam (CLE) | 120 tasks × 5-level ladders across 89 roles. Best system clears 47.7% of levels and finishes all 5 levels on 23.3% of tasks. Feedback plus one revision gives +5.3 pp median | ✓ abstract |
| [2611.12988](https://arxiv.org/abs/2611.12988) | Third Eye | LoRA self-update: +0.02 pp immediately, −1.76 pp after the next native update. Selection inflates the first step | ✓ abstract |
| [2611.08946](https://arxiv.org/abs/2611.08946) | Flex-RSI | Weaker model (Qwen3.8-27B) starting from a stronger model's (Gemini 4 Argon) harness: Active Search 49.2 vs 69.0 own (Table 9). A warm-start penalty confounded with the model swap | ✓ (Table 9, this session) |
| [2611.07624](https://arxiv.org/abs/2611.07624) | Beneath the Surface | All 16 evolved agents gain previously uncovered tasks while losing previously covered ones; one case +5.2 reliability despite losing 10 covered tasks | ✓ abstract |
| [2610.38392](https://arxiv.org/abs/2610.38392) | Masked Erosion | Acceptance gates erode redundant safeguards. 411 loops × 10 gate rules | ✓ abstract |
| [2611.13524](https://arxiv.org/abs/2611.13524) | The RSI Tax | Bounded-note baseline matches Meta-Harness, EvoSkill and RRSI with 65.6–71.0% fewer tokens. Cites Agent Plasticity | ✓ abstract |
| [2611.19700](https://arxiv.org/abs/2611.19700) | Where Should LLM Agents Adapt at Test Time? (Apple) | Qwen3.5-9B, 109 tasks, 5 orders. Best ICL policy 28.8→49.4%; ICL+GRPO 51.6%; SDPO mixed; ICL skips verification | ✓ abstract |
| [2611.22525](https://arxiv.org/abs/2611.22525) | When Agent Skills Go Stale (AWS) | After environment change, once-useful skills cost >10 pp or about 40% more execution vs no skill. Selective maintenance by replaying checks | ✓ abstract |
| [2611.14326](https://arxiv.org/abs/2611.14326) | FGSE: Falsification-Gated Self-Evolution | Lessons stored as hypotheses tested on transfer, falsification and archived-regression probes before commit. Regression damage 1.3%, harmful commits 3.6% | ✓ abstract |
| [2611.02216](https://arxiv.org/abs/2611.02216) | Do agents need to sleep? (Haber & Tononi) | Perspective. Accumulated memory as "unchecked plasticity". Predicts selectionist > instructionist consolidation at scale | ✓ |
| alphaxiv 2611.verified-archives-multi-agent-search | Scaling Discovery Does *Not* Require Forgetting (Cumes) | Verified shared archive vs forgetting on the Anthropic take-home. Names the confound of attempts vs conversation | ✓ (factcheck) |
| [2611.23219](https://arxiv.org/abs/2611.23219) | Do LLM Agents Still Need to Be Told How to Self-Improve? (MIT) | Frozen agents given primitives build their own learning loop. Learned text artifacts transfer across games and models | ◐ (title resolved ✓) |
| [2611.20655](https://arxiv.org/abs/2611.20655) | MetaAct: Self-Evolving Executable Action Constraints for Embodied Language Agents | Self-evolving executable action constraints instead of text artifacts (embodied) | ◐ (title resolved ✓) |
| [2606.09932](https://arxiv.org/abs/2606.09932) | Rejuvenating Plasticity for SFT→RL (CC4ksJJPIN) | Excessive SFT reduces RL plasticity. Remedy: base-anchored fusion plus neuron reset | ◐ |
| [2609.33620](https://arxiv.org/abs/2609.33620) | Learning Dynamics of CL (Oxford / GDM) | Unified view of attribution, forgetting and LoP for LM updates | ◐ |
| OpenReview [hip2PfZSRg](https://openreview.net/forum?id=hip2PfZSRg) | Do NNs Lose Plasticity in Gradually Changing Tasks? | Abrupt transitions contribute to LoP; gradual change reduces it | ◐ |
| OpenReview [f4TzyqXvHT](https://openreview.net/forum?id=f4TzyqXvHT) | TenureBench | 10 years of finance, medicine and law documents replayed to persistent agents. Agents do *worse* on later tasks; best 52.7% vs humans 84.0%; history helps only 1 of 5 agents | ◐ |
| OpenReview [2e5z57jIPK](https://openreview.net/forum?id=2e5z57jIPK) | Seven-Year Itch of Lifelong Agents | Beyond some history length, more active memory reduces adaptation to a drifting user. Finitude-Aware Memory | ◐ |
| OpenReview [udjtWhlqMB](https://openreview.net/forum?id=udjtWhlqMB) | Textual Catastrophic Forgetting in Skill Libraries (TCF-Bench) | Leave-and-return protocol. EcoForge returning success .755 vs .159, but the advantage does **not** reproduce with live LLM executors | ◐ |
| OpenReview [j2RMOsmge4](https://openreview.net/forum?id=j2RMOsmge4) | SkillFlow | 20 workflow families, 166 tasks, 10 models. Similar vanilla scores, very different benefit from skill evolution; self-confirming checks give negative transfer | ◐ |
| OpenReview [Pe4LarfEaD](https://openreview.net/forum?id=Pe4LarfEaD) | AGSE | Attribution-guided skill repair. FWT +0.0227, forgetting 13.95% | ◐ |
| OpenReview [hrBgrRlbjW](https://openreview.net/forum?id=hrBgrRlbjW) | Demotion Beats Deletion | Graded forgetting at 35% budget answers 60.0% of evicted-past questions vs 2.5% for deletion | ◐ |
| OpenReview [rcXmJY7QVs](https://openreview.net/forum?id=rcXmJY7QVs) | Active Forgetting (ACTF) | Dependency-aware context pruning. Lost-in-Conversation 44.2→61.0% | ◐ |
| OpenReview [3hwygrPlXH](https://openreview.net/forum?id=3hwygrPlXH) | When Does Continual Learning Require Learning | Weight, prompt, retrieval and compression all treated as update operators (Qwen3-8B). Retrieval wins for changing facts until conflicting versions pile up | ◐ |
| OpenReview [nw2tokNgb2](https://openreview.net/forum?id=nw2tokNgb2) | Meta-TTL | Bi-level meta-learning of the self-improvement policy; up to +23% ID | ◐ |
| OpenReview [ionMcwU5xU](https://openreview.net/forum?id=ionMcwU5xU) | SCLATE | Shared event scheduler for continual-learning agent evaluation and training. Added memory systems do not reliably beat native harness memory | ◐ |
| OpenReview [T04ZzT5m6j](https://openreview.net/forum?id=T04ZzT5m6j) | When Self-Reinforcement Ends Self-Improvement | Proof: deterministic judge errors make every fixed acceptance rule eventually drift negative | ◐ |
| [2610.03243](https://arxiv.org/abs/2610.03243) | Kinetic Theory of the Gated Self-Evolving Agent | Hitting-time certificate for gated skill evolution. The evaluator must stay outside the population | ◐ |
| [2611.05450](https://arxiv.org/abs/2611.05450) | ReScopeBench (experience lifecycles) | Benchmarks experience lifecycles in evolving environments | ◐ |
| [2610.07792](https://arxiv.org/abs/2610.07792), [2610.08215](https://arxiv.org/abs/2610.08215) | ServeLearnBench; Learn2Play | Learning-from-experience benchmarks (serving traces; unfamiliar games) | ◐ |
| [2609.04280](https://arxiv.org/abs/2609.04280) | EvoHarnessBench | Agents must keep pace with an evolving harness | ◐ |
| [2611.04952](https://arxiv.org/abs/2611.04952) | Artifact-to-Capability | Reflexion, Voyager and DGM on Gomoku; DGM shows "loss-only drift". Cites Agent Plasticity | † (r1) |
| [2611.11342](https://arxiv.org/abs/2611.11342) | Failure Traces vs matched search | About 60% of the gain comes from search alone. Partial budget-matched control | † (r1) |
| [2611.21710](https://arxiv.org/abs/2611.21710) | Learning In-Context Adaptivity for LLMs through Continual Gameplay | New S2 forward citer of Agent Plasticity (Nov 16); not read | † (title resolved via S2 ✓) |
| [2610.25561](https://arxiv.org/abs/2610.25561), [2610.34073](https://arxiv.org/abs/2610.34073) | Let Them Sleep; Memory Tax | NN memory "quietly stops working" (pruning remedy); many memory systems underperform naive RAG | † (r1) |
| Dohare 2024 Nature; [2303.01486](https://arxiv.org/abs/2303.01486); [2205.07802](https://arxiv.org/abs/2205.07802) | Classical LoP | Continual backprop, plasticity mechanisms, primacy bias / resets | † (Dohare 2024 Nature 632:768–774 confirmed as cited in 2610.08902 refs ✓) |

### Consensus, debates & contradictions

- **Consensus: where updates land decides forgetting.**
  - Global shared state forgets. GEPA prompt rewrites (2610.25858 ✓) and harness verification checks (2611.03219 ✓) are the clearest cases. A *single* injected verification conflict takes SWE from 96% to 0%.
  - Localized retrieval forgets less but collides on similar queries (2610.25858 §4.4 ✓).
  - Interleaving does not rescue shared-state methods (GEPA +5.8→+0.9 ✓).
- **Consensus: actor-level persistence is a speedup, not a capability gain.**
  - Continual AutoResearch: positive time-convergence, ~0 final, negative after first-solution control ✓.
  - Continual-ARC: harnesses as shipped ≤ demo cache, ~0 on unseen; even expert-guided notes save nothing measurable on unseen tasks ✓.
  - EmbodiedCLBench: saturates after one sample ✓.
  - TenureBench: agents get worse over a lifetime ◐.
  - The Dai/Ruan survey's line fits: "Reuse in later tasks alone does not establish … recursive dependency" (digest ✓).
- **Debate: forget vs remember.**
  - Tan et al.: resetting the session and keeping only the best artifact beats continuing (✓).
  - Cumes: a verified shared archive matches it, and forgetting conflates deleting attempts with deleting conversation (✓ factcheck).
  - CL-Bench: *full-context ICL* is the best continual learner (✓).
  - Probable reconciliation (judgment): context helps while the latent structure is still being discovered (tens of instances). It anchors once search must leave a local basin (hundreds of turns).
- **Debate: is warm-starting from evolved state good?**
  - Positive:
    - Continual Harness bootstrap-updating beats from-scratch at every Red milestone (same game) ✓.
    - SIM continuation beats fresh RSI on 2 of 3 subsequent Gemini generations ✓.
  - Negative:
    - Harness Transfer < Cold Start in 3/4 cross-model domains (2611.18856 ✓).
    - Flex-RSI Table 9 ✓.
    - SIM < fresh on the strongest model (94.8 vs 95.6) ✓.
    - Experience Rot (stale experience is worse than none) ✓.
  - Every negative case is confounded by a model or environment change. **None of the papers reviewed here isolates "same model, same environment family, k prior rounds" vs fresh.** That is the missing LoP test.
- **Debate: is plasticity a model property or trainable?**
  - Agent Plasticity: "a property of an agent under a particular learning protocol" (§4 ✓).
  - MetaExp shows it can be trained into weights (+8.5 vs −2.2) ✓.
  - Continual-ARC shows weight updates generalize where harnesses do not ✓.
  - 2611.19700 shows GRPO restores verification behaviour that ICL drops (✓ abstract).
  - Direction of travel: harness for speed, weights for generality.
- **Contradiction on memory systems.**
  - Dedicated memory systems underperform raw context or episodic stores in CL-Bench, Useful Memories and SCLATE.
  - Yet FGSE, HCL, AGSE and TCF-Bench methods report large wins, mostly in-house and single-run.
  - TCF-Bench's own live-executor check failed to reproduce its simulator advantage ◐.
- **Terminology clash.**
  - "Plasticity" means three things here:
    - gain per $ from experience (Agent Plasticity);
    - within-block learning P (2610.25858);
    - capacity to learn after prior learning (Dohare / Zyphra).
  - "Forgetting" is both a pathology (catastrophic) and a remedy (deliberate reset).
  - Papers in this corner should state which they mean.

### Open problems

1. **Forward LoP for frozen-weight agents.** After k ∈ {0, 2, 5, 10, 20} self-improvement rounds on families A, measure learning speed on fresh family B against a reset at matched tokens. Report a plasticity ratio PR(k), and an onset law analogous to Zyphra's T ∝ P^0.83. Not measured in any paper reviewed here.
2. **Improver (meta-level) plasticity.** Does a generation-g improver, given its procedure and/or archive, produce smaller gains on a fresh target than generation 0? SIM (2611.18856) studies optimizer *memory* for one transition only. Procedure × memory has not been crossed.
3. **Port classical LoP remedies to text and code state, scored on plasticity rather than forgetting.** Candidates:
   - periodic reset plus distilled notes (primacy-bias recipe);
   - utility-tracked reinitialization of low-use harness components (continual backprop);
   - prune-and-paraphrase (shrink-and-perturb);
   - selectionist sleep (Tononi's prediction, untested).
4. **The gate as the stability–plasticity dial.** Measure freeze rate vs PR(k) vs forgetting across gate rules. Evidence so far: the Meta-Harness gate freezes; Masked Erosion shows gates erode safeguards; T04ZzT5m6j proves fixed rules drift negative under deterministic judge errors. No systematic sweep exists.
5. **Separate what accumulates.** Tan et al. and Continual AutoResearch both ask for a factorial over conversation, scored attempts, notes, code and tools. Neither has run it.
6. **Drift-induced rigidity.** Experience Rot and Skills Go Stale show that old state blocks adaptation after *silent* change. The open question is whether harness self-evolution learns to re-verify, or gets worse with k. Most real deployments have exactly this structure.
7. **Long horizons.** Most streams are 3–5 tasks (Continual AutoResearch), tens of instances (CL-Bench) or 150–200 positions (2610.25858). Slow decay needs hundreds of family switches. A cheap procedural generator is the enabler.
8. **Seeds and open models.** Most headline numbers rest on one model and one run per cell: 2610.25858, 2611.03219, most of SIM, and Continual AutoResearch except Sol. A replication on open 9B–30B models with ≥ 5 seeds would itself be a contribution.
9. **Transfer of improvement strategies across tasks.** Agent Plasticity lists this as a limitation. Its interaction with plasticity loss is untested: specialization may raise home-domain plasticity and lower far-domain plasticity.

### Where it is heading (to mid-2027)

These are forecasts from judgment, not measurements.

- **"Plasticity" becomes a named sub-field.** Round-2 field forecast, 70% confidence (`round2/field_forecast.md` ✓ as a file).
  - Expect Meta MSL follow-ups on RL-for-plasticity and actor×artifact swaps. Both are publicly flagged in 2610.08902 §4.
  - Expect Mila/GDM follow-ups on the P/S decomposition with more models.
- **Harness + weights hybrids become the default answer** (MetaExp, HERD, Continual Harness co-learning, Continual-ARC DAgger, 2611.19700). Pure-harness continual learning will be framed as a fast but non-generalizing tier.
- **Benchmark crowding continues** (CL-Bench, CLE, EmbodiedCLBench, Continual-ARC, TenureBench, SkillFlow, SCLATE, ReScopeBench…). New benchmarks will need drift realism (TenureBench-style replay) or a new axis (the improver regime, as in MetaShift).
- **Memory governance papers multiply.** Topics: when to consolidate, demote or forget; provenance; falsification gates. Expect the selectionist-vs-instructionist test within months.
- **Most likely scoop for Dan's lane: a forward-LoP / "warm-start penalty" paper at actor level** from SIM-, Flex-RSI- or Continual-AutoResearch-adjacent groups. They already hold reset arms and transition setups. The improver-level version is likely to stay open longer: 2+ months of headroom, judgment.

### Connection to our ideas (`/home/user/nlp-lab-paper-search/research/ONE_PAGERS.md`)

- **#6 Meta-Overfitting (formerly "second-order plasticity" / meta-plasticity): most directly in this corner.**
  - New support:
    - SIM shows harness-only transfer finishing below cold start (3/4 cross-model domains) and a seed-beating start penalty, with recovery only via optimizer-side provenance memory (2611.18856 ✓).
    - This supports the predicted "procedure transfers worse than memory" and the "reset procedure, keep memory" rule.
  - New threat:
    - SIM already owns "optimizer memory across transitions".
    - MetaExp shows experience-learning skill *can* transfer when trained.
  - Required differentiation: graded A→B distance, a procedure × memory 2×2 at the *improver* level, k-step dose–response, and same-model transitions to remove SIM's model-swap confound.
- **Forward LoP / PR(k) (round-1 lead P1).** Still unclaimed among papers reviewed, and the cleanest plasticity contribution. Recommended framing: "improvability conditional on prior self-improvement vs reset". Avoid "stability–plasticity", which 2610.25858 owns, and actor-level "agent plasticity", which 2610.08902 owns. Tan et al.'s best-artifact reset is a ready-made remedy arm, and Experience Rot supplies a drift-probe family B (use agent-generated, not hand-written, histories: hand-written ones hide the effect, 2611.20922 §8 ✓).
- **#3 Gate, Not Willingness / #2 Forced-Quota.** The Meta-Harness gate "freezing the workspace" (2610.25858 ✓) is gate-induced plasticity loss at the object level. Add a freeze-rate and PR(k) readout to the gate factorial. T04ZzT5m6j's negative-drift theorem and Masked Erosion are needed citations.
- **#7 Varying Goals and #9 MetaShift.**
  - Reuse 2610.25858's streams and P/S/I/F metrics as baselines; its blocked vs interleaved runs are the two switching-period endpoints.
  - Experience Rot and Skills Go Stale are grounded "silent regime change" probes.
  - hip2PfZSRg (abruptness matters for weight LoP) and Seven-Year Itch are adjacent but stay at weight or memory level, not improver level.
- **#1 Taught, Not Selected.** Two observations from this corner fit "what the reflector reads": Continual AutoResearch's "highly specific cards anchor" failure and Experience Rot's copying of earlier calls (and of the read-back procedure in hand-written histories). Both are cases where retained text, not selection, steers later behaviour.
- **Practical pilot hint.** Tan et al. ran DeepSeek V4.1 Flash at $20–40 per replicate with 3 replicates. Their continue-vs-forget design from a plateaued state (Table 3) can be repurposed directly as the k-rounds vs reset probe for an improver, on Dan's procedural generator, for well under $100 per task (judgment; 3 replicates × 2 arms at $20 is ~$120 at their budgets, so expect $100–250 per task).

#### Fact-check log

| Claim | Verdict | Source location |
|---|---|---|
| 2610.25858: P–S r = −0.72, p = 0.008, n = 12; Gemini 3.6 Flash single model; 8 rollouts/instance | confirmed | 2610.25858 Fig. 2 (n = 12 label), §4.1 |
| 2610.25858: terminal adv. GEPA +3.7 / RB +2.5 / MH +1.4; P +6.2/+3.3, S −4.3/−1.9 | confirmed | §4.2, Table 1 "Overall average" row |
| 2610.25858: Meta-Harness "freezing the workspace" quote | confirmed | §4.2 |
| 2610.25858: BIRD-F1 share 100→40.2%, gain +7.5→−2.0; interleave +5.8→+0.9 | confirmed, location corrected (§4.3 + Appendix A, not §4.2); added intermediate values | §4.3, Fig. 3; Appendix A table |
| 2610.25858: BFCL-Miss ask rate 60.8→8.5% | confirmed, attribution clarified (Meta-Harness, compound drift); added GEPA 29.8→11.2% | §4.3 |
| 2610.20563: Table 3 +9.2% ±6.9 ($15.32) vs +1.4% ±1.3 ($24.67); DeepSeek V4.1 Flash; Pi agent; 3 runs | confirmed | §4, Table 3 |
| 2610.20563: Table 1 +39% AHC014, −23% VLIW; Table 2 +11.2/+4.8/+1.7; 532/763 turns, 66.2%; 150→30 lines, 28k→7k | confirmed | Tables 1–2, §5 |
| 2611.03219: 94.0→32.0% (66%); 87.1/8.2/0.6%; 96.0→0.0 | confirmed; clarified percentages are of pass→fail cases among 520 trajectories | abstract; §4.2.1; Table 1 |
| 2611.03219: "five evolution rounds per stage"; "six orderings" | corrected: AHE 9 rounds, Meta-Harness 5 (original settings), forgetting read after 5 continual rounds; six "directions" | §3.4, §4.1, Fig. 3 |
| 2611.05108: time-convergence +21.3/+20.9/+7.5/+6.1/+3.0; +24.0→+1.2; final +7.6/−0.9/−0.8; first-valid control negative | confirmed | §5.2, §5.3, Table 2 |
| 2611.05108: Sol repeat variation ≤ 0.75 points | corrected: ≤ 0.75 for outcome/initialization only; time-convergence varied up to 7.14 | §5.1 |
| 2611.18856: +2.36 [1.05, 3.68], p = 0.003; −0.60 to −5.48; HT < Cold Start in 3 domains; 68.41→84.75 vs 73.42; 94.8 vs 95.6 | confirmed | §4.3.1, §4.4, Tables 1, 3 |
| 2610.08902: P_sat 298/233/57/14/0; chess 37.5→73.3, 25.0→66.4; NetHack 61.75; reuse 94–98%, 83–99% | confirmed; model list extended (Opus 4.6); "12 held-out games" upgraded from † to ✓ | §2.2, §3.1–3.5 |
| 2610.21492: 65.0→73.5 vs 68.9→66.7; strict-OOD +9.3; 4,956 trajectories, GLM-5.2 teacher; AutomationBench +14.8 | confirmed; added net endpoint gap +6.8 alongside +4.6 | Table 1, Table 2, §5.1, abstract |
| 2611.14505: "neither harness beats its demo-cache floor" | corrected: holds for harnesses as shipped; expert-guided GLM notes reach S_seen 0.74 > 0.44 (S_held 0.07, n.s.) | §4, Table 1 |
| 2611.14505: demo cache 0.37/0.44, programs 0.65/0.81 & 0.03/0.05, DAgger S_held 0.29 [0.23, 0.35] | confirmed | §4 Baselines, Table 1 |
| 2611.20922: 63→100%, 23%, novices 88–100% | confirmed | §1, §4 |
| 2611.20922: "hand-written history with read-back restores 88%" as remedy | corrected framing: number right (88% vs 46%), but the paper presents it as an evaluation confound (§8); added the 99% task-message instruction result | §7–§8, Fig. 6 |
| 2606.05661: ICL Sonnet 4.6 22.3/25.4; ACE 10th by gain 8.6 at $62.8; Mem0 20.2; Gemini −5.6 | confirmed | §5 text, Table 1 |
| 2606.24752: 5M–314M, Vietnamese probe, T = 1.3×10⁻⁵·P^0.8269, stationary LoP | confirmed | abstract, Eq. 2 |
| 2605.09998: $130 vs $215; Flash-Lite 20% → 3–13%; bootstrap-updating > from-scratch on Red | confirmed (clarified Emerald vs Red) | §4.3, §4.4 |
| 2610.13021: zero-shot 3–12%, ICL 16–52%, skill 22–38%; k=0→1 8–10%→28–40% | confirmed | Table 1, §5.2 Finding 3 |
| 2611.08946: Table 9 Active Search 49.2 vs 69.0 | confirmed (Qwen3.8-27B from Gemini 4 Argon harness) | Table 9 |
| 2605.12978: below no-memory baseline; GPT-5.4 fails 54%; double accuracy | confirmed | abstract, §4 |
| Agent Plasticity distinguishes itself from LoP | confirmed | 2610.08902 appendix (line "This also distinguishes our usage of plasticity from the loss of plasticity…") |
| "No paper isolates same-model k-rounds vs fresh" / forward LoP "unmeasured anywhere" | unverifiable as a universal claim; softened to "none of the papers reviewed here" | judgment over papers read |
| Pilot cost "well under $100" | corrected: at Tan et al.'s $20–40 per replicate, 3 replicates × 2 arms ≈ $120–240 per task | 2610.20563 Appendix A, Table 4 |
| ID check 2611.23219 | confirmed: "Do LLM Agents Still Need to Be Told How to Self-Improve?" | local full text title |
| ID check 2611.20655 | confirmed: "MetaAct: Self-Evolving Executable Action Constraints for Embodied Language Agents" (2026-11-16) | Semantic Scholar |
| ID check 2611.21710 | corrected title: "Learning In-Context Adaptivity for LLMs through Continual Gameplay" (2026-11-16) | Semantic Scholar |
| ID check 2609.33620 (Learning Dynamics of CL, Oxford/GDM) | unverifiable this session (S2 rate-limited, arXiv API host unreachable, no local copy) | none |
| Cumes rebuttal numbers (914 cycles, 7/12 false-floor) | not re-checked; kept as prior factcheck ✓ | round2/verify factcheck |

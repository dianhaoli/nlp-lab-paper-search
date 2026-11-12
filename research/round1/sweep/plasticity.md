# Sweep: Agent plasticity, continual learning and loss of plasticity in (meta-)agents

Compiled 2026-11-12. Sources: alphaXiv discover/full-text for 6 papers, Semantic Scholar forward citations of Agent Plasticity (CorpusId 292694189 = arXiv 2610.08902), Continual Learning Bench (2606.05661) and Dohare et al. Nature 2024 (CorpusId 259251905, 477 citations, filtered for LLM and agent work). Every ID below appeared in a tool result. IDs written as alphaXiv slugs (non-numeric) are marked as such.

---

## 1. Summary of the landscape

**The short version.** Between June and November 2026, "continual learning for frozen-weight self-evolving agents" grew from a niche into a crowded subfield. Since October there has been roughly one new paper a week. The work falls into four clusters:

1. **Measuring learning-from-experience (actor-level plasticity).** Agent Plasticity (Meta MSL, Oct 6) defines plasticity as held-out gain per $ of learning cost. It is joined by Continual Learning Bench (Jun), AgentCL (Jun), Continual Learning Exam (Oct 25), Learn2Play, ServeLearnBench, ReScopeBench, EmbodiedCLBench, PATH-Bench, Continual AutoResearch and SelfImprove-Bench. All of them measure whether **the acting agent** gets better from experience.
2. **Forgetting and the stability-plasticity trade-off in harness evolution.** Are Self-Evolving Agents Continual Learners? (Mila/GDM, Oct 19) and Can Coding Agents Sustain Continual Evolution without Forgetting? (Tsinghua, Nov 2) both show catastrophic forgetting when GEPA, Meta-Harness or AHE evolve across a task sequence. In the Tsinghua study, SWE-bench drops from 94% to 32% after evolving on FeatureBench. Further work in this cluster: Harness Continual Learning (Aug), Do Self-Evolving Agents Forget? (May), REpertoires (Oct), HERD and ReHarness. **Forgetting** (the backward direction) is now well covered.
3. **Memory and consolidation degradation.** Useful Memories Become Faulty (May) shows consolidated-memory utility rises, then falls below the no-memory baseline. Related: Memory Tax, Let Them Sleep, Do agents need to sleep? (a Tononi perspective that frames accumulated agent memory as "unchecked plasticity"), Lost in Implicit Routing (skill libraries break at scale), and Measure Learning at Steady State (ICL context reaches 95k tokens and the cost inverts).
4. **Process-level skepticism about RSI.** Third Eye (Nov 8) finds a LoRA self-update looks neutral right away but hurts after the next update. Also: Auditing the Product, Not the Recursion (a meta-audit of 114 RSI papers: process claims are rarely tested), The RSI Tax, Failure Traces vs matched search, Beneath the Surface, Harness-Delta Attribution, Elicitation or Expansion, and Does Failure Feedback Direct Self-Modification.

**What nobody has done (the key gap for Dan):** no paper measures **loss of plasticity in the forward direction**. That question is whether an agent or improver that has already gone through many self-improvement steps is *worse at learning something new* than a fresh one at matched budget, the way Dohare et al. measured it for networks. Nobody has asked this of the **improver / meta-agent** itself either. The closest evidence is scattered and unplanned:

- In Are SE Agents CL, Meta-Harness's validation gate "rejects half or more of the candidate updates, freezing the workspace on later blocks at the cost of low plasticity". This is a gate-induced plasticity loss that the paper does not study further.
- In Agent Plasticity, GPT-5.5 stops using its own artifacts after checkpoint 9 (96–100% reuse falls to 32–50%) with the artifacts unchanged. Claude Opus 4.8 improves its artifact-reuse metrics in Go while its score falls.
- In Flex-RSI Table 9, starting a weaker model from a stronger model's evolved harness and improver is sometimes *worse* than starting fresh (Active Search: 49.2 vs 69.0). This is a warm-start penalty, but it is confounded by the model mismatch.
- Continual AutoResearch finds persistent state improves efficiency but not final quality, compared with a reset agent.
- Third Eye shows the same 2-step degradation for weights only.

Agent Plasticity explicitly *disclaims* the loss-of-plasticity reading. It says it "distinguishes our usage of plasticity from the loss of plasticity studied in continual neural-network training (Nikishin et al.; Lyle et al.; Dohare et al.)", and it never measures plasticity on a second task or as a function of how much has already been learned. **That opening is Dan's lane.**

---

## 2. Anchor paper deep-read: Agent Plasticity (Singh, Bakhtin, Shao, Synnaeve, Kulikov, Fergus, Arora, Keutzer, Weston, Mahajan, Goyal; arXiv 2610.08902; 2026-10-06)

- **Protocol.** Weights are frozen and each episode starts in a fresh context. The agent keeps a persistent artifact inventory I_t (Python tools, skills, strategies, memory). Reflection (same model as the actor) proposes I_{t+1} = U_M(I_t, E_train). Updates must pass structural/exec checks, and rejected attempts are still counted in the cost. Splits are train, held-out ID and held-out OOD, where OOD means a stronger opponent, not a new task.
- **Environments.** Chess (Hard/Easy, 20 update rounds; 16 train / 12 ID / 12 OOD games per checkpoint), 5×5 Go, 6×6 Hex, and NetHack (40 checkpoints, Claude Code / Codex harnesses).
- **Metric.** P = (S_T − S_0)/C_T, in points per $1k. The headline is P_sat: a monotone saturation curve is fitted and the gain per cost is taken at 90% of the fitted rise. P_sat is set to 0 if the rise is under 10 pp, and "plasticity to date" is reported if the curve has not saturated.
- **Findings.**
  - (1) Trajectories differ sharply by model. In Hard chess, Fable 5 goes from 37.5 to 73.3 and Opus 5 from 25 to 66.4. GPT-5.6 Sol goes from 0 to 36.9 but has the highest P_sat (298 vs 57 for Fable 5).
  - (2) Endpoint and acquisition efficiency diverge.
  - (3) Low-plasticity agents fail to reuse relevant artifacts (Luna does not use one in about 98% of relevant decisions). High-plasticity agents reuse artifacts at 94–98% but still fail while using them (83–99% of their remaining failures).
  - (4) Results are model × task dependent, with no universal ranking.
  - (5) NetHack: Opus 5.5 is the only reliable riser.
- **Limitations stated by the authors:**
  - (a) Experience is not isolated from extra compute. They name "budget-matched reflection without feedback" and controlled actor–artifact swaps as future work.
  - (b) The artifact-use diagnostics are observational. They suggest "artifact removal, replacement, or forced invocation" as interventions.
  - (c) Lineages are trained separately, so transfer of learned artifacts *or improvement strategies* across tasks is not established.
  - (d) The P estimate depends on headroom, horizon, fit and pricing.
  - (e) OOD only means a stronger opponent.
  - (f) Plasticity as a post-training (RL) target is future work.
- **What reviewers would attack.**
  - Board games with closed frontier models only, so the work is hard to reproduce.
  - Dollars are used as the cost unit, which is provider-dependent.
  - Labels come from a Gemini-3.1 judge.
  - Small game counts (12 held-out games per checkpoint) make the noise large.
  - Actor and reflector are the same model, so they cannot be separated.
  - Plasticity is never measured *after* prior learning (no loss-of-plasticity test).
  - There is no non-frontier/open model.
- **Who cites it (S2, as of Nov 12):**
  - The RSI Tax (2611.13524)
  - Agent Self-Improvement with Frozen Weights: Added Value and Limits of Failure Traces (2611.11342)
  - Artifact-to-Capability (2611.04952)

  None of the three studies plasticity loss or the improver's plasticity.
- **Its references** include Dohare 2024, Lyle 2023 (Understanding plasticity), Nikishin 2022 (Primacy bias), Gödel Agent, ADAS, DGM, HGM, STOP, Promptbreeder, CL-Bench, AgentCL, MUSE-Autoskill, MetaSkill-Evolve, EvaLearn, StreamBench and LifelongAgentBench.

---

## 3. Key papers table

| # | Title | ID | Date | Finding | Critique / limitation | Relevance |
|---|---|---|---|---|---|---|
| 1 | Agent Plasticity: Measuring Self-Improvement Through Experience | 2610.08902 | 2026-10-06 | Plasticity = held-out gain per $; endpoint ≠ efficiency; reuse is a bottleneck | No compute-matched no-feedback control; no transfer or second-task test; same model acts and reflects; closed models only; explicitly *not* loss of plasticity | Anchor; leaves meta-agent and LoP open |
| 2 | Are Self-Evolving Agents Continual Learners? (Mila/GDM) | 2610.25858 | 2026-10-19 | 4 drift streams (BIRD/BFCL/ScienceWorld); P vs S correlation r=−0.72; GEPA most plastic and least stable; Meta-Harness gate freezes workspace | Single model (Gemini 3.6 Flash); 1 seed per stream apparently; P is measured per block but never as a trend over blocks or against a reset agent | **Closest threat** to a "stability-plasticity in harness evolution" paper; leaves forward-plasticity loss open |
| 3 | Can Coding Agents Sustain Continual Evolution without Forgetting? | 2611.03219 | 2026-11-02 | Meta-Harness and AHE across SWE/SWT/FeatureBench; up to 98–100% relative forgetting; 87% of pass→fail cases are verification conflicts | 50/50/30 tasks, temperature 0, one model (DeepSeek-V4-Flash), no seeds, two-stage only | Forgetting covered; plasticity not measured |
| 4 | Harness Continual Learning: Continual Adaptation Beyond Model Parameters | 2608.19013 | 2026-08-19 | Defines harness-level forgetting; guarded updates; 4-component harness | Method plus framing; forgetting-centric | Prior framing for "HCL" |
| 5 | Do Self-Evolving Agents Forget? Capability Degradation and Preservation | 2605.09315 | 2026-05-10 | "Capability erosion" across workflow/skill/model/memory evolution; CPE mitigation | Backward-direction only | Prior for erosion |
| 6 | Useful Memories Become Faulty When Continuously Updated by LLMs | 2605.12978 | 2026-05-13 | Consolidated-memory utility rises, then degrades below no-memory; GPT-5.4 fails 54% of previously solved ARC items after consolidation from ground truth | Memory only, not the improver | Evidence for degradation as the learned state accumulates |
| 7 | Third Eye: Beyond Immediate Gains in RSI | 2611.12988 | 2026-11-08 | LoRA self-update: immediate +0.02 pp, after next native update −1.76 pp; selection inflates the immediate step; target/OOD loss > retention loss | Weights only, H=2, two-example LoRA, 3–8B models; mechanism unresolved | Methodological template (paired G1/G2, no-update baseline) for a harness version |
| 8 | Flex-RSI: Benchmarking RSI of Frontier Agents | 2611.08946 | 2026-11-06 | Improver edits executor *and itself*; paired frozen vs self-improving harness: +2 to +40 in 6/7; transferred harness/improver sometimes worse than fresh | Few pairs, single seeds, vision/robotics; warm-start comparison confounded by model swap | Accidental evidence of a warm-start penalty; benchmark competitor |
| 9 | From AutoResearch to Continual AutoResearch | 2611.05108 | 2026-11-03 | Persistent vs reset agents at matched budget: persistent state improves efficiency, not final quality; memory ablation | Actor-level; 5 frontier agents | Template for the persistent-vs-reset comparison |
| 10 | Continual Learning Bench | 2606.05661 | 2026-06-04 | 6 expert-validated stateful domains; naive full-context ICL often strongest | Short-horizon gain vs reset | Baseline benchmark (27 cites) |
| 11 | Measure Learning at Steady State: BIRD-SQL F1 | 2609.31640 | 2026-09-11 | Over a long horizon ICL context grows to ~95k and cost doubles; short-horizon gain misleads | Single case study | Supports "accumulation tax" |
| 12 | AgentCL: Rigorous Evaluation of CL in Language Agents | 2606.02461 | 2026-06-01 | Controlled cross-task relations, CL metrics | Memory-augmented agents | Metric prior |
| 13 | Continual Learning Exam (CLE) | 2610.33956 | 2026-10-25 | 120 expert tasks × 5-level ladders, 89 roles | LLM-judge rubric | Benchmark competitor ("long-horizon RSI") |
| 14 | PATH-Bench: Path-Dependent Evaluation of Lifelong Agents | 2608.01149 | 2026-08-02 | Helpful vs interfering histories; forward/backward transfer | Memory/skill agents only | Order/path dependence prior |
| 15 | EmbodiedCLBench | 2610.13021 | 2026-10-09 | Compositional transfer from basic to advanced embodied tasks | Embodied | Benchmark competitor |
| 16 | RSI Tax | 2611.13524 | 2026-11-10 | Bounded-note baseline matches Meta-Harness/EvoSkill/RRSI at 65–71% fewer tokens; verification is 75–94% of the tax | Accuracy-centric | Threatens the confound battery; cites Agent Plasticity |
| 17 | Agent Self-Improvement with Frozen Weights: Failure Traces | 2611.11342 | 2026-11-08 | Failure-trace notes +4.0 pp over matched search without traces; search alone ≈60% of the gain; no gain in full conversations | One 9B model, customer-service domain | **Partially runs Agent Plasticity's "budget-matched no-feedback" future work** |
| 18 | Artifact-to-Capability | 2611.04952 | 2026-11-03 | Reflexion/Voyager/DGM on Gomoku all show the 3 proxy signals but distinct breaks; DGM shows "loss-only drift" after the checkpoint | Single game | Process diagnosis prior |
| 19 | Auditing the Product, Not the Recursion | 2610.27610 | 2026-10-20 | Coding of 114 RSI studies: process claims (improver gets better at improving) rarely evidenced | Meta-science | **Supports** the gap for improver-level measurement |
| 20 | Beneath the Surface: What We Overlook in Agent Evolution | 2611.07624 | 2026-11-05 | All 16 evolved agents gain new tasks while losing covered ones; the top scorer is not the biggest improver; stricter admission discards alternatives | 4 methods | Threatens confound battery |
| 21 | Masked Erosion: Acceptance Gates Lose Redundant Safeguards | 2610.38392 | 2026-10-28 | Gates erode redundant safeguards unseen; 411 loops × 10 gate rules | Safety-focused | Gate dynamics prior |
| 22 | Do agents need to sleep? (Haber & Tononi) | 2611.02216 | 2026-11-01 | Perspective: memory accumulation = unchecked plasticity; predicts selectionist > instructionist consolidation at scale | No experiments | Falsifiable prediction Dan could test |
| 23 | Let Them Sleep: Recursive Self-Pruning Memory | 2610.25561 | 2026-10-18 | Nearest-neighbor memory "quietly stops working" over long runs; pruning | Memory method | Intervention baseline |
| 24 | Set-shifting Behavioral Test for Harnessed Agents | 2607.13396 | 2026-07-15 | Some models latch onto a fixed routine within a few turns after hidden tool-reliability shifts | Within-session | Behavioral perseveration = in-context LoP analog |
| 25 | Can Scale Save Us From Plasticity Loss in LLMs? (Zyphra) | 2606.24752 | 2026-06-23 | Weight-level LoP in 5M–314M transformers; onset follows a sublinear scaling law | Small models | Weight-level LoP background |
| 26 | On the Primacy Bias in RLVR Training | 2611.04061 | 2026-11-02 | RLVR sharpens easy problems; primacy bias | Weights | Primacy bias in LLM RL |
| 27 | Learning Dynamics of Continual Learning: Attribution, Forgetting, Plasticity Loss (Oxford/GDM) | 2609.33620 | 2026-09-27 | Unified view for LM updates | Weights | Theory background |
| 28 | From a Prompt to Repertoires (REpertoires) | 2610.11373 | 2026-10-08 | Sequential prompt optimization forgets; prompts accumulate overfit rules | Prompt-only | Forgetting prior |
| 29 | Memory Tax | 2610.34073 | 2026-10-25 | Many memory systems < naive RAG | Memory | "Accumulated state can be net negative" |
| 30 | Skill^∞: RSI for Agent Skill Optimization | 2610.35434 | 2026-10-26 | Meta-skill optimizes itself | Method | Self-referential improver testbed |
| 31 | Where Should Learning Live? Regret Analysis | 2611.09701 | 2026-11-06 | Excess loss splits into selection regret, family gap and history effect | Theory | Formal tool for history effects |
| 32 | Recurrent Harness (ReHarness) | 2610.12668 | 2026-10-08 | Backward revision of reflections in the stream | Method | Baseline |
| 33 | HERD | 2610.31401 | 2026-10-22 | Harness plus drift-constrained distillation for CL | Method | Harness→weights CL |

Older background (in neither reading_list nor the main table): Dohare et al. Nature 2024 (CorpusId 259251905); Lyle et al. 2023 Understanding plasticity (2303.01486); Nikishin et al. 2022 Primacy Bias (2205.07802); EvaLearn (2506.02672); StreamBench (2406.08747); LifelongAgentBench (2505.11942); A-GEM (1812.00420).

---

## 4. New since the reading list

All rows above except the classical ones are absent from reading_list.md. The ten most decision-relevant:

1. 2610.25858 Are SE Agents CL?
2. 2611.03219 Continual coding-harness forgetting
3. 2611.12988 Third Eye
4. 2611.08946 Flex-RSI
5. 2611.05108 Continual AutoResearch
6. 2610.27610 Auditing the Product
7. 2611.11342 Failure traces vs matched search
8. 2611.13524 RSI Tax
9. 2605.12978 Useful Memories Become Faulty
10. 2611.02216 Do agents need to sleep?

Also new: Learn2Play (2610.08215), ServeLearnBench (2610.07792), ReScopeBench (2611.05450), SelfImprove-Bench (2611.02797), Routed CL (2610.38796), Memory-Centric CL (2610.29357), Reliable Knowledge Acquisition for CL Agents (2610.21739), Evolving to Match Capabilities (2610.33024), Harness Evolution as Learning (2609.36892), A Theory of Reliable Self-Evolution for Agent Harnesses (2609.08175), Repo2Skill-Evo (2608.21964), Lost in Implicit Routing (2610.18522), Third-party "Self-Evolving Agents Can Repair Failures but Struggle to Generalize" (2610.22984), Does Failure Feedback Direct Self-Modification (2610.14662), Elicitation or Expansion (2610.27889), Harness-Delta Attribution (2610.22466), Object-Oriented Harness (2610.28380), Harness-S (2611.04277), MetaExp (2610.21492), Gödel Forest (2609.36675), RSI on a Budget (2610.29121), RSIHub (2610.32253), The Self-Improvement Problem (2610.21646), Trustworthy RSI w/ Statistical Guarantees (2611.10787), Second-Order Problem Solving for RSI (2610.05701).

alphaXiv slug, not a numeric ID (unverified ID): "When, Where, and How Experience Becomes Structure: Mechanisms for RSI in LLM Agents" (alphaxiv slug 2611.recursive-self-improvement-llm-agents, 2026-11-04).

---

## 5. Critiques and limitations that recur across papers

1. **No reset / fresh-start control for the learned state.** Nearly every CL paper compares to the *seed* at time 0. None compares "learning task B from an evolved state" with "learning task B from scratch" at matched budget. That comparison is the definition of loss of plasticity. Continual AutoResearch is the exception at the actor level.
2. **Compute is not separated from experience.** Agent Plasticity states this itself. 2611.11342 finds about 60% of the gain comes from search alone. RSI Tax and Rethinking-Eval make similar points.
3. **Single model, single seed, small task pools.** Are SE Agents CL uses Gemini Flash only, the continual coding study uses 50 tasks at temperature 0, and Flex-RSI pairs have n=1. Reviewers will hit all of these.
4. **Actor and improver are conflated.** Agent Plasticity uses the same model to act and reflect. Are SE Agents CL and the continual coding study evaluate the *harness*, never the evolver's own trajectory. Flex-RSI lets the improver edit itself but never measures how the improver itself changes.
5. **The gate drives the dynamics but is treated as a constant.** The Meta-Harness gate freezes the workspace; Masked Erosion shows gates erode safeguards; Beneath the Surface finds strict admission discards alternatives. Gate strictness is an obvious knob on the stability-plasticity trade-off, but nobody varies it systematically against plasticity.
6. **Observational diagnosis.** Artifact reuse and conflict categorization are correlational. Calls for intervention studies (removal, forced invocation, pruning) are stated but not done.
7. **Short horizons.** 20–40 checkpoints and 2–4 blocks are too short to see slow plasticity decay. Steady State (2609.31640) shows short-horizon metrics mislead.

---

## 6. Open gaps

- **G1: forward loss of plasticity in self-evolving agents.** After k self-improvement steps on family A, is an agent slower or worse at learning a new family B than a fresh agent with the same budget? How does this scale with k, harness size and gate strictness?
- **G2: plasticity of the improver (meta-plasticity).** For self-referential improvers (Gödel-Agent, Hyperagents, Skill∞, Flex-RSI-style), does the improver checkpointed at generation g produce smaller gains on a *fresh* target than the generation-0 improver? Is the loss due to the improver's *procedure* (its self-edits) or its *memory/archive*? Nobody has crossed procedure and memory at the improver level.
- **G3: plasticity-restoring interventions for text and code state.** Harness analogs of continual backprop (utility-tracked re-initialization of low-use components), shrink-and-perturb (random pruning plus re-paraphrase), periodic resets with distillation (the primacy-bias recipe), and "sleep" consolidation. All of these would be *evaluated on plasticity*, not only forgetting. Haber & Tononi's selectionist > instructionist prediction is untested.
- **G4: the role of the gate.** Acceptance-gate strictness as the stability-plasticity knob: freeze rate vs plasticity vs forgetting.
- **G5: separating "is the experience used" from extra compute** (Agent Plasticity's future work). This is now partially done by 2611.11342, but only for one domain with a 9B model and without a learning-curve or plasticity framing.
- **G6: long-horizon procedural streams.** Hundreds of task-family switches are needed to see slow decay. Existing benchmarks are fixed and small. Dan's procedural τ-style generator fits this.
- **G7: plasticity as a post-training target** (Agent Plasticity's future work). This needs RL on the reflector and is expensive; MetaExp and Evo² partly approach it.

---

## 7. Threats to Dan's ideas

- **"Meta-agent plasticity" (Dan's interest): ALIVE, and the strongest lane in this topic.** No paper measures loss of plasticity in the forward direction or at the improver level. Risks:
  - Are SE Agents CL (2610.25858) owns the "stability-plasticity in harness evolution" phrasing, so frame the work as **forward plasticity / improvability vs reset**, not P-vs-S.
  - The GDM/Mila or Meta MSL groups could follow up quickly. Agent Plasticity's future-work list does *not* include LoP or meta-level plasticity, which lowers that risk.
- **Improver scaling (#1): ALIVE but should be reframed.** "Does a generation-g improver produce larger gains on fresh A0?" *is* the meta-plasticity question. Plasticity framing gives it a surprise either way: improvers that lose plasticity is a Dohare-style result, and improvers that learn to learn is the RSI result. Partial overlaps:
  - Flex-RSI Table 9 (anecdotal improver transfer)
  - Hyperagents transfer imp@50
  - Auditing the Product (shows the gap exists)
- **Confound battery (#2): LARGELY SCOOPED as a standalone contribution.** Overlapping work: RSI Tax (2611.13524), Failure-traces-vs-matched-search (2611.11342), Harness-Delta Attribution (2610.22466), Beneath the Surface (2611.07624), Elicitation or Expansion (2610.27889), Does Failure Feedback Direct (2610.14662), plus Rethinking-Eval and Harness Updating ≠ Benefit from the reading list. Keep it only as the controls section of another paper.
- **Procedure vs memory arm: ALIVE at the improver level.** Continual AutoResearch does a memory ablation for the actor only.
- **Self-generated curriculum (#3): not directly threatened here.** Are SE Agents CL shows task *ordering and interleaving* strongly changes outcomes (GEPA terminal advantage +5.8 blocked vs +0.9 interleaved). Dan's task-ordering ablation should cite it, and curriculum effects may be confounded with forgetting/interference. CL-Bench and PATH-Bench also show path dependence.
- **Ignition test / compounding: partially threatened.** Third Eye shows the 2-step decline for weights, and Artifact-to-Capability shows DGM's "loss-only drift". This shifts the prior toward "doesn't compound", which makes a well-controlled harness-level ignition test with a plasticity readout *more* publishable.
- **Benchmark (#D infra): crowded.** CLE, CL-Bench, AgentCL, Learn2Play, ServeLearnBench, ReScopeBench, EmbodiedCLBench, PATH-Bench, SelfImprove-Bench and Flex-RSI already exist. The procedural generator should be infrastructure, not the headline.

---

## 8. Proposed ideas

### Idea P1 (lead): "Self-improvers lose plasticity" — a Dohare-style test for harness evolution and meta-agents
- **Core question.** After k rounds of self-improvement, does an evolved agent (harness plus memory plus, for self-referential systems, the improver) learn a *new* task family more slowly or less well than a fresh agent at matched budget? Does this worsen with k?
- **Claim (one sentence).** Self-evolving agents exhibit loss of plasticity: each round of accepted edits lowers future improvability on new tasks. Learning from an evolved state underperforms a reset after roughly N rounds, and simple harness "resets" (pruning or shrink-and-perturb) restore it. The alternative outcome, positive forward transfer (learning to learn), is equally publishable.
- **Key experiment.**
  - Use a procedural sequence of task families: a τ-style generator plus BIRD/BFCL-style families.
  - Run 3 evolvers: GEPA-style, Meta-Harness-style and a Gödel/Hyperagents-style self-referential improver.
  - Checkpoint at k ∈ {0, 2, 5, 10, 20} families learned. At each checkpoint, inject a probe family B and run a fixed budget of learning.
  - Metric: plasticity ratio PR(k) = ΔS_B(from checkpoint k) / ΔS_B(from seed). Report the area under the learning curve plus Agent-Plasticity-style P per $.
  - Diagnostics: harness length, rule count, gate rejection rate, edit diversity, artifact reuse.
  - Use 5 seeds, 2 cheap models (Haiku-class and Qwen3.5-9B), and matched tokens throughout.
- **Why new.** Agent Plasticity, Are SE Agents CL and the continual-coding study measure gain from the seed or forgetting. None measures improvability conditional on prior learning against a reset. Agent Plasticity explicitly sets loss of plasticity aside.
- **Closest prior.** 2610.25858; 2610.08902; 2611.05108 (persistent vs reset, actor-level); 2611.12988 (weights, H=2); Dohare 2024.
- **Estimated cost.** $600–1,500 API; a pilot of 2 families × 3 checkpoints × 2 seeds costs about $50.

### Idea P2: Procedure or memory? Crossing improver procedure × accumulated archive to locate meta-plasticity
- **Core question.** When a self-referential improver gets better (or worse) at improving, is it because of its *rewritten procedure* or its *accumulated memory/archive*?
- **Claim.** In current self-referential agents, the improver's apparent learning to improve is carried almost entirely by accumulated memory, not by procedural self-edits. Memory is also what causes the plasticity decay. Alternatively, procedure edits carry it, which would be the first clean evidence of procedural meta-learning.
- **Key experiment.**
  - A 2×2 design on fresh targets: {gen-0 procedure, gen-g procedure} × {empty archive, gen-g archive}.
  - Add a placebo arm: a length-matched random archive.
  - Use 2 improver families (Gödel-Agent-style, Hyperagents-style) and imp@k on held-out target harnesses.
- **Why new.** Hyperagents and Flex-RSI let the improver edit itself but never factor procedure from memory. Continual AutoResearch's memory ablation is actor-level. Auditing the Product shows process claims are rarely tested.
- **Closest prior.** Hyperagents 2603.19461; Flex-RSI 2611.08946; Continual AutoResearch 2611.05108; Skill∞ 2610.35434.
- **Estimated cost.** $400–1,000.

### Idea P3: Restoring plasticity in text/code learners — resets, shrink-and-perturb and sleep for self-evolving agents
- **Core question.** Do the classical remedies for plasticity loss transfer to harness and memory state? Candidates: periodic resets with distilled notes (primacy-bias recipe), utility-tracked re-initialization (continual backprop), random pruning plus paraphrase (shrink-and-perturb), and selectionist "sleep" consolidation (Haber & Tononi). Can they restore improvability without catastrophic forgetting?
- **Claim.** A cheap utility-tracked pruning schedule moves self-evolving agents onto a better stability-plasticity frontier than any existing evolver. This also tests the Tononi prediction that selectionist consolidation beats instructionist (Mem0-style) consolidation as stores grow.
- **Key experiment.** Use the P1 protocol with intervention arms. Report the frontier in (PR, forgetting F, cost) space across 3 evolvers and 2 models.
- **Why new.** Existing consolidation papers (Let Them Sleep, Darwin-Claw, Use and Disuse, Skill-V) optimize task score or forgetting. None targets forward plasticity, and none tests Tononi's prediction.
- **Closest prior.** 2611.02216; 2610.25561; 2605.12978; Dohare 2024; Nikishin 2022.
- **Estimated cost.** +$500 over P1. Works best as section 5 of P1.

### Idea P4: The acceptance gate is the stability-plasticity dial
- **Core question.** How does acceptance-gate strictness (paired-test α, eval batch size, held-out vs train gate, keep-if-better vs always-accept) trade off improvability, forgetting, and winner's-curse overfitting over long horizons?
- **Claim.** Strict gates that look safe in short runs cause a long-horizon plasticity collapse: the workspace freezes. Lax gates cause forgetting. A gate whose strictness adapts to the estimated noise floor dominates both.
- **Key experiment.** 10 gate rules × 2 evolvers × procedural stream; measure freeze rate, PR(k), F and held-out gain.
- **Why new.** The Meta-Harness freeze appears only as a side observation in 2610.25858. Masked Erosion studies gates for safeguards, not plasticity. The Winner's Curse lead studies acceptance noise, not long-horizon plasticity.
- **Closest prior.** 2610.25858; 2610.38392; Winner's Curse (extra_leads, unverified ID); Beneath the Surface 2611.07624.
- **Estimated cost.** $400–800. Good as a section or ablation.

### Idea P5 (riskier): Actor plasticity vs improver plasticity — whose capability limits self-improvement?
- **Core question.** Agent Plasticity uses the same model to act and reflect. If actor and reflector models are crossed (weak/strong × weak/strong), is plasticity limited by the actor's ability to *use* artifacts or by the reflector's ability to *write* them?
- **Claim.** Plasticity is reflector-bound for strong actors and actor-bound (reuse failures) for weak actors. This explains Agent Plasticity's reuse findings causally. It is the "controlled actor–artifact comparisons" the authors name as future work.
- **Key experiment.** A 3×3 actor × reflector grid with open and cheap models on a cheap environment (procedural τ or small games). Add the forced-invocation and artifact-removal interventions the authors propose.
- **Why new.** This is stated future work in 2610.08902 and is not done anywhere. Harness Updating ≠ Benefit varies the evolver but not plasticity per cost or reuse.
- **Closest prior.** 2610.08902; 2605.30621; 2610.33024 (Evolving to Match Capabilities).
- **Estimated cost.** $300–800. Scoop risk is high, because the Meta authors may do it themselves.

**Recommendation.** P1 as the main paper, with P2 as its mechanistic section and P3/P4 as interventions and ablations. It ties directly to the mentor's Gödel Agent "complexity ceiling" and long-horizon self-improvement, and it reuses Dan's procedural generator as infrastructure. Run the $20–50 pilot first: does PR(k) at k=5 differ from 1 beyond seed noise?

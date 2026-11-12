# Extra leads from Dan's earlier S2-assistant searches (Nov 12, 2026). Unverified — check before citing.

Dan's core question (improver scaling): When a self-improving agent "improves", how much is (a) extra compute/retries, (b) search structure (archives, keep-if-better gating), (c) real meta-learning (improver gets better at improving)? Method: checkpoint improver at several generations, evaluate each on fresh held-out tasks vs compute-matched baseline, multiple improvers and seeds, plot improver scaling curves (gain per unit compute vs generation). Compounds, plateaus ("complexity ceiling"), or Goodharts?
Also already read (beyond reading_list.md): Exo (Google), Harness², SPICE.

Threads to search: (1) evaluating self-improvement (compute-matched baselines, contamination, validation overfitting / winner's curse in archive search, multi-seed variance, held-out transfer, anytime curves, meta-optimizer benchmarks: HarnessOpt-Bench, Evo-Bench, VeRO, Meta-Agent Challenge); (2) measuring "improving at improving"; (3) attribution of accepted edits (ablation, Shapley, hitchhiker edits); (4) harness↔weights loops (amplify-then-distill, expert iteration, matched-compute vs plain GRPO/RLVR); (5) self-generated curricula/open-endedness for LLM agents w/ frozen-model self-referential harness improver; (6) failure modes (diversity collapse, drift, reward hacking/evaluator gaming, regularizing RSI).

## Leads (S2 CorpusIds where known)
- **Agent Plasticity: Measuring Self-Improvement Through Experience** (2026; Harman Singh, A. Bakhtin, ..., Anirudh Goyal; CorpusId 292694189) — plasticity = cost-normalized efficiency of converting experience into persistent held-out gains; diagnoses generalization gaps and artifact bottlenecks. Dan wants to apply plasticity idea to META-agents.
- MetaSkill-Evolve: RSI via Two-Timescale Meta-Skill Evolution (2026)
- Recursive Self-Evolving Agents via Held-Out Selection (2026) — regression gating on disjoint held-out split
- **The Winner's Curse in LLM Self-Improvement Loops: Selection Noise, Lock-in, and Acceptance Rules** (2026) — threatens confound-battery novelty
- From Solo to Social Learning: Recursive Social Improvement in LLMs (2026)
- BenchTrace (2026; Failure Avoidance Rate) 288741657
- GDPevo; Beyond Endpoint Performance: Process-Level Evaluation of Self-Evolving Agents (2026)
- RSIBench-Data; Harness Evolution Hits a Ceiling
- Experience-Sensitive Game Learning (290984858); Continual Learning Bench (Asawa, Gonzalez; 288977039); Beyond Final Scores long-horizon AI R&D eval (291084490); EmbodiedCLBench (292828527)
- MemSkill (285269715); Recursive self-improvement of AI research agents (Srikanth, Bingchen Zhao, Zhengyao Jiang — = AIDE²; 292227211)
- Truly Self-Improving Agents Require Intrinsic Metacognitive Learning (Liu & van der Schaar, ICML 2025; 279244233)
- Group-Evolving Agents (285285990); Critique of Agent Model (Eric Xing; 289626079); Self-Improving LLM Agents at Test-Time (281950440); TT-SI (ACL26; 289766446)
- Learn Like Humans: metacognitive reflection (ACL26); Rethinking Experience Utilization in Self-Evolving LM Agents (288110982); **LLM Agents Are Not Always Faithful Self-Evolvers** (285240886; causal interventions on whether experience is used)
- Aspire: self-evolve from vague goals (291542942); S3Gym (291542162); RSI-Forge (292694017); PAST-Bench (290836900); VibeTraining-Bench (293805195)
- Toward Recursive Self-Improvement in Physical AI (293857079); Dream-RSI (292025925); SelfSearch reward-free (292474846); Evolving Agents in the Dark (EMNLP26; 288976557); MetaExp (293003768); Self-Evolving Coding Agents (290833491); Evolving-RL (288259194); Chain-of-Experience (291188392); SBCO (291020860); ReCreate (ACL26); Knowledge-Centric Self-Improvement (290443195); SAGE (288903953); Audited Skill-Graph (284350534); Training agents for reward-free self-evolution via world knowledge exploration (287635393)
- Older: Contextual Experience Replay (ACL25); EvoTest (ICLR); Agent Learning via Early Experience; Better than Your Teacher (ICLR24); Reflexion; SICA; Self-Evolving Agents survey (Gao et al. 2025, 280323947); Learning on the Job; RL for Self-Improving Agent w/ Skill Library (ACL25).

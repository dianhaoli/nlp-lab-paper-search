# Ideas so far (Dan, as of Nov 12, 2026)

Mentor: Xunjian Yin (Duke PhD, co-advised by Shuyan Zhou & Bhuwan Dhingra; Student Researcher at Google DeepMind since May 2026).
His agenda: how AI systems keep improving themselves over long horizons — agents that rewrite their own logic, learn from experience, stay reliable when the environment changes. Focus: **self-referential agents and world models**.
His recent papers: Gödel Agent (ACL 2025, 2410.04444); WeAct: Dynamic Weight Switching as Agent Actions (2610.26847); Constructing Challenging Browser-Use Tasks by Controlled Environment Interventions / BreakingWeb (NeurIPS 2026, 2609.35814); LEDOM reverse LM (ACL 2026, 2507.01335); Geometry of Reasoning (ICLR 2026, 2510.09782); earlier: knowledge editing, knowledge boundary, ALCUNA, Themis.
Target: NeurIPS 2027 main track (deadline ~mid-May 2027). Preferred themes: meta-agents, self-improvement, possibly benchmarks.
Budget reality: student; API budget hundreds to low thousands of $; GPU for weight-based work is the expensive tier.

## A. Measurement papers (strongest lane)
1. **Improver scaling** — Does a later meta-agent checkpoint (iterations 0/10/20/40) produce bigger gains from the same starting harness A0 at fixed compute, measured by imp@k across checkpoints? ~$150 on Haiku. Risk: flat/noisy.
2. **Confound battery** — Do reported self-improvement gains survive matched-budget sampling, placebo edits, length matching, seeds, OOD eval? Natural target: Hyperagents transfer result (imp@50 = 0.630). ~$200. Workshop-level alone.
3. **Self-generated curriculum for harness evolution** (current lead) — Self-generated curricula work for RL on weights; do they work when the learner is a harness?
   - Data multiplier: n_eff(n) defined by S_real(n_eff) = S_gen(n), n ∈ {5,10,30,60}.
   - Decomposition: does more data help a harness at all (S_real(m) vs S_real(n)); generator quality (S_gen(n,m) vs S_real(m)); net gain (S_gen(n,m) vs S_real(n)).
   - Arms: fixed real; static generated; adaptive generated w/ frozen OMNI-style writer; anchored (R-Few style real mix-in); unified (Hyperagents writes its own tasks — Hyperagents' stated future work).
   - Side ablation: task ordering. Measure curriculum variance explained by validity, diversity, difficulty, distance-to-real.
   - Risks: statistical power (τ-bench pool small); single evolver (use GEPA-style + Hyperagents-style); overlap w/ DSPy, GEPA, DarwinX synthetic-to-real.
## B. Smaller experiments inside #3
4. Task-selection baselines (random, uniform, learning-progress bandit, frontier band). ~$300.
5. Anchoring ablation (0, 1–5%, more real tasks). ~$150.
## C. Weight-based (expensive)
6. Multi-round harness→weights compounding: evolve harness, distill into weights, repeat ≥5 rounds; does Δ_t grow or shrink? $1–10k GPU.
7. Co-evolutionary Goodhart: generator+solver co-evolve; drift of train score vs true held-out score. $4–15k.
8. Regret-style (PAIRED) task generator for LLM agents. $5–20k. Deferred.
## D. Infrastructure
- Procedural τ-style task generator (sample DB state + goal + policy rule; compute verified end state) → unlimited held-out tasks; fixes #3 power. Build as infra, not standalone benchmark (SEAGym, EvoAgentBench, GDPevo crowd that).
## Plan so far
~$20 noise-floor run + S_real(n) at n=5,10,30 decides paper: rising → curriculum live; flat → "harness learners saturate"; noisy → switch benchmark / build procedural generator. Main: #3 with #2 controls + #1 secondary.

## Other ideas from earlier chats
- **Hyperagents meta-transfer replication** under controls (whole agent carried over; generic infra like memory/tracking; code length; extra tokens; only 5 runs at single k). Nobody replicated; open models reportedly can't drive Hyperagents.
- **2×2 crossing decomposition** per round: old/new harness × old/new weights + placebo adapter → harness Δ, weight Δ, interaction (incl. HELIX's untested held-vs-rebuilt harness).
- **Survival under model upgrades**: fraction of evolved-harness gain absorbed by a newer model in same family (Qwen3→Qwen3.5); longitudinal version open.
- **Which lever when**: bandit/meta-RL scheduler choosing harness edit vs weight update vs Ceiling paper's process/content rule at matched compute. Medium-high scoop risk (SIA flagged).
- **Oversight after distillation**: once harness behavior distilled into weights, do harness-level audit trails still catch it? Safety venue.
- **Ignition test**: fixed improver vs self-modifying across Gödel Agent, AIDE², Hyperagents, Metaⁿ; does improvement accelerate across generations? "editable beats frozen" already shown; compounding open (AIDE² inconclusive, Hyperagents n.s.). Framing proposed elsewhere; contribution = execution.
- **Procedure vs memory arm**: separate real improvement of the procedure from accumulated memory/information. "When does self-reference pay off, and is it the procedure or the memory?"
- **Compute vs search vs meta-learning**: checkpoint the improver to separate extra compute, search structure, genuine meta-learning. Root of #1, #2.
- **Goodhart curve / ranking flip as the "surprise"** — a measurement paper needs a surprise.
- **Gödel Agent "complexity ceiling"**: does self-modification stall at some complexity? Combine with improver scaling.
- **Amplify-then-distill / expert iteration** for harnesses (basis for #6).
- Dan also likes: "Rethinking the evaluation of harness evolution" style matched-compute rigor; and the new **"meta-agent plasticity"** line (loss of plasticity / plasticity applied to meta-agents / self-improvers).
- Separate lines (not for this paper): Claudish Snowball (language drift in long multi-turn convos); RL environments (τ-bench RLVR, EditHunt, unplant).

## Clusters
1. Does improving the improver help? (improver scaling, ignition, procedure vs memory, Hyperagents replication, confound battery) — closest to Gödel Agent.
2. Does self-generated data help a harness learner? (curriculum + decomposition)
3. What happens when gains move into weights? (compounding, 2×2, which lever, upgrade survival, Goodhart)

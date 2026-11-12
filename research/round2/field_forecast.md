# Field forecast: self-improving and meta-agents through NeurIPS 2027

*Written 2026-11-12 (round 2). Sources: Semantic Scholar bulk-search counts (16 subtopics × 4 windows), 8 alphaXiv recency/default searches, and the round-1 and round-2 verification files (`verify/*.md`, `REPORT.md`). Every paper ID below appeared in a tool result this session or in a round-1/2 file. Use this file to pick an idea that sits where the field is about to arrive, not where it already is.*

---

## 1. How fast each subtopic is growing (S2 counts)

**Method.** I counted S2 `/paper/search/bulk` hits using boolean phrase queries, in four windows: Q1 (Jan–Mar), Q2 (Apr–Jun), Q3 (Jul–Sep) and Q4-to-date (Oct 1–Nov 12, about 1.4 months). Counts are shown **per month**.

**Caveats.**
- The queries are lexical, so the counts are noisy. Read the direction and ratios, not the absolute numbers.
- S2 indexes new papers late, so the Q4 numbers are *under*-counts. A rising Q4 figure is therefore strong evidence.
- These are my queries, not a curated census.

| Subtopic (query gist) | Q1 /mo | Q2 /mo | Q3 /mo | Q4 /mo (partial) | Shape |
|---|---|---|---|---|---|
| "recursive self-improvement" | 2.3 | 4.3 | 27.3 | **95.7** | **Explosive** (≈40× in 9 months; the name went mainstream in Sep–Oct) |
| harness + evolution/optimization + agent | 25.7 | 60 | 98 | **160.7** | Accelerating, high volume |
| reward hacking / spec gaming + agent | 7.7 | 20.3 | 16.7 | **28.6** | Accelerating again |
| user simulator + agent | 5.7 | 10 | 5.7 | **15** | Spiking (Oct fidelity cluster) |
| harness/experience → weights distillation (self-evolving) | 3.7 | 7.7 | 7 | 11.4 | Rising |
| test-time learning/training + LLM agent | 2.3 | 3.3 | 3.7 | 7.1 | Starting to accelerate |
| plasticity / continual learning + "LLM agent" | 1 | 2 | 3 | 4.3 | Small but doubling each quarter (*just starting*) |
| "self-evolving agent(s)" | 10.3 | 26.3 | 26.3 | 27.1 | **Plateau**: the name is being replaced by "RSI" and "harness evolution" |
| skill library + LLM agent | 2.7 | 15.7 | 13.7 | 15 | Plateau after the Q2 boom |
| AI scientist / research agent | 18 | 28 | 21.3 | 26.4 | High and flat |
| self-generated tasks/curricula (self-evolving) | 6.7 | 7.3 | 5.7 | 6.4 | Flat |
| multi-agent evolution / co-evolution | 29.7 | 35 | 24.7 | 17.1 | Declining (as a label) |
| world model + LLM/web agent | 2.3 | 5.7 | 7.3 | (429 error) | Rising |
| self-modification + safety/alignment | 1.7 | 3 | 1.3 | 1.4 | Flat, small |
| "meta-agent" + LLM | 2.3 | 2.3 | 3 | 2.1 | Flat (the term never caught on) |
| "self-referential" + agent | 0 | 2 | 1 | 1.4 | Tiny. Gödel-style self-reference is *under-named*. |

**Reading.** The money and attention went into **RSI-branded harness evolution** in Q3 and Q4. Labels that are plateauing ("self-evolving agent", skill libraries, curricula, AI scientist) mark *saturated method lanes*. Lanes that are small but doubling (plasticity, test-time learning for agents, simulator fidelity, reward hacking in self-evolving loops) are where the **next wave of papers** will land around ICML/NeurIPS 2027.

---

## 2. Trend table and the "next obvious paper" in each lane

| Subtopic | 2026 trajectory (evidence) | Saturation | Next obvious paper (avoid) | Scoop risk |
|---|---|---|---|---|
| **Harness evolution methods** | Many method papers: Harness² 2610.14224, HarnessPro 2610.26770, HarnessMM 2610.32813, Harmony 2610.15150, HGM-Dual 2610.30042, Mixture-of-Branches 2609.37834, Self-Evolving Agent Harness 2610.18813, SEA terminal 2610.27435 | **Saturated** | A multi-objective / Pareto / lineage-aware harness optimizer with +X on TB2/SWE-bench | Very high |
| **RSI evaluation / auditing** | 2610.27610 census; Beneath the Surface 2611.07624; HDA 2610.22466; 41-paper methods audit (alphaXiv 2609.self-improving-llm-agent-evaluation-audits); Flex-RSI 2611.08946; RSI-Index; RSI-Env 2611.01736; SelfImprove-Bench 2611.02797 | **Saturating** (audits and benchmarks) | (a) The "swap test actually run" paper: improver × state at matched compute. (b) Another RSI benchmark. | High for (a), ~Feb–Apr 2027 |
| **Improver-level ("evolve the evolver")** | MetaRSI 2609.06396, RSIFlow 2610.21791, Evo² 2610.13025 (GDM), HMED 2610.07979, SelfSearch 2609.37968, FreeEvolve 2610.09197 | Accelerating | Evolve-the-evolver method plus a small swap ablation, by a big lab | High |
| **Plasticity / continual learning of agents** | Agent Plasticity 2610.08902 (Meta MSL), Are SEAs Continual Learners? 2610.25858 (Mila/GDM), Do agents need to sleep? 2611.02216, 2611.03219 coding-agent forgetting, 2605.09315 | **Just starting** (doubling) | RL post-training for plasticity; consolidation/"sleep" mechanisms; actor-vs-artifact swaps (all named as future work) | High: the authors themselves named these |
| **Self-generated envs / curricula** | ActiveSaddler, Task-CoEvolve, RSI-Env, R2Orch 2610.22740, RoboACE 2611.13034 | Saturated for methods | A per-generation proxy–truth audit of a self-written curriculum | Medium |
| **World-model / simulator scoring of agents** | Dream-RSI 2609.14858, 2608.12253, 2611.01139, 2610.08033, user-sim fidelity cluster (MIMESIS 2610.09484 and others) | Accelerating | Simulator-fidelity knob → policy collapse curve (weight RL) | Medium (RL); low for harnesses |
| **Harness → weights / co-evolution** | HERD 2610.31401, CoEvo-RSI 2610.25078, JoMoHa 2611.11499, Evo² 2610.13025, HPO 2610.22791, ClawLoop 2610.14118, Adaptive Memory-Parameter 2608.01234 | **Saturated** | Multi-round co-evolution with a 2×2 crossing | Very high |
| **Reward hacking / Goodhart in self-evolving loops** | Masked Erosion 2610.38392, PRIME 2606.09711, 2611.08318, 2611.04838, SEABench 2609.35596, harness tampering 2609.00069, appreciation→injection 2610.39122 | Accelerating | Benchmark of endogenous exploit or misalignment in harness evolution | Medium-high |
| **Verifier / critic co-evolution, reward-free RSI** | When the Critic Falls Behind 2610.36826, Who Verifies the Verifier 2610.11464, SelfSearch (reward-free), RHO 2606.05922, multi-agent self-supervision 2610.12176 | **Hottest method lane** | Co-evolving rubric/verifier + harness method | Very high |
| **Multi-agent / social / population self-improvement** | GEA 2602.04837, Solo-to-Social 2609.38516, EvolveNet 2608.04968, Population-Centered 2610.36660, Homogenization in MAS 2610.09824 | Plateau (method); measurement thin | Token-matched population vs solo comparison | Medium |
| **Automated AI research (RSI of research agents)** | Weco 2609.26457, Aligning Autoresearch 2610.38882 (FAIR), Station 2610.08927, RSI-Opt 2610.32026, Organon 2610.33046 | High, flat | Cross-task carry-over of research skill (Organon v2, RSI-Opt v2) | High |
| **Theory / economics of RSI** | Economics of RSI 2609.15802 (METR/Epoch), A Theory of Reliable Self-Evolution 2609.08175, Harness Evolution as Learning 2609.36892, Self-Improvement Problem 2610.21646, Closed-Loop Saturation 2607.14185, TRUST 2611.10787 | Starting | PAC/generalization bound for harness evolution | Low-medium (theory people are few) |
| **Safety of self-modification** | Evolutionary Safety 2609.31186, Is RSI Safe? 2611.10261, Compositional Safety Failures 2609.33123, Trusting Trust Revisited 2609.17817, Safe Evolution with Circuit Anchors 2608.05158 | Flat, small, mostly position/benchmark | Endogenous-misalignment benchmark v2 | Medium |
| **Bio-evolution lens** (evolvability, Lamarckian vs Darwinian) | BONSAI evolvability-guided skill search 2608.07056, Darwinian/Lamarckian/Agent-Directed 2611.07048 (MIT, essay), EvoUndo recoverability 2608.28363, HarnessBank "gene bank" 2607.13683 | **Barely started**, used only as metaphor | A "Lamarckian vs Darwinian harness evolution" comparison | Low |
| **Reliability under environment change** | RSIAgent 2609.15364, Darwin-Claw 2610.23574, Adaptive Memory-Parameter 2608.01234, BreakingWeb 2609.35814 (Xunjian) | Starting | "Evolved harness X% degrades under API/UI drift" benchmark | Medium |

---

## 3. Predicted developments by May 2027 (NeurIPS 2027 deadline)

1. **A powered improver × state swap paper appears, probably from a big lab or the 2610.27610 / Flex-RSI / MetaRSI groups.** Confidence **75%** by May 2027, 50% by Feb. The pieces are public: SelfSearch's Table 4, MetaRSI's same-start swap, Evo² "evolving the evolver", and the census naming the experiment. → The *bare* Effect/Cause question of #1 will look incremental. Only Decay (cross-domain warm-start penalty) and mechanism-level claims stay fresh.
2. **"RSI" becomes the dominant label and gets a NeurIPS 2027 workshop or Datasets & Benchmarks cluster, with ≥5 more RSI benchmarks.** Confidence **85%**. → Benchmark papers will be crowded; reviewers will expect reported compute matching and seeds as table stakes.
3. **Plasticity / consolidation for agents becomes a named sub-field.** Expect RL-for-plasticity, "sleep"/consolidation and forgetting-vs-archive papers from Meta MSL, Mila/GDM and Berkeley. Confidence **70%**. → Actor-level plasticity is gone. Improver-level (meta) plasticity stays open but is ~6 months from being reached.
4. **Verifier/critic co-evolution and reward-free RSI become the leading method lane, followed by the first critiques showing that co-evolved verifiers collude or drift.** Confidence **65%**. The vacuous-verifier observation (2610.11464) and RHO's acknowledged self-preference risk are the seeds. → "Self-preference drift" and placebo-evaluator controls are timely now and crowded by summer.
5. **Simulator-fidelity → policy-collapse studies move from weight RL to LLM agents with user simulators.** The 2610.08033 author or the MIMESIS cluster runs the causal per-quirk intervention. Confidence **55%** for weights; **30%** for harness evolution specifically. → Evolve-in-dream keeps a window if it is harness-level and causal.
6. **Harness-evolution methods saturate.** Gains reported on TB2 / SWE-bench get contested; replication papers show 3–5× disagreement (already visible on TB2.1) and an "it's mostly prompt/memory edits" finding (SelfImprove-Bench: 87.9% of retained edits are prompts/memory, 0% improver). Confidence **70%**. → Mechanism-level claims about *what gets edited and why* are valued.
7. **Safety framing shifts from "can self-modification be harmful?" to "selection pressure erodes safeguards"** (Masked Erosion, compositional failures), with at least one frontier-lab safety paper on harness-level RSI. Confidence **55%**.
8. **Biology and economics lenses stay metaphorical.** Few papers will *test* quantitative predictions from evolutionary theory (error thresholds, canalization, the Baldwin hiding effect) on LLM improvers. Confidence **75%**. → This is the low-scoop zone (§4).

---

## 4. Non-obvious opening questions (low scoop risk, still impactful)

Each question uses a lens most RSI groups don't have. Each is falsifiable and runnable on a student API budget unless noted. ★ marks the ones that fit Xunjian best (self-reference, world models, reliability under environment change).

### Q1 ★ An error threshold for self-modification (Eigen's error catastrophe applied to Gödel-style agents)
- **Claim.** Every self-edit has a per-token defect rate ε. Once the agent's own code or prompt grows past a critical length L* ≈ ln(s)/ε, selection can no longer keep improvements: performance collapses or random-walks.
- **Testable predictions.**
  - L* falls when ε is raised. Raise ε by injecting edit noise or using a weaker editor.
  - L* rises with selection strength s, set by evaluator batch size.
  - L* gives a quantitative *complexity ceiling*, which is Gödel Agent's open limitation.
- **Why underexplored.** People talk about "harness bloat" and "complexity ceilings" anecdotally. HarnessMM and HarnessPro optimize cost, not a phase transition. Nobody fits a threshold law.
- **Cost.** Cheap: sweep injected noise × gate size.
- **Surprise.** A sharp phase transition, where the ceiling is set by the evaluator, not the model.

### Q2 ★ Canalization: does evolution make harnesses brittle to environment interventions?
- **Claim.** Selection on a fixed environment *decanalizes* artifacts: gains go up while robustness to small environment changes goes down.
- **Test.** Use BreakingWeb-style controlled interventions (UI or API perturbations). Measure the robustness slope over evolution generations, compared with hand-written harnesses at equal clean accuracy.
- **Why underexplored.** Robustness papers (RSIAgent, Darwin-Claw) adapt *after* drift. Nobody measures whether the evolution process itself *creates* fragility, as a dose-response in generations.
- **Fit.** Uses Xunjian's own tool. Low scoop risk.

### Q3 The Baldwin "hiding effect": does a self-improving harness *reduce* what weight RL learns?
- **Claim.** In biology, plasticity shields genotypes from selection. Prediction: RL on a model wrapped in an adaptive harness (memory, reflection) gives a *weaker* base model, judged harness-free, than RL with a frozen harness.
- **Why underexplored.** Co-evolution papers (Evo², CoEvo-RSI, HPO) measure the *joint* system and never the harness-ablated weights.
- **Cost.** Small-model GRPO, so GPU tier.
- **Surprise.** The more self-improvement you add, the less the weights learn. This is the opposite of the "amplify-then-distill" intuition.

### Q4 ★ Introspective drift: do self-referential improvers spend their edits on themselves?
- **Claim.** Over generations, the share of edits targeting self-inspection tooling (trajectory readers, log filters) rises while those tools' downstream call-coverage stays near zero. SelfSearch gens 6–10 are an anecdote of this.
- **Readout.** A drift index computed from call coverage. Its relation to Gödel-agent stalls is the falsifiable link.
- **Why underexplored.** Edit taxonomies exist (SelfImprove-Bench), but none track self-directed edits over time.
- **Fit.** Cheap. A standalone short paper or a section of #1.

### Q5 Information-theoretic ceiling: bits of feedback bound improvement
- **Claim.** An evaluator with n tasks and binary outcomes leaks at most about n·H bits per generation. Cumulative real held-out gain should then saturate as a function of *total feedback bits*, not of generations or compute. Beyond that, gains are overfitting.
- **Why underexplored.** Theory papers (2609.36892, 2609.08175) give generalization bounds but never check them against a feedback-budget curve. Empirical papers treat compute as the x-axis.
- **Cost.** Cheap with a procedural task generator: vary n and the feedback granularity (binary, score, full trace).
- **Surprise.** Trace feedback may not beat binary feedback per bit.

### Q6 Placebo-evaluator control (from the round-2 digest, N1)
- **Design.** Real vs shuffled vs always-pass vs no evaluator, at matched compute, over many generations.
- **Prediction.** For current improvers, much of the gain survives a placebo evaluator, so "self-improvement" is largely prior-driven refactoring.
- **Why open.** The vacuous-verifier observation (2610.11464) and SelfSearch's reward-free success are side findings. Nobody has run the controlled dose test.
- **Scoop risk.** Medium: it is timely and someone will do it by summer.

### Q7 Improver monoculture and post-shift evolvability
- **Claim.** Shared archives (GEA, EvolveNet) raise in-domain scores but collapse *meta*-diversity, so the population adapts more slowly after a domain shift.
- **Lens.** Ecology and evolvability. The literature has homogenization in MAS (2610.09824) at the object level, and Population-Centered 2610.36660 is a method paper.
- **Why underexplored.** Nobody measures diversity of improvers and post-shift recovery rate.
- **Cost.** Cheap to moderate.

### Q8 ★ Support-Goodhart in "dreaming" (from leads-b)
- **Claim.** Zero-model-error replay simulators (Dream-RSI) still Goodhart. They reward policies that imitate the logging policy, because off-support actions can't be scored.
- **Design.** Three simulator classes (replay, learned, quirk-injected) give a "model error vs support error" decomposition for harness evolution.
- **Why underexplored.** The fidelity literature thinks only about model error.
- **Fit.** World models.

### Q9 Goodhart-type decomposition over generations (Manheim–Garrabrant taxonomy made quantitative)
- **Claim.** Early generations show *regressional* Goodhart (selection noise, winner's curse); late generations show *extremal / causal* Goodhart (exploiting evaluator structure).
- **Test.** Separate the two with a re-test on fresh samples (removes regressional) and an artifact-patched evaluator (removes causal).
- **Why underexplored.** Papers report one undifferentiated "overfit gap" (RSEA 33 pt, FreeEvolve Haiku). Decomposing it says *which fix* (bigger gate or better evaluator) helps when.
- **Cost.** Cheap.

### Q10 ★ Improver–executor coupling (co-adaptation lock-in)
- **Claim.** A gen-k improver co-adapts to its executor model. Swapping the executor penalizes the evolved improver more than gen-0: a "home-model penalty", hinted by RSI-Opt 2610.32026's tables.
- **Lens.** Co-evolutionary lock-in and coevolved mutualism.
- **Why underexplored.** Executor-transfer tables are object-level, and nobody asks whether *improvement skill* is executor-specific.
- **Cost.** Cheap. Extends WeAct-style weight switching: switching the executor's weights mid-run is a clean intervention.

### Q11 Backward / retrodictive credit assignment for improvers (LEDOM lens)
- **Claim.** An improver that writes edits *backward* — first stating which past failure the edit must flip, then the edit, as a hindsight contract — gets better same-state Δ credit.
- **Readout.** The rate of sign disagreement between branch outcome and matched Δ. HMED reports 45–75% disagreement.
- **Prediction.** Retrodiction accuracy predicts which edits survive held-out.
- **Why underexplored.** Credit assignment for self-edits is treated as a search problem, not a reverse-reasoning one.
- **Cost.** Cheap. This is the riskiest of the list on surprise.

### Q12 Control-theoretic instability: evaluation delay and gain
- **Claim.** Self-improvement loops with delayed or batched feedback show oscillation (accept/revert cycles, the "false-floor loops" in the Verified Archives paper). Theory predicts a stability boundary in (step size = edits per generation) × (feedback delay).
- **Test.** Sweep both on a cheap testbed. Predict where oscillation starts.
- **Why underexplored.** No paper treats the improver as a feedback controller. Oscillation is reported as an anecdote.

---

## 5. Strategic takeaways for Dan

- **Avoid**: new harness or co-evolution methods, generic RSI benchmarks, actor-level plasticity, a bare improver swap test, verifier co-evolution methods. All are crowded, or will be by Feb 2027.
- **Anchor the lead paper (#1) on what the wave won't reach by May.** That means Decay (warm-start penalty) plus one mechanism claim taken from §4. Q1 (error threshold), Q4 (introspective drift) and Q10 (home-model coupling) all fit inside #1's runs as extra readouts at little extra cost.
- **Best standalone non-obvious bets by fit × surprise × scoop risk:**
  1. **Q1 error threshold.** Sharpest law-like claim. Directly tests Gödel Agent's ceiling.
  2. **Q2 canalization under environment interventions.** Uses BreakingWeb; reliability is Xunjian's stated agenda.
  3. **Q9 Goodhart decomposition.** Cheapest; it upgrades Learning-to-Goodhart.
  4. **Q3 Baldwin hiding effect.** The most surprising, but it needs a GPU.
- **Timing.** Post the preprint by Feb–Mar 2027. Re-run the OpenReview ICLR 2027 check once the host is reachable, and the workshop accepted lists after Dec 4.

## Caveats
- S2 counts come from lexical queries with indexing lag. Treat them as directional only.
- Most papers cited are unreviewed 2026 preprints.
- Confidence numbers are judgment, not calibrated forecasts.
- Several alphaXiv-only IDs use non-standard slugs (for example `2609.self-improving-llm-agent-evaluation-audits`). Verify them before citing.
- Papers I named from search results but did not read in full: 2611.07048, 2608.07056, 2608.10178, 2610.28815, 2611.01304, 2607.14185, 2609.15802. Their claims here rest on abstracts and snippets only.

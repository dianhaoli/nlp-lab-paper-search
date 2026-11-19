## Harness ↔ weights: co-evolution, distillation, Baldwin effects

*Marks: ✓ = checked in the full text, this session or in FULLTEXT_VERIFICATION.md, with section or table cited. ◐ = from an abstract only (arXiv, S2 or OpenReview). † = taken from earlier project notes and not re-checked. Full texts are cached under `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/<id>/paper.txt`.*

**Read in full this session (method, results, limitations):**
- Harness-Zero, Harness Evolution Hits a Ceiling, CoEvo-RSI, HERD, JoMoHa, Harness Annealing
- CODA, Evo², SIA, WHALE, HarnessAdapt-RL, Harness-S
- Co-Harness, HELIX, Self-HPO (Tables 1–3), Knowledge-in-the-Harness (2611.15268)

**Read in part:** HASE, WeAct, SEAL, and the surveys 2607.13104 and 2609.11873 (the cited sections only).

**Fresh searches run:** 4 alphaXiv recency queries, 9 ICLR-2027 OpenReview queries, and S2 forward citations of 4 papers. Harness-Zero and CoEvo-RSI are not yet indexed by S2; WHALE has 20 citers. The adversarial fact-check added further OpenReview lookups to confirm the ICLR IDs of 8 papers.

### At a glance
- **What this corner is.** It covers four kinds of loop:
  - **Co-evolution:** a harness (prompts, tools, memory, middleware, control logic) and model weights are improved in one loop, either alternating or jointly.
  - **Distillation:** behaviour that the harness induces is moved into the weights so the harness can be removed.
  - **Improver weights:** the weights of the model that *writes* harnesses are trained (Evo²).
  - **Baldwin question:** does a supportive harness during training *help* the weights learn ("assist"), or does it *substitute for* learning ("hiding", i.e. harness dependence)?
- **Size.** As a *method* lane it is saturated:
  - about 25 arXiv method papers between Jul and Nov 2026;
  - more than 12 ICLR 2027 co-evolution submissions (round-2 sweep †);
  - 9 more ICLR 2027 harness-distillation or internalization submissions found this session ◐.
- **State of the art, by setting:**
  - **Distillation, single round.** Harness-Zero: Qwen3.5-9B with the specialized harness removed scores 44.3 macro-average. That beats the base model *with* the evolved harness (41.7) ✓ (Table 2).
  - **Joint RL.** JoMoHa reaches 97.0% on ALFWorld ✓.
  - **Multi-round.** CoEvo-RSI runs 10 rounds and averages over 3 runs ✓ (§4.1).
  - **Long-horizon continual learning.** HERD runs 25 stages × 3 seeds and deploys bare weights ✓.
- **Consensus 1: one-sided optimisation usually leaves gains on the table.** The joint method beats its one-sided arms in WHALE, JoMoHa, CoEvo-RSI and HarnessAdapt-RL ✓. Two caveats:
  - SIA has **no weight-only arm**. It compares only harness-only against harness+weights ✓ (Table 3).
  - In Self-HPO, the harness-only Meta-Harness baseline essentially ties the joint method on SWE-V (69.1±1.9 vs 69.5±0.9). The joint method leads only on the 3-benchmark average (63.5 vs 64.8) ✓ (Table 1).
  - No paper matches compute across arms.
- **Consensus 2: naive coupling breaks.**
  - Alternating updates can collapse: JoMoHa falls from 90.5 to 31.5 ✓.
  - Co-evolved pairs become mutually specialised (CODA ✓; CoEvo-RSI's "brain–body mismatch" ✓).
  - Plain SFT on trajectories collected under a supportive harness creates harness dependence (Harness Annealing ✓, TAME ◐, harness entanglement l8aeyLANkN ◐).
- **Emerging regularity, the Baldwin sign.** Support *assists* weight learning when three conditions hold: it is gated to cases where every rollout failed, it is expressed in the deployment action space, and it supervises decisions rather than answers. Support *hides* learning when it is always on or reveals the answer. One shared mechanism has been proposed: the share of GRPO groups with mixed outcomes. Evidence: HarnessAdapt-RL, Harness-S, teacher-free co-evolution and Harness-Zero Table 3 ✓.
- **Biggest open problem.** No multi-round loop has a compute-matched, multi-seed, per-round decomposition into harness effect, weight effect and their interaction, with a placebo. So we do not know whether harness↔weights loops *compound* or just front-load one round of gains. A second gap: no work trains the improver on a signal of successor quality. TAME's reward for transfer after the model update is the closest ◐.

### How it developed
| Month | Paper | What changed |
|---|---|---|
| Jun 2025 | SEAL [2506.10943](https://arxiv.org/abs/2506.10943) | A model writes its own fine-tuning data ("self-edits"), and RL rewards it by performance after the update. This is the weight-side precursor; no harness is involved. |
| May 2026 | SIA [2605.27276](https://arxiv.org/abs/2605.27276) | An early "harness + weights" loop driven by one Feedback-Agent ("first" is from project notes †). It names "coupled co-evolutionary Goodhart" but does not measure it ✓ (§8). |
| Jul 2026 | HASE [2607.03935](https://arxiv.org/abs/2607.03935); Co-Harness [2607.22688](https://arxiv.org/abs/2607.22688); MetaEvolve [2607.21971](https://arxiv.org/abs/2607.21971) | HASE: one policy both solves tasks and edits the harness. Co-Harness: a dual loop that evolves the harness, then runs SFT. MetaEvolve: RL for refinement meta-skills. |
| Aug 2026 | HELIX [2608.13951](https://arxiv.org/abs/2608.13951); WHALE [2609.00196](https://arxiv.org/abs/2609.00196) | HELIX: a build–update–rebuild framing with "held vs rebuilt" harnesses, though the model update is never run. WHALE: a clean alternating recipe with harness-only, weight-only and stagewise controls. |
| Sep 2026 | Multi-Harness RL [2609.04518](https://arxiv.org/abs/2609.04518); CHART [2609.22247](https://arxiv.org/abs/2609.22247) †; Harness-Zero [2609.24974](https://arxiv.org/abs/2609.24974); VACE [2609.37105](https://arxiv.org/abs/2609.37105) | First measurements of harness dependence. Harness distillation with an agent acting as the harness. Alternation gated on a validation score. |
| Oct 2026 (1st half) | Harness Annealing [2610.01235](https://arxiv.org/abs/2610.01235); Not All Experience [2610.01787](https://arxiv.org/abs/2610.01787) †; CoTrace [2610.10426](https://arxiv.org/abs/2610.10426); Ceiling [2610.11655](https://arxiv.org/abs/2610.11655); Evo² [2610.13025](https://arxiv.org/abs/2610.13025); CODA [2610.20845](https://arxiv.org/abs/2610.20845) | "Harness internalization" becomes an explicit objective. First rules for "which lever" to pull (process vs content failures; locators and lessons vs procedures). Improver weights are trained (Evo²). First audit of co-evolution fragility (CODA). |
| Oct 2026 (2nd half) | Self-HPO [2610.22791](https://arxiv.org/abs/2610.22791); CoEvo-RSI [2610.25078](https://arxiv.org/abs/2610.25078); HarnessAdapt-RL [2610.28069](https://arxiv.org/abs/2610.28069); WeAct [2610.26847](https://arxiv.org/abs/2610.26847); HERD [2610.31401](https://arxiv.org/abs/2610.31401); CoHARL [2610.36469](https://arxiv.org/abs/2610.36469); Policy-Is-a-Harness [2610.37255](https://arxiv.org/abs/2610.37255) | Self-HPO: the policy proposes its own harness, with a 2×2 cross-evaluation. CoEvo-RSI: archive-based 10-round co-evolution. HarnessAdapt-RL: Baldwin evidence from hints vs mechanism skills. HERD: 25-stage recursive distillation. Policy-Is-a-Harness: a theory that a policy encodes its harness. |
| ICLR 2027 deadline | TAME (IXnpSW8g3v), Harness Annealing, AutoScaffold, harness entanglement, One Policy Many Harnesses, MECA, Harness2Policy, SkillCore, Differentiable Harness, Harness Training, LEAH (wKTGz682RV, title only) | "Capability retained after support removal" becomes a named metric, along with "harness rank reversal". Harness-to-weights distillation becomes a crowded subfield. |
| Nov 1–18 2026 | Latent Harness [2611.01415](https://arxiv.org/abs/2611.01415); Harness-S [2611.04277](https://arxiv.org/abs/2611.04277); JoMoHa [2611.11499](https://arxiv.org/abs/2611.11499); SkillDuet [2611.12109](https://arxiv.org/abs/2611.12109); AutoScaffold [2611.14515](https://arxiv.org/abs/2611.14515); Teacher-free co-evolution [2611.15268](https://arxiv.org/abs/2611.15268); ACE-RL [2611.16409](https://arxiv.org/abs/2611.16409); ModHarness-RL [2611.18533](https://arxiv.org/abs/2611.18533); Where to Adapt at Test Time [2611.19700](https://arxiv.org/abs/2611.19700); Google unifying framework [2611.22299](https://arxiv.org/abs/2611.22299) | Several shifts: the harness writer itself is trained by RL (JoMoHa). Gated rescue replaces always-on support (Harness-S). Small-model co-evolution without a teacher (2611.15268). Internalization at the activation level (Latent Harness). A head-to-head comparison of ICL, GRPO and self-distillation (2611.19700). |

### Taxonomy
The surveys split this corner on two axes.
- **What is updated.** θ = weights, Σ = scaffold (i.e. the harness). This comes from [2607.13104](https://arxiv.org/abs/2607.13104) §3.2 ✓.
- **Distributional vs action-level self-reference.** This comes from the same section ✓.

Ren et al. list "Parametric Distillation and Joint Optimization" as a future direction because of its credit-assignment problem: should a failure be fixed by a prompt, a tool wrapper or a gradient update? ✓ (§9.2 "Future Directions", item 3; an earlier draft said §10).

The alphaXiv survey [2611.recursive-self-improvement-llm-agents](https://www.alphaxiv.org/abs/2611.recursive-self-improvement-llm-agents) is said to *exclude* executor-weight updates, which would put this corner outside its scope †. The link is not a valid arXiv ID and could not be re-opened.

Duan et al. [2609.11873](https://arxiv.org/abs/2609.11873) supply the test we need: "structural vs effective recursion". Does a revised mechanism persist, *and* does it yield stronger successors under comparable budgets and independent evaluation? ✓ (§1.7). No paper in this chapter meets the "effective" bar.

| Approach | Representative papers | What persists across rounds |
|---|---|---|
| **A. Alternating co-evolution with an external improver** (harness search ↔ SFT, RSFT or RL) | WHALE, Co-Harness, SIA, VACE, CoEvo-RSI, CoTrace, HELIX (data only), HarnessAdapt-RL, ModHarness-RL | harness and weights; the improver is frozen |
| **B. Self-referential co-evolution** (the policy writes its own harness, or one policy both acts and edits) | HASE, Self-HPO, Teacher-free 2611.15268, JoMoHa (trained Engineer), ACE-RL, MECA ◐, LEAH † (title only) | harness and weights; the improver is the policy or shares it |
| **C. Improver-weight training** (train the harness writer; the executor may stay frozen) | Evo² (executor frozen), MetaEvolve, TAME ◐ (rewarded on transfer after the update), JoMoHa's Engineer | improver weights and harness |
| **D. Harness → weights distillation or internalization** (remove the harness at deployment) | Harness-Zero, Ceiling (LoRA arms), HERD (every stage), Harness Annealing, Harness2Policy ◐, SkillCore ◐, Harness-Aware Distillation ◐, HPD †, Latent Harness ◐ (activations), EvoIn † | weights (plus a minimal harness) |
| **E. Skill/memory ↔ policy co-evolution** (the harness narrowed to a skill bank or memory) | SkillRL, ReSkill, ARISE and Evolving-RL (audited by CODA), SkillDuet, MDSKILL ◐, ENGRAM ◐, TrackRL ◐, SEAL (self-edits) | skill bank and weights |
| **F. Training under support: Baldwin assist vs hiding** | HarnessAdapt-RL, Harness-S, AutoScaffold ◐, Harness Annealing, TAME ◐, CODA, Multi-Harness RL, One-Policy-Many-Harnesses ◐, harness entanglement ◐, Harness Scaling ◐ | the question is what the weights keep once support is removed |
| **G. Lever choice and theory** | Ceiling (process vs content), Not All Experience †, Where Should Learning Live †, Policy-Is-a-Harness ◐, Where to Adapt at Test Time ◐ | rules for where an improvement should live |
| **H. Modular weights as harness-like actions** | WeAct (mentor), Differentiable Harness ◐ (KV cache), Latent Harness ◐ | the boundary blurs: adapters or caches become an editable "harness" |

### Paper cards

**Harness-Zero: Harness Distillation via Agent-as-Harness** ([2609.24974](https://arxiv.org/abs/2609.24974), 21 Sep 2026; ICLR27 sub rZ7eO7DZmc ✓ ID resolves; PKU/Google/HKUST †)
- *What it does.* A harnessing agent reads a reference harness K, adapted from the evolved harness h⋆. It reviews each student proposal before execution and either passes it or replaces it with a minimal correction in the *target* harness's action space. The student is then SFT'd on these reviewed trajectories.
- *Setup.*
  - Student: Qwen3.5-9B. Harnessing agent: GPT-5.6 Sol. h⋆ evolved by Kimi K3.
  - Target harness: mini-SWE-agent with a single Bash tool.
  - Benchmarks: SpreadsheetBench Verified (100 test), AppWorld (168 test), USPTO (100 test).
  - Training: LoRA rank 32, 2 epochs, 487/282/500 rollouts, **seed 42 only**, single-run pass@1 ✓ (§4.1, App. E.3).
- *Key results.*
  - Macro-average under h rises from 23.3 to 44.3, vs 41.7 for the base model with h⋆ attached ✓ (Table 2).
  - USPTO: distilled 30.0 vs 38.0 with h⋆. Domain knowledge transfers less well than procedure ✓ (Obs. 3).
  - Ablation (USPTO only, Table 3 ✓):
    - SFT on the teacher's trajectories under h⋆ drops to 3.0 (action-space mismatch).
    - Plain teacher rollouts, and the student under h⋆, both leave the student at the 12.0 base.
    - Oracle-answer review reaches 98.6% collection success but only 15.0 on test.
    - K-guided review reaches 30.0.
  - 82.3% recovery across 28 harness-exclusive behaviours ✓ (Table 4).
  - At inference, agent-as-harness beats code-as-harness on frontier models, 81.1 vs 78.1 averaged over six benchmark–model settings ✓ (Table 1). The advantage turns negative with weaker reviewers: −12.0 on Qwen3.6-35B-A3B ✓ (App. F, Table 8).
- *Limitations and critiques.*
  - One seed; the ablation covers one domain.
  - Review costs 2.4× latency on USPTO ✓.
  - Our ICLR sweep notes it never compares against a frozen-harness RL baseline †.
  - It is the purest instance of "amplify-then-distill". It shows that naive distillation of amplified trajectories *fails*, and that it works only when supervision is re-expressed in the deployment action space.

**Harness Evolution Hits a Ceiling: When Weight Training Should Begin** ([2610.11655](https://arxiv.org/abs/2610.11655), 8 Oct 2026; ICLR27 sub FPaMYO47j4 ✓ ID resolves)
- *What it does.* Runs a self-evolving harness loop on DeepPlanning, then crosses seed vs evolved harness with base vs LoRA weights. Failures are labelled F1–F4: process failures (blocked calls, loops, exhausted budgets) vs content failures (a poor delivered plan).
- *Setup.*
  - Qwen3.5-4B and 9B, plus 7 other evolution lines (nine lines on eight models in total).
  - 48 dev and 72 held-out tasks; 4 rollouts per cell; anchors measured on the same night.
  - Transfer check: WebArena-Lite, 117 unseen tasks.
- *Key results.*
  - Held-out score: 4B 0.164 → 0.297, 9B 0.323 → 0.442 ✓ (Table 1).
  - LoRA trained on evolved-harness trajectories and run under the *seed* harness gains +0.126 (4B) and +0.131 (9B) held-out ✓.
  - On 4B the adapter stacks with the harness (+0.082). On 9B it substitutes for it (+0.007) ✓.
  - The answer-shuffled placebo falls 0.158 (dev) and 0.227 (held-out) below base, **9B only** ✓ (Table 10).
  - Full fine-tuning beats LoRA by +0.044 / +0.042 ✓.
  - The 9B adapter cuts F4 (content failures) from 28% to 5% held-out ✓.
  - WebArena-Lite: the harness gains +0.088. Adapters add nothing, because the gain lives in "what the model sees" ✓ (§7).
- *Limitations and critiques.*
  - The 4B ablation cells use 2 rollouts. There is no matched-budget test-time-search baseline, and a converter inflates scores by about 0.025 ✓ (Limitations).
  - Only one harness→weights round.
  - The authors say the ceiling belongs to the loop's evidence package, not to harness evolution in general ✓ (§6.2).

**CoEvo-RSI: When Harnesses Evolve, Which Models Should Follow?** ([2610.25078](https://arxiv.org/abs/2610.25078), 18 Oct 2026; NUS/A*STAR ✓)
- **ICLR link corrected.** An earlier draft linked ICLR27 sub 7UXKgr1Akp. That submission is titled "CoEvo-RSI: A Recursive Self-Improvement Framework for Long-Horizon Model–Harness Co-Evolution". Its abstract describes a different method: per-task scoring of each component, and 40 rounds on four benchmarks. So it is either a different paper with the same name or a substantially different version. Treat it as unlinked ◐.
- *What it does.* A meta-agent draws on a shared archive of harnesses, checkpoints and histories. Each round it develops a harness, selects or merges a checkpoint, and adapts the model by SFT on its own rejection-sampled trajectories. It introduces the Turn-level Interface Mismatch Rate (TIMR).
- *Setup.*
  - Qwen3.5-9B; meta-agent DeepSeek-V4.1-Flash in Codex.
  - 10 rounds, K=8 candidates; full-parameter SFT on 4×H100; averaged over 3 runs.
  - Tasks: Who&When, TraceElephant, RBAC Spider/BIRD, HomeBench ✓ (§4.1).
- *Key results.*
  - TraceElephant Who accuracy: 70.78 vs 56.62 for DeepSeek-V4-Pro ✓ (Table 1).
  - Who&When Algo: 62.17, *below* direct DeepSeek-V4.1-Flash at 68.25 ✓.
  - Ablations on Who&When Algo, all single-benchmark ✓ (§4.3):
    - Harness-only has TIMR > 1% in 8/10 rounds;
    - Model-only reaches at most 35% SA;
    - Latest-only regresses at round 4 and ends lowest;
    - the full method reuses **round-4 weights at rounds 6 and 8**.
  - *Correction:* our round-1 note said "round-3 weights at round 10".
- *Limitations.* No compute-matched decomposition. Held-out tests are same-domain only. TIMR captures only interface violations (authors' caveat ✓).

**HERD: Harness-Evolving Recursive Distillation for Continual Learning** ([2610.31401](https://arxiv.org/abs/2610.31401), 22 Oct 2026; not found on ICLR27 †)
- *What it does.* At every stage of a task stream:
  - the harness evolves skills plus a bounded high-residual memory;
  - the frozen previous checkpoint, given privileged context (skill plus verified answer), teaches the student;
  - distillation is drift-constrained by two anchors, the previous checkpoint and base θ0.
  Only the weights are deployed.
- *Setup.* Qwen3.5-4B; TRACE+17 with 25 stages; 3 seeds; ablations on a single seed ✓ (Fig. 4).
- *Key results* (Table 1 ✓):
  - AA 0.706. Acquisition 0.713 vs 0.715 for Seq-LoRA (statistically tied).
  - Forgetting 0.010 vs 0.019–0.032 for the parametric baselines. ΔC −0.063 vs −0.200 to −0.240.
  - Context-only learners (ACE 0.427, HCL 0.433) barely beat base (0.419). The harness alone does almost nothing on this stream.
  - Without skills, acquisition drops 4.5 and AA drops 5.0 ✓ (Fig. 4). The harness's job is to choose *what* to distill.
  - Attaching the harness after distillation changes the metrics by ≤0.3 ✓.
  - Cost: 10,723 s total vs 10,463 s for Seq-LoRA; OPHSD needs 659,000 s ✓.
- *Limitations.* One model, one stream, known task boundaries, and verified answers are required ✓. The skill writer is fixed at θ0, so the writer does not co-evolve ✓ (§2.3).

**JoMoHa: Joint Model–Harness Optimization with RL** ([2611.11499](https://arxiv.org/abs/2611.11499), 8 Nov 2026; HKU/Salesforce ✓)
- *What it does.* Trains two agents jointly by RL: an Engineer that writes executable harness code and a Solver. They share the task reward but have separate critics, plus peer weighting, a schedule and value normalisation.
- *Setup.* Qwen3.5-9B-class actors; 144 rounds × 8 paired episodes; ALFWorld, WebShop, DBBench. Baselines other than WHALE are copied from Harness-R1 ✓ (§6).
- *Key results.*
  - Naive alternation on ALFWorld: 83.0 → 90.5 → 78.5 → 48.5 → 31.5 ✓ (Table 2).
  - Fixed-harness cross-evaluation decomposes the decline: −47.5 = −34.5 (Solver decline, measured with the harness removed: 82.5 → 48.0) + −13.0 (compatibility loss) ✓ (Eq. 2).
  - Joint training reaches 97.0, vs 90.5 for Frozen Engineer (best checkpoint 91.5), 89.5 for weight-only and 80.5 for harness-only ✓.
  - 3-environment average: 70.3 vs 63.1 for WHALE, which was run locally ✓ (Table 4).
- *Limitations.*
  - Single runs; no seeds; no compute matching against RL alone.
  - The collapse diagnosis comes from one alternating run.
  - The decomposition shows co-adaptation damages the *standalone Solver*. This is Baldwin hiding inside co-evolution.

**Harness Annealing: Learning to Act with Less External Control** ([2610.01235](https://arxiv.org/abs/2610.01235), 1 Oct 2026; ICLR27 sub HOk367J7I0 ✓ ID resolves; SJTU/Berkeley ✓)
- *What it does.* Defines "harness internalization": learning to take over specified control responsibilities once runtime support is withdrawn.
  - The nested harnesses are H1 tools ⊂ H2 +state ⊂ H3 +workflow ⊂ H4 +verification.
  - HAT combines decision-level ("Layered") supervision with a curriculum over teacher trajectories from progressively weaker harnesses.
- *Setup.* Qwen3.5-9B and Qwen3.5-35B-A3B; SWE-QA and SWE-QA-Pro; scores from a GPT-5.5 reference-based judge; LoRA rank 64; **single seed** ✓ (App. A, Limitations).
- *Key results.*
  - Direct SFT on H4 trajectories scores **16.51 under H1, vs 59.93 for the untrained base** ✓ (Table 4). Even Direct H1-trained reaches only 32.45.
  - Layered H4-trained reaches 71.99 under H1. The H2-targeted checkpoint reaches 77.00 ✓ (Table 3).
  - At 35B the gains are smaller (base 78.14 under H1) ✓.
- *Limitations.* No equal-budget controls. Direct and Layered are not matched for tokens. The judge-based metric is a caveat. Our round-2 sweep calls this the strongest owner of "harness dependence plus a fix" †.

**Evolving Together, Failing Apart: The Skill–Policy Co-evolution Trap (CODA)** ([2610.20845](https://arxiv.org/abs/2610.20845), 14 Oct 2026; ICLR27 sub ZIBdUe2hGK ✓ ID resolves; Alibaba/PolyU ✓)
- *What it does.* Argues that "cooperative acceptance" ties the policy to its bank: an edit is accepted if it helps the *current* policy. It audits 4 co-evolution methods under semantic rewrites (T1), incomplete skills (T2) and misleading skills (T3).
- *Setup.* SkillRL, ReSkill, Evolving-RL, ARISE; 6 method–benchmark settings.
- *Key results.*
  - Worst-case success is 4.9–12.6 points below peak, erasing 52–93% of the skill gain ✓ (Table 2).
  - The *meaning-preserving* rewrite is the worst case in 3/6 settings. Consistency is 77.5–87.0, vs a noise floor of 91.9–94.4 ✓.
  - In one setting, the co-evolved policy without its bank scores 57.6% (n=5 evaluations). A policy never trained with skills reaches 70.6%, the mean of its last five checkpoints ✓ (App.).
  - That pure-RL run is itself unstable: it peaks at 87.5, then falls to 53.1 ✓.
- *Limitations.* Diagnoses without fixing. Covers skills only, not harness code or model upgrades. The bands reflect evaluation noise, not training seeds ✓.

**Evo²: Evolving the Evolver through Weight–Harness Co-Evolution** ([2610.13025](https://arxiv.org/abs/2610.13025), 9 Oct 2026; ICLR27 sub Ppb1YoSRol ✓ ID resolves; Google DeepMind/UIUC ✓)
- *What it does.* One policy plays two roles over a tree search with crossover: Proposer (writes harness edits) and Reflector (diagnoses edits). EvoTIDE trains *the evolver* with SFT plus interleaved GRPO on the outcomes of harness edits.
  - **The task executor stays frozen** ✓ (§3, §4.1: "the fixed task executor").
  - So this is improver-weights × harness co-evolution, not executor co-evolution.
- *Setup.* Qwen3.8-27B (FP8, greedy) as both executor and evolver; Polyglot, SWE-bench Verified, LiveCodeBench; memory tasks LoCoMo and LongMemEval.
- *Key results.*
  - Polyglot 82.1 vs 62.6 for GEA; SWE-V 48.3 vs 41.0 for HGM; LCB 58.0 vs 56.0 ✓ (Table 1).
  - Isolating weight evolution: Polyglot +16.3 (p=.001), SWE-V +5.0 (p=.086), LoCoMo +9.3 ✓ (Table 5).
  - Best transfer in 5/6 directions ✓ (Table 2).
- *Limitations and critiques.*
  - In the proprietary-model table, Evo² reports "the better result of its two top-ranked harnesses" while each baseline reports one. This is a selection asymmetry ✓ (§4.4).
  - The SWE-V gain is not significant.
  - The reward is order-0 (the child's gain), which bears on Dan's idea #5.

**SIA: Self Improving AI with Harness & Weight Updates** ([2605.27276](https://arxiv.org/abs/2605.27276), May 2026; Hexo Labs ✓)
- *What it does.* A Feedback-Agent iterates on the scaffold, then switches to LoRA weight updates once harness progress stalls.
- *Setup.* gpt-oss-120b, LoRA rank 32; LawBench (191-class), AlphaEvolve TriMul, MAGIC denoising.
- *Key results* (Table 3 ✓):
  - LawBench: 13.5 → 50.0 (harness only) → 70.1 (harness + weights).
  - TriMul: 0.105 → 0.120 → 1.475.
  - Denoising: 0.048 → 0.241 → 0.289.
- *Limitations.*
  - Updates are sequential, not interleaved.
  - No weight-only arm and no seeds. TriMul is evaluated at test time with no split.
  - §8 names "coupled co-evolutionary Goodhart" (two optimisers working against one verifier) but does not measure it ✓.

**WHALE: A Simple Recipe for Joint Harness–Weight Optimization** ([2609.00196](https://arxiv.org/abs/2609.00196), 31 Aug 2026; ICLR27 sub Gu5Zol8WcQ ✓ ID resolves; KRAFTON/KAIST/Stanford ✓, with Finn)
- *What it does.* Alternates online rejection-sampling fine-tuning with Meta-Harness search, switching on a fixed or patience-based schedule.
- *Setup.* Qwen3.5-2B for SearchQA (7 sets) and AIME24/25; Qwen3.5-4B for Lichess puzzles; metric mean@8 ✓.
- *Key results* (Tables 2–3 ✓):
  - SearchQA: 48.34 vs 38.27 for weight-only and 38.29 for harness-only.
  - Math: 24.79 vs 15.42 and 0.42. Harness search helps math only after the weights move.
  - Stagewise: 43.02 on SearchQA. Adaptive WHALE: 52.82, with 23% fewer rollouts ✓ (§6.2.3).
- *Limitations.* Reports **"best test mean@8"**, i.e. the best point along the test curve ✓. No seeds reported in the parts read. JoMoHa's local re-run gets 80.5 on ALFWorld for WHALE ✓.

**HarnessAdapt-RL: Training the Whole Agent, Not Just the Model** ([2610.28069](https://arxiv.org/abs/2610.28069), 20 Oct 2026; ZJU ✓)
- *What it does.* During GRPO, two frozen teachers act:
  - one writes task-specific "mechanism-level" skills, which are removed at evaluation;
  - the other evolves tools and hooks, which are kept. A change is accepted if the pass rate or trainable rate improves without lowering the other.
- *Setup.* Qwen3.5-9B base; SWE-bench Verified, Multilingual and Pro; 3 evaluation repeats of **one training run per configuration** ✓.
- *Key results* (Table 1 ✓):
  - Average 44.9 vs 35.3 for GRPO and 39.0 for OpenHands-GRPO.
  - Near-answer hints during training, evaluated after removal: **32.7, below GRPO's 35.3**. Mechanism skills: 38.3.
  - Mixed-outcome group share: 46.5% with mechanism skills vs 39.0% with hints ✓.
  - Re-evolving tools as the policy changes beats freezing early versions (+2.2/+4.6/+2.0) ✓.
  - Swapping the evolved H2 back to H0 at deployment costs 4.8/7.7/4.7 points ✓ (§4.5).
- *Limitations.* SWE only, one model, no training seeds. The acceptance rule is not a significance test ✓ (Limitations).

**Harness-S: Learning from Sibling Harnesses** ([2611.04277](https://arxiv.org/abs/2611.04277), 3 Nov 2026; SJTU/Xiaohongshu ✓)
- *What it does.* Builds a fixed pool of sibling harnesses, which are invoked *only when all base-harness rollouts fail*. The same policy generates the rescue rollouts, which replace the failed GRPO group. The policy is then evaluated back under H0.
- *Setup.* Qwen3.5-4B/9B; AppWorld, SWE-Gym, DBBench; 3 independent randomized runs × 3 rollouts ✓ (Table 1).
- *Key results.*
  - 4B AppWorld Avg@3 under H0: 56.87 ±1.47, vs 48.98 for GRPO, 51.91 for H0-rescue and 46.69/47.84 for the always-on mixtures ✓.
  - 9B: 62.34 vs 53.31 ✓.
  - Effective-group rate: 79.38% vs 70.94% for H0-rescue and 64.69% for GRPO ✓ (Fig. 3).
- *Limitations.*
  - The sibling pool is fixed and built by a frontier harness editor (GLM-5.3) ✓ (App.).
  - The paper does not say whether "independent randomized runs" retrain the policy or only re-sample evaluation.

**Self-HPO: Self-Evolving Coding Agents via Harness–Policy Optimization** ([2610.22791](https://arxiv.org/abs/2610.22791), 16 Oct 2026; ICLR27 sub LxC7C6AMxv ✓ ID resolves; MSR)
- *What it does.* The policy analyses its own failures and proposes harnesses (H-step). It is then trained by RL under the chosen harness (P-step). Hard tasks seed the next search.
- *Setup.* Qwen3.5-35B-A3B; mini-SWE-agent seed harness; SWE-bench Verified plus Multilingual and Pro as OOD sets.
- *Key results.*
  - 2×2 cross-evaluation on SWE-V (Table 3 ✓): P0/H0 61.0; P0/H2 64.5; P2/H0 64.4; P2/H2 69.5.
  - The joint gain (8.5) exceeds the sum of the individual gains (6.9) by 1.6, a **superadditive** interaction.
  - Average resolve rate 57.8 → 64.8 ✓ (Table 1).
  - Harness-only Meta-Harness reaches 69.1±1.9 on SWE-V, statistically tied with Self-HPO's 69.5. The variant with an external proposer (HPO) reaches 71.6 ✓ (Tables 1–2).
- *Limitations.* One model family. The ± values look like evaluation spread, not training seeds †. One 2×2 at the end of the run, not per round.

**Knowledge in the Harness, Reliability in the Weights: Teacher-Free Co-Evolution** ([2611.15268](https://arxiv.org/abs/2611.15268), 11 Nov 2026; Brightendata ✓)
- *What it does.* The base model of a Gemma 4 12B agent diagnoses its own failed τ²-bench conversations and writes rules into skills. Each rule must pass code checks, counterexample self-review, a paired retest and a noise-calibrated gate. GRPO then trains a LoRA under the new rules.
- *Key results.*
  - After 2 rounds: +12.5 pass^1 in retail and +19.5 in airline ✓ (abstract in the PDF).
  - The 2×2 shows two things ✓:
    - rules carry policy *knowledge* that RL with the rules in context does not internalize;
    - weights make *already-solved* tasks reliable.
  - Rules can raise the RL signal ✓ (§5):
    - In airline round 1, the accepted update cut batch tasks never solved in four trials from 71 to 42.
    - In the other three runs the count changed by +6, 0 and +3 (Table 12).
    - So the effect appeared in 1 of 4 runs.
- *Limitations.* One model, two domains, 2 rounds. With 114/50 official tasks, single updates are mostly not significant ✓. Uses a DeepSeek user simulator instead of the leaderboard's ✓.

**Co-Harness (Meituan/AI2)** ([2607.22688](https://arxiv.org/abs/2607.22688), 17 Jul 2026; ICLR27 sub 53Ak0jvesg, titled "Co-Harness: Co-Evolving Harnesses and Models for Agent Post-Training" ◐)
- *What it does.* A HarnessCritic attributes failures and proposes validated local diffs. SFT on trajectories from the improved harness follows.
- *Key results.* Qwen3-8B/32B on AIME24/25 and HMMT25: average 58.5 (R0) → 73.5 (R1) → 78.9 (R2) ✓ (Table 6).
- *Critique.*
  - Only 2 rounds. No SFT-under-fixed-harness control, so the weight effect is not separated. Tiny test sets.
  - The 22-version case study runs on AIME24 itself ✓.
  - The ICLR version adds SWE-bench Verified/Pro and phrases the result more cautiously ("improves over two joint rounds") ◐.

**HELIX** ([2608.13951](https://arxiv.org/abs/2608.13951), 14 Aug 2026; HKU)
- *What it does.* A source-traceable harness substrate for "build–update–rebuild". It exposes 96 ports per product contract and 1,024 coupled recipes, or 4,096 when acceptance is selected independently.
- *Key results.*
  - +4.0% *relative* coverage over Pi on 100 AtCoder public-test tasks (Pi solves 50/100).
  - Exports 438 verified training records ✓.
- *Critique.* **No model update is run.** "Comparing held with rebuilt harnesses across rounds" is listed as a next step ✓ (§9–10).

**WeAct: Dynamic Weight Switching as Agent Actions** ([2610.26847](https://arxiv.org/abs/2610.26847), Oct 2026; CMU/Duke ✓, Yin co-first author ✓)
- *What it does.* Capability LoRAs (plan, search, write) over a frozen Qwen3-32B. Loading and offloading an adapter are actions in the tool-calling interface.
- *Key results.* WeAct-hard averages 45.47 across 8 deep-research benchmarks, vs 42.80 for Full SFT and 35.15–35.66 for merging baselines ✓ (Table 1). Also one code-adapter insertion and one writing refresh.
- *Critique.* The capability split is hand-defined. "Sustained continual learning … open" ✓ (Limitations).
- *Relevance.* It turns weights into a harness-like, editable library, a natural target for harness-style evolution.

**SEAL: Self-Adapting Language Models** ([2506.10943](https://arxiv.org/abs/2506.10943), Jun 2025)
- *What it does.* RL on self-generated fine-tuning data ("self-edits"), rewarded by accuracy after the update.
- *Key results.*
  - Knowledge incorporation (Qwen2.5-7B): no-context SQuAD 33.5 → 47.0, vs 46.3 for GPT-4.1 synthetic data ✓.
  - ARC few-shot on Llama-3.2-1B: 72.5%, vs 20% for self-edits without RL and 0% for ICL ✓ (Table 1).
- *Critique.* Catastrophic forgetting under sequential self-edits, and an expensive TTT reward loop ✓ (§Limitations).

**Remaining papers**

| id | title | one-liner | mark |
|---|---|---|---|
| IXnpSW8g3v | TAME: Transfer-Aware Model-Harness Co-Evolution through Learned Harness Revisions | Crossed evaluation of training harness × deployment harness finds "harness rank reversal". A Feedback-Agent is trained by token-level PPO on transfer after the update, under fixed reference harnesses. Across 3 task models and 4 domains it beats the selected fixed teacher and a frozen reviser in 11/12 settings. | ◐ (rechecked) |
| wKTGz682RV | LEAH: Jointly Training LLM Agents to Act and Revise Their Harness | New this check. The title suggests approach B; not read. | † (title only) |
| [2607.03935](https://arxiv.org/abs/2607.03935) | HASE | One Qwen3-8B policy both solves tasks and edits whitelisted harness parts: 86.98±0.38 (5 seeds) vs 86.8 for Meta-Harness+GPT-OSS-120B on S2D (Table 2). | ✓ |
| [2607.21971](https://arxiv.org/abs/2607.21971) | MetaEvolve (COLM 2026) | RL on synthetic evolution trajectories to build refinement meta-skills: +10.01 ID / +24.12 OOD over the strongest baseline. | ◐ |
| [2609.37105](https://arxiv.org/abs/2609.37105) | VACE | Validation-gated alternation: +6.43/+9.09 over weight-only RL and +4.59/+6.95 over ungated alternation. 17 of 44 harness proposals lowered the validation score. | ◐ |
| [2610.36469](https://arxiv.org/abs/2610.36469) | CoHARL | A population of harnesses co-evolved with a shared RL policy; +6.4 over harness-only on SWE-rebench. | † |
| [2610.10426](https://arxiv.org/abs/2610.10426) | CoTrace | A harness-aware data recipe: provenance matching and routing of trajectories into SFT or RL. Qwen3.5-9B goes from 78 to 88 solved on Tmax (90 with online RL). | ◐ (rechecked, OpenReview pG5R1llh27) |
| [2611.18533](https://arxiv.org/abs/2611.18533) | ModHarness-RL | Compression and steering programs learned and evolved during RL, 1.5B–8B; beats a fixed harness "in most settings". | ◐ |
| [2611.16409](https://arxiv.org/abs/2611.16409) | Experience-Handoff RL for Recursive Self-Improvement (method: ACE-RL) | One model trained to act, reflect and curate playbooks under a fixed ACE harness, scored by downstream effect on future rollouts. | ✓ (abstract in PDF; ID resolves) |
| [2611.12109](https://arxiv.org/abs/2611.12109) | SkillDuet | Skills guide OPD at each step; "Strength" (response shift) selects which skills to consolidate. Qwen3-4B: ALFWorld 98.57, WebShop 92.97. Skill value depends on model compatibility. | ◐ |
| AiTVmLVnt1 | MECA | One policy edits procedural memory and is rewarded by transfer; the same signal decides whether a procedure stays external or is consolidated into weights. | ◐ |
| [2611.09472](https://arxiv.org/abs/2611.09472) | ENGRAM | Separates admission to training from release from retrieval: a record is released only when the bare updated model ≥ the base model with retrieval. | ◐ |
| [2611.05313](https://arxiv.org/abs/2611.05313) | MDSKILL | Multi-view OPSD to internalize skill memory. | ◐ |
| [2610.27495](https://arxiv.org/abs/2610.27495) | Privileged Retrospective Distillation (Meta) | Self-evolution via retrospective distillation with privileged context. | ◐ |
| [2611.01415](https://arxiv.org/abs/2611.01415) | Latent Harness | Compiles harness behaviour into post-block activation shifts; 8/10 pre-registered settings recover the gain. | † |
| [2609.35290](https://arxiv.org/abs/2609.35290) | EvoIn | Internalization via rewritten reasoning traces. | † |
| SbMawpgFq4 / [2610.02858](https://arxiv.org/abs/2610.02858) | Harness-Aware Distillation for Small Language Model Agents | Distills only what the teacher adds beyond the harness, by contrasting its actions with and without the harness. | ◐ |
| knp3wJXNQl / [2610.20261](https://arxiv.org/abs/2610.20261) | Harness Policy Distillation | Aligns the policy with runtime semantics; compliance 19.9% → 49.2%. | † |
| sUomzDTiu7 | Harness2Policy | Turns harness-guided tree exploration into process supervision, with recursive co-improvement; 4B/9B/35B beat the base model given oracle harnesses. | ◐ |
| 2EC98UmU1e | SkillCore | Removes skill-loading scaffolding from trajectories to internalize general skills. | ◐ |
| B4IMeyXCpy | Differentiable Harness | Optimizes the KV cache induced by harness text, with weights frozen; 25 task × harness configurations. | ◐ |
| 9MEy7m0jWc | Harness Training: A Post-Training View of Agent Harness Evolution | Reframes harness evolution as Harness-SFT vs Harness-RL. | ◐ |
| [2611.14515](https://arxiv.org/abs/2611.14515) / KPRr8wWiqO | AutoScaffold | A frozen teacher writes training-prompt scaffolds during RLVR, removed at evaluation; +7.2 on ALFWorld (Baldwin assist). | ◐ (ID resolves ✓) |
| [2609.04518](https://arxiv.org/abs/2609.04518) | Multi-Harness RL for Coding Agents: Training Gains and Harness Dependence | Qwen3-8B: mean RL gains +0.48/+0.77 pp; harness-to-harness variance about 7 pp. | ◐/† (ID resolves ✓) |
| 4KSNZln9nS | One Policy, Many Harnesses | 10 Qwen3.5-27B checkpoints × 4 harnesses, 87,720 sessions. RL gains are mostly harness adaptation on tasks already solved, and transfer depends on direction. | ◐ |
| l8aeyLANkN | When Coding Skills Bind to Their Harness | SFT on Claude Code trajectories loses 9.8 pp on a held-out harness ("harness entanglement"); the ARCHER consistency loss fixes it. | ◐ |
| cCqHNd56Pp | Harness Scaling for Generalizable Agentic RL | RL over a factorised harness space transfers to unseen harnesses. | † |
| eFi8mdUJNV | TrackRL | Counterfactual value of each skill within GRPO groups; skills become redundant, scaffolding or harmful as the policy evolves. | † |
| [2610.37255](https://arxiv.org/abs/2610.37255) | Your Policy Is Secretly a Harness: The Decision Geometry of Agent Interfaces | Under KL control, return = harness ceiling − policy mismatch, and training moves only the mismatch. Reversing MiniWoB field order: 85.0 → 19.2. | ◐ (ID resolves ✓) |
| [2611.09701](https://arxiv.org/abs/2611.09701) | Where Should Learning Live? | Regret decomposition for text-only, parameter-only and joint adaptation, with an interaction term. | † |
| [2610.01787](https://arxiv.org/abs/2610.01787) | Not All Experience Belongs in the Weights | Locators and lessons go to weights; procedures and state facts go to context. The rule predicts on a held-out backbone. | † |
| [2611.19700](https://arxiv.org/abs/2611.19700) | Where Should LLM Agents Adapt at Test Time? In Context, in Weights, or Both | 14 mechanisms (ICL, GRPO, SDPO and combinations) on matched streams, Qwen3.5-9B. | ◐ (ID resolves ✓) |
| [2611.12988](https://arxiv.org/abs/2611.12988) | Third Eye | Measures a self-update's value after one *further* update; −1.78 pp pooled. | † |
| [2610.34724](https://arxiv.org/abs/2610.34724) / [2610.12668](https://arxiv.org/abs/2610.12668) | Harness-Model Collaboration / Recurrent Harness | Continual-learning hybrids of harness and weights. | ◐ |
| [2609.39578](https://arxiv.org/abs/2609.39578) | Thinking Outside the Box | Asks whether models can rely on harness guidance *selectively* as they get stronger. | ◐ |
| [2611.22299](https://arxiv.org/abs/2611.22299) | Harnesses That Improve Themselves: A Unifying Framework (Google) | Framework: 4 editable components × a diagnose/propose/validate/deploy loop; modular, gated updates generalise across models. | ◐ (ID resolves ✓) |
| [2609.11873](https://arxiv.org/abs/2609.11873), [2607.13104](https://arxiv.org/abs/2607.13104), [2607.07663](https://arxiv.org/abs/2607.07663), [2611.recursive-…](https://www.alphaxiv.org/abs/2611.recursive-self-improvement-llm-agents) | Surveys | θ/Σ taxonomy (2607.13104 §3.2 ✓). "Distilled into the base model only when they transfer" (2607.13104 §7.1 ✓). Structural vs effective recursion (2609.11873 §1.7 ✓). Non-parametric scope (alphaXiv survey †; not a valid arXiv ID, not re-opened). | ✓ / † |

### Consensus, debates & contradictions
- **Consensus: both levers are needed, but each does different work.** The papers converge on a division of labour:
  - the harness handles process, knowledge and "what the model sees" (Ceiling, Teacher-free, Not All Experience †);
  - the weights handle content quality and consistency (Ceiling's 9B F4 cut, Teacher-free's "reliability in the weights", SIA's "domain intuition").
- **Contradiction: harness-only value varies enormously by task.**
  - Near-useless: HERD's context-only learners (0.43 vs 0.70 for weights); WHALE math (0.42); SIA TriMul (0.120 vs 1.475).
  - Large: Harness-Zero's h⋆ (+18.4); Ceiling 4B (+0.13); Self-HPO's Meta-Harness, which ties the joint method on SWE-V.
  - Only Ceiling's failure-composition rule predicts this *before* running, and only on one benchmark.
- **Debate: is distillation better than the amplifier?**
  - Yes in procedural domains: Harness-Zero's distilled 44.3 beats h⋆'s 41.7, and Ceiling's 9B adapter alone matches the evolution line.
  - No for knowledge: on USPTO, 30 < 38, and Ceiling's WebArena-Lite adapters add nothing.
  - How the data is built matters more than how good the teacher is:
    - SFT on the stronger teacher's trajectories under h⋆ *hurts* (3.0);
    - oracle review (98.6% collection success) gives only 15.0 on test.
- **Debate: Baldwin assist vs hiding.** The same type of intervention has opposite signs across papers.
  - *Hiding:*
    - HAT Direct SFT (16.51 vs 59.93 for base);
    - HarnessAdapt near-answer hints (32.7 < 35.3);
    - always-on mixtures in Harness-S (46.69 < 48.98);
    - CODA (57.6 without the bank vs 70.6);
    - JoMoHa (the Solver loses 34.5 with no harness);
    - TAME rank reversal ◐; harness entanglement −9.8 ◐.
  - *Assist:*
    - gated rescue (Harness-S +7.9);
    - mechanism skills (+3.0);
    - AutoScaffold (+7.2 ◐);
    - Layered HAT;
    - Self-HPO P2/H0 (+3.4);
    - Ceiling LoRA under the seed harness (+0.13);
    - Teacher-free rules shrinking never-solved tasks 71 → 42 (1 of 4 runs only).
  - *Proposed reconciliation:*
    - support helps when it *creates* within-group reward contrast on tasks where every rollout fails, and is expressed in actions the bare policy can take;
    - it hurts when it makes success trivial or acts outside the policy's action space.
    - Mixed-group statistics in three papers support this (HarnessAdapt-RL, Harness-S, Teacher-free), but **no paper varies support dose while holding everything else fixed**.
- **Contradiction: does co-evolution compound?**
  - Co-Harness claims "genuine compounding" over 2 rounds, with no control. Its ICLR abstract drops the word.
  - JoMoHa shows alternation collapses after round 1.
  - CoEvo-RSI's best checkpoints are *old* weights selected again.
  - WHALE shows small interleaved phases beat stagewise training.
  - Net: gains concentrate in early rounds, and inheritance or selection does the stabilising.
- **Superadditivity vs substitution.** Self-HPO finds harness × weights superadditive (+1.6). Ceiling finds stacking on 4B but substitution on 9B. The sign of the interaction likely depends on model strength; this has not been tested systematically.
- **Who writes the harness.**
  - External frontier writers dominate (WHALE, CoEvo-RSI, Harness-Zero, HarnessAdapt, Harness-S).
  - Same-model writers work with heavy gating (Teacher-free, Self-HPO, HASE). In Self-HPO, swapping the external proposer for the policy itself costs 2.1 points ✓ (Table 2).
  - Training the writer helps (JoMoHa +6.5 over Frozen Engineer; Evo² +16.3 on Polyglot), but the reward is order-0 (task score).

### Open problems
1. **A per-round, compute-matched decomposition of multi-round loops.**
   - For each round t, measure SR(θ_t, H_t), SR(θ_t, H_0), SR(θ_0, H_t) and SR(θ_0, H_0).
   - Add an answer-shuffled placebo and at least 3 seeds.
   - Use equal rollout and GPU budgets, and compare against weight-only RL and test-time search.
   - So far, JoMoHa does this once, on one run; Self-HPO once, at the end; Ceiling for one round.
2. **A dose-response for the Baldwin effect.**
   - Vary support strength and adaptivity during GRPO (none, gated rescue, always-on hints, a self-editing harness) with everything else fixed.
   - Measure what is retained after removal, with the mixed-group share as a mediator.
   - TAME and HAT already own the hiding diagnostic, so only the dose-response and the mechanism remain open.
3. **A prospective "which lever" predictor.** Ceiling's F1–F4 rule and the routing rule in Not All Experience are post hoc, on 1–2 benchmarks. Missing: a pre-registered predictor of harness gain vs weight gain across at least 5 domains.
4. **The writer should improve as the weights internalize.**
   - HERD's writer stays at θ0.
   - Evo² trains the writer but freezes the executor.
   - JoMoHa trains both but rewards only shared task success.
   - No one measures order-1 improvement: whether the co-evolved writer writes better harnesses for a *fresh* executor.
5. **Robustness of the delivered pair.**
   - Three kinds of drift matter: of the harness (CODA), of the model (upgrades), and of the deployment harness (entanglement, Policy-Is-a-Harness).
   - CODA-style audits should become standard.
   - Only two methods are trained for drift robustness: ARCHER (harness rendering) ◐ and TAME (reference harnesses) ◐.
6. **What is lost in distillation.**
   - Ren et al. warn that consolidation discards rare but crucial error-recovery strategies ✓ (§9.1).
   - Harness-Zero's per-pattern recovery ranges from 40% to 100%.
   - Whether safeguards and redundant checks survive distillation is unmeasured (Masked Erosion lattice † plus distillation).
7. **Selection hygiene.**
   - Weak practices are common:
     - reporting the "best test mean@8" (WHALE);
     - best-of-two harness selection (Evo²);
     - case studies run on the test split (Co-Harness);
     - acceptance rules that are not significance tests (HarnessAdapt).
   - Teacher-free shows that paired seeds do not pair dialogue outcomes (within-task correlation −0.09 to 0.07 ✓).
8. **Harness as weights.** WeAct adapters, Differentiable Harness caches and Latent Harness activation shifts are all "harness-like" parameters. Can harness-evolution machinery (gates, archives, crossover) operate on adapter libraries, and does it avoid co-evolution lock-in?

### Where it is heading (to mid-2027)
- **Method papers** will keep appearing, but reviewers will demand the controls listed in Open problems 1 and 7. Expect ICLR 2027 reviews of WHALE, CoEvo-RSI and Co-Harness to cite JoMoHa's collapse and CODA's fragility as required baselines (forecast).
- **The internalization lane will standardise on metrics.**
  - "Retained after support removal" (TAME, HAT) and per-behaviour recovery (Harness-Zero Table 4) will become the norm.
  - On-policy self-distillation with privileged harness context (HERD, SkillDuet, MDSKILL, ENGRAM) will become the default transfer tool.
- **Training the harness writer by RL** will become mainstream (JoMoHa, Evo², TAME, ACE-RL, MECA, likely LEAH). The debate will move to *what to reward*: task score, transfer after the update (TAME), or successor quality.
- **Gated rather than always-on support** in RL (Harness-S, Teacher-free, VACE) will become the default recipe. Baldwin-style analysis will be absorbed into "enrichment" theory (Understanding Enrichment 2610.02846 †).
- **Big labs are entering** (GDM's Evo², MSR's Self-HPO, Google's 2611.22299, Meta's PRD). This suggests production RL pipelines will co-train harness and weights, and academic leverage will shift to measurement, theory and robustness audits.

### Connection to our ideas
- **#5 Order-0 Rewards Breed Routers.** This idea sits directly in this corner.
  - Evo², MetaEvolve, JoMoHa's Engineer and ACE-RL all train improver weights on order-0 or child-gain rewards.
  - TAME's reward for transfer after the update ◐ is the closest thing to a successor-quality reward. It must be cited and differentiated.
  - Evo²'s Table 5 (+16.3, and +5.0 not significant) is a natural replication target for "does the trained evolver improve at order 1 on a fresh executor?"
- **#6 Meta-Overfitting.**
  - Evo² claims its evolved harnesses transfer in 5/6 directions, but all of them are coding tasks and the evolver was trained on them.
  - CoEvo-RSI admits its held-out tests are same-domain only.
  - A far-domain test of Evo²-style trained evolvers is a clean, cheap application of #6.
- **#1 Taught, Not Selected.**
  - Distillation propagates whatever rules the harness learned. HERD, Harness-Zero and Teacher-free all write rules from traces, then train the weights on them.
  - A misleading-trace channel would therefore become *parametric* after one distillation round.
  - Teacher-free's counterexample self-review and gate are the obvious defence to compare against.
- **#7 Varying Goals / #9 MetaShift.**
  - CODA (bank drift), Policy-Is-a-Harness (interface change) and CoEvo-RSI (old weights selected again) give motivation.
  - In a co-evolution loop, a "regime change" can come from the weights changing under a fixed harness. That is JoMoHa's −13.0 compatibility loss.
- **#2/#3 Forced Quota and Gate.** These belong in a gate taxonomy as concrete gate types:
  - VACE (17/44 harmful proposals ◐);
  - CoEvo-RSI's meta-agent selection;
  - Teacher-free's noise-calibrated gate;
  - HarnessAdapt's acceptance rule, which is not a significance test.
- **Parked: Baldwin hiding/assist (#18) and harness→weights compounding.**
  - Confirmed crowded. TAME, HAT, HarnessAdapt-RL, Harness-S and AutoScaffold together cover both signs.
  - The only defensible slice left is the controlled dose-response with mixed-group mediation (Open problem 2).
  - Multi-round compounding survives only as the measurement paper in Open problem 1.
- **Mentor link (WeAct).**
  - WeAct is the natural "harness made of weights": inserting and refreshing adapters are agent actions.
  - A gated, archive-based evolution of the adapter library could borrow CoEvo-RSI's checkpoint re-selection and CODA's drift audit.
  - This would connect Dan's improver-side ideas to Xunjian's own line without entering the saturated co-evolution method lane.

#### Fact-check log
| Claim | Verdict | Source location |
|---|---|---|
| Harness-Zero: macro 23.3 → 44.3 vs 41.7 with h⋆; USPTO 30 vs 38 | confirmed | 2609.24974 Table 2, §4.3 |
| Harness-Zero Table 3: teacher-under-h⋆ 3.0; oracle review 98.6% / 15.0; K 30.0 | confirmed (added: plain teacher and student-under-h⋆ both stay at 12.0) | 2609.24974 Table 3, Obs. 4–5 |
| Harness-Zero: 81.1 vs 78.1; −12.0 on Qwen3.6-35B-A3B; seed 42; 2.4× latency; 82.3% over 28 patterns | confirmed | 2609.24974 Table 1, App. E.3, App. F, Limitations |
| JoMoHa: alternation 83.0 → 90.5 → 78.5 → 48.5 → 31.5; −47.5 = −34.5 + −13.0; 97.0 vs 90.5/89.5/80.5; 70.3 vs WHALE 63.1 (local) | confirmed | 2611.11499 Table 2, Eq. 2, §6, Table 4 |
| Ceiling: 0.164 → 0.297, 0.323 → 0.442; LoRA +0.126/+0.131; stacking +0.082 vs +0.007; placebo −0.158/−0.227 (9B only); F4 28% → 5%; WebArena-Lite +0.088 | confirmed | 2610.11655 Table 1, Table 10, §5.3, §7, Limitations |
| CoEvo-RSI: 70.78 vs 56.62; 62.17 < 68.25; TIMR > 1% in 8/10; round-4 weights reused at rounds 6 and 8; 3 runs, K=8, 4×H100 | confirmed | 2610.25078 Table 1, §4.1, §4.3 |
| CoEvo-RSI = ICLR27 sub 7UXKgr1Akp | corrected: the OpenReview abstract describes a different method (per-task component scoring, 40 rounds, 4 benchmarks); link removed | OpenReview search, 7UXKgr1Akp abstract |
| HERD: AA 0.706, acquisition 0.713 vs 0.715, forgetting 0.010, context-only 0.427/0.433 vs 0.419, −skills −4.5/−5.0, cost 10,723 vs 10,463 vs 659,000 s; writer fixed at θ0 (was †) | confirmed; writer claim upgraded to ✓ | 2610.31401 Table 1, Fig. 4, §2.3 |
| Harness Annealing: Direct H4 16.51 vs base 59.93; Layered 71.99; H2-targeted 77.00; single seed | confirmed (35B is Qwen3.5-35B-A3B) | 2610.01235 Tables 3–4, Limitations, App. A |
| CODA: 4.9–12.6 drop, 52–93%, T1 worst in 3/6, CR 77.5–87.0 vs 91.9–94.4; 57.6 vs 70.6; peak 87.5 → 53.1 | confirmed, with wording fixed: 70.6 is the never-skilled policy's last-five-checkpoint mean, and n=5 refers to the co-evolved evaluations | 2610.20845 Table 2, §5, App. (≈ line 2400) |
| Evo²: 82.1/48.3/58.0; Table 5 +16.3 (p=.001), +5.0 (p=.086), +9.3; executor frozen; best of two harnesses | confirmed | 2610.13025 Tables 1, 2, 5; §4.1; §4.4 |
| WHALE: 48.34 vs 38.27/38.29; math 24.79 vs 15.42/0.42; stagewise 43.02; adaptive 52.82 | confirmed (adaptive uses 23% fewer rollouts; Qwen3.5-2B for SearchQA/Math, 4B for chess) | 2609.00196 Table 2, §6.2.3 |
| HarnessAdapt-RL: 44.9 vs 35.3/39.0; hints 32.7, mechanism 38.3; mixed-group share 46.5 vs 39.0; removal cost up to 7.7 (was ◐) | confirmed; removal cost upgraded to ✓ | 2610.28069 Table 1, §4.5, Limitations |
| Harness-S: 56.87 ±1.47 vs 48.98/51.91/46.69/47.84; 9B 62.34 vs 53.31; EG rate 79.38 vs 64.69 | confirmed (added H0-rescue 70.94%; pool editor is GLM-5.3) | 2611.04277 Table 1, Fig. 3, App. |
| Self-HPO 2×2 (61.0/64.5/64.4/69.5; +1.6 superadditive); average 57.8 → 64.8 (was ◐) | confirmed; average upgraded to ✓ | 2610.22791 Tables 1, 3 |
| "Every co-evolution paper beats its harness-only and weight-only arms (incl. SIA, Self-HPO)" | corrected: SIA has no weight-only arm; Self-HPO's harness-only Meta-Harness ties the joint method on SWE-V (69.1±1.9 vs 69.5±0.9) | 2605.27276 Table 3; 2610.22791 Table 1 |
| SIA: 13.5 → 50.0 → 70.1; 0.105 → 0.120 → 1.475; 0.048 → 0.241 → 0.289; Goodhart in §8 | confirmed | 2605.27276 Table 3, §8 |
| Teacher-free: +12.5/+19.5; never-solved tasks 71 → 42 | corrected: the reduction occurred in 1 of 4 runs (others +6, 0, +3); within-task correlation −0.09 to 0.07 confirmed | 2611.15268 abstract, §5 (Table 12), §5.6 |
| Co-Harness: 58.5 → 73.5 → 78.9; "genuine compounding" | confirmed (Table 6); ICLR 53Ak0jvesg title differs slightly and adds SWE-bench | 2607.22688 Table 6; OpenReview abstract |
| HELIX: +4.0% over Pi, 96 ports, 4,096 recipes, 438 records, no model update | corrected wording: 4.0% is relative (Pi 50/100); 1,024 coupled / 4,096 with independent acceptance | 2608.13951 abstract, §6, §9–10 |
| WeAct 45.47 vs 42.80 vs 35.15–35.66; SEAL 33.5 → 47.0 vs 46.3, ARC 72.5/20/0; HASE 86.98±0.38 (5 seeds) vs 86.8 | confirmed | 2610.26847 Table 1; 2506.10943 §4.1–4.2; 2607.03935 Table 2 |
| Ren et al. open direction "Parametric Distillation and Joint Optimization" in §10 item 3; rare error-recovery warning in §10 | corrected: §9.2 item 3 and §9.1 (§10 is the Conclusion) | 2607.13104 lines ≈3620, 3679 |
| Duan et al. structural vs effective recursion (§1.7); 2607.13104 §7.1 "only when they transfer" | confirmed | 2609.11873 §1.7; 2607.13104 §7.1 |
| TAME: PPO-trained Feedback-Agent, 11/12 settings | confirmed at abstract level (3 task models × 4 domains; vs fixed teacher and frozen reviser) | OpenReview IXnpSW8g3v abstract |
| ID resolution: 2611.16409 | resolves, but the title is "Experience-Handoff Reinforcement Learning for Recursive Self-Improvement" (method named ACE-RL); table updated | cached PDF p.1 |
| ID resolution: 2611.22299, 2610.37255, 2609.04518, 2611.19700, 2611.14515 | confirmed, titles match | cached PDFs p.1 |
| ID resolution: ICLR rZ7eO7DZmc, FPaMYO47j4, HOk367J7I0, ZIBdUe2hGK, LxC7C6AMxv, Gu5Zol8WcQ, Ppb1YoSRol | confirmed, titles match | OpenReview notes/search |
| alphaXiv survey "2611.recursive-self-improvement-llm-agents" excludes executor weights | unverifiable (not a valid arXiv ID; downgraded ✓ → †) | — |

## Label-free RL self-improvement: test-time RL (TTRL), self-rewarding and self-training of weights

*Scope: methods that change a model's **weights** using a reward or target the model makes itself, with no human labels for the target data. Covered here: majority-vote and consensus RL (TTRL and its descendants), intrinsic-confidence RL (RLIF), self-judging and self-verifying rewards, label-free self-distillation, and online test-time training. Data-free self-play gets only a short treatment because it has its own chapter.*

*Marks: ✓ means the claim was checked in the full text this session (section or table given). ◐ means it comes from the abstract only (including abstracts read inside the PDF). † means unverified. Local texts are in `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/<id>/paper.txt`, with a compacted copy of each as `body.txt`. An adversarial fact-check pass was run on this chapter; corrections are listed in the Fact-check log at the end.*

### At a glance

- **What this corner is.** The model samples several answers to an unlabeled prompt and builds a reward from them. The reward can be:
  - agreement with the majority answer (TTRL, SRT);
  - low entropy or high self-certainty (RENT, EMPO, INTUITOR);
  - its own judgment, a rubric or a co-trained verifier (Self-Rewarding LMs, CoVerRL, SERPO);
  - a cheap grounded check, such as a code interpreter, Lean or the environment's success flag (T3RL, JURY-RL, ASCENT).

  The weights are then updated by GRPO, DPO or on-policy self-distillation.
- **How the surveys classify it.** Ren et al. (2607.13104) put it under foundation-model (θ) improvement through "distributional self-reference", in the "consistency" family of §5.2 intrinsic evaluative feedback (TTRL, SRT, EMPO, INTUITOR). Chen et al. (2607.07663) file it under bounded self-refinement plus §3.4 test-time training (87 papers). In Duan et al. (2609.11873) it is at most L1/L2: the loop closes on the weights, but humans fix the reward rule and the acceptance criterion. Dai et al. (2611.recursive-…) leave it out, because their scope is non-parametric updates only. (Survey placements were not re-checked in this pass; †/from project notes.)
- **State of the art (Nov 2026).** On math, the best label-free methods recover roughly 75–95% of the gain from verifiable-reward RL on Qwen3 bases:
  - GMAE closes to within 1.0 point of RLVR on Qwen3-1.7B-Base, and recovers about 76–78% of the RLVR gain at 4B and 8B (✓ 2609.36750 Table 1 and §5.2, my arithmetic: 4B 12.9/16.9, 8B 10.3/13.2).
  - Label-free self-distillation (U-OPSD, OPST, STONE, CANON) now matches or beats supervised OPSD and GRPO in several settings (✓ OPST Table 2; ✓ U-OPSD §1 claim; the others ◐).
  - Grounding the pseudo-label in a tool or proof checker beats pure voting in every paired comparison reported by T3RL (15/15, ✓ 2603.02203 §5) — but only with an adequate verifier: a 0.5B code verifier makes it worse than voting (✓ App. Table 8).
- **Consensus mechanism.** Intrinsic rewards **sharpen the model's initial distribution**. They help when confidence already tracks correctness and fail when it does not (✓ 2603.08660 abstract, §3–4). Per-problem audits find almost no "zero-to-one" learning: on TTRL, 0.7% of problems are learned versus 21.6% degraded (✓ 2605.19444 Fig. 1/§1).
- **Consensus failure mode.** Long training on many prompts collapses. Pseudo-reward goes to 1 while accuracy falls, ending in prompt-independent template answers such as `\boxed{1}` (✓ 2505.21444 §4.2). Training on ≤128 prompts was stable, while ≥512 always collapsed; DAPO-32 never collapsed and DAPO-512 always did across 3 seeds (✓ 2603.08660 §5.1). A theory paper derives a critical problem count N_c ≃ ln(1/δ)/(λ²G), i.e. N_c ∝ λ⁻² up to a log factor (◐ 2610.37553 abstract).
- **Best new explanation.** *Error coherence, not error rate*, decides whether majority-vote labels can stand in for gold labels:
  - Majority-vote errors are as incoherent as a random-reward null.
  - The noise-corrected cosine between the majority-vote and gold updates is 1 within error at initialization (five models, two families).
  - Flipping up to 30% of gold labels at random leaves the update direction intact; the same fraction flipped by a shared rule destroys it.
  - All of this is ✓ 2610.20898 §1/§3.
- **Biggest open problem.** Nothing in this corner improves the *reward-making mechanism itself* under an independent evaluation. Every method sharpens toward a fixed self-consistency rule, or grounds in an external checker. Whether label-free weight updates can ever add capability the base model cannot already sample, rather than reliability, is unresolved:
  - TTSP gains are approximately zero below mean@512 ≈ 0.1 (✓ 2611.07367 §5 / Fig. 7).
  - OPST says it "can only amplify existing knowledge" (✓ Limitations).

### How it developed

| Month | Paper | What changed |
|---|---|---|
| 2024-01 | Self-Rewarding LMs (Yuan et al.) † | LLM-as-judge rewards its own outputs for iterative DPO. This is the "self-rewarding" lineage that the ICLR'27 score-saturation analysis yHVcEoxOlW later studies. |
| 2025-02 | Self-rewarding correction 2502.19613 ◐ | The model generates and self-evaluates within one pass, without external reward at inference. |
| 2025-04 | EMPO 2504.05812 ◐; **TTRL 2504.16084** ✓ | Semantic-entropy minimization; majority-vote reward with GRPO on the unlabeled test set itself (transductive). Became the field's anchor: 3.27k alphaXiv votes †, NeurIPS 2025 (per citing papers †). |
| 2025-05 | INTUITOR 2505.19590 ◐ (ICLR'26); RENT 2505.22660 ◐; **SRT 2505.21444** ✓; RLSR 2505.08827 ◐; Absolute Zero 2505.03335 † | Self-certainty and entropy as the only reward. First demonstration of collapse from reward hacking under prolonged self-training. Self-judging and self-play variants. |
| 2025-06 | Spurious Rewards 2506.10947 ✓ (abstract/§1); No Free Lunch 2506.17219 ◐ | Random rewards lift Qwen2.5-Math-7B on MATH-500 by 21.4 points (versus 29.1 for ground truth), but not Llama3 or OLMo2. RLIF rises and then falls below the base, and gives little gain on instruct models. |
| 2025-08/09 | Co-rewarding 2508.00410 ◐; R-Zero 2508.05004 †; EVOL-RL 2509.15194 ◐ | Cross-view and complementary rewards; a challenger–solver loop; novelty bonus against diversity collapse. |
| 2025-11 | "You Need Reasoning to Learn Reasoning" 2511.04902 ◐ | Label-free RL fails on weak bases. |
| 2026-02 | Test-time Recursive Thinking 2602.03094 ✓ | A test-time, *weight-free* recursive self-improvement baseline that self-ranks and carries knowledge across rounds. |
| 2026-03 | **How Far Can URLVR Scale** 2603.08660 ✓ (ICLR'26); CoVerRL 2603.17775 ◐; SCRL 2603.19880 ◐; T3RL 2603.02203 ✓; Dual Consensus 2603.16223 ◐; PowerFlow 2603.18363 ◐ | Taxonomy (intrinsic vs external) plus the sharpening theorem and Model Collapse Step. Generator–verifier co-evolution. Abstention and negative labels. Tool-verified voting, explicitly framed as recursive self-improvement. |
| 2026-04 | DDRL 2604.21327 ◐; JURY-RL 2604.25419 ◐ | Group-relative advantages *amplify* spurious signals from medium-consistency prompts. Votes propose and Lean disposes. |
| 2026-05 | Correct-Answer Extinction Window 2605.19444 ✓; gradient-norm reward 2605.09920 ◐; multi-reward RLIF 2605.22620 ◐ | Per-problem migration accounting; flip rate as a leading indicator of collapse. |
| 2026-06/07 | Confidence-conditioned verification 2606.03608 ◐ (ICLR'27 3iRWDF4SSh); intrinsic rewards for code 2606.20881 ◐; **SERPO 2607.26873** ◐/✓ | TTRL extended to open-ended generation through self-evolving rubrics. |
| 2026-08 | U-OPSD 2608.06296 ◐ (301 votes); Hi-TTRL 2608.03545 ◐; CoRE 2608.09324 ◐; TTPO 2608.27448 ◐ (266 votes); OM-GRPO 2608.03119 ◐ | The shift from scalar majority-vote RL toward *dense self-distillation* toward a consensus-conditioned teacher. |
| 2026-09 | **GMAE 2609.36750** ✓ (ICLR'27 VczpXmXDmE); label-free steering 2609.18587 ◐ (ICLR'27 Dou8KTwVNU) | Marginalizing the reward over group contexts. Compressing TTRL into bias-only parameters. |
| 2026-10 | **Wrong Votes, Right Gradients** 2610.20898 ✓; Collective Dynamics 2610.37553 ◐; **OPST** 2610.32770 ✓ (Google); **ASCENT** 2610.05303 ✓; R-Quest 2610.04299 ◐; random-reward probe 2610.01066 ◐; zero-data self-play placebo trial 2610.reward-placebo… ◐; MS-TTRL 2610.32234 ◐; SelectTTRL 2610.16104 ◐; Three Regimes 2611.01307 ✓ (posted Oct 31) | Batch-level theory of why majority vote works, and a collective-instability theory of why it collapses. Fully data-free self-training from topic descriptors. Online agent test-time training with self-distillation of verified episodes. Placebo controls become a genre. |
| 2026-11 (1–18) | PC-Dr.DPO 2611.09872 ✓ (§1); TTSP 2611.07367 ✓; Kernel-Law Lock-In 2611.11165 ◐; TRUER 2611.13714 ◐; STONE 2611.18916 ◐; SpeCal 2611.20691 ◐; "LMs Can Rewrite Their Own Weights" 2611.21885 ✓ (§1) | Majority-vote error split into population bias and finite-rollout variation. Self-play focused on a single target with no target verifier. Lock-in is predictable early in training. A small labeled rectifier. Easier self-written stepping stones. Hidden-state spectra. A 27B model edits its own weights *by reasoning* (adjacent to this corner, and uses graded answers). |

### Taxonomy

The corner splits on four axes. The primary axis is the **signal source**, meaning where the correctness information comes from. The others are the **update operator**, the **data regime** and the **fix family** used for consensus failures.

| Axis / approach | Mechanism | Representative papers |
|---|---|---|
| **S1. Intrinsic certainty** | Reward = −entropy, self-certainty, sequence probability, or gradient norm | RENT 2505.22660, EMPO 2504.05812, INTUITOR 2505.19590, RLSC 2506.06395, No Free Lunch 2506.17219, gradient-norm reward 2605.09920, Selective Internal Feedback 2610.21392, PowerFlow 2603.18363 |
| **S2. Ensemble consensus** | Reward = agreement with the group's majority, soft vote or cluster | TTRL 2504.16084, SRT 2505.21444, Co-rewarding 2508.00410, EVOL-RL 2509.15194, CoRE 2608.09324, GMAE 2609.36750, MS-TTRL 2610.32234, TCCA (ICLR'27 Cuk60HQcrN) |
| **S3. Self-judgment / self-verification** | The model (or a co-trained role) judges, ranks or writes rubrics | Self-Rewarding LMs †, Self-rewarding correction 2502.19613, RLSR 2505.08827, CoVerRL 2603.17775, CoVER (ICLR'27 DshkmNnrso), SERPO 2607.26873, score saturation (ICLR'27 yHVcEoxOlW), SRO fragility (ICLR'27 IVFASEtqp6) |
| **S4. Grounded asymmetric verification** | Votes propose, and a cheap checker (code, Lean, environment) disposes | T3RL 2603.02203, JURY-RL 2604.25419, ASCENT 2610.05303 (environment success), TTSP 2611.07367 (interpreter on synthetic tasks), the "external rewards" arm of URLVR 2603.08660 §7 |
| **S5. Self-generated tasks** | The model writes its own problems (see the self-play chapter) | Absolute Zero 2505.03335, R-Zero 2508.05004, R-Quest 2610.04299, OPST 2610.32770, STONE 2611.18916, TTSP 2611.07367 |
| **U1. Scalar RL** (GRPO/PPO/RLOO) | Policy gradient on the pseudo-reward | Most S1/S2 papers |
| **U2. Preference** (DPO) | Majority answer vs minority as a preference pair | PC-Dr.DPO 2611.09872, ScPO (baseline in SRT Table 1) |
| **U3. Label-free self-distillation** | A teacher = the same model conditioned on the consensus, its own solution or stepping stones; dense token targets | U-OPSD 2608.06296, CANON (ICLR'27 LiJUIYVa13), OPST 2610.32770, STONE 2611.18916, ASCENT 2610.05303, RCTS (ICLR'27 OCZ3eKMN9b) |
| **U4. Restricted parameters** | Bias-only or LoRA fast weights | Label-free steering 2609.18587, ASCENT (LoRA) |
| **R1. Transductive TTRL** | Train on the unlabeled test set, then evaluate on it | TTRL, T3RL, SCRL, DDRL, PC-Dr.DPO |
| **R2. Inductive unsupervised RLVR** | Train on an unlabeled train set, evaluate on held-out sets | SRT, URLVR-scale, GMAE, Wrong Votes, INTUITOR |
| **R3. Per-instance test-time scaling** | Adapt per single target problem | TTSP, TRT (no weights) |
| **R4. Online streams** | One pass, persistent updates | ASCENT, PC-Dr.DPO (cache), SERPO (sequential benchmarks), safety-in-mixed-streams (ICLR'27 mjNZLUFK21) |
| **F1. Filter or abstain** | Drop weak-consensus prompts, add negative labels | SCRL 2603.19880, DDRL 2604.21327, SelectTTRL 2610.16104 |
| **F2. Fix the advantage** | Marginalize or debias GRPO normalization | GMAE 2609.36750, DDRL, Three Regimes 2611.01307 |
| **F3. Keep exploration** | Novelty or rare-answer bonus, minority-preserving sampling | EVOL-RL 2509.15194, TTRL-Guard 2605.19444, Three Regimes, Beyond Self-Consensus (ICLR'27 BKfYuZRiix) |
| **F4. Monitor or predict collapse** | Flip rate, Model Collapse Step, kernel-law lock-in, N_c | 2605.19444, 2603.08660 §6, 2611.11165, 2610.37553 |
| **F5. Add a little truth** | A rectifier trained on a small labeled source set | TRUER 2611.13714 |

**A labeling contradiction to know about.** Gao et al. (2507.21046 §5.1) list majority vote under "External" rewards (†, not re-checked in this pass). He et al. (2603.08660 §2) classify it as *intrinsic* (ensemble-based) and reserve "external" for generation–verification asymmetries. This chapter follows He et al.

### Paper cards

**1. TTRL: Test-Time Reinforcement Learning** ([2504.16084](https://arxiv.org/abs/2504.16084), Apr 2025, NeurIPS 2025 per citing papers †; Tsinghua / Shanghai AI Lab)
- *What it does.* It samples N answers per unlabeled test question, takes the majority answer as the label, gives a 0/1 reward for agreement, and runs GRPO on the test set itself.
- *Setup* (✓ §3.1):
  - Models: Qwen2.5-Math 1.5B/7B, Qwen2.5-7B/32B, Qwen3-8B, LLaMA-3.x, Mistral-Nemo / Ministral-8B, DeepSeek-Math-7B-Instruct, DeepSeek-R1-LLaMA-8B, Skywork-OR1.
  - Benchmarks: AIME24, AMC, MATH-500, GPQA-D.
  - Training: 64 samples for voting, 32 downsampled for training; cosine LR with peak 5×10⁻⁷ (AdamW); 10/30/80 episodes for MATH-500/AMC/AIME24; max length 3,072 tokens (32,768 for LRMs); 8×A100-80GB.
  - Evaluation: pass@1 from 16 samples (4 for 32k-context runs), temperature 0.6. No seed variance is reported.
- *Key results:*
  - Qwen2.5-Math-7B AIME24 rises from 12.9 to 40.2, and the 4-benchmark average from 31.1 to 54.9. GPQA *drops* from 29.1 to 27.7 (✓ Table 1).
  - Qwen3-8B (non-thinking, 3k context) average rises from 53.8 to 64.5 (✓ Table 1).
  - Mistral-Nemo-Instruct AIME falls from 0.8 to 0 (✓ Table 2).
  - Average after TTRL beats the base model's maj@64 (✓ Fig. 7).
  - "Lucky hit": on AIME24, label accuracy is 37% but reward accuracy is 92%, because wrong answers are scattered (✓ §4.2).
  - Response length falls 44–73% across MATH-500 levels, and gains shrink with difficulty, from +45.4 at L1 to +16.8 at L5 (✓ Table 3).
- *Limitations and critiques:*
  - It is transductive (trains on the evaluation set).
  - The authors name failure modes from bad temperature or batch size and from a lack of prior knowledge (✓ §4.3).
  - Spurious Rewards (2506.10947 ✓ §1) shows that random rewards alone give Qwen2.5-Math-7B +21.4 points on MATH-500. Part of TTRL's +36.7 on that model (46.7 → 83.4, ✓ Table 1) is probably elicitation, not learning from the vote.
  - The Extinction Window paper (2605.19444 ✓) finds the gains are mostly sharpening of problems the model can already solve.

**2. How Far Can Unsupervised RLVR Scale LLM Training?** ([2603.08660](https://arxiv.org/abs/2603.08660), Mar 2026, ICLR 2026; Tsinghua, the TTRL group)
- *What it does.* Taxonomy, theory and a large sweep. All intrinsic rewards converge to sharpening the initial distribution. It proposes the **Model Collapse Step** (the step at which reward accuracy drops below 1%) as a cheap predictor of whether a model is trainable with RL.
- *Setup* (✓ §4–6):
  - Qwen3-1.7B-Base on DAPO-17k with five intrinsic rewards.
  - Per-problem study on 25 MATH500 problems.
  - Dataset-size sweep over {32, 128, 512, 2048, 8192, 16384} prompts; 3 seeds only for sizes {32, 128, 512}.
  - 7 models from the OLMo, LLaMA and Qwen families for the predictor.
- *Key results:*
  - Every intrinsic reward shows the same rise-then-fall pattern. Failure types: probability rewards give *length collapse*; entropy rewards give *repetition collapse* (✓ §4.1.2).
  - Single-problem training flips greedy correctness in only 3/25 problems (✓ §4.2.1).
  - ≤128 prompts are stable; ≥512 collapse. DAPO-32 never collapses while DAPO-512 always does, across 3 seeds (✓ §5.1).
  - Training on 32 filtered samples where almost all initial majorities are *wrong* still improves out-of-distribution results without collapse (✓ §5.3, Fig. 9).
  - In the Qwen family, the SFT/distilled variant stays stable while the base drops to near-zero reward accuracy by step 200; in the LLaMA family both collapse, with the base model failing by step 40 (✓ §6.1).
  - Model Collapse Step tracks the ground-truth RLVR gain and uses 5.6× fewer tokens than the GT-gain gold standard (1.19B vs 6.66B tokens across 7 models) (✓ §6.2–6.3, Table 3).
- *Limitations.* The analysis is mostly at 1.7B. The "external reward" escape route (§7) is preliminary.

**3. Can Large Reasoning Models Self-Train? (SRT)** ([2505.21444](https://arxiv.org/abs/2505.21444), May 2025, v2 Oct 2025 †; CMU, Zanette group)
- *What it does.* Majority-vote RL on *training* sets (inductive), with the self-labeling policy updated at every step.
- *Setup* (✓ §4):
  - Main math models: Qwen2.5-Math-7B, Llama-3.1-8B-Instruct, Qwen3-14B-Base and DeepSeek-Math-7B-Instruct (RLOO; GRPO in an appendix). Qwen3-4B-Base on Reasoning Gym for the curriculum experiments.
  - Training sets: MATH-12K, DAPO, AIME 1983–2023 (and Big-Math-RL-Verified for Llama).
  - Held-out: AIME24/25, AMC, MATH-500.
- *Key results:*
  - Peak mean@32 averaged over AIME24/25 and AMC, Qwen2.5-Math-7B (✓ Table 1):

    | Training set | Base | SFT on majority vote | DPO | ScPO | **SRT** | Ground-truth RL |
    |---|---|---|---|---|---|---|
    | DAPO | 0.15 | 0.18 | 0.21 | 0.20 | **0.31** | 0.36 |
    | MATH-12K | 0.15 | 0.18 | 0.23 | 0.20 | **0.32** | 0.33 |

  - Llama-3.1-8B-Instruct rises from 52.6% to nearly 60% average accuracy on held-out MATH-500 (✓ §4.1).
  - A curriculum on synthetic tasks lets the model keep climbing (✓ Fig. 5).
  - **Collapse** (✓ §4.2, Fig. 6–8): self-reward and KL spike together, and outputs become random tokens plus a template answer such as `\boxed{1}`. On MATH-12K this happens after about 1200 RL steps (✓ App. C, Fig. 11).
  - Neither the KL coefficient nor the choice of GRPO vs RLOO prevents collapse. A lower learning rate only delays it. Fewer generations per prompt inject noise into the vote and delay it (✓ Fig. 8 caption).
- *Limitations.* The proposed mitigations (early stopping, curriculum, a fixed teacher) are in App. I, and none removes the core problem. Feedback design is the open problem.

**4. Wrong Votes, Right Gradients: Why Majority-Vote Rewards Track Verifiable RL** ([2610.20898](https://arxiv.org/abs/2610.20898), Oct 2026; Dartmouth / WashU, the R-Zero group)
- *What it does.* Moves the analysis from the prompt to the batch update. It splits the majority-vote update into shrinkage plus *misdirection*, and shows that misdirection vanishes with batch size when label errors are incoherent across prompts.
- *Setup* (✓ §2):
  - Qwen3-4B-Base on DeepScaleR, B=256, n=16, no KL or entropy term. Gold-reward and majority-vote runs from the same initialization see the same prompts.
  - Gradient sketches on 5 models (Qwen3-1.7B-Base/4B-Base, Qwen2.5-7B, Llama-3.2-3B-Instruct, Llama-3.1-8B-Instruct).
  - 164/256 prompts are "mixed" under the gold reward.
- *Key results* (✓ §1/§3):
  - A prompt's own gradient accounts for less than 1% of the change in that prompt's output.
  - The noise-corrected cosine between the majority-vote and gold updates is 1 within error.
  - Majority-vote errors are as incoherent as a random-reward null.
  - Flipping up to 30% of labels at random leaves the direction intact; a shared-rule flip destroys it (at 31% the shared-rule update points away from gold).
  - Rewards that favor a response *type* on every prompt (frequency, confidence, a learned reward model) fall measurably short.
  - Held-out problems with a wrong majority acquire a correct majority at about 3× the resampling rate (✓ §5).
  - The equivalence is *local*: gold training still ends ahead on held-out majority accuracy.
- *Limitations* (✓ §Limitations): training runs on one model (Qwen3-4B-Base), two runs per reward, and the high-entropy regime is not analyzed.
- *Relevance beyond this corner.* This is the most transferable result here. It gives a testable criterion, error coherence, for when a cheap self-signal can replace a verifier.

**5. Detecting and Mitigating the Correct-Answer Extinction Window in TTRL** ([2605.19444](https://arxiv.org/abs/2605.19444), May 2026; Meituan)
- *What it does.* Per-problem tracking of initial vs final label accuracy, and **TTRL-Guard**, which combines flip-rate-aware reward scaling, minority-preserving sampling and risk-conditioned sparse updates.
- *Setup:* the per-problem analysis uses Llama-3.2-3B-Instruct, Qwen2.5-7B-Instruct and Qwen3-4B on AIME25 and MATH-500; the method is evaluated on 4 benchmarks (AIME24, AIME25, AMC, MATH-500) (✓ §1).
- *Key results* (✓ Fig. 1/§1; four of the paper's six categories shown):

  | Problem category | Share |
  |---|---|
  | Stable always-right | 44.5% |
  | Learned (zero-to-one) | 0.7% |
  | Degraded | 21.6% |
  | Always wrong | 26.2% |

  - A correct answer is briefly active and then permanently suppressed, and the damage is irreversible once the vote locks on. Flip rate leads the collapse.
  - TTRL-Guard gives +54% relative over TTRL on AIME 2025, and cuts the degraded fraction on Llama from 60.2% to 28.0% (✓ §1).
- *Limitations.* Small models only. The guard rules are heuristic.

**6. Group-Marginalized Self-Rewarding RL (GMAE)** ([2609.36750](https://arxiv.org/abs/2609.36750), Sep 2026; SJTU / Tencent; ICLR'27 submission VczpXmXDmE)
- *What it does.* A response's self-reward depends on which other samples happen to share its group. GMAE marginalizes over possible group contexts to get an expected advantage. Variants wrap TTRL (GMAE0), Co-Reward (GMAE-CR) and SR-TTRL (GMAE-SR).
- *Setup* (✓ §5.1):
  - Qwen3-1.7B/4B/8B-Base and Llama3.1-8B-Instruct trained on DAPO-14K for one epoch (GRPO, batch 64, LR 1e-6, G = G′ = 16).
  - 8 benchmarks (MATH500, AMC, AIME24/25/26, GPQA, MMLU-Pro, LiveCodeBench-v6), mean@16.
  - 9 self-rewarding baselines (7 ensemble-based: TTRL, CoVo, SCOPE, Self-Harmony, RESTRAIN, Co-Reward, SR-TTRL; 2 probability-based: Intuitor, EM-RL) plus an RLVR reference.
- *Key results* (✓ Table 1). Average mean@16:

  | Model | Raw | TTRL | Best GMAE | RLVR |
  |---|---|---|---|---|
  | Qwen3-1.7B-Base | 11.9 | 18.4 | 21.0 | 22.0 |
  | Qwen3-4B-Base | 17.2 | 27.0 | 30.1 | 34.1 |
  | Qwen3-8B-Base | 27.7 | 34.5 | 38.0 | 40.9 |
  | Llama3.1-8B-Instruct | 15.8 | 23.5 | 26.2 | 27.4 |

  - GMAE beats the baselines in 30 of 32 model–benchmark cells, by +2.0–2.8 points over the strongest baseline (✓ §5.2).
  - Intuitor and EM-RL (the confidence rewards) are among the weakest label-free baselines, but not the weakest in every block: CoVo is lowest at 4B (22.9) and 8B (30.1), and SCOPE (15.1) sits below EM-RL at 1.7B (✓ Table 1).
  - Across five rollout-sampling seeds on Qwen3-8B-Base, the standard deviation of mean@16 averaged over benchmarks is 0.48 for GMAE0 versus 0.69 for TTRL (✓ §5.3, Fig. 4).
- *Limitations.* Training is on math only. The gap to RLVR stays about 3–4 points at 4B–8B.

**7. Tool Verification for TTRL (T3RL)** ([2603.02203](https://arxiv.org/abs/2603.02203), Mar 2026, v2 Aug 2026; LMU / Stanford)
- *What it does.* Votes are weighted by code-execution verification, which suppresses "false-popular" majorities. The authors frame TTRL as a recursive self-improvement paradigm and T3RL as "verified online data synthesis".
- *Setup* (✓ §5): five 1–4B backbones (Qwen-Math, Qwen2.5, Qwen3, Llama-3.2) on MATH-500, AMC and AIME24, using TTRL's hyperparameters.
- *Key results* (✓ Table 1, §5):
  - Wins 15/15 paired comparisons with TTRL.
  - Qwen3-4B AIME24: 36.4 → 40.0.
  - Llama-3.2-3B-Instruct AIME24: 13.3 → 17.1.
  - Qwen-Math-1.5B average: 45.9 → 48.8.
  - Relative gain grows with difficulty, from +0.2% at L1 to +4.3% at L5.
- *Limitations:*
  - A weak verifier (Qwen-Coder-0.5B) makes things *worse* than plain voting (−1.4 average vs TTRL on Qwen-2.5-0.5B), and gains are small on easy tasks (✓ §6, App. Table 8).
  - TTRL baseline numbers are copied from the original paper "where possible" (✓ Baselines), so the comparisons are not same-seed.
  - Most gains are within one standard deviation on MATH-500 (✓ Table 1 ± values).

**8. On-Policy Self-Training (OPST): Data-Free Self-Improvement** ([2610.32770](https://arxiv.org/abs/2610.32770), Oct 2026; Google / Ulm; 84 votes †)
- *What it does.* Seeded only by topic descriptors, the model writes its own question plus privileged reasoning and answer. Filters:
  - Pseudo-verification: the model must reproduce the answer in a fresh context and under paraphrase.
  - Consensus gate: drop problems the student already solves.

  An EMA teacher sees the privileged information, and the student sees only the question. Training is on-policy self-distillation.
- *Setup* (✓ §4): Gemma 4 E2B/E4B, with and without thinking. MMLU, GPQA, GSM8K and HellaSwag in-domain, plus out-of-domain sets. A visual variant for VQAv2/POPE.
- *Key results:*
  - E2B without thinking: MMLU +10.2, GPQA +4.0, HellaSwag +7.5 (✓ Table 1).
  - On identical self-generated data, GRPO rewarding agreement with the generated answer gains only +1.0 average (E2B, no thinking), versus +7.3 for OPST (✓ Table 1).
  - Beats R-Zero by 1.4–5.2 and u-OPSD by 1.0–2.8 points, and recovers 88% of supervised OPSD's gain (+7.3 of +8.3, E2B without thinking) (✓ Table 2, §4).
  - With 75% wrong answers injected (filters off), OPST loses 1.9 points while SFT loses 5.7 (✓ Table 5c).
  - A placebo privileged context from another problem on the same topic gives only +2.7, versus +10.2 (✓ Table 5b).
  - The multi-domain ("All three") run regresses MATH-500 by 4.9 (✓ Table 3).
- *Limitations* (✓): it only amplifies existing knowledge; consistent errors pass pseudo-verification; topic descriptors are hand-chosen; models are small; there is no label-free stopping rule.

**9. Test-Time Self-Play: Scaling without a Verifier (TTSP)** ([2611.07367](https://arxiv.org/abs/2611.07367), Nov 5 2026; ETH Zurich / Tufa Labs, Krause group)
- *What it does.* For one hard target problem, a proposer writes related synthetic problems with tests and a reference solution, aimed at the solver's frontier. Both roles (two LoRA adapters on a shared frozen backbone) are trained with GRPO, using execution feedback from a Python interpreter. The target's own verifier is never used in training.
- *Setup* (✓ §4–5):
  - Qwen3-8B.
  - HardLCBv6 = LiveCodeBench-v6 problems with 0 < mean@512 < 0.7, split into MidHard (>0.1) and VeryHard (≤0.1).
  - 7 verifier strengths (v0, v25, v50, v75, public tests, private tests, oracle v⋆), from v0 (accepts everything) to the oracle.
  - Baselines: rejection sampling, functional majority voting, ACE.
- *Key results:*
  - Best method at every verifier strength. In the no-verifier limit, mean@8 improves by 35% absolute (◐ abstract).
  - On VeryHard the gain is approximately zero. Below mean@512 ≈ 0.1, the proposer cannot bootstrap a curriculum that leads back to the target (✓ §5 / Fig. 7).
- *Limitations* (✓): compute is per target and not amortized; proposers can reward-hack; the solver's entropy can collapse. "Without a verifier" means *without a target verifier*, since the interpreter grounds the synthetic tasks.

**10. ASCENT: Online Test-Time Training of Long-Horizon Agents** ([2610.05303](https://arxiv.org/abs/2610.05303), Oct 2026; UNSW)
- *What it does.* One pass over a task stream, batch size 1. Each environment-verified trajectory, with invalid-action turns pruned, is given as privileged context to the frozen initial model. That model's hindsight distributions are distilled into persistent LoRA fast weights.
- *Setup* (✓ §5): Qwen3.5-4B/9B on ALFWorld, WebShop and AppWorld. Baselines: offline and online harness methods (MemP, ACE, GEPA, EvoSkill, TextGrad, Trace2Skill, ReasoningBank, A-Mem, MemRL, Dynamic Cheatsheet) and TT-OPSD.
- *Key results* (✓ Table 1, ALFWorld seen stream):

  | Model | Base SR | ASCENT SR | Best other method | Turns, base → ASCENT |
  |---|---|---|---|---|
  | Qwen3.5-4B | 46.4 | 69.5 ±5.6 | 63.6 (online MemP) | 35.1 → 26.3 |
  | Qwen3.5-9B | 55.0 | 77.4 ±1.8 | 69.3 (offline MemP, online ACE) | 32.6 → 21.0 |

  - Naive online imitation, RFT and REINFORCE variants drift and end *below* the base model (✓ §3.2, Fig. 1).
- *Limitations* (✓): needs open weights for full next-token distributions; sparse episode-level gating; equal weighting of turns; the teacher may reconstruct student tokens. It is "label-free" only in that it uses no human labels, since the environment success flag is the verifier.
- *Relevance.* The most direct weights-vs-harness comparison in this chapter.

**11. SERPO: Self-Evolving Rubric Policy Optimization for Open-Ended TTRL** ([2607.26873](https://arxiv.org/abs/2607.26873), Jul 2026; HUST / USTC / Alibaba †; ICLR'27 npwCnxrO6F)
- *What it does.* Replaces answer voting with three things that evolve together:
  - Good–Normal–Bad response archives;
  - query-specific rubrics that are kept only if they discriminate the archives;
  - a criterion score computed from verdict-token likelihoods.
- *Setup:* Qwen3-4B-Instruct-2507 and Qwen3.5-9B on HealthBench and ResearchQA, plus 4 out-of-domain benchmarks, 30 epochs. A hidden GPT-5.1 grader evaluates (✓ §4).
- *Key results:* up to +20.63 on HealthBench and +20.31 on ResearchQA over the base, and up to +8.06 on the six-benchmark macro average (✓ abstract and §1). HealthBench keeps improving through 45 epochs on Qwen3-4B (✓ Fig. 1b caption).
- *Limitations* (✓ §6): two model configurations; one 45-epoch run; rubric generation and judging inherit the deployed model's biases.
- *Critique.* The judge is the same model that is being trained. There is no seed variance on training.

**12. Intrinsic-confidence rewards: INTUITOR / RENT / EMPO / No Free Lunch** ([2505.19590](https://arxiv.org/abs/2505.19590) ICLR'26; [2505.22660](https://arxiv.org/abs/2505.22660); [2504.05812](https://arxiv.org/abs/2504.05812); [2506.17219](https://arxiv.org/abs/2506.17219))
- *What they do.* Reward = self-certainty (INTUITOR), negative entropy (RENT), or semantic-cluster entropy (EMPO).
- *Key results:*
  - INTUITOR matches GRPO on math and generalizes better to code (◐).
  - EMPO lifts Qwen2.5-Math-7B math from 30.7 to 48.1, and Qwen2.5-7B MMLU-Pro from 32.1 to 50.1 (◐).
  - No Free Lunch finds that RLIF helps early, then degrades below the pre-training model, with little gain on instruct models (◐).
- *Critiques:*
  - Spurious Rewards (✓ abstract/§1): GRPO's clipping bias amplifies high-prior behaviors (Qwen2.5-Math "code reasoning" rises from 65% to over 90%) even with random rewards. The effect is model-specific.
  - In the URLVR-scale study (✓ §4.1.2), entropy rewards give repetition collapse and probability rewards give length collapse.
  - In GMAE's head-to-head, INTUITOR and EM-RL are among the weakest label-free baselines, though CoVo is lower at 4B and 8B (✓ Table 1).

**13. Selective-Complementary RL (SCRL) / DDRL / PC-Dr.DPO** — the "fix the pseudo-label" line ([2603.19880](https://arxiv.org/abs/2603.19880), [2604.21327](https://arxiv.org/abs/2604.21327), [2611.09872](https://arxiv.org/abs/2611.09872); CASIA (Liang group) ×2, and MBZUAI / Melbourne / Tokyo)
- *What they do:*
  - SCRL abstains from a positive label when consensus is weak, and adds entropy-gated *negative* labels (◐).
  - DDRL finds that medium-consistency prompts form an "ambiguity region" that is the main source of reward noise, and that GRPO's group normalization *amplifies* it. It applies frequency-based sampling, fixed debiased advantages, and an off-policy consensus refinement stage (◐; ✓ §2.3 heading).
  - PC-Dr.DPO splits majority-vote error into *population-majority bias*, which cannot be identified without labels, and *finite-rollout variation*, which can be mitigated. It uses prompt-conditional robust DPO plus a replay cache. It matches or beats the strongest TTRL baseline in 24/30 settings (✓ abstract and §1).
- *Limitations.* None of them can touch population-majority bias, the confidently-wrong mode. That needs an external signal (S4) or a little labeled data (TRUER).

**14. Test-time Recursive Thinking (TRT)** ([2602.03094](https://arxiv.org/abs/2602.03094), Feb 2026; MSR / UCSD) — the weight-free control
- *What it does.* Per round, the model generates K rollouts under specific strategies, self-ranks them, writes "what to avoid" into a knowledge list, and proposes new strategies. No weights change.
- *Key results* (✓ Fig. 1/3):
  - LiveCodeBench-v6 hard: o4-mini 63.5 → 73.9 and o3 57.1 → 71.9 over 8 rounds, versus RSA at 70.4 / 69.7.
  - gpt-oss-120b and Qwen3-235B reach 100% on AIME-25.
  - Code runs use self-designed test execution, which contributes 7.4 points for o4-mini (✓ §5).
- *Limitations* (✓): coding needs an execution environment; there is a residual selection gap; cost grows linearly with rounds.
- *Relevance.* The comparison point any weight-updating test-time method should beat at matched compute.

**Remaining papers**

| id | title | one-liner | mark |
|---|---|---|---|
| 2506.10947 | Spurious Rewards | Random rewards give +21.4 on MATH-500 for Qwen2.5-Math-7B (GT gives +29.1); little effect on Llama3 or OLMo2; mechanism is clipping bias | ✓ (abstract/§1) |
| 2610.01066 | Random-Reward RL as a Probe | Random-reward RL used to probe reachability | ◐ |
| 2610.reward-placebo-zero-data-self-play | Does the Reward Matter? Placebo Trial for Zero-Data Self-Play | Placebo control for Absolute-Zero-style gains | ◐ |
| 2610.37553 | Collective Dynamics of Self-Reinforcement | Collapse is a collective instability across shared parameters (cross-task coupling); critical N_c ≃ ln(1/δ)/(λ²G); no collapse for confident initial models | ◐ (title and N_c formula confirmed in local text abstract) |
| 2611.01307 | When Does Exploration Prevent Collapse? Three Regimes | Closed-form stability boundaries; a rare-answer bonus needs a coefficient above 1/log(G−1) before normalization, about 2–3× that after | ✓ (abstract and §1 in text) |
| 2611.11165 | The Race Is Decided Early (Kernel Law) | Lock-in is decided early by sampling noise between close answers; probability given in closed form | ◐ |
| 2603.17775 | CoVerRL | One model alternates generator and verifier roles; +4.7–5.9 over label-free baselines; self-verification accuracy rises from about 55% to over 85% | ◐ |
| 2604.25419 | JURY-RL | Plurality proposes, Lean verifies; ResZero fallback when verification is inconclusive | ◐ |
| 2611.13714 | TRUER | Test-time reward correction with an error-aware rectifier trained on a small labeled source set, aimed at wrong majorities and correct minorities; false positives dominate reward errors | ◐ (title confirmed) |
| 2603.16223 | Dual Consensus | Two-stage vote to escape a spurious majority | ◐ |
| 2605.19444 | (see card 5) | — | ✓ |
| 2608.03545 | Hi-TTRL | Hints regulate consensus | ◐ |
| 2608.09324 | CoRE | Consensus reward via equilibrium | ◐ |
| 2608.03119 | OM-GRPO | Outcome-masked GRPO for voting-based label-free RLVR | ◐ |
| 2606.03608 | Confidence-Conditioned Verification (ICLR'27 3iRWDF4SSh) | Exploits the verification–generation gap; verify where confident, explore elsewhere | ◐ |
| 2609.18587 | Label-free Steering (ICLR'27 Dou8KTwVNU) | TTRL compressed into bias-only subspaces | ◐ |
| 2610.32234 | MS-TTRL (ICLR'27 Kgd4RmQdtD) | Frequency conflict when a problem has multiple valid answers | ◐ |
| 2610.16104 | SelectTTRL (ICLR'27 5R65MtzBRK) | Representation-guided data selection | ◐ |
| 2609.09135 | Rank-Masked PO for code TTRL | Answer voting breaks for code; entropy-regularized rank masking | ◐ |
| 2610.00903 | Sharpen Before You Adapt | Data-free sharpening of the entry state before TTRL | ◐ |
| 2608.27448 | TTPO (266 votes) | Test-time policy optimization; bridges RL and OPSD | ◐ |
| 2608.06296 | U-OPSD (301 votes) | Majority-vote pseudo-solution as the teacher context; improves the base by 8.5% / 10.7% (Qwen3 4B/8B, non-thinking; 1.9–2.2% in thinking mode), outperforming baselines including supervised OPSD per the authors | ✓ (abstract/§1 numbers); comparison to supervised OPSD ◐ |
| ICLR'27 LiJUIYVa13 | CANON | Consensus as privileged context; up to +15 pass@1; +6 over label-free RL at 15% of compute | ◐ |
| 2611.18916 | STONE | Agreement moved to easier self-written "stepping stones"; +13.2 / +22.7 non-thinking | ◐ |
| 2611.20691 | SpeCal | Hidden-state spectra recalibrate advantages; +2.7 in-domain, +1.1 out-of-domain | ◐ |
| 2610.21392 | Selective Internal Feedback | Gates confidence rewards when confidence and correctness diverge | ◐ |
| 2605.09920 | Intrinsic Gradient-Norm Reward | Verifier-free reward from gradient norm | ◐ |
| 2605.22620 | Collapse-free Multi-Reward RLIF | Two intrinsic rewards to avoid collapse | ◐ |
| 2603.18363 | PowerFlow | RLIF as distribution matching | ◐ |
| 2606.20881 | When Do Intrinsic Rewards Work for Code? | Study of intrinsic rewards on code | ◐ |
| 2511.04902 | You Need Reasoning to Learn Reasoning | Label-free RL fails on weak bases | ◐ |
| 2509.15194 | EVOL-RL | Majority vote drives selection, a novelty bonus drives variation | ◐ |
| 2508.00410 | Co-rewarding | Cross-view and complementary self-supervised reward | ◐ |
| 2502.19613 | Self-rewarding correction | Self-evaluation within the reasoning trace | ◐ |
| 2505.08827 | RLSR | Reinforcement learning from self-reward (model as judge) | ◐ |
| 2610.04299 | R-Quest | Invalid and repeated questions cause self-evolution collapse; validity and novelty feedback; 10 stable rounds; +17.32 over R-Zero | ◐ |
| ICLR'27 ecw2VvTeE5 | What Actually Evolves in Self-Evolving RL? | Most solver gains come from questions the base already answers; fixes far fewer errors than human-labeled RL | ◐ (from our round-2 sweep) |
| ICLR'27 BKfYuZRiix | Beyond Self-Consensus | "Self-reinforcing exploration bias"; structured exploration; more than 30% fewer samples | ◐ |
| ICLR'27 mjNZLUFK21 | Safety & Reasoning Vulnerabilities in Self-Rewarding TTRL | In mixed streams, TTRL collapses jailbreak responses to one refusal template (54–78% identical), giving apparent safety plus a "reasoning tax" | ◐ |
| ICLR'27 IVFASEtqp6 | Fragility of Self-Rewarding Optimization | Self-reward fidelity has a phase transition; prompt-level attacks (re-ranking, preamble) inflate self-reward | ◐ |
| ICLR'27 yHVcEoxOlW | Score Saturation in Self-Rewarding LMs | Theory: preference optimization pushes scores to the rubric top, so informative pairs vanish | ◐ |
| ICLR'27 Og5SkVIfuE | PROSE (TTRL on Medical QA) | TTRL collapses on small answer spaces because wrong answers collide; process-reward self-training fixes it | ◐ |
| ICLR'27 Gbqu33OFyN | TTRL alignment exposes task-familiarity artifacts | TTRL used as label-free "train-before-test" for fair benchmarking | ◐ |
| ICLR'27 Cuk60HQcrN | TCCA | Target-conditioned credit assignment; beats TTRL in 12/12 single-run settings | ◐ |
| ICLR'27 OCZ3eKMN9b | RCTS | Teacher conditioned on a reflection; token selection by cross-group divergence | ◐ |
| ICLR'27 DshkmNnrso | CoVER | Contrastive self-verification plus programmatic feedback | ◐ |
| 2611.19700 | Where Should LLM Agents Adapt at Test Time? | In context vs in weights vs both, for repeated environments | ◐ |
| 2611.21885 | Language Models Can Rewrite Their Own Weights | A 27B model edits its own weights by reasoning, without gradients; +6.6 on 490 unseen problems; only 27B works (2B makes no edit; 4B and 9B break themselves); uses graded answers | ✓ (abstract/§1 numbers; "graded answers" ◐) |
| 2608.20290 | Phantom Gains | 3 rounds of LoRA self-training on Qwen3-8B vs a frozen control; 7 measurement failures, each inverting a reported finding | ◐ (project LITERATURE.md) |

### Consensus, debates & contradictions

**Consensus (multiple independent full-text confirmations):**
1. **Sharpening, not discovery.**
   - Intrinsic and consensus rewards push the policy toward its own mode (✓ 2603.08660 §3).
   - Per-problem accounting finds 0.7% learned versus 21.6% degraded (✓ 2605.19444).
   - TTSP and OPST both state that coverage of the correct answer is required (✓).
2. **Rise-then-fall with scale of training data or steps.**
   - SRT collapse (✓). URLVR-scale: ≤128 stable vs ≥512 collapse (✓). Collective-instability theory (◐). Kernel-law lock-in (◐).
   - The usual knobs (KL, GRPO vs RLOO) do not fix it; a lower learning rate delays it (✓ SRT Fig. 8).
3. **Base-model dependence.** Qwen models are more stable and LLaMA models collapse fast (✓ 2603.08660 §6.1). Spurious and random rewards work mainly on Qwen2.5-Math (✓ 2506.10947 §1). Most positive results are on Qwen.
4. **Grounding helps when the checker is good, and hurts when it is weak** (✓ T3RL §6 / App. Table 8; ◐ JURY-RL; ✓ TTSP verifier sweep).
5. **Dense self-distillation beats scalar agreement RL on the same self-generated signal** (✓ OPST Table 1: +7.3 vs +1.0 for GRPO; ◐ CANON, U-OPSD, STONE).

**Debates and contradictions:**
- **Does majority vote "add information"?**
  - Prompt-level view (He et al.): no, it only sharpens.
  - Batch-level view (Wilson et al. ✓): it transfers information across prompts. Wrong-majority prompts gain correct majorities at about 3× the resampling rate, and TTRL's average beats the initial maj@64 (✓ TTRL Fig. 7).
  - Both can hold. The batch-level equivalence is *local* (✓), and collapse appears when errors become coherent, for example through a shared template answer.
- **Is "lucky hit" reassurance or a warning?** TTRL argues weak models get *more* accurate rewards because their errors are scattered (✓ §4.2). Extinction-window, SCRL and DDRL show that medium-consistency prompts are exactly where spurious positives get amplified. Weak consensus is not harmless once group normalization rescales it.
- **Does GRPO's normalization help or hurt?**
  - DDRL: it amplifies spurious signals (◐).
  - Three Regimes: normalization leaves stability boundaries unchanged when exploration passes through it, and shifts them (by about 2–3×) when it bypasses it (✓ §1).
  - Wilson et al.: dividing by the group standard deviation moves c∞ by at most 0.010 (✓ §2).
  - The claims are at different levels (amplitude vs direction) and are not yet reconciled.
- **Small data: safe or useless?** URLVR-scale says small datasets and test-time training are the safe use (✓ §5). SRT says the gains are comparable to RLVR on full training sets before collapse (✓ Table 1). The practical recipe — stop early, use a small or transductive set, prefer Qwen — has no label-free stopping rule (OPST limitation ✓).
- **Benchmarks and contamination.** TTRL trains on AIME (30 problems) and MATH-500 test sets. TTRL alignment is now proposed as a *benchmarking* tool (◐ Gbqu33OFyN). Reported gains therefore partly measure format and familiarity, a point also made by the spurious-reward results.
- **"Label-free" is used loosely.** T3RL, JURY-RL, ASCENT and TTSP use executors, provers or environment success flags. SERPO uses a GPT-5.1 grader only for evaluation. TRUER uses labeled source data.
- **Is this recursive self-improvement?** T3RL calls TTRL a recursive self-improvement paradigm (✓ abstract). By Duan et al.'s structural-vs-effective distinction, none of these papers revises the improvement mechanism (the reward rule) and shows a better successor under matched budgets. CoVerRL's co-evolving verifier is the closest, and it is ◐ only.

### Open problems

1. **Separate learning from sharpening, by default.** Report per-problem migration (initial vs final label accuracy, as in 2605.19444) and large-k pass@k next to pass@1 and maj@k. Include a random-reward placebo arm (2506.10947, 2610.01066) and at least one non-Qwen family. Almost no method paper does all three.
2. **Error-coherence diagnostics before training.** Wilson et al. show coherence decides success. Can a cheap gradient-sketch coherence score at step 0 predict Model Collapse Step, the collective N_c, or kernel-law lock-in, and so tell you when to switch to grounded rewards?
3. **Population-majority bias.** No label-free method can correct a confidently wrong mode (PC-Dr.DPO's own decomposition). Open question: what is the minimum external signal (bits per prompt) that removes it? Candidates are TRUER-style rectifiers, cheap tools and small labeled sets.
4. **Label-free stopping and model selection.** Collapse is sudden (SRT), yet papers commonly pick peak checkpoints with gold labels, as in SRT Table 1 "peak performance". A label-free early-stopping rule (flip rate, KL spike, self-reward slope) that is validated across families is missing.
5. **Open-ended and agentic domains.**
   - SERPO's self-rubrics are judged by the trained model.
   - Score saturation and self-reward-fidelity results predict collapse.
   - Safety-in-mixed-streams shows refusal-template collapse.

   There is no study of long-horizon self-reward fidelity in agents. ASCENT side-steps it with an environment verifier.
6. **Matched-compute baselines.** Weight-updating test-time RL rarely compares with weight-free test-time scaling (TRT, functional majority voting, rejection sampling) at equal FLOPs. TTSP and ASCENT are exceptions.
7. **Heterogeneous and adversarial streams.** Online test-time RL on mixed traffic amplifies whatever the reward scores most easily (◐ mjNZLUFK21), and prompt-level attacks inflate self-reward (◐ IVFASEtqp6). No defense has been validated across methods.
8. **Effective recursion.** Can the reward-construction mechanism be revised by the system (a self-written reward rule or verifier) and shown to produce better successors under independent evaluation? Duan's "effective recursion" test is unmet in this whole corner.
9. **Weights vs harness at test time.** ASCENT beats harness methods on ALFWorld (✓). But "Where should agents adapt" (◐ 2611.19700) and harness-evolution results elsewhere in the atlas leave open when weight updates beat memory or skills, and whether gains compose.
10. **Coverage floor.** TTSP fails below mean@512 ≈ 0.1. Does any label-free method lift problems the base model has near-zero coverage on, beyond the 0.7% "learned"?

### Where it is heading (to mid-2027)

- **Consolidation into label-free self-distillation.** On-policy self-distillation toward a consensus- or stepping-stone-conditioned teacher (U-OPSD, CANON, OPST, STONE, RCTS, ASCENT) is displacing scalar majority-vote GRPO. Expect "privileged-context design" to be the main design axis, along with audits such as 2611.16064 (◐) asking whether privileged information really gives privileged guidance.
- **Hybrid grounding as the default.** Votes propose; tools, Lean, execution or small labeled rectifiers dispose. Papers that use pure intrinsic rewards will be expected to show a cross-family placebo.
- **A theory wave.** Batch coherence, collective N_c, kernel-law lock-in and three-regime exploration bounds appeared within weeks of each other (Oct–Nov 2026). Expect a unifying account and practical monitors (flip rate, Model Collapse Step) in training stacks by mid-2027.
- **Moving into agents and open-ended tasks.** Online test-time training for agents (ASCENT, HIRE kbCX1FVPSj ◐), rubric test-time RL (SERPO, RuACT, Rubric Curriculum RL ◐), and test-time RL for multimodal, video and diffusion LMs (many ICLR'27 submissions ◐).
- **ICLR 2027 decisions (late January)** will likely accept several "fix the vote" papers (GMAE, TRUER, SelectTTRL, SPINE, TCCA) and the audits (safety vulnerabilities, score saturation, SRO fragility). This will raise the evidence bar for NeurIPS 2027 submissions: per-problem accounting, placebo arms, non-Qwen bases, and label-free stopping.
- **Self-referential weight editing** ("LMs Can Rewrite Their Own Weights", ✓ §1 for scale results) may open a separate line: weight edits made by the model's own reasoning rather than by gradients. Right now it depends on graded answers and 27B scale.

### Connection to our ideas

Dan's ideas (`ONE_PAGERS.md`) work at the harness and improver level, not the weight level. This corner still supplies mechanisms, controls and threats for them:

- **#3 Gate, Not Willingness, and the N_e / bloat idea → Wilson et al. (2610.20898).**
  - A majority-vote reward is a *gate* built from noisy self-evaluations.
  - Their result — *coherence of errors, not error rate*, decides whether the gate tracks truth, and random flips average out with batch size while shared-rule errors leave a floor — maps directly onto harness gates.
  - For harness gates, the coherent error is a shared evaluator bias across validation tasks. That is the failure a "keep it if the score went up" gate cannot average away.
  - A cheap borrowed measurement for #3 is the cosine between gate decisions and oracle decisions on held-out tasks, split into coherent and incoherent parts. It is a quantitative answer to "proposer vs gate".
- **#5 Order-0 Rewards Breed Routers.** This corner is the cleanest weight-level evidence that self-consistency rewards select the cheapest pattern the reward can score:
  - template `\boxed{1}` collapse (SRT ✓);
  - refusal-template collapse (◐ mjNZLUFK21);
  - GRPO, rewarding agreement with self-generated answers, barely moves the model while dense context-conditioning does (OPST ✓).

  Cite these as the weight-level analogue of "router edits". The meta-reward comparison can borrow the placebo-twin design from Spurious Rewards, the random-reward probe and the zero-data self-play placebo trial.
- **Improvement by Inheritance / "one good early edit" → lock-in results.**
  - Extinction-window (✓), kernel-law (◐) and URLVR-scale (✓) all show that outcomes are decided *early* and are irreversible under self-reinforcement.
  - That supports reading inheritance-dominated gains as a lock-in phenomenon.
  - Flip rate and Model Collapse Step are cheap *diagnostics* to add to nested replay. They track how fast an improver lineage stops exploring.
- **#1 Taught, Not Selected.** The DDRL finding that group-relative normalization *amplifies* the noisiest signals, and PC-Dr.DPO's split into population bias vs finite-sample variation, give a vocabulary for "what the reflector reads" vs "what the score selects". Population bias corresponds to a systematically misleading transcript, which no amount of resampling fixes.
- **#6 Meta-Overfitting.** Label-free RL generalizes out of distribution in some reports (TTRL Fig. 4 †; URLVR §5.3 ✓) but regresses in others (OPST MATH-500 −4.9 ✓; TTRL GPQA −1.4 on Qwen2.5-Math-7B ✓). That is precedent that "home" sharpening can hurt far domains, which supports #6's prediction.
- **A cheap pilot, and a scoop check.**
  - A label-free *harness gate*: accept an edit if the majority of K replays agree it helps, then compare against an oracle gate. This would test the Wilson coherence criterion at the harness level.
  - Our searches (alphaXiv, ICLR'27 OpenReview) found no paper applying TTRL-style consensus rewards to *improver or harness edits*. That is not exhaustive, because S2 was rate-limited.
- **Threat to the order-1 ideas.** If weight-level self-training (ASCENT, OPST) keeps beating harness evolution on agent benchmarks (ASCENT ✓ beats GEPA, ACE and MemP on ALFWorld), reviewers will ask why improvers should edit prompts at all. Ideas #7–#9 should cite ASCENT and 2611.19700 and frame harness self-editing as complementary. The "varying goals" condition is the cleanest place to argue for harness edits over weight updates.

*Search coverage:*
- alphaXiv: 7 searches, including date-filtered ones after 2026-10-25 and 2026-11-01.
- ICLR 2027 OpenReview: 6 term searches plus 10 abstract pulls by title. Fetching notes by id is blocked by a challenge, which was not bypassed.
- Semantic Scholar: rate-limited (HTTP 429). Only one metadata hit, so no forward-citation sweeps.
- Full text read (methods, results, limitations): 2504.16084, 2505.21444, 2603.08660, 2610.20898, 2605.19444 (§1), 2609.36750, 2603.02203, 2610.32770, 2611.07367, 2610.05303, 2602.03094, 2611.01307 (§1). Fact-check pass additionally opened local full texts of 2506.10947, 2607.26873, 2608.06296, 2611.09872, 2611.21885, 2610.37553, 2611.13714, 2604.21327.
- Abstract or intro only: the rest.

#### Fact-check log

| Claim | Verdict | Source location |
|---|---|---|
| TTRL Qwen2.5-Math-7B AIME24 12.9→40.2, avg 31.1→54.9, GPQA 29.1→27.7; MATH-500 46.7→83.4 (+36.7) | confirmed | 2504.16084 Table 1 (paper.txt l.337–338) |
| TTRL Qwen3-8B avg 53.8→64.5 | confirmed | 2504.16084 Table 1 (l.355–356) |
| TTRL Mistral-Nemo AIME 0.8→0 | confirmed | 2504.16084 Table 2 (l.422–424) |
| TTRL "lucky hit" 37% label acc vs 92% reward acc | confirmed | 2504.16084 §4.2 (l.787–791) |
| TTRL MATH-500 levels +45.4 (L1) to +16.8 (L5); length −43.7% to −73.3% | confirmed | 2504.16084 Table 3 |
| TTRL hyperparameters (64 vote / 32 train, peak LR 5e-7, 10/30/80 episodes, 8×A100, 16 samples for pass@1) | confirmed; added "4 samples for 32k context", LR is cosine peak, and episodes map to MATH-500/AMC/AIME | 2504.16084 §3.1 (l.369–405) |
| URLVR: ≤128 stable, ≥512 collapse; DAPO-32 never / DAPO-512 always across 3 seeds | confirmed; corrected that 3 seeds apply only to sizes {32,128,512}, not the whole sweep | 2603.08660 §5.1 (l.978–986) |
| URLVR: 3/25 problems flip greedy correctness | confirmed | 2603.08660 §4.2.1 (l.910) |
| URLVR: Model Collapse Step 5.6× cheaper | confirmed (1.19B vs 6.66B tokens over 7 models) | 2603.08660 Table 3, §6.3 |
| URLVR §6.1 LLaMA base fails by step 40; §5.3 wrong-majority 32-sample run still helps OOD | confirmed; clarified Qwen base drops near zero by step 200 while SFT variant is stable | 2603.08660 §6.1 (l.1140–1152), §5.3 (l.1059–1110) |
| SRT Table 1 values (0.15/0.18/0.21/0.20/0.31/0.36 DAPO; 0.15/0.18/0.23/0.20/0.32/0.33 MATH-12K) | confirmed; added that Table 1 is Qwen2.5-Math-7B | 2505.21444 Table 1 (l.381–387) |
| SRT models "Qwen2.5-Math-7B, Llama-3.1-8B-Instruct, Qwen3-4B-Base" | corrected: main math runs also include Qwen3-14B-Base and DeepSeek-Math-7B-Instruct; Qwen3-4B-Base is the Reasoning Gym curriculum model | 2505.21444 Fig. 3/4 (l.261, 279), Fig. 5 (l.430) |
| SRT collapse after ~1200 steps on MATH-12K; KL/GRPO-vs-RLOO don't prevent; lower LR delays | confirmed | 2505.21444 App. C Fig. 11 (l.1394), Fig. 8 / §4.2 (l.565–584) |
| Wrong Votes: <1% own-gradient share, 164/256 mixed prompts, 30% random flips harmless, ~3× resampling rate, c∞ moves ≤0.010 | confirmed; Llama models corrected to -Instruct variants | 2610.20898 §1 (l.60–90), §2 (l.130–152), §5 (l.414), Limitations (l.562) |
| Extinction Window category shares 44.5/0.7/21.6/26.2 | confirmed (four of six categories) | 2605.19444 Fig. 1 / §1 |
| TTRL-Guard +54% relative on AIME25; Llama degraded 60.2%→28.0% | confirmed in §1 (upgraded ◐→✓) | 2605.19444 §1 (l.101–121) |
| GMAE Table 1 averages (all 16 numbers) and "within 1.0 point" at 1.7B; 30/32 cells; +2.0–2.8 | confirmed | 2609.36750 Table 1 (l.510–538), §5.2 (l.600–604) |
| GMAE "10 self-rewarding baselines" | corrected to 9 (7 ensemble + 2 probability-based) plus an RLVR reference | 2609.36750 §5.1 Baselines |
| GMAE "Intuitor and EM-RL weakest in every block" | corrected: CoVo is lowest at 4B (22.9) and 8B (30.1); SCOPE (15.1) below EM-RL (15.2) at 1.7B | 2609.36750 Table 1 |
| GMAE "run-to-run variance 0.48" | corrected: std of mean@16 averaged over 8 benchmarks, Qwen3-8B-Base, 5 seeds; GMAE0 0.48 vs TTRL 0.69 | 2609.36750 §5.3, Fig. 4 (l.674–678) |
| T3RL 15/15 wins; Qwen3-4B 36.4→40.0; Llama-3.2-3B 13.3→17.1; Qwen-Math-1.5B avg 45.9→48.8; +0.2%→+4.3% | confirmed | 2603.02203 §5 (l.461–462, 481–483), Table 1 (l.367–398) |
| T3RL weak verifier worse than voting | confirmed; specified Qwen-Coder-0.5B, −1.4 avg | 2603.02203 App. Table 8 (l.1015–1027) |
| T3RL TTRL baselines copied "where possible" | confirmed | 2603.02203 Baselines (l.440–443) |
| OPST E2B no-think +10.2/+4.0/+7.5; GRPO +1.0 vs OPST +7.3 | confirmed | 2610.32770 Table 1 (l.433–437), §4 (l.474–476) |
| OPST vs R-Zero +1.4–5.2, vs u-OPSD +1.0–2.8, 88% of supervised OPSD | confirmed (+7.3 of +8.3, E2B without thinking) | 2610.32770 §4 (l.483–487), Table 2 |
| OPST 75% wrong answers −1.9 vs SFT −5.7; placebo +2.7; MATH-500 −4.9 | confirmed | 2610.32770 Table 5b/5c, §5; Table 3 (l.552) |
| TTSP: VeryHard gain ≈0 below mean@512≈0.1; HardLCBv6 definition; 7 verifier strengths; affiliation | confirmed (ETH Zurich & Tufa Labs; 7 strengths enumerated) | 2611.07367 §5 (l.401–413, 616–621), Fig. 7; title page |
| TTSP 35-point mean@8 gain without verifier | unverifiable beyond abstract (kept ◐; abstract says "35% absolute") | 2611.07367 abstract (l.32) |
| ASCENT Table 1 (46.4→69.5±5.6 vs 63.6 online MemP; 55.0→77.4±1.8 vs 69.3; turns) | confirmed; baseline list expanded to match table | 2610.05303 Table 1 (l.467–502) |
| TRT o4-mini 63.5→73.9, o3 57.1→71.9, RSA 70.4/69.7, AIME-25 100%, 7.4 pp from test execution | confirmed | 2602.03094 Fig. 1/3 (l.29–57, 220, 257) |
| Three Regimes 1/log(G−1) threshold, 2–3× after normalization | confirmed | 2611.01307 abstract/§1 (l.42–43, 102–104) |
| Spurious Rewards +21.4 vs +29.1 on MATH-500; code reasoning 65%→>90%; not Llama3/OLMo2 | confirmed (upgraded ◐→✓) | 2506.10947 abstract/§1 (l.25–46) |
| SERPO +20.63/+20.31/+8.06; GPT-5.1 grader; 45-epoch run | confirmed; base model is Qwen3-4B-Instruct-2507 | 2607.26873 abstract/§1 (l.34, 164), §4 (l.429, 469), Fig. 1 caption (l.81) |
| U-OPSD +8.5 / +10.7 | corrected wording: paper reports "8.5% and 10.7%" (non-thinking, 4B/8B) | 2608.06296 abstract/§1 (l.35, 123) |
| PC-Dr.DPO 24/30 settings; affiliation | confirmed; affiliations are MBZUAI / Melbourne / Tokyo | 2611.09872 abstract/§1 (l.38, 128); title page |
| "LMs Can Rewrite Their Own Weights": 27B, +6.6 on 490 problems; 2B no edit, 4B/9B break | confirmed | 2611.21885 abstract/§1 (l.19–30, 104–105) |
| Collective Dynamics N_c ≃ ln(1/δ)/(λ²G), ∝ λ⁻² | confirmed in abstract text (kept ◐) | 2610.37553 abstract (l.36–46) |
| ID check: 2610.37553 → "Collective Dynamics of Self-Reinforcement…" | confirmed | 2610.37553 title page |
| ID check: 2611.13714 → "TRUER: Test-Time Reward Correction Using an Error-Aware Rectifier for RL" | confirmed | 2611.13714 title page |
| ID check: 2611.21885 → "Language Models Can Rewrite Their Own Weights" | confirmed | 2611.21885 title page |
| ID check (extra): 2604.21327 → DDRL "Understanding and Mitigating Spurious Signal Amplification in TTRL…" (Jian Liang, CASIA) | confirmed | 2604.21327 title page |
| TTRL Fig. 4 OOD generalization | unverifiable in this pass (downgraded ✓→†) | not re-opened |
| TTRL NeurIPS 2025, alphaXiv vote counts, survey placements, Gao et al. §5.1 labeling | unverifiable in this pass (marked †) | not re-opened |

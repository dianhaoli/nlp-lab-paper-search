# Critique: Do safeguards survive internalization? Oversight after harness-to-weights distillation
Topic: T5 Harness<->weights and model upgrades · Origin: Dan-modified · Reviewed 2026-11-12

## 0. Verdict up front
**Total 63.8/100: viable with fixes.** No kill flag fires outright. K4 ("Masked Erosion + HERD") is borderline.
The gap is real. No paper I found measures *per-safeguard* retention, or retention of *redundant* (masked) safeguards, after a harness is distilled into weights.
The headline as written ("masked safeguards drop at much higher rates") is the *expected* result once you have read Masked Erosion. The "audits lose most signal" half is close to tautological unless it is redefined.
The idea becomes NeurIPS-worthy only if it turns into a **mechanistic contrast between distillation signals**: can logit-level or on-policy self-distillation carry information about masked rules that trajectory-SFT provably cannot? It also needs a **layer-targeted stress battery**, because you cannot "delete a layer" from weights.

## 1. Scoop search (what I ran)
- alphaXiv discover (recency), 6 calls: harness distillation + safety rules; Masked Erosion/Provenance/Latent Harness/HERD; context distillation of system-prompt policies for tool agents; monitorability after internalization; agent distillation safety degradation; harness removal + redundant rules + defense in depth.
- Semantic Scholar keyword search (2026): 4 phrasings ("harness distillation safety constraints retention", "context distillation safety rules forgotten agent", "internalizing guardrails into weights agent permissions", "distilling scaffold into model weights oversight monitor"). Nothing on target.
- S2 forward citations of 2610.38392, 2610.31401, 2609.24974, 2611.01415, 2610.21458, 2610.02858, 2610.20261. Masked Erosion, Provenance Marks, HPD and Harness-Aware Distillation have 0 citers so far. Harness-Zero citers include Action Shaping 2609.32752, "Your Policy Is Secretly a Harness" 2610.37255, Ceiling 2610.11655 and Self-Evolving Coding Agents via Harness-Policy Opt 2610.22791. Latent Harness has one citer: SKILL2STEER 2611.04497.
- Deep-read (answer_pdf_queries): Masked Erosion 2610.38392, Harness-Zero 2609.24974, Latent Harness 2611.01415, HERD 2610.31401, Harness Policy Distillation 2610.20261, Policy-Verified Self-Training 2610.19372, Beyond Gain / Skill Internalization 2610.32322, Action Shaping 2609.32752.

### Closest papers and the gap each leaves
| Paper | Date | What it does | Overlap | Gap left |
|---|---|---|---|---|
| **Masked Erosion** 2610.38392 (So, Liu, Long) | 2026-10-28 | Full 16-subset harm lattice over 4 safeguards in AppWorld (credential misuse) + OpsSim (destructive cleanup). Runs 411 loops under 10 gates. Masked deletions are invisible to gates. Lost margin becomes harm under weaker models (Qwen3-30B misuse 0.30 vs 0.004). The fix is a margin-aware gate. | Same safeguard-lattice/margin machinery and the same "redundancy erodes unseen" thesis. Frames the model *swap* as a common-cause failure. | Fixed model only: no weight updates, no distillation. Its appendix warns that strong models mask rules themselves (τ²-bench deletions change nothing). Distillation is the obvious next "change" and they did not run it. **Highest scoop risk: the authors' own follow-up.** |
| **Harness-Zero** 2609.24974 (PKU/Google) | 2026-09-21 | Agent-as-harness review, then SFT into Qwen3.5-9B. Harness removed at deployment. Measures *per-pattern recovery*: 82.3% over 28 harness-exclusive patterns, incl. middleware such as "protect pre-filled cells" (92%) and "avoid repeated failed calls" (71%). | Per-behavior retention after harness removal is already a metric. | Recovery is scored only on tasks where the pattern *appears* (behaviorally visible). Masked safeguards are by definition not visible, so they cannot be scored. No safety, no redundancy, no audit. |
| **Latent Harness** 2611.01415 (HKUST) | 2026-11-01 | Compiles a harness into rank-8 post-block activation shifts. "Harmful harness compiles into harmful latent with the same fidelity." The shift substitutes for structure the harness imposes, even beyond what the text covers. | Symmetric-fidelity claim: whatever behavior is visible gets compiled. | No safety rules, no per-rule retention, no redundancy. The ethics section says bundles "should be audited" but gives no audit method. |
| **HERD** 2610.31401 (Monash) | 2026-10-22 | Evolve harness, run drift-constrained self-distillation (teacher = checkpoint + skill + verified answer), deploy bare weights, repeat for 25 stages. Re-attaching the harness changes ≤0.4 pt ("already internalized"). | A recursive harness-to-weights loop: the natural "multi-round" arm. | Pure capability/forgetting metrics. No rules, safety or audit. |
| **Policy-Verified Self-Training (PVST)** 2610.19372 | 2026-10-14 | Uses a compiled policy gate's verdicts as RFT/DPO signal. Violations 37.2→10.3% (AgentDojo). Gate interventions 39→9%. **Part of the gain is avoidance** (stops calling guarded tools). Identifiability condition: guarded actions must appear in both permitted and forbidden contexts. | Internalizing a runtime safety gate into weights. | One gate, no redundancy, no layer structure. The key gap is its identifiability condition: masked layers violate it by construction. |
| **Harness Policy Distillation** 2610.20261 (NJU) | 2026-10-14 | Distills runtime semantics (permissions, workflow stages, termination) into Qwen3-8B. Runtime compliance (RC) 19.9→49.2%. | Reports a compliance metric after the harness is internalized. | Aggregate RC only: no per-rule or safety-critical breakdown, no redundancy. |
| Beyond Gain / skill retirement 2610.32322 | 2026-10-23 | Gain-based "internalized" verdicts are invalid. A level rule plus a witness-model validity gate is needed. Verdicts flip across models for 14-23% of skills. | Measurement theory for "is X internalized?" | It is about skills, not safety, and has no distillation. Its "uncovered vs internalized" logic is directly reusable as a validity check for the safeguard-retention metric. |
| Action Shaping 2609.32752 | 2026-09 | "Policies absorb what they can express, and only that." An offset is absorbed iff the base can reproduce it exactly. Cites Harness-Zero as the language-agent analogue. | Theory of which training-time scaffolds are absorbed. | RL control only (TD3/SAC). It offers a possible theory hook ("absorbability") for safeguards. |
| Do Agents Still Follow the Rules? (compaction) 2611.03223 | 2026-11-02 | Rule compliance after context *compaction*. | Analogous "rules lost when the context is transformed". | Compaction, not weights. |
| Distillation for Incrimination (Redwood) 2610.11012 | 2026-10-07 | Distills a possibly-misaligned model into a weaker one so audits become informative. | Distillation × auditability. | Opposite direction (distillation *helps* audits). This gives a nice counterpoint for the framing. |
| Context: OPSD drift 2610.07842; Privileged Retrospective Distillation 2610.27495; Harness Annealing 2610.01235; Harness-Aware Distillation 2610.02858; Compaction Laundering 2610.29869; Compositional safety failures in harness evolution 2609.33123 (cited inside Masked Erosion, not opened) | Sep–Nov 2026 | The harness-distillation and harness-safety neighbourhood is very active. | — | None measures safeguard retention or redundancy after internalization. |
| Unverified (not opened this round) | — | OPSD 2608.13040, Provenance Marks 2610.21458. Provenance Marks is about plagiarism/watermarks in copied harnesses, so it is tangential to safeguards. | — | — |

**Scoop summary:** there is no direct scoop (K1 does not fire). However, (a) Masked Erosion supplies the full lattice/margin/gate apparatus and names model change as the threat, (b) Harness-Zero already reports per-behavior recovery after harness removal, and (c) PVST already reports that internalized compliance partly becomes avoidance. The incremental gap is "redundancy × distillation × audit". The window is probably 2-4 months: 5+ harness-distillation papers appeared between Sep and Nov 2026, and any of them could add a safety appendix.

## 2. Mock NeurIPS review (harsh but fair)
**Summary.** The paper asks whether safety rules embedded in an agent harness survive when the harness is distilled into weights. It distills factorial safeguard subsets with three methods in two environments, and measures per-safeguard retention and monitor detection after harness removal.

**Strengths.**
- It is a timely and policy-relevant question. Harness distillation (Harness-Zero, HERD, Latent Harness, HPD) is spreading quickly, and nobody has audited what happens to safety content.
- The factorial safeguard design inherits a clean measurement object (the harm lattice and margin) and gives exact per-subset ground truth.
- It is cheap and reproducible with 4-8B open models.

**Weaknesses.**
1. **The main claim is predictable.** Distillation learns from behavior. A masked safeguard produces no behavioral difference on the training distribution (that is what "masked" means: Masked Erosion Eq. 3, h01 ≤ τ). So of course the student does not learn it. A reviewer will say: "This is Masked Erosion's mechanism applied to an SFT dataset; the result follows from the definition." Without a condition under which masked rules *could* be learned, the paper has no tension.
2. **"Retention of a masked safeguard" is not identifiable as stated.** You cannot delete a safeguard from weights to test margin. Defining retention requires either (a) a stress that defeats one layer at a time (layer-targeted perturbations) or (b) a probe of internal representations. The proposal specifies neither. "Per-safeguard retention after harness removal" for a masked layer is currently undefined.
3. **The audit claim is near-tautological.** If the "harness-level audit" reads rule-firing logs or rule citations that only exist when the harness is present, then the audit loses signal by construction. Action-level violation detection is unchanged by distillation, because the actions are the same objects. The interesting version is *predictive/early-warning* monitorability (do trajectories still verbalize the check?) or *margin auditing* (can you estimate remaining depth from behavior?). Neither is specified.
4. **Floor effects and confounds** (warned in Masked Erosion App. A). Strong models already follow these rules unprompted, and weak models follow none. Distilled-student differences may reflect the base model's priors, not internalization. A no-harness SFT control (same trajectories, rules stripped from the teacher) and a base-model-prior arm are required.
5. **Three distillation methods ≠ a mechanism.** HERD-style, Harness-Zero-style and Latent-Harness-style differ in data source, loss, parameterization and on/off-policy all at once. Without a factorized comparison (sampled-SFT vs full-logit KL × off- vs on-policy), the ranking of methods is "engineering".
6. **Scope.** Two synthetic environments, 4 safeguards each, small models. Generality beyond "credential misuse" and "RBAC SQL" is unclear. One harm per environment repeats Masked Erosion's main limitation.
7. **Avoidance confound** (PVST 2610.19372). An apparently "retained" safeguard may just be tool avoidance. You need permitted/forbidden paired contexts and utility on legitimate guarded actions.

**Questions for authors.** How is retention of a masked layer operationalized? Does any condition predict that masked rules survive? Is the monitor measuring something distillation could change? What fraction of "retention" is avoidance?

**Likely scores as currently framed:** Soundness 2/4, Presentation 3/4, Contribution 2/4. **Overall 4 (borderline reject).** Typical comments: "natural extension of Masked Erosion", "result follows from the definition of masking", "better suited to a safety workshop (SoLaR / SafeGenAI)".
**With the fixes in §4:** a plausible 5-6 (borderline/weak accept), especially with the logit-vs-sample contrast and a predictive theory of which layers survive.

## 3. Rubric scores
| Dim | Score | Rationale |
|---|---|---|
| N (×3) | 3.5 | No paper runs safeguard-redundancy × distillation × audit. But Masked Erosion (lattice/margin/model-change threat), Harness-Zero (per-pattern recovery), PVST (internalized compliance becomes avoidance) and Latent Harness (harmful content compiles faithfully) together cover much of the surrounding ground. The authors' own follow-up is a live scoop risk. |
| S (×2.5) | 3.0 | As stated, the claim is the expected outcome, and the null ("safeguards survive") is publishable mostly at a safety venue. Rises to ~4 with the logit-vs-sample contrast, where either outcome is informative. |
| R (×2) | 2.5 | It touches harness→weights in self-improvement loops, but the core is safety/oversight rather than self-referential improvement or long-horizon RSI. Adding a multi-round HERD-style loop ("does margin decay per internalization round?") would raise it to ~3.5. |
| C (×2) | 3.0 | Factorial lattice and deterministic audits are strong. Weak points: two synthetic environments, an ill-defined retention metric for masked layers, a tautology risk in the audit claim, and unfactorized methods. |
| F (×1.5) | 3.5 | Qwen3-4B/8B LoRA. 16 subsets × 2 key methods × 3 seeds ≈ 100-200 short runs ≈ $1-3k GPU. Pilot (one environment, 4 subsets, SFT only) in about a week for under $100. Reproducing Latent Harness and HERD faithfully is engineering-heavy. |
| T (×1) | 4.0 | Harness distillation is the hottest Sep–Nov 2026 thread, and safety of self-evolving agents is rising. Still slightly ahead of the wave on the safety angle. |

**Total = 20 × (3·3.5 + 2.5·3 + 2·2.5 + 2·3 + 1.5·3.5 + 1·4)/12 = 20 × 38.25/12 = 63.75 ≈ 63.8 (viable with fixes).**

**Kill flags**
- K1: no. There is no same-claim, same-design paper.
- K2: borderline. "Masked rules drop under SFT" is predictable, and "they survive" is weakly interesting. Fixed by the signal-type contrast below.
- K3: no. Open models only.
- K4: borderline. Read naively, the paper is "Masked Erosion's lattice + HERD/Harness-Zero distillation". It must contribute a new mechanism or theory, not just a new combination.

## 4. Fixes (ranked by score impact)
1. **Make the core question a test of the distillation *signal*, not a survey of methods.** Theory: from sampled trajectories, a masked layer is unidentifiable (same actions with or without it, so zero gradient signal for it). Full-distribution, context-distillation-style KL (teacher sees all rules; student matches token logits on student rollouts) *can* carry it, because the teacher's next-token distribution shifts subtly even when the argmax action is the same (e.g. probability mass on a verification call, or verbalized checks).
   Prediction: masked-layer retention ≈ 0 for sampled-SFT, > 0 for logit-KL, and scaling with teacher-distribution divergence D_KL(teacher_with_layer ‖ teacher_without_layer) on the training states. That divergence is a **pre-distillation predictor** of which safeguards survive.
   Factorize: {sampled-SFT, logit-KL} × {off-policy, on-policy}, with the same data. Either outcome is publishable: "defense-in-depth is only transferable through logits", or "it is not transferable at all".
2. **Operationalize retention with a layer-targeted stress battery.** For each safeguard s, build perturbations that defeat *every other* layer while leaving s's trigger intact (e.g. forged owner prefix defeats AUTH, a matching-credential decoy defeats VERIFY). Retention(s) = protection under the "only s remains effective" stress. This gives a per-layer margin for weights, comparable with Masked Erosion's harness margin. Validate it on the harness model first (it must reproduce the lattice).
   A second, cheaper operationalization is a **model-swap analogue**: distill into a *weaker* student, which mirrors Masked Erosion's weaker-executor finding.
3. **Redefine the oversight half to avoid tautology.** Measure (a) *verbalized-check rate*, i.e. whether the trajectory still states the check (CoT/action-level precursor), and the AUROC of a fixed LLM monitor for *predicting* violations before the harmful call; and (b) whether **behavioral margin estimation**, Masked Erosion's one-layer-ablation audit (which no longer applies, because there are no layers to ablate), can be replaced by the stress battery. The claim becomes "internalization removes the audit handle for depth", not "logs disappear".
4. **Controls reviewers will demand.**
   - No-rule teacher: the same distillation pipeline with the safety rules stripped from the teacher, to separate internalization from base priors.
   - Base-model-prior arm.
   - Permitted/forbidden paired contexts, plus a legitimate-guarded-action utility metric (PVST avoidance confound).
   - Witness-model validity gate (2610.32322) so that "retained" is not just "the task never needed the rule".
   - 3 seeds and paired bootstrap throughout.
5. **Add a multi-round arm for mentor relevance.** In a HERD-style loop, each round evolves the harness under an acceptance gate and then distills it. Track margin across rounds. Hypothesis: depth erodes *twice*, once at the gate (masking) and once at distillation (unidentifiability), so self-improving loops that internalize lose safety depth monotonically.
   This connects directly to Gödel-Agent-style RSI ("what does a self-improving agent forget about safety when it rewrites itself into its weights?") and to Dan's plasticity line (re-learnability of a dropped layer).
6. **Scope.**
   - Use 3 environments: Masked Erosion's AppWorld credential task, which reuses their released lattice and gives direct comparability; RBAC SQL; and a τ-style permissions task built with the procedural generator from Dan's infra plan. Avoid stock τ²-bench, where strong models mask the rules themselves.
   - Use 2 student sizes (4B, 8B) and 2 families.
   - Drop the Latent Harness reproduction unless time allows. The logit/sample factorization is the scientific core.
7. **Framing and venue.** Pitch it to the NeurIPS main track as "the identifiability of defense-in-depth under internalization", with a small proposition (masked layer ⇒ zero expected SFT gradient; logit-KL signal bounded by teacher divergence) and an empirical predictor. Keep a safety-workshop version as a fallback. Contact or cite Masked Erosion authors early: their AppWorld lattice and released texts make the comparison exact.

## 5. Sharpened headline claim
"Harness-to-weights distillation preserves the safeguards an agent visibly uses but cannot preserve the redundant ones: masked safeguards are unidentifiable from sampled trajectories, so SFT-style distillation (Harness-Zero-style) keeps ≈0 of their margin. Only logit-level self-distillation transmits them, and only in proportion to a teacher-divergence score you can compute before training. Internalization also removes the one-layer-ablation audit that preserved depth in harness space, so each distill round in a self-improving loop silently spends safety margin."

## 6. Minimal pilot (≤1 week, <$100)
Use AppWorld credential-misuse (Masked Erosion texts) or RBAC SQL with Qwen3-4B as teacher+student.
1. Pick 4 lattice states: full stack AVPC, A only, V only, and AV.
2. Distill each with sampled-SFT and with on-policy logit-KL (500 trajectories, LoRA r16).
3. Run the layer-targeted stress for A and V.

Go criterion: measurable retention of a masked layer under logit-KL but not under SFT, or a clean zero under both with the teacher-divergence predictor explaining it.

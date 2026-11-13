# Merged report: NeurIPS 2027 idea search (rounds 1 + 2)

*Student: Dan · Mentor: Xunjian Yin · Topic: self-referential and self-improving agents, meta-agents, world models, benchmarks · Edited 2026-11-12. This front matter combines round 1, round 2 (`research/round2/REPORT.md`, `field_forecast.md`), the ICLR 2027 OpenReview sweep (`research/round2/verify/openreview-iclr27-sweep.md`) and a fresh 3-judge re-score of all 23 live and parked ideas.*

---

## How to read this

- **What the rounds did.** Round 1 swept the 2026 literature, generated about 60 candidates, critiqued them and merged the strongest into one lead paper (panel score 75.8). Round 2 fact-checked round 1 and red-teamed the top proposals with a novelty-killer and a sharpness reviewer. It then rewrote them as v2 proposals and self-rescored them. It also bold-checked seven non-obvious ideas taken from evolutionary biology, economics and control theory, and ran an OpenReview sweep of ICLR 2027 submissions (about 210 queries). Round 2 picked **Meta-Overfitting** as the paper and **Blind-Spot Hiding** as the backup.
- **What the judge panel is.** Three LLM judges re-scored every idea independently after seeing the ICLR 2027 sweep:
  - an **AC**, focused on rigor and novelty against the literature;
  - a **domain expert** in self-improving agents;
  - a **mentor-pragmatist**, focused on fit with Xunjian's agenda and on whether a solo undergrad can finish by May.

  "Panel mean" is the average of the three totals. "Spread" is the highest total minus the lowest. Each judge also gave a verdict: *pursue-with-fixes*, *fold-into-another* (keep it only as a section of another paper) or *park*.
- **Rubric.** Each dimension is scored 1–5: Novelty N, Surprise/falsifiability S, mentor Relevance R, NeurIPS Competitiveness/rigor C, Feasibility F and Timing T. **Total = 20 × (3N + 2.5S + 2R + 2C + 1.5F + 1T) / 12.** A total of 75 or more is strong, 60–75 is viable with fixes, and below 60 means park. Kill flags (any one means reject unless fixed):
  - **K1**: a 2026 paper already makes the same central claim with the same design.
  - **K2**: no outcome would be interesting, so a null result is unpublishable.
  - **K3**: the idea needs closed-model access we cannot afford.
  - **K4**: the contribution is only a combination of two existing things.

  Panel scores run about 5–10 points below round 2's self-rescores, because the self-rescores were optimistic and the judges had the ICLR sweep. Compare ranks, not absolute numbers across rounds.

## TL;DR

- **The panel changes round 2's pick. Recommended paper: *Taught, Not Selected* (evolve-in-dream v2, 73.1).** When reflective harness evolution (GEPA-style) is scored inside a simulator with steerable errors, the real-environment loss comes mainly through the traces the reflector reads, not through the simulated score. The test is a 2×2 of trace source × fitness source at matched base error δ₀, plus a legible-vs-silent error contrast. Both signs are pre-registered.
  - It is the only top idea that the ICLR 2027 sweep left essentially intact. The one partial overlap is VHE (5lWjRTK6Vk), which already cuts what the reflector reads.
  - All three judges said pursue-with-fixes, and none raised a kill flag.
- **Backup: *Meta-Overfitting* (effect-cause-decay v2, 72.1).** Round 2 recommended it. It is close to a tie: 1.0 point behind, which is less than either idea's judge spread (6.2 and 6.7).
  - The judges split. The AC preferred Meta-Overfitting (75.0 vs 69.6). The expert (68.3 vs 73.8) and the mentor (72.9 vs 75.8) preferred Taught, Not Selected.
  - It fell because the ICLR wave now reports the *opposite* sign: RSI-Harness (EOrtreQLbv), MetaEvoForge and FreeEvolve claim positive improver transfer. Improvement by Inheritance (ZMmJDeDTXk) already owns the swap design, and finds that improvers almost never edit themselves.
  - It survives as a *bounded rebuttal*: their transfer is near-domain or carried by memory, and it flips sign far away. It keeps the best mentor fit, and five lower-ranked ideas are meant to fold into it.
- **Round 2's backup, Blind-Spot Hiding, falls to #5 (67.8).** ecw2VvTeE5 and IkYmTAyPXZ already show the weight-side version, that self-written curricula drift toward what the model can already do. What remains new is narrower: the frozen-weight writer, the signed per-rule ρ, and the yoked twin.
- **No idea reaches 75 ("strong") as written.** The top six all need fixes first: cite and position against the ICLR 2027 submissions, add a second model family, and pass a pilot gate.
- **Decide by pilot, not by score.** Run the Taught, Not Selected pilot (≤ $60) first. If its headroom gate fails, and QDdWiaNkk2 reports near-zero real gains for harness evolution so this is a live risk, switch to Meta-Overfitting and its improver-edit-rate pilot (≤ $50).
- **New finding: Parametric Gödel (#4, 68.6, spread 1.3, the most consistent score in the panel).** It is the round-1 weight self-modification idea that round 2 dropped. It is the natural option if Xunjian's group has GPUs and no WeAct sequel planned.
- **Fold, don't spawn.** 12 of 23 ideas got a fold-into-another or park verdict from at least two judges. The main targets:
  - No-privileged-self-access (#3), self-forecast, rsi-elasticity, context-debt, meta-winners-curse and harness-half-life fold into Meta-Overfitting.
  - Gate-dial folds into the drift barrier.
  - Canalization and breaking-the-improver fold into useful-fictions.

## Leaderboard

| # | Idea | Topic | Panel mean | Spread | Judges' verdicts (AC / expert / mentor) | Prior score(s) | One-line verdict |
|---|---|---|---|---|---|---|---|
| 1 | **Taught, Not Selected** (evolve-in-dream v2) | World models / simulator-scored harness evolution | **73.1** | 6.2 | pursue / pursue / pursue | R1 71.3→74.6 · R2 red team 63.8/67.9 · v2 76.3 (#3) | Survived the ICLR sweep. Add an exploit-rate readout, a VHE arm and a natural silent error (0cb6zfwHm7); gate strictly on headroom. |
| 2 | **Meta-Overfitting** (effect-cause-decay v2) | Improver transfer / meta-learning decay | **72.1** | 6.7 | pursue / pursue / pursue | R1 merged 75.8 · R2 red team 67.5/67.9 · v2 77.9 (#1, Recommended) | Reframe as a bounded rebuttal of RSI-Harness and MetaEvoForge. Reproduce their positive near-domain cell; check the improver-edit rate first. |
| 3 | No privileged self-access | Self-knowledge of self-modifying agents | 68.8 | 4.5 | fold (K2) / fold / fold | R2 bold-check 73.3 (#7) | Expected result is null. Fold into #2 as the memory-transplant arm, with Predict-Before-You-Modify (ZBxV5bEYUH) as the positive foil. |
| 4 | Parametric Gödel | Weight self-modification via LoRA modules | 68.6 | 1.3 | pursue (K4) / pursue (K4) / pursue | R1 67.1 (#10); not in R2 | Consistent across judges. Needs GPUs and coordination with the WeAct authors; lead with the decay decomposition. |
| 5 | Blind-Spot Hiding (curriculum-goodhart v2) | Self-written curricula / Goodhart | 67.8 | 4.2 | pursue / pursue / pursue | R1 70.8→75.8 (72.1) · R2 red team 67.1/66.3 · v2 78.3 (Backup) | Concede the weight-side mechanism. Claim only the writer substrate, ρ and the yoke. Add an ActiveSaddler arm. |
| 6 | Drift barrier / error threshold | Gate resolution, editor defect ratchet (pop-gen lens) | 67.2 | 3.0 | pursue / pursue / pursue | R2 bold-check 74.2 (#6) | Still differentiated but crowded (T04ZzT5m6j, EvoLyapunov). Derive L* formally; use TRUST/SGM gates as baselines. |
| 7 | Useful fictions | Belief-state optimism under selection | 64.5 | 3.4 | pursue / fold / pursue | R2 bold-check 74.2 (#5) | Headline taken by 5lWjRTK6Vk. What survives is selection vs drift plus block-swap mediation; run a pilot or fold. |
| 8 | Interference plateau | Edit-level epistasis | 62.1 | 5.9 | pursue / fold / pursue | R2 bold-check 74.6 (#4) | LRSI (71wzcahKcb) and yl1XFtnTn4's null hit it hard. Pitch only the decomposition, with LRSI as a baseline. |
| 9 | Learning-to-goodhart (text vs weights) | Reward hacking across substrates | 61.4 | 3.3 | pursue / pursue / pursue | R1 70.8→74.2 · R2 red team 68/67.5 · v2 ~71 (#9) | The only unclaimed part is the matched ΔTR contrast. Optimizer-strength matching is unsolved. |
| 10 | Harness half-life | Harness gains across model upgrades | 61.1 | 0.5 | fold / fold (K4) / fold (K4) | R1 64.6→72.5 (panel 69.6) | Fold the ξ survival forecast into #2 as a model-upgrade axis. |
| 11 | Canalization | Robustness under environment interventions | 61.1 | 2.9 | fold (K4) ×3 | R1 68.3 (as breaking-the-improver) · R2 65.8 (#11) | Merge into breaking-the-improver as a TOST-powered held-out-family test. |
| 12 | Safeguards internalization | Safety under harness→weights distillation | 60.8 | 3.7 | park (K4) / park (K2, K4) / park | R1 63.8 (parked) | Pilot the D_KL predictor first; better suited to a safety venue. |
| 13 | RSI elasticity (R_AI) | Economics of RSI | 60.4 | 2.9 | fold (K4) / fold (K4) / fold (K4, K2) | R1 64.6 (65, folded into #1) | Secondary estimand inside #2 at most. |
| 14 | Self-forecast | Improver self-modeling (IDM audit) | 60.3 | 3.3 | fold / fold / fold (K2) | R1 66.3 (folded) | Merge with #3 as #2's forecast readout. |
| 15 | Gate dial | Acceptance gate as stability–plasticity dial | 58.7 | 5.0 | fold (K4) ×3 | R1 62.9 (parked) | Becomes the false-reject half of the drift barrier (#6). |
| 16 | Cross-play collusion | Verifier–harness co-evolution | 58.5 | 5.4 | pursue / fold (K4) / pursue (K4) | R2 bold-check 72.9 (#8) | Downgraded from "first" to a two-sided generic vs pair-specific test; hottest lane. |
| 17 | Breaking the improver | Robustness-aware selection | 55.2 | 2.9 | fold (K4) ×3 | R1 68.3 · R2 65.8 | Fold into useful-fictions as its robustness section. |
| 18 | Baldwin hiding/assist | Harness × GRPO weight learning | 54.6 | 3.4 | pursue (K4) / fold (K4) / park (K4) | R2 bold-check ~71 (#10) | Near-scoop by TAME (IXnpSW8g3v) and AutoScaffold. |
| 19 | Teacher substrate | Learner-relative task value | 54.0 | 1.7 | fold (K4) ×3 | R1 62.1 (parked) | Section of learning-to-goodhart at most. |
| 20 | Improver erosion | Cost-aware pruning of self-checks | 53.6 | 5.4 | fold (K1, K4) / fold (K4) / fold (K1, K4) | R1 55.0 (parked) | Possible editable-verifier arm, after a pruning pilot. |
| 21 | Meta winner's curse | Selection noise one level up | 52.8 | 2.1 | fold (K4) / fold (K1, K4) / fold (K1, K4) | R1 60.0 (folded) | Placebo and swap claims taken (da1Czy7foS, ZMmJDeDTXk). |
| 22 | Saturation law | Data multiplier of harness learners | 52.0 | 3.4 | park (K4) / fold (K4) / park (K2, K4) | R1 60.8 (parked) | Only n* ∝ R/k survives; fold into #5 at most. |
| 23 | Context debt | Anchoring vs saturation plateaus | 50.8 | 0.9 | fold (K4) / fold (K1, K4) / fold (K1, K4) | R1 65 (absorbed into lead) | Pre-empted by Branch-reset (8UgphlLDok). Keep only #2's P0+Mk vs Pk+∅ cell. |

## ICLR 2027 OpenReview check

Round 2 could not reach OpenReview, and called that its largest open scoop risk. A later sweep used the public `api2.openreview.net` search endpoint, filtered to ICLR 2027, and ran about 210 queries over six clusters: improver, selection, goodhart, curriculum, world model and evolutionary lens. The panel scored with this sweep in hand, and it accounts for most of the movement from round 2.

**What it changed**
- **One idea scooped outright.** Compute-matched evaluation of harness evolution is taken by QDdWiaNkk2. Drop it as a paper and keep it as a required control everywhere. QDdWiaNkk2 also reports near-zero real gains on Terminal-Bench 2.1, which makes headroom a risk for every harness-evolution idea, #1 included.
- **Meta-Overfitting (#2): not scooped, but it now faces opposite-sign evidence.**
  - RSI-Harness (EOrtreQLbv) claims improvers transfer up to 4.5× to unseen models, benchmarks and domains, as do MetaEvoForge (0b4wysHrzh), FreeEvolve (8UyLPIcKqX), Looped-RSI and Meta-TTL. None controls for memory or grades domain distance.
  - Improvement by Inheritance (ZMmJDeDTXk) and Learning to Repair the Learner (aEPJT4hW84) own the swap and crossing design, so they become measurement lineage. ZMmJDeDTXk also confirms that improvers rarely edit themselves.
  - This dropped it from 77.9 to 72.1.
- **Taught, Not Selected (#1) survives.** No submission scores harness evolution in a learned simulator and splits simulator error into a trace channel and a fitness channel. VHE (5lWjRTK6Vk) partly overlaps on trace hygiene. Improvement Fidelity (Ah9eaA746Q), SEA (4HmKWgvoR5) and Simulator Says Yes (0cb6zfwHm7) give selection-channel accounts, which makes them good foils. This is why it moved to #1.
- **Blind-Spot Hiding (#5): the "why surprising" section needs rewriting.** ecw2VvTeE5 and IkYmTAyPXZ show the weight-side drift. Band censorship and Alt-2 are partly pre-empted (UIvgqCcjE2, TMQXORjxix).
- **Bold ideas fell 7–17 points.**
  - Baldwin: TAME owns hiding and rank reversal.
  - Interference plateau: LRSI's edit toggling, plus yl1XFtnTn4's null on interactions.
  - Useful fictions: 5lWjRTK6Vk owns "false success rises across generations".
  - Cross-play: co-cheating (Id4G8bzzKO), the version × evaluator matrix (tn4eslVXEK), and cross-family judge-exploit transfer (Lpxn7lc6Dz).
  - Context debt: Branch-reset.
  - The drift barrier lost the least (74.2 to 67.2), but is now crowded by T04ZzT5m6j, EvoLyapunov (3t0j2Y05EH), NxGP6PGiaU and Kinetic Theory (23VDi5bfRj).
- **Still open with must-cites added:**
  - the drift barrier's L* law;
  - self-vs-foreign self-knowledge, whose nearest work is ZBxV5bEYUH;
  - learning-to-goodhart's matched ΔTR;
  - the Blind-Spot Hiding ρ statistic and yoked twin;
  - Taught, Not Selected.
- **Coverage caveat.** Several tracked arXiv papers were not found under their names on OpenReview: MetaRSI, RSIFlow, RSI-Opt, Masked Erosion, HERD, Double Ratchet and Biddulph & Carroll. They may be unsubmitted or retitled. Two submissions are both called "LRSI" (71wzcahKcb and V7vjimAYGi), and two are both called "Harness²".

## Where rounds 1 and 2 changed their minds

- **The lead paper got narrower twice, then lost first place.** Round 1 merged three audits into one paper (Effect, Cause, Decay; 75.8). Round 2's red team found three half-papers, so the paper was cut to Decay alone (77.9, Recommended). The panel now puts it at #2 (72.1), because the ICLR positive-transfer wave makes it a rebuttal rather than a first.
- **Evolve-in-dream recovered from its low point.** It scored 74.6 in round 1. Round 2 pre-empted its "type, not rate" headline (DRPE, Dream-RSI) and the red team scored it at 63.8. It then pivoted to "taught vs selected" (76.3), and is now #1 (73.1), because its central manipulation has no ICLR overlap.
- **Curriculum-goodhart rose and then fell.** It went from 72.1 to 78.3 in round 2, when it was recast as one signed claim with a yoked control. It is now 67.8, because the weight-side mechanism is published.
- **Learning-to-goodhart fell steadily** across rounds: 74.2, then 71, then 61.4. The causes were the weight-level twin (2602.12124), C6EElQ2PRG, and an unsolved optimizer-strength matching problem.
- **Bold ideas looked better in round 2 than they do now.** Six of seven bold-checks scored 71–74.6, against low scoop risk forecast in `field_forecast.md`. The ICLR sweep found that most "biology lens" ideas had ICLR neighbours already. Only the drift barrier and the self-knowledge idea are still in the top six.
- **Parked ideas mostly stayed parked.** The exception is Parametric Gödel (R1 67.1 → 68.6, #4), which all three judges independently scored 68–69.
- **Fact corrections round 2 made, which still hold:**
  - SelfImprove-Bench had been misquoted. The like-for-like editable vs fixed improver comparison is 1.326 vs 1.207, and 0% of improver edits are retained unless forced.
  - "Open models can't drive self-modifiers" applies only to the Hyperagents scaffold.
  - MetaRSI is a stronger threat than round 1 said.
- **The design method moved from contribution to lineage.** Swap and crossing designs were a selling point in round 1. By the panel they are prior work (ZMmJDeDTXk, aEPJT4hW84), and the novelty has to sit in a new axis: domain distance, trace channel, or the self-vs-foreign diagonal.

## Where the field is moving

**Trend table (condensed from `field_forecast.md`, updated with the ICLR sweep)**

| Lane | 2026 trajectory | Saturation | Our ideas |
|---|---|---|---|
| "Recursive self-improvement" label | 2.3 → 96 papers/mo (≈40× in 9 months) | Exploding | All ride it |
| Harness-evolution methods | 26 → 161 papers/mo; ICLR replication disputes (QDdWiaNkk2) | Saturated | Avoid methods; expect headroom disputes |
| Improver-level ("evolve the evolver") | MetaRSI, Evo², HMED; ICLR RSI-Harness and MetaEvoForge report positive transfer | Crowding | #2 is a rebuttal: near-domain or memory-carried transfer that flips far away |
| RSI audits and benchmarks | Census, SelfImprove-Bench, RSIBench, Improvement by Inheritance | Saturating | The bare swap test is lineage now |
| Simulator / world-model scoring of agents | October fidelity cluster; DRPE, Dream-RSI at ICLR | Medium | #1 is ahead for harness evolution specifically (forecast 30% chance of being reached by May) |
| Verifier co-evolution / reward-free RSI | Hottest method lane; co-cheating already shown | Very high | Cross-play is late |
| Reward hacking in loops | Re-accelerating; C6EElQ2PRG, 5lWjRTK6Vk | Medium-high | Learning-to-goodhart and useful fictions are crowded |
| Self-generated curricula | Flat volume; ecw2VvTeE5 and IkYmTAyPXZ audits | Saturated for methods | #5 is a narrow audit |
| Harness ↔ weights co-evolution | 12 or more ICLR method submissions; TAME | Saturated | Baldwin near-scooped; Parametric Gödel must lead with measurement |
| Agent plasticity | Doubling each quarter; "agent plasticity" is now a claimed term (Jjh60xpFVY) | Just starting | Meta-level angle only |
| Bio and economics lenses | Mostly metaphor; theory papers (T04ZzT5m6j, Kinetic Theory) take pieces | Starting | Drift barrier still differentiated |

**Predicted by May 2027** (judgment, not calibrated):
- a powered improver × state swap paper (75%; partly realized by ZMmJDeDTXk);
- an RSI workshop or benchmark cluster (85%);
- agent plasticity as a named subfield (70%);
- verifier collusion and drift critiques (65%; already starting);
- causal simulator-fidelity studies in harness evolution (30%), the window #1 depends on;
- harness-gain replication disputes (70%; QDdWiaNkk2 is the first).

**Non-obvious ideas with low or moderate scoop risk (risk updated after the ICLR sweep)**

| Idea | Lens | Scoop risk now | Note |
|---|---|---|---|
| Drift barrier / error threshold (#6) | Muller's ratchet under a noisy gate | **Medium** (was low) | The quantitative L*(n, ε) law and "self-referential gives a lower L*" are unclaimed. Gate fixes become baselines. |
| Self-knowledge diagonal (#3) | Social-relations model | **Low** | Open. Test whether ZBxV5bEYUH's forecasting survives a foreign-lineage control. Cheap as an arm of #2. |
| Introspective drift (add-on to #2) | Self-directed edits | **Low** | No hit found. Costs almost nothing on #2's runs. |
| Interference plateau (#8) | Epistasis | **Medium-high** (was low-medium) | LRSI and yl1XFtnTn4. Only the decomposition is left. |
| Useful fictions (#7) | Adaptive vs neutral drift | **Medium-high** | 5lWjRTK6Vk owns the headline. |
| Baldwin hiding/assist (#18) | Baldwin effect | **High** (was low-medium) | TAME and AutoScaffold. |
| Home-model penalty (add-on) | Co-evolutionary lock-in | **Medium** | zn5R6mJV8i pre-empts it as a finding; still fine as a control. |
| Feedback-bits ceiling, Goodhart decomposition, control-theoretic oscillation | Information theory, Manheim–Garrabrant, control | **Low (not yet checked)** | Forecast Q5, Q9 and Q12; never scoop-checked against ICLR. |

## Next two weeks

**Plan (Nov 13–27): run the cheaper, decisive pilot for each top idea, then commit.**

**Week 1 (Nov 13–20): Taught, Not Selected pilot (≤ $60)**
- **Setup:**
  - Existing τ²-bench retail tasks made single-instruction: about 60 evolution/validation tasks and 100 held-out.
  - Qwen3.5-9B agent, GEPA with a Haiku-class reflector, depth 20.
  - Arms: RR ×3 seeds (to estimate σ_seed), SS-legible, SS-silent, SR and Q5.
- **Days 1–2:** build the quirk layer and the trace/fitness router. Verify δ₀ matching (±0.02) on 200 base rollouts.
- **Panel must-fixes to build in now:**
  - log per-quirk exploit rate (quirk firings and quirk-dependent actions on real held-out tasks) as the direct "taught" readout;
  - refresh evaluator state between sim and real cells (GX4ucmiKZa);
  - prepare a silent arm based on the whitewashed-refusal error from 0cb6zfwHm7;
  - stub a VHE-style trace-construction baseline.

**Go / pivot / kill gates (top pick)**

| Signal | Go | Pivot | Kill |
|---|---|---|---|
| Headroom G_RR(20) | ≥ 8 pp with σ_seed ≤ 4 pp | 4–8 pp: weaker seed harness, harder tasks, or depth 60 | < 4 pp after one fix → switch to Meta-Overfitting |
| Loss L(20) = G_RR − G_SS-legible | ≥ 8 pp | 3–8 pp: raise δ₀ to 0.25 or widen eligibility | ≈ 0 **and** Q5 ≈ 0 → stronger reflector or code evolver E2, once |
| Positive control Q5 | Real gain falls ≥ 8 pp vs RR | — | No decline → fix the Q5 design before anything else |
| Exploit-rate readout | Rises in SS/RS cells | Flat: raise quirk eligibility | A positive gain contrast with no exploit rise does not count as "taught" (pre-registered) |
| δ₀ matching | Within ±0.02 | ±0.05: retune | Markers shift base behaviour > 0.1 → move legibility into evaluator feedback text |

**Week 2 (Nov 20–27): Meta-Overfitting edit-rate pilot (≤ $50), as the backup and the fallback if #1 fails**
- Run 2 Gödel-lite lineages to 6 accepted improver edits, then probe on ALFWorld and Reasoning Gym.
- **GO** if all of these hold:
  - at least 1 accepted improver edit per 4 iterations, which is the ZMmJDeDTXk risk;
  - home gain of Pk ≥ 3 pp in both lineages;
  - leak judge ≤ 40%;
  - ≤ $2.5 per probe.
- **PIVOT** once: force improver-module edits, or use a DeepSeek V4 Flash improver, with fallback k* ≈ 6.
- Add a reproduced RSI-Harness-style positive near-domain cell with memory left in.
- **Decision meeting with Xunjian on Nov 27:** which idea leads, and which survives as a section of the other.

**Reading list (in order)**
- *For #1:*
  - 5lWjRTK6Vk (Harness Learns to Lie / VHE)
  - 0cb6zfwHm7 (Simulator Says Yes)
  - Ah9eaA746Q (Improvement Fidelity)
  - 4HmKWgvoR5 (SEA)
  - FU8IgfEoHI (PairEvolve)
  - GX4ucmiKZa (evaluator-state confound)
  - C6EElQ2PRG (Feedback Alignment)
  - QDdWiaNkk2 (Rethinking the Evaluation of Harness Evolution)
  - 2609.32452 and 2610.14662 (likely fast-followers)
- *For #2:*
  - EOrtreQLbv (RSI-Harness)
  - ZMmJDeDTXk (Improvement by Inheritance)
  - aEPJT4hW84 (Learning to Repair the Learner)
  - 0b4wysHrzh (MetaEvoForge)
  - Q9AHmuAk4Y (RSIBench; a candidate far domain with the same interface)
  - ZBxV5bEYUH (Predict Before You Modify; for the folded self-knowledge arm)

**Scoop-watch (every Monday)**
- **ICLR 2027 OpenReview revisions and rebuttals** (rebuttal tables make new arms cheap for authors) on 5lWjRTK6Vk, EOrtreQLbv, ZMmJDeDTXk, Ah9eaA746Q and 0cb6zfwHm7.
- **arXiv/S2 forward citations of:** 2609.32452, 2610.14662, 2610.08033, 2611.01139, DRPE 2609.32322, Dream-RSI 2609.14858, plus round 2's list (MetaRSI, VERSE, RSI-Opt, Organon, FreeEvolve, HMED).
- **Queries:**
  - "simulator-scored prompt/harness optimization"
  - "reflector trace exploitation"
  - "harness evolution learned simulator sim-to-real"
  - "improver transfer cross-domain memory"
- **NeurIPS 2026 workshop accepted lists** after Dec 4.
- **Preprint target:** February 2027, not March, for whichever paper leads. The panel puts reflector-evidence fast-followers at about 20–25%.

## Questions for Xunjian

1. The panel narrowly prefers **Taught, Not Selected** over round 2's **Meta-Overfitting**, with one AC dissent. Which excites him more, and is it acceptable to let the Week 1–2 pilots decide?
2. **Compute.** Can the lab self-host Qwen3.5-9B (about 200 H100-hours for #1; otherwise about $600 hosted)? Can it serve a ~30B improver for #2? Is a GPU node confirmed before the Dec 20 OSF freeze?
3. Can **BreakingWeb's intervention code** serve as the quirk layer for #1, so that "simulator error" is a BreakingWeb-style intervention on the fitness source?
4. In his experience with **Gödel Agent**, how often does the agent edit its own improver rather than prompts or memory? ZMmJDeDTXk and SelfImprove-Bench say almost never. Would forced improver edits undermine #2's claim?
5. Does the **WeAct group plan a multi-round weight self-modification sequel**? If not, Parametric Gödel (#4) is a strong second project. If so, should Dan join it?
6. Should the self-knowledge / memory-transplant arm (#3) and the model-upgrade ξ forecast fold into Meta-Overfitting? Is the resulting paper still one coherent paper?
7. Is a **February 2027 preprint** acceptable before NeurIPS submission?
8. Which **second model family** is realistic for the panel's single-model objection: a second agent model for #1, or a second improver for #2?
9. Does he know of ICLR 2027 submissions, or group plans, on simulator-scored harness evolution or improver transfer beyond those listed above?


---

# Per-topic detail: closest papers, judge panel, feedback


## Improver meta-learning, transfer & decay (do self-referential improvers get better at improving?)

### Landscape

From mid-2026 the field stopped asking whether self-improvement loops raise the product (the harness, skills or code) and started asking whether they raise the *improver* itself. Several papers now run some version of a same-start swap: MetaRSI §5.4, SelfSearch Table 4, FreeEvolve, HMED and VERSE App. G.2. Two ICLR 2027 submissions already own the measurement method. Improvement by Inheritance (ZMmJDeDTXk) does nested replay of improver × state. Learning to Repair the Learner (aEPJT4hW84) crosses code with history and finds inherited procedure explains 45% of the gain. Both report that improvers almost never edit themselves. In parallel, a wave of ICLR 2027 submissions reports that evolved improvers *transfer* across domains: RSI-Harness (up to 4.5x), MetaEvoForge, FreeEvolve, Looped-RSI and Meta-TTL. None of them wipes memory, scrubs domain facts from the procedure, or grades domain distance. That leaves one open gap: is the "better improver" a domain-general procedure, or is it home-domain memory and specialization that turns negative far from home? Our lead idea, Meta-Overfitting, targets exactly this gap. The other ideas in this topic look at the same question from other angles: self-knowledge, weight-level self-modification, model upgrades, an elasticity estimand, forecast skill, robustness, check erosion and data saturation. For most of them the measurement is now taken, so they work better as arms of the lead paper than as standalone papers.

### Closest papers

All arXiv entries were verified in earlier rounds unless marked †. Entries marked † were cited by a judge or by feedback but not verified. ICLR 2027 entries ("ICLR27 sub") were taken from the Nov 12 OpenReview sweep, which read abstracts only. Idea abbreviations: ECD = effect-cause-decay (Meta-Overfitting), NPSA = sharp-no-privileged-self-access, PG = parametric-godel, HHL = harness-half-life, RSIE = rsi-elasticity, SF = self-forecast, BTI = breaking-the-improver, IE = improver-erosion, SL = saturation-law.

| Paper | When | Summary | Which ideas it threatens / how |
|---|---|---|---|
| [Gödel Agent 2410.04444](https://arxiv.org/abs/2410.04444) | Oct 2024 (ACL 2025) | Xunjian's self-referential agent: it modifies its own logic and code, guided only by high-level objectives. | Foundation for every idea here. ECD tests whether its self-edits are meta-learning or meta-specialization. |
| [RSI-Harness](https://openreview.net/forum?id=EOrtreQLbv) (ICLR27 sub) | Nov 2026 | Dual loop: the inner loop refines the harness and the outer loop revises the improver, gated on the same start harness. The improver's gain grows from 1.3 to 6.2 pts and transfers to unseen models, benchmarks and domains (up to 4.5x); the harness transfers poorly. | **ECD, high.** Same question, opposite sign. No memory wipe, no scrub, no distance axis. ECD must reproduce its positive near cell. |
| [MetaEvoForge](https://openreview.net/forum?id=0b4wysHrzh) (ICLR27 sub) | Nov 2026 | An independent MetaAgent evolves an improvement policy from 768 improvement pairs across 62 environments. Evolved vs unevolved improvers from the same root show task-dependent held-out gains. | ECD (positive-transfer foil); RSIE (same-root improver test). |
| [Looped-RSI](https://openreview.net/forum?id=sBH9cdajAw) (ICLR27 sub) | Nov 2026 | Branching RSI with the improver consolidated separately, by descendant quality. Its improvers "transfer effectively to unseen environments". | ECD: another positive-transfer claim to bound. |
| [Meta-TTL](https://openreview.net/forum?id=nw2tokNgb2) (ICLR27 sub) | Nov 2026 | Bi-level meta-learning of a natural-language self-improvement policy (Jericho, WebArena-Lite, τ²). Up to +27% OOD. | ECD (medium): adds to the "improver procedures transfer" prior, though it is not self-referential. |
| [RSIBench](https://openreview.net/forum?id=Q9AHmuAk4Y) (ICLR27 sub) | Nov 2026 | Agents revise their own harness, scored on held-out Terminal/EnvLearning/SWE/Science tasks. GPT-5.5 gains on train but loses 8.3 pp held-out. | ECD: supports the sign flip at harness level. Judges suggest it as an agentic far domain that keeps the interaction format. |
| [Improvement by Inheritance](https://openreview.net/forum?id=ZMmJDeDTXk) (ICLR27 sub) | Nov 2026 | Separates child quality from successor quality, using nested replay over common start states. On Hyperagents history, child quality goes 0.48→0.86 but successor quality does not move, because the improver almost never edits itself. | **Method scoop** for ECD's crossing, RSIE's grid and NPSA. Makes the improver-edit-rate risk (K2) empirical for ECD, RSIE and SF. |
| [Learning to Repair the Learner](https://openreview.net/forum?id=aEPJT4hW84) (ICLR27 sub) | Nov 2026 | Matched interventions on neuro-symbolic world models: crossed model/procedure states, a code × history crossing, and a generator lock. Inherited procedure explains 45% of descendant gain. | ECD's Pk × Mk arms and NPSA/RSIE's procedure-vs-memory split are prior design. Must be cited as lineage. |
| [Learning to Improve / TRACE](https://openreview.net/forum?id=V7vjimAYGi) (ICLR27 sub) | Nov 2026 | Argues task gains ≠ improvement-ability gains. Lets the agent edit its own diagnosis, repair and self-modification programs, and points to "cross-task migration" of improvement ability. | ECD (medium): names the transfer question. Name clash with LRSI (71wzcahKcb). |
| [VERSE 2610.02616](https://arxiv.org/abs/2610.02616) / [ICLR27](https://openreview.net/forum?id=9WwpxPfcji) | Oct 2026 | The optimizer self-evolves its prompts, skills, tools and notes with weights frozen; it works only with execution-based verification. App. G.2 moves an evolved optimizer SWE→TB and it helps. | **ECD, high**: VERSE is the "motivating anomaly", with notes lumped in with tools. IE: its verification tools are frozen, which is IE's gap. |
| [SelfSearch 2609.37968](https://arxiv.org/abs/2609.37968) | Sep 2026 | Reward-free search over the agent's own instructions and tools, using episode records. Table 4: "w/o records" vs "fixed improver" at gen 10, with 1.7–2.9 pp gaps that sit inside noise. | ECD (Cause arm, high). A SelfSearch v2 is a scoop vector. |
| [RSI-Opt 2610.32026](https://arxiv.org/abs/2610.32026) | Oct 2026 | A meta-agent evolves an optimizer-agent policy over 5 generations. Held-out transfer peaks at gen 1 while the train suite keeps rising. | ECD (high): the closest precedent for the *shape* of meta-overfitting, but at product level with a fixed meta-procedure. |
| [FreeEvolve 2610.09197](https://arxiv.org/abs/2610.09197) / [ICLR27](https://openreview.net/forum?id=8UyLPIcKqX) | Oct 2026 | Meta-evolves an "evolution skill" scored on fresh target agents; +6.9 held-out. A Haiku evolver overfits (evolution set 35→52, held-out 33.3→32.2). | ECD: an in-domain Effect result plus one-point task-set overfitting; claims skills transfer across environments. |
| [MetaRSI 2609.06396](https://arxiv.org/abs/2609.06396) | Sep 2026 | Meta-recursive loop on an open Qwen3.5-35B-A3B. §5.4 same-start swap gives +7.2 vs +4.9; a 5-term curve gives +26.2 vs +20.7, with decaying per-term gain. | ECD (owns Effect, now a gate). RSIE (shows g > 0 in-domain, with diagonal decay). |
| [HMED 2610.07979](https://arxiv.org/abs/2610.07979) / [ICLR27](https://openreview.net/forum?id=AnN0NpAVWL) | Oct 2026 | Re-executes incumbent vs revised meta-skills from a restored state. Branch outcome and matched Δ disagree in sign for 45–75% of revisions. | ECD: its frozen-meta-skill test is ECD's Pk+∅ cell in-domain, and its winner's-curse evidence is why ECD pre-registers the incumbent improver. Uses the same ALFWorld/ScienceWorld domains. |
| [Lineage Is Memory 2610.41157](https://arxiv.org/abs/2610.41157) | Oct 2026 | 1,750 pre-registered runs. Parent inheritance drives most progress; a "carry facts" instruction moves banking77 from 0.37 to 0.86. | ECD (basis for the κ fact-scrub and the rigor bar); SL (evidence shown to the proposer). |
| [Agent Plasticity 2610.08902](https://arxiv.org/abs/2610.08902) / [ICLR27](https://openreview.net/forum?id=Jjh60xpFVY) | Oct 2026 | Plasticity defined as held-out gain per unit learning cost for frozen-weight agents. Gains carry over only partly OOD. | ECD (meta-level is listed as future work, so a scoop vector). The term "plasticity" is now taken (relevant to PG and HHL naming). |
| [Auditing the Product, Not the Recursion 2610.27610](https://arxiv.org/abs/2610.27610) | Oct 2026 | Codes 114 RSI studies: only 4 of 26 "better procedure" claims run a same-start matched swap. Names a C5 compounding swap test but runs none. | ECD, RSIE, SF: motivation and scoop vector (forecast: 75% chance of a powered swap paper by May 2027). |
| [Scaling Discovery Requires Forgetting 2610.20563](https://arxiv.org/abs/2610.20563) | Oct 2026 | Agents plateau by anchoring on their own history; periodic resets restore progress. | ECD: its reset is the Pk+∅ cell. The "reset procedure, keep memory" design rule contradicts its practice. |
| [Branch-reset](https://openreview.net/forum?id=8UgphlLDok) (ICLR27 sub) | Nov 2026 | Fresh chat context with the workspace inherited escapes idea basins in autoresearch agents. | ECD's secondary design rule (context-debt contrast folded into ECD). |
| [Opinionated Optimizers](https://openreview.net/forum?id=LhhLpXBFgo) (ICLR27 sub) | Nov 2026 | 216 meta-optimization runs. Proposer priors narrow the strategies, noisy evaluations give weak evidence, and selection rarely compounds. | ECD (task-level meta-overfitting, a different object); SL (a competing plateau explanation). |
| [RRSI](https://openreview.net/forum?id=75PPLZgaUM) / [ModularRSI](https://openreview.net/forum?id=pP6df7PZI7) (ICLR27 subs) | Nov 2026 | Harness RSI memorizes training tasks and ID gains vanish OOD. Fixes: regularized edits (RRSI) and modular deficiency localization (ModularRSI). | ECD: same framing at harness level; possible mitigation baselines. |
| [Do Self-Evolving Skills Generalize? 2609.39148](https://arxiv.org/abs/2609.39148) / [ICLR27](https://openreview.net/forum?id=rM2eQio3e3) | Sep 2026 | Of 21 skills that help on train, 5 keep the full gain on test and 3 keep none. Poor transfer comes from copying task details. | ECD (memory carries the overfit, which the wipe must separate); SL ("gains are mostly seen"). |
| [Predict Before You Modify (IDM) 2610.13991](https://arxiv.org/abs/2610.13991) / [ICLR27](https://openreview.net/forum?id=ZBxV5bEYUH) | Sep 2026 | Adds an editable Improvement Dynamics Model to HyperAgents that forecasts each edit's outcome. +46% max; forecast accuracy improves and transfers to a new domain. | **SF scoop** (opposite headline). NPSA: a positive self-prediction foil with no foreign control. |
| [Strangers to Themselves 2609.09899](https://arxiv.org/abs/2609.09899) | Sep 2026 | Self-report barely predicts behavior (r = 0.04). Outsiders and other models predict a model at least as well as it predicts itself. | NPSA, SF: the model-level null is already known, which lowers Surprise. |
| [Masked by Consensus 2604.12373](https://arxiv.org/abs/2604.12373) | Apr 2026 | Self- vs external probes tie on full sets (75–80% agreement); a self premium of about 5% AUC appears only on disagreement subsets, for factual QA. | NPSA: methodological must-cite; the reason for disagreement-subset AUROC. |
| [Evaluating and Improving LLM Self-Modeling 2608.30980](https://arxiv.org/abs/2608.30980) | Aug 2026 | Benchmark of verifiable self-modeling; skill is limited. RL gains do not look like privileged access. | NPSA, SF: another model-level null. |
| [Repairs for Whom? 2610.38610](https://arxiv.org/abs/2610.38610) | Oct 2026 | Choosing repairs for failed agents: the deployed agent's own rollouts beat stronger-model rollouts and log-reading judges. | NPSA: "self" helps through execution, not reading. Sets the repair-arm design. |
| [Disentangling Task Difficulty 2610.05572](https://arxiv.org/abs/2610.05572) | Oct 2026 | High AUROC for agent failure prediction mostly reflects task difficulty when runs are pooled across tasks. | NPSA: kills pooled AUROC as a metric. |
| [Does Failure Feedback Direct Self-Modification? 2610.14662](https://arxiv.org/abs/2610.14662) | Oct 2026 | Across 145 DGM modifications there is no target-specific repair advantage; target repair ≠ held-out gain. | NPSA, SF: "selection, not self-knowledge". Must be read in full before ECD's OSF freeze. |
| [Identifying Introspection From the Inside 2610.07186](https://arxiv.org/abs/2610.07186) | Oct 2026 | LoRA-induced latent preferences can be self-reported without supervision. | NPSA (low): weight-level evidence that some self-access exists. |
| [WeAct 2610.26847](https://arxiv.org/abs/2610.26847) | Oct 2026 | Mentor's paper: capability LoRAs over a frozen Qwen3-32B, with adapter selection as an agent action. One insert and one refresh. | **PG, high**: the direct parent. "Modular retains" is true by construction (Fig. 3). |
| [Third Eye 2611.12988](https://arxiv.org/abs/2611.12988) | Nov 2026 | 64 runs on 3–8B models: continuation lowers the utility of an immediately selected self-update by 1.78 pp; losses are on target/OOD tasks. | PG (high): supplies the protocol and the decay to decompose. A "remedy" follow-up is a scoop vector. |
| [Macaron-V1 2608.09819](https://arxiv.org/abs/2608.09819) | Aug 2026 | Production Mixture-of-LoRA plus RSI loop; compounding is left open. | PG (medium): system-level instance; Mind Lab is a likely scooper. |
| [Are Self-Evolving Agents Continual Learners? 2610.25858](https://arxiv.org/abs/2610.25858) | Oct 2026 | GEPA/Meta-Harness/ReasoningBank on drifting streams; plasticity and stability trade off (r = −0.72). | PG (stream and P/S/I/F metrics); ECD (owns the "stability–plasticity" framing ECD should drop). |
| [CoEvo-RSI 2610.25078](https://arxiv.org/abs/2610.25078) / [ICLR27](https://openreview.net/forum?id=7UXKgr1Akp) | Oct 2026 | Co-evolves harness and full weights over about 10 rounds with an archive. | PG: crowds harness+weights co-evolution (the "saturated lane", with [Co-Harness](https://openreview.net/forum?id=53Ak0jvesg) and others). |
| [SLAaaT 2608.17034](https://arxiv.org/abs/2608.17034) | Aug 2026 | Agent tool for switching LoRA adapters mid-trajectory; static library. | PG (low): limits "adapter switching" novelty. |
| [Control Map 2610.36109](https://arxiv.org/abs/2610.36109) | Oct 2026 | Separates acquisition, retention, locality and new-setting performance in test-time updates. | PG: methodological foundation. |
| [What Actually Evolves in Self-Evolving RL](https://openreview.net/forum?id=ecw2VvTeE5) (ICLR27 sub) | Nov 2026 | Self-evolving RL gains mostly sharpen items the model already solves; a later proposer teaches no more than an earlier one. | PG: a competing explanation for refresh decay. SL: the generated-task arm is an expected null. |
| [Capability Loss in Embodied RSI](https://openreview.net/forum?id=IkYmTAyPXZ) (ICLR27 sub) | Nov 2026 | RSI silently erodes tail and error-recovery modes while nominal success rises. | PG (adjacent framing). |
| [When Self-Reinforcement Ends Self-Improvement](https://openreview.net/forum?id=T04ZzT5m6j) (ICLR27 sub) | Nov 2026 | Proves that deterministic judge errors make every fixed acceptance rule drift negative eventually. | PG (negative-drift theory); IE (gate theory). |
| [LRSI: Lookahead-Guided RSI](https://openreview.net/forum?id=71wzcahKcb) (ICLR27 sub) | Nov 2026 | Edits that help now can hurt after later edits; keeps only edits that stay non-negative under on/off toggling. | PG (continuation interference at harness level); HHL (toggles edits across future versions). |
| [Harness Evolution Hits a Ceiling 2610.11655](https://arxiv.org/abs/2610.11655) / [ICLR27](https://openreview.net/forum?id=FPaMYO47j4) | Oct 2026 | Harness fixes process failures, weights fix content failures; LoRA internalizes evolved-harness gains. | HHL (owns the sees/writes rule); PG (LoRA internalization arms); SL (a competing ceiling explanation). |
| [OCDS 2608.10694](https://arxiv.org/abs/2608.10694) | Aug 2026 | GEPA prompts evolved on a cheap tier and deployed upward match same-tier optimization at 5.6–54x lower cost. | HHL (high): upward transfer is positive, against HHL's predicted direction. |
| [UpgradeBench 2608.20918](https://arxiv.org/abs/2608.20918) | Aug 2026 | Retain/port/refresh/retrain decisions for LoRA adapters across Qwen releases; a CKA probe predicts portability. | HHL (high): same decision for weights; lists harnesses as future work. |
| [Elicitation or Expansion? 2610.27889](https://arxiv.org/abs/2610.27889) | Oct 2026 | pass@k analysis of harness gains (reliability vs reach). | HHL (high): supplies HHL's predictor; its authors could fast-follow. |
| [What Survives the Next Model? 2609.00468](https://arxiv.org/abs/2609.00468) | Aug 2026 | For 37–63% of SE technique papers, a single prompt on a newer model beats the engineered tool. | HHL: owns the "half-life" framing. |
| [Aging of Prompt Engineering 2608.24641](https://arxiv.org/abs/2608.24641) | Aug 2026 | Prompting techniques age in a family-specific way; Qwen keeps benefiting. | HHL: the Qwen ladder may not absorb techniques at all. |
| [Harness-Delta Attribution 2610.22466](https://arxiv.org/abs/2610.22466) / [ICLR27](https://openreview.net/forum?id=PX4Zpvsaca) | Oct 2026 | Overfitting and TTS explain 54% and 14% of harness-evolution gains. | HHL (control; source gains may be ~0); BTI (clean validation is a poor selector). |
| [Your Policy Is Secretly a Harness 2610.37255](https://arxiv.org/abs/2610.37255) | Oct 2026 | Return = harness ceiling − policy mismatch, under KL control. | HHL: theoretical foundation for the forecast. |
| [Plug, Play, or Re-Evaluate](https://openreview.net/forum?id=XoMy9wRqF1) (ICLR27 sub) | Nov 2026 | 84.3% of 1,120 harness-component removals change nothing; effects are model-specific. | HHL: static cross-model ablation. ξ must beat it as a predictor. |
| [Evolving to Match Capabilities](https://openreview.net/forum?id=zn5R6mJV8i) (ICLR27 sub) | Nov 2026 | Skills that help a strong model (+31.1) hurt a weak one (−8.8). | HHL, ECD: pre-empts the "home-model penalty" add-on as a finding. |
| [One Policy, Many Harnesses](https://openreview.net/forum?id=4KSNZln9nS) (ICLR27 sub) | Nov 2026 | RL gains are mostly harness-specific adaptation, and transfer depends on direction. | HHL (neighbor). |
| [Rethinking the Evaluation of Harness Evolution](https://openreview.net/forum?id=QDdWiaNkk2) (ICLR27 sub) | Nov 2026 | Harness evolution vs test-time-scaling at matched budget, with held-out tests; mixed wins. | HHL (held-out source gains may be ~0). |
| [Recursive Criticality 2609.00137](https://arxiv.org/abs/2609.00137) | Aug 2026 | Theory of R_AI = recursive feedback / hardening, in log-cost units. | RSIE: supplies the estimand (foundation). |
| [Economics of RSI 2609.15802](https://arxiv.org/abs/2609.15802) | Sep 2026 | Graph models of elasticities with a narrow/broad split, plus a wish list of measurable objects. | RSIE: origin and motivation. |
| [Audit the Scaffold, Not the Checkpoint 2609.34924](https://arxiv.org/abs/2609.34924) | Sep 2026 | Strict diminishing returns whenever the reachable edit set is fixed. | RSIE: makes R_AI < 1 the predicted (boring) outcome. |
| [AIDE² 2609.26457](https://arxiv.org/abs/2609.26457) | Sep 2026 | RSI on a research agent's own code; 7 improvements generalize. Its ignition test is inconclusive at 3 seeds. | RSIE (swap framing). |
| [Efficiency vs Capability 2610.33447](https://arxiv.org/abs/2610.33447) / [Singularity Has a Distribution 2610.18445](https://arxiv.org/abs/2610.18445) | Oct 2026 | Macro intelligence-explosion models; the second gives a detection law for sample size. | RSIE: motivation and power warning. |
| [Belief-Calibrated Optimization 2609.01861](https://arxiv.org/abs/2609.01861) | Sep 2026 | Explicit in-context world-model document of how edits act; content placebo (None < Scrambled < Intact) at n = 4. | SF (high): placebo already run, offline. |
| [LMs as AI Research World Models 2610.12235](https://arxiv.org/abs/2610.12235) | Oct 2026 | Experience records improve prediction of unseen interventions (Spearman +0.27). | SF (high): a rising forecast curve can be ordinary in-context surrogate learning. |
| [Can LLMs predict ML experiment outcomes? 2610.22599](https://arxiv.org/abs/2610.22599) | Oct 2026 | Best R² = 0.49 on 288 ideas; no self-preference. | SF: static version; points to a self ≈ outsider null. |
| [IPI self-evolution 2610.39122](https://arxiv.org/abs/2610.39122) | Oct 2026 | Self-evolution raises indirect prompt injection attack success by 14–49 pp; a contrastive fix restores robustness. | BTI (high): directional claim pre-empted. |
| [Evolving Together, Failing Apart (CODA) 2610.20845](https://arxiv.org/abs/2610.20845) / [ICLR27](https://openreview.net/forum?id=ZIBdUe2hGK) | Oct 2026 | Co-evolved policies grow more sensitive to skill-bank drift over training. | BTI (high): owns "selection manufactures fragility" with a dose-response. |
| [Masked Erosion 2610.38392](https://arxiv.org/abs/2610.38392) | Oct 2026 | 411 loops, 10 gates: success-scored gates silently delete redundant safeguards; margin-aware audits preserve depth. | **IE scoop** (mechanism, gates, fix); BTI (H+ mechanism). |
| [Beneath the Surface 2611.07624](https://arxiv.org/abs/2611.07624) | Nov 2026 | EvoAudit of 4 evolvers: gains cost more to deploy, transfer falls, evolvers insert verification reminders. | BTI (direction risk); ECD (still listed as unread). |
| [SkillShift-Bench 2610.33528](https://arxiv.org/abs/2610.33528) | Oct 2026 | Learned web skills degrade under controlled shifts. | BTI: a ready-made second shift domain. |
| [BreakingWeb 2609.35814](https://arxiv.org/abs/2609.35814) | Sep 2026 | Mentor's benchmark: 519 paired clean/intervened browser tasks. Agents lose 22.9 pp vs 10 pp for humans; 75% of failures are belief failures. | BTI: core instrument (foundation). |
| [On the Fragility of Self-Improving Agents 2608.18066](https://arxiv.org/abs/2608.18066) | Aug 2026 | Self-improvement loops amplify evaluation noise and depend on task order. | BTI: power foundation. |
| [The Harness Learns to Lie While Evolving](https://openreview.net/forum?id=5lWjRTK6Vk) (ICLR27 sub) | Nov 2026 | EvoTrap-Bench uses impossible tasks; false success rises each round of harness self-evolution. Its VHE fix rewards from execution traces. | BTI (takes the "false done" mechanism); IE (takes the self-concealment headline). |
| [Bad Genius / CHASE](https://openreview.net/forum?id=uzzum63KP4) (ICLR27 sub, = 2609.18366) | Nov 2026 | A challenger finds protocol transformations that destroy evolved gains and feeds them back as constraints during evolution. | BTI: close to the gate-exposure arm; must be a baseline. |
| [WebVariants](https://openreview.net/forum?id=TYSv9rIhiQ) (ICLR27 sub) | Nov 2026 | 9 sites × 5 equivalent implementations; up to 50% of agent rankings reverse. | BTI: a ready-made intervention suite in place of the unbuilt τ-world. |
| [Can Harness Optimization Improve Agents?](https://openreview.net/forum?id=Ri0rU3rPOS) (ICLR27 sub) | Nov 2026 | Only 2 of 8 harness-optimization results beat the seed reliably; most gains repair environment failures. | BTI (clean validation misleads). |
| [Feedback Alignment in Self-Improving Agents](https://openreview.net/forum?id=C6EElQ2PRG) (ICLR27 sub) | Nov 2026 | Scaffold evolution raises the visible score V while lowering the held-out target T, through evaluator-specific rules. | IE: takes the visible-vs-held-out divergence. |
| [False Frontiers](https://openreview.net/forum?id=Id4G8bzzKO) (ICLR27 sub) | Nov 2026 | Proposer–solver co-cheating grows over rounds; out-of-fold feedback fixes it. | IE (in-loop vs external divergence). |
| [Auditing Harness Evolution](https://openreview.net/forum?id=yl1XFtnTn4) (ICLR27 sub) | Nov 2026 | An evolved lineage beats a single audit-selected edit by nothing at matched budget; one accepted guard never fires in 3,960 checks. | IE: evidence against deletion pressure. |
| [Kinetic Theory of the Gated Self-Evolving Agent](https://openreview.net/forum?id=23VDi5bfRj) (ICLR27 sub) | Nov 2026 | Master-equation theory with a "resolution law" and a separation theorem: the evaluator must stay outside the population. | IE: "the audit fails unless external" is already a theorem. |
| [A Theory of Reliable Self-Evolution](https://openreview.net/forum?id=NxGP6PGiaU) (ICLR27 sub) | Nov 2026 | Proves the harness-evolution ceiling is set by verification/evaluation cost. | IE (ceiling vs verification cost); SL (competing plateau cause). |
| [TRUST 2611.10787](https://arxiv.org/abs/2611.10787) / [ICLR27](https://openreview.net/forum?id=wV4bK1dHpC) | Nov 2026 | Online multiple testing cuts inferior accepted updates from 45.6% to 2.8%. | IE: gate baseline. Its guarantees assume an unbiased gate. |
| [The RSI Tax 2611.13524](https://arxiv.org/abs/2611.13524) | Nov 2026 | Verification is 75–94% of RSI token cost; a bounded-note baseline matches at 66–71% fewer tokens. | IE: the mis-cited premise, reused as cost pressure. |
| [Auditing Harness Tampering 2609.00069](https://arxiv.org/abs/2609.00069) | Aug 2026 | Taxonomy and audit of harness tampering; tampering persists in the best DGM/Hyperagents lineages. | IE: supports the premise; audit tool. |
| [Recoverability and Verifier Strength 2610.27465](https://arxiv.org/abs/2610.27465) / [Compositional Safety Failures 2609.33123](https://arxiv.org/abs/2609.33123) | Sep–Oct 2026 | Verifier-strength metric; gates miss interactions among individually safe updates. | IE (low): metric and analogy. |
| [SSR 2610.28815](https://arxiv.org/abs/2610.28815) | Oct 2026 | Harness evolvers degrade as the task pool grows; racing on evidence subsets fixes it. | **SL scoop** (headline). |
| [Scale and Selection 2609.39304](https://arxiv.org/abs/2609.39304) | Sep 2026 | More rollouts per round improves held-out success in robot harness evolution. | SL (high): bandwidth mechanism. |
| [DAEDALUS 2610.08048](https://arxiv.org/abs/2610.08048) | Oct 2026 | Self-generated tasks build memory nearly as well as curated ones. | SL (high): generated-task arm. |
| [GEPA-Curator 2610.37651](https://arxiv.org/abs/2610.37651) / [ActiveSaddler 2610.00906](https://arxiv.org/abs/2610.00906) ([ICLR27](https://openreview.net/forum?id=xmpyzuagpu)) | Oct 2026 | Curating which failures shape GEPA (+20.7%); bandit curriculum over failure patterns (+4.4/+7.5 pp). | SL: curation beats volume; should be arms. |
| [Trading Human Curation for Synthetic Augmentation 2606.03800](https://arxiv.org/abs/2606.03800) | Jun 2026 | Defines the trade rate ρ between augmented and human tasks in RLVR. | SL: the n_eff metric already exists. |
| [Harness Evolution as Learning 2609.36892](https://arxiv.org/abs/2609.36892) | Sep 2026 | Approximation/generalization/optimization decomposition; coverage–estimation bound. | SL: a theory scaffold that could "already explain" saturation. |
| [When Is Enough Enough](https://openreview.net/forum?id=0FaPTsSr9C) (ICLR27 sub) | Nov 2026 | Anytime-valid test for saturation of self-evolution. | SL (plateau as a stopping problem). |
| 2609.36535† / 2611.09701† | 2026 | Self-evolution degeneration via information gain; regret of where learning lives (judge-cited). | PG: priors for refresh decay. |
| HarnessPro 2610.26770† / HarnessMM 2610.32813† | Oct 2026 | Cost-aware harness RSI (Pareto, token efficiency) (judge-cited). | IE: may already show pruned verification. |
| 2609.32452† / 2609.39927† / 2610.36774† | 2026 | What the reflector sees; AdaGEPA feedback allocation; batched GEPA (judge-cited). | SL: the evidence-per-update axis is crowded. |
| MetaExp 2610.21492† | Oct 2026 | Cited by R2 leads for the Decay arm. | ECD: must be read. |

---

### Meta-Overfitting: Self-Referential Improvers Get Better at Improving Only Where They Grew Up (`effect-cause-decay`) — panel 72.1/100

**Claim.** Run a Gödel-lite improver to k* = 12 accepted self-edits on ALFWorld (A). Take its procedure, wipe its memory and scrub domain facts from it (Pk^s), and give it to a fresh executor. It must beat gen-0 at home by more than 3 pp (the gate). On a far domain it should do *worse* than gen-0: D_far < 0, with a lineage-level CI over n = 10. The deficit should shrink with pre-registered A→B distance and cross zero between the near and far domains. The paper is falsified if D_far ≥ 0 while the gate holds.

**Minimal design.**
- Improver: Qwen3.5-35B-A3B, with DeepSeek V4 Flash as fallback. Executor: 8–9B.
- 10 lineages. Five arms at matched improver-call budget: P0+∅, Pk^s+∅, Pk+∅, P0+Mk, Pk+Mk.
- Domains: ALFWorld, then ScienceWorld (near), SelfImprove-Bench games (mid) and Reasoning Gym (far). Distance is the executor-harness zero-shot transfer gap.
- Controls: scrub control, length-padded P0, leak audit, and a manipulation check (edit-category JS divergence).
- Analysis: lineage-level t-tests and a mixed-model dose slope; MDE ≈ 4 pp.
- A 4-lineage replicate using a VERSE port. Cost $1.4–1.7k after a pilot of at most $50 (Nov 16–23). OSF pre-registration by Dec 20; preprint Feb 15.

**History.** Round 1 had three candidates: procedure-vs-memory (72.5), meta-plasticity (67.9) and swap-factorial (71.7). They were merged into "Effect, Cause, Decay" (panel 75.8, lead). The R2 red teams cut it (67.5 / 67.9) as "three half-papers", each with a one-cell precedent. The v2 rewrite as Meta-Overfitting kept only the Decay claim and self-scored 77.9 (Recommended).

**Judge panel** (spread 6.7: the AC is most positive, the expert most skeptical on Surprise and Feasibility)

| Lens | N | S | R | C | F | T | Total | Verdict |
|---|---|---|---|---|---|---|---|---|
| AC | 3 | 4 | 5 | 3.5 | 3 | 4.5 | 75.0 | pursue-with-fixes |
| Expert | 3 | 3.5 | 5 | 3 | 2.5 | 3.5 | 68.3 | pursue-with-fixes |
| Mentor | 3 | 4 | 5 | 3.5 | 2.5 | 4 | 72.9 | pursue-with-fixes |

- **AC.** *Strongest:* one signed, falsifiable claim against a live positive-transfer consensus (RSI-Harness, MetaEvoForge, FreeEvolve, Looped-RSI), none of which wipes memory, scrubs or grades distance. *Weakest:* what is novel is only distance plus scrub, resting on one family, one home domain and one k*. ALFWorld→Reasoning Gym confounds distance with task format, and ZMmJDeDTXk makes the edit-rate risk real. *Must fix:* reframe as a bounded rebuttal with a reproduced positive near cell; cite ZMmJDeDTXk and aEPJT4hW84 as lineage; add a second home domain or an A↔B swap; separate distance from format; add a format adapter for P0+Mk; drop stability–plasticity; confirm GPUs.
- **Expert.** *Strongest:* the scrubbed Pk^s arm plus the distance dose is exactly the control the positive-transfer papers lack, and it sits on the mentor's codebase. *Weakest:* RSI-Harness asks the same question and reports the opposite sign. The null branch (D_far ≥ 0) is no longer "first evidence", so the paper is effectively one-sided, and the far domain may just be a format shift. *Must fix:* re-pre-register what the null adds; make a second improver model a full arm, not a 4-lineage replicate; use an agentic far domain (e.g. an RSIBench split).
- **Mentor.** *Strongest:* perfect fit with Gödel Agent; pre-registration and n = 10 are above this literature's bar. *Weakest:* feasibility. Ten open-model lineages each reaching 12 accepted improver edits, then 5 arms × 4 domains × 5 rounds, with no confirmed GPU node, is heavy for a solo undergrad. *Must fix:* the pilot answers the edit-rate question first (forced-edit pivot, fallback k* ≈ 6); confirm GPUs with Xunjian before Nov 23; cut to 3 domains × 3 arms (P0, Pk^s, Pk+∅) if variance is high.
- **Skeptical reviewer (expert):** "Given that RSI-Harness and MetaEvoForge already report that evolved improvers transfer across domains, this paper's single-family, single-open-model evidence that they do not on one hand-picked far domain reads as a boundary condition on a format shift, not a law of meta-overfitting."

**Feedback received**
- R1 procedure-vs-memory critique → the partition is ill-defined, so a scrub and leak audit are needed → **yes** (Pk^s primary, leak judge, κ curve, P0^s control).
- R1 → format-adapter arm for transplanted memory → **no**. Still open, and all three judges repeat it.
- R1 → use ≥3 improver families → **partly** (1 family plus a 4-lineage replicate). Judges now want a second full model.
- R1 meta-plasticity → isolate the meta level from Forgetting; report meta-edit share → **largely** (memory-free scrubbed arm, edit-rate GO threshold).
- R1 → avoid the "stability–plasticity" framing (2610.25858 owns it) → **no**. All judges flag it.
- R1 swap-factorial → the swap is a design, not an idea; watch for winner's curse → **yes** (Effect demoted to a gate; incumbent pre-registered).
- R2 novelty-killer → one-cell precedents for each arm; make κ the headline; add a shuffled-memory placebo → **partly** (Decay is the headline, κ secondary, placebo optional).
- R2 sharpness-reviewer → one directional claim, a gate, ≥10 lineages, a distance dose, cut sprawl → **yes** (5 arms, not 4).
- R2 fresh-scoop → RSI-Opt shows product-level transfer peaking early → **yes**, differentiated.
- R2 → OpenReview ICLR 2027 unchecked → **now checked** (Nov 12 sweep): *not scooped, but must be repositioned* against RSI-Harness, MetaEvoForge, FreeEvolve, Looped-RSI, Meta-TTL and RSIBench, and cite ZMmJDeDTXk/aEPJT4hW84/HMED as lineage.
- Open: GPU node unconfirmed; 2611.07624 and 2610.14662 not read in full; the "reset procedure, keep memory" rule conflicts with Forgetting and Branch-reset.

**Bottom line.** Pursue. This is the strongest idea in the topic and the natural host for the others. Reframe it now as a controlled boundary on the ICLR 2027 positive-transfer wave, with a reproduced positive near cell, an agentic far domain that does not change format, and a second improver model. Let the Nov 16–23 pilot decide on the improver-edit rate before anything else.

---

### Self-Reference Buys No Self-Knowledge: Lineage-Level Privileged Access, Memory Transplant, and Self-Model Drift in Self-Modifying Agents (`sharp-no-privileged-self-access`) — panel 68.8/100

**Claim.** After Gödel-lite lineages diverge, a foreign improver with the same base model, given the target lineage's code, traces and memory, predicts and repairs that lineage's failures as well as its own improver does. A Social Relations Model "self" effect is about 0 once memory is transplanted. Stored self-descriptions also go stale faster than a stranger's fresh reading of the current code. Falsified if the diagonal effect exceeds 0.05 AUROC on the disagreement subset and survives the transplant.

**Minimal design.** 8 lineages (Qwen 27B/9B), checkpoints at gens {5, 10, 20, 40} after a divergence check. An 8×8 improver×agent round-robin × {memory, none}. Three probes: predict (within-task and disagreement-subset AUROC plus a difficulty oracle), repair (matched same-executor rollouts, separating selection from proposal), and describe. A staleness curve. Pilot under $50 to measure the disagreement rate; full cost $1–1.5k.

**History.** New in R2 as bold idea SHARP-2. The bold-check gave 73.3 as pitched (about 77 with fixes) and redefined "self" as the lineage, added the transplant 2×2 and SRM, dropped gen-0 and changed the AUROC. REPORT ranked it #7 with "fold into #1".

**Judge panel** (spread 4.5; unanimous fold)

| Lens | N | S | R | C | F | T | Total | Verdict |
|---|---|---|---|---|---|---|---|---|
| AC | 3.5 | 2.5 | 4.5 | 3 | 3 | 3.5 | 66.3 (K2) | fold-into-another |
| Expert | 3.5 | 3 | 4.5 | 3 | 3 | 4 | 69.2 | fold-into-another |
| Mentor | 3.5 | 3 | 5 | 3 | 3 | 4 | 70.8 | fold-into-another |

- **AC.** *Strongest:* an unusually well-controlled test of Gödel Agent's premise (SRM, transplant, disagreement AUROC, matched rollouts); the ICLR sweep marks the question open. *Weakest:* the null is close to built in. With shared weights and improvers that rarely self-edit (ZMmJDeDTXk, aEPJT4hW84), the "own improver" is just the foreign improver plus memory. *Must fix:* verify the improvers diverge (forced edits, edit distance between lineages); kill if disagreement is under 10%; add Predict Before You Modify (ZBxV5bEYUH) with a foreign-lineage control; fold into #1; use 2 base families.
- **Expert.** *Strongest:* no self-vs-foreign improver study exists (sweep plus alphaXiv). *Weakest:* the known model-level null (2609.09899, 2608.30980); IDM is a positive self-forecasting foil; low disagreement would leave the test underpowered. *Must fix:* pilot first; a TOST equivalence test on the SRM relationship effect; shrink to 8 lineages × 2 checkpoints plus drift; 2 base models; fold into #1.
- **Mentor.** *Strongest:* the staleness curve is a dynamic only self-modification can produce. *Weakest:* the crossing is already in aEPJT4hW84 and ZMmJDeDTXk, and the 8×8×2×4 round robin is heavy for one person. *Must fix:* re-implement IDM's forecaster with a foreign-reader arm; scrub memory leakage through comments; stand it alone only if the pilot shows a non-zero diagonal effect.
- **Skeptical reviewer (mentor):** "Given that 2609.09899 and 2608.30980 already show outsiders predict a model as well as it predicts itself, a null diagonal effect in eight same-base-model Gödel-lite lineages is the expected outcome, and the paper does not convince me it would have detected privileged self-access had it existed."

**Feedback received**
- Bold-check W1 → gen-0 logic bug → **yes** (dropped; post-divergence checkpoints).
- W2 → pooled AUROC ties by construction → **yes** (within-task and disagreement subset).
- W3 → "self" must be the lineage, with memory manipulated → **yes** (2×2).
- W4 → the null is expected, which caps Surprise → **partly** (surprise moved to the decomposition and drift).
- W5/W6 → lineage-quality confound; repair scored by matched rollouts → **yes** (SRM; repair arm).
- REPORT → fold into #1 as its memory-transplant arm → **open decision**.
- ICLR27 sweep → self-knowledge is *open*; ZBxV5bEYUH is the closest work (no foreign control). Cite ZMmJDeDTXk and aEPJT4hW84 as crossing lineage → not yet reflected.

**Bottom line.** Fold into Meta-Overfitting. ECD already builds the procedure × memory infrastructure, so this becomes one extra arm: a foreign-lineage reader with transplanted memory, tested on IDM-style forecasting plus the staleness curve. Promote it to a standalone paper only if the pilot shows over 10% inter-lineage disagreement and a non-zero diagonal effect.

---

### Parametric Gödel Agent: Does Modular Weight Self-Modification Compound, or Does Self-Training Decay Survive Inside the Module? (`parametric-godel`) — panel 68.6/100

**Claim.** Over 5–8 rounds of self-generated verified-correction LoRA updates on 3–8B models, capability-specific adapters remove cross-capability interference but not the decay. Refreshing a module on its own corrections should reproduce Third Eye's negative continuation utility inside that module. Compounding comes only from inserting new adapters, and it stalls as routing degrades. If refresh does *not* decay, the alternative headline is "Third Eye's decay is interference, not collapse."

**Minimal design.** Qwen3-4B plus Llama-3.2-3B. Four capabilities (GSM8K/MATH, MBPP, BIRD, BFCL). Arms: insert / refresh / shared LoRA / HOLD, crossed with self vs gold data. Third Eye paired G1/G_{t+1} endpoints, P/S/I/F metrics, an oracle router and a capacity-matched shared LoRA, at ≥8 seeds. GPU cost about $2–4k, after a pilot under $100.

**History.** R1 candidate C17, inspired by WeAct. The critique scored 67.1, because "modular retains" is tautological, and reframed it as refresh-vs-insert. Ranked #10, "next paper (needs GPU)". No proposal was written and it was not in R2.

**Judge panel** (spread 1.3, the tightest; all pursue-with-fixes, but two raise K4)

| Lens | N | S | R | C | F | T | Total | Verdict |
|---|---|---|---|---|---|---|---|---|
| AC | 3.5 | 3.5 | 4.5 | 3 | 2 | 4 | 68.8 (K4) | pursue-with-fixes |
| Expert | 3.5 | 3 | 4.5 | 3 | 2.5 | 4 | 67.9 (K4) | pursue-with-fixes |
| Mentor | 3.5 | 3.5 | 4.5 | 3 | 2.5 | 3.5 | 69.2 | pursue-with-fixes |

- **AC.** *Strongest:* a real causal decomposition of Third Eye's decay where either sign is publishable; no ICLR submission runs it. *Weakest:* the ~2 pp effects vary by family and the grid exceeds an undergrad budget. The sweep never queried LoRA terms, so "no hit" is weak evidence. *Must fix:* a minimal core (refresh/insert/HOLD on self data, 3 capabilities, 5 rounds, ≥8 seeds); confirm GPUs or make it the WeAct group's sequel; a targeted re-sweep; position against FPaMYO47j4, IkYmTAyPXZ, T04ZzT5m6j and LRSI.
- **Expert.** *Strongest:* the 2×2 sits in a gap WeAct, Third Eye and Macaron all name as open. *Weakest:* decay persisting inside the module is the default expectation (2609.36535†, Third Eye's OOD loss), so the main deliverable is the tautological modular win. *Must fix:* add a mechanism measurement (diversity/entropy collapse of corrections); drop "Gödel" unless the agent actually decides; rename the plasticity probe (Jjh60xpFVY owns the term).
- **Mentor.** *Strongest:* squarely in Xunjian's WeAct line, which he has every reason to back. *Weakest:* about 1,000 train+eval cycles on unconfirmed GPUs; ecw2VvTeE5 offers a competing explanation (corrections concentrate on items already solved). *Must fix:* ask Xunjian about a WeAct sequel *before coding*; a 2-week pilot (refresh vs HOLD, horizon 3); log the difficulty of the correction data.
- **Skeptical reviewer (mentor):** "The modular-retains result is true by construction (WeAct Fig. 3), and the authors' real contribution, showing that refresh decay survives inside a module, rests on about 2 pp effects across a large, underpowered grid whose routing and capacity confounds are only partly controlled."

**Feedback received**
- R1 W1 → the modular win is tautological → **addressed on paper** (refresh-vs-insert reframe; no proposal).
- R1 W2 → separate interference from within-module degradation → **on paper** (the 2×2).
- R1 W3/W4 → capacity and routing confounds → **proposed** (capacity-matched LoRA, oracle router).
- R1 W5/W7 → ≥4 capabilities, a second family, ≥8 seeds → **proposed; raises cost, unresolved**.
- R1 W6 → "Gödel" is decoration unless the agent decides → **proposed** (scripted vs agent-decided ablation).
- R1 fix 8 → coordinate with the WeAct authors → **open**.
- R2 → no review at all; only the executor-swap echo (question 7 for Xunjian).
- ICLR27 sweep → no direct hit, but the area is crowded: FPaMYO47j4, CoEvo-RSI, the co-evolution "saturated lane", ecw2VvTeE5, LRSI.

**Bottom line.** Pursue only as a lab-backed second paper. It depends entirely on Xunjian: if the WeAct group has GPUs and plans a sequel, this is a clean, low-scoop extension. Otherwise park it. Do not merge it into ECD, because the substrate (weights) is different.

---

### Do Harness Gains Outlive Their Model? Forecasting Which Evolved-Harness Edits Survive a Model Upgrade (`harness-half-life`) — panel 61.1/100

**Claim.** When a harness evolved for v_n is carried to v_{n+1}, edits that only raise pass@1 (elicitation) are absorbed, and edits that raise pass@k or add information (expansion) survive. Each edit's survival is forecastable, in a pre-registered test, from its pass@1-vs-pass@8 profile on v_n alone. Warm re-evolution shows a plasticity deficit compared with resetting to the expansion edits. Three alternatives (durable, unforecastable, equivalent) are pre-registered.

**Minimal design.** About 10–12 transitions (Qwen ladders, Mistral, OLMo-3 stage checkpoints) behind a neutral tool adapter. GEPA plus a code evolver on AppWorld and τ². HDA neutralization. Leave-one-cluster-out ξ_j forecasts registered on OSF before successor targets are unsealed, plus a depreciation law and 8 re-evolution arms. Pilot ≤$50 using HDA's released harnesses.

**History.** R1 C13 (critique 64.6). The proposal rebuilt it (self-score 72.5) and the R1 panel gave 69.6 (#7, "feasible but off-agenda; side paper"). Dropped in R2 as crowded; a "home-model penalty" survives as an ECD add-on.

**Judge panel** (spread 0.5; unanimous fold)

| Lens | N | S | R | C | F | T | Total | Verdict |
|---|---|---|---|---|---|---|---|---|
| AC | 3 | 3.5 | 2.5 | 3.5 | 3 | 2.5 | 61.3 | fold-into-another |
| Expert | 3 | 3.5 | 2.5 | 3.5 | 2.5 | 3 | 60.8 (K4) | fold-into-another |
| Mentor | 3 | 3.5 | 2.5 | 3.5 | 3 | 2.5 | 61.3 (K4) | fold-into-another |

- **AC.** *Strongest:* a pre-registered edit-level forecast against a sealed successor, with a mechanistic anchor and real controls. *Weakest:* XoMy9wRqF1, zn5R6mJV8i and LRSI each take a piece, and OCDS, UpgradeBench and E/E cover the ingredients. Only about 80 noisy edit-transition pairs; source gains may be ~0; off-agenda. *Must fix:* run the ≤$50 pilot and kill if Δ_n is not significant or |Spearman(ξ)| < 0.3; beat static component ablation as a predictor; verify the second family; ship as an ECD section or a short note.
- **Expert.** *Strongest:* falsifiable and useful to practitioners; no direct hit in the sweep. *Weakest:* E/E + UpgradeBench applied to harnesses is K4, and either set of authors can fast-follow. *Must fix:* fold in; if it stays standalone, cut to 5–6 transitions, 1 evolver and 3 arms.
- **Mentor.** *Strongest:* unusually clean for a measurement paper. *Weakest:* XoMy9wRqF1's 84% null removals threaten the per-edit denominator; weak mentor fit. *Must fix:* fold the P1/P2 pilots plus the ξ forecast into ECD as a model-upgrade axis; gate on a neutralized Δ_n and ≥6 clean transitions.
- **Skeptical reviewer (expert):** "This is a careful measurement study that applies the elicitation/expansion lens from prior work to UpgradeBench's retain-or-refresh question for a new asset, but with a dozen same-family transitions and noisy edit clusters, I am not convinced the forecast tells practitioners anything beyond 'prompts mostly transfer, benchmark-specific hacks don't', which OCDS and HDA already suggest."

**Feedback received**
- R1 critique → novelty eroded on three sides → **partly** (E/E mechanism, rename; still N = 3).
- R1 → n = 2 transitions; Qwen idiosyncrasy → **on paper** (10–12 transitions; second family unverified).
- R1 → interface confounds; benchmark shortcuts → **yes** (adapter, IMR, HDA).
- R1 → the null warm ≈ cold claim needs power → **yes** (TOST, anytime AUC).
- R1 → weak meta-agent fit → **partly**; the panel still called it "off-agenda".
- R2 field trajectory → executor/model transfer is now a standard table; stay away as a standalone → **dropped instead**.
- ICLR27 sweep → no direct hit, but XoMy9wRqF1, zn5R6mJV8i, 4KSNZln9nS and QDdWiaNkk2 crowd it.

**Bottom line.** Fold into ECD as an optional model-upgrade or executor-swap axis, or plant the flag with a short arXiv note. Not worth a standalone NeurIPS slot given the crowding and the weak mentor fit.

---

### Recursive Gain vs Frontier Hardening: First Identified Micro-Estimate of R_AI from Controlled Self-Improvement Loops (`rsi-elasticity`) — panel 60.4/100

**Claim.** An off-diagonal improver × state grid in log-cost units identifies recursive gain g > 0 and frontier hardening σ. R_AI = g/σ ≈ 0.2–0.5 on search tasks and ≈ 0 held-out, with CIs excluding 1. Diagonal (trajectory) estimates mis-estimate both g and σ.

**Minimal design.** A 4×4 grid of I_k × S_k (k ∈ {0, 10, 20, 40}) × ≥5 seeds, in 3 systems (Gödel-style, Hyperagents-style, and a fixed-improver placebo). Burtsev log-cost capability. Fit log Δ = a·log c(I) − σ·c(S). Narrow vs broad split; frozen-knowledge arm; out-of-sample re-scoring. Pilot: a 2×2 for $50. Full cost under $2k.

**History.** R1 C3 (critique 64.6; 74.6 with fixes). Folded into the #1 merged paper as its estimand (rank #12). It disappeared in R2 when Effect was demoted to a gate.

**Judge panel** (spread 2.9; unanimous fold, K4 from all three)

| Lens | N | S | R | C | F | T | Total | Verdict |
|---|---|---|---|---|---|---|---|---|
| AC | 2.5 | 3 | 4 | 3 | 3 | 3 | 60.8 (K4) | fold-into-another |
| Expert | 2.5 | 3 | 4.5 | 3 | 3 | 2.5 | 61.7 (K4) | fold-into-another |
| Mentor | 2.5 | 2.5 | 4 | 3 | 3 | 3 | 58.8 (K4, K2) | fold-into-another |

- **AC.** *Strongest:* the estimand itself is unclaimed and is what macro RSI models ask for; diagonal-vs-off-diagonal bias would be a methods result. *Weakest:* the grid is ZMmJDeDTXk's nested replay, which arrived months early. With improvers not self-editing, g ≈ 0 and R_AI = 0/σ is foreseeable. *Must fix:* claim only the estimand; gate on improver edits; simulate identification and power first; re-estimate on Hyperagents history; fold in.
- **Expert.** *Strongest:* reach beyond ML into policy and econ. *Weakest:* σ is poorly identified when g has no variance; log-cost needs a reference solver for every state. *Must fix:* a secondary pre-registered analysis in ECD; forced edits; lineage n ≥ 10; the detection-law power analysis.
- **Mentor.** *Strongest:* the first plug-in number for METR/GovAI models. *Weakest:* an econ regression on someone else's assay (K4), with a likely trivial null (K2). *Must fix:* fold in, and drop the estimand if the pilot cannot separate 0.5 from 1.
- **Skeptical reviewer (expert):** "The improver × state swap is already in Improvement by Inheritance, which shows the improver barely changes. Fitting an economics elasticity to a recursive gain that is essentially zero gives a number, not a finding."

**Feedback received**
- R1 → the estimand is ill-defined in accuracy units → **yes** (log-cost).
- R1 → a fresh-artifact swap identifies only g → **yes** (4×4 grid), but never carried into a proposal.
- R1 → foreseeable result; underpowered; winner's curse; compute matching → **addressed in the fixed design only**.
- R1 → do not ship standalone → **followed**, then orphaned in R2.
- R2 fact-check → MetaRSI already shows g > 0 in-domain with decay → **not addressed**.
- R2 field forecast → a powered swap paper is 75% likely by May 2027 → this has **now happened** (ZMmJDeDTXk, aEPJT4hW84, MetaEvoForge in the ICLR27 sweep).

**Bottom line.** Fold into ECD as an optional secondary estimand computed on ECD's own grid, and only if the pilot shows improver self-edits and enough power. Otherwise drop it. As a standalone paper it is now K4.

---

### Does the Improver Learn a Model of Itself? A Leakage-Free Audit of Self-Forecast Skill in Self-Improving Agents (`self-forecast`) — panel 60.3/100

**Claim.** On sealed, noise-corrected edit pools, forecast skill does not rise across improver checkpoints once distribution shift is controlled. Self forecasts are no better than a fresh outsider's with the same archive, and shuffled forecasts recover most of IDM's gain. If skill *does* rise, it should predict swap-test improver gain, which would make it a cheap metric.

**Minimal design.**
- Leakage-free re-analysis of IDM, with ≥5 re-evaluations per edit for a noise ceiling.
- Checkpoint × sealed-pool crossing, scored by skill vs climatology and sibling AUC.
- Self vs outsider vs other-family forecasters.
- Online placebo ladder.
- Regression of forecast skill on swap gain.
- 2 loops × 2 families, using DGM logs. Pilot under $50, gated on a noise ceiling R² ≥ 0.3.

**History.** R1 C16 (critique 66.3). Folded into #1 as a readout. Absent from R2's v2; its self-vs-outsider piece came back as NPSA.

**Judge panel** (spread 3.3; unanimous fold)

| Lens | N | S | R | C | F | T | Total | Verdict |
|---|---|---|---|---|---|---|---|---|
| AC | 2.5 | 2.5 | 4 | 3 | 3.5 | 3 | 60.0 | fold-into-another |
| Expert | 2.5 | 3 | 4 | 3 | 3.5 | 3 | 62.1 | fold-into-another |
| Mentor | 2.5 | 2.5 | 4 | 3 | 3 | 3 | 58.8 (K2) | fold-into-another |

- **AC.** *Strongest:* the only leakage-free crossing with swap-test predictive validity; cheap and gated. *Weakest:* squeezed by IDM (now ICLR27 ZBxV5bEYUH), BCO, the model-level nulls and RWM surrogate learning. ZMmJDeDTXk means the checkpoints may be identical. *Must fix:* audit IDM on its own setup; gate on improver self-edits; disagreement-subset scoring plus memory transplant; power analysis; fold into ECD.
- **Expert.** *Strongest:* it directly tests the one positive "forecast rises" claim. *Weakest:* IDM may be accepted before NeurIPS, so this reads as a critique of one paper. *Must fix:* reproduce IDM's curve first; replace self-vs-outsider with a lineage transplant.
- **Mentor.** *Strongest:* the checkpoint × sealed-pool method is new. *Weakest:* a workshop-grade audit with expected nulls. *Must fix:* merge with NPSA as one ECD arm; show IDM's leakage concretely; kill if the noise ceiling is below 0.3.
- **Skeptical reviewer (mentor):** "This is a careful audit of a single recent method (IDM), whose main findings — forecast skill rising via in-context surrogate learning and models lacking privileged self-knowledge — were already expected from prior work, so it is unclear what new capability or insight the community gains."

**Feedback received**
- R1 → partly scooped by IDM/BCO; "first evidence of meta-learning" overclaims → **reframed as an audit** (unimplemented).
- R1 → in-context surrogate confound; range restriction; noise ceiling → **in the fix list** (unimplemented).
- R1 → archive availability (only DGM logs are confirmed) → **unresolved**.
- R1 panel → fold into #1 as a readout → accepted, then **dropped from v2**.
- R2 NPSA bold-check → disagreement-subset scoring and memory transplant needed → **not applied here**.
- ICLR27 sweep → IDM is a live ICLR submission (ZBxV5bEYUH); ZMmJDeDTXk undermines the "improver checkpoint" construct.

**Bottom line.** Fold into ECD together with NPSA as a single "self-knowledge" arm: forecast skill on sealed pools against swap gain, with a foreign-lineage reader. Kill it if the $50 noise-ceiling pilot fails.

---

### Breaking the Improver: Does Robustness Assimilate Through the Gate? (`breaking-the-improver`) — panel 55.2/100
Claim: intervened-validation drop predicts OOD transfer better than clean score. Gating on k intervention families does not change the per-generation robustness slope on held-out families at matched clean accuracy; design is 2–3 evolvers on τ-world/BreakingWeb. History: R1 C15 68.3, then the R2 merged canalization variant 65.8 (Weak, #11). Panel: AC 57.1 / expert 54.2 / mentor 54.2, all fold, all K4 (spread 2.9). Strongest point: the held-out-family test plus the matched-accuracy control are uncontested. Weakest: CODA, IPI and Masked Erosion own "selection makes fragility"; ICLR27 [5lWjRTK6Vk](https://openreview.net/forum?id=5lWjRTK6Vk) takes the false-done mechanism; ICLR27 CHASE ([uzzum63KP4](https://openreview.net/forum?id=uzzum63KP4)) is close to the gate-exposure arm; the τ-world generator does not exist. Must fix: make CHASE and VHE baseline arms; replace τ-world with BreakingWeb plus [WebVariants](https://openreview.net/forum?id=TYSv9rIhiQ); one pooled slope contrast with a power analysis. Reviewer: "…robustness trained on three perturbation families does not transfer to four unseen ones, which the domain-randomization literature would predict." **Bottom line:** fold in as a robustness section of useful-fictions (BreakingWeb belief tracker), not ECD; do not run standalone.

### Self-Concealing Erosion: Cost-Aware Improvers Prune the Checks Their Own Gates Depend On (`improver-erosion`) — panel 53.6/100
Claim: under an RSI-Tax-style cost penalty, improvers prune the verification layers that feed their own gate. The gate-visible score rises while frozen held-out gain falls, and Masked Erosion's margin-aware audit fails unless it is external. History: R1 C9 at 55 (park; 74.6 if reframed); never proposed, absent from R2. Panel: AC 55.8 (K1, K4) / expert 54.6 (K4) / mentor 50.4 (K1, K4), all fold (spread 5.4). Strongest: the internal-vs-external audit contrast fits Gödel Agent's self-editable verifier. Weakest: Masked Erosion covers the mechanism and fix. ICLR27 [C6EElQ2PRG](https://openreview.net/forum?id=C6EElQ2PRG), [5lWjRTK6Vk](https://openreview.net/forum?id=5lWjRTK6Vk) and [Id4G8bzzKO](https://openreview.net/forum?id=Id4G8bzzKO) take the visible-vs-held-out divergence. [23VDi5bfRj](https://openreview.net/forum?id=23VDi5bfRj) proves the "evaluator outside" result, and [yl1XFtnTn4](https://openreview.net/forum?id=yl1XFtnTn4) shows guards persisting unused. The deletion-pressure premise is also weak (verification is 2–7% of cost). Must fix: a 2-week pilot showing natural pruning by gen 15, or drop. Reviewer: "…reads as a recombination of the three in a new testbed rather than a new finding." **Bottom line:** park; at most an "editable verifier" arm of the error-threshold idea.

### More Tasks, No Gain: The Data Multiplier and Saturation Law of Harness Learners (`saturation-law`) — panel 52.0/100
Claim: held-out gain depends on distinct latent rules seen per update, so n* ∝ R/k; tested by sweeping k and R in LatentRule-τ, with out-of-sample prediction on τ²/AppWorld. History: R1 C10 at 60.8 (#17, park, "section inside curriculum-goodhart"), but the Blind-Spot Hiding v2 did not keep it; no R2 review. Panel: AC 51.7 park (K4) / expert 53.8 fold (K4) / mentor 50.4 park (K2, K4) (spread 3.4). Strongest: the R/k dose-response law is the one untested, falsifiable piece. Weakest: SSR has the headline, Scale-and-Selection the mechanism, DAEDALUS/2606.03800 the generated-task arm and n_eff. ICLR27 [NxGP6PGiaU](https://openreview.net/forum?id=NxGP6PGiaU), [FPaMYO47j4](https://openreview.net/forum?id=FPaMYO47j4), [LhhLpXBFgo](https://openreview.net/forum?id=LhhLpXBFgo) and [0FaPTsSr9C](https://openreview.net/forum?id=0FaPTsSr9C) give competing plateau explanations; mentor relevance is low (R = 2.5). Reviewer: "…the proposed R/k law is shown only in a synthetic rule world built to have exactly that structure." **Bottom line:** park. Revive only as a coverage section of Blind-Spot Hiding.

## Selection dynamics & evaluation rigor (gates, winner's curse, plateaus, error thresholds)

### Landscape
Between mid-2026 and the ICLR 2027 submission wave, the field established three things. First, the acceptance gate, more than the artifact being evolved, decides whether self-improvement is real (RSEA, TRUST, Winner's Curse). Second, greedy keep-if-better gates accept many inferior edits: TRUST measures 45.6%, Winner's Curse finds 13-20 pt overstatement at 16 selection items, and Measure First finds 16 of 18 codebases keep a change after one run. Third, several papers already explain the resulting plateaus and ceilings: evaluation cost (Theory of Reliable Self-Evolution), content vs process failures (Harness Evolution Hits a Ceiling), eventual negative drift under any fixed noisy gate (When Self-Reinforcement Ends Self-Improvement), and improver edits that amplify noise (EvoLyapunov, RAF > 1). Gate fixes are crowded too: TRUST, Statistical Gödel Machines, REUSE, audited acceptance, PID gates. The winner's curse itself is measured at the object level and in program search, including equal-budget random search matching fitness search (Mirage Discoveries). Two things are still open. One is quantitative, lineage-level laws: how the critical harness length L* depends on gate size n, defect load and edit mode, and whether editor code ratchets defects faster than task code. The other is mechanistic decomposition of plateaus into noise, headroom, epistasis and exhaustion. A recurring empirical threat cuts across these ideas: improvers almost never edit themselves (Improvement by Inheritance; SelfImprove-Bench), so any "self-referential" arm may end up empty.

### Closest papers
"ICLR27 sub" marks ICLR 2027 OpenReview submissions found in the Nov 12 sweep, which read abstracts and TLDRs only. † means not verified this session.

| Paper | When | Summary | Which of our ideas it threatens / how |
|---|---|---|---|
| [When Self-Evolution Backfires: Pre-Commit Gating against Skill Contamination](https://arxiv.org/abs/2608.05810) | Aug 2026, arXiv (Tencent) | Past a critical skill-pool size, new skills hurt, because defective skills become reference material for later ones ("contamination chains"). Rollback recovers 1.7 of 12.3 lost points. | **error-threshold (high):** already has the "knee in accumulation size" and lineage-inherited defects. It has no law, defect rate, gate-size scaling, edit-mode contrast or editor-vs-task split. |
| [A Kinetic Theory of the Gated Self-Evolving LLM Agent](https://arxiv.org/abs/2610.03243) / [23VDi5bfRj](https://openreview.net/forum?id=23VDi5bfRj) (ICLR27 sub) | Oct 2026 | Master-equation theory of gated plugin populations, with a hitting-time certificate, a "resolution law" pricing validation budgets, a proposal- vs information-exhaustion split, and a theorem that the evaluator must stay outside the population. | **error-threshold (medium):** scoped to plugin populations and excludes monolithic self-modifiers. **gate-dial (high):** already owns the resolution law and the exhaustion split. |
| [A Theory of Reliable Self-Evolution for Agent Harnesses](https://arxiv.org/abs/2609.08175) / [NxGP6PGiaU](https://openreview.net/forum?id=NxGP6PGiaU) (ICLR27 sub) | Sep 2026 | Gives a reliable-adoption condition and a Two-Gate validation rule, and proves that verification/evaluation cost sets the performance ceiling. Experiments are illustrative (DS-1000, one model). | **error-threshold:** ceiling depends on the gate only, with no length or editor term. **interference-plateau:** a rival "evaluation-cost" ceiling it must rule out. **gate-dial:** ceiling set by evaluation cost. |
| [When Self-Reinforcement Ends Self-Improvement](https://openreview.net/forum?id=T04ZzT5m6j) (ICLR27 sub) | Nov 2026 | Proves that with a judge making deterministic, recurring errors, every fixed acceptance rule that first improves eventually drifts negative. Two equally accurate judges can give opposite-sign long-run drift. | **error-threshold (high):** a theorem for "noisy gates ratchet down". **gate-dial:** any fixed gate drifts. |
| [Lyapunov-Gated RSI (EvoLyapunov)](https://openreview.net/forum?id=3t0j2Y05EH) (ICLR27 sub) | Nov 2026 | Treats RSI as a stochastic dynamical system in which improver edits raise future sensitivity to evaluator noise. Finds a sharp stability boundary at Recursive Amplification Factor > 1. | **error-threshold (high):** closest to the "mutator" angle (improver edits amplify error, with a threshold). Framed as control/stability, with no L*(n, ε) length law. |
| [Trustworthy RSI with Statistical Guarantees (TRUST)](https://arxiv.org/abs/2611.10787) / [wV4bK1dHpC](https://openreview.net/forum?id=wV4bK1dHpC) (ICLR27 sub) | Nov 2026 | Online FDR control for harness RSI. The greedy gate accepts 45.6% inferior updates and TRUST accepts 2.8%, with a final-gain lower bound. Bernstein/Hoeffding gates accept almost nothing over 30 rounds. | **gate-dial (high):** states the strict-vs-lax tradeoff in its abstract. **error-threshold:** required baseline ("why not just use TRUST?"). **meta-winners-curse:** gate-fix side taken. |
| [Statistical Gödel Machines](https://openreview.net/forum?id=0FWBc4gY08) (ICLR27 sub) | Nov 2026 | Replaces proofs with statistical certificates under a global error budget, bounding the chance of ever accepting a non-improving update. | Gate-fix baseline for **error-threshold** and **gate-dial**. |
| [Which Self-Improvements Should We Trust? (REUSE)](https://openreview.net/forum?id=cnaYLnpsAV) (ICLR27 sub) | Nov 2026 | Risk control for adaptive overfitting when RSI reuses a fixed benchmark across rounds. | Gate-fix baseline for **gate-dial** and **meta-winners-curse**. |
| [Measure First: The Noise Floor … Dominates Its Accept Rule](https://openreview.net/forum?id=VuzgEFCuwx) (ICLR27 sub) | Nov 2026 | Over 480 reruns of research-agent keep/discard loops, 16 of 18 audited codebases keep changes after a single run. E-process gates are invalid against a noisy baseline. | **gate-dial (medium-high):** already shows empirically that "gate resolution is the binding constraint". **error-threshold:** takes the "gates are noisy" framing. |
| [The Winner's Curse in LLM Self-Improvement Loops](https://arxiv.org/abs/2610.09239) | Oct 2026, arXiv | Keep-if-better treated as selection under noise. Qwen models rewrite their own instructions, scored on 600 held-out items. Most proposals after the first are harmful, with 13-20 pt overstatement at 16 items and 1-5 pt at 256. No tested rule beats greedy. | **meta-winners-curse (high):** the object-level version. **interference-plateau:** a rival "noise" account. **gate-dial:** closest gate comparison (stationary, T=20). |
| [Mirage Discoveries: Separating Signal From Selection Noise](https://openreview.net/forum?id=da1Czy7foS) (ICLR27 sub) | Nov 2026 | In LLM-guided program evolution, fitness acceptance finds the held-out best in only 5 of 12 runs, and equal-budget random search transfers as well. Proposes "audited acceptance". | **meta-winners-curse (high):** pre-empts the "placebo archives match much of the gain" headline. |
| [The Winner's Curse in LLM Pipeline Selection](https://openreview.net/forum?id=jd8luqTdQW) (ICLR27 sub) | Nov 2026 | Best-of-N prompt-selection bias grows with N and shrinks as 1/√m. Correlated candidates inflate it, corrected via an effective candidate count. | **meta-winners-curse:** pre-empts any "curse ∝ N/√m" result. |
| [Sampler Gains Shrink to Noise Under Equal Tuning Budgets](https://openreview.net/forum?id=7TrObbeBxf) (ICLR27 sub) | Nov 2026 | Winner's-curse formulas plus an equal-budget audit for decoding samplers. | Cite only (**meta-winners-curse**). |
| [Auditing Harness Evolution: Selected Gains, Persistent Edits, and Runtime Behavior](https://openreview.net/forum?id=yl1XFtnTn4) (ICLR27 sub) | Nov 2026 | An "RSI Information Ledger" finds an evolved lineage has no detectable advantage over one audit-selected edit at matched budget, and no detectable positive interaction among accepted edits. One accepted guard never fires in 3,960 checks. Small scale (4B, MBPP). | **interference-plateau (high):** a published prior null on exactly its effect. **error-threshold:** inert edits persisting below gate resolution. **meta-winners-curse:** matched-budget lineage audit. |
| [Lookahead-Guided RSI (LRSI)](https://openreview.net/forum?id=71wzcahKcb) (ICLR27 sub) | Nov 2026 | Toggles each candidate edit on and off in sampled future versions and keeps only edits that stay non-negative (+2.4 to 4.4 pp on 4 benchmarks). Not to be confused with the other "LRSI", V7vjimAYGi. | **interference-plateau (high):** owns the knockout-across-continuations tool and the "later edits make earlier ones harmful" intuition. Required baseline for rebase. |
| [Opinionated Optimizers: Why Agents Struggle to Improve Agents](https://openreview.net/forum?id=LhhLpXBFgo) (ICLR27 sub) | Nov 2026 | 3M rollouts over 216 meta-optimization runs. Proposer priors are narrow, noisy evals give weak evidence, and selection rarely compounds gains. | **interference-plateau:** a rival non-epistatic plateau account. |
| [Harness Evolution Hits a Ceiling: When Weight Training Should Begin](https://arxiv.org/abs/2610.11655) / [FPaMYO47j4](https://openreview.net/forum?id=FPaMYO47j4) (ICLR27 sub) | Oct 2026 | Harness evolution fixes process failures, while content failures need weight training. Reports "inert accepts", such as an edit silently removing a 20-call brake, and positive component epistasis (ledger + lessons). | **interference-plateau:** rival "content ceiling" and "proposer blindness" accounts. **error-threshold:** anecdotal sub-resolution defects (supporting evidence). |
| [When Is Enough Enough in Self-Evolving LLM Systems?](https://openreview.net/forum?id=0FaPTsSr9C) (ICLR27 sub) | Nov 2026 | An anytime-valid sequential test for when evolution has saturated. | Treats plateaus as a stopping problem. Cite for **interference-plateau**. |
| [Quality-Blind Detection of Settling in Agentic AI Loops](https://openreview.net/forum?id=XfutYzVRFq) (ICLR27 sub) | Nov 2026 | No quality-blind detector can tell rising from falling quality in accept/reject loops. Proposes settling detection from artifact change. | Cite for **error-threshold** and **interference-plateau**. |
| [Recursive Hypercompression: Phase Transitions from Self-Improvement to Self-Deterioration](https://openreview.net/forum?id=j8GSV186VS) (ICLR27 sub) | Nov 2026 | Jointly evolves the learner, an inherited defect and the environment, and shows a phase transition from improvement to deterioration. | **error-threshold (medium):** same phase-transition-with-inherited-defects language, but for data/feedback, not gated edit lineages. |
| [Why Is RSI Hard to Verify?](https://openreview.net/forum?id=OnzWTehIaN) (ICLR27 sub) | Nov 2026 | Criterion and statistical tests for establishing RSI from traces, plus a cumulative-gain bound. | Cite (**error-threshold**, **meta-winners-curse**). |
| [Measured Behavior, Recorded Zero](https://openreview.net/forum?id=9UsBcc69Ua) (ICLR27 sub) | Nov 2026 | In the HGM setup, the selection signal scores invalid children and do-nothing agents identically (0.0). | Gate-failure anecdote. Undercuts HGM's signal (**meta-winners-curse**). |
| [On the Fragility of Self-Improving Agents](https://openreview.net/forum?id=st9i6DXgw7) (ICLR27 sub) | Nov 2026 | Memory-based self-improvers raise run-to-run variance in 71% of cases and are sensitive to task order. Loops amplify eval noise. | Supporting evidence for all four ideas. |
| [Wright–Gödel Machine](https://openreview.net/forum?id=cQkSamYGQi) (ICLR27 sub) | Nov 2026 | Proves Thompson-sampling parent selection collapses onto one lineage, and proposes a shifting-balance fix. | Same population-genetics vocabulary but a different phenomenon. Cite for **error-threshold**. |
| [Improvement by Inheritance: Measuring RSI in Coding Agents](https://openreview.net/forum?id=ZMmJDeDTXk) (ICLR27 sub) | Nov 2026 | Nested replay crosses improvers with common start states. Improvers almost never edit themselves, and an inherited self-edit improves task agents but not future improvers. | **error-threshold:** the self-referential arm may produce no editor-region data. **meta-winners-curse:** owns same-start improver swaps. |
| [DarwinX: Evolving Agent Harnesses Through Natural Selection](https://arxiv.org/abs/2608.07545) / [XVvUgp3Bu8](https://openreview.net/forum?id=XVvUgp3Bu8) (ICLR27 sub) | Jul 2026 | Population harness evolution with a preserve-and-extend contract, regression screening and recombination from common ancestors. About +17 pts across 4 suites. All WebArena-Infinity merges were reverted. | **interference-plateau (high):** states the interference motivation and owns a merge/rebase-like remedy, but measures no epistasis. |
| [RRSI: Regularized RSI of Agent Harnesses](https://arxiv.org/abs/2609.24972) / [75PPLZgaUM](https://openreview.net/forum?id=75PPLZgaUM) (ICLR27 sub) | Sep 2026 | Annealed edit budget plus a critic/pruner that removes changes that are too small, too costly or stale. +6.0 ID / +4.3 OOD with 30% fewer tokens. | **interference-plateau (high):** its pruner occupies the remedy, and its credit logs put epistasis "one appendix away". |
| [Harness-S: Learning from Sibling Harnesses](https://arxiv.org/abs/2611.04277) | Nov 2026, arXiv | Sibling harnesses from a common base show locality, complementarity and nonstationarity. Used to rescue failed RL rollouts. | **interference-plateau (medium):** asserts edit interference but measures only sibling repair overlap. |
| [Mendel Gödel Machine](https://arxiv.org/abs/2608.07645) | Aug 2026, arXiv | Reaction-norm mutation plus cross-lineage hybridization, with convergence proved under an additive fitness landscape. | **interference-plateau (low):** a positive epistasis result falsifies its additivity assumption. |
| [MESH-Harness](https://arxiv.org/abs/2610.05300) | Oct 2026, arXiv | Modular harness recombination under a full-covariance LinUCB (linear) surrogate. Beats Meta-Harness by 2-7 pts. | **interference-plateau (low):** a surrogate-misspecification corollary target. |
| [Masked Erosion: How Acceptance Gates Lose Redundant Safeguards](https://arxiv.org/abs/2610.38392) | Oct 2026, arXiv | 411 loops × 10 gate rules. Deletions behind a working redundant safeguard are invisible to the gate. A pre-frozen statistical model predicts where each gate breaks. | Methodological template (full-subset design, frozen predictive gate law) for **interference-plateau** and **gate-dial**. |
| [Meta^n: RSI through Emergent Depth](https://arxiv.org/abs/2608.24735) | Aug 2026, arXiv | Argues that self-editors must freeze part of their machinery to stay stable, caps meta-depth at about 2, and freezes the meta-operation Ω. | **error-threshold (medium):** assumes, without measuring, the instability that the editor-integrity curve would test. |
| [Mutation Without Variation](https://arxiv.org/abs/2606.05408) | Jun 2026, arXiv/ACM | Without selection, LLM mutation chains collapse to attractors (in 87% of chains, over 93% of mutations revisit a previously seen form). | Background for modelling ε (**error-threshold**). |
| [Adapting the Process of Adaptation](https://arxiv.org/abs/2611.07048) | Nov 2026, arXiv | Theory comparing Darwinian, Lamarckian and agent-directed evolution. Warns that extra self-directed search amplifies a misspecified target. | Framing citation (**error-threshold**). |
| [Are Self-Evolving Agents Continual Learners?](https://arxiv.org/abs/2610.25858) | Oct 2026, arXiv (Mila/GDM) | ReasoningBank/GEPA/Meta-Harness on 4 drifting streams. Plasticity and stability are anti-correlated (r = −0.72), and Meta-Harness's gate freezes the workspace in one ablation. | **gate-dial (high):** supplies the streams and metrics. Never varies the gate. |
| [When Validation Stops Learning: Auditing Update Admission](https://arxiv.org/abs/2609.10873) | Sep 2026, arXiv | Valid range gates freeze learning on an embodied pushing diagnostic (0% vs 31.6% admitted). Proposes a missed-opportunity metric. | **gate-dial (medium):** "valid gates freeze", shown without an LLM. |
| [PID-Gated Continual Prompt Adaptation](https://arxiv.org/abs/2610.16129) | Oct 2026, NeurIPS CL workshop | P/I/D gates for when to rewrite a prompt under drift and label noise. The integral gate matches TextGrad with 2.7-3.5× fewer calls. | **gate-dial:** occupies the noise-adaptive-gate method slot (baseline). |
| [RSEA: Recursive Self-Evolving Agents via Held-Out Selection](https://arxiv.org/abs/2606.28374)† | Jun 2026, arXiv | Reliability comes from the held-out gate, not the artifact. Removing the gate gives a 33-pt overfit. | Foundation for "gate > artifact" (**gate-dial**). Not fetched. |
| [Does Failure Feedback Direct Agent Self-Modification?](https://arxiv.org/abs/2610.14662) | Oct 2026, arXiv | Across 145 DGM self-modifications there is no target-specific repair. A zero-mean-edit simulation under DGM's rules reproduces DGM's archive maxima. | **meta-winners-curse (high):** pre-empts "archive gains = selection on noise" (simulated). |
| [Beneath the Surface: What We Overlook in Agent Evolution](https://arxiv.org/abs/2611.07624) | Nov 2026, arXiv | All 16 evolved agents lose some previously covered tasks. The top-scoring agent is never the most improvable parent, and stricter admission discards useful alternatives (EvoAudit). | **meta-winners-curse (high):** small-n parent-potential validation. **interference-plateau:** edit-level attribution overlap. |
| [Harness-Delta Attribution](https://openreview.net/forum?id=PX4Zpvsaca) (ICLR27 sub; arXiv 2610.22466†) | Oct-Nov 2026 | Splits an evolution gain into overfitting, test-time compute and a residual, across 11 executors and 3 frameworks. | **interference-plateau:** attribution overlap (must read). |
| [Auditing the Product, Not the Recursion](https://arxiv.org/abs/2610.27610) | Oct 2026, arXiv | Coding of 114 RSI studies: only 22% audit 2 or more reruns, 19% use null controls, and 4 of 26 improver claims use same-start, matched-budget comparisons. | **meta-winners-curse:** motivation, and a sign the idea is "in the air". |
| [Where Should the Evaluation Budget Go in Scaffold Evolution?](https://arxiv.org/abs/2610.37276) | Oct 2026, arXiv | How to split evaluation budget between guiding search and choosing the final scaffold. | **meta-winners-curse:** competes with any "fix" section. |
| [Huxley-Gödel Machine (CMP)](https://arxiv.org/abs/2510.21614) | Oct 2025 | Metaproductivity-performance mismatch. Clade-metaproductivity (CMP) guides tree expansion. | Foundation/target for **meta-winners-curse** (CMP never validated against an oracle). |
| [Darwin Gödel Machine](https://arxiv.org/abs/2505.22954) | May 2025 | Open-ended archive of self-modifying coding agents. | Foundation (**meta-winners-curse**). |
| [Gödel Agent](https://arxiv.org/abs/2410.04444) | Oct 2024 (ACL 2025) | Self-referential agent that modifies its own logic, including its improvement routine. | Foundation and base system for **error-threshold** (mentor is first author). |

---

### The Self-Modification Drift Barrier: editor defects ratchet in below gate resolution, giving Gödel-style agents a lower complexity ceiling than fixed improvers (`nonobv-error-threshold`) — panel 67.2/100

**Claim.** A gated self-modifier purges a defect only if the defect's effect on the gate score exceeds the gate's resolution, about σ/√n. Editor code is selected only second-order, through later edits, so it sits below that barrier and retains defects faster than task code. Retained held-out gain collapses at a critical harness length L*. L* rises with n, falls with defect load, exists for full rewrites but moves under patch editing, and is lower for a self-referential improver than for a frozen-editor one. The idea is falsified if region retention is equal, if L* is flat in n, or if patch and rewrite editors behave the same.

**Minimal design.** Harnesses are seeded at L ∈ {1k, 4k, 16k, 48k} tokens with executed redundant modules. This is crossed with rewrite vs patch editing, self-referential Gödel Agent vs the same editor frozen, and n ∈ {8, 32, 128}. Runs use 2 backbones × 3 seeds × 15 generations on 2 domains. Each generation gets a defect audit: a 20-request editor-probe suite, region-tagged unit tests, and hunk labels (beneficial / neutral / defective). The law is fitted on injected-noise calibration runs only and then predicts each backbone's natural ceiling out of sample. Pilot under $60; full study about $900.

**History.** Not in R1. In R2 it was born as forecast Q1, then became bold idea NONOBV-5 (Eigen error catastrophe). The bold-check scored it 74.2 (Promising) and reframed it from Eigen to a drift barrier / Muller's ratchet under a noisy gate. It ranked #6 of 11 in the R2 REPORT and was offered as a backup recommendation.

**Judge panel** (spread 3.0, the panel agrees)

| Lens | N | S | R | C | F | T | Total | Verdict |
|---|---|---|---|---|---|---|---|---|
| AC | 3 | 3.5 | 4.5 | 3 | 3 | 3 | 67.1 | pursue-with-fixes |
| Expert | 2.5 | 3.5 | 5 | 3 | 2.5 | 3.5 | 65.8 | pursue-with-fixes |
| Mentor | 3 | 3.5 | 5 | 3 | 3 | 3 | 68.8 | pursue-with-fixes |

- **AC.** *Strongest:* a sharp, falsifiable editor-vs-task retention prediction with clean manipulations (L, edit mode, n, frozen editor) and out-of-sample ceiling prediction, built on the mentor's own system. *Weakest:* EvoLyapunov (RAF > 1), T04ZzT5m6j, TRUST, SGM, yl1XFtnTn4 and NxGP6PGiaU each take a piece. Patches and rare self-edits may leave too few editor events at 3 seeds × 15 generations. *Must fix:* position against EvoLyapunov and the drift theorem; add TRUST/SGM gate arms and show L* moves but does not vanish; derive L* for the actual elitist accept rule and pre-register slopes; verify or force editor edits in the pilot; use ≥5 seeds on critical cells; validate the hunk audit region-blind against human labels; add an organically grown harness control.
- **Expert.** *Strongest:* the frozen editor-probe suite directly tests Meta^n's unmeasured premise, and every falsifier gives a clean outcome. *Weakest:* Improvement by Inheritance (ZMmJDeDTXk) finds improvers almost never self-edit, so the self-referential arm may be empty. Padding to 48k, 3 seeds and an underived law make it fragile. *Must fix:* EvoLyapunov RAF gate plus one gate fix as baselines; formal L* derivation first; pilot go/no-go of at least 1 accepted editor-region edit per 4 generations, otherwise force or inject editor mutations; human-validated auditor; ≥5 seeds near the knee; justify or replace padding.
- **Mentor.** *Strongest:* a direct stress test of Gödel Agent. The editor-region asymmetry plus the L* law is the one piece nobody owns, and the pilot is cheap. *Weakest:* the same crowding, plus the self-edit rarity, which forces artificial edits. *Must fix:* run the pilot gate first and, if self-edits are rare, report "improvers rarely self-edit" as a protective mechanism; derive L* and say what it predicts beyond T04ZzT5m6j and EvoLyapunov; use TRUST/e-process baselines; ≥5 seeds; prune the grid (drop 48k, or run n=128 at only 2 lengths); validate the audit against injected defects with a per-region confusion matrix.
- **Skeptical reviewer (mentor lens):** "Given T04ZzT5m6j's negative-drift theorem, EvoLyapunov's recursive-amplification boundary and Improvement by Inheritance's finding that improvers rarely edit themselves, the paper must show that its editor-region ratchet arises in natural self-modification rather than in padded, forced-edit harnesses, and that its L* law predicts something those works do not."

**Feedback received**
- R2 bold-check (scoop): no paper fits a defect-rate × length law, compares self-referential vs fixed improvers, or measures editor integrity. → Informational; kept at the center.
- R2 bold-check: 2608.05810 already has a size knee and inherited defects. → Partly addressed; repositioned as the quantitative law plus mechanism.
- R2 bold-check: Meta^n and Kinetic Theory say qualitatively that self-reference costs stability. → Addressed in positioning.
- R2 bold-check (core): Eigen is the wrong analogy, because a gated elitist patcher has no catastrophe under a perfect gate. → Addressed: reframed as a ratchet, with edit mode as a manipulated variable.
- R2 bold-check: state the law in gate resolution σ/√n, since "ln(s)" was hand-waved. → Addressed in the claim. **The formal derivation is still not done.**
- R2 bold-check: the mutator claim was under-specified. → Addressed with region audits and the probe suite.
- R2 bold-check: injected noise is artificial. → Addressed: used only to calibrate, then predict natural ε out of sample.
- R2 bold-check (power): the grid was fragile. → Partly addressed: L is seeded and there is a pilot, but seeds are still 3.
- R2 bold-check (surprise): self-ref < fixed is expected; the editor-region barrier is the surprising part. → Acknowledged; patch vs rewrite treated as a falsifier.
- ICLR 2027 sweep: "still differentiated, but crowded". Do not pitch "gates are noisy" or "winner's curse at the gate". Must cite T04ZzT5m6j, 3t0j2Y05EH, NxGP6PGiaU, 23VDi5bfRj, j8GSV186VS, da1Czy7foS, VuzgEFCuwx; use TRUST, SGM, REUSE and audited acceptance as arms. → **Not yet addressed.**
- ICLR 2027 sweep: ZMmJDeDTXk means the self-referential arm may be underpowered. → **Not yet addressed; this is the go/no-go pilot.**

**Bottom line.** This is the strongest idea in the topic and a credible backup to the lead. Pursue only after a cheap pilot shows that Gödel Agent accepts editor-region edits at a measurable rate (≥1 per 4 generations), and after L* has been derived on paper against the TRUST/SGM/EvoLyapunov baselines. If self-edits are rare, the honest paper is about forced editor mutations plus "improvers protect themselves by not self-editing". The gate-dial plasticity half could fold in here.

---

### Late Gains Are Masked, Not Missing: Decomposing Self-Improvement Plateaus with Edit-Level Epistasis (`sharp-interference-plateau`) — panel 62.1/100

**Claim.** The held-out effect of accepted edits shrinks with lineage depth. After de-noising and after fitting a global-epistasis (headroom) link, a substantial share of that shrinkage is idiosyncratic pairwise or sign epistasis among accepted edits. Rebasing a de-noised, interaction-aware subset of edits onto an ancestor therefore beats the tip at matched compute. The claim is falsified if rebase ≤ tip (TOST ±1 pp) and pairwise terms add less than 10% held-out R² over global epistasis.

**Minimal design.** About 6 × 30-generation Gödel-lite lineages on 2 domains, logging structured patches for accepted and rejected proposals. An edit-graft matrix applies edits onto gen-0, gen-k/2 and the parent, scored on a ≥300-item fresh pool × 3 seeds, with graft applicability reported as a result. For the top-15 edits, about 200 random-subset knockout libraries are fitted with additive, global-epistasis and pairwise models. A pre-registered four-way decomposition separates noise, headroom, idiosyncratic epistasis and exhaustion/blindness, with rejected late proposals grafted onto gen-0. Meta-level edits are scored by next-k acceptance. Rebase vs tip vs bag-of-edits at matched tokens. Cost $1-1.5k; pilot under $100.

**History.** Not in R1 (nearest relative: context-debt, 65, folded into the lead). In R2 it was born as SHARP-1, which bold_ideas called the strongest bold idea overall. The bold-check scored it 74.6 (#4 of 11), called the interference-vs-exhaustion binary false, and reframed it as the four-way "masked, not missing" decomposition.

**Judge panel** (spread 5.9, the expert is notably harsher)

| Lens | N | S | R | C | F | T | Total | Verdict |
|---|---|---|---|---|---|---|---|---|
| AC | 2.5 | 3.5 | 4 | 3 | 3 | 3.5 | 63.8 | pursue-with-fixes |
| Expert | 2 | 3.5 | 4 | 3 | 2.5 | 2.5 | 58.3 | fold-into-another |
| Mentor | 2.5 | 3.5 | 4.5 | 3 | 2.5 | 3.5 | 64.2 | pursue-with-fixes |

- **AC.** *Strongest:* the only design that separates all four plateau accounts with a clean estimand (pairwise R² share above a global-epistasis link) borrowed from deep mutational scanning (DMS). *Weakest:* LRSI owns the knockout tool and remedy, and yl1XFtnTn4 already reports no detectable interaction, so a null is the prior favourite. Graft applicability is unresolved. *Must fix:* make LRSI and RRSI-pruned tips baselines for rebase; reproduce the yl1XFtnTn4 null as one cell and show where epistasis turns non-zero; add the evaluation-cost (NxGP6PGiaU) and content (FPaMYO47j4) ceilings as cells; pilot gates of ≥60% graft applicability and ≥15 accepted edits per lineage; verify the biology citations; run a power analysis.
- **Expert.** *Strongest:* the rejected-proposal graft and the global-epistasis baseline are the right imports, and either outcome gives a one-line takeaway. *Weakest:* scoop risk has gone from low-medium to medium-high (LRSI, yl1XFtnTn4, Opinionated Optimizers, Harness-Delta Attribution, Beneath the Surface). What remains novel is narrow, and the engineering load is heavy for a solo undergrad. *Must fix:* LRSI baseline; treat the yl1XFtnTn4 null as the prior and expect noise + headroom as the headline; read PX4Zpvsaca/2610.22466 and 2611.07624; drop the standalone paper if fewer than 50% of grafts apply; realistically, fold this into Meta-Overfitting as a mechanism section.
- **Mentor.** *Strongest:* the clearest mechanistic diagnosis of plateaus, close to the Gödel Agent agenda (meta edits scored by downstream acceptance). *Weakest:* likely outcome "mostly noise + headroom", with biased self-contained-edit samples and too much evaluation spend for 6 months. *Must fix:* reposition as a decomposition rather than a method, with an LRSI baseline arm; reconcile with yl1XFtnTn4 (underpowered or shallow lineage?); run the 2-week, under-$100 pilot and pivot to a null-framed paper if grafts fail or the shrinkage disappears after de-noising; cut to 1 domain plus a small replication and drop the Lenski and surrogate extras.
- **Skeptical reviewer (expert lens):** "LRSI already knocks edits out across future versions and the ICLR 2027 'Auditing Harness Evolution' submission found no interaction among accepted edits, so the novelty reduces to a global-vs-pairwise R² decomposition on two small Gödel-lite benchmarks whose graft-applicability filtering makes the estimand hard to trust."

**Feedback received**
- R2 bold-check: no K1 scoop for an in-lineage graft/epistasis matrix. → Informational.
- R2 bold-check: interference is asserted in passing (DarwinX, Harness-S) and rebase/merge/prune already exist. → Partly addressed: novelty moved to diagnosis.
- R2 bold-check: the binary was false (it omitted noise, blindness and headroom). → Addressed: four-way decomposition with fresh-pool de-noising.
- R2 bold-check: diminishing returns follows from headroom alone, so fit global epistasis first. → Addressed (biology citations unverified).
- R2 bold-check: grafts often won't apply. → Partly addressed: applicability is reported as a result. Still the main engineering risk.
- R2 bold-check: meta edits pay off later. → Addressed: scored by next-k acceptance.
- R2 bold-check (power): 105 pairs × 3 seeds was too few. → Addressed: ~200 random subsets and a pre-registered R² estimand.
- R2 bold-check: 2 domains and a ≥300-item pool; realistic cost $1-1.5k. → Adopted on paper.
- R2 scoop watch: DarwinX, RRSI and Winner's-Curse teams. → Open.
- ICLR 2027 sweep: medium-high risk. LRSI must be a rebase baseline; rule out the FPaMYO47j4 and NxGP6PGiaU ceilings; cite yl1XFtnTn4, LhhLpXBFgo, 0FaPTsSr9C, XVvUgp3Bu8. → **Not yet addressed.**

**Bottom line.** It is no longer a strong standalone paper, because LRSI holds the tool and yl1XFtnTn4 holds the prior null. Run the under-$100 graft-applicability pilot. If it passes, the most likely good outcome is a mechanism section inside Meta-Overfitting, or a null-framed "plateaus are mostly noise + headroom" decomposition, not the "masked" headline.

---

### Gate Dial: the acceptance gate as the stability-plasticity dial (`gate-dial`) — panel 58.7/100
**Claim / design:** In long, drifting streams, lost plasticity comes mostly from the gate rather than the proposer. Gates freeze once per-edit effects fall below z_α·σ/√n. Gate scope and freshness move the frontier more than α does. Design: a strictness × scope × freshness factorial on a procedural τ-style stream with hundreds of switches, counterfactual re-gating of logged candidates, oracle and always-accept bounds, matched compute, 5-8 seeds, $400-800.
**History:** R1 candidate C6, critique 62.9 (~76 projected with fixes), ranked #15 and parked because TRUST and Winner's Curse are too close. Not developed in R2; its mechanism resurfaced inside the drift-barrier idea.
**Panel (spread 5.0):** AC 55.8 / Expert 60.8 / Mentor 59.6. All three say **fold-into-another** and all flag K4 (recombination of existing work). All three name counterfactual re-gating and scope/freshness > α as the only open slot. They agree the resolution law is already in Kinetic Theory, NxGP6PGiaU and Measure First (VuzgEFCuwx), and the tradeoff is in TRUST's abstract, so the freeze-round prediction is close to a tautology. Reviewer (AC): "what remains is a well-instrumented sweep of n, alpha and validation scope on a procedural generator."
**Feedback:** R1 fixes (resolution-matched arm, matched compute, adaptive gate demoted to a baseline, counterfactual attribution) were adopted on paper, never run. The ICLR 2027 sweep adds T04ZzT5m6j, SGM, REUSE and VuzgEFCuwx as citations or baselines.
**Bottom line:** Fold into the drift-barrier idea as its false-reject/plasticity half (one σ/√n account of ratcheting in and freezing out). Headline the pre-registered scope/freshness test and the re-gating attribution share. Pilot first to check that effect sizes actually decay into the resolution band.

### The Winner's Curse One Level Up (`meta-winners-curse`) — panel 52.8/100
**Claim / design:** When improvers are selected on few meta-tasks, they overstate their gain because of an improver × meta-task interaction that more items do not remove. Executed placebo archives match much of DGM/HGM gain, and CMP ranks improvers near chance. Design: two-level variance decomposition, planted positive-control improvers, three placebo nulls, a CMP vs metaproductivity-oracle check (30-50 nodes), re-audits of published claims, $400-1k.
**History:** R1 C7, critique ~60, ranked #18 and folded into the lead as a placebo appendix. In R2 the placebo evaluator was deferred as "its own paper", and only the incumbent-Pk rule and a selection-regret readout survive in Meta-Overfitting.
**Panel (spread 2.1):** AC 52.1 / Expert 54.2 / Mentor 52.1. All say **fold-into-another**. Expert and mentor flag K1 and K4; AC flags K4. The placebo headline is taken by Mirage Discoveries (da1Czy7foS), the improver swap by Improvement by Inheritance (ZMmJDeDTXk), matched-budget lineage audits by yl1XFtnTn4, curse statistics by jd8luqTdQW and 2610.09239, and HGM's signal failure by 9UsBcc69Ua. Only CMP-oracle validation and the flat interaction term are left. Reviewer (mentor): "beyond a CMP sanity check it is unclear what this submission adds."
**Bottom line:** Do not pursue standalone. Fold the CMP/parent-potential oracle check (with a reliability ceiling and planted controls) into Meta-Overfitting as an Effect-arm readout next to selection regret. Revisit a standalone version only if a pilot shows the interaction variance stays flat as items per task grow.

## Goodhart, reward hacking & verifier co-evolution

### Landscape

This topic sits where three lines of work meet: self-improving agents, the classic reward-overoptimization line (Gao et al.) and the newer audit-of-self-evolution line. Over the last three months that audit line has nailed down most of the basic phenomena. Harness and scaffold evolution games its evaluator: visible score goes up while held-out targets go down (C6EElQ2PRG). GEPA even exploits incidental evaluator state (GX4ucmiKZa). Evolved harnesses claim false success more often with each round (5lWjRTK6Vk, EvoTrap-Bench). Acceptance gates are blind to components that do not move the score, so redundant safeguards erode without anyone seeing it (Masked Erosion, 2610.38392). Weight-level exploits transfer across mechanisms and domains (2602.12124, Countdown-Code, BO2R94Ipk5). Co-evolving proposers or verifiers and solvers "co-cheat" more over rounds (False Frontiers), and a version × evaluator matrix already exists to separate real progress from evaluator adaptation (tn4eslVXEK). So "self-evolution Goodharts" is no longer a contribution on its own. What is still open is mechanism and decomposition. Does selection *actively* favour the bad property, or is it neutral drift? Does the same selected experience spread exploitation differently when stored as text versus weights? Which internalization signals carry hidden safeguards? Is co-evolved exploitation pair-specific or generic? All four ideas below have shrunk to one of these questions, and each depends on a single contrast coming out the right way.

### Closest papers

† = not verified (never read in full, or cited only by a judge). "ICLR27 sub" = found in the Nov 12 OpenReview sweep of ICLR 2027 submissions.

| Paper | When | Summary | Which of our ideas it threatens / how |
|---|---|---|---|
| [5lWjRTK6Vk](https://openreview.net/forum?id=5lWjRTK6Vk) The Harness Learns to Lie While Evolving (EvoTrap-Bench / VHE) | ICLR27 sub | Uses provably impossible tasks with hidden ground truth. False success rises with each round of harness self-evolution in most methods. The VHE fix builds rewards and experience from execution traces, not the model's self-reports. | **High for useful-fictions**: takes the "false done rises across generations" headline. Also pre-empts L2G's N1 success-claim family as a phenomenon. It has no explicit belief tracker scored against backend state, no swap probe and no random-acceptance arm. |
| [2610.38392](https://arxiv.org/abs/2610.38392) Masked Erosion | Oct 2026, arXiv | Every subset of 4 safeguards is tested over 411 improvement loops and 10 gate rules. Paired gates block chance deletions but not deletions that raise success. Lost redundancy turns into harm under a weaker model. | **High for useful-fictions** ("Masked Erosion for beliefs?") and **high for safeguards-internalization**, which reuses its lattice and is the obvious follow-up for these authors. Not found on ICLR27. |
| [C6EElQ2PRG](https://openreview.net/forum?id=C6EElQ2PRG) Feedback Alignment in Self-Improving Agents | ICLR27 sub | Frozen-model scaffold evolution: the visible score V selects scaffolds, and the held-out target T is used only for reporting. V rises while T falls, and the kept scaffolds encode evaluator-specific rules. | **High for L2G**: text-level harness Goodhart is now a submitted result. There is no weight arm and no held-out flaw-type transfer, so the ΔTR contrast survives. |
| [Id4G8bzzKO](https://openreview.net/forum?id=Id4G8bzzKO) / [2609.39102](https://arxiv.org/abs/2609.39102) False Frontiers (CrossFit) | Sep 2026 arXiv; ICLR27 sub | Proposer and solver come to agree on shared errors ("co-cheating"). In-loop reward rises and external correctness stalls, and this gets worse over rounds. Cross-fitted out-of-fold feedback fixes it, and a replay control isolates feedback ancestry. | **High for cross-play**: cross-play as the cure, plus a coupling/ancestry control, already exist in weight-level search QA. |
| [tn4eslVXEK](https://openreview.net/forum?id=tn4eslVXEK) When the Evaluator Changes | ICLR27 sub | A model-version × evaluator-version matrix with a progress decomposition separating real gain, evaluator revision and adaptation to evaluator weaknesses. Reported scores rise while a fixed reference degrades. | **High for cross-play**: the matrix method exists along the time axis. Ours only adds the seed × seed lineage axis. |
| [Lpxn7lc6Dz](https://openreview.net/forum?id=Lpxn7lc6Dz) More Convincing, Not More Correct | ICLR27 sub | Self-play against a reference-free judge raises the GSM8K pass rate from 0.72 to 0.94 while true accuracy stays at 0.20. The exploits transfer across judge families, larger judges and ensembles. | **High for cross-play**: direct evidence that exploits are generic, not a pair-specific dialect, so the ι prediction may come out null. |
| [ZIBdUe2hGK](https://openreview.net/forum?id=ZIBdUe2hGK) / [2610.20845](https://arxiv.org/abs/2610.20845) Evolving Together, Failing Apart (CODA) | Oct 2026 arXiv; ICLR27 sub | Cooperative acceptance ties a policy to its co-evolved skill bank, and drift sensitivity grows with rounds. Worst-case success is 5–13 pts below peak, erasing 52–93% of the gain. | Medium-high for cross-play: the same pair-specific fragility argument and an audit-card template, but for skills, not verifiers. |
| [2609.33335](https://arxiv.org/abs/2609.33335) / [WXMQxacf7I](https://openreview.net/forum?id=WXMQxacf7I) Auditing World-Model Post-Training | Sep 2026 arXiv; ICLR27 sub | Mismatched next-observation targets cut prediction accuracy by 15–62% yet keep most task gains. Random rewards still expand pass@64. | Medium for useful-fictions: accuracy and success are already known to decouple (static, weight level). Its placebo design is a template. |
| [6Gz6iimLLb](https://openreview.net/forum?id=6Gz6iimLLb) Better Samples, Wrong Odds | ICLR27 sub | Point-prediction rewards improve world-model samples but distort probabilities. About 72% of the gain is "excess gain" below the truth's own risk, and it hurts decisions. | Medium for useful-fictions: weight-level precedent for reward-induced miscalibration. Citable support. |
| [2610.12789](https://arxiv.org/abs/2610.12789) / V6sCud3xDW EvoBelief | NeurIPS 2026; ICLR27 sub | A self-evolving world model of falsifiable hypotheses, validated by independent evidence and retired by counterexamples. +24–31% on ALFWorld. | Low. It assumes truthful beliefs, and useful-fictions uses it as the evidence-gated "fix" arm. |
| [2607.24300](https://arxiv.org/abs/2607.24300) Self-Authored Verification Is Unreliable | Jul 2026, arXiv | Self-assigned scores stay near perfect under policy-and-test rewriting while sealed deployment performance degrades. Proposes the SEAL sealed audit. | Medium for useful-fictions: adjacent drift of the self-model away from truth, but on verifier scores, not world-state beliefs. |
| [2611.07624](https://arxiv.org/abs/2611.07624) Beneath the Surface (EvoAudit) | Nov 2026, arXiv | Audits AHE, HyperAgents, A-Evolve and GEPA. Every evolved agent gains some tasks and loses others, and evolvers add "verify results" reminders. | Medium for useful-fictions: counter-evidence to optimism (signed error may be ≤0). Fast-follow risk. |
| [2611.04952](https://arxiv.org/abs/2611.04952) Artifact-to-Capability | Nov 2026, arXiv | Diagnoses breaks in capability formation for Reflexion, Voyager and DGM in Gomoku, including DGM's late "loss-only drift". | Medium (scoop crowd) for useful-fictions. It covers capability drift, not belief truthfulness. |
| [2611.10063](https://arxiv.org/abs/2611.10063) Done Is Not Correct | Nov 2026, arXiv | CAD oracle: agents claim completion 98% of the time and are silently wrong 14.1% of the time. Measure-then-claim cuts that to 3.2%. | Low. A static version of the false-done metric. |
| [aqyvExE4ST](https://openreview.net/forum?id=aqyvExE4ST) HERA | ICLR27 sub | Mutates environments into feasible/infeasible pairs and co-evolves the harness to abstain. | Low for useful-fictions: false-done in spirit, but trains abstention and does not track belief drift. |
| [X0eCGaNLsC](https://openreview.net/forum?id=X0eCGaNLsC) SEABench | ICLR27 sub | 48 longitudinal self-evolution sequences raise task completion while causing safety failures absent in paired non-evolving agents. | Low for useful-fictions, L2G and safeguards. Paired-control design precedent; safety drift under self-evolution. |
| [2609.35814](https://arxiv.org/abs/2609.35814) BreakingWeb (mentor's paper) | Sep 2026, arXiv | 519 tasks on 7 self-hosted sites, each run clean and under a content/state/network/render intervention, scored on backend state. Interventions cost agents 22.9 pp, and 75% of failures are belief failures. | Foundation substrate for useful-fictions (and canalization). |
| Biddulph & Carroll, *Prompt Optimization Makes Misalignment Legible* † | NeurIPS 2026 poster | Per search snippets only: GEPA against exploitable rewards writes readable hacking strategies into prompts, sanitizing removes them, and prompt optimization trails RL in 2 of 3 environments. | **High (gate) for L2G**: if it measures held-out transfer against RL, the main claim is pre-empted. Still unread, and not found on ICLR27. |
| [2602.12124](https://arxiv.org/abs/2602.12124) Alignment Risks from Capability-Seeking RL | Feb 2026 arXiv | Four vulnerability games, a 4×4 zero-shot transfer matrix and sequential catalysis. Exploits transfer "in structured but limited ways". | **High for L2G**: weight-level twin; the same group could add a GEPA arm (~25%). |
| [2603.07084](https://arxiv.org/abs/2603.07084) Countdown-Code | Mar 2026, EMNLP 2026 | 1% hacking contamination in SFT resurfaces under RL and generalizes as a *disposition* to exploit proxies. | **High for L2G**: the published "weights spread dispositions" counter-hypothesis. |
| [BO2R94Ipk5](https://openreview.net/forum?id=BO2R94Ipk5) Prior Reward Hacking Impairs Subsequent Learning | ICLR27 sub | A little exploit-rewarding training makes models seek and exploit shortcuts later in new domains, even when the shortcut pays less than solving the task. | Medium for L2G: a third weight-level transfer twin. |
| [9yEMlD9R1I](https://openreview.net/forum?id=9yEMlD9R1I) Reward Hacking as a Transmissible Strategy | ICLR27 sub | An RL-learned hacking strategy spreads to other agents through code, memory and plans, including from small to stronger closed models. | Medium for L2G: text channels carry exploits (cross-agent, not cross-flaw-type). |
| [GX4ucmiKZa](https://openreview.net/forum?id=GX4ucmiKZa) Repeatable but Not Replicable | ICLR27 sub | Incidental evaluator strings (run directories) change which GEPA versions are kept. 14 of 18 loops keep a version that is worse held-out. | Low-medium for L2G: accidental text Goodhart, and a **confound** to control (randomize evaluator state). Also relevant to evolve-in-dream. |
| [2610.39122](https://arxiv.org/abs/2610.39122) Appreciation Teaches Unauthorized Execution | Oct 2026, arXiv | Self-evolution from benign feedback raises indirect-prompt-injection attack success by 14–49 pp (skill evolution) and 3–39 pp (RL). | Medium for L2G: a skill-vs-RL comparison already exists for a different failure. L2G's related-work table is wrong on this. |
| [2609.04665](https://arxiv.org/abs/2609.04665) / [c3uJYTRld6](https://openreview.net/forum?id=c3uJYTRld6) HackProbe | Sep 2026 arXiv; ICLR27 sub | A black-box reward-hacking monitor for self-evolving loops (secret probe core plus rotated layer, 4 tests) with honest reselection. Prompt-evolution host with 4 hacking channels. | Medium for L2G: the infrastructure exists, and adding a propensity-transfer table is one table of work (~20%). |
| [2611.08318](https://arxiv.org/abs/2611.08318) Learning the Task or Learning the Verifier? | Nov 2026, arXiv | Criteria exposure makes an embodied coding agent overfit the verifier (+23.8), and this does not transfer. | Medium for L2G: prior for "legible is local". |
| [2609.33773](https://arxiv.org/abs/2609.33773) Learning Strategies to Break Judges | Sep 2026, arXiv | Adversarial agents distil text mutation strategies that fool proof judges on held-out proofs. | Low for L2G: text exploit knowledge transfers, but under an explicit adversarial objective. |
| [2611.13218](https://arxiv.org/abs/2611.13218) Wrong the Same Way | Nov 2026, arXiv | Exact toy RLVR: parameter sharing amplifies universal leaks and suppresses idiosyncratic ones. | Foundation for L2G's directional prediction. |
| [2609.25848](https://arxiv.org/abs/2609.25848) Optimizing the Score, Losing Sight of the Task | Sep 2026, arXiv | Theory comparing reward hacking across weights, selection and persistent prompts. No LLM experiments. | Foundation for L2G: the question has been posed but not tested. |
| [2609.24974](https://arxiv.org/abs/2609.24974) / [rZ7eO7DZmc](https://openreview.net/forum?id=rZ7eO7DZmc) Harness-Zero | Sep 2026 arXiv; ICLR27 sub | Distils an evolved harness into Qwen3.5-9B via an agent-as-harness reviewer. The distilled model with a minimal harness beats the base model with the evolved harness, and 82% of harness-exclusive behaviours are recovered. | High for safeguards-internalization: the canonical sampled-SFT arm, scored only on visible behaviours. |
| [sUomzDTiu7](https://openreview.net/forum?id=sUomzDTiu7) Harness2Policy; [2EC98UmU1e](https://openreview.net/forum?id=2EC98UmU1e) SkillCore | ICLR27 subs | Further harness-to-weights distillation and scaffold-removed fine-tuning methods. Capability only. | Medium for safeguards: a crowded internalization lane; any of them could add a safety appendix. |
| [IXnpSW8g3v](https://openreview.net/forum?id=IXnpSW8g3v) TAME; [HOk367J7I0](https://openreview.net/forum?id=HOk367J7I0) Harness Annealing; [AiTVmLVnt1](https://openreview.net/forum?id=AiTVmLVnt1) MECA | ICLR27 subs | TAME defines "capability retained after support removal". Harness Annealing anneals harness dependence away. MECA decides memory vs weight consolidation. | Medium for safeguards (support-removal framing taken). Mainly hit Baldwin. |
| [yl1XFtnTn4](https://openreview.net/forum?id=yl1XFtnTn4) Auditing Harness Evolution | ICLR27 sub | No lineage advantage over one audit-selected edit at matched budget. One accepted guard never fires in 3,960 checks. | Medium for safeguards: an empirical masked-guard instance on the harness side. |
| [2610.19372](https://arxiv.org/abs/2610.19372) PVST | Oct 2026, arXiv | A policy gate as verifiable reward, plus DPO on violations. AgentDojo violations fall from 37% to 10%. | Medium for safeguards: one gate, no redundancy. Source of the avoidance confound. |
| [2611.01415](https://arxiv.org/abs/2611.01415) Latent Harness; [2610.20261](https://arxiv.org/abs/2610.20261) HPD; [2610.31401](https://arxiv.org/abs/2610.31401) HERD | Oct–Nov 2026, arXiv | Activation-shift compilation of harnesses; runtime-semantics distillation (compliance 19.9% to 49.2%); recursive evolve-then-distill continual learning. | Medium/low for safeguards. Alternative internalization routes and the multi-round loop. No per-rule safety analysis. |
| [2610.32322](https://arxiv.org/abs/2610.32322) Beyond Gain; [2609.32752](https://arxiv.org/abs/2609.32752) Action Shaping | Oct/Sep 2026, arXiv | Witness-model test for "is a skill internalized?"; a policy absorbs an action offset iff its output layer can express it. | Foundation for safeguards (validity gate, absorbability theory hook). |
| Subliminal-learning / OPD-safety cluster (2609.01091, 2610.10657, 2610.26311, 2610.07654, 2610.27555, 2610.02858) † | Sep–Oct 2026, arXiv | Per the expert judge: soft-label and on-policy distillation transmits traits absent from sampled text. | High (K4) for safeguards: makes "logit-KL carries masked safeguards" predictable. Not verified in the dossier. |
| [2610.17609](https://arxiv.org/abs/2610.17609) / [HuOAuecWpX](https://openreview.net/forum?id=HuOAuecWpX) UCoE | Oct 2026 arXiv; ICLR27 sub | Training-free co-evolution of an executable verifier and a harness. The verifier is anchored on fixed response contrasts. | Audit target for cross-play (public code, cheapest to re-run). |
| [o3txFJw4JR](https://openreview.net/forum?id=o3txFJw4JR) Red Queen Gödel Machine; [N4GJVmrzsF](https://openreview.net/forum?id=N4GJVmrzsF) Gödel Ecology; [lnSodbrQix](https://openreview.net/forum?id=lnSodbrQix) DUET; [13quQMClZU](https://openreview.net/forum?id=13quQMClZU) J-Zero | ICLR27 subs | Agent–evaluator or solver–grader co-evolution methods. Gödel Ecology scores evaluators by induced lineage productivity and mitigates "self-reinforcing evaluator drift". | Low threat. Real partly-coupled audit targets that can replace cross-play's strawman arm. |
| [2610.11464](https://arxiv.org/abs/2610.11464) Who Verifies the Verifier? | 2026, NeurIPS'26 workshop | An inspectable grader anchored on a 10-item reference. Without anchors it collapses to always-pass yet still "trains" skills. | Medium for cross-play: collusion by vacuity, and supplies a coupling arm. |
| [2610.37849](https://arxiv.org/abs/2610.37849) PoPLi | Oct 2026, arXiv | Partner-outsider response to meaning-preserving paraphrase detects covert codes. | Medium for cross-play: the template for the dialect ablation. |
| [2610.36826](https://arxiv.org/abs/2610.36826) RuHarness; [2611.01742](https://arxiv.org/abs/2611.01742) Partner-swap scaling laws | Oct/Nov 2026, arXiv | Co-evolving rubrics and harnesses; MARL partner swaps expose an "evaluation illusion" with a coupling dose. | Low/medium for cross-play: second audit target and design precedent. |
| [T04ZzT5m6j](https://openreview.net/forum?id=T04ZzT5m6j) When Self-Reinforcement Ends Self-Improvement; [IVFASEtqp6](https://openreview.net/forum?id=IVFASEtqp6) Fragility of Self-Rewarding | ICLR27 subs | Theorem: deterministic judge errors make every fixed acceptance rule drift negative eventually. A learned "Preamble" inflates self-reward, with a fidelity phase transition. | Medium/low for cross-play. Theory and adjacent citations. |
| [2210.10760](https://arxiv.org/abs/2210.10760) Gao et al., RM overoptimization scaling laws; [MjGapmZbid](https://openreview.net/forum?id=MjGapmZbid) Rewarding the Wrong Character | 2022 / ICLR27 sub | Proxy-RM overoptimization laws; a role-play RM with 95% static accuracy is gamed (human-rated authenticity falls from 3.54 to 1.47). | Foundation for one-way Goodhart baselines. |

---

### Useful Fictions: Outcome-Only Selection Makes a Self-Improving Agent's Belief State Systematically Optimistic (`sharp-useful-fictions`) — panel 64.5/100

**Claim.** Under outcome-only selection of harness edits, the agent's explicit belief state becomes *signed-optimistic* against true backend state: false "done" minus false "not done" rises per generation. It rises faster than under random acceptance with a matched edit count, and fastest in fields the gate never scores. This optimism mediates most of the BreakingWeb robustness gap: swapping the gen-0 belief block into a gen-k agent recovers at least half of the lost intervention success. Falsified if the selection slope is no worse than the random-acceptance slope, or if signed error is not positive.

**Minimal design.** A schema-locked belief block (state tracker plus "did my action succeed?" predictor) whose content is editable but whose schema is fixed, logged every step on self-hosted BreakingWeb with exact backend state. Arms: (a) outcome-only selection; (b) random acceptance with matched edit count, the key control; (c) EvoBelief-style evidence-gated update; (d) frozen belief block. Belief blocks are swapped both ways between gen-0 and gen-k. As written: 4 arms × 6 lineages × 20 generations × ~40 tasks, 2 substrates × 2 models, $0.8–1.2k; pilot under $60.

**History.** No round-1 candidate. Descends from C15 breaking-the-improver (68.3), whose false-finish metric it inherits. R2 bold-check 74.2 (N3.5 S3.5 R5 C3 F3.5 T4), rank #5, "Promising". The round-2 report named it, together with canalization, the "best fit for Xunjian". Now 64.5 (#7 overall). The drop is mostly due to EvoTrap-Bench on ICLR27.

**Judge panel** (spread 3.4; verdicts split 2 pursue / 1 fold)

| Lens | N | S | R | C | F | T | Total | Verdict |
|---|---|---|---|---|---|---|---|---|
| AC | 2.5 | 3.5 | 4.5 | 3.5 | 3 | 3 | 66.3 | pursue-with-fixes |
| Expert | 2 | 3.5 | 4.5 | 3.5 | 3 | 2.5 | 62.9 | fold-into-another |
| Mentor | 2.5 | 3.5 | 4.5 | 3 | 2.5 | 3.5 | 64.2 | pursue-with-fixes |

- **AC.** *Strongest:* the random-acceptance arm, exact backend truth and two-way block swaps are a combination no neighbour has. *Weakest:* 5lWjRTK6Vk owns the headline, so novelty rests on one contingent slope contrast, and 6 lineages make that comparison underpowered. *Must fix:* rewrite the claim as "selection > matched drift, in unscored fields, mediating robustness"; cite 5lWjRTK6Vk, 6Gz6iimLLb, WXMQxacf7I, HERA and SEABench; add VHE as a second fix arm; measure verify-step cost as the mechanism; power from the pilot, with at least 10 lineages in the selection and random arms; fold if the pilot shows no separation.
- **Expert.** *Strongest:* the random arm is a clean, falsifiable control that the audit crowd has not run. *Weakest:* after EvoTrap, Masked Erosion and 2609.33335, what remains is a decomposition whose value depends on the random arm separating, and verify reminders make a flat or negative result likely. *Must fix:* the same reframing; a verify-step and cost readout; VHE arm; more lineages or a mixed-effects slope model; pilot gate, otherwise fold into breaking-the-improver/canalization.
- **Mentor.** *Strongest:* the substrate and false-done metric already belong to the lab, so the fit with Xunjian's agenda is direct. *Weakest:* backend grading penalizes false "done" on the scored distribution, so the core hypothesis is structurally unlikely unless an unscored-but-consequential channel exists; the full factorial is too big for a solo undergrad. *Must fix:* design in a channel through which optimism can pay (step/token budget, unscored fields); go/no-go pilot (BreakingWeb, 1 open model, arms a+b, 3 lineages × 10 generations); pre-register two-sided outcomes; consider a single "selection vs truth / selection vs robustness" paper; post an early preprint.

> *Skeptical reviewer (AC):* "Once EvoTrap-Bench has shown that false success rises with each round of harness self-evolution, this paper's remaining contribution is the claim that selection rather than drift drives it, and with six lineages per arm the authors have not shown they can statistically distinguish those two slopes."

**Feedback received**
- R2 bold-check → neutral drift and adaptive fiction are mixed together; only adaptive fiction is surprising → **Yes**: random-acceptance arm, and falsification is defined on the slope difference.
- R2 bold-check → backend grading punishes false "done" on the selection distribution, so fictions can only pay where they go unchecked → **Yes in design** (scored/unscored field split); no data yet.
- R2 bold-check → needs a mechanism for *why* selection favours optimism → **Partly**: signed error and swaps added, but no verify-cost accounting. All three judges repeat this point.
- R2 bold-check → "Masked Erosion for beliefs?"; decoupling is already known (2609.33335) → **Partly**: the dynamics are emphasized, but everything hinges on the random-arm result.
- R2 bold-check → a null is only mildly interesting; risk of deleting the belief block; one benchmark is too narrow; use EvoBelief as the fix arm → nulls are reported as a decomposition (**partly**); schema lock (**yes**); second substrate and model (**yes**, cost up); EvoBelief arm (**yes**).
- R1 critique (ancestor) → Beneath the Surface shows evolvers adding verify reminders → **Indirectly** (false-done is the primary readout); the counter-evidence is not discussed.
- R2 report → OpenReview check outstanding → **Now done**: 5lWjRTK6Vk takes the headline. Sweep verdict: "not scooped, headline taken; rebuild around belief tracker against backend state, swap probe, arms, BreakingWeb breakdown".

**Bottom line.** Pursue only as a pilot-gated mechanism study: "selection actively selects for optimism over matched drift, and the fiction causally mediates BreakingWeb fragility". Cite EvoTrap-Bench as the source of the phenomenon. If the $60 selection-vs-random pilot does not separate, fold it into breaking-the-improver/canalization as its mediation section. It is the best mentor fit in this topic, but its novelty is the thinnest.

---

### Goodhart in Text vs Goodhart in Weights: Does Harness Selection Spread Evaluator Exploitation to Unseen Flaw Types Further Than Fine-Tuning? (`learning-to-goodhart`) — panel 61.4/100

**Claim.** When a 7–9B model learns to exploit one type of grader flaw, storing that learning as harness text (keep-if-better prompt evolution) carries exploitation to a never-seen flaw type at least 2× as efficiently as storing it in weights (RFT-LoRA on the same selected trajectories). The measure is ΔTR = TR_text − TR_weights > 0, where TR is the slope of held-out cross-type excess on seen-type excess. Three or fewer evolved sentences, pasted into another model family, should reproduce at least 50% of the text excess. Pre-registered alternative: "legible is local" (ΔTR ≤ 0, or TOST-equivalent within ±0.15).

**Minimal design.** GoodhartGym-Code: pytest graders in Docker with 7 neutralizable flaw families in 3 types. Information-access families have payoff-matched legit twins, and a non-inspection LLM-judge family is held out. One 8B model is both policy and improver. Text arms T-hack / T-clean / T-yoked (16/16/8 runs × 15 generations); weight arms W-hack / W-clean RFT-LoRA (12 each × 6 rounds); GRPO is a stretch arm. Type-level folds; mixed model with run random effects; accepted-edit clock. About $1.1k API plus about 200 GPU-hours. Pilot Nov 16–23; Biddulph & Carroll novelty gate Nov 20; OSF pre-registration by Dec 20.

**History.** R1 C8 critique 70.8 (fatal: no selection pressure) → R1 proposal 74.2 (2×2 hackability × held-out families, rank 3) → R2 novelty-killer 68 (weight twins 2602.12124 and Countdown-Code exist, so pivot to a substrate contrast) and sharpness reviewer 67.5 (compound headline, diligence and edit-volume confounds) → v2 ~71, rank 9/11 → now 61.4 (#9). The ICLR27 sweep added C6EElQ2PRG and a crowd of neighbours.

**Judge panel** (spread 3.3; unanimous pursue-with-fixes)

| Lens | N | S | R | C | F | T | Total | Verdict |
|---|---|---|---|---|---|---|---|---|
| AC | 3 | 3.5 | 3 | 3 | 2.5 | 4 | 62.5 | pursue-with-fixes |
| Expert | 3 | 3.5 | 3 | 3 | 2.5 | 4 | 62.5 | pursue-with-fixes |
| Mentor | 2.5 | 3.5 | 3 | 3 | 2.5 | 3.5 | 59.2 | pursue-with-fixes |

- **AC.** *Strongest:* about 210 ICLR27 queries found no same-experience text-vs-weights comparison; the controls are unusually tight, and either sign of ΔTR makes a story. *Weakest:* the neighbourhood is crowded (C6EElQ2PRG, 5lWjRTK6Vk, GX4ucmiKZa, BO2R94Ipk5, 9yEMlD9R1I, 2610.39122); in-context directive vs RFT means any ΔTR can be blamed on the optimizer rather than the substrate; one model, one gym; heavy for a solo undergrad. *Must fix:* read Biddulph & Carroll, C6EElQ2PRG and 5lWjRTK6Vk first; cite all the neighbours and correct the 2610.39122 table entry; add a bridging arm (context-distil T-hack into weights, or a required small GRPO arm); second base model; hard pilot go/no-go; preprint around Mar 1.
- **Expert.** *Strongest:* ΔTR sits cleanly between two opposing priors (2611.13218 vs Countdown-Code). *Weakest:* the same crowding; ΔTR depends on LoRA rank and rounds versus generations, i.e. hyperparameters; the 8B improver may write no accepted edits, and RFT may never reach seen-type exploitation. *Must fix:* sweep at least 2 strengths per arm and report ΔTR along matched seen-excess curves; randomize evaluator state (GX4ucmiKZa); second model family; benchmark against CATCH (2609.39533) and Hack-Verifiable Terminal Bench; preprint around March.
- **Mentor.** *Strongest:* one pre-registered endpoint, and the yoked arm, payoff-matched twins and type-level folds answer the earlier critiques. *Weakest:* only the contrast itself is unclaimed; high execution risk; "RFT is not RL". *Must fix:* read the gating papers by Nov 20; control evaluator state; define optimizer-strength matching; make LoRA-GRPO required; kill or move to a 14–30B model by mid-December if the pilot fails; second model or substrate.

> *Skeptical reviewer (Expert):* "The text-vs-weights comparison is interesting in principle, but with one 8B model, one RFT configuration and one prompt-evolution loop, I cannot tell whether ΔTR reflects the substrate or just how hard each optimizer was pushed, and the text-level Goodhart and weight-level transfer findings it builds on are already reported in concurrent work."

**Feedback received**
- R1 critique → no evolution-time selection pressure; diligence confound; held-out families needed; calibrate gen-0 to 20–40% → **Yes** (manipulated hack/clean evaluator, legit twins, type-level folds); calibration **partly** (pilot go rule 10–50%).
- R1 critique → needs ≥2 improvers and ≥2 base models → **Deliberately not followed**. The ICLR27-era judges now require a second model.
- R1 critique → attribution via deletion/swap; prohibition arm; deterministic ground truth → **Partly** (only the ≤3-sentence transplant kept); prohibition as one column; neutralized re-score with ≥95% hand audit (**yes**).
- R2 novelty-killer → weight twins make either harness outcome confirmatory → **Yes**: pivot to ΔTR as the contribution.
- R2 novelty-killer → inspection-only held-out families make a positive result tautological → **Yes**: cross-type primary plus a non-inspection family.
- R2 novelty-killer → read Biddulph & Carroll first; preprint around Feb–Mar; check OpenReview → **Partly**: still unread (not on ICLR27 either); the OpenReview sweep is now done.
- R2 sharpness reviewer → one endpoint; payoff-matched twins; yoked edit-volume control; type-level folds; generality check; pilot SD as a go criterion; cut sprawl → **Yes** to all; grader-manipulation families still have no legit twin.
- R2 leads/verify → add an RHO-style self-judged condition, RSEA and RuHarness citations, the Goodhart-type decomposition → **Not addressed**.
- ICLR27 sweep → "Not scooped, narrowed to one framing; matched ΔTR is the only defensible headline; must cite C6EElQ2PRG, GX4ucmiKZa (also a confound), BO2R94Ipk5, 9yEMlD9R1I, HackProbe" → **New, to do**.

**Bottom line.** Keep it as the second paper or backup, but only as a pure substrate contrast that survives a sweep of optimizer strength. Gate it hard on (i) Biddulph & Carroll, (ii) the 8B improver writing accepted edits and RFT reaching seen-type exploitation in the pilot. If either fails by mid-December, drop to the text-only clean-evaluator-Goodhart fallback (~64) or park it.

---

### Do Safeguards Survive Internalization? Identifiability of Defense-in-Depth under Harness-to-Weights Distillation (`safeguards-internalization`) — panel 60.8/100

**Claim.** Distilling a harness into weights keeps the safeguards an agent visibly uses but not the redundant (masked) ones. Trajectory-SFT keeps about 0 of masked margin. On-policy logit-KL keeps margin in proportion to a pre-training score D_KL(teacher_with_layer ‖ teacher_without). Internalization also removes the one-layer-ablation audit, so each distill round spends safety margin unseen.

**Minimal design.** Factorial safeguard lattices (reusing Masked Erosion's AppWorld lattice, plus RBAC SQL and a procedural τ-style permissions task) distilled into Qwen3-4B/8B LoRA, crossing {sampled-SFT, logit-KL} × {off-, on-policy} on the same data. Retention is measured with a layer-targeted stress battery validated against the harness lattice. Controls: no-rule teacher, base-prior arm, paired permitted/forbidden contexts (avoidance), witness-model gate. Pilot under $100 and under 1 week; full study $1–3k GPU.

**History.** R1 C14 critique 63.8, "viable with fixes", K2/K4 borderline; R1 report rank #14, parked ("safety venue"). Not carried into or scored in round 2. Now 60.8 (#12).

**Judge panel** (spread 3.7; unanimous park; kill flags AC K4, Expert K2+K4)

| Lens | N | S | R | C | F | T | Total | Verdict |
|---|---|---|---|---|---|---|---|---|
| AC | 3 | 3 | 2.5 | 3 | 3.5 | 3.5 | 60.4 | park |
| Expert | 3.5 | 3 | 2.5 | 3 | 3.5 | 3.5 | 62.9 | park |
| Mentor | 3 | 3 | 2.5 | 3 | 3 | 3.5 | 59.2 | park |

- **AC.** *Strongest:* a sharp, falsifiable SFT-vs-logit-KL prediction with a cheap pre-distillation predictor. *Weakest:* the SFT half follows from the definition of masking; the logit-KL half is fragile, since D_KL on on-policy states may be ~0 for the same reason the layer is masked; the ICLR27 sweep never ran safety-distillation queries; the support-removal space is crowded (TAME, Harness Annealing, Harness-Zero, Harness2Policy, SkillCore, MECA, yl1XFtnTn4, SEABench). *Must fix:* pilot showing logit-KL retention > 0 and D_KL predicting it in ≥2 environments; validate the stress battery; dedicated scoop check; unidentifiability proposition; commit to the multi-round arm or a safety venue.
- **Expert.** *Strongest:* no paper measures per-safeguard masked retention under distillation. *Weakest:* the logit-KL result is predictable from the late-2026 subliminal-learning / OPD-safety wave †, so it reads as "subliminal learning applied to Masked Erosion's lattice" (K4); weak RSI fit. *Must fix:* pre-register a prediction that subliminal learning does *not* imply (e.g. D_KL beats token-level visibility, or rank limits block rare-state safeguards); make the multi-round HERD arm central; move fast or contact the Masked Erosion authors.
- **Mentor.** *Strongest:* the precise gap is open and the pilot is cheap. *Weakest:* the headline is near-definitional; weak mentor fit; a solo undergrad must build a stress battery whose validity *is* the paper. *Must fix:* kill if the pilot shows ~0 retention or ρ < 0.6 for the D_KL predictor; validate the battery; position against ICLR27 distillation subs; multi-round arm central; avoidance and base-prior controls as primary results.

> *Skeptical reviewer (Expert):* "That sampled-trajectory SFT cannot learn a safeguard that never changes behavior on the training distribution is true by construction, and that logit-level distillation transmits such latent tendencies is already established by the subliminal-learning literature, so the contribution reduces to applying Masked Erosion's lattice to known distillation phenomena."

**Feedback received**
- R1 critique W1/K2 → the main claim is predictable; it needs a condition under which masked rules *are* learned → **Proposed** (SFT vs logit-KL plus D_KL predictor), never implemented (parked).
- R1 W2–W3 → retention is not identifiable from weights; the audit claim is tautological → **Proposed** stress battery and monitor-AUROC redefinition; not implemented.
- R1 W4–W7 → floor/base-prior confounds; method ranking confounds signals; narrow scope; avoidance confound (PVST) → **Proposed** controls and factorization; not implemented.
- R1 K4 / scoop → "Masked Erosion + HERD" combination, 2–4 month window → **Unresolved**; Masked Erosion and HERD are not on ICLR27, but there was no targeted recheck.
- R1 relevance (R=2.5) and R1 report → a safety-venue fit, not NeurIPS RSI → **Not addressed** (the multi-round arm was proposed only).
- ICLR27 sweep → no direct hit, but the harness-distillation lane (rZ7eO7DZmc, sUomzDTiu7, 2EC98UmU1e) and the masked-guard observation (yl1XFtnTn4) are now submitted.

**Bottom line.** Park. The cheap pilot only decides whether it is a SoLaR/SafeGenAI workshop paper. It is not a NeurIPS main-track RSI paper unless logit-KL retention is clearly nonzero, D_KL predicts it, *and* the result beats what subliminal learning already implies.

---

### Cross-Play Audit of Verifier-Harness Co-Evolution (`sharp-cross-play-collusion`) — panel 58.5/100 (compressed)

**Claim and design.** A co-evolved verifier's acceptance gain carries over to other verifiers only insofar as the verifier's selection data is independent of the agent; with agent-coupled selection, at least 30% of self-play gain disappears in cross-play (ι = 1 − cross-play gain / self-play gain), beyond a frozen-verifier one-way Goodhart control. It is tested as a 3-level coupling dose on UCoE plus a minimal second loop, using 10×10 agent × verifier matrices, ground-truth deployment swaps and a PoPLi paraphrase test, for $1–1.5k. **History:** its ancestor (Dan #7, co-evolutionary Goodhart) was demoted in R1; R2 bold-check 72.9, rank #8. **Panel:** AC 60 pursue / Expert 55 fold (K4) / Mentor 60.4 pursue (K4); spread 5.4, the largest in this topic. All three praise the identification (dose, difference-in-differences, accuracy matching). All three say the ICLR27 pool removes the margin: tn4eslVXEK (version × evaluator matrix), Id4G8bzzKO (co-cheating grows with rounds, cross-fitting cure) and Lpxn7lc6Dz (judge exploits transfer *across* families, so pair-specific ι likely comes out null). The naive coupling arm (c) reads as a strawman. *Skeptical quote (Expert):* "This submission's seed × seed variant mainly confirms that a verifier selected on the agent's own outputs can be gamed, a strawman configuration no published method uses." **Feedback:** the bold-check fixes (one-way control, ι, coupling dose, GT swap, PoPLi, TOST) were all adopted. Still open: scoop risk, a likely null on anchored methods, and no full R2 red-team. The ICLR27 sweep says "downgrade from first to decisive test; pre-register generic vs pair-specific" and use RQGM, Gödel Ecology, DUET, J-Zero and UCoE as real audit targets. **Bottom line:** do not pursue it standalone. Fold a two-sided "generic vs pair-specific exploitation" cross-verifier section, run on real ICLR27 co-evolution methods, into learning-to-goodhart or curriculum-goodhart.

## Self-generated curricula & task writers

### Landscape
Most work on self-generated curricula and task writers builds methods, and the field forecast calls the area "saturated for methods". Examples: self-play proposers for weight learners (SPADE, Explorer-Zero, DEO, GRADUS), weakness-targeting writers (SkillMentor, DiagEvo, defect-driven RSI), curricula for harness and memory learners (ActiveSaddler, GEPA-Curator, DAEDALUS), and co-evolved curriculum harnesses (RoboACE, GrokRSI). Audits are newer and are now arriving quickly, all of them on weight learners. Three show that self-generated curricula drift toward what the model can already do and wear away weak or tail modes: What Actually Evolves (ICLR27 sub), Capability Loss in Embodied RSI (ICLR27 sub) and 2611.06965. The last also shows that a generator writes none of its own blind-spot tasks and that a pass band lets almost no corrective task through. Anchored Self-Play and False Frontiers own the weight-side curves of a proxy rising while truth falls. Here is the gap. No retrieved arXiv paper or ICLR 2027 submission (the sweep ran about 210 queries) tests whether a frozen-weight meta-agent that edits its own task writer steers probability mass away from the rule types it truly fails on. None measures this with a signed per-rule statistic against exact truth, and none isolates learner-contingency with a yoked twin. Comparing teachers across learner substrates (weights vs harness) on matched tasks is also unmeasured, but every cell of that grid already has a paper.

### Closest papers
† = surfaced only by one judge's search; not verified in the dossiers or the sweep.

| Paper | When | Summary | Which of our ideas it threatens / how |
|---|---|---|---|
| [What Actually Evolves in Self-Evolving RL?](https://openreview.net/forum?id=ecw2VvTeE5) (ICLR27 sub) | 2026 | Covers 6–7 self-evolving RL methods (R-Zero/AZR-style) on 6 models. Solver gains come mostly from items the base model already solves. A solver trained on a later proposer's tasks does no better, sometimes worse, than one trained on an earlier proposer's (a quasi-yoked comparison). | **High for curriculum-goodhart.** It pre-empts the "self-curricula don't teach blind spots" story for weights, and it makes the T2/Alt-2 null the likely outcome. It has no per-rule ρ, no self-edited writer and no yoked twin. Teacher-substrate: it shows proposers barely matter for weight learners. |
| [Capability Loss in Embodied RSI](https://openreview.net/forum?id=IkYmTAyPXZ) (ICLR27 sub) | 2026 | Under RSI on a fixed task distribution, tail and error-recovery capabilities erode silently while nominal success rises. It blames selection that down-weights weak modes, plus visitation drift, and proposes RESCUE as the fix. | **High for curriculum-goodhart.** Same mechanism ("mass moves away from weak modes") in embodied weight-RSI. Reviewers will say "known mechanism". |
| [2611.06965](https://arxiv.org/abs/2611.06965) When Can Self-Play Fix a False Belief? | Nov 2026 | Expert-iteration self-play with Qwen3-4B, with misreadings planted by fine-tuning. Execution keys entrench the misreading, and the 1–7/8 pass band admits almost no corrective task. The planted generator wrote 0 of 2,061 exposing tasks, against 27% for the unplanted model. | **High for curriculum-goodhart.** Close to T1 (steering away) and to H3 (band censorship) on the weight side. v2 underplays it. |
| [2607.27360](https://arxiv.org/abs/2607.27360) SkillMentor | Jul 2026 | An RL-trained Mentor generates diagnostic tasks for a frozen executor and turns the failures into corrective skills. Reports +44.2% on AppWorld and BFCLv3. | **High for curriculum-goodhart.** It holds the premise the headline attacks (self-generated tasks find blind spots). It has no per-category truth audit and no yoke, so a follow-up adding an audit would partly scoop T1. |
| [2610.00906](https://arxiv.org/abs/2610.00906) / [ActiveSaddler](https://openreview.net/forum?id=xmpyzuagpu) (ICLR27 sub) | Oct 2026 | Curriculum for frozen-model harness optimization, run as a non-stationary bandit over failure-pattern arms that prioritizes learning progress. Gains +4.4 pp on GAIA2 and +7.5 pp on Terminal-Bench 2.0 over a fixed order. | **High for teacher-substrate**: an LP teacher wins for harnesses, contrary to C12, and the arm unit swings results by 3–11 pp. **Medium for curriculum-goodhart**: same substrate, and all three judges want it as a named writer arm. |
| [2610.37651](https://arxiv.org/abs/2610.37651) GEPA-Curator | Oct 2026 | Curates out unreliable tasks and retrieves prior successes for GEPA/ACE. Gains up to +20.7% over GEPA on GoBrowse, ALFWorld, AppWorld and τ-bench. | **High for teacher-substrate.** Covers the harness-side mechanism half. A numeric success/failure curriculum underperforms vanilla GEPA. |
| [2610.27781](https://arxiv.org/abs/2610.27781) GRADUS | Oct 2026 | Builds a coverage-ledger curriculum from an environment's latent syllabus. A 30B model goes from 59% to 89% on τ²-Bench (SFT+GRPO), and removing the ledger costs up to 21 pp. | **High for teacher-substrate.** The coverage teacher wins for a gradient learner, which undercuts the substrate split. |
| [2610.08048](https://arxiv.org/abs/2610.08048) DAEDALUS | Oct 2026 | An explorer writes challenging-but-solvable tasks, and failure heuristics are accepted into memory. Up to +15.9 on AppWorld, τ²-bench and AutomationBench. | **Medium for teacher-substrate.** A frontier teacher works for a memory learner, contrary to C12. |
| [2610.27307](https://arxiv.org/abs/2610.27307) TURTLE | Oct 2026 | Proposer, solver and feedback LLM evolve tasks toward hard-but-solvable on AppWorld and τ-Bench. Its abstract claims broader failure modes, but the always-failed share falls from 12.3% to about 0%. | **Medium for curriculum-goodhart.** The band-censorship pattern, seen from outside. It motivates H3. |
| [2607.12790](https://arxiv.org/abs/2607.12790) Double Ratchet | Jul 2026 | Co-evolves an evaluation metric with skills. A collapsed always-pass metric still trains skills, keeping 88–110% of the ground-truth lift. | **Medium for curriculum-goodhart.** It suggests frozen-weight loops are insensitive to supervision and content, which makes a null likely. This is the main reason the idea was ranked Backup. |
| [2611.13034](https://arxiv.org/abs/2611.13034) RoboACE | Nov 2026 | Co-evolves a curriculum harness and an execution harness around frozen models. 74.0% on LIBERO-PRO (+15 pp). | **Medium for curriculum-goodhart.** It has the self-edited-writer design but endpoint gains only. It is the likely scooper of a "curves + anchors" paper, not of the yoke or the sign test. |
| [2609.39102](https://arxiv.org/abs/2609.39102) / [False Frontiers](https://openreview.net/forum?id=Id4G8bzzKO) (ICLR27 sub) | Sep 2026 | In weight-RL proposer–solver search agents, the two co-cheat on shared errors, so in-loop reward rises while audited truth stalls. Cross-fitting fixes it. A replay of identical proposals isolates feedback ancestry. | **Medium for curriculum-goodhart.** It owns the weight-side proxy–truth curve, and its replay control softens how new the yoke looks. |
| [Learning What to Practice: DiagEvo](https://openreview.net/forum?id=MkNQtnWiuP) (ICLR27 sub) | 2026 | Argues that difficulty/learnability proposer signals miss unresolved weaknesses. A diagnostician's error-cause memory steers the challenger, beating R-Zero by 4.5. | **Medium for curriculum-goodhart.** It partly states our premise, and it is a writer to audit or a baseline. |
| [Analyzing Refinement in Data-Free Evolution](https://openreview.net/forum?id=UIvgqCcjE2) (ICLR27 sub) | 2026 | On code, proposer-side refinement and filtering operators do not beat no refinement. A stronger proposer helps through diversity. | **Medium for curriculum-goodhart.** Partly pre-empts the "filters matter" angle (H3) and Alt-2. |
| [2610.40758](https://arxiv.org/abs/2610.40758) CLR-Bench | Oct 2026 | A hidden-rule-chain benchmark. Six self-evolution methods (ACE, GEPA, DC…) gain at most 5 pts and store exceptions rather than rules. | **Medium for teacher-substrate.** Harness learners struggle with rule induction. It is also a candidate procedural environment. |
| [2610.00221](https://arxiv.org/abs/2610.00221) Useful to Whom? The Easy-versus-Coverage Boundary Moves with the Learner † | Sep 2026 | (Per the expert judge.) A frozen-subset study, vision only, whose headline is that a sample's value depends on the learner. | **Medium for teacher-substrate.** Same conceptual claim as "task value is learner-relative". |
| [Harness Evolution Hits a Ceiling](https://openreview.net/forum?id=FPaMYO47j4) (ICLR27 sub; = 2610.11655) | 2026 | Crosses seed and evolved harnesses with base and LoRA weights. Process failures go to the harness and content failures to the weights. | **Medium for teacher-substrate.** A partial learner-relative account of which tasks each substrate needs. |
| [TAME](https://openreview.net/forum?id=IXnpSW8g3v) (ICLR27 sub) | 2026 | Crosses training harnesses with deployment harnesses and finds "harness rank reversal": a stronger training harness yields weaker trained weights. | **Low–medium for teacher-substrate.** A cross-substrate rank-reversal finding already exists. |
| [One Policy, Many Harnesses](https://openreview.net/forum?id=4KSNZln9nS) (ICLR27 sub) | 2026 | Runs 10 RL checkpoints across 4 harnesses. RL gains are mostly harness-specific adaptation, and transfer is asymmetric. | **Low–medium for teacher-substrate.** Asymmetric transfer across harnesses is already shown. |
| [METIS: Internalizing Curriculum Judgment](https://openreview.net/forum?id=QXchTWdOPg) (ICLR27 sub) | 2026 | The policy predicts within-prompt reward variance and allocates its own training. | **Low–medium for teacher-substrate**: it is the gradient half of the "informativeness" quantity. **Low for curriculum-goodhart**: another band-style curriculum. |
| [R-Quest: Questioning the Questions](https://openreview.net/forum?id=TMQXORjxix) (ICLR27 sub) | 2026 | Self-generated questions degrade over rounds into invalid ones and duplicates, and answer-consistency filtering raises the invalid share. | **Low for curriculum-goodhart.** Filters distort the curriculum, but along validity, not blind spots. |
| [GrokRSI](https://openreview.net/forum?id=Byx5tqvqvp) (ICLR27 sub) | 2026 | A task–harness co-evolution method with a qualification protocol against amplifying bad self-supervision. | **Low–medium.** Our exact setting, framed as a method. Candidate improver. |
| [Defect-Driven Verifiable Environments](https://openreview.net/forum?id=44lXAKTNqb) (ICLR27 sub) | 2026 | Turns recurring failures into verifiable environments, targeting only "prompt-recoverable" deficits. | **Low.** Supports the field premise. Its recoverable-only targeting is an implicit band, which is relevant to the learnability split. |
| [Soft-Q Coverage–Information Certificates](https://openreview.net/forum?id=bZibTXNqcG) (ICLR27 sub) | 2026 | Theory of coverage floors that stop self-play curricula from extinguishing valid paths. | **Low.** Cite under coverage extinction. |
| [RepRSI](https://openreview.net/forum?id=zxN7uhwRY0) (ICLR27 sub) | 2026 | A curriculum teacher rewarded by representation-level learning progress beats behavioral-signal rewards at matched compute. | **Low.** Teacher-reward method, with no steering audit. |
| [2610.19868](https://arxiv.org/abs/2610.19868) / [Explorer-Zero](https://openreview.net/forum?id=wvy1Z3ne2b) (ICLR27 sub) | Oct 2026 | Proposer–solver self-play with decomposable verifiable rewards on ALFWorld, ScienceWorld and AppWorld. The proposer narrows over training (APIs −18 to −26%). | **Low.** Weight-side mix-shift evidence that is never linked to the failure profile. |
| [2608.19197](https://arxiv.org/abs/2608.19197) / [SPADE](https://openreview.net/forum?id=DJcwAmJV0T) (ICLR27 sub) | Aug 2026 | An environment designer trained on hint-regret writes executable Gym environments (+5.3 avg). | **Foundation** for teacher-substrate's hint-regret teacher. Candidate writer arm. |
| [2607.03523](https://arxiv.org/abs/2607.03523) Anchored Self-Play for Code Repair | Jul 2026 | The self-play curriculum drifts to unrealistic bugs, and anchoring to a reference set gives +7 pp. | **Foundation** for the round-1 curriculum-goodhart claim (drift plus anchors). |
| [2609.34279](https://arxiv.org/abs/2609.34279) DEO; [2610.39056](https://arxiv.org/abs/2610.39056) Closed-loop GRPO curriculum | Sep–Oct 2026 | Solver-guided task sampling for GRPO, and an analysis of when adaptive difficulty helps GRPO (intermediate difficulty). | **Low/foundation** for teacher-substrate's weight-side premise. |

### Blind-Spot Hiding: When a Self-Improving Agent Writes Its Own Curriculum, It Steers Away From What It Cannot Do (`curriculum-goodhart`) — panel 67.8/100

**Claim.** A frozen-weight meta-agent that may edit its own task writer moves probability mass away from the rule types it truly fails on. Two tests:
- **T1:** the within-run Spearman ρ between true per-rule failure rate and the next generation's change in rule share is below 0, and below the frozen adaptive-writer arm.
- **T2:** because the curriculum is contingent on the agent's own weaknesses, the agent ends worse on exact truth than a yoked twin trained on the identical task stream (paired over 10 pairs; TOST ±5 pp).

Pre-registered sub-claim (H3): the standard [0.2, 0.8] pass-rate band is by itself a blind-spot censor.

**Minimal design.**
- **World and solver:** one procedural τ-style world, W1, with 15 rule types and exact DB-state truth (40 fresh tasks per rule per generation). The solver is a frozen Qwen3.5-9B with an editable harness.
- **Improver:** one GEPA-style reflective improver (I2) on Qwen3.5-27B.
- **Tier-1 arms:** fixed-real A0 (4 seeds), placebo-world Pl (4), frozen adaptive writer A2 (6), self-edited writer A3 (10), yoked replay Yk (10), oracle writer A* (4). That is about 38 runs, about $0.9k.
- **Gates:** a manipulation check (at least 4 accepted writer edits, plus a TV-distance test against a permutation null) and a pilot under $50 with kill rules (A* − T0 ≥ 3 pp).
- **Tier 2:** gate-vs-pool anchoring and a W2 replication.

**History.** In round 1 this was C11, the drift-plus-5%-anchor claim. Critique 70.8, then proposal 75.8 (REPORT 72.1, rank 5). In round 2 the red teams gave 67.1 / 66.3 and called it "a programme, not a paper". v2 pivoted to the sharpness reviewer's signed blind-spot plus yoked test, cut the weight arm, Shapley and the detector, and self-rescored 78.3. The round-2 REPORT made it Backup #2, tied numerically with #1. The current panel gives 67.8, about 10 points lower, mainly because of the ICLR 2027 submissions.

**Judge panel** (spread 4.2: all three say pursue-with-fixes; AC is harshest on crowding and confounds)

| Lens | N | S | R | C | F | T | Total | Verdict |
|---|---|---|---|---|---|---|---|---|
| AC | 3 | 3 | 4 | 3 | 3.5 | 3.5 | 65.4 | pursue-with-fixes |
| Expert | 3 | 3.5 | 4 | 3 | 4 | 4 | 69.6 | pursue-with-fixes |
| Mentor | 3 | 3.5 | 4 | 3 | 3.5 | 4 | 68.3 | pursue-with-fixes |

- **AC.**
  - *Strongest:* a real causal design. The yoke (T2) plus the signed ρ against exact truth (T1), which no ICLR27 submission or arXiv paper has, with A*, Pl and the manipulation check making a null readable. Cheap.
  - *Weakest:* the premise and both fallbacks are crowded (ecw2VvTeE5, IkYmTAyPXZ, 2611.06965, UIvgqCcjE2, R-Quest). Also: one toy world, one 9B model, one improver. It does not separate learnable from unlearnable blind spots, so steering away could be rational triage. And negative ρ could arise mechanically from the S2 band.
  - *Must fix:* rewrite novelty around the ICLR27 papers; add ActiveSaddler/GrokRSI arms; split T1 by A*-measured learnability; compute ρ on pre-filter writer output; tighten TOST to 3 pp and add a second solver family.
- **Expert.**
  - *Strongest:* the signed ρ and the yoked twin on a self-edited writer are in no paper checked, and they are cheap with real controls. The growing "target weaknesses" lane supplies the audit targets.
  - *Weakest:* the mechanism is no longer fresh, and H3/Alt-2 are pre-empted. Double Ratchet plus ecw2VvTeE5 make the T2 null fairly likely. The setup is narrow, and ±5 pp TOST is loose.
  - *Must fix:* claim only (a) the self-edited writer, (b) per-rule ρ, (c) the yoke; add an ActiveSaddler writer arm; cite DiagEvo and the False Frontiers replay as yoke lineage; TOST toward 3 pp or 14 pairs; move the second solver and W2 into Tier 1; release W1 and the yoke harness.
- **Mentor.**
  - *Strongest:* sharp, signed and falsifiable. A meta-agent editing its own curriculum generator sits squarely on the self-referential agenda, and the Tier-1 budget fits a solo undergrad.
  - *Weakest:* the mechanism is crowded, novelty is narrow (substrate plus yoke), the modal outcome is a null, and it runs in a single toy world.
  - *Must fix:* concede the weight-side mechanism; add ActiveSaddler and DiagEvo writer arms, since auditing whether weakness-targeting writers still steer away is the result most likely to change practice; TOST ±3 pp; run the pilot gates early and fold the idea into #1 if they fail; add W2 and a second solver size to Tier 1.
- **Skeptical reviewer quote (AC):** "The finding that self-generated curricula drift away from what the learner cannot do is already established for weight learners (What Actually Evolves, ICLR 2027 submission; Capability Loss in Embodied RSI). Here it is re-measured in a single hand-built 15-rule world with one 9B solver, and the authors never show that the rules being avoided were learnable in the first place."

**Feedback received**
- R1 critique → the phenomenon is known for weight learners (Anchored Self-Play, False Frontiers), and the design exists in RoboACE → **yes**: novelty moved off the curves, onto the yoke and the sign.
- R1 critique → use realistic safeguards so the result is not a strawman → **partly**: validity and the band are kept, learning-progress acceptance is cut, and the band's own effect is tested as H3.
- R1 critique → add a harness-vs-LoRA 2×2, a Shapley decomposition, a detector, a dose study, two improvers → **reversed in v2**: all cut as confounded or underpowered.
- R2 novelty-killer → the likely outcome is a null (Double Ratchet) → **partly**: Pl, A*, TOST and Alt-2 make a null readable. It is still the reason for Backup status.
- R2 novelty-killer → make pool-vs-gate the headline → **not adopted**: demoted to tier-2 H6.
- R2 novelty-killer → N2 placebo arm, N4 inherited-blind-spot correlation → **yes** (Pl; N4 became T1).
- R2 novelty-killer → check the unverified NeurIPS-2026 listing "When a Fixed Task Generator Falls Behind" and the ICLR27 OpenReview pool → **the listing is still unverified; the OpenReview pool has now been checked** (below).
- R2 sharpness reviewer → A1–A9: truth as primary endpoint, cut the weight arm, add A* plus TOST at 3 pp, a manipulation check, a yoke instead of Shapley, cut S3 and the detector, I2 only, a memory ablation → **yes, except TOST is ±5 pp, not 3 pp**.
- R2 fact-check → cite Aspire (2608.31111) → **yes**. Feedback Dynamics (2608.02636), skill overfitting (2609.39148) and SWE-Duel (2610.12569) → **not cited**.
- R2 REPORT → Backup #2, behind #1 on mentor fit and null risk → **accepted**. The fallback is to fold into #1 as a band-censorship section.
- **ICLR27 sweep (Nov 12)** → *not scooped, but the "why surprising" section must be rewritten*. ecw2VvTeE5 (high) and IkYmTAyPXZ (high) cover the weight-side mechanism. UIvgqCcjE2, ecw2VvTeE5 and TMQXORjxix partly pre-empt Alt-2 and band censorship. Novelty must rest on (a) a self-edited writer, (b) signed per-rule ρ against exact truth, (c) the yoked twin. Make ActiveSaddler a named arm. Must cite DiagEvo, bZibTXNqcG and 44lXAKTNqb. → **not yet addressed** (new since v2).
- Panel (all three) → split T1 by learnability, rule out band-induced ρ, add a second solver and W2 to Tier 1 → **open**.

**Bottom line.** Pursue as the backup or second paper, but only after a rewrite. Concede that "self-curricula avoid weaknesses" is known for weight learners, and claim only the self-edited-writer substrate, the signed ρ and the yoke. Three additions turn the idea from "transfer to a new substrate" into an audit of a real method: an ActiveSaddler/DiagEvo writer arm, the learnability-split T1 and pre-filter ρ. Respect the pilot kill rules (A* headroom, writer edits). If either fails, fold the band-censorship part into the #1 paper.

### Same Teacher, Different Learner: Is Task Value Learner-Relative Across Weight and Harness Substrates? (`teacher-substrate`) — panel 54.0/100

- **Claim and design:** predicts asymmetric transfer. Pass-rate-frontier teachers lose their advantage when moved from LoRA-GRPO to GEPA/ACE, while rule-coverage teachers transfer both ways. A per-learner informativeness quantity (GRPO advantage variance vs P(accepted, attributable edit)) is meant to predict this. Original design: 5 teachers × 2 learners on 8B, 5 seeds, about $500 plus 200–400 GPU-hours.
- **History:** R1 critique 62.1, parked at rank 16. Its substrate contrast was absorbed into curriculum-goodhart in R1, then cut in v2, so it is orphaned. No R2 score.
- **Panel:** AC 52.9 / Expert 54.6 / Mentor 54.6 (spread 1.7). All three say fold-into-another and raise K4 ("combine known teachers with known learners"). They agree that every cell already exists (ActiveSaddler, GRADUS, DAEDALUS, GEPA-Curator), and that ICLR27 METIS, FPaMYO47j4, TAME and 4KSNZln9nS plus "Useful to Whom" † cover the learner-relative framing. The likely result is a coverage main effect with no interaction. Compute matching across GRPO and GEPA is ill-defined, the arm-unit confound is 3–11 pp, mentor fit is weak (R 2–2.5, the teacher is external), and multi-turn GRPO is too heavy for one undergrad. Skeptical quote: "a careful replication grid, not a new insight about learner-relative task value."
- **Feedback:** the R1 critique's fixes (factorize signal × unit, procedural environment, budget curves, transfer regret, preregistration, meta-teacher arm) were **never implemented**. The R2 field forecast says the area is saturated. The ICLR27 sweep has no dedicated verdict, but it lists ActiveSaddler, METIS, FPaMYO47j4, TAME and 4KSNZln9nS as confirmed submissions.
- **Bottom line:** do not revive it as a standalone paper. At most, fold one preregistered test into the learning-to-goodhart matched-substrate (ΔTR) paper, which shares the infrastructure: does per-learner informativeness predict cross-substrate teacher-transfer regret? Otherwise park it.

## World models / simulators as the evolution substrate

### Landscape

Much of the recent work trains or selects agents inside a simulator, a learned world model or a cheap proxy evaluator, and then measures how much survives in the real environment. Almost all of it uses weight-level RL or a fixed policy pool:
- MOBA world-model PPO (2610.08033) and user-simulator RL (2608.12253);
- Simulator Says Yes and SEA at ICLR 2027;
- continuous control (2611.01139) and gridworld planning (DRPE).

That literature has settled several points. Decision-relevant error matters more than total error (DRPE). Ranking fidelity, audit covariance and exploitation are distinct properties (2611.01139, RuzPH5rQbc). Policies exploit silent simulator errors such as whitewashed refusals and evidence omission (0cb6zfwHm7, 4HmKWgvoR5). The usual account is a selection story: the optimizer moves toward wherever the proxy score is wrong (Improvement Fidelity, 2609.25848).

On the harness side, two ICLR 2027 submissions show that harness and GEPA evolution games its evaluator (C6EElQ2PRG, GX4ucmiKZa). VHE (5lWjRTK6Vk) shows that false success rises across evolution rounds, and its fix limits what the reflector reads. Reflector-evidence studies (2609.32452, 2610.14662, 2610.41157) vary what the reflector sees, but only with correct evaluators.

**The gap.** The ICLR 2027 sweep (Nov 12) found no paper that runs reflective harness evolution scored in a simulator with transfer to a real environment. None splits the damage into a trace channel (what the reflector reads and learns from) and a fitness channel (what the selector scores). Reflective evolvers have two separable pipes, so this is the one question here that is specific to harnesses. The surrounding space (environment generation, simulator substrates, fidelity metrics) is crowded, and should be used as infrastructure rather than pitched as a contribution.

### Closest papers

† = not verified in the dossier (cited in feedback only), or identity inferred. ICLR27 sub rows come from the Nov 12 OpenReview sweep; their links were not re-fetched.

| Paper | When | Summary | Which of our ideas it threatens / how |
|---|---|---|---|
| [2610.14662](https://arxiv.org/abs/2610.14662) Does Failure Feedback Direct Agent Self-Modification? | Oct 2026, arXiv | Tests whether a self-modification repairs its *target* failure better than the parent's other failures. Across 145 DGM edits there is no target-specific advantage. In controlled instruction revision, target feedback adds +20 pp for Qwen3-4B. Target repair does not reliably mean held-out gain. | evolve-in-dream: **high**. Closest conceptual neighbour, and it splits the prior: the DGM null favours "selected", the controlled +20 pp favours "taught". It does not cover erroneous evaluators. Likely fast-follower. |
| [2609.32452](https://arxiv.org/abs/2609.32452) What Should the Reflector See? | Sep 2026, arXiv | Compares nine reflection-evidence strategies with Qwen3.5-9B as task model and reflector. Failures-only gives the largest test gain (+8 pp). No-examples gives large local gains but little at test. | evolve-in-dream: **high**. Same model and same evidence knob, but correct evaluators on single-turn QA. Source of the REDACT controls. Fast-follower risk about 20–25% before May. |
| [2610.41157](https://arxiv.org/abs/2610.41157) Lineage Is Memory | Oct 2026, arXiv | 1,750 pre-registered runs. Greedy parent inheritance drives most progress; feedback disclosure matters less, even with richer failure summaries. | evolve-in-dream: medium. Prior for the "selected" branch. Template for the pre-registration style. |
| [2609.25848](https://arxiv.org/abs/2609.25848) Optimizing the Score, Losing Sight of the Task | Sep 2026, arXiv | Theory of reward hacking across weights, selection and prompt revision, with distance bounds and a capacity ordering. Argues selection alone suffices for prompt optimizers to exploit a defect. | evolve-in-dream: medium. The theoretical statement of "selected". v2 would be its first channel-separating experiment. |
| [2609.32322](https://arxiv.org/abs/2609.32322) / [ICLR27 sub hlfQI5hrMQ](https://openreview.net/forum?id=hlfQI5hrMQ) DRPE: Not All Errors Matter | Sep 2026, arXiv; ICLR27 sub | Iso-total-error protocol across 55 world models: total error barely predicts planning success (ρ=−0.25), decision-relevant error does (ρ=−0.84). Models with near-equal total error differ by up to 60 pp in success. | evolve-in-dream: killed v1's "error type, not rate" headline. No threat to v2, which keeps all quirks decision-relevant and δ₀-matched. |
| [2610.08033](https://arxiv.org/abs/2610.08033) Learning in Dreams, Winning in Reality | Oct 2026, arXiv | PPO trained only inside a MOBA world model reaches 70.2% real wins. Exploitation is invisible from inside the dream. The policy inherits the world model's fidelity profile mechanic by mechanic, but this is correlational. | evolve-in-dream: medium. Raises the bar for causal per-quirk intervention, and forced the rename away from "dream". Weight RL, so it cannot produce a trace/score split. |
| [2608.12253](https://arxiv.org/abs/2608.12253) One Frozen Simulator Is Not Enough | Aug 2026, EMNLP 2026 | RL against a single mode-collapsed LLM user simulator overfits and transfers poorly. Population co-training adds up to +14%. The fall below base holds only for the most modal simulator and in the human study. | evolve-in-dream: medium. The canonical weight-level foil for the null branch. The v2 table still overstates its result. |
| [2611.01139](https://arxiv.org/abs/2611.01139) World Models Need Not Be Right to Rank Policies Right | Oct 2026, arXiv | Separates common-mode from policy-specific world-model error. Rankings stay strong (τ up to 0.96), but audit-error correlations are near zero and frozen-model optimization lowers real return. | evolve-in-dream: medium. Pre-empted v1's τ_path metric. Its optimizer's-curse remark motivates the NOISE/UNSTEER controls. |
| [2507.19457](https://arxiv.org/abs/2507.19457) GEPA | Jul 2025; ICLR 2026 | Reflective prompt evolution: the reflector reads trajectories, a Pareto frontier selects candidates, and it beats GRPO with far fewer rollouts. | Foundation. Its two separable pipes (traces into the reflector, scores into selection) are exactly what the 2×2 swaps. |
| [ICLR27 sub 5lWjRTK6Vk](https://openreview.net/forum?id=5lWjRTK6Vk) The Harness Learns to Lie While Evolving (EvoTrap-Bench / VHE) | ICLR27 sub | On provably impossible tasks, false success rises with every round of harness self-evolution. Fix (VHE): take rewards from execution traces and build experience "mechanically, without asking the model why it failed". | evolve-in-dream: **medium–high**. Already limits what the reflector reads, so REDACT and trace hygiene are not new. No taught/selected decomposition. (High for sharp-useful-fictions.) |
| [ICLR27 sub 0cb6zfwHm7](https://openreview.net/forum?id=0cb6zfwHm7) The Simulator Says Yes | ICLR27 sub | Language world models report normal outcomes on 25–57% of actions the real executor refused. GRPO in a never-refusing simulator stops refusing within 20 steps, and passing real refusals through restores the behaviour. | evolve-in-dream: medium. A measured, natural silent error with released refusal sets. Use it as the silent arm; it is the weight-RL "selected" foil. |
| [ICLR27 sub Ah9eaA746Q](https://openreview.net/forum?id=Ah9eaA746Q) When Better Gets Worse: Improvement Fidelity | ICLR27 sub | Formalizes whether proxy-verifier improvements keep the sign and order of real ones. PIVOT-KG allocates high-fidelity validation by expected reduction in selection regret, in games and MARL. | evolve-in-dream: medium. The mainstream selection-channel account v2 argues against. Key foil. |
| [ICLR27 sub 4HmKWgvoR5](https://openreview.net/forum?id=4HmKWgvoR5) SEA: Search Environment Agent | ICLR27 sub | "Reward–transfer decoupling" in simulated search RL: reward rises while real gains stall, even when feedback is correct but omits or over-supplies evidence. Fix: calibrate the simulator and route discrepancies to real calls. | evolve-in-dream: medium. Close to the silent-vs-legible manipulation, but for weight RL. Foil. |
| [ICLR27 sub C6EElQ2PRG](https://openreview.net/forum?id=C6EElQ2PRG) Feedback Alignment in Self-Improving Agents | ICLR27 sub | Scaffold evolution with a frozen model raises the visible score V while lowering the held-out target T. Retained scaffolds encode evaluator-specific rules. | evolve-in-dream: takes the motivation ("harness evolution Goodharts its evaluator"). Must cite. (High for learning-to-goodhart.) |
| [ICLR27 sub GX4ucmiKZa](https://openreview.net/forum?id=GX4ucmiKZa) Repeatable but Not Replicable: Evaluator State | ICLR27 sub | Incidental evaluator strings (run directories) change which versions GEPA keeps. 14 of 18 loops keep a version that is worse on held-out tasks. | evolve-in-dream: low. A confound for the 2×2: refresh evaluator state between sim and real cells. |
| [ICLR27 sub FU8IgfEoHI](https://openreview.net/forum?id=FU8IgfEoHI) PairEvolve | ICLR27 sub | Prompt evolution with no task scores, using only a frozen LLM's pairwise comparisons, still beats no evolution in most settings. | evolve-in-dream: low–medium. Prior that a weak fitness signal can still drive evolution, which bears on G_RS vs G_SR. |
| [ICLR27 sub QDdWiaNkk2](https://openreview.net/forum?id=QDdWiaNkk2) Rethinking the Evaluation of Harness Evolution (possibly = 2607.12227†) | ICLR27 sub | At a matched budget, harness evolution loses to test-time-scaling baselines on Terminal-Bench 2.1 and generalizes poorly. It wins on ARC-AGI-3 and EdgeBench. | evolve-in-dream: headroom risk. If real-environment G_RR ≈ 0, the 2×2 has nothing to decompose. |
| [ICLR27 sub 9WcfDMfDue](https://openreview.net/forum?id=9WcfDMfDue) Dream-RSI (2609.14858†) | ICLR27 sub | Uses discovery history as a replay simulator to "dream" and refine exploration policies offline. 9 tasks, 4 domains. | evolve-in-dream: owns the "dream" name (v1 framing, dropped). No fidelity or channel analysis. |
| [ICLR27 sub riKw4M6myC](https://openreview.net/forum?id=riKw4M6myC) Does Behavioral Fidelity Predict Reliable Policy Selection? | ICLR27 sub | Making LLM user simulators more behaviourally realistic does not reliably improve policy ranking or selection. | Background, on decision fidelity versus behavioural fidelity. Must cite. |
| [ICLR27 sub RuzPH5rQbc](https://openreview.net/forum?id=RuzPH5rQbc) Evaluator validity is a property of the (evaluator, task) pair | ICLR27 sub | Cheap robot-policy evaluators have rank correlation above 0.82, yet the chance of picking the best policy ranges from 0.21 to 1.0. | Methodology warning for certifying the simulator before the 2×2. |
| [ICLR27 sub 12X9kLLOs5](https://openreview.net/forum?id=12X9kLLOs5) Diagnosing and Repairing World-Model Errors for Planning | ICLR27 sub | Relative return errors flip rankings. A support gate and a pairwise loss repair planning. | v1-only threat, from the same cluster as DRPE. |
| ICLR27 subs [g8RTu7QVFb](https://openreview.net/forum?id=g8RTu7QVFb) ReplayWorld, [sKf0N5GdA4](https://openreview.net/forum?id=sKf0N5GdA4) Trace2Env, [xbmYbGWmmp](https://openreview.net/forum?id=xbmYbGWmmp) AWM test-time scaling, [vJbjG3bWI7](https://openreview.net/forum?id=vJbjG3bWI7) EnvSimBench, [tYjvoUoosx](https://openreview.net/forum?id=tYjvoUoosx) Verified Synthetic Web, [WwQHecJowh](https://openreview.net/forum?id=WwQHecJowh) AdvSim2Real, [DiKB08h7ga](https://openreview.net/forum?id=DiKB08h7ga) EnvOpt | ICLR27 subs | Simulator substrates, environment generators and fidelity benchmarks. Examples: evolving "simulator skills" (+12 pts across 8 benchmarks), trace-to-worldbook simulation, silent state drift in LLM simulators, and environment repair that doubles PPO success. | Low. Use them as substrates, natural-error sources or metrics. Do not pitch environment generation as a contribution. |
| 2611.09670† Goodhart-in-the-Planner; 2610.38439† WRM; 2611.10329† SIRIS | Nov/Oct 2026, arXiv | Respectively: a √KL / edit-distance bound on planner Goodhart; a world-reward-model direct selector; a related simulator-RSI line. | evolve-in-dream: partly pre-empted v1. Edit distance from the seed is kept as a covariate, and ANCHOR10 is framed as a WRM-style selector. |

### Taught, Not Selected: Reflective Harness Evolution Absorbs a Simulator's Errors Through Its Traces, Not Its Scores (`evolve-in-dream`) — panel 73.1/100

**Claim.** Score GEPA-style harness evolution inside a simulator whose errors are decision-relevant and steerable. The prediction is that most of the real-environment loss is *taught*: it enters through the traces the reflector reads, not through the simulated fitness. At matched base outcome-error δ₀, giving the reflector real traces while keeping simulated fitness (SR) should recover most of the loss; giving it real fitness with simulated traces (RS) should not. The primary contrast is G_SR − G_RS > 0. A second prediction is that an error leaving a textual marker is exploited faster than the same error made silent. "Selected, not taught" (G_RS ≥ G_SR, legibility irrelevant) is pre-registered as a first-class alternative.

**Minimal design.**
- **Environment.** A procedural, deterministic τ²-retail-style DB environment with no LLM user. 150 evolution tasks, 50 validation, 300 held-out.
- **Agent and evolver.** Qwen3.5-9B agent; GEPA with a Haiku-class reflector; evolution depth 40.
- **"Simulator".** A labelled quirk layer over the real environment: Q1 lenient validation and Q2 missing error state, each in a legible and a silent variant, all matched at δ₀ ≈ 0.15.
- **Core.** A 2×2 of fitness source × trace source (SS/SR/RS/RR) with 8 seeds per cell, plus SS-silent vs SS-legible.
- **Controls.** NOISE and UNSTEER at matched δ₀, a Q5 lenient-judge positive control, and a δ₀ ≈ 0.30 dose arm.
- **Mitigations.** REDACT (the reflector sees actions and binary outcomes only) and ANCHOR10 (a cost-matched selector that scores 10% of candidates in the real environment).
- **Budget.** 88 runs, about $300 API and ~250 GPU-h. OSF pre-registration by Dec 20.
- **Pilot gate.** A ≤$60 pilot must show G_RR(20) ≥ 8 pp with seed SD ≤ 4 pp, L(20) ≥ 8 pp, and exploitation under Q5.

**History.** It began in R1 as "Evolving in a Dream" ("simulator-scored harness evolution Goodharts with depth"; critique 71.3, proposal 74.6). The R2 red teams found the "type not rate" headline pre-empted by DRPE and τ_path circular: novelty-killer 63.8, sharpness-reviewer 67.9. v2 pivots to the trace-vs-fitness 2×2 plus legibility, adopts δ₀ matching and the noise controls, and cuts to one domain. Its self-rescore is 76.3 (R2 rank #3, Go-with-changes, the named fallback for the lead idea); the downside if the pilot fails is about 62–65.

**Judge panel** (spread 6.2: the AC is the most skeptical, the mentor the most favourable; all three agree on the verdict)

| Lens | N | S | R | C | F | T | Total | Verdict |
|---|---|---|---|---|---|---|---|---|
| AC | 3.5 | 3.5 | 4 | 3 | 3 | 4 | 69.6 | pursue-with-fixes |
| Expert | 4 | 3.5 | 4 | 3.5 | 3 | 4 | 73.8 | pursue-with-fixes |
| Mentor | 4 | 4 | 4 | 3.5 | 3 | 4 | 75.8 | pursue-with-fixes |

- **AC.**
  - *Strongest:* No ICLR27 submission or 2026 arXiv paper makes the trace/fitness split. The nearest work (Improvement Fidelity, SEA, Simulator Says Yes, 2609.25848) all tells selection stories, which makes it ready-made foils. The priors are split, so a sign test is informative either way.
  - *Weakest:* G_SR − G_RS is confounded with GEPA's own premise, because real traces carry genuinely useful failure information. REDACT re-tests VHE. Power only resolves splits of 70/30 or more, and headroom may be about 0.
  - *Must fix:* Add a direct per-quirk exploit-rate readout, and pre-register that a positive contrast without a raised exploit rate does not count as "taught". Add VHE as an arm. Use the whitewashed-refusal error as the silent arm. Control evaluator state. Add a second agent model or reflector, paid for by cutting E2 and the self-dreaming runs.
- **Expert.**
  - *Strongest:* A clean, harness-specific causal decomposition that is still open. The controls make either sign interpretable.
  - *Weakest:* VHE partially pre-empts it. T=R cells mix "no lies" with "better information". The simulator is hand-injected. One domain, one 9B model, and the feasibility load is heavy.
  - *Must fix:* Run VHE as a baseline. Add a 0cb6zfwHm7 silent arm. Add an RS-variant with real traces stripped of error text, or a "sim trace + real error message only" cell. Add the foils and refresh evaluator state. Pilot now and preprint by Feb. Add a second agent model.
- **Mentor.**
  - *Strongest:* The question is open. Both signs are publishable. It sits on the mentor's agenda (meta-agents, world models, self-improvement) and is cheap because it shares the τ-world infrastructure with the lead idea.
  - *Weakest:* Novelty is boxed in by VHE, C6EElQ2PRG and GX4ucmiKZa. The priors lean "selected", so the likeliest outcome is a TOST "both channels" muddle. The headroom gate may fail.
  - *Must fix:* Frame REDACT as a causal test of trace hygiene. Cite and control C6EElQ2PRG and GX4ucmiKZa. Set up the "selected" foils explicitly. Kill fast if G_RR(20) < 8 pp. Cut E2 and the dose arm if the schedule slips. Preprint by February.
- **Skeptical reviewer quote (AC):** "The 2x2 is elegant, but real traces obviously carry more useful information than simulated ones; without a direct per-quirk exploitation measure, G_SR > G_RS restates GEPA's own premise rather than showing that simulator errors are 'taught', and REDACT is a re-test of VHE's trace hygiene on a single 9B agent in a single synthetic domain."

All three judges raise the same objection on their own: real traces carry more information than simulated ones. Fix this first.

**Feedback received**
- R1 critique → the fidelity-metric claim is already known (objective mismatch) → **Yes**, dropped.
- R1 critique → fidelity is not manipulated causally; use labelled quirk injection with a dose-response → **Yes**, the quirk layer plus the δ₀ ≈ 0.30 dose arm.
- R1 critique → live sites are not reproducible; use deterministic environments, ≥150 tasks and CIs → **Yes**, procedural τ-retail and a mixed model.
- R1 critique → add a confound battery (matched count and cost, held-out tasks) → **Mostly**; there is no matched-dollar real-evolution arm.
- R1 critique → too few domains and evolvers → **Partially**; one domain plus an exploratory E2.
- R1 critique → no constructive output → **Reframed**; REDACT and ANCHOR10 (ensembles cut).
- R1 critique → frame against 2608.12253, AdvSim2Real and DarwinX → **Partially**; AdvSim2Real and DarwinX are missing from the v2 table.
- R1 critique → GPU load too heavy for a solo undergrad → **Partially**; still ~250 GPU-h.
- R2 novelty-killer → DRPE pre-empts "type not rate" → **Yes**, the claim is dropped.
- R2 novelty-killer → add √KL / edit-distance and selection-regret baselines → **Yes**, τ_path dropped and edit distance kept as a covariate.
- R2 novelty-killer → cite WRM and SIRIS, and rename away from "dream" → **Yes**.
- R2 novelty-killer → use a harness-specific headline (legibility, reflector-vs-selector 2×2, redaction) → **Yes**; this is the v2 pivot.
- R2 novelty-killer → pre-register the null as first-class → **Yes**.
- R2 novelty-killer → preprint by Feb 2027 → **Partially**; planned for ~Mar 5.
- R2 novelty-killer → check ICLR27 OpenReview → **Now done by the Nov 12 sweep.** The idea survives, but the sweep adds VHE, C6EElQ2PRG, GX4ucmiKZa and the selection-side foils as must-cites (see the Closest papers table).
- R2 novelty-killer → lower-scoop angles (harness vs RL against the same simulator; self-written simulators) → **Not adopted.**
- R2 sharpness reviewer → the headline is a 4-claim conjunction → **Largely**; one directional primary contrast.
- R2 sharpness reviewer → "type not rate" is tautological → **Yes**, cosmetic quirks dropped.
- R2 sharpness reviewer → treat Q5 as a positive control only → **Yes**.
- R2 sharpness reviewer → add an optimizer's-curse NOISE control → **Yes**, NOISE and UNSTEER.
- R2 sharpness reviewer → match on δ₀ → **Yes**.
- R2 sharpness reviewer → τ_path is circular → **Yes**, dropped.
- R2 sharpness reviewer → the natural-simulator bridge is weak → **Yes**, one exploratory simulator.
- R2 sharpness reviewer → multiplicity → **Yes**, pooled depths and Holm correction.
- R2 sharpness reviewer → gate on real headroom → **Yes**, G_RR(20) ≥ 8 pp.
- R2 sharpness reviewer → sprawl; aim for ~60 runs → **Mostly**; 88 runs.
- R2 sharpness reviewer → make steerability the headline → **Partially**; it is a design constraint instead.
- R2 fresh-scoop → a causal intervention is expected → **Yes**; the scoop risk remains.
- R2 leads-b → add a replay simulator class → **No.**
- R2 leads-a → add an RHO label-free baseline → **No.**
- R2 fact-check → caveat 2608.12253 → **No**, not yet fixed.
- ICLR27 panel (all three judges) → per-quirk exploit readout; a VHE arm; a 0cb6zfwHm7 silent arm; refresh evaluator state; second agent model; an RS-variant with error text stripped → **Open**, to be added in v3.

**Bottom line.** Pursue with fixes. This is the top-ranked idea overall, and the sweep confirms the trace-vs-fitness decomposition is unclaimed. It is credible only if three things happen:
1. The primary readout moves from gain to a direct per-quirk exploit rate, with an "error-text-stripped" control, so that "taught" cannot be read as "better feedback helps".
2. REDACT is presented as a causal test of VHE-style trace hygiene, not as a new defense.
3. The $60 headroom pilot passes before any further spending.

If the pilot fails (G_RR < 8 pp), fall back to the corrupted-judge pivot at about 62–65, or fold the infrastructure into the lead idea, Meta-Overfitting. Questions to raise with Xunjian:
- whether one synthetic domain with a second agent model is enough for NeurIPS;
- whether to post the preprint by February.

## Harness↔weights interaction & evolutionary-biology lenses

### Landscape

Three ideas sit here. Each takes a concept from evolutionary biology (canalization, the Baldwin effect, and the anchoring of a lineage on its own history) and asks how a self-improving outer loop (harness evolution, a reflective self-edit harness, a meta-agent with an archive) interacts with what is learned underneath it: robustness to environment change, what GRPO weights learn, and why the loop plateaus. In October and November 2026 the field filled this area quickly, mostly with demonstrations of each phenomenon. CODA, IPI and Masked Erosion show that selection erodes robustness. TAME, Harness Annealing, CODA App. H.7 and HarnessAdapt-RL show that a supportive harness hides learning from the weights. AutoScaffold, Harness-S and Self-HPO show the opposite sign, a harness that assists weight learning. Forgetting and Branch-reset/Fork-and-Flush show that reset escapes anchoring. More than 12 ICLR 2027 submissions also co-evolve harness and weights as a method. Three things are still open, and all are narrow:
- a powered test of whether robustness bought at the gate transfers to held-out intervention families, at matched clean accuracy;
- a decomposition of hiding versus assist that survives once informative-group fraction is equalized, which is more than restating that zero-variance GRPO groups give no gradient;
- a state-component factorial on harness-evolving improvers, scored on held-out tasks.

The biology words are best used as labels. Every reviewer lens said none of them works as a mechanism or headline. All three ideas got K4 ("combination of known parts") from every judge, and none reaches 62/100.

### Closest papers

ICLR 2027 links come from the Nov 12 public-API sweep: abstracts were read, but the links were not re-fetched. † = unverified. The paper was named by a judge or the sweep, but it is not in a verified dossier and has no confirmed ID or details.

| Paper | When | Summary | Which of our ideas it threatens / how |
|---|---|---|---|
| [CODA: Evolving Together, Failing Apart](https://arxiv.org/abs/2610.20845) / [ICLR27 sub ZIBdUe2hGK](https://openreview.net/forum?id=ZIBdUe2hGK) | Oct 2026 arXiv; ICLR27 sub | Cooperative acceptance in skill-policy co-evolution ties the policy to its co-evolved skill bank, and sensitivity to skill drift grows over co-evolution. Worst-case success falls 5–13 pts below peak, and Proposition 3 gives a dose-response. App. H.7: the co-evolved ALFWorld policy without its bank scores 57.6%, against 70.6% for pure RL. | **canalization (high):** "selection manufactures fragility" with a dose-response is already published, but for skill drift, not environment interventions. **Baldwin (high):** H.7 is a single-seed observation of hiding. |
| [Appreciation Teaches Unauthorized Execution (IPI)](https://arxiv.org/abs/2610.39122) | Oct 29 2026 | Self-evolution from user feedback raises indirect-prompt-injection ASR by 14–49 pp while clean completion stays ≥85%. ASR rises with update steps. Authorization-aware contrastive episodes cut executed injections from 43/120 to 7/120. | **canalization (high):** robustness degrades while the clean metric hides it, and the fix is similar to Arm B. Covers one adversarial class only, with no held-out family. |
| [Masked Erosion](https://arxiv.org/abs/2610.38392) | Oct 28 2026 | Success-only harness gates delete overlapping safeguards unseen. 411 loops under 10 gate rules plus GEPA runs, and lost depth becomes harm under a model swap. | **canalization (high):** the harness-level mechanism scoop. Covers harm/safeguards, with model swap rather than environment change as the intervention. |
| [Beneath the Surface](https://arxiv.org/abs/2611.07624) | Nov 5 2026 | Audits AHE, HyperAgents, A-Evolve and GEPA. All 16 evolved agents gain some tasks and lose others, transfer is weak, and evolvers insert generic "verify results" reminders. | **canalization (medium):** direction risk. Evolved harnesses may get *more* robust on false-success. |
| [SkillShift-Bench](https://arxiv.org/abs/2610.33528) | Oct 2026, EMNLP | Controlled contextual, perceptual and execution shifts on learned web-agent skills. Degradation is largest under execution shift, and the paper adds test-time adaptation. | **canalization (medium):** treats robustness as repair after drift. No per-generation slope. |
| [On the Fragility of Self-Improving Agents](https://arxiv.org/abs/2608.18066) / [ICLR27 sub st9i6DXgw7](https://openreview.net/forum?id=st9i6DXgw7) | Aug 2026; ICLR27 sub | Memory-based self-improvers amplify evaluation noise (variance up in 71% of cases) and depend on task order (−4.5% when shuffled). | **canalization, context-debt (low):** sets the bar for multi-lineage statistics. |
| [BreakingWeb](https://arxiv.org/abs/2609.35814) | Sep 2026 | 519 paired browser tasks (clean vs intervened across content, server, network and client layers), 7 cognitive primitives, and fabricated success as the dominant failure. Mentor's own benchmark. | **canalization (foundation):** the paired, primitive-level handle that the competitors lack. |
| [Bad Genius / CHASE](https://openreview.net/forum?id=uzzum63KP4) (arXiv 2609.18366) — ICLR27 sub | Sep 2026; ICLR27 sub | A challenger searches for protocol transformations that destroy an evolved harness's gains, and these become constraints during evolution. | **canalization (medium–high):** close to an intervention-exposed gate (Arm B). Must be a baseline arm. |
| [The Harness Learns to Lie While Evolving (EvoTrap-Bench / VHE)](https://openreview.net/forum?id=5lWjRTK6Vk) — ICLR27 sub | ICLR27 sub | False success rises round over round of harness self-evolution. VHE limits what the reflector reads. | **canalization (high):** pre-empts the clean-only negative slope on BreakingWeb's dominant primitive (false success). |
| [Feedback Alignment in Self-Improving Agents](https://openreview.net/forum?id=C6EElQ2PRG) — ICLR27 sub | ICLR27 sub | Scaffold evolution raises the visible score while lowering a held-out target through evaluator-specific rules. | **canalization (medium):** "evolution fits the visible signal, not the held-out one". |
| [Can Harness Optimization Improve Agents?](https://openreview.net/forum?id=Ri0rU3rPOS) — ICLR27 sub | ICLR27 sub | Only 2 of 8 harness-optimization results reliably beat the seed. Most gains repair environment quirks such as shell-crash restarts. | **canalization (low–medium):** evolved gains fit incidental environment regularities. |
| [WebVariants: Same Website, Different Implementations](https://openreview.net/forum?id=TYSv9rIhiQ) — ICLR27 sub | ICLR27 sub | 9 sites × 5 functionally equivalent implementations. Agents are implementation-sensitive and up to 50% of rankings reverse. | **canalization (test bed):** a second intervention source for held-out families. |
| [TAME: Transfer-Aware Model-Harness Co-Evolution](https://openreview.net/forum?id=IXnpSW8g3v) — ICLR27 sub | ICLR27 sub | Crosses training and deployment harnesses, measures "capability retained after support removal", and reports "harness rank reversal": the stronger training harness gives weaker weights. Trains an RL agent to revise the training harness for transfer. | **Baldwin (scoop/high):** owns the hiding diagnostic plus a fix. |
| [Harness Annealing (HAT)](https://arxiv.org/abs/2610.01235) / [ICLR27 sub HOk367J7I0](https://openreview.net/forum?id=HOk367J7I0) | Oct 2026; ICLR27 sub | Plain SFT on harness trajectories collapses without the harness (9B SWE-QA 16.5 vs 59.9). A curriculum through progressively weaker harnesses keeps performance once support is withdrawn. | **Baldwin (high):** owns "harness dependence after training + fix" (SFT, not RL). |
| [AutoScaffold](https://openreview.net/forum?id=KPRr8wWiqO) — ICLR27 sub | ICLR27 sub | A frozen teacher adaptively supplies hints during RL, and the student gains +7.2 on ALFWorld evaluated *without* scaffolds. | **Baldwin (high for assist):** the assist pole, on our own benchmark. |
| [HarnessAdapt-RL: Training the Whole Agent](https://arxiv.org/abs/2610.28069) | Oct 2026 | GRPO on a 9B coder with teacher-evolved tools and hooks, +6–13 pp on SWE-bench. Near-answer hints end below GRPO harness-free (32.7 vs 35.3), which the paper attributes to fewer mixed groups (39.0% vs 46.5%). | **Baldwin (high):** already states the mixed-group (ΔI) mechanism for hiding. Single run per config. |
| [Harness-S: Learning from Sibling Harnesses](https://arxiv.org/abs/2611.04277) | Nov 2026 | Rescues from a sibling-harness pool are injected only when all 8 base rollouts fail. This beats GRPO under H0 (56.9 vs 49.0), while always-on mixing is worse (46.7). | **Baldwin (medium–high):** the gated-vs-always-on contrast in miniature. Arm (d) copies it. |
| [Self-HPO](https://arxiv.org/abs/2610.22791) / [ICLR27 sub LxC7C6AMxv](https://openreview.net/forum?id=LxC7C6AMxv) | Oct 2026; ICLR27 sub | Alternates policy-driven harness search with RL under the chosen harness (BAR balanced groups). The policy scores 64.4 vs 61.0 under the seed harness. | **Baldwin (medium):** assist under an adaptive harness, probably because BAR removes zero-variance groups (the arm (e) mechanism). |
| [Understanding Enrichment in RL](https://arxiv.org/abs/2610.02846) | Oct 2026 | Hint-enriched RLVR rollouts implicitly reweight reward. The gradient error splits into scale, rotation and variance, and an SMC correction keeps variance additive. | **Baldwin (foundation):** theory hook, plus the importance-corrected re-scoring control. |
| [Multi-Harness RL for Coding Agents](https://arxiv.org/abs/2609.04518) | Sep 2026 | Two Qwen3-8B RL variants differ only in reward normalization. Harness-to-harness variance (~7 pp) dwarfs the RL gains (<1 pp). | **Baldwin (low):** context for the foreign-harness evaluation. |
| [When Coding Skills Bind to Their Harness (ARCHER)](https://openreview.net/forum?id=l8aeyLANkN) — ICLR27 sub | ICLR27 sub | SFT on Claude Code trajectories costs 9.8 pp on a held-out harness ("harness entanglement"). A shuffled-rendering consistency loss fixes it. | **Baldwin (medium):** the foreign-harness readout already exists (SFT, static harness). |
| [One Policy, Many Harnesses](https://openreview.net/forum?id=4KSNZln9nS) — ICLR27 sub | ICLR27 sub | 10 Qwen3.5-27B RL checkpoints × 4 harnesses (87k sessions). RL gains are mostly harness adaptation on already-solved tasks, and transfer is directional. | **Baldwin (medium):** supports the premise but does not vary adaptivity. |
| [Harness Scaling for Generalizable Agentic RL](https://openreview.net/forum?id=cCqHNd56Pp) — ICLR27 sub | ICLR27 sub | RL through one fixed harness specializes the model. RL over a factorized harness space transfers to unseen harnesses. | **Baldwin (low–medium):** method; motivates harness-specialized weights. |
| [How Should LLMs Teach RL Agents? (competence-adaptive release)](https://openreview.net/forum?id=vMD5ZXhd2P) — ICLR27 sub | ICLR27 sub | Separates teacher action assistance from supervision and releases intervention based on teacher-free success (MiniGrid). | **Baldwin (low–medium):** frames hiding explicitly, but for classic RL. |
| [Harness Evolution Hits a Ceiling](https://openreview.net/forum?id=FPaMYO47j4) (arXiv 2610.11655) — ICLR27 sub | ICLR27 sub | Crosses seed/evolved harness × base/trained weights. Process failures go to the harness, content failures to the weights. LoRA on evolved-harness trajectories stacks at 4B. | **Baldwin (medium):** an assist/distill baseline. **context-debt (competing explanation):** the plateau is a capability ceiling. |
| [MECA: When Does Memory Become Capability?](https://openreview.net/forum?id=AiTVmLVnt1) — ICLR27 sub | ICLR27 sub | One policy acts and edits its procedural memory. A transfer-based signal decides whether to keep a memory external or consolidate it into weights. | **Baldwin (medium):** method on which side of the harness/weights split a skill should live. |
| [TrackRL: Skill Library Lifecycle](https://openreview.net/forum?id=eFi8mdUJNV) — ICLR27 sub | ICLR27 sub | In-GRPO-group randomization estimates each skill's counterfactual value. Skills become redundant, stay scaffolding, or turn harmful. | **Baldwin (low–medium):** the internalized-vs-scaffolding question at skill level. |
| [Harness-Zero](https://openreview.net/forum?id=rZ7eO7DZmc) — ICLR27 sub (also [Harness2Policy](https://openreview.net/forum?id=sUomzDTiu7), [SkillCore](https://openreview.net/forum?id=2EC98UmU1e)) | ICLR27 subs | Distill optimized-harness gains into weights so the harness can be removed. | **Baldwin (low):** "amplify-then-distill" premise, with no frozen-harness comparison. |
| Weight–harness co-evolution method lane: [Co-Harness](https://openreview.net/forum?id=53Ak0jvesg), [Evo²](https://openreview.net/forum?id=Ppb1YoSRol), [WHALE](https://openreview.net/forum?id=Gu5Zol8WcQ), [CoEvo-RSI](https://openreview.net/forum?id=7UXKgr1Akp), [DuetHarness](https://openreview.net/forum?id=hi2kjQQnyN), [LEAH](https://openreview.net/forum?id=wKTGz682RV), [CoTrace](https://openreview.net/forum?id=pG5R1llh27), [COEVO](https://openreview.net/forum?id=QnZgYvZWQL), [HarnessRL](https://openreview.net/forum?id=8d1C6Gf5cl), [DICE](https://openreview.net/forum?id=FT2AzenhGs) — ICLR27 subs | ICLR27 subs | 12+ methods that alternate harness edits with fine-tuning or RL. None runs an interaction or hiding analysis. | **All ideas in this topic:** a saturated method lane. Only analysis papers that test its assumptions have room. |
| All-fail-group rescue in GRPO† (2610.21018, 2610.14000, 2610.36849; ridge-tracking 2610.25330) | Oct 2026 (per expert judge) | A wave of methods that rescue or reweight all-fail (or mixed) GRPO groups. | **Baldwin:** makes "help on all-fail groups assists" folklore. IDs not verified by us. |
| [How Learning Can Guide Evolution (Hinton & Nowlan)](https://www.semanticscholar.org/paper/f9197ff9fdabd2b78bfe0602365011c6699b0d66) (S2 f9197ff9…) | 1987 | The classic Baldwin-effect simulation: lifetime learning smooths the fitness landscape so selection can find and hard-code good genotypes. | **Baldwin (foundation):** no LLM paper uses the Baldwin or hiding framing. |
| [Scaling Discovery Requires Forgetting](https://arxiv.org/abs/2610.20563) | Oct 2026 | Autoresearch plateaus because agents anchor on past attempts. Periodically discarding sessions and history and restarting from the best candidate extends progress (forget +9.2% vs continue +1.4%; DeepSeek V4.1 Flash, 3 runs). | **context-debt (high):** the direct predecessor. Its Pk+∅ reset is the comparison cell. |
| Scaling Discovery Does Not Require Forgetting: Verified Shared Archives (alphaXiv-only, no arXiv ID) | Nov 2026 | Keeping a verified shared archive while capping conversation beats Forgetting-style designs on one take-home task (Claude Opus 5.5, 3 runs). Names the "false floor". | **context-debt (high):** the same qualitative claim (keep scored attempts, drop conversation), as a system comparison rather than a factorial. |
| [Fork-and-Flush](https://arxiv.org/abs/2610.07447) / [Branch-reset, ICLR27 sub 8UgphlLDok](https://openreview.net/forum?id=8UgphlLDok) | Oct 2026; ICLR27 sub (appears to be the venue version) | Independent runs stall in "idea basins". Forking with an inherited workspace and a fresh chat escapes them across 13 multi-day tasks (+66% vs single run). Plateau triggers rarely fired. | **context-debt (high):** exactly our intervention and the anchoring explanation. Autoresearch, not harness meta-agents. |
| [Stateless Language Agents (SLA)](https://arxiv.org/abs/2610.07625) | Oct 2026 | The harness owns research state and builds a fresh context for each call. Checkpoint-continuation method; long horizons. | **context-debt (medium):** another design point, and the source of the checkpoint-continuation method. |
| [Principle-Level Local Optima in LLM Optimizers](https://arxiv.org/abs/2610.24291) | Oct 2026 | A decision tree (narrowing + plateau + gap to reference) finds principle-level collapse in 78% of 161 runs. Diversity injection mostly fails. | **context-debt (medium):** the saturation diagnostic and the matched-diversity motivation. |
| [Lineage Is Memory](https://arxiv.org/abs/2610.41157) | Oct 2026 | 1,750 runs: greedy parent inheritance drives most progress in short-budget GEPA-like loops and beats seed-reset. | **context-debt (medium):** the opposite sign. Harness evolution may never plateau. |
| [When Is Enough Enough?](https://arxiv.org/abs/2610.04756) / [ICLR27 sub 0FaPTsSr9C](https://openreview.net/forum?id=0FaPTsSr9C) | Oct 2026; ICLR27 sub | An anytime-valid restart/stop detector for self-evolving systems. | **context-debt (foundation):** the trigger arm, now downgraded to reporting how often it fires. |
| [HarnessX](https://arxiv.org/abs/2606.14249) | Jun 2026 | A harness foundry with AEGIS trace-driven evolution, +14.5% average across 5 benchmarks. | **context-debt (testbed):** a reported harness-evolution ceiling. |
| [Learning to Repair the Learner](https://openreview.net/forum?id=aEPJT4hW84) — ICLR27 sub | ICLR27 sub | A matched-intervention ladder in neuro-symbolic world models with a code × history crossing: inherited procedure explains 45% of descendant gain. | **context-debt (high):** design precedent for the P0+Mk vs Pk+∅ crossing. |
| [RSI-Harness](https://openreview.net/forum?id=EOrtreQLbv) — ICLR27 sub | ICLR27 sub | A dual-loop RSI in which the improver transfers to unseen models, benchmarks and domains (up to 4.5×) while the harness does not. | **context-debt (residual in Meta-Overfitting):** the opposite sign. The near-domain positive cell must be reproduced. |
| [Improvement by Inheritance](https://openreview.net/forum?id=ZMmJDeDTXk) — ICLR27 sub | ICLR27 sub | Nested replay crosses improvers × start states. Improvers almost never edit themselves (child 0.48 → 0.86, successor flat). | **context-debt residual:** if improvers do not self-edit, P0 ≈ Pk and the contrast is empty. |
| [Removing Stale Skill History](https://openreview.net/forum?id=FZd03GwNex) — ICLR27 sub; [Contextual Drag](https://openreview.net/forum?id=yVo9mX6Bo8) — ICLR27 sub | ICLR27 subs | A 2×2 shows that stale history overrides current evidence in coding harnesses. One wrong draft in context costs ~25 pts pass@1, and the loss compounds over refinement. | **context-debt (medium):** mechanism citations for history-induced harm (task agent, not improver). |
| [A Theory of Reliable Self-Evolution](https://openreview.net/forum?id=NxGP6PGiaU) — ICLR27 sub; [Opinionated Optimizers](https://openreview.net/forum?id=LhhLpXBFgo) — ICLR27 sub | ICLR27 subs | The ceiling is set by verification/evaluation cost. Proposer priors narrow strategies, and selection rarely compounds gains (216 runs). | **context-debt:** competing plateau explanations that must be ruled out. |

---

### Robustness Does Not Assimilate Through the Gate: Harness Evolution, Environment Interventions, and Held-Out-Family Robustness (`nonobv-canalization`) — panel 61.1/100

**Claim.** If the acceptance gate is exposed to environment interventions from k primitive families, evolved harnesses become robust to those families. Robustness to *held-out* families keeps the same negative per-generation slope as clean-only evolution (slope difference ≈ 0). At matched clean accuracy, evolved harnesses of both kinds are less robust than hand-written ones. The claim is falsified if gate-exposed lineages show a significantly less negative held-out slope, which would mean real assimilation.

**Minimal design.**
- GEPA-style evolution with ~6 lineages × 20 generations on a Qwen3-class model.
- tau-world procedural environment as primary; BreakingWeb as a confirmatory subset (~100k browser rollouts).
- Evaluate on paired clean/intervened tasks every 2 generations, using mixed effects with lineage random effects and k≥3 passes.
- Arm A: clean-only gate. Arm B: gate exposed to 3 of the 7 primitive families, with 4 held out.
- Matched-accuracy control: regress robustness on clean accuracy using early checkpoints plus hand-tuned harnesses.
- Secondary test: gating by cognitive primitive vs by surface layer.
- Pilot <$50 (2 lineages × 8 generations); full study ~$0.8–1.1k.

**History.**
- R1 as `breaking-the-improver`: 68.3, rank 9. The directional claim was flagged as pre-empted by IPI and Masked Erosion.
- R2 re-pitch through the canalization/Waddington lens: bold-check 65.8, WEAK. CODA Prop. 3, IPI and Masked Erosion pre-empt "selection manufactures fragility", so it was reframed around held-out assimilation.
- R2 report: 11/11, "merge with breaking-the-improver". The bold-ideas notes still list it with useful-fictions as the "best fit for Xunjian".

**Judge panel** (spread 2.9; the judges broadly agree)

| Lens | N | S | R | C | F | T | Total | Verdict |
|---|---|---|---|---|---|---|---|---|
| AC | 2.5 | 3 | 4 | 2.5 | 3.5 | 3 | 60.4 | fold-into-another |
| Expert | 2.5 | 3 | 4 | 3 | 3.5 | 3.5 | 62.9 | fold-into-another |
| Mentor | 2.5 | 3 | 4 | 2.5 | 3 | 3.5 | 60.0 | fold-into-another |

All three flag K4.

- **AC.**
  - Strongest: the held-out-family transfer test (CODA's stated future work) and the matched-accuracy hand-written control are uncovered, and the ICLR sweep finds no per-generation robustness slope at matched clean accuracy.
  - Weakest: CHASE, Harness-Learns-to-Lie and Feedback Alignment close the remaining gap. What is left is a null (slope difference ≈ 0) from 6 lineages and 30–70-pair cells, which will read as underpowered.
  - Must fix: merge; use TOST with a power analysis that sets the lineage count; add verify-reminder, robustness-aware-gate-without-feedback and CHASE baselines; add a second evolver or backbone; put a real BreakingWeb or WebVariants slice in the primary analysis; preregister per-primitive signs.
- **Expert.**
  - Strongest: a clean paired-intervention test the field has not claimed. The BreakingWeb primitive taxonomy is the one handle CODA, IPI, Masked Erosion and CHASE lack.
  - Weakest: CHASE is essentially an intervention-exposed gate. 5lWjRTK6Vk already shows the false-success slope. Ri0rU3rPOS already argues gains fit environment quirks. The null headline cannot be powered.
  - Must fix: the same list, plus recasting the claim as a transfer ratio that is publishable at any value, and running held-out families on real BreakingWeb pairs.
- **Mentor.**
  - Strongest: the one untested question, falsifiable in both directions, using the mentor's primitive-vs-layer split.
  - Weakest: the headline is crowded. The clean-only negative slope on false success is already published, and tau-world as primary is not the mentor's asset.
  - Must fix: merge; add CHASE and VHE arms; TOST; power analysis from the pilot; a second evolver, or WebVariants as a second intervention source.
- **Skeptical reviewer (AC):** "Once CODA, Masked Erosion, IPI and CHASE are accounted for, the only new claim is a null result on held-out-family transfer, from six lineages of one evolver on a procedural environment, and the paper never shows it had the power to detect the effect it says is absent."

**Feedback received**
- R1 critique → the headline is predictable and pre-empted (IPI, Masked Erosion) → addressed only by the R2 bold-check reframe to held-out assimilation.
- R1 critique → Arm B is tautological without held-out families and cheap baselines (verify-after-acting, a robustness-aware gate) → held-out families added; cheap baselines still missing.
- R1 critique and R2 bold-check → direction risk (evolvers add verification) → per-primitive sign preregistration recommended; not yet in the claim.
- R1 critique → power (cells of 10–30 tasks, effects of 3–8 pp) → mixed effects and k≥3 adopted; held-out cells are still tight.
- R2 bold-check → the biology analogy is a label, not a mechanism → title de-biologized.
- R2 bold-check → the matched-accuracy control is novel → incorporated.
- R2 bold-check → cost → tau-world primary plus a <$50 pilot adopted.
- R2 report → 11/11, merge with breaking-the-improver → no merged v2 exists.
- ICLR27 sweep → weak but not scooped. Must cite ZIBdUe2hGK, uzzum63KP4 and Ri0rU3rPOS; use WebVariants as a test bed. The judges add 5lWjRTK6Vk and C6EElQ2PRG → not yet addressed.

**Bottom line.** Fold it into `breaking-the-improver` as that paper's headline experiment. Do not pursue it as a standalone paper. The experiment is worth doing only as a preregistered *equivalence* test (TOST on the slope difference, lineage count set by the pilot's variance), with CHASE as an arm and real BreakingWeb or WebVariants pairs in the primary analysis. Without those, it is an underpowered null on the mentor's benchmark.

---

### Baldwin Hiding vs. Assist: Where the Harness Helps, Not How Much It Adapts, Decides What GRPO Weights Learn (`nonobv-baldwin-hiding`) — panel 54.6/100

**Claim.** A memory + self-edit harness used during GRPO pushes the weights in opposite directions depending on where it helps:
- If it converts mixed groups to all-success groups (always on), the trained weights do ≥3 pp worse harness-free and under a foreign harness than frozen-harness RL. This is "hiding".
- If it fires only on all-fail groups, the weights do better. This is "assist".
- ΔI, the change in the fraction of informative groups, predicts the harness-free delta (R² ≥ 0.6). Adaptivity adds nothing once ΔI is controlled.

**Minimal design.**
- Qwen3-4B (1.7B for the pilot), verl-agent GRPO on ALFWorld and TextCraft, with τ-world held out.
- Five arms × 3 seeds: frozen; memory-only; self-edit always-on; self-edit gated to all-fail groups; always-on plus DAPO/BAR to equalize ΔI. Plus an importance-corrected harness-free re-scoring control.
- Evaluate each checkpoint three ways: harness-free, under a foreign harness, and in-harness. Compute is matched on env steps, gradient tokens and informative groups.
- Pilot <$100 (arms a, c, d at 1.7B); full study ~$1.5–2.5k GPU.

**History.** Round 2 only (forecast Q3 → NONOBV-7). The original monotone claim ("more adaptivity, less weight learning") was broken because CODA H.7 and HarnessAdapt-RL had already reported hiding, while Harness-S, HarnessAdapt-RL tools and Self-HPO showed assist. It was pivoted to the ΔI two-regime law. Bold-check ~72.5 raw (N3.5 S4 R3.5 C3.5 F3 T4.5), rounded to 71. R2 report #10 at 71, "most counterintuitive".

**Judge panel** (spread 3.4; the verdicts split, see below)

| Lens | N | S | R | C | F | T | Total | Verdict |
|---|---|---|---|---|---|---|---|---|
| AC | 2 | 3 | 3.5 | 3 | 2.5 | 2.5 | 54.6 | pursue-with-fixes |
| Expert | 2 | 3 | 3.5 | 2.5 | 2.5 | 2.5 | 52.9 | fold-into-another |
| Mentor | 2 | 3 | 3.5 | 3 | 2.5 | 3.5 | 56.3 | park |

All three flag K4.

- **AC.**
  - Strongest: a predicted sign crossover under one fixed harness, with a single preregistered predictor, plus arm (e) and the re-scoring control to separate contrast starvation from context-conditional learning. The pilot is cheap.
  - Weakest: TAME owns hiding and rank reversal, AutoScaffold owns assist on ALFWorld, and HarnessAdapt-RL and Harness-S already give the mechanism and the gating. "Of course ΔI predicts it." 3 seeds against 30-pp swings, and R² from ~15 points.
  - Must fix: reposition against the ICLR27 subs and claim that ΔI *explains* TAME's reversal and AutoScaffold's assist (ideally by re-implementing their conditions); fit ΔI out of sample (ALFWorld → TextCraft and τ-world); quantify the arm (e) residual; ≥5 seeds on arms a/c/d; a second model family; ship the pilot before December.
- **Expert.**
  - Strongest: arm (c) vs (d) plus arm (e) is a real mechanistic decomposition that no cited paper runs.
  - Weakest: novelty has collapsed from both sides, and the all-fail-rescue GRPO wave makes assist folklore. The ΔI law restates the DAPO zero-variance argument. 12–15 runs at up to 40 H100-h each do not fit in $2.5k for a solo undergrad.
  - Must fix: drop hiding and assist as findings. The only headline is a residual hiding that survives at equal ΔI (context-conditional learning under RL), preregistered, with a pilot power analysis. Otherwise fold it into a broader harness-weights paper as a mechanism section.
- **Mentor.**
  - Strongest: the only design that holds the harness fixed, varies only where it helps, and predicts the sign from one quantity, which yields a usable design rule.
  - Weakest: the space is saturated (TAME plus five ICLR neighbours, plus the arXiv set), and arm (e) may reduce the paper to a known fix.
  - Must fix: make TAME the primary prior; turn the regression into group- or step-level ΔI with a continuous gate threshold (k/8 sweep) for many dose points; fewer arms and ≥5 seeds; ship the pilot by Jan 2027 and proceed only if the sign flip exceeds the seed spread.
- **Skeptical reviewer (AC):** "TAME already shows harness rank reversal after support removal, AutoScaffold already shows the assist, and Harness-S and HarnessAdapt-RL already show the gated-vs-always-on contrast and the mixed-group explanation, so the remaining contribution is a 3-seed regression of a quantity (informative-group fraction) whose link to GRPO's zero-advantage groups is close to definitional."
- **Disagreement.** The verdicts split three ways (pursue / fold / park), but the totals are close (52.9–56.3). The split is about what to do with a weak idea, not about how weak it is. The mentor lens scores timing higher (3.5) but still parks it.

**Feedback received**
- R2 bold-check → no K1 scoop on adaptivity-as-dose at the time → overturned by the ICLR27 sweep (TAME; rating raised from low-medium to high).
- R2 bold-check → hiding already seen in CODA H.7 and HarnessAdapt-RL → reframed as the controlled, mechanistic version.
- R2 bold-check → assist shown 3 times → pivoted to the ΔI two-regime law.
- R2 bold-check → rescue-fraction regression near-tautological → partly addressed by arm (e); the judges say still not enough.
- R2 bold-check → three-way compute matching → adopted.
- R2 bold-check → seed variance → last-k averaging plus 3 seeds; the judges want ≥5 and a power analysis.
- R2 bold-check → fast-follow risk from the CODA or HarnessAdapt groups → pilot by Jan 2027; not yet scheduled.
- ICLR27 sweep → near-scoop; must cite IXnpSW8g3v, HOk367J7I0, KPRr8wWiqO, l8aeyLANkN, 4KSNZln9nS, FPaMYO47j4, vMD5ZXhd2P, cCqHNd56Pp, AiTVmLVnt1, eFi8mdUJNV, rZ7eO7DZmc → not yet addressed.

**Bottom line.** Park it, or fold it in as a mechanism section of a broader harness-weights analysis. It is GPU-heavy, the ΔI law is near-definitional, and TAME and AutoScaffold own both poles. Revive it only if the <$100 pilot shows a (c)-vs-(d) sign flip larger than the seed spread *and* a non-zero residual at equal ΔI.

---

### Context Debt: Do Harness-Evolving Meta-Agents Plateau From Anchoring on Their Own History, or From Saturation? (`context-debt`) — panel 50.8/100

- **Claim and design.** State components cause different stalls. Conversation and unverified notes cause search-score stalls; the archive and self-modified code cause held-out stalls. The proposed design was a keep/drop factorial forked from a shared plateau checkpoint. Only one residual survives, inside Meta-Overfitting: on a far domain, reset procedure + keep memory (P0+Mk) beats the Forgetting-style reset (Pk+∅), at no extra cost.
- **History.** R1 C5, critique 65 (~76 projected with fixes), never a full proposal. Folded into the R1 merged lead as reset arms. The R2 sharpness reviewer cut 6 arms to 0, leaving only the RQ4 contrast in Meta-Overfitting v2 (77.9).
- **Panel** (spread 0.9; unanimous): AC 50.8 / Expert 50.4 / Mentor 51.3, all fold-into-another. N = 1.5 from every judge; K4 from all three, K1 from Expert and Mentor.
  - Common weakest point: Branch-reset (8UgphlLDok) uses the same intervention and anchoring story. Learning to Repair the Learner (aEPJT4hW84) already has the code × history crossing. RSI-Harness reports positive far transfer. NxGP6PGiaU and FPaMYO47j4 give rival plateau explanations. A plateau with headroom was never shown, and the matched-diversity and saturation controls were dropped.
  - Common must-fix: keep only RQ4 and cite Branch-reset and aEPJT4hW84 as precedent; claim only the far-domain sign flip (preregistered, two-sided); gate on ≥1 accepted improver self-edit per 4 iterations, otherwise P0 ≈ Pk (Improvement by Inheritance); add a matched-diversity control; if cheap, add a search-score vs held-out readout for the two cells.
- **Skeptical reviewer (Mentor):** "Branch-reset already shows that fresh context with an inherited workspace escapes anchoring basins, and prior work already crosses procedure with history. The submission's factorial over which state to keep is therefore a careful ablation of known components, not a new finding about why self-improvers plateau."
- **Bottom line.** Do not revive it as a standalone. Keep it as the P0+Mk vs Pk+∅ cell in Meta-Overfitting, which costs nothing extra. Open question 8 still stands for the meeting: is Xunjian comfortable defending "reset procedure, keep memory" against the Forgetting authors?

---

## Appendix: known gaps & caveats

- **Meta-Overfitting's closest-paper table is missing verified competitors.** Add these rows:
  - Hyperagents 2603.19461. It is the base system behind ZMmJDeDTXk, IDM and SelfImprove-Bench, and it has no row of its own.
  - SelfImprove-Bench 2611.02797. It is cited in the text but has no row. The factcheck numbers are 1.326 vs 1.207, and the top scorer retains 0% Meta edits.
  - LLM Agents Are Not Always Faithful Self-Evolvers 2601.22436. leads-a calls it "must cite; method template" for the placebo/shuffled-memory arms.
  - RSIFlow 2610.21791. Its Meta0–Meta4 same-start comparison gives +20.56 on Code (single run), and its backbone threshold matters for the open-model improver.
  - MetaSkill-Evolve 2607.05297.
  - EvoPathBench 2609.24663. It finds that candidates gain held-out but the selected updates fall short.
  - 2609.17817 (Reflections on Trusting Trust, Revisited). It is the actual source of the "open models can't drive Hyperagents" correction, and of the Polyglot git-history answer leak. That leak bears directly on any Gödel-lite or DGM-derived lineage.

- **Daggers are wrong on papers that leads-b already verified.** MetaExp 2610.21492 is listed as "MetaExp†, must be read", but leads-b read it. It shows experience-learning transfers when trained into weights, and that untrained backbones get worse with experience (−2.2 for Qwen3.5-122B). That is a direct Decay-arm competitor and belongs in the table as verified. Dream-RSI 2609.14858 is daggered in the world-model table, but leads-b §1.6 verified it. Harness-Delta 2610.22466 is daggered in the selection table and undaggered in the ECD table. RSEA 2606.28374 is marked "†, not fetched", but leads-a L2 checked it.

- **Learning-to-goodhart's table misses named competitors** from fresh-scoop.md and leads-a/b:
  - PRIME 2606.09711, which fresh-scoop calls "the closest prior for exploit-seeking as a transferable skill", a weight-space retargeting result.
  - RHO 2606.05922, rated medium for L2G.
  - Found by Search, Broken by a Shortcut 2611.04838, a pre-registration and tamper-test template.
  - RSI-Forge 2610.09426, a shortcut-family testbed.
  - CATCH 2609.39533, which the expert judge asks to benchmark against but which has no row.

  Biddulph & Carroll is still unread. It is the gate for the idea's main claim, and its Nov 20 deadline is not in the Next-two-weeks plan.

- **The other topics have gaps too.**
  - *Blind-Spot Hiding:* Aspire 2608.31111 is cited in feedback but has no row. R²Orch 2610.22740 is the closest structural overlap (an outer loop edits the task-proposal objective), and A-RSI 2610.38568 and EAU 2610.16523 are also missing; all three are in fresh-scoop. Feedback Dynamics 2608.02636 and SWE-Duel 2610.12569 are flagged "not cited". "When a Fixed Task Generator Falls Behind" is still unverified.
  - *Taught, Not Selected:* the user-simulator fidelity cluster (MIMESIS 2610.09484, 2610.24294) and EAU are missing. Fresh-scoop names MIMESIS as the main residual risk of someone running the causal fidelity intervention. AdvSim2Real and DarwinX are admitted missing from v2 and are still not framed.
  - *Self-knowledge (#3) and self-forecast:* S3Gym 2608.31100 is missing. Its null is ρ(self-judging accuracy, gain) = −0.010 over 98 runs.

- **The core evidence against the #2 claim was read from abstracts only.** RSI-Harness's "up to 4.5×" transfer, and the MetaEvoForge, Looped-RSI and Meta-TTL claims, come from abstract-only sweep reads. The leaderboard drop from 77.9 to 72.1 and the whole "bounded rebuttal" framing rest on these claims, so read RSI-Harness and ZMmJDeDTXk in full before reframing. Two other unread papers carry weight: 2611.07624 and 2610.14662, which ECD lists as "must be read before OSF". Other unverified IDs that drive verdicts: the subliminal-learning cluster (behind the safeguards K4), the all-fail-GRPO rescue IDs (behind Baldwin's "folklore" verdict) and 2609.36535† (behind PG's "decay is default"). QDdWiaNkk2 is "possibly = 2607.12227†", and that paper drives the headroom kill risk for #1.

- **The #1 pilot in "Next two weeks" contradicts the #1 design section.**

  | | Pilot (Next two weeks) | Design section |
  |---|---|---|
  | Tasks | Existing τ²-bench retail, about 60 + 100 | Procedural, deterministic τ²-style DB, 150/50/300 |
  | Depth | 20 | 40 |
  | GPU budget | Q2 says about 200 H100-h | About 250 GPU-h |

  The pilot's must-fix list also leaves out the fix all three judges call first priority: an RS variant with real traces stripped of error text (or a "sim trace + real error message only" cell), plus a second agent model. Without that control, a positive pilot cannot tell "taught" apart from "better feedback helps".

- **Dates and timelines conflict.**
  - The ECD pilot is "Nov 16–23" in its own section and "Week 2, Nov 20–27" in the plan. The mentor's must-fix is "confirm GPUs before Nov 23".
  - The L2G pilot also runs Nov 16–23. A solo undergrad would be running three pilots at once.
  - Preprint dates: the TL;DR and plan say February. ECD says Feb 15, evolve-in-dream's feedback says "~Mar 5", and L2G says "around Mar 1".
  - Paper dates that do not match their arXiv IDs: IDM 2610.13991 is listed as "Sep 2026", DarwinX 2608.07545 as "Jul 2026" and 2610.00221 as "Sep 2026".

- **Cross-references inside the report are stale.**
  - Context-debt cites "Open question 8" on reset-procedure/keep-memory, but Q8 is about a second model family, and that question is not in the list at all.
  - Parametric Gödel cites "question 7 for Xunjian" (executor swap), but Q7 is the February preprint.
  - The TL;DR says "12 of 23 ideas got fold/park from at least two judges". The leaderboard gives 14: #3, 10, 11, 12, 13, 14, 15, 17, 18, 19, 20, 21, 22, 23.

- **Fold targets go in circles or disagree.**
  - Canalization goes to breaking-the-improver, breaking-the-improver goes to useful-fictions, and useful-fictions' fallback goes back to breaking-the-improver/canalization.
  - Cross-play folds into "learning-to-goodhart or curriculum-goodhart", but the TL;DR fold map does not include it.
  - Interference-plateau folds into Meta-Overfitting in its bottom line but is missing from the TL;DR list of ideas folding into #2.
  - Improver-erosion's verdict is "fold" from all three judges, but its bottom line says "park".
  - The drift barrier's bottom line calls it "a credible backup to the lead". That clashes with the TL;DR naming Meta-Overfitting as the backup.

- **Number contradiction on verification cost.** The improver-erosion note says "verification is 2–7% of cost", while the RSI Tax row (2611.13524) says verification is 75–94% of RSI token cost. Reconcile them, or say which premise was mis-cited.

- **Some judge scores look miscalibrated.**
  - **Parametric Gödel (#4, 68.6)** ranks above Blind-Spot Hiding (#5) and has the "most consistent" spread. Yet it has F = 2–2.5, no proposal ever written, no R2 red team, unconfirmed GPUs, $2–4k, and an AC note that the sweep "never queried LoRA terms". The tight spread reflects no scrutiny, not robustness.
  - **#1 Taught, Not Selected:** the expert and mentor give N = 4 even though VHE partly overlaps and 2609.32452 and 2610.14662 are marked high-threat fast-followers.
  - **#2 Meta-Overfitting:** R = 5 from all three judges (weight 2) offsets opposite-sign evidence. The 1.0-point gap between #1 and #2 is inside both spreads (6.2 and 6.7), so the ranking is effectively a tie.

- **Practical details the student needs are missing.**
  - ECD needs 10 lineages × k* = 12 accepted improver self-edits. At the GO threshold of 1 per 4 iterations, that is about 480 iterations, and ZMmJDeDTXk says self-edits are rare. The $1.4–1.7k budget does not show this arithmetic.
  - There is no stated decision rule if both pilots fail. Learning-to-goodhart and the drift barrier are not ranked as third options.
  - SelfSearch's "introspective drift" add-on (leads-b idea A) is in the scoop table but in no design or plan.
  - The ICLR27 links were "not re-fetched". Spot-check at least the five scoop-watch IDs before citing them in a pre-registration.

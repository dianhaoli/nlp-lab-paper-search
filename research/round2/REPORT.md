# Round 2: Final report on NeurIPS 2027 ideas for self-referential improvers

*2026-11-12 · Dan (mentor: Xunjian Yin). Written by an editor with a 3-person panel: a rigor-focused AC, an expert in self-improving agents, and a pragmatic undergrad advisor. Inputs: `verify/`, `redteam/`, `proposals/*.v2.md`, `bold_ideas.md`, `bold_checks/`, `field_forecast.md`.*

---

## 1. TL;DR

**Recommended paper: *Meta-Overfitting: Self-Referential Improvers Get Better at Improving Only Where They Grew Up*** (v2 score 77.9)

- **Claim.** A Gödel-style improver makes self-edits that raise its home-domain improvement rate. On distant domains, those same edits make it *worse* than its own gen-0 procedure at improving a fresh agent. The deficit survives a memory wipe and fact-scrubbing of its code, and it grows with domain distance, turning negative between near and far domains.
- **Why it is novel now.** Many groups now evolve the evolver: MetaRSI, RSIFlow, FreeEvolve, HMED and SelfSearch. They all assume improver edits are domain-general. None tests whether improvement skill transfers once memory is controlled. The field forecast predicts a powered improver × state *swap* paper by May (75%). Our Decay claim is the part that wave will not reach.
- **Why it is sharp.** It is one directional, pre-registered number, D_far < 0, at the lineage level (n=10). It has a gate (home gain ≥ 3 pp) and a dose-response. If true, it gives a design rule that reverses current practice: reset the *procedure*, keep the *memory*. Both alternatives are pre-registered and publishable: "domain-general meta-learning" (D_far ≥ 0) and "procedures are memory in disguise" (κ ≥ 0.7).
- **Prior evidence it is real.** FreeEvolve's Haiku evolver scored 52.2 on its evolution set but 32.2 held-out. RSI-Opt's held-out transfer peaks at generation 1.

**Backup: *Blind-Spot Hiding*** (curriculum-goodhart v2, 78.3). Give a meta-agent edit access to its own task writer and it steers away from the rule types its agent fails on. The agent then ends worse than a yoked twin trained on the identical task stream. Its score is numerically a tie with the recommendation. It ranks second because its mentor fit is slightly lower and because Double Ratchet (2607.12790) makes the null "harness learners ignore curriculum content" a likely outcome.

**What changed vs round 1**
- **Lead paper.** Round 1's three-part merged paper (75.8) is cut to the Decay claim alone. The red team found three half-papers. SelfSearch 2609.37968 Table 4 now holds a crude version of the Cause arm, and MetaRSI holds Effect.
- **Curriculum-goodhart** rose from 72.1 to 78.3. It became one signed mechanism claim tested with a yoked control.
- **Evolve-in-dream** pivoted to "taught vs selected", because DRPE 2609.32322 pre-empts the old "type not rate" headline.
- **Learning-to-goodhart** fell to 71 after the weight-level twin 2602.12124 surfaced.
- **Seven bold ideas were checked.** Six are promising (71–74.6) and one is weak.

---

## 2. What round 2 verified or overturned

**OpenReview could not be checked.** api2.openreview.net was blocked by the proxy, and openreview.net and neurips.cc did not resolve. OpenReview-only ICLR 2027 submissions remain **UNCHECKED**, the largest open scoop risk. As a fallback we searched for the ICLR template header and found 6 ICLR 2027 arXiv submissions: R2Flow, skill overfitting 2609.39148, Autogenesis, SWE-Duel, OpenForge RL and SciSlop. All are low threat.

**Fact-checks**
- **SelfImprove-Bench 2611.02797 was misquoted in round 1.**
  - The like-for-like editable vs fixed improver comparison is 1.326 vs 1.207. The 1.433 figure belongs to an agent that kept **zero** improver edits.
  - 87.9% of retained edits touch prompts or memory, and 0% touch the improver unless improver editing is forced.
  - This is the lead paper's top feasibility risk, and the pilot gates on it.
- **"Open models can't drive self-modifiers" is too strong.** It holds only for the Hyperagents scaffold. DGM, SICA, VERSE, MetaRSI and HMED all run on open models.
- **MetaRSI 2609.06396 is a stronger threat than round 1 said.** It has a same-start swap (+7.2 vs +4.9) and 5 seeds. It has no off-diagonal grid and no cross-domain test.
- **HMED 2610.07979:** branch credit and matched Δ disagree in sign in 45–75% of revisions. So Pk must be the pre-registered incumbent, not the archive best.
- **Other checks.** FreeEvolve's weak-evolver result is overfitting, not stalling. The VERSE, Forgetting and RSIFlow numbers verified. DGM/Hyperagents Polyglot staging had a git-history answer leak, which is a cheap audit target.

**New scoops and must-cites**

| Paper | Hits | Severity |
|---|---|---|
| SelfSearch 2609.37968 | Cause arm (2-cell procedure-vs-memory ablation, unpowered) | High for Cause; low for Decay |
| RSI-Opt 2610.32026 / Organon 2610.33046 | Decay. RSI-Opt: gen-1 transfer peak, home-model penalty. Organon names cross-task carryover as future work | Medium |
| MetaExp 2610.21492 | Decay framing (weight-level experience transfer) | Medium |
| DRPE 2609.32322, Dream-RSI 2609.14858, 2610.08033 | Evolve-in-dream v1 | High for v1; v2 survives |
| 2602.12124, Biddulph & Carroll (abstract only), PRIME 2606.09711 | Learning-to-goodhart | High for surprise |
| Double Ratchet 2607.12790, 2611.06965, TURTLE 2610.27307 | Curriculum-goodhart | Medium |

No idea has a K1 kill.

---

## 3. Final ranked table (all live ideas)

| # | Idea | Score | Status | One-line verdict |
|---|---|---|---|---|
| 1 | **Meta-Overfitting** (effect-cause-decay v2) | 77.9 | **Recommended** | Sharpest claim with the best mentor fit (R5). Feasibility hinges on getting enough improver edits. |
| 2 | **Blind-Spot Hiding** (curriculum-goodhart v2) | 78.3 | **Backup** | Tied on score. The yoked design is strong, but the null may be the likely outcome. |
| 3 | Taught, Not Selected (evolve-in-dream v2) | 76.3 | Go-with-changes | Clean 2×2 of trace channel vs fitness channel. World-model fit. Medium scoop risk. |
| 4 | Interference plateau ★ | 74.6 | Promising | "Plateaus are masked, not missing." Edit-graft and epistasis matrix, rebase vs tip. |
| 5 | Useful fictions ★ | 74.2 | Promising | Belief state becomes optimistic under outcome-only selection. Needs a random-acceptance control. |
| 6 | Drift barrier / error threshold ★ | 74 | Promising | Editor defects ratchet in below gate resolution. Law-like and cheap. |
| 7 | No privileged self-access | 73 | Promising / fold | Expected result is null. Better as a memory-transplant arm of #1. |
| 8 | Cross-play collusion | 72.9 | Promising | Verifier co-evolution audit in the hottest lane. Scoop risk medium-high. |
| 9 | Learning-to-goodhart (text vs weights) | 71 | Go-with-changes | Viable. Gated on reading Biddulph & Carroll. |
| 10 | Baldwin hiding/assist ★ | 71 | Promising | Most counterintuitive idea, but needs a GPU and agentic RL seeds are noisy. |
| 11 | Canalization | 65.8 | Weak | Framing is pre-empted by CODA and Masked Erosion. Merge with breaking-the-improver. |

★ = non-obvious, low-scoop lens (see §5).

---

## 4. Recommended paper: plan

**Core questions**
1. **Decay (primary).** At a fixed checkpoint k\* of accepted improver edits, does the memory-wiped, scrubbed procedure Pk^s improve a fresh agent on a far domain *less* than P0 does?
2. **Dose.** Is D_B monotone in a pre-registered A→B distance, and does it cross zero? This would reconcile the result with VERSE G.2, where methodology transferred to a near domain.
3. **κ.** What share of the home gain disappears when domain facts are scrubbed from the evolved code? κ ≥ 0.7 means "memory in disguise".
4. **Design rule.** On a far domain, does P0+Mk (reset procedure, keep memory) beat Pk+∅ (forget memory, keep procedure)?
5. **Exploratory.** Do introspective drift and editor-defect load predict D_far?

**Headline:** D_far < 0, with the lineage-level CI excluding 0. **Alternatives:** D_far ≥ 0 means the first controlled evidence of domain-general procedural meta-learning; κ ≥ 0.7 means procedures are memory in disguise.

**Minimal design**
- **System.** Gödel-lite, stripped from Xunjian's code. Improver is an open ~30B model; executor is 8–9B. 10 lineages on home domain ALFWorld.
- **Arms.** 5 arms: P0+∅, Pk^s+∅ (primary), Pk+∅, P0+Mk, Pk+Mk. Each runs on a fresh executor, with improver calls matched across arms.
- **Controls.** Scrubbed P0, length-padded P0, and a leak judge that audits the scrubber.
- **Far and near domains.** ScienceWorld, SI-Bench games and Reasoning Gym.
- **Statistics.** OSF pre-registration, lineage-level analysis, MDE about 4 pp.
- **Budget.** About $1.5k API.
- **Cheap add-ons on the same runs.** Introspective-drift index, home-model penalty (swap the executor), and a per-region editor-vs-task defect audit (a drift-barrier mini-test).

**Pilot (Nov 16–23, ≤ $50).** 2 lineages to 6 accepted improver edits, then probes on ALFWorld and Reasoning Gym with R=3.
- **GO** if all hold:
  - at least 1 accepted improver edit per 4 iterations
  - home gain of Pk ≥ 3 pp in both lineages
  - leak judge ≤ 40% on the 6-way task
  - ≤ $2.5 per probe
- **PIVOT once:** switch to a DeepSeek V4 Flash improver, force improver-module edits, or move to a home domain with more headroom.
- **KILL** if unscrubbed Pk still misses the 3 pp gate after the pivot. Fall back to Blind-Spot Hiding or Taught-Not-Selected, which reuse the same infrastructure.

**6-month timeline**

| Dates | Milestone |
|---|---|
| Nov 13–23 | Gödel-lite plus wrappers; pilot; read 2610.14662, 2610.31436, 2611.07624; check OpenReview by hand |
| Nov 24 – Dec 20 | Remaining domain wrappers, scrubber, leak judge, distance metric; **OSF frozen Dec 20** |
| Dec 21 – Feb 10 | 10 lineages to k\*; probes on home and far domains |
| **Feb 15** | **arXiv v1** (primary result), ahead of ICML-cycle follow-ups |
| Feb 15 – Mar 31 | Near and mid domains (dose); VERSE-port replicate; add-on readouts |
| Apr 1 – May 15 | Analysis and writing; code release; Xunjian review; submit |

---

## 5. Where the field is moving

**Trend table (condensed)**

| Lane | Trajectory | Saturation | Our ideas |
|---|---|---|---|
| "RSI" label | ~40× in 9 months (2.3 → 96 papers/mo) | Exploding | All ride it |
| Harness-evolution methods | 26 → 161 papers/mo | Saturated | Avoid methods |
| Improver-level / evolve-the-evolver | Accelerating (MetaRSI, Evo², HMED) | Crowding | #1 is *just ahead* of it (Decay) |
| RSI audits and benchmarks | Saturating | High | Bare swap test is *riding* the wave and is late |
| Agent plasticity | Doubling each quarter | Just starting | #1 is meta-level; actor-level is gone |
| Verifier co-evolution / reward-free | Hottest method lane | Very high | Cross-play collusion is *riding*; it is late by summer |
| Simulator fidelity | Spiking (Oct cluster) | Medium | #3 is ahead *for harnesses* only |
| Reward hacking in loops | Re-accelerating | Medium-high | L2G is riding; crowded |
| Self-generated curricula | Flat | Saturated for methods | #2 is *ahead*: an audit in a quiet lane |
| Bio and economics lenses | Metaphor only | Barely started | ★ ideas are *ahead* |

**Predicted by May 2027** (judgment, not calibrated):
- a powered improver × state swap paper (75%)
- an RSI workshop or benchmark cluster (85%)
- agent plasticity as a named subfield (70%)
- first critiques of verifier collusion and drift (65%)
- causal simulator-fidelity studies in RL (55%); in harness evolution (30%)
- harness-gain replication disputes (70%)
- evolutionary-theory predictions still untested (75%)

**Non-obvious, low-scoop-risk ideas, with scoop risk**

| Idea | Lens | Scoop risk | Note |
|---|---|---|---|
| Drift barrier / error threshold (74) | Muller's ratchet under a noisy gate | **Low** | Law: L* moves with gate size n; patch and rewrite editors differ. Directly tests Gödel Agent's ceiling. 2608.05810 already has a knee. |
| Interference plateau (74.6) | Fitness-landscape epistasis | **Low-medium** | Watch DarwinX, RRSI and the Winner's-Curse authors. |
| Useful fictions (74.2) | Adaptive vs neutral drift of beliefs | **Medium** | The audit-of-self-evolution space is moving fast. |
| Baldwin hiding/assist (71) | Baldwin effect | **Low-medium** | CODA and HarnessAdapt-RL could fast-follow. GPU needed. |
| Introspective drift (add-on to #1) | Self-directed edits | **Low** | Costs about nothing on #1's runs. |
| Home-model penalty (add-on to #1) | Co-evolutionary lock-in | **Low-medium** | Hinted at in RSI-Opt's tables. |
| Feedback-bits ceiling, Goodhart decomposition, control-theoretic oscillation | Information theory, Manheim–Garrabrant, control | **Low** | Unchecked seeds (forecast Q5, Q9, Q12). |

---

## 6. Biggest remaining risks

1. **Improvers barely self-edit.** SelfImprove-Bench found 0% improver edits retained. Mitigation: the pilot gate, and forced improver-edit proposals.
2. **The 75% swap-paper scoop absorbs Decay.** Mitigation: preprint by Feb 15. The anchor is Decay plus κ, not the swap.
3. **OpenReview is unchecked.** Allowlist openreview.net and api2.openreview.net, or search by hand this week.
4. **Single improver family.** Mitigation: the 4-lineage VERSE-port replicate.
5. **Scrubber validity.** If the leak judge exceeds 40%, κ is uninterpretable.

**Weekly scoop-watch (every Monday)**
- **Forward citations of:** SelfSearch 2609.37968, MetaRSI 2609.06396, VERSE 2610.02616, RSI-Opt 2610.32026, Organon 2610.33046, Agent Plasticity 2610.08902, 2610.27610, FreeEvolve 2610.09197, HMED 2610.07979.
- **alphaXiv/S2 queries:**
  - "improver transfer cross-domain self-referential"
  - "meta-overfitting self-improvement"
  - "evolved optimizer held-out domain"
  - "warm-start self-improving agent new domain"
  - "procedure vs memory self-improving"
  - "self-generated curriculum blind spot / proxy truth"
  - "epistasis harness edits"
- **Sources to recheck:** NeurIPS 2026 workshop accepted lists after Dec 4 (Meta-Agents, AIWILD, TTCL, Verify-Agents); ICLR 2027 OpenReview.
- **Reads due:** Biddulph & Carroll (by Nov 20), 2610.14662, 2610.31436, 2611.07624.

---

## 7. Open questions for Xunjian

1. Can we fork Gödel Agent into Gödel-lite? In his experience, how often does it edit its *own improver* rather than prompts or memory?
2. Recommendation or backup? Do Blind-Spot Hiding or the drift barrier excite him more?
3. Is a Feb 15 preprint acceptable before NeurIPS submission?
4. How should A→B domain distance be defined and pre-registered? Options: embedding, skill overlap, or human rating.
5. Does the lab have GPUs to serve the 8–9B executor and ~30B improver locally?
6. Does he know of ICLR 2027 submissions (OpenReview-only) on improver transfer or self-written curricula?
7. Should the BreakingWeb or WeAct infrastructure supply a far domain or the executor-swap arm?
8. Is the "reset procedure, keep memory" design rule a claim he is comfortable defending against the Forgetting 2610.20563 authors?
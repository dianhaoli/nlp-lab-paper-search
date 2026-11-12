# Bold check: sharp-interference-plateau

*"RSI plateaus come from interference, not exhaustion."* Skeptical area-chair check, 2026-11-12.

**Verdict: PROMISING, about 74.6/100. Not killed, but the claim as written is not yet sharp enough.**

- **No K1 scoop.** I found no paper that builds an edit-level graft or epistasis matrix inside a self-improvement lineage, and none that tests a rebase onto an ancestor at matched compute.
- **Two problems with the framing.**
  - *Prior assertions and operators.* The interference story has already been stated in passing, and merge/prune operators already exist: DarwinX, Harness-S and RRSI.
  - *A false dichotomy.* "Interference vs. exhaustion" leaves out at least two rival accounts that the literature has already put forward: selection noise / winner's curse, and proposer blindness.
- **Fix.** Reframe the paper as a **three- or four-way decomposition of the plateau** that uses evolutionary-genetics tools. The key move is to separate *global (headroom) epistasis* from *idiosyncratic (pairwise or sign) epistasis*. That framing is less likely to be scooped, and every outcome is publishable.

---

## 1. Scoop searches run

| # | Channel | Query / target | Relevant hits |
|---|---|---|---|
| 1 | alphaXiv (recency) | epistasis / interference among accepted edits in self-improving agents | Artifact-to-Capability 2611.04952, Does Failure Feedback Direct 2610.14662, Scope Before You Persist 2609.29144, **RRSI 2609.24972**, **RSI on a Budget 2610.29121**, SkillNEO 2610.20926, Third Eye 2611.12988 |
| 2 | alphaXiv (recency) | why harness evolution / RSI plateaus; leave-one-out ablation; non-additive edits | HDA 2610.22466, Beneath the Surface 2611.07624, **Harness Evolution Hits a Ceiling 2610.11655**, Masked Erosion 2610.38392, **Harness-S 2611.04277** |
| 3 | alphaXiv (recency) | path dependence, recombination/crossover, accumulated edits, bloat | **Winner's Curse 2610.09239**, Causal Improvement Graph 2610.05039, RSI Tax 2611.13524, HarnessPro 2610.26770 |
| 4 | Semantic Scholar (5 queries) | epistasis self-improving agent edits; non-additive interaction; RSI plateau interference; rebase/merge lineage; edit-interaction ablation | **Mendel Gödel Machine 2608.07645**, **MESH-Harness 2610.05300**, **DarwinX 2608.07545**, HGM-Dual 2610.30042, Phantom Guardrails 2607.13083 |
| 5 | S2 forward citations | Harness-S, Masked Erosion, Winner's Curse, DarwinX | Harness-S and Masked Erosion have 0 citations so far. Winner's Curse is cited by 2611.09699. DarwinX is cited by 16 papers, including **Cortex 2610.14216** (prune by removal) and HarnessFlow 2611.06254; none of them does in-lineage epistasis. |
| 6 | S2 | leave-one-out ablation, non-additive; complexity accumulation ceiling; replay accepted edits onto base | Nothing relevant. The only adjacent hit is "Audit the Scaffold, Not the Checkpoint" 2609.34924, which I did not open. |
| 7 | WebSearch (OpenReview / ICLR 2027) | self-improving agent edits epistasis interference plateau | No ICLR 2027 OpenReview submission found. The hits point back to DarwinX and RRSI. |

## 2. Deep reads: the papers that constrain the claim

### DarwinX 2608.07545 (Salesforce, Jul 2026). Closest framing threat.
- **Same motivation.** It states the motivation almost verbatim: *"single-lineage self-editors are biased by early edits and plateau [SICA]"* plus *"cross-task interference: an edit that fixes one family of tasks silently regresses another."*
- **It already has the operator.** It merges additive edits from a common ancestor, `H = H0 ⊕ Δ`, and keeps the merged child only if it covers the union of its parents' wins. That is close to your "rebase" arm.
- **What it does not do:**
  - It never measures epistasis.
  - It never grafts a lineage's own late edits back onto gen-0.
  - It admits that *"the contribution of recombination relative to single-lineage mutation still requires controlled ablation."*
  - On WebArena-Infinity, *every merge was reverted*. That is negative evidence for naive rebasing.
- **Consequence.** "Use a merge/rebase operator" is **not** novel as a remedy. What is novel is the mechanistic diagnosis of where single-lineage plateaus come from.

### Harness-S 2611.04277 (SJTU, 3 Nov 2026)
- **Interference is asserted, not tested.** It says *"even compatible edits can interfere with one another. A single accumulated design can therefore lose the strengths of its constituent variants."*
- **What it actually measures is sibling-level:** repaired vs. regressed tasks per sibling, oracle-union coverage, and pairwise repair-overlap (about 62%).
- **No in-lineage analysis:** no graft, no knockout, no additive-vs-pairwise model. It is a method paper about RL rescue training.
- **Consequence.** It supplies the *conjecture* that your paper would test. Cite it as motivation.

### Other papers that change the design
- **Winner's Curse 2610.09239.** *"Most proposals after the first are harmful"*; late commits are mostly false positives; *lock-in*: the incumbent's selection inflation acts as an implicit threshold. This is a **third account of plateaus** (selection noise) that the original falsification criterion ignores.
  - Late accepted edits may have about zero true effect everywhere. Then "graft on gen-0 vs. in-lineage" would be 0 vs. 0, which the TOST would read as "exhaustion" even though the real cause is noise.
- **Harness Evolution Hits a Ceiling 2610.11655.** Two findings matter:
  - *Positive* epistasis: the ledger and lessons components are each about null alone, but give +0.076 together.
  - It attributes the loop's ceiling to the **proposer's evidence package**: the proposer cannot see the cause of F4 failures. That is a **fourth account** of plateaus ("blindness").
- **RRSI 2609.24972 (Google, very visible, 372 votes).** Names "complexity accumulation" as a failure mode. Its L1-style **pruner removes components with no recent positive contribution**, and its annealed sparse edit budget makes edits attributable.
  - This partially occupies the "remove late interference" remedy.
  - It does not measure interaction terms and does not explain plateaus.
- **Mendel Gödel Machine 2608.07645.** Uses genetics vocabulary, including "cross-lineage hybridization". Its theory explicitly **assumes an additive fitness landscape**. Your result would directly test that assumption.
- **MESH-Harness 2610.05300.** Recombines module implementations under a **linear (additive) LinUCB surrogate**. RSI on a Budget 2610.29121 likewise scores diffs with a linear readout. If interference dominates, these surrogates are misspecified, which gives a concrete downstream implication.
- **Cortex 2610.14216.** Leave-one-out pruning of accumulated skills and rules. Its prune verdicts came out right on only 6 of 8 items, and on only 2 of the 4 planted ones. That is single-item removal, not interaction.
- **Masked Erosion 2610.38392.** Redundancy (subset structure) among safeguards, which is a form of epistasis among *safety* items. It establishes that gate-based loops hide interactions. Cite it as an analogue.

**Gap that survives:** no paper puts numbers on edit × background interaction along a self-improvement lineage, and none compares rebase vs. tip vs. bag-of-edits at matched compute.

## 3. Area-chair critique of the claim as written

1. **False binary.** "Interference vs. exhaustion" will be rejected by any reviewer who has read 2610.09239 or 2610.11655. A plateau can come from:
   - (a) proposer exhaustion;
   - (b) selection noise / winner's curse: late commits have true effect about 0, and good late proposals are rejected because of lock-in;
   - (c) interference among accepted edits;
   - (d) proposer blindness, i.e. what the evidence package shows;
   - (e) headroom / saturation.

   The falsification rule as written assigns (b) and (e) to "exhaustion".
2. **Global vs. idiosyncratic epistasis (the non-obvious hole).** In experimental evolution, a beneficial mutation is reliably worth more on an unfit background. This is *diminishing-returns epistasis* (Kryazhimskiy et al. 2014; Wünsche et al. 2017), and it is mostly explained by **one global nonlinearity**: fewer failures remain to fix. It is not explained by specific pairwise clashes.
   - Your signature 1 ("a late edit is worth more on gen-0") is predicted by headroom alone.
   - A paper that does not fit a global-epistasis model (additive latent + nonlinear link, e.g. a per-task logit) before claiming pairwise interference will be called confounded.
3. **Grafts often won't apply.** Late code edits call functions that earlier edits created (positive or sign epistasis by construction). Possible fixes:
   - LLM "porting" of an edit, which adds a confound;
   - restricting to self-contained edits, which biases the sample.

   Recommendation: report the applicability rate as a first-class result (dependency structure). Use semantic porting only as a secondary arm, with a porting-fidelity check.
4. **Gödel-specific subtlety.** In a Gödel-lite lineage, some edits change the *improver*, not the task policy. Effects of improver edits appear only in later generations, so graft-to-gen-0 scored on task performance mis-measures them.
   - Split edits into task-level vs. meta-level, and score meta-level edits by their effect on the *next k generations' acceptance rate*.
   - This is also what makes the paper Xunjian-specific.
5. **Power.**
   - A 15×15 pairwise matrix gives 105 pairs. At 3 seeds on a pool of a few hundred items, pair interactions of a few points are hard to resolve.
   - Fit a regularized model with a pre-registered variance-share estimand (as you planned), and use the knockout-of-k design (random subsets, like DMS "combinatorial libraries") rather than exhaustive pairs.
   - Note that the stated "<10% of total gain" threshold is not defined yet: share of variance of what? Pre-register it as the variance share of pairwise terms in held-out predictions of subset fitness.

## 4. Sharpened claim (proposed)

> **Late-generation self-improvement gains are masked, not missing.** Along Gödel-style lineages, the held-out effect of an accepted edit shrinks with the depth of the background it lands on. After de-noising (fresh re-evaluation) and after a global-epistasis link removes headroom, a substantial share of that shrinkage is *idiosyncratic* epistasis among accepted edits (pairwise and sign). It is not selection noise and it is not saturation. **Consequence:** replaying a de-noised, interaction-aware subset of the lineage's own accepted edits onto an ancestor ("rebase") beats the lineage tip at matched compute, and additive-surrogate methods (MGM theory, MESH's LinUCB, linear diff readouts) are misspecified exactly when lineages plateau.

**Pre-registered 4-way decomposition.** Each outcome is publishable.

| Account | Signature | Falsifier |
|---|---|---|
| Noise / winner's curse | Fresh-pool effect of late commits ≈ 0 *in-lineage and on gen-0* | ≥ 50% of gen≥10 commits have a fresh-pool effect > 1 SE on their parent |
| Global epistasis (headroom) | A single nonlinear link of additive edit scores predicts subset fitness (pairwise terms add < 10% held-out R²) | Pairwise / idiosyncratic terms add ≥ 10% held-out R² |
| **Idiosyncratic interference (headline)** | Sign epistasis present; rebase of top-m edits > tip at matched tokens | Rebase ≤ tip (TOST ±1pp) and pairwise share < 10% |
| Exhaustion / blindness | Late proposals have low *potential* effect even on gen-0; the proposal-quality curve declines | Graft-to-gen-0 effect of late *proposals* (including rejected ones) is not lower than that of early ones |

The last row uses a trick worth keeping: graft **rejected** late proposals onto gen-0 as well. If they would have helped there, the proposer is not exhausted, and its edits are being masked or mis-selected. That one control separates exhaustion from the rest cleanly.

**Non-obvious extensions (less likely to be scooped):**
- *Historical contingency / "replaying the tape"* (Lenski-style). Restart lineages from gen-k checkpoints with new seeds, and ask whether late gains are reachable only through specific early edits (potentiation). This is a direct analogue of Blount et al. 2008. Nobody in RSI has run it.
- *Epistasis of the improver.* Meta-level edits that make the improver better (Gödel Agent's core claim) may be exactly the ones with the strongest sign epistasis. This gives a mechanistic account of Gödel Agent's complexity ceiling.
- *Surrogate misspecification.* Show that MESH-style or linear-readout predictors lose rank correlation as lineage depth grows, in proportion to the measured idiosyncratic share. This is a cheap, broadly relevant corollary.

## 5. Feasibility

- **API cost.** The stated ~$600–900 is plausible with cheap executors (Qwen3.5-class or Flash-class) on 2 domains.
- **Recommended changes:**
  - **2 domains** (one code/agentic, one reasoning or τ-style), to answer the "single benchmark" objection;
  - **random-subset knockout libraries** (about 200 subsets of the top-15 edits per lineage) instead of exhaustive pairs;
  - **fresh paired pool** of 300 or more items.

  Realistic total is about $1–1.5k.
- **Pilot.** Doable in a week for under $100: 2 lineages × 15 generations, grafts only.
- **Main engineering risk.** Getting diffs to apply on gen-0. Log edits as structured patches, not free rewrites, from the start.

## 6. Rubric scores

| Dim | Weight | Score | Reason |
|---|---|---|---|
| N Novelty | 3 | 3.5 | The diagnostic (graft matrix, global vs. idiosyncratic epistasis, rebase at matched compute) is un-run. The narrative is already *asserted* (DarwinX, Harness-S), and merge/prune remedies exist (DarwinX, RRSI, Cortex). |
| S Surprise / falsifiable | 2.5 | 3.5 | As written it is binary and confounded (noise, headroom). The sharpened 4-way version is strong: every cell is publishable, and "late gains are masked" flips how people read flat curves. |
| R Mentor relevance | 2 | 4.5 | It is about long-horizon self-improvement, Gödel Agent's ceiling and meta-level edits. |
| C Competitiveness / rigor | 2 | 3.0 | Good controls are possible, but the result risks being benchmark-bound and graft-applicability adds confounds. Needs 2 domains plus the global-epistasis baseline to clear "engineering". |
| F Feasibility | 1.5 | 4.0 | Cheap models work, the cost is mostly evaluation and the pilot is quick. |
| T Timing | 1 | 4.5 | RSI-plateau and harness-evolution skepticism is the hottest Q4-2026 thread. |

**Total = 20 × (3·3.5 + 2.5·3.5 + 2·4.5 + 2·3.0 + 1.5·4.0 + 1·4.5) / 12 = 20 × 44.75 / 12 ≈ 74.6** (viable; it reaches strong if the 4-way decomposition and the contingency/replay arm are adopted).

**Scoop watch:**
- **DarwinX team.** They named "controlled ablation of recombination" as future work.
- **RRSI team.** Their pruner already logs component credit, so an epistasis analysis is one appendix away for them.
- **The Winner's-Curse authors.** They explicitly "do not model the dynamics of lock-in".

Move fast on the pilot.

## Sources
- DarwinX https://www.alphaxiv.org/abs/2608.07545
- Harness-S https://www.alphaxiv.org/abs/2611.04277
- RRSI https://www.alphaxiv.org/abs/2609.24972
- Harness Evolution Hits a Ceiling https://www.alphaxiv.org/abs/2610.11655
- Winner's Curse https://www.alphaxiv.org/abs/2610.09239
- Mendel Gödel Machine https://www.alphaxiv.org/abs/2608.07645
- MESH-Harness https://www.alphaxiv.org/abs/2610.05300
- RSI on a Budget https://www.alphaxiv.org/abs/2610.29121
- Causal Improvement Graph https://www.alphaxiv.org/abs/2610.05039
- Cortex https://www.alphaxiv.org/abs/2610.14216
- Masked Erosion https://www.alphaxiv.org/abs/2610.38392
- HDA https://www.alphaxiv.org/abs/2610.22466
- Web search summary pointing to DarwinX / RRSI: https://huggingface.co/papers/2608.07545.md , https://www.emergentmind.com/papers/2609.24972
- Unverified (cited from memory, not opened here): Kryazhimskiy et al. 2014 (global diminishing-returns epistasis); Wünsche et al. 2017; Blount et al. 2008 (historical contingency).

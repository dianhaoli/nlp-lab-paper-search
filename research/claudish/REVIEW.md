# Claudish Snowball: review

*For Dan · 2026-11-13 · Source idea: /home/user/nlp-lab-paper-search/research/claudish/idea.md. The literature sweep, red team and judges are summarised below. arXiv ids link to https://www.alphaxiv.org/abs/<id> and OpenReview ids to https://openreview.net/forum?id=<id>. A † means only the title or snippet was seen.*

## Verdict in 5 bullets

- **Scoop status: part of it is already published.**
  - **RQ1 (does every model do it?)** is about 70% done by Clark, Martin & Mahowald, **2610.29162** (Oct 2026). They cover 14 models including Opus 5 and 5.5, find base < instruct < reasoning, and use a meaning-matched control.
  - **The coarse part of RQ4 (post-training causes it)** is shown by 2610.29162, 2605.30526 (PASTA), 2611.01549 and 2609.32196.
  - **"Unreadable to humans, readable to models"** is shown for *engineered* text by BabelTele **2606.19857**: human QA drops from 56% to 36% while model QA goes from 90% to 97%.
  - **Still open:** the **snowball (RQ2)** in human–model chat, and the **three breaking points on an organic trajectory (RQ3)**.
  - **Watch out:** 2610.29162 names a reader study as its next step, so that group could take the human side of RQ3 before May.
- **Biggest strength.** The question is timely and nobody has answered it. Can ordinary, successful conversation push shared language out of what humans can read while agents keep working? That is the CoT-legibility worry moved into user-facing dialog. The design that measures C_H*, C_M* and C_U* on the same samples is clean and falsifiable.
- **Biggest risk: the snowball may be manufactured or may not exist.**
  - A user simulator told to reuse the model's terms puts the register in by construction. With an LLM simulator, reviewers will call it LLM–LLM attractor self-play (2606.30571).
  - Real data points the other way. Real users converge only weakly (2605.29278), models over-accommodate to *users*, and traits tend to plateau (2605.01130).
  - If loop gain is ≤ 1, the result is "persistence", which is a weak NeurIPS paper.
  - The name "Claudish" also invites construct-validity and "renaming a known phenomenon" objections. It is awkward too, because 2610.29162 finds Opus 5.5 *lower* than Opus 5.
- **Best venue.** As written, this is an **ACL/EMNLP (ARR) or COLM 2027** paper: a register, a human study and a Chinese replication. It becomes a credible NeurIPS paper only if it is reframed as **an in-context feedback loop**, with three pieces:
  1. loop gain g > 1 against controls;
  2. one dissociation figure where C_H* is well below C_U*;
  3. one causal post-training knob on OLMo, plus LEDOM forward vs reverse bits per byte (bpb).

  If the main asset ends up being the corpus plus the scorer, NeurIPS Datasets & Benchmarks is the fallback.
- **Where it ranks.** EV is **8.4%** (P(exec) 0.5 × novelty factor 0.70 × P(acc|done) 0.24). That ties #15 of 20 on the leaderboard (Useful Fictions, 8.4%) and sits about 4.5 pp below Taught-Not-Selected (12.9%) and about 3 pp below Forced-Quota Dose (11.2%). Its P(acc|done) of 0.24 is below every idea on the board (0.26–0.33), because the judges see it as an ACL-shaped paper aimed at NeurIPS. Caveat: different judge runs scored this idea and the others, so compare roughly. A 1–2 pp gap is noise, but a 4 pp gap probably is not. Honest read: as a NeurIPS bet it is mid-to-low tier. As an EMNLP/COLM paper it is much stronger.

## Closest existing work

| Paper | What it shows | RQ touched | Threat |
|---|---|---|---|
| [2610.29162](https://www.alphaxiv.org/abs/2610.29162) Clark, Martin, Mahowald, "Why is AI-generated language so hard to understand?" | 14 models overuse features that make human processing hard. The effect grows base → instruct → reasoning. A GRPO length-penalty run causes the shift. No reader study and no snowball. | RQ1, RQ4 | **High.** It is the paper reviewers will compare against, and the group may add the reader study. |
| [2606.19857](https://www.alphaxiv.org/abs/2606.19857) BabelTele, "LLMs Do Not Always Need Readable Language" | Prompted compressed text: human QA 56% → 36%, model QA 90% → 97%. PPL rises 9.6 → 177. Works for agent memory and multi-agent tasks. | RQ3, RQ4 | **High** for the headline claim; the text is engineered, not organic. |
| [2606.30571](https://www.alphaxiv.org/abs/2606.30571) Ko & Geiping, "Attractor States in Multi-Turn LLM Conversations" | Self-play converges to model-specific attractors. Claude Haiku's meta-commentary spreads to its partners. | RQ2 (LLM–LLM) | **Medium.** This is what an LLM-simulator snowball will be read as. |
| [B3Iq9eKyiz](https://openreview.net/forum?id=B3Iq9eKyiz) "Mirroring Is Not Sycophancy" (ICLR'27 sub.) | In 25k real chats, users adopt model vocabulary above a chance baseline, and adoption predicts preference votes. | RQ2 (human uptake) | **Medium.** Static uptake, no escalation. Useful for calibration. |
| [2605.29278](https://www.alphaxiv.org/abs/2605.29278) "Accommodation Goes Both Ways" | On WildChat, models over-converge to users; humans converge to models only at human–human rates. | RQ2 | **Counter-evidence.** Could damp any snowball. |
| [2605.01130](https://www.alphaxiv.org/abs/2605.01130) "Iterative Finetuning is Mostly Idempotent" | Seeded traits mostly decay or plateau across generations. | RQ2 | **Counter-evidence** for runaway amplification. |
| [2605.30526](https://www.alphaxiv.org/abs/2605.30526) PASTA (EMNLP'26) | Post-training adds an AI-like style. An aligned-minus-base direction, ablated at decode time, removes it across 11 models. | RQ4 | **High** on the coarse claim. Its direction is also the causal knob we can borrow. |
| [2611.01549](https://www.alphaxiv.org/abs/2611.01549) "Revisiting LLM Homogeneity" | 25 metrics over 18 LLMs. Model-specific conventions emerge in SFT and are reshaped by preference optimisation. | RQ1, RQ4 | Medium |
| [2609.32196](https://www.alphaxiv.org/abs/2609.32196) "Is the Judge Its Twin?" | OLMo-2 and Zephyr base → SFT → DPO: each stage makes the writing more predictable to other models. | RQ4 (PPL) | Medium. Close to an unconditional G_M. |
| [2609.01491](https://www.alphaxiv.org/abs/2609.01491) GlossoGen | Agents invent languages humans can't read, and task success rises, but only under efficiency pressure. | RQ2, RQ3 | Medium. Claudish's angle is "no pressure needed". |
| [wY0JSEIbKB](https://openreview.net/forum?id=wY0JSEIbKB) CoGENT (ICLR'27 sub.) | Self-coined terms *improve* multi-agent accuracy and cut tokens 56%. | RQ3 | Medium. Turns "coined labels" into a real tension worth testing. |
| [2510.27338](https://www.alphaxiv.org/abs/2510.27338) "Illegible CoTs" (NeurIPS'25) | RL makes CoT illegible to humans while it still works (Claude is the exception). | RQ3 framing | Low–medium |
| [2311.07484](https://www.alphaxiv.org/abs/2311.07484) Kuribayashi et al. | Instruction-tuned surprisal fits human reading worse than base surprisal. | RQ4 (PPL) | Medium. "PPL ≠ human naturalness" is old news. |
| [PTic6zZAch](https://openreview.net/forum?id=PTic6zZAch) "Expression Style-Induced Bias in LLM Agents" (ICLR'27 sub.) | Changing only the user's style shifts agent task success. | RQ3 (C_U*) | Low. It means non-inferiority is not a foregone result, which is good. |
| [2507.01335](https://www.alphaxiv.org/abs/2507.01335) LEDOM; [wjO8F2KPxN](https://openreview.net/forum?id=wjO8F2KPxN) Tide (ICLR'27 sub.) | LEDOM is a right-to-left LM (Xunjian's). Tide finds LLM text gets disproportionately harder to predict in reverse. | RQ4 | **Opportunity**: nobody has applied a reverse LM to register. |

## What reviewers will say

**Strengths**
1. The question is timely and matters for oversight. It extends legibility and monitorability concerns from CoT to shared human–model language.
2. A register snowball in human–model dialog, measured by onset, slope, plateau and ceiling, is genuinely unclaimed.
3. Measuring three breaking points on the same samples, plus the history-ablation grid, is a clean and falsifiable causal design.

**Weaknesses and fixes**
1. **"This is 2610.29162 plus multi-turn" and "BabelTele without the prompt."**
   *Fix:* Demote RQ1 to a calibration section that reuses 2610.29162's features and round-trip-translation control. Make RQ2 and RQ3 the paper. Add a head-to-head arm: BabelTele-style engineered compression versus organic text at matched C.
2. **The simulator manufactures the effect.**
   *Fix:* Define loop gain **g = ΔC(model, t+1) / ΔC(injected by user, t)** and claim a snowball only if g > 1. The primary condition is a **scripted template user** with a known dose and no register of its own. Compare it against four controls: no-uptake, semantic-only uptake, constant-dose and length-matched natural context. Calibrate uptake rates to WildChat (2605.29278, B3Iq9eKyiz). Add a 40-person Prolific real-human loop as an existence check.
3. **C(x) is not yet a construct.** The signatures pull in opposite directions (compressed versus over-structured), they overlap with length, jargon and Biber Dim-1, and coined labels plus compressed reference are what humans normally do when building common ground (Clark & Wilkes-Gibbs 1986).
   *Fix:*
   - Factor C(x) into 3–4 pre-registered subscales.
   - Anchor it on about 3k Bradley-Terry pairwise human judgments and report reliability.
   - Show incremental validity beyond length, Biber, a detector score and the 2610.29162 features.
   - Measure the discourse signatures as excess over a human–human common-ground baseline, using insider raters (with history) and outsider raters (without).
4. **"Readable to models" is circular, and humans and models see different contexts.**
   *Fix:*
   - Use a cross-family reader matrix, so the generator never reads its own text in the headline numbers.
   - Generate comprehension questions from a hidden fact ledger with exact-match answers.
   - Give humans and models the same context.
   - Run τ²-bench as a wording-only replay, with each rewrite certified by executable task contracts (2610.14477).
   - Pre-register the margins and use ε = 10 pp. Paired non-inferiority at 5 pp needs about 470 episodes per cell, which is unaffordable.
   - Run the dense C-curve on open agents and use the API for only 2 C levels.
5. **G_M is "in-context learning works" and the PPL critique is old.**
   *Fix:*
   - Test an interaction instead: does post-training raise the *in-context slope* of G_M? Use the OLMo Base → SFT → DPO → RLVR ladder, minimal-pair single-feature edits, length control (PriorBPB) and continuations from a held-out family.
   - Add one causal knob, either PASTA-direction ablation or a small DPO, and show it moves g.
   - Add LEDOM reverse bpb.
   - The concrete PPL claim is the **"PPL inversion"**: along a single trajectory, the model's bpb falls while measured human reading cost rises.

## Revised one-pager

## Register Snowball
**Do post-trained assistants amplify their own register in ordinary conversation until humans, but not models, stop understanding?**

*Suggested name:* use "register snowball" or "assistant register" in the paper, and mention "Claudish" once as the folk term. Report Claude as one of about 6 families and let the data decide which family has the highest gain.

**RQs**
1. Under realistic user uptake, does an assistant's register amplify turn by turn (loop gain g > 1), or does it only persist (g ≤ 1)?
2. Along that organic trajectory, which breaks first: human comprehension (C_H*), cross-family model comprehension (C_M*) or agentic task success (C_U*)?
3. Does post-training cause the gain, and does the model's likelihood keep favouring the register (falling bpb) while human cost rises?

**Core idea.** Post-training sets a register prior: abstraction density, over-structuring, and conversation-local coinage with compressed back-reference. In a cooperative loop, users echo some of the model's wording and the model conditions on its own history. If the model adds more register than it was fed (g > 1), the language drifts out of the region humans can read while staying inside the region where models still work. Perplexity and LLM judges would then certify the drift as fine. This is an oversight and evaluation problem, not a stylistics complaint.

**Experiments**
- **Measure C(x).** Build 3–4 subscales anchored on about 3k Bradley-Terry pairwise human judgments over meaning-matched twins. Make twins two ways: a HIP base-model paraphrase and human edits on a gold subset. Distil the judgments into an open scorer that is never an LLM judge from a scored family. Validate on held-out families and topics. Measure discourse coinage as excess over a human–human common-ground baseline.
- **Snowball (primary).** A scripted template user keeps a fixed intent script and inserts items extracted from the model's last turn at a controlled dose. Conditions:
  - full loop
  - no uptake
  - semantic-only uptake
  - constant dose
  - length-matched natural context

  Run 15–20 turns across about 6 families, with open models on the GPU. Fit C_model(t+1) = a + b·C_user(t) + g·C_model(t) + h·len(t), with conversation random effects. Set the dose from WildChat uptake rates.
- **Robustness and reality checks.**
  - Cross-family LLM simulators, one of them a base model.
  - A Prolific human-in-the-loop arm with about 40 people × 10 turns.
  - Mining of real long sessions from ContextEcho (ICLR'27 geTr6F4o1U).
- **Breaking points.** Take checkpoints at C ∈ {0, q25, q50, q75, max}.
  - **C_H*:** measured reading time plus ledger-based comprehension, with humans and models given equal context.
  - **C_M*:** a cross-family reader matrix scored on ledger QA and plain-language reconstruction.
  - **C_U*:** τ²-bench wording-only replay with contract-certified rewrites and pass^k.

  Fit 4-parameter-logistic dose-response curves with a shared normalised 25% margin, then run a joint bootstrap over the ordering. Two side arms: BabelTele engineered text at matched C, and sharded multi-turn tasks (2505.06120) to separate register effects from "lost in conversation".
- **Mechanism.**
  - Measure g and minimal-pair G_M at each OLMo Base → SFT → DPO → RLVR stage, scored with and without the chat template. Pre-2023 base models serve as the AI-free anchor.
  - Causal knob: ablate the PASTA aligned-minus-base direction, or run a small DPO toward or away from the register, and check whether g crosses 1.
  - Compute LEDOM forward vs reverse bpb along trajectories.
- **Cut.** RQ1 breadth becomes one calibration table. The Chinese replication goes to an appendix or a later ACL paper. Adaptive search survives only as an "inducibility ceiling" footnote.

**What each result means**
- g > 1 for post-trained models but not base models, with C_H* well below C_U*: the full claim. Successful conversation makes assistant language less human, and post-training is why.
- g ≤ 1 but a clear dose-response: "persistence, not snowball". The paper becomes an ACL/EMNLP dissociation paper, still publishable if pre-registered.
- The cross-family reader advantage vanishes: "self-familiarity, not model-readability". This is still a useful negative result about PPL and LLM judges.
- C_U* < C_H* (agents break first, as PTic6zZAch hints): also interesting. Register is then a robustness problem, not an oversight problem.

**Contributions.** (1) A human-anchored, open register scorer with discourse subscales. (2) A snowball protocol with loop-gain estimation and dose-matched controls. (3) The first three-threshold dissociation on organic text, with non-inferiority tests. (4) Post-training stage attribution plus one causal knob, and the LEDOM "PPL inversion". (5) A released trajectory corpus.

**Central claim.** Under realistic user uptake, post-trained assistants amplify their own discourse-level register with loop gain above 1, and base models do not. Text crosses human comprehension thresholds while cross-family model understanding and τ²-bench success stay non-inferior, and the model's bits per byte on that text keep falling. Low perplexity therefore stops being evidence of naturalness.

**Biggest risk.** The loop gain may stay at or below 1 under realistic uptake, since real users converge weakly and traits plateau. Second, the Clark group may publish the reader study first. Mitigations: pre-register the g ≤ 1 outcome as a reportable result, start IRB now, and post a preprint around Feb 2027.

**Pilot (1 week, about $80 API plus 1 GPU-day, no IRB needed).**
- **Snowball setup.** Scripted template user with 3 models: OLMo-3-Instruct, Qwen3-class instruct, and one API model. 15 turns × 30 conversations per condition × 4 conditions: full loop, no uptake, constant dose, length-matched natural.
- **Interim C.** An automatic proxy: the 2610.29162 feature set, Biber Dim-1, and coined-label reuse rate.
- **Quick check.** 5 lab members do 100 outsider pairwise comparisons on turn-1 vs turn-15 excerpts.
- **Mechanism check.** G_M on 200 minimal pairs across the OLMo Base/SFT/DPO checkpoints.

| Outcome | Condition |
|---|---|
| **GO** | g > 1 (CI excludes 1) in ≥ 2 of 3 models, above the length-matched control; and lab raters prefer turn-1 over turn-15 ≥ 70% of the time; and G_M rises across stages. |
| **PIVOT** | g is 0.8–1 with a clear constant-dose response. Re-scope to the ACL/EMNLP dissociation paper (RQ3 plus the PPL inversion), or raise the uptake dose to the top of the WildChat range and retest once. |
| **KILL** (as a NeurIPS bet) | C does not exceed the length-matched control, or raters cannot tell turn 1 from turn 15. Hand it back to Xunjian as a COLM or workshop measurement study. |

## How it could connect to Dan's agent ideas

Mostly it doesn't, and it should not be bundled. The board shows merges lower EV. There are two honest, cheap links:
- **Register drift in self-improving agents.** GEPA-style reflectors repeatedly rewrite harness prompts and memories from their own traces, which is a model-only version of the snowball. With the C(x) scorer, Dan could measure for free whether evolved harness text in Taught-Not-Selected or GodelLite logs becomes more assistant-register across generations. If it does, there is a follow-on oversight question: can a human still audit an evolved harness that the agent runs fine? That would be a workshop note or appendix, not a main paper.
- **Same moral as Taught-Not-Selected.** Both ideas are about what models read from their own history shaping what they do next. If Dan does Taught-Not-Selected, the trace-hygiene framing and the "history rewritten as natural" ablation here are conceptually shared, but the two are separate experiments.

## Questions to ask Xunjian

1. Is the goal NeurIPS specifically, or is ACL/EMNLP or COLM 2027 fine? As written, the judges think it fits there much better.
2. Are you attached to the name "Claudish" and to Claude-specificity, or is a neutral term ("register snowball" or "assistant register") OK? 2610.29162 finds Opus 5.5 is less dense than Opus 5.
3. Which of the four RQs do you care most about? Can RQ1 breadth and the Chinese study be cut to an appendix or a follow-up?
4. Did this come from specific long Claude or agent sessions? Do you have logs we could mine as real-world evidence of drift?
5. Can the lab run LEDOM-7B plus the OLMo stage ladder on our GPUs, and would you own the mechanism and PPL section?
6. Is about $1–1.5k of the $3k OK for human data (Prolific pairwise judgments plus a small real-human loop)? Who handles IRB, and can we start it now?
7. Are you in touch with the Clark, Martin & Mahowald group? Should we aim for a Feb 2027 preprint to set priority?
8. Should this be Dan's main project, or a second project alongside the agent line (Taught-Not-Selected sits at 12.9% vs 8.4% here)?

Key inputs: /home/user/nlp-lab-paper-search/research/claudish/idea.md, /home/user/nlp-lab-paper-search/research/ACCEPTANCE_RANKING.md, /home/user/nlp-lab-paper-search/research/ONE_PAGERS.md.

---

# Appendix A: literature sweep

## style

Scope: about 30 searches across Semantic Scholar (keyword searches plus forward-citation expansion of Reinhart et al. 2410.16107, Idiosyncrasies 2502.12150 and "Delve" 2412.11385), alphaXiv discover (2 broad runs), and ICLR 2027 OpenReview /notes/search (18 terms). I read the abstract of every paper in the table; a † means I saw only the title or snippet. Links: arXiv ids resolve at https://www.alphaxiv.org/abs/<id>; OpenReview ids resolve at https://openreview.net/forum?id=<id>.

**Bottom line.** The broad phenomenon is now heavily covered. It has names already: "AI-like writing style", "slop", "synthetic English", "LLM idiolect", "lexical misalignment". It has been measured with Biber MDA, MMD, excess vocabulary and slop taxonomies. Several 2026 papers already show it is caused or amplified by post-training. The specific Claudish claim is still open: a graded register of **conversation-local coinage, compressed anaphora and abstraction density that snowballs over multi-turn interaction**, with C_H*/C_M*/C_U* breaking points. The most dangerous neighbour is **Clark, Martin & Mahowald, 2610.29162 (Oct 2026)**. It covers multi-turn chat with simulated users, nine frontier models including Claude Opus 5 and 5.5, processing-difficulty features, base→instruct→reasoning ordering in OLMo, and a causal length-penalty RL proof of concept. It does **not** test snowballing, does not measure human comprehension, and does not look at task robustness.

### Key papers

| id / link | when | summary | overlap with Claudish RQ1-4 |
|---|---|---|---|
| **2610.29162** "Why is AI-generated language so hard to understand?" (Clark, Martin, Mahowald) | 2026-10-21 | 14 models (9 frontier, incl. Claude Opus 5/5.5; OLMo base/instruct/think) in chat with simulated users, agentic coding, and essays. They overuse processing-difficulty features compared with round-trip-translation paraphrases and human text: low-frequency n-grams, inanimate subjects with animate verbs, stacked nouns, long dependencies, reduced object relatives. Effects grow base→instruct→reasoning, and GRPO with a length penalty causes the shift (it transfers to untrained alternations). | **RQ1 partly, RQ4 partly.** Closest scoop on "dense, hard to read, but grammatical", with a post-training/RLVR info-density story. No snowball (RQ2), no measured comprehension or task breaking point (RQ3), no PPL preference G_M. The paper says itself that reader studies are needed. |
| **2605.30526** "Measuring, Localizing, and Reducing the AI-Like Writing Style of Post-Trained LLMs" (EMNLP 2026) | 2026-05-28 | Base vs aligned generations from matched human prefixes. Aligned text has lower human-corpus affinity and higher detector rates. PASTA ablates an aligned-minus-base residual direction during decoding, which lowers detection across 11 models and 6 detectors. | **RQ4 largely scooped** for "post-training introduces AI-like style", including a mechanistic localization. Uses detectors as the measure, not register or readability, and has no multi-turn component. |
| **2611.01549** "Revisiting LLM Homogeneity Through a Multidimensional Lens" | 2026-11-01 | 25 interpretable metrics over 18 LLMs. From 2024 to 2026 models become more like themselves and less like each other. Model-specific conventions emerge in SFT and are reshaped by preference optimization. | **RQ1/RQ4 partly.** Directly supports "model-specific register set in post-training" (SFT, then PO). Single turn only. |
| **2605.23651** "How Human-Like Are LLMs? A Register-Aware Linguistic Evaluation Framework" | 2026-05-22 | Biber's 67 features plus MMD against human-human CIs, across 5 registers and 7 open instruct models. All models deviate. They over-use nominalizations, present-participle clauses and attributive adjectives, are less "involved" in conversation, show lower variability, and cluster by family. | **RQ1 partly.** A ready-made measurement tool for the register-distance part of C(x). Instruct models only, no base models, no dynamics. |
| **2410.16107** Reinhart et al. "Do LLMs write like humans?" (PNAS) | 2024-10 (2025 pub) | Biber features: instruction-tuned models write a noun-heavy, informationally dense style even when asked for informal registers. Base Llama is closer to human. | **RQ1/RQ4 precedent** for "nominalization and abstraction density come from instruction tuning". This is the canonical citation. |
| **2412.11385** "Why does ChatGPT 'delve' so much?" (COLING 2025) | 2024-12 | Identifies 21 focal excess words and rules out architecture and data as the cause. Model comparisons are consistent with RLHF playing a role, and a user study gives mixed results. | RQ4 lexical-level precursor. |
| **2508.01930** "Word Overuse and Alignment in LLMs: Influence of LHF" | 2025-08 | Llama base vs LHF. Emulating LHF shows annotators prefer variants containing the overused words. | RQ4: lexical overuse comes from preference data. |
| **2606.03165** and **2606.00334** Lexical Alignment Score + Triangulated Preference Shift (LREC / FLAIRS 2026) | 2026-05/06 | Curation-free metrics for overused items and the share attributable to the preference stage, across 6 families. | RQ4 tools for the lexical layer (borrow these). |
| **2605.25358** "AI-Associated Lexical Shifts Across 34 Languages" | 2026-05-25 | GPT-4.1 overused lemmas converge across languages ('emphasize'-type verbs in 24/34). News text takes them up after 2022 in 26/34 languages. | Supports the Chinese replication; the lexical layer is cross-lingual. |
| **2603.27006** "The Last Fingerprint: How Markdown Training Shapes LLM Prose" | 2026-03-27 | The em dash is markdown leaking into prose. Across 12 models from 5 providers it survives instructions to drop markdown and even explicit prohibition. A base-vs-instruct comparison shows a latent pre-RLHF tendency that post-training amplifies, and rates vary by provider. | RQ1/RQ4 for the over-structuring signature. Single turn. |
| **2605.27878** "Narrative Flattening" (EMNLP 2026) | 2026-05-27 | Compares OLMo-32B Base, SFT, DPO and RLVR checkpoints on matched fiction continuations. Post-training compresses thematic, affective and stylistic variation, and post-trained endpoints converge across domains. | RQ4 design template: same-lineage checkpoints isolate post-training. |
| **2609.32196** "Is the Judge Its Twin?" | 2026-09-26 | OLMo-2 and Zephyr, base→SFT→DPO. The writing becomes more predictable (another family finds it 7-25% less surprising per token), but as judges the models barely come to prefer predictable writing. | **RQ4 PPL angle**: cross-model surprisal of post-trained style. Directly relevant to G_M and to the "PPL in the AI era" claim. |
| **2605.06030** "More Aligned, Less Diverse? Grammar and Lexicon of Two Generations" (ACL 2026) | 2026-05-07 | HPSG analysis of news text. Newer instruct LLMs have lower syntactic and especially lexical diversity than older non-instruct models. | Supports the RQ4 ordering "older < base < post-trained". |
| **2605.07632** "Post-training makes LLMs less human-like" | 2026-05-08 | Psych-201 benchmark: post-training lowers behavioral alignment with humans across families, and the gap widens in newer generations. | Same RQ4 narrative, but behavioral rather than linguistic. |
| **6IstnZXa0a** (ICLR 2027 sub) "Turing-Test Gap in Preference Alignment" | 2026 | Theory and DPO experiments: preference alignment lowers the likelihood of human responses even when all data is human. | RQ4 theory: a formal reason post-training moves the register off the human distribution. |
| **2510.01171** Verbalized Sampling (typicality bias) | 2025-10 | Mode collapse is driven by annotators' typicality bias in preference data. | RQ4 mechanism candidate (data-level). |
| **URQZKM8CLa** (ICLR 2027 sub) "Trait Hitchhiking in LLM Post-Training" | 2026 | A Price-equation model of how RL amplifies traits it does not target. | RQ4: a formal account of how a register prior can be amplified by RL. |
| **B3Iq9eKyiz** (ICLR 2027 sub) "Mirroring Is Not Sycophancy: Style Convergence and Position Change in Human-AI Conversation" | 2026 | In 25k real conversations, users adopt the model's vocabulary above a same-question chance baseline, and the amount of adoption predicts their preference votes. In a scripted audit of 16 models, the user's emotional register shifts every model's style. | **RQ2 partly.** Shows human lexical uptake in real conversations (one arm of the snowball loop). It does not track escalation over turns or readability decay. |
| **2409.01754** Yakura et al. "Empirical evidence of LLM influence on human spoken communication" | 2024-09 | Podcast synthetic control shows "delve"-class words rising after ChatGPT. A preregistered experiment (N=496) shows that a brief chatbot interaction makes participants adopt its words, and the effect persists. | RQ2: causal human uptake within a single interaction. |
| **2508.00238** Juzek-lab "Traces of AI-associated language in unscripted spoken English" (AIES) | 2025-08 | 22M words of podcast speech: LLM-associated words increase after 2022 and synonym baselines do not. | RQ2 (population-level uptake). |
| **2502.12150** "Idiosyncrasies in LLMs" (ICML 2025) | 2025-02 | Embedding classifiers attribute ChatGPT, Claude, Grok, Gemini and DeepSeek text at 97%. The signal is rooted in word distributions and survives rewriting and translation. | RQ1: model-specific registers exist and are separable. |
| **2610.28434** "Every Model Leaves a Trace: Multi-Resolution LLM Fingerprinting" | 2026-10-20 | Fingerprints persist down to distilled and LoRA variants and across languages, and survive cue suppression and paraphrase. | RQ1 (model-specificity). |
| **2608.06589** "Beyond 'AI Language': the idiolectal nature of LLM output" | 2026-08-06 | 2024 and 2026 cohorts each have unique per-model profiles, with a generational shift (contractions range from 1.2k to 30k per million words). | RQ1 framing: "AI language" vs model idiolect. Claudish is the idiolect claim for one model family. |
| "Synthetic English and LLM-mediated writing: research agenda for register variation and enregisterment" (English Today; no arXiv) | 2026-11-04 | Proposes the label "synthetic English" for recurrent LLM-associated distributions and an agenda on register and enregisterment. | **Naming precedent**: "LLM register" is now named. Claudish has to position itself as a model-specific, interaction-amplified subtype. |
| **2509.19163** "Measuring AI 'Slop' in Text" | 2025-09 | Expert-interview taxonomy of slop and span-level annotation. Binary slop judgments are somewhat subjective but correlate with coherence and relevance. | C(x) definition precedent built on human judgment. |
| **2510.15061** Antislop (ICLR 2026) | 2025-10 | Profiles model-specific slop against human baselines (some patterns are over 1000x more frequent). Introduces the Antislop sampler and FTPO, which cuts slop 90% without hurting GSM8K or MMLU. | RQ3-adjacent (removing style does not hurt tasks). Tool for building matched natural/Claudish pairs. |
| **2609.33905** SlopBench | 2026-09-27 | 18 models, 4 surface slop behaviors. Rankings are unstable under reweighting, and arena results, detectors and lexical diversity do not confirm the order. | Warning for RQ1: composite style scores rank models unreliably, so use human pairwise grounding as Claudish plans. |
| **2409.14509** "Can AI writing be salvaged?" (CHI 2025; LAMP corpus) | 2024-09 | Professional writers' 7-category idiosyncrasy taxonomy (clichés, unnecessary exposition, ...) and 1,057 edited paragraphs from GPT-4o, Claude-3.5-Sonnet and Llama. | Source of natural rewrites and paired data for C(x). |
| **2610.23715** "Cognitive Cost of LLM Writing: Reading-Time Comparison" | 2026-10-16 | RoBERTa trained on eye-tracking predicts higher per-word reading time for LLM rewrites (longer, rarer words) and lower total reading time. | RQ3 C_H proxy (predicted reading time, not measured). |
| **2604.03136** StoryScope | 2026-04-03 | Discourse-level narrative features attribute among 6 sources. Claude shows "notably flat event escalation". | Claude-specific style evidence at discourse level. |
| **2403.07183**, **2404.01268** Liang et al. (peer reviews / papers) | 2024 | Corpus-level MLE of the LLM-modified fraction based on adjective/word shifts in ICLR and other reviews and in papers. | Lexical-fingerprint background. |
| **2610.26008** "Accelerated drift of scientific prose" (EMNLP 2026) | 2026-10-19 | Interrupted time series: after 2022, longer words, lower readability, shallower dependency trees. Some markers then recede. | Population-level background. |
| **Zbw6oh8WqK** (ICLR 2027 sub) "AI-Likeness in the Peer-Review Ecology" | 2026 | Across 150k ICLR reviews, higher AI-likeness goes with higher ratings but less reviewer updating. | Peripheral (ecology). |
| **6rg4RWzNun** (ICLR 2027 sub) ECHO: "Common-mode hidden offsets drive vocabulary collapse in PO" | 2026 | A shared hidden-state offset during preference optimization builds a "vocabulary well". Found in 14 of 17 lineages, and present at low level in textbook DPO. | RQ4: a mechanistic route from PO to lexical concentration. |
| **2312.01552** URIAL / "Unlocking Spell" | 2023-12 | Base and aligned models differ mainly at stylistic token positions. | RQ4 baseline method (token distribution shift on stylistic tokens). |
| **2409.11704** "From Lists to Emojis: Format Bias" | 2024-09 | Reward models and humans favor lists, bold, links and emojis, and policies exploit this. | RQ4: an over-structuring source. |
| **2610.02873** ConvoDrift | 2026-10-02 | Dataset of style drift under fixed intent across 6 turns, driven by user preference changes. | RQ2 adjacent. It is user-driven tone drift, not model register amplification. |
| 2604.14111† "Interpretable Stylistic Variation in Human and LLM Writing Across Genres, Models, and Decoding" | 2026-04 | Title only (abstract not retrieved). | Possible RQ1 overlap; check. |
| 2605.19936† "What Are LLMs Doing to Scientific Communication? Writing Practices and Reading Experience" | 2026-05-19 | Snippet only: measures changes in NLP-paper style and the reading experience. | Possible C_H overlap; check. |

### Does this scoop any RQ?

- **RQ1 (do all models show Claudish):** **Partly scooped.** It is established that every instruct model deviates from the human register in the same direction: noun-heavy and dense (2410.16107, 2605.23651, 2610.29162), with model-specific idiolects (2502.12150, 2610.28434, 2608.06589, 2611.01549). Still open: **inducibility and maximum reachable Claudishness under a fixed interaction budget**, and the specific signatures (coined conversation-local labels, compressed anaphora, promoting passing phrases to formal concepts). None of the papers measures those. The term "Claudish" is not used anywhere I found, but "synthetic English", "AI-like style", "slop" and "idiolect" are taken, so the framing has to be precise.
- **RQ2 (snowball):** **Open.** This is the strongest novelty. Human vocabulary uptake from models is shown (B3Iq9eKyiz, 2409.01754, 2508.00238), and so are self-anchoring and drift in other properties (2605.30251 on answers, 2603.01239 on confidence). No paper measures **recursive amplification of model register over turns** (slope, onset, plateau, ceiling) or separates lexical, structural and self-conditioning uptake. 2610.29162 uses multi-turn chat but reports static rates.
- **RQ3 (what breaks first, C_H* < C_M* < C_U*):** **Open.** 2610.29162 and 2610.23715 use processing-difficulty proxies only, and 2610.29162 states that direct reader studies are missing. Antislop (2510.15061) shows that *removing* slop keeps task scores; nobody has stressed *increasing* register with task state fixed (τ²-bench style).
- **RQ4 (post-training mechanism and the role of PPL):** **Largely scooped at the coarse level, open at the fine level.** "Post-training introduces or amplifies AI-like style" is shown by 2605.30526 (with localization), 2605.27878, 2609.32196 (predictability), 2611.01549 (emerges in SFT, reshaped by PO), 2610.29162 (base<instruct<reasoning, plus causal length-penalty RL), 2508.01930 and 2606.03165 (lexical, from the preference stage), and 6IstnZXa0a (theory). Still open: G_M(c) as a **context-conditional** bits-per-byte preference (Claudish context vs natural context, with the full history-ablation grid), the claim that models *prefer* or easily process text humans find hard (a PPL/human-difficulty dissociation), and LEDOM-style reverse-LM checks.

### Useful methods/datasets to borrow
- **C(x) features:** Biber's 67 features plus MMD with human-human CIs (2605.23651); processing-difficulty features plus round-trip-translation paraphrase controls (2610.29162; spaCy and NLLB pipeline); the 25 homogeneity metrics (2611.01549); the Lexical Alignment Score and Triangulated Preference Shift (2606.03165, 2606.00334); slop taxonomy and span annotation (2509.19163); the Antislop profiling pipeline (2510.15061); the LAMP edits taxonomy and corpus (2409.14509).
- **Matched natural/Claudish pairs:** LAMP expert edits; the Antislop sampler or FTPO, or PASTA direction ablation (2605.30526), to generate "de-Claudified" twins with content fixed. PASTA's aligned-minus-base direction is also usable as a steering knob for dose-response in RQ3.
- **Post-training isolation:** OLMo-2/OLMo-3 Base, SFT, DPO and RLVR checkpoints (2605.27878, 2609.32196, 2610.29162). Zephyr and Tulu chains. Cross-model surprisal per token or byte (2609.32196).
- **Human side:** a RoBERTa reading-time predictor as a cheap C_H proxy (2610.23715) before paid reading-time studies; uptake-above-chance via same-first-question baselines (B3Iq9eKyiz).
- **Attribution:** an embedding-classifier idiolect probe (2502.12150, code at locuslab/llm-idiosyncrasies) to check whether Claudish features are Claude-specific or shared.

### Gaps Claudish can own
1. **Dynamics.** No one measures register *amplification over turns* with a controlled user simulator, uptake ablations (lexical, structural, self-history, reciprocal), onset, plateau and ceiling. This is the paper's core and it is unclaimed.
2. **Interaction-local coinage and compressed anaphora.** Existing work measures global features (nominalization, excess words, em dashes, processing-difficulty syntax). The discourse-level signatures (coined labels, promoting passing phrases to formal concepts, implicit back-reference) are unmeasured and are what make long Claude/agent sessions unreadable.
3. **The human/model dissociation with breaking points.** Estimate C_H*, C_M* and C_U* with τ²-bench state held fixed under a non-inferiority test. 2610.29162 explicitly lacks comprehension data, and no paper shows models staying functional on text humans cannot read.
4. **Conditional PPL preference (G_M) and the "PPL in the AI era" argument.** Existing work shows post-trained text is *more predictable to models* (2609.32196) and *harder for humans* (2610.29162), but nobody connects the two in one measure or runs the history-ablation grid. LEDOM (a reverse LM) gives a distinctive angle on whether the preference is directional.
5. **Positioning.** Cite 2610.29162, 2605.30526, 2611.01549 and "Synthetic English" up front. Present Claudish as "an interaction-amplified, model-family register", not "LLMs write unnaturally", which is already established. Expect a reviewer to ask "isn't this 2610.29162 plus multi-turn?" and pre-empt it with the RQ2/RQ3 results. The Chinese case can lean on 2605.25358 and 2609.07447 for cross-lingual lexical evidence.


## snowball

Scope: about 25 Semantic Scholar queries, 6 alphaXiv discovery calls, about 28 ICLR 2027 OpenReview term searches, and forward-citation expansion on 2606.30571, 2409.01754, 2508.03276 and 2605.29278. I read the abstract of every paper cited below. For 2610.29162, 2606.19857, 2606.30571 and 2609.01491 I also read the full alphaXiv report. A † marks a work whose abstract I could not retrieve.

**Bottom line:** No one has shown a register snowball in human–LLM dialog: a model's register amplifying turn after turn because the user picks up its words, with onset, slope and ceiling measured. Three neighbouring results are now well covered:
- LLM–LLM style attractors exist.
- LLMs can read text that humans can't, when that text is deliberately compressed.
- Post-training plausibly pushes models toward denser, harder-to-process syntax.

Claudish has to set itself apart from three close papers: **Clark, Martin & Mahowald 2610.29162**, **BabelTele 2606.19857** and **Ko & Geiping 2606.30571**.

| id / link | when | summary | overlap with Claudish RQ1–4 |
|---|---|---|---|
| [2610.29162](https://www.alphaxiv.org/abs/2610.29162) Why is AI-generated language so hard to understand? (Clark, Martin, Mahowald) | 2026-10-21 | Compares 14 models (frontier, plus OLMo base→instruct→Think) on chat, coding-agent and essay text. Frontier models overuse features linked to human processing difficulty: low-frequency n-grams, stacked nouns, long dependencies, inanimate subjects with animate verbs, denominal verbs, dropped "that". Effects grow from base to instruct to reasoning variants. A GRPO length-penalty run on Qwen2.5-1.5B makes the model prefer shorter syntactic alternatives, and the preference transfers to untrained constructions. Round-trip translation (via Japanese, NLLB-200) serves as the meaning-matched control. No reader study. | **RQ1 + RQ4, high overlap.** Same "post-training register prior" thesis, with the information-density pressure from RLVR as the proposed mechanism. They measure proxies only: no human comprehension, no multi-turn amplification. Also notes Opus 5.5 is lower than Opus 5 on most measures. |
| [2606.19857](https://www.alphaxiv.org/abs/2606.19857) LLMs Do Not Always Need Readable Language (BabelTele) | 2026-06-18 | Prompts LLMs to write dense multilingual, symbol-heavy text. Human QA accuracy falls from 56.1% to 35.8%, while Gemini 3.1 Pro rises from 90.0% to 96.7%. Perplexity under Llama-3-8B rises from 9.6 to 177. Readers transfer zero-shot across models. Task scores stay at about 97–99% of baseline for multi-agent work (DeepResearch Bench) and agent memory (LoCoMo). | **RQ3, high overlap.** Shows directly that human readability can break while models keep reading. But the text is deliberately engineered compression, not a register that emerges on its own. Uses PPL and bits-per-byte as "non-naturalness" (relevant to RQ4). |
| [2606.30571](https://www.alphaxiv.org/abs/2606.30571) Attractor States Emerge in Multi-Turn LLM Conversations (Ko, Geiping) | 2026-06-29 | 7 models, 20 topics, 20-turn debates in self-play and mixed-play. Each model settles into its own attractor basin in SBERT space. Influence between models is asymmetric: Claude Haiku resists change and pulls partners toward its meta-commentary, which partners then pick up; GPT-4.1-nano is the most malleable. | **RQ2, LLM–LLM, partial.** Style traits spread and settle over turns. Does not test readability, lexical uptake by humans, or task function. Metrics are reusable. |
| [2609.01491](https://www.alphaxiv.org/abs/2609.01491) GlossoGen (Stengel-Eskin, Kirby et al.) | 2026-09-01 | In a cooperative two-agent task, GPT-5.4, Sonnet 4.6 and Opus 4.7 develop compositional languages that humans can't read. GPT-2 PPL rises about 430%, and task success goes up. This happens only with a tight character budget plus a between-round "postmortem" channel. Llama-70B and Qwen3-32B don't invent such languages but can learn them. | **RQ2/RQ3, LLM–LLM, partial.** Text gets less readable while staying functional, but only under explicit efficiency pressure. Claudish's claim is that drift happens without that pressure. |
| [B3Iq9eKyiz](https://openreview.net/forum?id=B3Iq9eKyiz) Mirroring Is Not Sycophancy: Style Convergence and Position Change in Human–AI Conversation (ICLR 2027 sub.) | 2026-09/10 | 25k real conversations from a 125k evaluation corpus. Users adopt the model's vocabulary above chance, and how much they adopt predicts their preference vote. A 16-model audit: the user's emotional register shifts every model's style. Replacing the human with a model shrinks the asymmetry to near parity. | **RQ2 (human uptake), partial.** Shows users really do take up model vocabulary. Does not show it compounds into an escalating register. Also raises the point that preference data built on such conversations may reward convergence. |
| [QWlbJXhVzH](https://openreview.net/forum?id=QWlbJXhVzH) Interpreting and Steering Trajectory Entrainment in Goal-Oriented Conversations (ICLR 2027 sub.) | 2026-09/10 | Dyadic self-play as attractors. Measures entrainment and inertia over 10 turn pairs along style, semantics, emotion, task and personality, using linear probes. Contrastive activation steering pushes conversations toward convergence or divergence. | **RQ2/RQ4, partial.** Same idea of an amplification trajectory, but in representation space, with no register or readability measure. |
| [2605.29278](https://www.alphaxiv.org/abs/2605.29278) Accommodation Goes Both Ways | 2026-05-28 | WildChat, 8 languages. LLMs over-converge toward users' style; humans converge to LLMs at roughly human–human rates. | **RQ2 counter-evidence.** The dominant direction is model→user, which could damp a snowball. Claudish must show its loop despite this. |
| Who Accommodates Whom? (Behavioral Science, S2 c1a626b005) | 2026-05 | 1,319 GPT-4o WildChat conversations. Model accommodation is front-loaded at turn 1 and then stabilises. Users converge gradually over turns on pronoun dimensions. | **RQ2.** Gives a within-conversation slope analysis on real data. User convergence is progressive but small. |
| [2508.03276](https://www.alphaxiv.org/abs/2508.03276) Do LMs accommodate their users? (EACL) | 2025-08 | 16 LMs, substitution paradigm on existing dialog corpora. Models over-converge to the conversation's style; instruction-tuned and larger models converge less than pretrained ones. | **RQ1/RQ2.** Base vs instruct difference in convergence. Its protocol (also used by 2607.26015) can be reused. |
| [2409.01754](https://www.alphaxiv.org/abs/2409.01754) LLM influence on human spoken communication (Yakura et al., MPI) | 2024-09 | Synthetic-control analysis of 737k hours of speech: ChatGPT-preferred words ("delve", "meticulous") rose abruptly in spontaneous speech. | **RQ2 (human side), population scale.** Uptake happens, but across months, not within a conversation. |
| [2508.00238](https://www.alphaxiv.org/abs/2508.00238) AI-associated language in unscripted spoken English (AIES) | 2025-08 | 22M words of podcast speech: moderate but significant rise in LLM-associated words after 2022. | Same as above. |
| [2502.09606](https://www.alphaxiv.org/abs/2502.09606) Human-LLM Coevolution (ACL) | 2025-02 | "Delve" fell in arXiv abstracts after being publicly flagged; "significant" kept rising. | **RQ2 counterforce.** Humans also suppress model markers once they notice them. |
| [2412.11385](https://www.alphaxiv.org/abs/2412.11385), [2508.01930](https://www.alphaxiv.org/abs/2508.01930), [2606.03165](https://www.alphaxiv.org/abs/2606.03165) (Juzek & Ward line) | 2024-12 → 2026-06 | Ties lexical overuse to learning from human feedback (RLHF/DPO): participants prefer variants containing the focal words. Introduces the Lexical Alignment Score and Triangulated Preference Shift across 6 model families. | **RQ4, partial, lexical level only.** No discourse or register level, no in-context amplification. |
| [2605.25358](https://www.alphaxiv.org/abs/2605.25358) AI-associated lexical shifts across 34 languages | 2026-05-25 | GPT-4.1 overused-lemma lists per language; "emphasize"-type verbs appear in 24 of 34 languages; uptake in news in 26 of 34 languages. | Relevant to the Chinese replication and RQ1 cross-lingually. |
| [2605.27878](https://www.alphaxiv.org/abs/2605.27878) Narrative Flattening (EMNLP 2026) | 2026-05-27 | OLMo-32B Base/SFT/DPO/RLVR checkpoints. Post-training compresses thematic, affective and stylistic variation in fiction. | **RQ4 method.** A checkpoint ladder that isolates post-training stages. |
| [2610.28474](https://openreview.net/forum?id=2610.28474)† · see S2 2610.28474 Answering in the User's Variety | 2026-10-20 | Across 10 open model lineages, post-training (SFT→DPO→RL) standardises replies; compares log-prob of content-matched reply pairs. | **RQ4 method.** Content-matched pair log-prob is essentially Claudish's G_M. |
| [2605.20602](https://www.alphaxiv.org/abs/2605.20602) Self-Training Doesn't Flatten Language – It Restructures It | 2026-05-20 | 11 generations of self-training on 5 small LMs. Surface markers (connectives, hedges, em-dashes) increase while deep syntax collapses ("Structural Depth Hypothesis"). | **Training-time analog of RQ2.** Predicts which Claudish signatures should amplify. |
| [PsHzWVWL4u](https://openreview.net/forum?id=PsHzWVWL4u) A Dominant Supplier Slows Recursive Drift (ICLR 2027 sub.) | 2026-09/10 | 8 open models retrained over 5 generations on each other's text drift together toward fewer function words; a dominant supplier slows drift more than it steers it. | Model-collapse version of register drift, at training time. |
| [2305.17493](https://www.alphaxiv.org/abs/2305.17493) Curse of Recursion; [2311.09807](https://www.alphaxiv.org/abs/2311.09807) Curious Decline of Linguistic Diversity | 2023 | Recursive training on generated data loses distribution tails and lexical, syntactic and semantic diversity. | Background for RQ2/RQ4 only. These are training loops, not in-context loops. |
| [NTOdIP7hFT](https://openreview.net/forum?id=NTOdIP7hFT) Perplexity Can Reverse the Verdict on Model Collapse (ICLR 2027 sub.) | 2026-09/10 | Aggregate PPL hides rare-token loss: a frequency-band decomposition flips the conclusion in 5 of 10 experiments. | **RQ4 ("role of PPL").** A direct caution against using PPL or bits-per-byte as a naturalness measure. |
| [2603.11228](https://www.alphaxiv.org/abs/2603.11228) Markovian Generation Chains (EMNLP); [2502.20258](https://www.alphaxiv.org/abs/2502.20258) Broken Telephone (ACL) | 2025–26 | Iterative rephrasing or translation either converges to a small recurrent set or keeps producing novel sentences; distortion accumulates over the chain. | **RQ2 baseline.** A pure self-conditioning chain with no user. |
| [2605.17193](https://www.alphaxiv.org/abs/2605.17193) Multi-LLM Systems Exhibit Robust Semantic Collapse | 2026-05-16 | Over 200–1,000 rounds, interacting LLMs converge semantically despite surface variation; none of 13 interventions reliably restores diversity. | **RQ2, LLM–LLM.** Semantic rather than register level. |
| [2602.03664](https://www.alphaxiv.org/abs/2602.03664) Conversational Inertia; [2603.03308](https://www.alphaxiv.org/abs/2603.03308) Old Habits Die Hard; [2609.05882](https://www.alphaxiv.org/abs/2609.05882) What if LLMs Ate Their Words; [2611.00518](https://www.alphaxiv.org/abs/2611.00518) AC3 context pollution | 2026 | Models treat their own earlier replies as few-shot examples (diagonal attention). History acts as a "geometric trap". Replacing assistant history with neutral content ("neutralization") changes later behaviour causally. Assistant pollution even spreads to an analyzer model. | **RQ2/RQ4 mechanism (self-conditioning).** Neutralization is close to Claudish's "history rewritten as natural" ablation. None measures register. |
| [a4AhDiAVL0](https://openreview.net/forum?id=a4AhDiAVL0) Context Contamination & Persona Drift (ICLR 2027 sub.); [2412.00804](https://www.alphaxiv.org/abs/2412.00804) Identity Drift | 2024–26 | Persona-direction "drift" is largely contamination by the amount of new context; larger models show more identity drift. | **RQ2 confound.** Claudish needs length-matched natural-context controls (already planned). |
| [2505.06120](https://www.alphaxiv.org/abs/2505.06120) LLMs Get Lost in Multi-Turn Conversation | 2025-05 | Sharded multi-turn instructions cause a 39% average drop across 6 tasks. | **RQ3 confound.** Multi-turn degradation happens even without register drift, so it must be separated out. |
| [2609.09090](https://www.alphaxiv.org/abs/2609.09090) SPINE; [2608.21377](https://www.alphaxiv.org/abs/2608.21377) Agentic Scaffolding Amplifies Sycophancy; [2505.23840](https://www.alphaxiv.org/abs/2505.23840) SYCON | 2025–26 | Sycophantic collapse grows with conversation length (to 20–97% within 25 turns, using an adaptive LLM user); agentic feedback loops amplify it. | **RQ2 analog.** Turn-indexed amplification under an adaptive simulator, but for stance, not register. |
| [2603.19574](https://www.alphaxiv.org/abs/2603.19574) AI Psychosis: Does Conversational AI Amplify Delusion-Related Language? (EMNLP) | 2026-03-20 | Simulated users built from Reddit histories; a DelusionScore trajectory rises over turns for treatment users only. | **RQ2 template.** Amplification of a linguistic score across turns in human-like simulated users. |
| [wY0JSEIbKB](https://openreview.net/forum?id=wY0JSEIbKB) Self-coined Terms Improve Communication in Multi-Agent LLMs (CoGENT, ICLR 2027 sub.) | 2026-09/10 | Agents coin terms with definitions and reuse them on a shared board. Accuracy rises on 6 benchmarks (Claude Sonnet 4.6 on HLE: 18.3%→27.0%) and final-round tokens fall 56%. | **RQ3, important.** "Coined labels" is one of Claudish's listed signatures, and here it *helps* models. Claudish should test whether the same terms hurt humans. |
| [2510.27338](https://www.alphaxiv.org/abs/2510.27338) Reasoning Models Sometimes Output Illegible CoTs (NeurIPS 2025) | 2025-10-31 | Across 14 models, RL makes CoT illegible to humans and monitors (except Claude); accuracy drops 53% when forced to use only legible parts. | **RQ3/RQ4.** Readability for humans and usefulness to the model come apart under RL, but inside private CoT, not dialog. |
| [2407.13692](https://www.alphaxiv.org/abs/2407.13692) Prover-Verifier Games improve legibility | 2024-07 | Optimising only for correctness reduces legibility to humans; legibility training restores it. | **RQ4.** Optimisation pressure works against legibility. |
| [2310.06272](https://www.alphaxiv.org/abs/2310.06272) CIPHER (ICLR 2024); [2511.09149](https://www.alphaxiv.org/abs/2511.09149) Interlat; [2605.04426](https://www.alphaxiv.org/abs/2605.04426) Telegraph English; [2403.12968](https://www.alphaxiv.org/abs/2403.12968) LLMLingua-2 | 2023–26 | Leaving natural language behind (embeddings, latents, symbolic dialects, token deletion) keeps or improves model task performance. | **RQ3 endpoint reference.** The "readable to models but not humans" end of the axis, reached by design. |
| [2605.31170](https://www.alphaxiv.org/abs/2605.31170) Emergent Languages on Moltbook (AIES) | 2026-05-29 | In-the-wild agent posts proposing new languages, for token efficiency (166) or oversight evasion (59); other LMs learn them from descriptions alone. | **RQ2.** Naturalistic LLM–LLM language drift. |
| [2003.12694](https://www.alphaxiv.org/abs/2003.12694) Seeded Iterated Learning (ICML 2020); [1909.04499](https://www.alphaxiv.org/abs/1909.04499) Countering Language Drift via Visual Grounding | 2019–20 | Classic language drift under task fine-tuning in Lewis and translation games, with mitigations. | Background for RQ2 and "drift while still functional". |
| [2605.19936](https://www.alphaxiv.org/abs/2605.19936) What Are LLMs Doing to Scientific Communication? (LREC) | 2026-05-19 | ACL Anthology 2020–24 plus 3k LLM "improved" passages: more complex and longer words, lower lexical diversity. Experts rate LLM-improved text as *more* understandable. | **RQ3 caution.** One-shot Claudish may read as better; harm may show up only at high C. |
| [2604.19139](https://www.alphaxiv.org/abs/2604.19139) Verbal tics in frontier LMs (review) | 2026-04-21 | Reviews linguistic habits and sycophancy across 8 developers (evidence cutoff Oct 2026); says comparable measurements of the newest releases are sparse. | **RQ1.** Survey of signatures; confirms a measurement gap. |
| [2610.02873](https://www.alphaxiv.org/abs/2610.02873) ConvoDrift | 2026-10-02 | 15.7k multi-turn conversations annotated for progressive tone drift under fixed semantic intent. | Possible dataset or baseline for RQ2 measurement. |
| [cqWa4nYVKQ](https://openreview.net/forum?id=cqWa4nYVKQ) Stance Drift in AI-mediated communication; [giMpNTtGsL](https://openreview.net/forum?id=giMpNTtGsL) Homogenization in MAS (ICLR 2027 subs.) | 2026-09/10 | A generation→extraction pipeline keeps stance at most 0.7 of the time across 9 LLMs. Multi-agent systems show conformity, polarization and growing inertia. | Peripheral to RQ2/RQ3: functional drift without a register lens. |
| The Sycophantic Slide (ACM, DOI 10.1145/3841580.3841583)† | 2026-11 | Title suggests turn trajectories of sycophancy; no abstract available. | Unverified. |
| Gibberlink (ElevenLabs hackathon demo, 2025)†; Anthropic "spiritual bliss attractor" (Claude Opus 4 system card)† | 2025 | Agent-to-agent switch to a non-speech protocol; self-play attractor in Claude. Not peer-reviewed; known only second-hand (the bliss attractor through 2606.30571). | Motivating anecdotes only. |

### Does this scoop any RQ?

- **RQ1 (prevalence and inducibility): partly scooped.** 2610.29162 already shows that most frontier models overuse dense, hard-to-process constructions, and that the effect grows from base to instruct to reasoning (OLMo). The lexical "delve" papers (2412.11385, 2606.03165, 2605.25358) and 2604.19139 cover word-level tics across many models. Still open:
  - discourse-level signatures: coined local terms, compressed anaphora, over-structuring, treating a passing phrase as a formal concept;
  - *inducibility* and the *maximum reachable C* under a fixed interaction budget;
  - a C(x) grounded in human pairwise judgments.
- **RQ2 (snowball): open for human–LLM; partly scooped for LLM–LLM.**
  - LLM–LLM style attractors and trait transfer are shown: 2606.30571 (Claude Haiku spreads meta-commentary), QWlbJXhVzH, 2605.17193.
  - LLM–LLM drift to unreadable language is shown, but needs efficiency pressure: 2609.01491, 2605.31170.
  - Users adopting model vocabulary is shown in real logs, without escalation: B3Iq9eKyiz, c1a626b005, 2409.01754.
  - No paper measures a register-level amplification slope, onset, plateau or ceiling with controlled lexical vs structural vs semantic uptake.
  - Counter-evidence to address: 2605.29278 and 2508.03276 find models over-converge *to the user*, and 2502.09606 finds humans suppress markers once flagged.
- **RQ3 (what breaks first): partly scooped.**
  - Human comprehension collapsing while LLM QA holds is shown for *engineered* compression: BabelTele 2606.19857 (human QA 56→36%, model 90→97%).
  - It is shown for emergent agent languages: 2609.01491.
  - It is shown for RL-trained CoT: 2510.27338.
  - CoGENT (wY0JSEIbKB) shows coined terms *improve* model accuracy.
  - Still open: the three breaking points C_H*, C_M* and C_U* measured along an *organically amplified* register trajectory, on agentic tool-use (τ²-bench-style) with a non-inferiority test, plus whether models can detect Claudish and reconstruct plain language.
- **RQ4 (post-training mechanism and PPL): partly scooped.**
  - 2610.29162 offers RLVR information-density pressure, with a GRPO length-penalty demonstration on a 1.5B model.
  - The Juzek & Ward line ties lexical overuse to learning from human feedback.
  - 2605.27878 and 2610.28474 use OLMo or other lineage checkpoint ladders and content-matched pair log-probs, which is close to G_M.
  - 2606.19857 and 2609.01491 use PPL as "non-naturalness"; NTOdIP7hFT warns that aggregate PPL can reverse conclusions.
  - Still open: G_M(c) as a *context-conditional* preference (does Claudish history raise the model's relative log-prob for Claudish continuations?), its ordering from older models to base to post-trained to post-trained with Claudish context, and a reverse-LM (LEDOM) lens on PPL.

### Useful methods/datasets to borrow
- **Meaning-matched natural paraphrases.** Round-trip translation (NLLB, via Japanese or Russian) as in 2610.29162, giving the y_natural counterpart for C(x) pairs and G_M. Their psycholinguistic feature set and spaCy pipeline can be reused for automatic C proxies.
- **Trajectory metrics.** From 2606.30571: SBERT topic-centred PCA, basin-separation score, partnerward pull α, dominance Δ, off-axis drift, and LLM-judged trait transfer (including meta-commentary). Entrainment and inertia probes plus contrastive activation steering from QWlbJXhVzH.
- **Convergence measurement.** Asymmetric convergence metric on WildChat (2605.29278); a within- vs between-conversation design for user-side slopes (c1a626b005); the substitution paradigm on existing dialog corpora (2508.03276, 2607.26015).
- **Human uptake baseline.** Same-first-question chance baseline for user vocabulary adoption, and the link to preference votes (B3Iq9eKyiz).
- **History ablations.** "Neutralization" of assistant history (2609.05882), assistant-omission (2611.00518), and content-amount controls for contamination (a4AhDiAVL0). These map directly onto Experiment IV.
- **Simulated users.**
  - Reddit-history SimUsers with a turn-indexed linguistic score (2603.19574).
  - An adaptive LLM user for up to 25 turns (SPINE 2609.09090).
  - Sharded multi-turn tasks (2505.06120), to separate register effects from ordinary multi-turn loss.
- **Human vs model readability protocol.** Dale-Chall plus difficult-word ratio, human QA accuracy and completion time against LLM QA, and a cross-model reader transfer matrix (BabelTele 2606.19857).
- **Post-training attribution.** OLMo Base/SFT/DPO/RLVR checkpoints (2605.27878; OLMo Think in 2610.29162); the content-matched reply-pair log-prob method (2610.28474); Lexical Alignment Score and Triangulated Preference Shift (2606.03165).
- **Agent-language tooling.** The GlossoGen platform (fork, edit and swap agents; MDL grammar analysis) for an LLM–LLM arm; CoGENT's term board as an explicit coined-term manipulation.
- **PPL hygiene.** Frequency-band decomposition of loss (NTOdIP7hFT); report bits-per-byte across tokenizers.

### Gaps Claudish can own
1. **The first controlled human–LLM register snowball.** Use the planned uptake conditions (none / semantic / lexical-only / structural-only / self-history / full loop / length-matched natural) and report onset, slope, plateau and ceiling per model. Existing work has LLM–LLM attractors or a static measure of user adoption, never escalation under controlled uptake.
2. **Discourse-level signatures that no current metric covers:**
   - conversation-local coined labels promoted to "established concepts";
   - compressed anaphora back to earlier turns;
   - over-structuring and meta-commentary.

   2610.29162 is sentence-level psycholinguistics, and the delve line is lexical.
3. **Breaking points on an organic trajectory.** Measure C_H* < C_M* < C_U* on organically produced text with fixed τ²-bench goals and state and a non-inferiority margin. This links the BabelTele/GlossoGen claim ("models read it, humans don't") to *ordinary, pressure-free* interaction. It is also a direct test of the CoGENT tension: coined terms help models, do they hurt humans?
4. **Context-conditional G_M.** Show that Claudish history raises the model's relative log-prob for Claudish continuations, as an in-context self-reinforcement mechanism, and do it in bits-per-byte across the full ordering (older < base < post-trained < post-trained with Claudish context). Pair this with LEDOM (reverse LM) to ask whether naturalness under PPL is direction-specific. Nobody has combined a PPL critique with the register question.
5. **The convergence-asymmetry puzzle.** Models over-accommodate users (2605.29278), yet a model-driven snowball is hypothesised. Claudish can decompose user→model and model→user flow within the same loop, using the self-history-only and user-only-history ablations.
6. **Cross-lingual (Chinese) multi-turn replication.** Lexical uptake has been studied across 34 languages (2605.25358), but no register-snowball replication exists in Chinese.
7. **Positioning risk.** Reviewers will cite 2610.29162 for "post-training makes language harder", 2606.19857 for "models read unreadable text", and 2606.30571 for "multi-turn attractors". The novelty claim should centre on the *amplification dynamics under human-like uptake* plus *functional breaking points*, not on the existence of the register or of human/model decoupling. A head-to-head control is cheap and worth including: the BabelTele prompt as "maximal engineered C", compared with organic Claudish at matched C.


## function
Angle: does model functionality survive text that humans find unnatural or unreadable? The question covers compressed or model-native text, gibberish and scrambled input, illegible or steganographic chains of thought, LLM-to-LLM language drift, and how robust agents are to the user's style in τ-bench-like setups. Sources: about 30 queries across Semantic Scholar, alphaXiv and ICLR 2027 OpenReview, plus citation expansion on 2606.19857, 2503.01926, 2510.27338, 2409.01754 and 2609.01491. I read the abstract of every paper listed below except those marked †. For 2606.19857 I also read the alphaXiv full-paper report.

**Bottom line.** Two things are already well established: models can read text that humans can't, and post-trained text is harder for humans to process. Nobody has measured the trajectory: an ordinary-interaction snowball, with human, model and task breaking points measured on the same samples. The two closest threats are BabelTele (2606.19857, on RQ3) and "Why is AI-generated language so hard to understand?" (2610.29162, on the human side of RQ1 and on RQ4).

| id / link | when | summary | overlap with Claudish RQ1-4 |
|---|---|---|---|
| [2606.19857](https://www.alphaxiv.org/abs/2606.19857) "LLMs Do Not Always Need Readable Language" (BabelTele) | 2026-06 | Prompts LLMs (Gemini 3.1 Pro, GPT-5.4, Claude, Qwen and others) to write "model-native" compressed text: multi-script words, symbols, emoji. Human QA accuracy drops from 56.1% to 35.8% and Llama-3-8B PPL rises from 9.6 to 177. Model QA holds (90% → 96.7%), and semantic fidelity is 99.5% at 27.9% of the original length. The text transfers zero-shot across models and works for agent memory (LoCoMo) and multi-agent communication (DeepResearch Bench). | **RQ3 strongly** (the gap between human and model breaking points; PPL/BPB as a measure of departure from natural language) and **RQ4** (PPL no longer tracks model-readability). The text is deliberately engineered, not an emergent mild register, and there is no snowball or multi-turn uptake. |
| [2610.29162](https://www.alphaxiv.org/abs/2610.29162) "Why is AI-generated language so hard to understand?" (NYU / UT Austin) | 2026-10 | Across 9 frontier models, AI text uses more features known to make human processing harder: long dependencies, stacked nouns, reduced object relatives, low-frequency n-grams, inanimate subjects with animate verbs. The authors argue that post-training for math, code and reasoning pushes toward information density rather than communication. Fine-tuning with a length penalty reproduces the drift. | **RQ1 (definition and prevalence) and RQ4 (post-training mechanism)**: this is nearly the Claudish hypothesis seen from the human side. It does not cover multi-turn amplification, model task use or a breaking point. Must cite and position against. |
| [2605.30526](https://www.alphaxiv.org/abs/2605.30526) PASTA: "Measuring, Localizing, and Reducing the AI-Like Writing Style of Post-Trained LLMs" (EMNLP 2026) | 2026-05 | Compares base and aligned generations from matched human prefixes. Aligned text has lower human-corpus affinity and is easier for detectors to flag. An aligned-minus-base residual direction, ablated at decode time, lowers the detection rate across 11 models. | **RQ1 base vs instruct, RQ4**: a ready-made base/instruct protocol, and a steering direction that could serve as a causal "Claudish knob". |
| [2611.01549](https://www.alphaxiv.org/abs/2611.01549) "Revisiting LLM Homogeneity Through a Multidimensional Lens" | 2026-11 | Profiles 18 LLMs on 25 interpretable metrics. Releases from 2024 to 2026 grow more like themselves but less like each other. Model-specific conventions appear during SFT and are reshaped by preference optimization. | **RQ1 / RQ4**: per-model register signatures and the stage at which they appear. |
| [2609.32196](https://www.alphaxiv.org/abs/2609.32196) "Is the Judge Its Twin?" | 2026-09 | Follows OLMo-2 and Zephyr through base → SFT → DPO. The writing becomes more predictable at each stage (7-25% lower surprisal under another family's model), but the same models as judges barely start preferring predictable text. | **RQ4**: direct evidence for the "older/base < post-trained" PPL ordering, using cross-model surprisal. |
| [2605.25459](https://www.alphaxiv.org/abs/2605.25459) "Post-trained LMs recognize and react to their own generations" (IAS / Anthropic) | 2026-05 | Output entropy is 3-4× lower on on-policy than on off-policy context. An internal input-surprise representation modulates it. | **RQ2 self-conditioning, RQ4**: a mechanism by which Claudish history could lock in. |
| [2502.12150](https://www.alphaxiv.org/abs/2502.12150) "Idiosyncrasies in LLMs" (ICML 2025) | 2025-02 | Text embeddings classify the source model with 97% accuracy. The signal survives rewriting, translation and summarization. | **RQ1**: a classifier-based C(x) proxy, and a reason to control per model. |
| [2508.01930](https://www.semanticscholar.org/arxiv/2508.01930) and [2412.11385](https://www.semanticscholar.org/arxiv/2412.11385) ("delve" / word overuse and LHF) | 2024-12 / 2025-08 | Lexical overuse is traced to preference learning; human raters in an emulated LHF setup prefer the overused words. Related follow-ups: 2606.03165 (Lexical Alignment Score / Triangulated Preference Shift) and 2606.00334. | **RQ4** at the lexical level only. Claudish's discourse and structural features are not covered. |
| [2409.01754](https://www.semanticscholar.org/arxiv/2409.01754) (podcast speech uptake), [2508.00238](https://www.semanticscholar.org/arxiv/2508.00238), [2605.25358](https://www.semanticscholar.org/arxiv/2605.25358) (34 languages), [2609.07447](https://www.semanticscholar.org/arxiv/2609.07447) (Korean register shift) | 2024-2026 | Humans take up ChatGPT-favoured words in speech and writing at population scale (synthetic control). The effect appears across 34 languages and in a Korean academic register. | The **human-uptake arm of RQ2**, but at macro scale over months. None of these show uptake within a conversation. 2609.07447 is a template for the Chinese/cross-lingual replication. |
| "Who Accommodates Whom? Bidirectional Linguistic Accommodation…" (Behavioral Sciences, DOI 10.3390/bs16050720) | 2026-05 | On 1,319 WildChat GPT-4o conversations, the model accommodates at turn 1 and then stabilizes. Users converge gradually across turns on interpersonal-pronoun function words, not on topic words. | **RQ2**: the closest observational evidence of within-conversation user convergence. It covers function words only and has no amplification or naturalness measure. Useful as a WildChat baseline. |
| [2605.20602](https://www.alphaxiv.org/abs/2605.20602) "Self-Training Doesn't Flatten Language — It Restructures It" | 2026-05 | Over 11 self-training generations, surface markers (connectives, hedges, em-dashes) amplify and deep syntax collapses. A human-text fine-tuning control shows no such gradient. | **RQ2 analogue at training time.** Claudish is the same question at inference time. Borrow the feature panel. |
| [2603.11228](https://www.semanticscholar.org/arxiv/2603.11228) "Markovian Generation Chains" (EMNLP) | 2026-03 | Repeated rephrasing either converges to recurrent sets or keeps producing novel sentences, depending on temperature and seed. | **RQ2 methods**: attractor and plateau analysis for snowball trajectories. |
| [2602.24287](https://www.alphaxiv.org/abs/2602.24287) "Do LLMs Benefit From Their Own Words?" and [2609.05882](https://www.alphaxiv.org/abs/2609.05882) "What if LLMs Ate Their Words" | 2026-02 / 09 | Replacing assistant history with one-line summaries or neutral content barely changes quality, though context pollution happens. A causal history-editing (neutralization) design with a length-controlled subset. | **RQ2/RQ4 ablations**: closely matches the planned assistant-only and rewritten-history conditions. Copy the neutralization protocol. |
| [2505.06120](https://www.alphaxiv.org/abs/2505.06120) "LLMs Get Lost in Multi-Turn Conversation" (ICLR) | 2025-05 | Performance drops in sharded multi-turn settings across all top models. | **RQ3 confound**: snowball degradation has to be separated from generic multi-turn degradation. |
| [2610.02015](https://www.alphaxiv.org/abs/2610.02015) "On Language Drift during RLVR Post-Training" | 2026-10 | Proves RLVR permits unbounded CoT language drift and SFT does not. Drift appears on novel tasks, and it cannot be constrained without constraining reward. | **RQ4 theory**: a post-training account of the drift, for CoT only. |
| [2510.27338](https://www.alphaxiv.org/abs/2510.27338) "Reasoning Models Sometimes Output Illegible CoTs" (NeurIPS 2025) | 2025-10 | 14 reasoning models produce illegible CoTs (Claude is the exception) while final answers stay readable. Accuracy drops 53% when the model is forced to use only the legible portions. | **RQ3 analogue**: illegible to humans yet still working, but in model-internal reasoning, not in conversation. |
| [2609.37312](https://www.alphaxiv.org/abs/2609.37312), [2506.01926](https://www.semanticscholar.org/arxiv/2506.01926), [2603.20508](https://www.semanticscholar.org/arxiv/2603.20508), ICLR'27 [5xrNwUd8y4](https://openreview.net/forum?id=5xrNwUd8y4), [bDRNeSJYL0](https://openreview.net/forum?id=bDRNeSJYL0) | 2025-2026 | Monitorability and steganography line: hidden reasoning must leak but need not be readable; steganographic CoT can be learned; "transfer utility" measures legibility; monitorability is trainable; length penalties hurt monitorability. | Framing for why RQ3 matters (oversight). 2603.20508's weak-model transfer utility can be used as a model-side comprehension metric. |
| [2609.01491](https://www.alphaxiv.org/abs/2609.01491) GlossoGen | 2026-09 | Multi-agent LLMs under efficiency pressure evolve compositional languages that humans cannot read. Needed: efficiency pressure, strong models, a "postmortem" convention stage. | **RQ2 analogue** for agent-agent interaction with explicit pressure. Claudish is about human-model interaction with no pressure. |
| [2609.32701](https://www.semanticscholar.org/arxiv/2609.32701) "Frontier Agents Improvise Covert Channels" | 2026-09 | Model pairs develop shared private meaning at test time from 1-bit feedback. | **RQ2**: in-context convention formation without training. |
| [2503.01926](https://www.alphaxiv.org/abs/2503.01926) "Unnatural Languages Are Not Bugs but Features" (ICML 2025) | 2025-03 | Unnatural strings carry latent semantic features that transfer across models. Instruction-tuning on unnatural data matches natural data (49.7 LC-AlpacaEval win rate). Models filter noise and infer meaning from the remaining words. | **RQ3**: an extreme end point. Models read text far beyond what humans can read. |
| [2404.17120](https://www.alphaxiv.org/abs/2404.17120) "LM Babel", [2503.00224](https://www.alphaxiv.org/abs/2503.00224), [2311.18805](https://www.alphaxiv.org/abs/2311.18805) Scrambled Bench (EMNLP'23), [2410.01677](https://www.semanticscholar.org/arxiv/2410.01677) | 2023-2025 | GCG-optimized gibberish drives coherent outputs; Byzantine-symbol strings carry meaning for GPT-4o and Claude; GPT-4 reconstructs heavily scrambled text; typoglycemia robustness varies by model. | **RQ3 background**: C_M* can lie far beyond C_H* when the noise is adversarial or random. Claudish's novelty is a *naturalistic* register. |
| [2605.04426](https://www.alphaxiv.org/abs/2605.04426) Telegraph English, ICLR'27 [jQOAf9MQpv](https://openreview.net/forum?id=jQOAf9MQpv), [hEDqFXFy0v](https://openreview.net/forum?id=hEDqFXFy0v), [2608.04569](https://www.alphaxiv.org/abs/2608.04569), [2310.05736](https://www.alphaxiv.org/abs/2310.05736) / [2403.12968](https://www.semanticscholar.org/arxiv/2403.12968) LLMLingua(-2), gist tokens† (Mu et al. 2023) | 2023-2026 | A symbolic rewrite keeps 99% of key facts at about 50% of the tokens. The ICLR'27 pre-registered evaluation finds encoders break schema and token-budget rules. "Referential dangling": compression breaks anaphora links, losing 29-34 points of accuracy. | **RQ3**: the "compressed anaphora" feature of Claudish has a measured failure mode (dangling). Pre-registration practice from jQOAf9MQpv. |
| ICLR'27 [epGBf9UkIq](https://openreview.net/forum?id=epGBf9UkIq) "Is Human-Readable Text Necessary for Effective LLM Fine-Tuning?" | 2026 (ICLR'27 submission) | Optimized non-readable synthetic embeddings match natural-language fine-tuning data. | Peripheral to RQ3/RQ4 (training-side readability). |
| ICLR'27 [PTic6zZAch](https://openreview.net/forum?id=PTic6zZAch) "Expression Style-Induced Bias in LLM Agents" | 2026 (ICLR'27 submission) | The PA-Style taxonomy (polite/direct × calm/emotional) shows substantial task-success gaps in agents when only the user's expression style changes. | **RQ3 C_U***: shows that task-irrelevant user style *does* affect agents, so non-inferiority under Claudish is not a given. |
| [2610.35426](https://www.alphaxiv.org/abs/2610.35426) StressSim (ICLR'27 [1eU2OA2d7i](https://openreview.net/forum?id=1eU2OA2d7i)), [2509.23124](https://www.semanticscholar.org/arxiv/2509.23124) Non-collab simulators (ICLR), [2605.12894](https://www.semanticscholar.org/arxiv/2605.12894) PPol, [2610.19560](https://www.alphaxiv.org/abs/2610.19560) τ-Emotion-Bench, [2602.11348](https://www.semanticscholar.org/arxiv/2602.11348) AgentNoiseBench, [2611.07203](https://www.alphaxiv.org/abs/2611.07203) τ-bench-LILT | 2025-2026 | User-simulator perturbations that preserve intent: adversarial plans (τ-bench pass^4 down by up to 69%), non-collaborative behaviours, evolved personas on τ²-bench, emotion conditioning, user noise, and multilingual τ³-bench (drops of up to 61 points). | **RQ3 infrastructure**. None vary *register or naturalness* along a graded axis while holding intent, policy and DB fixed. Holding goal, policy and DB fixed (goal preservation) is already standard practice. |
| [2610.14477](https://www.alphaxiv.org/abs/2610.14477) "Executable task contracts" | 2026-10 | Embedding-similarity validity checks accept 75-100% of edits that actually change the task. Proposes executable contracts that check each perturbed case before the agent runs. | **RQ3 validity**: use contracts, not cosine similarity, to certify that Claudish rewrites preserve the task. |
| [2610.10988](https://www.alphaxiv.org/abs/2610.10988) "Back in Style" and ICLR'27 [dXIY9FObFf](https://openreview.net/forum?id=dXIY9FObFf) UserLM-2.0 | 2026-10 | Personas written as stylometric rates, with fidelity checked by authorship-verification stylometry. An RL user simulator jointly rewarded for intent and naturalness, using a learned naturalness discriminator. | **RQ2 simulator design**: controllable style uptake, and a discriminator usable as a naturalness scorer. |
| [2611.09768](https://www.alphaxiv.org/abs/2611.09768) "Language as Trait, Politeness as Strategy" | 2026-11 | In Werewolf agents, rewriting only the linguistic realization (simple vs complex) does not change trust or votes after correction for multiple comparisons. | **RQ3**: an example of a non-inferiority-style null with a replay design. |
| [2610.23715](https://www.semanticscholar.org/arxiv/2610.23715) "Cognitive Cost of LLM Writing" | 2026-10 | RoBERTa-RT, trained on eye-tracking data, predicts higher per-word reading time for LLM rewrites. | **RQ3 C_H***: a cheap reading-time proxy to use before human studies. |
| [2507.01335](https://www.semanticscholar.org/arxiv/2507.01335) LEDOM (ACL) | 2025-07 | A right-to-left LM trained at scale. | **RQ4**: an alternative PPL view. Reverse-LM surprisal can check whether the Claudish PPL gap is direction-dependent. |

### Does this scoop any RQ?
- **RQ1 (prevalence and inducibility): partly scooped.** 2610.29162 shows that all 9 frontier models over-use features that are hard for humans to process, with family variation. 2605.30526, 2611.01549 and 2502.12150 show post-training adds a model-specific style. Still open: *inducibility*, meaning susceptibility under a standard induction protocol and the maximum C reached within a fixed interaction budget, plus a human-grounded graded C(x) that is distinct from detectability.
- **RQ2 (snowball): open.** The nearest work is 2605.20602 (amplification under self-training, not in conversation), GlossoGen 2609.01491 and 2609.32701 (agent-agent interaction with pressure or feedback), the Behavioral Sciences accommodation paper (user function-word convergence in WildChat, no amplification), and 2409.01754 / 2605.25358 (macro-level human uptake). No paper measures turn-level amplification slope, onset, plateau or ceiling of a model register under controlled lexical vs structural uptake.
- **RQ3 (what breaks first): partly scooped on the claim "unreadable to humans, readable to models".** BabelTele 2606.19857 gives human and model QA plus PPL on engineered compressed text. 2503.01926, 2404.17120, 2311.18805 and 2510.27338 cover gibberish, scrambled text and illegible CoT. Still open: separate C_H*, C_M* and C_U* estimates along a *naturalistic* trajectory, and τ²-bench task success with goal, policy and DB fixed and only the wording varied. Also needed is a formal non-inferiority test. PTic6zZAch and StressSim suggest agents are *not* style-invariant, so the result is not obvious in advance.
- **RQ4 (mechanism / PPL): partly scooped.** 2610.29162 (the information-density hypothesis plus a length-penalty proof of concept), 2610.02015 (RLVR permits drift, SFT does not), 2508.01930 / 2412.11385 / 2606.03165 (LHF causes lexical overuse) and 2609.32196 (post-training lowers cross-model surprisal) together cover "post-training causes it". Still open: the conditional G_M(c) in bits per byte, as a function of *Claudish context*, with history ablations. Also open is "what PPL means" once human naturalness and model likelihood diverge (BabelTele shows high PPL with fine model use; Claudish predicts *low* model PPL with low human naturalness). LEDOM, the mentor's own model, gives a distinctive angle.

### Useful methods/datasets to borrow
- **Psycholinguistic difficulty feature set** from 2610.29162 (dependency length, stacked nouns, reduced relatives, low-frequency n-grams, animacy mismatch) as automatic C(x) components. Add the 17-feature depth panel from 2605.20602.
- **Base/aligned matched-prefix protocol and PASTA direction** (2605.30526): compare base and instruct models for RQ1, and use the ablation as a causal intervention ("remove the Claudish direction, check whether the snowball stops").
- **Cross-family surprisal / BPB** (2609.32196, BabelTele's PPL/BPB/BPC tables) for G_M. Add LEDOM reverse-LM surprisal.
- **History neutralization and replay** (2609.05882, 2602.24287, the 2611.09768 replay-from-checkpoint design) to implement the assistant-only, user-only, rewritten-history and style-kept/facts-swapped ablations.
- **User simulators with controllable style**: the stylometric persona schema (2610.10988), UserLM-2.0 with a naturalness discriminator (dXIY9FObFf), and StressSim's goal-preservation check. **Executable task contracts** (2610.14477) to certify that wording-only edits leave the task intact on τ²-bench.
- **τ²/τ³-bench variants** (τ-bench-LILT for the Chinese/multilingual replication, τ-Emotion, PPol) as harnesses. Report pass^k.
- **Human-side measures**: RoBERTa-RT reading-time predictor (2610.23715) for cheap C_H* screening, then a small human study. BabelTele's questionnaire design (difficulty, QA accuracy, completion time).
- **Population baselines**: WildChat accommodation measures (Behavioral Sciences 2026) and excess-vocabulary placebo methods (2609.07447, 2605.25358) to validate that the simulator's uptake rates are realistic.
- **Pre-registration practice** from jQOAf9MQpv, if planning confirmatory non-inferiority tests.

### Gaps Claudish can own
1. **Emergent, naturalistic unreadability with no pressure applied.** All existing "human-unreadable but model-readable" work uses engineered compression (BabelTele, Telegraph English), adversarial optimization (LM Babel, GCG), random noise (Scrambled Bench), or explicit efficiency or RL pressure (GlossoGen, RLVR drift, illegible CoT). Claudish asks whether *ordinary cooperative chat* drifts there by itself. That is the headline gap.
2. **Turn-level snowball dynamics in human-model interaction**: amplification slope, onset, plateau, model ceiling, and lexical vs structural uptake controls. Nothing published covers this; it only exists at training time (self-training) or population scale (delve studies).
3. **Three breaking points measured jointly on the same samples** (C_H*, C_M*, C_U*), with a τ²-bench non-inferiority test where only the wording varies. Prior work reports human and model QA at a single compression point, or agent robustness to style with no graded naturalness axis.
4. **The low-PPL / low-naturalness quadrant.** BabelTele is high-PPL text that models can read. Claudish predicts text that has *low* model PPL (especially in-context) yet is unnatural to humans. That would directly show PPL failing as a naturalness metric in post-trained models. LEDOM's reverse surprisal is a distinctive probe the lab already owns.
5. **A human-grounded graded register metric C(x)** that is not the same as AI-detectability or model fingerprinting (2502.12150 and the PASTA detectors measure attribution, not naturalness).
6. **A safety and oversight framing beyond CoT.** Monitorability work worries about CoT legibility. Claudish carries the same concern to *user-facing conversation and shared human-model language*. Cite 2510.27338, 2609.37312 and 2610.02015 as motivation.

† Not verified by abstract this session: gist tokens (Mu et al. 2023); "Measuring Reasoning Trace Legibility: Can Those Who Understand Teach?" and "Human Alignment: How Much Do We Adapt to LLMs?", which turned up in citation lists only and are not cited above.

## ppl

Search log: about 35 queries in total. That was 24 Semantic Scholar searches plus batch abstract fetches, 9 alphaXiv discover calls with one full-paper read, and 16 ICLR-2027 OpenReview term searches. Citation expansion covered 2410.16107 (Reinhart et al. PNAS), which returned more than 100 citers. Expansion on 2409.11704, 2605.30526 and 2610.29162 was rate-limited or returned no citers. I read the abstract of every paper below. The one exception is marked †.

| id / link | when | summary | overlap with Claudish RQ1-4 |
|---|---|---|---|
| **2610.29162** Clark, Martin, Mahowald, "Why is AI-generated language so hard to understand?" alphaxiv.org/abs/2610.29162 | 2026-10-21 | Studies 14 models (frontier, Olmo base/instruct/Think, Llama) in chat, coding-agent and essay settings, against human references. Models overuse features linked to processing difficulty: unattested bigrams, high trigram PPL, denominal verbs, inanimate subjects, that-less object relatives, long dependencies and stacked nouns. The control is round-trip translation (EN→JA→EN), which keeps meaning fixed. The effect grows Olmo base → instruct → Think, and Opus 5.5 scores lower than Opus 5. A GRPO length penalty on Qwen2.5-1.5B makes the model categorically prefer short alternations, and this transfers to untrained constructions. Comprehension and reading time are not measured, and there are no multi-turn dynamics. | **Highest overlap.** RQ1: its prevalence data across models and base vs instruct vs reasoning is very close to RQ1. RQ4: it already proposes a mechanism (compression or length pressure from RLVR/RLHF) and shows one causal toy. It does not measure likelihood preference (G_M), snowball dynamics or functional limits. |
| **2605.30526** "Measuring, Localizing, and Reducing the AI-Like Writing Style of Post-Trained LLMs" (UChicago/UIUC/TTIC, EMNLP 2026) | 2026-05-28 | Compares human text, base-model and aligned-model generations under matched human prefixes. Aligned outputs have lower human-corpus affinity and higher detector rates. PASTA estimates an "alignment signature" from aligned-minus-base residual contrasts and ablates it during decoding. Across 11 models and 6 detectors this lowers detection, and random directions do not. | RQ4 (partly): shows post-training causes AI-like style and localizes it. RQ1 base/instruct comparison. Nothing on multi-turn or PPL preference. |
| **2605.19516** "Base Models Look Human To AI Detectors" (CMU) | 2026-05-19 | GPTZero and Pangram judge base-model text as overwhelmingly human, but not the text of instruct counterparts. Iteratively paraphrasing with a minimally fine-tuned base model (HIP) humanizes text across Llama-3 and Qwen-3, 0.6B to 70B. The authors conclude that detectors track instruction-tuning artifacts. | RQ1/RQ4 support: the "AI-ness" signal comes from post-training, not from LM-ness. HIP is also a ready-made "de-Claudify" baseline for RQ3. |
| **2606.00334** "Isolating LLM Lexical Bias: Triangulated Preference Shift score" | 2026-05-06 | The metric triangulates human gold, base and instruct models to isolate lexical and format shifts caused by preference learning (delve, furthermore, formats) across 6 families, with no manual curation. | RQ4 (partly): a lexical-only version of "post-training sets a register prior". |
| **2610.23715** "The Cognitive Cost of LLM Writing: A Reading-Time Comparison" | 2026-10-16 | Trains RoBERTa-RT on eye-tracking data. On 496 human articles paired with LLM rewrites, the rewrites are shorter but have higher predicted reading time per word, with longer, rarer words and higher contextual predictability. | RQ3 C_H (partly, predicted RT only). This is the "low PPL to models, higher human cost" dissociation in one static step. |
| **2311.07484** Kuribayashi et al., "Psychometric Predictive Power of LLMs" (NAACL) | 2023-11 | Next-word probabilities from instruction-tuned LLMs predict human reading behaviour worse than base LLMs do. | RQ4: direct evidence that post-training decouples model likelihood from human processing. Key citation for "rethinking PPL". |
| 2212.12131 Oh & Schuler (TACL); 2106.01229 "Lower Perplexity is Not Always Human-Like" (ACL'21) | 2022 / 2021 | Lower-PPL and larger LMs give surprisal that fits human reading times worse. | RQ4 background: "PPL ≠ human naturalness" predates the AI era. Claudish adds the post-training and self-conditioning axis. |
| **2605.07632** "Post-training makes LLMs less human-like" (Psych-201) | 2026-05-08 | Post-training consistently reduces behavioural alignment with humans, across model families, sizes and objectives, and the gap widens in newer generations. | RQ4 context (behaviour, not text). |
| **2409.11704** Zhang et al., "From Lists to Emojis: Format Bias" (ACL) | 2024-09 | Human raters, GPT-4 and RewardBench RMs are biased towards lists, bold, links and emojis. Less than 1% biased data injects the bias, and BoN/DPO exploit it. | RQ4: mechanism for over-structuring (headings, segments). |
| **2611.03244** "Forecasting How RLHF Responds to Reward-Model Edits" (MBZUAI) | 2026-11-02 | RMs reward headers, lists and length, and policies learn these even when quality does not improve. Each feature moves in proportion to its within-prompt covariance with the reward change, which explains 82% of shifts across 54 RL runs. | RQ4: a quantitative tool for predicting which stylistic features RL amplifies. |
| **2610.01316** "What Wins a Vote? Compar:IA arena" | 2026-10-01 | 137k French arena votes. Bold adds +11% win odds per SD and lexical variety (MATTR) +16.8%. Style adjustment moves 36 of 116 models by 10 or more ranks. The bold effect is smaller in multi-turn conversations. | RQ4: human-preference source of the formatting prior. Also gives a multi-turn hint. |
| LMSYS "Style control" blog† (2024-08) | 2024 | Adjusts Arena Elo for markdown headers, lists, bold and length. | RQ4 context. Not a paper; unverified here. |
| 2611.14333 "AI Isn't Getting Dumber, We're Getting Pickier" | 2026-11-10 | On Chatbot Arena, the standards of the human judges drift over time. | RQ3 caveat: human naturalness baselines are not stationary. |
| **2510.01171** Verbalized Sampling | 2025-10 | Mode collapse after alignment is driven by typicality bias in preference data (annotators favour familiar text). | RQ4: a data-level account of why preference tuning concentrates mass on one register. |
| 2505.18949 "The Price of Format: Diversity Collapse" (EMNLP) | 2025-05 | Chat templates and structural tokens alone cause diversity collapse, which persists at high temperature. | RQ4: the template is part of the "register prior". |
| 2608.13430 "Impact of Instruction Tuning on Confidence and Lexical Diversity" | 2026-08 | Instruction tuning raises confidence, degrades likelihood calibration and cuts cross-rationale diversity (base/instruct pairs). | RQ4: likelihood shift from base to instruct. |
| 2605.06030 "More Aligned, Less Diverse? Grammar and Lexicon of Two LLM Generations" (ACL) | 2026-05 | HPSG analysis: newer instruction-tuned LLMs have lower syntactic and lexical diversity than older non-IT models and NYT text. | RQ4 "older < base < post-trained" ordering, in a diversity version. |
| 2410.16107 Reinhart et al., "Do LLMs write like humans?" (PNAS) | 2024-10 | IT models use a noun-heavy, informationally dense style, even when prompted to be informal. | RQ1 prior evidence for nominalization and abstraction. |
| 2605.23651 Register-aware human-likeness (Biber-67 + MMD, EMNLP); 2604.14111 Biber variation across 11 LLMs (EMNLP) | 2026 | All tested IT LLMs deviate from human register baselines. Chat variants cluster together, and model identity matters more than prompt or decoding. | RQ1 measurement toolkit. Also shows prompting does not remove the register. |
| 2607.21498 "Artificial Epanorthosis" | 2026-07 | The "not X, it's Y" figure is attributed to promotional pretraining data plus RLHF. An Epanorthosis Index against genre baselines shows miscalibration by register. | RQ1 signature feature. RQ4 speculation. |
| **2401.12070** Binoculars (ICML'24); **2310.05130** Fast-DetectGPT (ICLR'24); 2301.11305 DetectGPT | 2023-24 | Zero-shot detection from likelihood: probability curvature, conditional curvature, and the observer/performer cross-PPL ratio. | RQ4 instruments. Binoculars' cross-perplexity is effectively "is this text unusually likely to a model relative to its intrinsic surprise", a ready proxy for G_M. |
| 2605.01647 "Beyond Perplexity: Character Distribution Signatures" | 2026-05 | Argues that likelihood detectors hit a ceiling because RLHF makes distributions human-like. | RQ4 counterpoint to verify (it conflicts with 2605.19516). |
| 2605.06294 "Log-Likelihood, Simpson's Paradox, and Detection" | 2026-05 | The token-level likelihood signal is non-uniform across hidden space, and naive averaging destroys it. | RQ4 methods caveat: per-byte means can hide register signals, so stratify. |
| 2603.18482 "The Truncation Blind Spot" | 2026-03 | 8-18% of the tokens humans choose fall outside top-k/top-p truncation. Predictability plus lexical diversity reaches about 0.97 AUC. | RQ4: the decoding-stage part of low PPL, a confound to control. |
| 2606.08417 "Hacking Generative Perplexity" | 2026-06 | Gen-PPL is minimized by degenerate, incoherent samplers, so it is not a quality metric. | RQ4: PPL-as-quality failure, a direct citation. |
| ICLR'27 kB8Lwd90Io **PriorBPB** openreview.net/forum?id=kB8Lwd90Io | 2026 (sub.) | BPB carries a fixed per-string overhead (α/n), so longer strings score better (Spearman about -0.8 with length), and it gives spurious inverse scaling. | RQ4 G_M: Claudish and natural paraphrases differ in length, so length-match them or use PriorBPB. |
| 2611.04215 "Tokenizer Myths"; 2601.09039 "Information-Theoretic Perspective on Tokenizers" | 2026 | Recent work on cross-tokenizer comparability. | RQ4 cross-model bpb hygiene. |
| ICLR'27 wjO8F2KPxN **Tide**: detection via directional generation asymmetry openreview.net/forum?id=wjO8F2KPxN | 2026 (sub.) | Reading text in reverse makes all text harder to predict, but LLM text becomes disproportionately harder. Combining forward and reverse readings beats forward-only detectors. | RQ4 + **LEDOM**: suggests that the low forward PPL of LLM text is a left-to-right production artifact. |
| **2507.01335** LEDOM: Reverse Language Model (Yin et al., ACL) | 2025-07 | Open 2B/7B right-to-left LM trained on 435B tokens. Reverse Reward combines P(y\|x) with reverse P(x\|y) to penalize chains whose backward reconstruction degrades. | RQ4 tool owned by the mentor. Reverse bpb on Claudish vs natural text separates "predictable because of forward self-conditioning" from "intrinsically low-entropy". |
| 2505.15261 AGENT-X (Yin co-author) | 2025-05 | Zero-shot multi-agent AI-text detector with semantic, stylistic and structural guidelines from rhetoric and systemic functional linguistics. | RQ1 automatic C(x) scorer prototype (in-group). |
| 2406.18365 Themis (Yin co-author) | 2024-06 | Reference-free NLG evaluation LM. | Possible naturalness judge for RQ3 (in-group). |
| 2410.21819 Wataoka et al., "Self-Preference Bias in LLM-as-a-Judge"; 2404.13076 Panickssery et al. (NeurIPS'24); 2407.12856 "AI-AI bias" (PNAS) | 2024 | Judges favour lower-PPL (familiar) text and their own generations. LLMs prefer LLM-written pitches. ICLR'27 XhOtVNihIJ finds self-preference is smaller under strict controls. | RQ3/RQ4: model "understanding" of Claudish may be inflated by familiarity, so you need judge-independent task metrics. |
| 2606.30571 "Attractor States Emerge in Multi-Turn LLM Conversations" (MPI-IS) | 2026-06 | Self-play debates converge to model-specific attractors. Claude Haiku is a strong attractor, and partners adopt its metacommentary. | RQ2 (partly): model-model style snowball and attractors. No human readability measure. |
| 2605.29278 "Accommodation Goes Both Ways" (WildChat) | 2026-05 | On WildChat, LLMs over-converge towards users, while humans accommodate LLMs at human-human rates. | RQ2 design: in natural logs, the human-uptake channel is weak, so a lexical-uptake simulator is plausible but must be calibrated against these rates. |
| 2609.05882 "What if LLMs Ate Their Words"; 2602.24287 "Do LLMs Benefit From Their Own Words?" | 2026 | Replacing assistant history with neutral content or summaries changes performance little, and there is "context pollution" from self-history. | RQ2/RQ3 ablation precedent for assistant-only and rewritten history. |
| 2610.26008 "Accelerated Drift of Scientific Prose" (EMNLP); 2605.19936 "What Are LLMs Doing to Scientific Communication?"; 2609.07447 Korean register shift | 2026 | Population-level human writing has drifted since ChatGPT (longer words, lower readability, less depth variation). Experts rate LLM-improved text as more understandable. | RQ2 (macro-scale human uptake). The expert rating is a counterpoint for C_H. |
| 2601.11432 "Unreasonable effectiveness of pattern matching" (Jabberwocky); 2311.18805 Scrambled Bench (EMNLP); 2605.04426 Telegraph English | 2023-26 | LLMs recover meaning from text with nonce content words or scrambled characters. A symbol-dense "Telegraph English" keeps 99% of key facts at 50% compression. | RQ3: text unreadable to humans but readable to models exists for artificial corruptions. Claudish is an organically grown register, which none of these study. |
| 2602.18152 "The Statistical Signature of LLMs" | 2026-02 | LLM text is more compressible than human text (gzip), a model-agnostic regularity signal. | RQ4: a tokenizer-free complement to bpb. |
| ICLR'27 2pEA9r9V3E "How Much Is an AI Token Worth?" | 2026 (sub.) | 27-31% of mid-2026 web tokens are AI-generated. 800 pretraining runs show AI tokens first help and then hurt loss on human text. | RQ4 framing: PPL in the AI era. Future "base" models are no longer Claudish-free, so date base models carefully. |
| ICLR'27 URQZKM8CLa "Trait Hitchhiking in LLM Post-Training" | 2026 (sub.) | Price-equation model of how RL amplifies untargeted traits, validated on GRPO. | RQ4 formal mechanism for an unrewarded register drifting during RL. |
| 2609.11291 "Off-Target Effects of Response-Style Alignment" (Korean, 27B) | 2026-09 | Style post-training (lists, markdown, register) shifts untargeted behaviours such as abstention. | RQ3/RQ4: style and function are coupled in the opposite direction. |

### Does this scoop any RQ?
- **RQ1 (prevalence and inducibility): partly scooped on spontaneous prevalence, open on inducibility.** 2610.29162 already shows cross-model prevalence of difficulty-linked features in frontier, open and base/IT/Think models, with chat, coding and essays and a meaning-controlled baseline. Related evidence comes from 2605.30526, 2605.19516, 2410.16107, 2605.23651, 2604.14111 and 2606.00334. Nobody measures susceptibility under a standard induction protocol, or a maximum C within an interaction budget, or C(x) grounded in human pairwise judgments. Chinese is open: only Korean and French arena-adjacent work exists.
- **RQ2 (snowball): mostly open.** Closest are 2606.30571 (model-model attractors, Haiku metacommentary spreading), 2605.29278 (LLM over-accommodation and the weak human uptake rate) and 2610.26008 / 2605.19936 (macro drift in human prose). None runs a controlled lexical vs structural uptake loop or measures amplification slope, onset or ceiling.
- **RQ3 (what breaks first): open.** 2610.23715 gives only static, predicted human RT cost. 2601.11432, 2311.18805 and 2605.04426 show models reading artificially corrupted or compressed text. No one estimates C_H*, C_M* and C_U* on an organically amplified register, or does a non-inferiority analysis on τ²-bench.
- **RQ4 (mechanism + PPL): partly scooped on "post-training causes it", open on the likelihood formulation.** 2610.29162 offers a compression or length-pressure mechanism with a GRPO toy. 2605.30526 localizes an aligned-minus-base direction. 2409.11704 and 2611.03244 show RMs drive headers and lists. 2510.01171 points to typicality bias. 2311.07484 shows IT probabilities fit human reading worse. Nobody defines or measures G_M (bpb preference for a Claudish vs natural paraphrase) across older/base/post-trained/Claudish-context models, nor tracks it along a conversation trajectory, nor uses a reverse LM.

### Useful methods/datasets to borrow
- **Meaning-preserving paraphrase controls**: the round-trip translation control from 2610.29162 (EN→JA/RU→EN with NLLB-200), and its feature set (unattested bigrams, trigram PPL, denominal verbs, inanimate subjects, that-omission, dependency length, stacked nouns), as automatic components of C(x).
- **Base vs aligned under matched prefixes** and the PASTA residual-direction ablation (2605.30526). Use these as a causal knob for RQ4 and as a "de-Claudify" intervention. HIP base-model paraphraser (2605.19516) for natural rewrites in RQ3/RQ4 ablations. Triangulated Preference Shift score (2606.00334).
- **Human-cost proxies**: RoBERTa-RT reading-time predictor (2610.23715) for cheap C_H screening before paid human studies. Biber-67 + MMD register distance (2605.23651). Surprisal-to-RT fit from 2311.07484 to show that post-trained surprisal diverges from human cost along the trajectory.
- **Likelihood instruments**: Binoculars cross-PPL and Fast-DetectGPT curvature as per-turn trajectory signals. The local-calibration caveat from 2605.06294. PriorBPB (kB8Lwd90Io) or strict length matching for G_M. gzip compressibility (2602.18152) as a tokenizer-free check.
- **LEDOM (2507.01335) and Tide (wjO8F2KPxN)**: compute forward vs reverse bpb on Claudish vs natural paraphrases. Prediction: Claudish's advantage under forward post-trained models shrinks under reverse reading.
- **Mechanism toolkits**: format-bias injection with less than 1% data (2409.11704). The RM-edit covariance forecast (2611.03244) and Lookahead (ICLR'27 6a4v7CpeAu) to show which Claudish features an RM would amplify. The Price-equation trait-hitchhiking model (URQZKM8CLa). GRPO length-penalty toy (2610.29162).
- **Datasets**: WildChat (uptake calibration, from 2605.29278), PERSUADE 2.0 essays, the Compar:IA vote release (2610.01316) to correlate C(x) with wins, Scrambled Bench and Jabberwocky stimuli as upper-bound "model-readable" references, and assistant-history neutralization protocols (2609.05882, 2602.24287) for the RQ2/IV ablations.

### Gaps Claudish can own
1. **Recursive dynamics.** Every prior study is static (single response vs human) or model-model. A controlled human-model loop that separates lexical, structural, self-history and reciprocal uptake, with slope, onset, plateau and ceiling, is unclaimed. Calibrate the simulator's uptake rate against WildChat convergence (2605.29278) to pre-empt "unrealistic user" critiques.
2. **The three-way dissociation C_H* < C_M* < C_U*.** No one measures human comprehension or reading time, model reconstruction, and agentic task success on the same trajectory checkpoints. 2610.29162 explicitly lists the missing reader study as a limitation, so be first with real human RT and comprehension data.
3. **G_M as a likelihood-based register preference.** Report bits per byte (bpb), length-controlled, for each model class (older, base, post-trained, post-trained with Claudish context). Use the same open-model families across these classes (Olmo base/IT/Think, Llama, Qwen) so that only post-training differs. Then show the "PPL inversion": the post-trained model's bpb on Claudish text keeps falling across turns while human reading cost rises. That turns "rethink PPL in the AI era" into a measurable claim. Caveat: frozen pre-2023 base models are needed as controls, since 2026 web data is about 30% AI (2pEA9r9V3E).
4. **Reverse-LM diagnosis (mentor's edge).** Use LEDOM forward/reverse bpb asymmetry on Claudish vs natural paraphrases. This distinguishes left-to-right self-conditioning predictability from content entropy, and connects to Tide's directional-asymmetry finding. No one has applied a right-to-left LM to the AI-register question.
5. **Separating formatting priors from lexical and abstraction priors.** Format bias (2409.11704, 2611.03244, 2610.01316) and lexical bias (2606.00334) are studied in isolation. Claudish can test which channel (structural uptake vs lexical uptake) drives amplification, and whether the multi-turn attenuation of the bold effect seen in arena data holds under controlled loops.
6. **Model-readable organic register vs artificial corruption.** RQ3 can contrast a naturally amplified register with Jabberwocky, scrambled text and Telegraph English. This would show whether models' robustness to Claudish exceeds what familiarity alone predicts. Control for self-preference and low-PPL familiarity, which 2410.21819 and ICLR'27 XhOtVNihIJ show can inflate judge-based "understanding".
7. **A Chinese replication and human-grounded C(x).** None of the work found has a human-pairwise-anchored continuous register score or a Chinese study. AGENT-X and Themis from the mentor's group are natural automatic scorers to validate against human judgments.

Main competitor to track: the Clark, Martin and Mahowald group (2610.29162). A follow-up with reader studies or post-training checkpoints would overlap with RQ3 and RQ4 before May 2027, so position Claudish as the dynamic, functional-limit and likelihood-mechanism complement to it.

Scratch files are in /tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/ppl/: r1-r4.txt, abs2.txt, abs3.txt, or.txt.

---

# Appendix B: red team

## Red team: novelty

**Verdict: scoop risk is MEDIUM-HIGH for the paper as written, MEDIUM if it is refocused.** At the coarse level, the existence of the register and the claim that post-training causes it are already published (RQ1 and RQ4). The claim that "humans can't read it but models can" is published for engineered text (RQ3). One group (Clark, Martin and Mahowald) has a paper out that names a reader study as its next step, so they could take more ground before May 2027. Only one thing has no prior work: register amplification inside a human-to-model loop with realistic uptake, tied to functional breaking points. Even that is at risk of being called "obvious in-context learning" or "just LLM-LLM self-play".

Scope of my searches: 8 fresh Semantic Scholar queries (several hit 429 and were retried), 4 alphaXiv discovery runs, 17 ICLR 2027 OpenReview term searches (including "Claudish", which returned nothing), and forward-citation expansion on 2610.29162, 2606.19857 and 2606.30571 (2610.29162 returned no citers yet). New items found beyond the sweep:
- 2607.26015 (Jul 2026). Instruction-tuned Llama and Gemma models reuse more syntax from the previous turn than humans do, but not more conditional reuse. Relevant to "structural uptake".
- 2605.01130 "Iterative Finetuning is Mostly Idempotent". Traits seeded into a model mostly decay or plateau across SFT generations, and amplify only under self-preferring DPO. This is a prior *against* runaway snowballs.
- ICLR'27 geTr6F4o1U ContextEcho. Persona drift across 57 real, consented, released long agentic-coding sessions in Claude Code and Codex CLI; drift appears early and across providers. This is real long-session data you could mine for register drift.
- "Communicative convergence in human–LLM written dialogues" (Językoznawstwo, 2026-10, review). Convergence runs both ways but unevenly: models "reproduce or amplify cues present in user input".
- 2606.31725† (human vs LLM comprehension of obfuscated code; title and snippet only).
- 2610.31692 (chaos and bifurcations in continuous LM feedback). Peripheral: embedding feedback, not dialog.
- ICLR'27 1DurUu3EGy, an author rebuttal titled "Writing in AI style". Anecdote only: authors concede their dense AI-refined prose made the paper "hard to follow".

### Scoop risk per RQ

| RQ | Risk | Why |
|---|---|---|
| RQ1 prevalence and inducibility | **HIGH (about 70% done)** | 2610.29162: 14 models including Opus 5 and 5.5, ordered base < instruct < Think, with a meaning-matched control. Also 2605.30526, 2410.16107, 2605.23651, 2611.01549, 2502.12150 and "Synthetic English". Still open, but a small contribution: inducibility, the maximum reachable C, and discourse-level signatures. Note that 2610.29162 finds Opus 5.5 *lower* than Opus 5, which undercuts the idea that this register is specific to Claude. |
| RQ2 snowball | **MEDIUM** | No human-to-model register-escalation paper exists. Close neighbours are LLM-LLM attractors (2606.30571, QWlbJXhVzH), static user uptake (B3Iq9eKyiz, 2605.29278), and training-time amplification (2605.20602). Main risks: (a) reviewers see a simulated user as LLM-LLM self-play; (b) the result looks tautological if the simulator copies the model's terms; (c) the premise may fail, since real users converge only weakly (2605.29278), models over-accommodate users, and traits tend to decay (2605.01130). |
| RQ3 what breaks first | **HIGH for the headline, MEDIUM for the measurement** | Already shown: humans fail while models succeed for engineered text (BabelTele 2606.19857: human QA 56%→36%, model 90%→97%), for emergent agent languages (2609.01491), and for chain-of-thought (2510.27338). Coined terms *help* models (wY0JSEIbKB). 2610.29162 explicitly names a reader study as missing; that group is the most likely to scoop the human side before May 2027. |
| RQ4 mechanism and PPL | **HIGH coarse, MEDIUM fine** | "Post-training causes it" is covered by 2605.30526, 2610.29162 (GRPO length-penalty toy), 2611.01549, 2609.32196 and the Juzek line. Content-matched log-prob comparisons (2610.28474, 2609.32196) are essentially an unconditional G_M. "PPL ≠ human naturalness" is old (2311.07484, 2212.12131, 2106.01229), and newer work adds 2606.08417, NTOdIP7hFT and BabelTele. The *context-conditional* G_M is open, but the bare prediction "Claudish context raises the likelihood of Claudish continuations" is what in-context learning trivially predicts. |

### Top 5 problems and concrete fixes

1. **RQ1 and the coarse RQ4 are already published; the paper looks like "2610.29162 plus multi-turn".**
   *Fix:* Turn RQ1 into a calibration section, not a contribution. Reuse 2610.29162's feature set and round-trip-translation control, plus Biber-67 with MMD, as the sentence-level layer of C(x). Make the contribution the **discourse layer** no one measures:
   - conversation-local coined labels and their reuse rate;
   - promotion of passing phrases into formal concepts;
   - back-references with no antecedent in the current turn;
   - meta-structure density.

   Ground the composite C(x) in human pairwise judgments. Put 2610.29162, 2605.30526, 2611.01549 and "Synthetic English" in the first paragraph of related work, with one sentence each on what they do not do.

2. **The snowball is tautological or self-play unless the loop gain is shown to exceed one.** A simulator that reuses model terms injects Claudish by construction, and an LLM simulator makes this a variant of Ko & Geiping's attractors.
   *Fix:* Define the **loop gain** g = ΔC(model reply at turn t+1) / ΔC(injected by the user at turn t). Claim a snowball only if g > 1, meaning the model adds more than it was fed. Compare against two controls:
   - a dose-matched control, where the same total Claudish is injected once at turn 1;
   - a length-matched natural-context control.

   Calibrate the simulator's uptake rates to real logs: the B3Iq9eKyiz same-question chance baseline, 2605.29278 (WildChat), and the Behavioral Sciences turn-slope data. Add a small **real-human arm** (about 30–50 participants on Prolific, about 10 turns, within the $3k budget) so the effect is not a simulator artifact. Pre-register the null that matters: 2605.01130 says traits plateau, so a plateau below C_H* is a publishable negative result only if it was framed in advance.

3. **"Unreadable to humans, still works for models" is not new (BabelTele, GlossoGen, illegible chain-of-thought), and the Clark group will likely add reader studies.**
   *Fix:* The novelty has to be the **ordering on an organic, pressure-free trajectory**, measured on the *same checkpoints*: C_H* (measured reading time plus comprehension, not RoBERTa-RT alone), C_M* (reconstruction plus QA) and C_U* (τ²-bench pass^k with goal, policy and database fixed; certify each wording-only edit with executable task contracts, 2610.14477). Include two head-to-head arms:
   - BabelTele-style engineered compression at matched C, to show organic Claudish differs;
   - generic multi-turn degradation (2505.06120 sharded tasks plus a length-matched natural history), so register effects are separated from "lost in conversation".

   State the opposite hypothesis too: user style already moves agents (PTic6zZAch, StressSim), so C_U* < C_H* is possible and would also be interesting. Get a preprint out early, around Feb 2027, to set priority against the Clark follow-up.

4. **The G_M result will be read as "in-context learning works", and the PPL critique is old.**
   *Fix:* Test an **interaction**, not a main effect: does post-training raise the *in-context slope* dG_M/d(turn) relative to the matched base model, given an identical Claudish history (OLMo-2/3 Base→SFT→DPO→RLVR, plus Tülu and Zephyr chains)? That links the register prior to the snowball mechanistically.
   - Length-match, or use PriorBPB (kB8Lwd90Io), and stratify by frequency band (NTOdIP7hFT).
   - Use frozen pre-2023 base models as the "older" anchor, since about 27% of mid-2026 web tokens are AI-generated (2609.40295).
   - The PPL argument should be one concrete dissociation: along a single trajectory, the post-trained model's bits per byte falls while measured human reading time rises.
   - Add LEDOM forward vs reverse bits per byte (linked to Tide, wjO8F2KPxN) to test whether the low PPL is a left-to-right self-conditioning artifact. This is the lab's distinctive edge, and no one has applied it here.
   - Optional causal knob: ablate the PASTA aligned-minus-base direction and check whether g falls below 1.

5. **The name and framing are a liability.** "Claudish" is a brand name, the register is already named elsewhere ("synthetic English", "AI-like style", "idiolect"), and Clark et al. report Opus 5.5 below Opus 5. If Claude is not the strongest case across families, the name invites a fast rejection and possible objections over a trademark-like term.
   *Fix:* Use a neutral formal term such as "interaction-amplified assistant register (IAR)". Mention "Claudish" once as folk terminology. Report model-family results honestly, and drop any implication that this is specific to Claude unless the data show it. For the Chinese case, a small replication anchored on 2605.25358 and 2609.07447 is enough.

### Single most defensible novel claim

The loop gain inside an ordinary cooperative human-to-model conversation exceeds one, and it does so for **discourse-level** register features: coined local labels and compressed back-reference. Post-training is what pushes the gain above one, since base models given the same context stay below it. Along this organically produced trajectory, human comprehension fails before model understanding and agentic task success do.

### Revised headline

"Successful conversations make assistant language less human. Under realistic user uptake, post-trained models amplify their own discourse-level register turn by turn with loop gain above one. Base models do not. Text crosses measured human comprehension thresholds while τ²-bench task success stays non-inferior, and the model's bits per byte on that text keeps *falling*. Low perplexity therefore stops being evidence of naturalness."

Scratch outputs: /tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/nk/

## Red team: method

**Verdict: major revision before any compute is spent.** The question is good and RQ2 (snowball) and RQ3 (three breaking points) are still unclaimed, according to the sweep. As written, though, a methods-literate NeurIPS reviewer could dismiss every headline result:
- **Claudish is not yet a construct.** The signatures pull in opposite directions (compressed vs. over-structured), several of them depend on context, and normal human conversation produces one of them.
- **The snowball cannot be told apart from an LLM–LLM attractor.** The "user" is an LLM, so an amplification result may just be the simulator's style.
- **"Readable to models" is scored by the models that wrote the text**, with full context, using judge-based metrics.
- **G_M as written is mostly an on-policy/self-familiarity effect**, not a register preference.
- **C_H* < C_M* < C_U* compares three thresholds set with three arbitrary margins**, so the ordering is not identified.

All five can be fixed within ≤$3k API plus a modest GPU budget if the design is tightened as below.

### Problem 1: Construct validity. Claudish is not separated from (a) verbosity/jargon/AI-style, or (b) ordinary human common-ground formation.
- **Mixed and opposing signatures.** "Excessive abstraction" and "compressed anaphora" make text shorter and denser. "Over-structuring", headings and meta-comments make it longer. One scalar C(x) will mix several latent factors.
- **Overlap with established constructs.** Several items are already covered by prior work:
  - Biber nominal or informational density (2410.16107, 2605.23651)
  - processing-difficulty syntax (2610.29162)
  - format bias (2409.11704)

  If C(x) correlates highly with these, Claudish is a rename.
- **The two most novel signatures are normal human behaviour.** Coined conversation-local labels and compressed back-reference are what humans do when building common ground:
  - Referring expressions shorten across repeated reference (Clark & Wilkes-Gibbs 1986, *Cognition*).
  - Partners form "conceptual pacts" (Brennan & Clark 1996†).
  - These dynamics have been measured in repeated reference games (Hawkins et al. 1912.07199).
  - MLLMs do *not* spontaneously form such conventions (2408.01417).

  So a turn-15 excerpt being "unreadable" to a naive rater may just be common ground the rater lacks. That is not a register.
- **The name is a measurement problem.** "Claudish" implies the register is specific to one model family. RQ1 asks whether all models show it, and 2610.29162 finds Opus 5.5 is *lower* than Opus 5 on most features. If it is not Claude-specific, the name is wrong. If it is, RQ1 is not the question.

**Fix:**
1. **Factor it.** Pre-register 3–4 sub-scales:
   - (i) lexical/abstraction density
   - (ii) structural over-formatting
   - (iii) discourse-local coinage and reference compression
   - (iv) global collocational unnaturalness

   Annotate them separately, and report a factor analysis (CFA) on about 1–2k turns.
2. **Discriminant validity.** Show that human-grounded C predicts naturalness judgments *after partialling out* length, a domain-jargon rate, Biber Dim-1, a detector score (e.g., Binoculars) and the 2610.29162 feature set. Report incremental R² with CIs. If incremental R² is about 0, stop calling it a new register.
3. **Human-human control at matched depth.** Run the discourse metrics (coinage rate, anaphora distance and explicitness) on long human-human task dialogues at turn 15–20: collaborative coding chats, expert Slack threads, MultiWOZ-style or meeting corpora. Define Claudish (iii) as *excess over the human common-ground baseline*.
4. **Two kinds of rater.** Rate with the history visible ("insider") and without it ("outsider"). Only the insider deficit counts as Claudish. The outsider-minus-insider gap is common ground.
5. **Name.** Rename to a neutral term (e.g., "assistant register") in the paper and keep "Claudish" as the motivating anecdote, or test family-specificity explicitly with the idiolect classifier from 2502.12150.

### Problem 2: Measurement of C(x). Cost, reliability, matched twins, and scorer circularity.
- **Matched texts are hard to build without bias.**
  - Human pairwise judgments need *semantically matched* pairs, and every candidate way of making the natural twin brings its own register.
  - An LLM rewriter (especially Claude) brings its own register.
  - Round-trip translation (2610.29162's control) brings translationese, which the idea explicitly excludes from Claudish.
  - PASTA ablation and HIP base paraphrase (2605.30526, 2605.19516) are better options but are themselves model outputs.
- **Reliability is unknown.** Naturalness and slop judgments are known to be fairly subjective (2509.19163). SlopBench (2609.33905) shows composite style scores rank models unstably.
- **The scorer is circular.** If the automatic C scorer is an LLM judge, especially from a family under study, self-preference and familiarity bias apply (2410.21819, 2404.13076).

**Fix:**
1. **Build twins in more than one way.** Use at least 2 independent methods:
   - HIP base-model paraphrase
   - human expert edits (LAMP-style, 2409.14509), on a 150-item gold subset

   Check content equivalence with executable fact or QA contracts, not cosine similarity (2610.14477). Keep only items where every twin method agrees on direction.
2. **Fit Bradley–Terry to a reliability target.** Pre-register split-half reliability of item scores at r ≥ 0.8 and report Krippendorff's α per sub-scale. A budget sketch:
   - about 400 anchor items × about 8 comparisons each ≈ 3.2k judgments, at roughly $0.15–0.25 each on Prolific, so about $500–800
   - pilot with 100 items first to estimate the comparisons needed
3. **Automatic scorer rules.**
   - Train a small regression scorer (features plus a frozen encoder) on the BT scores.
   - Validate it on held-out topics *and* held-out generator families.
   - Never use an LLM judge from a family being scored.
   - Report scorer-vs-human agreement per family to catch differential validity.

### Problem 3: The snowball is confounded with the LLM user simulator, context length and persistence.
- **The simulator produces the style it is supposed to receive.** The "controlled user simulator" is itself a post-trained LLM, so it brings its own register prior. "Lexical uptake" by an LLM user is model–model convergence, which is already shown as attractor states (2606.30571: Haiku's meta-commentary spreads to its partner). Any amplification could come from the simulator. If the simulator shares the family, the result is self-play.
- **Persistence vs. amplification.** "Amplification" also has to be told apart from:
  - persistence: the model echoing in-context material; self-history acts as few-shot exemplars (2602.03664, 2609.05882)
  - drift from context length (a4AhDiAVL0)
  - generic multi-turn degradation (2505.06120)
- **Optimised vs. organic.** Adaptive search measures the reachable maximum, not organic drift. Mixing it in contaminates the "ordinary interaction" claim.
- **Realism of uptake rates.** In real logs, humans accommodate LLMs only at human-human rates while models over-accommodate users (2605.29278, 2508.03276). A simulator with high uptake may be unrealistic by construction.

**Fix:**
1. **Template or scripted simulator as the primary condition.** A deterministic slot-and-template user holds the intent script fixed and *inserts* extracted items from the model's last turn at a controlled dose: n coined terms, k nominalizations, a structural frame. User turns then have exactly known C_user(t), and the simulator adds no register of its own.
2. **LLM simulators as a robustness matrix only.** Use cross-family simulators, including a base-model simulator, and always measure C on user turns separately.
3. **Identify gain, not slope.** Fit C_model(t+1) = a + b·C_user(t) + g·C_model(t) + h·len(t), with conversation random effects. A snowball means the loop gain is greater than 1, or the system moves to a fixed point above the seed level, *beyond* the matched length-matched-natural-context control.
4. **Constant-dose control.** Hold C_user fixed at the turn-t level for several turns. If model C plateaus, the effect is a dose-response, not recursive amplification.
5. **Keep adaptive search separate.** Report it only as "inducibility ceiling" (RQ1).
6. **Calibrate uptake against real data.** Match the simulator's uptake rate to WildChat convergence (2605.29278) and the same-question chance baseline (B3Iq9eKyiz). Run a small real-human loop study (about 30–40 Prolific participants × 10-turn sessions, about $400–600) as an existence check.

### Problem 4: "Readable to models" is circular, and C_H/C_M/C_U are measured under unequal information.
- **Self-familiarity.** If the generating model (or its family) reads its own Claudish, any success may be self-familiarity: on-policy text has 3–4× lower entropy (2605.25459), and judges prefer their own text. That is not comprehension.
- **Unequal context.** The models get the full history and the human raters get excerpts.
- **Leakage.** QA answers may be recoverable from world knowledge.
- **Judge-scored understanding.** "Model detects Claudish and reconstructs plain language", if graded by an LLM, is circular again.

**Fix:**
1. **Reader matrix.** Rows are generator families; columns are reader families, which should include different families, base models, a small model (≤8B) and a pre-2023 model. Report the off-diagonal (cross-family) results as the main C_M result, as BabelTele (2606.19857) does with its cross-model transfer.
2. **Fact ledger.** Each trajectory is generated from a hidden structured state of facts, slots and constraints. Comprehension items are auto-generated from the ledger with exact-match answers. Use the "style kept, facts replaced" ablation with *counterfactual* facts to kill prior-knowledge leakage.
3. **Equal access.** Humans and models see the same context window: either both see the full history or both see a fixed excerpt. Report both.
4. **Reconstruction scoring.** Score plain-language reconstruction by ledger recall, not by an LLM judge.

### Problem 5: Breaking points and non-inferiority are not identified or powered, and G_M is not identified as specified.
**(a) The ordering is not identified.**
- C_H*, C_M* and C_U* each come from a different metric with its own ε, so the ordering can be flipped by choosing ε.
- Thresholds read off noisy curves have wide CIs.

**Fix:**
1. Fit a monotone or 4-parameter logistic dose-response per outcome, with outcomes normalised between two anchors: the natural twin (0) and a maximal-engineered-C anchor (BabelTele-style or scrambled, 1).
2. Define each breaking point as C where the normalised outcome loses 25% of that range. Margins are pre-registered and shared across outcomes.
3. Test the ordering with a joint bootstrap over conversations.

**(b) τ²-bench non-inferiority is underpowered.**
- Power with the standard settings (one-sided α=0.025, power 0.8, baseline pass^1 ≈ 0.7, ε = 5pp):
  - independent arms: about 1,300 episodes per arm
  - paired on the same task and seed with about 15% discordance: about 470 paired episodes *per model × C level*
- τ²-bench has only roughly 280 tasks across its domains, and repeated trials within a task are correlated.
- Each episode costs dozens of calls. At frontier pricing, a 3-model × 4-level grid eats the whole $3k.
- **Validity problem:** τ²'s user is itself an LLM. If you Claudify the user, the user may mis-transmit goals, which confounds agent robustness with simulator failure.

**Fix:**
1. Put the Claudish wording in a rewrite layer applied *after* the user sim generates each turn. Check each rewritten turn with an executable contract (2610.14477) before the agent sees it. Rewrite with a non-agent-family model.
2. Use ε = 10pp, which cuts n about 4×, and justify it.
3. Run the dense C-curve on open-weight agents served on the GPU. Spend API money only on 2 C levels × 1–2 frontier agents.
4. Report pass^k with task-clustered CIs.
5. Pre-register the margin.

**(c) G_M is not identified as specified.**
- **On-policy confound.** log P(y_Claudish|c) − log P(y_natural|c) is dominated by on-policy-ness if y_Claudish was sampled from the scored model, and by content and length differences between whole continuations. Per-byte normalisation has a length artifact (PriorBPB, kB8Lwd90Io).
- **Template confound.** Instruct models scored under a chat template vs. base models scored raw differ by format alone (2505.18949).
- **"Older < base" is confounded.** It mixes data vintage, capacity and tokenizer, and 2026 bases are already trained on about 30% AI web text (2pEA9r9V3E).

**Fix:**
1. Compute G_M on **minimal pairs**: controlled single-feature edits (nominalized vs. verbal, coined label vs. descriptive NP, header vs. prose), scored only over the edited span plus a fixed suffix.
2. Generate y_Claudish from a held-out family. Add the self vs. other × Claudish vs. natural 2×2 design.
3. Isolate post-training with the same-lineage OLMo Base→SFT→DPO→RLVR ladder (plus Tulu/Zephyr). Use Pythia or pre-2023 checkpoints as the "older" anchor, with size matched.
4. Score every model both with and without the chat template.
5. Report length-matched bpb or PriorBPB, plus a frequency-band decomposition (NTOdIP7hFT).
6. Add LEDOM reverse-LM bpb (2507.01335) and Tide-style asymmetry (wjO8F2KPxN) as the test of whether the preference depends on direction.

### Revised headline claim
"Post-trained assistants carry a measurable register prior: excess abstraction, over-structuring and discourse-local coinage, **beyond human common-ground rates and beyond length or jargon**. Under controlled, realistically calibrated user uptake, this prior shows **loop gain above 1** in some model families. Along the resulting trajectories, human comprehension (with the same context as the models) degrades at lower register intensity than **cross-family** model comprehension or τ²-bench task success. Same-lineage checkpoints show that post-training raises a minimal-pair likelihood preference for these features, and the preference grows in-context. Lower model bits per byte on this register therefore tracks model familiarity, not human naturalness."

The headline should be cut back in two cases:
- **No family shows loop gain above 1:** report "persistence with a dose-response, not a snowball".
- **The cross-family reader advantage disappears:** report "self-familiarity, not model-readability". That would still be a publishable negative result on PPL.

† Brennan & Clark 1996 is verified by title and venue only; I did not retrieve the abstract. I read the abstracts of the other papers added here: Clark & Wilkes-Gibbs 1986, Hawkins et al. 1912.07199, Hua & Artzi 2408.01417, and τ²-bench 2506.07982. Everything else is cited from the provided sweep. Source idea: /home/user/nlp-lab-paper-search/research/claudish/idea.md

## Red team: reviewer

**Reviewer persona.** A typical NeurIPS 2027 main-track reviewer in the LLM/NLP area. I have read Clark, Martin & Mahowald (2610.29162), BabelTele (2606.19857) and Ko & Geiping (2606.30571). I am sceptical of "phenomenon" papers that have no ML mechanism or method.

### Verdict

- **As written in idea.md, this is an ACL/EMNLP/COLM paper, not a NeurIPS paper.** It defines a register, measures it across models, adds a human reading study and a Chinese replication. That is a computational sociolinguistics and evaluation contribution. At NeurIPS the expected rating is **3/6 (borderline reject)**, at confidence 4. The phrase that would recur across reviews is "interesting, but incremental over 2610.29162 plus multi-turn, with a construct-validity problem in the user simulator."
- **It can be a NeurIPS paper** if it is reframed as **an in-context feedback loop with a measurable loop gain, a likelihood mechanism, and a causal post-training intervention**. That version is about learning dynamics and evaluation validity, not stylistics. With that reframing and the fixes below, a realistic outcome is 4–5/6, which is borderline-to-weak accept.
- **Fallbacks if the May 2027 deadline cannot carry the causal parts:**
  - COLM 2027 or ARR (EMNLP 2027) for the descriptive + human-study version.
  - NeurIPS Datasets & Benchmarks track if the main asset ends up being the trajectory corpus plus the human-anchored C(x) scorer.

### What the reviews would say

**Strengths:**
- The question is timely and important for oversight. It asks whether shared human–model language can drift to a point where humans can't follow it while agents still work. This carries CoT-legibility worries (2510.27338) over to user-facing dialog.
- RQ2, a controlled human-in-the-loop register snowball measured by onset, slope, plateau and ceiling, is genuinely unclaimed. Existing work covers only LLM–LLM attractors (2606.30571, QWlbJXhVzH), engineered compression (2606.19857), and pressure-driven agent languages (2609.01491).
- Measuring the three breaking points C_H*, C_M* and C_U* on the *same* samples is a clean, falsifiable design. 2610.29162 says explicitly that it has no reader study.
- The history-ablation grid (assistant-only, user-only, rewritten-natural, style-kept/facts-swapped) is a strong causal design.

**Weaknesses:**
- **Novelty.** "Post-training makes text dense and hard to process" is already shown:
  - 2610.29162: base<instruct<reasoning ordering, plus a causal GRPO length-penalty run.
  - 2605.30526 (PASTA) and 2611.01549.
  - 2410.16107 (Reinhart et al.).

  "Models read what humans can't" is also already shown (2606.19857, 2609.01491). RQ1 and the coarse half of RQ4 add little.
- **The simulator manufactures the effect.** A user simulator told to reuse the model's terms injects the register into the context by construction. Rising C(x) could just mean "we fed it Claudish." The paper needs to show *amplification* (loop gain > 1), not transmission.
- **Construct validity of C(x).** It is "grounded in human pairwise judgments", but the plan gives no sample size, reliability figures, or separation from AI-detectability and length. If an LLM judge does the scoring at scale, reviewers will call it circular, especially with Claude judging Claude.
- **RQ3 confounds:**
  - generic multi-turn degradation (2505.06120);
  - context length;
  - expression-style bias in agents (PTic6zZAch);
  - judge familiarity bias (2410.21819).

  A non-inferiority test without a pre-specified margin and a power analysis will be read as "absence of evidence."
- **RQ4 is correlational.** The ordering "older<base<post-trained" mixes up data vintage, scale and tokenizer. Bits per byte (bpb) is length-biased (PriorBPB, kB8Lwd90Io) and Claudish/natural pairs differ in length. "Rethinking PPL in the AI era" is a slogan, not a result.
- **Scope.** Four RQs, commercial plus open models, a human study, τ²-bench and a Chinese replication do not fit $3k of API spend plus a student's time. It reads as three papers.

### Is the name "Claudish" a problem?

**Yes, as the scientific term. Keep it at most as a hook.**
1. **It is model-branded while the claim is general.** RQ1 asks whether *all* models show it, and the evidence says they do (2610.29162, 2605.23651, 2611.01549). If GPT/Gemini/Qwen snowball as much or more, the name is simply wrong.
2. **Claude-specific evidence cuts both ways.**
   - 2610.29162 finds that Opus 5.5 scores *lower* than Opus 5 on most difficulty measures.
   - 2510.27338 finds that Claude is the exception for illegible CoT.
   - 2606.30571 finds that Claude Haiku is a strong *attractor*.

   Reviewers will ask whether the paper is about Claude or about post-training. You can't have both.
3. **It reads as vendor-targeted or as marketing.** Some reviewers will see it as a cheap shot. Others will see it as unserious, the same reaction "delve-speak" gets. Any affiliation that could be read as a conflict of interest makes this worse.
4. **The field already has names for this.** "Synthetic English" (English Today, 2026), "AI-like style", "slop" and "LLM idiolect" are taken. A new brand name without a new construct invites "renaming known phenomenon."

**Fix.** Use a neutral, mechanism-bearing term, for example **"assistant register"** or **"assistantese"** (the second is analogous to "translationese"; I have not checked whether it is already in use). Name the phenomenon after the dynamic, for example **"register snowball"** or **"in-context register amplification."** "Claudish" can appear once in the intro as the colloquial name users give it. Report Claude as one family among at least 6, and let the data decide who has the highest ceiling.

### Top 5 problems, each with a concrete fix

**1. Scoop exposure: "this is 2610.29162 + multi-turn" and "this is BabelTele without the prompt."**
- **Fix.** Make RQ2 + RQ3 the paper and demote RQ1 to a short setup section. In the first paragraph, state the three things no prior work has:
  - (a) loop gain in human-like interaction;
  - (b) the ordering of breaking points on an *organic*, unpressured trajectory;
  - (c) likelihood moving opposite to human cost along that trajectory.
- Add a cheap head-to-head control: the BabelTele prompt as "maximal engineered C", compared with organic snowball text at matched C, and the GlossoGen efficiency-pressure condition compared with no pressure.
- Reuse 2610.29162's feature set and round-trip-translation control as the *automatic* part of C. This shows you build on it rather than duplicate it.

**2. Simulator circularity and realism.**
- **Fix A, define loop gain.** g = ΔC(model turn t+1) / ΔC(injected by user turn t). The snowball claim is g>1 sustained over turns, against these controls:
  - **no-uptake**: the user reuses nothing from the model;
  - **semantic-only uptake**: the user reuses content but rephrases it naturally;
  - **length-matched natural**: the context grows by the same amount with natural text.
- **Fix B, dose-response.** Sweep the simulator's uptake rate and calibrate the realistic range against WildChat convergence (2605.29278, the Behavioral Sciences WildChat study) and against B3Iq9eKyiz's above-chance adoption. Report where real users sit on the curve.
- **Fix C, a small human-in-the-loop pilot.** Use about 40–60 Prolific participants × 10–15 turns on fixed tasks. This shows the slope is not a simulator artifact. Address the counter-evidence head-on: models over-converge *to* users (2605.29278, 2508.03276), and humans suppress flagged markers (2502.09606).

**3. C(x) is under-specified, and the whole paper rests on it.**
- **Fix.** Pre-register a human anchor: roughly 2–3k pairwise judgments on meaning-matched pairs, which costs about $1–1.5k on Prolific and fits the budget if API spend is trimmed.
- Fit a Bradley-Terry model and report split-half reliability and inter-annotator agreement.
- Distil the judgments into an open scorer that combines:
  - Biber-67 + MMD (2605.23651);
  - the 2610.29162 difficulty features;
  - discourse features nobody else measures: coined-label count, how often a passing phrase is later treated as an established concept, the share of anaphora whose referent sits more than k turns back.
- Validate the scorer on **held-out model families and topics**. Show it is distinct from detectability (partial out a Binoculars score) and from length.
- Never use the model under test as the judge. Use RoBERTa-RT (2610.23715) only as a cheap screen before measuring real reading time.

**4. RQ3 cannot separate register from multi-turn degradation, length, and style bias.**
- **Fix: a wording-only replay design on τ²-bench.**
  1. Freeze goals, policy and database state.
  2. Take user turns from trajectory checkpoints at C ∈ {0, q25, q50, q75, max}.
  3. Rewrite the *same* task-bearing user turns at each C level, certifying task equivalence with executable task contracts (2610.14477), not embedding similarity.
  4. Length-match the rewrites and report pass^k.
- Pre-specify the non-inferiority margin ε (for example 3 pp in pass^1) and do a power analysis. With about 100+ tasks × 4 seeds per model this is feasible on 2–3 open models plus 2 API models.
- Measure C_H* with real comprehension QA and self-paced reading on the same checkpoints.
- The output is a single plot with three thresholds and confidence intervals.

**5. RQ4 is correlational and partly scooped. The PPL claim is a slogan.**
- **Fix A, isolate post-training.** Use one lineage with released stage checkpoints, OLMo-2/3 Base → SFT → DPO → RLVR(Think), as in 2605.27878, 2609.32196 and 2610.29162. Measure *both* snowball slope and G_M at each stage. Add pre-2023 base models as AI-free controls, since about 30% of the 2026 web is AI-generated (2pEA9r9V3E).
- **Fix B, intervene causally.** Do at least one of these on a 1–8B model on the lab GPUs:
  - (i) ablate the PASTA aligned-minus-base direction (2605.30526) and show the loop gain drops;
  - (ii) run a small DPO on assistant-register vs natural pairs and show the loop gain and G_M rise.
- **Fix C, PPL hygiene.**
  - Use length-matched pairs or PriorBPB.
  - Stratify by frequency band (NTOdIP7hFT).
  - Add **LEDOM reverse bpb** (2507.01335; compare Tide wjO8F2KPxN). This tests whether the model's preference for the register is a left-to-right self-conditioning artifact. It is the lab's distinctive angle, so lead with it.
- **The concrete "PPL in the AI era" claim:** along a snowball trajectory, the post-trained model's in-context bpb on its own register *falls* while human reading time and comprehension error *rise*. Instruction-tuned surprisal already fits human reading worse (2311.07484). The paper shows the gap widening dynamically and attributes it to a specific post-training stage.

### Scoping and framing for NeurIPS specifically

- **Frame it as ML, not stylistics.** "Cooperative human–AI interaction is a feedback system. Post-training sets a prior with loop gain > 1, so language in long sessions drifts out of the human-legible region while staying inside the model-functional region. This is an oversight and evaluation problem: perplexity and LLM-judges certify the drift as fine." Connect it to monitorability and legibility (2510.27338, 2407.13692, 2610.02015).
- **Cut:**
  - most of the commercial-model breadth in RQ1 (keep about 6 families, one table);
  - the Chinese study (move to an appendix or a follow-up ACL paper);
  - the "adaptive search" induction arm (keep it only if it is cheap).
- **Keep:** loop gain with controls, three-threshold dissociation, the stage-ladder plus a causal intervention on open models, G_M with LEDOM.
- **Contributions to list:**
  1. a human-anchored, open register scorer;
  2. a snowball protocol with loop-gain estimation;
  3. the C_H* ≪ C_U* dissociation with a non-inferiority test;
  4. post-training-stage attribution plus a causal knob;
  5. a released trajectory corpus.
- **Budget.** Run API calls only for the model breadth in RQ1/RQ2 and a subset of τ²-bench. Run all mechanism work on open weights on the lab's GPUs. Spend about $1–1.5k on human data.

### The headline figure that gets it accepted

**One three-panel figure:**
- **(a) Snowball curves.** Assistant-register score C(x) against turn, for full-loop uptake vs no-uptake / semantic-only / length-matched-natural controls, across about 6 model families. Shaded confidence intervals. The loop-gain g is annotated.
- **(b) Dissociation.** The same x-axis, with three y-series:
  - human comprehension or reading time;
  - model QA / plain-language reconstruction;
  - τ²-bench pass^1 under wording-only replay.

  Mark C_H*, C_M* and C_U* with confidence intervals. The visual message is that humans cross threshold at turn ~k while task success stays inside the non-inferiority band far beyond it.
- **(c) Mechanism.** Loop gain and G_M (bpb, length-controlled) for OLMo Base → SFT → DPO → RLVR, plus the causal intervention bar. Overlay forward vs LEDOM reverse G_M. In-context forward bpb falls along the trajectory while human cost rises: the "PPL inversion."

If (a) shows g>1 only under realistic uptake rates, (b) shows a clear gap with confidence intervals, and (c) has one causal bar that moves, the paper clears the NeurIPS bar. Without (c) it is a strong EMNLP/COLM paper.

### Revised headline claim

> "In ordinary cooperative multi-turn conversation, the register that post-training instills in LLM assistants amplifies itself (loop gain > 1) once users take up model wording at realistic rates. Human comprehension falls below threshold within a few turns, while model understanding and agentic task success on τ²-bench remain non-inferior well beyond that point. The amplification grows with each post-training stage and is reduced by ablating the post-training style direction. Along the trajectory, the model's own in-context likelihood increasingly favours this register (but not under right-to-left reading). So perplexity and LLM-judges would certify as natural the very text humans can no longer read."

State this as the hypothesis to pre-register. The title should drop "Claudish", for example: *"Register Snowball: Post-Training Priors Amplify in Human–AI Conversation Until Humans, Not Models, Stop Understanding."*

No files were written. Source idea: /home/user/nlp-lab-paper-search/research/claudish/idea.md

---

# Appendix C: judges

| Judge | P(exec) | P(diff) | Sig | Sound | Clarity | Outcome | Pool | P(acc\|done) | Venue |
|---|---|---|---|---|---|---|---|---|---|
| ac | 0.55 | 0.55 | 2.6 | 2.3 | 2.7 | 0.5 | 0.45 | 0.22 | As scoped in idea.md, the paper defines a register, measures how common it is, adds a human reading study and a Chinese replication. That is a computational-sociolinguistics and evaluation paper, so ACL/EMNLP 2027 (ARR) or COLM 2027 fits best. The NeurIPS main track becomes realistic only if the paper is refocused on three things: an in-context feedback loop with loop gain above 1, against controls and with realistic uptake calibration; the C_H* << C_U* dissociation on the same organic trajectories; and a causal post-training knob, such as the OLMo stage ladder plus a PASTA-direction ablation or a small DPO, together with the LEDOM forward/reverse bits-per-byte result. If the main deliverable ends up being the trajectory corpus plus the human-anchored scorer, NeurIPS Datasets & Benchmarks is a reasonable fallback. |
| reviewer | 0.5 | 0.5 | 5.5 | 4.5 | 5.5 | 5 | 4.5 | 0.24 | As scoped in idea.md, this is best suited to EMNLP/ACL via ARR or COLM 2027. It is a descriptive register paper with a human study and a Chinese replication, which fits computational sociolinguistics and evaluation better than NeurIPS. NeurIPS main track becomes realistic only with a reframe: an in-context feedback loop with loop gain above 1, one dissociation figure showing C_H* well below C_U* with confidence intervals, and a causal post-training intervention on the OLMo stage ladder, such as PASTA direction ablation or a small DPO. If the main asset ends up being the trajectory corpus plus the human-anchored scorer, NeurIPS Datasets & Benchmarks is the fallback. |
| pi | 0.45 | 0.5 | 3 | 2.5 | 3 | 0.5 | 2.5 | 0.24 | The idea as written is a register-measurement, human-study and stylistics paper. That makes it a natural fit for ACL/EMNLP (ARR) or COLM 2027, where reviewers value construct validity and reader studies and do not need an ML mechanism. It is a credible NeurIPS main-track paper only if it is reframed as an in-context feedback loop. That version needs: loop gain g>1 against dose-matched and length-matched controls; a C_H* < C_U* dissociation on the same checkpoints; and a causal post-training knob on open models, either an OLMo Base→SFT→DPO→RLVR ladder plus PASTA-direction ablation or a small DPO, with LEDOM forward vs reverse bpb. If the main asset ends up being the trajectory corpus plus a human-anchored scorer, NeurIPS D&B is a fallback. |

- **ac**: risk: Reviewers are likely to call it '2610.29162 (Clark, Martin, Mahowald) plus multi-turn' and 'BabelTele without the prompt'. RQ1 and the coarse part of RQ4 are already published. The snowball (RQ2) may also fail empirically: real users converge only weakly, models over-accommodate to users, and traits tend to plateau. It could also be dismissed as simulator circularity or LLM-LLM attractor self-play. Meanwhile the Clark group has publicly named the missing reader study and may publish the RQ3 human side before May 2027. Finally, the construct C(x) and the brand name 'Claudish' invite construct-validity objections and 'renaming a known phenomenon' objections. · raise P: 1. Drop 'Claudish' as the scientific term in favour of something like 'register snowball' or 'assistant register', and treat Claude as one family among about 6.
2. Make RQ2 and RQ3 the paper and demote RQ1 to a calibration section. Cut the Chinese replication and the adaptive-search arm.
3. Define loop gain g. Use a scripted, dose-controlled template user as the primary condition, with no-uptake, semantic-only, length-matched-natural and constant-dose controls. Calibrate uptake rates against WildChat. Run a 40-60-person real-human loop pilot.
4. Ground C(x) in about 3k Bradley-Terry pairwise judgments with reliability reported. Factor it into subscales, and show incremental validity beyond length, Biber Dim-1, a detector score and the 2610.29162 features. Use a human common-ground baseline (insider vs outsider raters) for the discourse-coinage signatures.
5. For RQ3, use a cross-family reader matrix, ledger-based exact-match comprehension, equal context for humans and models, and wording-only τ²-bench replay with executable contracts. Pre-register margins and fit dose-response curves with joint-bootstrap CIs for the threshold ordering.
6. For RQ4, use the OLMo Base→SFT→DPO→RLVR ladder with minimal-pair, length-controlled G_M, held-out-family continuations, and LEDOM reverse bits per byte. Add one causal intervention that moves g.
7. Post a preprint around Feb 2027 to establish priority against the Clark follow-up.
- **reviewer**: risk: The paper could be read as "Clark, Martin & Mahowald 2610.29162 plus multi-turn" or as "BabelTele without the prompt". RQ1 and the coarse part of RQ4 are already published. The snowball (RQ2) can be dismissed as simulator circularity: an LLM user that reuses model terms injects Claudish by construction, which looks like LLM-LLM attractors or plain in-context copying. Several results also point toward a plateau rather than amplification: real users converge only weakly, and traits tend to decay (2605.29278, 2605.01130). If loop gain stays at or below 1, the core claim collapses into "persistence". Meanwhile the Clark group, who name a reader study as their next step, may publish the human-comprehension result before May 2027. The model-branded name "Claudish" adds a construct-validity problem and a framing problem, because 2610.29162 reports Opus 5.5 below Opus 5. · raise P: 1. Cut RQ1 and the Chinese study down to setup or appendix, and make RQ2 plus RQ3 the paper.
2. Rename to a neutral term such as "register snowball" or "assistant register", and mention Claudish once as folk terminology.
3. Make a scripted, dose-controlled template user simulator the primary condition. Define loop gain g and test it against no-uptake, semantic-only, constant-dose and length-matched natural controls. Calibrate uptake rates to WildChat, and add a 30-50 person Prolific human-in-the-loop arm.
4. Ground C(x) in about 3k Bradley-Terry pairwise human judgments, with reliability reported. Show incremental validity beyond length, Biber features, detector scores and the 2610.29162 feature set. Add discourse-level coinage and anaphora metrics, compared against a human common-ground baseline.
5. Run RQ3 as a wording-only τ²-bench replay certified by executable contracts. Pre-register the margins and do a power analysis. Use a cross-family reader matrix and a fact ledger so that self-familiarity cannot explain the model results.
6. For the mechanism, use the OLMo Base→SFT→DPO→RLVR ladder with one causal knob (PASTA ablation or a small DPO) that moves g. Add minimal-pair, length-controlled G_M, and use LEDOM reverse bits per byte as the lab's distinctive "PPL inversion" result.
7. Post an arXiv preprint around Feb 2027 to establish priority against the Clark follow-up.
8. Build one three-panel headline figure: snowball curves, the three-threshold dissociation, and the mechanism bars.
- **pi**: risk: Three risks compound. First, the paper may read as incremental over Clark, Martin & Mahowald 2610.29162 plus BabelTele 2606.19857, and the Clark group has said a reader study is their next step, so they could pre-empt RQ3 before May 2027. Second, the snowball may be unidentified: a user simulator that injects model terms creates the effect by construction, while real users converge weakly (2605.29278) and traits tend to plateau (2605.01130). The result could therefore be "persistence, not amplification" (g<1), which is a weak outcome for NeurIPS. Third, execution scope: 4 RQs, a construct-validated C(x) needing about 3k Prolific judgments, a real-human loop, an adequately powered τ²-bench non-inferiority test (hundreds of paired episodes per cell), a mechanism ladder and a Chinese replication. That is too much for one undergrad in about 6 months on $3k. The IRB and Prolific setup alone takes 1–2 months. · raise P: 1. Cut to RQ2+RQ3+mechanism. Demote RQ1 to calibration and drop Chinese to the appendix or a later paper. 2. Rename to a neutral term such as "register snowball" or "assistant register" and use "Claudish" only as the hook. 3. Make the primary snowball condition a template or scripted dose-controlled simulator. Pre-register a loop-gain g>1 test against a constant-dose control and a length-matched natural control. Add a 40-person Prolific loop as an existence check. 4. Use a fact ledger for comprehension items, give humans and models equal context, and use cross-family readers. Fit dose-response curves with shared normalized margins and a joint bootstrap over the three breaking points. 5. Run the dense τ²-bench C-curve on open-weight agents (ε=10pp, task-clustered CIs) and use the API only for 2 levels. 6. Add one causal bar on OLMo: PASTA-direction ablation or a small DPO that moves g and G_M, plus LEDOM reverse bpb as the lab's distinctive angle. 7. Start IRB now, and preprint by about Feb 2027 to set priority against the Clark follow-up. 8. Have the mentor own the mechanism and PPL section while the student owns the pipelines.

**EV = 8.4%** (P(exec) 0.5 × (0.4 + 0.6 × 0.5) × 0.24)
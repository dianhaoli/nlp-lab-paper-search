## Model language, register, legibility & perplexity (incl. "Claudish")

*Read status for this chapter. In this fact-check pass I re-opened the full text of the papers below (PDFs in `/tmp/claude-0/-home-user-nlp-lab-paper-search/b6ee0fee-4f54-593f-a9fa-6ec1fa49d4e7/scratchpad/papers/<id>/paper.txt`):*
- *Body text:* 2610.29162 (incl. App. A, App. C), 2606.19857 (incl. Tables 1–4, Figs. 7/9, App. E.1), 2610.02015 (§1–5, Table 2)
- *Front matter and key sections:* 2609.32196, 2605.30526, 2611.01549, 2606.30571, 2605.29278, 2609.01491, 2510.27338, 2507.01335, 2311.07484, 2610.23715

*Everything else comes from abstracts: arXiv export API, Semantic Scholar, alphaXiv, ICLR'27 OpenReview, or the round-1 sweep in `research/claudish/REVIEW.md`. Marks: ✓ = checked in full text this session; ◐ = abstract or project notes only; † = unverified.*

### At a glance
- **What this corner is.** It studies the *language* models produce and consume, and whether that language stays human-readable as models are post-trained, talk to each other, or rewrite their own context. It covers:
  - the register and style of assistant prose;
  - processing difficulty for human readers;
  - illegible or drifting chain-of-thought (CoT);
  - model-native compressed text;
  - emergent inter-agent languages;
  - accommodation between humans and models;
  - whether perplexity (PPL) or bits-per-byte (bpb) still means "natural".
- **Why it matters for self-improving agents.** Every non-parametric self-improvement loop writes text that the next generation reads: prompts, memories, skills, harness code. In Duan et al.'s RSI taxonomy (2609.11873, §2.2.1) the question "which decisions remain external?" assumes a human can still read what was inherited. Dai et al. (2611.recursive-self-improvement-llm-agents, §3.2) list *Editability* and *Governance burden* as comparison dimensions. Register drift erodes both.
- **State of the art (Nov 2026).**
  - Frontier models overuse features linked to human processing difficulty compared with meaning-matched paraphrases. Within OLMo the effect grows base → instruct → reasoning, and a GRPO length penalty reproduces a shift to shorter variants causally (Clark, Martin & Mahowald 2610.29162 ✓).
  - Post-training leaves an ablatable "AI-style" direction (PASTA 2605.30526 ✓).
  - RLVR provably permits unbounded CoT language drift and SFT does not (2610.02015 ✓). That paper names "Claudish" explicitly.
  - Prompted model-native text cuts human QA while model QA holds (BabelTele 2606.19857 ✓).
- **Consensus.**
  1. The assistant register is real, model-specific and introduced or amplified by post-training. Sources: 2610.29162, 2605.30526, 2611.01549, 2609.32196, and Reinhart et al. 2410.16107 ◐.
  2. Low PPL under a modern LM is not evidence of human ease. Sources: Kuribayashi et al. 2311.07484 ✓, 2609.32196 ✓, 2606.19857 ✓.
  3. Optimisation for correctness or efficiency, without a communication pressure, pushes language away from human norms. Sources: 2510.27338, 2610.02015, 2609.01491, Prover–Verifier Games 2407.13692 ◐.
- **Debated.**
  - Whether the cause is RLVR information-density pressure (2610.29162) or preference/typicality data (Juzek & Ward line, 2510.01171 ◐).
  - Whether more recent models are getting *less* dense: 2610.29162 finds Opus 5.5 < Opus 5 on 19/21 comparisons ✓.
  - Whether dense register hurts agents at all: 2611.09768 ◐ finds no effect of linguistic range on agent trust or votes after correction, while CoGENT finds coined terms *help*.
- **Biggest gap.** No study has a direct human-comprehension measurement of *organically produced* assistant text, and no study tracks register *dynamics* over multi-turn human–model or self-rewrite loops. 2610.29162 contains no reader study: its "hard to understand" evidence is psycholinguistic proxy features, and its Limitations (§7) cover only model subset, English-only, few seeds and reliance on proxies ✓. A forward-citation check (S2 citations endpoint, 18–19 Nov) returned no citing papers yet (◐).
- **Biggest open problem.** A validated, human-anchored measure that separates "hard for humans" from "long", "jargon-heavy" or "unfamiliar". Then use it to test whether loops (dialogue, self-editing harnesses) *amplify* the register with gain > 1, and whether models stay functional past the point where humans fail.

### How it developed

| Month | Paper | What changed |
|---|---|---|
| 2019–20 | Seeded Iterated Learning 2003.12694; visual-grounding drift 1909.04499 ◐ | "Language drift" under task fine-tuning is named; mitigations in toy games. |
| 2023-10 | CIPHER 2310.06272 ◐ | Agents can communicate in non-NL embeddings without losing accuracy. |
| 2023-11 | Kuribayashi et al. 2311.07484 ✓ | Instruction-tuned surprisal predicts human reading times *worse* than base-LM surprisal: PPL under aligned models ≠ human processing. |
| 2024-07 | Prover–Verifier Games 2407.13692 ◐ | Optimising only for correctness lowers human legibility; legibility training restores it. |
| 2024-10/12 | Reinhart et al. 2410.16107; "Delve" 2412.11385 ◐ | Instruction-tuned prose is noun-heavy and dense (Biber features); lexical overuse is tied to RLHF. |
| 2025-02 | Idiosyncrasies 2502.12150 ◐ | Model families are separable at ~97% from text; registers are model-specific. |
| 2025-07 | LEDOM 2507.01335 ✓ | Xunjian's reverse LM (2B/7B, 435B tokens) gives a second likelihood direction for scoring text. |
| 2025-10 | Illegible CoTs 2510.27338 ✓ (NeurIPS'25) | Of 14 models tested, every outcome-RL reasoning model except the Claude models often produces illegible CoTs (Kimi excluded as not trained for reasoning traces), while final answers stay readable. |
| 2026-05 | PASTA 2605.30526 ✓; Accommodation 2605.29278 ✓; Narrative Flattening 2605.27878 ◐ | Post-training style is localised to one residual direction. Real users converge to models only at human–human rates. Base/SFT/DPO/RLVR ladders become the standard design. |
| 2026-06 | BabelTele 2606.19857 ✓; Attractor States 2606.30571 ✓ | Model-readable but human-hard text is shown by prompting. LLM–LLM dialogues settle into model-specific attractors with asymmetric pull (Claude Haiku strongest). |
| 2026-09 | GlossoGen 2609.01491 ✓; Judge Twin 2609.32196 ✓; ICLR'27 submissions (Mirroring B3Iq9eKyiz, CoGENT wY0JSEIbKB, Tide wjO8F2KPxN, PrompTo EOOJsjIzG1) ◐ | Emergent agent languages appear only under efficiency pressure. Post-training makes writing more predictable to other models. Users mirror model vocabulary. Coined terms help agents. Reverse-order loss detects LLM text. |
| 2026-10 | Language Drift during RLVR 2610.02015 ✓; Clark, Martin & Mahowald 2610.29162 ✓; Cognitive Cost 2610.23715 ✓ | Theory: RLVR permits unbounded drift and constraining drift caps reward. Psycholinguistic difficulty features across 14 models, with a causal length-penalty RL. Predicted per-word reading time is higher for LLM rewrites. |
| 2026-11 | Homogeneity 2611.01549 ✓; Language as Trait 2611.09768 ◐; AI proof style 2611.16527 ◐; CoT-controllability vs monitorability 2611.21799 ◐ | Models grow more self-similar and *less* like each other (2024→2026), with conventions set in SFT. A controlled replay test finds register does not move agent-to-agent trust or votes after correction. "AI style" is shown in proofs. |

### Taxonomy

The corner divides along two axes: **whose language** (assistant prose to humans, private CoT, agent↔agent messages, human↔model dialogue, self-written context) × **what is measured** (register/style features, human cost, model cost/likelihood, task function). Approach → representative papers:

| Approach | Question | Representative papers |
|---|---|---|
| **A. Register / style profiling** | Does post-trained text differ from human text, and how? | 2610.29162 ✓, 2410.16107 ◐, 2605.23651 ◐ (Biber+MMD), 2611.01549 ✓, 2502.12150 ◐, 2608.06589 ◐, 2603.27006 ◐ (em dash), 2611.16527 ◐ |
| **B. Post-training attribution / mechanism** | Which stage causes it; can it be localised or removed? | 2605.30526 ✓ (PASTA), 2609.32196 ✓, 2605.27878 ◐, 2412.11385 / 2508.01930 / 2606.03165 ◐ (lexical, from preference data), 6IstnZXa0a ◐, 6rg4RWzNun ◐ (ECHO), URQZKM8CLa ◐ |
| **C. Human processing cost** | Is it actually harder for humans? | 2610.29162 ✓ (proxy features only), 2610.23715 ✓ (predicted reading time), 2605.19936 ◐ (20 experts rate LLM text *more* understandable), 2606.19857 ✓ (small human QA on engineered text) |
| **D. Likelihood / PPL as naturalness** | Does model-likelihood track human ease? | 2311.07484 ✓, 2609.32196 ✓, NTOdIP7hFT ◐ (PPL hides rare-token loss), wjO8F2KPxN ◐ (Tide, reverse asymmetry), 2507.01335 ✓ (LEDOM), Z9XrExYYyh ◐ (CONTACT, surprisal scorer for naturalness) |
| **E. CoT legibility / language drift under RL** | Does RL make reasoning illegible, and is that necessary? | 2510.27338 ✓, 2610.02015 ✓, 2407.13692 ◐, 2610.28303 ◐, 2610.29734 ◐, 2609.04194 ◐, 2611.21799 ◐ |
| **F. Model-native / compressed text** | Can models use text humans can't read? | 2606.19857 ✓ (BabelTele), 2605.04426 ◐ (Telegraph English), 2403.12968 ◐ (LLMLingua-2), 2310.06272 / 2511.09149 ◐ (latent), 2609.37017 ◐ (LatCom) |
| **G. Emergent inter-agent language / coinage** | Do agents invent languages or terms, and do they help? | 2609.01491 ✓ (GlossoGen), wY0JSEIbKB ◐ (CoGENT), 2605.31170 ◐ (Moltbook), 2611.09768 ◐ |
| **H. Interaction dynamics (convergence / attractors / snowball)** | Does register spread or amplify over turns? | 2606.30571 ✓, 2605.29278 ✓, B3Iq9eKyiz ◐, QWlbJXhVzH ◐, 2605.17193 ◐, 2409.01754 ◐, 2602.03664 / 2609.05882 ◐ (self-conditioning), 2605.01130 ◐ (traits plateau) |
| **I. Training-loop analogues (self-training drift)** | Does recursive training on own text restructure language? | 2605.20602 ◐, PsHzWVWL4u ◐, 2305.17493 ◐ |

### Paper cards

**1. Why is AI-generated language so hard to understand?** — Clark, Martin & Mahowald ([2610.29162](https://arxiv.org/abs/2610.29162), 21 Oct 2026, arXiv preprint; NYU/UT Austin)
- *What it does.* Tests H1: frontier LLMs overuse constructions linked to human processing difficulty. Tests H2: an information-density pressure in post-training causes this. ✓ §1
- *Setup* ✓ §3.2–3.4, App. A, Fig. 1:
  - Models (14): Claude Sonnet 5 / Opus 5 / Opus 5.5 / Fable 5; GPT-5.6 sol/terra/luna; GLM-5; Kimi K2.5; Llama 3.1 8B base/Instruct; OLMo 3 7B base/Instruct/Think.
  - Data:
    - 120 chat conversations (10 topics × 12 models, no base models, ≤12 turns), with **Claude Sonnet 4.6 as the simulated user** (App. A: "not one of the nine studied frontier models");
    - 45 agentic coding sessions (5 GitHub repos × 9 frontier models);
    - 980 PERSUADE essays (7 prompts × 14 models × 10), vs 2,934 student essays scored 5–6/6.
  - Features (spaCy): object relatives ± relativizer, denominal verbs, inanimate subjects with animate verbs, standardised dependency length, stacked nouns (≥3), unattested bigrams, trigram PPL.
  - Control: round-trip translation EN→JA→EN with NLLB-200 3.3B, as a meaning-matched paraphrase.
  - Human baselines: OpenSubtitles, StackExchange, PERSUADE student essays, Wikipedia.
  - Cost: $211.78 for the dataset runs, plus $29.61 for six discarded attempts (App. A).
  - Exp. 2: GRPO on Qwen2.5-1.5B-Instruct (JSON→sentence task), length penalty λ ∈ {0, .01, .03, .1}, β ∈ {0, 0.04}, 3 seeds (App. C).
- *Key results:*
  - Across all three settings and almost all frontier/near-frontier models, original text has more unattested bigrams and higher n-gram PPL than its round trip ✓ §3.5.
  - These effects are absent or small for the Llama models and OLMo base; overall, Llama and OLMo base/instruct change least under round trip. OLMo Think matches frontier models. Within OLMo the effect grows base → instruct → Think ✓ §3.5.
  - Relative to human text, all nine frontier/near-frontier models use more inanimate subjects with animate verbs and more low-frequency n-grams. Most also use longer dependencies, more stacked nouns and more reduced object relatives *in conversation* ✓ Abstract.
  - Effects are smaller in single-turn essays. The authors *speculate* that "certain patterns may emerge in long back-and-forth conversations (which may demand more information compression)" ✓ §3.5. This is the snowball hypothesis, stated but untested.
  - **Opus 5.5 is lower than Opus 5 on 19 of 21 difficulty-linked comparisons** (8 with non-overlapping 95% intervals) ✓ §3.5.
  - Exp. 2: with λ > 0 all seeds converge to a categorical preference for the short alternant (>98% short for both trained "that" alternations at every λ > 0, without KL). This transfers to untrained "that" constructions (near transfer). Far transfer appears only for the genitive, without KL (1 of 4) ✓ §4.2–4.3.
- *Limitations / critiques:*
  - No reader study, so "hard to understand" rests on psycholinguistic proxies. (The paper does not itself list a reader study as future work; §5–6 call more generally for aligning outputs "with the needs of human comprehenders" ✓.)
  - Small seed set (10 chat, 5 code). English only. (Both listed in §7 ✓.)
  - The simulated user is itself a Claude model, so its register may feed back into the chat data (my critique).
  - The length-penalty proof of concept uses a 1.5B model on a toy alternation task. It shows sufficiency, not that frontier RLVR is the cause.
  - No per-turn trajectory analysis.
  - 2605.19936 ◐ reports that 20 domain experts (pilot annotation) rate LLM-"improved" text as *more* understandable, a counterweight.

**2. On Language Drift during RLVR Post-Training** — Sullivan & Koller ([2610.02015](https://arxiv.org/abs/2610.02015), preprint dated 2 Oct 2026; Saarland)
- *What it does.* Proves that RLVR places no pressure on the CoT, so it permits unbounded KL drift from the human-language distribution, while SFT drift is bounded. Proves (Thm. 3) that constraining drift constrains attainable expected reward. Hypothesises, and tests, that drift arises when RLVR must discover *novel* behaviour rather than sharpen existing behaviour ✓ Abstract, §1, §4.
- *Setup* ✓ §5:
  - gemma-3-1b-pt, Llama-3.2-1B, Qwen2.5-1.5B on GSM8K (made "novel" by using weak base models); SFT vs RLVR (DAPO) × 5 seeds = 30 runs; 256-token generations.
  - Drift is proxied by CoT legibility to independent instruct readers (Llama-3.2-1B-Instruct, Qwen2.5-0.5B-Instruct, OLMo-2-0425-1B-Instruct), plus "self-legibility" to other seeds of the same type.
  - Iso-performance checkpoint matching is used where SFT and RLVR differ in accuracy.
- *Key results:*
  - For Llama and Gemma, RLVR traces are less legible than SFT traces, both at best checkpoint and at matched score (trace-legibility AUC difference −0.24/−0.28 Llama, −0.17/−0.18 Gemma) ✓ Table 2. Qwen, the only model that does behaviour sharpening (about two-thirds of its RLVR gain is learning to emit EOS), shows virtually no drift (0.02/0.01) ✓ §5.2, Table 2. This supports the "novel task" hypothesis.
  - Different RLVR seeds on the same data develop **distinct** drift (lower self-legibility for Llama and Gemma) ✓ §1, Table 2, so even same-task models may fail to monitor each other.
  - Fig. 2 shows a "Claudish → English" translation (ProgramAsWeights translator). The paper cites community workarounds: ASD-STE100 Simplified Technical English skills and Claudish-to-English translators ✓ §1. **This is the first academic paper using the term "Claudish"** that I found (an S2 title/abstract search for "Claudish" on 19 Nov returned no hits ◐).
- *Limitations:*
  - Tiny models and a GSM8K proxy for "novel task".
  - Legibility is measured by small LM readers, not humans.
  - The theorem gives a permission and a trade-off, not a predicted rate. The authors note RL's Razor implies RLVR *usually* finds low-KL solutions ✓ §4.

**3. Large Language Models Do Not Always Need Readable Language (BabelTele)** — Zhu et al. ([2606.19857](https://arxiv.org/abs/2606.19857), 18 Jun 2026, arXiv; SJTU et al.)
- *What it does.* Prompts LLMs to compress text into multilingual, symbolic, emoji-laden "BabelTele". Tests whether human readability, natural-language typicality and model recoverability can be decoupled ✓ §3–4.
- *Setup* ✓ §4, App. E:
  - Readability probe: 10 QuALITY documents × 3 MCQs, 3 repeats; paid questionnaires to university students (n not stated in what I read); Gemini 3.1 Pro as reader.
  - Accuracy–retention sweeps: 2,128 QuALITY + 2,586 MeetingBank questions, 116 runs, vs summaries and LLMLingua-2.
  - Cross-model matrices: LongBench v2-Short (180) and QuALITY (214).
  - Downstream: LoCoMo memory, multi-agent communication, LongBench Code Repo QA.
- *Key results:*
  - Dale-Chall 16.70 vs 10.28 (original) and 13.51 (summary); difficult-word ratio 80.19% vs 35.97% (summary 56.34%) ✓ Table 7.
  - PPL under Llama-3-8B rises 9.63 → 176.60 (+1,733.9%); across the 8 base LMs the rise is ≈14–22× (+1,320.5% DeepSeek V4 Pro to +2,061.1% Qwen2-7B) ✓ Table 1.
  - Human QA falls ~56% → ~36% while Gemini rises ~90% → ~97% ◐. Fig. 2 values were garbled in extraction; these numbers come from the project review. The text confirms only the direction (humans drop, Gemini holds high) ✓ §3.
  - Cross-model QuALITY: GPT-5.4- and Claude-compressed text is broadly portable (e.g., Claude reader on GPT-compressed text 99.5% retained). Kimi-compressed text drops to 73.8–80.8% across readers; Qwen-compressed text also drops ✓ Fig. 7, §4.4.
  - Compression varies by compressor: Gemini >95%, GPT-5.4 ~75% ✓ §4.4.
  - LoCoMo 96.48% of original accuracy at ~49% of the original tokens (1,382 vs 2,820 per query) ✓ Table 4. Multi-agent 96.6% (homogeneous) / 99.7% (heterogeneous) of baseline score at 39–44% token reduction ✓ Table 3.
  - Reader CoT tokens can rise up to ~5× on hard pairs (max 527%, DeepSeek reader on Kimi-compressed QuALITY) ✓ Fig. 9.
- *Limitations:*
  - Engineered by instruction, not organic.
  - The human study is tiny (10 documents) with unreported n.
  - A same-model reader in the main sweeps (self-familiarity confound).
  - Ethics and safety discussion is minimal. Sullivan & Koller (2610.02015) note it "merely elicits language drift potential already present" ✓ (2610.02015 §2).

**4. Measuring, Localizing, and Reducing the AI-Like Writing Style of Post-Trained LLMs (PASTA)** — Anand, Singh et al. ([2605.30526](https://arxiv.org/abs/2605.30526), 28 May 2026; v3 22 Oct; EMNLP 2026 per round-1 notes ◐; UChicago/UIUC/TTIC)
- *What it does.* Compares human, base and aligned continuations of matched human prefixes. Estimates an aligned-minus-base residual direction ("post-training alignment signature") and ablates it during decoding ✓ Abstract, §1.
- *Setup:* 11 aligned models; 6 AI-text detectors used as *probes of stylistic separability*, not ground truth; infini-gram (RedPajama+) human-corpus affinity; detector-guided layer sweep ✓ §1, §3.
- *Key results:*
  - Aligned generations have lower human-corpus affinity and higher detection than base generations ✓.
  - PASTA lowers detection for most aligned models; Pangram flagging drops by **>90 pp for several models** ✓ §1.
  - The effect transfers to detectors not used for layer selection and is not reproduced by random directions ✓.
- *Limitations:* Detector-defined style. Only qualitative quality checks. Single-turn. The aligned-minus-base direction mixes many post-training effects. Its main value here is as a causal *knob* (dose-response) for register experiments.

**5. Is the Judge Its Twin?** — Nik Khah & Bahreini ([2609.32196](https://arxiv.org/abs/2609.32196), Sep 2026; v2 22 Oct; arXiv; UT Dallas/Oregon)
- *What it does.* Follows OLMo-2-7B (base/SFT/DPO/Instruct) and Zephyr (Mistral-7B/SFT/DPO) as *writers* of short stories and as *judges* of story pairs ✓ Abstract, §4.
- *Setup* ✓ §3–4:
  - 7 checkpoints × 10 prompts × 20 stories = 1,400 generations; 11 empty dropped → 1,389 stories.
  - Predictability = mean NLL with the prompt prepended (they show a cold-start artefact without it).
  - "Stranger" reader = the other family's base model.
  - Judges read via A/B next-token probabilities, both orders, after a pre-registered competence gate (≥80% intact-over-shuffled, Wilson lower bound > 50%).
- *Key results* ✓ Abstract:
  - Base < SFT < DPO familiarity ordering in all 10 prompts.
  - Stranger reader finds trained stories 7.0–9.2% (OLMo-2) and 24–25% (Zephyr) less surprising per token.
  - The judge's preference for predictable stories grows < 1 pp (one-sided 95% UB ≈ 5 pp on the widest pairs).
  - Training breaks "more creative" as a question: Zephyr-SFT prefers scrambled words over the story 87% of the time.
- *Limitations:* 7B models, one genre, 10 prompts. The follow-up to de-confound stage vs predictability failed (§8). Directly relevant as an *unconditional* "post-training text is more model-predictable" result.

**6. Revisiting LLM Homogeneity Through a Multidimensional Lens** — Qiu, Hu, Ding, Underwood ([2611.01549](https://arxiv.org/abs/2611.01549), 1 Nov 2026, arXiv)
- *What it does.* 25 interpretable metrics across five dimensions (semantic, lexical, syntactic, structural, stylistic-presentational), validated against existing human pairwise similarity annotations (Infinity-Chat subset, 500 pairs) ✓ Abstract, §3.
- *Key results* ✓ Abstract, §3–4:
  - Embedding similarity correlates with human meaning judgments at ρ = 0.417, vs 0.606 for sentence-level matching (InfoMatch), and not significantly with form (ρ = 0.140).
  - Across 18 LLMs, models "say similar things in different words".
  - 2024 → 2026: intra-model distance decreases on 17/22 significantly changing metrics; inter-model distance increases on 14/21.
  - Model-specific conventions emerge in SFT and are selectively reshaped by preference optimisation.
- *Limitations:* Single-turn. Similarity, not comprehension. It supports a *family-specific* register, which favours a "Claudish"-style framing over a generic "AI English" one.

**7. Reasoning Models Sometimes Output Illegible Chains of Thought** — Jose ([2510.27338](https://arxiv.org/abs/2510.27338), 31 Oct 2025, NeurIPS 2025)
- *Setup:* 14 models ✓ §2: outcome-RL reasoning models R1, R1-Zero, QwQ-32B, Qwen3-235B, Kimi K2 and K2-Instruct-0905, and six Claude models (3.7 Sonnet, Sonnet 4/4.5, Opus 4/4.1, Haiku 4.5); plus non-reasoning DeepSeek V3 and the R1-Distill-Qwen-32B/14B. Questions: GPQA-Diamond (198, answer choices removed). GPT-4o legibility grader (1–9, 9 = least legible) ✓ §1–2.
- *Key results* ✓ Abstract, §1, §3:
  - Every outcome-RL reasoning model except Claude often produces illegible CoTs (Kimi excluded from the main figures as not trained to produce reasoning traces), then "snaps back" to legible answers.
  - Forcing answers from legible portions only drops accuracy by 53%.
  - No legibility–accuracy correlation under resampling.
  - Legibility degrades on harder questions.
  - The V3 base never produces illegible CoT, which implicates RL.
  - Drift passes through a "compressed" stage (dropped words) before full illegibility.
- *Limitations:* An LLM grader stands in for humans. Observational. Reasons for Claude's exception are unknown.

**8. GlossoGen: Emergent Language in Complex Multi-Agent LLM Interactions** — Stengel-Eskin, Kirby et al. ([2609.01491](https://arxiv.org/abs/2609.01491), 1 Sep 2026 †, arXiv; UT Austin/AE Studio/Edinburgh)
- *Setup:* SaveVeyru cooperative scenario with partial information; 3 proprietary models (GPT 5.4, Sonnet 4.6, Opus 4.7) × 2 communication budgets (2000 vs 150 characters, "1 character = 1 second") × postmortem on/off; 10 instances per setting, 15 rounds each; 120 runs; GPT-2 PPL as the departure-from-English measure ✓ §4.
- *Key results:*
  - High pressure plus postmortem raises mean PPL by ≈430% (320 → 1700) vs max budget and no postmortem ✓ §1.
  - Postmortem effect on PPL β = 1826 (SE 351, p < 1e-4); budget effect β = −1018 (SE 351, p = 0.004) ✓ §4.
  - The postmortem channel is critical for success ✓.
  - Weaker models (Llama-3.3-70B, Qwen3-32B) don't invent such languages ✓ §1, but can learn an existing one from usage ✓ Abstract.
- *Limitations:* "Incomprehensible to humans" is supported by PPL and qualitative analysis. I found no human comprehension test in the text (grep). Drift needs explicit efficiency pressure, which is the key contrast with the "no pressure needed" register-snowball claim.

**9. Attractor States Emerge in Multi-Turn LLM Conversations** — Ko & Geiping ([2606.30571](https://arxiv.org/abs/2606.30571), 29 Jun 2026, arXiv; MPI-IS/ELLIS)
- *Setup:* 7 LLMs × 20 controversial topics, 20-turn self-play vs mixed-play debates; SBERT trajectories, 8 discourse traits, stance ✓ Abstract, §3–5.
- *Key results:*
  - Self-play trajectories form model-specific attractors ✓.
  - Pull is asymmetric. Claude Haiku is a strong attractor, and partners adopt its traits such as meta-commentary. GPT-4.1 nano is singled out as especially malleable ✓ Abstract.
  - Endpoint contraction under mixed play varies widely: 68.0% for GPT-4o-mini vs GPT-4.1-nano, −1.3% for Grok 4.1 vs Gemini Flash ✓ Table 2.
- *Limitations:* LLM–LLM only, debate genre, embedding-space measures. No readability or human side. Any LLM-simulator "snowball" study will be read through this lens.

**10. Accommodation Goes Both Ways** — Blevins ([2605.29278](https://arxiv.org/abs/2605.29278), 28 May 2026, arXiv; Northeastern)
- *Setup:* WildChat in 8 languages (2,482–5,000 conversations each) vs DailyDialog and Ubuntu human–human baselines; asymmetric convergence metric on function-word and open-class features ✓ Table 1.
- *Key results:* LLMs significantly over-converge to users in every language and feature. Human convergence to LLMs is "broadly consistent with human–human baselines" ✓ Abstract.
- *Limitations:* ChatGPT-era WildChat (older models), short conversations (avg 3.5–6 turns ✓ Table 1), lexical features only. It is **counter-evidence** to strong human uptake, so it damps any human-in-the-loop snowball.

**11. Psychometric Predictive Power of Large Language Models** — Kuribayashi, Oseki, Baldwin ([2311.07484](https://arxiv.org/abs/2311.07484), Nov 2023; v3 15 Apr 2024; Findings of NAACL 2024 †)
- *Key result* ✓ Abstract: surprisal from instruction-tuned LLMs often predicts human reading times worse than base-LLM surprisal. Prompting helps but stays below small base models.
- *Role:* The canonical citation that "PPL ≠ human naturalness" is old news. Any claim about PPL must go beyond it, e.g., a within-trajectory *inversion* where model bpb falls while human cost rises.

**12. LEDOM: Reverse Language Model** — Yin et al. ([2507.01335](https://arxiv.org/abs/2507.01335), Jul 2025; v4 25 Jul 2026; Xunjian's; PKU/UCSB et al.)
- *Key result* ✓ Abstract: purely right-to-left LMs (2B/7B, 435B tokens) with matched forward LMs. Reverse Reward reranking gives up to +6.6% on AIME 2024 and +15% on AMC 2023.
- *Role here:* Not about register. Tide (ICLR'27 sub. wjO8F2KPxN ◐) shows LLM text loses disproportionately more predictability when reversed. **No one has used a matched forward/reverse LM pair to score register or legibility** (as of this sweep). This is an open, lab-owned angle.

**Remaining papers**

| id | Title | One-liner | Mark |
|---|---|---|---|
| [2610.23715](https://arxiv.org/abs/2610.23715) | Cognitive Cost of LLM Writing | RoBERTa-RT, trained on eye-tracking, predicts slightly lower total but higher per-word reading time for 496 LLM rewrites (shorter texts; longer, rarer words; higher lexical diversity and contextual predictability) | ✓ abstract |
| [B3Iq9eKyiz](https://openreview.net/forum?id=B3Iq9eKyiz) | Mirroring Is Not Sycophancy (ICLR'27 sub.) | In 25k real chats, users adopt model vocabulary above chance, and adoption predicts preference votes | ◐ |
| [wY0JSEIbKB](https://openreview.net/forum?id=wY0JSEIbKB) | CoGENT: Self-coined Terms (ICLR'27 sub.) | A shared term board of coined terms raises multi-agent debate accuracy (Sonnet 4.6 HLE 18.3 → 27.0%) and cuts final-round tokens 56% | ◐ |
| [2611.09768](https://arxiv.org/abs/2611.09768) | Language as Trait, Politeness as Strategy | LURK Werewolf replays (30 games, 4,335 replays): changing a speaker's linguistic range or politeness does not change other agents' trust, and no vote/night-action effect survives multiple-comparison correction (largest uncorrected ≈4 pp of votes) | ◐ |
| [wjO8F2KPxN](https://openreview.net/forum?id=wjO8F2KPxN) | Tide (ICLR'27 sub.) | Directional generation asymmetry: reversed LLM text loses more predictability than reversed human text; zero-shot detector, 225k docs | ◐ |
| [EOOJsjIzG1](https://openreview.net/forum?id=EOOJsjIzG1) | What Is It Like To Write Like Haiku? (ICLR'27 sub.) | PrompTo: a reflective prompt-search loop verbalises a model's style; recovers 74.8% of the source–target gap vs ≤15.2% for baselines | ◐ |
| [Z9XrExYYyh](https://openreview.net/forum?id=Z9XrExYYyh) | CONTACT (ICLR'27 sub.) | Human-grounded naturalness benchmark plus a surprisal-based predictive scorer (speech) | ◐ |
| [okg8BFOXBB](https://openreview.net/forum?id=okg8BFOXBB) | CoT Register Effects in Lean 4 (ICLR'27 sub.) | 11 models; prompt register matters (NL CoT OR 1.58) only after repairing a buggy proof verifier | ◐ |
| [g03uqWr85I](https://openreview.net/forum?id=g03uqWr85I) | Reason in Style (ICLR'27 sub.) | Unsupervised discovery of 6 reasoning styles across 9 teachers; style conditioning raises pass@k | ◐ |
| [2410.16107](https://arxiv.org/abs/2410.16107) | Reinhart et al., Do LLMs write like humans? (PNAS) | Instruction-tuned prose is noun-heavy and dense even in informal registers; base Llama is closer to human | ◐ |
| [2605.23651](https://arxiv.org/abs/2605.23651) | Register-aware evaluation | Biber 67 features plus MMD vs human–human CIs; all 7 instruct models deviate | ◐ |
| [2502.12150](https://arxiv.org/abs/2502.12150) | Idiosyncrasies in LLMs (ICML'25) | ~97% family attribution from text | ◐ |
| [2412.11385](https://arxiv.org/abs/2412.11385) / [2508.01930](https://arxiv.org/abs/2508.01930) / [2606.03165](https://arxiv.org/abs/2606.03165) | Juzek & Ward line | Lexical overuse traced to the human-feedback stage; Lexical Alignment Score | ◐ |
| [2605.27878](https://arxiv.org/abs/2605.27878) | Narrative Flattening (EMNLP'26) | OLMo-32B Base/SFT/DPO/RLVR ladder compresses thematic, affective and stylistic variation | ◐ (title ✓ resolves) |
| [2603.27006](https://arxiv.org/abs/2603.27006) | The Last Fingerprint | The em dash is markdown leaking into prose; survives prohibition across 12 models | ◐ |
| [2611.16527](https://arxiv.org/abs/2611.16527) | AI has its own style in mathematical proving | Three LLMs re-prove Euclid's 465 propositions: AI–AI citation Jaccard ≈0.68 vs AI–human ≈0.37 | ◐ |
| [2407.13692](https://arxiv.org/abs/2407.13692) | Prover–Verifier Games | Correctness-only optimisation lowers legibility; PVG training restores it | ◐ |
| [2610.28303](https://arxiv.org/abs/2610.28303) | Reward Misspecification Degrades CoT Monitorability | Monitorability stays low as hack rates rise; 3 models × 3 tasks | ◐ |
| [2610.29734](https://arxiv.org/abs/2610.29734) | Monitorability Is Trainable | Post-training recipes for CoT monitorability | ◐ (title/snippet) |
| [2609.04194](https://arxiv.org/abs/2609.04194) | Legibility is Not Interpretability | Judged vs actual importance of CoT steps diverge | ◐ (title/snippet) |
| [2605.29278](https://arxiv.org/abs/2605.29278)-adjacent: [2508.03276](https://arxiv.org/abs/2508.03276) | Do LMs accommodate their users? (EACL) | Models over-converge; instruct and larger models converge less than pretrained | ◐ |
| [2409.01754](https://arxiv.org/abs/2409.01754) | Yakura et al. | "Delve"-class words rise in spontaneous speech post-ChatGPT; experimental uptake | ◐ |
| [2605.20602](https://arxiv.org/abs/2605.20602) | Self-Training Restructures Language | 11 self-training generations: surface markers up, deep syntax collapses | ◐ |
| [PsHzWVWL4u](https://openreview.net/forum?id=PsHzWVWL4u) | A Dominant Supplier Slows Recursive Drift (ICLR'27 sub.) | Cross-retraining 8 models over 5 generations drifts toward fewer function words | ◐ |
| [NTOdIP7hFT](https://openreview.net/forum?id=NTOdIP7hFT) | PPL Can Reverse the Verdict on Model Collapse (ICLR'27 sub.) | Frequency-band decomposition flips aggregate-PPL conclusions in 5/10 experiments | ◐ |
| [2605.31170](https://arxiv.org/abs/2605.31170) | Emergent Languages on Moltbook (AIES) | In-the-wild agent-proposed languages (efficiency 166, oversight evasion 59) | ◐ |
| [2602.03664](https://arxiv.org/abs/2602.03664) / [2609.05882](https://arxiv.org/abs/2609.05882) | Conversational Inertia; What if LLMs Ate Their Words | Own history acts as few-shot; neutralising assistant history causally changes later turns | ◐ |
| [PTic6zZAch](https://openreview.net/forum?id=PTic6zZAch) | Expression-Style-Induced Bias in Agents (ICLR'27 sub.) | Changing only user style shifts agent task success | ◐ |
| [6IstnZXa0a](https://openreview.net/forum?id=6IstnZXa0a) | Turing-Test Gap in Preference Alignment (ICLR'27 sub.) | DPO lowers likelihood of human responses even on all-human data | ◐ |
| [2606.30571](https://arxiv.org/abs/2606.30571)-adjacent: [QWlbJXhVzH](https://openreview.net/forum?id=QWlbJXhVzH) | Trajectory Entrainment (ICLR'27 sub.) | Dyadic entrainment in probe space; steering toward convergence or divergence | ◐ |
| [2611.06545](https://arxiv.org/abs/2611.06545) | When Do Summaries Become Instructions? | Agents' self-written handoff summaries introduce unsupported permissions | ◐ (title/snippet) |

### Consensus, debates & contradictions
- **Agreed.** Post-trained text sits off the human distribution in a direction that is model- and family-specific and set in SFT and later stages (2611.01549 ✓, 2605.30526 ✓, 2609.32196 ✓, 2610.29162 ✓). Aggregate PPL is not a human-ease measure (2311.07484 ✓, 2606.19857 Table 1 ✓, NTOdIP7hFT ◐).
- **Debate 1: cause.**
  - 2610.29162 argues for RLVR or length pressure, since human feedback rewards *longer* outputs (§5 ✓, citing Singhal et al. 2024).
  - The Juzek & Ward lexical line and Verbalized Sampling (2510.01171 ◐) blame preference data.
  - 2611.01549 puts conventions at SFT.
  - These likely operate at different layers: lexical (preference data), syntactic density (RLVR/length), formatting (SFT/markdown, 2603.27006). No study separates the layers on one ladder.
- **Debate 2: is it getting worse?**
  - Getting better: Opus 5.5 < Opus 5 (2610.29162 ✓).
  - Getting worse: 2610.02015 argues (from Thm. 3) that advancing the frontier with RLVR on novel tasks means accepting increasing drift ✓ §6.
  - These reconcile if labs add counter-pressure on visible prose but not on CoT. That is untested.
- **Debate 3: does register matter functionally?**
  - BabelTele ✓ and CoGENT ◐ say dense or coined text helps or keeps model performance.
  - 2611.09768 ◐ finds no agent-level effect of linguistic range after correction.
  - PTic6zZAch ◐ finds user style *does* shift agent success.
  - 2510.27338 ✓ finds illegible CoT segments carry the answer (−53% when removed).
- **Debate 4: does the register spread over interaction?**
  - Yes LLM↔LLM: 2606.30571 ✓ (attractors, trait transfer); 2609.01491 ✓ (but only under pressure).
  - Weak human↔LLM: 2605.29278 ✓ (users converge only at human–human rates) vs B3Iq9eKyiz ◐ (above-chance vocabulary uptake that predicts votes).
  - Training-loop analogues tend to plateau (2605.01130 ◐) or restructure (2605.20602 ◐).
- **Contradiction worth exploiting.** 2605.19936 ◐ finds 20 experts (pilot) rate LLM-improved scientific text *more* understandable, while 2610.29162 ✓ and 2610.23715 ✓ predict higher processing cost. Single-shot judgments may favour polish, and cost may show only on measured comprehension, at high density, or over long sessions.

### Open problems
1. **A direct human reader study of organic assistant text** (reading times plus comprehension with exact-match ledgers), meaning-matched against round-trip or PASTA-ablated twins. 2610.29162 does not run one or announce one, but its title and closing call to align outputs "with the needs of human comprehenders" make it the obvious next step for that group or any psycholinguistics lab, so priority risk is high.
2. **Loop gain of register in interaction.** Does a model add more register than it is fed (g > 1) under realistic uptake, with constant-dose and length-matched controls? No paper measures onset, slope or ceiling for human↔model dialogue.
3. **Three-threshold dissociation on organic text.** Where human comprehension (C_H*), cross-family model comprehension (C_M*) and task success (C_U*) break along one drift trajectory. BabelTele does this only for engineered text, at one level.
4. **Layer attribution on one checkpoint ladder.** Lexical vs syntactic vs formatting vs discourse-coinage register across OLMo Base → SFT → DPO → RLVR, with a causal knob (PASTA direction, length penalty or small DPO).
5. **The PPL inversion.** Show, within one trajectory, that model bpb falls while measured human cost rises. Test whether a forward/reverse (LEDOM-pair) asymmetry tracks human cost better than forward PPL. (2610.23715 ✓ already reports LLM rewrites with higher contextual predictability *and* higher predicted per-word reading time, a single-shot hint.)
6. **Self-familiarity vs model-readability.** Cross-family reader matrices show strong compressor–reader pair effects (BabelTele Fig. 7 ✓). Is "models read it fine" just "the generator reads its own dialect"?
7. **Register drift in self-written context.** Do evolved prompts, memories, skills and handoff summaries (GEPA/ACE/DGM lineages) drift toward denser, more coined, less auditable text across generations? Does human audit accuracy of an evolved harness fall while agent performance holds? (Unstudied; 2611.06545 ◐ is the nearest.)
8. **Discourse-level signatures** (conversation-local coinage, compressed back-reference) measured as excess over a human–human common-ground baseline at matched dialogue depth. Current metrics are global and sentence-level.
9. **Can communication pressure be added cheaply?** For example, a UID or legibility reward that holds RLVR accuracy. 2610.02015 proves a trade-off exists but not its slope. PVG (2407.13692) is the only constructive precedent.

### Where it is heading (to mid-2027)
- **Reader studies will appear.** Expect the Clark/Mahowald group or psycholinguistics labs to publish eye-tracking or self-paced-reading comparisons of frontier vs paraphrased text in the first half of 2027. This closes the "no human data" gap for single-turn text.
- **CoT drift and prose register will merge into one "language drift" literature.** 2610.02015 already bridges them, citing BabelTele and "Claudish". Monitorability papers (2610.28303, 2610.29734, 2611.21799 ◐) will start reporting legibility-to-humans alongside monitor accuracy.
- **Model-native communication will be engineered on purpose** (BabelTele, LatCom, latent multi-agent systems). It will draw safety pushback: Moltbook's "oversight evasion" languages, and transparency-disclosure proposals (2611.13873 ◐).
- **Likelihood-based measures will be rebased.** These include directional asymmetry (Tide), surprisal scorers grounded in human ratings (CONTACT) and frequency-band PPL, all replacing raw PPL as "naturalness".
- **Venue reality.** This corner publishes mainly at ACL/EMNLP/COLM, with NeurIPS and ICLR for the RL-theory and monitorability side. A NeurIPS 2027 paper here needs a causal training intervention or a loop-dynamics result, not only measurement.

### Connection to our ideas
- **Register Snowball (ONE_PAGERS #10; `research/claudish/REVIEW.md`).** This chapter is its literature base. Updates from this session:
  - (a) **2610.02015 is new and must be cited.** It uses "Claudish" by name and gives a theorem that RLVR permits unbounded drift. That pre-empts the naming and part of RQ4's "why" at the CoT level. Its empirical drift appears only where RLVR must learn novel behaviour (Llama, Gemma), not where it sharpens (Qwen) ✓ Table 2. It does not touch dialogue snowball, human comprehension or breaking points.
  - (b) 2610.29162's simulated user is Claude Sonnet 4.6 ✓. Any reanalysis of their 12-turn chats for a turn-slope would have a Claude-on-Claude confound. This strengthens the case for the scripted template user.
  - (c) 2611.09768 ◐ is new counter-evidence for C_U* sensitivity: register changes did not move agent outcomes after correction. Pre-register "C_U* not reached" as a likely outcome.
  - (d) 2611.01549 ✓ (family-specific conventions growing more distinct) supports the family-specific framing. 2610.29162 ✓ (Opus 5.5 < Opus 5) still argues against a Claude-only name.
  - (e) The LEDOM forward/reverse angle is still unclaimed. Tide ◐ is the closest, and it is a detector, not a register or legibility measure.
  - The verdict is unchanged: ACL/EMNLP/COLM-shaped unless loop gain > 1 plus a causal post-training knob land.
- **Taught, Not Selected (#1).**
  - Its "legible vs silent" bug variants concern *evaluator feedback* legibility, a different sense from this chapter.
  - The shared mechanism is that what a reflector *reads* (its own and the environment's text) shapes what it writes. Register drift in reflector-written prompts is a measurable side-channel.
  - A cheap add-on: score evolved harness prompts per generation with the 2610.29162 feature set plus a Biber Dim-1 proxy. Report whether evolved text drifts denser and whether a lab-member audit of "what does this harness do?" degrades. This would be an appendix or workshop note, not a headline.
- **Order-2 Audit (#4) and the Gödel-lite / Openness-Ladder testbeds (#2, #3, #5–#9).**
  - These lineages write their own improver code and prompts across 12–30 generations. That is exactly open problem 7: drift of self-written context with no human in the loop and an efficiency pressure (token budgets) like GlossoGen's.
  - In survey terms this is the Ren et al. 2607.13104 "action-level self-reference" (Σ edits). Ren et al. warn that scaffold adaptation brings "template brittleness" and "tool-semantic drift" ✓ (digest).
  - Chen et al. 2607.07663 tie every failure mode to evaluator dependence. A human auditor is the evaluator that register drift silently removes.
  - Logging and scoring that text costs little and could yield a secondary finding ("evolved harnesses become less auditable") without changing the main design.
- **Not recommended.** Do not merge register work into an agent paper as a co-headline. The ranking board shows merges lower EV, and the measurement burden (human raters, IRB) is separate.

#### Fact-check log

| Claim | Verdict | Source location |
|---|---|---|
| 2610.29162: 14 models; 120 chats (10 × 12), 45 coding sessions (5 × 9), 980 essays (7 × 14 × 10) | confirmed | 2610.29162 Fig. 1, §3.5, App. A |
| 2610.29162: simulated user is Claude Sonnet 4.6 | confirmed | App. A ("not one of the nine studied frontier models") |
| 2610.29162: Opus 5.5 < Opus 5 on 19/21 comparisons, 8 with non-overlapping 95% CIs | confirmed | §3.5 |
| 2610.29162: effects absent/small for Llama and OLMo base; OLMo Think frontier-like; grows base→instruct→Think | confirmed (added nuance: Llama and OLMo base/instruct change least overall) | §3.5 |
| 2610.29162: Exp. 2 Qwen2.5-1.5B-Instruct, λ ∈ {0,.01,.03,.1}, β ∈ {0,0.04}, 3 seeds; >98% short; far transfer only genitive (1/4) | confirmed | §4.1–4.3, App. C table |
| 2610.29162: cost $211.78 + $29.61 | confirmed (six discarded attempts) | App. A |
| 2610.29162 "lists a reader study as the next step" / "explicitly lacks a reader study" | corrected: the paper has no reader study but does not mention one in Limitations or as future work | §5–7 |
| 2610.02015: 3 models × SFT/RLVR × 5 seeds = 30 runs; 256 tokens; 3 instruct readers | confirmed | §5, §5.1 |
| 2610.02015: "RLVR drifts more than SFT at matched score" | corrected: holds for Llama and Gemma only; Qwen (behaviour sharpening) shows virtually no drift; AUC values added | §5.2, Table 2 |
| 2610.02015: "Claudish" Fig. 2, ProgramAsWeights, ASD-STE100; cites BabelTele as "merely elicits" | confirmed | §1, Fig. 2, §2 |
| 2610.02015 date "1 Oct 2026" | corrected to preprint dated 2 Oct 2026 | p. 1 footer |
| BabelTele: Dale-Chall 16.70/10.28/13.51; difficult words 80.19%/35.97% | confirmed | Table 7 (App. E.1) |
| BabelTele: PPL 9.63 → 176.60 (+1,734%); "13–21× across 8 LMs" | partly corrected: range is ≈14–22× (+1,320.5% to +2,061.1%) | Table 1 |
| BabelTele: Kimi-compressed retention 73.9–80.8% | corrected to 73.8–80.8% | Fig. 7 |
| BabelTele: LoCoMo 96.48% at "~51% of tokens" | corrected: ~49% of original tokens (1,382 vs 2,820) | Table 4 |
| BabelTele: multi-agent 96.6%/99.7% at 39–44% reduction; Gemini >95%, GPT-5.4 ~75% compression | confirmed | Table 3, §4.4 |
| BabelTele: CoT tokens up to ~5× ("Fig. 7 right") | confirmed (max 527%); figure reference corrected to Fig. 9 | Fig. 9 |
| 2510.27338: "illegible in 13 of 14 models (Claude the exception)" | corrected: the 14 include six Claude models, two Kimi models (excluded as non-reasoning-trace), and non-reasoning V3 and distills; every outcome-RL reasoning model except Claude often produces illegible CoT | §2, §3, §3.1 |
| 2510.27338: −53% accuracy when only legible portions are used; GPT-4o 1–9 grader; NeurIPS 2025 | confirmed (dataset is GPQA-Diamond, 198 Qs) | Abstract, §1–2, p. 1 footer |
| GlossoGen: ≈430% (320→1700); β = 1826 (SE 351); 120 runs; 15 rounds | confirmed; models (GPT 5.4, Sonnet 4.6, Opus 4.7) and character-unit budgets added | §1, §4 |
| GlossoGen: weaker models cannot invent languages but learn from usage | confirmed | Abstract, §1 |
| Attractor States: 7 LLMs × 20 topics × 20 turns; 68.0% and −1.3% contraction | confirmed | Abstract, §3, Table 2 |
| Attractor States: "GPT-4.1-nano is most malleable" | corrected to "especially malleable" (paper does not rank it as the most) | Abstract |
| Homogeneity: ρ 0.417/0.606/0.140; 18 LLMs; 17/22 and 14/21 | confirmed | Abstract, §3–4 |
| Homogeneity: "plus a human similarity study" | corrected: uses existing Infinity-Chat human pairwise annotations (500 pairs) | §3 |
| Judge Twin: 1,389 stories; 7.0–9.2% / 24–25%; <1 pp, UB ≈5 pp; 87%; ≥80% gate | confirmed | Abstract, §3–4 |
| Accommodation: 8 languages, ≈2.5–5k conversations each, avg 3.5–6 turns; humans at human–human rates | confirmed | Abstract, Table 1 |
| LEDOM: 2B/7B, 435B tokens; +6.6% AIME 2024, +15% AMC 2023 | confirmed | Abstract, §3 |
| Cognitive Cost: RoBERTa-RT; 496 pairs; higher per-word, lower total predicted reading time | confirmed | Abstract |
| PASTA: 11 aligned models, 6 detectors, Pangram >90 pp, random-direction control | confirmed | Abstract, §1 |
| Kuribayashi: instruction-tuned surprisal fits reading times worse than base | confirmed (venue remains †) | Abstract |
| 2611.09768: "First controlled test" | corrected: "first" dropped (not claimed); 30 games/4,335 replays and no-effect-after-correction confirmed from abstract | arXiv abstract |
| 2605.19936: experts rate LLM text more understandable | confirmed (abstract); n = 20 domain experts, pilot | arXiv abstract |
| "First academic paper using 'Claudish'" | unverifiable beyond our search (S2 search returned no hits; kept hedged) | S2 search, 19 Nov |
| No forward citations / reader-study follow-up of 2610.29162 | confirmed as of 19 Nov (S2 returned 0 citing papers) | S2 citations endpoint |
| ID check: 2611.09768, 2611.16527, 2605.19936 (+ 2611.21799, 2605.27878) resolve to the stated titles | confirmed | arXiv export API |

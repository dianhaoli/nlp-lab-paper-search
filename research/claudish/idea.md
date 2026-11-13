# The Claudish Snowball: When Language Gets Worse but Models Keep Working
(Research idea from Xunjian Yin, pasted by Dan on 2026-11-13.)

YXJ RQs:
1. Do all models exhibit Claudish?
2. Can Claudish snowball through multi-turn conversations between humans and models, becoming increasingly severe, to the point that the language is almost unreadable to humans but still readable to models?
3. When the language becomes unreadable to humans, can models still execute tasks normally?
4. Can we explain the emergence of this phenomenon from the perspective of post-training? And what does it make us reconsider about the role of PPL in the AI era?

## Core idea
Post-trained models often write in a register that is intelligible but not natural. It usually leaves syntax, semantics and task ability intact. What goes wrong is a set of stylistic habits:
- excessive abstraction density
- over-structuring
- idiosyncratic terminology
- compressed anaphora
- phrasing that is coherent locally but stiff overall

We call this register "Claudish". Hypothesis: Claudish comes from a subtle register prior set during post-training. Multi-turn conversation then amplifies it recursively, through model self-conditioning and through humans picking up the model's words. Human-perceived fluency may fall a lot while model understanding and task performance stay stable, at least for a while.

Central question: how far can Claudish snowball before model functionality finally breaks?

## 1. Defining Claudish
Claudish is a graded, model-associated register: recurring lexical and discourse choices that make text feel less natural without necessarily breaking grammar or meaning.

Candidate signatures:
- excessive abstraction and nominalization
- coined labels and terminology local to the conversation
- compressed or implicit reference back to earlier turns
- dense parentheticals and mixed technical identifiers
- too many headings, segments and meta-comments
- collocations that are interpretable locally but unnatural overall
- treating a phrase used in passing earlier as an established formal concept

Claudish is not the same as verbosity, jargon, grammatical error, translationese, or AI-generated text in general. It is a continuous variable C(x). C(x) is grounded in human pairwise judgments over semantically matched texts and validated across topics and held-out models. Automatic scorers are measurement tools, not the definition.

## 2. Research questions

### RQ1. Prevalence and inducibility
Do all tested post-trained models show a spontaneous or reliably inducible Claudish tendency? For each model, measure:
- the spontaneous tendency under neutral prompts
- susceptibility under a standard induction protocol
- the maximum Claudishness reached within a fixed interaction budget

Compare commercial models, open-weight instruct models, and base/instruct pairs. English is the main language; Chinese is a smaller replication.

### RQ2. Snowball
Start from a mildly Claudish seed. At each turn, a controlled user simulator reuses terms, abstractions or discourse structures from the model's previous response, while keeping the user's next intent unchanged.

Measures:
- amplification slope
- turns until onset
- phase transitions and plateaus
- persistence
- the model-specific ceiling

Comparisons: lexical uptake, structural uptake, self-conditioning, direct prompting, and adaptive search that responds to the model's replies.

### RQ3. What breaks first
Take checkpoints along the trajectory and measure:
- human naturalness, reading time and comprehension
- whether models detect the Claudish and can reconstruct plain language
- semantic QA and instruction recovery
- reasoning, tool-call correctness and end-to-end task success

Estimate three breaking points: C_H* (humans), C_M* (model understanding) and C_U* (task use). Use τ²-bench or a similar benchmark, with goals, policies and database state held fixed and only the conversational wording changed. Test with a non-inferiority margin.

### RQ4. Mechanism
Define the model's conditional preference for Claudish:

G_M(c) = [log P(y_Claudish | c) − log P(y_natural | c)] / bytes

Predicted ordering: older models < base models < post-trained models < post-trained models with Claudish context. Cross-model comparisons use bits per byte.

## 3. Experiments
- **I. Prevalence and inducibility**, including base vs instruct.
- **II. Snowball**, with these controls:
  - no uptake
  - semantic uptake
  - lexical uptake only
  - structural uptake only
  - assistant self-history only
  - full reciprocal loop
  - length-matched natural context
- **III. Functional limit.** C* = max C(x) such that task performance ≥ baseline − ε.
- **IV. Mechanism**, using these context ablations:
  - full history
  - assistant-only history
  - user-only history
  - history rewritten as natural text with the facts kept
  - Claudish style kept with the facts replaced
  - shuffled or length-matched history

## 4. Chinese cross-lingual case study

## Central claim
Claudish is not just a model writing badly. It is a mild register tendency tied to post-training that ordinary, successful interaction can amplify recursively. Language may become unnatural well before models stop understanding it or using it effectively.

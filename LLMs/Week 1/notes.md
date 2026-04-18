# Week 1: Theoretical Notes — Foundations: Language Models + NLP Basics

## Overview
Week 1 sets the base for the course: (1) what a **language model** is (a probability distribution over token sequences) and how LLMs evolved, and (2) what **NLP** is, why language is hard for machines (ambiguity), and the core processing layers and tasks.

---

## Lecture 1 — Introduction and Recent Advances

### AI → ML → DL → LLMs (taxonomy)
- **Artificial Intelligence (AI)**: building systems that exhibit intelligent behavior.
- **Machine Learning (ML)**: systems learn patterns from data rather than hand-written rules.
- **Deep Learning (DL)**: ML using multi-layer neural networks.
- **Large Language Models (LLMs)**: large deep-learning models specialized for language understanding and generation.

**Why it matters**: this clarifies what “LLM” is (and is not). An LLM is a DL model trained for language; not every AI system is an LLM.

**Related Assignment Questions**: Q2 (distributional meaning), Q3 (ambiguity), Q9 (entailment).

### What is a language model?
A **language model** assigns probabilities to sequences of tokens.

If the token sequence is $x_1, x_2, \dots, x_n$, a standard (autoregressive) factorization is:

$$P(x_1, x_2, \dots, x_n) = \prod_{t=1}^{n} P(x_t \mid x_{<t})$$

Intuition: the model is a powerful “next-token predictor”. Generating text is repeated sampling/selection from $P(x_t\mid x_{<t})$.

**Token**: a unit the model operates on (often a word piece / subword). Tokens are not always whole words.

**Related Assignment Questions**: (Background for the week; no direct MCQ here, but it supports all).

### Evolution of LLMs (very high level)
- **ELMo (2017)**: early contextual representations (~94M parameters).
- **Transformer era**: Transformer architectures enabled strong scaling and parallel training.
- **BERT / T5**: strong “understanding” and text-to-text modeling.
- **GPT-2 → GPT-3 (175B)**: large-scale autoregressive models showed strong few-shot behavior.
- **PaLM / ChatGPT / Gemini / LLaMA (mid-2024 era)**: larger models + better instruction tuning + tooling.

**Related Assignment Questions**: none directly; gives context for the course.

### Scaling laws and in-context learning
- **Scaling laws**: as model size, data size, and compute increase (in a balanced way), performance tends to improve predictably.
- **In-context learning (ICL)**: the model can perform a task from the prompt (instructions + a few examples) without updating weights.

**Why it matters**: it explains why prompting works and why larger models can generalize better.

### Ethical concerns
Common issues discussed with LLMs:
- **Bias**: training data encodes societal biases.
- **Toxicity**: models may generate unsafe/offensive outputs.
- **Hallucination**: fluent but incorrect content; models optimize likelihood, not truth.

**Related Assignment Questions**: none directly (but important for the course framing).

---

## Lecture 2 — Introduction to Natural Language Processing (NLP)

### What is NLP, and why is it hard?
**Natural Language Processing (NLP)** studies how computers analyze, understand, and generate human language.

NLP is hard primarily because of **ambiguity** and because language is noisy in the real world.

#### Ambiguity types (with examples)

### Lexical ambiguity
A **word** has multiple possible meanings.
- Example: “bank” (river bank vs financial bank).

**Related Assignment Questions**: Q1, Q7, Q10.

### Syntactic ambiguity
A **sentence structure** allows multiple parses.
- Example: “Ravi saw the man with the binoculars.”
  - Parse A: Ravi used binoculars to see.
  - Parse B: the man had binoculars.

**Related Assignment Questions**: Q3.

### Pragmatic ambiguity
Meaning depends on speaker intent / world knowledge.
- Example: “Can you pass the salt?” is usually a request, not a question about ability.

**Related Assignment Questions**: Q4.

### Punctuation ambiguity
Punctuation changes interpretation.
- Example: “Let’s eat, Grandma!” vs “Let’s eat Grandma!”

**Related Assignment Questions**: (Conceptual background).

---

## NLP processing layers (from word form to meaning)

### Morphology (word structure)
- **Morpheme**: smallest meaning-bearing unit.
- **Stemming**: heuristic chopping (may produce non-words).
- **Lemmatization**: dictionary-based normalization to a canonical form.

### Morphemes: prefix, root, suffix
Example breakdown:
- `unhappiness` = `un` + `happy` + `ness`
  - `un-` (negation prefix)
  - `happy` (root)
  - `-ness` (nominalizing suffix)

**Related Assignment Questions**: Q5.

### Syntax (sentence structure)
Typical tasks:
- **POS tagging** (part-of-speech)
- **Chunking** (shallow phrase grouping, e.g., noun phrases)
- **Parsing** (full syntactic structure)

**Related Assignment Questions**: Q3.

### Semantics (meaning)
Common perspectives:
- **Decomposition / compositional semantics**: meaning composed from parts + structure.
- **Ontological semantics**: uses structured world knowledge/ontologies.
- **Distributional semantics**: meaning from context (co-occurrence patterns).

#### Distributional Semantics
Core idea: *“You shall know a word by the company it keeps.”*

- Statement (i) “Meaning is defined by relationships to other words” matches distributional semantics.
- Statement (ii) “Meaning does not rely on surrounding context” contradicts it.

**Related Assignment Questions**: Q2.

### Pragmatics & discourse (beyond one sentence)
- **Pragmatics**: implied meaning, intent, and context.
- **Discourse**: coherence across sentences (how meaning is built across a passage).

**Related Assignment Questions**: Q4.

---

## The NLP Trinity
Every NLP problem can be framed by:
1. **Language** (e.g., English, Hindi)
2. **Task** (e.g., translation, sentiment)
3. **Algorithm** (rules, classical ML, neural nets, LLMs)

**Why it matters**: changing any one of these can change what works and what fails.

---

## Core NLP tasks you should recognize

### Coreference resolution
Deciding what a pronoun or referring expression points to.
- Example: “Anita met Kavya… She was tired.”
  - Without more context, “she” is ambiguous.

**Related Assignment Questions**: Q4.

### Semantic role labeling (SRL)
Answering: “Who did what to whom, for whom, with what?”

Common roles:
- **Agent**: doer of action
- **Patient/Theme**: entity acted upon
- **Beneficiary**: entity that benefits from the action
- **Instrument**: tool used

Example: “The chef cooked pasta for the guests.”
- Agent = chef
- Patient/Theme = pasta
- Beneficiary = guests

**Related Assignment Questions**: Q6.

### Word sense disambiguation (WSD)
Choosing the correct meaning (sense) of a word in context.

**Related Assignment Questions**: Q7 (counting senses), Q1.

### Named entity recognition (NER) and information extraction (IE)
- **NER**: identify entities (person, location, organization).
- **IE**: extract structured facts/relations from text.

**Related Assignment Questions**: Q8 (why “Goa” is not tricky).

### Textual entailment (Natural Language Inference)
Given a premise (Sentence 1) and a hypothesis (Sentence 2), decide whether the hypothesis is:
- **Entailed** (must be true)
- **Contradicted** (must be false)
- **Neutral** (unknown)

Example:
- Premise: “Arjun bought a brand-new laptop.”
- Hypothesis: “Arjun owns a laptop.” → typically **entailed** under normal assumptions.

**Related Assignment Questions**: Q9.

---

## Lexical semantics: word relations

### Polysemy vs. homonymy
- **Polysemy**: one word with multiple **related** meanings (e.g., “paper” = material / academic paper).
- **Homonymy**: same form, **unrelated** meanings (often historically unrelated).

**Related Assignment Questions**: Q1.

### Synonymy, antonymy
- **Synonymy**: similar meaning.
- **Antonymy**: opposite meaning.

**Related Assignment Questions**: Q1, Q10 (by elimination).

### Meronymy (part–whole relation)
**Meronymy** is a **part–whole** relation.

- Example: engine → car (an engine is a *part of* a car)

**Related Assignment Questions**: Q10.

### Hyponymy (is-a relation)
**Hyponymy** is a category (“is-a”) relation.

- Example: car → vehicle (a car *is a* vehicle)

**Related Assignment Questions**: Q10.

---

## Real-world text issues: social media and informal language

### Non-standard English
Deviations from standard spelling/grammar (e.g., “workin”, “d” for “the”).

### Neologisms
Newly coined words/hashtags/blends (e.g., “#workcation”).

**Related Assignment Questions**: Q8.

---

## Key Takeaways

| Concept | Definition | Example |
|:---|:---|:---|
| Language model | Probability model over token sequences | $P(x_t\mid x_{<t})$ next-token prediction |
| Scaling laws | Predictable gains with scale | Larger model + more data → better performance |
| In-context learning | Task learning from prompt examples | Few-shot prompting |
| Lexical ambiguity | Word has multiple meanings | bank, paper, rose |
| Syntactic ambiguity | Multiple parses | “man with the binoculars” |
| Morphology | Word structure and morphemes | un + happy + ness |
| Distributional semantics | Meaning from context/co-occurrence | “company it keeps” |
| Coreference resolution | Resolve pronouns/references | “She was tired” |
| SRL | Who did what to whom/for whom | guests = beneficiary |
| Textual entailment | Logical relation between sentences | bought laptop ⇒ owns laptop |
| Meronymy | Part–whole relation | engine is part of car |
| Non-standard + neologisms | Informal / new forms in text | “workin”, “#workcation” |

---

## Summary
- LLMs are deep learning models for language; a language model is a probability distribution over token sequences.
- NLP is hard due to ambiguity; ambiguity can be lexical, syntactic, pragmatic, or punctuation-based.
- NLP processing goes from morphology → syntax → semantics → pragmatics/discourse.
- Week 1 assignment concepts map strongly to lexical relations, ambiguity, morphology, SRL, social-media text issues, and entailment.

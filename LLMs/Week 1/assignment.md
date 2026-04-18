# Week 1: Assignment 1 — Introduction to Large Language Models (LLMs) (noc26_cs88)

> **Due Date:** —
> **Submitted:** —
> **Total Score:** —

---

## Section 01 — Lexical Semantics (Word Meaning Relations)

### Q1 (1 point)
**Consider the word "paper" in the following sentences:**

"She submitted her research paper."  
"He wrapped the gift in paper."

The word "paper" is an example of:

- [ ] A) Homonymy
- [ ] B) Synonymy
- [ ] C) Polysemy
- [ ] D) Antonymy

**Answer:** C

**Answer Explanation**: This tests **polysemy**: one word form with multiple **related** meanings. “Research paper” (a written document) and “paper” (material) are historically and conceptually connected. **Homonymy** would require unrelated meanings that just share the same form; **synonymy** and **antonymy** are relations between *different* words, not senses of the same word.

**Related Topic**: [Polysemy vs. homonymy](notes.md#polysemy-vs-homonymy)

---

### Q10 (1 point)
**Which relation holds between "engine" and "car"?**

- [ ] A) Synonymy
- [ ] B) Antonymy
- [ ] C) Meronymy
- [ ] D) Hyponymy

**Answer:** C

**Answer Explanation**: This tests **meronymy** (part–whole relation). An engine is a **part of** a car. **Hyponymy** is an “is-a” relation (car is-a vehicle), which doesn’t apply here. Synonymy/antonymy are meaning-similarity/opposites, also not applicable.

**Related Topic**: [Meronymy (part–whole relation)](notes.md#meronymy-partwhole-relation)

---

## Section 02 — Distributional Semantics

### Q2 (1 point)
**Based on Distributional Semantics, which of the following statements is/are true?**

(i) The meaning of a word is defined by its relationship to other words.  
(ii) The meaning of a word does not rely on its surrounding context.

- [ ] A) Both (i) and (ii) are correct
- [ ] B) Only (ii) is correct
- [ ] C) Only (i) is correct
- [ ] D) Neither (i) nor (ii) is correct

**Answer:** C

**Answer Explanation**: Distributional semantics is based on the idea that a word’s meaning is reflected by the **contexts** it appears in (“company it keeps”). So (i) is correct. Statement (ii) is the opposite of the core idea: distributional meaning *does* rely on surrounding context.

**Related Topic**: [Distributional Semantics](notes.md#distributional-semantics)

---

## Section 03 — Ambiguity and Core NLP Tasks

### Q3 (1 point)
**The sentence "Ravi saw the man with the binoculars." is ambiguous because of:**

- [ ] A) Lexical ambiguity
- [ ] B) Morphological ambiguity
- [ ] C) Syntactic ambiguity
- [ ] D) Pragmatic ambiguity

**Answer:** C

**Answer Explanation**: The ambiguity comes from **sentence structure** (attachment ambiguity): “with the binoculars” can modify “saw” (Ravi used binoculars) or “the man” (the man had binoculars). The word meanings themselves don’t change, so it’s not lexical or morphological ambiguity.

**Related Topic**: [Syntactic ambiguity](notes.md#syntactic-ambiguity)

---

### Q4 (1 point)
**Consider the sentences:**

"Anita met Kavya after work. She was tired."

Who does "she" most likely refer to?

- [ ] A) Anita
- [ ] B) Kavya
- [ ] C) Both equally
- [ ] D) Cannot be determined

**Answer:** D

**Answer Explanation**: This tests **coreference resolution**. With only these two sentences, both Anita and Kavya are grammatically valid antecedents for “she”, and the text provides **no disambiguating context**. Humans might guess based on extra assumptions, but in strict NLP terms (and per the accepted answer), it **cannot be determined** from the given information.

**Related Topic**: [Coreference resolution](notes.md#coreference-resolution)

---

### Q6 (1 point)
**In the sentence "The chef cooked pasta for the guests.", what is the semantic role of "the guests"?**

- [ ] A) Agent
- [ ] B) Patient
- [ ] C) Beneficiary
- [ ] D) Instrument

**Answer:** C

**Answer Explanation**: This tests **semantic role labeling (SRL)**. “The chef” is the **Agent** (doer), “pasta” is the **Patient/Theme** (thing being acted upon), and “the guests” are the **Beneficiary** (the action is done *for* them). “Instrument” would be the tool used (e.g., pan), which is not mentioned.

**Related Topic**: [Semantic role labeling (SRL)](notes.md#semantic-role-labeling-srl)

---

## Section 04 — Morphology

### Q5 (1 point)
**What is the correct morpheme breakdown of the word "unhappiness"?**

- [ ] A) un + happy + ness
- [ ] B) unhappi + ness
- [ ] C) un + happi + ness
- [ ] D) unhappiness

**Answer:** A

**Answer Explanation**: This tests **morphology** and the idea of a **morpheme** (smallest meaning-bearing unit). `un-` is a negation prefix, `happy` is the root, and `-ness` turns the adjective into a noun meaning a “state”. Options with `happi` treat a spelling change as a different morpheme; the underlying morpheme is still `happy`.

**Related Topic**: [Morphemes: prefix, root, suffix](notes.md#morphemes-prefix-root-suffix)

---

## Section 05 — Lexical Ambiguity, Senses, and Noisy Text

### Q7 (1 point)
**Below is a sentence with lexical ambiguity:**

"Rose rose to put rose roes on her rows of roses."

How many senses of *rose* are present in the sentence?

- [ ] A) 2
- [ ] B) 3
- [ ] C) 1
- [ ] D) 4

**Answer:** D

**Answer Explanation**: This tests **lexical ambiguity** and **word senses**.

In this tongue-twister, *rose* appears with multiple meanings:
- Rose (proper noun): a person’s name
- rose (verb): past tense of “rise”
- rose (adjective/noun modifier): the color/variety sense
- roses (noun): the flower

That yields 4 distinct senses. (Note: “roes” and “rows” are different spellings and are not counted as *rose* senses.)

**Related Topic**: [Lexical ambiguity](notes.md#lexical-ambiguity)

---

### Q8 (1 point, multiple select)
**What issues can be observed in the following text?**

"On a much-needed #workcation in beautiful Goa. Workin & chillin by d waves!"

- [ ] A) Idioms
- [ ] B) Non-standard English
- [ ] C) Tricky Entity Names
- [ ] D) Neologisms

**Answer:** B, D

**Answer Explanation**: This tests robustness to **informal/social media text**.

- **Non-standard English**: “Workin”, “chillin”, and “d” for “the” deviate from standard spelling/grammar.
- **Neologisms**: “#workcation” is a newly coined blend/hashtag.

It’s not an **idiom** (no fixed expression like “spill the beans”), and “Goa” is a well-known location entity, so it’s not a “tricky entity name” in the usual NER sense.

**Related Topic**: [Non-standard English](notes.md#non-standard-english), [Neologisms](notes.md#neologisms)

---

## Section 06 — Textual Entailment

### Q9 (1 point)
**Sentence 1:** Arjun bought a brand-new laptop.  
**Sentence 2:** Arjun owns a laptop.

- [ ] A) Entailed
- [ ] B) Contradicted
- [ ] C) Neutral

**Answer:** A

**Answer Explanation**: This tests **textual entailment (NLI)**. Under normal assumptions about “bought” (a purchase implies possession/ownership), Sentence 2 must be true if Sentence 1 is true, so it is **entailed**. It is not contradicted, and it is not neutral because the ownership follows logically from buying.

**Related Topic**: [Textual entailment (Natural Language Inference)](notes.md#textual-entailment-natural-language-inference)

---

## Answer Key Summary

| Q | Answer | Concept |
|:--:|:--:|:---|
| 1 | C | Polysemy vs homonymy |
| 2 | C | Distributional semantics |
| 3 | C | Syntactic ambiguity |
| 4 | D | Coreference resolution |
| 5 | A | Morphology (morphemes) |
| 6 | C | Semantic role labeling (beneficiary) |
| 7 | D | Lexical ambiguity (word senses) |
| 8 | B, D | Non-standard English + neologisms |
| 9 | A | Textual entailment |
| 10 | C | Meronymy (part–whole) |

# Week 4: Assignment 4 — Introduction to Large Language Models (LLMs) (noc26_cs88)

> **Due Date:** 2026-02-18, 23:59 IST
> **Submitted:** —
> **Total Score:** —

---

## Section 01 — Word Representations (Count-based and Embeddings)

### Q1 (1 point)
**State True or False.** The Continuous Bag of Words (CBOW) model predicts the surrounding context words given a center target word.

- [ ] A) True
- [ ] B) False

**Answer:** B

**Answer Explanation**: The statement describes **Skip-gram** (center word → context). **CBOW** does the reverse: it uses surrounding context words to predict the **center/target** word.

**Related Topic**: [CBOW vs Skip-gram](notes.md#cbow-vs-skip-gram)

---

### Q2 (1 point)
**What is the main drawback of representing words as one-hot vectors?**

- [ ] A) They cannot capture semantic similarity between words.
- [ ] B) They are computationally inefficient.
- [ ] C) They cannot incorporate word order effectively.
- [ ] D) They are not robust to unseen words.

**Answer:** A

**Answer Explanation**: One-hot vectors treat every word as orthogonal to every other, so they cannot encode that “cat” and “dog” are closer in meaning than “cat” and “refrigerator”.

**Related Topic**: [One-hot encoding limitations](notes.md#one-hot-encoding-limitations)

---

### Q7 (2 points)
**For tf-idf weighting, if a word $t$ appears in every single document in a collection of size $N$, what is its $\text{idf}_t$ value?**

- [ ] A) 1
- [ ] B) $N$
- [ ] C) $\log_{10} N$
- [ ] D) 0

**Answer:** D

**Answer Explanation**: A common IDF definition is $\text{idf}(t)=\log\left(\frac{N}{\text{df}(t)}\right)$. If $t$ appears in every document, then $\text{df}(t)=N$, so $\text{idf}(t)=\log(1)=0$.

(The log base doesn’t matter here because $\log(1)=0$ for any base.)

**Related Topic**: [TF‑IDF](notes.md#tf-idf)

---

### Q8 (1 point)
**What does the term "self-supervision" mean in the context of Word2Vec?**

- [ ] A) Using labels provided by human annotators
- [ ] B) Automatically generating labels based on the relationship between words and their context
- [ ] C) Supervising the training process manually
- [ ] D) Employing external datasets for additional supervision

**Answer:** B

**Answer Explanation**: Word2Vec creates training examples directly from raw text (center/context relationships), so the data provides its own supervision without human labels.

**Related Topic**: [Self-supervision (in Word2Vec)](notes.md#self-supervision-in-word2vec)

---

### Q9 (1 point)
**What is the key concept underlying Word2Vec?**

- [ ] A) Ontological semantics
- [ ] B) Decompositional semantics
- [ ] C) Distributional semantics
- [ ] D) Morphological analysis

**Answer:** C

**Answer Explanation**: Word2Vec is built on **distributional semantics**: words that occur in similar contexts tend to have similar meanings, which embeddings capture geometrically.

**Related Topic**: [Distributional semantics (the core idea)](notes.md#distributional-semantics-the-core-idea)

---

## Section 02 — Tokenization (BPE and WordPiece)

### Q3 (1 point)
**A toy corpus has an initial base vocabulary of 6 unique characters:** {a, b, c, g, s, t}. If the BPE algorithm performs exactly three merge operations, what will be the final size of the vocabulary?

- [ ] A) 6
- [ ] B) 3
- [ ] C) 9
- [ ] D) 10

**Answer:** C

**Answer Explanation**: Each BPE merge creates one new token type and adds it to the vocabulary. Starting from 6 characters, after 3 merges the vocabulary size is $6 + 3 = 9$.

**Related Topic**: [BPE (Byte Pair Encoding)](notes.md#bpe-byte-pair-encoding)

---

### Q4 (1 point)
**Which formula is used by WordPiece to compute the score of a pair of tokens (Token A and Token B) to determine if they should be merged?**

- [ ] A) Score = Frequency(A, B) + Frequency(A) + Frequency(B)
- [ ] B) Score = Frequency(A, B) / (Frequency(A) × Frequency(B))
- [ ] C) Score = Frequency(A, B) × (Frequency(A) + Frequency(B))
- [ ] D) Score = (Frequency(A) × Frequency(B)) / Frequency(A, B)

**Answer:** B

**Answer Explanation**: WordPiece merges based on a score that rewards pairs that occur together unusually often compared to their individual frequencies: $\frac{\text{freq}(AB)}{\text{freq}(A)\,\text{freq}(B)}$.

**Related Topic**: [WordPiece merge score](notes.md#wordpiece-merge-score)

---

### Q5 (1 point)
**How does the WordPiece algorithm identify tokens during the tokenization of a new word?**

- [ ] A) It applies merge rules successively in the exact order they were learned during training.
- [ ] B) It identifies the longest subword starting from the beginning of the word that exists in the vocabulary.
- [ ] C) It uses the Viterbi algorithm to find the most probable sequence of tokens.
- [ ] D) It randomly splits the word into subwords and calculates the frequency of each.

**Answer:** B

**Answer Explanation**: WordPiece tokenization commonly uses greedy **longest-match** segmentation: take the longest subword prefix in the vocabulary, then continue on the remainder.

**Related Topic**: [WordPiece segmentation (tokenizing new words)](notes.md#wordpiece-segmentation-tokenizing-new-words)

---

### Q6 (1 point, multiple select)
**Which of the following statements are true about subword tokenization? (Select all that apply)**

- [ ] A) Frequently used words should be decomposed into smaller subwords to save space.
- [ ] B) Rare words should be decomposed into meaningful subwords.
- [ ] C) It serves as a middle ground between word-based and character-based tokenization.
- [ ] D) It consists of two components: a token learner to generate a vocabulary and a token segmenter to process raw text.

**Answer:** B, C, D

**Answer Explanation**: Subword tokenization typically keeps frequent words intact and splits rare words into meaningful pieces (B). It is the practical middle ground between word- and character-level tokenization (C). In practice, it involves a learner (to build the vocabulary) and a segmenter (to tokenize new text) (D).

**Related Topic**: [Tokenization strategies](notes.md#tokenization-strategies)

---

## Answer Key Summary

| Q | Answer | Concept |
|:--:|:--:|:---|
| 1 | False | CBOW vs Skip-gram |
| 2 | A | One-hot limitation (no similarity) |
| 3 | C | BPE merges increase vocab |
| 4 | B | WordPiece merge score |
| 5 | B | WordPiece greedy longest-match |
| 6 | B, C, D | Subword tokenization properties |
| 7 | D | TF‑IDF: idf = 0 if in all docs |
| 8 | B | Self-supervision |
| 9 | C | Distributional semantics |

# Week 4: Theoretical Notes — Word Representations and Tokenization

## Overview
Week 4 answers a core NLP question: how do we convert words into numbers that preserve meaning? We cover sparse representations like **one-hot** and **TF‑IDF**, then dense **word embeddings** (Word2Vec, fastText, GloVe) rooted in **distributional semantics**. We also study modern **tokenization** methods (BPE, WordPiece) and why **subword tokenization** is the practical middle ground for large language models.

---

## Why word representation matters
Neural networks operate on vectors, so text must be converted into numeric representations. The choice of representation controls what information is easy (or impossible) for a model to learn.

---

## One-hot encoding

### One-hot encoding limitations
In **one-hot encoding**, each word is represented by a vector of length $|V|$ (vocabulary size) with a single 1 and the rest 0s.

Main drawbacks:
- **No semantic similarity**: two different words are orthogonal, so “cat” is as far from “dog” as it is from “refrigerator”.
- **High dimensional and sparse**: vectors are mostly zeros.

**Related Assignment Questions**: Q2.

---

## Distributional semantics (the core idea)
**Distributional semantics**: words appearing in similar contexts tend to have similar meanings (“company it keeps”).

This idea underlies both count-based and prediction-based word representations.

**Related Assignment Questions**: Q9.

---

## Count-based representations

### Term–context matrix
A **term–context matrix** counts how often a word (term) co-occurs with context words (often within a window). This captures global co-occurrence statistics but becomes large and sparse.

### TF-IDF
**TF‑IDF** weights a term by:
- **TF**: term frequency in a document
- **IDF**: inverse document frequency across the corpus

A common IDF form:

$$\text{idf}(t)=\log\left(\frac{N}{\text{df}(t)}\right)$$

If a term appears in every document, then $\text{df}(t)=N$ and:

$$\text{idf}(t)=\log(1)=0$$

**Related Assignment Questions**: Q7.

---

## Prediction-based representations: Word2Vec

### Dense word embeddings
**Embeddings** are low-dimensional dense vectors (e.g., 100–300 dims) learned so that semantically related words end up close in the embedding space.

### CBOW vs Skip-gram
Word2Vec has two classic training objectives:

- **CBOW (Continuous Bag of Words)**: predict the **center/target word** from surrounding context words.
- **Skip-gram**: predict **surrounding context words** from the center/target word.

So: “predict context given center” describes **Skip-gram**, not CBOW.

**Related Assignment Questions**: Q1.

### Negative sampling
The softmax over a large vocabulary can be expensive. **Negative sampling** approximates training by updating parameters for:
- the true (positive) target/context pair, and
- a small set of sampled **negative** pairs.

### Self-supervision (in Word2Vec)
Word2Vec is **self-supervised** because labels are derived from raw text itself:
- from a sentence, form (center, context) pairs as training examples.

No human annotation is required.

**Related Assignment Questions**: Q8.

---

## fastText (subword-aware embeddings)
Word2Vec struggles with rare/OOV forms. **fastText** represents a word as a sum of character n-gram embeddings, improving robustness for rare words, misspellings, and morphology.

---

## GloVe (Global Vectors)
**GloVe** combines ideas:
- count-based: uses global co-occurrence statistics
- prediction-style learning: learns embeddings by fitting a weighted objective over co-occurrence counts

A common intuition: ratios of co-occurrence probabilities encode meaning differences.

---

## Tokenization strategies

### Word vs character vs subword
- **Word-based**: simple, but huge vocab and OOV problems.
- **Character-based**: tiny vocab, but long sequences and weak atomic meaning.
- **Subword-based**: middle ground; keep frequent words intact, decompose rare words into meaningful pieces.

**Related Assignment Questions**: Q6.

### Subword tokenization components
Most subword systems have two pieces:
- **Token learner**: learns a vocabulary from training text (e.g., BPE/WordPiece training).
- **Token segmenter**: tokenizes new text using that vocabulary.

**Related Assignment Questions**: Q6.

---

## BPE (Byte Pair Encoding)
BPE (as used in NLP tokenization) typically:
1. starts from characters (or bytes)
2. repeatedly merges the most frequent adjacent pair
3. adds each merge as a new token type

If you start with vocabulary size $|V_0|$ and do $m$ merges, then the final vocabulary size is:

$$|V|=|V_0|+m$$

(assuming each merge creates a new token type).

**Related Assignment Questions**: Q3.

---

## WordPiece

### WordPiece merge score
WordPiece uses a score to decide merges; a common form is:

$$\text{Score}(A,B)=\frac{\text{freq}(AB)}{\text{freq}(A)\cdot\text{freq}(B)}$$

**Related Assignment Questions**: Q4.

### WordPiece segmentation (tokenizing new words)
At inference/tokenization time, WordPiece commonly uses a greedy longest-match strategy:
- pick the **longest subword prefix** that is in the vocabulary
- repeat for the remaining suffix

**Related Assignment Questions**: Q5.

---

## Key Takeaways

| Concept | Definition | Example |
|:---|:---|:---|
| One-hot | Sparse $|V|$-dim vector | “cat” = 0…010…0 |
| One-hot drawback | No similarity + sparse | cat vs dog distance ≈ cat vs fridge |
| TF‑IDF | TF × IDF weighting | common-in-doc, rare-in-corpus words |
| IDF | rarity across documents | if term in all docs ⇒ idf = 0 |
| Distributional semantics | meaning from context | similar contexts ⇒ similar vectors |
| CBOW | context → center word | predict “sat” from surrounding words |
| Skip-gram | center word → context | predict neighbors given center |
| Negative sampling | efficient training approximation | update few negatives per step |
| Self-supervision | labels from raw text | (center, context) pairs |
| BPE | frequency-based merges | each merge adds a token |
| WordPiece | score-based merges + greedy segmenter | longest subword prefix |
| Subword tokenization | word/char middle ground | rare words split into pieces |

---

## Summary
- One-hot vectors are sparse and cannot encode semantic similarity.
- TF‑IDF and co-occurrence matrices are count-based; Word2Vec is prediction-based.
- Word2Vec’s CBOW and Skip-gram differ by the prediction direction; training is self-supervised.
- fastText adds subword n-grams to improve robustness for rare/OOV forms.
- Tokenization matters: subword methods (BPE/WordPiece) balance vocabulary size and OOV handling.

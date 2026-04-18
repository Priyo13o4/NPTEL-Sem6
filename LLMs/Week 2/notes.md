# Week 2: Theoretical Notes — Statistical Language Models: N-grams, Smoothing, Evaluation

## Overview
Week 2 covers **statistical language models (SLMs)**: how to compute $P(w_1\dots w_n)$ using the chain rule, how the **Markov assumption** yields **N-gram** models, why sparsity causes zero probabilities, and how **smoothing** fixes this. We also cover model evaluation via **extrinsic** (task-based) and **intrinsic** metrics, especially **perplexity**. Finally, we work through a small corpus example to connect the formulas to the Week 2 assignment.

---

## Lecture 3 — Introduction to Statistical Language Models

### What is a (statistical) language model?
A **language model** assigns probabilities to sequences of tokens/words. For a word sequence $w_1,\dots,w_n$:

$$P(w_1,\dots,w_n)$$

**Applications**: auto-completion, speech recognition, machine translation, spell checking.

**Related Assignment Questions**: Q4–Q6 (sentence probability), Q6 (perplexity).

### Chain rule (decomposition)
The chain rule expands sequence probability as:

$$P(w_1\dots w_n)=\prod_{i=1}^{n} P(w_i\mid w_1\dots w_{i-1})$$

This is exact, but conditioning on the entire history is impractical.

### Markov assumption
The **Markov assumption** approximates the history with a fixed window of the last $k$ words:

$$P(w_i\mid w_1\dots w_{i-1}) \approx P(w_i\mid w_{i-k}\dots w_{i-1})$$

- If $k=1$: **first-order** Markov → **bigram** model.
- If $k=2$: **second-order** Markov → **trigram** model.
- If $k=3$: **third-order** Markov → **4-gram** model.

**Related Assignment Questions**: Q1.

### N-gram models (unigram, bigram, trigram)
- **Unigram**: $P(w_i)$ (no context)
- **Bigram**: $P(w_i\mid w_{i-1})$
- **Trigram**: $P(w_i\mid w_{i-2}, w_{i-1})$

With sentence boundary tokens, a bigram sentence probability often looks like:

$$P(\text{<s>}\, w_1 \dots w_n \,\text{</s>}) \propto \prod_{i=1}^{n+1} P(w_i\mid w_{i-1})$$

(where $w_{n+1}=\text{</s>}$; many assignments ignore $P(\text{<s>})$).

### Maximum Likelihood Estimation (MLE) for N-grams
For a bigram model:

$$P( w_i \mid w_{i-1}) = \frac{\text{Count}(w_{i-1}, w_i)}{\text{Count}(w_{i-1})}$$

This is the most common starting point.

### Sparsity and the “zero-probability” problem
Most possible N-grams never appear in a finite corpus (typical tables are extremely sparse). With MLE, unseen N-grams get probability 0, and because we multiply probabilities, a single 0 makes the whole sentence probability 0.

**Related Assignment Questions**: Q4.

### OOV and the `<UNK>` token
If a test word was never seen in training, it is **out-of-vocabulary (OOV)**. A common fix is to map rare/unseen words to a special token like `<UNK>` during training so the model can assign probability mass to “unknown word here”.

### Basic smoothing
Smoothing redistributes probability mass so unseen events get non-zero probability.

#### Add-one (Laplace) smoothing
For bigrams:

$$P_{+1}(w_i\mid w_{i-1}) = \frac{\text{Count}(w_{i-1}, w_i)+1}{\text{Count}(w_{i-1})+V}$$

where $V$ is the vocabulary size.

#### Add-k smoothing
Same idea but add $k$ (often $k<1$) instead of 1.

#### Back-off and interpolation
- **Back-off**: if an N-gram is unreliable/unseen, fall back to a smaller model (trigram → bigram → unigram).
- **Interpolation**: mix models, e.g., $\lambda_3 P_{3\text{-gram}}+\lambda_2 P_{2\text{-gram}}+\lambda_1 P_{1\text{-gram}}$.

**Related Assignment Questions**: Q5.

---

## Lecture 4 — Advanced Smoothing and Evaluation

### Why add-one can be problematic
Add-one smoothing can distort probabilities by pushing too much mass toward unseen events, especially with large vocabularies.

### Good-Turing smoothing
Uses **frequency of frequencies**.

Let $N_c$ be the number of types that occur exactly $c$ times. Adjusted count:

$$c^* = \frac{(c+1)\,N_{c+1}}{N_c}$$

Intuition: how many “once-seen” events exist helps estimate how much mass should go to unseen events.

**Related Assignment Questions**: (Concept coverage for Week 2; not directly asked).

### Absolute discounting
Subtract a fixed discount $D$ (often around 0.75) from non-zero counts and redistribute the leftover mass.

Intuition: treat each observed N-gram count as slightly “overconfident,” then allocate the removed probability to unseen continuations.

### Kneser-Ney smoothing (unique context / continuation)
**Kneser-Ney** uses the idea of a **continuation probability**: a word should be more likely if it appears after **many different previous words** (many unique contexts), not just if it appears many times in one fixed phrase.

This “unique preceding contexts” property is the key distinction.

**Related Assignment Questions**: Q2.

---

## Evaluating language models

### Extrinsic vs intrinsic evaluation
- **Extrinsic evaluation**: plug the LM into a downstream task and measure task performance (e.g., BLEU for machine translation).
- **Intrinsic evaluation**: evaluate the LM directly, often using **perplexity** on a held-out set.

**Related Assignment Questions**: Q3.

### Perplexity
For a test sequence $W=w_1\dots w_n$:

$$\text{PP}(W)=P(W)^{-\frac{1}{n}}$$

Interpretation: lower perplexity means the model assigns higher probability to the test text (less “surprise”).

#### Relation to entropy
Perplexity is related to entropy $H$ (base-2) via:

$$\text{PP}=2^H$$

(Shannon-McMillan-Breiman gives theoretical grounding for average surprisal / entropy rate under mild assumptions.)

### Cross-entropy and KL divergence (high level)
- **Cross-entropy** measures average negative log probability under the model.
- **KL divergence** measures how far the model distribution is from the true distribution.

Perplexity is essentially an exponentiated cross-entropy measure.

---

## Worked mini-example (used in the assignment)

### Corpus

```text
<s> the wizard sees the dragon </s>
<s> the dragon breathes fire </s>
<s> the wizard casts a spell </s>
```

### Vocabulary size
Exclude boundary tokens `<s>` and `</s>`:

$V = 9$ (the, wizard, sees, dragon, breathes, fire, casts, a, spell)

### Why an unsmoothed bigram can yield 0
For the test sentence:

`<s> the dragon casts fire </s>`

the bigram `dragon casts` never appears in the corpus, so with MLE:

$$P(\text{casts}\mid\text{dragon})=0 \Rightarrow P(\text{sentence})=0$$

With add-one smoothing, every bigram gets non-zero probability (see Assignment Q5).

**Related Assignment Questions**: Q4–Q6.

---

## Key Takeaways

| Concept | Definition | Example |
|:---|:---|:---|
| Chain rule | Exact decomposition of $P(w_1\dots w_n)$ | Multiply conditional probabilities |
| Markov assumption | Limit context to last $k$ words | Bigram: $k=1$ |
| N-gram LM | LM using fixed window context | Unigram/bigram/trigram |
| MLE estimate | Relative frequency estimate | $\frac{\text{Count}(ab)}{\text{Count}(a)}$ |
| Sparsity | Most N-grams unseen → many zeros | `dragon casts` absent |
| Add-one smoothing | Avoid zeros by +1 counts | Denominator adds $V$ |
| Good-Turing | Uses $N_c$ (freq of freqs) | Adjusts $c\to c^*$ |
| Absolute discounting | Subtract constant $D$ | Redistribute leftover mass |
| Kneser-Ney | Uses unique contexts (continuation) | Context diversity matters |
| Extrinsic evaluation | Task-based metric | BLEU in MT |
| Perplexity | Intrinsic confusion metric | $P(W)^{-1/n}$ |

---

## Summary
- N-gram LMs approximate the chain rule with a fixed context window (Markov assumption).
- MLE + sparsity leads to zero probabilities; smoothing redistributes mass to fix this.
- Kneser-Ney stands out by using **unique context counts** (continuation probability).
- Extrinsic evaluation measures downstream task performance; intrinsic evaluation uses perplexity.

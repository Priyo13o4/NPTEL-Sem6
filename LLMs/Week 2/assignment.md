# Week 2: Assignment 2 — Introduction to Large Language Models (LLMs) (noc26_cs88)

> **Due Date:** —
> **Submitted:** —
> **Total Score:** —

---

## Section 01 — Markov Assumption and N-grams

### Q1 (1 point)
**A language model that conditions each word on the previous three words is equivalent to which Markov model?**

- [ ] A) First-order
- [ ] B) Second-order
- [ ] C) Third-order
- [ ] D) Fourth-order

**Answer:** C

**Answer Explanation**: A **$k$-th order Markov model** conditions on the previous $k$ words. Conditioning on the previous **three** words means $k=3$, i.e., a **third-order** Markov assumption (often implemented as a **4-gram** model).

**Related Topic**: [Markov assumption](notes.md#markov-assumption)

---

## Section 02 — Smoothing

### Q2 (1 point)
**Which smoothing technique leverages the number of unique contexts a word appears in?**

- [ ] A) Good-Turing
- [ ] B) Add-k
- [ ] C) Kneser-Ney
- [ ] D) Absolute Discounting

**Answer:** C

**Answer Explanation**: **Kneser-Ney smoothing** uses a **continuation probability**: it considers how many **unique preceding contexts** a word appears in (context diversity), not just the raw frequency. Good-Turing uses frequency-of-frequencies, Add-k adds a constant, and Absolute Discounting subtracts a constant.

**Related Topic**: [Kneser-Ney smoothing (unique context / continuation)](notes.md#kneser-ney-smoothing-unique-context--continuation)

---

## Section 03 — Evaluation

### Q3 (1 point)
**Using a language model inside a machine translation system to measure BLEU score is an example of:**

- [ ] A) Intrinsic evaluation
- [ ] B) Extrinsic evaluation
- [ ] C) Perplexity evaluation
- [ ] D) Likelihood estimation

**Answer:** B

**Answer Explanation**: This is **extrinsic evaluation** because the LM is evaluated **as part of a downstream task** (machine translation) using a task metric (BLEU). Intrinsic evaluation measures the LM directly (e.g., perplexity on held-out text).

**Related Topic**: [Extrinsic vs intrinsic evaluation](notes.md#extrinsic-vs-intrinsic-evaluation)

---

## Section 04 — Corpus Questions (Bigrams, Add-one, Perplexity)

For Q4–Q6, consider the following corpus:

```text
<s> the wizard sees the dragon </s>
<s> the dragon breathes fire </s>
<s> the wizard casts a spell </s>
```

### Q4 (2 points)
**Assuming a bigram language model, calculate the probability of the sentence:** `<s> the dragon casts fire </s>`.  
Ignore the unigram probability of $P(\text{<s>})$ in your calculation.

- [ ] A) 2/37
- [ ] B) 1/27
- [ ] C) 0
- [ ] D) 1/36

**Answer:** C

**Answer Explanation**: In an unsmoothed bigram LM (MLE),

$$P(w_i\mid w_{i-1})=\frac{\text{Count}(w_{i-1},w_i)}{\text{Count}(w_{i-1})}$$

The sentence requires the bigram `dragon casts`. In the corpus, `dragon` is followed by `</s>` once and by `breathes` once; it is **never** followed by `casts`, so:

$$\text{Count}(\text{dragon casts})=0 \Rightarrow P(\text{casts}\mid\text{dragon})=0$$

Multiplying bigram probabilities makes the entire sentence probability 0.

**Related Topic**: [Sparsity and the “zero-probability” problem](notes.md#sparsity-and-the-zero-probability-problem)

---

### Q5 (3 points)
**Using add-one smoothing, compute the probability of the sentence:** `<s> the dragon casts fire </s>`.  
Assume $V$ is the number of unique **non-boundary** word types (excluding `<s>`, `</s>`). For this assignment, use the same $+V$ denominator convention for all conditional probabilities, including the `</s>` bigram.

- [ ] A) 1/7150
- [ ] B) 1/35
- [ ] C) 1/67
- [ ] D) 1/9900

**Answer:** A

**Answer Explanation**: With add-one smoothing:

$$P_{+1}(w_i\mid w_{i-1})=\frac{\text{Count}(w_{i-1},w_i)+1}{\text{Count}(w_{i-1})+V}$$

From the corpus, $V=9$ (the, wizard, sees, dragon, breathes, fire, casts, a, spell).

Compute each required bigram:

- $P(\text{the}\mid\text{<s>})=\frac{\text{Count}(\text{<s> the})+1}{\text{Count}(\text{<s>})+V}=\frac{3+1}{3+9}=\frac{1}{3}$
- $P(\text{dragon}\mid\text{the})=\frac{\text{Count}(\text{the dragon})+1}{\text{Count}(\text{the})+V}=\frac{2+1}{4+9}=\frac{3}{13}$
- $P(\text{casts}\mid\text{dragon})=\frac{0+1}{2+9}=\frac{1}{11}$
- $P(\text{fire}\mid\text{casts})=\frac{0+1}{1+9}=\frac{1}{10}$
- $P(\text{</s>}\mid\text{fire})=\frac{\text{Count}(\text{fire </s>})+1}{\text{Count}(\text{fire})+V}=\frac{1+1}{1+9}=\frac{1}{5}$

Multiply:

$$\frac{1}{3}\cdot\frac{3}{13}\cdot\frac{1}{11}\cdot\frac{1}{10}\cdot\frac{1}{5}=\frac{1}{7150}$$

**Related Topic**: [Add-one (Laplace) smoothing](notes.md#add-one-laplace-smoothing)

---

### Q6 (2 points)
**Using the smoothed probability, compute the perplexity of:** `<s> the dragon casts fire </s>`.  
(Exclude `<s>` and `</s>` from word count.)

- [ ] A) $9000^{(1/5)}$
- [ ] B) $7150^{(1/4)}$
- [ ] C) $\left(\frac{1}{35}\right)^{(1/5)}$
- [ ] D) $\left(\frac{1}{9900}\right)^{(1/4)}$

**Answer:** B

**Answer Explanation**: Perplexity is:

$$\text{PP}(W)=P(W)^{-\frac{1}{N}}$$

From Q5, $P(W)=\frac{1}{7150}$. Excluding `<s>` and `</s>`, the word count is $N=4$ (the, dragon, casts, fire). Therefore:

$$\text{PP}=\left(\frac{1}{7150}\right)^{-\frac{1}{4}}=7150^{\frac{1}{4}}$$

**Related Topic**: [Perplexity](notes.md#perplexity)

---

## Answer Key Summary

| Q | Answer | Concept |
|:--:|:--:|:---|
| 1 | C | Markov order / N-grams |
| 2 | C | Kneser-Ney (unique contexts) |
| 3 | B | Extrinsic evaluation (BLEU) |
| 4 | C | Sparsity → zero probability |
| 5 | A | Add-one smoothing probability |
| 6 | B | Perplexity from smoothed probability |

# Numericals Only — Introduction to Large Language Models (LLMs) (noc26_cs88)

## Overview
This file collects only the **numerical / calculation-style** questions from the weekly assignments, along with the **key formula**, **final answer**, and a short **explanation**. Each question links back to the relevant notes section.

---

## Week 2 — N-grams, smoothing, perplexity

### Week 2 — Q4 (2 points): Bigram sentence probability (unsmoothed)
**Formula**

$$P(w_i\mid w_{i-1})=\frac{\text{Count}(w_{i-1},w_i)}{\text{Count}(w_{i-1})}$$

**Question**
Assuming a bigram language model, calculate the probability of the sentence: `<s> the dragon casts fire </s>`.
Ignore the unigram probability of $P(\langle s \rangle)$ in your calculation.

**Answer:** C (0)

**Answer Explanation**: The sentence requires the bigram `dragon casts`. In the corpus, $\text{Count}(\text{dragon casts})=0$, so $P(\text{casts}\mid\text{dragon})=0$, and the full sentence probability becomes 0.

**Related Topic**: [Sparsity and the “zero-probability” problem](Week%202/notes.md#sparsity-and-the-zero-probability-problem)

---

### Week 2 — Q5 (3 points): Add-one smoothing sentence probability
**Formula**

$$P_{+1}(w_i\mid w_{i-1})=\frac{\text{Count}(w_{i-1},w_i)+1}{\text{Count}(w_{i-1})+V}$$

**Question**
Using add-one smoothing, compute the probability of the sentence: `<s> the dragon casts fire </s>`.
Assume $V$ is the number of unique **non-boundary** word types (excluding `<s>`, `</s>`). Use the same $+V$ denominator convention for all conditional probabilities, including the `</s>` bigram.

**Answer:** A ($1/7150$)

**Answer Explanation**: From the corpus, $V=9$. The required smoothed bigrams multiply to:

$$\frac{1}{3}\cdot\frac{3}{13}\cdot\frac{1}{11}\cdot\frac{1}{10}\cdot\frac{1}{5}=\frac{1}{7150}$$

**Related Topic**: [Add-one (Laplace) smoothing](Week%202/notes.md#add-one-laplace-smoothing)

---

### Week 2 — Q6 (2 points): Perplexity from sentence probability
**Formula**

$$\text{PP}(W)=P(W)^{-\frac{1}{N}}$$

**Question**
Using the smoothed probability, compute the perplexity of: `<s> the dragon casts fire </s>`.
(Exclude `<s>` and `</s>` from word count.)

**Answer:** B ($7150^{1/4}$)

**Answer Explanation**: From Q5, $P(W)=\frac{1}{7150}$. Excluding `<s>` and `</s>`, $N=4$, so:

$$\text{PP}=\left(\frac{1}{7150}\right)^{-\frac{1}{4}}=7150^{\frac{1}{4}}$$

**Related Topic**: [Perplexity](Week%202/notes.md#perplexity)

---

## Week 4 — Tokenization + TF-IDF numericals

### Week 4 — Q3 (1 point): BPE merges → vocabulary size
**Formula**

$$|V_{\text{final}}| = |V_{\text{initial}}| + (\#\text{merges})$$

**Question**
A toy corpus has an initial base vocabulary of 6 unique characters: {a, b, c, g, s, t}. If the BPE algorithm performs exactly three merge operations, what will be the final size of the vocabulary?

**Answer:** C (9)

**Answer Explanation**: Each merge introduces one new token type. Starting from 6, after 3 merges: $6+3=9$.

**Related Topic**: [BPE (Byte Pair Encoding)](Week%204/notes.md#bpe-byte-pair-encoding)

---

### Week 4 — Q7 (2 points): TF-IDF idf when term appears in all documents
**Formula**

$$\text{idf}(t)=\log\left(\frac{N}{\text{df}(t)}\right)$$

**Question**
For tf-idf weighting, if a word $t$ appears in every single document in a collection of size $N$, what is its $\text{idf}_t$ value?

**Answer:** D (0)

**Answer Explanation**: If $t$ appears in every document, then $\text{df}(t)=N$, so $\text{idf}(t)=\log\left(\frac{N}{N}\right)=\log(1)=0$.

**Related Topic**: [TF‑IDF](Week%204/notes.md#tf-idf)

---

## Week 5 — Attention softmax numerical

### Week 5 — Q2 (3 points): Dot-product attention + softmax distribution
**Formula**

$$e_i = s^\top h_i, \qquad \alpha_i = \frac{\exp(e_i)}{\sum_j \exp(e_j)}$$

**Question**
Given encoder hidden states $h_1=[-2,0]$, $h_2=[0,0]$, $h_3=[2,0]$ and decoder state $s=[1,0]$, compute the attention distribution $(\alpha_1,\alpha_2,\alpha_3)$.

**Answer:** D ($0.016,\ 0.117,\ 0.867$)

**Answer Explanation**: Scores are $e_1=-2$, $e_2=0$, $e_3=2$. Softmax over $[-2,0,2]$ gives approximately $(0.016, 0.117, 0.867)$.

**Related Topic**: [Dot-product attention scoring](Week%205/notes.md#dot-product-attention-scoring)

---

## Week 6 — Transformer multi-head dimension numerical

### Week 6 — Q9 (1 point): Per-head dimension
**Formula**

$$d_{\text{head}} = \frac{d_{\text{model}}}{h}$$

**Question**
You are given a self-attention layer with input dimension 2048, using 64 heads. What is the output dimension per head?

**Answer:** C (32)

**Answer Explanation**: $d_{\text{head}}=2048/64=32$.

**Related Topic**: [Multi-head attention](Week%206/notes.md#multi-head-attention)

---

## Week 7 — Tensor contraction (einsum) numerical

### Week 7 — Q9 (2 points): einsum scalar contraction
**Formula**

For `einsum('i j, j i ->', A, B)`, the output is:

$$\sum_{i}\sum_{j} A_{ij}B_{ji}$$

**Question**
Let

$$A = \begin{bmatrix} 3 & 0 \\ 1 & -1 \end{bmatrix},\quad B = \begin{bmatrix} -2 & 8 \\ 7 & -7 \end{bmatrix}$$

What is the output of: `numpy.einsum('i j, j i->', A, B)`?

**Answer:** 9

**Answer Explanation**: Multiply and sum:
- $A_{00}B_{00}=3\cdot(-2)=-6$
- $A_{01}B_{10}=0\cdot 7=0$
- $A_{10}B_{01}=1\cdot 8=8$
- $A_{11}B_{11}=(-1)\cdot(-7)=7$

Sum: $-6+0+8+7=9$.

**Related Topic**: [Einsum intuition (why it shows up in attention)](Week%207/notes.md#einsum-intuition-why-it-shows-up-in-attention)

# Week 7: Assignment 7 — Introduction to Large Language Models (LLMs) (noc26_cs88)

> **Due:** 2026-03-11, 23:59 IST
> **Submitted:** —
> **Score:** —

---

## Section 01 - ELMo and BERT

### Q1 (1 point, multiple select)
**ELMo and BERT represent two different pre-training strategies for language models. Which of the following statement(s) about these approaches is/are true? (Select all that apply)**

- [ ] A) ELMo uses a bi-directional LSTM to pre-train word representations, while BERT uses a transformer encoder with masked language modeling.
- [ ] B) ELMo provides context-independent word representations, whereas BERT provides context-dependent representations.
- [ ] C) Pre-training of both ELMo and BERT involve next token prediction.
- [ ] D) Both ELMo and BERT produce contextual embeddings usable for downstream tasks.

**Answer:** A, D

**Answer Explanation**: Both models produce contextual (context-dependent) token representations. ELMo uses a bi-directional LSTM language model, while BERT uses Transformer encoders with masked language modeling (and often NSP). Option B is false because ELMo is also contextual. Option C is false because BERT is not trained with next-token prediction; it is trained to predict masked tokens.

**Related Topic**: [Static vs contextual word representations](notes.md#static-vs-contextual-word-representations)

---

### Q7 (1 point)
**Why does ELMo build its input token representations from a character-level CNN instead of fixed word embeddings?**

- [ ] A) To reduce training time by sharing parameters
- [ ] B) To avoid `UNK` tokens and generate representations for any string
- [ ] C) To compress embeddings to 128 dimensions
- [ ] D) To ensure the same vector for a word in every context

**Answer:** B

**Answer Explanation**: Character-level CNN inputs let ELMo compose word representations from characters, which greatly reduces out-of-vocabulary failures and supports rare/novel word forms.

**Related Topic**: [Character-level CNN input (why it matters)](notes.md#character-level-cnn-input-why-it-matters)

---

### Q8 (1 point)
**In the ELMo task-specific combination formula, what is the role of the scalar $\gamma_{task}$?**

- [ ] A) It enforces a unit norm on the combined vector.
- [ ] B) It selects which of forward/backward LSTMs to use.
- [ ] C) It is an embedding for the task label.
- [ ] D) It allows the downstream task to scale the overall magnitude of the combined representation.

**Answer:** D

**Answer Explanation**: $\gamma_{task}$ is a learned scalar that rescales the combined ELMo embedding so a downstream model can adjust its overall magnitude to match task-specific optimization dynamics.

**Related Topic**: [Task-specific layer combination and gamma task](notes.md#task-specific-layer-combination-and-gamma-task)

---

## Section 02 - Encoder-decoder and Decoder-only Pre-Training

### Q2 (1 point)
**Which model family is natively designed for conditional sequence-to-sequence tasks of the form $P(y\mid x)$ without architectural modification?**

- [ ] A) Decoder-only language models
- [ ] B) Encoder-only models
- [ ] C) Encoder-decoder models
- [ ] D) Character-CNN models

**Answer:** C

**Answer Explanation**: Conditional generation $P(y\mid x)$ naturally maps to an encoder-decoder: the encoder reads $x$, and the decoder generates $y$ while attending to the encoded representation.

**Related Topic**: [Model families: encoder-only vs encoder-decoder vs decoder-only](notes.md#model-families-encoder-only-vs-encoder-decoder-vs-decoder-only)

---

### Q3 (1 point, multiple select)
**Which of the following corruption strategies was/were explored in the pre-training of BART? (Select all that apply)**

- [ ] A) Token masking, i.e., random tokens replaced by mask markers
- [ ] B) Token deletion
- [ ] C) Sentence permutation
- [ ] D) Text infilling, i.e., replacing spans with a single sentinel and tasking decoder to generate the spans

**Answer:** A, B, C, D

**Answer Explanation**: BART is trained as a denoising autoencoder: it corrupts the input using several noise functions (masking, deletion, sentence shuffling, span infilling) and learns to reconstruct the original text.

**Related Topic**: [BART: denoising pre-training for encoder-decoder models](notes.md#bart-denoising-pre-training-for-encoder-decoder-models)

---

### Q4 (1 point, multiple select)
**What were the major changes introduced in GPT-2 family of models, compared to GPT-1? (Select all that apply)**

- [ ] A) Addition of layer normalization between sub-blocks
- [ ] B) Increased vocabulary size
- [ ] C) Increased context size
- [ ] D) Addition of Rotary Positional Embedding (RoPE)

**Answer:** B, C

**Answer Explanation**: GPT-2 largely kept the decoder-only architecture but scaled it up in practical ways (e.g., larger token vocabulary and longer context length). RoPE is a later positional method used in newer model families.

**Related Topic**: [GPT-1 vs GPT-2: decoder-only scaling changes](notes.md#gpt-1-vs-gpt-2-decoder-only-scaling-changes)

---

### Q5 (1 point)
**Which of the following architectures are most suited for the sentiment classification task and the machine translation task, respectively?**

- [ ] A) Sentiment Classification: Encoder-decoder, Machine-Translation: Encoder-only
- [ ] B) Sentiment Classification: Decoder-only, Machine-Translation: Encoder-only
- [ ] C) Sentiment Classification: Encoder-only, Machine-Translation: Decoder-only
- [ ] D) Sentiment Classification: Encoder-only, Machine-Translation: Encoder-decoder

**Answer:** D

**Answer Explanation**: Sentiment classification is a “read and classify” task well-suited to encoder-only models (bidirectional attention). Machine translation is conditional generation and is naturally handled by encoder-decoder seq2seq models.

**Related Topic**: [Model families: encoder-only vs encoder-decoder vs decoder-only](notes.md#model-families-encoder-only-vs-encoder-decoder-vs-decoder-only)

---

### Q6 (1 point)
**The chain-rule factorization used when training an autoregressive language model for a sentence $s = \{t_1, t_2, \dots, t_m\}$ leads to the log probability of $s$ being expressed as:**

- [ ] A) $\log P(s) = \sum_{j=1}^m \log P(t_j)$
- [ ] B) $\log P(s) = \log P(t_1) + \sum_{j=1}^{m-1} \log P(t_{j+1} \mid t_1, \dots, t_j)$
- [ ] C) $\log P(s) = \log P(t_1) \times \prod_{j=1}^{m-1} \log P(t_{j+1} \mid t_1, \dots, t_j)$
- [ ] D) $\log P(s) = \prod_{j=1}^m \log P(t_j)$

**Answer:** B

**Answer Explanation**: The sentence probability factorizes as a product of conditional next-token probabilities. Taking the logarithm converts the product into a sum of log probabilities.

**Related Topic**: [Autoregressive chain rule (log probability)](notes.md#autoregressive-chain-rule-log-probability)

---

## Section 03 - Practical Tooling and Tensor Operations

### Q9 (2 points)
**The `einsum` function in numpy is used as a generalized operation for performing tensor multiplications. Consider two matrices:**

$$A = \begin{bmatrix} 3 & 0 \\ 1 & -1 \end{bmatrix}, \quad B = \begin{bmatrix} -2 & 8 \\ 7 & -7 \end{bmatrix}$$

**What is the output of the following numpy operation?**

`numpy.einsum('i j, j i->', A, B)`

**Answer:** 9

**Answer Explanation**: The pattern `i j, j i ->` multiplies $A_{ij}$ with $B_{ji}$ and sums over all $i,j$.

Compute:
- $A_{00}B_{00} = 3\cdot (-2)=-6$
- $A_{01}B_{10} = 0\cdot 7=0$
- $A_{10}B_{01} = 1\cdot 8=8$
- $A_{11}B_{11} = (-1)\cdot (-7)=7$

Sum: $-6+0+8+7=9$.

**Related Topic**: [Einsum intuition (why it shows up in attention)](notes.md#einsum-intuition-why-it-shows-up-in-attention)

---

## Answer Key Summary

| Q | Answer | Concept |
|:--:|:--:|:---|
| 1 | A, D | ELMo vs BERT contextual pre-training |
| 2 | C | Encoder-decoder fits $P(y\mid x)$ |
| 3 | A, B, C, D | BART denoising corruption strategies |
| 4 | B, C | GPT-2 scaling: vocab + context |
| 5 | D | Encoder-only vs encoder-decoder task fit |
| 6 | B | Autoregressive chain rule (log form) |
| 7 | B | Char-CNN avoids OOV/`UNK` |
| 8 | D | $\gamma_{task}$ scales representation |
| 9 | 9 | Einsum contraction and summation |

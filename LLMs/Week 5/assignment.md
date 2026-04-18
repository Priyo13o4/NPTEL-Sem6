# Week 5: Assignment 5 — Introduction to Large Language Models (LLMs) (noc26_cs88)

> **Due:** 2026-02-25, 23:59 IST
> **Submitted:** —
> **Score:** —

---

## Section 01 - Neural Language Models (RNN, LSTM, GRU)

### Q4 (1 point)
**Which gate in an LSTM is responsible for deciding how much of the cell state to keep:**

- [ ] A) Output gate
- [ ] B) Input gate
- [ ] C) Forget gate
- [ ] D) Cell candidate gate

**Answer:** C

**Answer Explanation**: The **forget gate** outputs values in $[0,1]$ that scale the previous cell state $c_{t-1}$, directly controlling how much old memory is retained in $c_t$.

**Related Topic**: [LSTM architecture and gates](notes.md#lstm-architecture-and-gates)

---

### Q7 (1 point)
**Why are RNNs preferred over fixed-window neural models:**

- [ ] A) They always have a smaller parameter size.
- [ ] B) They can process sequences of arbitrary length.
- [ ] C) None of the above.
- [ ] D) They do not need shared weights across time.

**Answer:** B

**Answer Explanation**: RNNs apply the same recurrence across time steps, so they are not tied to a fixed context window and can process variable-length sequences.

**Related Topic**: [Neural language models: CNN vs RNN](notes.md#neural-language-models-cnn-vs-rnn)

---

## Section 02 - Seq2Seq and Attention

### Q2 (3 points)
**Given the following encoder and decoder hidden states, compute the attention scores (dot-product scoring + softmax):**

- $h_1 = [-2, 0]$
- $h_2 = [0, 0]$
- $h_3 = [2, 0]$
- Decoder state $s = [1, 0]$

Choose the correct attention distribution $(\alpha_1, \alpha_2, \alpha_3)$.

- [ ] A) $0.015,\ 0.843,\ 0.141$
- [ ] B) $0.843,\ 0.015,\ 0.141$
- [ ] C) $0.141,\ 0.843,\ 0.015$
- [ ] D) $0.016,\ 0.117,\ 0.867$

**Answer:** D

**Answer Explanation**: Dot-product scores are
$e_1=s^\top h_1=-2$, $e_2=s^\top h_2=0$, $e_3=s^\top h_3=2$.
Applying softmax to $[-2,0,2]$ gives approximately
$(0.016, 0.117, 0.867)$.
So most attention mass goes to $h_3$.

**Related Topic**: [Dot-product attention scoring](notes.md#dot-product-attention-scoring)

---

### Q3 (1 point)
**What improvement does attention bring to the basic Seq2Seq model:**

- [ ] A) Reduces training time
- [ ] B) Removes the need for an encoder
- [ ] C) Allows access to all encoder states during decoding
- [ ] D) Reduces the number of model parameters

**Answer:** C

**Answer Explanation**: Attention removes the single-vector bottleneck by allowing each decoder step to compute a context vector from all encoder hidden states.

**Related Topic**: [Attention in Seq2Seq models](notes.md#attention-in-seq2seq-models)

---

### Q8 (1 point)
**Which of the following is true about Seq2Seq models:**

(i) Seq2Seq models are always conditioned on the source sentence.  
(ii) The encoder compresses the input sequence into a fixed-size vector representation (in vanilla Seq2Seq).  
(iii) Seq2Seq models cannot handle variable-length sequences.

- [ ] A) (i) and (ii)
- [ ] B) (ii) only
- [ ] C) (iii) only
- [ ] D) (i), (ii), and (iii)

**Answer:** A

**Answer Explanation**: (i) is true because Seq2Seq generation is conditional on the source. (ii) is true for the vanilla encoder-decoder formulation. (iii) is false because Seq2Seq is designed for variable-length input/output sequences.

**Related Topic**: [Sequence-to-sequence encoder-decoder](notes.md#sequence-to-sequence-encoder-decoder)

---

## Section 03 - Decoding and Transformer Attention

### Q1 (1 point)
**State True or False. Increasing the temperature parameter during decoding makes the probability distribution sharper, resulting in more confident and deterministic output:**

- [ ] A) True
- [ ] B) False

**Answer:** B

**Answer Explanation**: Higher temperature ($\tau > 1$) flattens the distribution, increasing randomness and diversity. Sharper, more deterministic behavior comes from lower temperature ($\tau < 1$).

**Related Topic**: [Stochastic decoding: top-k, top-p, and temperature](notes.md#stochastic-decoding-top-k-top-p-and-temperature)

---

### Q5 (1 point)
**What is the benefit of using multiple attention heads in a Transformer model:**

- [ ] A) It reduces the number of computations required.
- [ ] B) It allows attending to multiple parts of the sequence from different representation subspaces.
- [ ] C) It increases model sparsity.
- [ ] D) It replaces the need for positional encodings.

**Answer:** B

**Answer Explanation**: Different heads can focus on different token relations (for example, local syntax vs longer-range dependencies) in parallel subspaces.

**Related Topic**: [Multi-head attention in Transformers](notes.md#multi-head-attention-in-transformers)

---

### Q6 (1 point)
**Why is masking used in the decoder part of a Transformer during training:**

- [ ] A) To prevent gradient explosion
- [ ] B) To ensure the model does not attend to future tokens
- [ ] C) To avoid overfitting
- [ ] D) To normalize attention weights

**Answer:** B

**Answer Explanation**: Causal masking blocks future positions so the decoder predicts token $t$ using only positions $\le t$, preventing information leakage.

**Related Topic**: [Decoder masking (causal self-attention)](notes.md#decoder-masking-causal-self-attention)

---

## Answer Key Summary

| Q | Answer | Concept |
|:--:|:--:|:---|
| 1 | B | Temperature effect on decoding |
| 2 | D | Dot-product attention + softmax |
| 3 | C | Attention removes fixed-vector bottleneck |
| 4 | C | LSTM forget gate |
| 5 | B | Multi-head attention benefit |
| 6 | B | Decoder causal masking |
| 7 | B | RNN variable-length processing |
| 8 | A | Vanilla Seq2Seq properties |

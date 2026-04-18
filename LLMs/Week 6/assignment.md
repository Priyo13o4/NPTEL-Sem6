# Week 6: Assignment 6 — Introduction to Large Language Models (LLMs) (noc26_cs88)

> **Due:** 2026-03-04, 23:59 IST
> **Submitted:** —
> **Score:** —

---

## Section 01 - Transformer Attention and Masking

### Q1 (1 point)
**State True or False: In Transformers, masking is used only during inference to prevent looking at future tokens:**

- [ ] A) True
- [ ] B) False

**Answer:** B

**Answer Explanation**: Decoder masking is critical during training too. When full target sequences are available in parallel, causal masking prevents future-token leakage.

**Related Topic**: [Masked decoding and causal masking](notes.md#masked-decoding-and-causal-masking)

---

### Q2 (1 point)
**In self-attention, which of the following is true:**

- [ ] A) Queries and values are always equal.
- [ ] B) Each token attends only to the previous token.
- [ ] C) The output of each token is a weighted sum over all values.
- [ ] D) The key vectors are fixed after initialization.

**Answer:** C

**Answer Explanation**: Attention weights are computed from query-key similarity, and each token output is the weighted aggregation of value vectors across the sequence.

**Related Topic**: [QKV framework and self-attention](notes.md#qkv-framework-and-self-attention)

---

### Q3 (1 point)
**What is the purpose of masked decoding in Transformers:**

- [ ] A) Prevents attention lookups into the future
- [ ] B) Discards irrelevant tokens from the sequence
- [ ] C) Applies dropout to the attention weights
- [ ] D) Limits the attention span of the model

**Answer:** A

**Answer Explanation**: Causal masking ensures token $t$ only attends to tokens up to $t$, preserving autoregressive generation.

**Related Topic**: [Masked decoding and causal masking](notes.md#masked-decoding-and-causal-masking)

---

## Section 02 - Positional Encoding and Architecture Choices

### Q4 (1 point, multiple select)
**For Rotary Position Embedding (RoPE), which statements are true:**

- [ ] A) Combines relative and absolute positional information.
- [ ] B) Applies a multiplicative rotation matrix to encode positions.
- [ ] C) Eliminates the need for positional encodings.
- [ ] D) All of the above.

**Answer:** A, B

**Answer Explanation**: RoPE is itself a positional encoding method based on multiplicative rotations of query and key vectors, and attention scores capture relative distance information.

**Related Topic**: [Relative positional encoding and RoPE](notes.md#relative-positional-encoding-and-rope)

---

### Q5 (1 point)
**Which of the following statements is NOT true:**

- [ ] A) Self-attention allows tokens to attend to every other token in the sequence.
- [ ] B) Multi-head attention increases the overall dimensionality of the transformer model.
- [ ] C) Relative positional encoding captures the distance between tokens rather than their absolute positions.
- [ ] D) Positional encoding is necessary because self-attention alone is permutation invariant.

**Answer:** B

**Answer Explanation**: Multi-head attention splits model width across heads and then projects back, so final model dimensionality remains the configured $d_{model}$.

**Related Topic**: [Multi-head attention](notes.md#multi-head-attention)

---

### Q6 (1 point)
**What is the primary motivation behind using transformers instead of RNNs or LSTMs:**

- [ ] A) To improve computational efficiency by avoiding sequential processing
- [ ] B) To reduce the need for large datasets
- [ ] C) To improve generalization to unseen data
- [ ] D) To reduce the number of model parameters

**Answer:** A

**Answer Explanation**: The key architectural advantage is parallel token processing. RNN/LSTM recurrence is sequential, while Transformer attention enables efficient large-scale GPU training.

**Related Topic**: [Why Transformers replaced recurrent models](notes.md#why-transformers-replaced-recurrent-models)

---

### Q7 (1 point)
**What is the primary reason for using layer normalization in transformers instead of batch normalization:**

- [ ] A) Layer normalization is faster than batch normalization.
- [ ] B) Batch normalization is not effective for sequence models.
- [ ] C) Layer normalization reduces overfitting.
- [ ] D) Batch normalization does not work with positional encodings.

**Answer:** B

**Answer Explanation**: LayerNorm is more stable for variable-length sequence modeling and avoids dependency on batch-level sequence statistics.

**Related Topic**: [Residual connections and layer normalization](notes.md#residual-connections-and-layer-normalization)

---

### Q8 (1 point)
**The sinusoidal positional encoding uses sine for even dimensions and ____ for odd dimensions:**

- [ ] A) sine
- [ ] B) cosine
- [ ] C) tangent
- [ ] D) None of these

**Answer:** B

**Answer Explanation**: In the original sinusoidal scheme, even index dimensions use sine and odd index dimensions use cosine.

**Related Topic**: [Sinusoidal positional encoding](notes.md#sinusoidal-positional-encoding)

---

## Section 03 - Transformer Math and Implementation

### Q9 (1 point)
**You are given a self-attention layer with input dimension 2048, using 64 heads. What is the output dimension per head:**

- [ ] A) 64
- [ ] B) 128
- [ ] C) 32
- [ ] D) 256

**Answer:** C

**Answer Explanation**: Per-head dimension is $d_{head}=d_{model}/h=2048/64=32$.

**Related Topic**: [Multi-head attention](notes.md#multi-head-attention)

---

### Q10 (1 point)
**Which operation maps the values of a feature to a fixed range (commonly $[0,1]$):**

- [ ] A) Standardization
- [ ] B) Normalization
- [ ] C) Transformation
- [ ] D) Scaling

**Answer:** B

**Answer Explanation**: Normalization (typically min-max scaling) maps values to a predefined range, while standardization targets zero mean and unit variance.

**Related Topic**: [Feature scaling: normalization vs standardization](notes.md#feature-scaling-normalization-vs-standardization)

---

## Answer Key Summary

| Q | Answer | Concept |
|:--:|:--:|:---|
| 1 | B | Causal masking is used in training too |
| 2 | C | Self-attention output as weighted sum of values |
| 3 | A | Masked decoding prevents future leakage |
| 4 | A, B | RoPE properties |
| 5 | B | Multi-head does not increase final model width |
| 6 | A | Parallelism vs recurrent sequential bottleneck |
| 7 | B | LayerNorm suitability for sequence models |
| 8 | B | Sinusoidal PE uses cosine on odd dimensions |
| 9 | C | Per-head size calculation |
| 10 | B | Normalization maps to fixed range |

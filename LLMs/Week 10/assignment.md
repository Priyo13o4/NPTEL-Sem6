# Week 10: Assignment 10 — Introduction to Large Language Models (LLMs) (noc26_cs88)

> **Due:** 2026-04-01, 23:59 IST
> **Submitted:** —
> **Score:** —

---

## Section 01 - PEFT Techniques

### Q1 (1 point)
**How do Prefix Tuning and Adapters differ in terms of where they inject new task-specific parameters in the Transformer architecture?**

- [ ] A) Prefix Tuning adds new feed-forward networks after every attention block, while Adapters prepend tokens.
- [ ] B) Both approaches modify only the final output layer but in different ways.
- [ ] C) Prefix Tuning learns trainable "prefix" hidden states at each layer's input, whereas Adapters insert small bottleneck modules inside the Transformer blocks.
- [ ] D) Both approaches rely entirely on attention masks to inject new task-specific knowledge.

**Answer:** C

**Answer Explanation**: Prefix tuning injects learned prefix states at each layer, influencing attention throughout the network without adding new sublayers to run at inference. Adapters add small bottleneck MLP modules inside blocks, which introduces additional computation in the forward pass.

**Related Topic**: [Prefix tuning](notes.md#prefix-tuning), [Adapters](notes.md#adapters)

---

### Q2 (1 point)
**The Structure-Aware Intrinsic Dimension (SAID) improves over earlier low-rank adaptation approaches by:**

- [ ] A) Ignoring the network structure entirely
- [ ] B) Learning one scalar per layer for layer-wise scaling
- [ ] C) Sharing the same random matrix across all layers
- [ ] D) Using adapters within self-attention layers

**Answer:** B

**Answer Explanation**: SAID explicitly models layer-wise differences by learning a scalar per layer (layer-wise scaling), instead of treating all layers identically.

**Related Topic**: [SAID (Structure-Aware Intrinsic Dimension)](notes.md#said-structure-aware-intrinsic-dimension)

---

### Q3 (1 point, multiple select)
**Which of the following are correct about the extensions of LoRA? (Select all that apply)**

- [ ] A) LongLoRA supports inference on longer sequences using global attention
- [ ] B) QLoRA supports low-rank adaptation on 4-bit quantized models
- [ ] C) DyLoRA automatically selects the optimal rank during training
- [ ] D) LoRA+ introduces gradient clipping to stabilize training

**Answer:** B, C

**Answer Explanation**: QLoRA enables LoRA training on a 4-bit quantized base model. DyLoRA supports dynamic rank behavior so you can effectively select an appropriate rank during/after training. LongLoRA is about long-context adaptation (not simply “global attention”), and LoRA+ is primarily about optimizing LoRA factor updates (not gradient clipping as its defining feature).

**Related Topic**: [LoRA extensions (high-level)](notes.md#lora-extensions-high-level)

---

## Section 02 - Compression for Efficient Inference

### Q4 (1 point)
**Which pruning technique specifically removes weights with the smallest absolute values first, potentially followed by retraining to recover accuracy?**

- [ ] A) Magnitude pruning
- [ ] B) Structured pruning
- [ ] C) Random pruning
- [ ] D) Knowledge distillation

**Answer:** A

**Answer Explanation**: Magnitude pruning removes weights with the smallest absolute values (closest to zero). Because pruning can hurt accuracy, a retraining step is often used to recover performance.

**Related Topic**: [Pruning](notes.md#pruning)

---

### Q5 (1 point)
**In Post-Training Quantization (PTQ) for LLMs, why is a calibration dataset used?**

- [ ] A) To precompute the entire attention matrix for all tokens.
- [ ] B) To remove outlier dimensions before applying magnitude-based pruning.
- [ ] C) To fine-tune the entire model on a small dataset and store the new weights.
- [ ] D) To estimate scale factors for quantizing weights and activations under representative data conditions.

**Answer:** D

**Answer Explanation**: PTQ needs representative activation ranges to choose scale factors/zero points; a calibration dataset provides typical inputs so quantization error stays small.

**Related Topic**: [Post-Training Quantization (PTQ)](notes.md#post-training-quantization-ptq)

---

### Q6 (1 point)
**Which best summarizes the function of the unembedding matrix $W_U$?**

- [ ] A) It merges the queries and keys for each token before final classification.
- [ ] B) It converts the final residual vector into vocabulary logits for next-token prediction.
- [ ] C) It is used for normalizing the QK and OV circuits so that their norms match.
- [ ] D) It acts as a second attention layer that aggregates multiple heads.

**Answer:** B

**Answer Explanation**: In the residual stream view, the model ends with a final residual representation; the unembedding matrix projects this vector into vocabulary logits used for next-token prediction.

**Related Topic**: [Unembedding matrix](notes.md#unembedding-matrix)

---

## Section 03 - Interpretability and QLoRA Details

### Q7 (1 point)
**Which definition best matches an induction head as discovered in certain Transformer circuits?**

- [ ] A) A head that specifically attends to punctuation tokens to determine sentence boundaries
- [ ] B) A feed-forward sub-layer specialized for outputting next-token probabilities for out-of-distribution tokens
- [ ] C) A head that looks for previous occurrences of a token A, retrieves the token B that followed it last time, and then predicts B again
- [ ] D) A masking head that prevents the model from looking ahead at future tokens

**Answer:** C

**Answer Explanation**: Induction heads implement a learned copy-and-continue pattern: find an earlier instance of A, retrieve the following token B, and increase the probability of predicting B when A reappears.

**Related Topic**: [Induction heads](notes.md#induction-heads)

---

### Q8 (1 point)
**In mechanistic interpretability, how can we define “circuit”?**

- [ ] A) A data pipeline for collecting training examples in an autoregressive model
- [ ] B) A small LSTM module inserted into a Transformer for additional memory
- [ ] C) A device external to the neural network used to fine-tune certain parameters after training
- [ ] D) A subgraph of the neural network hypothesized to implement a specific function or behaviour

**Answer:** D

**Answer Explanation**: A circuit is a hypothesized subgraph (specific heads/neurons/paths) that implements a particular behavior or function.

**Related Topic**: [Circuits](notes.md#circuits)

---

### Q9 (1 point)
**Which best describes the role of Double Quantization in QLoRA?**

- [ ] A) It quantizes the attention weights twice to achieve 1-bit representations.
- [ ] B) It reinitializes parts of the model with random bit patterns for improved regularization.
- [ ] C) It quantizes the quantization constants themselves for additional memory savings.
- [ ] D) It systematically reverts partial quantized weights back to FP16 whenever performance degrades.

**Answer:** C

**Answer Explanation**: QLoRA reduces memory by storing the base model in 4-bit; double quantization further compresses by quantizing the quantization constants (e.g., scales) themselves.

**Related Topic**: [Double quantization (in QLoRA)](notes.md#double-quantization-in-qlora)

---

### Q10 (1 point, multiple select)
**Which of the following are true about sequence-level distillation for LLMs? (Select all that apply)**

- [ ] A) It trains a student model by matching the teacher's sequence outputs (e.g., predicted token sequences) rather than just individual token distributions.
- [ ] B) It requires storing only the top-1 predictions from the teacher model for each token.
- [ ] C) It can be combined with word-level distillation to transfer both local and global knowledge.
- [ ] D) It forces the teacher to produce a chain-of-thought explanation for each example.

**Answer:** A, C

**Answer Explanation**: Sequence-level distillation focuses on matching full outputs/sequences; word-level distillation matches token-level distributions. Combining both can transfer local probabilities and global generation behavior. It does not require chain-of-thought, and it is not defined by keeping only top-1 tokens.

**Related Topic**: [Knowledge distillation](notes.md#knowledge-distillation)

---

## Answer Key Summary

| Q | Answer | Concept |
|:--:|:--:|:---|
| 1 | C | Prefix tuning vs adapters injection point |
| 2 | B | SAID layer-wise scaling |
| 3 | B, C | QLoRA and DyLoRA extensions |
| 4 | A | Magnitude pruning |
| 5 | D | PTQ calibration scales |
| 6 | B | Unembedding to logits |
| 7 | C | Induction heads for ICL |
| 8 | D | Circuits as subgraphs |
| 9 | C | Double quantization |
| 10 | A, C | Sequence-level distillation |

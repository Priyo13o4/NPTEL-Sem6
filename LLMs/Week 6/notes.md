# Week 6: Theoretical Notes — Transformer Architecture, Positional Encoding, and Implementation

## Overview
Week 6 introduces the Transformer, the architecture behind modern LLMs, and explains why it replaced recurrent models for large-scale training. We cover self-attention, multi-head attention, masking, positional encoding, and normalization inside encoder-decoder blocks. The key theme is parallel sequence processing with explicit token-to-token interactions, while preserving order information through positional methods.

---

## Why Transformers replaced recurrent models
RNNs and LSTMs process tokens sequentially, so token $t$ depends on computation from token $t-1$. This creates a training bottleneck on GPUs.

Transformers remove recurrence and compute token interactions with attention in parallel. This was a primary motivation for the architecture: better computational efficiency at scale.

**Related Assignment Questions**: Q6.

---

## QKV framework and self-attention
Each input embedding $x$ is linearly projected into:
- **Query**: what this token is looking for
- **Key**: what this token offers for matching
- **Value**: information this token contributes

Using learned matrices:

$$Q = XW^Q, \quad K = XW^K, \quad V = XW^V$$

For each token, the output is a weighted sum over all value vectors, where weights come from query-key similarity.

**Related Assignment Questions**: Q2.

### Scaled dot-product attention
The core operation is:

$$\text{Attention}(Q,K,V)=\text{softmax}\left(\frac{QK^\top}{\sqrt{d_k}}\right)V$$

- $QK^\top$ gives pairwise attention logits.
- Division by $\sqrt{d_k}$ stabilizes softmax gradients.
- Multiplication by $V$ produces context-aware token outputs.

---

## Multi-head attention
Instead of one attention map, Transformer uses multiple heads:

$$\text{head}_i = \text{Attention}(QW_i^Q, KW_i^K, VW_i^V)$$
$$\text{MHA}(Q,K,V)=\text{Concat}(\text{head}_1,\dots,\text{head}_h)W^O$$

If model width is $d_{model}$ and number of heads is $h$, each head typically has:

$$d_{head}=\frac{d_{model}}{h}$$

So multi-head attention does not increase the final model dimensionality; concatenated heads are projected back to $d_{model}$.

Example: if $d_{model}=2048$ and $h=64$, then $d_{head}=32$.

**Related Assignment Questions**: Q5, Q9.

---

## Masked decoding and causal masking
In decoder self-attention, future positions must be hidden so prediction at step $t$ cannot use tokens $>t$.

A causal mask $M$ sets forbidden logits to $-\infty$:

$$\text{softmax}\left(\frac{QK^\top}{\sqrt{d_k}} + M\right)$$

This masking is used during training and inference logic, not only inference. During training, it prevents future-token leakage when the full target sequence is available.

**Related Assignment Questions**: Q1, Q3.

---

## Positional information problem
Self-attention alone is permutation-invariant, so token order is not encoded unless position information is injected.

This is why positional encoding (or positional bias) is necessary in Transformers.

**Related Assignment Questions**: Q5.

### Sinusoidal positional encoding
Original Transformer uses fixed sinusoidal encoding:

$$PE(pos,2i)=\sin\left(\frac{pos}{10000^{2i/d_{model}}}\right)$$
$$PE(pos,2i+1)=\cos\left(\frac{pos}{10000^{2i/d_{model}}}\right)$$

- Even dimensions use sine.
- Odd dimensions use cosine.
- Different frequencies let model infer relative offsets.

**Related Assignment Questions**: Q8.

### Relative positional encoding and RoPE
Later models use relative position ideas so attention depends on token distance, not only absolute index.

**RoPE (Rotary Position Embedding)** rotates query/key vectors by position-dependent matrices:

$$q_m' = R_m q_m, \quad k_n' = R_n k_n$$

Then dot product depends on relative displacement $(m-n)$ through rotation structure. RoPE is multiplicative (rotation-based), and it carries both absolute position (through index-dependent rotation) and relative distance information in attention scores.

**Related Assignment Questions**: Q4.

---

## Residual connections and layer normalization
Each sublayer is wrapped as Add and Norm:

$$y = \text{LayerNorm}(x + f(x))$$

Residual paths improve gradient flow in deep stacks.

Transformers prefer **LayerNorm** over BatchNorm for sequence modeling because:
- LayerNorm normalizes per token across features.
- It is stable with variable-length sequences and padding.
- Behavior is consistent between training and inference.

**Related Assignment Questions**: Q7.

---

## Transformer block implementation in PyTorch
A standard implementation includes:
1. Multi-head self-attention (often with batched matrix multiplication or einsum).
2. Scaling by $\sqrt{d_k}$ and applying padding/causal masks.
3. Position-wise feedforward network:

$$\text{FFN}(x)=W_2\,\text{ReLU}(W_1x+b_1)+b_2$$

Typically, hidden width expands to about $4\times d_{model}$ then projects back.

4. Encoder block: Self-Attn -> Add and Norm -> FFN -> Add and Norm.
5. Decoder block: Masked Self-Attn -> Add and Norm -> Cross-Attn -> Add and Norm -> FFN -> Add and Norm.

These blocks are stacked $N$ times before final projection to vocabulary logits.

---

## Feature scaling: normalization vs standardization
For preprocessing terminology:
- **Normalization** (min-max scaling): maps values to a fixed range, commonly $[0,1]$.
- **Standardization**: transforms features to zero mean and unit variance.

So the operation that maps to a specified range is normalization.

**Related Assignment Questions**: Q10.

---

## Key Takeaways

| Concept | Definition | Example |
|:---|:---|:---|
| Transformer motivation | Remove sequential bottleneck of RNN/LSTM | Parallel token processing on GPU |
| QKV self-attention | Query-key scores weight values | Output token = weighted sum of all values |
| Scaled dot-product | Divide logits by $\sqrt{d_k}$ before softmax | Stabilizes training |
| Multi-head attention | Multiple attention subspaces in parallel | $2048/64=32$ dims per head |
| Causal masking | Block future-token access in decoder | Upper-triangular mask with $-\infty$ |
| Positional encoding | Inject order information into attention model | Sin for even, cos for odd dimensions |
| RoPE | Multiplicative rotational encoding for Q/K | Relative distance preserved in dot products |
| Add and Norm | Residual plus LayerNorm around sublayers | Better gradient flow in deep stacks |
| LayerNorm over BatchNorm | Better for variable-length sequence modeling | Stable train/inference behavior |
| Normalization | Min-max range mapping | Scale feature to $[0,1]$ |

---

## Summary
- Transformers replace recurrence with attention, enabling parallel sequence computation.
- Self-attention updates each token by weighted aggregation over all token values.
- Multi-head attention improves representation diversity without changing final model width.
- Decoder masking prevents future-token leakage and preserves autoregressive training.
- Positional encoding is essential because attention alone does not encode order.
- RoPE and relative methods improve long-context behavior by encoding distance-sensitive structure.
- LayerNorm and residual paths are core stability components of deep Transformer stacks.
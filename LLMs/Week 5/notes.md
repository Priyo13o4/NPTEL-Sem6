# Week 5: Theoretical Notes — Neural Language Models, Seq2Seq, and Attention

## Overview
Week 5 moves from fixed-context language models to sequence models that process ordered tokens over time. We cover RNNs, LSTMs, GRUs, Seq2Seq encoder-decoder systems, deterministic and stochastic decoding, and attention-based alignment. The unifying idea is conditional generation over sequences, with better memory and better token selection at inference.

---

## Neural language models: CNN vs RNN
A **fixed-window neural language model** only sees a local context (for example, the previous $k$ tokens). This can miss long-range dependencies.

A **CNN language model** applies kernels to local windows:

$$h_t = \text{Conv}(x_{t-k:t+k})$$

Even with stacking, context is still tied to receptive field size.

An **RNN language model** carries state over time:

$$h_t = \tanh(W_x x_t + W_h h_{t-1} + b_h)$$
$$P(w_t \mid w_{<t}) = \text{softmax}(W_o h_t + b_o)$$

Because parameters are shared across time steps, RNNs naturally handle variable-length sequences.

**Related Assignment Questions**: Q7.

---

## Training RNNs: BPTT and teacher forcing
**Backpropagation Through Time (BPTT)** unrolls the RNN over time and backpropagates through all unrolled steps.

During sequence generation training, **teacher forcing** feeds the gold previous token $y_{t-1}^{\text{gold}}$ to predict $y_t$ instead of feeding the model's own previous prediction.

Benefits:
- faster and more stable early training
- reduced error compounding during training

Limitations:
- train-test mismatch can appear at inference (exposure bias)

---

## Vanishing and exploding gradients
In long unrolled chains, repeated Jacobian products can shrink gradients toward zero (vanishing) or blow them up (exploding):

$$\frac{\partial \mathcal{L}}{\partial h_t} = \left(\prod_{j=t+1}^{T} \frac{\partial h_j}{\partial h_{j-1}}\right) \frac{\partial \mathcal{L}}{\partial h_T}$$

Common mitigations:
- LSTM/GRU gating
- gradient clipping (for exploding gradients)
- residual/highway style skip paths

---

## LSTM architecture and gates
An **LSTM** adds a dedicated cell state $c_t$ and gates to control information flow:

$$f_t = \sigma(W_f[h_{t-1};x_t] + b_f)$$
$$i_t = \sigma(W_i[h_{t-1};x_t] + b_i)$$
$$\tilde{c}_t = \tanh(W_c[h_{t-1};x_t] + b_c)$$
$$c_t = f_t \odot c_{t-1} + i_t \odot \tilde{c}_t$$
$$o_t = \sigma(W_o[h_{t-1};x_t] + b_o)$$
$$h_t = o_t \odot \tanh(c_t)$$

Interpretation:
- **forget gate** $f_t$: how much previous cell memory to keep
- **input gate** $i_t$: how much new candidate memory to write
- **output gate** $o_t$: how much memory to expose as hidden state

**Related Assignment Questions**: Q4.

---

## GRU (Gated Recurrent Unit)
A **GRU** simplifies LSTM by merging memory control with fewer gates and no separate cell state:

$$z_t = \sigma(W_z[h_{t-1};x_t] + b_z)$$
$$r_t = \sigma(W_r[h_{t-1};x_t] + b_r)$$
$$\tilde{h}_t = \tanh(W_h[r_t \odot h_{t-1};x_t] + b_h)$$
$$h_t = (1-z_t) \odot h_{t-1} + z_t \odot \tilde{h}_t$$

GRUs often train faster and use fewer parameters while remaining competitive.

---

## Deep recurrent variants
### Bidirectional and multi-layer RNNs
- **Bidirectional RNNs** combine past-to-future and future-to-past context.
- **Multi-layer RNNs** stack recurrent layers for richer representations.

### Residual, dense, and highway connections
These skip-style designs help gradient flow in deep networks by creating shorter paths for information and gradients.

---

## Sequence-to-sequence encoder-decoder
A **Seq2Seq** model learns conditional generation:

$$P(y \mid x) = \prod_{t=1}^{T_y} P(y_t \mid y_{<t}, x)$$

Vanilla structure:
- **encoder** reads source sequence and builds a context representation
- **decoder** generates target tokens autoregressively

In classic Seq2Seq, the decoder is conditioned on a fixed-size context vector from the encoder's final state.

**Related Assignment Questions**: Q8.

---

## Decoding in Seq2Seq: greedy, exhaustive, and beam search
- **Greedy decoding**: pick $\arg\max$ token at each step (fast, locally optimal).
- **Exhaustive search**: evaluate all sequences (optimal but intractable).
- **Beam search**: keep top $K$ partial hypotheses at each step.

Typical beam score with length normalization:

$$\text{score}(y) = \frac{1}{T^\alpha}\sum_{t=1}^{T} \log P(y_t \mid y_{<t}, x)$$

where $\alpha$ controls length penalty strength.

---

## Stochastic decoding: top-k, top-p, and temperature
For more diverse generation:
- **random sampling**: sample from full distribution
- **top-k**: keep top $K$ tokens, renormalize, sample
- **top-p (nucleus)**: keep smallest token set whose cumulative mass is at least $p$, renormalize, sample

**Temperature** rescales logits before softmax:

$$p_i(\tau)=\frac{\exp(z_i/\tau)}{\sum_j \exp(z_j/\tau)}$$

- low $\tau < 1$: sharper, more deterministic
- high $\tau > 1$: flatter, more diverse

**Related Assignment Questions**: Q1.

---

## Attention in Seq2Seq models
Vanilla Seq2Seq has a bottleneck: one fixed context vector for all decoding steps.

With **attention**, the decoder attends to all encoder states at each step.

### Dot-product attention scoring
At decoder step $t$, with decoder state $s_t$ and encoder states $h_i$:

$$e_{t,i} = s_t^\top h_i$$
$$a_{t,i} = \text{softmax}(e_{t,i})$$
$$c_t = \sum_i a_{t,i} h_i$$

Then combine context with decoder state for prediction.

**Related Assignment Questions**: Q2, Q3.

### Attention variants
Common scoring variants:
- **dot-product**: $s_t^\top h_i$
- **bilinear**: $s_t^\top W h_i$
- **additive (Bahdanau)**: $v^\top \tanh(W_s s_t + W_h h_i)$
- **scaled dot-product**: $\frac{qk^\top}{\sqrt{d_k}}$

---

## Transformer bridge: multi-head attention and masking
### Multi-head attention in Transformers
A Transformer uses several attention heads in parallel:

$$\text{head}_m = \text{softmax}\left(\frac{QW_m^Q(KW_m^K)^\top}{\sqrt{d_k}}\right)VW_m^V$$

Multiple heads let the model capture different relation types in different representation subspaces.

**Related Assignment Questions**: Q5.

### Decoder masking (causal self-attention)
During decoder training, a **causal mask** blocks attention to future positions by setting future logits to $-\infty$ before softmax. This enforces autoregressive behavior (no future-token leakage).

**Related Assignment Questions**: Q6.

---

## Key Takeaways

| Concept | Definition | Example |
|:---|:---|:---|
| RNN LM | Recurrent model with shared parameters across time | handles variable-length sentences |
| BPTT | Backpropagation on unrolled time steps | train recurrent parameters end-to-end |
| Teacher forcing | feed gold previous token during training | stabilizes early decoder learning |
| LSTM forget gate | controls retained cell memory | keeps/discards long-term context |
| GRU | gated recurrent model with fewer parameters | update/reset gates, no explicit cell state |
| Seq2Seq | conditional sequence generation | machine translation |
| Beam search | keep top $K$ partial hypotheses | better than greedy in many settings |
| Temperature | logit scaling before softmax | high $\tau$ increases diversity |
| Attention | dynamic weighting over encoder states | avoids fixed-vector bottleneck |
| Multi-head attention | parallel attention in subspaces | syntax and semantics can be tracked separately |
| Decoder masking | prevent access to future tokens | causal language modeling |

---

## Summary
- RNNs remove fixed-window limits but face gradient instability over long time horizons.
- LSTM and GRU gating mechanisms improve memory flow and training robustness.
- Seq2Seq reframes tasks like translation as conditional language modeling.
- Decoding quality and diversity depend heavily on strategy: greedy/beam vs top-k/top-p/temperature.
- Attention solves the Seq2Seq bottleneck by letting the decoder consult all encoder states dynamically.
- Multi-head attention and causal masking are key bridge concepts toward full Transformer models.

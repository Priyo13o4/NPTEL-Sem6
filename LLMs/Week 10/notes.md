# Week 10: Theoretical Notes — PEFT, Efficient Inference, Residual Stream View, and Mechanistic Interpretability

## Overview
This week focuses on two industry-critical themes: **efficiency** (making LLM adaptation and inference cheaper) and **interpretability** (understanding what parts of a Transformer implement particular behaviors). We cover **Parameter-Efficient Fine-Tuning (PEFT)** methods like prompt/prefix tuning, adapters, and **LoRA** (plus extensions like QLoRA and DyLoRA). Then we study compression techniques for efficient inference: **quantization**, **pruning**, and **knowledge distillation**. Finally, we learn an alternate Transformer view via the **residual stream perspective** (QK/OV circuits and unembedding) and core mechanistic interpretability tools including **circuits**, **induction heads**, probing, activation patching, and sparse autoencoders.

---

## Why PEFT?
Full fine-tuning updates all parameters, which is costly and can cause:
- high GPU memory usage
- large storage for separate fine-tuned copies
- overfitting on small datasets
- catastrophic forgetting of pretraining knowledge

**PEFT** freezes most of the pretrained model and trains only a small set of new parameters.

**Related Assignment Questions**: Q1.

---

## Soft / prompt tuning
**Soft (prompt) tuning** learns a small number of **continuous trainable vectors** (virtual tokens) that are prepended at the input layer.

Why it’s useful:
- you can serve many tasks from one base model by swapping prompt vectors
- the base model weights remain shared and frozen

**Related Assignment Questions**: Q1.

---

## Prefix tuning
**Prefix tuning** extends prompt tuning by injecting trainable **prefix hidden states** at *every Transformer layer* (not just at the input).

A common implementation detail:
- use an MLP reparameterization of the prefix parameters during training to stabilize optimization

**Related Assignment Questions**: Q1.

---

## Adapters
**Adapters** add small **bottleneck modules** inside each Transformer block (typically with residual connections).

Key trade-off:
- adapters add extra computation in the forward pass, so they can increase inference latency compared to methods that don’t add new layers

**Related Assignment Questions**: Q1.

---

## LoRA (Low-Rank Adaptation)
**LoRA** keeps the original weight matrix $W$ frozen and learns a low-rank update:

$$\Delta W = BA$$

- $B$ is a down-projection and $A$ is an up-projection (low rank $r$)
- LoRA is motivated by the idea that the task-specific update lies in a low intrinsic dimension
- commonly applied to attention projection matrices (e.g., $W_Q, W_K, W_V, W_O$)

A practical stability detail from the lecture:
- initialize one factor (often $B$) to zero so the initial update is near-zero

**Related Assignment Questions**: Q2, Q3.

### SAID (Structure-Aware Intrinsic Dimension)
**SAID** refines low-rank adaptation by incorporating network structure, e.g., learning **layer-wise scaling** so different layers can adapt by different amounts.

**Related Assignment Questions**: Q2.

### LoRA extensions (high-level)
- **QLoRA**: trains LoRA on a **4-bit quantized** base model
- **LongLoRA**: improves long-context adaptation (e.g., via attention design for long sequences)
- **LoRA+**: modifies optimization for LoRA factors (e.g., different learning rates)
- **DyLoRA**: supports **dynamic rank** behavior / selecting effective rank during training

**Related Assignment Questions**: Q3, Q9.

---

## Efficient inference: quantization, pruning, distillation
These techniques aim to reduce memory, latency, or compute at inference time.

---

## Quantization
**Quantization** maps weights/activations to lower-bit representations (e.g., 8-bit, 4-bit).

### Post-Training Quantization (PTQ)
**PTQ** quantizes a trained model without retraining.

Why a **calibration dataset** is used:
- to estimate representative activation ranges / scale factors, so quantization error is minimized under typical inputs

**Related Assignment Questions**: Q5.

### Quantization-aware training (QAT)
**QAT** simulates quantization during training so the model learns to be robust to lower precision.

### Double quantization (in QLoRA)
In QLoRA, you may additionally **quantize the quantization constants** (scales/zeros), saving extra memory.

**Related Assignment Questions**: Q9.

---

## Pruning
**Pruning** removes weights or structures from a model.

- **magnitude pruning**: remove weights with the smallest absolute values, sometimes followed by retraining
- **structured pruning**: remove entire components (e.g., heads, neurons, layers)
- **unstructured pruning**: remove individual weights (irregular sparsity)

**Related Assignment Questions**: Q4.

---

## Knowledge distillation
**Knowledge distillation** trains a smaller **student** to mimic a larger **teacher**.

- **word-level distillation**: match per-token probability distributions
- **sequence-level distillation**: match full sequence outputs (e.g., generated token sequences)

These two can be combined to transfer local + global behavior.

**Related Assignment Questions**: Q10.

---

## Residual stream perspective (alternate formulation)
Instead of viewing the Transformer as a strictly sequential pipeline, the **residual stream** view treats it as a shared state vector updated by components.

- attention heads and MLPs **read** from the residual stream and **write** back to it
- this makes it easier to reason about “which component wrote what information”

### QK and OV circuits
Attention can be decomposed conceptually into:
- **QK circuit**: determines *where* to attend (routing)
- **OV circuit**: determines *what content* gets moved (payload)

### Unembedding matrix
The **unembedding matrix** maps the final residual representation to vocabulary logits for next-token prediction.

**Related Assignment Questions**: Q6.

---

## Mechanistic interpretability
**Mechanistic interpretability** tries to reverse-engineer learned algorithms inside LLMs.

### Circuits
A **circuit** is a hypothesized **subgraph of the network** (heads/neurons/paths) implementing a specific function or behavior.

**Related Assignment Questions**: Q8.

### Induction heads
An **induction head** supports in-context learning by:
- finding an earlier occurrence of token $A$
- retrieving the token $B$ that followed it
- boosting the prediction of $B$ when $A$ appears again

**Related Assignment Questions**: Q7.

### Common techniques
- **probing classifiers**: train linear models to predict human-interpretable attributes from activations
- **activation patching / causal tracing**: intervene on activations to test causal responsibility for a behavior
- **sparse autoencoders (SAEs)**: learn sparse features that can make residual representations more interpretable

---

## Key Takeaways

| Concept | Definition | Example |
|:---|:---|:---|
| PEFT | Freeze most weights; train a small subset | LoRA on attention matrices |
| Soft/prompt tuning | Trainable vectors only at input | swap prompts for multitask serving |
| Prefix tuning | Trainable prefix states at every layer | per-layer prefixes via MLP |
| Adapters | Bottleneck modules inside blocks | extra modules can add latency |
| LoRA | Low-rank weight update $\Delta W=BA$ | apply to $W_Q, W_V$ |
| PTQ | Quantize without retraining | calibration dataset for scales |
| Magnitude pruning | Remove smallest-|w| weights | prune near-zero weights |
| Distillation | Student mimics teacher | word-level + sequence-level |
| Residual stream | Shared state updated by components | read/write view of Transformer |
| Circuit | Subgraph implementing behavior | name-mover circuit |
| Induction head | Copy-paste pattern for ICL | retrieve B after A |

---

## Summary
- PEFT adapts LLMs cheaply via prompt/prefix tuning, adapters, and LoRA-style low-rank updates.
- Quantization, pruning, and distillation reduce inference cost and can be combined (e.g., QLoRA).
- The residual stream perspective enables circuit-level reasoning using QK/OV decomposition and unembedding.
- Mechanistic interpretability uses circuits, induction heads, probing, activation patching, and SAEs to study model internals.

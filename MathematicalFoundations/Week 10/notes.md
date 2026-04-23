# Week 10: Theoretical Notes — Transformers, Attention, and Advanced Optimizers

## Overview

Week 10 marks the culmination of the course: **Transformers**, the architecture powering modern Large Language Models (LLMs) and state-of-the-art deep learning. We move from sequential processing (RNNs from Week 9) to parallel processing via the **attention mechanism**, which allows the model to simultaneously consider all tokens in a sequence. This week also covers practical techniques—**transfer learning** and **knowledge distillation**—for leveraging pretrained models, and **advanced optimizers** (SGD, Momentum, RMSProp, Adam) that accelerate and stabilize training.

---

## 1. The Attention Mechanism: Query, Key, Value

### Motivation: The Problem with RNNs

**Sequential Processing Bottleneck:**

RNNs process sequences one token at a time, maintaining a hidden state that evolves:
$$h_t = \sigma(W_{hh} h_{t-1} + W_{ih} x_t + b)$$

**Two Critical Issues:**
1. **Slow:** Cannot parallelize across time steps. A sequence of length $T$ requires $T$ sequential steps.
2. **Long-Range Dependencies:** To connect information at time $t$ to time $T$ (far apart), gradients must travel through $T - t$ time steps, leading to vanishing/exploding gradients.

**Solution: Attend to the Entire Sequence Simultaneously**

The attention mechanism allows every position to directly "look at" every other position in one step, enabling parallel computation and direct gradient paths.

### The Three Matrices: Query, Key, Value

**Definition:** The attention mechanism transforms input tokens via three learned linear projections:

$$Q = XW_Q \quad (\text{Query})$$
$$K = XW_K \quad (\text{Key})$$
$$V = XW_V \quad (\text{Value})$$

where:
- $X \in \mathbb{R}^{T \times d}$: Input token embeddings (sequence length $T$, embedding dimension $d$).
- $W_Q, W_K, W_V \in \mathbb{R}^{d \times d_k}$ (or $d_v$): Learnable projection matrices.

**Intuition (Information Retrieval Analogy):**

- **Query:** "What information am I looking for?" (What does token $i$ need to know?)
- **Key:** "What information do I have?" (What can token $j$ tell you?)
- **Value:** "The actual information." (The content associated with token $j$.)

**Example (Q1 from Assignment):**

Input tokens: $X \in \mathbb{R}^{5 \times 8}$ (5 tokens, 8-dimensional embeddings).
Query projection: $W_Q \in \mathbb{R}^{8 \times 4}$.
Result: $Q = XW_Q \in \mathbb{R}^{5 \times 4}$ (5 tokens, 4-dimensional queries).

**Related Assignment Questions:** Q1, Q2, Q3 test understanding of these dimensions.

---

## 2. Scaled Dot-Product Attention

### The Scoring Mechanism

**Step 1: Compute Similarity Scores**

The similarity between query $i$ and key $j$ is the dot product:
$$S_{ij} = q_i \cdot k_j = \sum_{\ell=1}^{d_k} q_{i\ell} k_{j\ell}$$

Collectively, the score matrix is:
$$S = QK^T \in \mathbb{R}^{T \times T}$$

Each row $i$ contains how much token $i$ "aligns" with all tokens in the sequence.

**Example (Q3 from Assignment):**

Query: $q = \begin{bmatrix} 1 \\ 2 \end{bmatrix}$, Key 1: $k_1 = \begin{bmatrix} 2 \\ 1 \end{bmatrix}$, Key 2: $k_2 = \begin{bmatrix} 1 \\ 3 \end{bmatrix}$.

$$q^T k_1 = (1)(2) + (2)(1) = 4$$
$$q^T k_2 = (1)(1) + (2)(3) = 7$$

**Step 2: Scale by Dimension**

Without scaling, the dot products can become very large, causing softmax to produce near-zero gradients (saturation). Scaling by $\sqrt{d_k}$ stabilizes gradients:

$$\tilde{S} = \frac{S}{\sqrt{d_k}} = \frac{QK^T}{\sqrt{d_k}}$$

**Step 3: Apply Softmax**

Convert scores to a probability distribution (weights) over all keys:

$$A = \text{softmax}(\tilde{S})_{ij} = \frac{\exp(\tilde{S}_{ij})}{\sum_{\ell=1}^T \exp(\tilde{S}_{i\ell})}$$

Each row of $A$ is a probability distribution: $\sum_{j=1}^T A_{ij} = 1$ for all $i$.

**Step 4: Weighted Sum of Values**

Multiply attention weights by values to get the final output:

$$\text{Output} = AV$$

### The Complete Attention Formula

$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$$

**Intuition:**
- High-scoring key-query pairs (high softmax weight) → their corresponding values have large influence.
- Low-scoring pairs (low softmax weight) → their values contribute little.

The model learns which tokens should "attend" to which other tokens.

**Related Assignment Questions:** Q2 (score matrix dimension), Q3 (dot products).

---

## 3. Multi-Head Attention

### The Limitation of Single Attention

A single attention head produces one output per token. But different types of relationships might be important:

- Head 1: Attends to nearby tokens (local structure).
- Head 2: Attends to semantically similar tokens (global meaning).
- Head 3: Attends to previous occurrences of the same entity (coreference).

**Solution: Run Multiple Attention Heads in Parallel**

**Definition:** Multi-Head Attention runs $h$ attention heads independently, each with its own query, key, and value projections:

$$Q^{(i)} = XW_Q^{(i)}, \quad K^{(i)} = XW_K^{(i)}, \quad V^{(i)} = XW_V^{(i)} \quad \text{for } i = 1, \ldots, h$$

Each head projects to dimension $d_k = d_{model} / h$ to keep total computation constant.

**Compute Attention for Each Head:**

$$Z^{(i)} = \text{Attention}(Q^{(i)}, K^{(i)}, V^{(i)}) \in \mathbb{R}^{T \times d_k}$$

**Concatenate Outputs:**

$$Z = [Z^{(1)}; Z^{(2)}; \ldots; Z^{(h)}] \in \mathbb{R}^{T \times d_{model}}$$

**Final Linear Projection:**

$$\text{MultiHead}(Q, K, V) = Z W^O$$

where $W^O \in \mathbb{R}^{d_{model} \times d_{model}}$ is learned.

**Example (Q4 from Assignment):**

Model dimension: $d_{model} = 12$. Number of heads: $h = 3$.

Dimension per head: $d_k = 12 / 3 = 4$.

Each head operates on 4-dimensional subspaces independently.

**Benefits:**
1. **Diverse Representations:** Different heads capture different relationship types.
2. **Computational Efficiency:** Same total computation as single-head, but richer expressivity.
3. **Parallel Processing:** All $h$ heads compute simultaneously.

**Related Assignment Questions:** Q4 (dimension per head), Q5 (number of attention scores).

---

## 4. Transformer Architecture

### Components of a Transformer Block

A **Transformer block** combines several layers for both encoding and decoding:

1. **Multi-Head Self-Attention:** Allows each token to attend to all other tokens.
2. **Feed-Forward Network:** Fully-connected layers applied independently to each token position.
3. **Residual Connections:** Add input to output of each sub-layer ($y = x + F(x)$) to maintain gradient flow.
4. **Layer Normalization:** Stabilizes training by normalizing activations across channels.

**Mathematical Flow:**

$$\text{Attention Out} = \text{MultiHead}(X) + X \quad (\text{residual})$$
$$\text{Normalized} = \text{LayerNorm}(\text{Attention Out})$$
$$\text{FF Out} = \text{FFN}(\text{Normalized}) + \text{Normalized} \quad (\text{residual})$$
$$\text{Output} = \text{LayerNorm}(\text{FF Out})$$

### Why Transformers Work

**Parallelization:** Unlike RNNs, all time steps can be processed in parallel:
- RNN: $O(T)$ sequential steps.
- Transformer: $O(1)$ steps (all tokens simultaneously).

**Long-Range Dependencies:** Every token can directly attend to every other token in one step, providing a short gradient path regardless of sequence length.

**Scalability:** Transformers scale to massive datasets and model sizes (GPT-3, GPT-4, etc.).

---

## 5. Positional Embeddings

### The Problem: Loss of Order Information

Transformers process all tokens in parallel—the order is lost! An attention head doesn't inherently "know" whether a token is first or last in the sequence.

**Counterintuitive Truth:** "The cat sat on the mat" and "mat the on sat cat The" have the exact same attention computation if no positional information is added.

### Solution: Inject Positional Information into Embeddings

**Definition:** **Positional embeddings** are fixed or learned vectors that encode position information, added to the input token embeddings:

$$\hat{X} = X + \text{PE}$$

where $\text{PE}_{t,d}$ is the positional encoding for position $t$ and dimension $d$.

**Sinusoidal Positional Encoding (Standard in Transformers):**

$$\text{PE}_{t, 2i} = \sin\left(\frac{t}{10000^{2i/d_{model}}}\right)$$
$$\text{PE}_{t, 2i+1} = \cos\left(\frac{t}{10000^{2i/d_{model}}}\right)$$

**Intuition:**

Different frequencies create a "fingerprint" for each position:
- Low frequencies (large wavelengths): Change slowly across positions. Encode long-range structure.
- High frequencies (small wavelengths): Change rapidly. Encode fine-grained position details.

**Example:**

Position $t=0$: $[\sin(0), \cos(0), \sin(0), \cos(0), \ldots] = [0, 1, 0, 1, \ldots]$
Position $t=1$: $[\sin(1/10000^0), \cos(1/10000^0), \sin(1/10000^{2/d}), \ldots] \approx [0.841, 0.540, \ldots]$

Each position has a unique vector, allowing the model to learn position-aware attention patterns.

---

## 6. Transfer Learning

### The Core Idea

Training massive neural networks from scratch requires:
- Enormous datasets (billions of examples).
- Massive compute (days on GPU/TPU clusters).
- Significant expertise in hyperparameter tuning.

**Transfer Learning solves this:**

Pretrain a large model $g_{\phi^*}$ on a vast, general dataset. For a new downstream task with limited data, reuse the pretrained weights and add a task-specific head.

### Two-Stage Process

**Stage 1: Pretraining (Expensive, Done Once)**

Train a large model on a massive, general-purpose dataset:
- Language models trained on internet text.
- Vision models trained on ImageNet or similar.

The model learns **universal feature representations** that are broadly useful.

**Stage 2: Fine-Tuning (Cheap, Task-Specific)**

Take the pretrained model, remove the final classification layer, and add a small task-specific head:

$$z = g_{\phi^*}(x) \quad (\text{frozen or fine-tuned pretrained features})$$
$$\hat{y} = f_{\theta}(z) \quad (\text{new trainable classification head})$$

Train the new head (and optionally fine-tune the pretrained weights) on your downstream task.

**Example (Q6 from Assignment):**

Pretrained model outputs 128-dimensional embeddings. New task: classify into 10 classes.

New classification head: $10 = 128 \times W$ where $W \in \mathbb{R}^{128 \times 10}$.

Weights: $128 \times 10 = 1,280$ parameters.

### Why Transfer Learning Works

1. **Shared Structure:** Feature detectors (edges, shapes, semantic concepts) are useful across tasks.
2. **Dramatic Data Efficiency:** A small downstream dataset combined with pretrained features can achieve excellent performance.
3. **Reduced Training Time:** No need to train a huge model from scratch.

**Real-World Impact:** Modern NLP and vision rely almost entirely on transfer learning.

---

## 7. Knowledge Distillation

### The Problem: Large Models Are Slow

State-of-the-art models (BERT, GPT-3) have billions of parameters. They achieve high accuracy but are too slow for deployment on mobile devices or real-time applications.

**Challenge:** How to get a small model to be nearly as accurate as a large, expensive model?

### The Solution: Train a Student to Mimic a Teacher

**Knowledge Distillation** is a training technique where a smaller "student" network learns to mimic a larger, pretrained "teacher" network.

**Process:**

1. **Train Teacher:** Train a large, accurate model (teacher) on the dataset.
2. **Soft Targets:** Use the teacher's output probability distribution (soft probabilities from softmax, not hard labels) as targets for the student.
3. **Train Student:** The student minimizes the divergence between its output distribution and the teacher's:

$$L = \text{KL}(P_{\text{teacher}} \| P_{\text{student}})$$

or using cross-entropy:

$$L = -\sum_c P_{\text{teacher}}(c) \log P_{\text{student}}(c)$$

**Why Soft Targets?**

Hard labels (e.g., "class 2") lose information. If the teacher confidently assigns 95% probability to class 2 and 4% to class 1, this structure is informative. The student learns not just the correct class but also the teacher's "confidence ordering" over classes.

**Temperature Scaling:**

To make the teacher's soft targets even softer (smoother probability distribution), use temperature $T$:

$$P_{\text{soft}}(c) = \frac{\exp(\log\text{it}_c / T)}{\sum_{c'} \exp(\log\text{it}_{c'} / T)}$$

Higher $T$ → softer probabilities → more information about relative class similarities.

**Example:**

Teacher on 10-class problem:
- Hard: [0, 0, 1, 0, 0, 0, 0, 0, 0, 0] (class 2 is correct)
- Soft (temperature=1): [0.01, 0.05, 0.80, 0.06, 0.02, 0.01, 0.03, 0.01, 0.01, 0.00]
- Softer (temperature=4): [0.08, 0.10, 0.20, 0.11, 0.09, 0.08, 0.09, 0.08, 0.08, 0.08]

The softer distribution reveals that the teacher finds classes 1, 3, 4 somewhat confusable with class 2. The student learns this structure.

---

## 8. Stochastic Gradient Descent (SGD)

### The Basic Update Rule

**Definition:** Stochastic Gradient Descent updates parameters in the direction opposite to the gradient:

$$w_{t+1} = w_t - \eta \nabla L(w_t)$$

where:
- $w_t$: Parameter at iteration $t$.
- $\eta$: Learning rate (step size).
- $\nabla L(w_t)$: Gradient of loss with respect to $w$ at iteration $t$.

**Why "Stochastic"?**

The gradient is estimated from a mini-batch of samples, not the entire dataset. This introduces noise, which paradoxically helps escape local minima and improve generalization.

**Example (Q8 from Assignment):**

Current weight: $w = 5$, Learning rate: $\eta = 0.1$, Gradient: $\nabla L = 3$.

$$w_{t+1} = 5 - 0.1 \times 3 = 5 - 0.3 = 4.7$$

The parameter moves in the direction opposite to the gradient (downhill on the loss landscape).

### Learning Rate Effects

- **Too Small:** Slow convergence, may get stuck in shallow local minima.
- **Too Large:** Overshooting, divergence, wild oscillations.
- **Goldilocks:** Converges smoothly to good minima.

Finding the right learning rate is crucial.

---

## 9. Momentum

### The Problem: Oscillations in Narrow Valleys

Imagine a narrow, elongated valley in the loss landscape. Gradient descent oscillates left and right (orthogonal to the valley axis) while progressing slowly down the valley. This is inefficient.

**Solution: Accumulate Gradient Direction**

Momentum maintains a running weighted average of past gradients:

$$v_t = \beta v_{t-1} + (1 - \beta) g_t$$

where:
- $v_t$: Velocity (accumulated momentum).
- $g_t = \nabla L(w_t)$: Current gradient.
- $\beta \in [0, 1)$: Momentum coefficient (typically 0.9).

**Physical Intuition:** Think of a ball rolling downhill. Momentum gives the ball "inertia"—it doesn't stop immediately when the slope changes. It coasts forward, reducing oscillations.

**Parameter Update:**

$$w_{t+1} = w_t - \eta v_t$$

where $\eta$ is the learning rate applied to the momentum-smoothed gradient.

**Example (Q9 from Assignment):**

Previous momentum: $v_{t-1} = 2$, Current gradient: $g_t = 4$, Momentum coefficient: $\beta = 0.9$.

$$v_t = 0.9 \times 2 + 0.1 \times 4 = 1.8 + 0.4 = 2.2$$

The velocity incorporates both the accumulated direction (0.9 × 2 = 1.8) and the current gradient (0.1 × 4 = 0.4).

### Benefits

1. **Faster Convergence:** Accumulates speed in consistent directions.
2. **Reduced Oscillations:** Smooths out noise in gradient estimates.
3. **Better Exploration:** Can help escape shallow local minima.

---

## 10. RMSProp (Root Mean Square Propagation)

### The Problem: Non-Uniform Gradient Magnitudes

Different parameters have different gradient magnitudes:
- Some parameters consistently receive large gradients.
- Others receive small gradients.

Using a fixed learning rate for all parameters is suboptimal. Parameters with large gradients take huge steps (unstable), while those with small gradients crawl.

**Solution: Adaptive Learning Rates**

RMSProp scales the learning rate for each parameter based on the history of gradient magnitudes.

### The Update Rule

**Maintain Exponential Moving Average of Squared Gradients:**

$$s_t = \beta s_{t-1} + (1 - \beta) g_t^2$$

where:
- $s_t$: Second moment (running average of squared gradients).
- $g_t$: Current gradient.
- $\beta$: Decay rate (typically 0.9).

**Adaptive Parameter Update:**

$$w_{t+1} = w_t - \frac{\eta}{\sqrt{s_t + \epsilon}} g_t$$

where:
- $\eta$: Initial learning rate (now adapted per parameter).
- $\epsilon$: Small constant (e.g., $10^{-8}$) to prevent division by zero.

**Interpretation:**

Parameters with large historic gradients (large $s_t$) get scaled down $(\sqrt{s_t} \text{ is large})$. Parameters with small historic gradients (small $s_t$) get scaled up $(\sqrt{s_t} \text{ is small})$. This equalizes the effective step size across parameters.

**Example (Q10 from Assignment):**

Previous second moment: $s_{t-1} = 4$, Current gradient: $g_t = 2$.

$$s_t = 0.9 \times 4 + 0.1 \times 2^2 = 3.6 + 0.1 \times 4 = 3.6 + 0.4 = 4.0$$

Note the squared gradient: $g_t^2 = 4$.

### Benefits

1. **Adaptive Learning Rates:** Each parameter has its own effective step size.
2. **Handles Sparse Gradients:** Parameters with rare large gradients won't destabilize.
3. **No Momentum Bias:** Simpler than momentum, no need to tune velocity.

---

## 11. Adam (Adaptive Moment Estimation)

### Combining Momentum and RMSProp

Adam combines the best of both worlds:
- **Momentum:** First moment (mean of gradients) for accelerated convergence.
- **RMSProp:** Second moment (variance of gradients) for adaptive learning rates.

### The Update Rule

**Maintain Two Moments:**

$$m_t = \beta_1 m_{t-1} + (1 - \beta_1) g_t \quad (\text{first moment})$$
$$v_t = \beta_2 v_{t-1} + (1 - \beta_2) g_t^2 \quad (\text{second moment})$$

**Bias Correction:**

The first and second moments are biased toward zero early in training (especially when $\beta_1, \beta_2$ are close to 1). Correct for this:

$$\hat{m}_t = \frac{m_t}{1 - \beta_1^t}, \quad \hat{v}_t = \frac{v_t}{1 - \beta_2^t}$$

**Parameter Update:**

$$w_{t+1} = w_t - \frac{\eta}{\sqrt{\hat{v}_t} + \epsilon} \hat{m}_t$$

**Typical Hyperparameters:**

- $\beta_1 = 0.9$: Momentum coefficient.
- $\beta_2 = 0.999$: RMSProp coefficient (much higher).
- $\eta = 0.001$ or $0.0001$: Base learning rate.
- $\epsilon = 10^{-8}$.

### Why Adam Works So Well

1. **Combines Benefits:** Accelerated convergence (momentum) + adaptive rates (RMSProp).
2. **Robust:** Performs well across many problem domains without extensive hyperparameter tuning.
3. **Fast:** Converges quickly, especially useful for large models.

**Industry Standard:** Adam is the default optimizer in modern deep learning frameworks (PyTorch, TensorFlow).

---

## Key Takeaways

| Concept | Definition | Formula | Related Q |
|---------|-----------|---------|-----------|
| **Attention Mechanism** | Transform inputs to Q, K, V; compute similarity scores; weight-sum values | $\text{Attention}(Q,K,V) = \text{softmax}(\frac{QK^T}{\sqrt{d_k}})V$ | Q1, Q2, Q3 |
| **Score Matrix** | Dot products between all query-key pairs | $S = QK^T \in \mathbb{R}^{T \times T}$ | Q2 |
| **Dot Product** | Inner product of two vectors | $q^T k = \sum_i q_i k_i$ | Q3 |
| **Multi-Head Attention** | Run multiple attention mechanisms in parallel | $h$ heads, dimension per head: $d_k = d_{model}/h$ | Q4 |
| **Total Attention Scores** | Per-head score matrix size | $T \times T = T^2$ scores | Q5 |
| **Transfer Learning Head** | Map pretrained embeddings to task outputs | Weights: $(embed\_dim) \times (output\_classes)$ | Q6 |
| **Multi-Task Heads** | Separate heads for different tasks on shared representation | Total weights: $\sum_i (\text{shared\_dim} \times output\_dim_i)$ | Q7 |
| **SGD Update** | Gradient descent step | $w_{t+1} = w_t - \eta \nabla L(w_t)$ | Q8 |
| **Momentum** | Running average of gradients | $v_t = \beta v_{t-1} + (1-\beta) g_t$ | Q9 |
| **RMSProp** | Adaptive learning via squared gradient average | $s_t = \beta s_{t-1} + (1-\beta) g_t^2$ | Q10 |
| **Adam** | Combines momentum and RMSProp | $m_t, v_t$ → bias-corrected → update | Advanced |

---

## Summary

**Week 10 represents the pinnacle of the course:**

1. **Attention & Transformers:** Move from sequential RNNs to parallel transformers using query-key-value attention. Multi-head attention captures diverse relationships. Positional embeddings preserve order information.

2. **Scalability:** Transformers enable training on massive datasets and scale to billions of parameters, enabling modern LLMs.

3. **Practical Techniques:** Transfer learning leverages pretrained models for new tasks with limited data. Knowledge distillation compresses large models into fast, deployable versions.

4. **Advanced Optimization:** Modern optimizers (SGD with momentum, RMSProp, Adam) adaptively adjust learning rates and acceleration, enabling efficient training of large models.

5. **Mathematical Foundation:** All concepts—attention scoring, positional encoding, multi-task learning, adaptive optimization—rest on clean linear algebra and calculus principles.

**Bridge to Real-World ML:** These architectures and techniques power modern systems: ChatGPT, DALL-E, GPT-4, etc. Understanding them at the mathematical level (as this course provides) is essential for advancing the field.


# Week 9: Theoretical Notes — Convolutional and Recurrent Neural Networks

## Overview

Week 9 bridges two foundational deep learning architectures: **Convolutional Neural Networks (CNNs)** for processing grid-structured data (images) and **Recurrent Neural Networks (RNNs)** for handling sequential data (time series, text). Both are regularized variants of MLPs that exploit the structure of their input domains through parameter sharing and architectural constraints. This week also introduces practical implementation in PyTorch, connecting theory to code via Tensors, automatic differentiation, and training loops.

---

## 1. Convolutional Neural Networks: Beyond MLPs

### Local Receptive Fields

**Definition:** A **local receptive field** is a small spatial region of the input that connects to a single neuron in the next layer. Instead of fully connecting every input pixel to every hidden neuron (as in MLPs), a CNN neuron only "sees" a localized window of the input.

**Why It Matters:**

An MLP treating a $32 \times 32$ image (1024 pixels) with 256 hidden units would require:
$$1024 \times 256 + 256 = 262,144 \text{ parameters}$$

This is computationally expensive and prone to overfitting. But images have **spatial structure**: pixels are related to nearby pixels, not distant ones.

**Example (3×3 Local Receptive Field):**

A neuron in layer 1 might connect only to a $3 \times 3$ patch of the input:
$$\text{Connections per neuron} = 3 \times 3 = 9$$

With 256 hidden units in a single layer:
$$9 \times 256 = 2,304 \text{ parameters (much smaller!)}$$

**Related Assignment Questions:** Q1 demonstrates this with a full fully-connected layer count; Q2 and Q3 show spatial dimension and weight counting in convolutional layers.

### Parameter Sharing

**Definition:** **Parameter sharing** means the same filter (weight matrix) is applied across all positions in the input. A filter that detects a vertical edge in the top-left corner uses the **exact same weights** to detect vertical edges everywhere else in the image.

**Why It Works:**

Features like edges, corners, and textures appear at multiple locations in an image. It's wasteful to learn separate filters for each position. Instead, CNNs learn a small set of reusable filters.

**Example (Single Filter, Multiple Positions):**

A $3 \times 3$ filter $F$ slides across a $32 \times 32$ image:
- Position (0,0): Compute $\text{output}(0,0)$ using $F$ and the $3 \times 3$ patch at $(0,0)$.
- Position (1,0): Compute $\text{output}(1,0)$ using **the same $F$** and the $3 \times 3$ patch at $(1,0)$.
- Continue for all positions...

This reduces parameters dramatically compared to learning independent filters at each position.

### CNN as Regularized MLP

**Key Insight:** A CNN is fundamentally an MLP with two **hard regularization constraints** applied to its parameter space:

1. **Sparse Connectivity:** Due to local receptive fields, neurons don't connect to all previous layer activations.
2. **Weight Sharing:** Due to parameter sharing, many weights are forced to be identical.

These constraints reduce parameters and improve generalization by encoding the inductive bias: "visual patterns are translation-equivariant"—a useful feature should work anywhere in the image.

**Related Assignment Question:** Q1 asks for parameter counting in a fully-connected layer; CNNs reduce this dramatically.

---

## 2. The Convolution Operation

### Spatial Dimension Formula

**Definition:** When convolving a filter of size $k$ over an input of size $p$ with stride $s$ and padding $pad$, the output spatial dimension is:

$$\text{Output Size} = \left\lfloor \frac{p + 2 \times \text{pad} - k}{s} \right\rfloor + 1$$

**Parameters Explained:**

- **Input size $p$:** Width (or height) of the input feature map.
- **Kernel size $k$:** Width (or height) of the filter.
- **Stride $s$:** How many pixels the filter moves at each step.
- **Padding $\text{pad}$:** How many zeros are added around the boundary.

**Example (Q2 from Assignment):**

Input: $32 \times 32$, Kernel: $5 \times 5$, Stride: 1, Padding: 2.

$$\text{Output} = \left\lfloor \frac{32 + 2(2) - 5}{1} \right\rfloor + 1 = \left\lfloor \frac{31}{1} \right\rfloor + 1 = 32$$

The padding exactly compensates for the kernel size, preserving spatial dimensions ("same" padding).

### Filter Parameters

**Definition:** A convolutional filter extends through the **entire depth** (number of channels) of the input. For $F$ filters of size $k \times k$ applied to an input with $C$ channels:

$$\text{Total Weights} = F \times k \times k \times C$$

(excluding bias terms; with biases, add $F$).

**Example (Q3 from Assignment):**

32 filters, each $3 \times 3$, applied to an input with 16 channels:

$$\text{Weights} = 32 \times 3 \times 3 \times 16 = 4,608$$

With bias terms (one per filter): $4,608 + 32 = 4,640$.

**Related Assignment Question:** Q3 directly tests this calculation.

---

## 3. Pooling Operations

### Max Pooling and Average Pooling

**Definition:** **Pooling** is a downsampling operation that reduces spatial dimensions. The most common variants are:

- **Max Pooling:** Output the maximum value in each pooling window.
- **Average Pooling:** Output the average value in each pooling window.

**Spatial Dimension Formula:**

Pooling uses the same formula as convolution (but typically with $\text{pad} = 0$):

$$\text{Output Size} = \left\lfloor \frac{p - k}{s} \right\rfloor + 1$$

**Example (Q8 from Assignment):**

Input: $28 \times 28$, Kernel: $2 \times 2$, Stride: 2.

$$\text{Output} = \left\lfloor \frac{28 - 2}{2} \right\rfloor + 1 = 13 + 1 = 14$$

**Why Pooling?**

1. **Computational Efficiency:** Reduces feature map size → fewer parameters in subsequent layers.
2. **Feature Robustness:** Max pooling extracts the most prominent feature in each region, ignoring small variations.
3. **Translation Invariance:** Small shifts in input don't significantly change the pooled output.

---

## 4. Recurrent Neural Networks: Sequential Processing

### The RNN Cell

**Definition:** An **RNN cell** processes a sequence $\{x^0, x^1, \ldots, x^T\}$ by maintaining a **hidden state** $h_t$ that evolves with each time step:

$$z_t = W_{hh} h_{t-1} + W_{ih} x_t + b$$
$$h_t = \sigma(z_t)$$

where:
- $W_{hh}$: Weight matrix from previous hidden state.
- $W_{ih}$: Weight matrix from input.
- $b$: Bias vector.
- $\sigma$: Activation function (e.g., tanh, ReLU).

**Key Difference from MLP:** The same weights are reused across all time steps. This is **parameter sharing across time**—a constraint that allows RNNs to handle variable-length sequences.

**Example (Q5 from Assignment):**

Input size 12, hidden size 20:
- $W_{ih}$: $12 \times 20 = 240$ parameters.
- $W_{hh}$: $20 \times 20 = 400$ parameters.
- Bias: 20 parameters.
- **Total:** $240 + 400 + 20 = 660$ parameters.

### Why RNNs for Sequences?

**Problem with MLPs:** Each input must have a fixed size. A sequence of variable length cannot be directly fed to an MLP.

**RNN Solution:** Process one time step at a time, updating the hidden state. The hidden state acts as a "memory" of previous inputs, allowing the network to handle arbitrary-length sequences.

---

## 5. The Vanishing Gradient Problem in RNNs

### Backpropagation Through Time (BPTT)

**Definition:** **Backpropagation Through Time (BPTT)** is the algorithm for training RNNs. It unrolls the recurrent connections over time and applies standard backpropagation.

**Unrolled Computation Graph:**

For a sequence of length $T$:
$$h_0 \to h_1 \to h_2 \to \cdots \to h_T$$

The output $h_T$ depends on all previous hidden states through a chain of multiplications:

$$\frac{\partial h_T}{\partial h_t} = \prod_{k=t}^{T-1} \frac{\partial h_{k+1}}{\partial h_k}$$

### The Vanishing Gradient Problem

**Problem:** As $T - t$ increases (longer time gap), the gradient becomes a product of many small factors:

$$\frac{\partial h_T}{\partial h_t} = \frac{\partial h_{T}}{\partial h_{T-1}} \cdot \frac{\partial h_{T-1}}{\partial h_{T-2}} \cdot \ldots \cdot \frac{\partial h_{t+1}}{\partial h_t}$$

**Why Gradients Vanish:**

1. **Sigmoid/Tanh Derivative:** Both activation functions have derivatives bounded by $\leq 1$ (sigmoid: $a(1-a) \leq 0.25$; tanh: $1 - a^2 \leq 1$).
2. **Weight Matrix:** The largest singular value of $W_{hh}$ is typically $\leq 1$.

Multiplying many factors $\leq 1$ together yields a product that **decays exponentially**:
$$\prod_{k=t}^{T-1} 0.8 \approx 0.8^{T-t} \to 0 \text{ as } T - t \to \infty$$

**Consequence:** Error signals cannot propagate back to early time steps. The network "forgets" the beginning of the sequence and cannot learn long-range dependencies.

**Exploding Gradients:** Conversely, if singular values $> 1$, gradients can explode (grow exponentially), causing numerical instability and NaN losses.

---

## 6. Gradient Clipping

**Definition:** **Gradient clipping** is a technique to prevent exploding gradients. If the norm of the gradient exceeds a threshold $c$, the entire gradient is scaled to have norm exactly $c$:

$$\text{Scaling Factor} = \frac{c}{\|g\|_2}$$

**Applied Gradient:**
$$g_{\text{clipped}} = g \times \text{Scaling Factor} = g \times \frac{c}{\|g\|_2}$$

**Example (Q6 from Assignment):**

Current gradient norm: $\|g\|_2 = 12$, Threshold: $c = 5$.

$$\text{Factor} = \frac{5}{12} \approx 0.417$$

The gradient is scaled down by factor $0.417$, reducing exploding updates while preserving gradient direction.

**Related Assignment Question:** Q6 directly tests this calculation.

---

## 7. LSTMs: Solving Vanishing Gradients

### The LSTM Architecture

**Definition:** A **Long Short-Term Memory (LSTM)** cell introduces a separate **cell state** $C_t$ with **additive updates**, which allows gradients to flow uninterrupted through time.

**Four Gates (all sigmoid outputs: $\in [0, 1]$):**

1. **Forget Gate** $f_t$: How much of the old cell state to keep.
2. **Input Gate** $i_t$: How much of the new candidate state to add.
3. **Candidate State** $\tilde{C}_t$: New information to potentially add (using tanh).
4. **Output Gate** $o_t$: How much of the cell state to expose as hidden state.

**Cell State Update (Additive!):**

$$C_t = (f_t \odot C_{t-1}) + (i_t \odot \tilde{C}_t)$$

where $\odot$ denotes element-wise multiplication.

**Hidden State:**

$$h_t = o_t \odot \tanh(C_t)$$

**Example (Q7 from Assignment):**

$f_t = 0.6$, $C_{t-1} = 2$, $i_t = 0.5$, $\tilde{C}_t = 4$.

$$C_t = (0.6 \times 2) + (0.5 \times 4) = 1.2 + 2.0 = 3.2$$

### Why LSTMs Work

**Additive State Update:** The key difference is the $+$ in the cell state update. When computing gradients:

$$\frac{\partial C_t}{\partial C_{t-1}} = f_t \quad (\text{not a product of derivatives!})$$

Because forget gate $f_t$ is a standalone gate output, not a squashing function applied to $C_{t-1}$, the gradient doesn't suffer from the chain-rule multiplication problem of vanilla RNNs.

**Highway of Information:** During backprop, gradients flow directly through the cell state, bypassing the vanishing squeeze of activation functions.

---

## 8. GRUs and Residual Networks

### Gated Recurrent Units (GRUs)

**Definition:** A **GRU** is a simplified LSTM with only **two gates** instead of four, providing a good balance between expressivity and computational efficiency.

**GRU Equations:**

$$r_t = \sigma(W_r[h_{t-1}, x_t] + b_r) \quad \text{(reset gate)}$$
$$z_t = \sigma(W_z[h_{t-1}, x_t] + b_z) \quad \text{(update gate)}$$
$$\tilde{h}_t = \tanh(W_h[r_t \odot h_{t-1}, x_t] + b_h) \quad \text{(candidate)}$$
$$h_t = (1 - z_t) \odot h_{t-1} + z_t \odot \tilde{h}_t$$

The update gate $z_t$ interpolates between old and new hidden states, similar to LSTM's forget and input gates combined.

### Residual Networks (ResNets)

**Definition:** A **residual block** uses a **skip connection** to add the input directly to the output:

$$y = x + F(x)$$

where $F(x)$ is the non-linear transformation (e.g., a few stacked convolutional or fully-connected layers).

**Gradient Flow in Residual Blocks:**

During backprop, the gradient w.r.t. input is:

$$\frac{\partial L}{\partial x} = \frac{\partial L}{\partial y} \left( 1 + \frac{\partial F}{\partial x} \right)$$

The $+1$ term ensures that gradients can flow directly through the skip connection, even if $\frac{\partial F}{\partial x}$ is very small. This mitigates vanishing gradients in deep networks.

**Example (Q9 from Assignment):**

Given $y = x + F(x)$ and $dL/dy = \delta$:

$$\frac{dL}{dx} = \delta \left( 1 + F'(x) \right)$$

The gradient always includes the $\delta$ term from the skip connection, preventing vanishing.

**Related Assignment Question:** Q9 directly tests the chain rule in residual blocks.

---

## 9. Activation Functions and Their Derivatives

### Sigmoid Function

**Formula:**
$$\sigma(z) = \frac{1}{1 + e^{-z}}$$

**Derivative:**
$$\sigma'(z) = \sigma(z)(1 - \sigma(z)) = a(1 - a)$$

where $a = \sigma(z)$ is the activation value.

**Key Property:** The derivative can be expressed entirely in terms of the output, making it efficient to compute during backprop.

**Example (Q4 from Assignment):**

If $a = 0.8$: $\sigma'(z) = 0.8 \times 0.2 = 0.16$.

**Related Assignment Question:** Q4 tests this property.

### ReLU Function

**Formula:**
$$\text{ReLU}(z) = \max(0, z)$$

**Derivative:**
$$\text{ReLU}'(z) = \begin{cases} 1 & \text{if } z > 0 \\ 0 & \text{if } z < 0 \\ \text{undefined} & \text{if } z = 0 \end{cases}$$

**Key Advantage:** No squashing; for positive inputs, the gradient is exactly 1, allowing unattenuated gradient flow during backprop.

**Example (Q10 from Assignment):**

At $z = 3$: $\text{ReLU}'(3) = 1$.

**Related Assignment Question:** Q10 tests ReLU derivative.

---

## 10. PyTorch Fundamentals

### Tensors and Operations

**Definition:** A **Tensor** in PyTorch is a multi-dimensional array that can be processed on GPUs and supports automatic differentiation.

**Creating Tensors:**

```python
import torch
x = torch.randn(32, 64)  # Shape (32, 64) with random normal values
y = torch.zeros(10)      # Shape (10,) filled with zeros
```

**Element-wise Operations:**

```python
z = x + y       # Addition
w = x * y       # Multiplication
h = torch.tanh(x)  # Apply activation function
```

**Matrix Operations:**

```python
w = torch.mm(A, B)  # Matrix multiplication
h = torch.matmul(x, W)  # Flexible matrix product (handles broadcasting)
```

### Automatic Differentiation (AutoGrad)

**Definition:** PyTorch's **AutoGrad** system automatically computes gradients of any scalar output with respect to leaf Tensors (inputs) that have `requires_grad=True`.

**Computational Graph:**

When operations are performed on Tensors with `requires_grad=True`, PyTorch builds a **Directed Acyclic Graph (DAG)** recording the computation:

```python
x = torch.tensor([2.0], requires_grad=True)
y = x ** 2
z = y * 3  # z = 3 * x^2
```

The DAG is: $x \to y \to z$.

**Backward Pass:**

```python
z.backward()  # Computes dz/dx via chain rule
print(x.grad)  # gradient: dz/dx = 6*x = 12 at x=2
```

**Related Assignment Questions:** Q1 and Q5 involve parameter counting that relates to understanding tensor shapes in PyTorch.

---

## 11. Training MLPs in PyTorch

### Model Definition

**Example:**

```python
import torch.nn as nn

class MLP(nn.Module):
    def __init__(self, input_dim, hidden_dim, output_dim):
        super().__init__()
        self.fc1 = nn.Linear(input_dim, hidden_dim)
        self.fc2 = nn.Linear(hidden_dim, output_dim)
    
    def forward(self, x):
        x = torch.relu(self.fc1(x))
        return self.fc2(x)

model = MLP(256, 64, 10)
```

The `nn.Linear` layer computes $y = Wx + b$ with learnable weights $W$ and biases $b$.

**Parameter Count:**

```python
total_params = sum(p.numel() for p in model.parameters())
```

For the MLP above (256→64→10):
- Layer 1: $256 \times 64 + 64 = 16,448$ (including bias).
- Layer 2: $64 \times 10 + 10 = 650$ (including bias).

**Related Assignment Question:** Q1 tests understanding of parameter counting.

### Training Loop

**Pseudo-code:**

```python
optimizer = torch.optim.SGD(model.parameters(), lr=0.01)
loss_fn = nn.CrossEntropyLoss()

for epoch in range(num_epochs):
    for batch_x, batch_y in train_loader:
        # Forward pass
        logits = model(batch_x)
        loss = loss_fn(logits, batch_y)
        
        # Backward pass
        optimizer.zero_grad()
        loss.backward()
        
        # Update parameters
        optimizer.step()
```

**Key Steps:**

1. **Forward:** Compute predictions.
2. **Loss:** Compare predictions to targets.
3. **Backward:** Compute gradients via AutoGrad.
4. **Optimizer.step():** Update weights using gradients.

---

## Key Takeaways

| Concept | Definition | Formula/Example | Related Q |
|---------|-----------|-----------------|-----------|
| **Local Receptive Field** | CNN neurons connect only to small input patches | $3 \times 3$ patch instead of entire image | Q1, Q2 |
| **Parameter Sharing** | Same filter applied across all positions | One filter, many locations | Q1, Q3 |
| **Convolution Output Size** | Spatial dimension formula | $\lfloor(p + 2 \cdot \text{pad} - k)/s\rfloor + 1$ | Q2 |
| **CNN Filter Weights** | Total weights per convolutional layer | $F \times k \times k \times C$ | Q3 |
| **Max Pooling** | Downsampling via maximum value | Output: $\lfloor(p-k)/s\rfloor + 1$ | Q8 |
| **RNN Cell** | Processes sequences with hidden state | $h_t = \sigma(W_{hh}h_{t-1} + W_{ih}x_t + b)$ | Q5 |
| **Vanishing Gradients** | Gradients decay exponentially in RNNs | $\prod \leq 1 \Rightarrow 0$ as $T-t \to \infty$ | Q6 |
| **Gradient Clipping** | Prevent exploding gradients | Factor: $c / \|g\|_2$ | Q6 |
| **LSTM Cell State** | Additive state update with gates | $C_t = f_t \odot C_{t-1} + i_t \odot \tilde{C}_t$ | Q7 |
| **Residual Block** | Skip connection for gradient flow | $y = x + F(x)$; gradient: $\delta(1 + F'(x))$ | Q9 |
| **Sigmoid Derivative** | Output-based derivative | $a(1-a)$ where $a=\sigma(z)$ | Q4 |
| **ReLU Derivative** | Linear pass-through for positive inputs | 1 if $z > 0$; 0 if $z < 0$ | Q10 |

---

## Summary

**Week 9 consolidates two essential architectures and introduces practical deep learning:**

1. **CNNs** exploit spatial structure through local receptive fields and parameter sharing, dramatically reducing parameters while improving generalization for grid-like data.

2. **The Convolution Operation** is controlled by kernel size, stride, and padding, with spatial dimensions computed via a simple formula.

3. **RNNs** handle variable-length sequences by reusing weights across time steps, but suffer from vanishing gradients due to the product of many small derivative terms.

4. **LSTMs and GRUs** solve vanishing gradients with additive state updates and gating mechanisms, allowing networks to learn long-range dependencies.

5. **ResNets** use skip connections ($y = x + F(x)$) to maintain strong gradient flow in very deep networks, applicable to both CNNs and RNNs.

6. **Activation Functions** (sigmoid, tanh, ReLU) each have different gradient properties that affect learning dynamics. ReLU's gradient of 1 for positive inputs is particularly beneficial for avoiding vanishing gradients.

7. **PyTorch** provides the infrastructure for building these networks: Tensors for computation, AutoGrad for automatic differentiation, and nn.Module for clean model definitions.

8. **Training** involves forward passes, loss computation, backprop via AutoGrad, and optimizer steps—a cycle repeated over epochs.

**Bridge to Next Week:** These foundational architectures are building blocks for more complex models (Transformers, attention mechanisms, and modern generative models), which process CNNs and RNNs at scale for state-of-the-art applications.


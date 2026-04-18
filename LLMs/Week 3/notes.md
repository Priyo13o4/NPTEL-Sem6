# Week 3: Theoretical Notes — Deep Learning Foundations + PyTorch Basics

## Overview
Week 3 moves from statistical counts to **neural networks**: how a neuron/perceptron computes predictions, why **non-linearity** is essential, and how **MLPs** solve problems like XOR that single-layer models cannot. We also cover **backpropagation** (chain rule), common training pathologies (vanishing/exploding gradients), and regularization methods to improve generalization. Finally, we introduce practical PyTorch building blocks: tensors, autograd, and the standard training pipeline.

---

## Lecture 5 — Introduction to Deep Learning

### A short history (why the field looks like this)
- **McCulloch–Pitts neuron (1943)**: binary inputs with a threshold.
- **Perceptron (1957)**: learns weights for linear classification.
- **MLP (1968)**: multiple layers (but training was hard at the time).
- **Backpropagation (1986)**: practical gradient-based training for multilayer nets.
- **Universal Approximation Theorem (1989)**: sufficiently wide networks can approximate many functions.

**Related Assignment Questions**: Q5 (XOR motivates MLP), Q6 (backprop).

---

## The “neuron” idea

### Biological analogy (intuition)
A neuron aggregates inputs, compares against a threshold, and fires. Artificial neurons mimic this: a weighted sum plus an activation function.

### MPNet (McCulloch–Pitts)
- Inputs are binary ($0/1$).
- Output is based on a **threshold function**.
- Can model logic gates like AND/OR.

**Key limitation**: no learning of real-valued weights in the modern sense.

---

## Perceptron

### Definition
Given input vector $x \in \mathbb{R}^d$, weights $w \in \mathbb{R}^d$, and bias $b$:

$$a = w^\top x + b$$

Then apply an activation (classic perceptron used a sign/step-like function):

$$\hat{y} = \text{sign}(a)$$

### Linear separability
A single perceptron learns a **linear decision boundary** (a hyperplane). It works only when classes are **linearly separable**.

### XOR problem (why a single layer fails)
XOR’s positive points lie in opposite corners, so no single straight line can separate positives from negatives.

**Related Assignment Questions**: Q5.

---

## Multi-Layer Perceptron (MLP)

### Architecture (feedforward)
An MLP stacks layers:
- **Input layer** → **Hidden layer(s)** → **Output layer**

For one hidden layer example:

$$a^{(1)} = W^{(1)}x + b^{(1)},\quad z^{(1)} = f(a^{(1)})$$
$$a^{(2)} = W^{(2)}z^{(1)} + b^{(2)},\quad \hat{y} = g(a^{(2)})$$

Where:
- $a$ = **pre-activation** (linear output)
- $z$ = **post-activation** (after non-linearity)

**Depth** = number of hidden layers.  
**Width** = neurons per layer.

### Why non-linearity matters
If every activation is linear, the whole network collapses into a single linear map:

$$W^{(2)}(W^{(1)}x) = (W^{(2)}W^{(1)})x$$

So “deep” becomes meaningless without non-linear activations.

**Related Assignment Questions**: Q1, Q2.

---

## Activation functions

### What activations do
Activations introduce **non-linearity**, enabling neural nets to represent complex functions.

**Related Assignment Questions**: Q1.

### Common activations (high level)
- **Linear**: no non-linearity (collapses layers).
- **Sigmoid**: outputs in $(0,1)$; can cause **vanishing gradients**.
- **Tanh**: outputs in $(-1,1)$; often better centered than sigmoid but can still vanish.
- **ReLU**: $\max(0, x)$; helps with vanishing gradients for positive inputs, but can create **dead neurons** (stuck output 0).
- **Leaky ReLU / PReLU**: allow small/learned negative slope to reduce dead neurons.
- **Softmax**: converts logits to probabilities that sum to 1 (multi-class outputs).
- **ELU, GELU**: smooth variants often used in modern nets/Transformers.
- **GLU / SwiGLU**: gating via element-wise multiplication; common in large LLM MLP blocks.
- **Swish/SiLU**: smooth non-linearity used in some architectures.

### Vanishing gradient (intuition)
In deep nets, gradients multiply through many layers. If derivatives are mostly small (as in sigmoid/tanh saturation), gradients shrink toward 0 → early layers learn very slowly.

**Related Assignment Questions**: Q3.

### Exploding gradients
If derivatives/weights cause gradients to grow across layers, updates can blow up → unstable training.

---

## Backpropagation (how networks learn)

### Core idea
Backprop computes gradients of the loss w.r.t. parameters using the **chain rule**, propagating from output back to input.

For parameters $\theta$ (weights/biases), gradient descent updates:

$$\theta \leftarrow \theta - \eta\,\nabla_\theta \mathcal{L}$$

where $\eta$ is the learning rate.

**Related Assignment Questions**: Q6, Q8.

### Why “chain rule” matters
Each layer depends on previous layers, so:

$$\frac{\partial \mathcal{L}}{\partial W^{(1)}} = \frac{\partial \mathcal{L}}{\partial a^{(2)}}\,\frac{\partial a^{(2)}}{\partial z^{(1)}}\,\frac{\partial z^{(1)}}{\partial a^{(1)}}\,\frac{\partial a^{(1)}}{\partial W^{(1)}}$$

You don’t need to memorize this product; the takeaway is: **gradients flow backward through dependencies**.

---

## Regularization (generalization over memorization)

### Goal
Regularization reduces **overfitting** and improves generalization.

**Related Assignment Questions**: Q10.

### Common methods
- **Early stopping**: stop when validation performance stops improving.
- **L1 regularization**: adds $\lambda\|W\|_1$; encourages **sparsity** (many weights become exactly 0).
- **L2 regularization (weight decay)**: adds $\lambda\|W\|_2^2$; discourages large weights.
- **Dropout**: randomly disable units during training; disabled at inference.

**Related Assignment Questions**: Q4, Q7.

---

## Lecture 6 — Introduction to PyTorch

### Tensors
A **tensor** is a multi-dimensional array (like NumPy) that can live on CPU or GPU.

Common creation APIs:
- `torch.tensor([...])`
- `torch.rand(...)`, `torch.zeros(...)`, `torch.ones(...)`, `torch.empty(...)`
- `torch.arange(...)`

Key tensor properties:
- `x.ndim`, `x.shape`
- `x.dtype` (often float32 by default)
- `x.device` (CPU or CUDA)

#### `torch.tensor` vs `torch.from_numpy`
- `torch.from_numpy(np_array)` typically **shares memory** with NumPy.
- `torch.tensor(np_array)` typically **copies**.

### Shape operations
- `.view(...)` / `.reshape(...)` to change shape.
- `torch.cat([a,b], dim=...)` concatenates along dimensions.
- Reductions: `sum`, `mean`, `argmax`.

---

## Autograd (automatic differentiation)

### Computational graph
If `requires_grad=True`, PyTorch tracks operations and builds a dynamic computation graph.

- **Leaf tensor**: created by the user; gradients are stored in `.grad`.
- **Non-leaf tensor**: result of operations; has a `grad_fn` (e.g., `AddBackward`, `MulBackward`).

### Backward pass
- Call `loss.backward()` to populate gradients.
- Gradients **accumulate** by default; reset them each step (e.g., `optimizer.zero_grad()`).

### Inference mode
Use `torch.no_grad()` during evaluation/inference to avoid tracking gradients and save memory.

---

## PyTorch training pipeline (typical)

1. Define model as an `nn.Module` with `__init__` and `forward`.
2. Choose loss (e.g., `nn.CrossEntropyLoss`).
3. Choose optimizer (e.g., `torch.optim.Adam`).
4. Load data with `DataLoader(batch_size=..., shuffle=...)`.
5. Loop: forward → loss → backward → `optimizer.step()` → `optimizer.zero_grad()`.

(Example used in lecture: MNIST MLP like $784 \to 400 \to 10$ with ReLU.)

---

## Key Takeaways

| Concept | Definition | Example |
|:---|:---|:---|
| Activation function | Adds non-linearity | ReLU, sigmoid, tanh |
| Linear collapse | All-linear MLP reduces to one linear map | $W_2(W_1x)=(W_2W_1)x$ |
| Vanishing gradient | Gradients shrink in deep nets | sigmoid/tanh saturation |
| Backpropagation | Chain rule gradients backward | output → input layers |
| Dropout | Randomly drop units during training | disabled at inference |
| L1 regularization | Encourages sparsity | many weights → 0 |
| Softmax | Multi-class probabilities | outputs sum to 1 |
| Learning rate too high | Unstable updates | loss oscillates/diverges |
| Tensor | Multi-dim array on CPU/GPU | `torch.rand(2,3)` |
| Autograd | Automatic gradients | `loss.backward()` |

---

## Summary
- Non-linear activations are what make neural networks expressive; without them, depth collapses into a single linear transform.
- Single-layer perceptrons fail on XOR because XOR is not linearly separable; MLPs solve it by composing multiple linear regions.
- Backpropagation applies the chain rule backward through layers; training stability depends on gradients and learning rate.
- Regularization (L1/L2, dropout, early stopping) is primarily about generalization.
- PyTorch provides tensors + autograd + `nn.Module` to implement the full training loop efficiently.

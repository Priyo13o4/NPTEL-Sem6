# Week 8: Theoretical Notes — Kernel Methods and Neural Networks

## Overview

Week 8 extends SVMs to handle non-separable data and introduces the **Kernel Trick**—one of machine learning's most elegant ideas. Then, it pivots to **Neural Networks**, which generalize SVMs by learning optimal transformations automatically. We prove the **Universal Approximation Theorem** (neural networks can approximate any function) and introduce **Backpropagation**, the algorithm that powers deep learning.

---

## Soft-Margin SVM and Slack Variables

### The Problem: Non-Linearly Separable Data

In Week 7, we assumed data was **perfectly linearly separable**: a line could separate the two classes with no misclassifications. Real data is messy.

**Example:** A red apple accidentally rolls into a field of green apples. No matter where you draw a straight line, you'll either misclassify that red apple or push your margin so narrow it won't generalize.

### The Solution: Slack Variables

We introduce **slack variables** $\xi_i \geq 0$ (pronounced "xi") for each training point:

$$y_i (w^T x_i + b) \geq 1 - \xi_i$$

**Interpretation:**
- $\xi_i = 0$: Point is safely on the correct side of the margin (ideal).
- $0 < \xi_i < 1$: Point sits inside the margin but is still correctly classified.
- $\xi_i \geq 1$: Point is misclassified (on the wrong side of the boundary).

Slack variables literally "slack off" the margin constraint to allow violations.

### The Soft-Margin Objective

$$\min_{w, b, \xi} \frac{1}{2}\|w\|_2^2 + C \sum_{i=1}^{n} \xi_i$$

subject to:
$$y_i (w^T x_i + b) \geq 1 - \xi_i, \quad \xi_i \geq 0 \, \forall i$$

**Parameters:**
- First term: Regularization. Minimize weight magnitude to maximize margin.
- Second term: Slack penalty. Penalize violations based on their severity.
- $C > 0$: Hyperparameter controlling the trade-off.

**Effect of $C$:**
- **Large $C$** (e.g., 1000): Heavy penalty for violations. Hard margin. Fewer training errors, possibly overfit.
- **Small $C$** (e.g., 0.01): Lenient on violations. Soft margin. Wider margin, possibly underfits but generalizes better.

### Connection to Regularization

This is the same bias-variance tradeoff from Week 7:
- **Small $C$** → Allow training violations → Larger margin → Better generalization.
- **Large $C$** → Punish violations heavily → Smaller margin, memorize data → Worse generalization.

**Related Assignment Questions:** Q7

---

## The Dual Problem Revisited

### Why the Dual Matters

The **Primal Problem** (above) is in terms of weights $w$ and $b$—unknowns we want to solve for.

The **Dual Problem** reformulates everything in terms of **Lagrange multipliers** $\alpha_i$ and **dot products** $x_i^T x_j$.

### The Dual Formulation

Using Lagrangian duality, the dual SVM problem is:

$$\max_{\alpha} \sum_{i=1}^{n} \alpha_i - \frac{1}{2} \sum_{i=1}^{n} \sum_{j=1}^{n} \alpha_i \alpha_j y_i y_j (x_i^T x_j)$$

subject to:
$$0 \leq \alpha_i \leq C, \quad \sum_{i=1}^{n} \alpha_i y_i = 0$$

### Key Insight: Sparsity in the Dual

**Complementary Slackness:** For most training points, $\alpha_i = 0$ (exactly zero).

- If $\alpha_i > 0$: Point $i$ is a **support vector** (on the margin or violating it).
- If $\alpha_i = 0$: Point $i$ can be deleted; it doesn't affect the solution.

This sparsity is powerful:
1. **Interpretability:** The model depends only on a few key data points.
2. **Efficiency:** Computing the decision for a new point only requires dot products with support vectors.

### Weight Vector in Dual Form

The learned weights can be expressed entirely in terms of support vectors:

$$w = \sum_{i=1}^{n} \alpha_i y_i x_i$$

Since $\alpha_i = 0$ for non-support vectors, only support vectors contribute.

The decision boundary for a new test point $x$ is:

$$f(x) = \sum_{i=1}^{n} \alpha_i y_i (x_i^T x) + b$$

**Notice:** The computation involves only **dot products** $x_i^T x$, not the weights $w$ explicitly!

**Related Assignment Questions:** Q8

---

## SVM as Empirical Risk Minimization

### The Hinge Loss

After Week 7's geometric insights, Week 8 reveals a beautiful fact: **SVM is just regularized loss minimization.**

**Hinge Loss** is defined as:

$$L_{\text{hinge}}(y_i, \hat{y}_i) = \max(0, 1 - y_i \hat{y}_i)$$

where $\hat{y}_i = w^T x_i + b$ is the raw score and $y_i \in \{-1, +1\}$ is the label.

**Behavior:**
- If $y_i \hat{y}_i \geq 1$ (confident and correct): Loss = 0 (no penalty).
- If $0 < y_i \hat{y}_i < 1$ (uncertain or slightly wrong): Loss = $1 - y_i \hat{y}_i$ (linear penalty).
- If $y_i \hat{y}_i \leq 0$ (misclassified): Loss = $1 - y_i \hat{y}_i \geq 1$ (large penalty).

**Intuition:** Hinge loss is forgiving—points safely on the correct side incur zero loss. Only violations are penalized, and the penalty grows linearly.

### Unconstrained Formulation

The soft-margin SVM primal problem is **mathematically equivalent** to:

$$\min_{w, b} \frac{1}{2}\|w\|_2^2 + C \sum_{i=1}^{n} \max(0, 1 - y_i(w^T x_i + b))$$

This is standard **regularized empirical risk minimization**:
- Regularization term: $\frac{1}{2}\|w\|_2^2$ (L2 penalty)
- Loss term: $C \sum_i L_{\text{hinge}}(y_i, \hat{y}_i)$ (weighted hinge loss)

**Insight:** SVM is not fundamentally different from Logistic Regression (which uses cross-entropy loss). The only difference is the loss function choice!

**Related Assignment Questions:** Q3, Q9

---

## The Kernel Trick: Mathematical Magic

### The Problem: Implicit Feature Expansion

Suppose data is not linearly separable in the original space. A solution is to map data to a higher-dimensional space using a transformation $\phi: \mathbb{R}^d \rightarrow \mathbb{R}^m$ where $m \gg d$.

**Example (2D → 3D):**
- Original features: $x = [x_1, x_2]$
- Expanded features: $\phi(x) = [x_1, x_2, x_1 x_2, x_1^2, x_2^2]$

Then fit a linear SVM in the expanded space:
$$f(x) = \sum_i \alpha_i y_i (\phi(x_i)^T \phi(x)) + b$$

**Problem:** Computing $\phi(x)$ for millions of high-dimensional points is prohibitively expensive. In some cases (like Gaussian RBF kernel), $\phi(x)$ maps to infinite dimensions!

### The Kernel Trick Solution

**Key Observation:** The SVM dual objective only uses **dot products** between training points. We never need to explicitly compute $\phi(x)$; we only need $\phi(x_i)^T \phi(x_j)$.

**Kernel Function:** A function $k: \mathbb{R}^d \times \mathbb{R}^d \rightarrow \mathbb{R}$ that computes:
$$k(x_i, x_j) = \phi(x_i)^T \phi(x_j)$$

without ever computing $\phi$ explicitly!

**The Trick:** Replace $x_i^T x_j$ with $k(x_i, x_j)$ in the dual:

$$\max_{\alpha} \sum_{i=1}^{n} \alpha_i - \frac{1}{2} \sum_{i=1}^{n} \sum_{j=1}^{n} \alpha_i \alpha_j y_i y_j k(x_i, x_j)$$

**Decision function:**
$$f(x) = \sum_i \alpha_i y_i k(x_i, x) + b$$

**Benefit:** We compute high-dimensional dot products instantly without ever materializing the high-dimensional space!

**Related Assignment Questions:** Q4

---

## Mercer's Theorem and Valid Kernels

### The Validity Question

Not every function can be a valid kernel. A function might compute a dot product in a non-existent space (containing mathematical paradoxes like negative distances).

**Mercer's Theorem** provides the mathematical gatekeeper:

A function $k: \mathbb{R}^d \times \mathbb{R}^d \rightarrow \mathbb{R}$ is a valid kernel (i.e., corresponds to some transformation $\phi$) if and only if:

$$k(x, z) = \phi(x)^T \phi(z)$$

for some function $\phi$, which is equivalent to requiring that the **Gram matrix** $K$ with entries $K_{ij} = k(x_i, x_j)$ is **Positive Semidefinite (PSD)** for all possible datasets $\{x_1, \ldots, x_n\}$.

### Understanding Positive Semidefiniteness

A matrix $K$ is **Positive Semidefinite** if:
$$v^T K v \geq 0 \quad \forall v \in \mathbb{R}^n$$

**Intuition:** All eigenvalues are non-negative. The "space" encoded by the kernel is geometrically valid (no paradoxes like negative squared distances).

**Practical Implication:** If someone proposes a kernel function $k$, compute the Gram matrix on your data and check if it's PSD (eigenvalues $\geq 0$). If yes, it's valid.

**Related Assignment Questions:** Q6

---

## Common Kernel Functions

### 1. Linear Kernel

$$k(x, z) = x^T z$$

No transformation. SVM becomes standard linear SVM.

### 2. Polynomial Kernel

$$k(x, z) = (1 + x^T z)^p$$

Maps to a space of all polynomial features up to degree $p$.

**Example ($p = 2$, $d = 2$):**
- Original: $[x_1, x_2]$
- Expanded (roughly): $[1, x_1, x_2, x_1^2, x_1 x_2, x_2^2]$ (degree ≤ 2 terms)

**Effect:** Creates polynomial decision boundaries (curves of degree $p$).

**Related Assignment Questions:** Q5

---

### 3. Gaussian (RBF) Kernel

$$k(x, z) = \exp\left( -\gamma \|x - z\|_2^2 \right)$$

where $\gamma > 0$ is a bandwidth parameter.

**Transformation:** Maps to infinite-dimensional space! The underlying $\phi$ is infinite-dimensional, yet the kernel trick handles it instantly.

**Interpretation:** 
- $k(x, z)$ is high (close to 1) if $x$ and $z$ are close (within distance $1/\sqrt{\gamma}$).
- $k(x, z)$ is low (close to 0) if $x$ and $z$ are far apart.

This creates **local decision boundaries**: the classifier is most influenced by nearby training points.

### 4. Sigmoid Kernel

$$k(x, z) = \tanh(\kappa x^T z + \theta)$$

Inspired by neural networks. Less commonly used now (neural networks themselves are preferred).

**Note:** Not always PSD, so needs validation.

---

## Introduction to Neural Networks

### What is a Neural Network?

A **neural network** is a composite function built by stacking layers. Each layer applies:

1. **Linear transformation:** $z = W x + b$
2. **Non-linear activation:** $a = \sigma(z)$

These alternate layer by layer.

### Architecture

For an $L$-layer network:

**Input:** $x^{(0)} = x \in \mathbb{R}^{d_0}$

**For each layer $\ell = 1, \ldots, L$:**
- **Pre-activation (raw scores):** $z^{(\ell)} = W^{(\ell)} a^{(\ell-1)} + b^{(\ell)}$
- **Activation (squished output):** $a^{(\ell)} = \sigma(z^{(\ell)})$

where $W^{(\ell)} \in \mathbb{R}^{d_\ell \times d_{\ell-1}}$ and $\sigma$ is the activation function.

**Output:** $\hat{y} = a^{(L)}$ (final layer output, possibly with softmax for classification).

### Notation

Standard neural network notation:
- $z_j^{(\ell)}$: Pre-activation of neuron $j$ in layer $\ell$ (raw linear sum).
- $a_j^{(\ell)}$: Activation of neuron $j$ in layer $\ell$ (after $\sigma$ applied).
- $w_{jk}^{(\ell)}$: Weight from neuron $k$ in layer $\ell-1$ to neuron $j$ in layer $\ell$.
- $b_j^{(\ell)}$: Bias of neuron $j$ in layer $\ell$.

**Related Assignment Questions:** Q1, Q10

---

### Common Activation Functions

| Function | Formula | Range | Properties |
|----------|---------|-------|-----------|
| **Sigmoid** | $\sigma(z) = \frac{1}{1 + e^{-z}}$ | $(0, 1)$ | Smooth, bounded, differentiable |
| **Tanh** | $\tanh(z) = \frac{e^z - e^{-z}}{e^z + e^{-z}}$ | $(-1, 1)$ | Smooth, zero-centered |
| **ReLU** | $\text{ReLU}(z) = \max(0, z)$ | $[0, \infty)$ | Non-differentiable at 0, efficient |
| **Softmax** | $\text{softmax}(z)_j = \frac{e^{z_j}}{\sum_k e^{z_k}}$ | $(0, 1)$, sums to 1 | Normalized probabilities for classification |

---

## The Universal Approximation Theorem

### The Statement

**Universal Approximation Theorem (Cybenko, Hornik):**

Let $\sigma$ be a bounded, continuous, and non-constant activation function. Then, for any continuous function $f: \mathbb{R}^d \rightarrow \mathbb{R}$ and any $\epsilon > 0$, there exists a **neural network with one hidden layer** such that:

$$|f(x) - \hat{f}(x)| < \epsilon \quad \forall x \in [a, b]^d$$

**Translation:** With enough hidden neurons and the right weights, a single-hidden-layer network can approximate any continuous function to arbitrary accuracy.

### Why This Matters

This is the holy grail of machine learning theory. It mathematically proves that neural networks have **universal expressivity**: no function is too complex for them (given enough neurons).

### The Conditions

**Activation $\sigma$ must be:**

1. **Bounded:** Does not shoot to infinity. Examples: Sigmoid, Tanh, ReLU (approximately). Counter-example: Linear function $z$ (unbounded).
2. **Continuous:** No jumps or breaks. Ensures smooth function approximation.
3. **Non-constant:** Not a flat line. Otherwise, the network output is constant regardless of input.

**Why Linear Doesn't Work:** If $\sigma(z) = z$, then stacking linear layers just creates another linear layer (linear compositions are linear). The network collapses to: $\hat{f}(x) = W_L W_{L-1} \cdots W_1 x + b$, which is just linear regression. It cannot approximate non-linear functions!

### Limitations of the Theorem

- **Existence, not construction:** It proves a network exists, but doesn't say how to find the weights.
- **Hidden layer size:** May require exponentially many neurons for some functions.
- **Compactness:** Assumes input domain is compact (bounded).

**Related Assignment Questions:** Q2

---

## Forward and Backward Propagation

### Forward Pass (Inference)

To compute the network's output for a test sample $x$:

1. **Layer 1:** $z^{(1)} = W^{(1)} x + b^{(1)}$, $a^{(1)} = \sigma(z^{(1)})$
2. **Layer 2:** $z^{(2)} = W^{(2)} a^{(1)} + b^{(2)}$, $a^{(2)} = \sigma(z^{(2)})$
3. ... (continue through all layers)
4. **Layer $L$:** $z^{(L)} = W^{(L)} a^{(L-1)} + b^{(L)}$, $\hat{y} = a^{(L)}$

**Output:** $\hat{y}$ is the network's prediction.

### Computing Loss

For a single training sample $(x_i, y_i)$:
$$L_i = L(y_i, \hat{y}_i)$$

For all $n$ training samples:
$$J = \frac{1}{n} \sum_{i=1}^{n} L_i + \lambda \|W\|_F^2$$

where $\|W\|_F$ is the Frobenius norm of all weights (regularization).

### Backward Pass (Backpropagation)

**Goal:** Compute gradients $\frac{\partial J}{\partial W^{(\ell)}}$ and $\frac{\partial J}{\partial b^{(\ell)}}$ for every layer.

**Method:** Use the **chain rule** of calculus, starting from the output and working backward.

Define the **error term** for layer $\ell$:
$$\delta^{(\ell)} = \frac{\partial L}{\partial z^{(\ell)}}$$

**Output layer ($\ell = L$):**
$$\delta^{(L)} = \frac{\partial L}{\partial z^{(L)}} = \frac{\partial L}{\partial \hat{y}} \cdot \sigma'(z^{(L)})$$

**Hidden layers ($\ell < L$):**
$$\delta^{(\ell)} = \left( (W^{(\ell+1)})^T \delta^{(\ell+1)} \right) \cdot \sigma'(z^{(\ell)})$$

where $\cdot$ denotes element-wise multiplication.

**Gradients:**
$$\frac{\partial J}{\partial W^{(\ell)}} = \frac{1}{n} \delta^{(\ell)} (a^{(\ell-1)})^T + \frac{\lambda}{n} W^{(\ell)}$$
$$\frac{\partial J}{\partial b^{(\ell)}} = \frac{1}{n} \delta^{(\ell)}$$

### Why "Backpropagation"?

The error signals $\delta^{(\ell)}$ flow **backward** from output to input, propagating responsibility for the error through each layer.

### Update Rule

Standard gradient descent:
$$W^{(\ell)} \leftarrow W^{(\ell)} - \eta \frac{\partial J}{\partial W^{(\ell)}}$$
$$b^{(\ell)} \leftarrow b^{(\ell)} - \eta \frac{\partial J}{\partial b^{(\ell)}}$$

Repeat over all training samples (or batches) until convergence.

---

## Summary

**Week 8 bridges two major paradigms:**

1. **Kernel Methods (SVM Extension):**
   - Soft-margin handles non-separable data via slack variables.
   - Dual problem uses only dot products → Kernel trick computes them implicitly.
   - Mercer's theorem validates kernels via PSD Gram matrices.
   - SVM is regularized hinge loss minimization.

2. **Neural Networks (Learned Feature Spaces):**
   - Instead of manually choosing a kernel, learn the transformation automatically.
   - Universal Approximation Theorem guarantees existence (not construction).
   - Backpropagation computes gradients via the chain rule.
   - Deep architectures learn hierarchical representations.

**Key Insight:** Both methods solve the same fundamental problem—finding non-linear decision boundaries—but with different approaches. Kernels are mathematically elegant but require human intuition. Neural networks automate this learning process.

---

## Key Takeaways

| Concept | Definition | Example |
|---------|-----------|---------|
| **Soft-Margin SVM** | Allow margin violations via slack variables $\xi_i$; trade off margin width vs training errors | Objective: $\min \frac{1}{2}\|w\|^2 + C \sum \xi_i$ |
| **Slack Variable** | $\xi_i$ measures degree of margin violation/misclassification for point $i$ | $\xi_i = 0$ (safe), $0 < \xi_i < 1$ (margin violation), $\xi_i \geq 1$ (misclassified) |
| **Parameter $C$** | Controls trade-off: high $C$ penalizes violations heavily (hard margin), low $C$ allows violations (soft margin) | Large $C$ → narrow margin, small training error; Small $C$ → wide margin, better generalization |
| **Hinge Loss** | $L_{\text{hinge}} = \max(0, 1 - y \hat{y})$; zero if confident and correct, linear penalty otherwise | SVM is equivalent to minimizing regularized hinge loss |
| **Dual SVM** | Reformulates optimization in terms of Lagrange multipliers $\alpha_i$ and dot products | Only support vectors have $\alpha_i > 0$; others can be deleted |
| **Weight in Dual** | $w = \sum_i \alpha_i y_i x_i$; weights are weighted sum of support vectors | Decision depends entirely on dot products with support vectors |
| **Kernel Trick** | Replace $x_i^T x_j$ with $k(x_i, x_j)$ to compute dot products in high-dimensional space without explicit transformation | Enables infinite-dimensional Gaussian kernel efficiently |
| **Kernel Function** | Shortcut for computing $\phi(x_i)^T \phi(x_j)$ without computing $\phi$ | Polynomial: $(1 + x^T z)^p$; Gaussian: $\exp(-\gamma \|x-z\|^2)$ |
| **Mercer's Theorem** | Kernel $k$ is valid iff Gram matrix $K_{ij} = k(x_i, x_j)$ is Positive Semidefinite (PSD) | Checks eigenvalues $\geq 0$ for validity |
| **Neural Network** | Composite function: alternating linear and non-linear mappings, layer by layer | $a^{(\ell)} = \sigma(W^{(\ell)} a^{(\ell-1)} + b^{(\ell)})$ |
| **Universal Approximation** | One-hidden-layer network with bounded, continuous, non-constant $\sigma$ can approximate any continuous function | Proves neural networks have unlimited expressivity (with enough neurons) |
| **Pre-activation** | Raw linear combination before activation: $z^{(\ell)} = W^{(\ell)} a^{(\ell-1)} + b^{(\ell)}$ | Used in backprop; derivative w.r.t. parameters |
| **Activation** | Output after applying activation function: $a^{(\ell)} = \sigma(z^{(\ell)})$ | Squishes pre-activation into bounded range |
| **Backpropagation** | Chain rule for computing gradients; error signals $\delta^{(\ell)}$ flow backward layer by layer | $\delta^{(\ell)} = (W^{(\ell+1)T} \delta^{(\ell+1)}) \cdot \sigma'(z^{(\ell)})$ |
| **Gradient Descent in NN** | Update weights iteratively: $W^{(\ell)} \leftarrow W^{(\ell)} - \eta \frac{\partial J}{\partial W^{(\ell)}}$ | Converges to local optimum; non-convex objective |

---

## References to Assignment Content

**Related Assignment Questions:**
- Q1: Definition of feedforward neural network
- Q2: Universal Approximation Theorem conditions on activation
- Q3: Soft-margin SVM objective (unconstrained)
- Q4: Kernel trick replaces dot products
- Q5: Polynomial kernel example
- Q6: Mercer's theorem (PSD condition)
- Q7: Slack variables measure margin violations
- Q8: Weight vector as weighted sum of support vectors
- Q9: Hinge loss formula
- Q10: Pre-activation notation in backprop

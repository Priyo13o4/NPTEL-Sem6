# Week 8: Assignment 8 — Mathematical Foundations of Machine Learning (CS02)

| Metric | Value |
|--------|-------|
| Due | 2026-03-18, 23:59 IST |
| Submitted | — |
| Score | — |

---

## Question Set 1: Neural Network Fundamentals

### Q1 (1 point)

**A feedforward neural network is described as:**

- [ ] A) A single linear map $w^T x$
- [x] B) A composite function with alternating linear and non-linear mappings
- [ ] C) A probabilistic graphical model with latent variables
- [ ] D) A nearest neighbour method

**Answer:** B

**Answer Explanation:**

This question asks for the **foundational definition** of what a feedforward neural network actually is.

**The Architecture:**

A feedforward neural network consists of **layers stacked sequentially**, where each layer performs two operations:

1. **Linear Transformation:** Apply weights and bias
   $$z^{(\ell)} = W^{(\ell)} a^{(\ell-1)} + b^{(\ell)}$$

2. **Non-Linear Transformation:** Apply an activation function
   $$a^{(\ell)} = \sigma(z^{(\ell)})$$

These two steps **alternate** layer by layer:
- Input $x$
- Layer 1: Linear ($z^{(1)}$) → Non-linear ($a^{(1)}$)
- Layer 2: Linear ($z^{(2)}$) → Non-linear ($a^{(2)}$)
- ... (continue)
- Output $\hat{y}$

**Why Both Operations Matter:**
- **Linear alone:** Cannot learn non-linear patterns. A composition of linear functions is still linear (collapses to regression).
- **Non-linear alone:** Cannot combine features effectively; just element-wise transformations.
- **Together:** Can approximate any continuous function (Universal Approximation Theorem).

**Example (2-layer network):**
$$\hat{y} = \sigma^{(2)}(W^{(2)} \sigma^{(1)}(W^{(1)} x + b^{(1)}) + b^{(2)})$$

Read this carefully: input $x$ → linear → sigmoid → linear → sigmoid → output. **Alternating layers.**

**Analysis of Incorrect Options:**

- **A)** A single linear map is just logistic or linear regression. No depth, no compositionality.
- **C)** Probabilistic graphical models (Bayesian Networks, HMMs) are different architectures. Neural networks can be made probabilistic but aren't inherently graphical models.
- **D)** Nearest neighbour (k-NN) is an instance-based method. Neural networks are parametric and learn weights.

**Related Topics:** [Introduction to Neural Networks](notes.md#introduction-to-neural-networks)

---

### Q2 (1 point)

**The Universal Approximation Theorem statement requires the activation $\sigma$ to be:**

- [ ] A) Only linear
- [ ] B) Unbounded and discontinuous
- [x] C) Bounded, continuous, and non-constant
- [ ] D) A probability density function

**Answer:** C

**Answer Explanation:**

The **Universal Approximation Theorem** (Cybenko, Hornik) guarantees that a one-hidden-layer neural network can approximate any continuous function—but only under specific conditions on the activation function.

**The Three Conditions on $\sigma$:**

**1. Bounded:**

The activation function must not go to infinity. Formally:
$$\exists M > 0: |\sigma(z)| \leq M \, \forall z \in \mathbb{R}$$

**Examples of bounded:** Sigmoid ($\sigma(z) \in (0,1)$), Tanh ($\sigma(z) \in (-1,1)$), ReLU ($\sigma(z) \geq 0$ but unbounded above—actually semi-bounded).

**Why needed:** Unbounded activations can cause instability. The approximation theory requires the function to be "controlled."

**2. Continuous:**

The activation function must not have jumps or breaks:
$$\lim_{z \to z_0} \sigma(z) = \sigma(z_0)$$

**Examples of continuous:** Sigmoid, Tanh, ReLU (continuous everywhere except at 0, but generally acceptable).

**Examples of discontinuous:** Step function, integer rounding.

**Why needed:** Continuity ensures small input changes cause small output changes. This allows smooth approximation of functions.

**3. Non-Constant:**

The activation function must not be a flat line everywhere:
$$\sigma(z_1) \neq \sigma(z_2) \, \text{for some } z_1 \neq z_2$$

**Examples of non-constant:** Sigmoid, Tanh, ReLU.

**Examples of constant:** $\sigma(z) = 5$ (always returns 5).

**Why needed:** If $\sigma$ is constant, then all outputs are identical regardless of input. The network becomes useless.

### Why Linear Fails

If $\sigma(z) = z$ (linear), then composing layers yields:
$$\hat{y} = W^{(2)} W^{(1)} x + (W^{(2)} b^{(1)} + b^{(2)}) = \tilde{W} x + \tilde{b}$$

This is **still linear**. A composition of linear functions is linear. The network cannot approximate non-linear functions!

### The Power of Non-Linearity

With sigmoid, tanh, or ReLU, the network gains universal approximation power. Any smooth function in any dimension can be approximated with enough hidden units.

**Analysis of Incorrect Options:**

- **A)** Linear doesn't work (explained above). Universal approximation requires non-linearity.
- **B)** Unbounded and discontinuous violates all three conditions. Would break the approximation theory.
- **D)** Being a PDF is not required. The activation doesn't need to integrate to 1 or be interpretable as a probability.

**Related Topics:** [The Universal Approximation Theorem](notes.md#the-universal-approximation-theorem)

---

## Question Set 2: Soft-Margin SVM and Hinge Loss

### Q3 (1 point)

**The unconstrained objective equivalent to soft-margin SVM is:**

- [ ] A) $\min_{w,b} \|w\|_2^2 + C \sum_i \|x_i\|_2^2$
- [x] B) $\min_{w,b} \|w\|_2^2 + C \sum_i \max(0, 1 - y_i(w^\top x_i + b))$
- [ ] C) $\min_{w,b} \sum_i (y_i - w^\top x_i)^2$
- [ ] D) $\min_{w,b} \sum_i \mathbf{1}[y_i \neq \text{sign}(w^\top x_i + b)]$

**Answer:** B

**Answer Explanation:**

This is the key connection: **Soft-Margin SVM** (from Week 7's constrained formulation) is **mathematically identical** to regularized hinge loss minimization.

**The Soft-Margin SVM Primal (Constrained):**

From Week 7:
$$\min_{w, b, \xi} \frac{1}{2}\|w\|_2^2 + C \sum_i \xi_i$$

subject to: $y_i(w^T x_i + b) \geq 1 - \xi_i, \quad \xi_i \geq 0$

Using Lagrangian duality, this is **exactly equivalent** to the unconstrained objective in option B.

**Decomposing Option B:**

$$\min_{w,b} \underbrace{\|w\|_2^2}_{\text{Regularization}} + C \underbrace{\sum_i \max(0, 1 - y_i(w^\top x_i + b))}_{\text{Hinge Loss}}$$

**First term** $\|w\|_2^2$: L2 regularization. Penalizes large weights to keep the margin wide.

**Second term** $C \sum_i \max(0, 1 - y_i(w^\top x_i + b))$: Weighted hinge loss.

**The Hinge Loss Explained:**

For a single sample, the hinge loss is:
$$L_{\text{hinge}} = \max(0, 1 - y_i (w^T x_i + b))$$

**Behavior:**
- If $y_i(w^T x_i + b) \geq 1$ (confident and correct): Loss = $\max(0, \text{negative}) = 0$.
- If $0 < y_i(w^T x_i + b) < 1$ (margin violation): Loss = $1 - y_i(w^T x_i + b) > 0$.
- If $y_i(w^T x_i + b) \leq 0$ (misclassified): Loss = large positive value.

**Parameter $C$:**
- Large $C$: Heavy weight on violations → tight margin, fewer training errors (overfit).
- Small $C$: Lenient on violations → wider margin, generalize better (underfit).

**Relationship to Slack Variables:**

In the constrained form, $\xi_i = \max(0, 1 - y_i(w^T x_i + b))$. The hinge loss IS the slack variable!

**Analysis of Incorrect Options:**

- **A)** $\sum_i \|x_i\|_2^2$ penalizes the data (doesn't make sense). Should penalize weights.
- **C)** Squared error loss is for regression, not classification. Also not SVM.
- **D)** 0-1 loss (misclassification count) is discontinuous and can't be minimized with gradient descent. Hinge loss is a convex upper bound.

**Key Insight:** SVM is not a unique algorithm; it's just a specific choice of loss function (hinge) with L2 regularization. Logistic regression uses cross-entropy loss instead.

**Related Topics:** [SVM as Empirical Risk Minimization](notes.md#svm-as-empirical-risk-minimization)

---

## Question Set 3: The Kernel Trick

### Q4 (1 point)

**After kernelization, the dual objective (conceptually) replaces $x_i^\top x_j$ by:**

- [ ] A) $\phi(x_i)$
- [ ] B) $\phi(x_j)$
- [x] C) $k(x_i, x_j)$
- [ ] D) $\|x_i - x_j\|$

**Answer:** C

**Answer Explanation:**

The **Kernel Trick** is one of machine learning's most elegant ideas. It solves the problem of implicit feature expansion without explicitly computing high-dimensional features.

**The Problem We're Solving:**

Suppose we want to map data to a high-dimensional space:
$$\phi: \mathbb{R}^d \rightarrow \mathbb{R}^m, \quad m \gg d$$

The SVM dual involves dot products in this expanded space:
$$\phi(x_i)^T \phi(x_j)$$

**Issue:** Computing $\phi(x)$ for millions of high-dimensional vectors is expensive. For some kernels (Gaussian), $\phi$ maps to infinite dimensions!

**The Solution: The Kernel Function**

A **kernel function** is a shortcut that computes:
$$k(x_i, x_j) = \phi(x_i)^T \phi(x_j)$$

without ever computing $\phi$ explicitly!

**How It Works:**

Instead of:
1. Compute $\phi(x_i)$ (expensive, high-dimensional)
2. Compute $\phi(x_j)$ (expensive, high-dimensional)
3. Compute dot product (expensive)

We directly compute $k(x_i, x_j)$ using a formula (fast!).

**Example: Polynomial Kernel**

Kernel function: $k(x, z) = (1 + x^T z)^2$

Equivalent transformation: $\phi(x) = [1, x_1, x_2, \ldots, x_d, x_1^2, x_1 x_2, \ldots, x_d^2]$ (degree-2 polynomial features).

Verify:
$$(1 + x^T z)^2 = 1 + 2x^T z + (x^T z)^2$$

This expands to include all degree-2 terms, matching $\phi(x)^T \phi(z)$.

**The Modified Dual Objective:**

Standard dual (no kernel):
$$\max_\alpha \sum_i \alpha_i - \frac{1}{2} \sum_{i,j} \alpha_i \alpha_j y_i y_j (x_i^T x_j)$$

With kernel trick:
$$\max_\alpha \sum_i \alpha_i - \frac{1}{2} \sum_{i,j} \alpha_i \alpha_j y_i y_j k(x_i, x_j)$$

**Just replace $x_i^T x_j$ with $k(x_i, x_j)$!**

**Decision Rule:**

Original: $f(x) = \sum_i \alpha_i y_i (x_i^T x) + b$

With kernel: $f(x) = \sum_i \alpha_i y_i k(x_i, x) + b$

**Benefits:**
1. **Efficiency:** Compute $k(x_i, x_j)$ directly (often a simple formula).
2. **Expressivity:** Handle infinite-dimensional spaces (Gaussian kernel).
3. **Elegance:** No explicit $\phi$ computation needed.

**Analysis of Incorrect Options:**

- **A) $\phi(x_i)$:** This is the expanded features of a single point, not a dot product. Not a scalar.
- **B) $\phi(x_j)$:** Same issue; singular feature vector, not a dot product.
- **D) $\|x_i - x_j\|$:** This is a distance, not a dot product. Completely different concept.

**Related Topics:** [The Kernel Trick: Mathematical Magic](notes.md#the-kernel-trick-mathematical-magic)

---

### Q5 (1 point)

**Which of the following is an example of a polynomial kernel?**

- [ ] A) $k(x, z) = \exp(-\gamma\|x - z\|^2)$
- [ ] B) $k(x, z) = \tanh(ax^T z + b)$
- [x] C) $k(x, z) = (1 + x^\top z)^p$
- [ ] D) $k(x, z) = \|x\| + \|z\|$

**Answer:** C

**Answer Explanation:**

The **Polynomial Kernel** is one of the most common kernel functions. It creates **polynomial decision boundaries** of degree up to $p$.

**The Formula:**
$$k(x, z) = (1 + x^T z)^p$$

where $p \geq 1$ is the polynomial degree.

**What This Does:**

This kernel implicitly maps data to a space containing all **polynomial features up to degree $p$**.

**Example: $p = 2$, $d = 2$ dimensions**

Original data: $x = [x_1, x_2]$

Implicit expansion (roughly): $\phi(x) = [1, x_1, x_2, x_1^2, x_1 x_2, x_2^2, \ldots]$

The kernel computes:
$$(1 + x_1 z_1 + x_2 z_2)^2 = 1 + 2x_1 z_1 + 2x_2 z_2 + (x_1 z_1)^2 + 2x_1 x_2 z_1 z_2 + (x_2 z_2)^2$$

This matches $\phi(x)^T \phi(z)$ for the polynomial feature space!

**Decision Boundary Shape:**

- $p = 1$: Linear boundaries (straight lines/hyperplanes).
- $p = 2$: Quadratic boundaries (parabolas, circles, ellipses).
- $p = 3$: Cubic boundaries (wavy curves).
- Higher $p$: More complex polynomial curves.

**Why Add the 1?**

The $(1 + x^T z)$ instead of just $(x^T z)^p$ includes **constant and linear terms** in the expansion, which is necessary for a complete polynomial feature space.

**Analysis of Incorrect Options:**

- **A) $\exp(-\gamma\|x - z\|^2)$:** **Gaussian (RBF) Kernel**. Maps to infinite dimensions. Not polynomial.
- **B) $\tanh(ax^T z + b)$:** **Sigmoid Kernel**. Inspired by neural networks. Not polynomial (not even always PSD).
- **D) $\|x\| + \|z\|$:** This is the sum of norms (distances). Not a valid kernel (not PSD). Doesn't compute a dot product.

**When to Use Polynomial Kernel:**

- Data has polynomial relationships (e.g., $y \approx x^2$).
- Want interpretability (features are explicit polynomials).
- Moderate dimensionality needed.

**Related Topics:** [Common Kernel Functions](notes.md#common-kernel-functions)

---

### Q6 (1 point)

**According to the Mercer-style validity condition, a kernel $k$ is valid if its Gram matrix $K$ with entries $K_{ij} = k(x_i, x_j)$ is:**

- [ ] A) Diagonal
- [ ] B) Invertible for all datasets
- [x] C) Positive semidefinite (PSD)
- [ ] D) Orthogonal

**Answer:** C

**Answer Explanation:**

**Mercer's Theorem** is the mathematical gatekeeper for valid kernel functions. Not every function can be a valid kernel; it must satisfy the PSD condition.

**The Theorem:**

A function $k: \mathbb{R}^d \times \mathbb{R}^d \rightarrow \mathbb{R}$ is a valid kernel (i.e., corresponds to some transformation $\phi$ where $k(x, z) = \phi(x)^T \phi(z)$) if and only if:

**For any finite set of points $\{x_1, \ldots, x_n\}$, the Gram matrix $K$ with $K_{ij} = k(x_i, x_j)$ is Positive Semidefinite (PSD).**

### Understanding Positive Semidefiniteness

A matrix $K \in \mathbb{R}^{n \times n}$ is **Positive Semidefinite (PSD)** if:

$$v^T K v \geq 0 \quad \forall v \in \mathbb{R}^n$$

**Equivalent condition:** All eigenvalues are non-negative: $\lambda_i \geq 0 \, \forall i$.

### Why PSD Matters

The Gram matrix encodes pairwise similarities between all training points. PSD ensures the "space" is geometrically valid:

- No contradictions (like negative distances).
- Can be factored as $K = \Phi^T \Phi$ where $\Phi = [\phi(x_1)^T; \phi(x_2)^T; \ldots; \phi(x_n)^T]$.
- Guarantees a valid feature space $\phi$ exists.

### How to Check

For a proposed kernel $k$:

1. Generate Gram matrix $K_{ij} = k(x_i, x_j)$ on your data.
2. Compute eigenvalues of $K$.
3. If all eigenvalues $\geq 0$ (up to numerical precision): Valid kernel.
4. If any eigenvalue $< 0$: Not a valid kernel.

**Example: Polynomial Kernel**

$k(x, z) = (1 + x^T z)^p$ with $p \geq 1$ is **always PSD** for any data. Mathematically proven.

**Example: Gaussian Kernel**

$k(x, z) = \exp(-\gamma \|x - z\|^2)$ with $\gamma > 0$ is **always PSD**. No matter what data you feed it, Gram matrix is always PSD.

**Example: Sigmoid Kernel**

$k(x, z) = \tanh(ax^T z + b)$ is **not always PSD**. Depending on $a, b$, and the data, the Gram matrix might have negative eigenvalues. Must validate before use!

**Analysis of Incorrect Options:**

- **A) Diagonal:** Some kernels give diagonal Gram matrices (trivial), but most don't. This is too restrictive.
- **B) Invertible:** Invertibility requires all eigenvalues $\neq 0$. But PSD allows eigenvalues $= 0$ (singular, non-invertible). A valid kernel can be singular!
- **D) Orthogonal:** Orthogonal matrices are very special (preserves angles). Kernels don't need to be orthogonal.

**Key Insight:** Mercer's Theorem is the mathematical seal of approval. It certifies that a proposed kernel corresponds to a real feature space and won't cause paradoxes.

**Related Topics:** [Mercer's Theorem and Valid Kernels](notes.md#mercers-theorem-and-valid-kernels)

---

## Question Set 4: Slack Variables and SVM Formulation

### Q7 (1 point)

**In the primal soft-margin formulation, the slack variable $\xi_i$ measures:**

- [ ] A) The distance of $x_i$ from the origin
- [ ] B) The margin width $2/\|w\|$
- [x] C) The degree of margin violation / misclassification
- [ ] D) The probability that $y_i = 1$

**Answer:** C

**Answer Explanation:**

The **slack variable** $\xi_i$ (pronounced "xi") is a key concept in soft-margin SVM. It quantifies how much a single data point violates the margin.

### The Margin Constraint (Hard-Margin)

In Week 7's hard-margin SVM, every point must be at distance at least 1 from the boundary:
$$y_i(w^T x_i + b) \geq 1$$

**The Problem:** If data is non-separable (one red apple in green field), no solution exists. The problem is infeasible.

### The Solution: Slack Variables

We relax the constraint to allow violations:
$$y_i(w^T x_i + b) \geq 1 - \xi_i, \quad \xi_i \geq 0$$

Now, $\xi_i$ measures **how much** the constraint is violated.

### Interpretation of $\xi_i$

| Value | Meaning | Geometric Location |
|-------|---------|-------------------|
| $\xi_i = 0$ | Constraint satisfied | Point is on correct side, beyond margin (distance ≥ 1) |
| $0 < \xi_i < 1$ | Margin violation | Point is inside the margin but correctly classified |
| $\xi_i = 1$ | On decision boundary | Point touches the boundary exactly |
| $\xi_i > 1$ | Misclassified | Point is on the wrong side of the boundary |

### Example

**Point A:** Correctly classified, far from boundary. $y_i(w^T x_i + b) = 2.5$. Then $\xi_i = \max(0, 1 - 2.5) = 0$.

**Point B:** Correctly classified, barely inside margin. $y_i(w^T x_i + b) = 0.7$. Then $\xi_i = \max(0, 1 - 0.7) = 0.3$.

**Point C:** Misclassified, on wrong side. $y_i(w^T x_i + b) = -0.5$. Then $\xi_i = \max(0, 1 - (-0.5)) = 1.5$.

### Slack in the Objective

The soft-margin objective:
$$\min_{w, b, \xi} \frac{1}{2}\|w\|^2 + C \sum_i \xi_i$$

subject to constraints using $\xi_i$.

**The $C$ parameter:**
- Large $C$: $\xi_i$ terms have huge cost. Model tries hard to make $\xi_i$ small → fewer violations.
- Small $C$: $\xi_i$ terms are cheap. Model allows violations for wider margin → more robust.

### Connection to Hinge Loss

Recall from Q3: the objective is equivalent to minimizing hinge loss:
$$\min_{w,b} \|w\|^2 + C \sum_i \max(0, 1 - y_i(w^T x_i + b))$$

The hinge loss $\max(0, 1 - y_i(w^T x_i + b))$ **is exactly** the slack variable:
$$\xi_i = \max(0, 1 - y_i(w^T x_i + b))$$

**Analysis of Incorrect Options:**

- **A)** Distance from origin is a separate quantity, not related to margin violation. $\xi_i$ is specific to each point's violation.
- **B)** The margin width is $2/\|w\|$ globally. $\xi_i$ is point-specific, not the margin width.
- **D)** Probability that $y_i = 1$ is a data property, not something the slack variable represents.

**Key Insight:** Slack variables make SVM practical. They allow the algorithm to gracefully handle real, messy data instead of requiring perfect separability.

**Related Topics:** [Soft-Margin SVM and Slack Variables](notes.md#soft-margin-svm-and-slack-variables)

---

### Q8 (1 point)

**In the SVM dual formulation, the learned weight vector can be expressed as:**

- [ ] A) $w = \sum_i x_i$
- [ ] B) $w = \sum_i \alpha_i x_i$
- [x] C) $w = \sum_i \alpha_i y_i x_i$
- [ ] D) $w = \sum_i y_i x_i$

**Answer:** C

**Answer Explanation:**

This is a fundamental insight: the learned weights in an SVM are a **weighted sum of the training data**, where the weights are determined by the Lagrange multipliers and labels.

### Derivation from Dual

In the SVM dual problem, the optimal weights are derived via the Lagrangian optimality condition:

$$\frac{\partial L}{\partial w} = 0 \quad \Rightarrow \quad w = \sum_{i=1}^{n} \alpha_i y_i x_i$$

where $\alpha_i$ are the Lagrange multipliers (dual variables) and $y_i \in \{-1, +1\}$ are the labels.

### Interpretation

**Each training point $x_i$ contributes to the weights** with:
- **Magnitude:** Determined by $\alpha_i$ (importance of that point).
- **Direction:** Controlled by $y_i$ (flip direction if label is -1).

### Sparsity: The Key Property

In the optimal dual solution:
- **Most $\alpha_i = 0$:** These points don't contribute to $w$ at all. They can be deleted without changing the solution.
- **Few $\alpha_i > 0$:** These are the **support vectors** (points on or violating the margin). They alone determine the boundary.

**Example:**

For a dataset with 1,000 points, maybe only 30 have $\alpha_i > 0$. The boundary is entirely determined by those 30 support vectors. The other 970 points are irrelevant!

### Decision Rule

For a new test point $x$:

$$f(x) = \sum_{i=1}^{n} \alpha_i y_i (x_i^T x) + b$$

**Notice:** Only the **support vectors** (where $\alpha_i > 0$) contribute to the decision.

### Connection to Kernel Trick

With kernelization, replace $x_i^T x$ with $k(x_i, x)$:

$$f(x) = \sum_{i: \alpha_i > 0} \alpha_i y_i k(x_i, x) + b$$

This is incredibly efficient: compute kernel similarities only with support vectors!

### Why Each Option is Wrong or Right

- **A) $w = \sum_i x_i$:** Ignores $\alpha_i$ and $y_i$. Points are equally weighted and wrongly signed.
- **B) $w = \sum_i \alpha_i x_i$:** Misses the label factor $y_i$. Without it, directions aren't flipped for negative labels.
- **C) $w = \sum_i \alpha_i y_i x_i$:** ✓ Correct. Includes both Lagrange multipliers and label signs.
- **D) $w = \sum_i y_i x_i$:** Ignores $\alpha_i$. All points equally important; should be weighted by dual variables.

**Key Insight:** This formula reveals SVM's elegance. The boundary is not some abstract mathematical object; it's a linear combination of training points themselves!

**Related Topics:** [The Dual Problem Revisited](notes.md#the-dual-problem-revisited)

---

## Question Set 5: Hinge Loss and Backpropagation

### Q9 (1 point)

**The hinge loss for a single example $(x_i, y_i)$ with score $h(x_i) = w^\top x_i + b$ is:**

- [ ] A) $(y_i - h(x_i))^2$
- [ ] B) $\log(1 + \exp(-y_i h(x_i)))$
- [x] C) $\max(0, 1 - y_i h(x_i))$
- [ ] D) $-y_i h(x_i)$

**Answer:** C

**Answer Explanation:**

**Hinge Loss** is the signature loss function for Support Vector Machines. It's piecewise linear and geometrically intuitive.

### The Formula

$$L_{\text{hinge}}(y_i, h(x_i)) = \max(0, 1 - y_i h(x_i))$$

where:
- $y_i \in \{-1, +1\}$ is the true label.
- $h(x_i) = w^T x_i + b$ is the raw score (pre-activation).

### Interpretation

The term $y_i h(x_i)$ measures **agreement between prediction and label**:
- If $y_i h(x_i) > 0$: Prediction and label agree in sign (correct direction).
- If $y_i h(x_i) < 0$: Opposite signs (wrong direction).
- If $y_i h(x_i) \geq 1$: Confident and correct (score is far on the right side).

### Loss Behavior

| Condition | $y_i h(x_i)$ | Loss Value | Meaning |
|-----------|-------------|-----------|---------|
| Confident, correct | $\geq 1$ | $0$ | No penalty; safe |
| Uncertain, correct | $0$ to $1$ | $1 - y_i h(x_i)$ | Linear penalty; grows |
| On boundary | $0$ | $1$ | Maximum penalty for zero score |
| Misclassified | $\leq 0$ | $\geq 1$ | Large penalty; pushed hard |

### Visual Intuition (1D)

```
Loss
  |
5 |                /
  |              /
3 |            /
1 |          /
  |_________/___________
  0   1         y_i h(x_i)
```

- Flat at 0 for $y_i h(x_i) \geq 1$ (nothing to do).
- Linear increase for $y_i h(x_i) < 1$ (push harder the more wrong).

### Why Not Squared Error?

Squared error loss $(y_i - h(x_i))^2$:
- Is smooth and differentiable everywhere.
- Penalizes confident-correct predictions (overfit).
- Overly harsh on outliers (large errors get huge penalties).

Hinge loss is better for classification:
- Doesn't waste gradient once you're confident and correct ($y_i h(x_i) \geq 1$).
- Ignores easy examples; focuses on hard ones.
- Naturally creates a margin.

### SVM Objective with Hinge Loss

The soft-margin SVM is:
$$\min_{w,b} \|w\|^2 + C \sum_i \max(0, 1 - y_i(w^T x_i + b))$$

This is **regularized hinge loss minimization**: the $\|w\|^2$ term adds regularization.

### Geometric Connection to Margin

The margin in SVM is the distance where $y_i h(x_i) = 1$. Points satisfying this are support vectors. Hinge loss is zero exactly when a point is on or beyond the margin. This directly encodes the margin maximization into the loss!

**Analysis of Incorrect Options:**

- **A) $(y_i - h(x_i))^2$:** Squared error. Used for regression, not classification. Different geometry.
- **B) $\log(1 + \exp(-y_i h(x_i)))$:** **Logistic loss** (used in logistic regression). Smooth but different from hinge.
- **D) $-y_i h(x_i)$:** Negative score. No margin concept, unbounded below.

**Key Insight:** Hinge loss is the bridge between SVM's geometric margin-maximization perspective and the practical ERM optimization framework.

**Related Topics:** [SVM as Empirical Risk Minimization](notes.md#svm-as-empirical-risk-minimization)

---

### Q10 (1 point)

**In the backprop notation, the pre-activation of neuron $j$ in layer $\ell$ is typically denoted by:**

- [ ] A) $a_j^{(\ell)}$
- [x] B) $z_j^{(\ell)}$
- [ ] C) $w_{jk}^{(\ell)}$
- [ ] D) $\delta_j^{(\ell)}$

**Answer:** B

**Answer Explanation:**

This is **standard neural network notation** used everywhere (textbooks, papers, code). Understanding the notation is crucial for reading and implementing backpropagation.

### Neural Network Notation

For a layer $\ell$ with neurons indexed by $j$ and previous layer neurons indexed by $k$:

**Forward Pass (per neuron):**

1. **Pre-activation (raw linear sum):** 
   $$z_j^{(\ell)} = \sum_{k} w_{jk}^{(\ell)} a_k^{(\ell-1)} + b_j^{(\ell)}$$
   
   This is the **weighted sum before activation**. Sometimes called **logit** or **net input**.

2. **Activation (after applying function):**
   $$a_j^{(\ell)} = \sigma(z_j^{(\ell)})$$
   
   This is the **output after activation function**. Also called **hidden unit** or **feature**.

**Example (2-layer network):**
- Input: $x \in \mathbb{R}^{d_0}$ (which is $a^{(0)}$)
- Layer 1: $z^{(1)} = W^{(1)} x + b^{(1)}$, then $a^{(1)} = \sigma(z^{(1)})$
- Layer 2: $z^{(2)} = W^{(2)} a^{(1)} + b^{(2)}$, then $a^{(2)} = \sigma(z^{(2)})$ (output $\hat{y}$)

### Why Separate $z$ and $a$?

**Backpropagation** uses both:
- **$z$:** To compute derivatives of activation function.
- **$a$:** To propagate errors to previous layer.

The chain rule gives:
$$\frac{\partial L}{\partial z_j^{(\ell)}} = \frac{\partial L}{\partial a_j^{(\ell)}} \cdot \frac{\partial a_j^{(\ell)}}{\partial z_j^{(\ell)}} = \delta_j^{(\ell)} \cdot \sigma'(z_j^{(\ell)})$$

Without separating $z$ and $a$, this is confusing.

### Error Term $\delta_j^{(\ell)}$

The **error signal** (used in backprop) is:
$$\delta_j^{(\ell)} = \frac{\partial L}{\partial z_j^{(\ell)}}$$

This measures how much neuron $j$ in layer $\ell$ contributed to the loss. It flows backward through the network.

### Weight Notation

- $w_{jk}^{(\ell)}$: Weight from neuron $k$ in layer $\ell-1$ to neuron $j$ in layer $\ell$.
- $b_j^{(\ell)}$: Bias of neuron $j$ in layer $\ell$.

### Summary Table

| Symbol | What It Is | When Used |
|--------|-----------|-----------|
| $z_j^{(\ell)}$ | Pre-activation (raw linear sum) | Computing activation, backprop derivative of $\sigma$ |
| $a_j^{(\ell)}$ | Activation (after $\sigma$ function) | Input to next layer, backprop chain rule |
| $w_{jk}^{(\ell)}$ | Weight from layer $\ell-1$ to layer $\ell$ | Forward pass, gradient computation |
| $b_j^{(\ell)}$ | Bias for neuron $j$ in layer $\ell$ | Forward pass, gradient computation |
| $\delta_j^{(\ell)}$ | Error signal for neuron $j$ | Backpropagation, gradient accumulation |
| $L$ | Scalar loss | Optimization objective |

**Analysis of Incorrect Options:**

- **A) $a_j^{(\ell)}$:** This is the **activation** (after sigmoid/ReLU), not pre-activation. It's the output of neuron $j$.
- **C) $w_{jk}^{(\ell)}$:** These are the **weights**, not neuron activations. Used to compute $z$.
- **D) $\delta_j^{(\ell)}$:** This is the **error term**, derivative of loss w.r.t. pre-activation. It's used in backprop but not the pre-activation itself.

**Key Insight:** Standard notation is critical for understanding papers and code. Learning it now pays dividends when studying modern deep learning.

**Related Topics:** [Forward and Backward Propagation](notes.md#forward-and-backward-propagation)

---

## Answer Key Summary

| Q | Answer | Concept | Difficulty |
|---|--------|---------|-----------|
| Q1 | B | Definition of feedforward NN | Easy |
| Q2 | C | Universal Approximation Theorem conditions | Easy |
| Q3 | B | Soft-margin SVM objective (hinge loss) | Medium |
| Q4 | C | Kernel trick replaces dot products | Easy |
| Q5 | C | Polynomial kernel example | Easy |
| Q6 | C | Mercer's theorem (PSD condition) | Medium |
| Q7 | C | Slack variables measure margin violations | Easy |
| Q8 | C | Weight vector as support vector combination | Medium |
| Q9 | C | Hinge loss formula | Easy |
| Q10 | B | Pre-activation notation | Easy |

**Overall Score:** 10/10 points possible

---

## Concept Map: Week 8 Progression

**From Geometry to Optimization to Learning:**

1. **Soft-Margin SVM (Week 7 → Week 8):**
   - Week 7 assumed perfect separability.
   - Week 8 introduces slack variables for messy data.
   - Hinge loss interpretation: loss is zero when confident and correct.

2. **Dual Problem and Support Vectors:**
   - Primal problem: Optimize weights directly (expensive in high dimensions).
   - Dual problem: Optimize in terms of dot products between training points.
   - Sparsity: Only support vectors (where $\alpha_i > 0$) matter.

3. **The Kernel Trick (SVM's Superpower):**
   - Maps to high-dimensional spaces without explicit computation.
   - Kernel functions replace dot products with shortcuts.
   - Mercer's theorem validates kernels (must have PSD Gram matrix).
   - Enables infinite-dimensional spaces (Gaussian kernel).

4. **Neural Networks (Human-Designed Features → Learned Features):**
   - SVM requires humans to choose the kernel (feature space).
   - Neural networks learn the feature transformation automatically.
   - Composite function: alternating linear and non-linear layers.

5. **Universal Approximation (Existence Theorem):**
   - Proves neural networks can approximate any continuous function.
   - Requires activation to be bounded, continuous, non-constant.
   - Doesn't tell us how to find the weights (that's what backprop does).

6. **Backpropagation (The Learning Algorithm):**
   - Forward pass: compute $z$ and $a$ for all layers.
   - Backward pass: error signals $\delta$ flow backward.
   - Chain rule computes gradients layer by layer.
   - Gradient descent updates weights iteratively.

**Week 8 Message:** We've now covered the major classical ML paradigms (SVM with kernels) and the modern dominant paradigm (neural networks). Both solve the same problem—finding non-linear decision boundaries—but with different trade-offs. Next weeks likely extend neural networks to deeper architectures and practical applications.

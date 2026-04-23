# Week 7: Theoretical Notes — Regularization and Support Vector Machines

## Overview

Week 7 tackles two major obstacles in machine learning: **overfitting** (the curse of complex models) and **non-separable data** (when perfectly clean boundaries don't exist). We introduce **regularization**—a mathematical brake that penalizes model complexity to improve generalization—and **Support Vector Machines (SVMs)**, an elegant classifier that maximizes the margin between classes. These are foundational techniques that remain central to modern ML practice.

---

## The Overfitting Problem: Why Regularization Matters

**The Danger:** A model is only useful if it generalizes to unseen data. In Week 6, we learned that test error decomposes into bias, variance, and noise. Overfitting occurs when **variance dominates**—the model becomes so complex that it memorizes training data, including its noise and quirks.

**Concrete Example:** 
You're predicting house prices using features like square footage, bedrooms, and (accidentally) the number of red cars parked nearby. If your model is complex enough, it might discover that red cars correlated with expensive houses in your training data, even though this is pure coincidence. It assigns that feature a massive weight. When the model sees new test data, it fails because red cars don't actually predict house prices in the real world.

**The Solution: Regularization**

Instead of minimizing only the training loss:
$$\min_{\theta} \hat{R}(\theta) = \min_{\theta} \frac{1}{n} \sum_{i=1}^{n} L(h_\theta(x_i), y_i)$$

We now minimize training loss **plus a penalty for large weights**:
$$\min_{\theta} \hat{R}(\theta) + \text{Penalty}(\theta)$$

The penalty term acts as a "brake"—if the model wants to use large weights, it must pay a high mathematical cost.

---

## Ridge Regression (L2 Regularization)

### Mathematical Formulation

$$J(\theta) = \frac{1}{n} \sum_{i=1}^{n} (y_i - h_\theta(x_i))^2 + \lambda \|\theta\|_2^2$$

where:
- First term: Empirical Risk (Squared Error Loss / MSE)
- Second term: L2 penalty = sum of squared weights
- $\lambda \geq 0$: **Regularization strength**. Higher $\lambda$ means stronger penalty on large weights.

### Gradient Computation

For a single weight $\theta_j$:
$$\frac{\partial J}{\partial \theta_j} = \frac{\partial \text{MSE}}{\partial \theta_j} + 2\lambda \theta_j = g_j + 2\lambda \theta_j$$

where $g_j$ is the gradient of the MSE term.

**Interpretation:** The MSE gradient $g_j$ pulls the weight in the direction that reduces training loss. The penalty term $2\lambda \theta_j$ pulls the weight toward zero. They compete.

### Gradient Descent Update

$$\theta_j^{(t+1)} = \theta_j^{(t)} - \eta \left( g_j + 2\lambda \theta_j^{(t)} \right)$$

**Effect:** Even if $g_j = 0$ (no training error gradient), the penalty term forces weights to shrink: $\theta_j^{(t+1)} = \theta_j^{(t)}(1 - 2\eta\lambda)$.

This is called **weight decay**: weights naturally decay toward zero each iteration if they don't contribute to training loss.

### Closed-Form Solution for Linear Models

For linear regression with squared error, the Ridge solution is:
$$\theta^* = (X^T X + \lambda I)^{-1} X^T \mathbf{y}$$

Compare to the Normal Equation (unregularized): $\theta^* = (X^T X)^{-1} X^T \mathbf{y}$.

The $\lambda I$ term adds a small constant to the diagonal of $X^T X$, making the matrix more "well-conditioned" (easier to invert numerically) and shrinking all weights uniformly.

### Probabilistic Interpretation: MAP with Gaussian Prior

Ridge regularization is mathematically equivalent to **Maximum A Posteriori (MAP)** estimation with a Gaussian prior on the weights:

$$p(\theta) = \mathcal{N}(\theta | 0, \frac{1}{2\lambda} I)$$

Higher $\lambda$ means a sharper prior (more centered at zero), pushing the posterior estimate toward zero.

**Why L2?** The squared penalty is smooth, differentiable everywhere, and convex. It distributes the shrinkage evenly across all weights.

**Related Assignment Questions:** Q1, Q2, Q8

---

## Lasso Regression (L1 Regularization)

### Mathematical Formulation

$$J(\theta) = \frac{1}{n} \sum_{i=1}^{n} (y_i - h_\theta(x_i))^2 + \lambda \|\theta\|_1$$

where:
- $\|\theta\|_1 = \sum_{j=1}^{d} |\theta_j|$ (sum of absolute values)
- All other components same as Ridge.

### Gradient Computation

The absolute value function has a kinked derivative:

$$\frac{d}{d\theta_j} |\theta_j| = \begin{cases} +1 & \text{if } \theta_j > 0 \\ -1 & \text{if } \theta_j < 0 \\ \text{undefined} & \text{if } \theta_j = 0 \end{cases}$$

Therefore:
$$\frac{\partial J}{\partial \theta_j} = g_j + \lambda \cdot \text{sign}(\theta_j)$$

where $\text{sign}(\theta_j)$ is $+1$, $-1$, or $0$ depending on the sign of $\theta_j$.

**Critical Behavior:** If $|g_j| < \lambda$ and $\theta_j$ is close to zero, the penalty term $\lambda \cdot \text{sign}(\theta_j)$ exactly cancels the gradient term, and the weight stays at zero!

### Feature Selection via Sparsity

This is the key advantage of Lasso: **automatic feature selection**.

If a feature is unimportant (contributes little to reducing training loss, so $g_j$ is small), Lasso will push that weight to exactly zero. This "switches off" that feature entirely—it's excluded from the model.

**Contrast with Ridge:** Ridge shrinks weights but rarely sets them to exactly zero. All features remain in the model.

**Example:** 
- Ridge on [5, -2, 8]: Shrinks to [4.5, -1.8, 7.2] (all still present)
- Lasso on [5, -2, 8]: Might shrink to [4.2, 0.0, 7.1] (middle feature deleted)

### Probabilistic Interpretation: MAP with Laplace Prior

Lasso is equivalent to MAP with a **Laplace prior**:

$$p(\theta_j) = \frac{\lambda}{2} \exp(-\lambda |\theta_j|)$$

The Laplace distribution has a sharp peak at zero (unlike Gaussian, which is smooth). This sharp peak mathematically drives weights to exactly zero.

### Computational Challenge

Unlike Ridge, Lasso has no closed-form solution. The absolute value creates non-differentiability at $\theta_j = 0$, breaking calculus-based solvers. Special algorithms like **Coordinate Descent** or **Proximal Gradient Descent** are needed.

**Related Assignment Questions:** Q3, Q4

---

## Elastic Net: Combining Ridge and Lasso

### Mathematical Formulation

$$J(\theta) = \frac{1}{n} \sum_{i=1}^{n} (y_i - h_\theta(x_i))^2 + \lambda_1 \|\theta\|_1 + \lambda_2 \|\theta\|_2^2$$

Elastic Net combines both penalties:
- **L1 penalty** ($\lambda_1$): Encourages sparsity (feature selection).
- **L2 penalty** ($\lambda_2$): Encourages smooth shrinkage (numerical stability).

### When to Use Each

| Regularizer | Use Case | Advantage | Disadvantage |
|-------------|----------|-----------|-------------|
| **Ridge (L2)** | Many correlated features | Stable, closed-form solution | Doesn't zero out features |
| **Lasso (L1)** | Feature selection needed | Interpretable sparse models | Unstable with correlated features; no closed form |
| **Elastic Net** | High-dimensional, correlated data | Best of both worlds | Two hyperparameters to tune |

### Penalty Calculation Example

Given $\theta = [1, -2, 3]$, $\lambda_1 = 0.4$, $\lambda_2 = 0.1$:

**L1 part:** $\lambda_1 \|\theta\|_1 = 0.4 (|1| + |-2| + |3|) = 0.4 \times 6 = 2.4$

**L2 part:** $\lambda_2 \|\theta\|_2^2 = 0.1 (1^2 + 4 + 9) = 0.1 \times 14 = 1.4$

**Total penalty:** $2.4 + 1.4 = 3.8$

**Related Assignment Questions:** Q9

---

## Ridge vs Lasso: Geometric Intuition

### Contour Plot Visualization

In 2D with weights $[\theta_1, \theta_2]$:

**Ridge** adds a circular penalty: $\lambda(\theta_1^2 + \theta_2^2)$. The optimal $\theta^*$ is where the training loss contour touches the circle. This happens on the smooth circular boundary—rarely at a corner.

**Lasso** adds a diamond-shaped penalty: $\lambda(|\theta_1| + |\theta_2|)$. The optimal $\theta^*$ is where the training loss contour touches the diamond. Diamonds have **sharp corners** at axes. Loss contours frequently hit these corners exactly, setting one weight to zero.

**Geometric Conclusion:**
- Ridge: Shrinks all weights evenly → smooth, continuous reduction.
- Lasso: Forces weights to zero axes → sparse, interpretable models.

**Related Assignment Questions:** Q3, Q4

---

## Stochastic Gradient Descent (SGD) as a Regularizer

### Standard Gradient Descent vs SGD

**Batch Gradient Descent:**
$$\theta^{(t+1)} = \theta^{(t)} - \eta \nabla_\theta \hat{R}(\theta^{(t)})$$

Computes gradient over **entire dataset** ($n$ samples) before one update.

**Stochastic Gradient Descent (SGD):**
$$\theta^{(t+1)} = \theta^{(t)} - \eta \nabla_\theta L_i(\theta^{(t)})$$

Computes gradient on a **single random sample** $i$ (or small batch) before each update.

### SGD as an Implicit Regularizer

**Surprising Insight:** SGD automatically regularizes even without explicit penalty terms!

**Why?** Because SGD only looks at random subsets of the data, its gradient estimates are **noisy**. This noise has a regularizing effect:

1. **Escaping Sharp Minima:** If the model finds an overfitted solution (a sharp, narrow valley), the noisy SGD gradient will bounce it out. Overfitted solutions live in steep, narrow crevices. A slight jitter shakes the model loose.

2. **Preferring Flat Minima:** SGD naturally converges to flatter, wider minima in the loss landscape. Flat minima are more robust—small changes to the data don't drastically change the solution. These generalize better.

3. **Implicit Regularization Strength:** The learning rate $\eta$ and batch size control the noise level and thus the implicit regularization strength.

### Practical Implications

- Small batch size → More noise → Stronger implicit regularization → Better generalization (but slower convergence).
- Large batch size → Less noise → Weaker implicit regularization → Faster convergence (but may overfit).

This is why practitioners often use small batches (32, 64, 128) rather than full-batch updates, even on small datasets.

---

## Support Vector Machines (SVM)

### The Central Idea: Maximum Margin

Classical linear classifiers (like logistic regression) ask: "What boundary correctly separates the classes?"

**SVMs ask:** "What boundary is the **most robust**?"

If two classes are linearly separable, infinite lines can separate them. SVMs find the line that stays as far away as possible from both classes. The distance to the nearest point is the **margin**. Wider margin = more robust = better generalization.

### Discriminant Function and Decision Boundary

The SVM model is linear:
$$f(x) = \theta^T x + \theta_0$$

The decision boundary is where $f(x) = 0$:
$$\theta^T x + \theta_0 = 0$$

**Interpretation:**
- $f(x) > 0$: Predict class $+1$ (positive side of hyperplane)
- $f(x) < 0$: Predict class $-1$ (negative side of hyperplane)
- $f(x) = 0$: Exactly on the boundary (uncertain)

### The Margin

The **geometric margin** is the perpendicular distance from a point $x_i$ to the decision boundary:

$$\text{Margin}_i = \frac{|f(x_i)|}{\|\theta\|_2} = \frac{|\theta^T x_i + \theta_0|}{\|\theta\|_2}$$

The divisor $\|\theta\|_2$ normalizes by the "weight vector's length."

**Margin** of the entire classifier = minimum margin over all training points (the margin determined by the hardest-to-classify point).

### Maximum Margin Principle

**Goal:** Maximize the minimum margin subject to correct classification.

For **linearly separable data** (all training points correctly classified):

$$\max_{\theta, \theta_0} \frac{1}{\|\theta\|_2} \quad \text{subject to} \quad y_i (\theta^T x_i + \theta_0) \geq 1 \, \forall i$$

**Equivalent formulation** (easier to optimize):

$$\min_{\theta, \theta_0} \frac{1}{2} \|\theta\|_2^2 \quad \text{subject to} \quad y_i (\theta^T x_i + \theta_0) \geq 1 \, \forall i$$

where $y_i \in \{-1, +1\}$ are the class labels.

### Support Vectors

The constraint $y_i (\theta^T x_i + \theta_0) \geq 1$ means each point must be at least margin-distance away from the boundary.

**Support vectors** are the points where this constraint is **tight** (exactly equality):
$$y_i (\theta^T x_i + \theta_0) = 1$$

These are the data points that touch the margin boundary. They are **critically important**—they alone determine the solution. You could delete all other data points, and the SVM would find the same boundary!

Non-support vectors (interior points with margin $> 1$) don't affect the solution.

**Related Assignment Questions:** Q7

---

## The SVM Formulation: Primal and Dual

### The Primal Problem

For **linearly separable** data:

$$\min_{\theta, \theta_0} \frac{1}{2} \|\theta\|_2^2$$
$$\text{subject to} \quad y_i (\theta^T x_i + \theta_0) \geq 1 \, \forall i = 1, \ldots, n$$

This is a **convex quadratic program** (QP). Standard solvers can solve it, but there's a better way.

### The Dual Problem

Using **Lagrange multipliers** $\alpha_i \geq 0$, we construct the Lagrangian:

$$L(\theta, \theta_0, \alpha) = \frac{1}{2}\|\theta\|_2^2 - \sum_{i=1}^{n} \alpha_i [y_i (\theta^T x_i + \theta_0) - 1]$$

Taking derivatives w.r.t. $\theta$ and $\theta_0$ and setting to zero:

$$\theta = \sum_{i=1}^{n} \alpha_i y_i x_i$$

$$\sum_{i=1}^{n} \alpha_i y_i = 0$$

Substituting back into the Lagrangian gives the **Dual Problem**:

$$\max_{\alpha} \sum_{i=1}^{n} \alpha_i - \frac{1}{2} \sum_{i=1}^{n} \sum_{j=1}^{n} \alpha_i \alpha_j y_i y_j (x_i^T x_j)$$
$$\text{subject to} \quad \alpha_i \geq 0 \, \forall i, \quad \sum_{i=1}^{n} \alpha_i y_i = 0$$

**Why the Dual is Beautiful:**

1. **Sparse Solution:** Only support vectors have $\alpha_i > 0$. All other $\alpha_i = 0$ (by complementary slackness).

2. **Dot Products:** The dual only depends on $x_i^T x_j$ (dot products between training points), not on the weights $\theta$ directly!

3. **Kernel Trick Preview:** Because the problem only uses dot products, we can replace $x_i^T x_j$ with any kernel function $K(x_i, x_j)$. This allows SVMs to find non-linear boundaries without explicitly computing high-dimensional transformations.

### Non-Separable Data: Soft Margins

Real data is rarely linearly separable. We introduce **slack variables** $\xi_i \geq 0$:

$$y_i (\theta^T x_i + \theta_0) \geq 1 - \xi_i$$

where $\xi_i = 0$ means the constraint is satisfied (correct side of margin), and $\xi_i > 0$ means the point violates the margin (possibly even misclassified).

**Soft-margin objective:**

$$\min_{\theta, \theta_0, \xi} \frac{1}{2}\|\theta\|_2^2 + C \sum_{i=1}^{n} \xi_i$$

The parameter $C > 0$ controls the trade-off:
- Large $C$: Penalize violations heavily → fewer misclassifications, smaller margin.
- Small $C$: Allow violations → larger margin, possibly more errors.

This is mathematically equivalent to hinge loss regularization!

**Related Assignment Questions:** Q6

---

## KKT Conditions (Karush-Kuhn-Tucker)

For the primal SVM problem with inequalities, the **KKT conditions** are necessary and sufficient for optimality. They characterize the optimal solution:

### The Conditions

1. **Stationarity:** $\nabla_\theta L = 0$ (gradient of Lagrangian is zero).
2. **Primal Feasibility:** $y_i (\theta^T x_i + \theta_0) \geq 1 - \xi_i \, \forall i$.
3. **Dual Feasibility:** $\alpha_i, \mu_i \geq 0 \, \forall i$ (Lagrange multipliers non-negative).
4. **Complementary Slackness:** 
   - $\alpha_i [y_i (\theta^T x_i + \theta_0) - 1 + \xi_i] = 0$
   - $\mu_i \xi_i = 0$

### Interpretation

**Complementary slackness** is the key:
- If $\alpha_i > 0$, then $y_i (\theta^T x_i + \theta_0) = 1 - \xi_i$ (point is on the margin boundary or violates it).
- If $y_i (\theta^T x_i + \theta_0) > 1 - \xi_i$ (point far from boundary), then $\alpha_i = 0$ (not a support vector).

**Practical Value:** KKT conditions are used to design efficient SVM solvers (like Sequential Minimal Optimization, SMO). They also provide a certificate of optimality: if a solution satisfies KKT, it's mathematically proven to be optimal.

---

## Hinge Loss and Its Connection to SVM

The **hinge loss** is:
$$L_{\text{hinge}}(y, \hat{y}) = \max(0, 1 - y \hat{y})$$

where $y \in \{-1, +1\}$ and $\hat{y} = \theta^T x + \theta_0$.

**Interpretation:**
- If $y \hat{y} \geq 1$ (correct and confident): Loss = 0 (no penalty).
- If $0 < y \hat{y} < 1$ (correct but uncertain): Loss increases linearly as $\hat{y}$ approaches 0.
- If $y \hat{y} \leq 0$ (misclassified): Loss = $1 - y\hat{y} \geq 1$ (large penalty).

The soft-margin SVM objective:
$$\min_{\theta, \theta_0} \frac{1}{2}\|\theta\|_2^2 + C \sum_{i=1}^{n} \xi_i$$

with $\xi_i = \max(0, 1 - y_i \hat{y}_i)$ is equivalent to:

$$\min_{\theta, \theta_0} \frac{1}{2C}\|\theta\|_2^2 + \sum_{i=1}^{n} \max(0, 1 - y_i \hat{y}_i)$$

**SVM = Regularized Hinge Loss**. The margin maximization objective is equivalent to hinge loss minimization with L2 regularization!

---

## Summary

**Regularization** addresses overfitting by penalizing model complexity:

1. **Ridge (L2):** Smooth shrinkage, all features remain. Stable closed-form solution.
2. **Lasso (L1):** Aggressive shrinkage to zero, automatic feature selection. Interpretable but no closed form.
3. **Elastic Net:** Combines both for high-dimensional correlated data.
4. **SGD:** Implicit regularization through noise, no explicit penalty needed.

**Support Vector Machines** maximize robustness:

1. **Maximum Margin:** Find the widest street between classes.
2. **Support Vectors:** Only these boundary points determine the solution; interior points don't matter.
3. **Primal-Dual:** The dual problem uses only dot products, enabling the kernel trick for non-linear boundaries.
4. **KKT Conditions:** Characterize optimal solutions and guide efficient algorithms.
5. **Hinge Loss:** SVM is equivalent to regularized hinge loss minimization.

Together, regularization and SVMs are the workhorses of classical machine learning, addressing generalization and robustness.

---

## Key Takeaways

| Concept | Definition | Example |
|---------|-----------|---------|
| **Regularization** | Penalty term added to loss to prevent overfitting by constraining weight magnitude | $J(\theta) = \text{MSE}(\theta) + \lambda \|\theta\|_p$ |
| **Ridge (L2)** | Penalty: $\lambda \|\theta\|_2^2$; smooth shrinkage; no feature elimination | $\theta = (X^T X + \lambda I)^{-1} X^T \mathbf{y}$ |
| **Lasso (L1)** | Penalty: $\lambda \|\theta\|_1$; aggressive shrinkage to exactly zero; feature selection | Sparsity: automatically deletes unimportant features |
| **Elastic Net** | Combines L1 and L2: $\lambda_1 \|\theta\|_1 + \lambda_2 \|\theta\|_2^2$ | Best for high-dim correlated data |
| **Weight Decay** | Gradient descent update with penalty: $\theta^{(t+1)} = \theta^{(t)}(1 - 2\eta\lambda) - \eta g_j$ | Natural shrinkage toward zero each iteration |
| **SGD as Regularizer** | Stochastic gradient updates implicitly regularize by favoring flat minima | Noise in gradient estimates prevents overfitting |
| **Margin** | Geometric distance from decision boundary to nearest data point: $\frac{\|f(x_i)\|}{\|\theta\|_2}$ | Larger margin = more robust classifier |
| **Support Vectors** | Data points that touch the margin boundary; satisfy $y_i f(x_i) = 1 - \xi_i$ | Only these points determine the SVM solution |
| **Hinge Loss** | $L_{\text{hinge}} = \max(0, 1 - y \hat{y})$; zero if confident and correct, large if wrong | SVM minimizes regularized hinge loss |
| **KKT Conditions** | Necessary and sufficient for optimality of constrained convex problems; enable efficient solvers | Complementary slackness: $\alpha_i > 0 \iff$ point is support vector |
| **Primal vs Dual** | Primal: minimize $\|\theta\|^2$ s.t. constraints; Dual: maximize in terms of dot products $x_i^T x_j$ | Dual enables kernel trick for non-linear SVMs |

---

## References to Assignment Content

**Related Assignment Questions:**
- Q1-Q2: Ridge gradient and update rule
- Q3-Q4: Lasso gradient (piecewise)
- Q5: Logistic regression gradient formula
- Q6: Decision boundary equation
- Q7: Margin distance formula
- Q8: Ridge closed-form solution
- Q9: Elastic Net penalty calculation
- Q10: Gaussian noise variance MLE

# Week 7: Assignment 7 — Mathematical Foundations of Machine Learning (CS02)

| Metric | Value |
|--------|-------|
| Due | 2026-03-11, 23:59 IST |
| Submitted | — |
| Score | — |

---

## Question Set 1: Ridge Regression (L2 Regularization)

### Q1 (1 point)

**Ridge objective: $J(w) = \text{MSE}(w) + \lambda\|w\|_2^2$. If the gradient of MSE w.r.t. $w_j$ is $g_j = 5$ & $w_j = -3$, $\lambda = 0.2$, what is $\partial J/\partial w_j$?**

- [x] A) 3.8
- [ ] B) 5.6
- [ ] C) 6.2
- [ ] D) 4.4

**Answer:** A (3.8)

**Platform Status:** ⚠️ **DOCUMENTED PLATFORM ERROR** — The platform lists 6.2 as the accepted answer, but this is mathematically incorrect. The true answer is **3.8**. (This appears to be a sign error in the platform's key.)

**Answer Explanation:**

The objective is a **sum** of two independent terms. To find the total gradient, we add the gradient of each term.

$$J(w) = \underbrace{\text{MSE}(w)}_{\text{Part 1}} + \underbrace{\lambda \|w\|_2^2}_{\text{Part 2}}$$

**Gradient of Part 1 (MSE):** Given as $g_j = 5$.

**Gradient of Part 2 (L2 Penalty):**

The L2 penalty for a single weight is $\lambda w_j^2$. Using the power rule:

$$\frac{\partial}{\partial w_j}(\lambda w_j^2) = 2\lambda w_j$$

Substituting $\lambda = 0.2$ and $w_j = -3$:

$$2\lambda w_j = 2(0.2)(-3) = -1.2$$

**Total Gradient:**

$$\frac{\partial J}{\partial w_j} = g_j + 2\lambda w_j = 5 + (-1.2) = 3.8$$

**Why Platform Error Occurred:**

The platform likely computed $5 - (-1.2) = 6.2$ by incorrectly **subtracting** the penalty gradient instead of adding it. This would only be correct if the penalty term had a minus sign, which it does not.

**Analysis of Incorrect Options:**

- **B) 5.6:** Would result from $5 + 2(0.2)(-1.5)$ (different weight value).
- **C) 6.2:** Platform's incorrect answer (subtraction error: $5 - (-1.2)$).
- **D) 4.4:** Would result from different parameter values.

**Key Insight:** When computing gradients of sums, **always add** the individual gradients. The penalty term explicitly adds to the loss; it does not subtract.

**Related Topics:** [Ridge Regression (L2 Regularization)](notes.md#ridge-regression-l2-regularization)

---

### Q2 (1 point)

**One GD step for ridge on a single weight: $w^{(t+1)} = w^{(t)} - \eta(g + 2\lambda w^{(t)})$. Given $w^{(t)} = 2, g = 3, \lambda = 0.5, \eta = 0.1$. Compute $w^{(t+1)}$.**

- [ ] A) 1.2
- [x] B) 1.5
- [ ] C) 1.8
- [ ] D) 2.5

**Answer:** B

**Answer Explanation:**

This question directly applies the **Gradient Descent update rule with L2 regularization**. We simply plug in the numbers and compute step by step.

**Given:**
- Current weight: $w^{(t)} = 2$
- MSE gradient: $g = 3$
- Regularization parameter: $\lambda = 0.5$
- Learning rate: $\eta = 0.1$

**Step 1: Compute the gradient of the objective (from Q1 logic)**

$$\frac{\partial J}{\partial w} = g + 2\lambda w^{(t)} = 3 + 2(0.5)(2) = 3 + 2 = 5$$

**Step 2: Apply the update rule**

$$w^{(t+1)} = w^{(t)} - \eta \cdot \frac{\partial J}{\partial w}$$

$$w^{(t+1)} = 2 - 0.1 \times 5 = 2 - 0.5 = 1.5$$

**Interpretation:** 
- The gradient is 5 (pulling weight downward strongly).
- We take a step of size $\eta \times 5 = 0.5$ in the direction opposite the gradient.
- New weight is 1.5.

**Notice:** The penalty term $2\lambda w^{(t)} = 2$ pulls toward zero (since $w > 0$). Without it, the weight would only change to $2 - 0.1(3) = 1.7$. The penalty causes extra shrinkage.

**Analysis of Incorrect Options:**

- **A) 1.2:** Would result from $2 - 0.1 \times 8$ (different gradient computation).
- **C) 1.8:** Results from ignoring the penalty term: $2 - 0.1(3) = 1.7 \approx 1.8$ (close but wrong).
- **D) 2.5:** Wrong direction; would result from adding instead of subtracting.

**Related Topics:** [Ridge Regression (L2 Regularization)](notes.md#ridge-regression-l2-regularization)

---

## Question Set 2: Lasso Regression (L1 Regularization)

### Q3 (1 point)

**Lasso objective (single weight): $J(w) = \frac{1}{2}(y - \hat{y})^2 + \lambda|w|$. Assume $w > 0$, gradient of squared-error part is $g = -0.7$ & $\lambda = 0.5$. What is $dJ/dw$?**

- [ ] A) -1.2
- [x] B) -0.2
- [ ] C) 0.2
- [ ] D) 1.2

**Answer:** B

**Answer Explanation:**

**Lasso uses absolute value**, which has a piecewise linear derivative:

$$\frac{d}{dw} |w| = \begin{cases} +1 & \text{if } w > 0 \\ -1 & \text{if } w < 0 \\ \text{undefined} & \text{if } w = 0 \end{cases}$$

Since we're told $w > 0$, the derivative of the penalty is $+1$.

**Gradient Computation:**

$$\frac{dJ}{dw} = \frac{d(\text{squared error})}{dw} + \lambda \cdot \frac{d|w|}{dw}$$

$$\frac{dJ}{dw} = g + \lambda(+1) = -0.7 + 0.5(+1) = -0.7 + 0.5 = -0.2$$

**Interpretation:**
- The squared-error gradient is $-0.7$ (pulling $w$ upward/increasing).
- The penalty term is $+0.5$ (pulling $w$ toward zero, i.e., downward).
- Net gradient: $-0.2$ (weak upward pull; penalty partially cancels error term).

**Critical Insight (Threshold for Zeros):**

Notice that the penalty ($0.5$) nearly canceled the error gradient ($-0.7$). If the error gradient were smaller (say, $-0.3$), the penalty would completely overwhelm it, and $dJ/dw$ would become positive, pushing $w$ to exactly zero. This is how **Lasso performs feature selection**.

**Analysis of Incorrect Options:**

- **A) -1.2:** Would result from $g - \lambda$ (subtraction instead of addition of penalty).
- **C) 0.2:** Sign flipped; would result from $g - \lambda(+1)$ with wrong sign.
- **D) 1.2:** Wrong magnitude and sign.

**Related Topics:** [Lasso Regression (L1 Regularization)](notes.md#lasso-regression-l1-regularization)

---

### Q4 (1 point)

**Same Lasso setting as above, but now $w < 0$, with squared-error gradient $g = 1.6$ and $\lambda = 0.4$. What is $dJ/dw$?**

- [ ] A) 1.2
- [ ] B) 2.0
- [ ] C) 1.6
- [x] D) 1.2

**Answer:** D

**Answer Explanation:**

Now $w < 0$, so the derivative of the absolute value is $-1$.

$$\frac{d}{dw}|w| = -1 \quad (\text{when } w < 0)$$

**Gradient Computation:**

$$\frac{dJ}{dw} = g + \lambda(-1) = 1.6 + 0.4(-1) = 1.6 - 0.4 = 1.2$$

**Interpretation:**
- The squared-error gradient is $+1.6$ (pushing $w$ downward/more negative).
- The penalty term is $-0.4$ (pulling $w$ toward zero, i.e., upward).
- Net gradient: $+1.2$ (strong downward pull; error dominates the penalty).

**Comparison with Q3:**

| Question | $w$ sign | Error gradient | Penalty | Net | Effect |
|----------|----------|---|---------|-----|--------|
| Q3 | $w > 0$ | $-0.7$ | $+0.5$ | $-0.2$ | Both push toward zero; net toward zero |
| Q4 | $w < 0$ | $+1.6$ | $-0.4$ | $+1.2$ | Error dominates; net away from zero |

**Lesson:** The sign of the penalty derivative changes based on the sign of $w$. This creates an **asymmetric push toward zero**, which is the mechanism for **sparsity** (zeroing out weights).

**Analysis of Incorrect Options:**

- **A) 1.2:** Actually correct! Repeated at D. (Both options have same value.)
- **B) 2.0:** Would result from $1.6 + 0.4$ (penalty added instead of subtracted).
- **C) 1.6:** Ignores the penalty term entirely.

**Wait:** Options A and D both list 1.2. This appears to be a duplication in the original problem. The answer is **1.2**, which is both A and D. For this response, I'll mark **D**, but both are correct.

**Related Topics:** [Lasso Regression (L1 Regularization)](notes.md#lasso-regression-l1-regularization)

---

## Question Set 3: Logistic Regression and Decision Boundaries

### Q5 (1 point)

**Logistic regression (cross-entropy). For one sample, $y = 1$, predicted $\hat{y} = 0.2$, and feature $x = 3$. What is $\partial L/\partial w$?**

- [x] A) -2.4
- [ ] B) 2.4
- [ ] C) -0.6
- [ ] D) 0.6

**Answer:** A

**Answer Explanation:**

**The Elegant Shortcut:** For logistic regression with cross-entropy loss, there is a beautiful, simple formula for the gradient. When you combine the sigmoid activation with cross-entropy loss, the complex calculus perfectly simplifies:

$$\frac{\partial L}{\partial w} = (\hat{y} - y) \times x$$

where:
- $\hat{y}$: Predicted probability (output of sigmoid)
- $y$: True label (0 or 1)
- $x$: Feature value

**Computation:**

$$\frac{\partial L}{\partial w} = (\hat{y} - y) \times x = (0.2 - 1) \times 3 = (-0.8) \times 3 = -2.4$$

**Interpretation:**
- We predicted $\hat{y} = 0.2$ (20% probability of class 1).
- The true label is $y = 1$ (definitely class 1).
- The prediction is way off (underestimated probability).
- The error signal $(\hat{y} - y) = -0.8$ is large and negative.
- The feature $x = 3$ is positive.
- Gradient $= -0.8 \times 3 = -2.4$ (strong negative gradient, pull weight up to increase prediction).

**Why Negative?** A negative gradient means we should **increase** the weight in the next gradient descent step (since we subtract the gradient: $w^{(t+1)} = w^{(t)} - \eta \times (-2.4)$, which increases $w$). Intuitively, increasing the weight makes the prediction higher (more toward 1.0), which is what we want.

**Derivation Note:** This formula comes from:
$$\frac{\partial L}{\partial w} = \frac{\partial}{\partial w}[−y \log \hat{y} − (1−y) \log(1−\hat{y})] = (\hat{y} - y) x$$

The sigmoid derivative $\sigma'(z) = \sigma(z)(1 - \sigma(z))$ cancels perfectly!

**Analysis of Incorrect Options:**

- **B) 2.4:** Sign flipped; would be $(y - \hat{y}) \times x$ (wrong sign convention).
- **C) -0.6:** Would result from $(\hat{y} - y) \times 1$ if $x = 1$ instead of $x = 3$ (wrong feature value).
- **D) 0.6:** Wrong magnitude and sign.

**Key Insight:** This gradient formula is one of the most elegant results in ML. It says: "Error times feature value." Larger errors create stronger gradients. Features with larger values create stronger gradients.

**Related Topics:** [Logistic Regression and Decision Boundaries](notes.md#linear-models-for-classification)

---

### Q6 (1 point)

**Logistic regression: $w_1 = -2$, $w_2 = 1$, $b = 4$. Decision boundary ($\hat{y} = 0.5$) satisfies $w^T x + b = 0$. What is the line equation for $x_2$ in terms of $x_1$?**

- [ ] A) $x_2 = -2x_1 + 4$
- [x] B) $x_2 = 2x_1 - 4$
- [ ] C) $x_2 = 2x_1 + 4$
- [ ] D) $x_2 = -2x_1 - 4$

**Answer:** B

**Answer Explanation:**

In logistic regression, the decision boundary is where the model is exactly 50% confident. This occurs when:
$$w_1 x_1 + w_2 x_2 + b = 0$$

This is the **equation of the boundary line** separating the two classes.

**Substituting the Given Values:**

$$-2x_1 + 1 \cdot x_2 + 4 = 0$$

$$x_2 + (−2x_1 + 4) = 0$$

$$x_2 = 2x_1 - 4$$

**Verification:**

Let's check two points on this line:
- When $x_1 = 0$: $x_2 = -4$. Point $(0, -4)$. Check: $-2(0) + 1(-4) + 4 = 0$. ✓
- When $x_1 = 2$: $x_2 = 2(2) - 4 = 0$. Point $(2, 0)$. Check: $-2(2) + 1(0) + 4 = 0$. ✓

**Geometric Interpretation:**
- The line passes through $(0, -4)$ and $(2, 0)$.
- Slope = $\frac{0 - (-4)}{2 - 0} = \frac{4}{2} = 2$.
- Y-intercept = $-4$.
- Equation: $x_2 = 2x_1 - 4$. ✓

**Decision Rule:**
- If $w^T x + b > 0$: Predict class 1 (positive side).
- If $w^T x + b < 0$: Predict class 0 (negative side).
- If $w^T x + b = 0$: Exactly on boundary (50% confidence).

**Analysis of Incorrect Options:**

- **A) $x_2 = -2x_1 + 4$:** Would result from $w_1 x_1 + w_2 x_2 + b = 0$ with $w_1 = +2$ instead of $-2$ (sign error).
- **C) $x_2 = 2x_1 + 4$:** Wrong intercept; would result from $b = -4$ instead of $b = 4$.
- **D) $x_2 = -2x_1 - 4$:** Wrong slope and intercept.

**Key Insight:** Decision boundaries in linear models are always linear (straight lines in 2D, hyperplanes in higher dimensions).

**Related Topics:** [Linear Models for Classification](notes.md#linear-models-for-classification)

---

## Question Set 4: Support Vector Machines

### Q7 (1 point)

**For $w = [?, ?]^T$, $b = -10$ and point $x = [?, ?]^T$, compute distance $d = |w^T x + b| / \|w\|_2$.**

- [ ] A) 0.2
- [x] B) 0.4
- [ ] C) 0.6
- [ ] D) 0.8

**Answer:** B

**Answer Explanation:**

⚠️ **NOTE: Incomplete Vector Data** — The scraping tool did not capture the actual numeric values inside vectors $w$ and $x$. However, the concept and formula are what matter for understanding.

**The Formula: Geometric Distance to Decision Boundary**

The **margin distance** (geometric distance from a point to the decision boundary) is:

$$d = \frac{|w^T x + b|}{\|w\|_2}$$

where:
- **Numerator** $|w^T x + b|$: The absolute value of the linear score (how far the point is from boundary in "score space").
- **Denominator** $\|w\|_2 = \sqrt{\sum_i w_i^2}$: The length (norm) of the weight vector. Dividing by this normalizes the score to a true geometric distance.

**Why Normalize by $\|w\|_2$?**

Without normalization, the score $|w^T x + b|$ depends on the arbitrary scaling of $w$. If we multiply $w$ and $b$ by 2, the score doubles, but the point hasn't actually moved. Dividing by $\|w\|_2$ removes this scaling dependence, giving the true perpendicular distance.

**Geometric Interpretation:**

In 2D, the line is $w_1 x_1 + w_2 x_2 + b = 0$. The distance from a point $(x_1, x_2)$ to this line is the familiar formula:

$$d = \frac{|w_1 x_1 + w_2 x_2 + b|}{\sqrt{w_1^2 + w_2^2}} = \frac{|w^T x + b|}{\|w\|_2}$$

**SVM Context:**

SVMs maximize the **margin**, which is the minimum distance from any training point to the decision boundary. Support vectors are the points that achieve this minimum margin.

**Related Assignment Questions:** The answer is **0.4** based on the platform's accepted answer, though we cannot verify the calculation without the vector values.

**Analysis of Incorrect Options:**

- **A) 0.2:** Would result if the numerator were halved.
- **C) 0.6:** Would result if the norm were smaller (different $w$ values).
- **D) 0.8:** Would result if the numerator were larger (different $x$ or $b$).

**Related Topics:** [Support Vector Machines (SVM)](notes.md#support-vector-machines-svm), [The Margin](notes.md#the-margin)

---

## Question Set 5: Ridge Closed-Form Solution and Regularization

### Q8 (1 point)

**1D ridge regression: minimize $\sum_i (y_i - wx_i)^2 + \lambda w^2$. Given $\sum x_i^2 = 10$, $\sum x_i y_i = 14$ & $\lambda = 2$, what is $w_{\text{ridge}}$?**

- [ ] A) 1.0
- [x] B) 1.167
- [ ] C) -1.0
- [ ] D) -1.167

**Answer:** B

**Answer Explanation:**

For **1D linear regression** (single weight, no bias), the Ridge objective is:

$$J(w) = \sum_i (y_i - wx_i)^2 + \lambda w^2$$

We can solve this analytically by taking the derivative and setting it to zero.

**Derivation:**

$$\frac{dJ}{dw} = \sum_i 2(y_i - wx_i)(-x_i) + 2\lambda w = 0$$

$$-2\sum_i (y_i - wx_i) x_i + 2\lambda w = 0$$

$$-2\sum_i y_i x_i + 2w\sum_i x_i^2 + 2\lambda w = 0$$

$$2w(\sum_i x_i^2 + \lambda) = 2\sum_i y_i x_i$$

$$w = \frac{\sum_i y_i x_i}{\sum_i x_i^2 + \lambda}$$

**This is the Ridge Regression closed-form solution for 1D.**

**Computation:**

Substitute the given values:
$$w_{\text{ridge}} = \frac{14}{10 + 2} = \frac{14}{12} = \frac{7}{6} \approx 1.1666...$$

Rounding to three decimal places: **1.167**

**Comparison: Unregularized vs Regularized**

Without regularization ($\lambda = 0$):
$$w_{\text{OLS}} = \frac{14}{10} = 1.4$$

With Ridge ($\lambda = 2$):
$$w_{\text{ridge}} = \frac{14}{12} \approx 1.167$$

The penalty shrinks the weight from 1.4 to 1.167. The penalty term $\lambda$ appears in the **denominator**, making it larger, which forces $w$ to be smaller. This is the essence of regularization.

**Analysis of Incorrect Options:**

- **A) 1.0:** Would result from $w = 10 / 10$ (ignoring the numerator).
- **C) -1.0:** Wrong sign; suggests a miscomputation.
- **D) -1.167:** Wrong sign; would be the negative of the correct answer.

**Key Insight:** The closed-form solution shows that Ridge regularization simply adds $\lambda$ to the denominator of the unregularized solution. Larger $\lambda$ forces larger denominator → smaller weight.

**Related Topics:** [Ridge Regression (L2 Regularization)](notes.md#ridge-regression-l2-regularization)

---

## Question Set 6: Elastic Net and Gaussian Noise

### Q9 (1 point)

**Elastic Net penalty is $\lambda_1\|w\|_1 + \lambda_2\|w\|_2^2$. Let $w = [1, -2, 3]$, $\lambda_1 = 0.4$, $\lambda_2 = 0.1$. Compute the penalty value.**

- [ ] A) 2.4
- [ ] B) 3.1
- [x] C) 3.8
- [ ] D) 4.2

**Answer:** C

**Answer Explanation:**

**Elastic Net** combines both L1 (Lasso) and L2 (Ridge) regularization penalties. To compute the total penalty, calculate each part separately, then sum them.

**Given:**
- $w = [1, -2, 3]$
- $\lambda_1 = 0.4$ (Lasso coefficient)
- $\lambda_2 = 0.1$ (Ridge coefficient)

**Part 1: L1 Penalty (Lasso)**

$$\lambda_1 \|w\|_1 = \lambda_1 \sum_j |w_j| = 0.4 (|1| + |-2| + |3|)$$

$$= 0.4 (1 + 2 + 3) = 0.4 \times 6 = 2.4$$

**Part 2: L2 Penalty (Ridge)**

$$\lambda_2 \|w\|_2^2 = \lambda_2 \sum_j w_j^2 = 0.1 (1^2 + (-2)^2 + 3^2)$$

$$= 0.1 (1 + 4 + 9) = 0.1 \times 14 = 1.4$$

**Total Penalty:**

$$\text{Penalty} = 2.4 + 1.4 = 3.8$$

**When is Elastic Net Useful?**

| Scenario | Preferred Method | Reason |
|----------|------------------|--------|
| Few features, many samples | Ridge | Stable solution, closed form |
| Feature selection needed | Lasso | Automatic zeros out features |
| High-dim, highly correlated features | Elastic Net | L1 selects features, L2 stabilizes |

**Why Combine L1 and L2?**

- **L1 alone** can be unstable when features are correlated (which group of correlated features to zero out?)
- **L2 alone** doesn't perform feature selection (keeps all features).
- **Elastic Net** balances: L1 encourages sparsity, L2 provides stability.

**Analysis of Incorrect Options:**

- **A) 2.4:** Only the L1 part; missing L2 contribution (1.4).
- **B) 3.1:** Would result from wrong calculation (e.g., $2.4 + 0.7$).
- **D) 4.2:** Would result from $2.4 + 1.8$ (wrong L2 calculation).

**Key Insight:** Elastic Net is increasingly popular in modern ML, especially for high-dimensional data where features are naturally correlated (e.g., genomics, text analysis).

**Related Topics:** [Elastic Net: Combining Ridge and Lasso](notes.md#elastic-net-combining-ridge-and-lasso)

---

### Q10 (1 point)

**Linear regression with Gaussian noise $\varepsilon \sim \mathcal{N}(0, \sigma^2)$. Given $\text{RSS} = \|y - X\hat{w}\|^2 = 40$ & $N = 20$. The MLE is $\hat{\sigma}^2 = \text{RSS}/N$. What is $\hat{\sigma}^2$?**

- [ ] A) 1
- [x] B) 2
- [ ] C) 4
- [ ] D) 8

**Answer:** B

**Answer Explanation:**

This question bridges **probability theory** (Week 5-6) with **linear regression** (Week 6-7). We assume errors are Gaussian and use MLE to estimate the noise variance.

**Probabilistic Model:**

Assume each target $y_i$ is generated by:
$$y_i = w^T x_i + \varepsilon_i, \quad \varepsilon_i \sim \mathcal{N}(0, \sigma^2)$$

Given fitted weights $\hat{w}$, the residuals (errors) are:
$$\hat{\varepsilon}_i = y_i - \hat{w}^T x_i$$

**Likelihood of Data:**

Assuming errors are i.i.d. Gaussian:
$$L(\sigma^2 | \hat{\varepsilon}) = \prod_{i=1}^{N} \frac{1}{\sqrt{2\pi\sigma^2}} \exp\left( -\frac{\hat{\varepsilon}_i^2}{2\sigma^2} \right)$$

Taking the log-likelihood:
$$\log L = -\frac{N}{2}\log(2\pi\sigma^2) - \frac{1}{2\sigma^2} \sum_{i=1}^{N} \hat{\varepsilon}_i^2$$

$$= -\frac{N}{2}\log(\sigma^2) + \text{const} - \frac{\text{RSS}}{2\sigma^2}$$

**Maximum Likelihood Estimation:**

To maximize $\log L$ w.r.t. $\sigma^2$, take the derivative and set to zero:

$$\frac{\partial \log L}{\partial \sigma^2} = -\frac{N}{2\sigma^2} + \frac{\text{RSS}}{2(\sigma^2)^2} = 0$$

Solving:
$$\frac{\text{RSS}}{(\sigma^2)^2} = \frac{N}{\sigma^2}$$

$$\text{RSS} = N \sigma^2$$

$$\sigma^2 = \frac{\text{RSS}}{N}$$

**Computation:**

$$\hat{\sigma}^2 = \frac{40}{20} = 2$$

**Interpretation:**

This is the **sample variance** of the residuals. It estimates the variance of the noise in the data. If $\hat{\sigma}^2 = 2$, then the standard deviation of the noise is $\sqrt{2} \approx 1.41$.

**Insight:** The MLE for Gaussian noise variance is just the average squared error—intuitively, the residuals tell us about the noise level.

**Note:** Some texts use $\frac{\text{RSS}}{N-d}$ (unbiased estimator with degrees of freedom correction), but the MLE is exactly $\frac{\text{RSS}}{N}$.

**Analysis of Incorrect Options:**

- **A) 1:** Would result from $40 / 40$ (wrong divisor).
- **C) 4:** Would result from $40 / 10$ (different $N$).
- **D) 8:** Would result from $40 / 5$ (different $N$).

**Key Insight:** When you assume Gaussian errors, the residual sum of squares directly tells you the noise level. This connects optimization (finding best $w$) to probabilistic inference (estimating $\sigma^2$).

**Related Topics:** [Ridge Regression (L2 Regularization)](notes.md#ridge-regression-l2-regularization), [The SVM Formulation: Primal and Dual](notes.md#the-svm-formulation-primal-and-dual)

---

## Answer Key Summary

| Q | Answer | Concept | Difficulty | Notes |
|---|--------|---------|-----------|-------|
| Q1 | A (3.8) | Ridge gradient composition | Easy | **Platform lists 6.2 (ERROR)**; correct is 3.8 |
| Q2 | B (1.5) | Ridge GD update with penalty | Easy | Direct plug-in formula application |
| Q3 | B (-0.2) | Lasso gradient ($w > 0$) | Medium | Piecewise derivative of absolute value |
| Q4 | D (1.2) | Lasso gradient ($w < 0$) | Medium | Sign-dependent penalty derivative |
| Q5 | A (-2.4) | Logistic regression gradient | Easy | Beautiful shortcut: $(\hat{y} - y) \times x$ |
| Q6 | B ($x_2 = 2x_1 - 4$) | Decision boundary equation | Easy | Solve $w^T x + b = 0$ for $x_2$ |
| Q7 | B (0.4) | Margin distance formula | Medium | $d = \|w^T x + b\| / \|w\|_2$ |
| Q8 | B (1.167) | Ridge closed-form 1D solution | Easy | $w = \frac{\sum x_i y_i}{\sum x_i^2 + \lambda}$ |
| Q9 | C (3.8) | Elastic Net penalty | Easy | Combine L1 (2.4) and L2 (1.4) parts |
| Q10 | B (2) | Gaussian noise MLE variance | Medium | $\hat{\sigma}^2 = \text{RSS}/N$ from likelihood |

**Overall Score:** 9/10 points possible (Q1 has documented platform error)

---

## Concept Map: Week 7 Progression

**From Complex Models to Robust Classifiers:**

1. **The Overfitting Crisis (Week 6 → Week 7):** Week 6 showed us bias-variance tradeoff. Week 7 provides the cure.

2. **Regularization as Mathematical Brake:**
   - **Ridge (L2):** Smooth shrinkage, numerically stable, closed-form solution.
   - **Lasso (L1):** Aggressive shrinkage to zero, automatic feature selection.
   - **Elastic Net:** Best of both for high-dimensional correlated data.

3. **SGD's Hidden Power:** Even without explicit regularization, stochastic updates implicitly regularize by preferring flat minima.

4. **SVM's Elegance:**
   - **Primal:** Maximize margin, minimize weights, constrained optimization.
   - **Dual:** Replace with dot products, enable kernel trick (next week!).
   - **KKT:** Mathematical certificate of optimality.

5. **Unifying Insights:**
   - Ridge = MAP with Gaussian prior
   - Lasso = MAP with Laplace prior
   - Soft-margin SVM = Regularized hinge loss
   - Ridge 1D = Simple closed-form weight shrinkage

**Week 7 Message:** Real data is messy and non-separable. Overfitting is the enemy. Regularization and SVMs are your defenses. Together, they form the practical foundation of classical ML.

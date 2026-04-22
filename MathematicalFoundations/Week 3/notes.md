# Week 3: Theoretical Notes — ERM and MLE

## Overview

This week bridges probability theory and practical machine learning by introducing **information theory** and the formal framework of **risk minimization**. We learn how to measure the "distance" between probability distributions using entropy and KL divergence, how to find optimal model parameters via Maximum Likelihood Estimation (MLE), and how the Empirical Risk Minimization (ERM) framework connects learning to optimization. The culmination is understanding the Bayes Classifier as the theoretical optimum and recognizing the gap between true and empirical risk.

## Entropy: Measuring Information

**Surprisal** measures how much "information" or "surprise" an event carries. A rare event contains more information than a common one.

**Definition:** For an event with probability $P(A)$, the surprisal (or self-information) is:

$$I(A) = -\log P(A)$$

**Intuition:**
- If an event is very likely ($P(A) \approx 1$), then $-\log P(A) \approx 0$ (not surprising)
- If an event is rare ($P(A) \approx 0$), then $-\log P(A) \rightarrow \infty$ (very surprising)

### Entropy of a Distribution

**Entropy** is the average surprisal across all possible outcomes—the overall unpredictability of a distribution.

**Definition:** For a discrete probability distribution $P$:

$$H(P) = -\sum_{x} P(x) \log P(x)$$

The log can be base 2 (bits), base $e$ (nats), or base 10 (dits). The choice determines the units.

**Intuition:**
- If $P$ is concentrated on one outcome (deterministic), entropy is near 0
- If $P$ is uniform (all outcomes equally likely), entropy is maximum: $H(P) = \log |\text{support}|$
- Higher entropy means more uncertainty

**Example:** For a uniform distribution over 4 equally likely outcomes:
$$H(P) = -4 \cdot \frac{1}{4} \log_2\left(\frac{1}{4}\right) = -\frac{1}{4} \cdot (-2) \cdot 4 = 2 \text{ bits}$$

For a distribution with $P(a) = 1/2, P(b) = 1/4, P(c) = 1/8, P(d) = 1/8$ using $\log_2$:
$$H(P) = -\left(\frac{1}{2} \log_2(1/2) + \frac{1}{4} \log_2(1/4) + \frac{1}{8} \log_2(1/8) + \frac{1}{8} \log_2(1/8)\right)$$
$$= 0.5 + 0.5 + 0.375 + 0.375 = 1.75 \text{ bits}$$

**Related Assignment Questions**: Q1

## Cross-Entropy: Distance Between Distributions

In machine learning, we often have a true data distribution $P$ but use a model distribution $Q$ to encode or predict it.

**Definition:** The **cross-entropy** from $P$ to $Q$ is the average code-length needed to encode data from $P$ using an optimal code designed for $Q$:

$$H(P, Q) = -\sum_{x} P(x) \log Q(x)$$

**Intuition:** 
- If $Q$ matches $P$ perfectly, cross-entropy equals entropy: $H(P, Q) = H(P)$
- If $Q$ is a poor approximation, cross-entropy is much larger
- We use more bits than necessary (the extra cost of using the wrong code)

**Example:** If data comes from $P = [1/2, 1/4, 1/8, 1/8]$ but we designed our encoder for uniform $Q = [1/4, 1/4, 1/4, 1/4]$, the cross-entropy $H(P, Q)$ would be higher than $H(P)$ itself.

**Related Assignment Questions**: Q2

## KL Divergence: Measuring Distribution Distance

The **Kullback-Leibler (KL) Divergence** quantifies the "distance" between two probability distributions—the penalty for using one distribution to approximate another.

**Definition:**

$$D_{KL}(P \mid\mid Q) = \sum_{x} P(x) \log \frac{P(x)}{Q(x)}$$

Alternatively (using cross-entropy):

$$D_{KL}(P \mid\mid Q) = H(P, Q) - H(P)$$

**Key Properties:**

1. **Non-negativity:** $D_{KL}(P \mid\mid Q) \geq 0$ always
2. **Zero iff identical:** $D_{KL}(P \mid\mid Q) = 0$ if and only if $P = Q$ (almost everywhere)
3. **Asymmetry:** $D_{KL}(P \mid\mid Q) \neq D_{KL}(Q \mid\mid P)$ in general
4. **Infinite penalty:** If there exists $x^*$ where $P(x^*) > 0$ but $Q(x^*) = 0$, then $D_{KL}(P \mid\mid Q) = +\infty$ (catastrophic modeling error)

**Intuition:** KL divergence measures the "information loss" or "wasted communication" from using distribution $Q$ instead of the true distribution $P$.

**Fundamental Identity:**

$$H(P, Q) = H(P) + D_{KL}(P \mid\mid Q)$$

This elegant relation decomposes the total code length into two parts:
- $H(P)$: The minimum possible (using the true distribution)
- $D_{KL}(P \mid\mid Q)$: The extra penalty (for guessing wrong)

**Related Assignment Questions**: Q3, Q4

## Maximum Likelihood Estimation (MLE)

Given i.i.d. samples from an unknown distribution, how do we find the best parametric model?

**The Problem:** We observe data $\{v_1, v_2, \ldots, v_N\}$ i.i.d. from unknown true distribution $p$. We assume the data follows a parametric family $p_{\theta}$ and want to find the best $\theta$.

**The MLE Approach:**

We want to minimize the KL divergence between the true distribution and our model:

$$\theta^* = \arg\min_{\theta} D_{KL}(p \mid\mid p_{\theta})$$

Since $H(p)$ is constant (doesn't depend on $\theta$), this is equivalent to:

$$\theta^* = \arg\min_{\theta} H(p, p_{\theta}) = \arg\max_{\theta} \mathbb{E}_{v \sim p}[\log p_{\theta}(v)]$$

**Law of Large Numbers:** With i.i.d. samples, the empirical average approximates the true expectation:

$$\mathbb{E}_{v \sim p}[\log p_{\theta}(v)] \approx \frac{1}{N} \sum_{i=1}^{N} \log p_{\theta}(v_i)$$

Therefore, the **Maximum Likelihood Estimator** is:

$$\theta^* = \arg\max_{\theta} \frac{1}{N} \sum_{i=1}^{N} \log p_{\theta}(v_i)$$

Or equivalently:

$$\theta^* = \arg\max_{\theta} \sum_{i=1}^{N} \log p_{\theta}(v_i)$$

**Interpretation:** MLE chooses parameters that make the observed data as probable as possible under the model.

**Related Assignment Questions**: Q5, Q6

## MLE for Gaussian Regression

An important special case: when the output follows a Gaussian distribution.

**Model Assumption:**

$$p_{\theta}(y \mid x) = \mathcal{N}(y; h_{\theta}(x), \sigma^2)$$

where $h_{\theta}(x)$ is the mean prediction and $\sigma^2$ is the fixed variance.

The Gaussian density is:

$$p_{\theta}(y \mid x) = \frac{1}{\sqrt{2\pi\sigma^2}} \exp\left(-\frac{(y - h_{\theta}(x))^2}{2\sigma^2}\right)$$

**Log-Likelihood:**

$$\log p_{\theta}(y \mid x) = \text{const} - \frac{1}{2\sigma^2}(y - h_{\theta}(x))^2$$

**MLE Optimization:**

To maximize the log-likelihood, we minimize the squared error term:

$$\theta^* = \arg\min_{\theta} \sum_{i=1}^{N} (y_i - h_{\theta}(x_i))^2$$

**This is Least Squares Regression!** The connection between MLE and squared error loss emerges naturally from the Gaussian assumption.

**Effect of Variance:** If we increase $\sigma^2$ (higher model uncertainty), the weight on squared errors is effectively reduced by the factor $1/(2\sigma^2)$, making the optimizer less sensitive to prediction errors.

**Related Assignment Questions**: Q7, Q8

## Risk Minimization Framework

This is the philosophical foundation of supervised learning.

**Loss Function:** A function $L(y, \hat{y})$ that quantifies the cost of prediction error for a single sample.

**Common Loss Functions:**
- **Squared Error (Regression):** $L(y, \hat{y}) = (y - \hat{y})^2$
- **Absolute Error:** $L(y, \hat{y}) = |y - \hat{y}|$
- **Zero-One Loss (Classification):** $L(y, \hat{y}) = \mathbb{1}[y \neq \hat{y}]$ (1 if wrong, 0 if right)
- **Cross-Entropy Loss (Classification):** $L(y, \hat{y}) = -y \log \hat{y} - (1-y) \log(1-\hat{y})$

### True Risk vs. Empirical Risk

**True Risk (Population Risk):**

$$R(h) = \mathbb{E}_{(x,y) \sim P_{XY}}[L(y, h(x))]$$

This is the expected loss over the true, unknown joint distribution. It represents perfect generalization to the entire population.

**Empirical Risk (Training Risk):**

$$\hat{R}(h) = \frac{1}{N} \sum_{i=1}^{N} L(y_i, h(x_i))$$

This is the average loss over the finite training dataset we actually have.

**The Central Question:** Does minimizing $\hat{R}(h)$ also minimize $R(h)$?

This is the **generalization problem**. A model can have low empirical risk (fits training data well) but high true risk (fails on new data). This phenomenon is called **overfitting**.

**Empirical Risk Minimization (ERM):**

$$h^* = \arg\min_{h \in \mathcal{H}} \hat{R}(h)$$

ERM is the standard approach: find the hypothesis that minimizes training loss, hoping it generalizes.

**Related Assignment Questions**: Q10

## The Bayes Classifier

If we *perfectly* knew the true conditional probability $p(y \mid x)$, what would be the optimal classifier?

**For Binary Classification with Zero-One Loss:**

The **Bayes Classifier** (or Bayes-optimal classifier) is:

$$h^*(x) = \begin{cases} 1 & \text{if } p(y=1 \mid x) > p(y=0 \mid x) \\ 0 & \text{otherwise} \end{cases}$$

Or more generally:

$$h^*(x) = \arg\max_{y} p(y \mid x)$$

**Why Optimal?** With zero-one loss, being wrong is equally bad regardless of confidence. The best we can do is predict the most probable class given the input. If 70% of samples with features $x$ have label 1 and 30% have label 0, predicting 1 is right 70% of the time.

**Bayes Risk:** The minimum achievable true risk is:

$$R^* = \mathbb{E}_{x} \left[\min(p(y=1 \mid x), p(y=0 \mid x))\right]$$

This is the irreducible error—even with perfect knowledge, we make mistakes when the data is genuinely ambiguous.

**Practical Reality:** We rarely know $p(y \mid x)$ exactly. We learn it from data using parametric or non-parametric methods. The gap between our learned classifier and the Bayes Classifier is what we hope to minimize through better models and more data.

**Related Assignment Questions**: Q9

## Summary

- **Entropy $H(P)$:** Average surprisal in a distribution; measures unpredictability

- **Cross-Entropy $H(P,Q)$:** Average code-length using code designed for $Q$ on data from $P$

- **KL Divergence $D_{KL}(P \mid\mid Q) = H(P,Q) - H(P)$:** Penalty for using wrong distribution; measures distance

- **Key Identity:** $H(P,Q) = H(P) + D_{KL}(P \mid\mid Q)$ (total = minimum + penalty)

- **MLE:** Choose parameters maximizing $\sum \log p_{\theta}(v_i)$; equivalent to minimizing $D_{KL}(p \mid\mid p_{\theta})$

- **Gaussian Regression:** MLE with Gaussian assumption reduces to minimizing squared error $\sum(y_i - h(x_i))^2$

- **True Risk $R(h)$:** Expected loss on true distribution (unknown)

- **Empirical Risk $\hat{R}(h)$:** Average loss on training data (observable)

- **ERM:** Minimize $\hat{R}(h)$, but generalization to $R(h)$ is not guaranteed

- **Bayes Classifier:** Predicts class with highest conditional probability; achieves minimum true risk for zero-one loss

## Key Takeaways

| Concept | Definition | Example |
|---------|-----------|---------|
| **Surprisal** | $-\log P(A)$; information content of rare event | Rare event has high surprisal |
| **Entropy** | $H(P) = -\sum P(x) \log P(x)$; average surprisal | Uniform distribution has max entropy |
| **Cross-Entropy** | $H(P,Q) = -\sum P(x) \log Q(x)$; code-length for wrong distribution | Using Gaussian on non-Gaussian data |
| **KL Divergence** | $D_{KL}(P \mid\mid Q) = H(P,Q) - H(P)$; distribution distance | How far model is from truth |
| **KL Properties** | Non-negative, zero iff equal, asymmetric, infinite if support mismatch | Fundamental information measure |
| **MLE Principle** | Maximize $\sum \log p_{\theta}(v_i)$ to minimize $D_{KL}(p \mid\mid p_{\theta})$ | Make observed data most probable |
| **Gaussian MLE** | Equivalent to minimizing $\sum(y_i - h(x_i))^2$ (least squares) | Natural connection of MLE to regression |
| **Loss Function** | $L(y, \hat{y})$; cost of single prediction error | Squared error, zero-one loss, cross-entropy |
| **True Risk** | $R(h) = \mathbb{E}[L(y,h(x))]$ on true distribution | Population performance (unknown) |
| **Empirical Risk** | $\hat{R}(h) = \frac{1}{N}\sum L(y_i,h(x_i))$ on training data | Training performance (observable) |
| **Bayes Classifier** | $h^*(x) = \arg\max_y p(y \mid x)$; predict most probable class | Optimal classifier if we knew $p(y \mid x)$ |

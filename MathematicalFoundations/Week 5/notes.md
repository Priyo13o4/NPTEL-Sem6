# Week 5: Theoretical Notes — EM, MAP Estimates, Non-Parametric Density Estimation

## Overview

This week introduces two major paradigm shifts in machine learning. First, we prove the EM algorithm's convergence and apply it to Gaussian Mixture Models. Second, we move beyond pure data-driven estimation to **Bayesian estimation** using priors (MAP), addressing the problem of limited data. Finally, we abandon parametric assumptions entirely and use **non-parametric density estimation** (Parzen windows and k-NN) to estimate densities directly from data without assuming a functional form. These techniques form the bridge from classical estimation to practical machine learning.

## EM Algorithm Convergence

The EM algorithm is guaranteed to improve the log-likelihood with each iteration.

**Convergence Guarantee:**

Let $\theta^{(t)}$ be the parameters at iteration $t$. Then:

$$\ell(\theta^{(t+1)}) \geq \ell(\theta^{(t)})$$

**Proof Sketch using ELBO:**

Recall from Week 4 the decomposition:

$$\ell(\theta) = \text{ELBO}(q) + D_{KL}(q(z) \mid\mid p_{\theta}(z|x))$$

**E-Step:** Set $q^{(t)}(z) = p_{\theta^{(t)}}(z|x)$
- The KL divergence becomes zero
- ELBO becomes exact: $\text{ELBO}(q^{(t)}) = \ell(\theta^{(t)})$

**M-Step:** Maximize ELBO with respect to $\theta$:

$$\theta^{(t+1)} = \arg\max_{\theta} \text{ELBO}(q^{(t)})$$

Since $\text{ELBO}(q^{(t)})$ was equal to $\ell(\theta^{(t)})$ and we're maximizing it, we get:

$$\text{ELBO}(q^{(t)}) \leq \text{ELBO}_{\theta^{(t+1)}}(q^{(t)})$$

Now, the new log-likelihood at $\theta^{(t+1)}$ satisfies:

$$\ell(\theta^{(t+1)}) = \text{ELBO}_{\theta^{(t+1)}}(q^{(t)}) + D_{KL}(q^{(t)} \mid\mid p_{\theta^{(t+1)}}(z|x)) \geq \text{ELBO}_{\theta^{(t+1)}}(q^{(t)})$$

Combining:

$$\ell(\theta^{(t+1)}) \geq \text{ELBO}_{\theta^{(t+1)}}(q^{(t)}) \geq \text{ELBO}(q^{(t)}) = \ell(\theta^{(t)})$$

**Termination:** The algorithm converges when $\ell(\theta^{(t+1)}) \approx \ell(\theta^{(t)})$.

**Related Assignment Questions**: (provides context for Q1-Q4)

## EM for Gaussian Mixture Models

The EM algorithm is particularly powerful for GMMs, where the latent variable indicates component assignment.

**Model:** 

$$p(x, z) = p(z) \cdot p(x|z)$$

where:
- $z \in \{1, 2, \ldots, K\}$ (component indicator)
- $p(z = k) = \alpha_k$ (mixing weight)
- $p(x | z = k) = \mathcal{N}(x; \mu_k, \Sigma_k)$ (component density)

### E-Step: Compute Responsibilities

Given current parameters $\theta = (\alpha, \mu, \Sigma)$, compute the posterior probability that point $x_i$ belongs to component $k$:

$$\gamma_{ik} = p(z_i = k | x_i) = \frac{\alpha_k \mathcal{N}(x_i; \mu_k, \Sigma_k)}{\sum_{j=1}^{K} \alpha_j \mathcal{N}(x_i; \mu_j, \Sigma_j)}$$

These $\gamma_{ik}$ values (called **responsibilities**) represent soft assignments—how much each component is responsible for each datapoint.

### M-Step: Update Parameters

Using the responsibilities, update the parameters:

$$\alpha_k^{(t+1)} = \frac{1}{N} \sum_{i=1}^{N} \gamma_{ik}^{(t)}$$

$$\mu_k^{(t+1)} = \frac{\sum_{i=1}^{N} \gamma_{ik}^{(t)} x_i}{\sum_{i=1}^{N} \gamma_{ik}^{(t)}}$$

$$\Sigma_k^{(t+1)} = \frac{\sum_{i=1}^{N} \gamma_{ik}^{(t)} (x_i - \mu_k^{(t+1)})(x_i - \mu_k^{(t+1)})^T}{\sum_{i=1}^{N} \gamma_{ik}^{(t)}}$$

**Interpretation:**
- $\alpha_k$ is the average responsibility across all points
- $\mu_k$ is a weighted average of points, weighted by their responsibility for component $k$
- $\Sigma_k$ is the weighted covariance around the component mean

**Iteration:** Repeat E and M steps until convergence (log-likelihood stops improving).

## Bayesian Estimation and MAP

Maximum Likelihood Estimation has a critical weakness: with limited data, it can be overconfident.

**The MLE Problem:**
- If you flip a coin 3 times and get 3 heads, MLE confidently says $\hat{\theta} = 1$ (100% heads)
- But this ignores domain knowledge: coins are typically fair or close to fair

**Bayesian Solution:** Introduce a **prior** $p(\theta)$—your belief about the parameter before seeing data.

**Bayes' Rule:**

$$p(\theta | x) = \frac{p(x | \theta) p(\theta)}{p(x)} \propto p(x | \theta) p(\theta)$$

The **posterior** $p(\theta | x)$ combines:
- **Likelihood** $p(x | \theta)$: how well the data explains the parameters
- **Prior** $p(\theta)$: your initial belief about the parameters

**Maximum A Posteriori (MAP) Estimate:**

$$\theta_{MAP} = \arg\max_{\theta} p(\theta | x) = \arg\max_{\theta} p(x | \theta) p(\theta)$$

Taking logarithms:

$$\theta_{MAP} = \arg\max_{\theta} [\log p(x | \theta) + \log p(\theta)]$$

This is MLE plus a regularization term from the prior.

**Comparison:**
- **MLE:** Trusts only the data
- **MAP:** Balances data likelihood with prior belief
- **Small data regime:** MAP is more robust; prior prevents overfitting
- **Large data regime:** Both converge to similar estimates as data dominates

**Related Assignment Questions**: Q1-Q4

## Conjugate Priors: Beta-Bernoulli

To make Bayesian computation tractable, we use **conjugate priors**—priors whose posterior has the same functional form.

**Beta Distribution:**

The Beta distribution is parameterized by $(\alpha, \beta)$:

$$p(\theta) = \text{Beta}(\alpha, \beta) \propto \theta^{\alpha - 1} (1 - \theta)^{\beta - 1}$$

where $\theta \in (0, 1)$ (ideal for probability parameters).

**Bernoulli Likelihood:**

$$p(x | \theta) = \theta^k (1 - \theta)^{n - k}$$

where $k$ is the number of successes in $n$ trials.

**Posterior (Conjugacy):**

$$p(\theta | x) \propto p(x | \theta) p(\theta) = \theta^k (1 - \theta)^{n-k} \cdot \theta^{\alpha - 1} (1 - \theta)^{\beta - 1}$$

$$= \theta^{k + \alpha - 1} (1 - \theta)^{(n - k) + \beta - 1}$$

This is exactly a Beta distribution with updated parameters:

$$p(\theta | x) = \text{Beta}(k + \alpha, (n - k) + \beta)$$

**Parameter Update Rule:**
- Add successes to $\alpha$: $\alpha_{\text{posterior}} = \alpha + k$
- Add failures to $\beta$: $\beta_{\text{posterior}} = \beta + (n - k)$

**MAP Estimate (Mode of Beta):**

The mode (peak) of $\text{Beta}(\alpha', \beta')$ is at:

$$\theta_{MAP} = \frac{\alpha' - 1}{\alpha' + \beta' - 2}$$

For the posterior:

$$\theta_{MAP} = \frac{(k + \alpha) - 1}{(k + \alpha) + (n - k + \beta) - 2} = \frac{k + \alpha - 1}{n + \alpha + \beta - 2}$$

**Example:** Coin flip with $n = 10$ trials, $k = 7$ successes, prior Beta(3, 5):
- Posterior: Beta(7 + 3, 3 + 5) = Beta(10, 8)
- $\theta_{MAP} = \frac{10 - 1}{10 + 8 - 2} = \frac{9}{16} = 0.5625$
- Compare to MLE: $\hat{\theta}_{ML} = 7/10 = 0.70$
- The prior "pulls" the estimate toward 0.5 (the mean of Beta(3,5))

## Non-Parametric Density Estimation

Instead of assuming a specific distributional form (like Gaussian), we directly estimate density from data using local counting.

**Master Formula:**

$$p(x) = \frac{k}{n \cdot V}$$

where:
- $k$ = number of samples in a region around $x$
- $n$ = total number of samples
- $V$ = volume of the region

**Intuition:** Density at a point is proportional to how many nearby samples we have.

### Parzen Window Estimation

**Fixed Volume Approach:** Choose window size $V$ (e.g., a hypercube with side length $h$), then count nearby samples.

**Window Definition:** 
For a $d$-dimensional hypercube of side length $h$:

$$V = h^d$$

**Density Estimate:**

$$\hat{p}(x) = \frac{k}{n \cdot h^d}$$

where $k$ is the count of samples within distance $h/2$ from $x$ in each dimension.

**Example:** In $\mathbb{R}^2$ with $n = 500$ samples, window side length $h = 0.1$:
- Volume: $V = 0.1^2 = 0.01$
- If $k = 8$ samples fall in window: $\hat{p}(x) = 8 / (500 \cdot 0.01) = 8 / 5 = 1.6$

**Trade-off:**
- Larger $h$: Smoother estimate, less variance, more bias
- Smaller $h$: Rougher estimate, more variance, less bias

### k-Nearest Neighbors (k-NN) Density Estimation

**Adaptive Volume Approach:** Fix the number of neighbors $k$, then find the smallest region containing exactly $k$ samples.

**Density Estimate:**

$$\hat{p}(x) = \frac{k}{n \cdot V(x)}$$

where $V(x)$ is the volume of the smallest region containing $k$ samples around $x$.

**Example:** In $\mathbb{R}^2$ with $n = 2000$ samples, $k = 20$ nearest neighbors, region volume $V = 0.05$:

$$\hat{p}(x) = \frac{20}{2000 \cdot 0.05} = \frac{20}{100} = 0.20$$

**Advantage:** Automatically adapts region size to local data density:
- Dense regions: small $V$ (high density)
- Sparse regions: large $V$ (low density)

## k-NN Classification

k-NN density estimation directly extends to classification.

**Classification Rule:**

Given a test point $x$:
1. Find its $k$ nearest neighbors in the training data
2. Count how many neighbors belong to each class: $K_j$ for class $j$
3. Estimate class probability: $p(y_j | x) = \frac{K_j}{k}$
4. Predict: $\hat{y} = \arg\max_j p(y_j | x)$ (Bayes decision rule)

**Example:** Binary classification with $k = 11$ neighbors:
- Class 1: $K_1 = 5$ neighbors
- Class 2: $K_2 = 6$ neighbors
- Probabilities: $p(y_1 | x) = 5/11$, $p(y_2 | x) = 6/11$
- Prediction: Class 2 (higher probability)

**Decision Boundary:** k-NN creates a piecewise constant decision boundary that is highly non-linear and adapts to the data.

**Properties:**
- **No training:** Just stores all data (lazy learner)
- **Instance-based:** Each prediction uses nearest neighbors
- **Non-parametric:** No parameters to learn
- **Sensitive to $k$:** Small $k$ = complex boundary, large $k$ = simple boundary
- **Curse of dimensionality:** In high dimensions, distances become less meaningful

## Parzen vs k-NN: A Comparison

| Aspect | Parzen Window | k-NN |
|--------|---------------|------|
| **Volume** | Fixed $V = h^d$ | Adaptive $V(x)$ |
| **Count** | Adaptive $k(x)$ | Fixed $k$ |
| **Dense regions** | May have few points | Automatically many points |
| **Sparse regions** | May have many points | Automatically few points |
| **Parameter tuning** | Choose $h$ | Choose $k$ |
| **Computation** | Compute distance to all for each point | Same as Parzen |
| **Practical use** | Density estimation | Classification & regression |

## Summary

- **EM Convergence:** Guaranteed monotonic increase in likelihood; proven via ELBO
- **EM for GMM:** E-step computes responsibilities; M-step updates parameters with weighted averages
- **Bayesian Estimation:** Prior + likelihood → posterior; balances data and prior belief
- **MAP vs MLE:** MAP includes prior regularization; more robust with small data
- **Conjugate Priors:** Posterior has same form as prior (e.g., Beta-Bernoulli)
- **Beta-Bernoulli:** Update rule: add successes to $\alpha$, failures to $\beta$
- **Non-Parametric Density:** No assumption on distribution shape; count local samples
- **Parzen Windows:** Fixed window size, adaptive point count
- **k-NN Density:** Fixed point count, adaptive window size
- **k-NN Classification:** Classify based on majority vote of $k$ nearest neighbors

## Key Takeaways

| Concept | Definition | Example |
|---------|-----------|---------|
| **EM Convergence** | Each iteration increases likelihood; guaranteed via ELBO | Provably optimal algorithm |
| **EM for GMM** | E-step: soft assign; M-step: weighted parameter update | Finding clusters in data |
| **Prior Distribution** | Belief about parameters before seeing data | Beta(3,5) for coin bias |
| **Posterior Distribution** | Updated belief after observing data | Beta(10,8) after 7 successes in 10 |
| **MAP Estimate** | Mode of posterior; balances likelihood & prior | Regularized parameter estimate |
| **Conjugate Prior** | Posterior has same form as prior | Beta is conjugate to Bernoulli |
| **Beta-Bernoulli Update** | Add successes to $\alpha$, failures to $\beta$ | Counting successes/failures |
| **Master Formula** | $p(x) = k / (n \cdot V)$; count local density | Non-parametric estimation |
| **Parzen Window** | Fixed volume $V$; count samples $k$ inside | Kernel density estimation |
| **k-NN Density** | Fixed count $k$; find volume $V$ | Adaptive density estimation |
| **k-NN Classification** | Majority vote of $k$ neighbors; $p(y\|x) = K_y / k$ | Instance-based learner |

# Week 4: Theoretical Notes — MLE and EM

## Overview

This week extends MLE to practical scenarios and introduces the **Expectation-Maximization (EM) algorithm**—one of the most powerful techniques in machine learning. We apply MLE to Gaussian and discrete distributions, discover the limitations of single parametric models through mixture models, and solve the intractable log-likelihood problem using EM. The key mathematical tool is **Jensen's inequality** and the insight that we can construct lower bounds (ELBO) that are easier to optimize.

## MLE for Gaussian Distribution

When data is assumed to follow a Gaussian (normal) distribution, MLE yields intuitive results.

**Univariate Gaussian Model:**

$$X \sim \mathcal{N}(\mu, \sigma^2)$$

The log-likelihood for i.i.d. samples $\{v_1, v_2, \ldots, v_N\}$ is:

$$\ell(\mu, \sigma^2) = -\frac{N}{2} \log(2\pi\sigma^2) - \frac{1}{2\sigma^2} \sum_{i=1}^{N} (v_i - \mu)^2$$

**MLE Solution:**

By taking derivatives and setting them to zero:

$$\hat{\mu}_{MLE} = \frac{1}{N} \sum_{i=1}^{N} v_i \quad \text{(sample mean)}$$

$$\hat{\sigma}^2_{MLE} = \frac{1}{N} \sum_{i=1}^{N} (v_i - \hat{\mu})^2 \quad \text{(sample variance)}$$

**Intuition:** The MLE estimates are exactly the empirical statistics we compute from data—no surprise here, just mathematical validation that the obvious choices are optimal.

**Related Assignment Questions**: Q1-Q2 (review)

## MLE for Generalized Discrete Random Variable

When data consists of discrete categories, we model it as a categorical (multinomial) distribution.

**Categorical Distribution:**

A random variable takes values in $\{1, 2, \ldots, K\}$ with probabilities $\theta = (\theta_1, \theta_2, \ldots, \theta_K)$ where:
- $\theta_k \geq 0$ for all $k$
- $\sum_{k=1}^{K} \theta_k = 1$

Given $N$ i.i.d. samples with counts $n_k$ (number of times category $k$ appears):

$$\sum_{k=1}^{K} n_k = N$$

**Log-Likelihood:**

$$\ell(\theta) = \sum_{k=1}^{K} n_k \log \theta_k$$

**MLE Solution using Lagrange Multipliers:**

To maximize $\ell(\theta)$ subject to the constraint $\sum_k \theta_k = 1$, we form the Lagrangian:

$$L(\theta, \lambda) = \sum_{k=1}^{K} n_k \log \theta_k + \lambda \left( \sum_{k=1}^{K} \theta_k - 1 \right)$$

Taking derivatives:

$$\frac{\partial L}{\partial \theta_k} = \frac{n_k}{\theta_k} + \lambda = 0 \quad \Rightarrow \quad \theta_k = -\frac{n_k}{\lambda}$$

Substituting back into the constraint:

$$\sum_{k=1}^{K} \theta_k = -\frac{1}{\lambda} \sum_{k=1}^{K} n_k = -\frac{N}{\lambda} = 1 \quad \Rightarrow \quad \lambda = -N$$

Therefore:

$$\hat{\theta}_k = \frac{n_k}{N}$$

**Intuition:** The MLE probability of each category is simply its **empirical frequency**—the proportion of times it appears in the data.

**Example:** If we observe (2, 8, 5, 5) occurrences out of N=20 samples:
$$\hat{\theta} = (2/20, 8/20, 5/20, 5/20) = (0.10, 0.40, 0.25, 0.25)$$

**Related Assignment Questions**: Q7, Q8, Q10

## When One Distribution Isn't Enough: Mixture Models

Real-world data often has **multiple peaks** (multimodal structure). A single parametric distribution fails to capture this complexity.

**The Problem:** Consider human height data:
- Men's heights cluster around 175 cm
- Women's heights cluster around 165 cm
- A single Gaussian would place its peak near 170 cm, where few people actually exist

**Gaussian Mixture Model (GMM):**

Instead of one Gaussian, we use a weighted sum of $J$ Gaussians:

$$p_{\theta}(x) = \sum_{j=1}^{J} \alpha_j \mathcal{N}(x; \mu_j, \Sigma_j)$$

Where:
- $\alpha_j \geq 0$ are **mixing weights** with $\sum_j \alpha_j = 1$
- $\mathcal{N}(x; \mu_j, \Sigma_j)$ is the $j$-th Gaussian component with mean $\mu_j$ and covariance $\Sigma_j$

**Interpretation:**
- Each component represents a distinct cluster or mode in the data
- The weight $\alpha_j$ indicates the proportion of data from component $j$
- The overall density is the weighted superposition of the components

**Why GMM is Powerful:**
- **Flexibility:** Can approximate any smooth distribution arbitrarily well with enough components
- **Interpretability:** Each component often corresponds to a meaningful cluster or subpopulation
- **Universality:** Considered a universal density approximator

**Challenge:** How do we estimate all parameters ($\alpha_j, \mu_j, \Sigma_j$) from data?

**Related Assignment Questions**: Q9

## Latent Variable Models

Often, the reason data has multiple modes is because of hidden, unobserved factors.

**Latent Variable ($Z$):**

A random variable $Z$ that is not directly observed but influences the observable data $X$.

**Joint Model:**

$$p_{\theta}(x, z) = p_{\theta}(z) \cdot p_{\theta}(x | z)$$

**Marginal Distribution (Key Challenge):**

The probability of observing $x$ alone requires summing/integrating over all possible values of the hidden $z$:

$$p_{\theta}(x) = \sum_{z} p_{\theta}(x, z) \quad \text{(discrete case)}$$

$$p_{\theta}(x) = \int p_{\theta}(x, z) \, dz \quad \text{(continuous case)}$$

**The Log-Likelihood Problem:**

To apply MLE, we need to maximize:

$$\ell(\theta) = \sum_{i=1}^{N} \log p_{\theta}(x_i) = \sum_{i=1}^{N} \log \sum_{z} p_{\theta}(x_i, z)$$

The **logarithm of a sum** is notoriously difficult to optimize. This is where EM comes in.

**Connection to GMM:**

A Gaussian Mixture Model can be viewed as a latent variable model where:
- $Z \in \{1, 2, \ldots, J\}$ is a discrete latent variable indicating component assignment
- $p(z = j) = \alpha_j$ (mixing weight)
- $p(x | z = j) = \mathcal{N}(x; \mu_j, \Sigma_j)$ (component density)
- The marginalized distribution is the mixture: $p(x) = \sum_j \alpha_j \mathcal{N}(x; \mu_j, \Sigma_j)$

**Related Assignment Questions**: Q1-Q6

## Jensen's Inequality and Lower Bounds

To solve the log-likelihood problem, we use a fundamental mathematical result.

**Jensen's Inequality for Concave Functions:**

For a concave function $f$ and any random variable $Y$:

$$f(\mathbb{E}[Y]) \geq \mathbb{E}[f(Y)]$$

**Why It Matters:** The logarithm is a concave function (curves upward). Therefore:

$$\log(\mathbb{E}[Y]) \geq \mathbb{E}[\log(Y)]$$

**Application to Log-Likelihood:**

For any distribution $q(z)$ over latent variables:

$$\log p_{\theta}(x) = \log \sum_{z} p_{\theta}(x, z)$$

$$= \log \sum_{z} q(z) \cdot \frac{p_{\theta}(x, z)}{q(z)}$$

$$= \log \mathbb{E}_{z \sim q}\left[\frac{p_{\theta}(x, z)}{q(z)}\right]$$

By Jensen's inequality:

$$\log p_{\theta}(x) \geq \mathbb{E}_{z \sim q}\left[\log \frac{p_{\theta}(x, z)}{q(z)}\right]$$

$$= \mathbb{E}_{z \sim q}[\log p_{\theta}(x, z)] - \mathbb{E}_{z \sim q}[\log q(z)]$$

**Related Assignment Questions**: Q3

## ELBO: Evidence Lower Bound

The right-hand side of the Jensen inequality is called the **Evidence Lower Bound (ELBO)**.

**Definition:**

$$\text{ELBO}(q) = \mathbb{E}_{z \sim q}[\log p_{\theta}(x, z) - \log q(z)]$$

**Key Property:**

$$\log p_{\theta}(x) = \text{ELBO}(q) + D_{KL}(q(z) \mid\mid p_{\theta}(z|x))$$

**Interpretation:**
- The true log-likelihood equals the ELBO plus a KL divergence term
- The KL divergence is always non-negative, so the ELBO is a **lower bound** on the true likelihood
- When $q(z) = p_{\theta}(z|x)$ (the true posterior), the KL term is zero and $\text{ELBO} = \log p_{\theta}(x)$

**Intuition:** The ELBO is a "tighter" approximation to the true likelihood when $q$ is closer to the true posterior. This motivates the EM algorithm.

**Related Assignment Questions**: Q5, Q6

## The Expectation-Maximization (EM) Algorithm

EM is a two-step iterative algorithm that monotonically increases the log-likelihood without ever computing the intractable sum/integral directly.

### E-Step: Expectation

Set the auxiliary distribution $q$ equal to the true posterior given current parameters:

$$q^{(t)}(z) = p_{\theta^{(t)}}(z | x)$$

This makes the ELBO perfectly tight against the true log-likelihood (KL divergence becomes zero):

$$\log p_{\theta^{(t)}}(x) = \text{ELBO}(q^{(t)})$$

### M-Step: Maximization

Maximize the ELBO with respect to the parameters:

$$\theta^{(t+1)} = \arg\max_{\theta} \text{ELBO}(q^{(t)}) = \arg\max_{\theta} \mathbb{E}_{z \sim q^{(t)}}[\log p_{\theta}(x, z) - \log q^{(t)}(z)]$$

Since $q^{(t)}$ doesn't depend on $\theta$:

$$\theta^{(t+1)} = \arg\max_{\theta} \mathbb{E}_{z \sim q^{(t)}}[\log p_{\theta}(x, z)]$$

### Convergence Guarantee

At each iteration, the log-likelihood increases (or stays the same):

$$\log p_{\theta^{(t+1)}}(x) \geq \log p_{\theta^{(t)}}(x)$$

The algorithm terminates when convergence is reached (log-likelihood stops increasing).

**Why It Works:**
1. E-step: Compute the tightest lower bound on the true likelihood
2. M-step: Maximize that lower bound
3. New likelihood is at least as good as the old bound, which was at least as good as the old likelihood
4. Repeat until convergence

**Related Assignment Questions**: Q5-Q6

## Summary

- **MLE for Gaussian:** Mean and variance are sample mean and sample variance
- **MLE for Categorical:** Each probability equals the empirical frequency $n_k / N$
- **Mixture Models:** Weighted sum of distributions handles multimodal data
- **Latent Variable Models:** Introduce unobserved $Z$ causing the observed $X$; marginalization over $Z$ creates log-sum challenge
- **Jensen's Inequality:** For concave log: $\log \mathbb{E}[Y] \geq \mathbb{E}[\log Y]$
- **ELBO:** Lower bound on log-likelihood; exact when $q(z) = p(z|x)$
- **EM Algorithm:** Iteratively E-step (tighten bound) then M-step (optimize parameters); guarantees monotonic increase in likelihood

## Key Takeaways

| Concept | Definition | Example |
|---------|-----------|---------|
| **MLE for Gaussian** | Mean = sample mean; variance = sample variance | Height data distributed normally |
| **MLE for Categorical** | $\hat{\theta}_k = n_k / N$ (empirical frequency) | Coin with 6 heads in 10 flips → p=0.6 |
| **Mixture Model** | $p(x) = \sum_j \alpha_j p_j(x)$; weighted sum of distributions | Bimodal height data (men + women) |
| **Gaussian Mixture Model** | $p(x) = \sum_j \alpha_j \mathcal{N}(x; \mu_j, \Sigma_j)$ | Modeling multiple clusters |
| **Latent Variable** | Unobserved $Z$ influencing observed $X$; need to marginalize | Hidden component causing mixture |
| **Log-Sum Problem** | $\log p(x) = \log \sum_z p(x,z)$ hard to optimize | Obstacle for direct MLE with latent variables |
| **Jensen's Inequality** | $\log \mathbb{E}[Y] \geq \mathbb{E}[\log Y]$ for concave log | Foundation for lower bounding likelihood |
| **ELBO** | $\mathbb{E}_q[\log p(x,z) - \log q(z)]$ bounds $\log p(x)$ | Tractable proxy for log-likelihood |
| **E-Step** | Set $q(z) = p(z\|x)$ to make ELBO tight | Makes lower bound perfect (KL = 0) |
| **M-Step** | Maximize ELBO w.r.t. parameters $\theta$ | Improves likelihood at each iteration |

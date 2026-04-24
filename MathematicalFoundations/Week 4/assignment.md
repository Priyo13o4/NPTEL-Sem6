# Week 4: Assignment 4 — Mathematical Foundations of Machine Learning (CS02)

| Metric | Value |
|--------|-------|
| Due | 2026-02-18, 23:59 IST |
| Submitted | — |
| Score | — |

---

## Question Set 1: KL Divergence Review and Properties

### Q1 (1 point)

**Which expression equals $D_{KL}(P \mid\mid Q)$ for discrete distributions?**

- [ ] A) $\sum_x Q(x) \log \frac{Q(x)}{P(x)}$
- [ ] B) $\sum_x P(x) \log \frac{P(x)}{Q(x)}$
- [ ] C) $\sum_x |P(x) - Q(x)|$
- [ ] D) $-\sum_x P(x) \log Q(x)$

**Answer:** B

**Answer Explanation:** This is the fundamental definition of **Kullback-Leibler Divergence** introduced in Week 3. It measures the expected logarithmic difference between the true probability distribution $P$ and an approximating distribution $Q$:

$$D_{KL}(P \mid\mid Q) = \sum_x P(x) \log \frac{P(x)}{Q(x)} = \mathbb{E}_{x \sim P}[\log P(x) - \log Q(x)]$$

This can also be written as:
$$D_{KL}(P \mid\mid Q) = H(P, Q) - H(P)$$

where $H(P,Q)$ is cross-entropy and $H(P)$ is entropy.

Incorrect options:
- **A)** Is the reverse direction $D_{KL}(Q \mid\mid P)$ (asymmetric)
- **C)** Is the $L_1$ distance, not a probabilistic divergence
- **D)** Is just the cross-entropy $H(P,Q)$, missing the entropy term

**Related Topics:** [Week 3 KL Divergence](../Week%203/notes.md#kl-divergence-measuring-distribution-distance)

---

### Q2 (1 point)

**Which statement is always true?**

- [ ] A) $D_{KL}(P \mid\mid Q) \leq 0$
- [ ] B) $D_{KL}(P \mid\mid Q) = D_{KL}(Q \mid\mid P)$
- [ ] C) $D_{KL}(P \mid\mid Q) \geq 0$ with equality iff $P = Q$
- [ ] D) $D_{KL}(P \mid\mid Q) = H(P)$

**Answer:** C

**Answer Explanation:** This statement captures the three fundamental properties of KL divergence:

1. **Non-negativity:** $D_{KL}(P \mid\mid Q) \geq 0$ always holds
   - Proof: By Jensen's inequality for the concave log function:
   $$D_{KL}(P \mid\mid Q) = \mathbb{E}_{x \sim P}[\log \frac{P(x)}{Q(x)}] \geq \log \mathbb{E}_{x \sim P}[\frac{P(x)}{Q(x)}] = \log 1 = 0$$

2. **Zero iff identical:** $D_{KL}(P \mid\mid Q) = 0$ if and only if $P = Q$ almost everywhere

3. **Distance-like property:** Measures how much $Q$ fails to capture $P$

Incorrect options:
- **A)** KL divergence is always non-negative, never negative
- **B)** KL is asymmetric; $D_{KL}(P \mid\mid Q) \neq D_{KL}(Q \mid\mid P)$ in general
- **D)** $D_{KL}(P \mid\mid Q) = H(P,Q) - H(P)$, not just $H(P)$

**Related Topics:** [Week 3 KL Divergence Properties](../Week%203/notes.md#kl-divergence-measuring-distribution-distance)

---

## Question Set 2: Jensen's Inequality and Mathematical Foundations

### Q3 (1 point)

**For a positive random variable Y and concave log(·), Jensen implies:**

- [ ] A) $\log \mathbb{E}[Y] \leq \mathbb{E}[\log Y]$
- [ ] B) $\log \mathbb{E}[Y] \geq \mathbb{E}[\log Y]$
- [ ] C) $\mathbb{E}[\log Y] \geq \mathbb{E}[Y]$
- [ ] D) $\log Y$ is convex so Jensen does not apply

**Answer:** B

**Answer Explanation:** **Jensen's Inequality** for a concave function $f$ states:

$$f(\mathbb{E}[Y]) \geq \mathbb{E}[f(Y)]$$

Since the natural logarithm is a concave function (curves upward, second derivative is negative), we have:

$$\log(\mathbb{E}[Y]) \geq \mathbb{E}[\log Y]$$

**Geometric Intuition:** If you draw a concave curve like the log function and pick any two points on it, the straight line connecting them (representing $\mathbb{E}[\log Y]$) lies *below* the curve itself. The curve value at the expected point $\log(\mathbb{E}[Y])$ is always higher.

Incorrect options:
- **A)** Is the reverse inequality (wrong direction)
- **C)** Is comparing log of $Y$ with $Y$ itself; unrelated to Jensen's inequality
- **D)** Log is concave, not convex; Jensen's inequality absolutely applies

**Related Topics:** [Jensen's Inequality and Lower Bounds](notes.md#jensens-inequality-and-lower-bounds)

---

### Q4 (1 point)

**If there exists an x such that P(x) > 0 but Q(x) = 0, then:**

- [ ] A) $D_{KL}(P \mid\mid Q) = 0$
- [ ] B) $D_{KL}(P \mid\mid Q)$ is finite and small
- [ ] C) $D_{KL}(P \mid\mid Q) = +\infty$
- [ ] D) $D_{KL}(Q \mid\mid P) = +\infty$

**Answer:** C

**Answer Explanation:** By the KL divergence definition:

$$D_{KL}(P \mid\mid Q) = \sum_x P(x) \log \frac{P(x)}{Q(x)}$$

If $P(x^*) > 0$ but $Q(x^*) = 0$, the term evaluates as:

$$P(x^*) \log \frac{P(x^*)}{0} = P(x^*) \log(\infty) = \infty$$

**Conceptual Interpretation:** Your model $Q$ claims that event $x^*$ is *absolutely impossible* (probability 0), but the true distribution $P$ proves it *can and does occur*. This is a catastrophic modeling failure—the penalty is infinite. The model $Q$ is completely unsuitable for approximating $P$.

**Practical Implication:** To avoid infinite KL divergence, any proposed model distribution $Q$ must have support that includes all outcomes where $P(x) > 0$.

Incorrect options:
- **A)** If $Q$ completely fails to model possible events, KL divergence is infinite, not zero
- **B)** KL divergence is infinite, not finite and small
- **D)** The reverse direction has $\log(0 / P(x^*)) = \log(0) = -\infty$, not $+\infty$. The forward direction is what's asked.

**Related Topics:** [Week 3 KL Divergence](../Week%203/notes.md#kl-divergence-measuring-distribution-distance)

---

## Question Set 3: ELBO and EM Foundation

### Q5 (1 point)

**For any distribution q(z), which decomposition is correct?**

- [ ] A) $\log p(x) = \mathbb{E}_q[\log p(x,z)] + D_{KL}(p(z|x) \mid\mid q(z))$
- [ ] B) $\log p(x) = \mathbb{E}_q[\log p(x,z) - \log q(z)]_{\text{ELBO}} + D_{KL}(q(z) \mid\mid p(z|x))$
- [ ] C) $\log p(x) = \mathbb{E}_q[\log p(z|x)] - H(q)$
- [ ] D) $\log p(x) = H(q) + D_{KL}(q \mid\mid p(z))$

**Answer:** B

**Answer Explanation:** This is the **fundamental decomposition** underlying the EM algorithm. It decomposes the true log-likelihood into two interpretable parts:

$$\log p(x) = \underbrace{\mathbb{E}_q[\log p(x,z) - \log q(z)]}_{\text{ELBO}(q)} + \underbrace{D_{KL}(q(z) \mid\mid p(z|x))}_{\text{KL divergence}}$$

**Derivation via Jensen's Inequality:**

Start with the log-likelihood:
$$\log p(x) = \log \sum_z p(x,z) = \log \sum_z q(z) \cdot \frac{p(x,z)}{q(z)} = \log \mathbb{E}_{z \sim q}[\frac{p(x,z)}{q(z)}]$$

By Jensen's inequality for the concave log function:
$$\log p(x) \geq \mathbb{E}_{z \sim q}[\log \frac{p(x,z)}{q(z)}] = \mathbb{E}_q[\log p(x,z) - \log q(z)]$$

The difference between the true likelihood and this lower bound is exactly the KL divergence:
$$\log p(x) - \mathbb{E}_q[\log p(x,z) - \log q(z)] = D_{KL}(q(z) \mid\mid p(z|x))$$

**Interpretation:**
- **ELBO:** Evidence Lower Bound; a lower bound on the true log-likelihood
- **KL term:** How far the auxiliary distribution $q(z)$ is from the true posterior $p(z|x)$
- When $q(z) = p(z|x)$, KL divergence is zero and ELBO equals the true likelihood

Incorrect options:
- **A)** Has the KL divergence in the wrong direction and missing the log $q(z)$ term
- **C)** Is incomplete and incorrect
- **D)** Lacks the proper decomposition structure

**Related Topics:** [ELBO: Evidence Lower Bound](notes.md#elbo-evidence-lower-bound)

---

### Q6 (1 point)

**When does $\log p(x) = \text{ELBO}(q)$ hold?**

- [ ] A) When $q(z)$ is uniform
- [ ] B) When $q(z) = p(z)$
- [ ] C) When $q(z) = p(z|x)$
- [ ] D) When $p(x)$ is Gaussian

**Answer:** C

**Answer Explanation:** From the decomposition in Q5:

$$\log p(x) = \text{ELBO}(q) + D_{KL}(q(z) \mid\mid p(z|x))$$

For the ELBO to equal the true log-likelihood, the KL divergence term must be zero:

$$D_{KL}(q(z) \mid\mid p(z|x)) = 0$$

By the properties of KL divergence (from Q2), this happens if and only if:

$$q(z) = p(z|x)$$

That is, when the auxiliary distribution $q$ exactly matches the true posterior distribution of the latent variable given the observed data.

**Why This Matters for EM:**
- In the **E-step**, we set $q(z) = p_\theta(z|x)$ (compute the posterior using current parameters)
- This makes the ELBO perfectly tight: $\log p(x) = \text{ELBO}(q)$
- In the **M-step**, we maximize this tight bound, which directly increases the true likelihood

Incorrect options:
- **A)** Uniform distribution generally doesn't match the posterior
- **B)** $p(z)$ is the prior; we need the posterior $p(z|x)$ that conditions on observed $x$
- **D)** The functional form of $p(x)$ is irrelevant; the decomposition holds for any $q$

**Related Topics:** [The Expectation-Maximization (EM) Algorithm](notes.md#the-expectation-maximization-em-algorithm)

---

## Question Set 4: MLE for Discrete Distributions

### Q7 (1 point)

**A categorical variable takes values in {1,2,3,4}. In N=20 samples, counts are (n₁,n₂,n₃,n₄) = (2,8,5,5). What is the MLE of (p₁,p₂,p₃,p₄)?**

- [ ] A) $(0.10, 0.40, 0.25, 0.25)$
- [ ] B) $(0.20, 0.80, 0.50, 0.50)$
- [ ] C) $(0.12, 0.38, 0.25, 0.25)$
- [ ] D) $(0.08, 0.42, 0.25, 0.25)$

**Answer:** A

**Answer Explanation:** For a categorical distribution, the MLE of each probability is simply the **empirical frequency** of that category:

$$\hat{p}_k = \frac{n_k}{N}$$

Computing for each category:
- $\hat{p}_1 = 2/20 = 0.10$
- $\hat{p}_2 = 8/20 = 0.40$
- $\hat{p}_3 = 5/20 = 0.25$
- $\hat{p}_4 = 5/20 = 0.25$

Verification: $0.10 + 0.40 + 0.25 + 0.25 = 1.00$ ✓

**Intuition:** If category 1 appeared only 2 out of 20 times, the most likely probability is 10%. This is the most straightforward and intuitive estimator—what you'd naturally guess from data.

Incorrect options:
- **B)** Doubles some values; incorrect calculation
- **C)** Has decimal approximations with no mathematical justification
- **D)** Contains slightly adjusted values for no reason

**Related Topics:** [MLE for Generalized Discrete Random Variable](notes.md#mle-for-generalized-discrete-random-variable)

---

### Q8 (1 point)

**To maximize $\sum_{j=1}^K n_j \log p_j$ subject to $\sum_{j=1}^K p_j = 1$, the Lagrangian is $L(p,\lambda) = \sum_{j=1}^K n_j \log p_j + \lambda(\sum_{j=1}^K p_j - 1)$. Setting $\partial L/\partial p_j = 0$ gives which relation?**

- [ ] A) $p_j = \lambda / n_j$
- [ ] B) $p_j = -n_j / \lambda$
- [ ] C) $p_j = \lambda \cdot n_j$
- [ ] D) $p_j = -\lambda \log n_j$

**Answer:** B

**Answer Explanation:** Taking the partial derivative of the Lagrangian with respect to $p_j$:

$$\frac{\partial L}{\partial p_j} = \frac{\partial}{\partial p_j}[n_j \log p_j] + \frac{\partial}{\partial p_j}[\lambda p_j]$$

$$= \frac{n_j}{p_j} + \lambda$$

Setting equal to zero:

$$\frac{n_j}{p_j} + \lambda = 0$$

$$\frac{n_j}{p_j} = -\lambda$$

$$p_j = -\frac{n_j}{\lambda}$$

To find $\lambda$, use the constraint $\sum_j p_j = 1$:

$$\sum_{j=1}^K p_j = -\frac{1}{\lambda} \sum_{j=1}^K n_j = -\frac{N}{\lambda} = 1$$

$$\lambda = -N$$

Therefore:

$$p_j = -\frac{n_j}{-N} = \frac{n_j}{N}$$

This recovers the empirical frequency formula from Q7.

**Technique:** Lagrange multipliers are used to optimize a function subject to equality constraints. The constraint ensures probabilities sum to 1.

Incorrect options:
- **A)** Inverted the relationship
- **C)** Wrong sign and relationship structure
- **D)** Mixing in unrelated log function

**Related Topics:** [MLE for Generalized Discrete Random Variable](notes.md#mle-for-generalized-discrete-random-variable)

---

## Question Set 5: Mixture Models and Latent Variables

### Q9 (1 point)

**A dataset clearly has two separate peaks (two clusters) along "x". A single Gaussian fits poorly. Which model class is most appropriate from the covered topics?**

- [ ] A) Single Gaussian $\mathcal{N}(\mu, \sigma^2)$
- [ ] B) Gaussian Mixture Model (GMM)
- [ ] C) Uniform distribution on an interval
- [ ] D) Single exponential distribution

**Answer:** B

**Answer Explanation:** When data exhibits **multimodal structure** (multiple distinct peaks), a single parametric distribution cannot capture the shape.

**Why GMM is appropriate:**

A **Gaussian Mixture Model** is:

$$p(x) = \sum_{j=1}^{J} \alpha_j \mathcal{N}(x; \mu_j, \Sigma_j)$$

With $J=2$ components, this can represent:
- Two clusters (each Gaussian component fits one cluster)
- Two distinct populations (e.g., men's vs. women's heights)
- Multimodal data from any combination of causes

**Advantages:**
- **Flexible:** Each component captures one mode
- **Identifiable:** Weights $\alpha_j$ show mixture proportions
- **Powerful:** Can approximate any smooth distribution with enough components

**Practical Example:** If data shows clear bimodal structure (heights from two age groups), a 2-component GMM will fit much better than a single Gaussian, which would place its peak at the dip between the two modes.

Incorrect options:
- **A)** A single Gaussian is unimodal (one peak); it cannot fit bimodal data
- **C)** Uniform distribution has no peaks; completely wrong for peaked data
- **D)** Exponential distribution is highly skewed; doesn't fit symmetric multimodal data

**Related Topics:** [When One Distribution Isn't Enough: Mixture Models](notes.md#when-one-distribution-isnt-enough-mixture-models)

---

### Q10 (1 point)

**A discrete RV takes values in {1,…,K}. Data yields counts nₖ for category k with ∑ₖ₌₁ᴷ nₖ = n. The multinomial parameter is θ = (θ₁,…,θK) with θₖ ≥ 0 and ∑ₖ θₖ = 1. What is the MLE?**

- [ ] A) $\hat{\theta}_k = 1/K$
- [ ] B) $\hat{\theta}_k = n_k / n$
- [ ] C) $\hat{\theta}_k = n / n_k$
- [ ] D) $\hat{\theta}_k \propto \log n_k$

**Answer:** B

**Answer Explanation:** This is the **multinomial MLE**—the generalized form of Q7.

For a multinomial distribution with parameters $\theta = (\theta_1, \ldots, \theta_K)$, the log-likelihood is:

$$\ell(\theta) = \sum_{k=1}^{K} n_k \log \theta_k$$

By the same Lagrange multiplier optimization (as in Q8), the MLE is:

$$\hat{\theta}_k = \frac{n_k}{n}$$

where $n_k$ is the count of category $k$ and $n = \sum_k n_k$ is the total number of samples.

**Verification:** Each probability is a proportion; $\sum_k \hat{\theta}_k = \sum_k (n_k / n) = (1/n) \sum_k n_k = n/n = 1$ ✓

**Why This Makes Sense:**
- Category with more observations gets higher probability
- Simple, intuitive, and optimal in the MLE sense
- Works for any number of categories $K$
- Foundation for many discrete data models (multinomial regression, language models, etc.)

Incorrect options:
- **A)** $1/K$ is the uniform distribution (ignores data)
- **C)** Reciprocal relationship is backwards
- **D)** Log of counts has no theoretical or practical justification

**Related Topics:** [MLE for Generalized Discrete Random Variable](notes.md#mle-for-generalized-discrete-random-variable)

---

## Answer Key Summary

| Q | Answer | Concept | Difficulty |
|---|--------|---------|-----------|
| Q1 | B | KL divergence definition | Easy |
| Q2 | C | KL properties (non-negative, zero iff equal) | Easy |
| Q3 | B | Jensen's inequality for concave log | Medium |
| Q4 | C | KL divergence infinite for support mismatch | Medium |
| Q5 | B | ELBO decomposition (KL + lower bound) | Hard |
| Q6 | C | ELBO tight when q = posterior | Hard |
| Q7 | A | Categorical MLE = empirical frequency | Easy |
| Q8 | B | Lagrange multiplier calculus for constrained optimization | Hard |
| Q9 | B | GMM for multimodal data | Easy |
| Q10 | B | Multinomial MLE = proportions | Easy |

**Overall Score:** 10/10 points possible

---

## Concept Map

**Week 4 builds on Week 3 by:**
1. Reviewing KL divergence and Jensen's inequality
2. Introducing ELBO as a lower bound on log-likelihood
3. Applying MLE to practical distributions (Gaussian, categorical, multinomial)
4. Introducing mixture models to handle multimodal data
5. Laying groundwork for EM algorithm through ELBO decomposition

**Key Connection:** The EM algorithm works because at each step, we:
- **E-step:** Make ELBO tight (set $q = p(z|x)$, KL = 0)
- **M-step:** Maximize the tight bound (increase true likelihood)
- **Guarantee:** Each iteration increases true likelihood monotonically

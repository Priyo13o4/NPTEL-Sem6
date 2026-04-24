# Week 3: Assignment 3 — Mathematical Foundations of Machine Learning (CS02)

| Metric | Value |
|--------|-------|
| Due | 2026-02-11, 23:59 IST |
| Submitted | — |
| Score | — |

---

## Question Set 1: Entropy and Information Theory

### Q1 (1 point)

**Let P be a discrete distribution over {a, b, c, d} with P(a) = 1/2, P(b) = 1/4, P(c) = 1/8, P(d) = 1/8. Using log₂, the entropy H(P) equals:**

- [ ] A) 1.5
- [ ] B) 1.75
- [ ] C) 2
- [ ] D) 2.25

**Answer:** B (Mathematically correct answer is **1.75**)

**⚠️ Platform Note:** The NPTEL platform marks this as correct answer = 2, which is mathematically incorrect. The true entropy calculation is 1.75 bits, as shown below.

**Answer Explanation:** Entropy is calculated as $H(P) = -\sum P(x) \log_2 P(x)$. Computing for each state:

- For $a$: $-\frac{1}{2} \log_2(1/2) = -\frac{1}{2} \cdot (-1) = 0.5$
- For $b$: $-\frac{1}{4} \log_2(1/4) = -\frac{1}{4} \cdot (-2) = 0.5$
- For $c$: $-\frac{1}{8} \log_2(1/8) = -\frac{1}{8} \cdot (-3) = 0.375$
- For $d$: $-\frac{1}{8} \log_2(1/8) = -\frac{1}{8} \cdot (-3) = 0.375$

**Total:** $0.5 + 0.5 + 0.375 + 0.375 = 1.75$ bits

The platform likely confused the actual entropy of this distribution with the maximum entropy for a 4-state system (which is $\log_2(4) = 2$, achieved when all states are equally likely).

**Related Topics:** [Entropy: Measuring Information](notes.md#entropy-measuring-information)

---

## Question Set 2: Cross-Entropy and KL Divergence

### Q2 (1 point)

**You are using a model "Q" to encode data generated from "P". The average code-length (in nats) achieved by using an optimal code for "Q" on samples from "P" is:**

- [ ] A) $H(P)$
- [ ] B) $H(Q)$
- [ ] C) $H(P, Q)$
- [ ] D) $D_{KL}(Q \mid\mid P)$

**Answer:** C

**Answer Explanation:** This is the literal definition of **cross-entropy** $H(P, Q) = -\sum P(x) \log Q(x)$. It measures the average number of bits (or nats, if using natural logarithm) needed to encode data drawn from the true distribution $P$ when you design your encoding scheme optimally for an incorrect distribution $Q$.

Incorrect options:
- **A)** $H(P)$ is the entropy of the true distribution—the *minimum* possible code-length if you perfectly knew $P$ and designed your code around it. But you didn't; you used $Q$ instead, so your code-length is longer.
- **B)** $H(Q)$ is the entropy of your model distribution, but this doesn't directly tell you how your code performs on data from $P$.
- **D)** $D_{KL}(Q \mid\mid P)$ is the KL divergence (asymmetric direction). It represents the *penalty* or *extra* code-length, not the total code-length.

**Related Topics:** [Cross-Entropy: Distance Between Distributions](notes.md#cross-entropy-distance-between-distributions)

---

### Q3 (1 point)

**Which identity is always true for discrete P, Q with Q(x) > 0 whenever P(x) > 0?**

- [ ] A) $H(P, Q) = H(Q) + D_{KL}(P \mid\mid Q)$
- [ ] B) $H(P, Q) = H(P) + D_{KL}(P \mid\mid Q)$
- [ ] C) $H(P) = H(P, Q) + D_{KL}(P \mid\mid Q)$
- [ ] D) $D_{KL}(P \mid\mid Q) = H(P, Q) + H(P)$

**Answer:** B

**Answer Explanation:** This fundamental identity decomposes the total code-length into two components:

$$H(P, Q) = H(P) + D_{KL}(P \mid\mid Q)$$

**Interpretation:**
- $H(P, Q)$ = **Total code-length** using distribution $Q$ on data from $P$
- $H(P)$ = **Minimum possible code-length** using the true distribution $P$
- $D_{KL}(P \mid\mid Q)$ = **Extra penalty** incurred by guessing wrong

This beautifully connects all three information theory concepts: the unavoidable minimum cost plus the extra cost of approximation.

Incorrect options:
- **A)** Uses $H(Q)$ instead of $H(P)$; the decomposition must be relative to the true distribution
- **C)** Reverses the decomposition logically
- **D)** Has the sum backward; KL divergence is the difference $H(P,Q) - H(P)$, not a sum

**Related Topics:** [KL Divergence: Measuring Distribution Distance](notes.md#kl-divergence-measuring-distribution-distance)

---

### Q4 (1 point)

**Suppose there exists x* such that P(x*) > 0 but Q(x*) = 0. Then:**

- [ ] A) $H(P, Q) = H(P)$
- [ ] B) $D_{KL}(P \mid\mid Q) = 0$
- [ ] C) $D_{KL}(P \mid\mid Q) = +\infty$
- [ ] D) $D_{KL}(Q \mid\mid P) = +\infty$

**Answer:** C

**Answer Explanation:** The KL divergence formula is:

$$D_{KL}(P \mid\mid Q) = \sum_{x} P(x) \log \frac{P(x)}{Q(x)}$$

If there exists $x^*$ where $P(x^*) > 0$ but $Q(x^*) = 0$, then the term $\log(P(x^*) / 0)$ diverges to infinity, making the entire KL divergence infinite.

**Conceptual Interpretation:** Your model $Q$ claims an event is *absolutely impossible* (assigns it probability 0), but the true distribution $P$ proves it *can and does occur*. This is a catastrophic modeling failure—the penalty for such a fundamental error is infinite. In practice, this means $Q$ is completely unsuitable as an approximation to $P$.

Incorrect options:
- **A)** The cross-entropy $H(P, Q) = -\sum P(x) \log Q(x)$ also diverges when $Q(x^*) = 0$
- **B)** KL divergence is definitely not zero (it's infinite)
- **D)** The reverse direction $D_{KL}(Q \mid\mid P)$ would involve $\log(Q(x) / P(x))$. Since $Q(x^*) = 0$ and $P(x^*) > 0$, this term is $\log 0$, which is $-\infty$, not $+\infty$. The question asks about the forward direction.

**Related Topics:** [KL Divergence: Measuring Distribution Distance](notes.md#kl-divergence-measuring-distribution-distance)

---

## Question Set 3: Maximum Likelihood Estimation

### Q5 (1 point)

**Let p be the true data distribution and {p_θ} a parametric model. Minimizing D_KL(p‖p_θ) over θ is equivalent to:**

- [ ] A) Minimizing $\mathbb{E}_{v \sim p}[\log p_{\theta}(v)]$
- [ ] B) Maximizing $\mathbb{E}_{v \sim p}[\log p_{\theta}(v)]$
- [ ] C) Minimizing $\mathbb{E}_{v \sim p_{\theta}}[\log p(v)]$
- [ ] D) Maximizing $\mathbb{E}_{v \sim p_{\theta}}[\log p(v)]$

**Answer:** B

**Answer Explanation:** Using the fundamental identity from Q3:

$$D_{KL}(p \mid\mid p_{\theta}) = H(p, p_{\theta}) - H(p) = -\mathbb{E}_{v \sim p}[\log p_{\theta}(v)] - H(p)$$

Since $H(p)$ is a constant (doesn't depend on $\theta$), minimizing $D_{KL}(p \mid\mid p_{\theta})$ is equivalent to maximizing $\mathbb{E}_{v \sim p}[\log p_{\theta}(v)]$.

**Interpretation:** To find the best parametric model, we maximize the expected log-likelihood—choose parameters that make observed data as probable as possible.

Incorrect options:
- **A)** Minimizing log-likelihood goes in the wrong direction
- **C)** and **D)** Use the expectation under $p_{\theta}$ rather than $p$; they're backwards

**Related Topics:** [Maximum Likelihood Estimation (MLE)](notes.md#maximum-likelihood-estimation-mle)

---

### Q6 (1 point)

**Given i.i.d. samples {vᵢ}ᵢ₌₁ᴺ ∼ p, the most direct empirical approximation to E_{v∼p}[log p_θ(v)] is:**

- [ ] A) $\log\left(\sum_{i=1}^{N} p_{\theta}(v_i)\right)$
- [ ] B) $\sum_{i=1}^{N} \log p_{\theta}(v_i)$
- [ ] C) $\frac{1}{N} \sum_{i=1}^{N} \log p_{\theta}(v_i)$
- [ ] D) $\prod_{i=1}^{N} \log p_{\theta}(v_i)$

**Answer:** C

**Answer Explanation:** This is a direct application of the **Law of Large Numbers**. The true expected value (a theoretical, abstract average over an infinite population) is approximated by the empirical average (the actual average of your finite sample):

$$\mathbb{E}_{v \sim p}[\log p_{\theta}(v)] \approx \frac{1}{N} \sum_{i=1}^{N} \log p_{\theta}(v_i)$$

You sum the log-likelihoods of all samples and divide by the number of samples $N$.

Incorrect options:
- **A)** Taking log of the sum is not the same as averaging the log
- **B)** Omits the normalization factor $1/N$; this is the sum, not the average
- **D)** Multiplying log values together is meaningless in this context

**Related Topics:** [Maximum Likelihood Estimation (MLE)](notes.md#maximum-likelihood-estimation-mle)

---

### Q7 (1 point)

**Assume p_θ(y|x) = N(y; h_θ(x), σ²) with fixed σ² > 0 and i.i.d. data {(xᵢ, yᵢ)}. The MLE for θ is:**

- [ ] A) $\arg\min_{\theta} \sum_{i=1}^{N} |y_i - h_{\theta}(x_i)|$
- [ ] B) $\arg\min_{\theta} \sum_{i=1}^{N} (y_i - h_{\theta}(x_i))^2$
- [ ] C) $\arg\min_{\theta} \sum_{i=1}^{N} \log|y_i - h_{\theta}(x_i)|$
- [ ] D) $\arg\max_{\theta} \sum_{i=1}^{N} (y_i - h_{\theta}(x_i))^2$

**Answer:** B

**Answer Explanation:** Under the Gaussian regression model, the log-likelihood is:

$$\log p_{\theta}(y_i \mid x_i) = \text{const} - \frac{1}{2\sigma^2}(y_i - h_{\theta}(x_i))^2$$

To maximize the total log-likelihood $\sum_i \log p_{\theta}(y_i \mid x_i)$, we must minimize the squared error term:

$$\theta^* = \arg\min_{\theta} \sum_{i=1}^{N} (y_i - h_{\theta}(x_i))^2$$

**This is Least Squares Regression!** The connection emerges naturally from the Gaussian assumption and MLE. This is one of the most important derivations in machine learning.

Incorrect options:
- **A)** Uses absolute error (L1 loss), which corresponds to a Laplacian model, not Gaussian
- **C)** Logarithm of absolute error doesn't correspond to any standard probability model
- **D)** Maximizing squared error is the opposite of what we want

**Related Topics:** [MLE for Gaussian Regression](notes.md#mle-for-gaussian-regression)

---

### Q8 (1 point)

**In the model of the previous question, if σ² is increased while kept fixed (not learned), then the MLE objective effectively:**

- [ ] A) Gives more weight to squared errors
- [ ] B) Gives less weight to squared errors
- [ ] C) Becomes independent of the squared errors
- [ ] D) Becomes ℓ₁ loss

**Answer:** B

**Answer Explanation:** The full MLE objective for Gaussian regression includes a variance term:

$$\theta^* = \arg\max_{\theta} \sum_{i=1}^{N} \left[\text{const} - \frac{1}{2\sigma^2}(y_i - h_{\theta}(x_i))^2\right]$$

Or equivalently:

$$\theta^* = \arg\min_{\theta} \frac{1}{2\sigma^2} \sum_{i=1}^{N} (y_i - h_{\theta}(x_i))^2$$

Notice the coefficient $\frac{1}{2\sigma^2}$ in front of the squared error sum. When $\sigma^2$ increases:
- The coefficient $\frac{1}{2\sigma^2}$ becomes smaller
- The squared errors are effectively multiplied by a smaller number
- The optimizer becomes less sensitive to prediction errors
- The model tolerates larger deviations from the data

**Intuition:** Higher variance means the model is more uncertain about the data, so it doesn't penalize errors as heavily.

Incorrect options:
- **A)** Increasing variance does the opposite
- **C)** The squared errors always matter; they're just weighted less
- **D)** The loss remains squared error, not L1

**Related Topics:** [MLE for Gaussian Regression](notes.md#mle-for-gaussian-regression)

---

## Question Set 4: Risk Minimization and Bayes Classifier

### Q9 (1 point)

**In binary classification with 0–1 loss, the Bayes-optimal decision rule predicts class 1 when:**

- [ ] A) $p(x \mid y=1) > p(x \mid y=0)$
- [ ] B) $p(y=1 \mid x) > p(y=0 \mid x)$
- [ ] C) $p(y=1) > p(y=0)$
- [ ] D) $D_{KL}(p(\cdot \mid y=1) \mid\mid p(\cdot \mid y=0)) > 0$

**Answer:** B

**Answer Explanation:** The **Bayes Classifier** (or Bayes-optimal classifier) for zero-one loss is:

$$h^*(x) = \begin{cases} 1 & \text{if } p(y=1 \mid x) > p(y=0 \mid x) \\ 0 & \text{otherwise} \end{cases}$$

**Why Optimal?** With zero-one loss, being wrong costs the same regardless of confidence. The best strategy is to predict the most probable class given the input. If 70% of samples with features $x$ have label 1, predicting 1 is correct 70% of the time. This achieves the lowest possible true risk (the Bayes risk).

Incorrect options:
- **A)** $p(x \mid y=1)$ is the likelihood (opposite direction). Using this would confuse data probability with label probability. By Bayes' rule: $p(y=1 \mid x) \propto p(x \mid y=1) p(y=1)$, but maximizing the likelihood without the prior is wrong.
- **C)** Prior probabilities $p(y=1)$ matter, but the decision should depend on the specific input $x$, not just the overall class frequencies.
- **D)** KL divergence between conditional distributions is not the criterion for classification.

**Related Topics:** [The Bayes Classifier](notes.md#the-bayes-classifier)

---

### Q10 (1 point)

**Which statement is correct?**

- [ ] A) Minimizing empirical risk $\hat{R}(h)$ always minimizes true risk $R(h)$ for any hypothesis class
- [ ] B) True risk is computed from the finite training sample, while empirical risk uses the population distribution
- [ ] C) Empirical risk is an approximation to true risk using the sample average of losses
- [ ] D) ERM replaces the loss function with KL divergence

**Answer:** C

**Answer Explanation:** This statement correctly describes the relationship between true and empirical risk.

**True Risk** (population risk):
$$R(h) = \mathbb{E}_{(x,y) \sim P_{XY}}[L(y, h(x))]$$
Expected loss over the true, unknown joint distribution of the entire population. We can never calculate it directly.

**Empirical Risk** (training risk):
$$\hat{R}(h) = \frac{1}{N} \sum_{i=1}^{N} L(y_i, h(x_i))$$
Average loss over the finite training dataset. Observable and computable.

By the Law of Large Numbers, as $N \rightarrow \infty$, $\hat{R}(h) \rightarrow R(h)$. So empirical risk is a finite-sample approximation to true risk.

Incorrect options:
- **A)** False (overfitting problem): minimizing $\hat{R}(h)$ does not guarantee low $R(h)$. A model can memorize training data (low empirical risk) but generalize poorly (high true risk).
- **B)** Backward: true risk uses the population distribution; empirical risk uses the finite sample.
- **D)** ERM uses whatever loss function you define (squared error, zero-one, cross-entropy, etc.). It does not inherently replace loss with KL divergence.

**Related Topics:** [Risk Minimization Framework](notes.md#risk-minimization-framework)

---

## Answer Key Summary

| Q | Answer | Concept | Difficulty |
|---|--------|---------|-----------|
| Q1 | 1.75* | Entropy calculation (platform error: says 2) | Easy |
| Q2 | C | Cross-entropy definition | Easy |
| Q3 | B | Fundamental identity: H(P,Q) = H(P) + D_KL | Medium |
| Q4 | C | KL divergence infinite for support mismatch | Medium |
| Q5 | B | KL minimization ↔ likelihood maximization | Hard |
| Q6 | C | Empirical approximation via Law of Large Numbers | Easy |
| Q7 | B | MLE with Gaussian → least squares | Hard |
| Q8 | B | Variance effect on squared error weight | Hard |
| Q9 | B | Bayes classifier: predict max posterior | Medium |
| Q10 | C | Empirical risk approximates true risk | Easy |

**Overall Score:** 9/10 points (or 10/10 if Q1 platform error is corrected)

*Q1 Note: Mathematically correct answer is **1.75 bits**, though the platform marks the accepted answer as 2.


# Mathematical Foundations of Machine Learning — Numerical Reference (Weeks 1–6)

**Course:** Mathematical Foundations of Machine Learning (CS02)  
**Scope:** Comprehensive numerical problems with step-by-step solutions  
**Last Updated:** April 2026

---

## Overview

This document is a **complete numerical reference** for Weeks 1–6, extracting every quantitative problem, formula, and calculation. Each entry includes:

1. **Topic Heading** — Conceptual context
2. **Formula & Components** — Mathematical definition
3. **Problem Statement** — Full question with numerical values
4. **Step-by-Step Calculation** — Detailed derivation
5. **Final Answer** — With interpretation
6. **Related Notes Link** — Reference to theoretical foundation

---

## Week 3: Entropy and Information Theory

### Information Content and Entropy Calculation

**Topic:** Discrete Entropy Computation

**Formula:**

$$H(P) = -\sum_{x} P(x) \log_2 P(x)$$

where $P(x)$ is the probability of state $x$, and $\log_2$ is the base-2 logarithm.

**Components:**
- $P(x)$: Probability of each outcome
- $\log_2$: Logarithm base 2 (information in bits)
- Negation: Ensures entropy is positive
- Summation: Total expected information across all states

---

#### Question 3.1

**Problem Statement:**

Let $P$ be a discrete distribution over $\{a, b, c, d\}$ with:
- $P(a) = 1/2$
- $P(b) = 1/4$
- $P(c) = 1/8$
- $P(d) = 1/8$

Using $\log_2$, calculate the entropy $H(P)$.

**Answer:** $1.75$ bits

**Calculation:**

For each state, compute $-P(x) \log_2 P(x)$:

**State $a$:**
$$-P(a) \log_2 P(a) = -\frac{1}{2} \log_2(1/2) = -\frac{1}{2} \times (-1) = 0.5 \text{ bits}$$

**State $b$:**
$$-P(b) \log_2 P(b) = -\frac{1}{4} \log_2(1/4) = -\frac{1}{4} \times (-2) = 0.5 \text{ bits}$$

**State $c$:**
$$-P(c) \log_2 P(c) = -\frac{1}{8} \log_2(1/8) = -\frac{1}{8} \times (-3) = 0.375 \text{ bits}$$

**State $d$:**
$$-P(d) \log_2 P(d) = -\frac{1}{8} \log_2(1/8) = -\frac{1}{8} \times (-3) = 0.375 \text{ bits}$$

**Total Entropy:**
$$H(P) = 0.5 + 0.5 + 0.375 + 0.375 = 1.75 \text{ bits}$$

**Interpretation:** 

The distribution requires an average of **1.75 bits** to encode messages from this source optimally. Since $\log_2(4) = 2$ bits would be needed for a uniform distribution over 4 outcomes, this distribution is slightly more efficient (entropy < 2) because $a$ is more probable.

**Related Topics:** [Entropy: Measuring Information](../Week%203/notes.md#entropy-measuring-information)

---

### Maximum Likelihood Estimation with Lagrange Multipliers

**Topic:** Optimizing Categorical Distribution Parameters

**Formula:**

Objective function:
$$\mathcal{L} = \sum_{j=1}^K n_j \log p_j$$

Constraint: $\sum_{j=1}^K p_j = 1$

Lagrangian:
$$L(p, \lambda) = \sum_{j=1}^K n_j \log p_j + \lambda\left(\sum_{j=1}^K p_j - 1\right)$$

**Components:**
- $n_j$: Number of observations in category $j$
- $p_j$: Probability of category $j$ (to be estimated)
- $\lambda$: Lagrange multiplier (enforces constraint)
- Constraint ensures probabilities sum to 1

---

#### Question 3.2

**Problem Statement:**

Maximize $\sum_{j=1}^K n_j \log p_j$ subject to $\sum_{j=1}^K p_j = 1$. 

Setting $\frac{\partial L}{\partial p_j} = 0$, derive the relation for $p_j$.

**Answer:** $p_j = -\frac{n_j}{\lambda}$

**Derivation:**

**Step 1: Take partial derivative**

$$\frac{\partial L}{\partial p_j} = \frac{\partial}{\partial p_j}[n_j \log p_j] + \frac{\partial}{\partial p_j}[\lambda p_j]$$

$$= \frac{n_j}{p_j} + \lambda$$

**Step 2: Set equal to zero**

$$\frac{n_j}{p_j} + \lambda = 0$$

$$\frac{n_j}{p_j} = -\lambda$$

**Step 3: Solve for $p_j$**

$$p_j = -\frac{n_j}{\lambda}$$

**Step 4: Find $\lambda$ using constraint**

Apply the constraint $\sum_{j=1}^K p_j = 1$:

$$\sum_{j=1}^K p_j = \sum_{j=1}^K \left(-\frac{n_j}{\lambda}\right) = -\frac{1}{\lambda} \sum_{j=1}^K n_j = 1$$

$$-\frac{N}{\lambda} = 1 \quad \text{(where } N = \sum_j n_j \text{)}$$

$$\lambda = -N$$

**Step 5: Recover empirical frequency**

Substitute $\lambda = -N$ back:

$$p_j = -\frac{n_j}{-N} = \frac{n_j}{N}$$

This is the **empirical frequency formula**: the MLE of each probability is the fraction of observations in that category.

**Interpretation:**

The Lagrange multiplier method elegantly incorporates the normalization constraint. The solution emerges as a simple ratio: each category's probability equals its observed proportion. This is the maximum likelihood estimate for a categorical distribution.

**Related Topics:** [MLE for Generalized Discrete Random Variable](../Week%204/notes.md#mle-for-generalized-discrete-random-variable)

---

## Week 4: Maximum Likelihood for Discrete Distributions

### Categorical Distribution MLE

**Topic:** Estimating Probabilities from Count Data

**Formula:**

For a categorical variable taking values in $\{1, 2, \ldots, K\}$:

$$\hat{p}_k = \frac{n_k}{N}$$

where $n_k$ is the count of category $k$ and $N = \sum_{j=1}^K n_j$ is total samples.

---

#### Question 4.1

**Problem Statement:**

A categorical variable takes values in $\{1, 2, 3, 4\}$. In $N = 20$ samples, the observed counts are:
- $(n_1, n_2, n_3, n_4) = (2, 8, 5, 5)$

Compute the MLE of $(p_1, p_2, p_3, p_4)$.

**Answer:** $(0.10, 0.40, 0.25, 0.25)$

**Calculation:**

For each category, apply $\hat{p}_k = \frac{n_k}{N}$:

**Category 1:**
$$\hat{p}_1 = \frac{n_1}{N} = \frac{2}{20} = 0.10$$

**Category 2:**
$$\hat{p}_2 = \frac{n_2}{N} = \frac{8}{20} = 0.40$$

**Category 3:**
$$\hat{p}_3 = \frac{n_3}{N} = \frac{5}{20} = 0.25$$

**Category 4:**
$$\hat{p}_4 = \frac{n_4}{N} = \frac{5}{20} = 0.25$$

**Verification:**

Check that probabilities sum to 1:
$$0.10 + 0.40 + 0.25 + 0.25 = 1.00 \quad \checkmark$$

**Interpretation:**

The most frequent category is category 2 (40% of samples). Categories 3 and 4 are equally likely (25% each). Category 1 is rarest (10%). These empirical frequencies form the best likelihood-based estimates of the true probabilities.

**Related Topics:** [MLE for Generalized Discrete Random Variable](../Week%204/notes.md#mle-for-generalized-discrete-random-variable)

---

## Week 5: Bayesian Estimation and Non-Parametric Density

### Bayesian MAP Estimation: Beta-Bernoulli

**Topic:** Combining Prior Beliefs with Observed Data

**Formula:**

For Beta-Bernoulli conjugacy with:
- Prior: $\theta \sim \text{Beta}(\alpha, \beta)$
- Data: $n$ Bernoulli trials with $k$ successes
- Posterior: $\theta \mid \text{data} \sim \text{Beta}(\alpha + k, \beta + n - k)$

**MAP Estimate (Mode of Posterior):**

$$\theta_{MAP} = \frac{\alpha' - 1}{\alpha' + \beta' - 2}$$

where $\alpha' = \alpha + k$ and $\beta' = \beta + n - k$.

**Components:**
- $\alpha$: Prior pseudo-count for successes
- $\beta$: Prior pseudo-count for failures
- $k$: Observed successes
- $n$: Total trials

---

#### Question 5.1

**Problem Statement:**

A Bernoulli parameter $\theta$ has prior $\theta \sim \text{Beta}(3, 5)$.

Observed data: $n = 10$ trials with $k = 7$ successes.

Compute $\theta_{MAP}$ using the formula: $\theta_{MAP} = \frac{k + \alpha - 1}{n + \alpha + \beta - 2}$.

**Answer:** $0.562$

**Calculation:**

**Step 1: Identify parameters**
- Prior: $\alpha = 3, \beta = 5$
- Data: $k = 7$ successes, $n = 10$ trials
- Posterior: $\alpha' = 3 + 7 = 10$, $\beta' = 5 + (10 - 7) = 8$

**Step 2: Apply MAP formula**

$$\theta_{MAP} = \frac{\alpha' - 1}{\alpha' + \beta' - 2} = \frac{10 - 1}{10 + 8 - 2}$$

$$= \frac{9}{16} = 0.5625 \approx 0.562$$

**Interpretation:**

- **Prior mean:** $\frac{\alpha}{\alpha + \beta} = \frac{3}{8} = 0.375$ (biased toward failures)
- **MLE (ignoring prior):** $\frac{k}{n} = \frac{7}{10} = 0.70$ (empirical rate)
- **MAP (posterior mode):** $0.562$ (compromise between prior and data)

The prior pulls the estimate back from the MLE (0.70) toward the prior belief (0.375), resulting in a moderate estimate of 0.562. This **regularization** is especially valuable with limited data.

**Related Topics:** [Bayesian Estimation and MAP](../Week%205/notes.md#bayesian-estimation-and-map), [Conjugate Priors: Beta-Bernoulli](../Week%205/notes.md#conjugate-priors-beta-bernoulli)

---

#### Question 5.2

**Problem Statement:**

A coin is tossed $n = 8$ times and shows $k = 6$ heads. Prior: $\theta \sim \text{Beta}(2, 2)$.

Compute both:
1. **MLE:** $\theta_{ML} = k/n$
2. **MAP:** $\theta_{MAP} = \frac{k + \alpha - 1}{n + \alpha + \beta - 2}$

Which pair is correct?

**Answer:** $(0.75, 0.70)$

**Calculation:**

**MLE (Maximum Likelihood Estimate):**

$$\theta_{ML} = \frac{k}{n} = \frac{6}{8} = 0.75$$

Pure data-driven; ignores prior.

**MAP (Maximum A Posteriori):**

- Prior: $\text{Beta}(2, 2)$ (symmetric, centered at 0.5)
- Posterior: $\text{Beta}(2 + 6, 2 + (8 - 6)) = \text{Beta}(8, 4)$

$$\theta_{MAP} = \frac{8 - 1}{8 + 4 - 2} = \frac{7}{10} = 0.70$$

**Comparison:**

| Estimator | Value | Interpretation |
|-----------|-------|---|
| MLE | 0.75 | 100% trust in observed data (6/8 heads) |
| MAP | 0.70 | Prior Beta(2,2) pulls toward 0.5 (fair coin) |
| Prior mean | 0.50 | Symmetric belief in fairness |

The prior $\text{Beta}(2, 2)$ regularizes the estimate, reducing the raw proportion 0.75 to a more conservative 0.70.

**Related Topics:** [Bayesian Estimation and MAP](../Week%205/notes.md#bayesian-estimation-and-map)

---

#### Question 5.3

**Problem Statement:**

Beta-Bernoulli conjugate update. Prior: $\theta \sim \text{Beta}(4, 6)$.

Observed data: $n = 20$ Bernoulli trials with $k = 9$ successes.

Find the posterior distribution.

**Answer:** $\text{Beta}(13, 17)$

**Calculation:**

**Conjugate Update Rule:**

For Beta-Bernoulli, the posterior parameters are:

$$\text{Posterior} = \text{Beta}(\alpha + k, \beta + (n - k))$$

**Step 1: Compute updated parameters**

- Prior parameters: $\alpha = 4, \beta = 6$
- Observed successes: $k = 9$
- Observed failures: $n - k = 20 - 9 = 11$

**Step 2: Apply update**

$$\alpha' = \alpha + k = 4 + 9 = 13$$
$$\beta' = \beta + (n - k) = 6 + 11 = 17$$

**Posterior:** $\text{Beta}(13, 17)$

**Interpretation:**

The conjugacy of Beta to Bernoulli makes updating trivial—just add counts. The posterior combines:
- Prior belief: Beta(4, 6) encodes 4 pseudo-successes and 6 pseudo-failures
- New data: 9 observed successes and 11 observed failures
- Result: Beta(13, 17) represents combined evidence

No integration, no optimization—just arithmetic!

**Related Topics:** [Conjugate Priors: Beta-Bernoulli](../Week%205/notes.md#conjugate-priors-beta-bernoulli)

---

#### Question 5.4

**Problem Statement:**

Observe $n = 4$ Bernoulli trials and all are successes ($k = 4$). Prior is $\text{Beta}(2, 5)$.

Compute both $\theta_{ML}$ and $\theta_{MAP}$.

**Answer:** $\theta_{ML} = 1.0$, $\theta_{MAP} = 5/9 \approx 0.556$

**Calculation:**

**MLE:**

$$\theta_{ML} = \frac{k}{n} = \frac{4}{4} = 1.0$$

The data shows 100% success rate. However, with only 4 trials, this is highly uncertain.

**MAP:**

- Prior: $\text{Beta}(2, 5)$ (strong belief toward failures; mean = 2/7 ≈ 0.286)
- Posterior: $\text{Beta}(2 + 4, 5 + 0) = \text{Beta}(6, 5)$

$$\theta_{MAP} = \frac{6 - 1}{6 + 5 - 2} = \frac{5}{9} \approx 0.556$$

**Dramatic Difference:**

| Estimator | Value | Interpretation |
|-----------|-------|---|
| MLE | 1.0 | 100% confidence in success (risky with $n=4$) |
| MAP | 0.556 | Prior doubts extreme observation; more reasonable |
| Prior mean | 0.286 | Strong prior toward failure |

**Why MAP is Preferred:**

With small $n = 4$, extreme observations like "all successes" are unreliable. The prior $\text{Beta}(2, 5)$ encodes domain knowledge that failures are likely. The MAP estimate (0.556) is a sensible compromise that prevents **overfitting** to extreme small-sample outcomes.

**Key Insight:** When data is limited, the prior is crucial for **regularization**.

**Related Topics:** [Bayesian Estimation and MAP](../Week%205/notes.md#bayesian-estimation-and-map)

---

### Non-Parametric Density Estimation: Parzen Windows

**Topic:** Estimating Density Without Parametric Assumptions

**Formula:**

Master formula for non-parametric density estimation:

$$p(x) = \frac{k}{n \cdot V}$$

where:
- $k$ = number of samples in the neighborhood of $x$
- $n$ = total number of training samples
- $V$ = volume of the neighborhood

For a $d$-dimensional hypercube with side length $h$:
$$V = h^d$$

**Components:**
- $k$: Count of samples inside window (determines local density)
- $n$: Normalization by total data size
- $V$: Volume normalization (ensures correct probability density)

---

#### Question 5.5

**Problem Statement:**

In a Parzen window with hypercube of side length $h$ in $d$ dimensions, compute the volume $V$.

Given: $d = 3$, $h = 0.2$

What is $V$?

**Answer:** $V = 0.008$

**Calculation:**

**Formula:** $V = h^d$

**Step 1: Substitute values**

$$V = (0.2)^3$$

**Step 2: Calculate power**

$$V = 0.2 \times 0.2 \times 0.2 = 0.008$$

**Interpretation:**

In 3D space with a small cube of side 0.2, the volume is only 0.008 cubic units. This reflects the **curse of dimensionality**: as dimension increases, neighborhoods become sparse, requiring either smaller windows (fewer samples) or larger data.

**Related Topics:** [Non-Parametric Density Estimation](../Week%205/notes.md#non-parametric-density-estimation), [Parzen Window Estimation](../Week%205/notes.md#parzen-window-estimation)

---

#### Question 5.6

**Problem Statement:**

Parzen window density estimation in $\mathbb{R}^2$.

Given:
- $n = 500$ total samples
- Dimension: $d = 2$
- Square window side length: $h = 0.1$
- Samples inside window: $k = 8$

Estimate $p(x)$ using the formula: $p(x) = \frac{k}{n \cdot h^d}$.

**Answer:** $p(x) = 1.6$

**Calculation:**

**Step 1: Compute volume**

$$V = h^d = (0.1)^2 = 0.01$$

**Step 2: Apply density formula**

$$p(x) = \frac{k}{n \cdot V} = \frac{8}{500 \times 0.01}$$

**Step 3: Simplify**

$$p(x) = \frac{8}{5} = 1.6$$

**Interpretation:**

The estimated density at $x$ is **1.6 samples per unit volume**. Since we have 8 samples in a window of volume 0.01 out of 500 total, this represents a region of **above-average density** (compared to the uniform density of 500/1 = 500 in total space).

**Verification:**

- Total samples: 500
- Total space (assuming unit square [0,1]²): 1
- Uniform density: 500 samples per unit volume
- Local density here: 1.6 (much sparser than uniform)

Wait, let me reconsider: The local density 1.6 is **lower** than global average because we're in a sparser region.

**Related Topics:** [Parzen Window Estimation](../Week%205/notes.md#parzen-window-estimation)

---

### k-NN Density Estimation and Classification

**Topic:** Adaptive Non-Parametric Density and Classification

**Formula:**

**Master formula (same as Parzen):**

$$p(x) = \frac{k}{n \cdot V}$$

**Key difference from Parzen:**
- **Parzen:** $V = h^d$ is **fixed**; $k$ varies
- **k-NN:** $k$ is **fixed**; $V$ is **adaptive** (chosen to capture exactly $k$ neighbors)

**k-NN Classification:**

For a test point $x$ with $k$ neighbors:

$$p(y_i | x) = \frac{K_i}{k}$$

where $K_i$ is the count of neighbors in class $i$.

**Bayes Decision Rule:**

$$\hat{y} = \arg\max_i p(y_i | x)$$

---

#### Question 5.7

**Problem Statement:**

k-NN density estimation in $\mathbb{R}^2$.

Given:
- $n = 2000$ total samples
- $k = 20$ (fixed number of neighbors)
- Adaptive region volume: $V = 0.05$

Estimate $p(x)$ using $p(x) = \frac{k}{n \cdot V}$.

**Answer:** $p(x) = 0.20$

**Calculation:**

**Step 1: Apply k-NN density formula**

$$p(x) = \frac{k}{n \cdot V} = \frac{20}{2000 \times 0.05}$$

**Step 2: Simplify**

$$p(x) = \frac{20}{100} = 0.20$$

**Interpretation:**

With 20 neighbors out of 2000 in a volume of 0.05, the local density is **0.20 samples per unit volume**.

**k-NN Adaptivity:**

Unlike Parzen (fixed window), k-NN automatically:
- Shrinks the window in **dense regions** (many neighbors close by → small $V$)
- Expands the window in **sparse regions** (few neighbors → larger $V$ needed to capture $k$)

This adaptive behavior prevents the fixed-window bias of Parzen.

**Related Topics:** [k-NN Density Estimation](../Week%205/notes.md#k-nearest-neighbors-k-nn-density-estimation)

---

#### Question 5.8

**Problem Statement:**

k-NN Classification. Around a test point $x$, you capture $k = 15$ neighbors with class counts:
- Class 1: $K_1 = 6$ neighbors
- Class 2: $K_2 = 5$ neighbors
- Class 3: $K_3 = 4$ neighbors

Compute $p(y_2 | x)$ using $p(y_i | x) = K_i / k$.

**Answer:** $p(y_2 | x) = 5/15 = 1/3 \approx 0.333$

**Calculation:**

**Formula:** $p(y_2 | x) = \frac{K_2}{k}$

**Substitute values:**

$$p(y_2 | x) = \frac{5}{15} = \frac{1}{3} \approx 0.333$$

**Interpretation:**

5 out of 15 neighbors belong to class 2, so the estimated probability that the test point belongs to class 2 is approximately **33.3%**.

**Full Probability Distribution:**

$$p(y_1 | x) = \frac{6}{15} = 0.40$$
$$p(y_2 | x) = \frac{5}{15} \approx 0.333$$
$$p(y_3 | x) = \frac{4}{15} \approx 0.267$$

Sum check: $0.40 + 0.333 + 0.267 = 1.0$ ✓

**Bayes Decision Rule:**

Since $p(y_1 | x) = 0.40$ is maximum, predict **class 1**.

**Related Topics:** [k-NN Classification](../Week%205/notes.md#k-nn-classification)

---

#### Question 5.9

**Problem Statement:**

Binary k-NN Classification. Around test point $x$, you find $k = 11$ neighbors:
- Class 1: $K_1 = 5$ neighbors
- Class 2: $K_2 = 6$ neighbors

Using the Bayes decision rule, what is the predicted class?

**Answer:** Class 2

**Calculation:**

**Step 1: Compute class probabilities**

$$p(y_1 | x) = \frac{K_1}{k} = \frac{5}{11} \approx 0.455$$

$$p(y_2 | x) = \frac{K_2}{k} = \frac{6}{11} \approx 0.545$$

**Step 2: Apply Bayes Decision Rule**

$$\hat{y} = \arg\max_j p(y_j | x)$$

$$= \arg\max(0.455, 0.545) = 2$$

**Step 3: Predict**

Predict **Class 2** because $p(y_2 | x) > p(y_1 | x)$.

**Interpretation:**

Class 2 has a slight majority (6 vs 5 neighbors), making it the most probable class. The k-NN classifier simply performs "neighbor voting"—choose the class with the most representatives.

**Confidence:**

The probabilities (54.5% vs 45.5%) show moderate confidence. A closer neighbor count (e.g., 6 vs 5) gives less confident decisions than a larger gap (e.g., 9 vs 2).

**Related Topics:** [k-NN Classification](../Week%205/notes.md#k-nn-classification)

---

#### Question 5.10

**Problem Statement:**

Parzen window volume scaling. In $\mathbb{R}^2$ with $n = 1000$ samples.

At location $x$, suppose $k = 10$ samples are inside the window when $h = 0.2$.

Hypothetically, if $k$ remained 10 and we changed $h = 0.1$, how does the density estimate $p(x) = \frac{k}{n \cdot h^d}$ change?

**Answer:** It becomes 4× larger.

**Calculation:**

**Original estimate (h = 0.2):**

$$V_{\text{old}} = (0.2)^2 = 0.04$$

$$p(x)_{\text{old}} = \frac{10}{1000 \times 0.04} = \frac{10}{40} = 0.25$$

**New estimate (h = 0.1):**

$$V_{\text{new}} = (0.1)^2 = 0.01$$

$$p(x)_{\text{new}} = \frac{10}{1000 \times 0.01} = \frac{10}{10} = 1.0$$

**Ratio of change:**

$$\frac{p(x)_{\text{new}}}{p(x)_{\text{old}}} = \frac{1.0}{0.25} = 4$$

**Derivation:**

When halving $h$ in $d$ dimensions:

$$V_{\text{new}} = (h/2)^d = \frac{h^d}{2^d} = \frac{V_{\text{old}}}{4 \text{ (for } d=2)}$$

Since $p(x) = \frac{k}{n \cdot V}$ and $V$ becomes 1/4:

$$p(x)_{\text{new}} = \frac{k}{n \cdot (V/4)} = 4 \times \frac{k}{n \cdot V} = 4 \times p(x)_{\text{old}}$$

**General Rule for $d$ Dimensions:**

If $h \to h/c$, then $p(x) \to p(x) \cdot c^d$

For $h \to h/2$ in $d$ dimensions: $p(x)$ increases by factor $2^d$

**Related Topics:** [Parzen Window Estimation](../Week%205/notes.md#parzen-window-estimation)

---

## Summary: Numerical Statistics by Week

### Problem Count

| Week | Total Questions | Numerical Problems | Focus |
|------|---|---|---|
| 1 | 10 | 0 | Foundational definitions |
| 2 | 10 | 0 | Distributions and axioms |
| 3 | 10 | 2 | Entropy, MLE optimization |
| 4 | 10 | 2 | Categorical MLE, KL divergence |
| 5 | 10 | 7 | Bayesian estimation, k-NN |
| 6 | 10 | 0 | Classification theory, loss functions |
| **Total** | **60** | **11** | Weeks 1-6 |

---

### Concepts and Formulas Quick Reference

**Information Theory:**
- Entropy: $H(P) = -\sum P(x) \log_2 P(x)$ (bits)
- KL Divergence: $D_{KL}(P \| Q) = \sum P(x) \log(P(x)/Q(x))$
- Cross-Entropy: $H(P, Q) = -\sum P(x) \log Q(x)$

**Bayesian Estimation:**
- MLE: $\hat{\theta} = \arg\max \sum_i \log p(y_i | x_i; \theta)$
- MAP: $\theta_{MAP} = \arg\max \log p(\theta) + \sum_i \log p(y_i | x_i; \theta)$
- Beta-Bernoulli posterior: $\text{Beta}(\alpha + k, \beta + n - k)$

**Non-Parametric Density:**
- Master formula: $p(x) = \frac{k}{n \cdot V}$
- Parzen: $V = h^d$ (fixed)
- k-NN: $k$ fixed, $V$ adaptive

**Classification:**
- k-NN probability: $p(y_i | x) = K_i / k$
- Bayes rule: $\hat{y} = \arg\max_j p(y_j | x)$

---

### Computational Checklist

**When solving numerical problems:**

1. **Identify the formula** — Which concept applies?
2. **Extract parameters** — What are $n$, $k$, $\alpha$, etc.?
3. **Substitute values** — Plug into formula step-by-step
4. **Simplify** — Arithmetic or algebraic reduction
5. **Verify** — Sanity checks (e.g., probabilities sum to 1?)
6. **Interpret** — What does the answer mean?

---

### Tricky Points and Common Errors

1. **Logarithm Base:** Entropy uses $\log_2$ (bits), MLE uses natural $\ln$ (nats). Don't mix them!

2. **Lagrange Multiplier Sign:** The constraint derivative adds to the Lagrangian with **plus** sign. Missing the minus in gradient descent is a classic mistake.

3. **Prior vs. Posterior:** Beta-Bernoulli posterior is $\text{Beta}(\alpha + k, \beta + n-k)$, not $(\alpha + k, \beta + k)$. The second parameter increments by **failures**, not successes.

4. **Parzen Volume Scaling:** When $h$ halves in $d$ dimensions, volume shrinks by $2^d$, so density **multiplies** by $2^d$. Volume is $h^d$, not $d \cdot h$.

5. **k-NN Independence:** In k-NN, the decision is independent of data scale or priors; it's purely local neighbor voting. Bayes rule (max probability) always applies.

---

## Topic-to-Formula Mapping

| Topic | Week | Formula | Usage |
|---|---|---|---|
| Entropy | 3 | $H(P) = -\sum P(x) \log_2 P(x)$ | Measure disorder/information |
| MLE Lagrange | 3 | $\frac{\partial L}{\partial p_j} = \frac{n_j}{p_j} + \lambda = 0$ | Optimize with constraints |
| Categorical MLE | 4 | $\hat{p}_k = n_k / N$ | Estimate category probabilities |
| MAP Beta | 5 | $\theta_{MAP} = \frac{\alpha' - 1}{\alpha' + \beta' - 2}$ | Bayesian point estimate |
| Parzen Volume | 5 | $V = h^d$ | Neighborhood volume |
| Density (Master) | 5 | $p(x) = k / (n \cdot V)$ | Non-parametric density |
| k-NN Probability | 5 | $p(y_i \| x) = K_i / k$ | Class probability by voting |
| Bayes Decision | 5 | $\hat{y} = \arg\max_j p(y_j \| x)$ | k-NN classification |

---

## Related Theoretical Resources

For detailed explanations of concepts, see:

- [Week 3 Notes: Information Theory](../Week%203/notes.md)
- [Week 4 Notes: MLE and KL Divergence](../Week%204/notes.md)
- [Week 5 Notes: Bayesian Methods & Non-Parametric Estimation](../Week%205/notes.md)

---

**Last Verified:** April 24, 2026  
**Status:** Complete Week 1-6 numerical reference  
**Next Steps:** Practice these problems repeatedly until confident with calculations and interpretations.
```


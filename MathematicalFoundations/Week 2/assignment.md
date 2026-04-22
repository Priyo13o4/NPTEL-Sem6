# Week 2: Assignment 2 — Mathematical Foundations of Machine Learning (CS02)

| Metric | Value |
|--------|-------|
| Due | 2026-02-11, 23:59 IST |
| Submitted | — |
| Score | — |

---

## Question Set 1: Joint and Vector-Valued Distributions

### Q1 (1 point)

**A vector-valued random variable is defined as:**

- [ ] A) $X: \mathbb{R}^d \rightarrow \Omega$
- [x] B) $X: \Omega \rightarrow \mathbb{R}^d$
- [ ] C) $X: \mathcal{F} \rightarrow \mathbb{R}^d$
- [ ] D) $X: \Omega \rightarrow \mathcal{F}$

**Answer:** B

**Answer Explanation:** A vector-valued random variable maps outcomes from the sample space $\Omega$ to a $d$-dimensional space of real numbers $\mathbb{R}^d$. This "translation" from abstract outcomes to numerical vectors is essential in machine learning, where each datapoint (image, medical record) is represented as a vector with $d$ numerical features.

Incorrect options:
- **A)** Is backward; random variables map from outcomes to numbers, not from numbers to outcomes
- **C)** $\mathcal{F}$ is the event space (sets of events), not the domain of a random variable
- **D)** Random variables map to numerical values ($\mathbb{R}^d$), not to the event space

**Related Topics:** [Joint Distributions](notes.md#joint-distributions), [Vector-Valued Random Variables](notes.md#joint-distributions)

---

### Q2 (1 point)

**Let $X_1: \Omega \rightarrow \mathbb{R}$ and $X_2: \Omega \rightarrow \mathbb{R}$ be two RVs on the same sample space. The joint distribution function $P_{X_1X_2}(a, b)$ corresponds to:**

- [ ] A) $P(X_1 = a \text{ or } X_2 = b)$
- [x] B) $P(X_1 \leq a, X_2 \leq b)$
- [ ] C) $P(X_1 \geq a, X_2 \geq b)$
- [ ] D) $P(X_1 = a, X_2 = b)$

**Answer:** B

**Answer Explanation:** This is the standard definition of the **Joint Cumulative Distribution Function (CDF)**. It gives the probability that $X_1$ is less than or equal to $a$ AND $X_2$ is less than or equal to $b$ simultaneously. This definition works for both discrete and continuous variables.

Incorrect options:
- **A)** Describes a union (either-or), not the joint cumulative distribution
- **C)** Represents the complementary survival function (probability of exceeding thresholds), not the CDF
- **D)** Is the Joint Probability Mass Function (PMF), valid only for discrete variables; the question asks for the general joint distribution function using "$\leq$" to accommodate continuity

**Related Topics:** [Joint Distributions](notes.md#joint-distributions)

---

## Question Set 2: Conditional Probability and Conditional Distributions

### Q3 (1 point)

**For events $A, B \in \mathcal{F}$ with $P(B) > 0$, conditional probability is:**

- [ ] A) $P(A \mid B) = P(A) \cdot P(B)$
- [ ] B) $P(A \mid B) = P(A) \cdot P(A)$
- [x] C) $P(A \mid B) = \frac{P(A \cap B)}{P(B)}$
- [ ] D) $P(A \mid B) = P(A \cap B)$

**Answer:** C

**Answer Explanation:** This is the fundamental definition of **conditional probability**. It divides the joint probability of both events occurring ($P(A \cap B)$) by the probability of the conditioning event ($P(B)$). This gives the probability that $A$ occurs within the restricted universe where $B$ is known to be true.

Incorrect options:
- **A)** The product $P(A) \cdot P(B)$ is used to test independence, not to compute conditional probability
- **B)** Mathematically meaningless in this context
- **D)** This is the joint probability; it completely ignores the conditioning aspect (the "given that $B$ occurred" part)

**Related Topics:** [Conditional Probability and Conditional Distributions](notes.md#conditional-probability-and-conditional-distributions)

---

### Q4 (1 point)

**If $X$ and $Y$ are two RVs, the conditional distribution is written (conceptually) as:**

- [ ] A) $P_{X \mid Y} = P_X \cdot P_Y$
- [x] B) $P_{X \mid Y} = \frac{P_{XY}}{P_Y}$
- [ ] C) $P_{X \mid Y} = \frac{P_Y}{P_{XY}}$
- [ ] D) $P_{X \mid Y} = P_X + P_Y$

**Answer:** B

**Answer Explanation:** This applies the conditional probability formula from Q3 to entire probability distributions. The conditional distribution of $X$ given $Y$ is the joint distribution divided by the marginal distribution of $Y$. In machine learning, this is exactly what classification does: estimate $P(Y \mid X)$, the probability of a label given input features.

Incorrect options:
- **A)** $P_X \cdot P_Y$ equals the joint distribution only if $X$ and $Y$ are completely independent
- **C)** This is the formula flipped upside down
- **D)** Adding distributions together has no standard interpretation in conditional probability

**Related Topics:** [Conditional Probability and Conditional Distributions](notes.md#conditional-probability-and-conditional-distributions)

---

## Question Set 3: Marginal Distributions

### Q5 (1 point)

**If a joint density (or joint distribution) is known, the marginal of $X$ is obtained by:**

- [ ] A) Differentiating with respect to $y$
- [x] B) Integrating (or summing) out $y$
- [ ] C) Multiplying by $P_Y$
- [ ] D) Setting $y = 0$

**Answer:** B

**Answer Explanation:** To extract the marginal distribution of $X$ from a joint distribution over $X$ and $Y$, we must eliminate (integrate or sum) the $Y$ variable entirely. For continuous variables: $P_X(x) = \int_{-\infty}^{\infty} P_{XY}(x,y) \, dy$. For discrete variables, we sum instead of integrate. This "collapses" across all possible $Y$ values to get the overall distribution of $X$.

Incorrect options:
- **A)** Differentiation (rate of change) does not yield a probability distribution
- **C)** Multiplying by $P_Y$ is mathematically incorrect for marginalization
- **D)** Setting $y=0$ just gives a single "slice" at that point; it ignores all other $Y$ values and thus is not the true marginal

**Related Topics:** [Marginal Distributions](notes.md#marginal-distributions)

---

## Question Set 4: The i.i.d. Assumption and Datasets

### Q6 (1 point)

**The dataset is represented as:**

- [ ] A) $D = \{x_i\}_{i=1}^{n} \sim_{\text{iid}} P_X$
- [ ] B) $D = \{y_i\}_{i=1}^{n} \sim_{\text{iid}} P_Y$
- [x] C) $D = \{(x_i, y_i)\}_{i=1}^{n} \sim_{\text{iid}} P_{XY}$
- [ ] D) $D = \{(x_i, y_i)\}_{i=1}^{n} \sim_{\text{iid}} P_{X \mid Y}$

**Answer:** C

**Answer Explanation:** In **supervised learning**, the dataset consists of input-label pairs sampled independently and identically distributed from the **joint distribution** $P_{XY}$. The notation "$\sim_{\text{iid}} P_{XY}$" means each pair $(x_i, y_i)$ is independently drawn from the same underlying joint distribution.

Incorrect options:
- **A)** Represents an unsupervised learning dataset (only features, no labels) from the marginal $P_X$
- **B)** Just labels without inputs; impossible to train a predictive model
- **D)** You do not sample datasets from conditional distributions; complete observations come from the joint environment

**Related Topics:** [The Independent and Identically Distributed (i.i.d.) Assumption](notes.md#the-independent-and-identically-distributed-iid-assumption)

---

### Q7 (1 point)

**"i.i.d." stands for:**

- [ ] A) indexed and independently defined
- [x] B) independent and identically distributed
- [ ] C) identical and independent dimensions
- [ ] D) integrated and inferred distribution

**Answer:** B

**Answer Explanation:** **i.i.d.** is a core statistical assumption meaning:
- **Identically distributed:** Each datapoint is drawn from the same probability distribution (every patient comes from the same population)
- **Independent:** Drawing one datapoint does not affect the probability of drawing another (Patient A's result doesn't influence Patient B's result)

Together, these properties allow us to use probability theory and statistical inference reliably.

Incorrect options:
- **A)**, **C)**, **D)** are fabricated distractors with no standard meaning in probability/statistics

**Related Topics:** [The Independent and Identically Distributed (i.i.d.) Assumption](notes.md#the-independent-and-identically-distributed-iid-assumption)

---

### Q8 (1 point)

**In a vector-valued datapoint $x \in \mathbb{R}^d$, independence is typically assumed:**

- [ ] A) among the $d$ coordinates within a datapoint
- [x] B) across datapoints, not necessarily across coordinates
- [ ] C) neither across datapoints nor coordinates
- [ ] D) only for labels, not for inputs

**Answer:** B

**Answer Explanation:** The **i.i.d. assumption applies across datapoints** (Patient A's data is independent of Patient B's data). However, **within a single datapoint**, features are generally dependent and correlated. For example, in a vectorized image, neighboring pixel values are highly dependent on each other. This distinction is crucial: assuming independence between features (ignoring their natural correlations) would destroy the structure of the data.

Note: The Naive Bayes classifier makes the naive assumption that features are independent within a datapoint, but this is an explicit simplification, not the typical assumption for general vector data.

Incorrect options:
- **A)** Would destroy feature structure and correlations
- **C)** Would break standard ML math; we always need independence across datapoints
- **D)** Independence applies to complete datapoints $(x,y)$, not just one part

**Related Topics:** [The Independent and Identically Distributed (i.i.d.) Assumption](notes.md#the-independent-and-identically-distributed-iid-assumption)

---

## Question Set 5: Probability Density Function and Distribution Estimation

### Q9 (1 point)

**If $X$ is a continuous RV with density $p_X$, then the CDF satisfies:**

- [x] A) $P_X(x) = \int_{-\infty}^{x} p_X(t) \, dt$
- [ ] B) $P_X(x) = \int_{x}^{\infty} p_X(t) \, dt$
- [ ] C) $p_X(x) = \int_{-\infty}^{x} P_X(t) \, dt$
- [ ] D) $p_X(x) = P(X \leq x)$

**Answer:** A

**Answer Explanation:** This is the fundamental relationship between the **Cumulative Distribution Function (CDF)** and the **Probability Density Function (PDF)**. The CDF at $x$ is the accumulated area under the PDF curve from $-\infty$ to $x$. Mathematically, the CDF is the integral (accumulated sum) of the PDF:

$$P_X(x) = \int_{-\infty}^{x} p_X(t) \, dt$$

This relationship shows that the PDF is the derivative of the CDF: $p_X(x) = \frac{d}{dx} P_X(x)$.

Incorrect options:
- **B)** $\int_{x}^{\infty} p_X(t) dt$ calculates the probability that $X > x$ (the survival function), not the CDF
- **C)** Reverses the relationship; the PDF is the derivative of the CDF, not the integral
- **D)** Equates PDF with the CDF definition; they are different concepts (PDF is a density/rate; CDF is a probability)

**Related Topics:** [Probability Density Function (PDF) and Cumulative Distribution Function (CDF)](notes.md#probability-density-function-pdf-and-cumulative-distribution-function-cdf)

---

### Q10 (1 point)

**In density estimation as described, a common parametric approach is:**

- [x] A) Assume a family $p_\theta$ for the unknown density and estimate $\theta$
- [ ] B) Avoid any modeling assumptions and never optimize parameters
- [ ] C) Set $p_X(x) = P(X = x)$ for all $x$
- [ ] D) Define $\theta$ without using data

**Answer:** A

**Answer Explanation:** **Parametric density estimation** involves three steps:
1. Choose a family of densities $p_\theta$ (e.g., Gaussian with parameters $\theta = (\mu, \Sigma)$)
2. Define a distance metric (e.g., Maximum Likelihood Estimation)
3. Optimize the parameters to fit the data

This approach is practical because we only need to estimate a few parameters rather than the entire distribution. The word "parametric" means we use parameters to define the model.

Incorrect options:
- **B)** Describes non-parametric approaches (like K-Nearest Neighbors), which are the opposite of parametric methods
- **C)** Is just the definition of a discrete Probability Mass Function; it's not a density estimation strategy
- **D)** Would be guessing, not learning; machine learning requires using data to estimate parameters

**Related Topics:** [Distribution Estimation: The Core ML Problem](notes.md#distribution-estimation-the-core-ml-problem)

---

## Answer Key Summary

| Q | Answer | Concept | Difficulty |
|---|--------|---------|-----------|
| Q1 | B | Vector-valued random variable definition | Easy |
| Q2 | B | Joint cumulative distribution function | Easy |
| Q3 | C | Conditional probability formula | Medium |
| Q4 | B | Conditional distribution formula | Medium |
| Q5 | B | Marginal distribution from joint | Medium |
| Q6 | C | Dataset representation with i.i.d. assumption | Easy |
| Q7 | B | Meaning of i.i.d. acronym | Easy |
| Q8 | B | Independence across vs. within datapoints | Hard |
| Q9 | A | PDF-CDF relationship (integration) | Medium |
| Q10 | A | Parametric density estimation approach | Medium |

**Overall Score:** 10/10 points possible


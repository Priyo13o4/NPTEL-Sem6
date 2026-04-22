# Week 5: Assignment 5 — Mathematical Foundations of Machine Learning (CS02)

| Metric | Value |
|--------|-------|
| Due | 2026-02-25, 23:59 IST |
| Submitted | — |
| Score | — |

---

## Question Set 1: Bayesian Estimation and MAP with Beta-Bernoulli

### Q1 (1 point)

**A Bernoulli parameter θ has prior θ ∼ Beta(α, β) with α = 3, β = 5. You observe n = 10 trials with k = 7 successes. Using the MAP formula θ_MAP = (k + α − 1) / (n + α + β − 2), what is θ_MAP?**

- [ ] A) 0.689
- [x] B) 0.562
- [ ] C) 0.692
- [ ] D) 0.750

**Answer:** B

**Answer Explanation:** This is a direct application of the **MAP estimate for Beta-Bernoulli conjugacy**.

**Given:**
- Prior: $\theta \sim \text{Beta}(3, 5)$
- Data: $n = 10$ trials, $k = 7$ successes
- Posterior: $\text{Beta}(3 + 7, 5 + (10 - 7)) = \text{Beta}(10, 8)$

**MAP Formula:** The mode (peak) of $\text{Beta}(\alpha', \beta')$ is:

$$\theta_{MAP} = \frac{\alpha' - 1}{\alpha' + \beta' - 2}$$

Substituting $\alpha' = 10, \beta' = 8$:

$$\theta_{MAP} = \frac{10 - 1}{10 + 8 - 2} = \frac{9}{16} = 0.5625 \approx 0.562$$

**Interpretation:** The prior Beta(3, 5) has mean $\frac{3}{3+5} = 0.375$ (biased toward failure). The data (7 successes) pulls the estimate toward 0.70 (MLE). The MAP estimate (0.562) is between them, balancing prior belief and data evidence.

Incorrect options:
- **A)** and **C)** appear to confuse the formula or use wrong parameter values
- **D)** 0.75 is the MLE (ignoring prior): $7/10 = 0.70$, not 0.75

**Related Topics:** [Bayesian Estimation and MAP](notes.md#bayesian-estimation-and-map), [Conjugate Priors: Beta-Bernoulli](notes.md#conjugate-priors-beta-bernoulli)

---

### Q2 (1 point)

**A coin is tossed n = 8 times and comes up heads k = 6 times. Prior is θ ∼ Beta(2, 2). Compute θ_ML = k/n and θ_MAP = (k + α − 1) / (n + α + β − 2). Which pair is correct?**

- [ ] A) (0.75, 0.75)
- [x] B) (0.75, 0.7)
- [ ] C) (0.75, 0.667)
- [ ] D) (0.80, 0.714)

**Answer:** B

**Answer Explanation:** 

**MLE (Maximum Likelihood Estimate):**

$$\theta_{ML} = \frac{k}{n} = \frac{6}{8} = 0.75$$

Pure data-driven estimate; ignores the prior.

**MAP (Maximum A Posteriori):**

Given:
- Prior: $\text{Beta}(2, 2)$ (symmetric, centered at 0.5)
- Posterior: $\text{Beta}(2 + 6, 2 + (8 - 6)) = \text{Beta}(8, 4)$

$$\theta_{MAP} = \frac{8 - 1}{8 + 4 - 2} = \frac{7}{10} = 0.7$$

**Comparison:**
- **MLE:** $0.75$ (trusts data completely)
- **MAP:** $0.7$ (prior pulls estimate down toward 0.5)
- **Effect of prior:** The symmetric Beta(2,2) prior (which favors fairness) "regularizes" the MLE, pulling it slightly toward 0.5. This is especially valuable when data is limited.

Incorrect options:
- **A)** Would require both to be identical (no prior effect)
- **C)** Incorrect posterior or formula computation
- **D)** Wrong MLE value; MLE is 6/8 = 0.75, not 0.80

**Related Topics:** [Bayesian Estimation and MAP](notes.md#bayesian-estimation-and-map)

---

### Q3 (1 point)

**Posterior parameters for Beta-Bernoulli. Prior: θ ∼ Beta(α, β) with α = 4, β = 6. Data: n = 20 Bernoulli trials with k = 9 successes. What is the posterior distribution?**

- [x] A) Beta(13, 17)
- [ ] B) Beta(9, 11)
- [ ] C) Beta(4, 6)
- [ ] D) Beta(24, 26)

**Answer:** A

**Answer Explanation:** The **conjugacy of Beta prior to Bernoulli likelihood** makes updating trivial: simply add the observed counts to the prior parameters.

**Conjugate Update Rule:**

$$\text{Posterior} = \text{Beta}(\alpha + k, \beta + (n - k))$$

**Computation:**
- Prior: $\text{Beta}(4, 6)$
- Data: $n = 20$ trials, $k = 9$ successes, $(n - k) = 11$ failures
- Posterior: $\text{Beta}(4 + 9, 6 + 11) = \text{Beta}(13, 17)$

**Interpretation:**
- Successes: Prior $\alpha = 4$ + observed $k = 9$ → Posterior $\alpha' = 13$
- Failures: Prior $\beta = 6$ + observed $(n-k) = 11$ → Posterior $\beta' = 17$

This is extremely efficient: no integration, no numerical optimization—just arithmetic!

Incorrect options:
- **B)** Uses only the observed counts without the prior
- **C)** Is just the original prior (ignores data)
- **D)** Sums prior and data counts without proper decomposition

**Related Topics:** [Conjugate Priors: Beta-Bernoulli](notes.md#conjugate-priors-beta-bernoulli)

---

### Q4 (1 point)

**You observe n = 4 Bernoulli trials and all are successes (k = 4). Prior is Beta(2, 5). Compute θ_ML and θ_MAP.**

- [ ] A) $\theta_{ML} = 1, \theta_{MAP} = 1$
- [x] B) $\theta_{ML} = 1, \theta_{MAP} = 5/9$
- [ ] C) $\theta_{ML} = 4/5, \theta_{MAP} = 4/9$
- [ ] D) $\theta_{ML} = 4/5, \theta_{MAP} = 4/9$

**Answer:** B

**Answer Explanation:** This example shows how the prior can **dramatically regularize** the MLE when data is extreme.

**MLE:**

$$\theta_{ML} = \frac{k}{n} = \frac{4}{4} = 1$$

The data says 100% success rate. However, this is just 4 trials—highly uncertain!

**MAP:**

- Prior: $\text{Beta}(2, 5)$ (strong belief toward failure; mean = $2/7 \approx 0.29$)
- Posterior: $\text{Beta}(2 + 4, 5 + 0) = \text{Beta}(6, 5)$

$$\theta_{MAP} = \frac{6 - 1}{6 + 5 - 2} = \frac{5}{9} \approx 0.556$$

**Dramatic Difference:**
- **MLE:** $\theta_{ML} = 1.0$ (100% confidence in success)
- **MAP:** $\theta_{MAP} = 0.556$ (pulls back drastically toward prior)

**Why MAP is Better:** The strong prior Beta(2, 5) encodes domain knowledge that failure is likely. Even though all 4 trials were successes, the prior justifiably "doubts" this extreme observation, pulling the estimate back to a more realistic value.

**Lesson:** When data is limited (small $n$), the prior is crucial and prevents overfitting.

Incorrect options:
- **A)** Would require prior to have no regularizing effect
- **C)** and **D)** have incorrect MLEs and MAPs

**Related Topics:** [Bayesian Estimation and MAP](notes.md#bayesian-estimation-and-map)

---

## Question Set 2: Parzen Window Density Estimation

### Q5 (1 point)

**Parzen window: volume in d dimensions. In Parzen window estimation with a hypercube window of side length h, the region volume is V = h^d. If d = 3, h = 0.2, what is V?**

- [x] A) 0.008
- [ ] B) 0.040
- [ ] C) 0.060
- [ ] D) 0.200

**Answer:** A

**Answer Explanation:** This is a basic geometric calculation.

**Volume Formula for Hypercube:**

For a $d$-dimensional hypercube with side length $h$:

$$V = h^d$$

**Calculation:**

With $d = 3$ and $h = 0.2$:

$$V = (0.2)^3 = 0.2 \times 0.2 \times 0.2 = 0.008$$

**Intuition:** As dimension increases, for a fixed side length $h$, the volume shrinks exponentially. This is part of the **curse of dimensionality**—in high dimensions, neighborhoods become sparse.

Incorrect options:
- **B)** $0.2 \times 3 = 0.6$ (incorrect: multiplies instead of powers)
- **C)** Similar error
- **D)** Would be $h$ itself

**Related Topics:** [Non-Parametric Density Estimation](notes.md#non-parametric-density-estimation), [Parzen Window Estimation](notes.md#parzen-window-estimation)

---

### Q6 (1 point)

**You have n = 500 samples in R². A square Parzen window of side h = 0.1 is centered at x. If k = 8 samples fall inside the window, estimate p(x) using p(x) = k / (n · h^d) (d = 2).**

- [ ] A) 0.8
- [x] B) 1.6
- [ ] C) 3.2
- [ ] D) 8.0

**Answer:** B

**Answer Explanation:** This applies the **master formula for non-parametric density estimation**.

**Master Formula:**

$$p(x) = \frac{k}{n \cdot V}$$

where $V = h^d$ for a hypercube.

**Given:**
- $n = 500$ samples
- $d = 2$ (2D space)
- $h = 0.1$ (window side length)
- $k = 8$ (samples inside window)

**Calculation:**

Step 1: Compute volume
$$V = h^d = (0.1)^2 = 0.01$$

Step 2: Apply density formula
$$p(x) = \frac{k}{n \cdot V} = \frac{8}{500 \cdot 0.01} = \frac{8}{5} = 1.6$$

**Interpretation:** With 8 samples in a volume of 0.01 out of 500 total, the estimated density at $x$ is 1.6 samples per unit volume. This is a moderate density.

Incorrect options:
- **A)** Would result from $k / (n \cdot h)$ instead of $k / (n \cdot h^d)$
- **C)** Would use wrong volume calculation
- **D)** Would be $k / n$ (ignoring volume)

**Related Topics:** [Parzen Window Estimation](notes.md#parzen-window-estimation)

---

## Question Set 3: k-NN Density Estimation

### Q7 (1 point)

**In d = 2, you use k-NN density estimation: choose the smallest region around x with volume V that contains k samples. If n = 2000, k = 20 and the required region volume is V = 0.05, estimate p(x) using p(x) = k / (n · V).**

- [ ] A) 0.05
- [ ] B) 0.10
- [x] C) 0.20
- [ ] D) 0.50

**Answer:** C

**Answer Explanation:** **k-NN density estimation** uses the same master formula as Parzen windows, but with a crucial difference: the volume $V$ is **adaptive**.

**Master Formula (Same for Both):**

$$p(x) = \frac{k}{n \cdot V}$$

**Given:**
- $n = 2000$ samples
- $k = 20$ (fixed number of neighbors)
- $V = 0.05$ (adaptive volume needed to capture $k$ neighbors)

**Calculation:**

$$p(x) = \frac{20}{2000 \cdot 0.05} = \frac{20}{100} = 0.20$$

**Key Insight:** Unlike Parzen windows (where $V = h^d$ is fixed), k-NN **adaptively finds the volume $V$** needed to contain exactly $k$ neighbors. This makes k-NN naturally adapt to local density:
- Dense regions: small $V$ (many neighbors in small volume) → high density
- Sparse regions: large $V$ (few neighbors spread out) → low density

Incorrect options:
- **A)** Would be $k / n$ (ignoring volume)
- **B)** Would be $k / (n \cdot 0.1)$ (different volume)
- **D)** Would be $k / (n \cdot 0.02)$ (different volume)

**Related Topics:** [k-Nearest Neighbors (k-NN) Density Estimation](notes.md#k-nearest-neighbors-k-nn-density-estimation)

---

## Question Set 4: k-NN Classification

### Q8 (1 point)

**In a k-NN classifier, within the chosen volume around x you captured k = 15 neighbors. Counts by class are: K₁ = 6, K₂ = 5, K₃ = 4. Using p(yᵢ|x) = Kᵢ/k, what is the value of p(y₂|x)?**

- [ ] A) 4/15
- [x] B) 5/15
- [ ] C) 6/15
- [ ] D) 9/15

**Answer:** B

**Answer Explanation:** **k-NN Classification** extends density estimation to categorical prediction using local neighbor voting.

**Conditional Probability via k-NN:**

Each class probability is the proportion of neighbors belonging to that class:

$$p(y_i | x) = \frac{K_i}{k}$$

where $K_i$ is the count of neighbors in class $i$.

**Given:**
- Total neighbors: $k = 15$
- Class 2 neighbors: $K_2 = 5$

**Calculation:**

$$p(y_2 | x) = \frac{K_2}{k} = \frac{5}{15} = \frac{1}{3} \approx 0.333$$

**Interpretation:** 5 out of 15 neighbors belong to class 2, so the estimated probability of the test point being class 2 is $1/3$.

**Verification:** All probabilities sum to 1:
$$p(y_1|x) + p(y_2|x) + p(y_3|x) = \frac{6}{15} + \frac{5}{15} + \frac{4}{15} = \frac{15}{15} = 1$$ ✓

Incorrect options:
- **A)** 4/15 is $p(y_3|x)$ (wrong class)
- **C)** 6/15 is $p(y_1|x)$ (wrong class)
- **D)** 9/15 would be sum of two classes

**Related Topics:** [k-NN Classification](notes.md#k-nn-classification)

---

### Q9 (1 point)

**A binary k-NN classifier uses k = 11. Around a test point x, the counts are K₁ = 5, K₂ = 6. Using p(yᵢ|x) = Kᵢ/k and the Bayes decision rule, what is the predicted class?**

- [ ] A) Class 1
- [x] B) Class 2
- [ ] C) Tie (random)
- [ ] D) Cannot be determined without priors

**Answer:** B

**Answer Explanation:** **Bayes Decision Rule** for classification: predict the class with the highest posterior probability.

**Bayes Decision Rule:**

$$\hat{y} = \arg\max_j p(y_j | x)$$

**Given:**
- $k = 11$ neighbors
- Class 1: $K_1 = 5$ neighbors
- Class 2: $K_2 = 6$ neighbors

**Compute Class Probabilities:**

$$p(y_1 | x) = \frac{5}{11} \approx 0.455$$

$$p(y_2 | x) = \frac{6}{11} \approx 0.545$$

**Decision:**

$$\hat{y} = \arg\max(0.455, 0.545) = 2$$

Predict **Class 2** because $p(y_2 | x) > p(y_1 | x)$.

**Intuition:** Class 2 has more neighbors (6 > 5), so it's more probable. The Bayes rule simply says "pick the most likely class."

Incorrect options:
- **A)** Would require $K_1 > K_2$, but $5 < 6$
- **C)** Would only be tie if $K_1 = K_2 = 5.5$ (impossible with integer counts)
- **D)** Priors are implicitly uniform in k-NN (equal for all classes)

**Related Topics:** [k-NN Classification](notes.md#k-nn-classification)

---

### Q10 (1 point)

**In R², n = 1000 and at a fixed location x, the count inside the window is k = 10 when h = 0.2. Assume the same k remains (hypothetically) when changing to h = 0.1. How does the Parzen estimate p(x) = k / (n · h²) change?**

- [ ] A) It becomes half
- [ ] B) It becomes 2×
- [x] C) It becomes 4×
- [ ] D) It stays the same

**Answer:** C

**Answer Explanation:** This question tests understanding of how the **Parzen window volume** depends on side length $h$ in $d$ dimensions.

**Given:** 
- $d = 2$ (2D space)
- $n = 1000$ samples
- $k = 10$ (held constant as hypothetical)
- $h$ changes from $0.2$ to $0.1$

**Parzen Density Formula:**

$$p(x) = \frac{k}{n \cdot h^d}$$

**Original Estimate (h = 0.2):**

$$p(x)_{\text{old}} = \frac{10}{1000 \cdot (0.2)^2} = \frac{10}{1000 \cdot 0.04} = \frac{10}{40} = 0.25$$

**New Estimate (h = 0.1):**

$$p(x)_{\text{new}} = \frac{10}{1000 \cdot (0.1)^2} = \frac{10}{1000 \cdot 0.01} = \frac{10}{10} = 1.0$$

**Change:**

$$\frac{p(x)_{\text{new}}}{p(x)_{\text{old}}} = \frac{1.0}{0.25} = 4$$

The estimate **becomes 4 times larger**.

**Algebraic Insight:** When $h$ is halved, the denominator volume becomes:

$$V_{\text{new}} = (h/2)^d = h^d / 2^d = V_{\text{old}} / 2^d$$

For $d = 2$: $V_{\text{new}} = V_{\text{old}} / 4$

Dividing by a quarter makes the result 4 times larger!

**General Rule:** If $h \rightarrow h/c$ in $d$ dimensions:

$$p(x) \rightarrow p(x) \cdot c^d$$

Incorrect options:
- **A)** Would be for $h \rightarrow h/2$ in 1D
- **B)** Would be for $h \rightarrow h/2$ also incorrect
- **D)** Would ignore the volume dependence on $h$

**Related Topics:** [Parzen Window Estimation](notes.md#parzen-window-estimation)

---

## Answer Key Summary

| Q | Answer | Concept | Difficulty |
|---|--------|---------|-----------|
| Q1 | B (0.562) | MAP with Beta-Bernoulli formula | Easy |
| Q2 | B (0.75, 0.7) | MLE vs MAP comparison | Easy |
| Q3 | A Beta(13, 17) | Conjugate prior update rule | Easy |
| Q4 | B (1, 5/9) | MAP regularization with small data | Medium |
| Q5 | A (0.008) | Hypercube volume calculation | Easy |
| Q6 | B (1.6) | Parzen window density formula | Easy |
| Q7 | C (0.20) | k-NN density estimation | Easy |
| Q8 | B (5/15) | k-NN class probability | Easy |
| Q9 | B Class 2 | Bayes decision rule in k-NN | Easy |
| Q10 | C 4× | Parzen window volume scaling | Medium |

**Overall Score:** 10/10 points possible

---

## Concept Map: Week 5 Progression

**From Data-Driven to Adaptive Estimation:**

1. **Bayesian Paradigm (Q1-Q4):** Balance data likelihood with prior belief
   - MLE is overconfident with small data
   - MAP includes prior regularization
   - Conjugate priors (Beta-Bernoulli) enable efficient computation

2. **Non-Parametric Density (Q5-Q7):** Estimate density without distributional assumptions
   - Master formula: $p(x) = k / (n \cdot V)$
   - Parzen: Fixed $V$, adaptive $k$
   - k-NN: Fixed $k$, adaptive $V$ (preferred for adaptation)

3. **k-NN Classification (Q8-Q10):** Use density estimation for classification
   - Conditional probability: $p(y|x) = K_y / k$ (neighbor voting)
   - Bayes decision: predict class with maximum probability
   - Scaling: understand how parameters affect estimates

**Key Insight:** Week 5 bridges three major paradigms:
- **Bayesian:** Incorporate prior knowledge
- **Non-parametric:** No distributional assumptions
- **Instance-based:** Learn from local neighborhoods

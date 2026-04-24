# Week 11: Assignment 11 — Mathematical Foundations of Machine Learning (CS02)

| Metric | Value |
|--------|-------|
| Due | 2026-04-08, 23:59 IST |
| Submitted | 2026-04-05, 23:52 IST |
| Score | 10/10 ✅ |

---

## Question Set 1: Impurity Measures for Classification Trees

### Q1 (1 point)

**A node in a binary classification tree contains 8 samples from class 0 and 2 samples from class 1. What is the Gini impurity of this node?**

- [ ] A) 0.16
- [ ] B) 0.24
- [ ] C) 0.32
- [ ] D) 0.40

**Answer:** C

**Answer Explanation:**

This question tests the **Gini impurity formula**, the core metric for evaluating classification tree splits. Gini impurity measures the probability of incorrectly labeling a randomly chosen element if labeled according to the class distribution.

**The Gini Impurity Formula:**

$$\text{Gini}(R_m) = 1 - \sum_{k=1}^{K} p_k^2$$

where:
- $R_m$: A node or region.
- $p_k$: Proportion of samples of class $k$ in the node.
- $K$: Number of classes.

**Step-by-Step Calculation:**

Given:
- Class 0: 8 samples
- Class 1: 2 samples
- Total: 10 samples

**Step 1: Calculate probabilities**

$$p_0 = \frac{8}{10} = 0.8$$
$$p_1 = \frac{2}{10} = 0.2$$

**Step 2: Square each probability**

$$p_0^2 = 0.8^2 = 0.64$$
$$p_1^2 = 0.2^2 = 0.04$$

**Step 3: Sum and subtract from 1**

$$\text{Gini} = 1 - (p_0^2 + p_1^2) = 1 - (0.64 + 0.04) = 1 - 0.68 = 0.32$$

**Interpretation:**

A Gini impurity of 0.32 means there's a 32% chance of misclassifying a random element if we labeled it according to the class distribution of this node. This node is relatively **pure** (dominated by class 0), so the impurity is low.

**Why This Formula?**

The probability of selecting two elements of different classes is:

$$P(\text{different classes}) = p_0(1 - p_0) + p_1(1 - p_1) = p_0 p_1 + p_1 p_0 = 2p_0 p_1$$

Summing over all classes: $\sum_k p_k(1 - p_k) = \sum_k (p_k - p_k^2)$. Rearranging: $1 - \sum p_k^2$.

**Intuitive Meaning:**

- **Gini = 0:** Pure node (all one class). No misclassification possible.
- **Gini = 0.5:** Maximum impurity for binary classification (50-50 split).
- **Higher Gini:** More mixed classes, higher impurity.

**Analysis of Incorrect Options:**

- **A) 0.16:** This is $(p_0 - p_1)^2 = (0.8 - 0.2)^2 = 0.36$, then halved incorrectly. Or possibly $p_0 \times p_1 / 2 = 0.8 \times 0.2 / 2 = 0.08$, doubled. Mathematical confusion.

- **B) 0.24:** Could be $1 - (0.8 + 0.2) = 1 - 1 = 0$ (wrong). Or $p_0 \times p_1 = 0.8 \times 0.2 = 0.16$ (missing the factor). Or $0.8 \times 0.3 = 0.24$ (mixing probability and incorrect formula).

- **D) 0.40:** This could come from $2 \times p_0 \times p_1 = 2 \times 0.8 \times 0.2 = 0.32$, off by $0.08$. Or a simple arithmetic error in the final calculation.

**Key Insight:** The Gini formula is $1 - \sum p_k^2$. Always square the probabilities, sum them, then subtract from 1. This is not the same as the product $p_0 p_1$ or any other combination.

**Related Topics:** [Gini Impurity](notes.md#gini-impurity)

---

### Q2 (1 point)

**A node contains 3 samples of class A and 1 sample of class B. Using base-2 logarithm, what is the entropy of the node?**

- [ ] A) 0.311
- [ ] B) 0.811
- [ ] C) 1.0
- [ ] D) 1.5

**Answer:** B

**Answer Explanation:**

This question tests the **entropy formula**, an alternative impurity measure based on information theory. Entropy measures the average information content or "uncertainty" in a distribution.

**The Entropy Formula:**

$$H(R_m) = -\sum_{k=1}^{K} p_k \log_2(p_k)$$

where:
- $p_k$: Proportion of samples of class $k$ in the node.
- $\log_2$: Base-2 logarithm (information in "bits").
- Convention: $0 \log_2(0) = 0$ (because $\lim_{p \to 0^+} p \log p = 0$).

**Step-by-Step Calculation:**

Given:
- Class A: 3 samples
- Class B: 1 sample
- Total: 4 samples

**Step 1: Calculate probabilities**

$$p_A = \frac{3}{4} = 0.75$$
$$p_B = \frac{1}{4} = 0.25$$

**Step 2: Compute the logarithmic terms**

$$p_A \log_2(p_A) = 0.75 \times \log_2(0.75)$$

We need $\log_2(0.75)$:
$$\log_2(0.75) = \log_2(3/4) = \log_2(3) - \log_2(4) = \log_2(3) - 2$$
$$\log_2(3) \approx 1.585, \text{ so } \log_2(0.75) \approx 1.585 - 2 = -0.415$$

Thus: $0.75 \times (-0.415) \approx -0.311$

$$p_B \log_2(p_B) = 0.25 \times \log_2(0.25)$$

We know: $\log_2(0.25) = \log_2(1/4) = -\log_2(4) = -2$

Thus: $0.25 \times (-2) = -0.5$

**Step 3: Negate and sum**

$$H = -(p_A \log_2(p_A) + p_B \log_2(p_B)) = -(-0.311 - 0.5) = 0.311 + 0.5 = 0.811$$

**Interpretation:**

An entropy of 0.811 bits means the node has moderate uncertainty. The node is not pure (entropy > 0) but skews toward class A (entropy < 1).

**Entropy Ranges:**

- **H = 0:** Pure node (all one class).
- **H = 1:** Maximum entropy for binary classification (50-50 split).
- **H = log₂(K):** Maximum entropy for K classes.

**Information Gain (Context):**

Decision trees choose splits that maximize **information gain**, the reduction in entropy:

$$\text{IG} = H(\text{parent}) - \left[\frac{N_L}{N} H(\text{left}) + \frac{N_R}{N} H(\text{right})\right]$$

Splits that create pure child nodes produce high information gain.

**Analysis of Incorrect Options:**

- **A) 0.311:** This is only the contribution from class A: $|0.75 \times \log_2(0.75)| = 0.311$. The student forgot to include the contribution from class B.

- **C) 1.0:** This would be the entropy of a balanced 50-50 binary classification node. Here, the node is 75-25, which is more pure and thus has lower entropy.

- **D) 1.5:** This is roughly $0.75 + 0.75 = 1.5$, a miscalculation where the student summed something incorrectly or doubled a value.

**Key Insight:** Entropy uses logarithms, not powers like Gini. The two main terms (one for each class) must both be computed and summed. Always negate the final result (because $\log$ of a fraction is negative).

**Logarithm Reminder (Base 2):**

- $\log_2(1) = 0$
- $\log_2(2) = 1$
- $\log_2(4) = 2$
- $\log_2(1/4) = -2$
- $\log_2(0.75) \approx -0.415$

**Related Topics:** [Entropy and Information Gain](notes.md#entropy-and-information-gain)

---

## Question Set 2: Regression Trees

### Q3 (1 point)

**In a regression tree node, the target values are [2, 4, 6, 8]. What is the prediction made by that leaf?**

- [ ] A) 4
- [ ] B) 6
- [ ] C) 20
- [ ] D) 5

**Answer:** D

**Answer Explanation:**

This question tests the fundamental prediction rule for regression trees: **the leaf node outputs the arithmetic mean of all target values in that region**.

**The Regression Tree Prediction Formula:**

$$\hat{y}_m = \frac{1}{N_m} \sum_{i \in R_m} y_i$$

where:
- $\hat{y}_m$: Prediction for region $m$.
- $N_m$: Number of samples in region $m$.
- $y_i$: Target value for sample $i$.

**Calculation:**

Given targets: [2, 4, 6, 8]. Number of samples: $N_m = 4$.

$$\hat{y}_m = \frac{2 + 4 + 6 + 8}{4} = \frac{20}{4} = 5$$

**Why the Mean?**

The mean is the value that **minimizes Mean Squared Error (MSE)**. Mathematically:

$$\text{MSE}(c) = \frac{1}{N} \sum_{i} (y_i - c)^2$$

Taking the derivative with respect to $c$ and setting it to zero:

$$\frac{\partial}{\partial c} \text{MSE} = -\frac{2}{N} \sum_i (y_i - c) = 0$$

Solving: $\sum_i y_i = N \times c$, so $c = \frac{1}{N} \sum_i y_i$ (the mean).

**Intuition:**

Unlike classification trees (which assign the majority class), regression trees output a continuous value. The mean minimizes squared deviations and is thus the optimal constant prediction for a region.

**Analysis of Incorrect Options:**

- **A) 4:** This is one of the data points in the region. If you mistakenly picked the second value or the median of the four values, you might get 4 or other values, but not the mean.

- **B) 6:** This is another data point in the region. Could result from incorrectly picking the maximum, median (of [4, 6]), or the third value.

- **C) 20:** This is the **sum** of the targets, not the mean. Forgetting to divide by the number of samples is a common error.

**Key Insight:** In regression trees, the leaf prediction is always the **mean** of targets in that leaf. This is different from classification trees, which use the **majority class**. Always divide by the count.

**Related Topics:** [Prediction in Leaf Nodes](notes.md#prediction-in-leaf-nodes)

---

### Q4 (1 point)

**In a regression tree node, the target values are [1, 3, 5]. What is the average squared deviation from the node mean?**

- [ ] A) 2
- [ ] B) 4/3
- [ ] C) 8/3
- [ ] D) 4

**Answer:** C

**Answer Explanation:**

This question tests the **variance (MSE) calculation within a regression tree node**, which measures impurity and is used to evaluate split quality.

**The Regression Impurity (Variance) Formula:**

$$I(R_m) = \frac{1}{N_m} \sum_{i \in R_m} (y_i - \hat{y}_m)^2$$

where:
- $\hat{y}_m$: Mean of targets in the node.
- The numerator is the sum of squared deviations from the mean.
- Dividing by $N_m$ gives the **mean** squared deviation (variance).

**Step-by-Step Calculation:**

Given targets: [1, 3, 5]. Number of samples: $N_m = 3$.

**Step 1: Calculate the mean**

$$\hat{y}_m = \frac{1 + 3 + 5}{3} = \frac{9}{3} = 3$$

**Step 2: Calculate squared deviations**

$$(1 - 3)^2 = (-2)^2 = 4$$
$$(3 - 3)^2 = 0^2 = 0$$
$$(5 - 3)^2 = 2^2 = 4$$

**Step 3: Sum squared deviations**

$$\sum (y_i - \hat{y}_m)^2 = 4 + 0 + 4 = 8$$

**Step 4: Divide by number of samples**

$$I(R_m) = \frac{8}{3}$$

**Interpretation:**

The average squared deviation is $8/3 \approx 2.667$. This measures the "spread" of targets around their mean. If this variance is high, it indicates the region is impure (mixed target values). A split is valuable if it reduces this variance.

**Comparison to Gini/Entropy:**

- **Classification (Gini, Entropy):** Measure purity based on class distribution.
- **Regression (Variance):** Measure spread of continuous values around the mean.

Both are impurity measures; trees choose splits that minimize the weighted average impurity of child nodes.

**Analysis of Incorrect Options:**

- **A) 2:** This is close to $8/3 \approx 2.67$ but rounded incorrectly, or the calculation forgot one of the squared deviations.

- **B) 4/3:** This might come from $(4 + 0)/3 = 4/3$, omitting one of the squared deviations (the $4$ for the third point).

- **D) 4:** This is the **sum** of squared deviations (8), divided by 2 instead of 3. Or possibly just the maximum squared deviation.

**Key Insight:** Always calculate the **mean first**, then compute squared deviations from that mean, then average (divide by count). This is the variance formula: $\text{Var} = \frac{1}{N}\sum(x - \bar{x})^2$.

**Related Topics:** [Impurity: Variance / MSE](notes.md#impurity-variance--mse)

---

### Q5 (1 point)

**In bagging, a bootstrap sample of size n is drawn with replacement from a dataset of size n. For large n, what fraction of original samples is expected to be included at least once in one bootstrap sample?**

- [ ] A) 0.523
- [ ] B) 0.623
- [ ] C) 0.632
- [ ] D) 0.532

**Answer:** C

**Answer Explanation:**

This question tests a **fundamental property of bootstrap sampling**: the mathematical relationship between sample size, sampling with replacement, and the expected fraction of unique samples included.

**The Bootstrap Probability Setup:**

When sampling with replacement from a dataset of size $n$:

- Probability that a specific sample is **not drawn** on a single trial: $1 - \frac{1}{n}$.
- Probability that it is **never drawn** in $n$ independent trials: $\left(1 - \frac{1}{n}\right)^n$.
- Probability that it is **drawn at least once**: $1 - \left(1 - \frac{1}{n}\right)^n$.

**Taking the Limit (Large n):**

As $n \to \infty$:

$$\lim_{n \to \infty} \left(1 - \frac{1}{n}\right)^n = \frac{1}{e}$$

where $e \approx 2.71828$ (Euler's number).

Thus:

$$\lim_{n \to \infty} \left[1 - \left(1 - \frac{1}{n}\right)^n\right] = 1 - \frac{1}{e} \approx 1 - 0.368 = 0.632$$

**Numerical Verification:**

For $n = 100$:
$$\left(1 - \frac{1}{100}\right)^{100} = 0.99^{100} \approx 0.366$$
$$1 - 0.366 = 0.634 \approx 0.632$$

For $n = 1000$:
$$\left(1 - \frac{1}{1000}\right)^{1000} \approx 0.368$$
$$1 - 0.368 = 0.632$$

**Interpretation:**

In a bootstrap sample of size $n$:
- Approximately **63.2%** of the original data appears at least once (with possible duplicates).
- Approximately **36.8%** of the original data is omitted entirely.
- On average, unique samples ≈ 0.632n.

**Why This Matters for Bagging:**

1. Each tree in a bagging ensemble trains on a bootstrap sample, which includes only 63.2% of the original data.
2. The 36.8% omitted becomes "out-of-bag" data, useful for validation without a separate test set.
3. Trees trained on different bootstrap samples see different subsets, introducing variance in their predictions.

**Analysis of Incorrect Options:**

- **A) 0.523:** Off by about 0.109. Could result from miscalculating the limit or using an incorrect formula.

- **B) 0.623:** Very close to the correct answer (off by 0.009), likely a rounding error or typo.

- **D) 0.532:** Off by about 0.1. Another computational or formula error.

**Key Insight:** The magic number for bootstrap inclusion is $1 - 1/e \approx 0.632$. This is a fundamental property of sampling with replacement and appears frequently in ensemble learning.

**Connection to the Next Question:**

The complement (36.8% omitted) is central to Q8.

**Related Topics:** [Bootstrap Sampling](notes.md#bootstrap-sampling)

---

### Q6 (1 point)

**A regression tree split produces two child nodes. Left child targets: [1, 3], right child targets: [2, 6]. What is the weighted average squared deviation after the split?**

- [ ] A) 2
- [ ] B) 3
- [ ] C) 2.5
- [ ] D) 3.5

**Answer:** C

**Answer Explanation:**

This question tests the **evaluation of split quality in regression trees** using the **weighted average variance** metric. Decision trees choose splits that minimize this metric.

**The Weighted Variance Formula:**

After splitting a parent node into left (L) and right (R) child nodes:

$$\text{Weighted Variance} = \frac{N_L}{N} \times \text{Var}(L) + \frac{N_R}{N} \times \text{Var}(R)$$

where:
- $N_L, N_R$: Number of samples in left and right children.
- $N = N_L + N_R$: Total samples (before split).
- $\text{Var}(L), \text{Var}(R)$: Variance (MSE) within each child.

**Step-by-Step Calculation:**

**Left Child: [1, 3]**

Mean: $\bar{y}_L = \frac{1 + 3}{2} = 2$

Variance: $\text{Var}(L) = \frac{(1-2)^2 + (3-2)^2}{2} = \frac{1 + 1}{2} = \frac{2}{2} = 1$

Number of samples: $N_L = 2$

**Right Child: [2, 6]**

Mean: $\bar{y}_R = \frac{2 + 6}{2} = 4$

Variance: $\text{Var}(R) = \frac{(2-4)^2 + (6-4)^2}{2} = \frac{4 + 4}{2} = \frac{8}{2} = 4$

Number of samples: $N_R = 2$

**Weighted Average:**

Total samples: $N = N_L + N_R = 2 + 2 = 4$

$$\text{Weighted Variance} = \frac{2}{4} \times 1 + \frac{2}{4} \times 4 = 0.5 \times 1 + 0.5 \times 4 = 0.5 + 2 = 2.5$$

**Why This Matters:**

The tree evaluates all possible splits at each node and chooses the split that **minimizes** the weighted variance of child nodes. This is the **variance reduction criterion**:

$$\text{Variance Reduction} = \text{Var}(\text{Parent}) - \text{Weighted Variance}(\text{Children})$$

Larger variance reduction → better split.

**Intuition:**

The weighted average ensures that larger child nodes have more influence. If the right child (with 4 samples) had very low variance, it would contribute more to reducing overall impurity.

**Analysis of Incorrect Options:**

- **A) 2:** Close to 2.5 but computed incorrectly. Might result from averaging only the left child's variance or forgetting the right child's contribution.

- **B) 3:** Between the two child variances (1 and 4). Could be the simple arithmetic mean $(1 + 4)/2 = 2.5$... wait, that's 2.5. Maybe $(2 + 4)/2 = 3$? Using sample counts instead of variances.

- **D) 3.5:** Could result from incorrect weighting: $\frac{1}{2}(1 + 4) + 1 = 3.5$ (adding an extra 1 for some reason).

**Key Insight:** The weighted average accounts for imbalanced splits. Always use $\frac{N_L}{N} \times \text{Var}_L + \frac{N_R}{N} \times \text{Var}_R$. Weights must sum to 1.

**Related Topics:** [Regression Trees, Impurity: Variance / MSE](notes.md#impurity-variance--mse)

---

## Question Set 3: Bagging and Bootstrap Properties

### Q7 (1 point)

**Suppose each base learner in a bagging ensemble has variance $\sigma^2 = 9$, pairwise correlation $\rho = 0.2$, and there are $B = 5$ learners. Using $\text{Var}(\bar{f}) = \sigma^2(\rho + \frac{1-\rho}{B})$, what is the ensemble variance?**

- [ ] A) 2.88
- [ ] B) 4.32
- [ ] C) 3.24
- [ ] D) 9

**Answer:** C

**Answer Explanation:**

This question tests the **ensemble variance formula for bagging**, which reveals the fundamental trade-off between correlation and variance reduction. This is a critical insight for understanding why bagging works and why more trees don't always help.

**The Bagging Ensemble Variance Formula:**

$$\text{Var}(\bar{f}) = \sigma^2 \left( \rho + \frac{1 - \rho}{B} \right)$$

where:
- $\sigma^2$: Variance of a single base learner.
- $\rho$: Pairwise correlation between any two learners' predictions (assuming all pairs have the same correlation).
- $B$: Number of learners (ensemble size).

**Step-by-Step Calculation:**

Given: $\sigma^2 = 9$, $\rho = 0.2$, $B = 5$

$$\text{Var}(\bar{f}) = 9 \times \left( 0.2 + \frac{1 - 0.2}{5} \right)$$

**Compute the term in parentheses:**

$$0.2 + \frac{0.8}{5} = 0.2 + 0.16 = 0.36$$

**Multiply by $\sigma^2$:**

$$\text{Var}(\bar{f}) = 9 \times 0.36 = 3.24$$

**Interpretation:**

The ensemble variance is **3.24**, down from **9** (a single learner). That's a variance reduction of $\frac{9 - 3.24}{9} \approx 64\%$. Adding 5 trees from 1 reduced variance by roughly two-thirds.

**Understanding the Two Terms:**

1. **$\rho \sigma^2$ term (First term: $0.2 \times 9 = 1.8$):** The "irreducible" part due to correlation. As $B \to \infty$, this term never disappears. It's the floor on variance reduction.

2. **$\frac{1-\rho}{B} \sigma^2$ term (Second term: $\frac{0.8 \times 9}{5} = 1.44$):** The part that shrinks with more learners. As $B$ increases, this term decreases.

**Why Correlation Matters:**

- **If $\rho = 0$ (uncorrelated):** $\text{Var}(\bar{f}) = \frac{\sigma^2}{B}$. Variance scales inversely with ensemble size. More trees always help.
- **If $\rho > 0$ (correlated):** The first term $\rho \sigma^2$ is a floor. Adding more trees helps but never overcomes this baseline.

**Bootstrap samples are correlated:** They're all drawn from the same original data, so trees trained on them have $\rho > 0$ (typically 0.1–0.3). This explains why bagging has diminishing returns: the correlation floor prevents unlimited variance reduction.

**Analysis of Incorrect Options:**

- **A) 2.88:** This is $9 \times 0.32 = 2.88$. The student might have computed $0.2 + \frac{0.8}{6} = 0.2 + 0.133 = 0.333 \approx 0.32$ (using 6 trees instead of 5 or miscomputing the division).

- **B) 4.32:** This is $9 \times 0.48 = 4.32$. Possible computation: $0.2 + \frac{0.8}{2} = 0.2 + 0.4 = 0.6$ is wrong; maybe $(0.2 + 0.8) \times 0.48 = 1.0 \times 0.48$? Arithmetic error.

- **D) 9:** This is the variance of a single learner, completely ignoring the ensemble variance reduction. The student may have thought the formula gives no benefit, which is incorrect.

**Key Insight:** The formula shows that ensemble variance has a correlation floor ($\rho \sigma^2$). To reduce correlation and maximize variance reduction, bagging uses bootstrap sampling; Random Forests go further by subsampling features.

**Related Topics:** [Bagging (Bootstrap Aggregating), Variance Reduction Formula](notes.md#bagging-ensemble-averaging)

---

### Q8 (1 point)

**If the probability that a training point is not selected in a bootstrap sample is approximately 0.368, then in a dataset of 1000 points, about how many points are expected to be omitted from one bootstrap sample?**

- [ ] A) 368
- [ ] B) 268
- [ ] C) 500
- [ ] D) 632

**Answer:** A

**Answer Explanation:**

This question tests the **practical application of bootstrap sampling properties**. Given the omission probability from Q5 (or derived independently), calculate the expected number of omitted samples in a real dataset.

**The Expected Value Calculation:**

If the probability of a sample being omitted is $p = 0.368$, and the dataset has $n = 1000$ points, the **expected number of omitted points** is:

$$\mathbb{E}[\text{Omitted}] = n \times p = 1000 \times 0.368 = 368$$

**Connection to Q5:**

From Q5, we learned that the probability of a sample being **included at least once** is $1 - 1/e \approx 0.632$. The complement is:

$$P(\text{Omitted}) = 1 - 0.632 = 0.368$$

**Interpretation:**

In a bootstrap sample of size 1000:
- Expected omitted samples: 368
- Expected included samples: 632
- Note: "Included samples" (632) may contain duplicates; unique samples ≈ 632.

**Out-of-Bag (OOB) Validation:**

The omitted samples (36.8% of data) are called **out-of-bag** data. Since they didn't appear in the bootstrap sample, they can be used to validate the tree trained on that sample, without needing a separate test set. This is a powerful feature of bagging.

**Analysis of Incorrect Options:**

- **B) 268:** Off by about 100 samples. Could result from computing $1000 \times 0.268 = 268$, using an incorrect omission probability.

- **C) 500:** This assumes a 50-50 omission rate, which is incorrect. Maybe the student thought "random sampling" meant half the data is left out.

- **D) 632:** This is the expected number of **included** samples, not omitted. The student confused the complement. The correct pairing is: included ≈ 632, omitted ≈ 368.

**Key Insight:** Always use the exact probability value given. Here, 0.368 means $1000 \times 0.368 = 368$ points are omitted. This is the complement of the 0.632 from Q5.

**Related Topics:** [Bootstrap Sampling, The Mathematics of Bootstrap](notes.md#the-mathematics-of-bootstrap)

---

### Q9 (1 point)

**In a random forest, suppose the input has 16 features and at each node 4 features are randomly considered. What fraction of the total features is examined at each split?**

- [ ] A) 0.125
- [ ] B) 0.25
- [ ] C) 0.5
- [ ] D) 0.75

**Answer:** B

**Answer Explanation:**

This question tests understanding of **feature subsampling in Random Forests**, the key mechanism for decorrelating trees and improving ensemble variance reduction.

**The Feature Sampling Fraction:**

At each node split in a Random Forest, the algorithm randomly selects $m$ features from the total $p$ available features and considers splits only on these $m$ features. The fraction of features examined is:

$$\text{Feature Fraction} = \frac{m}{p}$$

where:
- $m$: Number of randomly selected features at each node.
- $p$: Total number of features.

**Calculation:**

Given: $m = 4$, $p = 16$

$$\text{Feature Fraction} = \frac{4}{16} = \frac{1}{4} = 0.25$$

**Why Random Feature Selection?**

1. **Reduces Correlation:** Trees are forced to use different features, making their predictions less correlated ($\rho$ decreases).
2. **Improves Bagging:** Lower correlation means the ensemble variance floor ($\rho \sigma^2$) drops, allowing better variance reduction.
3. **Reduces Computational Cost:** Searching for splits only on 4 features is faster than searching on 16.

**Typical Default Values:**

- **Classification:** $m = \sqrt{p}$ (square root of total features).
  - Example: 16 features → $m = 4$ (as in this question).
  
- **Regression:** $m = p/3$ (one-third of total features).
  - Example: 16 features → $m \approx 5.3$ (round to 5).

**Hyperparameter Tuning:**

In practice, $m$ is a hyperparameter that can be tuned via cross-validation. Smaller $m$ → more diverse trees → lower correlation but higher bias. Larger $m$ → more features searched → higher variance but lower bias.

**Analysis of Incorrect Options:**

- **A) 0.125:** This is $2/16 = 0.125$. The student might have halved the intended feature count or misread the problem.

- **C) 0.5:** This is $8/16 = 0.5$. The student may have thought "half the features" or doubled the numerator.

- **D) 0.75:** This is $12/16 = 0.75$. Off by one-quarter of the features.

**Key Insight:** Random Forest feature subsampling is a simple ratio: $m/p$. This is distinct from bootstrap sampling (which samples *rows/observations*) and is what makes Random Forests more decorrelated than vanilla bagging.

**Related Topics:** [Random Forests](notes.md#6-random-forests)

---

## Question Set 4: Gradient Boosting

### Q10 (1 point)

**In gradient boosting, suppose for a sample the current prediction is $H_{m-1}(x) = 5$, the new weak learner predicts $h_m(x) = 2$, and the step size is $\alpha = 0.3$. What is the updated prediction?**

- [ ] A) 5.3
- [ ] B) 6.0
- [ ] C) 5.6
- [ ] D) 6.5

**Answer:** C

**Answer Explanation:**

This question tests the **gradient boosting update rule**, which sequentially improves predictions by adding scaled residuals. This is the core mechanism of boosting algorithms like XGBoost and LightGBM.

**The Gradient Boosting Update Formula:**

$$H_m(x) = H_{m-1}(x) + \alpha_m h_m(x)$$

where:
- $H_{m-1}(x)$: Prediction from the ensemble at iteration $m-1$.
- $h_m(x)$: Prediction from the new weak learner at iteration $m$.
- $\alpha_m$: Step size or learning rate (scales the contribution of the new learner).
- $H_m(x)$: Updated ensemble prediction.

**Step-by-Step Calculation:**

Given:
- $H_{m-1}(x) = 5$ (current ensemble prediction)
- $h_m(x) = 2$ (new weak learner prediction, i.e., predicted residual)
- $\alpha = 0.3$ (learning rate)

$$H_m(x) = 5 + 0.3 \times 2 = 5 + 0.6 = 5.6$$

**Interpretation:**

The ensemble's prediction moves from 5 to 5.6, a small step of 0.6 in the direction suggested by the new learner. If the true target is (say) 8, the residual is $8 - 5 = 3$. The new learner predicts 2, and adding $0.3 \times 2 = 0.6$ brings us closer to 8 (new prediction: 5.6 vs. old: 5).

**Why a Scaled Update?**

The learning rate $\alpha$ prevents aggressive updates that might overshoot:

- **Large $\alpha$ (e.g., 0.5):** Aggressive updates, faster training, risk of oscillation/overfitting.
- **Small $\alpha$ (e.g., 0.01):** Conservative updates, slower training, better generalization.

In practice, small $\alpha$ (0.01–0.1) is preferred; more iterations are needed but final model is more robust.

**How Boosting Reduces Bias:**

Suppose the true target is $y = 8$, and the current ensemble predicts 5. The residual is 3. If the new learner perfectly predicts this residual (2 is close), then:

$$H_m(x) = 5 + 0.3 \times 2 = 5.6$$

With more iterations, the residuals decrease, and the ensemble gets closer and closer to the true target. This is **reducing bias** (systematic underprediction).

**Contrast with Bagging:**

- **Bagging:** Train many trees independently, average their predictions. Reduces variance.
- **Boosting:** Train trees sequentially, each focuses on previous ensemble's mistakes. Reduces bias.

**Analysis of Incorrect Options:**

- **A) 5.3:** This is $5 + 0.3 = 5.3$, as if you forgot to multiply the learning rate by the weak learner's prediction. Forgot the $h_m(x) = 2$ factor.

- **B) 6.0:** This is $5 + 1 = 6.0$, treating the new learner's prediction as a full update ($\alpha = 1$) instead of scaled by $0.3$.

- **D) 6.5:** This is $5 + 1.5 = 6.5$. Could result from $\alpha \times h_m = 0.75 \times 2 = 1.5$ (using $\alpha = 0.75$ instead of 0.3), or from $5 + 2 \times 0.75 = 6.5$.

**Key Insight:** The gradient boosting formula is additive: $H_m = H_{m-1} + \alpha h_m$. Always multiply the learning rate by the new learner's prediction. The learning rate scales the contribution, preventing wild swings.

**Real-World Note:**

In gradient boosting regression, the new learner $h_m$ is actually trained to predict the negative gradient (residual) of the loss. For squared error loss, this is indeed the residual $y - H_{m-1}(x)$.

**Related Topics:** [Gradient Boosting, The Boosting Update, Residual-Based Learning](notes.md#the-boosting-update)

---

## Answer Key Summary

| Q | Answer | Concept | Difficulty |
|---|--------|---------|-----------|
| Q1 | C (0.32) | Gini impurity formula | Easy |
| Q2 | B (0.811) | Entropy calculation with logarithms | Medium |
| Q3 | D (5) | Regression tree leaf prediction (mean) | Easy |
| Q4 | C (8/3) | Regression tree node variance | Medium |
| Q5 | C (0.632) | Bootstrap inclusion probability ($1 - 1/e$) | Medium |
| Q6 | C (2.5) | Weighted variance post-split | Medium |
| Q7 | C (3.24) | Bagging ensemble variance formula | Hard |
| Q8 | A (368) | Bootstrap omitted samples | Easy |
| Q9 | B (0.25) | Random forest feature fraction | Easy |
| Q10 | C (5.6) | Gradient boosting update rule | Easy |

**Overall Score:** 10/10 points ✅

---

## Concept Map: Week 11 Progression

**From Individual Trees to Powerful Ensembles:**

### **Part 1: Tree Fundamentals (Q1–Q4)**

**Classification Trees (Q1–Q2):**
- Gini impurity: Measure misclassification probability.
- Entropy: Information-theoretic measure of uncertainty.
- Both used to evaluate splits; choose split that minimizes impurity.

**Regression Trees (Q3–Q4):**
- Leaf prediction: Mean of target values.
- Impurity: Variance (MSE) of targets around the mean.
- Split evaluation: Minimize weighted variance of children.

**Common Theme:** Trees partition feature space recursively, using impurity metrics to decide splits.

### **Part 2: Bagging & Bootstrap Properties (Q5–Q8)**

**Bootstrap Mathematics (Q5, Q8):**
- Sampling with replacement creates diverse datasets.
- Expected fraction included: $1 - 1/e \approx 0.632$.
- Expected fraction omitted: $1/e \approx 0.368$ (out-of-bag data).

**Bagging Variance Formula (Q7):**
- $\text{Var}(\bar{f}) = \rho \sigma^2 + \frac{1-\rho}{B} \sigma^2$
- Two terms: correlation floor + term decreasing with B.
- Bootstrap creates correlated samples → limits variance reduction.

### **Part 3: Random Forests (Q9)**

**Feature Subsampling:**
- Decorrelates trees by forcing different feature subsets.
- Feature fraction: $m/p$ (e.g., $4/16 = 0.25$).
- Reduces correlation floor → better variance reduction than bagging.

### **Part 4: Gradient Boosting (Q10)**

**Sequential Ensemble Building:**
- Additive update: $H_m = H_{m-1} + \alpha h_m$
- New learner trained on residuals (negative gradient).
- Learning rate $\alpha$ controls step size.
- Reduces bias (not variance) by iteratively correcting errors.

---

## Summary: When to Use Each Method

| Method | Goal | Strengths | Weaknesses | Example |
|--------|------|-----------|-----------|---------|
| **Decision Tree** | Interpretability | Fast, handles mixed types | Prone to overfitting | Credit approval (rule-based) |
| **Random Forest** | Balanced accuracy | Fast, parallel, low variance | Less interpretable | Image classification on tabular features |
| **Gradient Boosting** | Maximum accuracy | Often highest accuracy on tabular data | Slow training, requires tuning | Kaggle competitions, fraud detection |

---

## Week 11 Learning Outcomes

After this week, you should understand:

1. **Impurity Metrics:** Gini and Entropy for classification; Variance for regression.
2. **Tree Construction:** Recursive splitting to minimize impurity.
3. **Bootstrap Sampling:** Mathematical properties and implications.
4. **Bagging & Ensembles:** Variance reduction through averaging correlated predictions.
5. **Random Forests:** Decorrelation via feature subsampling.
6. **Gradient Boosting:** Bias reduction via sequential residual learning.
7. **Practical Trade-offs:** Accuracy vs. interpretability, training time vs. generalization.

**Bridge to Real-World ML:**

Decision trees and ensemble methods are **not just academic exercises**—they are the industry standard for tabular data (structured, column-based datasets). While neural networks dominate unstructured data (images, text), gradient boosting models (XGBoost, LightGBM, CatBoost) consistently win:

- Real-world Kaggle competitions.
- Credit risk and financial modeling.
- Healthcare diagnosis and treatment planning.
- Recommendation systems with explicit features.

Master these tools alongside neural networks (Weeks 1–10) to become a well-rounded ML practitioner.


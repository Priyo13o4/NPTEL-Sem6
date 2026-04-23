# Week 11: Theoretical Notes — Classification and Regression Trees, Ensemble Methods

## Overview

Week 11 pivots from neural networks and transformers to **tree-based models** and **ensemble learning**—classical yet powerful methods that dominate practical machine learning, especially for **tabular data** (structured, column-based data like financial time series, medical records, or customer data). While deep learning excels with unstructured data (images, text), decision trees and ensembles like Random Forests and Gradient Boosting often outperform neural networks on tabular problems. This week covers the mathematical foundations of tree construction, impurity measures, and how combining multiple weak learners creates robust strong learners.

---

## 1. Decision Trees: Recursive Data Space Partitioning

### Core Idea: Non-Parametric Learning

**Definition:** A **decision tree** is a non-parametric supervised learning model that recursively partitions the input feature space into rectangular regions, assigning a prediction (class label or continuous value) to each region.

**Why Non-Parametric?**

Unlike neural networks (which learn a fixed number of weights), decision trees adapt their structure to the data. A simple dataset might yield a shallow tree; complex data produces a deep tree. The model complexity grows with data complexity, not bounded by architecture choices.

**Recursive Partitioning:**

At each node, the algorithm selects a feature and threshold, creating a binary split:

$$\text{Split: If } x_j < t \text{ then go LEFT, else go RIGHT}$$

This continues recursively on each child node until a stopping criterion (e.g., pure leaf, max depth) is reached.

### Decision Boundaries

**Geometry:**

Decision trees partition the feature space into axis-aligned rectangular regions (hyperboxes). Unlike neural networks that learn arbitrary curved boundaries, trees produce step-function boundaries parallel to axes.

**Example (2D Feature Space):**

```
Feature 2
  ↑
  |  [Class A | Class B]
  |  [Class A | Class B]
  |_______________
  |  [Class A | Class A]
  |_______________→ Feature 1
```

Each rectangular region is assigned a constant prediction.

---

## 2. Impurity Measures for Classification Trees

### Gini Impurity

**Definition:** Gini impurity measures the probability of misclassifying a randomly chosen element if it were randomly labeled according to the class distribution.

**Formula:**

$$\text{Gini}(R_m) = 1 - \sum_{k=1}^{K} p_k^2$$

where:
- $R_m$: Region/node $m$.
- $p_k$: Proportion of samples of class $k$ in the node.
- $K$: Number of classes.

**Interpretation:**

- **Gini = 0:** Pure node (all samples one class). No misclassification possible.
- **Gini = 0.5:** For binary classification, a 50-50 split. Maximum impurity.

**Example (Q1 from Assignment):**

Node: 8 samples class 0, 2 samples class 1. Total 10.

$$p_0 = 0.8, \quad p_1 = 0.2$$
$$\text{Gini} = 1 - (0.8^2 + 0.2^2) = 1 - (0.64 + 0.04) = 0.32$$

**Why Gini Works:**

The term $p_k(1 - p_k)$ represents the probability of selecting one sample of class $k$ and one of another class. Summing over classes and subtracting from 1 gives the total misclassification probability.

**Related Assignment Questions:** Q1 (direct calculation).

### Entropy and Information Gain

**Definition:** **Entropy** measures the average information content or "surprise" in a distribution. High entropy = high uncertainty.

**Formula:**

$$H(R_m) = -\sum_{k=1}^{K} p_k \log_2(p_k)$$

where $p_k \log_2(p_k) = 0$ when $p_k = 0$ (by convention).

**Interpretation:**

- **H = 0:** Pure node (all one class). No information needed to describe it.
- **H = 1:** Binary classification with 50-50 split. Maximum uncertainty.

**Example (Q2 from Assignment):**

Node: 3 samples class A, 1 sample class B. Total 4.

$$p_A = 0.75, \quad p_B = 0.25$$
$$H = -0.75 \log_2(0.75) - 0.25 \log_2(0.25)$$

**Calculation:**
- $\log_2(0.75) \approx -0.415$, so $-0.75 \times (-0.415) = 0.311$
- $\log_2(0.25) = -2$, so $-0.25 \times (-2) = 0.5$
- $H = 0.311 + 0.5 = 0.811$

**Information Gain:**

When considering a split, the gain is the reduction in entropy:

$$\text{IG} = H(\text{parent}) - \left[\frac{N_L}{N} H(\text{left}) + \frac{N_R}{N} H(\text{right})\right]$$

**Related Assignment Questions:** Q2 (entropy calculation).

### Gini vs. Entropy

Both are impurity measures. Gini is slightly faster to compute (no logarithms). Entropy has information-theoretic interpretation. Empirically, they produce similar trees.

---

## 3. Regression Trees

### Prediction in Leaf Nodes

**Definition:** In a regression tree, the prediction for a region $R_m$ is the **mean** of all target values in that region:

$$\hat{y}_m = \frac{1}{N_m} \sum_{i \in R_m} y_i$$

where $N_m$ is the number of samples in region $m$.

**Example (Q3 from Assignment):**

Targets: [2, 4, 6, 8]. Prediction = Mean = $(2+4+6+8)/4 = 5$.

**Why Mean?**

The mean minimizes the Mean Squared Error (MSE) loss. For any distribution, the value that minimizes $\mathbb{E}[(y - c)^2]$ is $c = \mathbb{E}[y]$ (the expected value).

### Impurity: Variance / MSE

**Definition:** For regression, impurity is the **Mean Squared Error (variance)** within a region:

$$I(R_m) = \frac{1}{N_m} \sum_{i \in R_m} (y_i - \hat{y}_m)^2$$

where $\hat{y}_m$ is the mean of the region.

**Example (Q4 from Assignment):**

Targets: [1, 3, 5]. Mean = 3.

$$I = \frac{1}{3}[(1-3)^2 + (3-3)^2 + (5-3)^2] = \frac{1}{3}[4 + 0 + 4] = \frac{8}{3}$$

**Split Quality (Q6 from Assignment):**

When evaluating a split, use **weighted average variance**:

$$\text{Weighted Variance} = \frac{N_L}{N} \text{Var}(\text{Left}) + \frac{N_R}{N} \text{Var}(\text{Right})$$

The split that **minimizes** this weighted variance is chosen.

**Related Assignment Questions:** Q3 (leaf prediction), Q4 (node variance), Q6 (weighted post-split variance).

---

## 4. Ensemble Methods: The Power of Many

### The Ensemble Learning Paradigm

**Core Principle:**

A single model can have high variance (overfitting) or high bias (underfitting). Ensemble methods combine multiple "weak" learners to reduce variance, bias, or both.

**Weak Learner Definition:**

A learner that performs only slightly better than random guessing (e.g., a shallow decision tree).

**Strong Learner Definition:**

A model with high accuracy. Ensembles combine weak learners into strong learners through aggregation.

### Two Aggregation Strategies

1. **Parallel (Bagging/Random Forests):** Train many weak learners independently, then average.
   - Goal: Reduce **variance**.
   - Why: Independent errors cancel out; systematic biases compound.

2. **Sequential (Boosting/Gradient Boosting):** Train weak learners sequentially, each correcting previous errors.
   - Goal: Reduce **bias**.
   - Why: Each new learner focuses on hard examples the ensemble misses.

---

## 5. Bagging (Bootstrap Aggregating)

### Bootstrap Sampling

**Definition:** **Bootstrap sampling** is sampling from a dataset **with replacement**, creating a new dataset of the same size where some samples appear multiple times and some don't appear at all.

**Example:**

Original: [A, B, C, D, E]. Bootstrap samples might be: [A, A, B, D, E], [B, C, C, E, E], etc.

### The Mathematics of Bootstrap

**Probability a Sample is Omitted:**

For a dataset of size $n$, when drawing $n$ samples with replacement:
- Probability a specific sample is **not** drawn on a single trial: $1 - \frac{1}{n}$.
- Probability it's **never** drawn in $n$ trials: $\left(1 - \frac{1}{n}\right)^n$.

**Taking the Limit:**

$$\lim_{n \to \infty} \left(1 - \frac{1}{n}\right)^n = \frac{1}{e} \approx 0.368$$

**Interpretation:**

- Each bootstrap sample omits approximately **36.8%** of the original data.
- Each bootstrap sample includes approximately **63.2%** of the original data (with duplicates).
- Unique samples in a bootstrap: approximately 63.2% of original size.

**Example (Q5 & Q8 from Assignment):**

Q5: For large $n$, fraction included at least once = $1 - 1/e \approx 0.632$.
Q8: From 1000 points, omitted ≈ $1000 \times 0.368 = 368$.

**Related Assignment Questions:** Q5, Q8.

### Bagging Ensemble Averaging

**Algorithm:**

1. For $b = 1$ to $B$:
   - Generate a bootstrap sample from the original data.
   - Train a weak learner $h_b$ on this sample.
2. For prediction, average the predictions:
   $$\hat{y}_{\text{bag}} = \frac{1}{B} \sum_{b=1}^{B} h_b(x)$$

**Variance Reduction Formula:**

Assuming the $B$ weak learners are trained on correlated datasets (bootstrap samples from the same original data), their predictions are not independent. The ensemble variance is:

$$\text{Var}(\bar{f}) = \rho \sigma^2 + \frac{1-\rho}{B} \sigma^2$$

where:
- $\sigma^2$: Variance of a single weak learner.
- $\rho$: Pairwise correlation between any two weak learners' predictions.
- $B$: Number of weak learners.

**Example (Q7 from Assignment):**

$\sigma^2 = 9$, $\rho = 0.2$, $B = 5$.

$$\text{Var}(\bar{f}) = 0.2 \times 9 + \frac{0.8}{5} \times 9 = 1.8 + 1.44 = 3.24$$

The ensemble variance (3.24) is much lower than a single tree (9.0).

**Key Insight from the Formula:**

- **If $\rho = 0$ (uncorrelated learners):** Variance scales as $\sigma^2/B$. Adding more trees always helps.
- **If $\rho > 0$ (correlated learners):** The first term $\rho \sigma^2$ is a lower bound. No matter how many trees, variance won't drop below this floor.

**The Bagging Bottleneck:**

Bootstrap samples are highly correlated (they're all drawn from the same original data). Thus $\rho$ is positive, limiting variance reduction.

**Related Assignment Questions:** Q7 (direct variance calculation).

---

## 6. Random Forests

### The Innovation: Decorrelate the Trees

**Problem with Bagging:**

Trees trained on bootstrap samples are correlated because the samples are very similar to the original data. This correlation $\rho$ limits variance reduction (Q7 shows the $\rho \sigma^2$ floor).

**Solution: Random Forests**

At **each node split**, randomly select a subset of features to consider for the split. This forces different trees to learn different feature combinations, reducing correlation.

**Feature Subsampling:**

At each node, the algorithm:
1. Randomly select $m$ features from the total $p$ features.
2. Search for the best split among only these $m$ features.
3. The remaining $p - m$ features are not considered at this node.

**Typical Choice:**

For classification: $m \approx \sqrt{p}$ (square root of total features).
For regression: $m \approx p/3$.

**Example (Q9 from Assignment):**

Total features: 16. Features considered per split: 4.

Fraction: $4/16 = 0.25 = 25\%$.

**Effect on Correlation:**

By forcing trees to use different feature subsets, random forests produce predictions that are much less correlated ($\rho$ closer to 0), allowing the bagging formula to achieve much better variance reduction.

**Related Assignment Questions:** Q9 (feature fraction).

---

## 7. Gradient Boosting

### Sequential, Not Parallel

Unlike bagging/random forests (train many independent learners in parallel), **boosting** trains learners **sequentially**, where each new learner focuses on correcting the mistakes of the previous ensemble.

### The Boosting Update

**The Additive Model:**

$$H_m(x) = H_{m-1}(x) + \alpha_m h_m(x)$$

where:
- $H_{m-1}(x)$: Prediction from the ensemble at iteration $m-1$.
- $h_m(x)$: New weak learner prediction at iteration $m$.
- $\alpha_m$: Step size (learning rate), typically 0.01–0.1.

**Interpretation:**

The new ensemble prediction is the old prediction plus a small, scaled contribution from the new learner.

**Example (Q10 from Assignment):**

$H_{m-1}(x) = 5$, $h_m(x) = 2$, $\alpha = 0.3$.

$$H_m(x) = 5 + 0.3 \times 2 = 5 + 0.6 = 5.6$$

### Residual-Based Learning

**Key Idea:**

The new learner $h_m$ is trained to predict the **negative gradient** (residuals) of the loss function:

$$g_i = -\frac{\partial L(y_i, H_{m-1}(x_i))}{\partial H_{m-1}(x_i)}$$

For squared error loss $L = (y - \hat{y})^2$, the negative gradient is the residual: $g_i = y_i - H_{m-1}(x_i)$.

**Intuition:**

If the ensemble predicts $H_{m-1}(x) = 5$ but the true value is $y = 8$, the residual is $8 - 5 = 3$. The new learner tries to predict this residual (the mistake). Adding 0.3 times this prediction brings the ensemble closer to the true value.

**Reduces Bias, Not Variance:**

Unlike bagging (trains independent learners to reduce variance), boosting reduces **bias** by iteratively fitting residuals, pushing the ensemble toward better predictions.

### Gradient Boosting vs. Bagging

| Aspect | Bagging | Boosting |
|--------|---------|----------|
| **Training** | Parallel, independent | Sequential, dependent |
| **Data Source** | Bootstrap samples | Original data, residual targets |
| **Goal** | Reduce variance | Reduce bias |
| **Correlation** | Tries to reduce | Increases over iterations |
| **Weak Learner** | Often full trees | Shallow trees (depth 1–5) |

**Related Assignment Questions:** Q10 (gradient boosting update).

---

## 8. Gradient Boosting: A Deeper Dive

### Why Use Negative Gradient?

**Newton's Method Connection:**

Gradient descent updates parameters to minimize a loss function. In machine learning, we're optimizing model predictions. The negative gradient points toward decreasing the loss.

**The Boosting Algorithm:**

```
Initialize: H_0(x) = 0 (or mean target value)
For m = 1 to M:
  1. Compute residuals: r_i = -∇_H L(y_i, H_{m-1}(x_i))
  2. Train weak learner h_m on features x, targets r
  3. Update: H_m(x) = H_{m-1}(x) + α h_m(x)
```

### Loss Function Flexibility

Gradient boosting works with **any differentiable loss function**:

- **Regression (L2 Loss):** $L = (y - \hat{y})^2$. Residual = $y - \hat{y}$.
- **Classification (Log Loss):** $L = -y \log(\hat{y}) - (1-y) \log(1-\hat{y})$. Gradient points toward correcting misclassifications.
- **Robust Regression (Huber Loss):** Down-weights outliers.

Different losses → different residuals → different learning dynamics.

### The Importance of the Learning Rate $\alpha$

- **Large $\alpha$ (e.g., 0.5):** Aggressive updates, fast training, high risk of overfitting.
- **Small $\alpha$ (e.g., 0.01):** Conservative updates, slow training, better generalization.

Small learning rates are preferred in practice; more trees are needed but the final model is more robust.

---

## 9. Tree Depth and Complexity

### Shallow vs. Deep Trees

**Shallow Trees (Depth ≤ 5):**
- High bias (underfitting): Can't fit complex patterns.
- Low variance: Similar on different data samples.
- Fast training and prediction.

**Deep Trees (Depth > 15):**
- Low bias (fits complex patterns).
- High variance (overfitting): Memorize training data.
- Slow training and prediction.

### Ensemble Strategy

**Bagging:**
- Uses deep, full-grown trees (low bias individually).
- Averaging reduces variance.

**Boosting:**
- Uses shallow trees (depth 1–5), called "stumps."
- Sequentially fit to push bias toward zero.

---

## 10. Practical Considerations

### Hyperparameter Tuning

**Tree-Based Models:**

- **Max Depth:** Control complexity. Shallow (3–5) for bagging, deeper (8–15) for boosting.
- **Min Samples Split:** Minimum samples to split a node. Prevents spurious splits on noise.
- **Number of Estimators (B):** More trees reduce variance in bagging, reduce bias in boosting.

**Gradient Boosting Specifics:**

- **Learning Rate $\alpha$:** Typically 0.01–0.1. Smaller is often better (trade off with number of iterations).
- **Subsample:** Fraction of data sampled at each iteration (regularization, similar to dropout).

### When to Use Each Method

| Method | Best For | Strengths | Weaknesses |
|--------|----------|-----------|-----------|
| **Decision Tree** | Interpretability | Simple, fast, handles mixed data types | Prone to overfitting |
| **Random Forest** | Tabular data, balance | Fast, parallel, works well generally | Less interpretable than trees |
| **Gradient Boosting** | Maximum accuracy | Often best accuracy on tabular data | Requires careful tuning, slow training |

---

## Key Takeaways

| Concept | Definition | Formula | Related Q |
|---------|-----------|---------|-----------|
| **Gini Impurity** | Probability of misclassification | $1 - \sum p_k^2$ | Q1 |
| **Entropy** | Information content, uncertainty | $-\sum p_k \log_2 p_k$ | Q2 |
| **Regression Tree Prediction** | Mean of target values in region | $\hat{y}_m = \frac{1}{N_m}\sum y_i$ | Q3 |
| **Regression Impurity (Variance)** | MSE within region | $\frac{1}{N_m}\sum(y_i - \bar{y})^2$ | Q4 |
| **Bootstrap Inclusion** | Fraction of unique samples in bootstrap | $1 - 1/e \approx 0.632$ | Q5, Q8 |
| **Weighted Variance Post-Split** | Variance reduction from split | $\frac{N_L}{N}\text{Var}_L + \frac{N_R}{N}\text{Var}_R$ | Q6 |
| **Bagging Ensemble Variance** | Variance with correlation | $\rho\sigma^2 + \frac{1-\rho}{B}\sigma^2$ | Q7 |
| **Random Forest Features** | Feature subsampling fraction | $m/p$ | Q9 |
| **Gradient Boosting Update** | Sequential ensemble update | $H_m = H_{m-1} + \alpha h_m$ | Q10 |

---

## Summary

**Week 11 introduces the non-neural-network side of machine learning:**

1. **Decision Trees:** Recursively partition feature space using impurity measures (Gini, Entropy for classification; Variance for regression).

2. **Regression Trees:** Predict continuous values using leaf means; optimize splits by minimizing weighted variance.

3. **Bagging & Random Forests:** Reduce variance by training many trees on bootstrap samples. Random Forests additionally subsample features to decorrelate trees.

4. **Gradient Boosting:** Reduce bias by sequentially training trees to predict residuals, using a small learning rate to prevent overfitting.

5. **Practical Impact:** Tree-based methods, especially gradient boosting (XGBoost, LightGBM), often outperform neural networks on tabular data—the most common real-world data format.

**Bridge to Production ML:** Understanding both neural networks (Weeks 1–10) and tree-based methods (Week 11) is essential. Modern ML practitioners use the right tool for each problem: transformers for text/vision, trees for tabular data.


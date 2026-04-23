# Week 12: Theoretical Notes — AdaBoost, Cross-Validation, and Unsupervised Learning

## Overview

Week 12 is the **capstone** of the Mathematical Foundations course, uniting two critical paradigms: **supervised ensemble learning** (concluding with AdaBoost) and the cross the threshold into **unsupervised learning**. The first half introduces AdaBoost, a foundational boosting algorithm that adaptively reweights training samples. The second half shifts perspective entirely: instead of predicting labeled targets $y$, we assume data arises from hidden latent variables $z$ and aim to **discover the underlying structure** of the data. This includes discrete clustering (k-means, Gaussian Mixture Models) and continuous dimensionality reduction (PCA, autoencoders). By week's end, you'll understand both the tools dominating supervised learning (trees + boosting) and the unsupervised methods that power exploratory data analysis, feature extraction, and anomaly detection in production systems.

---

## 1. AdaBoost: Adaptive Boosting

### Algorithm Overview

**AdaBoost (Adaptive Boosting)** is a sequential ensemble method that improves weak learners by **reweighting training samples** based on previous mistakes. Unlike bagging (parallel) and gradient boosting (residual fitting), AdaBoost explicitly tracks data-point weights and increases weights on misclassified points.

**Target Variable:** Binary classification, $y_i \in \{-1, +1\}$ (sometimes $\{0, 1\}$).

**Weak Learner:** Any classifier with error slightly better than random (e.g., shallow decision trees, depth 1–3).

### Initialization

**Data-Point Weights:**

At iteration $m = 1$, all samples start with equal weight:

$$w_i^{(1)} = \frac{1}{N}$$

where $N$ is the total number of samples.

**Interpretation:**

Initially, every sample is equally important to the learning algorithm. The weights form a probability distribution: $\sum_{i=1}^N w_i^{(1)} = 1$.

**Note on Alternative Initialization:**

Some variants (particularly in imbalanced classification) initialize separate weight distributions for positive and negative classes:

$$w_i^{(1)} = \begin{cases} \frac{1}{2N_+} & \text{if } y_i = +1 \\ \frac{1}{2N_-} & \text{if } y_i = -1 \end{cases}$$

For perfectly balanced data ($N_+ = N_- = N/2$), this reduces to $w_i^{(1)} = \frac{1}{2N}$.

**Related Assignment Questions:** Q1 (initialization detail).

### Training a Weak Learner

At iteration $m$, train a weak learner $h_m(x)$ that respects the current weights $w^{(m)}$. The **weighted error** is:

$$\epsilon_m = \sum_{i: h_m(x_i) \neq y_i} w_i^{(m)}$$

This sums the weights of all misclassified samples. Interpretation: If a heavily weighted sample is misclassified, the error is high; if only lightly weighted samples are wrong, the error is low.

### Computing Classifier Weight $\alpha_m$

Once $\epsilon_m$ is computed, calculate how much "voting power" this weak learner receives:

$$\alpha_m = \frac{1}{2} \log \left( \frac{1 - \epsilon_m}{\epsilon_m} \right)$$

**Interpretation:**

- **If $\epsilon_m = 0$ (perfect classifier):** $\alpha_m \to \infty$ (infinite weight, dominates ensemble).
- **If $\epsilon_m = 0.5$ (random guessing):** $\alpha_m = 0$ (no contribution to ensemble).
- **If $\epsilon_m = 1$ (all wrong):** $\alpha_m \to -\infty$ (inverted predictions help).

The formula is monotonically decreasing in $\epsilon_m$: better accuracy → higher $\alpha_m$.

**Related Assignment Questions:** Q2 (purpose of computing weighted error).

### Updating Sample Weights

After computing $\alpha_m$, increase weights on misclassified points:

$$w_i^{(m+1)} = w_i^{(m)} \exp\left(-\alpha_m y_i h_m(x_i)\right)$$

**Expansion of the Exponent:**

- If $h_m(x_i) = y_i$ (correct): $-\alpha_m y_i h_m(x_i) = -\alpha_m (1) = -\alpha_m$, so $w_i^{(m+1)} = w_i^{(m)} \exp(-\alpha_m)$ (weight decreases).
- If $h_m(x_i) \neq y_i$ (incorrect): $-\alpha_m y_i h_m(x_i) = \alpha_m (1) = \alpha_m$, so $w_i^{(m+1)} = w_i^{(m)} \exp(\alpha_m)$ (weight increases).

**Normalization:**

After updating all weights, renormalize so they sum to 1:

$$w_i^{(m+1)} \leftarrow \frac{w_i^{(m+1)}}{\sum_{j=1}^N w_j^{(m+1)}}$$

**Intuition:**

The algorithm "forces" the next weak learner to focus on the hard examples (those with high weights). Over iterations, the ensemble learns to correct the mistakes of previous learners.

### Final Prediction

After training $M$ weak learners, the final ensemble prediction is:

$$H(x) = \text{sign}\left(\sum_{m=1}^M \alpha_m h_m(x)\right)$$

Each weak learner votes, weighted by its $\alpha_m$. The sign of the sum determines the final class.

### Connection to Exponential Loss

**Theorem:** AdaBoost is equivalent to **gradient boosting** with **exponential loss**:

$$L(y, H(x)) = \exp(-y \cdot H(x))$$

**Proof Sketch:**

The weight update rule $w_i^{(m+1)} = w_i^{(m)} \exp(-\alpha_m y_i h_m(x_i))$ accumulates exponential misclassification penalties. Fitting weak learners to weighted data is equivalent to minimizing the exponential loss through stage-wise optimization.

**Implication:** AdaBoost shares the exponential loss's aggressive penalization of misclassifications—a single point with very negative score can dominate the loss. This makes AdaBoost sensitive to outliers, unlike gradient boosting with squared error or robust losses.

---

## 2. Cross-Validation

### The Three Data Splits

When building a machine learning model, the data must be carefully partitioned:

**Training Set:** Used to update model parameters (weights in neural networks, splits in trees, etc.). Typically 60–70% of data.

**Validation Set:** Used **during training** to evaluate the model and **choose hyperparameters**. Typically 10–15% of data. This set is not used to update parameters, only to guide hyperparameter selection.

**Test Set:** Reserved for **final evaluation only**. Used strictly at the end to report unbiased metrics. Never used during training or validation. Typically 15–20% of data.

### Why Three Sets?

1. **Training Set Alone:** Model memorizes the data (overfitting). Training error is misleading.
2. **Train + Test:** If you tune hyperparameters using test errors, you're overfitting to the test set. Test results become biased.
3. **Train + Validation + Test:** Validation guides hyperparameter tuning; test provides honest final metrics.

### K-Fold Cross-Validation

When data is **limited**, a single train-validation-test split wastes data. **K-fold cross-validation** efficiently uses all data:

**Algorithm:**

1. Divide the dataset into $K$ disjoint folds (e.g., $K=5$ or $K=10$).
2. For $k = 1$ to $K$:
   - Validation set = Fold $k$.
   - Training set = Folds $\{1, 2, \ldots, K\} \setminus \{k\}$.
   - Train model and record validation error.
3. **Average Validation Error:** $\text{CV Error} = \frac{1}{K} \sum_{k=1}^K \text{Error}_k$.

**Advantage:** Every data point appears in exactly one validation fold and $K-1$ training folds. No data is wasted.

**Typical Choice:** $K = 5$ or $K = 10$.

**Related Assignment Questions:** Q10 (purpose of validation set).

---

## 3. Unsupervised Learning Paradigm

### The Latent Variable Model

In unsupervised learning, assume data arises from a hidden "latent variable" $z$:

$$P_\theta(x) = \int P_\theta(x | z) P_\theta(z) dz$$

where:
- $P_\theta(z)$: **Prior** over latent variables.
- $P_\theta(x | z)$: **Likelihood** of observing $x$ given $z$.
- $P_\theta(x)$: **Marginal likelihood** (what we observe).

**Goal:** Estimate parameters $\theta$ that explain the observed data $D = \{x_1, \ldots, x_n\}$.

### Discrete vs. Continuous Latent Variables

**Discrete $z \in \{1, 2, \ldots, K\}$ → Clustering:**

Each data point comes from one of $K$ clusters. The discrete choice is the latent variable.

- **K-Means:** Hard clustering (deterministic assignment).
- **Gaussian Mixture Model (GMM):** Soft clustering (probabilistic assignment).

**Continuous $z \in \mathbb{R}^d$ with $d \ll D$ → Dimensionality Reduction:**

Data is generated from a lower-dimensional latent space that's then expanded to high dimensions.

- **Principal Component Analysis (PCA):** Linear projection to lower dimensions.
- **Autoencoders:** Non-linear projection using neural networks.

---

## 4. K-Means Clustering

### Problem Setup

Given $N$ data points $\{x_1, \ldots, x_N\}$ in $\mathbb{R}^d$, partition them into $K$ disjoint clusters.

**Parameters:** Cluster centers (centroids) $\{\mu_1, \ldots, \mu_K\}$.

### Algorithm

**Initialize:** Randomly select $K$ points as initial centroids.

**Repeat until convergence:**

**Assignment Step:** Assign each point to its nearest centroid.

$$c_i = \arg\min_k \|x_i - \mu_k\|_2^2$$

where $c_i \in \{1, \ldots, K\}$ is the cluster assignment for point $i$.

**Update Step:** Recalculate centroids as the mean of assigned points.

$$\mu_k = \frac{1}{|C_k|} \sum_{i \in C_k} x_i$$

where $C_k = \{i : c_i = k\}$ is the set of indices assigned to cluster $k$.

**Convergence:** Stop when assignments no longer change (or objective no longer decreases).

### Objective Function

K-Means implicitly minimizes:

$$J = \sum_{i=1}^N \|x_i - \mu_{c_i}\|_2^2$$

the **sum of squared distances** from points to their assigned centroids.

### Properties

**Hard Clustering:** Each point belongs to exactly one cluster (discrete assignment).

**Greedy Algorithm:** K-Means is **not guaranteed** to find the global optimum; it finds a local optimum. Results depend on initialization.

**Computational Complexity:** Assignment step is $O(NKd)$; update step is $O(Nd)$. Typically fast even for large datasets.

**Euclidean Distance:** K-Means assumes clusters are roughly **spherical** (same variance in all directions). It fails on elongated or non-convex clusters.

**Related Assignment Questions:** Q7 (assignment step based on squared Euclidean distance).

---

## 5. Gaussian Mixture Models (GMM)

### Probabilistic Clustering

Unlike K-Means (hard assignment), GMM assigns a **probability distribution** over clusters.

**Model:**

$$P_\theta(x) = \sum_{k=1}^K \pi_k \mathcal{N}(x | \mu_k, \Sigma_k)$$

where:
- $\pi_k = P(z = k)$: Probability of cluster $k$ (**mixing weight**).
- $\mathcal{N}(x | \mu_k, \Sigma_k)$: Gaussian component $k$ with mean $\mu_k$ and covariance $\Sigma_k$.

### Posterior (Soft Assignment)

Given a data point $x$, the posterior probability of it belonging to cluster $k$ is:

$$P_\theta(z = k | x) = \frac{\pi_k \mathcal{N}(x | \mu_k, \Sigma_k)}{\sum_{j=1}^K \pi_j \mathcal{N}(x | \mu_j, \Sigma_j)}$$

This is a **distribution over clusters**, not a single hard label. A point can be 40% Cluster A, 60% Cluster B.

**Related Assignment Questions:** Q9 (interpretation of posterior as soft assignment).

### EM Algorithm

Fit GMM to data using Expectation-Maximization:

**E-Step (Expectation):** Compute posterior probabilities $P_\theta(z | x_i)$ for all points.

**M-Step (Maximization):** Update parameters $\theta = \{\pi_k, \mu_k, \Sigma_k\}$ using the weighted posteriors.

The algorithm alternates until convergence.

### GMM to K-Means Connection

**Theorem:** As the variance of GMM components shrinks to zero, soft assignments collapse to hard assignments.

**Mathematical Limit:**

$$\lim_{\sigma \to 0} P_\theta(z = k | x) \to \begin{cases} 1 & \text{if } k = \arg\min_j \|x - \mu_j\|_2^2 \\ 0 & \text{otherwise} \end{cases}$$

The Gaussian probabilities become infinitely sharp (Dirac-delta-like), and soft GMM becomes hard K-Means.

**Interpretation:** K-Means is the limit of GMM as variance → 0.

**Related Assignment Questions:** Q8 (Dirac-delta limit).

---

## 6. Principal Component Analysis (PCA)

### Motivation

In high-dimensional data, many features may be **correlated** or **redundant**. PCA finds a **linear projection** to a lower-dimensional subspace that **preserves maximum variance**.

### The PCA Projection

Project data onto a lower-dimensional subspace spanned by columns of $W \in \mathbb{R}^{D \times d}$ (where $d < D$):

$$z = W^T x$$

**Goal:** Choose $W$ to maximize variance in the projected space while minimizing information loss.

### The Optimization Problem

**Objective:**

Maximize the **projected variance** subject to orthonormality:

$$\max_W \text{Var}(W^T X) = \max_W W^T \hat{\Sigma} W$$

**Constraint:**

$$W^T W = I_d$$

(Columns of $W$ are orthonormal.)

### The Solution: Eigenvalues and Eigenvectors

**Theorem:** The optimal $W$ is constructed from the top $d$ eigenvectors of the **sample covariance matrix**:

$$\hat{\Sigma} = \frac{1}{n} X^T X$$

(Assuming centered data; subtract the mean first.)

**Specifically:**

Sort eigenvectors by descending eigenvalues:

$$\lambda_1 \geq \lambda_2 \geq \cdots \geq \lambda_D$$

Choose the first $d$ eigenvectors:

$$W = [v_1, v_2, \ldots, v_d]$$

where $v_i$ corresponds to eigenvalue $\lambda_i$.

### Variance Explained

The variance preserved in the $i$-th principal component is equal to its eigenvalue:

$$\text{Var}(z_i) = \lambda_i$$

**Total variance explained:** $\frac{\sum_{i=1}^d \lambda_i}{\sum_{i=1}^D \lambda_i}$.

To preserve 95% of variance, select the smallest $d$ such that the fraction above is ≥ 0.95.

### Key Properties

- **Linear:** Projects data onto linear subspaces. Cannot capture nonlinear manifolds.
- **Unsupervised:** No labels required; purely variance-maximizing.
- **Efficient:** Eigenvalue decomposition is fast even for moderately high dimensions.

**Related Assignment Questions:** Q5 (first PC is largest eigenvector), Q6 (maximization objective).

---

## 7. Autoencoders

### Architecture

An **autoencoder** is a neural network with three parts:

1. **Encoder** $E_\phi(x)$: Maps input $x$ to a lower-dimensional latent code $z$.
2. **Bottleneck:** The latent representation $z$ with dimension $d_z \ll d_x$.
3. **Decoder** $D_\theta(z)$: Maps latent code back to reconstructed input $\hat{x}$.

**End-to-End:**

$$\hat{x} = D_\theta(E_\phi(x))$$

### Training Objective

Minimize **reconstruction loss**:

$$\mathcal{L}(\theta, \phi) = \frac{1}{n} \sum_{i=1}^n \|x_i - \hat{x}_i\|_2^2$$

The network learns to compress information into $z$ while minimizing reconstruction error. The **bottleneck forces information compression**, creating a useful feature representation.

### Comparison to PCA

| Aspect | PCA | Autoencoder |
|--------|-----|-------------|
| **Linearity** | Linear projection | Non-linear (with hidden layers) |
| **Capacity** | Limited to linear relationships | Can learn complex, non-linear manifolds |
| **Interpretability** | Eigenvalues have clear variance meaning | Black box; harder to interpret |
| **Computation** | Eigenvalue decomposition | Backpropagation |

**A linear autoencoder (no hidden layers) is equivalent to PCA.**

### Applications

- **Feature extraction:** Use the encoder to create compact representations.
- **Denoising:** Train on noisy data; the autoencoder learns to ignore noise.
- **Anomaly detection:** Points with high reconstruction error are anomalous.
- **Generative models:** With additional constraints (VAE), autoencoders can generate new data.

**Related Assignment Questions:** Q3 (reconstruction loss), Q4 (encoder-decoder structure).

---

## 8. Practical Considerations

### Hyperparameter Tuning

**K-Means:**
- Number of clusters $K$: Use **elbow method** or **silhouette score**.
- Initialization: Run multiple random initializations, pick best result.

**GMM:**
- Number of components $K$: Use **Bayesian Information Criterion (BIC)** or **Akaike Information Criterion (AIC)**.
- Covariance type: Full, tied, diagonal, spherical (trades off expressiveness vs. computational cost).

**PCA:**
- Number of components $d$: Select to preserve 90–95% of variance.

**Autoencoders:**
- Bottleneck dimension: Start with $d_z = d_x / 2$ or smaller; tune based on reconstruction vs. generalization trade-off.
- Architecture: Symmetric encoder/decoder, or asymmetric. Deeper networks can capture more complex manifolds.

### Standardization

**PCA and Autoencoders:** Always **standardize (z-score)** features before training. PCA is sensitive to scale; features with large variances dominate.

**K-Means:** Also benefits from standardization, as it uses Euclidean distance (scale-dependent).

---

## Key Takeaways

| Concept | Definition | Formula | Related Q |
|---------|-----------|---------|-----------|
| **AdaBoost Init** | Uniform weights summing to 1 | $w_i = 1/N$ (standard) | Q1 |
| **Weighted Error** | Sum of weights of misclassified points | $\epsilon_m = \sum_{i: \text{wrong}} w_i^{(m)}$ | Q2 |
| **Classifier Weight** | Voting power of weak learner $m$ | $\alpha_m = \frac{1}{2}\log\frac{1-\epsilon_m}{\epsilon_m}$ | Q2 |
| **Autoencoder Loss** | MSE between input and reconstruction | $\frac{1}{n}\sum \|x_i - \hat{x}_i\|_2^2$ | Q3 |
| **Autoencoder Structure** | Encoder → Latent → Decoder | $\hat{x} = D_\theta(E_\phi(x))$ | Q4 |
| **First PC** | Direction of maximum variance | Eigenvector with largest eigenvalue | Q5 |
| **PCA Objective** | Maximize variance in projection | $\max_W W^T \Sigma W$ s.t. $W^TW=I$ | Q6 |
| **K-Means Assignment** | Cluster by nearest centroid | $\arg\min_k \|x_i - \mu_k\|_2^2$ | Q7 |
| **GMM Limit** | Soft → Hard as variance → 0 | Dirac-delta-like assignments | Q8 |
| **GMM Posterior** | Soft cluster assignment probability | $P_\theta(z\|x) = \frac{\pi_k \mathcal{N}(x\|\mu_k,\Sigma_k)}{\sum}$ | Q9 |
| **Validation Set** | Choose hyperparameters, not train | Separate from train and test | Q10 |

---

## Summary

**Week 12 Unifies Four Major Concepts:**

1. **AdaBoost:** Adaptive reweighting to focus on hard examples. Exponential loss penalizes misclassifications aggressively. Sequential ensemble building.

2. **Cross-Validation:** Train/Validation/Test splits to avoid overfitting. K-fold uses all data efficiently.

3. **Clustering:** K-Means (hard, fast, Euclidean), GMM (soft, probabilistic, Bayesian). GMM is a generalization; K-Means emerges in the limit.

4. **Dimensionality Reduction:** PCA (linear, variance-maximizing, eigenvalue-based). Autoencoders (non-linear, deep learning, reconstruction-based).

**Bridge Between Paradigms:**

- Weeks 1–11 focused on **supervised learning**: predicting $y$ given $x$.
- Week 12 transitions to **unsupervised learning**: discovering structure in $x$ alone.
- Modern ML combines both: supervised for specific tasks, unsupervised for exploratory analysis and feature engineering.

**Production Relevance:**

- **AdaBoost:** Historical importance; superseded by gradient boosting but still used in some systems.
- **K-Means:** Industry standard for clustering on tabular data (customer segmentation, genomics).
- **PCA:** Quick baseline for dimensionality reduction; often used before other algorithms.
- **Autoencoders:** Deep learning approach; VAEs and more advanced variants are research-active.


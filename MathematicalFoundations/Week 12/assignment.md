# Week 12: Assignment 12 — Mathematical Foundations of Machine Learning (CS02)

| Metric | Value |
|--------|-------|
| Due | 2026-04-15, 23:59 IST |
| Submitted | 2026-04-05, 23:53 IST |
| Score | 9/10 |

---

## Question Set 1: AdaBoost Fundamentals

### Q1 (1 point)

**In AdaBoost, how are the data-point weights initialized at the beginning of training for a dataset of N samples?**

- [ ] A) $w_i = 0$
- [x] B) $w_i = \frac{1}{N}$, $i = 1, \ldots, N$
- [ ] C) $w_i = \frac{1}{2N}$
- [ ] D) $w_i = y_i$

**Submitted Answer:** B

**NPTEL "Correct" Answer:** C ($w_i = \frac{1}{2N}$)

**Your Score:** 0/1 ✗ (per NPTEL system)

**Answer Explanation (With Important Caveats):**

This question highlights a subtle discrepancy between **standard machine learning literature** and this specific NPTEL course iteration. Both answers have mathematical validity, but they represent different algorithmic choices.

**The Standard AdaBoost Initialization (Your Answer: B):**

In the canonical AdaBoost algorithm published by Freund and Schapire, weights are initialized uniformly:

$$w_i^{(1)} = \frac{1}{N}, \quad i = 1, 2, \ldots, N$$

**Why This Works:**

The weights form a **probability distribution** (they sum to 1 and are non-negative). At each iteration, the weighted error is computed as:

$$\epsilon_m = \sum_{i=1}^N w_i^{(m)} \mathbb{1}[h_m(x_i) \neq y_i]$$

where $\mathbb{1}[\cdot]$ is the indicator function. Since weights are a probability distribution, $\epsilon_m$ is interpreted as the **probability of error** on a randomly sampled point.

**Evidence from Notation:**

Your course notes explicitly define $w_i^1 = \frac{1}{N}$ in the AdaBoost section, confirming this is the primary definition taught.

**The NPTEL Course Variant Initialization (NPTEL Answer: C):**

$$w_i^{(1)} = \frac{1}{2N}$$

This initialization is used in some **balanced binary classification variants** where the positive and negative classes are treated separately:

$$w_i^{(1)} = \begin{cases} \frac{1}{2N_+} & \text{if } y_i = +1 \\ \frac{1}{2N_-} & \text{if } y_i = -1 \end{cases}$$

where $N_+$ and $N_-$ are the number of positive and negative samples.

**When does this reduce to $\frac{1}{2N}$?**

If the dataset is **perfectly balanced** ($N_+ = N_- = N/2$), then:

$$w_i^{(1)} = \frac{1}{2 \times N/2} = \frac{1}{N}$$

which is the standard initialization. However, for **imbalanced data**, this ensures each class contributes equally to the initial error calculation.

**Why Might NPTEL Prefer This?**

Balanced initialization is pedagogically useful for imbalanced classification, a common real-world scenario. However, it's a **specific variant**, not the canonical algorithm.

**Pedagogical Assessment:**

Both answers are mathematically correct within their respective frameworks. Your answer aligns with:
- Freund & Schapire's original AdaBoost paper
- Most machine learning textbooks (e.g., Murphy, Bishop)
- Standard implementations (scikit-learn)

**Key Insight:** When learning from lectures, always verify algorithmic details against multiple sources. Variants exist; the "correct" answer depends on context.

**Related Topics:** [AdaBoost: Algorithm Overview, Initialization](notes.md#1-adaboost-adaptive-boosting)

---

### Q2 (1 point)

**In AdaBoost, the weighted error of the mth weak learner is computed mainly to determine:**

- [ ] A) The number of training samples
- [ ] B) The depth of the weak learner
- [x] C) The classifier weight $\alpha_m$
- [ ] D) The feature dimension

**Answer:** C ✅

**Answer Explanation:**

This question tests understanding of the **causal flow** in AdaBoost: computing weighted error is an intermediate step whose explicit purpose is to determine the voting power of each weak learner.

**The Weighted Error:**

After training the mth weak learner $h_m$ on the dataset with current weights $w^{(m)}$, compute the weighted error as the sum of weights on misclassified points:

$$\epsilon_m = \sum_{i=1}^N w_i^{(m)} \mathbb{1}[h_m(x_i) \neq y_i]$$

This is the **only input** to the next formula.

**Computing the Classifier Weight:**

The weighted error $\epsilon_m$ is immediately used to compute how much voting power the mth learner receives:

$$\alpha_m = \frac{1}{2} \log \left( \frac{1 - \epsilon_m}{\epsilon_m} \right)$$

**Causal Chain:**

```
Train h_m → Compute ε_m → Compute α_m → Update weights w^(m+1) → Train h_{m+1}
```

The error $\epsilon_m$ is a bottleneck: it's the bridge between model performance and ensemble voting weight.

**Interpretation of $\alpha_m$:**

- **If $\epsilon_m = 0.1$ (90% accuracy):** $\alpha_m = \frac{1}{2}\log\frac{0.9}{0.1} \approx 1.099$ (high voting power).
- **If $\epsilon_m = 0.5$ (50% accuracy, random guessing):** $\alpha_m = 0$ (no contribution).
- **If $\epsilon_m = 0.3$ (70% accuracy):** $\alpha_m = \frac{1}{2}\log\frac{0.7}{0.3} \approx 0.424$ (moderate power).

The relationship is **monotonic and monotonically decreasing**: better error → higher $\alpha_m$.

**Analysis of Incorrect Options:**

- **A) The number of training samples:** The dataset size $N$ is fixed from the beginning. Computing $\epsilon_m$ doesn't change $N$.

- **B) The depth of the weak learner:** Tree depth is a **hyperparameter** chosen before training. The weighted error provides no information about depth; depth doesn't affect the error-to-alpha computation.

- **D) The feature dimension:** The input feature dimension $d$ is a property of the data, independent of algorithm execution.

**Key Insight:** In AdaBoost, the error $\epsilon_m$ serves a single, critical purpose: determining the voting strength $\alpha_m$. This is why computing it is mandatory.

**Related Topics:** [AdaBoost: Training a Weak Learner, Computing Classifier Weight](notes.md#training-a-weak-learner)

---

## Question Set 2: Autoencoders

### Q3 (1 point)

**The reconstruction loss shown for the autoencoder is:**

- [ ] A) $\sum_{i=1}^{n} y_i \log \hat{y}_i$
- [ ] B) $\sum_{i=1}^{n} \exp(-y_i h(x_i))$
- [x] C) $\frac{1}{n}\sum_{i=1}^{n}\|x_i - \hat{x}_i\|_2^2$
- [ ] D) $\max_i \|x_i - \hat{x}_i\|_2$

**Answer:** C ✅

**Answer Explanation:**

This question tests the core objective function for autoencoders: **Mean Squared Error (MSE)** between input and reconstruction.

**The Autoencoder Goal:**

Compress information into a bottleneck representation, then reconstruct the original input as accurately as possible. The loss measures reconstruction fidelity.

**The Reconstruction Loss Formula:**

$$\mathcal{L}(\theta, \phi) = \frac{1}{n} \sum_{i=1}^{n} \|x_i - \hat{x}_i\|_2^2$$

where:
- $x_i$: Original input.
- $\hat{x}_i = D_\theta(E_\phi(x_i))$: Reconstructed output.
- $\|x_i - \hat{x}_i\|_2^2$: Squared Euclidean distance (L2 norm squared).
- $\frac{1}{n}$: Average over all $n$ samples.

**Geometric Interpretation:**

For each dimension of each sample, compute $(x_i^{(j)} - \hat{x}_i^{(j)})^2$, sum across all dimensions, then sum across all samples, and divide by $n$. This is standard MSE.

**Why MSE?**

1. **Smooth Gradient:** Differentiable everywhere, enabling backpropagation.
2. **Interpretable:** Direct measure of reconstruction error in the original scale.
3. **Optimal for Gaussian Noise:** Under Gaussian noise assumption, MSE is the maximum likelihood estimator.

**Alternative: Reconstruction as Compression:**

The autoencoder works by:
1. **Encoder:** $z = E_\phi(x)$ compresses $x$ to lower-dimensional $z$.
2. **Bottleneck:** $z$ has fewer dimensions than $x$ (e.g., $z \in \mathbb{R}^{10}$ from $x \in \mathbb{R}^{100}$).
3. **Decoder:** $\hat{x} = D_\theta(z)$ expands back to original dimensions.
4. **Loss:** Minimize reconstruction error to force the bottleneck to capture maximum information.

**Analysis of Incorrect Options:**

- **A) $\sum y_i \log \hat{y}_i$:** This is **cross-entropy loss**, used in classification with probabilistic outputs. Autoencoders don't have class labels $y_i$; they reconstruct raw inputs.

- **B) $\sum \exp(-y_i h(x_i))$:** This is **exponential loss**, used by AdaBoost for binary classification. It penalizes misclassifications exponentially, not suited for continuous reconstruction.

- **D) $\max_i \|x_i - \hat{x}_i\|_2$:** This is the **infinity norm** (maximum error across samples). Using max instead of mean ignores most samples, focusing only on the single worst reconstruction. Autoencoders use the mean.

**Key Insight:** Autoencoders use MSE for reconstruction because it's smooth, interpretable, and standard for continuous variables. The loss directly reflects fidelity of the compression.

**Related Topics:** [Autoencoders, Training Objective](notes.md#7-autoencoders)

---

### Q4 (1 point)

**In an autoencoder, the model structure is best described as:**

- [ ] A) Classifier followed by softmax
- [x] B) Encoder $E_\phi(x)$ to latent code $z$, then decoder $D_\theta(z)$ to reconstruction $\hat{x}$
- [ ] C) A single linear regression model
- [ ] D) A decision tree with two leaves

**Answer:** B ✅

**Answer Explanation:**

This question tests the **architectural understanding** of autoencoders: their distinctive three-part bottleneck structure.

**The Autoencoder Architecture:**

An autoencoder consists of three interconnected components:

```
Input x
    ↓
Encoder E_φ (parameterized by φ)
    ↓
Latent Code z (lower-dimensional representation)
    ↓
Decoder D_θ (parameterized by θ)
    ↓
Reconstruction x̂
```

**Mathematical Formulation:**

$$z = E_\phi(x) \in \mathbb{R}^{d_z}, \quad d_z < d_x$$
$$\hat{x} = D_\theta(z) \in \mathbb{R}^{d_x}$$

where $d_x$ is the input dimension and $d_z$ is the latent dimension.

**End-to-End Function:**

$$\hat{x} = D_\theta(E_\phi(x)) = f_{\theta,\phi}(x)$$

**Training:**

Minimize reconstruction loss:

$$\min_{\theta, \phi} \frac{1}{n} \sum_{i=1}^n \|x_i - D_\theta(E_\phi(x_i))\|_2^2$$

Both $\theta$ and $\phi$ are updated simultaneously via backpropagation.

**The Bottleneck Design:**

The **bottleneck** (latent code $z$) forces compression. Because $d_z \ll d_x$, the encoder must extract the most important features. The decoder reconstructs from this compressed representation. This is unsupervised feature learning.

**Comparison to Related Architectures:**

| Architecture | Purpose | Structure |
|--------------|---------|-----------|
| **Autoencoder** | Unsupervised compression + reconstruction | Encoder → Bottleneck → Decoder |
| **Supervised Classifier** | Predict labels from input | Feature layers → Softmax output |
| **Linear Regression** | Predict continuous target | Input → Single linear layer |

**Analysis of Incorrect Options:**

- **A) Classifier followed by softmax:** This is supervised classification (with labels $y$). Autoencoders are unsupervised; they have no labels and output reconstructions, not class probabilities.

- **C) A single linear regression model:** Linear regression predicts a scalar or vector output. Autoencoders have two networks (encoder and decoder), potentially non-linear, and explicitly learn a latent representation. A **linear autoencoder** (single hidden layer, no non-linearity) is equivalent to PCA, but the general autoencoder is non-linear.

- **D) A decision tree with two leaves:** Trees are not typically used for autoencoders. The architecture is neural network-based, trained with backpropagation. Trees are discrete partitioners, not suited for learning continuous latent representations.

**Key Insight:** The autoencoder's defining feature is the bottleneck: a compressed latent representation that forces information extraction. Encoder and decoder are symmetric (or asymmetric in variants).

**Related Topics:** [Autoencoders, Architecture](notes.md#7-autoencoders)

---

## Question Set 3: Principal Component Analysis (PCA)

### Q5 (1 point)

**The first principal component in PCA is given by:**

- [ ] A) The eigenvector corresponding to the smallest eigenvalue of the covariance matrix
- [x] B) The eigenvector corresponding to the largest eigenvalue of the covariance matrix
- [ ] C) The mean vector of the data
- [ ] D) The vector of all ones

**Answer:** B ✅

**Answer Explanation:**

This question tests the **fundamental mathematical result** underlying PCA: the direction of maximum variance is the eigenvector of the largest eigenvalue.

**PCA Optimization Problem:**

PCA seeks the **linear projection** that maximizes variance:

$$\max_w w^T \Sigma w \quad \text{subject to} \quad w^T w = 1$$

where $w$ is a unit vector (norm = 1) and $\Sigma$ is the covariance matrix.

**The Lagrangian:**

$$\mathcal{L}(w, \lambda) = w^T \Sigma w - \lambda(w^T w - 1)$$

Taking the gradient and setting it to zero:

$$\nabla_w \mathcal{L} = 2\Sigma w - 2\lambda w = 0$$

This simplifies to:

$$\Sigma w = \lambda w$$

**Eigenvalue Equation:**

This is the **eigenvalue equation**. The solution $w$ is an eigenvector of $\Sigma$, and $\lambda$ is the corresponding eigenvalue.

**Variance in the Projected Direction:**

For an eigenvector $w$ with eigenvalue $\lambda$:

$$w^T \Sigma w = w^T (\lambda w) = \lambda (w^T w) = \lambda$$

So the **variance in the projected direction is equal to the eigenvalue**.

**Maximizing Variance:**

To maximize variance, choose the eigenvector corresponding to the **largest eigenvalue** $\lambda_1$.

**First Principal Component (1PC):**

$$w_1 = \arg\max_w w^T \Sigma w \quad \text{s.t.} \quad w^T w = 1$$

Solution: The eigenvector $v_1$ corresponding to the largest eigenvalue $\lambda_1$.

**Second Principal Component (2PC):**

Choose the eigenvector $v_2$ corresponding to the second-largest eigenvalue $\lambda_2$, orthogonal to $v_1$.

**Variance Ordering:**

$$\text{Var}(\text{1PC}) = \lambda_1 > \text{Var}(\text{2PC}) = \lambda_2 > \cdots > \text{Var}(\text{dPC}) = \lambda_d$$

**Analysis of Incorrect Options:**

- **A) Smallest eigenvalue:** This would minimize variance, not maximize. Choosing the smallest eigenvector projects data onto the direction where it varies least (noise or nuisance factors).

- **C) The mean vector:** The mean $\bar{x}$ is used to **center** the data before PCA, but it's not itself a principal component. PCA is applied to centered data; the mean is a preprocessing step.

- **D) The vector of all ones:** A constant vector has no relationship to data covariance. It's not an eigenvector unless the covariance matrix has a very special structure (which is almost never true).

**Key Insight:** The "principal" in PCA refers to the **principal (most important)** directions, ranked by variance. The first principal component captures the most variance; subsequent components capture decreasing variance. This is why eigenvectors are the answer—they directly correspond to variance-ordered directions.

**Related Topics:** [PCA: The Optimization Problem, The Solution](notes.md#6-principal-component-analysis-pca)

---

### Q6 (1 point)

**In PCA, if the projected representation is z = Wx, then the optimization objective is to:**

- [ ] A) Minimize projected variance
- [x] B) Maximize projected variance subject to a norm constraint
- [ ] C) Maximize reconstruction error
- [ ] D) Minimize the number of samples

**Answer:** B ✅

**Answer Explanation:**

This question tests the **optimization objective** of PCA: balancing maximum variance preservation with computational constraints.

**The Problem Statement:**

Given high-dimensional data $x \in \mathbb{R}^D$, project to lower dimensions:

$$z = W^T x, \quad z \in \mathbb{R}^d, \quad d < D$$

We want to choose $W \in \mathbb{R}^{D \times d}$ to maximize information preservation.

**Information and Variance:**

In unsupervised learning, **variance is information**. High variance in a direction means data is spread out, indicating that direction contains useful signal. Low variance means data is tightly bunched (all samples similar), containing little information.

**The Optimization Objective:**

Maximize the variance of the **projected data**:

$$\max_W \mathbb{E}[\|z\|^2] = \max_W \mathbb{E}[\|W^T x\|^2] = \max_W W^T \Sigma W$$

where $\Sigma = \mathbb{E}[xx^T]$ is the covariance matrix.

**The Norm Constraint:**

If we didn't constrain $W$, the optimization would have a trivial solution: set all entries of $W$ to infinity (or equally large values), making the variance arbitrarily large. This would be meaningless.

To prevent this, impose a **norm constraint**:

$$W^T W = I_d$$

or equivalently, columns of $W$ are orthonormal (unit length, mutually perpendicular).

**The Constrained Problem:**

$$\max_W W^T \Sigma W \quad \text{subject to} \quad W^T W = I_d$$

**Interpretation:**

- **Objective:** Spread the data out as much as possible in the projected space (maximize variance).
- **Constraint:** Ensure $W$ is well-defined (unit norm, non-redundant directions).

**The Solution:**

The optimal $W$ has columns equal to the top $d$ eigenvectors of $\Sigma$ (as derived in Q5).

**Analysis of Incorrect Options:**

- **A) Minimize projected variance:** This would compress all data into a tiny region, destroying information. This is the opposite of PCA's goal.

- **C) Maximize reconstruction error:** PCA is about **information preservation**, which is equivalent to **minimizing reconstruction error** (if you reconstruct from the projection, smaller error means less information loss). Maximizing error is backward.

- **D) Minimize the number of samples:** The sample size $n$ is fixed. PCA doesn't change it. This option is nonsensical.

**Reconstruction Interpretation:**

PCA is equivalent to minimizing reconstruction error when projecting and reconstructing:

$$\min_W \frac{1}{n} \sum_{i=1}^n \|x_i - W W^T x_i\|_2^2$$

This is equivalent to the variance maximization objective (by the theory of orthogonal projections). Both interpretations are valid.

**Key Insight:** PCA is fundamentally about **variance preservation**. The norm constraint prevents trivial solutions. Together, they yield a well-posed optimization problem whose solution is given by the eigendecomposition of the covariance matrix.

**Related Topics:** [PCA: The Optimization Problem](notes.md#the-optimization-problem)

---

## Question Set 4: Clustering Algorithms

### Q7 (1 point)

**In k-means clustering, each data point $x_i$ is assigned to cluster m based on:**

- [ ] A) Maximum inner product with $\mu_m$
- [x] B) Minimum squared Euclidean distance to $\mu_m$
- [ ] C) Maximum determinant of covariance
- [ ] D) Minimum class entropy

**Answer:** B ✅

**Answer Explanation:**

This question tests the **assignment rule** of k-means: the core operation that partitions data into clusters based on spatial proximity.

**The Assignment Step:**

In each iteration of k-means, assign every point to its nearest cluster centroid:

$$c_i = \arg\min_{m=1,\ldots,K} \|x_i - \mu_m\|_2^2$$

where:
- $c_i \in \{1, 2, \ldots, K\}$: Cluster assignment for point $i$.
- $\mu_m$: Centroid (mean) of cluster $m$.
- $\|\cdot\|_2^2$: **Squared Euclidean distance** (not just distance, but squared).

**Why Squared Distance?**

Squaring has two benefits:
1. **Monotonicity:** Minimizing $d^2$ is equivalent to minimizing $d$ (since $x^2$ is monotonic increasing for $x \geq 0$).
2. **Computational:** Avoids the sqrt operation; squaring is faster.

**Geometric Intuition:**

Imagine K colored points (centroids) in a 2D space. Each data point is colored based on which centroid it's closest to. This creates **Voronoi cells**—regions where all points are closer to one centroid than any other.

**The Objective Function Connection:**

K-means minimizes the objective:

$$J = \sum_{i=1}^N \|x_i - \mu_{c_i}\|_2^2$$

Each assignment step greedily minimizes $J$ by choosing the nearest centroid.

**Analysis of Incorrect Options:**

- **A) Maximum inner product with $\mu_m$:** Inner product $x_i^T \mu_m$ measures directional similarity (like cosine distance), not Euclidean proximity. K-means uses spatial distance, not angular similarity. (You might use inner products in cosine-similarity-based clustering, but not k-means.)

- **C) Maximum determinant of covariance:** The determinant of a covariance matrix is a scalar describing the "spread" of cluster points. K-means doesn't compute or use cluster covariances during assignment; it only uses centroid locations.

- **D) Minimum class entropy:** Entropy measures purity of class labels (a classification concept). K-means is unsupervised; there are no class labels, so entropy is undefined. This is a conceptual mismatch.

**When Assignment is "Hard":**

K-means produces **hard assignments**: each point belongs to exactly one cluster. This contrasts with **soft assignments** (probabilistic) in Gaussian Mixture Models.

**Related Topics:** [K-Means Clustering, Algorithm](notes.md#4-k-means-clustering)

---

### Q8 (1 point)

**In the limit where each Gaussian component in a mixture has covariance $\sigma^2 I$ with $\sigma \to 0$, the soft assignments tend toward:**

- [ ] A) Uniform probabilities
- [x] B) Dirac-delta-like hard assignments
- [ ] C) Random assignments
- [ ] D) Zero vectors

**Answer:** B ✅

**Answer Explanation:**

This question reveals the **mathematical limit relationship** between Gaussian Mixture Models (soft clustering) and k-means (hard clustering).

**GMM Soft Assignment:**

In a Gaussian Mixture Model, the posterior probability that point $x$ belongs to cluster $k$ is:

$$P(z = k | x) = \frac{\pi_k \mathcal{N}(x | \mu_k, \Sigma_k)}{\sum_{j=1}^K \pi_j \mathcal{N}(x | \mu_j, \Sigma_j)}$$

where $\mathcal{N}(x | \mu, \Sigma)$ is a multivariate Gaussian with mean $\mu$ and covariance $\Sigma$.

**A Point Between Two Clusters:**

Suppose a point $x$ is equidistant from two cluster centers. If covariances are large (high variance), the Gaussian probabilities are spread out, and the point might have 50% probability to each cluster.

**As Variance Shrinks:**

Now assume each Gaussian has covariance $\Sigma_k = \sigma^2 I$ (spherical, isotropic), and $\sigma$ is small.

The Gaussian becomes:

$$\mathcal{N}(x | \mu_k, \sigma^2 I) = \frac{1}{(2\pi\sigma^2)^{d/2}} \exp\left( -\frac{\|x - \mu_k\|_2^2}{2\sigma^2} \right)$$

**Taking the Limit:**

As $\sigma \to 0$, the exponential term dominates:

- **If $\|x - \mu_j\|_2^2 < \|x - \mu_k\|_2^2$:** The difference $\frac{\|x - \mu_j\|_2^2 - \|x - \mu_k\|_2^2}{2\sigma^2} \to -\infty$, and $\mathcal{N}(x | \mu_j, \sigma^2 I) \to \infty$ (relative to other terms).

- **If $\|x - \mu_k\|_2^2 > \|x - \mu_j\|_2^2$:** Then $\mathcal{N}(x | \mu_k, \sigma^2 I) \to 0$ (relative to the closest).

**The Result:**

$$\lim_{\sigma \to 0} P(z = k | x) \to \begin{cases} 1 & \text{if } k = \arg\min_j \|x - \mu_j\|_2^2 \\ 0 & \text{otherwise} \end{cases}$$

This is a **Dirac-delta distribution**: probability concentrates entirely on the nearest centroid.

**Connection to k-Means:**

This limiting behavior recovers the k-means assignment rule exactly:

$$c_i = \arg\min_{m=1,\ldots,K} \|x_i - \mu_m\|_2^2$$

**Interpretation:**

- **GMM at high variance:** Soft clustering. Points "belong" to multiple clusters with non-zero probability.
- **GMM at zero variance (limit):** Hard clustering. Points belong entirely (probability 1) to the nearest cluster.
- **k-Means:** The degenerate case; equivalent to GMM with $\sigma \to 0$.

**Analysis of Incorrect Options:**

- **A) Uniform probabilities:** This occurs when variance is **infinitely large** (data points far from all centroids). As $\sigma \to 0$, the opposite happens.

- **C) Random assignments:** The limit is deterministic (based on distance), not random. Randomness enters via initialization, not convergence.

- **D) Zero vectors:** Probabilities must sum to 1 (they form a distribution). Assigning zero to all clusters is impossible.

**Key Insight:** K-Means emerges as a limiting case of GMM. Understanding this connection reveals that soft (probabilistic) and hard (deterministic) clustering are not fundamentally different—they represent different variance regimes of the same generative model.

**Related Topics:** [Gaussian Mixture Models, GMM to K-Means Connection](notes.md#gmm-to-k-means-connection)

---

### Q9 (1 point)

**In a GMM-based clustering setup, the posterior $P_\theta(z|x) = P_\theta(x|z)P_\theta(z) / P_\theta(x)$ is interpreted as:**

- [ ] A) A hard cluster label only
- [x] B) A distribution over cluster assignments given x
- [ ] C) The covariance matrix
- [ ] D) The reconstruction error

**Answer:** B ✅

**Answer Explanation:**

This question tests the **interpretation of GMM posteriors**: what the mathematical expression represents in practical terms.

**Bayes' Rule in GMM:**

By Bayes' theorem:

$$P_\theta(z | x) = \frac{P_\theta(x | z) P_\theta(z)}{P_\theta(x)}$$

where:
- $P_\theta(z | x)$: **Posterior** (probability of cluster $z$ given observation $x$).
- $P_\theta(x | z)$: **Likelihood** (probability of $x$ given cluster $z$).
- $P_\theta(z)$: **Prior** (probability of cluster $z$ overall, i.e., mixing weight $\pi_z$).
- $P_\theta(x)$: **Evidence** (marginal probability of $x$ across all clusters).

**The Posterior as a Distribution:**

The posterior $P_\theta(z | x)$ is a **probability distribution over the $K$ possible cluster assignments**, given a single observation $x$.

**Example:**

Suppose $K = 3$ clusters. For a particular $x$, the posterior might be:

$$P_\theta(z = 1 | x) = 0.70$$
$$P_\theta(z = 2 | x) = 0.20$$
$$P_\theta(z = 3 | x) = 0.10$$

**Interpretation:**

The point $x$ is most likely from cluster 1 (70% chance), less likely from cluster 2 (20%), and unlikely from cluster 3 (10%). This is **soft clustering**—probabilistic membership.

**Contrast to Hard Clustering (k-Means):**

In k-means, each point is assigned to **exactly one** cluster:

$$c_i \in \{1, 2, \ldots, K\}$$

This is a **discrete label**, not a distribution.

In GMM, the posterior is a **full probability distribution**:

$$\sum_{z=1}^K P_\theta(z | x) = 1, \quad P_\theta(z | x) \geq 0 \quad \forall z$$

**Why Use Soft Clustering?**

1. **Uncertainty:** Some points genuinely belong to multiple clusters (e.g., customers with interests spanning multiple categories).
2. **Probabilistic:** Provides confidence measures and uncertainty quantification.
3. **Generative Model:** GMM learns a full generative model $P_\theta(x)$, not just assignments.

**Connection to Expectation-Maximization (EM):**

The E-step of the EM algorithm computes the posterior $P_\theta(z | x)$ for all points. These soft assignments guide parameter updates in the M-step.

**Analysis of Incorrect Options:**

- **A) A hard cluster label only:** Hard labels are single integers. The posterior is a probability distribution (a vector of $K$ values summing to 1), not a label.

- **C) The covariance matrix:** The posterior is a vector (or distribution) of shape $(K,)$. The covariance matrix $\Sigma_k$ is a parameter of the model, of shape $(d, d)$, with different dimensionality and meaning.

- **D) The reconstruction error:** Reconstruction error is computed in autoencoders (difference between input and output). GMM posteriors are probabilistic cluster assignments, not reconstruction errors.

**Key Insight:** GMM posteriors represent **soft** (probabilistic) cluster membership. Each point can "belong" to multiple clusters with non-zero probability. This is more nuanced than k-means' hard assignments and captures uncertainty in the data.

**Related Topics:** [Gaussian Mixture Models, Probabilistic Clustering](notes.md#5-gaussian-mixture-models-gmm)

---

## Question Set 5: Model Evaluation

### Q10 (1 point)

**The main purpose of a validation set in cross-validation is to:**

- [ ] A) Train the final model parameters
- [x] B) Choose hyperparameters
- [ ] C) Replace the test set
- [ ] D) Increase the feature dimension

**Answer:** B ✅

**Answer Explanation:**

This question tests the **distinct roles** of training, validation, and test sets in building robust machine learning models.

**Three-Set Partition:**

A rigorous ML workflow divides data into three parts:

| Set | Purpose | Size | Used When |
|-----|---------|------|-----------|
| **Training** | Update model parameters (weights, splits, etc.) | 60–70% | Every training iteration |
| **Validation** | Tune hyperparameters, choose best model | 10–15% | After each epoch/iteration (during training) |
| **Test** | Final evaluation, report metrics | 15–20% | Only at the very end (after all decisions are made) |

**The Validation Set's Role:**

During training, many **hyperparameter choices** must be made:

- **Learning rate** (how fast to update parameters).
- **Regularization strength** (L1/L2 penalty weight).
- **Number of hidden units** in neural networks.
- **Tree depth** in decision trees.
- **Number of clusters** in k-means.
- **Kernel type** in SVMs.

The **validation set is used to select among these options**:

1. Train the model with hyperparameter choice A on the training set.
2. Evaluate on the validation set; record performance.
3. Train with hyperparameter choice B on the training set.
4. Evaluate on the validation set; record performance.
5. Choose the hyperparameters with best validation performance.

**Why Separate from Training and Test?**

- **Training Set Only:** If you tune hyperparameters on the training set itself, you're fitting not just the model but also the hyperparameter settings to the training data. This leads to **overfitting**. The hyperparameters will be optimized for the training set, not generalizable to new data.

- **Test Set Only:** The test set must remain completely untouched during development. If you tune hyperparameters on the test set, the test results become **biased** (overfitted to your specific test data). The final metrics won't reflect true generalization performance.

- **Validation Set:** A held-out set used specifically for tuning, yet separate from the final test set. This allows unbiased hyperparameter selection.

**K-Fold Cross-Validation:**

When data is limited, use k-fold cross-validation:

1. Divide data into $K$ folds.
2. For each fold:
   - Validation set = This fold.
   - Training set = All other folds.
   - Train the model and record validation error.
3. Average validation errors across folds.
4. Choose hyperparameters that minimize average validation error.
5. **Retrain on the full dataset** (or all but the test set) using the selected hyperparameters.
6. Evaluate on a held-out test set.

**Pseudocode:**

```
best_hyperparams = None
best_score = -∞

for each hyperparameter choice h:
    cv_scores = []
    for each fold k in K-folds:
        train_model(training_data, h)
        score_k = evaluate(validation_data)
        cv_scores.append(score_k)
    avg_score = mean(cv_scores)
    if avg_score > best_score:
        best_score = avg_score
        best_hyperparams = h

# Now retrain with best hyperparameters on full dataset
final_model = train_model(full_dataset, best_hyperparams)
test_error = evaluate(final_model, test_set)
```

**Analysis of Incorrect Options:**

- **A) Train the final model parameters:** This is the **training set's** purpose. The validation set does not update model parameters; it only evaluates them (with different hyperparameter settings).

- **C) Replace the test set:** The test set is sacred—it must remain separate and untouched until the very end. Validation is a substitute for the test set during development, but the actual test set must be reserved.

- **D) Increase the feature dimension:** Feature dimension is a property of the data; partitioning into train/validation/test doesn't change it.

**Common Mistake:**

Beginning practitioners sometimes report test error as their main metric, but tune hyperparameters on the same test set. This is **circular**—the test error becomes inflated and optimistic. Always use a separate validation set or cross-validation.

**Key Insight:** Validation sets serve a specific purpose: **hyperparameter tuning**. By keeping it separate from both training (where parameters are fit) and testing (where final metrics are reported), you ensure honest, unbiased model selection.

**Related Topics:** [Cross-Validation](notes.md#2-cross-validation)

---

## Answer Key Summary

| Q | Answer | Concept | Difficulty | NPTEL Status |
|---|--------|---------|------------|--------------|
| Q1 | B ($1/N$) | AdaBoost weight initialization | Medium | ✗ Marked incorrect; answer is 1/N (standard), but NPTEL expects 1/2N (variant) |
| Q2 | C | Weighted error determines classifier weight $\alpha_m$ | Easy | ✅ Correct |
| Q3 | C | Autoencoder MSE reconstruction loss | Easy | ✅ Correct |
| Q4 | B | Encoder → Latent → Decoder structure | Easy | ✅ Correct |
| Q5 | B | First PC is largest eigenvector | Medium | ✅ Correct |
| Q6 | B | Maximize projected variance (with norm constraint) | Medium | ✅ Correct |
| Q7 | B | K-Means: minimum squared Euclidean distance | Easy | ✅ Correct |
| Q8 | B | GMM limit (σ→0): Dirac-delta hard assignments | Hard | ✅ Correct |
| Q9 | B | GMM posterior: soft cluster distribution | Medium | ✅ Correct |
| Q10 | B | Validation set for hyperparameter tuning | Easy | ✅ Correct |

**Overall Score:** 9/10 ✅

---

## Concept Map: Week 12 Capstone

**Bridge Between Paradigms:**

### **Part 1: Supervised Learning Conclusion (Q1–Q2)**

**AdaBoost:**
- Weights start uniform ($1/N$ or $1/2N$, variant-dependent).
- Iteratively upweight misclassified points.
- Weighted error determines voting power $\alpha_m$.
- Sequential ensemble: each tree corrects previous mistakes.

### **Part 2: Unsupervised Dimensionality Reduction (Q3–Q6)**

**Autoencoders (Q3–Q4):**
- Encoder compresses; Decoder reconstructs.
- Minimize MSE reconstruction loss.
- Nonlinear feature extraction via bottleneck.

**PCA (Q5–Q6):**
- Linear projection maximizing variance.
- First PC: eigenvector of largest eigenvalue.
- Objective: maximize projected variance (constrained).

### **Part 3: Unsupervised Clustering (Q7–Q9)**

**K-Means (Q7):**
- Hard clustering via nearest centroid.
- Assignment: minimize squared Euclidean distance.
- Deterministic, fast, simple.

**GMM (Q8–Q9):**
- Soft clustering: probability distribution over clusters.
- Posterior: $P(z|x)$ gives uncertainty.
- K-Means emerges in limit as variance → 0.

### **Part 4: Model Development Best Practices (Q10)**

**Cross-Validation:**
- Train/Validation/Test separation.
- Validation for hyperparameter tuning.
- Test for unbiased final evaluation.

---

## Summary: Week 12 Unites All Concepts

**The Big Picture:**

1. **AdaBoost** concludes supervised ensemble methods (boosting, weighted resampling).
2. **PCA & Autoencoders** enable dimensionality reduction and feature extraction.
3. **K-Means & GMM** discover discrete cluster structure.
4. **Cross-Validation** ensures honest model evaluation.

**Why These Methods Matter:**

- **AdaBoost:** Foundational boosting algorithm; shows how reweighting improves ensemble.
- **PCA:** Fast baseline for dimensionality reduction; interpretable via eigenvalues.
- **Autoencoders:** Deep learning approach to compression; can capture nonlinear manifolds.
- **K-Means:** Industry standard for clustering on tabular data (billions of data points).
- **GMM:** Probabilistic variant; foundation for expectation-maximization and probabilistic modeling.

**The Q1 Discrepancy:**

Your answer (1/N) is **standard** across ML literature. The NPTEL variant (1/2N) is used in balanced imbalanced-classification setups. Both are correct; context matters. In future courses/work, verify algorithmic details against multiple sources.

**Production Relevance:**

- **Scikit-learn, PyTorch, TensorFlow:** All implement these algorithms.
- **Kaggle Competitions:** PCA and k-means are quick baselines; gradient boosting (XGBoost) dominates.
- **Real-World Systems:** K-Means powers customer segmentation; GMM for probabilistic modeling; PCA for feature preprocessing.

This concludes the Mathematical Foundations of Machine Learning course. You now have mastery of:
- **Supervised Learning:** Regression, classification, trees, neural networks, ensembles.
- **Unsupervised Learning:** Clustering, dimensionality reduction.
- **Theoretical Foundations:** Convexity, optimization, probability, information theory.
- **Practical Considerations:** Cross-validation, hyperparameter tuning, regularization.

Use these skills to solve real-world problems!


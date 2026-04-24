# Week 10: Assignment 10 — Mathematical Foundations of Machine Learning (CS02)

| Metric | Value |
|--------|-------|
| Due | 2026-04-01, 23:59 IST |
| Submitted | — |
| Score | — |

---

## Question Set 1: Attention Mechanism Dimensions

### Q1 (1 point)

**Suppose the input token matrix is $X \in \mathbb{R}^{5 \times 8}$ and the query projection matrix is $W_Q \in \mathbb{R}^{8 \times 4}$. What is the dimension of the query matrix $Q = XW_Q$?**

- [ ] A) 5×8
- [ ] B) 8×4
- [ ] C) 4×5
- [ ] D) 5×4

**Answer:** D

**Answer Explanation:**

This question tests the **fundamental matrix multiplication rule**, which governs all linear projections in neural networks, including transformers.

**The Matrix Multiplication Dimension Rule:**

When multiplying matrices $A \in \mathbb{R}^{m \times n}$ and $B \in \mathbb{R}^{n \times p}$, the result is $C = AB \in \mathbb{R}^{m \times p}$.

**Key Principle:** The inner dimensions must match ($n$ in both matrices), and the result has the outer dimensions.

**Application to This Problem:**

$$Q = X_{(5 \times 8)} \times W_Q_{(8 \times 4)} = Q_{(5 \times 4)}$$

**Breaking It Down:**

- $X \in \mathbb{R}^{5 \times 8}$: 5 tokens (sequences), each 8-dimensional.
- $W_Q \in \mathbb{R}^{8 \times 4}$: Projection matrix from 8 dimensions to 4 dimensions.
- Inner dimensions: $8 = 8$ ✓ (match)
- Outer dimensions: $5 \times 4$ ✓
- Result: $Q \in \mathbb{R}^{5 \times 4}$ (5 tokens, now 4-dimensional queries)

**Intuition:**

Each of the 5 tokens is transformed from its original 8-dimensional embedding into a 4-dimensional query vector using the weight matrix $W_Q$. The transformation is applied identically to all 5 tokens.

**Analysis of Incorrect Options:**

- **A) 5×8:** This is just the dimension of $X$. It would result if you didn't apply the projection at all.

- **B) 8×4:** This is the dimension of the weight matrix $W_Q$. Confusing the intermediate transformation matrix with the output is a common mistake.

- **C) 4×5:** This is the transpose of the correct answer. It would be correct if we computed $W_Q^T X^T$ instead of $X W_Q$.

**Key Insight:** Matrix multiplication $(m \times n) \times (n \times p) = (m \times p)$. The first dimension of the result comes from the first matrix, the second from the second matrix. This is how we control layer sizes in neural networks.

**Related Topics:** [The Three Matrices: Query, Key, Value](notes.md#the-three-matrices-query-key-value)

---

### Q2 (1 point)

**Let $Q \in \mathbb{R}^{6 \times 4}$ and $K \in \mathbb{R}^{6 \times 4}$. What is the dimension of the score matrix $QK^\top$ used in attention?**

- [ ] A) 4×4
- [ ] B) 6×4
- [ ] C) 4×6
- [ ] D) 6×6

**Answer:** D

**Answer Explanation:**

This question tests understanding of the **attention score matrix**, which computes pairwise similarities between all tokens in a sequence.

**The Transpose Operation:**

If $K \in \mathbb{R}^{6 \times 4}$, then $K^T \in \mathbb{R}^{4 \times 6}$ (rows and columns swap).

**Matrix Multiplication:**

$$\text{Score Matrix} = Q_{(6 \times 4)} \times K^T_{(4 \times 6)} = \text{Score}_{(6 \times 6)}$$

**What This Represents:**

- $Q \in \mathbb{R}^{6 \times 4}$: 6 tokens, 4-dimensional query vectors.
- $K^T \in \mathbb{R}^{4 \times 6}$: Transposed keys (now 6 keys along the columns).
- Score matrix $QK^T \in \mathbb{R}^{6 \times 6}$: A $6 \times 6$ grid where entry $(i,j)$ is the dot product similarity between query $i$ and key $j$.

**Geometric Intuition:**

For a sequence with 6 tokens, we need a $6 \times 6$ matrix where:
- Each row $i$ represents how much token $i$ should attend to all 6 tokens.
- Each column $j$ represents the keys available from token $j$.

**Computing One Entry:**

The $(i, j)$ entry is:
$$\text{Score}_{ij} = q_i \cdot k_j = Q_{i,:} \cdot K_{j,:}$$

The dot product of the $i$-th row of $Q$ (query) with the $j$-th row of $K$ (key).

**Why Transpose?**

We want to compute all possible query-key combinations. Transposing $K$ aligns it so that matrix multiplication naturally computes all these dot products simultaneously.

**Analysis of Incorrect Options:**

- **A) 4×4:** This would be the result of computing $K^T K$ instead of $Q K^T$. The dimension 4 comes from the embedding dimension, not the sequence length.

- **B) 6×4:** This is just the dimension of $Q$ or $K$. It would result if you forgot to transpose $K$.

- **C) 4×6:** This is the dimension of $K^T$. You'd get this if you computed $K^T Q$ instead of $Q K^T$ (wrong order).

**Key Insight:** The score matrix is always square ($T \times T$) for a sequence of length $T$, because every token attends to every other token. The dimension depends on sequence length, not embedding dimension.

**Related Topics:** [Scaled Dot-Product Attention](notes.md#2-scaled-dot-product-attention)

---

### Q3 (1 point)

**A query vector is $q = \begin{bmatrix} 1 \\ 2 \end{bmatrix}$ and two key vectors are $k_1 = \begin{bmatrix} 2 \\ 1 \end{bmatrix}$ and $k_2 = \begin{bmatrix} 1 \\ 3 \end{bmatrix}$. What are the dot-product scores $q^\top k_1$ and $q^\top k_2$?**

- [ ] A) (3, 5)
- [ ] B) (5, 7)
- [ ] C) (2, 6)
- [ ] D) (4, 7)

**Answer:** D

**Answer Explanation:**

This question tests the **dot product**, the core similarity metric in attention. Computing dot products correctly is essential for understanding attention scores.

**The Dot Product Definition:**

For two vectors $a = [a_1, a_2, \ldots, a_n]$ and $b = [b_1, b_2, \ldots, b_n]$:

$$a \cdot b = a^\top b = \sum_{i=1}^{n} a_i b_i$$

**Calculation for $q^\top k_1$:**

$$q^\top k_1 = \begin{bmatrix} 1 & 2 \end{bmatrix} \begin{bmatrix} 2 \\ 1 \end{bmatrix} = (1)(2) + (2)(1) = 2 + 2 = 4$$

**Calculation for $q^\top k_2$:**

$$q^\top k_2 = \begin{bmatrix} 1 & 2 \end{bmatrix} \begin{bmatrix} 1 \\ 3 \end{bmatrix} = (1)(1) + (2)(3) = 1 + 6 = 7$$

**Answer: (4, 7)**

**Geometric Intuition:**

The dot product measures alignment between vectors:
- Large dot product → vectors point in similar directions → high similarity.
- Small dot product → vectors point in dissimilar directions → low similarity.
- Zero dot product → perpendicular vectors → orthogonal.

In this case:
- $q$ and $k_1$ have a score of 4 (moderate similarity).
- $q$ and $k_2$ have a score of 7 (higher similarity).

During softmax, $k_2$ would receive more attention weight than $k_1$ because it has a higher score.

**Analysis of Incorrect Options:**

- **A) (3, 5):** These would be vector additions, not dot products. $q + k_1 = [1+2, 2+1] = [3, 3]$, but then why (3, 5)? This is mixing addition with some other error.

- **B) (5, 7):** The second value is correct, but the first is wrong. This might come from $q + k_1 = [3, 3]$ then summing to 6 (still wrong), or it's just a miscalculation. Let's check: if someone computed $(1 \times 2) + (2 \times 2) = 6$, that's also wrong. Maybe $(1 + 2) \times 2 = 6$? No match.

- **C) (2, 6):** Both values are wrong. This might come from multiplying element-wise without summing: $q \odot k_1 = [2, 2]$ and $q \odot k_2 = [1, 6]$, but why then (2, 6)? Could be taking one element from each.

**Common Mistakes to Avoid:**

1. **Forgetting to sum:** $(1 \times 2) + (2 \times 1)$ must be summed to get 4, not left as separate terms.
2. **Confusing with vector addition:** The dot product is not element-wise addition.
3. **Confusing with matrix multiplication without summing:** Matrix multiplication includes summing the products.

**Key Insight:** The dot product is a single scalar value, not a vector. It measures how much two vectors "agree" in direction and magnitude.

**Related Topics:** [The Scoring Mechanism](notes.md#step-1-compute-similarity-scores)

---

## Question Set 2: Multi-Head Attention and Transformers

### Q4 (1 point)

**A transformer uses $d_{model} = 12$ and $h = 3$ attention heads. Assuming equal split across heads, what is the dimension per head?**

- [ ] A) 2
- [ ] B) 3
- [ ] C) 5
- [ ] D) 4

**Answer:** D

**Answer Explanation:**

This question tests the **multi-head attention dimension allocation**, which balances representation capacity with computational efficiency.

**The Design Principle:**

In multi-head attention, the total model dimension $d_{model}$ is split equally across $h$ attention heads. Each head operates on a subspace of dimension $d_k = d_{model} / h$.

**Calculation:**

$$d_k = \frac{d_{model}}{h} = \frac{12}{3} = 4$$

**Why This Design?**

Total computational cost is roughly proportional to $d_k^2 \times h$ (number of operations per head times number of heads). By keeping $d_k$ small and using multiple heads, we maintain constant total computation while gaining the ability to learn multiple diverse attention patterns simultaneously.

**Example Decomposition:**

- **Head 1:** Operates on dimensions 0–3 of the 12-dimensional model.
- **Head 2:** Operates on dimensions 4–7.
- **Head 3:** Operates on dimensions 8–11.

Each head learns independent query, key, and value projections:

$$Q^{(i)} = X W_Q^{(i)}, \quad K^{(i)} = X W_K^{(i)}, \quad V^{(i)} = X W_V^{(i)}$$

where each projection maps from $d_{model} = 12$ to $d_k = 4$.

**Benefits:**

1. **Diverse Representations:** Each head can learn to focus on different types of relationships (syntax, semantics, long-range dependencies, etc.).
2. **Efficient:** Total computation remains constant compared to single-head attention with dimension $d_{model}$.
3. **Robust:** Redundancy across heads helps with generalization.

**Analysis of Incorrect Options:**

- **A) 2:** This is $12 / 6$, as if there were 6 heads instead of 3.

- **B) 3:** This is $12 / 4$, as if there were 4 heads instead of 3.

- **C) 5:** This is neither $12/3 = 4$ nor any obvious related calculation.

**Key Insight:** Multi-head attention uses the formula $d_k = d_{model} / h$. Always divide, not any other operation. The product $h \times d_k = d_{model}$ must hold exactly.

**Related Topics:** [Multi-Head Attention](notes.md#3-multi-head-attention)

---

### Q5 (1 point)

**If a sequence has length $T = 7$, then for one attention head, how many attention scores are computed in the full score matrix before softmax?**

- [ ] A) 7
- [ ] B) 14
- [ ] C) 28
- [ ] D) 49

**Answer:** D

**Answer Explanation:**

This question tests understanding of the **attention score matrix size**, which is a fundamental parameter in transformer computational complexity.

**The Score Matrix Dimensionality:**

For a sequence of length $T$, the attention score matrix (before softmax) has shape $T \times T$:

$$\text{Score Matrix} = Q K^T \in \mathbb{R}^{T \times T}$$

where $Q, K \in \mathbb{R}^{T \times d_k}$.

**Number of Scores:**

The total number of scalar values in a $T \times T$ matrix is:

$$\text{Number of Scores} = T \times T = T^2$$

**Calculation:**

$$T^2 = 7^2 = 49$$

**What Each Score Represents:**

Each of the 49 scores is a dot product between one query and one key:

$$\text{Score}_{ij} = q_i \cdot k_j$$

**Visualization (Simplified):**

For 7 tokens, the score matrix looks like:

```
        Key1  Key2  Key3  Key4  Key5  Key6  Key7
Query1  [s11  s12  s13  s14  s15  s16  s17]
Query2  [s21  s22  s23  s24  s25  s26  s27]
Query3  [s31  s32  s33  s34  s35  s36  s37]
Query4  [s41  s42  s43  s44  s45  s46  s47]
Query5  [s51  s52  s53  s54  s55  s56  s57]
Query6  [s61  s62  s63  s64  s65  s66  s67]
Query7  [s71  s72  s73  s74  s75  s76  s77]
```

**Total: $7 \times 7 = 49$ scores**

**Computational Complexity:**

The $O(T^2)$ complexity of attention is why transformers can become expensive for very long sequences. For a sequence of length 10,000, we'd compute $10,000^2 = 100$ million attention scores!

**Analysis of Incorrect Options:**

- **A) 7:** This is just the sequence length $T$. It would be the number of queries (rows) or keys (columns), but not the total number of scores.

- **B) 14:** This is $2T = 14$. Possibly thinking of $T$ queries and $T$ keys (but you still need the cross-product).

- **C) 28:** This is $4T = 28$. Another attempt at a linear relationship, but attention requires all-pairs comparison.

**Key Insight:** Attention score matrices are always square ($T \times T$) for a sequence of length $T$. The scaling is quadratic in sequence length, which is a key limitation of transformers for very long sequences.

**Related Topics:** [The Scoring Mechanism, Multi-Head Attention](notes.md#step-1-compute-similarity-scores)

---

## Question Set 3: Transfer Learning and Multi-Task Learning

### Q6 (1 point)

**In a transfer learning setting, a pretrained network produces an embedding of dimension 128. A new classification head maps this to 10 output classes using a fully connected layer. Ignoring bias, how many weights are in this new layer?**

- [ ] A) 128
- [ ] B) 138
- [ ] C) 1280
- [ ] D) 2560

**Answer:** C

**Answer Explanation:**

This question tests **parameter counting in transfer learning scenarios**, where a small task-specific head is added to a pretrained model.

**The Transfer Learning Setup:**

1. **Pretrained Feature Extractor:** Produces 128-dimensional embeddings.
2. **New Classification Head:** Maps 128-dimensional embeddings → 10 class probabilities.

**Parameter Calculation:**

A fully connected layer mapping $n_{in}$ inputs to $n_{out}$ outputs requires:

$$\text{Weights} = n_{in} \times n_{out}$$

(excluding biases as stated in the problem).

**Application:**

$$\text{Weights} = 128 \times 10 = 1,280$$

**Geometric Interpretation:**

The weight matrix $W \in \mathbb{R}^{128 \times 10}$ has 128 rows (one per input dimension) and 10 columns (one per output class). Each of the 1,280 weights represents a learned connection between an input feature and an output class.

**The Classification Head Function:**

$$\text{logits} = W^T x + b = W^T z + b$$

where $x \in \mathbb{R}^{128}$ is the pretrained embedding and $W \in \mathbb{R}^{128 \times 10}$ are the weights.

**Why Transfer Learning Saves Parameters:**

- From-scratch model: Might have millions of parameters across many layers.
- Transfer learning: Only 1,280 new parameters for the classification head (plus any fine-tuned pretrained weights).
- Massive reduction in training cost!

**Analysis of Incorrect Options:**

- **A) 128:** This is just the input dimension. You need to multiply by the output dimension too.

- **B) 138:** This adds inputs to outputs ($128 + 10 = 138$). A common mistake when confusing addition with multiplication.

- **D) 2560:** This is double the correct answer ($2 \times 1,280$). Maybe thinking of both weights and biases, or multiplying by 2 for some other reason.

**Key Insight:** Fully connected layer parameter count is always $(\text{inputs}) \times (\text{outputs})$. For transfer learning, this is tiny compared to training a full network from scratch.

**Related Topics:** [Transfer Learning](notes.md#6-transfer-learning)

---

### Q7 (1 point)

**In a multi-task model, a shared representation of dimension 64 is used by two heads: one classification head with 5 outputs and one regression head with 2 outputs. Ignoring biases, how many total weights are needed in the two heads?**

- [ ] A) 320
- [ ] B) 384
- [ ] C) 448
- [ ] D) 512

**Answer:** C

**Answer Explanation:**

This question tests parameter counting for **multi-task learning**, where multiple task-specific heads share a common representation.

**The Multi-Task Setup:**

```
Shared Representation (64-dim)
         ↙         ↘
  Classification   Regression
   Head (5-way)    Head (2-way)
```

**Parameter Calculation for Each Head:**

**Classification Head:**
$$\text{Weights}_{\text{clf}} = 64 \times 5 = 320$$

Maps 64-dimensional shared representation to 5 class labels.

**Regression Head:**
$$\text{Weights}_{\text{reg}} = 64 \times 2 = 128$$

Maps 64-dimensional shared representation to 2 continuous outputs.

**Total Parameters:**
$$\text{Total} = 320 + 128 = 448$$

**Why Multi-Task Learning Works:**

1. **Shared Representation:** Both tasks learn to extract useful features from the same input.
2. **Inductive Bias:** Task 1 might help Task 2 learn better shared features (and vice versa).
3. **Parameter Efficiency:** One large shared representation is more efficient than two separate models.

**Example:**

- **Task 1:** Predict sentiment (5 classes: very negative, negative, neutral, positive, very positive).
- **Task 2:** Predict sentiment magnitude (2 values: magnitude for task 1a, magnitude for task 1b).

Both tasks benefit from a shared understanding of the text, encoded in the 64-dimensional representation.

**Analysis of Incorrect Options:**

- **A) 320:** This is only the classification head weights. It forgets the regression head.

- **B) 384:** This would be $64 \times 5 + 64 = 320 + 64 = 384$ (if regression head had only 1 output instead of 2).

- **D) 512:** This is $64 \times 8$, as if there were a single head with 8 outputs instead of two separate heads.

**Key Insight:** Multi-task parameter counting: $\sum_i (\text{shared\_dim} \times \text{output\_dim}_i)$. Each task head is independent, but they share the same input representation.

**Related Topics:** [Transfer Learning, Multi-Task Learning Concepts](notes.md#6-transfer-learning)

---

## Question Set 4: Optimization Algorithms

### Q8 (1 point)

**In plain SGD, if the current parameter is $w = 5$, learning rate $\eta = 0.1$, and gradient $\nabla L(w) = 3$, what is the updated parameter after one step?**

- [ ] A) 4.6
- [ ] B) 4.5
- [ ] C) 4.7
- [ ] D) 4.8

**Answer:** C

**Answer Explanation:**

This question tests the **basic SGD update rule**, the foundation of all gradient-based optimization.

**The SGD Update Formula:**

$$w_{t+1} = w_t - \eta \nabla L(w_t)$$

where:
- $w_t$: Current parameter.
- $\eta$: Learning rate (step size).
- $\nabla L(w_t)$: Gradient of loss w.r.t. $w$.

**Step-by-Step Calculation:**

Given:
- $w_t = 5$
- $\eta = 0.1$
- $\nabla L(w_t) = 3$

$$w_{t+1} = 5 - 0.1 \times 3 = 5 - 0.3 = 4.7$$

**Geometric Interpretation:**

The parameter moves in the **direction opposite to the gradient** (downhill on the loss surface). The learning rate controls step size:
- Large $\eta$: Big jumps (risk overshooting).
- Small $\eta$: Tiny steps (slow but careful).

**Why Negative Gradient?**

The gradient points toward increasing loss. We want to decrease loss, so we move opposite (negative) direction. This is **steepest descent**.

**Multiple Steps:**

If you took two steps:
$$w_1 = 5 - 0.1 \times 3 = 4.7$$
$$w_2 = 4.7 - 0.1 \times (\text{new gradient at } w=4.7)$$

Over many steps, this algorithm converges to a local minimum where $\nabla L(w) \approx 0$.

**Analysis of Incorrect Options:**

- **A) 4.6:** This is $5 - 0.4 = 4.6$, as if the learning rate were $0.1 \times 4$ instead of $0.1 \times 3$. Or $5 - 0.2 - 0.2 = 4.6$ (two separate mistakes).

- **B) 4.5:** This is $5 - 0.5 = 4.5$, as if the learning rate were 0.1 and the gradient were 5 instead of 3.

- **D) 4.8:** This is $5 - 0.2 = 4.8$, as if you computed $0.1 \times 2 = 0.2$ instead of $0.1 \times 3 = 0.3$.

**Key Insight:** Always apply the update rule carefully: subtract (learning rate × gradient). The minus sign is crucial—it ensures we move downhill.

**Related Topics:** [Stochastic Gradient Descent (SGD)](notes.md#8-stochastic-gradient-descent-sgd)

---

### Q9 (1 point)

**In momentum-based SGD, suppose the update rule is $v_t = 0.9v_{t-1} + 0.1g_t$ with $v_{t-1} = 2$ and current gradient $g_t = 4$. What is $v_t$?**

- [ ] A) 2.0
- [ ] B) 1.8
- [ ] C) 2.2
- [ ] D) 2.4

**Answer:** C

**Answer Explanation:**

This question tests the **momentum update**, which accumulates past gradient directions to accelerate convergence.

**The Momentum Formula:**

$$v_t = \beta v_{t-1} + (1 - \beta) g_t$$

where:
- $v_t$: Velocity (accumulated momentum).
- $\beta$: Momentum coefficient (typically 0.9), controlling how much past velocity is retained.
- $g_t$: Current gradient.

**Step-by-Step Calculation:**

Given:
- $v_{t-1} = 2$
- $g_t = 4$
- $\beta = 0.9$, so $(1 - \beta) = 0.1$

$$v_t = 0.9 \times 2 + 0.1 \times 4 = 1.8 + 0.4 = 2.2$$

**Two Components:**

1. **Momentum Term:** $0.9 \times 2 = 1.8$ (90% of previous velocity is retained).
2. **Current Gradient Term:** $0.1 \times 4 = 0.4$ (10% of current gradient is added).

**Total Velocity:** $1.8 + 0.4 = 2.2$

**Physical Intuition:**

Think of a ball rolling downhill. Momentum gives it inertia:
- It doesn't stop immediately when the slope changes (thanks to the 0.9 term).
- It responds to new directions (thanks to the 0.1 term).
- It "coasts" forward, smoothing out oscillations caused by noisy gradients.

**Why This Helps:**

In a narrow, elongated valley:
- **Without momentum:** Oscillates left-right across the valley while slowly descending.
- **With momentum:** Builds up speed downhill, coasting through side-to-side oscillations.

**Parameter Update (Next Step):**

The accumulated velocity $v_t = 2.2$ is then used to update weights:

$$w_{t+1} = w_t - \eta v_t$$

**Analysis of Incorrect Options:**

- **A) 2.0:** This would be the result if you forgot to add the current gradient term: just $0.9 \times 2 = 1.8$ is not 2.0 anyway, so this is clearly wrong.

- **B) 1.8:** This is only the momentum term $0.9 \times 2 = 1.8$, forgetting to add the current gradient contribution $0.1 \times 4 = 0.4$.

- **D) 2.4:** This is $2 + 0.4 = 2.4$, as if you added the current gradient to the previous velocity directly (wrong formula—should be a weighted combination).

**Key Insight:** The momentum update is a weighted average of past velocity (majority, 90%) and current gradient (minority, 10%). This creates a smoothed, accelerated trajectory toward minima.

**Related Topics:** [Momentum](notes.md#9-momentum)

---

### Q10 (1 point)

**In RMSProp, suppose $s_t = 0.9s_{t-1} + 0.1g_t^2$ with $s_{t-1} = 4$ and $g_t = 2$. What is $s_t$?**

- [ ] A) 4.2
- [ ] B) 4.4
- [ ] C) 4.0
- [ ] D) 3.8

**Answer:** C

**Answer Explanation:**

This question tests the **RMSProp second moment update**, which tracks the exponential moving average of squared gradients for adaptive learning rates.

**The RMSProp Formula:**

$$s_t = \beta s_{t-1} + (1 - \beta) g_t^2$$

where:
- $s_t$: Second moment (running average of squared gradients).
- $\beta$: Decay rate (typically 0.9).
- $g_t^2$: **Squared** current gradient (not just $g_t$).

**Critical Detail: Square the Gradient**

The current gradient $g_t = 2$ must be squared first:

$$g_t^2 = 2^2 = 4$$

**Step-by-Step Calculation:**

Given:
- $s_{t-1} = 4$
- $g_t = 2$, so $g_t^2 = 4$
- $\beta = 0.9$, so $(1 - \beta) = 0.1$

$$s_t = 0.9 \times 4 + 0.1 \times 4 = 3.6 + 0.4 = 4.0$$

**Two Components:**

1. **Decayed Previous Second Moment:** $0.9 \times 4 = 3.6$ (retains 90% of history).
2. **Current Squared Gradient:** $0.1 \times 4 = 0.4$ (incorporates 10% of new squared gradient).

**Total Second Moment:** $3.6 + 0.4 = 4.0$

**Why Track Squared Gradients?**

Parameters with consistently large gradients need smaller learning rate steps (to avoid instability). RMSProp identifies these by tracking $g_t^2$:

- Large gradients → large $g_t^2$ → large $s_t$ → small effective learning rate $\eta / \sqrt{s_t}$ → small steps.
- Small gradients → small $g_t^2$ → small $s_t$ → large effective learning rate → larger steps.

**The Full Update (Not Asked, But Related):**

The parameter update using RMSProp is:

$$w_{t+1} = w_t - \frac{\eta}{\sqrt{s_t + \epsilon}} g_t$$

where $\epsilon \approx 10^{-8}$ prevents division by zero.

**Analysis of Incorrect Options:**

- **A) 4.2:** This might come from $0.9 \times 4 + 0.1 \times 2 = 3.6 + 0.2 = 3.8$ (forgetting to square) + some error, or just arithmetic mistakes.

- **B) 4.4:** This could be $0.9 \times 4 + 0.1 \times 4 + 0.3 = 4.0 + 0.4$? Or $0.9 \times 4 + 0.5 = 3.6 + 0.8 = 4.4$? Some variant arithmetic error.

- **D) 3.8:** This is the result of **forgetting to square the gradient**: $0.9 \times 4 + 0.1 \times 2 = 3.6 + 0.2 = 3.8$. A very common mistake! You must square $g_t$ before using it in RMSProp's second moment update.

**Key Insight:** In RMSProp, always square the current gradient: $g_t^2$, not $g_t$. This is different from momentum, which uses $g_t$ directly. The squared term ensures positive values and penalizes large gradients more heavily.

**Related Topics:** [RMSProp (Root Mean Square Propagation)](notes.md#10-rmsprop-root-mean-square-propagation)

---

## Answer Key Summary

| Q | Answer | Concept | Difficulty |
|---|--------|---------|-----------|
| Q1 | D | Query matrix dimension (matrix multiplication rule) | Easy |
| Q2 | D | Attention score matrix dimension ($T \times T$) | Easy |
| Q3 | D | Dot-product computation between query and keys | Easy |
| Q4 | D | Multi-head dimension allocation ($d_{model}/h$) | Easy |
| Q5 | D | Total attention scores ($T^2$) | Medium |
| Q6 | C | Transfer learning head weight count | Easy |
| Q7 | C | Multi-task head parameter counting | Medium |
| Q8 | C | Plain SGD update ($w - \eta \cdot g$) | Easy |
| Q9 | C | Momentum velocity accumulation | Medium |
| Q10 | C | RMSProp second moment (squared gradients) | Medium |

**Overall Score:** 10/10 points possible

---

## Concept Map: Week 10 Progression

**From Mechanism to Architecture to Optimization:**

1. **Attention Foundations (Q1–Q3):**
   - Matrix dimensions govern data flow through neural networks.
   - Query, Key, Value are learned projections of inputs.
   - Dot products compute pairwise similarities (attention scores).

2. **Multi-Head Attention (Q4–Q5):**
   - Multiple heads operate on subspaces of total dimension $d_{model}$.
   - Each head can learn different relationship types.
   - Attention scores form a $T \times T$ matrix for sequence length $T$.

3. **Transformers:**
   - Parallel processing of all tokens (unlike sequential RNNs).
   - Self-attention allows direct gradient paths over any distance.
   - Positional embeddings preserve sequence order information.

4. **Practical Deep Learning (Q6–Q7):**
   - Transfer learning: Reuse pretrained features, fine-tune small task-specific head.
   - Multi-task learning: Share representation across multiple tasks.
   - Parameter efficiency compared to training from scratch.

5. **Optimization Algorithms (Q8–Q10):**
   - **SGD:** Basic gradient descent, move opposite to gradient.
   - **Momentum:** Accumulate velocity for accelerated convergence, smooth oscillations.
   - **RMSProp:** Adaptive learning rates based on squared gradient history.
   - **Adam:** Combines momentum (first moment) and RMSProp (second moment) for robust, fast training.

**Week 10 Unifying Theme:**

Modern deep learning success rests on three pillars:
1. **Architecture:** Transformers with attention for parallelism and long-range dependencies.
2. **Methodology:** Transfer learning and knowledge distillation for practical efficiency.
3. **Optimization:** Advanced optimizers (SGD → Momentum → RMSProp → Adam) that adaptively accelerate and stabilize training.

**Bridge to Production ML:**

These Week 10 concepts power production systems:
- **BERT, GPT, ChatGPT:** Transformer-based LLMs.
- **Vision Transformers (ViT):** Transformers for images.
- **Transfer Learning:** EVERY modern pretrained model in HuggingFace, TensorFlow Hub, PyTorch.
- **Adam:** Default optimizer in nearly all frameworks.

Understanding the mathematical foundations (as this course provides) is essential for innovating and debugging in the modern ML landscape.


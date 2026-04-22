# Week 1: Assignment 1 — Mathematical Foundations of Machine Learning (CS02)

| Metric | Value |
|--------|-------|
| Due | 2026-02-04, 23:59 IST |
| Submitted | — |
| Score | — |

---

## Question Set 1: Function Approximation and Data Representation

### Q1 (1 point)

**In the function-approximation view of supervised learning, the dataset is best represented as:**

- [ ] A) $D = \{x_1, x_2, \ldots, x_n\}$
- [ ] B) $D = \{y_1, y_2, \ldots, y_n\}$
- [x] C) $D = \{(x_1,y_1), (x_2,y_2), \ldots, (x_n,y_n)\}$
- [ ] D) $D = \{(y_1,x_1), (y_1,x_2), \ldots, (y_1,x_n)\}$

**Answer:** C

**Answer Explanation:** Supervised learning requires pairs of input-output observations. To learn the unknown mapping function $f: X \rightarrow Y$, we need both the input features ($x_i$) and their corresponding correct outputs ($y_i$). 

Incorrect options:
- **A)** represents only inputs with no labels (unsupervised learning)
- **B)** provides only labels without inputs—impossible to learn the mapping
- **D)** incorrectly pairs a single label $y_1$ with every input, which is semantically wrong

**Related Topics:** [Machine Learning as Function Approximation](notes.md#machine-learning-as-function-approximation)

---

### Q2 (1 point)

**If an image has size $p \times q$ and is vectorized for learning, it is naturally viewed as an element of:**

- [ ] A) $\mathbb{R}^{p+q}$
- [x] B) $\mathbb{R}^{pq}$
- [ ] C) $\mathbb{R}^{p/q}$
- [ ] D) $\mathbb{R}^{p-q}$

**Answer:** B

**Answer Explanation:** A $p \times q$ image contains exactly $p$ rows and $q$ columns, giving a total of $pq$ pixel values. When vectorized (flattened into a 1D array), this becomes a single point in $\mathbb{R}^{pq}$, a space with $pq$ dimensions.

Incorrect options:
- **A)** $\mathbb{R}^{p+q}$ incorrectly adds dimensions instead of multiplying (e.g., 10×10 image would be $\mathbb{R}^{20}$ instead of $\mathbb{R}^{100}$)
- **C)** $\mathbb{R}^{p/q}$ produces a non-integer dimension (aspect ratio, not total pixels)
- **D)** $\mathbb{R}^{p-q}$ can be negative or zero, which is mathematically invalid

**Related Topics:** [Vectorization of Data](notes.md#vectorization-of-data)

---

### Q3 (1 point)

**In binary classification, the label set commonly used is:**

- [x] A) $Y = \{0, 1\}$
- [ ] B) $Y = \{-1, +1, +2\}$
- [ ] C) $Y = \{1, 2, 3\}$
- [ ] D) $Y = \mathbb{R}$

**Answer:** A

**Answer Explanation:** "Binary" means exactly two distinct categories. The standard convention represents these two classes using discrete integers 0 and 1 (e.g., 0 = non-diseased, 1 = diseased).

Incorrect options:
- **B)** and **C)** contain three distinct values, making them multi-class problems, not binary
- **D)** $\mathbb{R}$ (all real numbers) indicates a continuous output space, which is regression, not classification

**Related Topics:** [Classification Task Setup](notes.md#classification-task-setup)

---

## Question Set 2: Why Probability in Machine Learning

### Q4 (1 point)

**Which statement best captures why probability is introduced in the ML viewpoint discussed?**

- [ ] A) Because $f$ is always linear
- [ ] B) Because $f$ is known exactly from physics
- [x] C) Because the true mapping $f$ is complex/uncertain and we model uncertainty statistically
- [ ] D) Because images always have pixel values in a bounded range

**Answer:** C

**Answer Explanation:** In real-world applications (medical diagnosis, image classification, etc.), the exact rule that maps inputs to outputs is far too complex to write down manually or derive from first principles. Additionally, inherent variability and noise mean outcomes are not deterministic. Probability theory provides the mathematical framework to model and manage this uncertainty.

Incorrect options:
- **A)** Modern machine learning deals extensively with highly non-linear functions
- **B)** If the function were exactly known, we would not need machine learning or statistics
- **D)** While bounded pixel values are a technical property, this is not the fundamental reason we use probability; probability is needed because of complexity and uncertainty, not data representation

**Related Topics:** [Probabilistic Viewpoint](notes.md#probabilistic-viewpoint)

---

## Question Set 3: Probability Space and Axioms

### Q5 (1 point)

**A probability space is a triplet:**

- [ ] A) $(X, Y, f)$
- [x] B) $(\Omega, \mathcal{F}, P)$
- [ ] C) $(\mathbb{R}, \Omega, \mathcal{F})$
- [ ] D) $(P, \Omega, \mathbb{R}^d)$

**Answer:** B

**Answer Explanation:** The probability space is the foundational mathematical object in probability theory. It consists of:
- $\Omega$ (sample space): all possible outcomes
- $\mathcal{F}$ (event space): collection of measurable events
- $P$ (probability measure): function assigning likelihoods to events

Incorrect options:
- **A)** $(X, Y, f)$ describes the function-approximation setup (input space, output space, target function), not probability theory
- **C)** Incorrectly mixes $\mathbb{R}$ (real numbers) into the definition, replacing the vital probability measure
- **D)** Drops the event space $\mathcal{F}$ and replaces it with $\mathbb{R}^d$ (vector space), breaking the definition

**Related Topics:** [Probability Space: The Foundation](notes.md#probability-space-the-foundation)

---

### Q6 (1 point)

**For any event $A$, which of the following must always hold for a probability measure $P$?**

- [ ] A) $P(A) \in [-1, 1]$
- [x] B) $P(A) \geq 0$
- [ ] C) $P(A) > 1$
- [ ] D) $P(A) = 0$ for all $A$

**Answer:** B

**Answer Explanation:** This is the **First Axiom of Probability (Non-Negativity)**. Probabilities must be non-negative; you cannot have a negative likelihood of an event occurring.

Incorrect options:
- **A)** Negative probabilities do not exist; probabilities are strictly in $[0, 1]$
- **C)** Probabilities cannot exceed 1 (100%); the maximum is $P(\Omega) = 1$
- **D)** This would mean nothing ever happens; $P(A) = 0$ only for impossible events, not all events

**Related Topics:** [Axioms of Probability](notes.md#axioms-of-probability)

---

### Q7 (1 point)

**If $A$ and $B$ are disjoint events (i.e., $A \cap B = \emptyset$), then:**

- [ ] A) $P(A \cup B) = P(A) \cdot P(B)$
- [x] B) $P(A \cup B) = P(A) + P(B)$
- [ ] C) $P(A \cup B) = P(A) - P(B)$
- [ ] D) $P(A \cup B) = 1$

**Answer:** B

**Answer Explanation:** This is the **Third Axiom of Probability (Additivity for disjoint events)**. If two events are disjoint (cannot happen simultaneously), the probability that either one occurs is exactly the sum of their individual probabilities.

**Intuition:** If outcomes are mutually exclusive, their probabilities add directly.

Incorrect options:
- **A)** $P(A) \cdot P(B)$ applies to independent events occurring together ($P(A \cap B)$), not disjoint events
- **C)** This formula has no basis in probability axioms
- **D)** This is only true if $A$ and $B$ together exhaust the sample space (are collectively exhaustive), not for all disjoint events

**Related Topics:** [Axioms of Probability](notes.md#axioms-of-probability)

---

## Question Set 4: Random Variables and Distributions

### Q8 (1 point)

**A random variable is defined as:**

- [ ] A) A number chosen by the teacher
- [x] B) A function $X: \Omega \rightarrow \mathbb{R}$
- [ ] C) A set of all outcomes $\Omega$
- [ ] D) A probability measure $P: \mathcal{F} \rightarrow [0,1]$

**Answer:** B

**Answer Explanation:** A random variable is a *function*, not an ordinary variable. It maps abstract outcomes from the sample space ($\Omega$) into real numbers ($\mathbb{R}$), allowing us to perform mathematical operations. This "translation" makes probability calculations feasible.

Incorrect options:
- **A)** A joke/nonsensical distractor
- **C)** This is the definition of the sample space, not a random variable
- **D)** This is the definition of a probability measure, a separate mathematical object

**Related Topics:** [Random Variables](notes.md#random-variables)

---

### Q9 (1 point)

**The distribution induced by a random variable $X$ is commonly described through the probability of events of the form:**

- [ ] A) $P(\{X = x\})$
- [x] B) $P(\{X \leq x\})$
- [ ] C) $P(\{\omega \leq x\})$
- [ ] D) $P(\{x \in \Omega\})$

**Answer:** B

**Answer Explanation:** This is the **Cumulative Distribution Function (CDF)**. The CDF describes the probability that a random variable $X$ is less than or equal to a specific value $x$: $F_X(x) = P(X \leq x)$. This definition is universal—it works for both discrete and continuous random variables.

**Why $\leq$ instead of $=$?** For continuous random variables, the probability of being exactly equal to one specific number is zero. Therefore, we use the cumulative form for a robust, general definition.

Incorrect options:
- **A)** $P(X = x)$ is the Probability Mass Function (PMF), which only works for discrete variables
- **C)** $\omega$ is an abstract outcome in the sample space; you cannot apply mathematical operators like "$\leq$" to it until it is mapped to a number by a random variable
- **D)** $x$ is a real number, not an element of $\Omega$ (the outcome space); this statement is semantically invalid

**Related Topics:** [Scalar Random Variables](notes.md#scalar-random-variables)

---

### Q10 (1 point)

**A vector-valued random variable in $\mathbb{R}^d$ is defined as:**

- [ ] A) $X: \mathbb{R}^d \rightarrow \Omega$
- [x] B) $X: \Omega \rightarrow \mathbb{R}^d$
- [ ] C) $X: \Omega \rightarrow \mathcal{F}$
- [ ] D) $X: \mathcal{F} \rightarrow [0,1]$

**Answer:** B

**Answer Explanation:** Just as a scalar random variable maps outcomes to single numbers, a vector-valued random variable maps outcomes from the sample space ($\Omega$) to $d$-dimensional vectors of numbers ($\mathbb{R}^d$). This is essential in machine learning, where data typically has multiple features.

Incorrect options:
- **A)** This is backward; random variables map *from* outcomes *to* numbers, not the other way around
- **C)** $\mathcal{F}$ is the event space (a collection of subsets); random variables map to numerical values, not to sets
- **D)** This is the definition of a probability measure $P$, not a random variable

**Related Topics:** [Vector-Valued Random Variables](notes.md#vector-valued-random-variables)

---

## Answer Key Summary

| Q | Answer | Concept | Difficulty |
|---|--------|---------|-----------|
| Q1 | C | Dataset representation in supervised learning | Easy |
| Q2 | B | Image vectorization and dimensionality | Easy |
| Q3 | A | Binary classification label set | Easy |
| Q4 | C | Role of probability in modeling uncertainty | Medium |
| Q5 | B | Probability space definition | Medium |
| Q6 | B | Axiom of non-negativity | Easy |
| Q7 | B | Additivity axiom for disjoint events | Medium |
| Q8 | B | Definition of random variable | Medium |
| Q9 | B | Cumulative distribution function | Medium |
| Q10 | B | Vector-valued random variable definition | Easy |

**Overall Score:** 10/10 points possible


# Week 1: Theoretical Notes — Function Approximation and Probability Foundations

## Overview

This week introduces supervised machine learning as a function approximation problem where we try to learn an unknown mapping function $f: X \rightarrow Y$ from data. We explore both deterministic and probabilistic viewpoints, then establish the mathematical foundations of probability theory—including probability spaces, axioms, random variables, and distributions—that underpin all statistical learning.

## Machine Learning as Function Approximation

**Function Approximation** is the core concept of supervised learning. Instead of writing the rules manually, we provide examples and let the algorithm infer the underlying pattern.

**Key Setup:**
- **Input Space ($X$):** The domain of possible inputs (e.g., images, measurements)
- **Output Space ($Y$):** The range of possible outputs (e.g., labels, predictions)
- **Unknown Function ($f$):** An unknown, true mapping $f: X \rightarrow Y$ that we assume exists
- **Dataset:** A collection of input-output pairs $D = \{(x_1, y_1), (x_2, y_2), \ldots, (x_n, y_n)\}$ where each $y_i$ is the correct output for input $x_i$ under this unknown function

**Example:** In medical image diagnosis, $x_i$ is an X-ray image and $y_i \in \{0, 1\}$ represents the diagnosis (0 = healthy, 1 = diseased).

**Related Assignment Questions**: Q1

## Deterministic Viewpoint

In the **deterministic view**, we assume:
1. There exists a true function $f$ that perfectly maps inputs to outputs: $f(x_i) = y_i$ for all training data
2. Our job is to find a function $\hat{f}$ (our estimate) that closely approximates $f$
3. Once learned, $\hat{f}$ will reliably predict outputs for new, unseen inputs

This view treats the mapping as a fixed, unknown rule we must uncover through data and optimization.

## Probabilistic Viewpoint

In the **probabilistic view**, we acknowledge that real-world mappings are often too complex to describe deterministically. Instead:

**Why Probability?** The true mapping function $f$ is often extremely complex or contains inherent uncertainty. In domains like medical diagnosis, many factors influence the outcome, and even identical inputs might have different labels due to noise, measurement error, or genuine variability. Therefore, we model the relationship statistically using probability theory.

**Statistical Model:** Rather than searching for one deterministic function, we model the output $Y$ as a random variable whose distribution depends on the input $X$. We assume our dataset consists of independent, identically distributed (i.i.d.) samples drawn from an unknown probability distribution, and our goal is to estimate that distribution.

**Related Assignment Questions**: Q4

## Vectorization of Data

Real-world data (images, audio, text) must be converted into numerical vectors for mathematical processing.

**Image Vectorization Example:** An image of size $p \times q$ pixels contains $p \times q$ pixel values. To feed this into a learning algorithm, we "flatten" it into a single one-dimensional vector with $pq$ entries. Each vectorized image is now a single point in $\mathbb{R}^{pq}$—a $pq$-dimensional vector space.

**Related Assignment Questions**: Q2

## Classification Task Setup

**Binary Classification:** A supervised learning task with exactly two distinct output classes.
- **Label Set:** $Y = \{0, 1\}$ (standard convention: 0 = negative class, 1 = positive class)
- **Examples:** Spam vs. Not Spam, Diseased vs. Healthy, Approved vs. Rejected

**Multi-class Classification:** More than two classes (e.g., $Y = \{1, 2, 3\}$ for digit recognition 1-3)

**Regression:** Continuous output space $Y = \mathbb{R}$ (e.g., predicting house price, temperature)

**Related Assignment Questions**: Q3

## Probability Space: The Foundation

A **Probability Space** is the mathematical bedrock for modeling uncertainty. It is formally defined as a triplet:

$$(\Omega, \mathcal{F}, P)$$

Where:

- **Sample Space ($\Omega$):** The set of all possible outcomes of a random experiment. Every conceivable result is an element of $\Omega$.
  - *Example:* Rolling a die: $\Omega = \{1, 2, 3, 4, 5, 6\}$
  - *Example:* Patient diagnosis: $\Omega = \{\text{healthy}, \text{diseased}\}$ or abstractly any outcome space

- **Event Space ($\mathcal{F}$):** A collection of subsets of $\Omega$ that we care about. Not all subsets are included—only those that are "measurable" (we can assign probability to them).
  - *Example:* For a die roll, the event "rolling an even number" is the set $\{2, 4, 6\} \in \mathcal{F}$
  - *Example:* The event "rolling a number less than 5" is $\{1, 2, 3, 4\} \in \mathcal{F}$
  - The entire sample space $\Omega$ and the empty set $\emptyset$ are always in $\mathcal{F}$

- **Probability Measure ($P$):** A function $P: \mathcal{F} \rightarrow [0, 1]$ that assigns a probability (likelihood) to each event in $\mathcal{F}$.
  - *Example:* $P(\text{rolling a 3}) = 1/6$
  - *Example:* $P(\text{rolling even}) = 1/2$

**Related Assignment Questions**: Q5

## Axioms of Probability

For a function $P$ to be a valid probability measure, it must satisfy three fundamental axioms:

### Axiom 1: Non-Negativity
$$P(A) \geq 0 \quad \text{for all events } A \in \mathcal{F}$$

Probabilities cannot be negative. The likelihood of any event is always non-negative.

### Axiom 2: Certainty (Normalization)
$$P(\Omega) = 1$$

The probability that *some* outcome in $\Omega$ occurs is always 1 (100% certain). The probability of the empty set (nothing happens) is $P(\emptyset) = 0$ (impossible).

### Axiom 3: Additivity (for disjoint events)
If $A$ and $B$ are **disjoint events** (they cannot both occur simultaneously, i.e., $A \cap B = \emptyset$), then:
$$P(A \cup B) = P(A) + P(B)$$

The probability that either event $A$ or event $B$ occurs is the sum of their individual probabilities.

**Intuition:** If two outcomes cannot happen together, their probabilities simply add.

**Example:** A patient either has disease type X (probability 0.3) or disease type Y (probability 0.2), and these are mutually exclusive. The probability of having one of these two diseases is $0.3 + 0.2 = 0.5$.

**Related Assignment Questions**: Q6, Q7

## Random Variables

A **Random Variable** is not a standard variable—it is a *function* that maps abstract outcomes to real numbers, allowing us to do mathematics.

### Scalar Random Variables

$$X: \Omega \rightarrow \mathbb{R}$$

A scalar random variable takes an outcome from the sample space and assigns it a real number.

**Intuition:** The sample space $\Omega$ may contain abstract, non-numeric outcomes. A random variable "translates" these outcomes into numbers we can work with mathematically.

**Example:** 
- Outcome space: Patients either recover or don't recover
- Random variable: $X = 1$ if patient recovers, $X = 0$ if patient doesn't recover
- Now we can compute probabilities like $P(X = 1)$

**Key Insight:** We describe the behavior of a random variable using the **Cumulative Distribution Function (CDF)**:

$$F_X(x) = P(X \leq x)$$

This gives the probability that the random variable $X$ takes a value less than or equal to $x$. This definition works for both discrete (integer-valued) and continuous (real-valued) random variables.

**Why $X \leq x$ instead of $X = x$?** For continuous random variables, the probability of being exactly equal to one specific number is zero. So we use the cumulative form $P(X \leq x)$ for a universal definition that applies everywhere.

**Related Assignment Questions**: Q8, Q9

### Vector-Valued Random Variables

$$X: \Omega \rightarrow \mathbb{R}^d$$

A vector-valued random variable maps outcomes to $d$-dimensional vectors of numbers. This is essential in machine learning, where data typically has many features.

**Example:**
- A vectorized image is a vector-valued random variable in $\mathbb{R}^{pq}$ (where $p \times q$ is image size)
- A medical record with patient age, weight, blood pressure, etc., is a random variable in $\mathbb{R}^3$

**Intuition:** Each vectorized datapoint in machine learning is treated as a realization (sample) from a vector-valued random variable drawn from an unknown probability distribution.

**Related Assignment Questions**: Q10

## Summary

- **Function Approximation View:** Machine learning learns an unknown function $f: X \rightarrow Y$ from a labeled dataset $D = \{(x_i, y_i)\}$

- **Data Vectorization:** Non-numeric data (images, etc.) is flattened into points in $\mathbb{R}^{pq}$, where the dimension equals the number of features

- **Probability Necessity:** Real-world mappings are complex and uncertain; probability theory provides a framework to model this uncertainty statistically

- **Probability Space ($\Omega, \mathcal{F}, P$):**
  - $\Omega$ = all possible outcomes
  - $\mathcal{F}$ = collection of events (measurable subsets)
  - $P$ = function assigning likelihoods to events

- **Three Axioms of Probability:**
  1. Non-negativity: $P(A) \geq 0$
  2. Certainty: $P(\Omega) = 1$
  3. Additivity (disjoint): $P(A \cup B) = P(A) + P(B)$ when $A \cap B = \emptyset$

- **Random Variables:** Functions mapping outcomes to numbers. Distributions described via cumulative distribution: $P(X \leq x)$

- **Vector-Valued Random Variables:** Map outcomes to $\mathbb{R}^d$; essential for multi-feature data like images and patient records

## Key Takeaways

| Concept | Definition | Example |
|---------|-----------|---------|
| **Function Approximation** | Learning an unknown mapping $f: X \rightarrow Y$ from labeled pairs $(x_i, y_i)$ | X-ray images → diagnosis (0 or 1) |
| **Sample Space** | Set of all possible outcomes $\Omega$ in an experiment | Die roll: $\{1,2,3,4,5,6\}$ |
| **Event Space** | Collection $\mathcal{F}$ of measurable subsets of $\Omega$ | $\{\text{even outcomes}\} = \{2,4,6\}$ |
| **Probability Measure** | Function $P: \mathcal{F} \rightarrow [0,1]$ assigning likelihoods to events | $P(\text{rolling 3}) = 1/6$ |
| **Non-Negativity Axiom** | All probabilities satisfy $P(A) \geq 0$ | No negative probabilities |
| **Certainty Axiom** | $P(\Omega) = 1$ and $P(\emptyset) = 0$ | Something must happen |
| **Additivity Axiom** | For disjoint $A, B$: $P(A \cup B) = P(A) + P(B)$ | Mutually exclusive outcomes add |
| **Random Variable** | Function $X: \Omega \rightarrow \mathbb{R}$ mapping outcomes to numbers | Diagnosis: 1 if diseased, 0 otherwise |
| **Cumulative Distribution** | $F_X(x) = P(X \leq x)$ describes random variable behavior | $P(\text{height} \leq 180\text{ cm})$ |
| **Vector-Valued RV** | Function $X: \Omega \rightarrow \mathbb{R}^d$ mapping to $d$-dimensional vectors | Vectorized image in $\mathbb{R}^{pq}$ |

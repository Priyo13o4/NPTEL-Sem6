# Week 2: Theoretical Notes — Setting Up the ML Problem

## Overview

This week expands from basic probability to how real-world data is modeled in machine learning. We introduce joint and conditional distributions, establish the i.i.d. assumption for datasets, and frame the core ML challenge as distribution estimation. The key insight is that supervised learning fundamentally requires estimating an unknown joint probability distribution from samples, using parametric models and optimization.

## Joint Distributions

When dealing with multiple random variables, we need to understand how they interact together.

**Definition:** For two random variables $X_1: \Omega \rightarrow \mathbb{R}$ and $X_2: \Omega \rightarrow \mathbb{R}$ on the same sample space, the **joint cumulative distribution function** is:

$$P_{X_1 X_2}(a, b) = P(X_1 \leq a, X_2 \leq b)$$

This represents the probability that $X_1$ is less than or equal to $a$ *and* $X_2$ is less than or equal to $b$ simultaneously.

**Intuition:** While a univariate distribution shows how a single variable behaves, a joint distribution captures how two variables behave together. It reveals whether high values of $X_1$ tend to occur with high or low values of $X_2$.

**Generalization to Vector-Valued Data:** For a $d$-dimensional random vector $X: \Omega \rightarrow \mathbb{R}^d$, the joint distribution naturally extends to:

$$P_X(\mathbf{a}) = P(X_1 \leq a_1, X_2 \leq a_2, \ldots, X_d \leq a_d)$$

This is essential in machine learning, where each datapoint (an image, a patient's features) is represented as a vector in $\mathbb{R}^d$.

**Related Assignment Questions**: Q1, Q2

## Marginal Distributions

Often we have a joint distribution but only care about one variable.

**Definition:** The **marginal distribution** of $X$ is obtained from a joint distribution $P_{XY}$ by "integrating out" (for continuous variables) or "summing out" (for discrete variables) the other variable $Y$:

$$P_X(x) = \int_{-\infty}^{\infty} P_{XY}(x, y) \, dy \quad \text{(continuous case)}$$

$$P_X(x) = \sum_{y} P_{XY}(x, y) \quad \text{(discrete case)}$$

**Intuition:** Imagine a 3D landscape showing how $X$ and $Y$ vary together. To find just the marginal distribution of $X$, you collapse the $Y$ axis by summing/integrating across all possible $Y$ values. The result is a 1D curve representing the overall probability of $X$ regardless of $Y$.

**Example:** If you know the joint distribution of height and weight in a population, the marginal distribution of height is obtained by summing over all possible weights. It tells you the overall distribution of height without conditioning on anything.

**Related Assignment Questions**: Q5

## Conditional Probability and Conditional Distributions

Conditional probability addresses the question: "What is the probability of event $A$ given that event $B$ has already occurred?"

**Definition:** For events $A, B \in \mathcal{F}$ with $P(B) > 0$, the **conditional probability** of $A$ given $B$ is:

$$P(A \mid B) = \frac{P(A \cap B)}{P(B)}$$

This formula divides the joint probability of both events occurring by the probability of the condition $B$.

**Intuition:** When we condition on $B$, we're essentially restricting our attention to a smaller universe where $B$ is true. We then ask: within this restricted universe, what fraction of cases also satisfy $A$?

**Example:** 
- $P(\text{Patient is diseased}) = 0.1$ (unconditional)
- $P(\text{Patient is diseased AND tests positive}) = 0.09$
- $P(\text{Patient tests positive}) = 0.15$
- $P(\text{Patient is diseased} \mid \text{tests positive}) = \frac{0.09}{0.15} = 0.6$ (conditional)

### Conditional Distribution

Applying conditional probability to random variables:

$$P_{X \mid Y} = \frac{P_{XY}}{P_Y}$$

This is the **conditional distribution** of $X$ given $Y$. In the context of machine learning, this is exactly what classification solves: estimating $P(Y \mid X)$, the probability of a label $Y$ given the observed features $X$.

**In supervised learning:** We want to learn the conditional distribution $P(Y \mid X)$ because it tells us: given a new input $X$, what is the probability of each possible output/label $Y$?

**Related Assignment Questions**: Q3, Q4

## The Independent and Identically Distributed (i.i.d.) Assumption

Real-world datasets are collections of samples. To use probability theory effectively, we make a crucial assumption.

**Definition:** We say observations are **i.i.d.** (independent and identically distributed) if:

1. **Identically Distributed:** Every observation $(x_i, y_i)$ is drawn from the same probability distribution $P_{XY}$
2. **Independent:** The observations are drawn independently—the value of $(x_1, y_1)$ does not influence the probability of observing $(x_2, y_2)$

Formally, a dataset is represented as:

$$D = \{(x_1, y_1), (x_2, y_2), \ldots, (x_n, y_n)\} \sim_{\text{iid}} P_{XY}$$

The notation "$\sim_{\text{iid}} P_{XY}$" means each pair is independently sampled from the joint distribution.

**Identically Distributed:** In medical imaging, every patient's X-ray comes from the same underlying population distribution (the distribution of healthy and diseased populations doesn't change between patients).

**Independence Across Datapoints:** Patient A's diagnosis does not influence Patient B's diagnosis. They are independent trials.

### Important Distinction: Features Within a Datapoint

**Critical clarification:** The i.i.d. assumption applies *across* datapoints, **not within** a single datapoint.

Within a single image (a single datapoint $x \in \mathbb{R}^d$):
- Neighboring pixels are highly dependent on each other
- Features are generally correlated
- We do *not* assume independence across the $d$ coordinates

This distinction is crucial: ignoring dependencies between features (like assuming pixel independence in an image) destroys the structure of the data and typically hurts model performance. Modern algorithms leverage feature dependencies to make better predictions.

**Related Assignment Questions**: Q6, Q7, Q8

## Probability Density Function (PDF) and Cumulative Distribution Function (CDF)

For continuous random variables, we work with two related functions.

**Cumulative Distribution Function (CDF):** 
$$P_X(x) = P(X \leq x)$$

The CDF gives the probability that the random variable $X$ is less than or equal to a specific value $x$. It is defined for both discrete and continuous variables. The CDF is always increasing and ranges from 0 to 1.

**Probability Density Function (PDF):**
$$p_X(x) = \frac{d}{dx} P_X(x)$$

The PDF (denoted with lowercase $p$) is the rate of change of the CDF. It represents the relative likelihood of the variable taking a value near $x$. For continuous variables, the PDF must be non-negative and integrate to 1.

**Fundamental Relationship:**

$$P_X(x) = \int_{-\infty}^{x} p_X(t) \, dt$$

The CDF is the accumulated area under the PDF curve from $-\infty$ to $x$.

**Intuition:** 
- The PDF is like the "density" or "height" at each point on the number line
- The CDF is the cumulative total of that density up to point $x$
- The PDF tells you "density at this point"; the CDF tells you "total probability up to this point"

**Example:** For a normally distributed random variable (bell curve):
- The PDF peaks at the mean and drops off on both sides
- The CDF is S-shaped, starting at 0, crossing 0.5 at the mean, and approaching 1

**Related Assignment Questions**: Q9

## Distribution Estimation: The Core ML Problem

The fundamental challenge in machine learning is that the true probability distribution $P_{XY}$ that generates our data is unknown.

**The Problem Statement:** 
Given i.i.d. samples $D = \{(x_i, y_i)\}_{i=1}^n$ from an unknown distribution $P_{XY}$, we want to estimate the distribution to make predictions on new data.

### Parametric Approach

In **parametric estimation**, we assume the unknown distribution has a specific functional form controlled by parameters.

**Step 1: Choose a Model Family**
Assume the density belongs to a family $p_{\theta}$ parameterized by $\theta$:
$$p_{\theta}(x, y) = \text{some function with parameters } \theta$$

**Example:** Assume the data follows a Gaussian (normal) distribution with parameters:
- $\theta = (\mu, \Sigma)$ where $\mu$ is the mean vector and $\Sigma$ is the covariance matrix
- Then $p_{\theta}(x) = \frac{1}{(2\pi)^{d/2}|\Sigma|^{1/2}} \exp\left(-\frac{1}{2}(x-\mu)^T\Sigma^{-1}(x-\mu)\right)$

**Step 2: Define a Distance Metric**
Choose a metric $d(P_{XY}, p_{\theta})$ to measure how far our parametric guess is from the true distribution:
- Maximum Likelihood Estimation (MLE): measures the likelihood of observing the data under $p_{\theta}$
- Kullback-Leibler Divergence: measures the information loss from using $p_{\theta}$ instead of the true distribution

**Step 3: Optimize Parameters**
Find the parameters that minimize the distance:
$$\theta^* = \arg\min_{\theta} d(P_{XY}, p_{\theta})$$

This optimization process is what we call **training the model**. We adjust $\theta$ to fit the data as well as possible.

**Advantages:**
- Efficient: requires learning only a few parameters
- Interpretable: parameters often have meaningful interpretations
- Theoretical guarantees: convergence rates are well-studied

**Disadvantages:**
- May be wrong: if the true distribution doesn't match our chosen family, we get poor estimates
- Risk of underfitting: oversimplified models miss important structure

**Contrast with Non-Parametric Methods:**
Non-parametric approaches (like K-Nearest Neighbors or kernel methods) don't assume a fixed functional form. They directly estimate the distribution from data patterns, offering more flexibility but requiring more data and computation.

**Related Assignment Questions**: Q10

## Summary

- **Joint Distributions:** $P_{XY}(a,b) = P(X \leq a, Y \leq b)$ captures how multiple variables behave together

- **Marginal Distributions:** Obtained by integrating/summing out other variables from the joint: $P_X(x) = \int P_{XY}(x,y) \, dy$

- **Conditional Probability:** $P(A \mid B) = \frac{P(A \cap B)}{P(B)}$ gives probability of $A$ given $B$ has occurred

- **Conditional Distribution:** $P_{X \mid Y} = \frac{P_{XY}}{P_Y}$; crucial for classification (estimating $P(Y \mid X)$)

- **i.i.d. Assumption:** Datapoints are identically distributed (from same population) and independent (across datapoints, not within)

- **PDF vs CDF Relationship:** $P_X(x) = \int_{-\infty}^x p_X(t) \, dt$; CDF accumulates PDF

- **Parametric Distribution Estimation:** Assume a family $p_{\theta}$, define a distance metric, optimize parameters $\theta$ to fit data

- **Core ML Challenge:** With unknown true distribution, we use observed data to find best-fitting parameters

## Key Takeaways

| Concept | Definition | Example |
|---------|-----------|---------|
| **Joint Distribution** | $P_{XY}(a,b) = P(X \leq a, Y \leq b)$; probability of both events | Height and weight behavior together |
| **Marginal Distribution** | Distribution of one variable obtained by integrating out others | Height distribution regardless of weight |
| **Conditional Probability** | $P(A \mid B) = \frac{P(A \cap B)}{P(B)}$; probability of A given B | Disease given positive test result |
| **Conditional Distribution** | $P_{X \mid Y} = \frac{P_{XY}}{P_Y}$; distribution of X given Y | Label distribution given input features |
| **i.i.d. Samples** | Identically distributed and independently sampled from $P_{XY}$ | Dataset of independent patient records |
| **Independence Across Datapoints** | One datapoint doesn't influence another | Patient A's diagnosis independent of B's |
| **Dependence Within Datapoint** | Features within a point are generally correlated | Pixels in an image are dependent |
| **PDF** | Probability density function; derivative of CDF | Bell curve of a normal distribution |
| **CDF** | Cumulative distribution; $P_X(x) = \int_{-\infty}^x p_X(t) dt$ | S-shaped curve accumulating probability |
| **Parametric Estimation** | Assume family $p_{\theta}$ and optimize parameters | Fitting Gaussian with $\mu, \Sigma$ |

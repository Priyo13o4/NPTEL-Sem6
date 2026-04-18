# Week 3: Assignment 3 — Introduction to Large Language Models (LLMs) (noc26_cs88)

> **Due Date:** —
> **Submitted:** —
> **Total Score:** —

---

### Q1 (1 point)
**Which component of a neural network introduces non-linearity into the model?**

- [ ] A) Weights
- [ ] B) Bias
- [ ] C) Activation function
- [ ] D) Loss function

**Answer:** C

**Answer Explanation**: Weights and bias form an affine (linear + shift) transformation $w^\top x + b$. The **activation function** applies a non-linear mapping to that value, enabling the network to represent non-linear decision boundaries.

**Related Topic**: [Activation functions](notes.md#activation-functions)

---

### Q2 (1 point)
**If all activation functions in an MLP are linear, what will be the effect on the network?**

- [ ] A) It becomes equivalent to logistic regression
- [ ] B) It becomes equivalent to a single linear transformation
- [ ] C) It will not converge
- [ ] D) It will overfit

**Answer:** B

**Answer Explanation**: Composing linear layers without non-linearities collapses into one linear map: $W_2(W_1x)=(W_2W_1)x$. So depth provides no extra expressive power if activations are linear.

**Related Topic**: [Why non-linearity matters](notes.md#why-non-linearity-matters)

---

### Q3 (1 point)
**Which activation function is least likely to suffer from vanishing gradients?**

- [ ] A) Tanh
- [ ] B) Sigmoid
- [ ] C) ReLU
- [ ] D) Step function

**Answer:** C

**Answer Explanation**: Sigmoid and tanh saturate, giving very small derivatives over large input ranges, which causes gradients to shrink through many layers. **ReLU** has derivative 1 for positive inputs, so it is less prone to vanishing gradients in the active region.

**Related Topic**: [Vanishing gradient (intuition)](notes.md#vanishing-gradient-intuition)

---

### Q4 (1 point)
**What happens to dropout during inference?**

- [ ] A) Dropout is applied with higher probability
- [ ] B) Dropout is applied with lower probability
- [ ] C) Dropout is disabled
- [ ] D) Dropout is replaced by L2 regularization

**Answer:** C

**Answer Explanation**: Dropout is a **training-time** regularizer. During inference/evaluation, dropout is disabled so the model uses its full capacity deterministically.

**Related Topic**: [Regularization (generalization over memorization)](notes.md#regularization-generalization-over-memorization)

---

### Q5 (1 point)
**Why can a single-layer perceptron not solve the XOR problem?**

- [ ] A) XOR requires probabilistic outputs
- [ ] B) XOR is non-linear and not linearly separable
- [ ] C) XOR has more than two classes
- [ ] D) XOR requires backpropagation

**Answer:** B

**Answer Explanation**: A single-layer perceptron can only learn a **linear** decision boundary (a hyperplane). XOR’s classes are **not linearly separable**, so no single hyperplane can separate them.

**Related Topic**: [XOR problem (why a single layer fails)](notes.md#xor-problem-why-a-single-layer-fails)

---

### Q6 (1 point)
**Among the following options, select the statement that is TRUE about backpropagation.**

- [ ] A) It updates weights using only forward pass
- [ ] B) It is used to randomly update the weights of a neural network
- [ ] C) It applies the chain rule from output to input layers
- [ ] D) It works only for sigmoid activations

**Answer:** C

**Answer Explanation**: Backpropagation computes gradients of the loss with respect to parameters using the **chain rule**, propagating error information from the output layer backward through earlier layers.

**Related Topic**: [Backpropagation (how networks learn)](notes.md#backpropagation-how-networks-learn)

---

### Q7 (1 point)
**Which type of regularization encourages sparsity in the weights?**

- [ ] A) L1 regularization
- [ ] B) L2 regularization
- [ ] C) Dropout
- [ ] D) Early stopping

**Answer:** A

**Answer Explanation**: **L1 regularization** penalizes absolute weight magnitude, which tends to push many weights exactly to 0, producing sparse parameter vectors.

**Related Topic**: [Regularization (generalization over memorization)](notes.md#regularization-generalization-over-memorization)

---

### Q8 (1 point)
**What can one observe during the training of a neural network if the learning rate in gradient descent is set too high?**

- [ ] A) Loss decreases smoothly
- [ ] B) Loss oscillates or diverges
- [ ] C) The loss always stays zero
- [ ] D) None of these

**Answer:** B

**Answer Explanation**: With too high a learning rate, updates overshoot minima, causing unstable training where the loss can oscillate or blow up (diverge).

**Related Topic**: [Backpropagation (how networks learn)](notes.md#backpropagation-how-networks-learn)

---

### Q9 (1 point)
**State the reason for softmax being commonly used in multi-class classification.**

- [ ] A) Outputs values in [−1, 1]
- [ ] B) Produces probabilities that sum to 1
- [ ] C) Avoids overfitting
- [ ] D) Eliminates the need for regularization

**Answer:** B

**Answer Explanation**: **Softmax** turns a vector of logits into a probability distribution: all outputs are non-negative and sum to 1, making them interpretable as class probabilities.

**Related Topic**: [Activation functions](notes.md#activation-functions)

---

### Q10 (1 point)
**What is the primary goal of regularization in machine learning?**

- [ ] A) To speed up the training process and help in faster convergence
- [ ] B) To reduce overfitting and improve generalization
- [ ] C) To help increase the model complexity
- [ ] D) None of these

**Answer:** B

**Answer Explanation**: Regularization techniques constrain or perturb learning so the model doesn’t memorize training noise, improving performance on unseen data (generalization).

**Related Topic**: [Regularization (generalization over memorization)](notes.md#regularization-generalization-over-memorization)

---

## Answer Key Summary

| Q | Answer | Concept |
|:--:|:--:|:---|
| 1 | C | Non-linearity via activations |
| 2 | B | Linear activations collapse depth |
| 3 | C | ReLU and vanishing gradients |
| 4 | C | Dropout disabled at inference |
| 5 | B | XOR not linearly separable |
| 6 | C | Backprop = chain rule backward |
| 7 | A | L1 encourages sparsity |
| 8 | B | Too-high learning rate instability |
| 9 | B | Softmax probabilities sum to 1 |
| 10 | B | Regularization improves generalization |

# Week 9: Assignment 9 — Mathematical Foundations of Machine Learning (CS02)

| Metric | Value |
|--------|-------|
| Due | 2026-03-25, 23:59 IST |
| Submitted | — |
| Score | — |

---

## Question Set 1: MLP and CNN Parameter Counting

### Q1 (1 point)

**An MLP takes an input vector of dimension 256 and has a hidden layer with 64 neurons. Assuming a fully connected layer with bias terms, how many trainable parameters are there in this first layer?**

- [ ] A) 256×64 = 16384
- [x] B) 256×64+64 = 16448
- [ ] C) 256+64 = 320
- [ ] D) 256×64+256 = 16640

**Answer:** B

**Answer Explanation:**

This question tests understanding of **fully connected (dense) layer** parameter counting, fundamental to all neural networks including MLPs and the first layers of CNNs.

**The Calculation:**

In a fully connected layer, every input connects to every neuron:

1. **Weight Matrix:** Each of the 64 neurons receives input from all 256 input dimensions. The weight matrix $W$ has shape $256 \times 64$:
   $$\text{Weights} = 256 \times 64 = 16,384$$

2. **Bias Vector:** Every neuron in the output layer gets exactly one bias term (independent of input dimension):
   $$\text{Biases} = 64$$

3. **Total Trainable Parameters:**
   $$16,384 + 64 = 16,448$$

**Geometric Intuition:**

The weight matrix $W$ is applied as:
$$z = W x + b$$

where:
- $x \in \mathbb{R}^{256}$ is the input vector.
- $W \in \mathbb{R}^{64 \times 256}$ (note: for output, it's actually $\mathbb{R}^{256 \times 64}$ in typical notation; the key is 256 inputs connect to 64 neurons).
- $b \in \mathbb{R}^{64}$ is the bias vector.

**Analysis of Incorrect Options:**

- **A) 256×64 = 16384:** This counts only the weights and forgets the bias terms. Every neural network layer includes biases as trainable parameters.

- **C) 256+64 = 320:** This confuses the counting. It adds inputs to neurons rather than connecting them. There is no "256 + 64" logic in layer parameter counting.

- **D) 256×64+256 = 16640:** This adds a bias for every *input dimension* (256) instead of every *neuron* (64). A common mistake when confusing weight and bias roles. Biases are one per neuron, not per input.

**Key Insight:** The formula for a fully connected layer is always $(\text{inputs} \times \text{neurons}) + \text{neurons}$. Memorizing this pattern helps with all subsequent architecture parameter counting.

**Related Topics:** [MLP Parameter Counting and Deep Learning Fundamentals](notes.md#11-training-mlps-in-pytorch)

---

### Q2 (1 point)

**A CNN layer takes an input image of size 32×32. It uses a convolution with kernel size F=5, stride S=1, and padding P=2. What is the output spatial size?**

- [ ] A) 28×28
- [ ] B) 30×30
- [x] C) 32×32
- [ ] D) 34×34

**Answer:** C

**Answer Explanation:**

This question tests the **CNN spatial dimension formula**, which controls how input sizes map to output sizes through convolution.

**The Formula:**

The spatial output size is:
$$\text{Output Size} = \left\lfloor \frac{\text{Input} + 2 \times \text{Padding} - \text{Kernel Size}}{\text{Stride}} \right\rfloor + 1$$

**Step-by-Step Calculation:**

Given:
- Input: $32 \times 32$
- Kernel size: $F = 5$
- Stride: $S = 1$
- Padding: $P = 2$

$$\text{Output} = \left\lfloor \frac{32 + 2(2) - 5}{1} \right\rfloor + 1 = \left\lfloor \frac{36 - 5}{1} \right\rfloor + 1 = \lfloor 31 \rfloor + 1 = 32$$

**What Padding Does:**

Padding adds zero-valued borders around the input. With $P = 2$:
- Original input: $32 \times 32$
- Padded input: $(32 + 2 \times 2) \times (32 + 2 \times 2) = 36 \times 36$

**What the Kernel Does:**

A $5 \times 5$ kernel removes 4 pixels worth of spatial extent (one direction):
- Padded size: 36
- After applying kernel: $36 - 5 + 1 = 32$

**The "Same" Padding Convention:**

When padding exactly compensates for the kernel size (preserving input spatial dimensions), it's called **"same" padding**. This is common in practice because:
1. Easier to stack layers (sizes don't shrink unexpectedly).
2. Preserves spatial information near boundaries.

**Analysis of Incorrect Options:**

- **A) 28×28:** This is the result of **no padding** ($P = 0$): $\lfloor(32 - 5)/1\rfloor + 1 = 28$. A common mistake is forgetting the padding term.

- **B) 30×30:** This would result from $P = 1$ instead of $P = 2$: $\lfloor(32 + 2 - 5)/1\rfloor + 1 = 30$.

- **D) 34×34:** This would be the result of adding padding incorrectly (without subtracting kernel size): $32 + 2 \times 2 = 36$ is close, but the actual answer involves the formula. A pure arithmetic error.

**Related Topics:** [The Convolution Operation](notes.md#2-the-convolution-operation)

---

### Q3 (1 point)

**A convolutional layer has 32 filters of size 3×3 and the input has 16 channels. How many trainable weights are there in this layer, excluding bias?**

- [ ] A) 32×3×3 = 288
- [ ] B) 16×3×3 = 144
- [x] C) 32×16×3×3 = 4608
- [ ] D) 32×16 = 512

**Answer:** C

**Answer Explanation:**

This question extends parameter counting from fully-connected layers to **convolutional layers**, where the crucial insight is that **filters extend through all input channels**.

**The Concept:**

A convolutional filter is not a 2D array; it's a **3D volume** that spans the entire depth (channel dimension) of the input.

**The Calculation:**

For each filter:
- Spatial dimensions: $3 \times 3$
- Input channels: 16 (the filter must process all 16 channels)
- Weights per filter: $3 \times 3 \times 16 = 144$

Total number of filters: 32

Total weights:
$$32 \times 144 = 32 \times 3 \times 3 \times 16 = 4,608$$

**Intuition:**

Think of the input as a 3D tensor of shape $(H, W, 16)$ where $H$ is height, $W$ is width, and 16 is the number of channels. Each filter slides over the spatial dimensions (H and W) while simultaneously processing all 16 channels.

**With Bias Terms:**

If biases are included, add 1 bias per filter:
$$4,608 + 32 = 4,640 \text{ total parameters}$$

**Comparison to Q1:**

Q1 tested fully-connected layers (2D weight matrices). This question shows that convolutional layers have weights that reflect the **multi-channel input structure**. The key difference is the "$\times C$" factor representing input channels.

**Analysis of Incorrect Options:**

- **A) 32×3×3 = 288:** This counts the spatial dimensions and the number of filters, but **ignores the 16 input channels**. This would only be correct for a single-channel (grayscale) input.

- **B) 16×3×3 = 144:** This is the weight count for **exactly one filter**. It forgets to multiply by the 32 filters.

- **D) 32×16 = 512:** This multiplies the number of filters by the number of input channels but completely ignores the spatial dimensions of the filter $(3 \times 3)$. A fundamental misunderstanding of the filter structure.

**Key Insight:** CNN filters are always 3D (spatial height × spatial width × input channels). The parameter count formula is: $F \times k \times k \times C$, where $F$ is the number of filters, $k \times k$ is the spatial size, and $C$ is the number of input channels.

**Related Topics:** [Filter Parameters in Convolutional Networks](notes.md#filter-parameters)

---

## Question Set 2: Activation Functions and Their Derivatives

### Q4 (1 point)

**In an MLP, a neuron uses sigmoid activation. If its activation is a = 0.8, what is the derivative of the activation with respect to its pre-activation input?**

- [ ] A) 0.8
- [ ] B) 0.2
- [x] C) 0.16
- [ ] D) 0.4

**Answer:** C

**Answer Explanation:**

This question tests the **sigmoid activation function derivative**, a foundational concept for understanding how gradients flow through neural networks during backpropagation.

**The Sigmoid Function:**

$$\sigma(z) = \frac{1}{1 + e^{-z}}$$

Output range: $(0, 1)$ (always between 0 and 1).

**The Derivative (Key Insight):**

The derivative of sigmoid can be expressed entirely in terms of its **output** $a = \sigma(z)$:

$$\sigma'(z) = a(1 - a)$$

This is a beautiful property of the sigmoid function—no need to know the pre-activation $z$; the output alone determines the gradient!

**Proof (optional):**

Using the chain rule:
$$\sigma'(z) = \frac{d}{dz}\left[\frac{1}{1 + e^{-z}}\right] = \frac{e^{-z}}{(1 + e^{-z})^2} = \frac{1}{1 + e^{-z}} \cdot \frac{e^{-z}}{1 + e^{-z}} = \sigma(z) \cdot (1 - \sigma(z)) = a(1 - a)$$

**Example Calculation (Q4):**

Given $a = 0.8$:
$$\sigma'(z) = 0.8 \times (1 - 0.8) = 0.8 \times 0.2 = 0.16$$

**Geometric Intuition:**

The sigmoid curve is steepest (largest derivative) when $a = 0.5$:
$$\max(a(1-a)) = 0.5 \times 0.5 = 0.25 \text{ at } a = 0.5$$

When the output is very close to 0 or 1 (saturated), the derivative is very small, slowing down learning. This is the **saturation problem** of sigmoid activations.

**Analysis of Incorrect Options:**

- **A) 0.8:** This is just the activation value $a$ itself, not its derivative. Common confusion between the neuron's output and its rate of change.

- **B) 0.2:** This is just the $(1 - a)$ portion of the formula, missing the multiplication by $a$.

- **D) 0.4:** This is double the correct answer. It might arise from a calculation error like $0.8 + 0.2 = 1.0$ followed by confusion, or misremembering the maximum derivative as $0.5 \times 0.5 = 0.25$ and then doubling.

**Why This Matters:**

During backpropagation, gradients are multiplied by $\sigma'(z)$ at each sigmoid layer. If many layers use sigmoid, the small gradient values multiply together, leading to **vanishing gradients** (a motivation for using ReLU instead).

**Related Topics:** [Activation Functions and Their Derivatives](notes.md#9-activation-functions-and-their-derivatives)

---

### Q10 (1 point)

**A neuron uses the ReLU activation function: ReLU(z) = max(0, z). What is the derivative of ReLU at a point where z = 3?**

- [ ] A) 0
- [x] B) 1
- [ ] C) 3
- [ ] D) Undefined

**Answer:** B

**Answer Explanation:**

This question tests the **ReLU (Rectified Linear Unit) activation derivative**, which is simpler than sigmoid but has important implications for gradient flow.

**The ReLU Function:**

$$\text{ReLU}(z) = \max(0, z) = \begin{cases} z & \text{if } z > 0 \\ 0 & \text{if } z \leq 0 \end{cases}$$

**The Derivative:**

$$\text{ReLU}'(z) = \begin{cases} 1 & \text{if } z > 0 \\ 0 & \text{if } z < 0 \\ \text{undefined} & \text{if } z = 0 \end{cases}$$

**Why the Derivative is Defined as Above:**

For $z > 0$, ReLU behaves like the identity function $y = z$, which has slope 1.

For $z < 0$, ReLU is constant at 0, which has slope 0.

At exactly $z = 0$, the function has a "kink" (corner), so the derivative is technically undefined. In practice, implementations handle $z = 0$ arbitrarily (e.g., set to 0 or 1).

**Example Calculation (Q10):**

At $z = 3 > 0$:
$$\text{ReLU}'(3) = 1$$

**Key Advantage Over Sigmoid:**

Unlike sigmoid, ReLU has:
- **No saturation:** The derivative is always 0 or 1 (never decays to tiny values).
- **Sparse activation:** For negative inputs, the derivative is 0, which can lead to sparse representations.
- **Computational efficiency:** No exponentials or division needed.

**Gradient Flow in Deep Networks:**

In a deep network with ReLU activations, as long as a neuron is "active" (received positive input), the gradient doesn't decay layer by layer:
$$\frac{\partial L}{\partial z_1} = \frac{\partial L}{\partial z_n} \cdot \text{ReLU}'(z_{n-1}) \cdot \ldots \cdot \text{ReLU}'(z_1) = \frac{\partial L}{\partial z_n} \cdot 1 \cdot \ldots \cdot 1 = \frac{\partial L}{\partial z_n}$$

This is why ReLU became the default activation for modern deep learning.

**Analysis of Incorrect Options:**

- **A) 0:** This would only be true if $z < 0$. Since $z = 3 > 0$, the derivative is 1, not 0.

- **C) 3:** This is the **output value** of ReLU at $z = 3$ (i.e., $\max(0, 3) = 3$), not the derivative. A common confusion between function value and its rate of change.

- **D) Undefined:** The derivative is undefined only at the kink ($z = 0$). At $z = 3$, we are far from the kink, so the derivative is well-defined: 1.

**Related Topics:** [Activation Functions and Their Derivatives](notes.md#9-activation-functions-and-their-derivatives)

---

## Question Set 3: RNN Fundamentals

### Q5 (1 point)

**A standard RNN has input size 12 and hidden size 20. It uses weight matrices W_ih & W_hh, along with one bias vector of size 20. How many trainable parameters does it have?**

- [ ] A) 620
- [x] B) 660
- [ ] C) 640
- [ ] D) 600

**Answer:** B

**Answer Explanation:**

This question tests parameter counting for **Recurrent Neural Networks (RNNs)**, which have a distinct structure due to the **hidden-to-hidden recurrence**.

**The RNN Equations:**

At each time step $t$, the hidden state evolves as:
$$z_t = W_{hh} h_{t-1} + W_{ih} x_t + b$$
$$h_t = \sigma(z_t)$$

**Parameter Breakdown:**

1. **Input-to-Hidden Weights ($W_{ih}$):**
   - Shape: $(\text{input size}) \times (\text{hidden size}) = 12 \times 20$
   - Parameters: $12 \times 20 = 240$

2. **Hidden-to-Hidden Weights ($W_{hh}$):**
   - Shape: $(\text{hidden size}) \times (\text{hidden size}) = 20 \times 20$
   - Parameters: $20 \times 20 = 400$
   - **This is unique to RNNs!** It encodes how the hidden state influences itself at the next time step.

3. **Bias Vector ($b$):**
   - Shape: $(\text{hidden size},) = (20,)$
   - Parameters: $20$
   - One bias per hidden neuron.

**Total:**
$$240 + 400 + 20 = 660$$

**Intuition:**

The RNN has:
- One weight matrix connecting inputs to hidden (like an MLP layer).
- An additional weight matrix connecting the previous hidden state to the current hidden state (the recurrence).
- A shared bias vector.

**Comparison to MLP:**

An MLP with input 12 and one hidden layer of size 20 would have:
$$12 \times 20 + 20 = 260 \text{ parameters}$$

An RNN has 660, which is $2.5 \times$ more due to the $W_{hh}$ term. This extra capacity allows RNNs to maintain and update a "memory" of previous inputs.

**Analysis of Incorrect Options:**

- **A) 620:** Missing 40 parameters. Possibly calculating $12 \times 20 + 20 \times 20 = 240 + 400 = 640$, then subtracting the bias by mistake.

- **C) 640:** This correctly computes $W_{ih} + W_{hh}$ weights: $240 + 400 = 640$, but **forgets to add the bias vector** (20 parameters). A common mistake is treating bias as optional or overlooking it.

- **D) 600:** A round number; likely an error or miscalculation.

**Related Topics:** [The RNN Cell](notes.md#4-recurrent-neural-networks-sequential-processing)

---

## Question Set 4: Gradient Clipping and LSTM

### Q6 (1 point)

**In training an RNN, the gradient vector has norm ‖g‖₂ = 12, and the clipping threshold is c = 5. By what factor is the gradient scaled during gradient clipping?**

- [ ] A) 12/5
- [x] B) 5/12
- [ ] C) 5
- [ ] D) 12

**Answer:** B

**Answer Explanation:**

This question tests **gradient clipping**, a critical technique for stabilizing RNN training when gradients explode during backpropagation through time.

**The Gradient Explosion Problem:**

RNNs can suffer from **exploding gradients** during backpropagation through time (BPTT). When computing gradients over many time steps, the chain rule multiplies many terms:

$$\frac{\partial L}{\partial W} \propto \prod_{k=1}^{T} \frac{\partial h_k}{\partial h_{k-1}}$$

If each factor is $> 1$, the product grows exponentially, leading to huge gradient values that destabilize training (NaN losses, weight divergence).

**Gradient Clipping: The Solution**

Gradient clipping scales the gradient vector to have a maximum norm:

$$g_{\text{clipped}} = g \times \min\left(1, \frac{c}{\|g\|_2}\right)$$

where $c$ is the clipping threshold.

**Equivalently:**

If $\|g\|_2 > c$:
$$\text{Scaling Factor} = \frac{c}{\|g\|_2}$$

**Interpretation:** If the current gradient norm exceeds the threshold, **scale it down** to exactly match the threshold, preserving direction but limiting magnitude.

**Example Calculation (Q6):**

Given:
- Current gradient norm: $\|g\|_2 = 12$
- Clipping threshold: $c = 5$

Since $12 > 5$, clipping applies:
$$\text{Scaling Factor} = \frac{5}{12} \approx 0.4167$$

The gradient is multiplied by $0.4167$, reducing its norm from 12 to 5:
$$\|g_{\text{clipped}}\|_2 = 12 \times \frac{5}{12} = 5 \checkmark$$

**Applied Update:**

Instead of:
$$w_{\text{new}} = w - \eta \times g \quad (\text{norm } 12)$$

We perform:
$$w_{\text{new}} = w - \eta \times \left(g \times \frac{5}{12}\right) \quad (\text{norm } 5)$$

This prevents the destabilizing large update while maintaining the gradient direction.

**Analysis of Incorrect Options:**

- **A) 12/5:** This is the **reciprocal** of the correct factor. Using this would scale the gradient *up* (multiply by $2.4$), making the explosion worse. A sign-error style mistake.

- **C) 5:** This is the clipping threshold itself, not the scaling factor. The factor is the ratio of threshold to norm.

- **D) 12:** This is the current norm, not a scaling factor. It doesn't represent how much to scale.

**Key Insight:** Gradient clipping is a **heuristic** (not theoretically perfect) but empirically very effective. It's standard practice in RNN training, especially for LSTMs and GRUs.

**Related Topics:** [Gradient Clipping](notes.md#6-gradient-clipping)

---

### Q7 (1 point)

**In an LSTM, suppose for one component of the state we have: f_t = 0.6, C_{t-1} = 2, i_t = 0.5, C̃_t = 4. What is the updated cell state value C_t for that component?**

- [ ] A) 2.2
- [x] B) 3.2
- [ ] C) 2.8
- [ ] D) 1.2

**Answer:** B

**Answer Explanation:**

This question tests the **LSTM cell state update**, the core mechanism that allows LSTMs to solve the vanishing gradient problem and learn long-term dependencies.

**The LSTM Cell State Update:**

The LSTM maintains a **separate cell state** $C_t$ (in addition to hidden state $h_t$) that updates additively:

$$C_t = (f_t \odot C_{t-1}) + (i_t \odot \tilde{C}_t)$$

where $\odot$ denotes element-wise multiplication and:
- $f_t$: **Forget gate** (output of sigmoid, in $[0, 1]$) — decides how much of the old state to keep.
- $C_{t-1}$: Previous cell state.
- $i_t$: **Input gate** (output of sigmoid, in $[0, 1]$) — decides how much of the new candidate state to add.
- $\tilde{C}_t$: **Candidate state** (output of tanh) — new information to potentially add.

**Intuition:**

The cell state is a "highway" of information. At each step:
1. **Forget phase:** Multiply old state by forget gate (values between 0 and 1).
2. **Add phase:** Add a weighted version of new candidate state.

This **additive structure** (the $+$ operation) is crucial: it allows gradients to flow backward through time without vanishing, unlike the multiplicative structure of vanilla RNNs.

**Step-by-Step Calculation (Q7):**

Given:
- $f_t = 0.6$
- $C_{t-1} = 2$
- $i_t = 0.5$
- $\tilde{C}_t = 4$

**Forget Phase:**
$$f_t \odot C_{t-1} = 0.6 \times 2 = 1.2$$

The old state is scaled by the forget gate. We keep 60% of the previous state.

**Add Phase:**
$$i_t \odot \tilde{C}_t = 0.5 \times 4 = 2.0$$

We add 50% of the candidate state.

**Total:**
$$C_t = 1.2 + 2.0 = 3.2$$

**Example Interpretation:**

- The old cell state value of 2 is mostly retained (60%).
- The new candidate value of 4 is partially incorporated (50%).
- Result: 3.2 is a balanced combination, leaning slightly toward the new information.

**Why This Works:**

During backprop, the gradient w.r.t. $C_{t-1}$ is:
$$\frac{\partial C_t}{\partial C_{t-1}} = f_t$$

Since $f_t$ is a gate output (value between 0 and 1) and **not a derivative**, it doesn't suffer from squashing. This allows gradients to flow uninterrupted through the cell state over many time steps.

**Analysis of Incorrect Options:**

- **A) 2.2:** An arithmetic error; possibly $(0.6 \times 2) + (0.5 \times 2) = 1.2 + 1.0 = 2.2$. This uses the wrong value for the candidate state.

- **C) 2.8:** An arithmetic error; possibly $(0.6 \times 2) + (0.5 \times 0.4) + \ldots$. Not matching the correct formula.

- **D) 1.2:** This is only the first part of the equation ($f_t \times C_{t-1}$), entirely forgetting to add the new information ($i_t \times \tilde{C}_t$). A critical mistake that ignores half the mechanism.

**Related Topics:** [LSTMs: Solving Vanishing Gradients](notes.md#7-lstms-solving-vanishing-gradients)

---

## Question Set 5: Pooling and Advanced Topics

### Q8 (1 point)

**A feature map of size 28×28 is passed through a max-pooling layer with kernel size 2×2 and stride 2. What is the output size?**

- [x] A) 14×14
- [ ] B) 16×16
- [ ] C) 12×12
- [ ] D) 16×16 (duplicate)

**Answer:** A

**Answer Explanation:**

This question applies the **spatial dimension formula** to max-pooling layers, verifying understanding of how CNNs control spatial resolution.

**The Pooling Spatial Formula:**

Pooling uses the same formula as convolution (but typically with no padding):

$$\text{Output Size} = \left\lfloor \frac{\text{Input Size} - \text{Kernel Size}}{\text{Stride}} \right\rfloor + 1$$

**Step-by-Step Calculation:**

Given:
- Input size: $28 \times 28$
- Kernel size: $2 \times 2$
- Stride: $2$

$$\text{Output} = \left\lfloor \frac{28 - 2}{2} \right\rfloor + 1 = \left\lfloor \frac{26}{2} \right\rfloor + 1 = 13 + 1 = 14$$

So the output is $14 \times 14$.

**How Max Pooling Works:**

Max-pooling divides the input feature map into non-overlapping windows (determined by kernel size and stride) and outputs the maximum value in each window.

**Example (First Window):**

The top-left $2 \times 2$ window of the $28 \times 28$ input contains 4 values. Max-pooling outputs the maximum of these 4 values.

**Why Stride 2 and Kernel 2×2?**

When stride equals kernel size, the windows are non-overlapping and tile the entire input perfectly:
- Number of windows in each dimension: $28 / 2 = 14$
- Output spatial size: $14 \times 14$

This is a common pattern: $2 \times 2$ max-pooling with stride 2 **reduces spatial dimensions by half**.

**Benefits of Pooling:**

1. **Computational reduction:** Fewer activations in the next layer.
2. **Translation robustness:** Small shifts in input don't change the maximum value in a pooling region.
3. **Feature extraction:** Captures the most salient features (for max-pooling).

**Analysis of Incorrect Options:**

- **B) 16×16 and D) 16×16:** These would result from a stride of 1.75 or miscalculation. Let's check: if stride were 1: $\lfloor(28-2)/1\rfloor + 1 = 27$. If stride were 1.5 (not typical): $\lfloor(28-2)/1.5\rfloor + 1 \approx 18$. Neither gives 16. The value 16 doesn't arise naturally from this setup.

- **C) 12×12:** This would be the result of a kernel size of $4 \times 4$ with stride 2: $\lfloor(28-4)/2\rfloor + 1 = 13$ (close but not exact). A miscalculation of the formula.

**Related Topics:** [Pooling Operations](notes.md#3-pooling-operations)

---

### Q9 (1 point)

**In a residual block, the output is given by: y = x + F(x). If dL/dy = δ, which expression correctly gives dL/dx?**

- [ ] A) δ
- [ ] B) δF′(x)
- [x] C) δ(1+F′(x))
- [ ] D) 1+F′(x)

**Answer:** C

**Answer Explanation:**

This question tests the **chain rule in residual networks**, which is fundamental to understanding why skip connections prevent vanishing gradients.

**The Residual Block:**

A residual block has the form:
$$y = x + F(x)$$

where:
- $x$ is the input (passed through a skip connection).
- $F(x)$ is some non-linear transformation (e.g., convolutional or fully-connected layers).
- The **addition** combines the input and the transformation.

**Computing dL/dx Using the Chain Rule:**

We want to find how the loss $L$ changes with respect to the input $x$:

$$\frac{dL}{dx} = \frac{dL}{dy} \cdot \frac{dy}{dx}$$

We're given $\frac{dL}{dy} = \delta$, so:

$$\frac{dL}{dx} = \delta \cdot \frac{dy}{dx}$$

**Computing dy/dx:**

From $y = x + F(x)$:

$$\frac{dy}{dx} = \frac{d}{dx}[x + F(x)] = 1 + \frac{dF}{dx} = 1 + F'(x)$$

**Final Result:**

$$\frac{dL}{dx} = \delta (1 + F'(x))$$

**Gradient Flow Analysis:**

The key insight is the **additive term 1**:

$$\frac{dL}{dx} = \delta \cdot 1 + \delta \cdot F'(x) = \underbrace{\delta}_{\text{skip path}} + \underbrace{\delta F'(x)}_{\text{residual path}}$$

**Two paths:**
1. **Skip path:** Gradient flows directly through the addition: $\delta$. This is unaffected by the depth or complexity of $F$.
2. **Residual path:** Gradient flows through the learned transformation: $\delta F'(x)$. If $F'(x)$ is small (as in very deep networks), this term diminishes, but the skip path provides a "backup" route.

**Why This Prevents Vanishing Gradients:**

In a vanilla deep network (without skip connections), if $F'(x)$ is small at many layers, the gradient product becomes exponentially small. But with skip connections, the $+1$ term ensures that **at least some gradient magnitude is preserved** through the skip path.

**Example (Intuition):**

Suppose $F'(x) = 0.1$ (small). In a vanilla network:
$$\frac{dL}{dx} = \delta \cdot 0.1 = \text{small}$$

In a residual network:
$$\frac{dL}{dx} = \delta \cdot (1 + 0.1) = \delta \cdot 1.1 \approx \delta$$

The gradient magnitude is preserved! This is why ResNets can be very deep without vanishing gradient issues.

**Analysis of Incorrect Options:**

- **A) δ:** This would only be true if $F(x) = 0$ (no learned transformation). It ignores the gradient flowing through the residual path.

- **B) δF′(x):** This would be correct if the output were $y = F(x)$ (without the skip). It completely ignores the gradient flowing through the skip connection (the 1 term).

- **D) 1+F′(x):** This is $dy/dx$ (the local gradient), but it's missing the multiplication by $\delta$ (the upstream gradient from the loss). Backprop requires multiplying upstream and local gradients.

**Related Topics:** [Residual Networks (ResNets)](notes.md#8-grus-and-residual-networks)

---

## Answer Key Summary

| Q | Answer | Concept | Difficulty |
|---|--------|---------|-----------|
| Q1 | B | MLP fully-connected parameter counting | Easy |
| Q2 | C | CNN spatial dimension formula ("same" padding) | Easy |
| Q3 | C | CNN filter weight counting (multi-channel) | Medium |
| Q4 | C | Sigmoid activation derivative ($a(1-a)$) | Easy |
| Q5 | B | RNN parameter counting (W_ih + W_hh + bias) | Medium |
| Q6 | B | Gradient clipping scaling factor ($c/\|g\|_2$) | Medium |
| Q7 | B | LSTM cell state update (additive gates) | Medium |
| Q8 | A | Max-pooling spatial size reduction | Easy |
| Q9 | C | Residual block gradient flow (chain rule) | Medium |
| Q10 | B | ReLU activation derivative (1 for z > 0) | Easy |

**Overall Score:** 10/10 points possible

---

## Concept Map: Week 9 Progression

**From Parameter Counting to Deep Learning Architecture:**

1. **Fully-Connected Layers (Q1):**
   - Foundation: Every input connects to every neuron.
   - Parameters: $(\text{inputs} \times \text{neurons}) + \text{neurons}$ (weights + biases).
   - Building block for MLPs and the first/last layers of CNNs.

2. **Convolutional Layers (Q2, Q3):**
   - Spatial structure: Local receptive fields and parameter sharing.
   - Spatial formula: Output size depends on input, kernel, stride, padding.
   - Weight counting: Must account for multi-channel inputs ($F \times k \times k \times C$).
   - **Hard regularization** reduces parameters compared to fully-connected.

3. **Pooling (Q8):**
   - Downsampling operation using the same spatial formula.
   - Max-pooling extracts most salient features and provides translation robustness.
   - Stride matching kernel size halves spatial dimensions.

4. **Activation Functions (Q4, Q10):**
   - Sigmoid: Smooth but can saturate, small derivatives.
   - ReLU: Linear for positive inputs, gradient of 1 or 0 (no saturation).
   - Derivative properties affect how gradients flow during backprop.

5. **Recurrent Neural Networks (Q5, Q6, Q7):**
   - Sequential processing via hidden state recurrence.
   - Parameters: Input-to-hidden ($W_{ih}$) + hidden-to-hidden ($W_{hh}$) + bias.
   - Vanishing gradients: Multiplicative structure over time steps.
   - Gradient clipping: Heuristic fix for exploding gradients.

6. **LSTMs (Q7):**
   - Additive cell state update: $C_t = f_t \odot C_{t-1} + i_t \odot \tilde{C}_t$.
   - Gates control information flow without severe squashing.
   - Gradient flow: Uninterrupted through the cell state via the additive update.

7. **Residual Networks (Q9):**
   - Skip connections: $y = x + F(x)$.
   - Gradient flow: Two paths—skip path (always contributes) and residual path (may be small).
   - Enables very deep networks without vanishing gradients.

**Week 9 Unifying Theme:**

Every concept—parameter counting, spatial formulas, activation derivatives, RNN recurrence, LSTM gates, residual connections—is ultimately about **understanding what happens during forward and backward passes**, ensuring that information and gradients flow effectively through the network. This is the foundation for modern deep learning success.


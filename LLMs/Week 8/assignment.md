# Week 8: Assignment 8 — Introduction to Large Language Models (LLMs) (noc26_cs88)

> **Due:** 2026-03-18, 23:59 IST
> **Submitted:** —
> **Score:** —

---

## Section 01 - Instruction Tuning

### Q1 (1 point, multiple select)
**Which factors influence the effectiveness of instruction tuning? (Select all that apply)**

- [ ] A) The number of instruction templates used in training
- [ ] B) The tokenization algorithm used by the model
- [ ] C) The diversity of tasks in the fine-tuning dataset
- [ ] D) The order in which tasks are presented during fine-tuning

**Answer:** A, B, C

**Answer Explanation**: Template variety helps the model generalize across different ways users phrase the same intent, tokenization affects how instructions are represented internally, and task diversity teaches broad instruction-following behavior across domains. The task order can matter in some curricula, but the accepted answer set focuses on templates, tokenization, and diversity.

**Related Topic**: [What affects instruction tuning effectiveness](notes.md#what-affects-instruction-tuning-effectiveness)

---

## Section 02 - Prompt-based Learning

### Q2 (1 point, multiple select)
**What are key challenges of soft prompts in prompt-based learning? (Select all that apply)**

- [ ] A) Forward pass with them is computationally inefficient compared to that with hard prompts
- [ ] B) They require additional training, unlike discrete prompts
- [ ] C) They cannot be interpreted or used effectively by non-expert users
- [ ] D) They require specialized architectures that differ from standard transformers

**Answer:** A, B

**Answer Explanation**: Soft prompts are learned continuous vectors, so you typically need optimization/training to obtain them. They can also increase compute by adding additional “virtual prompt tokens,” increasing the effective sequence length (and thus attention cost) compared to a short hard prompt. They do not require a fundamentally different architecture.

**Related Topic**: [Hard vs soft prompts](notes.md#hard-vs-soft-prompts)

---

### Q3 (1 point)
**Which statement best describes the impact of fine-tuning versus prompting in LLMs?**

- [ ] A) Fine-tuning is always superior to prompting in generalization tasks
- [ ] B) Prompting requires gradient updates, while fine-tuning does not
- [ ] C) Fine-tuning modifies the model weights permanently, while prompting does not
- [ ] D) Prompting performs better on in-domain tasks compared to fine-tuning

**Answer:** C

**Answer Explanation**: Fine-tuning updates parameters (persistent behavior change). Prompting changes only the input context; model weights stay fixed.

**Related Topic**: [Fine-tuning vs prompting (what changes)](notes.md#fine-tuning-vs-prompting-what-changes)

---

## Section 03 - Advanced Prompting and Prompt Sensitivity

### Q4 (1 point, multiple select)
**Which of the following aspects of model outputs are captured by POSIX? (Select all that apply)**

- [ ] A) Diversity in the responses to intent-preserving prompt variations
- [ ] B) Entropy of the distribution of response frequencies
- [ ] C) Time required to generate responses for intent-preserving prompt variations
- [ ] D) Variance in the log-likelihood of the same response for different input prompt variations

**Answer:** A, B, D

**Answer Explanation**: POSIX focuses on stability under intent-preserving prompt changes (diversity), how spread out responses are (entropy), and confidence instability (variance in log-likelihood). It does not target generation latency.

**Related Topic**: [Prompt sensitivity and POSIX](notes.md#prompt-sensitivity-and-posix)

---

### Q5 (1 point)
**Which key mechanism makes Tree-of-Thought (ToT) prompting more effective than Chain-of-Thought (CoT)?**

- [ ] A) ToT uses reinforcement learning for better generalization
- [ ] B) ToT allows backtracking to explore multiple reasoning paths
- [ ] C) ToT reduces hallucination by using domain-specific heuristics
- [ ] D) ToT eliminates the need for manual prompt engineering

**Answer:** B

**Answer Explanation**: ToT explores multiple branches and can backtrack from weak branches, unlike single-chain CoT which commits to one path.

**Related Topic**: [Tree-of-Thoughts (ToT)](notes.md#tree-of-thoughts-tot)

---

### Q6 (1 point)
**What is a key limitation of measuring accuracy alone when evaluating LLMs?**

- [ ] A) Accuracy is always correlated with model size
- [ ] B) Accuracy cannot be measured on open-ended tasks
- [ ] C) Accuracy is independent of the training dataset size
- [ ] D) Accuracy does not account for prompt sensitivity

**Answer:** D

**Answer Explanation**: A model can score well on one prompt but fail on a paraphrase with identical intent; accuracy alone misses this prompt sensitivity.

**Related Topic**: [Prompt sensitivity and POSIX](notes.md#prompt-sensitivity-and-posix)

---

## Section 04 - Alignment and RLHF

### Q7 (1 point)
**Why is instruction tuning not sufficient for aligning large language models?**

- [ ] A) It does not generalize to unseen tasks
- [ ] B) It cannot prevent models from generating undesired responses
- [ ] C) It reduces model performance on downstream tasks
- [ ] D) It makes models less capable of learning from new data

**Answer:** B

**Answer Explanation**: Instruction tuning improves obedience/helpfulness but does not guarantee refusal of unsafe/undesired requests; alignment requires additional objectives and safety constraints.

**Related Topic**: [Alignment: why instruction tuning is not enough](notes.md#alignment-why-instruction-tuning-is-not-enough)

---

### Q8 (1 point)
**Why is KL divergence minimized in regularized reward maximization?**

- [ ] A) To maximize the probability of generating high-reward responses
- [ ] B) To make training more computationally efficient
- [ ] C) To prevent the amplification of bias in training data
- [ ] D) To ensure models do not diverge too far from the reference model

**Answer:** D

**Answer Explanation**: The KL term penalizes drifting away from a reference policy, keeping the aligned model close to the original model’s distribution and preventing extreme, unnatural behavior.

**Related Topic**: [KL-regularized reward maximization](notes.md#kl-regularized-reward-maximization)

---

### Q9 (1 point)
**What is the primary advantage of using the log-derivative trick in REINFORCE?**

- [ ] A) Reducing data requirements
- [ ] B) Expanding the token vocabulary
- [ ] C) Simplifying gradient computation
- [ ] D) Improving sampling diversity

**Answer:** C

**Answer Explanation**: The log-derivative trick rewrites gradients of expectations into an expectation involving $\nabla\log\pi$, which is tractable to estimate from samples.

**Related Topic**: [REINFORCE and the log-derivative trick](notes.md#reinforce-and-the-log-derivative-trick)

---

### Q10 (1 point)
**Which method combines reward maximization and minimizing KL divergence?**

- [ ] A) REINFORCE
- [ ] B) Monte Carlo Approximation
- [ ] C) Proximal Policy Optimization
- [ ] D) Constitutional AI

**Answer:** C

**Answer Explanation**: PPO is widely used in RLHF to implement stable policy updates that optimize reward while controlling divergence from a reference model (often via KL-style regularization).

**Related Topic**: [PPO](notes.md#ppo)

---

## Answer Key Summary

| Q | Answer | Concept |
|:--:|:--:|:---|
| 1 | A, B, C | Instruction tuning effectiveness factors |
| 2 | A, B | Soft prompt challenges |
| 3 | C | Fine-tuning updates weights; prompting doesn’t |
| 4 | A, B, D | POSIX measures prompt sensitivity |
| 5 | B | ToT backtracking + branching |
| 6 | D | Accuracy ignores prompt sensitivity |
| 7 | B | Instruction tuning ≠ alignment |
| 8 | D | KL keeps policy close to reference |
| 9 | C | Log-derivative trick simplifies gradients |
| 10 | C | PPO used for KL-regularized RLHF |

# Week 8: Theoretical Notes — Instruction Tuning, Prompting, Prompt Sensitivity, and Alignment (RLHF)

## Overview
Week 8 explains how we move from a base next-token predictor to an assistant that follows instructions, reasons more reliably, and avoids undesired behavior. We cover **instruction tuning** (including FLAN and Self-Instruct), **prompt-based learning** (zero/one/few-shot, hard vs soft prompts), advanced prompting methods (CoT, self-consistency, ToT/GoT), and **alignment** via RLHF (reward modeling, KL-regularized optimization, REINFORCE, PPO) and **Constitutional AI**.

---

## Why base LMs don’t automatically follow instructions
A pre-trained language model is optimized for **next-token prediction**, not for “do what the user asked.” So even if it knows facts, it may fail to:
- follow formatting constraints
- map labels correctly (e.g., output `positive` vs `1`)
- refuse unsafe or undesired instructions

This motivates instruction tuning and alignment.

**Related Assignment Questions**: Q7.

---

## Instruction tuning
**Instruction tuning** fine-tunes a model on many tasks formatted as natural-language instructions.

A common data format is an (instruction, input, output) triple:
- instruction: what to do
- input: the instance
- output: the target response

The key idea is to teach the model the *pattern* of following directions across tasks.

### What affects instruction tuning effectiveness
Practical factors include:
- **template variety** (many ways to phrase the same task)
- **task diversity** (many different task types)
- **tokenization** (how text is split into tokens can change how instructions are represented)

**Related Assignment Questions**: Q1.

### Multitask learning vs instruction tuning (intuition)
- **multitask learning**: train on multiple tasks, often still in task-specific formats
- **instruction tuning**: rewrite tasks into a uniform “instruction → response” interface so the model generalizes better to new instruction styles

---

## FLAN and Self-Instruct
### FLAN
**FLAN** is an instruction-tuning style dataset built by taking many existing NLP tasks and writing instruction templates for them (in the scrape: 1,836 tasks).

### Self-Instruct
**Self-Instruct** automates instruction dataset creation by using a stronger model to generate instruction-following examples. Typical pipelines include:
- generate candidate instructions and outputs
- filter low-quality or near-duplicate items (e.g., heuristics using ROUGE-L overlap)

---

## Prompt-based learning: pre-train → prompt → predict
Large models made full fine-tuning expensive. **Prompting** keeps model weights fixed and uses a prompt to elicit the desired behavior.

### Zero-shot, one-shot, few-shot
- **zero-shot**: only instructions, no examples
- **one-shot**: one example demonstration
- **few-shot**: a handful of examples to establish pattern/format

### Cloze vs prefix prompts
- **cloze prompts**: fill-in-the-blank style
- **prefix prompts**: continuation style (model completes the text)

### Hard vs soft prompts
- **hard prompts**: discrete natural-language tokens a human can read/edit
- **soft prompts**: continuous learned vectors (prompt embeddings) prepended to the model input

Soft prompts often require additional training to learn the prompt vectors, even if the base model weights remain frozen.

**Related Assignment Questions**: Q2.

---

## Fine-tuning vs prompting (what changes)
- **fine-tuning** updates model parameters; behavior changes persist
- **prompting** does not update parameters; behavior changes only via the input prompt

This distinction is central to “prompting instead of fine-tuning.”

**Related Assignment Questions**: Q3.

---

## Advanced prompting
### Chain-of-Thought (CoT)
**CoT prompting** encourages a model to produce intermediate reasoning steps, often improving accuracy on multi-step tasks.

### Self-consistency
**Self-consistency** runs CoT multiple times and uses a selection rule (often majority vote) over final answers.

### Tree-of-Thoughts (ToT)
**ToT** generalizes CoT by exploring multiple reasoning branches and allowing scoring and **backtracking** when a branch looks unpromising.

**Related Assignment Questions**: Q5.

### Graph-of-Thoughts (GoT)
**GoT** extends ToT-style reasoning to graph structures with modular generation and evaluation components.

---

## Prompt sensitivity and POSIX
Even with identical intent, small prompt wording changes can produce different answers. **Prompt sensitivity** measures this fragility.

**POSIX** (prompt sensitivity index) captures (as described in the scrape):
- **diversity** of responses to intent-preserving prompt variations
- **entropy** of the response-frequency distribution
- **semantic coherence** across responses
- **variance in log-likelihood confidence** across prompt variations

So accuracy alone can be misleading: you also want to know whether the model is stable under minor prompt changes.

**Related Assignment Questions**: Q4, Q6.

---

## Alignment: why instruction tuning is not enough
Instruction tuning can make a model more helpful, but it may still generate **undesired or harmful responses** if asked.

Alignment methods attempt to optimize behavior beyond pure instruction following.

**Related Assignment Questions**: Q7.

---

## RLHF: reward modeling and regularized optimization
**RLHF (Reinforcement Learning from Human Feedback)** is a common alignment pipeline:

1. **Collect preference data**: humans choose which response is better among candidates.
2. **Train a reward model** $R_\phi(x,y)$ to predict human preference.
3. **Optimize the policy model** to maximize reward, while staying close to a reference model.

### KL-regularized reward maximization
A standard objective encourages high reward but penalizes drifting too far from a reference model $\pi_{ref}$.

One common form (high-level) is:

$$\max_{\pi} \; \mathbb{E}_{y \sim \pi(\cdot\mid x)}\big[ R_\phi(x,y) \big] - \beta\, \mathrm{KL}\big(\pi(\cdot\mid x)\;\|\;\pi_{ref}(\cdot\mid x)\big)$$

Minimizing KL helps keep outputs fluent and prevents extreme divergence.

**Related Assignment Questions**: Q8, Q10.

---

## REINFORCE and the log-derivative trick
In policy-gradient RL, you optimize an expected reward by differentiating through a sampling distribution. The **log-derivative trick** rewrites gradients into a convenient form.

If $J(\theta)=\mathbb{E}_{y\sim \pi_\theta}[R(y)]$, then:

$$\nabla_\theta J(\theta)=\mathbb{E}_{y\sim \pi_\theta}\big[R(y)\,\nabla_\theta \log \pi_\theta(y)\big]$$

This avoids differentiating through the sampling operation directly and simplifies gradient computation.

**Related Assignment Questions**: Q9.

---

## PPO and Constitutional AI
### PPO
**PPO (Proximal Policy Optimization)** is a practical algorithm often used in RLHF to stabilize updates while optimizing reward and controlling divergence (commonly via KL-style regularization).

**Related Assignment Questions**: Q10.

### Constitutional AI
**Constitutional AI (CAI)** uses a set of written principles (“a constitution”) to guide critique and revision, reducing reliance on direct human preference labels.

---

## Key Takeaways

| Concept | Definition | Example |
|:---|:---|:---|
| Instruction tuning | Fine-tune on instruction-formatted multitask data | (instruction, input, output) triples |
| Template diversity | Many paraphrases of the same task prompt | “Summarize” vs “TL;DR” |
| FLAN | Large multitask instruction dataset (scrape: 1,836 tasks) | Many NLP tasks rewritten as instructions |
| Self-Instruct | Auto-generate instruction data + filter | ROUGE-L based filtering heuristics |
| Prompting | Pre-train → prompt → predict without weight updates | Few-shot exemplars in prompt |
| Soft prompts | Learned continuous prompt vectors | Train prompt embeddings with frozen LM |
| ToT | Branching + scoring + backtracking reasoning | Explore multiple solution paths |
| POSIX | Prompt sensitivity metric (diversity/entropy/variance) | Same intent, different wordings |
| RLHF objective | Reward maximization with KL penalty | Stay close to reference model |
| Log-derivative trick | Simplifies policy-gradient computation | $\nabla \log \pi$ form |

---

## Summary
- Instruction tuning improves instruction following via diverse task and template data.
- Prompt-based learning often replaces fine-tuning when models are large and expensive to update.
- Advanced prompting (CoT, self-consistency, ToT/GoT) can improve reasoning without retraining.
- POSIX-style metrics highlight prompt sensitivity beyond accuracy.
- Alignment methods like RLHF optimize helpfulness/safety using reward models and KL-regularization; PPO is commonly used for stable updates.

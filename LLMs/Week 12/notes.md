# Week 12: Theoretical Notes — Responsible LLMs (Bias, Fairness, Robustness, Safety)

## Overview
This final week focuses on deploying LLMs responsibly. We study four axes of **Responsible LLMs**: **Explainability** (understanding and intervening in model behavior), **Fairness** (measuring and mitigating bias), **Robustness** (stability under prompt variations and attacks), and **Safety/Security** (defending against adversarial and prompt-injection style misuse). We also cover bias measurement using **StereoSet** metrics (LMS, SS, ICAT) and two mitigation strategies: **adversarial triggers** and **in-context learning (ICL) safety demonstrations**.

---

## Responsible LLMs: the four axes
### Explainability
**Explainability** aims to understand why the model produced an output and which internal components were responsible.

Examples of explainability tools (high-level):
- interpretability / mechanistic analysis
- inference-time interventions (e.g., editing or patching activations)

### Fairness
**Fairness** focuses on identifying, quantifying, and mitigating bias across demographic groups.

### Robustness
**Robustness** asks whether small changes to a prompt or input distribution cause unstable or unsafe behavior.

### Safety and security
**Safety/Security** covers defenses against harmful queries and adversarial attacks (e.g., prompt injection), with attention to white-box vs black-box threat models.

**Related Assignment Questions**: Q8.

---

## Bias in LLMs: what it means and why it matters
In this context, **bias** refers to systematic tendencies that can:
- produce stereotypical, objectionable, or unfair outputs
- cause harmful real-world impacts when LLMs are used in decision-making systems

Bias is typically not “intentionally inserted” by curators; it often arises from the data and training setup.

**Related Assignment Questions**: Q1.

---

## Prominent sources of bias
Common sources highlighted in the lecture:
- **skewed training distributions**: over-representation of some topics/groups
- **temporal bias**: meanings and social norms drift over time, so older data can embed outdated associations
- **cultural and linguistic bias**: high-resource languages dominate data, reinforcing a cycle where low-resource language performance lags

**Related Assignment Questions**: Q3, Q7, Q10.

---

## StereoSet and bias metrics
StereoSet is a benchmark for measuring stereotypical associations in language models.

### Language Modeling Score (LMS)
**LMS** measures whether the model prefers meaningful/coherent continuations.

### Stereotype Score (SS)
**SS** is the proportion of examples where the model prefers a **stereotypical** association over an **anti-stereotypical** association.

**Related Assignment Questions**: Q2.

### ICAT score
**ICAT** is designed as a balanced metric that captures both:
- language modeling ability (LMS)
- tendency to avoid stereotypical bias (keeping SS near a neutral midpoint)

**Related Assignment Questions**: Q9.

---

## Bias mitigation via adversarial triggers
**Adversarial triggers** are specially chosen tokens prepended to a prompt to exploit a model’s distributional patterns and reduce or flip biased associations at inference time.

High-level workflow:
- search for trigger tokens that optimize an objective tied to bias reduction
- prepend the tokens during generation (without updating model weights)

**Related Assignment Questions**: Q4.

---

## Regard metric
The **regard** metric is a classifier-derived label reflecting the attitude/stance toward a demographic group in generated text.

It is often used to quantify whether mitigations change outputs toward more respectful, less harmful associations.

**Related Assignment Questions**: Q5.

---

## Improving safety with ICL safety demonstrations
A practical approach for safer responses uses **in-context learning** with retrieved demonstrations:
- retrieve top-K similar safety demonstrations for a query (e.g., via sentence transformers with cosine similarity or BM25)
- prepend those demonstrations to the prompt to guide the model’s behavior during generation

This is a retrieval-augmented prompting strategy; it does not require fine-tuning.

**Related Assignment Questions**: Q6.

---

## Course conclusion: expert panel discussion
The final session is a concluding panel reviewing themes across the course:
- architectures and training
- fine-tuning and PEFT
- prompting and alignment
- knowledge graphs and retrieval
- interpretability
- responsible AI and open problems

---

## Key Takeaways

| Concept | Definition | Example |
|:---|:---|:---|
| Explainability | Understand and intervene in model behavior | interpretability + interventions |
| Fairness | Measure/mitigate demographic bias | StereoSet-based evaluation |
| Robustness | Stability under prompt variations/attacks | prompt sensitivity testing |
| Safety/Security | Defend against harmful use and attacks | prompt-injection awareness |
| SS | Prefer stereotype vs anti-stereotype | stereotypical choice rate |
| ICAT | Balanced bias + capability metric | high LMS + SS near neutral |
| Adversarial triggers | Prepend tokens to neutralize bias | trigger-token search |
| Regard | Attitude label toward demographic entity | classifier output |
| ICL safety demos | Retrieve + prepend safe examples | top-K demos in prompt |

---

## Summary
- Responsible LLMs are evaluated across explainability, fairness, robustness, and safety/security.
- Bias arises from skewed data, temporal drift, and cultural/linguistic imbalance; it can cause harmful outputs and real-world impacts.
- StereoSet provides LMS, SS, and ICAT to measure bias while preserving language modeling quality.
- Mitigation strategies include adversarial triggers and retrieval-based ICL safety demonstrations.

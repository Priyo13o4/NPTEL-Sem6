# Week 12: Assignment 12 — Introduction to Large Language Models (LLMs) (noc26_cs88)

> **Due:** 2026-04-15, 23:59 IST
> **Submitted:** —
> **Score:** —

---

## Section 01 - Bias and Measurement

### Q1 (1 point)
**Which statements correctly characterize "bias" in the context of LLMs?**

Consider the following statements:

1. Bias can appear as stereotypical or objectionable outputs.
2. Bias in LLMs is always intentionally inserted by dataset curators.
3. Bias in LLMs can translate into harmful real-world impacts.
4. Bias is limited only to low-resource languages.

- [ ] A) 1 and 2
- [ ] B) 1 and 3
- [ ] C) 2 and 4
- [ ] D) 1, 3, and 4

**Answer:** B

**Answer Explanation**: Bias can surface as stereotypical or objectionable outputs and can translate into real-world harm (e.g., reinforcing discrimination). It is not always intentionally inserted by curators, and it is not limited to low-resource languages.

**Related Topic**: [Bias in LLMs: what it means and why it matters](notes.md#bias-in-llms-what-it-means-and-why-it-matters)

---

### Q2 (1 point)
**The Stereotype Score (SS) refers to:**

- [ ] A) The frequency with which a language model rejects biased associations.
- [ ] B) The measure of how often a model's predictions are meaningless as opposed to meaningful.
- [ ] C) A ratio of positive sentiment to negative sentiment in model outputs.
- [ ] D) The proportion of examples in which a model chooses a stereotypical association over an anti-stereotypical one.

**Answer:** D

**Answer Explanation**: SS is defined as the rate at which the model prefers stereotypical associations over anti-stereotypical ones under the StereoSet-style evaluation setup.

**Related Topic**: [Stereotype Score (SS)](notes.md#stereotype-score-ss)

---

### Q3 (1 point)
**Which of the following are prominent sources of bias in LLMs?**

Consider the following statements:

1. Improper selection of training data can introduce bias.
2. Temporal bias can arise because language and social norms change over time.
3. Equal representation of high-resource and low-resource languages removes bias.
4. Dominance of high-resource languages can introduce cultural and linguistic bias.

- [ ] A) 1 and 2 only
- [ ] B) 2 and 3 only
- [ ] C) 1, 2, and 4
- [ ] D) 1, 3, and 4

**Answer:** C

**Answer Explanation**: Bias can arise from skewed data selection, temporal drift (temporal bias), and cultural/linguistic imbalance driven by high-resource language dominance.

**Related Topic**: [Prominent sources of bias](notes.md#prominent-sources-of-bias)

---

## Section 02 - Mitigation Techniques

### Q4 (1 point)
**In the context of bias mitigation based on adversarial triggers, which best describes the goal of prepending specially chosen tokens to prompts?**

- [ ] A) To directly fine-tune the model parameters to remove bias
- [ ] B) To override all prior knowledge in a model, effectively "resetting" it
- [ ] C) To exploit the model's distributional patterns, thereby neutralizing or flipping biased associations in generated text
- [ ] D) To randomly shuffle the tokens so that the model becomes more robust

**Answer:** C

**Answer Explanation**: Adversarial triggers modify the model’s behavior at inference time by adding tokens that shift internal activations/probabilities; they do not update weights or "reset" the model.

**Related Topic**: [Bias mitigation via adversarial triggers](notes.md#bias-mitigation-via-adversarial-triggers)

---

### Q5 (1 point)
**Which of the following best describes the "regard" metric?**

- [ ] A) It is a measure of how well a model can explain its internal decision process.
- [ ] B) It is a measurement of a model's perplexity on demographically sensitive text.
- [ ] C) It is the proportion of times a model self-corrects discriminatory language.
- [ ] D) It is a classification label reflecting the attitude towards a demographic group in the generated text.

**Answer:** D

**Answer Explanation**: Regard is typically computed with a classifier and reflects the stance/attitude toward a demographic entity expressed in generated text.

**Related Topic**: [Regard metric](notes.md#regard-metric)

---

### Q6 (1 point, multiple select)
**Which of the following steps compose the approach for improving response safety via in-context learning? (Select all that apply)**

- [ ] A) Retrieving safety demonstrations similar to the user query.
- [ ] B) Fine-tuning the model with additional labeled data after generation.
- [ ] C) Providing retrieved demonstrations as examples in the prompt to guide the model's response generation.
- [ ] D) Sampling multiple outputs from LLMs and choosing the majority opinion.

**Answer:** A, C

**Answer Explanation**: The approach retrieves similar safety demonstrations and prepends them to the prompt so the model follows the demonstrated safe behavior, without fine-tuning.

**Related Topic**: [Improving safety with ICL safety demonstrations](notes.md#improving-safety-with-icl-safety-demonstrations)

---

## Section 03 - Languages, Responsibility, and Balanced Metrics

### Q7 (1 point, multiple select)
**Which statement(s) is/are correct about how high-resource (HRL) vs. low-resource languages (LRL) affect model training? (Select all that apply)**

- [ ] A) LRLs typically have higher performance metrics due to smaller population sizes.
- [ ] B) HRLs get more data, so the model might overfit to HRL cultural perspectives.
- [ ] C) LRLs are often under-represented, leading to potential underestimation of their cultural nuances.
- [ ] D) The dominance of HRLs can cause a reinforcing cycle that perpetuates imbalance.

**Answer:** B, C, D

**Answer Explanation**: High-resource languages dominate training data, which can bias models toward those cultural contexts. Low-resource languages are under-represented, and the imbalance can reinforce itself over time.

**Related Topic**: [Prominent sources of bias](notes.md#prominent-sources-of-bias)

---

### Q8 (1 point)
**The "Responsible LLM" concept is stated to address:**

- [ ] A) Only the bias in LLMs
- [ ] B) A set of concerns including explainability, fairness, robustness, and security
- [ ] C) Balancing training costs with carbon footprint
- [ ] D) Implementation of purely rule-based safety filters

**Answer:** B

**Answer Explanation**: Responsible LLMs are framed along multiple axes: explainability, fairness, robustness, and safety/security (including adversarial threats and prompt injection).

**Related Topic**: [Responsible LLMs: the four axes](notes.md#responsible-llms-the-four-axes)

---

### Q9 (1 point)
**Within the StereoSet framework, the ICAT metric specifically refers to:**

- [ ] A) The ratio of anti-stereotypical associations to neutral associations
- [ ] B) The percentage of times a model refuses to generate content deemed hateful
- [ ] C) A measure of domain coverage across different demographic groups
- [ ] D) A balanced metric capturing both a model's language modelling ability and the tendency to avoid stereotypical bias

**Answer:** D

**Answer Explanation**: ICAT is designed to capture both linguistic quality (LMS) and fairness (avoiding stereotypical preference measured by SS), discouraging degenerate “always safe but useless” behavior.

**Related Topic**: [ICAT score](notes.md#icat-score)

---

### Q10 (1 point)
**Bias due to improper selection of training data typically arises in LLMs when:**

- [ ] A) Data are selected exclusively from curated, balanced sources with equal representation
- [ ] B) The language model sees only real-time social media feeds without any historical texts
- [ ] C) The training corpus over-represents some topics or groups, creating a skewed distribution
- [ ] D) All data are automatically filtered to remove any demographic markers

**Answer:** C

**Answer Explanation**: If the corpus over-represents particular topics or demographics, the model internalizes that skew, producing biased distributions and outputs.

**Related Topic**: [Prominent sources of bias](notes.md#prominent-sources-of-bias)

---

## Answer Key Summary

| Q | Answer | Concept |
|:--:|:--:|:---|
| 1 | B | Bias characteristics and harm |
| 2 | D | Stereotype Score definition |
| 3 | C | Sources of bias |
| 4 | C | Adversarial triggers goal |
| 5 | D | Regard metric |
| 6 | A, C | ICL safety demonstrations |
| 7 | B, C, D | HRL/LRL imbalance |
| 8 | B | Responsible LLM axes |
| 9 | D | ICAT balanced metric |
| 10 | C | Skewed training distributions |

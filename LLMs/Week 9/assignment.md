# Week 9: Assignment 9 — Introduction to Large Language Models (LLMs) (noc26_cs88)

> **Due:** —
> **Submitted:** —
> **Score:** —

---

## Section 01 - Knowledge Graph Basics and Alignment

### Q1 (1 point)
**Which of the following statement best describes why knowledge graphs (KGs) are considered more powerful than a traditional relational knowledge base (KB)?**

- [ ] A) KGs require no schema, whereas KBs must have strict schemas.
- [ ] B) KGs store data only in the form of hypergraphs, eliminating redundancy.
- [ ] C) KGs allow flexible, graph-based connections and typed edges, enabling richer relationships and inferences compared to KBs.
- [ ] D) KGs completely replace the need for textual sources by storing all possible facts.

**Answer:** C

**Answer Explanation**: KGs represent entities as nodes and relations as typed edges, so adding new relationship types and traversing multi-hop connections is natural. This supports richer relational structure and inference than rigid table schemas.

**Related Topic**: [Knowledge graphs: triples and typed edges](notes.md#knowledge-graphs-triples-and-typed-edges)

---

### Q2 (1 point, multiple select)
**Entity alignment and relation alignment are crucial between KGs of different languages. Which of the following factors contribute to effective alignment? (Select all that apply)**

- [ ] A) Aligning relations solely by their lexical similarity, ignoring semantic context
- [ ] B) Transliteration or language-based string matching for entity labels
- [ ] C) Ensuring all language aliases are represented identically in each KG
- [ ] D) Matching neighbours, or connected entities, across different KGs

**Answer:** B, D

**Answer Explanation**: Cross-lingual alignment uses both surface-form signals (transliteration/string similarity) and structural signals (neighbor overlap in the graph). Lexical-only relation matching is insufficient, and aliases need not be identical to be alignable.

**Related Topic**: [Entity and relation alignment across languages](notes.md#entity-and-relation-alignment-across-languages)

---

## Section 02 - KGQA and Knowledge Graph Completion

### Q3 (1 point)
**In the context of knowledge graph completion (KGC), which statement best describes the role of the scoring function $f(s,r,o)$?**

- [ ] A) It determines whether two entities refer to the same real-world concept.
- [ ] B) It produces a raw confidence score indicating how plausible a triple $(s,r,o)$ is.
- [ ] C) It explicitly encodes only the subject's embedding, ignoring the relation and object embeddings.
- [ ] D) It ensures that every negative triple gets a higher score than any positive triple.

**Answer:** B

**Answer Explanation**: The scoring function maps embeddings of subject, relation, and object to a scalar plausibility score used for ranking and training.

**Related Topic**: [Scoring function fsro](notes.md#scoring-function-fsro)

---

### Q4 (1 point)
**One key difference between the differentiable KG approach and the semantic interpretation approach to KGQA is:**

- [ ] A) Differentiable KG approaches are fully rule-based, while semantic interpretation is purely neural.
- [ ] B) Differentiable KG approaches do not require any graph embeddings, relying instead on explicit logical forms.
- [ ] C) Semantic interpretation is more transparent or interpretable, whereas differentiable KG is end-to-end trainable but less interpretable.
- [ ] D) Both approaches use logical forms.
- [ ] E) The primary difference is the type of question they can answer.

**Answer:** C

**Answer Explanation**: Semantic interpretation produces explicit queries (transparent), while differentiable KGQA learns a neural end-to-end mapping (flexible but harder to interpret).

**Related Topic**: [KGQA paradigms](notes.md#kgqa-paradigms)

---

### Q5 (1 point, multiple select)
**Considering the differentiable KG approach, which elements are typically learned jointly when training an end-to-end KGQA model? (Select all that apply)**

- [ ] A) The textual question representation (e.g., BERT embeddings)
- [ ] B) The graph structure encoding (e.g., GCN or transformer-based graph embeddings)
- [ ] C) Predefined logical forms to ensure interpretability
- [ ] D) The final answer selection mechanism that identifies which node(s) in the graph satisfy the question

**Answer:** A, B, D

**Answer Explanation**: End-to-end differentiable KGQA typically learns question encoders, graph encoders, and an answer selection/scoring module jointly. Predefined logical forms belong to the semantic interpretation approach.

**Related Topic**: [Differentiable end-to-end KGQA](notes.md#differentiable-end-to-end-kgqa)

---

### Q6 (1 point)
**Uniform negative sampling can have high variance and may require large number of samples. Why is that the case?**

- [ ] A) Because the margin-based loss cannot converge without big mini-batches.
- [ ] B) Because randomly picking negative entities does not guarantee close or challenging negatives, causing unstable training estimates.
- [ ] C) Because negative sampling must ensure every possible negative triple is covered.
- [ ] D) Because the number of relations in the KG is too large for small number of samples.

**Answer:** B

**Answer Explanation**: Uniform negatives are often trivially wrong and provide weak gradients; the rarity of hard negatives makes minibatch estimates noisy, increasing variance and sample needs.

**Related Topic**: [Why uniform negative sampling can be high variance](notes.md#why-uniform-negative-sampling-can-be-high-variance)

---

## Section 03 - Evaluation and Embedding Models

### Q7 (1 point)
**In testing embedding and score quality for KG completion, mean rank and hits@K are typical metrics. What does hits@K specifically measure in this context?**

- [ ] A) The percentage of queries for which the correct answer appears in the top-K of the ranked list.
- [ ] B) The reciprocal of the rank of the correct answer.
- [ ] C) The probability of the correct answer appearing as the highest scored candidate.
- [ ] D) The margin of the correct triple score relative to all negative triples.

**Answer:** A

**Answer Explanation**: Hits@K is a top-K success rate: how often the true entity appears among the top K candidates for each query.

**Related Topic**: [Hits@K](notes.md#hitsk)

---

### Q8 (1 point)
**In the TransE model, the scoring function for a triple $(s,r,o)$ is $f(s,r,o) = \lVert e_s + e_r - e_o \rVert$. Which statement best explains what a low value of $f(s,r,o)$ indicates?**

- [ ] A) That $(s,r,o)$ is an invalid triple according to the learned embeddings.
- [ ] B) That $e_s$ and $e_o$ must be orthogonal.
- [ ] C) That the relation embedding $e_r$ is zero.
- [ ] D) That $(s,r,o)$ has a high likelihood of being a true fact in the knowledge graph.

**Answer:** D

**Answer Explanation**: TransE uses a distance-based score; if $e_s + e_r$ lands near $e_o$, the norm is small, meaning the triple is geometrically consistent and thus more plausible.

**Related Topic**: [TransE](notes.md#transe)

---

### Q9 (1 point)
**In RotatE, if a relation $r$ is intended to be symmetric, how would that typically manifest in the complex plane?**

- [ ] A) The relation embedding $e_r$ must always equal zero.
- [ ] B) The angle of $e_r$ must be $\pi/2$.
- [ ] C) The relation embedding $e_r$ is its own inverse (i.e., a rotation with angle $0$ or $\pi$).
- [ ] D) The magnitude of $e_r$ must be greater than 1.

**Answer:** C

**Answer Explanation**: A symmetric relation satisfies $r=r^{-1}$. In RotatE that means the rotation is self-inverse, which occurs for angle $0$ or $\pi$; the non-trivial symmetric case corresponds to a 180° rotation.

**Related Topic**: [RotatE](notes.md#rotate)

---

### Q10 (1 point)
**Which main advantage do rotation-based models (like RotatE) have over translation-based ones (like TransE) when it comes to complex multi-relational patterns in a KG?**

- [ ] A) Rotation-based models cannot model any symmetry or inverse patterns, so they are simpler.
- [ ] B) Rotation-based models handle a broader set of relation properties (symmetry, anti-symmetry, inverses, composition) more naturally.
- [ ] C) Rotation-based models have no hyperparameters to tune, unlike TransE.
- [ ] D) Rotation-based models are guaranteed to yield perfect link prediction.

**Answer:** B

**Answer Explanation**: Rotations in complex space naturally represent self-inverse relations, inverses, and composition as angle addition, making it easier to model multi-relational patterns than pure translation geometry.

**Related Topic**: [Symmetry, inverses, and composition](notes.md#symmetry-inverses-and-composition)

---

## Answer Key Summary

| Q | Answer | Concept |
|:--:|:--:|:---|
| 1 | C | KG graph structure vs rigid KB tables |
| 2 | B, D | Cross-lingual alignment signals |
| 3 | B | Scoring function outputs plausibility |
| 4 | C | Interpretability vs end-to-end training |
| 5 | A, B, D | Jointly learned KGQA components |
| 6 | B | Uniform negatives miss hard negatives |
| 7 | A | Hits@K top-K success rate |
| 8 | D | TransE low distance → plausible triple |
| 9 | C | Symmetry as self-inverse rotation |
| 10 | B | RotatE models richer relation patterns |

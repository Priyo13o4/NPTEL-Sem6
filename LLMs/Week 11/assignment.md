# Week 11: Assignment 11 — Introduction to Large Language Models (LLMs) (noc26_cs88)

> **Due:** 2026-04-08, 23:59 IST
> **Submitted:** 2026-04-05, 23:47 IST
> **Score:** —

---

## Section 01 - Multiplicative Models for KGC

### Q1 (1 point)
**What is the main modification that SimplE makes to DistMult-like models to handle asymmetric relations?**

- [ ] A) Replacing entity embeddings with random fixed vectors
- [ ] B) Introducing separate entity embeddings for subject and object roles, along with inverse relations
- [ ] C) Restricting the rank of the relation tensor to 1
- [ ] D) Using negative sampling for half of the triple set

**Answer:** B

**Answer Explanation**: DistMult’s diagonal relation form makes it symmetric in subject/object. SimplE breaks this by giving each entity two embeddings (subject-role and object-role) and by introducing inverse relations, enabling directionality.

**Related Topic**: [SimplE](notes.md#simple)

---

### Q2 (1 point, multiple select)
**Which statements correctly characterize the basic DistMult approach for knowledge graph completion? (Select all that apply)**

- [ ] A) Each relation r is parameterized by a full $D\times D$ matrix that can capture asymmetric relations.
- [ ] B) The relation embedding is a diagonal matrix, leading to a multiplicative interaction of entity embeddings.
- [ ] C) DistMult struggles with asymmetric relations because with diagonal $M_r$ we have $\text{score}(s,r,o)=\text{score}(o,r,s)$.
- [ ] D) DistMult's performance is typically tested only on fully symmetric KGs.

**Answer:** B, C

**Answer Explanation**: DistMult uses a diagonal relation matrix (efficient multiplicative interaction) but the resulting score is symmetric under swapping subject/object, so it struggles with asymmetric relations.

**Related Topic**: [DistMult](notes.md#distmult)

---

### Q3 (1 point, multiple select)
**Which statements about the ComplEx extension of DistMult are true? (Select all that apply)**

- [ ] A) It uses complex-valued embeddings to better capture asymmetric or anti-symmetric relations.
- [ ] B) It replaces the multiplication in DistMult with element-wise addition of real-valued vectors.
- [ ] C) For a perfectly symmetric relation, one could set the imaginary part of the relation embedding to zero.
- [ ] D) ComplEx requires each entity vector to be unit norm in the complex plane.

**Answer:** A, C

**Answer Explanation**: ComplEx uses complex embeddings and conjugation so it can represent asymmetric and anti-symmetric patterns. If the imaginary part of the relation is zero, the relation behaves symmetrically (reducing toward DistMult-like behavior).

**Related Topic**: [ComplEx](notes.md#complex)

---

### Q4 (1 point)
**Which best describes the main advantage of using a factorized representation (e.g., DistMult, ComplEx) for large KGs?**

- [ ] A) It enforces that every relation in the KG be perfectly symmetric.
- [ ] B) It ensures each entity is stored as a one-hot vector, simplifying nearest-neighbour queries.
- [ ] C) It collapses the entire KG into a single scalar value.
- [ ] D) It significantly reduces parameters and enables generalization to unseen triples by capturing low-rank structure.

**Answer:** D

**Answer Explanation**: Factorization compresses the sparse KG tensor into low-rank factors, reducing parameters and enabling generalization to new (unseen) triples through shared structure.

**Related Topic**: [Why factorization helps at KG scale](notes.md#why-factorization-helps-at-kg-scale)

---

### Q5 (1 point)
**Which statement best describes the reshaping of a 3D KG tensor $X\in\mathbb{R}^{|E|\times|R|\times|E|}$ into a matrix factorization problem?**

- [ ] A) One axis remains for subject, one axis remains for object, and relations are combined into a single expanded axis.
- [ ] B) The subject dimension is repeated to match the relation dimension, resulting in a 2D matrix.
- [ ] C) Each subject–relation pair is collapsed into a single dimension, while objects remain as separate entries.
- [ ] D) The entire KG is vectorized into a 1D array and then factorized with an SVD approach.

**Answer:** C

**Answer Explanation**: A common flattening groups the subject and relation into one combined index (rows), keeping objects as the other axis (columns), so standard 2D factorization methods apply.

**Related Topic**: [KG tensor view and reshaping](notes.md#kg-tensor-view-and-reshaping)

---

## Section 02 - Modeling Hierarchies

### Q6 (1 point)
**Which key property of hierarchical relationships (e.g. is-a, transitivity) motivates the exploration of specialized embedding methods over standard Euclidean KG embeddings?**

- [ ] A) Symmetry in the relation (A, is-a, B) implying (B, is-a, A)
- [ ] B) Frequent presence of cycles in hierarchical graphs
- [ ] C) Transitivity in the form (camel, is-a, mammal) and (mammal, is-a, animal) ⟹ (camel, is-a, animal)
- [ ] D) The high dimensionality of the entity embeddings

**Answer:** C

**Answer Explanation**: Hierarchies require transitive structure: if $x$ is under $y$ and $y$ is under $z$, then $x$ is under $z$. Specialized embeddings (hyperbolic/order/box) model such containment/transitivity more naturally.

**Related Topic**: [Hierarchical relations and why Euclidean embeddings struggle](notes.md#hierarchical-relations-and-why-euclidean-embeddings-struggle)

---

### Q7 (1 point, multiple select)
**Which of the following statements correctly describe hyperbolic (Poincaré) embeddings for hierarchical data? (Select all that apply)**

- [ ] A) They map nodes onto a disk (or ball) such that large branching factors can be represented with lower distortion than in Euclidean space.
- [ ] B) Distance grows slowly near the center and becomes infinite near the boundary, making it naturally suited for tree-like structures.
- [ ] C) They require each node to be embedded on the surface of the Poincaré disk of radius 1.
- [ ] D) They can achieve arbitrarily low distortion embeddings for trees with the same dimension as Euclidean space.

**Answer:** A, B

**Answer Explanation**: Hyperbolic space expands rapidly with radius, which matches exponential growth in trees. The Poincaré disk/ball provides low-distortion representations for large branching hierarchies compared to Euclidean embeddings.

**Related Topic**: [Hyperbolic (Poincaré) embeddings](notes.md#hyperbolic-poincare-embeddings)

---

### Q8 (1 point, multiple select)
**Why might a partial-order-based approach (like order embeddings) be beneficial for modelling “is-a” relationships compared to purely distance-based approaches? (Select all that apply)**

- [ ] A) They explicitly encode the ancestor–descendant relation as a coordinate-wise inequality or containment.
- [ ] B) They can represent negative correlations (i.e., sibling vs. ancestor) more easily than distance metrics.
- [ ] C) They inherently guarantee transitive closure of the hierarchy in the learned embedding space.
- [ ] D) They do not rely on pairwise distances but use a notion of coordinate-wise ordering or interval containment.

**Answer:** A, D

**Answer Explanation**: Order-based methods encode hierarchy via coordinate-wise ordering/containment rather than distance. This directly expresses ancestor–descendant relationships in the embedding representation.

**Related Topic**: [Order embeddings](notes.md#order-embeddings)

---

### Q9 (1 point)
**Which statement about box embeddings in hierarchical modelling is most accurate?**

- [ ] A) Each entity or type is assigned a single real-valued vector, ignoring bounding volumes.
- [ ] B) Containment $I_x \subseteq I_y$ (all dimensions) encodes $x \prec y$.
- [ ] C) They rely on spherical distances around a central node to measure tree depth.
- [ ] D) They cannot be used to represent set intersections or partial overlap.

**Answer:** B

**Answer Explanation**: Box embeddings represent entities as axis-aligned hyperrectangles; hierarchy is captured by containment. Unlike point embeddings, boxes can overlap and intersect, representing partial overlap and set-like behavior.

**Related Topic**: [Box embeddings](notes.md#box-embeddings)

---

### Q10 (1 point)
**What is a key challenge with axis-aligned open-cone (order) embeddings for hierarchical KG data?**

- [ ] A) They enforce that all sibling categories have identical cone apices, which causes overlap.
- [ ] B) They require symmetrical relationships for all edges.
- [ ] C) They do not allow partial orders to be extended to total orders.
- [ ] D) The volume (measure) of cones is the same regardless of how “broad” or “narrow” the cone is, making sub-categories indistinguishable by volume.

**Answer:** D

**Answer Explanation**: If cone volume does not vary with category breadth, then broad and narrow categories can be indistinguishable by that measure, reducing representational granularity.

**Related Topic**: [Open-cone limitation (axis-aligned cones)](notes.md#open-cone-limitation-axis-aligned-cones)

---

## Answer Key Summary

| Q | Answer | Concept |
|:--:|:--:|:---|
| 1 | B | SimplE fixes asymmetry |
| 2 | B, C | DistMult diagonal + symmetry issue |
| 3 | A, C | ComplEx complex embeddings |
| 4 | D | Low-rank factorization benefit |
| 5 | C | Tensor flattening (subject–relation) |
| 6 | C | Transitivity in hierarchies |
| 7 | A, B | Hyperbolic/Poincaré intuition |
| 8 | A, D | Partial orders via inequalities |
| 9 | B | Box containment encodes hierarchy |
| 10 | D | Open-cone volume limitation |

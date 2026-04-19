# Week 11: Theoretical Notes — Multiplicative KG Models, Hierarchies, and Temporal Knowledge Graphs

## Overview
This week extends knowledge graph completion beyond translation/rotation (TransE/RotatE) to **multiplicative (factorization) models** like **DistMult**, **SimplE**, and **ComplEx**. We then study why **hierarchical relations** (is-a, transitivity) are hard to represent in Euclidean space, motivating **hyperbolic (Poincaré)** embeddings and **partial-order** representations (order, box embeddings). Finally, we introduce **temporal knowledge graphs** where facts change over time, and how scoring functions can be extended with time-aware encodings.

---

## Multiplicative (factorization) models for KGC
Multiplicative models treat KGC as a structured **tensor/matrix factorization** problem.

Motivation (analogy): document-term factorization (LSA) or embedding factorization (Word2Vec) compresses large sparse co-occurrence data into dense, low-rank factors.

**Related Assignment Questions**: Q4.

---

## KG tensor view and reshaping
A KG can be represented as a sparse 3D tensor $X$ capturing whether triples are present:

$$X \in \mathbb{R}^{|E|\times|R|\times|E|}$$

Two tractable ideas mentioned:
1. **reshape to 2D**: collapse a pair of axes (e.g., subject–relation) into one axis, and apply matrix factorization (e.g., SVD)
2. **restrict relation structure**: constrain relation parameters to special forms (diagonal, complex, etc.)

A common flattening used in the lectures/assignment:
- treat each **subject–relation** pair as one “row index” and keep **objects** as the “column index”

**Related Assignment Questions**: Q5.

---

## DistMult
**DistMult** restricts each relation to a **diagonal matrix**, producing a three-way multiplicative interaction:

$$\text{score}(s,r,o)= a_s^\top M_r a_o$$

With $M_r$ diagonal, this is equivalent to a weighted element-wise product followed by a sum.

### Symmetry limitation
DistMult is inherently **symmetric** in $s$ and $o$, so it struggles with asymmetric relations:

$$\text{score}(s,r,o)=\text{score}(o,r,s)$$

**Related Assignment Questions**: Q2.

---

## SimplE
**SimplE** fixes DistMult-style symmetry issues by:
- using **separate embeddings** for an entity’s subject-role and object-role
- adding **inverse relations** so directionality is modeled explicitly

This makes asymmetric relations possible (e.g., parent_of, works_for).

**Related Assignment Questions**: Q1.

---

## ComplEx
**ComplEx** uses **complex-valued** embeddings and uses the **complex conjugate** of the object embedding in scoring, enabling asymmetry/anti-symmetry.

Key relation-pattern facts from the lecture:
- if the **imaginary part** of the relation embedding is 0, the relation behaves **symmetric** (reduces toward DistMult-like behavior)
- if the **real part** of the relation embedding is 0, it can behave **anti-symmetric**

Regularization note:
- **ComplEx-N3** uses nuclear 3-norm style regularization to control complexity

Benchmarks highlighted:
- WN18RR and FB15K-237 are “hardened” benchmarks that remove test leakage present in older variants.

**Related Assignment Questions**: Q3.

---

## Why factorization helps at KG scale
Factorized representations (DistMult/ComplEx family) are useful because they:
- drastically reduce parameters vs memorizing a huge sparse tensor
- capture **low-rank structure** that can generalize to **unseen triples**

**Related Assignment Questions**: Q4.

---

## Hierarchical relations and why Euclidean embeddings struggle
Hierarchies like taxonomies have **transitivity**:

$$(\text{camel is-a mammal}) \wedge (\text{mammal is-a animal}) \Rightarrow (\text{camel is-a animal})$$

Standard Euclidean embeddings (designed for “flat” relations) can struggle to represent large branching hierarchies with low distortion.

**Related Assignment Questions**: Q6.

---

## Hyperbolic (Poincaré) embeddings
**Poincaré / hyperbolic embeddings** place nodes in a hyperbolic disk/ball.

Two key geometric intuitions emphasized:
- hyperbolic space supports **large branching factors** with lower distortion than Euclidean space
- distances grow slowly near the center and explode near the boundary, making it natural for tree-like structures

**Related Assignment Questions**: Q7.

---

## Partial-order representations
### Order embeddings
**Order embeddings** encode ancestor–descendant relationships via **coordinate-wise inequality** (a partial order) rather than pairwise distances.

**Related Assignment Questions**: Q8.

### Box embeddings
**Box embeddings** represent each entity/type as an axis-aligned hyperrectangle $I_x$.

Containment encodes hierarchy:

$$I_x \subseteq I_y \;\;\Rightarrow\;\; x \prec y$$

Boxes also support intersections and partial overlap.

**Related Assignment Questions**: Q9.

### Open-cone limitation (axis-aligned cones)
A limitation discussed for open-cone style order embeddings:
- **cone volume** does not reflect “breadth” (broad vs narrow categories can have the same volume), making sub-category granularity hard to distinguish by volume alone

**Related Assignment Questions**: Q10.

---

## Temporal knowledge graphs
Many facts are time-sensitive. Temporal KGs augment triples with time:

$$(s, r, o, t) \quad \text{or} \quad (s, r, o, [t_\text{start}, t_\text{end}])$$

### Temporal KGC
Temporal KGC extends scoring functions (e.g., TransE/DistMult/ComplEx) with time-aware components.

### Time encoding strategies
Common strategies mentioned:
- discretize time into bins
- continuous time embeddings
- temporal regularization to encourage smooth changes in embeddings over time

---

## Key Takeaways

| Concept | Definition | Example |
|:---|:---|:---|
| DistMult | Diagonal relation matrix, three-way product score | symmetric scoring issue |
| SimplE | Separate subject/object embeddings + inverse relations | models parent_of direction |
| ComplEx | Complex embeddings + conjugation | can model anti-symmetry |
| Tensor reshaping | Flatten KG tensor to apply 2D factorization | (subject, relation) as row |
| Low-rank structure | Compact factors generalize beyond seen triples | predict missing edges |
| Hyperbolic embeddings | Non-Euclidean geometry for trees | Poincaré disk |
| Order embeddings | Partial order via coordinate-wise inequality | ancestor ≥ descendant |
| Box embeddings | Hierarchy via interval containment | $I_{poodle}\subseteq I_{dog}$ |
| Temporal KG | Facts indexed by time | (leader_of, t) changes |

---

## Summary
- Multiplicative KG models perform structured factorization; DistMult is efficient but symmetric.
- SimplE adds subject/object roles and inverse relations to model asymmetry; ComplEx uses complex numbers to capture richer relation patterns.
- Hierarchies motivate hyperbolic geometry and partial-order approaches like order and box embeddings.
- Temporal knowledge graphs attach time to facts and require time-aware scoring and evaluation.

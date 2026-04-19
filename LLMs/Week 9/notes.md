# Week 9: Theoretical Notes — Knowledge Graphs, KGQA, Knowledge Graph Completion, and Embedding Models

## Overview
Week 9 introduces **knowledge graphs (KGs)** as structured stores of factual information and explains how they support retrieval, question answering, and keeping facts up-to-date. We study two KGQA paradigms (semantic interpretation vs differentiable end-to-end), the **knowledge graph completion (KGC)** task with scoring functions and negative sampling, and evaluation metrics like **MRR** and **Hits@K**. Finally, we cover KG embedding models: translation-based (TransE/TransH) and rotation-based (RotatE).

---

## Knowledge graphs: triples and typed edges
A **knowledge graph** stores facts as directed, typed edges between entities, typically as triples:

$$(s, r, o) = (\text{subject},\;\text{relation},\;\text{object})$$

- entities are nodes (people, places, products)
- relations are labeled edges (e.g., `born_in`, `capital_of`)

This graph representation enables rich connectivity and multi-hop reasoning compared to a rigid table-centric relational KB.

**Related Assignment Questions**: Q1.

---

## Prominent knowledge graphs and identifiers
Large KGs use **entity identifiers** to disambiguate names:
- Wikidata, Freebase, WordNet, YAGO (examples from the scrape)
- an entity can have many **aliases** (surface forms)
- multilingual KGs store labels across languages, often needing alignment

Entity IDs help separate ambiguous strings (e.g., “Apple” fruit vs company).

---

## Entity and relation alignment across languages
When you have multiple KGs in different languages, you often want to **align**:
- **entity alignment**: map entity nodes across graphs
- **relation alignment**: map relation types across graphs

Two practical signals emphasized in the scrape:
1. **transliteration / string matching** of entity labels
2. **neighbor matching** (graph structure): if two candidate entities connect to similar neighbors, alignment is more plausible

**Related Assignment Questions**: Q2.

---

## KG incompleteness and why we combine LLMs with KGs
KGs are typically incomplete because facts are added over time and extraction is imperfect.

Motivations for combining LLMs with KGs include:
- reduce hallucination by grounding answers in retrieved facts
- improve interpretability (you can point to supporting triples)
- handle mutable facts without retraining a large LM

---

## KGQA paradigms
**KGQA (Knowledge Graph Question Answering)** aims to answer natural-language questions using a KG.

### Semantic interpretation: NL to SPARQL
- translate question to an explicit logical query (e.g., SPARQL)
- **pros**: transparent and debuggable
- **cons**: brittle if parsing fails or schema mismatches

### Differentiable end-to-end KGQA
A neural pipeline typically learns components jointly:
- question representation (e.g., BERT embeddings)
- graph encoding (e.g., GCN/Transformer graph embeddings)
- answer selection (e.g., attention over nodes)

- **pros**: end-to-end trainable and robust to linguistic variation
- **cons**: less interpretable than explicit logical forms

**Related Assignment Questions**: Q4, Q5.

---

## Knowledge graph completion (KGC)
**KGC** predicts missing facts by scoring candidate triples.

### Scoring function fsro
A scoring function $f(s,r,o)$ produces a raw plausibility score for a triple:

$$f(s,r,o) \rightarrow \text{plausibility score}$$

Higher plausibility should correspond to “more likely true” under the learned embedding model (the direction depends on the specific scoring design).

**Related Assignment Questions**: Q3.

### Positive vs negative sampling (local closed-world assumption)
KGs primarily store positives (true triples). To train discriminatively you create negatives by corrupting a triple:
- replace the subject: $(s', r, o)$
- replace the object: $(s, r, o')$

Under a **local closed-world assumption**, missing edges are treated as negative for training, acknowledging this can introduce false negatives.

### Why uniform negative sampling can be high variance
If negatives are chosen uniformly at random, most are “too easy” (obviously wrong). Training signals become noisy because:
- you rarely sample *hard negatives* (plausible but incorrect)
- gradient estimates vary a lot across minibatches

So you often need many samples or smarter sampling strategies.

**Related Assignment Questions**: Q6.

---

## KGC losses: margin vs softmax (high-level)
Two common training styles:
- **margin / hinge loss**: enforce a score gap between positive and negative triples
- **softmax log-likelihood**: treat the true entity as the correct class among candidates

The key design choice is how you turn scores into an optimization objective.

---

## Evaluation metrics for KGC
KGC evaluation often ranks candidate entities for a query like $(s,r,?)$ or $(?,r,o)$.

### Hits@K
**Hits@K** measures the fraction of queries where the correct entity appears in the top $K$ ranked candidates.

**Related Assignment Questions**: Q7.

### Mean Reciprocal Rank (MRR)
For each query, compute $1/\text{rank}$ of the true entity and average across queries.

### MAP and consistency
**MAP (Mean Average Precision)** is used in retrieval-style setups.

A key practical detail: **filtered vs unfiltered** evaluation.
- unfiltered: other true triples can appear as “negatives” and hurt rank unfairly
- filtered: remove known true triples from the candidate set before ranking

Consistency matters when comparing papers.

---

## Translation-based KG embedding models
### TransE
**TransE** models relations as translations in embedding space:

$$f(s,r,o) = \lVert e_s + e_r - e_o \rVert$$

Here, a **low** value means the relation translation lands near the object embedding, making the triple more plausible.

Limitations:
- struggles with 1-to-many, many-to-1, many-to-many relations
- struggles with symmetric relations (geometry can conflict)

**Related Assignment Questions**: Q8.

### TransH
**TransH** addresses some many-to-many issues by projecting entities onto a relation-specific hyperplane before applying translation.

---

## Rotation-based KG embedding models
### RotatE
**RotatE** embeds entities in the complex plane and models relations as element-wise rotations:

$$f(s,r,o) = \lVert e_s \circ e_r - e_o \rVert^2$$

- $\circ$ is element-wise multiplication in complex space
- relations encode rotation angles (typically with unit modulus)

#### Symmetry, inverses, and composition
RotatE can model several relation patterns naturally:
- **symmetry**: a relation can be its own inverse ($r = r^{-1}$), which corresponds to a rotation of $0$ or $\pi$; the non-trivial symmetric case is $\pi$ (180°)
- **inverse relations**: $r^{-1}$ corresponds to reversing the rotation
- **composition**: following a path corresponds to composing rotations

**Related Assignment Questions**: Q9, Q10.

---

## Key Takeaways

| Concept | Definition | Example |
|:---|:---|:---|
| Knowledge graph | Fact store of (subject, relation, object) triples | (Paris, capital_of, France) |
| Typed edges | Relations label connections between entities | directed `born_in` edge |
| Alignment | Map entities/relations across languages | label match + neighbor match |
| KGQA semantic interpretation | NL question → explicit query | NL → SPARQL |
| KGQA differentiable model | End-to-end neural KGQA | BERT + GCN + attention |
| KGC scoring function | Plausibility score for triples | $f(s,r,o)$ |
| Negative sampling | Create corrupted triples for training | replace subject/object |
| Hits@K | Top-K success rate | correct answer in top 10 |
| TransE | Translation model in vector space | low $\lVert e_s+e_r-e_o\rVert$ |
| RotatE | Rotation model in complex space | symmetry/inverses/composition |

---

## Summary
- Knowledge graphs store facts as typed edges over entities, enabling rich graph connectivity.
- Cross-lingual alignment uses label matching and neighbor structure signals.
- KGQA can be transparent (NL-to-query) or differentiable (end-to-end neural).
- KGC learns scoring functions over triples using negative sampling and is evaluated with ranking metrics like Hits@K and MRR.
- TransE/TransH model relations as translations; RotatE models them as rotations and handles more complex relation patterns.

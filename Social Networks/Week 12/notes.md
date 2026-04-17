# Week 12: Theoretical Notes — K-Core Decomposition, Coreness & Viral Cascades

## Overview

This final week synthesizes everything we've learned about network structure and dynamics into a unified framework for identifying true centers of influence. For eleven weeks, we've explored how networks are organized—through clustering, centrality measures, small-world structure, and community detection. Yet we've never directly answered the most practical question: **Who truly matters in a network, and why do some individuals trigger massive cascades while others with thousands of followers fail?** The answer lies not in degree (follower count), but in **coreness**—a node's structural depth within the network's densest regions. We introduce k-core decomposition, a peeling algorithm that reveals the network's layered structure like an onion. We then demonstrate through independent cascade simulations that coreness predicts spreading power far better than degree alone. Finally, we uncover the surprising existence of "pseudo-cores"—intermediate structural layers that generate viral cascades rivaling the innermost core, yet with vastly greater reach and accessibility. This week transforms network science into actionable knowledge for viral marketing, epidemiology, and organizational design.

---

## 1. The Limits of Degree: Why Follower Count Isn't Everything

### The Celebrity Paradox

Consider two influencers on a social media platform:

**Influencer A**: 1 million followers, but they are globally distributed, largely strangers to each other. A posts a meme. 1 million people see it momentarily. Few retweet or share because there is no social proof, no dense cluster of peers all discussing it simultaneously.

**Influencer B**: 50,000 followers, densely clustered within an elite professional network. These followers know each other, collaborate, and actively engage. B posts a thought-provoking insight. Within minutes, it cascades through the tightly connected network, each node sharing with peers they trust, triggering exponential growth.

**The Paradox**: Despite B's followers being 1% of A's, B's message often achieves 10–100x greater reach and adoption.

### Why Degree Fails

**Degree centrality** measures purely local information: how many immediate neighbors does a node have? It completely ignores:
- **Structural redundancy**: Do my neighbors also know each other, creating echo chambers?
- **Global position**: Am I in the center of the network or isolated in a peripheral cluster?
- **Connectivity of neighbors**: Are my neighbors connected to highly connected nodes, or are they all dead ends?

A node with high degree in an isolated star graph (one central hub with many leaf connections) may have zero cascading power because its neighbors lack further connections.

---

## 2. K-Core Decomposition: Peeling the Network Onion

### The Fundamental Concept

A **k-core** is a maximal subgraph where every node has degree at least $k$ within that subgraph.

**Mathematical Definition**:
$$C_k = \{V' \subseteq V : \forall v \in V', \deg_{V'}(v) \geq k\}$$

where $\deg_{V'}(v)$ is the degree of node $v$ calculated only among nodes in the subgraph $V'$.

**Intuition**: Imagine repeatedly peeling a network:
- Remove all nodes with degree < 1 (isolated nodes).
- Remove all nodes with degree < 2.
- Continue removing nodes with degree < 3, < 4, ... until you cannot remove any more nodes without deleting the entire graph.

The remaining nodes at each step form a k-core.

### The K-Core Decomposition Algorithm

**Input**: Graph $G = (V, E)$  
**Output**: Coreness value for each node

**Algorithm**:
```
1. FOR each node v in V:
     degree[v] = |neighbors of v|
   
2. FOR k = 1, 2, 3, ...
     REPEAT:
       Remove all nodes with degree < k
       Update degrees of remaining neighbors
     UNTIL no nodes have degree < k
     
     Mark all removed nodes as having coreness < k
   
3. The final k value reached is the maximum coreness
```

**Example: Small Network**

Consider a 10-node network:
- Nodes 1, 2, 3: degree 1 each (leaf nodes)
- Nodes 4, 5, 6: degree 2 each (peripheral cluster)
- Nodes 7, 8, 9, 10: degree 4 each (densely connected core)

**Decomposition Steps**:

**k=1**: Remove all isolated nodes (none exist). All nodes have ≥ 1 connection.

**k=2**: Remove nodes with degree < 2.
- Nodes 1, 2, 3 (degree 1) are removed.
- Remaining nodes now: {4, 5, 6, 7, 8, 9, 10}.
- Nodes 4, 5, 6 now have degree 1 or 2 (after losing connections to 1, 2, 3).

**k=3**: Remove nodes with degree < 3.
- Nodes 4, 5, 6 (degree ≤ 2) are removed.
- Remaining nodes: {7, 8, 9, 10}.
- All have degree 4, well above the threshold.

**k=4**: All remaining nodes have degree ≥ 4. No nodes removed.

**Final Coreness**:
- Nodes 1, 2, 3: coreness = 1
- Nodes 4, 5, 6: coreness = 2
- Nodes 7, 8, 9, 10: coreness = 4

### Why Iterative Removal Matters

The crucial insight: **removing a node cascades**. When you delete a node, its neighbors lose that connection, potentially dropping their degree below the current $k$ threshold. This triggers a cascade of further removals until the remaining subgraph stabilizes with all nodes having degree ≥ $k$.

**Example of Cascading**:
- In the algorithm above, when nodes 1, 2, 3 are removed during k=2, node 4 loses 1 connection.
- If node 4 originally had degree 2, and it was connected to node 1, it now has degree 1.
- This triggers node 4's removal during k=2, which further reduces neighbors' degrees.

This cascading ensures that k-cores are maximally dense and deeply embedded.

### Properties of K-Cores

1. **Nested Structure**: $C_1 \supseteq C_2 \supseteq C_3 \supseteq ... \supseteq C_{\max}$
   - Each k-core is contained within the previous one.

2. **Coreness Assignment**: A node's **coreness** is the maximum $k$ value for which it belongs to $C_k$.
   - If a node is removed during the $k=5$ step, its coreness is 4.

3. **Uniqueness**: Each node has a single, well-defined coreness value.

4. **Computational Efficiency**: The algorithm runs in $O(m)$ time, where $m$ is the number of edges—remarkably fast even for massive networks.

---

## 3. Coreness as a Centrality Measure

### Coreness vs. Degree: A Comparison

| Property | Degree | Coreness |
|----------|--------|----------|
| **Definition** | Number of immediate neighbors | Maximum k in which node survives |
| **What it captures** | Local connectivity | Structural depth and embeddedness |
| **Sensitivity to echo chambers** | Insensitive (counts all connections equally) | Sensitive (requires reciprocal density) |
| **Computation** | $O(1)$ per node | $O(m)$ for entire graph |
| **Predictive power for cascades** | Weak (~0.3–0.5 correlation) | Strong (~0.7–0.9 correlation) |
| **Interpretation** | "How many people do I know?" | "How central am I to the network's core?" |

### Why Coreness Outperforms Degree for Influence

**Reason 1: Structural Density**
Coreness requires that a node survives in a region where every other node also has ≥ k connections. A high-coreness node is therefore surrounded by highly connected peers. In cascade models, this density means rapid, repeated exposures and high thresholds for adoption.

**Reason 2: Global Position**
A high-degree node could be the hub of an isolated star graph—locally central but globally peripheral. Coreness forces consideration of global structure: to survive in a high k-core, neighbors must also be deeply embedded.

**Reason 3: Redundancy Elimination**
Degree counts every connection equally. If a node has 100 followers but they are all in one clique that overlaps with its other 100 followers' cliques, there is massive redundancy. Coreness inherently captures this: to achieve high coreness, connections must fan out across dense, interconnected regions—not cluster redundantly.

---

## 4. Independent Cascade Model: Simulating Viral Spread

### The Model Specification

The **Independent Cascade Model** simulates information diffusion in discrete time steps:

**Model Definition**:
1. **Initialization**: One or more "seed" nodes are marked as "infected" (adoptive).
2. **Activation**: At each time step $t$, each newly infected node $v$ (infected at $t-1$) attempts to activate each of its uninfected neighbors $u$ independently with probability $p$ (the transmission probability).
3. **Irreversibility**: Once a node attempts to activate a neighbor, that edge becomes inactive and cannot transmit again.
4. **Termination**: The cascade stops when a time step passes with zero new infections.

**Mathematical Formulation**:
$$I(t+1) = I(t) \cup \{u : v \in I(t), (v,u) \in E, \text{random}() < p\}$$

where $I(t)$ is the set of infected nodes at time $t$.

### Spreading Dynamics

**Phase 1: Rapid Expansion** (Early Steps)
- Seeded nodes activate their neighbors.
- In densely connected regions (high-coreness nodes), numerous nodes activate simultaneously, creating positive feedback.
- Cascade size grows exponentially.

**Phase 2: Plateau** (Middle Steps)
- Most nodes in core regions are infected.
- Activation reaches peripheral regions (low-coreness nodes).
- Cascade size growth slows as fewer uninfected neighbors remain.

**Phase 3: Termination** (Late Steps)
- Cascade reaches isolated nodes slowly or not at all.
- Eventually, an iteration passes with zero new activations.
- Cascade stabilizes at final size.

### Empirical Observations from Cascades

In real networks, seeding nodes from different k-cores yields dramatically different cascade sizes:

| Seeding Location | Cascade Size | Time to Termination |
|---|---|---|
| Innermost core ($k_{\max}$) | ~40% of network | 8–10 steps |
| Mid-core shells ($k = 0.75 k_{\max}$) | ~38% of network | 9–12 steps |
| Peripheral shells ($k ≤ 5$) | ~5–10% of network | 15–20+ steps |

The dramatic difference arises because high-coreness nodes inject the infection into the network's densest region, where transmission rates are highest.

---

## 5. The Surprising Discovery: Pseudo-Cores

### The Problem with Targeting the Innermost Core

The innermost core ($k = k_{\max}$) contains the most densely connected nodes, but:

1. **Rarity**: The innermost core typically comprises < 1% of the network (e.g., 0.5% in the microblogging case study).
2. **Cost**: These nodes are celebrities, experts, or key figures—often expensive or impossible to access.
3. **Echo Chamber**: Because the core is so densely interconnected, seeding multiple core nodes provides highly redundant exposure to the same audience. There is minimal outreach to different communities.

**Strategic Challenge**: How can marketers achieve core-level spreading power while reaching diverse communities and staying within budget?

### The Discovery: Intermediate Shells Generate Comparable Cascades

Empirical research revealed a surprising result: **intermediate k-core shells, far removed from the absolute center, generate cascade sizes nearly identical to the innermost core**.

**Example from Case Study 1** (Microblogging Platform):
- Innermost core ($k=45$): 0.5% of network, cascade size ≈ 40%.
- Pseudo-core shells ($k=30$–$k=35$): 4% of network, cascade size ≈ 38–39%.
- Peripheral nodes ($k≤5$): 40% of network, cascade size ≈ 5–8%.

**Why?** The intermediate shells possess:
- **Sufficient density**: Nodes at $k=30$ still exist in highly connected regions; their neighbors and neighbors' neighbors are highly connected.
- **Cross-community reach**: Unlike the inner core (which is purely internal), intermediate shells border multiple different k-core shells. They serve as bridges between communities.
- **Activation momentum**: When seeded, pseudo-cores activate enough local density to sustain cascading without the echo-chamber problem.

### Formal Definition of Pseudo-Cores

**Pseudo-Core** is an informal term referring to k-core shells where:
$$\text{Cascade}(C_k) \geq 0.9 \times \text{Cascade}(C_{\max})$$

In other words, the cascade size achieves at least 90% of the innermost core's size, despite being much larger (more abundant) in the network.

### Why Pseudo-Cores Are Strategically Superior

**Comparison: Innermost Core vs. Pseudo-Core Strategy**

| Dimension | Innermost Core | Pseudo-Core |
|---|---|---|
| **Cascade Size** | 40% | 38–39% |
| **Abundance** | 0.5% | 4–5% |
| **Cost per Node** | Very high (celebrities) | Moderate (influential professionals) |
| **Total Budget Required** | Extreme | 8–10x less than core |
| **Audience Diversity** | High redundancy | Diverse, multiple communities |
| **Implementation** | Difficult (celebrities resist) | Feasible (professionals cooperate) |
| **Viral Momentum** | Rapid | Slightly slower but sustained |

**Business Insight**: For viral marketing campaigns with realistic budgets, seeding 50 pseudo-core nodes generates nearly the same cascade as seeding 5 innermost-core celebrities, with far greater reach and sustainability.

---

## 6. Why Coreness Predicts Cascades Better Than Degree

### The Empirical Evidence

Across multiple network types, research shows:
- **Correlation between degree and cascade size**: $r \approx 0.35–0.50$ (weak)
- **Correlation between coreness and cascade size**: $r \approx 0.75–0.90$ (strong)

### The Structural Reason

**Cascade Theory Principle**: A node's spreading power depends not on how many people it reaches directly, but on how **densely interconnected** those people are with the broader network.

**Mathematical Insight**: In the independent cascade model, cascade size depends on the **reachability** of uninfected nodes through activated ones. Reachability is highest when:
1. Activated nodes have many neighbors (high degree).
2. Neighbors are also highly connected (high neighbor coreness).
3. The region is densely interconnected (high local clustering).

Coreness captures all three factors simultaneously. Degree only captures the first.

### Real-World Validation

**Microblogging Platform Analysis**:
- Randomly selected high-degree nodes: average cascade size ~25% of innermost core.
- Randomly selected high-coreness nodes (pseudo-cores): average cascade size ~85–95% of innermost core.
- Improvement: 3–4x better predictive power from coreness.

---

## 7. Removing Core Nodes: Impact on Network Resilience

### The Fragmentation Effect

Removing high-coreness nodes has catastrophic consequences for network-wide cascades:

**Simulation**: In a network of 500,000 nodes with a largest k-core size of 28:
- **Baseline cascade** (all nodes): 40% adoption.
- **After removing innermost core** ($k=28$): cascade drops to 8–12%.
- **After removing mid-core** ($k=20$–$k=25$): cascade drops to 18–22%.

**Why**: High-coreness nodes serve as the **structural scaffolding** of the network. Removing them fragments the dense regions into smaller, isolated clusters. These fragments have reduced transmission rates and limited outreach.

### Comparison: Removing High-Degree but Low-Coreness Nodes

Removing high-degree but peripheral nodes (e.g., celebrities with isolated star-graph followers) has comparatively minimal impact on cascades:
- **Cascade reduction**: 2–5% (vs. 28–32% for core removal).

**Insight**: **Structural position matters far more than follower count for network resilience**.

---

## 8. Applications: Beyond Viral Marketing

### Emergency Response Networks

In disaster relief coordination networks (as discussed in Week 11 and now week 12):
- Core coordinators (high coreness) are the backbone of information diffusion.
- Removing core coordinators causes critical communication failures.
- Pseudo-core coordinators offer redundancy: nearly comparable reach with greater abundance.

### Disease Control

In epidemiological networks:
- High-coreness individuals are superspreaders—targeting them for vaccination provides maximal reduction in epidemic spread.
- Vaccinating random high-degree individuals (e.g., party-goers) is inefficient.
- Pseudo-core individuals offer cost-effective vaccination strategies: slightly less impact than core, but vastly more accessible.

### Organizational Influence

In corporate hierarchies (often invisible, not formal org charts):
- True leaders are not always the highest titles.
- High-coreness employees are deeply embedded in informal networks, driving decisions.
- Pseudo-core employees (mid-level professionals in multiple teams) are change agents.

---

## 9. Comparison: Centrality Measures Across the Course

By Week 12, we've encountered multiple centrality measures. A comprehensive comparison:

| Measure | Definition | Time Complexity | Cascade Prediction | Captures |
|---|---|---|---|---|
| **Degree** | Number of neighbors | $O(1)$ | Weak (r~0.4) | Local connectivity |
| **Betweenness** | Fraction of shortest paths | $O(mn)$ | Moderate (r~0.55) | Bridge role |
| **Closeness** | Average distance to others | $O(n+m)$ | Weak (r~0.35) | Global efficiency |
| **PageRank** | Iterative prestige ranking | $O(m)$ | Moderate (r~0.60) | Authority/trust |
| **Coreness** | Max k-core membership | $O(m)$ | Strong (r~0.82) | Structural depth |
| **Clustering Coeff** | Local density | $O(1)$ | Weak (r~0.30) | Local redundancy |

**Winner for Cascade Prediction**: **Coreness** dominates empirically and theoretically.

---

## 10. The Mathematics of K-Core Dynamics

### Lemma: Nested Structure Proof

**Claim**: $C_1 \supseteq C_2 \supseteq C_3 \supseteq ... \supseteq C_{\max}$

**Proof**: If a node $v$ belongs to $C_k$, then $v$ has degree ≥ $k$ within $C_k$. By definition, $C_{k+1}$ removes all nodes with degree < $k+1$. Nodes in $C_k$ with degree exactly $k$ are removed. Therefore, $C_{k+1} \subset C_k$. ∎

### Theorem: Computational Complexity

**Claim**: K-core decomposition runs in $O(m)$ time for a graph with $m$ edges.

**Proof Sketch**: 
- Maintain a degree count for each node: $O(n)$ initialization.
- Process each node at most once (when removed): $O(n)$ removals.
- Each removal updates degrees of neighbors: total $O(m)$ edge updates.
- Total: $O(n + m) = O(m)$ for sparse graphs. ∎

---

## Summary

K-core decomposition reveals a hidden hierarchical structure within networks: the "onion" of increasingly dense core shells. Coreness—a node's depth within this hierarchy—predicts spreading power far better than degree alone. The independent cascade model demonstrates that high-coreness nodes trigger exponentially larger information cascades. Yet the practical breakthrough is the discovery of pseudo-cores: intermediate shells that achieve 85–95% of the innermost core's cascade size while comprising 8–10x more nodes and costing significantly less to access. This synthesis—combining structural analysis (k-cores), dynamics (cascades), and practical strategy (pseudo-core targeting)—provides a principled approach to viral marketing, disease control, organizational influence, and network resilience. The core principle is simple yet profound: **in networks, structural position is destiny**.

---

## Key Takeaways

| Concept | Definition | Real-World Example |
|---------|-----------|-------------------|
| **K-Core** | Maximal subgraph where every node has degree ≥ k | Innermost professional network layer |
| **K-Core Decomposition** | Iterative algorithm peeling nodes with degree < k | Revealing network's hidden hierarchy |
| **Coreness** | Maximum k value for which a node survives in $C_k$ | Depth of embeddedness in dense regions |
| **Degree vs. Coreness** | Local connectivity vs. structural depth | Celebrity (high degree) vs. connector (high coreness) |
| **Independent Cascade** | Discrete-time spreading model with transmission probability | Simulating meme adoption or rumor spread |
| **Cascade Size** | Fraction of network infected before termination | 40% adoption via core seeding vs. 5% via peripheral |
| **Pseudo-Core** | Intermediate shell with ~90% of core's cascade size | Mid-level professionals rivaling celebrities in influence |
| **Echo Chamber Problem** | Targeting only highest-degree nodes reaches same audience repeatedly | Influencer saturation effect in follower overlap |
| **Network Resilience** | Impact of node removal on overall cascading | Removing core nodes reduces cascade by 28–32% |
| **Optimal Strategy** | Seeding diverse, abundant pseudo-cores over rare, expensive core | Viral marketing budget optimization |

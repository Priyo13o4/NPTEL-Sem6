# Week 8: Theoretical Notes — HITS Algorithm & PageRank Matrix Operations

This week elevates the network analysis concepts from previous weeks by introducing **linear algebra** and a competing algorithm to PageRank. While Week 6 introduced PageRank as an intuitive concept (points distribution and random walks), this week reveals the mathematical engine behind it—**matrix operations**—and introduces **HITS (Hyperlink-Induced Topic Search)**, a fundamentally different approach to ranking networks. We explore two critical questions: (1) How does a computer calculate rankings on billion-node networks without crashing? and (2) When is HITS superior to PageRank, and when should we use PageRank instead? The answers involve understanding the mathematical properties of **stochastic matrices**, the mechanics of **iterative convergence**, the problem of **dangling nodes**, and the distinction between local (topic-specific) and global (network-wide) ranking approaches.

---

## 1. The HITS Algorithm: Hubs and Authorities

### The Fundamental Insight

Before Google dominated search with PageRank, **HITS (Hyperlink-Induced Topic Search)**, developed by Jon Kleinberg, proposed a different framework: the internet consists of two distinct types of useful pages, each serving different roles.

**Key Assumption:** A page's value isn't monolithic; it depends on context and role.

### Hub Scores and Authority Scores

HITS assigns **two separate scores** to every node:

**Authority Score:** A measure of the page's valuable **original content**. Authorities are pointed to by many sources.
- Examples: Primary research papers, official government websites, the homepage of a famous creator, expert blog posts
- Intuition: An authority is a destination—somewhere you *want* to be

**Hub Score:** A measure of the page's value as a **curator or directory**. Hubs point to many authorities.
- Examples: Reddit megathreads, link aggregators, review articles, topic-specific directories, curated resource collections
- Intuition: A hub is a guide—something that helps you *find* authorities

### The Mutually Recursive Definition

In HITS, hub and authority scores depend on each other:

**Authority Score Formula:**
$$\text{Authority}(p) = \sum_{q \to p} \text{Hub}(q)$$

A page's authority is the **sum of the hub scores of all pages pointing to it.**

**Hub Score Formula:**
$$\text{Hub}(p) = \sum_{p \to q} \text{Authority}(q)$$

A page's hub score is the **sum of the authority scores of all pages it points to.**

### The Iterative Calculation Process

Because these formulas are mutually dependent, HITS computes them iteratively:

**Algorithm:**
1. **Initialize:** Set all hub scores and authority scores to 1
2. **Authority Update:** Calculate new authority scores using current hub scores
3. **Hub Update:** Calculate new hub scores using new authority scores
4. **Normalization:** Divide all scores by the sum to keep them bounded (prevents scores from growing to infinity)
5. **Repeat:** Continue until scores stabilize (convergence)

**Example (Small Network):**

Network: A → B → C; A → C

Iteration 1:
- Hub(A) = Authority(B) + Authority(C) = 1 + 1 = 2
- Hub(B) = Authority(C) = 1
- Hub(C) = 0 (no outgoing links)
- Authority(A) = 0 (no incoming links)
- Authority(B) = Hub(A) = 2
- Authority(C) = Hub(A) + Hub(B) = 2 + 1 = 3

After normalization (dividing by sum), scores gradually converge to stable values reflecting the network structure.

### Why HITS Fails at Global Scale

**Computational Burden:**
HITS requires computing all pairs of hub-authority interactions, making it $O(n^2)$ in complexity for a network of $n$ nodes. For billion-node networks, this becomes intractable.

**Sensitivity to Graph Structure:**
HITS is extremely sensitive to the network topology. Small changes—adding or removing a few nodes/edges—can drastically shift rankings. This is because:
- HITS lacks a **global normalization baseline**. There's no "random jump" mechanism to provide universal reference.
- HITS amplifies local, densely connected clusters. A tightly connected group of spam pages can artificially inflate their hub/authority scores, making the algorithm vulnerable to **link manipulation**.
- HITS doesn't distribute probability uniformly across the network. Periphery nodes (pages with few connections) can be completely ignored.

**Topic Sensitivity:**
HITS works best when applied to a small, thematically coherent subgraph. For instance:
- Topic: "Renewable Energy" → Extract ~10,000 pages mentioning renewable energy → Run HITS → Excellent results
- Topic: Entire Web → Run HITS → Results heavily biased by spam clusters and local popularity contests

### When HITS is Useful

**Topic-Specific Search:**
For focused queries where you've already filtered the graph to relevant pages, HITS perfectly identifies:
- **Authorities:** Pages with core, authoritative content on the topic
- **Hubs:** Curated collections and review articles that gather topic-specific authorities

**Recommendation Systems:**
HITS identifies both content creators (authorities) and curators (hubs), enabling recommendations like:
- "If you read this authority, you might also enjoy these other authorities linked by this hub"

---

## 2. Matrix Representation of Networks

### From Graph to Adjacency Matrix

To perform calculations at scale, networks must be represented mathematically as **matrices**.

**Adjacency Matrix:**
For a directed graph with $n$ nodes, create an $n \times n$ matrix $A$ where:
$$A[i][j] = \begin{cases} 1 & \text{if edge exists from node } i \text{ to node } j \\ 0 & \text{otherwise} \end{cases}$$

**Example:**
Network: A → B, A → C, B → C

Adjacency Matrix:
$$A = \begin{pmatrix} 0 & 1 & 1 \\ 0 & 0 & 1 \\ 0 & 0 & 0 \end{pmatrix}$$

- Row 0 (Node A): 1 in columns 1 and 2 (edges to B and C)
- Row 1 (Node B): 1 in column 2 (edge to C)
- Row 2 (Node C): all zeros (no outgoing edges)

### The Transition Matrix (Stochastic Matrix)

To model a random walk, convert the adjacency matrix to a **transition matrix** $M$ by normalizing each row by its out-degree (sum).

**Transition Matrix Formula:**
$$M[i][j] = \frac{A[i][j]}{\text{out-degree}(i)}$$

For the example above:
- Node A has out-degree 2 → divide row 0 by 2: [0, 1/2, 1/2]
- Node B has out-degree 1 → divide row 1 by 1: [0, 0, 1]
- Node C has out-degree 0 → problematic! (dangling node)

Partial Transition Matrix (before handling dangling nodes):
$$M = \begin{pmatrix} 0 & 1/2 & 1/2 \\ 0 & 0 & 1 \\ ? & ? & ? \end{pmatrix}$$

### Stochastic Matrices: Probability Conservation

A **stochastic matrix** is one where each row sums to exactly 1. This represents probability conservation—from any node, the random walk must go *somewhere* with 100% certainty.

**Mathematical Property:**
$$\sum_{j=0}^{n-1} M[i][j] = 1 \quad \text{for all rows } i$$

**Physical Interpretation:**
If you're on a webpage, there's a 100% probability you'll do something next (click a link or teleport). The matrix entries represent the *probability* of each action.

**Implication:**
If a stochastic matrix is applied repeatedly to any probability distribution (vector), the result will remain a valid probability distribution—probabilities won't mysteriously vanish or exceed 1.

---

## 3. PageRank as Iterative Matrix Multiplication

### The PageRank Vector

The current PageRank score of every node is stored in a **column vector** $v$ of length $n$:

$$v = \begin{pmatrix} \text{PageRank}(0) \\ \text{PageRank}(1) \\ \vdots \\ \text{PageRank}(n-1) \end{pmatrix}$$

Initially, all nodes start with equal PageRank: $v_0 = (1/n, 1/n, \ldots, 1/n)^T$

### The Update Formula: Matrix Multiplication

Instead of manually distributing points across edges, PageRank uses matrix multiplication:

$$v_{t+1} = M \cdot v_t$$

Where $M$ is the transition matrix and $v_t$ is the PageRank vector at iteration $t$.

**What's happening:**
- Element $[i]$ of the new vector represents the total PageRank flowing into node $i$ from all its predecessors
- By multiplying the entire matrix by the vector, we compute all nodes' updates simultaneously

### Iteration Until Convergence

Repeat the multiplication until the vector stops changing:

$$v_{t+1} = M \cdot v_t$$

When $v_{t+1} \approx v_t$ (within a tolerance), **convergence** is reached, and $v_t$ contains the final PageRank scores.

**Convergence Guarantee:**
The **Perron-Frobenius theorem** (a fundamental result in linear algebra) guarantees that a stochastic matrix applied repeatedly will converge to a unique, stable distribution if the graph satisfies certain properties (discussed below).

### Computational Efficiency

**Why matrix multiplication instead of manual distribution?**
- Manual calculation: $O(n \times m)$ operations per iteration, where $m$ is number of edges
- Matrix multiplication: Modern linear algebra libraries (like BLAS) optimize this to $O(n^2)$ or better, using sophisticated algorithms
- For sparse graphs (most real networks), iterative methods exploit sparsity, making computation practical even for billion-node networks

---

## 4. Convergence, Conservation, and Dangling Nodes

### Conservation of Probability Mass

A fundamental requirement for convergence is **conservation of probability**.

**The Principle:**
If the system starts with a total of 1.0 unit of probability (since probabilities sum to 1), it must end with 1.0 unit. Probability can't be created or destroyed.

**Mathematical Guarantee:**
A stochastic matrix (rows summing to 1) preserves this property. If you multiply a probability vector (entries summing to 1) by a stochastic matrix, the result is a probability vector.

### The Dangling Node Problem

A **dangling node** is a node with **zero outgoing edges** (out-degree = 0).

**The Mathematical Problem:**
In the transition matrix, a dangling node's row is all zeros:
$$M[\text{dangling}] = [0, 0, \ldots, 0]$$

When you multiply this row by the PageRank vector:
$$M[\text{dangling}] \cdot v = 0 + 0 + \cdots + 0 = 0$$

**The Consequence:**
- A dangling node can receive PageRank from incoming edges (incoming nodes distribute to it)
- But it distributes no PageRank to its successors (it has none)
- Probability mass accumulates at dangling nodes and never escapes
- After many iterations, all PageRank drains into dangling nodes, and every other node's rank approaches zero

**Real-World Example:**
On the web:
- PDF documents, images, and dead pages are dangling nodes
- If not handled, 30-40% of web pages would eventually absorb all PageRank
- The entire ranking system collapses

### The Teleportation Fix

To handle dangling nodes, the algorithm assumes a random surfer gets bored at a dangling node and randomly teleports to any webpage.

**Implementation:**
Replace a dangling node's row of zeros with a uniform distribution:
$$M[\text{dangling}] = [1/n, 1/n, \ldots, 1/n]$$

This represents equal probability of teleporting to any of the $n$ nodes.

**Effect:**
- Now every row sums to 1 (stochastic property restored)
- Dangling nodes can distribute PageRank to all other nodes
- Probability mass is conserved and circulates the network

### The Damping Factor (Teleportation Probability)

The full PageRank formula with damping factor $d$ (typically 0.85):

$$M_{\text{damped}} = d \cdot M + (1-d) \cdot \frac{1}{n} \mathbf{J}$$

Where $\mathbf{J}$ is an $n \times n$ matrix of all 1's (divided by $n$ to make each row sum to $(1-d)$).

**Interpretation:**
- With probability $d = 0.85$: Follow an outgoing link (using the transition matrix)
- With probability $1-d = 0.15$: Teleport randomly to any page

**Why this works:**
- Ensures strong connectivity: Every node can eventually reach every other node
- Handles dangling nodes: Surfer doesn't get stuck
- Guarantees convergence: The damping factor ensures the matrix meets conditions for Perron-Frobenius theorem

---

## 5. Convergence Mathematics

### Conditions for Convergence

A stochastic matrix $M$ converges to a unique, stable distribution when applied repeatedly if it is:

1. **Stochastic:** All rows sum to 1 ✓ (enforced by normalization and teleportation handling)
2. **Irreducible:** From any node, you can eventually reach any other node ✓ (ensured by damping/teleportation)
3. **Aperiodic:** The graph doesn't have cycles that trap the walk ✓ (ensured by teleportation and self-loops)

These conditions are met by the damped transition matrix with proper handling of dangling nodes.

### Proof Sketch (Intuitive)

**Why does iteration converge?**

Imagine you're computing: $v_1 = M v_0$, $v_2 = M v_1$, $v_3 = M v_2$, ...

Each iteration represents one step of the random walk. The vector $v_t$ represents the probability distribution of where the surfer is after $t$ steps.

As $t \to \infty$, the surfer has had infinitely many steps to explore. The probability distribution "forgets" where it started and stabilizes to a unique distribution that balances all the edges in the network.

This is the **stationary distribution** or **steady-state**, and it's exactly the PageRank vector.

### Convergence Rate

**How many iterations until convergence?**

This depends on the **spectral gap**—the difference between the largest eigenvalue (which is always 1 for stochastic matrices) and the second-largest eigenvalue.

- **Larger spectral gap** → Faster convergence (typically 20-30 iterations for web graphs)
- **Smaller spectral gap** → Slower convergence (can require hundreds of iterations)

The damping factor affects the spectral gap; higher damping (closer to 1) gives larger spectral gap but makes teleportation less likely, while lower damping (closer to 0) makes the algorithm converge very slowly.

Standard choice: $d = 0.85$ balances convergence speed and physical interpretation.

---

## 6. Global vs. Topic-Specific Ranking

### PageRank: Global Approach

**Advantages:**
- Produces a **single, stable ranking** that applies to the entire network
- Based on global link structure; resistant to local manipulation
- Requires only one computation; results apply to all queries
- Scalable to billion-node networks
- Mathematically well-defined and robust

**Disadvantages:**
- Doesn't understand query context
- A page about "renewable energy" might rank lower than a generic hub even if it's the most authoritative on the specific topic
- Query-independent ranking can be suboptimal for topic-specific searches

### HITS: Topic-Specific Approach

**Advantages:**
- Finds authorities and hubs **within a specific topic**
- Can identify both content creators and curators
- Flexible; different topics get different subgraph rankings
- Conceptually clear: distinguishes roles (hub vs. authority)

**Disadvantages:**
- Must first filter the graph to topic-relevant pages (query-dependent preprocessing)
- Highly sensitive to the filtered subgraph structure
- Computationally expensive; requires new calculation per query
- Vulnerable to manipulation within topic clusters
- Doesn't scale to full-web ranking

### Hybrid Approaches

Modern search engines use:
1. **PageRank** for global site authority (static ranking)
2. **HITS** for topic-specific authority within filtered results
3. **Relevance signals** (text matching, freshness, user signals) combined with both

---

## 7. Manipulation and Robustness

### Why HITS is Vulnerable

HITS lacks global baseline distribution and damping:

**Link Farming:**
A spammer creates 100 fake pages linking to each other in a tight cluster. HITS sees dense interconnection and inflates their mutual hub/authority scores.

**Self-Citation Rings:**
In citation networks, researchers create papers citing only each other's work. Without damping, they artificially inflate their authority.

**Why it happens:**
HITS amplifies local structure. If a cluster is internally dense, HITS assigns it high scores regardless of external connections.

### Why PageRank is Robust

PageRank resists manipulation through:

1. **Damping Factor:** 15% of rank teleports randomly, so even a perfect spam cluster loses 15% of its influence every iteration
2. **Global Normalization:** All nodes compete for a fixed total of 1.0 probability mass. Spam inflating themselves means legitimate pages get less.
3. **Convergence Properties:** The algorithm reaches equilibrium based on fundamental network structure, not local patterns

**Example:**
Spammers create 1,000 pages all linking to each other and their target. In PageRank:
- Yes, they collectively receive some boost
- But with damping, 15% of their collective rank teleports away randomly
- The boost is far less than if they'd gotten genuine links from outside the cluster

---

## Summary

**Key Learning Objectives:**

1. **HITS Algorithm:** Two scores (hub and authority) defined recursively; excellent for topic-specific ranking but poor for global web ranking.

2. **Matrix Representation:** Networks represented as adjacency and transition matrices enable efficient computational algorithms.

3. **Stochastic Matrices:** Matrices with rows summing to 1; essential for modeling probability conservation in random walks.

4. **PageRank as Matrix Multiplication:** $v_{t+1} = M \cdot v_t$ iterates until convergence; reveals PageRank as solving an eigenvector equation.

5. **Dangling Nodes:** Pages with no outgoing links create probability sinks; fixed by teleportation/uniform redistribution.

6. **Convergence Guarantees:** Perron-Frobenius theorem ensures stochastic, irreducible, aperiodic matrices converge to unique stable distribution.

7. **Damping Factor:** Controls probability of following links (typically 0.85) vs. random teleportation (0.15); critical for convergence and robustness.

8. **Global vs. Topic-Specific:** PageRank for network-wide ranking (scalable, stable); HITS for topic-specific authority (sensitive, flexible).

9. **Robustness:** PageRank resistant to link manipulation due to damping and global normalization; HITS vulnerable due to sensitivity to local structure.

---

## Key Takeaways

| Concept | Definition | Application |
|---------|-----------|-------------|
| **Hub Score** | Sum of authority scores of pages/nodes it points to | Finding curators and directories |
| **Authority Score** | Sum of hub scores of pages/nodes pointing to it | Finding authoritative content |
| **HITS Algorithm** | Mutually recursive computation of hubs and authorities | Topic-specific web search, recommendations |
| **Adjacency Matrix** | $n \times n$ matrix; entry $[i,j] = 1$ if edge from $i$ to $j$ | Network representation for computation |
| **Transition Matrix** | Normalized adjacency matrix; rows represent probabilities | Random walk simulation |
| **Stochastic Matrix** | Matrix with all rows summing to 1 | Probability conservation |
| **PageRank Vector** | Column vector of PageRank scores for all nodes | Final ranking output |
| **Matrix Multiplication** | $v_{t+1} = M \cdot v_t$; repeated to convergence | Efficient PageRank computation |
| **Dangling Node** | Node with zero outgoing edges | Probability sink; requires handling |
| **Teleportation** | Random jump to any node; solves dangling node problem | Convergence guarantee; robustness |
| **Damping Factor** | Probability of following a link vs. teleporting (typically 0.85) | Balances convergence and robustness |
| **Convergence** | Point where $v_{t+1} \approx v_t$; stable equilibrium reached | Stopping condition for iterations |
| **Spectral Gap** | Difference between largest and second eigenvalue | Controls convergence speed |
| **Irreducibility** | From any node, can reach any other node eventually | Condition for Perron-Frobenius convergence |
| **Aperiodicity** | No cycles that trap the walk in subgraph | Condition for unique steady state |

---

## Related Lecture Topics

- **Lectures 101–102:** Introduction to Hubs and Authorities, Principle of Repeated Improvement
- **Lectures 103–104:** Repeated Improvement Examples, HITS Algorithm
- **Lectures 105–108:** PageRank Revisited with examples, Convergence demonstration
- **Lectures 109–112:** Matrix operations (multiplication, prerequisites, convergence details)
- **Lectures 113–114:** PageRank as matrix operation, PageRank explained

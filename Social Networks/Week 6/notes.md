# Week 6: Theoretical Notes — Web Graph & PageRank

This week introduces **Google's PageRank**, one of the most important and commercially successful algorithms in computer science history. We transition from undirected social networks to **directed graphs**, where edge direction fundamentally changes power dynamics. PageRank solves a critical problem: not all connections are equal. A hyperlink is a "vote of confidence"—a link from an authoritative source carries far more weight than thousands of links from spam sites. This week explores the theory behind PageRank, two equivalent computational implementations, and its remarkable applicability beyond web search to biology, sports, finance, and social influence.

---

## 1. The Web Graph & The Problem with DegreeRank

### Historical Context

Before Google, search engines ranked websites primarily by counting keywords and measuring **In-Degree Centrality** (the number of incoming hyperlinks), a metric called **DegreeRank**.

### The DegreeRank Problem

DegreeRank is trivially easy to manipulate. To rank your website #1, you could simply write a script to create 10,000 fake websites, all linking to your main site. Your In-Degree becomes 10,000, and you artificially win search rankings.

**The fundamental flaw:** DegreeRank assumes all links are equally valuable. It doesn't distinguish between:
- A link from Wikipedia or the BBC (highly authoritative sources)
- A link from a random spam blog (zero credibility)

### The PageRank Philosophy: Recursive Authority

Larry Page and Sergey Brin solved this by recognizing a key insight: **A node's importance is recursively defined by the importance of the nodes linking to it.**

**The Principle:**
- Getting 1 link from a highly authoritative site is worth more than 10,000 links from low-quality spam sites.
- Importance flows *through* the network: if an important node links to you, some of that importance transfers to you.
- This recursive relationship creates a powerful feedback loop where quality propagates and spam gets filtered.

**Why it works:**
A spam site might link to hundreds of targets, diluting its influence. But an authoritative site links selectively, concentrating its influence on fewer targets. Links from selective, high-authority nodes become extremely valuable because they represent genuine endorsement, not indiscriminate linking.

---

## 2. Implementation Method 1: Points Distribution (The Coin Game)

### The Mechanics of Point Distribution

PageRank can be calculated using an intuitive point distribution system, sometimes called the **"coin game"** or **"flow simulation."**

**Step 1: Initialization**
Every node in the network is given an equal starting amount of "influence points" (e.g., 100 points each). In a network of N nodes, total points = $N \times 100$.

**Step 2: Distribution in Each Iteration**
At each iteration, a node takes its current points and divides them *equally* among all of its outgoing links (out-degree).

**Example:**
- Node A has 300 points and links to 3 websites → gives $300 / 3 = 100$ points to each
- Node B has 300 points and links to 1 website → gives all 300 points to that one site
- Node C has 300 points and links to 5 websites → gives $300 / 5 = 60$ points to each

**The Key Insight:** Links from highly selective nodes (low out-degree) carry much more weight than links from indiscriminate nodes (high out-degree). This naturally rewards quality over quantity.

**Step 3: Iteration Until Convergence**
All nodes distribute points simultaneously. The process repeats dozens of times until the point totals at each node stabilize (reach equilibrium). The final equilibrium values are each node's PageRank score.

### The Sink Node Problem

**What is a sink node?**
A node with *no* outgoing links (out-degree = 0). Examples: a webpage with no hyperlinks, a paper with no citations, a dead-end in the graph.

**The Problem:**
When a sink node receives points from other nodes, it keeps those points and never distributes them forward. Over many iterations, points accumulate in sinks like water in a drain. Eventually, all the network's points get trapped in sinks, and the algorithm breaks down.

### The Damping Factor & Teleportation Solution

To solve the sink node problem, PageRank introduces a **damping factor** (denoted $d$, $s$, or $\alpha$), typically set to **0.85** (though other values like 0.80 or 0.90 are used).

**The Mechanism:**
At each step, a node only distributes **85% of its points** through its outgoing links. The remaining **15% is pooled together** and **redistributed uniformly across every single node** in the network.

**Mathematical Formulation:**
$$P(v) = \frac{1-d}{N} + d \sum_{u: u \to v} \frac{P(u)}{out\text{-}degree(u)}$$

Where:
- $P(v)$ = PageRank of node $v$
- $d$ = damping factor (e.g., 0.85)
- $N$ = total number of nodes
- The sum iterates over all nodes $u$ with outgoing edges to $v$

**Why it works:**
- The $(1-d)/N$ term represents uniform redistribution from the "teleportation pool"
- Every node, including sinks, receives a guaranteed minimum allocation from this pool
- Points continuously circulate instead of getting permanently trapped
- This "tax" or "friction" prevents any node from hoarding the entire network's value

**Interpretation as a Random Surfer:**
Imagine a web surfer clicking random links. With probability $d$ (0.85), they click a hyperlink. With probability $(1-d)$ (0.15), they jump to a random page. This prevents the surfer from getting stuck forever on a dead-end page.

### Example Calculation: One Iteration

**Scenario:**
- Network has 3 pages: A, B, C (each starts with 100 points)
- Links: A → B, A → C (A links to B and C), B → A (B links to A), C → A (C links to A)
- Damping factor: $d = 0.8$

**Distribution (80% through links):**
- A: 100 points ÷ 2 outgoing links = 50 to B, 50 to C
- B: 100 points ÷ 1 outgoing link = 100 to A
- C: 100 points ÷ 1 outgoing link = 100 to A

**Before Teleportation:**
- A receives: 100 (from B) + 100 (from C) = 200 points
- B receives: 50 (from A) = 50 points
- C receives: 50 (from A) = 50 points

**Teleportation (20% distributed equally):**
- Total pool = $(1 - 0.8) \times 300 = 60$ points
- Each node gets $60 / 3 = 20$ points

**Final PageRank After One Iteration:**
- A: $200 + 20 = 220$ points
- B: $50 + 20 = 70$ points
- C: $50 + 20 = 70$ points

This process repeats until the values stabilize.

---

## 3. Implementation Method 2: The Random Walk (The Drunk Surfer)

### The Mechanics of Random Walk Simulation

Instead of mathematically distributing fractions of points, we can **simulate PageRank using probability and a "random surfer" analogy.**

**The Algorithm:**
1. A virtual surfer lands on a random page in the network.
2. From that page, they click a random outgoing link (if links exist).
3. They continue clicking random links, visiting pages in sequence.
4. Every time they land on a page, they drop a "token" or tally mark.
5. Repeat millions of times with different starting pages and paths.

**After Convergence:**
The page with the most tokens is the highest-ranked page. Pages that naturally act as "funnels" (highly connected, authoritative nodes) accumulate tokens faster because random walkers are more likely to visit them.

### Why Random Walk Works

The random walk mimics how information, influence, or power naturally flows through a network. Nodes that are:
- Frequently reachable from other nodes
- Highly connected
- Linked from other important nodes

...will be visited more often by the random walker, naturally accumulating higher PageRank.

### Handling Sink Nodes in Random Walk

When the surfer reaches a page with no outgoing links (a sink), they can't click a link. **Teleportation** solves this: with probability $(1-d)$, the surfer jumps to a random page instead of clicking a link.

This is exactly equivalent to the damping factor in the Points Distribution method—same mechanism, different interpretation.

### Network Coverage: The n log n Relationship

To ensure a random walk thoroughly explores a network of size $n$, ensuring representative sampling of each node's PageRank, the required number of steps is approximately:

$$\text{Steps Required} \approx n \log(n)$$

**Example:** For a network of 8,000 nodes:
$$8000 \times \ln(8000) \approx 8000 \times 8.987 \approx 71,900 \text{ steps}$$

This means millions of discovery journeys are needed for large networks (1 billion+ nodes on the web). Each step is computationally cheap, but the volume is massive.

**Why log(n)?** This comes from the theory of random walks on graphs—visiting each node once requires $O(n)$ steps on average, but ensuring good statistical coverage requires $O(n \log n)$ steps.

---

## 4. PageRank Beyond the Internet

The brilliance of PageRank is its universality. Any system with directed **"vouching" or "endorsement"** can be modeled and ranked using PageRank.

### Academic Citations: Citation Impact Assessment

**Traditional metric:** Citation count (in-degree). A paper with 100 citations ranks higher than one with 20 citations.

**PageRank advantage:** A paper with 20 citations from Nobel laureates and seminal foundational works may have far higher PageRank than a paper with 100 citations from obscure, low-impact journals.

**Real-world finding:** A foundational 1995 neural networks paper with only 45 direct citations achieved the 3rd-highest PageRank in a 500-paper academic network because it was cited by other seminal papers. Meanwhile, a 2020 survey paper with 320 citations ranked 67th because most citing papers came from low-impact venues.

**Key insight:** PageRank identifies "hidden gem" papers—foundational works of enormous intellectual influence that may not have high citation counts.

### Sports Ranking: NCAA Basketball and Tennis

**Graph Construction:**
When Team A defeats Team B, draw a directed edge from B → A (the loser "vouches for" the winner). This represents the winner's authority—they beat someone, so they inherit that opponent's power.

**Interpretation:**
A team that beats strong opponents accumulates more incoming edges from high-authority nodes, boosting their PageRank. This is superior to simple win-loss records because it accounts for **strength of schedule**—beating a powerhouse team is worth more than beating a weak team.

**Real-world application (2013 NCAA):**
- Champion Louisville ranked highly in PageRank
- Final Four teams Michigan and Syracuse also ranked well
- The algorithm couldn't account for injuries, momentum, or coaching changes
- Different damping factors (0.5 vs 0.85 vs 0.9) produced different top-ten orderings

### Biology: GeneRank and Critical Genes

**The Problem:**
Identifying which genes are most critical in a regulatory network is difficult. A gene with many direct incoming connections might be less critical than a gene with few but highly influential incoming connections.

**GeneRank Solution:**
Genes are nodes, regulatory interactions are directed edges. GeneRank identifies "hub genes"—genes whose disruption would most severely impact the entire regulatory system.

**Impact:** Pharmaceutical research uses GeneRank to prioritize drug targets, identifying genes whose dysregulation drives disease.

### Environmental Science: Toxic Accumulation in Food Webs

**The Problem:**
How do toxic chemicals accumulate through ecosystems? Simple poison levels at each organism don't capture the full picture of bioaccumulation.

**PageRank Application:**
Organisms are nodes, feeding relationships are directed edges. Predators receive "contamination flow" from prey. PageRank identifies **apex predators** where toxins accumulate most severely, and also traces **primary sources** where contamination enters the food web.

**Impact:** Environmental protection agencies use this to identify critical intervention points for reducing toxic bioaccumulation.

### Finance: Stock Valuation and Supply Chain Networks

**Application:**
Companies are nodes, supply chain and board member relationships are edges. PageRank identifies economically critical companies whose failure would cascade through the system.

**Real-world use:**
Identifying "too big to fail" nodes (companies whose disruption threatens systemic financial stability) and detecting market-leading companies through supply chain dominance, not just revenue.

### Social Media: Authentic Influence Detection

**The Problem:**
Follower counts are manipulable (bots, purchased followers). An account with 1 million followers might have less real influence than an account with 100,000 authentic followers.

**PageRank Application:**
In influencer networks, directed edges represent endorsements, mentions, or authentic engagement. TrendHub's case: they use PageRank to rank 50,000 content creators. The algorithm identifies creators whose endorsements come from other selective, high-authority creators.

**Key advantage:** 10 endorsements from selective mega-influencers outweighs 500 endorsements from average creators. This filters out bot networks and identifies genuine influence.

---

## Summary

**Key Learning Objectives:**

1. **Web Graph & Link Validation:** PageRank replaces DegreeRank (simple link counting) with recursive authority—links from authoritative sources carry more weight.

2. **Points Distribution:** Nodes distribute influence equally among outgoing links; selective nodes have more impactful links. Damping factor (0.85) solves sink node problems through teleportation.

3. **Random Walk Equivalence:** The drunk surfer algorithm produces identical results to points distribution; requires $n \log n$ steps for thorough network exploration.

4. **Damping Factor Significance:** Different damping values (0.5, 0.80, 0.85, 0.90) produce meaningfully different rankings by controlling the balance between direct influence and global redistribution.

5. **Universal Applicability:** PageRank applies to any directed network: citations, sports, biology, environment, finance, social media.

6. **Quality Over Quantity:** High in-degree (many links/citations) does not guarantee high PageRank; a few high-quality endorsements from authoritative sources outweighs many low-quality ones.

7. **Convergence and Stability:** The algorithm always converges to a unique equilibrium, but convergence rate depends on damping factor and network topology.

---

## Key Takeaways

| Concept | Definition | Application |
|---------|-----------|-------------|
| **PageRank** | Recursive authority metric where node importance depends on incoming node importance | Google search, influence ranking, citation impact |
| **In-Degree vs. PageRank** | In-degree counts links; PageRank weights links by source authority | Distinguishing spam from legitimate sites |
| **Points Distribution** | Nodes split points equally among outgoing links; iterated until convergence | Direct computation of PageRank scores |
| **Damping Factor** | Probability (usually 0.85) of following a link vs. random jump; enables teleportation | Preventing convergence to sinks; balances local vs. global influence |
| **Sink Node** | Node with zero outgoing edges; traps points without damping factor | Webpages with no links, papers with no citations |
| **Random Walk** | Probabilistic simulation: surfer clicks random links, drops tokens on visited pages | Approximates PageRank through sampling; requires n log n steps |
| **Teleportation** | Random jump to arbitrary node instead of following link; solves sink problem | Enables convergence and prevents dead ends |
| **n log n Exploration** | Network of n nodes requires n log n random walk steps for thorough coverage | Estimating sample size for random walk convergence |
| **Authority Flow** | Importance transmitted through network following directed edge direction | Modeling endorsements, citations, supply chains, defeats |
| **Selective Endorsement** | Fewer outgoing connections amplify link value | Influential accounts with selective followings rank higher |

---

## Related Lecture Topics

- **Lectures 75–76**: The Web Graph, collecting web graph data from hyperlink structures
- **Lectures 77–79**: Equal coin distribution, random walk coin distribution, Google PageRank overview
- **Lectures 80–83**: Implementing PageRank using four variations of the points distribution method
- **Lectures 84–85**: Implementing PageRank using two variations of the random walk method
- **Lecture 86**: Comparison of DegreeRank versus PageRank on real and synthetic graphs

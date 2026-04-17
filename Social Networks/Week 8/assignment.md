# Week 8: Assignment 8 — Social Networks (NOC26_CS82)

| Field | Value |
|-------|-------|
| **Due** | 2026-03-18, 23:59 IST |
| **Submitted** | 2026-03-11, 22:04 IST |
| **Total Score** | 14.5/15 |

---

## Case Study 1 — Ranking Influence on a Growing Social Media Platform

A rapidly growing social media platform redesigned its influence ranking using a **directed follow graph** (edge A→B means A follows B). Simple follower count proved flawed—some users followed thousands indiscriminately with little influence, while others had fewer but highly active followers.

The analytics team experimented with the **HITS algorithm**: users who followed many influential accounts = **hubs**; users followed by strong hubs = **authorities**. Content curators and news aggregators emerged as hubs; subject-matter experts as authorities. However, HITS produced unstable results on the full platform graph—rankings fluctuated when users joined or left. HITS worked best on **topic-specific subgraphs**.

**PageRank** was evaluated as an alternative—treating the platform as a random-walk process with attention flowing through follow links. PageRank produced a **single global ranking**, remained stable as the network grew, but required special handling for **dangling nodes** (users who followed no one). The platform ultimately adopted PageRank for global ranking and HITS for topic-specific recommendations.

---

### Q1 (1 point)
**In the HITS framework, a user is considered a strong hub primarily because they:**

- A) Have many followers
- B) Follow many authoritative users
- C) Have high PageRank
- D) Post frequently

**Answer:** B

**Answer Explanation:** In the **HITS mutually recursive definition**, a node's Hub score is completely determined by the Authority scores of the nodes it points to. The formula is: Hub(p) = Sum of Authority scores of pages that p points to. A user is a strong hub because they selectively follow influential, authoritative accounts—not because they have a large follower count. Think of a content curator who carefully selects the smartest people to follow; they become a hub because of the *quality* of who they follow, not the quantity of who follows them. Option A (followers) is a common misconception but doesn't define hub scores in HITS. Options C and D are irrelevant to the hub calculation.

**Related Topic**: [Hub Scores and Authority Scores](notes.md#hub-scores-and-authority-scores)

---

### Q2 (1 point)
**Why did HITS perform poorly when applied to the entire social media graph?** *(Select all that apply)*

- A) It ignores direction of links
- B) It is sensitive to irrelevant parts of the graph
- C) It lacks a global notion of importance
- D) It does not converge

**Answer:** B, C

**Answer Explanation:** HITS has two fundamental limitations at global scale: **(B) Sensitivity to irrelevant parts of the graph** — HITS amplifies local, densely connected clusters. A coordinated group of spam bot accounts could link to each other in a tight cluster, and HITS would artificially inflate their mutual hub/authority scores, making rankings noise-dominated by irrelevant structure. **(C) Lacks global notion of importance** — HITS has no "random baseline" (like PageRank's teleportation) to provide universal reference. PageRank ensures every node gets some minimum standing through random exploration; HITS doesn't. This makes HITS vulnerable to manipulation and local echo chambers. Option A is incorrect—HITS explicitly uses directed links (direction matters). Option D is incorrect—HITS mathematically converges; the problem is *what* it converges to (local amplification rather than balanced global ranking).

**Related Topic**: [Why HITS Fails at Global Scale](notes.md#why-hits-fails-at-global-scale)

---

### Q3 (1 point) — NAT
**If a user is followed by two hubs with hub scores 3 and 5, what is their authority score before normalization?**

**Answer:** 8

**Answer Explanation:** Direct application of the HITS Authority formula. Authority(p) = Sum of Hub scores of all incoming nodes. The user receives Authority from two hubs with scores 3 and 5, so: Authority = 3 + 5 = 8. The phrase "before normalization" is important—during iteration, HITS scales all scores down (dividing by the sum) to prevent infinite growth. But the raw contribution is 8.

**Related Topic**: [The Mutually Recursive Definition](notes.md#the-mutually-recursive-definition)

---

### Q4 (1 point)
**PageRank was preferred for global ranking mainly because it:**

- A) Requires no matrix operations
- B) Considers only local neighborhoods
- C) Uses the entire link structure
- D) Ignores dangling nodes

**Answer:** C

**Answer Explanation:** PageRank's key advantage is that it models the entire network simultaneously through **matrix multiplication**. The transition matrix $M$ encodes all edges; when you multiply $M \cdot v$ iteratively, you're simulating a random walk that can travel anywhere in the network. This global perspective ensures every node gets evaluated based on the entire graph structure, not just local clusters. PageRank considers both direct links and paths multiple hops away, making it resilient to local noise. Option A is incorrect—PageRank fundamentally relies on matrix operations for efficiency. Option B is the opposite; PageRank uses the entire graph, not just local neighborhoods. Option D is incorrect—PageRank must handle dangling nodes properly (though not ignore them).

**Related Topic**: [PageRank as Iterative Matrix Multiplication](notes.md#pagerank-as-iterative-matrix-multiplication)

---

### Q5 (1 point)
**Which mechanisms ensure PageRank convergence?** *(Select all that apply)*

- A) Damping factor less than 1
- B) Handling dangling nodes
- C) Uniform out-degree
- D) Graph symmetry

**Answer:** A, B

**Answer Explanation:** Convergence requires specific mathematical properties (Perron-Frobenius theorem). **(A) Damping factor less than 1** (typically 0.85) ensures the matrix is **aperiodic**—there's no cycle trap. It also creates a "strongly connected" graph by allowing random jumps. **(B) Handling dangling nodes** (replacing zero rows with uniform distribution) ensures the matrix is **stochastic** (all rows sum to 1), preserving probability conservation. These two mechanisms together satisfy the three convergence requirements: stochastic, irreducible (can reach any node from any other), and aperiodic (no trapped cycles). Option C is incorrect—uniform out-degree isn't necessary; sparse graphs converge fine. Option D is incorrect—asymmetric graphs converge perfectly; symmetry isn't required.

**Related Topic**: [Conditions for Convergence](notes.md#conditions-for-convergence)

---

## Case Study 2 — Optimizing Web Search Using Link Analysis

A search engine company crawled billions of webpages, treating each as a node and each hyperlink as a directed edge. Keyword matching proved inadequate for ranking credibility, so the company adopted **link analysis**.

**HITS** was initially implemented—for focused queries like "renewable energy," pages cited by resource hubs (educational portals, research directories) ranked highly. But HITS struggled at scale: each query required building a new subgraph and computing scores in real-time.

**PageRank** modeled users as a random surfer following links with high probability and randomly jumping with a small probability (controlled by a **damping factor**), preventing cycles. **Dangling nodes** (pages with no outgoing links) caused rank accumulation and distorted results unless handled by redistributing rank uniformly, preserving the **stochastic** nature of the transition matrix. PageRank proved robust, scalable, and resistant to minor structural changes.

---

### Q6 (1 point)
**HITS is better suited than PageRank for:**

- A) Large-scale web ranking
- B) Topic-specific searches
- C) Handling dangling nodes
- D) Ensuring convergence

**Answer:** B

**Answer Explanation:** HITS excels at **topic-specific searches** because it isolates and ranks within a narrowly filtered subgraph. For instance: query "renewable energy" → extract 10,000 pages containing "renewable energy" → apply HITS → identify the best authorities on this topic and the best hubs curating them. This separation of roles (authority vs. hub) is powerful when context is clear. PageRank, while excellent for global ranking, doesn't distinguish roles. For topic-specific work, HITS's role distinction is valuable. Option A is opposite—PageRank scales better. Option C is incorrect—PageRank handles dangling nodes better. Option D is incorrect—both converge; it's convergence *to what* that differs.

**Related Topic**: [When HITS is Useful](notes.md#when-hits-is-useful)

---

### Q7 (1 point)
**Which mechanisms help PageRank avoid getting stuck while ranking webpages?** *(Select all that apply)*

- A) Random jumps to other pages
- B) Redistributing rank from pages with no outgoing links
- C) Increasing the number of hyperlinks between pages
- D) Following hyperlinks most of the time

**Answer:** A, B, D

**Answer Explanation:** This perfectly describes the **"Drunk Surfer" model** underlying PageRank. **(A) Random jumps to other pages** — With probability $(1-d) = 0.15$, the surfer ignores links and teleports randomly. This prevents getting trapped in link cycles. **(B) Redistributing rank from pages with no outgoing links** — Dangling nodes (no outgoing links) get a row of uniform probabilities, so their rank distributes to all pages rather than vanishing. **(D) Following hyperlinks most of the time** — With probability $d = 0.85$, the surfer follows actual links, simulating realistic browsing. Together, these ensure the walker can traverse the entire graph. Option C is incorrect—artificially adding hyperlinks isn't part of the algorithm; the structure is fixed.

**Related Topic**: [The Damping Factor (Teleportation Probability)](notes.md#the-damping-factor-teleportation-probability)

---

### Q8 (1 point) — NAT
**If a webpage receives links from three pages with PageRank scores 0.2, 0.1, and 0.3, what is the total incoming contribution before damping?**

**Answer:** 0.6

**Answer Explanation:** In PageRank, a page's incoming contribution (before applying damping) is the sum of contributions from all incoming pages. Assuming these PageRank scores represent the specific fractions those pages allocate to this target node (after dividing by their out-degrees), the total is: 0.2 + 0.1 + 0.3 = 0.6. This is the raw incoming flow; after applying the damping factor $d = 0.85$, this 0.6 value would be weighted and combined with the teleportation contribution $(1-d)/n$.

**Related Topic**: [The Update Formula: Matrix Multiplication](notes.md#the-update-formula-matrix-multiplication)

---

### Q9 (1 point)
**Dangling nodes must be handled because they:**

- A) Increase computation time
- B) Absorb probability mass
- C) Reduce authority scores
- D) Break hub calculations

**Answer:** B

**Answer Explanation:** A dangling node has **out-degree = 0** (no outgoing links). In the transition matrix, its row is all zeros. When you multiply this zero row by the PageRank vector, the result is zero—the node contributes nothing to others. But it *receives* PageRank from its incoming neighbors. Over iterations, PageRank accumulates at dangling nodes and never escapes—they **absorb probability mass like black holes**. If not handled, eventually all probability drains into dangling nodes, and every other page's rank approaches zero. Handling dangling nodes by replacing zero rows with uniform distributions allows them to redistribute their accumulated rank, preserving the stochastic property and preventing probability sinks.

**Related Topic**: [The Dangling Node Problem](notes.md#the-dangling-node-problem)

---

### Q10 (1 point)
**Why must the probabilities of moving from a webpage to other pages sum to 1 in PageRank?**

- A) To represent a valid random walk
- B) To reduce computation time
- C) To limit the number of hyperlinks
- D) To prevent self-links

**Answer:** A

**Answer Explanation:** PageRank models a random walk—the surfer is always *somewhere* and always doing *something* next. From any webpage, the surfer either follows an outgoing link or teleports. The sum of probabilities across all actions must be 100% (or 1.0) because one of these actions *will* occur with certainty. If a row summed to 0.8, it would mean 20% of the time the surfer ceases to exist—a violation of probability theory. This requirement is formalized as the **stochastic property** of the transition matrix, critical for mathematical consistency and convergence guarantees. Options B, C, D are incorrect; they're benefits or side effects, not the fundamental reason for the requirement.

**Related Topic**: [Stochastic Matrices: Probability Conservation](notes.md#stochastic-matrices-probability-conservation)

---

## Case Study 3 — Ranking Research Impact in an Academic Citation Network

A research analytics firm studied an **academic citation network**—each paper is a node, a directed edge from paper A to paper B means A cited B.

Using **HITS**: papers cited by many survey/review articles = **authorities**; review papers citing many influential works = **hubs**. However, HITS rankings varied depending on the subset of papers considered. PageRank provided a stable ranking across the **entire citation graph**.

Some authors attempted manipulation—creating papers that cite many others to inflate hub scores, or using excessive self-citations to boost authority. These had limited impact on PageRank due to its **global normalization and damping**. Papers in well-connected research areas benefited from faster rank propagation; isolated fields showed slower influence growth.

---

### Q11 (1 point)
**In a citation network, an authority paper is one that:**

- A) Cites many papers
- B) Is cited by many influential papers
- C) Has recent publication date
- D) Appears in many conferences

**Answer:** B

**Answer Explanation:** In HITS applied to citation networks, **Authority Score = Sum of Hub scores of pages citing it**. An authority paper is defined by its *incoming* citations, not outgoing ones. Moreover, those incoming citations must come from *influential* sources (papers with high hub scores). A foundational paper cited heavily by other influential papers (like survey articles or highly-cited works) becomes an authority. Option A is incorrect—citing many papers makes you a hub, not an authority. The distinction is fundamental to HITS: hubs point outward; authorities are pointed to.

**Related Topic**: [Hub Scores and Authority Scores](notes.md#hub-scores-and-authority-scores)

---

### Q12 (1 point)
**Which actions can manipulate HITS scores?** *(Select all that apply)*

- A) Creating hub papers citing many works
- B) Excessive self-citations
- C) Increasing paper length
- D) Publishing in niche journals

**Answer:** A, B

**Answer Explanation:** HITS is vulnerable to local manipulation because it lacks global damping. **(A) Creating hub papers citing many works** — An author publishes a "junk review paper" that indiscriminately cites 500 famous papers. This paper's hub score becomes high (sum of all those famous authorities' authority scores), and it artificially inflates the authority scores of all its targets. **(B) Excessive self-citations** — If you're a paper author, you can cite your own previous papers in a coordinated ring. Each self-citation increases your authority score (as a hub pointing to authority) and boosts authority (as being cited by a hub). Without damping, this echo chamber artificially inflates mutual scores. PageRank would suppress this because the damping factor means 15% of the rank teleports away, preventing closed loops from retaining 100% of their influence. Options C and D don't directly affect HITS calculations—length and journal venue don't enter the algorithm.

**Related Topic**: [Why HITS Fails at Global Scale](notes.md#why-hits-fails-at-global-scale)

---

### Q13 (1 point) — NAT
**If a paper is cited by two papers with authority scores 1.5 and 2.5, what is its hub-independent authority contribution before normalization?**

**Answer:** 4.0

**Answer Explanation:** Note: The question wording mentions "authority scores" of the citing papers, but mathematically, incoming citations contribute their **hub scores** (not authority scores) to the target's authority. The question is testing the summation formula. Regardless of the terminology quirk, the math is: Authority contribution = Sum of incoming = 1.5 + 2.5 = 4.0. The "hub-independent" phrase clarifies that we're calculating the raw sum before HITS' iterative feedback between hub and authority.

**Related Topic**: [The Mutually Recursive Definition](notes.md#the-mutually-recursive-definition)

---

### Q14 (1 point)
**PageRank is more resistant to manipulation mainly because it:**

- A) Ignores direction of citations
- B) Uses global normalization and damping
- C) Requires content analysis
- D) Eliminates hubs

**Answer:** B

**Answer Explanation:** **PageRank's resistance stems from damping and global normalization.** Even if 100 spammers create papers citing each other in a closed loop, the damping factor dictates that 15% of the rank from that loop teleports away randomly to the entire citation network. This constant "leakage" prevents the loop from retaining 100% of its collective influence. Additionally, global normalization means all papers compete for a fixed total of 1.0 probability mass. If spammers amplify themselves, they're taking rank from legitimate papers—making the manipulation visible and mathematically bounded. HITS lacks this escape valve; influence can concentrate indefinitely within a tight cluster. Option A is incorrect—PageRank relies on directed edges; ignoring direction would destroy the algorithm. Options C and D are wrong—content analysis isn't involved, and hubs aren't eliminated; PageRank just doesn't distinguish them.
**Related Topic**: [Why PageRank is Robust](notes.md#why-pagerank-is-robust)
---

### Q15 (1 point)
**Which network properties improve ranking stability in PageRank-based systems?** *(Select all that apply)*

- A) High connectivity
- B) Presence of hubs
- C) Uniform degree distribution
- D) Strong community isolation

**Answer:** A, B

**Answer Explanation:** **(A) High connectivity** improves stability because the dense web of connections creates many alternative paths for probability flow. When adding or removing an edge, the global equilibrium shifts minimally because information has many routes to reach all nodes. Low-connectivity networks have bottlenecks; removing a critical edge can dramatically shift rankings. **(B) Presence of hubs** — While PageRank doesn't explicitly compute hub scores, networks with hubs (highly connected nodes) actually improve stability. Hubs act as "reference points" that stabilize the PageRank distribution. In contrast, networks with isolated clusters are sensitive to structural changes because each cluster's ranking depends heavily on internal structure. Option C is incorrect—uniform degree distribution isn't necessary; real networks are power-law distributed. Option D is opposite—strong isolation creates instability; networks benefit from cross-cluster connections.

**Related Topic**: [Why PageRank is Robust](notes.md#why-pagerank-is-robust)

---

## Answer Key Summary

| Q | Answer | Concept Tested |
|---|--------|----------------|
| 1 | B | HITS hub definition: sum of authority scores of outgoing neighbors |
| 2 | B, C | HITS limitations: sensitivity to irrelevant clusters, lack of global baseline |
| 3 | 8 | HITS authority calculation: sum of incoming hub scores |
| 4 | C | PageRank advantage: uses entire link structure through matrix operations |
| 5 | A, B | PageRank convergence: damping factor + dangling node handling |
| 6 | B | HITS strength: topic-specific ranking and role separation |
| 7 | A, B, D | PageRank mechanisms: random jumps, dangling redistribution, link following |
| 8 | 0.6 | PageRank incoming contribution: sum of fractions from incoming nodes |
| 9 | B | Dangling nodes: absorb probability mass without proper handling |
| 10 | A | Stochastic requirement: probabilities sum to 1 for valid random walk |
| 11 | B | Authority in citations: cited by many influential papers |
| 12 | A, B | HITS manipulation: creating hubs with many citations, self-citation rings |
| 13 | 4.0 | HITS authority sum calculation: 1.5 + 2.5 |
| 14 | B | PageRank robustness: damping prevents concentrated influence |
| 15 | A, B | Stability factors: high connectivity and hub presence |

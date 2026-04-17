# Week 2: Assignment 2 — Social Networks (NOC26_CS82)

> **Due:** 2026-02-04, 23:59 IST
> **Submitted:** 2026-02-01, 20:55 IST
> **Total Score:** 15/15

---

## Case Study 1 — Centrality and Clustering in a Global Ingredients Network

A multinational food technology company maintains a culinary knowledge base of over **1.5 million recipes**. Each ingredient is treated as a node, and an **undirected connection** is formed between two ingredients if they appear together in a statistically significant number of recipes. The network grew to include more than **18,000 ingredients** and several million connections.

Ingredients like **salt, onion, garlic, and oil** had exceptionally high connections. Ingredients like **soy sauce** or **cumin** appeared in fusion dishes, connecting otherwise separate clusters. Certain groups like Italian, South Indian, or Middle Eastern cuisines formed tightly interconnected clusters.

The company wanted to identify:
1. Ingredients that were globally influential
2. Ingredients that connected different culinary traditions
3. Structural patterns that explained why some cuisines felt cohesive while others blended easily

---

### Q1 (1 point)
**Ingredients like salt and oil having a large number of connections primarily indicates:**

- A) High clustering within a single cuisine
- B) Frequent co-occurrence across many recipes
- C) Strong bridging between distant clusters
- D) Low overall influence

**Answer:** B

**Answer Explanation**: In an ingredient network, an edge represents co-occurrence. If salt has massive connections, it simply means it shares a recipe with almost every other ingredient in the database. This is the definition of high **Degree Centrality**.

---

### Q2 (1 point)
**An ingredient with relatively few connections but crucial links between cuisine clusters is most significant because it:**

- A) Appears rarely
- B) Forms tightly-knit local groups
- C) Connects otherwise separate regions of the network
- D) Reduces the size of clusters

**Answer:** C

**Answer Explanation**: This perfectly describes **Betweenness Centrality**. An ingredient like "soy sauce" in a fusion dataset might not be in every dish (low degree), but it acts as the vital bridge between the "Asian" cluster and the "Western" cluster.

---

### Q3 (1 point)
**A) Recipes are uniformly distributed**
- B) Ingredients combine randomly
- C) Certain cuisines form dense substructures
- D) All ingredients serve the same role

**Answer:** C
- [x] **C) Certain cuisines form dense substructures** ✅
- [ ] D) All ingredients serve the same role

**Answer Explanation**: This refers to the **Clustering Coefficient**. High clustering means the network isn't a random soup of edges; it has defined, dense communities (like a highly interconnected Italian cuisine sub-graph).

---

### Q4 (1 point)
**A) Counting total appearances**
- B) Finding ingredients within the same dense clusters
- C) Measuring shortest paths across the entire network
- D) Removing highly connected ingredients

**Answer:** B dense clusters** ✅
- [ ] C) Measuring shortest paths across the entire network
- [ ] D) Removing highly connected ingredients

**Answer Explanation**: If two ingredients share almost the exact same neighbors (they are in the same dense community), they serve the same culinary function and can likely be substituted for one another.

---
A) Most nodes have similar connectivity
- B) A few nodes dominate connections while many remain peripheral
- C) Nodes form a strict hierarchy
- D) Connections grow linearly

**Answer:** B
- [ ] A) Most nodes have similar connectivity
- [x] **B) A few nodes dominate connections while many remain peripheral** ✅
- [ ] C) Nodes form a strict hierarchy
- [ ] D) Connections grow linearly

**Answer Explanation**: This is the definition of a **Scale-Free Network**. Salt and garlic are the massive hubs, while obscure ingredients like "dragon fruit powder" are peripheral nodes with only one or two links.

---

## Case Study 2 — Influence and Structure in a Large-Scale Synonym Network

A computational linguistics research group built a **synonym network** of approximately **42,000 words**. Each word is a node, and an **undirected edge** is created if two words are synonyms in at least two independent sources. Using **Python and NetworkX**, they observed that words like **good, big,** and **fast** had very high connections.

Some words with **low degree** still appeared on a large number of shortest paths — removing a few of these caused the network to split into disconnected components. Community detection revealed clusters corresponding to everyday language, technical usage, and literary contexts. The degree distribution showed most words had few connections while a small number had extremely many.

---

### Q6 (1 point)
**Words like *good* having many synonym connections would most directly result in high values of:**

- A) Clustering coefficient
- B) Degree centrality
- C) Network diameter
- D) Path length

**Answer:** B

**Answer Explanation**: Again, just counting direct neighbors. A highly versatile word has many synonyms, meaning a high raw count of edges.

---

### Q7 (1 point)
**A word with low degree but appearing on many shortest paths is important primarily because it:**

- A) Appears frequently in text
- B) Belongs to a dense cluster
- C) Connects different groups of words
- D) Has high clustering

**Answer:** C

**Answer Explanation**: Exactly the same concept as Q2. This is **Betweenness Centrality**. A word might bridge the "academic" jargon community with the "slang" community.

---

### Q8 (1 point)
**A) High PageRank
- B) High betweenness
- C) High clustering
- D) Low degree

**Answer:** Bweenness** ✅
- [ ] C) High clustering
- [ ] D) Low degree

**Answer Explanation**: If a node sits on all the shortest paths between two massive clusters, removing it severs the only bridge, shattering the network into disconnected components.

---

### Q9 (1 point)
**A) Shortest path computation
- B) Community detection
- C) Degree sorting
- D) BFS traversal

**Answer:** Betection** ✅
- [ ] C) Degree sorting
- [ ] D) BFS traversal

**Answer Explanation**: Community detection algorithms specifically hunt for dense sub-graphs (clusters where nodes are highly connected to each other but not to the outside), which in linguistics maps perfectly to semantic domains or themes.

---
A) Uniform structure
- B) Random network
- C) Presence of hubs
- D) Tree-like structure

**Answer:** C
- [ ] A) Uniform structure
- [ ] B) Random network
- [x] **C) Presence of hubs** ✅
- [ ] D) Tree-like structure

**Answer Explanation**: This is the signature "heavy tail" of a scale-free network, proving that language relies heavily on a few extremely versatile anchor words (hubs).

---

## Case Study 3 — Tracking Influence and Visibility in the Web Graph

Data scientists analyzed a snapshot of the **web graph** containing **1.1 million webpages** and over **30 million hyperlinks**. Each page is a node and each hyperlink a **directed edge**. They computed **PageRank values** using a damping factor of **0.85** and observed that encyclopedic resources and government portals consistently ranked higher.

A large number of pages had **no outgoing links**, requiring special handling to prevent rank accumulation. The team compared PageRank ranking with simple link-count ranking and found significant differences.

---

### Q11 (1 point)
**A webpage receiving links from already influential pages gains higher visibility mainly because PageRank considers:**

- A) Total number of users
- B) Number of outgoing links
- C) Quality of incoming links
- D) Age of the webpage

**Answer:** C

**Answer Explanation**: PageRank is fundamentally a measure of **quality**, not just quantity. A link from a node with a high PageRank score passes a fraction of that high score down to you.

---

### Q12 (1 point)
**Pages with no outgoing links require special handling because they:**

- A) Reduce network size
- B) Absorb rank without redistributing it
- C) Increase clustering
- D) Break directed paths

**Answer:** B

**Answer Explanation**: These are **Rank Sinks**. The PageRank algorithm simulates rank flowing from node to node. If a node has no outbound edges, the mathematical flow stops there, trapping the mathematical value.

---

### Q13 (1 point)
**A) Ignores direction of links
- B) Treats all links equally
- C) Considers influence of linking pages
- D) Penalizes popular pages

**Answer:** Cy
- [x] **C) Considers influence of linking pages** ✅
- [ ] D) Penalizes popular pages

**Answer Explanation**: Simple link counting is just In-Degree centrality (which is easy to manipulate via spam). PageRank looks at the **weight** and **importance** of the linking nodes to prevent spam and surface genuinely authoritative content.

---

### Q14 (1 point)
**A) Adding self-links
- B) Getting links from authoritative sites
- C) Removing all outgoing links
- D) Increasing page size

**Answer:** B authoritative sites** ✅
- [ ] C) Removing all outgoing links
- [ ] D) Increasing page size

**Answer Explanation**: Because of the recursive formula, receiving an edge from a node that already possesses high PageRank will exponentially boost your own score compared to adding random self-links or changing your own outgoing structure.

---
A) Tree
- B) Undirected random graph
- C) Directed network with hubs
- D) Complete graph

**Answer:** C
- [ ] A) Tree
- [ ] B) Undirected random graph
- [x] **C) Directed network with hubs** ✅
- [ ] D) Complete graph

**Answer Explanation**: Hyperlinks have a strict direction (A points to B), and the internet is heavily reliant on massive centralized hubs (like Google, Wikipedia, Amazon) surrounded by millions of peripheral sites.

---

# ✅ Answer Key Summary

| Q | Answer | Concept |
|---|--------|---------|
| 1 | B | Degree Centrality |
| 2 | C | Betweenness Centrality |
| 3 | C | Clustering Coefficient |
| 4 | B | Community Detection |
| 5 | B | Scale-Free Networks |
| 6 | B | Degree Centrality |
| 7 | C | Betweenness Centrality |
| 8 | B | High Betweenness (Network Fragility) |
| 9 | B | Community Detection |
| 10 | C | Scale-Free Networks & Hubs |
| 11 | C | PageRank (Quality of Links) |
| 12 | B | Rank Sinks |
| 13 | C | PageRank vs. Link Counting |
| 14 | B | PageRank Improvement |
| 15 | C | Directed Scale-Free Networks |

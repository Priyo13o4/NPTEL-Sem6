# Week 11: Assignment 11 — Social Networks (NOC26_CS82)

**Theme:** Small World Phenomenon, Milgram's Experiment, Decentralized Search & Kleinberg's Model  
**Due:** 2026-04-08, 23:59 IST  
**Submitted:** 2026-04-05, 23:48 IST  
**Total Score:** 13.67/15

---

## Case Study 1 — Building a Global Alumni Outreach Without a Central Directory

A large public university with over 300,000 alumni across 90+ countries sought to create a mentorship and outreach program. The challenge: no centralized, reliable alumni directory. Instead of a database, the university relied on existing social connections. A student seeking mentorship contacts someone they personally knew, who forwards the request to someone closer to the intended mentor—using only local judgment based on shared discipline, geographic proximity, industry, or past experience.

Pilot trials revealed many requests reached the intended alumnus in fewer than 7 intermediate steps, reflecting Six Degrees of Separation. Analysis showed strong local clustering (within departments, cohorts, regional groups) with occasional cross-cluster transitions through individuals who had studied abroad or worked multinationally. In regions with few external connections, requests stalled; regions with even a small number of cross-regional connections saw significantly higher success rates.

---

### Q1 (1 point)

**The alumni outreach system works effectively primarily because:**

- A) The alumni network is centrally managed
- B) Short paths exist and can be found using local decisions
- C) Everyone knows a large number of alumni
- D) Alumni connections are uniformly random

**Answer:** B

**Answer Explanation:** The alumni network succeeds through the combination of two properties discovered by Milgram: the existence of short paths globally, and the ability of local navigators (alumni forwarding requests) to discover them without a central directory. Option A is false—the system explicitly avoids centralization. Option C is false—most alumni know relatively few people. Option D is false—as discussed in Kleinberg's model, uniformly random connections would cause requests to overshoot targets. The system works precisely because individual alumni can make local decisions (forwarding to someone they judge closer to the target in discipline, geography, or career field) that collectively navigate the small-world structure. This directly tests the combined insights from notes sections **"Milgram's Experiment: The Small World Phenomenon"** and **"The Decentralized Search Paradox"**.

**Related Topic:** [Milgram's Experiment: The Small World Phenomenon](notes.md#milgrams-experiment-the-small-world-phenomenon)

---

### Q2 (1 point)

**Which properties of the alumni network enable requests to reach distant mentors efficiently?** *(Select all that apply)*

- A) Local clustering among similar alumni
- B) Presence of a few long-range connections
- C) Complete connectivity across regions
- D) Ability of individuals to make local forwarding decisions

**Answer:** A, B, D

**Answer Explanation:** This question tests the structural foundation of searchable networks. Option A (local clustering) is essential—alumni within the same department or cohort naturally form dense clusters, creating local pathways within regions. Option B (occasional long-range connections) is critical—alumni who studied abroad or worked internationally bridge distant clusters, providing the "shortcuts" essential to small-world structure. Option D (local decision-making) is fundamental to decentralized search—each alumnus can estimate who in their personal network is "closer" to the target in relevant dimensions. Option C is false because complete connectivity (everyone knowing everyone) is unnecessary and unrealistic; small-world networks achieve efficiency with sparse, strategically distributed connections. This tests understanding of Watts-Strogatz structure (clustering + weak ties) and Kleinberg's insight (distance-aware forwarding) from notes sections **"The Watts-Strogatz Model"** and **"Kleinberg's Model"**.

**Related Topic:** [The Watts-Strogatz Model: Creating Small Worlds](notes.md#3-the-watts-strogatz-model-creating-small-worlds)

---

### Q3 (1 point)

**The forwarding strategy used by participants most closely resembles:**

- A) Random forwarding
- B) Centralized routing
- C) Greedy decentralized search
- D) Exhaustive search

**Answer:** C

**Answer Explanation:** In computer science, a "greedy" algorithm makes the optimal choice at each step based on **current local information**, without computing the entire path. Alumni forwarding requests to whoever they judge closest to the target—using only personal knowledge—exemplifies greedy decentralized search. Option A is wrong because it's not random; participants deliberately choose their "best" candidate. Option B is wrong—there's no central authority. Option D is wrong because alumni don't exhaust all possible paths. The alumni strategy perfectly matches the definition of greedy search discussed in notes sections **"The Decentralized Search Paradox"** and case studies throughout.

**Related Topic:** [The Decentralized Search Paradox](notes.md#4-the-decentralized-search-paradox)

---

### Q4 (1 point) — NAT

**If a request passes through 6 intermediaries before reaching the mentor, how many individuals were involved in the chain (including sender and receiver)?**

**Answer: 8**

**Explanation:** A request chain consists of: 1 sender + 6 intermediaries + 1 receiver = 8 total individuals. This simple counting problem reinforces Milgram's "six degrees of separation" concept. The six degrees refer to the number of **hops** (intermediaries), not the total number of people involved. This calculation connects directly to notes section **"Milgram's Experiment"**, which discusses the six-step average path length found in Milgram's packages.

**Related Topic:** [Milgram's Experiment: The Small World Phenomenon](notes.md#milgrams-experiment-the-small-world-phenomenon)

---

### Q5 (1 point)

**Why do requests sometimes stall within certain alumni clusters?**

- A) Alumni refuse to forward messages
- B) The clusters lack weak ties to other groups
- C) The network is disconnected
- D) Individuals choose randomly

**Answer:** B

**Answer Explanation:** A cluster becomes a "dead end" when it has no **weak ties** (long-range connections) to other clusters. If a cluster of alumni from the same department only know each other and have zero connections to other departments or geographic regions, a request targeting someone outside the cluster cannot escape. This is precisely the problem that Watts-Strogatz solved with rewiring—the strong local clustering that makes local navigation efficient becomes a prison without occasional long-range bridges. Option A is false—alumni are presumably willing to help. Option C is false—the overall network isn't disconnected, but the local cluster is isolated. Option D is false—the problem isn't randomness but the lack of outgoing connections. This tests Kleinberg's insight that **distance-aware weak ties are essential**, covered in notes sections **"The Watts-Strogatz Model"** and **"Kleinberg's Model"**.

**Related Topic:** [Kleinberg's Model: Distance-Sensitive Long-Range Connections](notes.md#5-kleinbergs-model-distance-sensitive-long-range-connections)

---

## Case Study 2 — Emergency Resource Routing During a Natural Disaster

Following a large-scale natural disaster affecting multiple coastal districts, relief organizations faced critical coordination challenges—damaged roads, partial communication infrastructure, no real-time global overview of supplies. Relief efforts relied on a network of local coordinators (district officers, hospital administrators, NGO volunteers, logistics partners). Each coordinator maintained frequent contact with nearby coordinators, plus a limited number of long-range contacts from previous collaborations.

When a local shortage was identified, requests were forwarded coordinator-to-coordinator using only local knowledge. Three contrasting outcomes: (1) Only local contacts → requests stalled, (2) Indiscriminate random long-range jumps → requests overshot relevant regions causing delays, (3) Structured long-range links reflecting distance and relevance → efficient cross-regional routing. Consistent with Jon Kleinberg's findings.

---

### Q6 (1 point)

**The relief network demonstrates that efficient routing requires:**

- A) Maximum number of long-range links
- B) No long-range links
- C) A balanced number of distance-aware long-range links
- D) Complete global knowledge

**Answer:** C

**Answer Explanation:** This question directly tests Kleinberg's central insight. Option A is wrong—too many long-range links cause overshooting and routing confusion (scenario 2 in the case study). Option B is wrong—without long-range connections, requests stall (scenario 1). Option D is wrong—the whole point is to avoid centralized global knowledge. Option C is correct: Kleinberg proved that **distance-aware long-range links** (reflecting geographic proximity and relevance—nearby districts, similar organizational roles) enable efficient routing. The exponent $\alpha$ in $P \propto 1/d^\alpha$ must equal the spatial dimension ($\alpha = 1$ for a 1D relief network distributed across a coastline) to optimize both connectivity and searchability. This is the core of notes section **"Kleinberg's Model"**.

**Related Topic:** [Kleinberg's Model: Distance-Sensitive Long-Range Connections](notes.md#5-kleinbergs-model-distance-sensitive-long-range-connections)

---

### Q7 (1 point)

**Which scenarios are likely to increase routing delays?** *(Select all that apply)*

- A) Only local connections exist
- B) Long-range connections ignore distance
- C) Coordinators follow a greedy local strategy
- D) The network has weak ties placed carefully

**Answer:** A, B

**Answer Explanation:** Option A causes delays because without long-range links, requests must travel step-by-step across the entire region, like moving through a pure lattice. Option B causes delays because random long-range links create the overshooting problem: a request jumps to a region far from the target, then has no efficient way back. Option C is neutral or beneficial—greedy routing is actually the correct strategy when the network is properly structured (as in Kleinberg's model). Option D is beneficial—carefully placed weak ties (distance-aware, reflecting relevance) are exactly what enable efficient routing. This question contrasts the failure modes of pure lattices (A) and random graphs (B) with the success of structured networks, as discussed in notes sections **"Network Structures"** and **"The Decentralized Search Paradox"**.

**Related Topic:** [The Decentralized Search Paradox](notes.md#4-the-decentralized-search-paradox)

---

### Q8 (1 point)

**The routing strategy used by coordinators assumes that:**

- A) Each node knows the full network
- B) Distance to destination can be locally estimated
- C) The network is acyclic
- D) All paths are equally short

**Answer:** B

**Answer Explanation:** Greedy decentralized routing fundamentally requires that each node can estimate which neighbor is "closer" to the target using only local information. For the relief network, a district officer can estimate: "Is the shortage in the coastal north, or the inland south? Which of my contacts are better positioned for each region?" This local distance estimation enables globally efficient routing. Option A is false—decentralized algorithms avoid global knowledge. Option C is false—routing works fine on graphs with cycles. Option D is false—the whole point is that some paths are shorter, and local decisions should bias toward them. This tests the core assumption of Kleinberg's searchability model, covered in notes section **"Kleinberg's Model"** and the discussion of greedy routing in **"The Decentralized Search Paradox"**.

**Related Topic:** [Kleinberg's Model: Distance-Sensitive Long-Range Connections](notes.md#5-kleinbergs-model-distance-sensitive-long-range-connections)

---

### Q9 (1 point) — NAT

**If increasing a parameter controlling distance sensitivity makes long-range links extremely rare, what happens to routing efficiency?**

**Answer: decreases**

**Explanation:** In Kleinberg's model, the distance exponent $\alpha$ controls the strength of the distance bias. As $\alpha$ increases, the probability of long-range connections drops sharply. In the extreme ($\alpha \to \infty$), coordinators have almost no long-range contacts—the network becomes a pure local lattice. In this regime, requests cannot make the global "jumps" needed to cross large distances efficiently. Path length reverts from $O((\log n)^2)$ (optimal searchability) to $O(\sqrt{n})$ (lattice-like), dramatically decreasing routing efficiency. This is the "over-biased" regime discussed in notes section **"Kleinberg's Model: The Role of Alpha"**. The optimal value is $\alpha = d$ (for a $d$-dimensional network), which balances global and local links.

**Related Topic:** [Kleinberg's Model: Distance-Sensitive Long-Range Connections](notes.md#5-kleinbergs-model-distance-sensitive-long-range-connections)

---

### Q10 (1 point)

**This case best illustrates the difference between:**

- A) Connectivity and randomness
- B) Shortest paths and discoverable paths
- C) Centralized and distributed databases
- D) Dense and sparse graphs

**Answer:** B

**Answer Explanation:** The emergency relief case reveals the fundamental paradox solved by Kleinberg: in scenario 2, short paths might exist (connectivity), yet the random long-range links prevent greedy search from finding them (discoverability). Scenario 3 solves this by structuring long-range links, making paths both short and discoverable. Option A conflates connectivity with randomness—the question is about navigability. Option C is about infrastructure architecture, not network topology. Option D is about density, not navigation. The real insight, central to Week 11, is that **the existence of short paths is different from the ability to discover them through local decisions**. This distinction is explicitly discussed in notes section **"Searchability vs. Connectivity"**.

**Related Topic:** [Searchability vs. Connectivity: The Core Distinction](notes.md#6-searchability-vs-connectivity-the-core-distinction)

---

## Case Study 3 — Designing a Searchable Peer-to-Peer Knowledge Platform

A peer-to-peer knowledge-sharing platform allowed users to seek expert guidance via trust-based forwarding—no centralized recommendation engine. Users belonged to overlapping communities (professional domain, geography, education, shared interests), forming dense local clusters. Three network designs tested:

Almost entirely local connections → strong cohesion, slow discovery of distant experts; Many random long-range connections → reduced distances but caused routing confusion; Structured long-range connections (likelihood decreasing with social/topical distance) → consistently led questions to experts in few steps. Only when long-range links were balanced—rare but appropriately distributed—did the network become both small and searchable. Consistent with Kleinberg's findings: searchability requires network structure to align with local decision-making.

---

### Q11 (1 point)

**The key factor that made the platform searchable was:**

- A) High clustering
- B) Existence of short paths
- C) Distance-aware distribution of long-range links
- D) Large number of connections per user

**Answer:** C

**Answer Explanation:** Options A and B are necessary but insufficient—the case shows that a purely local network has high clustering but is unsearchable (slow discovery). Similarly, random networks might have short paths but confuse local routing. Option D (more connections) increases clutter without solving the core navigation problem. Option C is the key differentiator: only when long-range connections reflect **social/topical distance**—linking experts in the same domain but different geography, or different domains but same location—do users find experts through greedy forwarding. This embodies Kleinberg's principle that $P(link) \propto 1/d^\alpha$ where distance reflects relevant dimensions (domain expertise, location) and $\alpha$ is calibrated appropriately. This directly tests notes section **"Kleinberg's Model"** and the distinction discussed in **"Searchability vs. Connectivity"**.

**Related Topic:** [Kleinberg's Model: Distance-Sensitive Long-Range Connections](notes.md#5-kleinbergs-model-distance-sensitive-long-range-connections)

---

### Q12 (1 point)

**Which conditions lead to poor decentralized search performance?** *(Select all that apply)*

- A) Uniformly random long-range links
- B) Excessively local connections
- C) Distance-sensitive weak ties
- D) Balanced mix of local and long-range links

**Answer:** A, B

**Answer Explanation:** Option A (random links) causes overshooting and routing confusion, as shown in the "many random long-range connections" scenario of the case. Option B (purely local) prevents global jumps, causing slow discovery, as shown in the "almost entirely local" scenario. Option C is beneficial—distance-sensitive weak ties are exactly what Kleinberg recommends. Option D is also beneficial—a balanced mix is optimal. This question reinforces the twin pitfalls: either the network is too local (no searchability due to long paths) or randomness is unstructured (no searchability due to overshooting). The solution requires distance-awareness. This tests the comparison between failure modes in notes sections **"The Decentralized Search Paradox"** and success in **"Kleinberg's Model"**.

**Related Topic:** [The Decentralized Search Paradox](notes.md#4-the-decentralized-search-paradox)

---

### Q13 (1 point)

**Even when short paths exist, search can fail because:**

- A) Users lack motivation
- B) Paths are too long
- C) Local decisions may not align with global structure
- D) The network is disconnected

**Answer:** C

**Answer Explanation:** This question isolates the core paradox that Kleinberg solved. Short paths can exist globally (as in random graphs or Watts-Strogatz with random rewiring), yet a decentralized agent—forwarding to whoever seems locally closest—might never find them. For example, an expert might be only 3 hops away globally, but if the 3 paths all require moving through unfamiliar domains or irrelevant geographic regions, a user's local greedy choices repeatedly pick wrong directions, leading to overshooting and dead ends. Option A is demotivational, not structural. Option B contradicts the premise. Option D is false—the network is connected, but locally unnavigable. Option C captures the exact distinction between connectivity and searchability. This is the central insight of notes section **"The Decentralized Search Paradox"** and the motivation for **"Kleinberg's Model"**.

**Related Topic:** [The Decentralized Search Paradox](notes.md#4-the-decentralized-search-paradox)

---

### Q14 (1 point) — NAT

**In a two-dimensional network, which value of the distance exponent yields optimal decentralized search?**

**Answer: 2**

**Explanation:** Kleinberg's fundamental result proves that for a $d$-dimensional lattice (regular grid), optimal decentralized search occurs when the distance exponent $\alpha = d$. For a 2D network, $\alpha = 2$ creates the perfectly balanced distribution of long-range links: a few at global scale, some at regional scale, many at neighborhood scale. This multi-scale structure allows greedy routing to consistently halve the distance to the target, achieving near-logarithmic path length. For a 1D network (linear arrangement), optimal $\alpha = 1$. For 3D networks, optimal $\alpha = 3$. This critical result is central to notes section **"Kleinberg's Model: The Role of Alpha"** and the mathematical discussion in **"The Mathematics of Multi-Scale Structure"**.

**Related Topic:** [Kleinberg's Model: Distance-Sensitive Long-Range Connections](notes.md#5-kleinbergs-model-distance-sensitive-long-range-connections)

---

### Q15 (1 point)

**This case study most directly validates which idea?**

- A) Homophily alone explains navigation
- B) Weak ties alone guarantee search
- C) Small-world networks are always searchable
- D) Searchability depends on structured randomness

**Answer:** D

**Answer Explanation:** The case demonstrates the necessity of **structured randomness**—long-range links that are neither purely random (which confuses routing) nor purely local (which lacks connectivity). Option A is incomplete—homophily (similarity preference) drives local clustering but isn't sufficient for navigation. Option B is incomplete—weak ties are necessary but only work when appropriately distributed. Option C is false—Watts-Strogatz small-worlds with random rewiring are not searchable by greedy algorithms. Option D captures the complete insight: **searchability emerges from the precise interplay of randomness (links exist across distance) and structure (links are biased by distance in a dimension-matched way)**. This is Kleinberg's central contribution and is discussed throughout notes sections **"Kleinberg's Model"** and **"Searchability vs. Connectivity"**.

**Related Topic:** [Searchability vs. Connectivity: The Core Distinction](notes.md#6-searchability-vs-connectivity-the-core-distinction)

---

## Answer Key Summary

| Q | Answer | Concept Tested |
|---|--------|---|
| 1 | B | Milgram's Small World + Decentralized Navigation |
| 2 | A, B, D | Watts-Strogatz Structure (Clustering + Weak Ties) |
| 3 | C | Greedy Decentralized Search Algorithm |
| 4 | 8 | Six Degrees Counting (Hops vs. People) |
| 5 | B | Weak Ties & Cluster Isolation Problem |
| 6 | C | Kleinberg's Distance-Sensitive Links (Balanced Approach) |
| 7 | A, B | Failure Modes: Local-Only vs. Random-Only |
| 8 | B | Local Distance Estimation Assumption |
| 9 | decreases | Over-Biased Alpha → Loss of Long-Range Links |
| 10 | B | Connectivity vs. Discoverability Distinction |
| 11 | C | Distance-Aware Distribution Critical Factor |
| 12 | A, B | Random vs. Local-Only Both Fail |
| 13 | C | Local Decisions ≠ Global Alignment Paradox |
| 14 | 2 | Optimal Exponent for 2D Network ($\alpha = d$) |
| 15 | D | Structured Randomness as Core Principle |

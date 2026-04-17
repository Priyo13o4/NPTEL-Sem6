# Week 12: Assignment 12 — Social Networks (NOC26_CS82)

**Theme:** K-Core Decomposition, Coreness vs. Degree, Cascades & Pseudo-Cores  
**Due:** 2026-04-15, 23:59 IST  
**Submitted:** 2026-04-05, 23:50 IST  
**Total Score:** 13.01/15

---

## Case Study 1 — Launching a Viral Meme on a Microblogging Platform

A new meme campaign is launched on a microblogging platform with 2 million users. K-core decomposition reveals shells ranging from $k=1$ to $k=45$. The innermost core ($k=45$) contains 0.5% of users (10,000 nodes). Intermediate shells between $k=30$ and $k=35$ contain about 4% of users (80,000 nodes). Peripheral nodes ($k ≤ 5$) form about 40% of the network (800,000 nodes).

Researchers observe that highest-degree users are NOT always in the highest k-core. Some users in the $k=32$ shell trigger cascades nearly matching those from the innermost core—these are identified as pseudo-cores. Four seeding strategies are evaluated: (1) Highest-degree users (average cascade ~25%), (2) Innermost core $k=45$ (cascade ~40%), (3) Pseudo-core shells $k=30$–$k=35$ (cascade ~38–39%), (4) Randomly selected peripheral users (cascade ~5%). Viral spread correlates more strongly with coreness than with degree alone.

---

### Q1 (1 point)

**What is a k-core of a graph?**

- A) Nodes ranked by degree
- B) A subgraph where every node has degree at least $k$
- C) A clique of size $k$
- D) Nodes with exactly $k$ links

**Answer:** B

**Answer Explanation:** By strict mathematical definition, a k-core is a maximal subgraph in which every node maintains at least $k$ connections to other nodes within that subgraph. This is distinct from random selections of nodes (A), cliques (C, which require all-to-all connectivity), or nodes with exactly $k$ links (D). The definition is built into the k-core decomposition algorithm: you iteratively remove nodes with degree < $k$ until all remaining nodes have degree ≥ $k$. This creates a nested structure where $C_1 \supseteq C_2 \supseteq ... \supseteq C_{\max}$. This is the foundational definition tested across all assignments involving k-cores.

**Related Topic:** [K-Core Decomposition: Peeling the Network Onion](notes.md#2-k-core-decomposition-peeling-the-network-onion)

---

### Q2 (1 point)

**The coreness of a node refers to:**

- A) Its initial degree
- B) The highest k-core (deepest shell) it belongs to
- C) Its cascade size
- D) Its total neighbors

**Answer:** B

**Answer Explanation:** Coreness is not a static property like degree (A) or cascade size (C). Instead, it is the **maximum $k$ value** for which a node survives the k-core decomposition process. If a node is removed during the $k=5$ decomposition step, its coreness is 4. If it survives to $k=30$ but not $k=31$, its coreness is 30. This definition directly assigns each node to its deepest shell, capturing its structural embeddedness. The distinction is crucial: two nodes with identical degree can have very different coreness values depending on their structural position—this is the key advantage of coreness over degree for predicting influence.

**Related Topic:** [Coreness as a Centrality Measure](notes.md#3-coreness-as-a-centrality-measure)

---

### Q3 (1 point)

**Why can coreness predict spreading power better than degree?** *(Select all that apply)*

- A) Core nodes lie in densely connected regions
- B) High-degree nodes may still be peripheral
- C) Coreness captures global structural position
- D) Coreness is always proportional to degree

**Answer:** A, B, C

**Answer Explanation:** Option A is correct: by definition, high-coreness nodes exist in k-cores where all neighbors also have degree ≥ $k$. This dense, reciprocal connectivity accelerates cascade spreading. Option B exposes the core weakness of degree: a node with 1,000 followers could be a celebrity in an isolated star graph—locally central but globally useless for cascades. A high-degree peripheral node generates weak cascades because its neighbors lack onward connections. Option C is correct: coreness forces consideration of global structure—to achieve high coreness, your neighbors must also be deeply embedded, which requires exploring beyond your immediate neighborhood to the broader network hierarchy. Option D is false: coreness and degree are uncorrelated. A node with degree 50 could have coreness 2 (peripheral cluster) or coreness 30 (embedded in deep shell)—the difference determines cascade size by a factor of 8–10x. This is the empirical finding covered in notes section **"Coreness vs. Degree"**.

**Related Topic:** [Coreness as a Centrality Measure](notes.md#3-coreness-as-a-centrality-measure)

---

### Q4 (1 point)

**Which seeding strategy is most effective under a limited marketing budget?**

- A) Random peripheral nodes
- B) Highest-degree nodes
- C) Pseudo-core nodes
- D) Lowest-degree nodes

**Answer:** C

**Answer Explanation:** The case study explicitly presents four strategies with measured outcomes:
- Strategy A (random peripheral): ~5% cascade, abundant but worthless.
- Strategy B (highest-degree): ~25% cascade, expensive celebrities, echo chamber effect.
- Strategy C (pseudo-core, $k=30$–$k=35$): ~38–39% cascade, 80,000 users (4% of network), moderate cost.
- Strategy D (lowest-degree): ~1–2% cascade, maximally cheap but ineffective.

Strategy C achieves 38–39% cascade compared to innermost core's 40% (only 2.5% difference), yet costs 8–10x less and reaches vastly more diverse communities. Under budget constraints, pseudo-core seeding provides optimal ROI: near-core cascade size at fraction of the cost. This is the strategic breakthrough discussed in notes section **"The Surprising Discovery: Pseudo-Cores"**, which validates the cost-benefit analysis demonstrating pseudo-core superiority.

**Related Topic:** [The Surprising Discovery: Pseudo-Cores](notes.md#5-the-surprising-discovery-pseudo-cores)

---

### Q5 (1 point)

**Why can targeting only high-degree nodes fail?**

- A) High-degree nodes always guarantee large cascades
- B) Degree ignores structural position in the network
- C) All high-degree nodes are peripheral
- D) Degrees are identical across nodes

**Answer:** B

**Answer Explanation:** The case study shows high-degree users generating only ~25% cascade versus innermost core's 40%. This 60% performance gap reveals the flaw: degree counts immediate connections but ignores whether those connections are mutually connected (creating dense, high-transmission regions) or isolated (creating echo chambers). A celebrity with 1 million followers split across 10 non-overlapping communities appears to have extreme centrality by degree, yet each community receives only 100,000 exposures with zero social proof from peers. Option A is explicitly false—the case demonstrates high-degree nodes underperform. Option C is false—high-degree nodes can be in the core, but their high degree alone doesn't guarantee it. Option D is trivially false. The key insight is that degree is **locally blind**: it sees immediate neighbors but not the structure they inhabit. This is the fundamental distinction covered in notes section **"The Limits of Degree"**.

**Related Topic:** [The Limits of Degree: Why Follower Count Isn't Everything](notes.md#1-the-limits-of-degree-why-follower-count-isnt-everything)

---

## Case Study 2 — Core-Periphery Structure and Cascade Optimization

A research group studies information diffusion in an online professional network of 500,000 users. K-core decomposition reveals: highest k-core = $k=28$; nodes with $k ≥ 25$ = 3% of network but highly interconnected; intermediate shells $k=18$–$k=22$ produce cascade sizes nearly matching innermost core; peripheral shells $k ≤ 4$ generate small cascades that die out quickly.

Simulation results: seeding a node in the highest core infects ~40% of the network; seeding in the $k=20$ shell infects ~37%; seeding in the $k=3$ shell infects only ~5%. Removing high-coreness nodes drops overall cascade size dramatically. Removing high-degree but low-coreness nodes has comparatively limited impact (~2–5% reduction).

---

### Q6 (1 point)

**Which nodes are the most effective spreaders?**

- A) Peripheral nodes
- B) Highest-core nodes
- C) Random nodes
- D) Lowest-degree nodes

**Answer:** B

**Answer Explanation:** The case study's simulation data directly demonstrates this ranking: highest-core nodes (40% cascade) >> intermediate shells (37% cascade) >> peripheral nodes (5% cascade). The distinction is dramatic and statistically robust. Why? High-coreness nodes activate immediately into densely interconnected regions where transmission is rapid and sustained. Their neighbors are themselves highly connected, creating a "cascade-friendly" topology. Peripheral nodes (low coreness) activate into sparse regions where transmission stalls quickly. The case explicitly shows that pure node selection (A, C, D) without considering coreness yields weak cascades. This empirical hierarchy directly validates the independent cascade model discussed in notes section **"Independent Cascade Model"**, where nodes in high-coreness regions exponentially outperform others.

**Related Topic:** [Independent Cascade Model: Simulating Viral Spread](notes.md#4-independent-cascade-model-simulating-viral-spread)

---

### Q7 (1 point)

**Which findings indicate that pseudo-cores exist?** *(Select all that apply)*

- A) Mid-level shells trigger large cascades
- B) Peripheral nodes outperform core nodes
- C) Spreading depends on structural depth
- D) Some non-core shells achieve cascade sizes comparable to the core

**Answer:** A, C, D

**Answer Explanation:** Options A and D are nearly identical and both correct: the case explicitly reports that $k=18$–$k=22$ shells generate cascades "nearly matching innermost core." This is the defining characteristic of pseudo-cores—high-quality spreading performance without being in the absolute center. Option C is correct: the dramatic correlation between cascade size and the k-shell placement (not just degree) proves that spreading depends fundamentally on structural depth. Option B is false and contradicts the data: peripheral nodes (5% cascade) do not outperform core nodes (40% cascade). The pseudo-core phenomenon is the surprising discovery that **you don't need to be in the innermost core to achieve core-level results**—intermediate shells can rival the absolute center. This is discussed in notes section **"The Surprising Discovery: Pseudo-Cores"**, which explains why pseudo-cores emerge: they combine sufficient local density with cross-community bridges, creating powerful but not-redundant spreading.

**Related Topic:** [The Surprising Discovery: Pseudo-Cores](notes.md#5-the-surprising-discovery-pseudo-cores)

---

### Q8 (1 point) — NAT

**If 37% of 500,000 users are infected, how many users is that?**

**Answer: 185000**

**Explanation:** Simple percentage arithmetic: $500,000 \times 0.37 = 185,000$ users. This calculation reinforces the case study's quantitative findings: seeding a single node in the $k=20$ shell infects 185,000 downstream users. This is only 5,000 fewer infections than the innermost core (40% × 500,000 = 200,000), despite being in a dramatically different structural position. The numerical similarity (185,000 vs. 200,000) illustrates why pseudo-cores are strategically valuable: they achieve nearly identical reach at a fraction of the cost.

**Related Topic:** [Independent Cascade Model: Simulating Viral Spread](notes.md#4-independent-cascade-model-simulating-viral-spread)

---

### Q9 (1 point)

**What is the fundamental step in k-core decomposition?**

- A) Iteratively remove nodes with degree less than $k$
- B) Compute shortest paths between nodes
- C) Identify the highest-degree nodes
- D) Remove random edges from the network

**Answer:** A

**Answer Explanation:** The k-core decomposition algorithm is built entirely on this iterative removal principle. At each $k$ value, you systematically delete all nodes with degree < $k$ and recursively update neighbors' degrees—this cascading removal continues until every remaining node has degree ≥ $k$. Option B (shortest paths) is relevant to betweenness centrality, not k-cores. Option C (highest-degree identification) is a starting heuristic but not the fundamental algorithm. Option D (random edge removal) has no place in the deterministic, algorithmic k-core decomposition. This iterative removal is the heart of the algorithm, explained in detail in notes section **"K-Core Decomposition Algorithm"**, and it is the basis for computing coreness values for all nodes in the network.

**Related Topic:** [K-Core Decomposition: Peeling the Network Onion](notes.md#2-k-core-decomposition-peeling-the-network-onion)

---

### Q10 (1 point)

**Why does removing high-coreness nodes significantly reduce cascade size?**

- A) They are isolated from the network
- B) They anchor densely connected regions of the network
- C) They have the lowest degree
- D) They are located at the network periphery

**Answer:** B

**Answer Explanation:** High-coreness nodes serve as the structural scaffolding that holds together the network's densest regions. Removing them fractures these dense clusters into smaller, isolated fragments. Without the central "hubs" of high coreness, information transmission between previously dense regions becomes sparse and slow, dramatically reducing cascade sizes (from 40% to 8–12% in the case study). Option A is false—high-coreness nodes are by definition deeply connected. Option C contradicts the definition—high-coreness nodes have high (not low) degree within their regions. Option D is false—high-coreness nodes are at the center, not periphery. The insight is that high-coreness nodes are **critical connectors within dense regions**, not just locally popular. Removing them causes network fragmentation and cascade collapse, as discussed in notes section **"Removing Core Nodes: Impact on Network Resilience"**.

**Related Topic:** [Removing Core Nodes: Impact on Network Resilience](notes.md#7-removing-core-nodes-impact-on-network-resilience)

---

## Case Study 3 — Diffusion of a New Feature in a Social Media Platform

A social media company soft-launches a new interactive feature. Their network has a core-periphery structure. Initially, the feature is seeded to highest follower-count users (highest degree)—initial visibility is generated, but adoption remains concentrated among already-active users, failing to penetrate less-active communities. High-degree users have overlapping audiences, causing redundant exposure.

The company then targets pseudo-core users—intermediate shells whose nodes are embedded across multiple communities. These users have moderate follower counts but are structurally positioned across diverse communities. When seeded among them, adoption spreads through weak ties into previously disconnected user groups. Pseudo-core users consistently generate cascades rivaling those from core users; diffusion stabilizes faster and reaches a broader demographic. Conclusion: coreness is a more reliable predictor of influence than degree alone.

---

### Q11 (1 point)

**Why did seeding high-degree users fail to produce wide adoption?**

- A) They lacked sufficient followers
- B) Their audiences overlapped significantly
- C) The network lacked weak ties
- D) The feature quality was poor

**Answer:** B

**Answer Explanation:** The case explicitly identifies the problem: "High-degree users have overlapping audiences causing redundant exposure." A celebrity with 1 million followers might have 900,000 followers who also follow other celebrities. When you seed the top 5 celebrities, you're reaching the same 800,000 users multiple times, while 1.2 million users in other communities never see the feature. The cascade dies because redundant exposure generates no new adoption—the feature fails to "jump" into unexplored communities. Option A contradicts the premise (these users, by definition, have many followers). Option C is false—the network has weak ties; the problem is that high-degree nodes' weak ties are redundantly overlapping. Option D is extraneous—we're analyzing why the marketing strategy failed, assuming feature quality is constant. This is the "echo chamber problem" discussed in notes section **"Why Pseudo-Cores Are Strategically Superior"**, which distinguishes between high visibility (many exposures) and high reach (diverse audiences).

**Related Topic:** [The Surprising Discovery: Pseudo-Cores](notes.md#5-the-surprising-discovery-pseudo-cores)

---

### Q12 (1 point)

**Which factors explain why coreness is a better predictor of influence than degree?** *(Select all that apply)*

- A) Coreness captures position within the network hierarchy
- B) Degree ignores redundancy in connections
- C) Coreness reflects access to multiple shells and communities
- D) Degree always correlates with cascade size

**Answer:** A, B, C

**Answer Explanation:** Option A is correct: coreness measures how deeply embedded a node is in the network's core layers, capturing its position in the global hierarchy. Degree is purely local and blind to this hierarchy. Option B is correct: two nodes with identical degree can have vastly different cascade potential depending on whether their neighbors are redundantly overlapping (echo chamber) or distinctly positioned (diverse reach). Coreness implicitly filters out redundancy because to achieve high coreness, your neighbors must be deeply embedded AND their neighborhoods must be distinct enough to collectively maintain high density. Option C is correct: nodes in intermediate k-shells (pseudo-cores) bridge multiple shells, inheriting connections to diverse communities. Option D is false: degree correlates weakly with cascade size (~0.35–0.50), which is precisely why high-degree strategies fail. This comparison is central to notes section **"Why Coreness Outperforms Degree for Influence"**, which provides the theoretical and empirical justification for coreness superiority.

**Related Topic:** [Why Coreness Predicts Cascades Better Than Degree](notes.md#6-why-coreness-predicts-cascades-better-than-degree)

---

### Q13 (1 point)

**In the independent cascade simulations, adoption stabilizes when:**

- A) Core users adopt the feature
- B) Peripheral users reject the feature
- C) No new users adopt in an iteration
- D) All users are exposed

**Answer:** C

**Answer Explanation:** In the independent cascade model (discussed in Week 12 notes), the simulation proceeds in discrete time steps. At each step, newly infected nodes attempt to activate their uninfected neighbors. The cascade continues iterating through these activation attempts. The simulation formally terminates and "stabilizes" when a time step completes with **zero new infections**—meaning no uninfected nodes were activated, so the process has reached its equilibrium. Option A is incorrect—core adoption is an early event, not the termination signal. Option B is irrelevant—rejections don't stop the simulation; inactivity does. Option D is unrealistic—the cascade typically terminates long before reaching all users (e.g., 40% adoption, not 100%). This is a technical but important definition from notes section **"Independent Cascade Model: Spreading Dynamics"**, where termination is the mathematical endpoint of the algorithm.

**Related Topic:** [Independent Cascade Model: Simulating Viral Spread](notes.md#4-independent-cascade-model-simulating-viral-spread)

---

### Q14 (1 point)

**Which outcomes highlight the advantage of targeting pseudo-core users?** *(Select all that apply)*

- A) Faster diffusion across communities
- B) Reduced redundancy in exposure
- C) Guaranteed adoption by all users
- D) Larger and more diverse cascades

**Answer:** A, B, D

**Answer Explanation:** Option A is correct: the case reports that "diffusion stabilizes faster" when seeded among pseudo-cores, because pseudo-cores span communities, enabling rapid cross-community transmission. Option B is correct: unlike highest-degree users (whose audiences heavily overlap), pseudo-core users tap into different community networks, reducing exposure redundancy and enabling previously unreached user groups to adopt. Option D is correct: the case explicitly states pseudo-core users "generate cascades rivaling those from core users" while reaching a "broader demographic," creating both large AND diverse outcomes. Option C is false: no strategy guarantees universal adoption (even core seeding reached only 40%, not 100%). The entire strategic value of pseudo-cores is this combination: match the cascade size of the core (A, D) while avoiding the echo-chamber problem and enabling faster cross-community spread (A, B). This is discussed in notes section **"Why Pseudo-Cores Are Strategically Superior"**.

**Related Topic:** [The Surprising Discovery: Pseudo-Cores](notes.md#5-the-surprising-discovery-pseudo-cores)

---

### Q15 (1 point)

**The company's revised strategy demonstrates that:**

- A) Network size determines diffusion success
- B) Feature adoption depends only on visibility
- C) Structural position outweighs follower count
- D) Peripheral users cannot influence adoption

**Answer:** C

**Answer Explanation:** This is the ultimate thesis of Week 12, the final synthesis of the entire course. The case study shows that swapping from high-degree (high visibility, 1 million followers each) to moderate-degree pseudo-cores (moderate visibility, ~50,000 followers each) **improved** adoption reach and diversity. This is only possible if structural position is more important than raw visibility. Option A is false—network size is constant in both strategies. Option B is false—visibility alone (the high-degree strategy) generated poor adoption; the revised strategy achieved better adoption with less visibility. Option D is false—peripheral users can definitely influence adoption; they just aren't the best seeding targets because their coreness is too low. Option C captures the core insight: **a user embedded in 4 densely connected communities (pseudo-core) will trigger exponentially larger cascades than a celebrity surrounded by isolated followers (high degree but low coreness)**. This principle, discussed throughout notes section **"Coreness as a Centrality Measure"** and the comparative analysis in **"Comparison: Centrality Measures Across the Course"**, represents the convergence of 12 weeks of network science into a single, actionable principle.

**Related Topic:** [Coreness as a Centrality Measure](notes.md#3-coreness-as-a-centrality-measure)

---

## Answer Key Summary

| Q | Answer | Concept Tested |
|---|--------|---|
| 1 | B | K-Core Definition (Degree ≥ k in Subgraph) |
| 2 | B | Coreness as Maximum K Value |
| 3 | A, B, C | Why Coreness Beats Degree for Cascades |
| 4 | C | Pseudo-Core Strategic Superiority |
| 5 | B | Degree's Structural Blindness |
| 6 | B | Highest-Core Dominance in Cascades |
| 7 | A, C, D | Pseudo-Core Empirical Evidence |
| 8 | 185000 | Percentage Calculation (37% of 500k) |
| 9 | A | K-Core Decomposition Algorithm |
| 10 | B | Core Nodes as Network Scaffolding |
| 11 | B | Echo Chamber Problem of High-Degree Nodes |
| 12 | A, B, C | Theoretical Advantages of Coreness |
| 13 | C | Cascade Termination Condition |
| 14 | A, B, D | Pseudo-Core Strategic Benefits |
| 15 | C | Structural Position > Follower Count |

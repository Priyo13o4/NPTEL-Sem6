# Week 6: Assignment 6 — Social Networks (NOC26_CS82)

| Field | Value |
|-------|-------|
| **Due** | 2026-03-04, 23:59 IST |
| **Submitted** | 2026-02-24, 23:41 IST |
| **Total Score** | 11.08/15 |

---

## Case Study 1 — Influence Ranking at TrendHub (50,000 Creators)

TrendHub is a social media analytics company helping brands identify influential content creators. With over **50,000 active creators**, traditional follower-count metrics proved misleading—many accounts purchase fake followers while genuinely influential creators with smaller audiences drive better engagement. Their initial approach of **100 human analysts** cost $500,000/year and produced inconsistent rankings.

TrendHub's data science team built an **"influence graph"** inspired by Google's PageRank—each creator is a node, directed edges represent endorsements. Automated crawlers perform **random walks** starting from known creators, requiring approximately **n log n** steps to map most of the network. Two equivalent mechanisms rank creators:
1. Each creator starts with **100 influence points**, distributed equally among endorsed creators iteratively until equilibrium
2. Millions of **"discovery journeys"** (random walks) drop an influence point at each visit

Key insight: 10 endorsements from selective mega-influencers outweigh 500 endorsements from average creators. The system reduced costs by **80%** and improved campaign ROI by **45%**.

---

### Q1 (1 point)
**TrendHub has 8,000 content creators on their platform. Based on the n log n relationship for network exploration, approximately how many random walk steps would be needed to map most of the influence graph?**

- A) Approximately 24,000 random walk steps would be required for comprehensive mapping
- B) Approximately 52,000 random walk steps would be required for comprehensive mapping
- C) Approximately 72,000 random walk steps would be required for comprehensive mapping
- D) Approximately 96,000 random walk steps would be required for comprehensive mapping

**Answer:** C

**Answer Explanation:** To ensure a random walk adequately explores a network, the formula is $n \times \ln(n)$. For 8,000 creators: $8000 \times \ln(8000) \approx 8000 \times 8.987 \approx 71,896$, which rounds to approximately 72,000 steps. This ensures the random walker thoroughly samples the influence graph before convergence. The relationship accounts for both visiting each node and obtaining statistically representative coverage of the network topology.

**Related Topic**: [Network Coverage: The n log n Relationship](notes.md#network-coverage-the-n-log-n-relationship)

---

### Q2 (1 point)
**A fashion brand is evaluating three creators for a campaign: Creator A has 1 million followers and endorses 500 other creators monthly. Creator B has 200,000 followers and endorses 10 creators monthly, but is endorsed by 5 top-tier influencers. Creator C has 500,000 followers with moderate endorsements. According to TrendHub's influence point system, which statements are accurate?** *(Select all that apply)*

- A) Creator A will distribute their influence points thinly across many recipients
- B) Creator B could rank higher than A despite fewer followers due to endorsements
- C) The number of followers directly determines the final influence ranking scores
- D) Creator B's selective endorsement pattern makes their endorsements more valuable

**Answer:** A, B, D

**Answer Explanation:** This question highlights the fundamental flaw in DegreeRank (follower counts). Creator A endorses 500 people, so their influence points are fractured into tiny fractions distributed thinly across many recipients (A is correct). Creator B selectively endorses only 10 people and receives incoming endorsements from top-tier influencers, making their endorsements incredibly valuable and potentially boosting their overall rank above Creator A despite fewer followers (B and D are correct). Option C is false because PageRank specifically overrides raw follower counts by introducing recursive authority weighting—a key insight of the PageRank algorithm.

**Related Topic**: [Implementation Method 1: Points Distribution](notes.md#implementation-method-1-points-distribution)

---

### Q3 (1 point)
**TrendHub discovers that Creator Lisa receives endorsements from 8 mega-influencers who each have 50+ endorsements from other verified high-quality creators, and these mega-influencers endorse only 5 people each. Creator Mike receives endorsements from 200 micro-influencers with average reach. In the influence point distribution system, what is the most likely outcome?**

- A) Mike will rank higher because the total number of endorsing creators matters most
- B) Lisa will rank higher because quality and selectivity of endorsements carry weight
- C) Both will rank similarly as the system only counts direct endorsement numbers
- D) Mike will rank higher initially but Lisa eventually as the system iterates further

**Answer:** B

**Answer Explanation:** Lisa achieves PageRank success through the combination of **quality** and **selectivity**. She receives incoming influence from nodes with high authority (mega-influencers), and because those nodes have low out-degrees (only 5 endorsements each), Lisa receives a massive chunk of each mega-influencer's PageRank score—approximately $1/5 = 20\%$ of their influence. In contrast, Mike receives tiny fractions from low-authority micro-influencers. In the points distribution method, when a node distributes points equally among its outgoing links, selective endorsement becomes extremely valuable. Lisa's concentrated endorsements vastly outweigh Mike's diluted volume.

**Related Topic**: [The PageRank Philosophy: Recursive Authority](notes.md#the-pagerank-philosophy-recursive-authority)

---

### Q4 (1 point) — NAT
**In TrendHub's simulation, they performed 100,000 random discovery journeys across their creator network. Creator Jordan's profile was visited 3,500 times during these journeys. What percentage of the total random walks resulted in visits to Jordan's profile?** *(Answer as a number between 0 and 100)*

**Answer:** 3.5

**Answer Explanation:** This is a straightforward percentage calculation based on the random walk simulation. The fraction of visits to Jordan is $3500 / 100000 = 0.035$, which converts to a percentage: $0.035 \times 100 = 3.5\%$. In the random walk implementation of PageRank, the proportion of visits a node receives is directly proportional to its PageRank score.

**Related Topic**: [Implementation Method 2: The Random Walk](notes.md#implementation-method-2-the-random-walk)

---

### Q5 (1 point)
**TrendHub claims their influence ranking system helped brands achieve 45% higher campaign ROI compared to using follower counts. Which of the following explain why the graph-based ranking system is superior to simple follower counts?** *(Select all that apply)*

- A) The system identifies creators whose influence is validated by other influential creators recursively
- B) Follower counts can be artificially inflated through purchased fake followers easily
- C) The iterative point distribution naturally filters out creators with low-quality connections
- D) Random walk frequency directly measures how often real users would discover the creator

**Answer:** A, B, C

**Answer Explanation:** PageRank addresses three critical flaws in follower-count rankings. **(A)** PageRank is recursive—a node's importance depends on the importance of nodes linking to it, creating a feedback loop that validates genuine influence. **(B)** Follower counts are manipulable; PageRank is resistant because fake followers typically don't establish meaningful endorsement relationships, and even if they do, they carry low authority. **(C)** The iterative point distribution process naturally suppresses low-quality connections—a creator with many endorsements from low-authority nodes receives minimal influence accumulation, while one with few but high-quality endorsements accumulates large influence. Option **(D)** is incorrect because the random walk is a mathematical simulation to calculate PageRank; it doesn't literally measure real user behavior. Random walk frequency simulates influence flow, not actual discovery patterns.

**Related Topic**: [The PageRank Philosophy: Recursive Authority](notes.md#the-pagerank-philosophy-recursive-authority)

---

## Case Study 2 — Applying PageRank to NCAA March Madness (2013)

Every year, millions attempt to predict NCAA March Madness outcomes. Traditional ranking methods like the AP Poll rely on biased human voters, while RPI uses win-loss records but misses complex interactions. In 2013, data scientists applied PageRank to NCAA basketball as a directed graph problem.

**Methodology:** Over **5,000 games** were converted into a directed graph—each team is a node, and when Team A defeats Team B, an edge points **from B to A** (the loser "vouches for" the winner). Strong teams accumulate incoming edges from many opponents. Implementation: each team starts with **100 points**, distributed equally among teams they lost to, iterated until convergence. A damping factor of **0.85** addresses sink problems.

**Results:** Champion Louisville ranked highly, as did Final Four teams Michigan and Syracuse. However, different damping factors (0.5 vs 0.85 vs 0.9) produced different top-ten orderings. The algorithm couldn't account for injuries, momentum, or coaching adjustments.

**Other real-world PageRank applications:**
- **GeneRank**: identifies critical genes in biological networks
- **Environmental science**: maps toxic chemical accumulation in ecosystems
- **Finance**: identifies market-leading stocks via supply chain and board connections
- **Tennis**: Jimmy Connors ranks highest when matches form a directed graph

---

### Q6 (1 point)
**Team Alpha accumulated 240 points and lost to exactly three opponents. How many points does each opponent receive from Team Alpha in one iteration?**

- A) 80 points
- B) 60 points
- C) 90 points
- D) 72 points

**Answer:** A

**Answer Explanation:** In the points distribution method, a node divides its points *equally* among all its outgoing edges. In the NCAA context, a loss creates an outgoing edge to the winning team. Team Alpha has 240 points and 3 losses (3 outgoing edges). Each opponent receives: $240 \div 3 = 80$ points. This implements the PageRank principle that selective endorsements carry weight—a team that loses to few opponents distributes its "authority" concentrated among those few winners.

**Related Topic**: [The Mechanics of Point Distribution](notes.md#the-mechanics-of-point-distribution)

---

### Q7 (1 point)
**Which represent legitimate challenges when applying PageRank to NCAA basketball predictions?** *(Select all that apply)*

- A) Different damping factors like 0.5 versus 0.85 produce significantly different rankings
- B) The algorithm cannot account for injuries or momentum entering tournament games
- C) Teams from stronger conferences naturally accumulate more validation affecting rankings
- D) The method requires manual adjustment of game weights based on margin of victory

**Answer:** A, B, C

**Answer Explanation:** **(A)** is correct—damping factors control the balance between direct wins and global influence redistribution. Lower damping (0.5) emphasizes direct connections; higher damping (0.85-0.90) emphasizes global structure. Different values fundamentally alter ranking equilibria. **(B)** is correct—PageRank sees only the directed graph structure (wins/losses); it cannot perceive real-world context like player injuries, team momentum, or coaching changes. **(C)** is correct—teams in stronger conferences play more highly-ranked opponents, so they naturally receive PageRank from higher-authority nodes, inflating their rankings. This creates a feedback loop where conference strength compounds. Option **(D)** is incorrect; the basic PageRank algorithm doesn't require manual weighting—it uses binary edges. (Advanced versions might weight edges by margin of victory, but the question asks about legitimate challenges, not optional enhancements.)

**Related Topic**: [The Damping Factor & Teleportation Solution](notes.md#the-damping-factor--teleportation-solution)

---

### Q8 (1 point)
**Why do edges point from losing team to winning team?**

- A) Creates network flow where losers distribute authority to winners
- B) Prevents convergence problems by ensuring all nodes have outgoing paths
- C) Matches hyperlinks creating consistency with web page ranking
- D) Reduces computational complexity for large datasets

**Answer:** A

**Answer Explanation:** In PageRank, a directed edge represents a "vote of confidence" or "validation." When Team A loses to Team B, this mathematically represents Team A "vouching for" Team B's strength. By drawing the edge from loser to winner, we create network flow where losing teams distribute their accumulated authority upward to the teams that defeated them. This is the core principle of PageRank—authority flows through the network following the direction of endorsement/validation. Option **(B)** is incorrect because sink nodes (teams with zero losses, i.e., no outgoing edges) create convergence problems, which is why damping factors are needed. Option **(C)** is partially true conceptually but not the primary reason—the real reason is that edges model the "vote of confidence" relationship inherent in defeats.

**Related Topic**: [The PageRank Philosophy: Recursive Authority](notes.md#the-pagerank-philosophy-recursive-authority)

---

### Q9 (1 point) — NAT
**Team A has 500 points and loses to Teams B and C. Team D has 300 points and loses only to Team B. Team E has 200 points and loses to Teams B, C, and D. Calculate total points Team B receives in one iteration.** *(Round to nearest whole number)*

**Answer:** 617

**Answer Explanation:** We calculate the fractions Team B receives from all incoming edges (teams that lost to B):
- **Team A** has 500 points and 2 outgoing edges (losses to B and C). Team B receives: $500 \div 2 = 250$ points.
- **Team D** has 300 points and 1 outgoing edge (loss to B). Team B receives: $300 \div 1 = 300$ points.
- **Team E** has 200 points and 3 outgoing edges (losses to B, C, and D). Team B receives: $200 \div 3 = 66.67$ points, which rounds to 67 points.
- **Total received by Team B:** $250 + 300 + 67 = 617$ points.

This demonstrates how PageRank accumulates: Team B benefits from teams losing to them, with the benefit magnitude depending on how selectively those teams lost (out-degree).

**Related Topic**: [Example Calculation: One Iteration](notes.md#example-calculation-one-iteration)

---

### Q10 (1 point)
**Which statements accurately describe real-world PageRank applications outside web search?** *(Select all that apply)*

- A) GeneRank identifies critical genes by analyzing biological network connectivity
- B) Environmental scientists use PageRank to map toxic chemical accumulation patterns
- C) Tennis match analysis identifies Jimmy Connors as the highest-ranked player
- D) Financial PageRank applications have completely eliminated traditional stock analysis

**Answer:** A, B, C

**Answer Explanation:** **(A)** is correct—GeneRank applies PageRank to biological networks where genes are nodes and regulatory interactions are edges. Genes with high PageRank are critical regulatory hubs whose disruption would severely impact the system. **(B)** is correct—environmental scientists model organisms and feeding relationships (predator-prey) as directed graphs, using PageRank to identify apex predators where toxins accumulate most severely and to trace primary contamination sources. **(C)** is correct—historical tennis matches form a directed graph where edges point from the loser to the winner. Jimmy Connors, with his prolific career and victories over numerous strong opponents, naturally ranks highest in PageRank analysis of the tennis network. Option **(D)** is incorrect—PageRank applications supplement but haven't eliminated traditional stock analysis; both methods are used complementarily.

**Related Topic**: [PageRank Beyond the Internet](notes.md#pagerank-beyond-the-internet)

---

## Case Study 3 — Citation Impact Assessment System (Dr. Rajesh Kumar)

Dr. Rajesh Kumar at a university's research analytics department implements PageRank to evaluate research paper impact across **10,000 papers** published over a decade. Traditional citation counts (in-degree) fail to capture quality of citing papers.

**Implementation:** Two approaches—**points distribution** and **random walk**—with a damping factor of **s = 0.8** (80% retained, 20% redistributed uniformly), handling sink nodes (papers with no outgoing citations). After running on a **500-paper subset**, he compares results with NetworkX's built-in PageRank.

**Key findings:**
- A foundational **1995 neural networks paper** has only 45 direct citations but achieves the **3rd-highest PageRank** (cited by seminal papers with thousands of citations)
- A **2020 survey paper** has 320 citations but ranks **67th** (citing papers from obscure, low-impact venues)
- High citation count ≠ proportionally high PageRank, and vice versa

---

### Q11 (1 point)
**In the points distribution method implementation, after each iteration Rajesh applies the sink-handling mechanism. If a paper initially has 250 PageRank points and the damping factor s = 0.8, with a total of 500 papers in the network each starting with 100 points, how many points does this paper retain after the sink-handling step (before receiving redistributed points)?**

- A) 200 points retained, receives additional 10 points from redistribution
- B) 200 points retained, receives additional 20 points from redistribution
- C) 250 points retained, receives additional 20 points from redistribution
- D) 200 points retained, receives additional 50 points from redistribution

**Answer:** B

**Answer Explanation:** The damping factor $s = 0.8$ means the paper retains 80% of its points for distribution through citations, and 20% is taxed into a redistribution pool. 

**Retention calculation:** $250 \times 0.8 = 200$ points retained (distributed through outgoing citation edges).

**Redistribution calculation:** The damping factor applies to the *entire network*. Total network points = 500 papers $\times$ 100 points = 50,000 points. The tax collected is $(1 - 0.8) \times 50,000 = 0.2 \times 50,000 = 10,000$ points. These 10,000 points are redistributed equally to all 500 papers: $10,000 \div 500 = 20$ points per paper.

**Answer:** 200 points retained, 20 points received from redistribution (Option B).

**Related Topic**: [The Damping Factor & Teleportation Solution](notes.md#the-damping-factor--teleportation-solution)

---

### Q12 (1 point)
**During Rajesh's random walk implementation with 1,000,000 iterations on 500 papers, he observes that the PageRank ordering differs slightly from NetworkX's results for papers ranked 15th and 16th. Which of the following are valid explanations for this discrepancy?** *(Select all that apply)*

- A) The two papers may have identical or nearly identical true PageRank values, causing ranking ambiguity
- B) 1,000,000 iterations may be insufficient for complete convergence to exact PageRank values
- C) The random walk method uses probabilistic sampling, introducing stochastic variation in visit frequencies
- D) NetworkX uses a different damping factor default value of 0.85 instead of 0.80

**Answer:** A, B, C, D

**Answer Explanation:** All four options are valid reasons for ranking discrepancies:

**(A)** Papers ranked 15th and 16th may have nearly identical PageRank scores (e.g., 0.00847 vs 0.00851), falling within numerical rounding uncertainty. Small score differences can flip ranking positions. 

**(B)** While 1,000,000 iterations is substantial, random walk convergence to the *exact* theoretical PageRank requires infinite iterations. With finite iterations, you obtain an approximation. Papers ranked close together are most affected by convergence imprecision.

**(C)** The random walk method is inherently probabilistic. Two runs with different random seeds produce slightly different visit frequencies. This stochastic variation causes ranking perturbations, especially for papers with similar true PageRank.

**(D)** NetworkX's default damping factor (alpha parameter) is 0.85, not 0.80. Different damping factors produce different equilibria. With $s = 0.80$, papers near different network clusters receive different amounts of redistribution (20% pool) vs. $s = 0.85$ (15% pool), shifting rankings.

**Related Topic**: [Implementation Method 2: The Random Walk](notes.md#implementation-method-2-the-random-walk)

---

### Q13 (1 point) — NAT
**In Rajesh's citation network analysis, he finds that a paper with an in-degree of 5 has a PageRank score of 0.0087, while another paper with an in-degree of 150 has a PageRank score of 0.0031. If he calculates the ratio of (PageRank/in-degree) for both papers and then divides the first ratio by the second ratio, what value does he obtain?** *(Round to 2 decimal places)*

**Answer:** 84.19

**Answer Explanation:** This question demonstrates that PageRank and in-degree are not linearly correlated.

**First paper:** PageRank/in-degree = $0.0087 / 5 = 0.00174$

**Second paper:** PageRank/in-degree = $0.0031 / 150 = 0.000020667$

**Ratio of ratios:** $0.00174 / 0.000020667 = 84.19$

This enormous ratio (84.19x) illustrates the key insight: the first paper is cited by only 5 sources but each citation is from a highly authoritative paper, yielding a PageRank/in-degree ratio 84 times higher than the second paper, which is cited 150 times but mostly by low-impact sources. This demonstrates PageRank's superiority over simple citation counting for assessing impact quality.

**Related Topic**: [The PageRank Philosophy: Recursive Authority](notes.md#the-pagerank-philosophy-recursive-authority)

---

### Q14 (1 point)
**Rajesh implements teleportation in his random walk method to handle sink nodes. If during a random walk he reaches a paper that has cited 0 other papers (a sink node), what is the fundamental reason this teleportation mechanism is necessary for accurate PageRank computation?**

- A) Without teleportation, all PageRank mass would accumulate in sink nodes, preventing proper distribution and convergence
- B) Without teleportation, the random walk would terminate prematurely, undersampling the majority of the network
- C) Without teleportation, papers with high in-degree would receive disproportionately inflated PageRank scores
- D) Without teleportation, the damping factor mechanism would fail to redistribute 20% of points uniformly

**Answer:** B

**Answer Explanation:** In a random walk simulation, a "drunk surfer" clicks random outgoing links from the current page. At a sink node (a paper with zero outgoing citations), there are *no links to click*. Without teleportation, the algorithm cannot proceed—the walk gets stuck at a sink, terminates prematurely, and can never explore the rest of the network.

**Why this matters:** The $n \log n$ exploration formula assumes the random walker traverses the entire network. Early termination at sinks means many nodes are undersampled or never visited. This biases the final PageRank scores, over-representing sink nodes and under-representing nodes reachable only *after* passing through sinks.

**Teleportation solution:** With probability $(1 - d)$, instead of clicking a link, the surfer jumps to a random page (teleportation). This allows the walker to escape sinks and continue exploring, ensuring comprehensive network sampling and proper convergence.

**Related Topic**: [Handling Sink Nodes in Random Walk](notes.md#handling-sink-nodes-in-random-walk)

---

### Q15 (1 point)
**After plotting in-degree versus PageRank for all 500 papers, Rajesh observes a non-linear scatter plot with notable outliers. Which of the following interpretations are correct?** *(Select all that apply)*

- A) Papers with high in-degree but low PageRank are cited frequently by less influential papers
- B) Papers with low in-degree but high PageRank are cited by highly influential papers
- C) The lack of linear correlation proves that in-degree is completely uncorrelated with PageRank
- D) Papers appearing as outliers with very high PageRank relative to in-degree represent foundational works

**Answer:** A, B, D

**Answer Explanation:** The scatter plot reveals the power of PageRank over simple citation counting:

**(A)** is correct—a paper with many incoming citations (high in-degree) but low PageRank indicates those citations come from low-impact sources. The volume doesn't translate to influence because the citing papers themselves lack authority.

**(B)** is correct—a paper cited only a few times (low in-degree) but with very high PageRank is cited by authoritative papers. A foundational paper might have only 45 direct citations from highly influential works, but each citation carries enormous weight. The algorithm recognizes this quality difference.

**(D)** is correct—papers appearing as outliers with exceptionally high PageRank relative to in-degree are typically foundational works. They may be older papers with only moderate direct citations, but they're cited by seminal papers in the field, making them intellectually critical despite low citation volume. The 1995 neural networks paper with 45 citations ranking 3rd is an example.

**(C)** is incorrect—the scatter plot shows *non-linear* correlation, not zero correlation. In-degree and PageRank are correlated, but not linearly. PageRank weights citations by source authority, breaking the linear relationship with raw in-degree. A general upward trend exists, but with significant deviation due to the quality weighting that PageRank introduces.

**Related Topic**: [The PageRank Philosophy: Recursive Authority](notes.md#the-pagerank-philosophy-recursive-authority)

---

## Answer Key Summary

| Q | Answer | Concept Tested |
|---|--------|----------------|
| 1 | C | Network exploration: n log n relationship for random walk coverage |
| 2 | A, B, D | Points distribution: selective endorsements vs. follower volume |
| 3 | B | PageRank quality: authority from selective mega-influencers |
| 4 | 3.5 | Random walk proportion: visit frequency as PageRank proxy |
| 5 | A, B, C | Graph-based ranking advantages: recursive authority, spam resistance, quality filtering |
| 6 | A | Points distribution mechanics: equal division among outgoing edges |
| 7 | A, B, C | PageRank challenges in sports: damping factors, real-world context, conference effects |
| 8 | A | Directed edge semantics: losers distribute authority to winners |
| 9 | 617 | Points distribution calculation: accumulating from multiple source nodes |
| 10 | A, B, C | Real-world PageRank applications: genes, environment, sports |
| 11 | B | Damping factor and redistribution: retention vs. pool calculation |
| 12 | A, B, C, D | Random walk discrepancies: convergence, stochasticity, damping differences |
| 13 | 84.19 | PageRank vs. in-degree ratio: quality weighting amplification |
| 14 | B | Teleportation necessity: preventing premature walk termination at sinks |
| 15 | A, B, D | Scatter plot interpretation: in-degree vs. PageRank non-linearity |

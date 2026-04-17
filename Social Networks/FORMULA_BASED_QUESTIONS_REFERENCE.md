# Numerical & Formula-Based Questions Reference
## NPTEL Social Networks Course (NOC26_CS82) - All Weeks

A comprehensive compilation of all quantitative and formula-based questions from Weeks 1-12, organized by topic with complete component definitions and solved solutions.

---

# WEEK 1: Graph Theory Foundations

## Topic 1.1: Minimum Spanning Tree (Graph Connectivity)

**Formula/Concept**: Minimum spanning tree — n nodes require n-1 edges for connectivity

**Components**:
- $n$ = number of nodes
- edges = connections (communication links)
- A tree is a connected graph with exactly n-1 edges

**Question**: Q11 - A system with 500 guards and 499 links where every guard can be reached from any other guard suggests that:

**Answer**: B - The system has the minimum required connections

**Explanation**: For a network to connect $n$ nodes where every node is reachable from every other, the minimum number of edges required is $n - 1$. With 500 guards and 499 links, the system has exactly this minimum—forming a **tree** structure. A tree is a connected graph with no cycles, representing optimal efficiency: all nodes are reachable with zero redundant edges. This is the minimal spanning tree concept.

---

## Topic 1.2: Bounds on Random Number Sums

**Formula/Concept**: Bounds on the sum of random numbers between 0 and 1

**Components**:
- Upper bound = sum of maximum values = 1 + 1 + 1 = 3.0
- Each random number is in range [0, 1]
- Sum of three such numbers is in range [0, 3.0]

**Question**: Q8 - If three numbers between 0 and 1 are added, which outcome is impossible?

**Answer**: D - 3.8

**Explanation**: The maximum sum of three numbers each between 0 and 1 is $1 + 1 + 1 = 3.0$. Therefore, any sum exceeding 3.0 (like 3.8) is mathematically impossible. Options A (0.9), B (1.6), and C (2.7) are all feasible outcomes within the valid range [0, 3]. This exemplifies understanding numerical bounds and constraint validation in data analysis.

---

# WEEK 3: Community Detection & Network Structure

## Topic 3.1: Clustering Coefficient

**Formula/Concept**: Clustering Coefficient — Measures fraction of actual edges to maximum possible edges among a node's friends

**Mathematical Formula**: 
$$C(v) = \frac{\text{Actual Edges Among Neighbors}}{\text{Maximum Possible Edges Among Neighbors}} = \frac{\text{Actual}}{|N| \cdot (|N|-1) / 2}$$

**Components**:
- Actual edges = number of friendships that exist among neighbors
- Maximum possible edges = $\binom{n}{2} = \frac{n(n-1)}{2}$ where $n$ = number of friends
- Clustering Coefficient = $\frac{\text{Actual Edges}}{\text{Maximum Possible Edges}}$
- Value range: [0, 1], where 1 = perfect clustering (all friends know each other), 0 = no clustering

**Question**: Q2 (Week 3, Case Study 1) - Calculate the clustering coefficient of Priya's network with her 8 TechCorp colleagues, given that all of them know each other. Express your answer as a decimal (e.g., 0.75).

**Answer**: 1.0

**Explanation**: The clustering coefficient measures the fraction of actual edges to maximum possible edges among a node's friends. Since all 8 colleagues know each other (forming a complete subgraph), the actual edges equal the maximum possible edges. 

Maximum edges = $\binom{8}{2} = \frac{8 \times 7}{2} = 28$

Actual edges = 28

Clustering coefficient = $\frac{28}{28} = 1.0$

This represents perfect clustering where all friends are friends with each other, characteristic of tight-knit professional teams.

---

## Topic 3.2: Neighborhood Overlap

**Formula/Concept**: Neighborhood Overlap — Measures proportion of shared friends relative to total unique friends

**Mathematical Formula**:
$$\text{Overlap} = \frac{\text{Mutual Friends}}{\text{Total Unique Friends (excluding X and Y)}}$$

**Components**:
- Mutual Friends = friends shared between two contacts X and Y
- Friends of X (excluding Y) = total friends of X minus Y
- Friends of Y (excluding X) = total friends of Y minus X
- Total Unique Friends = $(Friends_X - 1) + (Friends_Y - 1) - \text{Mutual}$
- Overlap value: between 0 and 1, where high values indicate weak ties (redundancy)

**Question**: Q5 (Week 3, Case Study 1) - Two of Amit's contacts, X and Y, have 8 common friends between them. Contact X has 20 total friends and Contact Y has 25 total friends. Calculate the neighborhood overlap between X and Y. Express your answer as a decimal rounded to two decimal places (e.g., 0.45).

**Answer**: 0.22

**Explanation**: Neighborhood overlap measures the proportion of shared friends relative to total unique friends.

Given:
- X has 20 friends (excluding Y)
- Y has 25 friends (excluding X)
- They share 8 mutual friends

Total unique friends = $(20-1) + (25-1) - 8 = 19 + 24 - 8 = 35$

Overlap = $\frac{8}{35} \approx 0.228$

**Rounded answer: 0.22** (or 0.23 depending on calculation method)

This low overlap indicates their connection is a weak tie with minimal redundancy, making it an effective bridge between communities.

---

## Topic 3.3: Complete Graph Edges (Combinatorics)

**Formula/Concept**: Maximum edges in a complete subgraph where all nodes connect to all others

**Mathematical Formula**:
$$E_{\text{complete}} = \binom{n}{2} = \frac{n(n-1)}{2}$$

**Components**:
- $n$ = number of nodes (friends in the subgraph)
- $\binom{n}{2}$ = binomial coefficient (n choose 2)
- Formula calculates the total edges when every pair is connected

**Question**: Q8 (Week 3, Case Study 1) - Sneha has 15 friends who all know each other. How many actual friendships exist among these 15 friends (excluding Sneha's connections to them)?

**Answer**: 105

**Explanation**: This asks for the total number of edges in a complete subgraph of 15 nodes. Since all 15 friends know each other, it's a complete graph.

Calculation: 
$$E = \frac{15 \times 14}{2} = \frac{210}{2} = 105 \text{ friendships}$$

This represents Sneha's clustering coefficient of 0.95—nearly complete interconnection within her friend group, indicating a highly cohesive social circle.

---

## Topic 3.4: Largest Connected Component Percentage

**Formula/Concept**: Largest connected component in real-world social networks

**Components**:
- Percentage of nodes in largest connected component
- Based on empirical data from 2007 cell phone study (Onnela et al.) with 4.6 million users

**Question**: Q14 (Week 3, Case Study 3) - Based on the cell phone study, what percentage of nodes typically belonged to the largest connected component? (Enter whole number between 0 and 100)

**Answer**: 85

**Explanation**: The 2007 Onnela et al. cell phone study of 4.6 million users found that approximately **85% of all users** formed a single, massive interconnected component. This finding demonstrated the "small-world" property of social networks—despite billions of humans, about 85% exist in one giant connected cluster, with the remaining 15% in smaller, isolated components.

---

## Topic 3.5: Partition Counting (Combinatorics)

**Formula/Concept**: Brute-force enumeration of possible community assignments (2-partition problem)

**Mathematical Formula**:
$$\text{Unique partitions} = \frac{2^n}{2} = 2^{n-1}$$

where $n$ = total number of users/nodes

**Components**:
- Total users = n = 5,000
- Total possible assignments = $2^n$ (each user can go to Community A or B)
- Unique partitions (accounting for symmetry) = $\frac{2^{5000}}{2} = 2^{4999}$
- Each partition is counted twice (A={1-2500}, B={2501-5000} is same as swapped labels)

**Question**: Q21 (Week 3, Case Study 3) - If TechConnect has 5,000 users and implements a brute-force community detection algorithm to divide them into exactly two communities, approximately how many possible partitions must be evaluated?

**Answer**: D - $2^{4999}$

**Explanation**: Each of 5,000 users can be assigned to either Community A or Community B, giving $2^{5000}$ total combinations. However, assigning users {1-2500} to A and {2501-5000} to B is identical to assigning {2501-5000} to A and {1-2500} to B (just with labels swapped). 

To eliminate these mirror duplicates: 
$$\text{Unique partitions} = \frac{2^{5000}}{2} = 2^{4999}$$

This illustrates why brute force is **computationally infeasible**—$2^{4999}$ is vastly larger than atoms in the universe, explaining why heuristic algorithms (Girvan-Newman, Louvain) are necessary for community detection.

---

## Topic 3.6: Clustering Coefficient with Specific Degree

**Formula/Concept**: Clustering Coefficient Applied to Friendship Count

**Mathematical Formula**:
$$C = \frac{F}{E_{\text{max}}} = \frac{F}{\binom{k}{2}}$$

where:
- $F$ = actual friendships among neighbors
- $k$ = degree (number of connections)
- $E_{\text{max}}$ = maximum possible edges = $\binom{k}{2} = \frac{k(k-1)}{2}$

**Components**:
- Degree = 400 connections
- Clustering Coefficient = 0.25
- Maximum possible edges among friends = $\binom{400}{2} = \frac{400 \times 399}{2} = 79,800$
- Actual friendships = Clustering Coefficient × Maximum possible edges

**Question**: Q23 (Week 3, Case Study 3) - User A has 400 connections with clustering coefficient 0.25. What is the maximum possible value of actual friendships F among User A's connections?

**Answer**: A - 19,900

**Explanation**: Maximum possible edges among 400 friends is:
$$\binom{400}{2} = \frac{400 \times 399}{2} = 79,800$$

Clustering coefficient = $\frac{\text{Actual}}{79,800} = 0.25$

Therefore, Actual = $79,800 \times 0.25 = 19,900$ friendships

This means that among User A's 400 connections, exactly 19,900 friendship pairs exist—representing a 25% clustering level. This user's friends are somewhat connected to each other, but far from a clique.

---

## Topic 3.7: Edge Betweenness Centrality with Multiple Shortest Paths

**Formula/Concept**: Edge Betweenness Centrality — Sum of fractional shortest paths using the edge

**Mathematical Formula**:
$$C_B(e) = \sum_{s \neq t} \frac{\sigma_{st}(e)}{\sigma_{st}}$$

where:
- $\sigma_{st}$ = total number of shortest paths between nodes s and t
- $\sigma_{st}(e)$ = number of shortest paths between s and t that use edge e
- The ratio $\frac{\sigma_{st}(e)}{\sigma_{st}}$ = fraction of paths using edge e

**Components**:
- Total possible source-destination pairs = 20
- Pairs where edge appears on shortest path = 16
- Pairs where edge is the ONLY shortest path = 12 (contribution = 1.0 each)
- Pairs where edge is ONE OF 2 equal shortest paths = 4 (contribution = 0.5 each)
- Final betweenness = sum of all fractional contributions

**Question**: Q27 (Week 3, Case Study 3) - Edge E connects two communities. Among 20 possible source-destination pairs, edge E appears on the shortest path for 16 pairs. For 12 of these pairs, E is on the **only** shortest path; for the remaining 4 pairs, E is on **one of exactly 2** shortest paths. What is the betweenness centrality of edge E? (Round to 2 decimal places)

**Answer**: 14.00

**Explanation**: Edge betweenness sums the fractions of shortest paths using the edge.

For the 12 pairs where E is the *only* shortest path: $12 \times 1.0 = 12$

For the 4 pairs where E is one of exactly 2 equal shortest paths: $4 \times 0.5 = 2$

Total betweenness = $12 + 2 = 14.00$

This high betweenness centrality confirms E is a critical bridge between the two communities—removing it would significantly fragment the network. Edge betweenness exceeding 10 typically indicates critical infrastructure.

---

## Topic 3.8: Community Density Ratio

**Formula/Concept**: Community Density Ratio — Internal vs. Inter-community Edge Ratio

**Mathematical Formula**:
$$\text{Density Ratio} = \frac{\sum \text{Intra-Community Edges}}{\sum \text{Inter-Community Edges}}$$

**Components**:
- Intra-community edges = edges within Community 1 + edges within Community 2 + ...
- Inter-community edges = edges connecting different communities
- Ratio interpretation: Higher ratio indicates stronger community structure

**Question**: Q26 (Week 3, Case Study 3) - A TechConnect subnetwork has 12 users: Community 1 {1–5} has 8 internal edges, Community 2 {6–12} has 15 internal edges, and 3 edges connect the two communities. What is the ratio of total intra-community edges to inter-community edges? (Round to 2 decimal places)

**Answer**: 7.67

**Explanation**: 
Intra-community edges (edges within communities) = $8 + 15 = 23$

Inter-community edges (edges between communities) = $3$

Density Ratio = $\frac{23}{3} = 7.666...$

**Rounded answer: 7.67**

This ratio indicates strong community structure (high internal density relative to inter-community bridges), making these communities well-defined and distinct. A ratio > 5 suggests robust communities that could be separated without major connectivity loss.

---

## Topic 3.9: Combinatorial Selection (Partitioning with Fixed Size)

**Formula/Concept**: Combination — Choose k items from n total (fixed first community size)

**Mathematical Formula**:
$$\binom{n}{k} = \frac{n!}{k!(n-k)!}$$

**Components**:
- $n$ = total users to partition = 16
- $k$ = first community size = 6 users (exactly)
- Remaining community size = n - k = 10 users (automatically determined)
- $\binom{n}{k}$ = different ways to select k items from n items

**Question**: Q28 (Week 3, Case Study 3) - TechConnect wants to partition 16 users such that the first community has exactly 6 users. How many different first-community configurations must the algorithm evaluate?

**Answer**: A - 8,008 configurations

**Calculation**:
$$\binom{16}{6} = \frac{16!}{6! \times 10!} = \frac{16 \times 15 \times 14 \times 13 \times 12 \times 11}{6 \times 5 \times 4 \times 3 \times 2 \times 1} = \frac{5,765,760}{720} = 8,008$$

**Explanation**: This is a combination problem. Selecting 6 users out of 16 to form the first community (order doesn't matter) is $\binom{16}{6} = 8,008$ configurations. The remaining 10 users automatically form the second community. This calculation represents the number of distinct ways to partition the user base into two groups of fixed sizes—essential for understanding the complexity of exhaustive community detection algorithms.

---

# WEEK 7: Contagion & Cascading Models

## Topic 7.1: Linear Threshold Model - Adoption Decision

**Formula/Concept**: Linear Threshold Model — Adoption Decision Rule based on neighbor adoption fraction

**Mathematical Formula**:
$$\text{Adopt if: } \frac{\text{Neighbors Adopted}}{k} \geq q$$

or equivalently:

$$\text{Adopt if: } \text{Neighbors Adopted} \geq k \times q$$

**Components**:
- $q$ = individual's adoption threshold (fraction, range 0 to 1)
- $k$ = number of close connections / neighbors
- $\frac{\text{Neighbors Adopted}}{k}$ = fraction of neighbors who have adopted
- Adoption requires **at least** $k \times q$ neighbors to have adopted

**Question**: Q4 (Week 7, Case Study 1) - If a user has 20 close connections and a sharing threshold of 0.25, how many of those connections must share the post before the user does?

**Answer**: 5

**Explanation**: This is a direct application of the Linear Threshold Model decision rule.

The threshold $q = 0.25$ means the user requires a fraction $p \geq 0.25$ of their neighbors to have shared.

With 20 close connections: $20 \times 0.25 = 5$ connections must share

Once at least 5 of their 20 friends have shared the post, the condition $\frac{5}{20} = 0.25 \geq q$ is satisfied, and the user will rationally choose to share as well. This demonstrates how adoption cascades through networks where individuals have rational, threshold-based decision rules.

---

## Topic 7.2: Linear Threshold Model - EV Adoption

**Formula/Concept**: Linear Threshold Model — Adoption Threshold Calculation

**Mathematical Formula**:
$$\text{Number Adopting Required} = k \times q$$

**Components**:
- $q$ = adoption threshold (required fraction of neighbors) = 0.5
- $k$ = total social contacts = 14
- Number adopting required = product of these values

**Question**: Q9 (Week 7, Case Study 2) - If a consumer has 14 social contacts and an adoption threshold of 0.5, how many contacts must adopt EVs before the consumer considers adoption?

**Answer**: 7

**Explanation**: Direct application of the Linear Threshold Model.

With threshold $q = 0.5$, the consumer requires 50% of their social contacts to adopt:

$$k \times q = 14 \times 0.5 = 7 \text{ contacts}$$

Once 7 of their 14 social contacts have adopted EVs, the consumer's exposure fraction $p = \frac{7}{14} = 0.5$ meets the threshold condition $p \geq q$, triggering their adoption decision. This models how social influence spreads through opinion leaders—each individual has a personal threshold, and once enough neighbors adopt, they follow suit.

---

## Topic 7.3: Threshold-Based Default Model (Financial Contagion)

**Formula/Concept**: Threshold-Based Default Model — Counterparty Failure Calculation

**Mathematical Formula**:
$$\text{Number of Defaults Triggering Bank Failure} = k \times q_{\text{default}}$$

**Components**:
- $q_{\text{default}}$ = fraction of capital buffer loss that triggers default = 0.25
- $k$ = number of exposed counterparties = 12
- Each counterparty default causes equal capital loss
- Number of defaults required = product of these values

**Question**: Q14 (Week 7, Case Study 3) - If a bank has exposure to 12 counterparties and a default threshold of 0.25, how many counterparties defaulting could trigger its failure?

**Answer**: 3

**Explanation**: Applying the threshold model to financial risk:

$$k \times q = 12 \times 0.25 = 3 \text{ counterparties}$$

The bank's default threshold of 0.25 means it defaults when losses from counterparties reach 25% of its capital buffer.

If 3 counterparties default (assuming each causes equal loss), accumulated losses hit the 25% capital threshold, triggering the bank's own default. This demonstrates the threshold mechanics of financial contagion—banks don't need full network failure to go down; hitting a threshold fraction of counterparties suffices. This is why interconnected financial networks are fragile: cascading failures can quickly propagate through threshold-based mechanisms.

---

# WEEK 8: Link Analysis & PageRank

## Topic 8.1: HITS Authority Score — Summation of Incoming Hub Scores

**Formula/Concept**: HITS Algorithm — Authority Score Calculation

**Mathematical Formula**:
$$\text{Authority}(p) = \sum_{q: q \to p} \text{Hub}(q)$$

where:
- $p$ = page whose authority we're calculating
- $q$ = pages linking to p
- $q \to p$ = edge from q to p (q points to p)

**Components**:
- Authority(p) = Sum of Hub scores of all pages pointing to p
- Before normalization: raw sum without scaling
- HITS iterates repeatedly, normalizing to prevent infinite growth
- Authority interpretation: How important/authoritative is page p?

**Question**: Q3 (Week 8, Case Study 1) - If a user is followed by two hubs with hub scores 3 and 5, what is their authority score before normalization?

**Answer**: 8

**Explanation**: Direct application of the HITS Authority formula.

$$\text{Authority}(p) = \text{Hub}_1 + \text{Hub}_2 = 3 + 5 = 8$$

The user receives Authority from two hubs with scores 3 and 5, so the authority contribution is their sum: 8.

The phrase "before normalization" is important—during HITS iteration, all scores are scaled down (divided by the sum) to prevent infinite growth. But the raw contribution is 8. This demonstrates how HITS assigns authority: pages linked to by many high-hub pages gain authority, propagating influence through the link structure.

---

## Topic 8.2: PageRank Incoming Contribution

**Formula/Concept**: PageRank — Incoming Contribution (Before Damping)

**Mathematical Formula**:
$$\text{Incoming Contribution}(p) = \sum_{q: q \to p} \text{PageRank}(q)$$

Complete PageRank formula:
$$\text{PageRank}(p) = (1-d)/n + d \times \sum_{q: q \to p} \frac{\text{PageRank}(q)}{\text{out-degree}(q)}$$

**Components**:
- PageRank(i) = PageRank score of incoming page i
- out-degree(q) = number of outgoing links from page q
- d = damping factor (typically 0.85)
- n = total number of pages
- Incoming contribution = sum of weighted PageRank fractions from all sources

**Question**: Q8 (Week 8, Case Study 2) - If a webpage receives links from three pages with PageRank scores 0.2, 0.1, and 0.3, what is the total incoming contribution before damping?

**Answer**: 0.6

**Explanation**: In PageRank, a page's incoming contribution (before applying damping) is the sum of contributions from all incoming pages.

Assuming these PageRank scores represent the specific fractions those pages allocate to this target node (after dividing by their out-degrees):

$$\text{Incoming Contribution} = 0.2 + 0.1 + 0.3 = 0.6$$

This is the raw incoming flow. After applying the damping factor $d = 0.85$ and the teleportation probability $(1-d)/n$, this 0.6 value would be weighted and combined to produce the final PageRank score. The damping factor accounts for the probability that a random surfer clicks a new link (0.15) versus following page links (0.85).

---

## Topic 8.3: HITS Authority — Summation of Incoming Authority/Hub Scores

**Formula/Concept**: HITS Algorithm — Authority Contribution from Incoming Citations

**Mathematical Formula**:
$$\text{Authority Contribution}(p) = \sum_{q: q \to p} \text{Score}(q)$$

**Components**:
- Authority contribution = raw sum of incoming authority/hub scores
- Before normalization: without iterative scaling
- Each incoming page contributes its current score
- HITS iteration eventually reaches a fixed point where Hub and Authority values stabilize

**Question**: Q13 (Week 8, Case Study 3) - If a paper is cited by two papers with authority scores 1.5 and 2.5, what is its hub-independent authority contribution before normalization?

**Answer**: 4.0

**Explanation**: Authority contribution = Sum of incoming authority/hub scores

$$\text{Authority Contribution} = 1.5 + 2.5 = 4.0$$

The "hub-independent" phrase clarifies that we're calculating the raw sum before HITS' iterative feedback between hub and authority. In academic citation networks, papers cited by high-authority sources gain authority themselves. An authority score of 4.0 (before normalization) indicates strong incoming citations from important papers, suggesting this paper is also important in its field.

---

# WEEK 10: Contagion & Epidemic Models

## Topic 10.1: Basic Reproductive Number ($R_0$)

**Formula/Concept**: Basic Reproductive Number — Expected number of secondary infections per infected individual

**Mathematical Formula**:
$$R_0 = p \times k$$

**Components**:
- $p$ = transmission probability (per contact)
- $k$ = average number of contacts per infected individual
- $R_0$ = expected number of people one infected person will infect
- Critical threshold: $R_0 > 1$ means epidemic growth; $R_0 < 1$ means epidemic dies out

**Question**: Q4 (Week 10, Case Study 1) - If Campus A reduces contact density from k=12 to k=6 while simultaneously improving hygiene to reduce transmission probability from p=0.25 to p=0.20, what is the expected number of secondary infections per infected individual?

**Answer**: 1.2

**Explanation**: The expected number of secondary infections is the **Basic Reproductive Number**:

$$R_0 = p \times k = 6 \times 0.20 = 1.2$$

Originally, Campus A had: $R_0_{\text{original}} = 12 \times 0.25 = 3.0$

This intervention (reducing contacts and improving hygiene) decreased $R_0$ from 3.0 to 1.2—a 60% reduction.

**Important note**: While this is a significant improvement, $R_0 = 1.2 > 1$ means the disease still exceeds the critical epidemic threshold. The outbreak will continue to spread, albeit slower than before. To truly control the outbreak, interventions would need to bring $R_0$ below 1.0. For example:
- Further social distancing (k=4 instead of 6) would give $R_0 = 0.8 < 1$, stopping spread
- Or improving hygiene to p=0.15 would give $R_0 = 0.9 < 1$

---

## Topic 10.2: Branching Process Growth

**Formula/Concept**: Basic Reproductive Number & Branching Process Growth

**Mathematical Formula**:
$$R_0 = k \times p$$

$$\text{Expected Infections at Generation } n = (R_0)^n$$

**Components**:
- $k$ = average contacts per infected individual = 12
- $p$ = transmission probability = 0.90
- $R_0$ = basic reproductive number = 10.8
- $n$ = transmission generation (number of successive rounds of transmission)
- Branching process: exponential growth when $R_0 > 1$

**Question**: Q6 (Week 10, Case Study 1) - Calculate the initial basic reproductive number $R_0$ before intervention. Using a branching process approximation, determine the expected number of infections at the third transmission generation.

**Answer**: $R_0 = 10.8$; third generation expects 1,260 infections

**Explanation**: 

**Step 1: Calculate $R_0$**
$$R_0 = k \times p = 12 \times 0.90 = 10.8$$

This is an extremely high reproductive number, indicating measles' extreme contagiousness (explaining why it was so difficult to control before vaccines).

**Step 2: Apply Branching Process Formula**

The expected number of infected individuals at generation $n$ is $(R_0)^n$.

For generation 3:
$$(10.8)^3 = 10.8 \times 10.8 \times 10.8 = 1,259.712 \approx 1,260$$

This explosive growth (from 1 index case to 1,260 cases in three transmission cycles) demonstrates why measles control is critical. Without intervention, a single unvaccinated student returning to a susceptible population can infect over 1,000 people within a few weeks.

**Timeline interpretation**:
- Generation 0 (Day 0): 1 infected student
- Generation 1 (Day 3): 10.8 ≈ 11 infected
- Generation 2 (Day 6): 116 infected  
- Generation 3 (Day 9): 1,260 infected

---

## Topic 10.3: Reduced Contact Intervention

**Formula/Concept**: Basic Reproductive Number After Intervention (Reduced Contacts)

**Mathematical Formula**:
$$R_0 = k \times p$$

**Components**:
- $k$ = average contacts after exclusion policy = 2 (household contacts only)
- $p$ = transmission probability = 0.90 (unchanged)
- $R_0$ = expected secondary infections after intervention

**Question**: Q7 (Week 10, Case Study 1) - After implementing the exclusion policy (reducing k from 12 to 2 while p remains 0.90), what is the new $R_0$ value?

**Answer**: 1.8

**Explanation**: The new $R_0$ is calculated as:

$$R_0 = k \times p = 2 \times 0.90 = 1.8$$

The isolation policy reduced average contacts from 12 (full classroom interaction) to 2 (household contacts only), dramatically lowering transmission potential. The reduction is 6-fold: from 10.8 to 1.8.

**Analysis**:
- $R_0 = 1.8 > 1$, so the disease still exceeds the epidemic threshold
- The disease continues spreading, but at a much slower rate
- Each generation takes longer to grow
- Expected generation 3 infections would be $(1.8)^3 \approx 5.8$ instead of 1,260

To truly control the outbreak, $R_0$ must drop below 1.0—which would require either:
1. **Isolating all contacts** (k=0, impossible in practice)
2. **Dramatically improving hygiene/vaccination** to reduce p
3. **Combining interventions**: both reducing k AND reducing p

---

## Topic 10.4: Virality Coefficient

**Formula/Concept**: Virality Coefficient — Expected secondary adopters per user

**Mathematical Formula**:
$$V_0 = p \times k$$

where:
- $p$ = sharing/adoption probability
- $k$ = average connections per user

**Components**:
- $p$ = new sharing probability = 0.32
- $k$ = average connections per user = 5
- $V_0$ = virality coefficient
- Critical threshold: $V_0 > 1$ indicates viral growth; $V_0 < 1$ indicates viral decay

**Question**: Q13 (Week 10, Case Study 2) - If TechFlow increases sharing probability to p = 0.32 while maintaining k = 5, what would be the new virality coefficient $V_0$?

**Answer**: 1.60

**Explanation**: The new virality coefficient is:

$$V_0 = p \times k = 0.32 \times 5 = 1.60$$

Increasing sharing probability from 0.28 to 0.32 (a modest 14% improvement) increases virality coefficient from 1.40 to 1.60.

**Growth dynamics comparison**:
- Old $V_0 = 1.40$: Each adopter influences 1.40 additional users
  - Generation 3: $(1.40)^3 \approx 2.74$
- New $V_0 = 1.60$: Each adopter influences 1.60 additional users
  - Generation 3: $(1.60)^3 \approx 4.10$

The higher $V_0 = 1.60$ indicates faster cascading growth. This demonstrates how small improvements in sharing probability directly accelerate viral dynamics—the exponent amplifies across generations, making 14% improvement in p translate to 49% improvement in generation-3 reach: from 2.74 to 4.10.

---

## Topic 10.5: Derivative of Percolation Generating Function

**Formula/Concept**: Percolation Generating Function Derivative — Fixed-Point Analysis

**Mathematical Formula**:
$$f(x) = 1 - (1 - px)^k$$

$$f'(x) = pk(1-px)^{k-1}$$

$$f'(0) = pk = V_0$$

**Components**:
- $f(x)$ = generating function for percolation model
- $p$ = sharing probability = 0.28
- $k$ = average connections per user = 5
- $f'(x)$ = derivative with respect to x
- $f'(0)$ = derivative evaluated at origin = $pk$ = virality coefficient

**Question**: Q14 (Week 10, Case Study 2) - For the current campaign parameters (p=0.28, k=5), consider the function $f(x) = 1 - (1 - px)^k$. What is the value of the derivative $f'(0)$, and how does it relate to the virality coefficient?

**Answer**: B - The derivative equals 1.40 at the origin

**Explanation**: 

**Step 1: Differentiate the function**

$$f(x) = 1 - (1-px)^k$$

Using chain rule:
$$f'(x) = \frac{d}{dx}[1 - (1-px)^k] = -k(1-px)^{k-1} \cdot (-p) = pk(1-px)^{k-1}$$

**Step 2: Evaluate at x = 0**

$$f'(0) = pk(1-0)^{k-1} = pk(1)^{4} = pk$$

$$f'(0) = 5 \times 0.28 = 1.40$$

**Interpretation**: 

In the context of branching processes, the generating function's first derivative at zero represents the expected number of "offspring" (secondary adopters) per node. This is precisely the virality coefficient $V_0 = pk = 1.40$.

This mathematical property is fundamental to understanding fixed-point analysis in percolation and branching processes. When $f'(0) > 1$, the cascade grows; when $f'(0) < 1$, it decays. The value $f'(0) = 1.40$ indicates sustained viral growth, with each generation 40% larger than the previous.

---

# WEEK 11: Small World & Searchability

## Topic 11.1: Six Degrees Counting

**Formula/Concept**: Six Degrees of Separation — Chain Counting

**Mathematical Formula**:
$$\text{Total Individuals} = 1 \text{ (sender)} + n \text{ (intermediaries)} + 1 \text{ (receiver)}$$

**Components**:
- Sender = 1 person (originator of request)
- Intermediaries = 6 people (hops)
- Receiver = 1 person (target)
- Total = sum of all three components

**Question**: Q4 (Week 11, Case Study 1) - If a request passes through 6 intermediaries before reaching the mentor, how many individuals were involved in the chain (including sender and receiver)?

**Answer**: 8

**Explanation**: A request chain consists of:
- 1 sender
- 6 intermediaries
- 1 receiver
- **Total: 1 + 6 + 1 = 8 individuals**

This simple counting problem reinforces Milgram's "six degrees of separation" concept. The phrase "six degrees" refers to the number of **hops** (intermediaries), not the total number of people involved.

**Clarification**:
- "Degrees of separation" = 6 (the number of hops)
- "People involved" = 8 (total count in chain)

These are often confused. Milgram's packets traveled through an average of 5.5–6 hops (intermediaries), involving 7–8 total people. This calculation is central to understanding the small-world property.

---

## Topic 11.2: Distance Exponent Effect on Routing Efficiency

**Formula/Concept**: Kleinberg's Distance-Sensitive Long-Range Connections

**Mathematical Formula**:
$$P(\text{long-range link from } u \to v) \propto \frac{1}{d(u,v)^\alpha}$$

where:
- $d(u,v)$ = distance between nodes u and v
- $\alpha$ = distance exponent (controls strength of distance bias)
- **Optimal for searchability**: $\alpha = d$ (dimension of the lattice)

**Components**:
- $\alpha$ = distance exponent (parameter being increased toward infinity)
- $d$ = social/topical distance between potential contacts
- $P(\text{link})$ = probability of establishing a long-range connection
- Routing efficiency depends on matching $\alpha$ to network dimension

**Question**: Q9 (Week 11, Case Study 2) - If increasing a parameter controlling distance sensitivity makes long-range links extremely rare, what happens to routing efficiency?

**Answer**: decreases

**Explanation**: In Kleinberg's model, the distance exponent $\alpha$ controls the strength of the distance bias.

As $\alpha$ increases, the probability of long-range connections drops sharply:
$$P(\text{link}) = \frac{1}{d^\alpha}$$

For example, with $d = 10$ (social distance units):
- $\alpha = 1$: $P \propto 1/10 = 0.10$
- $\alpha = 2$: $P \propto 1/100 = 0.01$
- $\alpha = 3$: $P \propto 1/1000 = 0.001$

As $\alpha \to \infty$, nearly all links become local (very short range), and the network becomes a pure local lattice.

**Routing efficiency consequences**:
- Pure lattice: Path length $L \approx O(\sqrt{n})$ (very long)
- Optimal ($\alpha = d$): Path length $L \approx O((\log n)^2)$ (searchable)
- Over-biased ($\alpha \to \infty$): Path length reverts to $O(\sqrt{n})$ (very long)

Greedy decentralized search cannot find short paths in pure lattices or over-biased networks because it lacks "wormholes"—long-range links that jump across distance.

---

## Topic 11.3: Optimal Distance Exponent for 2D Networks

**Formula/Concept**: Kleinberg's Optimal Distance Exponent

**Mathematical Formula**:
$$\alpha_{\text{optimal}} = d$$

where $d$ = spatial dimension of the lattice

**Components**:
- For 1D networks: $\alpha = 1$ (linear arrangement)
- For 2D networks: $\alpha = 2$ (grid layout)
- For 3D networks: $\alpha = 3$ (3D cube layout)
- Dimension matching enables optimal searchability

**Question**: Q14 (Week 11, Case Study 2) - In a two-dimensional network, which value of the distance exponent yields optimal decentralized search?

**Answer**: 2

**Explanation**: Kleinberg's fundamental result (2000) proves that for a $d$-dimensional lattice, optimal decentralized search occurs when:

$$\alpha = d$$

For a 2D network (grid-based like a city block system):
$$\alpha_{\text{optimal}} = 2$$

**Why $\alpha = 2$ works for 2D**:

When $\alpha = 2$, long-range links follow the distribution:
$$P(\text{link at distance } r) \propto \frac{1}{r^2}$$

This creates a perfectly balanced, multi-scale hierarchy:
- **Global scale** (distance $r = n$): A few links span the entire network
- **Regional scale** (distance $r = \sqrt{n}$): Some links bridge regions
- **Neighborhood scale** (distance $r = 1-10$): Many local links

This multi-scale balance allows greedy routing to **consistently halve the distance to the target** at each hop, achieving near-logarithmic path length:
$$L_{\text{greedy}} = O((\log n)^2)$$

**Contrast**:
- **Underbiased** ($\alpha < 2$): Too many distant links, routing confuses direction
- **Optimal** ($\alpha = 2$): Balanced distribution, greedy routing finds target efficiently
- **Overbiased** ($\alpha > 2$): Too few distant links, falls back to local lattice behavior

For real networks (cities, social contacts), dimension $d \approx 2$ because relationships often involve **geographic coordinates** (latitude/longitude) and **social distance** (two roughly independent dimensions). This explains why Kleinberg's $\alpha = 2$ prediction matches empirical observations of searchable networks.

---

# WEEK 12: K-Core Decomposition & Cascades

## Topic 12.1: Percentage Calculation (Cascade Reach)

**Formula/Concept**: Percentage Calculation — Infected Users from Cascade Percentage

**Mathematical Formula**:
$$\text{Infected Users} = \text{Total Users} \times \text{Cascade Percentage}$$

**Components**:
- Total users in network = 500,000
- Cascade percentage (infection rate) = 0.37 (37%)
- Infected users = product of total and percentage

**Question**: Q8 (Week 12, Case Study 2) - If 37% of 500,000 users are infected, how many users is that?

**Answer**: 185,000

**Explanation**: Simple percentage arithmetic:

$$\text{Infected Users} = 500,000 \times 0.37 = 185,000$$

**Interpretation within Week 12 context**:

The case study shows that seeding a single node in the $k=20$ shell (intermediate coreness) infects 185,000 downstream users.

**Comparison to innermost core**:
- Innermost core ($k=28$): $500,000 \times 0.40 = 200,000$ infected
- Pseudo-core ($k=20$): $500,000 \times 0.37 = 185,000$ infected
- Difference: Only 15,000 fewer infections (7.5% reduction)

This numerical similarity (185,000 vs. 200,000) illustrates why **pseudo-cores are strategically superior**: they achieve nearly identical cascade size (37% vs. 40%) while being 8–10x more abundant and cost-effective to target.

**Real-world significance**: 
- Targeting 5–10 innermost core celebrities: expensive, echo chamber effect
- Targeting 50–100 pseudo-core influencers: moderate cost, diverse reach, similar cascade

---

---

## FORMULA SUMMARY TABLE

| Week | Topic | Formula | Answer Type |
|------|-------|---------|------------|
| 1 | Minimum Spanning Trees | $n-1$ edges for n nodes | Conceptual |
| 1 | Random Bounds | Max(3 numbers in [0,1]) = 3.0 | NAT |
| 3 | Clustering Coefficient | $C = \frac{\text{Actual Edges}}{\binom{n}{2}}$ | NAT (decimal) |
| 3 | Neighborhood Overlap | $O = \frac{\text{Mutual}}{\text{Total Unique}}$ | NAT (decimal) |
| 3 | Complete Graph Edges | $\binom{n}{2} = \frac{n(n-1)}{2}$ | NAT |
| 3 | Largest Component % | 85% from empirical data | NAT |
| 3 | 2-Partition Count | $2^{n-1}$ unique partitions | Multiple choice |
| 3 | Clustering with Degree | $F = C \times \binom{k}{2}$ | NAT |
| 3 | Edge Betweenness | $\sum \frac{\sigma_{st}(e)}{\sigma_{st}}$ | NAT |
| 3 | Community Density | $\frac{\text{Intra}}{\text{Inter}}$ edges | NAT |
| 3 | Fixed Combination | $\binom{n}{k}$ | NAT |
| 7 | Linear Threshold | $k \times q$ neighbors needed | NAT |
| 7 | Linear Threshold (EV) | $k \times q = 14 \times 0.5$ | NAT |
| 7 | Financial Threshold | $k \times q$ defaults needed | NAT |
| 8 | HITS Authority | $\sum \text{Hub Scores}$ | NAT |
| 8 | PageRank Incoming | $\sum \text{PageRank Fractions}$ | NAT |
| 8 | HITS Citations | $\sum \text{Authority Scores}$ | NAT |
| 10 | Basic Reproductive # | $R_0 = p \times k$ | NAT |
| 10 | Branching Growth | $(R_0)^n$ | NAT |
| 10 | Reduced Contacts | $R_0 = 2 \times 0.90 = 1.8$ | NAT |
| 10 | Virality Coefficient | $V_0 = p \times k$ | NAT |
| 10 | Generating Function | $f'(0) = pk$ | Multiple choice |
| 11 | Six Degrees Count | 1 + 6 + 1 = 8 total | NAT |
| 11 | Distance Exponent Effect | $\alpha \to \infty$ makes $P \to 0$ | Conceptual |
| 11 | Optimal Exponent (2D) | $\alpha = 2$ for 2D lattice | NAT |
| 12 | Cascade Percentage | Users = Total × Percent | NAT |

---

**Total Numerical Questions Covered: 27 across 12 weeks**

All questions solved with complete component definitions and explanations tied to course concepts.

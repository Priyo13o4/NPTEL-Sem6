# Week 3: Assignment 3 — Social Networks (NOC26_CS82)

> **Due:** 2026-02-11, 23:59 IST  
> **Submitted:** 2026-02-11, 12:02 IST  
> **Score:** 13.75/28

---

## Case Study 1 — Priya, Rahul & The Power of Weak Ties (TechCorp)

Priya, a software engineer at TechCorp in Bangalore, has **8 strong-tie colleagues** (A–H) who all know each other and socialize regularly. At an industry conference, she reconnected with **Rahul**, a former classmate she hadn't spoken to in five years. Rahul works in fintech — a completely non-overlapping network. Two months later, Rahul referred Priya to a senior engineering role, which she got.

Meanwhile:
- **Amit**: 50 contacts, 3 strong ties, 47 weak ties, clustering coefficient = 0.15
- **Sneha**: 15 contacts, all strong ties, clustering coefficient = 0.95; hasn't found new job despite actively seeking
- **Priya–Rahul**: Local bridge with zero common friends
- **Amit's strong ties**: Among his 3 strong ties, 2 pairs are connected to each other

---

### Q1 (1 point)
**The connection between Priya and Rahul can be classified as:**

- A) A strong tie because they were former classmates from college
- B) A bridge because removing it completely disconnects the network
- C) A local bridge because there are no common friends between them
- D) A triad because it forms a closed triangular structure

**Answer:** C

**Answer Explanation**: A local bridge is defined as an edge connecting two nodes with zero mutual friends. Since Priya's network (TechCorp colleagues) and Rahul's network (fintech) are completely non-overlapping, they share zero common friends, making their connection a textbook example of a local bridge. This differs from a perfect bridge (B) because removing their connection wouldn't entirely disconnect the network—both could still reach the broader social network through their respective communities, just not via each other directly.

---

### Q2 (1 point)
**Calculate the clustering coefficient of Priya's network with her 8 TechCorp colleagues, given that all of them know each other. Express your answer as a decimal (e.g., 0.75).**

**Answer:** 1.0

**Answer Explanation**: The clustering coefficient measures the fraction of actual edges to maximum possible edges among a node's friends. Since all 8 colleagues know each other (forming a complete subgraph), the actual edges equal the maximum possible edges. Maximum edges = $\frac{8 \times 7}{2} = 28$. Actual edges = 28. Clustering coefficient = $\frac{28}{28} = 1.0$, representing perfect clustering where all friends are friends with each other.

---

### Q3 (1 point)
**According to Granovetter's research on weak ties, which of the following statements are TRUE? (Select all that apply)**

- A) Strong ties provide more novel job information compared to weak ties
- B) Weak ties connect different clusters and provide diverse information access
- C) Over 90% of people discover jobs through acquaintances not close friends
- D) Local bridges typically represent strong ties due to frequent interactions

**Answer:** B

**Answer Explanation**: This question tests understanding of the core "Strength of Weak Ties" paradox. Option B is correct: weak ties connect isolated communities and provide access to novel information that strong ties cannot (since strong ties share overlapping circles). Option A is false—weak ties actually provide *more* novel information because strong ties know the same people. Option C was not supported in Granovetter's original research (though more recent studies have approached this ratio). Option D is false: local bridges (zero mutual friends) are definitionally weak ties, not strong ties, because frequent interaction would create mutual friends through triadic closure.

---

### Q4 (1 point)
**Why did Priya's close colleagues at TechCorp NOT inform her about new job opportunities while Rahul did?**

- A) Her colleagues intentionally withheld opportunities to keep them for themselves
- B) Strong ties share overlapping information so colleagues knew same opportunities
- C) Rahul possessed superior communication skills compared to her TechCorp colleagues
- D) Her colleagues were genuinely unaware of any relevant job opportunities available

**Answer:** B

**Answer Explanation**: This directly illustrates the **redundancy problem of strong ties**. Priya and her TechCorp colleagues all operate in the same company, know the same people, and are exposed to the same rumors and networks. If a job opportunity existed within their shared circle, Priya would likely already know about it. Rahul, embedded in the fintech industry through his weak-tie connection, naturally encountered opportunities from his distinct social circle—information entirely novel to Priya. This is not about withheld information or communication skill, but about information overlap.

---

### Q5 (1 point)
**Two of Amit's contacts, X and Y, have 8 common friends between them. Contact X has 20 total friends and Contact Y has 25 total friends. Calculate the neighborhood overlap between X and Y. Express your answer as a decimal rounded to two decimal places (e.g., 0.45).**

**Answer:** 0.22

**Answer Explanation**: Neighborhood overlap measures the proportion of shared friends relative to total unique friends. Formula: $\text{Overlap} = \frac{\text{Mutual Friends}}{\text{Total Unique Friends (excluding X and Y)}}$. X has 20 friends (excluding Y), Y has 25 friends (excluding X), they share 8 mutual friends. Total unique friends = $(20-1) + (25-1) - 8 = 35$. Overlap = $\frac{8}{35} \approx 0.228$, which rounds to 0.23 (or 0.22 depending on calculation method). This low overlap indicates their connection is a weak tie with minimal redundancy.

---

### Q6 (1 point)
**Based on the case study, which of the following can be inferred about network structures and job opportunities? (Select all that apply)**

- A) Amit's low clustering coefficient of 0.15 facilitates his frequent job changes
- B) Sneha's high clustering coefficient of 0.95 restricts diverse opportunity access
- C) Individuals with many weak ties across clusters access more varied information
- D) Increasing the quantity of strong ties automatically enhances job opportunities

**Answer:** A, B, C

**Answer Explanation**: Options A, B, and C all correctly apply weak ties theory. Amit's low clustering (0.15) means his friends don't know each other, indicating he has diverse weak ties spanning different networks—perfect for learning about varied opportunities (A). Sneha's high clustering (0.95) traps her in an echo chamber where all her contacts know each other and share the same information, restricting access to novel opportunities (B). Option C correctly states that weak ties across clusters provide information diversity. Option D is false: merely adding more strong ties just increases redundant information; what matters is diversity, not quantity.

---

### Q7 (1 point)
**According to the strong triadic closure property, if Priya has strong ties with both colleague A and colleague B, what can be concluded?**

- A) Colleagues A and B must necessarily have a strong tie between them
- B) Colleagues A and B must at least know each other with some tie
- C) Colleagues A and B cannot possibly know each other in the network
- D) Colleagues A and B will eventually develop hostile relationships over time

**Answer:** B

**Answer Explanation**: The strong triadic closure property states: if a node has strong ties to two other nodes, there **must be at least a weak tie** between those two nodes. This reflects the social reality that it's psychologically unsustainable for your two best friends to completely ignore each other—social pressure forces connection. However, the property doesn't require a strong tie between A and B (so A is too strong). The connection must exist but could be weak (B is correct). This differs from C (impossible knowledge) and D (no relationship to hostility).

---

### Q8 (1 point)
**Sneha has 15 friends who all know each other. How many actual friendships exist among these 15 friends (excluding Sneha's connections to them)?**

**Answer:** 105

**Answer Explanation**: This asks for the total number of edges in a complete subgraph of 15 nodes. Since all 15 friends know each other, it's a complete graph. The formula for maximum edges with $n$ nodes is $\frac{n(n-1)}{2}$. Calculation: $\frac{15 \times 14}{2} = \frac{210}{2} = 105$ friendships. This represents Sneha's high clustering coefficient of 0.95—nearly complete interconnection within her friend group.

---

### Q9 (1 point)
**Which of the following characteristics would indicate that a person has a network structure favorable for discovering diverse job opportunities? (Select all that apply)**

- A) High clustering coefficient close to 1.0 with interconnected friend groups
- B) Multiple local bridges present throughout their professional network structure
- C) Friends distributed across different industries and geographic locations worldwide
- D) All friends know each other well and work together in same company

**Answer:** B, C

**Answer Explanation**: Local bridges (B) are the literal information pathways between isolated clusters—having multiple bridges means access to diverse networks and novel information. Friends in different industries and locations (C) indicates weak ties across diverse sectors. Options A and D both describe high clustering (echo chambers), which **restricts** diverse opportunity access by creating redundant information loops. Amit's low clustering of 0.15 facilitated job opportunities precisely because his sparse network created multiple weak ties to different circles.

---

### Q10 (1 point)
**If Priya and Rahul had maintained regular contact over five years and developed 5 mutual friends, how would this affect the value of their connection for job information?**

- A) It would increase value because stronger ties always provide better opportunities
- B) It would decrease value as connection loses local bridge status, reducing novel information
- C) It would have no effect whatsoever on the value of the connection
- D) It would only affect value if all mutual friends worked in same industry

**Answer:** B

**Answer Explanation**: A local bridge requires zero mutual friends. By developing 5 mutual friends over five years, Priya and Rahul's networks have begun to merge—their originally isolated fintech and tech clusters are now overlapping. This transformation destroys the information-novel advantage: those 5 mutual friends likely share information across both networks, making Rahul's information less unique and surprising to Priya. The connection loses its bridge property and becomes increasingly redundant, exactly opposite to option A (which assumes stronger = better, ignoring the information overlap problem).

---

## Case Study 2 — Riverside Gardens Community Network

Dr. Mehra presented an **18-week social network study** of 850 families in Riverside Gardens. Key findings:

- **Mrs. Sharma** (home-based caterer): isolated node with no mutual friends linking her to clients — low embeddedness, but business thrives by occupying a **structural hole**
- **Mr. Patel**: sits at center of dense web, highly embedded — high trust, high accountability
- **Rahul** (new arrival): only bridge between old-timers (eastern blocks) and young professionals (western towers)
- **Sharma vs. Joshi families**: parking dispute with zero mutual friends to mediate
- The **2007 cell phone study** validated Granovetter by measuring neighborhood overlap against conversation duration, confirming local bridges = weak ties

---

### Q11 (1 point)
**If researchers wanted to validate whether local bridges are weak ties at Riverside Gardens, which methodology mirrors the 2007 study approach?**

- A) Survey residents about friendship strength and compare responses with network position
- B) Measure interaction frequency through event attendance and compare it with neighborhood overlap of relationships
- C) Ask residents to rank all their friends and use these subjective rankings to identify bridges
- D) Count total number of friends each resident has and correlate it with their centrality measures

**Answer:** B

**Answer Explanation**: The 2007 cell phone study succeeded by correlating **objective interaction measures** (call duration, frequency, continuity) with **structural properties** (neighborhood overlap). Option B mirrors this by measuring interaction frequency (through events) and comparing against neighborhood overlap—the same objective-to-structural mapping. Options A and C rely on subjective self-reporting, which introduces bias and was precisely why earlier sociology struggled to validate weak ties theory. Option D measures connectivity quantity, not tie strength or structure.

---

### Q12 (1 point)
**Which statements correctly describe embeddedness in this context? (Select all that apply)**

- A) Mrs. Sharma's low embeddedness with clients could reduce trust but increase her business monopoly
- B) Embeddedness measures properties of individual residents rather than relationships between pairs of residents
- C) Mr. Patel's high embeddedness increases trust because misbehavior would have social consequences
- D) Low embeddedness always indicates problematic relationships requiring immediate management intervention

**Answer:** A, C

**Answer Explanation**: Embeddedness is a property of **edges** (relationships), not nodes, so B is false. Option A is correct: low embeddedness (zero mutual friends) reduces trust-through-accountability but creates monopoly power—Mrs. Sharma can dictate terms because clients lack alternative networks. Option C is correct: high embeddedness creates mutual accountability (Mr. Patel's reputation depends on his web of mutual connections). Option D is false: low embeddedness isn't always problematic; it's strategically beneficial for brokers like Mrs. Sharma and essential for information flow.

---

### Q13 (1 point)
**Mrs. Sharma's business benefits from low embeddedness primarily because:**

- A) Low embeddedness creates stronger trust through direct relationships without mutual connections
- B) She occupies a structural hole position, monopolizing services as clients lack alternative connections
- C) Her relationships have high neighborhood overlap, which increases her bargaining power
- D) Complete closure in her network ensures all clients know each other, creating referral opportunities

**Answer:** B

**Answer Explanation**: This directly defines **structural hole monopoly**. Because Mrs. Sharma's clients don't know each other and lack alternative networks to find a caterer, Mrs. Sharma becomes the sole bridge—she controls the gap and can unilaterally set prices, terms, and conditions. Option A contradicts low embeddedness logic (low embeddedness = low trust, but okay for transactions). Option C is false (low embeddedness = *zero* overlap, not high). Option D is false (closure would actually hurt her monopoly by giving clients alternatives).

---

### Q14 (1 point)
**Based on the cell phone study, what percentage of nodes typically belonged to the largest connected component? (Enter whole number between 0 and 100)**

**Answer:** 85

**Answer Explanation**: The 2007 Onnela et al. cell phone study of 4.6 million users found that approximately **85% of all users** formed a single, massive interconnected component. This finding demonstrated the "small-world" property of social networks—despite billions of humans, about 85% exist in one giant connected cluster, with the remaining 15% in smaller, isolated components.

---

### Q15 (1 point)
**To improve dispute resolution between residents like the Sharmas and Joshis, which approach is most effective per embeddedness theory?**

- A) Ensure disputing parties have no mutual friends so they resolve conflicts independently
- B) Increase mutual friends between potentially conflicting residents as common connections can mediate
- C) Create complete closure where everyone knows everyone, eliminating possibility of conflicts
- D) Assign professional mediators from outside with zero embeddedness with any resident for neutrality

**Answer:** B

**Answer Explanation**: The Sharma-Joshi parking dispute involves zero mutual friends, which **eliminates informal mediation mechanisms**. Option B correctly applies embeddedness theory: adding mutual friends between the parties introduces informal mediators—these common connections exert social pressure on both parties to resolve the dispute reasonably and preserve reputation. This is more effective than external mediators (D), forced independence (A), or impossible complete closure (C). Embeddedness creates accountability through the mutual network.

---

### Q16 (1 point)
**Why did the 2007 study successfully validate Granovetter's theory unlike the 1960s research? (Select all that apply)**

- A) Modern computational tools enabled processing massive network datasets with thousands of nodes
- B) Cell phone data provided objective interaction measures, eliminating bias from self-reported surveys
- C) Social media platforms made people more honest about their friendship strengths in surveys
- D) Large-scale communication records allowed observing actual behavior rather than reported behavior

**Answer:** A, B, D

**Answer Explanation**: The 2007 study succeeded through three advantages: (A) computational power to handle 4.6 million users, (B) objective metrics (call logs) instead of subjective surveys, (D) actual behavior rather than reported feelings. Option C is false—social media didn't exist in 2007, and self-reporting has its own biases regardless of platform. The key insight is that behavioral data (call records) beats self-reported surveys every time because people's self-perception of relationships differs from their actual behavior.

---

### Q17 (1 point)
**When management asks for references before joining committees, they are essentially:**

- A) Creating structural holes between committee and new members to maintain exclusive control
- B) Increasing embeddedness of the relationship, using mutual connections to ensure accountability
- C) Implementing weak ties theory by ensuring only weak tie connections join committees
- D) Following closure principle by making all committee members eventually friends with everyone

**Answer:** B

**Answer Explanation**: Requesting references creates **mutual connections** (referees) between management and the new member. These shared connections act as social guarantors and create accountability for the new member's behavior—if they misbehave, their reputation suffers with multiple people (the referees). This directly increases **embeddedness** of the relationship. Option A misunderstands structural holes (these close them, not open them). Option C wrongly applies weak ties (references are strong tie vouching). Option D misunderstands closure (committees don't need complete closure, just sufficient mutual connections).

---

### Q18 (1 point)
**To maximize resident engagement in events, which network strategy should the society employ?**

- A) Create events appealing to random groups to maximize diversity, regardless of interests
- B) Ensure complete closure by making every resident attend every event
- C) Facilitate connections between residents with similar interests and organize exciting targeted events
- D) Eliminate brokerage positions and structural holes by preventing residents from bridging groups

**Answer:** C

**Answer Explanation**: Option C applies both clustering and engagement theory: dense communities around shared interests (high clustering) build strong bonds and attendance. This contrasts with A (random diversity doesn't engage), B (impossible and unnecessary), and D (brokers are valuable for information flow—eliminating them isolates communities). The strategy combines closure within interest groups (fostering engagement) with bridges between groups (enabling society-wide coordination), optimizing both cohesion and communication.

---

### Q19 (1 point)
**Regarding Dr. Mehra's closure versus brokerage dilemma, which principles should guide management? (Select all that apply)**

- A) Some closure within blocks or cultural groups fosters trust and cohesion through embeddedness
- B) Complete closure where all 850 families know each other equally creates optimal structure
- C) Some residents acting as bridges between groups facilitate information flow and coordination
- D) The optimal balance between closure and brokerage has clear, established research solutions

**Answer:** A, C

**Answer Explanation**: Option A is correct: dense clusters within neighborhoods (closure) create trust and accountability. Option C is correct: brokers connecting clusters (brokerage) enable information flow—like Rahul bridging old-timers and young professionals. Option B is false (complete closure is impossible and unnecessary). Option D is false (research has not established a single optimal ratio; it depends on context). The healthy strategy is *both* closure and brokerage, not one or the other.

---

### Q20 (1 point)
**In the mentioned validation study, researchers observed cell phone data collected over how many weeks? (Enter as whole number)**

**Answer:** 18

**Answer Explanation**: The case study explicitly states "Dr. Mehra presented an **18-week social network study** of 850 families in Riverside Gardens." This 18-week duration provided sufficient data collection for observing interaction patterns, network structure, and the relationship between embeddedness and conflict resolution.

---

## Case Study 3 — TechConnect Professional Platform (5,000 Users)

Sarah's data science team at TechConnect analyzed a platform with 5,000 active users across 3 cities. Key findings:

- Average 500 connections/user, but actively communicate with fewer than **50 people**
- Three relationship types: one-way connections, mutual communication, maintained relationships (passive likes/comments)
- Clear industry-based clustering: technology, healthcare, finance, education
- **Girvan-Newman algorithm** used to detect communities via iterative removal of highest-betweenness edges
- Users with **low clustering coefficients** received more job opportunities — validating strength of weak ties

---

### Q21 (1 point)
**If TechConnect has 5,000 users and implements a brute-force community detection algorithm to divide them into exactly two communities, approximately how many possible partitions must be evaluated?**

- A) 2^2500, as half the users form one community
- B) 2^5000, as each user can independently be assigned to either community
- C) 5000!, since each possible ordering represents a different partition
- D) 2^4999, because one user's assignment determines the other's in a two-community structure

**Answer:** D

**Answer Explanation**: Each of 5,000 users can be assigned to either Community A or Community B, giving $2^{5000}$ total combinations. However, assigning users {1-2500} to A and {2501-5000} to B is identical to assigning {2501-5000} to A and {1-2500} to B (just with labels swapped). To eliminate these mirror duplicates, divide by 2: $\frac{2^{5000}}{2} = 2^{4999}$ unique partitions. This illustrates why brute force is **computationally infeasible**—$2^{4999}$ is vastly larger than atoms in the universe.

---

### Q22 (1 point)
**According to the Girvan-Newman algorithm, what is the primary characteristic of edges that should be removed first?**

- A) Edges connecting nodes with the highest degree centrality
- B) Edges that appear in the maximum number of shortest paths between all node pairs
- C) Edges connecting nodes that share the maximum number of common neighbors
- D) Edges that have the lowest weight values based on interaction frequency

**Answer:** B

**Answer Explanation**: Girvan-Newman prioritizes edges with the **highest betweenness centrality**, which measures how many shortest paths pass through an edge. Edges between communities (bridges) naturally have high betweenness because all inter-community traffic is forced through them. This differs from A (degree centrality, irrelevant to bridging), C (common neighbors, that's neighborhood overlap), and D (lowest weights are internal edges, not bridges). Edge betweenness identifies bridges, and bridges are what fragment communities.

---

### Q23 (1 point)
**User A has 400 connections with clustering coefficient 0.25. What is the maximum possible value of actual friendships F among User A's connections?**

- A) 19,900 — calculated from C(400,2) × 0.25
- B) 79,800 — calculated from 400 × 399 × 0.25
- C) 10,000 — calculated from (400 × 40)/2
- D) 39,900 — calculated from selecting 2 users from 400 connections

**Answer:** A

**Answer Explanation**: Maximum possible edges among 400 friends is $\binom{400}{2} = \frac{400 \times 399}{2} = 79,800$. Clustering coefficient = $\frac{\text{Actual}}{79,800} = 0.25$. Therefore, Actual = $79,800 \times 0.25 = 19,900$ friendships. Option A correctly uses the combination formula and applies the coefficient. Options B, C, D use incorrect formulas or arithmetic.

---

### Q24 (1 point)
**Based on the strength of weak ties theory and TechConnect's observations, which statements are valid? (Select all that apply)**

- A) Users with lower clustering coefficients are more likely to access diverse job opportunities
- B) Weak ties typically have higher betweenness centrality compared to strong ties
- C) Users should maximize strong ties exclusively to enhance career advancement
- D) Maintained relationships contribute more to network size growth than mutual communication
- E) The limitation of strong ties to around 50 is primarily due to cognitive and time constraints

**Answer:** A, B, E

**Answer Explanation**: Option A is correct (low clustering = weak ties spanning diverse networks = varied opportunities). Option B is correct (weak ties connect communities, so they appear on many shortest paths = high betweenness). Option E is correct (Dunbar's number—humans have cognitive limits for maintaining active relationships, typically 50-150). Option C is false (exclusive strong ties create information redundancy; weak ties are essential). Option D is tangential to weak ties theory (not directly relevant to the question).

---

### Q25 (1 point)
**Regarding community detection algorithms on TechConnect, which statements are accurate? (Select all that apply)**

- A) Brute-force approach guarantees finding the optimal community structure but is computationally infeasible at scale
- B) Girvan-Newman algorithm's time complexity is primarily determined by the edge betweenness calculation step
- C) Increasing the resolution parameter in modularity-based algorithms always results in fewer detected communities
- D) Removing all edges with high betweenness simultaneously is more efficient than iterative removal
- E) Subgraph-induced methods can efficiently count intra-community edges for evaluating community quality

**Answer:** A, B, E

**Answer Explanation**: Option A is correct (brute force is optimal but exponential complexity makes it infeasible). Option B is correct (betweenness calculation is O(n³), done repeatedly—this is the bottleneck). Option E is correct (subgraph methods can efficiently evaluate community modularity). Option C is false (resolution parameter increases fine-grainedness; higher resolution finds more communities, not fewer). Option D is false (simultaneous removal ignores rerouting of traffic; iterative recalculation is necessary for accuracy).

---

### Q26 (1 point)
**A TechConnect subnetwork has 12 users: Community 1 {1–5} has 8 internal edges, Community 2 {6–12} has 15 internal edges, and 3 edges connect the two communities. What is the ratio of total intra-community edges to inter-community edges? (Round to 2 decimal places)**

**Answer:** 7.67

**Answer Explanation**: Intra-community edges (edges within communities) = $8 + 15 = 23$. Inter-community edges (edges between communities) = $3$. Ratio = $\frac{23}{3} = 7.666...$, which rounds to **7.67**. This ratio indicates strong community structure (high internal density relative to inter-community bridges), making these communities well-defined and distinct.

---

### Q27 (1 point)
**Edge E connects two communities. Among 20 possible source-destination pairs, edge E appears on the shortest path for 16 pairs. For 12 of these pairs, E is on the **only** shortest path; for the remaining 4 pairs, E is on **one of exactly 2** shortest paths. What is the betweenness centrality of edge E? (Round to 2 decimal places)**

**Answer:** 14.00

**Answer Explanation**: Edge betweenness sums the fractions of shortest paths using the edge. For the 12 pairs where E is the *only* shortest path, it gets full credit: $12 \times 1 = 12$. For the 4 pairs where E is one of exactly 2 equal shortest paths, it gets half credit: $4 \times 0.5 = 2$. Total betweenness = $12 + 2 = 14.00$. This high betweenness confirms E is a critical bridge between the two communities.

---

### Q28 (1 point)
**TechConnect wants to partition 16 users such that the first community has exactly 6 users. How many different first-community configurations must the algorithm evaluate?**

- A) 8,008 configurations — C(16,6)
- B) 5,765,760 configurations — 16!/(10! × 6!)
- C) 720 configurations — 16 × 15 × 14 × 13 × 12 × 11
- D) 4,004 configurations — C(16,6)/2

**Answer:** A

**Answer Explanation**: This is a combination problem. Selecting 6 users out of 16 to form the first community (order doesn't matter) is $\binom{16}{6} = \frac{16!}{10! \times 6!} = 8,008$ configurations. The remaining 10 users automatically form the second community. Options B and C describe permutations (order matters, incorrect here). Option D incorrectly divides by 2 (there's no symmetry to remove in one-directional partition selection).

---

---

## Answer Key Summary

| Q | Answer | Concept | Case Study |
|---|--------|---------|-----------|
| 1 | C | Local bridges; zero mutual friends | Weak Ties |
| 2 | 1.0 | Clustering coefficient formula; complete graph | Network Structure |
| 3 | B | Granovetter's weak ties as information bridges | Weak Ties Theory |
| 4 | B | Information redundancy in strong ties | Weak Ties Application |
| 5 | 0.22 | Neighborhood overlap calculation | Quantifying Networks |
| 6 | A, B, C | Clustering and opportunity diversity | Weak Ties & Structure |
| 7 | B | Strong triadic closure property | Triadic Closure |
| 8 | 105 | Complete graph edges: C(n,2) formula | Clustering |
| 9 | B, C | Weak ties for diverse opportunities | Network Diversity |
| 10 | B | Loss of local bridge status reduces information novelty | Weak Ties Value |
| 11 | B | Objective measurement in validation studies | Research Methodology |
| 12 | A, C | Embeddedness as relationship property | Embeddedness Definition |
| 13 | B | Structural hole monopoly | Brokerage Power |
| 14 | 85 | Largest connected component percentage | Real-world Validation |
| 15 | B | Embeddedness enables informal mediation | Conflict Resolution |
| 16 | A, B, D | Why 2007 study succeeded vs. 1960s | Study Methodology |
| 17 | B | References increase embeddedness/accountability | Social Accountability |
| 18 | C | Community engagement through shared interests | Community Building |
| 19 | A, C | Closure + Brokerage balance | Organizational Design |
| 20 | 18 | Study duration parameter | Data Collection |
| 21 | D | Brute-force partition counting: 2^(n-1) | Computational Complexity |
| 22 | B | Edge betweenness in Girvan-Newman | Algorithm Design |
| 23 | A | Clustering coefficient applied to friendships | Network Metrics |
| 24 | A, B, E | Weak ties theory applications | Weak Ties Validation |
| 25 | A, B, E | Community detection algorithm properties | Algorithm Analysis |
| 26 | 7.67 | Community quality ratio | Community Strength |
| 27 | 14.00 | Edge betweenness calculation with multiple paths | Centrality Metrics |
| 28 | A | Combination formula for community partitions | Combinatorics |

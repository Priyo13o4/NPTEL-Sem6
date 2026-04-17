# Week 7: Assignment 7 — Social Networks (NOC26_CS82)

| Field | Value |
|-------|-------|
| **Due** | 2026-03-11, 23:59 IST |
| **Submitted** | 2026-03-11, 22:03 IST |
| **Total Score** | 15/15 |

---

## Case Study 1 — Engineering a Viral Marketing Campaign That Didn't Fully Take Off

A mid-sized consumer electronics company launched a new wearable device targeting college students and young professionals. The marketing team planned a viral campaign on a popular social media platform where users formed dense communities based on shared interests (fitness, gaming, campus life), with far more interaction within communities than across them.

The company identified influencers with large follower counts, who posted simultaneously on launch day, generating high initial visibility. However, sharing behavior was unexpected—many users viewed the content but only a fraction shared it. In tightly knit communities, the campaign spread rapidly through long chains of reshares. In loosely connected communities, spread stopped after one or two steps.

Investigation revealed users required a **sufficient proportion of their close connections** to have already shared the post before sharing themselves. A small number of **bridging users** (members of multiple communities) played a disproportionate role in cross-community spread.

---

### Q1 (1 point)
**The chain of users sharing the marketing post is best described as:**

- A) Clustering
- B) Diffusion
- C) Modularity
- D) Centrality

**Answer:** B

**Answer Explanation:** In network science, the process by which an innovation, behavior, or piece of information spreads from node to node across edges of a graph over time is formally defined as **diffusion**. The sharing behavior propagates through the network—some users see the post, choose to share it, and their friends observe that sharing and make their own decisions. This sequential propagation of the behavior through the network structure is the definition of diffusion. Option A (clustering) refers to the tendency of nodes to form tightly connected groups, which is a property of the network structure itself, not the process of spread. Options C (modularity) and D (centrality) are structural properties, not dynamic processes.

**Related Topic**: [Simple vs. Complex Contagion](notes.md#simple-vs-complex-contagion)

---

### Q2 (1 point)
**Which factors limited the campaign from becoming platform-wide?** *(Select all that apply)*

- A) High individual sharing thresholds
- B) Weak connections between communities
- C) Lack of influencers
- D) Dependence on a small number of bridging users

**Answer:** A, B, D

**Answer Explanation:** This question directly tests understanding of the **Linear Threshold Model** and cluster dynamics. **(A) High individual sharing thresholds** are critical—users require a sufficient proportion of their close connections to share before they do, implementing the $p \geq q$ decision rule. **(B) Weak connections between communities** act as mathematical firewalls per the Cascade Theorem. Single weak ties between communities provide exposure $p = 1/d$ (where $d$ is total friends), which is far too low to overcome thresholds in complex contagion. The cascade stopped at cluster boundaries because weak bridges couldn't transmit enough social proof. **(D) Dependence on a small number of bridging users** is critical—these users are the only bridges; if they don't share, the cascade is completely blocked from entering adjacent clusters. Even if Community A went viral, without bridging users sharing with Community B, the cascade ends. Option **(C)** is incorrect—the company had influencers; the problem wasn't their existence but that follower counts don't guarantee cascade success (as explained in the notes: an influencer with 1M followers reaching 1% of each follower's network provides insufficient exposure).

**Related Topic**: [The Linear Threshold Model: Payoffs and Thresholds](notes.md#the-linear-threshold-model-payoffs-and-thresholds)

---

### Q3 (1 point)
**Why did the campaign spread faster within certain communities than across communities?**

- A) Communities had fewer users
- B) Members observed more neighbors sharing
- C) Influencers posted more frequently
- D) Algorithms restricted content

**Answer:** B

**Answer Explanation:** Dense communities naturally create higher exposure fractions ($p$). Within a tightly knit community, if even a few members share, every other member observes multiple friends who have shared due to high clustering. A person with 20 friends in a highly clustered group might see 5 of them share the post, giving exposure $p = 5/20 = 0.25$ (25%). If their threshold is $q = 0.2$ (20%), they adopt. In a loosely connected community, the same person might see only 1 friend share ($p = 1/20 = 0.05$ or 5%), which stays below threshold. This high-density effect within clusters versus low exposure across clusters perfectly demonstrates the distinction between within-cluster and between-cluster diffusion dynamics. Option A is irrelevant to diffusion speed. Options C and D are external factors not mentioned in the case study.

**Related Topic**: [Clusters as Mathematical Firewalls](notes.md#clusters-as-mathematical-firewalls)

---

### Q4 (1 point) — NAT
**If a user has 20 close connections and a sharing threshold of 0.25, how many of those connections must share the post before the user does?**

**Answer:** 5

**Answer Explanation:** This is a direct application of the Linear Threshold Model decision rule. The threshold $q = 0.25$ means the user requires a fraction $p \geq 0.25$ of their neighbors to have shared. With 20 close connections: $20 \times 0.25 = 5$ connections must share. Once at least 5 of their 20 friends have shared the post, the condition $p \geq q$ is satisfied, and the user will rationally choose to share as well.

**Related Topic**: [The Linear Threshold Model: Payoffs and Thresholds](notes.md#the-linear-threshold-model-payoffs-and-thresholds)

---

### Q5 (1 point)
**Starting the campaign from users who connect multiple communities is effective primarily because they:**

- A) Have higher follower counts
- B) Reduce individual thresholds
- C) Enable cross-community diffusion
- D) Increase content visibility algorithms

**Answer:** C

**Answer Explanation:** Bridging users create **wide bridges** between clusters. Because they are members of multiple communities, they simultaneously expose multiple community clusters to the behavior. When a bridging user shares, they don't just reach their in-cluster friends (who might already be sharing); they also transmit the behavior to their out-of-cluster friends in the adjacent community. This creates the necessary multiple redundant connections for a cascade to jump between clusters. Without bridging users sharing, the cascade would be stopped by the cluster firewall described in the Cascade Theorem. Option A is irrelevant—the benefit isn't follower count. Option B is incorrect—bridging users don't change anyone's threshold; they increase exposure for out-of-cluster friends. Option D misattributes the mechanism to algorithmic amplification rather than network structure.

**Related Topic**: [The Reversal of Weak Ties Theory](notes.md#the-reversal-of-weak-ties-theory)

---

## Case Study 2 — Adoption of Electric Vehicles Across Interacting Social Communities

A national government launched a policy program to accelerate **electric vehicle (EV)** adoption through financial incentives, expanded charging infrastructure, and media campaigns. Despite this, adoption rates varied widely across regions.

Researchers modeled the population as a social network with strong **community structure**—urban professionals, environmentally conscious groups, corporate employees, and rural households formed tightly connected clusters with few cross-cluster connections. A small group of **early adopters** purchased EVs despite limited infrastructure and high prices, driven by lower risk resistance and environmental motivation.

In some communities, 2–3 early adopters triggered rapid spread. In others, adoption stagnated despite exposure. Large-scale cascade occurred only when clusters with **low adoption thresholds** were connected, forming a **giant connected component**. In fragmented parts of the network, even strong incentives failed.

---

### Q6 (1 point)
**The most appropriate network for modeling EV adoption behavior is a:**

- A) Transportation network
- B) Citation network
- C) Social network
- D) Supply chain network

**Answer:** C

**Answer Explanation:** Although EVs are products (suggesting transportation network), the mechanism of adoption is fundamentally driven by human psychology, social influence, and risk assessment. The case study explicitly emphasizes that different **communities** adopt at different rates based on their threshold parameters and social structure—this is a social network phenomenon. Early adopters reduce perceived risk for their social neighbors through social proof and visible adoption. The diffusion mechanism described is the Linear Threshold Model applied to social networks. A transportation network would model physical movement of vehicles, not adoption decisions. Citation and supply chain networks don't capture the social dynamics of individual adoption choices.

**Related Topic**: [The Linear Threshold Model: Payoffs and Thresholds](notes.md#the-linear-threshold-model-payoffs-and-thresholds)

---

### Q7 (1 point)
**Early adopters play a critical role in diffusion primarily because they:**

- A) Have higher incomes
- B) Reduce uncertainty through visible adoption
- C) Are directly subsidized by the government
- D) Are connected to policymakers

**Answer:** B

**Answer Explanation:** Early adopters act as **seed nodes** initiating cascades. They demonstrate through their visible adoption that the technology is viable, reducing the perceived risk for everyone connected to them. In the Linear Threshold Model, observing early adopters increases $p$ (the fraction of neighbors adopting), which brings people closer to their adoption threshold $q$. EV adoption is a high-risk, high-cost decision; most of the network is waiting for social proof before committing. Early adopters break this impasse by publicly taking on the risk, making adoption safer-seeming for everyone else. This is a core diffusion mechanism—the cascade cannot start without initial seed nodes breaking the status quo. Option A (income) is irrelevant to the mechanism. Option C (subsidies) misses the point—subsidies affect $a$ and $b$ payoffs, but early adopters matter because they provide visibility. Option D (policy connection) is incorrect—the value is social proof, not political access.

**Related Topic**: [Cascades: The Domino Effect](notes.md#cascades-the-domino-effect)

---

### Q8 (1 point)
**Which conditions are necessary for a large EV adoption cascade from a small initial group?** *(Select all that apply)*

- A) Existence of a giant connected component
- B) Uniform adoption thresholds across all individuals
- C) Presence of many individuals with low thresholds
- D) Complete connectivity among all nodes

**Answer:** A, C

**Answer Explanation:** For a cascade to achieve massive scale: **(A) A giant connected component** must exist so that there is an actual pathway of edges through which adoption can propagate. If the network is fragmented into isolated clusters, even a successful cascade in one region cannot reach another. Without connectivity, scale is impossible. **(C) Many individuals with low thresholds** must exist to sustain the cascade momentum. If the cascade encounters a wall of high-threshold individuals (people requiring 70%+ of neighbors to adopt), the cascade dies immediately. The presence of low-threshold individuals keeps adoption spreading through layers of the network. Option **(B)** is incorrect—uniform thresholds are not necessary; in fact, diversity of thresholds (some low, some high) is realistic and doesn't prevent cascades as long as low-threshold individuals exist. Option **(D)** is incorrect—complete connectivity is not necessary; cascades can spread through sparse networks as long as they're connected and contain low-threshold nodes.

**Related Topic**: [Cascades: The Domino Effect](notes.md#cascades-the-domino-effect)

---

### Q9 (1 point) — NAT
**If a consumer has 14 social contacts and an adoption threshold of 0.5, how many contacts must adopt EVs before the consumer considers adoption?**

**Answer:** 7

**Answer Explanation:** Direct application of the Linear Threshold Model. With threshold $q = 0.5$, the consumer requires 50% of their social contacts to adopt: $14 \times 0.5 = 7$ contacts. Once 7 of their 14 social contacts have adopted EVs, the consumer's exposure $p = 7/14 = 0.5$ meets the threshold condition $p \geq q$, triggering their adoption decision.

**Related Topic**: [The Linear Threshold Model: Payoffs and Thresholds](notes.md#the-linear-threshold-model-payoffs-and-thresholds)

---

### Q10 (1 point)
**The primary reason EV adoption differed sharply across communities was that:**

- A) Incentives were unevenly distributed
- B) Media campaigns targeted only cities
- C) Network structure and thresholds varied across communities
- D) Charging stations were unavailable

**Answer:** C

**Answer Explanation:** This is the essence of the **Cascade Theorem** and diffusion in community-structured networks. A government can offer identical incentives nationwide, which affects all payoffs $a$ and $b$ uniformly. But if Community A is an open network with many low-threshold individuals, the cascade spreads like wildfire—the exposure from early adopters quickly pushes people past their adoption thresholds. If Community B is a highly dense, isolated cluster with high-threshold individuals, the mathematical firewall defined by the Cascade Theorem prevents the adoption cascade from penetrating, regardless of incentive magnitude. The case explicitly states: "In some communities, 2–3 early adopters triggered rapid spread. In others, adoption stagnated despite exposure." This demonstrates that incentives alone don't determine outcomes; network structure and thresholds do. Options A and B are about incentive distribution, missing the structural explanation. Option D (charging stations) is an infrastructure factor, not a network structure factor that the case emphasizes.

**Related Topic**: [Clusters as Mathematical Firewalls](notes.md#clusters-as-mathematical-firewalls)

---

## Case Study 3 — Financial Contagion and Cascading Failures in an Interbank Network

Financial regulators analyzed the stability of a national **interbank lending network** during economic uncertainty. Each bank is a node; a directed edge from Bank A to Bank B indicates A extended a loan to B.

The network had a **core–periphery structure**—a few large, highly connected banks served as major liquidity providers, while smaller banks clustered regionally. When a mid-sized regional bank defaulted, immediate lenders experienced losses, some were forced to liquidate assets, depressing market prices and stressing other institutions. In some simulations the failure stayed local; in others, a **cascade of failures** spread across multiple banking communities.

A **threshold-based model** was applied: each bank defaults if losses exceed a fixed fraction of its capital buffer. Counterintuitively, **moderate or low connectivity** networks proved more resilient—shocks were less likely to spread system-wide.

---

### Q11 (1 point)
**In an interbank lending network, a directed edge from Bank A to Bank B represents:**

- A) B lending to A
- B) A lending to B
- C) Shared ownership
- D) Regulatory supervision

**Answer:** B

**Answer Explanation:** In financial networks, directed edges represent the flow of capital and exposure to risk. If Bank A extends a loan to Bank B, then Bank A has **capital at risk**—if Bank B defaults, Bank A suffers a loss. The convention is to direct the edge from the creditor (lender) to the debtor (borrower), so the arrow points **from the entity with funds at stake to the entity that owes repayment**. This directional convention makes contagion visualization clear: defaults propagate backward along edges to creditors. Option A is the opposite. Options C and D describe other banking relationships but not the lending flow.

**Related Topic**: [Financial Contagion and Systemic Risk](notes.md#financial-contagion-and-systemic-risk)

---

### Q12 (1 point)
**Which mechanisms contribute to cascading defaults in financial networks?** *(Select all that apply)*

- A) Direct losses through lending relationships
- B) Indirect losses due to asset price declines
- C) Random failures unrelated to network structure
- D) Deliberate regulatory intervention

**Answer:** A, B

**Answer Explanation:** Financial contagion spreads through two primary channels: **(A) Direct losses** occur through the lending network itself—Bank B defaults on its loan to Bank A, Bank A suffers a loss toward its default threshold, and may itself default if losses are severe enough. This is threshold-based contagion through the network structure. **(B) Indirect losses** occur through asset markets—when Bank B defaults and is forced to liquidate its assets (stocks, bonds, derivatives) to raise emergency cash, it floods the market with supply, crashing prices. Other banks holding the same assets (even unconnected to Bank B in the lending network) see their balance sheets deteriorate instantly. This creates **contagion through asset prices**, not the lending network directly, but it's equally destructive to system stability. Option **(C)** is incorrect—random failures unrelated to network structure don't account for the "cascading" pattern; cascades follow network pathways. Option **(D)** is incorrect—deliberate regulatory intervention is a response to contagion, not a mechanism causing it.

**Related Topic**: [Financial Contagion and Systemic Risk](notes.md#financial-contagion-and-systemic-risk)

---

### Q13 (1 point)
**High connectivity can increase systemic risk because it:**

- A) Slows down information flow
- B) Amplifies shock propagation across institutions
- C) Reduces market liquidity
- D) Isolates failing banks

**Answer:** B

**Answer Explanation:** This is the **connectivity paradox** core to financial contagion theory. High connectivity acts like a superhighway for shocks. When a massive failure occurs, it immediately reaches hundreds of creditor banks simultaneously through the highly connected network. Each bank suffers exposure from the failed bank, and if losses exceed their thresholds, they default, triggering further defaults among *their* creditors. High connectivity ensures that shock propagates instantly across the entire system, turning local failure into systemic crisis. In contrast, low connectivity means a shock in one region doesn't instantly reach the national banking system; contagion takes time and may be localized. Option A is incorrect—high connectivity improves information flow, not reduces it. Option C is incorrect—connectivity relates to risk propagation, not market liquidity. Option D is incorrect—high connectivity does the opposite: it connects failing banks to others, amplifying their impact.

**Related Topic**: [Financial Contagion and Systemic Risk](notes.md#financial-contagion-and-systemic-risk)

---

### Q14 (1 point) — NAT
**If a bank has exposure to 12 counterparties and a default threshold of 0.25, how many counterparties defaulting could trigger its failure?**

**Answer:** 3

**Answer Explanation:** Applying the threshold model to financial risk: the bank's default threshold of 0.25 means it defaults when losses from counterparties reaching 25% of its capital buffer occur. With exposure to 12 counterparties, assuming each default causes equal loss: $12 \times 0.25 = 3$ counterparties. If 3 counterparties default, accumulated losses hit the 25% capital threshold, triggering the bank's own default. This demonstrates the threshold mechanics of financial contagion—banks don't need full network failure to go down; hitting a threshold fraction of counterparties suffices.

**Related Topic**: [Financial Contagion and Systemic Risk](notes.md#financial-contagion-and-systemic-risk)

---

### Q15 (1 point)
**According to threshold-based contagion models, which network property enhances resilience to shocks?**

- A) High connectivity
- B) Low connectivity
- C) Complete connectivity
- D) Identical thresholds for all banks

**Answer:** B

**Answer Explanation:** This is the counterintuitive breakthrough of financial network theory. **Low connectivity** creates structural compartmentalization—clusters of banks that are weakly linked to other clusters. When a massive shock hits, it devastates one cluster completely (losses spread locally), but because weak connections isolate the cluster, the contagion has difficulty jumping to other clusters. The shock is contained geographically. In contrast, high connectivity means shocks bypass local clusters and instantly infect the entire system. For small shocks, high connectivity is beneficial (losses distribute across many partners, no single bank hits default threshold). But for **large, system-wide shocks**, high connectivity is catastrophic—it acts as the delivery mechanism for system-wide failure. Option A is the opposite of the correct answer. Option C (complete connectivity) is extreme high connectivity, making systemic failure inevitable. Option D (identical thresholds) doesn't determine resilience; structural connectivity matters more.

**Related Topic**: [Financial Contagion and Systemic Risk](notes.md#financial-contagion-and-systemic-risk)

---

## Answer Key Summary

| Q | Answer | Concept Tested |
|---|--------|----------------|
| 1 | B | Diffusion: process of behavior spreading through networks |
| 2 | A, B, D | Thresholds, cluster firewalls, bridging users |
| 3 | B | Within-cluster vs. between-cluster exposure dynamics |
| 4 | 5 | Linear Threshold Model calculation: threshold fraction |
| 5 | C | Wide bridges enabling cross-community cascades |
| 6 | C | Network type for adoption modeling |
| 7 | B | Early adopters reducing uncertainty through visible adoption |
| 8 | A, C | Necessary conditions for large-scale cascades |
| 9 | 7 | Linear Threshold Model: adoption requirement calculation |
| 10 | C | Network structure and thresholds determining diffusion outcomes |
| 11 | B | Edge direction in lending networks (creditor → debtor) |
| 12 | A, B | Direct and indirect loss mechanisms in financial contagion |
| 13 | B | High connectivity amplifying shock propagation |
| 14 | 3 | Threshold-based default calculation |
| 15 | B | Low connectivity enhancing resilience to massive shocks |

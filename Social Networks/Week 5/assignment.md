# Week 5: Assignment 5 — Social Networks (NOC26_CS82)

> **Due:** 2026-02-25, 23:59 IST
> **Submitted:** 2026-02-24, 23:19 IST
> **Total Score:** 15/15

---

## Case Study 1 — Urban Segregation via the Schelling Model

Urban planners studied a new residential development with **100 housing units** arranged in a **10×10 grid**. Initially, families of various backgrounds were distributed randomly: **45 Type A**, **45 Type B**, and **10 empty cells**. Each household has up to **8 neighbors** (4 adjacent + 4 diagonal). A household is "unsatisfied" if fewer than **t** neighbors are of the same type.

With **t = 3** (37.5% similarity), the grid reached 100% satisfaction in **8 iterations**. At **t = 4**, segregation became more pronounced. At **t = 6**, the simulation took significantly longer and produced extreme segregation.

---

### Q1 (1 point)
**In the Schelling model, if a household at position (4,5) has threshold t=3 and currently has 2 same-type neighbors among its 8 neighbors, what happens when it is selected for relocation?**

- [ ] A) It stays in place to minimize system-wide disruption to neighboring households
- [ ] B) It moves randomly to an empty cell, then all satisfaction statuses are recalculated
- [ ] C) It moves only if an empty cell with 3+ same-type neighbors is available nearby
- [ ] D) It waits in queue until all currently satisfied households are re-evaluated first

**Answer:** B

**Answer Explanation**: In the Schelling Model, unsatisfied households move **immediately and randomly** to any empty cell—no destination evaluation occurs. Moving from position (4,5) with 2 same-type neighbors triggers satisfaction recalculation for all 8 of its former neighbors (they lost a neighbor) and all neighbors of the destination cell (they gained a neighbor). This ripple effect is fundamental to the model's behavior.

**Related Topic**: [The Simulation Mechanics](notes.md#the-simulation-mechanics)

---

### Q2 (1 point)
**Consider a 10×10 grid where boundary nodes are defined as having either u=0, v=0, u=n-1, or v=n-1. Which statements correctly characterize the neighbor relationships in this grid? *(Select all that apply)***

- [ ] A) Corner node (0,0) has exactly 3 neighbors, making satisfaction impossible when t≥4
- [ ] B) Boundary node (0,5) has exactly 5 neighbors but uses the same threshold as internal nodes
- [ ] C) Internal node (5,5) has 8 neighbors including diagonals after adding diagonal edge connections
- [ ] D) The grid contains 36 boundary nodes total: 4 corners plus 32 edge nodes with <8 neighbors

**Answer:** A, B, C, D

**Answer Explanation**: All statements are geometrically correct. (A) Corner nodes have 3 neighbors (right, down, diagonal), so t≥4 guarantees permanent dissatisfaction. (B) Edge nodes have 5 neighbors; the threshold applies uniformly across all nodes. (C) 8-connectivity includes diagonals. (D) A 10×10 grid has 4 corners + 32 edges = 36 boundary nodes (60 internal).

**Related Topic**: [Boundary vs. Internal Nodes](notes.md#boundary-vs-internal-nodes)

---

### Q3 (1 point)
**With threshold t=3, the simulation reached 100% satisfaction in 8 iterations. If threshold t=2 (25% similarity) was used instead with the same initial distribution, what would most likely occur?**

- [ ] A) Slower convergence and more extreme segregation due to stricter satisfaction requirements
- [ ] B) Faster convergence and less extreme segregation because satisfaction is achieved more easily
- [ ] C) Similar convergence speed but less extreme segregation due to stable integrated configurations
- [ ] D) Faster convergence but more extreme segregation due to positive feedback from initial clusters

**Answer:** B

**Answer Explanation**: Lower threshold (t=2 vs. t=3) means households are satisfied with 25% same-type neighbors instead of 37.5%. Fewer households trigger dissatisfaction → fewer moves → system stabilizes in fewer iterations. With fewer moves, the network converges quickly in integrated (less segregated) configurations.

**Related Topic**: [The Paradox and Its Resolution](notes.md#the-paradox-and-its-resolution)

---

### Q4 (1 point) — NAT
**In an iteration with t=3 and 25 unsatisfied households, one unsatisfied household moves from position P₁ to empty position P₂. What is the MAXIMUM possible number of households whose satisfaction status could change as a direct result of this single move?**

**Answer:** ______

**Answer Explanation**: Position P₁ has **8 neighbors** whose satisfaction statuses must be recalculated (they lost a neighbor). Position P₂ has **8 neighbors** whose statuses must be recalculated (they gained a neighbor). Maximum = 8 + 8 = **16** households affected (not counting the moved household itself or any overlapping neighbors).

**Related Topic**: [The Simulation Mechanics](notes.md#the-simulation-mechanics)

---

### Q5 (1 point) — NAT
**In a 10×10 grid with no empty cells, households are arranged in a checkerboard pattern where each household has exactly 4 same-type neighbors and 4 different-type neighbors. What is the MINIMUM threshold value t that would make this integrated configuration unstable?**

**Answer:** ______

**Answer Explanation**: In a checkerboard, every household has exactly 4 same-type and 4 different-type neighbors (8 total). Thresholds t=1,2,3,4 all allow satisfaction (4 same-type neighbors ≥ t). The moment t=5, every household requires 5+ same-type neighbors but only has 4 → all households become unsatisfied. **Minimum t = 5.**

**Related Topic**: [The Simulation Mechanics](notes.md#the-simulation-mechanics)

---

## Case Study 2 — Structural Balance at InnovateCorp (30 Employees)

InnovateCorp has **30 employees** in a complete social network (**450 total relationships**). HR manager Sarah maps relationships as positive (collaborative) or negative (antagonistic). Key observations:

- **Alice–Bob–Charlie**: Alice+Bob = positive, Alice+Charlie = positive, Bob+Charlie = negative → **unstable triangle**
- **David & Emma** bond after discovering a shared dislike of manager Frank → "enemy's enemy is a friend"
- Team-building "sweat together" exercise designed to convert negative ties to positive
- After 3 months: network evolves toward structural balance, likely forming **two factions**

---

### Q6 (1 point)
**In the initial network with 30 employees, why is the Alice-Bob-Charlie triangle (positive-positive-negative) considered unstable according to structural balance theory?**

- [ ] A) The triangle creates computational inefficiency in network analysis algorithms
- [ ] B) The triangle generates psychological pressure as Alice faces conflicting loyalties, potentially leading to relationship changes
- [ ] C) The triangle violates the mathematical requirement that all triangles must have an even number of negative edges
- [ ] D) The triangle indicates that exactly one person must leave the organization to restore balance

**Answer:** B

**Answer Explanation**: The (+ + −) triad creates **psychological stress**: Alice is friends with both Bob and Charlie, but they hate each other. Alice is caught between conflicting loyalties. This pressure motivates her (or others) to flip an edge—either reconcile Bob and Charlie (flip − to +, creating + + +) or drop one friend (flip a + to −, creating + − −).

**Related Topic**: [Type 2: Two Positive, One Negative (+ + −) — UNSTABLE](notes.md#type-2-two-positive-one-negative-unstable)

---

### Q7 (1 point)
**Based on structural balance theory, which statements are correct regarding InnovateCorp's social network evolution? *(Select all that apply)***

- [ ] A) When David and Emma bond over their mutual dislike of Frank, this demonstrates the principle that an enemy's enemy becomes a friend
- [ ] B) A triangle with three negative edges is stable because it satisfies the condition that enemies of enemies remain enemies
- [ ] C) The "sweat together" team-building exercise could convert negative relationships to positive ones by creating shared experiences
- [ ] D) If the network reaches complete structural balance, it must eventually organize into either one unified group or exactly two opposing factions

**Answer:** A, C, D

**Answer Explanation**: (A) is correct—David−Emma are now friends because they share enmity toward Frank (+ − − is stable). (B) is incorrect—three negatives (− − −) is **unstable** (odd number of negatives); any two would ally. (C) is correct—flipping negatives to positives is a valid evolution mechanism. (D) is correct—**Harary's Balance Theorem** guarantees two possible endpoints: one unified group or two factions.

**Related Topic**: [Harary's Balance Theorem (The Grand Conclusion)](notes.md#harays-balance-theorem-the-grand-conclusion)

---

### Q8 (1 point) — NAT
**In a stable configuration with two factions — Faction A (18 employees) and Faction B (12 employees) — all within-faction relationships are positive and all cross-faction relationships are negative. How many negative edges exist in the entire network?**

**Answer:** ______

**Answer Explanation**: In a balanced two-faction network, all negative edges are between factions. Number of cross-faction edges = $18 \times 12 = \textbf{216}$. All within-faction edges are positive.

**Related Topic**: [Structure 2: Two Opposing Factions](notes.md#structure-2-two-opposing-factions)

---

### Q9 (1 point)
**Introducing just one negative relationship into an otherwise completely positive 30-person network triggers a cascade. What best describes the eventual outcome according to structural balance theory?**

- [ ] A) The single negative edge will necessarily propagate until all 450 relationships become negative
- [ ] B) The cascade will continue indefinitely without reaching stability
- [ ] C) The network will reach a stable state that could be either one positive group or two opposing factions
- [ ] D) Exactly half of all relationships will become negative while the other half remain positive

**Answer:** C

**Answer Explanation**: Adding one negative edge breaks the "utopia" (all positive). This creates unstable triads that must be resolved. As the network evolves through edge flips, it must reach a stable configuration. By **Harary's Balance Theorem**, the only stable states are: (1) all positive (utopia, unlikely after cascade), or (2) two factions. The outcome depends on which edges get flipped, but the endpoint is guaranteed to be stable.

**Related Topic**: [Harary's Balance Theorem (The Grand Conclusion)](notes.md#harays-balance-theorem-the-grand-conclusion)

---

### Q10 (1 point)
**Which of the following scenarios represent **structurally balanced** triangular configurations? *(Select all that apply)***

- [ ] A) Three employees from the design team who all have mutually positive relationships and regularly collaborate
- [ ] B) Two developers who are close friends, both of whom have negative relationships with a product manager
- [ ] C) Employee X is friends with Employee Y, Employee Y dislikes Employee Z, and Employee X also dislikes Employee Z
- [ ] D) Three employees who all mutually dislike each other and avoid interaction (three negative edges)

**Answer:** A, B, C

**Answer Explanation**: (A) is (+ + +)—3 positives (even number of negatives) → **stable**. (B) is (+ − −)—1 positive, 2 negatives (even number of negatives) → **stable**. (C) is also (+ − −)—X and Y are friends (+), Y−Z are enemies (−), X−Z are enemies (−) → **stable**. (D) is (− − −)—3 negatives (odd number) → **unstable**.

**Related Topic**: [Summary: The Stability Rule](notes.md#summary-the-stability-rule)

---

## Case Study 3 — Diplomatic Relations Among 10 Countries

A research team models diplomatic relationships of **10 countries** as a **complete signed network** (positive = alliance, negative = hostility). They simulate evolution toward structural balance by iteratively stabilizing unstable triangles:

- **Unstable (0 positive edges)**: randomly convert one negative edge to positive
- **Unstable (2 positive edges)**: randomly invert one edge sign
- Once stable: all countries partition into **exactly two coalitions** via BFS

The BFS variant: start with Country 1 in Coalition 1 → neighbors with positive ties → Coalition 1; neighbors with negative ties → Coalition 2.

---

### Q11 (1 point)
**In a network of 8 countries with 28 triangles, after stabilizing one unstable triangle the count of unstable triangles increases from 26 to 28. What best explains this phenomenon?**

- [ ] A) Stabilizing one triangle always increases unstable counts because changed edges affect all adjacent triangles uniformly
- [ ] B) The increase is random coincidence from edge inversions that will decrease as the algorithm converges globally
- [ ] C) Changing one edge sign can destabilize adjacent triangles temporarily before the network eventually reaches stability
- [ ] D) The algorithm has a counting bug in early iterations that self-corrects through redundant stabilization operations

**Answer:** C

**Answer Explanation**: Each edge belongs to N-2 = 6 adjacent triangles (in an 8-node complete graph). Flipping one edge to stabilize a triad can accidentally destabilize 0-6 other triads that share that edge. The count of unstable triads can spike temporarily before the system converges. Eventually, the system reaches stability, but the path is non-monotonic.

**Related Topic**: [The Evolution Process](notes.md#the-evolution-process)

---

### Q12 (1 point)
**Which statements about stable and unstable triangular configurations in signed networks are correct? *(Select all that apply)***

- [ ] A) A triangle with two positive edges and one negative edge is unstable because two enemies share a common friend
- [ ] B) Triangles with three positive edges are stable as they satisfy the principle "the friend of my friend is my friend"
- [ ] C) A stable triangle with one positive edge represents two friends sharing a common enemy as the third node
- [ ] D) An unstable triangle with zero positives can stabilize by converting any one of its three negative edges to positive

**Answer:** A, B, C, D

**Answer Explanation**: (A) (+ + −) is unstable—correct. (B) (+ + +) is stable—correct. (C) (+ − −) is stable with 1 positive—correct. (D) (− − −) with zero positives is unstable; flipping any one edge to + gives (+ − −), which is stable—correct. All statements are accurate.

**Related Topic**: [Summary: The Stability Rule](notes.md#summary-the-stability-rule)

---

### Q13 (1 point)
**In a complete signed network stable state partitioned into Coalition 1 (n₁ countries) and Coalition 2 (n₂ countries), where n₁ + n₂ = N, how many negative edges exist?**

- [ ] A) N(N-1)/2 − n₁(n₁-1)/2 − n₂(n₂-1)/2, representing total edges minus all positive edges within each coalition
- [ ] B) Exactly n₁ × n₂, representing all edges between coalitions since inter-coalition edges are all negative
- [ ] C) (n₁ + n₂) − 1, representing minimum spanning tree of negative relationships
- [ ] D) Cannot be determined as it depends on the random stabilization sequence and initial configuration

**Answer:** B

**Answer Explanation**: In a balanced two-faction network, all negative edges are **between coalitions**. All **within-coalition edges are positive**. The number of edges between two groups of size n₁ and n₂ is simply the Cartesian product: $n_1 \times n_2$. This is deterministic (not random).

**Related Topic**: [Structure 2: Two Opposing Factions](notes.md#structure-2-two-opposing-factions)

---

### Q14 (1 point) — NAT
**A signed network contains 6 countries in a complete graph. In the stable state: Coalition 1 has 4 countries, Coalition 2 has 2 countries. What is the total number of **positive** edges in this stable network?**

**Answer:** ______

**Answer Explanation**: Positive edges exist only **within coalitions**. Coalition 1 (4 nodes) has $\binom{4}{2} = 6$ positive edges. Coalition 2 (2 nodes) has $\binom{2}{2} = 1$ positive edge. Total = 6 + 1 = **7** positive edges.

**Related Topic**: [Structure 2: Two Opposing Factions](notes.md#structure-2-two-opposing-factions)

---

### Q15 (1 point)
**During the evolution of a signed network from unstable to stable state, which observations about the algorithmic process are necessarily true? *(Select all that apply)***

- [ ] A) Each iteration changes exactly one edge sign, affecting at most (N-2) triangles in a network of N nodes
- [ ] B) The algorithm terminates in finite time as unstable triangles monotonically decrease at each iteration step
- [ ] C) The final stable configuration is deterministic and independent of the random sequence of triangle selections
- [ ] D) Stabilizing a triangle with two positives produces either three positives or one positive, never zero positives

**Answer:** A, D

**Answer Explanation**: (A) is correct—one edge flip affects exactly the N-2 triangles containing that edge. (D) is correct—flipping one edge of a (+ + −) triangle creates either (+ + +) or (+ − −), never (− − −). (B) is incorrect—unstable counts can spike temporarily (non-monotonic). (C) is incorrect—the final faction partition depends heavily on *which* edges get flipped first (random choice) and can differ across runs.

**Related Topic**: [The Evolution Process](notes.md#the-evolution-process)

---

# ✅ Answer Key Summary

| Q | Answer | Concept |
|---|--------|---------|
| 1 | B | Random movement & satisfaction recalculation |
| 2 | A, B, C, D | Neighbor counts at boundary and internal nodes |
| 3 | B | Lower threshold → faster convergence, less segregation |
| 4 | 16 | Maximum neighbors affected by single move |
| 5 | 5 | Minimum threshold for checkerboard instability |
| 6 | B | Psychological pressure in (+ + −) triads |
| 7 | A, C, D | Balance evolution & Harary's theorem |
| 8 | 216 | Cross-faction negative edges (18 × 12) |
| 9 | C | Cascade reaches one group or two factions |
| 10 | A, B, C | Balanced triad configurations |
| 11 | C | Temporary destabilization of adjacent triads |
| 12 | A, B, C, D | All statements about triad stability correct |
| 13 | B | Negative edges = n₁ × n₂ |
| 14 | 7 | Positive edges in two-faction network |
| 15 | A, D | Monotonic decrease & determinism NOT guaranteed |

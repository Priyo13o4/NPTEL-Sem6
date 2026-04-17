# Week 5: Theoretical Notes — Schelling Segregation Model & Structural Balance Theory

This week explores two powerful phenomena that explain segregation and conflict formation in human systems. The **Schelling Model** demonstrates how individual preference for diversity can paradoxically produce extreme segregation at a societal level. **Structural Balance Theory** reveals the psychological dynamics driving relationship formation in signed networks (networks with positive and negative edges). Together, they show how simple local rules generate complex global patterns.

---

## 1. The Schelling Segregation Model: From Micro-Motives to Macro-Behavior

### Historical Context and Motivation

In 1971, economist **Thomas Schelling** (later a Nobel laureate) posed a seemingly paradoxical question: Why are American cities so racially and economically segregated, even when most individuals claim they're comfortable in diverse neighborhoods?

He created a grid-based simulation (originally played with pennies and dimes on a chessboard) to demonstrate that **micro-motives (individual preferences) do not necessarily produce macro-behavior (aggregate outcomes) that match those preferences**. This principle is central to understanding emergent behavior in complex systems.

### Model Components

#### **The Grid: Spatial Structure**

The city is represented as a **2D grid** (e.g., 10×10 = 100 houses). Each cell represents a household. The key innovation is that the *topology matters*:

**Neighborhood Definition (8-connectivity):**
Each house has up to 8 neighbors:
- **4 adjacent neighbors** (up, down, left, right)
- **4 diagonal neighbors** (corners)

**Boundary Effects:**
Not all cells have 8 neighbors:
- **Corner cells** (e.g., position (0,0)): Only 3 neighbors (right, down, diagonal)
- **Edge cells** (e.g., position (0,5)): Only 5 neighbors
- **Internal cells** (e.g., position (5,5)): Full 8 neighbors

**Implication:** Corner houses are naturally isolated and face harder satisfaction criteria.

#### **The Threshold: Individual Preference**

Each household has a **satisfaction threshold** $t$—the minimum number of same-type neighbors required to be satisfied.

**Example:** If $t = 3$ and a household has 8 neighbors, it wants at least 3 to be the same type. This is only 37.5% similarity—a highly integrated preference!

**Interpretation:**
- Lower $t$ (e.g., $t=2$): Highly tolerant; satisfied with diverse neighborhoods
- Higher $t$ (e.g., $t=6$): Intolerant; demands mostly same-type neighbors

### The Simulation Mechanics

**Algorithm:**
1. **Initialization**: Randomly distribute households (e.g., 45 Type A, 45 Type B, 10 empty)
2. **Satisfaction Check**: For each occupied cell, count same-type neighbors
3. **Identification**: Flag all cells where same-type neighbors < $t$ as "unsatisfied"
4. **Movement**: Each unsatisfied household **randomly moves to an empty cell**
5. **Recalculation**: Recompute satisfaction for all neighbors of the moved household (and its new neighbors)
6. **Iteration**: Repeat until no unsatisfied households remain (convergence)

**Key Behavior:** When a household moves:
- Its **old neighbors lose a neighbor** → some may drop below threshold
- Its **new neighbors gain a neighbor** → some may reach satisfaction
- This creates a **ripple effect** where one move can trigger cascading changes

### The Paradox and Its Resolution

**Empirical Finding:** With $t=3$ (37.5% tolerance), a 10×10 grid reaches 100% satisfaction in ~8 iterations, but produces **extreme spatial segregation**—Type A clusters in one corner, Type B in another.

**The Explanation:** 
- At $t=3$, being isolated is terrifying. A household in an empty area (surrounded by different types) is unsatisfied.
- Moving toward any cluster improves odds of satisfying the threshold.
- Clusters attract—once they form, they're stable because members exceed threshold.
- The system cascades from integrated → segregated through **local optimization**.

**Threshold Effects:**
- $t=2$ (25% threshold): Easier satisfaction → fewer moves → faster convergence → less segregation
- $t=4$ (50% threshold): Stricter → more moves → more extreme segregation
- $t=6$ (75% threshold): Very strict → extreme segregation takes much longer to achieve

### Boundary vs. Internal Nodes

**Boundary nodes** (corners and edges) are naturally disadvantaged:
- Corner node (3 neighbors): Can never be satisfied if $t > 3$
- Edge node (5 neighbors): Requires all 5 neighbors to be same-type if $t=5$
- Internal node (8 neighbors): More flexibility for achieving satisfaction

**Implication:** Segregation patterns concentrate minorities in boundary regions or interior clusters.

### Micro-Motives ≠ Macro-Behavior

**Schelling's Insight:** Even with individual tolerance ($t=3$), the aggregate behavior is intolerance (extreme segregation). This demonstrates that:
1. Individual preferences don't directly translate to societal outcomes
2. Small preference biases compound into large-scale patterns
3. Emergent properties arise from local interactions

---

## 2. Structural Balance Theory: Psychology of Signed Networks

### Definition of Signed Networks

In standard graphs, edges simply represent connections. In **signed networks**, edges carry a **sign**:
- **Positive edge (+)**: Friendship, alliance, collaboration
- **Negative edge (−)**: Enmity, antagonism, conflict

A **signed network** models relationships where the quality (friendly vs. hostile) matters as much as the existence of a relationship.

### The Fundamental Unit: The Triad

A **triad** is a triangle of three nodes (A, B, C) with three signed edges (AB, BC, AC). There are exactly **8 possible signed triangles** (2³ combinations of signs), but they collapse into **4 distinct types** due to symmetry:

#### **Type 1: Three Positive Edges (+ + +) — STABLE**

**Configuration:** All three people are mutual friends.

**Psychological Rule:** *"The friend of my friend is my friend"*

**Stability:** This is completely harmonious. No conflicting loyalties. Everyone is happy.

**Real-world example:** Three colleagues who collaborate regularly and support each other.

---

#### **Type 2: Two Positive, One Negative (+ + −) — UNSTABLE**

**Configuration:** Two edges are positive (friendships), one is negative (conflict). Example: You're friends with Bob and Charlie, but Bob and Charlie hate each other.

**Psychological Rule:** *"My friends are in conflict"* = pressure to reconcile or pick sides.

**Instability Source:** This creates **cognitive dissonance**. You face pressure to:
- Option A: Convince your friends to reconcile (flip the negative edge to positive → Type 1)
- Option B: Pick a side and drop one friend (flip one positive edge to negative → Type 3)

**Real-world example:** Alice is friends with both the tech lead (Bob) and the designer (Charlie), but they clash on architecture decisions. Alice feels caught in the middle.

---

#### **Type 3: One Positive, Two Negative (+ − −) — STABLE**

**Configuration:** One positive edge, two negative edges. Example: You and Bob are friends, and you both dislike Charlie.

**Psychological Rule:** *"The enemy of my enemy is my friend"*

**Stability:** This is harmonious. Your friend's enemy is your enemy. No contradiction. You naturally ally.

**Real-world example:** Two students bond over their shared dislike of a difficult professor. Their friendship strengthens through this common opposition.

---

#### **Type 4: Three Negative Edges (− − −) — UNSTABLE**

**Configuration:** A "Mexican standoff"—all three people mutually dislike each other.

**Psychological Rule:** *"Total chaos"*—every pair has an incentive to ally against the third.

**Instability Source:** Any two of the three can form a coalition (flip one edge to positive) to isolate the third. Once they do, the triangle becomes Type 3 (stable).

**Real-world example:** Three rival factions in a company that all compete; the smallest will likely form an alliance with one of the larger ones.

### Summary: The Stability Rule

A triad is **STABLE** if and only if the **product of the three edge signs is positive** (i.e., an even number of negative edges):

$$\text{Stability} = \begin{cases}
\text{STABLE} & \text{if } (\text{\#negative edges}) \text{ is even} \\
\text{UNSTABLE} & \text{if } (\text{\#negative edges}) \text{ is odd}
\end{cases}$$

Or equivalently: **A triangle is stable if it contains 0, 2, or 3 positive edges** (i.e., an odd number of positive edges is unstable).

---

## 3. Network Evolution and Harary's Balance Theorem

### The Evolution Process

If a signed network contains unstable triads, the network is in a **tension state**. Relationships change over time as individuals act to resolve psychological stress:

**At each step:**
1. Identify an unstable triad
2. Flip one edge sign (change a friendship to enmity, or vice versa) to stabilize it
3. Recalculate all adjacent triads

**Complication:** Flipping an edge to fix one triad can destabilize other triads that share that edge. In a complete graph with N nodes, each edge belongs to N-2 triads. So, stabilizing one triad might temporarily increase the total number of unstable triads before the system eventually converges.

### Harary's Balance Theorem (The Grand Conclusion)

**Theorem:** If a complete signed network evolves until every triad is stable, the entire network must have one of exactly **two possible structures**:

#### **Structure 1: One Positive Group (Utopia)**
Every edge in the network is positive. All N nodes form one cohesive, friendly group.

$$\text{All edges are positive} \Rightarrow \text{All triads are (+, +, +)} \Rightarrow \text{Perfect stability}$$

#### **Structure 2: Two Opposing Factions**
The N nodes partition into exactly two groups (Faction 1 and Faction 2):
- **Within Faction 1**: All edges are positive
- **Within Faction 2**: All edges are positive
- **Between Faction 1 and 2**: All edges are negative

$$\text{Partition} \Rightarrow \text{Any triad has} \begin{cases} \text{3 positives} & \text{(same faction)} \\ \text{1 positive, 2 negatives} & \text{(mixed faction)} \end{cases} \Rightarrow \text{All stable}$$

**Proof Sketch:**
- Suppose a stable network has nodes A, B, C such that A and B are in the same faction (positive edge AB).
- If C is in the same faction as A, then AC and BC are both positive. Triad (ABC) has 3 positives → stable ✓
- If C is in a different faction, then AC and BC are both negative. Triad (ABC) has 1 positive, 2 negatives → stable ✓
- This structure exhausts all possibilities for stable triads.

### Finding the Two Factions: Breadth-First Search (BFS)

Once a network reaches stability with two factions, you can find them using **BFS on the signed graph**:

**Algorithm:**
1. Start with any node (e.g., Node 1) and assign it to **Faction A**
2. For each positive-edge neighbor of Node 1: Assign to **Faction A**
3. For each negative-edge neighbor of Node 1: Assign to **Faction B**
4. Repeat: For each node in Faction A, assign its positive neighbors to A, negative neighbors to B
5. The BFS naturally partitions the network into exactly two groups (if the network is connected and balanced)

**Correctness:** If the network is balanced, all edges within each faction are positive, and all edges between factions are negative. BFS respects this structure and converges to the unique two-faction partition.

---

## 4. Practical Evolution: Temporal Dynamics

### Movement of the Needle

In a network with N nodes and M unstable triads, each edge flip changes the signs of the triads that contain that edge. This can:
- **Decrease** the number of unstable triads (good progress)
- **Increase** the number of unstable triads (temporary chaos)
- **Leave it unchanged** (rare, unlikely)

**Key Observation:** The system is not guaranteed to have a monotonically decreasing count of unstable triads. However, over time, the system must converge (finite state space, and the network "prefers" stable configurations).

### Why Not Monotonic Decrease?

When you flip edge AB to stabilize triad (ABC), you affect:
- All triads of form (ABX) for other nodes X
- Some of these may become unstable

**Example:** If (ABC) has edges (AB = +, BC = −, AC = −), flipping AB to − creates:
- (ABC) becomes unstable! (Now −, −, −)
- But triads (ABX) for neighbors X may become stable

This is why algorithms must be designed carefully—random edge flips can oscillate. In practice, the system eventually settles, but the path is non-monotonic.

---

## 5. Quantifying Segregation and Balance

### Segregation Index in Schelling Model

**Residential Segregation** can be quantified as the **proportion of same-type neighbors for average household**:

$$S = \frac{1}{N} \sum_{i=1}^{N} \frac{\text{same-type neighbors of } i}{\text{total neighbors of } i}$$

- $S = 1.0$: Perfect segregation (all neighbors are same type)
- $S = 0.5$: Random mixing (50% same type, 50% other)
- $S = 0.45$ (for 45-55 split): Natural diversity baseline

**Finding:** Even with $t=3$ (only 37.5% threshold), typical simulations reach $S > 0.8$ (80% same-type neighbors)—extreme segregation from minimal preference.

### Counting Negative Edges in Balanced Networks

**Two-Faction Network:**
- Faction 1: $n_1$ nodes → $\binom{n_1}{2}$ positive edges within
- Faction 2: $n_2$ nodes → $\binom{n_2}{2}$ positive edges within
- Cross-faction edges: $n_1 \times n_2$ negative edges

**Total edges:** $\frac{N(N-1)}{2}$ (complete graph)

**Negative edges:** $n_1 \times n_2$

**Positive edges:** $\binom{n_1}{2} + \binom{n_2}{2} = \frac{N(N-1)}{2} - n_1 \times n_2$

---

## 6. Real-World Applications

### Urban Segregation
The Schelling Model predicts residential patterns in cities, showing why diverse intent produces segregated reality.

### International Relations
Signed networks model alliances and hostilities. Stable configurations tend toward two opposing power blocs (Cold War dynamics) or universal peace (rare).

### Corporate Teams
InnovateCorp's case: 30 employees with positive (collaborative) and negative (antagonistic) relationships. Team-building interventions aim to flip negative edges to positive.

### Social Media
Online networks with "follow" (positive) and "block" (negative) relationships naturally segregate into opposing factions (echo chambers, polarization).

---

## Summary

**Key Learning Objectives:**

1. **Schelling Model**: Individual tolerance for diversity, when aggregated through local movement decisions, produces extreme segregation.

2. **Threshold Effects**: Lower thresholds (more tolerance) → faster convergence, less segregation. Higher thresholds → longer evolution, more segregation.

3. **Boundary Effects**: Corner and edge nodes are naturally disadvantaged in satisfaction.

4. **Signed Networks**: Relationships have signs (positive/negative), not just existence.

5. **Structural Balance**: Triads with even numbers of negative edges are stable; odd numbers are unstable.

6. **Balance Theorem**: Every stable signed network must partition into either one unified group or exactly two opposing factions.

7. **Evolution Dynamics**: Networks evolve toward stability by flipping edge signs, but this process is not monotonic.

---

## Key Takeaways

| Concept | Definition | Assignment Connection |
|---------|-----------|----------------------|
| **Schelling Model** | Grid-based segregation simulation with threshold satisfaction | Q1-Q5 (segregation mechanics) |
| **Satisfaction Threshold** | Minimum proportion of same-type neighbors required | Q3-Q5 (threshold effects) |
| **Boundary Nodes** | Cells with <8 neighbors (corners, edges) | Q2 (neighbor count) |
| **Signed Network** | Network with positive (+) and negative (−) edges | Q6-Q15 (balance theory) |
| **Triad** | Triangle of three nodes with three signed edges | Q6-Q12 (balance configurations) |
| **Stable Triad** | Even number of negative edges (0, 2, or 3 edges negative) | Q6, Q10, Q12 (stability rules) |
| **Unstable Triad** | Odd number of negative edges (1 or 3 negatives) | Q6, Q9-Q10, Q12 (instability) |
| **Balance Theorem** | Stable networks partition into 1 group or 2 factions | Q7, Q9, Q13-Q15 (faction structure) |
| **Faction Partition** | Division into two groups with all positive within, negative between | Q8, Q13-Q14 (counting edges) |
| **Network Evolution** | Iterative edge flipping to stabilize unstable triads | Q11, Q15 (evolution dynamics) |

---

## Related Lecture Topics

- **Lectures 54–56**: Schelling Model introduction, simulation, conclusion
- **Lectures 57–61**: Schelling Model implementation (code walkthrough)
- **Lectures 62–67**: Structural balance, positive/negative relationships, balance theorem, proof
- **Lectures 68–74**: Balance network implementation, coalition formation, visualization

# Week 1: Assignment 1 — Social Networks (NOC26_CS82)

> **Due Date:** 2026-02-04, 23:59 IST
> **Submitted:** 2026-02-01, 20:49 IST
> **Total Score:** 15/15

---

## Case Study 01 — The Notice That Traveled Half the Campus

A large public university spread across **120 acres** housed more than **4,200 students** in **six residential hostels**. Each hostel functioned almost like a small community, with students mostly interacting within their own blocks, classrooms, and clubs. To modernize communication and reduce dependency on notice boards, the university introduced a **digital notice system** that allowed students to forward important messages to others they were already connected with.

In this system, a student could directly message only those whose contact details they had saved — usually roommates, classmates, project partners, or club members. No student could send messages to themselves, and repeated connections were ignored by the system.

To test the system, the hostel administration conducted a **water-supply maintenance drill**. At **6:30 PM**, a notice was sent from the hostel office to **10 student coordinators**, chosen from different hostels. Within **5 minutes**, students in two hostels had widely received the notice. However, by **8:00 PM**, complaints began to arrive — nearly **700 students** reported never receiving the notice at all.

The IT team discovered that entire groups of students were **cut off** from the original message, with no possible chains of communication linking certain hostels to the coordinators. The team concluded that **simply increasing the number of users does not guarantee complete information flow** — **the way users are connected matters just as much**.

---

### Q1 (1 point)
**The communication system described is most appropriately represented as:**

- [ ] A) A table of contacts
- [ ] B) A graph of users and connections
- [ ] C) A loop-based structure
- [ ] D) A sorted list

**Answer:** B

**Answer Explanation**: The system has users (nodes) and connections between them (edges), which is the definition of a graph. The notice spreads through these connections, making it a network problem.

**Related Topic**: [Nodes and Edges](notes.md#nodes-and-edges)

---

### Q2 (1 point)
**The main reason some students never received the notice was that:**

- [ ] A) The message was sent too late
- [ ] B) The coordinators did not forward it
- [ ] C) There were no communication paths to them
- [ ] D) The system deleted messages

**Answer:** C

**Answer Explanation**: The IT team discovered that entire groups were **disconnected** — no chain of contacts linked them to the coordinators. This is the definition of a **disconnected graph** where certain nodes cannot be reached from the source.

**Related Topic**: [Network Connectivity and Paths](notes.md#network-connectivity-and-paths)

---

### Q3 (1 point)
**Ignoring self-messaging and duplicate connections makes the structure:**

- [ ] A) Random
- [ ] B) Directed
- [ ] C) Simple
- [ ] D) Cyclic

**Answer:** C

**Answer Explanation**: A **Simple Graph** has exactly two rules:
1. No self-loops (a node cannot connect to itself)
2. No multiple edges (only one edge between any two nodes)

The system satisfies both rules, making it a simple graph.

**Related Topic**: [Simple Graphs vs. Multi-graphs](notes.md#simple-graphs-vs-multi-graphs)

---

### Q4 (1 point)
**Tracing the spread of the message step by step from the hostel office is best described as:**

- [ ] A) Sorting users
- [ ] B) Searching through connections
- [ ] C) Counting messages
- [ ] D) Reversing data

**Answer:** B

**Answer Explanation**: The notice spreads by following the chain of connections from user to user, which is a **graph traversal** or **search** operation. Algorithms like **Breadth-First Search (BFS)** model this process.

**Related Topic**: [Graph Traversal (Information Flow)](notes.md#graph-traversal-information-flow)

---

### Q5 (1 point)
**If every student could directly message every other student, the notice would:**

- [ ] A) Still spread slowly
- [ ] B) Reach only coordinators
- [ ] C) Reach everyone immediately
- [ ] D) Fail due to overload

**Answer:** C

**Answer Explanation**: This describes a **complete graph** where every node connects directly to every other node. In a complete graph, any message reaches all nodes in one hop, ensuring instant global communication.

**Related Topic**: [Network Density (Complete Graphs)](notes.md#network-density-complete-graphs)

---

---

## Case Study 02 — Making Sense of Fest Data Using Python

During the university's **three-day cultural festival**, student coordinators monitored activity on the campus communication app. Hourly message counts were recorded sequentially. Two additional busy hours were later added to the dataset. To analyze engagement from the most recent hour backward, the coordinator **reversed the list**. She also ran a Python program that **generated three random numbers between 0 and 1** and added them together, observing that results varied but never exceeded a certain limit.

---

### Q6 (1 point)

**Which Python structure is most suitable for storing hourly data in sequence?**

- [ ] A) Set
- [ ] B) Dictionary
- [ ] C) List
- [ ] D) Tuple

**Answer:** C

**Answer Explanation:** A **List** is the most appropriate data structure for storing sequential hourly data because it maintains order, allows easy modification (adding new hours), and supports efficient traversal. A Set (A) is unordered. A Dictionary (B) is key-value paired, unnecessarily complex for simple sequences. A Tuple (D) is immutable, preventing the addition of new data points. Lists provide the flexibility needed for dynamic data collection in time-series applications.

**Related Topic:** [Nodes and Edges](notes.md#nodes-and-edges)

---

### Q7 (1 point)

**Reversing the list helped the coordinator mainly to:**

- [ ] A) Change the values
- [ ] B) Analyze recent activity first
- [ ] C) Increase the data size
- [ ] D) Remove older data

**Answer:** B

**Answer Explanation:** Reversing the list reorders the data so that the most recent hour appears first, enabling analysis of engagement patterns from the present backward in time. This is useful for identifying recent trends. Reversing does not change values (A), increase size (C), or remove data (D)—it merely reorganizes the existing data structure for analytical convenience.

**Related Topic:** [Nodes and Edges](notes.md#nodes-and-edges)

---

### Q8 (1 point)

**If three numbers between 0 and 1 are added, which outcome is impossible?**

- [ ] A) 0.9
- [ ] B) 1.6
- [ ] C) 2.7
- [ ] D) 3.8

**Answer:** D

**Answer Explanation:** The maximum sum of three numbers each between 0 and 1 is 1 + 1 + 1 = 3.0. Therefore, any sum exceeding 3.0 (like 3.8) is mathematically impossible. Options A (0.9), B (1.6), and C (2.7) are all feasible outcomes within the valid range [0, 3]. This exemplifies understanding numerical bounds and constraint validation in data analysis.

**Related Topic:** [Nodes and Edges](notes.md#nodes-and-edges)

---

### Q9 (1 point)

**The random number experiment showed that randomness:**

- [ ] A) Has no limits
- [ ] B) Always gives large values
- [ ] C) Produces varying results within bounds
- [ ] D) Removes all patterns

**Answer:** C

**Answer Explanation:** Random processes generate diverse outcomes (varying results) but within defined constraints—in this case, [0, 3]. This illustrates that randomness is not chaos; it follows probabilistic rules. Option A is false (randomness is bounded); option B contradicts observation (results vary, not always large); option D is partially misleading (patterns can emerge in aggregate). The key insight is that randomness has **mathematical structure and limits**.

**Related Topic:** [Nodes and Edges](notes.md#nodes-and-edges)

---

### Q10 (1 point)

**Creating more connections between users mainly helps to:**

- [ ] A) Reduce data storage
- [ ] B) Improve message reach
- [ ] C) Decrease user count
- [ ] D) Control randomness

**Answer:** B

**Answer Explanation:** Additional connections (edges) between users (nodes) expand the communication pathways available for messages to travel. This directly increases the likelihood that information can reach distant users through intermediate nodes. More connections enhance **reachability** and **connectivity** of the network. Options A, C, and D are either irrelevant or false—connections don't reduce storage, decrease users, or directly control randomness.

**Related Topic:** [Network Connectivity and Paths](notes.md#network-connectivity-and-paths)

---

## Case Study 03 — A Simple Safety Network That Worked Too Well

A corporate campus employing **500 security guards** built an internal safety communication system. Initially, the system had **499 communication links**, and every guard could be reached from any other guard. Communication worked **both ways**. Engineers added **random extra connections** to test improvements. The results were worse—guards received the same alert multiple times through different routes. The original setup had exactly the minimum connections needed. The experiment showed that **simplicity can sometimes be more efficient than excessive connectivity**.

---

### Q11 (1 point)

**A system with 500 guards and 499 links where everyone is reachable suggests that:**

- [ ] A) The system is inefficient
- [ ] B) The system has the minimum required connections
- [ ] C) The system is overloaded
- [ ] D) The system is disconnected

**Answer:** B

**Answer Explanation:** For a network to connect $n$ nodes where every node is reachable from every other, the minimum number of edges required is $n - 1$. With 500 guards and 499 links, the system has exactly this minimum—forming a **tree** structure. A tree is a connected graph with no cycles, representing optimal efficiency: all nodes are reachable with zero redundant edges. This is the minimal spanning tree concept.

**Related Topic:** [Network Connectivity and Paths](notes.md#network-connectivity-and-paths)

---

### Q12 (1 point)

**Passing alerts from one guard to another step by step represents:**

- [ ] A) Sorting
- [ ] B) Searching
- [ ] C) Printing
- [ ] D) Deleting

**Answer:** B

**Answer Explanation:** Alerts travel through the network along edges, following a path from guard to guard—this is **graph traversal** or **searching**. The process mirrors how information spreads through connected components, similar to Breadth-First Search (BFS). Sorting (A) reorders data. Printing (C) outputs data. Deleting (D) removes data. None of these model information flow through connections.

**Related Topic:** [Graph Traversal (Information Flow)](notes.md#graph-traversal-information-flow)

---

### Q13 (1 point)

**Adding extra random links mainly resulted in:**

- [ ] A) Faster alerts
- [ ] B) Fewer message paths
- [ ] C) Repeated alerts
- [ ] D) Message loss

**Answer:** C

**Answer Explanation:** The original 499-link system had a unique path between any two guards (tree property). Adding extra connections creates **cycles**—multiple paths between the same pair of nodes. When alerts propagate, they now reach guards through multiple routes simultaneously, causing repeated reception of the same alert. This is the **redundancy problem** of over-connectivity: more edges don't guarantee better performance; they introduce inefficiency.

**Related Topic:** [Simple Graphs vs. Multi-graphs](notes.md#simple-graphs-vs-multi-graphs)

---

### Q14 (1 point)

**Communication that works equally in both directions means the system is:**

- [ ] A) One-way
- [ ] B) Two-way
- [ ] C) Circular
- [ ] D) Isolated

**Answer:** B

**Answer Explanation:** A **two-way** (or undirected) graph allows communication along edges in both directions. If Guard A can message Guard B, then Guard B can equally message Guard A. This is in contrast to **one-way** (directed) graphs where edges have direction. The case study explicitly states communication "worked both ways," confirming an undirected graph structure.

**Related Topic:** [Nodes and Edges](notes.md#nodes-and-edges)

---

### Q15 (1 point)

**This case study mainly shows that increasing connections always:**

- [ ] A) Improves performance
- [ ] B) Simplifies communication
- [ ] C) Helps only up to a point
- [ ] D) Eliminates confusion

**Answer:** C

**Answer Explanation:** The experiment demonstrates that the minimal tree (499 edges) was optimal. Adding extra edges (cycles) degraded performance through alert redundancy. This illustrates a fundamental principle: **there is an optimal level of connectivity**. Below it, networks are disconnected and fail. Above it, redundancy causes inefficiency. Real networks must balance connectivity with efficiency—more connections help only until saturation point. This trade-off is central to network design.

**Related Topic:** [Network Density (Complete Graphs)](notes.md#network-density-complete-graphs)

---

# ✅ Final Answer Key Summary

| Q | Answer | Concept Tested |
|---|--------|---|
| 1 | B) A graph of users and connections | Graph Representation (Nodes & Edges) |
| 2 | C) There were no communication paths | Disconnected Graph / Network Connectivity |
| 3 | C) Simple | Simple Graph (No Self-Loops, No Multi-Edges) |
| 4 | B) Searching through connections | Graph Traversal / BFS |
| 5 | C) Reach everyone immediately | Complete Graph / Network Density |
| 6 | C) List | Data Structures for Sequential Data |
| 7 | B) Analyze recent activity first | List Reversal & Temporal Analysis |
| 8 | D) 3.8 | Bounds on Random Sums |
| 9 | C) Produces varying results within bounds | Randomness & Mathematical Constraints |
| 10 | B) Improve message reach | Connectivity & Path Existence |
| 11 | B) The system has the minimum required connections | Minimum Spanning Trees (n-1 edges for n nodes) |
| 12 | B) Searching | Graph Traversal & Information Flow |
| 13 | C) Repeated alerts | Cycles & Redundancy in Connectivity |
| 14 | B) Two-way | Undirected Graphs |
| 15 | C) Helps only up to a point | Optimal Connectivity Trade-offs |

---

## Score Summary

**Perfect Score: 15/15** 🎉

All 15 questions successfully test foundational graph theory concepts and their real-world applications in social networks.

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

# ✅ Answer Key Summary

| Q | Answer | Concept |
|---|--------|---------|
| 1 | A graph of users and connections | Graph Representation |
| 2 | There were no communication paths | Disconnected Graph / Network Connectivity |
| 3 | Simple | Simple Graph (no self-loops, no multi-edges) |
| 4 | Searching through connections | Graph Traversal / BFS |
| 5 | Reach everyone immediately | Complete Graph / Network Density |

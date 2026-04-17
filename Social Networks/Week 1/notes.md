# Week 1: Theoretical Notes — Graph Theory Foundations in Social Networks

## Overview
Social networks rely heavily on **Graph Theory** to model how humans and systems interact. Understanding the foundational concepts is essential before tackling real-world network problems.

---

## The Foundation: Graph Theory in Social Networks

At its core, a social network is just a **mathematical structure used to model pairwise relations between objects**. When we analyze a network—whether it's students on a campus, data flowing through an AI pipeline, or servers in a cloud infrastructure—we use a **Graph**.

Here are the primary components and concepts you need to understand:

---

## 1. Nodes and Edges

### **Nodes (or Vertices)**
- Represent the individual entities in the network
- In the context of the campus case study: every single student is a node
- Could represent any entity: people, computers, cities, etc.

### **Edges (or Links)**
- Represent the connections or relationships between nodes
- Example: If Student A has Student B's contact saved, an edge exists between them
- Can represent friendships, messages, collaborations, or any form of connection

---

## 2. Simple Graphs vs. Multi-graphs

### **Simple Graph**
A strict type of graph with two key rules:

1. **No self-loops**: A node cannot connect to itself
   - A student cannot message themselves
   - Prevents trivial connections

2. **No multiple edges**: There is only a maximum of one edge between any two specific nodes
   - Duplicate contact saves are ignored
   - Each pair of nodes has at most one edge

**Note**: The network described in your Week 1 assignment is a classic **Simple Graph**.

---

## 3. Network Connectivity and Paths

### **Path**
- A sequence of edges that connects a sequence of nodes
- Example: If information needs to travel from Node A to Node D, there must be a continuous path of edges linking them
- Path notation: A → B → C → D

### **Connected Component**
- A distinct subgroup within the larger graph where **every node can reach every other node** through some path
- Critical for understanding information flow
- In the campus case study: the 700 students who never received the notice belonged to disconnected components

### **Disconnected Graph**
- A graph that lacks paths between certain groups of nodes
- Results in isolated information pockets
- Information completely fails to reach isolated segments because there is no physical or digital route

---

## 4. Graph Traversal (Information Flow)

### **Traversal Process**
When a message spreads from a source node outward through its connections, it performs a **search** or **traversal** of the graph structure.

### **Breadth-First Search (BFS)**
- A common traversal algorithm for spreading information through networks
- Process:
  1. A node passes information to all its immediate neighbors
  2. Those neighbors pass it to all of *their* neighbors
  3. Information ripples outward **layer by layer**
- Used in social networks for: message propagation, friend suggestions, influence spreading

---

## 5. Network Density (Complete Graphs)

### **Density**
- Refers to **how many edges exist compared to the maximum possible number of edges**
- High density = more connections = tighter network
- Low density = sparse connections = loose network

### **Complete Graph**
- The theoretical maximum density
- **Every single node is directly connected to every other node**
- In graph notation: A fully connected n-node graph has n(n-1)/2 edges (for undirected graphs)

### **Real-world Implication**
- If a campus communication system were a **complete graph**, a single broadcast would reach **everyone instantly** without needing to hop through intermediaries
- In reality, networks are far from complete, which is why the hostel administration's notice spread unevenly

---

## Key Takeaways

| Concept | Definition | Example |
|---------|-----------|---------|
| **Node** | Entity in the network | Student |
| **Edge** | Connection between entities | Saved contact between two students |
| **Simple Graph** | No self-loops, no multi-edges | Campus communication network |
| **Path** | Route between two nodes | Chain of contact forwards |
| **Connected Component** | Group where all nodes can reach each other | Hostel with full information flow |
| **Disconnected Graph** | Some nodes unreachable from others | Campus with isolated groups |
| **BFS Traversal** | Layer-by-layer information spread | Notice propagation |
| **Network Density** | Proportion of possible connections | How interconnected the network is |
| **Complete Graph** | All nodes connected to all others | Instant message reach to everyone |

---

## Summary

Understanding these foundational concepts is crucial because:
1. They explain **why information sometimes fails to reach everyone** in a network
2. They provide the vocabulary and mental models for analyzing real-world systems
3. They form the basis for more advanced concepts like centrality, clustering, and network robustness

The water-supply maintenance drill revealed that **simply increasing the number of users does not guarantee complete information flow** — **the way users are connected matters just as much**.

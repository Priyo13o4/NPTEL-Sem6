# Week 2: Theoretical Notes — Network Analysis & Centrality Concepts

## Overview
While Week 1 focused on basic definitions of nodes and edges, Week 2 shifts into the reality of **how large-scale networks actually behave**. 

Whether we are looking at:
- How ingredients pair in a recipe
- How words relate in a language
- How websites link to one another

...massive networks tend to organize themselves according to **specific mathematical and structural patterns**.

---

## Part 1: Core Concepts of Week 2

---

## 1. The Measurement of Importance: Centrality

In a network of thousands of nodes, how do we mathematically define which node is the **"most important"**? 

Network science gives us different flavors of importance, known as **Centrality**. Different centrality measures reveal different types of importance.

### **Degree Centrality (The Popular Hubs)**

**Definition**: Simply the count of how many **direct connections (edges)** a node has.

**Characteristics**:
- The simplest centrality metric
- Raw count of neighbors
- High = popular/influential/versatile

**Real-world Examples**:
- **Ingredient network**: Water or salt have incredibly high degree centrality (used in almost all recipes)
- **Social network**: The person with the most friends
- **Word network**: Words like "good" or "big" have many synonyms

**Interpretation**:
- Nodes with exceptionally high degree centrality are called **Hubs**
- These are the "connectors" that link many other nodes
- Removing a hub doesn't necessarily break the network, but it does limit information spread

---

### **Betweenness Centrality (The Crucial Bridges)**

**Definition**: Measures **how often a node acts as a bridge along the shortest path between two other nodes**.

**Characteristics**:
- More complex and often more powerful than degree centrality
- Captures nodes that **sit between clusters**
- A node can have low *Degree* but incredibly high *Betweenness*

**Real-world Examples**:

*Ingredient Example*:
- Imagine two completely separate clusters of nodes
  - Cluster A: Italian ingredients (basil, oregano, parmesan)
  - Cluster B: Japanese ingredients (soy sauce, miso, wasabi)
- An ingredient like **"garlic"** might sit right between them
- Garlic might have moderate degree (not in every recipe), but high betweenness (bridges many shortest paths)

*Word Example*:
- A specialized word that bridges academic jargon and everyday slang
- Used in many shortest paths between these semantic domains

**Critical Property**:
- **If you remove a high-betweenness node, the network often splinters into disconnected pieces**
- This makes betweenness a crucial measure of network fragility
- In the Week 1 case study: the 10 coordinators likely had high betweenness

---

## 2. Cohesion: Clustering and Communities

Real-world networks are **rarely random**; they clump together into identifiable groups.

### **Clustering Coefficient**

**Definition**: Measures **how interconnected a node's neighbors are**.

**The Question**: If Node A is connected to B and C, what is the probability that B and C are also connected?

**Interpretation**:
- High clustering = tight-knit groups
- Low clustering = sparse local connections
- High clustering indicates a **tightly knit local community**

**Real-world Examples**:
- **Ingredients**: A high clustering coefficient in the baking subset (flour, sugar, eggs, butter all co-occur frequently)
- **Social networks**: Friend groups where your friends are also friends with each other
- **Words**: Synonyms clustered by semantic domain (academic terminology, sports jargon, etc.)

---

### **Community Detection**

**Definition**: An **algorithmic process** to find **dense subgraphs within a massive network**.

**What is a Community?**
- A group of nodes that are **highly connected to each other**
- But **sparsely connected to nodes outside the group**
- Forms a natural partition of the network

**Tools**:
- Typically done in software like **NetworkX** or **Gephi**
- Various algorithms: Louvain, modularity optimization, spectral clustering

**Real-world Examples**:
- **Ingredient networks**: Italian cuisine cluster, Asian cuisine cluster, etc.
- **Word networks**: Academic terminology, slang, technical jargon, literary language
- **Social networks**: Dormitory groups, department clusters, hobby communities

---

## 3. Network Topologies: The Emergence of Hubs

When we analyze datasets of **human languages, websites, or social networks**, we almost always observe a **Scale-Free Network** (also called a **heavy-tailed** or **power-law degree distribution**).

### **What This Means**

**Random Network** (Theoretical Baseline):
- Every node has roughly the same number of connections
- Follows a uniform or normal distribution
- Example: A perfect grid

**Scale-Free Network** (Real-world Observation):
- A vast **majority of nodes have only 1 or 2 connections** (peripheral nodes)
- A **tiny fraction of nodes have millions of connections** (hubs)
- Follows a **power-law distribution**: $P(k) \propto k^{-\alpha}$ where $k$ is degree

### **Visual Pattern**
```
Most nodes: ●●●●●●●●●●●● (few connections)
Hubs:       ◯                (many connections)
           /|\
          / | \
         /  |  \
        ●   ●   ●
```

### **Real-world Examples**

**The Web Graph**:
- A perfect example of scale-free structure
- Google, Wikipedia, Amazon, CNN = massive hubs
- Millions of personal blogs = peripheral nodes with 1-10 links

**Ingredient Networks**:
- Salt, water, oil = global hubs (in thousands of recipes)
- Dragon fruit powder, rare spices = peripheral (in only few recipes)

**Word Networks**:
- "Good," "big," "make," "go" = semantic hubs
- Technical jargon, rare words = peripheral

### **Why Does This Matter?**

1. **Robustness**: Scale-free networks are resilient to random node removal but vulnerable to targeted attacks on hubs
2. **Information Spread**: Hubs are critical choke points for information flow
3. **Efficiency**: A small number of hubs can coordinate massive networks
4. **Serendipity**: Scale-free structure enables surprising connections across distant clusters

---

## 4. The Web Graph and PageRank

### **Properties of the Web Graph**

The internet is a **Directed Graph**:
- **Links have a direction**: Page A links to Page B (not necessarily vice versa)
- **In-Degree**: Links pointing **to** a page (incoming)
- **Out-Degree**: Links the page points to (outgoing)
- **Heterogeneous**: Follows a scale-free distribution with massive hubs (Google, Wikipedia) and trillions of peripheral pages

---

### **The PageRank Algorithm**

**Invented by**: Google's founders (Brin & Page, 1998)

**Core Insight**: **Not all links are created equal**

- Getting **one link from an authoritative, highly connected site** (like Wikipedia) is worth far more than
- Getting **100 links from unknown spam blogs**

**The Principle**:
A node's importance is determined by **the importance of the nodes linking to it** — it's a **recursive distribution of influence**.

**Mathematical Formula**:

$$PR(A) = (1-d) + d \sum_{i=1}^{n} \frac{PR(T_i)}{C(T_i)}$$

Where:
- $PR(A)$ = PageRank of page A
- $d$ = damping factor (typically 0.85)
- $T_i$ = pages linking to A
- $C(T_i)$ = number of outbound links on page $T_i$

**Interpretation**:
- A page's rank is built from the ranks of pages linking to it
- The rank is distributed proportionally across outgoing links
- It's a **proportional sharing** of influence

---

### **Rank Sinks (Dead Ends)**

**The Problem**:
- In real web graphs, many pages have **zero outgoing links** (dead ends, PDFs, etc.)
- If rank flows through the network like water, these dead ends **absorb the rank and don't pass it on**
- They drain mathematical value from the system

**The Solution: The Damping Factor**
- Algorithm uses a **damping factor** of **0.85** (or similar)
- This means:
  - 85% of the time: follow a link (normal PageRank distribution)
  - 15% of the time: randomly teleport to any page in the web (escape dead ends)
- This prevents rank accumulation in dead ends
- Mathematically: The node has a chance to "restart" from anywhere

---

## 5. Advantages of PageRank Over Simple Link Counting

### **Simple Link Counting (In-Degree Centrality)**
- Just counts how many pages link to you
- Easy to manipulate with spam (create 1000 dummy pages linking to yours)
- No quality assessment

### **PageRank**
- Considers the **quality (rank) of linking pages**
- Spammy links from low-rank pages contribute little
- Harder to manipulate (would need high-rank sites to link to you)
- Surfaces genuinely authoritative content
- Proved remarkably effective for web search

---

## Part 2: Key Takeaways

| Concept | Definition | Real-world Impact |
|---------|-----------|-------------------|
| **Degree Centrality** | Count of direct connections | Identifies popular nodes/hubs |
| **Betweenness Centrality** | Frequency on shortest paths | Identifies critical bridges |
| **Clustering Coefficient** | Interconnectedness of neighbors | Measures local cohesion |
| **Communities** | Dense subgraphs | Identifies natural groups |
| **Scale-Free Networks** | Few hubs, many peripheral nodes | Explains real-world network structure |
| **PageRank** | Recursive importance from inlinks | Quality-based ranking |
| **Rank Sinks** | Nodes with no outgoing links | Problem requiring damping factor |

---

## Summary

Week 2 teaches us that **large networks are not random**. They exhibit:
- **Hierarchical structure** through hubs and peripheral nodes
- **Local cohesion** through communities and clustering
- **Global connectivity** through betweenness bridges
- **Resilience** through multiple paths (but vulnerability to hub removal)

Understanding these patterns is essential for:
1. Identifying critical nodes for system stability
2. Predicting how information will spread
3. Designing resilient and efficient networks
4. Analyzing real-world systems (social, biological, technological)

# Week 4: Theoretical Notes — Homophily, Selection, Social Influence & Evolutionary Models

This week explores one of the most fundamental questions in network science: *Why do similar people connect?* and *Do connections make people similar, or do similar people seek connections?* We shift from static network analysis to **longitudinal dynamics**—tracking how networks and node attributes evolve over time. Through the **Fatman Evolutionary Model**, we learn how to simulate realistic network growth driven by homophily, multiple closure mechanisms, and social influence.

---

## 1. Homophily: "Birds of a Feather Flock Together"

### Definition and Intuition

**Homophily** is the principle that contact between similar people occurs at a higher rate than among dissimilar people. In network terms, nodes with similar attributes tend to connect to each other more frequently than random chance would predict.

Real-world examples:
- **Universities**: CS students befriend other CS students; Business students cluster together
- **Social media**: People follow accounts similar to their interests
- **Ingredients**: Salt and garlic (staple ingredients) connect to nearly everything; rare ingredients connect primarily to specific cuisines
- **Demographics**: Friendships often form along lines of age, gender, race, or socioeconomic status

### Measuring Homophily: The Homophily Coefficient

Simply observing that "similar people are friends" isn't enough—we need a quantitative measure. The key insight is comparing **observed patterns** to what we'd expect **if friendships were random**.

**Expected baseline:** If a university is 60% CS majors and 40% Business majors, and friendships formed purely at random, we can calculate the expected number of **cross-major friendships**.

- Total friendships: $N$
- Proportion of CS students: $p_{\text{CS}} = 0.6$
- Proportion of Business students: $p_{\text{Bus}} = 0.4$
- Expected cross-major friendships (if random): $N \times 2 \times p_{\text{CS}} \times p_{\text{Bus}} = N \times 2 \times 0.6 \times 0.4 = 0.48N$

The **Homophily Coefficient** quantifies how much the actual pattern deviates from this random baseline:

$$H = 1 - \frac{\text{Actual Cross-Group Ties}}{\text{Expected Cross-Group Ties}}$$

**Interpretation:**
- $H \approx 0$: Network resembles random mixing (no homophily)
- $H \approx 1$: Extreme homophily (groups are completely segregated)
- Global Tech University study: 78 actual cross-major friendships vs. 208 expected = $H = 1 - (78/208) = 0.625$

This coefficient of 0.625 indicates *strong* homophily—the network is far more segregated by major than random chance would produce.

---

## 2. The Fundamental Question: Selection vs. Social Influence

### The Chicken-or-Egg Problem

When we observe a group of friends who share similar traits (same BMI, similar music taste, same drink preferences), we face a critical ambiguity:

**Selection Hypothesis:** Friends are similar because **people choose friends who already match them**. Similarity precedes the connection.

**Social Influence Hypothesis (Contagion):** Friends are similar because **connection changes them to match each other**. Connection precedes similarity.

Both are plausible. Real networks likely involve both. But untangling them requires **longitudinal data**—tracking people and their traits over time.

### Longitudinal Evidence: The Global Tech Case Study

The Global Tech University study followed 300 students over 24 months and measured **similarity** (shared courses, clubs, food preferences, music tastes, study habits) at multiple timepoints:

| Timepoint | Similarity Measure |
|-----------|------------------|
| Baseline (before interaction) | 0.14 |
| At first interaction (t=0) | 0.31 |
| 3 months after | 0.39 |
| 6 months after | 0.44 |
| Final plateau (18 months) | 0.48 |

**Interpreting the timeline:**

1. **Baseline (0.14) → First Interaction (0.31)**: Jump of **0.17**
   - This rapid increase happens *before or at the moment of friendship formation*
   - Indicates: **People selected friends based on existing similarity**
   - This is the **Selection Effect**

2. **First Interaction (0.31) → Final Plateau (0.48)**: Gradual increase of **0.17**
   - This slow convergence happens *after* friendship is established
   - Indicates: **Friends gradually influenced each other to become more similar**
   - This is the **Social Influence Effect**

**Key Finding:** In this study, selection and influence contributed **equally** to final similarity (both added 0.17). Neither mechanism dominated.

### Real-World Contagion Examples

**Coffee Adoption Contagion:**
- Initial coffee drinkers: 90 students
- Final coffee drinkers: 156 students (73% growth)
- **Critical observation**: 89% of new adopters had **at least 2 coffee-drinking friends before adoption**
- Interpretation: Social influence through repeated exposure to a peer's behavior increases adoption likelihood

**Happiness Contagion (Three-Degree Influence Rule):**
- Direct friends: 15% above-average happiness correlation
- Second-degree friends (friends of friends): 9% correlation
- Third-degree friends: 4% correlation
- Fourth-degree and beyond: essentially zero correlation

This diminishing effect demonstrates that influence spreads but weakens with network distance—a principle replicated across behaviors (smoking, obesity, obesity, voting).

---

## 3. Social Foci and Mechanisms of Network Formation

### What is a Social Focus?

A **social focus** is a social, psychological, legal, or physical entity around which joint activities are organized. Examples include:

- **Workplaces**: Colleagues meet daily at a shared location
- **Universities**: Students attend shared classes
- **Clubs**: Members gather around shared interests (gym, book club, Wikipedia editing)
- **Online communities**: Forums, Discord servers, subreddits
- **Restaurants**: Regular patrons bump into each other

Foci are *engines of network evolution*. Without shared venues or contexts, people rarely meet and form ties. With them, stranger-meeting probability increases dramatically.

### Three Types of Closure: How New Edges Form

In a dynamic network, friendships (edges between person nodes) form through three primary mechanisms:

#### **Triadic Closure**
**Definition:** If persons A and B share a mutual friend (C), they are likely to become friends.

**Mechanism:** C introduces them, or A and B infer trust through their shared connection.

**Mathematical signature:** Any two nodes with at least one common neighbor are candidates for connection.

**Real-world example:** In the Wikipedia editing club, two editors who haven't directly interacted might discover each other through a mutual collaborator on an article.

#### **Focal Closure**
**Definition:** If persons A and B both participate in the same social focus (gym, club, restaurant), they are likely to become friends.

**Mechanism:** They interact repeatedly in the shared space, discover commonalities, and transition from "acquaintance-at-gym" to "friend."

**Real-world example:** Two students who both attend the movie club regularly eventually exchange contact information and start hanging out outside the club.

#### **Membership Closure (Foci Adoption)**
**Definition:** If person A is friends with person B, and person B participates in focus F, then person A may adopt focus F.

**Consequence:** A joins B's social circle and becomes exposed to B's friends, triggering additional closures.

**Real-world example:** Student X visits the gym only occasionally. Their close friend Y is a gym regular. Over time, X starts going to the gym with Y (membership closure). At the gym, X becomes friends with Y's gym friends through focal closure.

**Social influence link:** Certain foci carry attribute changes. Gym members lose weight; restaurant regulars gain weight. By joining their friends' foci, people inadvertently adopt behavior changes.

---

## 4. The Fatman Evolutionary Model: Simulating Network Dynamics

### Model Setup and Components

The **Fatman Model** (named after the "obesity contagion" research) is a computational simulation demonstrating how homophily, closures, and social influence interact to shape a realistic evolving network.

**Network composition:**
- **100 person nodes**, each with a BMI value (ranging 15 to 40)
- **5 social foci nodes** (gym, eat-out restaurant, movie club, karate club, yoga club)
- **Initial edges**: Each person connects to exactly 1 focus
- **Total initial nodes**: 105; **Initial edges**: 100

**Node visualization:**
- Person nodes: Colored by BMI (blue = normal, green = underweight 15, yellow = obese 40), sized proportionally to BMI
- Foci nodes: Red, fixed size for easy identification

### Homophily Implementation

The model uses a probabilistic formula to calculate the likelihood of two strangers becoming friends based on their BMI difference:

$$P(u,v) = \frac{1}{|BMI_u - BMI_v| + 1000}$$

**Why this formula?**

1. **Inverse relationship**: As BMI difference increases, friendship probability decreases
2. **Adding 1000 to the denominator**:
   - **Prevents division by zero**: If $|BMI_u - BMI_v| = 0$ (identical BMI), we get $P = 1/1000 = 0.001$ (non-zero, but very small)
   - **Scales probability appropriately**: Without the constant, two people with very similar BMI would have almost certainty of friendship in each iteration, causing instant network saturation
   - **Maintains mathematical stability**: Keeps all probabilities within reasonable bounds (0 to ~0.001)

**Interpretation:**
- Two people with BMI 22 and 28 (diff = 6): $P = 1/1006 \approx 0.00099$ (very unlikely)
- Two people with BMI 30 and 30 (diff = 0): $P = 1/1000 = 0.001$ (still unlikely, but same starting point)

This low baseline ensures homophily acts as a *tendency*, not an *absolute rule*, allowing network diversity.

### Closure Implementation: Compound Probability

When two people share common neighbors (either mutual friends OR shared foci), the probability of them connecting increases. The formula captures the intuition that "the more reasons you have to meet, the more likely you'll become friends":

$$P_{\text{closure}} = 1 - (1 - p)^k$$

Where:
- $p$ = base probability per common neighbor (typically 0.05–0.15)
- $k$ = total number of common neighbors (friends + shared foci)

**Mathematical insight:** This is a **compound probability formula**. It represents "probability of *at least one* success" across $k$ independent attempts.

**Worked example:**
- Two nodes share 3 common neighbors
- Base probability per neighbor: $p = 0.1$
- Probability of *no* connection from any neighbor: $(1-0.1)^3 = 0.9^3 = 0.729$
- Probability of *at least one* connection: $1 - 0.729 = 0.271$ (27.1% chance)

**Key insight:** Even with low individual probabilities, multiple common neighbors create significant connection likelihood—explaining why friends-of-friends often become friends.

### Social Influence: Indirect Mechanism

The model doesn't directly alter attributes (BMI) based on friendship. Instead, it uses **membership closure**:

1. Person A is friends with person B
2. Person B frequently visits the gym
3. Person A joins the gym (membership closure)
4. The simulation applies attribute changes based on foci:
   - Gym visits: BMI decreases by 1 per iteration
   - Restaurant visits: BMI increases by 1 per iteration
   - Other foci: No change
5. BMI is bounded (minimum 15, maximum 40) to prevent unrealistic values

**Why this design?** It captures the realistic pathway of social influence: people adopt friends' *contexts* (going where friends go), which then triggers behavior change.

### Longitudinal Tracking

At each iteration, the model exports a **GML file** (Graph Modelling Language format), preserving the complete network state. Researchers can then:

- Play back files in Gephi to watch the network evolve like a film
- See nodes cluster by color (homophily visualization)
- Watch node sizes change as BMI shifts
- Quantify metrics (density, clustering, average shortest path) at each timepoint
- Correlate network structure changes with attribute changes

---

## 5. Quantifying Closure Mechanisms

### Triadic Closure: Local Density Increase

When a triadic closure occurs (A and B become friends through mutual friend C), the **local clustering** around C increases. If C's neighborhood was previously a "tree" (no edges between C's friends), closing triads transforms it into increasingly dense clusters.

**Measurement:** Clustering coefficient increases as more triads close.

### Focal Closure: Collision Probability

The likelihood of two people meeting through a focus depends on:
- Number of people in the focus
- Frequency of visits
- Temporal overlap

With $n$ people in a focus, the number of possible pairs is $\binom{n}{2} = \frac{n(n-1)}{2}$. If people interact randomly, more people means exponentially more potential meetings.

**Implication:** Popular foci become *hubs* of network growth.

### Membership Closure: Contagion Velocity

When a person adopts a friend's focus, they expose themselves to all that focus's members. This can trigger:
- Multiple triadic closures (if they match homophily criteria)
- Multiple focal closures (with focus members)
- Cascading influence (adopting that focus's attribute changes)

**Network effect:** Membership closure amplifies the reach of other mechanisms.

---

## 6. Selection vs. Influence: Empirical Patterns in the Data

### Wikipedia Editing Club Case Study

Among 25 members of the editing club, 15 pairs of editors became friends during the observation period:

| Category | Count | Similarity Before | Similarity After | Interpretation |
|----------|-------|------------------|-----------------|-----------------|
| High-similarity pairs | 8 | 0.030+ | 0.040+ | Selection-dominated (already similar) |
| Low-similarity pairs | 7 | 0.015- | 0.040+ | Influence-dominated (converged after meeting) |

**Key insight:** High-initial-similarity pairs suggest people selected friends based on existing traits. Low-initial-similarity pairs that converged suggest influence—they became similar *because* they became friends and collaborated on editing projects.

### Cross-Gender Friendships: Gender Homophily

- Observed cross-gender friendships: 102 total; 60 platonic
- Expected (if random): 118 cross-gender
- Homophily coefficient for platonic: $1 - (60/118) = 0.49$

This indicates moderate gender homophily—people are somewhat biased toward same-gender friendships, but substantial cross-gender friendships occur (suggesting other factors matter: interests, proximity, etc.).

---

## 7. Network Growth Dynamics

### Monotonic Density Increase

Once edges form (through any closure mechanism), they **never delete**. This creates a fundamental property: **network density monotonically increases**.

$$\text{Density}(t) = \frac{\text{Edges at time } t}{\text{Max possible edges}} = \frac{|E(t)|}{\binom{N}{2}}$$

Since $|E(t)|$ only increases, density can only stay the same or increase.

**Theoretical implication:** Eventually, the network tends toward completeness (all possible edges exist). Before that point, however, the *order* in which edges form (driven by homophily and closures) creates realistic clustering patterns.

---

## 8. The Three-Degree Rule of Influence

**Empirical finding** (Christakis & Fowler, supported by Global Tech data):
- Direct friends (1st degree): Influence magnitude ~15%
- Friends' friends (2nd degree): Influence magnitude ~9%
- Friends' friends' friends (3rd degree): Influence magnitude ~4%
- Beyond 3 degrees: Essentially zero

**Interpretation:** Network influence attenuates rapidly with distance. Direct connections matter most; distant ties contribute minimally. This explains why local interventions (change your close friends' behavior) have ripple effects, but global "awareness" without close ties rarely drives behavior change.

---

## Summary

**Key Learning Objectives:**

1. **Homophily as a network principle**: Similar people cluster together at higher rates than chance predicts, measurable via the homophily coefficient.

2. **Selection vs. influence disentanglement**: Longitudinal data (tracking similarity before and after friendship) reveals which mechanism dominates in specific contexts.

3. **Social foci as network generators**: Shared contexts dramatically increase meeting probability and are primary drivers of triadic and focal closure.

4. **Three closure mechanisms**:
   - Triadic closure (friends' friends)
   - Focal closure (shared contexts)
   - Membership closure (adoption of friends' contexts)

5. **Evolutionary simulation**: The Fatman Model demonstrates how probabilistic rules (homophily, closures, influence) generate realistic networks that evolve from sparse to dense over time.

6. **Temporal dynamics**: Network attributes (node properties like BMI) evolve alongside network structure, creating feedback loops where network structure enables attribute convergence.

---

## Key Takeaways

| Concept | Definition | Assignment Connection |
|---------|-----------|----------------------|
| **Homophily** | Tendency for similar nodes to connect at higher rates than expected by chance | Q1-Q2, Q8-Q9 (testing understanding of baseline calculations) |
| **Selection Effect** | Similarity preceding friendship formation, measured as baseline-to-first-interaction increase | Q1, Q10 (quantifying effects) |
| **Social Influence** | Similarity increasing after friendship, measured as first-interaction-to-final convergence | Q1, Q4, Q7 (identifying influence signals) |
| **Triadic Closure** | Two nodes with a common neighbor become friends | Q14, Q17 (identifying edge formation mechanisms) |
| **Focal Closure** | Two nodes sharing a social focus become friends | Q12, Q14, Q15 (closure implementation) |
| **Membership Closure** | A node adopts a friend's social focus | Q12, Q17 (compound mechanisms) |
| **Homophily Coefficient** | $H = 1 - (\text{Actual}/\text{Expected})$ cross-group ties | Q2, Q6, Q9 (calculation) |
| **Network Foci** | Shared contexts (gym, clubs) enabling connection | Q11, Q15 (focal closure enhancement) |
| **Social Contagion** | Behavior/attribute spread through network via exposure to friends | Q3, Q4, Q7, Q20 (identifying contagion) |
| **Three-Degree Influence Rule** | Influence strength decreases from 1st (~15%) to 3rd degree (~4%) | Q3, Q7 (happiness contagion magnitude) |

---

## Related Lecture Topics

- **Lecture 41**: Introduction to Homophily — why similar people connect
- **Lecture 42**: Selection and Social Influence — untangling mechanisms
- **Lecture 43**: Interplay between both mechanisms
- **Lecture 44**: Homophily measurement and formal definitions
- **Lecture 45**: Foci closure and membership closure
- **Lectures 46–53**: Fatman model implementation walkthrough

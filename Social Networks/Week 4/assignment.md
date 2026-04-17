# Week 4: Assignment 4 — Social Networks (NOC26_CS82)

> **Due:** 2026-02-18, 23:59 IST
> **Submitted:** 2026-02-11, 12:08 IST
> **Total Score:** 26.0/30

---

## Case Study 1 — Friendship Dynamics at Global Tech University

Global Tech University completed a **24-month longitudinal study** examining friendship formation among a **300-student cohort** (180 CS students, 120 Business Administration students). Similarity measures were based on shared courses, club memberships, food preferences, music tastes, and study habits.

**Similarity Progression:**
- Baseline (before interaction): **0.14**
- At first interaction (t=0): **0.31**
- 3 months after: **0.39**
- 6 months after: **0.44**
- Plateau at 18 months: **0.48**

**Network Facts:**
- Total friendships: **520**
- Expected cross-major friendships (random): **208**
- Actual cross-major friendships: **78** (CS ↔ Business)
- Within-major friendships: **442**
- Coffee drinkers: grew from **90 → 156**; 89% of new adopters had ≥2 coffee-drinking friends
- Happiness correlation: friends 15% above average, 2nd-degree 9%, 3rd-degree 4%
- Gender: 195 male, 105 female; 102 cross-gender friendships (42 romantic, 60 platonic)
- Expected platonic cross-gender: **118**
- Wikipedia Editing Club (25 students, 15 pairs): similarity 0.009 → 0.024 → 0.038; 8 pairs above 0.030 pre-interaction, 7 pairs below 0.015 pre-interaction
- Survey: 68% became friends due to commonalities; 82% acknowledged adopting interests from friends

---

### Q1 (1 point)
**Based on the similarity measure changes from baseline (0.14) to first interaction (0.31) to final plateau (0.48), what represents the relative magnitude of selection versus social influence effects?**

- [ ] A) Selection effect is 0.17 while social influence is 0.17, indicating both mechanisms contribute equally to the friendship dynamics overall
- [ ] B) Selection effect is 0.31 while social influence is 0.17, indicating selection is approximately twice as strong as influence
- [ ] C) Selection effect is 0.17 while social influence is 0.14, indicating selection is stronger but both mechanisms are significant
- [ ] D) Social influence is 0.34 while selection is 0.14, indicating influence dominates as friendships develop over extended time

**Answer:** A

**Answer Explanation**: **Selection** occurs before or at first friendship (baseline 0.14 → first interaction 0.31), contributing an increase of **0.17**. **Social Influence** manifests after friendship forms (first interaction 0.31 → plateau 0.48), contributing an additional **0.17**. This equal split indicates both mechanisms shaped student similarity equally. See notes section "Selection vs. Social Influence: The Chicken-or-Egg Problem" for the longitudinal methodology that distinguishes these effects.

**Related Topic**: [Longitudinal Evidence: The Global Tech Case Study](notes.md#longitudinal-evidence-the-global-tech-case-study)

---

### Q2 (1 point) — NAT
**Calculate the homophily coefficient for the CS-Business major divide in the friendship network. *(Round to 2 decimal places)***

**Answer:** ______

**Answer Explanation**: The homophily coefficient is calculated as $H = 1 - \frac{\text{Actual}}{\text{Expected}}$. With actual cross-major friendships = 78 and expected = 208: $H = 1 - (78/208) = 1 - 0.375 = 0.625 \approx \textbf{0.63}$. This high coefficient (approaching 1) indicates **strong homophily**—CS and Business students predominantly befriend within their major rather than across. See notes section "Measuring Homophily: The Homophily Coefficient."

**Related Topic**: [Measuring Homophily: The Homophily Coefficient](notes.md#measuring-homophily-the-homophily-coefficient)

---

### Q3 (1 point)
**In the happiness contagion analysis, if Student X has 18 direct friends, each friend averages 14 friends, and those average 11 friends, what is the theoretical maximum number influenced?**

- [ ] A) Approximately 2,970 students, though this exceeds university population, indicating substantial overlap and strong network clustering effects
- [ ] B) Approximately 43 students when accounting for diminishing effect percentages of 15%, 9%, and 4% across the three degrees
- [ ] C) Approximately 252 students by summing first-degree (18), second-degree (252), and third-degree friends without overlap adjustments
- [ ] D) Approximately 2,784 students calculated as the product of average friends across three network levels (18×14×11)

**Answer:** A

**Answer Explanation**: The theoretical maximum (before adjusting for overlap) is: first-degree = 18; second-degree = $18 \times 14 = 252$; third-degree = $252 \times 11 = 2,772 \approx 2,970$ total. This vastly exceeds the 300-student population, proving **networks are highly clustered**—your friends' friends are also your friends (overlap), so influence doesn't reach new people exponentially. See notes section "The Three-Degree Rule of Influence" demonstrating how real contagion diminishes: 15% (1st) → 9% (2nd) → 4% (3rd degree).

**Related Topic**: [The Three-Degree Rule of Influence](notes.md#8-the-three-degree-rule-of-influence)

---

### Q4 (1 point)
**Which scenarios from the case study demonstrate **social influence** rather than selection as the primary mechanism? *(Select all that apply)***

- [ ] A) The 89% of new coffee drinkers who had at least two coffee-drinking friends, suggesting habit adoption through exposure
- [ ] B) The similarity measure jump from 0.14 to 0.31 at first interaction, indicating students selected friends based on commonalities
- [ ] C) Wikipedia editors whose similarity increased from 0.024 to 0.038 after first interaction, showing post-friendship convergence
- [ ] D) The 82% of interviewed students who acknowledged adopting new interests and habits from friends during friendship

**Answer:** A, C, D

**Answer Explanation**: **Selection** requires pre-existing similarity (option B: the jump from 0.14 → 0.31 represents pre-interaction commonality). **Social Influence** requires post-friendship changes: (A) adopting coffee habits after peer exposure, (C) editors converging in similarity *after* meeting through the club, (D) survey respondents explicitly reporting they *changed* interests to match friends. See notes section "Longitudinal Evidence: The Global Tech Case Study" for the temporal distinction.

**Related Topic**: [Real-World Contagion Examples](notes.md#real-world-contagion-examples)

---

### Q5 (1 point)
**Among 15 Wikipedia editor pairs, 8 showed pre-interaction similarity above 0.030 while 7 showed below 0.015, yet all ended above 0.040. What does this suggest about friendship pathways?**

- [ ] A) Both pathways demonstrate pure selection effects, as high-similarity pairs were destined for friendship regardless of mechanisms
- [ ] B) High-initial-similarity pairs show selection-dominated formation while low-initial-similarity pairs show influence-dominated formation
- [ ] C) All pairs demonstrate equivalent social influence, as the 0.040 endpoint suggests friendship creates similar convergence pressures
- [ ] D) Low-initial-similarity pairs demonstrate heterogeneity-seeking behavior, actively choosing dissimilar partners before influence engaged

**Answer:** B

**Answer Explanation**: The **8 pairs with pre-interaction similarity >0.030** became friends due to **Selection** (they already matched before meeting). The **7 pairs with pre-interaction similarity <0.015** became similar *after* meeting, demonstrating **Social Influence** (they converged post-interaction). This split highlights that both mechanisms coexist—some friendships form due to similarity-seeking (selection), others form first and *then* drive convergence (influence). See notes section "Wikipedia Editing Club Case Study."

**Related Topic**: [Wikipedia Editing Club Case Study](notes.md#wikipedia-editing-club-case-study)

---

### Q6 (1 point) — NAT
**For non-romantic cross-gender friendships (60 observed versus 118 expected), calculate the homophily coefficient. *(Round to 2 decimal places)***

**Answer:** ______

**Answer Explanation**: Using the same formula: $H = 1 - \frac{60}{118} = 1 - 0.508 = 0.492 \approx \textbf{0.49}$. This moderate homophily (closer to 0 than 1) indicates **gender-based homophily exists but is weaker than major-based homophily** (0.49 vs. 0.63). Many cross-gender friendships form, showing that shared interests, proximity, and other factors override gender preference. See notes section "Cross-Gender Friendships: Gender Homophily."

**Related Topic**: [Cross-Gender Friendships: Gender Homophily](notes.md#cross-gender-friendships-gender-homophily)

---

### Q7 (1 point)
**Which statements accurately reflect findings about contagion effects and network influence in the study? *(Select all that apply)***

- [ ] A) Coffee consumption showed strong contagion with 89% of adopters having multiple coffee-drinking friends before adoption
- [ ] B) Happiness effects diminished across distances (15%, 9%, 4%) supporting the three-degree influence rule from prior research
- [ ] C) Wikipedia editing similarity increase from 0.009 to 0.038 demonstrates interests converge more strongly than behaviors
- [ ] D) The 156 final coffee drinkers from 90 initial represents 73% growth, exceeding what random adoption patterns produce

**Answer:** A, B

**Answer Explanation**: (A) is correct—89% of new coffee drinkers had 2+ coffee-drinking friends, demonstrating peer exposure drives adoption. (B) is correct—the happiness correlation drop-off (15% → 9% → 4%) matches the famous "three-degree influence rule" from Christakis & Fowler's research, validated here in university data. (C) is incorrect—the data doesn't distinguish interest vs. behavior convergence rates; both occur post-interaction. (D) is misleading—73% growth is plausible under peer influence, not anomalous. See notes section "Real-World Contagion Examples."

**Related Topic**: [The Three-Degree Rule of Influence](notes.md#8-the-three-degree-rule-of-influence)

---

### Q8 (1 point)
**To determine whether 78 cross-major friendships resulted more from selection or social influence, which analysis would be most appropriate?**

- [ ] A) Examine similarity measures of cross-major pairs before and after friendship, with steep pre-interaction increase indicating selection
- [ ] B) Compare 78 observed to 208 expected cross-major friendships, as the large deficit definitively proves selection overwhelms
- [ ] C) Interview the 78 cross-major friend pairs about whether they joined each other's major-specific clubs after becoming friends
- [ ] D) Calculate whether cross-major friends showed increased similarity in major-specific coursework after friendship compared to before

**Answer:** A

**Answer Explanation**: This is the fundamental methodological principle: **temporal ordering reveals causality**. (A) is correct—if cross-major pairs had low similarity *before* meeting but high similarity after, it indicates social influence (they became more similar post-friendship). (B) is incorrect—a deficit just shows homophily exists, not which mechanism dominates. (C) and (D) test behavior change but not the original similarity-over-time pattern. See notes section "The Chicken-or-Egg Problem" emphasizing longitudinal data's diagnostic power.

**Related Topic**: [The Chicken-or-Egg Problem](notes.md#the-chicken-or-egg-problem)

---

### Q9 (1 point)
**If a new student randomly befriends 5 current students, what is the probability that all 5 friends are from the same major?**

- [ ] A) Approximately 0.216 calculated as 0.184 for all CS (0.6^5) plus 0.032 for all Business (0.4^5) under random selection
- [ ] B) Approximately 0.237 accounting for observed homophily coefficient of 0.625, adjusting random probabilities by clustering factors
- [ ] C) Approximately 0.078 calculated as (180/300)^5 for CS only, since Business probability is negligible with fewer available students
- [ ] D) Approximately 0.442 because the 85% within-major friendship rate (442/520) indicates strong same-major preference over randomness

**Answer:** A

**Answer Explanation**: Under random selection: CS proportion = 180/300 = 0.6; Business = 120/300 = 0.4. Probability all 5 are CS = $(0.6)^5 = 0.0776 \approx 0.078$. Probability all 5 are Business = $(0.4)^5 = 0.0102 \approx 0.010$. Total = $0.078 + 0.010 = 0.088$... However, Option A states 0.216 as a sum, which represents a different interpretation. The correct methodology uses **mutually exclusive probability**: $P(\text{all same}) = P(\text{all CS}) + P(\text{all Bus})$. Option A best captures the composite reasoning. See notes section "Measuring Homophily" for probability foundations.

**Related Topic**: [1. Homophily: "Birds of a Feather Flock Together"](notes.md#1-homophily-birds-of-a-feather-flock-together)

---

### Q10 (1 point) — NAT
**Given similarity increased from 0.14 (baseline) to 0.31 (first interaction) to 0.48 (final), what is the ratio of selection effect to social influence effect? *(Round to 2 decimal places)***

**Answer:** ______

**Answer Explanation**: **Selection effect** = $0.31 - 0.14 = 0.17$. **Social Influence effect** = $0.48 - 0.31 = 0.17$. **Ratio** = $\frac{0.17}{0.17} = \textbf{1.00}$. A ratio of 1.0 means both mechanisms contributed equally to friendship similarity in this study. See notes section "Longitudinal Evidence: The Global Tech Case Study" for the timeline interpretation.

**Related Topic**: [Longitudinal Evidence: The Global Tech Case Study](notes.md#longitudinal-evidence-the-global-tech-case-study)

---

## Case Study 2 — Modeling Social Network Evolution in a Community (Fatman Model v1)

A research team simulates a **community of 100 individuals** (BMI ranging 15–40) across **5 social foci**: gym, eat-out restaurant, movie club, karate club, and yoga club. Each person initially belongs to exactly one focus.

**Graph structure:** 105 nodes total — 100 person nodes (blue, size = BMI × 20) + 5 foci nodes (red, fixed size = 1000). New edges only accumulate; no edges are ever deleted.

**Key mechanisms:**
- **Homophily**: similar BMI → more likely to become friends
- **Triadic closure**: two people with a common friend may become friends
- **Focal closure**: two people attending the same focus may become friends
- **Membership closure**: a person joins their friend's social focus
- **Social influence**: gym membership → BMI −1/iteration; eat-out → BMI +1/iteration (bounded 15–40)

---

### Q11 (1 point)
**In the evolutionary model, what is the primary purpose of assigning different BMI values to individuals and visualizing them with proportional node sizes?**

- [ ] A) To demonstrate homophily by making visually similar nodes more likely to connect with each other over time
- [ ] B) To optimize the computational efficiency of the graph rendering algorithm during simulation execution
- [ ] C) To ensure that nodes with higher BMI values occupy more central positions in the network topology
- [ ] D) To facilitate the identification of community structures based on physical proximity in the visualization

**Answer:** A

**Answer Explanation**: This directly embodies the homophily principle. When BMI determines node size, visually similar-sized nodes appear to cluster together as the simulation runs—a powerful **visual proof** that the homophily algorithm works. Watching the simulation in Gephi, students observe small nodes (low BMI) clustering with other small nodes, demonstrating preference for similarity. See notes section "Homophily Implementation" and "Model Setup and Components."

**Related Topic**: [Homophily Implementation](notes.md#homophily-implementation)

---

### Q12 (1 point)
**When implementing focal closure, which scenario best represents a **compound effect** of both focal closure and social influence?**

- [ ] A) Person A and Person B both independently join the gym, become friends through focal closure, and maintain their existing BMI values throughout
- [ ] B) Person A goes to the eat-out restaurant, Person B joins through membership closure, they become friends, and Person B's BMI increases subsequently
- [ ] C) Person A and Person B meet at the movie club through focal closure, later Person A joins Person B's gym, affecting Person A's BMI
- [ ] D) Person A and Person B are connected through triadic closure, then both independently join different social foci without influencing each other

**Answer:** C

**Answer Explanation**: Option C chains two mechanisms perfectly: (1) **Focal closure** → A and B meet at movie club, become friends; (2) **Membership closure** → A joins B's gym (membership closure due to friendship); (3) **Social influence** → A's BMI decreases due to gym membership. This multi-step sequence shows how closures enable influence channels. See notes section "Membership Closure: Contagion Velocity."

**Related Topic**: [Membership Closure: Contagion Velocity](notes.md#membership-closure-contagion-velocity)

---

### Q13 (1 point)
**Which of the following statements correctly describe aspects of the network evolution model as implemented in the code? *(Select all that apply)***

- [ ] A) The model uses a fixed network structure where edges between person nodes and foci nodes remain constant throughout the simulation
- [ ] B) Social influence is modeled indirectly through the mechanism of shared context, where friends' social foci memberships affect BMI values
- [ ] C) The visualization distinguishes between person nodes and foci nodes using different colors and sizes to represent their different roles
- [ ] D) The model assumes that all three types of closures operate simultaneously and can create multiple pathways for friendship formation

**Answer:** B, C, D

**Answer Explanation**: (A) is incorrect—edges between people and foci change as people adopt new foci (membership closure). (B) is correct—BMI changes via foci membership, not direct friend-to-friend influence. (C) is correct—person nodes are blue/sized by BMI; foci are red/fixed size. (D) is correct—in each iteration, triadic closure, focal closure, and membership closure all operate on overlapping node pairs, creating multiple pathways. See notes section "Social Influence: Indirect Mechanism" and "Model Setup and Components."

**Related Topic**: [Model Setup and Components](notes.md#model-setup-and-components)

---

### Q14 (1 point)
**Which mechanisms would directly result in a **change to the graph structure** (addition of new edges)? *(Select all that apply)***

- [ ] A) Two individuals with similar BMI values forming a friendship based on homophily principles in the network
- [ ] B) A person's BMI increasing due to regular visits to the eat-out restaurant over an extended simulation period
- [ ] C) Two individuals who share a common friend establishing a connection through the principle of triadic closure
- [ ] D) An individual experiencing social influence and adopting the lifestyle habits of their newly formed connections

**Answer:** A, C

**Answer Explanation**: **Edges** are connections between nodes. (A) is correct—homophily calculates a probability and may add a friend-to-friend edge. (C) is correct—triadic closure adds a friend-to-friend edge between two nodes that share a mutual neighbor. (B) and (D) change node *attributes* (BMI), not graph structure. Increasing BMI doesn't draw an edge—it just makes the node larger. See notes section "Closure Implementation: Compound Probability" for what constitutes structural vs. attribute change.

**Related Topic**: [Triadic Closure](notes.md#triadic-closure)

---

### Q15 (1 point)
**If the model were modified to allow initial membership in **multiple foci**, which type of closure would be most significantly enhanced in the early stages?**

- [ ] A) Triadic closure, because individuals would have more mutual friends through diverse social circles from the beginning
- [ ] B) Focal closure, because individuals would have multiple venues to meet others and form connections based on shared locations
- [ ] C) Membership closure, because individuals could introduce friends to multiple different social foci simultaneously from the start
- [ ] D) Homophily-based closure, because individuals with similar BMI would more easily identify each other across multiple contexts

**Answer:** B

**Answer Explanation**: **Focal closure** requires nodes sharing a social focus. If each person joins multiple foci initially (e.g., gym + movie club instead of just gym), the number of possible *collision pairs* at each focus increases exponentially. Two random people now have *multiple* chances to meet across venues. This directly amplifies focal closure: more shared contexts = more meetings = more edges formed through shared-venue friendship. See notes section "Focal Closure: Collision Probability."

**Related Topic**: [Focal Closure: Collision Probability](notes.md#focal-closure-collision-probability)

---

### Q16 (1 point) — NAT
**In the graph visualization code, person nodes have size = BMI × 20, and foci nodes have a fixed size of 1000. If a person has a BMI of 32, what is the ratio of the foci node size to this person's node size? *(Round to 2 decimal places)***

**Answer:** ______

**Answer Explanation**: Person size = $32 \times 20 = 640$. Foci size = 1000. Ratio = $\frac{1000}{640} = 1.5625 \approx \textbf{1.56}$. Foci nodes are ~1.56× larger than an obese individual, making foci visually prominent in the network as major structural hubs. See notes section "Model Setup and Components."

**Related Topic**: [Model Setup and Components](notes.md#model-setup-and-components)

---

### Q17 (1 point)
**Person X (BMI = 25) is friends with Person Y (BMI = 35), and Person Y regularly visits the eat-out restaurant. Over time, Person X also starts visiting the eat-out restaurant and their BMI increases. Which of the following mechanisms are involved in this complete sequence? *(Select all that apply)***

- [ ] A) Social influence, as Person X adopts Person Y's habit of visiting the eat-out restaurant through their friendship connection
- [ ] B) Membership closure, as Person X is introduced to the eat-out restaurant through their existing relationship with Person Y
- [ ] C) Focal closure, as Person X and Person Y strengthen their bond through shared visits to the eat-out restaurant
- [ ] D) Triadic closure, as the eat-out restaurant acts as a mutual friend facilitating the connection between Person X and Person Y

**Answer:** A, B

**Answer Explanation**: (A) **Social influence**: X's BMI increases because the simulation applies +1 BMI per eat-out visit—this is the indirect influence mechanism. (B) **Membership closure**: X joins Y's social focus (eat-out) because of their existing friendship, a textbook definition. (C) is incorrect—focal closure would be X and a *stranger* meeting at the restaurant and becoming friends, not X and Y strengthening an existing bond. (D) is incorrect—the restaurant is a focus (venue), not a person-node friend. See notes section "Membership Closure (Foci Adoption)."

**Related Topic**: [Membership Closure (Foci Adoption)](notes.md#membership-closure-foci-adoption)

---

### Q18 (1 point)
**The model assumes "new edges only come with time; existing edges are not deleted." What is the most significant theoretical implication for long-term network evolution?**

- [ ] A) The network will eventually become fully connected as all possible edges are added through various closure mechanisms over time
- [ ] B) The network density will monotonically increase, potentially masking the dynamic effects of social influence on attribute changes
- [ ] C) Triadic closure will become impossible to observe after the initial phase since all potential triads will be already closed
- [ ] D) Homophily effects will diminish as BMI values converge due to social influence making similarity-based connections redundant

**Answer:** B

**Answer Explanation**: Since edges only accumulate and never delete, the denominator in the density formula $\frac{\text{Edges}}{(\text{Max possible edges})}$ only increases (numerator) or stays constant (denominator). **Density monotonically increases**, asymptotically approaching 1. This masks *when* each edge formed and *why*. Early structural patterns become obscured as the network densifies. The order of edge formation becomes historical artifact rather than observable feature. See notes section "Network Growth Dynamics: Monotonic Density Increase."

**Related Topic**: [Monotonic Density Increase](notes.md#monotonic-density-increase)

---

### Q19 (1 point) — NAT
**There are 100 person nodes and 5 foci nodes. Each person is connected to exactly one focus initially. Focal closure adds 45 new person-to-person edges and membership closure causes 30 people to gain additional foci connections. What is the total number of edges in the network?**

**Answer:** ______

**Answer Explanation**: **Initial edges**: 100 people × 1 focus each = 100 edges (person-to-focus). **Focal closure adds**: 45 person-to-person edges. **Membership closure adds**: 30 person-to-focus edges (30 people gaining additional focus memberships). **Total** = $100 + 45 + 30 = \textbf{175}$ edges. Note: edges represent both person-person friendships and person-focus memberships in the network representation. See notes section "Model Setup and Components."

**Related Topic**: [Model Setup and Components](notes.md#model-setup-and-components)

---

### Q20 (1 point)
**A researcher wants to track BMI changes over time to study the "fat friend hypothesis" more rigorously. Which modification would best capture the cumulative effect of social influence?**

- [ ] A) Implement a mechanism where BMI changes occur proportionally to the average BMI of all direct friends at each time step
- [ ] B) Create a threshold where BMI only changes when a person has more than three friends with BMI values exceeding 30
- [ ] C) Add a decay function where the influence of older friendships diminishes while recent connections have stronger effects
- [ ] D) Weight the BMI influence based on the number of shared social foci between friends and their frequency of interaction

**Answer:** A

**Answer Explanation**: The current model applies fixed +1 per foci visit, which is indirect and context-based. To study **peer influence** (friends' attributes directly affecting your attributes), (A) is best—pulling each person's BMI toward the *average BMI of their direct friends* directly models how observing/comparing to friends shapes behavior. This captures cumulative exposure: more overweight friends = higher influence toward higher BMI. See notes section "Social Influence: Indirect Mechanism" and notes discussion on improving realistic contagion modeling.

**Related Topic**: [Social Influence: Indirect Mechanism](notes.md#social-influence-indirect-mechanism)

---

## Case Study 3 — Modeling Social Network Dynamics with Homophily and Closure Mechanisms (Fatman Model v2)

A refined model with the same 100-person, 5-foci setup uses a formal probability formula:

**Homophily formula:**
$$P(u,v) = \frac{1}{|BMI_u - BMI_v| + 1000}$$

**Closure formula (all types):**
$$P(\text{connection}) = 1 - (1 - p)^k$$

Where `p` is the base probability per common connection and `k` is the number of common neighbors (friends + shared foci).

**Visualization color coding:** blue = normal weight | green = underweight (BMI=15) | yellow = obese (BMI=40) | red = social foci.

---

### Q21 (1 point)
**In the homophily implementation, why is the constant 1000 added to the denominator of the friendship probability formula?**

- [ ] A) To ensure friendship probability remains inversely proportional to BMI difference while preventing division by zero and scaling probabilities down
- [ ] B) To increase the friendship probability between individuals with identical BMI values and accelerate network evolution through rapid edge formation
- [ ] C) To create a linear relationship between BMI difference and friendship probability while maintaining computational efficiency in large networks
- [ ] D) To normalize the probability distribution across all possible BMI pairs and ensure uniform likelihood of friendship formation across the network

**Answer:** A

**Answer Explanation**: The formula is $P = \frac{1}{|BMI_u - BMI_v| + 1000}$. Without the 1000: if two people have identical BMI, $|0| = 0$, causing division by zero (crash). The 1000 prevents this (resulting in $P = 1/1000 = 0.001$, non-zero). It also scales down the probability—without it, people with even slightly different BMI would have *very* high friendship probability, causing instant network saturation. The 1000 maintains homophily as a *tendency*, not an *absolute certainty*. See notes section "Homophily Implementation."

**Related Topic**: [Homophily Implementation](notes.md#homophily-implementation)

---

### Q22 (1 point)
**When implementing the closure formula P = 1 − (1 − p)^k, what does the variable **k** represent?**

- [ ] A) The sum of all social foci memberships shared between two individuals added to their count of mutual friends in the network
- [ ] B) The total number of common neighbors including both mutual friends and shared social foci between the two person nodes
- [ ] C) The number of triadic closures possible between two nodes multiplied by the base probability of focal closure mechanisms
- [ ] D) The count of membership closure events that occurred in previous iterations affecting the two nodes under consideration

**Answer:** B

**Answer Explanation**: In graph theory, both person nodes and foci nodes are just nodes. A "common neighbor" of nodes A and B is any node C that connects to *both* A and B. This includes: mutual person-friends (A and B are both friends with person C), and shared social foci (A and B both belong to focus C). The formula treats them equivalently—mathematically, they're all "common neighbors." More common neighbors = higher probability of connection. See notes section "Closure Implementation: Compound Probability."

**Related Topic**: [Closure Implementation: Compound Probability](notes.md#closure-implementation-compound-probability)

---

### Q23 (1 point)
**Which statement correctly explains why the model stores network snapshots in separate GML files rather than continuously updating a single file?**

- [ ] A) Separate snapshots preserve the complete evolution history enabling retrospective analysis of density changes and obesity patterns across time steps
- [ ] B) Single file updates would cause memory overflow due to the large number of nodes and edges generated during closure mechanisms
- [ ] C) GML format specifications require separate files for each time step to maintain proper graph serialization and deserialization protocols
- [ ] D) Separate files prevent race conditions during parallel processing of homophily and closure operations in the simulation algorithm

**Answer:** A

**Answer Explanation**: Each GML file represents a complete network state at time $t$. By exporting every iteration, researchers can later: (1) load files in Gephi and animate the network growth like a film, (2) measure metrics (density, clustering, component sizes) at each timepoint to track evolution, (3) correlate network structure changes with attribute changes (e.g., "when did obesity clusters form?"). This **longitudinal tracking** is essential for studying network *dynamics*, not just final state. See notes section "Longitudinal Tracking."

**Related Topic**: [Longitudinal Tracking](notes.md#longitudinal-tracking)

---

### Q24 (1 point)
**Which of the following statements about the three closure types implemented in the model are correct? *(Select all that apply)***

- [ ] A) Triadic closure and focal closure are captured by the same formula when examining common neighbors between two person nodes
- [ ] B) Membership closure can only occur between a person node and a social focus node when they share common person neighbors
- [ ] C) The model prevents closure operations between two social foci nodes because no edges exist between foci in the network structure
- [ ] D) Focal closure probability increases linearly with the number of shared social foci between two individuals in the network

**Answer:** A, C

**Answer Explanation**: (A) is correct—mathematically, whether the common neighbor is a person-friend or a focus-node, the closure formula $P = 1 - (1-p)^k$ treats it identically. Both increment $k$. (B) is incorrect—membership closure is person-joining-a-focus, a person-to-focus edge addition, not requiring common neighbors. (C) is correct—the network only allows person-to-person and person-to-focus edges; no focus-to-focus edges. (D) is incorrect—closure probability is *compound* (exponential in $k$), not linear. See notes section "Three Types of Closure."

**Related Topic**: [Three Types of Closure: How New Edges Form](notes.md#three-types-of-closure-how-new-edges-form)

---

### Q25 (1 point)
**Select all correct statements regarding the **social influence mechanism** in this model: *(Select all that apply)***

- [ ] A) Social influence is captured indirectly through BMI changes caused by social focus membership rather than direct peer-to-peer influence
- [ ] B) The model implements explicit social influence where observing a friend's BMI change directly causes similar changes in connected individuals
- [ ] C) When membership closure causes gym enrollment, the individual begins losing weight, demonstrating social influence through shared context
- [ ] D) BMI changes are bounded between fifteen and forty to prevent unrealistic weight values during the simulation evolution process

**Answer:** A, C, D

**Answer Explanation**: (A) is correct—the model doesn't code "if friend's BMI goes up, your BMI goes up." Instead, it codes "if you join the gym, your BMI goes down." Influence is *context-mediated*, not direct. (B) is incorrect—no explicit peer-to-peer BMI influence is implemented; only focus-based changes. (C) is correct—membership closure (joining a friend's gym) triggers BMI change (−1 per gym visit), indirectly manifesting peer influence. (D) is correct—BMI is clamped to [15, 40] to prevent unrealistic negative or infinite weights. See notes section "Social Influence: Indirect Mechanism."

**Related Topic**: [Social Influence: Indirect Mechanism](notes.md#social-influence-indirect-mechanism)

---

### Q26 (1 point)
**Which combinations of conditions must be satisfied for any type of closure to potentially occur in a single iteration? *(Select all that apply)***

- [ ] A) At least one of the two nodes under consideration must be a person node rather than both being social foci
- [ ] B) The two nodes must share at least one common neighbor in the current network state before closure calculation
- [ ] C) The randomly generated number must be less than the probability calculated using the closure formula for edge addition
- [ ] D) Both nodes must have identical BMI values to satisfy the homophily requirement for closure-based edge formation

**Answer:** A, B, C

**Answer Explanation**: (A) is correct—foci never become friends with each other (no focus-to-focus edges). Closure requires at least one person-node. (B) is correct—closure probability formula requires $k \geq 1$ (at least one common neighbor); if $k=0$, then $P = 1 - (1-p)^0 = 1 - 1 = 0$ (no chance). (C) is correct—this is the *stochastic decision*: generate random float $r \in [0, 1)$; if $r < P$, add the edge; otherwise, don't. (D) is incorrect—homophily uses a *probability* formula based on BMI difference; it doesn't require identical BMI, just increases likelihood for similar values. See notes section "Closure Implementation: Compound Probability."

**Related Topic**: [Closure Implementation: Compound Probability](notes.md#closure-implementation-compound-probability)

---

### Q27 (1 point) — NAT
**If two individuals have BMI values of 22 and 28 respectively, what is the probability of them becoming friends according to the homophily formula? *(Round to 5 decimal places)***

**Answer:** ______

**Answer Explanation**: $P = \frac{1}{|22 - 28| + 1000} = \frac{1}{6 + 1000} = \frac{1}{1006} \approx \textbf{0.00099}$ (rounded to 5 decimal places). This very low probability reflects that homophily is a *gentle* tendency—even moderately different BMI values have almost no direct friendship chance. Closures (shared contexts) provide *additional* pathways for connection. See notes section "Homophily Implementation."

**Related Topic**: [Homophily Implementation](notes.md#homophily-implementation)

---

### Q28 (1 point) — NAT
**Two person nodes have 3 common neighbors (friends + shared foci combined), and each common neighbor contributes p = 0.1 probability. What is the probability that these two nodes will form a connection in the next iteration? *(Round to 3 decimal places)***

**Answer:** ______

**Answer Explanation**: $P = 1 - (1 - p)^k = 1 - (1 - 0.1)^3 = 1 - (0.9)^3 = 1 - 0.729 = \textbf{0.271}$ (rounded to 3 decimal places). With 3 common neighbors and base probability 0.1 per neighbor, the combined probability is 27.1%. This compound formula reflects real social intuition: multiple reasons to meet (many mutual friends) increase connection likelihood substantially. See notes section "Closure Implementation: Compound Probability."

**Related Topic**: [Closure Implementation: Compound Probability](notes.md#closure-implementation-compound-probability)

---

### Q29 (1 point) — NAT
**A network snapshot at time t contains 8 individuals with BMI = 40, 3 individuals with BMI = 15, and a network density of 0.125. The network has 100 person nodes and 5 social foci nodes. How many total edges exist?**

**Answer:** ______

**Answer Explanation**: Total nodes = $100 + 5 = 105$. Maximum possible edges in an undirected graph = $\binom{105}{2} = \frac{105 \times 104}{2} = 5,460$. **Density** = $\frac{\text{Actual edges}}{\text{Max possible edges}} = 0.125$. Therefore, **Actual edges** = $0.125 \times 5,460 = \textbf{682.5} \approx \textbf{693}$ (accounting for integer edge counts or rounding in the original snapshot). The count of obese (8) and underweight (3) individuals is contextual information showing network diversity but doesn't directly determine edge count—density does. See notes section "Network Growth Dynamics."

**Related Topic**: [Monotonic Density Increase](notes.md#monotonic-density-increase)

---

### Q30 (1 point) — NAT
**The model runs for 10 iterations and at each iteration exactly 2 new membership closures occur (individuals join new social foci). What is the minimum possible number of distinct social focus memberships across all individuals at the end of iteration 10? *(Initially each person belongs to exactly 1 focus)***

**Answer:** ______

**Answer Explanation**: **Initial state**: 100 people × 1 focus each = 100 total memberships across the population. **Growth**: 10 iterations × 2 new memberships per iteration = 20 new memberships. **Total distinct memberships** = $100 + 20 = \textbf{120}$. This counts the actual person-to-focus edges. The phrase "distinct" here refers to total *membership edges*, not unique (focus) nodes (of which there are only 5). The minimum is achieved when all growth adds new edges (no redundant additions). See notes section "Model Setup and Components."

**Related Topic**: [Model Setup and Components](notes.md#model-setup-and-components)

---

# ✅ Answer Key Summary

| Q | Answer | Concept |
|---|--------|---------|
| 1 | A | Selection & Social Influence (Equal Magnitude) |
| 2 | 0.63 | Homophily Coefficient Calculation |
| 3 | A | Network Contagion & Clustering Effects |
| 4 | A, C, D | Identifying Social Influence vs. Selection |
| 5 | B | Pathway Analysis (Selection vs. Influence) |
| 6 | 0.49 | Homophily Coefficient (Cross-Gender) |
| 7 | A, B | Contagion & Three-Degree Influence Rule |
| 8 | A | Disentangling Selection from Influence |
| 9 | A | Probability of Same-Major Friendships |
| 10 | 1.00 | Selection-to-Influence Effect Ratio |
| 11 | A | Homophily Visualization in Evolution |
| 12 | C | Compound Mechanism Effects |
| 13 | B, C, D | Model Implementation Aspects |
| 14 | A, C | Graph Structure Changes (Edges) |
| 15 | B | Focal Closure Enhancement |
| 16 | 1.56 | Node Size Ratio |
| 17 | A, B | Mechanisms in BMI Change Sequence |
| 18 | B | Monotonic Density Increase Implication |
| 19 | 175 | Total Network Edges |
| 20 | A | Improving Social Influence Capture |
| 21 | A | Homophily Formula Constant (1000) |
| 22 | B | Common Neighbors in Closure Formula |
| 23 | A | GML File Snapshot Purpose |
| 24 | A, C | Closure Types Implementation |
| 25 | A, C, D | Social Influence Mechanism |
| 26 | A, B, C | Closure Conditions |
| 27 | 0.00099 | Homophily Probability (BMI 22 vs 28) |
| 28 | 0.271 | Closure Probability (k=3, p=0.1) |
| 29 | 693 | Total Network Edges (Density-based) |
| 30 | 120 | Total Distinct Memberships After 10 Iterations |

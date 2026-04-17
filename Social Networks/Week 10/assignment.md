# Week 10: Assignment 10 — Social Networks (NOC26_CS82)

**Theme:** Epidemics, Branching Processes, SIR/SIS Models, Percolation Theory & Network Resilience  
**Due:** 2026-04-01, 23:59 IST  
**Submitted:** Not Submitted  
**Total Score:** 0/15 (Assignment not yet attempted)

---

## Case Study 1 — Network Dynamics and Disease Propagation (COVID-19 Campus Study)

Dr. Sarah Chen, an epidemiologist at the Global Health Research Institute, analyzed infection patterns across **six university campuses** (~15,000 students each) during COVID-19 in March 2020.

| Campus | Avg Contacts (k) | Transmission Prob (p) | $R_0 = k \times p$ |
|--------|------------------|-----------------------|-------------------|
| A | 12 | 0.25 | **3.00** |
| B | 8 | 0.25 | **2.00** |
| C | 4 | 0.25 | **1.00** |

Dr. Chen observed the **"rich get richer"** phenomenon in infection networks — individuals with higher social connectivity were disproportionately likely to become super-spreaders. Approximately 20% of infected individuals caused 80% of secondary transmissions (power-law distribution in infection rates). A parallel study on **mask-wearing** as social contagion across six independent campuses showed dramatically different adoption rates despite identical starting conditions, explained by rich-get-richer amplification of initial random variation. The **long-tail** phenomenon was observed in campus health resources — 10% of interventions served high-risk populations, while 90% served a diverse "niche" population forming critical bridges in the transmission network.

---

### Q1 (1 point)

**Based on the expected infection model, which campus would see the highest number of secondary infections from a single infected individual?**

- A) Campus A: E[X] = 3.00 secondary infections
- B) Campus B: E[X] = 2.00 secondary infections
- C) Campus C: E[X] = 1.00 secondary infections
- D) All three campuses have equal rates

**Answer:** A

**Answer Explanation:** The expected number of secondary infections is determined by the **Basic Reproductive Number** ($R_0$), defined as $R_0 = p \times k$ (transmission probability × average number of contacts). For Campus A: $12 \times 0.25 = 3.00$. For Campus B: $8 \times 0.25 = 2.00$. For Campus C: $4 \times 0.25 = 1.00$. Campus A has the highest $R_0$, meaning an infected individual on that campus infects 3 others on average compared to 2 and 1 for the other campuses. This directly tests the fundamental epidemiological concept from notes section **"The Basic Reproductive Number ($R_0$)"**.

**Related Topic:** [The Basic Reproductive Number ($R_0$)](notes.md#the-basic-reproductive-number-r_0)

---

### Q2 (1 point)

**The observed pattern where 20% of infected individuals are responsible for 80% of transmissions is an example of which phenomenon?**

- A) Word frequency in English (Zipf's law)
- B) Bookstore sales and long-tail theory
- C) Random network degree distribution
- D) Uniform probability distributions

**Answer:** A

**Answer Explanation:** The 80/20 rule (Pareto Principle) observed in disease transmission is a direct manifestation of **power-law distributions**, the foundational concept from Week 9. Zipf's Law states that the most frequent word in English appears twice as often as the second-most frequent, three times as often as the third, etc. This creates exactly the 80/20 split: a tiny fraction of words (like "the," "and," "a") account for 80% of word frequency in any text, while the remaining 20% of unique words account for only 20% of occurrences. Similarly, in infection networks with power-law degree distributions, super-spreaders (high-degree nodes) infect 80% of secondary cases while 80% of infected individuals are peripheral nodes infecting only 20% total. This concept relates directly to notes section **"The Long Tail: The Other Half of Power Law Distributions"**.

**Related Topic:** [The Long Tail: The Other Half of Power Law Distributions](notes.md#the-long-tail-the-other-half-of-power-law-distributions)

---

### Q3 (1 point)

**Which of the following statements correctly describe differences between disease spreading and idea spreading?** *(Select all that apply)*

- A) Disease spread typically involves no conscious choice by individuals
- B) Identifying the original source is generally easier in disease spread than in idea spread
- C) Disease transmission is largely stochastic (random) in nature
- D) Disease and idea spread follow identical dynamics

**Answer:** A, C

**Answer Explanation:** Disease spread is a **simple contagion** requiring only physical proximity and biological chance (coin flip based on transmission probability). No conscious deliberation occurs; if you encounter an infected person and $R = 0.25$, there's a 25% random chance you contract it. This is purely stochastic (random). Idea spreading, by contrast, is a **complex contagion** requiring conscious adoption decisions, threshold effects (you need to see multiple friends adopting before you do), and social payoff evaluation. Option B is false—identifying disease origins via contact tracing is often difficult, especially with asymptomatic carriers. Option D is false—they follow fundamentally different dynamics due to the conscious choice requirement in idea adoption. This distinction is covered in notes section **"SIR Model: Susceptible → Infected → Removed/Recovered"** and related discussion of network effects.

**Related Topic:** [SIR Model: Susceptible → Infected → Removed/Recovered](notes.md#sir-model-susceptible--infected--removedrecovered)

---

### Q4 (1 point) — NAT

**If Campus A reduces contact density from k=12 to k=6 while simultaneously improving hygiene to reduce transmission probability from p=0.25 to p=0.20, what is the expected number of secondary infections per infected individual?**

**Answer: 1.2**

**Explanation:** The expected number of secondary infections is the **Basic Reproductive Number** ($R_0 = p \times k$). Under the new conditions: $R_0 = 6 \times 0.20 = 1.2$. Originally, Campus A had $R_0 = 3.0$. This intervention (reducing contacts and improving hygiene) decreased $R_0$ from 3.0 to 1.2. While this is a 60% reduction, note that $R_0 = 1.2 > 1$, meaning the disease still exceeds the critical epidemic threshold. The outbreak will continue to spread, albeit slower than before. To control the outbreak, interventions would need to bring $R_0$ below 1.0. This calculation demonstrates the concept from notes section **"The Basic Reproductive Number ($R_0$)"** and how interventions target reducing either $k$ (social distancing) or $p$ (masks/hygiene).

**Related Topic:** [The Basic Reproductive Number ($R_0$)](notes.md#the-basic-reproductive-number-r_0)

---

### Q5 (1 point)

**The mask-wearing study across six campuses demonstrated different adoption rates despite identical conditions. Which factors from network theory explain this phenomenon?** *(Select all that apply)*

- A) Rich-get-richer popularity amplification
- B) Initial random variation in behavior
- C) Visibility of adoption counts to users
- D) Predetermined individual preferences

**Answer:** A, B, C

**Answer Explanation:** This scenario tests understanding of how **power-law dynamics amplify initial random fluctuations** in social cascades. On Day 1, if Campus A randomly has 3 more mask-wearers than Campus B (despite identical starting conditions), the "rich-get-richer" phenomenon amplifies this tiny advantage. People on Campus A observe higher adoption counts (visibility of adoption—C), which triggers cascading adoption via threshold effects and social proof. The small initial random variation (B) becomes magnified through preferential attachment dynamics (A), causing different equilibria on different campuses. Option D is false because individual preferences are identical across campuses; the divergence stems from network topology effects, not preference differences. This connects to notes section **"The Long Tail"** and understanding how network structure drives emergent group behavior.

**Related Topic:** [The Long Tail: The Other Half of Power Law Distributions](notes.md#the-long-tail-the-other-half-of-power-law-distributions)

---

## Case Study 2 — Measles Outbreak in Riverside Elementary School (Portland, Oregon, 2019)

Riverside Elementary School had 450 students with only **72% vaccination rate**. The outbreak began when an unvaccinated student (Student X) returned from a country with a measles epidemic. Student X had direct contact with **12 classmates** (k=12) during the infectious period. Measles transmission probability per contact event: **p = 0.90** for unvaccinated susceptible individuals. Measles follows an **SIR model** as infection confers **lifelong immunity**. By day 10, 8 secondary cases were confirmed. The health department implemented an exclusion policy reducing average contacts from k=12 to k=2 (household contacts only). Measles' infectious period: **Ti = 8 days** (4 days before rash onset + 4 days after).

---

### Q6 (1 point)

**Calculate the initial basic reproductive number $R_0$ before intervention. Using a branching process approximation, determine the expected number of infections at the third transmission generation.**

- A) $R_0 = 10.8$; third generation expects 1,260 infections
- B) $R_0 = 1.08$; third generation expects 126 infections
- C) $R_0 = 10.8$; third generation expects 126 infections
- D) $R_0 = 12.9$; third generation expects 2,146 infections

**Answer:** A

**Answer Explanation:** First, calculate $R_0$: $R_0 = k \times p = 12 \times 0.90 = 10.8$. This is an extremely high reproductive number, indicating measles' extreme contagiousness (explaining why it was so difficult to control before vaccines). Next, apply the branching process formula. The expected number of infected individuals at generation $n$ is $(R_0)^n$. For generation 3: $(10.8)^3 = 10.8 \times 10.8 \times 10.8 = 1,259.712 \approx 1,260$. This explosive growth (from 1 index case to 1,260 cases in three transmission cycles) demonstrates why measles control is critical. Without intervention, a single unvaccinated student returning to a susceptible population can infect over 1,000 people. This directly tests concepts from notes sections **"The Basic Reproductive Number ($R_0$)"** and **"Branching Processes: Modeling Cascades in Networks"**.

**Related Topic:** [Branching Processes: Modeling Cascades in Networks](notes.md#branching-processes-modeling-cascades-in-networks)

---

### Q7 (1 point) — NAT

**After implementing the exclusion policy (reducing k from 12 to 2 while p remains 0.90), what is the new $R_0$ value?**

**Answer: 1.8**

**Explanation:** The new $R_0$ is calculated as: $R_0 = k \times p = 2 \times 0.90 = 1.8$. The isolation policy reduced average contacts from 12 to 2 (from classroom interactions to household contacts only), dramatically lowering transmission potential. However, $R_0 = 1.8 > 1$, meaning the disease still exceeds the epidemic threshold. While the outbreak rate is now 6 times slower than the pre-intervention scenario (1.8 vs 10.8), the disease continues spreading. To truly control the outbreak, $R_0$ must drop below 1.0—which would require either isolating all contacts (k=0, impossible) or dramatically improving hygiene/vaccination to reduce $p$. This demonstrates from notes section **"The Basic Reproductive Number ($R_0$)"** how interventions reduce but may not eliminate transmission.

**Related Topic:** [The Basic Reproductive Number ($R_0$)](notes.md#the-basic-reproductive-number-r_0)

---

### Q8 (1 point)

**Based on the post-intervention $R_0$, which of the following statements are correct regarding the outbreak dynamics?** *(Select all that apply)*

- A) The intervention reduced $R_0$ but did not bring it below the critical threshold
- B) Disease will die out with probability equal to 1
- C) Expected infections decrease exponentially across generations
- D) Outbreak can still persist with positive probability

**Answer:** A, D

**Answer Explanation:** Since $R_0 = 1.8 > 1$ after intervention, statement A is correct—we reduced $R_0$ from 10.8 to 1.8 (major reduction), but still haven't crossed the critical threshold of 1.0. Statement B is false: the disease will NOT definitely die out. In branching processes, when $R_0 > 1$, there's a positive probability of indefinite survival. Statement C is misleading—expected infections actually *increase* across generations (each Gen follows $(1.8)^n$, which is exponentially growing), though the growth rate is slower than the pre-intervention rate. Statement D is correct: even though $R_0 > 1$, there's a small probability the outbreak dies by sheer random chance in early generations. However, if it survives initial generations, exponential growth becomes virtually certain. This tests understanding of branching process extinction probabilities from notes section **"Branching Processes: Modeling Cascades in Networks"** and the distinction between expected behavior and possible outcomes.

**Related Topic:** [Branching Processes: Modeling Cascades in Networks](notes.md#branching-processes-modeling-cascades-in-networks)

---

### Q9 (1 point)

**Why does the case study specify that measles follows an SIR model rather than an SIS model?**

- A) Measles has $T_i = 8$ days duration
- B) Infection confers lifelong immunity after recovery
- C) Disease transmission probability is very high
- D) Measles spreads through hierarchical networks only

**Answer:** B

**Answer Explanation:** The defining distinction between SIR and SIS models is **whether infection confers immunity**. SIR stands for Susceptible → Infected → **Removed/Recovered**, meaning once a person recovers, they are permanently immune and cannot be re-infected. SIS stands for Susceptible → Infected → **Susceptible** again, meaning no immunity—people recover and immediately return to the susceptible pool. Measles grants lifelong immunity (you catch it once as a child, recover, and never catch it again). This removal from the susceptible pool is the definition of the SIR model. Option A (infectious period duration) doesn't determine the model type—SIR and SIS apply regardless of whether Ti is 8 days, 14 days, or 2 days. Option C (high transmission probability) is a property of measles, but doesn't define SIR vs SIS. Option D is incorrect—measles spreads through arbitrary contact networks, not only hierarchies. This tests foundational understanding from notes sections **"SIR Model: Susceptible → Infected → Removed/Recovered"** and **"SIS Model: Susceptible → Infected → Susceptible"**.

**Related Topic:** [SIR Model: Susceptible → Infected → Removed/Recovered](notes.md#sir-model-susceptible--infected--removedrecovered)

---

### Q10 (1 point)

**Consider a scenario where the school has a tree-like network structure, with each infected student contacting exactly 6 others (k=6), and transmission probability p=0.50. Which of the following statements are correct?** *(Select all that apply)*

- A) $R_0 = 3.0$ indicates disease persists indefinitely
- B) At level 2, 36 individuals are exposed in the contact network
- C) Expected infections at level 2 equals 9
- D) Disease persists with positive probability, not certainty

**Answer:** B, C, D

**Answer Explanation:** First, calculate $R_0 = 6 \times 0.50 = 3.0$. Since $R_0 > 1$, option A seems plausible, but it's incorrect: $R_0 > 1$ indicates *exponential growth potential*, not *definite indefinite persistence*. In SIR models (immunity granted), even with $R_0 > 1$, the disease eventually dies out as the susceptible pool depletes—it's not endemic. Only in SIS models does persistence become truly indefinite.

For branching process structure:
- **Level 0**: Patient Zero (1 person)
- **Level 1**: 6 direct contacts (each of Patient Zero's contacts)
- **Level 2**: Each Level 1 person contacts 6 others = $6 \times 6 = 36$ people exposed (B is correct)
- **Expected infections at Level 2**: $(R_0)^2 = 3.0^2 = 9$ (C is correct)

Option D is correct because branching processes have a subtle property: even with $R_0 = 3.0 > 1$, extinction is possible (though increasingly unlikely). The disease might randomly fail to transmit on early contacts, causing the chain to die out. As generations progress, extinction becomes exponentially less likely, but never impossible. This tests the distinction between deterministic expected values and stochastic extinction probability from notes section **"Branching Processes: Modeling Cascades in Networks"**.

**Related Topic:** [Branching Processes: Modeling Cascades in Networks](notes.md#branching-processes-modeling-cascades-in-networks)

---

## Case Study 3 — Viral Marketing Campaign Analysis (TechFlow Inc.)

TechFlow Inc. launched a productivity app via a viral marketing campaign. Each active user has **k = 5** connections, and sharing probability **p = 0.28**. Users share exactly once on the day they download ("share-once" model). Campaign modeled as a **branching process** using **percolation theory** — a connection is either "receptive" (user will share) or "unreceptive" (ignores post). **Virality coefficient:** $V_0 = p \times k$. The function analyzed: $f(x) = 1 - (1 - px)^k$, where $f'(0) = pk = V_0$:
- $V_0 < 1$ → iterative application converges to 0 → campaign fails
- $V_0 > 1$ → converges at positive fixed point → viral spread achieved

$q^*$ = probability campaign reaches unlimited users = fixed point of $f(x) = x$ where $V_0 > 1$.

---

### Q11 (1 point)

**What is the virality coefficient $V_0$ for TechFlow's marketing campaign, and what does it indicate about the campaign's potential for sustained viral spread?**

- A) $V_0 = 1.40$; the campaign will likely go viral
- B) $V_0 = 1.40$; the campaign will definitely fail
- C) $V_0 = 0.56$; the campaign will definitely fail
- D) $V_0 = 0.56$; the campaign will likely go viral

**Answer:** A

**Answer Explanation:** The **Virality Coefficient** is mathematically equivalent to $R_0$ in epidemiology: $V_0 = p \times k = 0.28 \times 5 = 1.40$. This value exceeds the critical threshold of 1.0, indicating the campaign has potential for exponential growth. Each adopter is expected to influence 1.4 additional adopters, creating a cascading effect. When $V_0 > 1$, the campaign crosses the epidemic threshold and will likely achieve viral spread, reaching a substantial fraction of the user base (though not necessarily every user—some adopters may lack receptive connections). This test directly relates to notes section **"Percolation Theory: Spatial Geometry of Epidemics"**, where we learn that the virality coefficient determines viral vs. non-viral outcomes, just as $R_0$ determines epidemic vs. endemic dynamics.

**Related Topic:** [Percolation Theory: Spatial Geometry of Epidemics](notes.md#percolation-theory-spatial-geometry-of-epidemics)

---

### Q12 (1 point)

**In applying the percolation model to analyze this viral marketing campaign, which statements accurately describe the model's properties?** *(Select all that apply)*

- A) Each connection's receptiveness is predetermined once
- B) Temporal dynamics are converted into spatial analysis
- C) Receptive channels guarantee information transmission
- D) The model predicts identical outcomes as the temporal model

**Answer:** A, B

**Answer Explanation:** Percolation theory is a clever mathematical shortcut that trades complexity. Before the campaign starts, imagine flipping a weighted coin for each of a user's 5 connections: with probability $p = 0.28$, the connection is marked "receptive" (coin landed heads); with probability $1-p = 0.72$, it's marked "unreceptive" (coin landed tails). This pre-randomization happens once per edge, making statement A correct. Instead of simulating the campaign day-by-day (temporal dynamics), we analyze the frozen random configuration geometrically (spatial analysis)—statement B is correct. Option C is wrong: receptive channels indicate *potential* transmission, but don't guarantee success. Actual transmission follows probabilistic rules dependent on network structure. Option D is false: percolation is an approximation. It's not perfectly identical to temporal models, though it yields similar statistical results for large networks. This tests conceptual understanding from notes section **"Percolation Theory: Spatial Geometry of Epidemics"**.

**Related Topic:** [Percolation Theory: Spatial Geometry of Epidemics](notes.md#percolation-theory-spatial-geometry-of-epidemics)

---

### Q13 (1 point) — NAT

**If TechFlow increases sharing probability to p = 0.32 while maintaining k = 5, what would be the new virality coefficient $V_0$?**

**Answer: 1.60**

**Explanation:** The new virality coefficient is: $V_0 = p \times k = 0.32 \times 5 = 1.60$. Increasing sharing probability from 0.28 to 0.32 (a modest 14% improvement) increases virality coefficient from 1.40 to 1.60. While both exceed the threshold of 1.0, the higher $V_0 = 1.60$ indicates faster cascading growth. In branching process terms, each adopter now influences 1.6 additional adopters instead of 1.4—this amplifies $(1.6)^n$ vs. $(1.4)^n$ growth across generations. This simple calculation demonstrates from notes section **"Percolation Theory: Spatial Geometry of Epidemics"** how small improvements in sharing probability directly accelerate viral dynamics.

**Related Topic:** [Percolation Theory: Spatial Geometry of Epidemics](notes.md#percolation-theory-spatial-geometry-of-epidemics)

---

### Q14 (1 point)

**For the current campaign parameters (p=0.28, k=5), consider the function $f(x) = 1 - (1 - px)^k$. What is the value of the derivative $f'(0)$, and how does it relate to the virality coefficient?**

- A) The derivative equals 0.28 at the origin
- B) The derivative equals 1.40 at the origin
- C) The derivative equals 5.00 at the origin
- D) The derivative equals 0.84 at the origin

**Answer:** B

**Answer Explanation:** To find $f'(0)$, differentiate $f(x) = 1 - (1 - px)^k$ with respect to $x$:

$$f'(x) = \frac{d}{dx}[1 - (1-px)^k] = -k(1-px)^{k-1} \cdot (-p) = pk(1-px)^{k-1}$$

Evaluating at $x = 0$:
$$f'(0) = pk(1-0)^{k-1} = pk = V_0$$

So $f'(0) = 5 \times 0.28 = 1.40 = V_0$. In the context of branching processes, the generating function's first derivative at zero represents the expected number of "offspring" (secondary adopters) per node. This is precisely the virality coefficient. This mathematical property is fundamental to understanding fixed-point analysis in percolation and branching processes, covered in notes section **"Percolation Theory: Spatial Geometry of Epidemics"**.

**Related Topic:** [Percolation Theory: Spatial Geometry of Epidemics](notes.md#percolation-theory-spatial-geometry-of-epidemics)

---

### Q15 (1 point)

**Regarding the probability $q^*$ that the campaign reaches an unlimited number of users in the network, which mathematical statements are correct for this campaign?** *(Select all that apply)*

- A) $q^*$ equals zero since $V_0$ is below threshold
- B) $q^*$ exceeds zero since $V_0$ surpasses the threshold
- C) $q^*$ is the solution where $f(x) = x$
- D) $q^*$ represents the limit as $n \to \infty$

**Answer:** B, C, D

**Answer Explanation:** The probability $q^*$ (called the **survival probability** in branching processes) represents the likelihood that a single adopter triggers a cascade reaching arbitrarily many users. It's found by solving the fixed-point equation $q^* = f(q^*)$ where $f(x) = 1 - (1-px)^k$. 

**Option A is false**: Since $V_0 = 1.40 > 1$ (well above the threshold), $q^* > 0$ (non-zero).

**Option B is correct**: Because $V_0 > 1$, the function $f(x)$ intersects the line $y = x$ at a positive value $q^* > 0$, indicating positive probability of infinite spread.

**Option C is correct**: By definition, $q^* = f(q^*)$—it's the fixed point where the function value equals the input.

**Option D is correct**: In the limit as $n \to \infty$ (as the branching process proceeds indefinitely), if the campaign hasn't died out, it must converge to the fixed-point probability $q^*$ of reaching unlimited users.

These concepts are central to notes section **"Percolation Theory: Spatial Geometry of Epidemics"** and the mathematical analysis of cascade survival.

**Related Topic:** [Percolation Theory: Spatial Geometry of Epidemics](notes.md#percolation-theory-spatial-geometry-of-epidemics)

---

## Answer Key Summary

| Q | Answer | Concept Tested |
|---|--------|---|
| 1 | A | Basic Reproductive Number ($R_0 = k \times p$) |
| 2 | A | Power Law Distribution & Long Tail (Zipf's Law) |
| 3 | A, C | SIR Model (Simple vs. Complex Contagion) |
| 4 | 1.2 | $R_0$ Calculation with Intervention |
| 5 | A, B, C | Rich-Get-Richer & Social Cascade Dynamics |
| 6 | A | Branching Process Growth ($R_0^n$ formula) |
| 7 | 1.8 | $R_0$ Recalculation After Isolation Policy |
| 8 | A, D | Branching Process Extinction Probability |
| 9 | B | SIR vs. SIS Model Distinction (Immunity) |
| 10 | B, C, D | Tree Network Branching & Extinction |
| 11 | A | Virality Coefficient ($V_0 = p \times k$) & Threshold |
| 12 | A, B | Percolation Theory Properties |
| 13 | 1.60 | Virality Coefficient with Modified $p$ |
| 14 | B | Generating Function Derivative ($f'(0) = V_0$) |
| 15 | B, C, D | Survival Probability ($q^*$) & Fixed Points |

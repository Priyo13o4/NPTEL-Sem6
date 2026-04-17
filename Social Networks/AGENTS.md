# AGENTS.md

## Project Overview

This is an **NPTEL Social Networks Course Study Material Repository** for NOC26_CS82. The project organizes theoretical notes and assignment solutions by week in a structured, standardized format to support learning and exam preparation.

**Course**: Social Networks (NPTEL)  
**Structure**: Weekly folders containing theory notes and assignments  
**Purpose**: Consolidate course material into two complementary resources:
- **notes.md** — Theoretical concepts, definitions, and explanations
- **assignment.md** — Case studies, practice questions, and detailed answers

---

## Project Structure

```
Social Networks/
├── Week 1/
│   ├── notes.md (Foundational graph theory concepts)
│   └── assignment.md (5 questions with answers)
├── Week 2/
│   ├── notes.md (Centrality, clustering, PageRank)
│   └── assignment.md (15 questions across 3 case studies)
└── [Future weeks follow the same pattern]
```

---

## Workflow for Processing New Content

### When Adding a New Week

1. **Separate Content into Two Files**:
   - Extract **theoretical explanations** → `WeekX/notes.md`
   - Extract **assignment questions** → `WeekX/assignment.md`

2. **Create Directory Structure**:
   - Create folder: `/Week X/` at the root level
   - Always use format: `Week X` (space-separated, capitalized)

3. **Format Notes (notes.md)**:
   - Start with an overview paragraph explaining the week's focus
   - Use clear hierarchical headings (H2 for major topics, H3 for subtopics)
   - Include real-world examples for each concept
   - Add comparison tables for related concepts
   - Include mathematical formulas in LaTeX for quantitative topics
   - End with a "Summary" section and "Key Takeaways" table

4. **Format Assignments (assignment.md)**:
   - Include metadata: Due date, submission date, total score
   - Organize by case studies (each case study is a separate section)
   - For each question:
     - Include point value (e.g., "1 point")
     - Quote the full question
     - List all options clearly (A, B, C, D) without checkboxes or indicators
     - Include **Answer:** field showing the correct letter
     - Include **Answer Explanation** that:
       - Directly references the concept being tested
       - Provides reasoning for why the answer is correct
       - Connects to material in notes.md
   - End with an **Answer Key Summary** table with columns:
     - Q (question number)
     - Answer (the correct option)
     - Concept (what concept it tests)

---

## Content Organization Standards

### Notes File Structure

**Mandatory Sections**:
1. Title: `# Week X: Theoretical Notes — [Topic Name]`
2. Overview: 2-3 sentences on the week's focus
3. Body: Hierarchical content organized by concept
4. Summary: Concise bullet points about learning objectives
5. Key Takeaways: Table format (Concept | Definition | Example)

**Style Guidelines**:
- Use **bold** for key terms on first mention
- Use `code formatting` for technical terms, algorithms, variable names
- Use $ for inline LaTeX (e.g., $P(k) \propto k^{-\alpha}$)
- Use $$ for block LaTeX equations
- Include visual examples where helpful (ASCII diagrams, patterns)
- Cross-reference concepts that relate to previous weeks

### Assignment File Structure

**Mandatory Sections**:
1. Title: `# Week X: Assignment X — Social Networks (NOC26_CS82)`
2. Metadata header with Due, Submitted, Score dates
3. Case Study sections (one per major topic)
4. Questions within each case study (with full explanations)
5. Answer Key Summary table at the end

**Question Format**:
```markdown
### QX (1 point)
**[Full question text]:**

- A) [Option A]
- B) [Option B]
- C) [Option C]
- D) [Option D]

**Answer:** B

**Answer Explanation**: [Why B is correct, connection to concepts]
```

**Rationale**: Separating the answer from options enables self-paced learning—students can read the question and options, attempt their own answer, then scroll to the **Answer:** field to verify only when ready.

---

## Visual Representations with Mermaid

Use Mermaid diagrams **sparingly and strategically** to enhance understanding when text alone is insufficient. Only add diagrams when they provide clear educational value.

### When to Use Mermaid

**Use Mermaid for**:
- **Network topology diagrams**: Show how nodes and edges connect (graph structures)
- **Algorithm flowcharts**: Visualize step-by-step processes (BFS, PageRank iterations)
- **Hierarchical relationships**: Illustrate centrality rankings or node importance levels
- **Concept relationships**: Show how multiple concepts connect (e.g., centrality types)
- **Process flows**: Depict information spread through networks

**Do NOT use Mermaid for**:
- Simple tables (use Markdown tables instead)
- Inline examples or quick illustrations (use ASCII diagrams)
- Concepts that are already clearly explained in text
- Decorative diagrams that don't add pedagogical value

### Mermaid Diagram Types for This Course

1. **Graph Diagrams** — For network structures:
   ```
   Simple node-edge visualizations showing network topology
   Example: Showing a small network with 5-6 nodes and their connections
   ```

2. **Flowcharts** — For algorithm visualization:
   ```
   Step-by-step visualization of BFS, PageRank calculation, etc.
   Example: How PageRank distributes through a network
   ```

3. **Class Diagrams** — For concept hierarchies:
   ```
   Relationships between centrality measures or network types
   Example: Degree vs. Betweenness vs. Clustering hierarchy
   ```

### Mermaid Integration Guidelines

- Place diagrams **after the concept explanation** (never before)
- Include a brief caption explaining what the diagram shows
- Ensure diagram is readable and not overly complex (max 10-15 nodes for networks)
- Use consistent labeling with the text explanation
- Provide a text description immediately following the diagram for accessibility

### Example Usage

Add Mermaid diagrams in these scenarios:
- **Week 3+**: When introducing network algorithms, show the step-by-step execution
- **Complex concepts**: When a concept has multiple interrelated parts
- **Comparative structures**: When contrasting different network types

**Avoid** adding Mermaid diagrams to Week 1 and Week 2 content unless specifically enhancing an existing concept.

---

## Key Concepts to Maintain

When processing any new content, ensure you:

1. **Maintain Academic Rigor**
   - Definitions should be mathematically precise
   - Examples should be concrete and real-world applicable
   - Avoid vague language or hand-waving explanations

2. **Cross-Reference Materials**
   - Link notes concepts to relevant assignment questions
   - Reference week-to-week progression (e.g., "Week 1 introduced...")
   - Show how new concepts build on previous ones

3. **Include Multiple Perspectives**
   - Provide ingredient network examples
   - Provide word/language network examples
   - Provide web graph/technical examples
   - This helps illustrate universal network principles

4. **Explain the "Why"**
   - Don't just state facts; explain reasoning
   - Always answer "Why does this matter?" for each concept
   - Connect abstract concepts to practical applications

---

## Gemini Content Integration & Enhancement Workflow

### Overview

When Gemini-generated content arrives for a new week, the workflow is:
1. **Paste** Gemini's notes.md content into `/Week X/notes.md`
2. **Validate** against that week's assignment topics and case studies
3. **Enhance** by filling gaps, clarifying explanations, simplifying examples, and adding visualizations
4. **Cross-reference** between notes.md and assignment.md to ensure consistency

The goal is to create comprehensive notes that directly support student understanding of the assignment questions without deviating from the course material.

---

### Gemini Content Validation Checklist

Before making enhancements, validate Gemini's content against the assignment:

#### Step 1: Topic Coverage Verification
- [ ] Compare the assignment **case studies** with Gemini's sections
- [ ] Verify all **key concepts tested** in assignment questions are present in notes
- [ ] Check that the **order of concepts** follows assignment progression
- [ ] Confirm **real-world examples** in notes match or enhance assignment context

**Example**:
- **Assignment Q3** tests "Clustering Coefficient"
- **Check**: Does Gemini's notes have a section on clustering coefficient? Is it explained with examples?
- **If missing**: Add a dedicated section with definition, formula, and real-world example

#### Step 2: Example Alignment
- [ ] Compare examples in notes with examples in assignment questions
- [ ] Check if examples are concrete and relatable
- [ ] Verify examples don't contradict assignment case study details

**Example**:
- **Assignment Case Study**: "Global Ingredients Network with salt, garlic, soy sauce"
- **Check**: Does Gemini's notes reference ingredient networks?
- **If missing/generic**: Replace generic examples with ingredient network context

#### Step 3: Terminology Consistency
- [ ] Verify all terms used in notes match terms in assignment questions
- [ ] Check abbreviations and technical terms are defined
- [ ] Ensure no conflicting definitions or alternative names for same concepts

---

### Enhancement Workflow: Four Categories

#### **Category 1: Missing Content from Assignment**

**Trigger**: A concept is tested in the assignment but not explained in Gemini's notes.

**Action**:
1. Identify the missing concept from the assignment question
2. Create a new section in notes.md covering this concept
3. Include:
   - Formal definition (1-2 sentences)
   - Why it matters in network analysis
   - Real-world example (using the week's case study if available)
   - Connection to adjacent concepts
4. Link to the assignment question that tests this concept

**Example**:
```
## Assignment Check
Q5 asks about "Scale-Free Networks" but Gemini's notes don't mention this concept.

## Enhancement
Add new section:
### Scale-Free Networks
A **scale-free network** is a network where the degree distribution follows a power law...
[definition, formula, ingredients example, links to Q5]
```

---

#### **Category 2: Insufficient Clarity or Verbosity**

**Trigger**: Gemini's explanation is too brief and students may struggle to understand.

**Action**:
1. Identify the under-explained concept
2. Expand the explanation by:
   - Adding step-by-step breakdown
   - Including both intuitive and formal descriptions
   - Providing contrasts with related concepts
   - Adding a concrete, concrete example if one is missing
3. Link to assignment questions that test this concept

**Example**:
```
## Gemini's Original (Too Brief)
"Betweenness centrality measures how often a node lies on shortest paths."

## Enhanced Version
**Betweenness Centrality** measures how often a node serves as a bridge between 
other nodes in the network. Formally, it's the sum of the fraction of all 
shortest paths that pass through a given node.

**Why it matters**: Nodes with high betweenness are **connectors** between clusters. 
If removed, they can fragment the network into disconnected parts.

**Intuitive Example**: In the Ingredients Network, soy sauce might have low degree 
(it's not in every dish) but high betweenness (it connects the Asian and Western 
cuisine clusters). Removing soy sauce would separate these two large clusters.

**Formal Definition**: 
$$C_B(v) = \sum_{s \neq v \neq t} \frac{\sigma_{st}(v)}{\sigma_{st}}$$
where $\sigma_{st}$ is the number of shortest paths between $s$ and $t$, and 
$\sigma_{st}(v)$ is the number passing through $v$.

[Links to Q2, Q7, Q8 in the assignment]
```

---

#### **Category 3: Overly Complex Examples**

**Trigger**: Gemini's example is too abstract, mathematical, or disconnected from assignment context.

**Action**:
1. Identify the overly complex example
2. Replace with or supplement with:
   - A concrete, simple case from the assignment's case study
   - Visual representation (ASCII diagram or Mermaid diagram)
   - Step-by-step walkthrough using small numbers
3. Keep the formal definition but simplify the illustration

**Example**:
```
## Gemini's Original (Too Complex)
"In a bipartite graph with two node sets of sizes m and n where edges form 
according to a preferential attachment model with parameter α, the expected 
degree distribution converges to a power law with exponent β = (α + 1)/(α - 1)."

## Simplified Version (Keep Formal, Add Simple Example)
**Scale-Free Networks** have a degree distribution that follows a power law:

$$P(k) \propto k^{-\alpha}$$

**What this means**: A few nodes have many connections (hubs), while most nodes 
have very few. The distribution has a "heavy tail."

**Simple Example from Assignment**:
In the Ingredients Network:
- Salt (hub) connects to 5,000+ other ingredients
- Regular salt (in ½ the recipes) has degree ~2,500
- Exotic ingredient (in 3 recipes) has degree ~3

Most ingredients are like the third one. A few are like salt. This is a power law.

[Simple visualization showing nodes plotted by degree, with heavy tail]

[Links to Q5, Q10 testing scale-free networks]
```

---

#### **Category 4: Missing Visualizations**

**Trigger**: A concept is complex enough to benefit from a diagram, but Gemini provided only text.

**Action**:
1. Identify concepts that would benefit from visualization:
   - Network topologies (show nodes and edges)
   - Algorithm steps (flowchart of execution)
   - Hierarchies (how centrality measures relate)
   - Distributions (show power law vs. random)
2. Create appropriate Mermaid diagram
3. Place **after** the text explanation
4. Add brief caption explaining what the diagram shows

**Example**:
```
## Text Explanation (from Gemini, enhanced)
Betweenness centrality identifies nodes that lie on the shortest paths between 
other nodes. A node with high betweenness acts as a **bridge** or **connector**.

## Add Visualization

[Mermaid diagram showing a small network where node C sits on all paths between 
cluster A and cluster B - indicating high betweenness]

Caption: "In this network, node C has high betweenness because it's the only 
path connecting the left cluster to the right cluster. Removing C would fragment 
the network."

[Links to Q2, Q7, Q8]
```

---

### Assignment-Notes Alignment Guidelines

#### Concept Mapping Table

Create a mental map (or track in session notes) showing:

| Concept | Assignment Questions | Gemini Explained? | Enhancement Needed |
|---------|----------------------|-------------------|--------------------|
| Degree Centrality | Q1, Q6 | ✓ | ✗ |
| Betweenness Centrality | Q2, Q7, Q8 | ✓ | Needs example simplification |
| Clustering Coefficient | Q3, Q4 | Partial | Missing formal definition |
| Scale-Free Networks | Q5, Q10 | ✗ | Add new section |
| PageRank | Q11-Q14 | ✓ | ✓ |

#### Linking Guidance

In notes.md, after explaining a concept, add cross-references to assignment:
```markdown
**Related Assignment Questions**: See Q1, Q2, Q5 for applications of this concept.
```

In assignment.md answer explanations, reference the notes section:
```markdown
**Answer Explanation**: This tests **Degree Centrality** 
(see notes section "Degree Centrality" for definition).
```

---

### Quality Checklist for Gemini Content Enhancements

After integrating and enhancing Gemini content, verify:

- [ ] Every concept tested in assignment questions is explained in notes
- [ ] Every explanation in notes can be directly traced to assignment questions
- [ ] No new concepts introduced in notes without context from assignment
- [ ] Real-world examples match the week's assignment case studies
- [ ] Terminology is consistent between notes and assignment
- [ ] Verbose explanations include step-by-step breakdowns and contrasts
- [ ] Complex examples have simplified concrete versions using assignment context
- [ ] Visualizations (Mermaid diagrams) are placed after explanations with captions
- [ ] All mathematical formulas have intuitive interpretations in plain language
- [ ] Cross-references between notes.md and assignment.md are bidirectional

---

## Hyperlink Implementation Workflow

### Adding Question-to-Topic Hyperlinks

After completing notes.md and assignment.md for a new week, add **clickable hyperlinks** from each assignment question to its corresponding notes section. This enables students to seamlessly navigate from questions to theoretical explanations.

### Automated Process Using Subagents

To add hyperlinks efficiently across all questions:

**Step 1: Prepare Section Anchors in notes.md**
- Ensure all section headers in notes.md are clearly formatted (H2 or H3)
- These headers will automatically become markdown anchors (e.g., "Homophily Implementation" → `#homophily-implementation`)

**Step 2: Launch Subagent for Hyperlink Addition**
- Create a single subagent prompt (template below)
- Instruct the subagent to:
  1. Read both `/Week X/notes.md` and `/Week X/assignment.md`
  2. Map each question to its corresponding notes section
  3. Add hyperlinks in the format: `**Related Topic**: [Section Name](notes.md#section-anchor)` at the end of each Answer Explanation
  4. Update the assignment.md file directly using `replace_string_in_file`

**Step 3: Subagent Prompt Template**

```
You are tasked with adding hyperlinks to Week X's assignment.md file.

**Task:**
1. Read `/Volumes/My Drive/Priyodip/college notes and stuff/sem6/NPTEL/Social Networks/Week X/notes.md`
2. Read `/Volumes/My Drive/Priyodip/college notes and stuff/sem6/NPTEL/Social Networks/Week X/assignment.md`
3. For each question, identify which notes section it tests
4. Add a hyperlink at the end of each Answer Explanation section:
   Format: `**Related Topic**: [Section Name](notes.md#section-anchor)`
   - Convert section headers to lowercase with hyphens for anchors
5. Use replace_string_in_file to update the assignment file directly

**Topic Mapping for Week X:** [List of question→topic mappings specific to that week]

**Output:** Confirm all questions updated and list hyperlinks added.
```

### Hyperlink Format Standards

**Markdown Syntax:**
```markdown
**Related Topic**: [Section Name](notes.md#anchor-name)
```

**Anchor Naming Convention:**
- Convert section headers to lowercase
- Replace spaces with hyphens
- Examples:
  - "Homophily: Birds of a Feather Flock Together" → `#homophily-birds-of-a-feather-flock-together`
  - "Measuring Homophily: The Homophily Coefficient" → `#measuring-homophily-the-homophily-coefficient`
  - "Degree Centrality" → `#degree-centrality`

**Placement in Assignment:**
- Add the hyperlink at the **end of each Answer Explanation section**
- Example:

```markdown
### Q3 (1 point)
**Question text here**

- [ ] A) Option A
- [ ] B) Option B
- [ ] C) Option C
- [ ] D) Option D

**Answer:** B

**Answer Explanation**: Explanation text here. This tests the concept of XXX.

**Related Topic**: [Degree Centrality](notes.md#degree-centrality)
```

### Why Hyperlinks Matter

1. **Navigation**: Students can immediately jump from a confusing question to the exact theoretical explanation
2. **Self-Paced Learning**: Links enable students to review related concepts without manual searching
3. **Bidirectional Reference**: Combines with notes having question references, creating a complete two-way knowledge graph
4. **Study Efficiency**: Reduces cognitive load by eliminating the search for "where did we learn this?"

### Question-to-Topic Mapping: Standard Categories

**Common topics across all weeks:**
- Centrality measures (Degree, Betweenness, Closeness, PageRank)
- Clustering Coefficient and related measures
- Network algorithms (BFS, Girvan-Newman)
- Scale-Free Networks and Power Law distributions
- Homophily and similar-like-similar principles
- Closure types (Triadic, Focal, Membership)
- Community detection
- Contagion and social influence

**Mapping Strategy:**
1. Extract the concept name from the answer explanation
2. Find the matching section header in notes.md
3. Convert to anchor format
4. Add hyperlink to Answer Explanation section

### Quality Checklist for Hyperlinks

- [ ] All questions have at least one hyperlink
- [ ] Each hyperlink target exists in notes.md as a section header
- [ ] Anchor names match section header titles (converted to kebab-case)
- [ ] Hyperlinks are placed consistently (end of Answer Explanation)
- [ ] No broken links (verify by checking notes section names)
- [ ] Related concepts are linked where applicable (not just one per question)
- [ ] Markdown syntax is correct: `[text](file#anchor)`

---

## Answer Explanation Quality Standards

Every assignment answer must include:

1. **Concept Identification**: Name the specific concept being tested
2. **Direct Answer**: State why the selected option is correct
3. **Why Not Others**: Briefly explain what makes other options wrong
4. **Practical Example**: Show how this concept applies in real-world context
5. **Connection**: Link to the theoretical material in notes.md

**Example Template**:
> **Answer Explanation**: This perfectly describes **[Concept Name]**. [Why this answer is correct]. [Real-world example]. [What the other options represent]. This differs from [related concept] because [key distinction].

---

## Formatting Rules

### Markdown Conventions

- **File Names**: Use lowercase with hyphens (e.g., `notes.md`, `assignment.md`)
- **Headings**: H1 for title, H2 for major sections, H3 for subsections
- **Lists**: Use `-` for bullet points, numbered lists only for steps
- **Emphasis**: `**bold**` for key terms, `*italic*` for subtle emphasis
- **Code**: Backticks for inline code, triple backticks for blocks
- **Links**: Use relative paths for internal references

### Table Standards

- Always include alignment indicators (`:---`, `:---:`, `---:`)
- Minimum 3 columns for readability
- Use for: Concept summaries, Answer keys, Real-world examples

### Mathematical Standards

- Inline equations: `$equation$`
- Block equations: `$$equation$$`
- Define variables in text following the equation
- Provide intuitive interpretation after formal notation

---

## Common Pitfalls to Avoid

1. ❌ Don't mix theoretical notes with assignment answers
2. ❌ Don't create answer-only files without explanations
3. ❌ Don't use generic explanations; always reference concepts by name
4. ❌ Don't include metadata (due dates, scores) in theory notes
5. ❌ Don't skip connecting new material to previous weeks
6. ❌ Don't use unexplained jargon without definition
7. ❌ Don't create files outside the Week X folder structure

---

## File Checklist Before Completing

### Initial Content Structure
- [ ] Created `/Week X/` folder
- [ ] Created `notes.md` with all theory sections
- [ ] Created `assignment.md` with all questions and answers

### Notes Content Quality
- [ ] All questions have full explanations, not just correct answers
- [ ] Answer key summary table is complete with concept column
- [ ] Cross-references between notes.md and assignment.md work
- [ ] All mathematical formulas render correctly in LaTeX
- [ ] Real-world examples are present for each concept
- [ ] Consistent formatting and style throughout
- [ ] Summary/Key takeaways sections present in notes.md
- [ ] Mermaid diagrams used **only where they add value** (not overused)
- [ ] All Mermaid diagrams have descriptive captions
- [ ] Diagrams placed after explanations (not before)

### Gemini Content Validation (if integrating from Gemini)
- [ ] Every assignment case study is covered in notes.md
- [ ] Every concept tested in assignment questions appears in notes.md
- [ ] All real-world examples align with assignment case study context
- [ ] Terminology is consistent between notes.md and assignment.md
- [ ] Missing concepts from assignment have been added to notes
- [ ] Explanations are verbose enough for student understanding
- [ ] Complex examples have simplified versions with assignment context
- [ ] Visualizations (graphs, diagrams) added where helpful
- [ ] All mathematical formulas have intuitive plain-language explanations
- [ ] Bidirectional cross-references link notes to assignment questions

---

## When New Gemini Content Arrives: Complete Workflow

### Step-by-Step Process

1. **Receive Gemini Content**
   - User provides Gemini-generated notes for Week X
   - Save content to temporary location for review

2. **Paste Content into notes.md**
   - Create `/Week X/` folder if it doesn't exist
   - Paste Gemini content directly into `/Week X/notes.md`
   - **Do not edit yet** — preserve original format for validation

3. **Load the Assignment**
   - Open `/Week X/assignment.md` (should already exist)
   - Read all case studies and questions carefully
   - Identify all concepts being tested

4. **Run Validation Checklist** (see "Gemini Content Validation Checklist" above)
   - Compare Gemini's sections to assignment case studies
   - Identify missing concepts (Category 1)
   - Identify unclear explanations (Category 2)
   - Identify overly complex examples (Category 3)
   - Identify missing visualizations (Category 4)

5. **Execute Enhancements**
   - For each category identified:
     - Add missing content with definition + example + link to assignment
     - Expand brief explanations with intuitive breakdowns and contrasts
     - Simplify complex examples using assignment context
     - Add Mermaid diagrams after text explanations
   - Maintain consistent terminology with assignment questions

6. **Add Cross-References**
   - In notes.md: Add "Related Assignment Questions" links after each concept
   - In assignment.md: Reference notes section in each Answer Explanation
   - Ensure bidirectional linking for student navigation

7. **Run Quality Checklist** (see "Quality Checklist for Gemini Content Enhancements" above)
   - Verify all assignment concepts are in notes
   - Verify all notes concepts link to assignment
   - Check consistency and clarity
   - Validate visualizations add value

8. **Verify Formatting**
   - Check all LaTeX renders correctly
   - Verify consistent markdown style
   - Check Answer Explanation format
   - Test cross-reference links

9. **Final Review**
   - Read notes as a student would (checking against assignment)
   - Verify examples align with case studies
   - Confirm no deviation from course material
   - Document any assumptions or additions made

---

## Contact for Questions

When new content arrives, follow this complete workflow:
1. Receive Gemini content for new week
2. Paste into `/Week X/notes.md`
3. Load assignment for that week
4. Run **Gemini Content Validation Checklist** against assignment questions
5. Apply enhancements from **Enhancement Workflow: Four Categories**
6. Add cross-references between notes.md and assignment.md
7. Run **Quality Checklist for Gemini Content Enhancements**
8. Verify all formatting and rendering
9. Confirm all content ties directly to assignment topics without deviation

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

- [ ] Created `/Week X/` folder
- [ ] Created `notes.md` with all theory sections
- [ ] Created `assignment.md` with all questions and answers
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

---

## Contact for Questions

When new content arrives, follow this workflow:
1. Ask user if they have new weekly content (Week 3, 4, etc.)
2. Apply this AGENTS.md structure consistently
3. Separate content into notes + assignments automatically
4. Organize into the Week X folder structure
5. Verify all formatting and cross-references before completion

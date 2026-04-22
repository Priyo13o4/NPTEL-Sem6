# AGENTS.md

## What this repo is

This repository is a **study-material organizer for multiple NPTEL courses**. Each course is stored in its own folder, and each week is split into two files:

- `notes.md` — clear theory notes (definitions, intuition, examples)
- `assignment.md` — questions + answers + explanations (linked back to notes)

The goal is **fast revision + exam preparation** with consistent formatting.

---

## Standard structure

```
NPTEL/
├── <Course Name>/
│   ├── Week 1/
│   │   ├── notes.md
│   │   └── assignment.md
│   ├── Week 2/
│   │   ├── notes.md
│   │   └── assignment.md
│   └── ...
└── docs/ (optional)
```

**Naming rules**

- Course folder: short, human name (e.g., `Social Networks`, `LLMs`)
- Week folder: `Week X` (space-separated, capitalized)
- Files: always `notes.md` and `assignment.md`

---

## Writing rules (keep it consistent)

### `notes.md`

**Required layout**

1. Title: `# Week X: Theoretical Notes — <Topic>`
2. `## Overview` (2–4 sentences)
3. Concept sections (H2/H3)
4. `## Key Takeaways` table: `Concept | Definition | Example`
5. `## Summary` (tight bullets)

**Style**

- Bold key terms on first mention.
- Use inline math like `$P(y\mid x)$` and block math with `$$...$$` when the lecture is quantitative.
- Prefer simple, concrete examples (use the week’s assignment context when possible).
- Do **not** add “nice-to-have” topics unless they help answer the assignment or were hinted by the lecture.

### `assignment.md`

**Required layout**

1. Title: `# Week X: Assignment X — <Course Name> (<Course Code>)`
2. Metadata block (unknown values are allowed as `—`): Due / Submitted / Score
3. Group questions by case study/topic (when applicable)
4. End with an **Answer Key Summary** table: `Q | Answer | Concept`

**Question format**

```md
### Q1 (1 point)
**<Full question text>:**

- [ ] A) ...
- [ ] B) ...
- [ ] C) ...
- [ ] D) ...

**Answer:** B

**Answer Explanation**: What concept this tests + why correct + why others are wrong.

**Related Topic**: [<Notes Section Title>](notes.md#<anchor>)
```

---

## Notes ↔ Assignment linking (mandatory)

- Every question must include at least one `**Related Topic**` link into `notes.md`.
- In `notes.md`, important concepts should include: `**Related Assignment Questions**: Q1, Q4, ...`

**Anchor convention**

- Use the markdown heading’s auto-anchor (`kebab-case`, lowercase, punctuation removed).
- Keep headings stable so links don’t rot.

---

## Working from scraped / Gemini content

Use this lightweight checklist:

1. Extract assignment concepts → ensure each has a dedicated notes section.
2. If Gemini misses a concept that appears in the scrape/assignment, **add it** (definition + intuition + example).
3. Keep terminology consistent between notes and assignment.
4. Make sure the concepts mentioned in the options of the assignment questions are defined in the notes and explained verbosely. DO NOT assume the reader already knows them.

---

## Audit → Patch → Validate (required)

After creating/updating any week’s `notes.md` / `assignment.md`, do a quick sanity pass and fix issues immediately:

1. **Audit**
	- Coverage: every assignment concept exists as a notes section.
	- Links: every question has `**Related Topic**` links, and each anchor exists.
	- Markdown rendering syntax: headings/anchors align, tables render, lists/checklists are valid, math delimiters render correctly, and no duplicated option labels like `A) A)`.
	- Consistency: answers match explanations; formulas/counts are consistent.
	- Summary: Answer Key table matches the question answers.
2. **Patch**
	- Apply the minimal edits needed to resolve discrepancies.
3. **Validate**
	- Re-check the above items after patching.

If you’re using subagents, run a review agent after writing and then apply its suggested fixes in the same pass (don’t leave “TODO: fix later”).

---
name: edit-slides
description: Use when the user asks to edit, improve, or restructure Beamer lecture slides or teaching presentations. Triggers on "edit my slides", "improve these lecture slides", "clean up this Beamer deck", "fix the flow of my slides", or "/edit-slides".
argument-hint: "[path to .tex slide deck]"
---

# edit-slides

## Working principles

**One main message per slide.** If a slide tries to do more than one thing, split it.

**Minimal edits by default.** Do not rewrite everything unless asked. Prefer local edits that improve logic, clarity, pedagogical usefulness, and internal consistency. Preserve the instructor's intent, ordering, and style.

**Intuition first, then formalisation.** Introduce economic intuition before equations or formal structure. Never present a formal object without explaining what it means in words. Keep long derivations off the main slides — use appendix slides instead.

**Return LaTeX ready to paste.** Provide edited `.tex` snippets or the full edited file, ready to compile. Keep formatting changes minimal unless a larger rewrite is needed for clarity.

---

## Default workflow

### 1. Map the narrative

Write a concise outline of the lecture's logic. Identify where the argument jumps too quickly. Propose bridge slides where needed.

### 2. Diagnose slide by slide

For each slide, identify:
- **Goal** — what this slide is trying to do
- **Likely student confusion** — what students may misunderstand
- **Fix** — rewrite, reorder, simplify, add intuition, add figure, or move to appendix

### 3. Revise

Provide edited LaTeX ready to paste and compile.

### 4. Appendix slides

Add only when they materially improve comprehension. Use for: derivations, robustness, technical details, optional advanced material.

### 5. Quality checks

Before returning material, verify:
- Notation consistency throughout
- Timing conventions, sign conventions, and units
- Internal cross-references across slides
- Consistency between slides and speaking notes

---

## Speaking notes

When slides are revised, speaking notes must also be revised to match. For each slide, notes should include:
- What to say in 30–60 seconds
- Common confusion to pre-empt
- How the slide connects to the previous and next

---

## Slide structure template

Each substantive slide should follow (implicitly or explicitly):
1. **Headline claim** — the main point
2. **Intuition** — the economic idea in plain language
3. **Formal object** — equation, diagram, or definition if needed
4. **Takeaway** — what students should retain

---

## Output format

**1. Narrative map**

**2. High-impact fixes**

**3. Slide-by-slide suggested edits**

**4. Proposed appendix slides (if any)**

**5. Edited LaTeX (ready to paste)**

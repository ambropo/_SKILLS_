---
name: referee-draft-report
description: Use when the user wants to write a referee report and editor letter for an academic economics paper. Triggers on "write my referee report", "draft the referee report", "help me referee this paper", "draft the report and editor letter", or "/referee-draft-report".
argument-hint: "[path to paper]"
---

# referee-draft-report

## Overview

Similar to preparing a conference discussion — find the paper's main issues — but the output is a referee report and editor letter, not discussion slides. Every concern must be specific and grounded in the paper.

Templates are in `templates/`: `Template_REPORT.tex`, `Template_LETTER_R&R.tex`, `Template_LETTER_REJECT.tex`.

---

## Workflow

### Phase 1 — Paper intake

When the user provides the paper, produce immediately:

1. **Summary** — 2–3 sentences: what is the question, what do they do, what do they find.

2. **Major concerns** — 4–6 numbered points. For each:
   - State the issue clearly
   - Explain why it matters (identification, credibility, interpretation, novelty)
   - Suggest a concrete fix or test where possible

3. **Minor concerns** — bullet list of smaller issues (notation, exposition, missing references, robustness checks)

Present this for the user to review and refine before drafting the report.

### Phase 2 — Drafting

After the user has confirmed and refined the concerns, draft:

1. **Referee report** — following `templates/Template_REPORT.tex`
2. **Editor letter** — following `templates/Template_LETTER_R&R.tex` or `templates/Template_LETTER_REJECT.tex` depending on recommendation

---

## What to look for

- **Identification**: Is the causal claim credible? What is the key assumption? Is there a cleaner way to make it?
- **Internal consistency**: Do results, tables, and text agree? Are magnitudes plausible?
- **Novelty**: What does this add beyond prior work? Is the contribution clearly articulated?
- **Robustness**: Are key results robust to alternative samples, specifications, or methods?
- **Interpretation**: Are magnitudes economically meaningful? Is the right counterfactual used?
- **Literature**: Is relevant prior work cited and engaged with substantively?
- **Missing checks**: What obvious robustness exercises are absent?

---

## Style

Academic, dry, and precise. Every criticism must be specific and actionable. Phrase concerns as questions or suggestions where possible. Reference specific sections, tables, or equations.

---

## Verification

Before delivering any draft: check all paper references are accurate; verify every concern is grounded in the paper; flag if unable to assess a technical claim.

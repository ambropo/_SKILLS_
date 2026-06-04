---
name: editor-ref-consolidate
description: Use when the user wants to consolidate, summarise, or compare referee reports for a manuscript. Triggers on "consolidate the referee reports", "summarise referee comments", "what do the referees say", "merge the referee reports", or "/editor-ref-consolidate".
argument-hint: "[manuscript-folder or path to referee files]"
---

# editor-ref-consolidate

## Inputs

Look for referee files in the manuscript folder. Files follow the naming convention:
- `R1_letter`, `R1_referee` (or variations: `R1_letter.pdf`, `R1_report.pdf`, etc.)
- `R2_*`, `R3_*` for additional referees

Read all referee letters and reports you find. If no files are found, ask the user to point to them.

---

## Workflow

1. Read all referee files
2. Extract the main concerns raised by each referee
3. Identify points raised by multiple referees (consolidated concerns)
4. Identify points raised by only one referee (idiosyncratic concerns)
5. Deliver the structured report below

---

## Output format

### Consolidated concerns
*(raised by two or more referees — list each point once, note which referees raised it)*

For each consolidated concern:
- **[Short label]**: Description of the concern. *(Referees X and Y)*

### Idiosyncratic concerns

**Referee 1:**
- Bullet list of concerns raised only by Referee 1

**Referee 2:**
- Bullet list of concerns raised only by Referee 2

**Referee N (if applicable):**
- ...

### Overall tone
One sentence per referee: positive/negative, major/minor concerns, recommendation (if stated).

---

## Guidelines

- Synthesise overlapping points — do not list the same issue twice under different phrasings
- Be specific: reference what the referee says, not generic paraphrases
- Preserve important nuance; do not over-compress
- Flag if a referee's concern is ambiguous or hard to classify
- Do not add your own assessment of the paper — report only what the referees say

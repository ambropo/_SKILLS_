---
name: edit-stata
description: Use when the user asks to comment, structure, edit, or clean up a Stata do-file. Triggers on "comment my do-file", "add comments to Stata", "edit this Stata code", "structure my do-file", "clean up this do-file", or "/edit-stata".
argument-hint: "[path-to-dofile or directory]"
---

# edit-stata

## Working principles

**Process section by section.** Read the full file, identify all sections, then edit each section in turn using the `Edit` tool. Never rewrite the entire file in one shot — apply changes incrementally so each edit is targeted and reviewable. Return a brief change summary at the end.

**Comments only — never touch executable code.** You may freely add, correct, or restructure comments, headers, and formatting. Do not modify executable code unless the user explicitly asks.

**No guessing.** If a comment is ambiguous or you cannot determine the purpose of a block, say so rather than inventing an explanation.

---

## Mandatory checklist

For every commenting request, do all of the following unless the user narrows the scope:

- Check that the file header is correct and complete
- Check that the header lists all sections and subsections present in the code
- Check that the header lists all outputs (datasets, figures, tables) actually produced
- Update `Last edited` to today's date
- Add a short informative description below every section and subsection header
- Check that section and subsection numbering is sequential and consistent
- Check that `****` block comments are accurate, informative, and concise
- Add a `NOTES TO FIG/TAB` block for every figure/table-producing block (always required — see §NOTES TO FIG/TAB)
- Replace every `[add insight]` placeholder with an `INSIGHTS` block (see §INSIGHTS)
- Check spacing: exactly one empty line after every code block or section description

**Final spacing check (mandatory):** before returning the file, scan the full edited scope and confirm there is never more than one consecutive empty line anywhere.

---

## File header

Every do-file must begin with a `/* */` header block containing:

- **SUMMARY**: 2–3 sentences describing what the code does
- **SECTIONS**: numbered list matching the section headers in the code
- **OUTPUTS**: list of output files created (datasets, figures, tables)
- **Last edited**: date in `YYYY-MM-DD` format

```stata
/*==============================================================================
SUMMARY:
-------
Brief description of what this code does and its purpose in the project.

SECTIONS:
--------
[1] First main section
[2] Second main section

OUTPUTS:
-------
- output1.dta: Description
- output2.pdf: Description

--------------------------------------------------------------------------------
Last edited: YYYY-MM-DD
==============================================================================*/
```

---

## Sections and subsections

Section:

```stata
*===============================================================================
**** [1] SECTION NAME IN ALL CAPS
*===============================================================================
```

Subsection:

```stata
*-------------------------------------------------------------------------------
**** [1.1] Subsection name in sentence case
*-------------------------------------------------------------------------------
```

Each section and subsection must be followed by a short explanatory comment block:

```stata
/* This subsection computes event-window surprises from the aligned Bitcoin
price series, stores the shock measures used downstream, and prepares the
objects later used in regressions and charts. */
```

Descriptions must be specific. Do not restate the title or use generic placeholders.

---

## Block comments

- Add a `****` comment before every code block that is preceded by an empty line, unless that line is a section/subsection header.
- When multiple consecutive lines form one logical unit, add **one** `****` comment before the first line only.
- Use `//` only for brief inline clarifications at the end of a line.
- Leave a blank line immediately above every `****` comment, except after a section/subsection header or separator.
- Never leave two or more consecutive blank lines.

**Applies to all Stata commands**, including: `preserve`, `restore`, `use`, `save`, `export`, `reshape`, `capture`, `forvalues`, `foreach`, `if`, `gen`, `replace`, `drop`, `keep`, `merge`, `append`, `collapse`, `bysort`, `sort`, `xtset`, `reghdfe`, `reg`, `graph`, `twoway`, and any others.

**Verification step:** after commenting, scan the file line by line. For every empty line, check if the next non-empty line is executable code (not a section/subsection header). If yes, confirm a `****` comment appears immediately above it. Fix any violations before returning.

---

## Existing comments

- Keep existing comments; clarify or correct them if needed.
- Do not remove comments unless clearly wrong and replaced.
- Comments written as `/**** ... ****/` are author notes — never modify, move, or remove them.

---

## INSIGHTS blocks

Add an `INSIGHTS` block only when an `[add insight]` placeholder is present in the code. Do not add proactively.

Use for deeper explanation: why this approach is used, what the non-obvious logic is, relevant caveats.

```stata
/*....... INSIGHTS .......
	Explanation text. */
```

Place below the section/subsection header and description, before the first `****` action comment. Each line indented with one tab.

---

## NOTES TO FIG/TAB blocks

Required for every figure/table-producing block. Place below the `****` action comment, before the code.

```stata
/*....... NOTES TO FIG/TAB .......
	Draft-ready note describing the output, construction, sample, and sources. */
```

Must include:
- What is shown
- Sample/time period
- Key construction details
- Meaning of groups/colors/panels
- Data sources

---

## Date

Every time you modify a do-file, update `Last edited` in the file header to today's date in `YYYY-MM-DD` format.

---

## Style

- Comments explain **what the code does and why** — not restate syntax.
- Keep comments concise and factual.
- If spacing rules conflict, prioritise: (1) readability, (2) internal consistency, (3) strict rule adherence.

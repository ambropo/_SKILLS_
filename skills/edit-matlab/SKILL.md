---
name: edit-matlab
description: Use when the user asks to comment, structure, edit, or clean up a MATLAB file. Triggers on "comment my MATLAB file", "add comments to MATLAB", "edit this .m file", "structure my MATLAB code", "clean up this function", or "/edit-matlab".
argument-hint: "[path-to-file or directory]"
---

# edit-matlab

## Output format (required every time)

Return output in exactly this structure — do not omit any section:

**1. Edited file** (or edited scope if partial)

**2. Change summary:** what was added or corrected; any header fields updated; any blocks where intent was uncertain

**3. Verification report:** confirm you scanned the full edited scope line by line. For every blank line followed by executable code, confirm a `%` comment appears immediately before it. State how many violations were found and fixed (or "none found").

If there are no flags, write "None."

---

## Working principles

**Process section by section.** Read the full file, identify all sections, then edit each section in turn using the `Edit` tool. Never rewrite the entire file in one shot — apply changes incrementally so each edit is targeted and reviewable.

**Comments only — never touch executable code.** You may freely add, correct, or restructure comments, headers, and formatting. Do not modify executable code unless the user explicitly asks.

**No guessing.** If a comment is ambiguous or you cannot determine the purpose of a block, say so rather than inventing an explanation.

---

## Mandatory checklist

For every commenting request, do all of the following unless the user narrows the scope:

- Check that the file header is correct and complete
- Check that the header lists all sections and subsections present in the code
- Check that the header lists all outputs (figures, tables, saved files) actually produced
- Update `Updated:` date to today's date in `YYYY-MM-DD` format
- Add a short informative description below every section and subsection header
- Check that section and subsection numbering is sequential and consistent
- Check that `%` block comments are accurate, sufficiently informative, and concise
- Add a `NOTES TO FIG/TAB` block for every figure/table-producing block (always required — see §NOTES TO FIG/TAB)
- Replace every `[add insight]` placeholder with an `INSIGHTS` block (see §INSIGHTS)
- Check spacing: exactly one empty line after every code block or section description

**Final spacing check (mandatory):** before returning the file, scan the full edited scope and confirm there is never more than one consecutive empty line anywhere.

---

## File header

**Function files** (e.g. `VARmodel.m`) use this header immediately after the `function` declaration:

```matlab
function [out] = FuncName(args)
% =======================================================================
% One-line description of what this function does
% =======================================================================
% [out] = FuncName(args)
% -----------------------------------------------------------------------
% INPUT
%   - arg1: description [type]
%   - arg2: description [type]
% -----------------------------------------------------------------------
% OPTIONAL INPUT
%   - opt1: description [dflt = X]
% -----------------------------------------------------------------------
% OUTPUT
%   - out: description [type]
% -----------------------------------------------------------------------
% EXAMPLE
%   arg1 = randn(100,3);
%   [out] = FuncName(arg1);
% =======================================================================
% VAR Toolbox 4.0
% Ambrogio Cesa-Bianchi
% ambrogiocesabianchi@gmail.com
% First version: Month YYYY. Updated: YYYY-MM-DD
% -----------------------------------------------------------------------
```

**Script files** (e.g. `GO_SW2001.m`) use a short comment block at the top:

```matlab
% One-line description of what this script does.
% =======================================================================
% Additional context if needed (2–3 lines max): what replication, what
% outputs are produced, what toolbox version is required.
% =======================================================================
% VAR Toolbox 4.0
% Ambrogio Cesa-Bianchi
% ambrogiocesabianchi@gmail.com
% Month YYYY. Updated: YYYY-MM-DD
% -----------------------------------------------------------------------
```

Rules:
- Omit `OPTIONAL INPUT` if there are none; omit `OUTPUT` only for void functions
- Description must be specific — do not restate the function name
- Top border uses `=`; internal separators use `-`; both run exactly 71 characters
- `EXAMPLE` must be self-contained with made-up data defined inline (e.g. `X = randn(20,3)`), showing how to call the function and capture its output. Aim for 1–2 lines; never exceed 5. Do not refer to external files.

---

## Sections and subsections

Sections use MATLAB cell-mode `%%` (creates a new cell in MATLAB's cell editor). Subsections use a single `%`. Section names in ALL CAPS; subsection names in sentence case.

**Section:**
```matlab
%% N. SECTION NAME IN ALL CAPS
% -----------------------------------------------------------------------
```

**Subsection:**
```matlab
% N.M. Subsection name in sentence case
% -----------------------------------------------------------------------
```

Each section and subsection must be followed by a short explanatory `%` comment block:

```matlab
%% 2. IDENTIFICATION
% -----------------------------------------------------------------------
% The B matrix maps structural shocks to reduced-form residuals. It is
% recovered differently depending on the identification scheme specified
% in VARopt.ident (short, long, sign, iv, exog).

% 2.1 Short-run restrictions
% -----------------------------------------------------------------------
% Cholesky decomposition of the VAR covariance matrix; ordering matters.
```

Descriptions must be specific. Do not restate the title or use generic placeholders. Numbering must be sequential and consistent.

---

## Block comments

- Add a `%` comment before every logical code block preceded by a blank line, unless that line is a section/subsection header or separator.
- When multiple consecutive lines form one logical unit, add **one** `%` comment before the first line only.
- Use inline `% comment` at the end of a line only for brief clarifications: `x = x + 1; % shift index`
- Leave one blank line above every `%` block comment, except directly after a section/subsection header or separator line.
- Never leave two or more consecutive blank lines.

**No uncommented code blocks.** Every group of executable lines preceded by a blank line must have a `%` comment immediately above it.

**Verification step:** after commenting, scan the file line by line. For every blank line followed by executable code (not a section/subsection header or separator), confirm a `%` comment appears immediately before that code. Fix any violations before returning.

---

## Existing comments

- Keep existing comments; clarify or correct them if needed.
- Do not remove comments unless clearly wrong and replaced.
- Comments written as `%ACB` are author notes — never modify, move, or remove them.

---

## INSIGHTS blocks

Add an `INSIGHTS` block only when an `[add insight]` placeholder is present in the code. Do not add proactively.

Use for non-obvious logic, design choices, or caveats only.

```matlab
%....... INSIGHTS .......
% Explanation of why this approach is used, any non-obvious logic,
% and relevant caveats or limitations.
%..........................
```

Place below the section/subsection header and description, before the first block comment.

---

## NOTES TO FIG/TAB blocks

Required for every figure/table-producing block. Place below the `%` block comment, before the figure/table code — outside any loop, not inside it.

```matlab
% Open figure and loop over responses
%....... NOTES TO FIG/TAB .......
% What is shown, sample/time period, key construction details,
% meaning of lines/colors/panels, data sources.
%..................................
figure;
for ii = 1:nvar
    ...
end
```

Do not place the NOTES TO FIG/TAB block inside the loop body.

---

## Date

Every time you modify a MATLAB file, update the `Updated:` date in the file header to today's date. Format:

```matlab
% First version: Month YYYY. Updated: YYYY-MM-DD
```

Both `First version:` and `Updated:` (with colons) are mandatory.

---

## Style

- Comments explain **what the code does and why** — not restate syntax or variable names.
- Keep comments concise and factual.
- If spacing rules conflict, prioritise: (1) readability, (2) internal consistency, (3) strict rule adherence.

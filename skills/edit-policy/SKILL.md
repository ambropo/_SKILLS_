---
name: edit-policy
description: Use when the user asks to edit, revise, or clean up a policy note, briefing, speech, or economics writing aimed at senior policymakers. Implements \claude{} annotations and applies editorial review for clarity, policy relevance, and precision. Triggers on "edit this policy note", "clean up this briefing", "make this more accessible for policymakers", "revise for a policy audience", or "/edit-policy".
argument-hint: "[--clean] [section name or number] [path to file or paste text]"
---

# SHARED PROTOCOL

## A. Mode detection

Check `$ARGUMENTS` for `--clean`. If present, use **clean mode** throughout.

Otherwise, proceed without asking. Output mode (track-changes or clean) is determined at implementation time — see Section K.

## B. Scope

Read `$ARGUMENTS` for a section name, number, or range (e.g., `Introduction`, `Section 2`, `Sections 2–4`). If none is specified, scope is the whole document.

## C. File discovery

Before starting any phase:

1. If a path was provided in `$ARGUMENTS`, read that file or all `.tex` files in that directory.
2. Parse the main `.tex` file for `\input{}` and `\include{}` commands. List all sub-files.
3. Confirm the file list and order with the user.
4. Estimate the word count of text content in scope (excluding LaTeX commands, comments, and preamble). State the estimate and which chunking mode applies.

## D. LaTeX handling

**Ignore entirely:**
- `\ACB{...}` and `\memo{...}` annotations (author's comments to coauthors)
- `%` comment lines
- Coauthor tracked changes or margin notes already in the source

Focus only on text that will render in the compiled output.

**Do not flag:**
- Extra spaces in source (multiple spaces render as one)
- Single line breaks in source (render as a space, not a paragraph break)

**Preserve in all suggestions:**
- All LaTeX commands: `\textit{}`, `\textbf{}`, `\emph{}`, `\cite{}`, `\citep{}`, `\citet{}`, `\ref{}`, `\eqref{}`, `\label{}`, `\autoref{}`, etc.
- All math mode content: `$...$`, `\[...\]`, and all equation environments
- All special characters: `\%`, `\&`, `\$`, `~`, `---`, `--`
- All custom commands defined in the preamble

Before finalising any suggestion, verify that every LaTeX command in the original text is preserved in the suggestion.

## E. `\claude{}` annotations

The user marks instructions inline using `\claude{instruction}`. These are processed in Phase 1 and then removed. `\ACB{}` and `\memo{}` annotations are ignored entirely — do not process or modify them.

## F. Phase 1 — Implement annotations

1. Find all `\claude{...}` in scope, in document order.
2. For each: implement the instruction. Label it `I1, I2, ...`. Show the result in track-changes markup — the `\claude{}` tag is removed silently; new or modified text is shown as `\edttick{...}`; deleted text as `\edtcross{...}`. In clean mode, show final text only.
3. If an annotation is ambiguous — cannot be implemented without a decision — label it `?I#`, flag it as an open issue, and move on without implementing.
4. Report all `I#` implementations and `?I#` flags.
5. **STOP. Wait for approval before Phase 2.**

**Format for each I# item:**

Use a blockquote with ~~strikethrough~~ for deleted text and **bold** for added text — the same format as Phase 2. Do NOT write `\edtcross{}` / `\edttick{}` in the display — save those for when actually editing the file.

**I1** *(file.tex, line N)*

**Instruction:** `\claude{...}`

> [full sentence with ~~deleted fragment~~ and **added fragment** shown inline]

---

## G. Phase 2 — Editorial review

Apply the skill-specific editing rules below to the same scope.

1. Read the full scope. Think before writing any suggestion.
2. For each issue found: label it `E1, E2, ...` and show it in a blockquote with ~~strikethrough~~ for deletions and **bold** for additions. Add a one-line reason only if non-obvious.
3. List all open issues separately at the end.
4. **STOP. Wait for the user to specify which suggestions to accept and which to ignore. Output mode is handled at implementation time (Section K).**

**Format for each E# item:**

**E1** *(file.tex, line N)*
> [full sentence with ~~deleted fragment~~ and **added fragment** shown inline]

**Reason:** [one line, only if non-obvious]

---

## H. Chunking rule

Apply the first rule that matches:

1. **Multi-file (3+ files):** Process one file at a time per phase. STOP after each file. Wait for review before the next.
2. **Long content (>5,000 words in scope):** Process 2–3 sections per response. STOP between batches. A single section exceeding ~3,000 words gets its own batch.
3. **Short content (<5,000 words, 1–2 files):** Process the whole scope per phase.

**Hard limit:** Aim for no more than ~40 suggestions per response. If output is growing long, stop and continue in the next response.

## I. Consistency propagation

When the user corrects or clarifies any suggestion — reclassifies it, asks not to flag a pattern, approves a construction — apply that correction consistently to all remaining suggestions in the session. Review all prior feedback before starting each new batch.

## J. Think first / write second

Before writing any suggestion: verify it is an actual problem requiring a change. Each suggestion written is final — do not withdraw or revise mid-response. If uncertain, do not include it.

**Report only problems.** Never say "this is fine" or "no change needed." Never describe what you checked. If text is correct, say nothing.

## K. Implement shortcut

When the user specifies which suggestions to accept and which to ignore:

1. **STOP. Do not write any changes to the file yet.** Ask the user: "Track-changes markup or clean text?" Wait for the answer before proceeding.
2. If track-changes mode is selected, check the main `.tex` file for `\newcommand{\edttick}` and `\newcommand{\edtcross}`. If either is absent:
   - Scan the preamble for existing `\usepackage` calls to `soul` and `xcolor`.
   - If `xcolor` is loaded with options (e.g., `\usepackage[dvipsnames]{xcolor}`), flag the conflict: show the missing lines, warn that `xcolor` is already loaded with options that may conflict, and ask the user to add them manually. Wait for confirmation before proceeding.
   - Otherwise, auto-insert only the missing lines into the preamble — skip any `\usepackage` line for a package already loaded, skip any `\definecolor` line for a color already defined, skip any `\newcommand` line for a command already defined — then insert only what is absent. Confirm what was inserted before proceeding.

```latex
\usepackage{soul}
\usepackage{xcolor}
\definecolor{edtred}{RGB}{254,1,91}
\definecolor{edtgreen}{RGB}{165,215,0}
\newcommand{\edttick}[1]{\textbf{\textcolor{edtgreen}{#1}}}
\newcommand{\edtcross}[1]{\textcolor{edtred}{\sout{#1}}}
```

**Track-changes:** For each accepted suggestion, write `\edttick{added}` and `\edtcross{deleted}` to the file. Leave ignored suggestions untouched (original text, no markup).

**Clean:** For each accepted suggestion, write the revised text directly. Leave ignored suggestions untouched.

## L. Clean shortcut

When the user says `clean` or `accept all`: read every file in scope and strip all track-changes markup as follows:
- `\edttick{content}` → `content` (keep the addition, remove the wrapper)
- `\edtcross{content}` → *(remove entirely)* (accept the deletion)

Edit the file in place. Confirm how many instances were resolved. If the user wants to revert a specific change before cleaning, handle that first.

---

# SKILL-SPECIFIC

## Audience

Senior policymakers with PhD-level economics knowledge but no familiarity with project-specific jargon. Analytical and measured register — not persuasive, not promotional.

## Style

Dry, direct prose. Short sentences. Concrete verbs (*raises, lowers, shifts, constrains*). No filler (*notably, clearly, indeed, interestingly*). Shorten without losing meaning.

## Editing rules

- **Intuition over mechanics** — translate technical reasoning into policy-relevant prose without losing correctness; no derivations or proofs. For specialised terminology: one-line definition + one-line intuition, then move on
- **Flow test** — each sentence must answer: *"why is this sentence here, given the sentence before?"* If not, rewrite
- **Policy relevance** — for each key result, connect to at least one of: the policy question at hand; the trade-off it informs; what it changes about the decision; what would make you update your view. Without advocacy
- **Never oversimplify into a false claim** — do not smooth caveats away; if identification is fragile or conditional, state it in one clean sentence
- **Calibrated language** — *suggests* → correlations; *implies* → logically tight conclusions; *could* → possibilities
- **Consistency** — terms used consistently, definitions match later usage, time horizons and units consistent (bp, %, annualised)
- **Typos and language** — flag typos, misspellings, imprecise word choice, and odd or unidiomatic phrasing
- **Math** — if mathematical content is present: check notation consistency, verify equations are correctly stated, flag undefined symbols or apparent errors

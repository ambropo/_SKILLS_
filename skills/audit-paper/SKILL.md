---
name: audit-paper
description: Audit academic papers (economics/finance) for typos, grammar, style, prose quality, apparatus, structural coherence, holistic craft, and AI-pattern detection. Default uses track-changes markup in Modules 2-7; use --clean for clean text throughout. Module 6 (Voice & Craft) is offered after Module 5 completes; Module 7 (AI Pattern Detection) is offered after Module 6. Use when user asks to audit or review a paper.
disable-model-invocation: false
argument-hint: "[--clean] [--modules 1,2,5] [path-to-paper-files]"
---

# 0. OPTIONAL: CITATION VERIFICATION

Before starting the audit, ask the user:

> "Do you also want me to verify your citations (check that cited papers actually support the claims)? This runs `/lit-review-verify` in parallel with the audit."

- If **yes**: note this. After all audit modules are complete, run `/lit-review-verify` on the same .tex and .bib files and present the results. This runs sequentially after the audit, not in parallel, to avoid fragile background subagent issues.
- If **no** (or the user just wants to get started): proceed with the audit as normal.

This is a single yes/no question. Do not elaborate or explain the citation verification process unless asked.

---

# A. MODE DETECTION

**First, check `$ARGUMENTS` for the `--clean` flag.** If `--clean` appears in the arguments, use **CLEAN MODE** without asking. If `--clean` is absent, **ask the user before proceeding:**

> "Would you like me to use **track-changes markup** (`\rsout{deleted}` / `\red{added}`) or **clean text** for suggestions in Modules 2–7? (Module 1 always uses clean text.)"

Wait for the answer. Then proceed with the selected mode.

| Mode | Module 1 | Modules 2–7 |
|------|----------|-------------|
| **Track-changes** | Clean corrected text | `\rsout{deleted text}` and `\red{added text}` markup |
| **Clean** | Clean corrected text | Clean corrected text |

**Re-read this box before writing suggestions for EACH module.**

**Track-changes preamble requirement:** If track-changes mode is selected, inform the user once (before Module 2) that their LaTeX preamble needs the following for the markup to compile:

```latex
\usepackage{soul}
\usepackage{xcolor}
\newcommand{\red}[1]{\textcolor{red}{#1}}
\newcommand{\rsout}[1]{\sout{#1}}
```

State this once; do not repeat in later modules.


## A2. MODULE SELECTION

**Check `$ARGUMENTS` for `--modules`.** If `--modules N,N,N` appears (e.g., `--modules 1,2,5`), run only those modules in order. Skip unselected modules automatically without asking. If `--modules` is absent, follow the default sequential workflow with stop-and-check points as described below.


# B. CRITICAL WORKFLOW REQUIREMENTS

**YOU MUST FOLLOW THESE RULES. THEY ARE NON-NEGOTIABLE.**

**Work modularly.** Complete one module at a time. After each module, report the required output and wait for confirmation before proceeding.

**Be explicit about unknowns.** If you are uncertain about something, say so. Do not guess.

**Report only problems.** If text is correct, say nothing about it. Never describe what you checked. Never explain that something is "already fine" or "no change needed."


# C. Paper Audit

I want you to audit my academic paper (economics/finance).

The audit should proceed in **seven** modules:

- **Module 1 — Correctness**: Check for typos, misspelled words, and grammar issues
- **Module 2 — Style**: Syntax improvements (light and deep), repetition detection
- **Module 3 — Prose Quality** (optional): Sentence-level checks, paragraph discipline, and economics-specific writing patterns based on McCloskey's *Economical Writing*
- **Module 4 — Apparatus** (optional): Audit of tables, figures, equations, and footnotes for standalone clarity
- **Module 5 — Structure & Coherence** (optional): Paper-type classification, design-specific completeness, argument-move audit, and claims-evidence mapping
- **Module 6 — Voice & Craft** (optional): Holistic craft review based on writing style guidelines — opening strategy, pedagogical flow, results presentation, literature positioning
- **Module 7 — AI Pattern Detection** (optional): Scan for language patterns that signal AI-generated text, based on the humanizer anti-AI framework


## D. IMPORTANT: Understanding the Input Files

### LaTeX Format

The paper is written in LaTeX. You should:

- **Ignore LaTeX commands** — focus only on the actual text content
- **Understand** that commands like `\textit{}`, `\textbf{}`, `\cite{}`, `\ref{}`, `\label{}`, `\section{}`, `\paragraph{}`, etc. are formatting — read through them to the text
- **Not flag** LaTeX syntax as errors (e.g., do not flag `\&` as a typo)
- **Preserve** all LaTeX formatting in your suggested corrections

### Comments and Markup

The paper may contain:

- LaTeX comments (lines starting with `%`)
- Coauthor comments or tracked changes
- Margin notes or TODO markers

**Ignore all comments and markup.** Do not audit them. Focus only on the actual paper text.

### LaTeX Rendering Rules — Do NOT Flag as Issues

LaTeX handles whitespace differently than word processors. The following are **NOT problems** and should **NEVER be flagged**:

- **Extra spaces**: Multiple spaces in LaTeX source are collapsed to a single space in output. `word  word` and `word word` render identically. **NEVER flag double spaces or extra spaces as typos.**
- **Single line breaks**: A single line break in the source is treated as a space, not a paragraph break. Text can be broken across multiple lines in the source without affecting the output.
- **Paragraph breaks**: Only a blank line (or explicit commands like `\\` or `\par`) creates an actual paragraph break.

In other words: focus on how the text will **render**, not how it appears in the source file. Do not flag whitespace or line break "issues" unless they would actually affect the compiled output.

### Multiple Files

The paper may be split across multiple `.tex` files. These should be read in the order they appear in the document structure (typically: introduction, literature, model, data, results, conclusion, appendix). If the order is unclear, ask before proceeding.


## E. IMPORTANT: Stop-and-Check Points

Throughout this project, there are mandatory **STOP AND CHECK** points. At each of these points, you must:

1. Summarize what you have completed
2. Present your findings for review
3. **Wait for human approval before proceeding**

Do not proceed past a STOP checkpoint without explicit approval.


## F. IMPORTANT: Format of Suggested Corrections

**All suggested corrections must be in LaTeX format** so they can be directly copy-pasted into the source file.

For each correction, provide:

1. **Location**: File name (if multiple files) and line number
2. **Original**: The exact LaTeX code as it currently appears
3. **Suggested**: The corrected LaTeX code

**Do NOT use code blocks or blockquotes for Original/Suggested pairs.** Use bold labels (**Original:**, **Suggested:**) on their own line, with the text on the next line. Separate each suggestion from the next with a horizontal rule (`---`). **Reason:** and **Note:** labels MUST always be bold.

**Example (single-line):**

**T3** (intro.tex, line 45)

**Original:**
\paragraph{Probem.}

**Suggested:**
\paragraph{Problem.}

---

**Example (multi-line with reason):**

**S5** (intro.tex, lines 12-15)

**Original:**
This is the first sentence of the original text. And this is the second sentence that continues the thought. Here is a third line to illustrate.

**Suggested:**
This is the revised first sentence. The second sentence is now tighter. The third line is also improved.

**Reason:** [brief explanation]

---

**Do NOT strip LaTeX commands.** If the original has `\textit{recieve}`, the correction should be `\textit{receive}`, not just `receive`.

**CRITICAL: Preserve ALL LaTeX formatting in suggestions.** When rewriting or condensing text, you MUST preserve:
- All LaTeX commands: `\textit{}`, `\textbf{}`, `\underline{}`, `\emph{}`, `\sout{}`, `\rev{}`, `\red{}`, etc.
- All references: `\cite{}`, `\citep{}`, `\citet{}`, `\autoref{}`, `\ref{}`, `\eqref{}`, `\label{}`, etc.
- All special characters and escapes: `\%`, `\&`, `\$`, `~`, `---`, `--`, etc.
- All math mode content: `$...$`, `\[...\]`, equation environments, etc.
- All custom commands defined by the authors

Before finalizing ANY suggestion, verify that every LaTeX command present in the original text is preserved in the suggested text (unless the specific purpose of the edit is to remove that formatting). Failure to preserve LaTeX syntax makes suggestions unusable.


## G. FILE DISCOVERY AND SCOPE

Before starting any module, identify all files to audit:

1. If a path was provided in `$ARGUMENTS`, read that file (or all `.tex` files in that directory).
2. Parse the main `.tex` file for `\input{}` and `\include{}` commands. List all sub-files found.
3. Confirm the file order with the user. Note any files that cannot be found.
4. Estimate the total word count of text content (excluding LaTeX commands, comments, and preamble). State the estimate and which chunking mode applies.
5. If sections contain TODO placeholders or are marked as incomplete, note their existence but do not audit them. Focus on completed text.


## H. CHUNKING RULE

**Priority order** (apply the first rule that matches):

1. **Multi-file papers (3+ files):** Process each module **one file at a time**. Produce output for ONE file, then STOP. Wait for approval before the next file. Within each file, if it exceeds ~5,000 words, apply rule 2 below.
2. **Long content (over ~5,000 words in the current unit):** Process by `\section{}` groups, batching 2-3 sections per response. Stop between batches. If a single section exceeds ~3,000 words, give it its own batch.
3. **Short papers (under ~5,000 words, 1-2 files):** Process the whole paper per module.

**HARD LIMIT: If your output is growing long, STOP and break it into chunks. It is always better to stop early and continue in the next response than to hit the output token limit. Aim for no more than ~40 suggestions per response.**


## I. IMPLEMENTATION RULES: MODULE 1

When I say **"implement correction XX"** (e.g., "implement T1, G3"), apply the change by **directly replacing** the original text with the corrected text.

**Rules:**

1. Show the full corrected line(s) ready to copy-paste into the LaTeX source
2. Preserve all surrounding LaTeX commands and formatting
3. Provide the file name and line number(s) for reference
4. **Do NOT use track-changes markup** (`\rsout{}`, `\red{}`, etc.) — just provide the clean corrected text


## J. IMPLEMENTATION RULES: MODULES 2–4

**IF CLEAN MODE** (`--clean` was specified):
Same as Module 1 — directly replace with clean corrected text. No markup.

**IF TRACK-CHANGES MODE** (default):
Provide the full text with `\rsout{}` and `\red{}` markup ready to copy-paste. Since your suggestions already use this markup, implementation means providing the "Suggested" line with full context.

**Rules:**

1. Show the full implemented line(s) ready to copy-paste into the LaTeX source
2. Preserve all surrounding LaTeX commands and formatting
3. In track-changes mode: only mark the changed portion with `\rsout{}` and `\red{}`
4. Provide the file name and line number(s) for reference


## K. IMPLEMENTATION RULES: MODULES 5–7

When the user says "implement" for items from Modules 5-7, the action depends on the item type:

- **D-items (Design completeness):** Draft the missing content (e.g., a paragraph discussing the identifying assumption) and provide it ready to insert at the specified location.
- **A-items (Argument structure):** Provide the restructured paragraph(s), split, moved, or trimmed as suggested.
- **C-items (Claims-evidence):** Provide the corrected text with the missing reference added, or flag the orphan result for the user to decide.
- **V-items Type A (Voice/craft text-level):** Provide Original/Suggested text pairs as in Modules 2-4, respecting the current mode (track-changes or clean).
- **V-items Type B (Voice/craft structural):** Draft the new content and specify where it should be inserted.
- **AI-items (AI pattern detection):** Provide Original/Suggested text pairs as in Modules 2-4.


---

## Module Router

The 7 modules live in `references/`. For each selected module (per `--modules` or the default sequential workflow), **load the corresponding file and follow its instructions verbatim**. Each module file is self-contained: announcement, rules, checks, output format, stop-and-check.

| Module | File |
|---|---|
| 1. Correctness (typos + grammar) | `references/module-1-correctness.md` |
| 2. Style (syntax + repetition) | `references/module-2-style.md` |
| 3. Prose Quality (McCloskey) | `references/module-3-prose.md` |
| 4. Apparatus (tables, figures, equations, footnotes) | `references/module-4-apparatus.md` |
| 5. Structure & Coherence | `references/module-5-structure.md` |
| 6. Voice & Craft | `references/module-6-voice.md` |
| 7. AI Pattern Detection | `references/module-7-ai-patterns.md` |

**Default workflow:** Run Module 1 first, then prompt to continue. After Module 5 completes, ask whether to run Module 6. After Module 6, ask whether to run Module 7. The stop-and-wait language for each transition lives inside each module file.

**Module 6 prerequisite:** read `~/.claude/preferences/writing-style-guidelines.md` before starting Module 6.

**Module 7 prerequisite:** read `~/.claude/preferences/humanizer-rules.md` and the "Negative Space" section of `~/.claude/preferences/voice-profile-matray.md` before starting Module 7.


---

## Consistency Rules

### Maintaining Consistency Based on User Feedback

**If you provide ANY corrections, clarifications, or modifications to my audit approach at any stop point, I MUST apply those same corrections/modifications consistently to ALL remaining modules and sections in the audit.**

This includes but is not limited to:

- **Classification changes**: If you say "this is not a typo, it's correct terminology," I must not flag similar terminology anywhere else
- **Scope clarifications**: If you say "don't flag X as an issue," I must never flag X in any remaining modules
- **Style preferences**: If you indicate you prefer a certain phrasing or construction, I must not suggest changing it in later sections
- **Technical term exceptions**: If you clarify that a term is field-specific jargon, I must not flag it as a typo or suggest changing it later
- **Grammar conventions**: If you approve a certain grammatical construction, I must apply that standard throughout
- **Threshold adjustments**: If you say "only flag sentences longer than 50 words," I must apply that threshold in all remaining modules

**Before proceeding to each new module, I must review all feedback you have provided on previous modules and ensure I apply it consistently.**

**I must NEVER flag the same issue type again after you have indicated it should not be flagged.**


## General Rules for All Modules

### How to write the audit

- **Think first, write second.** Before writing ANY suggestion, verify it is an actual problem requiring a change.
- **Never withdraw or revise suggestions mid-response.** If you write a suggestion and then realize it's not valid, you have already failed. Do not write "I withdraw this" or "actually this is fine" — those phrases should never appear.
- **Each suggestion you write is final.** If you are unsure whether something is a problem, do not include it.

### What NOT to include in the audit

- **Only report actual problems or changes needed.** Do NOT list things that are already correct.
- Do NOT say things like "this sentence is fine" or "no change needed here"
- If the text is fine, do not mention it at all
- Every item in your audit should be something that requires action or a decision from me
- **NEVER describe your review process.** Do not say "I checked X and it was fine" or "After reviewing, I found no issues with Y"
- **ABSOLUTE PROHIBITION: NEVER suggest using `---` (em dash). Under no circumstances should you propose inserting, adding, or replacing anything with an em dash. This is a hard rule with zero exceptions. Violations of this rule make the entire audit unusable.**

### Technical terms and jargon

Economics and finance papers use field-specific terminology. Do NOT flag as errors:

- Standard econometric terms (heteroskedasticity, endogeneity, etc.)
- Financial terms (yield spread, basis points, etc.)
- Variable names and notation
- Standard abbreviations (OLS, IV, GMM, CAPM, etc.)

If you are unsure whether something is a technical term, do not flag it.

### Consistency rule

When you find ANY error, immediately search the entire document for:

1. The exact same error elsewhere
2. The same pattern applied to different words (e.g., if you find one subject-verb disagreement, re-check ALL subject-verb pairs; if you find "Earning" should be "Earnings", check ALL similar words throughout)

This prevents inconsistent corrections and missed duplicates.

### Second-pass requirement (Module 1 only)

Within Module 1, after your first pass through the document, do a second pass applying the systematic checks below. The first pass catches obvious errors; the second pass catches errors that require careful analysis.

**Second-pass mandatory checks:**
1. Search for every `\citet{` in the document and verify the following verb is third-person singular
2. Check all subject-verb agreement patterns, especially with collective nouns and citation subjects
3. Run the consistency rule (see above) on every error type found in the first pass

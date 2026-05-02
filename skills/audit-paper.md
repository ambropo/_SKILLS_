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

## Module 1: Correctness (Typos + Grammar)

**Before doing any work in this module, print this announcement on its own line:**

> **MODULE 1: CORRECTNESS** — Checking typos, misspelled words, and grammar

**⚠️ RULES REMINDER: (1) No em dashes. (2) Preserve all LaTeX. (3) Only report problems. (4) Never withdraw suggestions. (5) Apply consistency rule. (6) Review all prior user feedback before starting.**

Read through the entire paper and identify all typos, misspelled words, and grammar issues.

Key exclusions: do NOT flag LaTeX commands, extra spaces (LaTeX collapses them), technical terms, proper nouns, stylistic choices, or field-specific conventions.

**MANDATORY grammar check — citation verb agreement:** A `\citet{}` citation refers to a *paper* (singular), not the authors. Every `\citet{...}` followed by a verb MUST use third-person singular: "shows", "finds", "documents", "argues", "formalizes". Flag every instance of `\citet{...} show`, `\citet{...} find`, `\citet{...} argue`, etc. as a grammar error. This is a hard rule with zero exceptions.

**Labeling:**
- Typos: T1, T2, T3, ...
- Grammar: G1, G2, G3, ...

**OUTPUT ORDER REQUIREMENT:**
Group by type: all typos first (## TYPOS), then all grammar issues (## GRAMMAR). Within each group, list in order of appearance (by file, then by line number).

**Report format:**

## TYPOS

**T1** (filename.tex, line XX)

**Original:**
...
**Suggested:**
...

---

[etc.]

## GRAMMAR

**G1** (filename.tex, line XX)

**Original:**
...
**Suggested:**
...

---

[etc.]

If no issues in a category, state "No typos found." or "No grammar issues found."

**STOP and wait for approval.**

When you stop, write:

```
STOPPED - AWAITING YOUR INPUT (Module 1: Correctness)

You may:
- Ask questions or request clarifications
- Tell me which corrections to implement (e.g., "implement T1, T3, G2")
- Say "move to next" to proceed to Module 2
```

**Do NOT proceed to Module 2 until I explicitly say "move to next"**


---

## Module 2: Style (Syntax + Repetition)

**Before doing any work in this module, print this announcement on its own line:**

> **MODULE 2: STYLE** — Syntax improvements and repetition detection

**⚠️ FORMAT REMINDER: Re-read section A (Mode Detection) now. If `--clean` was specified, provide clean corrected text. Otherwise, use `\rsout{}` and `\red{}` markup.**

**⚠️ CHUNKING REMINDER: Re-read section H (Chunking Rule) now.**

**⚠️ RULES REMINDER: (1) No em dashes. (2) Preserve all LaTeX. (3) Only report problems. (4) Never withdraw suggestions. (5) Apply consistency rule. (6) Review all prior user feedback before starting.**

Read through the paper and suggest syntax improvements and flag repetition.

### Light syntax (sentence-level fixes)

- Sentences that are too long and could be split
- Awkward phrasing that has a simple fix
- Unclear sentences where a small rewording helps
- Passive voice that would be clearer as active (only if the fix is simple)
- Word choice improvements (only if clearly better, not just different)

**Constraints:** Do NOT rewrite paragraphs — sentence-level changes only. Do NOT change the author's voice or style. Be conservative — if acceptable, leave it alone.

### Deep syntax (structural improvements)

- Paragraphs that could be restructured for clarity
- Verbose phrases with simple replacements (e.g., "in order to" → "to", "due to the fact that" → "because")
- Weak or vague language (e.g., "makes a lot of sense", "is related to an important dimension")
- Informal language inappropriate for academic writing
- Filler phrases that add nothing (e.g., "It is important to note that")
- Unnecessary hedging (e.g., "We think that..." when you can just state the point)
- Sentences that should be combined or reordered
- Transitions between paragraphs that are missing or weak
- **Subordinate clause discipline:** When a sentence uses "While" or "Because" clauses, the context-setting clause should come first and the main assertion second. Flag sentences where the subordinate clause buries the main point or where clause ordering obscures logical flow.
- **Passive-to-active for contributions:** Sentences stating the paper's contributions should use active, direct construction ("We show that...", "This paper provides..."). Flag passive or hedged contribution statements ("It is shown that...", "This paper aims to provide...").

### Repetition

- **Across the paper**: same idea stated multiple times in different sections
- **Within paragraphs**: same point made twice in slightly different words

When you identify repetition, report all locations, which instance to keep, and suggested deletions or consolidations.

**Labeling:**
- Style suggestions: S1, S2, S3, ... with severity tag `[minor]` or `[substantial]`
- Repetition issues: R1, R2, R3, ...

**OUTPUT ORDER REQUIREMENT:**
- List S suggestions in order of appearance, with sub-headers `### Light Syntax`, `### Deep Syntax`
- List R suggestions separately at the end under `### Repetition`

**Report format:**

**IF TRACK-CHANGES MODE** (default):

## STYLE SUGGESTIONS (Module 2)

### Light Syntax

**S1** [minor] (filename.tex, line XX)

**Original:**
This sentence is awkward and hard to follow.
**Suggested:**
This sentence \rsout{is awkward and hard to follow}\red{reads more clearly now}.
**Reason:** [brief explanation]

---

### Deep Syntax

**S4** [substantial] (filename.tex, line XX)

**Original:**
This sentence is too wordy and could be tightened up significantly.
**Suggested:**
This sentence \rsout{is too wordy and could be tightened up significantly}\red{could be tightened}.
**Reason:** [explanation]

### Repetition

R1: [Brief description of what is repeated]
- Location 1: filename.tex, line XX — `[text]`
- Location 2: filename.tex, line YY — `[text]`
Suggestion: Keep location 1, delete location 2.

**IF CLEAN MODE** (`--clean` was specified):
Same structure, but use clean corrected text instead of `\rsout{}`/`\red{}` markup.

If no suggestions, state "No style suggestions" and/or "No repetition issues found."

**STOP and wait for approval.**

When you stop, write:

```
STOPPED - AWAITING YOUR INPUT (Module 2: Style)

You may:
- Ask questions or request clarifications
- Tell me which corrections to implement (e.g., "implement S1, S4, R1")
- Say "move to next" to proceed to Module 3 (Prose Quality)
- Say "done" to end the audit here
```

**Do NOT proceed to Module 3 until I explicitly say "move to next"**


---

## Module 3: Prose Quality (Optional — McCloskey Checks)

**Before doing any work in this module, print this announcement on its own line:**

> **MODULE 3: PROSE QUALITY** — Sentence-level checks and economics writing patterns

**⚠️ FORMAT REMINDER: Re-read section A (Mode Detection) now. If `--clean` was specified, provide clean corrected text. Otherwise, use `\rsout{}` and `\red{}` markup.**

**⚠️ RULES REMINDER: (1) No em dashes. (2) Preserve all LaTeX. (3) Only report problems. (4) Never withdraw suggestions. (5) Apply consistency rule. (6) Review all prior user feedback before starting.**

This module is offered after Module 2 completes. It targets sentence-level prose problems that are pervasive in economics writing, based on McCloskey's *Economical Writing*. Module 2 already catches verbose phrases, filler, and simple passive voice. This module goes deeper: it looks for patterns that individually seem minor but collectively make prose heavy and hard to read.

**Do NOT re-flag** anything already caught by Module 2. If you flagged "in order to" as S7, do not flag it again here.

### Always check

- **Nominalizations.** Verbs turned into nouns with "is/was" ("an estimation was performed" → "we estimated"; "the implementation of the policy" → "implementing the policy"). Flag only when multiple instances accumulate in a section; pick the worst 3-5 per section.

- **"There is/It is" openers.** Empty subject constructions that bury the real subject ("There is evidence that X" → "The evidence shows X"; "It is worth noting that Y" → "Y"). Flag the clearest cases where the real subject appears later in the sentence.

- **Bare "this/that."** Bare pronoun without a following noun ("This shows..." → "This result shows..."; "That is important" → "That finding is important"). Flag only when the referent is genuinely ambiguous across sentence boundaries. Do NOT flag "this/that + noun" constructions ("this coefficient", "that specification"), which are correct.

- **Metadiscourse.** Narrating the paper's own structure instead of delivering content ("In this section, we will show..." → just show it; "We now turn to..." → start the content; "The remainder of the paper is organized as follows" → cut or move to a footnote). One roadmap sentence in the introduction is fine; flag when it recurs throughout.

- **Dead phrases.** Filler that adds zero meaning: "due to the fact that" (→ "because"), "the process of" (delete), "it is important to note that" (delete), "plays an important role" (→ specify what it does), "a number of" (→ "several" or give the number). Flag when they accumulate; one stray instance in a long paper is not worth reporting.

### Check if prominent

These require judgment. Flag only when clearly harmful, not every instance.

- **Elegant variation.** Using different words for the same concept across sentences to avoid repetition ("firms" then "companies" then "enterprises" for the same entities). In economics, precision matters: one concept, one word. Flag only when the variation could genuinely confuse the reader about whether two different things are meant.

- **Weak sentence openers.** Sentences starting with "However," "Moreover," "Furthermore," "Additionally" in quick succession. One transitional adverb is fine; three in a row signals autopilot prose. Flag clusters, not individual instances.

- **Adjective/adverb pileup.** Stacking modifiers when fewer would be stronger ("very significantly positively correlated" → "strongly correlated"; "highly statistically significant" → "significant at the 1\% level"). Flag when the modifiers add no precision.

### Paragraph discipline

Focus on the **introduction, contribution, and results sections** only. Skip data descriptions, robustness sections, and appendices. Flag only when a paragraph clearly fails one of these checks.

- **No governing point.** The first sentence of a paragraph should state its purpose. Flag paragraphs where the opener is a detail, a citation, or a subordinate clause rather than a claim or orientation. *Bad:* "\citet{Smith2020} studies credit supply shocks in emerging markets using a novel instrument..." (literature detail as opener). *Good:* "Our identification strategy exploits variation in bank exposure to the 2008 crisis." (states the paragraph's point). Suggest moving the key claim to the first sentence.

- **Contribution by literature listing.** The contribution section should state what *this* paper does, not what others have done. Flag contribution paragraphs where cited papers are the grammatical subjects of most sentences ("Author A studies X. Author B studies Y. We study Z."). *Fix:* Lead with "We [verb]..." and subordinate the citations ("Unlike \citet{A}, who focus on X, we examine Y").

- **Results before setup.** Before presenting numbers, the reader needs to know: what comparison is being made, and what sign or magnitude would support the hypothesis. Flag results paragraphs that open with "The coefficient is..." or "Column (3) shows..." without first stating the expected pattern. *Good structure:* (1) what we test, (2) what to expect, (3) what we find.

### Economics-specific writing failures

Flag these only when clearly present. Each requires judgment; do not over-flag.

- **Motivating without asking.** The introduction spends multiple paragraphs on why a topic matters without stating the precise research question. Flag if you reach the contribution paragraph without finding a clear question or statement of what the paper does. One motivation paragraph is fine; three before the question is a problem.

- **Mechanism named but not explained.** The paper invokes a mechanism ("moral hazard," "the collateral channel," "general equilibrium effects") as an explanation without describing *how* it operates in the specific context. Flag when a mechanism label is used as if self-explanatory, with no sentence describing the causal steps. Do NOT flag when the mechanism has been explained earlier in the paper. *Bad:* "The effect operates through the collateral channel." (named, not explained). *Good:* "When house prices fall, firms with real estate collateral lose borrowing capacity, reducing investment. This collateral channel explains..."

- **Empty importance claims.** Words like "important," "novel," "rich," "unique," "first" used without substantive backing. Flag when the claim is asserted rather than demonstrated. *Bad:* "This is an important contribution to the literature." *Good:* "This is the first paper to measure X using Y, which allows us to distinguish between Z and W." Do NOT flag when the claim is followed by a concrete reason. Note: this is distinct from Module 3's "dead phrases" check, which targets filler words. This check targets substantive claims that lack substance.

- **Interpretation-result conflation.** Sliding from a regression coefficient to a welfare or policy statement without flagging the interpretive leap. Flag when a paragraph moves from "the coefficient is X" to "therefore policy Y is optimal" without discussing the assumptions needed for that step. *Fix:* Suggest separating the empirical finding from the interpretation with explicit caveats ("Under the assumption that..., this estimate implies...").

### Constraints

- **Preserve the author's meaning** — rewrites must convey the same information
- **Preserve technical accuracy** — do not simplify in ways that lose precision
- **Preserve academic tone** — do not make the writing casual
- **Preserve ALL LaTeX formatting** — every `\underline{}`, `\textit{}`, `\autoref{}`, `\cite{}`, and other LaTeX command in the original MUST appear in the rewritten version (unless the edit specifically intends to remove it)
- **Flag uncertainty** — if a rewrite might alter meaning, note this explicitly
- **Prioritize density over count** — a section with 10 nominalizations in 3 paragraphs deserves 3 suggestions showing the pattern, not 10 individual flags

**Labeling:** Label each suggestion as P1, P2, P3, ... (P for Polish)

**Report format:**

**IF TRACK-CHANGES MODE** (default):

## PROSE QUALITY (Module 3)

**P1** (filename.tex, lines XX-YY)

**Original:**
An estimation of the treatment effect was performed using a difference-in-differences approach.
**Suggested:**
\rsout{An estimation of the treatment effect was performed using}\red{We estimated the treatment effect with} a difference-in-differences approach.
**Reason:** Nominalization. "An estimation was performed" → "We estimated."

---

**IF CLEAN MODE** (`--clean` was specified):

**P1** (filename.tex, lines XX-YY)

**Original:**
An estimation of the treatment effect was performed using a difference-in-differences approach.
**Suggested:**
We estimated the treatment effect with a difference-in-differences approach.
**Reason:** Nominalization. "An estimation was performed" → "We estimated."

---

If no suggestions, state "No prose quality issues found" — but this should be rare in a multi-page economics paper.

**STOP and wait for approval.**

When you stop, write:

```
STOPPED - AWAITING YOUR INPUT (Module 3: Prose Quality)

You may:
- Ask questions or request clarifications
- Tell me which corrections to implement (e.g., "implement P1, P4, P7")
- Say "move to next" to proceed to Module 4 (Apparatus)
- Say "done" to end the audit here
```

**Do NOT proceed to Module 4 until I explicitly say "move to next"**


---

## Module 4: Apparatus (Optional — Tables, Figures, Equations, Footnotes)

**Before doing any work in this module, print this announcement on its own line:**

> **MODULE 4: APPARATUS** — Tables, figures, equations, and footnotes

**⚠️ FORMAT REMINDER: Re-read section A (Mode Detection) now. If `--clean` was specified, provide clean corrected text. Otherwise, use `\rsout{}` and `\red{}` markup.**

**⚠️ RULES REMINDER: (1) No em dashes. (2) Preserve all LaTeX. (3) Only report problems. (4) Never withdraw suggestions. (5) Apply consistency rule. (6) Review all prior user feedback before starting.**

This module is offered after Module 3 completes. It audits the paper's apparatus (tables, figures, displayed equations, and footnotes) for standalone clarity. Readers often scan tables and figures before reading the text; titles and captions must carry meaning on their own. Equations need prose interpretation. Footnotes should not carry the paper's real argument.

**Boundary with Module 3:** Module 4 checks apparatus *structure* (whether a title states a finding, whether a caption is self-contained, whether an equation is interpreted). Module 3 checks *prose quality within* the text. If a sentence near a table has a nominalization, that is a Module 3 issue, not Module 4. **Do NOT re-flag** anything already caught by Modules 2-3.

### Table and figure titles

- **Titles that state the method, not the finding.** Titles like "Main Results," "Regression Results," or "Robustness Checks" force the reader to study the table to learn what it shows. Flag tables whose titles do not convey the key finding or content. *Bad:* `\caption{Main Results}`. *Good:* `\caption{Firms with higher leverage invest less after credit shocks}`. For summary statistics tables, a descriptive title is acceptable ("Summary statistics for firm-level variables"). For robustness tables, stating what is being tested is sufficient ("Results are robust to alternative clustering").

- **Figure captions too vague to read standalone.** A reader should understand what a figure shows from its caption alone, without reading surrounding text. Flag captions that say only "Distribution of X" or "Time series of Y" without stating the takeaway, sample, or units. *Bad:* `\caption{Distribution of firm size}`. *Good:* `\caption{Distribution of firm size (log employees). The distribution is right-skewed, with a median of 50 employees. Sample: Compustat firms, 2000-2015.}`

### Displayed equations

- **Equations without prose interpretation.** Every displayed equation should be accompanied by prose explaining what it means: what each key term represents and what the equation captures economically. Flag equations where the surrounding text says only "We estimate the following:" followed by the equation and then immediately moves to results. *Fix:* Suggest adding a sentence interpreting the key terms ("where $Y_{it}$ is firm $i$'s investment in year $t$, and $D_i$ is the treatment indicator, so $\beta_1$ captures the differential investment response").

### Footnotes

- **Footnotes carrying the real argument.** Footnotes should contain supplementary information, not essential reasoning. Flag footnotes that contain: key identification assumptions, main mechanism explanations, important caveats that affect interpretation of results, or significant empirical results. *Fix:* Suggest promoting the content to the main text in condensed form. Do NOT flag footnotes that contain data source descriptions, technical estimation details, or minor clarifications; those belong in footnotes.

### Economic magnitude translation

- **Coefficients reported without economic interpretation.** When tables report regression coefficients, check whether the surrounding text translates key estimates into economically interpretable magnitudes (dollar terms, percentage of a standard deviation, comparison to a familiar benchmark). Flag results sections where coefficients are discussed only in terms of statistical significance without economic magnitude. *Bad:* "The coefficient on treatment is 0.034 and significant at the 1% level." *Good:* "The coefficient implies that treated firms increase investment by 3.4 percentage points, equivalent to approximately $2.1 million at the sample mean." This check applies to headline results only, not every coefficient in every robustness table.

### Constraints

- **Do NOT audit content** — this module checks presentation clarity, not whether the empirical strategy is valid
- **Preserve the author's meaning** — title/caption suggestions must accurately reflect the table/figure content
- **Be conservative with equation interpretation** — suggest adding prose, not changing the equation
- **Preserve ALL LaTeX formatting** — `\caption{}`, `\label{}`, `\footnote{}`, table environments, etc.
- **Skip appendix tables/figures** unless they are referenced prominently in the main text

**Labeling:** Label each suggestion as Q1, Q2, Q3, ... (Q for Quality of apparatus)

**Report format:**

**IF TRACK-CHANGES MODE** (default):

## APPARATUS (Module 4)

**Q1** (filename.tex, lines XX-YY)

**Original:**
\caption{Main Results}
**Suggested:**
\caption{\rsout{Main Results}\red{Higher leverage reduces investment after credit shocks}}
**Reason:** Table title states the method, not the finding.

---

**IF CLEAN MODE** (`--clean` was specified):

**Q1** (filename.tex, lines XX-YY)

**Original:**
\caption{Main Results}
**Suggested:**
\caption{Higher leverage reduces investment after credit shocks}
**Reason:** Table title states the method, not the finding.

---

If no suggestions, state "No apparatus issues found."

**STOP and wait for approval.**

When you stop, write:

```
STOPPED - AWAITING YOUR INPUT (Module 4: Apparatus)

You may:
- Ask questions or request clarifications
- Tell me which corrections to implement (e.g., "implement Q1, Q3")
- Say "move to next" to proceed to Module 5 (Structure & Coherence)
- Say "done" to end the audit here
```

**Do NOT proceed to Module 5 until I explicitly say "move to next"**


---

## Module 5: Structure & Coherence (Optional)

**Before doing any work in this module, print this announcement on its own line:**

> **MODULE 5: STRUCTURE & COHERENCE** — Argument structure, claims-evidence mapping, and design completeness

**⚠️ FORMAT REMINDER: Re-read section A (Mode Detection) now. If `--clean` was specified, provide clean corrected text. Otherwise, use `\rsout{}` and `\red{}` markup.**

**⚠️ RULES REMINDER: (1) No em dashes. (2) Preserve all LaTeX. (3) Only report problems. (4) Never withdraw suggestions. (5) Apply consistency rule. (6) Review all prior user feedback before starting.**

This module is offered after Module 4 completes. It checks paper-level structural coherence: whether the empirical design is complete for the paper type, whether each paragraph serves a clear purpose, and whether text claims and empirical results are properly linked. Unlike Modules 2-4 (which work at the sentence or element level), this module reads the paper as a whole.

**Do NOT re-flag** anything already caught by Modules 2-4.


### 5a. Paper-type classification and design-specific completeness

Classify the paper into one of four types:

| Type | Signature |
|------|-----------|
| **Reduced-form** | DiD, IV, RDD, synthetic control, event study |
| **Structural** | Calibrated/estimated model with counterfactuals |
| **Theory + empirics** | Model with testable predictions taken to data |
| **Descriptive** | New facts, measurement, stylized facts |

State the classification before proceeding. If the paper spans types (e.g., a structural model estimated on reduced-form moments), note both and apply both checklists.

Then check for the design elements that a referee would expect for that paper type. Flag only what is **missing**. Do not describe items that are present.

**Reduced-form:**
- Explicit discussion of the identifying assumption (parallel trends for DiD, exclusion restriction for IV, no manipulation for RDD)
- Estimand clearly defined (ATE, ATT, LATE, ITT)
- Pre-trends or placebo tests discussed
- For IV: LATE interpretation discussed (not just "the effect of X")
- For DiD: treatment timing variation discussed if staggered

**Structural:**
- Model environment clearly described before estimation
- Identified vs. calibrated parameters distinguished
- Identification discussed (what data variation pins down each parameter vs. functional form assumptions)
- Convergence diagnostics mentioned for estimation
- Counterfactual exercises clearly separated from estimation results
- Model fit assessed before running counterfactuals

**Theory + empirics:**
- Testable predictions stated explicitly and numbered/listed
- For each prediction, empirical test clearly mapped
- Whether predictions can distinguish this theory from alternatives (flag if predictions are consistent with multiple models and this is not discussed)
- Test power discussed (could the data reject the prediction if false?)

**Descriptive:**
- Construct/measure clearly defined and validated
- Measurement error discussed
- Key facts stated explicitly (not buried in tables)
- Representativeness of the sample discussed

**Labeling:** D1, D2, D3, ... (D for Design)


### 5b. Argument-move audit

Check every section of the paper (not just intro/contribution/results as in Module 3's paragraph discipline). Each paragraph in the body should have a single identifiable purpose:

- Motivation / setup
- Claim / finding statement
- Evidence presentation
- Mechanism explanation
- Qualification / limitation
- Transition / roadmap

Flag paragraphs that:

- **Serve no clear purpose** — the paragraph exists but does not advance the argument
- **Try to do too many things** — e.g., presents a result, explains a mechanism, and qualifies it all at once
- **Are in the wrong place** — e.g., a limitation buried mid-results instead of grouped with other limitations
- **Empirical sections that do not open with their question.** Each empirical section or subsection should open by stating the question it answers or the hypothesis it tests. Flag sections that jump straight into specification or results without first orienting the reader. *Bad:* "Column (3) shows the effect of..." as a section opener. *Good:* "We now ask whether the treatment effect varies across firm size."

Be conservative. Most paragraphs will be fine. Only flag clear structural problems where a reader would lose the thread. Do NOT flag every paragraph — that defeats the purpose.

**Labeling:** A1, A2, A3, ... (A for Argument)


### 5c. Claims-evidence mapping

Check the bidirectional link between text claims and empirical results.

**Text → Results:** For every empirical claim in the text (e.g., "we find that X increases Y by Z%"), verify it points to a specific table, column, or figure. Flag claims that:

- Reference no table/figure at all
- Reference a table/figure that does not contain the claimed result
- State magnitudes or significance levels not visible in the referenced output

**Results → Text:** For every table and figure in the main paper, verify it is discussed in the text. Flag:

- Tables/figures never referenced in the text (orphan results)
- Tables/figures referenced but whose key results are not interpreted in the text

**Labeling:** C1, C2, C3, ... (C for Claims)


### Output format

**Report format:**

## STRUCTURE & COHERENCE (Module 5)

**Paper type:** [classification and brief justification]

### Design Completeness

**D1** (filename.tex, section X)

**Missing:** [what is absent and where a referee would expect it]
**Reason:** [why this matters for this paper type]

---

### Argument Structure

**A1** (filename.tex, lines XX-YY)

**Original:**
[the paragraph or its opening sentence]
**Issue:** [serves no clear purpose / tries to do too many things / wrong location]
**Suggestion:** [how to fix — split, move, or cut]

---

### Claims-Evidence Mapping

**C1** (filename.tex, line XX)

**Claim:** [the text claim]
**Issue:** [no reference / reference does not match / magnitude not in table]

---

If no issues in a sub-section, state "No [design / argument / claims-evidence] issues found."

**STOP and wait for final approval.**

When you stop, write:

```
STOPPED - AWAITING YOUR INPUT (Module 5: Structure & Coherence)

You may:
- Ask questions or request clarifications
- Tell me which items to address (e.g., "implement D1, A3, C2")
- Say "move to next" to proceed to Module 6 (Voice & Craft)
- Say "done" to end the audit here
```

**Do NOT proceed to Module 6 until I explicitly say "move to next"**

**When Module 5 is complete (after implementing any requested changes), ask:**

> "The core inspections are done. Do you want me to run Module 6 for a holistic craft review based on your writing style guidelines? (Module 7, AI pattern detection, can follow after.)"

If the answer is **yes**, proceed to Module 6. If **no**, end the audit.


---

## Module 6: Voice & Craft (Optional)

**Before doing any work in this module, print this announcement on its own line:**

> **MODULE 6: VOICE & CRAFT** — Holistic craft review based on writing style guidelines

**⚠️ FORMAT REMINDER: Re-read section A (Mode Detection) now. If `--clean` was specified, provide clean corrected text. Otherwise, use `\rsout{}` and `\red{}` markup.**

**⚠️ RULES REMINDER: (1) No em dashes. (2) Preserve all LaTeX. (3) Only report problems. (4) Never withdraw suggestions. (5) Apply consistency rule. (6) Review all prior user feedback before starting.**

This module is optional. Only proceed if the user says "move to next" or answers "yes" to the Module 6 question.

**MANDATORY: Before starting this module, read the reference file `~/.claude/commands/prompt-references/writing-style-guidelines.md` and use it as your checklist.** Do not work from memory of what the guidelines say. Read the file.

This module evaluates the paper's craft at a holistic level: opening strategy, pedagogical flow, narrative arc, and voice. Unlike Modules 2-5 (which check discrete elements), this module reads the paper as a unified argument and assesses whether it follows best practices in applied economics writing.

**Do NOT re-flag** anything already caught by Modules 1-5.

### 6a. Opening strategy

Check the introduction for:

- **Practical problem first.** Does the introduction open with a concrete research problem or empirical puzzle, or does it open with a literature review or abstract motivation? Flag introductions that take more than two paragraphs to reach the research question.
- **Contribution structure.** Are contributions stated clearly and affirmatively ("We show that...", "Our contribution is twofold. First,...")? Flag vague or passive contribution statements.
- **Answer preview.** Are the main findings stated in the introduction (not just the question)? Flag introductions where the reader reaches the roadmap paragraph without knowing the paper's answer.
- **Explicit roadmap.** Is there a clear paper organization statement? The reader should never question the direction of argument.

### 6b. Pedagogical flow

- **Intuition before formalism.** Does the paper provide a motivating example or simple case before the general framework? Flag papers that jump from introduction directly to the most general version of the model or estimator.
- **Simple-to-complex progression.** When the paper builds a framework, does it start with the simplest case (e.g., 2x2, binary treatment) before generalizing? Flag sections where the general case is presented without a simpler lead-in.
- **Running example.** Does the paper use a recurring example to build continuity? This is not required, but if an example is introduced early, check that it is referenced in later sections to help readers track concepts.
- **"To see this" and direct reader engagement.** Does the paper address anticipated reader questions ("To see this, note that...", "Why?")? This is a sign of strong pedagogical writing. Its absence is not an error, but note if the paper is dense and could benefit from these devices.

### 6c. Results presentation craft

- **Visual results first.** Do results sections lead with figures before tables where possible? Flag sections that present only table-based results when a figure could communicate the main finding more effectively.
- **Results-before-setup.** Before presenting numbers, does the text establish what comparison is being made and what sign/magnitude would support the hypothesis? Flag results paragraphs that open with "The coefficient is..." or "Column (3) shows..." without first stating the expected pattern. Good structure: (1) what we test, (2) what to expect, (3) what we find.

### 6d. Literature positioning

- **Constructive framing.** Does the paper position itself constructively relative to prior work ("Our paper expands on X in three key ways") rather than competitively ("Unlike X, who fail to account for...")? Flag adversarial framing.
- **Credit and contrast.** Does the paper give credit generously while clearly distinguishing its own contribution? Flag places where the distinction between this paper and prior work is vague.

### Constraints

- **This module evaluates craft, not correctness.** Suggestions are about making the paper more effective, not fixing errors.
- **Be conservative.** Only flag patterns that clearly weaken the paper. Not every paper needs a running example or a 2x2 lead-in.
- **Preserve the author's voice.** Do not suggest rewrites that impose a different writing personality. Flag structural issues and let the author decide how to address them.
- **Preserve ALL LaTeX formatting.**

**Labeling:** V1, V2, V3, ... (V for Voice/craft)

### Two suggestion types

Module 6 suggestions fall into two categories. Use the right format for each:

**Type A — Text-level suggestions (rewritable).** When the issue can be fixed by rewriting specific sentences or paragraphs (e.g., a passive contribution statement, a missing answer preview, an adversarial literature framing, a results paragraph that skips setup), provide concrete Original/Suggested text pairs, just like Modules 2-5. These are the majority of suggestions.

**Type B — Structural suggestions (not rewritable).** When the issue requires adding new content that does not yet exist in the paper (e.g., "add a motivating example in Section 2," "add a roadmap paragraph," "lead with a figure before the table"), use the Issue/Suggestion/Reference format. Provide as much concrete guidance as possible: where the new content should go, what it should contain, and a draft if feasible.

**Default to Type A.** If you can point to specific text and propose a rewrite, do so. Only use Type B when there is genuinely no existing text to revise.

**Report format:**

**IF TRACK-CHANGES MODE** (default):

## VOICE & CRAFT (Module 6)

**V1** (filename.tex, lines XX-YY) — *Type A*

**Original:**
The introduction opens with a literature review that delays the research question.
**Suggested:**
The introduction \rsout{opens with a literature review that delays the research question}\red{now leads with the concrete problem}.
**Reason:** [brief explanation]
**Reference:** [which section of the writing style guidelines this relates to]

---

**V2** (filename.tex, section X) — *Type B*

**Issue:** [description of the structural craft problem]
**Suggestion:** [what to add, where to add it, and a draft of the new content if feasible]
**Reference:** [which section of the writing style guidelines this relates to]

---

**IF CLEAN MODE** (`--clean` was specified):
Same structure, but Type A suggestions use clean corrected text instead of `\rsout{}`/`\red{}` markup.

If no suggestions, state "No voice/craft issues found."

**STOP and wait for approval.**

When you stop, write:

```
STOPPED - AWAITING YOUR INPUT (Module 6: Voice & Craft)

You may:
- Ask questions or request clarifications
- Tell me which items to address (e.g., "implement V1, V3")
- Say "move to next" to proceed to Module 7 (AI Pattern Detection)
- Say "done" to end the audit here
```

**Do NOT proceed to Module 7 until I explicitly say "move to next"**

**When Module 6 is complete (after implementing any requested changes), ask:**

> "Do you want me to run Module 7 to scan for AI-sounding language patterns (based on your humanizer anti-AI framework)?"

If the answer is **yes**, proceed to Module 7. If **no**, end the audit.


---

## Module 7: AI Pattern Detection (Optional)

**Before doing any work in this module, print this announcement on its own line:**

> **MODULE 7: AI PATTERN DETECTION** — Scanning for language that signals AI-generated text

**⚠️ FORMAT REMINDER: Re-read section A (Mode Detection) now. If `--clean` was specified, provide clean corrected text. Otherwise, use `\rsout{}` and `\red{}` markup.**

**⚠️ RULES REMINDER: (1) No em dashes. (2) Preserve all LaTeX. (3) Only report problems. (4) Never withdraw suggestions. (5) Apply consistency rule. (6) Review all prior user feedback before starting.**

This module is optional. Only proceed if the user says "move to next" or answers "yes" to the Module 7 question.

**MANDATORY: Before starting this module, read the reference files `~/.claude/commands/prompt-references/humanizer-rules.md` and the "Negative Space" section of `~/.claude/commands/prompt-references/voice-profile-matray.md`. Use them as your checklist.** Do not work from memory of what the rules say. Read the files.

This module scans the paper for language patterns that are characteristic of AI-generated text. These patterns may have entered the paper through AI-assisted drafting, or they may simply be habits that happen to overlap with AI style. Either way, a referee or reader who notices them will question the paper's authenticity. The goal is to flag every instance so the author can decide which to revise.

**Do NOT re-flag** anything already caught by Modules 1-6. If a filler phrase was caught as S7 in Module 2, do not flag it again here. This module catches AI-specific patterns that earlier modules were not designed to detect.

### 7a. AI vocabulary

Flag any use of words and phrases that are strongly associated with AI-generated text. These words are rarely found in published economics papers and their presence is a red flag:

- **Core AI words:** delve, foster, garner, interplay, intricate, landscape, tapestry, underscore, pivotal, showcase, testament, enhance, align with, crucial, additionally, furthermore, moreover (when used as sentence openers in clusters), multifaceted, nuanced (as filler adjective), comprehensive (when not describing data coverage), robust (when not describing a statistical test)
- **Copula substitutes:** "serves as," "stands as," "boasts," "features" used where "is" or "has" would be natural. AI text avoids simple copulas. Economics papers should not. Flag every instance where the simpler verb works.

For each flagged word, suggest a concrete replacement that sounds like a human economist wrote it. Often the fix is simply deleting the word or using a plain alternative.

### 7b. Significance inflation

Flag phrases that inflate the importance of findings or concepts beyond what the evidence supports, using characteristic AI amplification patterns:

- "plays a pivotal/crucial/vital role" (say what it actually does)
- "testament to" (just state the evidence)
- "setting the stage for" (state the relationship directly)
- "sheds light on" when overused (once is fine; three times signals AI)
- "paving the way for" (state the implication directly)

These phrases substitute rhetorical weight for actual content. The fix is almost always to replace the inflated phrase with a concrete statement of what happens.

### 7c. Superficial -ing phrases

AI text uses present participles as cheap connectors that sound analytical but add nothing:

- "highlighting the importance of..."
- "underscoring the need for..."
- "emphasizing the role of..."
- "reflecting the broader trend of..."
- "showcasing the potential of..."
- "demonstrating the significance of..."

Flag these when they appear as sentence-level connectors or clause openers. The fix is to state the connection directly ("This result implies..." or "The data confirm...") or to cut the phrase entirely if the connection is already clear from context.

### 7d. Promotional and editorial language

Flag adjectives and adverbs that editorialize findings rather than letting the reader judge:

- **Promotional:** vibrant, breathtaking, groundbreaking, nestled, renowned, stunning, remarkable, extraordinary
- **Editorializing results:** striking, impressive, surprising, dramatic, notable, intriguing (when used to describe empirical findings)

In economics papers, state the result and its magnitude. "The coefficient is 0.15, three times larger than the OLS estimate" lets the reader decide it is striking. Writing "The striking coefficient of 0.15" does the reader's job for them and sounds like AI.

One exception: "novel" is acceptable when backed by a concrete reason ("novel because no prior dataset links X to Y"), but not as a bare assertion ("a novel contribution").

### 7e. Structural AI patterns

These are organizational and rhetorical patterns that AI defaults to:

- **Rule of three.** AI text forces ideas into groups of three ("First... Second... Third..." or "X, Y, and Z") even when the content does not naturally divide that way. Flag instances where a list of three feels forced or where two items would suffice. (Note: genuine three-part contribution statements are fine when each part is substantive.)
- **Negative parallelisms.** "It's not just X; it's Y" or "It's not merely X but also Y." This is a rhetorical tic AI uses constantly. Economics papers state what something is, not what it is not.
- **False ranges.** "From X to Y" where X and Y are not endpoints of a meaningful scale. AI uses these to sound comprehensive. Flag when the range is decorative rather than informative.
- **Synonym cycling.** Using different words for the same concept across consecutive sentences to avoid repetition ("firms," then "companies," then "enterprises"). In economics, precision requires one concept, one word. (This overlaps with Module 3's elegant variation check; flag here only if not already caught.)

### 7f. Filler, hedging, and throat-clearing

Flag patterns that pad the text without adding information:

- **Filler phrases:** "it is important to note that" (delete), "it is worth mentioning that" (delete), "in the context of" (usually deletable), "it should be noted that" (delete)
- **Excessive hedging clusters:** "could potentially possibly" or stacking multiple qualifiers ("may perhaps suggest"). One hedge per claim is fine; two or more signals AI uncertainty rather than scientific caution.
- **Generic conclusions:** "the future looks bright," "exciting times ahead," "opens up new avenues for future research" (if the paper actually names specific future research, that is fine; flag only the generic version)

### 7g. Formatting and punctuation tells

- **Em dashes.** Already banned in your style, but double-check the entire document. Flag every `---` (em dash). For `--` (en dash), flag only if used between clauses rather than in a number range (e.g., "2000--2015" is fine).
- **Boldface overuse.** AI text mechanically bolds key terms. Flag if terms are bolded beyond first-use definitions.
- **Curly quotes.** Check for `\textquoteleft`/`\textquoteright` or Unicode curly quotes that may have leaked from AI output into LaTeX source.

### 7h. Chat artifacts and tone

Flag any residual AI chat patterns that may have survived editing:

- "I hope this helps" or similar service-oriented phrases
- "Great question" or "Excellent point" (should never appear in a paper, but check footnotes and author notes)
- "As of [date]" or "based on available information" (knowledge disclaimers)
- "Let's explore" or "let's dive in" (signposting)
- Any phrase that reads as Claude/ChatGPT talking to the user rather than an author writing to a reader

### Constraints

- **Flag density, not exhaustiveness.** If a word appears 15 times, flag the first 3 instances and note "X more instances throughout the paper." Do not list every occurrence.
- **Context matters.** "Robust" in "robust standard errors" is fine. "Robust" in "a robust framework for understanding" is AI. Use judgment.
- **Preserve the author's meaning.** Every suggestion must convey the same information as the original.
- **Preserve ALL LaTeX formatting.**
- **Do NOT re-flag** anything caught in Modules 1-6.

**Labeling:** AI1, AI2, AI3, ... with a category tag: `[vocab]`, `[inflation]`, `[ing-phrase]`, `[editorial]`, `[structural]`, `[filler]`, `[formatting]`, `[chat]`

**Report format:**

**IF TRACK-CHANGES MODE** (default):

## AI PATTERN DETECTION (Module 7)

**AI1** [vocab] (filename.tex, line XX)

**Original:**
This paper delves into the relationship between credit supply and firm investment.
**Suggested:**
This paper \rsout{delves into}\red{examines} the relationship between credit supply and firm investment.
**Reason:** "Delve" is strongly associated with AI-generated text. "Examines" is standard academic usage.
**Rule:** Humanizer Rules, #7 (AI vocabulary)

---

**AI2** [editorial] (filename.tex, line XX)

**Original:**
The striking coefficient of 0.15 suggests a dramatic shift in firm behavior.
**Suggested:**
The coefficient of 0.15\rsout{, striking in magnitude,} suggests a \rsout{dramatic} shift in firm behavior.
**Reason:** "Striking" and "dramatic" editorialize the finding. State the magnitude and let the reader judge.
**Rule:** Humanizer Rules, Editorializing Results

---

**IF CLEAN MODE** (`--clean` was specified):
Same structure, but use clean corrected text instead of `\rsout{}`/`\red{}` markup.

If no suggestions, state "No AI patterns detected." (Unlikely if any part of the paper was AI-assisted.)

**STOP — PAPER AUDIT COMPLETE.**

When you stop, write:

```
PAPER AUDIT COMPLETE (Modules 1-7)

You may:
- Ask questions or request clarifications
- Tell me which items to address (e.g., "implement AI1, AI4, AI7")
- Request additional review of specific sections
```


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

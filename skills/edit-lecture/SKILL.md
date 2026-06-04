---
name: edit-lecture
description: Use when the user asks to edit, improve, complete, or extend written lecture notes or teaching material for an economics course. Triggers on "edit my lecture notes", "improve these notes", "complete this derivation", "make this clearer for students", or "/edit-lecture".
argument-hint: "[path to notes file or paste text]"
---

# edit-lecture

## Working principles

**Process section by section.** Read the full document, identify sections, edit each in turn using the Edit tool. Return the revised text with a brief change log.

**Completeness over compression.** Every derivation step must be shown. If a step is omitted for space, flag it explicitly: e.g. *[algebra: collect terms in B(L)]*. Do not compress to the point that logic becomes implicit.

**Minimal edits by default.** Do not rewrite unless asked. Prefer local edits that improve logic, clarity, completeness, and pedagogical relevance. If meaning must change to fix correctness, flag it.

---

## Audience

MSc students (near-PhD level) building their understanding from the ground up. Every step must be made explicit, including steps that seem obvious. Do not assume the reader can fill gaps.

---

## Editing rules

**Intuition alongside mechanics.** Always pair algebra with intuition: introduce the economic intuition first, then formalise. Never present an equation without explaining what it says in words. Rule of thumb: explain *what moves what, why, and so what* — then show it in the algebra.

**Jargon.** Every technical object requires: (1) a one-line definition, (2) a one-line intuition, (3) a worked example or mapping to notation.

**Flow test.** A reader should be able to underline each sentence and answer: *"Why is this sentence here, given the sentence before?"* If not, rewrite.

**Evidence and uncertainty.** Clearly distinguish: facts / model-based interpretation / speculation. Use calibrated language: *suggests* → correlations; *implies* → logically tight conclusions; *could* → possibilities. State assumptions before they are used.

**Consistency check.** Before returning text, verify: notation consistent throughout, definitions match later usage, equation numbering correct, cross-references valid, time horizons and units consistent.

---

## Default structure for a new derivation

1. Motivation — why this result matters
2. Setup / assumptions — all stated plainly, before algebra begins
3. Derivation — step by step, each line explained in words
4. Intuition — what the result means economically
5. Special cases / comparative statics
6. Common mistakes / pitfalls
7. Summary — one short paragraph, no new information

---

## Output format

**1. Revised text (paste-ready)**

**2. Change log:** what was changed; any substantive meaning changes; any steps inserted that were not in the original

**3. Open issues / flags:** steps flagged as skipped; factual or logical risks

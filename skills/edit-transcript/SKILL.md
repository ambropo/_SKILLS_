---
name: edit-transcript
description: Use when the user wants to clean up, format, or process a raw ASR (auto-transcribed) transcript from an economics seminar, panel, or lecture. Triggers on "clean up this transcript", "edit this ASR output", "process this transcript", "format this seminar transcript", or "/edit-transcript".
argument-hint: "[path to transcript file or paste text]"
---

# edit-transcript

## Core principle: fidelity over readability

The default is verbatim transcription. Preserve spoken register: false starts, filler words ("um", "you know"), repetitions, and incomplete sentences are part of the record. Do not rewrite for style or clarity.

Permitted interventions only:
- Correcting clear ASR errors (wrong words, garbled proper nouns)
- Reconstructing genuinely unintelligible passages (marked with `\texttt{}`)
- Inserting `[unclear]` tags where reconstruction is not possible

---

## ASR error correction (silent fixes)

Fix silently, without marking:
- **Proper nouns**: people, institutions, countries, currencies, policies — e.g. "Cristalina Gorkiva" → Kristalina Georgieva; "hooka" → hukou
- **Obvious mishearings**: single words where the correct word is unambiguous from context
- **Fused or split words** that are clearly one word or phrase
- **Punctuation and sentence boundaries** where ASR has clearly failed

Do not silently fix: anything substantive or interpretive; passages where multiple readings are plausible; anything longer than a word or short phrase.

---

## Reconstructed passages: `\texttt{}` markup

When a passage is unintelligible but can be reconstructed with reasonable confidence, wrap the reconstruction in `\texttt{...}`. Apply conservatively — if in doubt, use `[unclear]`.

---

## Unresolvable passages: `[unclear]` tags

- Insert `[unclear]` in place of missing content
- For longer stretches: `[unclear: approx. N words]`
- Do not guess. Do not fill gaps with plausible-sounding content.

---

## Speaker attribution

- Label every speaker turn
- Format: `\textbf{\textit{Firstname Lastname:}}`
- Unidentified: `\textbf{\textit{[Unknown speaker]:}}`
- Uncertain: `\textbf{\textit{[Speaker, possibly Name]:}}`

---

## LaTeX output format

Produce a compile-ready `.tex` file:

**Preamble note** (after `\begin{document}`):
```latex
\textit{Transcript. Typewriter font (\texttt{}) denotes passages reconstructed
from unclear audio. Speaker names corrected throughout from ASR errors.}
```

Speaker turns separated by `\medskip`. Use `\section{}` for named segments. Include event name, date, location, and panelist list in the document header.

---

## Summary section

Always include `\section{High-Level Summary \& Key Points}` before the transcript body:
- 3–5 paragraphs, ~300–400 words
- Points of agreement across speakers
- Points of substantive disagreement, attributed by name
- Notable empirical claims or data points raised
- Dry and analytical — no editorialising
- Based strictly on transcript content; do not add outside knowledge

---

## Output

1. Compiled `.tex` file with summary section and full transcript
2. Brief log: proper nouns corrected (ASR → correct); number and location of `\texttt{}` reconstructions; any `[unclear]` tags; passages where confidence in reconstruction is low

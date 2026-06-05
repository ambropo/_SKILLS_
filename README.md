# Claude Code — Skills & Global Config

Custom skills and global configuration for Claude Code, built for academic research workflows in economics.

---

## Global Configuration

[`CLAUDE.md`](CLAUDE.md) lives in this folder and is symlinked to `~/.claude/CLAUDE.md`. It sets research assistant standards that apply to every Claude Code session, in every project:

- **Verification** — never claim something works without evidence; run and inspect all outputs
- **Debugging** — own all errors; resolve before delivery
- **Uncertainty** — distinguish established facts from assumptions; flag gaps explicitly
- **Audience** — PhD economists, top-journal publication standard
- **Reproducibility** — every result reproducible via a single documented pipeline
- **Traceability** — every output maps to a script, output file, and dataset
- **Definition discipline** — consistent variable definitions, samples, and transformations
- **Internal consistency** — signs, units, labels, and cross-references verified before delivery
- **Writing discipline** — dry, formal, academic style; clarity and logical flow
- **Editing discipline** — local, minimal edits only; no unsolicited rewrites
- **Citations** — complete, accurate, no placeholders
- **Deliverables** — compile-ready (LaTeX) or run-ready (Stata/MATLAB/R)
- **Pre-delivery checklist** — mandatory before sending any output
- **Communication** — report what was done, what was verified, what remains uncertain
- **Operating principle** — no unstated objectives; stop and clarify when in doubt

Project-level `CLAUDE.md` files stack on top and add project-specific rules.

---

## Skills

This repo mixes skills from the [amatray/claude-skills-public](https://github.com/amatray/claude-skills-public) fork with custom skills built for this workflow.

### Paper & Peer Review

| Skill | Description |
|-------|-------------|
| `/audit-paper` | Full paper audit: typos, grammar, style, apparatus, structural coherence, and AI-pattern detection. Modular — run all or selected passes; supports track-changes markup or clean output. |
| `/simulate-referee` | Simulated peer review: journal-calibrated Editor + 2 referee reports on your own paper. Surfaces weaknesses before submission. Supports AER, QJE, Econometrica, JFE, RFS. |
| `/referee-report` | Draft a referee report and editor letter for a paper you are reviewing. |
| `/referee-discuss` | Prepare a conference discussion: structured critique and discussant slides for a paper you have been assigned. |
| `/lit-review-verify` | Audit citations against claims in a LaTeX or Markdown manuscript. Resolves DOIs, fetches abstracts, flags misattributions and weak references. |

### Editing

| Skill | Description |
|-------|-------------|
| `/edit-stata` | Comment, structure, and clean up Stata do-files. |
| `/edit-matlab` | Comment, structure, and clean up MATLAB `.m` files. |
| `/edit-slides` | Edit and restructure Beamer slide decks. |
| `/edit-lecture` | Edit, improve, or extend written lecture notes for an economics course. |
| `/edit-policy` | Edit policy notes, briefings, or speeches for a senior policymaker audience. |
| `/edit-email-formal` | Edit formal professional emails to senior colleagues, editors, or external contacts. |
| `/edit-email-quick` | Edit short, informal emails to peers or junior colleagues. |
| `/edit-transcript` | Clean and format raw ASR transcripts from seminars, panels, or lectures. |

### Journal Editorship (EER)

| Skill | Description |
|-------|-------------|
| `/editor-draft-letters` | Draft EER decision letters (R&R or rejection) from manuscript and referee reports. |
| `/editor-ref-consolidate` | Consolidate and compare referee reports for a manuscript. |

### Research Workflow

| Skill | Description |
|-------|-------------|
| `/solving-model` | Guided workflow for setting up, solving, and verifying an economic model with step-by-step derivations. |
| `/audit-code` | Systematic review of Stata do-files: macro definitions, undefined paths, internal consistency, unused variables. |
| `/compile-latex` | Compile LaTeX via the `xelatex → bibtex → xelatex → xelatex` pipeline; fix errors and re-compile until clean. |
| `/overleaf` | Sync a local Overleaf project via Git (`pull` / `push`). |
| `/pdf-chunker` | Reliable chunked extraction from PDF files — use for any PDF input. |
| `/review-plan` | Stress-test a plan document: expert critique, blind spots, missing steps, unstated assumptions. |
| `/review-plan-auto` | Automated iterative plan review with convergence detection and deterioration safeguards. |
| `/prompt` | Reformat a rough or dictated request into a structured prompt, then execute it. |
| `/prompt-only` | Same as `/prompt` but outputs the reformatted prompt only — does not execute. |
| `/restart` | Snapshot the current session to `restart.md` before clearing context so a fresh conversation can resume. |
| `/memory` | Maintain a durable `PROJECT_MEMORY.md` at the project root (research question, decisions, data, workstreams, lessons). |
| `/skill-creator` | Create, modify, evaluate, and benchmark skills. |

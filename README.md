# Claude Code — Skills & Global Config

Custom skills and global configuration for Claude Code, built for academic research workflows in economics.

**Legend:** ↑ from [amatray/claude-skills-public](https://github.com/amatray/claude-skills-public) upstream · ★ custom

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

### Paper & Peer Review

| Skill | Description |
|-------|-------------|
| `/simulate-referee` ↑ | Simulated peer review: journal-calibrated Editor + 2 referee reports on your own paper. Surfaces weaknesses before submission. Supports AER, QJE, Econometrica, JFE, RFS. |
| `/referee-report` ★ | Draft a referee report and editor letter for a paper you are reviewing. |
| `/referee-discuss` ★ | Prepare a conference discussion: structured critique and discussant slides for a paper you have been assigned. |
| `/lit-review-verify` ↑ | Audit citations against claims in a LaTeX or Markdown manuscript. Resolves DOIs, fetches abstracts, flags misattributions and weak references. **Invoked by** `/audit-paper`. |

### Editing

| Skill | Description |
|-------|-------------|
| `/edit-paper` ★ | Edit a draft academic economics paper: implements `\claude{}` annotations and applies editorial review for argument structure, claim precision, and prose. Pair with `/audit-paper` for a full review cycle (edit draft first, audit finished version). |
| `/edit-slides` ★ | Edit academic presentation slides (Beamer): implements `\claude{}` annotations and reviews slide-level clarity, economy of language, and consistency. |
| `/edit-lecture` ★ | Edit lecture notes for an economics course: implements `\claude{}` annotations and reviews completeness, inline intuition, and pedagogical clarity. |
| `/edit-policy` ★ | Edit policy notes or briefings for a senior policymaker audience: implements `\claude{}` annotations and reviews flow, policy relevance, and calibrated language. |
| `/edit-stata` ★ | Comment, structure, and clean up Stata do-files. |
| `/edit-matlab` ★ | Comment, structure, and clean up MATLAB `.m` files. |
| `/edit-email-formal` ★ | Edit formal professional emails to senior colleagues, editors, or external contacts. |
| `/edit-email-quick` ★ | Edit short, informal emails to peers or junior colleagues. |
| `/edit-transcript` ★ | Clean and format raw ASR transcripts from seminars, panels, or lectures. |

### Auditing

| Skill | Description |
|-------|-------------|
| `/audit-paper` ↑ | Full paper audit: typos, grammar, style, apparatus, structural coherence, and AI-pattern detection. Modular — run all or selected passes; supports track-changes markup or clean output. Optionally **invokes** `/lit-review-verify`. |
| `/audit-stata` ★ | Wrapper for `/audit-code` — **invokes it** and passes all arguments through. Entry point for Stata project audits. |
| `/audit-matlab` ★ | Full audit of a MATLAB project: function definitions, path consistency, variable scope, and reproducibility. |
| `/audit-code` ↑ | Core Stata audit engine: macro definitions, undefined paths, internal consistency, unused variables. **Invoked by** `/audit-stata`. |

### Journal Editorship (EER)

| Skill | Description |
|-------|-------------|
| `/editor-draft-letters` ★ | Draft EER decision letters (R&R or rejection) from manuscript and referee reports. |
| `/editor-ref-consolidate` ★ | Consolidate and compare referee reports for a manuscript. |

### Research Workflow

| Skill | Description |
|-------|-------------|
| `/solving-model` ↑ | Guided workflow for setting up, solving, and verifying an economic model with step-by-step derivations. |
| `/compile-latex` ↑ | Compile LaTeX via the `xelatex → bibtex → xelatex → xelatex` pipeline; fix errors and re-compile until clean. |
| `/overleaf` ↑ | Sync a local Overleaf project via Git (`pull` / `push`). |
| `/pdf-chunker` ↑ | Reliable chunked extraction from PDF files — use for any PDF input. |
| `/review-plan` ↑ | Stress-test a plan document: expert critique, blind spots, missing steps, unstated assumptions. |
| `/review-plan-auto` ↑ | Autonomous iterative variant of `/review-plan` (does not call it): runs multiple review passes with convergence detection and deterioration safeguards. |
| `/prompt` ↑ | Reformat a rough or dictated request into a structured prompt, then execute it. |
| `/prompt-only` ↑ | Non-executing variant of `/prompt` (does not call it): outputs the reformatted prompt only. |
| `/restart` ↑ | Snapshot the current session to `restart.md` before clearing context so a fresh conversation can resume. |
| `/memory` ↑ | Maintain a durable `PROJECT_MEMORY.md` at the project root (research question, decisions, data, workstreams, lessons). |
| `/skill-creator` ↑ | Create, modify, evaluate, and benchmark skills. |

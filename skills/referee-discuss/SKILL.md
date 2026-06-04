---
name: referee-discuss
description: Use when the user is preparing a conference discussion, has been invited to discuss a paper, or needs help preparing discussant slides. Triggers on "prepare my discussion", "I'm discussing this paper", "help me discuss this paper", "draft discussion slides", or "/referee-discuss".
argument-hint: "[path to paper or paste paper text]"
allowed-tools: ["Read", "Glob", "Write", "Agent"]
---

# discuss-paper

## Context

Conference discussions differ from referee reports: the paper is accepted, the session is public, the tone must be collegial and constructive. Every criticism must come paired with a suggestion. Open with something genuinely good about the paper.

---

## Phases

Work through four phases in order. When the user pastes or uploads a paper, proceed immediately to Phase 1 without asking for clarification.

### Phase 1 — Paper intake with simulated peer review

1. **Read the paper** thoroughly. For multi-file papers, read all files referenced via `\input{}` or `\include{}`.

2. **Spawn a simulate-referee subagent** using the Agent tool. Use the following prompt, substituting the actual paper path for `[paper_path]`:

   > You are running a simulated peer review of an economics paper. Complete all steps in order.
   >
   > **Step 1.** Read both files in full before doing anything else:
   > - `~/.claude/preferences/journal-profiles.md`
   > - `~/.claude/preferences/domain-profile.md`
   >
   > **Step 2.** Read the paper at `[paper_path]`. For multi-file papers, also read all files referenced via `\input{}` or `\include{}`.
   >
   > **Step 3.** Assign two referee dispositions from the domain profile — one domain expert (contribution, novelty, positioning, external validity, economic significance) and one methods expert (identification, estimation, inference, robustness, data quality). Choose the dispositions most adversarial and revealing for this specific paper.
   >
   > **Step 4.** Write Referee 1 Report (Domain Expert). For each major comment: state the issue, explain why it matters, classify as FATAL / ADDRESSABLE / TASTE, and add a "What would change my mind" field. Every comment must cite a specific section, table, figure, or equation. Do not write generic comments.
   >
   > **Step 5.** Write Referee 2 Report (Methods Expert). Same format and classification system.
   >
   > **Step 6.** Before returning, re-read every major comment. If any comment could apply to a generic paper rather than this specific paper, revise it to be specific. Flag any you could not make specific.
   >
   > Return both referee reports in full, preserving all FATAL / ADDRESSABLE / TASTE labels and R1 / R2 source tags.

3. Wait for the subagent to return. Extract and merge the major comments from both reports. Prioritize FATAL > ADDRESSABLE > TASTE. Retain R1/R2 source tags and classifications.

4. Produce immediately:

   - **Intuitive summary** — two short paragraphs in plain language: what is the question, what do they do, what do they find.
   - **"What this paper does" slide** — 3–5 bullets, ready to paste into a Beamer frame. Frame the contribution positively.
   - **Raw discussion points** — 5–6 numbered points drawn from the simulated reports, presented as substantive issues without yet applying constructive framing. Label each with its FATAL/ADDRESSABLE/TASTE classification and R1/R2 source. Keep the language direct and analytical at this stage.

### Phase 2 — Brainstorming

The user reacts to Phase 1: picks points to develop, redirect, combine, or drop. Help sharpen arguments, find supporting evidence or examples, and think through implications. Follow the user's lead.

The points remain in raw form throughout this phase — do not apply constructive framing yet.

### Phase 3 — Reframing

Once the user has finalized which points to keep, reframe each one for the conference setting:

- Rephrase as an opportunity or open question, not an attack
- Pair every criticism with a concrete, constructive suggestion
- Keep each point to 2–3 sentences maximum
- Ensure the opening point says something genuinely good about the paper

Present the reframed points for user approval before proceeding to slides.

### Phase 4 — Beamer slides

When the user confirms the reframed points, draft the Beamer `.tex` content. Check existing `.tex` files in the folder for style conventions (custom `jambro` theme, `\begin{frame}{Title}` / `\begin{itemize}` structure).

---

## What makes a good discussion point

- **Identification**: Is the causal claim credible? What is the key assumption? Is there a cleaner way to make it?
- **Mechanism**: Is the proposed mechanism actually tested, or just assumed?
- **External validity**: Do results travel beyond the specific sample, time period, or institutional setting?
- **Contribution**: What does this add beyond prior work? What does it rule out?
- **Interpretation**: Are magnitudes economically meaningful? Is the right counterfactual used?
- **Extensions**: What is the natural next step?

---

## Style

- Positive framing (Phase 3 onwards): open by saying something genuinely good about the paper
- Big picture only: ignore typos, notation, minor data issues
- Every criticism paired with a suggestion
- Five or six points is enough; no padding

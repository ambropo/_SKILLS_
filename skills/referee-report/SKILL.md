---
name: referee-report
description: Use when the user wants to write a referee report and editor letter for an academic economics paper. Triggers on "write my referee report", "draft the referee report", "help me referee this paper", "draft the report and editor letter", or "/referee-report".
argument-hint: "[path to paper]"
allowed-tools: ["Read", "Glob", "Write", "Agent"]
---

# referee-draft-report

## Overview

Similar to preparing a conference discussion — find the paper's main issues — but the output is a referee report and editor letter, not discussion slides. Every concern must be specific and grounded in the paper.

Templates are in `templates/`: `Template_REPORT.tex`, `Template_LETTER_R&R.tex`, `Template_LETTER_REJECT.tex`.

Phase 1 deploys a simulate-referee subagent to generate adversarial, disposition-calibrated comments before the concerns list is assembled. This seeds the report with the quality standard of real peer review.

---

## Workflow

### Phase 1 — Paper intake with simulated peer review

When the user provides the paper:

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
   > **Step 3.** Assign two referee dispositions from the domain profile — one for a domain expert (contribution, novelty, positioning, external validity, economic significance) and one for a methods expert (identification, estimation, inference, robustness, data quality). Choose the dispositions that would be most adversarial and revealing for this specific paper.
   >
   > **Step 4.** Write Referee 1 Report (Domain Expert). Adopt the assigned disposition as a demanding adversarial referee. For each major comment: state the issue, explain why it matters, classify as FATAL / ADDRESSABLE / TASTE, and add a "What would change my mind" field describing concrete evidence or analysis that would resolve the concern. Every comment must cite a specific section, table, figure, or equation. Do not write generic comments.
   >
   > **Step 5.** Write Referee 2 Report (Methods Expert). Same format and classification system. Check at minimum: are identification assumptions stated and testable; are standard errors clustered at the right level; are obvious robustness checks missing; is the first stage strong enough (IV) or are pre-trends convincing (DiD).
   >
   > **Step 6.** Before returning, re-read every major comment in both reports. If any comment could apply to a generic paper rather than this specific paper, revise it to be specific. Flag any comment you could not make specific.
   >
   > Return both referee reports in full, preserving all FATAL / ADDRESSABLE / TASTE labels and referee source tags (R1 / R2).

3. Wait for the subagent to return both reports.

4. **Synthesize the concerns list** from the subagent output:
   - Merge and deduplicate major comments from R1 and R2
   - Prioritize: FATAL first, then ADDRESSABLE, then TASTE
   - Retain the R1 / R2 source tag and classification on each item
   - Produce **4–6 major concerns** and a **minor concerns** bullet list

5. **Present the synthesized concerns** to the user, clearly marking the source (domain expert vs. methods expert) for each comment.

**STOP.** Ask: "Do these concerns reflect your reading of the paper? Add, remove, or revise any before I draft the report."

---

### Phase 2 — Drafting

After the user has confirmed and refined the concerns, draft:

1. **Referee report** — following `templates/Template_REPORT.tex`
2. **Editor letter** — following `templates/Template_LETTER_R&R.tex` or `templates/Template_LETTER_REJECT.tex` depending on the recommendation

---

## What to look for

- **Identification**: Is the causal claim credible? What is the key assumption? Is there a cleaner way to make it?
- **Internal consistency**: Do results, tables, and text agree? Are magnitudes plausible?
- **Novelty**: What does this add beyond prior work? Is the contribution clearly articulated?
- **Robustness**: Are key results robust to alternative samples, specifications, or methods?
- **Interpretation**: Are magnitudes economically meaningful? Is the right counterfactual used?
- **Literature**: Is relevant prior work cited and engaged with substantively?
- **Missing checks**: What obvious robustness exercises are absent?

---

## Style

Academic, dry, and precise. Every criticism must be specific and actionable. Phrase concerns as questions or suggestions where possible. Reference specific sections, tables, or equations.

---

## Verification

Before delivering any draft: check all paper references are accurate; verify every concern is grounded in the paper; flag if unable to assess a technical claim.

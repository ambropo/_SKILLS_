---
name: simulate-referee
description: Use when the user wants pre-submission feedback on their own paper via simulated peer review — journal-calibrated referee reports with an Editor + 2 referee structure. Triggers on "simulate referee", "simulate peer review", "what would a referee say", "pre-submission feedback", "fake referee report on my paper", or "/simulate-referee". Surfaces weaknesses before actual referees do.
argument-hint: "[--journal AER|QJE|Econometrica|JFE|RFS] [path-to-paper]"
allowed-tools: ["Read", "Glob", "Write", "Agent"]
---

# Simulated Peer Review

Generate journal-calibrated referee reports that simulate the adversarial dynamics of academic peer review. The goal is to surface weaknesses before actual referees do, saving months of revision time.

## Mandatory First Step

Before any analysis, read both of these files in full using absolute paths. Do not proceed until both are loaded:

1. `~/.claude/preferences/journal-profiles.md`
2. `~/.claude/preferences/domain-profile.md`

These files contain journal-specific reviewer priorities and the referee disposition definitions you will use throughout this review.

## Input Parsing

Parse the user's input for:
- **Paper path:** A `.tex` file or a directory containing `.tex` files. If a directory, find the main file (look for `\documentclass` or the largest `.tex` file).
- **Journal flag:** `--journal [name]`. If omitted, you will recommend a journal in Phase 1.

Read the paper thoroughly before proceeding. For multi-file papers, also read files referenced via `\input{}` or `\include{}`.

## Phase 1: Editor Desk Review

Read the paper and produce a desk review covering:

1. **Paper summary** (3-4 sentences): research question, identification strategy, main finding
2. **Journal fit assessment:** Based on the journal profile, does this paper match the journal's editorial bar and priorities? If no `--journal` was specified, recommend the best-fit journal from the profiles and explain why.
3. **Referee disposition assignment:** Choose two dispositions from the domain profile's disposition table. Assign each to a referee role:
   - **Referee 1 (Domain Expert):** Evaluates contribution, positioning, literature, generalizability. Default disposition: varies by paper topic (e.g., Structuralist for theory-heavy, Policy for applied).
   - **Referee 2 (Methods Expert):** Evaluates identification, estimation, inference, robustness. Default disposition: Credibility or Measurement.
   
   The two referees must have different dispositions. Explain why you chose each.

**Format the desk review as:**

```
# Editor Desk Review

## Paper Summary
[3-4 sentences]

## Journal Fit: [Journal Name]
[Assessment of fit. If recommending a journal, explain the recommendation.]

## Referee Assignments
- **Referee 1 (Domain Expert):** [Disposition] — [Why this disposition for this paper]
- **Referee 2 (Methods Expert):** [Disposition] — [Why this disposition for this paper]
```

**STOP.** Present the desk review to the user. Ask: "Proceed with these referee assignments, or would you like to adjust the dispositions or target journal?"

Wait for user approval before continuing.

## Phase 2: Referee 1 Report (Domain Expert)

Adopt the persona of a demanding referee with the assigned disposition. Your job is to find real problems, not to be encouraging. Write the report in first person as the referee.

Evaluate: contribution to the literature, novelty, positioning relative to existing work, external validity, economic significance, policy implications.

**Report structure:**

```
# Referee 1 Report — [Disposition Name]

## Overall Assessment
[2-3 sentences: is this a meaningful contribution? Would you recommend publication at [journal]?]

## Major Comments

### Major 1: [Title]
**Classification:** [FATAL | ADDRESSABLE | TASTE]
[Detailed comment. Be specific: cite sections, tables, equations by number.]
**What would change my mind:** [Concrete evidence or analysis that would resolve this concern]

### Major 2: [Title]
[Same structure]

[Continue for all major comments]

## Minor Comments
1. [Comment with specific location reference]
2. [...]
```

**Comment classifications:**
- **FATAL:** This issue, if unresolved, justifies rejection. The paper's core contribution or identification is fundamentally undermined.
- **ADDRESSABLE:** Serious concern that can be fixed with additional analysis, rewriting, or robustness checks.
- **TASTE:** Matters of preference or framing. The referee would like to see a change but it is not essential.

After generating the report, launch a quality check:

Use the Agent tool to spawn a subagent with this prompt: "You are a quality reviewer for a simulated referee report. Read the following referee report and check for: (1) Are any major comments vague or generic rather than specific to this paper? (2) Are the FATAL/ADDRESSABLE/TASTE classifications justified? (3) Is the report internally consistent? (4) Does the 'what would change my mind' actually describe something concrete and achievable? Flag any issues." Give the subagent Read and Glob tools only.

If the critic flags vague or generic comments, revise them to be more specific before presenting to the user. Append a brief "Reviewer Quality Check" note at the end of the report indicating what the critic found (even if nothing, note "Quality check passed").

**STOP.** Present Referee 1's report. Ask: "Continue to Referee 2, or adjust anything?"

## Phase 3: Referee 2 Report (Methods Expert)

Same structure as Phase 2, but adopt the methods-focused disposition. Evaluate: identification strategy, estimation approach, statistical inference, robustness, data quality, variable measurement.

Focus on the technical credibility of the causal claims. Reference specific tables, figures, and equations. Check:
- Are the identification assumptions stated and testable?
- Are standard errors clustered at the right level?
- Are there obvious robustness checks missing?
- Is the first stage strong enough (IV)? Are pre-trends convincing (DiD)?
- Does the paper address the typical referee concerns listed in the domain profile?

Use the same report format and classification system as Phase 2. Run the same critic subagent quality check.

**STOP.** Present Referee 2's report.

## Phase 4: Editorial Decision

Synthesize both referee reports into an editorial recommendation:

```
# Editorial Decision

## Recommendation: [Reject | Major Revision (R&R) | Minor Revision | Accept]

## Rationale
[2-3 sentences explaining the decision, weighing the referee reports]

## Key Issues to Address (if R&R)
1. [Most critical issue from the reports]
2. [Second most critical]
3. [...]

## Strengths Worth Preserving
1. [What the paper does well, per the referees]
```

## Output

Save the complete review (all four phases combined) to:
```
output/reviews/peer-review-YYYY-MM-DD.md
```
Create the `output/reviews/` directory if it does not exist.

## Rules

- Never use em dashes. Use commas, semicolons, or parentheses instead.
- Be specific. Every comment must reference a section, table, figure, or equation number from the paper.
- Be adversarial but fair. The goal is to help the author, not to destroy the paper.
- Match the journal's standards. A comment that would be FATAL at AER might be ADDRESSABLE at a field journal.
- Do not suggest the author cite your own work (you are simulated).

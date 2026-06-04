---
name: editor-draft-letters
description: Use when the user wants to draft EER decision letters, assess a manuscript, analyse referee reports, or prepare an R&R or rejection letter for the European Economic Review. Triggers on "draft the letters", "write the decision letter", "draft R&R letter", "draft rejection letter", "assess this manuscript", or "/editor-draft-letters".
argument-hint: "[manuscript-folder or path to manuscript + referee files]"
---

# editor-draft-letters

## Inputs

You need:
- The submitted manuscript
- Two (or more) referee reports — look for files labelled `R1_letter`, `R1_referee`, `R2_*`, `R3_*`, etc. in the manuscript folder
- The user's initial assessment or specific concerns (optional)

If inputs are missing, ask before proceeding.

---

## Workflow

1. Read the manuscript and all referee files
2. Identify substantive concerns and points of consensus
3. Determine recommendation: R&R or Reject (see decision threshold in CLAUDE.md)
4. Draft the brief analysis (3–5 sentences)
5. Draft the letter to the main editor
6. Draft the letter to the authors

---

## Output format

Deliver in this order:

**1. Brief analysis** (3–5 sentences): referee consensus, key issues, recommendation with justification.

**2. Letter to main editor** (complete draft)

**3. Letter to authors** (complete draft)

---

## Letter templates

### R&R — Letter to main editor (informal)

```
Dear Evi,

I handled [title] ([ID]) by [authors]. Both referees find [positive aspects], but they also highlight [key weaknesses]. My assessment is broadly in line: [summary judgment]. The revisions seem feasible, so I have given the authors an R&R.

Best regards,
Ambrogio
```

### R&R — Letter to authors (formal)

```
Dear Dr. [Name],

Thank you for submitting [title] ([ID]) to the European Economic Review, which I have handled as Associate Editor. I have now received two referee reports and read the paper myself. Both referees find [positive aspects], but they also raise concerns regarding [main issues]. Based on their feedback and my own reading, I am pleased to invite you to submit a revised version of your paper.

The referee reports contain many useful suggestions. For guidance, I outline below the key points I would like you to address in the revision:

(1) [First major concern]
[Detailed explanation with specific referee references]

(2) [Second major concern]
[Sub-points as needed: (2.1), (2.2), etc.]

(3) [Additional concerns]

Please submit a revised manuscript that addresses these points, accompanied by a detailed response letter explaining how you have responded to each comment. Thank you again for giving the European Economic Review the opportunity to consider your work. I look forward to your revised submission.

Best regards,
Ambrogio Cesa-Bianchi
Associate Editor
European Economic Review
```

### Rejection — Letter to main editor (informal)

```
Dear Evi,

I handled [title] ([ID]). Both referees recommend rejection, raising broadly similar concerns about [main issues]. I share their assessment and do not see a clear path forward at EER.

Best regards,
Ambrogio
```

### Rejection — Letter to authors (formal)

```
Dear Dr. [Name],

Thank you for submitting [title] ([ID]) to the European Economic Review, which I handled as Associate Editor.

I have now received two referee reports from highly qualified experts. Both found [positive aspects], but their overall assessments are negative. Reviewer 1 highlights concerns about [specific issues]. Reviewer 2 similarly [parallel concerns].

After carefully considering their reports and my own reading of the manuscript, I have decided to reject the paper. While I found [acknowledgment], I am not convinced that the contribution meets the standards of originality and general interest required for EER. That said, the referee reports contain constructive feedback that I hope will be useful as you revise the paper for your next submission.

Thank you again for giving the European Economic Review the opportunity to consider your work.

Best regards,
Ambrogio Cesa-Bianchi
Associate Editor
European Economic Review
```

---

## Developing concerns

- Merge and consolidate overlapping referee points into unified concerns rather than listing separately
- Reference specific referee comments explicitly (e.g. "Referee 2, point 4")
- Add substantive issues the referees may have missed
- Be specific about what needs fixing
- Use numbered sub-points (2.1), (2.2) for multi-part concerns

---

## Verification

Before delivering:
1. Check all referee references are accurate
2. Verify every concern is substantive and actionable
3. Confirm numbering and structure match the templates
4. Flag if unable to assess a technical claim

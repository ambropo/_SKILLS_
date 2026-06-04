---
name: discuss-paper
description: Use when the user is preparing a conference discussion, has been invited to discuss a paper, or needs help preparing discussant slides. Triggers on "prepare my discussion", "I'm discussing this paper", "help me discuss this paper", "draft discussion slides", or "/discuss-paper".
argument-hint: "[path to paper or paste paper text]"
---

# discuss-paper

## Context

Conference discussions differ from referee reports: the paper is accepted, the session is public, the tone must be collegial and constructive. Every criticism must come paired with a suggestion. Open with something genuinely good about the paper.

---

## Phases

Work through three phases in order. When the user pastes or uploads a paper, proceed immediately to Phase 1 without asking for clarification.

### Phase 1 — Paper intake

Produce immediately:

1. **Intuitive summary** — Two short paragraphs in plain language. What is the question? What do they do? What do they find? Write it as you would explain the paper to a smart colleague in the corridor.

2. **"What this paper does" slide** — 3–5 bullets, ready to paste into a Beamer frame. Frame the contribution positively. These appear on the opening slide of the discussion.

3. **Discussion points** — 5–6 numbered points. Each point must:
   - Identify a substantive, big-picture issue (no notation errors, no minor data quibbles)
   - Immediately follow with a concrete, constructive suggestion or reframing
   - Be phrased as an opportunity or open question, not an attack
   - Stay concise — no more than 2–3 sentences

### Phase 2 — Brainstorming

The user will react to Phase 1: pick points to develop, redirect, combine, or drop. Help sharpen arguments, find supporting evidence or examples, and think through implications. Follow the user's lead.

### Phase 3 — Beamer slides

When the user explicitly requests slides, draft the Beamer `.tex` content. Check existing `.tex` files in the folder for style conventions (custom `jambro` theme, `\begin{frame}{Title}` / `\begin{itemize}` structure).

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

- Positive framing: open by saying something genuinely good about the paper
- Big picture only: ignore typos, notation, minor data issues
- Every criticism paired with a suggestion
- Five or six points is enough; no padding

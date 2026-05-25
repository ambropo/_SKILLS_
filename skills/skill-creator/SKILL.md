---
name: skill-creator
description: Create new skills, modify and improve existing skills, and measure skill performance. Use when users want to create a skill from scratch, edit, or optimize an existing skill, run evals to test a skill, benchmark skill performance with variance analysis, or optimize a skill's description for better triggering accuracy.
---

# Skill Creator

The core loop (RED–GREEN–REFACTOR):

1. Decide what the skill does.
2. Write 2–3 pressure-test scenarios — the kind of prompt where a naive agent would fail or cut corners.
3. **RED**: run scenarios *without* a skill. Capture verbatim what the agent does and how it rationalizes any failures.
4. **GREEN**: draft the skill targeted at the observed failures. Don't write defenses against problems you didn't see.
5. Re-run scenarios *with* the skill. Verify behavior actually changed.
6. Review outputs with the user (qualitatively) and assertions (quantitatively).
7. **REFACTOR**: close loopholes from new rationalizations that emerge under the skill.
8. Repeat 4–7 until satisfied.
9. Optimize the description for triggering accuracy.

Why baseline first: skills get written against imagined failures and end up bloated with defenses against problems the agent would never have made. Watching the unaided baseline tells you what actually breaks — and whether the skill is needed at all. If the baseline already does the right thing, stop.

Figure out where the user is in this loop and pick up from there. If they say "just vibe with me, skip the evals", do that — but resist skipping the baseline; it's cheap and decides whether the skill earns its existence.

---

## Creating a skill

### Capture intent

Extract from conversation history first. The user may already have given you the workflow, tools used, input/output formats, and corrections. Confirm gaps, then ask:

1. What should this skill enable Claude to do?
2. When should it trigger (user phrases, contexts)?
3. What's the expected output format?
4. Test cases worth setting up? Skills with verifiable outputs (file transforms, data extraction, code, fixed workflows) benefit from tests. Subjective skills (style, art) often don't.

Research in parallel via subagents (or inline) if useful.

### Frontmatter

Only `name` and `description` are required. Optional fields earn their place:

- `name` — kebab-case slug; must match the folder name.
- `description` — primary triggering mechanism. **Describe only WHEN to use the skill, not WHAT it does.** Start with "Use when…" and list concrete triggers, symptoms, and contexts. Reason: when the description summarizes the workflow, Claude follows the description and skips reading the skill body — the skill becomes documentation Claude ignores. The skill body is for *what*; the description is only for *when*. Claude also undertriggers, so be a little pushy on coverage: list multiple phrasings the user might use, including indirect ones.

  ✅ Good: `Use when the user asks to audit, review, or critique an economics manuscript, or mentions checking a paper for typos, structure, or AI-generated language.`
  ❌ Bad: `Audits papers by running seven modules — typos, grammar, style, prose, apparatus, voice, AI patterns — then reports findings. Use when reviewing a paper.` (summarizes workflow, will cause Claude to skip the body)
- `argument-hint` — optional. Shown in `/` autocomplete (e.g. `"[path-to-file]"`, `"[--clean] [path]"`).
- `allowed-tools` — optional but load-bearing when set. Restricts which tools the skill can call. List every tool needed, including MCP tools.

Example:

```yaml
---
name: review-paper
description: Use when the user asks to review, audit, or critique an economics manuscript, or mentions checking a paper for argument structure, econometric specification, citation completeness, or referee objections.
argument-hint: "[path to .tex or .pdf]"
allowed-tools: ["Read", "Grep", "Glob", "Write", "Task"]
---
```

### Anatomy

```
skill-name/
├── SKILL.md (required)
│   ├── YAML frontmatter
│   └── Markdown instructions
└── (optional bundled resources)
    ├── scripts/    — executable code for deterministic tasks
    ├── references/ — docs loaded into context on demand
    └── assets/     — files used in output (templates, icons)
```

Three loading levels:

1. **Metadata** (name + description) — always in context (~100 words).
2. **SKILL.md body** — loaded when the skill triggers. Keep under 500 lines.
3. **Bundled resources** — loaded on demand; scripts can run without being read into context.

For multi-domain skills, split by variant (`references/aws.md`, `references/gcp.md`, …) and have SKILL.md route.

### Writing patterns

Use the imperative. Explain *why*, not just *what* — modern LLMs do better with reasoning than with rigid `MUST`s. If you're reaching for `ALWAYS` / `NEVER` in caps, reframe with the reasoning instead.

Output formats: state them as a template the model can match.

```markdown
## Report structure
Use this template:
# [Title]
## Executive summary
## Key findings
## Recommendations
```

Examples carry weight:

```markdown
**Example 1:**
Input: Added user authentication with JWT tokens
Output: feat(auth): implement JWT-based authentication
```

### Discipline-enforcing skills

For skills whose job is to make the agent *follow a rule* (not just produce an output), the agent will find loopholes under pressure. Three techniques close them. Skip this subsection for technique or reference skills, where the agent isn't tempted to cheat.

**1. Foundational sentence.** State once, early: *"Violating the letter of the rule is violating the spirit."* This single sentence cuts off the entire class of "I'm following the spirit, just not the letter" rationalization. Place it at the top of the rule section.

**2. Rationalization table.** When the agent invents excuses to skip the rule, document each verbatim with a one-line reality check. Build the table from *observed* failures during testing, not from imagination:

| Excuse | Reality |
|---|---|
| "Too simple to test" | Simple code breaks. Test takes 30 seconds. |
| "I already tested it manually" | Manual tests don't replay. Write the assertion. |
| "Tests-after achieve the same goals" | Tests-after = "what does this do?" Tests-first = "what should this do?" |

The table doesn't need to be complete on first draft. Add rows every time you catch a new rationalization in testing.

**3. Red-flags list.** A short list of warning signs the agent can self-check against, with a single hard instruction at the end:

```
## Red Flags — STOP

- Code before test
- "It's about spirit not ritual"
- "This is different because…"
- "I'll add the test after"

All of these mean: stop, start over.
```

Together: the foundational sentence forbids spirit-vs-letter arguments, the table names the excuses, the red-flags list lets the agent catch itself mid-rationalization. Without these, "always do X" gets eroded the first time the agent finds X inconvenient.

**Deeper reading**: `references/persuasion-principles.md` covers the empirical basis (Cialdini, 2021; Meincke et al., 2025, N=28,000) for which persuasion levers — authority, commitment, scarcity, social proof, unity — actually move compliance in LLMs, and which to avoid (liking, reciprocity). Load it when writing a discipline-enforcing skill.

### Test cases

**Write test cases before drafting the skill, not after.** 2–3 realistic prompts a real user would actually type, ideally pressure scenarios (time pressure, conflicting incentives, ambiguity) for discipline skills. Confirm with the user. Save to `evals/evals.json`:

```json
{
  "skill_name": "example-skill",
  "evals": [
    {"id": 1, "prompt": "User's task prompt", "expected_output": "Description of expected result", "files": []}
  ]
}
```

Assertions come later, during the runs. See `references/schemas.md` for the full schema.

---

## Baseline (RED phase)

Before drafting the skill, run each scenario *without* any skill. One baseline subagent per scenario, all spawned in the same turn:

```
Execute this task:
- Task: <eval prompt>
- Input files: <eval files, or "none">
- Save outputs to: <workspace>/baseline/eval-<ID>/outputs/
- Outputs to save: <what the user cares about>
```

Outputs go in `<skill-name>-workspace/baseline/eval-<ID>/`. Read each transcript carefully — not just the final output. Capture:

- What did the agent do that the skill should prevent or change?
- What rationalizations did it use, **verbatim**? ("I'll add the test after", "this case is different because…", "manual review is enough"). These become rows in the rationalization table for discipline skills.
- Where did it cut corners under pressure?

If the baseline already does what you want, the skill is redundant — stop and tell the user. Otherwise, the failures you observed become the targets the skill body must address. Don't write defenses against problems you didn't see.

After drafting, the loop below handles the GREEN (verify) and REFACTOR (close loopholes) phases.

**Deeper reading**: `references/testing-skills-with-subagents.md` covers pressure-scenario design (time + sunk cost + authority + exhaustion combinations), the seven pressure types, meta-testing (asking the agent why it failed even with the skill loaded), and a worked example of iterating until bulletproof. Load it for discipline skills where the agent has real incentive to rationalize.

## Running and evaluating test cases

This section is one continuous sequence. Don't stop partway.

Workspace lives at `<skill-name>-workspace/` as a sibling of the skill folder. Inside: `iteration-1/`, `iteration-2/`, … and within each, `eval-0/`, `eval-1/`, … Create directories as you go.

### Step 1: Spawn all runs in one turn

For each test case, spawn two subagents in the same turn: one with the skill, one baseline.

**With-skill:**

```
Execute this task:
- Skill path: <path-to-skill>
- Task: <eval prompt>
- Input files: <eval files, or "none">
- Save outputs to: <workspace>/iteration-<N>/eval-<ID>/with_skill/outputs/
- Outputs to save: <what the user cares about>
```

**Baseline:**
- New skill → no skill at all. Save to `without_skill/outputs/`.
- Improving existing skill → snapshot the old version first (`cp -r <skill-path> <workspace>/skill-snapshot/`), point the baseline at the snapshot, save to `old_skill/outputs/`.

Write `eval_metadata.json` per eval (assertions empty for now). Give each eval a descriptive name (not just `eval-0`):

```json
{
  "eval_id": 0,
  "eval_name": "descriptive-name-here",
  "prompt": "The user's task prompt",
  "assertions": []
}
```

### Step 2: Draft assertions while runs are in progress

Good assertions are objectively verifiable, with descriptive names that read clearly in the viewer. Subjective skills (writing style, design) need qualitative review, not assertions — don't force the wrong tool.

Update `eval_metadata.json` and `evals/evals.json` with assertions once drafted. Explain to the user what the viewer will show.

### Step 3: Capture timing as runs complete

Each subagent notification contains `total_tokens` and `duration_ms`. Save to `timing.json` in the run directory immediately — this data isn't persisted elsewhere:

```json
{"total_tokens": 84852, "duration_ms": 23332, "total_duration_seconds": 23.3}
```

### Step 4: Grade, aggregate, launch viewer

1. **Grade** — spawn a grader subagent reading `agents/grader.md`, or grade inline. Save to `grading.json` per run. Use fields `text`, `passed`, `evidence` exactly (the viewer depends on these names). For programmatic checks, write a script — faster and reusable.

2. **Aggregate**:

   ```bash
   python -m scripts.aggregate_benchmark <workspace>/iteration-N --skill-name <name>
   ```

   Produces `benchmark.json` and `benchmark.md`. Put each with_skill version before its baseline. See `references/schemas.md` for the schema.

3. **Analyst pass** — read `agents/analyzer.md`. Look for assertions that always pass (non-discriminating), high-variance evals (flaky), time/token tradeoffs.

4. **Launch the viewer**:

   ```bash
   nohup python <skill-creator-path>/eval-viewer/generate_review.py \
     <workspace>/iteration-N \
     --skill-name "my-skill" \
     --benchmark <workspace>/iteration-N/benchmark.json \
     > /dev/null 2>&1 &
   VIEWER_PID=$!
   ```

   Iteration 2+: add `--previous-workspace <workspace>/iteration-<N-1>`.

   Headless environments: use `--static <output_path>` to write a standalone HTML file. The "Submit All Reviews" button downloads `feedback.json`; copy it into the workspace for the next iteration.

   Use `generate_review.py`. Don't hand-roll HTML.

5. **Tell the user**: "Two tabs — Outputs lets you click through and leave feedback, Benchmark shows the quantitative comparison. Come back when you're done."

### What the user sees

**Outputs** tab, one test case at a time: prompt, output files (rendered inline where possible), previous output (iteration 2+, collapsed), formal grades (collapsed), feedback textbox (auto-saves), previous feedback.

**Benchmark** tab: pass rates, timing, tokens per configuration, per-eval breakdowns, analyst observations.

Arrow keys or prev/next to navigate. "Submit All Reviews" writes `feedback.json`.

### Step 5: Read feedback

```json
{
  "reviews": [
    {"run_id": "eval-0-with_skill", "feedback": "missing axis labels", "timestamp": "..."},
    {"run_id": "eval-1-with_skill", "feedback": "", "timestamp": "..."}
  ],
  "status": "complete"
}
```

Empty feedback = user thought it was fine. Focus improvements where they had specific complaints.

Kill the viewer when done: `kill $VIEWER_PID 2>/dev/null`.

---

## Improving the skill

1. **Generalize from feedback.** The skill will be invoked across many prompts. The user is iterating on a few examples because it's fast for them, but overfitting to those examples is useless. If a stubborn issue persists, try different metaphors or working patterns rather than piling on rigid constraints.

2. **Keep the prompt lean.** Read the transcripts, not just outputs. If the skill is making the model burn time on unproductive work, cut what's causing that.

3. **Explain the why.** When you reach for `ALWAYS` / `NEVER`, that's a yellow flag. Reframe with reasoning — the model handles "do X because Y" far better than rote rules.

4. **Bundle repeated work.** If the test transcripts show subagents independently writing the same helper (`create_docx.py`, `build_chart.py`), put it in `scripts/` and reference it from the skill.

### Iteration loop

1. Apply improvements.
2. Rerun all test cases into `iteration-<N+1>/`, with baselines.
3. Launch the viewer with `--previous-workspace` pointing at the previous iteration.
4. Read new feedback.
5. Repeat.

Stop when: the user says they're happy, feedback is all empty, or progress stalls.

---

## Description optimization

The `description` is the primary triggering mechanism. After the skill is otherwise done, offer to optimize it.

### Step 1: Generate trigger eval queries

20 realistic queries, mix of should-trigger and should-not-trigger. Realistic = what a real user would actually type, with backstory, file paths, column names, typos, casual phrasing.

Bad: `"Format this data"`, `"Create a chart"`.

Good: `"ok so my boss just sent me this xlsx file (its in my downloads, called something like 'Q4 sales final FINAL v2.xlsx') and she wants me to add a column for profit margin as a percentage. Revenue is column C, costs column D i think"`

**Should-trigger** (8–10): different phrasings of the same intent, formal and casual, cases where the user doesn't explicitly name the file type but clearly needs the skill, uncommon use cases, competitive cases where this skill should win over an adjacent one.

**Should-not-trigger** (8–10): the valuable ones are *near-misses* — queries that share keywords but actually need something different. Adjacent domains, ambiguous phrasing that a naive keyword match would catch. Avoid obvious negatives ("write a fibonacci function" as a negative for a PDF skill tests nothing).

### Step 2: Review with user

Use the HTML template:

1. Read `assets/eval_review.html`.
2. Replace placeholders: `__EVAL_DATA_PLACEHOLDER__` (JS array, no surrounding quotes), `__SKILL_NAME_PLACEHOLDER__`, `__SKILL_DESCRIPTION_PLACEHOLDER__`.
3. Write to `/tmp/eval_review_<skill-name>.html` and `open` it.
4. The user edits queries, toggles should-trigger, exports.
5. Downloads land in `~/Downloads/eval_set.json` (or `eval_set (1).json` if multiple).

Bad eval queries → bad descriptions. This step matters.

### Step 3: Run the optimization loop

```bash
python -m scripts.run_loop \
  --eval-set <path-to-trigger-eval.json> \
  --skill-path <path-to-skill> \
  --model <model-id-powering-this-session> \
  --max-iterations 5 \
  --verbose
```

Use the model ID from your system prompt so the triggering test matches what the user experiences.

The loop splits 60% train / 40% held-out test, evaluates the current description (3 runs per query for stable trigger rates), proposes improvements, re-evaluates, iterates up to 5 times. Selects `best_description` by test score (not train) to avoid overfitting. Opens an HTML report when done.

Periodically tail the output to report progress to the user.

### How triggering works

Claude consults skills only for tasks it can't easily handle alone. Simple, one-step queries ("read this PDF") may not trigger a skill regardless of how well the description matches, because Claude can handle them directly. Multi-step or specialized queries trigger reliably when the description matches. Design eval queries that are substantive enough to actually need a skill.

### Step 4: Apply

Take `best_description` from the JSON output, update the frontmatter, show before/after and scores to the user.

---

## Package and present

If the `present_files` tool is available:

```bash
python -m scripts.package_skill <path/to/skill-folder>
```

Point the user at the resulting `.skill` file. Skip this step if `present_files` isn't available.

---

## Claude.ai

Same loop (draft → test → review → improve → repeat), but no subagents:

- **Test cases**: run each in series yourself by reading SKILL.md and following its instructions. Less rigorous (you wrote it and you're running it), but a useful sanity check. Skip baselines.
- **Reviewing**: no browser → present results inline. For file outputs (.docx, .xlsx), save to filesystem and tell the user where. Ask "How does this look?" inline.
- **Benchmarking**: skip — without baselines, the numbers aren't meaningful.
- **Description optimization**: skip — needs `claude -p` from Claude Code.
- **Blind comparison**: skip — needs subagents.
- **Packaging**: works anywhere with Python and a filesystem.

### Updating an existing skill

- **Preserve the original name** (directory and frontmatter `name`). Don't append `-v2`.
- **Copy to a writeable location before editing** (installed paths may be read-only). Work in `/tmp/skill-name/`, package from the copy.
- If packaging manually, stage in `/tmp/` first, then move to the output directory.

---

## Cowork

- Subagents available — the main parallel workflow works. If timeouts hit, fall back to serial runs.
- No display: pass `--static <output_path>` to `generate_review.py` to produce a standalone HTML file the user can open.
- **Generate the eval viewer before doing any qualitative review yourself.** Get the outputs in front of the user first.
- Feedback: "Submit All Reviews" downloads `feedback.json`. Read from the downloaded location.
- Packaging works.
- Description optimization works (`run_loop.py` uses `claude -p` via subprocess, no browser). Save it for the end, after the skill is in good shape.

---

## Advanced: blind comparison

Optional. For "is the new version actually better?", give two outputs to an independent agent without telling it which is which, let it judge, then analyze why the winner won. See `agents/comparator.md` and `agents/analyzer.md`. Most users won't need this.

---

## Reference files

- `agents/grader.md` — evaluating assertions against outputs
- `agents/comparator.md` — blind A/B comparison
- `agents/analyzer.md` — analyzing why one version beat another
- `references/schemas.md` — JSON structures for `evals.json`, `grading.json`, `benchmark.json`
- `references/persuasion-principles.md` — empirical basis (Cialdini; Meincke et al., 2025) for the discipline-enforcement levers
- `references/testing-skills-with-subagents.md` — pressure-scenario design, pressure-type taxonomy, meta-testing, worked bulletproofing example

For Anthropic's official skill-authoring guide (degrees-of-freedom framing, conciseness rules), see <https://github.com/obra/superpowers/blob/main/skills/writing-skills/anthropic-best-practices.md> or the upstream <https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview>. Not mirrored locally — too large, mostly duplicative of this skill.

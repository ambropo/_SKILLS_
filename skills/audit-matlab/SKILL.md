---
name: audit-matlab
description: Use when the user asks to audit, review, or check MATLAB .m files for undefined functions, broken paths, internal consistency, or unused variables. Triggers on "audit code", "audit MATLAB", "check my MATLAB code", "audit my MATLAB project", or "/audit-matlab".
argument-hint: "[path-to-mfiles-directory]"
---

# audit-matlab

## Working principles

**Work modularly — one module at a time.** Stop after each module and wait for confirmation. Reason: code audits compound noise fast. If module 1 surfaces 30 issues and the user catches a false positive, you want to recalibrate before module 2 generates 30 more in the same wrong direction.

**Flag unknowns explicitly instead of guessing.** MATLAB code is full of context-dependent behavior (workspace scope vs. function scope, `nargin`/`nargout` guards, scripts vs. functions). A confident-sounding guess is worse than a flagged unknown that prompts the user to clarify.

**Report only problems.** Don't list what you checked or what was already fine. The user already trusts that you ran the audit; padding the report with "Module 1: no issues found in `01_clean.m`" buries the actual findings.


# Code Audit

I want you to audit the different .m files.

The audit should proceed in **three** steps:

- **Module 1**: Look through all the .m files and make sure that all functions called are defined somewhere (either in the project, as a built-in, or in a known installed toolbox). Similarly, make sure that all paths are correctly specified and that `addpath` calls are consistent.

- **Module 2**: Analyze each .m file separately and only focus on the internal consistency of the code within the file.

- **Module 3**: Go again through all the code and identify all variables that are created but never read later.


## IMPORTANT: Stop-and-Check Points

Throughout this project, there are mandatory **STOP AND CHECK** points. At each of these points, you must:

1. Summarize what you have completed
2. Present key outputs for review
3. List any issues or concerns
4. **Wait for human approval before proceeding**

Do not proceed past a STOP checkpoint without explicit approval.


---

## Module 1: Check Paths and Functions

Carefully review the .m files and make sure that:
- All user-defined functions called in any file are defined somewhere in the project (a matching `.m` file exists)
- All `addpath` / `addpath(genpath(...))` calls reference directories that exist relative to the project root
- Hard-coded path strings (e.g., `'/data/...'`) are consistent and not duplicated with minor variations that could break on another machine
- `load` and `save` targets are consistent with the declared path structure

**Assume the following toolboxes are installed and treat all their functions as valid:**
- Statistics and Machine Learning Toolbox
- Econometrics Toolbox
- Optimization Toolbox
- Signal Processing Toolbox
- Financial Toolbox
- Parallel Computing Toolbox

Do NOT flag functions from these toolboxes as undefined.

**Before proceeding, confirm:**

- [ ] All user-defined functions are resolvable
- [ ] No `addpath` call points to a missing directory
- [ ] Path strings are consistent across files

**Present for review:**

1. All instances where a called function cannot be resolved
2. Any `addpath` calls that reference non-existent directories
3. Any hard-coded path strings that appear inconsistent or fragile

**STOP and wait for approval to proceed to Module 2**


---

## Module 2: Carefully Audit Each .m File Separately

### Structure of the audit

For each file, produce an audit where you separate **critical** problems from **suggestions**.

**OUTPUT ORDER REQUIREMENT:**
- All Critical issues must be listed in order of line number (lowest to highest)
- All Suggestions must be listed in order of line number (lowest to highest)
- Never jump backwards in the code within a category

For each problem identified (whether critical or suggestion), report:

1. The line where the problem occurs
2. The actual code causing the issue
3. A solution to fix it

Make sure to label each problem "Critical 1, Critical 2, ..." or "Suggestion 1, Suggestion 2, ... Suggestion n" so that I can easily refer to them when interacting with you.


### Things to remember about MATLAB scoping

- **Scripts share the base workspace.** A variable defined in one script persists into a subsequently `run`-called script unless `clear` is used. This is a common source of silent contamination — flag any case where a script relies on a variable that is not defined within it and could only come from a prior script.
- **Functions have isolated scope.** A variable defined inside a function is NOT visible in the caller unless explicitly returned. Flag any code that assumes a function's internal variable is accessible outside.
- **Nested functions share the enclosing function's workspace.** This is intentional and valid — do NOT flag it as a scoping issue.
- **`nargin`/`nargout` guards are valid.** Code like `if nargin < 2, x = default; end` is correct idiomatic MATLAB. Do not suggest removing it.


### Bugs and bad coding patterns to always flag as critical

- **Shadowing built-in functions**: If a variable is named the same as a MATLAB built-in (`i`, `j`, `sum`, `max`, `min`, `mean`, `std`, `var`, `input`, `output`, `data`, `error`, `warning`, `isnan`, etc.), flag it — MATLAB will silently use the variable instead of the built-in everywhere in that scope, causing hard-to-trace bugs.
- **Floating-point equality comparisons**: Flag any use of `==` or `~=` to compare floating-point values directly (e.g., `x == 0.1`). Suggest `abs(x - 0.1) < tol` or `ismembertol`.
- **`eval` / `evalin` / `evalc`**: Flag any use of these. They make code opaque, break static analysis, and are almost always avoidable. Suggest an alternative if one is obvious; otherwise flag and note the risk.
- **`global` variable declarations**: Flag any `global` statement. Global state is a source of silent side effects across functions and scripts. Flag it as critical and suggest passing the value as an argument instead.
- **Unassigned output arguments**: If a function declares output arguments (e.g., `function [a, b] = foo(...)`) but a code path exists where `b` is never assigned before the function returns, flag it — MATLAB will throw an error at runtime if the caller requests that output.
- **Logical indexing on the wrong dimension**: If code indexes a matrix with a logical vector whose length matches the wrong dimension (e.g., a row vector used to index rows of a matrix), flag it.
- **Mismatched `end` in nested indexing**: If `end` is used inside nested indexing expressions in a way that could resolve to the wrong dimension, flag it.
- **Integer/type overflow**: If code assigns the result of arithmetic to an integer type (e.g., `uint8`, `int16`) without explicit bounds checking, and the inputs could plausibly overflow, flag it.
- **`find` inside a loop**: If `find(...)` is called inside a loop where the logical condition could be evaluated once outside the loop, flag it as both a correctness risk (stale index) and a performance issue.
- **Comparing arrays of different sizes with `==`**: If `==` is used between arrays that may not be the same size, flag it — in older MATLAB versions this silently returns an empty array rather than erroring.


### Things that should always be included as suggestions

- **Preallocation**: If a variable is grown inside a loop (e.g., `x = [x; newval]`), suggest preallocating with `zeros`, `NaN`, or `cell` before the loop.
- **Vectorization**: If a loop can be replaced with a vectorized operation or a built-in function (`sum`, `cumsum`, `arrayfun`, `cellfun`, `bsxfun`, `find`, etc.), suggest it.
- **`fullfile` for path construction**: If paths are built by string concatenation (e.g., `[root '/data/' file]`), suggest `fullfile(root, 'data', file)` for cross-platform safety.
- **`~` for ignored outputs**: If code assigns a dummy variable (e.g., `dummy = sort(...)`) solely to suppress output or ignore a return value, suggest `~` (e.g., `[~, idx] = sort(...)`).
- **Output suppression**: If a function call whose output is not needed lacks a trailing `;`, suggest adding one to suppress spurious console output.


### Things that should NEVER be included as suggestions

- Missing `clear` at the top of scripts — clearing the workspace is a stylistic choice, not a requirement
- Missing `close all` or `clc` — these are personal preferences
- Adding comments or documentation — do not suggest documentation improvements
- Replacing `for` loops with `parfor` — parallelization changes program semantics and requires the user's explicit decision
- Suggesting `single` precision instead of `double` — the user's numerical precision choices are intentional
- Replacing `fprintf` with `disp` or vice versa — verbosity style is not an error
- Missing input validation (`validateattributes`, `assert`) inside functions — defensive programming style is the user's choice


### How to write the audit

- **Think first, write second.** Before writing ANY suggestion, verify it is an actual problem requiring a change.
- **Never withdraw or revise suggestions mid-response.** If you write a suggestion and then realize it's not valid, you have already failed. Do not write "I withdraw this" or "actually this is fine" — those phrases should never appear.
- **Each suggestion you write is final.** If you are unsure whether something is a problem, do not include it.
- **Every numbered item MUST contain a code change.** If you cannot write "Solution: [new code]" with actual different code, do not create the item.


### Before submitting each file audit

Re-read each item. Delete any item where:
- The "Solution" code is identical to the "Original" code
- You wrote "no change needed", "this is fine", "no issue", or similar
- You are simply confirming that existing code is correct


### What NOT to include in the audit

- **Only report actual problems or changes needed.** Do NOT list things that are already correct.
- Do NOT say things like "this already uses `fullfile` — no change needed" or "this is correct"
- If the code is fine, do not mention it at all
- Every item in your audit should be something that requires action or a decision from me
- **NEVER describe your review process.** Do not say "I checked X and it was fine" or "After reviewing, I found no issues with Y"
- **If a file has no issues, simply state "No critical issues" and "No suggestions" — nothing else.**


### MANDATORY STOP AFTER EACH FILE

After completing the audit of ONE file:

1. Present your findings (critical issues + suggestions)
2. **STOP COMPLETELY**
3. **WAIT for my response** — I may have questions, comments, or requests for clarification about your audit
4. **DO NOT proceed to the next file** until I explicitly say **"next"**

**Any other response from me (including "ok", "yes", "thanks", "I understand", corrections, or questions) is NOT permission to proceed.** Stay on the current file and respond to my input.

When you stop, always write:

```
STOPPED - AWAITING YOUR INPUT

To move to next file, say "next"
```


### CRITICAL: Maintaining Consistency Based on User Feedback

**If you provide ANY corrections, clarifications, or modifications to my audit approach at any stop point, I MUST apply those same corrections/modifications consistently to ALL remaining files in the audit.**

This includes but is not limited to:

- **Classification changes**: If you say "this should be a suggestion, not critical," apply that classification to identical issues in all future files
- **Scope clarifications**: If you say "don't flag X as an issue," never flag X in any remaining files
- **Pattern approvals**: If you clarify that a certain coding pattern is intentionally correct, do not flag that same pattern anywhere else
- **Threshold adjustments**: If you say "only flag loops that run 1000+ iterations," apply that threshold throughout
- **Variable/function exceptions**: If you say "function X is intentionally defined externally," do not flag similar cases later
- **Style preferences**: If you indicate a preference for a certain coding style, use that standard for all remaining audits

**Before auditing each new file, review all feedback provided on previous files and apply it consistently.**

**NEVER flag the same issue type again after the user has indicated it should not be flagged.**


---

## Module 3: Identify Unused Variables

Go again through all the code and identify all variables that are created (assigned) but never subsequently read.

**MATLAB-specific notes for this module:**

- **`ans`** is a silent unused-variable trap. If any expression produces output that is neither assigned nor suppressed with `;`, it lands in `ans`. Flag these if `ans` is never subsequently used.
- **Loop index variables** (`i`, `j`, `k`, etc.) that are defined by a `for` loop but never used inside the loop body are unused — flag them.
- **Suppressed but assigned outputs** (e.g., `[~, idx] = sort(x)` where `idx` is never read) count as unused — flag `idx`.
- **Function output arguments** that are defined inside a function but never assigned before `return` (or end of function) are a critical gap, not just unused — escalate those to Module 2 if found.
- Do NOT flag variables that are passed to `save`, `writematrix`, `writetable`, or similar output functions — writing to disk counts as "used."
- Do NOT flag variables used only inside a `try/catch` error message — these are intentionally conditional.

**Present for review:**

1. List of variables created but never read
2. The file and line where each unused variable is created

**STOP and wait for final approval**

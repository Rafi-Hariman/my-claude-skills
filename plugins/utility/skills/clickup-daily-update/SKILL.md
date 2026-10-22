---
name: clickup-daily-update
description: >-
  Build a copy-ready daily ClickUp work update from a main task pasted by the user and
  evidence found in the current code workspace. Use when the user invokes
  /clickup-daily-update, asks to recap today's engineering progress for a ClickUp task,
  or wants evidence-based subtasks and statuses derived from current Git/code work.
  This skill does not connect to ClickUp or modify Git. It must distinguish existing
  baseline code from work evidenced in the current daily window and must not claim a
  task is done merely because related code exists.
argument-hint: "[ClickUp main task title or brief]"
tools: Read, Grep, Glob, Bash
license: MIT
metadata:
  author: Rafi Hariman
  version: "1.0.0"
---

# ClickUp Daily Update

Turn a ClickUp main task pasted as text into a daily engineering progress report that
is grounded in the current workspace and Git history.

ClickUp is only the destination and task vocabulary. This skill has no ClickUp API,
MCP, token, credential, or workspace access. It reads local project evidence and
returns text the user can copy to ClickUp manually.

The goal is not to reconstruct the full project plan. Report only work that can be
supported by evidence in the requested daily window.

## Core rule

A file or feature that exists in the repository is not evidence that it was worked on
today.

Always separate:

- **baseline**: implementation that already existed before the reporting window;
- **current work in progress**: relevant staged or unstaged changes that exist now;
- **recent committed work**: relevant commits inside the reporting window;
- **unverified implementation**: code that appears implemented but whose completion
  cannot be established by tests/build/acceptance evidence.

Never convert repository presence into a daily-progress claim.

## Permissions and side effects

This skill is read-only.

Do not run commands that alter the working tree, index, branch, or Git history,
including:

```text
git add
git restore
git reset
git checkout
git switch
git commit
git commit --amend
git rebase
git cherry-pick
git push
```

Do not edit source files, create ClickUp tasks, update task statuses, post comments, or
persist reporting state.

Use project commands such as tests or builds only when they are already available,
read the current workspace safely, and execution is necessary to distinguish
`Implemented, unverified` from `Verified`. Do not install packages or change
configuration for a daily report.

## 1. Parse the main task

Treat the text after invocation as the main ClickUp task.

Example:

```text
/clickup-daily-update Implement API Feature Pascasarjana
```

Preserve the main task wording. Extract search concepts without rewriting the task into
a broader requirement.

Useful search concepts can include:

- feature/domain terms;
- page or route names;
- entity names;
- likely service/API concepts;
- explicit module/folder/component names supplied by the user.

Do not invent acceptance criteria, endpoint names, components, or business process
steps from a short task title.

If the input includes both a title and ClickUp description, use both as task evidence.

## 2. Establish the daily window

Default v1 reporting window:

```text
current working tree
+
relevant commits since the start of the current local calendar day
```

Use the machine's local date/timezone for the Git cutoff unless the user specifies a
different reporting period.

The working tree is included regardless of file modification timestamp because Git
state is the evidence for uncommitted work currently in progress.

Relevant read-only Git evidence can include:

```bash
git status --short
git diff --stat
git diff --name-status
git diff
git diff --cached --stat
git diff --cached --name-status
git diff --cached
git log --since='<start-of-local-day>' --name-status --format='...'
git show <relevant-commit>
```

Do not use filesystem `mtime` as the primary definition of daily work. Tooling,
checkouts, generated files, rebases, and restores can change timestamps without
representing the user's actual work.

If Git history is unavailable, continue using current code/worktree evidence but state
that committed daily work could not be established.

## 3. Find candidate workspace scope

Search the current workspace using the main-task concepts.

Start broad enough to find likely ownership, then narrow. Useful evidence includes:

- file and directory names;
- routes and navigation configuration;
- component/page names;
- services and API client methods;
- domain models/types;
- feature/module ownership;
- relevant test names;
- branch name only as a weak supporting signal;
- changed paths in Git.

Do not use a textual keyword match by itself as proof that a scope belongs to the task.

Rank candidate scopes using combined evidence:

1. semantic match to the main task;
2. relevant current Git changes;
3. relevant commits inside the daily window;
4. route/service/component relationships;
5. surrounding repository structure.

### No matching workspace evidence

If no credible scope can be found, stop and return:

```text
No matching workspace evidence was found for this ClickUp task.
Main task: <task>
```

Do not manufacture subtasks.

### Feature exists but no daily work evidence

If the matching feature exists but there are no relevant working-tree changes and no
relevant commits inside the daily window, stop and return:

```text
Matching workspace scope found: <scope>
No work evidence was detected in the current daily window.
```

Do not call the task `Not Started`. Work may have occurred outside the observable
window or in another repository.

## 4. Confirm scope only when needed

Do not ask the user to identify files that the workspace can resolve reliably.

Ask one scope confirmation only when multiple plausible scopes remain after evidence
ranking or when applying the wrong scope would materially corrupt the report.

The confirmation must include concrete evidence.

Good:

```text
I found two plausible scopes for "Pascasarjana":

1. projects/.../academic/pascasarjana
   - 4 current changes
   - API service references

2. projects/.../admission/pascasarjana
   - exists, but no current changes

Is scope 1 the workspace for this ClickUp task?
```

Bad:

```text
Which folder should I check?
```

If one scope clearly dominates the evidence, proceed without ceremony and state the
resolved scope in the report.

## 5. Build an evidence map

For the resolved scope, collect evidence before naming subtasks.

Classify evidence into:

### Current WIP

Relevant unstaged and staged changes in the working tree.

For each coherent change, determine the engineering outcome rather than listing every
changed line.

Examples:

- index template receives a new rendered result section;
- component state now models loading/error/empty results;
- service method calls the Pascasarjana endpoint;
- response mapper converts API records to view models;
- test covers an empty response.

### Recent committed work

Inspect commits inside the daily window that touch the resolved scope or clearly
support the main task.

Do not include a commit merely because it occurred today. It must relate to the main
task.

### Baseline

Use pre-window code only to understand what current changes mean. Do not report
baseline behavior as today's progress.

If necessary, compare relevant current files with their prior Git state to identify
what actually changed.

### Validation evidence

Evidence can include:

- targeted tests that already ran successfully or can safely be run;
- typecheck/build outcome;
- existing test changes that demonstrate intended behavior;
- generated build output only when trustworthy and relevant.

Code appearance alone is not validation.

## 6. Derive subtasks from observed work

A subtask is an engineering work unit supported by evidence. It is not a generic
project checklist.

Prefer outcome-oriented subtasks:

```text
Build Pascasarjana index UI
Connect study-program list API
Map API response into table rows
Handle empty Pascasarjana results
Add pagination request parameters
Cover API error state in component tests
```

Avoid generic decomposition unless evidence actually supports it:

```text
Design UI
Develop frontend
Integrate API
Testing
Documentation
```

Group several low-level edits when they serve one engineering outcome. Split them when
they can have meaningfully different completion states.

Do not infer business subtasks from symbol names alone.

### Naming rules

Subtask wording should:

- begin with a concrete verb when natural;
- describe observable engineering work;
- use domain language found in the task/code;
- remain short enough to paste into ClickUp;
- avoid implementation trivia unless it matters to the task;
- avoid claims such as `complete`, `finalize`, or `production ready` in the title.

## 7. Assign status from evidence

Use these internal statuses unless the user supplies the company's ClickUp status
vocabulary.

### `In progress`

Use when there is relevant staged/unstaged work or a sequence of commits that shows the
subtask is actively changing but completion is not established.

Typical evidence:

- implementation exists in the working tree but remains uncommitted;
- only part of the required flow is present;
- UI and service integration are being wired together;
- validation is currently failing or incomplete.

### `Implemented, unverified`

Use when the observable implementation for a derived subtask appears present, but
there is no sufficient validation evidence to call it verified.

This is useful for daily reporting because implementation progress and acceptance are
not the same thing.

### `Verified`

Use only when the implementation evidence is accompanied by an appropriate successful
check for the subtask, such as a targeted test, typecheck/build, or other existing
project validation that actually covers the claim.

A passing unrelated build does not automatically verify behavior.

### `Blocked / needs input`

Use only when actual workspace/task evidence shows a blocking dependency, missing
contract, failing prerequisite, or unresolved scope/domain decision.

Do not invent blockers to make the report look complete.

### `Not evidenced`

Primarily an internal classification. Use when a task concept exists but no daily work
evidence supports reporting it.

Normally omit these rows from the daily update. If no subtask has evidence, return the
no-work-evidence result and stop.

## 8. Optional ClickUp status mapping

ClickUp status names can vary by workspace hierarchy. Without ClickUp API access, use
only mappings explicitly supplied by the user or documented inside the project/work
instructions.

Example user-provided mapping:

```text
In progress -> IN PROGRESS
Implemented, unverified -> READY DEV
Verified -> DONE
Blocked / needs input -> BLOCKED
```

If no mapping exists, keep the internal statuses. Do not guess organization-specific
names.

Never infer `Closed` or other task-level ClickUp state for the main task from code
alone.

## 9. Daily update output

Default output is concise and copy-ready.

```markdown
## Daily task update

**Main task:** Implement API Feature Pascasarjana
**Workspace scope:** projects/.../pascasarjana
**Evidence window:** current worktree + relevant commits since 2026-10-22 00:00 local

| Subtask | Status | Evidence |
|---|---|---|
| Build Pascasarjana index UI | In progress | Index component and template changed in the current worktree |
| Connect study-program list API | Implemented, unverified | Service call and response mapping are present; no targeted validation found |
| Handle empty API response | Verified | Empty-state handling added and the relevant test passes |

**Current focus:** API integration and index-page state handling.
```

### Evidence column

Keep evidence short. It exists to make the status auditable, not to expose the entire
diff.

Do not include sensitive values, credentials, private API payloads, personal data, or
irrelevant internal details.

### If the user requests only ClickUp fields

Return:

```markdown
| Main task | Subtask | Status |
|---|---|---|
| <main task> | <subtask> | <status> |
```

Continue using evidence internally even when it is omitted from the presentation.

## 10. Relationship with commit messages

This skill and `commit-message-creator` solve different problems.

`commit-message-creator` describes a specific Git commit boundary.

`clickup-daily-update` describes observed progress against one main work item across a
daily time window, including uncommitted work.

Commit messages are supporting evidence, not authoritative subtasks. Verify relevant
commits against their diffs when their subject is too broad, ambiguous, or inconsistent
with the current main task.

Do not call or require the commit-message skill merely to produce a daily report.

## 11. Special cases

### Several repositories or worktrees

Report only evidence from the current workspace unless the task explicitly covers
several repositories. Do not search the user's filesystem indiscriminately.

### Generated files

Do not create subtasks solely from generated output unless generation itself is the
work item. Trace to the source/configuration change where possible.

### Formatting-only changes

Report them only when they are materially part of the main task. Do not convert
formatter churn into fake product progress.

### Tests changed without implementation changes

This can be a valid subtask when test coverage or regression reproduction is real work.
Describe the testing outcome accurately.

### Implementation committed before today

Use it as baseline context, not today's progress, unless the user explicitly changes
the reporting window.

### Commit authored today but unrelated to the task

Exclude it.

### Dirty workspace from another task

Exclude unrelated changes. If changes in the same files belong to multiple tasks and
cannot be separated reliably from the diff/history, mark the affected derived subtask
as ambiguous rather than assigning all work to the ClickUp task.

## 12. Quality check

Before returning the update, verify that:

- the main task wording is preserved;
- the resolved workspace scope has concrete evidence;
- baseline code is not reported as daily work;
- subtasks come from observable engineering work rather than a template;
- every reported status has supporting evidence;
- `Verified` has relevant validation evidence;
- unrelated commits and dirty files are excluded;
- untracked files are not silently treated as diffed content;
- organization-specific ClickUp statuses were not invented;
- no source, Git, ClickUp, or workspace state was modified;
- the report contains no secrets or unnecessary sensitive data.

## 13. Stop conditions

A short stop result is preferable to an invented update.

Stop without a subtask table when:

- no credible workspace scope matches the main task;
- a matching scope exists but no work evidence exists in the reporting window;
- the scope remains materially ambiguous after workspace analysis and user confirmation
  is required;
- Git/project evidence is insufficient to distinguish current work from baseline.

State exactly which condition prevented a reliable daily update.

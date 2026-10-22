---
name: commit-message-creator
description: >-
  Draft repository-consistent Git commit messages from the exact change set that will
  be committed. Use when the user asks for a commit message, wants changes summarized
  for a commit, or explicitly asks Claude to create a commit. Inspect staged changes
  first, repository commit conventions, and recent history before choosing Conventional
  Commits or another local format. Include unstaged or untracked work only when the
  user asks to describe, stage, or commit it. Never stage, amend, reset, or commit
  changes unless the user explicitly authorizes that action.
argument-hint: "[optional: intended scope, ticket, or commit boundary]"
tools: Read, Bash
license: MIT
metadata:
  author: Rafi Hariman
  version: "2.0.0"
---

# Commit Message Creator

Create a commit message for a specific Git changeset. The message should explain the
intent of the commit without inventing motivation that is not supported by the diff,
repository context, or user input.

The staged index is the default source of truth because it is the content Git will
commit. Working-tree and untracked changes are context, not part of the proposed commit,
unless the user explicitly includes them.

## 1. Safety and side-effect boundary

Message generation is read-only.

Do not run any command that changes the working tree, index, branch, or history unless
the user explicitly requested the corresponding action. In particular, do not run:

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

A request such as "buatkan commit message" authorizes drafting only.

A request such as "commit perubahan ini" authorizes committing the intended change set,
but still does not authorize staging unrelated files, amending another commit, pushing,
or rewriting history.

Before any requested commit, show the final message and the exact paths/change set that
will be committed when ambiguity remains material.

## 2. Establish repository context

Confirm the current directory is inside a Git worktree and inspect the repository
without modifying it.

Useful read-only evidence includes:

```bash
git status --short
git diff --cached --stat
git diff --cached --name-status
git diff --cached
git log -n 20 --format='%s'
```

Use `git diff --staged` or `git diff --cached`; Git treats them as synonyms for the
index-to-HEAD diff.

Also inspect repository conventions when present, for example:

- `CONTRIBUTING.md` or contributor documentation;
- `commitlint.config.*` / `.commitlintrc*`;
- package scripts or CI jobs that run commitlint;
- release/versioning tooling such as semantic-release;
- issue/ticket conventions visible in recent commits;
- repository-specific instructions affecting commits.

Do not assume Conventional Commits merely because this skill supports them.
Repository policy outranks this skill's default.

## 3. Select the intended change set

Use this priority:

### A. Staged changes exist

Draft the message from the staged diff by default.

Inspect unstaged/untracked status only to detect a mixed worktree and to avoid implying
that those files are included.

If unstaged changes modify a file that is also staged, remember that the staged version
may differ from the working-tree version. Describe the index, not the file as it exists
on disk.

### B. Nothing is staged, tracked unstaged changes exist

If the user asked only for a commit-message draft, a message may be drafted from the
unstaged diff, but label the source as `working tree; not staged`.

Do not imply the message already corresponds to a commit-ready index.

If the user asked to commit and nothing is staged, staging becomes a material decision.
Stage only the explicitly intended paths. Do not default to `git add -A` in a mixed
worktree.

### C. Untracked files exist

`git diff` does not show their contents. Do not silently omit them if the user intends
them to be part of the commit.

Inspect only untracked files within the requested scope when necessary and safe, and do
not stage them without authorization.

### D. No relevant changes

State that no matching changes are available for a commit message. Do not manufacture a
message from recent history.

## 4. Determine the repository's commit convention

Use the repository's explicit rule when one exists.

If the repository uses Conventional Commits, follow version 1.0.0 semantics:

```text
<type>[optional scope][!]: <description>

[optional body]

[optional footer(s)]
```

Important facts:

- `feat` identifies a feature;
- `fix` identifies a bug fix;
- other types such as `docs`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, and
  `style` are ecosystem conventions, not mandatory types in the core specification;
- scope is optional;
- a breaking change can use `!`, a `BREAKING CHANGE:` footer, or the repository's
  stricter commitlint convention when configured;
- additional footers use Git-trailer-like syntax.

If the repository does not use Conventional Commits, imitate its established format
rather than imposing one.

Do not infer a hard 72-character subject rule from Conventional Commits. The core
specification does not require that limit. Apply the repository's configured limit.
When no limit is configured, keep the subject concise enough to scan rather than
optimizing for an arbitrary character count.

## 5. Understand the change before naming it

Read enough diff and nearby context to answer:

- What behavior, contract, documentation, build process, or test changed?
- Is this a user-visible capability, a bug fix, an internal restructuring, or supporting
  work?
- Why was the change made, if that reason is evidenced?
- Does it change a public API, persisted format, configuration contract, CLI behavior,
  or other compatibility boundary?
- Are there multiple independent concerns accidentally grouped together?

Do not invent a rationale from code style alone. If the diff proves *what* changed but
not *why*, write a factual body or omit it.

A commit message documents the commit's intent, not a file-by-file transcript.

## 6. Choose type and scope from semantics

When Conventional Commits is active, classify by the primary intent of the changeset.

### `feat`

Use for a new externally meaningful capability in the codebase. A new internal helper
is not automatically a feature.

### `fix`

Use when behavior was wrong and the change corrects it. Describe the failure prevented
or corrected when the diff establishes it.

### `refactor`

Use when code structure changes without intending to alter externally observable
behavior and the change is not primarily a bug fix or performance improvement.

### `perf`

Use when performance improvement is the purpose and evidence supports that intent.

### `docs`, `test`, `build`, `ci`, `style`, `chore`

Use according to the repository's configured convention. `style` ordinarily means
formatting/non-semantic source style in Conventional-Commits-derived configurations,
not UI/CSS feature work.

Do not choose a type solely from changed file extensions.

### Scope

Use a scope only when it helps identify the logical area and matches repository style.
Infer it from module/package/feature ownership and recent history, not merely the first
folder name.

Omit scope when the change legitimately spans several areas or the repository rarely
uses scopes.

## 7. Detect bad commit boundaries

Before drafting, determine whether the changeset has one coherent intent.

Signals of an accidental mixed commit include:

- unrelated product features;
- behavior change plus unrelated formatting across other modules;
- independent bug fixes that can be reverted separately;
- generated/vendor changes mixed with hand-written changes for another purpose;
- a refactor bundled with unrelated documentation or configuration edits.

If changes are logically independent, recommend a split and show the proposed grouping
by path/hunk when useful. Do not stage, reset, or interactively split the index unless
explicitly requested.

Do not split mechanically by file. Tests and documentation supporting one code change
usually belong to that same commit.

## 8. Write the message

### Subject

Write a concise description of the primary change.

For Conventional Commits:

- use the repository's casing/style rules;
- begin the description with a direct verb when natural;
- avoid a final period unless local convention requires it;
- describe the outcome instead of saying `update`, `changes`, or `fix stuff` without
  an object;
- do not include implementation trivia that belongs in the body.

### Body

Add a body only when it preserves information not obvious from the subject/diff at a
glance, such as:

- why the change was necessary;
- the failure mode it prevents;
- an important design decision or constraint;
- migration/compatibility behavior;
- why a non-obvious alternative was not used, when documented by the task/context.

Do not force a body for a small self-explanatory commit.

Do not write boilerplate such as "This commit updates..." merely to make the message
look professional.

### Footers

Preserve or add issue references only when supplied by the user or established by the
repository/task context. Do not invent ticket numbers.

Use `BREAKING CHANGE: <description>` only for an actual breaking compatibility change.
Explain what consumers must change. Do not add it because a diff is large.

## 9. AI attribution

Do not hard-code a Claude co-author trailer into every generated message.

Treat AI attribution as repository/user policy:

1. inspect Claude Code's active attribution setting when relevant and available;
2. respect repository or organization policy;
3. respect an explicit user instruction to include or omit attribution;
4. avoid adding a duplicate trailer when Claude Code itself will add one during the
   commit operation.

Current Claude Code exposes `attribution.commit` for customizing or hiding commit
attribution. Therefore a static line such as
`Co-Authored-By: Claude <noreply@anthropic.com>` can conflict with the configured
behavior and may duplicate automatic attribution.

When a co-author trailer is actually required, format it as a Git trailer in the final
trailer block separated from the body by a blank line. Use the configured exact value
instead of inventing a model name or email.

Do not add AI attribution to a draft merely because Claude helped *write the commit
message*. Follow the policy for contribution attribution in that repository.

## 10. Validate the draft

Before returning the result:

- confirm the message describes the selected change set, especially the staged index;
- verify type and scope against repository convention;
- verify no breaking change has been invented or missed;
- check ticket/footer values against available evidence;
- check length/case requirements from commitlint or repository configuration;
- ensure unstaged/untracked work is not described as committed;
- ensure no secret value from a diff is copied into the message.

If commitlint is installed and the user asked to commit, use the repository's normal
validation path when practical. Do not install commitlint solely to validate one
message.

## 11. Output behavior

### Draft only

Return one recommended message in a copyable code block.

If several conventions are genuinely plausible because repository evidence is absent,
prefer a single concise recommendation and mention the assumption in one sentence.
Do not produce five stylistic variants unless the user asks for alternatives.

If the current change set should be split, return proposed commit boundaries and one
message per proposed commit instead of pretending one message is appropriate.

### Commit requested

When the user explicitly asks to commit:

1. inspect the intended staged change set;
2. draft and validate the message;
3. commit only the authorized index/paths;
4. re-read `git status --short` and the created commit metadata;
5. report the commit hash, subject, and any remaining unstaged/untracked changes.

Do not push unless separately requested.

## 12. Completion check

Before finishing, verify that:

- the staged index was preferred as the default commit source;
- mixed staged/unstaged state was not conflated;
- repository conventions were inspected before imposing Conventional Commits;
- type/scope reflect intent rather than filenames;
- body text contains evidence-based rationale only;
- breaking-change syntax matches real compatibility impact and repository tooling;
- attribution follows active policy rather than a hard-coded Claude trailer;
- no write operation occurred for a draft-only request;
- a requested commit was verified after creation and was not pushed implicitly.

---
name: skill-forge
description: >-
  Create, audit, test, vendor, categorize, and register Agent Skills in the
  my-claude-skills repository. Use when the user wants to add or improve a skill,
  turn a repeated workflow or prompt into a reusable skill, research an existing
  open-source skill before building one, optimize skill triggering, or maintain the
  repository taxonomy and marketplace. Read the repository's current TAXONOMY.md and
  plugin structure before writing. Do not vendor third-party skill content until its
  license, provenance, and required notices have been verified.
argument-hint: "[skill idea, existing prompt/SKILL.md, or workflow to package]"
license: MIT
metadata:
  author: Rafi Hariman
  version: "2.0.0"
---

# Skill Forge

Maintain `my-claude-skills` as a collection of useful, testable workflows rather than
a warehouse of prompts. A new skill should earn its place by changing agent behavior
in a repeatable way that is difficult to obtain from a clear one-off request alone.

Do not equate a valid `SKILL.md` with a good skill. The workflow is:

```text
locate repository -> read repository policy -> capture intent -> research where useful
-> decide build/vendor/reuse -> design -> validate structure -> evaluate behavior
-> register only what changed -> report
```

## 1. Locate the source repository safely

Work in the source repository, never an installed plugin cache.

Search in this order:

1. the current workspace and its parents for a repository whose
   `.claude-plugin/marketplace.json` and `TAXONOMY.md` identify
   `my-claude-skills`;
2. `~/Development/freelance/my-claude-skills` when it exists;
3. a bounded search under `~/Development` for `my-claude-skills` containing both
   expected marker files.

Do not run an unbounded filesystem scan when a narrow search is sufficient.

Verify the repository before writing:

- inspect the Git worktree status;
- confirm the expected taxonomy and marketplace files exist;
- do not overwrite unrelated uncommitted changes;
- record the repository root as `REPO` for the rest of the task.

If no unique source checkout can be identified, ask for its path. Never write to a
Claude plugin installation/cache merely because it contains the same skill names.

## 2. Read the repository contract

Before classifying or creating anything, read:

```text
$REPO/TAXONOMY.md
$REPO/.claude-plugin/marketplace.json
```

Then inspect the target plugin manifest and neighboring skills when a category becomes
a candidate.

`TAXONOMY.md` is the repository's authority for category intent and naming. Do not
hard-code its current category list into this skill because the taxonomy can evolve.

Use the live repository to answer:

- which plugin owns the domain/output;
- naming conventions;
- current skill layout;
- whether a semantically equivalent skill already exists;
- whether marketplace or bundle metadata needs modification.

Do not edit taxonomy merely to make a new skill fit. A taxonomy change is a separate
repository-design decision and must be justified explicitly.

## 3. Decide whether this should be a skill

A reusable skill is appropriate when at least one of these applies:

- the user repeatedly supplies the same multi-step instructions;
- execution depends on organization/project-specific procedure or knowledge;
- a specialized review/checklist materially improves a recurring task;
- bundled scripts make a repetitive operation deterministic;
- references or assets are needed only for a particular class of task;
- triggering the workflow automatically is useful and reasonably distinguishable.

Prefer another mechanism when appropriate:

- **one-off prompt** for a task unlikely to recur;
- **CLAUDE.md/project instructions** for short facts or conventions that should be
  available broadly rather than activated as a procedure;
- **hook** for deterministic behavior that must run on an event regardless of model
  judgment;
- **MCP/tool integration** when the core requirement is access to an external system,
  not procedural knowledge;
- **subagent** when the primary need is persistent role/tool/context isolation rather
  than an on-demand workflow.

If an existing skill already covers the intent, improve or extend it rather than
creating a near-duplicate unless the trigger/runtime behavior truly needs separation.

## 4. Capture the skill contract

Extract from the current conversation before asking questions. Do not require the user
to repeat information already provided.

Establish:

- **job**: what capability changes when this skill is loaded;
- **trigger surface**: realistic user tasks that should activate it;
- **non-triggers**: nearby tasks that should remain outside it;
- **inputs/context**: files, arguments, project state, connected systems, or external
  information required;
- **workflow**: decisions and actions the agent must perform;
- **output**: artifact or observable result;
- **success criteria**: how quality can be checked;
- **side effects**: files, commands, network, credentials, installations, commits, or
  other changes;
- **portability**: whether this is Claude Code-specific or should remain compatible
  with the Agent Skills standard.

Ask only for a missing answer that materially changes these choices. Low-risk details
can be resolved from repository evidence or represented as an explicit assumption.

## 5. Research before building when external knowledge matters

Research is required when the proposed skill depends on changing external behavior or
when the user asks to find/reuse an existing skill. Examples include framework APIs,
Claude Code/plugin behavior, security standards, third-party tools, or an
open-source implementation to vendor.

Prioritize:

1. official product/specification documentation;
2. maintained upstream repositories and release notes;
3. strong community implementations as comparators;
4. articles/forums only as supporting evidence.

Compare candidates on workflow fit, current maintenance, compatibility, license,
dependencies, permissions, secret handling, privacy, prompt-injection surface, and
supply-chain risk. Popularity is not a substitute for fit.

Do not search the internet simply to add authority to a skill whose workflow is fully
local and already well specified.

## 6. Choose build, vendor, adapt, or reuse

### Build

Choose `build` when no existing implementation fits well or the workflow is specific
to this repository/user.

### Vendor

Choose `vendor` only when incorporating third-party copyrighted files is useful and
their license permits the intended redistribution/modification.

Before copying any third-party content:

- identify the exact upstream repository and revision/tag where practical;
- read the license from the upstream source, not a secondary directory listing;
- check redistribution, modification, attribution, notice, and share-alike
  requirements that apply;
- preserve required copyright/license notices;
- include the upstream license/notice files required by that license;
- record upstream project/author provenance in a repository-appropriate location;
- inspect bundled scripts/assets for unexpected network access, executable behavior,
  credential use, or other supply-chain risk.

Do not assume "GitHub public" means reusable. If redistribution rights are unclear,
do not vendor the content. Prefer an independently written skill informed by lawful
public documentation, or leave `[NEEDS INPUT]` for the licensing decision.

Do not copy a third-party `SKILL.md`, then relabel the entire derivative as MIT if the
upstream terms require different treatment.

### Adapt

Use `adapt` when an upstream idea is useful but the repository needs an independently
written workflow. Preserve factual provenance where appropriate, but do not copy
protected expression unnecessarily.

### Reuse

Choose `reuse` when an existing local skill already satisfies the need. No new skill
should be created merely to provide a different name for the same workflow.

If two choices have materially different licensing, maintenance, security, or
behavioral consequences, present the trade-off before implementing the irreversible
or policy-changing choice.

## 7. Classify and name from the live taxonomy

Select exactly one primary plugin category according to `TAXONOMY.md`, based on the
skill's capability/output and domain.

Before naming:

- search existing skill directories for collisions and near-duplicates;
- follow the taxonomy's current naming rules;
- keep the name concise enough to invoke naturally;
- prefer capability/result language over author names or marketing adjectives.

The frontmatter `name` must match the skill directory name and comply with the current
Agent Skills name constraints.

Do not ask for confirmation of a straightforward taxonomy/name decision. Report the
choice. Ask only when the alternatives imply materially different ownership or
behavior.

## 8. Design for triggering

The `description` is routing metadata, not marketing copy. It must state:

1. what the skill does;
2. when it should be used;
3. distinctive task language or contexts that separate it from nearby skills;
4. important non-trigger boundary when false activation would be costly.

Use natural tasks rather than an exhaustive bag of magic phrases. Explicit aliases
such as a user's established command phrase may be included, but behavior should not
depend on exact wording alone.

Do not put critical trigger rules only in the body. The body is loaded after a skill
is selected, so it cannot repair a description that fails discovery.

Before finalizing the description, create trigger cases covering:

- clear positive tasks;
- paraphrased positives without the skill's name;
- nearby negatives;
- ambiguous overlap with another installed skill;
- explicit invocation.

## 9. Write for progressive disclosure

Use the smallest `SKILL.md` that reliably controls the workflow.

Keep in the body:

- operational decisions;
- workflow order when order matters;
- safety/permission boundaries;
- verification requirements;
- pointers describing exactly when to load supporting resources.

Move material out of the main body when it is large or optional:

```text
<skill>/
├── SKILL.md
├── references/   # domain material loaded only when needed
├── scripts/      # deterministic/repetitive operations
├── assets/       # templates/resources used in outputs
└── evals/        # repository-local test cases when adopted by this collection
```

Do not split files merely for aesthetics. Progressive disclosure is useful only when
it prevents irrelevant context from loading.

Prefer imperative, concrete instructions. Remove motivational prose, role-play, and
claims such as "world-class", "expert", "think deeply", "production-grade", or
"best practices" unless they resolve to observable behavior.

Explain a constraint when the reason helps the agent choose correctly in an edge case.
Avoid repeated `MUST`/`ALWAYS` language when an evidence-based decision rule is more
robust.

## 10. Frontmatter

Start from the *current* repository convention and validate it against the current
Claude Code/Agent Skills format. Do not assume this skill's own frontmatter is a
permanent template.

At minimum, Agent Skills requires `name` and `description`. The repository may retain
additional metadata such as license, author, version, or argument hints when supported
by its conventions/client.

Only declare environment/tool requirements the skill actually needs. Prefer least
privilege. A writing-only skill should not request shell/network access. A read-only
review skill should not request write privileges without a workflow reason.

When a field is client-specific, keep portability implications explicit instead of
pretending it is part of the cross-client standard.

## 11. Write or update repository files

Before writing, show a concise implementation decision when the task involves a new
skill, third-party vendoring, new executable scripts, marketplace changes, or other
material side effects. If the user has already explicitly authorized creating the
skill and the choices are non-material, proceed without an extra confirmation loop.

Write only the files required by the design.

For a new local skill, the usual path is:

```text
$REPO/plugins/<category>/skills/<name>/SKILL.md
```

Use the live plugin layout as the authority rather than assuming this path will never
change.

Do not overwrite an existing skill unless the task is explicitly an update or the
user already authorized replacement. Preserve unrelated uncommitted changes.

Do not `git commit`, `git push`, publish a release, or update a remote marketplace
unless the user explicitly asked for those git/network side effects.

## 12. Validate structure and registration

After writing:

1. parse/check YAML frontmatter;
2. confirm directory name and `name` match;
3. verify required fields and repository metadata conventions;
4. verify referenced files exist and relative paths resolve;
5. inspect manifests/marketplace entries for whether any update is actually required;
6. run the current official Claude Code plugin/marketplace validation command when the
   repository structure supports it;
7. inspect `git diff` and ensure only intended files changed.

Do not edit `.claude-plugin/marketplace.json` merely because one skill was added inside
an already registered plugin. Marketplace entries identify plugins, not every skill.
Update marketplace or bundle metadata only when the live structure requires it.

If the repository provides tests/scripts for taxonomy or marketplace integrity, run
them too.

## 13. Evaluate behavior, not just syntax

A syntactically valid skill is only a draft until its behavior has been exercised.

For skills with objective outputs, create a small evaluation set containing realistic
prompts and verifiable expectations. For subjective skills, use representative examples
and rubric-based review rather than fake objective assertions.

At minimum, evaluate:

### Triggering

- Does the skill activate for intended paraphrases?
- Does it stay out of nearby unrelated tasks?
- Does it conflict with another installed skill?

### Outcome quality

Where tooling permits, compare:

```text
same task + skill
vs.
same task without skill
```

Judge the difference on task-specific criteria such as correctness, omitted
requirements, unnecessary actions, safety, output usability, and context/cost overhead.

A skill that does not materially outperform the baseline may not deserve to exist even
if its instructions are well written.

For non-deterministic model outputs, use multiple representative runs before making a
strong claim when the evaluation tooling supports it.

If the environment cannot run behavioral evals, save or report the test cases and mark
behavioral quality as unverified. Do not call the skill "production-ready" merely
because its Markdown looks good.

## 14. Installing and updating

Derive installation/update commands from the repository's current README, marketplace
identity, and installed Claude Code behavior. Do not hard-code stale placeholders or
assume the plugin must be reinstalled in every case.

When giving a command, distinguish:

- local repository change;
- git commit/push;
- marketplace registration;
- plugin installation;
- plugin update.

Only include commands relevant to what changed.

Never run destructive git commands to clean the repository for convenience.

## 15. Completion report

Report evidence, not ceremony:

```text
Decision: build | vendor | adapt | reuse
Category: <category>
Skill: <name>
Path: <path>
Files changed:
- <path> — <reason>
Validation:
- structure: pass/fail/not run
- Claude/plugin validation: pass/fail/not run
- trigger eval: pass/fail/not run
- outcome eval vs baseline: pass/fail/not run
External provenance/license: <when applicable>
Risks or unresolved items: <only material items>
Suggested git/update commands: <only if relevant>
```

Do not append generic follow-up offers.

## 16. Quality gate

Before declaring the work complete, verify that:

- the capability belongs in a skill rather than another Claude Code mechanism;
- taxonomy and naming came from the live repository;
- no existing local skill already solves the same problem adequately;
- the description carries the actual trigger contract;
- body instructions change behavior rather than decorate the prompt;
- optional detail uses progressive disclosure where worthwhile;
- tools/permissions are no broader than necessary;
- third-party content has verified provenance and compatible redistribution terms;
- executable/vendor resources were inspected for supply-chain and secret risks;
- structure/manifests validate;
- intended-positive and nearby-negative trigger cases exist;
- outcome quality was compared with a no-skill baseline when the environment permits;
- unverified behavior is labeled unverified;
- git/network side effects were not performed without authorization.

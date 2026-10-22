# Skill Taxonomy

This file is the source of truth for classifying skills in `my-claude-skills`.

`/meta:skill-forge` and contributors must read the current taxonomy before creating, moving, or categorizing a skill. Do not classify a skill from its author, implementation language, or a single keyword. Classify it by its primary job, output, and domain.

## Classification rules

Each skill belongs to exactly one primary plugin category.

Use these rules in order:

1. Identify the skill's primary output or capability.
2. Identify the domain that owns that capability.
3. Check existing skills for overlap before creating a new one.
4. Prefer the category where users would naturally look for the capability.
5. If a skill spans several domains, classify it by its primary outcome rather than every technology it touches.

A supporting technology does not determine the category. For example, a UI audit implemented against React still belongs to `ui-ux` when its primary output is design evaluation, while a React component refactor belongs to `frontend` when its primary output is code engineering.

## Categories

### `ui-ux`

Visual design, design systems, interaction design, branding, and user-experience review.

Use for skills whose primary output is a visual or UX decision, visual artifact, design direction, or interface-quality assessment.

Examples:

- `ui-styling`
- `ui-ux-pro-max`
- `avoid-ai-design`

Do not place general frontend implementation or framework refactoring here unless visual/UX design is the primary job.

### `slicing`

Design-to-code reconstruction and implementation.

Use when the primary input is a screenshot, mockup, design image, or visual reference and the primary output is frontend code that reproduces it.

Examples:

- `sliceui`
- `sliceui-handoff`

### `frontend`

Client-side software engineering.

Use for component architecture, framework code, browser-side state, forms, TypeScript application code, client-side API consumption, and frontend testing or refactoring.

Examples:

- `ng-component-audit-doc`
- `typescript-pro`

Client code that consumes an API belongs here. Defining or implementing the server API itself belongs to `backend`.

### `backend`

Server-side application and API engineering.

Use for endpoint implementation, controllers/handlers, server-side authentication and authorization, persistence, database access, queues, server-side domain logic, and backend service architecture.

Examples will be added when the first backend-specific skills enter the collection.

### `analysis`

Reasoning, review, quality analysis, decision support, and text transformation.

Use when the primary output is an assessment, recommendation, structured reasoning result, quality report, or rewritten text rather than a code artifact.

Examples:

- `sonarqube`
- `llm-council`
- `humanizer`
- `prompt-context-builder`

### `utility`

Cross-domain developer utilities and workflow helpers.

Use for technical tasks that support development work but are not owned by a specific application layer or design domain.

Examples:

- `commit-message-creator`
- `clickup-daily-update`

`clickup-daily-update` belongs here because it converts local Git/code evidence into a copy-ready work update. It does not manage ClickUp through an API and is not a code-analysis policy skill.

### `meta`

Skills that create, evaluate, organize, or maintain other skills and the skill repository itself.

Examples:

- `skill-forge`

### `bundle`

Distribution-only plugin for installing the complete collection.

Do not place standalone skills here. This category exists to aggregate the other plugins for installation.

## Boundary examples

Use these examples when two categories appear plausible:

| Task | Category | Reason |
|---|---|---|
| Audit visual hierarchy of an Angular page | `ui-ux` | Primary output is a design/UX assessment |
| Refactor Angular component lifecycle and typing | `frontend` | Primary output is frontend code engineering |
| Convert a Figma screenshot into React components | `slicing` | Primary transformation is design-to-code |
| Implement an HTTP endpoint and database query | `backend` | Server-side API implementation |
| Review SonarQube findings and recommend fixes | `analysis` | Primary output is quality analysis/remediation guidance |
| Generate a Git commit message | `utility` | Cross-domain development workflow helper |
| Generate a daily ClickUp update from Git evidence | `utility` | Cross-domain work-reporting helper |
| Create or evaluate a new Agent Skill | `meta` | Operates on the skill system itself |

## Skill naming

Skill names must follow the current Agent Skills naming constraints:

- lowercase letters, numbers, and hyphens only;
- maximum 64 characters;
- no leading or trailing hyphen;
- no consecutive hyphens;
- the `name` field must match the parent skill directory exactly.

Prefer concise capability or outcome names such as:

```text
commit-message-creator
clickup-daily-update
ng-component-audit-doc
```

Avoid author names, marketing adjectives, version numbers, or implementation details unless they are necessary to distinguish the capability.

## Trigger descriptions

The `description` field is routing metadata. It should state:

1. what the skill does;
2. when it should be used;
3. distinctive task language or context that separates it from nearby skills;
4. an important non-trigger boundary when false activation would be costly.

Do not rely on exact magic phrases alone. Use natural task descriptions and clear boundaries.

## `SKILL.md` frontmatter

Agent Skills requires only `name` and `description` in YAML frontmatter. Other fields are optional and should be added only when they serve the skill or repository.

Minimal portable frontmatter:

```yaml
---
name: <skill-name>
description: >-
  <What the skill does and when it should be used.>
---
```

Repository-standard frontmatter for first-party skills:

```yaml
---
name: <skill-name>
description: >-
  <What the skill does and when it should be used.>
argument-hint: "[optional argument hint]"
license: MIT
metadata:
  author: Rafi Hariman
  version: "1.0.0"
---
```

`argument-hint` and other Claude Code-specific fields are client extensions, not required fields in the portable Agent Skills specification.

Use `compatibility` only when the skill has meaningful environment requirements such as a specific product, system package, runtime, or network access.

Use tool-permission frontmatter only when the client supports it and the skill genuinely benefits from pre-approval. Prefer least privilege.

## Supporting files

A skill must contain `SKILL.md`. Add supporting resources only when they improve execution or reduce unnecessary context.

Recommended layout when needed:

```text
<skill-name>/
├── SKILL.md
├── references/   # Documentation loaded only when needed
├── scripts/      # Deterministic or repetitive operations
└── assets/       # Templates or output resources
```

Keep operational instructions in `SKILL.md`. Move large optional reference material out of the main file when progressive disclosure is useful.

## Third-party skills

Before vendoring third-party content:

- verify the upstream license and redistribution terms;
- preserve required copyright and license notices;
- retain provenance and attribution required by the upstream license;
- inspect scripts and assets for unexpected network access, credentials, permissions, or other supply-chain risks;
- do not assume a public GitHub repository grants permission to redistribute its contents.

If a third-party idea is useful but the repository does not need a literal copy, prefer an independently written adaptation based on lawful public documentation and preserve factual provenance where appropriate.

## Taxonomy changes

Do not create a new category for a single awkward skill by default.

Add or rename a category only when the existing taxonomy cannot represent a recurring group of capabilities without persistent ambiguity. Treat taxonomy changes as repository-architecture changes and update `README.md`, marketplace/bundle documentation, and affected skill paths when necessary.

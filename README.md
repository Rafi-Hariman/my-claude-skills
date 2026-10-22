# my-claude-skills

Personal Claude Code skill collection, organized as domain-based plugins and distributed through a single marketplace.

## Repository structure

```text
my-claude-skills/
├── .claude-plugin/
│   └── marketplace.json
├── TAXONOMY.md
└── plugins/
    ├── ui-ux/       # UI/UX and visual design
    ├── slicing/     # Design-to-code workflows
    ├── frontend/    # Frontend engineering
    ├── backend/     # Backend and API engineering
    ├── analysis/    # Review, analysis, and reasoning
    ├── utility/     # File and workflow utilities
    ├── meta/        # Skill authoring and maintenance
    └── bundle/      # Bundle containing all plugins
```

`TAXONOMY.md` is the source of truth for skill categories and naming conventions.

Skills are automatically namespaced by plugin, for example:

```text
/ui-ux:ui-styling
/frontend:typescript-pro
/meta:skill-forge
```

## Installation

Register the marketplace once:

```bash
claude plugin marketplace add Rafi-Hariman/my-claude-skills
```

Install a plugin by category:

```bash
claude plugin install ui-ux@my-claude-skills --scope user
```

Or install the complete collection:

```bash
claude plugin install all@my-claude-skills --scope user
```

Use `--scope project` for project-specific skills such as `ng-component-audit-doc` and `sonarqube`.

## Creating or improving a skill

Use the built-in Skill Forge:

```text
/meta:skill-forge "<skill idea, workflow, prompt, or existing skill>"
```

Skill Forge uses the live repository taxonomy to classify, design, validate, and evaluate skills before adding them to the collection.

Standard skill location:

```text
plugins/<category>/skills/<skill-name>/SKILL.md
```

## Publishing changes

Review the changes, then commit and push them using the repository's normal Git workflow:

```bash
git status
git diff --staged
git commit
git push
```

Update an installed plugin after the changes are published:

```bash
claude plugin update <category>@my-claude-skills
```

## Third-party skills

Vendored third-party content must comply with its upstream license and redistribution terms.

Preserve all required:

- copyright notices;
- license and notice files;
- attribution and provenance;
- modification or redistribution requirements.

A public GitHub repository does not automatically grant permission to copy, modify, or redistribute its contents. Verify the upstream license before vendoring a skill or resource.

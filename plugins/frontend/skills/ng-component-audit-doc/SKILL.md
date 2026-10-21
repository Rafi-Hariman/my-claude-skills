---
name: ng-component-audit-doc
description: Use this skill whenever the user asks to audit, harden, or refactor Angular (v13+) components to senior-engineer production standard, and/or wants a per-component documentation .md file generated for a scope of Angular components. Trigger this on phrases like "ponytail audit", "audit komponen Angular", "audit dan dokumentasi component", "refactor ke senior standard", "buatkan .md untuk tiap component", or whenever the user pastes an Angular routing module / folder path and asks for a code audit plus docs. This is always a strict two-phase workflow (Task 1: audit/refactor, then Task 2: documentation) — never skip a phase, never merge them into a shortcut, and always get plan approval before touching files.
---

# Angular Component Audit & Documentation

A two-phase workflow for hardening Angular components to senior-engineer standard and producing contributor-facing documentation for each one. Phase 1 (audit/refactor) must be fully completed and confirmed before Phase 2 (docs) begins for that component.

## Determining scope

The user must specify scope in their request — e.g. a routing module path, a feature folder, or an explicit list of components. Do not assume scope from a previous session. If the user references a routing module (e.g. `*-routing.module.ts`), open it and resolve every `component:` entry in the relevant route block(s), including nested `children` routes, to build the full component list before presenting the plan.

If scope is ambiguous or not provided, ask the user for it before proceeding — do not guess which folder or components are in scope.

## Hard rules (apply to every run)

- **Plan before code.** Before modifying or creating any file, present a plan: the full list of components in scope, and per-component what you intend to change in Task 1 and why. Wait for explicit user approval before editing anything.
- **Task 1 must finish before Task 2 starts**, per component or per batch — never write a `.md` doc for a component whose audit isn't done and confirmed.
- **Don't rewrite business logic.** This is a hardening/refactor pass, not a feature rewrite. If a genuine fix requires changing observable behavior, flag it in the report instead of silently applying it.
- **Never fabricate business-process details** you can't infer from the code. If the real-world process behind a component is unclear, ask the user instead of guessing.
- **Work in batches for large scopes.** Report progress after each batch rather than doing the whole scope silently and dumping one giant report at the end.
- **Task 1 report is in-chat by default** — only write it to a file if the user asks for that.

---

## Task 1 — Code Audit & Refactor to Senior Standard

Run an audit pass on every component in scope. For each component file (`.ts`, `.html`, `.css`/`.scss`, `.spec.ts` if present), review and fix:

1. **Typing** — no implicit `any`; proper interfaces/types for `@Input()`/`@Output()`, API payloads, and API responses.
2. **RxJS / API handling**
   - Every HTTP call must be cancellable (`takeUntil(destroy$)`, `switchMap`, or equivalent) so it can't re-hit the API or leak after the component is destroyed or the request is superseded.
   - If an API call fails mid-flight, the in-progress request must be properly cancelled/aborted — not silently retried, not left hanging.
   - Proper `unsubscribe` in `ngOnDestroy` for any subscription not using the async pipe.
3. **User feedback / toast handling** — every success, error, and warning state from an API call or user action must trigger the appropriate toast/notification. No silent failures.
4. **Error handling** — every async operation and every conditional that can fail (null/undefined data, empty arrays, failed API call, invalid form state) must be explicitly handled, never assumed to succeed.
5. **Accessibility (WCAG)** — semantic HTML, `aria-*` where needed, keyboard navigability, focus management, labeled form controls, contrast-dependent classes where applicable.
6. **UX details** — loading states, disabled state while a request is in flight, empty states, guards against duplicate submit actions.
7. **Code cleanliness** — remove unused variables/imports, consolidate redundant local variables to the minimal efficient set, meaningful naming, no dead code, formatting consistent with the rest of the codebase.
8. **Don't break existing functionality** — see hard rules above.

### Task 1 report format

For every file touched, report:

- File path
- Change type: `added` / `modified` / `deleted`
- Brief description of what changed and why

---

## Task 2 — Component Documentation (`.md`)

Only after a component's Task 1 audit is done and confirmed: add exactly one new file, `component-name.md`, alongside the component's existing files:

```
component-name/
  component-name.component.ts
  component-name.component.html
  component-name.component.css
  component-name.component.spec.ts   (if it exists)
  component-name.md                  <- new
```

Write it for a new contributor who has never seen this codebase. Cover:

1. **Purpose** — what the component does and where it fits in the app.
2. **Business process flow** — the real-world process this component supports, step by step, in plain language (not a code walkthrough).
3. **Inputs / Outputs** — every `@Input()`, `@Output()`, and injected service/dependency, with what each is for.
4. **Function/logic explanation** — for each non-trivial method: what triggers it, what it does, what it returns or affects.
5. **API interactions** — which endpoints it calls, when, and how success/error/loading states are handled.
6. **Edge cases / known constraints** — anything a new contributor should watch out for.

Keep it clear and concise — documentation, not marketing copy.

---

## Output conventions for this skill

- Deliver the Task 1 report and Task 2 `.md` content directly in chat/as files in the repo — no diagrams, visualizations, or extra widgets for this workflow.
- If the user only asks for Task 1, stop after the report and don't auto-generate docs.
- If the user only asks for Task 2 on components already known to be hardened, skip straight to documentation — but confirm that assumption with the user first.
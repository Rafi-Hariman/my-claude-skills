---
name: ng-component-audit-doc
description: >-
  Audit and minimally harden an existing Angular component scope, then optionally
  create evidence-based contributor documentation for the audited components. Use
  when the user asks for an Angular component audit, hardening/refactor, "ponytail
  audit", audit plus per-component docs, or contributor docs for a supplied Angular
  scope. Resolve routing scopes when relevant. Inspect the workspace's actual Angular,
  TypeScript, RxJS, test, lint, and UI-stack versions before applying version-specific
  guidance. Do not require documentation when the user asks only for code audit, and
  do not refactor merely to satisfy a generic "senior" style.
argument-hint: "[routing module, feature directory, component list, or audit request]"
---

# Angular component audit and documentation

Audit existing Angular components against the behavior, conventions, and supported
APIs of the target workspace. Fix demonstrated defects and material maintainability
problems with the smallest coherent change. Produce component documentation only when
the user requests it or the requested workflow includes it.

"Senior standard" is not a checklist or a syntax preference. In this skill it means:
correct behavior, explicit contracts, lifecycle safety, accessible interaction,
useful tests, repository consistency, and changes whose necessity can be explained.

## 1. Establish the workspace baseline

Before recommending a refactor, inspect the relevant workspace metadata and existing
code. Determine, when available:

- Angular and Angular CLI versions;
- TypeScript and RxJS versions;
- application/framework structure and whether the component is NgModule-based,
  standalone, or mixed;
- package manager and workspace layout;
- test runner and existing component-test conventions;
- lint and formatter configuration;
- UI/component libraries such as Angular Material or project-specific controls;
- state-management conventions, if relevant to the component;
- repository instructions and nearby component patterns.

Do not upgrade Angular, convert NgModules to standalone components, introduce signals,
replace dependency-injection style, or adopt a modern API solely because current
Angular documentation recommends it. A hardening audit is not a migration.

For Angular 13-era projects, do not use APIs introduced in later Angular releases.
For example, `takeUntilDestroyed` is a later Angular API and must not be backported
into an Angular 13 component without a separately authorized framework upgrade.

When official guidance conflicts with established local style, distinguish a defect
from a style preference. Prefer repository consistency unless the local pattern causes
a concrete correctness, security, accessibility, or maintainability problem.

## 2. Determine scope from evidence

Accept any explicit scope the user supplies, including:

- one or more component paths;
- a feature directory;
- a routing module;
- an explicit component list.

If a routing module defines the scope, inspect the route configuration and resolve
components reachable from the relevant routes, including nested children. Also account
for lazy-loaded routes and components when the configuration makes them part of the
requested scope.

Do not assume that the routing file is a complete inventory of every supporting
component. Routed page components may compose local child components that matter to a
specific defect. Include such children only when the user's requested scope or the
issue being fixed requires them, and report the expansion.

If the request provides no usable scope and the repository does not make the target
unambiguous, ask for the smallest missing scope decision. Do not infer a feature from
an unrelated previous session.

## 3. Plan changes, not ceremony

For a review-only request, inspect and report findings without requiring an approval
round.

For a request that authorizes fixes, first inspect enough code to produce an informed
change plan. Present material changes before editing when they:

- alter observable behavior or public component contracts;
- affect several components or shared services;
- add/remove dependencies;
- modify routing, state management, API contracts, or shared styling;
- require a migration or architectural decision.

Do not force an approval checkpoint for obvious, local, reversible fixes when the user
has already explicitly asked you to fix them and repository policy permits editing.
Never use a plan as a substitute for reading the actual implementation.

## 4. Audit dimensions

Audit only dimensions applicable to the component. Do not manufacture findings so
every category has an entry.

### Component contract and typing

Check:

- `@Input()` and `@Output()` contracts and nullability;
- event payload types;
- service/API response types where the component owns or consumes them;
- unsafe casts, unnecessary `any`, and unchecked nullable values;
- mutable state whose mutation makes behavior difficult to reason about.

Do not replace `any` blindly. If the real shape is unknown, trace the local API or
service contract first. An invented interface is worse than an acknowledged unknown.

### Observable and lifecycle behavior

Classify subscriptions before changing them:

- template subscriptions managed by `async` or an equivalent framework mechanism;
- finite observables that complete naturally, such as ordinary `HttpClient` requests;
- long-lived streams such as router, form, event, store, subject, interval, or custom
  observable subscriptions;
- request streams where a new trigger should supersede an older one;
- write operations where cancellation semantics may be inappropriate.

Do not apply `takeUntil` to every HTTP call as a universal rule. Ordinary HttpClient
observables normally complete after their response. Use lifecycle cancellation when a
pending result must not affect a destroyed component or when local conventions require
it.

Use flattening operators by behavior:

- `switchMap` when only the latest result is relevant and superseding an earlier
  request is semantically correct;
- `exhaustMap` when duplicate triggers must be ignored while work is active;
- `concatMap` when operations must complete sequentially;
- `mergeMap` when concurrent operations are valid.

For mutations, remember that client unsubscription does not prove the server operation
was rolled back. Do not describe cancellation as transactional abort unless the API
contract establishes that behavior.

Use the lifecycle pattern supported by the installed Angular version. On newer Angular
versions, `takeUntilDestroyed` may be appropriate. On older versions, use established
project patterns such as `takeUntil` plus `ngOnDestroy` where needed.

### Async state and error handling

For each user-visible asynchronous operation, verify that state transitions are
coherent:

```text
idle -> pending -> success | empty | error
```

The exact UI does not need all four labels. It does need deterministic behavior.
Ensure loading/disabled state is cleared on both success and failure when applicable.
Avoid duplicate submissions when duplicates are invalid.

Handle errors at the layer that can act on them. Do not add `catchError` or toast calls
everywhere if a shared interceptor/service already owns that policy.

Do not treat every empty array, optional value, or false conditional as an error. An
empty result may be a valid application state.

### User feedback

Notifications must follow the product's existing UX contract. Do not require a toast
for every successful operation, warning, validation state, or error.

Prefer the existing pattern, which may be:

- inline validation for form errors;
- contextual error content;
- a global interceptor notification;
- optimistic UI/state change;
- toast/snackbar for asynchronous confirmation;
- no notification when the UI itself already makes success obvious.

Avoid duplicate feedback from both component and interceptor layers.

### Accessibility

Review rendered interaction, not just ARIA syntax:

- semantic native controls before custom roles;
- accessible names and form-label associations;
- keyboard operation for interactive elements;
- visible and logical focus behavior;
- correct state/relationship attributes when custom widgets require them;
- dynamic status/error announcements when users otherwise cannot perceive the update;
- routing/dialog focus behavior where relevant;
- image/media alternatives when present.

Prefer native semantics. Do not add ARIA when HTML already exposes the correct
semantics. Account for the syntax supported by the workspace's Angular version rather
than copying examples from a newer major blindly.

Treat color contrast as a rendered-style concern. Do not infer compliance from a CSS
class name alone. If the environment cannot render or measure the UI, report contrast
as unverified.

### Template and presentation logic

Check for:

- unsafe null assumptions;
- expensive or side-effecting template expressions;
- inaccessible click handlers on non-interactive elements;
- unnecessary duplication;
- confusing conditional rendering;
- inconsistent control state;
- style rules that break layout, focus visibility, or interaction.

Do not rewrite templates simply to match the latest Angular syntax. Preserve the
workspace's supported template features and local conventions.

### Maintainability

Remove code only when evidence shows it is unused or redundant. Prefer names based on
purpose, not implementation detail. Keep lifecycle hooks simple and avoid moving
business logic merely to make the component class look smaller.

When a component contains domain logic that would be safer or easier to test in an
existing service/state layer, report that design issue. Do not silently perform a
cross-layer architectural refactor as part of component hardening.

### Tests

Inspect existing `.spec.ts` files and project testing conventions. Add or modify tests
when the audit changes behavior or when a material defect can be protected by a
focused regression test.

Prioritize observable behavior:

- rendering from inputs/state;
- emitted outputs;
- user interaction;
- loading/error/empty states;
- service interaction boundaries;
- routing or accessibility behavior when relevant.

Do not create low-value tests that merely instantiate the component or duplicate
implementation details to increase test count.

## 5. Finding classification

Before editing, classify material observations:

- **Defect**: incorrect behavior, lifecycle leak/race, broken interaction,
  accessibility failure, or unsafe state transition.
- **Risk**: code is currently functional but has a plausible failure mode supported by
  the implementation.
- **Maintainability**: a concrete complexity or contract problem that impedes safe
  changes.
- **Test gap**: important behavior lacks useful regression coverage.
- **Preference**: stylistic alternative without a demonstrated quality improvement.
- **Needs domain input**: correctness depends on business behavior that code does not
  establish.

Fix defects and justified maintainability problems within scope. Do not churn code for
preferences. Report business-logic uncertainties rather than guessing.

## 6. Implement the smallest safe change

When editing is authorized:

- preserve observable behavior unless the user approved a behavior change;
- preserve public component inputs/outputs unless the contract itself is defective and
  the change is approved;
- follow installed Angular APIs and project conventions;
- avoid unrelated formatting and file-wide rewrites;
- do not add a dependency for a problem the current stack can solve cleanly;
- keep API/service ownership boundaries intact unless a broader refactor was approved.

If a genuine bug fix necessarily changes behavior, state the previous behavior, the
corrected behavior, and the evidence supporting the change.

## 7. Validate Task 1

Use the repository's actual commands rather than assuming `ng test` or `ng lint` is
configured in a particular way. Inspect `package.json`, workspace configuration, and
CI where necessary.

Run the narrowest useful verification first, then broader checks when practical:

1. affected unit/component tests;
2. type checking or Angular build/compile;
3. configured lint/static analysis;
4. broader project tests when the change risk justifies them;
5. rendered/manual accessibility or interaction verification when tooling exists.

Record commands and actual outcomes. Do not say an accessibility issue, race, or UI
behavior is fixed if it was not verifiable in the available environment.

### Task 1 report

For each material finding, report:

```text
Component: <name>
File: <path>
Classification: Defect | Risk | Maintainability | Test gap | Needs domain input
Evidence: <specific code/behavior>
Change: <what changed, or "not changed">
Validation: <command/check and actual result>
```

Then provide a concise file-change inventory:

```text
<path> — added | modified | deleted — <reason>
```

For review-only runs, omit `Change` when no edit was requested.

## 8. Documentation phase

Documentation follows code stabilization only when docs are part of the request.
There is no need to ask the user to reconfirm that Task 1 is complete when the user
already requested both phases and validation succeeded.

If Task 1 leaves unresolved behavior that materially affects the documentation, mark
that section `[NEEDS INPUT]` or ask for the required domain fact. Do not fabricate a
business process from method names.

Create one `component-name.md` beside each documented component unless the repository
has an established documentation location or naming convention that should take
precedence.

### Documentation content

Use only sections supported by actual information:

```markdown
# <Component name>

## Purpose
What the component is responsible for in the application. Distinguish facts observed
in routing/templates/code from business interpretation.

## User flow
The user-visible workflow supported by the component. Omit this section or mark
[NEEDS INPUT] when the real business process cannot be established from trustworthy
project evidence.

## Component contract
### Inputs
### Outputs

Document public component inputs and outputs. Do not list dependency-injected services
as inputs.

## Dependencies
Services, state stores, router/forms facilities, shared components, or libraries that
matter to understanding the component. Explain their role briefly.

## Data and API interactions
For each verified interaction: service method, trigger, known request/response shape,
and UI behavior. State an HTTP endpoint only when it is visible in code/configuration
or another trustworthy project source. Do not infer URLs from service method names.

## Important behavior
Explain non-trivial methods, reactive flows, state transitions, and side effects by
purpose. Do not document trivial getters/setters or restate code line by line.

## Error, empty, and loading states
Only the states implemented by the component and relevant shared layers.

## Accessibility notes
Component-specific semantics, focus, keyboard, labels, or announcements that a
contributor must preserve.

## Tests
What behavior is covered by the component's tests and any important known gap.

## Constraints and change risks
Contracts or assumptions that a contributor should preserve. Use [NEEDS INPUT] for
important unresolved domain knowledge.
```

Documentation describes current verified behavior after the audit. It is not a change
log and should not advertise that the component is "senior standard" or "production
ready."

## 9. Large scopes

For a large feature, work in coherent batches so validation failures remain
attributable. Batch boundaries should follow routing/features/dependencies, not an
arbitrary number of components.

Do not stop merely to announce progress when the task can continue safely. Surface a
checkpoint only when a material decision is needed, validation fails in a way that
affects subsequent work, or repository policy requires approval.

## 10. Boundaries

Do not:

- modernize an Angular version as part of a component audit;
- force `takeUntil` onto every HTTP observable;
- assume `switchMap` is correct for every request;
- claim that client cancellation reverses a server-side mutation;
- add a toast for every success/error without checking existing UX policy;
- treat every `null`, empty array, or false branch as an error;
- invent business processes, API endpoints, payload fields, or user roles;
- add ARIA where native HTML already provides correct semantics;
- rewrite business logic under the label of cleanup;
- generate documentation from pre-refactor code when the requested audit is still
  unresolved;
- claim quality based on code inspection alone when tests/build/runtime behavior were
  not verified.

## 11. Completion criteria

An audit is complete only when:

- the actual Angular/RxJS/TypeScript context was considered;
- scope is traceable to user input or repository evidence;
- material findings are evidence-based and classified;
- edits are limited to justified findings;
- relevant validation was run when the environment permits it;
- unresolved behavior is explicit;
- requested documentation reflects the post-audit implementation;
- business interpretations not supported by code are marked `[NEEDS INPUT]`;
- file changes and validation results are reported accurately.

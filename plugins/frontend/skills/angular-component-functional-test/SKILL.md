---
name: angular-component-functional-test
description: >-
  Analyze, plan, implement, and validate functional tests for an Angular component as a complete UI contract: TypeScript behavior, rendered template bindings, forms, custom controls, child-component events, services, API-facing data contracts, async state, loading/error paths, rapid interactions, and mutation concurrency. Use when functionally testing an Angular component, verifying TS and HTML together, testing form-to-service-to-UI flows, auditing duplicate submissions or stale reads, or replacing shallow creation-only specs with meaningful functional coverage. Discover project, UI, service, and API context before writing tests; never infer contracts from names alone.
argument-hint: "[Angular component path or component name]"
license: MIT
metadata:
  author: Rafi Hariman
  version: "1.2.1"
---

# Angular Component Functional Test

Test an Angular component as a functional boundary: TypeScript, rendered template, Angular bindings, forms, relevant child controls, dependencies, and observable asynchronous state working together.

The objective is not to maximize test count or make every test green. The objective is to determine whether established functional contracts are true.

## Operating principles

1. **Source is authoritative for current behavior.** Project documentation accelerates discovery but does not override current source, configuration, installed packages, or repository state.
2. **Test observable contracts.** A variable, private method, constructor dependency, or lifecycle hook is not a test requirement by itself.
3. **Use the cheapest sufficient layer.** Keep component behavior at the component boundary; test service-owned HTTP serialization at the service boundary.
4. **Trace before mocking.** Never invent endpoints, request fields, response fields, custom-control events, or library behavior.
5. **Async correctness includes concurrency.** A one-request happy path does not prove behavior under rapid interaction.
6. **A failing test can be correct.** Never omit or weaken an established contract merely because production violates it.
7. **Preserve scope.** Functional testing does not authorize production fixes, dependency changes, test-infrastructure repair, or unrelated cleanup.

## Modes

### `analyze`

Discover context, build the functional contract map, identify gaps, and return a test plan. Do not modify files.

### `implement`

Discover context, build the plan, update the appropriate tests, run project-native validation when available, and report functional health.

Do not modify production source unless the user separately authorizes production fixes.

# Phase 0: capture source integrity

Before editing tests, record enough repository state to distinguish pre-existing behavior and changes from work performed by this task.

When Git is available, inspect:

```text
git status --short
git rev-parse HEAD
git diff -- <component/template/spec/relevant-config>
git diff --cached -- <component/template/spec/relevant-config>
```

Do not clean, reset, stage, revert, or normalize the user's worktree.

At completion classify task-related files:

| Change class | Meaning |
|---|---|
| Test implementation | Requested spec, fixture, harness, or focused helper |
| Production source | Application source changed during the task |
| Test infrastructure | Runner, workspace, tsconfig, dependency, or global test setup |
| Pre-existing | Change or failure that existed before this task |

A test must not be reported as proving pre-existing behavior if the task first changed production source to create that behavior.

## Test infrastructure is immutable by default

Without separate authorization, do not:

- edit Karma, Jest, Vitest, Angular workspace, or global testing configuration;
- install, remove, or replace dependencies;
- rename, move, delete, or temporarily hide unrelated specs;
- add exclusions for unrelated failing tests;
- modify unrelated tsconfig/build targets;
- repair unrelated test debt;
- create a temporary tsconfig, alternate runner config, synthetic workspace target, or disposable compiler harness.

Use only the project-native targeting mechanism supported by the installed Angular CLI and configured runner.

If project-native targeting is blocked by unrelated compile/test infrastructure errors, report the target as `Blocked` and state the smallest remediation. Do not apply the remediation automatically.

A disposable isolated harness is allowed only when the user explicitly authorizes isolated execution. When authorized, inherit the real project configuration, remove temporary artifacts afterward, and label the result **Isolated execution**, never project-native or CI-equivalent.

# Phase 1: establish project context

Inspect only context needed by the requested component:

```text
CLAUDE.md
.claude/rules/**
docs/ai/frontend-testing-context.md
package.json
angular.json / workspace.json / project.json
lockfile
tsconfig*.json
karma.conf.* / jest.config.* / vitest.config.*
test bootstrap/polyfills
relevant CI commands
```

Determine:

| Context | Required evidence |
|---|---|
| Angular | Installed version; NgModule or standalone architecture |
| TypeScript / RxJS | Installed versions relevant to tests and async behavior |
| Test runner | Actual repository runner and assertion style |
| Target command | Supported project-native targeting mechanism |
| UI libraries | Installed package/version for custom controls in this component |
| Data layer | Services, clients, stores, facades, enums, request/response models |
| Shared behavior | Interceptors, error handling, notifications, auth, global loading |
| Test utilities | Existing harnesses, fixtures, factories, doubles, helpers |

Do not import modern Angular testing APIs into a legacy project without version evidence.

Project testing documentation is a discovery index, not a duplicate database of endpoints/components. Current source wins when documentation is stale.

# Phase 2: resolve the component boundary

Read the sources defining observable behavior:

```text
component.ts
component.html
component.spec.ts if present
module/standalone configuration
route/host context when relevant
styles only when they materially change interaction/visibility
```

Resolve relevant imports and inspect:

- lifecycle effects and cleanup ownership;
- template-bound state;
- reactive/template-driven forms and validators;
- inputs and outputs;
- observables, subjects, signals, promises, subscriptions;
- injected services/stores/facades/providers;
- pipes/directives that alter functional output;
- storage/browser APIs;
- router/navigation;
- modal/dialog/drawer flows;
- debounce, polling, timers, and request orchestration.

Do not invent lifecycle requirements that the source does not have.

# Phase 3: build the interaction inventory

Parse the actual template. Do not assume the component contains only select, button, and table controls.

For every native or custom interactive element determine:

- how state enters it;
- how user interaction leaves it;
- which form/control/component state changes;
- which conditional UI changes;
- whether it starts async work;
- whether repeated activation can occur;
- how pending, disabled, error, success, empty, and retry states behave.

Consider any interaction family actually present:

| UI family | Contracts to inspect |
|---|---|
| Text/number/masked/date/textarea | DOM event, form value, validation, normalization, payload |
| Select/autocomplete/multiselect | Value semantics, dependent options, reset, async lookup, stale selection |
| Button/link/menu | Reachability, handler, navigation/mutation, repeated activation |
| Checkbox/radio/toggle | Model binding, conditional UI, payload representation |
| Table/grid/list | Rows, paging, sorting, filters, selection, actions, empty/loading/error |
| Tabs/accordion/stepper/wizard | Active state, lazy content, state preservation/reset, gates |
| Modal/dialog/drawer/overlay | Open payload, confirm/cancel/close events, refresh flow |
| Upload/download/import/export | File contract, request, progress, cancellation/error |
| Pagination/infinite scroll | Page/cursor, duplicate loads, append/replace, stale response |
| Search/typeahead | Debounce, thresholds, latest-intent semantics, reset |
| Router/navigation | Destination, params/query, dirty-state/return flow |
| Permission/feature flags | Visibility and client action state; never infer server authorization |
| Async status UI | Idle, pending, success, empty, error, retry, disabled state |

Unknown custom components are not automatically stubbed. Resolve their public API, installed package, declaration, or existing test harness first.

# Phase 3.1: verify primary-action reachability

Identify primary user actions such as Search, Save, Generate, Submit, Confirm, Pay, Delete, Continue, Upload, Download, and Retry.

For each action determine:

- legitimate prerequisites;
- when it becomes actionable;
- whether initialization can leave it permanently unavailable;
- whether pending work prevents duplicate activation;
- whether every owned terminal path restores the intended state.

Test rendered/public behavior rather than the internal flag name.

If an established primary action remains unreachable after its prerequisites settle, write a failing regression test. Do not keep the suite green by turning the defect into prose only.

# Phase 4: build the functional contract map

Group methods, bindings, UI events, and dependencies into observable behavior rather than one row per method.

Assign **Materiality** before computing overall functional health:

| Materiality | Meaning |
|---|---|
| Material | Failure can block a primary task, produce wrong loaded/persisted data, duplicate a mutation, leave UI stuck, expose incorrect authorization-sensitive UI, or violate an established domain contract |
| Advisory | Relevant audit observation that is not established as required for the primary task and does not currently justify failing overall health |
| [NEEDS INPUT] | Impact cannot be established and could be Material |

Never downgrade a contract to Advisory merely to produce a passing report.

Use this contract shape:

| Contract | Trigger | Input/state | Operation | Observable outcome | Materiality | Evidence |
|---|---|---|---|---|---|---|
| Search | Form submit | Current filters | Load results | Pending then result/empty/error | Material | TS + HTML + service |

Lifecycle hooks are tested through their effects when possible. Prefer `create component -> required initial state exists` over spying that `ngOnInit` called a private setup method.

# Phase 5: trace dependency and API contracts

Trace the actual ownership chain:

```text
component
  -> service/store/facade/client
  -> operation method/enum
  -> HTTP/client implementation when needed
  -> URL/config resolution
  -> request params/body/type
  -> response/error mapping
  -> existing contract test/OpenAPI/backend source when available
```

Component tests normally prove component-owned service arguments, pending state, and controlled result-to-UI behavior.

Service/API tests normally prove HTTP method/URL/headers/query/body serialization and service-owned response/error behavior.

Never infer endpoint or schema from an enum/method name. Use `Unknown` or `[NEEDS INPUT]` when evidence is unavailable.

# Phase 6: map async state, ownership, and concurrency

Every user-triggered async operation receives an ownership record:

```text
Action: <action>
Trigger: <public trigger>
Owner: component | service | store | mixed
Operation: read | mutation
Ordering: latest-wins | first-wins | queue | all-complete | unknown
Pending UI: <observed>
Success: <observed>
Empty: <observed>
Error owner: <observed>
Destroy owner: <observed>
Race/duplicate test: required | not required | [NEEDS INPUT]
```

Trace through service/store/facade wrappers before declaring cancellation/error/duplicate protection missing.

Report local and effective protection separately when ownership crosses layers.

## Terminal-state matrix

For each async flow test only reachable states owned by the component or relevant layer:

```text
idle
  -> pending
      -> success -> settled
      -> empty -> settled
      -> error -> settled
      -> superseded/cancelled -> no stale commit
      -> destroy -> no late component-owned update
```

If shared infrastructure fully owns an error path, document that owner and test it there instead of inventing a component expectation.

## Read operations

For filters/search/typeahead/paging/dependent controls, determine whether new user intent supersedes old intent.

Do not require `switchMap` by name. Test behavior:

```text
request A starts
state changes; request B starts
B resolves; UI reflects B
A resolves late
UI still reflects B
```

Debounce and distinctness reduce trigger frequency but do not, by themselves, prove ordering between requests already in flight.

If expected ordering is not established, mark the applicable contract `[NEEDS INPUT]`; never mark it Not applicable merely because policy is unknown.

## Mutations and duplicate acceptance

For every user-triggered async write that can remain pending, explicitly test duplicate acceptance when duplicate writes would be materially harmful.

A disabled button is a **UX guard**, not sufficient proof of mutation correctness.

When single acceptance is the contract:

```text
trigger mutation #1
keep mutation #1 pending
verify pending/disabled UI when applicable
attempt the same mutation again through the public UI/component path
assert service/write call count remains exactly 1
settle mutation #1 with success or error
verify owned pending state clears
```

If duplicate activation is demonstrably impossible at the public boundary, record that evidence. Otherwise do not label duplicate protection Proven solely from `[disabled]` state.

Do not prescribe `exhaustMap`, an explicit guard, or another implementation unless the user asks for a fix. Test first-operation-wins semantics when that is the established contract.

Do not claim client unsubscription rolls back a server mutation.

# Phase 6.1: preserve boundary fidelity

A controlled double proves the component's reaction to the double. It does not prove the real dependency produces the same event/result.

Keep distinct:

| Claim | Example |
|---|---|
| Parent boundary | Parent reloads when controlled modal `onClose` emits |
| Real child contract | Real modal emits `onClose` on its real close path |
| Integration | Real close path causes parent refresh |

Do not promote parent-boundary coverage into real child/service integration coverage.

# Phase 6.2: detect contract drift

When documentation, contributor notes, existing tests, source, or API contracts disagree, report **Contract drift** rather than silently choosing one expected behavior.

Source is authoritative for what current code does. Project/product/API evidence may be authoritative for what it should do.

Example:

| Evidence A | Evidence B | Observed behavior | Status |
|---|---|---|---|
| Doc says selected program survives | Source resets program | Selection clears | Contract drift / [NEEDS INPUT] |

# Phase 7: inspect existing protection

Read existing tests before proposing new ones.

| Coverage | Meaning |
|---|---|
| Protected | Existing assertion would fail if the contract regressed |
| Partial | Related test exists but misses a material part |
| Missing | No relevant protection exists |
| Unknown | Existing setup cannot be interpreted confidently |

A test name is not evidence. Read setup, stimulus, dependency controls, and assertions.

## Contract applicability and status

```text
Does the functional path exist?
  no -> Not applicable
  yes
    -> Is expected behavior established?
         no -> [NEEDS INPUT] or Unproven
         yes -> Does evidence prove it?
                  yes -> Proven
                  no -> Failed
```

Use `Not applicable` only when the path is absent.

When execution is blocked, distinguish evidence provenance:

- **Failed (executed)**: an executed test/check reproduced the violation;
- **Failed (source-proven)**: a deterministic reachable source path violates an established contract even though execution is blocked;
- **Unproven**: source suggests a behavior/risk but evidence is insufficient to prove the contract;
- **[NEEDS INPUT]**: expected product behavior or impact cannot be established.

Do not promote downstream effects beyond the traced boundary. For example, frontend evidence may prove duplicate POST acceptance; duplicate backend records/billing remain `[NEEDS INPUT]` unless backend concurrency/idempotency behavior was traced.

# Phase 8: test inclusion gate

Include tests only when they increase meaningful protection:

- observable user/integration behavior;
- error, race, stale-state, or duplicate-action failure;
- an important boundary not reliably covered lower down;
- request/payload/domain transformations with behavioral consequences;
- state transitions that can leave UI stuck or duplicate effects;
- established defects.

Reject cases that only:

- assert Angular/TypeScript wiring guarantees;
- verify constructor injection without behavior;
- spy on internals when public outcome is observable;
- freeze non-contract markup/CSS/DOM nesting;
- duplicate existing assertions;
- test third-party internals instead of application integration;
- increase test count without increasing protection.

## Finding severity and impact

Do not assign Low/Medium/High from code shape alone. Severity requires evidence of user, data, workflow, security, or operational impact.

When impact is unresolved, use `Impact: [NEEDS INPUT]`.

Separate evidence from remediation:

```text
Observed: <what happens>
Owner: <component/service/etc.>
Status: Failed / Unproven / [NEEDS INPUT]
Required outcome: <functional condition>
Remediation: outside testing scope unless requested
```

## Dead/unreachable functional paths

Report TypeScript state/handlers/branches with no reachable template, host, route, service, or public-event path separately as `Dead/unreachable functional path`.

Do not count dead paths as failed functional contracts unless an established requirement says they should be reachable. Removal/refactoring needs separate authorization.

## Avoid combinatorial waste

Derive tests from actual branches and equivalence classes. Eight independent filters do not imply 256 test combinations.

One scenario may protect multiple connected contracts when it reflects a real flow and remains diagnosable. Split when error/concurrency semantics differ or ownership belongs to another layer.

# Phase 9: choose the test layer

| Layer | Use when |
|---|---|
| Pure/class | Transformation/state logic needs no rendered Angular behavior |
| Component functional | Template/forms/child outputs/rendering/service interaction/async UI |
| Service/API contract | HTTP/client serialization and response handling owned by service |
| Existing harness | Custom/shared widget exposes stable harness/public API |
| Browser/E2E | Browser/runtime mechanics cannot be represented reliably lower down |
| Build/type/lint | Static/compile contract only; never a substitute for runtime tests |

# Phase 10: plan before implementation

In implement mode, build the evidence-based plan before editing the spec.

Do not ask for confirmation for ordinary test implementation already authorized by the user. A separate decision is required when proceeding would modify production source, dependencies, test infrastructure, testing architecture, or public behavior.

Use Markdown tables for functional contracts, async contracts, test plan, and evidence.

# Phase 11: implement with project conventions

- update the existing spec when that is repository convention;
- reuse existing fixtures/factories/harnesses/helpers;
- use the installed runner/assertion style;
- exercise real Angular forms/template behavior when under test;
- mock only the boundary required for deterministic control;
- keep async work pending long enough to assert loading/race/duplicate behavior;
- clean test-owned storage/global state;
- restore spies/mocks;
- keep tests isolated and order-independent.

Do not use blanket stubs/schemas when they erase the binding/event contract being tested.

# Phase 12: validate

Validation scopes are independent:

| Scope | Meaning |
|---|---|
| Generated component tests | Whether implemented scenarios executed and passed/failed |
| Component functional health | Health of Material functional contracts |
| Target type/build | Whether relevant target/spec compiled/type-checked |
| Project-wide tests | Health of containing project suite |

Run the narrowest project-native checks first. Do not mutate infrastructure to obtain a green run.

Label execution provenance:

- **Project-native**: repository-defined configuration and supported target selection;
- **Isolated execution**: explicitly authorized disposable harness;
- **Blocked**: project-native execution cannot proceed and isolated execution is not authorized.

Never present isolated execution as CI-equivalent.

When tests are written but neither compiled nor run, report **Spec implementation confidence: Unvalidated**.

# Phase 13: report results

Lead with validation scope and source integrity, not “all tests passed.”

## Canonical Markdown report file policy

When the user asks to save the audit/report as Markdown, create **one canonical file by default**:

```text
<component-name>.functional-report.md
```

Keep the concise executive summary inside the canonical report under `## Summary`.

Do **not** automatically create a separate `functional-summary.md`.

Create separate summary and detailed Markdown files only when:

1. the user explicitly requests separate artifacts; or
2. a **verified repository convention** requires both.

A repository convention is verified only by project documentation, checked-in contributor instructions, or multiple independent human-maintained examples.

Do **not** infer repository convention solely from files produced by a previous invocation of this skill or another AI agent. Previous AI-generated artifacts must not become self-reinforcing conventions without independent project evidence.

If a sibling `functional-summary.md` exists but its provenance is unknown, do not copy that pattern automatically. The full `functional-report.md` remains authoritative.

If a separate summary is explicitly required, it must be derivative: it must not introduce statuses, findings, or evidence absent from the canonical report.

Do not create Markdown report files when the user only asked to implement tests or receive the result in chat. File creation requires an explicit save/write request or a verified project workflow that requires the artifact.

## Canonical report structure

```markdown
# <Component> — Functional Test Report

## Summary
...

## Validation scopes
| Scope | Status | Evidence |
|---|---|---|
| Generated component tests | Pass / Fail / Blocked | ... |
| Spec implementation confidence | Validated / Unvalidated | ... |
| Component functional health | Pass / Fail / Incomplete | ... |
| Target type/build | Pass / Fail / Not run | ... |
| Project-wide tests | Pass / Fail / Blocked / Not run | ... |

## Source integrity
| File | Before task | After task | Changed by task | Purpose |
|---|---|---|---|---|
| ... | ... | ... | ... | ... |

## Functional contracts
| Contract | Applicability | Materiality | Status | Evidence | Gap |
|---|---|---|---|---|---|
| ... | Applicable | Material | Proven | ... | None |
| ... | Applicable | Material | Failed (executed/source-proven) | ... | ... |
| ... | Applicable | Advisory / [NEEDS INPUT] | [NEEDS INPUT] | ... | ... |

## Async operations
| Action | Owner | Pending state | Concurrency/error ownership | Terminal coverage | Status |
|---|---|---|---|---|---|
| ... | ... | ... | ... | ... | ... |

## Contract drift
| Evidence A | Evidence B | Observed behavior | Status |
|---|---|---|---|
| ... | ... | ... | ... |

## Findings
| Materiality | Impact/severity | Finding | Evidence | Required outcome |
|---|---|---|---|---|
| ... | ... | ... | ... | ... |

## Dead/unreachable functional paths
| Path | Evidence | Status |
|---|---|---|
| ... | ... | Advisory |

## Project blockers
| Scope | Status | Summary |
|---|---|---|
| ... | ... | ... |
```

Use status labels precisely:

- **Proven**: executed evidence demonstrated an established contract;
- **Failed (executed)**: runtime/test execution reproduced an established violation;
- **Failed (source-proven)**: deterministic reachable source path violates an established contract while execution is blocked;
- **Unproven**: contract is applicable but evidence is insufficient;
- **[NEEDS INPUT]**: expected behavior, impact, or ownership cannot be established;
- **Not applicable**: functional path does not exist;
- **Blocked**: required evidence/execution is unavailable.

Compute overall health from Material contracts:

- **Pass**: all established Material contracts are Proven and no potentially Material contract has unresolved materiality;
- **Fail**: at least one Material contract is Failed (executed or source-proven);
- **Incomplete**: no Material contract Failed, but a Material/potentially Material contract is Unproven, `[NEEDS INPUT]`, or Blocked.

Advisory findings remain visible but do not make health Incomplete by themselves.

## Project blocker reuse

When the same project-wide blockers were already verified in the current session at the same Git HEAD, the skill may reuse that evidence instead of fully re-diagnosing every unrelated spec on each component. Verify HEAD and relevant test configuration have not changed, then cite the existing blocker set and perform only a minimal sanity check.

# Production defect handling

Do not silently repair production code during a testing task.

When expected behavior is established, keep the failing/source-proven contract and report the defect. When behavior is unresolved, use `[NEEDS INPUT]` rather than encoding preference.

# Stop conditions

Stop implementation and report the exact blocker when a material contract cannot be established safely, including unknown test stack, untraceable custom-control contract, unavailable critical API contract, ambiguous error/loading ownership, or blocked project-native execution without isolated-run authorization.

Do not compensate with invented mocks or infrastructure edits.

# Output quality

Write engineering evidence, not generic AI commentary. Use concrete symbols, paths, state transitions, executed checks, and explicit boundaries.

Prefer:

```text
Duplicate POST acceptance is reachable because two confirmation callbacks can call `insert` while the first request is still pending; downstream duplicate-record impact is [NEEDS INPUT].
```

Avoid:

```text
Robust asynchronous handling is important for a seamless user experience.
```

# Completion criteria

The task is complete only when applicable items are satisfied:

- source integrity was captured without modifying user state;
- framework/test versions were discovered rather than assumed;
- TS and template were analyzed together;
- custom UI contracts were resolved when material;
- service/API contracts were traced to trustworthy sources;
- primary actions were checked for reachability;
- async ownership and terminal states were mapped;
- read concurrency was assessed where applicable;
- duplicate mutation acceptance was explicitly exercised when materially relevant;
- controlled doubles did not overstate real integration coverage;
- contract materiality was assigned before overall health;
- unsupported severity labels were replaced with evidence or `[NEEDS INPUT]`;
- documentation/runtime disagreement was labeled Contract drift;
- dead paths remained separate from failed contracts;
- existing tests were read before new tests were selected;
- unnecessary and combinatorial tests were filtered out;
- production source and unrelated test infrastructure stayed unchanged unless separately authorized;
- disposable test/compiler harnesses were not created without explicit authorization;
- validation provenance is Project-native, Isolated execution, or Blocked;
- written-but-unrun specs are labeled Unvalidated;
- task-created file changes were reported;
- Markdown reporting creates one canonical `functional-report.md` by default;
- no separate summary file is created unless explicitly requested or required by verified repository convention;
- prior AI-generated artifacts are never treated as repository convention without independent evidence;
- final output uses concise Markdown tables and explicit evidence.

---
name: javascript-pro
description: >-
  Implement, debug, refactor, and review real JavaScript projects using the runtime,
  module system, browser targets, test runner, lint configuration, and architectural
  conventions already established by the repository. Use for .js, .mjs, and .cjs
  applications, Node.js services and CLIs, vanilla browser applications, Web Workers,
  Service Workers, Fetch-based flows, streams, events, async iterators, and JavaScript
  libraries. Prefer modern JavaScript when the project's declared runtimes support it,
  but never force an ECMAScript edition, ESM migration, Jest, a coverage percentage,
  lint auto-fixes, or broad modernization that the project did not request.
argument-hint: "[JavaScript file, feature, bug, module, or task]"
license: MIT
metadata:
  author: Rafi Hariman
  based-on: "JavaScript Pro by Jeff Allan / Synergetic Solutions"
  upstream-license: MIT
  version: "1.0.0"
  domain: language
  role: specialist
  scope: implementation-review
---

# JavaScript Pro — Project-Aware Edition

Work on JavaScript as it actually exists in the target repository.

The repository's runtime contract, module system, test stack, compatibility targets, and
established architecture are authoritative. Modern syntax and APIs are tools, not goals.

The objective is to produce the smallest correct change that behaves correctly in the
project's supported environments and is validated with project-native tooling.

## Use boundaries

Use this skill for JavaScript-language implementation, debugging, refactoring, and
review. Route nearby work elsewhere:

- TypeScript type-system design or compiler errors: `typescript-pro`.
- Angular component audits and functional tests: the Angular skills.
- Full trust-boundary security audits: `feature-security-audit`.
- SonarQube-specific remediation: `sonarqube`.

## Core principles

1. **Discover before modernizing.** Read runtime, module, lint, test, build, and browser
   targets before selecting syntax or APIs.
2. **Project truth beats stylistic preference.** Do not migrate CommonJS to ESM, Jest to
   another runner, callbacks to Promises, or older supported syntax merely because a
   different style is newer.
3. **Use the correct async abstraction.** Prefer `async`/`await` for linear Promise flow;
   preserve streams, events, async iterators, workers, and Promise combinators when those
   abstractions model the problem better.
4. **Preserve error semantics.** Catch an error only when this layer can recover,
   translate, enrich, clean up, retry, or establish an error boundary.
5. **Validate without hidden scope expansion.** Run project-native checks and keep
   auto-fixes, mass formatting, dependency changes, and architecture migrations separate
   unless explicitly requested.
6. **Tests protect behavior, not a percentage.** Use the repository's existing runner and
   coverage policy. Do not invent an 85% or other numeric threshold.
7. **Performance work requires evidence.** Do not optimize, profile, inspect heaps, or
   change algorithms without a relevant workload, symptom, budget, or regression signal.
8. **Browser compatibility is a runtime contract.** New ECMAScript syntax or Web APIs are
   acceptable only when native support or the project's transpilation/polyfill strategy
   establishes compatibility.
9. **Node compatibility is a runtime contract.** Respect `engines`, version managers,
   containers, CI, deployment runtimes, and supported LTS lines.
10. **Do not hide failure.** Avoid broad catch-and-return-null patterns unless `null` is the
    established domain representation of that failure.

## Modes

### `implement`

Implement or modify requested JavaScript behavior using existing project architecture.

### `debug`

Reproduce or trace the failure, identify the smallest root cause, apply a minimal fix when
authorized, and validate the relevant path.

### `review`

Review JavaScript correctness, async/resource lifecycle, compatibility, error ownership,
module boundaries, and project conventions without changing files unless requested.

### `refactor`

Refactor only the requested scope while preserving observable behavior. Do not combine a
refactor with module-system migration, dependency replacement, formatting sweep, or broad
modernization unless explicitly authorized.

## Phase 0: capture source integrity and scope

When Git is available, record enough state to distinguish pre-existing work from task
changes:

```text
git status --short
git rev-parse HEAD
git diff -- <target paths>
git diff --cached -- <target paths>
```

Do not clean, reset, stage, revert, or normalize the user's worktree.

Before editing, state the requested boundary:

```text
Target files/modules:
Observed dependent modules:
Production behavior allowed to change:
Out-of-scope modernization/refactor:
```

Keep unrelated changes untouched.

## Phase 1: discover the JavaScript runtime contract

Inspect only relevant project files, typically:

```text
package.json
package-lock.json / pnpm-lock.yaml / yarn.lock / bun.lock*
.nvmrc / .node-version / volta config
browserslist / package.json browserslist
babel.config.* / .babelrc*
tsconfig/jsconfig when JavaScript tooling uses them
vite.config.* / webpack.* / rollup.config.* / esbuild config
eslint.config.* / .eslintrc*
prettier/editor config when relevant
jest/vitest/mocha/tap/node:test configuration
CI workflow and container/runtime files when they define supported Node/browser versions
```

Determine:

| Contract | Evidence to resolve |
|---|---|
| Runtime | Browser, Node, worker, service worker, CLI, library, mixed |
| Node support | `engines`, CI/container/version-manager evidence |
| Browser support | browserslist, transpiler/bundler targets, deployed environment |
| Module system | ESM, CommonJS, dual package, bundler-managed |
| File semantics | `.js`, `.mjs`, `.cjs`, package `type` |
| Test runner | Existing installed/configured runner |
| Lint/style | Existing ESLint/project rules and scripts |
| Build | Existing bundle/transpile/package process |
| Coverage | Existing repository thresholds, if any |
| Error model | thrown errors, Result/status objects, callbacks, events, centralized boundary |

If evidence conflicts, preserve current working behavior and report the conflict before
changing architecture.

## Phase 2: choose syntax and APIs from support evidence

Do not require a named ECMAScript edition such as ES2023 merely because it is modern.

Use the newest syntax/API that is compatible with the declared environment and project
transformation pipeline.

Examples to evaluate before use:

- optional chaining and nullish coalescing;
- private fields;
- top-level `await`;
- import attributes/dynamic import;
- `structuredClone`;
- Web Streams;
- `AbortController` / `AbortSignal`;
- `fetch` in Node;
- `Temporal`;
- iterator helpers and other newly standardized runtime APIs.

Syntax support and API support are distinct. A transpiler may transform syntax without
polyfilling a missing runtime API.

Do not add a transpiler/polyfill/dependency merely to use a newer construct unless the
user authorizes that dependency and compatibility change.

## Phase 3: preserve module-system semantics

Resolve module behavior from file extension and package/config evidence.

### Existing projects

Preserve the established module system by default.

Do not:

- convert CommonJS to ESM during an unrelated feature/fix;
- change `package.json` `type` merely for style;
- rename `.js`/`.mjs`/`.cjs` files without need;
- mix `require()` and ESM imports in a way the runtime does not support;
- introduce default/named export churn without a public-API reason.

### New greenfield JavaScript modules

ESM is a reasonable default only when the target runtime/tooling supports it and no
project convention says otherwise.

For libraries, inspect package exports, consumers, bundler expectations, and whether dual
CJS/ESM output is already part of the package contract.

## Phase 4: async and concurrency semantics

Choose the abstraction based on behavior.

### Promise flows

Use `async`/`await` when it improves a sequential Promise workflow.

Do not mechanically rewrite useful concurrency:

```js
const [user, permissions] = await Promise.all([
  getUser(id),
  getPermissions(id),
]);
```

into serial awaits unless ordering is required.

### Events and streams

Do not force event emitters, Node streams, browser events, message ports, or Web Streams
into one-shot Promise patterns when the source naturally emits multiple values.

### Async iterators

Use `for await...of` when consuming an async iterable is the established/appropriate API.

### Cancellation

For abortable async work, propagate existing cancellation semantics such as
`AbortSignal` rather than inventing global flags.

Determine ownership:

```text
Who starts the operation?
Who may cancel it?
Who owns timeout/retry?
Who releases listeners/resources?
Who handles terminal errors?
Can stale completion overwrite newer intent?
Can repeated activation duplicate a mutation?
```

Do not claim `async`/`await` solves races, stale responses, cancellation, or duplicate
writes by itself.

## Phase 5: error ownership

Do not require `try/catch` in every async function.

Catch errors when the current layer can do at least one of:

- recover with a valid fallback;
- translate a low-level error into a domain/public error;
- add material context and rethrow with preserved cause;
- clean up owned state/resources;
- implement an established retry/circuit policy;
- convert the error into the project's explicit Result/status protocol;
- terminate at a CLI/request/UI process boundary with appropriate reporting.

Let errors propagate when a higher layer is the established owner.

Prefer preserving causal information:

```js
try {
  return await repository.load(id);
} catch (error) {
  throw new UserLoadError(`Unable to load user ${id}`, { cause: error });
}
```

Do not use this generic pattern unless the API contract truly defines `null` as failure:

```js
try {
  return await doWork();
} catch (error) {
  console.error(error);
  return null;
}
```

Distinguish absence from failure.

## Phase 6: nullish and optional values in JavaScript

JavaScript does not use TypeScript union syntax such as `X | null` in source code.

Use project conventions and explicit runtime semantics:

- choose `null` when the API/domain intentionally represents “known empty” with `null`;
- choose `undefined` when a value is absent/not supplied according to project conventions;
- do not normalize between them merely for personal preference;
- use optional chaining when the receiver is legitimately optional;
- use nullish coalescing when only `null`/`undefined` should trigger a default;
- do not replace valid falsy semantics (`0`, `false`, `""`) with a default accidentally.

Example:

```js
const timeoutMs = config.timeoutMs ?? DEFAULT_TIMEOUT_MS;
```

Do not assume:

```js
const timeoutMs = config.timeoutMs || DEFAULT_TIMEOUT_MS;
```

is equivalent when `0` is a meaningful value.

If the repository uses JSDoc typing, follow its established JSDoc conventions instead of
inventing TypeScript syntax inside JavaScript.

## Phase 7: browser JavaScript

When the target runs in browsers, inspect relevant browser contracts.

### DOM and rendering

Prefer framework/project abstractions where established. Avoid raw HTML/string execution
sinks unless required and safely handled.

### Network

For `fetch` or equivalent APIs:

- distinguish HTTP error status from network rejection;
- follow project authentication/credential policy;
- honor existing cancellation/timeouts;
- parse response bodies according to content/status contracts;
- do not retry non-idempotent mutations automatically without an established policy.

### Listeners and observers

For event listeners, observers, intervals, workers, channels, and subscriptions:

- identify lifecycle owner;
- release long-lived resources when the owner terminates;
- avoid duplicate registration;
- verify message/source/origin assumptions when security-relevant.

### Workers

For Web Workers/Service Workers:

- preserve structured-clone/transfer semantics;
- handle worker errors and termination ownership;
- avoid assuming DOM access in dedicated workers;
- for service workers, treat caching/update/offline behavior as persistent application
  state, not a simple function call.

### Blocking work

Avoid CPU-heavy synchronous work on UI-critical browser paths. When material, assess
chunking, workers, streaming, or algorithmic changes based on measured workload rather
than arbitrary thresholds.

## Phase 8: Node.js JavaScript

Respect the Node version supported by the repository, not the machine running the agent.

### I/O

Prefer asynchronous I/O on request-serving, concurrent, or latency-sensitive paths.

Synchronous I/O is not universally forbidden. It can be valid when blocking is deliberate
and bounded, for example:

- process startup before serving traffic;
- small one-shot CLI utilities;
- migrations/build scripts;
- test/bootstrap setup;
- configuration loading where synchronous startup behavior is intentional.

Document or avoid synchronous I/O when used on hot/concurrent paths.

### Streams

Respect backpressure and error/close lifecycle. Do not replace a streaming design with
whole-buffer reads merely to use `await`.

### Processes and workers

For child processes/worker threads:

- define input/output/error/exit ownership;
- avoid shell construction with untrusted input;
- clean up or terminate owned workers/processes;
- preserve cancellation and timeout policy.

### Servers

Do not swallow request errors or leave promises detached. Preserve framework-established
request/error boundaries and graceful-shutdown behavior.

## Phase 9: performance and memory

Do not run heavyweight profiling on every JavaScript change.

Trigger performance/memory investigation when at least one applies:

- user reports a performance/memory problem;
- benchmark/budget regressed;
- change adds long-lived listeners, caches, workers, streams, timers, or retained graphs;
- hot-loop/large-data processing is modified;
- project CI has bundle/performance budgets;
- profiling is explicitly requested.

Use the project's relevant tools, for example Node inspector/heap snapshots, browser
DevTools, framework bundler analysis, or an existing benchmark suite.

Do not claim a memory leak merely because an object remains allocated at one snapshot.
Establish unintended retention/lifecycle behavior.

## Phase 10: implementation discipline

When editing:

- make the smallest change that satisfies the requested behavior;
- preserve public API unless a breaking change is authorized;
- preserve function/module naming and architectural patterns;
- use `const` when a binding is not reassigned and `let` when it is;
- avoid `var` in new code unless preserving generated/legacy semantics makes changing it
  riskier and no modernization was requested;
- do not mutate parameters unless the API explicitly defines in-place mutation and
  callers rely on it;
- add JSDoc where it clarifies a non-obvious public contract, callback shape, generic-like
  value, thrown error, or complex function; do not comment obvious syntax;
- avoid speculative abstractions and premature helper extraction.

## Phase 11: testing

Discover and use the installed project-native runner. Possible runners include Jest,
Vitest, Node's built-in test runner, Mocha, AVA, tap, Jasmine, or project-specific tools.

Do not install or migrate to Jest merely because this skill knows Jest.

Test material behavior:

- requested happy path;
- meaningful branch/error path;
- async terminal behavior;
- cancellation/cleanup when owned;
- race/repeated-action behavior when material;
- public API/module compatibility;
- regression that motivated a bug fix.

Coverage policy:

1. use the repository's configured threshold when present;
2. if CI/package scripts enforce coverage, run that project-native check when practical;
3. if no threshold exists, do not invent 85% or another percentage;
4. do not write low-value assertions merely to increase coverage.

Keep deterministic tests independent of wall-clock timing when controllable clocks,
fixtures, fake timers, or explicit deferred Promises are project-appropriate.

## Phase 12: lint, build, and validation

Run the narrowest existing project-native checks first.

Typical discovery:

```text
package.json scripts
README/CONTRIBUTING/CLAUDE instructions
CI workflows
lint/test/build configuration
```

Validation order when applicable:

```text
targeted tests
  -> lint/check for changed scope
  -> type/JSDoc check if project uses it
  -> build/package check
  -> broader tests only when justified
```

### Lint mutation policy

Do not default to `eslint --fix`.

Run existing lint/check commands read-only first. Apply auto-fixes only when:

- the task authorizes code modification;
- the project's normal workflow uses them;
- the affected scope is controlled;
- unrelated user changes will not be rewritten.

If the project uses an older ESLint configuration, do not migrate it merely because a
newer ESLint supports flat config.

### Build/bundle

Verify bundle size only when the project has a bundle, budget, or material size concern.
Use project-native budgets/configuration rather than an invented threshold.

## Phase 13: report outcomes

Report what changed and what was actually validated.

Recommended result shape:

```markdown
## Result
- requested behavior
- files changed
- runtime/module assumptions used

## Validation
| Check | Status | Evidence |
|---|---|---|
| Targeted tests | Pass/Fail/Blocked/Not run | ... |
| Lint | Pass/Fail/Blocked/Not run | ... |
| Build/package | Pass/Fail/Blocked/Not run | ... |
| Coverage threshold | Pass/Fail/Not configured/Not run | ... |
| Performance/memory | Not applicable / Checked / Needs input | ... |

## Compatibility
- Node/browser targets verified
- module system preserved
- dependency changes, if any

## Limitations
- checks not run
- unrelated project blockers
- behavior requiring environment-specific validation
```

Never say “validated” when a check was not executed.

## Security boundary

This is a JavaScript implementation/correctness skill, not a full security audit.

While editing, avoid introducing obvious unsafe behavior such as code execution from
untrusted strings, unsafe DOM injection, shell construction from untrusted values, or
secret leakage. For deeper trust-boundary, authorization, business-logic abuse, replay,
data-isolation, or client-security analysis, invoke the project's dedicated security
audit workflow when available.

Do not duplicate a dedicated SAST/security scanner manually.

## Stop and ask/decide separately when

A requested change would materially require:

- changing Node/browser support policy;
- migrating CommonJS to ESM or creating dual-package output;
- adding/changing a transpiler or polyfill strategy;
- replacing the test runner;
- adding a new major dependency;
- changing public package exports/API;
- changing production behavior outside requested scope;
- broad performance architecture changes;
- mass lint/format migration.

These are architecture/workflow decisions, not incidental implementation cleanups.

## Completion criteria

The task is complete when applicable items are satisfied:

- source integrity and requested scope were identified;
- runtime/browser/Node compatibility came from project evidence;
- module semantics were preserved unless migration was authorized;
- syntax/API choices are supported by the actual target or transformation pipeline;
- async abstraction matches the problem rather than an arbitrary style rule;
- error ownership and propagation remain semantically correct;
- nullish behavior does not use TypeScript-only syntax in JavaScript;
- browser/Node resource lifecycle is handled when owned by changed code;
- synchronous I/O is judged by execution context rather than universally banned;
- tests use the project's runner and protect material behavior;
- repository coverage policy is honored without inventing a percentage;
- lint runs project-native configuration and does not auto-fix unrelated files;
- performance/memory work is evidence-triggered rather than mandatory ceremony;
- no unrelated modernization, dependency, or infrastructure changes were introduced;
- executed validation is reported separately from checks that were blocked/not run;
- remaining compatibility/runtime limitations are explicit.

---
name: typescript-pro
description: >-
  Review, design, and repair TypeScript types for correctness at compile-time and at
  runtime boundaries. Use when the user asks for a TypeScript type-safety review,
  strictness hardening, removal of unsafe any/assertions, API or library type design,
  generic/union/narrowing work, or diagnosis of TypeScript compiler errors. Inspect
  the project's installed TypeScript version, tsconfig, lint rules, framework, and
  public compatibility constraints before recommending language features or compiler
  flags. Do not refactor sound code merely to introduce advanced type-system features.
argument-hint: "[TS file, compiler error, type/API to design, or review scope]"
tools: Read, Grep, Glob
license: MIT
metadata:
  author: Rafi Hariman
  version: "2.0.0"
---

# TypeScript Pro

Improve TypeScript correctness with the smallest type model that represents the real
runtime domain. Type sophistication is not a goal by itself. Prefer code that the
compiler can verify, maintainers can understand, and runtime validation can support.

## 1. Establish the project baseline

Before applying a TypeScript rule, inspect the relevant project configuration:

- installed TypeScript version;
- `tsconfig.json` and any inherited/config-specific tsconfigs;
- framework and framework version;
- lint configuration, especially type-aware `typescript-eslint` rules;
- package/library build targets and declaration-output requirements;
- public API compatibility constraints;
- generated code and third-party type boundaries.

Do not recommend syntax, compiler options, or standard-library types unsupported by
the installed toolchain.

Treat repository policy as distinct from TypeScript language semantics. A team may
intentionally choose stricter or looser settings for migration, generated code,
library compatibility, or build performance.

## 2. Strictness is a configuration decision

Prefer `strict: true` for new TypeScript codebases and for projects that can adopt it
without uncontrolled breakage. Do not silently change compiler options during a file
review.

Remember:

- `strict` enables a family of strict type-checking options;
- projects may override individual strict-family options;
- `noUncheckedIndexedAccess` is a separate option and adds `undefined` to undeclared
  indexed access;
- other useful options such as `exactOptionalPropertyTypes`, `noImplicitOverride`, or
  `useUnknownInCatchVariables` have their own compatibility consequences.

When proposing a new compiler flag, inspect the current error surface or run the
project's type check when execution tools are available. Present migration impact
separately from local code findings.

Do not claim that a project is "strict" merely because a checklist of flags appears in
this skill. The active tsconfig is the source of truth.

## 3. Model the runtime domain first

Start with the values that can actually exist and the invariants the program must
preserve.

Ask of a type:

1. What values arrive at runtime?
2. Which states are valid?
3. Which states must be impossible or rejected?
4. Where does trust enter the program?
5. Which contract is public and therefore compatibility-sensitive?

Choose the simplest representation that answers those questions.

Do not reach for generics, conditional types, mapped types, overloads, branded types,
or recursive types until they solve a concrete relationship or invariant.

## 4. `any`, `unknown`, and trust boundaries

### `any`

Treat `any` as an escape hatch because it disables useful checking, but do not label
every occurrence a failure.

An `any` may be justified at narrowly contained compatibility boundaries such as:

- inaccurate or missing third-party declarations;
- generated/interoperability code;
- incremental JavaScript-to-TypeScript migration;
- deliberately unconstrained library escape hatches.

Before accepting it:

- determine why the value cannot be typed more precisely;
- contain the `any` to the smallest boundary;
- avoid allowing it to propagate through internal application code;
- document the reason if it is non-obvious and durable.

Do not replace `any` mechanically with `unknown`. `unknown` is safer specifically
because consumers must narrow it, which may expose additional code that needs a real
runtime check.

### `unknown`

Use `unknown` when a value's type is genuinely not known at the current boundary.
Narrow it before domain operations.

Do not assume that a TypeScript annotation validates runtime data.

For untrusted JSON, storage, message events, URL-derived values, external API payloads,
or other runtime boundaries, use one of:

- explicit runtime checks;
- an existing schema/validator already used by the project;
- protocol/client-generated types when the generation pipeline provides runtime
  guarantees appropriate to the boundary.

Do not add Zod, Valibot, io-ts, ArkType, or another validation dependency during a
typing cleanup unless runtime validation is actually required and the dependency
choice is authorized.

`response.json() as User` changes the compiler's belief. It does not prove the payload
is a `User`.

## 5. Narrowing before assertion

Prefer control-flow narrowing that follows runtime evidence:

- `typeof`;
- `instanceof`;
- property/existence checks;
- equality/discriminant checks;
- truthiness only when falsy values are not legitimate domain values;
- well-implemented type predicates;
- assertion functions when failure should terminate/control flow.

Use type assertions when runtime knowledge exists that TypeScript cannot represent or
infer conveniently. Keep the assertion close to that evidence.

Do not use `as` to silence a compiler error whose cause has not been understood.
Avoid double assertions such as `value as unknown as T` unless implementing a proven
boundary that cannot be expressed another way.

Use `satisfies` when the requirement is to check that an expression is assignable to
a target type while retaining useful inference for the expression. It is not a
runtime validator and is not a universal replacement for `as`.

## 6. Object shapes: `interface` and `type`

Do not enforce "interface for objects, type for unions" as an absolute law.

Both aliases and interfaces can model object shapes. Choose according to semantics and
project conventions:

- `interface` is useful for extendable object contracts and declaration merging;
- `type` is necessary for unions, intersections, tuples, primitives, mapped/conditional
  constructions, and many derived types;
- established repository/public-library convention may be more valuable than changing
  equivalent declarations.

Do not churn a codebase by converting `type` to `interface` or the reverse without a
real contract or tooling benefit.

## 7. Discriminated unions and impossible states

Use a discriminated union when alternatives have meaningfully different valid data.
A discriminant such as `status`, `kind`, or `type` should correspond to a real domain
state, not be added solely to demonstrate the pattern.

Example:

```ts
type LoadState<T> =
  | { status: 'idle' }
  | { status: 'loading' }
  | { status: 'success'; data: T }
  | { status: 'error'; error: Error };
```

This is preferable to several unrelated booleans when those booleans permit impossible
combinations.

For closed unions where every case must be handled, use an exhaustive check compatible
with the installed TypeScript version and repository style. Do not add exhaustiveness
ceremony to an intentionally open contract.

## 8. Generics should encode relationships

Use a generic when callers and return values share a type relationship that cannot be
expressed more clearly with concrete types.

Good generic design usually has:

- a meaningful relationship between at least two positions;
- the narrowest constraint required by the implementation;
- inference that saves callers from redundant annotations.

Avoid:

- a type parameter used only once when a concrete or union type is clearer;
- unconstrained `<T>` followed by unsafe casts inside the implementation;
- deep conditional/mapped types that save a few duplicated properties but make error
  messages and maintenance much worse;
- generic wrappers that erase useful domain names.

Overloads are not inherently inferior to generics. Use overloads when the public API
has a small number of distinct call shapes that are clearer as separate signatures.
Use unions or generics when they express the relationship more accurately.

## 9. Derived and utility types

Use `Pick`, `Omit`, `Partial`, `Required`, `Readonly`, `Record`, `ReturnType`,
`Parameters`, `Awaited`, and related utilities when they express a real derivation.

Do not prefer a utility type merely to avoid writing a named domain type. A separate
request DTO, persisted model, and UI form model may deserve separate definitions even
when their properties currently overlap.

Be especially cautious with `Partial<T>` for update payloads. It means every property
of `T` is optional, which may be broader than the actual API contract.

## 10. Mutability and literals

Use `readonly` when mutation is not part of the contract, especially on public data
structures where immutability communicates an invariant.

Use `as const` when literal narrowing and/or readonly literal structure is desired.
Do not scatter it over values whose widened type is intentional.

Distinguish compile-time readonly from runtime immutability. `readonly` does not freeze
an object at runtime.

## 11. Enums and literal alternatives

Do not ban `enum` categorically. TypeScript enums are runtime constructs and can be
appropriate when the project or public API intentionally needs that runtime object or
interop contract.

Consider literal unions or `as const` objects when:

- only type-level closed values are needed;
- JavaScript-friendly runtime objects fit better;
- bundle/runtime emission matters;
- the repository already uses that convention.

Before replacing an existing enum, inspect runtime uses, serialization, generated
clients, template/framework integration, and public API compatibility. An enum cleanup
can be a breaking change.

## 12. Public API design

Use explicit annotations where they protect a contract or keep declaration output
stable. Do not require return annotations on every implementation function merely for
visual explicitness; inference is often correct and easier to maintain.

Be deliberate for:

- exported library functions;
- callbacks whose contextual type is important;
- overloaded APIs;
- declarations whose inferred type leaks internal implementation details;
- boundaries where a wider public type is intentional.

For application-local functions, judge explicit annotations by whether they clarify or
stabilize the contract.

## 13. Framework-aware typing

Respect the installed framework's own typing model.

For Angular, React, Vue, Node, or other frameworks:

- inspect framework and TypeScript versions before recommending newer syntax/APIs;
- use generated/router/form/component types that already exist in the project;
- avoid duplicating types supplied by a well-typed library;
- distinguish framework/runtime validation from compile-time TypeScript declarations.

Do not make a framework-specific type migration under the generic label "TypeScript
Pro" unless the user asked for that migration.

## 14. Review workflow

### Step 1: compile/config evidence

Read the relevant tsconfig and lint setup. If the user supplies a compiler diagnostic,
preserve its code and message.

### Step 2: find unsafe boundaries

Search the requested scope for relevant patterns such as:

```text
any
as any
as unknown as
@ts-ignore
@ts-expect-error
non-null assertions
broad index signatures
unchecked parsed/external data
```

A textual match is not automatically a defect. Inspect each use in context.

For `@ts-expect-error`, distinguish a deliberate, self-checking compatibility/test
case from stale suppression. `@ts-ignore` deserves stronger scrutiny because it does
not assert that an error must continue to exist.

### Step 3: classify findings

Use:

- **Type defect**: the declared type permits or assumes behavior inconsistent with the
  runtime domain.
- **Unsafe boundary**: external/untrusted data is trusted without sufficient runtime
  evidence.
- **Suppression debt**: a compiler/lint suppression hides an issue without adequate
  justification.
- **API design risk**: exported typing is unstable, misleading, or unnecessarily
  permissive.
- **Maintainability**: type machinery is harder to understand than the invariant it
  protects.
- **Preference**: an equivalent stylistic alternative with no demonstrated safety or
  maintenance benefit.

Do not refactor preference-only findings unless the user explicitly requests style
standardization.

### Step 4: make the smallest correction

Prefer fixes in this order when applicable:

1. correct the source/runtime model;
2. narrow from runtime evidence;
3. improve the domain type;
4. derive a related type;
5. use a justified assertion at a trustworthy boundary;
6. retain a narrowly scoped escape hatch when the type system cannot model the
   integration reasonably.

Do not redesign unrelated domain models while fixing one type error.

### Step 5: validate

When execution is available, use the project's existing scripts/configuration rather
than inventing a compile command. Relevant checks may include:

- project typecheck/`tsc --noEmit` where compatible with the build setup;
- framework compiler/build;
- configured ESLint/type-aware lint;
- focused tests for runtime behavior affected by added validation or narrowing;
- declaration build/API extraction for libraries when applicable.

A clean typecheck proves static consistency under that configuration. It does not prove
that untrusted runtime data conforms to compile-time types.

## 15. Output

For review requests, prioritize findings over tutorial prose:

```text
File: <path:line>
Classification: Type defect | Unsafe boundary | Suppression debt | API design risk | Maintainability
Evidence: <compiler/runtime/type relationship>
Impact: <what can go wrong or become misleading>
Recommendation: <smallest safe correction>
Validation: <actual check, if run>
```

Include before/after code only where it makes the correction clearer. Do not duplicate
the entire file when a focused excerpt is enough.

For implementation requests, edit the code and report:

- files changed;
- material type-contract changes;
- compiler/lint/test commands actually run and their outcomes;
- remaining suppressions or unsafe boundaries;
- any runtime validation that remains the caller's/service's responsibility.

## 16. Completion check

Before finishing, verify that:

- recommendations match the installed TypeScript/framework versions;
- compiler-option claims match the active tsconfig;
- no runtime safety claim relies only on a type assertion;
- `any` findings were evaluated rather than mechanically replaced;
- generics and utility types encode useful relationships rather than type cleverness;
- public API compatibility was considered before changing exported types or enums;
- preference-only changes are separated from correctness findings;
- runtime validation is present where untrusted data actually requires it;
- validation results are reported accurately.

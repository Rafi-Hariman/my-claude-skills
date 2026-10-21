---
name: service-pattern-handoff
description: "Scan a frontend codebase for its HTTP/service usage patterns and write ONE markdown doc (docs/project-docs/service-pattern.md) so you or a repo-less agent can write new API calls consistent with existing code. Use when the user asks for service/API pattern documentation, an api-usage doc, or a handoff of how a project makes HTTP requests."
tools: Read, Glob, Grep, Bash, Write
---

# Service Pattern Handoff — document how THIS project makes HTTP calls

You are producing ONE file, `docs/project-docs/service-pattern.md`, that lets any agent
(including one with no repo access) write a NEW API call that reads native to this project:
which service to call, which verb is hidden behind which method, how the URL/params/body are
wired, how loading and notifications and unsubscription are handled around the call.

This is an agent skill, not a CLI — every stage needs judgment (which file is representative,
when to stop reading). Never brute-force full-read the repo.

## Preconditions

1. Determine the project root (the dir with `package.json`). Note the framework + exact
   version from `package.json`. Never guess the version.
2. Locate the HTTP layer's entry signals (Stage 1). If NONE of the signals match, say so and
   STOP — there is nothing to document.
3. Output target is `docs/project-docs/frontend-<repo>.md`, where `<repo>` is the `name` field
   from `package.json`, sanitized for a filename (lowercase, spaces → `-`). If `docs/project-docs/`
   does not exist in the scanned project, ASK the user before creating it — do not silently
   invent a doc tree.

## Pipeline

### Stage 1 — Find candidate files (pay cheap cost first, read bodies later)

Run these signals as `grep`/`Glob` and merge; stop as soon as the set stops growing:

| # | Signal | What it catches |
|---|--------|-----------------|
| 1 | `grep -rlE "HttpClient|HttpService|httpSvc\.|this\.http\." --include=*.ts <src>` | any file touching an HTTP client |
| 2 | `*.service.ts` basename, in a `data-access/` / `services/` / `service*/` dir — **observe the actual dir name, don't assume it** | the service classes |
| 3 | `grep -rlE "['\"]/(v[0-9]+|public|private)/" --include=*.ts <src>` | files that build API paths without a named client |
| 4 | `grep -rlE "@Inject\(ENV\)|apiUrl|BASE_URL" --include=*.ts <src>` | base-URL ownership |

Report the per-signal counts to the user. Everything in the union enters Stage 2, EXCEPT:
`*.spec.ts`, `*.test.ts`, e2e/cypress, `node_modules` (except `.d.ts` for the HTTP client —
see Stage 2). Record what you scoped out.

> Quoting note: `--include=*.ts` in zsh needs quoting (`--include="*.ts"`) or the glob is
> expanded by the shell before grep sees it.

### Stage 2 — Identify service classes + the real verb behind each method

For each candidate service file, read its class and methods (these are small; full-read is
fine — but STOP full-reading once you've seen N≥3 services with an identical shape; for the
rest, read only their `getUrl`/method signatures to confirm sameness).

For every public method, resolve the real HTTP verb from the **body**, never from the name:

1. **Hop 1 — project code.** Read the method body. `listAll(...)` → `return this.httpSvc.get(...)`.
   The verb comes from the callee it returns/awaits: `get`/`post`/`put`/`delete`/`deleteWithBody`/
   `pagedCollection`. `listAll` may be a `post` in some project — the body is authoritative.
   Also capture: how the URL is resolved (switch-on-enum → literal path; template string;
   `+ (pathParams ?? '')`), and the parameter signature.
2. **Hop 2 — external symbol.** If the callee is external (e.g. `this.httpSvc` is `HttpService`
   from a dependency), resolve its REAL signature from the installed `.d.ts`
   (`node_modules/<pkg>/.../http.service.d.ts`) and copy the signature verbatim. Example that
   matters: `pagedCollection(url, page, callback, params): void` is NOT an Observable — it
   takes a callback and cannot be `subscribe`d. If Hop 2 fails (no `.d.ts` readable), put the
   method under **Unresolved** in the output. Never infer a verb/signature from a name.
3. **Classify each service by shape** (use in the "Service Shapes" section):
   - **A** enum `ServiceType` + `getUrl()` switch + thin verb wrappers (`insert`→post, etc.)
   - **B** `httpSvc.get(url)` written directly per method (no enum layer)
   - **C** not an HTTP service (zero client calls — e.g. a `data-sharing`/`callback` service).
     Exclude type-C from the API table but state them in one line with the reason — never
     silently drop them.
4. **Detect divergence.** If services disagree (some A, some B), report BOTH shapes
   separately. Never force uniformity. Note which shape is dominant (count the services).

### Stage 3 — Find real usage sites and pick representative examples

For each discovered method, `grep -rn "\.<method>\(" --include="*.ts"` to find call sites. Then:

1. **Cap 3 call sites per method**, cap total method→example pairs (aim the doc at a portable
   attachment, ~≤20 KB — quality over exhaustiveness).
2. **Priority for the pick:** (a) the call showing the fullest handling — `finalize` +
   `takeUntil` + notification; (b) a call from a DIFFERENT feature dir than the first pick;
   (c) otherwise skip. Never dump every call site.
3. Read ONLY the enclosing function region (Grep → line → `Read` with offset/limit ±25 lines).
   Never full-read a 2000-line component for one call.
4. **Identical repeated pattern** → ONE code block + one line naming the other components that
   repeat it ("same pattern in X, Y, Z"). Never repeat near-identical blocks.
5. Extract per example: the import line, the constructor injection, the call itself, the
   success handling (`.subscribe`), loading flags around it, and unsubscription if present.

### Stage 4 — Infer the response shape from evidence, never invent

Collect ONLY properties that call sites actually READ: `res['data']`, `response.message`,
`result.page`, `data.count`, etc. Rules:

- Property seen at **≥2 independent call sites** → Fact, cite `file:line`.
- Property seen **once** → mark "observed once, unconfirmed".
- Do NOT emit a hand-written `interface ApiResponse<T>` as if it were real unless the code
  actually declares it. If the project defines a shared response model/interface, cite it;
  otherwise present the observed-properties list as the evidence it is.

### Stage 5 — Write `docs/project-docs/service-pattern.md`

First line: `<!-- Generated by Service Pattern Handoff (agent skill) — regenerate with /service-pattern-handoff. -->`

Then this skeleton, in this order. Every section is REQUIRED — if a section has no content,
write `_(not detected — <what to do instead>)_` explicitly; never silently drop it.

```markdown
> Generated <ISO timestamp> by the service-pattern-handoff agent skill.
> Project: <name> — framework <name> <version>. Scanned <N> files (<M> services).
> Signals used: <which of Stage-1 signals matched>. Scoped out: <spec/tests/e2e/node_modules>.
> If the codebase changed since then, re-run the skill.

# Service Pattern — <Project> (<framework> <version>)

## How to use this file
2–3 lines: this is how THIS project calls APIs; to add a new call, copy the shape in
"Usage Patterns", pick the service + method + ServiceType from "Quick Reference".

## Service Architecture
Table: service | file location | role | shape (A/B/C). Cite real paths.

## Service Shapes
The detected shapes (A enum+getUrl, B direct, C non-HTTP) with a short code example each and
which services belong to which. If only one shape exists, say so plainly. If divergent, both
are shown and NOT forced uniform.

## Universal Service Methods
Only methods that appear in ≥2 services, grouped GET vs POST/PUT/DELETE, with signature (from
Stage 2 hop 2 where external). Note which are Observable vs callback-style.

## Usage Patterns
Per scenario (list/dropdown, create, update, delete one, delete many, paginated, detail,
search-with-params, with-path-param, count): a real snippet + `path:line` provenance. Include
import, constructor injection, call, subscribe, success handling. Only scenarios that were
actually found; skip the rest (but keep the section honest).

## Standard Response Structure
Observed properties only (from Stage 4), with `file:line` for each. Shared model/interface if
one truly exists. No invented interface.

## Loading State Management
Before/after patterns: the loading flag name(s), `finalize(...)` usage, success AND error
paths both closing it. Real names from the code, not generic.

## Notification Types
The enum/constants actually used + a usage line. Cite where defined if findable.

## Subscription Management
The unsubscribe pattern(s): `takeUntil(destroy$)` + `ngOnDestroy`, or `unsubscribe()`, or
absent. Show the real code, and say honestly if some sites don't clean up.

## Direct HTTP Calls
Any component/file that calls HTTP WITHOUT a service layer. If none, write
`_(none detected — all HTTP goes through the service layer)`.

## Quick Reference
Table: operation | method | example ServiceType/url constant.

## Unresolved
Methods whose real verb/signature could not be proven from code or `.d.ts`. Empty → state it.
```

### Compactness + honesty rules

- Target ≤ ~25 KB. Dedup near-identical blocks. Samples teach the pattern — quote ≤ 8 lines.
- Cite a real `path:line` for every important claim, so a reader can verify.
- The OUTPUT doc language: use the project's own language for strings/section comments it
  contains; section headers may stay English. Don't translate real code.

## Anti-patterns

- **Never** infer a verb from a method name (`insert` might not be POST). Read the body.
- **Never** invent an interface, endpoint, or signature the code doesn't show.
- **Never** force uniformity across divergent services — report both shapes.
- **Never** silently drop a section or a type-C service — mark it explicitly.
- **Never** point to a file as the source of a fact without having READ it this session.
- **Never** full-read a large component for a single call site — read the ±25-line region.

## After writing

Report: the output path, file size, per-signal scan counts, which shapes were found (and which
is dominant), which sections had no content, and any discrepancy between what the code actually
does vs. any existing api-usage doc in the repo (flag staleness rather than trusting it).

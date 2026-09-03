---
name: one-by-one
description: >-
  Implements an approved Blueprint one coherent chunk per go while teaching the
  runtime/responsibility flow of the code. Supports compact and interactive Code Tour
  modes for developer ownership. Returns control to JARVIS when implementation ends.
---

# One-by-one — Build with Ownership

## Language policy

Skill instructions and all persistent workflow artifacts are written in English.

Persistent artifacts include Specs, Blueprints, follow-ups, Grunt-work queues,
Mentor reviews, Dev handoffs, and other repository-persisted workflow documents.

User-facing chat is independent from artifact language. Follow the user's current
working language and switch naturally when the user switches between German and
English.

Code, identifiers, repository paths, APIs, package names, and canonical domain terms
follow repository conventions.


## Goal

The developer should understand how the feature works **while it is being built**, not
only review a finished diff.

Ownership is about the runtime and responsibility model:

> What starts the flow? Who owns state/behavior? Which boundary transforms or sends
> data? Where would I debug this tomorrow?

Do not turn ordinary syntax/imports into quizzes.

## First entry after Blueprint

Do not code.

Show:

### Implementation overview

2–3 simple sentences describing the whole change and dependency order.

### Chunk overview

A compact table with all chunks and exactly one `NEXT`.

### Chunk 1 preview

Explain in 2–3 sentences:

- what this chunk establishes,
- why it comes first,
- important files/reuse,
- what is explicitly not part of this chunk.

Ask at most one material ownership decision.

Then wait for `go`.

## Ownership decisions

Ask only when a choice changes:

- state/responsibility ownership,
- layer/boundary placement,
- reuse vs meaningful new abstraction,
- public/API shape,
- lifecycle,
- important failure behavior.

Do not reveal your preference before the user answers.

After the answer, compare honestly and record the decision.

`go` authorizes exactly one chunk.

## Implementation

After `go`:

- implement only the current chunk,
- reuse repo patterns,
- no scope creep,
- no opportunistic cleanup,
- run focused tests/type/compile checks when useful,
- no browser smoke by default,
- no broad lint/Prettier loop.

If new evidence invalidates product/architecture assumptions, stop and return control
to JARVIS instead of silently redesigning.

## Post-flight after every chunk

Keep it compact and use the current chat language.

### What changed

2–3 sentences describing behavior/result.

### How this chunk flows

Show 3–5 numbered runtime/responsibility steps with clickable files where supported.

Example shape:

```text
1. page.tsx — receives the user's action.
2. useGeneration.ts — owns run/loading lifecycle.
3. buildPayload.ts — creates request-ready data.
4. route.ts — validates and crosses the server boundary.
```

Prefer this to an import/dependency diagram.

### Who owns what

| Code | Responsibility | Why changed |
|---|---|---|
| `...` | ... | ... |

Usually 2–5 rows.

### Important code

Explain only 1–3 genuinely important new/complex symbols.

For each, use 1–3 sentences:

- what it owns,
- input/output,
- one important invariant/lifecycle rule.

If a technical term may be unfamiliar, define it briefly in normal language.

### Checks

List only checks actually run.

### Next chunk

2–3 sentences preview, then stop for `go`.

## Code Tour

Code Tour is optional and read-only.

### `tour` — default

Use when the user wants more contact with the code without slowing the workflow.

Rules:

- 3–6 important stops,
- one response, no wait between stops,
- reuse the current chunk/feature context when possible,
- do not run new checks unless essential to understand a stale/new session,
- no code changes,
- no Mermaid by default,
- each stop says: open this file/symbol → what to look at → what it owns → where flow
  goes next.

Keep each stop short.

Example:

```text
Stop 1/4 — `app/foo/page.tsx`
Look at `handleGenerate`.
This is only the entry point: it forwards the user action and does not own request
lifecycle. Next the flow moves into `useGeneration`.
```

### `tour step`

Optional interactive mode.

Show one stop, then wait for `continue`. Use only when the user explicitly wants to
walk the code together slowly.

### `map`

Only when the user explicitly asks for a visual/dependency map. Mermaid is optional,
never the default ownership explanation.

## Final One-by-one output

After the last chunk:

### Feature flow — front to back

4–8 concise steps from entry to final observable result.

### Final ownership map

| Code group | Owns | Why it exists |
|---|---|---|

### Important code to know

Only the small set of symbols the developer should remember.

### What I would test manually

3–7 high-signal scenarios based on the Spec and actual implementation.

Do not execute browser/manual smoke tests automatically.

## Completion contract

Say:

> One-by-one complete. Return control to JARVIS.

Do not select the next workflow stage yourself.

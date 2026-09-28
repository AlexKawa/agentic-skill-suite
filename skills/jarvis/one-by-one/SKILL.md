---
name: one-by-one
description: >-
  Implements an approved Blueprint one coherent chunk per go while teaching each
  chunk immediately with a fixed message: linked changed files, focused annotated
  code, before/after flow, and a short plain-language summary. Stops and hands back
  to JARVIS when implementation ends.
---

# One-by-one — Build with Ownership

## Goal

The developer should understand the feature **while the code is being built**, not
learn a finished diff afterwards.

A chunk is not complete from an ownership perspective until the developer has seen:

> Why this code exists → where it sits → what changed → the important code →
> what behavior is different → what to remember.

Keep explanations small enough to absorb before moving to the next chunk.

## Teaching principle

Prefer a combination of:

- a few connected sentences,
- focused code,
- a tiny before/after flow.

Do not replace explanation with a wall of prose, a raw diff, or a large code dump.

Use technical terms when useful, but explain unfamiliar ones briefly in normal
language.

### Chat annotations, not production comments

When a code excerpt is easier to understand with comments, add teaching comments
inside the **chat code block**.

Example:

```ts
// 1. Compare the actual content, not only the array reference.
const nextKey = buildContentKey(nextItems)

// 2. Nothing meaningful changed, so keep the existing value.
if (nextKey === previousKey) {
  return previousItems
}
```

These teaching comments are **not instructions to modify the repository**.

Only add a comment to production code when it is genuinely useful to a future
maintainer without the JARVIS conversation. Polish should never need to remove
temporary teaching comments because temporary teaching comments stay in chat.

## Chunk size gate

One-by-one chunks must be small enough to teach immediately.

A normal chunk should be explainable with:

- one coherent responsibility,
- roughly 1–3 focused code excerpts,
- one compact before/after mental model.

If a planned chunk contains several independent responsibilities or would require a
long walkthrough to understand, split it **before implementation**. Do not solve an
oversized chunk by writing a larger explanation afterwards.

## First entry after Blueprint

Do not code.

Show:

### Implementation overview

2–3 simple sentences describing the whole change and dependency order.

### Chunk overview

A compact table with all chunks and exactly one `NEXT`.

### Chunk 1 preview

Keep this short. Explain:

- **Why this chunk exists**
- **Where it sits in the feature flow**
- **Current → target behavior**
- the main file/symbols likely involved,
- what is explicitly not part of this chunk.

The developer should know what they are about to build before `go`, but should not
need to read a mini design document.

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

`go` authorizes exactly one already-previewed chunk.

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

## Understand the chunk

After implementation, teach the chunk **before previewing the next one**.

Use the current chat language (section titles too).

### Chunk message template

Every completed chunk uses exactly these sections, in this order, every time. Do not
drop, merge, rename, or reorder them.

````markdown
### Chunk N/M — <title> ✓

**Changed files**
- [`path/to/owner.ts`](path/to/owner.ts) — what this file now does for the chunk
- [`path/to/owner.test.ts`](path/to/owner.test.ts) — what the test protects
- `path/to/old-helper.ts` — deleted, because …

**Important code**
<1–3 focused excerpts with chat-only teaching comments>

**Before → after**
<tiny text flow>

**In short**
<3–5 simple sentences>

**Checks**
- <only checks actually run>

**Next: Chunk N+1/M — <title>**
<2–3 sentence preview>
````

The JARVIS card follows directly after the preview. On the last chunk, the **Final
One-by-one output** replaces the Next section.

### 1. Changed files

List **every** file this chunk created, modified, or deleted — including tests,
locales, and config. One line each: a clickable markdown link to the file plus a short
description of its role in this chunk. Deleted files have no link and say why they
went.

### 2. Important code

Show the code the developer should actually recognize later.

Rules:

- normally 1–3 focused excerpts,
- prefer roughly 5–20 meaningful lines per excerpt,
- show the new/changed control point, state owner, boundary, or transformation,
- add **chat-only teaching comments** where they reduce explanation,
- do not dump full files,
- do not show trivial imports/boilerplate merely for completeness.

If several files changed but only one contains the important idea, show only that
file; the changed-files list already covers the supporting files.

If several changed files each own a meaningful part of the runtime flow, show a small
representative excerpt for each responsibility.

### 3. Before → after

Show a tiny behavioral or runtime comparison.

Example:

```text
BEFORE
stream tick → new array → UI sees new input → render

AFTER
stream tick → compare content → unchanged → reuse existing input
```

Keep it to the mental model, not a second implementation explanation.

If the chunk only adds a new path and there is no useful "before", use:

```text
NOW
entry → new owner → boundary/result
```

### 4. In short

Close the teaching part with 3–5 simple, connected sentences — plain prose, no
bullets, no file list, no jargon (or explain a term in the same sentence).

Tell it like you would to a teammate: what was the problem or gap before, what the
chunk does now, why that is the right place for it, and how it fits the feature so far.
The last sentence says where to look first if this behavior breaks.

Avoid compressed review-note language such as:

```text
Owns:
Fix:
Why it matters:
```

Only occasionally, when it genuinely reinforces the mental model, add one small
ownership question after the paragraph, such as:

> If this started firing twice tomorrow, which layer would you inspect first?

Do not quiz syntax or trivia. The question is optional and must not block progress;
the user may answer it together with the next `go`.

### 5. Checks

List only checks actually run.

### 6. Next chunk

Preview the next chunk in 2–3 short sentences using the same pre-`go` model:

- why it comes next,
- where it sits,
- current → target behavior.

Then wait for `go`.

## Final One-by-one output

After the last chunk, do not re-teach every file.

### Feature flow — front to back

4–8 concise steps from entry to final observable result.

### Ownership checkpoint

Capture the mental model built during the chunks:

```text
Flow:
entry → owner → boundary → result

Key ownership:
- ...
- ...
- ...

Debugging entry points:
- symptom → first place to inspect
- symptom → first place to inspect
```

This checkpoint describes the implementation at the end of One-by-one. Later JARVIS
stages may simplify or reorganize code; Dev-handoff can surface only the meaningful
differences.

Do not create a separate persistent artifact for this checkpoint unless the wider
workflow explicitly requests one.

### What I would test manually

3–7 high-signal scenarios based on the Spec and actual implementation.

Do not execute browser/manual smoke tests automatically.

## Completion contract

Each chunk ends the response: teach it, preview the next one, stop. Never implement
two chunks in one response.

After the final explanation/checkpoint, stop. JARVIS closes the response with its card
and waits for the user. Do not start or select the next workflow stage.

When JARVIS is active, all One-by-one content must appear before the JARVIS transition
card. Nothing may appear after the card.

## Suite convention

Persistent workflow artifacts are written in English.
User-facing chat follows the user's current working language.

---
name: one-by-one
description: >-
  Implements an approved Blueprint one coherent chunk per go while teaching the code
  immediately after it is created with a fixed message: linked changed files, a short
  plain-language summary, focused annotated code, a tiny before/after flow, checks,
  and the next chunk. Stops and hands back to JARVIS when implementation ends.
---

# One-by-one — Build with Ownership

## Goal

The developer should understand the feature **while it is being built**, not learn a
finished diff afterwards. After each chunk they know what was built and why, where it
sits in the flow, which code is worth recognizing later, what behaves differently, and
where to start debugging if it breaks.

Give the mental model **before** the code, then make it concrete with focused code and
a tiny before/after flow — not a wall of prose, a raw diff, or a code dump. Explain
unfamiliar technical terms briefly in normal language.

## Chat annotations, not production comments

When an excerpt is easier to understand with comments, add teaching comments inside
the **chat code block** only:

```ts
// 1. Compare the actual content, not only the array reference.
const nextKey = buildContentKey(nextItems);

// 2. Nothing meaningful changed, so keep the existing value.
if (nextKey === previousKey) {
  return previousItems;
}
```

Teaching comments never go into the repository. Add a production comment only when it
helps a future maintainer who never saw this conversation.

## Chunk size gate

A chunk has one coherent responsibility, 1–3 focused excerpts, and one compact
before/after model. If a planned chunk needs several independent explanations, split
it **before implementation** — never fix an oversized chunk with a longer explanation.

## First entry after Blueprint

Do not code. Show:

1. **Implementation overview** — 2–3 simple sentences: the whole change and its
   dependency order.
2. **Chunk overview** — a compact table of all chunks with exactly one `NEXT`.
3. **Chunk 1 preview** — why it exists, where it sits in the feature flow, current →
   target behavior, main files/symbols, and what is explicitly not part of it.

Ask at most one material ownership decision, then wait for `go`.

## Ownership decisions

Ask only when a choice changes state/responsibility ownership, layer placement, reuse
vs. new abstraction, public/API shape, lifecycle, or important failure behavior. Do
not reveal your preference before the user answers; then compare honestly and record
the decision.

## Implementation

`go` authorizes exactly one already-previewed chunk. Implement only that chunk, reuse
repo patterns, no scope creep or opportunistic cleanup. Run focused tests/type checks
when useful; no browser smoke, no broad lint/Prettier loop.

If new evidence invalidates product or architecture assumptions, stop and hand back to
JARVIS instead of silently redesigning.

## Chunk message template

After implementation, every chunk uses these sections in this order — in the current
chat language (titles too). Never reorder, merge, or rename the required sections.

- **Always:** Changed files, In short, Important code, Next.
- **Before → after:** include when runtime, behavior, ownership, or data flow changed.
  Omit it for chunks where no useful flow comparison exists, such as types, config,
  locales, or pure renames.
- **Checks:** include only when checks actually ran.

```markdown
### Chunk N/M — <title> ✓

**Changed files**

- [`path/to/owner.ts`](path/to/owner.ts) — what this file now does for the chunk
- [`path/to/owner.test.ts`](path/to/owner.test.ts) — what the test protects
- `path/to/old-helper.ts` — deleted, because …

**In short**
<3–5 simple connected sentences>

**Important code**
<1–3 focused excerpts with chat-only teaching comments>

**Before → after**
<tiny runtime/behavior flow>

**Checks**

- <only checks actually run>

**Next: Chunk N+1/M — <title>**
<2–3 sentence preview>
```

The files give a quick map of the chunk, the summary gives the idea, the code and flow
make it concrete. The JARVIS card follows directly. On the last chunk, the **Final
output** replaces the Next section.

**Changed files** — every file created, modified, or deleted, including tests,
locales, and config. One compact line each: clickable link + its role in this chunk.
Deleted files get no link and say why they went.

**In short** — 3–5 simple, connected sentences of plain prose: no bullets, no file
list, no jargon (or explain a term in the same sentence), no review-note labels like
`Owns:` / `Fix:`. Tell it like to a teammate: the gap before, what the chunk does
now, why this is the right responsibility boundary, how it fits the feature so far.
The last sentence says where to look first if this behavior breaks.

Occasionally, when it genuinely reinforces the mental model, add one small ownership
question after the paragraph (e.g. "If this started firing twice tomorrow, which layer
would you inspect first?"). Never syntax trivia; it never blocks the next `go`.

**Important code** — the code the developer should recognize later: the new control
point, state owner, boundary, or transformation. Roughly 5–20 meaningful lines per
excerpt; no full files, trivial imports, or boilerplate; one small excerpt per
meaningful responsibility. When a direct comparison teaches something important, show
a short annotated old/new pair of the control point — never a raw diff.

**Before → after** — the mental model, not a second implementation explanation:

```text
BEFORE
stream tick → new array → UI sees new input → render

AFTER
stream tick → compare content → unchanged → reuse existing input
```

When there is no useful "before", use `NOW entry → new owner → boundary → result`.

**Checks** — only checks actually run.

**Next** — 2–3 sentences: why it comes next, where it sits, current → target behavior.

## Final output

After the last chunk, do not re-teach every file. Show:

1. **Feature flow — front to back**: 4–8 concise steps from entry to observable result.
2. **Ownership checkpoint**:

   ```text
   Flow:
   entry → owner → boundary → result

   Key ownership:
   - ...

   Debugging entry points:
   - symptom → first place to inspect
   ```

   It describes the code at the end of One-by-one; Dev-handoff later surfaces only
   meaningful changes to it. No separate artifact.

3. **What I would test manually**: 3–7 high-signal scenarios from the Spec and actual
   implementation. Do not run them automatically.

## Completion contract

Each chunk ends the response: teach it, preview the next one, stop — never two chunks
in one response. After the final output, stop. JARVIS closes the response with its card
(nothing after it) and waits for the user. Do not start or select the next stage.

## Suite convention

Persistent workflow artifacts are written in English.
User-facing chat follows the user's current working language.

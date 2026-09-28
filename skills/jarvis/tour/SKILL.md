---
name: tour
description: >-
  Read-only orientation and recap for an existing feature, working tree, or code path.
  JARVIS normally uses it once before Speculation so developer and agent share the real
  runtime/ownership context. It can also be invoked later as a standalone recap without
  changing code or workflow state.
---

# Tour — Understand Existing Code

## Goal

> What is this code doing, why does it exist, how does the flow move through it, and
> where would I look when something breaks?

Tour is a **read-only utility**, not a JARVIS pipeline stage. It makes existing code
feel less foreign and gives the agent a grounded mental model before it reasons about
changes. Use it before Speculation (default in a new JARVIS task), on a working tree or
inherited change, when context was lost mid-task, or as a whole-feature recap.

## Scope

Infer the smallest useful scope from the request, working tree, and nearby code — the
requested feature, the current diff, a named file/code path, or the final
implementation. If the scope is obvious, do not ask the user to restate it.

For greenfield work, tour the nearest relevant patterns, boundaries, and reuse points.
If there is genuinely nothing useful to tour, say so briefly instead of manufacturing
an architecture walk-through. After implementation, current code beats earlier plans.

## Read-only rule

No code changes, patches, formatters, product decisions, Spec, or implementation plan;
no stage advancement; no turning observations into fixes. Run checks only when
essential to understand stale or ambiguous code.

## Teaching style

Same principle as One-by-one: mental model first → focused code → flow/ownership →
what to remember. Use short connected explanation, tiny text flows, and concrete
debugging entry points. No file-by-file changelogs, code dumps, terse `Owns / Fix`
fragments, or Mermaid by default.

Annotated snippets are welcome when seeing code beats describing it. Teaching comments
live in the **chat snippet** only:

```ts
// This is the entry point: it forwards the action.
// It does not own the request lifecycle.
return runGeneration(input);
```

## Pre-Speculation mode (JARVIS preflight)

Compact orientation, roughly a 60–120 second read — not a second planning phase.
Teach **current reality** only; never invent the intended behavior or the solution.

1. **Current mental model** — 2–4 connected sentences: what the area does today, where
   the requested change appears to touch it, the front-to-back shape.
2. **Current flow** — 3–7 steps, e.g.
   `entry → state/behavior owner → transform/boundary → observable result`.
3. **Code worth seeing** — 1–3 focused annotated excerpts, only when they materially
   help; say first what to look for.
4. **Responsibility map** — `file/symbol → responsibility`, real boundaries only.
5. **Carry into Speculation** — current behavior not to assume away, useful
   reuse/patterns, and genuine product unknowns. Do not answer those unknowns.

## Standalone mode

Start with a **60-second mental model**, then follow the runtime/responsibility flow,
not file order. For every meaningful stop: why it exists and where it sits, what it
owns and deliberately does not own, a focused excerpt when it helps, how it connects
to the next stop, one short takeaway. Group trivial imports, fixtures, and boilerplate
instead of making them stops.

For a **working tree**, separate what already existed, what the change adds, removes,
or redirects, and how the runtime model differs now — without narrating every hunk.
Without a meaningful diff, teach the system as it exists.

End with a compact cheat sheet:

```text
Feature in one sentence:
...

Flow:
A → B → C → result

Remember:
- ...

Debug first:
- symptom → file/symbol
```

For a working-tree tour, add `Biggest behavior/ownership change: ...`. In
pre-Speculation mode, **Carry into Speculation** replaces the cheat sheet.

## `tour step`

Interactive mode: one meaningful stop, then wait for `continue`. Every few stops, an
optional small ownership/debugging question may help retention; it never gates
progress.

## Completion contract

When done, stop. In a JARVIS task, JARVIS closes the response with its card (nothing
after it): after the preflight it points to Speculation; mid-workflow it repeats the
previous card unchanged. No persistent Tour artifact.

## Suite convention

Persistent workflow artifacts are written in English.
User-facing chat follows the user's current working language.

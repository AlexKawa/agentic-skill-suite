---
name: dev-handoff
description: >-
  Writes the final concise English developer handoff from actual final code and commit
  history: runtime flow, ownership, review order, verification, meaningful ownership
  changes since implementation, a presentation-style visual brief, and a copy-paste
  PR title and description in chat.
---

# Dev-handoff — Make the Branch Easy to Own

## Goal

A developer should understand the final branch in a few minutes without reading the
AI workflow history. The handoff describes the **final implementation**, protects the
mental model built during One-by-one, and ends with a ready-to-paste PR.

Artifact (always English):

```text
work/{work-id}/dev-handoff.md
```

## Sources of truth

Final code/diff, actual commits, `spec.md`, `blueprint.md`, `mentor-review.md`,
`follow-ups.md`, resolved ownership decisions, and the One-by-one ownership checkpoint
when available. Final code beats earlier plans, reviews, and mental models. Never
invent an earlier checkpoint.

## Understanding delta

Compare the final code with the One-by-one ownership checkpoint. Surface only changes
that would make the developer's earlier mental model incomplete or wrong: runtime
flow, state/responsibility ownership, an important boundary, lifecycle, a debugging
entry point, or code they were told to remember. Renames, formatting, imports, small
helper extraction, test-only changes, and cleanup with the same model do not count.

For each delta (usually 0–4), say in normal sentences: what they learned before, what
is different now, why it changed, what to remember now. If nothing changed:

> No meaningful runtime or ownership changes since the last ownership checkpoint. The
> previous mental model is still valid.

## Visual feature brief

A ready-to-paste image-generation prompt for one clean **16:9 presentation-style
technical knowledge slide** — understandable in 30–60 seconds, not UML, not a code
screenshot. Use only facts from the final code and artifacts.

Regions when relevant: problem, core solution idea, front-to-back runtime flow,
ownership (which code area owns what), key invariant / "remember this", small
debugging cheat sheet or edge case.

The prompt names the actual feature and its real problem/outcome, uses short real
file/symbol labels only where they help, and asks for strong visual hierarchy with
short labels. No invented components, no paragraphs of tiny text, no dense UML
notation, no generic flowchart.

## Document shape

```markdown
# Dev Handoff — Feature

## 60-second overview

Problem → outcome → actual solution shape.

## Runtime flow

4–8 short steps from entry to observable result.

## Ownership map

| Code group | Owns | Important invariant |
| ---------- | ---- | ------------------- |

## Understanding delta

Omit when no earlier checkpoint exists.

## Key decisions

Only decisions future maintainers need.

## Review order / key files

3–5 important code groups with why to start there.

## Commit story

Actual commits and what each establishes.

## How to verify

3–6 high-signal scenarios.

## Follow-ups / deferred

Only important open items + link to follow-ups.md.

## Out of scope

Only still-useful boundaries.

## Visual feature brief

The ready-to-paste image prompt.
```

Keep it concise; no giant file changelog.

## Chat output

After writing, give a short summary: handoff path, best first file/group to review,
the single most important ownership point, whether the mental model changed —
explaining only a meaningful delta, never the unchanged parts — and that the visual
prompt is in the handoff.

Then always end with the PR (chat only, not in `dev-handoff.md`), based on the
**actual commits** — including scope added after the original Spec:

**Title** — one plain outcome sentence in its own `text` block. No conventional-commit
prefix, ticket id, or hashtags unless the user asks.

**Description** — its own `markdown` block, no code fences inside:

```text
## Summary
2–4 bullets: what the branch changes and why.

## Test plan
- [ ] 3–6 checkbox scenarios a reviewer can actually run

## Out of scope
Only boundaries a reviewer might otherwise expect to see covered.
```

## Completion contract

When done, stop. JARVIS closes the response with its recap and card (nothing after it).

## Suite convention

Persistent workflow artifacts are written in English.
User-facing chat follows the user's current working language.

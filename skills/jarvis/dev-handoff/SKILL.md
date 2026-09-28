---
name: dev-handoff
description: >-
  Writes the final concise English developer handoff from actual final code and commit
  history, preserving runtime flow, ownership, review order, verification, meaningful
  ownership changes since implementation, and a presentation-style visual brief.
---

# Dev-handoff — Make the Branch Easy to Own

## Goal

A developer should understand the final branch in a few minutes without reading the
whole AI workflow history.

The handoff describes the **final implementation** and protects the mental model built
during One-by-one. It should also produce a ready-to-paste visual prompt that can turn
the feature into a clean presentation-style knowledge slide.

Artifact:

```text
work/{work-id}/dev-handoff.md
```

Always English.

## Sources of truth

Read:

- final code/diff,
- actual commits,
- `spec.md`,
- `blueprint.md`,
- `mentor-review.md`,
- `follow-ups.md`,
- resolved ownership decisions,
- the latest One-by-one ownership checkpoint when available,
- the latest Tour cheat sheet/checkpoint when it is newer and relevant.

Final code beats earlier plans, reviews, and earlier mental models.

Do not invent an earlier checkpoint when none exists.

## Understanding delta

Compare the final implementation with the latest reliable ownership/understanding
checkpoint when one exists.

The purpose is **not** to repeat One-by-one or Tour. Surface only changes that would
make the developer's earlier mental model incomplete or wrong.

Include a delta when later stages changed:

- runtime flow,
- state/responsibility ownership,
- an important abstraction or boundary,
- lifecycle behavior,
- a meaningful debugging entry point,
- important code/symbols the developer was told to remember.

Do not count these by themselves:

- renames,
- formatting,
- imports,
- small helper extraction with unchanged responsibility,
- test-only changes,
- visual polish with unchanged behavior,
- cleanup that preserves the same runtime/ownership model.

Explain each meaningful delta in normal sentences:

1. what the developer previously learned,
2. what is different in the final code,
3. why it changed,
4. what they should remember now.

Usually 0–4 deltas are enough.

If nothing meaningful changed, say:

> No meaningful runtime or ownership changes since the last ownership checkpoint. The
> previous mental model is still valid.

## Visual feature brief

Create a **ready-to-paste image-generation prompt** from the final implementation.

Purpose:

> Turn the feature into a single presentation-style technical knowledge slide that is
> useful for remembering the feature and explaining it to another developer.

This is not a UML diagram and not a code screenshot.

The image prompt should ask for a clean **16:9 slide / technical cheat sheet** with
short labels and strong visual hierarchy.

Use only facts supported by the final code and workflow artifacts.

Prefer these visual regions when relevant:

```text
1. Problem / reason the feature exists
2. Core solution idea
3. Front-to-back runtime flow
4. Ownership: which code area is responsible for what
5. Key invariant / "remember this"
6. Small debugging cheat sheet or important edge case
```

Keep code to an absolute minimum. Prefer file/symbol names, arrows, small concepts,
and visual grouping.

The generated image should be understandable in roughly 30–60 seconds and useful as a
"how this feature works" slide in a presentation.

### Prompt requirements

The prompt must:

- name the actual feature,
- describe the real problem and outcome,
- include the actual runtime/ownership flow,
- use short real file/symbol labels only where they help,
- avoid invented services/components,
- avoid paragraphs of tiny text,
- avoid dense UML/database-style notation,
- ask for a polished modern presentation slide rather than a generic flowchart,
- prioritize structure, relationships, and recall over source-code detail.

## Document shape

```markdown
# Dev Handoff — Feature

## 60-second overview
Problem → outcome → actual solution shape.

## Runtime flow
4–8 short steps from entry to observable result.

## Ownership map
| Code group | Owns | Important invariant |
|---|---|---|

## Understanding delta
Only meaningful runtime/ownership changes since the latest reliable checkpoint.
If none changed, state that the previous mental model is still valid.
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
A ready-to-paste image-generation prompt for one clean 16:9 presentation-style
technical knowledge slide.
```

Keep the handoff concise; no giant file changelog and no second full Tour.

## Chat output

After writing, give a short HOME-chat summary with:

- handoff path,
- best first file/group to review,
- single most important ownership point,
- whether the earlier mental model changed meaningfully,
- that the visual feature prompt is ready in the handoff.

If there was a meaningful Understanding Delta, explain only that delta in the chat
summary. Do not repeat unchanged parts.

### Pull request (copy-paste)

Always end the chat output with a PR title and description. They live in chat only,
not in `dev-handoff.md`.

**Title** — one plain outcome sentence in its own `text` block. No conventional-commit
prefix, no ticket id, no hashtags unless the user asks.

**Description** — its own `markdown` block with:

```text
## Summary
2–4 bullets: what the branch changes and why.

## Test plan
- [ ] 3–6 checkbox scenarios a reviewer can actually run

## Out of scope
Only boundaries a reviewer might otherwise expect to see covered.
```

Do not put code fences inside the description block.

Base both on the **actual commits** on the branch, including any scope the user added
after the original spec (for example small UI fixes on the same PR).

## Completion contract

After writing the handoff and the chat output, stop. JARVIS owns the final completion
recap and card.

When JARVIS is active, all Dev-handoff chat output must appear before the final JARVIS
transition card. Nothing may appear after the card.

## Suite convention

Persistent workflow artifacts are written in English.
User-facing chat follows the user's current working language.

---
name: dev-handoff
description: >-
  Writes the final concise English developer handoff from actual final code and commit
  history, preserving runtime flow, ownership boundaries, review order, verification,
  and important follow-ups.
---

# Dev-handoff — Make the Branch Easy to Own

## Goal

A developer should understand the branch in a few minutes without reading the whole AI
workflow history.

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
- resolved ownership decisions.

Final code beats earlier plans.

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
```

Keep it concise; no giant file changelog.

## Chat output

After writing, give a short HOME-chat summary with:

- handoff path,
- best first file/group to review,
- single most important ownership point.

Then say:

> Dev-handoff complete. Return control to JARVIS.

JARVIS owns the final completion recap.

## Suite convention

Persistent workflow artifacts are written in English.
User-facing chat follows the user's current working language.

---
name: blueprint
description: >-
  Planner-only repo-aware technical design for an approved JARVIS spec. Runs in the
  dedicated Blueprint fork with the strongest planning/reasoning model, writes an
  English implementation blueprint, and returns control without starting code.
---

# Blueprint — Repo-aware Implementation Design

## Session boundary

Blueprint runs only in the dedicated **Blueprint Fork**.

The user should have forked the HOME conversation and switched to the strongest
planning/reasoning model.

If invoked in the normal implementation chat before that boundary, do not start
planning; tell the user to follow the JARVIS Blueprint transition card.

## Goal

Turn the approved Spec into the smallest coherent implementation design supported by
real repository evidence.

Blueprint should resolve repo facts and expose only a few material ownership choices.

## Principles

- smallest coherent change,
- reuse before creation,
- no speculative abstraction,
- no unrelated refactor,
- explicit failure/error behavior,
- preserve established repo patterns,
- concrete code evidence,
- 0–3 real ownership checkpoints,
- plan chunks by behavior/mechanism, not file type.

## Artifact

Read:

```text
work/{work-id}/spec.md
```

Write/update in English:

```text
work/{work-id}/blueprint.md
```

Include:

- current flow / relevant repo evidence,
- reuse targets,
- change budget,
- create/modify/do-not-touch map,
- resolved design unknowns,
- material ownership checkpoints,
- implementation chunks,
- error/edge behavior per chunk,
- focused verification per chunk,
- scope guard.

Do not write production code.

## Chunk quality

Each chunk should be one understandable implementation unit, generally one mechanism
or a small group of closely related files.

Avoid standalone "types/helpers first" chunks unless independently meaningful.

## Completion contract

End with:

- blueprint path,
- short chunk overview,
- open ownership checkpoints if any,
- highest-risk chunk.

Then say:

> Blueprint complete. Return to the HOME JARVIS chat.

Do not start One-by-one. Do not choose the next workflow stage.

## Suite convention

Persistent workflow artifacts are written in English.
User-facing chat follows the user's current working language.

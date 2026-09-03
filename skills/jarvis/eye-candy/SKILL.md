---
name: eye-candy
description: >-
  Behavior-preserving readability/structure pass after Refine. Improves component,
  hook, JSX, predicate, and file scanability one meaningful cleanup per go, without
  changing product behavior or architecture ownership.
---

# Eye-candy — Make the Final Code Easy to Scan

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

Make the reduced implementation pleasant to read and own.

This is **code readability**, not visual UI polish.

Look for:

- mixed rendering/orchestration responsibilities,
- meaningful hook/component boundaries,
- deep JSX nesting,
- nested ternaries/boolean soup,
- unnamed important predicates,
- giant inline handlers,
- pointless fragmentation/wrapper files.

Avoid splitting by arbitrary line count.

## Candidate scan

Inspect the full changed feature first.

Show at most 1–5 high-value candidates.

For each:

- current readability problem,
- intended responsibility shape,
- affected files,
- explicit statement that behavior remains unchanged.

If nothing is worth changing, report `Eye-candy clean`.

## One cleanup per go

For each accepted candidate:

- wait for `go`,
- implement only that cleanup,
- run focused behavior-preserving checks,
- explain the new responsibility flow,
- stop before the next candidate.

## Escalate

Return control to JARVIS if a "readability" cleanup actually needs:

```text
behavior decision            → One-by-one
architecture/state ownership → Blueprint
product rule                 → Speculation
reduction/residue question   → Refine
```

## Completion contract

Say:

> Eye-candy complete. Return control to JARVIS.

Do not select Mentor-me yourself; JARVIS owns the session transition.

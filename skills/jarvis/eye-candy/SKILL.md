---
name: eye-candy
description: >-
  Behavior-preserving readability/structure pass after Refine. Improves component,
  hook, JSX, predicate, and file scanability one meaningful cleanup per go, without
  changing product behavior or architecture ownership.
---

# Eye-candy — Make the Final Code Easy to Scan

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

Stop and hand back to JARVIS if a "readability" cleanup actually needs:

```text
behavior decision            → One-by-one
architecture/state ownership → Blueprint
product rule                 → Speculation
reduction/residue question   → Refine
```

## Completion contract

Each cleanup block ends the response. When all accepted candidates are done (or the
scan is clean), stop. JARVIS closes the response with its card (nothing after it) and
waits for the user. Do not start or select the next stage — JARVIS owns the Mentor
session transition.

## Suite convention

Persistent workflow artifacts are written in English.
User-facing chat follows the user's current working language.

---
name: refine
description: >-
  Reduces a working implementation into the smallest clear version worth keeping
  while preserving verified behavior. Uses an explicit behavior lock before risky
  deletion/consolidation and returns control to JARVIS.
---

# Refine — Reduce Without Breaking Behavior

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

Remove AI-generated residue and unnecessary concepts after implementation works.

Prefer:

```text
same behavior
less code
fewer concepts
clearer ownership
more reuse
```

Refine is not visual/JSX scanability polish; Eye-candy owns that later.

## Scope

Only code introduced or materially affected by the current task.

Look for:

- redundant state/effects,
- duplicated transforms,
- dead/temporary code,
- unnecessary wrappers/helpers,
- abstractions used without real pressure,
- duplicated types/validation,
- unnecessary files,
- missed existing reuse.

Do not perform unrelated repository cleanup.

## Behavior lock

Refine may delete a surprising amount of code, so every non-trivial reduction needs a
behavior lock.

Before changing it:

1. state the behavior/invariant that must remain true,
2. identify the relevant Spec acceptance criteria and current code path,
3. use focused existing tests/checks as a baseline when available,
4. make the smallest reduction,
5. run the same focused verification afterward.

If the reduction changes externally observable behavior, violates an accepted
ownership decision, or cannot be verified with reasonable confidence, **do not keep
that reduction**.

When risky logic has no useful regression protection, prefer leaving it intact or
adding only a narrowly justified focused test rather than deleting optimistically.

## Do not

- redesign the feature,
- reopen product rules,
- move architecture ownership casually,
- add speculative abstractions,
- run broad formatting/lint loops,
- optimize for negative line count.

## Output

### Refine result
Short quality summary.

### Behavior preserved
Name the important invariants protected.

### Simplified / removed
Only meaningful reductions.

### Kept intentionally
Surprising complexity that still earns its place.

### Final mental model
2–5 bullets or short steps describing the resulting responsibilities.

### Verification
Checks actually run before/after where relevant.

A clean no-op is valid.

## Completion contract

Say:

> Refine complete. Return control to JARVIS.

Do not select the next workflow stage.

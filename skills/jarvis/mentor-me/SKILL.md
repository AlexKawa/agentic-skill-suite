---
name: mentor-me
description: >-
  Independent read-only correctness, architecture, security, minimality, and test
  review of the final post-Refine/post-Eye-candy implementation. Initial review runs
  in a brand-new clean chat with a strong review model and writes an English review.
---

# Mentor-me — Independent Final Review

## Session rule

Initial Mentor-me runs in a **new clean chat**, not a fork of the implementation chat.

Use a strong review/reasoning model.

The reviewer must rebuild understanding from durable evidence:

```text
work/{work-id}/spec.md
work/{work-id}/blueprint.md
current code/diff
tests
repo rules / relevant ADRs
resolved ownership decisions
```

Do not rely on implementation-chat explanations.

## Read-only rule

Mentor-me diagnoses and teaches. It never fixes production code.

Artifact:

```text
work/{work-id}/mentor-review.md
```

## Review priorities

1. Spec/acceptance correctness
2. Blueprint/ownership compliance
3. bugs/runtime behavior
4. async/lifecycle/races
5. security/trust boundaries
6. failure behavior
7. unnecessary complexity/reuse
8. React/Next/data concerns when relevant
9. meaningful regression coverage

Do not produce generic best-practice padding.

Every finding requires evidence and concrete impact.

## Finding shape

```markdown
### MM-001 — Short title

**Status:** Open
**Severity:** Critical | High | Medium | Low
**Confidence:** High | Medium
**Return stage:** Grunt-work | One-by-one | Refine | Eye-candy | Blueprint | Speculation

**Evidence**
...

**Problem**
...

**Why it matters**
...

**Learning note**
Explain the underlying idea in simple language matching the current chat language.

**Recommended direction**
Smallest useful constraint/direction; do not implement the fix.
```

## Routing

```text
small local behavior             → Grunt-work
larger implementation behavior   → One-by-one
generated residue/reduction      → Refine
readability/component/hook shape → Eye-candy
architecture/responsibility      → Blueprint
product rule                     → Speculation
```

## Verdict

Use one:

```text
Ready for commits
Changes required
Blocked by design decision
Blocked by product decision
```

## Verification review

After fixes, the same dedicated Mentor chat may be reused.

Re-check open findings against current code and mark:

```text
Resolved
Still open
Changed
Accepted
```

Do not write code.

## Completion contract

Write/update `mentor-review.md`, summarize verdict, then say:

> Mentor-me review complete. Return to the HOME JARVIS chat.

Do not choose the next workflow stage.

## Suite convention

Persistent workflow artifacts are written in English.
User-facing chat follows the user's current working language.

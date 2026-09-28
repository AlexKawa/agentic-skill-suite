---
name: grunt-work
description: >-
  Handles concrete post-implementation remarks and Mentor findings in the HOME JARVIS
  chat. Triages the full queue, then fixes exactly one local root cause per go using
  the same coding model. Escalates larger work instead of hiding it as a small fix.
---

# Grunt-work — Small Corrections, One at a Time

Grunt-work stays in the **HOME chat** with the same coding model as One-by-one. Never
ask for a new chat, fork, or model switch because Grunt-work started.

## Input

The user may send all remarks at once (or they come from Mentor findings). Persist the
queue in English in `work/{work-id}/grunt-work.md` — one file, not one per remark.

## First turn: triage only

Classify the whole queue before any code:

```text
Safe Grunt
One-by-one
Blueprint
Speculation
Duplicate / same root cause
Deferred
```

Merge remarks that share one root cause. Then preview only the first Safe Grunt item
and wait for `go`.

## One root cause per go

Preview the current item: observed behavior, actual root cause after inspection,
smallest fix, files/area, and what stays untouched. `go` authorizes exactly that one
root cause.

After the fix, use the One-by-one chunk message shape, scaled down:

- **Changed files** — every file, linked, with its role in the fix.
- **Before → after** — 2–5 steps through the relevant code.
- **In short** — 2–3 simple sentences: what was wrong, what changed, where to look if
  it comes back.
- **Checks** — focused checks actually run.
- **Next** — preview of the next queue item.

Update `grunt-work.md`, then stop.

## Escalation

Do not hide larger work as a small fix. Stop and hand back to JARVIS — no code — when
the clean fix needs:

```text
new/meaningful implementation mechanism → One-by-one
responsibility/architecture change       → Blueprint
new/unclear product rule                 → Speculation
```

## Completion contract

Each root cause ends the response. When all items are Fixed / Escalated / Deferred /
Duplicate, summarize the counts and stop. JARVIS closes the response with its card
(nothing after it) and waits for the user. Do not start or select the next stage.

## Suite convention

Persistent workflow artifacts are written in English.
User-facing chat follows the user's current working language.

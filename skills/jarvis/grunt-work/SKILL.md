---
name: grunt-work
description: >-
  Handles concrete post-implementation remarks in the HOME JARVIS chat. Triages the
  full queue, then fixes exactly one local root cause per go using the same coding
  model. Escalates larger work instead of creating another planning/session layer.
---

# Grunt-work — Small Corrections, One at a Time

## Session rule

Grunt-work stays in the **HOME chat** and uses the same capable coding model as
One-by-one.

Do not request a new chat, fork, or planning-model switch merely because Grunt-work
started.

## Input

The user may send all concrete remarks at once.

Persist them in English:

```text
work/{work-id}/grunt-work.md
```

Do not create one file/ticket for every trivial remark.

## First turn: triage only

Classify the whole queue before code:

```text
Safe Grunt
One-by-one
Blueprint
Speculation
Duplicate / same root cause
Deferred
```

Merge remarks that share one root cause.

Then preview only the first Safe Grunt item and wait for `go`.

## One issue per go

For the current item show:

- observed behavior,
- actual/root cause after inspection,
- smallest fix,
- files/area,
- what remains untouched.

`go` authorizes exactly one root cause.

After implementation show:

### What changed
2–3 sentences.

### Flow after the fix
2–5 steps through the relevant code.

### Ownership
Small table of changed code + responsibility + why.

### Checks
Focused checks actually run.

Update `grunt-work.md`, preview the next queue item, and stop.

## Escalation

Do not hide larger work as a small fix.

Return control to JARVIS when the clean fix requires:

```text
new/meaningful implementation mechanism → One-by-one
responsibility/architecture change       → Blueprint
new/unclear product rule                 → Speculation
```

No code for an escalated item.

## Completion contract

When all items are Fixed / Escalated / Deferred / Duplicate, summarize counts and say:

> Grunt-work complete. Return control to JARVIS.

Do not select the next workflow stage.

## Suite convention

Persistent workflow artifacts are written in English.
User-facing chat follows the user's current working language.

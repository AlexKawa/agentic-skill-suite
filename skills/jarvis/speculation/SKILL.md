---
name: speculation
description: >-
  Clarifies issue-local intent, behavior, scope, edge cases, and acceptance criteria
  before technical design. Writes an approved English spec and returns control to
  JARVIS without choosing the next stage.
---

# Speculation — Shared Understanding Before Design

## Goal

Clarify what must become true before deciding how to build it.

Speculation owns product/feature behavior for the current JARVIS issue. It does not
write production code or implementation architecture.

## Working style

- Ask one meaningful question per turn.
- Inspect repo/docs for discoverable facts instead of asking the user.
- Let the user form meaningful product decisions before revealing your preference.
- Use short normal paragraphs; avoid question batteries and label soup.
- Do not reopen initiative-level decisions inherited from Shaper unless new evidence
  contradicts them.

## Discover

Resolve only what materially affects the issue:

- intent,
- in/out of scope,
- happy path,
- relevant loading/error/empty/cancel/retry/lifecycle behavior,
- important regression boundaries,
- observable acceptance criteria,
- relevant auth/i18n/security/data/mobile implications.

Engineering implementation forks may remain as explicit design unknowns for Blueprint.

## Artifact

Write/update:

```text
work/{work-id}/spec.md
```

The file is always English, even when chat is German.

It should explain:

```text
WHY
WHAT
SCOPE
DONE
BOUNDARIES
OPEN ENGINEERING QUESTIONS
```

Avoid implementation filenames, hook architecture, or commit plans.

Deferred work goes to:

```text
work/{work-id}/follow-ups.md
```

## Complete when

- intent and scope are stable,
- acceptance criteria are observable,
- meaningful product decisions are resolved,
- regression/failure rules are clear enough,
- remaining unknowns are genuine engineering questions,
- user approves/corrects the Spec.

## Completion contract

Summarize the approved Spec in chat and say:

> Speculation complete. Return control to JARVIS.

Do not tell the user which workflow stage comes next. JARVIS owns ordering.

## Suite convention

Persistent workflow artifacts are written in English.
User-facing chat follows the user's current working language.

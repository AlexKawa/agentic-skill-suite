# Changelog

## [1.1.0] - 2026-09-28

Focus: developer ownership during implementation and a JARVIS flow that stays on
track — one step per message, consistent outputs, and less instruction bloat.

### Added

- **Tour skill** — read-only utility. Runs as a Context Tour preflight before
  Speculation (skippable) and as a standalone recap (`tour`, `tour step`) without
  changing the workflow position.
- **Turn contract** — every HOME reply re-anchors, runs at most one authorized step,
  stops, and ends with the transition card, including after side questions.
- **`work/{work-id}/jarvis-state.md`** — tiny mirror of the last card so the flow
  survives long or summarized chats.
- **Skip contract** — `skip` passes exactly one current step; Mentor review gates are
  never skippable.
- **Transition card** — phase rail (SHAPE → SHIP), fixed line order, stricter rendering
  rules, copy-paste prompts in their own block above the card, transition table.
- **One-by-one chunk message template** — changed files (linked), **In short**,
  important code, before → after, checks, next preview; always in this order.
- **Ready-commits rules** — granular commits, whole-file rule, always
  `type(scope): subject`, two messages with a short technical body, copyable blocks.
- **Dev-handoff** — copy-paste PR title and description in chat, understanding delta
  since the One-by-one checkpoint, and a visual feature brief prompt.
- **Mentor loop** — explicit intake turns, triage before any fix, verification in the
  existing Mentor chat before Polish.

### Changed

- **Specialists stop instead of handing back mid-turn** — "Return control to JARVIS"
  is replaced by an explicit stop, so the next stage never starts in the same reply.
- **No multi-stage exception** — naming several stages in one message runs only the
  first valid step.
- **Grunt-work** — after-fix message follows the One-by-one shape, scaled down.
- **Orchestrator condensed** — each rule stated once, stage details live in the
  specialists, golden rules reduced to a short checklist (`jarvis` skill ~970 → ~340
  lines).
- **`work/`** — documented as workflow state that stays out of product commits.

### Removed

- **`map` command.**
- **Hunk-split commits** in Ready-commits (`git add -p`).

## [1.0.0] - 2026-09-03

First public release.

JARVIS has gone through several private iterations before this release.
This version marks the beginning of the public release history.

---
name: jarvis
description: >-
  Orchestrates the JARVIS developer workflow with one stable stage order, a simple
  three-session model, explicit transition cards, and strong developer ownership.
  Use at the start of a task or when the user says jarvis, status, continue, resume,
  back, stop, tour, or asks what happens next.
---

# JARVIS — Developer Workflow Orchestrator

## Language policy

Skill instructions and all persistent workflow artifacts are written in English.

Persistent artifacts include Specs, Blueprints, follow-ups, Grunt-work queues,
Mentor reviews, Dev handoffs, and other repository-persisted workflow documents.

User-facing chat is independent from artifact language. Follow the user's current
working language and switch naturally when the user switches between German and
English.

Code, identifiers, repository paths, APIs, package names, and canonical domain terms
follow repository conventions.


## Mission

JARVIS is the **single source of truth for workflow order**.

Specialist skills own how their stage works. They do **not** decide which stage comes
next. When a specialist finishes, it returns control to JARVIS.

The full quality pipeline stays intact, but is grouped into five mental phases:

```text
SHAPE
Speculation → Blueprint

BUILD
One-by-one → Manual Try-out → optional Grunt-work

CLEAN
Refine → Eye-candy

REVIEW
Mentor-me

SHIP
Polish → Ready-commits → manual commits → Dev-handoff
```

## The three-session model

There are only three session types.

### 1. HOME CHAT

The normal implementation conversation.

Runs:

```text
Speculation
One-by-one
Manual Try-out
Grunt-work
Refine
Eye-candy
Polish
Ready-commits
Dev-handoff
```

Use the same capable coding model for normal HOME work unless the user explicitly
chooses otherwise.

### 2. BLUEPRINT FORK

Blueprint is the only normal **fork**.

```text
HOME
  ↓
Speculation approved
  ↓
FORK current chat
  ↓
switch to strongest planning/reasoning model
  ↓
Blueprint only
  ↓
return HOME
```

A fork is useful here because Blueprint benefits from the already-built product/spec
context while planning remains isolated from implementation.

### 3. MENTOR CLEAN CHAT

Initial Mentor-me is the only normal **brand-new clean chat**.

```text
HOME
  ↓
Refine + Eye-candy complete
  ↓
NEW CLEAN CHAT
  ↓
strongest review/reasoning model
  ↓
Mentor-me read-only review
  ↓
return HOME
```

The reviewer rebuilds its understanding from persistent artifacts and code instead of
implementation-chat explanations.

### No special Grunt-work session

Grunt-work stays in HOME and uses the same coding model as One-by-one.

Its safety comes from strict issue-size triage and one issue per `go`, not from another
chat/model boundary.

---

# Mandatory JARVIS transition card

At every meaningful stop, end the response with the same compact card.

Do not improvise a different transition format.

Normal example:

```text
──────────────── JARVIS ────────────────
BUILD · One-by-one · Chunk 2/4
Session: SAME CHAT · coding model
Next: Chunk 3
Action: `go`
```

Blueprint boundary:

```text
──────────────── JARVIS ────────────────
SHAPE · Speculation complete
Session: FORK THIS CHAT · strongest planning model
Next: Blueprint
Start: "Fork done. Strongest model selected. Run Blueprint for work/<id>/spec.md."
Return: this HOME chat when blueprint.md is ready
```

Mentor boundary:

```text
──────────────── JARVIS ────────────────
CLEAN · Eye-candy complete
Session: NEW CLEAN CHAT · strongest review model
Next: Mentor-me
Start: "Run Mentor-me initial review for work/<id>. Read-only review."
Return: this HOME chat when mentor-review.md is ready
```

Manual Git boundary:

```text
──────────────── JARVIS ────────────────
SHIP · Ready-commits complete
Session: SAME CHAT
Next: manual commits
Action: commit the prepared groups, then `continue`
```

The card should remain short enough to scan in a second.

---

# Task-local artifacts

Choose one `{work-id}` and reuse it for the full JARVIS task.

```text
work/{work-id}/
├── spec.md
├── blueprint.md
├── follow-ups.md
├── grunt-work.md       optional
├── mentor-review.md
└── dev-handoff.md
```

Every file above is written in English.

Durable system/domain knowledge remains in its normal repo location, for example:

```text
CONTEXT.md
CONTEXT-MAP.md
docs/adr/**
```

---

# Specialist discovery

Locate specialist skills by frontmatter `name:` rather than relying on one editor path.

JARVIS should locate:

```text
speculation
blueprint
one-by-one
grunt-work
refine
eye-candy
mentor-me
polish
ready-commits
dev-handoff
```

Specialists must not carry their own hard-coded pipeline order. Their completion
contract is:

> Stage complete. Return control to JARVIS.

---

# Phase 1 — SHAPE

## Stage 1 — Speculation

Question:

> What exactly should become true?

Artifact:

```text
work/{work-id}/spec.md
```

Complete when issue/product behavior, scope, acceptance criteria, meaningful
edge/failure behavior, and regression boundaries are clear.

After approval, JARVIS uses the **Blueprint Fork** transition card.

## Stage 2 — Blueprint

Question:

> How should this be built in this repo with minimum necessary complexity?

Artifact:

```text
work/{work-id}/blueprint.md
```

Blueprint runs only in the Blueprint Fork. It is planner-only and never implements.

When complete, the fork tells the user only:

> Blueprint complete. Return to the HOME JARVIS chat.

When the user returns to HOME and says `continue`, JARVIS re-reads `blueprint.md`,
enters One-by-one, shows the implementation overview + Chunk 1 preview, and waits for
`go`.

`continue` never authorizes Chunk 1 code by itself.

---

# Phase 2 — BUILD

## Stage 3 — One-by-one

One-by-one implements one Blueprint chunk per `go`.

The developer should not merely see a list of files. After each chunk, the output must
show the **runtime/responsibility flow**:

```text
user/system entry
    ↓
file / symbol that receives it
    ↓
state/domain/request owner
    ↓
boundary/service
    ↓
observable result
```

Use simple language in the current chat language. Introduce technical terms only when
they genuinely help; explain unfamiliar terms in one short sentence.

At the end of One-by-one, provide a complete feature flow and a short manual try-out
list. Do not run browser smoke tests by default.

### Code Tour

`tour` is a read-only ownership aid, not a workflow stage.

Default `tour`:

- covers 3–6 important code stops,
- stays in **one response**,
- uses current context instead of re-analyzing the whole repo when possible,
- does not modify code,
- does not run checks,
- uses clickable file references when supported,
- explains what each stop owns and why the change exists,
- avoids Mermaid by default.

This keeps token/time cost low.

`tour step` is optional interactive mode: one code stop per user `continue`.

A text flow is the default. Mermaid/graph output is only used when the user explicitly
asks for a diagram/map.

## Manual Try-out checkpoint

After the last One-by-one chunk, the developer gets 3–7 high-signal scenarios.

They may:

- try them and return with remarks,
- say the feature looks clean,
- continue without testing now.

No extra specialist or chat is created.

## Stage 4 — Grunt-work (optional)

Use only for concrete observed remarks.

Persist remarks in:

```text
work/{work-id}/grunt-work.md
```

Triage the whole list first. Then process **exactly one root cause per `go`** in HOME.

Routing:

```text
local correction                  → Grunt-work
larger implementation mechanism   → One-by-one
architecture/responsibility       → Blueprint Fork
product behavior                  → Speculation
```

Grunt-work uses the same coding model as HOME. Do not add a planning-model switch.

---

# Phase 3 — CLEAN

## Stage 5 — Refine

Question:

> What can disappear, collapse, or reuse existing code while behavior stays the same?

Refine is the reduction pass and may remove significant generated residue.

### Behavior lock

Before a non-trivial reduction:

1. identify the behavior/invariant being preserved,
2. use relevant existing tests or focused checks as the baseline when available,
3. make the smallest behavior-preserving reduction,
4. run the same relevant verification afterward,
5. if confidence drops or observable behavior changes, do not keep the reduction.

Refine must never trade verified behavior for a smaller diff.

No broad Prettier/lint loop here; Polish owns mechanical cleanup.

## Stage 6 — Eye-candy

Question:

> Is the reduced implementation easy to scan, navigate, and own?

Eye-candy improves code structure/readability without changing behavior or
architecture: component/hook boundaries, calm JSX, named predicates, shallow
conditions, and removal of pointless fragmentation.

Meaningful cleanup remains one block per `go`.

When complete, JARVIS uses the **Mentor Clean Chat** transition card.

---

# Phase 4 — REVIEW

## Stage 7 — Mentor-me

Initial Mentor-me runs in a **new clean chat** and is read-only.

Artifact:

```text
work/{work-id}/mentor-review.md
```

The reviewer reads the Spec, Blueprint, current code/diff, tests, repo rules, and
ownership decisions. It diagnoses; it never fixes production code.

If findings exist, return HOME. JARVIS routes each finding to the smallest responsible
stage.

After semantic fixes, verification is required again. Reuse the same dedicated Mentor
chat for verification when convenient; it already knows the findings but still has
never written code.

Expected clean verdict:

```text
Ready for commits
```

---

# Phase 5 — SHIP

## Stage 8 — Polish

Polish runs only when the current Mentor verdict is clean.

It owns consolidated mechanical cleanup:

- Prettier,
- ESLint warnings/errors,
- imports,
- relevant i18n/static-quality rules,
- other safe changed-file-only mechanical fixes.

### Mentor validity invariant

**Any semantic code change invalidates the current Mentor verdict.**

A semantic change includes behavior, meaningful control flow, state ownership,
component/hook architecture, API/data contracts, or non-trivial error behavior.

Therefore Polish may not silently make such a fix. It must return control to JARVIS,
which routes the issue, then Mentor verification runs again.

Pure formatting/import/mechanical cleanup does not require another Mentor pass.

## Stage 9 — Ready-commits

Plan-only. Never stage or commit.

Before commit groups, show a compact final code ownership overview:

| Code group | Owns | Why it changed |
|---|---|---|
| ... | ... | ... |

Then group changes into coherent commits and explain the purpose of each group.
Files should be presented as related code stories, not a raw file dump.

User commits manually.

## Stage 10 — Dev-handoff

Artifact:

```text
work/{work-id}/dev-handoff.md
```

Use final code and actual commits as source of truth.

The handoff must preserve the feature's runtime flow, responsibility map, key
decisions, review order, verification, and important follow-ups in concise English.

After Dev-handoff, JARVIS gives one short completion recap and stops.

---

# Checks policy

Before Polish:

- focused tests are encouraged when they protect the current behavior,
- relevant type/compile checks are appropriate around changed contracts,
- do not run browser/manual smoke tests by default,
- do not repeatedly run broad lint/Prettier,
- do not run full-repo checks merely because a chunk completed.

Refine and Eye-candy should use focused verification proportional to the structural
risk they introduce.

---

# Backtracking

Use the earliest stage whose responsibility became invalid.

```text
product rule                  → Speculation
architecture/responsibility   → Blueprint Fork
planned implementation        → One-by-one
observed local correction     → Grunt-work
generated residue/reduction   → Refine
readability/structure         → Eye-candy
review correctness            → Mentor-me verification
mechanical cleanup            → Polish
```

Do not patch around a wrong earlier decision.

---

# Commands

### `jarvis`
Start/recover.

### `status`
Show phase, stage, work-id, artifact state, and the mandatory transition card.

### `continue` / `weiter`
Advance only through the transition currently offered.

### `go`
Authorize exactly one One-by-one chunk, Grunt-work issue, or Eye-candy cleanup block.

### `tour`
Show a compact 3–6-stop code tour in one response.

### `tour step`
Interactive code tour; one stop per `continue`.

### `map`
Optional visual/text dependency map. Mermaid only if the user explicitly wants it.

### `skip`
Skip only the explicitly optional/proposed item.

### `back`
Return to the appropriate earlier stage.

### `stop`
Stop modifications and show current transition card.

---

# Resume / recovery

On a new HOME session:

1. identify `{work-id}`,
2. inspect `work/{work-id}/`,
3. inspect Git state,
4. reconstruct the smallest reliable status,
5. show the JARVIS transition card.

Artifact existence alone does not prove completion.

The order must always remain:

```text
Speculation
Blueprint
One-by-one
optional Grunt-work
Refine
Eye-candy
Mentor-me
Polish
Ready-commits
Dev-handoff
```

Never forget Refine or swap Refine/Eye-candy/Mentor-me.

---

# Golden rules

1. JARVIS alone owns workflow order.
2. Specialists return control; they do not choose the next stage.
3. Only three session types exist: HOME, Blueprint Fork, Mentor Clean Chat.
4. Grunt-work stays HOME with the coding model.
5. Every transition uses the same JARVIS card.
6. One-by-one teaches runtime/responsibility flow, not just changed files.
7. `tour` is optional, read-only, compact, and normally one response.
8. No Mermaid by default.
9. Refine uses a behavior lock before risky reductions.
10. Mentor reviews the post-Refine/post-Eye-candy code.
11. Any later semantic change invalidates the Mentor verdict.
12. Polish is mechanical only.
13. Ready-commits includes a final ownership/code-group overview.
14. Persistent workflow artifacts are English; chat is adaptive.
15. The goal is strong code **and** strong developer ownership, without workflow ceremony.

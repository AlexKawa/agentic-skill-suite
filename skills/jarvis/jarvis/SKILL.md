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

`tour` is a read-only utility skill. It may be used before, during, or after the
pipeline without becoming a stage or advancing workflow state.

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

## Turn contract

Applies to **every HOME response** while a JARVIS task is active.

1. **Re-anchor.** Re-read `work/{work-id}/jarvis-state.md`. If these JARVIS rules are
   no longer fully in context (long chat, summarized history), re-read this skill first.
2. **Do only what is authorized.** `go` runs the one previewed item. `continue` runs
   the one step named in **Next** on the last card / state file. Anything else — a
   question, a remark, a Mentor report — is handled without advancing the pipeline.
3. **Stop after one step.** When a stage or work item completes in this response, the
   response ends. Never start the next stage, even when it looks trivial or obvious.
4. **Close.** Update `jarvis-state.md`, then end with the JARVIS card as the final
   block — also after side questions (repeat the current card unchanged).

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

While a JARVIS task is active, every HOME response ends with the same compact card —
stage work, intake turns, and side questions alike. For a side question, repeat the
current card unchanged.

## Final-block invariant

The JARVIS transition card is the **final rendered block of the response**.

Nothing may appear after it:

- no specialist completion sentence,
- no commit suggestion,
- no extra note,
- no verification reminder,
- no "return control" line,
- no prose of any kind.

All stage output, summaries, checks, caveats, and suggestions must appear **before**
the card.

JARVIS alone renders the transition card. A specialist's completion contract is a
**stop signal**, not a second footer and not a hand-over that lets JARVIS run the next
stage in the same response. If a completion sentence is useful to the user, place it
before the card; usually the card makes it unnecessary.

## Card format

Render the card in exactly this shape:

```text
──────────────── JARVIS ────────────────
✓ SHAPE  ▶ BUILD  · CLEAN  · REVIEW  · SHIP
Stage:   One-by-one · Chunk 2/4 done
Next:    Chunk 3 · Persist draft on blur
Session: SAME CHAT · coding model
Action:  go
```

Lines, always in this order:

- **Header** — always identical.
- **Phase rail** — all five phases in fixed order. `✓` done, `▶` the phase of the
  Next step, `·` pending. Always present so the developer sees where the task stands.
- **Stage** — current stage and position inside it (chunk n/m, fix n/m). Inside the
  Mentor loop, name the loop and round, e.g. `Mentor loop · round 2 · fix 1/2 done`.
- **Next** — the single next step.
- **Session** — `SAME CHAT`, `FORK THIS CHAT`, `NEW CLEAN CHAT`, or
  `MENTOR CLEAN CHAT · existing`, plus the model.
- **Action** — exactly what the user types or does.
- **Return** — only at session boundaries: where to come back and when.

Rendering rules:

- The card is its own fenced `text` block: blank line before it, opening and closing
  fence each on their own line.
- Never put the card inside a list, quote, table, or another code block.
- Close every earlier code block in the response before the card.
- No backticks, bold, links, or other markdown inside the card.
- Keep each line under ~60 characters; shorten Next rather than wrapping.
- Copy-paste prompts for a fork or new chat never go inside the card. Put them in their
  own `text` block directly above the card, introduced by one short sentence; the
  card's Action points to it.

Do not improvise a different format.

## Card examples

Blueprint boundary — prompt block, then card:

```text
Fork done. Strongest model selected. Run Blueprint for work/<id>/spec.md.
```

```text
──────────────── JARVIS ────────────────
▶ SHAPE  · BUILD  · CLEAN  · REVIEW  · SHIP
Stage:   Speculation complete
Next:    Blueprint
Session: FORK THIS CHAT · strongest planning model
Action:  fork, switch model, paste the prompt above
Return:  here when blueprint.md is ready
```

Mentor boundary — prompt block, then card:

```text
Run Mentor-me initial review for work/<id>. Read-only review.
```

```text
──────────────── JARVIS ────────────────
✓ SHAPE  ✓ BUILD  ✓ CLEAN  ▶ REVIEW  · SHIP
Stage:   Eye-candy complete
Next:    Mentor-me initial review
Session: NEW CLEAN CHAT · strongest review model
Action:  open a new chat, paste the prompt above
Return:  here when mentor-review.md is ready
```

Mentor findings triaged in HOME:

```text
──────────────── JARVIS ────────────────
✓ SHAPE  ✓ BUILD  ✓ CLEAN  ▶ REVIEW  · SHIP
Stage:   Mentor loop · round 1 · findings triaged
Next:    Fix 1/3 · MM-001 <short title>
Session: SAME CHAT · coding model
Action:  go
```

Mentor fixes complete — prompt block, then card:

```text
Verify the open findings for work/<id> against current code. Read-only.
```

```text
──────────────── JARVIS ────────────────
✓ SHAPE  ✓ BUILD  ✓ CLEAN  ▶ REVIEW  · SHIP
Stage:   Mentor loop · round 1 · fixes 3/3 done
Next:    Mentor-me verification
Session: MENTOR CLEAN CHAT · existing · review model
Action:  paste the prompt above in the Mentor chat
Return:  here when mentor-review.md is updated
```

Mentor verification clean:

```text
──────────────── JARVIS ────────────────
✓ SHAPE  ✓ BUILD  ✓ CLEAN  ✓ REVIEW  ▶ SHIP
Stage:   Mentor verified · Ready for commits
Next:    Polish
Session: SAME CHAT · coding model
Action:  continue
```

Polish complete:

```text
──────────────── JARVIS ────────────────
✓ SHAPE  ✓ BUILD  ✓ CLEAN  ✓ REVIEW  ▶ SHIP
Stage:   Polish complete
Next:    Ready-commits
Session: SAME CHAT · coding model
Action:  continue
```

Manual Git boundary:

```text
──────────────── JARVIS ────────────────
✓ SHAPE  ✓ BUILD  ✓ CLEAN  ✓ REVIEW  ▶ SHIP
Stage:   Ready-commits complete
Next:    manual commits, then Dev-handoff
Session: SAME CHAT
Action:  commit the groups, then continue
```

Task done:

```text
──────────────── JARVIS ────────────────
✓ SHAPE  ✓ BUILD  ✓ CLEAN  ✓ REVIEW  ✓ SHIP
Stage:   Dev-handoff complete
Next:    nothing · task done
Session: SAME CHAT
Action:  open the PR
```

---

# Stage advancement (strict)

**One user message → at most one forward pipeline stage.**

Do not chain stages in a single assistant turn unless the user **explicitly names**
multiple stages in one message (for example: "run polish, then ready-commits").

### What counts as one stage

Each of these is exactly **one** stage and must complete (and show a transition card)
before the next runs:

```text
Polish
Ready-commits
Dev-handoff
Each One-by-one chunk
Each Grunt-work root cause
Each Eye-candy block
Mentor-me initial (Mentor Clean Chat)
Mentor-me verification (Mentor Clean Chat)
```

### Mentor intake is not `continue`

Messages that **report** Mentor progress are **intake only**. They do **not** advance
SHIP or run the next specialist:

```text
"mentor done" / "2nd mentor check done" / "verification clean" / "ready for commits"
```

On intake, HOME must:

1. re-read `work/{work-id}/mentor-review.md` (and Git diff if needed),
2. interpret the verdict,
3. emit **one** JARVIS transition card for the **single** next authorized step.

HOME must **not** on an intake turn:

- run Polish, Ready-commits, or Dev-handoff,
- rewrite `mentor-review.md` verdict lines (verification writes belong in **Mentor Clean Chat**),
- treat conversation summaries or handoff prompts as permission to batch SHIP.

Only **`continue` / `weiter`** advances from the **last transition card** — exactly
one stage per `continue`.

### Forbidden single-turn bundles

Never combine in one assistant response:

```text
Polish + Ready-commits
Ready-commits + Dev-handoff
Mentor intake handling + any SHIP stage
Polish + Dev-handoff
```

Ready-commits content (staging commands, commit messages) must appear **before** the
transition card when that stage runs — never after the card, and never bundled with
the next stage.

### Dev-handoff gate

Dev-handoff runs only when:

- Polish is complete,
- Ready-commits plan was delivered,
- the user has **committed** (or explicitly says to skip commits and write handoff now).

Do not write `dev-handoff.md` during Mentor intake or in the same turn as Polish /
Ready-commits unless the user explicitly requests that combined scope.

---

# Task-local artifacts

Choose one `{work-id}` and reuse it for the full JARVIS task.

```text
work/{work-id}/
├── jarvis-state.md
├── spec.md
├── blueprint.md
├── follow-ups.md
├── grunt-work.md       optional
├── mentor-review.md
└── dev-handoff.md
```

Every file above is written in English.

## `jarvis-state.md`

A tiny mirror of the last card, so the flow survives long chats and summarized history.
Create it with the work-id; overwrite it whenever the card changes.

```markdown
# JARVIS state — <work-id>

Active JARVIS flow. Re-read the jarvis skill before acting on this task.

Phase: REVIEW
Stage: Mentor loop · round 2 · fix 1/2 done
Mentor verdict: Changes required (round 2)
Next: Fix 2/2 · MM-004 <short title>
Action: go
```

Keep it to these lines. It is not a log and not a second artifact of stage output.

If the state file, the last card, and Git/artifact reality disagree, say so, reconstruct
the smallest reliable status, and show the card — do not guess forward.

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
tour
grunt-work
refine
eye-candy
mentor-me
polish
ready-commits
dev-handoff
```

Specialists must not carry their own hard-coded pipeline order. Their completion
contract is a **stop**: the stage's output is done, JARVIS closes the response with the
card, and the next stage waits for the user's next message.

---

# Utility — Tour

Tour is a separate read-only specialist, not part of the stage order.

Use it when the user wants to understand existing code without changing it, for
example:

- before Speculation to orient around an unfamiliar feature/working tree,
- during a task when context was lost,
- after One-by-one or later as a whole-feature recap,
- before explaining the feature to another developer.

Tour may inspect the current work-id, working tree, named feature, or named code path.
It teaches runtime flow, ownership, focused annotated code, and debugging entry points.

Tour never advances the workflow. After it completes, resume the exact pipeline
position and transition/action that was active before Tour.

One-by-one does **not** depend on Tour for learning. One-by-one teaches every chunk
immediately as it is built.

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

Its primary ownership mechanism is **immediate chunk teaching**, not a later Tour.

Before each chunk, the developer gets a short preview:

```text
why this chunk
where it sits
current → target behavior
```

After each chunk, One-by-one uses its fixed **chunk message template** (changed files
→ important code → before/after → in short → checks → next preview). JARVIS does not
restate, shorten, or vary that template.

Teaching annotations may appear in chat code blocks but are not temporary production
comments.

Keep chunks small enough to understand immediately. If a chunk would require several
independent explanations or a long walkthrough, split it before implementation.

At the end of One-by-one, provide:

- a concise front-to-back feature flow,
- an ownership/debugging checkpoint,
- a short manual try-out list.

That checkpoint lets Dev-handoff later explain only meaningful changes introduced by
Refine, Eye-candy, Mentor fixes, or other later stages.

Do not run browser smoke tests by default.

## Manual Try-out checkpoint

After the last One-by-one chunk, the developer gets 3–7 high-signal scenarios.

They may:

- try them and return with remarks,
- say the feature looks clean,
- continue without testing now.

No extra specialist or chat is created.

## Stage 4 — Grunt-work (optional)

Use for concrete observed remarks **and** for implementation of Mentor findings that
route to Grunt-work.

Persist remarks in:

```text
work/{work-id}/grunt-work.md
```

### Mandatory triage gate

Triage the whole list first. Then process **exactly one already-previewed root cause
per `go`** in HOME.

A return from Mentor with `Changes required` always enters this gate before any code
is changed. Messages such as:

```text
mentor review ready
mentor done
back from mentor
continue
fix the mentor findings
go
```

do **not** authorize implementation when the new Mentor findings have not yet been
triaged in HOME.

On that first HOME turn:

1. re-read `mentor-review.md`,
2. inspect the current verdict and open findings,
3. classify/merge findings by root cause and return stage,
4. show the queue,
5. preview exactly one next root cause,
6. end with the JARVIS card whose action is `go`.

A bare `go` only authorizes work that JARVIS has already previewed. It never skips the
triage gate.

Routing:

```text
local correction                  → Grunt-work
larger implementation mechanism   → One-by-one
architecture/responsibility       → Blueprint Fork
product behavior                  → Speculation
```

Grunt-work uses the same coding model as HOME. Do not add a planning-model switch.

When the queue originates from Mentor, finishing the fixes does **not** advance to
Polish. JARVIS must return to the existing Mentor chat for verification first.

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

### Returning from Mentor to HOME

A HOME message indicating the Mentor review is ready is an **intake signal**, not
implementation permission and **not** `continue`. Re-read `mentor-review.md` before
deciding anything. End the turn with **one** transition card only — see **Stage
advancement (strict)**.

Handle the verdict explicitly:

```text
Ready for commits           → show clean-review transition; next is Polish
Changes required            → triage findings first; no code on this HOME turn
Blocked by design decision  → route to the earliest responsible design stage
Blocked by product decision → route to Speculation
```

For `Changes required`, JARVIS routes each finding to the smallest responsible stage.
Grunt-work findings obey the mandatory triage gate and one-root-cause-per-`go`
contract. Findings routed to One-by-one, Blueprint, Refine, or Eye-candy obey that
stage's normal authorization boundary.

### Verification loop

After **any semantic Mentor fix**, verification is required before SHIP. Reuse the
same dedicated Mentor chat when convenient; it already knows the findings but still
has never written production code.

When all routed fixes are complete, stop in HOME and show the **Mentor fixes complete**
transition card. Do not run Polish yet.

Only a current clean verification verdict:

```text
Ready for commits
```

allows the pipeline to enter Polish.

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

Polish does **not** own commit grouping, commit message suggestions, staging commands,
or Git-story planning. Those belong to Ready-commits.

### Mentor validity invariant

**Any semantic code change invalidates the current Mentor verdict.**

A semantic change includes behavior, meaningful control flow, state ownership,
component/hook architecture, API/data contracts, or non-trivial error behavior.

Therefore Polish may not silently make such a fix. It must return control to JARVIS,
which routes the issue, then Mentor verification runs again.

Pure formatting/import/mechanical cleanup does not require another Mentor pass.

## Stage 9 — Ready-commits

Plan-only. Never stage or commit.

Suggested commit messages follow the Ready-commits skill **Commit message rules** — no
automatic team hashtags or `#padkrapao`; the user adds tags when committing.

Before commit groups, show a compact final code ownership overview:

| Code group | Owns | Why it changed |
|---|---|---|
| ... | ... | ... |

Then group changes into coherent commits and explain the purpose of each group.
Files should be presented as related code stories, not a raw file dump.

User commits manually.

## Stage 10 — Dev-handoff

Runs **after** manual commits from Ready-commits (or after explicit user skip). Not
part of Mentor intake and not bundled with Polish / Ready-commits.

Artifact:

```text
work/{work-id}/dev-handoff.md
```

Use final code and actual commits as source of truth.

The handoff must preserve the feature's runtime flow, responsibility map, key
decisions, review order, verification, and important follow-ups in concise English.

When a prior One-by-one/Tour ownership checkpoint exists, Dev-handoff surfaces only
meaningful changes to that mental model instead of repeating the walkthrough.

In chat, Dev-handoff always gives a copy-paste PR title and description.

Dev-handoff also creates a ready-to-paste prompt for a single 16:9
presentation-style technical knowledge slide: problem, core idea, runtime flow,
ownership, and the most useful remember/debug cues. It is a visual cheat sheet, not
UML and not a code dump.

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
Advance **exactly one** pipeline stage — the step named in **Next** on the **last**
JARVIS transition card (mirrored in `jarvis-state.md`). It never skips a newly required
Mentor-finding triage.

It does **not** apply to Mentor intake phrases ("mentor done", "verification clean",
etc.). Those require intake handling only, then a new card (often **Next: Polish**,
**Action: `continue`** on a **later** message).

After Polish completes, stop and show the Ready-commits transition; do not run
Ready-commits in the same turn as Polish unless the user explicitly asked for both.

### `go`
Authorize exactly one **already-previewed** One-by-one chunk, Grunt-work root cause,
or Eye-candy cleanup block. A bare `go` never skips required triage/intake.

### `tour`
Run the separate read-only Tour specialist over the smallest useful current scope.
Do not advance or change the active JARVIS stage. After Tour, restore the same
transition/action that was active before it.

### `tour step`
Run Tour in interactive mode; one meaningful stop per `continue`. Preserve JARVIS
workflow state.

### `map`
Optional visual/text dependency map through Tour. Mermaid only if the user explicitly
wants it.

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
2. read `work/{work-id}/jarvis-state.md`, then inspect the other artifacts,
3. inspect Git state,
4. reconstruct the smallest reliable status,
5. show the JARVIS transition card.

Artifact existence alone does not prove completion.

The normal forward order remains:

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

Mentor findings create a **review loop**, not a new forward stage:

```text
Mentor-me · Changes required
→ HOME triage
→ routed fixes
→ Mentor-me verification
→ repeat until Ready for commits
→ Polish
```

Never forget Refine or swap Refine/Eye-candy/Mentor-me. Never skip Mentor verification
after semantic review fixes.

---

# Golden rules

1. JARVIS alone owns workflow order.
2. Specialists stop when their stage is done; they do not choose or start the next stage.
3. The JARVIS transition card is always the **last rendered block**; nothing follows it.
4. Every HOME response in an active task ends with the card, including side questions.
5. Only three session types exist: HOME, Blueprint Fork, Mentor Clean Chat.
6. Grunt-work stays HOME with the coding model.
7. Returning from Mentor never authorizes code; HOME reads the verdict and triages first.
8. `go` authorizes only one already-previewed work item; it never skips triage.
9. Semantic Mentor fixes always return to Mentor verification before Polish.
10. One-by-one teaches every chunk immediately with its fixed chunk message template.
11. Tour is a separate read-only utility and never advances workflow state.
12. No Mermaid by default.
13. Refine uses a behavior lock before risky reductions.
14. Any later semantic change invalidates the Mentor verdict.
15. Polish is mechanical only and never plans commits.
16. Ready-commits owns commit grouping/message suggestions and includes final ownership.
17. Dev-handoff includes the final understanding delta and visual feature brief.
18. Persistent workflow artifacts are English; chat is adaptive.
19. The goal is strong code **and** strong developer ownership, without workflow ceremony.
20. **One user message → one forward stage**; never batch SHIP unless the user names it.
21. Mentor intake reports are not `continue`; HOME shows one card, then waits.
22. HOME does not rewrite Mentor verification verdicts; Mentor Clean Chat owns that.
23. Polish, Ready-commits, and Dev-handoff are three separate turns (three `continue`s).
24. Conversation summaries and handoff prompts must not override **Stage advancement (strict)**.
25. `jarvis-state.md` is re-read at the start and updated at the end of every HOME turn.

---
name: jarvis
description: >-
  Orchestrates the JARVIS developer workflow: a read-only context tour before shaping,
  one stable stage order, three session types, one step per user message, explicit
  skips, and a transition card closing every HOME reply. Use at the start of a task or
  when the user says jarvis, status, continue, weiter, go, skip, tour, back, stop, or
  asks what happens next.
---

# JARVIS — Developer Workflow Orchestrator

JARVIS is the **single source of truth for workflow order**. Specialists own how their
stage works; they never choose or start the next stage.

```text
(pre)    Context Tour — read-only preflight, not a phase
SHAPE    Speculation → Blueprint
BUILD    One-by-one → Manual Try-out → optional Grunt-work
CLEAN    Refine → Eye-candy
REVIEW   Mentor-me (loop until Ready for commits)
SHIP     Polish → Ready-commits → manual commits → Dev-handoff
```

## Turn contract

Applies to **every HOME response** while a JARVIS task is active.

1. **Re-anchor.** Re-read `work/{work-id}/jarvis-state.md`. If these JARVIS rules are
   no longer fully in context (long chat, summarized history), re-read this skill first.
2. **Do only what is authorized.** `go` runs the one previewed item. `continue` runs
   the one step named in **Next** on the last card / state file. Anything else — a
   question, a remark, a Mentor report, a Tour request — is handled without advancing
   the pipeline. Conversation summaries and handoff prompts never authorize steps.
3. **Stop after one step.** When a stage or work item completes in this response, stop.
   Never start the next stage, even when it looks trivial or obvious. There is no
   multi-stage exception: `run polish, then ready-commits` runs only the first valid
   step.
4. **Close.** Update `jarvis-state.md`, then end with the JARVIS card as the final
   block — also after side questions (repeat the current card unchanged).

One step is: one One-by-one chunk, one Grunt-work root cause, one Eye-candy block, one
triage/intake turn, one skip, or one whole stage such as Speculation, Refine, Polish,
Ready-commits, or Dev-handoff.

## Skip contract

`skip` passes **exactly one** current step without running it — also when the card
did not offer it. `skip the rest` or `skip refine and eye-candy` skips only the first.
Accept a skip only when the next stage still has reliable input; otherwise say why
briefly and keep the current card. Then update state and show the next card.

Never skippable: the initial Mentor review, Mentor verification after semantic fixes,
and the clean-verdict requirement before Polish. Manual commits may be skipped.

## Language

Skill instructions and all artifacts under `work/{work-id}/` are English. Chat follows
the user's current language and switches when they switch between German and English.
Code, identifiers, paths, APIs, and domain terms follow repository conventions.

---

# Preflight — Context Tour

A new task starts with the read-only `tour` utility in its pre-Speculation mode, so
developer and agent begin from the real code instead of assumptions. The `tour` skill
owns how. Skip it only when the user says so (`skip tour`). Afterwards the card points
to Speculation.

Later, `tour` is a side utility at any point: it never changes the current stage or
the offered action.

---

# Sessions

| Session               | Runs                                                                                                           | Model                               |
| --------------------- | -------------------------------------------------------------------------------------------------------------- | ----------------------------------- |
| **HOME CHAT**         | Tour + everything except Blueprint and Mentor-me                                                               | one capable coding model throughout |
| **BLUEPRINT FORK**    | Blueprint only — a fork of HOME after Spec approval, planner-only                                              | strongest planning model            |
| **MENTOR CLEAN CHAT** | Mentor-me — a brand-new chat that rebuilds understanding from artifacts and code only; reused for verification | strongest review model              |

There are no other sessions. Grunt-work stays in HOME with the coding model.

---

# Stages

0. **Context Tour** · HOME, read-only preflight — not a phase (see Preflight).
1. **Speculation** · HOME → `spec.md`. Done when the user approves the Spec.
   Card: Blueprint fork boundary.
2. **Blueprint** · BLUEPRINT FORK → `blueprint.md`. Planner-only. Back in HOME,
   `continue` makes JARVIS re-read `blueprint.md` and One-by-one show the
   implementation overview + Chunk 1 preview. `continue` never authorizes Chunk 1
   code; only `go` does.
3. **One-by-one** · HOME. One previewed chunk per `go`, using its fixed chunk message
   template — JARVIS does not restate, shorten, or vary it. Ends with feature flow,
   ownership checkpoint, and manual try-out list.
4. **Manual Try-out** · HOME, no specialist. The developer tries the scenarios, then
   reports remarks (→ Grunt-work), says it looks clean, continues without testing, or
   skips the checkpoint.
5. **Grunt-work** (optional) · HOME → `grunt-work.md`. Triage the whole queue first,
   then one previewed root cause per `go`.
6. **Refine** · HOME. Behavior-preserving reduction.
7. **Eye-candy** · HOME. Readability, one block per `go`. Card: Mentor boundary.
8. **Mentor-me** · MENTOR CLEAN CHAT → `mentor-review.md`. Read-only. See Mentor loop.
9. **Polish** · HOME. Only with a current clean verdict; mechanical only.
10. **Ready-commits** · HOME. Plan-only; never touches Git. Card: manual Git boundary.
11. **Dev-handoff** · HOME → `dev-handoff.md` + PR title/description in chat. Only
    after the user committed (or explicitly skips commits). Then one short completion
    recap and the task-done card.

## Mentor loop

Any HOME message about the review ("mentor done", "verification clean", "back from
mentor" — even `continue` or `go`) is an **intake turn**:

1. re-read `mentor-review.md`,
2. act on the verdict,
3. end with one card — no code and no SHIP stage in this turn.

```text
Ready for commits           → card: Next Polish, Action continue
Changes required            → triage: route findings, show queue, preview first fix
Blocked by design decision  → earliest responsible design stage
Blocked by product decision → Speculation
```

Route each finding by responsibility (see Backtracking); each routed fix keeps its
stage's normal `go` boundary. When all fixes are done, do not run Polish — show the
fixes-complete card pointing to verification in the existing Mentor chat. Repeat until
the verdict is `Ready for commits`.

HOME never rewrites Mentor verdicts; the Mentor chat owns `mentor-review.md`.

**Any semantic change after a clean verdict invalidates it**: behavior, meaningful
control flow, state ownership, component/hook architecture, API/data contracts, or
non-trivial error handling. Such changes go back through the loop. Pure formatting,
import, or mechanical cleanup does not.

## Backtracking

Return to the earliest stage whose responsibility became invalid. Do not patch around
a wrong earlier decision.

```text
product rule                  → Speculation
architecture/responsibility   → Blueprint Fork
larger implementation         → One-by-one
local correction              → Grunt-work
generated residue/reduction   → Refine
readability/structure         → Eye-candy
mechanical cleanup            → Polish
```

## Checks

Focused tests and type/compile checks around changed contracts are welcome. No
browser/manual smoke tests by default, no repeated broad lint/Prettier (Polish owns
that), no full-repo checks just because a chunk completed.

---

# Transition card

## Final block

The card is the **final rendered block** of every HOME response in an active task.
Nothing follows it — no completion sentence, note, suggestion, or reminder. All stage
output comes before it. JARVIS alone renders the card; a specialist's completion
contract is a stop signal, not a footer.

## Format

```text
──────────────── JARVIS ────────────────
✓ SHAPE  ▶ BUILD  · CLEAN  · REVIEW  · SHIP
Stage:   One-by-one · Chunk 2/4 done
Next:    Chunk 3 · Persist draft on blur
Session: SAME CHAT · coding model
Action:  go
```

- **Header** — always identical.
- **Phase rail** — all five phases in fixed order. `✓` done, `▶` the phase of the
  Next step, `·` pending. During preflight, all five remain pending.
- **Stage** — current stage and position (chunk n/m, fix n/m). In the Mentor loop,
  name loop and round, e.g. `Mentor loop · round 2 · fix 1/2 done`.
- **Next** — the single next step.
- **Session** — `SAME CHAT`, `FORK THIS CHAT`, `NEW CLEAN CHAT`, or
  `MENTOR CLEAN CHAT · existing`, plus the model.
- **Action** — exactly what the user types or does.
- **Skip** — optional; render only when JARVIS already knows the offered step is safe
  to skip.
- **Return** — only at session boundaries: where to come back and when.

Rendering rules:

- The card is its own fenced `text` block: blank line before, opening and closing
  fence each on their own line.
- Never inside a list, quote, table, or another code block. Close every earlier code
  block first.
- No backticks, bold, links, or other markdown inside the card.
- Each line under ~60 characters; shorten Next rather than wrapping.
- Copy-paste prompts never go inside the card. Put them in their own `text` block
  directly above it, introduced by one short sentence.

Session boundary example — prompt block, then card:

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

## Transitions

| After         | Stage                                    | Next                             | Session                                     | Action                                     |
| ------------- | ---------------------------------------- | -------------------------------- | ------------------------------------------- | ------------------------------------------ |
| Context Tour  | Context Tour complete (or skipped)       | Speculation                      | SAME CHAT · coding model                    | continue                                   |
| chunk         | One-by-one · Chunk n/m done              | Chunk n+1 · title                | SAME CHAT · coding model                    | go                                         |
| Spec approved | Speculation complete                     | Blueprint                        | FORK THIS CHAT · strongest planning model   | fork, switch model, paste the prompt above |
| Eye-candy     | Eye-candy complete                       | Mentor-me initial review         | NEW CLEAN CHAT · strongest review model     | open a new chat, paste the prompt above    |
| triage        | Mentor loop · round n · findings triaged | Fix 1/k · MM-00x title           | SAME CHAT · coding model                    | go                                         |
| last fix      | Mentor loop · round n · fixes k/k done   | Mentor-me verification           | MENTOR CLEAN CHAT · existing · review model | paste the prompt above in the Mentor chat  |
| clean verdict | Mentor verified · Ready for commits      | Polish                           | SAME CHAT · coding model                    | continue                                   |
| Polish        | Polish complete                          | Ready-commits                    | SAME CHAT · coding model                    | continue                                   |
| Ready-commits | Ready-commits complete                   | manual commits, then Dev-handoff | SAME CHAT                                   | commit the groups, then continue           |
| Dev-handoff   | Dev-handoff complete                     | nothing · task done              | SAME CHAT                                   | open the PR                                |

Start prompts for the session boundaries:

- Blueprint — `Fork done. Strongest model selected. Run Blueprint for work/<id>/spec.md.`
- Mentor — `Run Mentor-me initial review for work/<id>. Read-only review.`
- Verification — `Verify the open findings for work/<id> against current code. Read-only.`

Session-boundary cards add `Return: here when <artifact> is ready` (or `updated`).

---

# Artifacts

Choose one `{work-id}` and reuse it for the whole task.

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

Durable system/domain knowledge stays in its normal repo location (`CONTEXT.md`,
`CONTEXT-MAP.md`, `docs/adr/**`).

`work/{work-id}/` is workflow state, not product code: it stays out of product commits
(usually via ignore rules) unless the repo explicitly versions it.

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

Keep it to these lines — not a log. If the state file, the last card, and Git/artifact
reality disagree, say so, reconstruct the smallest reliable status, and show the card;
do not guess forward.

## Specialists

Locate by frontmatter `name:`, not by editor path: `speculation`, `blueprint`,
`one-by-one`, `grunt-work`, `refine`, `eye-candy`, `mentor-me`, `polish`,
`ready-commits`, `dev-handoff` — plus the read-only utility `tour`.

---

# Commands

- **`jarvis`** — start or recover. On a new task, run the Context Tour first unless
  the user explicitly skips it.
- **`status`** — phase, stage, work-id, artifact state, and the card.
- **`continue` / `weiter`** — run the one step in **Next** on the last card. A Mentor
  report is intake, not `continue`.
- **`go`** — run exactly one already-previewed chunk, Grunt-work root cause, or
  Eye-candy block. Never skips triage or intake.
- **`skip`** — skip exactly one current step (see Skip contract).
- **`tour`** / **`tour step`** — read-only Tour (compact / one stop per `continue`);
  afterwards restore the same card.
- **`back`** — return to the appropriate earlier stage (see Backtracking).
- **`stop`** — stop modifications and show the card.

# Resume

On a new HOME session:

1. identify `{work-id}`,
2. read `jarvis-state.md`, then the other artifacts,
3. inspect Git state,
4. reconstruct the smallest reliable status,
5. show the card.

Artifact existence alone does not prove completion.

# Golden rules

1. JARVIS alone owns workflow order; specialists stop when their stage is done.
2. One user message → at most one step (a run, a skip, or an intake); no exceptions.
3. New tasks begin with a read-only Context Tour unless the user skips it.
4. Every HOME reply in an active task ends with the card; nothing follows it.
5. `go` runs only an already-previewed item; `continue` only the card's Next.
6. Returning from Mentor is intake — read, triage, card — never code.
7. Semantic changes go back through Mentor verification before Polish.
8. `jarvis-state.md` is re-read at the start and updated at the end of every turn.
9. Artifacts are English, chat is adaptive; no Mermaid unless asked.

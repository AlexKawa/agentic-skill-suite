# JARVIS

A developer-owned workflow for building software with AI agents without giving up understanding, review quality, or control.

JARVIS turns a well-scoped task into a structured development flow: clarify the behavior, design against the real repository, implement in understandable chunks, reduce unnecessary generated complexity, review independently, polish the final diff, and leave behind a useful developer handoff.

It is intentionally **not** a fully autonomous coding loop. The agent handles exploration, implementation effort, and mechanical work; the developer stays involved where decisions, understanding, and long-term ownership matter.

## Workflow

JARVIS keeps the full workflow, but groups it into five easy-to-remember phases:

```text
(pre)
Context Tour — read-only orientation, not a phase

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

### Context Tour

A new task starts with a read-only **Tour** of the code the task touches, so developer and agent begin from the real runtime and ownership boundaries instead of assumptions. It can be skipped (`skip tour`) and used again at any point as a recap (`tour`, `tour step`) without changing the workflow position.

### SHAPE

**Speculation** clarifies what should become true: intent, scope, behavior, edge cases, acceptance criteria, and regression boundaries.

**Blueprint** turns the approved Spec into the smallest coherent implementation design supported by the actual codebase. It looks for existing patterns and reuse before proposing new code.

### BUILD

**One-by-one** implements the Blueprint one coherent chunk at a time. Each chunk is explained through the runtime and responsibility flow rather than only through a list of changed files.

After implementation, the developer gets a small **Manual Try-out** checklist instead of automatic browser smoke tests.

**Grunt-work** is optional and handles concrete issues found while trying the feature. It triages the full remark list, but fixes only one local root cause per `go`. Anything larger is escalated back to the appropriate JARVIS stage.

### CLEAN

**Refine** removes implementation residue: redundant state, unnecessary abstractions, duplicate transformations, dead code, avoidable wrappers, and missed reuse.

**Eye-candy** then improves the readability of the reduced implementation: clearer component and hook boundaries, calmer JSX, named predicates, and easier navigation without changing behavior.

### REVIEW

**Mentor-me** performs an independent read-only review of the final post-cleanup implementation against the Spec, Blueprint, repository rules, tests, and actual code.

It looks for correctness issues, architectural drift, security problems, lifecycle/race bugs, unnecessary complexity, and missing regression protection.

### SHIP

**Polish** owns the final mechanical cleanup of the changed files: formatting, lint, imports, relevant static-quality rules, and other safe non-semantic fixes.

**Ready-commits** turns the reviewed diff into coherent commit stories without staging or committing anything.

The developer commits manually.

**Dev-handoff** writes the final concise mental model of the branch: runtime flow, ownership boundaries, important decisions, review order, verification, and relevant follow-ups. In chat it also gives a copy-paste PR title and description.

## Session model

JARVIS deliberately keeps chat/session rules simple:

| Session | Runs | Model |
|---|---|---|
| **Home chat** | everything except Blueprint and Mentor-me | one capable coding model |
| **Blueprint fork** | Blueprint only; the fork keeps the problem context but never implements | strongest planning model |
| **Mentor clean chat** | Mentor-me; a brand-new chat without implementation-chat anchoring, rebuilt from artifacts, code, tests, and repo rules; reused to verify fixes | strongest review model |

## One source of truth

Only the JARVIS orchestrator owns workflow order.

Specialist skills know how to perform their stage, but they do not decide what stage comes next. When they finish, control returns to JARVIS.

This prevents different skills from gradually drifting into conflicting pipeline orders.

At workflow boundaries, JARVIS uses a consistent transition card:

```text
──────────────── JARVIS ────────────────
✓ SHAPE  ▶ BUILD  · CLEAN  · REVIEW  · SHIP
Stage:   One-by-one · Chunk 2/4 done
Next:    Chunk 3 · Persist draft on blur
Session: SAME CHAT · coding model
Action:  go
```

The phase rail always shows where the task stands. For a session boundary the card also tells the developer whether to fork, create a new chat, or switch model; the start prompt sits in its own copyable block right above the card.

Every HOME reply in an active task ends with this card, and one user message advances at most one step. A tiny `work/{work-id}/jarvis-state.md` mirrors the last card so the flow survives long or summarized chats.

## Developer ownership

A central goal of JARVIS is that the developer should be able to answer:

1. What changed?
2. How does the feature flow from entry point to result?
3. Which code owns which responsibility?
4. Which important decisions did I participate in?
5. Where would I start debugging this tomorrow?

One-by-one therefore explains code through **runtime and responsibility flow**, not only through diffs.

After every chunk, the developer sees the same fixed message:

```text
Changed files     every file, linked, with a one-line role
In short          3–5 simple sentences explaining the chunk
Important code    1–3 focused excerpts with chat-only teaching comments
Before → after    tiny runtime/behavior flow
Checks            only checks actually run
Next              short preview of the next chunk
```

Technical terms should be explained briefly when they matter instead of assuming the developer already knows every piece of vocabulary.

## Refine safety

Refine may delete and consolidate generated code, which makes it one of the riskiest cleanup stages. Non-trivial reductions therefore use a **Behavior Lock**: name the behavior that must stay true, run focused verification before and after, and drop any reduction that changes observable behavior or can't be verified with reasonable confidence.

Mentor-me runs after Refine and Eye-candy, so the independent review sees the code that is actually intended to ship.

## Mentor validity

A clean Mentor verdict only applies to the code that was reviewed. Any later **semantic change** — behavior, control flow, state ownership, component/hook architecture, API/data contracts, or non-trivial error handling — invalidates it.

Polish is therefore mechanical-only. If it finds a problem that needs a semantic change, JARVIS routes it back to the right stage and Mentor verifies again.

## Task artifacts

Each JARVIS task gets one work directory:

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

Persistent workflow artifacts are written in English.

The user-facing conversation may use whatever language the developer is currently working in.

Durable project/domain documentation remains separate from task-local artifacts, for example:

```text
CONTEXT.md
CONTEXT-MAP.md
docs/adr/**
```

## Commands

The small command set is intentionally predictable:

```text
jarvis      start or recover the workflow
status      show current phase/stage
continue    run the one step named on the last card
go          authorize exactly one implementation/fix/cleanup unit
skip        skip exactly one current step (never Mentor review gates)
tour        read-only code walkthrough; tour step = one stop per continue
back        return to the appropriate earlier stage
stop        stop modifications and show current state
```

## Specialist skills

The `jarvis` skill is the orchestrator; `tour` is a read-only utility; the others are specialists. Install the complete suite together so the orchestrator can hand work to the appropriate specialist.

## Installation

JARVIS is designed to stay as host-independent as practical. Copy the complete suite into the location your editor or coding agent uses for custom skills and preserve the folder structure:

```text
<skill-root>/
├── jarvis/
├── tour/
├── speculation/
├── blueprint/
├── one-by-one/
├── grunt-work/
├── refine/
├── eye-candy/
├── mentor-me/
├── polish/
├── ready-commits/
└── dev-handoff/
```

The orchestrator discovers specialists by their skill `name:` rather than depending on one specific editor path.

## Design principles

### Ownership over blind automation

AI should remove typing and exploration effort, not the developer's understanding of the system.

### Reuse before creation

Existing repository patterns and responsibilities should be understood before introducing new abstractions.

### Small coherent changes

Work should be split where it improves understanding and reviewability, not into arbitrary micro-tasks.

### Working code is not finished code

Implementation proves the solution. Refine and Eye-candy decide what is worth keeping. Mentor-me challenges whether it is actually good enough. Polish prepares the reviewed result for commits.

### Durable context over conversation history

Specs, Blueprints, findings, follow-ups, and handoffs live in the repository so a workflow can survive chat boundaries and be picked up by another developer or agent.

### Quality without ceremony

Every stage should earn its place by improving correctness, understanding, or maintainability. JARVIS should simplify orchestration whenever process starts becoming more expensive than the value it provides.

## Status

JARVIS has gone through several private iterations and real-world usage before its first public release.

The public release history starts with the surrounding Agentic Skill Suite repository.

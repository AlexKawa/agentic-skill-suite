# JARVIS

A developer-owned workflow for building software with AI agents without giving up understanding, review quality, or control.

JARVIS turns a well-scoped task into a structured development flow: clarify the behavior, design against the real repository, implement in understandable chunks, reduce unnecessary generated complexity, review independently, polish the final diff, and leave behind a useful developer handoff.

It is intentionally **not** a fully autonomous coding loop. The agent handles exploration, implementation effort, and mechanical work; the developer stays involved where decisions, understanding, and long-term ownership matter.

## Workflow

JARVIS keeps the full workflow, but groups it into five easy-to-remember phases:

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

**Dev-handoff** writes the final concise mental model of the branch: runtime flow, ownership boundaries, important decisions, review order, verification, and relevant follow-ups.

## Session model

JARVIS deliberately keeps chat/session rules simple.

```text
Blueprint = FORK current chat
Mentor    = NEW CLEAN CHAT
Everything else = SAME HOME CHAT
```

### Home chat

The normal implementation conversation:

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

Use one capable coding model for normal HOME work.

### Blueprint fork

Blueprint is the planning boundary.

Fork the current conversation and switch to the strongest available planning/reasoning model. The fork gets the existing problem context, but remains planner-only and never implements production code.

When `blueprint.md` is ready, return to the HOME chat.

### Mentor clean chat

Initial Mentor-me review starts in a brand-new clean chat with a strong review/reasoning model.

This intentionally removes the implementation conversation's explanations and anchoring. The reviewer reconstructs the feature from durable artifacts, current code, tests, and repository rules.

The same Mentor chat can be reused later to verify findings after fixes.

## One source of truth

Only the JARVIS orchestrator owns workflow order.

Specialist skills know how to perform their stage, but they do not decide what stage comes next. When they finish, control returns to JARVIS.

This prevents different skills from gradually drifting into conflicting pipeline orders.

At workflow boundaries, JARVIS uses a consistent transition card:

```text
──────────────── JARVIS ────────────────
BUILD · One-by-one · Chunk 2/4
Session: SAME CHAT · coding model
Next: Chunk 3
Action: `go`
```

For a session boundary it also tells the developer exactly whether to fork, create a new chat, switch model, and which prompt to start with.

## Developer ownership

A central goal of JARVIS is that the developer should be able to answer:

1. What changed?
2. How does the feature flow from entry point to result?
3. Which code owns which responsibility?
4. Which important decisions did I participate in?
5. Where would I start debugging this tomorrow?

One-by-one therefore explains code through **runtime and responsibility flow**, not only through diffs.

After a typical chunk, the developer sees:

```text
What changed

How this chunk flows
1. Entry point receives the action
2. State/domain owner handles the lifecycle
3. Boundary prepares or validates the data
4. Service/API performs the external work
5. Result returns to the UI

Who owns what
Code | Responsibility | Why changed

Important code
Only the genuinely important functions/hooks

Checks

Next chunk
```

Technical terms should be explained briefly when they matter instead of assuming the developer already knows every piece of vocabulary.

## Code Tour

`tour` is an optional read-only walkthrough for getting more direct contact with the code.

```text
tour
```

It normally gives 3–6 important stops in one response. Each stop identifies the file/symbol, what to look at, what responsibility lives there, and where the flow goes next.

The tour reuses the current implementation context when possible, does not modify code, and does not run additional checks by default.

For a slower walkthrough:

```text
tour step
```

This shows one code stop at a time and waits for `continue`.

Diagrams are optional. Mermaid is not part of the default explanation style.

## Refine safety

Refine is powerful because it is allowed to delete and consolidate generated code. That also makes it one of the highest-risk cleanup stages.

Non-trivial reductions therefore use a **Behavior Lock**:

```text
identify behavior that must remain true
        ↓
identify relevant Spec rule / code path
        ↓
use focused baseline verification when available
        ↓
reduce the implementation
        ↓
run the same relevant verification again
```

If a reduction changes observable behavior or cannot be kept with reasonable confidence, it should not remain.

Mentor-me runs after Refine and Eye-candy, so the independent review sees the code that is actually intended to ship.

## Mentor validity

A clean Mentor verdict only applies to the code that was reviewed.

Any later **semantic code change** invalidates that verdict.

Semantic changes include meaningful changes to:

- behavior,
- control flow,
- state ownership,
- component/hook architecture,
- API or data contracts,
- non-trivial error handling.

Polish is therefore mechanical-only. If it discovers a problem that requires a semantic change, JARVIS routes the issue back to the appropriate stage and Mentor verifies the result again.

## Task artifacts

Each JARVIS task gets one work directory:

```text
work/{work-id}/
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
continue    advance through the current transition
go          authorize exactly one implementation/fix/cleanup unit
tour        compact code walkthrough
tour step   interactive code walkthrough
map         optional dependency/flow visualization
skip        skip an explicitly optional item
back        return to the appropriate earlier stage
stop        stop modifications and show current state
```

## Specialist skills

The JARVIS suite currently contains:

```text
jarvis/
speculation/
blueprint/
one-by-one/
grunt-work/
refine/
eye-candy/
mentor-me/
polish/
ready-commits/
dev-handoff/
```

The `jarvis` skill is the orchestrator. The others are specialists.

Install the complete suite together so the orchestrator can hand work to the appropriate specialist.

## Installation

JARVIS is designed to stay as host-independent as practical.

The exact skill directory depends on the editor or coding agent. Copy the complete JARVIS suite into the location your host uses for custom skills/instructions and preserve the folder structure.

Conceptually:

```text
<skill-root>/
├── jarvis/
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

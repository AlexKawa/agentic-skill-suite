# Agentic Skill Suite

A collection of reusable skills for agentic coding workflows, focused on developer ownership and code quality.

## Why this exists

AI coding agents can produce strong code quickly, but speed alone does not guarantee understanding, maintainability, or good engineering decisions.

This repository collects opinionated skills and workflows designed to make agent-assisted development more deliberate. The goal is not maximum automation. The goal is to keep the developer in control while still benefiting from the speed and breadth of modern coding agents.

The skills focus on a few recurring principles:

- understand the problem before implementation,
- make important decisions explicit,
- work in small, reviewable units,
- keep code ownership with the developer,
- reduce unnecessary AI-generated complexity,
- review the result critically before shipping,
- leave behind useful context for the next developer or session.

## Skills

### JARVIS

JARVIS is a structured developer workflow for taking a well-scoped task from idea to implementation and handoff.

It combines product clarification, repository-aware planning, incremental implementation, code ownership, cleanup, independent review, mechanical polish, commit preparation, and developer handoff.

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
Polish → Ready-commits → Dev-handoff
```

JARVIS is intentionally not a fully autonomous coding loop. It uses explicit checkpoints where developer understanding or ownership matters, while avoiding unnecessary approvals for routine mechanical work.

See [`skills/jarvis/`](skills/jarvis/) for the full suite.

## Repository structure

```text
agentic-skill-suite/
├── skills/
│   ├── jarvis/
│   │   ├── README.md
│   │   ├── jarvis/
│   │   ├── speculation/
│   │   ├── blueprint/
│   │   ├── one-by-one/
│   │   ├── grunt-work/
│   │   ├── refine/
│   │   ├── eye-candy/
│   │   ├── mentor-me/
│   │   ├── polish/
│   │   ├── ready-commits/
│   │   └── dev-handoff/
│
├── README.md
├── CHANGELOG.md
└── LICENSE
```

Each skill is kept as a small, portable set of Markdown instructions and supporting references. The repository structure intentionally keeps larger workflows such as JARVIS grouped together while still allowing individual skills to evolve independently.

## Using the skills

The exact installation location depends on the coding agent or editor you use.

In general, copy the relevant skill folders into the location your agent uses for custom skills or instructions. Keep the folder structure intact when a workflow contains multiple cooperating skills.

For JARVIS, install the complete `skills/jarvis/` suite rather than only the orchestrator. JARVIS coordinates the workflow, while the specialist skills define the behavior of each stage.

## Language convention

The skill source and persistent workflow artifacts are written in English.

User-facing conversations are independent from artifact language and may naturally use another language. Code, identifiers, repository paths, APIs, package names, and canonical domain terms follow the conventions of the target repository.

## Design philosophy

### Developer ownership over blind automation

The agent should do the mechanical work, repository exploration, and implementation effort that computers are good at. The developer should remain involved where understanding, trade-offs, or long-term ownership matter.

### Durable artifacts over chat memory

Important decisions and handoffs should survive the conversation that produced them. Specs, Blueprints, reviews, issue maps, and developer handoffs are therefore persisted as repository artifacts instead of relying on chat history.

### Small coherent units over giant code drops

Implementation is easier to understand and review when it happens in meaningful chunks with clear responsibilities. The workflows deliberately avoid both huge autonomous changes and excessive micro-tasks.

### Quality after generation

Working code is not automatically finished code. JARVIS separates implementation from reduction, readability cleanup, independent review, and mechanical polish so that generated code is challenged before it is shipped.

### Simple orchestration

The workflows should add structure without becoming bureaucracy. When a rule or stage no longer improves understanding, safety, or quality, it should be simplified.

## Status

This repository starts its public history at **v1.0.0**.

The workflows have gone through several private iterations and real-world usage before the first public release. Future public changes are documented in [`CHANGELOG.md`](CHANGELOG.md).

## License

MIT.

You are free to use, modify, and redistribute the skills under the terms of the license.

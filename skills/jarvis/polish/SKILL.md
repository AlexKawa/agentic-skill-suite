---
name: polish
description: >-
  Consolidated changed-files-only mechanical cleanup after a current clean Mentor
  verdict. Formats, lints, fixes safe warnings/imports/i18n/static issues, and stops
  instead of making semantic changes that would invalidate review.
---

# Polish — Mechanical Readiness Only

## Language policy

Skill instructions and all persistent workflow artifacts are written in English.

Persistent artifacts include Specs, Blueprints, follow-ups, Grunt-work queues,
Mentor reviews, Dev handoffs, and other repository-persisted workflow documents.

User-facing chat is independent from artifact language. Follow the user's current
working language and switch naturally when the user switches between German and
English.

Code, identifiers, repository paths, APIs, package names, and canonical domain terms
follow repository conventions.


## Entry gate

A current Mentor verdict must be:

```text
Ready for commits
```

If semantic code changed after that verdict, Mentor verification is stale and Polish
must not pretend it is current.

## Scope

Current changed files only, plus directly required companion files such as locale
entries.

Own:

- Prettier,
- ESLint safe fixes,
- warnings/errors,
- imports,
- safe local type cleanup,
- relevant i18n/static-quality rules,
- changed-file-only mechanical cleanup.

Do not run whole-repo fixers by default.

## Semantic-change guard

**Any semantic code change invalidates the Mentor verdict.**

Polish must not silently perform changes to:

- behavior,
- meaningful control flow,
- state ownership,
- component/hook architecture,
- API/data contracts,
- non-trivial error semantics,
- architecture boundaries.

If a quality rule requires such a change:

1. stop,
2. report the concrete issue,
3. return control to JARVIS,
4. let JARVIS route the fix,
5. require Mentor verification afterward.

Pure formatting/import/mechanical cleanup does not require Mentor re-review.

## Output

```text
Pipeline-ready for changed files
```

or a concrete blocker.

List:

- files in scope,
- safe fixes performed,
- checks,
- any verification limitation.

## Completion contract

Say:

> Polish complete. Return control to JARVIS.

Do not select the next workflow stage.

---
name: ready-commits
description: >-
  Plan-only commit preparation after clean Mentor + Polish. Presents a compact final
  code ownership overview, groups related files into coherent review stories, and
  never mutates Git.
---

# Ready-commits — Organize the Final Code Story

## Preconditions

Expected in the full pipeline:

```text
Mentor-me: Ready for commits
Polish: Pipeline-ready for changed files
```

If not current, return control to JARVIS instead of calling the branch ready.

## Git rule

Read-only Git only.

Never run:

```text
git add
git commit
git reset
git restore
git checkout
git stash
```

Commands shown for staging are instructions for the user.

## First: final ownership overview

Before commit chunks, show a compact map of related code:

| Code group | Responsibility | Why it changed |
|---|---|---|
| `...` | ... | ... |

Group files by runtime/responsibility story, not directories.

Then show a short front-to-back feature flow when useful:

```text
UI entry → state/domain owner → request/service boundary → result
```

No Mermaid by default.

This is the final ownership checkpoint before commits.

## Commit plan

Each commit should answer one review question.

For each chunk include:

### Chunk N — title

**Purpose**
1–2 sentences: what coherent behavior/responsibility this commit establishes.

**Code ownership**
Which files belong together and what each contributes.

**Files**
- `...`

**Hunk split**
Only when a file genuinely contains separable stories.

**Stage — user runs**
```bash
git add ...
```

**Message option A**
...

**Message option B**
...

Exactly two useful message options.

Account for every changed/untracked file either in a chunk, hunk-split plan, or
left-unstaged section.

## Optional `tour`

If the user says `tour` here, provide the same compact final 3–6-stop code tour without
changing the commit plan or code.

## Completion contract

Say:

> Ready-commits complete. Return control to JARVIS.

Do not commit and do not select the next workflow stage.

## Suite convention

Persistent workflow artifacts are written in English.
User-facing chat follows the user's current working language.

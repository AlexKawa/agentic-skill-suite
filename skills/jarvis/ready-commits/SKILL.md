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

Split **granularly**: one commit per smallest coherent change that answers one review
question — often 1–3 files. Tests go in the same commit as the code they test. Do not
bundle unrelated stories just to reduce the number of commits.

### Whole-file rule

Every file appears in **exactly one** commit. Never split a file's hunks across
commits and never suggest `git add -p`.

If two stories touch the same file, merge them into one commit with all their files,
and say in **Purpose** that both stories live there.

### Commit shape

For each commit include:

````markdown
### Commit N/M — <title>

**Purpose**
1–2 sentences: what coherent behavior/responsibility this commit establishes.

**Files**
- [`path/to/file.ts`](path/to/file.ts) — what it contributes

**Stage**
```bash
git add path/to/file.ts path/to/file.test.ts
```

**Message A**
```text
feat(magic-chat): keep the chat input stable while streaming

Compare streamed items by content key in useChatInput and reuse the
previous array when nothing changed, so the input stops re-rendering.
```

**Message B**
```text
fix(magic-chat): stop input flicker during streamed replies

Stream ticks created a new items array each time; useChatInput now
reuses the previous value when the content key is unchanged.
```
````

The `git add` command and both messages always sit in their own fenced blocks so the
user can copy them. Message blocks contain only the message; the user runs
`git commit` themselves.

### Commit message rules

- Always `type(scope): subject`. Never omit the scope.
- **Type**: conventional (`feat`, `fix`, `refactor`, `test`, `docs`, `style`, `chore`, …).
- **Scope**: the area this commit touches, in kebab-case (for example `magic-chat`,
  `chat-api`, `i18n`). Reuse scopes that already appear in the repo's `git log` when
  they fit.
- **Subject**: simple words, says what changes for the reader, imperative, lowercase
  start, no trailing period, whole line ≤ 72 characters.
- **Body**: 1–2 short lines (wrapped around 72 characters) of technical context —
  which mechanism/symbol changed and why — so the first glance already explains the
  commit.
- Exactly two options per commit that differ in wording/angle, not just punctuation.
- **Do not** append team tags, hashtags, or ticket footers (for example `#padkrapao`,
  `#linear-…`) to suggested messages. The user adds those manually when they want them.
- Do not copy repo `AGENTS.md` commit-tag conventions into Ready-commits output unless
  the user explicitly asks for tagged messages in that session.

Account for every changed/untracked file either in a commit or in a left-unstaged
section.


## Completion contract

After the complete commit plan, stop. JARVIS closes the response with its card and
waits for the user.

Do not commit and do not start or select the next workflow stage — Dev-handoff always
waits for a later message.

When JARVIS is active, Ready-commits content — including all commit message options
and staging instructions — must appear **before** the JARVIS transition card. Nothing
may appear after the card.

## Suite convention

Persistent workflow artifacts are written in English.
User-facing chat follows the user's current working language.

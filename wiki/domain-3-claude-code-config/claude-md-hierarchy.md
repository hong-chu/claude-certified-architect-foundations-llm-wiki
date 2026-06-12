# CLAUDE.md Hierarchy

> Personal rules at user level, team rules at project level, big rules modularized.

## What it is
The scoping and organization of [[claude-code]]'s `CLAUDE.md`
instruction/memory files.

## Why it matters
Put instructions at the wrong scope and teammates miss them (or
personal preferences leak into shared config). A bloated CLAUDE.md
becomes unmaintainable.

## Key ideas
- **Personal rules → `~/.claude/CLAUDE.md`** (user-level, not shared).
- **Team rules → project `CLAUDE.md`** / `.claude/CLAUDE.md`.
- **Big rules → `@import` or `.claude/rules/`** for modularity.
- **Confusing behavior → `/memory`** to see which files are loaded.
- Exam fixes: teammate misses instructions → move from user to
  project level; huge file → `@import` / split; behaves differently
  across sessions → `/memory`.

## Related
- [[path-specific-rules]]
- [[slash-commands-and-skills]]
- [[project-vs-user-scope]]

## Sources
- [[CCA-F Exam Notes]] — task statement 3.1.

## Continue reading
- **Conditional rules** → [[path-specific-rules]]
- **Project vs user, generally** → [[project-vs-user-scope]]

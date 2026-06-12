# Path-Specific Rules

> Conventions that depend on file type/path live in `.claude/rules/` with a `paths` glob.

## What it is
Rules that activate only for matching file paths, loaded conditionally
instead of all the time.

## Why it matters
Not every convention is relevant everywhere. Loading everything always
bloats context; subdirectory `CLAUDE.md` only works when a convention
maps cleanly to one folder.

## Key ideas
- **Path/type-dependent rules → `.claude/rules/` + a `paths` glob**
  (with YAML frontmatter).
- **Universal rules → `CLAUDE.md`.**
- Use path globs when matching files are **spread across the repo**
  (e.g. all test files), where a single subdirectory `CLAUDE.md`
  can't reach.
- The glob **pattern in a rule** is **not** the **Glob tool** —
  same name, different thing.

## Related
- [[claude-md-hierarchy]]
- [[slash-commands-and-skills]]
- [[builtin-tools]]

## Sources
- [[CCA-F Exam Notes]] — task statement 3.3.

## Continue reading
- **The hierarchy** → [[claude-md-hierarchy]]
- **Plan vs direct** → [[plan-mode-vs-direct-execution]]

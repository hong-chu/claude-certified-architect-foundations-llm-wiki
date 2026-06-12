# @import vs path-scoped rules (organize ≠ reduce context)

> `@import` organizes files but still loads everything eagerly. To *reduce* loaded context, use path-scoped `.claude/rules/`.

A subtle Claude Code config trap: both options "break up a giant
CLAUDE.md," but only one actually shrinks what's loaded into context.

| Dimension | `@import` | `.claude/rules/` with path frontmatter |
|---|---|---|
| What it does | Splits content into files, pulled back in via import | Loads a rule **only when an open/edited file matches its `paths` glob** |
| Loading | **Eager** — imported content is always loaded | **Lazy** — loaded only when relevant |
| Reduces context tokens? | **No** | **Yes** |
| Use when | You only need *organization / reuse* | You need to *reduce loaded context by path* |

## Bottom line
If the goal is just to keep a large CLAUDE.md maintainable, `@import` is
fine. If the goal is to **cut context** so irrelevant conventions don't
load, move topic-specific sections into `.claude/rules/<topic>.md` with
`paths:` frontmatter — they load only when a matching file is in play.

Don't confuse three layers (see [[claude-md-hierarchy]],
[[path-specific-rules]]):
- **Always-on** standards → CLAUDE.md
- **Path/file-type-specific** convention → `.claude/rules/` with paths
- **Directory-wide, always** in one folder → that directory's CLAUDE.md

> Note: a rule's `paths` uses **glob *patterns*** in frontmatter — not
> the **Glob *tool***. Same word, different thing.

## Sources
- [[CCA-F Practice Exam (v1).meta|CCA-F Practice Exam (v1)]] — improvement areas 13 & 18.
- [[CCA-F Notes.meta|CCA-F Notes]] — Task Statements 3.1, 3.3.

## Continue reading
- **CLAUDE.md hierarchy** → [[claude-md-hierarchy]]
- **Path-specific rules** → [[path-specific-rules]]
- **Rules vs skills vs commands** → [[slash-commands-and-skills]]

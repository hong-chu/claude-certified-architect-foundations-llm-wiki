# Slash Commands and Skills

> Slash commands are shortcuts; skills are richer reusable workflows with configuration.

## What it is
Two ways to extend [[claude-code]]: **slash commands** (command
shortcuts) and **skills** (packaged workflows defined by a
`SKILL.md`).

## Why it matters
Choosing the right mechanism and scope, and configuring skills
correctly (isolation, tool limits, inputs), is directly tested.

## Key ideas
- **Scope**: team/shared → project `.claude/`; personal →
  `~/.claude/` (commands in `commands/`, skills in `skills/`).
- A **Skill** lives in `.claude/skills/` with a **`SKILL.md`**.
  Frontmatter options:
  - **`context: fork`** — run in isolated context (messy/verbose/
    long-running work; return a clean summary). Not for every skill.
  - **`allowed-tools`** — restrict a risky skill's tools.
  - **`argument-hint`** — prompt for required input.
- Want a personal variant → **copy** the skill; don't modify the
  shared team one.
- Use a Skill (on-demand workflow) rather than `CLAUDE.md` for things
  that should only run when invoked.

## Related
- [[claude-md-hierarchy]]
- [[plan-mode]]
- [[project-vs-user-scope]]

## Sources
- [[CCA-F Exam Notes]] — task statement 3.2.

## Continue reading
- **Where rules live** → [[claude-md-hierarchy]]
- **Project vs user** → [[project-vs-user-scope]]

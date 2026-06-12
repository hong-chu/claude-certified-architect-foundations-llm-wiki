# Project Scope vs User Scope

> Shared/team things go in the project; personal things go in your home config.

This split recurs across CLAUDE.md, slash commands, skills, and MCP
servers. The rule is always the same: **team → project; personal →
user.**

| Artifact | Team / shared (project) | Personal (user) |
|---|---|---|
| Instructions | project `CLAUDE.md`, `.claude/CLAUDE.md` | `~/.claude/CLAUDE.md` |
| Slash commands | `.claude/commands/` | `~/.claude/commands/` |
| Skills | `.claude/skills/` | `~/.claude/skills/` |
| MCP servers | `.mcp.json` (committable) | `~/.claude.json` |
| Secrets | env vars (never commit) | env vars |

## Bottom line
If teammates need it, it goes in the **project** (committable). If
it's a personal preference or experiment, it goes in **user-level**
config. Classic wrong answers: team standards in `~/.claude/CLAUDE.md`
(new teammates miss them); personal experiments in project
`.claude/skills/`; committing API tokens into `.mcp.json` instead of
using env vars.

## Sources
- [[CCA-F Exam Notes]] — task statements 3.1, 3.2, 2.4.

## Continue reading
- **CLAUDE.md hierarchy** → [[claude-md-hierarchy]]
- **MCP config** → [[mcp-configuration]]

# Claude Code

> Anthropic's agentic coding tool — the subject of Domain 3 and the CI/CD scenario.

## Summary
Claude Code is configured for teams through [[claude-md-hierarchy]]
files, [[slash-commands-and-skills]], [[path-specific-rules]], and
[[model-context-protocol]] servers. It supports [[plan-mode]] vs
direct execution and runs non-interactively in CI via `-p`.

## Key facts
- **Configuration files**: project `CLAUDE.md` / `.claude/CLAUDE.md`
  (team), `~/.claude/CLAUDE.md` (personal); `@import` and
  `.claude/rules/` for modular/path-specific rules.
- **Extensions**: slash commands (`commands/`) and skills
  (`skills/SKILL.md`, with `context: fork`, `allowed-tools`,
  `argument-hint`).
- **Built-in tools**: Read, Write, Edit, Bash, Grep, Glob. See
  [[builtin-tools]].
- **Diagnostics**: `/memory` (which instructions loaded), `/compact`
  (shrink context).
- **CI/CD**: `-p`/`--print` non-interactive; JSON output + schema;
  independent review instance. See [[claude-code-cicd]].

## Related
- [[claude-agent-sdk]]
- [[claude-md-hierarchy]]
- [[plan-mode]]

## Sources
- [[CCA-F Exam Notes]] — Domain 3.

## Continue reading
- **Where instructions live** → [[claude-md-hierarchy]]
- **Running it in CI** → [[claude-code-cicd]]

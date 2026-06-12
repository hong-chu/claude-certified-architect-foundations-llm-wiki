# MCP Configuration

> Project config is shared; user config is personal; secrets come from env vars.

## What it is
How and where you configure [[model-context-protocol]] servers, and
how you protect credentials.

## Why it matters
Wrong scope means teammates don't get shared tooling (or personal
experiments leak into the repo), and committed secrets are a security
hole.

## Key ideas
- **Project-level `.mcp.json`** = shared team tooling — committable.
- **User-level `~/.claude.json`** = personal/experimental servers.
- **Secrets via environment variables**, never committed into
  `.mcp.json`.
- **Multiple servers** can be active; Claude discovers all their
  tools at connection time (but too many tools confuse selection).
- Standard integration (Jira, etc.) → **existing community server**;
  team-specific workflow → **custom server**.
- Improve MCP tool descriptions so Claude doesn't fall back to
  built-in `Grep` over a better MCP search.

## Related
- [[mcp-resources-vs-tools]]
- [[model-context-protocol]]
- [[project-vs-user-scope]]

## Sources
- [[CCA-F Exam Notes]] — task statement 2.4.

## Continue reading
- **Tools vs resources** → [[mcp-resources-vs-tools]]
- **Project vs user everywhere** → [[project-vs-user-scope]]

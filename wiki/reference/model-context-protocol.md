# Model Context Protocol (MCP)

> The protocol for connecting Claude to external tools and resources — the core of Domain 2.

## Summary
MCP servers expose **tools** (actions) and **resources** (read-only
catalogs) to Claude. Configuration scope, secret handling, and clear
tool descriptions decide whether Claude picks the right capability.
Claude selects tools mainly by **name + description**.

## Key facts
- **Tools vs resources**: tools = actions Claude calls; resources =
  content/catalogs Claude inspects to avoid blind exploration. See
  [[mcp-resources-vs-tools]].
- **Scope**: project `.mcp.json` (shared, committable) vs user
  `~/.claude.json` (personal). See [[mcp-configuration]].
- **Secrets** via environment variables, never committed.
- **`isError`** flag marks a tool result as an error; pair with
  retryable/non-retryable metadata. See [[structured-tool-errors]].
- Multiple servers can be active; all their tools are discovered at
  connection time.
- Standard integration → existing community server; bespoke workflow
  → custom server.

## Related
- [[tool-interface-design]]
- [[scoped-tool-access]]
- [[claude-code]]

## Sources
- [[CCA-F Exam Notes]] — Domain 2.

## Continue reading
- **Configuring servers** → [[mcp-configuration]]
- **Tools vs resources** → [[mcp-resources-vs-tools]]

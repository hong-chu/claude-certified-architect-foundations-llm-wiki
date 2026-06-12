# MCP Resources vs Tools

> Tools are actions Claude calls; resources are read-only maps Claude inspects.

## What it is
The two things an [[model-context-protocol]] server can expose:
**tools** (callable actions) and **resources** (content/catalogs).

## Why it matters
Exposing the right map as a **resource** reduces wasted exploratory
tool calls — Claude doesn't have to probe blindly to learn what
exists.

## Key ideas
- **Tools = actions** Claude can call (do something).
- **Resources = content/catalogs** Claude can inspect (a map).
- Good resource examples: issue summaries, documentation hierarchy,
  database schemas, available-report lists, API endpoint catalogs,
  data dictionaries.
- Anti-pattern: expose everything **only** as tools when a resource
  catalog would cut down exploration.

## Related
- [[mcp-configuration]]
- [[model-context-protocol]]
- [[tool-interface-design]]

## Sources
- [[CCA-F Exam Notes]] — task statement 2.4.

## Continue reading
- **Configuring servers** → [[mcp-configuration]]
- **Built-in tools** → [[builtin-tools]]

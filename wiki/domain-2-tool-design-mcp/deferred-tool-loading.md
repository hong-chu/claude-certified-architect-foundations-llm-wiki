# Deferred tool loading (Tool Search Tool)

> Many MCP servers? Don't load every tool definition at startup — let Claude search for the tools it needs.

When an enterprise wires up many MCP servers, loading **all** their
tool definitions at session start bloats the context window and
*degrades tool selection* — the same "too many tools" failure as
[[scoped-tool-access]], but caused by *discovery* rather than *role*.

## The fix
Use the **Tool Search Tool** with **`defer_loading: true`**:
- Tool definitions are **not** all loaded up front.
- Claude first **searches** for relevant tools by intent.
- Only the **matching** tool definitions are loaded into context, on
  demand.

## When to use it
| Situation | Approach |
|---|---|
| Few tools | Normal loading — just expose them |
| Many tools / many MCP servers (enterprise) | **Tool Search Tool + `defer_loading: true`** |

## Why this is its own task statement (2.6)
It's the *scale* answer that complements two others, and the exam tests
the distinction:
- Too many tools **for one agent's role** → [[scoped-tool-access]].
- Too many tool **definitions loaded at once** → **deferred loading**.
- A tool **isn't being chosen** because its description is weak →
  [[tool-interface-design]] (a description problem, not a loading one).

## Sources
- [[CCA-F Notes.meta|CCA-F Notes]] — Task Statement 2.6.
- [[CCA-F Practice Exam (v1).meta|CCA-F Practice Exam (v1)]] — improvement area 17 ("missed deferred
  tool loading for many MCP tools").

## Continue reading
- **Scope by role** → [[scoped-tool-access]]
- **MCP setup** → [[mcp-configuration]]
- **Tool descriptions** → [[tool-interface-design]]

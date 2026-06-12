# Domain 2 — Tool Design & MCP Integration

> **18% of the exam.** Designing tool interfaces, structured errors, scoped access, and MCP server integration so Claude picks the right tool and recovers well.

Core premise: **Claude chooses tools mainly by tool name + tool
description.** Vague or overlapping descriptions → Claude calls the
wrong tool. See [[model-context-protocol]].

> **How this domain is tested** — the cross-cutting [[01-first-principles|method]] as it bites here:
> - **Constrain / least privilege** — fewer, scoped tools beat more; "add tools" degrades selection (2.1, 2.3). → [[04-does-more-improve-reliability]]
> - **Never collapse states** — *tool failed* ≠ *found nothing* (2.2). → [[01-first-principles]] (E1)
> - **Fix the interface first** — rename/split tools before reaching for classifiers or prompt nudges. → [[02-root-cause-decision-tree]] · traps in [[03-never-right-answers]]

## 2.1 — Designing effective tool interfaces

Tool descriptions are **API documentation for Claude**. Claude does
not automatically know what your internal tool does. See
[[tool-interface-design]].

A good description includes **purpose, input format, examples, edge
cases, and boundaries**. Fixes, in order of preference:
- Confused between similar tools (`get_customer` vs `lookup_order`)
  → **improve descriptions** with boundaries/examples.
- Two overlapping tools (`analyze_content` vs `analyze_document`) →
  **rename / clarify boundaries**, or split by purpose.
- One generic tool overused (`analyze_document` for summarize +
  extract + verify) → **split into purpose-specific tools**.
- If selection is weird, also check **system prompt wording**.

Wrong: add more tools without clarifying; reach for a routing
classifier *first*; fine-tune the model; consolidate everything into
one generic tool; vague "choose the correct tool" instructions.

## 2.2 — Structured error responses for MCP tools

Generic errors make agents dumb. A tool error should answer:
**What failed? Why? Is it retryable? What next? Are there partial
results?** See [[structured-tool-errors]].

- MCP's **`isError`** flag tells the agent a tool result is an error.
- **Retryable vs non-retryable** metadata prevents wasted retries
  (retryable = might work next time; non-retryable = don't bother).
- **Business-rule violations** → `retriable: false` + a
  customer-friendly explanation.
- In multi-agent systems, subagents should **recover locally** for
  transient failures and propagate only unresolved errors **with
  partial results + what was attempted**.
- **Empty result ≠ error** — distinguish access failure from a valid
  "found nothing." See [[access-failure-vs-empty-result]].

Wrong: return generic "Operation failed"; retry everything; never
retry; treat empty results as failures; hide partial results;
terminate the whole workflow on one subagent timeout.

## 2.3 — Scoped tool access

**More tools ≠ better.** Too many available tools **degrades
tool-selection reliability** by increasing decision complexity.
**Agent role determines tool access.** See [[scoped-tool-access]].

- Restrict each subagent's tools to its role → prevents
  **cross-specialization misuse** (e.g. a synthesis agent doing web
  searches).
- Replace broad generic tools with constrained role-specific tools.
- Scoped cross-role tools are OK: small frequent need → narrow
  cross-role tool; complex need → go through the coordinator.
- **`tool_choice`**: `auto` (model decides whether to use a tool) /
  `any` (must use *a* tool, model picks) / forced (must call a
  *specific* tool). See [[tool-choice-auto-any-forced]].

## 2.4 — Integrating MCP servers

See [[mcp-configuration]]. MCP servers give Claude access to external
tools/resources; configure at the right scope and protect secrets.

- **Project-level `.mcp.json`** = shared team tooling (committable).
- **User-level `~/.claude.json`** = personal / experimental servers.
- **Secrets** come from **environment variables**, never committed
  into `.mcp.json`.
- Multiple MCP servers can be active; Claude discovers all their
  tools at connection time (but too many tools confuse selection).
- **Resources vs tools**: **tools = actions** Claude can call;
  **resources = read-only catalogs/maps** (issue summaries, doc
  hierarchies, DB schemas, endpoint catalogs) that **reduce
  exploratory tool calls**. See [[mcp-resources-vs-tools]].
- Standard integration (e.g. Jira) → use an **existing community MCP
  server**; team-specific workflow → build a **custom** one.
- Improve MCP tool descriptions so Claude stops preferring built-in
  `Grep` over a more capable MCP search.

## 2.5 — Built-in tools

See [[builtin-tools]]. Use the right one:
- **Grep** = search **text inside** files (function names, errors,
  imports).
- **Glob** = find **files** by path/name pattern / extension.
- **Read** = load file content. **Edit** = targeted change on
  **unique** anchor text. **Write** = create/replace a whole file.
- **Bash** = run commands — use carefully.

Method: **Search → Read → Trace imports/callers → Modify → Test.**
Build understanding incrementally; don't read the whole codebase
upfront. When **Edit fails** (anchor not unique) → Read + Write.

Wrong: Glob to search inside files; Grep to find files by extension;
Edit on non-unique text; Write for a tiny change; Bash before
understanding the repo; modifying before tracing callers.

## 2.6 — Deferred tool loading

When **many** MCP servers are connected, loading **all** their tool
definitions at session start bloats context and degrades selection —
the "too many tools" failure of 2.3, but caused by *discovery* rather
than *role*. Fix: the **Tool Search Tool** with **`defer_loading:
true`** — Claude searches for relevant tools and only the matching
definitions load on demand. See [[deferred-tool-loading]].

- Few tools → load normally.
- Many tools / many MCP servers → **Tool Search Tool + `defer_loading: true`**.

## Continue reading
- **Why descriptions decide everything** → [[tool-interface-design]]
- **Fewer tools, better choices** → [[scoped-tool-access]]
- **Next domain** → [[domain-3-claude-code-config]]

## Sources
- [[CCA-F Exam Notes]] — Domain 2, task statements 2.1–2.5.
- [[CCA-F Notes.meta|CCA-F Notes]] — task statement 2.6 (deferred tool loading).

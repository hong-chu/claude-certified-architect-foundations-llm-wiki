# Built-in Tools (Read, Write, Edit, Bash, Grep, Glob)

> Grep searches inside files; Glob finds files; Read inspects; Edit patches; Write replaces; Bash executes.

## What it is
Claude Code's built-in file/system tools and when to use each.

## Why it matters
Picking the wrong one wastes calls or causes errors (e.g. Edit on
non-unique text). The exam tests crisp tool-to-job matching.

## Key ideas
- **Grep** — search **text inside** files (function names, error
  messages, imports).
- **Glob** — find **files** by path/name pattern or extension.
- **Read** — load file content. **Edit** — targeted change on a
  **unique** anchor. **Write** — create/replace a whole file.
- **Bash** — run commands; use carefully (understand the repo first).
- Method: **Search → Read → Trace imports/callers → Modify → Test.**
  Build understanding incrementally; don't read the whole codebase.
- **Edit fails** when the anchor text isn't unique → fall back to
  Read + Write.

## Related
- [[model-context-protocol]]
- [[large-codebase-context]]
- [[claude-code]]

## Sources
- [[CCA-F Exam Notes]] — task statement 2.5.

## Continue reading
- **Exploring big repos** → [[large-codebase-context]]
- **MCP vs built-in** → [[mcp-resources-vs-tools]]

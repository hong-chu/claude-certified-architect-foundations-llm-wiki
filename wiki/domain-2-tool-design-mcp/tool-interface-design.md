# Tool Interface Design

> Claude picks tools by name + description — so descriptions are API docs for Claude.

## What it is
Designing tool names, descriptions, and boundaries so Claude reliably
selects the right tool. Claude does not know what your internal tool
does unless the description says so.

## Why it matters
Vague or overlapping descriptions are the #1 cause of wrong-tool
selection. The fix is almost always **better descriptions or
splitting tools** — not classifiers or fine-tuning.

## Key ideas
- A good description includes **purpose, input format, examples, edge
  cases, and boundaries**.
- **Similar tools confused** (`get_customer` vs `lookup_order`) →
  clarify descriptions/boundaries.
- **Overlapping tools** (`analyze_content` vs `analyze_document`) →
  rename / clarify, or split by purpose.
- **One generic tool overused** → split into purpose-specific tools
  with clear contracts.

## Diagnosing *why* a tool isn't used (high-yield)
- **Not discovered at all → config/discovery issue** (scope, MCP
  registration). **Discovered but ignored → description/selection
  issue** (fix the description).
- **Routing breaks right after a system-prompt change, or one keyword
  flips selection** (e.g. the word "account" makes Claude call
  `get_customer` 78% of the time) → the root cause is **system-prompt
  keyword steering**, *not* the tool descriptions. Fix the system
  prompt wording; don't add negative examples to already-clear
  descriptions.
- When choosing a fix, prefer removing semantic overlap **at the
  interface** (rename, then description, then split) over downstream
  patches like routing classifiers or prompt nudges — fix the earliest
  layer. See [[02-root-cause-decision-tree]].

## Related
- [[scoped-tool-access]]
- [[model-context-protocol]]
- [[mcp-resources-vs-tools]]

## Sources
- [[CCA-F Exam Notes]] — task statement 2.1.
- [[CCA-F Practice Exam (v1).meta|CCA-F Practice Exam (v1)]] — improvement areas 9 & 16 (system-prompt
  steering; discovered-but-ignored vs not-discovered).

## Continue reading
- **Fewer tools, better choices** → [[scoped-tool-access]]
- **Structured failures** → [[structured-tool-errors]]

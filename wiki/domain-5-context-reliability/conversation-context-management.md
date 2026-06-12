# Conversation Context Management

> Long context ≠ useful context. Preserve facts, trim noise, surface key findings.

## What it is
Keeping the important information reliable across long, multi-turn
interactions instead of letting it get buried or summarized away.

## Why it matters
Progressive summarization can quietly turn precise facts into vague
statements, and a bigger context window does not fix an
**organization** problem.

## Key ideas
- **Don't summarize away** numbers, dates, IDs, amounts, or explicit
  user expectations.
- Keep a **persistent "case facts" block** (structured, not a vague
  summary).
- **"Lost in the middle"** → put key findings at the **beginning**;
  use clear section headers.
- **Trim tool outputs** before they bloat context.
- Subagents return **clean structured findings**, not reasoning
  dumps, to a context-limited synthesis agent.

## Related
- [[large-codebase-context]]
- [[session-state-management]]
- [[provenance-synthesis]]

## Sources
- [[CCA-F Exam Notes]] — task statement 5.1.

## Continue reading
- **Big-repo techniques** → [[large-codebase-context]]
- **Resume/fork/fresh** → [[session-state-management]]

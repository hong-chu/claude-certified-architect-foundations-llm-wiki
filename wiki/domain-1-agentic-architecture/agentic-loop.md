# Agentic Loop

> The application-side loop that drives an agent — `stop_reason`-driven, not text-driven.

## What it is
Claude does not execute tools by itself. Your application code runs a
loop: **send message → check `stop_reason` → if a tool is requested,
execute it → append the tool result back into the conversation
history → repeat until Claude signals completion.**

## Why it matters
Getting the loop's *stopping condition* right is the foundation of
every agentic system ([[claude-agent-sdk]]). Termination is decided
by `stop_reason`, and **tool results must be fed back** before asking
Claude what to do next.

## Key ideas
- Drive the loop with **`stop_reason`**, not by parsing assistant
  prose.
- **Append tool results** to history each turn — skipping this breaks
  the agent's reasoning.
- An iteration cap is a **safety net**, not the primary stop signal.
- "Assistant produced some text" is **not** a completion signal.

## Related
- [[claude-agent-sdk]]
- [[coordinator-subagent-pattern]]
- [[tool-interface-design]]

## Sources
- [[CCA-F Exam Notes]] — task statement 1.1.

## Continue reading
- **Delegating the work** → [[coordinator-subagent-pattern]]
- **The whole domain** → [[domain-1-agentic-architecture]]

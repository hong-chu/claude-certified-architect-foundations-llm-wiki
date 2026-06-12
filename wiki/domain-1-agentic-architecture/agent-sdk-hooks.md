# Agent SDK Hooks

> Deterministic checkpoints in the agent workflow — normalize results after, block risky calls before.

## What it is
A **hook** is code that runs automatically at a point in the agent
workflow. Hooks are the **deterministic** alternative to prompt-based
enforcement.

## Why it matters
When a business rule needs **guaranteed** compliance, a prompt
instruction (probabilistic) is not enough — a hook (deterministic)
is. See [[prompt-vs-programmatic-enforcement]].

## Key ideas
- **`PostToolUse`** — runs *after* a tool returns, *before* Claude
  sees the result. Use to **normalize messy tool output** (e.g.
  inconsistent date/status formats across MCP tools). Normalize
  **before** the model reasons, not in the final response.
- **Tool-call interception** — runs *before* a tool executes. Use to
  **block or redirect risky calls** (e.g. refund above policy limit →
  block + escalate).
- Choose hooks over "stronger prompt wording," more few-shot, or
  model self-confidence whenever compliance must be guaranteed.

## Related
- [[prompt-vs-programmatic-enforcement]]
- [[structured-tool-errors]]
- [[agentic-loop]]

## Sources
- [[CCA-F Exam Notes]] — task statement 1.5.

## Continue reading
- **Prompt vs gate decision** → [[prompt-vs-programmatic-enforcement]]
- **The whole domain** → [[domain-1-agentic-architecture]]

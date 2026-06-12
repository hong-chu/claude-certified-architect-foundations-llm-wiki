# Task Tool

> The Agent SDK tool that spawns subagents.

## Summary
The `Task` tool is how a coordinator spawns subagents in the
[[claude-agent-sdk]]. Without it in `allowedTools`, the coordinator
cannot delegate at all.

## Key facts
- **Must be in `allowedTools`** — a missing-Task error means add
  `"Task"`.
- Each spawned subagent is configured by an **`AgentDefinition`**
  (description + system prompt + tool restrictions).
- **Parallel subagents** = multiple `Task` calls in a **single**
  coordinator response (independent subtasks → lower latency).
- Subagents started this way have **isolated context** — see
  [[subagent-context-passing]].

## Related
- [[coordinator-subagent-pattern]]
- [[subagent-context-passing]]
- [[claude-agent-sdk]]

## Sources
- [[CCA-F Exam Notes]] — task statement 1.3.

## Continue reading
- **What to pass it** → [[subagent-context-passing]]
- **The orchestration pattern** → [[coordinator-subagent-pattern]]

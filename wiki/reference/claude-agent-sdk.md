# Claude Agent SDK

> The SDK for building agentic applications with Claude — the backbone of Domain 1.

## Summary
The Claude Agent SDK is used to build agentic systems: it runs the
[[agentic-loop]], spawns subagents via the [[task-tool]], applies
[[agent-sdk-hooks]], and manages session state (resume/fork). On the
exam it underpins the customer-support, multi-agent research, and
developer-productivity scenarios.

## Key facts
- Runs agentic loops driven by `stop_reason` — the **app code** runs
  the loop, Claude does not self-execute tools.
- Multi-agent work uses a **coordinator-subagent** design; subagents
  are configured by an **`AgentDefinition`** (description + system
  prompt + tool restrictions).
- Subagents are spawned with the **`Task` tool** (must be in
  `allowedTools`); parallel subagents = multiple Task calls in one
  response.
- **Hooks** (`PostToolUse`, tool-call interception) give deterministic
  enforcement and normalization.
- Session controls: `--resume <name>` and `fork_session`.

## Related
- [[claude-code]]
- [[model-context-protocol]]
- [[coordinator-subagent-pattern]]

## Sources
- [[CCA-F Exam Notes]] — Domain 1.

## Continue reading
- **The loop it runs** → [[agentic-loop]]
- **How it delegates** → [[coordinator-subagent-pattern]]

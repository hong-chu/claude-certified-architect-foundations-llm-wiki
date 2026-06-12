# Subagent Context Passing

> Subagents start blank — feed them complete findings plus metadata, explicitly.

## What it is
How a coordinator equips a subagent to do its job: spawning it with
the [[task-tool]] and passing the context it needs **explicitly** in
its prompt, because subagents **do not inherit** the coordinator's
context.

## Why it matters
This is an exam-heavy area. Vague summaries cause bad results; the
fix is structured **content + metadata** and complete prior findings.

## Key ideas
- The **`Task` tool spawns subagents** — it must be in `allowedTools`,
  or the coordinator cannot spawn.
- **`AgentDefinition`** = description + system prompt + tool
  restrictions (job description + permissions).
- **Pass complete prior findings directly** (e.g. web-search results
  + document analysis → synthesis agent).
- Use a **structured format with metadata/source attribution**, not
  vague summaries.
- **Parallel** independent subtasks = multiple Task calls **in one
  response** (latency win).
- Prompt subagents with **goals + quality criteria**, not rigid
  step-by-step procedures.

## Related
- [[coordinator-subagent-pattern]]
- [[task-tool]]
- [[scoped-tool-access]]

## Sources
- [[CCA-F Exam Notes]] — task statement 1.3.

## Continue reading
- **The spawning tool** → [[task-tool]]
- **Right tools per role** → [[scoped-tool-access]]

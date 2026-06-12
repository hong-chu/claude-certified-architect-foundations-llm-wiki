# Coordinator-Subagent Pattern

> One coordinator decomposes and aggregates; specialized subagents do isolated work.

## What it is
The standard multi-agent design: a **coordinator** breaks a task
into subtasks, routes them to **specialized subagents** (each with
isolated context), then aggregates and synthesizes the results.

## Why it matters
It is chosen for **observability, consistent error handling, and
controlled information flow**. Multi-agent success depends less on
"more agents" and more on **good decomposition, explicit context
passing, and gap checking**.

## Key ideas
- **Coordinator owns**: decomposition, delegation, **scope
  partitioning**, result aggregation, **gap detection**,
  re-delegation, final synthesis.
- **Partition scope *before* delegation.** When subagents are
  source-specialized (e.g. a web-search agent and a document-analysis
  agent), they will investigate the *same* subtopics and double token
  use unless the coordinator partitions the research space up front.
  The wrong fix is "let them finish, then deduplicate" — that's too
  late in the pipeline. Partition by source type / subtopic *before*
  work begins.
- It invokes subagents **based on query complexity** — not every task
  needs every subagent.
- **Subagents have isolated context** and do not inherit the
  coordinator's knowledge — see [[subagent-context-passing]].
- The coordinator must **not** accept the first synthesis if it is
  incomplete (gap detection → re-delegate).
- Subagents should **not** communicate freely with each other or
  share memory implicitly.

## Related
- [[subagent-context-passing]]
- [[agentic-loop]]
- [[error-propagation]]

## Sources
- [[CCA-F Exam Notes]] — task statement 1.2.
- [[CCA-F Practice Exam (v1).meta|CCA-F Practice Exam (v1)]] — improvement area 3 (coordinator
  partitions scope before work begins).

## Continue reading
- **Feeding subagents correctly** → [[subagent-context-passing]]
- **When errors happen** → [[error-propagation]]

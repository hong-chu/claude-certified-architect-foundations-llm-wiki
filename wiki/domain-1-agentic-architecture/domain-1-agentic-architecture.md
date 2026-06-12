# Domain 1 — Agentic Architecture & Orchestration

> **27% of the exam.** How to design agent workflows that can act, delegate, enforce rules, and manage state safely.

This is the heaviest domain. It centers on the [[claude-agent-sdk]]:
building agentic loops, coordinator-subagent systems, enforcement,
hooks, and session state.

> **How this domain is tested** — the cross-cutting [[01-first-principles|method]] as it bites here:
> - **Mechanisms over wishes** — mandatory order/compliance is a *gate or hook*, never prose (1.4, 1.5). → [[05-prompt-fix-or-structural-fix]]
> - **Constrain, don't add** — "more agents" is the trap; good decomposition + explicit context wins (1.2, 1.3). → [[04-does-more-improve-reliability]]
> - **Objective triggers, earliest layer** — escalate on policy/workflow, hand off with structure. → [[02-root-cause-decision-tree]] · traps in [[03-never-right-answers]]

## 1.1 — The agentic loop is `stop_reason`-driven

Claude does **not** execute tools by itself. Your application code
runs the loop: **send message → check `stop_reason` → if tool
needed, execute it → append the tool result back into history →
repeat until Claude finishes.** See [[agentic-loop]].

Anti-patterns (all wrong):
- Parsing natural language to decide the loop should stop.
- Using an arbitrary iteration cap as the *main* stopping mechanism.
- Treating "assistant produced text" as the completion signal.

The two things that matter most: the loop is driven by `stop_reason`,
and **tool results must be appended back** before asking Claude what
to do next.

## 1.2 — Coordinator-subagent orchestration

Use a **coordinator-subagent pattern** for multi-agent systems —
it buys observability, consistent error handling, and controlled
info flow. See [[coordinator-subagent-pattern]].

The **coordinator owns**: task decomposition, delegation, result
aggregation, **gap detection**, re-delegation, and final synthesis.
It decides which subagents to invoke **based on query complexity** —
not every task needs every subagent. **Subagents** are specialized
workers with **isolated context**.

Success depends less on "more agents" and more on good
decomposition, **explicit context passing**, and gap checking.

Wrong: overly narrow decomposition; accepting the first incomplete
synthesis; letting subagents talk freely to each other; assuming
subagents inherit coordinator memory; skipping gap detection.

## 1.3 — Subagent invocation, context passing, spawning

- The **[[task-tool]]** is how you spawn subagents. No Task tool in
  `allowedTools` → coordinator **cannot** spawn. Fix: add `"Task"`.
- Subagents **do not inherit context automatically** — pass it
  **explicitly** in the subagent's prompt. See
  [[subagent-context-passing]].
- An **`AgentDefinition`** = role + system prompt + tool
  restrictions (the subagent's "job description + permissions").
- Pass **complete prior findings** directly (e.g. feed search
  results + document analysis into the synthesis agent).
- Use a **structured format**: content **+ metadata**, not vague
  summaries (exam-heavy point).
- **Parallel subagents** = multiple Task calls **in one coordinator
  response** (for independent subtasks → lower latency).
- Coordinator prompts should specify **goals + quality criteria**,
  not rigid step-by-step procedures.
- **Fork** a session to explore divergent approaches from one
  baseline. See [[session-state-management]].

## 1.4 — Multi-step workflows: enforcement & handoff

**Prompts suggest; gates enforce.** For mandatory step order, do not
rely on the model "remembering" — enforce **programmatically** with
hooks / gates / prerequisite checks. See
[[prompt-vs-programmatic-enforcement]].

- Mandatory order → **programmatic gate**.
- Money / compliance / irreversible action → don't stop at a hook;
  enforce **inside the tool** so it can't happen unless valid. See
  [[action-boundary-enforcement]].
- Multiple issues in one message → **decompose, investigate all,
  synthesize** (don't investigate only the first).
- Escalation → **structured handoff summary** (never a vague summary,
  and don't make the human re-read the full transcript).

Wrong: "add stronger prompt instructions" / "more few-shot examples"
/ "use model confidence" for what is really a *mandatory ordering*
problem.

## 1.5 — Agent SDK hooks

A **hook** is code that runs automatically at a point in the agent
workflow — a **deterministic** checkpoint. Choose hooks over
prompt-based enforcement when business rules require **guaranteed**
compliance (prompt = probabilistic; hook = deterministic). See
[[agent-sdk-hooks]].

Two main types:
- **`PostToolUse`** — runs *after* a tool returns, *before* Claude
  sees it. Use to **normalize messy tool output**.
- **Tool-call interception** — runs *before* a tool executes. Use to
  **block / redirect risky calls** (e.g. refund over policy limit).

Exam patterns: inconsistent tool data formats → `PostToolUse`
normalize; risky action despite prompt instructions → interception
hook; compliance guarantee → programmatic enforcement, not prompting.
Wrong: normalize data only in the final natural-language response
(must normalize *before* the model reasons over it).

## 1.6 — Choosing the workflow shape

- **Known steps → prompt chaining** (fixed sequential pipeline):
  predictable, repeatable, known order, want consistency.
- **Unknown path → dynamic adaptive decomposition**: open-ended,
  unknown dependencies, next step depends on intermediate findings.
- **Large review → local passes + integration pass.**

See [[prompt-chaining-vs-dynamic-decomposition]]. Wrong: one giant
prompt for a large complex review; a fixed pipeline for an
exploratory task; dynamic agents for a simple predictable workflow;
a full implementation plan before exploring unknown dependencies.

## 1.7 — Session state, resumption, forking

See [[session-state-management]] and the [[resume-vs-fork]] decision.
- Prior context still valid → **`--resume <session-name>`**.
- Multiple possible approaches from one baseline → **`fork_session`**.
- Resume **+ changed files** → tell Claude **exactly what changed**
  (don't trust stale tool results).
- Prior context stale/noisy → **fresh session + structured summary**
  (resuming can make the agent *worse*).

## Continue reading
- **The loop everything sits on** → [[agentic-loop]]
- **When prompts aren't enough** → [[prompt-vs-programmatic-enforcement]]
- **Next domain** → [[domain-2-tool-design-mcp]]

## Sources
- [[CCA-F Exam Notes]] — Domain 1, task statements 1.1–1.7.

# Scoped Tool Access

> More tools ≠ better. Restrict each agent's tools to its role.

## What it is
Giving each agent only the tools its role needs, instead of all tools
it might possibly use.

## Why it matters
Too many available tools **degrades tool-selection reliability** by
increasing decision complexity. **Agent role determines tool access.**

## Key ideas
- Restricting tools to a role prevents **cross-specialization misuse**
  (e.g. a synthesis agent attempting web searches).
- Replace **broad generic tools** with **constrained role-specific**
  ones.
- **Scoped cross-role tools** are OK: small frequent need → a narrow
  cross-role tool; complex need → route through the coordinator.
- **`tool_choice`** controls how required a call is — see
  [[tool-choice-auto-any-forced]].

## Related
- [[tool-interface-design]]
- [[tool-choice-auto-any-forced]]
- [[subagent-context-passing]]

## Sources
- [[CCA-F Exam Notes]] — task statement 2.3.

## Continue reading
- **auto / any / forced** → [[tool-choice-auto-any-forced]]
- **Designing the tools** → [[tool-interface-design]]

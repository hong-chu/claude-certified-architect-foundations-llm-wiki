# Multi-Agent Error Propagation

> Local recovery first, then structured error + partial results + coverage gaps.

## What it is
How subagents handle and report failures so the coordinator can
decide what to do — without hiding errors or crashing the workflow.

## Why it matters
The two anti-patterns (hide the error / kill the whole workflow) both
lose information the coordinator needs.

## Key ideas
- Return **structured reasons**, not "search unavailable."
- **Access failure ≠ valid empty result** — see
  [[access-failure-vs-empty-result]].
- **Transient error → local retry first**; unresolved → structured
  report to coordinator with **partial results + what was attempted**.
- Synthesis with partial evidence → **label coverage gaps**; don't
  pretend completeness.
- Don't retry non-retryable failures repeatedly.

## Related
- [[structured-tool-errors]]
- [[access-failure-vs-empty-result]]
- [[coordinator-subagent-pattern]]

## Sources
- [[CCA-F Exam Notes]] — task statement 5.3.

## Continue reading
- **Structured tool errors** → [[structured-tool-errors]]
- **Failure vs empty** → [[access-failure-vs-empty-result]]

# Structured Tool Errors

> Generic errors make agents dumb; structured errors let them retry, ask, explain, escalate, or continue.

## What it is
Tool error responses that carry enough structure for the agent to
make a recovery decision, rather than a bare "Operation failed."

## Why it matters
The agent can only retry/ask/escalate/continue intelligently if the
error tells it **what failed, why, whether it's retryable, what to do
next, and whether there are partial results**.

## Key ideas
- MCP's **`isError`** flag marks a result as an error.
- **Retryable vs non-retryable** metadata prevents wasted retries.
- **Business-rule violations** → `retriable: false` + a
  customer-friendly explanation.
- **Empty result ≠ error** — distinguish access failure from a valid
  "found nothing." See [[access-failure-vs-empty-result]].
- In multi-agent systems, subagents **recover locally** for transient
  failures and propagate only unresolved errors with **partial
  results + what was attempted**. See [[error-propagation]].

## Related
- [[error-propagation]]
- [[access-failure-vs-empty-result]]
- [[agent-sdk-hooks]]

## Sources
- [[CCA-F Exam Notes]] — task statement 2.2.

## Continue reading
- **Across multiple agents** → [[error-propagation]]
- **Failure vs empty** → [[access-failure-vs-empty-result]]

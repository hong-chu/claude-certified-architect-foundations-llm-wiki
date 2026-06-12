# Session State Management

> Resume, fork, or start fresh — pick based on whether prior context is still valid.

## What it is
How to handle an agent's accumulated context across runs: continue a
named session, branch from a baseline, or discard and restart with a
summary.

## Why it matters
"More context" is not always better — stale tool results and noisy
context can make the agent *worse*. See [[resume-vs-fork]].

## Key ideas
- **`--resume <session-name>`** — continue a prior investigation when
  context is still mostly valid and files haven't changed much.
- **`fork_session`** — branch from one baseline to explore divergent
  approaches independently (incremental refactor vs full rewrite vs
  wrapper).
- **Resume + changed files** → tell Claude **exactly what changed**;
  don't trust old tool results.
- **Stale/noisy prior context** → **fresh session + structured
  summary** beats resuming.

## Related
- [[resume-vs-fork]]
- [[conversation-context-management]]
- [[large-codebase-context]]

## Sources
- [[CCA-F Exam Notes]] — task statement 1.7.

## Continue reading
- **The clean decision table** → [[resume-vs-fork]]
- **Keeping context useful** → [[conversation-context-management]]

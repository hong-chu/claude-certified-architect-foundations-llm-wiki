# Resume vs Fork vs Fresh Session

> Continue one path → resume. Branch into several → fork. Stale context → start fresh.

| Situation | Action | Mechanism |
|---|---|---|
| Prior context still valid, files unchanged | **Resume** | `--resume <session-name>` |
| One baseline, multiple approaches to explore | **Fork** | `fork_session` |
| Resuming but files changed | Resume **+ state exactly what changed** | `--resume` + explicit diff note |
| Prior context stale/noisy/outdated | **Fresh session + structured summary** | new session |

## Bottom line
"More context" is not always better. **Resume** to continue the same
line of reasoning; **fork** to explore divergent strategies from a
shared baseline (incremental refactor vs full rewrite vs wrapper);
**start fresh** when old tool results are stale or context is noisy.
Always tell a resumed session **which files changed** — never trust
old tool results after the code moved.

## Sources
- [[CCA-F Exam Notes]] — task statement 1.7.

## Continue reading
- **Session state concept** → [[session-state-management]]
- **Context hygiene** → [[conversation-context-management]]

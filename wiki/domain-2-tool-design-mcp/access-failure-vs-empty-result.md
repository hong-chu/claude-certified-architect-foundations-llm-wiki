# Access Failure vs Valid Empty Result

> "The tool failed" and "the tool found nothing" are different facts — never collapse them.

| Dimension | Access failure | Valid empty result |
|---|---|---|
| Meaning | Tool couldn't run/search | Tool ran; nothing matched |
| `isError` | true | false |
| Retryable? | Maybe (transient) | No — retrying won't change it |
| Agent action | Recover/retry, then escalate | Report "none found" as a real answer |
| Anti-pattern | Returning empty results to hide a failure | Treating "none found" as a failure |

## Bottom line
Distinguish **failure to search** from **search found nothing**.
Returning an empty result when a search actually failed silently
corrupts downstream reasoning; treating a legitimate "no matches" as
an error wastes retries and may trigger needless escalation. Encode
the difference in [[structured-tool-errors]] and propagate it
correctly in [[error-propagation]].

## Sources
- [[CCA-F Exam Notes]] — task statements 2.2, 5.3.

## Continue reading
- **Structured errors** → [[structured-tool-errors]]
- **Error propagation** → [[error-propagation]]

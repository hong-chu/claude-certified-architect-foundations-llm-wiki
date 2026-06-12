# Message Batches API

> The Claude API's asynchronous, lower-cost path for large non-urgent jobs (Domain 4).

## Summary
The Message Batches API processes many requests asynchronously at
lower cost, with no fast-completion guarantee. Use it for bulk,
non-interactive workloads; never for blocking/interactive ones.

## Key facts
- **Cheaper but slower** — can wait hours → batch.
- **Bad for pre-merge / interactive** work (unit tests, lint,
  security scan, type check, Claude review, real-time support) →
  use the **synchronous API**. See [[batch-vs-synchronous-api]].
- **No multi-turn tool calling** inside a single batch request —
  self-contained one-pass jobs only.
- Each request needs a unique **`custom_id`** (responses return out
  of order).
- On partial failure, **resubmit only the failed `custom_id`s**.
- **Test the prompt on a sample** before a large batch.

## Related
- [[batch-processing]]
- [[structured-output]]

## Sources
- [[CCA-F Exam Notes]] — Domain 4, task statement 4.5.

## Continue reading
- **When to batch vs not** → [[batch-vs-synchronous-api]]
- **Batch strategy details** → [[batch-processing]]

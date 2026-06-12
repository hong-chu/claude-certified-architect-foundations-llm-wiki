# Batch Processing

> Cheaper, slower, async — for large non-urgent one-pass jobs.

## What it is
Using the [[message-batches-api]] for high-volume, non-interactive
workloads.

## Why it matters
Batch trades latency for cost. Misusing it for blocking workflows
stalls users/CI; misusing the sync API for overnight jobs wastes
money.

## Key ideas
- **Can wait hours / large non-urgent** → batch.
- **User or CI waiting (pre-merge)** → synchronous API. See
  [[batch-vs-synchronous-api]].
- **No multi-turn tool calling** inside one batch request —
  self-contained one-pass jobs only.
- Unique **`custom_id`** per request (out-of-order responses).
- On failure, **resubmit only failed `custom_id`s**.
- **Test the prompt on a sample** before a huge batch.

## Related
- [[message-batches-api]]
- [[batch-vs-synchronous-api]]
- [[structured-output]]

## Sources
- [[CCA-F Exam Notes]] — task statement 4.5.

## Continue reading
- **The decision** → [[batch-vs-synchronous-api]]
- **The API** → [[message-batches-api]]

# Batch API vs Synchronous API

> Batch = cheap + async for bulk jobs. Synchronous = anything someone is waiting on.

| Dimension | Message Batches API | Synchronous API |
|---|---|---|
| Cost | Lower | Higher |
| Latency | Slow, no fast guarantee | Immediate |
| Good for | Large, non-urgent jobs (can wait hours) | Interactive / pre-merge work |
| Tool calling | **No** multi-turn tools in one request | Full tool loop |
| Examples | Overnight extraction over 100k docs | Unit tests, lint, security scan, type check, Claude review, real-time support |
| Matching | Needs unique `custom_id` | N/A |

## Bottom line
Choose **batch** only when the work is large, non-urgent, and a
**self-contained one-pass** job. Choose **synchronous** whenever a
user or CI is **waiting** (pre-merge checks) or a **multi-turn tool
loop** is needed. Wrong: batch for pre-merge or real-time support;
batch when tool calls are needed mid-request; synchronous for
overnight jobs just because it's simpler; resubmitting the whole
batch when only some `custom_id`s failed.

## Sources
- [[CCA-F Exam Notes]] — task statement 4.5.

## Continue reading
- **Batch strategy** → [[batch-processing]]
- **The API entity** → [[message-batches-api]]

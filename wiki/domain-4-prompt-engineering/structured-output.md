# Structured Output (Tool Use + JSON Schema)

> Tool use + JSON schema guarantees structure, not truth.

## What it is
Forcing valid, schema-conformant JSON output by using `tool_use` with
a JSON schema, instead of asking Claude to "return JSON."

## Why it matters
Schema guarantees **syntax/shape**; it does **not** guarantee
**meaning**. You still need semantic validation. See
[[schema-vs-semantic-validation]].

## Key ideas
- Need guaranteed valid JSON → **`tool_use` + JSON schema**.
- **`tool_choice`**: `auto` (optional) / `any` (multiple schemas,
  unknown doc type) / forced (specific extraction first). See
  [[tool-choice-auto-any-forced]].
- **Optional/nullable** fields for data that may be absent — required
  absent fields force fabrication.
- **Enums** with **`unclear`** (ambiguous) and **`other` + detail**
  (unexpected).
- Add **normalization rules** for source formats.
- Schema validity ≠ correct extraction → add
  [[validation-retry-loops]].

## Related
- [[schema-vs-semantic-validation]]
- [[validation-retry-loops]]
- [[tool-choice-auto-any-forced]]

## Sources
- [[CCA-F Exam Notes]] — task statement 4.3.

## Continue reading
- **Shape vs meaning** → [[schema-vs-semantic-validation]]
- **Validate and retry** → [[validation-retry-loops]]

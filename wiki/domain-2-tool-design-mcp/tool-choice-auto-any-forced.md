# tool_choice: auto vs any vs forced

> auto = maybe a tool; any = some tool, model picks; forced = this specific tool.

| Value | Meaning | Use when |
|---|---|---|
| `auto` | Model decides whether to call a tool | Tool call is **optional** |
| `any` | Must call **a** tool; model picks which | Multiple extraction schemas, **unknown** document type; structured output is mandatory |
| forced (specific) | Must call **one named** tool | A specific extraction/action **must happen first** |

## Bottom line
`tool_choice` controls **how required** a tool call is. For
structured output that **must** be produced, never use `auto` — use
`any` (when the right schema isn't known) or **force** the specific
tool (when one extraction must happen). Pair with giving agents only
the few tools they need ([[scoped-tool-access]]).

## Sources
- [[CCA-F Exam Notes]] — task statements 2.3, 4.3.

## Continue reading
- **Structured output** → [[structured-output]]
- **Scoped tools** → [[scoped-tool-access]]

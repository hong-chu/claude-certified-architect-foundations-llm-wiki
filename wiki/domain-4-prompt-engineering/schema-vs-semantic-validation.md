# Schema (Syntax) vs Semantic Validation

> Schema fixes the shape; semantic checks fix the meaning.

| Dimension | Schema / syntax validation | Semantic validation |
|---|---|---|
| Checks | JSON shape, types, required fields, enums | Whether values are *correct* / consistent |
| Guaranteed by | `tool_use` + JSON schema | Custom checks, cross-field rules |
| Catches | Malformed output | Wrong-but-well-formed output |
| Example | "amount is a number" | "amount matches the line items" |
| Limitation | Valid JSON can still be wrong | Needs domain logic |

## Bottom line
A passing schema means the output is **well-formed**, not **true**.
Always add **semantic validation** on top, and on failure retry with
the **specific** errors plus the original document and the failed
extraction. Wrong: assuming schema prevents semantic errors; skipping
validation because the JSON is valid.

## Sources
- [[CCA-F Exam Notes]] — task statements 4.3, 4.4.

## Continue reading
- **Structured output** → [[structured-output]]
- **Retry loops** → [[validation-retry-loops]]

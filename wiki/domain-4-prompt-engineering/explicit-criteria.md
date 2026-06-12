# Explicit Criteria

> Define exactly what to report, what to skip, and how to rate severity — with examples.

## What it is
Replacing vague guidance ("be conservative", "high-confidence only")
with explicit report/skip rules and example-anchored severity levels.

## Why it matters
Precision (fewer false positives) comes from specificity, not from
adjectives. False positives damage developer trust in review tools.

## Key ideas
- Define **report vs skip** categories explicitly.
- High false positives → **reduce scope** or **disable noisy
  categories** so they don't pollute the whole review.
- Define **severity with concrete examples**.
- Don't review everything at once (style + security + bugs + perf +
  docs + architecture) — scope it.
- Don't filter on model confidence alone.

## Related
- [[few-shot-prompting]]
- [[claude-code-cicd]]
- [[independent-multipass-review]]

## Sources
- [[CCA-F Exam Notes]] — task statement 4.1.

## Continue reading
- **Examples that teach** → [[few-shot-prompting]]
- **The whole domain** → [[domain-4-prompt-engineering]]

# Claude Code in CI/CD

> Non-interactive, machine-readable, no duplicate feedback.

## What it is
Running [[claude-code]] inside a CI/CD pipeline for automated review,
test generation, and PR feedback.

## Why it matters
CI has no human to answer prompts, downstream systems must parse
output, and noisy/duplicate feedback erodes trust.

## Key ideas
- **`-p` / `--print`** → non-interactive mode (fixes hanging).
- **JSON output + schema** → automated parsing.
- **`CLAUDE.md`** carries project review/testing criteria (fixes
  low-value feedback) — not just stuffed in the prompt.
- **Independent review instance** → don't let a session review its
  own generated code. See [[independent-multipass-review]].
- **Reruns**: include prior findings to **suppress duplicate** PR
  comments; show **existing tests** to avoid duplicate test
  suggestions.
- **Pre-merge work is synchronous** (waiting CI), not batch. See
  [[batch-vs-synchronous-api]].

## Related
- [[independent-multipass-review]]
- [[explicit-criteria]]
- [[batch-vs-synchronous-api]]

## Sources
- [[CCA-F Exam Notes]] — task statement 3.6.

## Continue reading
- **Don't grade your own homework** → [[independent-multipass-review]]
- **Precise review criteria** → [[explicit-criteria]]

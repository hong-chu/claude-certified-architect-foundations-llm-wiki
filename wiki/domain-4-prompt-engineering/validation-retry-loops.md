# Validation, Retry & Feedback Loops

> Retry with the specific errors — and know when retry can't help.

## What it is
The loop around structured extraction: validate output, retry with
precise feedback, and escalate/null out when the information simply
isn't there.

## Why it matters
Valid JSON is not correct extraction. Blind retries waste calls when
the source lacks the data.

## Key ideas
- **Retry with specific validation errors**, not vague "try again."
- Give the retry **all three**: original document + previous failed
  extraction + specific errors.
- **Schema (syntax) vs semantic** validation are separate layers —
  see [[schema-vs-semantic-validation]].
- **Know when retry won't help**: info absent → retry won't create
  truth → return null / escalate.
- Track **`detected_pattern`** (the category that triggered a
  finding, not the issue text) to spot false-positive sources.
- Add self-correction **validation fields** that expose
  inconsistencies.
- Not all validation errors are equally retryable.

## Related
- [[structured-output]]
- [[schema-vs-semantic-validation]]
- [[confidence-calibration]]

## Sources
- [[CCA-F Exam Notes]] — task statement 4.4.

## Continue reading
- **Guaranteeing structure** → [[structured-output]]
- **Calibrating trust** → [[confidence-calibration]]

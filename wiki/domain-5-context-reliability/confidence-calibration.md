# Confidence Calibration & Human Review

> Don't trust one accuracy number — measure by segment, calibrate, sample, and route.

## What it is
Designing human-review workflows and confidence handling for
extraction/review systems.

## Why it matters
An overall accuracy number hides bad segments, and raw model
confidence is not trustworthy until calibrated.

## Key ideas
- **Overall accuracy hides bad segments** → measure **by document
  type and field**.
- Use **field-level** confidence (not just document-level), and
  **calibrate** it against labeled data.
- **Stratified random sampling** — QA even high-confidence outputs.
- **Route** limited human-review capacity to ambiguous/risky cases.
- Don't fully automate just because aggregate accuracy is high (e.g.
  97%).

## Related
- [[independent-multipass-review]]
- [[validation-retry-loops]]
- [[escalation-patterns]]

## Sources
- [[CCA-F Exam Notes]] — task statement 5.5.

## Continue reading
- **Independent review** → [[independent-multipass-review]]
- **Validation loops** → [[validation-retry-loops]]

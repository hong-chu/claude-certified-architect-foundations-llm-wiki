# Independent & Multi-Pass Review

> Don't let Claude grade its own homework.

## What it is
Making Claude's review of work more reliable through independent
review instances, multi-pass analysis, and (calibrated) verification
passes.

## Why it matters
The same session that generated something is a biased reviewer — it
carries the generation reasoning and misses its own gaps.

## Key ideas
- **Same-session self-review is biased** → use a **second independent
  instance**.
- **Large multi-file review** → **per-file passes + a cross-file
  integration pass** (integration bugs hide between files).
- A **verification pass** can attach confidence/priority — but **do
  not blindly trust** it; calibrate. See [[confidence-calibration]].
- Don't substitute extended thinking for independent review; don't
  review everything in one giant pass.

## Related
- [[claude-code-cicd]]
- [[confidence-calibration]]
- [[explicit-criteria]]

## Sources
- [[CCA-F Exam Notes]] — task statement 4.6.

## Continue reading
- **Calibrating confidence** → [[confidence-calibration]]
- **CI/CD review** → [[claude-code-cicd]]

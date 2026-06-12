# Iterative Refinement

> Examples clarify requirements; tests guide fixes; interviews uncover hidden tradeoffs.

## What it is
Techniques for progressively improving Claude's output by giving it
the right kind of feedback for the problem.

## Why it matters
"Be more careful" and raising temperature don't work. Matching the
technique to the failure mode does.

## Key ideas
- Inconsistent transformation/format → **2–3 concrete input/output
  examples**.
- Broken implementation → **share specific failing tests** (input,
  expected, actual).
- Unfamiliar domain / hidden tradeoffs → **interview pattern** before
  implementing.
- **Interacting** problems → one detailed message (reason about
  interactions); **independent** problems → sequential iteration.

## Related
- [[few-shot-prompting]]
- [[explicit-criteria]]
- [[plan-mode]]

## Sources
- [[CCA-F Exam Notes]] — task statement 3.5.

## Continue reading
- **Teaching by example** → [[few-shot-prompting]]
- **CI/CD usage** → [[claude-code-cicd]]

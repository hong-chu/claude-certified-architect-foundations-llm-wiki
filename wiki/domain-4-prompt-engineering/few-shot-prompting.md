# Few-Shot Prompting

> Teach by example — show correct, incorrect, and ambiguous cases so Claude generalizes.

## What it is
Providing a few labeled examples in the prompt to demonstrate the
desired behavior, format, and boundaries.

## Why it matters
The default fix for inconsistent output, ambiguous judgment, false
positives, and missing-field handling — more effective than prose,
temperature changes, or immediate fine-tuning.

## Key ideas
- Use it when: output format is inconsistent; ambiguous judgment
  boundary; false positives; varied document formats; missing fields.
- A good spread: a **positive**, a **negative**, an
  **edge/ambiguous**, and maybe a **missing-field/conflict** example.
- Add **2–4 targeted** examples (one example is not enough for a
  complex ambiguous boundary).
- Goal is **generalization**, not memorization.

## Related
- [[explicit-criteria]]
- [[structured-output]]
- [[iterative-refinement]]

## Sources
- [[CCA-F Exam Notes]] — task statement 4.2.

## Continue reading
- **Explicit report/skip rules** → [[explicit-criteria]]
- **Guaranteeing structure** → [[structured-output]]

# When is the fix a prompt change vs a structural/programmatic change?

> Match the fix to the failure: prose for *judgment/consistency*, mechanisms for *guarantees*.

**Status:** resolved

## Why I'm asking
A huge share of exam items present a failure and four fixes. Two
look like "edit the prompt" (add prose, add an example) and two look
like "change the system" (add a gate, restructure the tools). Picking
the wrong *kind* of fix is the most common way to lose points.

## Current best answer
**Match the fix to the failure's root cause:**

| Symptom | Right kind of fix |
|---|---|
| Output format inconsistent / judgment ambiguous | **Prompt** — add 2–4 [[few-shot-prompting]] examples |
| Vague instruction ("be careful") not followed | **Prompt** — replace with [[explicit-criteria]] |
| A step *must always* happen, or *must* be ordered | **Structural** — enforce in code/gate, not prose. See [[prompt-vs-programmatic-enforcement]] |
| Wrong tool chosen among many | **Structural** — reduce/scope tools. See [[scoped-tool-access]] |
| Malformed JSON | **Structural** — `tool_use` + JSON schema. See [[structured-output]] |
| Well-formed but *wrong* values | **Structural** — [[schema-vs-semantic-validation|semantic validation]] + retry |
| Context noisy / stale | **Structural** — reorganize context, fresh session. See [[conversation-context-management]] |

## Rule of thumb
If a requirement is **mandatory** (must always hold) or
**safety/correctness-critical**, prose alone can't guarantee it —
enforce it **programmatically**. If the issue is **consistency of
style or judgment**, a prompt example is the cheaper correct fix.
See the recurring theme in [[04-does-more-improve-reliability]].

## Sources
- [[CCA-F Exam Notes]] — recurring across domains 1, 2, 4.

## Continue reading
- **Prompt vs gate** → [[prompt-vs-programmatic-enforcement]]
- **Few-shot** → [[few-shot-prompting]]
- **Does more help?** → [[04-does-more-improve-reliability]]

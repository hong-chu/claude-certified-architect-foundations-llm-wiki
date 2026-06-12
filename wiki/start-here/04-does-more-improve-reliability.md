# Does adding more (agents, tools, context) improve reliability?

> Usually no — the exam rewards *constraining* over *adding*. "More" is the trap, not the fix.

**Status:** resolved

## Why I'm asking
Many exam distractors offer "add more" fixes — more tools, more
agents, more context window, more prose. They feel helpful and are
almost always wrong.

## Current best answer
**No — "more" usually hurts.** The exam repeatedly rewards
*constraining* over *adding*:
- **More tools** → degrades tool-selection reliability (decision
  complexity). Use [[scoped-tool-access]] instead.
- **More agents** → success comes from good decomposition + explicit
  context passing + gap checking, not agent count. See
  [[coordinator-subagent-pattern]].
- **Bigger context window** → does **not** fix an *organization*
  problem; structure facts instead. See
  [[conversation-context-management]].
- **More prose** ("be careful", "be conservative") → replace with
  explicit criteria, examples, or a gate. See [[explicit-criteria]],
  [[prompt-vs-programmatic-enforcement]].

## Evidence for
- "Too many tools confuse selection" (2.3, 2.4).
- "Bigger context window as the only fix" is a listed wrong answer
  (5.1, 5.4).
- "Add more few-shot/prose" wrong when the issue is mandatory order
  (1.4) or tool selection (2.1).

## Evidence against
- Few-shot examples (2–4) *are* the right "add" for inconsistent
  format/ambiguous judgment — adding the *right thing* in the *right
  place* still matters. See [[few-shot-prompting]].

## Related
- [[scoped-tool-access]]
- [[conversation-context-management]]
- [[prompt-vs-programmatic-enforcement]]

## Sources
- [[CCA-F Exam Notes]] — recurring across all domains.

## Continue reading
- **Fewer tools** → [[scoped-tool-access]]
- **Prompt vs gate** → [[prompt-vs-programmatic-enforcement]]

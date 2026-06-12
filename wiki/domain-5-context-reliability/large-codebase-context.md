# Large Codebase Context

> Subagents for noisy discovery, scratchpads for facts, summaries between phases, manifests for recovery, /compact when bloated.

## What it is
Techniques for keeping [[claude-code]] reliable during long
explorations of a large repository.

## Why it matters
Long sessions get noisy and stale; important facts get buried and
generic knowledge competes with repo-specific facts. Don't rely on
long-session memory.

## Key ideas
- **Scratchpad files** — persistent notes of exact findings; re-read
  after compression. Make it deterministic ("after each phase, update
  the scratchpad").
- **Subagents** for verbose/messy discovery (keeps main context
  clean).
- **Summarize before each phase transition.**
- **Manifests** — structured record of what each agent did and where
  state is saved → crash recovery.
- **`/compact`** when bloated — but it does **not** write the
  scratchpad. Safe order: **Explore → write scratchpad → summarize
  phase → compact if needed → continue.**

## Related
- [[conversation-context-management]]
- [[builtin-tools]]
- [[plan-mode]]

## Sources
- [[CCA-F Exam Notes]] — task statement 5.4.

## Continue reading
- **General context hygiene** → [[conversation-context-management]]
- **Built-in tools** → [[builtin-tools]]

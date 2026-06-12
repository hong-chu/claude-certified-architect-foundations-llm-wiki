# Plan Mode

> Explore and design before making changes — for complex, multi-file, architectural work.

## What it is
A [[claude-code]] mode where Claude explores and designs before
editing, preventing premature changes.

## Why it matters
Jumping into edits on complex work causes rework; plan mode forces
understanding first. But staying in plan mode after the plan is clear
wastes time.

## Key ideas
- **Plan mode** — complex/architectural/many-file changes (e.g.
  microservice restructuring); explore dependencies first.
- **Direct execution** — simple, clear, one-file changes.
- **Explore subagent** — offload very large/messy discovery outside
  the main context (analogous to `context: fork` for skills).
- **Combine**: plan first, then direct-execute once the plan is clear.

## Related
- [[plan-mode-vs-direct-execution]]
- [[large-codebase-context]]
- [[slash-commands-and-skills]]

## Sources
- [[CCA-F Exam Notes]] — task statement 3.4.

## Continue reading
- **The decision** → [[plan-mode-vs-direct-execution]]
- **Big-repo exploration** → [[large-codebase-context]]

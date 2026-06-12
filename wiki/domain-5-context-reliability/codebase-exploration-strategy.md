# Codebase exploration strategy (explore the graph, not a keyword list)

> First name the mode — understand architecture, find callers, or debug — then traverse the code as a graph from entry points and interfaces.

A whole cluster of exam questions are really *code-navigation*
questions. The wrong instinct is "grep one keyword and read snippets."
The right instinct is to **traverse the dependency graph**:

> entry point → interface / base class → imports & callers →
> implementation → tests

## Step 1 — identify the mode
The correct strategy depends on *what you're trying to learn*:

| Mode | Question looks like | Best pattern |
|---|---|---|
| **Understand architecture** | "Understand auth across 800 files" | Grep for entry points (login/token/middleware) → read those → follow imports/calls → build the map incrementally |
| **Find all callers** | "A function is renamed through wrappers" | Read the wrapper/library modules **first** → list all exposed names/aliases → grep each alias |
| **Debug a failure** | "Endpoint intermittently 500s" | **Adaptive** decomposition — follow the evidence, not a fixed plan or blind fan-out |

## Step 2 — obey the navigation rules
- Do **not** read everything upfront.
- Do **not** grep one keyword and trust snippets alone.
- **Start from entry points or interfaces.**
- Then follow imports, callers, and real code paths.
- Order of operations: **Search → Read → Trace → Modify → Test.**

## The mental model
Think of the codebase as a city: *entry point* = where the request
enters; *route* = the path it follows; *middleware* = checkpoints;
*service* = business-logic department; *database* = storage; *wrapper*
= a front desk/alias for an existing function; *caller* = code that uses
a function; *interface/base class* = a blueprint shared by many
implementations.

## How it relates
- For *large* repos, combine this with [[large-codebase-context]]
  (subagents for noisy discovery, scratchpads, summaries, `/compact`).
- "Debug = adaptive" is the same instinct as
  [[prompt-chaining-vs-dynamic-decomposition|dynamic decomposition]]:
  unknown path → let findings drive the next step.

## Sources
- [[CCA-F Practice Exam (v1).meta|CCA-F Practice Exam (v1)]] — "Explore code like a graph";
  "Software / codebase exploration vocabulary."

## Continue reading
- **Large-repo context** → [[large-codebase-context]]
- **Built-in tools** → [[builtin-tools]]
- **Fixed vs adaptive** → [[prompt-chaining-vs-dynamic-decomposition]]

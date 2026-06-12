# Source: CCA-F Practice Exam (v1).pdf

- **Type:** mistake-pattern review / exam post-mortem (PDF, 36 pages, 3.5 MB)
- **Ingested:** 2026-06-11 (via `pdftotext -layout`)
- **Status:** fully ingested as text
- **What it is:** NOT a raw question bank — it's a worked review of a
  practice attempt: 19 "improvement areas," each pairing a scenario
  with the **correct** root-cause fix and the **wrong** (plausible but
  shallow) fix, plus a master decision tree, 7 first principles, and a
  list of recurring traps. The single highest-signal source for *how
  the exam wants you to think*.
- **Genuinely new material (drove new wiki pages):**
  - **Root cause vs workaround / fix the earliest controllable layer**
    — the dominant meta-pattern. → [[02-root-cause-decision-tree]].
  - **Action-boundary enforcement** — money/compliance/irreversible
    actions must be enforced *inside the tool*, not just via a hook or
    prompt. → [[action-boundary-enforcement]].
  - **@import organizes but does not reduce loaded context; path-scoped
    rules do.** → [[import-vs-path-scoped-rules]].
  - **Temporal / multi-value domain modeling** — amendments and
    time-series have *multiple* valid values; don't auto-pick newest.
    → [[temporal-data-modeling]].
  - **Codebase exploration as a graph** (entry point → interface →
    callers → implementation → tests) with 3 modes. →
    [[codebase-exploration-strategy]].
  - **Deferred tool loading** for many MCP tools. → [[deferred-tool-loading]].
  - Diagnostics: *tool not discovered = config issue; discovered but
    ignored = description issue*; *system-prompt keyword steering*;
    *Edit non-unique match → add context first, not Read+Write*.

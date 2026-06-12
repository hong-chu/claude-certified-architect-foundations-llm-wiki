# Domain 3 — Claude Code Configuration & Workflows

> **20% of the exam.** Configuring [[claude-code]] for teams: CLAUDE.md hierarchy, slash commands & skills, path-specific rules, plan mode, iterative refinement, and CI/CD.

(The raw notes mislabel this domain's header — the content outline
names it *Claude Code Configuration & Workflows*.)

> **How this domain is tested** — the cross-cutting [[01-first-principles|method]] as it bites here:
> - **Right thing, right place** — always-on → CLAUDE.md; path-specific → rules; task-specific → skill; manual → command (3.1–3.3). → [[01-first-principles]] (C4)
> - **Organize vs reduce context** — `@import` organizes but loads eagerly; path-scoped rules actually cut context. → [[02-root-cause-decision-tree]]
> - **Scope traps** — team config in user scope, `@import` used to "save tokens." → [[03-never-right-answers]]

## 3.1 — CLAUDE.md hierarchy, scoping, modular organization

`CLAUDE.md` is Claude Code's project memory / instruction file. See
[[claude-md-hierarchy]].

- **Personal rules → `~/.claude/CLAUDE.md`** (user-level, **not**
  shared with teammates).
- **Team rules → project `CLAUDE.md`** (or `.claude/CLAUDE.md`).
- **Big rules → `@import` or `.claude/rules/`** to stay modular — but
  `@import` only *organizes* (loads eagerly); path-scoped rules
  actually *reduce* loaded context. See [[import-vs-path-scoped-rules]].
- **Confusing/inconsistent behavior → `/memory`** to inspect which
  instruction files are actually loaded.

Team vs personal placement is itself a recurring decision — see
[[project-vs-user-scope]].

Exam patterns: new teammate misses instructions → they were in
user-level config; move them to project level. Huge CLAUDE.md →
`@import` / split into `.claude/rules/`. Behaves differently across
sessions → `/memory`.

## 3.2 — Custom slash commands and skills

**Slash commands = command shortcuts. Skills = richer reusable
workflows with configuration.** See [[slash-commands-and-skills]].

- Scope: **team/shared → project `.claude/`**; **personal →
  `~/.claude/`** (commands in `commands/`, skills in `skills/`).
- A **Skill** lives in `.claude/skills/` with a **`SKILL.md`** file.
  Frontmatter options:
  - **`context: fork`** — run the skill in **isolated context** (do
    messy/verbose/long-running thinking elsewhere, return a clean
    summary). Don't apply to every skill.
  - **`allowed-tools`** — restrict what a risky skill can use.
  - **`argument-hint`** — prompt the user for required input.
- Want your own version of a team skill → **copy it personally**,
  don't modify the shared team skill.

## 3.3 — Path-specific rules

See [[path-specific-rules]].
- Rules that depend on file path/type → **`.claude/rules/` + a
  `paths` glob** (with YAML frontmatter).
- Rules that apply everywhere → **`CLAUDE.md`**.
- Subdirectory `CLAUDE.md` works **only** when the convention maps
  cleanly to one folder; use path globs when files are spread across
  the repo (e.g. all test files).
- Note: glob **patterns in rules** are not the **Glob tool** —
  similar name, different thing.

## 3.4 — Plan mode vs direct execution

See [[plan-mode]] and [[plan-mode-vs-direct-execution]].
- **Plan mode** = explore/design **before** changes — complex,
  architectural, many-file work (e.g. microservice restructuring).
- **Direct execution** = simple, clear, one-file changes.
- **Explore subagent** = offload very large/messy discovery outside
  the main context (analogous to `context: fork` for skills).
- You can **combine**: plan first, then direct-execute once the plan
  is clear. Don't stay in plan mode after the plan is settled.

## 3.5 — Iterative refinement

See [[iterative-refinement]]. Match the technique to the problem:
- Inconsistent transformation/format → **2–3 concrete input/output
  examples**.
- Broken implementation → **share specific failing test cases**
  (input, expected, actual).
- Unfamiliar domain / hidden tradeoffs → **interview pattern** before
  implementing.
- **Interacting** problems → fix together in **one detailed message**;
  **independent** problems → **sequential** iteration.

Wrong: "be more careful"; raise temperature; vague prose; fixing
interacting bugs one at a time; skipping tests; implementing before
clarifying unfamiliar requirements.

## 3.6 — Claude Code in CI/CD

In CI, Claude Code must run **non-interactively**, produce
**machine-readable** output, and avoid duplicate/low-value feedback.
See [[claude-code-cicd]].

- **`-p` / `--print`** → non-interactive mode (fixes hanging).
- **JSON output + schema** → automated parsing downstream.
- **`CLAUDE.md`** → project review/testing criteria (fixes low-value
  feedback) — not just stuffed into the prompt.
- **Independent review instance** → don't let a session review its
  own generated code. See [[independent-multipass-review]].
- **Reruns** → include prior findings to **suppress duplicate** PR
  comments; show **existing tests** to avoid duplicate test
  suggestions.

## Continue reading
- **Where instructions live** → [[claude-md-hierarchy]]
- **Think first or just do it?** → [[plan-mode-vs-direct-execution]]
- **Next domain** → [[domain-4-prompt-engineering]]

## Sources
- [[CCA-F Exam Notes]] — Domain 3, task statements 3.1–3.6.

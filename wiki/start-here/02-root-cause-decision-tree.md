# The root-cause decision tree (how the exam wants you to choose)

> The exam is almost always asking: *which fix addresses the root cause at the earliest, safest layer?* — not "which answer sounds helpful?"

This is the single most useful exam artifact, distilled from the
practice-exam post-mortem. Most wrong answers are **reasonable but fix
the problem too late in the pipeline**. The right answer fixes the
**earliest controllable layer**. Where [[01-first-principles]] explains
*why*, this page is the *procedure*.

## Step 1 — label the failure mode
Before choosing, force yourself to name what kind of problem it is:

> definition · example · schema · validation · context · tool-boundary ·
> workflow/decomposition · provenance/time · code-exploration ·
> escalation/customer-experience

## Step 2 — walk the decision tree
| # | Is the problem caused by… | Fix |
|---|---|---|
| 1 | Unclear boundaries (tools/agents/interface) | Fix the boundary — rename/split tools, scope access. [[tool-interface-design]], [[scoped-tool-access]] |
| 2 | A **vague rule / definition** | Add [[explicit-criteria]] |
| 3 | **Inconsistent application** of a *known* rule | Add [[few-shot-prompting|few-shot examples]] |
| 4 | **Ambiguous transformation** | Add concrete input/output examples |
| 5 | **Invalid** structured output | `tool_use` + JSON schema. [[structured-output]] |
| 6 | Valid structure but **wrong values** | Semantic validation + retry with specific errors. [[schema-vs-semantic-validation]] |
| 7 | **Risky/mandatory** workflow order | Programmatic gate/hook — and for money/compliance/irreversible, [[action-boundary-enforcement|enforce inside the tool]] |
| 8 | **Long/noisy** context | Preserve structured facts, trim output, scratchpads/summaries. [[conversation-context-management]] |
| 9 | **Duplicated/inefficient** work | Decompose & **partition before work starts**. [[coordinator-subagent-pattern]] |
| 10 | **Missing trust/provenance** | Preserve claim–source–date mappings. [[provenance-synthesis]] |

## Step 3 — apply the tie-breaker
When two answers both sound reasonable:

> **Pick the one that preserves the most useful information while
> removing the least context** — and that acts *earliest* in the
> pipeline.

Dates preserve temporal meaning ([[temporal-data-modeling]]); multiple
values preserve amendment history; a structured summary preserves a
conversation without stale tool results; interface-first reading
preserves architecture ([[codebase-exploration-strategy]]).

## The recurring "good pattern, wrong situation" traps
A correct technique applied to the wrong failure mode is still wrong:
- **"Use schema"** fixes structure, not meaning — and not a wrong-shaped
  schema (temporal/multi-value needs a redesign).
- **"Use subagents"** is right only *after* scope is known; unknown
  scope → explore entry points first.
- **"Use case facts"** is for exact transactional facts; long narrative
  continuity wants progressive summarization instead.
- **"Escalate to human"** is not always immediate — enough context →
  offer/route; zero context → ask one targeted question first;
  frustrated-but-solvable → acknowledge + solve.
- **"@import"** organizes but doesn't reduce context — path-scoped rules
  do. [[import-vs-path-scoped-rules]].

## Diagnostic one-liners worth memorizing
- Tool **not discovered** → config/discovery issue. Tool **discovered
  but ignored** → description/selection issue.
- Tool routing breaks **after a system-prompt change** → fix the system
  prompt wording (not the tool descriptions).
- **Edit** fails on a non-unique match → add surrounding context to make
  the anchor unique; fall back to Read+Write only if Edit still can't
  work.
- Severity consistency → **concrete examples per level**, not a numeric
  rubric alone.

## First principles (the practice-exam's own list)
1. Find the failure mode. 2. Fix the earliest root cause. 3. Use
deterministic controls for critical rules. 4. Keep agent/tool/config
boundaries narrow. 5. Decompose complex work before solving.
6. Preserve facts, metadata, and sources. 7. Use feedback loops to
improve over time. And the config mnemonic: **Rule = policy · Command =
button · Skill = workflow · CLAUDE.md = always-on · MCP = external
tools/data.**

## Sources
- [[CCA-F Practice Exam (v1).meta|CCA-F Practice Exam (v1)]] — "Master decision tree," "First
  Principles," "One final exam rule," "Knowing when NOT to use a good
  pattern."

## Continue reading
- **Why these work** → [[01-first-principles]]
- **The traps catalog** → [[03-never-right-answers]]
- **Exam overview** → [[00-overview]]

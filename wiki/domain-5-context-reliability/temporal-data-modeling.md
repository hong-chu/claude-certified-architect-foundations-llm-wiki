# Temporal & multi-value domain modeling

> If reality has more than one valid value, don't force a single one — model the multiplicity and keep the dates.

A recurring "cross-domain instinct" the exam rewards: **model the
domain correctly.** When a fact legitimately has multiple values over
time (amendments, revisions, time-series statistics) or across sources,
collapsing it to one value *loses information* — the opposite of what a
good architect does.

## The rule
- **Multiple valid values over time → the schema must store *multiple*
  values**, each with its **source and effective/collection date**.
- **Temporal conflict → preserve the dates; do NOT auto-pick the
  newest.** "Newer" is not the same as "correct"; the right value may
  depend on the as-of date the question cares about.
- **Amendments → multiple effective values**, not an overwrite.

## Examples
| Scenario | Wrong | Right |
|---|---|---|
| A contract is amended twice | Keep only the latest terms | Store each version + effective date |
| Two credible sources report different stats | Pick the newer number | Keep both + dates + context |
| A policy changed mid-year | Overwrite with the new policy | Record both with date ranges |

## Why it's tested
It sits at the intersection of [[structured-output]] (the schema must
have room for multiplicity) and [[provenance-synthesis]] (each value
keeps its source + date). The trap answer "use schema/tool_use" fixes
*format* but not a **schema that's the wrong shape for the domain** —
the deeper fix is redesigning the schema to hold multiple dated values.

## Connection to first principles
This is principle **E1 (never collapse distinct states)** applied to
*time*, and the master heuristic: *preserve the most useful information
while removing the least context*. See [[01-first-principles]],
[[02-root-cause-decision-tree]].

## Sources
- [[CCA-F Practice Exam (v1).meta|CCA-F Practice Exam (v1)]] — "Biggest conceptual gaps: model the
  domain correctly"; "Temporal data conflict → preserve dates."

## Continue reading
- **Schema vs meaning** → [[schema-vs-semantic-validation]]
- **Provenance** → [[provenance-synthesis]]
- **Structured output** → [[structured-output]]

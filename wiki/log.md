# Change Log

Append-only. One entry per ingest / lint / recompile / schema
change. Newest at the top.

Format:
```
## YYYY-MM-DD — <one-line summary>
- created/updated: <path>  ← <source or reason>
- notes: <optional>
```

---

## 2026-06-12 — comprehensive audit + fixes
- **Empty graph nodes (fixed):** `[[CCA-F Notes.meta|CCA-F Notes]]`, `[[CCA-F Practice
  Exam (v1)]]`, `[[CCA-F Cheatsheet.meta|CCA-F Cheatsheet]]`, `[[ccaf.meta|ccaf]]` pointed at PDFs with
  no `.md`, so Obsidian rendered them as unresolved/empty nodes.
  Re-pointed all to `[[<name>.pdf|<name>]]` so they resolve to the real
  source (clicking opens the PDF; hidden from graph by the sources
  filter + showAttachments off).
- **Hub→leaf coverage gap (fixed):** 8 leaf pages weren't linked from
  their domain hub — incl. Domain 2 **omitting task 2.6 entirely**.
  Added a real **2.6 — Deferred tool loading** section to the D2 hub
  and wove the other 7 into their hubs (action-boundary-enforcement,
  resume-vs-fork, import-vs-path-scoped-rules, project-vs-user-scope,
  schema-vs-semantic-validation, codebase-exploration-strategy,
  temporal-data-modeling). Every hub now links all its domain pages.
- **Clean:** lint (H1/tagline/Sources/Continue-reading) ✅, 0 orphans,
  all kebab-case, task statements 1.1–5.6 all covered, 0 broken links.
- **Flagged, not changed:** citation imbalance — 55 pages cite the
  superseded `CCA-F Exam Notes` (.md) vs few citing the authoritative
  `CCA-F Notes` (.pdf). Decision pending: retire the old markdown +
  re-point, or leave.

## 2026-06-11 — weave start-here method into the domains (reciprocal)
- added a tailored **"How this domain is tested"** callout to each of
  the 5 domain hubs, naming the specific principles/traps that bite in
  that domain and linking the cross-cutting method pages
  ([[01-first-principles]], [[02-root-cause-decision-tree]],
  [[03-never-right-answers]], the two question pages).
- intent: the method is now reached *from within* each domain (a
  companion accessed per-domain), not a top-level layer above them.
  Content of the method pages unchanged (user likes it). The graph
  already hides `start-here` so the syntheses don't render as a megahub.
- verified 0 broken links.

## 2026-06-11 — name hubs with their real title (folder-note)
- renamed each hub to **match its folder** (folder-note convention),
  e.g. `domain-1-agentic-architecture/domain-1-agentic-architecture.md`.
  The filename is now the domain's real title; with inline titles off,
  the page shows its H1. Supersedes `00-domain-N` / `domain-N-overview`.
- updated all `[[wikilinks]]`; graph color query `file:domain-` still
  matches all 5; updated `CLAUDE.md`. Verified 0 broken.

## 2026-06-11 — rename hubs to domain-N-overview (final)
- renamed each hub → `domain-N-overview.md` (e.g.
  `domain-1-agentic-architecture/domain-1-overview.md`). Leads with the
  domain, no artificial `00-` prefix, no full-topic repeat.
- TRADEOFF accepted (per user): the hub now sorts mid-folder
  alphabetically instead of first.
- updated all `[[wikilinks]]`, graph color query (`file:00-domain-` →
  `file:domain-`), and `CLAUDE.md` naming/sort notes. Verified 0 broken.
- (supersedes the earlier `00-domain-N` and `00-domain-N-<topic>` names.)

## 2026-06-11 — rename 00-start-here → start-here (domains lead tree)
- renamed folder `wiki/00-start-here/` → `wiki/start-here/` so the five
  `domain-N-*/` folders sort to the **top** of the file tree (d < r < s),
  making the domains the visible top layer. Page filenames (and thus all
  `[[wiki-links]]`) unchanged — verified 0 broken.
- updated folder-name prose in `index.md`, `start-here/00-overview.md`,
  `CLAUDE.md` (kept older `log.md` entries as historical record).
- graph: color group narrowed to **only the 5 domain hubs**
  (`file:00-domain-`, one color); all other nodes uncolored. NOTE:
  Obsidian overwrites `.obsidian/graph.json` while running, so this must
  be (re)applied via the graph's Groups UI to persist.

## 2026-06-11 — graph cleanup: color-groups + connect orphan metas
- added: Obsidian graph **color groups by folder** in
  `.obsidian/graph.json` (each domain + start-here + reference +
  sources gets its own color) so the domain structure is visible in
  the graph view.
- fixed: connected two orphan-island source meta files
  (`CCA-F Exam Notes.meta`, `CCA-F Cheatsheet.meta`) by linking them to
  [[CCA-F Notes.meta|CCA-F Notes]] and the domain hubs.
- noted (not yet changed): citation imbalance — 54 pages still cite the
  superseded `[[CCA-F Exam Notes]]` vs 3 citing the newer `[[CCA-F Notes.meta|CCA-F Notes]]`
  PDF. Candidate for a re-point or retiring the old markdown.

## 2026-06-11 — polish pass: hub sort-order, reading funnel, taglines
- renamed: each domain hub → `00-domain-N-*.md` so it sorts first in
  its folder (file-tree users open the hub first).
- renamed: `00-start-here/` pages to a numbered reading funnel
  (`00-overview` → `01-first-principles` → `02-root-cause-decision-tree`
  → `03-never-right-answers` → `04-does-more…` → `05-prompt-fix…`).
- updated: all `[[wiki-links]]` to renamed pages across `wiki/`,
  `sources/`, `README.md` (verified 0 broken); `index.md` links given
  clean display aliases.
- fixed: added missing blockquote taglines to the two question pages
  (lint: every page needs an H1 + one-line blockquote).
- updated: `CLAUDE.md` with the sort-order naming convention so future
  ingests keep hubs/reading-order sorting correctly.

## 2026-06-11 — schema migration: reorganize wiki domain-first
- moved: all 60 content pages out of type-folders (`concepts/`,
  `comparisons/`, `syntheses/`, `entities/`, `questions/`) into
  domain-first folders: `00-start-here/`, `domain-1-agentic-architecture/`,
  `domain-2-tool-design-mcp/`, `domain-3-claude-code-config/`,
  `domain-4-prompt-engineering/`, `domain-5-context-reliability/`,
  `reference/`. Removed the old empty type-folders + template READMEs.
- updated: `CLAUDE.md` schema (new directory tree + "Organizing
  principle — domain-first" + placement rules in ingest/lint). This is
  a **schema migration**.
- updated: `wiki/index.md` rewritten as a domain-first study map.
- rationale: wiki is for exam study; the unit of study is the domain,
  not the page type. Filenames unchanged, so all `[[wiki-links]]`
  still resolve (verified 0 broken). Page *types* retained as a
  writing convention (see templates), just no longer as folders.

## 2026-06-11 — ingest 3 new PDFs (Notes, Practice Exam, Cheatsheet)
- added sources: `CCA-F Notes.pdf` (160p), `CCA-F Practice Exam (v1).pdf`
  (36p), `CCA-F Cheatsheet.pdf` (12p); installed poppler so PDFs now
  extract via `pdftotext`.
- created meta: `CCA-F Notes.meta.md`, `CCA-F Practice Exam (v1).meta.md`,
  `CCA-F Cheatsheet.meta.md`.
- created concepts: `deferred-tool-loading.md` (Tool Search Tool /
  `defer_loading`, Task 2.6), `action-boundary-enforcement.md`,
  `temporal-data-modeling.md`, `codebase-exploration-strategy.md`.
- created comparison: `import-vs-path-scoped-rules.md`.
- created synthesis: `root-cause-decision-tree.md` (master decision tree
  + failure-mode labeling + earliest-controllable-layer).
- updated: `coordinator-subagent-pattern.md` (+scope partitioning before
  delegation), `tool-interface-design.md` (+system-prompt steering,
  +discovered-vs-ignored diagnostic), `index.md`.
- notes: the Notes PDF is a cleaner restatement of the older
  `CCA-F Exam Notes.md` (kept both); the Practice Exam PDF was the
  highest-signal new source. Source PDFs still carry the Domain 3
  header mislabel. The older image-only `ccaf.pdf` remains un-ingested
  (now possible with poppler — deferred unless needed).

## 2026-06-01 — rework first-principles into 6 grouped axiom families
- updated: `wiki/syntheses/first-principles.md` ← user feedback ("not
  good enough"; missing the *why* behind human-in-the-loop)
- notes: expanded 9 flat principles → 16 axioms in 6 groups (A model
  nature, B humans/authority, C system design, D guarantees, E
  information, F decisions). Added group B (B1 boundary of authority,
  B2 autonomy∝reversibility, B3 accountable hand-off) so human-in-the-
  loop is rooted in authority/reversibility, not model uncertainty.
  Also added A2 stateless/bounded, A3 non-determinism, C2 least
  privilege, C3 decomposition, D3 fail-loud, E3 explicit-over-implicit,
  F3 iterate-with-feedback. All 33 wiki-links verified.

## 2026-05-31 — add first-principles synthesis
- created: `wiki/syntheses/first-principles.md` ← user request
- notes: initial 9 flat axioms (P1–P9); superseded by the 2026-06-01 rework.

## 2026-05-31 — add never-right-answers trap catalog
- created: `wiki/questions/never-right-answers.md` ← user request
- updated: `wiki/index.md` (linked new question page)
- notes: consolidates the six distractor trap families across all domains.

## 2026-05-31 — first ingest of CCA-F exam notes
- created: `sources/CCA-F Exam Notes.meta.md`, `sources/ccaf.meta.md`
- updated: `wiki/overview.md` (exam structure, domain weights, wrong-answer meta-patterns)
- created: 5 syntheses in `wiki/syntheses/` (one per exam domain) ← CCA-F Exam Notes
- created: 5 entities in `wiki/entities/` (Agent SDK, Claude Code, MCP, Batches API, Task tool)
- created: 29 concepts in `wiki/concepts/`
- created: 10 comparisons in `wiki/comparisons/` (decision pairs)
- created: 2 questions in `wiki/questions/` (does-more-improve-reliability, prompt-fix-or-structural-fix)
- updated: `wiki/index.md` (curated entry point)
- notes: primary source = `CCA-F Exam Notes.md` (3067 lines, read in full).
  `ccaf.pdf` (40-page deck) NOT text-ingested — no PDF renderer available
  (poppler not installed); diagram-only content still pending. Raw notes
  mislabel the Domain 3 header as "Agentic Architecture"; it is actually
  Claude Code Configuration & Workflows (corrected in the synthesis).

## YYYY-MM-DD — wiki initialized
- created: `CLAUDE.md` (schema, v1)
- created: scaffolding under `wiki/` and `sources/`
- notes: scaffolded by the new-llm-wiki skill. Ready to ingest sources.

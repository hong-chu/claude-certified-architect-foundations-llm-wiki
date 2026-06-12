# CLAUDE.md — LLM Wiki Schema

> This file is the **schema**. It tells the agent how this repo is
> laid out, what files mean, and which workflows to run. Humans read
> the wiki; the agent maintains it.
> Pattern adapted from
> [Andrej Karpathy's *llm-wiki* gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

## TL;DR for the agent

You are the maintainer of a wiki about **the Claude Certified Architect – Foundations certification exam**: the Claude/Anthropic platform knowledge, prompt-engineering and agent-design principles, API features, and exam objectives needed to study for and pass the exam.

The user curates raw material into `sources/`. You compile it into a
clean, interlinked set of pages under `wiki/`. You never modify
`sources/`. You always update `wiki/log.md`.

## Three layers

```
<vault root>/
├── CLAUDE.md       # Layer 3 — schema (this file)
├── README.md       # human entry point
├── sources/        # Layer 1 — raw, immutable inputs (the user owns this)
├── wiki/           # Layer 2 — derived markdown (you own this)
│   ├── index.md
│   ├── log.md
│   ├── start-here/                 # cross-cutting "how to think" — read first
│   │   (overview, first-principles, root-cause-decision-tree,
│   │    never-right-answers, and exam-wide question pages)
│   ├── domain-1-agentic-architecture/ # Agentic Architecture & Orchestration (27%)
│   ├── domain-2-tool-design-mcp/      # Tool Design & MCP Integration (18%)
│   ├── domain-3-claude-code-config/   # Claude Code Configuration & Workflows (20%)
│   ├── domain-4-prompt-engineering/   # Prompt Engineering & Structured Output (20%)
│   ├── domain-5-context-reliability/  # Context Management & Reliability (15%)
│   └── reference/                     # products & SDKs (entities): SDK, Claude Code, MCP, …
└── derived/        # optional: charts, slides, exports built from wiki/
```

### Organizing principle — domain-first
This wiki is for **exam study**, so pages are grouped by **exam domain**,
not by page type. Each `domain-N-*/` folder is a self-contained study
unit containing that domain's:
- **synthesis** (the domain hub, named the same as its folder, e.g.
  `domain-1-agentic-architecture/domain-1-agentic-architecture.md`),
- **concept** pages (one idea/technique/term each), and
- **comparison** pages (`x-vs-y.md` decision pairs).

The five **page *types*** (concept · entity · synthesis · comparison ·
question) still define how a page is *written* — see Page templates
below — they just no longer define *where it lives*. Placement rules:
- A page that belongs to one domain → that `domain-N-*/` folder.
- A cross-cutting page (spans all domains: principles, decision trees,
  trap catalogs, exam-wide questions) → `start-here/`.
- An **entity** (a product/SDK/spec referenced across domains) →
  `reference/`.
- A question scoped to one domain → that domain folder; otherwise
  `start-here/`.

Obsidian resolves `[[wiki-links]]` by **filename**, so a page can be
moved between folders without breaking links — keep filenames unique
and `kebab-case.md`.

**Sort-order convention** (so the file tree reads top-down):
- Each domain's **hub** is named the **same as its folder** (the
  "folder-note" convention) so the filename is the domain's real title,
  e.g. `domain-4-prompt-engineering/domain-4-prompt-engineering.md`. No
  `00-` prefix. (With **Show inline title** off, the page displays its
  H1 — "Domain 4 — Prompt Engineering & Structured Output" — not the
  filename. Tradeoff: the hub sorts mid-folder, not first.)
- `start-here/` pages carry a two-digit reading-order prefix
  (`00-overview`, `01-first-principles`, `02-root-cause-decision-tree`,
  `03-never-right-answers`, then focused question pages `04+`).
- When renaming a page, update its `[[wiki-links]]` everywhere
  (`wiki/`, `sources/*.meta.md`, `README.md`) and re-run the broken-link
  check.

### Layer 1 — `sources/`  (immutable)
- The user drops in PDFs, papers, screenshots, transcripts, blog
  dumps, notebooks, links collected as `.md` clippings, etc.
- **You read but never write here.** If a source needs cleanup, copy
  it, don't mutate it.
- Each source should ideally have a sibling `*.meta.md` with: title,
  author, url, date, why-it-matters. If missing, create it on first
  ingest.

### Layer 2 — `wiki/`  (the wiki — you own it)
- One concept, entity, synthesis, comparison, or question per file.
- File names: `kebab-case.md` (e.g. `mixture-of-experts.md`).
- Cross-link liberally with **wiki-style links**:
  `[[mixture-of-experts]]`, `[[entities/openai]]`. Every page should
  link out to ≥ 2 others.
- Every page must end with a `## Sources` section listing the
  `sources/` files (or external URLs) it draws from.

### Layer 3 — `CLAUDE.md`  (the schema)
- This file. Update it when conventions evolve. Treat changes here
  as schema migrations — note them in `wiki/log.md`.

## Page templates

Pages are written for *humans first* and agents second. Every page
opens with a one-sentence summary as a markdown blockquote (`> …`),
followed by structured prose, and closes with a `## Continue reading`
footer that points the reader at 2–3 curated next pages. Wiki-links
use `[[wiki-link]]` syntax; every page has a `## Sources` section.

### Concept page (`wiki/domain-N-*/<slug>.md`)
```markdown
# <Concept Name>

> <single-sentence summary that doubles as a tagline>

## What it is
<2–6 sentence explanation, plain English first>

## Why it matters
<the practical or theoretical consequence>

## Key ideas
- bullet
- bullet

## Related
- [[concepts/<other>]]
- [[entities/<paper-or-person>]]

## Open questions
- ...

## Sources
- [[sources/<file>]] — <one-line note>

## Continue reading
- **<short reader-facing label>** → [[<target-page>]]
- **<short reader-facing label>** → [[<target-page>]]
```

### Entity page (`wiki/reference/<slug>.md`)
```markdown
# <Entity Name>

> <single-sentence summary including type if useful>

## Summary
<who/what, 3–8 sentences>

## Key facts
- founded / released / authored: ...
- affiliation: ...
- notable for: ...

## Timeline
- YYYY-MM — event

## Related
- [[entities/...]]
- [[concepts/...]]

## Sources
- ...

## Continue reading
- **<label>** → [[<target>]]
- **<label>** → [[<target>]]
```

### Synthesis (`wiki/domain-N-*/<slug>.md`, or `start-here/`)
A short essay (300–800 words) that pulls from ≥ 2 sources to argue a
non-obvious point or summarize a thread of work. Each domain's hub page
(named like its folder) is a synthesis. Open with a blockquote tagline; must
cite each source; end with `## Continue reading`.

### Comparison (`wiki/domain-N-*/<slug>.md`)
Use a table. Always include columns for: dimension, A, B,
who-wins-when, sources. Open with a blockquote tagline; below the
table, a 1-paragraph "bottom line"; end with `## Continue reading`.

### Question (`wiki/<domain-or-start-here>/<slug>.md`)
Question pages keep an explicit `**Status:**` line — readers care
whether something is open or resolved.

```markdown
# <The Question>

**Status:** open | partially-answered | resolved

## Why I'm asking
...

## Current best answer
...

## Evidence for
- ...
## Evidence against
- ...

## Related
- ...

## Sources
- ...

## Continue reading
- **<label>** → [[<target>]]
- **<label>** → [[<target>]]
```

## Workflows

### `ingest` — new file appeared in `sources/`
1. Read the source. If it has no `*.meta.md` sibling, generate one.
2. Decide: does it primarily extend an **existing page** or warrant a
   **new** one?
   - "New" if: a new concept, entity, paper, or model not covered yet.
   - "Extend" if: deepens / contradicts / dates an existing page.
3. Place a new page by **domain** (see "Organizing principle" above):
   the relevant `domain-N-*/` folder, or `start-here/` if
   cross-cutting, or `reference/` if it's an entity.
4. Apply the change. Keep prose tight; prefer linking to duplicating.
5. Add a one-line entry to `wiki/log.md` (date, change, source touched).
6. Update `wiki/index.md` (under the right domain section) if a new
   page was created.

### `query` — user asks a question
1. Search `wiki/` first. If the answer exists, cite the page(s).
2. If partial, answer from the wiki and flag gaps as `wiki/questions/`
   entries (status: open).
3. If absent, search `sources/`. If found there but not yet on the
   wiki, answer **and** queue a "promote-to-wiki" item in
   `wiki/log.md`.
4. Always cite. Inline links:
   `[mixture-of-experts](concepts/mixture-of-experts.md)`.

### `lint` — periodic hygiene pass
- Every page has: H1, blockquote one-liner, ≥ 2 outgoing links, a
  `## Sources` section, and a `## Continue reading` footer (questions
  also keep `**Status:**`).
- No orphan pages: every page is reachable from `index.md` in ≤ 2 hops.
- Every page lives in the right folder per the domain-first rule
  (domain folder / `start-here/` / `reference/`).
- No dead `[[wiki-links]]`: target file must exist.
- File names are `kebab-case.md`; no spaces, no upper-case.
- `wiki/log.md` is append-only and dated.
- Stubs older than 30 days get a `**TODO:**` banner.

### `recompile` — rebuild from scratch
- Treat `wiki/` as ephemeral. Delete it, then re-derive every page
  from `sources/` + `CLAUDE.md`. Useful as a sanity check.

## Style rules

- Prefer **short, plain sentences**. Define jargon on first use.
- One idea per page. Split when a page grows beyond ~600 lines.
- Link, don't duplicate. If you'd repeat a definition, link instead.
- Cite everything. No claim without a source link.
- Mark uncertainty explicitly: *"as of YYYY-MM, …"*,
  *"unverified:"*.
- Disagreements between sources go into a `## Contradictions`
  section, not silently averaged.
- Dates: ISO `YYYY-MM-DD`.

## What NOT to do

- Don't edit anything in `sources/`.
- Don't create pages with no sources behind them. (Personal opinion
  is fine but must be marked `> Note (user):` and dated.)
- Don't let `wiki/index.md` become a wall of links — keep it
  curated.
- Don't invent citations.

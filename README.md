# Claude Certified Architect – Foundations (CCA-F) — Study Wiki

An interlinked, domain-organized study wiki for the **Claude Certified
Architect – Foundations** exam. It distills the exam's five domains
into bite-sized, cross-linked pages: the concepts, the decision pairs
("when X vs Y"), the recurring distractor traps, and a first-principles
method for choosing the right answer.

> ⚠️ **Unofficial.** This is a community study aid, not affiliated with
> or endorsed by Anthropic. It reflects one learner's synthesis and may
> contain mistakes — verify against the official exam guide and the
> [Anthropic docs](https://docs.anthropic.com). Raw source PDFs are not
> included (see *Sources* below).

<p align="center">
  <img src="assets/graph.png" alt="CCA-F study wiki as an Obsidian graph" width="480"><br>
  <sub><em>The wiki as a knowledge graph — the five blue nodes are the exam-domain hubs; every concept and decision page links into them.</em></sub>
</p>

## 📝 How I passed in 2 weeks

Companion write-up on the strategy behind this wiki:
**[How I Passed the Claude Certified Architect – Foundations (CCA-F) Exam in 2 Weeks](https://medium.com/@yeesun.chu/how-i-passed-the-claude-certified-architect-foundations-cca-f-exam-in-2-weeks-6b967e6effb4)** (Medium).

## How to use it

- **Browse on GitHub** — every page is plain Markdown; start with
  [`wiki/index.md`](wiki/index.md).
- **Visualization: open in [Obsidian](https://obsidian.md)** — *Open
  folder as vault* → select this repo. You get working `[[wiki-links]]`,
  backlinks, and a graph view (pre-configured to highlight the five
  domain hubs).
- **Best: ask your AI to study with you** — point an AI coding agent
  (Claude Code, Cursor, Codex, …) at this repo and have it tutor you
  from the wiki: quiz you on a domain, explain a decision pair, walk
  the root-cause decision tree on a practice question, or drill the
  traps. The agent reads [`CLAUDE.md`](CLAUDE.md) and the pages and
  works straight from them.

**Start here:** [`wiki/start-here/00-overview.md`](wiki/start-here/00-overview.md)
→ then read the five domain folders in weight order.

## What's inside

```
wiki/
├── start-here/                     read first: how to think
│   ├── 00-overview                 exam structure, domain weights, scenarios
│   ├── 01-first-principles         the axioms behind every right/wrong answer
│   ├── 02-root-cause-decision-tree label the failure → fix the earliest layer
│   ├── 03-never-right-answers      catalog of distractor traps
│   └── 04–05                       two focused study questions
├── domain-1-agentic-architecture/  Agentic Architecture & Orchestration (27%)
├── domain-2-tool-design-mcp/       Tool Design & MCP Integration (18%)
├── domain-3-claude-code-config/    Claude Code Configuration & Workflows (20%)
├── domain-4-prompt-engineering/    Prompt Engineering & Structured Output (20%)
├── domain-5-context-reliability/   Context Management & Reliability (15%)
└── reference/                      the products & SDKs (Agent SDK, MCP, …)
```

Each `domain-*/` folder is a self-contained study unit: a hub page
(named after the domain) plus its concept and "X vs Y" decision pages.
The `start-here/` method pages are cross-cutting — every domain hub
links to the principles and traps that apply to it.

## The five domains

| # | Domain | Weight |
|---|--------|--------|
| 1 | Agentic Architecture & Orchestration | 27% |
| 2 | Tool Design & MCP Integration | 18% |
| 3 | Claude Code Configuration & Workflows | 20% |
| 4 | Prompt Engineering & Structured Output | 20% |
| 5 | Context Management & Reliability | 15% |

## Sources

This wiki is derived from personal study notes. The **raw source PDFs
are intentionally not committed** (they're personal and may include
third-party material); each is described by a `*.meta.md` file in
[`sources/`](sources/) so citations and provenance still make sense.

## How it's maintained

Built on the [LLM-Wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
(Karpathy): raw material goes in `sources/`, an LLM agent compiles it
into `wiki/` following the schema in [`CLAUDE.md`](CLAUDE.md), and
[`wiki/log.md`](wiki/log.md) records every change.

## Contributing

Corrections welcome — open an issue or PR. Keep pages in the existing
style (one-line summary, `## Sources`, `## Continue reading`,
`kebab-case.md`); see [`CLAUDE.md`](CLAUDE.md) for the full schema.

## License

[CC BY 4.0](LICENSE) — share and adapt freely, with attribution.
© 2026 Hong Chu.

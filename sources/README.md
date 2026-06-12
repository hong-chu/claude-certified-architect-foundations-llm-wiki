# sources/  — Layer 1 (raw, immutable)

Your **input layer**. Drop anything you've read or want to remember:

- Papers (`.pdf`)
- Blog clippings (`.md` is best — paste the body)
- Transcripts (interviews, talks, podcasts)
- Screenshots, diagrams, notes
- Notebooks (`.ipynb`)
- Curated link lists

## Rules
- **The agent does not modify files in this folder.** If a source
  needs cleanup, the agent will copy it elsewhere, not edit in place.
- One source = one file (or one folder if it's multi-part).
- Names: `YYYY-MM-DD-short-slug.<ext>` is a good default — e.g.
  `2017-06-12-attention-is-all-you-need.pdf`.

## Suggested sibling: `*.meta.md`

For every non-text source, add a sibling `*.meta.md` so the agent
has context. Example, beside `2017-06-12-attention-is-all-you-need.pdf`:

```markdown
# Meta

- title: Attention Is All You Need
- authors: Vaswani et al.
- url: https://arxiv.org/abs/1706.03762
- date: 2017-06-12
- type: paper
- why-it-matters: introduces the Transformer architecture
- tags: architecture, attention, foundational
```

The agent will create one for you on first ingest if it's missing.

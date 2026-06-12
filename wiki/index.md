# Index

Curated entry point for the **Claude Certified Architect – Foundations**
exam wiki. The folders mirror the exam: a cross-cutting **`start-here`**
("how to think"), one folder per **domain** (your study units), and a
**`reference`** shelf for the products/SDKs.

> Suggested path: read `start-here` first, then study domains in
> weight order (1 → 4 → 3 → 5 → 2). Each domain's synthesis is its hub.

## 📍 start-here — read first (in order)
- [[00-overview|Overview]] — exam structure, domain weights, recurring patterns
- [[01-first-principles|First principles]] — the axioms behind every right/wrong answer
- [[02-root-cause-decision-tree|Root-cause decision tree]] — the procedure: label the failure → fix the earliest layer
- [[03-never-right-answers|Never-right answers]] — catalog of distractor traps
- [[04-does-more-improve-reliability|Does "more" help?]] — adding tools/agents/context (usually no)
- [[05-prompt-fix-or-structural-fix|Prompt fix or structural fix?]] — which *kind* of fix matches the failure

## Domain 1 — Agentic Architecture & Orchestration (27%)
Hub: [[domain-1-agentic-architecture|Domain 1 hub]]
- Concepts: [[agentic-loop]] · [[coordinator-subagent-pattern]] · [[subagent-context-passing]] · [[session-state-management]] · [[agent-sdk-hooks]] · [[action-boundary-enforcement]]
- Decisions: [[prompt-vs-programmatic-enforcement]] · [[prompt-chaining-vs-dynamic-decomposition]] · [[resume-vs-fork]]

## Domain 2 — Tool Design & MCP Integration (18%)
Hub: [[domain-2-tool-design-mcp|Domain 2 hub]]
- Concepts: [[tool-interface-design]] · [[structured-tool-errors]] · [[scoped-tool-access]] · [[deferred-tool-loading]] · [[mcp-configuration]] · [[mcp-resources-vs-tools]] · [[builtin-tools]]
- Decisions: [[tool-choice-auto-any-forced]] · [[access-failure-vs-empty-result]]

## Domain 3 — Claude Code Configuration & Workflows (20%)
Hub: [[domain-3-claude-code-config|Domain 3 hub]]
- Concepts: [[claude-md-hierarchy]] · [[slash-commands-and-skills]] · [[path-specific-rules]] · [[plan-mode]] · [[iterative-refinement]] · [[claude-code-cicd]]
- Decisions: [[plan-mode-vs-direct-execution]] · [[project-vs-user-scope]] · [[import-vs-path-scoped-rules]]

## Domain 4 — Prompt Engineering & Structured Output (20%)
Hub: [[domain-4-prompt-engineering|Domain 4 hub]]
- Concepts: [[explicit-criteria]] · [[few-shot-prompting]] · [[structured-output]] · [[validation-retry-loops]] · [[batch-processing]] · [[independent-multipass-review]]
- Decisions: [[schema-vs-semantic-validation]] · [[batch-vs-synchronous-api]]

## Domain 5 — Context Management & Reliability (15%)
Hub: [[domain-5-context-reliability|Domain 5 hub]]
- Concepts: [[conversation-context-management]] · [[large-codebase-context]] · [[codebase-exploration-strategy]] · [[error-propagation]] · [[escalation-patterns]] · [[confidence-calibration]] · [[provenance-synthesis]] · [[temporal-data-modeling]]
- Decisions: [[escalate-vs-clarify-vs-solve]]

## reference — products & SDKs
- [[claude-agent-sdk]] · [[claude-code]] · [[model-context-protocol]] · [[message-batches-api]] · [[task-tool]]

## Meta
- [[log]] — what changed and when
- [[../CLAUDE|Schema (CLAUDE.md)]]
- [[../README|README]]

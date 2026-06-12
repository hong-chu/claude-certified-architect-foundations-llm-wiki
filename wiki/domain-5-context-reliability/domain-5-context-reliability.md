# Domain 5 — Context Management & Reliability

> **15% of the exam.** Keeping Claude reliable when conversations, tool outputs, documents, or multi-agent work get long and messy.

> **How this domain is tested** — the cross-cutting [[01-first-principles|method]] as it bites here:
> - **Structure over volume** — a bigger context window never fixes an *organization* problem; structure the facts (5.1, 5.4). → [[04-does-more-improve-reliability]] · [[01-first-principles]] (E2)
> - **Objective triggers** — escalate on policy/workflow, never sentiment or self-reported confidence (5.2, 5.5). → [[01-first-principles]] (F1)
> - **Never collapse states; keep provenance** — failure ≠ empty; preserve source + date (5.3, 5.6). → [[02-root-cause-decision-tree]] · traps in [[03-never-right-answers]]

## 5.1 — Conversation context across long interactions

**Long context ≠ useful context.** Preserve facts, trim noise,
structure summaries, keep metadata, surface key findings. See
[[conversation-context-management]].

- **Progressive summarization loses precise facts** — don't summarize
  away numbers, dates, IDs, amounts, or explicit user expectations.
- Keep a **persistent "case facts" block** (structured, not a vague
  summary).
- **"Lost in the middle"**: put key findings at the **beginning**;
  use clear section headers.
- **Trim tool outputs** before they bloat context.
- Subagents should return **clean structured findings**, not huge
  reasoning dumps, to a context-limited synthesis agent.

Wrong: summarize everything into one paragraph; keep all raw tool
output forever; bury critical facts mid-prompt; drop metadata;
assume a bigger context window fixes an **organization** problem.

## 5.2 — Escalation & ambiguity resolution

**Escalate for explicit human requests, policy gaps, or no
progress. Clarify ambiguity. Don't rely on sentiment or model
confidence.** See [[escalation-patterns]] and
[[escalate-vs-clarify-vs-solve]].

- Valid triggers: explicit "I want a human", **policy silent/
  ambiguous**, workflow stuck (no progress).
- Explicit "human now" → escalate. Frustrated but solvable →
  acknowledge + solve.
- **Don't use sentiment** as a proxy for complexity; **don't trust
  self-reported confidence**.
- Multiple customer matches → **ask for identifiers**, don't guess.
- Policy doesn't cover the case → escalate, **don't invent a policy**.

## 5.3 — Multi-agent error propagation

**Local recovery first, then structured error + partial results +
coverage gaps.** A subagent should neither hide errors nor crash the
whole workflow. See [[error-propagation]].

- Return **structured reasons**, not "search unavailable."
- **Access failure ≠ valid empty result.** See
  [[access-failure-vs-empty-result]].
- Transient error → **local retry first**; unresolved → structured
  report to coordinator with partial results + what was attempted.
- Synthesis with partial evidence → **label coverage gaps**; don't
  pretend completeness.

Wrong: empty results when a search failed; stop everything on one
subagent failure; hide partial results; retry non-retryable
failures; final report with no coverage note; "no matches" = "tool
failed."

## 5.4 — Large codebase exploration

Long repo investigations get noisy and stale; **don't rely on
long-session memory.** See [[large-codebase-context]]. For *how* to
traverse the code (name the mode — understand / find callers / debug —
then follow entry points → interfaces → callers), see
[[codebase-exploration-strategy]].

- **Scratchpad files** — persistent notes of exact findings; Claude
  re-reads them after compression. Make it deterministic: "after each
  phase, update the scratchpad."
- **Subagents** for verbose/messy discovery.
- **Summarize before each phase transition.**
- **Manifests** — structured record of what each agent did and where
  state is saved → crash recovery.
- **`/compact`** when context is bloated — but it does **not** write
  the scratchpad. Safe sequence: **Explore → write scratchpad →
  summarize phase → compact if needed → continue.**

Wrong: keep all exploration in main session; rely on Claude to
remember exact classes; start every phase from scratch; resume after
crash without structured state; bigger context window as the only fix.

## 5.5 — Human review & confidence calibration

**Don't trust one overall accuracy number.** See
[[confidence-calibration]].
- Overall accuracy hides bad segments → measure **by document type
  and field**.
- Use **field-level** confidence (not just document-level), and
  **calibrate** it against labeled data.
- **Stratified random sampling** — QA even high-confidence outputs.
- **Route** limited human-review capacity to ambiguous/risky cases.

Wrong: aggregate accuracy only; full automation because overall is
97%; trust uncalibrated confidence; review only low-confidence and
never sample high-confidence; document-level confidence only.

## 5.6 — Provenance in multi-source synthesis

**Claim + source + date + excerpt + conflict handling + appropriate
format.** See [[provenance-synthesis]].
- Don't summarize away the source — use **claim-source mappings**;
  every claim carries its source.
- **Conflicting credible sources** → preserve **both** values and
  explain context; don't randomly pick one.
- Keep **publication/collection dates**; don't treat older vs newer
  as a contradiction without checking the time frame.
- When a fact legitimately has **multiple valid values over time**
  (amendments, time-series), model the multiplicity + dates — don't
  auto-pick the newest. See [[temporal-data-modeling]].
- Let the **coordinator reconcile conflicts** before synthesis (don't
  hide the conflict upstream).
- Format different content types appropriately (not one generic
  format).

## Continue reading
- **Keep the facts, drop the noise** → [[conversation-context-management]]
- **When to hand off to a human** → [[escalation-patterns]]
- **Back to the map** → [[00-overview]]

## Sources
- [[CCA-F Exam Notes]] — Domain 5, task statements 5.1–5.6.

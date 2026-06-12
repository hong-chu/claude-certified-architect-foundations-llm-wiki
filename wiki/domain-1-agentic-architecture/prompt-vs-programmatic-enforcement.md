# Prompt Instructions vs Programmatic Enforcement

> Prompts suggest; gates enforce. For guaranteed compliance, use a hook/gate, not better wording.

| Dimension | Prompt instructions | Programmatic enforcement (hooks/gates) |
|---|---|---|
| Guarantee | Probabilistic | **Deterministic** |
| Use when | Soft guidance, preferences | Mandatory order, business/compliance rules |
| Mechanism | System prompt, few-shot | [[agent-sdk-hooks]], gates, prerequisite checks |
| Example | "Prefer verifying first" | Block refund > limit; require step N before N+1 |
| Failure mode | Model "forgets" under load | (correct) |
| Who wins | Rarely the right exam answer | **When the rule must always hold** |

## Bottom line
Whenever a scenario says a step is **mandatory**, a rule **must** be
guaranteed, or an action **must never** happen, the answer is a
**hook / programmatic gate** — not "stronger prompt wording," "more
few-shot examples," or "ask the model to check carefully." Prompts
are for soft guidance; gates are for guarantees.

## Sources
- [[CCA-F Exam Notes]] — task statements 1.4, 1.5.

## Continue reading
- **The hook mechanics** → [[agent-sdk-hooks]]
- **Workflow shapes** → [[prompt-chaining-vs-dynamic-decomposition]]

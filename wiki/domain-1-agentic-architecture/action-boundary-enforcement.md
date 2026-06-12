# Action-boundary enforcement

> For money, compliance, or irreversible actions, validate *inside the tool that performs the action* — not in the prompt, and not only in a hook.

This is the **strongest** rung of [[prompt-vs-programmatic-enforcement]].
The exam pushes past "use a hook": for zero-tolerance requirements, the
check must live at the **action boundary** — the tool/server code that
actually executes the operation — so the action *cannot happen* unless
the condition holds.

## The escalation ladder of enforcement
| Strength | Mechanism | Guarantee |
|---|---|---|
| Weakest | Prompt instruction ("always verify consent first") | Probabilistic |
| Stronger | Few-shot examples | Still probabilistic |
| Strong | [[agent-sdk-hooks|Hook]] / interception | Deterministic *checkpoint* |
| **Strongest** | **Validation inside the tool / server-side gate** | Action is *impossible* unless valid |

## When to climb to the top rung
Reach the action boundary whenever a failure is **unacceptable**:
- Money: refund/payment limits, transfers.
- Compliance: identity verification, guardian consent before booking,
  permission checks.
- Irreversible actions: deletes, irreversible state changes.

> Worked exam case: a healthcare agent must not book without guardian
> consent. A **hook** *sounds* right, but the exam wanted validation
> **inside / around the booking operation** so the booking cannot occur
> unless consent is recorded. "Money / compliance / irreversible →
> enforce inside the tool itself."

## How it connects
- It's the concrete form of first principle **B2** (autonomy ∝
  reversibility) and **D1** (mechanisms over wishes). See
  [[01-first-principles]].
- Contrast with [[confidence-calibration]]: a confidence score routes
  review; it never *guarantees* compliance.

## Sources
- [[CCA-F Practice Exam (v1).meta|CCA-F Practice Exam (v1)]] — improvement areas 15 & 19; Principle 2.

## Continue reading
- **Prompt vs gate** → [[prompt-vs-programmatic-enforcement]]
- **Hooks** → [[agent-sdk-hooks]]
- **First principles** → [[01-first-principles]]

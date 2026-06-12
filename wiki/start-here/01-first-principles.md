# First principles behind right vs wrong answers

> The architect builds a reliable system *around* a model that is fallible, stateless, and non-deterministic — while keeping humans accountable for the decisions that are theirs to make.

This page distills the axioms that *generate* the surface-level traps
catalogued in [[03-never-right-answers]]. They are grouped by the thing
they're a fact *about* — the model, the humans, the system, its
information, and its decisions. Each correct exam answer is almost
always restoring one of these principles; each wrong answer violates
one. Learn the axioms and you can *derive* the answer instead of
memorizing it.

---

## A. The nature of the model

### A1 — The model is fallible by design
It can always make a mistake, no matter how good the prompt. Reliability
therefore lives *around* the model: verification, gates, validation,
review.
- **Corollary:** "fine-tune / retrain the model" is **never** the right
  answer here — this is an *architecture* exam; your levers are
  prompting, tools, context, and orchestration, not model weights.
- **Corollary:** "just trust the model" is wrong wherever correctness
  matters. See [[validation-retry-loops]], [[independent-multipass-review]].

### A2 — The model is stateless and its attention is bounded
It remembers nothing between turns except what's in the context window,
and more context is not free — relevance and structure beat volume. So
state must be *managed* (sessions, explicit hand-offs) and context must
be *curated*, not dumped.
- Generates: "use a bigger context window" is rarely the fix; structure
  instead. See [[session-state-management]],
  [[conversation-context-management]], [[large-codebase-context]].

### A3 — The model is non-deterministic
The same input can yield different output. So the model can never be the
thing that *guarantees* an invariant — only a deterministic mechanism
can. (This is the root of principle D1.)

---

## B. Humans and authority — *why there is a human in the loop*

### B1 — Know the boundary of your authority
A human enters the loop not because the model is unsure, but because the
decision is **not the system's to make**: the policy is silent or
ambiguous, the request is explicitly for a human, or the case is outside
the system's defined mandate. The agent's job is to *recognize that
edge* and hand off — never to invent a policy to stay autonomous.
- Generates: escalate on policy/workflow triggers, not on sentiment or
  confidence. See [[escalation-patterns]], [[escalate-vs-clarify-vs-solve]].

### B2 — Match autonomy to stakes and reversibility
The more **irreversible** or **high-impact** an action, the stronger the
human gate before it. Cheap, reversible actions → act autonomously;
costly or destructive ones → confirm first. This is the principle behind
plan mode, confirmation gates, and "propose before you execute."
- Generates: review the plan before a large/destructive change. See
  [[plan-mode]], [[plan-mode-vs-direct-execution]].

### B3 — Consequential decisions must be accountable and traceable
A hand-off to a human (or to a downstream step) carries a **structured
summary**, never a vague one, and the trail of *why* is preserved. The
system stays auditable.
- See [[escalation-patterns]], [[provenance-synthesis]].

---

## C. Designing the system

### C1 — Constrain, don't add
Reliability comes from *shrinking* the decision space, not enlarging it:
fewer, well-scoped tools; explicit criteria; focused agents. Generates
the whole "add more" trap family. See [[04-does-more-improve-reliability]],
[[scoped-tool-access]].

### C2 — Least privilege
Give each agent and tool the *minimum* access it needs. Narrower scope
is both safer and more reliable (fewer wrong choices available). See
[[scoped-tool-access]], [[tool-interface-design]].

### C3 — Decompose into focused units with clear contracts
Hard problems are solved by good *decomposition* — each subagent or
chained step does one thing with an explicit input/output contract — not
by one giant prompt or a swarm of agents. See
[[coordinator-subagent-pattern]], [[prompt-chaining-vs-dynamic-decomposition]],
[[subagent-context-passing]].

### C4 — The right thing in the right place
Adding helps only when it's the *correct* thing at the *correct* layer.
Few-shot fixes format/judgment, not mandatory ordering; rules live at
the matching scope. See [[few-shot-prompting]], [[project-vs-user-scope]],
[[claude-md-hierarchy]].

---

## D. Guarantees and enforcement

### D1 — Mechanisms over wishes
Anything **mandatory** or safety-critical must be enforced
*deterministically* (code, hooks, schema, gates) — never by asking the
model nicely in prose. Prose is for *judgment*; mechanisms are for
*guarantees*. (Follows directly from A3.) See
[[prompt-vs-programmatic-enforcement]], [[05-prompt-fix-or-structural-fix]],
[[agent-sdk-hooks]].

### D2 — Verify, then trust
Check the output before relying on it: schema validation for shape,
semantic validation for meaning, independent passes for high-stakes
judgment. A passing schema means *well-formed*, not *true*. See
[[schema-vs-semantic-validation]], [[validation-retry-loops]],
[[independent-multipass-review]].

### D3 — Fail loud, then recover deliberately
Surface errors as **structured signals**; never swallow a failure or let
a raw stack trace reach the model. Retry only what retrying can fix, and
retry *with the specific error* fed back. See [[structured-tool-errors]],
[[error-propagation]], [[validation-retry-loops]].

---

## E. Information and state

### E1 — Never collapse distinct states
Preserve information; don't flatten two different facts into one.
- *Failure* ≠ *empty result*. See [[access-failure-vs-empty-result]].
- *Well-formed* ≠ *correct*. See [[schema-vs-semantic-validation]].
- *Escalate* ≠ *clarify* ≠ *solve*. See [[escalate-vs-clarify-vs-solve]].

### E2 — Structure over volume
Organize facts; don't just pile more in. A bigger window or longer
transcript never fixes a *structuring* problem. See
[[conversation-context-management]], [[large-codebase-context]].

### E3 — Explicit over implicit
Pass context **explicitly** between agents and across sessions; state
assumptions and what changed; don't rely on hidden shared state. A
resumed session must be told which files moved. See
[[subagent-context-passing]], [[resume-vs-fork]].

### E4 — Preserve provenance
Keep sources traceable; never fabricate or launder where a fact came
from. See [[provenance-synthesis]].

---

## F. Making decisions

### F1 — Triggers are objective, not subjective
Decisions key off observable policy/workflow conditions — not the
customer's mood or the model's self-reported confidence. See
[[escalation-patterns]], [[confidence-calibration]].

### F2 — Match the mechanism to the job's shape
Pick the tool whose *shape* fits the task: batch for latency-insensitive
bulk, sync for interactive; `auto` tool_choice for genuine reasoning,
forced only when the call is certain; resume to continue one path, fork
to branch. See [[batch-vs-synchronous-api]], [[tool-choice-auto-any-forced]],
[[resume-vs-fork]].

### F3 — Iterate with specific feedback
Improvement comes from a tight loop with *concrete* signals — the exact
validation error, the failing case, the diff — not from a vague "try
again" or "be more careful." See [[iterative-refinement]],
[[validation-retry-loops]].

---

## How to use these on a question
1. Find the **root cause** of the failure or the decision the scenario
   hinges on.
2. Ask **which axiom it's about** — the model's limits (A), whose
   decision it is (B), system shape (C), what guarantees it (D), how
   information is handled (E), or how the choice is made (F).
3. The right answer **restores** that principle — it *constrains,
   defers to the right authority, enforces, distinguishes, structures,
   or fits the shape* — while the wrong answers *add, trust, guess,
   collapse, or wish*.

## Sources
- [[CCA-F Exam Notes]] — synthesized across all five domains.

## Continue reading
- **The traps these generate** → [[03-never-right-answers]]
- **When humans decide** → [[escalate-vs-clarify-vs-solve]]
- **Does more help?** → [[04-does-more-improve-reliability]]
- **Exam overview** → [[00-overview]]

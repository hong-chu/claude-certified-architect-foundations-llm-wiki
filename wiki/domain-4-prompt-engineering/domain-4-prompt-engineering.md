# Domain 4 — Prompt Engineering & Structured Output

> **20% of the exam.** Precise criteria, few-shot examples, JSON-schema-enforced output, validation/retry loops, batch processing, and multi-pass review.

Recurring trigger: when output is **inconsistent**, **misjudges
ambiguous cases**, has **false positives**, mishandles **null/empty
fields**, or **ignores the format** → add **2–4 targeted few-shot
examples**. See [[few-shot-prompting]].

> **How this domain is tested** — the cross-cutting [[01-first-principles|method]] as it bites here:
> - **Match the fix to the failure** — vague rule → criteria; inconsistent → few-shot; invalid JSON → schema; wrong values → validation (4.1–4.4). → [[05-prompt-fix-or-structural-fix]] · [[02-root-cause-decision-tree]]
> - **Verify, then trust** — schema = well-formed, *not* true; always add semantic validation. → [[01-first-principles]] (D2)
> - **Prose traps** — "be more conservative", "schema passed so it's correct." → [[03-never-right-answers]]

## 4.1 — Explicit criteria reduce false positives

Vague instructions ("be conservative", "report only high-confidence")
are weak. **Precision improves when prompts define exactly what to
report, what to skip, and how to classify severity** — with
examples. See [[explicit-criteria]].

- Define **report vs skip** categories explicitly.
- High false positives → **reduce scope** or **disable noisy
  categories** so they don't pollute the whole review.
- Define **severity with concrete examples**.

Wrong: "be more conservative"; confidence-only filtering; severity
without examples; reviewing everything (style + security + bugs +
perf + docs + architecture) at once.

## 4.2 — Few-shot prompting

**Few-shot = teach by example.** Good examples help Claude
**generalize**, not just memorize. See [[few-shot-prompting]].

Use a spread: a **positive**, a **negative**, an **edge/ambiguous**,
and maybe a **missing-field/conflict** example. Use it for
inconsistent format, ambiguous judgment boundaries, false positives,
varied document formats, and missing fields.

Wrong: more prose; "be consistent"; raise temperature; add context
but no examples; fine-tune immediately; one example for a complex
ambiguous boundary.

## 4.3 — Structured output via tool use + JSON schema

**Tool use + JSON schema guarantees structure, not truth.** See
[[structured-output]].

- Need guaranteed valid JSON syntax → **`tool_use` + JSON schema**
  (not "please return JSON").
- Schema fixes **format**; **semantic validation** fixes **meaning**.
- **`tool_choice`**: `auto` (optional) / `any` (multiple schemas,
  unknown doc type) / forced (a specific extraction must happen
  first). See [[tool-choice-auto-any-forced]].
- **Optional/nullable** fields for data that may be absent — don't
  make absent fields required (forces fabrication).
- Use **enums with `unclear`** (ambiguous category) and **`other` +
  detail** (unexpected category).
- Add **normalization rules** for source formats.

Wrong: "return JSON" with no schema; `auto` when structure is
mandatory; required absent fields; assuming schema = correctness;
enums with no `other`/`unclear`; skipping validation because JSON is
valid.

## 4.4 — Validation, retry, feedback loops

Valid structure isn't enough. See [[validation-retry-loops]].
- **Retry with the specific validation errors**, not vague "try
  again."
- Give the retry **all three**: original document + previous failed
  extraction + specific errors.
- **Schema (syntax) vs semantic** validation are different layers —
  see [[schema-vs-semantic-validation]].
- **Know when retry won't help**: if the info is **absent**, retry
  won't create truth → return null / escalate.
- Track **`detected_pattern`** (the category that triggered a
  finding, not the issue text) to systematically improve prompts and
  spot false-positive sources.
- Add self-correction **validation fields** that expose
  inconsistencies.

Wrong: retry without saying what failed; retry when info is absent;
"valid JSON = correct"; dropping the failed extraction from retry
context; not tracking false-positive patterns; treating all errors
as equally retryable.

## 4.5 — Batch processing

**Message Batches API = cheaper but slower (async).** See
[[batch-processing]] and [[batch-vs-synchronous-api]].

- Can wait hours / large non-urgent jobs → **batch**.
- User or CI waiting (**pre-merge**: unit tests, lint, security scan,
  type check, Claude review) → **synchronous API**.
- Batch **does not support multi-turn tool calling** inside one
  request — self-contained one-pass jobs only.
- Use a unique **`custom_id`** per request (responses return out of
  order).
- On failure, **resubmit only the failed `custom_id`s**.
- **Test the prompt on a sample** before a huge batch.

## 4.6 — Multi-instance & multi-pass review

**Don't let Claude grade its own homework.** See
[[independent-multipass-review]].
- Same-session self-review is biased (it keeps the generation
  reasoning) → use a **second independent instance**.
- Large multi-file review → **per-file passes + a cross-file
  integration pass** (integration bugs hide between files).
- A **verification pass** can attach confidence/priority — but **do
  not blindly trust** model confidence; calibrate it.

Wrong: same session reviews its own code; extended thinking instead
of independent review; one giant pass; per-file only (skip
integration); trusting confidence without calibration.

## Continue reading
- **Teach by example** → [[few-shot-prompting]]
- **Guarantee the shape, then check the meaning** → [[structured-output]]
- **Next domain** → [[domain-5-context-reliability]]

## Sources
- [[CCA-F Exam Notes]] — Domain 4, task statements 4.1–4.6.

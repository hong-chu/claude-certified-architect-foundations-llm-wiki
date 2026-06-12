# Never-right answers (common exam traps)

> If a distractor says "add more" or "tell it to try harder," it is almost certainly wrong.

**Status:** resolved

## Why I'm asking
The CCA-F exam is mostly single-best-answer items where three of four
options are plausible-sounding traps. Memorizing the *shapes* of the
wrong answers is as valuable as knowing the right one — a flagged
distractor eliminates itself.

## The trap families

### 1. "Add more" (the biggest family)
- **"Give the agent more tools"** → more tools *degrades* selection
  reliability. Right answer is usually [[scoped-tool-access]].
- **"Add more agents / subagents"** → success comes from decomposition
  + explicit context passing, not headcount. See
  [[coordinator-subagent-pattern]].
- **"Use a bigger context window"** → never fixes an *organization*
  problem; structure facts instead. See
  [[conversation-context-management]].
- **"Add more prose ('be careful', 'be conservative')"** → vague prose
  is never the fix. Use [[explicit-criteria]].

See the consolidated argument in [[04-does-more-improve-reliability]].

### 2. Prose instead of enforcement
- **"Tell the model in the prompt to always do X"** when X is
  *mandatory* or safety-critical → prose can't *guarantee* it; enforce
  programmatically. See [[prompt-vs-programmatic-enforcement]].
- **"Add a few-shot example to enforce step ordering"** → few-shot
  fixes format/judgment consistency, not mandatory ordering. See
  [[few-shot-prompting]] and [[05-prompt-fix-or-structural-fix]].

### 3. Sentiment- / confidence-driven escalation
- **"Escalate because the customer is angry"** → sentiment is **never**
  an escalation trigger.
- **"Escalate because model confidence is low"** → self-reported
  confidence isn't a trigger either. See [[confidence-calibration]].
- **"Guess the most likely record when several match"** → never guess;
  **clarify**. See [[escalate-vs-clarify-vs-solve]].

### 4. Error handling
- **"Return an empty result when the tool actually failed"** → collapses
  *failure* into *found-nothing*. See [[access-failure-vs-empty-result]].
- **"Retry a valid empty result"** → "none found" is a real answer.
- **"Swallow the error" / "let the raw stack trace reach the model"** →
  neither; return a structured error. See [[structured-tool-errors]].

### 5. Validation
- **"Schema passed, so the output is correct"** → schema = well-formed,
  not true. See [[schema-vs-semantic-validation]].
- **"Skip validation because we used structured output"** → shape ≠
  meaning. See [[structured-output]].
- **"On failure, just retry"** without feeding back the specific errors
  + original input → blind retry is wrong. See [[validation-retry-loops]].

### 6. API / workflow
- **"Use the synchronous API for a large offline batch"** → use
  [[message-batches-api]] when there's no latency requirement. See
  [[batch-vs-synchronous-api]].
- **"Force a tool for an open-ended task"** → `auto` is correct when the
  model should decide. See [[tool-choice-auto-any-forced]].
- **"Resume the old session"** without stating what changed → never
  trust stale tool results. See [[resume-vs-fork]].
- **"Put a project rule in user-scope CLAUDE.md"** (or vice versa) →
  scope mismatch. See [[project-vs-user-scope]].

## The meta-rule
The exam rewards **constraining over adding** and **mechanisms over
wishes**. When two options say "add more / try harder" and two say
"scope it / enforce it / structure it," the second pair almost always
wins.

## Sources
- [[CCA-F Exam Notes]] — recurring across all five domains.

## Continue reading
- **Does more help?** → [[04-does-more-improve-reliability]]
- **Which kind of fix?** → [[05-prompt-fix-or-structural-fix]]
- **Exam overview** → [[00-overview]]

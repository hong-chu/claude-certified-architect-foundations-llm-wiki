# CCA\-F Exam

This exam tests foundational knowledge across **Claude Code, the Claude Agent SDK, the Claude API, and Model Context Protocol \(MCP\)**

Questions on this exam are grounded in realistic scenarios drawn from actual customer use cases, including building agentic systems for **customer support, designing multi\-agent research pipelines, integrating Claude Code into CI/CD workflows, building developer productivity tools, and extracting structured data from unstructured documents**\. 



# Scope

The ideal candidate for this certification is a **solution architect **who designs and implements production applications with Claude\. This candidate has hands\-on experience with:

- **Building agentic applications using the Claude Agent SDK**, including multi\-agent orchestration, subagent delegation, tool integration, and lifecycle hooks

- **Configuring and customizing Claude Code for team workflows** using CLAUDE\.md files, Agent Skills, MCP server integrations, and plan mode

- **Designing Model Context Protocol \(MCP\) tool and resource interfaces** for backend system integration

- **Engineering prompts that produce reliable** structured output, leveraging JSON schemas, few\-shot examples, and extraction patterns

- **Managing context windows effectively **across long documents, multi\-turn conversations, and multi\-agent handoffs

- **Integrating Claude into CI/CD pipelines** for automated code review, test generation, and pull request feedback

- **Making sound escalation and reliability decisions**, including error handling, human\-in\-the\-loop workflows, and self\-evaluation patterns



## Content Outline

- Domain 1: Agentic Architecture \&amp; Orchestration** 27%**

- Domain 2: Tool Design \&amp; MCP Integration** 18%**

- Domain 3: Claude Code Configuration \&amp; Workflows **20%**

- Domain 4: Prompt Engineering \&amp; Structured Output **20%**

- Domain 5: Context Management \&amp; Reliability **15%**



## Exam Scenarios \(4 out of 6\)

**Scenario 1: Customer Support Resolution Agent **

You are building with the Claude Agent SDK\. 

The agent handles high\-ambiguity requests like returns, billing disputes, and account issues\. It has access to your backend systems through custom MCP tools \(`get\_customer, lookup\_order, process\_refund, escalate\_to\_human`\)\. 

Your target is 80%\+ first\-contact resolution while knowing when to escalate\. 

> Primary domains: Agentic Architecture \&amp; Orchestration, Tool Design \&amp; MCP Integration, Context Management \&amp; Reliability
> 
> 



**Scenario 2: Code Generation with Claude Code **

You are using Claude Code to accelerate software development\. 

Your team uses it for code generation, refactoring, debugging, and documentation\. You need to integrate it into your development workflow with custom slash commands, CLAUDE\.md configurations, and understand when to use plan mode vs direct execution\. 

> Primary domains: Claude Code Configuration \&amp; Workflows, Context Management \&amp; Reliability
> 
> 



**Scenario 3: Multi\-Agent Research System **

You are building a multi\-agent research system using the Claude Agent SDK\. 

A coordinator agent delegates to specialized subagents: one searches the web, one analyzes documents, one synthesizes findings, and one generates reports\. 

The system researches topics and produces comprehensive, cited reports\.

> Primary domains: Agentic Architecture \&amp; Orchestration, Tool Design \&amp; MCP Integration, Context Management \&amp; Reliability
> 
> 



**Scenario 4: Developer Productivity with Claude**

You are building developer productivity tools using the Claude Agent SDK\. 

The agent helps engineers explore unfamiliar codebases, understand legacy systems, generate boilerplate code, and automate repetitive tasks\. It uses built\-in tools \(`Read, Write, Bash, Grep, Glob`\) and integrates with MCP servers\. 

> Primary domains: Tool Design \&amp; MCP Integration, Claude Code Configuration \&amp; Workflows, Agentic Architecture \&amp; Orchestration
> 
> 



**Scenario 5: Claude Code for Continuous Integration **

You are integrating Claude Code into your CI/CD pipeline\. 

The system runs automated code reviews, generates test cases, and provides feedback on pull requests\. You need to design prompts that provide actionable feedback and minimize false positives\. 

> Primary domains: Claude Code Configuration \&amp; Workflows, Prompt Engineering \&amp; Structured Output
> 
> 



**Scenario 6: Structured Data Extraction **

You are building a structured data extraction system using Claude\. 

The system extracts information from unstructured documents, validates the output using JSON schemas, and maintains high accuracy\. It must handle edge cases gracefully and integrate with downstream systems\. 

> Primary domains: Prompt Engineering \&amp; Structured Output, Context Management \&amp; Reliability
> 
> 





---

# Domain 1\. Agentic Architecture \&amp; Orchestration

> How to design agent workflows that can act, delegate, enforce rules, and manage state safely\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ODQ4MGQ5YTBjNDJmZWNhMGNlNjM0YjA0MzdhNWZlZmZfMTdhZjc5OGMxOTI0YjYwOWZmNzA2NDAyMzcwOGJlZWRfSUQ6NzY0NDQ2MTAxMDgyODkzODk3Nl8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)

## Task Statement 1\.1: Design and implement agentic loops for autonomous task execution

> **Shortcut: **
> 
> 1. **Agent loop = stop\_reason\-driven, not text\-driven\.**
> 
> 2. **Tool results must be appended back** into the conversation history before asking Claude what to do next\.
> 
> 

**Correct:**

Claude does not magically execute tools by itself\. 

The application code must run a loop: send message → check `stop\_reason` → execute tool if needed → send tool result back → repeat until Claude finishes\.



**Wrong:**

Avoid these anti\-patterns: parsing natural language to decide loop termination, using arbitrary iteration caps as the main stopping mechanism, or checking whether assistant text exists as a completion signal\.  

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZDE1OWI5MjQxZmVjOWU0NWI3ZTdkMzcyNjFkMmZhYzVfNDE4YWUxMmY3MGIwM2JkYWI3MjEyNWQ4NmIwYTIxNWVfSUQ6NzY0NDM3NTM0ODc0MTcyMTgyNl8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZTU0OTgxYzBmMjZiYmJhZjNiZmE0ZWNmZjE2ZDI2MDJfODU2ZTU4M2ZjMmIxODc2ODkxMTAyYzMxYTY3ZTVmN2JfSUQ6NzY0NDM3Mzc1Mjk5ODgyNTY5MV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)





## Task Statement 1\.2: Orchestrate multi\-agent systems with coordinator\-subagent patterns

> **Shortcut:**
> 
> **Coordinator owns decomposition, routing, aggregation, gap detection, and error handling\. Subagents are specialized workers with isolated context\.**
> 
> **Multi\-agent success depends less on “more agents” and more on good coordinator decomposition, explicit context passing, and gap checking\.**
> 
> 

**Correct:**

1. For multi\-agent systems, it should be a **coordinator\-subagent pattern** for: 

    1. **Observability**

    2. **Consistent error handling**

    3. **Controlled info flow**

P\.S\.: Decides which subagents to invoke **based on query complexity**

2. The coordinator is responsible for **task decomposition, delegation, result aggregation, gap detection, re\-delegation, and final synthesis\.**

3. Subagents do not automatically know everything the coordinator knows\. You must **pass that information explicitly\.**



**Wrong:**

- Overly narrow decomposition \- didn\&\#39;t cover all the subtasks

- Not every task needs every subagent

- The coordinator should not just accept the first synthesis if it is incomplete

- Let subagents communicate freely with each other

- Assume subagents inherit coordinator memory automatically

- Ignore gap detection after synthesis

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=OWI0Y2RkZmMwMDhkOWRmYzQzODFlYjk0MzE4YWYzZGRfMTk5Yzg4MTcxNTVkOGE4OGFmOWQ1OGVkZDFkODgxZjhfSUQ6NzY0NDM3Njk5MTY2NDk1MTAwOF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MWYxNmZiMDI3NGVhNTk3NjA5YjIyMzlkYjcwOTg0ZTZfODcyYjJiZTJlNDQ4MDg3MjA3NGEyYTI4NTIwNjc2YmNfSUQ6NzY0NDM3ODYyOTUyMjM5NDg0N18xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)





## Task Statement 1\.3: Configure subagent invocation, context passing, and spawning

> **Shortcut:**
> 
> **Task tools spawn subagents; explicit context feeds subagents; structured metadata preserves attribution; parallel Task calls improve latency\.**
> 
> 

**Wrong:**

- Subagents automatically inherit coordinator context\.

- The coordinator can spawn subagents without the Task tool\.

- Pass vague summaries without source metadata\.

- Spawn independent subagents one\-by\-one across separate turns\.

- Give all subagents the same tools\.

- Force subagents to follow rigid step\-by\-step procedures instead of goals and quality criteria\.

- Let subagents share memory implicitly\.



### **The Task tool is how you spawn subagents**

> **No Task tool allowed = coordinator cannot spawn subagents\.**
> 
> If error \-\&gt; Add `\&\#34;Task\&\#34;` to `allowedTools`\.
> 
> 



### **Subagents do not inherit context automatically**

> **Subagent context must be passed explicitly**\. Be more specific\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MmNlZjhiNzljYzNhNTU2YmQwZTRkZGZhMWMzMjY1YzVfOGE2YWM5ZDJlYzU2MDljMGZkMzc1MjE0OTJhMDQyMTZfSUQ6NzY0NDM4MjkxNTI5MTI4NzI2Nl8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **AgentDefinition = role \+ instructions \+ tools**

> `AgentDefinition` is basically the configuration \(job description \&amp; permissions\) for each subagent type, including:
> 
> - **description**
> 
> - **system prompt**
> 
> - **tool restrictions**
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MTJjYWMzZGQ1MmZiYmI5Y2Y1NDJkMjNmYTgxY2E3NzVfNGE1NjNhMDhjZDM0MjQxYjJlZGE1OTcxNWVhZDRjNDFfSUQ6NzY0NDM4MzQ2MDc0ODIwMTY5NF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Pass complete prior findings directly**

> **Include complete findings** from prior agents directly in the subagent’s prompt, such as passing web search results and document analysis outputs to the synthesis agent\. 
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=YzY3Nzc1MDEzNDE0YzRmZmE3YTQ0Mzg4ZDgzNzExOWJfOWU3MWYxODRjZDJiZjM1MmQxZWY0YWVkMTAwZmYwMTJfSUQ6NzY0NDM4Mzg4MjI2MzAzOTcxMl8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Use structured format when passing context**

> **Pass content \+ metadata, not vague summaries \(exam\-heavy\)\.**
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=OTJlZmIzNTAyMDhhMTM1ZmI4N2Y0OWNkYmFmZTg0MWNfNDZhMWRiYzEwN2E1NGYwN2U5NzQ5MGJkMjNkMTk3ZmRfSUQ6NzY0NDM4NDIwMzA2MTc3NjA5M18xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Parallel subagents = multiple Task calls in one coordinator response**

> For independent subtasks, spawn subagents in parallel using multiple Task calls in one response\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=Y2M3MGE1NWE2YTU1Mzk2NjVjMjA0YzM1ZjVjYzhlODNfZTNjMTQwZTA2ODJkMGJjNTA3YzUzNGM4MmY4Zjk5Y2VfSUQ6NzY0NDM4NDU5NTM4NzI3MjkyNF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Coordinator prompts should specify goals, not rigid step\-by\-step instructions**

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=M2JmYzM5Yjk5NjUwNzkyMzIyMDYwMjcxMmQyMTZlZDlfNzVjYTVlMjk1NDNmNzk3MDAxOTBmMzQ0NzcyODBiZThfSUQ6NzY0NDM4NDcxODM4NTAwODM1NF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Fork\-based session management**

> Use fork when exploring divergent approaches from the same baseline\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MThkOGU3MjIyOWZhMjNkYzhjMjMwMTRmYjRiMjM2NWVfYzI5NjU3ZDU1MzJjYTc4ZWQ0MDI4ZmVlOTk0YTUzMTRfSUQ6NzY0NDM4NTA0NDUzODA4NTA4NF8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)





## Task Statement 1\.4: Implement multi\-step workflows with enforcement and handoff patterns

> **Shortcut:**
> 
> **For critical workflow order, prompts suggest; gates enforce:**
> 
> - **Mandatory order → programmatic gate\.**
> 
> - **Multiple issues → decompose, investigate, synthesize\.**
> 
> - **Escalation → structured handoff summary\.**
> 
> 

**Correct:**

- When the workflow has required steps, do not rely only on the model “remembering” the right order\. Enforce critical steps programmatically\.

- Choose **hooks / gates / prerequisite** checks over better prompt / more examples / model confidence\.



**Wrong:**

- Add stronger prompt instructions for required compliance steps\.

- Add more few\-shot examples when the issue is mandatory tool order\.

- Use model confidence to decide whether to skip verification\.

- Escalate with a vague summary\.

- Investigate only the first issue in a multi\-issue customer message\.

- Let the human agent read the full transcript instead of preparing structured handoff\.

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NDYwNDNiOGU1MDlhZTAyYmM4NmM0ZTkwNDdiYmRmOTZfYTU1OTA5Y2Q5OGVmNmM0MjA5ZTNjOWNlMDYwNTQwMjhfSUQ6NzY0NDQ0MzUyNjA3NTczMTY4NV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=N2UyZGI2N2RkNzczNDE3MDk4NzZmNDI2NDQxZTAxMmRfZWIyMmE1Y2I5NWFkZjM5ZGQ3ZjdlZjMwMTRmMDZlMWJfSUQ6NzY0NDQ0MzY3OTAzMzMxNTAzNl8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=YTFiYjMxNDgzN2I4NzgzNDM2ZWM0NTU4ZmIyM2Q0NTdfZGI2MTUxZTE1ZTdlYzFlYmM3OTc3ZDAwMWU5ZWY3ZTVfSUQ6NzY0NDQ0NDA5NzQ1NjkzNDYyOV8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MWY0YzFhZGE1MDFhMTRmMWYyNGI5NmQyZGFmYzc1NjBfNzA0Y2QyZmU3ZGFhZTc4MTM4Nzg5YWQ1ODM2YTkyYWNfSUQ6NzY0NDQ0NDMxOTU2NDAwOTE4Nl8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)





## Task Statement 1\.5: Apply Agent SDK hooks for tool call interception and data normalization

> **Shortcut:**
> 
> **Hooks are deterministic checkpoints: normalize tool results after use, and block risky tool calls before execution\.**
> 
> 

One way to implement that enforcement \(in 1\.4 for critical workflow order\) is with **Agent SDK hooks **\(1\.4 is the principle; 1\.5 is one implementation mechanism\)

**Choose hooks over prompt\-based enforcement** when business rules require guaranteed compliance\. 

- Prompt instruction = probabilistic

- Hook enforcement = deterministic

### **Hook**

A **hook** is code that runs automatically at a certain point in the agent workflow\.

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=YWU5MzU3ZjkzOTY3MTRjNzM4YjQ4N2UxYmUwMmZiM2NfZWUxYmQyZjRiMjFlNTcxMDJmMDQ1YWVlMDliOWZlNjZfSUQ6NzY0NDQ0NzY4ODg1MDg2OTk4NF8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZjQzMWI2YTRkNmM1ZTAwZTZlM2QxMmZlMTY4NTZmMjRfZjVmMGU2NDgwOGNlMjQ0NjFhZTU2NmE1NWMyOGQ2MTRfSUQ6NzY0NDQ0NzU4Nzc4MjI5OTM1NV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Two main hook types**

#### **1\. PostToolUse hook**

> **PostToolUse = clean/normalize tool results before Claude sees them\.**
> 
> 

This happens **after a tool returns a result**, before Claude uses that result\.

Use it to** normalize messy tool output**\.

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZWQwYjNkYTVhM2IyNDI3MGI4YTQ3ODEwZGE1MmU4MTFfMjRkZDNjM2FmNzUwZDZhNjA1MTgxODNkNTA1NmU1MzNfSUQ6NzY0NDQ0ODgzMzU4MTE0MTcyNF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



#### **2\. Tool call interception hook**

> **Tool interception = block or redirect risky tool calls before they happen\.**
> 
> 

This happens **before a tool call is executed\.**

Use it to **block** policy\-violating actions\.

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MDVmZWQ2N2M3ZmE4Y2FmNWEzNTAyOTg5ZTg1YTQ3MzlfZGQxNmE1N2MzMzM1ODk5MDA1ZTllNGRlMTAwMTdjZGFfSUQ6NzY0NDQ0OTUzOTU4NzYyNDY3N18xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Common exam patterns**

**Pattern 1: Inconsistent tool data format**

**Problem: **

Different MCP tools return dates/statuses in inconsistent formats, causing Claude to misinterpret order status\.

**Solution:**

Use `PostToolUse` hooks to normalize tool outputs before Claude processes them\.



**Pattern 2: Risky tool action**

**Problem: **

Agent sometimes refunds above policy limit despite prompt instructions\.

**Solution:**

Use a tool call interception hook to block refunds above threshold and redirect to escalation\.



**Pattern 3: Business rule guarantee**

**Problem: **

Compliance requires guaranteed prevention of certain action\.

**Solution:**

Use hooks/programmatic enforcement, not just prompting or few\-shot examples\.





**Correct:**

- Use `PostToolUse` hooks to normalize tool outputs before Claude processes them\.

- Use a tool call interception hook to block refunds above threshold and redirect to escalation\.

- Use hooks/programmatic enforcement, not just prompting or few\-shot examples\.



**Wrong:**

- Add stronger prompt wording\.

- Add more few\-shot examples\.

- Ask the model to check the refund amount carefully\.

- Use self\-reported confidence\.

- Let Claude decide whether policy applies\.

- Normalize data in the final natural language response instead of before the model reasons over it\.





## Task Statement 1\.6: Implement multi\-step workflows with enforcement and handoff patterns

> **Shortcut:**
> 
> - **Known steps → prompt chain\. **
> 
> - **Unknown path → adaptive decomposition\. **
> 
> - **Large review → local passes \+ integration pass\.**
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=YTI1OWZkYzc0OWNkMTc3ODhlZmM4YTk2MmU5MDY4MzVfNjk3NzEzMDk1NTI1ZTIyOTg4YjVhYjU5YmFkMjdmOGNfSUQ6NzY0NDQ1NTQxMzI4NzA5NjAzMl8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)

**Wrong:**

- use one giant prompt for a large complex review

- force a fixed pipeline when the task is exploratory

- use dynamic agents when the workflow is simple and predictable

- skip cross\-file integration analysis in large PR reviews

- make a full implementation plan before exploring unknown dependencies



### **Fixed sequential pipeline / prompt chaining**

> **Predictable task → prompt chaining\.**
> 
> 

Use it when:

- the workflow is predictable;

- steps are repeatable;

- you know the order;

- you want consistency\.

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=YTZkYjliM2FmZTUxMWQ0YWZlN2ZhNWNhOTEzYmJkOWNfM2E1MjJmYWJlMDE4Yjk3MWQ1ZGE0NDM5M2UxNTA2NTBfSUQ6NzY0NDQ1NDI3MDk2ODIyMTQwN18xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **Dynamic adaptive decomposition**

> **Open\-ended investigation → dynamic decomposition\.**
> 
> 

Use it when:

- the task is open\-ended;

- dependencies are unknown;

- the system needs investigation;

- the next step depends on intermediate findings\.

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZGNlYjdmMGY3OWExNGQyYWNmNjMzNTg3YmE1ZGM5MDNfNzkxMjFjZjI0MmRiYWU5NTViNjI3OTI4OTRhOGI2YjNfSUQ6NzY0NDQ1NDcyMTcyNTQ4NDc2M18xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MTQ4MGU4MDQ4ZmE4OWRiZTMzNjY4NmE5ODNiNWRlZWFfN2ZjMzJlOTU3ZTFlYTA3Yjk1MDA3NjI2MjkwM2U0ODhfSUQ6NzY0NDQ1NTA1Mjc3NTM0NTg4NV8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)





## Task Statement 1\.7: Manage session state, resumption, and forking

> **Shortcut:**
> 
> - **Prior context still valid → resume\.**
> 
> - **Multiple possible approaches → fork\.**
> 
> - **Resume \+ changed files → tell Claude exactly what changed\.**
> 
> - **Prior context stale/noisy → fresh session \+ structured summary\.**
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=YzYzYjMyMGIwODM4ZWY2NGYzNTI3NjU4MGNlYjFiNWJfYWU0NGYyNjljYjQ0OWE2OGQxMGZhYzI0YmQ3NTIwNmRfSUQ6NzY0NDQ1ODU0MzYyMTUyNTIxNV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)

**Wrong:**

- Always resume because more context is always better\.

- Always start over and discard useful context\.

- Resume without telling Claude which files changed\.

- Use fork when you actually need to continue the same path\.

- Use one session to explore mutually incompatible strategies\.

- Trust old tool results after code has changed\.



### **1\. ****`\-\-resume`****= continue a previous named session**

> Use `\-\-resume \&lt;session\-name\&gt;` when you want to continue a specific prior investigation\.
> 
> 

```Plain Text
claude --resume refund-flow-investigation
```

Use this when:

- the previous context is still mostly valid

- the files have not changed much

- you want to continue the same line of reasoning

- the agent already built useful understanding



### **2\. ****`fork\_session`****= branch from a shared baseline**

> Use `fork\_session` when you have a shared analysis baseline but want to explore different possible approaches independently\.
> 
> 

```Plain Text
Baseline:
Claude analyzed the codebase and found two possible refactor strategies.

Fork A:
Explore incremental refactor.

Fork B:
Explore full module rewrite.

Fork C:
Explore compatibility-wrapper approach.
```

Use this when:

- the previous context is still mostly valid

- the files have not changed much

- you want to continue the same line of reasoning

- the agent already built useful understanding



### **3\. Tell resumed sessions what changed**

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZmE3ZmI4ZWU2OTc2NDVhNzY1MTE1N2M4YWI3NjFhZjZfZDM4Y2Y4NmViMTlkZmM4N2ZlMGUxNjA1NjA4NTc5ZjJfSUQ6NzY0NDQ1Nzc3MTA3NjczNDY4Nl8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **4\. Sometimes fresh session \+ structured summary is better**

> If the old session contains stale tool results, too much noisy context, or outdated assumptions, resuming may make the agent worse\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=YzJjOWZiYTgxMWMyOWIwNGFlNWMwNWU5YjU3MWEzNTBfM2I2M2QzYzU2YTNhYmM0MWVkZjk4MzYxMjcxOGRiNDNfSUQ6NzY0NDQ1ODExNTE1MjM2NzMyNl8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



---

# Domain 2\. Tool Design \&amp; MCP Integration

Claude chooses tools mainly based on the **tool name \+ tool description**\. If the descriptions are vague or overlapping, Claude will call the wrong tool\.

## Task Statement 2\.1: Design effective tool interfaces with clear descriptions and boundaries

> **Shortcut: **
> 
> **Claude does not automatically know what your internal tool does\.**
> 
> **Claude picks tools by name \+ description\. Clear boundaries beat vague generic tools\.**
> 
> 

**Correct:**

Claude does not magically execute tools by itself\. 

The application code must run a loop: send message → check `stop\_reason` → execute tool if needed → send tool result back → repeat until Claude finishes\.



**Wrong:**

Avoid these anti\-patterns: parsing natural language to decide loop termination, using arbitrary iteration caps as the main stopping mechanism, or checking whether assistant text exists as a completion signal\.  



### **Tool descriptions are like API documentation for Claude**

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=OGU1YjZhZjE2YjZjZDlhYzllOWE5NjFlYmI0ZmY2YmZfNjhjZDZlNDcyZTU1Zjg2YzM1YTM5OTdjOWM4YmRlZGNfSUQ6NzY0NDQ2MTc5MzAzNDQ1Njc5OF8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **A good tool description should include boundaries**

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MjY4YjZhNWVmYmExZGZlYzgzMDUyODY5ZDYwOWU4NzJfNGU0MzBiYWM3MWU0MjlmMmZlYzQ5NmMzYjRmMmI1OGNfSUQ6NzY0NDQ2MzQxNDExODc4MDYzOV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Avoid overlapping tool descriptions**

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ODgzYzJjMGMyNDBhNmQ2ODBkMzI0MjFjZWY1ZWRhMGFfNWYzMjE2Y2M3NDQwMzE3MTY0ZjcwM2RlODljNmM4YzRfSUQ6NzY0NDQ2MzY3MjY4NDk5MDE3NV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Rename vague tools**

> Vague tool name → rename it to match its real purpose\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=YjQ4NzAyNTA1Y2Y1NGNhODVkOTZhN2Q1Mjk2ZGNmOWJfYWNjZWQ4MzViNDRjM2M2NjdmYzMxODlkYjYyN2MxZGVfSUQ6NzY0NDQ2NDEyODk3NDczNzExOV8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **Split generic tools into purpose\-specific tools**

> Generic overloaded tool → split into purpose\-specific tools\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZmQ0NTA2ZTU3ZjgyNmM3MWVlZDI5ZThhOWI2ZDdhNGNfYTNhY2ZkNWZkNDZiMjE0MWFlZDYzYzQzMTVkMGFmYjZfSUQ6NzY0NDQ2NDM3ODY5OTQzNTc0Nl8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **Watch out for system prompt wording**

> If tool selection is weird, check both tool descriptions and system prompt wording\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MDdjZDNjYWI2MDhkODAzN2RlZGQ0Mjg1MWRmMzM5YjFfZTc1ZjRmZGI2NTE3YjY2YTNlZWY5Zjg5OTVkY2JlNDZfSUQ6NzY0NDQ2NDY3MzIzMTg1MTIzNF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Common exam patterns**

**Pattern 1: Wrong tool selected between similar tools**

**Problem: **

Claude confuses `get\_customer` and `lookup\_order`\. Both have minimal descriptions\.

**Solution:**

Improve tool descriptions with purpose, input format, examples, edge cases, and boundaries\.



**Pattern 2: Two tools overlap**

**Problem: **

`analyze\_content` and `analyze\_document` are frequently confused\.

**Solution:**

Rename and clarify tool boundaries, or split tools by purpose\.



**Pattern 3: One generic tool is overused**

**Problem: **

Claude calls `analyze\_document` for summarization, extraction, and verification, but outputs are inconsistent\.

**Solution:**

Split into purpose\-specific tools with clear contracts\.



**Wrong:**

- Add more tools without clarifying descriptions\.

- Use a routing classifier as the first fix\.

- Fine\-tune the model to choose tools\.

- Consolidate all tools into one generic tool\.

- Add vague prompt instructions like “choose the correct tool\.”

- Ignore system prompt wording\.





## Task Statement 2\.2:  Implement structured error responses for MCP tools

> **Shortcut: **
> 
> **Generic errors make agents dumb\. Structured errors let agents retry, ask, explain, escalate, or continue with partial results\.**
> 
> **Tool error should answer:**
> 
> 1. **What failed?**
> 
> 2. **Why did it fail?**
> 
> 3. **Is it retryable?**
> 
> 4. **What should happen next?**
> 
> 5. **Are there partial results?**
> 
> 



**Correct:**

Making tool failures understandable to the agent\.

A tool should not just say “failed\.” It should say **what kind of failure happened**, whether it is **retryable**, and what the agent should do next\.

Generic error responses like `\&\#34;Operation failed\&\#34;` prevent the agent from making appropriate recovery decisions\.



**Wrong:**

- Return generic `\&\#34;Operation failed\&\#34;`\.

- Retry every error\.

- Never retry any error\.

- Treat empty results as failures\.

- Hide partial results from the coordinator\.

- Immediately terminate the whole workflow on a subagent timeout\.

- Suppress failure and return empty successful results\.



### **MCP ****`isError`****flag**

> In MCP, a tool can return an `isError` flag to tell the agent:
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NTNjYmQ4OTMxMjFhMzViN2UyNGZhZGMzZDExMjc2ZmVfNzAzN2QyYmQyNTBiZDU4ZmVkYzA4NGVjYTI0ZGNhZWJfSUQ6NzY0NDQ2NzYxNTIxMzQ0MDczM18xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Different error types need different actions**

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MGY3Y2ViMDA2YWViNWZjZDMxMGFiZjg0NGI2MjU3MjRfMWYwNDA5ODZlY2EyYTk2M2EyYjllNTRlYjgzOTQ2N2JfSUQ6NzY0NDQ2ODEwMTY3MjgxNjM1MV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Retryable vs non\-retryable**

> Retryable = maybe it will work if we try again\.
> 
> Non\-retryable = trying again will waste time\.
> 
> Structured metadata prevents wasted retry attempts\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MzQxMDNiNWI4MzFhNzA3YmI5ZDliZTBlMDFhNjQyMDVfZDY3N2ZkZDc2NmY0MmE3MjljZWNmODlkM2E5OWUwY2JfSUQ6NzY0NDQ2ODc2NzM5NDM4NTYzMF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=YTkyNWY4ZmQ1ZTVhODhkNjZkZDgyZTkxMjZjZDE2NmVfZWMwMGQ3MTk0NjZjMGM0MWI2NTczMjk2ZDAwZWViODRfSUQ6NzY0NDQ2ODgwNjI1Mjg3NTQ4NV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Business rule violations need customer\-friendly explanations**

> Business rule violations should include `retriable: false` flags and customer\-friendly explanations so the agent can communicate appropriately\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NGE4YzE2MjY3YzYyM2M3ZGFkY2MxN2YzY2UyOGJjOGNfNDk0NmZiMDg2NzBhYmQ4MjQwZTQ5OWQ4YjQ4Y2JkN2RfSUQ6NzY0NDQ2OTAzMDA0ODQ1MjMyMF8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **Subagents should recover locally when possible**

> In a multi\-agent system, not every small error should immediately go to the coordinator\.
> 
> Subagents should implement local recovery for transient failures and propagate only unresolved errors, with partial results and what was attempted\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MjE5ODQ2N2ZmNDUzMzUxZmYyYzVlYzY2MGQ5NDg0MDVfMDM2ZDIxOTQ3MzJkMWYzMDkwZWY4YjEwNzRkMzFkMGFfSUQ6NzY0NDQ2OTQzNjY4NjM3MjU3MV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Empty result is not always an error**

> Distinguish access failures from valid empty results\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MTg5YzAzZTEzNmU3Y2E0MWMyMDM2Yjc2YjFiMjEzOTdfZTkzMzdlOWYwN2EzMjQ0MjFjYTU3NjU0MjRlZmMyYmRfSUQ6NzY0NDQ3MDIzNzcxMzc0NzY3OV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)





## Task Statement 2\.3:  Implement structured error responses for MCP tools

> **Shortcut: **
> 
> More tools ≠ better\. Too many tools make Claude less reliable at choosing the right one\. It degrades tool\-selection reliability by increasing decision complexity\.
> 
> Agent role determines tool access\.
> 
> 

### **Use scoped tool access**

> Agents with tools outside their specialization tend to misuse them, such as a synthesis agent attempting web searches\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MTFlYmMxNWRkNjNhNDMwMzVjYzdlMTNhN2E5ZGQyMDVfZThiYjQ4ZjllZjljZmI5YjJjZjI5MThiNDc0M2Q3NjNfSUQ6NzY0NDQ3NzI2NzEzMjk3Njg2MF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Avoid cross\-specialization misuse**

> Each subagent’s tool set should be restricted to tools relevant to its role to prevent cross\-specialization misuse\.  
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NWMyN2IzNTc0MmQyM2Y5Njg1ZGJhYWM2ZjI3MmRkZjVfMDVhZjJlYmM1ZGM3ZWE1MWFhZTFjZDc4MWI3YzlmNzhfSUQ6NzY0NDQ3NzQ3OTk4NTQwMTU2OF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Use constrained tools instead of broad generic tools**

> Broad generic tool → replace with constrained role\-specific tool\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=YTdhZGQ0NmJiNzBjNGI4YzA5NTdhM2UyZTJiZTc3ZGVfMjFkYzEyMzYwYTI5MTJjYjIzODlkMjBiNDZjOGIyYjZfSUQ6NzY0NDQ3NzcxNDE3MDQ5ODc3OV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Scoped cross\-role tools are okay**

> Small frequent need → narrow cross\-role tool\.
> 
> Complex need → coordinator\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZTkxNWU5Y2MyOTViMzUyZjUxYTA1M2FhMzNkZjI0NGFfM2YxNWVlYWJhNjFlYzVjMzg0N2U5ZjZjYjk0MzE0OTNfSUQ6NzY0NDQ3Nzk4OTA0NDE2MjI3Ml8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **5\. ****`tool\_choice`****: auto vs any vs forced**

> Give agents the few tools they need, not all tools they might possibly use\. Use auto/any/forced based on how required the tool call is\.
> 
> auto = model decides whether to use a tool\.
> 
> any = must use a tool, model chooses which\.
> 
> forced tool = must call a specific tool\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZDhiNzk2YzVlZTA3ZGI1MjRjYjdhNTRiZDZmYTRiNzZfODE3NzY1MTc5MWJkMWUwNzc4OGVkM2MxMTM1YmY0MTlfSUQ6NzY0NDcwNjgxODc5NDUyNDM3OV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)





## Task Statement 2\.4:  Integrate MCP servers into Claude Code and agent workflows

> **Shortcut: **
> 
> Project MCP config is shared; user MCP config is personal; env vars protect secrets; resources provide maps; tools perform actions\.
> 
> 

MCP servers are how Claude gets access to external tools/resources\. You need to configure them at the right scope, manage credentials safely, and expose useful resources so Claude does not waste tool calls exploring blindly\.

**Wrong:**

- Put shared team MCP config only in `\~/\.claude\.json`

- Commit API tokens directly into `\.mcp\.json`

- Build a custom MCP server for Jira before checking existing options

- Expose everything only as tools when a resource catalog would reduce exploration

- Assume only one MCP server can be active

- Ignore tool descriptions and wonder why Claude uses `Grep` instead of MCP search

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MmE5NWU0MWNmZmU0NzAzYzMwNmJlM2FmMjMzMGJjOThfZDMxNGExOTFlOTMzYzE1YTJiZTVhY2IyYTcwMGJkYjVfSUQ6NzY0NDcyOTg5MjAyMTY1MzIxNF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Project\-level vs user\-level MCP config**

> Project\-level: \.mcp\.json
> 
> User\-level: \~/\.claude\.json
> 
> 

project\-level `\.mcp\.json` is for shared team tooling, while user\-level `\~/\.claude\.json` is for personal or experimental servers\.



### **Use environment variables for secrets**

> Shared config can be committed\. Secrets should come from environment variables\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZWVhMDJkNWQ4NDAwNThkMzZkYWViMjcyYjVhMmM0OTVfYjVjNWFmM2Q1ZWZhYTZkM2JlZmI5MWY0ZTg3YjkwYTlfSUQ6NzY0NDcyMzUwNTA3MzgwMjk3OF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Tools from all MCP servers are available together**

> If multiple MCP servers are configured, Claude discovers their tools at connection time\.
> 
> However, too many available tools can confuse tool selection, so descriptions and scope matter\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NGJlNDZhMDQ0NjBlMDA0ODBjMmFlNzAxZGQwYzcyZGFfODc0ZWVmYmI4ZmYwNDBiNTdiYmJhZmQyMDliMjhmNDVfSUQ6NzY0NDcyNDU4NDk5ODAzMTA2OF8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **MCP resources = catalogs / read\-only context**

> Resources give Claude a map\. Tools let Claude take action\.
> 
> - Tools = actions Claude can call
> 
> - Resources = content/catalogs Claude can inspect
> 
> 

MCP resources can expose content catalogs, such as issue summaries, documentation hierarchies, and database schemas, to reduce exploratory tool calls\.

Examples of MCP resources:

- issue summaries;

- documentation hierarchy;

- database schemas;

- available report list;

- API endpoint catalog;

- data dictionary\.



### **Improve MCP tool descriptions**

> Enhancing MCP tool descriptions can prevent the agent from preferring built\-in tools like `Grep` over more capable MCP tools
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZmJhODRmNGM5NjA5YzRkMjM3ODU2OGZmNWE1YWEzZDFfZDkwZDg5OTBhODRmYTg3NDAyYzc1OWI4MzE0MWQzZmVfSUQ6NzY0NDcyODU2NDc1NTA0MjAxNl8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Use community MCP servers for standard integrations**

> If the integration is common, like Jira, use an existing community MCP server when possible\.
> 
> - Standard integration → existing MCP server\.
> 
> - Team\-specific workflow → custom MCP server\.
> 
> 





## Task Statement 2\.5:  Select and apply built\-in tools \(Read, Write, Edit, Bash, Grep, Glob\) effectively

> **Shortcut: **
> 
> **Grep finds text inside files; Glob finds files; Read inspects; Edit patches; Write replaces/creates; Bash executes\.**
> 
> 

Use the right built\-in tool for the job: **Grep searches content, Glob finds files, Read/Write handle full files, Edit changes specific text, Bash runs commands\.**

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MDczOTllMzg0NzYzMTg4NjgyOTM3YzQ1MDQzN2QzZWRfYWY0MzQ1YTk5YThmYWUwYTIzZTMzNjZmNmMwN2FlODhfSUQ6NzY0NDczNTUyMjM4ODA4NjQ5NV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)

**Correct:**

- Glob finds files\.

- Grep finds text\.

- Read understands files\.

- Edit patches unique text\.

- Write replaces/creates files\.

- Bash executes commands — use carefully\.



**Wrong:**

- use Glob to search for function names inside files \(Grep\)

- use Grep to find files by extension \(Glob\)

- read the entire codebase upfront \(Grep \+ Read\)

- use Edit when the target text is not unique \(Read \+ Write\)

- use Write for a tiny change when Edit would be safer

- run Bash before understanding the repo \(be careful\!\!\!\)

- modify files before tracing imports and callers \(Search → Read → Trace → Modify → Test\)



### **Grep = search inside file contents**

> Looking inside files → Grep
> 
> 

Grep is for content search, like searching file contents for function names, error messages, or import statements

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MDc5NzM4NDNhNTE1MDFlNWEyOWVlOTFiNDJhNDFiYTVfNmJlZDBjMjNmY2JiNTAzZGJhNWZiMTY5MDIxNzE2ZWVfSUQ6NzY0NDczMTA3ODQzMDcyMzgxMF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Glob = find files by path/name pattern**

> Looking for file names/paths → Glob
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MzI2ZDczZWU0ZjVjNTE4MDIxZjBiY2M1NTgyOGU4NjZfZmRiMDBkMzJmYWIwOWZkMWYzOWE1Njk5M2E2M2I5MzBfSUQ6NzY0NDczMTM3NzEyODkwMjM2M18xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Read = load file content**

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=Y2Q4ZjM0NWUxZTZlNGNhY2YwYmM1ZDhjMGFkY2Q2YzZfNjFlMjMyOWU0ODRhM2IzZTk3N2YzZTEwMDQ2YzlmMzlfSUQ6NzY0NDczMjkyODQ0MTczMjgzMl8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Edit = targeted modification**

> Small targeted change with unique anchor text → Edit
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NDliZjJmMmM4NjQ3ZTA0YmZjZjYzNDZiZTc2OTlkMjBfNTJmYTQ5NWIwYzYxYTczMWFkZDhjODMwYzJmNGE1ZWVfSUQ6NzY0NDczMzE5ODczNTM4MDE5MF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Write = full file write/rewrite**

> Use **Write** when creating or replacing a whole file
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ODMyMTlhNzk2Zjc1MDYzYjFmZWExMjJjMTNkODg3MTZfODNlOGEwYWE1ODhiZDMyYTM4MjAxNDYyNmQ4NDBjOGFfSUQ6NzY0NDczMzczOTk3MjQ0Nzk2OF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **When Edit fails, use Read \+ Write**

> Sometimes Edit fails because the target text appears multiple times, so it cannot find a unique anchor
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NDA1YjJhNjdkM2Y3ODNlZTE4MjRkYzY0OWE5MGVhNzRfMjIxZWZmNmU2YTE1ODRhNThjMGYzOTQyMjAzZDg3MmFfSUQ6NzY0NDczMzk2NDc5NzMxNjgzN18xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **Build codebase understanding incrementally**

> Search first, then read selectively\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=OTIyZjEwZjE2Y2JhODY2OThhOGJiYjUxMTg2NzgyZmJfMTIxN2FhNzkwYmI5ZGUwOGVhYTkwOWUyYjllNWE2NDRfSUQ6NzY0NDczNTMzMDI0Mjk3MzQxMF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)





---

# Domain 3\. Agentic Architecture \&amp; Orchestration

> `CLAUDE\.md` is like Claude Code’s project memory/instruction file\. Put shared team instructions at the project level, personal preferences at the user level, and local conventions at the directory/rule level\.
> 
> 



## Task Statement 3\.1:  Configure CLAUDE\.md files with appropriate hierarchy, scoping, and modular organization

> **Shortcut: **
> 
> - **Personal rules → \~/\.claude/CLAUDE\.md**
> 
> - **Team rules → project CLAUDE\.md**
> 
> - **Big rules → @import or \.claude/rules/**
> 
> - **Confusing behavior → /memory**
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NDEyZGYxNzJlNTYyMjY0ZTE3ZmY4ZTljM2MzNDg4MzJfYTJmZTg1MTJjZTk3YjZhOWRhNWRjNWRlNjUxMjU0YThfSUQ6NzY0NDc1NjA5MjMyNDQ1MDAxMl8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **CLAUDE\.md hierarchy**

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=YzgyODc5NmM0YWUzM2Y5ZTI4ODFiMDJhODllMzcyMzJfMTk3Mzg2MDcwYjJkM2E1NmQ2M2E2MmM1ZTY0NGRiNDZfSUQ6NzY0NDc0NDQzMTI3ODkxOTM4N18xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **User\-level is not shared with teammates**

> Team instruction → project\-level\.
> 
> Personal preference → user\-level\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZDA3OThiY2Q3ZjczMDY3YzQ3OTQxM2IzZTVjYmI4NWFfOWU5ZTJkNmE5YTNjYTg2MDAyMWVmMmE2ZDkyMzAxMWNfSUQ6NzY0NDc0NDc3MzAyNjQzNDc3OV8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **Use ****`@import`****to keep CLAUDE\.md modular**

> Large CLAUDE\.md → modularize with @import\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MDU1N2Q0YTNjMDI5NTFkYmY5MmEzMzhiNDQ4YzM5YjBfMTBhM2IzODFiMjJjYmM3MWE5YmZiMzdhOGRmMDBkYzBfSUQ6NzY0NDc0NjAwMDM4MjEzNjAyOF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Use ****`\.claude/rules/`****for topic\-specific rules**

> Many separate standards → \.claude/rules/\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=YTcyOGJkNmVlZTViZDE3ODFhYWQ5NWQwMTM4YzAxYjhfM2Y0Y2Q1MWI3Y2JhMjZhMWFlOTU4ZWI2Y2M3NmUxMDJfSUQ6NzY0NDc1MzExODEwMzIyODEzM18xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Use ****`/memory`****to diagnose loaded instructions**

> Inconsistent Claude Code behavior → check /memory\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MGJkNzAzMDE5YjViYzk5NTUzMDUzYzc3OWU4YjE0ZTdfNTY4NzNiYzEzM2EwMzVmMzA0NTExMGJjMzljZGNhNzVfSUQ6NzY0NDc1MzYyODI0ODM2MjcxNl8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Common exam patterns**

**Pattern 1: New teammate doesn’t get instructions**

**Problem: **

Instructions are in user\-level \~/\.claude/CLAUDE\.md

**Solution:**

Move shared instructions to project\-level \.claude/CLAUDE\.md or root CLAUDE\.md



**Pattern 2: CLAUDE\.md is huge and hard to maintain**

**Solution:**

Use @import or split into \.claude/rules/



**Pattern 3: Claude behaves differently across sessions**

**Solution:**

Use /memory to inspect which files are loaded



**Wrong:**

- Put team standards in `\~/\.claude/CLAUDE\.md`

- Use user\-level config for shared repo behavior

- Put everything into one massive root `CLAUDE\.md`

- Ignore `/memory` when diagnosing loaded instructions

- Duplicate the same rules manually across many files instead of using `@import` or `\.claude/rules/`





## Task Statement 3\.2:  Create and configure custom slash commands and skills

> **Shortcut: **
> 
> - **Slash commands = convenient command shortcuts\.**
> 
> - **Skills = richer reusable workflows with configuration\.**
> 
> 

Commands are shortcuts; skills are reusable workflows\. 

Team/shared → project \.claude/

Personal → \~/\.claude/

Verbose skill → context: fork

Risky skill → allowed\-tools



### **Project\-scoped vs user\-scoped slash commands**

> Team command → \.claude/commands/
> 
> Personal command → \~/\.claude/commands/
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MjZmYWQzNTMxODMzZjU0YzQ2NTFlMTdiOGJlZjI5N2JfYWY4M2ZkZWQ5OTk3MDU2NjE5ODRkZjc5N2VmNjJiNmRfSUQ6NzY0NDc1NzM1MDU0MDQ4MDIyNF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=YjlmZDYwY2U5ZTA1ZjUxMTFkOTM3Mzc1YzJhOTQyYWFfNzQ4MWIxY2ExYWE0MjcyYjExNWE0ZGFkMmQ5ZGY1NDFfSUQ6NzY0NDc1NzQ1MjQxNjA3NzUzMV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **What is a Skill?**

> A **Skill** is a packaged workflow for a task lives in \.claude/skills/
> 
> 

Each skill has a `SKILL\.md` file\. The guide says skills in `\.claude/skills/` use `SKILL\.md` files and support frontmatter options like `context: fork`, `allowed\-tools`, and `argument\-hint`\.



### **`context: fork`****= run skill in isolated context**

> **Fork = do messy thinking elsewhere, return clean summary\.**
> 
> 

Use context: fork when the skill is noisy, exploratory, or long\-running\.

Do not use it automatically for every skill\.

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NmZhYmMxMzg3MTk3OWMyOGFiNzIwNmJhZGQxMmJiNWFfYWQyMzM4MjQ2M2MwY2Q0YzZhYzEzZGE4ODk3ZmU4NWNfSUQ6NzY0NDc2NzkzNDc3ODgwNTk4NF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZTVkYmI0OGQxMmI4NDkzYWZhY2VlMWJiODUyZDAxYWZfMDFjMDRhZTE4YzU2MWJiYTViNTE1Zjg1MDEwMmE5ZjNfSUQ6NzY0NDc2ODM1ODg5OTg3OTY0Nl8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZGIwYmVmM2MyNzkxNDcyOTRlZjZkNzgzMzliYWYzYWNfMDJhODE1OWJkMDAzOTViYWE5ZDllODVjM2I4OGVhYzNfSUQ6NzY0NDc2ODM5MjQ1MjI5NjQyMV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **`allowed\-tools`****= restrict what the skill can use**

> Risky workflow → restrict allowed\-tools\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NTJmMTI1MWUyNGM2OGU4NjM0YmYyYzdkYThlMmM0ODhfYzQzOTJhMzYzOTFlMTU5MzM2ODM0MGQ3ZTkwMzE1NTNfSUQ6NzY0NDgwNDMzMDMwMTc1NTEwMV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **`argument\-hint`****= prompt user for required input**

> Skill needs input → argument\-hint\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=YzViNzI4MTJkZGQ2ZTg4MTYwYzA2YzdkMDYwZTUzOTRfMGU3ZGYyNDU4ZTIwOWM2NDE5OWNlODBiMDIyMWJmZGRfSUQ6NzY0NDgwNDgwMjEwMjM2NTkyNV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Skill vs CLAUDE\.md**

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=YjA0MjQ3ZmM3NTI2NGZjYTg1ZDQyMGU2NWU5NDcyMWZfMzE4ZjhjODllZWVkZmE5NTZiMzgzZDJlMWY3Njc4ODFfSUQ6NzY0NDgwODM4NDUzMDM3MDI3NF8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **Personal skill customization**

> If you want your own version of a skill, do not change the team skill\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=YTNkZTdkMmI0OTI2YjhmZTA3OGUzMGQwNzNkMWY1MjlfZDYxNmY3OTJhMmJlMTU1YzM0MTQyZmFlNGU1ODIxODVfSUQ6NzY0NDgwODg0MDA0NjkwNzEwM18xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Common exam patterns**

**Wrong:**

- Put team slash commands in `\~/\.claude/commands/`

- Put personal experiments in project `\.claude/skills/`

- Use `CLAUDE\.md` for a workflow that should be invoked only on demand

- Let verbose analysis skills run in the main context

- Give a skill broad tools when it only needs read\-only access

- Modify the shared team skill for personal preference





## Task Statement 3\.3:  Apply path\-specific rules for conditional convention loading

> **Shortcut: **
> 
> - **Rules that depend on file path/type → \.claude/rules/ \+ paths glob\.**
> 
> - **Rules that apply everywhere → CLAUDE\.md\.**
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=M2ExOWQyNDg5Njg4NDA0OTZhYTYyMTgyM2IzNzdjOTdfMzBhNjhlNjA1MWE0NGU2ODUyZjRhNmI4YWQ3Njg3ZGVfSUQ6NzY0NDgzMzU4MzMyNzcxMDk0Ml8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)

**Wrong:**

- Put all conventions in one giant root `CLAUDE\.md`

- Use directory\-level `CLAUDE\.md` for test files spread across the whole repo

- Load all rules all the time

- Forget YAML frontmatter

- Use path\-specific rules without `paths`

- Use `Glob` tool instead of glob patterns in rules\. Similar name, different thing\.



### **What is a path\-specific rule?**

> A path\-specific rule is a rule that activates only for matching file paths\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZTdlMzVjMjU4MTJiNzFiMTlkMDk2Nzc1ZjM1NmNiY2VfZjAzNDhmNzBkZDg2MzFlYmEwZWVkM2EwODc3NjE1NDBfSUQ6NzY0NDgzMjIzOTk4NjUyNzk3M18xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Why use path\-specific rules?**

> Because not every rule is relevant all the time\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=OWVmYWEyZDU1ZDkwYmNkNmUxNDJmY2YyMzQ5NWY0MDVfZjVhNjFlNDRkOGVmMTYxNDJkZGQ3YWFlYjU0YWI0ZGFfSUQ6NzY0NDgzMjUxMzEzMTAwNzcwOV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Glob patterns**

> A **glob pattern** is a path\-matching pattern\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZGEyNjI4YmU3MWZhZWNmNTU1YmE4MjgwN2Q2NTI4ODdfZTExZWUxOGUxZmM3M2JmMzkyZWQ2MTM5NjdjOGJhMGRfSUQ6NzY0NDgzMjY3NTgyNzYwMTEyMF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Why not just use subdirectory CLAUDE\.md?**

> Subdirectory `CLAUDE\.md` works ONLY when the convention maps cleanly to one folder\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=YjVhNTVlNzk0NWJlZWRmZDM0YzhmNzM4MDBjNmI1MmJfNzk1NjQzMDhlNjNiNThjNjgwZjhjYjM4OWQ0NDBmNmVfSUQ6NzY0NDgzMzA1ODI4OTY2NzgxM18xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



## Task Statement 3\.4: Determine when to use plan mode vs direct execution

> **Shortcut: **
> 
> - **Plan mode = think/design before big changes\.**
> 
> - **Direct execution = do simple clear changes\.**
> 
> - **Explore subagent = messy discovery outside main context\.**
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZmZjZTAyM2ZlM2NlNDEzMmJjYmJkOTljZTJlZDgzYzVfMGNmZmZhODY5NmMwYmQzYTIwZDI4NWMzMmIyNjEzZGJfSUQ6NzY0NDg0MzIwMzI0OTg2ODUxMV8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)

**Wrong:**

- Use direct execution for a microservice restructuring

- Start editing before understanding dependencies

- Use plan mode for every tiny change

- Keep using plan mode after the plan is already clear

- Let verbose exploration fill the main context

- Switch to plan mode only after obvious complexity causes problems



### **Plan mode = think before changing**

> Complex / architectural / many files → plan mode
> 
> Use **plan mode** when Claude should explore and design before making changes\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ODg5M2U2ZWFhZTg0YzliYTk0MjQzOGE2YWI5ZjFhMDBfNGFkMWRlNWU3NmM2NzMzNjk1YjdiYWZmNDVjMWFhNTdfSUQ6NzY0NDg0MTUxMDU3NTY1NjY2N18xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **Direct execution = just do the clear task**

> Simple / clear / one file → direct execution\.
> 
> Use **direct execution** when the task is simple and the correct change is obvious\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ODgzNzAzM2JlNGZmZWM3ODU2MTk2MjZhMWFiNTk5YzNfMzQ1MWM3ZjYwYzQ3NTZjOTk5NmMwMzE1MDYwOWEyM2NfSUQ6NzY0NDg0MTkyNTk4Nzg4MDY3Ml8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **Why plan mode matters**

> Plan mode prevents Claude from jumping into changes too early\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=OTgyNWJmNTA0ZmU3NDk2YTczYTUwYmMyNGZkMzg0NmZfMTg4ZDU0OTAwNTAyNGRjY2VmY2ZkMTY0OTU5MmE3NzdfSUQ6NzY0NDg0MjM4ODAzNjY1Mjc2N18xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Explore subagent = separate messy discovery**

> Very large discovery tasks → Explore subagent\.
> 
> This is similar to `context: fork` from skills: messy investigation happens outside the main session\.
> 
> Explore subagent is the explorer\. context: fork is the isolation setting for skills\.
> 
> - Explore subagent → mostly Claude Code exploration/planning concept
> 
> - context: fork → skill configuration concept
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NjZlMmJjNDcwZGZiZDBiNzc5OGNhMDVjNTU3OTBmMTRfM2QwYzZjZTZlNzdhMDI2ZTNjNWY2Mjg5YTFmMTUxZGZfSUQ6NzY0NDg0MjY1MjAxMzQ0ODkyOF8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **You can combine plan mode \+ direct execution**

> Plan first, execute after plan is clear\.
> 
> It is not always one or the other forever\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ODI0ZDhhZDJjMWFhZjRkMDM1NmEyOTM3Yzg3ZGU2NzVfZjFmMGYwZTRkNDM3MDlhYTAwM2QwZDNiMGQxYTI1ZmFfSUQ6NzY0NDg0MzExNDg0NjM1OTI2OV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)





## Task Statement 3\.5: Apply iterative refinement techniques for progressive improvement

> **Shortcut: **
> 
> - **Examples clarify requirements\. Tests guide fixes\. Interview uncovers hidden tradeoffs\. **
> 
> - **Interacting issues go together; independent issues go sequentially\.**
> 
> 



### **Concrete input/output examples are powerful**

> Inconsistent transformation → give 2–3 input/output examples\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NGQ1YThkNmQ5ZmNlNGY5ZDM0YjMyZDNmM2M3MzVhZTFfZDA1OTg3YzM2NjA3YmMxNTQ5MTk4MThlN2RkMThiNDdfSUQ6NzY0NTE4MTkwNDk1Mzk1NDAxNF8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **Test\-driven iteration**

> Broken implementation → share specific test failures\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZjNmOTlmNzVkYzVlNDRiYzU1M2Y2MTM3MjU2ZjIzZDlfNDcxNzkzMmE2ZGU4MDdiZTFkMjE5N2I0ZmY0MDIyNWJfSUQ6NzY0NTE4MjIwOTg1ODYyMTE1MV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZDEwMTYyMjI4MDk0YWZjYWU0OGJjZmVjNGQ0YWY3Y2NfMTNmZmQ4MGI2ZDlhZTA0ZjJlNTFmOTA2NzA0MTRiN2VfSUQ6NzY0NTE4MjI0MzQyMTU4OTIxNl8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Interview pattern**

> Unfamiliar domain / hidden design tradeoffs → interview pattern\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=Y2U5Y2ViMzliYWEwODZlMDQ3ODA2ODY4NTFmNGNhYjFfNjJhZjUwNmJiYWExZDZkODkyZTIxZTFkNTEyOTZjMzFfSUQ6NzY0NTE4MjQ3NjE1MDg4NjEwOF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **All issues at once vs sequential fixes**

> Interacting problems → one detailed message\.
> 
> Independent problems → sequential iteration\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=M2RhZDVjMjQ4M2Y3YzgzZGY3ZmU1MTI2NWI5MzY0MGNfMmVkNzc4MzQwNjg4ZjZmNTA0M2Q3NzUzOTMxNmY2N2ZfSUQ6NzY0NTE4MjcxMzYxNTY4MzI5MV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Common exam patterns**

**Pattern 1: Claude keeps misformatting output**

**Solution:**

Give 2–3 concrete input/output examples\.



**Pattern 2: Claude’s implementation fails edge cases**

**Solution:**

Share specific failing test cases with input, expected output, and actual output\.



**Pattern 3: Developer is unsure about design implications**

**Solution:**

Use the interview pattern before implementation\.



**Pattern 4: Multiple bugs interact**

**Solution:**

Provide all related issues together so Claude can reason about interactions\.

**Wrong:**

- Tell Claude to be more careful\.

- Increase model temperature\.

- Give only vague prose descriptions\.

- Fix interacting issues one at a time\.

- Skip tests and inspect manually\.

- Ask Claude to implement before clarifying unfamiliar requirements\.





## Task Statement 3\.6:  Integrate Claude Code into CI/CD pipelines

> **Shortcut: **
> 
> - **CI Claude Code = \-p for non\-interactive**
> 
> - **JSON/schema for parsing, CLAUDE\.md for project context**
> 
> - **independent session for review**
> 
> - **prior findings to avoid duplicates**
> 
> 

In CI/CD, Claude Code must run non\-interactively, produce structured machine\-readable output, and avoid duplicate/low\-value feedback\.



### **Use ****`\-p`****/ ****`\-\-print`****for non\-interactive mode**

> CI pipeline hanging → use \-p / \-\-print\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MzdhYTIzOWNjYTI1OWNjOWU5OGUwYzdhOTJjZWYyNDhfYWUwZTE1ZWQ4OTY5M2IyMWEzODliMTNmNmEyYjczMWVfSUQ6NzY0NTE4NjAzNDQ2NDAwMTc1Nl8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Use JSON output for machine\-readable results**

> CI needs automated parsing → JSON output \+ schema\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ODUwOTJlZTMxMDNhMjQ0YTdiM2IwNjRhMTVlMWYxNDdfYTJkNDg1NzE3YWJiZDkwMjAzNWRmZmFlOThiNmE3MDFfSUQ6NzY0NTE4NjczMDM5NTc5OTI2MV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Use ****`CLAUDE\.md`****to provide project context**

> Claude gives low\-value CI feedback → improve CLAUDE\.md review/testing criteria\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NzgxYjEwNTQyZDczYjRkNDY5ZDA4OTBmOWMwZmFkYzlfNGRhNjk3ODZhYzU2NWQ2NjM3MDJkZTA5YzI1NjU3MmJfSUQ6NzY0NTE4NzA1MzU4MzYxNzc1OV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Use an independent review instance**

> Review generated code → use independent review instance\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NDkyZWYyZTU3YThjMTY5ODA3YWYwZTNlODQ4MWMxZmRfYTk0MmYzYjU4NDk3ZTc4ZDJhNDkxZjk3YTAxZWM4NDdfSUQ6NzY0NTE4NzMwMTQ4Nzg5MDE0NF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Avoid duplicate PR comments on reruns**

> Rerun review → include prior findings and suppress duplicates\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NzgyOTJlNmYyYjQ2ZDU1NGFmZjNkMTIyMjdmNGJjNTRfZTI2NDc2M2RhYzdkZTEwMDk5N2RjOGE3Yjg0MjgyZTBfSUQ6NzY0NTE4NzcxNzY5NDYzOTgzOV8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **Avoid duplicate test suggestions**

> If Claude is generating test cases, give it existing test files first\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=OTJkZjZlOGZiNTUyNTJhMDk4OTdiN2Q4Y2FmZDVhMWRfMDVlMDM2NWFjNDkyOTEwZWU1MDg4MDAwZDdhYzNhZDRfSUQ6NzY0NTE4ODA4NDU3NjkxNTE2N18xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Common exam patterns**

**Wrong:**

- Run Claude Code interactively in CI\.

- Use plain text when downstream systems need to parse findings\.

- Let the same session review its own code\.

- Rerun reviews without prior comments, causing duplicates\.

- Generate tests without showing existing tests\.

- Put project testing/review standards only in the prompt instead of `CLAUDE\.md`\.





---

# Domain 4\. Prompt Engineering \&amp; Structured Output

> Few\-shot = examples that teach Claude the boundary between correct, incorrect, and ambiguous cases\.
> 
> 

When you see

> Claude output is inconsistent
> 
> Claude misunderstands ambiguous cases
> 
> Claude has false positives
> 
> Claude extracts null/empty fields incorrectly
> 
> Claude doesn’t follow desired output format
> 
> 

Add 2–4 targeted few\-shot examples showing correct behavior, output format, and edge\-case handling\.

## 
Task Statement 4\.1: Design prompts with explicit criteria to improve precision and reduce false positives

> **Shortcut: **
> 
> **Precision improves when prompts define exactly what to report, what to skip, and how to classify severity\.**
> 
> 

Vague instructions like “be conservative” are weak\. Specific criteria with examples are much stronger\.

### **Explicit criteria beat vague instructions**

> Vague review instruction → replace with explicit report/skip criteria\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZjdlM2U5ZThhNjBhZmU4OWE1MmY0OTNmMTk4YmVkYzlfOTI0NjBkNjRkNDcyMDQ5ZjA5NTU4OTQ5Nzg4N2FiMjZfSUQ6NzY0NTkyMjM4MjY1OTEyOTA1OF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **“Be conservative” is not enough**

> “Be conservative” is too vague\. Define report vs skip categories\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=YTM4ZTdlZTNlN2QxZTUxNzk2NTQyZWZjZGJhZmRkYmJfMWYwMTdkOWY1ZWQ0Yzk4NTU1MWUwOWI3YmE2YTQ2YjdfSUQ6NzY0NTkyMjYzNjQ1NDgwOTMxN18xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **False positives damage developer trust**

> High false positives → reduce scope or disable noisy categories\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MzdkNGRjNWM2OWIyMWMzNTFkYzY2NzQ1YmQ4YjhmOWZfZTA2NWVkZDJlYTdmMjU3YzQ4NDU2NzkzOTM1NWExZGFfSUQ6NzY0NTkzMDIyODk1MDI3Mzc2MF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Temporarily disable noisy categories**

> If one category is causing many false positives, don’t let it pollute the whole review\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=OGRjMDNmYjMyN2ZmYWNlZTBjN2ZkYTdmN2M3ZTJiYzZfMDQyNTAyZGU5NDQ2ZWJkOTQ3MWE4MTc4ZjlhY2M5ZGJfSUQ6NzY0NTkzMTU2MTY1MjU4ODI1Nl8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Define severity with examples**

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=M2MwNGRjNTUzY2Q1YjE0YzA0OGQ0M2ZjMGRkZjczZTFfYzI0OGVhMTI4MjJiY2Y0ZTY1MDI1MGUxODc4OThkMTlfSUQ6NzY0NTkzMzQ0MjU4NTc3NTg0Ml8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **Common exam patterns**

**Wrong:**

- Tell Claude to “be more conservative\.”

- Ask Claude to report only “high\-confidence” findings without criteria\.

- Keep noisy false\-positive categories enabled\.

- Use model confidence alone to filter issues\.

- Define severity levels without concrete examples\.

- Ask for broad review of everything: style, security, bugs, performance, docs, architecture all at once\.



## 
Task Statement 4\.2: Apply few\-shot prompting to improve output consistency and quality

> **Shortcut: **
> 
> **Few\-shot = teach by example\. Use it for inconsistent format, ambiguous judgment, false positives, varied documents, and missing fields\.**
> 
> - **positive example**
> 
> - **negative example**
> 
> - **edge/ambiguous example**
> 
> - **maybe 1 missing\-field/conflict example**
> 
> 



### **What does “few\-shot” mean?**

> “Few\-shot” means you provide a few examples in the prompt\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=OWNlMzExY2JiNmQwMDBhNDFkZWFlMWE1ZjEyYTRlNzlfYjAwZTYxMTAwYmY1MTYxYzk3ZDAzYjY1ZWYzMjExNDZfSUQ6NzY0NTkzNzA1NDkwOTA5MTU1NF8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **Few\-shot helps when output is inconsistent**

> Inconsistent output format → add few\-shot examples\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZGUyZWRiOGIzZjY4ZDZkNjg5MzJkMGE3YmY1YTk3YzFfYjMzMDhmOGQ1NWJjMGM5ODczY2UxZWFhNmRkNWNkMDVfSUQ6NzY0NTkzNzY5OTE4NzcyMzk5N18xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **Few\-shot helps with ambiguous cases**

> Ambiguous boundary → show examples of both sides\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MzU3YjhhMDBkMDYwYmYwYThkNWIxZTA5N2U4YWY4OTJfMjM2YTJiZmQyMGRmZWYzMWVhMDZjZTFhZTFkNDRlZGVfSUQ6NzY0NTkzOTM2MDU2NjY3NzIxMl8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Few\-shot is not just memorization**

> Good examples help Claude generalize\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NzM5NWYxODNhMDYzMWY1ZDA3MTQ3NDhhODEzNWI4YTNfNThlMzlhOTY0MDUwNDRlOTlmYjRmNDNiOWE0MjU4OTdfSUQ6NzY0NTkzOTU5MjU4NzE1MzExOV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Few\-shot reduces hallucination in extraction**

> Varied document formats / missing fields → few\-shot examples\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MjRhM2ViOGJhYTBhM2NhZTRlNGE0MTcyMjBkZGU0YmVfYmJlNjVkNzEwOTdlNmFiYjhkYzgwNmUyZjYxYmIxZjJfSUQ6NzY0NTkzOTg4Mzg4MTU4MjMwNF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)





### **Common exam patterns**

**Pattern 1: Output format inconsistent**

**Problem: **

Add few\-shot examples showing exact desired format\.

**Solution:**

Move shared instructions to project\-level \.claude/CLAUDE\.md or root CLAUDE\.md



**Pattern 2: Claude confuses acceptable code with real issues**

**Solution:**

Add examples distinguishing acceptable patterns from genuine issues\.



**Pattern 3: Extraction misses fields in varied documents**

**Solution:**

Add examples covering different document structures and missing\-field behavior\.

**Wrong:**

- Just add more prose instructions\.

- Tell Claude to be consistent\.

- Increase temperature\.

- Add more context but no examples\.

- Fine\-tune immediately\.

- Use only one example for a complex ambiguous boundary\.



## 
Task Statement 4\.3: Enforce structured output using tool use and JSON schemas

> **Shortcut: **
> 
> **Tool\_use \+ JSON schema guarantees structure, not truth\. Use nullable fields, enums with unclear/other, normalization rules, and semantic validation\.**
> 
> 



### **Why tool use \+ JSON schema?**

> Need guaranteed valid JSON syntax → tool\_use \+ JSON schema\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=OTBiY2ZjYmZiMTljODQ0NGViMjZmZTVlY2NkZDYxODFfMzhhZjVjOTQ2MmRhM2ViODRiMDlhZmNhZDQ1YjVkYTRfSUQ6NzY0NTk0MzQ3MzM1NDMxMzQzOV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **But schema does not guarantee semantic correctness**

> Schema fixes format\. Validation fixes meaning\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=YmQ4ZDRiMDMzZDMzN2ZjOWE4M2MwNzc0MDA3ODYxOWFfMzYwNzJjNDIwYjMwZjhkMDE5NzI4MzhkNzI2Njc5MTNfSUQ6NzY0NTk0MzY3MDkxODYzMTE0MV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **`tool\_choice`****: auto vs any vs forced**

> - **`auto`** \- Use when tool call is optional\.
> 
> - **`any`** \- Use when there are multiple extraction schemas and the document type is unknown\.
> 
> - Forced tool \- Use when a specific extraction must happen first\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ODZhMTY5OTA3YjE1Yjk3OTE3YmQyZjY0YzI0YmFmM2RfYTEyYzdhYjJjMWNmOWNjMDRmY2E0ZTgwM2IzZjJlNzFfSUQ6NzY0NTk0NDE0MTYwOTI3NTEwM18xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=N2EzYjQxNmUxY2ZjYWQzNTE2NGIxNGY2ZDM0N2QzYWJfNDBjNjA4MmMyZDFiNzE0ZmJmODA5OTkyNTM1NGU4MmJfSUQ6NzY0NTk0NDE3NTIyMDgzODExMF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Required vs optional fields**

> Field may be absent → optional/nullable\.
> 
> If a field may not exist in the source document, do **not** make it required in a way that forces fabrication\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ODFlODJhMGE1ZDRkNDFjMGE1NTQ0OWE0MDNkMDM2NmJfODdmNmVlYjI3NTU3OGY4MjIxOWQwZTQxMTc3ZGEwNjFfSUQ6NzY0NTk0NTUyMTMzMjM3NTI2MF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Use enum with ****`unclear`****and ****`other`**

> Ambiguous category → unclear\.
> 
> Unexpected category → other \+ detail\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NmE2NDRkNzMxMTBhN2YzYTgzYjVjYjgwYzZhMDQ3YjhfNThmMWYyZjk5NWYzOTgwOGQ0NDJmOWEyOTI0ZWZmZDRfSUQ6NzY0NTk0NjQwMjE4NjU2MzI5MV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Add normalization rules**

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NWVmYmM4ODg2YmViMjExNDQzMzJkNjZjZjdlYjNkMjhfMzk4NDIxYzgxODY2NjdjYjZjMzc2ZWY5YTIxZDgyOTRfSUQ6NzY0NTk0NjkyMjU0ODcyNzUyMF8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **Common exam patterns**

**Wrong:**

- Ask Claude to “return JSON” without schema/tool use\.

- Use `tool\_choice: auto` when structured output is mandatory\.

- Make absent fields required, causing hallucination\.

- Assume schema prevents semantic errors\.

- Use enums without `other` or `unclear`\.

- Skip validation because the JSON is valid\.

- Ignore source format normalization\.



## 
Task Statement 4\.4: Implement validation, retry, and feedback loops for extraction quality

> **Shortcut: **
> 
> **Structured output needs validation loops: schema for syntax, semantic checks for meaning, specific error feedback for retry, and null/escalation when info is absent\.**
> 
> 

Valid structure is not enough\. Add validation, retry with specific errors, and feedback loops\.



### **Retry with error feedback**

> Retry with specific validation errors, not vague “try again\.”
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NmZiNmM5ZTg4YzlkYmUyNTAyZDUzMDk2ZWM0ODZmYmNfNDJlMTE5YzUyM2U0YmVlZDk3MTdjOGM5MTA2NDY3YTFfSUQ6NzY0NjIzMTA0MDMzMTQxOTM1Nl8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Know when retry will not help**

If info is absent, retry won’t create truth\.

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZGExMmFmMmE4NzBhODA2YjBiYTc3MzRkNzc1YzhjYjdfM2ExOWQ0ZGRjNjg4YmY1OWQ0YTkzMDJjZTE4NzBlODhfSUQ6NzY0NjIzMTU1MDAzMTU5NzI3NV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=Y2FjMzFhZWZmYWE2ZDZhODhhZGE2ZTY3ODE0NjA1NDlfNDM4YThkOWUwNTg0OTEyY2FmZTA4ZGJmNzgwZWI1ODBfSUQ6NzY0NjIzMTU5MDg3OTgyNTYzMl8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Semantic validation vs schema syntax validation**

> Schema validates shape\. Semantic checks validate meaning\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZjY0NTQ4MjkzMWQzMzBhMDVlN2FhYzY2OWNmNDQ0NmVfZWU4MTAwMTI4YTMzODk5NGI2YmU2MTI3NzU1ZjkzZWRfSUQ6NzY0NjIzMTg2MjUxMTI0Mjk3MV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZmYyMWQ0MGFlMGQ3MTQzMTIwYjIzN2RmZDE5MGU4OTRfZDY2MGExOWVjNWEyMWNmOTI2NWMwMDg0OWZhNzc3NTRfSUQ6NzY0NjIzMTkwODcxMTY0ODk4N18xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Include original document \+ failed extraction \+ validation errors**

> For a retry loop, give Claude all three:
> 
> 1. Original source document
> 
> 2. Previous failed extraction
> 
> 3. Specific validation errors
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZTI1MDE2YzA2MTEyY2Q3YWE3NmZiOWFlNjc2NjI1OWNfZWNkMjk1NDJmNDJhYzEwZmQ0YzBjM2E3YjJhN2IwNzdfSUQ6NzY0NjIzMjI3MzEzNzkyOTk1NF8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **`detected\_pattern`****for false positive analysis**

> This part is about feedback loops in code review / findings systems\.
> 
> Track detected\_pattern to improve prompts systematically\. If developers dismiss findings, you want to know **which patterns** are causing false positives\.
> 
> 

The `detected\_pattern` is not the issue text\. It is the category/pattern that triggered the finding\.

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MzllNzZjYTQxMzg5ZDViNTM5YTk0YmIzMTExYjY5MGRfNDJhYzFhNGZhOGViOWI4MzZiMzcwOTQwMjdjZTI2NmFfSUQ6NzY0NjIzMjk1ODUyMDY1OTY4NV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZGNhYjg1YmExZWViOWI5OGRiZDVkNjA0NWFmY2JiNjJfMTlkZWYzYzYwYjA1MzVjMjM2ZWMyOTU5YzUyNGFlOTRfSUQ6NzY0NjIzMzE3NTcyMjc5MDYyM18xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Self\-correction validation fields**

> Add validation fields that expose inconsistencies\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NmNjMGEyOGMzZmIxYzA3ZDA2NTM2YzMxOWIxN2I5ZThfYzFmM2I4NmExZDM2N2E5YWYyMzBkOTAwY2Q1MmU0MzNfSUQ6NzY0NjIzNjc4MzUyOTEzNTgzOV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Common exam patterns**

**Wrong:**

- Retry without telling Claude what failed\.

- Retry repeatedly when information is absent\.

- Assume valid JSON means correct extraction\.

- Skip semantic validation\.

- Remove failed extraction from retry context\.

- Do not track false positive patterns\.

- Treat all validation errors as equally retryable\.





## Task Statement 4\.5: Design efficient batch processing strategies

> **Shortcut: **
> 
> **Batch API = lower cost for async large jobs, not for blocking/interactive workflows\. Use custom\_id, sample\-test prompts first, and resubmit only failed items\.**
> 
> **Pre\-merge work \-\&gt; Synchronous API**
> 
> **e\.g\. unit tests, lint checks, security scan, type checking, Claude code review, Claude\-generated test review**
> 
> 



Batch API is cheaper but slower\. Use it for large, non\-urgent workloads\. Do not use it for blocking workflows where users or CI/CD are waiting\.



### **Message Batches API = cheaper, but no fast guarantee**

> Batch = cheap but not immediate\.
> 
> Can wait hours → batch\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NjMxZjkwODg3ZDE4ZGQ4YWViODc3NWY0ZTc4Y2ZiZjNfN2Y4MzQ1NGFhNDg2ZjQ0ODgxNTQ1OTY1OGNlNDc3MjRfSUQ6NzY0NjI0MDQzOTY0OTE5MzY5OF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Good use cases for batch processing**

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MzllYzQ3ZmNkNTQ2NjhkMDY5NDI1MmJiZTYzOThiNGJfN2NiMzZkMTFhOTcxZTIyNjAxZGIwMDgwMDY3OWFhYmJfSUQ6NzY0NjI0MTA1NjE5MTA0MTI1M18xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Bad use cases for batch processing**

> User/CI is waiting \(pre\-merge\) → synchronous API\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MTE5MWRiNTUyZmFmNzBlODgxMDdmN2EzYWQ3MGRjOTVfMTVjNjU4MTU2MTA3YmEzOGY2YTlhNDk2Y2Q4YmYyMDlfSUQ6NzY0NjI1MjMzMDU0MDIwNzg0Ml8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Batch does not support multi\-turn tool calling inside one request**

> Needs interactive tool loop → not batch\.
> 
> Self\-contained one\-pass job → batch can work\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=YjYzNTNiOGIyZDliMGI3ZDkzYzk0OGFmMzVlOTc4NTBfYzVhMzIwYzY3ZjJkMjAzNDJlNTkyOTMyOTgzNmE2NGRfSUQ6NzY0NjI1NDU0MzIxODUyNzk3MF8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **Use ****`custom\_id`****to match request and response**

> Batch responses need custom\_id for matching\.
> 
> In batch jobs, responses may not come back in the same order\.
> 
> So every request should include a unique `custom\_id`\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZGVmN2NlMWNjNjNkNzE4ZWUyMTcwNTNhZTM5ZjQzYWFfMjBjZDZlMWZhY2Q5ZGUzYzhiNzE4Njg2YWYwZjQyM2ZfSUQ6NzY0NjI1NTA4NDM4NDQyMzY1M18xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Resubmit only failed documents**

> Batch failure → resubmit failed custom\_ids only\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MWUxMGEwZmFiMjBiZDRjNWYxMDA5NjAxYmVjYzgzNjZfNTFhZjUxYzJiMGY2MDBjM2IxMWYwYjdhM2ZmODc3NzRfSUQ6NzY0NjI1NTU5MDkwNTU0ODUxMV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Refine prompts on a sample before large batch**

> Before huge batch → test sample first\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NmM1ZGMwNmE1YWI4ZDRkNjA3NjU0ZGZhY2E3NWQ2NDdfM2VmZjIwOWZjYjYxY2QxYTFhOTVjNGQ3MDBkZmUzMGZfSUQ6NzY0NjI1NTkzMzk2MTgxNzgyOV8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **Common exam patterns**

**Wrong:**

- Use batch for pre\-merge checks\.

- Use batch for real\-time customer support\.

- Use batch when tool calls are needed mid\-request\.

- Resubmit the entire batch when only some items fail\.

- Skip `custom\_id`\.

- Run large\-volume batch before testing prompt on a sample\.

- Choose synchronous API for overnight jobs just because it is simpler\.





## Task Statement 4\.6: Design multi\-instance and multi\-pass review architectures

> **Shortcut: **
> 
> **Don’t let Claude grade its own homework\. Use independent review, multi\-pass analysis, and cross\-file integration checks\.**
> 
> 



The same Claude instance that created something is not the best reviewer of that same thing\. Use independent review and split big reviews into focused passes\.



This is about making Claude review work more reliable by using:

1. independent review instances

2. multi\-pass review architecture

3. confidence/verification passes



### **Self\-review has limitations**

> Same session self\-review is biased\. It may miss issues because it still has the reasoning context from generation\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MWZmMWU3MGZiMTA5N2FhYjFkZDU3YmM4OWZhNTUwZjBfMGIxMzExYzBhZTRiZmFhYTk3ZTJiNmE3NDMwZGEyYmNfSUQ6NzY0NjI1OTQxODAwNjY4NzQ1Nl8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **Use an independent review instance**

> Generated code review → use a second independent Claude instance\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZGYwZGQ1M2I4OWRjYWU2OGQyMzZkODQ0ZjM4ODBhMjZfMmM2ZDNlMzMzOTk3MjFmYjUyNjI4MmM3ZWQzYjFjZjRfSUQ6NzY0NjI1OTYwNjczMzQ0Mjc4Ml8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Multi\-pass review**

> Large multi\-file review → per\-file passes \+ integration pass\.
> 
> For a large PR or multi\-file change, don’t review everything in one giant pass\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZTliZTFhOTA0MWU2ZGJiNWRmMmYwZGZiZGIwODg2OWJfY2M3NmQxMDE4M2MxODljNmQ2NWUwOWE2NjM3OTkzYjJfSUQ6NzY0NjI2MDAwNDUzNDQwNjg4Ml8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Local issues vs integration issues**

> A per\-file review may miss this\. A cross\-file integration pass is designed to catch it\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NGRiOWE5ZTc3OTk3OWNjOTE0YmNmNDA3NjhkNzM0MzNfYWNkODM4ZmEwNDZkNTU5ZDFjNTY2YmE2OGI0YjM1ZDlfSUQ6NzY0NjI2MTQ2NTYyNDEwNDY3N18xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Verification pass with confidence**

> A verification pass means Claude reviews findings and attaches confidence or priority\.
> 
> But DO NOT trust it\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=YmQ2OTkxMDgxM2VmN2U3MjE1NTA0YzQyZWE1YTY4OWVfOWY4M2NlMjI1MDIwNzg4ZTUwNDdjYzQwYjE4OWMwZmJfSUQ6NzY0NjI2Mjk4ODgxNTA2MDY5OV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Common exam patterns**

**Wrong:**

- Ask the same Claude session to review its own code\.

- Use extended thinking instead of independent review\.

- Review all files in one giant pass\.

- Use only per\-file review and skip cross\-file integration\.

- Trust model confidence without calibration\.

- Require consensus across many full\-PR passes instead of designing focused passes\.



---

# Domain 5\. Context Management \&amp; Reliability

> **Shortcut**
> 
> **Keeping Claude reliable when the conversation, tool outputs, documents, or multi\-agent work becomes long and messy\.**
> 
> 

## 
Task Statement 5\.1: Manage conversation context to preserve critical information across long interactions

> **Shortcut: **
> 
> **Long context ≠ useful context\. Preserve facts, trim noise, structure summaries, keep metadata, and put key findings where Claude can see them\.**
> 
> 

### **Progressive summarization can lose important facts**

> Summaries are useful, but they can accidentally turn precise facts into vague statements\.
> 
> Do not summarize away numbers, dates, IDs, amounts, or explicit user expectations\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=OTU4ZDQyNGQ2ZGY1ZDJjYjJlMWU3YzYzYTUxNGI4ZjhfYzFhOTk2YTZhZTMwZTkyYmYxNThmNjAxOGM1ZjA4MzZfSUQ6NzY0NTE5MDUwMjQzODA2MzgzNV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Use a persistent “case facts” block**

> Critical facts → structured persistent block, not vague summary\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=OGZiMmNiNjA4NmFjOWQ5ZDgwYTg3YmQ1NGYzMTNlYTRfODYzM2FmMmQ5ZGVkMWRmMmU1YTEwZjBjNDdkOWU2YTZfSUQ6NzY0NTE5MDg1Nzk4NTA4NTE0OV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **“Lost in the middle” effect**

> Put key findings at the beginning\. Use clear section headers\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NGM2MjkyZmU2MzUzY2IzODkxZmE5MDFkMTA5MzNkZjlfZGRiMWFjYTNlZmZiMGI2YjFkZWFhOTYyYmY0YmViNDRfSUQ6NzY0NTE5MTA0OTE5MTkzNTcxMV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Tool outputs can bloat context**

> Trim tool output before it pollutes context\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NmI0ODYwMTU3ZWRjNjg1NGE4NGE4ZjQxOTVmNDY3MGZfMDU3NTk4NDA3MGFmNWEzOTI5OTVkOTgxZWU3OThmNTFfSUQ6NzY0NTE5MTgzMDE5OTU3MDE0Ml8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Complete conversation history still matters**

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NjBmMzljMTExMGMwMjE4MTdlNWI0NjcwMzE2ZmJmMmRfN2Y1N2FiZTAwNGIyZmNjZTQwYTI5ZjgwMmU2ZGFkNzFfSUQ6NzY0NTE5MjI0NDMwMzU0ODEzMF8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **Subagents should return structured data, not huge reasoning dumps**

> If a downstream synthesis agent has limited context, upstream agents should return clean structured findings\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MGY4NGYwMDIxMjQ3NjZmY2M5YTNiODUzZGZiN2Y4MWNfMjgxMmVkZDE2MGIwNWY5YzQ0NTM2NWNkNzQwNDkyYmFfSUQ6NzY0NTE5MjU2MzQyMjYxMzIyMV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Common exam patterns**

**Wrong:**

- Summarize everything into a short paragraph\.

- Keep all raw tool outputs forever\.

- Put critical facts only in the middle of a long prompt\.

- Let subagents return long reasoning chains to synthesis\.

- Drop metadata like source dates, page numbers, or methodology\.

- Assume bigger context window solves organization problems\.





## Task Statement 5\.2: Design effective escalation and ambiguity resolution patterns

> **Shortcut: **
> 
> **Escalate for explicit human requests, policy gaps, or no progress\. Clarify ambiguity\. Do not rely on sentiment or model confidence\.**
> 
> 

### **Valid escalation triggers**

> Human request / policy gap / stuck workflow → escalate\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NWQwYzk1NjMyYTA1Y2U2NGFlOTNmMjUwMzVhNmIwMWJfN2M2M2ZkNmE0NjZjNTJiNmVkYjE4OGQ5ZjI4Yjk1MmZfSUQ6NzY0NTE5NjEzMTI0NDQyOTAyMF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Explicit human request vs frustration**

> Explicit “human now” → escalate\.
> 
> Frustrated but solvable → acknowledge \+ solve\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MmQyOTc4MDBkMzIyOWU4ZWIwZTYxNmYyYTg2MzVmMjBfOWMwZTU2NDZiOGRlYzI0NzgwNGUzOTJjMzA0MTg5MTJfSUQ6NzY0NTE5NjMxNTc3NzE5MTY0Nl8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Don’t use sentiment as proxy for complexity**

> Do not escalate just because sentiment is negative\.
> 
> Escalate because policy/workflow requires it\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NGY5NDljNjA3MGYwNmJmODIxZDczNjAxOWY2NTc4YTVfMzRmYmZiMTg5NzIxZGZjZDczNGNmOTZkMDkxOTA1NzRfSUQ6NzY0NTE5NjUwMzc3MDQ2ODA2NF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Don’t trust self\-reported confidence**

> Escalation criteria \&gt; self\-confidence score\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=Yjk2M2RlZTExYWQ4ZGRlYWM2NmJhNjViYzA1NThiOTBfYzYzNzAxMWY3YTNiODY3MGJmN2M3NDZhZDhlZDY2MGNfSUQ6NzY0NTE5NzA4MzM2MzkwNTI0Nl8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Multiple customer matches require clarification**

> Multiple matches → ask for identifiers, don’t guess\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NTQ0ZGZhNWMyYTQxYjFiNzc0YzJmYjQxNWJmMWRhYmRfY2Y5ZTU4M2Y1ZmY2YmZjMDMzNGRmM2IwNTRiYTUzZWRfSUQ6NzY0NTE5Nzc5Nzk4MjU3MjI1NF8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **Policy ambiguous or silent = escalate**

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=N2NkMzVjZTNiZjVkZjc4M2U5NDA5ZTBmNmQ0OWU4YTBfM2QwMzQ0ODRjMDZjYThmMmExYWU0MjM4NGYyOTkwMmVfSUQ6NzY0NTE5Nzk3NDQ3MzExNzQwOF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Common exam patterns**

**Wrong:**

- Escalate whenever sentiment is negative\.

- Use model confidence score to decide escalation\.

- Pick the most likely customer when multiple matches appear\.

- Keep trying forever instead of escalating when stuck\.

- Try to resolve after the customer explicitly asks for a human\.

- Invent a policy for cases not covered by the policy\.





## Task Statement 5\.3: Implement error propagation strategies across multi\-agent

> **Shortcut: **
> 
> **Multi\-agent error propagation = local recovery first, then structured error \+ partial results \+ coverage gaps\.**
> 
> 

A subagent should not hide errors, and it should not crash the whole workflow\. It should return structured error context so the coordinator can decide what to do\.

### **Generic error is not enough**

> Return structured reasons for an error\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZTlkOWJiOTdmNjg2NmJkOWNmYTUzMDQ2NzgzODdjM2RfZGJkYjkyNjM5ZmQ3NWY4ZjkxNjJlMjlhYjc1ZTA1N2RfSUQ6NzY0NTQwNjI1NDc1OTY5NDA0NV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Access failure vs valid empty result**

> Failure to search ≠ search found nothing\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ODg0ODY2OGM5YmZkNDI5ZmRhMzdhMDVmZDkyZjNiMTZfYzlkOWQ2YmU5ZWQ0ZDc4YWY2MjdhYjlmNGVmNDAxODNfSUQ6NzY0NTQwNjYzMzk4MzUxMjI5M18xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=YzZhNzBlODNhZDdlYzcwZjYwNTUyMjBkMjAyNGIyYTZfOWU5ZjdjNTNmMTBmM2NmMjY3MWU0NDBiZDBkNDZmZjNfSUQ6NzY0NTQwNjY2MTg1MjE3MTk5OV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Two anti\-patterns**

> Do not hide error\. Do not kill workflow\. Report structured error \+ partial results\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=N2U5YmY1MWIzODY3NjJkMGJmZDhiZTEwNDAzNjE0ZjRfYTNjZDhhMDc3YjY4ZmVlZWVlYmNiZTBhMmI0MThhMDFfSUQ6NzY0NTQwOTQ0NTc4MTk5OTMyM18xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=M2UyMmJiOTAzYTMwNjBhNzFmNTM2MDNjN2IyMzM2ZTBfZmY5MWYwZmE0YTU5M2M0NTZhOWE1MzBjMTIyNjk1ODdfSUQ6NzY0NTQwOTQ3NTY5NTc3NTQ1NV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Local recovery first, then propagate**

> Transient error → local retry first\.
> 
> Unresolved error → structured report to coordinator\.
> 
> Return partial results and what was attempted\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZjAyMjhlNDdmNGYzMmY4NTI5Mzg5NTFlNWUxODUxOTdfOTI3NDhhYzVhMGM4OGU4ZGQ4OTE0NWMzZjA2MzhiNWVfSUQ6NzY0NTQwOTc5OTM3NDQ3NDk3NF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Coverage annotations in synthesis**

> Partial evidence → label coverage gaps\.
> 
> If some sources failed, the final report should not pretend everything is complete\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=YTFiMzA1YTc0ZWY2NGM3YTEyMTc5MjZjOWZhZGEyOTVfYmFjNDZmMGRmOGYxOGIzYTNjNTVjYzJmNDZiMTZhOWFfSUQ6NzY0NTQwOTk3NjUyMDgwNjExNF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Common exam patterns**

**Wrong:**

- Return empty results when a search failed\.

- Stop the whole workflow on one subagent failure\.

- Return only “search unavailable\.”

- Hide partial results\.

- Retry non\-retryable failures repeatedly\.

- Produce a final report without noting missing coverage\.

- Treat “no matches found” as the same as “tool failed\.”





## Task Statement 5\.4: Manage context effectively in large codebase exploration

> **Shortcut: **
> 
> **Large repo exploration = subagents for noisy discovery \+ scratchpads for key facts \+ summaries between phases \+ manifests for recovery \+ /compact when context gets bloated\.**
> 
> 

This is about what happens when Claude Code explores a large repo for too long\.

In long codebase investigations, context gets noisy and stale\. Use subagents, scratchpad files, summaries, manifests, and `/compact` to preserve useful findings without drowning the main session\.

Do not rely on Claude’s long\-session memory\.

- Use scratchpads for exact findings\.

- Use subagents for noisy exploration\.

- Use /compact when context is bloated\.

- Use structured summaries/manifests when resuming or recovering\.



### **Context degradation**

> Long session → risk of generic/stale reasoning\.
> 
> Reason:
> 
> 1. Important facts get buried
> 
> 2. Generic knowledge competes with repo\-specific facts
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NjgyOWY1ODc0NzRhZTcyMjgxMzU2MTdlNTY3Mzk4NTZfYjRiODllZjk2YzI4YTg5NTZiMzY3YTJhNDczZmYzOTlfSUQ6NzY0NTQxNDI5MjIyNDgwNjYyM18xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



Complimentary

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZjY0OTM4MzI2ZTQzMjc3Y2ZlYzViZTNkMDFhYzhkNzlfNjRkYjk1OWYwYmIwYTJhOTk3ZGIwODU1NzYwYTI0MmVfSUQ6NzY0NTQxNjg3NTIyMzk5NDA3Nl8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZmZkN2YyNzVjMDA3ODYzMWZlZmMzZTJmMWJiNWJmNGFfNzRmZjhhNWViM2E2ZjQwOTdkMjU5YTEyMDVkYmIxNzdfSUQ6NzY0NTQxNzA4NTY3NzM0MjQyN18xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **Scratchpad files**

> Important discovered repo facts → write to scratchpad \(explicitly tell Claude\)
> 
> A **scratchpad file** is a persistent note file where Claude records key findings\.
> 
> If the conversation context gets compressed or stale, Claude can re\-read the scratchpad and recover exact facts\.
> 
> Claude may sometimes decide to write notes on its own, but a production workflow should make this deterministic: “After each phase, update the scratchpad\.”
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MDQ2YWM3ZTk3MWM1M2ZkMDBlYzU3YjFjMmQ4MzM5YzZfYmUwZjNmMWU4MWE1ZmQyNGQ2MWUxMGRkYmI5ODg3MWJfSUQ6NzY0NTQxNDk3MDM3MjQ2MDI1NF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



Complimentary

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=YTI2ZWFmYWExYzAzNGZiMzYxNzU3ZmExYmQxY2I4NThfYTkwMDE3MDA1YjFhYjRiNzI1NzhjMjk5ZTdlNzkzZWVfSUQ6NzY0NTQxNzMwNDcyNDg4NTIxOF8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **Use subagents for verbose exploration**

> Messy repo discovery → delegate to subagents\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=N2ZkYmQ2MzBhZDg2OWYyMjZlNjRjYzlhY2FhZDBjM2FfNmQ1M2M3NmIzNjZhMzhlMjEzMTIyZDQxNTBhNmM5ODFfSUQ6NzY0NTQxNTMyOTE5Njc3MzA4NF8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Summarize before next phase**

> Phase transition → summarize first\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=OWUyYzk4ZTljZGUyZDRmZjkyNTc5ODk3MDE3OGQ2OTVfZTkzNjUwMGQ5N2IxMTU5ZTY4MGZlNTc3MTFjYmE5ZjNfSUQ6NzY0NTQxNTQ2Mjk1NTY4MzU1Ml8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Structured state / manifests for crash recovery**

> Need crash recovery → export structured state \+ manifest\.
> 
> 

A **manifest** is a structured file that records what each agent did and where its state is saved\.

If the session crashes, the coordinator can load the manifest and resume intelligently\.

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NzcyYzJmNTVlOGM1MGY3OWU0ZWJkM2FmZDczZjFlMzNfOTA2OTVlOTViNmVlNjhiMzllNDVjNTFmMmU5NzgxMzVfSUQ6NzY0NTQxNTgxMzEzMzk5NTc0Nl8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **`/compact`**

> Context full of verbose exploration → /compact\.
> 
> DO NOT assume /compact writes to scratchpad\.
> 
> The safest sequence: Explore → write scratchpad → summarize phase → compact if needed → continue\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NzgwYWE4ZmQxNWIxMmNiY2EwMjQxZDU4ZjhiN2M4MGNfYWU4MjI0NjU2ZDQ0YzYzOGU0MzBmOWEzNTZjNGRhNzZfSUQ6NzY0NTQxNjE1MDExMjc1MTMyM18xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



Complimentary

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MmQ0ODE2MmJlZjRlMTEzZDgxMzQzN2NhMTk1NzhjYTRfNDk0OTYyMGE5NmEzNTEzYjllZmVjYjg1ZWZmNzBlOGZfSUQ6NzY0NTQxNzgzMDc1NzY5NTU0Nl8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Common exam patterns**

**Wrong:**

- Keep all exploration details in the main session\.

- Rely on Claude to remember exact discovered classes after a long session\.

- Start every phase from scratch\.

- Ignore scratchpad files\.

- Resume after crash without structured state\.

- Let subagents return huge raw logs instead of summaries\.

- Use only a larger context window as the fix\.





## Task Statement 5\.5: Design human review workflows and confidence calibration

> **Shortcut: **
> 
> **Human review strategy = segment\-level accuracy \+ calibrated field confidence \+ stratified sampling \+ review routing for ambiguity/risk\.**
> 
> 

Don’t trust one overall accuracy number\. Measure performance by segment, calibrate confidence, and route uncertain or risky cases to human review\.



### **Overall accuracy can hide bad segments**

> Overall accuracy is not enough\. Check accuracy by segment\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=OThlZjgyYmI2OGMwNGRkYzZjYzMyNmI5MTA5NjQyM2ZfYzY5MTZjODBmZDU5ODI2NDZkYmJmNGVlMGI3YzE4OTdfSUQ6NzY0NTQyMTg4MzIxMDM4NzE2OF8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **Analyze by document type and field**

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ODdlNzQwZTg5MDIzYzY4NWFhYmM0NTFmYzFkYjQxNmNfYzU5YTUwYzQ4ODU3ZWFlNGY5MjY2MGFmY2I3YjdhMWZfSUQ6NzY0NTQyMjEyMjcyNjExNzA4NV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Field\-level confidence, not just document\-level confidence**

> Use field\-level confidence, then calibrate it with labeled data\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZjIyNDRlMGMyYjM0MmQ2ZGZmZGFjOGQ3ZTFhMWMwMzdfYmEyNGJkNjhjYTNjNWU0NDk2YWJmZWZlYWY4NDkyZGNfSUQ6NzY0NTQyMjI1MTUzNzQwMzYxNV8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **Confidence must be calibrated**

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=OGRkY2ZjYjUyYjYzNmYwNDcwMzM2Nzk0YzFiOTk4MjdfMGFiNTM5ZDY4MDM0N2I3MTdkNGI4MmMxNjBiMTY3YTVfSUQ6NzY0NTQyMjQzMzA2NjgzMTU4M18xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Stratified random sampling**

> Even high\-confidence outputs need sampled QA\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NTljMzE4N2RmNmY4OGFiYTM5OGNhODY4MmUyNjk4OTdfZjA3OGFmNzg0Y2ZhYTk5ZjhmZTZiODVkMDMyYWQyN2JfSUQ6NzY0NTQyMjgwODg0MDM0MzI1OV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Route limited human review capacity intelligently**

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=YjExZGMwOTg1YWEwODc0ZDI2MzU1NDU0YTZmMzZmYmVfOTBiMjUwMmRkNGI3MzBkN2M5ZTk3ZGE5MzAxM2RhZDRfSUQ6NzY0NTQyMzAwOTU0ODc2Njk0M18xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Common exam patterns**

**Wrong:**

- Use only aggregate accuracy\.

- Fully automate because overall accuracy is 97%\.

- Trust model confidence without calibration\.

- Review only low\-confidence cases and never sample high\-confidence cases\.

- Use document\-level confidence only\.

- Reduce human review before checking accuracy by field/document type\.

- Ignore contradictory source documents\.





## Task Statement 5\.6: Preserve information provenance and handle uncertainty in multi\-source synthesis

> **Shortcut: **
> 
> **Multi\-source synthesis = claim \+ source \+ date \+ excerpt \+ conflict handling \+ appropriate format\.**
> 
> 



When combining multiple sources, do not lose where each claim came from\. Preserve claim\-source mappings, dates, conflicts, and uncertainty\.



### **Source attribution can get lost during summarization**

> Do not summarize away the source\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=Zjk5NGU5ZGYwMjgzNTNhMTE4M2ZkMDRiN2ZiNDk2MTFfYzNkNjY0YzdlNGYyYTdjMjEzMTkwZjBlZmU3YjFhMTZfSUQ6NzY0NTQyNTU0NzUwNTM3Mjg5Ml8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Use claim\-source mappings**

> Every claim should carry its source\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=NTdlNjU1YjU5MjFmNjQzYWM5MDg5NDU4Y2VlNjllZmFfNmViZWJiOWU2NmM3NTUxMjFjZTA5OWRjM2NmYTRmMTdfSUQ6NzY0NTQyNTgwMDE3NDQ0MDE1N18xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Conflicting sources: annotate, don’t randomly choose**

> Credible conflict → preserve both values and explain context\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ODY1YjFmN2RkMjI0NzQ4Zjk3N2FmOWUwMjdkZjRjZTNfOTBjNTYxMWMxNWU0OTE3Nzk2NTk0MmNmZGFlYThhMzZfSUQ6NzY0NTQyNjEwNzM2NTI5Nzg4OF8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **Dates matter**

> Always keep publication/data collection date\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=ZTRlOTM0OTJmOWZjZDY0ZmM2MTgyMWUwMDQ2NjBiN2ZfZTQ3OGE1NzFjZThhNGYwZDI2ZWMzOWQ4YTZmYWI4NzNfSUQ6NzY0NTQyNjIzNTQwODkyODQ4NV8xNzgwMjg4MTA1OjE3ODAzNzQ1MDVfVjM)



### **Let coordinator reconcile conflicts before synthesis**

> If document analysis finds conflicting values, do not hide the conflict before the coordinator sees it\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=MDQ0YmFhZTU4MjFmOWYyOGE2YzNiNWZmZTdmMGY3YTlfZjU0MjhhZjUxOWViOTU5ODA3ZDdkNTJmYzJhMzkzNDlfSUQ6NzY0NTQyNjM3MTU1NjA2OTA5M18xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **Format different content types appropriately**

> Not all content should be rendered the same way\.
> 
> 

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=Njc0ZDkwMGU1YzBkMzk4OTE1YTc0NmZjZGI4NTZkNWRfMDQ2MGVhMjljZGRhMjdkNDg2YjAyMDQ3YmUyYTUyMDFfSUQ6NzY0NTQyNjU2MzcxMzk0NTMwOV8xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)



### **Common exam patterns**

**Wrong:**

- Summarize all sources into one paragraph and drop citations\.

- Pick one statistic when credible sources conflict\.

- Ignore publication dates\.

- Treat older/newer sources as contradictions without checking time frame\.

- Let subagents pass only conclusions without source metadata\.

- Convert all output into one generic format\.

- Ignore contradictory source documents\.





---

# Appendix

![Image](https://internal-api-drive-stream-sg.larksuite.com/space/api/box/stream/download/authcode/?code=YjViZjczYzIzMmQ2NjVhYTJmNTQyMmI1MWQ4ZWZkZTFfNTYxYzAxNTc1YzYzNTg2NWNhMjg1NTEzNGY5N2QxZWNfSUQ6NzY0NjI2NzE1MDM0ODg2NTI0M18xNzgwMjg4MTA2OjE3ODAzNzQ1MDZfVjM)




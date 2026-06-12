# Plan Mode vs Direct Execution

> Think/design before big changes; just do simple clear ones.

| Dimension | Plan mode | Direct execution |
|---|---|---|
| Complexity | Complex, architectural | Simple, obvious |
| Scope | Many files | One file |
| Dependencies | Need exploring | Understood |
| Risk of early edits | High | Low |
| Example | Microservice restructuring | Rename a variable, small fix |

## Bottom line
Use **plan mode** when Claude should explore and design before
changing (complex/multi-file/architectural). Use **direct execution**
when the correct change is simple and obvious. They **combine**: plan
first, then execute once the plan is clear — and stop using plan mode
after that. For very large, messy discovery, push it to an **Explore
subagent** (like `context: fork` for skills) so it doesn't fill the
main context. Wrong: direct execution for a restructuring; plan mode
for every tiny change; switching to plan mode only after complexity
already caused problems.

## Sources
- [[CCA-F Exam Notes]] — task statement 3.4.

## Continue reading
- **Plan mode details** → [[plan-mode]]
- **Big-repo context** → [[large-codebase-context]]

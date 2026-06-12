# Prompt Chaining vs Dynamic Decomposition

> Known steps → fixed chain. Unknown path → adaptive decomposition.

| Dimension | Prompt chaining (fixed pipeline) | Dynamic adaptive decomposition |
|---|---|---|
| Task shape | Predictable, repeatable | Open-ended, exploratory |
| Order | Known up front | Emerges from findings |
| Dependencies | Known | Unknown |
| Goal | Consistency | Discovery |
| Next step | Pre-defined | Depends on intermediate results |
| Risk if misused | Forces a fixed pipeline on exploration | Over-engineers a simple task |

## Bottom line
Use a **fixed sequential chain** when you know the steps and want
consistency; use **dynamic decomposition** when the path depends on
what you discover. Separately, a **large review** = local passes +
an integration pass. Wrong answers: one giant prompt for a big
review; a fixed pipeline for exploration; dynamic agents for a simple
predictable workflow; a full plan before exploring unknown
dependencies.

## Sources
- [[CCA-F Exam Notes]] — task statement 1.6.

## Continue reading
- **Enforcing order** → [[prompt-vs-programmatic-enforcement]]
- **Plan vs direct** → [[plan-mode-vs-direct-execution]]

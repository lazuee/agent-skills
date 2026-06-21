---
title: One Skill, One Topic
impact: HIGH
tags: [scope, focus, organization]
---

# One Skill, One Topic

A skill should cover one coherent topic. If you need "and" to describe what it covers, it's probably two skills.

## Why

- **Relevance**: Agents load skills based on topic match — broad skills load for irrelevant tasks
- **Context budget**: A focused skill fits in context alongside other relevant skills
- **Quality**: Deep coverage of one topic beats shallow coverage of many
- **Maintainability**: Smaller skills are easier to update when the codebase changes

## Signs a Skill Is Too Broad

```markdown
# Bad: Kitchen-sink skill
name: react-best-practices
description: Best practices for React, testing, state management,
  routing, styling, and deployment.

# This skill has 25 rules across 8 categories.
# It loads for ANY React task, wasting context on irrelevant rules.
```

Split it into focused skills:
```
skills/
├── react-component-best-practices/    # Components, hooks, props
├── react-testing-best-practices/      # Testing patterns
├── react-state-management/            # State patterns
└── react-router-data-mode/            # Routing conventions
```

## How to Define Scope

Before writing a skill, answer three questions:

1. **What topic does this cover?** One sentence, no "and"
2. **When should an agent load this?** Specific trigger situations
3. **When should it NOT load?** Explicit boundaries

```markdown
# Good: Clear scope
name: prisma-schema-naming-convention-best-practices
description: Naming conventions for Prisma schemas that map to MySQL
  tables. Use when defining models, reviewing schema changes, or
  introspecting legacy databases.

# Scope: naming conventions only
# Not in scope: query patterns, migrations, client usage, performance
```

## Splitting Signals

Split a skill when:

- **Different triggers**: The "When to Apply" section has unrelated bullets
- **Different audiences**: Some rules are for frontend, others for backend
- **Different frameworks**: Rules mix React, Prisma, and Express conventions
- **>10 rules**: Most skills work best with 3-8 rules
- **Category drift**: You're adding a third or fourth unrelated category

## Related Skills Cross-Reference

Related skills can mention each other without merging:

```markdown
# In react-component-best-practices/SKILL.md
## When to Apply
- Writing or reviewing React components
- For testing patterns, see react-testing-best-practices
- For routing, see react-router-data-mode
```

## Rules

1. One skill = one coherent topic, describable without "and"
2. Define scope before writing: what's in, what's out
3. Split when triggers, audiences, or frameworks diverge
4. Aim for 3-8 rules per skill — if you have 15, you have 2 skills
5. Related skills cross-reference; they don't merge

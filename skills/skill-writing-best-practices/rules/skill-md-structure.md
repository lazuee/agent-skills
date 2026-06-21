---
title: SKILL.md Structure
impact: HIGH
tags: [structure, skill-file]
---

# SKILL.md Structure

The main SKILL.md file has four parts: frontmatter, overview, rules summary, and optional philosophy section.

## Why

- **Quick reference**: Agents can scan SKILL.md to find relevant rules fast
- **Context**: Frontmatter helps agents decide when to load this skill
- **Depth on demand**: Summary links to detailed rules when needed

## 1. Frontmatter

YAML frontmatter with name and description:

```yaml
---
name: topic-best-practices
description: Brief description of what this skill covers. Mention when to use it.
---
```

The description should help agents understand when to reference this skill. Include trigger conditions if relevant.

### Frontmatter Reference

| Field                      | Purpose                                                    |
|----------------------------|------------------------------------------------------------|
| `name`                     | Slash command name (lowercase, hyphens, max 64 chars)      |
| `description`              | **Recommended** - helps Claude decide when to use skill    |
| `argument-hint`            | Autocomplete hint, e.g., `[issue-number]`                  |
| `disable-model-invocation` | `true` = only user can invoke (for deploy, commit, etc.)   |
| `user-invocable`           | `false` = hide from `/` menu (background knowledge)        |

### Writing Effective Descriptions

The `description` is how agents decide whether to load your skill. Write it like a search snippet.

```yaml
# Bad: Too vague — agent can't determine relevance
description: Best practices for coding.

# Good: Specific triggers and scope
description: Naming conventions for Prisma schemas that map to MySQL
  tables. Use when defining models, reviewing schema changes, or
  introspecting legacy databases.
```

Include: what the skill covers, when to trigger it, and specific keywords agents will match against.

## 2. Overview

Title, intro, and application guidance:

```markdown
# Topic Best Practices

Brief intro about what's covered. Mention rule count and categories.

## When to Apply

Reference these guidelines when:

- Doing X
- Working with Y
- Reviewing Z code
```

Keep the intro to 1-2 sentences. The bullet list helps agents quickly assess relevance.

## 3. Rules Summary

Group rules by category with impact levels. Each rule gets:
- Header linking to full file
- One-sentence description
- Optional: ultra-brief example (1-2 lines) if the pattern is hard to grasp from text alone

```markdown
## Rules Summary

### Category Name (IMPACT)

#### rule-name - @rules/rule-name.md

One sentence explaining what to do.

\`\`\`ruby
# Optional: only if a 1-2 line snippet clarifies the pattern
after_create_commit :notify_later
\`\`\`

#### another-rule - @rules/rule-name.md

Another one-sentence explanation.
```

Impact levels:
- **CRITICAL/HIGH** - Core patterns, always follow
- **MEDIUM** - Important but flexible
- **LOW** - Nice-to-haves

**On inline examples in summaries**: Include a code snippet only when the pattern is hard to understand from the description alone. Keep it to 1-2 lines. If the description is clear, skip the example — the rule file has the full code. Don't duplicate multi-line examples from rule files.

## 4. Philosophy (Optional)

End with core principles if the skill embodies a specific approach:

```markdown
## Philosophy

These patterns embody X approach:

1. **Principle One** - Brief explanation
2. **Principle Two** - Brief explanation
```

## Complete Example

```markdown
---
name: example-best-practices
description: Example patterns. Use when working with examples.
---

# Example Best Practices

Patterns for examples. Contains 3 rules in 2 categories.

## When to Apply

- Writing examples
- Reviewing example code

## Rules Summary

### Structure (HIGH)

#### example-structure - @rules/example-structure.md

Examples should be self-contained.

\`\`\`ruby
# Good: Complete example
def complete_example
  setup
  action
  verify
end
\`\`\`

### Style (MEDIUM)

#### example-naming - @rules/example-naming.md

Use descriptive names.

## Philosophy

1. **Clarity** - Examples should be obvious
2. **Brevity** - Show only what matters
```

## Rules

1. Frontmatter has `name` and `description`
2. Overview includes "When to Apply" bullets
3. Rules are grouped by category with impact levels
4. Each rule gets one sentence + optional ultra-brief example
5. Link to full rules with `@rules/rule-name.md`

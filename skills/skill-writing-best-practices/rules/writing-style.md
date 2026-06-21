---
title: Writing Style
impact: MEDIUM
tags: [style, tone, formatting, audience]
---

# Writing Style

Write for an AI agent consuming your skill in a context window. Be direct, cut filler, and make every line earn its place.

## Why

- **Readability**: Natural prose is easier to scan
- **Trust**: AI-sounding text feels generated, not authored
- **Density**: Removing filler packs more value per line
- **Professionalism**: Clean writing reflects clear thinking

## Avoid Horizontal Rules

Don't use `---` as section separators:

```markdown
# Bad
## Section One

Content here.

---

## Section Two

More content.

---

# Good
## Section One

Content here.

## Section Two

More content.
```

Headings already create visual separation.

## Avoid Filler Phrases

Cut phrases that add no information:

```markdown
# Bad
"In this section, we will explore the various ways in which..."
"It's important to note that..."
"As mentioned previously..."
"Let's take a look at..."

# Good
Just say the thing directly.
```

## Avoid Over-Formatting

Don't bold or bullet everything:

```markdown
# Bad: Everything is emphasized
**Always** use `after_commit` for jobs because:
- **It ensures** transaction safety
- **It prevents** race conditions
- **It guarantees** data consistency

# Good: Emphasis is meaningful
Use `after_commit` for jobs. This ensures the transaction has committed
before the job runs, preventing race conditions where the job can't find
the record.
```

Reserve bold for terms being defined or key concepts in lists.

## Use Direct Language

```markdown
# Bad: Passive and hedging
"It is recommended that consideration be given to..."
"One approach that could potentially be utilized..."
"It should be noted that in some cases..."

# Good: Direct
"Use X when Y."
"Consider X for Y situations."
"X doesn't apply when Y."
```

## Keep Paragraphs Short

Long paragraphs are hard to scan:

```markdown
# Bad: Wall of text
When implementing the pattern you should consider that there are multiple
approaches and each has tradeoffs. The first approach involves X which has
the benefit of Y but the downside of Z. The second approach...

# Good: Broken up
Consider two approaches:

**First approach**: X. Benefits from Y but has Z downside.

**Second approach**: A. Better for B situations.
```

## Code Comments Should Be Minimal

Let code speak for itself:

```ruby
# Bad: Over-commented
# This method closes the card by creating a closure record
# and then touching the updated_at timestamp
def close
  # Create the closure record for this card
  create_closure!(user: Current.user)
  # Update the timestamp
  touch
end

# Good: Comments add context code can't express
def close
  create_closure!(user: Current.user)
  touch  # Triggers cache invalidation
end
```

## Write for Agents, Not Humans Browsing Docs

Your primary reader is an AI agent scanning a context window. This changes what matters:

**Lead with the rule, not the preamble.** Agents parse the first line of each section to decide relevance.

```markdown
# Bad: Buried lede
In modern web development, there are many approaches to handling
form submissions. One common pattern that has emerged is the use
of server actions. Let's explore when to use them.

# Good: Rule first
Use server actions for mutations that don't need URL-based navigation.
Use route actions when the mutation should be bookmarkable or shareable.
```

**Use consistent terminology.** If you call it a "concern" in one rule, don't switch to "module" or "mixin" in another.

**Frontmatter matters.** The `description` field in SKILL.md is how agents decide whether to load your skill. Write it like a search snippet — specific keywords, clear scope.

```yaml
# Bad: Vague
description: Best practices for coding.

# Good: Specific triggers
description: Naming conventions for Prisma schemas that map to MySQL
  tables. Use when defining models, reviewing schema changes, or
  introspecting legacy databases.
```

## Rules

1. No horizontal rules (`---`) as separators
2. Cut filler phrases that add no information
3. Reserve formatting (bold, bullets) for emphasis, not decoration
4. Use direct, active language
5. Keep paragraphs short and scannable
6. Code comments explain why, not what
7. Lead with the rule — agents parse first lines for relevance
8. Use consistent terminology across all rules
9. Write specific, keyword-rich frontmatter descriptions

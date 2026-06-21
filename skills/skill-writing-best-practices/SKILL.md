---
name: skill-writing-best-practices
description: Guidelines for creating AI agent skills. Use when writing new skills, documenting coding patterns, or reviewing skill files. Triggers when creating or modifying files in the skills/ directory.
---

# Skill Writing Best Practices

Patterns for creating effective AI agent skills. Contains 10 rules across structure, content, and style.

## When to Apply

- Creating a new skill from scratch
- Extracting patterns from an existing codebase
- Reviewing or improving existing skills
- Converting documentation into skill format

## Rules Summary

### Structure (HIGH)

#### skill-directory-structure - @rules/skill-directory-structure.md

One skill = one directory: `SKILL.md` + `rules/` subdirectory. Directory names use `{topic}-best-practices`.

#### skill-md-structure - @rules/skill-md-structure.md

SKILL.md has four parts: frontmatter, overview with "When to Apply", grouped rule summaries with one-liner descriptions, and optional philosophy.

#### rule-file-structure - @rules/rule-file-structure.md

Each rule file has frontmatter (title, impact, tags), a "Why" section, a "Pattern" section with bad/good code, and numbered takeaways.

#### skill-scope - @rules/skill-scope.md

One skill = one coherent topic. If you need "and" to describe it, it's two skills. Aim for 3-8 rules per skill.

### Content (HIGH)

#### ground-in-source-code - @rules/ground-in-source-code.md

Find the code first, then write the rule. Copy patterns from real files. Never invent APIs or assume framework behavior from memory.

#### concrete-examples - @rules/concrete-examples.md

Every rule needs code examples. Show before/after transformations, use real code, and match example complexity to rule complexity.

#### explain-why - @rules/explain-why.md

Every non-trivial rule needs a "Why" section. Use bolded benefit names with concrete, specific explanations — not vague phrases like "more maintainable".

#### anti-patterns - @rules/anti-patterns.md

Know what to leave out: no tutorial content, no API docs, no duplicating external library docs, no rules without examples.

### Style (MEDIUM)

#### writing-style - @rules/writing-style.md

Write for AI agents consuming context windows. Be direct, cut filler, keep SKILL.md summaries to one sentence per rule, and reserve formatting for emphasis.

### Quality (MEDIUM)

#### validate-before-publishing - @rules/validate-before-publishing.md

Run a self-review checklist before publishing: every rule has an example, code compiles, no contradictions between rules, and SKILL.md references match rule files.

## Philosophy

1. **Grounded** — Rules come from observed code, not assumed knowledge
2. **Focused** — One skill, one topic, 3-8 rules
3. **Concrete** — Every rule has code examples
4. **Reasoned** — Explains why, not just what
5. **Scannable** — One sentence per rule summary; details in rule files
6. **Brevity** — SKILL.md is a summary; rule files hold the depth
7. **Validated** — Tested against real scenarios before publishing
8. **Honest** — Shows when NOT to use a pattern
9. **Natural** — Written like documentation, not AI output
---
title: Validate Before Publishing
impact: MEDIUM
tags: [validation, quality, review]
---

# Validate Before Publishing

Run through a checklist before considering a skill complete. Skills that pass validation produce reliable agent output.

## Why

- **Catch errors early**: One bad rule can cause agents to generate wrong code
- **Consistency**: Validation catches contradictions between SKILL.md and rule files
- **Completeness**: Ensures every rule has what it needs to be followed

## Self-Review Checklist

After writing a skill, verify each item:

### Structure

- [ ] SKILL.md has frontmatter with `name` and `description`
- [ ] Every rule file listed in SKILL.md exists in `rules/`
- [ ] Every file in `rules/` is listed in SKILL.md
- [ ] Rule file names match the `kebab-case.md` pattern

### Content

- [ ] Every rule has at least one code example
- [ ] Code examples would actually compile/run in the target language
- [ ] No APIs or methods are invented — all verified in source or docs
- [ ] Bad/good examples show the same scenario, just done differently
- [ ] "Why" sections use concrete reasons, not vague phrases

### Consistency

- [ ] SKILL.md summary count matches actual rule count
- [ ] Category groupings in SKILL.md match rule file tags/topics
- [ ] Terminology is consistent across all rules
- [ ] No contradictions between rules (check: do any rules say opposite things?)

### Scope

- [ ] All rules relate to the same topic
- [ ] "When to Apply" triggers are specific, not generic
- [ ] The skill doesn't duplicate another existing skill

## Test: Give It to an Agent

The best validation is to actually use the skill:

1. Write a prompt that should trigger the skill
2. Ask an agent to complete the task using the skill
3. Check the output:
   - Did it follow the rules?
   - Did it hallucinate any patterns?
   - Did it miss important rules?
   - Would you accept this code in a PR?

If the agent produces wrong code, the skill has a problem — fix the rule, not the agent.

## Common Validation Failures

**Orphaned rules**: SKILL.md references `rules/foo.md` but the file doesn't exist, or `rules/bar.md` exists but isn't listed.

**Stale examples**: Code examples that worked in an older version of the framework but use deprecated APIs.

**Contradicting rules**: One rule says "always use named exports" while another example uses default exports without marking it as bad.

**Overlapping rules**: Two rules cover the same concept with slightly different wording. Merge them.

## Rules

1. Run the self-review checklist before publishing
2. Verify every rule file in `rules/` is referenced in SKILL.md and vice versa
3. Test the skill by asking an agent to use it on a real task
4. Fix contradictions — don't let rules say opposite things
5. If the agent hallucinates, the skill is unclear — rewrite the rule

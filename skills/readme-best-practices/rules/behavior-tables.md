---
title: Use Tables for Condition to Outcome
impact: HIGH
impactDescription: branching behavior is scannable instead of buried in bullets
tags: tables, lists, behavior
---

## Use Tables for Condition to Outcome

When behavior depends on a condition (exit code, flag, input type), use a two-column table. When steps happen in order, use a numbered list. Use plain bullets only for unordered, parallel items.

Do not bold the outcome words. The table already separates them.

**Why:** bullets with bold lead-ins and nested explanations are hard to compare. A table lets the reader find their case in one glance.

**Incorrect:**

````markdown
- exit `0` → the file is **copied**;
- exit `1` → the file **conflicts**: the run stops and prints the diff;
- anything else → the file is **skipped**.
````

**Correct:**

````markdown
| Result | What happens |
|---|---|
| Exit `0` | The file is copied |
| Exit `1` | The run stops and prints the diff |
| Anything else | The file is skipped |
````

Put the default or fallback case last.

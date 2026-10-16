---
name: readme-best-practices
description: Guidelines for writing README.md files that read like a person wrote them, not like generated filler. Use when creating, rewriting or reviewing a README.md, when documenting a package, plugin, CLI, patch or script, or when a README sounds like AI slop. Triggers when creating or modifying README.md files.
---

# README Best Practices

Rules for short, plain, accurate READMEs. Contains 7 rules covering structure, wording, and accuracy.

## When to Apply

- Creating a README.md for a new package, plugin, CLI, patch or script
- Rewriting a README that reads like marketing copy
- Reviewing a README before it ships
- Converting notes or a pasted draft into a README

## Rules Summary

### Structure (HIGH)

#### readme-structure - @rules/readme-structure.md

Title, tagline, Installation, Usage, Uninstall (only if needed), License. Skip sections with nothing to say.

#### tagline - @rules/tagline.md

One blockquote sentence: what it does and what changes for the user.

### Writing (HIGH)

#### plain-prose - @rules/plain-prose.md

Short sentences, concrete subjects, no hype words, no em dashes, no decorative bold.

#### install-and-uninstall - @rules/install-and-uninstall.md

Requirements and failure messages first, then commands, then verification, re-run and restart behavior.

#### behavior-tables - @rules/behavior-tables.md

Use a table for condition to outcome, a numbered list for sequences.

### Accuracy (HIGH)

#### verified-facts - @rules/verified-facts.md

Read the source first. Every flag, path, exit code, timeout and error string comes from the code or the user.

### Formatting (MEDIUM)

#### code-blocks - @rules/code-blocks.md

Language tags, minimal comments, relative paths, four-backtick fences when the README sits inside another markdown block.

## Output

Write the result to `README.md` in the package directory. After writing, tell the user in one or two lines which facts you could not verify and which assumptions you made (license, years, package manager).

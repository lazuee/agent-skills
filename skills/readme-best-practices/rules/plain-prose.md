---
title: Write Plain Prose
impact: HIGH
impactDescription: removes the patterns readers recognize as generated text
tags: writing, style, tone
---

## Write Plain Prose

Write the way you would explain it to a coworker. One idea per sentence. Name the subject and the verb.

Avoid these:

- Hype words: powerful, seamless, robust, blazing, effortless, leverage, supercharge, "simply", "just"
- Em dashes. Use a period or a comma.
- Bold used for emphasis. Bold a word only when it is a UI label the reader must find.
- Sentence fragments that act as labels ("Idempotent. Safe to re-run.")
- Several parentheticals or semicolons in one sentence
- Filler openers: "Note that", "It's worth mentioning", "In order to"
- Rule-of-three lists written for rhythm

**Why:** these patterns carry no information, and a reader who spots them skims the whole file. Short sentences also keep exact details, such as error names and flags, easy to see.

**Incorrect:**

````markdown
Install the CLI first — the command fails with `ERR_NO_GIT` (or `ERR_NO_NODE`) if `git` or `node` is not on `PATH`:
````

**Correct:**

````markdown
`git` and `node` both need to be on your `PATH`. If either is missing, the command exits with `ERR_NO_GIT` or `ERR_NO_NODE`.
````

**Incorrect:**

````markdown
Idempotent — an existing config block is replaced, so re-run after an upgrade.
````

**Correct:**

````markdown
The command is idempotent. It replaces any existing config block, so run it again after upgrading.
````

Keep every technical detail when rewriting. Cut words, not facts.

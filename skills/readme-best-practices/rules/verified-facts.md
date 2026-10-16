---
title: State Only What You Verified
impact: HIGH
impactDescription: a wrong flag or error name in a README costs the reader more than a missing one
tags: accuracy, sources, verification
---

## State Only What You Verified

Before writing, read the code, scripts, config and any existing README. Take every command, flag, path, exit code, timeout, error string and version from those files or from the user. Copy exact strings instead of retyping them.

If a detail cannot be verified, leave it out of the README and list it in your reply to the user. Do not guess, and do not write "should work".

Only add a "tested against" line when the user or the repo gives you the version or date.

**Why:** a README is trusted by default. An invented exit code or compatibility claim sends readers into debugging with a false premise.

**Incorrect:**

````markdown
Works with all versions of Node and any shell.
````

**Correct:**

````markdown
Requires Node 20 or later. Tested on bash and zsh.
````

(only when `engines` in `package.json` says 20 and the user confirmed the shells)

When rewriting an existing README, keep every factual detail from it. If you suspect one is wrong, say so in your reply and leave the original wording.

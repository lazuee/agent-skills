---
title: Order Install Steps by What the Reader Needs
impact: HIGH
impactDescription: readers hit the failure case before they hit the command
tags: installation, uninstall, prerequisites
---

## Order Install Steps by What the Reader Needs

In Installation, write in this order:

1. Requirements, with the exact error or symptom if one is missing
2. Install command for each platform, one command per line with a short platform comment
3. How to check it worked, as its own sentence after the block
4. The main command (run the setup, enable the plugin)
5. Behavior on re-run, and whether a restart is needed
6. Compatibility: what version or file layout is required, and what it was tested against

Match the package manager to the repo's lockfile. For Node packages, show the dev flag when the package is a dev tool.

If the tool edits files outside its own folder, add `### Uninstall` with a copy-pasteable command or script, and say what to restart afterwards.

**Why:** a reader who lacks a requirement should learn that before running anything. Mixing the verify step into a sentence about the install command hides it.

**Incorrect:**

````markdown
Install `foo` first, then verify with `which foo`, then run the setup:
````

**Correct:**

````markdown
`foo` needs to be on your `PATH`. If it is missing, setup exits with `ERR_FOO_NOT_FOUND`.

```bash
brew install foo
```

Check that it's installed with `which foo`, then run the setup:
````

---
title: Format Code Blocks Consistently
impact: MEDIUM
impactDescription: copy-paste works and the file renders correctly everywhere
tags: formatting, code, fences
---

## Format Code Blocks Consistently

- Add a language tag to every fenced block (`bash`, `js`, `ts`, `json`).
- Use comments inside a block only for platform labels or options that are not obvious from their name.
- Put file paths, commands, flags and error names in inline backticks in prose.
- Show full, working config for the first example, not a fragment, so it can be pasted as is.
- Use one block per purpose. Do not chain unrelated commands.
- When you show a whole README inside another markdown block (for example in a chat reply), wrap it in four backticks so the inner three-backtick blocks do not break it.

**Why:** untagged blocks lose highlighting, and a broken outer fence makes the rest of the README render as plain text.

**Incorrect:**

````markdown
```
brew install foo # you can also use the curl installer if you prefer, or winget on windows
```
````

**Correct:**

````markdown
```bash
brew install foo         # macOS / Linux
winget install foo       # Windows
```
````

For a config option that needs explaining, one trailing comment is fine:

````markdown
```js
export default defineConfig({
  entry: "src/index.ts", // Path to your entry file
});
```
````

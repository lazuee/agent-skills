---
title: Use the House Section Order
impact: HIGH
impactDescription: readers find install and usage in the same place in every project
tags: structure, headings, sections
---

## Use the House Section Order

Use this order and heading levels:

| Section | Level | Include when |
|---|---|---|
| Title (package name) | `##` | always |
| Tagline | blockquote | always |
| Installation | `###` | always |
| Usage | `###` | the user runs or calls something after install |
| Uninstall | `###` | the tool edits files outside its own folder |
| License | `##` | always |

Sub-sections inside Installation or Usage use `####`.

**Why:** the same layout across repos means nobody hunts for the install command. Sections with no content (Features, Why, Contributing, Roadmap) pad the file and push the install command down.

**Name headings after what the reader does.** If nothing is run after install (patches, presets, plugins that load on their own), use `### How it works` instead of `### Usage`.

**Incorrect:**

````markdown
## my-lib

### ✨ Features
### 🚀 Why my-lib?
### Getting Started
### Contributing
````

**Correct:**

````markdown
## my-lib

> `my-lib` is a Vite plugin that adds X to Y.

### Installation

### Usage

## License

This project is licensed under the MIT License - see the [LICENSE.md](../../LICENSE.md) file for details

Copyright © `2025-2026` `lazuee`
````

Check the repo's LICENSE file for the license name and the years before writing the footer. Keep the relative path to LICENSE.md correct for where the README lives.

---
title: Tree-shaking Imports
impact: HIGH
tags: [imports, bundle-size, formats]
---

# Tree-shaking Imports

When the format is known, import from the format-specific entry point to keep bundles small.

## Why

- Reduce bundle size in web and edge builds
- Speed cold starts by skipping unused parsers
- Make intent explicit for reviewers

## Pattern

```ts
// Bad - pulls in all formats
import { readXlsx, writeXlsx } from "hucre";

// Good - only XLSX
import { readXlsx, writeXlsx } from "hucre/xlsx";

// Good - only CSV
import { parseCsv, writeCsv } from "hucre/csv";
```

## Rules

1. Prefer `hucre/xlsx`, `hucre/csv`, `hucre/ods`, `hucre/json`, or `hucre/xml` when the file type is known.
2. Use the root `hucre` entry only when format auto-detection is required.

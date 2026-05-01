---
title: Round-trip Preservation
impact: MEDIUM
tags: [round-trip, xlsx, preservation]
---

# Round-trip Preservation

Use the round-trip APIs when you need to keep charts, VBA, or unknown workbook parts intact.

## Why

- Preserves content that hucre does not model directly
- Prevents accidental loss of macros, themes, or drawings
- Makes minimal edits safer for user-provided files

## Pattern

```ts
// Bad - reserialize with loss of unknown parts
import { readXlsx, writeXlsx } from "hucre/xlsx";

const wb = await readXlsx(buf);
wb.sheets[0].rows[0][0] = "Updated";
const output = await writeXlsx({ sheets: wb.sheets });

// Good - round-trip mode
import { openXlsx, saveXlsx } from "hucre/xlsx";

const workbook = await openXlsx(buf);
workbook.sheets[0].rows[0][0] = "Updated";
const output = await saveXlsx(workbook);
```

## Rules

1. Use `openXlsx` and `saveXlsx` whenever you must preserve unknown parts.
2. Use `readXlsx` and `writeXlsx` only when you control the full workbook output.

---
title: Sparse Cell Overrides
impact: MEDIUM
tags: [cells, styling, performance]
---

# Sparse Cell Overrides

Use the `cells` map for targeted styles, formulas, or metadata instead of rebuilding full row data.

## Why

- Keeps memory usage low for large sheets
- Lets you style or annotate a few cells without touching the rest
- Separates values (`rows` or `data`) from formatting concerns

## Pattern

```ts
// Bad - rebuild every row just to style headers
const rows = data.map((row) => row.map((cell) => cell));

// Good - use a sparse cells map (row,col are 0-based)
import { writeXlsx } from "hucre/xlsx";

const cells = new Map([
  ["0,0", { style: { font: { bold: true } } }],
  ["0,1", { style: { font: { bold: true } } }],
  ["1,1", { style: { numFmt: "$#,##0.00" } }],
]);

const output = await writeXlsx({
  sheets: [{
    name: "Products",
    rows: [
      ["Name", "Price"],
      ["Widget", 9.99],
    ],
    cells,
  }],
});
```

## Rules

1. Use `rows` or `data` for values and reserve `cells` for targeted overrides.
2. Keep `cells` keys in the `"row,col"` format with zero-based indexes.
3. Avoid building giant `rows` arrays just to apply styles to a few cells.

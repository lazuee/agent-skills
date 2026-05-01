---
title: Stream Large Sheets
impact: HIGH
tags: [streaming, performance, large-files]
---

# Stream Large Sheets

For large datasets, stream rows or write incrementally to avoid loading entire workbooks into memory.

## Why

- Prevents high memory usage
- Enables processing files larger than RAM
- Avoids Excel row limit surprises by splitting sheets

## Pattern

```ts
// Bad - load everything for a large file
import { readXlsx } from "hucre/xlsx";

const wb = await readXlsx(buf);
for (const row of wb.sheets[0].rows) {
  processRow(row);
}

// Good - stream rows
import { streamXlsxRows } from "hucre/xlsx";

for await (const row of streamXlsxRows(buf)) {
  processRow(row.values);
}

// Good - stream writing
import { XlsxStreamWriter, XLSX_MAX_ROWS_PER_SHEET } from "hucre/xlsx";

const writer = new XlsxStreamWriter({
  name: "BigData",
  columns: [{ header: "ID" }, { header: "Value" }],
});

for (const item of items) {
  writer.addRow([item.id, item.value]);
}

// Sheets auto-split after XLSX_MAX_ROWS_PER_SHEET
const output = await writer.finish();
```

## Rules

1. Use `streamXlsxRows` for large reads and `XlsxStreamWriter` for large writes.
2. Treat `XLSX_MAX_ROWS_PER_SHEET` as a hard limit when designing exports.
3. Avoid `readXlsx` for massive files unless you need full workbook access.

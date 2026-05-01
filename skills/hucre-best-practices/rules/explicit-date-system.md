---
title: Explicit Date System and Formats
impact: MEDIUM
tags: [dates, formatting, numfmt]
---

# Explicit Date System and Formats

Set `dateSystem` and `numFmt` explicitly when working with dates to avoid Excel serial mismatches.

## Why

- Excel uses two date systems (1900 and 1904)
- Date serials are just numbers without a format
- Explicit formats prevent locale and display confusion

## Pattern

```ts
// Bad - implicit date handling
import { writeXlsx } from "hucre/xlsx";

const output = await writeXlsx({
  sheets: [{ name: "Report", rows: [[new Date()]] }],
});

// Good - explicit date system and format
import { writeXlsx } from "hucre/xlsx";

const output = await writeXlsx({
  dateSystem: "1900",
  sheets: [{
    name: "Report",
    columns: [{ header: "Date", key: "date", numFmt: "yyyy-mm-dd" }],
    data: [{ date: new Date() }],
  }],
});
```

## Rules

1. Set `dateSystem` in `readXlsx` or `writeXlsx` when you know the source system.
2. Apply a date `numFmt` whenever you write Date values.
3. Use `dateToSerial` and `serialToDate` when you need stable numeric dates.

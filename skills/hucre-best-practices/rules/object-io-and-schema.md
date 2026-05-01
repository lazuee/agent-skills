---
title: Object I/O and Schema Validation
impact: HIGH
tags: [schema, objects, validation]
---

# Object I/O and Schema Validation

Use object-based APIs and schema validation to avoid brittle column indexes.

## Why

- Headers change less often than column positions
- Schema validation catches data issues early
- Coercion and defaults stay centralized

## Pattern

```ts
// Bad - positional columns and ad-hoc parsing
import { readXlsx } from "hucre/xlsx";

const wb = await readXlsx(buf);
const rows = wb.sheets[0].rows;
const products = rows.slice(1).map((row) => ({
  name: String(row[0] ?? ""),
  price: Number(row[1] ?? 0),
}));

// Good - headers plus schema validation
import { readXlsxObjects } from "hucre/xlsx";
import { validateWithSchema } from "hucre";

const { data } = await readXlsxObjects(buf, {
  sheet: 0,
  headerRow: 0,
  skipEmptyRows: true,
});

const result = validateWithSchema(data, {
  Name: { type: "string", required: true },
  Price: { type: "number", required: true, min: 0 },
});

if (result.errors.length > 0) {
  throw new Error("Invalid spreadsheet input");
}

const safeProducts = result.data;
```

## Rules

1. Use `readXlsxObjects` or `parseCsvObjects` when a header row defines your columns.
2. Always check `result.errors` from `validateWithSchema` before using data.
3. Use `transformHeader` when you need normalized keys for downstream code.

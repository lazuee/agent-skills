---
name: hucre-best-practices
description: Use when working with hucre to read/write spreadsheets, choose format-specific imports, stream large data, validate schemas, and preserve round-trip content.
---

# Hucre Best Practices

Practical rules for using hucre's read/write APIs, streaming, and schema validation. Contains 6 rules across 5 categories.

## When to Apply

- Reading or writing XLSX, CSV, ODS, JSON, or XML with hucre
- Choosing between the unified API and format-specific entry points
- Handling large datasets or memory-sensitive workflows
- Preserving charts, VBA, or unknown workbook parts
- Applying styles, dates, or validation rules

## Rules Summary

### Imports & Formats (HIGH)

#### tree-shaking-imports - @rules/tree-shaking-imports.md

Use format-specific entry points when the file type is known to keep bundles small.

### Data Integrity (HIGH)

#### object-io-and-schema - @rules/object-io-and-schema.md

Prefer object-based reads plus schema validation over positional column parsing.

### Scale (HIGH)

#### stream-large-sheets - @rules/stream-large-sheets.md

Stream rows or write incrementally to avoid loading full workbooks.

### Preservation & Styling (MEDIUM)

#### round-trip-preservation - @rules/round-trip-preservation.md

Use `openXlsx` and `saveXlsx` when you need to keep unknown parts.

#### sparse-cell-overrides - @rules/sparse-cell-overrides.md

Use `cells: Map` for targeted styling or formulas instead of rebuilding full rows.

### Dates & Numbers (MEDIUM)

#### explicit-date-system - @rules/explicit-date-system.md

Set `dateSystem` and `numFmt` explicitly when working with dates.

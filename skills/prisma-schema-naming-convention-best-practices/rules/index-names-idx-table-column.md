---
title: Index Names Use idx_table_column
impact: HIGH
tags: [prisma, schema, indexes, constraints, naming]
---

# Index Names Use idx_table_column

Name indexes with `idx_<table>_<column>` in lowercase using Prisma `map`. For foreign keys, use `idx_<table>_<referencedtable>id`.

## Why

- **Traceability**: Index names reveal table and column at a glance
- **Operational clarity**: Easier to debug query plans and migration diffs
- **Determinism**: Explicit names prevent tool-generated random naming

## Pattern

```prisma
// Bad: Auto-generated index names
model users_logs {
  id          Int      @id @default(autoincrement())
  UserID      Int
  DateCreated DateTime @db.DateTime(0)

  @@index([UserID])
  @@map("users_logs")
}

// Good: Explicit idx_ naming with table + column
model users_logs {
  id          Int      @id @default(autoincrement())
  UserID      Int
  DateCreated DateTime @db.DateTime(0)

  @@index([UserID], map: "idx_users_logs_userid")
  @@index([DateCreated], map: "idx_users_logs_datecreated")
  @@map("users_logs")
}

// Good: FK index naming pattern
model providers_locations {
  id         Int @id @default(autoincrement())
  ProviderID Int

  @@index([ProviderID], map: "idx_providers_locations_providerid")
  @@map("providers_locations")
}
```

## Rules

1. Always define index `map` names for deterministic DB naming
2. Use lowercase `idx_<table>_<column>` format
3. For FK indexes, use `idx_<table>_<referencedtable>id`
4. Convert PascalCase columns to lowercase in index map names (`UserID` -> `userid`)

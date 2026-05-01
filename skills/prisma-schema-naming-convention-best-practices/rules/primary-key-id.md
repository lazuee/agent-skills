---
title: Primary Keys Are id
impact: HIGH
tags: [prisma, schema, primary-key, naming]
---

# Primary Keys Are id

Use `id` as the primary key column name for every model.

## Why

- **Uniformity**: Every table has the same primary key name
- **Lower cognitive load**: Queries and relations do not need per-table PK exceptions
- **Safer refactors**: Shared conventions reduce migration mistakes

## Pattern

```prisma
// Bad: Table-specific primary key names
model users {
  UserID Int @id @default(autoincrement())
  Email  String @db.VarChar(255)

  @@map("users")
}

// Good: Primary key is always id
model users {
  id     Int    @id @default(autoincrement())
  Email  String @db.VarChar(255)

  @@map("users")
}

// Good: relation keys still use <ReferencedTable>ID in child tables
model users_logs {
  id     Int @id @default(autoincrement())
  UserID Int

  @@map("users_logs")
}
```

## Rules

1. Define `id Int @id @default(autoincrement())` on each model
2. Do not rename primary keys to table-specific names
3. Keep relation columns separate from the primary key (`UserID`, `ProviderID`, etc.)

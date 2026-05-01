---
title: Join Tables Use table1_table2
impact: HIGH
tags: [prisma, schema, join-table, relations, naming]
---

# Join Tables Use table1_table2

Name join tables with the `<table1>_<table2>` snake_case pattern. Use full table names so relationship intent stays explicit.

## Why

- **Discoverability**: Developers can find join tables quickly
- **Consistency**: Relation tables follow one naming pattern across the schema
- **Interoperability**: SQL queries and reporting tools read naturally

## Pattern

```prisma
// Bad: Join table uses mixed or shortened naming
model providersPermit {
  id                Int @id @default(autoincrement())
  ProviderID        Int
  ClearancePermitID Int

  @@map("providersPermit")
}

// Good: table1_table2 naming for link/log style tables
model users_logs {
  id     Int @id @default(autoincrement())
  UserID Int

  @@map("users_logs")
}

// Good: Join table uses <table1>_<table2>
model providers_clearances_permits {
  id                Int @id @default(autoincrement())
  ProviderID        Int
  ClearancePermitID Int

  providers          providers          @relation(fields: [ProviderID], references: [id], onDelete: NoAction, onUpdate: NoAction)
  clearances_permits clearances_permits @relation(fields: [ClearancePermitID], references: [id], onDelete: NoAction, onUpdate: NoAction)

  @@index([ProviderID], map: "idx_providers_clearances_permits_providerid")
  @@index([ClearancePermitID], map: "idx_providers_clearances_permits_clearancepermitid")
  @@map("providers_clearances_permits")
}
```

## Rules

1. Use `<table1>_<table2>` for join table names
2. Keep join table names in snake_case
3. Include both FK columns with `<ReferencedTable>ID` naming
4. Add indexes for each FK using `idx_` map names

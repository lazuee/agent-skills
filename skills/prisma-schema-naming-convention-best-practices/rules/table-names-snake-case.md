---
title: Table Names Use snake_case
impact: HIGH
tags: [prisma, schema, naming, tables]
---

# Table Names Use snake_case

Name database tables in snake_case plural form. Keep names stable and descriptive, for example `users`, `users_logs`, and `service_providers`.

## Why

- **Consistency**: Table names match the naming already used across the schema
- **Readability**: snake_case table names are easy to scan in SQL and migrations
- **Compatibility**: Prevents case-sensitivity issues across tools and environments

## Pattern

```prisma
// Bad: Mixed casing and singular naming in table mapping
model userLog {
  id Int @id @default(autoincrement())

  @@map("UserLog")
}

// Good: snake_case plural table naming
model users_logs {
  id Int @id @default(autoincrement())

  @@map("users_logs")
}

// Good: Domain-specific table names stay snake_case
model service_providers {
  id   Int    @id @default(autoincrement())
  Name String @db.VarChar(255)

  @@map("service_providers")
}
```

## Rules

1. Use snake_case for every table name
2. Use plural table names (`users`, not `user`)
3. Avoid PascalCase, camelCase, and spaces in table names
4. Keep table naming explicit with `@@map("<table_name>")`

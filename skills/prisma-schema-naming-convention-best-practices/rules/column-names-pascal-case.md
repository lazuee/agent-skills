---
title: Column Names Use PascalCase
impact: HIGH
tags: [prisma, schema, naming, columns]
---

# Column Names Use PascalCase

Name columns in PascalCase. Use exact field names like `UserID`, `Details`, `DateCreated`, and `HashPassword`.

## Why

- **Predictability**: Field names follow one style across all models
- **Alignment**: Matches existing database contracts and generated client types
- **Clarity**: Important suffixes like `ID` stay visible and consistent

## Pattern

```prisma
// Bad: snake_case columns mixed with PascalCase columns
model users_logs {
  id           Int      @id @default(autoincrement())
  user_id      Int
  details      String   @db.Text
  DateCreated  DateTime @db.DateTime(0)

  @@map("users_logs")
}

// Good: all business columns use PascalCase
model users_logs {
  id          Int      @id @default(autoincrement())
  UserID      Int
  Details     String   @db.Text
  DateCreated DateTime @db.DateTime(0)

  @@map("users_logs")
}
```

## Rules

1. Use PascalCase for non-primary-key columns
2. Keep acronym suffixes uppercase where established (`UserID`, `ProviderID`)
3. Avoid snake_case and camelCase for column names
4. Preserve exact casing during schema updates and migrations

---
title: Declare Unique Constraints Explicitly
impact: HIGH
tags: [prisma, schema, unique, constraints]
---

# Declare Unique Constraints Explicitly

Define unique constraints directly in the Prisma schema for every business-unique field set, including `Name`, `Email`, `Username`, and `HashPassword` combinations.

## Why

- **Data integrity**: Uniqueness is enforced in the database, not just in app logic
- **Clear contracts**: The schema shows which values must be unique
- **Safer writes**: Constraint violations fail fast and predictably

## Pattern

```prisma
// Bad: No explicit unique constraints
model users {
  id           Int    @id @default(autoincrement())
  Email        String @db.VarChar(255)
  Username     String @db.VarChar(255)
  HashPassword String @db.VarChar(255)

  @@map("users")
}

// Good: Explicit unique constraints on business keys
model users {
  id           Int    @id @default(autoincrement())
  Email        String @db.VarChar(255)
  Username     String @db.VarChar(255)
  HashPassword String @db.VarChar(255)

  @@unique([Email, Username, HashPassword], map: "uq_users_email_username_hashpassword")
  @@map("users")
}

// Good: Explicit single-column unique constraint
model clearances_permits {
  id   Int    @id @default(autoincrement())
  Name String @db.VarChar(255)

  @@unique([Name], map: "uq_clearances_permits_name")
  @@map("clearances_permits")
}
```

## Rules

1. Define unique constraints in schema, never only in application code
2. Use `@@unique([...])` for composite uniqueness
3. Add `map` names for constraints when you need deterministic database names
4. Keep unique columns aligned with real business identity rules

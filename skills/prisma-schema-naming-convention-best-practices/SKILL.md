---
name: prisma-schema-naming-convention-best-practices
description: Naming conventions for Prisma schemas that map to MySQL tables using snake_case tables, PascalCase columns, id primary keys, ReferencedTableID foreign keys, explicit unique constraints, and idx_ index names.
---

# Prisma Schema Naming Convention Best Practices

Conventions for writing Prisma schemas that match a legacy MySQL naming contract. Contains 7 rules across table, key, and constraint naming.

## When to Apply

- Defining or updating models in `**/prisma/schema.prisma`
- Reviewing pull requests that change Prisma model names, keys, or constraints
- Creating new tables and relations that must stay consistent with existing naming
- Introspecting a legacy database and normalizing naming in the Prisma schema

## Rules Summary

### Table and Column Naming (HIGH)

#### table-names-snake-case - @rules/table-names-snake-case.md

Database table names use snake_case plural form, such as `users`, `users_logs`, and `providers_locations`.

```prisma
model users_logs {
  @@map("users_logs")
}
```

#### column-names-pascal-case - @rules/column-names-pascal-case.md

Columns use PascalCase names such as `UserID`, `Details`, and `DateCreated`.

```prisma
model users_logs {
  UserID      Int
  DateCreated DateTime @db.DateTime(0)
}
```

### Key Naming (HIGH)

#### primary-key-id - @rules/primary-key-id.md

Every model uses `id` as the primary key field.

```prisma
id Int @id @default(autoincrement())
```

#### foreign-key-referenced-table-id - @rules/foreign-key-referenced-table-id.md

Foreign key columns use `<ReferencedTable>ID`, such as `UserID`, `ProviderID`, and `AccessID`.

```prisma
UserID Int
```

### Relationship and Constraint Naming (HIGH)

#### join-table-table1-table2 - @rules/join-table-table1-table2.md

Join table names use `<table1>_<table2>` in snake_case.

```prisma
model providers_clearances_permits {
  @@map("providers_clearances_permits")
}
```

#### explicit-unique-constraints - @rules/explicit-unique-constraints.md

Declare unique constraints explicitly on business-unique columns such as `Name`, `Email`, and `Username`.

```prisma
@@unique([Email, Username, HashPassword], map: "uq_users_email_username_hashpassword")
```

#### index-names-idx-table-column - @rules/index-names-idx-table-column.md

Indexes use `idx_<table>_<column>` lowercase names via `map`, especially for foreign keys.

```prisma
@@index([UserID], map: "idx_users_logs_userid")
```

## References

- https://www.prisma.io/docs/orm/prisma-schema/data-model/database-mapping
- https://www.prisma.io/docs/orm/prisma-schema/data-model/indexes

---
title: Foreign Keys Use ReferencedTableID
impact: HIGH
tags: [prisma, schema, foreign-key, relations, naming]
---

# Foreign Keys Use ReferencedTableID

Name every foreign key column as `<ReferencedTable>ID`. Examples: `UserID`, `ProviderID`, `AccessID`, `ClearancePermitID`.

## Why

- **Immediate intent**: The column name tells you which table it references
- **Stable conventions**: New relations follow an obvious naming recipe
- **Cleaner reviews**: Naming mismatches stand out quickly in diffs

## Pattern

```prisma
// Bad: Foreign key names are ambiguous or inconsistent
model users_logs {
  id      Int @id @default(autoincrement())
  UserRef Int

  user users @relation(fields: [UserRef], references: [id])

  @@map("users_logs")
}

// Good: FK follows <ReferencedTable>ID
model users_logs {
  id          Int      @id @default(autoincrement())
  UserID      Int
  Details     String   @db.Text
  DateCreated DateTime @db.DateTime(0)

  user users @relation(fields: [UserID], references: [id], onDelete: NoAction, onUpdate: NoAction)

  @@index([UserID], map: "idx_users_logs_userid")
  @@map("users_logs")
}
```

## Rules

1. FK columns must use `<ReferencedTable>ID`
2. Keep the same casing pattern as other columns (PascalCase)
3. `@relation(fields: [...], references: [id])` must point to the matching FK field
4. Add an FK index named with the `idx_` convention

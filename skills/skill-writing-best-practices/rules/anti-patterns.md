---
title: Know What to Leave Out
impact: HIGH
tags: [content, scope, brevity]
---

# Know What to Leave Out

Skills capture conventions, not documentation. Knowing what *not* to include is as important as knowing what to write.

## Why

- **Context budget**: Agents load skills into limited context windows — every line costs tokens
- **Signal-to-noise**: Irrelevant content buries the rules that matter
- **Maintenance**: Less content means fewer things to keep up to date
- **Focus**: A skill that tries to do everything does nothing well

## Anti-Pattern: Tutorial Content

Skills aren't tutorials. Don't teach basics — encode decisions.

```markdown
# Bad: Teaching how React hooks work
## useEffect

The useEffect hook lets you perform side effects in function components.
It runs after every render by default. You can control when it runs by
passing a dependency array...

# Good: Encoding a convention about useEffect
## Effect Dependencies

Always include all reactive values in the dependency array. Omitting
dependencies causes stale closures — the effect captures old values.

\`\`\`tsx
// Bad: missing dependency
useEffect(() => { fetchData(id) }, []);

// Good: all reactive values listed
useEffect(() => { fetchData(id) }, [id]);
\`\`\`
```

## Anti-Pattern: API Documentation

Don't copy API docs. Link to them and add your convention on top.

```markdown
# Bad: Restating the API
The `useFetcher` hook returns an object with these properties:
- `state`: "idle" | "loading" | "submitting"
- `data`: The data from the action or loader
- `submit()`: Submits form data to a route action
- `load()`: Loads data from a route loader

# Good: Convention on top of the API
## Fetcher vs Navigate

Use `useFetcher` when the mutation shouldn't change the URL.
Use `submit` (from `useSubmit`) when navigating after mutation.

\`\`\`tsx
// Toggle a bookmark — no navigation needed
const fetcher = useFetcher();

// Create a record — navigate to it after
const submit = useSubmit();
\`\`\`
```

## Anti-Pattern: Duplicating External Docs

If a library's own docs cover it well, don't repeat it. Add only your project's conventions.

```markdown
# Bad: Copying Prisma docs
## Creating Records

To create a record in Prisma, use the `create` method:

\`\`\`ts
const user = await prisma.user.create({
  data: { email: "alice@prisma.io" },
});
\`\`\`

# Good: Adding project conventions
## Naming: Model Fields

Field names use camelCase in the schema. The `@@map` directive
handles the database column naming.

\`\`\`prisma
model User {
  firstName String @map("first_name")
}
\`\`\`
```

## Anti-Pattern: Rules Without Examples

A rule without an example is just a suggestion. Every rule needs at least one code snippet.

```markdown
# Bad: Abstract rule
Use proper error handling patterns in your API routes.

# Good: Concrete rule
Return typed error responses from API routes. Use a shared error
schema so clients can parse failures consistently.

\`\`\`ts
// Bad: generic string error
return NextResponse.json({ error: "Not found" }, { status: 404 });

// Good: typed error shape
return NextResponse.json(
  { error: { code: "NOT_FOUND", message: "User not found" } },
  { status: 404 }
);
\`\`\`
```

## Anti-Pattern: Overlapping Rules

Each rule should cover one concept. If two rules say similar things, merge them.

```markdown
# Bad: Two rules about the same thing
## Rule: Use Named Exports
Use named exports for better tooling support.

## Rule: Avoid Default Exports
Default exports make refactoring harder...

# Good: One rule, clear title
## Use Named Exports

Use named exports instead of default exports. Named exports
enable better IDE autocomplete, safer refactoring, and
consistent import syntax.

\`\`\`ts
// Bad
export default function formatCurrency() {}

// Good
export function formatCurrency() {}
\`\`\`
```

## Anti-Pattern: Scope Creep at the Skill Level

A skill that covers too many topics becomes noise. If you need "and" to describe what it covers, split it.

```markdown
# Bad: Kitchen-sink skill
name: fullstack-best-practices
description: Best practices for React, Prisma, Express, testing,
  deployment, and database design.

# This loads for EVERY task, wasting context on irrelevant rules.

# Good: Focused skills
name: react-component-best-practices
description: Conventions for React components, hooks, and props.

name: prisma-schema-naming-convention-best-practices
description: Naming conventions for Prisma schemas that map to MySQL.
```

Related skills can cross-reference each other — they don't need to merge.

## Rules

1. Skills encode decisions and conventions, not tutorials or API docs
2. Every rule needs at least one code example — no exceptions
3. Link to external docs instead of copying them
4. One concept per rule — merge overlapping rules
5. If you can't show a concrete example, the rule isn't ready
6. One skill = one topic — split when you need "and" to describe it

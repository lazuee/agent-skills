---
title: Ground Rules in Source Code
impact: CRITICAL
tags: [hallucination, accuracy, verification]
---

# Ground Rules in Source Code

Every rule must be grounded in actual code you've seen, not assumed knowledge. This is the single most important rule for preventing hallucinated skills.

## Why

- **Accuracy**: Invented patterns produce wrong code when agents follow them
- **Trust**: One hallucinated rule undermines the entire skill
- **Specificity**: Observed patterns include project-specific context that generic advice misses
- **Verifiability**: Grounded rules can be checked against the codebase

## The Problem

AI agents writing skills have a tendency to:
- Invent API methods that don't exist
- Describe framework behaviors from training data, not from the actual codebase
- Copy "common best practices" without checking if this project follows them
- Write examples from memory instead of from real code

```markdown
# Bad: Invented from general knowledge
## Pattern

Use `useMutation` from React Query for server mutations:

\`\`\`tsx
const mutation = useMutation(postData, {
  onSuccess: () => queryClient.invalidateQueries('todos'),
});
\`\`\`

(The project might use `useFetcher` from React Router, not React Query.)
```

## The Rule: Copy, Don't Paraphrase

When writing a rule, find the actual code first. Copy patterns from real files, then add context.

```markdown
# Good: Grounded in actual codebase
## Pattern

Use `useFetcher` for mutations that don't navigate:

\`\`\`tsx
// src/components/toggle-bookmark.tsx (actual file)
const fetcher = useFetcher();

fetcher.submit(
  { bookmarkId: bookmark.id },
  { method: "post", action: "/api/bookmarks/toggle" }
);
\`\`\`
```

The file path proves this pattern exists in the project.

## Distinguish Observed vs Proposed

Be clear about whether you're documenting an existing pattern or proposing a new one.

```markdown
# Observed: This is how the codebase works today
## Pattern

The codebase uses `resource` routes for mutations, not custom controller actions.

\`\`\`ruby
# Found in config/routes.rb
resources :cards do
  resource :closure, only: [:create, :destroy]
end
\`\`\`

# Proposed: This is what I recommend
## Pattern

Consider using `resource` routes for single-record mutations instead
of custom controller actions. This keeps routing RESTful.

\`\`\`ruby
# Recommended pattern
resources :cards do
  resource :closure, only: [:create, :destroy]
end
\`\`\`
```

Observed patterns are facts. Proposed patterns are opinions. Label them accordingly.

## Verify Before Writing

Before writing a rule, check:

1. **Does this API exist in the project?** Search for imports and usage
2. **Does this pattern appear in the codebase?** Grep for the actual code
3. **Is this the project's convention?** Check if the codebase consistently follows it
4. **Would this example compile?** If you're writing TypeScript, make sure types match

```markdown
# Checklist before writing a rule
- [ ] Found at least one real usage in the codebase
- [ ] Copied the pattern from actual source files
- [ ] Verified the API/method exists (not from memory)
- [ ] Example would actually compile/run
- [ ] Not assuming a framework convention without checking
```

## When General Knowledge Is OK

Some rules genuinely come from the framework, not the codebase:

- Language syntax rules (`const` vs `let`)
- Framework conventions (React hooks rules)
- Well-documented APIs (Prisma's `@@map` directive)

Even here, verify the project uses the framework version you think it does.

## Rules

1. Find the code first, then write the rule — not the other way around
2. Copy patterns from real files; include file paths as proof
3. Distinguish "observed in codebase" from "proposed convention"
4. Verify APIs exist before documenting them
5. If you can't find it in the code, don't write it as a rule

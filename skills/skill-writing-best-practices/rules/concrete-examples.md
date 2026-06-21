---
title: Use Concrete Examples
impact: HIGH
tags: [content, examples, clarity]
---

# Use Concrete Examples

Every rule needs code examples. Abstract advice without examples is hard to apply.

## Why

- **Actionable**: Code shows exactly what to do
- **Unambiguous**: Examples eliminate interpretation guesswork
- **Memorable**: Concrete patterns stick better than abstract principles
- **Verifiable**: Readers can compare their code to the example

## Bad: Abstract Advice

```markdown
# Bad: Too vague
"Keep your code organized and maintainable."

"Use appropriate design patterns."

"Structure your files logically."
```

These don't help because they don't show what "organized" or "appropriate" means.

## Good: Concrete Patterns

Show directory structures, file paths, or exact code:

```markdown
"Place model-specific concerns in `app/models/model_name/`, not `app/models/concerns/`."

\`\`\`
app/models/
├── card.rb
├── card/
│   ├── closeable.rb     # Card::Closeable
│   └── searchable.rb    # Card::Searchable
└── concerns/            # Only shared concerns
    └── mentionable.rb
\`\`\`
```

The reader knows exactly where to put files.

## Show the Transformation

When showing a pattern, include before and after:

```typescript
// Bad: Inline conditional classes, hard to read at a glance
<button className={`btn ${isActive ? 'btn-active' : ''} ${isDisabled ? 'btn-disabled' : ''}`}>

// Good: Utility function, intent is clear
<button className={cn("btn", { "btn-active": isActive, "btn-disabled" isDisabled })}>
```

The contrast makes the improvement obvious.

## Use Real Code

Patterns from real codebases are more convincing than invented snippets:

```typescript
function Button({ className, variant, children }: ButtonProps) {
  return (
    <button
      className={cn(
        "inline-flex items-center rounded-lg font-medium",
        {
          "bg-teal-500 text-white": variant === "primary",
          "bg-neutral-100 text-neutral-900": variant === "secondary",
        },
        className
      )}
    >
      {children}
    </button>
  );
}
```

Real code shows that the pattern actually works in production.

## Match Example Complexity to Rule Complexity

Simple rules get simple examples:

```typescript
// Simple rule: use named exports
// Bad
export default function formatCurrency() {}

// Good
export function formatCurrency() {}
```

Complex rules may need longer examples with comments:

```typescript
function useDebounce<T>(value: T, delay: number): T {
  let [debouncedValue, setDebouncedValue] = useState(value);

  useEffect(() => {
    let timer = setTimeout(() => setDebouncedValue(value), delay);
    return () => clearTimeout(timer); // Clean up on change or unmount
  }, [value, delay]);

  return debouncedValue;
}
```

## Rules

1. Every rule must have at least one code example
2. Show bad/good contrast when applicable
3. Use real code from actual codebases when possible
4. Match example complexity to rule complexity
5. Abstract advice alone is not a rule

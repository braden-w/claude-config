# TypeScript Guidelines

## Core Rules

- Always use `type` instead of `interface` in TypeScript.

- When moving components to new locations, always update relative imports to absolute imports (e.g., change `import Component from '../Component.svelte'` to `import Component from '$lib/components/Component.svelte'`)

- When functions are only used in the return statement of a factory/creator function, use object method shorthand syntax instead of defining them separately:

```typescript
// Instead of:
function myFunction() {
  const helper = () => { /* ... */ };
  return { helper };
}

// Use:
function myFunction() {
  return {
    helper() { /* ... */ }
  };
}
```

## Type Inference

TypeScript 5.5+ automatically infers type predicates in `.filter()` callbacks. Don't add manual type assertions:

```typescript
// Good - TypeScript infers the narrowed type automatically
const filtered = items.filter((x) => x !== undefined);

// Bad - unnecessary type predicate
const filtered = items.filter((x): x is NonNullable<typeof x> => x !== undefined);
```

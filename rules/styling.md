# Styling Best Practices

## Minimize Wrapper Elements

When applying styles, avoid creating unnecessary wrapper divs. If classes can be applied directly to an existing semantic element with the same outcome, prefer that approach:

### Good (Direct Application)
```svelte
<main class="flex-1 mx-auto max-w-7xl">
  {@render children()}
</main>
```

### Avoid (Unnecessary Wrapper)
```svelte
<main class="flex-1">
  <div class="mx-auto max-w-7xl">
    {@render children()}
  </div>
</main>
```

This principle applies to all elements where the styling doesn't conflict with the element's semantic purpose or create layout issues.

## shadcn-svelte Best Practices

### Component Organization
- When using $state, $derived, or functions in Svelte component files that are only referenced once in the component markup, inline them directly in the markup for better code locality
- Use the CLI for adding/managing shadcn-svelte components: `bunx shadcn-svelte@latest add [component]`
- Each component should be in its own folder under `$lib/components/ui/` with an `index.ts` export file
- Follow kebab-case for component folder names (e.g., `dialog/`, `toggle-group/`)
- Group related sub-components in the same folder (e.g., all dialog parts in `dialog/`)

### Import Patterns

**Namespace imports** (preferred for multi-part components):
```typescript
import * as Dialog from '$lib/components/ui/dialog';
import * as ToggleGroup from '$lib/components/ui/toggle-group';
```

**Named imports** (for single components):
```typescript
import { Button } from '$lib/components/ui/button';
import { Input } from '$lib/components/ui/input';
```

### Styling and Customization
- Always use the `cn()` utility from `$lib/utils` for combining Tailwind classes
- Modify component code directly rather than overriding styles with complex CSS
- Use `tailwind-variants` for component variant systems
- Follow the `background`/`foreground` convention for colors
- Leverage CSS variables for theme consistency

### Component Usage Patterns

Use proper component composition following shadcn-svelte patterns:

```svelte
<Dialog.Root bind:open={isOpen}>
  <Dialog.Trigger>
    <Button>Open</Button>
  </Dialog.Trigger>
  <Dialog.Content>
    <Dialog.Header>
      <Dialog.Title>Title</Dialog.Title>
    </Dialog.Header>
  </Dialog.Content>
</Dialog.Root>
```

### Custom Components
- When extending shadcn components, create wrapper components that maintain the design system
- Add JSDoc comments for complex component props
- Ensure custom components follow the same organizational patterns
- Consider semantic appropriateness (e.g., use section headers instead of cards for page sections)

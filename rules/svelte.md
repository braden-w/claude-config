# Svelte Guidelines

## Mutation Pattern Preference

### In Svelte Files (.svelte)

Always prefer `createMutation` from TanStack Query for mutations. This provides:
- Loading states (`isPending`)
- Error states (`isError`)
- Success states (`isSuccess`)
- Better UX with automatic state management

### The Preferred Pattern

Pass `onSuccess` and `onError` as the second argument to `.mutate()` to get maximum context:

```svelte
<script lang="ts">
  import { createMutation } from '@tanstack/svelte-query';
  import * as rpc from '$lib/query';

  // Create mutation with just .options (no parentheses!)
  const deleteSessionMutation = createMutation(rpc.sessions.deleteSession.options);

  // Local state that we can access in callbacks
  let isDialogOpen = $state(false);
</script>

<Button
  onclick={() => {
    deleteSessionMutation.mutate({ sessionId }, {
      onSuccess: () => {
        isDialogOpen = false;
        toast.success('Session deleted');
        goto('/sessions');
      },
      onError: (error) => {
        toast.error(error.title, { description: error.description });
      },
    });
  }}
  disabled={deleteSessionMutation.isPending}
>
  {#if deleteSessionMutation.isPending}
    Deleting...
  {:else}
    Delete
  {/if}
</Button>
```

### Why This Pattern?
- **More context**: Access to local variables and state at the call site
- **Better organization**: Success/error handling is co-located with the action
- **Flexibility**: Different calls can have different success/error behaviors

### In TypeScript Files (.ts)

Always use `.execute()` since createMutation requires component context:

```typescript
const result = await rpc.sessions.createSession.execute({
  body: { title: 'New Session' }
});

const { data, error } = result;
if (error) {
  // Handle error
} else if (data) {
  // Handle success
}
```

### Exception: When to Use .execute() in Svelte Files

Only use `.execute()` in Svelte files when:
1. You don't need loading states
2. You're performing a one-off operation
3. You need fine-grained control over async flow

### Inline Simple Handler Functions

When a handler function only calls `.mutate()`, inline it directly:

```svelte
<!-- Good: Inline simple handlers -->
<Button onclick={() => shareMutation.mutate({ id })}>
  Share
</Button>

<!-- Avoid: Unnecessary wrapper function -->
<script>
  function handleShare() {
    shareMutation.mutate({ id });
  }
</script>
<Button onclick={handleShare}>
  Share
</Button>
```

## Self-Contained Component Pattern

### Prefer Component Composition Over Parent State Management

When building interactive components (especially with dialogs/modals), create self-contained components rather than managing state at the parent level.

### The Anti-Pattern (Parent State Management)

```svelte
<!-- Parent component -->
<script>
  let deletingItem = $state(null);

  function handleDelete(item) {
    deletingItem = null;
  }
</script>

{#each items as item}
  <Button onclick={() => deletingItem = item}>Delete</Button>
{/each}

<AlertDialog open={!!deletingItem}>
  <!-- Single dialog for all items -->
</AlertDialog>
```

### The Pattern (Self-Contained Components)

```svelte
<!-- DeleteItemButton.svelte -->
<script>
  let { item } = $props();
  let open = $state(false);

  function handleDelete() {
    // delete logic directly in component
  }
</script>

<AlertDialog.Root bind:open>
  <AlertDialog.Trigger>
    <Button>Delete</Button>
  </AlertDialog.Trigger>
  <AlertDialog.Content>
    <!-- Dialog content -->
  </AlertDialog.Content>
</AlertDialog.Root>

<!-- Parent component -->
{#each items as item}
  <DeleteItemButton {item} />
{/each}
```

### Why This Pattern Works
- **No parent state pollution**: Parent doesn't need to track which item is being deleted
- **Better encapsulation**: All delete logic lives in one place
- **Simpler mental model**: Each row has its own delete button with its own dialog
- **No callbacks needed**: Component handles everything internally
- **Scales better**: Adding new actions doesn't complicate the parent

### When to Apply This Pattern
- Action buttons in table rows (delete, edit, etc.)
- Confirmation dialogs for list items
- Any repeating UI element that needs modal interactions
- When you find yourself passing callbacks just to update parent state

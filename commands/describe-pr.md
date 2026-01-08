---
description: Generate comprehensive PR descriptions with code examples and diagrams
---

# Generate PR Description

Generate a PR description that explains WHY the change exists, shows HOW to use any new APIs, and includes diagrams for architecture changes.

## Steps

### 1. Identify the PR

```bash
gh pr view --json url,number,title,state 2>/dev/null
# If no PR, list open PRs:
gh pr list --limit 10 --json number,title,headRefName,author
```

### 2. Gather Information

```bash
gh pr diff {number}
gh pr view {number} --json commits,baseRefName,url,title,number,state
```

### 3. Analyze Changes

- Read the entire diff
- Identify the PROBLEM being solved (the "why")
- Identify new APIs, functions, types, CLI commands, or endpoints
- Identify architecture changes (data flow, component interactions)

### 4. Generate Description

**Structure (in order):**

1. **Opening paragraph**: WHY this change exists. What problem does it solve? Link to previous PRs if this restores/continues prior work.

2. **Diagram** (REQUIRED if architecture changed): ASCII flow diagram showing how components interact.

   ```
   ┌─────────────┐     ┌─────────────┐
   │  Component  │ ──> │  Component  │
   └─────────────┘     └─────────────┘
   ```

3. **Code examples** (REQUIRED for API changes): Show actual usage, not descriptions.

   ```typescript
   // Show the actual API call site
   const result = newFunction({ param: "value" });
   ```

4. **Implementation notes**: Technical decisions, why certain approaches were chosen.

5. **Verification section**: What was tested (automated and manual).

### 5. Update the PR

```bash
gh pr edit {number} --body "$(cat <<'EOF'
[description here]
EOF
)"
```

## Mandatory Requirements

### Code Examples Required For:

- New functions, types, or exports
- Changes to function signatures
- New CLI commands or flags
- New HTTP endpoints
- Configuration changes

**Good** (shows usage):

```typescript
const actions = {
  posts: {
    create: defineMutation({
      input: type({ title: "string" }),
      handler: ({ title }) => db.insert({ title }),
    }),
  },
};

const cli = createCLI(client, { actions });
```

**Bad** (only describes):

> This PR adds an action system that generates CLI commands from action definitions.

### Diagrams Required For:

- Changes to data flow
- New component interactions
- Architecture refactors

### The "Why" Must Come First

Every PR description MUST start with the problem being solved. Not what files changed, not how it works—WHY.

**Good opening**:

> This PR restores the action system removed in #1209, but with a clearer understanding of where the boundary belongs.

**Bad opening**:

> This PR adds defineQuery and defineMutation functions.

## What to Avoid

- Listing files changed (GitHub shows this)
- Section headers like "## Summary" or "## Changes Made"
- Bullet point lists for the main content
- Marketing language ("revolutionary", "seamless")
- Walls of text without code examples
- AI watermarks or attribution

## Voice

- Conversational but precise
- Direct and honest
- Show your thinking: "We considered X, but Y made more sense because..."

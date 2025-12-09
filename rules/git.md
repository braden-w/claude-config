# Git Commit and Pull Request Guidelines

## Conventional Commits Format

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

### Commit Types
- `feat`: New features (MINOR version bump)
- `fix`: Bug fixes (PATCH version bump)
- `docs`: Documentation only changes
- `refactor`: Code changes that neither fix bugs nor add features
- `perf`: Performance improvements
- `test`: Adding or modifying tests
- `chore`: Maintenance tasks, dependency updates
- `style`: Code style changes (formatting, etc.)
- `build`: Changes to build system or dependencies
- `ci`: Changes to CI configuration

### Description Rules
- Start with lowercase immediately after the colon and space
- Use imperative mood ("add" not "added" or "adds")
- No period at the end
- Keep under 50-72 characters on first line

### Breaking Changes
- Add `!` after type/scope: `feat(api)!: change endpoint structure`
- Include `BREAKING CHANGE:` in footer with details

## Commit Messages Best Practices

- NEVER include Claude Code or opencode watermarks or attribution
- Each commit should represent a single, atomic change
- Write commits for future developers (including yourself)
- If you need more than one line to describe what you did, consider splitting the commit

## What NOT to Include

- `Generated with [Claude Code](https://claude.ai/code)`
- `Co-Authored-By: Claude <noreply@anthropic.com>`
- Any references to AI assistance
- Tool attribution or watermarks

## Pull Request Guidelines

- PR title should follow same conventional commit format
- Focus on the "why" and "what" of changes
- Include any breaking changes prominently
- Link to relevant issues
- Use paragraph format, not excessive bullet lists

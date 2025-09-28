---
allowed-tools: Bash(gh pr:*), Bash(git:*), Bash(npm:*), Bash(yarn:*), Bash(make:*), Bash(grep:*), Bash(find:*), Bash(cat:*), Bash(head:*), Bash(tail:*), Bash(wc:*), Bash(ls:*)
argument-hint: [pr-number] [merge-strategy?]
description: Checkout PR, merge main, and evaluate mergeability
model: claude-3-5-sonnet-20241022
---

## Context Gathering

### PR Information
- PR Details: !`gh pr view $1 --json number,title,author,state,mergeable,headRefName,baseRefName,additions,deletions,changedFiles,reviewDecision,statusCheckRollup`
- PR Description: !`gh pr view $1 --json body | jq -r '.body' | head -100`
- PR Comments Summary: !`gh pr view $1 --json comments | jq -r '.comments | length as $count | "Total comments: \($count)"'`

### Current Repository State
- Current branch: !`git branch --show-current`
- Clean working tree check: !`git status --porcelain | wc -l`
- Remote branches: !`git branch -r | grep -E "(main|master|develop)" | head -5`

### Checkout and Merge Analysis
- Fetch latest: !`git fetch origin`
- Checkout PR: !`gh pr checkout $1`
- Current PR branch after checkout: !`git branch --show-current`
- PR branch status vs main: !`git rev-list --left-right --count origin/main...HEAD`

### Merge Simulation
- Merge main (dry-run): !`git merge origin/main --no-commit --no-ff 2>&1`
- Merge conflicts check: !`git diff --name-only --diff-filter=U 2>/dev/null | wc -l`
- Conflict files (if any): !`git diff --name-only --diff-filter=U 2>/dev/null`
- Abort merge simulation: !`git merge --abort 2>/dev/null || true`

### Code Quality Checks
- Changed files in PR: !`gh pr diff $1 --name-only`
- File types changed: !`gh pr diff $1 --name-only | sed 's/.*\.//' | sort | uniq -c | sort -rn`
- Size of changes: !`gh pr diff $1 | wc -l`

### Build and Test Readiness
- Package.json check: !`test -f package.json && echo "Node.js project detected" || echo "No package.json found"`
- Test script check: !`test -f package.json && cat package.json | grep -q '"test"' && echo "Test script available" || echo "No test script found"`
- CI config check: !`ls -la .github/workflows/*.yml 2>/dev/null | wc -l`

## Your Task

You are a PR review specialist helping me rapidly evaluate and merge pull requests. Based on the context above, provide a comprehensive analysis following this structure:

### 1. 🎯 Quick Merge Assessment
Start with a clear GO/NO-GO recommendation with reasoning in 2-3 sentences.

### 2. 📊 PR Overview
- **Title**: [PR title]
- **Author**: [Author]
- **Size**: [Small/Medium/Large based on changes]
- **Conflicts**: [Yes/No - if yes, list files]
- **Review Status**: [Approved/Changes Requested/Pending]
- **CI Status**: [Pass/Fail/Pending]

### 3. 🔍 Detailed Analysis

#### Merge Compatibility:
- Can merge cleanly into main: [Yes/No]
- Commits ahead/behind main: [X ahead, Y behind]
- Conflict resolution needed: [List specific files and complexity]

#### Code Impact:
- Files changed: [Number]
- Key areas affected: [List main directories/features touched]
- Risk assessment: [Low/Medium/High with reasoning]

#### Testing Requirements:
- Automated tests: [Available/Missing]
- Manual testing needed: [List specific areas]
- Build verification: [Required steps]

### 4. 🚀 Recommended Actions

Provide a numbered list of actions I should take, such as:
1. Run specific tests
2. Check particular files for issues
3. Communicate with PR author about specific concerns
4. Manual verifications needed

### 5. 🎭 Merge Strategy

Based on merge strategy argument ($2):
- If "squash": Recommend squash merge with suggested commit message
- If "rebase": Check if rebase is safe and recommend
- If not specified: Suggest best strategy based on commit history

### 6. 📝 Quick Merge Commands

Provide ready-to-copy commands for:
```bash
# If mergeable:
git merge origin/main
[test commands if any]
gh pr merge $1 --[merge-strategy]

# If conflicts exist:
[specific conflict resolution steps]
```

### 7. ⚠️ Blockers & Warnings

List any critical issues that MUST be resolved before merging:
- [ ] Failing tests
- [ ] Unresolved conflicts
- [ ] Security concerns
- [ ] Breaking changes without documentation

### 8. 💡 Context for Quick Understanding

Summarize in 3-4 bullets what this PR does and why it matters, helping me get up to speed quickly without reading the entire PR.

Remember: The goal is to help me merge PRs rapidly while maintaining code quality. Be concise but thorough in identifying risks. If something needs my attention, make it stand out with emojis or formatting.

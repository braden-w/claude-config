---
allowed-tools: Bash(gh pr:*), Bash(git:*), Bash(jq:*), Bash(grep:*), Bash(sort:*), Bash(wc:*), Bash(head:*), Bash(cat:*)
argument-hint: [state?] [limit?]
description: Evaluate and rank multiple PRs by mergeability and readiness
model: sonnet
---

## Context Gathering

### Fetch All PRs
- Get open PRs with key metrics: !`gh pr list --state $1 --limit $2 --json number,title,author,state,mergeable,reviewDecision,statusCheckRollup,commits,additions,deletions,changedFiles --template '{{range .}}{{.number}},{{.title}},{{.author.login}},{{.mergeable}},{{.reviewDecision}},{{.statusCheckRollup}},{{.commits}},{{.additions}},{{.deletions}},{{.changedFiles}}{{"\n"}}{{end}}'`

### Repository Context
- Current branch: !`git branch --show-current`
- Main branch: !`git rev-parse --abbrev-ref origin/HEAD | cut -d/ -f2`
- Latest main commit: !`git log -1 --format=%h origin/main`

## Your Task

You are a PR triage specialist helping me rapidly assess multiple pull requests and determine which are ready to merge. Rank and sort all PRs by mergeability and readiness.

### 1. 🎯 Mergeability Ranking

Create a ranked list (out of 10) with this scoring system:

**Scoring Criteria (each 0-2 points):**
- **Mergeable Status**: 2 if clean merge, 1 if has conflicts, 0 if blocked
- **Review Status**: 2 if approved, 1 if pending/changes requested, 0 if not reviewed
- **CI Status**: 2 if all passing, 1 if some failures, 0 if critical failures
- **Change Size**: 2 if small (< 100 lines), 1 if medium (100-500), 0 if large (> 500)
- **Test Coverage**: 2 if has tests, 1 if partial, 0 if missing

**Display format:**
```
[#XXX - 9/10] Title (Author) - Ready to merge
[#XXX - 7/10] Title (Author) - Needs review approval
[#XXX - 4/10] Title (Author) - Has merge conflicts
```

### 2. 📊 Summary Table

Create a quick reference table showing all PRs:

| PR # | Title | Author | Score | Status | Action |
|------|-------|--------|-------|--------|--------|
| #XXX | ... | @user | 9/10 | ✅ Ready | Merge now |
| #XXX | ... | @user | 7/10 | ⏳ Pending | Request review |
| #XXX | ... | @user | 4/10 | ⚠️ Blocked | Resolve conflicts |

### 3. 🚀 Merge Candidates

List PRs that are ready to merge (8-10 score) with suggested commands:
- Include merge strategy recommendation (squash/rebase/merge)
- Provide ready-to-copy merge commands

### 4. ⏳ In Progress

List PRs that need attention (4-7 score) with specific blockers:
- What's blocking them
- What action is needed
- Who needs to do it

### 5. 🚫 Blocked PRs

List PRs that cannot merge (0-3 score) with reasons:
- Merge conflicts (specific files)
- Failed CI checks
- Awaiting reviews
- Other blockers

### 6. 💡 Quick Summary

Provide 3-5 bullet points summarizing the PR queue:
- How many are ready to merge
- What's the main blocker across PRs
- Recommended next steps
- Any patterns or trends you notice

---

**Remember:** The goal is to help you rapidly triage your PR queue and identify which ones are safe to merge immediately vs. which need work. Use the score out of 10 to guide your prioritization.

---
allowed-tools: Bash(git add:*), Bash(git status:*)
description: Stage modified tracked files for commit
model: claude-haiku-4-5
---

# Stage Session Files

Stage only the files that were edited by Claude Code in the current session.

## Instructions
1. Identify all files that were modified using Edit or Write tools in this session
2. Stage each file individually using `git add <file>`
3. Show the final staged status with `git status`

IMPORTANT: Do NOT use `git add -u` or `git add .` as these stage all modified files in the repository, including changes made outside this session. Only stage the specific files that Claude Code edited during this conversation.

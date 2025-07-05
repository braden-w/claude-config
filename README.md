# My Claude Code Configuration

This repository contains my personal global configuration files for [Claude Code](https://github.com/anthropics/claude-code), Anthropic's CLI for Claude.

## Files

### `CLAUDE.md`
My personal coding preferences and instructions that Claude follows across all projects. This includes:
- TypeScript conventions (prefer `type` over `interface`)
- Import patterns and path preferences
- My standard development workflow
- Code style guidelines
- Git commit conventions
- Framework-specific practices (Svelte, shadcn-svelte)

### `settings.json`
Security and permission settings for Claude Code, featuring:
- Model selection (opus)
- Extensive read-only bash command permissions for context gathering
- Git CLI permissions for repository exploration
- Package manager inspection commands
- Tool permissions (WebFetch, WebSearch, TodoRead)

## Philosophy

These settings prioritize:
- **Read-only operations**: Claude can explore and understand codebases without making unwanted changes
- **Transparency**: Clear conventions and workflows that both Claude and I follow
- **Efficiency**: Pre-authorized commands reduce permission prompts during development

## Usage

To use these settings:
1. Place both files in your `~/.claude/` directory
2. Claude Code will automatically load these configurations

## Note

These are my personal preferences optimized for my workflow. Feel free to fork and adapt them to your needs.
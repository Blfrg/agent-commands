# `/commit` - Intelligent Atomic Git Commit Automation

## Summary

A slash command for [Claude Code](https://claude.ai/code) or
[Cursor](https://www.cursor.com/) that automatically analyzes your changes and
creates multiple atomic, well-organized git commits following
[Conventional Commits](https://www.conventionalcommits.org/) standards with
TDD-first ordering and priority-based sequencing.

## Core Philosophy

This command follows an opinionated approach to git history:

- **Granular by default** - When in doubt, split into separate commits
- **TDD-first workflow** - Tests commit BEFORE implementation (red → green)
- **Priority-ordered** - Critical fixes first, documentation last
- **Autonomous** - Makes principled decisions without asking

## What It Does

Instead of dumping all changes into one large commit, this command:

- Analyzes all changes and identifies logical commit boundaries
- Orders commits by priority (security → breaking → features → fixes → docs)
- Creates TDD commit pairs (test first, then implementation)
- Uses partial staging (`git add -p`) to split changes within the same file
- Generates [Conventional Commits](https://www.conventionalcommits.org/) messages
- Handles issue references precisely (closing keywords only on implementation)

## Key Benefits

- ✅ **TDD Commit Pairs** - Tests commit before their implementation
- ✅ **Priority Ordering** - Critical fixes committed before features
- ✅ **Micro-intent Commits** - Each commit has a single, clear purpose
- ✅ **Cherry-pickable** - Any commit can be applied independently
- ✅ **Bisectable** - Easy to find which commit introduced issues
- ✅ **[Conventional Commits](https://www.conventionalcommits.org/)** - Industry
  standard format (feat, fix, docs, etc.)
- ✅ **Bash Safety** - Handles special characters correctly (no `!` expansion)
- ✅ **Issue Precision** - Only implementation commits use closing keywords

## Priority Ordering

Commits are created in this order:

1. **Critical** - Security fixes, data corruption, crashes
2. **Breaking** - API changes, schema migrations
3. **Features** - New capabilities, enhancements
4. **Bug fixes** - Non-critical functional issues
5. **Improvements** - Refactoring, performance
6. **Supporting** - Documentation, chores, CI

## Example Output

```bash
Created 6 atomic commits:

# Priority 1: Critical security fix
- test(auth): add token validation security tests
- fix(auth): patch token signature bypass vulnerability

# Priority 3: Feature (with TDD pairing)
- test(api): add user search endpoint tests
- feat(api): add user search endpoint

# Priority 6: Documentation
- docs(api): add user search endpoint documentation
```

## Issue References

Use `/commit fixes #123` to link commits to issues:

- **Implementation commit**: Gets `Fixes #123` (closes the issue)
- **All other commits**: Get `Related to #123` (links without closing)

## When to Use

- After a coding session with multiple unrelated changes
- Before pushing to a shared branch
- When you need clean, reviewable, bisectable commits
- To maintain professional git history with TDD workflow visible

## Installation

For detailed installation instructions, see:

- **[Claude Code Installation Guide](../../docs/install/claude-code.md)**
- **[Cursor Installation Guide](../../docs/install/cursor.md)**

### Quick Install

```bash
# Clone the repository
git clone https://github.com/Blfrg/agent-commands.git
cd agent-commands

# For Claude Code
cp commands/commit/commit.md ~/.claude/commands/

# For Cursor
cp commands/commit/commit.md ~/.cursor/commands/
```

## How to Use

Simply type `/commit` in [Claude Code](https://claude.ai/code) or
[Cursor](https://www.cursor.com/) when you have unstaged changes. The AI will
handle the rest, asking for input only when necessary.

**Note**: Respects project-specific conventions from `CONTRIBUTING.md` or
`.gitmessage` files when available.

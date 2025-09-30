# `/commit` - Intelligent Atomic Git Commit Automation

## Summary

A slash command for Claude Code or Cursor that automatically analyzes your
unstaged changes and creates multiple atomic, well-organized git commits
following conventional commit standards.

## What It Does

Instead of dumping all changes into one large commit, this command:

- Analyzes all modified files and intelligently groups related changes
- Creates separate commits for features, fixes, refactoring, tests, and docs
- Generates properly formatted conventional commit messages
- Handles breaking changes safely with proper notation
- Stages changes incrementally using `git add -p` when needed

## Key Benefits

- ✅ **Clean Git History** - Each commit does one thing well
- ✅ **Conventional Commits** - Follows industry standards (feat, fix, docs,
  etc.)
- ✅ **Automatic Grouping** - AI intelligently groups related changes
- ✅ **Breaking Change Support** - Properly marks and documents breaking changes
- ✅ **Bash Safety** - Handles special characters correctly (no `!` expansion
  errors)
- ✅ **Interactive When Needed** - Asks for clarification on ambiguous changes

## Example Output

```bash
Created 4 atomic commits:
- feat(auth): add OAuth2 provider support
- fix(api): handle null responses in user endpoint
- test(auth): add OAuth integration tests
- docs: update API authentication guide
```

## When to Use

- After a coding session with multiple unrelated changes
- Before pushing to a shared branch
- When you need clean, reviewable commits
- To maintain professional git history standards

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

Simply type `/commit` in Claude Code or Cursor when you have unstaged changes.
The AI will handle the rest, asking for input only when necessary.

**Note**: Respects project-specific conventions from `CONTRIBUTING.md` or
`.gitmessage` files when available.

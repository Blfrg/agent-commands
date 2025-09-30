# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with
code in this repository.

## Project Overview

This is a collection of **slash commands** (markdown-based LLM instructions)
for AI development environments like Claude Code and Cursor. Each command in
the `commands/` directory is a standalone, optional markdown file that users
can install to enhance their workflow.

## Architecture

### Directory Structure

```bash
commands/
  └── [command-name]/
      ├── [command-name].md    # The LLM instruction file
      └── README.md             # User-facing documentation
docs/
  └── install/                  # Installation guides
      ├── claude-code.md
      └── cursor.md
```

### Command Philosophy

Each command is:

- **Self-contained**: No dependencies between commands
- **Optional**: Users install only what they need
- **Instruction-based**: Markdown files contain clear, actionable steps for LLMs

## Development Guidelines

### Creating New Commands

1. **Structure**: Create `commands/[name]/` with both `[name].md` and `README.md`
2. **Instruction clarity**: Write for LLM consumption in the `.md` file
3. **Documentation**: Write for human consumption in the `README.md`
4. **Arguments**: Use `$ARGUMENTS` for user input when needed
5. **Error handling**: Include fallback steps and error scenarios

### Markdown Linting

The project uses markdownlint with configuration in `.markdownlint.json`:

- Line length: 80 characters (120 for code blocks)
- Heading style: ATX (`#` prefix)
- List indent: 2 spaces
- List style: dashes (`-`) for unordered lists

**Lint command**:

```bash
npx markdownlint-cli2 commands/**/*.md docs/**/*.md *.md
```

### Commit Conventions

Follow conventional commits format based on the `/commit` command implementation:

- Types: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`,
  `ci`, `chore`
- Scope examples: `api`, `auth`, `ui`, `commands`, `docs`
- Breaking changes: Use `!` before colon (e.g., `feat(api)!: change endpoint`)

**Important**: Always use single quotes for commit messages with `!` to prevent
bash history expansion.

### Testing Commands

Before submitting changes:

1. Install locally: `cp commands/[name]/[name].md ~/.claude/commands/`
2. Test in Claude Code with actual use cases
3. Verify markdown syntax
4. Check that README explains usage clearly

## Installation Paths

Commands can be installed:

- **Globally**: `~/.claude/commands/` (all projects)
- **Locally**: `.claude/commands/` (project-specific)

Same applies for Cursor with `~/.cursor/commands/`

## Command Examples

See `commands/commit/commit.md` for reference implementation demonstrating:

- Structured analysis steps
- Priority-based decision making
- Error handling
- Safe bash command construction
- Project convention detection

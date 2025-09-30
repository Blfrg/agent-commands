# Contributing to Agent Commands

Welcome! This is a collection of optional markdown scripts for AI development
assistants like [Claude Code](https://claude.ai/code) and
[Cursor](https://www.cursor.com/). Each command is standalone - users install
only what they need.

## Adding a New Command

### 1. Create Your Command Folder

```bash
mkdir commands/your-command-name
```

### 2. Add the Command File

Create `your-command-name.md` with your LLM instructions:

```markdown
# Task

[Clear, actionable instructions for the AI]

## Steps

1. [Specific steps the AI should follow]
2. [Include error handling where appropriate]

## Notes

- Use $ARGUMENTS for user input if needed
- Keep instructions clear and unambiguous
```

### 3. Add a README

Create a `README.md` explaining:

- What the command does
- When to use it
- Example usage
- Expected output

## Command Philosophy

- **Single Purpose** - Each command does one thing well
- **Optional** - Users choose which commands to install
- **Self-Contained** - No dependencies between commands
- **LLM-Friendly** - Clear instructions an AI can follow

## Testing

Before submitting:

1. Test your command:

   ```bash
   cp commands/your-command/your-command.md ~/.claude/commands/
   # Try it in Claude Code: https://claude.ai/code
   ```

2. (Optional) Check markdown formatting:

   ```bash
   # This repo includes .markdownlint.json for consistency
   # Uses markdownlint: https://github.com/DavidAnson/markdownlint
   npx markdownlint-cli2 commands/your-command/*.md
   ```

## Pull Requests

1. Fork and create a feature branch
2. Add your command in `commands/your-command/`
3. Include both the `.md` file and README
4. Submit PR with description of what it does

## Examples of Good Commands

- `/commit` - Analyzes changes and creates atomic git commits
- `/review` - Performs code review with specific criteria
- `/test` - Generates appropriate test cases

## Not Suitable

- Commands requiring external APIs or services
- Commands that modify system configuration
- Commands with complex dependencies

Questions? Open an issue for discussion!

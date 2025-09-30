# Installing Agent Commands in Claude Code

## Quick Start

```bash
# Clone this repository
git clone https://github.com/Blfrg/agent-commands.git
cd agent-commands

# Install globally (available in all projects)
cp commands/commit/commit.md ~/.claude/commands/

# OR install in a specific project
cp commands/commit/commit.md /path/to/project/.claude/commands/
```

Type `/commit` in Claude Code to use the command.

## Installation Locations

### Global (User) Commands

- **Location**: `~/.claude/commands/`
- **Scope**: Available in all projects
- **Usage**: Type `/commandname`

### Project (Local) Commands

- **Location**: `.claude/commands/` (in project root)
- **Scope**: Only in that project
- **Usage**: Type `/commandname`

## Step-by-Step Installation

### 1. Create the directory

```bash
# For global commands
mkdir -p ~/.claude/commands

# For project commands
mkdir -p .claude/commands
```

### 2. Copy command files

```bash
# Single command
cp commands/commit/commit.md ~/.claude/commands/

# All commands
cp commands/*/[!README]*.md ~/.claude/commands/
```

### 3. Use the commands

- Type `/` to see available commands
- Select from the dropdown or type the full command
- May require restarting Claude Code session

## Managing Commands

### Update

```bash
cd agent-commands
git pull
cp commands/commit/commit.md ~/.claude/commands/
```

### Remove

```bash
rm ~/.claude/commands/commit.md
```

### List

```bash
ls -la ~/.claude/commands/
```

## Troubleshooting

**Command not appearing?**

- Check file exists: `ls ~/.claude/commands/commit.md`
- Restart Claude Code session
- Try `claude --debug` to see loaded commands

**Command not working?**

- Check for markdown syntax errors
- Review error messages from Claude Code

## Learn More

- [Official Claude Code Slash Commands Documentation](https://docs.claude.com/en/docs/claude-code/slash-commands)
- See individual command README files for usage details

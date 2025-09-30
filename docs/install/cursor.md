# Installing Agent Commands in Cursor

## Quick Start

```bash
# Clone this repository
git clone https://github.com/Blfrg/agent-commands.git
cd agent-commands

# Install globally (available in all projects)
cp commands/commit/commit.md ~/.cursor/commands/

# OR install in a specific project
cp commands/commit/commit.md /path/to/project/.cursor/commands/
```

Type `/commit` in Cursor to use the command.

## Installation Locations

### Global (User) Commands

- **Location**: `~/.cursor/commands/`
- **Scope**: Available in all projects
- **Usage**: Type `/commandname`

### Project (Local) Commands

- **Location**: `.cursor/commands/` (in project root)
- **Scope**: Only in that project
- **Usage**: Type `/commandname`

## Step-by-Step Installation

### 1. Create the directory

```bash
# For global commands
mkdir -p ~/.cursor/commands

# For project commands
mkdir -p .cursor/commands
```

### 2. Copy command files

```bash
# Single command
cp commands/commit/commit.md ~/.cursor/commands/

# All commands
cp commands/*/[!README]*.md ~/.cursor/commands/
```

### 3. Use the commands

- Type `/` in the Agent input to see available commands
- Select from the dropdown or type the full command
- Commands are available immediately (no restart needed)

## Managing Commands

### Update

```bash
cd agent-commands
git pull
cp commands/commit/commit.md ~/.cursor/commands/
```

### Remove

```bash
rm ~/.cursor/commands/commit.md
```

### List

```bash
ls -la ~/.cursor/commands/
```

## Platform Notes

### Windows

Use `%USER_PROFILE%\.cursor\commands\` instead of `~/.cursor/commands/`

### macOS/Linux

Commands work as shown above

## Troubleshooting

**Command not appearing?**

- Check file exists: `ls ~/.cursor/commands/commit.md`
- Press Cmd/Ctrl + R to refresh Cursor

**Command not working?**

- Check for markdown syntax errors
- Ensure `.md` file extension

## Learn More

- [Official Cursor Commands Documentation](https://docs.cursor.com/en/agent/chat/commands)
- See individual command README files for usage details

# Agent Commands Collection

A curated collection of intelligent slash commands for AI-powered development
environments like Claude Code and Cursor. Each folder in the `commands`
directory represents a specialized agent command that enhances your coding
workflow with smart automation.

## 📁 Available Commands

### `/commit` - Intelligent Atomic Git Commit Automation

Transform messy, uncommitted changes into clean, well-organized atomic commits
following conventional commit standards. This command analyzes your codebase
changes and intelligently groups related modifications into separate, meaningful
commits.

**Key Features:**

- Automatic change analysis and grouping
- Conventional commit message generation
- Breaking change detection and proper notation
- Interactive mode for ambiguous changes
- Respects project-specific conventions

[Learn more →](./commands/commit/README.md)

## 🚀 Installation

Choose your AI development environment:

- **[🤖 Claude Code Installation Guide](./docs/install/claude-code.md)**
- **[📝 Cursor Installation Guide](./docs/install/cursor.md)**

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

For detailed installation instructions, troubleshooting, and platform-specific
notes, see the installation guides linked above.

## 📋 Repository Structure

```
agent-commands/
├── commands/           # All agent commands
│   └── commit/        # Example: commit command
│       ├── README.md  # Command documentation
│       └── commit.md  # Command implementation
├── docs/              # Documentation
│   └── install/       # Installation guides
│       ├── claude-code.md
│       └── cursor.md
├── LICENSE            # MIT License
└── README.md          # This file
```

Each command folder contains:

- `README.md` - Detailed documentation and usage examples
- `[command-name].md` - The actual command implementation
- Additional support files as needed

## 🎯 Philosophy

These agent commands are designed to:

- **Enhance productivity** by automating repetitive tasks
- **Maintain code quality** through intelligent analysis
- **Follow best practices** automatically
- **Provide transparency** by explaining actions taken
- **Stay flexible** with interactive modes when needed

## 🤝 Contributing

Want to add a new agent command? See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed
guidelines.

For contributors using Claude Code, see [CLAUDE.md](CLAUDE.md) for architecture
details and development workflow.

Each command should:

1. Solve a specific workflow problem
2. Include comprehensive documentation
3. Follow the established folder structure
4. Provide clear installation instructions
5. Include examples of usage and output

## 📝 License

This repository is licensed under the MIT License - see the [LICENSE](LICENSE)
file for details.

## 🛠️ Requirements

- Claude Code or Cursor AI development environment
- Git (for version control commands)
- Project-specific tools as required by individual commands

---

_This repository is actively maintained. Star ⭐ to stay updated with new
commands!_

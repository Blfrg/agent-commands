# Agent Commands Collection

A curated collection of intelligent slash commands for AI-powered development
environments like [Claude Code](https://claude.ai/code) and
[Cursor](https://www.cursor.com/). Each folder in the `commands` directory
represents a specialized agent command that enhances your coding workflow with
smart automation.

## What are slash commands?

Slash commands are markdown-based instructions that extend AI coding assistants
with custom workflows. Learn more:

- [Claude Code Slash Commands](https://docs.claude.com/en/docs/claude-code/slash-commands)
- [Cursor Commands](https://docs.cursor.com/en/agent/chat/commands)

## 📁 Available Commands

### `/commit` - Intelligent Atomic Git Commit Automation

Transform messy, uncommitted changes into clean, well-organized atomic commits
following [Conventional Commits](https://www.conventionalcommits.org/) standards.
This command analyzes your changes and creates granular, priority-ordered commits
with TDD-first sequencing.

**Key Features:**

- TDD commit pairs (tests before implementation)
- Priority-based ordering (critical fixes → features → docs)
- Micro-intent commits (each commit has one clear purpose)
- Partial staging for within-file boundaries (`git add -p`)
- Precise issue references (closing keywords only on implementation)
- [Conventional Commits](https://www.conventionalcommits.org/) message generation
- Breaking change detection and proper notation
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

```text
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

This repository is licensed under the [MIT License](LICENSE) - see
[opensource.org](https://opensource.org/licenses/MIT) for details.

## 🛠️ Requirements

- [Claude Code](https://claude.ai/code) or [Cursor](https://www.cursor.com/) AI
  development environment
- [Git](https://git-scm.com/) (for version control commands)
- Project-specific tools as required by individual commands

---

_This repository is actively maintained. Star ⭐ to stay updated with new
commands!_

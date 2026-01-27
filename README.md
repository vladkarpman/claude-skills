# Claude Skills

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A collection of reusable skills for Claude that enhance its capabilities for specific tasks.

## Available Skills

| Skill | Description |
|-------|-------------|
| [prompt-improver](./skills/prompt-improver/) | Build or improve prompts using the 6-component framework (Persona, Task, Steps, Context, Goal, Format) |

## Commands

| Command | Description |
|---------|-------------|
| `/improve-prompt` | Analyze and improve an existing prompt |
| `/build-prompt` | Build a new prompt from scratch |

## Installation

### Claude Code (CLI)

**Option 1: Via Marketplace (recommended)**
```bash
# Add the marketplace (one time)
claude plugin marketplace add vladkarpman/claude-plugins

# Install the plugin
claude plugin install claude-skills
```

**Option 2: Direct install**
```bash
claude plugin add vladkarpman/claude-skills
```

After installation, use the commands:
```bash
/improve-prompt write code to process data
/build-prompt
```

### Claude Desktop / Claude Cowork

1. Navigate to [skills/prompt-improver/SKILL.md](./skills/prompt-improver/SKILL.md)
2. Copy the file contents (everything including the frontmatter)
3. Open Claude Desktop → Settings → Custom Instructions
4. Paste the content

### Claude.ai (Web)

1. Navigate to [skills/prompt-improver/SKILL.md](./skills/prompt-improver/SKILL.md)
2. Copy the file contents
3. Create or open a Project
4. Go to Project Instructions
5. Paste the content

## License

MIT License — see [LICENSE](./LICENSE) for details.

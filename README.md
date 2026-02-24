# Claude Skills

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A collection of reusable skills for Claude that enhance its capabilities for specific tasks.

## Available Skills

| Skill | Description |
|-------|-------------|
| [prompt-improver](./skills/prompt-improver/) | Build or improve prompts using the 6-component framework (Persona, Task, Steps, Context, Goal, Format) |
| [think](./skills/think/) | Universal task entry point — orchestrates superpowers skills and adds OpenAI Codex CLI as an adversarial second opinion |

## Commands

| Command | Description |
|---------|-------------|
| `/improve-prompt` | Analyze and improve an existing prompt |
| `/build-prompt` | Build a new prompt from scratch |
| `/think` | Start any task with superpowers orchestration + Codex adversarial review |

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
/think add user authentication to the REST API
```

### Claude Desktop / Claude Cowork

1. Download [SKILL.md](./skills/prompt-improver/SKILL.md)
2. Open Claude Desktop → Settings → **Capabilities**
3. Scroll down to **Skills** section → click **+ Add**
4. Select **"Upload a skill"**
5. Choose the downloaded SKILL.md file

## License

MIT License — see [LICENSE](./LICENSE) for details.

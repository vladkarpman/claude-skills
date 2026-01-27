# Claude Skills

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A collection of reusable skills for Claude that enhance its capabilities for specific tasks.

## What Are Claude Skills?

Skills are prompt templates that give Claude specialized abilities. Instead of writing complex prompts from scratch, you can use a skill to get consistent, high-quality results for common tasks.

## Available Skills

| Skill | Description |
|-------|-------------|
| [prompt-improver](./prompt-improver/) | Build or improve prompts using the 6-component framework (Persona, Task, Steps, Context, Goal, Format) |

## Installation

### Claude.ai (Web)

1. Start a new conversation
2. Click the **Projects** icon in the sidebar
3. Create or select a project
4. Go to **Project instructions**
5. Copy the contents of `SKILL.md` and paste it into the instructions

### Claude Desktop

1. Open Claude Desktop settings
2. Navigate to **Custom Instructions** or **System Prompt**
3. Copy the contents of `SKILL.md` and paste it

### Claude Code (CLI)

**Option 1: Manual**
```bash
# Copy SKILL.md to your project's .claude folder
cp prompt-improver/SKILL.md ~/.claude/skills/
```

**Option 2: Via Plugin**
```bash
# Add as a plugin (if supported)
claude plugin add ./prompt-improver
```

## Creating Your Own Skills

Use the `_template/` folder as a starting point:

1. Copy `_template/SKILL.md` to a new folder
2. Fill in the name, description, and instructions
3. Test your skill with various inputs
4. Submit a PR to share it!

## Documentation

- [Agent Skills Specification](https://agentskills.io) — Official standard for skills
- [Anthropic Skills Repository](https://github.com/anthropics/skills) — Official Anthropic skills

## License

MIT License — see [LICENSE](./LICENSE) for details.

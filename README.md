# Claude Skills

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A collection of reusable skills for Claude that enhance its capabilities for specific tasks.

## Available Skills

| Skill | Description |
|-------|-------------|
| [prompt-improver](./skills/prompt-improver/) | Build or improve prompts using the 6-component framework (Persona, Task, Steps, Context, Goal, Format) |

## Installation

### Claude Code (CLI)

```bash
claude plugin add vladkarpman/claude-skills
```

After installation, skills are available automatically. Invoke directly with `/claude-skills:prompt-improver` or let Claude use them when relevant.

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

## Creating Your Own Skills

Use the `skills/_template/` folder as a starting point:

1. Copy `skills/_template/SKILL.md` to `skills/your-skill-name/SKILL.md`
2. Fill in the name, description, and instructions
3. Test your skill
4. Submit a PR to share it

## Skill Format

Each skill is a markdown file with YAML frontmatter:

```yaml
---
name: skill-name
description: What this skill does and when Claude should use it.
---

# Skill Name

Instructions for Claude...
```

## License

MIT License — see [LICENSE](./LICENSE) for details.

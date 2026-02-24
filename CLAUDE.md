# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A Claude Code plugin providing prompt engineering skills. Users can analyze/improve existing prompts or build new ones using the 6-component framework (Persona, Task, Steps, Context, Goal, Format).

## Commands

| Command | Description |
|---------|-------------|
| `/improve-prompt [prompt]` | Analyze and improve an existing prompt |
| `/build-prompt [description]` | Build a new prompt from scratch interactively |
| `/think [task]` | Start any task with superpowers + Codex adversarial review |

## Architecture

```
.claude-plugin/plugin.json   # Plugin manifest (commands, metadata)
commands/                    # Slash command definitions
  improve-prompt.md          # Triggers skill in Improve Mode
  build-prompt.md            # Triggers skill in Build Mode
skills/
  prompt-improver/           # Main skill implementation
    SKILL.md                 # Core logic and framework (350 lines)
    README.md                # User documentation
    examples.md              # Before/after transformation examples
    tests.md                 # Test cases
  think/
    SKILL.md                 # Orchestrator + Codex review loop
    references/
      codex-review.md        # Review prompt template
  _template/                 # Template for new skills
```

**Flow**: Command (`/improve-prompt`) → Skill (`prompt-improver`) → Interactive workflow

## The 6-Component Framework

Every skill improvement follows this analysis structure:

| Component | Question |
|-----------|----------|
| Persona | What expert role should handle this? |
| Task | What specific action or output is needed? |
| Steps | What steps should be followed? (complex tasks only) |
| Context | What background/constraints apply? |
| Goal | How will you know the output is correct? |
| Format | What structure should the response have? |

## Skill Modes

**Improve Mode**: Analyze prompt → Show gaps (✅/🟡/❌) → Ask questions one-by-one → Generate improved version

**Build Mode**: Ask Task → Goal → Persona → Context → Steps → Format (one question at a time) → Generate prompt

## Adding New Skills

1. Create `skills/{skill-name}/SKILL.md` with YAML frontmatter (name, description)
2. Define "When to Use" triggers
3. Add step-by-step workflow
4. Include examples
5. Reference `skills/_template/SKILL.md` for structure

## Adding New Commands

1. Create `commands/{command}.md` with YAML frontmatter (name, description, arguments)
2. Add command path to `.claude-plugin/plugin.json` commands array
3. Command should reference and invoke the appropriate skill

## Key Files

- `skills/prompt-improver/SKILL.md` - Edit to change skill logic, framework, or workflow
- `skills/prompt-improver/examples.md` - Add transformation examples here
- `.claude-plugin/plugin.json` - Register new commands here

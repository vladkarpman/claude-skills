# Prompt Improver

Build or improve prompts using the proven 6-component framework.

## Features

- **Two modes**: Improve existing prompts or build new ones from scratch
- **6-component framework**: Persona, Task, Steps, Context, Goal, Format
- **Interactive guidance**: Analyzes gaps and asks targeted questions
- **Visual feedback**: Shows what's present (✅), weak (⚠️), or missing (❌)

## The Framework

| Component | Purpose |
|-----------|---------|
| **Persona** | Who should the AI be? |
| **Task** | What needs to be done? |
| **Steps** | How should it be done? |
| **Context** | What background/constraints? |
| **Goal** | What does success look like? |
| **Format** | How should output be structured? |

## Usage

### Improve Mode

> "Improve this prompt: write code to process data"

The skill will:
1. Analyze against the 6-component framework
2. Show a gap analysis table
3. Ask questions about missing components
4. Generate an improved prompt

### Build Mode

> "Help me create a prompt for writing blog posts"

The skill will:
1. Ask about each component one at a time
2. Build the prompt step by step
3. Generate the complete prompt

## Quick Example

**Before:**
```
write an email
```

**After:**
```
You are a senior manager at a tech company.

Write a professional email declining a meeting invitation.

Context:
- The meeting is a vendor sales pitch
- Maintain the relationship but decline politely

Goal:
- Clear decline, door open for future
- Under 100 words

Format:
- Standard email with subject line
```

## Files

- `SKILL.md` — Main skill instructions
- `examples.md` — More before/after examples
- `tests.md` — Test cases for validation

## Installation

See the [main README](../README.md) for installation instructions.

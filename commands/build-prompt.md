---
name: build-prompt
description: Build a new prompt from scratch using the 6-component framework
arguments:
  - name: description
    description: Brief description of what the prompt should do (optional)
    required: false
---

# Build Prompt

Use the prompt-improver skill in **Build Mode** to create a new prompt from scratch.

## Input

Initial description (if provided): `$ARGUMENTS`

## Process

Guide the user through building an effective prompt by asking about each component one at a time.

**Question order:**
1. **Task** (always): "What do you need the AI to do?"
2. **Goal** (always): "What does a successful result look like?"
3. **Persona** (skip for simple): "What kind of expert should handle this?"
4. **Context** (skip for simple): "Any context or constraints?"
5. **Steps** (only for complex): "Should the AI follow specific steps?"
6. **Format** (always, can be brief): "How should the output be formatted?"

After gathering answers:
- Generate the complete prompt
- Only include components that are relevant (don't over-engineer simple prompts)

Follow the full Build Mode process defined in the prompt-improver skill.

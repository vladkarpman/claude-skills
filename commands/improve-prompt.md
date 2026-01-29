---
name: improve-prompt
description: Analyze and improve an existing prompt using the 6-component framework
arguments:
  - name: prompt
    description: The prompt to improve (can also be provided after the command)
    required: false
---

# Improve Prompt

Use the prompt-improver skill in **Improve Mode** to analyze and enhance the provided prompt.

## Input

The prompt to improve: `$ARGUMENTS`

If no prompt was provided, ask the user to paste their prompt.

## Process

1. Analyze the prompt against the 6-component framework (Persona, Task, Steps, Context, Goal, Format)
2. Show the Framework Analysis table with status indicators (✅ 🟡 ❌)
3. Assess complexity (Simple/Medium/Complex) to determine which components are needed
4. Ask ONE question at a time about missing or weak components
5. Generate the improved prompt once you have enough information
6. Explain what changed and why

Follow the full process defined in the prompt-improver skill.

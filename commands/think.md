---
name: think
description: Think through a hard problem with Claude x Codex deliberation
arguments:
  - name: task
    description: The problem, question, or decision you want to think through
    required: false
---

# Think

Use the think skill for structured deliberation on hard decisions.

## Input

The problem to think through: `$ARGUMENTS`

If no input was provided, ask the user what they'd like to think through.

## Process

1. Verify Codex CLI is installed (required)
2. Engage in Socratic dialogue — ask sharp questions, challenge assumptions, surface trade-offs
3. When ready, Claude and Codex deliberate to find the best recommendation
4. Present the recommendation for your feedback
5. Iterate until you're satisfied, then summarize and suggest next steps

Follow the full process defined in the think skill.

---
name: think
description: Think through a hard problem with a Socratic thinking partner and optional Codex second opinion
arguments:
  - name: task
    description: The problem, question, or decision you want to think through
    required: false
---

# Think

Use the think skill as a Socratic thinking partner for hard decisions.

## Input

The problem to think through: `$ARGUMENTS`

If no input was provided, ask the user what they'd like to think through.

## Process

1. Engage in Socratic dialogue — ask sharp questions, challenge assumptions, surface trade-offs
2. If the user requests a second opinion, get an independent take from Codex
3. When ready, summarize the decision, reasoning, and risks
4. Suggest the logical next skill based on the decision

Follow the full process defined in the think skill.

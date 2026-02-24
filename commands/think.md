---
name: think
description: Start working on any task with superpowers orchestration and Codex adversarial review
arguments:
  - name: task
    description: Description of what to work on, or path to an existing plan/artifact to review
    required: false
---

# Think

Use the think skill to orchestrate superpowers and add Codex CLI adversarial review.

## Input

The task to work on: `$ARGUMENTS`

If no input was provided, ask the user what they'd like to work on.

## Process

1. Understand the task — detect if input is a file path or description
2. Route to appropriate superpowers skills (brainstorming, writing-plans, systematic-debugging)
3. Run the output through the Codex CLI review loop until APPROVED (max 5 rounds)
4. Save the final artifact and suggest next steps

Follow the full process defined in the think skill.

---
name: team-up
description: Orchestrate an agent team to implement a task with prompt refinement, team design, execution, and code review
arguments:
  - name: task
    description: Description of the task to implement with an agent team
    required: false
---

# Team Up

Orchestrate a full agent team lifecycle: refine the task, design a team, execute in parallel, and review the result.

## Input

The task to team up on: `$ARGUMENTS`

If no input was provided, ask the user what they'd like the team to work on.

## Process

1. Understand and refine the task using the 6-component framework
2. Analyze the codebase and design a team plan with file ownership
3. Create the team, spawn teammates, and coordinate execution
4. Run a final code review and clean up

Follow the full process defined in the team-up skill.

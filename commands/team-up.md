---
name: team-up
description: Create and orchestrate an agent team for a task — always spawns teammates
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

**CRITICAL: This command MUST result in `TeamCreate` + `Agent` tool spawning. Do NOT implement the task yourself. You are the lead — you orchestrate.**

1. Understand and refine the task (skip if already well-defined — don't over-refine)
2. Analyze the codebase and design a team plan with file ownership
3. **Create the team (`TeamCreate`), spawn teammates (`Agent` tool), and coordinate execution**
4. Run a final code review and clean up

Follow the full process defined in the team-up skill. If you get stuck in Steps 1-2, skip to Step 3 — team creation is non-negotiable.

# Team-Up Superpowers Awareness

Make team-up teammates superpowers-aware Opus agents running at max effort.

## Problem

Teammates spawned by team-up are plain code-and-report workers. They don't invoke superpowers skills (subagent-driven-development, TDD, debugging, code review) and use the default model/effort. This leaves quality and capability on the table.

## Solution

Two targeted changes to make teammates skill-aware and high-powered:

1. **Teammate prompt template** — add a `## Superpowers` section listing key skills and the invocation rule
2. **SKILL.md** — add effort pre-flight check (Step 5e) and set `model: "opus"` on all Agent spawns (Step 6)

## Changes

### 1. `skills/team-up/references/teammate-prompt-template.md`

Add inside the template code block, after the `## Rules` section content and before the closing triple backtick (`` ``` ``):

```markdown
## Superpowers

You have access to superpowers skills via the Skill tool. Before starting each task, check if a skill applies. Key skills for implementation work:

- **`superpowers:subagent-driven-development`** — Use when your task involves multiple files or complex implementation. Dispatches sub-agents with two-stage review.
- **`superpowers:test-driven-development`** — Use for writing tests and test-first implementation.
- **`superpowers:debugging`** — Use when you hit unexpected failures or need to investigate issues.
- **`superpowers:requesting-code-review`** — Use after completing implementation to self-review before reporting done.

**Rule:** If there's even a 1% chance a skill applies, invoke it via the Skill tool. Don't rationalize skipping it.

**When NOT to use subagent-driven-development:**
- Task is a single-file change under ~20 lines
- Task is purely mechanical (rename, move, format)
- For these, implement directly and verify.
```

### 2. `skills/team-up/SKILL.md`

#### Add Step 5e: Verify effort configuration

Insert after Step 5d (Assign owners), before Step 6:

```markdown
**5e. Verify effort configuration**

Before spawning teammates, check that the session is configured for maximum performance. Inform the user of the current effort level:

> **Pre-flight check:** Teammates inherit the session's effort level and will run on Opus. For best results, effort should be set to `max`. You can set it via `/model` (arrow keys), `claude --effort max`, or `CLAUDE_CODE_EFFORT_LEVEL=max`.

If effort is not `max`, recommend the user adjust it and offer to wait. If already `max`, proceed directly to Step 6.
```

#### Update Step 6: Add model parameter

Update the Agent spawn parameters to include `model: "opus"`:

```markdown
3. Spawn via Agent tool:
   - `name`: teammate name from the plan
   - `team_name`: team name from Step 5a
   - `prompt`: the filled-in template
   - `mode`: "auto"
   - `model`: "opus"
```

## Files Changed

| File | Change |
|------|--------|
| `skills/team-up/SKILL.md` | Add Step 5e (effort pre-flight), update Step 6 spawn params |
| `skills/team-up/references/teammate-prompt-template.md` | Add `## Superpowers` section |

## Files NOT Changed

- `commands/team-up.md` — no changes needed
- `references/team-plan-template.md` — no changes needed
- No new files

## Limitations

- **Effort is session-level only.** The `max` effort level cannot be set per-agent; teammates inherit the lead's session effort. The pre-flight check in Step 5e addresses this by prompting the user to configure it before spawning.
- **`max` effort is Opus 4.6 only.** Other models return an error if `max` is requested. Since we set `model: "opus"` on all spawns, this is compatible.

## Design Decisions

- **Concise skill list over full using-superpowers embed.** The full using-superpowers content is ~200 lines. Embedding it would bloat teammate prompts. Instead, we list the 4 most relevant skills and the core invocation rule. Teammates can invoke any skill — these are just the ones most likely to apply.
- **Pre-flight check over silent assumption.** Rather than assuming effort is configured, we explicitly ask. This avoids silent quality degradation.
- **No changes to team plan template.** Model and effort are runtime concerns, not planning concerns. The plan template stays focused on task decomposition.
- **`executing-plans` excluded from skill list.** Teammates receive their plan from the lead, so they don't need to create their own. `subagent-driven-development` is the relevant execution skill for teammates.
- **`model: "opus"` is verified** against the Agent tool schema (accepts enum: "sonnet", "opus", "haiku"). Update if the default best model changes.

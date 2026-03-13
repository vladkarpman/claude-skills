# Team-Up Superpowers Awareness Implementation Plan

> **For agentic workers:** REQUIRED: Use superpowers:subagent-driven-development (if subagents available) or superpowers:executing-plans to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Make team-up teammates superpowers-aware Opus agents running at max effort.

**Architecture:** Two edits to existing files — add a Superpowers section to the teammate prompt template, and update SKILL.md with an effort pre-flight check and model parameter.

**Tech Stack:** Markdown (Claude Code plugin skill files)

**Spec:** `docs/superpowers/specs/2026-03-13-team-up-superpowers-awareness-design.md`

---

## File Structure

No new files. Two existing files modified:

| File | Responsibility | Change |
|------|---------------|--------|
| `skills/team-up/references/teammate-prompt-template.md` | Template for teammate system prompts | Add `## Superpowers` section inside template code block |
| `skills/team-up/SKILL.md` | Team-up skill process definition | Add Step 5e + update Step 6 spawn params |

---

## Task 1: Add Superpowers section to teammate prompt template

**Files:**
- Modify: `skills/team-up/references/teammate-prompt-template.md:49` (insert before closing code fence at line 50)

- [ ] **Step 1: Add the Superpowers section**

Insert the following after line 49 (`- **Commit nothing.** The lead handles all git operations. Focus on code changes only.`) and before line 50 (the closing `` ``` ``):

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

- [ ] **Step 2: Verify the template structure**

Read the file and confirm:
- The `## Superpowers` section is inside the template code block (between the opening `` ``` `` at line 9 and the closing `` ``` ``)
- It appears after `## Rules` and before the closing code fence
- No duplicate sections

- [ ] **Step 3: Commit**

```bash
cd /Users/vladislavkarpman/ClaudeProjects/claude-skills
git add skills/team-up/references/teammate-prompt-template.md
git commit -m "feat: add superpowers skill awareness to teammate prompt template"
```

---

## Task 2: Add effort pre-flight check and model parameter to SKILL.md

**Files:**
- Modify: `skills/team-up/SKILL.md:145-164` (insert Step 5e after line 145, update Step 6 spawn params at line 160-164)

- [ ] **Step 1: Add Step 5e after Step 5d**

Insert the following after line 145 (`**Confirm creation to the user** with a summary: team name, number of tasks, number of teammates.`) and before line 147 (`### Step 6: Spawn Teammates`):

```markdown

**5e. Verify effort configuration**

Before spawning teammates, check that the session is configured for maximum performance. Inform the user of the current effort level:

> **Pre-flight check:** Teammates inherit the session's effort level and will run on Opus. For best results, effort should be set to `max`. You can set it via `/model` (arrow keys), `claude --effort max`, or `CLAUDE_CODE_EFFORT_LEVEL=max`.

If effort is not `max`, recommend the user adjust it and offer to wait. If already `max`, proceed directly to Step 6.
```

- [ ] **Step 2: Add model parameter to Step 6 spawn params**

Find the current spawn parameters (around line 160-164):

```markdown
3. Spawn via Agent tool:
   - `name`: teammate name from the plan
   - `team_name`: team name from Step 5a
   - `prompt`: the filled-in template
   - `mode`: "auto"
```

Replace with:

```markdown
3. Spawn via Agent tool:
   - `name`: teammate name from the plan
   - `team_name`: team name from Step 5a
   - `prompt`: the filled-in template
   - `mode`: "auto"
   - `model`: "opus"
```

- [ ] **Step 3: Verify the changes**

Read the file and confirm:
- Step 5e appears between Step 5d and Step 6
- Step 5e uses `**5e.**` bold formatting consistent with 5a-5d
- The spawn parameters in Step 6 include `model: "opus"` as the last item
- No other content was displaced or duplicated

- [ ] **Step 4: Commit**

```bash
cd /Users/vladislavkarpman/ClaudeProjects/claude-skills
git add skills/team-up/SKILL.md
git commit -m "feat: add effort pre-flight check and opus model to team-up spawning"
```

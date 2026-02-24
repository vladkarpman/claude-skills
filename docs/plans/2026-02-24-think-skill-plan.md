# /think Skill Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Create a `/think` command and skill that orchestrates superpowers and adds Codex CLI adversarial review as a second opinion on every output artifact.

**Architecture:** Thin orchestrator skill (`SKILL.md`) that routes to superpowers skills based on task intent, then runs a Codex CLI review loop (max 5 rounds) until VERDICT: APPROVED. Command entry point (`think.md`) routes to the skill.

**Tech Stack:** Claude Code skills (markdown), OpenAI Codex CLI (`codex exec`), Bash for shell commands

---

### Task 1: Create the Codex review prompt template

**Files:**
- Create: `skills/think/references/codex-review.md`

**Step 1: Create the directory**

```bash
mkdir -p skills/think/references
```

**Step 2: Write the Codex review prompt**

This is the prompt template sent to Codex CLI. It must instruct Codex to review the artifact, look for specific issue categories, and return a structured verdict.

Create `skills/think/references/codex-review.md` with this content:

```markdown
# Codex Review Prompt

You are a senior technical reviewer. You have been given an artifact (plan, analysis, diagnosis, or architecture document) produced by another AI assistant. Your job is to provide an independent, adversarial review.

## What to Review

Evaluate the artifact against these categories:

1. **Completeness** — Are there missing steps, undefined terms, or gaps in logic?
2. **Correctness** — Are there factual errors, wrong assumptions, or flawed reasoning?
3. **Security** — Are there authentication gaps, injection risks, missing validation, or exposed secrets?
4. **Consistency** — Are there contradictions, conflicting field names, or incompatible design choices?
5. **Edge Cases** — Are failure modes, error handling, and boundary conditions addressed?
6. **Practicality** — Is the approach realistic? Are there broken commands, missing dependencies, or untested assumptions?

## Response Format

List every issue you find with:
- **Category** (from the list above)
- **Issue** (specific description)
- **Suggestion** (concrete fix)

Then end with exactly one of:

VERDICT: APPROVED

or

VERDICT: REVISE

Use APPROVED only when you have zero remaining issues. If you have any concerns, use REVISE.

## Rules

- Be thorough and adversarial — your job is to find problems, not praise
- Be specific — "needs better error handling" is useless; "the /api/users endpoint has no auth check" is useful
- Do not invent requirements that aren't implied by the artifact's stated goals
- If this is a revision, verify that previously raised issues have been addressed before raising new ones
```

**Step 3: Commit**

```bash
git add skills/think/references/codex-review.md
git commit -m "feat: add Codex review prompt template for /think skill"
```

---

### Task 2: Create the main SKILL.md

**Files:**
- Create: `skills/think/SKILL.md`

**Step 1: Write the skill file**

Create `skills/think/SKILL.md` with this content:

```markdown
---
name: think
description: >
  Universal entry point for any task. Use when the user wants to start working on something —
  implementing a feature, debugging a bug, reviewing architecture, or finding improvements.
  Orchestrates superpowers skills and adds OpenAI Codex CLI as an adversarial second opinion.
  Trigger phrases: "think about", "let's think", "start working on", or invoked via /think command.
---

# Think

Universal entry point for tackling any task. Orchestrate the right superpowers skills based on what the user needs, then run every output artifact through an adversarial Codex CLI review loop for a second opinion from a different model.

## Prerequisites

Verify before starting:

1. **OpenAI Codex CLI** installed and on PATH. Verify with `codex --version`. If missing: `npm install -g @openai/codex`.

---

## The Process

### Step 1: Understand the Task

Read the user's input and determine what they need.

**Input detection:**
- If the input is a **file path** that exists on disk → skip to Step 3 (Codex review loop on that file)
- If the input is a **description** → continue to Step 2

Do NOT use hardcoded keywords or classification tables. Read the intent naturally and determine which superpowers skills to use.

**Available superpowers and when to use them:**

| Skill | Use When |
|-------|----------|
| `superpowers:brainstorming` | Exploring ideas, design decisions, open-ended problems |
| `superpowers:writing-plans` | Creating implementation plans, concrete task breakdowns |
| `superpowers:systematic-debugging` | Diagnosing bugs, test failures, unexpected behavior |

Use your judgment for sequencing. For example:
- New feature → brainstorming first, then writing-plans
- Bug report → systematic-debugging
- Architecture review → brainstorming (exploration mode)
- Refactoring → brainstorming, then writing-plans

### Step 2: Orchestrate Superpowers

Invoke the selected superpowers skills using the Skill tool. Let each skill's interactive workflow proceed normally — brainstorming will ask questions, writing-plans will produce a plan document, debugging will investigate.

**Your role here is coordination, not duplication.** Do not reimplement what superpowers already do. Invoke the skill and let it run.

Once the superpowers workflow produces an output artifact (a plan, diagnosis, analysis, or design document), save it and proceed to Step 3.

### Step 3: Codex Review Loop

Run the output artifact through an adversarial review loop using OpenAI Codex CLI.

**3a. Save the artifact**

Generate a UUID and save the artifact to a temp file:

```bash
THINK_UUID=$(uuidgen | tr '[:upper:]' '[:lower:]')
# Save the artifact content to /tmp/think-${THINK_UUID}.md
```

Write the full artifact content to `/tmp/think-${THINK_UUID}.md`.

**3b. First Codex review**

Read the review prompt from `references/codex-review.md` in this skill's directory. Run:

```bash
codex exec --quiet -p "<review-prompt-content>

Review the following artifact:

$(cat /tmp/think-${THINK_UUID}.md)"
```

Capture the output and the session ID from the Codex response.

**3c. Parse the verdict**

Look for exactly one of:
- `VERDICT: APPROVED` → proceed to Step 4
- `VERDICT: REVISE` → continue to 3d

**3d. Revise and resubmit**

Address each issue raised by Codex:
- Read the feedback items
- Revise the artifact to address each one
- Overwrite `/tmp/think-${THINK_UUID}.md` with the revised content

Resume the Codex session to re-review:

```bash
codex exec resume <session-id> --quiet -p "I have revised the artifact to address your feedback. Please re-review the updated version:

$(cat /tmp/think-${THINK_UUID}.md)"
```

**3e. Loop**

Repeat 3c-3d until:
- `VERDICT: APPROVED` → proceed to Step 4
- **Max 5 rounds reached** → stop, present the best version and list any unresolved items from the last review

### Step 4: Save and Present

**4a. Save the final artifact**

Save to `docs/plans/YYYY-MM-DD-<topic>.md` with this header prepended:

```markdown
---
think-session: <uuid>
task-type: <implementation|debugging|review|improvement>
codex-rounds: <N>
codex-verdict: <APPROVED|MAX_ROUNDS>
date: YYYY-MM-DD
---
```

**4b. Present the summary**

Show the user:

1. What was produced (plan, diagnosis, analysis)
2. How many Codex review rounds it took
3. Key issues Codex caught and how they were addressed
4. Any unresolved items (if max rounds hit)

**4c. Suggest next step**

Based on the task type, suggest the logical next action:
- Implementation plan → "Ready for `superpowers:executing-plans` or `superpowers:subagent-driven-development`"
- Bug diagnosis → "Ready to implement the fix"
- Architecture review → "Consider creating an implementation plan for the recommended changes"

---

## Examples

### Example 1: New Feature

**User provides:**
> /think add user authentication to the REST API

**Process:**
1. Understand → implementation task
2. Invoke `superpowers:brainstorming` → explore auth approaches (JWT vs sessions, OAuth providers)
3. Invoke `superpowers:writing-plans` → produce implementation plan
4. Codex review loop:
   - Round 1: REVISE — "No rate limiting on login endpoint, no token rotation strategy"
   - Round 2: REVISE — "Password hashing uses bcrypt but cost factor not specified"
   - Round 3: APPROVED
5. Save to `docs/plans/2026-02-24-user-auth.md`, suggest `executing-plans`

### Example 2: Bug Investigation

**User provides:**
> /think the app crashes when users upload files larger than 10MB

**Process:**
1. Understand → debugging task
2. Invoke `superpowers:systematic-debugging` → investigate, form hypotheses, diagnose
3. Codex review of diagnosis:
   - Round 1: REVISE — "Diagnosis doesn't account for nginx proxy_max_body_size limit"
   - Round 2: APPROVED
4. Save diagnosis, suggest implementing the fix

### Example 3: Existing Plan Review

**User provides:**
> /think docs/plans/2026-02-24-migration-plan.md

**Process:**
1. Detect file path → skip to Codex review loop
2. Codex reviews the existing plan directly
3. Auto-revise based on feedback
4. Save updated plan

---

## Notes

- Codex runs in read-only mode — it reviews but never modifies files
- Each /think session gets a unique UUID for temp file isolation
- The review prompt lives in `references/codex-review.md` — edit it to tune what Codex looks for
- If Codex CLI is not installed, the skill will inform the user and provide installation instructions
- Superpowers skills run their full interactive workflows — this skill does not shortcut them
```

**Step 2: Commit**

```bash
git add skills/think/SKILL.md
git commit -m "feat: add /think skill — superpowers orchestrator with Codex review"
```

---

### Task 3: Create the command entry point

**Files:**
- Create: `commands/think.md`

**Step 1: Write the command file**

Create `commands/think.md` following the existing command pattern (see `commands/improve-prompt.md`):

```markdown
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
```

**Step 2: Commit**

```bash
git add commands/think.md
git commit -m "feat: add /think command entry point"
```

---

### Task 4: Register the command in plugin.json

**Files:**
- Modify: `.claude-plugin/plugin.json`

**Step 1: Add the command path**

Add `"./commands/think.md"` to the `commands` array in `.claude-plugin/plugin.json`. The result should be:

```json
{
  "name": "claude-skills",
  "version": "1.6.0",
  "description": "A collection of reusable skills for Claude that enhance its capabilities for specific tasks",
  "author": {
    "name": "Vladislav Karpman",
    "url": "https://github.com/vladkarpman"
  },
  "homepage": "https://github.com/vladkarpman/claude-skills",
  "repository": "https://github.com/vladkarpman/claude-skills",
  "license": "MIT",
  "keywords": ["skills", "prompts", "prompt-engineering", "claude", "ai"],
  "commands": [
    "./commands/improve-prompt.md",
    "./commands/build-prompt.md",
    "./commands/think.md"
  ]
}
```

Note: bump version from `1.5.0` to `1.6.0` (new feature).

**Step 2: Commit**

```bash
git add .claude-plugin/plugin.json
git commit -m "feat: register /think command and bump to v1.6.0"
```

---

### Task 5: Update project CLAUDE.md

**Files:**
- Modify: `CLAUDE.md`

**Step 1: Add /think to the Commands table**

Add this row to the Commands table:

```markdown
| `/think [task]` | Start any task with superpowers + Codex adversarial review |
```

**Step 2: Add think skill to Architecture section**

Add to the architecture tree:

```
  think/
    SKILL.md                 # Orchestrator + Codex review loop
    references/
      codex-review.md        # Review prompt template
```

**Step 3: Commit**

```bash
git add CLAUDE.md
git commit -m "docs: add /think skill to project documentation"
```

---

### Task 6: Manual verification

**Step 1: Verify Codex CLI is available**

```bash
codex --version
```

Expected: version number output (e.g., `0.1.x`)

**Step 2: Verify plugin structure**

```bash
ls -la skills/think/
ls -la skills/think/references/
ls -la commands/think.md
```

Expected: all files exist

**Step 3: Verify plugin.json is valid JSON**

```bash
python3 -c "import json; json.load(open('.claude-plugin/plugin.json')); print('Valid')"
```

Expected: `Valid`

**Step 4: Test /think with a simple task**

Invoke in a new Claude Code session:

```
/think add a health check endpoint to the API
```

Expected behavior:
1. Skill detects implementation task
2. Invokes brainstorming (asks clarifying questions)
3. Invokes writing-plans (produces plan)
4. Runs Codex review loop
5. Saves approved artifact to `docs/plans/`

**Step 5: Test /think with a file path**

```
/think docs/plans/2026-02-24-think-skill-design.md
```

Expected behavior:
1. Detects file path
2. Skips to Codex review loop
3. Reviews the design doc
4. Saves reviewed version

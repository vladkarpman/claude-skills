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

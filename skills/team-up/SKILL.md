---
name: team-up
description: >
  Full lifecycle agent team orchestrator. Trigger phrases: "team up", "create a team",
  "parallelize this", "use a team", or /team-up command. Takes a task, refines the prompt,
  designs team structure, creates the team, coordinates execution, and runs final code review.
---

# Team Up

Full lifecycle agent team orchestrator. Takes a rough task description and turns it into a coordinated team deployment: refine the prompt, design team structure with file ownership, create the team, coordinate parallel execution, and run a final code review.

---

## The Process

### Step 1: Understand the Task

Read the user's input from `$ARGUMENTS`.

**If vague or single-sentence:**
- Proceed to Step 2 for refinement

**If already detailed (multi-paragraph with clear requirements):**
- Ask: "This looks well-defined. Want me to refine it further, or skip straight to team design?"
- If skip → proceed to Step 3

### Step 2: Refine with 6-Component Framework

Apply the 6-component framework to strengthen the task description before team decomposition.

**Analyze the input against all 6 components:**

| Component | Status | Assessment |
|-----------|--------|------------|
| Persona | (filled/vague/missing) | What expert role should handle this? |
| Task | (filled/vague/missing) | What specific action or output is needed? |
| Steps | (filled/vague/missing) | What steps should be followed? |
| Context | (filled/vague/missing) | What background/constraints apply? |
| Goal | (filled/vague/missing) | How will you know the output is correct? |
| Format | (filled/vague/missing) | What structure should the response have? |

**Show the gap table** to the user with status indicators.

**For team tasks, prioritize filling gaps in this order:**
1. **Task** — What exactly needs to be built/changed?
2. **Goal** — What does "done" look like? What are the acceptance criteria?
3. **Context** — What existing code, patterns, or constraints matter?
4. **Steps** — What's the rough breakdown of work?

Persona and Format are less critical for team tasks — skip them unless clearly relevant.

**Ask ONE question at a time** for each missing/vague component. After each answer, update the assessment and ask the next.

**Synthesize** the improved task description once all critical gaps are filled. Show it to the user for confirmation.

### Step 3: Analyze Codebase

Explore the codebase to inform team design decisions.

**Investigate:**
- Project structure and key directories
- Existing patterns and conventions (naming, architecture, frameworks)
- Test infrastructure (test runner, test file locations, patterns)
- Natural file ownership boundaries (frontend vs backend, feature modules, layers)

**Present a brief summary** of findings relevant to team decomposition. Keep it to 5-10 bullet points.

### Step 4: Design Team Plan

Decompose the refined task into a team structure.

**Rules for decomposition:**
- **3-6 tasks per teammate** — the sweet spot for productivity
- **No shared file ownership** — each file has exactly one Read/Write owner
- **DAG dependencies** — no circular dependencies between tasks
- **Testing included** — at least one testing task per teammate
- **Max 4 teammates** by default (user can override)

**Read the template** from `references/team-plan-template.md` and fill in all sections:
1. Team Overview table
2. Task Breakdown table
3. Dependency Graph
4. File Ownership Map

**Run the verification checklist** from the template before presenting.

**Present the full plan to the user.**

**CHECKPOINT: Wait for explicit user approval before proceeding.** If the user requests changes, iterate on the plan. Do NOT create the team until approved.

### Step 5: Create Team and Tasks

After user approval, create the team infrastructure.

**5a. Create the team**

Generate a team name as a short slug derived from the task (e.g., `auth-system`, `api-refactor`).

Use `TeamCreate` with:
- `name`: the slug
- `description`: one-sentence summary of the task

**5b. Create tasks**

Use `TaskCreate` for each task from the plan:
- `subject`: task title
- `description`: include acceptance criteria, file ownership (Read/Write files and Read-only files), and any relevant context
- `activeForm`: present continuous form (e.g., "Implementing user model")

**5c. Set up dependencies**

Use `TaskUpdate` with `addBlockedBy` to wire up the dependency graph from Step 4.

**5d. Assign owners**

Use `TaskUpdate` with `owner` to assign each task to the appropriate teammate name.

**Confirm creation to the user** with a summary: team name, number of tasks, number of teammates.

### Step 6: Spawn Teammates

Spawn each teammate using the Agent tool.

**For each teammate:**

1. Read `references/teammate-prompt-template.md`
2. Fill in the template with:
   - Teammate name and role from the plan
   - Team name from Step 5a
   - The refined task description from Step 2
   - Their assigned tasks with acceptance criteria
   - Their file ownership boundaries
3. Spawn via Agent tool:
   - `name`: teammate name from the plan
   - `team_name`: team name from Step 5a
   - `prompt`: the filled-in template
   - `mode`: "auto"

**Spawn all teammates in parallel** — include all Agent tool calls in a single message.

### Step 7: Coordinate Execution

Monitor the team and handle issues as they arise.

**Your role as lead:**
- Teammate messages arrive automatically — read and respond to them
- Handle blockers: reassign tasks, clarify requirements, unblock dependencies
- If a teammate reports completion of all their tasks, acknowledge it
- Periodically check `TaskList` to assess overall progress

**Exit conditions:**
- **All tasks completed** → proceed to Step 8
- **Unresolvable blocker** → inform the user and ask for guidance
- **Teammate stuck after 2 messages about the same issue** → intervene directly or ask the user

### Step 8: Final Review and Cleanup

**8a. Code review**

Invoke `superpowers:requesting-code-review` via the Skill tool to review all changes made by the team.

**8b. Handle review results**

- **Issues found** → create follow-up tasks, assign to available teammates, send them messages, loop back to Step 7
- **Review passes** → proceed to cleanup

**8c. Cleanup**

1. Send shutdown message to all teammates:
   `SendMessage` to team with "All tasks complete. Shutting down. Thank you."
2. Clean up the team:
   `TeamDelete` with the team name

**8d. Present final summary**

Show the user:
- What was built (high-level description)
- Files created/modified (grouped by teammate)
- Code review status (passed/issues addressed)
- Suggested next steps (commit, manual testing, deployment)

---

## Examples

### Example 1: Full-Stack Feature

**User provides:**
> /team-up add user authentication with JWT tokens

**Process:**
1. Refine → clarify: password hashing strategy, token expiration, refresh tokens needed?
2. Analyze codebase → Express.js backend, React frontend, Jest tests, Prisma ORM
3. Design team:
   - `backend-dev`: user model, auth routes, JWT middleware (5 tasks)
   - `frontend-dev`: login/register pages, auth context, protected routes (5 tasks)
   - `test-dev`: integration tests, e2e auth flow tests (4 tasks)
4. User approves → create team, spawn 3 teammates
5. Coordinate → backend-dev finishes first, unblocks test-dev
6. Code review → catches missing rate limiting → follow-up task for backend-dev
7. All done → present summary, suggest committing

### Example 2: Refactoring Task

**User provides:**
> /team-up refactor the payment module to use the strategy pattern

**Process:**
1. Refine → clarify: which payment providers, backward compatibility needed?
2. Analyze codebase → monolithic PaymentService with if/else chains, 3 providers
3. Design team:
   - `architect`: strategy interface, provider implementations (4 tasks)
   - `migrator`: update call sites, remove old code, update DI (5 tasks)
4. User approves → create team, spawn 2 teammates
5. Coordinate → architect finishes interface first, unblocks migrator
6. Code review → passes cleanly
7. Done → present summary

---

## Notes

- **Cleanup on interruption**: If the user stops the skill mid-execution, remind them to run `TeamDelete` manually to clean up any orphaned team
- **File ownership is non-negotiable**: If a teammate needs to modify a file they don't own, they must message the owner. The lead should mediate if needed.
- **Scaling**: For tasks requiring more than 4 teammates, ask the user to confirm. Large teams have higher coordination overhead.
- **Git**: The lead (you) handles all git operations. Teammates focus on code changes only. Commit after the final review passes.

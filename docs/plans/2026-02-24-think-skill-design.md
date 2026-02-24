# /think Skill Design

**Date:** 2026-02-24
**Status:** Approved

## Overview

A `/think` command that serves as the primary entry point for any task — implementation, debugging, architecture review, improvements. It orchestrates existing superpowers skills and adds an adversarial Codex CLI review loop as a second opinion on every output.

## Decisions

| Decision | Choice | Rationale |
|----------|--------|-----------|
| Reviewer | OpenAI Codex CLI | True cross-model adversarial review, different architecture catches different blind spots |
| Codex role | Side review / second opinion | Not a blocking gate — Claude does the work, Codex provides independent perspective |
| Scope | Universal entry point | Features, bugs, architecture, improvements — any task type |
| Routing | No classification step | List available superpowers, let Claude pick the right one naturally |
| Input detection | Auto-detect | File path → skip to review; description → full pipeline |
| Iteration | Auto-iterate | Claude revises based on Codex feedback, loops until APPROVED (max 5 rounds) |
| Command name | `/think` | Short, memorable, captures "two models thinking deeply before acting" |

## Architecture

Three layers:

```
┌─────────────────────────────────────┐
│  Layer 1: Task Understanding        │
│  Read user input, pick superpowers  │
├─────────────────────────────────────┤
│  Layer 2: Superpowers Orchestration │
│  Invoke the right skills in order   │
│  Collect output artifacts           │
├─────────────────────────────────────┤
│  Layer 3: Codex Review Loop         │
│  Send artifact to Codex CLI         │
│  Parse VERDICT, auto-revise         │
│  Iterate until APPROVED (max 5)     │
└─────────────────────────────────────┘
```

### File Structure

```
skills/
  think/
    SKILL.md              # Orchestrator logic + review loop
    references/
      codex-review.md     # Codex review prompt template + verdict format
commands/
  think.md                # Slash command entry point
```

## Layer 1: Task Understanding

No hardcoded signal words or classification tables. The skill lists available superpowers and their purposes, then trusts Claude to pick the right one:

- `brainstorming` — exploring ideas, design decisions
- `writing-plans` — creating implementation plans
- `systematic-debugging` — diagnosing bugs
- Sequencing is natural (e.g., brainstorming before writing-plans for new features)

**Input detection:**
- If input is a file path that exists → skip to Layer 3 (Codex review on that file)
- If input is a description → run full pipeline starting from Layer 1

## Layer 2: Superpowers Orchestration

Invoke the selected superpowers skills via the Skill tool. The interactive workflows (brainstorming questions, plan checkpoints) proceed normally. Once a superpowers skill produces an output artifact (plan, diagnosis, analysis), hand it to Layer 3.

## Layer 3: Codex Review Loop

```
Claude produces artifact (plan/diagnosis/analysis)
    │
    ▼
Save to /tmp/think-<uuid>.md
    │
    ▼
codex exec --quiet -p "<review prompt>" /tmp/think-<uuid>.md
    │
    ▼
Parse response for VERDICT
    │
    ├─ VERDICT: APPROVED → done, save final artifact
    │
    └─ VERDICT: REVISE + feedback
        │
        ▼
    Claude addresses each feedback item
    Overwrites /tmp/think-<uuid>.md
        │
        ▼
    codex exec resume <session-id> --quiet -p "Review the revision"
        │
        ▼
    (loop back to Parse)
        │
    Max 5 rounds → stop, present best version with unresolved items noted
```

**Key details:**

- **UUID per session** — unique ID for temp files, avoids collisions
- **Session resumption** — `codex exec resume <session-id>` keeps Codex context across rounds
- **Codex runs read-only** — reviews but never modifies files
- **Review prompt** — structured prompt in `references/codex-review.md` covering security, completeness, edge cases, consistency
- **Max 5 rounds** — safety cap

## Output & Completion

After Codex approves:

1. **Save artifact** to `docs/plans/YYYY-MM-DD-<topic>.md` with metadata (task type, review rounds, verdict)
2. **Present summary** — what was produced, key decisions, what Codex caught
3. **Suggest next step** — the logical next superpowers skill (e.g., `executing-plans`, `subagent-driven-development`)

## Inspiration

Based on the iterative plan review concept from [aseemshrey.in](https://aseemshrey.in/blog/claude-codex-iterative-plan-review/), adapted to integrate with the superpowers skill ecosystem rather than being a standalone review tool.

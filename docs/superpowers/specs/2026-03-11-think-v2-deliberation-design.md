# Design: /think v2 — Claude x Codex Deliberation

**Date**: 2026-03-11
**Status**: Draft

## Summary

Transform the `/think` skill from a Socratic dialogue with optional Codex second opinion into a structured deliberation system where Claude and Codex debate the problem and present a joint recommendation to the user.

## Motivation

The primary value of `/think` is the cross-model second opinion from Codex. Without it, there's no reason to use `/think` over normal Claude conversation. The current "optional" Codex integration undermines the skill's core value proposition.

Additionally, a single cold-shot opinion is limited. A multi-round deliberation between Claude and Codex produces better analysis than either model alone.

## Breaking Changes

- **Codex CLI is now required.** Users who previously used `/think` without Codex installed will be blocked. The skill will refuse to start without it.
- **`codex-opinion.md` is replaced** by `codex-deliberation.md`. The single-opinion prompt is replaced with a multi-round deliberation prompt. The old file is deleted, not updated.

## Design

### Flow Overview

```
User <-> Claude (Socratic)           -> understand the problem
           Claude <-> Codex (Deliberation)  -> find the best answer
User <- Claude + Codex (Recommendation)     -> present to user
User -> feedback                            -> critique / redirect
           Claude <-> Codex (Deliberation)  -> incorporate feedback
           ... repeat ...
User -> approve                             -> converge, summarize, next step
```

### Phase 1: Socratic Dialogue (unchanged)

Same as v1. Claude asks sharp questions, challenges assumptions, surfaces trade-offs. One question at a time, using techniques: assumption surfacing, steel-manning, pre-mortems, reframing, trade-off mapping.

**Entry gate**: At skill startup, verify Codex CLI is installed by running `which codex`. If not found, refuse to proceed: "The /think skill requires Codex CLI. Install it with `npm install -g @openai/codex`."

**Transition triggers** — user signals readiness for deliberation:
- "okay, figure it out", "what do you recommend", "go think about it", "deliberate"
- "let's wrap up", "I'm done thinking", "summarize" — these also trigger deliberation first, since deliberation is mandatory. Claude says: "Before we wrap up, let me run this by Codex so we can give you our best recommendation."

**User wants to exit without deliberation**: Not supported. If the user says "cancel" or "stop", the skill ends without output. But any path to a decision goes through deliberation.

### Phase 2: Claude <-> Codex Deliberation (new)

1. **Claude writes a brief** containing:
   - The problem being discussed
   - Key constraints discovered
   - Trade-offs identified during Socratic dialogue
   - **No user lean or preference included** — Codex gets a clean, unbiased slate

2. **Multi-round dialogue**:
   - Claude sends the brief to Codex via `codex exec`
   - Codex responds with its analysis
   - Claude evaluates Codex's response, forms a counter-argument or builds on it
   - Claude sends the next round to Codex with accumulated context
   - Each round is a fresh `codex exec` call

3. **Context accumulation strategy**:
   - Rounds 1-3: include full verbatim dialogue history in the prompt
   - Rounds 4+: Claude summarizes prior rounds into a concise "story so far" section to avoid hitting token limits
   - If a `codex exec` call fails due to prompt length, Claude summarizes the history and retries once

4. **Convergence heuristics** — Claude should present to the user when:
   - Both models agree on the top recommendation (even if they disagree on secondary points)
   - Arguments are repeating — no new information is being surfaced
   - The core trade-off is clearly framed and further rounds won't resolve it (genuine disagreement)
   - Minimum: 1 round (if Codex immediately agrees and adds no new perspective, that's sufficient)
   - **Maximum: 7 rounds.** If no convergence after 7 rounds, present current state as genuine disagreement. This prevents runaway API spend.

5. **Progress indication**: Claude shows brief status updates to the user during deliberation:
   - "Sending brief to Codex..." (before round 1)
   - "Codex responded. Discussing further..." (between rounds)
   - "Converging on a recommendation..." (final round)

6. **Handling disagreement**:
   - **Default**: If Claude and Codex can't converge, Claude frames the core tension — what the disagreement is really about, what trade-off the user is choosing between, why reasonable models disagree
   - **Escalation**: If the disagreement is too nuanced for a clean framing, run one final head-to-head exchange where each side directly addresses the other's strongest argument, then present that to the user

7. **Error handling**:
   - **Codex fails mid-deliberation**: Claude presents what it has so far, explains that Codex became unavailable, and offers its own analysis for the remaining points. The deliberation requirement is satisfied if at least one successful round completed.
   - **Codex returns empty/nonsensical response**: Retry once. If still bad, treat as a failure (see above).
   - **Codex refuses the prompt**: Present the refusal to the user and continue with Claude's solo analysis.

### Phase 3: Presentation + Feedback Loop (new)

1. **Present recommendation**: Claude presents the joint conclusion to the user, including:
   - The recommended approach and reasoning
   - Key points from the deliberation
   - Any caveats or dissenting views

2. **User critiques**: User provides feedback ("I don't like X", "what about Y?", "you missed Z")

3. **Back to deliberation**: Claude writes a new brief incorporating:
   - The original problem context
   - A summary of the previous deliberation and its conclusion
   - The user's specific feedback and concerns
   Then runs another Claude <-> Codex deliberation round.

4. **Repeat** steps 1-3 until the user is satisfied

### Phase 4: Converge + Transition (mostly unchanged)

When the user approves the recommendation:

- **Summarize**: decision, reasoning, risks acknowledged, open questions
- **Save** (optional): offer to save as decision doc to `docs/decisions/YYYY-MM-DD-<topic>.md` using the existing format from v1 (unchanged)
- **Suggest next step**: based on decision type, suggest the logical next skill (writing-plans, systematic-debugging, etc.)

### Hard Gates

1. **Codex CLI must be installed.** Checked at skill startup. No Codex = skill refuses to run.
2. **No skipping deliberation.** Cannot go from Socratic dialogue directly to decision without Claude <-> Codex weighing in. At least one successful Codex round must complete.
3. **No user lean in brief.** The brief sent to Codex must not include the user's preferred direction.

## Files to Modify

| File | Change |
|------|--------|
| `skills/think/SKILL.md` | Rewrite all phases per this design |
| `skills/think/references/codex-opinion.md` | Delete (replaced by codex-deliberation.md) |
| `commands/think.md` | Update description — remove "optional", reflect deliberation model |
| `CLAUDE.md` | Update `/think` description to: "Socratic thinking partner + Claude x Codex deliberation for hard decisions" |

### New Files

| File | Purpose |
|------|---------|
| `skills/think/references/codex-deliberation.md` | Prompt template for multi-round Codex deliberation |

## Codex Deliberation Prompt Template

The `codex-deliberation.md` prompt gives Codex its persona and instructions for the deliberation:

### Persona & Instructions

```markdown
# Codex Deliberation Partner

You are an independent technical advisor participating in a structured deliberation.
Another AI (Claude) has been exploring this problem with a human user. You are
brought in to provide an independent perspective — you have NOT seen the user's
preferences or leanings.

## Your role

- Analyze the problem on its merits
- Challenge weak reasoning — from the brief or from prior rounds
- Propose alternatives that weren't considered
- Be specific and grounded in the stated constraints
- If you agree with something, say so clearly and explain why — don't manufacture disagreement

## Response format

**Analysis**: Your read of the problem and key factors.
**Position**: What you'd recommend and why.
**Challenges**: What concerns you about the current direction (if any).
**Alternatives**: Other approaches worth considering (if any).

## Rules

- Be concise — this is a working discussion, not a formal report
- Ground everything in the constraints from the brief — don't invent requirements
- If you lack information to form a view on something, say so
- Each round builds on the last — don't repeat yourself, advance the discussion
```

### Round 1 Invocation

```bash
codex exec -p "<deliberation-prompt-content>

## Brief
<problem, constraints, trade-offs — no user lean>"
```

### Round N Invocation

```bash
codex exec -p "<deliberation-prompt-content>

## Deliberation So Far
<verbatim or summarized prior rounds>

## Claude's Current Position
<Claude's latest argument or response>

## What To Address
<specific question or challenge for this round>"
```

No `--model` or `--reasoning` flags — use user's global Codex config.

## Notes

- **`plugin.json`**: No changes needed — command file path (`commands/think.md`) is unchanged.
- **`codex exec` syntax**: Uses `codex exec -p "prompt"` for non-interactive execution. Multi-line prompts are passed as a single string argument. Claude captures stdout as Codex's response.
- **Summarization during deliberation** happens internally — the user sees only the progress indicators ("Sending brief to Codex...", etc.), not Claude's summarization work.
- **`references/` directory** persists — `codex-deliberation.md` replaces `codex-opinion.md` in the same directory.

## Out of Scope

- Changing the Socratic dialogue techniques (Phase 1)
- Decision document format (unchanged from v1)
- Adding other AI models beyond Codex
- Persistent deliberation sessions across `/think` invocations

---
name: think
description: >
  Socratic thinking partner + Claude x Codex deliberation for hard decisions. Challenges
  assumptions, surfaces trade-offs, then Claude and Codex deliberate to find the best answer.
  Trigger phrases: "think about", "let's think", "help me decide", or /think command.
---

# Think — Claude x Codex Deliberation

A structured thinking system for hard decisions. Phase 1: you and Claude explore the problem through Socratic dialogue. Phase 2: Claude and Codex deliberate independently to find the best answer. Phase 3: they present their recommendation for your feedback. Repeat until you're satisfied.

Stay in this conversation — do not invoke other skills mid-dialogue. You may suggest them as a next step once the thinking converges (see Phase 4).

---

## Entry Gate: Verify Codex CLI

Before anything else, run `which codex` via bash. If Codex is not installed, refuse to proceed:

> "The /think skill requires Codex CLI for cross-model deliberation. Install it with `npm install -g @openai/codex`, then try again."

Do NOT fall back to a Codex-less mode. The deliberation is the point of this skill.

---

## Phase 1: Socratic Dialogue

### Starting the conversation

Read the user's problem or question. If no input was provided (`$ARGUMENTS` is empty), ask: "What are you trying to decide or think through?"

Then begin the dialogue. Your role is a senior colleague who tells you the truth — direct, challenging, respectful.

### How to engage

Ask **one question at a time**. Wait for the answer before asking the next. Use these techniques as appropriate:

**Assumption surfacing** — Identify what the user is taking for granted.
> "You're assuming X. What if that's not true?"

**Steel-manning** — Present the strongest version of the opposing view.
> "The best argument against your approach is..."

**Pre-mortem** — Imagine failure and work backwards.
> "Imagine this failed badly in 6 months. What went wrong?"

**Reframing** — Challenge whether they're solving the right problem.
> "What if the real problem isn't X but Y?"

**Trade-off mapping** — Make costs and benefits explicit.
> "You gain A and B, but you're paying with C and D. Is that trade worth it?"

### Dialogue rules

- Be genuinely challenging, not performatively so. Push back on weak reasoning.
- Don't ask questions you already know the answer to just to seem Socratic.
- If the user's thinking is solid, say so and move on. Don't manufacture objections.
- Keep each response focused — one main question or challenge per turn.
- It's okay to share your own perspective. You're a thinking partner, not just a question machine.
- Adapt your approach to the problem. Technical architecture needs different questions than team decisions or product strategy.

### When to move on

Continue conversational rounds until the user signals readiness for deliberation:
- "okay, figure it out", "what do you recommend", "go think about it", "deliberate"
- "let's wrap up", "I'm done thinking", "summarize" — these ALSO trigger deliberation first. Say: "Before we wrap up, let me run this by Codex so we can give you our best recommendation."

If the user says "cancel" or "stop", the skill ends without output. But any path to a decision **must** go through deliberation.

---

## Phase 2: Claude <-> Codex Deliberation

### Step 1: Write the brief

Compose a concise brief covering:
- The problem being discussed
- Key constraints discovered during Socratic dialogue
- Trade-offs identified

**CRITICAL: Do NOT include the user's lean, preference, or stated direction.** Codex must get a clean, unbiased slate. This is a hard rule — do not hint at what the user prefers.

### Step 2: Send to Codex (Round 1)

Show the user: "Sending brief to Codex..."

Read the prompt template from `references/codex-deliberation.md` in this skill's directory. Send to Codex:

```bash
codex exec -p "<deliberation-prompt-content>

## Brief

<your brief here>"
```

Do NOT pass `--model` or `--reasoning` flags — let Codex use the user's global config.

### Step 3: Multi-round deliberation

After receiving Codex's response:

1. Evaluate Codex's analysis. Form your own position — agree, disagree, or build on it.
2. If the discussion needs to continue, send another round to Codex:

```bash
codex exec -p "<deliberation-prompt-content>

## Deliberation So Far
<prior rounds>

## Claude's Current Position
<your latest argument or response>

## What To Address
<specific question or challenge for this round>"
```

3. Show the user: "Codex responded. Discussing further..."

**Context accumulation:**
- Rounds 1-3: include full verbatim dialogue history
- Rounds 4+: summarize prior rounds into a "story so far" to manage token limits
- If a `codex exec` call fails due to prompt length, summarize and retry once

### Step 4: Convergence

Stop deliberating and present to the user when:
- Both models agree on the top recommendation
- Arguments are repeating — no new information is being surfaced
- The core trade-off is clearly framed and further rounds won't resolve it
- Minimum: 1 round (if Codex agrees and adds nothing new, that's sufficient)
- **Maximum: 7 rounds.** After 7, present current state as genuine disagreement.

Show the user: "Converging on a recommendation..."

### Handling disagreement

**Default:** Frame the core tension — what the disagreement is really about, what trade-off the user is choosing between, why reasonable models disagree. This is the most useful output: not two opinions, but a clear framing of the actual decision.

**Escalation:** If the disagreement is too nuanced for a clean framing, run one final head-to-head exchange where each side directly addresses the other's strongest argument. Present both final statements to the user.

### Error handling

- **Codex fails mid-deliberation:** Present what you have so far. Explain Codex became unavailable. Offer your own solo analysis for remaining points. The deliberation requirement is satisfied if at least one successful round completed.
- **Codex returns empty/nonsensical response:** Retry once. If still bad, treat as failure (see above).
- **Codex refuses the prompt:** Present the refusal to the user and continue with solo analysis.

---

## Phase 3: Presentation + Feedback Loop

### Present the recommendation

Present the joint conclusion to the user:
- **Recommendation**: The approach and reasoning
- **Key deliberation points**: The strongest arguments that shaped the conclusion
- **Caveats**: Any dissenting views or unresolved tensions

If Claude and Codex disagreed, present it as an analyzed disagreement (see "Handling disagreement" above).

### Feedback loop

The user may critique the recommendation ("I don't like X", "what about Y?", "you missed Z").

When the user provides feedback:
1. Write a **new brief** incorporating:
   - The original problem context
   - A summary of the previous deliberation and its conclusion
   - The user's specific feedback and concerns
2. Run another Claude <-> Codex deliberation round (Phase 2)
3. Present the updated recommendation

Repeat until the user is satisfied.

---

## Phase 4: Converge + Transition

When the user approves the recommendation:

### Summarize

Present a clear summary:
- **Decision**: What was decided
- **Key reasoning**: The 2-3 strongest arguments that led here
- **Risks acknowledged**: What could go wrong and any mitigations discussed
- **Open questions**: Anything left unresolved

### Save (optional)

If the decision is significant enough to document, offer to save it:

> "Want me to save this as a decision doc?"

If yes, save to `docs/decisions/YYYY-MM-DD-<topic>.md` with this structure:

```markdown
# Decision: <topic>

**Date**: YYYY-MM-DD
**Status**: Decided

## Context
<what prompted the decision>

## Decision
<what was decided>

## Reasoning
<key arguments>

## Risks
<acknowledged risks and mitigations>

## Open Questions
<anything unresolved>
```

### Suggest next step

Based on what was decided, suggest the logical next skill:

- Implementation work → "Ready to plan? I can invoke `superpowers:writing-plans`."
- Bug fix → "Ready to investigate? I can invoke `superpowers:systematic-debugging`."
- Architecture / design → "Ready to plan the implementation? I can invoke `superpowers:writing-plans`."
- Non-technical or no clear next skill → End with the summary. Not every decision needs a follow-up skill.

Offer to invoke the suggested skill. Don't invoke automatically.

---

## Examples

### Example 1: Architecture Decision

**User**: `/think should I use a monorepo or polyrepo for my new microservices project?`

**Claude**: "Before we get into repo structure — how many services are you starting with, and how many people will be working on them?"

*(several rounds of Socratic dialogue about team size, deployment strategy, shared code, CI/CD preferences)*

**User**: "Okay, figure it out"

**Claude**: "Sending brief to Codex..."

*(Claude sends brief WITHOUT the user's lean — just problem, constraints, trade-offs)*

*(2-3 rounds of Claude <-> Codex deliberation)*

**Claude**: "Converging on a recommendation..."

**Claude**: "Here's what we recommend: **Monorepo with Nx/Turborepo.** Your services share auth, logging, and data access — that's 60% common code. Independent deployment is achievable through proper CI pipeline configuration. Codex and I both converged on this, though Codex specifically flagged that you should plan your CI cache strategy early to avoid slow builds at scale."

**User**: "What about using git submodules instead?"

*(Claude incorporates feedback, runs another deliberation round with Codex)*

**Claude**: Presents updated recommendation addressing submodules.

**User**: "Okay, let's go with the monorepo approach"

**Claude**: Summarizes decision, reasoning, risks. Offers to save decision doc and suggests `writing-plans`.

### Example 2: Genuine Disagreement

**User**: `/think should we build our own auth system or use Auth0?`

*(Socratic dialogue, then deliberation)*

**Claude**: "Codex and I couldn't fully agree on this one, and that's actually informative. Here's the core tension:

**Claude's position**: Build custom auth. Your compliance requirements (SOC2 + HIPAA) mean you'll end up customizing Auth0 so heavily that you lose the 'managed service' benefit. Better to own it from day one.

**Codex's position**: Use Auth0. The compliance argument cuts the other way — Auth0 has pre-built compliance tooling that would take months to replicate. The customization overhead is real but bounded.

**What this comes down to**: How much do you trust a vendor's compliance tooling vs. your team's ability to build compliant auth from scratch? This is a bet on your team's security engineering depth."

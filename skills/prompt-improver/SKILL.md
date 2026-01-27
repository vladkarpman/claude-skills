---
name: prompt-improver
description: Use when user asks to improve, optimize, or create a prompt, mentions "prompt engineering", or wants feedback on a prompt they wrote.
---

# Prompt Improver

Help users create effective prompts using a 6-component framework. Work interactively — either improving an existing prompt or building one from scratch.

## The Framework

Every effective prompt has 6 components. Use this framework for analysis and construction:

| # | Component | Purpose | Question to Ask |
|---|-----------|---------|-----------------|
| 1 | **Persona** | Who should the AI be? | "What expert role should handle this task?" |
| 2 | **Task** | What needs to be done? | "What specific action or output do you need?" |
| 3 | **Steps** | How should it be done? | "What steps should be followed to complete this?" |
| 4 | **Context** | What background/constraints? | "What context, rules, or limitations apply?" |
| 5 | **Goal** | What does success look like? | "How will you know the output is correct?" |
| 6 | **Format** | How should output be structured? | "What format do you want the response in?" |

## Mode Selection

When the user invokes this skill, determine the mode:

**Improve Mode** — User provides an existing prompt to enhance
- Trigger: User shares a prompt and asks to improve/optimize it
- Action: Analyze against framework → Show gaps → Ask about missing parts → Generate improved version

**Build Mode** — User wants to create a prompt from scratch
- Trigger: User asks to "create", "write", or "build" a prompt, or says they need help making one
- Action: Ask questions for each component → Build prompt step by step

If unclear, ask: "Do you have a prompt you'd like to improve, or shall we build one from scratch?"

---

## Mode 1: Improve Mode

### Step 1: Analyze Against Framework

When the user provides a prompt, analyze each component and present:

```
┌─────────────────────────────────────────────────────────────┐
│  📋 FRAMEWORK ANALYSIS                                      │
├─────────────┬────────┬──────────────────────────────────────┤
│ Component   │ Status │ Found                                │
├─────────────┼────────┼──────────────────────────────────────┤
│ Persona     │ ✅/⚠️/❌ │ [what you found or "missing"]        │
│ Task        │ ✅/⚠️/❌ │ [what you found or "missing"]        │
│ Steps       │ ✅/⚠️/➖ │ [found, "missing", or "not needed"]  │
│ Context     │ ✅/⚠️/❌ │ [what you found or "missing"]        │
│ Goal        │ ✅/⚠️/❌ │ [what you found or "missing"]        │
│ Format      │ ✅/⚠️/❌ │ [what you found or "missing"]        │
└─────────────┴────────┴──────────────────────────────────────┘

Legend: ✅ Clear  ⚠️ Vague  ❌ Missing  ➖ Not needed
```

### Step 2: Assess Complexity

Determine what components are actually needed:

**Simple tasks** (factual questions, single actions):
- Only need: Task + Format (maybe Goal)
- Skip: Persona, Steps, detailed Context

**Medium tasks** (content creation, analysis):
- Need: Persona + Task + Context + Goal + Format
- Steps optional unless multi-part

**Complex tasks** (multi-step processes, technical work):
- Need all 6 components
- Steps are essential

Mark unneeded components as ➖ in the analysis.

### Step 3: Fill Gaps Step-by-Step

For components marked ⚠️ or ❌, ask ONE question at a time. Wait for each answer before asking the next. Closely related aspects (e.g., "language + library" for code) can be grouped into one question.

**Suggest answers when possible:** Use available context to propose a reasonable default. Example: "What programming language? Based on your task, Python with `smtplib` would work well — or did you have something else in mind?"

**Priority order:** Task → Goal → Context → Persona → Format → Steps

Only ask about Steps if the task is complex enough to need them.

**Flow:**
1. Show the Framework Analysis
2. Ask first question about highest-priority gap
3. Wait for answer
4. Ask next question (if needed)
5. Continue until gaps are filled
6. Generate improved prompt

### Step 4: Generate Improved Prompt

Once you have enough information, generate the improved prompt. Only include components that are relevant:

```
┌─────────────────────────────────────────────────────────────┐
│  ✨ IMPROVED PROMPT                                         │
└─────────────────────────────────────────────────────────────┘

You are [PERSONA - if needed].

[TASK - always include]

**Steps:** (only for complex tasks)
1. [Step 1]
2. [Step 2]
...

**Context:**
- [Constraint or background info]
- [Another constraint]

**Goal:**
[Success criteria - what good output looks like]

**Format:**
[Output structure requirements]
```

### Step 5: Show What Changed

After presenting the improved prompt, add a brief summary:

```
┌─────────────────────────────────────────────────────────────┐
│  📝 WHAT CHANGED                                            │
├─────────────────────────────────────────────────────────────┤
│  + Added: [components that were missing]                    │
│  ↑ Clarified: [components that were vague]                  │
│  → Why: [brief explanation of how this helps]               │
└─────────────────────────────────────────────────────────────┘
```

---

## Mode 2: Build Mode

Guide the user through creating a prompt by asking about each component. Adapt questions based on complexity.

### The Build Flow

Ask ONE question at a time. Wait for the user's response before proceeding. Closely related aspects can be grouped. When possible, suggest an answer based on context you already have.

```
┌──────────────────────────────────────────────────────────┐
│  ❶ TASK (always ask)                                     │
│                                                          │
│  "What do you need the AI to do?                         │
│   Describe the task or output you're looking for."       │
└──────────────────────────────────────────────────────────┘
```
After this answer, assess complexity:
- **Simple** → Ask only Goal and Format
- **Medium** → Ask Goal, Persona, Context, Format
- **Complex** → Ask all including Steps

```
┌──────────────────────────────────────────────────────────┐
│  ❷ GOAL (always ask)                                     │
│                                                          │
│  "What does a successful result look like?               │
│   How will you know the output is good?"                 │
└──────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────┐
│  ❸ PERSONA (skip for simple)                             │
│                                                          │
│  "What kind of expert should handle this?                │
│   I'd suggest [X] based on your task — sound good?"      │
└──────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────┐
│  ❹ CONTEXT (skip for simple)                             │
│                                                          │
│  "Any context or constraints?                            │
│   • Background information?                              │
│   • Things to include or avoid?"                         │
└──────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────┐
│  ❺ STEPS (only for complex tasks)                        │
│                                                          │
│  "Should the AI follow specific steps?                   │
│   If yes, what are they? Or should it decide?"           │
└──────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────┐
│  ❻ FORMAT (always ask, can be brief)                     │
│                                                          │
│  "How should the output be formatted?                    │
│   (e.g., paragraphs, bullet points, code, table)"        │
└──────────────────────────────────────────────────────────┘
```

### Smart Skipping

Don't ask unnecessary questions:
- If user says "just a quick question" → Skip Persona, Context, Steps
- If task is obviously simple → Skip to Format after Goal
- If user seems impatient → Offer to generate with defaults and refine after

### Generate the Prompt

After gathering components, generate the prompt. Only include relevant sections — don't pad simple prompts with unnecessary structure.

---

## Output Options

| Option | Description |
|--------|-------------|
| **Single** (default) | One optimized prompt |
| **Variations** | 2-3 versions: concise, structured (XML), conversational |
| **Detailed** | Prompt + explanation of each component |

---

## Examples

See [examples.md](examples.md) for before/after transformations across coding, writing, analysis, and creative domains.

---

## Final Notes

- Always preserve the user's original intent
- Don't over-engineer simple requests
- Ask clarifying questions rather than assuming
- If a prompt is already good, say so — don't change for the sake of change
- Match complexity of improvement to complexity of task

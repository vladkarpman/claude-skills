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

When the user provides a prompt, analyze each component and present using this format:

## 📋 FRAMEWORK ANALYSIS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**Persona**   ❌  missing
**Task**      ⚠️  *"[quoted from prompt]"* — too broad/vague
**Steps**     ➖  not needed for this task
**Context**   ❌  missing — [what's unclear]
**Goal**      ❌  missing success criteria
**Format**    ❌  missing output expectations

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
`✅ Clear` · `⚠️ Vague` · `❌ Missing` · `➖ Not needed`

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

For components marked ⚠️ or ❌, ask ONE question at a time using this format:

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📍 *Question 1 of N*
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## ❓ [COMPONENT NAME]

**[Main question in bold]**

> 💡 *[Context or suggestion based on what you know]*

Choose one:
  `a` **[Option A]** — [brief description]
  `b` **[Option B]** — [brief description]
  `c` **[Option C]** — [brief description]

*Or describe in your own words.*

---

**Guidelines:**
- Wait for each answer before asking the next
- Closely related aspects (e.g., "language + library" for code) can be grouped into one question
- **Suggest answers when possible:** Use available context to propose a reasonable default
- **Priority order:** Task → Goal → Context → Persona → Format → Steps
- Only ask about Steps if the task is complex enough to need them

### Step 4: Generate Improved Prompt

Once you have enough information, generate the improved prompt using this format:

## ✨ IMPROVED PROMPT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
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

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
*📋 Copy the prompt above and use it with any AI assistant.*

### Step 5: Show What Changed

After presenting the improved prompt, add this summary:

## 📝 WHAT CHANGED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| | |
|---|---|
| ✅ **Added** | [components that were missing] |
| 🔧 **Clarified** | [components that were vague — what changed] |
| 💡 **Why better** | [brief explanation of how this helps the AI] |

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎉 *Done! Your prompt is ready to use.*
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

---

## Mode 2: Build Mode

Guide the user through creating a prompt by asking about each component. Adapt questions based on complexity.

### The Build Flow

Ask ONE question at a time using the formatted question style. Wait for the user's response before proceeding.

**Question 1: TASK** (always ask)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📍 *Question 1 of N*
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## ❓ TASK

**What do you need the AI to do?**

Describe the task or output you're looking for.

*Examples: "Write a marketing email", "Debug this code", "Explain quantum computing"*

---

After this answer, assess complexity:
- **Simple** → Ask only Goal and Format (2-3 questions total)
- **Medium** → Ask Goal, Persona, Context, Format (4-5 questions total)
- **Complex** → Ask all including Steps (5-6 questions total)

Update the "Question X of N" indicator based on assessed complexity.

**Question 2: GOAL** (always ask)

## ❓ GOAL

**What does a successful result look like?**

> 💡 *Based on your task, I'm thinking [suggested criteria].*

How will you know the output is good?

---

**Question 3: PERSONA** (skip for simple)

## ❓ PERSONA

**What kind of expert should handle this?**

> 💡 *I'd suggest **[suggested persona]** based on your task.*

Choose one:
  `a` **[Suggested persona]** — [why it fits]
  `b` **[Alternative]** — [different angle]
  `c` **General assistant** — no specific expertise needed

*Or specify a different expert role.*

---

**Question 4: CONTEXT** (skip for simple)

## ❓ CONTEXT

**Any context or constraints?**

Consider:
- Background information the AI should know?
- Things to include or avoid?
- Tone, audience, or style requirements?

*Skip if none — just say "none" or "skip".*

---

**Question 5: STEPS** (only for complex tasks)

## ❓ STEPS

**Should the AI follow specific steps?**

> 💡 *For this task, I'd suggest: [proposed steps]*

Choose one:
  `a` **Use suggested steps** — [brief summary]
  `b` **Let AI decide** — no specific order needed
  `c` **Custom steps** — I'll specify

---

**Question 6: FORMAT** (always ask, can be brief)

## ❓ FORMAT

**How should the output be formatted?**

Choose one:
  `a` **Paragraphs** — flowing prose
  `b` **Bullet points** — scannable list
  `c` **Code** — with syntax highlighting
  `d` **Table** — structured data
  `e` **Mixed** — whatever fits best

---

### Smart Skipping

Don't ask unnecessary questions:
- If user says "just a quick question" → Skip Persona, Context, Steps
- If task is obviously simple → Skip to Format after Goal
- If user seems impatient → Offer to generate with defaults and refine after

### Generate the Prompt

After gathering components, generate the prompt using the same format as Improve Mode (Step 4), followed by the completion celebration.

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

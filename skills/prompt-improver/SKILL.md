---
name: prompt-improver
description: |
  TRIGGERS — Use this skill IMMEDIATELY when user:
  • Types "improve-prompt", "improve prompt", or "/improve-prompt" → use Improve Mode
  • Types "build-prompt", "build prompt", or "/build-prompt" → use Build Mode
  • Types "create a prompt", "write a prompt", or "help me write a prompt" → use Build Mode
  • Types "analyze this prompt", "analyze my prompt", or "review this prompt" → use Improve Mode
  • Types "make this prompt better" or "fix this prompt" → use Improve Mode
  • Starts message with any of the above followed by text

  PATTERN — Identify the prompt to improve (Improve Mode only):
  • Text in "quotes" after the trigger phrase
  • Text in [square brackets] after the trigger phrase
  • Text in ```code blocks``` after the trigger phrase
  • If no quotes/brackets/blocks, treat everything after the trigger as the prompt

  ALSO USE when user:
  • Asks to improve, optimize, or refine a prompt → Improve Mode
  • Asks to create, build, or write a new prompt → Build Mode
  • Mentions "prompt engineering" or "6-component framework"
  • Wants feedback on a prompt they wrote → Improve Mode

  DO NOT USE when user wants help writing content (emails, documents) —
  only use for prompts intended for AI assistants.
---

# Prompt Improver

Help users create effective prompts using a 6-component framework. Work interactively — either improving an existing prompt or building one from scratch.

## The Framework

Every effective prompt has 6 components:

| # | Component | Purpose |
|---|-----------|---------|
| 1 | **Persona** | Who should the AI be? |
| 2 | **Task** | What needs to be done? |
| 3 | **Steps** | How should it be done? |
| 4 | **Context** | What background/constraints? |
| 5 | **Goal** | What does success look like? |
| 6 | **Format** | How should output be structured? |

## Mode Selection

**Improve Mode** — User provides an existing prompt to enhance

**Build Mode** — User wants to create a prompt from scratch

If unclear, ask: *"Do you have a prompt you'd like to improve, or shall we build one from scratch?"*

---

## Mode 1: Improve Mode

### Step 1: Analyze Against Framework

Present your analysis using this format:

### 📋 Framework Analysis

| Component | Status | Notes |
|:----------|:------:|:------|
| Persona | ❌ | missing |
| Task | 🟡 | too vague, needs specifics |
| Steps | ➖ | not needed |
| Context | ❌ | missing |
| Goal | ❌ | missing |
| Format | ❌ | missing |

**Legend:** ✅ Clear · 🟡 Vague · ❌ Missing · ➖ Not needed

---

### Step 2: Assess Complexity

- **Simple** (factual questions) → Only need: Task + Format
- **Medium** (content creation) → Need: Persona + Task + Context + Goal + Format
- **Complex** (multi-step work) → Need all 6 components

### Step 3: Fill Gaps

Ask ONE question at a time. Use this format:

---

### 📍 Question 1 of N

## ❓ GOAL

**What does success look like for this task?**

> 💡 *Based on [context], I'm guessing you want [suggestion].*

**Choose one:**
- `a` **[Option A]** — description
- `b` **[Option B]** — description
- `c` **[Option C]** — description

*Or describe in your own words.*

---

**Guidelines:**
- Wait for each answer before asking the next
- **Suggest answers** based on available context
- **Priority order:** Task → Goal → Context → Persona → Format → Steps

### Step 4: Generate Improved Prompt

---

### ✨ Improved Prompt

```
You are [PERSONA].

[TASK]

**Context:**
- [constraint 1]
- [constraint 2]

**Goal:**
[success criteria]

**Format:**
[output structure]
```

> 📋 *Copy and use with any AI assistant.*

---

### Step 5: Show What Changed

---

### 📝 What Changed

**✅ Added:**
- [component] — [brief note]
- [component] — [brief note]

**🔧 Clarified:**
- [what changed] — before → after

**💡 Why this is better:**
[brief explanation]

---

### 🎉 Done!

Your prompt is ready to use.

---

## Mode 2: Build Mode

Ask ONE question at a time. Adapt based on complexity.

### Question Flow

**❶ TASK** *(always ask)*

> **What do you need the AI to do?**
>
> Describe the task or output you're looking for.

After this, assess complexity:
- **Simple** → 2-3 questions (Task, Goal, Format)
- **Medium** → 4-5 questions (add Persona, Context)
- **Complex** → 5-6 questions (add Steps)

**❷ GOAL** *(always ask)*

> **What does a successful result look like?**
>
> 💡 *Based on your task, I'd suggest: [criteria]*

**❸ PERSONA** *(skip for simple)*

> **What kind of expert should handle this?**
>
> 💡 *I'd suggest **[persona]** — [why]*
>
> Choose:
> - `a` [Suggested persona]
> - `b` [Alternative]
> - `c` General assistant

**❹ CONTEXT** *(skip for simple)*

> **Any context or constraints?**
>
> - Background info?
> - Things to include/avoid?
> - Tone or audience?

**❺ STEPS** *(only for complex)*

> **Should the AI follow specific steps?**
>
> 💡 *I'd suggest: [steps]*
>
> Choose:
> - `a` Use suggested steps
> - `b` Let AI decide
> - `c` I'll specify custom steps

**❻ FORMAT** *(always ask)*

> **How should the output be formatted?**
>
> - `a` Paragraphs
> - `b` Bullet points
> - `c` Code
> - `d` Table
> - `e` Mixed

### Generate the Prompt

Use the same format as Improve Mode (Step 4), then show the completion message.

---

## Final Notes

- Preserve the user's original intent
- Don't over-engineer simple requests
- If a prompt is already good, say so
- Match complexity of improvement to complexity of task

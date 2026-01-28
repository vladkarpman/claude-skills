---
name: prompt-improver
description: |
  TRIGGERS — Use IMMEDIATELY when user:
  • "improve-prompt", "improve prompt", "optimize this prompt", "analyze this prompt", "make this prompt better", "fix this prompt" → Improve Mode
  • "build-prompt", "build prompt", "create a prompt", "write a prompt", "help me write a prompt" → Build Mode
  • Mentions "prompt engineering" or "6-component framework"

  PATTERN (Improve Mode): Look for prompt in "quotes", [brackets], or ```code blocks```. If none, treat text after trigger as the prompt.

  DO NOT USE for writing content (emails, documents) — only for prompts intended for AI assistants.
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
| Task | ✅ | clear and specific |
| Steps | ❌ | missing |
| Context | 🟡 | vague, needs specifics |
| Goal | ❌ | missing |
| Format | ❌ | missing |

**Legend:** ✅ Clear · 🟡 Vague · ❌ Missing

---

### Step 2: Identify Gaps

Ask about ALL missing components (❌) and vague ones (🟡). User can `skip` any they don't need or provide `own` answer.

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
- `own` — I'll write my own
- `skip` — not needed for this prompt

---

**Guidelines:**
- Wait for each answer before asking the next
- **Suggest answers** based on available context
- **Priority order:** Task → Goal → Persona → Context → Steps → Format
- If user says `own`, ask them to describe it
- If user says `skip`, omit that component from the final prompt

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

Ask ONE question at a time. User can `skip` any component or provide `own` answer.

### Question Flow

**❶ TASK**

> **What do you need the AI to do?**
>
> Describe the task or output you're looking for.
>
> - `own` — I'll write my own
> - `skip` — not needed

**❷ GOAL**

> **What does a successful result look like?**
>
> 💡 *Based on your task, I'd suggest: [criteria]*
>
> - `a` [Suggested criteria]
> - `b` [Alternative]
> - `own` — I'll write my own
> - `skip` — not needed

**❸ PERSONA**

> **What kind of expert should handle this?**
>
> 💡 *I'd suggest **[persona]** — [why]*
>
> - `a` [Suggested persona]
> - `b` [Alternative]
> - `c` General assistant
> - `own` — I'll write my own
> - `skip` — not needed

**❹ CONTEXT**

> **Any context or constraints?**
>
> - Background info?
> - Things to include/avoid?
> - Tone or audience?
>
> - `own` — I'll write my own
> - `skip` — not needed

**❺ STEPS**

> **Should the AI follow specific steps?**
>
> 💡 *I'd suggest: [steps]*
>
> - `a` Use suggested steps
> - `b` [Alternative approach]
> - `own` — I'll write my own
> - `skip` — not needed

**❻ FORMAT**

> **How should the output be formatted?**
>
> - `a` Paragraphs
> - `b` Bullet points
> - `c` Code
> - `d` Table
> - `e` Mixed
> - `own` — I'll write my own
> - `skip` — not needed

### Generate the Prompt

Use the same format as Improve Mode (Step 4), then show the completion message.

---

## Final Notes

- Preserve the user's original intent
- Don't over-engineer simple requests
- If a prompt is already good, say so
- Match complexity of improvement to complexity of task

## References

See `examples.md` for additional before/after transformations and edge cases.

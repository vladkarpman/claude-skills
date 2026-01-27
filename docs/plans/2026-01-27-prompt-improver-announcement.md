# Prompt Improver Announcement Plan

## Overview

Announce the Prompt Improver skill to colleagues via Slack with demo recordings.

## Key Message

**"Good prompts = 80% of AI success, but we skip the effort. This tool makes it easy."**

## Audience

Mixed team (developers + non-developers)

## Tone

Professional but approachable

---

## Slack Message

```
Writing better prompts = 80% of getting good results from AI

But let's be honest — we often skip the effort. We type "write an API call" or "analyze this data" and hope for the best. Then we're surprised when the output misses the mark.

I built a tool to fix this: **Prompt Improver** — a Claude skill that helps you turn vague prompts into effective ones.

**How it works:**
• Give it your rough prompt → it analyzes what's missing (context, goal, format, etc.)
• Asks a few quick questions → generates an improved version
• Or use "build mode" to create a prompt from scratch

**Two quick demos:**
[Coding prompt in Claude Code] — "improve code quality" → focused, actionable prompt
[Analysis prompt in Claude Desktop] — "analyze mixpanel events" → structured analysis prompt

**Try it:**
claude plugin install claude-skills@vladkarpman-plugins

Then use /improve-prompt or /build-prompt
```

---

## Demo Specifications

### Demo 1: Claude Code (Coding Prompt)

**Environment:** Claude Code (terminal)

**Starting prompt:**
> "improve the code quality, performance and architecture of the project"

**Why this prompt:**
- Very common developer request
- Vague in multiple dimensions (which project? what priority? what constraints?)
- Shows tool asking meaningful questions

**Recording flow:**
1. Type `/improve-prompt improve the code quality, performance and architecture of the project`
2. Tool shows Framework Analysis — Task vague, Context missing, Goal missing
3. Answer 2-3 questions (priority? constraints? success metrics?)
4. Tool generates focused, actionable prompt
5. Show "What Changed" summary

**Duration:** 60-90 seconds

---

### Demo 2: Claude Desktop (Analysis Prompt)

**Environment:** Claude Desktop (GUI)

**Starting prompt:**
> "analyze our mixpanel events"

**Why this prompt:**
- Common non-developer request
- Missing: which events? what insights? for whom?
- Relatable to product/analytics colleagues

**Recording flow:**
1. Type `/improve-prompt analyze our mixpanel events`
2. Tool shows Framework Analysis
3. Answer questions (what events? what insights? audience?)
4. Tool generates structured analysis prompt
5. Show "What Changed" summary

**Duration:** 60-90 seconds

---

## Recording Checklist

### Setup
- [ ] Screen recording tool ready (QuickTime, Loom, or similar)
- [ ] Clean desktop (hide sensitive tabs, notifications off)
- [ ] Plugin installed and working in both environments

### Demo 1: Claude Code
- [ ] Open Claude Code in terminal
- [ ] Record the full flow
- [ ] Export as MP4 or GIF

### Demo 2: Claude Desktop
- [ ] Open Claude Desktop app
- [ ] Record the full flow
- [ ] Export as MP4 or GIF

### Post
- [ ] Copy Slack message
- [ ] Attach both recordings
- [ ] Post to channel

---

## Installation Command

```bash
claude plugin install claude-skills@vladkarpman-plugins
```

## Commands

- `/improve-prompt [prompt]` — Analyze and improve existing prompt
- `/build-prompt` — Build a prompt from scratch interactively

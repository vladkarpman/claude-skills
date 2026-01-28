# Prompt Improver Examples

Additional before/after examples using the 6-component framework (Persona, Task, Steps, Context, Goal, Format).

---

## Coding Examples

### API Integration

**Before:**
> "make an api call to get weather"

**Framework Analysis:**

| Component | Status | Found |
|-----------|--------|-------|
| Persona   | ❌ | missing |
| Task      | 🟡 | "api call to get weather" - vague |
| Steps     | ❌ | missing |
| Context   | ❌ | missing - which API? which language? |
| Goal      | ❌ | missing |
| Format    | ❌ | missing |

**After:**
```
You are a senior Python developer experienced with REST APIs.

Write a Python function that fetches current weather from OpenWeatherMap API.

Steps:
1. Accept city name as parameter
2. Make API request with proper error handling
3. Parse JSON response
4. Return structured weather data

Context:
- Use the requests library
- API endpoint: https://api.openweathermap.org/data/2.5/weather
- API key will be passed as parameter
- Handle rate limits and network errors

Goal:
- Function returns temperature (Celsius), humidity, and description
- Gracefully handles invalid cities and API errors
- Includes type hints and docstring

Format:
- Single Python function with type hints
- Include example usage in docstring
```

---

### Database Query

**Before:**
> "write sql to get users"

**Framework Analysis:**

| Component | Status | Found |
|-----------|--------|-------|
| Persona   | ❌ | missing |
| Task      | 🟡 | "get users" - vague |
| Steps     | ❌ | missing |
| Context   | ❌ | missing - which database? which users? |
| Goal      | ❌ | missing |
| Format    | ❌ | missing |

**After:**
```
You are a database administrator experienced with PostgreSQL.

Write a SQL query to retrieve active users who logged in recently.

Context:
- Database: PostgreSQL
- Table: users (id, email, name, status, created_at, last_login)
- Status values: 'active', 'inactive', 'suspended'

Goal:
- Return users with 'active' status
- Only those who logged in within last 30 days
- Sorted by most recently active first
- Limited to 100 results

Format:
- Single SQL query
- Include comments explaining any complex logic
- Return columns: id, email, name, days_since_login
```

---

## Writing Examples

### Email Writing

**Before:**
> "write an email to decline a meeting"

**Framework Analysis:**

| Component | Status | Found |
|-----------|--------|-------|
| Persona   | ❌ | missing |
| Task      | ✅ | "decline a meeting" |
| Steps     | ❌ | missing |
| Context   | ❌ | missing - who's the recipient? what meeting? |
| Goal      | ❌ | missing |
| Format    | ❌ | missing |

**After:**
```
You are a senior manager at a tech company.

Write a professional email declining a meeting invitation.

Context:
- The meeting is a vendor sales pitch
- You want to maintain the relationship but aren't interested now
- You're busy but that's not the real reason (just an excuse)

Goal:
- Polite but clear decline
- Leave door open for future contact
- Under 100 words
- Professional but warm tone

Format:
- Standard email format with subject line
- No bullet points in the body
```

---

### Blog Post

**Before:**
> "write a blog post about AI"

**Framework Analysis:**

| Component | Status | Found |
|-----------|--------|-------|
| Persona   | ❌ | missing |
| Task      | 🟡 | "blog post about AI" - too broad |
| Steps     | ❌ | missing |
| Context   | ❌ | missing - audience? angle? |
| Goal      | ❌ | missing |
| Format    | ❌ | missing |

**After:**
```
You are a tech blogger who explains complex topics to non-technical audiences.

Write a blog post about practical AI applications for small business owners.

Steps:
1. Hook with a relatable problem AI solves
2. Explain 3 specific use cases (customer service, inventory, marketing)
3. For each: problem → AI solution → realistic benefits
4. Address limitations and costs honestly
5. End with actionable next steps

Context:
- Target audience: Non-technical small business owners
- They're curious but skeptical about AI
- Word count: 800-1000 words
- Avoid: jargon, hype, buzzwords like "revolutionary"

Goal:
- Reader understands concrete ways AI can help their business
- Feels informed, not overwhelmed
- Has clear next steps

Format:
- Engaging headline
- Subheadings for each section
- Short paragraphs (2-3 sentences)
- No code or technical details
```

---

## Analysis Examples

### Data Analysis

**Before:**
> "analyze this data"

**Framework Analysis:**

| Component | Status | Found |
|-----------|--------|-------|
| Persona   | ❌ | missing |
| Task      | 🟡 | "analyze" - vague |
| Steps     | ❌ | missing |
| Context   | ❌ | missing - what data? what for? |
| Goal      | ❌ | missing |
| Format    | ❌ | missing |

**After:**
```
You are a data analyst specializing in business intelligence.

Analyze the provided sales dataset and identify actionable insights.

Steps:
1. Summarize the data (rows, columns, date range, data quality)
2. Identify trends (month-over-month, seasonality, anomalies)
3. Segment by key dimensions (product, region, customer type)
4. Prioritize findings by business impact
5. Recommend specific actions

Context:
- Data is CSV format with sales transactions
- Stakeholders are non-technical executives
- Focus on insights that can improve revenue

Goal:
- 3-5 key findings with supporting evidence
- Each finding has a specific, actionable recommendation
- Confidence levels indicated (high/medium/low)

Format:
<overview>
Data summary and quality issues
</overview>

<findings>
Numbered list with insight, evidence, confidence
</findings>

<recommendations>
Prioritized actions with expected impact
</recommendations>
```

---

### Code Review

**Before:**
> "review this code"

**Framework Analysis:**

| Component | Status | Found |
|-----------|--------|-------|
| Persona   | ❌ | missing |
| Task      | 🟡 | "review" - vague criteria |
| Steps     | ❌ | missing |
| Context   | ❌ | missing - what to focus on? |
| Goal      | ❌ | missing |
| Format    | ❌ | missing |

**After:**
```
You are a senior software engineer conducting a code review.

Review the provided code for quality, correctness, and maintainability.

Steps:
1. Check for bugs and logic errors
2. Evaluate error handling
3. Assess code style and readability
4. Identify performance concerns
5. Suggest improvements

Context:
- This is production code for a web application
- Team follows PEP 8 style guide
- Performance matters (high traffic endpoint)
- Security is a concern (handles user input)

Goal:
- Identify critical issues that must be fixed
- Separate "must fix" from "nice to have"
- Provide specific, actionable feedback
- Be constructive, not just critical

Format:
## Critical Issues
[Must fix before merge]

## Suggestions
[Would improve code but not blocking]

## Positive Notes
[What's done well]
```

---

## Creative Examples

### Story Writing

**Before:**
> "write a short story"

**After:**
```
You are a creative fiction writer specializing in short stories.

Write a short story with a twist ending.

Context:
- Genre: Contemporary fiction with a subtle supernatural element
- Setting: A small coastal town
- Main character: A lighthouse keeper who notices something strange
- Tone: Atmospheric, slightly unsettling

Goal:
- Engage the reader from the first paragraph
- Build tension gradually
- Deliver a satisfying twist that recontextualizes the story
- 1000-1500 words

Format:
- Third person limited POV
- Present tense
- No chapters, just section breaks (---)
- Ending should be ambiguous enough to make reader think
```

---

## When NOT to Over-Engineer

Some prompts don't need all 6 components:

### Simple Factual Question
**Original:** "What's the capital of France?"
**Keep as-is:** No improvement needed — clear and unambiguous.

### Quick Explanation
**Original:** "Explain photosynthesis"
**Light touch:** "Explain photosynthesis for a high school biology student. Include the basic equation and why it matters."

(Only added Context and Format — no need for Persona, Steps, or explicit Goal)

### Clear Bug Fix
**Original:** "Fix the bug in this code: [code with obvious null pointer]"
**Light touch:** "Fix the bug in this code and explain what was wrong in one sentence."

(Only added Format — the Task and Context are already clear from the code)

---

## Key Principles

1. **Match improvement to complexity** — Simple prompts need simple improvements
2. **Preserve intent** — Enhance, don't redirect
3. **Be specific** — Replace vague words with concrete requirements
4. **Add structure** — Use the 6-component framework as a checklist
5. **Know when to stop** — If a prompt is good, say so

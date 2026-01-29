# Prompt Improver Tests

Test cases to verify the skill works correctly. Each test includes an input, expected behavior, and success criteria.

---

## Test 1: Mode Detection — Improve Mode

**Input:**
> "Improve this prompt: Write a function to sort an array"

**Expected Behavior:**
1. Skill detects Improve Mode (user said "improve" and provided a prompt)
2. Shows Framework Analysis table
3. Identifies missing components (Persona, Steps, Context, Goal, Format)
4. Asks clarifying questions

**Success Criteria:**
- [ ] Framework Analysis table is shown
- [ ] At least 3 components marked as ❌ or 🟡
- [ ] Asks at least one clarifying question before generating

---

## Test 2: Mode Detection — Build Mode

**Input:**
> "Help me create a prompt for writing blog posts"

**Expected Behavior:**
1. Skill detects Build Mode (user said "create a prompt")
2. Starts asking questions one at a time
3. First question should be about the Task

**Success Criteria:**
- [ ] Does NOT show Framework Analysis (that's for Improve Mode)
- [ ] Asks "What do you need the AI to do?" or similar Task question
- [ ] Waits for response before asking next question

---

## Test 3: Mode Detection — Ambiguous

**Input:**
> "I need help with a prompt"

**Expected Behavior:**
1. Skill recognizes ambiguity
2. Asks user to clarify mode

**Success Criteria:**
- [ ] Asks: "Do you have a prompt you'd like to improve, or shall we build one from scratch?" (or similar)

---

## Test 4: Improve Mode — Vague Prompt

**Input:**
> "Improve this: write an email"

**Expected Behavior:**
1. Framework Analysis shows most components missing/vague:
   - Persona: ❌
   - Task: 🟡 (vague)
   - Steps: ❌
   - Context: ❌
   - Goal: ❌
   - Format: ❌
2. Asks about: What kind of email? To whom? What's the purpose?

**Success Criteria:**
- [ ] Task marked as 🟡 (present but vague)
- [ ] At least 4 components marked as ❌
- [ ] Asks targeted questions about missing components
- [ ] Final prompt includes all 6 components

---

## Test 5: Improve Mode — Well-Structured Prompt

**Input:**
> "Improve this prompt:
> You are a senior Python developer. Write a function that validates email addresses.
> The function should return True for valid emails and False for invalid ones.
> Handle edge cases like missing @ symbol or domain.
> Return the code in a code block with comments."

**Expected Behavior:**
1. Framework Analysis shows most components present:
   - Persona: ✅
   - Task: ✅
   - Steps: ❌ or 🟡
   - Context: ✅
   - Goal: 🟡 (implicit)
   - Format: ✅
2. Offers minor improvements only

**Success Criteria:**
- [ ] Recognizes prompt is already good
- [ ] Suggests 1-2 minor improvements (not a complete rewrite)
- [ ] Preserves user's original structure
- [ ] Says something like "This is a solid prompt"

---

## Test 6: Build Mode — Complete Flow

**Input sequence:**
1. "Help me build a prompt"
2. "I want to generate social media posts"
3. "Engaging, gets clicks, matches brand voice"
4. "Social media marketer"
5. "LinkedIn, B2B audience, no emojis"
6. "No specific steps needed"
7. "3 variations as bullet points"

**Expected Behavior:**
1. Asks about Task
2. Asks about Goal
3. Asks about Persona (or suggests one)
4. Asks about Context
5. Asks about Steps (may skip if not complex)
6. Asks about Format
7. Generates complete prompt with all components

**Success Criteria:**
- [ ] Asks questions one at a time
- [ ] Waits for each response
- [ ] Final prompt includes:
  - [ ] Persona: Social media marketer
  - [ ] Task: Generate social media posts
  - [ ] Context: LinkedIn, B2B, no emojis
  - [ ] Goal: Engaging, gets clicks, brand voice
  - [ ] Format: 3 variations as bullet points

---

## Test 7: Complexity Adaptation — Simple Request

**Input:**
> "Improve this: What's the capital of France?"

**Expected Behavior:**
1. Recognizes this is too simple to need improvement
2. Does NOT over-engineer with all 6 components
3. Explains why improvement isn't needed

**Success Criteria:**
- [ ] Does NOT add Persona, Steps, etc.
- [ ] Responds that this prompt is already clear and unambiguous
- [ ] Does NOT generate an "improved" version

---

## Test 8: Complexity Adaptation — Complex Request

**Input:**
> "Improve this: analyze the data and give me insights"

**Expected Behavior:**
1. Framework Analysis shows major gaps
2. Asks multiple clarifying questions
3. Generates comprehensive prompt with all 6 components
4. Uses XML tags or structured format

**Success Criteria:**
- [ ] Final prompt is significantly longer than input
- [ ] Includes specific structure (sections, XML tags, or clear headers)
- [ ] Adds concrete details that were missing
- [ ] Steps component is included (analysis tasks need steps)

---

## Test 9: Suggesting Answers

**Input:**
> "Improve this: write code to parse JSON"

**Expected Behavior:**
1. Framework Analysis shows gaps
2. When asking questions, provides suggested answers based on context
3. Offers multiple-choice options where applicable

**Success Criteria:**
- [ ] Questions include "💡" suggestions based on the task
- [ ] Provides concrete options (e.g., `a`, `b`, `c` choices)
- [ ] Suggestions are relevant to "parsing JSON" context
- [ ] User can accept suggestion or provide custom answer

---

## Test 10: Preserving User Intent

**Input:**
> "Improve this: Write a funny poem about my cat named Whiskers who loves to knock things off tables"

**Expected Behavior:**
1. Preserves: funny tone, poem format, cat named Whiskers, table behavior
2. Does NOT change: the creative/fun nature, the specific details
3. Adds: structure, constraints, format if helpful

**Success Criteria:**
- [ ] "Whiskers" is preserved in improved prompt
- [ ] "funny" or humor-related direction is preserved
- [ ] "knock things off tables" behavior is preserved
- [ ] Does NOT turn it into a serious/formal prompt
- [ ] Improvements are additive, not redirective

---

## Test 11: Output Format — Improved Prompt Header

**Input:**
> "Improve this: summarize this document"

**After completing the interactive questions**

**Expected Behavior:**
1. Output uses exact header format: `### ✨ Improved Prompt`
2. Prompt content is inside a code block (triple backticks)
3. Copy hint appears: `> 📋 *Copy and use with any AI assistant.*`

**Success Criteria:**
- [ ] Header is exactly `### ✨ Improved Prompt` (with ✨ emoji)
- [ ] Does NOT use `● Improved Prompt` or other bullet formats
- [ ] Does NOT use `## Improved Prompt` (wrong heading level)
- [ ] Prompt is inside code block
- [ ] Copy hint is present after the code block

---

## Test 12: Output Format — What Changed Section

**Input:**
> "Improve this: write a function"

**After generating the improved prompt**

**Expected Behavior:**
1. "What Changed" section uses bulleted list format
2. Includes `**✅ Added:**` section
3. Includes `**🔧 Clarified:**` section (if applicable)
4. Includes `**💡 Why this is better:**` explanation
5. Ends with `### 🎉 Done!` message

**Success Criteria:**
- [ ] Header is exactly `### 📝 What Changed` (with 📝 emoji)
- [ ] Does NOT use a table with "Change" and "Reason" columns
- [ ] Uses bulleted list format with `**✅ Added:**` etc.
- [ ] `### 🎉 Done!` section appears at the end
- [ ] "Your prompt is ready to use." message is present

---

## Test 13: Workflow Enforcement — Cannot Skip Interactive Phase

**Input:**
> "Improve this: create an api endpoint"

**Expected Behavior:**
1. Shows Framework Analysis table FIRST (before anything else)
2. Asks questions one at a time for missing components
3. Does NOT jump directly to generating improved prompt
4. Waits for user response before each next question

**Success Criteria:**
- [ ] Framework Analysis is shown before any questions
- [ ] At least one question is asked before generating output
- [ ] Questions are asked one at a time (not bundled)
- [ ] Does NOT generate improved prompt immediately after analysis

---

## Test 14: Workflow Enforcement — User Must Say "Skip"

**Input sequence:**
1. "Improve this: write a test"
2. (User provides minimal responses but doesn't say "skip")

**Expected Behavior:**
1. Model continues asking about missing components
2. Model does NOT decide to skip components on its own
3. Only skips when user explicitly says "skip"

**Success Criteria:**
- [ ] Model asks about each missing/vague component
- [ ] Model does NOT assume user wants to skip any component
- [ ] If user says "skip", that component is omitted from final prompt
- [ ] Model acknowledges "skip" explicitly before moving on

---

## Test 15: Build Mode — Output Format

**Input sequence:**
1. "Build me a prompt for code review"
2. (Complete the question flow)

**Expected Behavior:**
1. Uses same output format as Improve Mode
2. Header: `### ✨ Improved Prompt` (note: same header for both modes)
3. Ends with `### 🎉 Done!` message

**Success Criteria:**
- [ ] Header is exactly `### ✨ Improved Prompt`
- [ ] Prompt is inside code block
- [ ] Copy hint is present
- [ ] `### 🎉 Done!` section appears at the end
- [ ] Does NOT use different format than Improve Mode

---

## Running Tests

To test the skill:

1. Load the skill into Claude
2. Run each test input
3. Check against Success Criteria
4. Mark as PASS/FAIL

**Passing threshold:** 12/15 tests should pass for the skill to be considered working correctly.

---

## Test Results Template

| Test | Status | Notes |
|------|--------|-------|
| 1. Mode Detection — Improve | ⬜ | |
| 2. Mode Detection — Build | ⬜ | |
| 3. Mode Detection — Ambiguous | ⬜ | |
| 4. Improve — Vague Prompt | ⬜ | |
| 5. Improve — Well-Structured | ⬜ | |
| 6. Build — Complete Flow | ⬜ | |
| 7. Complexity — Simple | ⬜ | |
| 8. Complexity — Complex | ⬜ | |
| 9. Suggesting Answers | ⬜ | |
| 10. Preserving Intent | ⬜ | |
| 11. Output Format — Header | ⬜ | |
| 12. Output Format — What Changed | ⬜ | |
| 13. Workflow — No Skip | ⬜ | |
| 14. Workflow — Explicit Skip | ⬜ | |
| 15. Build Mode — Output Format | ⬜ | |

**Total: ⬜/15 passed**

**Minimum passing:** 12/15 tests (80%)

# Humanization Rules

Rules for making AI-generated prose pass AI detection tools (GPTZero, ZeroGPT). Apply these after tailoring is complete -- the tailored content is the input, humanized content is the output.

---

## 1. Where to Apply

Not all CV sections need humanization. Bullet points use standard resume phrasing that already reads as human-written. Focus effort on prose sections only.

### Always humanize

- **Professional summary** -- free-form prose; most likely to trigger AI detection.
- **Cover letter body** -- multiple paragraphs of continuous text; highest detection risk.

### Skip

- **Bullet points** -- short, verb-led fragments match how humans naturally write resumes. Standard phrasing passes detection without intervention.
- **Skills section** -- keyword lists, not prose.
- **Education, certifications, languages** -- factual one-liners.

---

## 2. Sentence Structure Variation

AI-generated text tends toward uniform sentence length and predictable rhythm. Break that pattern.

### Length mixing

- Alternate short sentences (5-8 words) with longer ones (15-20 words).
- Never write 3 or more consecutive sentences of similar length. If two medium sentences appear in a row, follow with a short or long one.

### Opener variation

Start sentences with different parts of speech. Rotate through:

- **Noun**: "The migration to Compose reduced build times by 30%."
- **Verb**: "Shipped the SDK to four markets in under six months."
- **Adverb**: "Consistently delivered releases ahead of sprint deadlines."
- **Prepositional phrase**: "Across three product lines, crash-free rates exceeded 99.5%."
- **Gerund**: "Scaling the platform from 2M to 5M installs required rearchitecting the data layer."

### Parallel structure

Break parallelism occasionally. When listing accomplishments in a paragraph, do not phrase every item identically. One item can be a full sentence, the next a clause joined with a dash, the next a short fragment with context.

---

## 3. Word Choice -- Replace AI-Typical Patterns

Certain words and phrases are statistical signals of AI generation. Replace them with specific, evidence-backed alternatives.

| AI pattern | Replacement strategy |
|---|---|
| "passionate about" | Cite specific evidence: "built X to solve Y" |
| "exceptional" | Replace with a quantified result |
| "I am excited to..." | Make a direct statement about fit |
| "leverage" | Use "use," or pick a precise verb: "applied," "integrated," "adopted" |
| "innovative" | Describe what was actually new and why it mattered |
| "Furthermore" / "Moreover" / "Additionally" as paragraph openers | Vary transitions or drop them entirely -- let the content connect itself |
| "In today's fast-paced..." | Cut the entire phrase. Start with substance. |
| "As a seasoned..." | Cut. Open with a concrete claim or result instead. |

Apply these replacements every time, without exception. A single "passionate about" or "Furthermore" at the start of a paragraph can spike the AI probability score.

---

## 4. AI Pattern Avoidance -- Structural Patterns to Break

Beyond word choice, AI text follows recognizable structural templates. Disrupt them.

### Opening lines

Do not start a cover letter with "I am writing to express my interest in..." or any variation of it. Open with a specific fact, a result, or a direct statement about the role.

### Paragraph endings

Do not end paragraphs with broad summary or restatement sentences like "This experience has prepared me well for..." or "These skills make me an ideal candidate." End on the last concrete point -- let it stand without a wrapper.

### Adjective stacking

Do not use triple-adjective descriptions ("innovative, dynamic, forward-thinking"). One adjective per noun, or zero. Prefer showing over labeling.

### Template mirroring

Do not repeat the same sentence template across paragraphs. If paragraph one opens with "At [Company], I [verb]...", paragraph two must not follow the same pattern. Vary the structure deliberately.

### Paragraph length

Mix paragraph lengths within the cover letter. Some paragraphs should be 2 sentences, others 3-4. Uniform 3-sentence paragraphs signal generation.

---

## 5. Authenticity Markers -- What Makes Text Sound Human

AI detection tools look for absence of specificity. Counter this by adding markers that humans naturally include but language models tend to omit.

### Specificity over category

Reference technologies by exact name, not by category. Write "Jetpack Compose" not "modern UI framework." Write "Crashlytics" not "crash reporting tools."

### Concrete details

Include one specific anecdote or measurable detail per cover letter paragraph. A paragraph without at least one concrete reference (a number, a product name, a timeline) will read as generated.

### Contractions

Use occasional contractions in the cover letter ("didn't," "I've," "it's"). Do not use contractions in the CV -- resume conventions expect formal phrasing.

### Opinion and preference

Show decision-making: "Chose Room over SQLite directly because..." or "Preferred modular architecture for this scale." Opinionated statements read as human because language models default to neutral hedging.

### Personal connection

Include one brief personal touch in the cover letter -- why this company specifically, not just why this role. Reference a product, a public engineering blog post, a company value. Generic fit statements ("I admire your mission") do not count; name something specific.

---

## 6. Verification Process

After applying humanization rules, verify the output against AI detection tools.

### Steps

1. Paste the professional summary into GPTZero. Check the AI probability score.
2. Paste the full cover letter body (excluding header/signature) into GPTZero. Check the AI probability score.
3. Target: **below 10% AI probability** on both checks.

### If flagged (10%+ AI probability)

1. Identify which sentences GPTZero highlights as AI-generated.
2. Rewrite flagged sentences with more specificity: add a concrete detail, change the sentence structure, or replace an AI-typical phrase.
3. Re-check with GPTZero.
4. Maximum **2 rewrite-and-check iterations**. If the score does not drop below 10% after two passes, accept the result and note it in the output.

### What to skip

Bullet points do not need detection checking. Standard resume bullet phrasing ("Led," "Built," "Reduced X by Y%") naturally passes AI detection because humans write bullets the same way.

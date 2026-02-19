---
name: cv-tailor
description: >
  This skill should be used when the user asks to "tailor my CV", "customize my resume",
  "adapt my CV for a job", "create a tailored resume", "tailor CV for [company]",
  "generate a cover letter", "prepare my application", or provides a job description
  and asks for a customized CV or application materials. Produces ATS-optimized PDF
  CV and cover letter that pass AI detection checks using WeasyPrint and browser-based
  verification.
---

# CV Tailor

Produce a tailored CV and cover letter from a master CV and a job description. Accept the job description as pasted text or a URL, run a six-step pipeline (parse, analyze, tailor, humanize, verify, generate), and output two print-ready PDF files. Every tailored document preserves factual accuracy from the master CV while maximizing keyword alignment with the target role.

**Inputs:** A job description (pasted text or URL) and the master CV at `references/master-cv.md`.

**Outputs:** A tailored CV PDF and a cover letter PDF, both formatted from HTML/CSS templates and rendered with WeasyPrint.

---

## Prerequisites

Confirm the following before starting the pipeline:

1. **WeasyPrint** installed and on PATH. Verify with `weasyprint --version`. If missing: `pip install weasyprint`.
2. **Claude-in-Chrome browser automation** available for AI detection verification (Step 5).
3. **Master CV** exists at `references/master-cv.md` and loads without error.

---

## Pipeline

### Step 1: Parse Job Description

Determine the input format and extract structured information from the job description.

**Input detection:**
- If the input starts with `http://` or `https://`, treat it as a URL. Fetch the page content using the WebFetch tool with a prompt to extract the full job description text. Convert the fetched content to plain text.
- If the input is plain text, use it directly without fetching.

**Extraction:**
Parse the job description and organize the following fields:

| Field | Description |
|-------|-------------|
| Company name | The hiring organization |
| Role title | Exact title as written in the JD |
| Required skills | Technologies, tools, and methodologies listed as requirements |
| Tech stack | Specific languages, frameworks, libraries, and platforms mentioned |
| Responsibilities | Day-to-day duties and expectations described in the role |
| Nice-to-haves | Skills or experience listed as preferred but not required |

Follow the extraction priority defined in `references/tailoring-rules.md` Section 1: Qualifications/Requirements first (highest signal), then Responsibilities, then Overview, then Nice-to-haves.

Record the JD's exact terminology. If the JD says "Jetpack Compose," write "Jetpack Compose" -- not "modern Android UI toolkit." Match casing, abbreviations, and spelling exactly.

**Present the extracted information to the user in a structured table and ask for confirmation before proceeding.** Correct any misinterpretations the user flags.

---

### Step 2: Gap Analysis

Read `references/master-cv.md` and compare the JD requirements against the master CV content.

**Build a match table** with three columns:

| JD Keyword | CV Evidence | Match Strength |
|------------|-------------|----------------|
| (each required skill/technology from the JD) | (where it appears in the master CV, with specific bullet or section reference) | Strong / Partial / Gap |

**Classify each item:**
- **Strong** -- keyword appears explicitly in the master CV with supporting evidence.
- **Partial** -- adjacent or related experience exists but not an exact match. Note the connection.
- **Gap** -- skill does not appear in the master CV. Flag honestly; recommend how to address it (growth area, adjacent skills, or omit).

**Produce an emphasis strategy:** a prioritized list of 3-5 areas to lead with, based on the strongest JD/CV overlaps.

Refer to `examples/tailoring-example.md` for the expected format and depth of a gap analysis.

**CHECKPOINT: Present the match table, gap list, and emphasis strategy. Wait for explicit confirmation before proceeding to Step 3.**

---

### Step 3: Tailor Content

Read and apply the rules in `references/tailoring-rules.md` for all content modifications in this step.

**Professional summary:**
Rewrite the professional summary targeting the specific role. Follow the rewriting rules from `references/tailoring-rules.md` Section 2:
- Name the target role and company in the first sentence.
- Lead with the most relevant experience area based on the emphasis strategy from Step 2.
- Mirror 2-3 key phrases from the JD naturally within the sentence structure.
- Keep the summary to 2-3 sentences.
- Include at least one quantified result pulled from the master CV.

**Skills section:**
Reorder skills within each existing category to surface JD-mentioned skills first. Follow `references/tailoring-rules.md` Section 3:
- Move JD-matched skills to the front of each category.
- Use the JD's exact terminology for skill names.
- Promote relevant but non-prominent skills to more visible positions.
- Never add skills absent from the master CV.
- Never remove skills -- reorder only.

**Work experience bullets:**
Rephrase bullets to increase JD alignment while preserving factual accuracy. Follow `references/tailoring-rules.md` Section 4:
- Preserve the original achievement and metric exactly. Never round, inflate, or reframe a number.
- Reframe context using JD language where it fits naturally.
- Mirror specific JD verbs (e.g., "architect" becomes "architected," "drive" becomes "drove").
- Reorder bullets within each role so the strongest JD matches appear first.
- Never fabricate experience, projects, or technologies not present in the master CV.

**Boundaries -- do not change:**
Company names, employment dates, job titles, metrics and numbers, education, certifications, languages, or technologies not actually used in a given role. These are factual anchors per `references/tailoring-rules.md` Section 5.

**Run the quality checklist** from `references/tailoring-rules.md` Section 6 before presenting the output. Verify 80%+ coverage of required JD skills, accurate metrics, natural readability, and no fabricated content.

**Cover letter:**
Draft a cover letter body (3-4 paragraphs) using the tailored content as source material:
- Open with a specific fact, result, or direct statement about the role. Do not open with "I am writing to express my interest."
- Each paragraph must contain at least one concrete detail (a number, a product name, a timeline).
- Close on the last concrete point. Do not add a wrapper sentence like "These skills make me an ideal candidate."
- Reference something specific about the company (a product, a public engineering blog, a stated mission) to demonstrate genuine interest.

**CHECKPOINT: Present the tailored summary, reordered skills, rephrased bullets, and cover letter draft. Wait for explicit approval before proceeding to Step 4.**

---

### Step 4: Humanize

Read and apply all rules in `references/humanization-rules.md` to the prose sections of the tailored content.

**Scope -- humanize these only:**
- Professional summary -- free-form prose, highest AI detection risk on the CV.
- Cover letter body -- multiple paragraphs of continuous text.

**Skip:** bullet points, skills lists, education, certifications, and languages. These formats already read as human-written.

**Apply every rule from `references/humanization-rules.md`**, specifically:
- **Section 2 (Sentence structure):** Vary sentence length, rotate opener types (noun, verb, adverb, prepositional phrase, gerund), break parallelism occasionally.
- **Section 3 (Word choice):** Replace every AI-typical phrase ("passionate about," "leverage," "innovative," "Furthermore" as opener, etc.) with specific, evidence-backed alternatives. Zero tolerance -- a single "passionate about" can spike the detection score.
- **Section 4 (Structural patterns):** Never open a cover letter with "I am writing to express my interest." Never close paragraphs with summary restatements. Avoid triple-adjective descriptions. Vary sentence templates across paragraphs. Mix paragraph lengths.
- **Section 5 (Authenticity markers):** Use exact technology names, include at least one concrete detail per paragraph, use occasional contractions in the cover letter (not the CV), include opinionated technical statements, and add one personal connection to the specific company.

---

### Step 5: AI Detection Verification

Verify the humanized prose against GPTZero to confirm it reads as human-written.

**Procedure:**

1. Open a new browser tab using the Claude-in-Chrome tools. Navigate to `gptzero.me`.
2. Locate the text input area on GPTZero's main page.
3. Paste the professional summary text into the input area and submit for analysis.
4. Read the AI probability score from the results.
5. Clear the input and repeat with the full cover letter body text (excluding header and signature).
6. Read the AI probability score for the cover letter.

**Threshold:** Both scores must be below 10% AI probability.

**If either score is 10% or higher:**
1. Identify which sentences GPTZero highlights as AI-generated.
2. Rewrite the flagged sentences with more specificity: add a concrete detail, change the sentence structure, or replace an AI-typical phrase per the humanization rules.
3. Re-submit the rewritten text to GPTZero and check the new score.
4. Allow a maximum of 2 rewrite-and-check iterations. If the score does not drop below 10% after two passes, accept the result and note the final score in the output.

**Report the final detection scores for both the professional summary and cover letter to the user.**

---

### Step 6: Generate PDFs

Render the finalized content into PDF files using the HTML/CSS templates and WeasyPrint.

**CV PDF:**

1. Read `assets/cv-template.html`.
2. Replace all `{{PLACEHOLDER}}` variables with the tailored content:
   - `{{FULL_NAME}}` -- "Vladislav Karpman"
   - `{{EMAIL}}` -- from master CV contact section
   - `{{LINKEDIN}}` -- from master CV contact section
   - `{{GITHUB}}` -- from master CV contact section
   - `{{PROFESSIONAL_SUMMARY}}` -- the humanized professional summary
   - `{{SKILLS_ANDROID}}` -- reordered Android skills, comma-separated
   - `{{SKILLS_BACKEND}}` -- reordered Backend skills, comma-separated
   - `{{SKILLS_TESTING}}` -- reordered Testing skills, comma-separated
   - `{{SKILLS_DEVOPS}}` -- reordered DevOps skills, comma-separated
   - `{{SKILLS_LEADERSHIP}}` -- reordered Leadership skills, comma-separated
   - `{{EXPERIENCE_ENTRIES}}` -- generate HTML for each role using the template's `.experience-entry` structure:
     ```html
     <div class="experience-entry">
       <div class="experience-header">
         <span class="company-name">Company Name</span>
         <span class="date-range">YYYY-Present</span>
       </div>
       <div class="job-title">Job Title</div>
       <ul class="achievements">
         <li>Bullet point text</li>
       </ul>
     </div>
     ```
   - `{{EDUCATION}}` -- education entry HTML
   - `{{CERTIFICATIONS}}` -- certification text
   - `{{LANGUAGES}}` -- languages text
3. Write the completed HTML to a temporary file at `/tmp/cv_temp.html`.
4. Run WeasyPrint to generate the PDF:
   ```bash
   weasyprint /tmp/cv_temp.html "CV_Vladislav_Karpman_{Company}.pdf"
   ```
   Replace `{Company}` with the company name from Step 1, substituting spaces with underscores.

**Cover Letter PDF:**

1. Read `assets/cover-letter-template.html`.
2. Replace all `{{PLACEHOLDER}}` variables. Use the same contact fields as the CV, plus:
   - `{{DATE}}` -- current date in "Month Day, Year" format (e.g., "February 19, 2026")
   - `{{COMPANY_NAME}}` -- from Step 1 extraction
   - `{{HIRING_MANAGER_NAME}}` -- "Hiring Manager" unless the user provides a specific name
   - `{{COVER_LETTER_BODY}}` -- the humanized cover letter body wrapped in `<p>` tags, one per paragraph
3. Write the completed HTML to `/tmp/cl_temp.html`.
4. Run WeasyPrint:
   ```bash
   weasyprint /tmp/cl_temp.html "CL_Vladislav_Karpman_{Company}.pdf"
   ```

**Save both PDF files to the current working directory.** Confirm the files exist and report their file paths and sizes to the user.

---

## Output Naming Convention

All output files follow this pattern:

- **CV:** `CV_Vladislav_Karpman_{Company}.pdf`
- **Cover Letter:** `CL_Vladislav_Karpman_{Company}.pdf`

Replace spaces in the company name with underscores. Example: "Acme Corp" produces `CV_Vladislav_Karpman_Acme_Corp.pdf` and `CL_Vladislav_Karpman_Acme_Corp.pdf`.

---

## User Checkpoints

The pipeline pauses at two mandatory checkpoints for user review:

1. **After Step 2 (Gap Analysis)** -- present the match table, gap list, and emphasis strategy. Wait for the user to confirm the analysis direction and any adjustments before tailoring begins.
2. **After Step 3 (Tailored Content)** -- present the rewritten professional summary, reordered skills, rephrased bullets, and cover letter draft. Wait for the user to approve the content before humanization and PDF generation proceed.

Do not skip these checkpoints. Both require an explicit go-ahead from the user before the pipeline continues.

---

## Resource References

All bundled resources are relative to the `skills/cv-tailor/` directory:

| File | Purpose |
|------|---------|
| `references/master-cv.md` | Full master CV content -- the single source of truth for all factual claims, metrics, and experience |
| `references/tailoring-rules.md` | Keyword extraction priorities, professional summary rewriting rules, skills reordering logic, bullet rephrasing guidelines, boundary constraints, and the quality checklist |
| `references/humanization-rules.md` | Sentence variation techniques, AI-typical word replacements, structural pattern disruption rules, authenticity markers, and the GPTZero verification process |
| `assets/cv-template.html` | HTML/CSS template for the CV PDF with placeholder variables for all content sections |
| `assets/cover-letter-template.html` | HTML/CSS template for the cover letter PDF with placeholder variables for header, body, and sign-off |
| `examples/tailoring-example.md` | Before/after demonstration of gap analysis, summary rewriting, skills reordering, and bullet rephrasing for a sample fintech role |

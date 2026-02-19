# Tailoring Rules

Rules for adapting a master CV to a specific job description (JD). Every instruction below applies to the tailored CV output only -- the master CV is never modified.

---

## 1. Keyword Extraction

Extract keywords from the JD before making any changes to the CV.

### What to extract

- **Required skills and technologies** -- exact names, versions, and acronyms as written in the JD.
- **Responsibilities** -- verbs and domains (e.g., "architect microservices," "mentor junior engineers").
- **Soft skills and cultural signals** -- terms like "collaborative," "ownership," "fast-paced."
- **Industry or domain context** -- healthcare, fintech, e-commerce, etc.

### Extraction priority

Parse the JD in this order and weight matches accordingly:

1. **Qualifications / Requirements section** -- highest signal. These are hard filters the recruiter checks first.
2. **Responsibilities section** -- strong signal. Shows what the role does day-to-day.
3. **Overview / About the role** -- supporting signal. Useful for summary rewriting and tone.
4. **Nice-to-haves** -- include if the master CV supports them, but do not prioritize over required items.

### Terminology rule

Record the JD's exact wording. If the JD says "Jetpack Compose," write "Jetpack Compose" -- not "modern Android UI toolkit." If the JD says "CI/CD," use "CI/CD" -- not "Continuous Integration / Continuous Delivery." Match casing, abbreviations, and spelling.

---

## 2. Professional Summary Rewriting

Rewrite the professional summary for every tailored CV. Do not reuse the master summary verbatim.

### Rules

1. **Name the target role and company** in the first sentence. Example opening: "Lead Android Developer with 9+ years of experience..."
2. **Lead with the most relevant experience area.** If the JD emphasizes SDK development, open with SDK work. If it emphasizes team leadership, open with leadership.
3. **Mirror 2-3 key phrases from the JD** naturally. Weave them into the sentence structure -- do not list them or force them into unnatural positions.
4. **Keep it to 2-3 sentences.** One sentence for positioning, one for core strength, one (optional) for a supporting proof point.
5. **Include at least one quantified result.** Pull the most JD-relevant metric from the master CV (e.g., "scaled products to 6M+ installs," "improved crash-free rate to 99.75%").

### Anti-patterns to avoid

- Generic openers like "Highly motivated professional seeking..."
- Listing every skill from the JD in a run-on sentence.
- Repeating the JD verbatim without adding personal context.

---

## 3. Skills Section Reordering

Reorder skills within each existing category. Do not create new categories or remove existing ones.

### Rules

1. **Surface JD-mentioned skills first** within each category. If the JD emphasizes "Jetpack Compose" and "MVVM," move those to the front of the Android category.
2. **Use the JD's exact terminology.** If the JD says "CI/CD," list "CI/CD" -- even if the master CV says "Continuous Integration." Keep both only if they are meaningfully different in context.
3. **Promote relevant but non-prominent skills.** If the master CV lists a skill deep in a category and the JD calls for it, move it to a more visible position.
4. **Never add skills absent from the master CV.** If the JD asks for "Flutter" and the master CV does not include it, do not add it.
5. **Drop nothing silently.** All master CV skills remain in the output. Reorder -- do not delete.

---

## 4. Bullet Point Rephrasing

Adjust work-experience bullets to increase JD alignment while preserving factual accuracy.

### Rules

1. **Preserve the original achievement and metric exactly.** "Reducing user-reported spam complaints by 40%" stays as "40%." "Grew product installs from 2M to over 5M" stays as those numbers. Never round, inflate, or reframe a metric to look more impressive.
2. **Reframe context using JD language.** If the JD emphasizes "scalable architecture," a bullet about modularizing a codebase can adopt that framing: "Transformed a monolithic codebase into 40+ feature modules using Clean Architecture, building a scalable architecture that..."
3. **Mirror specific JD verbs.** If the JD says "architect," use "architected." If it says "drive," use "drove." Match the tense and form naturally.
4. **Lead with the most JD-relevant bullets in each role.** Reorder bullets so the strongest JD matches appear first. Move less relevant bullets down, but keep them.
5. **Limit rewording scope.** Change phrasing, not substance. If a bullet describes building an onboarding flow, do not rewrite it as a "mentoring" bullet just because the JD values mentoring.
6. **Never fabricate experience.** Do not invent projects, technologies, or outcomes that do not exist in the master CV. If a bullet cannot be made more relevant without fabrication, leave it with minimal changes.

### Rephrasing examples

| JD emphasis | Master CV bullet | Tailored bullet |
|---|---|---|
| "architect scalable systems" | "Transformed a monolithic codebase into 40+ feature modules using Clean Architecture and MVVM" | "Architected a scalable multi-module system (40+ feature modules) using Clean Architecture and MVVM, enabling independent team delivery" |
| "mentor engineers" | "Led a team of 3 engineers; established code review standards and mentoring framework for 2 mid-level developers" | "Mentored 2 mid-level developers through a structured code review and growth framework; led a team of 3 engineers" |

---

## 5. Boundaries -- What NOT to Change

These elements are factual anchors. Altering them would misrepresent the candidate.

1. **Company names** -- never rename, abbreviate differently, or omit an employer.
2. **Employment dates** -- never shift, round, or merge date ranges.
3. **Job titles** -- use the exact title held. Do not upgrade "Android Developer" to "Senior Android Developer."
4. **Metrics and numbers** -- never invent a percentage, user count, or timeline that does not appear in the master CV.
5. **Education** -- degree, institution, and graduation year stay unchanged.
6. **Certifications** -- name and issuing body stay unchanged.
7. **Languages** -- language list and proficiency levels stay unchanged.
8. **Technologies not actually used** -- do not add a technology to a role's bullets if the master CV does not associate that technology with that role.

---

## 6. Quality Checklist

Run this checklist after generating the tailored CV. If any item fails, revise before outputting.

### Coverage

- [ ] 80%+ of the JD's required skills appear somewhere in the CV (skills section or bullet points).
- [ ] Every required skill that exists in the master CV is surfaced prominently.
- [ ] Nice-to-have skills from the JD are included where the master CV supports them.

### Professional summary

- [ ] Summary mentions the target role explicitly.
- [ ] Summary includes at least one quantified result.
- [ ] Summary reads as 2-3 natural sentences, not a keyword list.

### Accuracy

- [ ] No fabricated experience, projects, or technologies.
- [ ] All metrics match the master CV exactly.
- [ ] Company names, dates, and titles are unchanged.
- [ ] No skills added that are absent from the master CV.

### Readability

- [ ] Bullet points read naturally -- not keyword-stuffed.
- [ ] JD terminology is woven into context, not dropped in mechanically.
- [ ] The CV could pass a read-aloud test without sounding robotic.

### Formatting

- [ ] Skills are reordered but none are missing.
- [ ] Bullet order within each role leads with the strongest JD match.
- [ ] Education, certifications, and languages sections are present and unchanged.

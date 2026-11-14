# How to prototype the Student Directory in Claude Design

## What's in this folder

| File | What it is | Upload to Claude Design? |
|---|---|---|
| `1-design-brief.md` | Design-ready digest of the Student Directory dictionary: words, entities, status chips, who sees what, rules, open items, suggested screens | Yes |
| `2-sample-data.json` | 14 synthetic students and 15 guardians across Punggol, East Coast and Noida, with transfers, a withdrawal, a pending guardian, Student Pass cases, an APAAR refusal and a MyGIIS conflict | Yes |
| `2-sample-directory.csv` | The same students as one flat table | Yes |
| `3-starter-prompts.md` | The first prompt, follow-up prompts per screen and a review checklist | No, paste from it |

All people and numbers are invented, so the files are safe to upload.

## Steps

**1. Set up MinV as your design system (once).**
Open claude.ai/design, click your organization name at the bottom left, and run the design system onboarding. Give it the MinV source: link the GitHub repo `huyapps1003/AI-Product-Design` (it holds `design-tokens/` and `design-system.html`), or upload `design-system.html`. Check the result on a test page, then turn on **Published** so new projects use MinV. If MinV also exists as React components, you can run `/design-sync` from Claude Code instead.

**2. Create a project** called "Admission and Enrolment prototype" and attach the three files above.

**3. Paste Prompt 1** from `3-starter-prompts.md`. It builds the directory, the student profile and the admissions pipeline board.

**4. Add one screen at a time** with the follow-up prompts: complete enrolment, campus transfer, guardian verification, Student Pass cases, role views, sync conflict, parent app.

**5. Check each round** against the review checklist at the end of `3-starter-prompts.md`, then share or export.

## Limits to know

- The admissions pipeline (enquiry, application, assessment, offer, contract) has no data dictionary yet, so pipeline screens stay light. The next dictionary will fill this in.
- Items in section 7 of the brief are undecided (AEPP, custody, consent, health alerts, student app, Student ID format). Prototypes should show them as "Pending decision".
- Claude Design uses the same usage limits as the rest of Claude; linking the whole repo uses more than uploading the three files.

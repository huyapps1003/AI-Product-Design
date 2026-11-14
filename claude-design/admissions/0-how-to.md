# How to prototype Admissions in Claude Design

| File | What it is | Upload? |
|---|---|---|
| `1-design-brief.md` | Design-ready digest of the Admissions dictionary v0.1 (DL-56…DL-61 Confirmed 14 Nov 2026) | Yes |
| `2-sample-data.json` | 4 enquiries and 10 applications at every stage, mostly SMART Campus Punggol; viewer Lina Goh; "today" 12 Nov 2026 | Yes |
| `3-starter-prompts.md` | First prompt, follow-ups per screen, review checklist | No, paste from it |

All people and numbers are invented.

## Steps

1. Use the same Claude Design project as the Student Directory ("Admission and Enrolment prototype") so MinV and the directory screens are already there, or create a new one with MinV published.
2. Attach the three files (the Student Directory files can stay attached).
3. Paste Prompt 1. It builds the enquiry queue, the pipeline board and the application detail.
4. Add one screen at a time with the follow-ups: capacity and waitlist, offer, contract and cooling-off, Student Pass case, enrolment handoff, campus settings "Not set", parent view.
5. Check each round against the review checklist.

## Limits

- Section 6 of the brief lists what is still open (online applications, who issues offers, contracts outside Singapore, fee amounts…). Show those as "Pending decision".
- Rule values (7 working days cooling-off, fee refund on ICA/MOE rejection) are GIIS Singapore's own settings, not group defaults (DL-59).

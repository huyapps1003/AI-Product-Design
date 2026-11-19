# How to prototype Attendance in Claude Design

| File | What it is | Upload? |
|---|---|---|
| `1-design-brief.md` | Design-ready digest of the Attendance dictionary v0.1 (scope approved by Henry 19 Nov 2026; DL-97…DL-107 Assumed) | Yes |
| `2-sample-data.json` | SMART Campus Punggol CBSE 2026-27 school days (real holidays, 196 study days), campus rules (90/85/75%), 7B register for Thu 19 Nov 2026, gate log, leave requests, corrections, 7B attendance summary, alerts incl. an ICA review, Grades 6-8 register board, parent view; viewer Farah Ismail | Yes |
| `3-starter-prompts.md` | First prompt, follow-ups per screen, review checklist | No, paste from it |

All people and numbers are invented. Holidays and study days come from the GIIS SG Holiday Schedule 2026-27; thresholds, leave routing and late rules from the GIIS handbook (p.23, p.166–167, p.174).

## Steps

1. Use the same Claude Design project as the other modules, so MinV and the earlier screens are already there.
2. Attach the two files.
3. Paste Prompt 1. It builds the class teacher's register on a phone, her leave inbox and the 7B attendance view.
4. Add one screen at a time with the follow-ups: coordinator register board and corrections, reception gate log, Principal leave decision, ICA review (Tier 3), school-day calendar, parent view.
5. Check each round against the review checklist.

## Limits

- Section 6 of the brief lists what is still open. Show those as "Pending decision".
- Thresholds, cut-off and lock times, warning margin and leave routing limits are campus settings, not group defaults.
- DL-97…DL-107 are Assumed until Henry confirms them.

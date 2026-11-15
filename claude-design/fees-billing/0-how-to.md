# How to prototype Fees & Billing in Claude Design

| File | What it is | Upload? |
|---|---|---|
| `1-design-brief.md` | Design-ready digest of the Fees & Billing dictionary v0.1 (PR #20; DL-77…DL-86 Confirmed 14–15 Nov 2026) | Yes |
| `2-sample-data.json` | GIIS Singapore Term 3 billing (April-based CBSE/IGCSE terms; IB DP runs July–May, DL-96): fee plans, billing runs, 12 billing accounts, invoices, payments, credit notes, refunds, a long-leave pause and FPS records. IN-NOI and OWIS have nothing set. Viewer Siti Rahman; "today" 14 Nov 2026 | Yes |
| `3-starter-prompts.md` | First prompt, follow-ups per screen, review checklist | No, paste from it |

All people and amounts are invented. None of them are GIIS's real fees.

## Steps

1. Use the same Claude Design project as Student Directory, Admissions and Classes, so MinV and the earlier screens are already there.
2. Attach the two files.
3. Paste Prompt 1. It builds the finance home, the Sharma family account and the billing run review.
4. Add one screen at a time with the follow-ups: parent app, record a payment, overdue and reminders, refund case, fee plan editor, Admissions view, India "Not set".
5. Check each round against the review checklist.

## Limits

- Section 6 of the brief lists what is still open (amounts, Table 3A/3B percentages, payment methods, discount rules, overdue consequences). Show those as "Pending decision" or "Not set".
- Fee amounts, billing periods, tax and refund rules are each brand-country's or campus's own settings, never group defaults.
- DL-77…DL-86 are Confirmed (14–15 Nov 2026).

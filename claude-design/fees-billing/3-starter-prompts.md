# Starter prompts for Claude Design (Fees & Billing)

Attach `1-design-brief.md` and `2-sample-data.json`, then paste Prompt 1.

## Prompt 1 — first prototype

```
You are designing a clickable prototype of the Fees & Billing module of Global Campus OS, the school platform Global Schools Group runs for its brands. GIIS Singapore is the example.

Read the attached files first:
- 1-design-brief.md: words, chips, rules that change the UI, who sees what, pending decisions.
- 2-sample-data.json: fee plans, billing runs, billing accounts, invoices, payments, credit notes, refunds. Use only these records and amounts.

The viewer is Siti Rahman, Finance Executive for GIIS Singapore (SG-PGL and SG-ECS). Today is 14 November 2026. Term 3 (Dec 2026–Mar 2027) invoices were issued on 3 Nov, due 30 Nov. Billing periods are per curriculum (DL-96): CBSE, Cambridge and IGCSE run April–March; IB DP runs July–May with its own runs. Desktop staff web app.

Build these linked screens:
1. Finance home: to-do cards for the SG-ECS IB DP January joiners run (Needs review, 1 exception), Chloe Tan's refund (due 19 Nov, blocked: Table 3B Not set), overdue accounts (Rohan Mehta, SGD 4,905.30 from Term 2) and one failed card payment.
2. Billing account BA-SG-000418 (payer Neha Sharma): Aarav (SG-PGL 7B) and Ishita (SG-ECS 9A) on one statement; balance; invoices (Term 2 Paid, Term 3 Issued, Science Centre visit Issued); payments; Ishita's sibling discount. Note that Vikram Sharma is a guardian but not a payer.
3. Billing run review for the SG-ECS IB DP January joiners run (IB DP Term 2, Nov 2026–Feb 2027): totals, the exception (Daniel Lee: no payer for welfare and technology fees), and Approve disabled until it is fixed or excluded. Also show the SG-ECS CBSE & IGCSE Term 3 run (Ready for approval; Kavya Nair: long leave, bill March only, resolved automatically).

Rules:
- Use the words and chip colours in the brief.
- Issued invoices have no Edit: Void (before payment) or Credit note.
- Amounts and rules are brand-country or campus settings; "Not set" blocks the step. Never invent refund percentages.
- Never show card numbers; refund bank details masked.
- Teachers never see fees; students never see fees.
- Anything in section 6 of the brief shows "Pending decision".
- No delete.
- Use the attached design system (MinV). Public Sans; sidebar 280px; header 64px; content max width 1200px.
```

## Follow-ups (one at a time)

**Parent app (phone)**
```
Switch the viewer to Neha Sharma on a phone. Tabs: What's due, History, Receipts. What's due lists Term 3 (SGD 16,112.93, due 30 Nov) and the Science Centre visit (SGD 49.05, due 24 Nov) with a total and Pay now (PayNow or Card). Each invoice expands to both children's lines and the sibling discount. Then switch to Vikram Sharma: he sees "Fees for Aarav and Ishita are managed by the payer on this account" and no amounts.
```

**Record a payment**
```
As Siti, record a counter cheque payment of SGD 4,905.30 from Rajesh Mehta. Show the allocation preview (oldest due first: Term 2 invoice SG-2026-030377), the receipt number issued, and Rohan's fee status changing from Overdue to Due.
```

**Overdue and reminders**
```
Add an Overdue list: one row per account with days overdue, amount, last reminder and fee status. Open Rohan Mehta to see reminders sent (7 days, 30 days, a manual WhatsApp on 15 Oct). Late fees and consequences show "Pending decision (OQ-84)". Nothing is blocked automatically.
```

**Refund case**
```
Open Hana Kim's refund (ICA rejected the Student Pass): registration fee SGD 1,308.00, less the SGD 50 administrative charge = SGD 1,258.00, approved by Wendy Koh and Vivek Menon, paid on 10 Nov to ••••4417. Then open Chloe Tan's: basis Table 3B (other reasons), notice 10 Nov, last day 31 Dec, documents requested 11 Nov, due by 19 Nov with escalation steps. The calculation is blocked with "Table 3B percentages Not set (OQ-86)". Show the draft credit note for Jan–Feb proration.
```

**Fee plans**
```
Add Fee plans for SG-PGL AY 2026-27: list with status and version. Open IB PYP v2 (change reason, valid from 1 Sep, v1 Superseded). Open CBSE Grades 11-12 Draft: tuition "Not set" and Submit disabled. Switch to Wendy Koh to approve a submitted plan.
```

**Admissions view**
```
Switch to Lina Goh (Admissions). She sees only admission fee invoices: Noah Fernandes registration Paid (contract in cooling-off until 17 Nov), Sofia Rossi registration Due, Hana Kim Refunded. No tuition invoices, no billing accounts.
```

**India "Not set"**
```
Switch to Ankit Bansal (Finance Executive, IN-NOI). Billing periods, fee plan amounts, tax and refund rules are all "Not set", so the run can't start and Meher Kapoor's refund is blocked. Each blocker links to the setting that's missing.
```

## Review checklist

- [ ] Only sample records and amounts used.
- [ ] Issued invoices never editable; void or credit note only.
- [ ] Billing account = payer; siblings on one statement; sponsor shown.
- [ ] Run exceptions block approval.
- [ ] Discounts shown as their own line.
- [ ] Failed payment changes nothing; card numbers never shown; bank details masked.
- [ ] Fee status visible to Principal and Admissions, never teachers; no automatic sanctions.
- [ ] Refund shows rule version, basis, approvals, deadline; no invented percentages.
- [ ] Parents see only accounts they pay; students see nothing.
- [ ] "Not set" and "Pending decision" shown, never invented.
- [ ] No Delete.

# Campus OS — Fees & Billing: design brief for Claude Design

**Built from:** `product-truth/03-domain/fees-billing-data-dictionary.md` v0.1 (PR #20), OBJ-23 card, Student Directory Spec v1.0 (ST02, ST05), Admissions dictionary (AD09), GIIS Student Handbook 2025-26 (HB) p.237–247
**Sample data:** `2-sample-data.json`. Where this brief and the dictionary disagree, the dictionary wins.

## 1. Context

- Global Campus OS is the school platform GSG runs for all its brands. GIIS Singapore is the detailed example: SMART Campus Punggol (SG-PGL) and East Coast (SG-ECS). GIIS Noida (IN-NOI) and OWIS show the "Not set" path.
- Scope: **what a family is charged, who pays, how they pay, and how money goes back.** Campus OS bills; the ERP only receives journals (DL-77, Confirmed). The ledger, tax filing, transport routes, shops and scholarship selection are out.
- Main viewer: **Siti Rahman, Finance Executive, GIIS Singapore** (SG-PGL and SG-ECS). Today is **14 Nov 2026**. Term 2 (Nov 2026–Feb 2027) invoices were issued on 3 Nov and are due on 30 Nov.
- Other viewers: Wendy Koh (Finance Controller, GIIS Singapore), Vivek Menon (Country Director), Dr Anita Rao (Principal SG-PGL), Lina Goh (Admissions SG-PGL), Neha Sharma (parent, payer), Vikram Sharma (parent, not a payer), Ankit Bansal (Finance Executive IN-NOI).
- Staff web app, desktop first. The parent app is mobile first.

## 2. Words

| Use | Meaning | Don't use |
|---|---|---|
| **Fee type** | A kind of fee in the brand-country list, e.g. Tuition, Bus, Technology | Fee head, charge code |
| **Fee plan** | A campus's fees for one programme and grade band for a year; approved by the Finance Controller and versioned (DL-78) | Fee structure, price list |
| **Billing period** | The slice invoices are raised for; GIIS SG uses 4-month terms (HB p.244) | Cycle |
| **Billing account** | What one payer settles in one brand-country; a family statement (DL-79) | Ledger, wallet |
| **Payer** | The guardian or sponsor who pays a share (DL-79) | Bill-to, customer |
| **Sponsor** | An organisation that pays, e.g. an employer (OQ-85) | Corporate client |
| **Billing run** | Raising one period's invoices for a campus as a reviewed batch (DL-80) | Batch job |
| **Invoice**, **Credit note**, **Receipt** | As usual; issued invoices never change (DL-80) | Bill (in staff UI) |
| **Discount**, **Scholarship**, **Waiver** | Dated reductions with an approver (DL-81) | Concession |
| **Fee status** | Clear, Due, Overdue, Paused per student (DL-83) | Defaulter |
| **Refund** | Money paid back, worked out from the refund rules (DL-84) | Reimbursement |
| **Long leave** | Approved absence with no fees (HB p.237) | Deferment (in the parent UI) |
| **Fee protection (FPS)** | Singapore's fee insurance per student (HB p.246) | |
| **Not set** | A setting nobody has entered yet; it blocks the step | 0, default |

## 3. Chips

Invoice: Draft · default, Issued · info, Partially paid · warning, Paid · success, Overdue · error, Void · default, Credited · default.
Fee status: Clear · success, Due · info, Overdue · error, Paused · default.
Fee plan: Draft · default, Submitted · info, Approved · success, Superseded · default. An amount shown as "Not set" · warning.
Billing run: Draft · default, Needs review · warning, Ready for approval · info, Approved · info, Issued · success, Cancelled · default. Exception count · warning.
Payment: Pending · info, Succeeded · success, Failed · error, Reversed · warning.
Refund: Calculating · info, Awaiting documents · warning, Awaiting approval · info, Approved · info, Paid · success, Rejected · default, Forfeited · default. "Blocked: Not set" · warning.
Adjustment: Requested · info, Approved · success, Rejected · default, Ended · default.
FPS: Required · warning, Purchased · success, Cancelled · default.

## 4. Rules that change the UI

1. **Settings, no group default** (SR-FB-02). Amounts, billing periods, due dates, tax and refund rules belong to each brand-country or campus. Missing values show "Not set" and block the step: plan approval, the billing run, or the refund calculation. IN-NOI and OWIS have nothing set.
2. **Fee plans are versioned** (DL-78). An approved plan can't be edited. "New version" asks for a valid-from date and a reason. IB PYP is on version 2 (technology fee added from 1 Sep). CBSE Grades 11–12 is a Draft with tuition "Not set".
3. **Billing run review** (DL-80). A run lists exceptions that must be fixed or excluded before Wendy approves: Arjun Reddy (plan Not set, excluded), Daniel Lee (welfare and technology fees have no payer). Approving issues all invoices, and parents are notified.
4. **Invoices never change** (DL-80). Issued invoices have no Edit button. Before any payment: Void with a reason. After a payment: Credit note.
5. **Billing account = payer** (DL-79). The Sharma account shows Aarav (SG-PGL 7B) and Ishita (SG-ECS 9A) on one statement. Daniel Lee's tuition is paid by a sponsor (Hanbit Logistics) and his registration by his father. Each student shows their share (100%, or a split).
6. **Discounts on the line** (DL-81). The invoice shows the full fee, then the discount as its own line (Ishita: sibling 5%; Yuki: scholarship 25%). The reason is visible to Finance only.
7. **Payments** (DL-82). Online payments count only when the gateway confirms them, so a failed card shows as Failed and changes nothing. Finance records counter, cheque and bank transfer payments, and a receipt is issued. Money goes to the oldest due invoice first unless Finance chooses. Card numbers never appear. Refund bank details are masked (••••4417).
8. **Fee status, no sanctions** (DL-83). Principals and Admissions see Clear/Due/Overdue/Paused. Teachers never do. Nothing is blocked automatically. Any consequence of overdue fees shows "Pending decision (OQ-84)".
9. **Refunds** (DL-84). The refund screen shows the rule set version, the basis (cooling-off, Table 3A, Table 3B, ICA/MOE rejection, humanitarian), the calculation per fee, the two approvals (Finance Controller and Country Director at GIIS SG) and the 7-working-day deadline with escalation. Hana Kim: registration fee refunded less SGD 50, Paid. Chloe Tan: blocked, because the Table 3B percentages are "Not set (OQ-86)". Never invent percentages.
10. **Parents see only what they pay** (DL-85). Neha sees both children's invoices, Pay now, history and receipts. Vikram is a verified guardian but not a payer, so he sees "Fees are managed by the payer on this account" and nothing else. Students never see fees.
11. **Long leave and withdrawal** (DL-86). Kavya Nair's long leave pauses billing from Dec to Feb, so the run bills November only. A withdrawal approved at least one month before a term isn't billed for that term (GIIS SG setting). Chloe's credit note prorates Term 2 to December.
12. **Admission fees** (SR-FB-09). Admissions sees only admission fee invoices and their status, mirrored on AD09: Noah Paid, Sofia Due, Hana Refunded.
13. **No delete.** Void, credit, reverse, end, cancel.

## 5. Who sees what (summary)

Finance executive: prepares plans and runs, manages accounts, records payments, requests discounts, prepares refunds. Finance Controller: approves plans, runs, discounts, credit notes and refunds. Country Director: co-approves refunds (GIIS SG). Principal: fee status and campus totals, no invoices. Admissions: admission fee invoices and fee status of their applicants. Teachers: nothing. Parent who pays: their account. Other guardians: only if a payer shares it. Students: nothing. Group leadership: totals only.

## 6. Pending decisions (show "Pending decision" or "Not set")

OQ-68 / OQ-80 fee amounts and billing frequencies (sample amounts are synthetic) · OQ-81 ERP and ledger codes · OQ-82 payment methods per country, card surcharge · OQ-83 discount rules and approval limits · OQ-84 reminders, late fees, overdue consequences · OQ-85 who can be a payer, sponsor access · OQ-86 Table 3A/3B percentages, refund rules outside GIIS SG · OQ-87 tax per fee type · OQ-88 fee protection outside SG · OQ-89 deposits · OQ-90 history from MyGIIS · OQ-92 other currencies.

## 7. Suggested screens

1. Finance home (to do: runs to review, refunds due, overdue, failed payments) · 2. Billing account (statement, students and shares, invoices, payments, adjustments) · 3. Invoice detail · 4. Billing run review with exceptions · 5. Fee plans list + editor with versions · 6. Record a payment · 7. Overdue list and reminders · 8. Refund case · 9. Parent app: What's due, Pay, History, Receipts · 10. Admissions fee view · 11. Settings with "Not set".

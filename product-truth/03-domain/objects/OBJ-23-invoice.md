---
id: OBJ-23
name: Billing account and invoice
entities: [FB01, FB02, FB03, FB04, FB05, FB06, FB07, FB08, FB09, FB10, FB11, FB12, FB13, FB14, FB15, FB16, FB17]
owner: fees_finance        # Campus OS owns billing; the ERP receives journals (DL-77)
home: cell                 # data location of the brand-country (DL-47); one account per payer per brand-country (DL-79)
tenant_path: group/brand/brand-country/campus
operating_entity: issuer   # invoice issuer = enrolment operating_entity_id (DL-41); named after OQ-06
effective_dated: true      # fee plan versions (FB02), responsibilities (FB06), adjustments (FB07), pauses (FB16)
states:                    # invoice_status, DL-27 rule; colours proposed for review
  DRAFT: default
  ISSUED: info
  PARTIALLY_PAID: warning
  PAID: success
  OVERDUE: danger
  VOID: default
  CREDITED: default
# Fee status per enrolment (DL-83): CLEAR success, DUE info, OVERDUE danger, PAUSED default. No automatic sanctions.
# Billing run (FB08) run_status: DRAFT, NEEDS_REVIEW, READY_FOR_APPROVAL, APPROVED, ISSUED, CANCELLED
# Refund (FB14) refund_status: CALCULATING, AWAITING_DOCUMENTS, AWAITING_APPROVAL, APPROVED, PAID, REJECTED, FORFEITED
field_groups:
  - name: Fee types and fee plans
    tier: T1
    capability: fees.plan.edit
    approve: [fees.plan.approve]              # Finance Controller (Arch § Users and roles)
  - name: Billing account, payers and shares
    tier: T2
    capability: fees.account.view
    edit: [fees.account.manage]
    parent: payer or shared only (DL-85)
  - name: Invoices, payments, receipts, credit notes
    tier: T2
    capability: fees.invoice.view
    edit: [fees.payment.record, fees.credit_note.create, fees.invoice.issue_one_off]
    parent: own accounts: view, pay, download (DL-85); students never (DL-68)
  - name: Fee status per enrolment
    tier: T2
    capability: fees.status.view              # Admissions, Principal; never teachers (DL-83)
  - name: Discounts and scholarships
    tier: T2
    capability: fees.adjustment.request
    approve: [fees.adjustment.approve]        # approver OQ-83
  - name: Refund calculation and approval
    tier: T2
    capability: fees.refund.prepare
    approve: [fees.refund.approve]            # GIIS SG: Country Director + Finance Controller (HB p.244)
  - name: Refund bank details
    tier: T2
    capability: fees.refund.pay               # masked otherwise; reveal logged (SR-FB-20)
actions:
  - fees.catalogue.manage
  - fees.plan.edit
  - fees.plan.approve
  - fees.account.manage
  - fees.adjustment.request
  - fees.adjustment.approve
  - fees.run.prepare
  - fees.run.approve
  - fees.invoice.issue_one_off
  - fees.invoice.void
  - fees.credit_note.create
  - fees.credit_note.approve
  - fees.payment.record
  - fees.payment.reverse
  - fees.refund.prepare
  - fees.refund.approve
  - fees.refund.pay
  - fees.fps.manage
events: [FeePlanApproved, BillingRunApproved, InvoiceIssued, InvoiceOverdue, PaymentReceived, AdmissionFeePaid, PaymentReversed, CreditNoteIssued, FeeStatusChanged, RefundApproved, RefundPaid, BillingPaused, FpsRecordChanged, JournalPosted]
variation: [MC-12, MC-14]
open: [OQ-06, OQ-68, OQ-80, OQ-81, OQ-82, OQ-83, OQ-84, OQ-85, OQ-86, OQ-87, OQ-88, OQ-89, OQ-90, OQ-91, OQ-92]
sample: product-truth/03-domain/samples/fees-billing-sample.json
# Amounts, billing periods, tax, reminders and refund rules are settings with no group default: show the value or "Not set" (SR-FB-02). Never invent Table 3A/3B percentages (OQ-86).
patterns: [AP-01, AP-02, AP-04, AP-05, AP-13, AP-14]
---

# OBJ-23 · Billing account and invoice

**Status:** Proposed with the fees & billing dictionary v0.1, 14 Nov 2026 (DL-77, DL-83, DL-85 Confirmed; others Assumed) · **Owner:** Henry
**Cites:** [fees & billing dictionary](../fees-billing-data-dictionary.md) · DL-08, DL-41, DL-47, DL-59, DL-68 · HB p.237–247

The family sees a **billing account** (one payer in one brand-country) with invoices for each child it pays for. Staff see the same account with runs, payments, credits and refunds. Invoices are never edited once issued (DL-80); a correction is a credit note. Fee status is the only finance fact other modules read, and it never blocks learning on its own (DL-83).

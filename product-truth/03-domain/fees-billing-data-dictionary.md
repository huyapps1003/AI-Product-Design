# Global Campus OS — Fees & Billing Data Dictionary v0.1 (draft)

**Status:** Proposed draft, 14 Nov 2026. DL-77…DL-86 Confirmed (Henry, 14–15 Nov 2026); OQ-80…OQ-92 Open · **Owner:** Henry
**Cites:** [Architecture v1](../00-sources/Global_Campus_OS_Product_Architecture_v1.md) § Context (refunds, MyGIIS "Fees History"), § Users and roles (Country Director, Finance Controller, Parent), § Domains and modules (module 7 Fees and Finance), § Key workflows (Fees, withdrawal and refund; Admission to enrolment step 2), § Tenancy (fee plans per campus; currency, tax, refund and fee protection per country; effective dating; regulator packs), § Data model (Finance: fee plan, invoice, payment, refund case, scholarship, insurance policy), § Integrations (payment gateways, accounting/ERP, FPS insurer), § Security (Tier 2 finance), § Open questions (build, buy or hybrid) · [Student Directory Spec v1.0](./student-directory-data-dictionary.md) ST02, ST04, ST05, F01, StudentEnrolled, EnrolmentEnded · [Admissions dictionary](./admissions-data-dictionary.md) AD08, AD09, AD10, SR-AD-04, SR-AD-06 · [Classes dictionary](./classes-data-dictionary.md) CL01 · [Organisation dictionary](./organisation-data-dictionary.md) OR03 brand-country, OR04 campus, OR07/OR08 legal entity and operator link · [architecture.md](./architecture.md) DL-41 `operating_entity_id`, DL-44 events · [data-classification](../05-data-governance/data-classification.md) · [regulatory-context](../05-data-governance/regulatory-context.md) (FPS) · GIIS Student Handbook 2025-26 (HB) p.34, p.137, p.236, p.237–239 (deferment, withdrawal, transfer), p.240–246 (refund policy, FPS) · DL-08, DL-11, DL-22, DL-25, DL-27, DL-28, DL-32, DL-37, DL-41, DL-47, DL-53, DL-55, DL-59, DL-68 · MC-12, MC-14 · OQ-06, OQ-28, OQ-68
**Tóm tắt (VI):** Từ điển dữ liệu cho module Học phí & Thanh toán: danh mục loại phí theo brand-country, biểu phí theo cơ sở × chương trình × khối × năm học, tài khoản thanh toán của người trả (phụ huynh hoặc nhà tài trợ), đợt lập hoá đơn, hoá đơn, giảm trừ và học bổng, thanh toán và biên lai, nhắc nợ, hoàn phí, tạm dừng tính phí khi nghỉ dài hạn, và bảo hiểm học phí (FPS). Campus OS là nơi lập hoá đơn; ERP chỉ nhận bút toán (DL-77). Mỗi trường có trạng thái Known / Assumed / Open.

> Drafted by Claude from product truth on 14 Nov 2026 after Henry approved the scope the same day and chose option A: Campus OS owns billing (DL-77).

## How to read this file

Same layout and rules as the [Student Directory Spec](./student-directory-data-dictionary.md#how-to-read-this-file): **Known** (cited), **Assumed** (DL), **Open** (OQ); tiers T1/T2/T3 (DL-11; finance is T2, Architecture § Security); `Y`/`N`/`C` required. Every row also carries `tenant_path`, `data_region` (UI term **Data location**, DL-47) and the shared common fields. Facts from the GIIS Handbook are **GIIS Singapore examples**, not group rules (DL-22). All enums are unapproved candidates (DL-27 rule). All amounts in samples are synthetic, never GIIS's real fees.

## Scope

Approved by Henry, 14 Nov 2026: **what a family is charged, who pays, how they pay, and how money goes back.**

| In this dictionary | Not in this dictionary (owner) |
|---|---|
| Fee type catalogue per brand-country | General ledger, GST/VAT filing, accounts payable (ERP; Campus OS sends journals, DL-77) |
| Fee plans per campus × programme offering × grade × academic year, effective-dated | Withdrawal and transfer cases (Engagement Case + Admissions); this file adds fee clearance and the refund |
| Billing periods (for example GIIS SG's 4-month terms, HB p.244) | Scholarship applications and selection (out; this file records the award) |
| Billing accounts, payers, split and sponsor billing, family statements | Transport routes, bookshop, uniform shop (Campus Services); their charges arrive as one-off charges |
| Billing runs, invoices, one-off charges, admission fee invoices (AD09) | Agent commissions (OQ-70), medical insurance (Campus Services) |
| Discounts, scholarship awards, waivers | Payroll, procurement, staff expenses |
| Payments (gateway, bank transfer, counter), receipts, credit notes | Fee amounts for admission (still OQ-68; this file holds them once set) |
| Due dates, reminders, fee status read by other modules | |
| Refunds (cooling-off, Table 3A/3B, admission fee refund on ICA/MOE rejection), approvals, deadlines | |
| Billing pause for long leave (HB p.237) | |
| Fee protection record (Singapore FPS) | |
| Parent view: what's due, pay, history, receipts (MyGIIS "Fees History" parity, Arch § Context) | |

## Modeling boundaries

- **Campus OS bills; the ERP keeps the books (DL-77, Confirmed).** Fee plans, invoices, payments, credit notes and refunds are written here. Each posting emits a journal to the brand-country's accounting system (Arch § Integrations "Accounting / ERP: outbound journals"). Which ERP, and its chart of accounts, is open (OQ-81).
- **Fee types per brand-country, amounts per campus (DL-78).** The brand-country keeps the list of fee types (tuition, bus, technology...) with their refund class and tax treatment. Each campus sets amounts in a fee plan per programme offering, grade and academic year. The Finance Controller approves a plan; an approved plan is never edited, a change is a new version with a valid-from date (Arch § Tenancy "Effective dating"). No group default amounts (DL-59 pattern): unset shows "Not set" and blocks the billing run.
- **Bill the payer, not the child (DL-79).** A billing account belongs to one payer in one brand-country: a VERIFIED guardian (ST05) or a sponsor organisation (an employer, an embassy). Each student's enrolment is linked to one or more accounts with a share (100%, or 50/50 between parents) and the fee types it covers. Siblings at different campuses of the same brand-country appear on one family statement. The invoice is issued by the enrolment's operating legal entity (DL-41, `operating_entity_id`).
- **Invoices come from runs or defined events, and never change once issued (DL-80).** Term invoices come from a billing run that Finance reviews and approves per campus and period. Other invoices come only from defined events: an admission fee due (AD09), a one-off charge, a mid-term joiner. An issued invoice is never edited; a correction is a credit note plus, if needed, a new invoice.
- **Discounts and scholarships are dated adjustments (DL-81).** Sibling, staff-child, scholarship, bursary and waiver adjustments are dated records with an approver, applied when an invoice is built. The adjustment shows on the invoice line, so the parent sees the full fee and the reduction.
- **Payments: the gateway is the truth online; Finance records offline (DL-82).** An online payment is PAID only when the gateway confirms it. Bank transfers, cheques and counter payments are recorded by Finance with a receipt. A payment is allocated to the oldest due invoice first unless Finance chooses otherwise. Card numbers are never stored, only the gateway's reference. Bank details for a refund payout are masked.
- **One fee status per enrolment, no automatic sanctions (DL-83, Confirmed).** Fees and Finance publishes a derived fee status per enrolment: CLEAR, DUE, OVERDUE or PAUSED. Admissions (AD09) and withdrawal clearance (HB p.237 step 5) read it. It never blocks attendance, lessons, results or the parent app on its own. Any consequence of overdue fees is a campus setting, still open (OQ-84). Teachers don't see fee status.
- **Refunds are calculated from a versioned rule set and approved on a Case (DL-84).** Each brand-country keeps a refund rule set (for GIIS SG: cooling-off, Table 3A, Table 3B, non-refundable fees, the SGD 50 administrative charge, humanitarian grounds; HB p.240–245). A refund records which rule version it used. The approval and its deadline run on the shared Case (DL-08). Approvers are a brand-country setting; GIIS SG's are the Country Director and the Finance Controller (Known, HB p.244; Arch § Key workflows).
- **Only payers see an account (DL-85, Confirmed).** A parent sees and pays invoices for accounts where they are a payer. A verified guardian who isn't a payer sees nothing financial unless a payer shares the account with them. Students never see fees. This sits on the parent path (DL-37): the family link (F01) proves the relationship, the billing responsibility (FB06) narrows finance to payers.
- **Long leave pauses billing; withdrawal stops it (DL-86).** An approved deferment (HB p.237: "The student does not pay fees during this long leave period") creates a billing pause. Billing after a withdrawal follows the campus's setting; GIIS SG bills to the month of the withdrawal date if it was approved at least one month before the term starts (HB p.244).
- **Data location.** Billing accounts, invoices and payments sit in the data location of the brand-country (DL-47). A family with children in two countries has one account per country (DL-55 pattern for guardian profiles); the parent app shows both.

## Entities

| ID | Entity | Module | Status |
|---|---|---|---|
| FB01 | Fee Type | Fees and Finance | Known need (HB p.242 fee categories; Arch § Tenancy currency and tax per country); model Confirmed (DL-78) |
| FB02 | Fee Plan | Fees and Finance | Known (Arch § Key workflows "Fee plans are set per campus and programme"; § Data model "Fee plan") |
| FB03 | Fee Plan Line | Fees and Finance | Confirmed (DL-78) |
| FB04 | Billing Period | Fees and Finance (reads Academic year) | Known for GIIS SG (4-month terms, HB p.244; Arch § Context); per campus Assumed |
| FB05 | Billing Account | Fees and Finance | Confirmed (DL-79) |
| FB06 | Billing Responsibility | Fees and Finance | Confirmed (DL-79); who can pay Open (OQ-85) |
| FB07 | Fee Adjustment | Fees and Finance | Known "Scholarship" (Arch § Data model; HB p.236 scholarship office); model Confirmed (DL-81) |
| FB08 | Billing Run | Fees and Finance | Confirmed (DL-80) |
| FB09 | Invoice | Fees and Finance | Known (Arch § Data model "Invoice"; HB p.137 "School Fees / Invoice") |
| FB10 | Invoice Line | Fees and Finance | Confirmed (DL-80) |
| FB11 | Payment | Fees and Finance | Known (Arch § Data model "Payment"; § Integrations payment gateways) |
| FB12 | Payment Allocation | Fees and Finance | Confirmed (DL-82) |
| FB13 | Credit Note | Fees and Finance | Confirmed (DL-80) |
| FB14 | Refund | Fees and Finance (workflow on Case, OBJ-10) | Known (Arch § Data model "Refund case"; HB p.240–245) |
| FB15 | Payment Reminder | Fees and Finance (sends through Engagement) | Known need (MyGIIS "outstanding fees", HB p.34); schedule Open (OQ-84) |
| FB16 | Billing Pause | Fees and Finance | Known for GIIS SG (HB p.237 long leave) |
| FB17 | Fee Protection Record | Fees and Finance (Singapore regulator pack) | Known (HB p.246–247 FPS; Arch § Integrations FPS insurer) |

ST02, ST04, ST05, AD09, AD10 and Case (OBJ-10) are referenced, not redefined. AD09 `invoice_ref` points to FB09.

## Detailed attribute dictionary

### FB01 — Fee Type

**Purpose:** One kind of fee a brand-country charges, such as tuition or bus fees, with the rules that follow it everywhere.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| fee_type_id | UUID | PK | Y | T1 | Assumed | |
| brand_country_id | UUID | FK:OR03 | Y | T1 | Confirmed (DL-78) | The list belongs to the brand-country. |
| fee_code | VARCHAR(32) | UK with brand-country | Y | T1 | Known examples (HB p.242: Tuition, Student Welfare, Activity, Bus, Resource, Building Development, School Event, Technology, Registration, Application) | |
| fee_name | VARCHAR(80) | — | Y | T1 | Known | Shown to parents. |
| fee_category | ENUM | — | Y | T1 | Assumed (DL-27) | See enums. |
| refund_class | ENUM | — | Y | T1 | Known for GIIS SG (HB p.242: refundable categories; registration and application non-refundable except on ICA/MOE rejection) | REFUNDABLE, NON_REFUNDABLE, REFUNDABLE_ON_APPROVAL_REJECTED. |
| fps_covered | BOOLEAN | — | C | T1 | Known for SG (HB p.246: FPS covers all fees except application fee, agent commission, miscellaneous fees and GST) | Required where the regulator pack has fee protection. |
| tax_code | VARCHAR(16) | — | Y | T1 | Known need (Arch § Tenancy "currency and tax (GST)"); rates Open (OQ-87) | "Not set" blocks invoicing. |
| is_optional | BOOLEAN | — | Y | T1 | Assumed | Optional fees (bus) apply only when the student opts in. |
| ledger_code | VARCHAR(32) | — | C | T1 | Open (OQ-81) | Mapping to the ERP chart of accounts. |
| active | BOOLEAN | — | Y | T1 | Assumed | Retired types stay on old invoices. |

### FB02 — Fee Plan

**Purpose:** The fees a campus charges for one programme offering, grade and academic year. Approved once, then fixed.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| fee_plan_id | UUID | PK | Y | T1 | Known entity (Arch § Data model) | |
| campus_id | UUID | FK:OR04 | Y | T1 | Known (Arch § Tenancy "Campus: fee plans") | |
| programme_offering_id | UUID | FK:AC06 | Y | T1 | Known (Arch "per campus and programme") | |
| grade_ids | UUID[] | FK:AC02 | Y | T1 | Assumed | One plan may cover a band of grades (Grades 6–8). A grade appears in one plan per offering and year. |
| academic_year_id | UUID | FK:AC07 | Y | T1 | Assumed | |
| version | INT | UK with the four keys | Y | T1 | Known need (Arch § Tenancy "Every fee plan has a valid-from date") | |
| valid_from | DATE | — | Y | T1 | Known (Arch § Tenancy) | Invoices use the version valid on the period start. |
| currency | CHAR(3) | — | Y | T1 | Known (Arch § Tenancy currency per country; MC-12) | Other currencies: OQ-92. |
| plan_status | ENUM | — | Y | T1 | Assumed (DL-27) | DRAFT, SUBMITTED, APPROVED, SUPERSEDED. |
| prepared_by_user_id | UUID | FK:A01 | Y | T1 | Assumed | Finance executive. |
| approved_by_user_id / approved_at | — | — | C | T1 | Known (Arch § Users and roles: Finance Controller "Fee plans") | |
| change_reason | TEXT | — | C | T1 | Assumed | Required for version 2 onwards. |
| annual_total | DECIMAL | — | Y | T1 | Assumed; derived | Shown on admission offers and the parent fee schedule. |

### FB03 — Fee Plan Line

**Purpose:** One fee in a plan: what, how much, and how often.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| plan_line_id | UUID | PK | Y | T1 | Assumed | |
| fee_plan_id | UUID | FK:FB02 | Y | T1 | Assumed | |
| fee_type_id | UUID | FK:FB01 | Y | T1 | Assumed | |
| amount | DECIMAL(12,2) | — | Y | T1 | Open values (OQ-68 admission fees; OQ-80 others) | Per charge, before tax. "Not set" blocks approval. |
| charge_frequency | ENUM | — | Y | T1 | Known for GIIS SG tuition: per 4-month term (HB p.244); others Open (OQ-80) | See enums. |
| applies_to | ENUM | — | Y | T1 | Assumed | ALL_STUDENTS, NEW_STUDENTS_ONLY, OPT_IN. |
| due_stage | ENUM | — | C | T1 | Known (AD09 `due_stage`) | Admission fees only: AT_APPLICATION, AT_ACCEPTANCE. |

### FB04 — Billing Period

**Purpose:** The slices of an academic year that invoices are raised for. Years are per campus × programme offering (DL-96), so one campus can have different periods per curriculum.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| billing_period_id | UUID | PK | Y | T1 | Assumed | |
| campus_id | UUID | FK:OR04 | Y | T1 | Assumed | |
| academic_year_id | UUID | FK:AC07 | Y | T1 | Assumed | |
| programme_offering_id | UUID | FK:AC06 | Y | T1 | Confirmed (DL-96, DL-114) | Always set: years are per campus × offering, so billing periods are too (IB DP July start differs from CBSE April). Billing periods lie inside the AC07 year but are not academic terms (AC08). |
| period_label | VARCHAR(40) | — | Y | T1 | Known for GIIS SG ("term period of four (4) months", HB p.244) | e.g. "Term 2 (Aug–Nov)" for GIIS SG CBSE (April year; HB p.244 example "Term 2 … starts in August"). |
| period_start / period_end | DATE | — | Y | T1 | Assumed | Must sit inside the academic year (MC-06). |
| invoice_date | DATE | — | Y | T1 | Assumed | When the run issues invoices. |
| due_date | DATE | — | Y | T1 | Open (OQ-84) | Campus setting; "Not set" blocks the run. |
| withdrawal_cutoff_date | DATE | — | C | T1 | Known for GIIS SG (one month before the term starts, HB p.244) | Withdrawals approved by this date are not billed for the period. |

### FB05 — Billing Account

**Purpose:** The account one payer settles in one brand-country. The family statement.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| billing_account_id | UUID | PK | Y | T2 | Confirmed (DL-79) | |
| account_number | VARCHAR(20) | UK | Y | T2 | Assumed | Shown on statements and bank transfer references. Format Open (with OQ-55). |
| brand_country_id | UUID | FK:OR03 | Y | T1 | Confirmed (DL-79) | Selects the data location (DL-47). |
| payer_type | ENUM | — | Y | T2 | Assumed; sponsors Open (OQ-85) | GUARDIAN, SPONSOR_ORGANISATION. |
| payer_person_id | UUID | FK:Person | C | T2 | Confirmed (DL-55) | Required for GUARDIAN. Must hold a VERIFIED ST05 to every student billed to the account (SR-FB-05). |
| sponsor_name / sponsor_contact | VARCHAR / JSON | — | C | T2 | Assumed | Required for SPONSOR_ORGANISATION. Sponsors never get a parent-app login (OQ-85). |
| billing_address | TEXT | — | Y | T2 | Assumed | Defaults from the guardian's current address (ST06). |
| tax_registration_no | VARCHAR(32) | — | N | T2 | Open (OQ-87) | For sponsors who need tax invoices. |
| shared_with_person_ids | UUID[] | — | N | T2 | Confirmed (DL-85) | Other verified guardians the payer lets see the account. |
| statement_language | VARCHAR(8) | — | N | T1 | Assumed | From the guardian's preferred language (ST04). |
| fee_status | ENUM | — | Y | T2 | Confirmed; derived (DL-83) | Worst status across the account's open invoices. |
| balance | DECIMAL | — | Y | T2 | Assumed; derived | Open invoice totals minus unallocated payments and credits. |
| account_status | ENUM | — | Y | T1 | Assumed | ACTIVE, CLOSED. |

### FB06 — Billing Responsibility

**Purpose:** Which account pays which share of which student's fees, for which dates.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| responsibility_id | UUID | PK | Y | T2 | Confirmed (DL-79) | |
| enrolment_id | UUID | FK:ST02 | Y | T1 | Assumed | Per enrolment, so a new year or campus can change who pays. |
| billing_account_id | UUID | FK:FB05 | Y | T2 | Assumed | |
| share_percent | DECIMAL(5,2) | — | Y | T2 | Assumed | Shares for the same fee types add up to 100 (SR-FB-04). |
| fee_type_ids | UUID[] | — | N | T2 | Assumed | Empty = all fee types. Lets an employer pay tuition while a parent pays bus. |
| valid_from / valid_to | DATE | — | Y / N | T1 | Assumed | Changes take effect from the next billing period. |
| set_by_user_id | UUID | FK:A01 | Y | T1 | Assumed | Finance, from the admission contract (AD08) or a parent request. |

### FB07 — Fee Adjustment

**Purpose:** A dated reduction or waiver for one student, such as a sibling discount or a scholarship.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| adjustment_id | UUID | PK | Y | T2 | Confirmed (DL-81) | |
| enrolment_id | UUID | FK:ST02 | Y | T1 | Assumed | |
| adjustment_type | ENUM | — | Y | T2 | Known: SCHOLARSHIP (Arch § Data model; HB p.236); others Open (OQ-83) | See enums. |
| fee_type_ids | UUID[] | — | Y | T2 | Assumed | Usually tuition only. |
| method | ENUM | — | Y | T2 | Assumed | PERCENT, FIXED_AMOUNT. |
| value | DECIMAL | — | Y | T2 | Open (OQ-83) | |
| valid_from / valid_to | DATE | — | Y | T2 | Assumed | Scholarships often for one year. |
| reason | TEXT | — | Y | T2 | Assumed | Shown to Finance only; parents see the type name. |
| scholarship_ref | VARCHAR | — | C | T2 | Assumed | Award reference from the scholarship office (outside this module). |
| status | ENUM | — | Y | T1 | Assumed (DL-27) | REQUESTED, APPROVED, REJECTED, ENDED. Only APPROVED applies. |
| approved_by_user_id | UUID | FK:A01 | C | T2 | Open approver (OQ-83) | |

### FB08 — Billing Run

**Purpose:** Raising one period's invoices for one campus as a reviewed batch.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| billing_run_id | UUID | PK | Y | T2 | Confirmed (DL-80) | |
| campus_id | UUID | FK:OR04 | Y | T1 | Assumed | |
| billing_period_id | UUID | FK:FB04 | Y | T1 | Assumed | One APPLIED run per campus and period, plus supplementary runs for late joiners. |
| run_type | ENUM | — | Y | T1 | Assumed | REGULAR, SUPPLEMENTARY. |
| run_status | ENUM | — | Y | T1 | Assumed (DL-27) | See enums. |
| exceptions | JSON | — | Y | T2 | Assumed; derived | Students the run can't bill: no billing account, shares not 100, plan "Not set", tax code "Not set". Must be cleared or excluded before approval. |
| totals | JSON | — | Y | T2 | Assumed; derived | Invoice count, gross, adjustments, tax, net, per currency. |
| prepared_by_user_id | UUID | FK:A01 | Y | T1 | Assumed | Finance executive. |
| approved_by_user_id / approved_at | — | — | C | T1 | Assumed (Arch: Finance Controller "invoicing") | |
| issued_at | TIMESTAMP | — | C | T1 | Assumed | Invoices become visible to parents. |

### FB09 — Invoice

**Purpose:** What one account owes for one period or event.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| invoice_id | UUID | PK | Y | T2 | Known entity (Arch § Data model) | Referenced by FB10, FB12, FB13, FB15 and AD09 `invoice_ref`. |
| invoice_number | VARCHAR(24) | UK per issuer | Y | T2 | Assumed | Sequential per legal entity; format Open (OQ-87 e-invoicing). |
| billing_account_id | UUID | FK:FB05 | Y | T2 | Assumed | |
| issuer_legal_entity_id | UUID | FK:OR07 | Y | T1 | Known slot (DL-41 `operating_entity_id`) | From the enrolment's operating entity; named once OQ-06 is answered. |
| campus_id | UUID | FK:OR04 | Y | T1 | Assumed | |
| source | ENUM | — | Y | T1 | Confirmed (DL-80) | See enums. |
| billing_run_id / billing_period_id | UUID | FK:FB08 / FB04 | C | T1 | Assumed | For BILLING_RUN invoices. |
| invoice_date / due_date | DATE | — | Y | T2 | Assumed | |
| currency | CHAR(3) | — | Y | T2 | Known (MC-12) | |
| subtotal / adjustments / tax / total | DECIMAL | — | Y | T2 | Assumed; derived from FB10 | |
| amount_paid / amount_credited / amount_due | DECIMAL | — | Y | T2 | Assumed; derived from FB12, FB13 | |
| invoice_status | ENUM | — | Y | T2 | Assumed (DL-27) | See enums. OVERDUE is set by the system the day after `due_date`. |
| issued_at | TIMESTAMP | — | C | T2 | Assumed | After this, lines are read-only (SR-FB-07). |
| voided_reason | TEXT | — | C | T2 | Assumed | Void only before any payment; after that use a credit note. |

### FB10 — Invoice Line

**Purpose:** One fee for one student on an invoice.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| invoice_line_id | UUID | PK | Y | T2 | Assumed | |
| invoice_id | UUID | FK:FB09 | Y | T2 | Assumed | |
| enrolment_id | UUID | FK:ST02 | Y | T1 | Assumed | Shows the student's name and class on the invoice. |
| fee_type_id | UUID | FK:FB01 | Y | T1 | Assumed | |
| plan_line_id | UUID | FK:FB03 | C | T1 | Assumed | Which plan version priced it. Empty for one-off charges. |
| fee_requirement_id | UUID | FK:AD09 | C | T2 | Known link (AD09) | Admission fee lines. |
| description | VARCHAR(160) | — | Y | T2 | Assumed | e.g. "Tuition, Term 3 (Dec 2026–Mar 2027)". |
| quantity / unit_amount | DECIMAL | — | Y | T2 | Assumed | Proration uses quantity (months billed / months in period). |
| share_percent | DECIMAL(5,2) | — | Y | T2 | Assumed (FB06) | |
| adjustment_id / adjustment_amount | UUID / DECIMAL | FK:FB07 | C | T2 | Confirmed (DL-81) | |
| tax_code / tax_amount | — | — | Y | T2 | Open rates (OQ-87) | |
| line_total | DECIMAL | — | Y | T2 | Assumed; derived | |

### FB11 — Payment

**Purpose:** Money received from a payer.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| payment_id | UUID | PK | Y | T2 | Known entity (Arch § Data model) | |
| billing_account_id | UUID | FK:FB05 | Y | T2 | Assumed | |
| channel | ENUM | — | Y | T2 | Known: online gateway (Arch § Integrations "cards, PayNow in Singapore, UPI in India"); others Open (OQ-82) | See enums. |
| method | ENUM | — | Y | T2 | Known: CARD, PAYNOW, UPI; others Open (OQ-82) | |
| gateway_reference | VARCHAR(64) | — | C | T2 | Confirmed (DL-82) | Required for GATEWAY. No card number is ever stored. |
| amount / currency | DECIMAL / CHAR(3) | — | Y | T2 | Assumed | |
| surcharge_amount | DECIMAL | — | N | T2 | Known that card charges exist (HB p.246 "charges for credit card payment"); policy Open (OQ-82) | |
| payment_status | ENUM | — | Y | T2 | Assumed (DL-27) | See enums. |
| received_at | TIMESTAMP | — | Y | T2 | Assumed | Gateway confirmation time or the date Finance records. |
| recorded_by_user_id | UUID | FK:A01 | C | T2 | Assumed | Required for offline payments. |
| receipt_number | VARCHAR(24) | UK per issuer | C | T2 | Known need (MyGIIS payment history, HB p.34) | Issued when SUCCEEDED. |
| reversal_reason | TEXT | — | C | T2 | Assumed | Bounced cheque, chargeback. |

### FB12 — Payment Allocation

**Purpose:** How much of a payment settles which invoice.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| allocation_id | UUID | PK | Y | T2 | Confirmed (DL-82) | |
| payment_id | UUID | FK:FB11 | Y | T2 | Assumed | |
| invoice_id | UUID | FK:FB09 | Y | T2 | Assumed | Same billing account and currency. |
| amount | DECIMAL | — | Y | T2 | Assumed | Sum per payment ≤ payment amount; the rest stays as account credit. |
| allocated_by | ENUM | — | Y | T1 | Assumed | AUTO_OLDEST_FIRST, PARENT_CHOSE, FINANCE. |

### FB13 — Credit Note

**Purpose:** A correction that reduces what an issued invoice asks for.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| credit_note_id | UUID | PK | Y | T2 | Confirmed (DL-80) | |
| credit_note_number | VARCHAR(24) | UK per issuer | Y | T2 | Assumed | |
| invoice_id | UUID | FK:FB09 | Y | T2 | Assumed | |
| lines | JSON | — | Y | T2 | Assumed | Which invoice lines and how much. |
| reason_code | ENUM | — | Y | T2 | Assumed (DL-27) | BILLING_ERROR, WITHDRAWAL_PRORATION, LATE_ADJUSTMENT, GOODWILL, REFUND. |
| reason_note | TEXT | — | Y | T2 | Assumed | |
| total | DECIMAL | — | Y | T2 | Assumed | Can't exceed the invoice total minus earlier credits. |
| approved_by_user_id | UUID | FK:A01 | Y | T2 | Assumed; approver above a threshold Open (OQ-83) | |

### FB14 — Refund

**Purpose:** Money paid back to a payer, with how it was worked out.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| refund_id | UUID | PK | Y | T2 | Known entity (Arch § Data model "Refund case") | |
| case_id | UUID | FK:CC03 | Y | T2 | Confirmed (DL-08, DL-84) | Approval steps, documents, deadline and escalation run on the shared Case. |
| billing_account_id | UUID | FK:FB05 | Y | T2 | Assumed | |
| enrolment_id | UUID | FK:ST02 | Y | T1 | Assumed | |
| refund_basis | ENUM | — | Y | T2 | Known for GIIS SG (HB p.240–244) | See enums. |
| notice_date | DATE | — | Y | T2 | Known (HB p.241: refund within 7 working days of written notice) | Written notice of withdrawal, or the date the school learned of non-delivery. |
| contract_signed_date | DATE | — | C | T2 | Known (AD08; cooling-off counts from signature by both parties, HB p.241) | |
| rule_set_version | VARCHAR(32) | — | Y | T1 | Confirmed (DL-84) | e.g. "GIIS SG refund rules 2025-26". |
| calculation | JSON | — | Y | T2 | Assumed | Per fee: paid, refund %, admin charge, amount. Table 3A/3B percentages Open (OQ-86). |
| refund_amount | DECIMAL | — | Y | T2 | Assumed; derived | Finance can change it only with a reason (humanitarian). |
| refund_status | ENUM | — | Y | T2 | Assumed (DL-27) | See enums. |
| approvals | JSON | — | Y | T1 | Known for GIIS SG (HB p.244: committee of Country Director and Finance Controller) | Approver roles per brand-country setting. |
| documents_requested_at | DATE | — | C | T2 | Known (HB p.245: notify within 3 working days; parent has 7 working days; else forfeited unless extended) | |
| due_by | DATE | — | Y | T2 | Known for GIIS SG (7 working days, HB p.241; Arch § Key workflows) | Escalation: Finance Controller, then Country Director after 4 working days (HB p.245). |
| payout_method / payout_account_masked | ENUM / VARCHAR | — | C | T2 | Confirmed (DL-82) | Bank details masked; full value only to `fees.refund.pay`. |
| paid_at | TIMESTAMP | — | C | T2 | Assumed | |

### FB15 — Payment Reminder

**Purpose:** A reminder sent to a payer about an invoice that is due or overdue.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| reminder_id | UUID | PK | Y | T2 | Assumed | |
| invoice_id | UUID | FK:FB09 | Y | T2 | Assumed | |
| step | VARCHAR(24) | — | Y | T1 | Open schedule (OQ-84) | e.g. BEFORE_DUE_7D, OVERDUE_7D. From the campus reminder setting. |
| channel | ENUM | — | Y | T1 | Assumed | PARENT_APP, EMAIL, SMS, WHATSAPP (channels from ST04 contacts). |
| sent_at | TIMESTAMP | — | Y | T1 | Assumed | Sent through Engagement messaging. |
| sent_by | ENUM | — | Y | T1 | Assumed | SCHEDULE, FINANCE_MANUAL. |

### FB16 — Billing Pause

**Purpose:** A stretch of time a student is not billed, such as approved long leave.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| pause_id | UUID | PK | Y | T2 | Confirmed (DL-86) | |
| enrolment_id | UUID | FK:ST02 | Y | T1 | Assumed | |
| reason | ENUM | — | Y | T2 | Known: LONG_LEAVE (HB p.237); others Assumed | LONG_LEAVE, OTHER. |
| pause_from / pause_to | DATE | — | Y | T2 | Known need (HB p.237) | Billing runs skip covered months. |
| case_id | UUID | FK:CC03 | Y | T2 | Known (HB p.239: Principal processes, Associate Director Academic Quality Assurance approves at GIIS SG) | The approved deferment. |

### FB17 — Fee Protection Record

**Purpose:** The fee protection insurance that covers a student's fees where the regulator requires it.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| fps_record_id | UUID | PK | Y | T2 | Known (HB p.246; regulatory-context FPS) | |
| enrolment_id | UUID | FK:ST02 | Y | T1 | Known | |
| insurer_name | VARCHAR(80) | — | Y | T1 | Known for GIIS SG (HB p.246 names the insurer) | Brand-country setting. |
| certificate_number | VARCHAR(40) | — | C | T2 | Known (Arch § Integrations "fee protection certificates") | |
| cover_from / cover_to | DATE | — | Y | T2 | Known (HB p.247: fees up to 12 months of course duration) | |
| fps_status | ENUM | — | Y | T1 | Known events (HB p.237: cancelled by Finance on withdrawal; p.238 purchased after transfer) | REQUIRED, PURCHASED, CANCELLED. |
| covered_amount | DECIMAL | — | N | T2 | Assumed | Sum of FPS-covered fees (FB01 `fps_covered`). |

## Billing lifecycle and how it maps to other modules

| Moment | Fees record | Other module | Rule |
|---|---|---|---|
| Plan next year | FB02 DRAFT → APPROVED (Finance Controller) | Admissions shows the annual fee on the offer | DL-78 |
| Application or offer accepted | FB09 source ADMISSION, lines from AD09 | AD09 `payment_status` follows the invoice | SR-FB-09, SR-AD-04 |
| Contract signed | FB05/FB06 set from the contract payer | AD08 SIGNED | DL-79 |
| ICA/MOE rejects | FB14 basis APPROVAL_REJECTED (registration fee less SGD 50, GIIS SG) | AD10 REJECTED | SR-AD-06, HB p.242–243 |
| Student enrolled | FB17 REQUIRED where FPS applies | StudentEnrolled (ST02) | HB p.246 |
| Term starts | FB08 run → APPROVED → ISSUED; FB09 per account | Parent app shows "What's due" | DL-80 |
| Mid-term joiner | FB08 SUPPLEMENTARY, prorated lines | StudentEnrolled | SR-FB-08 |
| Long leave approved | FB16 pause | Deferment case (Engagement) | DL-86 |
| Withdrawal approved by Principal | Fee clearance: open invoices listed; FB13 proration; FB14 refund on a Case; FB17 CANCELLED | Withdrawal case; EnrolmentEnded | HB p.237, DL-84 |
| Invoice overdue | FB09 OVERDUE; FB15 reminders; enrolment fee status OVERDUE | Readers: Admissions, withdrawal clearance, Principal summary | DL-83 |

## Field visibility by role

`M` manage, `A` approve, `V` view, `S` summary (counts and totals), `own` the viewer's own accounts, `—` none. All cells Assumed (DL-26 rule) except where cited.

| Record | Finance executive (campus) | Finance Controller (brand-country) | Country Director (brand-country) | Principal (campus) | Admissions officer (campus) | Class / subject teacher | Parent who pays | Other verified guardian | Student | Group leadership |
|---|---|---|---|---|---|---|---|---|---|---|
| Fee types, fee plans | M (prepare) | A (Known: Arch "Fee plans") | V | V | V (annual totals for offers) | — | published fee schedule | published fee schedule | — | S |
| Billing accounts, payers, shares | M | V | — | — | V (payer named on the contract) | — | own | shared only (DL-85) | — | — |
| Billing runs | M (prepare) | A (Known: Arch "invoicing") | — | S | — | — | — | — | — | S |
| Invoices, payments, receipts, credit notes | M | V; A credit notes over threshold (OQ-83) | — | S | V (admission fee invoices only) | — | own: view, pay, download | shared only | — (DL-68) | S |
| Discounts and scholarships | M (request) | A (OQ-83) | — | V | — | — | own (type and amount, not the reason) | — | — | S |
| Fee status per enrolment | V | V | V | V | V (own applicants) | — (DL-83) | own | — | — | S |
| Refund calculation and approval | M | A (Known GIIS SG, HB p.244) | A (Known GIIS SG, HB p.244) | V (withdrawals they approved) | — | — | own: status and amount | — | — | S |
| Refund bank details | V masked; reveal with `fees.refund.pay` | V masked | — | — | — | — | own | — | — | — |
| Fee protection record | M | V | — | — | — | — | own certificate | — | — | S |

## Enumerations (candidates, DL-27 rule)

| Entity | Field | Values |
|---|---|---|
| FB01 | fee_category | TUITION, ADMISSION, RECURRING_SERVICE, ONE_OFF, DEPOSIT |
| FB01 | refund_class | REFUNDABLE, NON_REFUNDABLE, REFUNDABLE_ON_APPROVAL_REJECTED |
| FB02 | plan_status | DRAFT, SUBMITTED, APPROVED, SUPERSEDED |
| FB03 | charge_frequency | PER_BILLING_PERIOD, ANNUAL, MONTHLY, ONCE |
| FB03 | applies_to | ALL_STUDENTS, NEW_STUDENTS_ONLY, OPT_IN |
| FB05 | payer_type | GUARDIAN, SPONSOR_ORGANISATION |
| FB05 / ST02 | fee_status | CLEAR, DUE, OVERDUE, PAUSED |
| FB07 | adjustment_type | SIBLING, STAFF_CHILD, SCHOLARSHIP, BURSARY, WAIVER |
| FB07 | status | REQUESTED, APPROVED, REJECTED, ENDED |
| FB08 | run_status | DRAFT, NEEDS_REVIEW, READY_FOR_APPROVAL, APPROVED, ISSUED, CANCELLED |
| FB09 | source | BILLING_RUN, ADMISSION, ONE_OFF, PRORATION |
| FB09 | invoice_status | DRAFT, ISSUED, PARTIALLY_PAID, PAID, OVERDUE, VOID, CREDITED |
| FB11 | channel | GATEWAY, BANK_TRANSFER, COUNTER, CHEQUE |
| FB11 | method | CARD, PAYNOW, UPI, BANK_TRANSFER, CASH, CHEQUE |
| FB11 | payment_status | PENDING, SUCCEEDED, FAILED, REVERSED |
| FB14 | refund_basis | COOLING_OFF, NON_DELIVERY (Table 3A), OTHER_REASONS (Table 3B), APPROVAL_REJECTED, HUMANITARIAN |
| FB14 | refund_status | CALCULATING, AWAITING_DOCUMENTS, AWAITING_APPROVAL, APPROVED, PAID, REJECTED, FORFEITED |
| FB17 | fps_status | REQUIRED, PURCHASED, CANCELLED |

## Business rules

- **SR-FB-01 (Confirmed, DL-77):** Campus OS is the only writer of fee plans, invoices, payments, credit notes and refunds. Every posting emits a journal to the brand-country's ERP; the ERP never writes back to an invoice.
- **SR-FB-02 (Confirmed, DL-78; DL-59 pattern):** Fee amounts, billing periods, due dates, tax codes, reminder steps and refund rules are set per brand-country or campus, with no group default. A value that isn't set shows "Not set" and blocks the step that needs it (approving a plan, approving a run, calculating a refund).
- **SR-FB-03 (Known Arch § Tenancy; Confirmed DL-78):** An approved fee plan is never edited. A change is a new version with a valid-from date and a reason; invoices already issued keep the version they used.
- **SR-FB-04 (Confirmed, DL-79):** For each enrolment and fee type, the shares across billing accounts add up to 100%. A student without a full 100% shows as a run exception and is not billed until fixed.
- **SR-FB-05 (Confirmed, DL-79; DL-25):** A guardian can be a payer only for students they have a VERIFIED relationship (ST05) with. A sponsor organisation needs Finance to record the sponsor letter (OQ-85).
- **SR-FB-06 (Confirmed, DL-80):** Term invoices are issued only from an approved billing run. One REGULAR run per campus and billing period; later joiners go in SUPPLEMENTARY runs.
- **SR-FB-07 (Confirmed, DL-80):** An issued invoice is never edited. Before any payment it may be voided with a reason; after that, only a credit note reduces it.
- **SR-FB-08 (Assumed):** A student who joins or leaves mid-period is billed by whole months by default (GIIS SG bills "till September" for a 20 September withdrawal, HB p.244). The proration rule is a campus setting.
- **SR-FB-09 (Known SR-AD-04; Assumed):** Admission fees (AD09) are invoiced as soon as they are due; AD09 `payment_status` mirrors the invoice: PAID, WAIVED (fully credited) or REFUNDED.
- **SR-FB-10 (Confirmed, DL-82):** An online payment counts only when the gateway confirms it. A payment is allocated to the oldest due invoice first unless the parent picked invoices or Finance reallocates. Leftover money stays as account credit.
- **SR-FB-11 (Confirmed, DL-83):** Fee status per enrolment is CLEAR, DUE, OVERDUE or PAUSED, derived from its invoice lines. No module may block attendance, lessons, results or app access on fee status without a campus setting that names the consequence (OQ-84).
- **SR-FB-12 (Known GIIS SG HB p.237; Assumed elsewhere):** A withdrawal can't reach final clearance while the enrolment's fee status is DUE or OVERDUE, unless Finance records a write-off or a refund offset.
- **SR-FB-13 (Known GIIS SG HB p.241–242):** Registration and application fees are not refundable, except for new admissions when ICA rejects the Student Pass or MOE rejects a Singapore citizen's application, less an administrative charge of SGD 50 (HB p.243).
- **SR-FB-14 (Known GIIS SG HB p.241):** A written withdrawal within 7 working days of the contract being signed by both parties is refunded under Table 3A, whether or not the course has started.
- **SR-FB-15 (Known GIIS SG HB p.241, p.244–245):** A refund is paid within 7 working days of the notice; missing documents are requested within 3 working days and due within 7, or the refund is forfeited unless the Finance Controller and Country Director extend; late refunds escalate to the Finance Controller, then the Country Director after 4 working days.
- **SR-FB-16 (Known GIIS SG HB p.244):** A withdrawal approved at least one month before a term starts is not billed for that term.
- **SR-FB-17 (Known GIIS SG HB p.237; Confirmed DL-86):** No fees are billed for months covered by an approved long-leave pause.
- **SR-FB-18 (Known SG regulator pack, HB p.246–247):** Where fee protection applies, each enrolled student has an FPS record; it is cancelled by Finance on withdrawal and purchased again after a course change.
- **SR-FB-19 (Confirmed, DL-85; DL-68):** Parents see finance only for accounts they pay or that a payer shares with them. Students never see fees.
- **SR-FB-20 (Confirmed, DL-82):** Card numbers are never stored. Refund bank details are masked and revealed only to holders of `fees.refund.pay`; every reveal is logged.

## Events

FeePlanApproved, BillingRunApproved, InvoiceIssued (Engagement notifies payers), InvoiceOverdue, PaymentReceived, AdmissionFeePaid (Admissions, already listed there), PaymentReversed, CreditNoteIssued, FeeStatusChanged (per enrolment), RefundApproved, RefundPaid, BillingPaused, FpsRecordChanged, JournalPosted (ERP). Events carry IDs only (architecture.md).

## Capabilities (new, DL-53 naming)

`fees.catalogue.manage` · `fees.plan.edit`, `.approve` · `fees.account.view`, `.manage` · `fees.adjustment.request`, `.approve` · `fees.run.prepare`, `.approve` · `fees.invoice.view`, `.issue_one_off`, `.void` · `fees.credit_note.create`, `.approve` · `fees.payment.record`, `.reverse` · `fees.status.view` (fee status only, for Admissions and Principals) · `fees.refund.prepare`, `.approve`, `.pay` · `fees.fps.manage`. Parent actions (view own invoices, pay, download receipts, share an account) are added to the parent capability set R03 through the payer path (DL-85).

## Decisions

| ID | Decision | Why |
|---|---|---|
| DL-77 (**Confirmed**, Henry chose option A, 14 Nov 2026) | Campus OS owns billing: fee plans, invoices, payments, credit notes and refunds are written in the Fees and Finance domain (DL-43), capability prefix `fees`. The ERP receives journals and never writes back | Admissions, withdrawal and the parent app need live paid/overdue status in one place (Arch § Integrations already lists outbound journals) |
| DL-78 (**Confirmed**, Henry, 15 Nov 2026) | Fee types are a brand-country list; amounts are set per campus in fee plans per offering × grade(s) × academic year; the Finance Controller approves; approved plans are versioned, never edited; no group default amounts | Arch § Tenancy (fee plans per campus, effective dating); DL-59 pattern |
| DL-79 (**Confirmed**, Henry, 15 Nov 2026) | Billing account = one payer in one brand-country (verified guardian or sponsor organisation); each enrolment links to accounts with a share and fee types; siblings across that brand-country's campuses share one statement; the invoice issuer is the enrolment's operating legal entity | Handles split parents, employer-paid tuition and siblings at two campuses (Aarav at Punggol, Ishita at East Coast) without copying families |
| DL-80 (**Confirmed**, Henry, 15 Nov 2026) | Invoices come from approved billing runs (per campus × period) or defined events (admission fee, one-off charge, proration). Issued invoices are never edited; corrections are credit notes | One reviewed batch per term; a clean audit trail for regulators and the ERP |
| DL-81 (**Confirmed**, Henry, 15 Nov 2026) | Discounts, scholarships, bursaries and waivers are dated adjustments with an approver, applied when invoices are built and shown on the line | Parents see the full fee and the reduction; scholarship selection stays outside |
| DL-82 (**Confirmed**, Henry, 15 Nov 2026) | Gateway confirmation is the truth for online payments; Finance records offline payments with a receipt; oldest-due-first allocation by default; card numbers never stored; refund bank details masked | Fewer mismatches; keeps card data out of Campus OS |
| DL-83 (**Confirmed**, Henry chose A, 14 Nov 2026) | One derived fee status per enrolment (CLEAR, DUE, OVERDUE, PAUSED) that other modules read; it causes no automatic sanction; teachers don't see it | Admissions and withdrawal need it; children shouldn't lose learning because of an automatic rule (OQ-84 decides any consequence) |
| DL-84 (**Confirmed**, Henry, 15 Nov 2026) | Refunds are calculated from a versioned refund rule set per brand-country and the refund records which version it used; approval, documents and the deadline run on the shared Case (DL-08); approver roles are a brand-country setting (GIIS SG: Country Director + Finance Controller) | Arch § Principles "refund tables become versioned rules"; one SLA engine for cases |
| DL-85 (**Confirmed**, Henry chose A, 14 Nov 2026) | Parents see and pay only accounts where they are a payer, or that a payer shared with them; other verified guardians see no finance; students never see fees | Separated parents and sponsors make "every linked guardian sees fees" unsafe |
| DL-86 (**Confirmed**, Henry, 15 Nov 2026) | An approved long leave creates a billing pause; billing after withdrawal follows the campus's cut-off setting (GIIS SG: approved ≥ 1 month before term = not billed) | HB p.237 and p.244 rules, kept as GIIS SG settings |

## Open questions (new)

| ID | Question | Why it matters for design |
|---|---|---|
| OQ-80 | Which fee types and billing frequencies does each brand-country use (term, monthly, annual upfront), and are there early-payment or annual-payment discounts? | Fee plan editor, billing periods and the parent fee schedule |
| OQ-81 | Which accounting system or ERP does each brand-country use, and what journal detail and chart of accounts does it need? | Ledger codes on fee types and the journal export |
| OQ-82 | Which payment methods and gateways does each country use (cards, PayNow, UPI, GIRO, bank transfer, cheque, cash), and who pays card charges? | Pay screen, counter screen and surcharges |
| OQ-83 | What discounts exist per brand (sibling, staff child, early bird), do they stack, and who approves discounts, scholarships, waivers and credit notes, up to what amount? | Adjustment approval steps and limits |
| OQ-84 | What are the due dates and reminder schedule, are late fees charged, and does an overdue balance have any consequence (report card, exam slip, re-enrolment, transfer certificate)? | Reminder settings and what other modules may do with fee status |
| OQ-85 | Who can be a payer: one or both parents, a split, an employer, an embassy or a scholarship body; do sponsors get statements or a login? | Billing account setup and the payer picker |
| OQ-86 | What are the Table 3A and 3B percentages in the GIIS SG contract (the handbook shows them only as images), and what refund rules, approvers and deadlines apply in other brand-countries? | Refund calculator; only GIIS SG text rules are known |
| OQ-87 | Which fees carry GST or VAT per country, at what rate, and is e-invoicing required (for example India GST e-invoices)? | Tax lines, invoice numbering and format |
| OQ-88 | Which countries besides Singapore require fee protection, escrow or deposit protection, and what must be recorded? | Regulator packs and the FPS record |
| OQ-89 | Do any brands take security deposits or caution money, and when are they returned? | DEPOSIT fee category and exit clearance |
| OQ-90 | How much fee history moves from MyGIIS and the current finance systems at cutover, and are opening balances imported? | Parent "History" tab on day one (MyGIIS parity) |
| OQ-91 | How long must invoices, receipts and refund records be kept per country, and what may be deleted after a family leaves? | Retention policy for finance records (with OQ-10) |
| OQ-92 | Can a campus bill in a currency other than its country's (for example USD for some families), and who sets exchange rates? | Currency on plans, invoices and payments |

## Readiness

- **Can be designed now:** fee plan list and editor with "Not set" amounts, billing run review with exceptions, family account with siblings across campuses and split payers, invoice detail, record a counter payment, parent "What's due / Pay / History / Receipts", overdue list with reminders, refund case with the GIIS SG rule set, admission fee refund (Hana Kim: registration fee less SGD 50), long-leave pause, FPS records.
- **Keep generic, label Pending:** fee amounts (OQ-68, OQ-80), Table 3A/3B percentages (OQ-86), payment methods per country (OQ-82), discount rules and approvers (OQ-83), overdue consequences (OQ-84), sponsors (OQ-85), tax (OQ-87).

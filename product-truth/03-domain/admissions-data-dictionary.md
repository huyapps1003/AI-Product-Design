# Global Campus OS — Admissions Data Dictionary v0.1 (draft)

**Status:** Proposed draft, 12 Nov 2026. DL-56, DL-57, DL-58, DL-60 Confirmed (Henry, 14 Nov 2026); DL-59, DL-61 Assumed; OQ-62…OQ-72 Open · **Owner:** Henry
**Cites:** [Architecture v1](../00-sources/Global_Campus_OS_Product_Architecture_v1.md) § Users and roles, § Domains and modules (module 1), § Key workflows (Admission to enrolment; Fees, withdrawal and refund), § Integrations (ICA) · [Student Directory Spec](./student-directory-data-dictionary.md) ST01, ST02, ST04, ST05, ST08, ST09, ST11, SR-07 · [Organisation dictionary](./organisation-data-dictionary.md) OR04, OR07, OR08, OR09 · [architecture.md](./architecture.md) AR-xx · [operating-context](../01-context/operating-context.md) (enquiry.globalschools.com, MyGIIS) · [regulatory-context](../05-data-governance/regulatory-context.md) (CPE/EduTrust, ICA, FPS) · GIIS Student Handbook 2025-26 (HB) p.125, p.232–245, § SEN · DL-22, DL-24, DL-27, DL-29, DL-39…DL-45, DL-47, DL-53…DL-55 · MC-01, MC-05, MC-06, MC-07, MC-12, MC-14 · OQ-06, OQ-07, OQ-10, OQ-29, DL-63, DL-65, DL-67 (Henry, 13 Nov 2026)
**Tóm tắt (VI):** Từ điển dữ liệu cho module Tuyển sinh và Nhập học: từ lúc gia đình hỏi thông tin đến ngày đầu tiên đi học (hỏi thông tin, hồ sơ, đánh giá/xếp lớp, thư mời nhập học, hợp đồng và phí ghi danh, hồ sơ xin Student Pass/MOE, bàn giao sang hồ sơ học sinh). Mỗi trường có trạng thái Known / Assumed / Open.

> Drafted by Claude from product truth on 12 Nov 2026 after Henry approved the scope the same day. Henry confirmed DL-56, DL-57, DL-58 and DL-60 on 14 Nov 2026.

## How to read this file

Same layout and rules as the [Student Directory Spec](./student-directory-data-dictionary.md#how-to-read-this-file): **Known** (cited), **Assumed** (DL), **Open** (OQ); tiers T1/T2/T3 (DL-11), `T2*` = tier assigned by assumption (DL-26 rule applied here); `Y`/`N`/`C` required. Every row also carries `tenant_path`, `data_region` (UI term **Data location**, DL-47) and the shared common fields. Facts from the GIIS Handbook are **GIIS Singapore examples**, not group rules (DL-22). All enums are unapproved candidates (DL-27 rule). All example values are synthetic.

## Scope

Approved by Henry, 12 Nov 2026: **a new family's path from first enquiry to first day.**

| In this dictionary | Not in this dictionary (owner) |
|---|---|
| Enquiry, and matching it to an existing Person | Withdrawal and refund cases (Fees and Finance + Engagement cases) |
| Application per child, document checklist | Campus transfers, promotion, year-end rollover (Student Records; DL-54) |
| Assessment and placement (entrance test, AEPP placement, SEN pre-admission evaluation) | Fee plans, invoices, payments (Fees and Finance); this file only records that a fee is due and whether it is paid |
| Intake capacity and waitlist | Marketing campaigns and lead nurturing |
| Offer and acceptance | SEN profile, evaluation notes (Wellbeing and Inclusion, Tier 3) |
| Student contract and cooling-off | Student and guardian master data after handoff (Student Directory ST01–ST11) |
| Government approval case (ICA Student Pass, MOE) | Logins and Family Link creation mechanics (Identity and Access Registry) |
| Enrolment handoff | |

## Modeling boundaries

- **Enquiry is per family, application is per child (DL-56).** One enquiry may name several children and several campuses. Each child applies once per target (campus × programme offering × grade × academic year). A family that applies to two campuses has two applications for the same child.
- **One Person, never auto-merged (DL-42, DL-58).** The enquirer and the child are matched to existing Persons by ID-based checks; possible matches go to the duplicate review queue. A sibling or returning family reuses the existing guardian Person.
- **When a student record exists (DL-57).** Before an offer, the child is a Person plus applicant fields on the application. Issuing an offer creates the Student (ST01) and an Enrolment (ST02) in status `OFFERED`, which the Student Directory already defines. That is why ST02 has `OFFERED` and `CONFIRMED`.
- **Admissions writes, Student Records holds (OBJ-03, AR-06).** After handoff, profile edits go through the Student Directory, not the application.
- **Rules are settings, GIIS Singapore is one example (DL-59).** Cooling-off days, fees due at admission, required documents, required assessments and offer expiry are settings on the scope tree (MC-01, MC-14). The Handbook values are the GIIS Singapore defaults in samples, not group rules.
- **Restricted outcomes only (DL-60).** SEN pre-admission evaluation notes and immigration documents stay Tier 3 with their owners. The application holds only an outcome code and a link.
- **Data location.** An enquiry is held in the data location of the campus it is routed to. An enquiry not yet routed (group enquiry site) is held where the group site's data is held (OQ-29, OQ-63).

## Entities

| ID | Entity | Module | Status |
|---|---|---|---|
| AD01 | Enquiry | Admissions | Known need (Arch § Admission to enrolment step 1; Arch § Users and roles "Enquiries"; operating-context enquiry site); fields Assumed |
| AD02 | Enquiry Child | Admissions | Assumed (DL-56) |
| AD03 | Application | Admissions | Known (Arch step 1; module 1 "Enquiry, application, AEPP") |
| AD04 | Application Document | Admissions | Known need (Arch § Fees "documents checklist" pattern; HB p.35 student particulars); list Open (OQ-72) |
| AD05 | Admission Assessment | Admissions (outcome) / Wellbeing (SEN notes) | Known (Arch step 1 "assessment… AEPP placement"; HB p.125 placement test; HB § SEN pre-admission evaluation) |
| AD06 | Intake Capacity | Admissions | Assumed (DL-61); rules Open (OQ-65) |
| AD07 | Offer | Admissions | Known (Arch step 2) |
| AD08 | Student Contract | Admissions | Known (Arch step 2; HB p.232–236; ST02 `contract_id`) |
| AD09 | Admission Fee Requirement | Admissions (status) / Fees and Finance (invoice, payment) | Known (Arch step 2 "registration fee"; HB p.242) |
| AD10 | Government Approval Case | Admissions on Case (OBJ-10) | Known (Arch step 3; HB p.236, p.242; ST09 `approval_case_id`) |

ST01, ST02, ST04, ST05, ST08, ST09 and ST11 are referenced, not redefined.

## Detailed attribute dictionary

### AD01 — Enquiry

**Purpose:** First contact from a family, before any child applies.
**Data controller:** "[data controller — OQ-06]"; operating entity copied from the routed campus (DL-41). Unrouted enquiries: OQ-63.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| enquiry_id | UUID | PK | Y | T1 | Assumed (DL-56) | |
| enquiry_number | VARCHAR(20) | UK | Y | T1 | Open (format, OQ-55 pattern) | Shown to staff and the family. |
| source_channel | ENUM | — | Y | T1 | Known (enquiry site, operating-context); values Assumed (DL-27) | See enums. |
| source_detail | VARCHAR(120) | — | N | T1 | Assumed | Event name, referrer, agent name (OQ-70). |
| enquirer_person_id | UUID | FK:Person | Y | T1 | Confirmed rule (DL-42) | Matched or new Person; never auto-merged (DL-58). |
| enquirer_name | VARCHAR(160) | — | Y | T1 | Assumed | As typed; kept for the enquiry even if the Person later changes. |
| enquirer_relationship | ENUM | — | N | T1 | Assumed | Reuses ST05 relationship values. |
| contact_email / contact_phone | VARCHAR | — | C | T2* | Assumed (DL-26 rule) | At least one required. |
| preferred_language | VARCHAR(10) | — | N | T1 | Assumed (MC-12) | |
| brand_id | UUID | FK:OR02 | C | T1 | Known need (operating-context: "chosen school isn't carried") | Required once routed. |
| campus_id | UUID | FK:OR04 | C | T1 | Known need (same) | Routed campus. Several campuses: OQ-63. |
| assigned_to_user_id | UUID | FK:User | N | T1 | Assumed | Admissions officer who owns follow-up. |
| enquiry_status | ENUM | — | Y | T1 | Assumed (DL-27) | See enums. |
| next_action_at | TIMESTAMP | — | N | T1 | Assumed | Drives the follow-up queue. |
| visit_at | TIMESTAMP | — | N | T1 | Assumed | Campus tour or meeting. |
| lost_reason | ENUM | — | C | T1 | Assumed | Required when CLOSED_LOST. |
| marketing_consent | BOOLEAN | — | N | T2* | Open (OQ-10, OQ-69) | Not designed until the consent record exists. |
| received_at | TIMESTAMP | — | Y | T1 | Known need | |

### AD02 — Enquiry Child

**Purpose:** A child named on an enquiry, before an application exists.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| enquiry_child_id | UUID | PK | Y | T1 | Assumed (DL-56) | |
| enquiry_id | UUID | FK:AD01 | Y | T1 | Assumed | |
| child_person_id | UUID | FK:Person | N | T1 | Confirmed rule (DL-42) | Set when matched or when an application starts. |
| first_name / last_name | VARCHAR(80) | — | Y | T1 | Assumed | |
| date_of_birth | DATE | — | N | T2* | Assumed | Drives grade suggestion; age-to-grade rules Open (OQ-64). |
| interested_grade_id | UUID | FK:Grade | N | T1 | Assumed | OBJ-15 (Academic structure, pending). |
| interested_offering_id | UUID | FK:Programme offering | N | T1 | Assumed | MC-07. |
| interested_academic_year_id | UUID | FK:Academic year | N | T1 | Assumed | MC-06: years start at different times. |
| application_id | UUID | FK:AD03 | N | T1 | Assumed | Set on conversion. |

### AD03 — Application

**Purpose:** One child's request for a seat at one target. The spine of the pipeline.
**Data controller:** "[data controller — OQ-06]"; `operating_entity_id` copied from the campus (DL-41). Held in the campus's data location.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| application_id | UUID | PK | Y | T1 | Assumed | |
| application_number | VARCHAR(20) | UK | Y | T1 | Open (format) | |
| enquiry_id | UUID | FK:AD01 | N | T1 | Assumed | Walk-in applications may have none. |
| child_person_id | UUID | FK:Person | Y | T1 | Confirmed rule (DL-42) | |
| student_id | UUID | FK:ST01 | C | T1 | Assumed (DL-57) | Set when the offer is issued; Student ID format per DL-67. A returning student or a year-end move into another brand or country (a new admission, DL-63) keeps the existing student (DL-31). |
| campus_id | UUID | FK:OR04 | Y | T1 | Known (Arch step 1) | |
| operating_entity_id | UUID | FK:OR07 | Y | T1 | Confirmed rule (DL-41) | Copied from OR08 at creation. |
| programme_offering_id | UUID | FK:Offering | Y | T1 | Known (MC-07) | AEPP is a placement inside a regular enrolment, never an offering (DL-65). |
| grade_id | UUID | FK:Grade | Y | T1 | Known | Applied-for grade. |
| academic_year_id | UUID | FK:Academic year | Y | T1 | Known need (DL-54) | |
| requested_start_date | DATE | — | Y | T1 | Assumed | Mid-year joiners are common in international schools (assumption, not sourced). |
| applicant_legal_first_name / last_name | VARCHAR(80) | — | Y | T1 | Assumed | Copied to ST01 at offer (DL-57). |
| applicant_date_of_birth | DATE | — | Y | T2* | Assumed | |
| applicant_gender | ENUM | — | N | T2* | Assumed | ST01 values. |
| applicant_nationality | CHAR(2)[] | — | Y | T2* | Known need (HB p.35: nationality drives Student Pass) | |
| needs_government_approval | ENUM | — | Y | T2 | Known (Arch step 3 "where needed"; HB p.242) | NONE, STUDENT_PASS, MOE_APPROVAL, OTHER (OQ-22 for other countries). |
| home_language | VARCHAR(35) | — | N | T2* | Open (OQ-37) | Possible AEPP input. |
| previous_school_name | VARCHAR(160) | — | N | T2* | Assumed | |
| previous_school_country | CHAR(2) | — | N | T2* | Assumed | |
| sen_disclosed | BOOLEAN | — | N | T3 | Known (HB § SEN: "information shared at the time of seeking admission") | Triggers AD05 SEN evaluation; Tier 3 (Arch § Security: SEN). |
| sibling_student_ids | UUID[] | — | N | T1 | Assumed | Derived from guardian links; priority rule Open (OQ-65). |
| primary_contact_guardian_person_id | UUID | FK:Person | Y | T1 | Assumed | Becomes ST04/ST05 at offer (DL-57); relationship unverified until ST05 verification (DL-25). |
| application_status | ENUM | — | Y | T1 | Assumed (DL-27) | See enums and SR-AD-01. |
| submitted_at | TIMESTAMP | — | C | T1 | Assumed | |
| submitted_channel | ENUM | — | Y | T1 | Open (OQ-62) | STAFF_ENTERED, PARENT_ONLINE, AGENT. |
| decision | ENUM | — | C | T2 | Assumed | OFFER, WAITLIST, REJECT. |
| decision_by_user_id | UUID | FK:User | C | T2 | Open (OQ-66: who decides) | |
| decision_reason | TEXT | — | C | T2 | Assumed | Required for REJECT. |
| waitlist_position | INT | — | C | T2 | Open (OQ-65) | Computed; ordering rule Open. |
| closed_reason | ENUM | — | C | T1 | Assumed | When WITHDRAWN or DECLINED. |

### AD04 — Application Document

**Purpose:** One item on the application's document checklist.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| application_document_id | UUID | PK | Y | T1 | Assumed | |
| application_id | UUID | FK:AD03 | Y | T1 | Assumed | |
| document_type | ENUM | — | Y | T1 | Open (list, OQ-72) | Required set comes from a setting (DL-59). |
| required | BOOLEAN | — | Y | T1 | Assumed (DL-59) | From the setting at the time the checklist was made. |
| document_status | ENUM | — | Y | T1 | Assumed | REQUESTED, RECEIVED, ACCEPTED, REJECTED, WAIVED. |
| file_ref | VARCHAR | — | C | per type | Assumed | Passport, birth certificate: T3 (Arch § Security). Report cards: T2. |
| identity_document_id | UUID | FK:ST08 | N | T3 | Assumed | Accepted passports become ST08 at offer. |
| reviewed_by_user_id / reviewed_at | — | — | C | T1 | Assumed | |

### AD05 — Admission Assessment

**Purpose:** A test, interview or evaluation that informs the decision or placement.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| assessment_id | UUID | PK | Y | T1 | Assumed | |
| application_id | UUID | FK:AD03 | Y | T1 | Assumed | |
| assessment_type | ENUM | — | Y | T1 | Known: AEPP_PLACEMENT (HB p.125), SEN_PRE_ADMISSION (HB § SEN); others Open (OQ-64) | ENTRANCE_TEST, INTERVIEW Assumed. |
| scheduled_at | TIMESTAMP | — | N | T1 | Assumed | |
| assessor_user_id | UUID | FK:User | N | T1 | Assumed | Teacher, coordinator or counsellor. |
| outcome | ENUM | — | C | T2 (T3 for SEN) | Assumed (DL-60) | Generic: MEETS, BELOW, PLACE_IN_AEPP. SEN: CAN_SUPPORT, NEEDS_DISCUSSION, CANNOT_SUPPORT. |
| score_summary | VARCHAR(120) | — | N | T2 | Assumed | Not used for SEN. |
| restricted_record_ref | UUID | FK:Wellbeing record | C | T3 | Assumed (DL-60) | SEN notes live in Wellbeing; Admissions sees the outcome only. |
| recommended_grade_id / offering_id | UUID | — | N | T1 | Known need (HB p.125 "initial placement") | |

### AD06 — Intake Capacity

**Purpose:** Seats available for a target, so officers know whether to offer or waitlist.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| intake_capacity_id | UUID | PK | Y | T1 | Assumed (DL-61) | |
| campus_id, programme_offering_id, grade_id, academic_year_id | UUID | UK | Y | T1 | Assumed | One row per target. |
| seat_limit | INT | — | Y | T1 | Open (OQ-65: who sets it, per grade or per class) | |
| seats_enrolled / seats_held | INT | — | Y | T1 | Assumed | Derived from ST02 (ENROLLED) and open offers (OFFERED, CONFIRMED). |
| waitlist_open | BOOLEAN | — | Y | T1 | Assumed | |

### AD07 — Offer

**Purpose:** A seat offered to one application.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| offer_id | UUID | PK | Y | T1 | Assumed | |
| application_id | UUID | FK:AD03 | Y | T1 | Known (Arch step 2) | One open offer per application. |
| enrolment_id | UUID | FK:ST02 | Y | T1 | Assumed (DL-57) | Created in status OFFERED. |
| offered_grade_id / offering_id / start_date | — | — | Y | T1 | Known need | May differ from what was applied for (placement). |
| conditions | TEXT | — | N | T2 | Assumed | For example "subject to Student Pass approval". |
| issued_by_user_id / issued_at | — | — | Y | T1 | Open (OQ-66: who may issue) | |
| expires_at | DATE | — | Y | T1 | Open (OQ-66: validity); setting (DL-59) | |
| offer_status | ENUM | — | Y | T1 | Assumed (DL-27) | ISSUED, ACCEPTED, DECLINED, EXPIRED, RESCINDED. |
| responded_by_person_id / responded_at | — | — | C | T1 | Assumed | Who accepted: guardian with authority (OQ-11). |

### AD08 — Student Contract

**Purpose:** The signed agreement between the operating entity and the parent.
**Known (GIIS Singapore example):** a contract is signed by both parties; cooling-off of 7 working days starts at signature (HB p.233); a course change needs a new contract, a campus change does not (HB p.232); Admissions terminates the contract on withdrawal or course transfer (HB p.236). Contract terms are regulated in Singapore (CPE/EduTrust, regulatory-context). Other countries: OQ-67.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| contract_id | UUID | PK | Y | T2 | Known (ST02 `contract_id`) | |
| offer_id | UUID | FK:AD07 | Y | T2 | Assumed | |
| operating_entity_id | UUID | FK:OR07 | Y | T1 | Confirmed rule (DL-41) | The contracting party. Named only after OQ-06. |
| contract_template_code / version | VARCHAR | — | Y | T1 | Open (OQ-67) | |
| signatory_person_ids | UUID[] | — | Y | T2 | Known need (HB "both parties"); who signs for the family Open (OQ-67, OQ-11) | |
| signature_method | ENUM | — | Y | T1 | Open (OQ-67) | WET_INK, E_SIGNATURE. |
| contract_status | ENUM | — | Y | T2 | Assumed (DL-27) | See enums. |
| signed_at | TIMESTAMP | — | C | T2 | Known need (HB p.233) | Fully signed by both parties. |
| cooling_off_working_days | INT | — | C | T1 | Known 7 (GIIS SG, HB p.233); setting (DL-59) | Copied from the setting at signature. |
| cooling_off_ends_on | DATE | — | C | T1 | Known need | Computed with the campus's working calendar (MC-06). |
| terminated_at / termination_reason | — | — | C | T2 | Known (HB p.236) | Termination itself belongs to withdrawal/transfer flows (out of scope). |

### AD09 — Admission Fee Requirement

**Purpose:** A fee that must be paid before the admission can progress. Invoice and payment live in Fees and Finance; Admissions reads the status.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| fee_requirement_id | UUID | PK | Y | T2 | Assumed | |
| application_id | UUID | FK:AD03 | Y | T2 | Assumed | |
| fee_type | ENUM | — | Y | T2 | Known: APPLICATION, REGISTRATION (HB p.242); others Open (OQ-68) | |
| due_stage | ENUM | — | Y | T1 | Assumed | AT_APPLICATION, AT_ACCEPTANCE. |
| amount / currency | DECIMAL / CHAR(3) | — | Y | T2 | Open amounts (OQ-68); currency per country (MC-12) | |
| invoice_ref | VARCHAR | — | C | T2 | Assumed | Fees and Finance invoice. |
| payment_status | ENUM | — | Y | T2 | Assumed | DUE, PAID, WAIVED, REFUNDED. |
| refundable_condition | ENUM | — | Y | T1 | Known (GIIS SG, HB p.242: refundable less admin charge only if ICA or MOE rejects) | NONE, ON_APPROVAL_REJECTED. |

### AD10 — Government Approval Case

**Purpose:** A Student Pass (ICA) or MOE approval needed before the child can start. Uses the Case object (OBJ-10) with an admissions type; immigration data stays in ST09 (Tier 3).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| case_id | UUID | FK:Case | Y | T3 | Known (ST09 `approval_case_id`) | |
| application_id | UUID | FK:AD03 | Y | T2 | Assumed | |
| approval_type | ENUM | — | Y | T2 | Known (Arch step 3; HB p.242) | STUDENT_PASS, MOE_APPROVAL; other countries Open (OQ-22). |
| authority | VARCHAR | — | Y | T1 | Known (ICA, MOE Singapore) | |
| approval_status | ENUM | — | Y | T2 | Assumed (DL-27) | PREPARING, SUBMITTED, APPROVED, REJECTED, WITHDRAWN. |
| submitted_at / decided_at | DATE | — | C | T2 | Assumed | |
| external_reference | VARCHAR | — | N | T3 | Assumed | Authority's reference number. |

## Pipeline and how it maps to the Student Directory

| Stage | Admissions record | ST02 enrolment_status | Rule |
|---|---|---|---|
| Enquiry | AD01, AD02 | — | |
| Application, documents, assessment | AD03–AD05 | — | No Student record yet (DL-57) |
| Offer issued | AD07 ISSUED | OFFERED | Creates ST01, ST02, ST04/ST05 from applicant data (DL-57) |
| Offer accepted, contract signed, registration fee paid | AD07 ACCEPTED, AD08 SIGNED, AD09 PAID | CONFIRMED | SR-AD-04 |
| Approval approved or not needed, start date reached | AD10 APPROVED / NONE | ENROLLED | SR-07: class, timetable, bus, logins (Arch step 4) |
| Offer declined, expired or rescinded | AD07 | CANCELLED | |
| Approval rejected | AD10 REJECTED | CANCELLED | Registration fee refund rule (HB p.242) |
| Deferred start | AD03 | DEFERRED | Deferment rules Open (OQ-71) |

## Field visibility by role

`M` manage, `V` view, `S` summary (counts, status only), `—` none. All cells Assumed (DL-26 rule) except where cited. T3 always needs a record ACL (A15/A17).

| Field group | Tier | Admissions officer (campus) | Admissions manager (country) | Principal (campus) | Assessor (assigned) | Counsellor / SEN (campus) | Finance (country) | Parent (own application) | Group leadership |
|---|---|---|---|---|---|---|---|---|---|
| Enquiry and contact details | T1/T2 | M | M | V | — | — | — | own | S |
| Application core, status, decision | T1/T2 | M | M | V (decides, OQ-66) | V (own) | V | — | own (status) | S |
| Documents (passport, birth certificate) | T3 | M with ACL | V with ACL | — | — | — | — | own | — |
| Assessment outcome (non-SEN) | T2 | V | V | V | M (own) | V | — | outcome only (OQ-64) | — |
| SEN evaluation | T3 | outcome only (DL-60) | outcome only | outcome only | — | M | — | own | — |
| Offer and contract | T2 | M | M | V | — | — | V | own | S |
| Admission fees | T2 | V | V | — | — | — | M | own | S |
| Government approval case | T3 | M with ACL (Known: Admissions owns Student Pass cases, Arch § Users and roles) | V with ACL | — | — | — | — | own status | — |

Known: Admissions and customer relations work at campus or country scope and own enquiries, enrolment, Student Pass cases and parent logins (Arch § Users and roles). Principals coordinate with Admissions on course or campus changes (HB p.236).

## Enumerations (candidates, DL-27 rule)

| Entity | Field | Values |
|---|---|---|
| AD01 | source_channel | WEBSITE_GROUP, WEBSITE_BRAND, WALK_IN, PHONE, EMAIL, EVENT, REFERRAL, AGENT, OTHER |
| AD01 | enquiry_status | NEW, CONTACTED, VISIT_BOOKED, VISITED, APPLICATION_STARTED, CLOSED_LOST, DUPLICATE |
| AD01 | lost_reason | NO_RESPONSE, CHOSE_OTHER_SCHOOL, FEES, LOCATION, NO_SEAT, NOT_ELIGIBLE, OTHER |
| AD03 | application_status | DRAFT, SUBMITTED, DOCUMENTS_PENDING, ASSESSMENT, UNDER_REVIEW, WAITLISTED, OFFERED, ACCEPTED, ENROLLED, DECLINED, REJECTED, WITHDRAWN |
| AD04 | document_status | REQUESTED, RECEIVED, ACCEPTED, REJECTED, WAIVED |
| AD07 | offer_status | ISSUED, ACCEPTED, DECLINED, EXPIRED, RESCINDED |
| AD08 | contract_status | DRAFT, SENT, SIGNED, IN_COOLING_OFF, ACTIVE, CANCELLED_IN_COOLING_OFF, TERMINATED |
| AD09 | payment_status | DUE, PAID, WAIVED, REFUNDED |
| AD10 | approval_status | PREPARING, SUBMITTED, APPROVED, REJECTED, WITHDRAWN |

## Business rules

- **SR-AD-01 (Assumed, DL-56):** An application moves forward only; going back (for example UNDER_REVIEW to DOCUMENTS_PENDING) is allowed and logged. ENROLLED, DECLINED, REJECTED and WITHDRAWN are final.
- **SR-AD-02 (Confirmed rule, DL-42 / DL-58):** Matching an enquirer or child to a Person never merges automatically; possible duplicates go to the review queue.
- **SR-AD-03 (Assumed, DL-61):** An offer that would exceed `seat_limit` needs an override with a reason (who may override: OQ-65).
- **SR-AD-04 (Assumed, DL-57):** ST02 becomes CONFIRMED when the offer is accepted, the contract is SIGNED and every AT_ACCEPTANCE fee is PAID or WAIVED.
- **SR-AD-05 (Known, GIIS SG, HB p.233):** Withdrawal within the cooling-off period (7 working days from full signature) is handled as a cooling-off withdrawal. The withdrawal case itself is out of scope.
- **SR-AD-06 (Known, GIIS SG, HB p.242):** Application and registration fees are non-refundable, except for new admissions when ICA rejects the Student Pass or MOE rejects a Singapore citizen's application, less an administrative charge.
- **SR-AD-07 (Known, Arch step 4 = student SR-07):** On ENROLLED the student gets a class, timetable, bus route and logins for parent and student apps. Parent access comes from a Family Link created from the verified ST05 relationship (DL-25), never from the application.
- **SR-AD-08 (Assumed, DL-60):** SEN evaluation notes never appear on the application; only the outcome and the link to the Wellbeing record.
- **SR-AD-09 (Assumed, DL-59):** Every Handbook value used here (7 working days, fee types, refund condition) is a setting on the scope tree; screens read it, never hard-code it.
- **SR-AD-10 (Known, Arch step 1; Confirmed DL-65):** AEPP placement is recorded as an AD05 outcome and becomes a placement inside the regular enrolment, not a separate AEPP enrolment.

## Events

EnquiryReceived, EnquiryRouted, ApplicationSubmitted, AssessmentRecorded, ApplicationDecided, OfferIssued, OfferResponded, ContractSigned, CoolingOffEnded, AdmissionFeePaid (from Fees), ApprovalCaseDecided, StudentEnrolled (existing, student dictionary). Events carry IDs only (DL-44 rule in architecture.md).

## Capabilities (new, DL-53 naming)

`admissions.enquiry.view`, `.create`, `.assign` · `admissions.application.view`, `.edit`, `.decide` · `admissions.document.review` · `admissions.assessment.record` · `admissions.capacity.manage` · `admissions.offer.issue`, `.rescind` · `admissions.contract.issue`, `.void` · `admissions.approval_case.manage` · existing `admissions.enrolment.create`.

## Decisions (DL-56, 57, 58, 60 Confirmed 14 Nov 2026; DL-59, 61 Assumed)

| ID | Decision | Why |
|---|---|---|
| DL-56 | Enquiry is per family (AD01, may name several children and campuses); application is per child per target (campus × offering × grade × academic year) | Matches how families enquire and how seats are counted |
| DL-57 | Student (ST01) and Enrolment (ST02 OFFERED) are created when an offer is issued; before that the child is a Person plus applicant fields | ST02 already has OFFERED and CONFIRMED; avoids creating student records for every enquiry |
| DL-58 | Enquirers and applicants are matched to existing Persons with the DL-42 review queue; returning families and siblings reuse their Persons | One Person rule; no duplicate guardians across siblings |
| DL-59 | Cooling-off days, fees due, required documents, required assessments and offer validity are scope-tree settings; GIIS Singapore Handbook values are sample defaults | Only GIIS SG rules are documented (DL-22, MC-14) |
| DL-60 | SEN evaluation and identity documents stay Tier 3 with their owners; Admissions sees an outcome code and a link | Same pattern as DL-30 |
| DL-61 | Intake capacity is a seat limit per campus × offering × grade × academic year; offers above it need an override | Officers need to know whether to offer or waitlist |

## Open questions (new)

| ID | Question | Why it matters for design |
|---|---|---|
| OQ-62 | Can families apply online before enrolment, and with what login? (GIIS gives MyGIIS logins only on enrolment, HB p.34) | Decides whether there is a parent applicant portal or staff-entered applications only |
| OQ-63 | Who owns an enquiry that names several campuses or brands, and where is an unrouted group enquiry held? | Routing, ownership and data location for group-site enquiries (with OQ-29) |
| OQ-64 | Which brands, grades and offerings require entrance tests or interviews, what are the age-to-grade cut-offs, and do parents see results? | Assessment steps and grade suggestions |
| OQ-65 | Who sets seat limits, can they be overridden, and how is the waitlist ordered (siblings, staff children, date)? | Capacity view and waitlist screens |
| OQ-66 | Who decides an application and issues an offer (officer, manager, principal), and how long is an offer valid? | Approval step and expiry |
| OQ-67 | What contract applies in each country, who signs for the family, and is e-signature allowed? | Contract screen and signature flow; only GIIS SG (CPE) is documented |
| OQ-68 | Which fees are due at admission per brand and country (application, registration, deposit), how much, and when? | Fee step; HB names application and registration fees only |
| OQ-69 | How long are enquiries and unsuccessful applications kept, and what marketing consent is collected? | Retention and consent on enquiry forms (with OQ-10) |
| OQ-70 | Do education agents submit applications, and what can they see? (HB p.246 mentions an agent commission fee) | Agent channel and access |
| OQ-71 | How are returning students and deferred starts handled, and which steps does a year-end cross-brand or cross-country move skip now that it is a new admission (DL-63)? | Whether a returning student reuses ST01 (DL-31) and skips steps |
| OQ-72 | Which documents are required per country, grade and nationality? | Document checklist defaults |

## Readiness

- **Can be designed now:** enquiry queue, application list and detail, document checklist, assessment outcome entry, offer and contract status, approval case status, enrolment handoff, all with GIIS Singapore sample values labelled as settings.
- **Keep generic, label Pending:** parent applicant portal (OQ-62), offer approver (OQ-66), contract signature (OQ-67), fee amounts (OQ-68), waitlist order (OQ-65).

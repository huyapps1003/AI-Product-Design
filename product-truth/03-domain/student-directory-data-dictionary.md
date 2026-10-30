# Global Campus OS — Student Directory Data Dictionary v0.1

**Status:** Proposed, 30 Oct 2026; DL-23…DL-27 Assumed; DL-28…DL-31 Confirmed by Henry · **Owner:** Henry
**Cites:** [Architecture v1](../00-sources/Global_Campus_OS_Product_Architecture_v1.md) § Users and roles, § Domains and modules, § Key workflows (Admission to enrolment), § Multi-campus and tenancy model, § High-level data model, § Security · [Employee Data Dictionary v1](../00-sources/Global_Campus_OS_Employee_Data_Dictionary_v1.md) (format, BR-01, E08 pattern) · [rbac-model](../04-access/rbac-model.md) Layers 4–6 · [data-classification](../05-data-governance/data-classification.md) · [data-controller-model](../05-data-governance/data-controller-model.md) · [regulatory-context](../05-data-governance/regulatory-context.md) · GIIS Student Handbook 2025-26 (HB) · GIIS Noida Almanac 2025-26 · DL-11, DL-17, DL-18, DL-19, DL-22 · RES-F05, MC-08, MC-09, MC-10 · DL-23…DL-27 (Assumed), DL-28…DL-31 (Confirmed, Henry 30 Oct 2026) · OQ-35…OQ-45
**Tóm tắt (VI):** Từ điển dữ liệu cho danh bạ học sinh (hồ sơ học sinh, ghi danh, phụ huynh/người giám hộ, giấy tờ, mã định danh) phục vụ thiết kế module Admissions and Enrolment. Mỗi trường có trạng thái Known / Assumed / Open, mức phân loại dữ liệu, quyền xem theo vai trò và ghi chú về bên kiểm soát dữ liệu.

> Drafted by Claude from product truth on 30 Oct 2026 and reviewed by Henry the same day. Henry confirmed DL-28…DL-31; DL-23…DL-27 are Assumed working directions; OQ-35…OQ-45 are in the open-questions register.

## How to read this file

- Same layout as the Employee Data Dictionary v1: entities, then one attribute table per entity, then cardinality, enums, rules, decisions and integration ownership.
- Every attribute carries a **Status** column (CONVENTIONS § 2):
  - **Known** — stated in a source. Cited as `Arch § …`, `HB p.N` (GIIS Handbook page), `ED` (Employee Data Dictionary), `RES-…`/`MC-…`.
  - **Assumed** — a design direction so work can start, cited by DL ID. Expect change.
  - **Open** — unknown, cited by OQ ID. Keep the field generic in designs; don't invent values.
- **Tier** is the data classification from Architecture § Security and DL-11: **T1** Internal (directory), **T2** Confidential (academic, finance), **T3** Restricted (identity documents, health, SEN, counselling, child protection; field-level encryption, named roles only, every read logged). A tier marked `T2*` is assigned by DL-26 (Assumed) because Architecture does not name it. Whether children's data should sit higher across the board is open (OQ-13).
- `Y` = required, `N` = optional, `C` = conditionally required. Types and keys are proposals for data architect review (as in ED).
- Every row also carries `tenant_path` (group/country/campus) and the shared common fields from ED § Shared common fields (`created_at`, `created_by_user_id`, `updated_at`, `updated_by_user_id`, `row_version`, `source_system_code`). They are not repeated per table.
- Scope is every GSG brand (DL-22). Facts from GIIS files are labelled GIIS examples. SmartLearn is excluded.

## Scope of "student directory"

The student directory is the record of **who a student is, where they are enrolled, and who acts for them**. In Architecture terms it spans the *Student Records* module (profiles, families, documents, transfers) and the parts of *Admissions and Enrolment* that create and end enrolments (Arch § Domains and modules).

| In this dictionary | Not in this dictionary (owner) |
|---|---|
| Student identity and profile | Enquiry, application, assessment, offer, contract, registration fee (Admissions pipeline; next dictionary) |
| Enrolment (campus × programme offering × year × class), subject choices, house | Health, sick bay, SEN profile, counselling, access arrangements (Wellbeing and Inclusion; Tier 3) |
| Guardians and the student–guardian relationship | Attendance, leave, conduct, house points (Attendance and Conduct) |
| Addresses and contact points | Fee plans, invoices, refunds, scholarships, insurance (Fees and Finance) |
| Identity documents and immigration status (Tier 3) | Bus subscriptions (Campus Services) |
| Board, national and legacy identifiers | Marks, grades, report cards, predicted grades (Assessment and Reporting) |
| Student account link | Consent records — Architecture names a *Consent* entity with no fields (OQ-10) |

## Modeling boundaries

- `student_id` is an immutable, never-reused global key for the learner. Student numbers, board IDs and legacy portal IDs are separate attributes or mappings (DL-23 Assumed; mirrors ED BR-01 and E08).
- **Enrolment is not the student.** Enrolment links a student to a class and subject choices for a year (Arch § Data model). A campus transfer is a record move for the same student, not a new student or a new login (Arch § Multi-campus: "families span campuses").
- **Guardian relationship is not access.** The directory records who a guardian is to a student. Parent access comes from a Family Link (F01, DL-18), created from a verified relationship (DL-25 Assumed).
- **Class and Segment (DL-29, Confirmed).** Architecture uses "Section" for two things. The product terms are **Class** for a group of students like 7B, and **Segment** for a school segment like Pre-Primary, Primary, Middle or Higher. Don't use "Section" in designs.
- Whether Student, Guardian and Staff share one *Person* record (Arch § Data model: "One Person can be a parent on one campus and staff on another") or keep separate identities (ED uses `employee_id` as the person key) is open (OQ-35). Each entity below carries a conditional `person_id`.
- All example values are synthetic. All enums are unapproved candidates (DL-27).

## Entities

| ID | Entity | Module | Status |
|---|---|---|---|
| ST01 | Student | Student Records | Entity Known (Arch § Data model); fields mixed |
| ST02 | Student Enrolment | Admissions and Enrolment → Student Records | Entity Known (Arch § Data model); fields mixed |
| ST03 | Enrolment Subject Choice | Student Records | Known ("subject choices for a year", Arch § Data model) |
| ST04 | Guardian | Student Records | Entity Known (Arch § Data model "Guardian"); fields mixed |
| ST05 | Student Guardian Relationship | Student Records | Known need (Arch "Family link"; HB p.35, p.194); model Assumed (DL-25) |
| F01 | Family Link (access) | Identity and roles | Assumed (DL-18, rbac-model Layer 4) — referenced, not redefined |
| ST06 | Address | Student Records | Known need (HB p.35) |
| ST07 | Contact Point | Student Records | Known need (HB p.35) |
| ST08 | Identity Document | Student Records | Known, Tier 3 (Arch § Data model "Identity documents") |
| ST09 | Immigration Status | Student Records | Known, Tier 3 (Arch § Data model; HB p.35, p.167) |
| ST10 | Student External Identifier | Student Records / Integration | Partly Known (MC-08, RES-F05); list Open (OQ-23) |
| ST11 | Student Account Link | Identity and roles | Known need (Arch § Admission to enrolment step 4); student access Open (OQ-19) |

## Detailed attribute dictionary

### ST01 — Student

**Purpose:** Durable identity of a learner across campuses, brands, enrolments and years.
**Data controller:** "[data controller — OQ-06]". Record lives in the student's country cell (Arch § System architecture: "everything about a student stays in that student's country cell"). Whether any student fields appear in the global directory layer is open (OQ-39).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| student_id | UUID | PK | Y | T1 | Assumed (DL-23) | Immutable global key; never reused; survives transfer between campuses and countries. |
| person_id | UUID | FK:Person | C | T1 | Open (OQ-35) | Only if a shared Person entity is confirmed. |
| student_number | VARCHAR(64) | UK | Y | T1 | Confirmed (DL-31); format Open (OQ-36) | Shown to people as **Student ID**. Unique across Campus OS and stays the same when the student transfers between campuses. Never reused. Separate from the internal `student_id` key, which is never shown. |
| legal_first_name | VARCHAR(120) | — | Y | T1 | Known (Arch Tier 1 "directory"; passport details HB p.35) | As on the identity document. Children's data sensitivity: OQ-13. |
| legal_last_name | VARCHAR(120) | — | C | T1 | Known field; rule Open (OQ-37) | Required where local naming has a family name (same rule as ED E01). |
| preferred_name | VARCHAR(160) | — | N | T1 | Assumed (DL-23, mirrors ED E01) | Display name in class lists and apps; not a legal identifier. |
| date_of_birth | DATE | — | Y | T2* | Assumed (DL-26); evidence: under-18 consent rules (regulatory-context) | Needed to tell when consent rules for children apply. Which identity attributes each country requires: OQ-37. |
| sex_or_gender | ENUM | — | ? | T2* | Open (OQ-37) | No source defines values, or legal vs. preferred. Don't design an enum. |
| nationality_codes | CHAR(2)[] | — | N | T3 | Known (passport details, HB p.35) | ISO 3166-1 alpha-2; derived from ST08 passports. Tier 3 because it comes from identity documents (Arch § Security). |
| photo_ref | VARCHAR(512) | — | N | T2* | Assumed (DL-26); consent Open (OQ-10) | Secure asset reference. HB: parental consent is taken before photos are used in social media; consent for in-system use is open. |
| home_language | VARCHAR(35) | — | N | T2* | Open (OQ-37) | Possible input to AEPP placement (Arch § Admission to enrolment); not in any source as a field. |
| preferred_locale | VARCHAR(35) | — | N | T1 | Assumed (DL-15) | BCP 47 for student-facing UI; UI copy English in v1. |
| record_status | ENUM | — | Y | T1 | Assumed (DL-27) | Master record state (ACTIVE, INACTIVE, ARCHIVED), independent of enrolment status. Retention after exit: OQ-43. |

### ST02 — Student Enrolment

**Purpose:** Effective-dated placement of a student in a campus, programme offering, year and class. One student has many over time.
**Data controller:** "[data controller — OQ-06]". The school operating entity that enrols the student is known to exist but not recorded (data-controller-model; OQ-02). A transfer between brands or countries moves data between entities (OQ-07).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| enrolment_id | UUID | PK | Y | T1 | Known entity (Arch § Data model) | F01 Family Link points here (`student_enrolment_id`, rbac-model Layer 4). |
| student_id | UUID | FK:ST01 | Y | T1 | Known | |
| campus_id | UUID | FK:E06 | Y | T1 | Known (Arch § Tenancy) | Uses the ED E06 campus catalog. Brand of the campus: OQ-14. |
| operating_entity_id | UUID | FK:Org Catalog | ? | T1 | Open (OQ-02) | Legal entity that enrols the student. Not the data controller (data-controller-model § For design). |
| academic_year_id | UUID | FK:Calendar | Y | T1 | Known (Arch § Data model "Enrolment … for a year") | From the calendar engine; years start in April (Noida) or mid-year (Singapore) (MC-06). |
| programme_offering_id | UUID | FK:Programme offering | Y | T1 | Known (Arch § Tenancy) | e.g. CBSE 6–8 at Punggol, IGCSE 9–10 at East Coast. Whether AEPP is a programme offering: OQ-45. |
| grade_level_code | VARCHAR(32) | — | Y | T1 | Known (Arch § Data model; levels from programme pack) | e.g. KG1, Grade 7, IBDP Year 1. Never hard-coded; read from the pack. |
| segment_code | VARCHAR(32) | — | N | T1 | Known values (Noida Almanac: Pre-Primary, Primary, Middle, Higher); term Confirmed (DL-29) | Derived from grade level and campus settings. |
| class_id | UUID | FK:Class | C | T1 | Known (Arch § Data model "Section (class)"); term Confirmed (DL-29) | Required once ENROLLED; assigned on enrolment (Arch § Admission to enrolment step 4). |
| house_code | VARCHAR(32) | — | N | T1 | Known for GIIS SG ("one of four Houses", HB p.169); list per campus Open (OQ-20) | Campus reference list, not fixed to four. |
| enrolment_status | ENUM | — | Y | T1 | Assumed (DL-27); terms Known (HB p.232) | See enums. Withdrawal, transfer and deferment are defined in HB p.232. |
| start_date | DATE | — | Y | T1 | Assumed (DL-24) | First day of this enrolment. |
| end_date | DATE | — | N | T1 | Known rule for withdrawal (HB p.234: "withdrawal effective date is the last attendance date") | Inclusive; null = open. Must not precede start_date. |
| end_reason | ENUM | — | C | T2* | Assumed (DL-27); terms Known (HB p.232–235) | Required when end_date is set: WITHDRAWAL, CAMPUS_TRANSFER, COURSE_CHANGE, PROMOTION_ROLLOVER, GRADUATION, NON_DELIVERY, EXPULSION, OTHER. |
| contract_id | UUID | FK:Contract | C | T2 | Known (HB p.232: course change needs a new contract, campus change does not; Arch § Admission to enrolment step 2) | Admissions pipeline entity, next dictionary. |
| previous_enrolment_id | UUID | FK:ST02 | N | T1 | Assumed (DL-24) | Links a transfer or rollover to the enrolment it replaced, so history is never overwritten. |
| approval_case_id | UUID | FK:Case | N | T2 | Known (Arch: one Case object; HB p.235: both principals approve campus transfers) | Transfer, withdrawal or Student Pass case behind this change. |

### ST03 — Enrolment Subject Choice

**Purpose:** Subjects, languages and options a student takes in one enrolment.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| subject_choice_id | UUID | PK | Y | T1 | Known entity need (Arch § Data model) | |
| enrolment_id | UUID | FK:ST02 | Y | T1 | Known | |
| subject_id | UUID | FK:Programme pack subject | Y | T1 | Known (Arch: subjects come from the programme pack) | e.g. second language Hindi, Tamil, French, Mandarin; third language Sanskrit (GIIS, HB § Language policy). |
| choice_type | ENUM | — | Y | T1 | Assumed (DL-27) | CORE, SECOND_LANGUAGE, THIRD_LANGUAGE, ELECTIVE, SUPPORT (e.g. English as a Second Language). |
| valid_from | DATE | — | Y | T1 | Assumed (DL-24) | Subject changes mid-year keep history. |
| valid_to | DATE | — | N | T1 | Assumed (DL-24) | |

### ST04 — Guardian

**Purpose:** A parent, legal guardian or local guardian who acts for one or more students, on any campus.
**Data controller:** "[data controller — OQ-06]". Guardians are data subjects (data-controller-model). Guardian records may span countries (Arch: "a parent account links to children on any campus or country"); which cell holds a guardian linked to children in two countries is open (OQ-39).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| guardian_id | UUID | PK | Y | T1 | Known entity (Arch § Data model) | |
| person_id | UUID | FK:Person | C | T1 | Open (OQ-35) | A guardian may also be staff (rbac-model Layer 4 "parent who is also staff"). |
| legal_first_name | VARCHAR(120) | — | Y | T1 | Known (HB p.35 "both parents") | |
| legal_last_name | VARCHAR(120) | — | C | T1 | Known field; rule Open (OQ-37) | |
| preferred_name | VARCHAR(160) | — | N | T1 | Assumed (DL-23) | |
| nationality_codes | CHAR(2)[] | — | N | T3 | Known (passport details of parents, HB p.35) | From ST08. |
| preferred_language | VARCHAR(35) | — | N | T1 | Known need (Arch § Tech stack: "parent preferences per channel and language"); values Open (OQ-25) | Notices in scheduled Indian languages on request (DPDP s.5(3)). |
| record_status | ENUM | — | Y | T1 | Assumed (DL-27) | ACTIVE, INACTIVE, ARCHIVED. |

### ST05 — Student Guardian Relationship

**Purpose:** Who a guardian is to a student, and what they may do for that student. The directory fact behind the Family Link.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| relationship_id | UUID | PK | Y | T1 | Assumed (DL-25) | |
| student_id | UUID | FK:ST01 | Y | T1 | Known | Links to the student, not the enrolment, so it survives transfers. |
| guardian_id | UUID | FK:ST04 | Y | T1 | Known | |
| relationship_type | ENUM | — | Y | T2* | Known types exist (HB p.194: "parents/legal/local guardians"); values Assumed (DL-27) | MOTHER, FATHER, LEGAL_GUARDIAN, LOCAL_GUARDIAN, OTHER. |
| legal_authority | ENUM | — | ? | T3 | Open (OQ-11) | Custody, parental responsibility, court orders. Don't design until OQ-11 is answered. |
| can_collect_student | BOOLEAN | — | N | T2* | Known need (HB p.175: Nursery to Grade 5 sign out only with a parent or guardian in person) | Read by security/reception. |
| is_emergency_contact | BOOLEAN | — | Y | T2* | Assumed (DL-25); evidence: nurse makes parent calls (Arch § Users and roles) | |
| contact_priority | SMALLINT | — | N | T2* | Assumed (DL-25) | 1 = call first. |
| receives_communications | BOOLEAN | — | Y | T2* | Assumed (DL-25) | Whether announcements and messages go to this guardian. |
| verification_status | ENUM | — | Y | T2* | Assumed (DL-25) | PENDING, VERIFIED, REJECTED. A Family Link (F01) is created only from a VERIFIED relationship. How verification works per country (DPDP verifiable parental consent): OQ-10, OQ-11. |
| valid_from | DATE | — | Y | T1 | Assumed (DL-24) | |
| valid_to | DATE | — | N | T1 | Assumed (DL-24) | Ending it ends the F01 link (rbac-model Layer 4 "Revocation"). |

### F01 — Family Link (reference only)

Defined in [rbac-model Layer 4](../04-access/rbac-model.md#layer-4--relationship-based-access) (DL-18, Assumed): `family_link_id`, `parent_account_id`, `student_enrolment_id`, `relationship_type`, `status`, `valid_from`, `valid_to`. This dictionary adds one proposal: `relationship_id FK:ST05`, so every link traces to a verified directory relationship (DL-25). Through this path parents also edit their child's profile, documents, immigration status, addresses and contacts, as in MyGIIS Profile Update (DL-28, Confirmed). This adds profile edit to the parent capability set R03.

### ST06 — Address

**Purpose:** Current and international home addresses for students and guardians.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| address_id | UUID | PK | Y | T2* | Assumed (DL-26) | |
| owner_type | ENUM | — | Y | T1 | Assumed (DL-27) | STUDENT, GUARDIAN. |
| owner_id | UUID | — | Y | T1 | Assumed | ST01 or ST04 key. |
| address_type | ENUM | — | Y | T2* | Known (HB p.35: "current address, international home address for student pass holders") | CURRENT, INTERNATIONAL_HOME, OTHER. |
| address_lines | VARCHAR(500) | — | Y | T2* | Known need (HB p.35) | Structured format per country is an implementation detail. |
| country_code | CHAR(2) | — | Y | T2* | Known need | ISO 3166-1 alpha-2. |
| postal_code | VARCHAR(20) | — | N | T2* | Assumed | |
| valid_from | DATE | — | Y | T1 | Assumed (DL-24) | Changes kept as history (HB p.35 asks parents to update immediately). |
| valid_to | DATE | — | N | T1 | Assumed (DL-24) | |

Conditional rule: a student with an ACTIVE Student Pass (ST09) needs an INTERNATIONAL_HOME address (Known, HB p.35; Singapore pack).

### ST07 — Contact Point

**Purpose:** Phone numbers and emails for guardians (and students where allowed).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| contact_point_id | UUID | PK | Y | T2* | Assumed (DL-26) | |
| owner_type | ENUM | — | Y | T1 | Assumed | STUDENT, GUARDIAN. Student contact points depend on OQ-19. |
| owner_id | UUID | — | Y | T1 | Assumed | |
| channel | ENUM | — | Y | T2* | Known (HB p.35 "all contact phone numbers and email ID of both parents"; Arch: push, email, SMS, WhatsApp) | PHONE, EMAIL, WHATSAPP. |
| value | VARCHAR(254) | — | Y | T2* | Known need | Phones keep country dialling format (ED E01 convention). Never used to join people (ED § guardrails). |
| is_primary | BOOLEAN | — | Y | T2* | Assumed | One primary per owner and channel. |
| verified_at | TIMESTAMPTZ | — | N | T2* | Assumed | Supports OTP login for parents (Arch § Security). |

### ST08 — Identity Document

**Purpose:** Passports and other identity documents for students and guardians.
**Tier 3.** Field-level encryption, named roles only, every read logged, record-level ACL (Arch § Security; DL-19). Never sent to an AI model. Names and passports never leave the country cell (Arch § System architecture).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| document_id | UUID | PK | Y | T3 | Known entity (Arch § Data model) | |
| owner_type | ENUM | — | Y | T3 | Known (HB p.35 "child and/or both parents") | STUDENT, GUARDIAN. |
| owner_id | UUID | — | Y | T3 | Known | |
| document_type | ENUM | — | Y | T3 | Known types: Passport, Student Pass, Medical certificate (Arch § Data model); full list per country Open (OQ-22, OQ-37) | Medical certificates are attendance evidence and belong to Attendance and Conduct; listed here only because Architecture groups them. |
| document_number | VARCHAR(64) | — | Y | T3 | Known need | Encrypted at field level. Masked by default (P-LOCK). |
| issuing_country_code | CHAR(2) | — | Y | T3 | Known need | |
| issue_date | DATE | — | N | T3 | Assumed | |
| expiry_date | DATE | — | C | T3 | Known need (HB p.167: parents must contact school before Student Pass expiry) | Drives renewal reminders via the rules engine. |
| file_ref | VARCHAR(512) | — | N | T3 | Assumed | Object store reference to the scan. |
| verified_by_user_id | UUID | FK:A01 | N | T3 | Assumed | |
| verified_at | TIMESTAMPTZ | — | N | T3 | Assumed | Changes logged (Arch § Data model "changes logged"). |

### ST09 — Immigration Status

**Purpose:** Residency and pass status of a student or guardian in the campus country. Drives attendance thresholds and ICA reporting in Singapore.
**Tier 3** (Arch § Data model "Immigration status"). Class teachers see only the resulting attendance threshold (for example "Attendance threshold 90%"), never the Student Pass or any immigration field (DL-30, Confirmed). The rules engine reads this table; the threshold is the only value that leaves Tier 3.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| immigration_status_id | UUID | PK | Y | T3 | Known entity | |
| owner_type | ENUM | — | Y | T3 | Known (HB p.35 "child and/or both parents") | STUDENT, GUARDIAN. |
| owner_id | UUID | — | Y | T3 | Known | |
| country_code | CHAR(2) | — | Y | T3 | Known | Country the status applies to. |
| status_type | ENUM | — | Y | T3 | Known values for Singapore (HB p.35, p.167: citizen, Permanent Resident, Dependent Pass, Student Pass); other countries Open (OQ-22) | Country regulator pack supplies the list. |
| pass_document_id | UUID | FK:ST08 | C | T3 | Known | Required for pass types. |
| valid_from | DATE | — | Y | T3 | Known need (HB p.35: inform school on change, e.g. citizenship acquired) | |
| valid_to | DATE | — | N | T3 | Known need | |
| approval_case_id | UUID | FK:Case | N | T3 | Known (Arch: Student Pass or MOE approval case; HB p.235: Admissions coordinates issue and cancellation) | |

### ST10 — Student External Identifier

**Purpose:** Board, national, regulator and legacy-system identifiers, including recorded refusals.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| external_identifier_id | UUID | PK | Y | T2 | Assumed (DL-23, mirrors ED E08) | |
| student_id | UUID | FK:ST01 | Y | T2 | Known | |
| identifier_scheme | VARCHAR(64) | — | Y | T2 | Known examples: APAAR (CBSE, RES-F05), CBSE board registration, IB and Cambridge candidate registration (Arch § Integrations), MyGIIS legacy ID (Arch § Integrations); full list Open (OQ-23) | Managed catalog per country and programme pack. |
| identifier_value | VARCHAR(160) | — | C | T2 | Known | Null when status is REFUSED or NOT_GENERATED. National IDs may be Tier 3: OQ-23. |
| identifier_status | ENUM | — | Y | T2 | Known for APAAR (RES-F05: "refused" with copy kept, or "NOGEN") | ACTIVE, PENDING, REFUSED, NOT_GENERATED, RETIRED. |
| refusal_evidence_ref | VARCHAR(512) | — | C | T3 | Known (RES-F05: copy of parental refusal kept) | Required when REFUSED. |
| sync_status | ENUM | — | N | T1 | Assumed (ED E08 pattern) | For legacy mappings: PENDING, SYNCED, FAILED, CONFLICT. Conflicts quarantined, never overwritten (ED BR-20 pattern). Authority during migration: OQ-44. |
| valid_from | DATE | — | Y | T1 | Assumed | |
| valid_to | DATE | — | N | T1 | Assumed | |

### ST11 — Student Account Link

**Purpose:** Links a student to their login. Students sign in with Google Workspace SSO (Arch § Tech stack, § Security). Logins are created on enrolment (Arch § Admission to enrolment step 4).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| student_account_link_id | UUID | PK | Y | T1 | Assumed (ED A02 pattern) | |
| student_id | UUID | FK:ST01 | Y | T1 | Known | |
| user_id | UUID | FK:A01 | Y | T1 | Known (ED A01) | |
| link_status | ENUM | — | Y | T1 | Assumed (ED A02 enum) | ACTIVE, ENDED. Ending all enrolments ends the link. |
| valid_from | DATE | — | Y | T1 | Assumed | |
| valid_to | DATE | — | N | T1 | Assumed | What a student can see beyond "self": OQ-19. From which grade students get logins: OQ-19. |

## Field visibility by role

`E` edit · `V` view · `M` masked (P-LOCK, reveal logged) · `—` hidden · `?` open. Every cell is limited by scope first (rbac-model Layers 1–3: GA ceiling, TM membership, data scope); Tier 3 also needs a record ACL (Layer 5, DL-19). Roles are Architecture school roles; their access-role mapping for these modules is not set yet (role-catalogue: "Not mapped").

| Field group | Tier | Admissions (campus/country) | Principal / VP (campus) | Coordinator (programme band) | Class teacher (own class) | Subject teacher (own classes) | Nurse / counsellor | Transport / security | Parent (own child, F01) | Student (self) | Group leadership |
|---|---|---|---|---|---|---|---|---|---|---|---|
| Name, preferred name, class, grade, house, programme | T1 | E | V | V | V | V | V | V | V | V | — (de-identified) |
| Enrolment status, dates, end reason, transfer history | T1 / T2* | E | V (approves transfers, HB p.235) | V | V (status) | — | — | — | V (own) | ? (OQ-19) | — |
| Photo | T2* | E | V | V | V | V | V | V | V | V | — |
| Date of birth | T2* | E | V | V | V | — | V | — | V | ? | — |
| Guardians: names, relationship, can-collect, emergency flag | T2* | E | V | V | V | — | V | V (collection only) | V (own) | ? | — |
| Guardian phone and email | T2* | E | V | V | V | M | V | V (transport only) | E (DL-28) | — | — |
| Addresses | T2* | E | V | M | M | — | — | V (transport, pickup point only) | E (DL-28) | — | — |
| Identity documents, nationality, immigration status | T3 | E (ACL; manages Student Pass cases) | M | — | — | — | — | — | E (DL-28; HB p.35 Profile Update) | — | — |
| Attendance threshold (derived; reason not shown) | T2 | V | V | V | V (DL-30) | V | — | — | V (own) | ? (OQ-19) | — |
| External identifiers (APAAR, board IDs) | T2 | E | V | V (exports) | — | — | — | — | V (own) | — | — |
| Legal authority / custody | T3 | ? (OQ-11) | ? | — | ? | — | — | ? | — | — | — |

Basis for the cells:

- **Known:** Admissions owns enquiries, enrolment, Student Pass cases and parent logins (Arch § Users and roles); principals of both campuses approve transfers (HB p.235); nurses make parent calls; transport runs rider manifests; security handles sign-out by parent or guardian (HB p.175); least privilege by scope ("a Grade 4 class teacher sees their section"); Tier 3 named roles only; group analytics see no names, passports or health data (Arch § Security, § System architecture); parents see only their own children (DL-18).
- **Confirmed (DL-28):** parents edit their child's passport, immigration status, addresses and contacts, as in MyGIIS Profile Update (HB p.35).
- **Assumed (DL-26):** every other V/M/— cell, set by the rule "the least each role needs for the workflows Architecture names". These need confirmation from Admissions owners and DPOs.
- **Open:** student access (OQ-19), custody (OQ-11).
- **Confirmed (DL-30):** class teachers see the attendance threshold, not why it applies; Student Pass and immigration status stay Tier 3.

## Data controller and residency notes

| Entity | Controller | Residency | Leaves the cell? | Open |
|---|---|---|---|---|
| ST01–ST03 Student, enrolment, subjects | [data controller — OQ-06] | Student's country cell (Known, Arch § System architecture) | De-identified events only, to group analytics (Known) | OQ-06, OQ-08, OQ-39 |
| ST04–ST07 Guardians, relationships, addresses, contacts | [data controller — OQ-06] | Country cell of the linked students; guardian across countries Open | Parent identity for login lives in the global identity layer (Known: "thin global layer holds only identity, directory") | OQ-39 |
| ST08–ST09 Identity documents, immigration | [data controller — OQ-06] | Country cell; never in raw form outside (Known, rbac-model Layer 6) | Disclosed to ICA and other regulators by law (Known, Arch § Integrations) | OQ-22 |
| ST10 External identifiers | [data controller — OQ-06] | Country cell | Exported to CBSE, IB, Cambridge (Known, Arch § Integrations) | OQ-23 |
| ST02 on transfer between brands or countries | Two entities may be involved (MC-10) | Record moves to the receiving cell (Arch "a transfer is a record move") | Yes, under terms not yet defined | OQ-07 |

Don't name a controller, fiduciary or processor in any notice, consent or export screen until OQ-06 is answered (data-controller-model § For design). The operating entity on the enrolment is not the data controller.

## Cardinality and referential integrity

| Parent | Child | Cardinality | FK | Meaning | Status |
|---|---|---|---|---|---|
| ST01 | ST02 | 1 → 0..N | ST02.student_id | One learner, many enrolments over time and campuses | Known (Arch) |
| ST02 | ST02 | 1 → 0..1 | ST02.previous_enrolment_id | Transfer and rollover chain | Assumed (DL-24) |
| ST02 | ST03 | 1 → 0..N | ST03.enrolment_id | Subject choices for the year | Known (Arch) |
| ST01 | ST05 | 1 → 0..N | ST05.student_id | A student can have several guardians | Known (HB "both parents", local guardians) |
| ST04 | ST05 | 1 → 0..N | ST05.guardian_id | A guardian can have children on several campuses and countries | Known (Arch "families span campuses") |
| ST05 | F01 | 1 → 0..N | F01.relationship_id | One link per active enrolment of the student | Assumed (DL-25) |
| ST01/ST04 | ST06, ST07, ST08, ST09 | 1 → 0..N | owner_type + owner_id | Polymorphic owner; history kept | Assumed |
| ST01 | ST10 | 1 → 0..N | ST10.student_id | One per scheme (active) | Assumed |
| ST01 | ST11 | 1 → 0..N | ST11.student_id | At most one ACTIVE link | Assumed (ED D05 pattern) |

## Candidate enums — require approval (DL-27)

| Entity | Field | Values | Source of terms |
|---|---|---|---|
| ST01, ST04 | record_status | ACTIVE, INACTIVE, ARCHIVED | ED E01 |
| ST02 | enrolment_status | OFFERED, CONFIRMED, ENROLLED, DEFERRED, TRANSFERRED_OUT, WITHDRAWN, COMPLETED, CANCELLED | HB p.232 (withdrawal, transfer, deferment); Arch § Admission to enrolment (offer, contract) |
| ST02 | end_reason | WITHDRAWAL, CAMPUS_TRANSFER, COURSE_CHANGE, PROMOTION_ROLLOVER, GRADUATION, NON_DELIVERY, EXPULSION, OTHER | HB p.232–235 |
| ST03 | choice_type | CORE, SECOND_LANGUAGE, THIRD_LANGUAGE, ELECTIVE, SUPPORT | HB language policy |
| ST05 | relationship_type | MOTHER, FATHER, LEGAL_GUARDIAN, LOCAL_GUARDIAN, OTHER | HB p.194 |
| ST05 | verification_status | PENDING, VERIFIED, REJECTED | DL-25 |
| ST06 | address_type | CURRENT, INTERNATIONAL_HOME, OTHER | HB p.35 |
| ST07 | channel | PHONE, EMAIL, WHATSAPP | HB p.35; Arch § Tech stack |
| ST08 | document_type | PASSPORT, STUDENT_PASS, MEDICAL_CERTIFICATE, OTHER (country pack extends) | Arch § Data model |
| ST09 | status_type (SG pack) | CITIZEN, PERMANENT_RESIDENT, DEPENDENT_PASS, STUDENT_PASS, OTHER | HB p.35, p.167 |
| ST10 | identifier_status | ACTIVE, PENDING, REFUSED, NOT_GENERATED, RETIRED | RES-F05 (APAAR) |

## Business rules

- **SR-01 (Assumed, DL-23):** Each learner has one immutable, never-reused `student_id`, kept across campuses, brands and countries.
- **SR-01a (Confirmed, DL-31):** Each learner has one Student ID (`student_number`), unique across Campus OS, that stays the same when they transfer between campuses.
- **SR-02 (Known, Arch):** An enrolment links one student to one class and its subject choices for one academic year at one campus.
- **SR-03 (Assumed, DL-24):** A new enrolment period never overwrites a past one. Transfers and rollovers end the old enrolment and create a new one linked by `previous_enrolment_id`.
- **SR-04 (Known, HB p.232):** A course change ends the student contract and needs a new one; a campus change with the same course does not.
- **SR-05 (Known, HB p.235):** A campus transfer is accepted only when both principals approve; decision within three weeks; profile updated within three working days.
- **SR-06 (Known, HB p.234):** Withdrawal effective date is the last attendance date and becomes `end_date`.
- **SR-07 (Known, Arch § Admission to enrolment):** On enrolment the student gets a class, timetable, bus route and logins for parent and student apps.
- **SR-08 (Known, HB p.35, Singapore pack):** A Student Pass holder needs an international home address.
- **SR-09 (Known, HB p.167; Arch § Absence and leave):** Immigration status sets the attendance threshold (90% monthly for Student Pass holders) and the ICA notice after 7 consecutive days of absence.
- **SR-10 (Known, Arch § Security; DL-19):** Identity documents and immigration status are Tier 3: field-level encryption, named roles, record ACL, every read logged, never sent to AI.
- **SR-11 (Assumed, DL-25):** A Family Link (F01) is created only from a VERIFIED student–guardian relationship; ending the relationship ends the link.
- **SR-12 (Known, BR-06 principle):** Enrolment, guardianship or campus affiliation never grants staff access. Staff see students only through GA/TM grants, scope and ACL.
- **SR-13 (Known, ED § guardrails):** Never join people by name, email, phone or student number; use `student_id`, `guardian_id` and explicit mappings.
- **SR-14 (Known, ED BR-20 pattern):** Data from MyGIIS or another brand's SIS that conflicts with the directory is quarantined, never silently overwritten.
- **SR-15 (Known, Arch § Security):** Changes to enrolment, guardianship, identity documents and immigration status produce immutable audit events.

## Decisions (Assumed)

| ID | Decision | Why |
|---|---|---|
| DL-23 | `student_id` is an immutable UUID, separate from student numbers, board IDs and legacy IDs (ST10 holds those) | Same reasoning as ED BR-01 and E08; transfers keep one identity |
| DL-24 | Enrolment and directory sub-records are effective-dated; history is never overwritten | Architecture effective-dating rule; ED BR-03 |
| DL-25 | The student–guardian relationship (ST05) is the directory record; the Family Link (F01) is the access grant created from a verified relationship | Keeps "who is this person to the child" separate from "what can they see", like BR-06 for staff |
| DL-26 | Tiers and role visibility for fields Architecture doesn't classify (marked `T2*` and Assumed cells in the matrix) | Lets design start; to confirm with Admissions owners and DPOs. OQ-13 may raise tiers |
| DL-27 | Candidate enums above | Designers need fixed labels; same approach as DL-10 |

## Decisions (Confirmed)

| ID | Decision | Confirmed by |
|---|---|---|
| DL-28 | Parents edit their child's profile the way MyGIIS Profile Update works today: passport details, immigration status, addresses and contact details of the child and both parents (HB p.35). This extends the read-only parent capability set in DL-18. Answers OQ-38. | Henry, 30 Oct 2026 |
| DL-29 | **Class** means a group of students like 7B. **Segment** means a school segment like Pre-Primary, Primary, Middle or Higher. "Section" is not used as a product term. Answers OQ-40 (AEPP part moved to OQ-45). | Henry, 30 Oct 2026 |
| DL-30 | Teachers and coordinators see a student's attendance threshold (for example 90%) without the reason. Student Pass and immigration status stay Tier 3, visible only to Admissions and other named roles with a record ACL. Answers OQ-41. | Henry, 30 Oct 2026 |
| DL-31 | The student number is called **Student ID** in the product. It is unique across Campus OS and stays the same when a student transfers between campuses. Answers OQ-36 except the format. | Henry, 30 Oct 2026 |

## Open questions (added to the register)

| ID | Question | Why it matters for design | Evidence | Status |
|---|---|---|---|---|
| OQ-35 | Is there one shared Person record across students, guardians and staff, or separate identities per role? | Decides whether a parent who is also staff has one profile or two | Arch § Data model "Person"; ED uses `employee_id`; rbac-model Layer 4 | Open |
| OQ-36 | What is the format and uniqueness scope of the student (admission) number, and does it change on transfer between campuses, brands or countries? | Directory search, ID cards and exports show it | No source defines it; ED Decision D02 is the staff equivalent | **Answered → DL-31** (unique, kept on transfer); format still open |
| OQ-37 | Which identity attributes must a student record hold in each country (date of birth, sex or gender values, name structure, nationality, home language)? | Form fields and validation differ by country | Not in Architecture or HB; MC-05 | Open |
| OQ-38 | Can parents edit the student profile, identity documents and contacts themselves (as in MyGIIS Profile Update), and does the school verify edits before they apply? | DL-18 gives parents a read-only set, but MyGIIS parity is the Phase 1 gate | HB p.35; Arch § Roadmap Phase 1; DL-18 | **Answered → DL-28** |
| OQ-39 | Do any student or guardian fields sit in the global identity and directory layer, and which cell holds a guardian whose children are in two countries? | Search across campuses, and transfers | Arch § System architecture ("staff and family directory" at the edge vs. "everything about a student stays in that student's country cell") | Open |
| OQ-40 | Architecture uses "Section" for both a segment (Pre-Primary…Higher) and a class (7B). Which product term means which? Is AEPP a programme offering? | Labels on every enrolment and class screen | Arch § Tenancy vs. § Data model; Noida Almanac; HB AEPP | **Answered → DL-29** (AEPP part → OQ-45) |
| OQ-41 | Can teachers see that a student holds a Student Pass (to apply the 90% threshold) without seeing Tier 3 immigration data? | Attendance warnings show the threshold to class teachers | Arch § Absence and leave step 4; Arch § Security | **Answered → DL-30** |
| OQ-42 | Which health alerts (allergies, medical conditions), if any, appear on the student directory card for teachers, transport and security? | Tier 3 health data vs. safety on trips and buses | HB medical conditions; Arch § Security | Open |
| OQ-43 | How long are student and guardian records kept after withdrawal or graduation, per country, and what is deleted or anonymised? | Archive and delete states in the directory | regulatory-context (retention not defined); ED Decision D12 | Open |
| OQ-44 | During migration, which system is authoritative for student data: MyGIIS, other brands' SIS, or Campus OS? | Which fields are read-only with a source chip | Arch § Integrations (MyGIIS two-way sync); OQ-27, OQ-28 | Open |
| OQ-45 | Is AEPP (Academic English Preparatory Program) a programme offering with its own enrolment, or a placement inside another programme? | Decides whether an AEPP student has an AEPP enrolment or a flag on a regular one | Arch § Admission to enrolment (AEPP placement); HB § AEPP (six-month course, placement test, classes AEPP 1 and 2) | Open |

Existing questions this dictionary depends on: OQ-02, OQ-06, OQ-07, OQ-08, OQ-10, OQ-11, OQ-13, OQ-14, OQ-19, OQ-20, OQ-22, OQ-23, OQ-25.

## Source of truth and integration ownership

| Domain | Entities | Proposed authority | Consumers | Contract |
|---|---|---|---|---|
| Student identity | ST01 | Student Records (authority during migration: OQ-44) | All modules, apps | Canonical `student_id`, change events |
| Enrolment | ST02, ST03 | Admissions and Enrolment creates; Student Records holds | Teaching and Learning, Attendance, Assessment, Fees, Campus Services | Enrolment events; never grants staff access |
| Guardians and family | ST04–ST07 | Student Records | Identity and roles (F01), Communication and Cases, Notifications | Verified relationship → family link |
| Identity documents, immigration | ST08, ST09 | Student Records (Admissions manages Student Pass cases) | Rules engine (thresholds), Cases (ICA) | Tier 3; only the threshold leaves (DL-30) |
| External identifiers | ST10 | Student Records | Integration hub (CBSE, IB, Cambridge, MyGIIS) | Scheme catalog per pack |
| Student login | ST11 | Identity and roles | Student app | Google Workspace SSO |

## Change event candidates

| Event | Entity | Payload | Consumers | Risk policy |
|---|---|---|---|---|
| StudentCreated | ST01 | student_id | Directory, search | No PII in payload beyond IDs |
| StudentEnrolled | ST02 | enrolment_id, student_id, campus_id, class_id, academic_year_id | Timetable, Attendance, Fees, Transport, Identity (logins) | Creates logins and F01 links (SR-07, SR-11) |
| EnrolmentEnded | ST02 | enrolment_id, end_date, end_reason | Fees (refund case), Identity (end links), Attendance | Ends F01 links; history kept |
| StudentTransferred | ST02 | old and new enrolment_id, campuses | Both campuses, Identity | Cross-cell move needs OQ-07 |
| GuardianRelationshipChanged | ST05 | relationship_id, student_id, status | Identity (F01), Notifications | Fail closed on unverified |
| ImmigrationStatusChanged | ST09 | owner_id, resulting attendance threshold only (DL-30) | Rules engine, Cases | Tier 3 values never in payload |
| ExternalIdentifierChanged | ST10 | student_id, scheme, status | Integration hub | Refusals recorded, not hidden |

## Readiness

Not ready to freeze as a schema. Design can start on ST01–ST03 and ST05–ST07 with the Assumed decisions. ST08–ST09 screens should show the Tier 3 lock treatment and generic fields until OQ-37 is answered; parent edit screens follow MyGIIS Profile Update (DL-28). Legal authority (OQ-11) and consent (OQ-10) stay out of designs.

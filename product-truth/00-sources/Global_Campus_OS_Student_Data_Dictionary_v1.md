# Global Campus OS — Student Data Dictionary v1.0

**Status:** Proposed specification for review; 1 November 2026. **Merged** into [`03-domain/student-directory-data-dictionary.md`](../03-domain/student-directory-data-dictionary.md) (v0.2) on 9 Nov 2026; that file wins where they differ. Decision D01–D14 map to DL and OQ IDs in its § Merge notes.
**Scope:** Student Master Data + Guardian and Family Access (Student Records, Admissions and Enrolment).

## Confirmed inputs

- All learners across every GSG brand (GIIS, OWIS and others; DL-22).
- A student may be enrolled at multiple campuses over time; a campus transfer preserves identity.
- Guardians (parents, legal guardians, local guardians) may have children on any campus or country.
- Parent edit rights for profile, identity documents, addresses and contacts confirmed (DL-28).
- Student ID (`student_number`) is unique across Campus OS and kept on campus transfer (DL-31).
- Class and Segment terminology confirmed: Class = group like 7B, Segment = school segment like Pre-Primary/Primary/Middle/Higher (DL-29).
- Teachers see the attendance threshold, not the immigration reason behind it (DL-30).
- Student authoritative source is Student Records; migration authority is unresolved (D09).

## Modeling boundaries

- `student_id` is an immutable global person key; student numbers, board IDs and legacy portal IDs are separate attributes or mappings (ST10).
- Enrolment is not the student. An enrolment links a student to a class and programme for one academic year at one campus.
- Guardian relationship is not access. The directory records who a guardian is to a student; parent access comes from a Family Link (F01) created from a verified relationship.
- Class and Segment are distinct. Do not use the word "Section" in designs (DL-29).
- Whether Student, Guardian and Staff share one Person record is open (D01).
- All mutable records use the shared system fields in the **Common fields** section.
- `TIMESTAMPTZ` denotes an offset-aware timestamp stored/compared in UTC. Date interval endpoints are inclusive.
- `Y` = required, `N` = optional, `C` = conditionally required.
- All example values are synthetic; all enums are unapproved candidates.
- Data sensitivity: **Internal** for ordinary directory attributes; **Confidential** for personally identifying or enrolment details; **Restricted** for identity documents, immigration, health, SEN, counselling and child-protection data (field-level encryption, named roles only, every read logged, never sent to AI).
- PK/FK field and type choices are proposals subject to data architect review.

## Entities

| ID | Entity | Domain | Owner / authority | Status |
|---|---|---|---|---|
| ST01 | Student | Student Records | Student Records — TBD | Confirmed population; fields proposed |
| ST02 | Student Enrolment | Admissions and Enrolment → Student Records | Student Records — TBD | Confirmed entity; fields proposed |
| ST03 | Enrolment Subject Choice | Student Records | Student Records — TBD | Proposed |
| ST04 | Guardian | Student Records | Student Records — TBD | Confirmed entity; fields proposed |
| ST05 | Student Guardian Relationship | Student Records | Student Records — TBD | Proposed |
| F01 | Family Link (access) | Identity and Roles | Identity and Roles | Referenced — not redefined here |
| ST06 | Address | Student Records | Student Records — TBD | Proposed |
| ST07 | Contact Point | Student Records | Student Records — TBD | Proposed |
| ST08 | Identity Document | Student Records | Student Records — TBD | Confirmed Restricted (Arch § Security) |
| ST09 | Immigration Status | Student Records | Student Records — TBD | Confirmed Restricted (Arch § Security) |
| ST10 | Student External Identifier | Student Records / Integration | Student Records / Integration — TBD | Proposed |
| ST11 | Student Account Link | Identity and Roles | Identity and Roles | Proposed |

## Detailed attribute dictionary

### ST01 — Student

**Purpose:** Durable identity of a learner across campuses, brands, enrolments and years.
**Proposed owner:** Student Records — TBD
**Scope/status:** Core / Confirmed population; fields proposed

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| student_id | UUID | PK | Y | Internal | Immutable global student key; never reuse; survives campus and country transfer. |
| person_id | UUID | FK:Person | C | Internal | Only if a shared Person entity is confirmed. Open (D01). |
| student_number | VARCHAR(64) | UK | Y | Internal | Shown to people as **Student ID**. Unique across Campus OS; kept on campus transfer (DL-31). Never reused. Separate from internal `student_id`, which is never shown. Format open (D02). |
| legal_first_name | VARCHAR(120) | — | Y | Confidential | As on the identity document. Localization may require alternative structure. |
| legal_last_name | VARCHAR(120) | — | C | Confidential | Required where local naming convention has a family name. Rule per country open (D03). |
| preferred_name | VARCHAR(160) | — | N | Internal | Display name in class lists and apps; not a legal identifier. |
| date_of_birth | DATE | — | Y | Confidential | Required to determine when under-18 consent rules apply (regulatory-context). |
| sex_or_gender | ENUM | — | ? | Confidential | No source defines values or legal vs. preferred distinction. Do not design an enum until D03 is answered. |
| nationality_codes | CHAR(2)[] | — | N | Restricted | ISO 3166-1 alpha-2; derived from ST08 passports. Restricted because it comes from identity documents (Arch § Security). |
| photo_ref | VARCHAR(512) | — | N | Confidential | Secure asset reference, not raw image bytes. Parental consent for in-system use open (D04). |
| home_language | VARCHAR(35) | — | N | Confidential | Possible input to AEPP placement (Arch § Admission to enrolment); country fields open (D03). |
| preferred_locale | VARCHAR(35) | — | N | Internal | BCP 47 for student-facing UI. |
| record_status | ENUM | — | Y | Internal | Student master record state, independent of enrolment status. |

### ST02 — Student Enrolment

**Purpose:** Effective-dated placement of a student in a campus, programme offering, academic year and class. One student may have many over time.
**Proposed owner:** Admissions and Enrolment creates; Student Records holds — TBD
**Scope/status:** Core / Confirmed entity; fields proposed

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| enrolment_id | UUID | PK | Y | Internal | Unique enrolment instance. Family Link (F01) references `student_enrolment_id` from here. |
| student_id | UUID | FK:ST01 | Y | Internal | Student associated with this enrolment. |
| campus_id | UUID | FK:E06 | Y | Internal | Uses the campus catalog (E06 from Employee Data Dictionary). |
| operating_entity_id | UUID | FK:Org Catalog | ? | Internal | Legal entity that enrols the student. Not the data controller. Open (D05). |
| academic_year_id | UUID | FK:Calendar | Y | Internal | From the calendar engine; year start varies by campus. |
| programme_offering_id | UUID | FK:Programme offering | Y | Internal | e.g. CBSE 6–8 at Punggol, IGCSE 9–10 at East Coast. Whether AEPP is a programme offering: open (D10). |
| grade_level_code | VARCHAR(32) | — | Y | Internal | e.g. KG1, Grade 7, IBDP Year 1. Never hard-coded; read from the programme pack. |
| segment_code | VARCHAR(32) | — | N | Internal | School segment: Pre-Primary, Primary, Middle, Higher. Derived from grade level and campus settings. Term confirmed (DL-29). |
| class_id | UUID | FK:Class | C | Internal | Required once ENROLLED; assigned on enrolment (Arch § Admission to enrolment step 4). Term confirmed (DL-29). |
| house_code | VARCHAR(32) | — | N | Internal | Campus reference list; not fixed to four. |
| enrolment_status | ENUM | — | Y | Internal | See enums. Withdrawal, transfer and deferment defined in GIIS Handbook p.232. |
| start_date | DATE | — | Y | Internal | First day of this enrolment period. |
| end_date | DATE | — | N | Internal | Last day (inclusive); null = open. Must not precede start_date. |
| end_reason | ENUM | — | C | Confidential | Required when end_date is set. |
| contract_id | UUID | FK:Contract | C | Confidential | Course change requires a new contract; campus change with same course does not (Handbook p.232; Arch § Admission to enrolment step 2). |
| previous_enrolment_id | UUID | FK:ST02 | N | Internal | Links a transfer or rollover to the enrolment it replaced; history never overwritten. |
| approval_case_id | UUID | FK:Case | N | Confidential | Transfer, withdrawal or Student Pass case behind this change. |

### ST03 — Enrolment Subject Choice

**Purpose:** Subjects, languages and options a student takes in one enrolment.
**Proposed owner:** Student Records — TBD
**Scope/status:** Core / Proposed

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| subject_choice_id | UUID | PK | Y | Internal | Individual subject selection for this enrolment. |
| enrolment_id | UUID | FK:ST02 | Y | Internal | Enrolment this choice belongs to. |
| subject_id | UUID | FK:Programme pack subject | Y | Internal | e.g. second language Hindi, Tamil, French, Mandarin; third language Sanskrit. |
| choice_type | ENUM | — | Y | Internal | CORE, SECOND_LANGUAGE, THIRD_LANGUAGE, ELECTIVE, SUPPORT. |
| valid_from | DATE | — | Y | Internal | Subject changes mid-year keep history. |
| valid_to | DATE | — | N | Internal | End of this choice; null = open. |

### ST04 — Guardian

**Purpose:** A parent, legal guardian or local guardian who acts for one or more students on any campus.
**Proposed owner:** Student Records — TBD
**Scope/status:** Core / Confirmed entity; fields proposed

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| guardian_id | UUID | PK | Y | Internal | Immutable guardian identity key. |
| person_id | UUID | FK:Person | C | Internal | Only if a shared Person entity is confirmed. A guardian may also be staff (D01). |
| legal_first_name | VARCHAR(120) | — | Y | Confidential | As on identity document (GIIS Handbook p.35 "both parents"). |
| legal_last_name | VARCHAR(120) | — | C | Confidential | Required where local naming has a family name. Rule per country open (D03). |
| preferred_name | VARCHAR(160) | — | N | Internal | Preferred display name; not a legal identifier. |
| nationality_codes | CHAR(2)[] | — | N | Restricted | ISO 3166-1 alpha-2; derived from ST08 passports of guardian. |
| preferred_language | VARCHAR(35) | — | N | Internal | BCP 47; notices in scheduled Indian languages on request (DPDP s.5(3)). Values open (D06). |
| record_status | ENUM | — | Y | Internal | Guardian master record state; independent of relationship and family-link state. |

### ST05 — Student Guardian Relationship

**Purpose:** Who a guardian is to a student and what they may do for that student. The directory fact behind the Family Link.
**Proposed owner:** Student Records — TBD
**Scope/status:** Core / Proposed

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| relationship_id | UUID | PK | Y | Internal | Unique relationship record. |
| student_id | UUID | FK:ST01 | Y | Internal | Links to the student, not the enrolment, so it survives campus transfers. |
| guardian_id | UUID | FK:ST04 | Y | Internal | Guardian party to this relationship. |
| relationship_type | ENUM | — | Y | Confidential | MOTHER, FATHER, LEGAL_GUARDIAN, LOCAL_GUARDIAN, OTHER (Handbook p.194). |
| legal_authority | ENUM | — | ? | Restricted | Custody, parental responsibility, court orders. Do not design until D07 is answered. |
| can_collect_student | BOOLEAN | — | N | Confidential | Required for Nursery–Grade 5 sign-out rule (Handbook p.175). |
| is_emergency_contact | BOOLEAN | — | Y | Confidential | Nurse makes parent calls (Arch § Users and roles). |
| contact_priority | SMALLINT | — | N | Confidential | 1 = call first. |
| receives_communications | BOOLEAN | — | Y | Confidential | Whether announcements and messages go to this guardian. |
| verification_status | ENUM | — | Y | Confidential | PENDING, VERIFIED, REJECTED. A Family Link (F01) is created only from a VERIFIED relationship. |
| valid_from | DATE | — | Y | Internal | Beginning of this relationship. |
| valid_to | DATE | — | N | Internal | End; ending this record ends the corresponding F01 link. |

### F01 — Family Link (reference only)

Defined in the RBAC model, Layer 4 (DL-18): `family_link_id`, `parent_account_id`, `student_enrolment_id`, `relationship_type`, `status`, `valid_from`, `valid_to`. This dictionary adds one proposal: `relationship_id FK:ST05`, so every link traces back to a verified directory relationship. Parents can also edit their child's profile, documents, immigration status, addresses and contacts through this link (DL-28).

### ST06 — Address

**Purpose:** Current and international home addresses for students and guardians.
**Proposed owner:** Student Records — TBD
**Scope/status:** Core / Proposed

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| address_id | UUID | PK | Y | Confidential | Address record identifier. |
| owner_type | ENUM | — | Y | Internal | STUDENT, GUARDIAN. |
| owner_id | UUID | — | Y | Internal | ST01 or ST04 key. |
| address_type | ENUM | — | Y | Confidential | CURRENT, INTERNATIONAL_HOME, OTHER (Handbook p.35). |
| address_lines | VARCHAR(500) | — | Y | Confidential | Structured format per country is an implementation detail. |
| country_code | CHAR(2) | — | Y | Confidential | ISO 3166-1 alpha-2. |
| postal_code | VARCHAR(20) | — | N | Confidential | Format per country. |
| valid_from | DATE | — | Y | Internal | Address changes kept as history (Handbook p.35: parents must update immediately). |
| valid_to | DATE | — | N | Internal | End of this address record; null = current. |

Conditional rule: a student with an ACTIVE Student Pass (ST09) requires an INTERNATIONAL_HOME address (Handbook p.35, Singapore regulatory pack).

### ST07 — Contact Point

**Purpose:** Phone numbers and emails for guardians and students where allowed.
**Proposed owner:** Student Records — TBD
**Scope/status:** Core / Proposed

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| contact_point_id | UUID | PK | Y | Confidential | Contact point identifier. |
| owner_type | ENUM | — | Y | Internal | STUDENT, GUARDIAN. Student contact points depend on D08. |
| owner_id | UUID | — | Y | Internal | ST01 or ST04 key. |
| channel | ENUM | — | Y | Confidential | PHONE, EMAIL, WHATSAPP (Handbook p.35; Arch push/email/SMS/WhatsApp channels). |
| value | VARCHAR(254) | — | Y | Confidential | Phones keep country dialling format. Never used to join people; use canonical IDs. |
| is_primary | BOOLEAN | — | Y | Confidential | One primary per owner and channel. |
| verified_at | TIMESTAMPTZ | — | N | Confidential | Supports OTP login for parents (Arch § Security). |

### ST08 — Identity Document

**Purpose:** Passports and other identity documents for students and guardians.
**Proposed owner:** Student Records — TBD
**Scope/status:** Core / Confirmed Restricted (Arch § Security; DL-19)

**Sensitivity: Restricted.** Field-level encryption required. Named roles only. Every read must be logged. Record-level ACL required. Never sent to an AI model. Names and passports must never leave the student's country cell (Arch § System architecture).

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| document_id | UUID | PK | Y | Restricted | Immutable document record key. |
| owner_type | ENUM | — | Y | Restricted | STUDENT, GUARDIAN (Handbook p.35 "child and/or both parents"). |
| owner_id | UUID | — | Y | Restricted | ST01 or ST04 key. |
| document_type | ENUM | — | Y | Restricted | PASSPORT, STUDENT_PASS, MEDICAL_CERTIFICATE, OTHER. Full list per country is an implementation detail (D03). |
| document_number | VARCHAR(64) | — | Y | Restricted | Encrypted at field level. Masked by default in all UI. |
| issuing_country_code | CHAR(2) | — | Y | Restricted | ISO 3166-1 alpha-2. |
| issue_date | DATE | — | N | Restricted | Issue date of the document. |
| expiry_date | DATE | — | C | Restricted | Required for pass types; drives renewal reminders (Handbook p.167). |
| file_ref | VARCHAR(512) | — | N | Restricted | Object store reference to the document scan. |
| verified_by_user_id | UUID | FK:A01 | N | Restricted | Staff who verified this document. |
| verified_at | TIMESTAMPTZ | — | N | Restricted | Verification timestamp; changes logged (Arch § Data model). |

### ST09 — Immigration Status

**Purpose:** Residency and pass status of a student or guardian in the campus country. Drives attendance thresholds and ICA reporting in Singapore.
**Proposed owner:** Student Records — TBD
**Scope/status:** Core / Confirmed Restricted (Arch § Security)

**Sensitivity: Restricted.** Class teachers see only the resulting attendance threshold; they must never see the Student Pass or any immigration field (DL-30). The rules engine reads this table; the threshold is the only value that may leave Tier 3.

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| immigration_status_id | UUID | PK | Y | Restricted | Immigration status record key. |
| owner_type | ENUM | — | Y | Restricted | STUDENT, GUARDIAN. |
| owner_id | UUID | — | Y | Restricted | ST01 or ST04 key. |
| country_code | CHAR(2) | — | Y | Restricted | Country this status applies to. |
| status_type | ENUM | — | Y | Restricted | Country regulator pack supplies the list. Singapore values: CITIZEN, PERMANENT_RESIDENT, DEPENDENT_PASS, STUDENT_PASS, OTHER. Other countries open (D03). |
| pass_document_id | UUID | FK:ST08 | C | Restricted | Required for pass types; links to the physical document. |
| valid_from | DATE | — | Y | Restricted | Inform school on change (Handbook p.35, e.g. citizenship acquired). |
| valid_to | DATE | — | N | Restricted | End of this status; null = current. |
| approval_case_id | UUID | FK:Case | N | Restricted | Student Pass or MOE approval case (Handbook p.235: Admissions coordinates issue and cancellation). |

### ST10 — Student External Identifier

**Purpose:** Board, national, regulator and legacy-system identifiers, including recorded refusals.
**Proposed owner:** Student Records / Integration — TBD
**Scope/status:** Conditional / Proposed

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| external_identifier_id | UUID | PK | Y | Confidential | Individual external identifier record. |
| student_id | UUID | FK:ST01 | Y | Confidential | Canonical student key. |
| identifier_scheme | VARCHAR(64) | — | Y | Confidential | Managed catalog per country and programme pack. Examples: APAAR (CBSE), CBSE board registration, IB candidate number, Cambridge candidate number, MyGIIS legacy ID. Full list open (D03). |
| identifier_value | VARCHAR(160) | — | C | Confidential | Null when status is REFUSED or NOT_GENERATED. National IDs may be Restricted (D03). |
| identifier_status | ENUM | — | Y | Confidential | ACTIVE, PENDING, REFUSED, NOT_GENERATED, RETIRED. APAAR: refusal with copy kept. |
| refusal_evidence_ref | VARCHAR(512) | — | C | Restricted | Required when REFUSED; parental refusal copy kept (APAAR rule). |
| sync_status | ENUM | — | N | Internal | For legacy mappings: PENDING, SYNCED, FAILED, CONFLICT. Conflicts quarantined, never overwritten. Authority during migration open (D09). |
| valid_from | DATE | — | Y | Internal | Beginning of this identifier. |
| valid_to | DATE | — | N | Internal | End of this identifier; null = current. |

### ST11 — Student Account Link

**Purpose:** Links a student to their login. Students sign in with Google Workspace SSO. Logins created on enrolment (Arch § Admission to enrolment step 4).
**Proposed owner:** Identity and Roles
**Scope/status:** Core / Proposed

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| student_account_link_id | UUID | PK | Y | Internal | Person / account association key. |
| student_id | UUID | FK:ST01 | Y | Internal | Student linked to account. |
| user_id | UUID | FK:A01 | Y | Internal | Account being linked; A01 from Employee Data Dictionary. |
| link_status | ENUM | — | Y | Internal | ACTIVE, ENDED. Ending all enrolments ends the link. |
| valid_from | DATE | — | Y | Internal | Beginning of active link. |
| valid_to | DATE | — | N | Internal | End of link; null = open. What a student can see and from which grade students get logins open (D08). |

## Cardinality and referential integrity

| Parent | Child | Cardinality | FK | Meaning | Status |
|---|---|---|---|---|---|
| ST01 | ST02 | 1 → 0..N | ST02.student_id | One learner may have many enrolments over time and campuses. | Confirmed (Arch) |
| ST02 | ST02 | 1 → 0..1 | ST02.previous_enrolment_id | Transfer and rollover chain; history never overwritten. | Proposed |
| ST02 | ST03 | 1 → 0..N | ST03.enrolment_id | Subject choices for this enrolment year. | Confirmed (Arch) |
| ST01 | ST05 | 1 → 0..N | ST05.student_id | A student may have several guardians (both parents, local guardians). | Confirmed (Handbook) |
| ST04 | ST05 | 1 → 0..N | ST05.guardian_id | A guardian may have children on multiple campuses and countries. | Confirmed (Arch) |
| ST05 | F01 | 1 → 0..N | F01.relationship_id | One family link per active enrolment of the student. | Proposed |
| ST01/ST04 | ST06 | 1 → 0..N | owner_type + owner_id | Polymorphic owner; address history kept. | Proposed |
| ST01/ST04 | ST07 | 1 → 0..N | owner_type + owner_id | Polymorphic owner; contact point history kept. | Proposed |
| ST01/ST04 | ST08 | 1 → 0..N | owner_type + owner_id | Polymorphic owner; document history kept. | Confirmed (Arch) |
| ST01/ST04 | ST09 | 1 → 0..N | owner_type + owner_id | Polymorphic owner; immigration history kept. | Confirmed (Arch) |
| ST01 | ST10 | 1 → 0..N | ST10.student_id | One active record per scheme; all history kept. | Proposed |
| ST01 | ST11 | 1 → 0..1 | ST11.student_id | At most one ACTIVE link at a time. | Proposed |

## Shared common fields

| Attribute | Type | Req. | Definition |
|---|---|---|---|
| created_at | TIMESTAMPTZ | Required | UTC creation timestamp for every persisted entity. |
| created_by_user_id | UUID nullable | Conditional | Initiator of record creation, or system principal if imported. |
| updated_at | TIMESTAMPTZ | Required | Last persisted update timestamp. |
| updated_by_user_id | UUID nullable | Conditional | Last modifying user/system principal. |
| row_version | INTEGER | Required | Optimistic concurrency / version check for mutable records. |
| source_system_code | VARCHAR(100) nullable | Conditional | Provenance for externally sourced writes where needed. |

All entities also carry `tenant_path` (group/country/campus) from Architecture multi-tenancy model.

## Candidate enums — require approval

| Entity | Field | Values | Source of terms |
|---|---|---|---|
| ST01, ST04 | record_status | ACTIVE, INACTIVE, ARCHIVED | Employee Data Dictionary E01 pattern |
| ST02 | enrolment_status | OFFERED, CONFIRMED, ENROLLED, DEFERRED, TRANSFERRED_OUT, WITHDRAWN, COMPLETED, CANCELLED | GIIS Handbook p.232; Arch § Admission to enrolment |
| ST02 | end_reason | WITHDRAWAL, CAMPUS_TRANSFER, COURSE_CHANGE, PROMOTION_ROLLOVER, GRADUATION, NON_DELIVERY, EXPULSION, OTHER | GIIS Handbook p.232–235 |
| ST03 | choice_type | CORE, SECOND_LANGUAGE, THIRD_LANGUAGE, ELECTIVE, SUPPORT | GIIS Handbook language policy |
| ST05 | relationship_type | MOTHER, FATHER, LEGAL_GUARDIAN, LOCAL_GUARDIAN, OTHER | GIIS Handbook p.194 |
| ST05 | verification_status | PENDING, VERIFIED, REJECTED | DL-25 |
| ST06 | address_type | CURRENT, INTERNATIONAL_HOME, OTHER | GIIS Handbook p.35 |
| ST07 | channel | PHONE, EMAIL, WHATSAPP | GIIS Handbook p.35; Arch § Tech stack |
| ST08 | document_type | PASSPORT, STUDENT_PASS, MEDICAL_CERTIFICATE, OTHER | Arch § Data model; country pack extends |
| ST09 | status_type (SG pack) | CITIZEN, PERMANENT_RESIDENT, DEPENDENT_PASS, STUDENT_PASS, OTHER | GIIS Handbook p.35, p.167 |
| ST10 | identifier_status | ACTIVE, PENDING, REFUSED, NOT_GENERATED, RETIRED | APAAR (RES-F05) |
| ST11 | link_status | ACTIVE, ENDED | Employee Data Dictionary A02 pattern |

## Atomic business rules

- **SR-01 (Proposed):** Each learner shall have one immutable, never-reused `student_id`. [Decision: D02]
- **SR-01a (Confirmed — DL-31):** Each learner has one Student ID (`student_number`), unique across Campus OS, that stays the same when they transfer between campuses.
- **SR-02 (Confirmed — Arch):** An enrolment links one student to one class and its subject choices for one academic year at one campus.
- **SR-03 (Proposed):** A new enrolment period must never overwrite a past one. Transfers and rollovers end the old enrolment and create a new one linked by `previous_enrolment_id`. [Decision: D02]
- **SR-04 (Confirmed — Handbook p.232):** A course change ends the student contract and requires a new one; a campus change with the same course does not.
- **SR-05 (Confirmed — Handbook p.235):** A campus transfer is accepted only when both principals approve; decision within three weeks; profile updated within three working days.
- **SR-06 (Confirmed — Handbook p.234):** Withdrawal effective date is the last attendance date and becomes `end_date`.
- **SR-07 (Confirmed — Arch § Admission to enrolment):** On enrolment the student receives a class, timetable, bus route and logins for parent and student apps.
- **SR-08 (Confirmed — Handbook p.35, Singapore pack):** A Student Pass holder requires an international home address.
- **SR-09 (Confirmed — Handbook p.167; Arch § Absence and leave):** Immigration status sets the attendance threshold (90% monthly for Student Pass holders) and triggers the ICA notice after 7 consecutive absences.
- **SR-10 (Confirmed — Arch § Security; DL-19):** Identity documents and immigration status are Restricted: field-level encryption, named roles, record-level ACL, every read logged, never sent to AI.
- **SR-11 (Proposed):** A Family Link (F01) is created only from a VERIFIED student–guardian relationship; ending the relationship ends the link. [Decision: D04]
- **SR-12 (Confirmed — BR-06 principle):** Enrolment, guardianship or campus affiliation alone never grants staff access to student data. Staff see students only through GA/TM grants, scope and ACL.
- **SR-13 (Confirmed — Employee Data Dictionary § guardrails):** Never join people by name, email, phone or student number; use `student_id`, `guardian_id` and explicit mappings.
- **SR-14 (Proposed):** Data from MyGIIS or another brand's SIS that conflicts with the student directory is quarantined, never silently overwritten. [Decision: D09]
- **SR-15 (Confirmed — Arch § Security):** Changes to enrolment, guardianship, identity documents and immigration status produce immutable audit events.

## Unresolved decisions

| ID | Priority | Decision | Question | Default direction (not confirmed) |
|---|---|---|---|---|
| D01 | P0 | Person model | Is there one shared Person record across students, guardians and staff, or separate identities per role? | Keep separate keys for now; add conditional `person_id` FK to support future merge. |
| D02 | P0 | Student identifiers | What is the format and uniqueness scope of the student number? Does it change on transfer between campuses, brands or countries? | Unique across Campus OS, kept on transfer (DL-31). Format TBD. Separate from internal UUID. |
| D03 | P0 | Country-specific identity fields | Which identity attributes must a student record hold in each country (date of birth, sex/gender values, name structure, nationality, home language, document types)? | Minimal global required set; country packs extend. |
| D04 | P0 | Photo and consent | Can parents edit the student profile and documents, including photo, directly? Does the school verify edits before they apply? | Parent edit confirmed for passport, immigration, addresses, contacts (DL-28). Photo consent and edit verification open. |
| D05 | P0 | Operating entity | Which legal entity is recorded as the enrolment school, and how does it relate to brand, country and campus hierarchy? | Record `operating_entity_id` on ST02; do not name as data controller until OQ-06 answered. |
| D06 | P0 | Guardian language preferences | Which languages are supported for guardian-facing communications per country? | BCP 47 field on guardian; values from country pack. |
| D07 | P0 | Legal authority and custody | What legal authority, custody or court-order data must the directory hold, and which roles see it? | Do not design until confirmed; hold as Restricted open field. |
| D08 | P0 | Student access | From which grade do students receive logins, and what can they see? | Provision login on enrolment (Arch § Admission to enrolment step 4); read scope TBD. |
| D09 | P0 | Migration authority | During migration, which system is authoritative for student data: MyGIIS, other brands' SIS, or Campus OS? Which fields are read-only with a source chip? | Support external sources via ST10 mapping; quarantine conflicts per SR-14. |
| D10 | P1 | AEPP offering type | Is AEPP a programme offering with its own enrolment, or a placement flag on a regular enrolment? | Keep as open field on ST02 until confirmed; do not hard-code. |
| D11 | P1 | Cross-country guardian residency | Which country cell holds a guardian whose children are enrolled in two different countries? | Guardian data in originating cell; cross-cell design TBD. |
| D12 | P1 | Data retention | How long are student and guardian records retained after withdrawal or graduation per country? What is deleted or anonymised? | Effective-dated history; implement retention schedule by jurisdiction. |
| D13 | P1 | Health alerts on directory card | Which health alerts (allergies, medical conditions), if any, appear on the student directory card for teachers, transport and security? | No health data in directory without confirmed safety exception; design generic field. |
| D14 | P1 | Data controller | Which entity is the data controller for student records per country? | Do not name a controller in any screen or notice until confirmed. |

## Source of truth and integration ownership

| Domain | Related entities | Proposed authority | Consumers | Contract |
|---|---|---|---|---|
| Student identity | ST01 | Student Records (migration authority: D09) | All modules and apps | Canonical `student_id`, profile reads and change events |
| Enrolment | ST02, ST03 | Admissions and Enrolment creates; Student Records holds | Teaching and Learning, Attendance, Assessment, Fees, Campus Services | Enrolment events; never grants staff access |
| Guardians and family | ST04–ST07 | Student Records | Identity and Roles (F01), Communication and Cases, Notifications | Verified relationship → family link |
| Identity documents, immigration | ST08, ST09 | Student Records (Admissions manages Student Pass cases) | Rules engine (thresholds only), Cases (ICA) | Restricted; only the attendance threshold may leave |
| External identifiers | ST10 | Student Records | Integration hub (CBSE, IB, Cambridge, MyGIIS) | Scheme catalog per programme pack |
| Student login | ST11 | Identity and Roles | Student app | Google Workspace SSO |
| Audit events | ST15 (shared A14 pattern) | Shared Audit Service — TBD | Security, Support, Compliance | Immutable redacted event log |

## Change event candidates

| Event | Entity | Suggested payload | Consumers | Risk policy |
|---|---|---|---|---|
| StudentCreated | ST01 | student_id | Directory, search | No PII in payload beyond IDs |
| StudentEnrolled | ST02 | enrolment_id, student_id, campus_id, class_id, academic_year_id | Timetable, Attendance, Fees, Transport, Identity (logins) | Creates logins and F01 links (SR-07, SR-11) |
| EnrolmentEnded | ST02 | enrolment_id, end_date, end_reason | Fees (refund case), Identity (end links), Attendance | Ends F01 links; history kept |
| StudentTransferred | ST02 | old and new enrolment_id, campuses | Both campuses, Identity | Cross-cell move needs D11 |
| GuardianRelationshipChanged | ST05 | relationship_id, student_id, status | Identity (F01), Notifications | Fail closed on unverified |
| ImmigrationStatusChanged | ST09 | owner_id, resulting attendance threshold only (DL-30) | Rules engine, Cases | Restricted values never in payload |
| ExternalIdentifierChanged | ST10 | student_id, scheme, status | Integration hub | Refusals recorded, not hidden |
| ProfileEditedByParent | ST01/ST06/ST07/ST08 | student_id, guardian_id, changed_fields | Student Records, Audit | Verify before applying if school policy requires it (D04) |

## Implementation guardrails

- Enforce referential integrity where domains share a storage boundary; use validated integration references otherwise.
- Do not join people by name, email, phone or student number; use `student_id`, `guardian_id` and explicit mappings.
- Enforce `valid_to >= valid_from`, prevent overlapping duplicate active records unless an approved exception applies, and time-bound all grants and links.
- Restricted fields (ST08, ST09, nationality) require field-level encryption, record-level ACL, named-role lists and read logging. Never send them to an AI model.
- Do not infer a student's immigration status from any attribute other than ST09. The attendance threshold derived by the rules engine is the only ST09 output allowed to leave Restricted scope.
- Do not name a data controller, data fiduciary or data processor in any screen, consent flow or notice until D14 is answered.
- Source conflicts from MyGIIS or external SIS must be quarantined, never silently merged or overwritten.
- Student Pass and identity document data must not leave the student's country cell in raw form (Arch § System architecture).
- Passwords, access tokens and full audit snapshots do not belong in this Student Data Dictionary.
- Review jurisdiction-specific privacy requirements (PDPA, DPDP, FERPA and others), data residency rules and retention periods before any production schema is frozen.

## Readiness

**Not ready to freeze as a physical database schema until P0 decisions D01–D09 are resolved.** Design can start on ST01–ST03 and ST05–ST07 with the proposed decisions in place. ST08–ST09 screens should show Restricted field treatment and generic field labels until D03 is answered. Parent edit screens follow MyGIIS Profile Update (DL-28, Confirmed).

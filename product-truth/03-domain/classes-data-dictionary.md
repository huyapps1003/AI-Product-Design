# Global Campus OS — Classes Data Dictionary v0.1 (draft)

**Status:** Proposed draft, 14 Nov 2026. DL-70…DL-76 Assumed (pending Henry); OQ-73…OQ-79 Open · **Owner:** Henry
**Cites:** [Architecture v1](../00-sources/Global_Campus_OS_Product_Architecture_v1.md) § Users and roles (Class teacher), § Data model (Academic structure "Section (class)"; Organisation "Room"), § Key workflows (Admission to enrolment step 4), § Security (least privilege by scope) · [Student Directory Spec v1.0](./student-directory-data-dictionary.md) ST02 (`class_id`, `house_code`, `previous_enrolment_id`), ST03, SR-02, SR-03, SR-07, SR-16, SR-18 · [Admissions dictionary](./admissions-data-dictionary.md) AD06, SR-AD-07 · [Organisation dictionary](./organisation-data-dictionary.md) OR04, segments · [Employee Data Dictionary v1](../00-sources/Global_Campus_OS_Employee_Data_Dictionary_v1.md) D01, D02 (`section_id`) · [architecture.md](./architecture.md) DL-43 domains · GIIS Student Handbook 2025-26 (HB) p.9–14 (classroom allocations), p.125 and § AEPP, p.128–131 (promotion policy), p.169 (house system) · DL-16, DL-22, DL-29, DL-30, DL-53, DL-54, DL-59, DL-61, DL-62, DL-63, DL-65, DL-66, DL-68 · MC-06, MC-07, MC-14 · OQ-20, OQ-55
**Tóm tắt (VI):** Từ điển dữ liệu cho module Lớp học: lớp (ví dụ 7B) theo cơ sở × chương trình × khối × năm học, kế hoạch số lớp và sĩ số (nguồn cho giới hạn chỗ của Tuyển sinh), giáo viên chủ nhiệm, xếp lớp và chuyển lớp trong năm, nhà (house), và chuyển năm học cuối năm (lên lớp, ở lại, chuyển cơ sở theo giai đoạn). Mỗi trường có trạng thái Known / Assumed / Open.

> Drafted by Claude from product truth on 14 Nov 2026 after Henry approved the scope the same day. Henry asked for "Classes & Sections"; the product term is **Class** and "Section" is never used (DL-29), so the module is called **Classes**.

## How to read this file

Same layout and rules as the [Student Directory Spec](./student-directory-data-dictionary.md#how-to-read-this-file): **Known** (cited), **Assumed** (DL), **Open** (OQ); tiers T1/T2/T3 (DL-11); `Y`/`N`/`C` required. Every row also carries `tenant_path`, `data_region` (UI term **Data location**, DL-47) and the shared common fields. Facts from the GIIS Handbook are **GIIS Singapore examples**, not group rules (DL-22). All enums are unapproved candidates (DL-27 rule). All example values are synthetic.

## Scope

Approved by Henry, 14 Nov 2026: **how students are grouped into classes for a year, and how they move up at year end.**

| In this dictionary | Not in this dictionary (owner) |
|---|---|
| Class per campus × programme offering × grade × academic year | Timetable, periods, rooms booking (Teaching and Learning: Timetable) |
| Class plan per grade: number of classes × class size, the source of the Admissions seat limit (DL-61) | Subject-teacher allocation (Teaching: Academic Allocation, OBJ-12 / Employee DD D02) |
| Class teacher and co-teacher | Elective subject groups and sets, streams (open: OQ-77) |
| Placing students: Admissions handoff, start-of-year allocation, in-year class moves, AEPP entry and exit | Attendance, leave, conduct (Attendance and Conduct) |
| Year-end rollover: next year's classes, promotion, retention, stage-campus promotion (DL-54, DL-62) | Promotion rules and results (Assessment and Reporting; this file reads the outcome) |
| House list and house allocation | House points and events (Activities and Awards) |
| Class lists (rosters) and who sees them | Setting up academic years, terms, grades and programme offerings (Academic structure, referenced only) |

## Modeling boundaries

- **A class is one group for one year (DL-70).** A class belongs to one campus, one programme offering, one grade and one academic year. Next year's 8B is a new class, not 7B renamed. Rollover can suggest the link (`previous_class_id`), but past rosters are never edited.
- **The class plan is the seat limit (DL-71).** The Principal approves, per grade, how many classes run and the size limit of each. The sum of the class size limits is the Admissions seat limit (AD06 `seat_limit`, DL-61), so the number exists once. Admissions keeps its buffer, held seats and waitlist.
- **Classes writes the placement, Student Records holds the enrolment (DL-72).** Placement history (CL04) is the single writer of the student's class. ST02 `class_id` always shows the current placement. A class move within the same campus, offering and year is not a transfer and does not end the enrolment; a campus move within the year still is (SR-16).
- **Class teacher lives on the class (DL-73).** Class teacher and co-teacher are assignments on the class (CL03). They feed data-scope suggestions for that class (DL-16) and never grant access on their own (BR-06, SR-12). Subject teaching stays in Academic Allocation.
- **AEPP classes are placement classes (DL-74).** AEPP 1 and AEPP 2 are real classes with rooms (HB p.14) whose students keep their grade and programme enrolment (DL-65). Leaving AEPP is a class move into a regular class.
- **Promotion is decided elsewhere (DL-75).** Assessment and Reporting applies promotion rules (HB p.128–131) and hands Classes an outcome per student. Classes turns the outcomes into next-year enrolments and classes, and the Principal approves the run.
- **Houses carry over (DL-76).** House stays on ST02 `house_code`; Classes allocates it and copies it forward at rollover and on class moves.
- **Rules are settings with no group default (DL-59 pattern).** Class size limits per grade and the house list are campus settings. A value that isn't set shows "Not set" and blocks the step that needs it (approving the class plan; allocating houses). GIIS Singapore values appear only as GIIS Singapore's own settings in sample data.
- **Data location.** Classes, plans, placements and rollover runs are held in the campus's data location. A stage-campus promotion inside one country stays in one data location; a cross-country move is a new admission (DL-63).

## Entities

| ID | Entity | Module | Status |
|---|---|---|---|
| CL01 | Class | Classes (Academic structure domain) | Known (Arch § Data model "Section (class)"; ST02 `class_id`; HB p.9–14); fields Assumed |
| CL02 | Class Plan | Classes | Assumed (DL-71); the seat limit it produces is Confirmed (DL-61) |
| CL03 | Class Staff Assignment | Classes | Known role (Arch § Users and roles "Class teacher", scope Section); model Assumed (DL-73) |
| CL04 | Class Placement | Classes | Known need (Arch step 4 "assigned a class"; ST02 `class_id`); history Assumed (DL-72) |
| CL05 | House | Classes (list) / Student Records (ST02 `house_code`) | Known for GIIS SG ("one of four Houses", HB p.169); list per campus Open (OQ-20) |
| CL06 | Rollover Run | Classes | Known need (SR-16 promotion; DL-54; DL-62); model Assumed (DL-75) |
| CL07 | Rollover Line | Classes (reads Assessment outcome) | Known outcomes (HB p.128–131); model Assumed (DL-75) |

ST02, ST03, AD06 and Employee DD D01/D02 are referenced, not redefined. Employee DD D02 `section_id` means CL01 `class_id` (DL-29).

## Detailed attribute dictionary

### CL01 — Class

**Purpose:** A group of students taught together for one academic year, such as 7B, IB PYP 2A or AEPP 1.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| class_id | UUID | PK | Y | T1 | Known entity (Arch § Data model) | Referenced by ST02 `class_id` and Employee DD D02 `section_id`. |
| class_name | VARCHAR(40) | UK with campus + year | Y | T1 | Known that names vary (HB p.9–14: "Standard 7 B", "IB PYP 2A", "K1 A Extnd", "AEPP 1"; sample: "4 Orchid", "Year 2 Oak") | Free text; naming convention per campus. Unique within a campus and year. |
| short_name | VARCHAR(12) | — | N | T1 | Assumed | For chips and tight lists, e.g. "7B". |
| campus_id | UUID | FK:OR04 | Y | T1 | Known | |
| academic_year_id | UUID | FK:Academic year | Y | T1 | Confirmed (DL-70) | Years start at different times (MC-06). |
| programme_offering_id | UUID | FK:Offering | Y | T1 | Known (MC-07) | e.g. CBSE 1-12, IB PYP, Cambridge CLSP 6-8. |
| class_type | ENUM | — | Y | T1 | Assumed (DL-74) | REGULAR, PLACEMENT (AEPP). Composite or mixed-age: OQ-78. |
| grade_id | UUID | FK:Grade | C | T1 | Known (Arch: grade level from the programme pack) | Required for REGULAR. |
| grade_ids | UUID[] | — | C | T1 | Assumed (DL-74) | PLACEMENT classes only: grades the class can take (AEPP spans grades). |
| segment_code | VARCHAR(32) | — | N | T1 | Known values per campus (organisation dictionary); derived | Pre-Primary, Primary, Middle, Higher at GIIS (DL-29). |
| size_limit | INT | — | Y | T1 | Assumed (DL-71) | From the class plan; may differ per class (e.g. one smaller class). |
| home_room | VARCHAR(40) | — | N | T1 | Known (HB p.9–14 room per class, e.g. "S4 - 04") | Becomes FK:Room when the Organisation building/room list exists (Arch § Data model "Room"). |
| class_status | ENUM | — | Y | T1 | Assumed (DL-27) | DRAFT, ACTIVE, CLOSED. See enums. |
| previous_class_id | UUID | FK:CL01 | N | T1 | Assumed (DL-70) | Suggested predecessor from rollover (7B → 8B). Display only. |
| enrolled_count | INT | — | Y | T1 | Assumed; derived | Current CL04 placements. Never stored by hand. |
| opened_at / closed_at | DATE | — | C | T1 | Assumed | Closed at year end or when merged mid-year. |

### CL02 — Class Plan

**Purpose:** How many classes a grade runs in a year and how big each may be. The Principal approves it; Admissions reads the seat limit from it.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| class_plan_id | UUID | PK | Y | T1 | Assumed (DL-71) | |
| campus_id, programme_offering_id, grade_id, academic_year_id | UUID | UK | Y | T1 | Confirmed shape (DL-61 target) | Same key as AD06. |
| planned_class_count | INT | — | Y | T1 | Confirmed (DL-61 "classes") | |
| default_size_limit | INT | — | C | T1 | Confirmed (DL-61 "class size"); value per campus setting (OQ-74) | Required to approve. "Not set" blocks approval. |
| seat_limit | INT | — | Y | T1 | Confirmed (DL-61); derived (DL-71) | Sum of the planned classes' `size_limit`. Written to AD06 `seat_limit`. |
| plan_status | ENUM | — | Y | T1 | Assumed (DL-27) | DRAFT, SUBMITTED, APPROVED. |
| prepared_by_user_id | UUID | FK:User | Y | T1 | Open (who prepares, OQ-73) | |
| approved_by_user_id / approved_at | — | — | C | T1 | Confirmed (DL-61: set by the Principal) | |
| revision_reason | TEXT | — | C | T1 | Assumed | Required when an APPROVED plan changes. |
| minimum_allowed_seats | INT | — | Y | T1 | Assumed (SR-CL-02); derived | Enrolled + held seats (AD06). The plan can't go below it. |

### CL03 — Class Staff Assignment

**Purpose:** Who looks after a class day to day.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| class_staff_id | UUID | PK | Y | T1 | Assumed (DL-73) | |
| class_id | UUID | FK:CL01 | Y | T1 | Assumed | |
| employee_id | UUID | FK:E01 | Y | T1 | Known (Employee DD keys by `employee_id`) | |
| class_role | ENUM | — | Y | T1 | Known: CLASS_TEACHER (Arch § Users and roles); CO_CLASS_TEACHER, ASSISTANT Assumed | One active CLASS_TEACHER per class (SR-CL-05). |
| valid_from / valid_to | DATE | — | Y / N | T1 | Assumed | Effective-dated; a mid-year change ends one row and starts another. |
| assigned_by_user_id | UUID | FK:User | Y | T1 | Open (OQ-73) | |

### CL04 — Class Placement

**Purpose:** One student in one class for a period. The history of a student's classes within an enrolment.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| placement_id | UUID | PK | Y | T1 | Assumed (DL-72) | |
| enrolment_id | UUID | FK:ST02 | Y | T1 | Known (ST02 holds `class_id`) | |
| class_id | UUID | FK:CL01 | Y | T1 | Known | Same campus, offering and year as the enrolment (SR-CL-03). |
| valid_from | DATE | — | Y | T1 | Assumed | For a CONFIRMED student, the enrolment start date (pre-placement). |
| valid_to | DATE | — | N | T1 | Assumed | Null = current. |
| placement_reason | ENUM | — | Y | T1 | Assumed (DL-27) | See enums. |
| reason_note | TEXT | — | C | T2 | Assumed | Required for CLASS_MOVE. May describe a family or conduct reason, so T2. |
| placed_by_user_id | UUID | FK:User | Y | T1 | Open (OQ-73) | |
| approved_by_user_id | UUID | FK:User | C | T1 | Open (OQ-73) | Over-limit placements need the Principal (SR-CL-04). |

### CL05 — House

**Purpose:** The campus's list of houses. The student's house is ST02 `house_code`.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| house_id | UUID | PK | Y | T1 | Known (HB p.169) | |
| campus_id | UUID | FK:OR04 | Y | T1 | Open (campus or brand-country list, OQ-20) | |
| house_code | VARCHAR(32) | UK with campus | Y | T1 | Known (ST02 `house_code`) | |
| house_name / colour | VARCHAR | — | Y / N | T1 | Assumed | Names are synthetic in samples ("House A"…). |
| active | BOOLEAN | — | Y | T1 | Assumed | Campuses may have no houses (several samples show none). |

### CL06 — Rollover Run

**Purpose:** Moving one campus's students from one academic year into the next, as one reviewed batch.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| rollover_run_id | UUID | PK | Y | T1 | Assumed (DL-75) | |
| campus_id | UUID | FK:OR04 | Y | T1 | Assumed | The campus students leave. Stage-campus targets are per line. |
| from_academic_year_id / to_academic_year_id | UUID | FK | Y | T1 | Confirmed rule (DL-54: the year decides promotion) | |
| run_status | ENUM | — | Y | T1 | Assumed (DL-27) | See enums. |
| prepared_by_user_id | UUID | FK:User | Y | T1 | Open (OQ-73) | |
| approved_by_user_id / approved_at | — | — | C | T1 | Known (Arch § Users and roles: Principal approves promotion) | |
| applied_at | TIMESTAMP | — | C | T1 | Assumed | When new enrolments were created. |
| line_counts | JSON | — | Y | T1 | Assumed; derived | Counts per outcome and line status for the header. |

### CL07 — Rollover Line

**Purpose:** What happens to one student at year end.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| rollover_line_id | UUID | PK | Y | T1 | Assumed (DL-75) | |
| rollover_run_id | UUID | FK:CL06 | Y | T1 | Assumed | |
| from_enrolment_id | UUID | FK:ST02 | Y | T1 | Known (SR-03) | |
| promotion_outcome | ENUM | — | Y | T2 | Known values (HB p.128–131: promotion, conditional promotion with re-test, weak pass, retained/detained, board criteria for 10 and 12); source Assessment | See enums. T2 because it reflects academic results. |
| outcome_source | ENUM | — | Y | T1 | Assumed | ASSESSMENT, WITHDRAWAL_CASE, PRINCIPAL_DECISION, PARENT_REQUEST (K–5 retention, HB p.128). |
| target_campus_id | UUID | FK:OR04 | C | T1 | Confirmed rule (DL-62) | Differs from the run's campus for a stage-campus promotion. |
| target_offering_id / target_grade_id | UUID | — | C | T1 | Known need (SR-16) | Grade 10 CBSE → Grade 11 needs a stream choice (OQ-77). |
| target_class_id | UUID | FK:CL01 | C | T1 | Assumed | Next-year class; may be left for start-of-year allocation. |
| house_code | VARCHAR(32) | — | N | T1 | Assumed (DL-76) | Copied forward. |
| line_status | ENUM | — | Y | T1 | Assumed (DL-27) | See enums. |
| approval_case_id | UUID | FK:Case | C | T2 | Confirmed (DL-62: non-stage year-end campus change needs both principals) | |
| new_enrolment_id | UUID | FK:ST02 | C | T1 | Known (SR-03 `previous_enrolment_id` link) | Set when applied. |

## Class lifecycle and how it maps to other modules

| Moment | Classes record | Other module | Rule |
|---|---|---|---|
| Plan next year | CL02 DRAFT → APPROVED | Admissions AD06 `seat_limit` updates | SR-CL-02, DL-61 |
| Create classes | CL01 DRAFT (one per planned class), CL03 class teachers | Timetable reads classes (out of scope) | DL-70, DL-73 |
| Offer accepted, student CONFIRMED | CL04 pre-placement (optional) | Admissions handoff (SR-AD-07) | SR-CL-06 |
| Year starts | CL01 ACTIVE, CL04 ALLOCATION | ST02 ENROLLED, `class_id` set | SR-07 |
| Mid-year joiner enrolled | CL04 NEW_ENROLMENT | ST02 ENROLLED | SR-CL-06 |
| Class move, AEPP exit | CL04 old row ends, new row starts | ST02 `class_id` changes; enrolment unchanged | DL-72, DL-74 |
| Year end | CL06/CL07 run → APPLIED; CL01 CLOSED | ST02 ends PROMOTION_ROLLOVER, new ST02 created | DL-75, SR-16, SR-18 |

## Field visibility by role

`M` manage, `A` approve, `V` view, `O` own class only, `S` summary (counts), `—` none. All cells Assumed (DL-26 rule) except where cited. Student fields shown on a roster follow the [Student Directory visibility table](./student-directory-data-dictionary.md#field-visibility-by-role); this table covers class records only.

| Record | Principal (campus) | Academic coordinator (programme band) | Class teacher | Subject teacher | Admissions (campus/country) | Parent / student | Group leadership |
|---|---|---|---|---|---|---|---|
| Class, room, class teacher | M | M (OQ-73) | O (V) | V (own classes) | V | own class name and class teacher (DL-68) | S |
| Class plan, seat limit | A | M (prepare, OQ-73) | — | — | V | — | S |
| Roster (names, class, house, medical alert flag, attendance threshold) | V | V | O (Known: class teacher sees their class, Arch § Security; DL-30, DL-66) | O (own classes) | V (own placements) | classmates: OQ-75 | — |
| Placement history and reason note | V | M | O (dates only) | — | V (dates only) | — | — |
| Rollover run and outcomes | A (Known: Principal approves promotion, Arch § Users and roles) | M | O (own class, read) | — | V (leavers and new admissions) | own outcome via Assessment | S |
| House allocation | V | M | O (V) | V | — | own house (DL-68) | S |

## Enumerations (candidates, DL-27 rule)

| Entity | Field | Values |
|---|---|---|
| CL01 | class_type | REGULAR, PLACEMENT |
| CL01 | class_status | DRAFT, ACTIVE, CLOSED |
| CL02 | plan_status | DRAFT, SUBMITTED, APPROVED |
| CL03 | class_role | CLASS_TEACHER, CO_CLASS_TEACHER, ASSISTANT |
| CL04 | placement_reason | NEW_ENROLMENT, ALLOCATION, ROLLOVER, CLASS_MOVE, AEPP_ENTRY, AEPP_EXIT, CORRECTION |
| CL06 | run_status | DRAFT, OUTCOMES_PENDING, READY_FOR_APPROVAL, APPROVED, APPLIED, CANCELLED |
| CL07 | promotion_outcome | PROMOTED, PROMOTED_CONDITIONAL, RETAINED, GRADUATING, LEAVING, NEW_ADMISSION_ELSEWHERE |
| CL07 | line_status | AWAITING_OUTCOME, READY, NEEDS_DECISION, AWAITING_APPROVAL_CASE, APPLIED, EXCLUDED |

## Business rules

- **SR-CL-01 (Assumed, DL-70):** A class belongs to one campus, offering, grade (or grade list for PLACEMENT) and academic year. Classes are never reused across years; a closed class and its roster are read-only.
- **SR-CL-02 (Confirmed seat rule DL-61; mechanism Assumed DL-71):** The Admissions seat limit for a grade and year is the sum of the size limits in its approved class plan. A plan can't be approved with a size limit "Not set", and can't go below enrolled + held seats.
- **SR-CL-03 (Known SR-02; Assumed DL-72):** A student has exactly one current placement per enrolment, in a class of the same campus, offering and academic year. Moving class ends the old placement and starts a new one; the enrolment continues.
- **SR-CL-04 (Assumed, DL-71):** Placing a student into a full class is blocked. The Principal may approve one over the limit with a reason, which is logged.
- **SR-CL-05 (Assumed, DL-73):** Each ACTIVE class has one current class teacher. A class without one shows "No class teacher" and can't be made ACTIVE.
- **SR-CL-06 (Known SR-07; Assumed):** A student becomes ENROLLED with a class (ST02 `class_id` required once ENROLLED). CONFIRMED students due to start within the campus's lead time appear in "Needs a class"; they can be pre-placed from their start date.
- **SR-CL-07 (Known DL-16, BR-06, SR-12):** A class teacher assignment suggests data scope for that class. Access still comes from a membership and grant.
- **SR-CL-08 (Assumed, DL-75; Known HB p.128–131 for the outcomes):** Classes never decides promotion. A rollover line waits for an outcome from Assessment, a withdrawal case or a recorded Principal or parent decision.
- **SR-CL-09 (Confirmed DL-54, DL-62, DL-63; SR-16, SR-18):** Applying a run ends each enrolment with PROMOTION_ROLLOVER and creates the next-year enrolment linked by `previous_enrolment_id`. Stage-campus promotion needs no extra approval. Any other year-end campus change needs both principals (approval case). A year-end move into another brand or country is excluded and goes to Admissions as a new admission.
- **SR-CL-10 (Known, GIIS SG, HB p.128):** In Kindergarten to Grade 5 (CBSE/IB PYP) a student is retained only on the parents' written request. Recorded as a GIIS Singapore campus setting, not a group rule.
- **SR-CL-11 (Known, GIIS SG, HB p.129–131):** Conditional promotion depends on re-tests; the line stays NEEDS_DECISION until the result is in (timing OQ-79).
- **SR-CL-12 (Assumed, DL-76):** House is copied forward at rollover and kept on class moves. Allocation rules (siblings, balance) are open (OQ-76).
- **SR-CL-13 (Assumed, DL-59 pattern):** Class size limits per grade and the house list are campus settings with no group default; unset shows "Not set" and blocks plan approval or house allocation.
- **SR-CL-14 (Known DL-30, DL-66):** Rosters show the medical alert flag and attendance threshold only; never the medical detail, Student Pass or immigration status.

## Events

ClassPlanApproved (AD06 recalculates), ClassCreated, ClassActivated, ClassClosed, ClassTeacherAssigned, StudentPlaced, StudentClassMoved, HouseAssigned, RolloverRunApproved, RolloverApplied (with StudentEnrolled per new enrolment, student dictionary). Events carry IDs only (architecture.md).

## Capabilities (new, DL-53 naming)

`classes.class.view`, `.manage` · `classes.roster.view` (scoped to own classes for teachers) · `classes.plan.edit`, `.approve` · `classes.staff.assign` · `classes.placement.manage`, `.approve_over_limit` · `classes.house.assign` · `classes.rollover.prepare`, `.approve`.

## Decisions proposed (Assumed, pending Henry)

| ID | Decision | Why |
|---|---|---|
| DL-70 | A class is one group for one campus × offering × grade × academic year; next year's classes are new records, linked only by a suggested `previous_class_id`. Classes is a feature area of the Academic structure domain (DL-43), shown in the staff app beside the Student directory, with capability prefix `classes` | Keeps past rosters intact (SR-03 pattern); matches how the handbook lists classes per year |
| DL-71 | The Principal's class plan (classes × size limit per grade and year) is the only place the seat limit is set; AD06 reads it. Placing over a class's size limit needs the Principal | DL-61 says seat limit = classes × class size; one number, one writer |
| DL-72 | Placement history (CL04) is the single writer of a student's class; an in-year class move within the same campus, offering and year doesn't end the enrolment | ST02 `class_id` needs history without a new enrolment for every class change |
| DL-73 | Class teacher and co-teacher are effective-dated assignments on the class; they suggest data scope (DL-16), never grant access | Class teacher is a class fact; subject teaching stays in Academic Allocation |
| DL-74 | AEPP 1 and AEPP 2 are PLACEMENT classes that can take several grades; students keep their grade and programme; leaving AEPP is a class move | DL-65 and HB p.14 (AEPP rooms) |
| DL-75 | Year-end rollover is one run per campus and year: Assessment supplies outcomes, Classes builds next-year enrolments and classes, the Principal approves before it applies | Promotion rules live in Assessment (Arch § Key workflows); Principal approves promotion (Arch § Users and roles) |
| DL-76 | House stays on ST02 `house_code`; Classes allocates it from the campus house list and copies it forward at rollover and on class moves | Houses run across years (HB p.169) |

## Open questions (new)

| ID | Question | Why it matters for design |
|---|---|---|
| OQ-73 | Who prepares the class plan, allocates students and moves them during the year (coordinator, vice principal, office), and does a class move need approval or a notice to parents? | Who sees the allocation screens and whether a move is one step or an approval |
| OQ-74 | What class size limits apply per brand, country, programme and grade (board, regulator or early-years ratios)? | Class plan defaults and over-limit warnings |
| OQ-75 | When are class lists published, and what do parents and students see (class teacher, classmates' names)? | Parent app class card and publishing step |
| OQ-76 | How are houses allocated (siblings together, balanced by size), and does a student keep their house after a campus transfer or stage-campus promotion? | House allocation suggestions (with OQ-20 for the house list) |
| OQ-77 | Are streams (CBSE 11 Science, Commerce) and elective sets part of the class, or subject groups owned by Teaching? | Class names like "11 Science A"; Grade 10 → 11 rollover needs a stream |
| OQ-78 | Do any campuses run composite or mixed-age classes (Montessori/GMP, small campuses), beyond AEPP? | Whether REGULAR classes can span grades or offerings |
| OQ-79 | When results arrive after the rollover (re-tests, conditional promotion, Grade 10 and 12 board results), how and by when is the student's class changed, and who decides retention? | Late changes to an applied rollover |

## Readiness

- **Can be designed now:** class overview per campus and year, class detail with roster and class teacher, class plan with seat limit feeding Admissions, "Needs a class" queue and class move, AEPP exit, rollover run review with GIIS Singapore sample outcomes, house list and allocation.
- **Keep generic, label Pending:** who allocates and approves moves (OQ-73), size limit defaults (OQ-74), parent view of classmates (OQ-75), house rules (OQ-76), streams (OQ-77), late result changes (OQ-79).

# Global Campus OS — Attendance Data Dictionary v0.1 (draft)

**Status:** Proposed draft, 19 Nov 2026. DL-97…DL-108 all Confirmed (Henry, 21 Nov 2026); OQ-108 answered by DL-103 and DL-108; OQ-109…OQ-121 Open · **Owner:** Henry
**Cites:** [Architecture v1](../00-sources/Global_Campus_OS_Product_Architecture_v1.md) § Principles (policy as rules; campus calendar engine), § Context (attendance thresholds, ICA notice), § Users and roles (Class teacher "daily attendance, leave intake"; Principal "approvals (leave over 2 days)"; Programme or level coordinator "early leave approval"; Security / reception "late sign-in, out pass"; Parent "leave requests"), § Domains and modules (module 5 Attendance and Conduct), § Key workflows (Absence and leave, steps 1–4), § Data model (Attendance and conduct row: Attendance mark, Leave request, Gate log; Case object for leave over 2 days and Student Pass issues; event log), § Integrations (access control and gate devices; ICA case exports), § Security (immutable audit of attendance edits and ICA notifications), § Roadmap (Phase 1 daily loop pilot: attendance and leave) · [Student Directory Spec](./student-directory-data-dictionary.md) ST02, ST08 (medical certificate), ST09, SR-09, DL-30, OQ-41 · [Classes dictionary](./classes-data-dictionary.md) CL01, CL03, CL04, DL-73 · [Assessment dictionary](./assessment-data-dictionary.md) AS05 `min_attendance_percent`, AS10 `attendance_snapshot`, DL-92, DL-95 · [Fees dictionary](./fees-billing-data-dictionary.md) DL-83 · [architecture.md](./architecture.md) DL-43 domains · GIIS Student Handbook 2025-26 (HB) p.23 (attendance, late sign-in, out pass, leave over two days), p.128–131 (promotion attendance), p.166–167 (Student Attendance Policy, ICA), p.174–175 (late arrival) · GIIS SG Holiday Schedule 2026-27 · DL-11, DL-16, DL-22, DL-26, DL-27, DL-30, DL-32, DL-47, DL-53, DL-59, DL-66, DL-68, DL-73, DL-83, DL-92, DL-95, DL-96
**Tóm tắt (VI):** Từ điển dữ liệu cho module Điểm danh: ngày học (từ lịch năm học theo chương trình, trừ ngày lễ và kỳ nghỉ), sổ điểm danh hằng ngày của giáo viên chủ nhiệm (có giờ khoá sổ và sửa sau khoá phải có lý do), đơn xin nghỉ của phụ huynh (1–2 ngày giáo viên chủ nhiệm duyệt, trên 2 ngày Hiệu trưởng duyệt, nghỉ ốm cần giấy bác sĩ), sổ ra vào cổng (đến muộn, xin về sớm), ngưỡng chuyên cần và cảnh báo sớm, báo cáo ICA cho học sinh giữ Student Pass ở Singapore, và tỷ lệ chuyên cần gửi sang Đánh giá. Mỗi trường có trạng thái Known / Assumed / Open.

> Drafted by Claude from product truth on 19 Nov 2026. The architecture names the module "Attendance and Conduct"; this file covers **Attendance** only. Conduct (incidents, sanctions, house points) gets its own dictionary later (DL-97).

## How to read this file

Same layout and rules as the [Student Directory Spec](./student-directory-data-dictionary.md#how-to-read-this-file): **Known** (cited), **Assumed** (DL), **Open** (OQ); tiers T1/T2/T3 (DL-11; a student's attendance record is Tier 2, absence reasons are Tier 2, medical certificates and anything about a Student Pass are Tier 3); `Y`/`N`/`C` required. Every row also carries `tenant_path`, `data_region` (UI term **Data location**, DL-47) and the shared common fields. Facts from the GIIS Handbook are **GIIS Singapore examples**, not group rules (DL-22). All enums are unapproved candidates (DL-27 rule). All example values are synthetic.

## Scope

Approved by Henry, 19 Nov 2026: **whether each student was in school each school day, why not, and what happens when attendance falls too low.**

| In this dictionary | Not in this dictionary (owner) |
|---|---|
| School days per campus × programme offering × year: term dates (DL-96) minus holidays, breaks and closures | The full campus calendar engine: events, PTMs, exam windows, segment calendars (Calendar, later) |
| Daily registers per class, marked by the class teacher; period registers where a campus turns them on | Timetable and periods ([Teaching & Timetable](./teaching-timetable-data-dictionary.md) TE02, TE11; this file reads periods) |
| Attendance marks per student per day (OBJ-20), corrections after lock | Conduct: incidents, sanctions, digital badges, house points (Conduct, later dictionary) |
| Leave requests from parents, routing and approval, medical certificates | Staff attendance and staff leave (Staff and HR) |
| Gate log: late sign-in and out pass at reception or security | Bus boarding and transport attendance (Transport) |
| Attendance thresholds per campus and student group, attendance % and early warnings | Gate device and biometric hardware setup (Integrations) |
| ICA notices for Student Pass holders (Singapore), raised as a Case | The Student Pass itself and immigration status (Student Records ST09, Tier 3) |
| Attendance % per reporting period for report cards and promotion | Promotion decisions (Assessment AS12) and report cards (AS10) |

## Modeling boundaries

- **Attendance is its own module; Conduct follows separately (DL-97).** Attendance owns school days, registers, marks, leave, the gate log, thresholds and attendance alerts. The capability prefix is `attendance` (DL-53); the module team key stays `attendance_conduct` until Conduct is designed (modules-and-services.md, S4 flow). Both are feature areas of the Student Records domain (DL-43).
- **School days come from the year, not from the register (DL-98).** A school day is a weekday inside the term dates of a campus × programme offering × academic year (DL-96) that isn't a holiday, break or closure. Attendance keeps the non-school-day list for each campus × offering × year until a calendar engine exists. GIIS SG 2026-27 lists 17 public holidays, four vacations and breaks and **196 study days** (Holiday Schedule 2026-27). The attendance % denominator is school days, never the number of registers taken.
- **One day mark per student per school day is the record (DL-99).** The class teacher's register produces one mark per student per school day: PRESENT, LATE, LEFT_EARLY, ABSENT, ON_LEAVE or SCHOOL_ACTIVITY. Period registers are a campus setting; when on, the day mark is derived from the periods and the class teacher confirms it. A student who changes class mid-day keeps one day mark (CL04).
- **Registers lock; changes after lock need a reason (DL-100).** A register is due by a campus cut-off time (GIIS SG: students arrive before 9:00, HB p.174) and locks at the end of the school day. After lock, a mark changes only through a correction with a reason, approved by the coordinator (`attendance.mark.edit_after_lock`, S4 flow). Every change is audited (Arch § Security: "attendance edits").
- **Parents ask for leave in the app; length decides who approves (DL-101).** A parent submits leave with dates, type and reason. One or two school days go to the class teacher; more than two go to the Principal, with the class teacher and coordinator informed (HB p.23, p.166; Arch step 2). Medical leave needs a medical certificate, uploaded on the student's return (HB p.23). Approved leave turns those days' marks into ON_LEAVE. Email, letters, diary notes and phone calls are replaced by the app; a staff member can record a request on a parent's behalf.
- **The gate log feeds the register (DL-102).** Reception or security records a late sign-in (time and the reason the student or parent gives) and an out pass (time out, who collected the student, who approved). Early exit needs the class teacher's or level coordinator's approval before the student leaves (HB p.23). A late sign-in sets the day mark to LATE and an out pass sets LEFT_EARLY; the class teacher can see both on the register. Gate devices can send the same events later (Arch § Integrations).
- **Attendance % is calculated, never typed (DL-103).** For each enrolment and measurement period: days attended ÷ school days in that period from the enrolment start date. PRESENT, LATE, LEFT_EARLY and SCHOOL_ACTIVITY count as attended. ABSENT and ON_LEAVE do not, including approved medical leave (Confirmed DL-103, answers OQ-108). Approved leave still counts as a valid reason for the ICA rule (DL-105). The figure is recalculated when a mark changes.
- **Thresholds are campus rules per student group, with early warning (DL-104).** A threshold rule says who it applies to (Student Pass holders; a grade band), the minimum %, and the period it's measured over (month, term or semester). GIIS SG: Student Pass holders 90% per month; others from Class 6 85% per semester; others KG to Class 5 75% per term, which the Principal may relax (HB p.166). Each student gets one threshold per rule; class teachers see the threshold, never why it applies (DL-30). The system projects each student's % to the period end and warns the class teacher, coordinator and parent when it is at risk (Arch step 4). Thresholds are settings with no group default (DL-59 pattern).
- **ICA notices are a Case for the Tier 3 team, never automatic (DL-105).** In Singapore, a Student Pass holder absent for more than 7 consecutive school days, or under 90% for a month without a valid reason, must be reported to ICA (HB p.166–167). The rule opens a Student Pass Case (OBJ-10) for the named Tier 3 owner (Admissions/registrar, who handles Student Pass matters, HB p.235). That person checks the reason and files the notice; the system records the filing but never sends it on its own. Teachers see only "Attendance below threshold", never the ICA link (DL-30).
- **Other modules read a summary, never reasons (DL-106).** Attendance publishes, per enrolment and reporting period, school days, days attended and the % (AS10 `attendance_snapshot`, AS05 promotion threshold, DL-92). Absence reasons, leave notes and medical certificates never leave Attendance (DL-95, DL-66 pattern). Fee status never blocks marking, leave or anything else here (DL-83, Confirmed).
- **Parents see their own child's day the same day (DL-107).** Parents see each day's mark, late and out-pass times, leave status and the attendance % against the threshold for their own children. If a student is ABSENT at the cut-off with no approved leave, the parent gets a same-day notice. Students see their own attendance per campus setting (OQ-117). Parents never see other students' marks.
- **Data location.** School days, registers, marks, leave, gate log and alerts are held in the campus's data location (DL-47). Group dashboards get counts and % only.

## Entities

| ID | Entity | Module | Status |
|---|---|---|---|
| AT01 | School Day Calendar | Attendance (until the Calendar engine exists) | Known need (Arch § Principles 4: "attendance… reads from it"; Holiday Schedule 2026-27); keyed per offering (DL-96); model Confirmed (DL-98) |
| AT02 | Non-school Day | Attendance | Known for GIIS SG (Holiday Schedule 2026-27); model Confirmed (DL-98) |
| AT03 | Register | Attendance | Known need (Arch § Users and roles "Class teacher: daily attendance"; step 3 "by period or by day"); model Confirmed (DL-99, DL-100) |
| AT04 | Attendance Mark | Attendance (OBJ-20) | Known entity (Arch § Data model "Attendance mark"); values Confirmed (DL-99) |
| AT05 | Mark Correction | Attendance | Known need (Arch § Security "attendance edits" audited; S4 `attendance.mark.edit_after_lock`); model Confirmed (DL-100) |
| AT06 | Leave Request | Attendance | Known entity (Arch § Data model "Leave request"; HB p.23, p.166); routing Known for GIIS SG; model Confirmed (DL-101) |
| AT07 | Gate Event | Attendance | Known entity (Arch § Data model "Gate log covers late sign-in and out pass"; HB p.23, p.174); model Confirmed (DL-102) |
| AT08 | Attendance Rule | Attendance (Rules service) | Known values for GIIS SG (HB p.166); model Confirmed (DL-104) |
| AT09 | Attendance Summary | Attendance | Known need (Arch step 4 "projects each student's attendance"; AS10 attendance snapshot); calculated (DL-103) |
| AT10 | Attendance Alert | Attendance (opens Cases) | Known need (Arch step 4 "warns teacher, coordinator and parent early"; HB p.166–167 ICA); model Confirmed (DL-104, DL-105) |

ST02, ST08, ST09, CL01, CL03, CL04, AS05, AS10 and the Case object (OBJ-10) are referenced, not redefined. The medical certificate is stored as an ST08 document of type Medical certificate, linked from AT06 (ST08 notes that it belongs to Attendance).

## Detailed attribute dictionary

### AT01 — School Day Calendar

**Purpose:** The school days of one campus × programme offering × academic year. The denominator for every attendance %.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| calendar_id | UUID | PK | Y | T1 | Confirmed (DL-98) | |
| campus_id, programme_offering_id, academic_year_id | UUID | UK; FK:OR04 / AC06 / AC07 | Y | T1 | Confirmed key (DL-96) | GIIS SG CBSE starts 6 Apr 2026; IB DP at SG-ECS starts 13 Jul 2026. |
| term_dates | JSON | — | Y | T1 | Confirmed source (DL-96) | Read from the year and term dates, never retyped here. |
| school_week | VARCHAR(16) | — | Y | T1 | Assumed; campus setting | e.g. MON-FRI. Saturday schooling: OQ-119. |
| school_day_count | INT | — | Y | T1 | Known for GIIS SG ("Total no of study days: 196", Holiday Schedule 2026-27); derived | Weekdays in term dates minus AT02 days. Shown so the office can check it against the published almanac. |
| calendar_status | ENUM | — | Y | T1 | Assumed (DL-27) | DRAFT, PUBLISHED. Registers can't open on a DRAFT calendar. |
| published_by_user_id / published_at | — | — | C | T1 | Open (who maintains it, OQ-120) | |

### AT02 — Non-school Day

**Purpose:** A day or range inside the term dates when there is no school for some or all of the campus.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| non_school_day_id | UUID | PK | Y | T1 | Confirmed (DL-98) | |
| calendar_id | UUID | FK:AT01 | Y | T1 | Assumed | |
| date_from / date_to | DATE | — | Y | T1 | Known examples (Holiday Schedule: Deepavali 9–10 Nov 2026; December vacation 5 Dec 2026 – 3 Jan 2027) | Inclusive. |
| day_type | ENUM | — | Y | T1 | Known kinds (Holiday Schedule: holidays, vacation, semester break); CLOSURE Assumed | PUBLIC_HOLIDAY, VACATION, TERM_BREAK, SCHOOL_HOLIDAY, CLOSURE. |
| name | VARCHAR(80) | — | Y | T1 | Known | "Deepavali", "Sem 1 Break". |
| applies_to_grades | JSON | — | N | T1 | Assumed (Arch § Principles 4: scoped "by segment") | Null = whole offering. A grade-only day (e.g. exam study leave) is OQ-119. |
| added_after_publish_reason | TEXT | — | C | T1 | Assumed | Required for an unplanned closure after the calendar is published; marks already taken that day are voided with an audit entry. |

### AT03 — Register

**Purpose:** One class's attendance for one school day (or one period, where period registers are on).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| register_id | UUID | PK | Y | T1 | Confirmed (DL-99) | |
| class_id | UUID | FK:CL01 | Y | T1 | Known (Arch § Users and roles: class teacher scope is the class) | Students listed are the class's placements valid that day (CL04). |
| school_date | DATE | UK with class + period | Y | T1 | Assumed | Must be a school day in AT01. |
| register_kind | ENUM | — | Y | T1 | Known both exist (Arch step 3 "by period or by day"); campus setting Confirmed (DL-99) | DAY or PERIOD. |
| period_id | UUID | FK:TE02 | C | T1 | Assumed | Required when PERIOD. The bell period of the dated lesson (TE11) in the [teaching & timetable dictionary](./teaching-timetable-data-dictionary.md). |
| taken_by_employee_id | UUID | FK:E01 | Y | T1 | Known: class teacher (Arch § Users and roles); cover teacher Assumed | Class teacher, co-teacher or a cover teacher with that day's scope (TE13, DL-145; who takes the day register OQ-165). |
| due_at | TIMESTAMPTZ | — | Y | T1 | Assumed; campus setting (OQ-110) | From the campus register cut-off. Late registers show "Overdue" to the coordinator. |
| register_status | ENUM | — | Y | T1 | Assumed (DL-27) | NOT_TAKEN, SUBMITTED, LOCKED. See enums. |
| submitted_at / locked_at | TIMESTAMPTZ | — | C | T1 | Confirmed (DL-100) | Lock is automatic at the campus end-of-day time. |

### AT04 — Attendance Mark

**Purpose:** Whether one student was in school on one school day (or one period), and the code for why not. OBJ-20.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| mark_id | UUID | PK | Y | T2 | Known entity (Arch § Data model "Attendance mark") | |
| register_id | UUID | FK:AT03 | Y | T2 | Assumed | |
| enrolment_id | UUID | FK:ST02 | Y | T2 | Known (attendance belongs to the enrolment, ST02 events) | |
| school_date | DATE | — | Y | T2 | Assumed | Denormalised for summaries. |
| mark_code | ENUM | — | Y | T2 | Known kinds (HB p.23: late, early exit, leave, medical leave); codes Confirmed (DL-99) | PRESENT, LATE, LEFT_EARLY, ABSENT, ON_LEAVE, SCHOOL_ACTIVITY. See enums. |
| counts_as_attended | BOOLEAN | — | Y | T2 | Confirmed (DL-103); derived | True for PRESENT, LATE, LEFT_EARLY, SCHOOL_ACTIVITY. ON_LEAVE is not attended (DL-103). |
| absence_category | ENUM | — | C | T2 | Assumed (DL-27) | For ABSENT and ON_LEAVE: MEDICAL, FAMILY, TRAVEL, RELIGIOUS, OTHER, UNEXPLAINED. Never a diagnosis. |
| valid_reason | BOOLEAN | — | C | T2 | Known need (HB p.166: ICA "without a valid reason"); who decides Open (OQ-112) | True when covered by approved leave or a medical certificate. Drives the ICA rule (DL-105). |
| arrival_time / departure_time | TIME | — | C | T2 | Known (HB p.23, p.174 sign-in and out pass) | From AT07 for LATE and LEFT_EARLY. |
| source | ENUM | — | Y | T1 | Assumed | TEACHER, GATE, LEAVE, PERIODS, CORRECTION, SYSTEM. |
| leave_request_id | UUID | FK:AT06 | C | T2 | Assumed | Set when ON_LEAVE. |
| gate_event_id | UUID | FK:AT07 | C | T2 | Assumed | Set when LATE or LEFT_EARLY came from the gate. |
| teacher_note | VARCHAR(280) | — | N | T2 | Known for lates (HB p.174: "Class teachers to record the reason for late coming, as stated by the student") | Not shown to other parents; not exported (DL-106). |
| version | INT | — | Y | T1 | Confirmed (DL-100) | Increments on each correction. |

### AT05 — Mark Correction

**Purpose:** A change to a mark after its register locked. The audit trail behind every edited mark.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| correction_id | UUID | PK | Y | T2 | Confirmed (DL-100) | |
| mark_id | UUID | FK:AT04 | Y | T2 | Assumed | |
| from_code / to_code | ENUM | — | Y | T2 | Assumed | |
| reason | TEXT | — | Y | T2 | Known need (Arch § Security "attendance edits" in the immutable audit log) | Required. |
| requested_by_user_id | UUID | FK:A01 | Y | T1 | Assumed | Class teacher, office, or SYSTEM when leave is approved late. |
| approved_by_user_id | UUID | FK:A01 | C | T1 | Assumed (S4 `attendance.mark.edit_after_lock`); approver Open (OQ-110) | Not needed when an approved leave request or a gate event changes the mark. |
| correction_status | ENUM | — | Y | T1 | Assumed (DL-27) | REQUESTED, APPLIED, REJECTED. |

### AT06 — Leave Request

**Purpose:** A parent's request for a student to be away from school on given days, and the decision.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| leave_request_id | UUID | PK | Y | T2 | Known entity (Arch § Data model "Leave request") | |
| enrolment_id | UUID | FK:ST02 | Y | T2 | Known | |
| requested_by_guardian_id | UUID | FK:ST04 | C | T2 | Known (HB p.166: "should be made by the parent"; Arch § Users and roles: parent leave requests) | Needs an active family link (F01). |
| recorded_by_user_id | UUID | FK:A01 | C | T1 | Confirmed (DL-101) | When office or the class teacher enters a request for a parent. |
| date_from / date_to | DATE | — | Y | T2 | Known (HB p.166 "the specific dates for which leave is sought") | Inclusive. Partial days: OQ-111. |
| school_days_count | INT | — | Y | T1 | Assumed; derived from AT01 | Drives routing. Weekends and holidays don't count. |
| leave_type | ENUM | — | Y | T2 | Known: medical, pre-planned (HB p.23); others Assumed (DL-27) | MEDICAL, FAMILY, TRAVEL, RELIGIOUS, COMPETITION, OTHER. |
| planned | BOOLEAN | — | Y | T2 | Known (HB p.23 "pre-planned leave of more than two days") | False = told after the absence started. |
| reason | TEXT | — | Y | T2 | Known (HB p.166 "giving details of reasons") | Seen by the approver and class teacher only. Never a diagnosis needed. |
| approval_route | ENUM | — | Y | T1 | Known for GIIS SG (HB p.23, p.166; Arch step 2); thresholds a campus setting (DL-59 pattern) | CLASS_TEACHER (1–2 school days), PRINCIPAL (more than 2). |
| approver_user_id | UUID | FK:A01 | C | T1 | Known roles (HB p.166); delegate Open (OQ-109) | |
| certificate_required | BOOLEAN | — | Y | T1 | Known (HB p.23 "Medical leaves must be supported with a medical certificate"; HB p.166 Student Pass holders) | True for MEDICAL. Due on return (HB p.23). |
| certificate_document_id | UUID | FK:ST08 | C | T3 | Known type (ST08 "Medical certificate") | Tier 3 file; the approver and class teacher see "Certificate received", the file only with `attendance.certificate.view`. |
| case_id | UUID | FK:CC03 | C | T2 | Known (Arch § Data model: Case covers "leave over 2 days") | Created for PRINCIPAL route so the decision has SLA and history. |
| request_status | ENUM | — | Y | T2 | Assumed (DL-27) | SUBMITTED, APPROVED, DECLINED, CANCELLED, CERTIFICATE_DUE, CLOSED. |
| decision_note | TEXT | — | C | T2 | Assumed | Required to decline. Shown to the parent. |

### AT07 — Gate Event

**Purpose:** A late arrival or early exit recorded at reception or security.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| gate_event_id | UUID | PK | Y | T2 | Known entity (Arch § Data model "Gate log") | |
| enrolment_id | UUID | FK:ST02 | Y | T2 | Known | |
| event_type | ENUM | — | Y | T2 | Known (HB p.23 "sign in… sign out at the Out Pass") | LATE_SIGN_IN, OUT_PASS, RETURN. |
| event_time | TIMESTAMPTZ | — | Y | T2 | Known | |
| stated_reason | VARCHAR(200) | — | N | T2 | Known (HB p.174: reason "as stated by the student"; parents note arrival time and reason) | |
| late_exemption | ENUM | — | N | T1 | Known for GIIS SG (HB p.174: school bus, doctor's note, court documents, prior permission) | SCHOOL_BUS, DOCTOR_NOTE, PRIOR_PERMISSION, OTHER. An exempt late doesn't count toward the late-arrival steps. |
| out_pass_approved_by_user_id | UUID | FK:A01 | C | T1 | Known (HB p.23: "only on approval from the Level Coordinator / Class Teacher") | Required for OUT_PASS. |
| collected_by | VARCHAR(80) | — | C | T2 | Assumed | Guardian or authorised adult who collected the student. Authorised-pickup list: OQ-116. |
| recorded_by_user_id | UUID | FK:A01 | Y | T1 | Known role (Arch § Users and roles "Security / reception") | |
| source | ENUM | — | Y | T1 | Known need (Arch § Integrations: gate devices, inbound events, phase 2) | DESK, DEVICE. |

### AT08 — Attendance Rule

**Purpose:** A threshold a campus applies to a group of students, and what happens when it is missed.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| rule_id | UUID | PK | Y | T1 | Known (Arch § Data model: "Policy, Rule version, Threshold") | |
| campus_id | UUID | FK:OR04 | Y | T1 | Confirmed (DL-104, DL-59 pattern) | Brand-country default copied: OQ-113. |
| applies_to | JSON | — | Y | T1 | Known for GIIS SG (HB p.166) | Student group: STUDENT_PASS (from ST09, Tier 3 lookup by the rules engine only), grade band, or ALL. |
| minimum_percent | DECIMAL(5,2) | — | Y | T1 | Known for GIIS SG: 90 / 85 / 75 (HB p.166) | |
| measured_over | ENUM | — | Y | T1 | Known for GIIS SG: month, semester, term (HB p.166) | MONTH, TERM, SEMESTER, YEAR. |
| warn_margin_points | DECIMAL(4,1) | — | Y | T1 | Confirmed (DL-104) | Projection within this many points of the minimum → AT_RISK. Campus setting, no default. |
| consequence | ENUM | — | Y | T1 | Known for GIIS SG (HB p.128–131 promotion; p.166–167 ICA) | WARN_ONLY, PROMOTION_INPUT, ICA_CASE. A rule can have several. |
| principal_may_relax | BOOLEAN | — | Y | T1 | Known for GIIS SG KG–5 (HB p.166 "can be relaxed at the discretion of the Principal") | |
| valid_from_year_id | UUID | FK:AC07 | Y | T1 | Assumed | Versioned per year like other rules. |

### AT09 — Attendance Summary

**Purpose:** The calculated attendance of one enrolment over one measurement or reporting period.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| summary_id | UUID | PK | Y | T2 | Confirmed (DL-103) | |
| enrolment_id | UUID | FK:ST02 | Y | T2 | Known | |
| period_kind / period_start / period_end | — | UK with enrolment | Y | T2 | Assumed | MONTH, TERM, SEMESTER, YEAR, REPORTING_PERIOD (AS02 periods). |
| school_days | INT | — | Y | T2 | Assumed; derived | From AT01, counted from the enrolment start (OQ-115). |
| days_attended / days_absent / days_on_leave / late_count | INT | — | Y | T2 | Assumed; derived | |
| attendance_percent | DECIMAL(5,2) | — | Y | T2 | Confirmed (DL-103) | Shown to one decimal. Blank before the first school day. |
| projected_percent | DECIMAL(5,2) | — | C | T2 | Known need (Arch step 4 "projects") | Best case if present every remaining day; method Assumed. |
| threshold_percent | DECIMAL(5,2) | — | C | T1 | Confirmed display (DL-30) | The applicable AT08 minimum; never its reason. |
| standing | ENUM | — | C | T2 | Confirmed (DL-104) | ON_TRACK, AT_RISK, BELOW, CANNOT_RECOVER. |

### AT10 — Attendance Alert

**Purpose:** A notice raised by an attendance rule: to whom, about what, and what was done.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| alert_id | UUID | PK | Y | T2 | Assumed | |
| enrolment_id | UUID | FK:ST02 | Y | T2 | Known | |
| alert_type | ENUM | — | Y | T2 | Known triggers (HB p.166–167, p.174; Arch step 4); codes Assumed | UNEXPLAINED_ABSENCE, REPEATED_LATE, AT_RISK, BELOW_THRESHOLD, CONSECUTIVE_ABSENCE, ICA_REVIEW. |
| rule_id | UUID | FK:AT08 | C | T1 | Assumed | |
| raised_at | TIMESTAMPTZ | — | Y | T1 | Assumed | |
| audience | JSON | — | Y | T1 | Known (Arch step 4: teacher, coordinator, parent) | ICA_REVIEW goes to the Tier 3 owner only (DL-105). |
| case_id | UUID | FK:CC03 | C | T3 for ICA | Known (Arch § Data model: Case covers Student Pass issues) | Set for ICA_REVIEW and any alert escalated to a Case. |
| ica_filed_at / ica_reference | — | — | C | T3 | Known need (Arch § Security: ICA notifications audited; § Integrations "Case exports") | Recorded by the Tier 3 owner after filing. |
| alert_status | ENUM | — | Y | T1 | Assumed (DL-27) | OPEN, ACKNOWLEDGED, RESOLVED, DISMISSED. Dismissing needs a reason. |

## Attendance day and how it maps to other modules

| Moment | Attendance record | Other module | Rule |
|---|---|---|---|
| Year set up | AT01 DRAFT → PUBLISHED with AT02 holidays | Reads year and term dates (DL-96) | SR-AT-01 |
| Parent applies for leave ahead | AT06 SUBMITTED → APPROVED | Case for more than 2 days | SR-AT-06, SR-AT-07 |
| Student arrives late | AT07 LATE_SIGN_IN | — | SR-AT-09 |
| Register due | AT03 NOT_TAKEN → SUBMITTED; AT04 marks | Parent notice for unexplained absence | SR-AT-03, SR-AT-12 |
| Out pass | AT07 OUT_PASS; mark → LEFT_EARLY | — | SR-AT-09 |
| End of day | AT03 LOCKED; AT09 recalculated | Rules engine checks AT08 | SR-AT-04, SR-AT-10 |
| Student Pass holder absent day 8 | AT10 ICA_REVIEW + Case | Admissions/registrar (Tier 3) | SR-AT-11 |
| Reporting period ends | AT09 REPORTING_PERIOD | AS10 attendance snapshot; AS12 promotion input | SR-AT-13 |
| Student moves class | Day mark stays on the enrolment | CL04 | SR-AT-03 |
| Enrolment ends | No new marks after `end_date` (last attendance date) | ST02 (HB p.234) | SR-AT-14 |

## Field visibility by role

`M` manage, `A` approve, `V` view, `O` own class only, `S` summary (counts and %), `—` none. All cells Assumed (DL-26 rule) except where cited.

| Record | Principal (campus) | Coordinator (programme band) | Class teacher | Subject teacher | Reception / security | Admissions / registrar (Tier 3) | Parent | Student | Group leadership |
|---|---|---|---|---|---|---|---|---|---|
| School days and holidays | M | V | V | V | V | V | V (campus calendar) | V | S |
| Register and marks | V | V, A corrections (OQ-110) | O (M) (Known: Arch § Users and roles) | own periods (period registers only) | — | V (Student Pass holders) | own child (DL-107) | own (OQ-117) | S |
| Absence category, teacher note | V | V | O | — | — | V | own child's category only | — | — |
| Leave request and reason | A (more than 2 days, Known HB p.23) | V | O, A (1–2 days, Known HB p.166) | — | — | V | own (M) | — | — |
| Medical certificate file (T3) | V (with `attendance.certificate.view`) | — | "Received" only | — | — | V | own upload | — | — |
| Gate log | V | V, A out pass (Known HB p.23) | O, A out pass | — | M (Known Arch § Users and roles) | — | own child's times | — | — |
| Attendance %, threshold | V | V | O (threshold, not why; DL-30 Confirmed) | — | — | V | own child | own (OQ-117) | S |
| ICA review Case | V | — | — | — | — | M | — | — | — |

## Enumerations (candidates, DL-27 rule)

| Entity | Field | Values |
|---|---|---|
| AT01 | calendar_status | DRAFT, PUBLISHED |
| AT02 | day_type | PUBLIC_HOLIDAY, VACATION, TERM_BREAK, SCHOOL_HOLIDAY, CLOSURE |
| AT03 | register_kind | DAY, PERIOD |
| AT03 | register_status | NOT_TAKEN, SUBMITTED, LOCKED |
| AT04 | mark_code | PRESENT, LATE, LEFT_EARLY, ABSENT, ON_LEAVE, SCHOOL_ACTIVITY |
| AT04 | absence_category | MEDICAL, FAMILY, TRAVEL, RELIGIOUS, OTHER, UNEXPLAINED |
| AT04 | source | TEACHER, GATE, LEAVE, PERIODS, CORRECTION, SYSTEM |
| AT05 | correction_status | REQUESTED, APPLIED, REJECTED |
| AT06 | leave_type | MEDICAL, FAMILY, TRAVEL, RELIGIOUS, COMPETITION, OTHER |
| AT06 | approval_route | CLASS_TEACHER, PRINCIPAL |
| AT06 | request_status | SUBMITTED, APPROVED, DECLINED, CANCELLED, CERTIFICATE_DUE, CLOSED |
| AT07 | event_type | LATE_SIGN_IN, OUT_PASS, RETURN |
| AT07 | late_exemption | SCHOOL_BUS, DOCTOR_NOTE, PRIOR_PERMISSION, OTHER |
| AT08 | measured_over | MONTH, TERM, SEMESTER, YEAR |
| AT08 | consequence | WARN_ONLY, PROMOTION_INPUT, ICA_CASE |
| AT09 | standing | ON_TRACK, AT_RISK, BELOW, CANNOT_RECOVER |
| AT10 | alert_type | UNEXPLAINED_ABSENCE, REPEATED_LATE, AT_RISK, BELOW_THRESHOLD, CONSECUTIVE_ABSENCE, ICA_REVIEW |
| AT10 | alert_status | OPEN, ACKNOWLEDGED, RESOLVED, DISMISSED |

## Business rules

- **SR-AT-01 (Confirmed, DL-98; Confirmed key DL-96):** School days are the weekdays inside the term dates of a campus × offering × year, minus AT02 days. Registers can only be taken on a school day of a PUBLISHED calendar.
- **SR-AT-02 (Confirmed, DL-98):** A closure added after publish voids marks already taken that day (audited) and removes the day from every %.
- **SR-AT-03 (Confirmed, DL-99):** One day mark per enrolment per school day. With period registers on, the day mark is derived (PRESENT if present in the first period, LATE if first seen later, LEFT_EARLY if absent from the last period; rule OQ-110) and the class teacher confirms it.
- **SR-AT-04 (Confirmed, DL-100):** A register locks at the campus end-of-day time. After lock, a mark changes only by an AT05 correction with a reason, approved by the coordinator, except when an approved leave request or a gate event changes it.
- **SR-AT-05 (Known, Arch § Security):** Every mark change, correction, leave decision and ICA filing is in the immutable audit log.
- **SR-AT-06 (Known for GIIS SG, HB p.23, p.166; Arch step 2):** Leave of 1–2 school days goes to the class teacher; more than 2 goes to the Principal with the class teacher and coordinator informed and a Case created. The day counts are campus settings (DL-59 pattern).
- **SR-AT-07 (Known for GIIS SG, HB p.23, p.166):** Medical leave needs a medical certificate, due when the student returns. Until it's uploaded the request shows CERTIFICATE_DUE and the days have `valid_reason` false.
- **SR-AT-08 (Confirmed, DL-101):** Approved leave sets the days' marks to ON_LEAVE (source LEAVE). Leave approved after the days have passed corrects ABSENT marks automatically, without coordinator approval.
- **SR-AT-09 (Known for GIIS SG, HB p.23, p.174):** A late sign-in sets the day mark to LATE with the arrival time. An out pass needs the class teacher's or level coordinator's approval before the student leaves, and sets LEFT_EARLY.
- **SR-AT-10 (Known for GIIS SG, HB p.166; Confirmed mechanism DL-104; by student group DL-108):** Each student is measured against every AT08 rule that applies to them. GIIS SG: Student Pass 90% per month; others from Class 6 85% per semester; others KG–Class 5 75% per term, which the Principal may relax. Class teachers see the threshold only (DL-30).
- **SR-AT-11 (Known for GIIS SG, HB p.166–167; Confirmed mechanism DL-105):** For a Student Pass holder, more than 7 consecutive school days absent, or a month under 90% without a valid reason, opens an ICA review Case for the Tier 3 owner. The system never notifies ICA itself; the owner records the filing.
- **SR-AT-12 (Confirmed, DL-107):** A student ABSENT at the register cut-off with no approved leave triggers a same-day parent notice (UNEXPLAINED_ABSENCE).
- **SR-AT-13 (Confirmed, DL-106; Confirmed consumers DL-92, DL-95):** Assessment reads AT09 for report cards and promotion; never reasons or certificates.
- **SR-AT-14 (Known, HB p.234):** No marks after the enrolment's last attendance date (ST02 `end_date`). A mid-year joiner is measured from their start date (OQ-115).
- **SR-AT-15 (Known for GIIS SG, HB p.174):** Late arrivals step up per segment (KG and Primary: first three excused, the fourth a note to parents from the Level Coordinator or Vice Principal). Exempt lates (school bus, doctor's note, prior permission) don't count. The steps are a campus setting (OQ-111).
- **SR-AT-16 (Confirmed, DL-83):** Fee status never blocks marking, leave, or anything in this module.

## Events

CalendarPublished, RegisterSubmitted, RegisterLocked, AttendanceMarked, MarkCorrected, LeaveRequested, LeaveDecided, CertificateReceived, GateEventRecorded, AttendanceSummaryUpdated (Assessment, parent app), AttendanceAlertRaised (Communication, Cases), IcaReviewOpened (Case, Tier 3; payload IDs only). Events carry IDs only (architecture.md).

## Capabilities (new, DL-53 naming)

`attendance.calendar.manage` · `attendance.register.take` (scoped to own classes; DL-73) · `attendance.register.view` · `attendance.mark.edit_after_lock` (exists in S4) · `attendance.correction.approve` · `attendance.leave.request` (parent path) · `attendance.leave.approve` (class teacher up to the campus limit; Principal above) · `attendance.certificate.view` (Tier 3) · `attendance.gate.record` · `attendance.out_pass.approve` · `attendance.rule.manage` · `attendance.summary.view` · `attendance.ica.manage` (Tier 3).

## Decisions (all Confirmed by Henry, 21 Nov 2026)

| ID | Decision | Why |
|---|---|---|
| DL-97 (**Confirmed**, Henry 21 Nov 2026) | Attendance is designed as its own module (capability prefix `attendance`); Conduct gets a separate dictionary. The `attendance_conduct` module team key stays until then. Feature areas of the Student Records domain (DL-43) | Attendance is the Phase 1 daily loop (Arch § Roadmap); Conduct has different roles, tiers and rules |
| DL-98 (**Confirmed**, Henry 21 Nov 2026) | School days = weekdays in the term dates of a campus × offering × year (DL-96) minus a non-school-day list that Attendance keeps until a calendar engine exists. The % denominator is school days | One denominator for every %, already per offering |
| DL-99 (**Confirmed**, Henry 21 Nov 2026) | One day mark per student per school day with six codes (PRESENT, LATE, LEFT_EARLY, ABSENT, ON_LEAVE, SCHOOL_ACTIVITY). Period registers are a campus setting; when on, they derive the day mark | Arch step 3 allows both; thresholds are measured in days (HB p.166) |
| DL-100 (**Confirmed**, Henry 21 Nov 2026) | Registers lock at end of day; later changes are corrections with a reason, approved by the coordinator, except changes from approved leave or the gate log | Arch § Security audits attendance edits; S4 already names `attendance.mark.edit_after_lock` |
| DL-101 (**Confirmed**, Henry 21 Nov 2026) | Parents request leave in the app; 1–2 school days → class teacher, more than 2 → Principal (with a Case); medical leave needs a certificate on return; approved leave sets ON_LEAVE | HB p.23, p.166; Arch step 2 |
| DL-102 (**Confirmed**, Henry 21 Nov 2026) | Reception/security keep the gate log; late sign-in sets LATE, an approved out pass sets LEFT_EARLY; devices can feed the same events later | HB p.23, p.174; Arch § Integrations |
| DL-103 (**Confirmed**, Henry 21 Nov 2026) | Attendance % = days attended ÷ school days from the enrolment start; LATE, LEFT_EARLY and SCHOOL_ACTIVITY count as attended; ON_LEAVE doesn't (answers OQ-108). Calculated, never typed | Matches how thresholds are stated (HB p.166) |
| DL-104 (**Confirmed**, Henry 21 Nov 2026) | Thresholds are campus rules per student group and measurement period, no group default; the system projects each student's % and warns teacher, coordinator and parent when at risk | Arch § Principles 3 and step 4; DL-59 pattern |
| DL-105 (**Confirmed**, Henry 21 Nov 2026) | The ICA trigger opens a Tier 3 Case for the Admissions/registrar owner; a person files the notice and records it; teachers never see the link | HB p.166–167; DL-30; Arch § Integrations "Case exports" |
| DL-106 (**Confirmed**, Henry 21 Nov 2026) | Other modules read only the attendance summary (days, %), never reasons or certificates; fee status never blocks attendance | DL-95, DL-66 pattern; DL-83 |
| DL-107 (**Confirmed**, Henry 21 Nov 2026) | Parents see their own child's marks, times, leave and % the same day, and get a same-day notice for an unexplained absence | MyGIIS parity; Arch § Users and roles (parent) |
| DL-108 (**Confirmed**, Henry 21 Nov 2026; answers OQ-108) | The attendance minimum applies by student group (GIIS SG: Student Pass 90% per month; others Class 6+ 85% per semester; KG–5 75% per term), as on HB p.166. The p.23 line "a minimum of 90% attendance is mandatory for all students" is read as the Student Pass figure, not a separate rule | Henry: "Attendance minimum apply by student group" |

## Open questions (new)

| ID | Question | Why it matters for design |
|---|---|---|
| OQ-108 | Does approved leave (especially medical leave with a certificate) count against a student's attendance %, or only against the ICA rule's "without a valid reason"? The handbook also says "a minimum of 90% attendance is mandatory for all students" (p.23) but gives 90/85/75% by group on p.166. | The % shown to parents; promotion and ICA. **Answered → DL-103** (approved leave is not attended) and **DL-108** (minimum applies by student group, HB p.166), 21 Nov 2026 |
| OQ-109 | Who approves leave when the class teacher or Principal is away (co-teacher, Vice Principal), and do other brands route by days the same way? | Approval routing and delegation |
| OQ-110 | What are the register cut-off and lock times per campus, who may correct a locked mark (coordinator or Principal), and who takes the register when the class teacher is absent? With period registers, which period decides LATE and LEFT_EARLY? | Overdue registers, correction approvals, cover |
| OQ-111 | How are partial days (half-day leave, medical appointments) recorded, and what are the late-arrival steps for Middle and Senior students (HB p.174–175)? | Mark codes and repeated-late alerts |
| OQ-112 | At GIIS SG, who files the ICA notice, how (portal, email), within what time, and what counts as a "valid reason"? | ICA review Case steps and evidence |
| OQ-113 | What thresholds and reporting duties apply to other brands and countries (GIIS India/Noida, OWIS, regulators such as MOE or CBSE)? | Rule defaults beyond GIIS SG; non-GIIS samples are synthetic |
| OQ-114 | Do school activities (field trips, competitions, external events, HB p.167) count as present, and who records them? | SCHOOL_ACTIVITY code and who sets it |
| OQ-115 | For mid-year joiners and transfers in, is the % measured from their start date, and does a stage-campus move carry the % across? | Summary denominator |
| OQ-116 | Is there an authorised-pickup list per student for out passes, and where does it live (guardians or a separate list)? | Out pass "collected by" check |
| OQ-117 | From which grade may students see their own attendance in the student app? | Student app attendance tab (extends DL-68) |
| OQ-118 | How long are attendance records and medical certificates kept, and how much MyGIIS attendance history moves at cutover? | Retention policy; history on day one |
| OQ-119 | Are there Saturday school days, exam study-leave days or grade-only days off that change school days for some grades? | School-day calendar by grade |
| OQ-120 | Who maintains holidays and closures (campus office or brand-country), and who adds unplanned closures? | Calendar edit rights |
| OQ-121 | Do gate devices exist at GIIS SG campuses today, and what do they send (card tap, face, time only)? | Phase 2 device feed vs desk entry |

## Readiness

- **Can be designed now:** class teacher's daily register (phone first), the coordinator's register status board, parent leave request and the approvals inbox, reception gate log (late sign-in, out pass), student attendance view with % against threshold, at-risk list, ICA review Case shell, school-day calendar with GIIS SG 2026-27 holidays.
- **Keep generic, label Pending:** cut-off and lock times (OQ-110), partial days (OQ-111), ICA filing steps (OQ-112), non-GIIS thresholds (OQ-113), student app access (OQ-117).

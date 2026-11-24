# Global Campus OS — Cases & Communications Data Dictionary v0.1 (draft)

**Status:** Proposed draft, 22 Nov 2026. DL-119 (scope) Confirmed by Henry, 22 Nov 2026; DL-120 Confirmed by Henry, 24 Nov 2026 (own lists per module, shared engine); DL-121…DL-130 Assumed, awaiting Henry; OQ-130…OQ-146 Open · **Owner:** Henry
**Cites:** [Architecture v1](../00-sources/Global_Campus_OS_Product_Architecture_v1.md) § Principles 3 ("refund tables and complaint deadlines… become versioned rules with alerts"), § How GIIS operates today ("Complaints. Acknowledge in 2 working days, close in 7, escalate at 14 and 21, then the CPE dispute scheme"; "Appeals"; "Refunds"), § Users and roles (Principal "complaint escalation"; Class teacher "parent messages, PTM slots"; Admissions and customer relations "Student Pass cases"; Discipline committee "incident cases"), § Domains and modules (module 9 "Communication and Cases: messages, PTMs, announcements; feedback and complaint cases"; shared services "Cases and SLAs", "Notifications"), § Key workflows (Feedback and complaints, steps 1–3; Fees, withdrawal and refund step 2; Absence and leave step 5), § Data model (Engagement: "Message, Announcement, Case (complaint, appeal, withdrawal), Survey, Consent"; "Case is one shared object with SLA timers"; "One Case object for complaints, appeals, withdrawals, leave over 2 days and Student Pass issues, each with type-specific fields and SLA timers"), § Tech stack (durable workflow engine for "SLA timers for complaints, refunds, appeals and ICA cases"; "Push, email, SMS and WhatsApp through one notification service"; "Parent preferences per channel and language"), § Integrations (Messaging providers), § Security (Tier 3 child protection cases restricted to designated officers) · [OBJ-10 Case](./objects/OBJ-10-case.md) · [modules-and-services](./modules-and-services.md) `communication_cases`, "Cases and SLAs" · [Student Directory Spec](./student-directory-data-dictionary.md) ST04, ST05, ST07 (contact channels), `preferred_language`, `approval_case_id` (ST02, ST09) · [Admissions](./admissions-data-dictionary.md) AD10 · [Classes](./classes-data-dictionary.md) CL06 `approval_case_id`, DL-62 · [Fees](./fees-billing-data-dictionary.md) FB14, FB16, DL-84, DL-86 · [Assessment](./assessment-data-dictionary.md) AS11, AS12, AS13 `appeal_case_id`, DL-91, DL-94 · [Attendance](./attendance-data-dictionary.md) AT06, AT10, DL-101, DL-105, DL-107 · [Organisation](./organisation-data-dictionary.md) OR05 `change_case_id`, DL-50 · [rbac-model](../04-access/rbac-model.md) A15, A17, DL-08, DL-19, DL-69 · [rules-to-ui](../06-experience/admin-platform/rules-to-ui.md) § Status vocabulary (Case) · GIIS Student Handbook 2025-26 (HB) p.8–9 (contacts and escalation by level), p.24 (PTM, feedback, enrolment clarifications 24/48 hours), p.34 (MyGIIS Messaging "main communication channel"), p.233–237 (withdrawal and transfer timelines), p.245 (refund document and escalation timelines), p.249–250 (Feedback and Complaint Management System) · DL-08, DL-11, DL-19, DL-22, DL-27, DL-30, DL-41, DL-43, DL-47, DL-53, DL-59, DL-84, DL-96, DL-110, DL-118
**Tóm tắt (VI):** Từ điển dữ liệu cho module Hồ sơ xử lý (Case) và Liên lạc. Một "Case" dùng chung cho mọi việc cần người xử lý và có hạn chót: khiếu nại, phúc khảo, hoàn phí, xin nghỉ trên 2 ngày, rút hồ sơ, chuyển trường, báo cáo ICA, yêu cầu cấp quyền. Case giữ trạng thái, người xử lý, đồng hồ hạn chót tính theo ngày làm việc, các bậc leo thang và nhật ký; chi tiết riêng (số tiền hoàn, ngày nghỉ) vẫn nằm ở module gốc. Loại case do GSG định nghĩa, thời hạn và người duyệt do từng brand-country đặt. Phần Liên lạc gồm tin nhắn giáo viên–phụ huynh về một học sinh, thông báo cho trường/khối/lớp, và một dịch vụ gửi thông báo chung với kênh và ngôn ngữ phụ huynh chọn. Mỗi trường có trạng thái Known / Assumed / Open.

> Drafted by Claude from product truth on 22 Nov 2026. The architecture's module 9 is "Communication and Cases" and the shared services include "Cases and SLAs" and "Notifications". This file covers the Case engine, complaints and feedback, messages, announcements and notifications. PTM booking, surveys and consent are later (DL-119).

## How to read this file

Same layout and rules as the [Student Directory Spec](./student-directory-data-dictionary.md#how-to-read-this-file): **Known** (cited), **Assumed** (DL), **Open** (OQ); tiers T1/T2/T3 (DL-11); `Y`/`N`/`C` required. A case's tier is the tier of its type (an ICA review is Tier 3, a canteen complaint Tier 2). Every row also carries `tenant_path`, `data_region` (UI term **Data location**, DL-47) and the shared common fields. Facts from the GIIS Handbook are **GIIS Singapore examples**, not group rules (DL-22). All enums are unapproved candidates (DL-27 rule). All example values are synthetic.

## Scope

Approved by Henry, 22 Nov 2026 (decision card "Cases + complaints + messaging"): **anything a person must act on by a deadline, and every message the school sends or receives about a student.**

| In this dictionary | Not in this dictionary (owner) |
|---|---|
| One Case engine (OBJ-10): case types, brand-country case policies, status, assignee, deadline clock in working days, escalation steps, approvals, timeline, documents | The detail each module keeps on its own record: refund amount (FB14), leave dates (AT06), appeal marks (AS11), Student Pass data (ST09) |
| Complaints, compliments and suggestions from parents, students, staff and the public, with the GIIS SG 2 / 7 / 14 / 21 working-day ladder and the external CPE referral | Child protection, counselling and discipline cases (Wellbeing and Conduct, later dictionaries; they will use this engine as RESTRICTED types) |
| Staff ↔ guardian conversations about a student (replaces MyGIIS Messaging, HB p.34) | Staff grievances and HR cases (Staff and HR) |
| Announcements to a campus, programme, grade, class or house, with optional acknowledgement | PTM booking, surveys and consent forms (later; consent is OBJ-17, Governance) |
| One notification service for every module: templates per brand-country and language, channels, guardian preferences, delivery log | SMS, email, WhatsApp and push provider setup (Integrations) |
| Working-day calendar used by every case deadline | School days for attendance (AT01, AT02) and the campus calendar engine (later) |

## Modeling boundaries

- **Cases & Communications is one module with two parts (DL-119).** Module code `communication_cases` (modules-and-services), Engagement domain (DL-43). Capability prefixes `cases` (case engine, complaints) and `comms` (messages, announcements, notifications) (DL-53). The existing seed capabilities `gov.case.view_all` and `gov.access_review.decide` stay as they are.
- **One Case engine; the module keeps the detail (DL-120).** A Case holds what every tracked request has: type, who it is about, who raised it and how, status, who is handling it, deadlines, escalation, approvals, comments, documents and audit (Arch § Data model: "one shared object with SLA timers"). The owning module keeps the type-specific record and points to the case with `case_id` (AT06, AT10, FB14, FB16, AD10, AS13, CL06, ST02, ST09, OR05). The case points back with `source_module` + `source_record_id`. When the case decision is made, the module applies it (APPLIED). Eleven fields across six dictionaries already point here. **Each module shows its own case list on its own screens (Confirmed, Henry 24 Nov 2026):** Fees shows refunds and long-leave pauses, Attendance shows leave over 2 days and ICA reviews, Admissions shows withdrawals, transfers and Student Pass cases, Assessment shows appeals, the Access Registry shows access requests, and Cases & Communications shows complaints and feedback. All of them run on the one engine underneath, so deadlines, escalation, approvals and audit work the same everywhere. Whether a person also gets one combined list across modules (for example a Principal's approvals) is not decided.
- **GSG defines case types; each brand-country sets deadlines, routing and approvers (DL-121).** The case type catalogue is a GSG list (complaint, appeal, refund, withdrawal, transfer, leave over 2 days, ICA review, access request, and so on). A brand-country case policy, versioned and effective-dated, sets the deadline steps, who receives the case, who approves and who it escalates to. Each case records the policy version it started under, so a later change never moves a running deadline (the DL-84 refund rule pattern; DL-110 template-and-adapt pattern).
- **Deadlines count working days, and the clock pauses while waiting for the family (DL-122).** "Working day" in the handbook (complaints, withdrawals, refunds, appeals) means the campus office's working days: Monday to Friday minus the public holidays of the campus's country (GIIS SG: Deepavali 9–10 Nov 2026, Christmas, New Year). School vacations are working days for the office (Assumed; OQ-130). The day a case arrives is day 0. While a case is WAITING for the complainant or parent to send something, the clock stops and later deadlines move by the paused working days (Assumed; OQ-131). Every deadline shows its due date, never only "n days".
- **Complaints follow the brand-country ladder; GIIS SG uses 2 / 7 / 14 / 21 working days (DL-123).** For GIIS SG (HB p.249–250): the person who receives the complaint acknowledges it within 2 working days; the recipient or the process owner replies in writing to close it within 7; if it is still open, the process owner works with their supervisor to resolve it within 14; if still open, it goes to the Dispute Resolution Committee, chaired by the Country Director with the Academic and Examination Board, within 21; if no solution is agreed, the Country Director proposes the CPE Dispute Resolution Scheme (external). Missing a deadline moves the case to the next step automatically and tells the next owner; the earlier owner keeps access. The CPE referral is recorded, never sent by the system.
- **Compliments and suggestions are feedback, not cases (DL-123).** A compliment or suggestion is acknowledged within 2 working days (HB p.249 B) and compiled for internal review (HB p.249 C); it has no resolution deadline. A complaint always opens a Case. Staff can reclassify an item in either direction with a reason.
- **One status list for every case; "Overdue" is a flag, not a status (DL-124).** Statuses: NEW, IN_PROGRESS, WAITING (for the family or a third party), DECIDED (with an outcome: APPROVED, REJECTED, RESOLVED, NO_ACTION), APPLIED (the owning module has acted), CLOSED, WITHDRAWN. Overdue and the escalation level are shown beside the status, because a case can be overdue while still in progress. This replaces the OBJ-10 list in rules-to-ui (`IN_REVIEW`, `ESCALATED`, `APPROVED` become `IN_PROGRESS`, the overdue flag and the DECIDED outcome).
- **Restricted cases show as a locked row; only named people open them (DL-125, Confirmed by Henry 24 Nov 2026; builds on DL-19).** A case type can be RESTRICTED by default (ICA review, Student Pass, later child protection and SEN). Only its assignee, named participants (A15) and holders of a Tier 3 type grant for that type (A17, approved by the Campus Leader, DL-69) can open it and see its content. Other staff who can see the same queue or module case list see a locked row: "Restricted case", the case type, the received date and the next due date, with no student name, title or content, and they cannot open it. The locked row never appears on teacher, class or student screens, so teachers still see only the effect ("Attendance below threshold"), never the case (DL-30). Restricted cases never appear in search or in notifications to people who are not named; dashboards show totals.
- **Families see their own cases, with replies kept apart from staff notes (DL-126).** Every timeline entry is either an internal note (staff only) or a reply to the family. Parents see cases they raised or that are about their own child, when the type allows it (complaint, appeal, leave, withdrawal, transfer, refund), with status, the next due date and replies. They never see internal notes, restricted cases or other families' cases. In v1 (staff web app, DL-118) staff log cases received by email, phone, helpdesk or in person and send replies by email; the Parent app comes later and reads the same records.
- **Messages are conversations about one student (DL-127).** A conversation is between staff and the guardians of one student (or one guardian), started by either side. Staff can start a conversation only with guardians of students in their data scope (class teacher: own class, DL-73); guardians can write to the staff who teach their child and to campus offices listed in the directory (HB p.8–9 contacts). No parent-to-parent or student-to-student messaging. A message can be turned into a complaint Case or a leave request; the link is kept both ways. Messages are Tier 2; staff must not put Tier 3 content (health, immigration, SEN) in messages.
- **Announcements go to an audience, never a typed list (DL-128).** An announcement targets an audience rule: a campus, programme offering, grade, class, house or group of staff, for guardians, students, staff or a mix. The class teacher posts to their own class; a grade or campus announcement needs the coordinator's or Principal's approval (campus setting). Announcements can be scheduled, can ask for an acknowledgement ("I have read this"), and keep who received and read them. An acknowledgement is not consent (OBJ-17).
- **One notification service for every module (DL-129).** Modules publish events (LeaveDecided, AttendanceAlertRaised, InvoiceIssued, ReportCardReleased, CaseUpdated); this module decides who receives them, in which channel and language, from a template. Each template belongs to a brand-country, has a version per language and channel, and carries IDs and approved wording only, never Tier 3 content. Notifications have three categories: SAFETY (cannot be turned off, all channels), REQUIRED (cannot be turned off; the guardian chooses the channel: absence notices, case replies, fees due) and OPTIONAL (newsletters, event reminders; can be turned off). Guardians choose channel and language per category (Arch § Tech stack "parent preferences per channel and language"); every send is logged with its delivery result.
- **Data location (DL-130).** Cases, feedback, messages, announcements and the delivery log live in the campus's data location (DL-47). The operating entity is copied onto each case at creation (DL-41). Group and brand-country dashboards get counts, deadline performance and categories, never content. A case that involves two campuses (transfer) lives in the originating campus and grants named participants at the other campus (A15).

## Entities

| ID | Entity | Part | Status |
|---|---|---|---|
| CC01 | Case Type | Cases (GSG catalogue) | Known need (Arch § Data model "each with type-specific fields and SLA timers"; DL-08 types); catalogue Assumed (DL-121) |
| CC02 | Case Policy | Cases (brand-country, versioned) | Known values for GIIS SG (HB p.233–237, p.245, p.249–250); model Assumed (DL-121, DL-84 pattern) |
| CC03 | Case | Cases (OBJ-10) | Known entity (Arch § Data model; OBJ-10); fields Assumed (DL-120) |
| CC04 | Case Deadline | Cases (SLA clock) | Known need (Arch § Tech stack "reliable SLA timers"; HB p.249–250); model Assumed (DL-122, DL-123) |
| CC05 | Case Participant | Cases | Known need (HB p.249 recipient and process owner; p.250 committee; DL-19 A15); model Assumed |
| CC06 | Case Decision | Cases | Known need (DL-08 "line-level decisions"; DL-84 two approvers; DL-62 both principals); model Assumed |
| CC07 | Case Activity | Cases (timeline) | Known need (Arch "audit trail"; HB p.249 "replies… in writing"); model Assumed (DL-126) |
| CC08 | Feedback Item | Cases (complaints and feedback) | Known (HB p.249 A: complaint, compliment, others; channels "letter, email, MyGIIS"); model Assumed (DL-123) |
| CC09 | Working Day Calendar | Cases (until the calendar engine exists) | Known need (HB "working days" throughout); definition Open (OQ-130) |
| CC10 | Conversation | Communications | Known need (HB p.34 MyGIIS Messaging; Arch § Users and roles "parent messages"); model Assumed (DL-127) |
| CC11 | Message | Communications | Known entity (Arch § Data model "Message"); model Assumed (DL-127) |
| CC12 | Announcement | Communications (OBJ-26) | Known entity (Arch § Data model "Announcement"); model Assumed (DL-128) |
| CC13 | Announcement Receipt | Communications | Known need (Arch § Roadmap "notifications"; read tracking); model Assumed (DL-128) |
| CC14 | Notification Template | Communications (Notifications service) | Known need (Arch § Tech stack "one notification service"); model Assumed (DL-129) |
| CC15 | Notification | Communications (delivery log) | Known need (Arch § Tech stack; § Security audit); model Assumed (DL-129) |
| CC16 | Notification Preference | Communications | Known need (Arch § Tech stack "parent preferences per channel and language"; ST04 `preferred_language`, ST07 channels); model Assumed (DL-129) |

ST02, ST04, ST05, ST07, ST08 (documents), ST09, E01, A15, A17, OR03, OR04, OR07 and each module's case-linked record are referenced, not redefined. Case documents are stored as ST08 documents (student cases) or case attachments (other cases), never inside messages.

## Detailed attribute dictionary

### CC01 — Case Type

**Purpose:** One kind of case in the GSG catalogue: what it is about, which module owns its detail, its tier and whether families can see it.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| case_type_code | VARCHAR(40) | PK | Y | T1 | Assumed (DL-121) | e.g. `COMPLAINT`, `RESULT_APPEAL`, `PREDICTED_GRADE_APPEAL`, `REFUND`, `WITHDRAWAL`, `CAMPUS_TRANSFER`, `LONG_LEAVE`, `LEAVE_OVER_LIMIT`, `ICA_REVIEW`, `STUDENT_PASS`, `RETENTION_REQUEST`, `YEAR_END_CAMPUS_CHANGE`, `ACCESS_REQUEST` (DL-08 types), `CAMPUS_OPERATOR_HANDOVER` (DL-50). |
| family | ENUM | — | Y | T1 | Assumed (DL-27) | FEEDBACK, ACADEMIC, ENROLMENT, FINANCE, ATTENDANCE, IMMIGRATION, ACCESS, GOVERNANCE. Used for filters and dashboards. |
| owning_module | VARCHAR(40) | — | Y | T1 | Known (each module's dictionary) | `communication_cases` for complaints; `fees` for refunds; `attendance` for leave and ICA; `admissions` for withdrawal, transfer, Student Pass; `assessment` for appeals; `access` for DL-08 types. |
| source_entity | VARCHAR(10) | — | C | T1 | Known (AT06, AT10, FB14, FB16, AD10, AS13, CL06, OR05) | The entity whose record holds the detail. Empty for complaints. |
| tier | ENUM | — | Y | T1 | Known (DL-11) | T2 or T3. ICA_REVIEW, STUDENT_PASS are T3. |
| default_visibility | ENUM | — | Y | T1 | Confirmed mechanism (DL-19); per type Confirmed (DL-125) | NORMAL, RESTRICTED. T3 types are always RESTRICTED. |
| family_visible | BOOLEAN | — | Y | T1 | Assumed (DL-126) | Whether guardians see the case when the Parent app exists. False for ICA_REVIEW and access types. |
| raised_by_allowed | ENUM[] | — | Y | T1 | Known (HB p.249: "public, staff, students or parents") | GUARDIAN, STUDENT, STAFF, PUBLIC, SYSTEM (rule-raised, e.g. ICA_REVIEW). |
| status | ENUM | — | Y | T1 | Assumed | ACTIVE, RETIRED. Owned by the GSG platform team (`cases.type.manage`). |

### CC02 — Case Policy

**Purpose:** How one brand-country handles one case type: deadline steps, who receives it, who approves, who it escalates to. Versioned.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| policy_id | UUID | PK | Y | T1 | Assumed (DL-121) | |
| brand_country_id | UUID | FK:OR03 | Y | T1 | Known (DL-84 pattern) | |
| case_type_code | VARCHAR(40) | FK:CC01 | Y | T1 | Assumed | |
| version, valid_from, valid_to | INT, DATE | UK | Y | T1 | Assumed (DL-84 pattern) | A running case keeps the version it started with. |
| steps | JSON | — | Y | T1 | Known values for GIIS SG (HB p.249–250); format Assumed (DL-123) | Ordered steps: `step_code`, `days` (working days from receipt), `owner_rule`, `on_breach`. GIIS SG complaint: ACKNOWLEDGE 2 → RESOLVE 7 → SUPERVISOR 14 → DISPUTE_COMMITTEE 21 → EXTERNAL_REFERRAL (no deadline). |
| intake_owner_rule | JSON | — | Y | T1 | Known for GIIS SG (HB p.8–9 escalation by level; p.249 "recipient… may forward… to that process owner") | Who gets a new case: by topic and campus level (class teacher, coordinator, Vice Principal Primary, Principal Secondary, Finance, Admissions). |
| approvers | JSON | — | C | T1 | Known for some (DL-84 GIIS SG refund: Country Director + Finance Controller; DL-62 both principals; DL-94 committee headed by the Principal; HB p.239 deferment: Associate Director Academic Quality Assurance) | Roles that must each record a decision (CC06). |
| committee | JSON | — | C | T1 | Known for GIIS SG complaints (HB p.250 G: Dispute Resolution Committee chaired by the Country Director with the Academic and Examination Board) | |
| family_escalation | JSON | — | N | T1 | Known for GIIS SG (HB p.237: withdrawal or transfer not completed in time → Finance Controller, then after 4 working days the Director Business and Strategy; HB p.245: refund not paid → Finance Controller, then after 4 working days the Country Director) | Who the family can escalate to when a deadline passes. |
| pause_when_waiting | BOOLEAN | — | Y | T1 | Assumed (DL-122); Open (OQ-131) | |
| status | ENUM | — | Y | T1 | Assumed | DRAFT, PUBLISHED, RETIRED. Brand-country publishes (`cases.policy.manage`). |

GIIS SG values recorded in the sample (all from the handbook): complaint 2/7/14/21 (p.249–250); withdrawal acknowledge 2, documents ready 7 after the effective date (p.233–234); campus transfer acknowledge 2, decision within 3 weeks, profile updated within 3 working days (p.235–236); refund missing-documents notice 3, family has 7 to send them (p.245); result appeal response 6 working days for CBSE 1–8 (p.88, AS11); enrolment clarifications answered within 24 hours (Finance Controller, Customer Relations) or 48 hours (Admissions Manager) (p.24; hours, not working days: OQ-132).

### CC03 — Case

**Purpose:** One tracked request with an owner and a deadline (OBJ-10).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| case_id | UUID | PK | Y | T1 | Known (OBJ-10) | |
| case_number | VARCHAR(24) | UK | Y | T1 | Assumed | `CASE-{country}-{year}-{seq}`, e.g. CASE-SG-2026-1291 (samples). Shown to families. |
| case_type_code | VARCHAR(40) | FK:CC01 | Y | T1 | Assumed (DL-121) | |
| policy_id | UUID | FK:CC02 | Y | T1 | Assumed (DL-121) | The version this case runs under. |
| campus_id | UUID | FK:OR04 | Y | T1 | Known (home: cell, OBJ-10) | Originating campus for two-campus cases (DL-130). |
| operating_entity_id | UUID | FK:OR07 | Y | T1 | Confirmed (DL-41) | Copied at creation, never re-pointed. |
| subject_type, subject_id | ENUM, UUID | — | C | T1 | Assumed | STUDENT (ST01), ENROLMENT (ST02), GUARDIAN (ST04), EMPLOYEE (E01), ACCOUNT (A01), NONE (general complaint about a service). |
| source_module, source_record_id | VARCHAR, UUID | — | C | T1 | Assumed (DL-120) | Back-link to AT06, FB14, AD10… Empty for complaints. |
| title | VARCHAR(160) | — | Y | per type | Assumed | Short, written by staff. Tier follows the type; restricted titles never appear in notifications. |
| raised_by_type, raised_by_id | ENUM, UUID | — | Y | T1 | Known (HB p.249 who may complain) | GUARDIAN, STUDENT, STAFF, PUBLIC (name and contact held on CC08), SYSTEM. |
| received_via | ENUM | — | Y | T1 | Known (HB p.24, p.249: MyGIIS, helpdesk, email, letter, telephone, meetings) | APP, EMAIL, PHONE, LETTER, IN_PERSON, HELPDESK, MESSAGE (converted from CC11), SYSTEM. |
| received_at | TIMESTAMP | — | Y | T1 | Known (HB p.249 "within 7 working days of receipt") | Day 0 of every deadline. Staff logging a late email enter the real receipt time. |
| status | ENUM | — | Y | T1 | Assumed (DL-124) | NEW, IN_PROGRESS, WAITING, DECIDED, APPLIED, CLOSED, WITHDRAWN. |
| outcome | ENUM | — | C | T1 | Assumed (DL-124) | APPROVED, REJECTED, RESOLVED, NO_ACTION. Set with DECIDED. |
| outcome_summary | TEXT | — | C | per type | Assumed | The written reply or reasons (HB p.249 "in writing"). |
| assignee_employee_id | UUID | FK:E01 | C | T1 | Known (HB p.249 recipient, process owner) | Current owner. Empty only while NEW in a team queue. |
| queue | VARCHAR(40) | — | Y | T1 | Assumed | Team queue from the policy's intake rule (e.g. "SG-PGL Admissions", "SG-PGL Finance"). |
| current_step | VARCHAR(40) | — | Y | T1 | Assumed (DL-123) | From CC02 steps. |
| next_due_at | TIMESTAMP | — | C | T1 | Assumed (DL-122) | Earliest open CC04 deadline. Drives sort order and the overdue flag. |
| is_overdue | BOOLEAN | — | Y | T1 | Assumed (DL-124) | Derived. Shown as "Overdue" (error chip) beside the status. |
| escalation_level | INT | — | Y | T1 | Assumed (DL-123) | 0 = with recipient/process owner, 1 = supervisor, 2 = committee, 3 = external referral. |
| visibility | ENUM | — | Y | T1 | Confirmed (DL-19) | NORMAL, RESTRICTED. Defaults from CC01; can be raised to RESTRICTED, never lowered for T3 types. |
| family_visible | BOOLEAN | — | Y | T1 | Assumed (DL-126) | From CC01; staff may turn off for one case with a reason. |
| external_reference | VARCHAR(64) | — | N | T2 | Known (CPE referral HB p.250 I; ICA filing reference AT10; board appeal reference AS11) | |
| closed_at, closed_by | TIMESTAMP, UUID | — | C | T1 | Assumed | |

### CC04 — Case Deadline

**Purpose:** One deadline step on one case, with when it was met or missed.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| deadline_id | UUID | PK | Y | T1 | Assumed (DL-122) | |
| case_id | UUID | FK:CC03 | Y | T1 | Assumed | |
| step_code | VARCHAR(40) | — | Y | T1 | Known for GIIS SG complaints (HB p.249–250) | ACKNOWLEDGE, RESOLVE, SUPERVISOR, DISPUTE_COMMITTEE; other types define their own (DECIDE, DOCUMENTS_DUE, APPLY). |
| working_days | INT | — | C | T1 | Known for GIIS SG (CC02) | From the policy version. |
| due_at | TIMESTAMP | — | Y | T1 | Assumed (DL-122) | Calculated from `received_at` on CC09 working days; end of the office day (time: OQ-130). |
| paused_working_days | INT | — | Y | T1 | Assumed (DL-122) | Added while the case is WAITING. |
| met_at | TIMESTAMP | — | C | T1 | Assumed | When the step's action happened (acknowledgement sent, reply sent, decision recorded). |
| breached_at | TIMESTAMP | — | C | T1 | Assumed (DL-123) | Set by the timer; moves the case to the next step and notifies the next owner. |
| status | ENUM | — | Y | T1 | Assumed (DL-27) | PENDING, MET, BREACHED, SKIPPED (case closed earlier), PAUSED. |

### CC05 — Case Participant

**Purpose:** Who else is on the case: approvers, committee members, watchers and staff at a second campus.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| participant_id | UUID | PK | Y | T1 | Assumed | |
| case_id | UUID | FK:CC03 | Y | T1 | Assumed | |
| account_id | UUID | FK:A01 | Y | T1 | Assumed | |
| role_on_case | ENUM | — | Y | T1 | Known need (HB p.249 recipient, process owner; p.250 supervisor, committee) | RECIPIENT, PROCESS_OWNER, SUPERVISOR, APPROVER, COMMITTEE_MEMBER, WATCHER, INFORMED (e.g. class teacher and coordinator on leave over 2 days, DL-101). |
| acl_id | UUID | FK:A15 | C | T1 | Confirmed mechanism (DL-19) | Created for RESTRICTED cases and for staff outside the case's campus scope. |
| added_by, added_at, removed_at | UUID, TIMESTAMP | — | Y | T1 | Assumed | Access ends when removed; history kept. |

### CC06 — Case Decision

**Purpose:** One approver's decision on a case (line-level decisions, DL-08). A case with two approvers has two rows.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| decision_id | UUID | PK | Y | T1 | Assumed | |
| case_id | UUID | FK:CC03 | Y | T1 | Assumed | |
| approver_role | VARCHAR(60) | — | Y | T1 | Known per policy (CC02 `approvers`) | e.g. "Country Director", "Finance Controller", "Principal, originating campus". |
| decided_by | UUID | FK:A01 | C | T1 | Assumed | Empty until decided. A delegate (A16) is recorded as acting for the approver. |
| line_ref | VARCHAR(80) | — | N | T1 | Known (DL-08 line-level decisions on access requests) | The line decided (one requested capability; one refund basis). |
| decision | ENUM | — | C | T1 | Assumed (DL-27) | APPROVE, REJECT, RETURN_FOR_INFO. |
| reason | TEXT | — | C | per type | Assumed | Required for REJECT and RETURN_FOR_INFO. |
| decided_at | TIMESTAMP | — | C | T1 | Assumed | |

### CC07 — Case Activity

**Purpose:** The case timeline: replies to the family, internal notes, status changes, documents and system events. Append-only.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| activity_id | UUID | PK | Y | T1 | Assumed | |
| case_id | UUID | FK:CC03 | Y | T1 | Assumed | |
| kind | ENUM | — | Y | T1 | Assumed (DL-126) | REPLY_TO_FAMILY, INTERNAL_NOTE, STATUS_CHANGE, ASSIGNED, DOCUMENT_ADDED, DECISION, DEADLINE_BREACHED, ESCALATED, EXTERNAL_REFERRAL, LINKED. |
| visible_to_family | BOOLEAN | — | Y | T1 | Assumed (DL-126) | True only for REPLY_TO_FAMILY and selected status changes; INTERNAL_NOTE is always false. |
| body | TEXT | — | C | per type | Assumed | |
| document_ids | UUID[] | FK:ST08 or attachment | N | per type | Assumed | |
| channel_sent | ENUM | — | C | T1 | Assumed | For replies: EMAIL in v1, APP later, LETTER. |
| actor_id, created_at | UUID, TIMESTAMP | — | Y | T1 | Known (Arch § Security audit) | Never edited; a correction is a new entry. |

### CC08 — Feedback Item

**Purpose:** One piece of feedback as received: complaint, compliment or suggestion. A complaint opens a case.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| feedback_id | UUID | PK | Y | T1 | Assumed (DL-123) | |
| campus_id | UUID | FK:OR04 | Y | T1 | Known | |
| category | ENUM | — | Y | T1 | Known (HB p.249 A) | COMPLAINT, COMPLIMENT, SUGGESTION. |
| topic | ENUM | — | Y | T1 | Assumed; list Open (OQ-133) | ACADEMIC, TRANSPORT, FEES, CANTEEN, FACILITIES, SAFETY, STAFF_CONDUCT, COMMUNICATION, ADMISSIONS, OTHER. Routes the case (CC02 intake rule). |
| from_type, from_id | ENUM, UUID | — | Y | T1 | Known (HB p.249) | GUARDIAN, STUDENT, STAFF, PUBLIC. |
| from_name, from_contact | VARCHAR | — | C | T2 | Known need (public complainants have no account) | Only for PUBLIC. |
| anonymous | BOOLEAN | — | Y | T1 | Open (OQ-134) | Whether anonymous feedback is accepted. |
| about_student_id | UUID | FK:ST01 | N | T1 | Assumed | |
| received_via, received_at | ENUM, TIMESTAMP | — | Y | T1 | Known (HB p.249) | Same values as CC03. |
| body | TEXT | — | Y | T2 | Known | As received; never edited. |
| acknowledged_at | TIMESTAMP | — | C | T1 | Known (HB p.249 B "within 2 working days") | For all three categories. |
| case_id | UUID | FK:CC03 | C | T1 | Assumed (DL-123) | Set for COMPLAINT. |
| review_cycle | VARCHAR(20) | — | C | T1 | Known (HB p.249 C "during internal review"); cycle Open (OQ-135) | For compliments and suggestions. |
| reclassified_from, reclass_reason | ENUM, TEXT | — | N | T1 | Assumed | |

### CC09 — Working Day Calendar

**Purpose:** The office working days that case deadlines count, per campus.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| calendar_id | UUID | PK | Y | T1 | Assumed (DL-122) | |
| campus_id, calendar_year | UUID, INT | UK | Y | T1 | Assumed | |
| working_week | VARCHAR(20) | — | Y | T1 | Assumed (Arch § How GIIS operates: SG Mon–Fri; India "Saturdays marked NWD") | MON-FRI; IN-NOI may differ (OQ-130). |
| public_holidays | JSON | — | Y | T1 | Known for GIIS SG (Holiday Schedule 2026-27, the same list as AT02 public holidays) | Shared with AT02, never retyped. |
| office_closures | JSON | — | N | T1 | Open (OQ-130) | Days the office is closed outside public holidays. |
| office_day_end | TIME | — | Y | T1 | Open (OQ-130) | When a due day ends (sample 18:00). |

### CC10 — Conversation

**Purpose:** A message thread between staff and the guardians of one student.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| conversation_id | UUID | PK | Y | T1 | Assumed (DL-127) | |
| campus_id | UUID | FK:OR04 | Y | T1 | Known | |
| student_id | UUID | FK:ST01 | Y | T1 | Assumed (DL-127) | Every conversation is about one student. Siblings get separate threads. |
| subject | VARCHAR(160) | — | Y | T1 | Assumed | |
| staff_participants | UUID[] | FK:E01 | Y | T1 | Assumed | Staff must have the student in data scope, or be a campus office listed in the directory. |
| guardian_participants | UUID[] | FK:ST04 | Y | T1 | Known need (HB p.34 parents mail teachers and staff) | Guardians with a verified relationship (ST05) and family access (F01). |
| started_by_type | ENUM | — | Y | T1 | Assumed | STAFF, GUARDIAN. |
| status | ENUM | — | Y | T1 | Assumed (DL-27) | OPEN, CLOSED, CONVERTED (to a case; read-only, link kept). |
| converted_case_id | UUID | FK:CC03 | C | T1 | Assumed (DL-127) | |
| last_message_at | TIMESTAMP | — | Y | T1 | Assumed | |

### CC11 — Message

**Purpose:** One message in a conversation.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| message_id | UUID | PK | Y | T1 | Known entity (Arch § Data model) | |
| conversation_id | UUID | FK:CC10 | Y | T1 | Assumed | |
| sender_type, sender_id | ENUM, UUID | — | Y | T1 | Assumed | STAFF, GUARDIAN, SYSTEM. |
| body | TEXT | — | Y | T2 | Assumed (DL-127) | No Tier 3 content (screen warns; detection Open, OQ-136). |
| attachment_ids | UUID[] | — | N | T2 | Assumed | Documents that belong to a record (medical certificate) go to that record, not the message. |
| sent_at | TIMESTAMP | — | Y | T1 | Assumed | |
| read_by | JSON | — | N | T1 | Assumed | Per participant read time. |
| deleted_at | TIMESTAMP | — | N | T1 | Assumed | Sender may withdraw within a set time; audit keeps the original (OQ-137). |

### CC12 — Announcement

**Purpose:** A notice sent to an audience defined by a rule (OBJ-26).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| announcement_id | UUID | PK | Y | T1 | Known entity (Arch § Data model) | |
| campus_id | UUID | FK:OR04 | C | T1 | Assumed | Empty for brand-country announcements (all campuses of a brand-country). |
| title, body | VARCHAR(160), TEXT | — | Y | T1 | Assumed | Announcements are Tier 1 by rule; no student names. |
| audience_rule | JSON | — | Y | T1 | Assumed (DL-128) | Scope (campus, offering, grade, class, house, staff team) × audience (GUARDIANS, STUDENTS, STAFF). e.g. `{campus: SG-PGL, grades: [6,7,8], audience: [GUARDIANS]}`. Resolved at send time. |
| author_id | UUID | FK:E01 | Y | T1 | Assumed | |
| approval_required, approved_by, approved_at | BOOLEAN, UUID, TIMESTAMP | — | C | T1 | Assumed (DL-128); approver Open (OQ-138) | Required when the audience is wider than the author's own class. |
| category | ENUM | — | Y | T1 | Assumed (DL-129) | SAFETY, REQUIRED, OPTIONAL (decides whether guardians can mute it). |
| ack_required, ack_due | BOOLEAN, DATE | — | N | T1 | Assumed (DL-128) | "I have read this." Not consent (OBJ-17). |
| publish_at, expires_at | TIMESTAMP | — | Y | T1 | Assumed | Scheduled sending. |
| channels | ENUM[] | — | Y | T1 | Known (Arch: push, email, SMS, WhatsApp) | Defaults from category; guardian preference applies (CC16). |
| status | ENUM | — | Y | T1 | Assumed (DL-27) | DRAFT, AWAITING_APPROVAL, SCHEDULED, SENT, WITHDRAWN. |
| translations | JSON | — | N | T1 | Open (OQ-25) | Language versions. |

### CC13 — Announcement Receipt

**Purpose:** One recipient's copy of an announcement: delivered, read, acknowledged.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| receipt_id | UUID | PK | Y | T1 | Assumed (DL-128) | |
| announcement_id | UUID | FK:CC12 | Y | T1 | Assumed | |
| recipient_account_id | UUID | FK:A01 | Y | T1 | Assumed | Resolved from the audience rule at send time; a family added later does not receive past announcements. |
| for_student_id | UUID | FK:ST01 | C | T1 | Assumed | Which child put the guardian in the audience (one receipt per guardian per announcement even with two children in it). |
| delivered_at, read_at, acknowledged_at | TIMESTAMP | — | N | T1 | Assumed | |

### CC14 — Notification Template

**Purpose:** The approved wording for one event, per brand-country, language and channel.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| template_id | UUID | PK | Y | T1 | Assumed (DL-129) | |
| brand_country_id | UUID | FK:OR03 | Y | T1 | Assumed (DL-110 pattern) | GSG provides a starter set. |
| event_code | VARCHAR(60) | — | Y | T1 | Known (each module's Events section) | e.g. `AttendanceAbsentUnexplained` (DL-107), `LeaveDecided`, `InvoiceIssued`, `ReportCardReleased`, `CaseAcknowledged`, `CaseReplied`, `CaseOverdue`. |
| category | ENUM | — | Y | T1 | Assumed (DL-129) | SAFETY, REQUIRED, OPTIONAL. |
| language, channel | VARCHAR(35), ENUM | UK | Y | T1 | Known need (Arch "per channel and language"); languages Open (OQ-25) | PUSH, EMAIL, SMS, WHATSAPP, IN_APP. |
| version, status | INT, ENUM | — | Y | T1 | Assumed | DRAFT, ACTIVE, RETIRED. WhatsApp templates may also need provider approval (OQ-139). |
| body | TEXT | — | Y | T1 | Assumed (DL-129) | Placeholders for names, dates and links only. Never Tier 3 values; never restricted case titles. |

### CC15 — Notification

**Purpose:** One notification to one recipient for one event, with delivery results. The delivery log.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| notification_id | UUID | PK | Y | T1 | Assumed (DL-129) | |
| event_code, event_id | VARCHAR, UUID | — | Y | T1 | Known (event log, Arch § Data model) | |
| recipient_account_id | UUID | FK:A01 | Y | T1 | Assumed | |
| about_student_id | UUID | FK:ST01 | C | T1 | Assumed | |
| template_id | UUID | FK:CC14 | Y | T1 | Assumed | |
| category | ENUM | — | Y | T1 | Assumed | |
| channels_attempted | JSON | — | Y | T1 | Assumed | Per channel: SENT, DELIVERED, FAILED, SKIPPED_BY_PREFERENCE, with provider reference. |
| fallback_used | BOOLEAN | — | Y | T1 | Assumed (DL-129) | REQUIRED and SAFETY fall back to another channel if the first fails. |
| sent_at | TIMESTAMP | — | Y | T1 | Assumed | |

### CC16 — Notification Preference

**Purpose:** One person's choice of channel and language for each category.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| preference_id | UUID | PK | Y | T1 | Assumed (DL-129) | |
| account_id | UUID | FK:A01 | Y | T1 | Assumed | Guardians, students and staff. |
| category | ENUM | UK | Y | T1 | Assumed | SAFETY, REQUIRED, OPTIONAL. |
| channels | ENUM[] | — | Y | T1 | Known need (Arch; ST07 channels PHONE, EMAIL, WHATSAPP) | At least one for SAFETY and REQUIRED. Contact details stay in ST07, never copied. |
| language | VARCHAR(35) | — | N | T1 | Known need (ST04 `preferred_language`); values Open (OQ-25) | Defaults from ST04. |
| quiet_hours | JSON | — | N | T1 | Assumed; Open (OQ-140) | OPTIONAL only; SAFETY ignores quiet hours. |
| opted_out | BOOLEAN | — | Y | T1 | Assumed (DL-129) | Allowed for OPTIONAL only. |

## How other modules use a Case

| Module record | Case type | Who receives / decides (GIIS SG example) | Deadline (GIIS SG) | Source |
|---|---|---|---|---|
| Complaint (CC08) | COMPLAINT | Recipient → process owner → supervisor → Dispute Resolution Committee (Country Director) | 2 / 7 / 14 / 21 working days, then CPE | HB p.249–250 |
| AT06 leave > 2 school days | LEAVE_OVER_LIMIT | Principal; class teacher and coordinator informed | Before the leave starts (Open, OQ-109) | DL-101 |
| AT10 ICA review (RESTRICTED) | ICA_REVIEW | Admissions/registrar (Tier 3) | Open (OQ-112) | DL-105 |
| FB14 refund | REFUND | Country Director + Finance Controller | Missing-documents notice 3, family 7; family escalation after 4 | HB p.244–245, DL-84 |
| FB16 long-leave pause | LONG_LEAVE | Principal processes, Associate Director Academic QA approves | Open | HB p.239, DL-86 |
| Withdrawal (ST02 `approval_case_id`) | WITHDRAWAL | Admissions accepts → Principal exit interview → Finance | Acknowledge 2; documents 7 after effective date | HB p.233–237 |
| Campus transfer (ST02) | CAMPUS_TRANSFER | Both principals | Acknowledge 2; decide in 3 weeks; profile in 3 | HB p.235–236 |
| AD10 Student Pass / MOE (RESTRICTED) | STUDENT_PASS | Admissions | Open | HB p.236, AD10 |
| AS13 predicted grade appeal | PREDICTED_GRADE_APPEAL | Committee headed by the Principal | Open | HB p.133, DL-94 |
| AS12 retention request (K–5) | RETENTION_REQUEST | Principal | Open | HB p.128, SR-AS-13 |
| CL06 year-end campus change | YEAR_END_CAMPUS_CHANGE | Both principals | Before the rollover is applied | DL-62 |
| OR05 operator handover | CAMPUS_OPERATOR_HANDOVER | Group (owner OQ-58) | Open | DL-50 |
| Access request and reviews | ACCESS_REQUEST, ONBOARDING_ACCESS, AFFILIATION_REVIEW, EXIT_REVIEW, CAMPUS_ATTESTATION | Per DL-08; Tier 3 type grant: Campus Leader (DL-69) | Not set (DL-08 names SLA timers, no values) | DL-08, DL-69 |

AS11 result appeals keep their own status and deadline in Assessment (DL-91) and have no `case_id`; whether they should also run on a Case is OQ-141.

## Field visibility by role

| Field group | Tier | Case assignee | Participants (A15) | Campus office queue | Principal | Brand-country / Country Director | Class teacher | Guardian (Parent app, later) | Group dashboards |
|---|---|---|---|---|---|---|---|---|---|
| Case header (number, type, status, due date) | T1 | M | V | V (own queue) | V (campus) | V (escalated, own types) | V own class, NORMAL family-visible types only | V own child, family-visible types | counts |
| Case content and timeline | per type | M | V/E | V | V | V when on the case | — | replies only | — |
| Internal notes | T2 | M | V/E | V | V | V when on the case | — | — | — |
| RESTRICTED case (any part) | T3 | M | V/E | — | only via A15 or A17 | only via A15 or A17 | — | — | totals only |
| Case policy, types | T1 | V | V | V | V | M (policy) | — | — | V |
| Feedback items (compliments, suggestions) | T2 | — | — | M | V | V | — | own | counts by topic |
| Conversations | T2 | — | — | M own office | V on request (OQ-142) | — | M own class | M own | — |
| Announcements | T1 | — | — | M | approve | M (brand-country) | M own class | V | counts |
| Notification log | T1 | — | — | V own sends | V | V | V own sends | own | counts |
| Notification preferences | T1 | — | — | V | — | — | — | M own | — |

## Enumerations (candidates, DL-27 rule)

| Entity | Field | Values | Source |
|---|---|---|---|
| CC01 | family | FEEDBACK, ACADEMIC, ENROLMENT, FINANCE, ATTENDANCE, IMMIGRATION, ACCESS, GOVERNANCE | Assumed |
| CC03 | status | NEW, IN_PROGRESS, WAITING, DECIDED, APPLIED, CLOSED, WITHDRAWN | DL-124 |
| CC03 | outcome | APPROVED, REJECTED, RESOLVED, NO_ACTION | DL-124 |
| CC03 | received_via | APP, EMAIL, PHONE, LETTER, IN_PERSON, HELPDESK, MESSAGE, SYSTEM | HB p.24, p.249 |
| CC03 | visibility | NORMAL, RESTRICTED | DL-19 |
| CC04 | step_code (complaint) | ACKNOWLEDGE, RESOLVE, SUPERVISOR, DISPUTE_COMMITTEE | HB p.249–250 |
| CC04 | status | PENDING, MET, BREACHED, SKIPPED, PAUSED | Assumed |
| CC05 | role_on_case | RECIPIENT, PROCESS_OWNER, SUPERVISOR, APPROVER, COMMITTEE_MEMBER, WATCHER, INFORMED | HB p.249–250, DL-101 |
| CC06 | decision | APPROVE, REJECT, RETURN_FOR_INFO | Assumed |
| CC07 | kind | REPLY_TO_FAMILY, INTERNAL_NOTE, STATUS_CHANGE, ASSIGNED, DOCUMENT_ADDED, DECISION, DEADLINE_BREACHED, ESCALATED, EXTERNAL_REFERRAL, LINKED | DL-126 |
| CC08 | category | COMPLAINT, COMPLIMENT, SUGGESTION | HB p.249 A ("Others: suggestions or comment") |
| CC08 | topic | ACADEMIC, TRANSPORT, FEES, CANTEEN, FACILITIES, SAFETY, STAFF_CONDUCT, COMMUNICATION, ADMISSIONS, OTHER | OQ-133 |
| CC10 | status | OPEN, CLOSED, CONVERTED | Assumed |
| CC12 | status | DRAFT, AWAITING_APPROVAL, SCHEDULED, SENT, WITHDRAWN | Assumed |
| CC12, CC14, CC16 | category | SAFETY, REQUIRED, OPTIONAL | DL-129 |
| CC14, CC16 | channel | PUSH, EMAIL, SMS, WHATSAPP, IN_APP | Arch § Tech stack; ST07 |

**Status chips (DL-124 Confirmed; now the Case row in rules-to-ui):** NEW → New · info · IN_PROGRESS → In progress · secondary · WAITING → Waiting for family · warning · DECIDED → Approved / Rejected / Resolved / No action · success or error · APPLIED → Done · success · CLOSED → Closed · default · WITHDRAWN → Withdrawn · default. Beside the status: **Overdue** · error, and the escalation step ("With supervisor", "With committee") · warning.

## Business rules

- **SR-CC-01 (Known for GIIS SG, HB p.249 B; Assumed mechanism DL-123):** Every feedback item is acknowledged within 2 working days of receipt. Staff send the acknowledgement from the item; an unacknowledged complaint shows Overdue on day 3.
- **SR-CC-02 (Known for GIIS SG, HB p.249 C–D):** The recipient replies in writing to close a complaint within 7 working days, or forwards it to the process owner, who replies within the same 7 days and copies the recipient.
- **SR-CC-03 (Known for GIIS SG, HB p.250 E–H; Assumed automatic move DL-123):** A complaint still open after 7 working days goes to the process owner's supervisor (due day 14); still open after 14 goes to the Dispute Resolution Committee chaired by the Country Director (due day 21). Each move notifies the new owner and the complainant.
- **SR-CC-04 (Known for GIIS SG, HB p.250 I):** If no solution is agreed by day 21, the Country Director proposes the CPE Dispute Resolution Scheme. Staff record the referral and any reference; the case stays open as EXTERNAL until the outcome is recorded.
- **SR-CC-05 (Assumed, DL-122; OQ-130, OQ-131):** Working days are Monday to Friday minus the campus country's public holidays. The clock pauses while the case is WAITING for the family; deadlines move by the paused working days.
- **SR-CC-06 (Assumed, DL-121):** A case keeps the policy version it started with. Publishing a new policy version affects new cases only.
- **SR-CC-07 (Confirmed DL-08, DL-84, DL-62; Assumed DL-120):** A case with several approvers is DECIDED only when every required approver has decided; one REJECT rejects the case unless the policy says otherwise.
- **SR-CC-08 (Assumed, DL-120):** DECIDED cases with a source record are APPLIED by the owning module (leave days marked ON_LEAVE, refund paid, enrolment ended); the case shows when and by whom.
- **SR-CC-09 (Confirmed DL-19, DL-125):** For people who are not named, a RESTRICTED case appears only as a locked row (type, received date, next due date; no student, title or content) in queue and module case lists they can already see; never in search, on teacher, class or student screens, in counts per student or in notifications. Opening it is refused. Every read of the content is logged (Tier 3).
- **SR-CC-10 (Assumed, DL-126):** Internal notes are never shown to families. A reply to the family is sent by email in v1 and is stored on the case.
- **SR-CC-11 (Assumed, DL-127):** Staff start conversations only with guardians of students in their data scope; guardians write to their child's teachers and listed campus offices. No parent-to-parent or student-to-student messaging.
- **SR-CC-12 (Assumed, DL-127):** Converting a message to a complaint creates the case with `received_via = MESSAGE` and `received_at` = the message's send time, so the deadline counts from when the parent wrote.
- **SR-CC-13 (Assumed, DL-128; OQ-138):** An announcement wider than the author's own class needs approval before it is sent. The audience is resolved at send time.
- **SR-CC-14 (Assumed, DL-129):** SAFETY and REQUIRED notifications cannot be turned off; they fall back to another channel if delivery fails. OPTIONAL notifications respect opt-out and quiet hours.
- **SR-CC-15 (Assumed, DL-129; DL-30):** Notification text never includes Tier 3 values or restricted case titles. A restricted case notice says only "You have a case to review" with a link.
- **SR-CC-16 (Known HB p.237, p.245):** Where a policy sets a family escalation path, the family is told who to escalate to and when, in the acknowledgement.

## Events

CaseOpened, CaseAssigned, CaseAcknowledged, CaseWaiting, CaseResumed, CaseDeadlineBreached, CaseEscalated, CaseDecided (source module applies), CaseApplied, CaseClosed, CaseWithdrawn, FeedbackReceived, ConversationStarted, MessageSent, AnnouncementSent, AnnouncementAcknowledged, NotificationSent, NotificationFailed. Inbound: every module event listed in CC14 templates. Events carry IDs only (architecture.md).

## Capabilities (new, DL-53 naming)

`cases.type.manage` (GSG platform team) · `cases.policy.manage` (brand-country) · `cases.case.view`, `cases.case.create`, `cases.case.work` (assign, reply, note, change status) · `cases.case.decide` (approver steps; module-specific approvals such as `attendance.leave.approve` and `gov.access_review.decide` still apply) · `cases.case.restrict` · `cases.feedback.manage` · `cases.report.view` (deadline performance) · `comms.conversation.use` · `comms.conversation.oversee` (OQ-142) · `comms.announcement.post`, `comms.announcement.approve` · `comms.template.manage` · `comms.notification_log.view`. Existing: `gov.case.view_all`, `gov.access_review.decide`.

## Decisions (DL-119 Confirmed; DL-120…DL-130 proposed, awaiting Henry)

| ID | Decision | Basis |
|---|---|---|
| DL-119 (**Confirmed**, Henry 22 Nov 2026) | Cases & Communications is one module (`communication_cases`, Engagement domain) with capability prefixes `cases` and `comms`; PTM booking, surveys and consent come later | Arch module 9; modules-and-services; DL-43, DL-53 |
| DL-120 (**Confirmed**, Henry 24 Nov 2026) | One Case engine holds status, owner, deadlines, escalation, approvals and timeline; the owning module keeps the detail, shows its own case list on its own screens and applies the decision; links both ways | Arch § Data model "one shared object with SLA timers"; 11 existing `case_id` fields |
| DL-121 (**Confirmed**, Henry 24 Nov 2026) | GSG defines case types; each brand-country publishes versioned case policies (deadline steps, routing, approvers, escalation); a case keeps the version it started with | DL-84, DL-110 patterns; Arch § Principles 3 |
| DL-122 (**Confirmed**, Henry 24 Nov 2026) | Deadlines count office working days (Mon–Fri minus the campus country's public holidays; vacations count); day 0 = receipt; the clock pauses while waiting for the family | HB "working days" (p.233–250); OQ-130, OQ-131 |
| DL-123 (**Confirmed**, Henry 24 Nov 2026) | Complaints follow the brand-country ladder (GIIS SG 2 / 7 / 14 / 21 working days, then CPE, recorded only); a missed deadline moves the case up automatically; compliments and suggestions are feedback items, acknowledged in 2 days, not cases | HB p.249–250; Arch § Key workflows |
| DL-124 (**Confirmed**, Henry 24 Nov 2026) | One case status list (NEW, IN_PROGRESS, WAITING, DECIDED + outcome, APPLIED, CLOSED, WITHDRAWN); Overdue and escalation step are flags beside it; replaces the OBJ-10 list in rules-to-ui | rules-to-ui Case row; a case can be overdue while in progress |
| DL-125 (**Confirmed**, Henry 24 Nov 2026) | RESTRICTED case types (ICA review, Student Pass; later child protection, SEN) open only for people named (A15) or holding a type grant (A17); others who see the same queue or module list see a locked row (type, dates, no student or content); never on teacher, class or student screens (DL-30); dashboards show totals | DL-19, DL-30, DL-69; Arch § Security |
| DL-126 (Assumed) | Case timelines separate replies to the family from internal notes; families see their own and their child's family-visible cases (status, next due date, replies); v1 staff log cases and reply by email | DL-118; HB p.249 "in writing" |
| DL-127 (Assumed) | Messages are staff ↔ guardian conversations about one student, within the staff member's data scope; no parent-to-parent; a message can become a case or leave request | HB p.34 MyGIIS Messaging; DL-73 |
| DL-128 (Assumed) | Announcements target an audience rule (campus, offering, grade, class, house, staff team); wider than own class needs approval; optional read acknowledgement, which is not consent | Arch § Data model "Announcement"; OBJ-17 |
| DL-129 (Assumed) | One notification service: modules publish events, templates per brand-country × language × channel, categories SAFETY / REQUIRED / OPTIONAL, guardians choose channel and language, only OPTIONAL can be muted; no Tier 3 in notification text; every send logged | Arch § Tech stack; ST04, ST07; DL-30 |
| DL-130 (Assumed) | Cases, messages, announcements and the delivery log live in the campus data location; the operating entity is copied onto the case; group views see counts only; two-campus cases live at the originating campus | DL-41, DL-47 |

## Open questions (new)

| ID | Question | Affects |
|---|---|---|
| OQ-130 | What counts as a "working day" for case deadlines at each campus: are school vacations working days for the office, what about India's Saturdays (NWD), and at what time does a due day end? | CC09, every deadline |
| OQ-131 | Does the complaint clock stop while the school waits for the complainant, and are the 14 and 21 working days counted from receipt or from the previous step? | CC04, DL-122 |
| OQ-132 | Are the enrolment clarification response times (Admissions Manager 48 hours; Finance Controller, Customer Relations 24 hours; HB p.24) cases with hour-based deadlines, or service targets on messages? | CC02, CC10 |
| OQ-133 | Which complaint topics and process owners does GIIS SG use (transport, canteen, fees, academic, staff conduct), and who is each owner's supervisor at step 14? | CC08 topic, CC02 intake and supervisor rules |
| OQ-134 | Are anonymous complaints accepted, and how are complaints about a named staff member handled (who must not see them)? | CC08, visibility |
| OQ-135 | When is the "internal review" of compliments and suggestions held (HB p.249 C), and who attends? | CC08 review cycle, reports |
| OQ-136 | Should the system check messages for Tier 3 content (health, immigration) or only warn the sender? | CC11 |
| OQ-137 | How long are cases, messages, announcements and delivery logs kept, can a sender delete a message, and how much MyGIIS message history moves at cutover? | Retention, migration |
| OQ-138 | Who approves a grade-wide or campus-wide announcement at GIIS SG (coordinator, Vice Principal, Principal, marketing), and who may send brand-country announcements? | CC12 approval |
| OQ-139 | Which channels does each brand-country use today (email, SMS, WhatsApp Business, app push), with which providers, and who pays for SMS and WhatsApp? | CC14, CC15 |
| OQ-140 | Are there quiet hours for teacher ↔ parent messages and optional notifications, and is there an expected reply time for teachers? | CC10, CC16 |
| OQ-141 | Should result appeals (AS11), which keep their own status and deadline today, also run on a Case so they appear in the same inbox? | AS11, CC01 |
| OQ-142 | May a Principal or coordinator read teacher ↔ parent conversations, and when (complaint, safeguarding concern, on request)? | `comms.conversation.oversee`, privacy notice |
| OQ-143 | What complaint processes do other brands and countries use (OWIS Singapore, GIIS India, regulators other than CPE), and do they have a dispute committee? | CC02 policies beyond GIIS SG |
| OQ-144 | Who is the "recipient" of a complaint that arrives by email to a general helpdesk address (HB p.8 Helpdesk.sg@…), and who triages the helpdesk queue? | CC02 intake rule, queues |
| OQ-145 | Do students raise complaints or message teachers in the system, and from which grade? | CC08, CC10, Student app |
| OQ-146 | Which notifications must reach both guardians and which only the primary contact or bill payer? | CC15 recipient rules |

## Readiness

- **Can be designed now:** the case inbox (my cases, team queue, overdue first, restricted cases hidden), case detail with the deadline ladder, timeline (reply vs internal note), approvals and a link to the source record; logging a complaint received by email or phone; feedback list (compliments and suggestions); case policy screen with GIIS SG values; staff ↔ parent conversations from the teacher's side; announcement composer with audience rule and approval; notification templates and the delivery log; deadline performance for the Principal. All with GIIS SG sample values labelled as settings.
- **Needs answers first:** working-day definition and pause rule (OQ-130, OQ-131), complaint topics and owners (OQ-133), anonymous complaints (OQ-134), announcement approver (OQ-138), channels and providers (OQ-139), message oversight (OQ-142), other brands' processes (OQ-143).
- **Parent app (later, DL-118):** "My requests" (status, next due date, replies), raise a complaint or compliment, messages with teachers, announcements with acknowledgement, notification settings per category. This file defines what parents see so the Parent app can reuse it.

# Global Campus OS — Staff & HR Data Dictionary v0.1 (draft)

**Status:** Proposed draft, 25 Nov 2026. DL-146…DL-159 Confirmed; DL-160…DL-164 Assumed; OQ-166…OQ-193 Open · **Owner:** Henry
**Cites:** [Architecture v1](../00-sources/Global_Campus_OS_Product_Architecture_v1.md) § Domains and modules (module 12 Staff and HR "Staff records, allocation; cover, training, appraisal"), § Users and roles (Principal approvals; staff across campuses and programmes), § Data model (People "Person… Staff… Role grant"; "One Person can be a parent on one campus and staff on another"), § Security (Tier 3 never leaves its cell; AI keeps a human approver) · [Employee Data Dictionary v1](../00-sources/Global_Campus_OS_Employee_Data_Dictionary_v1.md) E01–E08, BR-01…BR-20, Decisions D01–D15 (all Proposed, 14 Oct 2026) · [Organisation dictionary](./organisation-data-dictionary.md) OR03, OR04, OR07, OR08, OR09 · [Teaching & timetable dictionary](./teaching-timetable-data-dictionary.md) TE06, TE07, TE12, DL-143, OQ-159, OQ-163, OQ-164 · [Cases & communications dictionary](./cases-communications-data-dictionary.md) CC01, CC03, DL-120, DL-125, DL-129 · [Academic structure dictionary](./academic-structure-data-dictionary.md) AC07, DL-96 · [Attendance dictionary](./attendance-data-dictionary.md) AT01, AT02 · [architecture.md](./architecture.md) DL-39, DL-41, DL-42, DL-43, DL-45 · [permission catalogue](../04-access/permission-catalogue.md) `hr.*` seed capabilities · GIIS Student Handbook 2025-26 (HB) p.210–213 (Child Protection Policy: Principal is Designated Person; staff allegations, suspension pending investigation), p.138 (English teachers receive IB training at workshops) · DL-09, DL-11, DL-13, DL-16, DL-19, DL-22, DL-24, DL-27, DL-34, DL-36, DL-47, DL-53, DL-58, DL-59, DL-69, DL-73, DL-118
**Tóm tắt (VI):** Từ điển dữ liệu cho module Nhân sự (phạm vi "Full HR", Henry chọn 25/11/2026): hồ sơ nhân viên và hợp đồng (kế thừa E01–E08 của Employee DD v1), phòng ban, chức danh, người quản lý trực tiếp; nghỉ phép của nhân viên (loại phép, chính sách theo brand-country, số dư, xin và duyệt phép, tự động báo "vắng" sang module Giảng dạy để xếp dạy thay, không bao giờ gửi lý do); hồ sơ tuân thủ (giấy phép lao động, kiểm tra lý lịch/an toàn trẻ em, bằng cấp, chứng chỉ IB/CBSE/Cambridge, sơ cứu) với cảnh báo hết hạn; tuyển dụng (yêu cầu tuyển, ứng viên, phỏng vấn, offer); tiếp nhận và nghỉ việc (checklist, bàn giao); đánh giá năng lực hằng năm và đào tạo. Lương bổng do hệ thống payroll bên ngoài giữ. Mỗi trường có trạng thái Known / Assumed / Open.

> Drafted by Claude from product truth on 25 Nov 2026. The architecture's module 12 "Staff and HR" covers staff records, allocation, cover, training and appraisal; allocation and cover were designed in Teaching (DL-131). This file covers **who works for the school, on what terms, when they are away, whether they are cleared to work with children, how they are hired, start and leave, and how they are reviewed and trained.** Payroll and pay stay in each brand-country's payroll system (DL-147).

## How to read this file

Same layout and rules as the [Student Directory Spec](./student-directory-data-dictionary.md#how-to-read-this-file): **Known** (cited), **Assumed** (DL), **Open** (OQ); tiers T1/T2/T3 (DL-11); `Y`/`N`/`C` required. Every row also carries `tenant_path`, `data_region` (UI term **Data location**, DL-47) and the shared common fields. Facts from the GIIS Handbook are **GIIS Singapore examples**, not group rules (DL-22). The Handbook is a student handbook, so almost no staff policy is Known: leave entitlements, check types, appraisal cycles and hiring steps below are candidates until HR owners answer (section T of open questions). All enums are unapproved candidates (DL-27 rule). All example values are synthetic.

## Scope

Approved by Henry, 25 Nov 2026 (decision card "Full HR", chosen over the smaller recommended "Records + leave + compliance").

| In this dictionary | Not in this dictionary (owner) |
|---|---|
| Staff records: employee profile, employments, campus assignments, departments, positions, line managers (refines Employee DD E01–E08) | Login accounts, access grants, teams, templates (Identity and Access Registry, A01–A17, `04-access/`) |
| Contracts: type, FTE, probation, notice, renewals; emergency contacts | Pay, salary, bank details, tax, CPF and payroll runs (payroll system per brand-country; receives staff changes from Campus OS, DL-147) |
| Staff leave: leave types, policies per brand-country, balances, requests and approvals | Why a teacher is away is never sent to Teaching; Teaching records only "away" and arranges cover (TE12, TE13, DL-143) |
| Compliance: work passes, safeguarding and background checks, qualifications, board training, first aid; requirements per position; expiry alerts | Student identity documents and Student Pass (Student Records, Tier 3, DL-30) |
| Recruitment: vacancy requests, candidates, stages, interviews, offers | Job advertising on external boards, careers website (later) |
| Joining and leaving: checklists, handover lists, exit review | Teaching assignments and academic responsibilities themselves (Teaching TE06, TE07) |
| Appraisal: annual cycles, goals, self-review, manager review, observation notes, outcome | Staff conduct investigations and child-protection cases (Cases engine, RESTRICTED type, DL-163) |
| Training: records, hours, certificates, requests | Learning content and course delivery (LMS, later) |

## Modeling boundaries

- **Staff & HR is one module in the Staff and HR domain (DL-146).** Capability prefix `hr` (DL-53, seed codes already in the catalogue); module team key `staff_hr` (DL-04). It becomes the home of Employee DD E01–E05, E07 and E08: the IDs stay, the fields are refined here, and the source file stays as it is (`00-sources/`). E06 Campus is already OR04 (Organisation dictionary). Other modules read from it: Teaching (who can be assigned, who is away, DL-156, DL-158), Identity and Access Registry (joiners and leavers start access reviews, BR-06, BR-10), Cases (staff concerns, DL-163), Notifications (DL-129).
- **Campus OS owns the whole staff record (DL-147, Confirmed by Henry 25 Nov 2026).** From each brand-country's go-live, Campus OS is the only place staff data is created or changed: identity, employments, contracts, campus, department, line manager, leave, compliance, hiring, appraisal and training. Any HR system the brand uses today stops editing staff data at that date; its records are imported once at cutover (DL-45 authority register, one cutover date for the whole record). Payroll systems become receivers: Campus OS sends them the staff changes they need (new joiner, legal name, contract dates, FTE, end date, unpaid leave) through E08, or HR re-keys them where no feed exists. Pay, bank and tax data still never enter Campus OS. Which payroll system each brand-country uses and what it needs to receive is Open (OQ-166).
- **One Person, one employee, employments per employer (DL-148).** An employee is the Employee profile of a global Person (DL-42): a teacher who is also a parent has one Person and one login. `employee_id` never changes (BR-01). Each contract with a legal employer is an employment (E02) with its own employee number (Employee DD D02) and its `operating_entity_id` (DL-41). Concurrent employments with two legal entities are allowed (Employee DD D03; OQ-167).
- **Campuses: one primary; a campus move gives standard access through the Registry (DL-149, Confirmed by Henry 27 Nov 2026).** An active employment has exactly one primary campus; group, brand and country office staff may have none (Employee DD D04). A teacher can be assigned to more campuses (SG-PGL and SG-ECS, OQ-154). When a campus assignment is added or ended, the Access Registry automatically grants the position's standard access template at the new campus from the move date and ends the old campus's position access on the end date (like DL-73 roster scope); every grant is logged, the Module Admin can remove it, and it never includes Tier 3 (A17 still needs its own approval, DL-69). HR never grants access directly: the campus field is an input the Registry reads (BR-06 amended for campus assignments only; AR-09 holds). Which access each position's template holds is Open (OQ-192).
- **Departments, positions and two managers are HR facts, not access (DL-150, Confirmed by Henry 27 Nov 2026).** Departments (E03) are set per brand-country or campus: academic departments (Sciences, Languages) and functions (Admissions, Finance, Facilities). Positions (E05) come from a GSG job family list that each brand names its own way ("Teacher – Secondary", "HR Executive"). Each employment has two managers on its primary E04: a **line manager** (`manager_assignment_id`), who approves leave and sees the team's leave and compliance, and an **appraisal reviewer** (`appraisal_reviewer_assignment_id`), who runs the appraisal. They can be the same person (the default when HR creates the employment) or different: Farah Ismail's line manager is Meera Joshi (Level Coordinator) and her appraisal reviewer is Suresh Pillai (Academic Supervisor). An HR position is not an access role (A06) and not an academic responsibility (TE07). The only link is DL-149: a campus assignment makes the Access Registry apply the position's standard access template at that campus.
- **Who sees what (DL-151).** Colleagues at the campus see the staff directory (Tier 1: name, photo, position, department, campus, work contact). A line manager also sees their team's leave dates and types and compliance status; the appraisal reviewer sees the appraisals they run (DL-150). HR sees Tier 2 (personal contact, address, date of birth, nationality, emergency contacts, contracts, qualifications). Tier 3 (identity and work pass numbers, background check results, medical certificates, staff concern outcomes) needs an HR capability plus a Tier 3 type grant (A17, DL-69). Staff see and update their own contact details and emergency contacts at once; changes to legal name, nationality or identity need HR to verify a document.
- **Contracts are effective-dated terms (DL-152).** Each employment has contract terms (HR01) with type, FTE, contracted hours, probation end and notice period; a renewal or FTE change is a new term, never an edit (DL-24). HR is alerted before a fixed-term contract or probation ends (campus setting, no group default; DL-59). The signed contract is a Tier 2 document. Salary is not stored.
- **Leave starts from a GSG country template; each brand-country adopts and adjusts it (DL-153, Confirmed by Henry 27 Nov 2026).** GSG HR keeps one leave template per country (Singapore, India, Japan…): the usual leave types (annual, sick, hospitalisation, maternity, paternity, childcare, compassionate, unpaid, study, duty) with a policy per type × employee category, and the statutory minimum days where the law sets one. A brand-country adopts its country's template once (a copy, not a live link) and then changes days, accrual, carry forward, certificate and school-day rules for its own staff. A brand-country policy below the template's statutory minimum can't be activated. When GSG publishes a new template version (for example a law change), each brand-country that adopted it sees "Template updated" and chooses to apply or keep its own; nothing changes by itself. Policies are versioned. GIIS SG, GIIS India and OWIS entitlements are Open (OQ-169); who in GSG maintains templates is Open (OQ-193).
- **Leave requests go to the line manager, long or term-time leave to the Principal (DL-154).** A staff member applies in the staff app for days or half days. The line manager approves; leave longer than a campus threshold, or a teacher's leave on school days outside the policy, also needs the Principal. When an approver is away, their delegate approves (A16, DL-36). Sick leave can be reported the same morning and approved afterwards, with the certificate uploaded within the policy's days. Approved leave can be cancelled before it starts by the employee, after by HR.
- **Balances are calculated, never typed (DL-155).** A balance per employee × leave type × leave year is entitlement (pro-rated for start date and FTE) + carried forward + HR adjustments − taken − booked. HR adjustments need a reason and are audited. Staff see their own balance before they apply; a request over the balance is blocked unless the type allows going negative.
- **Approved leave tells Teaching "away", never why (DL-156).** When leave is approved or sick leave is reported, Staff & HR creates a teacher absence in Teaching (TE12, `source` STAFF_LEAVE) with only dates and periods, so cover can be arranged (DL-143; answers the "feed automatically" part of OQ-163). Cancelling the leave cancels the absence. Colleagues and coordinators see "Away"; the leave type is seen only by the employee, their line manager, the Principal and HR.
- **Compliance items and requirements per brand-country and position (DL-157).** A catalogue of compliance item types (work pass, background or police check, safeguarding training, teaching qualification, board training such as IB Category 1, first aid, medical check, teacher registration) and a requirement per brand-country × position or category: required before start or within N days, how often it renews. GIIS teachers "receive IB training at workshops" (HB p.138); which checks Singapore and India require is Open (OQ-171, OQ-172).
- **Compliance status gates work with children, never ends employment by itself (DL-158).** Each employee has one compliance status: Cleared, Due soon (an item expires within the alert window), Action needed (an item is missing or expired but not a blocking type) or Not cleared (a blocking item, such as a safeguarding check or work pass, is missing or expired). Not cleared blocks the employment from starting, blocks new teaching assignments and leaves the teacher out of cover suggestions (TE06, TE13); it opens an HR case but never suspends or ends employment automatically. A missing board qualification for an assignment is a warning to the coordinator, not a block (partly answers OQ-159). Alerts go at 90, 60 and 30 days (campus setting).
- **Recruitment is a vacancy, candidates and an offer (DL-159).** A line manager or Principal requests a vacancy (position, department, campus, FTE, start date, reason: new or replacement); the Principal approves, and the Country Director too when it adds headcount (OQ-174). Candidates are matched to existing Persons through the review queue, never by name or email (DL-58), so a parent or former employee who applies is recognised. Each application moves through stages; each interviewer records structured feedback (Tier 2) before seeing others'. An offer needs approval; when accepted it creates the employment as PENDING with a joining checklist. Unsuccessful candidates' data is kept for a country retention period (OQ-176).
- **Joining and leaving follow checklists (DL-160).** A checklist template per brand-country × category lists tasks with owners: compliance items collected, contract signed, account invite (sent 7 days before start, DL-13), access request case, induction, campus tour. Leaving: when an end date is set the employee shows **Leaving**; Staff & HR lists everything that must be handed over (teaching assignments, class teacher roles, academic responsibilities, open cases, approvals they hold as delegate), opens the exit review (BR-10), and the account is disabled after the last day (DL-13).
- **Appraisal is an annual cycle per brand-country (DL-161).** A cycle follows the academic year of the campus × programme (DL-96) and has stages: goals set, mid-year check-in, self-review, manager review, moderation by the Principal, outcome shared. Teachers can have lesson observation notes attached. Rating scales are set per brand; results are Tier 2, seen by the employee, their reviewer, the Principal and HR, never by colleagues; Campus OS does not link ratings to pay (OQ-177, OQ-178).
- **Training builds a record that can clear compliance (DL-162).** Training records hold course, provider, dates, hours and certificate; a record of a type that matches a compliance item (child-protection training, IB Category 1, first aid) updates that item. Staff request training; the line manager approves. Hours targets per year are a brand-country setting (OQ-179).
- **Staff concerns are restricted cases; suspension shows no reason (DL-163).** A concern or allegation about a staff member is a RESTRICTED case type (STAFF_CONCERN) in the Case engine (DL-120, DL-125), open only to the Designated Person for Child Protection, the Principal and named HR (HB p.210–213: the Principal is the Designated Person; "consideration will be given as to whether that member of staff should face suspension"). If the employee is suspended, the employment shows SUSPENDED with no reason outside the case; the account is suspended (DL-13) and Teaching sees "Away". The staff member named in a complaint never sees it (OQ-134).
- **Staff data stays in the employer's data location (DL-164).** An employee's HR records live in the data location of the country of the employing legal entity (DL-47); a person employed in two countries has a profile in each, joined by the global Person (DL-42). Group views see counts (headcount, leave days, compliance status totals), never names, unless the viewer has access in that data location. Retention after leaving is per country (OQ-185).

## Entities

| ID | Entity | Module | Status |
|---|---|---|---|
| E01 | Employee (refined) | Staff & HR (OBJ-05) | Known entity (Employee DD E01, Arch § Data model "Staff"); linked to Person (DL-42, DL-148) |
| E02 | Employment (refined) | Staff & HR (OBJ-06) | Known entity (Employee DD E02); concurrent rule Confirmed (DL-148) |
| E03 | Department (Organizational Unit) | Staff & HR (OBJ-06) | Known entity (Employee DD E03); scope Confirmed (DL-150) |
| E04 | Organizational Assignment | Staff & HR (OBJ-06) | Known entity (Employee DD E04); line manager Confirmed (DL-150) |
| E05 | Position | Staff & HR (OBJ-06) | Known entity (Employee DD E05); job families Confirmed (DL-150) |
| E07 | Employee Campus Assignment | Staff & HR (OBJ-05) | Known (Employee DD E07; multi-campus Confirmed input); primary rule Confirmed (DL-149) |
| E08 | External Employee Mapping | Staff & HR (OBJ-05) | Known entity (Employee DD E08); outbound to payroll Confirmed (DL-147) |
| HR01 | Contract Term | Staff & HR (OBJ-06) | Confirmed (DL-152) |
| HR02 | Emergency Contact | Staff & HR (OBJ-05) | Confirmed (DL-151) |
| HR03 | Leave Type | Staff & HR (OBJ-28) | Confirmed (DL-153) |
| HR04 | Leave Policy | Staff & HR (OBJ-28) | Confirmed (DL-153); values Open (OQ-169) |
| HR05 | Leave Balance | Staff & HR (OBJ-28) | Confirmed (DL-155) |
| HR06 | Leave Request | Staff & HR (OBJ-28) | Known need (Arch module 12; TE12 `source` STAFF_LEAVE; OQ-163); model Confirmed (DL-154) |
| HR07 | Leave Approval Step | Staff & HR (OBJ-28) | Confirmed (DL-154) |
| HR08 | Compliance Item Type | Staff & HR (OBJ-29) | Known need (HB p.210–213 child protection; IB training); catalogue Confirmed (DL-157) |
| HR09 | Compliance Requirement | Staff & HR (OBJ-29) | Confirmed (DL-157); values Open (OQ-171, OQ-172) |
| HR10 | Compliance Record | Staff & HR (OBJ-29) | Confirmed (DL-157, DL-158) |
| HR11 | Vacancy | Staff & HR (OBJ-30) | Confirmed (DL-159) |
| HR12 | Candidate Application | Staff & HR (OBJ-30) | Confirmed (DL-159) |
| HR13 | Interview Feedback | Staff & HR (OBJ-30) | Confirmed (DL-159) |
| HR14 | Offer | Staff & HR (OBJ-30) | Confirmed (DL-159) |
| HR15 | Checklist Task | Staff & HR (OBJ-06) | Known need (BR-10 exit review; DL-13 invite timing); model Assumed (DL-160) |
| HR16 | Appraisal Cycle | Staff & HR (OBJ-31) | Known need (Arch module 12 "appraisal"); model Assumed (DL-161) |
| HR17 | Appraisal | Staff & HR (OBJ-31) | Assumed (DL-161) |
| HR18 | Training Record | Staff & HR (OBJ-31) | Known need (Arch module 12 "training"; HB p.138 IB training at workshops); model Assumed (DL-162) |

OR03, OR04, OR07, OR08, OR09, AC07, TE06, TE07, TE12, CC01, CC03, A01, A15, A16 and A17 are referenced, not redefined. Employee DD E06 is OR04.

## Detailed attribute dictionary

### E01 — Employee (refined)

**Purpose:** The staff profile of one person, across employers and campuses. Fields from Employee DD E01 keep their meaning; new ones are marked.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| employee_id | UUID | PK | Y | T1 | Known (Employee DD E01, BR-01) | Never reused. Referenced by TE06, TE12, TE13. |
| person_id | UUID | FK:Person | Y | T1 | Confirmed (DL-42) | New. One Person, one employee profile per data location (DL-164). |
| preferred_name | VARCHAR(160) | — | N | T1 | Known (E01) | Shown in the directory and on timetables. |
| legal_first_name / legal_last_name | VARCHAR(120) | — | Y / C | T2 | Known (E01) | Written only in Campus OS; sent to payroll (DL-147). |
| work_email / work_phone | VARCHAR | — | N | T1 | Known (E01) | |
| profile_photo_ref | VARCHAR(512) | — | N | T1 | Known field (E01); tier Confirmed (DL-151) | Employee DD had Confidential; the directory shows it (DL-151). |
| date_of_birth | DATE | — | Y | T2 | Assumed | New. Needed for checks and work passes. |
| gender | ENUM | — | N | T2 | Assumed (DL-27) | New. As the person states it. |
| nationality | CHAR(2)[] | — | Y | T2 | Assumed | New. Drives which work pass is required (HR09). |
| personal_email / personal_phone | VARCHAR | — | N | T2 | Assumed | New. Self-service (DL-151). |
| home_address | JSON | — | N | T2 | Assumed | New. Self-service. |
| identity_document | JSON | — | C | T3 | Confirmed (DL-151) | New. NRIC/FIN, passport or Aadhaar number per country; only with A17. Never on an event (architecture.md). |
| preferred_locale | VARCHAR(35) | — | N | T1 | Known (E01) | DL-15. |
| compliance_status | ENUM | — | Y | T1 | Confirmed (DL-158) | New, derived from HR10: CLEARED, DUE_SOON, ACTION_NEEDED, NOT_CLEARED. Status only; details are T2/T3. |
| record_status | ENUM | — | Y | T1 | Known (E01) | ACTIVE, INACTIVE, ARCHIVED. Not employment status. |

### E02 — Employment (refined)

**Purpose:** One engagement with one legal employer.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| employment_id | UUID | PK | Y | T1 | Known (E02) | |
| employee_id | UUID | FK:E01 | Y | T1 | Known (E02) | |
| employer_entity_id | UUID | FK:OR07 | Y | T1 | Known (E02 `employer_org_id`); catalogue Confirmed (DL-41) | The legal employer. |
| operating_entity_id | UUID | FK:OR07 | Y | T1 | Confirmed (DL-41) | New. Copied at creation; controller slot (OQ-06). |
| employee_number | VARCHAR(64) | UK with employer | C | T1 | Known (E02, Decision D02) | Per employer. |
| employee_category | ENUM | — | Y | T1 | Known field (E02); values Assumed (DL-27) | TEACHING, TEACHING_SUPPORT, NON_TEACHING, LEADERSHIP, OTHER. Selects leave and compliance rules. |
| employment_type | ENUM | — | Y | T1 | Known (E02) | PERMANENT, FIXED_TERM, PART_TIME, CASUAL, OTHER. Current term in HR01. |
| start_date / end_date | DATE | — | Y / N | T1 | Known (E02) | End date set → shown as Leaving (DL-160). |
| employment_status | ENUM | — | Y | T1 | Known (E02); transitions Assumed (DL-160, DL-163) | PENDING, ACTIVE, SUSPENDED, ENDED. History kept (DL-24). |
| leaving_reason | ENUM | — | C | T2 | Known field (E02 `termination_reason_code`); values Assumed | RESIGNED, CONTRACT_ENDED, RETIRED, DISMISSED, MUTUAL, TRANSFERRED_WITHIN_GSG, OTHER. Detail stays in the case. |
| source_hiring_id | UUID | FK:HR14 | N | T1 | Confirmed (DL-159) | New. The offer that created it. |

### E03 — Department (Organizational Unit)

**Purpose:** A department or function staff belong to.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| org_unit_id | UUID | PK | Y | T1 | Known (E03) | |
| scope_node_id | UUID | FK:OR01 | Y | T1 | Confirmed (DL-150) | Brand-country or campus. Replaces `organization_id`. |
| parent_org_unit_id | UUID | FK:E03 | N | T1 | Known (E03) | No cycles. |
| unit_code / unit_name | VARCHAR | UK with scope | Y | T1 | Known (E03) | "SCI", "Sciences". |
| unit_type | ENUM | — | Y | T1 | Known field; values Assumed | ACADEMIC_DEPARTMENT, FUNCTION, TEAM. |
| unit_status | ENUM | — | Y | T1 | Known (E03) | ACTIVE, INACTIVE. |

### E04 — Organizational Assignment

**Purpose:** Which position an employment holds, in which department, reporting to whom.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| org_assignment_id | UUID | PK | Y | T1 | Known (E04) | |
| employment_id | UUID | FK:E02 | Y | T1 | Known (E04) | |
| org_unit_id | UUID | FK:E03 | Y | T1 | Known (E04) | |
| position_id | UUID | FK:E05 | Y | T1 | Known (E04) | |
| manager_assignment_id | UUID | FK:E04 | C | T1 | Known (E04); line manager Confirmed (DL-150) | Line manager: approves leave. Required unless the position heads the brand-country. No cycles. |
| appraisal_reviewer_assignment_id | UUID | FK:E04 | C | T1 | New, Confirmed (DL-150) | Appraisal reviewer. Defaults to the line manager; HR may set someone else (for example the Head of Department). Required when the employment is in an appraisal cycle. No cycles; not the employee. |
| is_primary | BOOLEAN | — | Y | T1 | Known (E04) | One primary per employment. |
| valid_from / valid_to | DATE | — | Y / N | T1 | Known (E04) | |
| assignment_status | ENUM | — | Y | T1 | Known (E04) | PLANNED, ACTIVE, ENDED, CANCELLED. |

### E05 — Position

**Purpose:** A job title from the brand's catalogue.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| position_id | UUID | PK | Y | T1 | Known (E05) | |
| brand_id | UUID | FK:OR02 | Y | T1 | Confirmed (DL-150) | Each brand names its own titles. |
| position_code / position_title | VARCHAR | UK with brand | Y | T1 | Known (E05) | "Teacher – Secondary", "HR Executive". |
| job_family | ENUM | — | Y | T1 | Known field (E05); GSG list Confirmed (DL-150) | TEACHING, LEADERSHIP, LEARNING_SUPPORT, ADMISSIONS, FINANCE, HR, IT, OPERATIONS, OTHER. Group reporting uses this. |
| employee_category | ENUM | — | Y | T1 | Assumed | Default for E02. |
| position_status | ENUM | — | Y | T1 | Known (E05) | ACTIVE, INACTIVE. |

### E07 — Employee Campus Assignment

Fields as Employee DD E07, with: `campus_id` FK:OR04 (E06 is OR04); `is_primary` exactly one TRUE per ACTIVE employment unless the position's scope is above campus (DL-149); `assignment_reason` ENUM HOME, ADDITIONAL_TEACHING, SECONDMENT, TRANSFER (Assumed). Adding or ending one emits `CampusAssignmentChanged`; the Access Registry applies or ends the position's standard access template at that campus (DL-149), never Tier 3.

### E08 — External Employee Mapping

Fields as Employee DD E08, with `direction` ENUM (new, Confirmed DL-147): IMPORTED_AT_CUTOVER (the one-time load from today's HR system) or PAYROLL_FEED (Campus OS sends changes out). Nothing flows back in after go-live. `sync_status` FAILED puts the change in HR's "Not sent to payroll" list.

### HR01 — Contract Term

**Purpose:** The terms of an employment for a period. A change is a new term.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| contract_term_id | UUID | PK | Y | T1 | Confirmed (DL-152) | |
| employment_id | UUID | FK:E02 | Y | T1 | Assumed | |
| contract_type | ENUM | — | Y | T1 | Assumed (DL-27) | PERMANENT, FIXED_TERM, PART_TIME, CASUAL. |
| fte | DECIMAL(3,2) | — | Y | T1 | Assumed | 1.00 full time; pro-rates leave (DL-155). Teaching load is periods (TE08), not FTE. |
| contracted_hours_week | DECIMAL | — | N | T2 | Assumed | |
| valid_from / valid_to | DATE | — | Y / C | T1 | Assumed | `valid_to` required for FIXED_TERM. |
| probation_end | DATE | — | N | T2 | Assumed | Alert before (setting, DL-152). |
| notice_period_days | INT | — | N | T2 | Assumed | |
| work_pattern | JSON | — | N | T1 | Assumed | Working days for part-time staff ("Mon–Wed"). Used for leave days and cover suggestions. |
| contract_document_ref | VARCHAR(512) | — | N | T2 | Assumed | Signed contract. |
| term_status | ENUM | — | Y | T1 | Assumed | DRAFT, SIGNED, ACTIVE, ENDED. |

### HR02 — Emergency Contact

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| emergency_contact_id | UUID | PK | Y | T2 | Confirmed (DL-151) | |
| employee_id | UUID | FK:E01 | Y | T2 | Assumed | |
| name / relationship / phone | VARCHAR | — | Y | T2 | Assumed | Self-service. At least one required before start (HR15). |
| priority | INT | — | Y | T2 | Assumed | 1 = call first. |

### HR03 — Leave Type

**Purpose:** A kind of leave in a GSG country template or offered by a brand-country.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| leave_type_id | UUID | PK | Y | T1 | Confirmed (DL-153) | |
| owner_level | ENUM | — | Y | T1 | Confirmed (DL-153) | GSG_TEMPLATE (with `country_code`) or BRAND_COUNTRY (with `brand_country_id`). |
| country_code | CHAR(2) | — | C | T1 | Confirmed (DL-153) | ISO 3166; template types only. |
| brand_country_id | UUID | FK:OR03 | C | T1 | Assumed | Brand-country types only. |
| adopted_from_type_id | UUID | FK:HR03 | N | T1 | Confirmed (DL-153) | The template type it was copied from. |
| code / name | VARCHAR | UK with brand-country | Y | T1 | Assumed | "AL Annual leave". |
| leave_category | ENUM | — | Y | T1 | Assumed (DL-27) | ANNUAL, SICK, HOSPITALISATION, MATERNITY, PATERNITY, CHILDCARE, COMPASSIONATE, STUDY, UNPAID, DUTY, OTHER. DUTY (training, trip) isn't deducted. |
| type_status | ENUM | — | Y | T1 | Assumed | ACTIVE, RETIRED. |

### HR04 — Leave Policy

**Purpose:** The rule for one leave type and employee category, versioned; either a GSG country template policy or a brand-country's adopted policy (DL-153).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| leave_policy_id | UUID | PK | Y | T1 | Confirmed (DL-153) | |
| leave_type_id | UUID | FK:HR03 | Y | T1 | Assumed | |
| employee_categories | ENUM[] | — | Y | T1 | Assumed | Teaching staff and office staff often differ (OQ-169). |
| adopted_from_policy_id / adopted_version | UUID FK:HR04 / INT | — | N | T1 | Confirmed (DL-153) | The template policy and version a brand-country policy was copied from; a newer template version shows "Template updated" until HR applies or dismisses it. |
| statutory_min_days | DECIMAL | — | N | T1 | Confirmed (DL-153); values Open (OQ-169) | Set on template policies only. A brand-country policy with `days_per_year` below it can't become ACTIVE. |
| version / valid_from | INT / DATE | — | Y | T1 | Assumed | New version, never edit. |
| leave_year | ENUM | — | Y | T1 | Assumed | CALENDAR, ACADEMIC (follows AC07), EMPLOYMENT_ANNIVERSARY. |
| days_per_year | DECIMAL | — | C | T1 | Open (OQ-169) | Null = "Not set" (DL-59). |
| accrual | ENUM | — | Y | T1 | Assumed | UPFRONT, MONTHLY. |
| carry_forward_max_days | DECIMAL | — | N | T1 | Open (OQ-169) | |
| min_service_months | INT | — | N | T1 | Open | Eligibility. |
| certificate_after_days | INT | — | N | T1 | Assumed | Sick leave: certificate needed from this many days (0 = always). |
| certificate_due_days | INT | — | N | T1 | Assumed | Days after return to upload it. |
| allow_half_day / allow_negative | BOOLEAN | — | Y | T1 | Assumed | |
| school_day_rule | ENUM | — | Y | T1 | Confirmed (DL-154) | ANY, PRINCIPAL_APPROVAL, NOT_ALLOWED. Whether teachers may take it on school days (AT01). |
| principal_over_days | INT | — | N | T1 | Confirmed (DL-154) | Requests longer than this also need the Principal. Campus may override (OR09). |
| policy_status | ENUM | — | Y | T1 | Assumed | DRAFT, ACTIVE, RETIRED. |

### HR05 — Leave Balance

**Purpose:** What one employee has of one leave type in one leave year. Calculated.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| balance_id | UUID | PK | Y | T2 | Confirmed (DL-155) | |
| employee_id / leave_type_id | UUID | UK with year | Y | T2 | Assumed | |
| leave_year_start / leave_year_end | DATE | — | Y | T2 | Assumed | |
| entitlement_days | DECIMAL | — | Y | T2 | Derived | Pro-rated for start date and FTE (HR01). |
| carried_forward_days | DECIMAL | — | Y | T2 | Derived | |
| adjustment_days | DECIMAL | — | Y | T2 | Assumed | Sum of HR adjustments, each with a reason (audited). |
| taken_days / booked_days | DECIMAL | — | Y | T2 | Derived | From HR06 APPROVED (past / future). |
| available_days | DECIMAL | — | Y | T2 | Derived | entitlement + carried + adjustment − taken − booked. |

### HR06 — Leave Request

**Purpose:** A staff member's request for leave, or a same-day sick report.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| leave_request_id | UUID | PK | Y | T1 | Confirmed (DL-154) | |
| employee_id | UUID | FK:E01 | Y | T1 | Assumed | |
| employment_id | UUID | FK:E02 | Y | T1 | Assumed | |
| leave_type_id | UUID | FK:HR03 | Y | T2 | Confirmed (DL-156) | The type is T2: colleagues see "Away" only. |
| date_from / date_to | DATE | — | Y | T1 | Assumed | Inclusive. |
| part_of_day | ENUM | — | N | T1 | Assumed | FULL, AM, PM; or `periods` for teachers (TE02). |
| days_counted | DECIMAL | — | Y | T1 | Derived | Working days in the employee's pattern minus public holidays (CC09) and, for teaching staff on ACADEMIC policies, per OQ-170. |
| note | TEXT | — | N | T2 | Assumed | Free text to the approver. Never medical detail; the UI says so. |
| certificate_ref | VARCHAR(512) | — | C | T3 | Confirmed (DL-151) | Medical certificate. HR with A17 only; the line manager sees "Certificate received". |
| request_kind | ENUM | — | Y | T1 | Assumed | PLANNED, SAME_DAY_REPORT (reported the morning of, approved after), ENTERED_BY_HR. |
| cover_note | TEXT | — | N | T1 | Confirmed (DL-156) | For teachers: work set for cover; copied to TE12 `cover_note`. |
| request_status | ENUM | — | Y | T1 | Assumed (DL-27) | DRAFT, PENDING, APPROVED, REJECTED, CANCELLED, CERTIFICATE_DUE. |
| teaching_absence_id | UUID | FK:TE12 | N | T1 | Confirmed (DL-156) | Set when the absence is created. |

### HR07 — Leave Approval Step

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| step_id | UUID | PK | Y | T1 | Confirmed (DL-154) | |
| leave_request_id | UUID | FK:HR06 | Y | T1 | Assumed | |
| sequence | INT | — | Y | T1 | Assumed | 1 line manager, 2 Principal when needed. |
| approver_role | ENUM | — | Y | T1 | Assumed | LINE_MANAGER, PRINCIPAL, HR. |
| approver_user_id / acting_for_user_id | UUID | FK:User | Y / N | T1 | Assumed; delegation Confirmed (DL-36) | Delegate approvals record whom they acted for (A16). |
| decision / decided_at / comment | ENUM / TIMESTAMPTZ / TEXT | — | C | T1 | Assumed | APPROVED, REJECTED. A rejection needs a comment. |

### HR08 — Compliance Item Type

**Purpose:** A kind of check, document or qualification staff may need.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| item_type_id | UUID | PK | Y | T1 | Confirmed (DL-157) | |
| code / name | VARCHAR | UK | Y | T1 | Assumed | GSG catalogue; brand-country adds local types. |
| item_category | ENUM | — | Y | T1 | Assumed (DL-27) | WORK_PASS, BACKGROUND_CHECK, SAFEGUARDING_TRAINING, TEACHING_QUALIFICATION, BOARD_TRAINING, TEACHER_REGISTRATION, FIRST_AID, MEDICAL_CHECK, OTHER. |
| result_tier | ENUM | — | Y | T1 | Confirmed (DL-151) | T2 or T3. WORK_PASS, BACKGROUND_CHECK, MEDICAL_CHECK are T3. |
| blocking | BOOLEAN | — | Y | T1 | Confirmed (DL-158) | Missing or expired → Not cleared. Default TRUE for WORK_PASS, BACKGROUND_CHECK, SAFEGUARDING_TRAINING. |
| satisfied_by_training | BOOLEAN | — | Y | T1 | Assumed (DL-162) | A matching HR18 record updates it. |

### HR09 — Compliance Requirement

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| requirement_id | UUID | PK | Y | T1 | Confirmed (DL-157) | |
| brand_country_id | UUID | FK:OR03 | Y | T1 | Assumed | No group default (DL-59). |
| item_type_id | UUID | FK:HR08 | Y | T1 | Assumed | |
| applies_to | JSON | — | Y | T1 | Assumed | Employee categories, positions or job families; nationality rule for WORK_PASS ("non-citizens"). |
| due | ENUM | — | Y | T1 | Assumed | BEFORE_START, WITHIN_DAYS. |
| due_within_days | INT | — | C | T1 | Open (OQ-171, OQ-172) | |
| renew_every_months | INT | — | N | T1 | Open (OQ-171, OQ-172) | Null = never expires. |
| requirement_status | ENUM | — | Y | T1 | Assumed | ACTIVE, RETIRED. |

### HR10 — Compliance Record

**Purpose:** One employee's item: a work pass, a check, a certificate.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| compliance_record_id | UUID | PK | Y | T1 | Confirmed (DL-157) | |
| employee_id | UUID | FK:E01 | Y | T1 | Assumed | |
| item_type_id | UUID | FK:HR08 | Y | T1 | Assumed | |
| reference_number | VARCHAR(64) | — | N | per HR08 `result_tier` | Confirmed (DL-151) | Pass number, check reference. |
| issuer | VARCHAR(160) | — | N | T2 | Assumed | "Ministry of Manpower", "IBO", university. |
| issued_on / expires_on | DATE | — | N | T1 | Assumed | Dates are T1 so the status can show. |
| result | ENUM | — | C | per HR08 `result_tier` | Assumed | CLEAR, NOT_CLEAR, PENDING. For checks only. |
| document_ref | VARCHAR(512) | — | N | per HR08 `result_tier` | Assumed | |
| verified_by_user_id / verified_at | UUID / TIMESTAMPTZ | — | C | T1 | Assumed | HR verifies an upload before it counts. |
| source_training_id | UUID | FK:HR18 | N | T1 | Assumed (DL-162) | |
| record_status | ENUM | — | Y | T1 | Confirmed (DL-158) | SUBMITTED, VALID, EXPIRING, EXPIRED, REJECTED, WAIVED. WAIVED needs the Principal and a reason. |

### HR11 — Vacancy

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| vacancy_id | UUID | PK | Y | T1 | Confirmed (DL-159) | |
| campus_id / org_unit_id / position_id | UUID | FK | Y | T1 | Assumed | |
| fte / employment_type | DECIMAL / ENUM | — | Y | T1 | Assumed | |
| reason | ENUM | — | Y | T1 | Assumed | NEW_HEADCOUNT, REPLACEMENT (links the leaving employment), COVER_FOR_LEAVE. |
| replaces_employment_id | UUID | FK:E02 | C | T1 | Assumed | |
| target_start | DATE | — | Y | T1 | Assumed | Teaching posts usually start with a term (AC08). |
| requested_by_user_id | UUID | FK:User | Y | T1 | Assumed | Line manager or Principal. |
| hiring_manager_employee_id | UUID | FK:E01 | Y | T1 | Assumed | |
| vacancy_status | ENUM | — | Y | T1 | Assumed (DL-27) | DRAFT, PENDING_APPROVAL, OPEN, ON_HOLD, FILLED, CANCELLED. |

### HR12 — Candidate Application

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| application_id | UUID | PK | Y | T2 | Confirmed (DL-159) | |
| vacancy_id | UUID | FK:HR11 | Y | T2 | Assumed | |
| person_id | UUID | FK:Person | Y | T2 | Assumed; matching Confirmed pattern (DL-58) | Matched through the review queue; an internal candidate keeps their `employee_id`. |
| source | ENUM | — | Y | T2 | Assumed | DIRECT, REFERRAL, AGENCY, INTERNAL, JOB_BOARD. |
| cv_ref | VARCHAR(512) | — | N | T2 | Assumed | |
| stage | ENUM | — | Y | T2 | Assumed (DL-27) | APPLIED, SCREENING, INTERVIEW, DEMO_LESSON, REFERENCES, OFFER, HIRED, REJECTED, WITHDRAWN. |
| rejection_reason | ENUM | — | C | T2 | Assumed | |
| retain_until | DATE | — | Y | T2 | Open (OQ-176) | |

### HR13 — Interview Feedback

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| feedback_id | UUID | PK | Y | T2 | Confirmed (DL-159) | |
| application_id | UUID | FK:HR12 | Y | T2 | Assumed | |
| interviewer_employee_id | UUID | FK:E01 | Y | T2 | Assumed | |
| stage | ENUM | — | Y | T2 | Assumed | INTERVIEW, DEMO_LESSON. |
| scores | JSON | — | Y | T2 | Assumed | Per criterion of the vacancy's scorecard (brand-set). |
| recommendation | ENUM | — | Y | T2 | Assumed | STRONG_YES, YES, NO, STRONG_NO. |
| submitted_at | TIMESTAMPTZ | — | Y | T2 | Confirmed (DL-159) | An interviewer sees others' feedback only after submitting their own. |

### HR14 — Offer

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| offer_id | UUID | PK | Y | T2 | Confirmed (DL-159) | |
| application_id | UUID | FK:HR12 | Y | T2 | Assumed | |
| proposed_start / contract_type / fte | DATE / ENUM / DECIMAL | — | Y | T2 | Assumed | Copied into E02 and HR01 on acceptance. |
| offer_letter_ref | VARCHAR(512) | — | N | T2 | Assumed | Pay terms live in the letter and payroll, not in fields (DL-147). |
| approved_by_user_id | UUID | FK:User | C | T2 | Assumed | Principal; Country Director for new headcount (OQ-174). |
| offer_status | ENUM | — | Y | T2 | Assumed | DRAFT, PENDING_APPROVAL, SENT, ACCEPTED, DECLINED, WITHDRAWN. ACCEPTED creates E02 PENDING and the joining checklist. |

### HR15 — Checklist Task

**Purpose:** One task in a joining or leaving checklist.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| task_id | UUID | PK | Y | T1 | Assumed (DL-160) | |
| employment_id | UUID | FK:E02 | Y | T1 | Assumed | |
| checklist_kind | ENUM | — | Y | T1 | Assumed | JOINING, LEAVING, CAMPUS_TRANSFER. |
| task_code / title | VARCHAR | — | Y | T1 | Assumed | From the brand-country template: COMPLIANCE_ITEMS, CONTRACT_SIGNED, EMERGENCY_CONTACT, ACCOUNT_INVITE, ACCESS_REQUEST, INDUCTION, HANDOVER_TEACHING, HANDOVER_CLASS, HANDOVER_RESPONSIBILITIES, HANDOVER_CASES, RETURN_ASSETS, EXIT_INTERVIEW, ACCOUNT_DISABLE. |
| owner_role | ENUM | — | Y | T1 | Assumed | HR, LINE_MANAGER, COORDINATOR, IT, EMPLOYEE, SYSTEM. |
| due_date | DATE | — | Y | T1 | Assumed | Relative to start or last day. |
| linked_record | JSON | — | N | T1 | Assumed | The TE06, CL03, TE07, case or HR10 the task is about. |
| task_status | ENUM | — | Y | T1 | Assumed | OPEN, DONE, NOT_NEEDED, OVERDUE. SYSTEM tasks close themselves. |

### HR16 — Appraisal Cycle

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| cycle_id | UUID | PK | Y | T1 | Assumed (DL-161) | |
| brand_country_id | UUID | FK:OR03 | Y | T1 | Assumed | |
| academic_year_id | UUID | FK:AC07 | C | T1 | Assumed (DL-96) | Or a calendar year for non-teaching staff (OQ-177). |
| name | VARCHAR(80) | — | Y | T1 | Assumed | "Appraisal 2026-27, CBSE staff". |
| applies_to | JSON | — | Y | T1 | Assumed | Categories, campuses. |
| stage_dates | JSON | — | Y | T1 | Assumed | GOALS, MID_YEAR, SELF_REVIEW, MANAGER_REVIEW, MODERATION, SHARED. |
| rating_scale | JSON | — | Y | T1 | Open (OQ-178) | Brand-set; "Not set" until then. |
| cycle_status | ENUM | — | Y | T1 | Assumed | DRAFT, OPEN, CLOSED. |

### HR17 — Appraisal

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| appraisal_id | UUID | PK | Y | T2 | Assumed (DL-161) | |
| cycle_id / employee_id | UUID | UK | Y | T2 | Assumed | |
| reviewer_employee_id | UUID | FK:E01 | Y | T2 | Assumed | Appraisal reviewer at cycle open (E04 `appraisal_reviewer_assignment_id`, DL-150). |
| goals | JSON | — | Y | T2 | Assumed | 3–5 goals with measures; agreed by both. |
| observations | JSON | — | N | T2 | Assumed | Lesson observation notes (date, observer, group from TE04, notes). |
| self_review / manager_review | TEXT | — | C | T2 | Assumed | |
| rating | VARCHAR(24) | — | C | T2 | Open (OQ-178) | |
| moderated_by_user_id | UUID | FK:User | N | T2 | Assumed | Principal. |
| employee_acknowledged_at | TIMESTAMPTZ | — | N | T2 | Assumed | The employee can add a comment; acknowledging isn't agreeing. |
| stage | ENUM | — | Y | T2 | Assumed | GOALS, IN_YEAR, SELF_REVIEW, MANAGER_REVIEW, MODERATION, SHARED, CLOSED. |

### HR18 — Training Record

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| training_id | UUID | PK | Y | T1 | Assumed (DL-162) | |
| employee_id | UUID | FK:E01 | Y | T1 | Assumed | |
| title / provider | VARCHAR | — | Y | T1 | Assumed; Known example (HB p.138: IB training at workshops) | "IB Category 1: Language A", "IBO". |
| training_kind | ENUM | — | Y | T1 | Assumed | WORKSHOP, COURSE, CONFERENCE, IN_HOUSE, ONLINE. |
| item_type_id | UUID | FK:HR08 | N | T1 | Assumed (DL-162) | When it counts as a compliance item. |
| date_from / date_to / hours | DATE / DATE / DECIMAL | — | Y | T1 | Assumed | |
| certificate_ref | VARCHAR(512) | — | N | T2 | Assumed | |
| request_status | ENUM | — | Y | T1 | Assumed | REQUESTED, APPROVED, REJECTED, COMPLETED, RECORDED (added after the event). |
| approved_by_user_id | UUID | FK:User | C | T1 | Assumed | Line manager. Cost and budget Open (OQ-180). |

## A staff year and how it maps to other modules

| Moment | Staff & HR record | Other module | Rule |
|---|---|---|---|
| Vacancy approved | HR11 OPEN | — | SR-HR-12 |
| Offer accepted | HR14 ACCEPTED → E02 PENDING, HR01 DRAFT, HR15 joining tasks | Person review queue (DL-58) | SR-HR-13 |
| 7 days before start | Account invite task | A01 invite (DL-13) | SR-HR-14 |
| First day | E02 ACTIVE if Cleared | Access request case (Module Admin); Teaching may assign (TE06) | SR-HR-09, SR-HR-14 |
| Teacher assigned to a group | Compliance and qualification checked | TE06 warns or blocks | SR-HR-10 |
| Leave approved | HR06 APPROVED; HR05 booked | TE12 created (STAFF_LEAVE) | SR-HR-06 |
| Sick report at 07:30 | HR06 SAME_DAY_REPORT | TE12 created at once; cover board | SR-HR-06 |
| Work pass expires in 60 days | HR10 EXPIRING; E01 DUE_SOON | Notification to HR and the employee (DL-129) | SR-HR-08 |
| Check expired | E01 NOT_CLEARED; HR case | TE06 blocks new assignments; TE13 leaves them out | SR-HR-09 |
| Concern raised about a staff member | — | STAFF_CONCERN case, RESTRICTED (DL-125) | SR-HR-17 |
| Suspended | E02 SUSPENDED | Account suspended (DL-13); TE12 "Away" | SR-HR-17 |
| Appraisal cycle opens | HR17 per employee | Academic year (AC07) | SR-HR-15 |
| End date set | E02 shows Leaving; HR15 handover tasks | TE06, CL03, TE07, open cases listed | SR-HR-14 |
| Last day passes | E02 ENDED | Account disabled; exit review (BR-10) | SR-HR-14 |

## Field visibility by role

`M` manage, `A` approve, `V` view, `O` own record, `—` none. All cells Confirmed (DL-151) except where cited.

| Record | Employee (self) | Colleague (campus) | Line manager | Principal (campus) | HR (brand-country) | Coordinator | Group leadership |
|---|---|---|---|---|---|---|---|
| Directory: name, photo, position, department, campus, work contact (T1) | O | V | V | V | M | V | counts |
| Personal contact, address, DOB, nationality, emergency contacts (T2) | O edit contact (DL-151) | — | emergency contacts of team | V | M | — | — |
| Identity and work pass numbers, check results, medical certificates (T3) | O (own documents) | — | — | with A17 | M with A17 (DL-69) | — | — |
| Employment, contract terms | O | — | V (team, no documents) | V | M | — | counts |
| Leave balance and requests, with type | O apply | — | A (team) | A (over threshold) | M | — | days totals |
| "Away" today (no type) | — | V | V | V | V | V (feeds cover) | — |
| Compliance status chip | O | — | V (team) | V | M | V (when assigning, DL-158) | totals |
| Compliance details | O | — | — | V (T2) | M | — | — |
| Vacancies | — | — | request | A | M | — | counts |
| Candidates, feedback, offers (T2) | own application | — | hiring manager: own vacancy | A | M | interviewer: own feedback, then all | — |
| Joining and leaving checklists | own tasks | — | own tasks | V | M | handover tasks | — |
| Appraisal (T2) | O | — | appraisal reviewer: M (line manager sees nothing unless also reviewer) | A moderate | V | observer: own notes | completion % |
| Training records | O request | — | A | V | M | — | hours totals |

## Enumerations (candidates, DL-27 rule)

| Entity | Field | Values |
|---|---|---|
| E01 | compliance_status | CLEARED, DUE_SOON, ACTION_NEEDED, NOT_CLEARED |
| E01 | record_status | ACTIVE, INACTIVE, ARCHIVED |
| E02 | employee_category | TEACHING, TEACHING_SUPPORT, NON_TEACHING, LEADERSHIP, OTHER |
| E02 | employment_type | PERMANENT, FIXED_TERM, PART_TIME, CASUAL, OTHER |
| E02 | employment_status | PENDING, ACTIVE, SUSPENDED, ENDED (LEAVING derived: end date set) |
| E02 | leaving_reason | RESIGNED, CONTRACT_ENDED, RETIRED, DISMISSED, MUTUAL, TRANSFERRED_WITHIN_GSG, OTHER |
| E03 | unit_type | ACADEMIC_DEPARTMENT, FUNCTION, TEAM |
| E05 | job_family | TEACHING, LEADERSHIP, LEARNING_SUPPORT, ADMISSIONS, FINANCE, HR, IT, OPERATIONS, OTHER |
| E07 | assignment_reason | HOME, ADDITIONAL_TEACHING, SECONDMENT, TRANSFER |
| HR01 | term_status | DRAFT, SIGNED, ACTIVE, ENDED |
| HR03 | leave_category | ANNUAL, SICK, HOSPITALISATION, MATERNITY, PATERNITY, CHILDCARE, COMPASSIONATE, STUDY, UNPAID, DUTY, OTHER |
| HR04 | leave_year | CALENDAR, ACADEMIC, EMPLOYMENT_ANNIVERSARY |
| HR04 | school_day_rule | ANY, PRINCIPAL_APPROVAL, NOT_ALLOWED |
| HR06 | request_kind | PLANNED, SAME_DAY_REPORT, ENTERED_BY_HR |
| HR06 | request_status | DRAFT, PENDING, APPROVED, REJECTED, CANCELLED, CERTIFICATE_DUE |
| HR08 | item_category | WORK_PASS, BACKGROUND_CHECK, SAFEGUARDING_TRAINING, TEACHING_QUALIFICATION, BOARD_TRAINING, TEACHER_REGISTRATION, FIRST_AID, MEDICAL_CHECK, OTHER |
| HR10 | record_status | SUBMITTED, VALID, EXPIRING, EXPIRED, REJECTED, WAIVED |
| HR11 | vacancy_status | DRAFT, PENDING_APPROVAL, OPEN, ON_HOLD, FILLED, CANCELLED |
| HR12 | stage | APPLIED, SCREENING, INTERVIEW, DEMO_LESSON, REFERENCES, OFFER, HIRED, REJECTED, WITHDRAWN |
| HR14 | offer_status | DRAFT, PENDING_APPROVAL, SENT, ACCEPTED, DECLINED, WITHDRAWN |
| HR15 | task_status | OPEN, DONE, NOT_NEEDED, OVERDUE |
| HR17 | stage | GOALS, IN_YEAR, SELF_REVIEW, MANAGER_REVIEW, MODERATION, SHARED, CLOSED |
| HR18 | request_status | REQUESTED, APPROVED, REJECTED, COMPLETED, RECORDED |
| CC01 | case type (new) | STAFF_CONCERN (RESTRICTED), STAFF_COMPLIANCE (HR), STAFF_EXIT_REVIEW (= EXIT_REVIEW, DL-08) |

### Status chips (proposed, AP-14)

| List | Value → screen label → chip |
|---|---|
| Employment | PENDING → Starts {date} → info · ACTIVE → Active → success · Leaving (derived) → Leaves {date} → warning · SUSPENDED → Suspended → warning · ENDED → Left → default |
| Compliance status | CLEARED → Cleared → success · DUE_SOON → Due soon → warning · ACTION_NEEDED → Action needed → warning · NOT_CLEARED → Not cleared → error |
| Compliance item | SUBMITTED → To verify → info · VALID → Valid → success · EXPIRING → Expires {date} → warning · EXPIRED → Expired → error · WAIVED → Waived → default |
| Leave request | PENDING → Waiting for {approver} → info · APPROVED → Approved → success · REJECTED → Declined → error · CANCELLED → Cancelled → default · CERTIFICATE_DUE → Certificate due → warning |
| Vacancy | PENDING_APPROVAL → Waiting approval → info · OPEN → Open → success · ON_HOLD → On hold → warning · FILLED → Filled → default |
| Candidate stage | stage name → default; HIRED → Hired → success; REJECTED → Not progressed → default |
| Appraisal | stage name → info; overdue stage → Overdue → warning; CLOSED → Done → default |
| Checklist task | OPEN → (none) · OVERDUE → Overdue → error · DONE → Done → success |

## Business rules

- **SR-HR-01 (Confirmed, DL-147):** After a brand-country's go-live every staff field is edited in Campus OS only; no field shows a "From {system}" lock. A change payroll needs (joiner, legal name, contract dates, FTE, unpaid leave, end date) is sent through E08 PAYROLL_FEED; a failed send appears in HR's "Not sent to payroll" list until resolved. Before go-live, the brand-country's old HR system stays the record and Campus OS shows no staff editing for it.
- **SR-HR-02 (Confirmed, DL-148; Known BR-01):** Creating an employee matches the Person by ID documents or work email through the review queue (DL-58); a Person who is already a guardian keeps one Person and one login.
- **SR-HR-03 (Confirmed, DL-149; amends BR-06 for campus assignments):** An ACTIVE employment has exactly one primary campus unless its position's scope is above campus. Adding, ending or changing a campus assignment emits `CampusAssignmentChanged`; the Access Registry automatically grants the position's standard access template at the new campus from the move date and ends the old campus's position access on the end date (like DL-73 roster scope); every grant is logged, the Module Admin can remove it, and it never includes Tier 3 (A17 still needs its own approval, DL-69). The profile shows "Access from campus move" with the date so anyone can answer "Why can I see this?" (AR-09).
- **SR-HR-04 (Confirmed, DL-150):** Every ACTIVE employment has one primary E04 with a line manager and an appraisal reviewer (the same person unless HR sets otherwise), except the head of a brand-country. Neither chain may loop, and nobody reviews themselves. When a line manager or reviewer leaves, the people they managed or reviewed appear in HR's "No manager" or "No reviewer" list.
- **SR-HR-19 (Confirmed, DL-153):** Only GSG HR (capability `hr.leave_template.manage`) edits country templates. A brand-country adopts a template once as a copy; template changes never flow into adopted policies automatically. A brand-country policy can't be activated with `days_per_year` below the template's `statutory_min_days`.
- **SR-HR-05 (Assumed, DL-153, DL-154, DL-155):** A leave request computes `days_counted` from the work pattern, public holidays and the policy; it is blocked over `available_days` unless `allow_negative`; it routes to the line manager, then the Principal when longer than `principal_over_days` or when a teacher's leave falls on school days under PRINCIPAL_APPROVAL. Delegates approve with `acting_for` recorded (DL-36).
- **SR-HR-06 (Confirmed, DL-156; Confirmed DL-143):** On APPROVED (or SAME_DAY_REPORT) for an employee with lessons, Staff & HR creates TE12 with dates, periods and cover note, `absence_kind` LEAVE (DUTY for DUTY leave), `source` STAFF_LEAVE, and no type or note. Cancelling the request cancels TE12.
- **SR-HR-07 (Confirmed, DL-154):** A same-day sick report is accepted until a campus cut-off (OQ-168), becomes CERTIFICATE_DUE when the policy needs a certificate, and goes to HR's list if none arrives within `certificate_due_days`.
- **SR-HR-08 (Confirmed, DL-158):** Compliance status is recalculated daily: any blocking requirement missing or EXPIRED → NOT_CLEARED; any other requirement missing or EXPIRED → ACTION_NEEDED; any item expiring within the alert window → DUE_SOON; else CLEARED. Alerts to the employee and HR at 90, 60 and 30 days (campus setting).
- **SR-HR-09 (Confirmed, DL-158):** NOT_CLEARED: the employment cannot move from PENDING to ACTIVE; Teaching cannot confirm a new TE06 and leaves the teacher out of TE13 suggestions; an HR case (STAFF_COMPLIANCE) opens. Existing assignments stay; nothing suspends or ends employment automatically. A Principal may WAIVE an item with a reason (audited).
- **SR-HR-10 (Confirmed, DL-158):** When a coordinator assigns a teacher (TE06), a missing TEACHING_QUALIFICATION or BOARD_TRAINING required for that programme shows a warning, never a block (OQ-159).
- **SR-HR-11 (Confirmed, DL-152):** Fixed-term end dates and probation ends raise an HR task N days before (campus setting, "Not set" otherwise).
- **SR-HR-12 (Confirmed, DL-159):** A vacancy opens only after approval; REPLACEMENT vacancies link the leaving employment; new headcount needs the Country Director (OQ-174).
- **SR-HR-13 (Confirmed, DL-159):** An interviewer sees other feedback only after submitting their own. An ACCEPTED offer creates E02 PENDING, HR01 DRAFT and the joining checklist; the vacancy becomes FILLED.
- **SR-HR-14 (Assumed, DL-160; Assumed DL-13; Known BR-10):** Joining: account invite 7 days before start; access request case on start. Leaving: end date set → HR15 handover tasks listing every TE06, CL03, TE07, open case and active delegation of the employee; last day passed → E02 ENDED, account disabled, exit review case.
- **SR-HR-15 (Assumed, DL-161; reviewer Confirmed DL-150):** Each appraisal's reviewer is the employment's appraisal reviewer when the cycle opens; a change of manager mid-cycle moves it with the history kept. Ratings are hidden from the employee until SHARED.
- **SR-HR-16 (Assumed, DL-162):** A COMPLETED or RECORDED training with `item_type_id` creates or renews the matching HR10 once HR verifies the certificate.
- **SR-HR-17 (Assumed, DL-163; Known HB p.210–213):** A STAFF_CONCERN case is RESTRICTED (DL-125): only the Designated Person for Child Protection, the Principal and named HR open it; the staff member named never sees it. Suspension sets E02 SUSPENDED with no reason outside the case, suspends the account (DL-13) and creates TE12 "Away".
- **SR-HR-18 (Assumed, DL-164):** HR records live in the data location of the employing entity's country; group dashboards show counts only (AR-01).

## Events

EmployeeCreated, EmploymentStarted, EmploymentEnded, EmploymentSuspended, CampusAssignmentChanged (Registry applies or ends position access template, DL-149), LineManagerChanged (leave and appraisal routing), LeaveRequested, LeaveApproved, LeaveCancelled (Teaching TE12), ComplianceStatusChanged (Teaching assignment check and cover suggestions), ComplianceItemExpiring (Notifications), VacancyOpened, OfferAccepted (Person review, joining checklist), LeavingDateSet (handover tasks, Registry exit review), AppraisalShared, TrainingCompleted. Events carry IDs only (architecture.md); no leave type, check result or reason on any event.

## Capabilities (DL-53 naming)

Existing seeds: `hr.employee.view`, `hr.employee.create`, `hr.employee.update`, `hr.employment.end`, `hr.campus_assignment.manage`, `hr.confidential.view`. New: `hr.restricted.view` (T3, with A17) · `hr.self.view`, `hr.self.update` (own record) · `hr.org.manage` (departments, positions) · `hr.contract.manage` · `hr.leave.request` · `hr.leave.approve` · `hr.leave.manage` (adjust, enter for others) · `hr.leave_policy.manage`, `hr.leave_template.manage` (GSG HR, DL-153) · `hr.compliance.view` (status) · `hr.compliance.manage` · `hr.compliance.waive` · `hr.compliance_rule.manage` · `hr.vacancy.request` · `hr.vacancy.approve` · `hr.recruitment.manage` · `hr.interview.feedback` · `hr.offer.approve` · `hr.checklist.manage` · `hr.appraisal.manage` (cycles) · `hr.appraisal.review` · `hr.appraisal.moderate` · `hr.training.request` · `hr.training.approve`.

## Decisions (DL-146…DL-164)

| ID | Decision | Why |
|---|---|---|
| DL-146 (**Confirmed**, Henry 25 Nov 2026, decision card "Full HR") | Staff & HR is one module (`staff_hr`, Staff and HR domain, prefix `hr`): staff records and contracts, departments and positions, staff leave, compliance, recruitment, joining and leaving, appraisal and training. It is the home of Employee DD E01–E05, E07, E08 (IDs kept; E06 is OR04). Payroll stays outside | Arch module 12; Teaching's absence (DL-143) waits for leave; OBJ-05/06 still pointed at a Proposed source |
| DL-147 (**Confirmed**, Henry 25 Nov 2026, decision card "Campus OS owns everything") | Campus OS owns the whole staff record from each brand-country's go-live; existing HR systems stop editing staff data and are imported once at cutover; payroll receives the changes it needs from Campus OS (E08 feed, or HR re-keys); pay, bank and tax never enter Campus OS | One writer, simplest screens (answers Employee DD D01); chosen over keeping payroll-owned fields read-only |
| DL-148 (**Confirmed**, Henry 26 Nov 2026, decision card "As proposed") | An employee is the Employee profile of a global Person (DL-42); `employee_id` never changes; an employment per legal employer with its own employee number and `operating_entity_id`; concurrent employments allowed | Employee DD D02, D03; DL-41, DL-42 |
| DL-149 (**Confirmed**, Henry 27 Nov 2026, decision card "Automatic through the Registry") | One primary campus per active employment (none for group, brand and country office staff); more campuses allowed; a campus change makes the Access Registry grant the position's standard access at the new campus (and end it at the old), logged, removable, never Tier 3 | Employee DD D04, D07; amends BR-06 for campus assignments; chosen over an access review step |
| DL-150 (**Confirmed**, Henry 27 Nov 2026, decision card "Two managers") | Departments per brand-country or campus; positions from a GSG job-family list, named by each brand; two managers per employment: line manager (leave) and appraisal reviewer (appraisal), the same person by default; HR position ≠ access role ≠ academic responsibility, except the DL-149 template | Employee DD boundaries; TE07 (DL-135); chosen over one line manager |
| DL-151 (**Confirmed**, Henry 27 Nov 2026, decision card "As proposed") | Directory (T1) visible to colleagues at the campus; line manager sees team leave and compliance status, appraisal reviewer sees the appraisals they run; HR sees T2; T3 (identity, work pass, check results, medical certificates) needs an HR capability plus A17; staff edit their own contact details and emergency contacts directly, legal and identity changes need HR to verify | DL-11, DL-34, DL-69; Employee DD D13 |
| DL-152 (**Confirmed**, Henry 27 Nov 2026, decision card "As proposed") | Contract terms are effective-dated (type, FTE, hours, probation, notice, work pattern); a change is a new term; alerts before fixed-term and probation ends (campus setting); no salary | DL-24; DL-59 |
| DL-153 (**Confirmed**, Henry 27 Nov 2026, decision card "GSG template") | GSG keeps a leave template per country (types, policies per employee category, statutory minimums); each brand-country adopts it once as a copy and adjusts it; never below the statutory minimum; template updates are offered, never applied automatically; policies versioned | Entitlements differ by country; chosen over brand-country-only policies with no group default |
| DL-154 (**Confirmed**, Henry 27 Nov 2026, decision card "As proposed") | Staff apply in the staff app (days, half days or periods); line manager approves; the Principal also approves long leave and teachers' leave on school days where the policy says so; delegates approve when the approver is away; sick leave can be reported the same morning | Arch Principal approvals; DL-36 delegation |
| DL-155 (**Confirmed**, Henry 27 Nov 2026, decision card "As proposed") | Balances are calculated (entitlement pro-rated + carried + adjustments − taken − booked), never typed; HR adjustments need a reason | Audit; no spreadsheet balances |
| DL-156 (**Confirmed**, Henry 27 Nov 2026, decision card "As proposed") | Approved leave or a sick report creates the Teaching absence (TE12, STAFF_LEAVE) with dates and periods only; the leave type stays with the employee, line manager, Principal and HR; colleagues see "Away" | DL-143; answers the "feed automatically" part of OQ-163 |
| DL-157 (**Confirmed**, Henry 27 Nov 2026, decision card "As proposed") | A GSG catalogue of compliance item types and requirements per brand-country × position or category (before start or within N days, renewal) | Child protection (HB p.210–213); IB training; work passes for non-citizens |
| DL-158 (**Confirmed**, Henry 27 Nov 2026, decision card "As proposed") | One compliance status per employee (Cleared, Due soon, Action needed, Not cleared); Not cleared blocks starting, new teaching assignments and cover suggestions and opens an HR case, never suspends or ends employment by itself; missing board qualification only warns; alerts at 90/60/30 days | Safety without silent HR actions; partly answers OQ-159 |
| DL-159 (**Confirmed**, Henry 27 Nov 2026, decision card "As proposed") | Recruitment: approved vacancy → candidates matched to Persons (DL-58) → stages with independent interview feedback → approved offer → accepted offer creates the employment as PENDING with a joining checklist | One pipeline from request to first day |
| DL-160 | Joining and leaving checklists per brand-country × category; leaving lists every assignment, class, responsibility, open case and delegation to hand over; exit review and account disable after the last day | BR-10; DL-13 |
| DL-161 | Appraisal: an annual cycle per brand-country following the academic year, with goals, mid-year check-in, self-review, manager review, Principal moderation; rating scales per brand; Tier 2; not linked to pay in Campus OS | Arch module 12 "appraisal"; DL-96 |
| DL-162 | Training records (course, provider, hours, certificate) can clear a matching compliance item; staff request, line manager approves; hours targets a brand-country setting | Arch module 12 "training"; HB p.138 IB training at workshops |
| DL-163 | A concern about a staff member is a RESTRICTED STAFF_CONCERN case (Designated Person, Principal, named HR); suspension shows no reason outside the case, suspends the account and makes the teacher "Away" | HB p.210–213; DL-120, DL-125; DL-13 |
| DL-164 | HR records live in the data location of the employing entity's country; two countries mean two profiles under one Person; group views counts only | DL-39, DL-42, DL-47 |

## Open questions (new)

| ID | Question | Why it matters for design |
|---|---|---|
| OQ-166 | Which HR and payroll systems does each brand-country use today, when can each stop editing staff data (go-live date), and can each payroll system accept a feed from Campus OS? | Cutover plan; payroll feed (SR-HR-01, DL-147) |
| OQ-167 | Do staff hold contracts with two GSG legal entities at once (for example teaching at GIIS and OWIS), and does group or country office staff have a home campus? | Concurrent employments; primary campus |
| OQ-168 | How and by when do GIIS staff report sick today, and who is told first (line manager, coordinator, HR)? | Same-day report cut-off; cover timing (with OQ-163) |
| OQ-169 | What are the leave types and entitlements at GIIS SG, GIIS India and OWIS SG (by category and service), the leave year, and the carry-forward rule? | Leave policies; every balance |
| OQ-170 | Can teaching staff take annual leave on school days at all, and do school vacations count as leave for teachers? | School-day rule; days counted |
| OQ-171 | Which checks does Singapore require for school staff (police or background check, CPE teacher requirements, child-protection training), how often, and for which roles? | GIIS SG requirements |
| OQ-172 | Which checks and registrations does India require (police verification, CBSE affiliation norms for teacher qualifications), and how are they renewed? | GIIS India requirements |
| OQ-173 | Who in HR may see work pass and background check results, and who approves their Tier 3 type grant? | A17 holders; T3 screens |
| OQ-174 | Who approves a vacancy and an offer (Principal, Country Director, Finance Controller), and does replacing a leaver need approval? | Approval steps |
| OQ-175 | Does GIIS run demo lessons, reference checks or panels for teacher hiring, and from how many interviewers? | Candidate stages; feedback |
| OQ-176 | How long may unsuccessful candidates' data be kept per country, and may they be contacted for future roles? | Retention; consent |
| OQ-177 | When does the appraisal cycle run for teachers and for non-teaching staff, and who is a teacher's usual appraisal reviewer (HOD, coordinator, Vice Principal)? | Cycle dates; reviewer |
| OQ-178 | What rating scale and criteria does each brand use, and are lesson observations part of appraisal? | Rating field; observation notes |
| OQ-179 | Is there a CPD hours target per year, and which trainings are mandatory (IB category workshops, child protection, first aid)? | Training targets; compliance links |
| OQ-180 | Do training requests need a budget or cost approval, and who holds the training budget? | Training approval step |
| OQ-181 | What happens today when a staff member leaves mid-year: notice periods, handover owner, exit interview? | Leaving checklist; notice |
| OQ-182 | Who is the line manager for teachers at GIIS (level coordinator, HOD, Vice Principal), and for office staff? | Leave and appraisal routing |
| OQ-183 | Are there staff positions shared across campuses (one HR team for SG-PGL and SG-ECS), and which department structure does each campus use? | E03 scope; directory |
| OQ-184 | Are staff concerns that aren't child protection (performance, grievances) handled in the same restricted case, or separately? | Case types |
| OQ-185 | How long are staff records kept after leaving, per country, and what is deleted or anonymised? | Retention (with OQ-43) |
| OQ-186 | Do staff use relief or casual teachers, and are they employees in Campus OS? | Casual employment; cover (with OQ-164) |
| OQ-187 | Do staff children get admission priority or fee benefits that HR must confirm (DL-61 waitlist "staff children")? | Link to Admissions and Fees |
| OQ-188 | Which staff self-service changes need HR approval (bank details are out of scope), and does HR need to see an audit of them? | Self-service rules |
| OQ-189 | Do OWIS and other brands use different leave, appraisal or compliance processes from GIIS? | Brand variation; OWIS shows "Not set" |
| OQ-190 | Is the Designated Person for Child Protection always the Principal at every campus, and who is the backup? | STAFF_CONCERN case access |
| OQ-191 | Do Indian campuses need statutory HR registers (attendance register for staff, PF, gratuity) inside Campus OS? | India scope; payroll boundary |
| OQ-192 | Which access does each position's standard template give at a campus (for example Teacher, Head of Department, HR Business Partner, Campus Admin), and who maintains the templates? | Campus-move access (DL-149, SR-HR-03) |
| OQ-193 | Who in GSG maintains the country leave templates and statutory minimums (group HR, legal, each country lead), and how is a law change checked? | Leave templates (DL-153, SR-HR-19) |

## Readiness

- **Can be designed now:** staff directory with compliance chips; staff profile (overview, employment and contract, leave, compliance, training, appraisal, documents with Tier 3 locks); leave apply and balance (self, phone and web); leave approvals inbox with team calendar and "creates cover" note; compliance dashboard (expiring in 90 days, not cleared, to verify); recruitment board per vacancy (stages, feedback, offer); joining and leaving checklists with handover list; appraisal cycle overview and one appraisal; training log.
- **Keep generic, label Pending:** go-live and payroll feeds per brand-country (OQ-166), entitlements (OQ-169, OQ-170), required checks (OQ-171, OQ-172), approval chains (OQ-174, OQ-182), rating scale (OQ-178), retention (OQ-176, OQ-185), India statutory registers (OQ-191).

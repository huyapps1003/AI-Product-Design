# Global Campus OS — Assessment & Report Cards Data Dictionary v0.1 (draft)

**Status:** Proposed draft, 15 Nov 2026. DL-87…DL-95 Confirmed (Henry, 15 Nov 2026; DL-88…DL-95 "All A"); OQ-93…OQ-106 Open · **Owner:** Henry
**Cites:** [Architecture v1](../00-sources/Global_Campus_OS_Product_Architecture_v1.md) § Principles (programme packs, policy as rules), § Context (assessment rhythm, grading, promotion; MyGIIS "Exam and Results"), § Users and roles (Principal, Academic Supervisor, Programme or level coordinator, Subject teacher), § Domains and modules (module 4 Assessment and Reporting), § Key workflows (Assessment to report card), § Tenancy (programme offering owns grading scale and promotion rule versions; effective dating), § Data model (Assessment row), § Integrations (IB and Cambridge), § Security (audit of marks changes), § Phases (Phase 2 gate "report cards issued from the OS") · [architecture.md](./architecture.md) (Assessment domain) · [Classes dictionary](./classes-data-dictionary.md) CL01, CL06, CL07 · [Fees & billing dictionary](./fees-billing-data-dictionary.md) fee status (DL-83) · [Student Directory Spec](./student-directory-data-dictionary.md) ST02, ST03 · [Employee Data Dictionary v1](../00-sources/Global_Campus_OS_Employee_Data_Dictionary_v1.md) D01–D03 · GIIS Student Handbook 2025-26 (HB) p.34 (MyGIIS), p.43 (absence and proration), p.65 (KG Holistic Progress Card), p.66–79 (CBSE, CLSP, IGCSE, IBDP schemes and grade scales), p.85 (co-scholastic), p.86–88 (PYP reports, return of scripts and appeals), p.127 (appeal policy), p.128–131 (promotion), p.132–133 (predicted grades) · GIIS SG Exam Schedule 2026-27 · DL-11, DL-16, DL-22, DL-26, DL-27, DL-30, DL-32…DL-38, DL-47, DL-53, DL-54, DL-59, DL-66, DL-68, DL-70, DL-73, DL-75, DL-83 · MC-06, MC-07 · OQ-79
**Tóm tắt (VI):** Từ điển dữ liệu cho module Đánh giá & Học bạ: khung đánh giá (thành phần, trọng số, thang điểm, quy tắc lên lớp) lấy từ mẫu theo chương trình (CBSE, IB, Cambridge…) rồi mỗi brand-country điều chỉnh và Academic Supervisor duyệt từng phiên bản; lịch các bài kiểm tra; nhập điểm, kiểm duyệt và khoá điểm; quy đổi điểm sang xếp loại; học bạ phát hành cho phụ huynh; phúc khảo; kết quả lên lớp chuyển sang Lớp học; điểm dự đoán (IB, CBSE 12). Mỗi trường có trạng thái Known / Assumed / Open.

> Drafted by Claude from product truth on 15 Nov 2026 after Henry approved the scope the same day and chose option A: GSG ships a template per board, each brand-country adapts it, and the Academic Supervisor approves each version (DL-87).

## How to read this file

Same layout and rules as the [Student Directory Spec](./student-directory-data-dictionary.md#how-to-read-this-file): **Known** (cited), **Assumed** (DL), **Open** (OQ); tiers T1/T2/T3 (DL-11; students' academic data is Tier 2); `Y`/`N`/`C` required. Every row also carries `tenant_path`, `data_region` (UI term **Data location**, DL-47) and the shared common fields. Facts from the GIIS Handbook are **GIIS Singapore examples**, not group rules (DL-22). All enums are unapproved candidates (DL-27 rule). All example values are synthetic.

## Scope

Approved by Henry, 15 Nov 2026: **how students are assessed, how marks become grades and report cards, and what result decides promotion.**

| In this dictionary | Not in this dictionary (owner) |
|---|---|
| Scheme templates per board or programme pack (GSG platform team) | Exam calendar engine, holiday and clash checks (Calendar; this file reads exam windows) |
| Assessment schemes per brand-country × programme offering × grade band × year: reporting periods, components and weights, grade scale, promotion rule; versioned (DL-87) | Invigilation, seating plans, exam halls |
| Assessments: the dated tests per grade and subject (PT1, Half Yearly, CT1, Sem 1…) | Question paper, answer key and vetting templates (Teaching) |
| Mark sheets per assessment and class: entry, submission, moderation, lock | Online exam portal (integration, OQ-101) |
| Absence, exemption and prorated marks (HB p.43) | Board exam registration, IB and CAIE submissions and exports (Integrations, later phase) |
| Subject results and grades, including CBSE co-scholastic and KG/PYP descriptors | Letters of recommendation, transcripts for universities (Careers and Universities) |
| Report cards: generate, release, parent download, reissue (MyGIIS "Exam & Results" parity) | Attendance marks and leave (Attendance and Conduct; this file reads the attendance %) |
| Appeals and re-evaluation of school results | Academic honesty and malpractice cases (Attendance and Conduct, Case) |
| Promotion decisions per student and re-tests; handed to the Classes rollover (DL-75) | Awards (GSA), house points (Activities and Awards) |
| Predicted grades (IB DP, CBSE 12) and their appeal committee | Timetable, subject-teacher allocation (Teaching, OBJ-12) |

## Modeling boundaries

- **Rules come from a pack template that each brand-country adapts (DL-87, Confirmed).** The GSG platform team keeps one **scheme template** per board or programme pack (CBSE 6-8, IGCSE 9-10, IB DP…), built from the board's rules. A brand-country copies a template into its own **assessment scheme** and adjusts weights, grade bands, re-test rules and report card layout. Each version is approved by an Academic Supervisor before use (who approves for a whole brand-country: OQ-93). An approved version never changes; a change is a new version with a valid-from year, so 2025-26 report cards keep 2025-26 rules (Arch § Tenancy, effective dating).
- **A scheme belongs to a programme offering and grade band.** Example: GIIS Singapore "CBSE 6-8" for Grades 6–8: Term 1 = Periodic Assessment 20 (PT1 10 + Notebook 5 + Subject Enrichment 5) + Half Yearly 80; Term 2 the same with PT2 and Yearly (HB p.67–68). A campus that runs the offering uses its brand-country's scheme; campus-only variations are not modelled (OQ-93).
- **Assessments are dated, marks belong to them (DL-88).** An assessment is one component on one date for one grade and subject (Grade 7 PT2 Mathematics, 17 Nov 2026). It sits inside an exam window from the campus calendar. Marks are entered per class on a **mark sheet** that moves Open → Submitted → Moderated → Locked. After lock a mark changes only through an appeal or a correction with a reason; every change is audited (Arch § Security).
- **Grades are calculated, never typed (DL-89).** The scheme version converts marks to weighted percentages and grades (CBSE eighths A1–E, IB 1–7 by subject category, IGCSE scale). To change a grade, correct the mark. Descriptor scales (KG Holistic Progress Card: Beginner…Advanced; PYP: Exemplary…; CBSE co-scholastic A–E) are entered as descriptors, not marks.
- **A report card is a released snapshot (DL-90).** A report card is generated per student per reporting period from locked results, checked by the class teacher and coordinator, and released by the coordinator for a whole grade. Once released it never changes; a corrected result produces a reissued version and a notice to parents. Report cards are never held back for fees: fee status is information only (DL-83, Confirmed).
- **Appeals have one round, with deadlines set per brand-country (DL-91).** A parent raises an appeal on a released result within the window (GIIS SG: 2 working days after scripts are returned for CBSE 1–8, HB p.88; at least 7 working days from results for other grades, HB p.127; the conflict is OQ-97). The subject teacher re-marks through the HOD, the Examination Board endorses, the coordinator sends the outcome. No second appeal. Board re-evaluations (CBSE, IB, CAIE) happen outside and are recorded as an external outcome.
- **Assessment proposes promotion, the Principal approves it in the rollover (DL-92).** At year end the promotion rule turns results and the attendance % into a proposed outcome per student: promoted, weak pass, conditional (re-test), detained, or board criteria for Grades 10 and 12 (HB p.128–131). The coordinator reviews; outcomes that need a judgment wait for a decision. Confirmed outcomes are handed to the Classes rollover (CL07), where the Principal approves the run (DL-75, Confirmed). A re-test is an assessment of type RETEST linked to the decision.
- **Teachers see their own teaching, parents see only released results (DL-93).** Subject teachers enter and see marks for the subjects and classes they teach (Teaching allocation, OBJ-12). The class teacher sees all subjects for their own class (scope from DL-73). Parents see an assessment's marks once the coordinator publishes them after lock (MyGIIS shows test marks online, HB p.34), and report cards once released, for their own children only. Students see their own released report cards only where the campus turns this on for their grade (extends DL-68; OQ-96).
- **Predicted grades are teacher judgments with a committee appeal (DL-94).** Subject teachers enter predicted grades for IB DP and CBSE 12; the coordinator releases them (GIIS SG: first week of October, HB p.133). An appeal goes to a committee headed by the Principal, recorded on a Case.
- **Absence is a code, never a medical detail (DL-95).** A missing mark carries a code: ABSENT, APPROVED_ABSENCE, EXEMPT or NOT_ASSESSED. A prorated mark for an approved absence needs the Principal's approval (HB p.43). The reason itself stays in Attendance or Wellbeing (DL-66 pattern).
- **Rules are settings with no group default (DL-59 pattern).** Appeal windows, release deadlines, attendance thresholds for promotion, whether students see their report card and whether K–5 retention needs a parent request are brand-country or campus settings. A value that isn't set shows "Not set" and blocks the step that needs it.
- **Data location.** Schemes are held with the brand-country; assessments, marks, report cards, appeals and decisions are held in the campus's data location (DL-47). Templates are global and hold no student data.

## Entities

| ID | Entity | Module | Status |
|---|---|---|---|
| AS01 | Scheme Template | Assessment (GSG platform team) | Known need (Arch § Principles "Programme packs… assessment types, grading scale, promotion rules and report card templates"); owner Confirmed (DL-87) |
| AS02 | Assessment Scheme | Assessment | Known (Arch § Tenancy: offering owns grading scale and promotion rule versions); model Confirmed (DL-87) |
| AS03 | Scheme Component | Assessment | Known values for GIIS SG (HB p.66–79 weightages) |
| AS04 | Grade Scale | Assessment | Known (Arch § Data model "Grade scale"; HB p.67–85) |
| AS05 | Promotion Rule | Assessment (Rules service) | Known for GIIS SG (HB p.128–131); model Confirmed (DL-92) |
| AS06 | Assessment | Assessment (reads Calendar exam windows) | Known (Arch § Data model "Assessment"; Exam Schedule 2026-27) |
| AS07 | Mark Sheet | Assessment | Known need (Arch § Key workflows "moderation and lock"; HB "answer script moderation form"); model Confirmed (DL-88) |
| AS08 | Mark | Assessment | Known (Arch § Data model "Mark") |
| AS09 | Subject Result | Assessment | Known (Arch § Data model "Grade"); calculated (DL-89) |
| AS10 | Report Card | Assessment | Known (Arch § Data model "Report card"; HB p.34, p.67–79 release counts) |
| AS11 | Appeal | Assessment | Known (Arch § Data model "Appeal"; HB p.88, p.127) |
| AS12 | Promotion Decision | Assessment (hands outcome to Classes CL07) | Known outcomes (HB p.128–131); model Confirmed (DL-92) |
| AS13 | Predicted Grade | Assessment | Known (Arch § Data model "Predicted grade"; HB p.132–133) |

ST02, ST03, CL01, CL07, OBJ-12 teaching allocation, the Case and the calendar's exam window are referenced, not redefined. Academic year, term, programme offering, grade and subject still need the Academic structure dictionary.

## Detailed attribute dictionary

### AS01 — Scheme Template

**Purpose:** The starting rules for one board or programme pack, kept by the GSG platform team. Holds no student data.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| template_id | UUID | PK | Y | T1 | Confirmed (DL-87) | |
| programme_pack_code | VARCHAR(32) | — | Y | T1 | Known packs (Arch § Principles: CBSE, IB PYP, IB DP, Cambridge CLSP, IGCSE, Global Montessori Plus) | |
| grade_band_id | UUID | FK:AC04; UK with pack + version | Y | T1 | Known bands (HB annexes: CBSE 3-5, 6-8, 9-10, 11-12; IB Year 1–2…); key Confirmed (DL-115) | Was free text `grade_band` before the 28 Nov 2026 review; AC04 is the band list. |
| template_version | VARCHAR(16) | UK | Y | T1 | Assumed | e.g. "2026.1". New board rules = new version. |
| reporting_periods | JSON | — | Y | T1 | Known shapes (HB: Term 1/Term 2; Semester 1/2; "after every assessment" for CBSE 3-5) | Default periods with names. |
| default_components | JSON | — | Y | T1 | Known (HB weightages) | Copied into AS03 on adoption. |
| default_grade_scale_id | UUID | FK:AS04 | Y | T1 | Known | |
| default_promotion_rule | JSON | — | N | T1 | Known board criteria (e.g. "Board promotion criteria" for Grades 10 and 12, HB p.131) | |
| report_card_layout_code | VARCHAR(32) | — | Y | T1 | Open (layouts per brand, OQ-94) | |
| template_status | ENUM | — | Y | T1 | Assumed (DL-27) | DRAFT, PUBLISHED, RETIRED. |
| source_note | TEXT | — | Y | T1 | Assumed | Board circular or document the rules came from. |

### AS02 — Assessment Scheme

**Purpose:** The rules one brand-country uses for one programme offering and grade band in an academic year, copied from a template and adjusted.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| scheme_id | UUID | PK | Y | T1 | Confirmed (DL-87) | |
| scope_node_id | UUID | FK:OR01 | Y | T1 | Confirmed: brand-country (DL-87); campus-only schemes Open (OQ-93) | |
| programme_offering_id | UUID | FK:AC06 | Y | T1 | Known (Arch § Tenancy) | |
| grade_band_id | UUID | FK:AC04 | Y | T1 | Known; key Confirmed (DL-115) | e.g. "Grades 6-8". Was free text `grade_band` before the 28 Nov 2026 review. |
| template_id / template_version | UUID / VARCHAR | FK:AS01 | Y | T1 | Confirmed (DL-87) | Which template it was copied from. Shows "Differs from template" where changed. |
| scheme_version | INT | UK with scope + offering + band | Y | T1 | Known need (Arch § Tenancy effective dating) | |
| valid_from_academic_year_id | UUID | FK:AC07 | Y | T1 | Known (Arch: "2025-26 report cards keep using 2025-26 rules") | |
| scheme_status | ENUM | — | Y | T1 | Assumed (DL-27) | DRAFT, SUBMITTED, APPROVED, SUPERSEDED. |
| prepared_by_user_id | UUID | FK:A01 | Y | T1 | Assumed (coordinator) | |
| approved_by_user_id / approved_at | — | — | C | T1 | Confirmed: Academic Supervisor (DL-87); for brand-country scope OQ-93 | Required before any assessment uses the version. |
| change_summary | TEXT | — | C | T1 | Assumed | Required for version 2+. |
| report_card_layout_code | VARCHAR(32) | — | Y | T1 | Open (OQ-94) | |
| release_deadline_days | INT | — | N | T1 | Known for GIIS SG: results within one month (HB p.127) | Setting; "Not set" shows no deadline. |

### AS03 — Scheme Component

**Purpose:** One part of a reporting period and its weight, such as "PT1, 10 of 100" or "Half Yearly, 80 of 100".

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| component_id | UUID | PK | Y | T1 | Assumed | |
| scheme_id | UUID | FK:AS02 | Y | T1 | Assumed | |
| reporting_period_code | VARCHAR(32) | — | Y | T1 | Known (HB: TERM_1, TERM_2, SEM_1, SEM_2, OVERALL) | |
| component_code | VARCHAR(32) | — | Y | T1 | Known codes (HB: PT1, PT2, NOTEBOOK, SUBJECT_ENRICHMENT, HALF_YEARLY, YEARLY, CT1…CT3, IA1…IA4, SEM_1, SEM_2, PRE_BOARD) | |
| component_name | VARCHAR(80) | — | Y | T1 | Known | Shown on mark sheets and report cards. |
| component_kind | ENUM | — | Y | T1 | Assumed (DL-27) | TEST, EXAM, COURSEWORK, PRACTICAL, DESCRIPTOR. |
| max_marks | DECIMAL(6,2) | — | C | T1 | Known (HB: PT1 out of 20, Half Yearly out of 80; CBSE 9-10 "all marks entered out of 100") | Null for DESCRIPTOR. |
| weight | DECIMAL(5,2) | — | Y | T1 | Known (HB p.67–84) | Weights of a period add up to 100 (SR-AS-02). |
| subject_filter | JSON | — | N | T1 | Assumed | Components that apply to some subjects only (practicals). |
| dated | BOOLEAN | — | Y | T1 | Assumed | TRUE = scheduled as AS06 (PT1); FALSE = entered at term end (Notebook). |

### AS04 — Grade Scale

**Purpose:** How a percentage or descriptor becomes a grade.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| grade_scale_id | UUID | PK | Y | T1 | Known (Arch § Data model) | |
| scale_name | VARCHAR(80) | — | Y | T1 | Known | e.g. "CBSE 6-8 eight-point", "IB DP Category 1". |
| scale_type | ENUM | — | Y | T1 | Assumed (DL-27) | PERCENT_BANDS, POINTS, DESCRIPTORS. |
| bands | JSON | — | Y | T1 | Known for GIIS SG (CBSE 6-8: 91–100 A1 … 32 and below E, HB p.68–69; CBSE 3-5: A*–D, p.67; IB DP Category 1: 90–100% = 7 … <20% = 1, p.79; co-scholastic A–E with 5–1 points, p.85; KG: Beginner, Progressing, Proficient, Advanced, p.65; PYP: Exemplary, Proficient, Progressing…, p.87) | Ordered; no gaps or overlaps (SR-AS-03). |
| pass_band | VARCHAR(8) | — | N | T1 | Known (CBSE 6-9: D; CLSP: 40%) | Used by the promotion rule. |
| owner | ENUM | — | Y | T1 | Confirmed (DL-87) | TEMPLATE or SCHEME. |
| version / valid_from | — | — | Y | T1 | Known (Arch: "Grade scales are versioned per programme pack") | |

### AS05 — Promotion Rule

**Purpose:** What a student needs at year end to move up, for one scheme version.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| promotion_rule_id | UUID | PK | Y | T1 | Confirmed (DL-92) | |
| scheme_id | UUID | FK:AS02 | Y | T1 | Assumed | One per scheme version. |
| rule_kind | ENUM | — | Y | T1 | Known (HB p.128–131) | NO_RETENTION (K–5: retention only on parent request), MIN_GRADE_ALL_SUBJECTS (CBSE 6–9: D), MIN_PERCENT_ALL_SUBJECTS (CLSP, IGCSE, CBSE 11: 40%), POINTS_AND_REQUIREMENTS (IB Year 1: 24 points, 4/4/4 HL and SL, CAS, lab reports, EE draft), BOARD (CBSE 10 and 12). |
| min_attendance_percent | DECIMAL(5,2) | — | C | T1 | Known for GIIS SG: 85%, 90% for Student Pass holders (HB p.128) | From the student's attendance threshold (DL-30). "Not set" blocks proposals. |
| weak_pass | JSON | — | N | T1 | Known for GIIS SG IGCSE 9 (one subject at 35%, aggregate 40%) | |
| conditional | JSON | — | N | T1 | Known (IGCSE 9: two subjects at 35% → re-tests; CBSE 6–9: one more attempt for an E; CBSE 11: improvement test in one subject) | Number of subjects, re-test attempts. |
| other_requirements | JSON | — | N | T1 | Known (IB Year 1 CAS two-thirds, 60% of lab reports, EE draft) | Checked by the coordinator; not calculated. |
| decision_authority | VARCHAR(80) | — | Y | T1 | Known: Principal (Arch § Users and roles; HB p.130 "at the discretion of the Principal") | |

### AS06 — Assessment

**Purpose:** One dated test or exam for one grade and subject, such as Grade 7 PT2 Mathematics on 17 Nov 2026.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| assessment_id | UUID | PK | Y | T1 | Known (Arch § Data model) | |
| campus_id | UUID | FK:OR04 | Y | T1 | Known | |
| academic_year_id | UUID | FK:AC07 | Y | T1 | Known; per offering (DL-96) | The offering's year: GIIS SG CBSE April–March, IB DP July–May. |
| scheme_id / component_id | UUID | FK:AS02 / AS03 | Y | T1 | Confirmed (DL-88) | Must be an APPROVED scheme version. |
| grade_id | UUID | FK:AC02 | Y | T1 | Known (Exam Schedule: per grade) | |
| subject_id | UUID | FK:AC03 | Y | T1 | Known | Language groups (L2 Hindi/Tamil/French) are one assessment with several mark sheets. |
| assessment_kind | ENUM | — | Y | T1 | Assumed (DL-27) | REGULAR, RETEST, IMPROVEMENT, PRE_BOARD. |
| exam_window_id | UUID | FK:Calendar | N | T1 | Known (Arch § Data model Calendar "Exam window") | |
| scheduled_date / start_time / duration_minutes | DATE / TIME / INT | — | Y / N / N | T1 | Known (Exam Schedule; HB "Duration of PT1 45 Minutes") | |
| max_marks | DECIMAL(6,2) | — | C | T1 | Known | Defaults from AS03. |
| syllabus_note | TEXT | — | N | T1 | Known (HB: syllabus sent one week before a PT, 15 days before a term exam) | Shared to parents with the date. |
| assessment_status | ENUM | — | Y | T1 | Assumed (DL-27) | SCHEDULED, HELD, MARKING, LOCKED, PUBLISHED, CANCELLED. Derived from mark sheets once held. |
| marks_published_at / published_by_user_id | — | — | C | T1 | Known need (HB p.34: MyGIIS "Exam & Results" shows test marks online); Confirmed (DL-93) | Coordinator publishes locked marks to parents per assessment; this starts the appeal window where the setting counts from results. |
| scheduled_by_user_id | UUID | FK:A01 | Y | T1 | Known: coordinator publishes the assessment calendar (Arch § Key workflows step 1) | |

### AS07 — Mark Sheet

**Purpose:** The marks for one assessment and one class (or teaching group), entered by its teacher and moderated by the coordinator.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| mark_sheet_id | UUID | PK | Y | T2 | Confirmed (DL-88) | |
| assessment_id | UUID | FK:AS06 | Y | T1 | Assumed | |
| class_id / teaching_group_id | UUID | FK:CL01 / TE04 | Y (one) | T1 | Known (CL01); teaching groups in the [teaching & timetable dictionary](./teaching-timetable-data-dictionary.md) (DL-132) | |
| teacher_employee_id | UUID | FK:E01 | Y | T1 | Known (Employee DD D02 subject allocation, now TE06) | Default: the group's LEAD teacher (TE06, SR-TE-13). |
| sheet_status | ENUM | — | Y | T1 | Assumed (DL-27) | NOT_OPEN, OPEN, SUBMITTED, RETURNED, MODERATED, LOCKED. |
| submitted_at | TIMESTAMP | — | C | T1 | Assumed | |
| moderated_by_user_id / moderated_at | — | — | C | T1 | Known need (HB "Answer script moderation form for Half-Yearly and Yearly exam") | Coordinator or HOD (OQ-98). |
| moderation_note | TEXT | — | N | T2 | Assumed | |
| locked_by_user_id / locked_at | — | — | C | T1 | Known (Arch step 3 "moderation and lock follow") | |
| scripts_returned_on | DATE | — | N | T1 | Known (HB p.88: scripts given to students; starts the appeal window) | |
| entry_deadline | DATE | — | N | T1 | Assumed; setting | Shown as due date to the teacher. |

### AS08 — Mark

**Purpose:** One student's mark, or the reason there isn't one, on one mark sheet.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| mark_id | UUID | PK | Y | T2 | Known (Arch § Data model) | |
| mark_sheet_id | UUID | FK:AS07 | Y | T2 | Assumed | |
| enrolment_id | UUID | FK:ST02 | Y | T1 | Known | |
| raw_mark | DECIMAL(6,2) | — | C | T2 | Known | 0 ≤ raw_mark ≤ max_marks. Null when a code is set. |
| descriptor | VARCHAR(32) | — | C | T2 | Known (descriptor scales, AS04) | For DESCRIPTOR components only. |
| absence_code | ENUM | — | C | T2 | Confirmed need (HB p.43); codes Confirmed (DL-95) | ABSENT, APPROVED_ABSENCE, EXEMPT, NOT_ASSESSED ("NA", HB p.88). |
| prorated | BOOLEAN | — | N | T2 | Known (HB p.43 "marks would be prorated for the report card") | Only with APPROVED_ABSENCE and a Principal approval (SR-AS-07). |
| proration_approved_by_user_id | UUID | FK:A01 | C | T1 | Known (HB p.43 "approval from the principal") | |
| mark_version | INT | — | Y | T1 | Confirmed (DL-88) | Every change after lock adds a version. |
| change_reason | ENUM | — | C | T1 | Assumed (DL-27) | APPEAL, CORRECTION, RETEST. Required after lock. |
| appeal_id | UUID | FK:AS11 | C | T1 | Assumed | |
| entered_by_user_id / entered_at | — | — | Y | T1 | Assumed | |

### AS09 — Subject Result

**Purpose:** A student's calculated result in one subject for one reporting period, such as "Mathematics, Term 1: 78.5%, B1".

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| subject_result_id | UUID | PK | Y | T2 | Confirmed (DL-89) | |
| enrolment_id / subject_id / reporting_period_code | UUID, UUID, VARCHAR | UK; FK:ST02 / AC03 | Y | T2 | Known | |
| scheme_id | UUID | FK:AS02 | Y | T1 | Known (Arch: effective-dated rules) | The version used. |
| weighted_percent | DECIMAL(5,2) | — | C | T2 | Known (HB weightages) | Calculated from locked marks; never typed. |
| grade | VARCHAR(8) | — | C | T2 | Known | From AS04. Descriptor scales store the descriptor. |
| grade_points | DECIMAL(4,1) | — | N | T2 | Known (co-scholastic points, HB p.85; IB points) | |
| result_status | ENUM | — | Y | T1 | Assumed (DL-27) | INCOMPLETE (a mark missing), CALCULATED, FINAL. |
| missing_components | JSON | — | C | T1 | Assumed | Lists what blocks INCOMPLETE. |
| below_pass | BOOLEAN | — | N | T2 | Assumed; derived | Feeds the promotion watch list. |

### AS10 — Report Card

**Purpose:** The released record of a student's results for one reporting period.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| report_card_id | UUID | PK | Y | T2 | Known (Arch § Data model) | |
| enrolment_id | UUID | FK:ST02 | Y | T1 | Known | |
| reporting_period_code | VARCHAR(32) | UK with enrolment + version | Y | T1 | Known (HB: GIIS SG CBSE 6-8 "2 times a year"; CBSE 3-5 "after every assessment (4 Report Cards)"; KG HPC "twice in a year") | |
| card_version | INT | UK | Y | T1 | Confirmed need (DL-90) | 1, then 2… on reissue. |
| scheme_id / layout_code | UUID, VARCHAR | FK:AS02 | Y | T1 | Known; layout Open (OQ-94) | |
| class_id_snapshot / class_teacher_snapshot | — | — | Y | T1 | Assumed | As printed; not updated by later class moves. |
| results_snapshot | JSON | — | Y | T2 | Confirmed (DL-90) | Subject results, co-scholastic grades, descriptors at generation. |
| attendance_snapshot | JSON | — | N | T2 | Assumed; source Attendance | Days present / working days. Threshold only, never leave reasons. |
| class_teacher_remark | TEXT | — | N | T2 | Known practice (HB p.114: "well-defined report card to record student progress") | Written by the class teacher. |
| card_status | ENUM | — | Y | T1 | Assumed (DL-27) | DRAFT, READY, RELEASED, SUPERSEDED. |
| released_by_user_id / released_at | — | — | C | T1 | Known: coordinator releases results (Arch § Users and roles "results release") | |
| reissue_reason | ENUM | — | C | T1 | Assumed | APPEAL, CORRECTION. |
| document_file_id | UUID | FK:File | C | T2 | Known (HB p.34 "e-Report cards are available for download") | PDF made at release. |
| first_viewed_by_guardian_at | TIMESTAMP | — | N | T1 | Assumed | Shows "Seen by parent". |

### AS11 — Appeal

**Purpose:** A request to re-check a released mark, with its outcome.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| appeal_id | UUID | PK | Y | T2 | Known (Arch § Data model "Appeal") | |
| enrolment_id | UUID | FK:ST02 | Y | T1 | Known | |
| mark_id | UUID | FK:AS08 | Y | T2 | Known (HB p.127 "appeal for results") | One mark per appeal. |
| raised_by | ENUM + UUID | — | Y | T1 | Known: parent by email through MyGIIS (HB p.127) | GUARDIAN or STAFF_ON_BEHALF. |
| raised_at | TIMESTAMP | — | Y | T1 | Known | |
| window_closes_at | TIMESTAMP | — | Y | T1 | Known for GIIS SG (2 working days after scripts, HB p.88; ≥7 working days from results, HB p.127); per brand-country setting (DL-91) | Late appeals blocked ("deemed accepted", HB p.127). |
| reason | TEXT | — | Y | T2 | Assumed | |
| appeal_status | ENUM | — | Y | T1 | Assumed (DL-27) | SUBMITTED, WITH_TEACHER, AWAITING_ENDORSEMENT, UPHELD, NOT_UPHELD, WITHDRAWN, LATE_REJECTED. |
| reviewed_by_employee_id | UUID | FK:E01 | C | T1 | Known (HB p.127: forwarded to the teacher through the HOD) | |
| original_mark / revised_mark | DECIMAL | — | C | T2 | Known (HB p.127 "Modified marks may be re-entered") | |
| endorsed_by_user_id | UUID | FK:A01 | C | T1 | Known (HB p.127 "endorsed by the Examination Board"); membership Open (OQ-98) | |
| outcome_sent_at | TIMESTAMP | — | C | T1 | Known (coordinator mails the outcome) | |
| response_due_at | TIMESTAMP | — | Y | T1 | Known for GIIS SG (6 working days for CBSE 1–8, HB p.88; 4 weeks in-house, 8 weeks external, HB p.127) | |
| external_board_ref | VARCHAR(64) | — | N | T1 | Known (CBSE re-evaluation form, IBIS, CAIE Direct, HB p.127) | Board appeals tracked as a reference only. |

### AS12 — Promotion Decision

**Purpose:** What Assessment says should happen to one student at year end, and who decided it.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| promotion_decision_id | UUID | PK | Y | T2 | Confirmed (DL-92) | |
| enrolment_id / academic_year_id | UUID | UK; FK:ST02 / AC07 | Y | T1 | Known | |
| promotion_rule_id | UUID | FK:AS05 | Y | T1 | Assumed | |
| proposed_outcome | ENUM | — | Y | T2 | Known values (HB p.128–131) | Calculated. See enums. |
| proposal_reasons | JSON | — | Y | T2 | Assumed | e.g. "E in Mathematics", "Attendance 86% (needs 90%)". |
| final_outcome | ENUM | — | C | T2 | Assumed | Set by the coordinator or Principal; maps to CL07 `promotion_outcome`. |
| decision_status | ENUM | — | Y | T1 | Assumed (DL-27) | PROPOSED, NEEDS_DECISION, RETEST_PENDING, CONFIRMED, SENT_TO_ROLLOVER. |
| retest_assessment_ids | UUID[] | FK:AS06 | C | T1 | Known (HB: re-tests, improvement test) | |
| decided_by_user_id / decided_at | — | — | C | T1 | Known: Principal for detention, retention, IB Year 1 (HB p.130) | Coordinator for straight promotions (OQ-99). |
| parent_request_ref | UUID | FK:CC03 | C | T1 | Known for K–5 (HB p.128: parents' written request to retain) | |
| rollover_line_id | UUID | FK:CL07 | C | T1 | Confirmed (DL-75) | Set when handed to Classes. |

### AS13 — Predicted Grade

**Purpose:** A teacher's judgment of a student's final board result in one subject.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| predicted_grade_id | UUID | PK | Y | T2 | Known (Arch § Data model) | |
| enrolment_id / subject_id | UUID | UK with round; FK:ST02 / AC03 | Y | T1 | Known | |
| board | ENUM | — | Y | T1 | Known (HB p.132: IB DP, CBSE 12) | IB_DP, CBSE_12. |
| calculated_basis | JSON | — | N | T2 | Known for GIIS SG IB (Year 1 consolidated 5% FA I + 10% CT I + 5% FA II + 10% CT II + 20% Sem I + 50% Sem II; Year 2 20% IA + 80% Sem I; average mapped to board thresholds, HB p.132) | Shown to the teacher as a starting point. |
| predicted_value | VARCHAR(8) | — | Y | T2 | Known (IB 1–7; CBSE score) | Teacher judgment; may differ from the basis. |
| entered_by_employee_id | UUID | FK:E01 | Y | T1 | Known (HB p.133 "given by subject teachers") | |
| prediction_status | ENUM | — | Y | T1 | Assumed (DL-27) | DRAFT, SUBMITTED, RELEASED, UNDER_APPEAL, REVISED. |
| released_at | TIMESTAMP | — | C | T1 | Known for GIIS SG (first week of October, HB p.133) | |
| appeal_case_id | UUID | FK:CC03 | C | T2 | Known (committee headed by the Principal, with Academic Supervisor, IB Coordinator, Career Counsellor, HB p.133) | |

## Assessment lifecycle and how it maps to other modules

| Moment | Assessment record | Other module | Rule |
|---|---|---|---|
| New board rules | AS01 template PUBLISHED by GSG platform team | — | DL-87 |
| Plan the year | AS02 copied and adjusted, APPROVED by Academic Supervisor | — | DL-87, SR-AS-01 |
| Publish assessment calendar | AS06 SCHEDULED inside exam windows | Calendar checks clashes (out of scope); parent app shows dates and syllabus | DL-88 |
| Test held | AS07 OPEN per class | Teaching allocation gives the teacher (OBJ-12) | SR-AS-04 |
| Marks in | AS07 SUBMITTED → MODERATED → LOCKED | Audit event per change (OBJ-11) | DL-88 |
| Marks published | AS06 PUBLISHED | Parent app shows the marks; scripts returned; appeal window opens | DL-91, DL-93 |
| Period ends | AS09 CALCULATED → FINAL | Attendance supplies days present | DL-89 |
| Release | AS10 READY → RELEASED | Parent app notification; fee status is not checked (DL-83) | DL-90 |
| Appeal | AS11 → AS08 new version → AS10 reissued | Parent app; Examination Board | DL-91 |
| Year end | AS12 PROPOSED → CONFIRMED → SENT_TO_ROLLOVER | Classes CL07 receives the outcome; Principal approves the run | DL-75, DL-92 |
| Predicted grades | AS13 RELEASED | Careers reads them for transcripts (out of scope) | DL-94 |

## Field visibility by role

`M` manage, `A` approve, `V` view, `O` own classes or subjects only, `S` summary (counts, averages), `—` none. All cells Assumed (DL-26 rule) except where cited.

| Record | Principal (campus) | Academic Supervisor | Coordinator (programme band) | Class teacher | Subject teacher | Parent | Student | Group / brand leadership |
|---|---|---|---|---|---|---|---|---|
| Scheme template | V | V | V | — | — | — | — | V; GSG platform team M |
| Assessment scheme | V | A (DL-87) | M (prepare) | V | V | — | — | S |
| Assessment calendar | V | V | M (Known, Arch step 1) | V | V | V (dates and syllabus, own child's grade) | V (own grade) | — |
| Marks before publishing | V | V | M (moderate, lock, publish) | O (own class, all subjects; DL-73 scope) | O (enter: own subjects and classes, Known Arch § Users and roles) | — | — | — |
| Published marks, results, report cards | V, A (proration) | V | M (publish, release) | O (remarks) | O (own subjects) | V own children, after publishing or release (Known, HB p.34) | V own, if campus setting on (OQ-96) | S |
| Appeals | V | V | M (send outcome) | O (read) | O (re-mark own subject) | M own (raise, track) | — | S |
| Promotion decisions | A (Known, Arch § Users and roles) | V | M (review) | O (read) | — | own child's outcome after the rollover is applied | — | S |
| Predicted grades | V, chairs appeal committee | V (committee) | M (release) | — | O (enter own subjects) | V own child, after release | V own, after release (HB p.133) | — |

Fee status (DL-83) never appears on any assessment screen.

## Enumerations (candidates, DL-27 rule)

| Entity | Field | Values |
|---|---|---|
| AS01 | template_status | DRAFT, PUBLISHED, RETIRED |
| AS02 | scheme_status | DRAFT, SUBMITTED, APPROVED, SUPERSEDED |
| AS03 | component_kind | TEST, EXAM, COURSEWORK, PRACTICAL, DESCRIPTOR |
| AS04 | scale_type | PERCENT_BANDS, POINTS, DESCRIPTORS |
| AS05 | rule_kind | NO_RETENTION, MIN_GRADE_ALL_SUBJECTS, MIN_PERCENT_ALL_SUBJECTS, POINTS_AND_REQUIREMENTS, BOARD |
| AS06 | assessment_kind | REGULAR, RETEST, IMPROVEMENT, PRE_BOARD |
| AS06 | assessment_status | SCHEDULED, HELD, MARKING, LOCKED, PUBLISHED, CANCELLED |
| AS07 | sheet_status | NOT_OPEN, OPEN, SUBMITTED, RETURNED, MODERATED, LOCKED |
| AS08 | absence_code | ABSENT, APPROVED_ABSENCE, EXEMPT, NOT_ASSESSED |
| AS08 | change_reason | APPEAL, CORRECTION, RETEST |
| AS09 | result_status | INCOMPLETE, CALCULATED, FINAL |
| AS10 | card_status | DRAFT, READY, RELEASED, SUPERSEDED |
| AS11 | appeal_status | SUBMITTED, WITH_TEACHER, AWAITING_ENDORSEMENT, UPHELD, NOT_UPHELD, WITHDRAWN, LATE_REJECTED |
| AS12 | proposed_outcome / final_outcome | PROMOTED, WEAK_PASS, CONDITIONAL_RETEST, DETAINED, RETAINED_ON_REQUEST, BOARD_RESULT_PENDING, CERTIFICATE_CANDIDATE (IB, HB p.130) |
| AS12 | decision_status | PROPOSED, NEEDS_DECISION, RETEST_PENDING, CONFIRMED, SENT_TO_ROLLOVER |
| AS13 | prediction_status | DRAFT, SUBMITTED, RELEASED, UNDER_APPEAL, REVISED |

**Mapping to Classes (CL07 `promotion_outcome`):** PROMOTED and WEAK_PASS → PROMOTED; CONDITIONAL_RETEST → PROMOTED_CONDITIONAL (until the re-test result, then PROMOTED or RETAINED); DETAINED, RETAINED_ON_REQUEST, CERTIFICATE_CANDIDATE → RETAINED; BOARD_RESULT_PENDING → stays AWAITING_OUTCOME (OQ-79, OQ-100).

## Business rules

- **SR-AS-01 (Confirmed, DL-87):** An assessment can only use an APPROVED scheme version. Approving needs an Academic Supervisor; an approved version is read-only and a change is a new version from a later year.
- **SR-AS-02 (Known HB weightages; Assumed):** Component weights in a reporting period add up to 100. The scheme can't be submitted otherwise.
- **SR-AS-03 (Assumed):** Grade bands are ordered with no gaps or overlaps; every percentage from 0 to 100 maps to one grade.
- **SR-AS-04 (Known, Arch § Users and roles "marks entry" for subject teachers; Confirmed DL-93):** A teacher can enter marks only on mark sheets for subjects and classes in their teaching allocation (OBJ-12). The coordinator can enter on any sheet in their band, logged.
- **SR-AS-05 (Confirmed DL-88):** A mark sheet can be submitted only when every student has a mark, a descriptor or an absence code.
- **SR-AS-06 (Known Arch § Security; Confirmed DL-88):** After lock, a mark changes only through an upheld appeal, a re-test or a correction with a reason. Each change creates a new mark version and an audit event; the old value stays visible in history.
- **SR-AS-07 (Known, GIIS SG, HB p.43):** A prorated mark needs APPROVED_ABSENCE and the Principal's approval. A student with any absence in an assessment is flagged "Not eligible for awards" for Activities and Awards (HB p.43); the award rules stay there.
- **SR-AS-08 (Confirmed DL-89):** Grades are calculated by the scheme version in force for the academic year. There is no manual grade override.
- **SR-AS-09 (Confirmed DL-90):** A report card can be released only when all its subject results are FINAL. Release is per grade and reporting period; a single student can be held back only with a reason (for example an open re-test).
- **SR-AS-10 (Confirmed, DL-83):** Fee status is never checked before generating, releasing or showing a report card or result.
- **SR-AS-11 (Confirmed DL-90):** A released report card never changes. A corrected result creates the next card version, marks the old one SUPERSEDED and notifies the parents.
- **SR-AS-12 (Known, GIIS SG, HB p.88, p.127; Confirmed DL-91):** An appeal is accepted only inside the window set for the brand-country and grade band. One appeal per mark; no second appeal. Outcomes are due within the response time setting.
- **SR-AS-13 (Known, GIIS SG, HB p.128):** In Kindergarten to Grade 5 (CBSE, IB PYP) the proposal is always PROMOTED; RETAINED_ON_REQUEST needs the parents' written request (Case). GIIS Singapore setting, not a group rule.
- **SR-AS-14 (Known, GIIS SG, HB p.128–131; Confirmed DL-92):** The promotion proposal uses the attendance threshold on the enrolment (85%, or 90% for Student Pass holders at GIIS SG). Below it, the line is NEEDS_DECISION, never automatically DETAINED.
- **SR-AS-15 (Confirmed, DL-75):** Only CONFIRMED decisions are sent to the Classes rollover. Classes never changes an outcome; a later change (re-test result) is sent again and the rollover line updates (timing OQ-79).
- **SR-AS-16 (Known HB p.133; Confirmed DL-94):** Predicted grades are released by the coordinator; an appeal opens a Case for the committee headed by the Principal.
- **SR-AS-17 (Assumed, DL-59 pattern):** Appeal windows, response times, release deadline, attendance thresholds for promotion, student access to report cards and K–5 retention are settings with no group default. "Not set" blocks the step that needs it.

## Events

SchemeTemplatePublished, SchemeApproved, AssessmentScheduled, MarkSheetSubmitted, MarkSheetLocked, MarksPublished, MarkChanged (with reason), ResultsCalculated, ReportCardsReleased, ReportCardReissued, AppealRaised, AppealDecided, PromotionProposed, PromotionConfirmed (Classes CL07 reads it), PredictedGradesReleased. Events carry IDs only (architecture.md).

## Capabilities (new, DL-53 naming)

`assessment.template.manage` (GSG platform team) · `assessment.scheme.edit`, `.approve` · `assessment.calendar.manage` · `assessment.mark.enter` (scoped to own teaching), `.moderate`, `.lock`, `.correct` · `assessment.mark.approve_proration` · `assessment.mark.publish` · `assessment.result.view` · `assessment.report_card.generate`, `.release`, `.remark` · `assessment.appeal.raise` (parent), `.review`, `.endorse` · `assessment.promotion.review`, `.decide` · `assessment.predicted.enter`, `.release`.

## Decisions

| ID | Decision | Why |
|---|---|---|
| DL-87 (**Confirmed**, Henry chose A, 15 Nov 2026) | GSG platform team keeps a scheme template per board or programme pack. Each brand-country copies it into a versioned assessment scheme per programme offering and grade band, adjusting weights, grade bands, re-test rules and report card layout. An Academic Supervisor approves each version. Assessment is its own domain (architecture.md), capability prefix `assessment` | GIIS already adds its own weights and weak-pass rules on top of the board rules, and other brands will differ; one template keeps board rules in one place |
| DL-88 (**Confirmed**) | An assessment is one component on one date for one grade and subject; marks are entered per class on a mark sheet (Open → Submitted → Moderated → Locked); after lock only appeals, re-tests or corrections with a reason change a mark, each as a new version | Arch § Key workflows "moderation and lock"; audit of marks changes (Arch § Security) |
| DL-89 (**Confirmed**) | Results and grades are calculated by the scheme version for that year; no manual grade override; descriptor scales are entered as descriptors | One source for a grade; 2025-26 results keep 2025-26 rules |
| DL-90 (**Confirmed**) | A report card is a snapshot per student per reporting period; the coordinator releases per grade; released cards never change, corrections reissue a new version with a parent notice; fee status is never checked (DL-83) | Parents and universities need a stable record; Arch § Users and roles gives results release to coordinators |
| DL-91 (**Confirmed**) | One appeal round per mark: parent raises it in the app within the brand-country window, subject teacher re-marks via the HOD, Examination Board endorses, coordinator sends the outcome; board re-evaluations tracked as a reference | HB p.88, p.127 process; windows differ by grade so they are settings |
| DL-92 (**Confirmed**) | The promotion rule proposes an outcome per student from results and attendance %; judgment cases wait for a decision (Principal for detention and retention); confirmed outcomes go to the Classes rollover; re-tests are RETEST assessments | DL-75 says outcomes come from Assessment; HB p.128–131 rules |
| DL-93 (**Confirmed**) | Subject teachers see and enter marks for their own subjects and classes; class teachers see all subjects of their own class; parents see only published marks and released report cards for their own children; students see their own released report cards only where the campus turns it on for their grade (extends DL-68) | Least privilege by scope (Arch § Security); DL-73 class scope |
| DL-94 (**Confirmed**) | Predicted grades (IB DP, CBSE 12) are entered by subject teachers, released by the coordinator, appealed on a Case to a committee headed by the Principal | HB p.132–133 |
| DL-95 (**Confirmed**) | A missing mark carries a code (ABSENT, APPROVED_ABSENCE, EXEMPT, NOT_ASSESSED); proration needs the Principal; the reason stays in Attendance or Wellbeing | HB p.43; DL-66 keeps health detail out of other modules |

## Open questions (new)

| ID | Question | Why it matters for design |
|---|---|---|
| OQ-93 | Who approves an assessment scheme for a whole brand-country (the Academic Supervisor is a campus role), who in GSG maintains the board templates, and may a single campus vary its brand-country's scheme? | Scheme approval step and who sees "Submit for approval" |
| OQ-94 | What report card layouts does each brand use (sections, signatures, logos, languages, printed or download only)? | Report card preview and the layout setting |
| OQ-95 | Does a release need the Principal's sign-off, and what is the release deadline per brand (GIIS SG: within one month, HB p.127)? | Release button and deadline warnings |
| OQ-96 | From which grade may students see their own marks and report cards in the student app? | Student app results tab (extends DL-68) |
| OQ-97 | The GIIS handbook gives two appeal windows: 2 working days after scripts are returned (CBSE 1–8, p.88) and at least 7 working days from results (p.127). Which applies to which grades, and what do other brands use? | Appeal window setting and the "Appeal" button cut-off |
| OQ-98 | Who is the "Examination Board" that endorses appeal outcomes (HB p.127), and who moderates mark sheets (coordinator, HOD)? | Appeal and moderation approvers |
| OQ-99 | Who confirms straight promotions (coordinator or Principal), and how are re-tests and improvement tests scheduled and combined with the original mark (replace, cap at the pass mark)? | Promotion review screen and re-test results (with OQ-79) |
| OQ-100 | How do Grade 10 and 12 board results and IB final results come in (import, manual), and do they appear on report cards? | BOARD_RESULT_PENDING lines; Integrations scope |
| OQ-101 | Is the online exam portal (Arch step 3) a separate product, and do its marks import into mark sheets? | Mark sheet "Import" action |
| OQ-102 | How are marks handled for mid-year joiners and transfers in (HB p.43 "apply to mid joiners from the day they commence"): prorated, carried from the previous campus, or left blank? | Incomplete results for joiners |
| OQ-103 | What assessment schemes do brands other than GIIS use (OWIS, Harrods, others)? None are documented | Template coverage; sample data beyond GIIS is synthetic |
| OQ-104 | Does PYP reporting stay in Toddle (HB p.87) or move to Campus OS, and does KG Holistic Progress Card move? | Whether descriptor report cards are in Phase 2 |
| OQ-105 | Who enters co-scholastic and Holistic Progress Card descriptors (class teacher, subject teacher), and against which indicators? | Descriptor entry screens |
| OQ-106 | How long are marks, report cards and answer-script records kept (GIIS SG keeps some scripts five years, HB p.88), and how much MyGIIS results history moves at cutover? | Retention policy; parent "Past report cards" on day one |

## Readiness

- **Can be designed now:** scheme editor with template differences and approval, assessment calendar per grade (GIIS SG Exam Schedule 2026-27), teacher mark entry on a mark sheet with absence codes, coordinator moderation and lock, results by class, report card preview and release, parent report card view and appeal, appeal handling, promotion review feeding the Classes rollover (OWIS calendar-year example), predicted grades.
- **Keep generic, label Pending:** brand-country scheme approver (OQ-93), report card layouts (OQ-94), Principal release sign-off (OQ-95), student access (OQ-96), appeal window per grade (OQ-97), Examination Board (OQ-98), re-test combination (OQ-99), board results (OQ-100), PYP and KG reporting (OQ-104).

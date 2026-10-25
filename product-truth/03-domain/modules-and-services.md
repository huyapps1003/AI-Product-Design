# Modules and core services

**Status:** Proposed (Architecture v1); module codes Assumed (DL-04), 25 Oct 2026 · **Owner:** Henry
**Cites:** [Architecture v1 § Domains and modules](../00-sources/Global_Campus_OS_Product_Architecture_v1.md#domains-and-modules) · research §05 · DL-04
**Tóm tắt (VI):** 4 bề mặt trải nghiệm, 12 module campus và 8 dịch vụ lõi theo Architecture v1, kèm mã module (DL-04), bằng chứng từ tài liệu GIIS và mức thị trường đã có.

Use these names exactly in navigation and copy. Don't invent new module names or navigation groups.

## Experience surfaces

| Surface | Who | Design status |
|---|---|---|
| Parent app | Parents and guardians | Not started (out of v1, DL-01) |
| Student app | Students | Not started |
| Staff app and web | All staff; the Admin Platform lives inside it (DL-05) | Admin Platform slice designed |
| Leadership views | Principals, Country Directors, group leadership | Not started |

## Campus modules

| `module_code` (DL-04) | Module | Domain | Evidence in GIIS files | Market coverage found |
|---|---|---|---|---|
| `admissions` | Admissions and Enrolment | Learner lifecycle | Transfer policy, Student Pass cases | Most SIS suites, OpenApply. Transfers within a group not found as a feature |
| `student_records` | Student Records | Learner lifecycle | Identity documents kept current | Most SIS suites |
| `careers` | Careers and Universities | Learner lifecycle | AEPP placement, LOR, predicted grades | — |
| `teaching_learning` | Teaching and Learning (includes Academic Allocation and Timetable) | Teaching and assessment | Syllabus, worksheets, timetables | Toddle, ManageBac for IB |
| `assessment` | Assessment and Reporting | Teaching and assessment | CBSE eighths, IB 1–7, IGCSE and CLSP scales; re-evaluation in 2 days | IB/Cambridge in international suites; CBSE in Indian ERPs |
| `attendance_conduct` | Attendance and Conduct | Daily operations | 90% and 85% attendance rules, ICA notice after 7 days, gate log | Common |
| `campus_services` | Campus Services | Daily operations | Bus requests, bookshop, canteen | Common |
| `activities_awards` | Activities and Awards | Daily operations | Clubs, house points, awards | Common |
| `wellbeing` | Wellbeing and Inclusion | Care | Counselling, SEN, child protection restricted to named roles | Veracross, iSAMS |
| `fees_finance` | Fees and Finance | Money | Term billing in 4-month blocks, refund tables 3A and 3B, FPS | Most suites; strong in Indian ERPs |
| `communication_cases` | Communication and Cases | Engagement | Complaint SLAs of 2, 7, 14 and 21 working days | Messaging common; SLA-tracked cases not found |
| `staff_hr` | Staff and HR | People | Staff across campuses and programmes | Central HR in Arbor (UK trusts) |

Platform modules (DL-04): `global_admin` (Global Admin) and `governance` (access review, audit, reports).

The evidence column is GIIS only. Whether other brands' rules fit the same modules hasn't been researched (OQ-27).

## Shared core services

| Service | What it does (Architecture) |
|---|---|
| Identity and roles | One login per person; role plus scope ([`../04-access/`](../04-access/README.md)) |
| Tenancy and config | The hierarchy and inherited settings ([tenancy and scope](./tenancy-and-scope.md)) |
| Calendar engine | Academic years, terms, events, working week per campus |
| Rules engine | Versioned policies and thresholds |
| Programme packs | Curricula as data |
| Cases and SLAs | One Case object for complaints, appeals, withdrawals, leave over 2 days, Student Pass issues; reused for access requests and reviews (DL-08) |
| Notifications | Email, SMS, WhatsApp Business, push |
| Events and audit | Event log behind every change; immutable audit |

Market note (research §05): group configuration exists in vendors (iSAMS, Arbor); versioned policy rules were not found as a product feature.

## Open

OQ-20 (who owns programme packs and policy templates across brands) · OQ-27 (other brands' systems) · OQ-31 (LMS).

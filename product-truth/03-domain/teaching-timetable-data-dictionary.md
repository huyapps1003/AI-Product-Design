# Global Campus OS — Teaching & Timetable Data Dictionary v0.1 (draft)

**Status:** Proposed draft, 24 Nov 2026. DL-131…DL-145 Confirmed; OQ-77 answered by DL-132; OQ-147…OQ-165 Open · **Owner:** Henry
**Cites:** [Architecture v1](../00-sources/Global_Campus_OS_Product_Architecture_v1.md) § Principles 4 (calendar engine; "timetables… read from it"), § Context (MyGIIS baseline includes Timetable), § Users and roles (Programme or level coordinator "Timetables"; Subject teacher "Subject and sections"; Student "Timetable"), § Domains and modules (module 3 Teaching and Learning "Timetables, lessons, homework"; module 8 Campus Services "rooms"; module 12 Staff and HR "allocation; cover"), § Key workflows (Absence step 3 "by period or by day"; Admission step 4 "section, timetable"), § Data model (Organisation "Building, Room… feed timetabling"; Teaching "Timetable, Period"), § Tech (web for "timetables, marks entry"), § Security (AI "timetable optimisation… keeps a human approver"), § Roadmap (Phase 1 daily loop "timetable, homework") · [Employee Data Dictionary v1](../00-sources/Global_Campus_OS_Employee_Data_Dictionary_v1.md) D01 Academic Role Assignment, D02 Teacher Subject Allocation, D03 Timetable Teaching Session (all "External — do not own / Proposed") · [Academic structure dictionary](./academic-structure-data-dictionary.md) AC02, AC03, AC07, AC08, AC09, DL-116 · [Classes dictionary](./classes-data-dictionary.md) CL01 `home_room`, CL03, CL04, DL-73, OQ-77 · [Student Directory Spec](./student-directory-data-dictionary.md) ST02, ST03 · [Assessment dictionary](./assessment-data-dictionary.md) AS07 `teaching_group_id`, DL-93, SR-AS-04 · [Attendance dictionary](./attendance-data-dictionary.md) AT01, AT03 `period_id`, DL-99 · [Cases & communications dictionary](./cases-communications-data-dictionary.md) DL-127, DL-129 · [architecture.md](./architecture.md) DL-43 domains · GIIS Student Handbook 2025-26 (HB) p.22 (school timing), p.34–35 (MyGIIS Timetable, Syllabus, Worksheets), p.9–14 (room per class), p.47–62 (subjects per class), p.141–142 (additional languages, about 2½ hours a week) · DL-11, DL-16, DL-22, DL-26, DL-27, DL-30, DL-32, DL-43, DL-47, DL-53, DL-59, DL-73, DL-93, DL-96, DL-98, DL-110, DL-116, DL-118
**Tóm tắt (VI):** Từ điển dữ liệu cho module Giảng dạy & Thời khoá biểu: nhóm học (cả lớp, hoặc nhóm ghép nhiều lớp cho ngoại ngữ 2, môn tự chọn, môn IB DP), ai dạy nhóm nào (phân công giảng dạy, thay cho Employee DD D02), vai trò học thuật (điều phối khối, trưởng bộ môn), số tiết mỗi tuần theo môn và khối, khung giờ tiết học theo cơ sở và cấp học, danh sách phòng, thời khoá biểu có phiên bản (nháp → công bố) với kiểm tra trùng lịch, các tiết học theo ngày sinh ra từ thời khoá biểu và lịch ngày học, và dạy thay khi giáo viên vắng (ghi nhận giáo viên vắng, gợi ý người dạy thay, quyền xem học sinh chỉ trong ngày dạy thay). Bài tập về nhà, giáo án để sau. Mỗi trường có trạng thái Known / Assumed / Open.

> Drafted by Claude from product truth on 24 Nov 2026. The architecture's module 3 "Teaching and Learning" covers timetables, lessons, homework, syllabus, worksheets and LMS; this file covers **who teaches what and when, and who steps in when a teacher is away**. Homework, lesson plans, syllabus units and LMS come later (DL-131).

## How to read this file

Same layout and rules as the [Student Directory Spec](./student-directory-data-dictionary.md#how-to-read-this-file): **Known** (cited), **Assumed** (DL), **Open** (OQ); tiers T1/T2/T3 (DL-11; teaching groups, timetables and rooms are Tier 1; a teacher's load is Tier 1 work data, not HR data); `Y`/`N`/`C` required. Every row also carries `tenant_path`, `data_region` (UI term **Data location**, DL-47) and the shared common fields. Facts from the GIIS Handbook are **GIIS Singapore examples**, not group rules (DL-22). All enums are unapproved candidates (DL-27 rule). All example values are synthetic unless a source is cited; period times in examples are invented (OQ-147).

## Scope

Approved by Henry, 24 Nov 2026 (decision card "Allocation + timetable + cover"): **who teaches which students which subject, in which room, at what time, and who covers a lesson when its teacher is away.**

| In this dictionary | Not in this dictionary (owner) |
|---|---|
| Teaching groups: the students taught one subject together (a whole class, or a set drawn from several classes) | Classes and class placement (Classes CL01–CL07) |
| Who is in each group, kept in step with class placement and subject choices | Subject choices themselves (Student Records ST03) and which subjects a grade offers (Academic structure AC09) |
| Teaching assignments: which teacher teaches each group (replaces Employee DD D02) | Contracts, FTE, qualifications, staff leave (Staff and HR) |
| Academic responsibilities: level and programme coordinators, heads of department, academic supervisors (replaces Employee DD D01) | Access roles and grants (Identity and Access Registry, `04-access/`) |
| Period plans: periods per week per subject and grade; teacher load | Assessment schemes and exam timetables (Assessment AS02, AS06) |
| Bell schedules: the periods, breaks and assembly of a school day per campus and segment | School days, holidays and closures (Attendance AT01, AT02 until the Calendar engine) |
| A simple room list per campus, until Campus Services owns rooms | Room booking for events, assets, maintenance (Campus Services, later) |
| Timetables: versioned weekly patterns, built and checked for clashes, then published | Staff leave requests, leave balances and why a teacher is away (Staff and HR) |
| Dated lessons: each lesson on each school day, with one-off changes (replaces Employee DD D03) | Homework, worksheets, lesson plans, syllabus units, LMS (Teaching and Learning, later) |
| Teacher absences as they affect lessons, and cover for each affected lesson | Automatic timetable generation (later, with a human approver; Arch § Security) |

## Modeling boundaries

- **Teaching & Timetable is one module in the Teaching domain (DL-131).** Capability prefix `teaching` (DL-53); module team key `teaching_learning` (DL-04). It also arranges cover for absent teachers. It becomes the owner of the three Employee DD entities marked "External — do not own": D01 becomes TE07, D02 becomes TE06 and D03 becomes TE11. Other modules read from it: Assessment (who enters marks, DL-93), Attendance (periods, DL-99), Cases & Communications (who a teacher may message, DL-127), Classes (rooms, CL01 `home_room`).
- **A teaching group is what gets taught (DL-132).** Every subject that the whole grade takes is taught class by class, so each class × subject is a group by default: "7B Mathematics". A subject students choose is taught to a group drawn from several classes: "Grade 7 Hindi (L2)" takes the Hindi students of 7A, 7B and 7C (HB p.52; Assessment sample mark sheets already use these groups). IB DP subjects are groups too ("DP1 Biology HL"). **Streams such as CBSE 11 Science and Commerce are classes; the electives inside them are groups** (answers OQ-77). Mark sheets (AS07) and period registers (AT03) point at a group.
- **Group members follow placement and choice (DL-133).** A class group's members are the class's placements (CL04) on each date. A choice group's members are the students of its source classes whose subject choice (ST03) matches. When one choice is too big for one group (40 Hindi students), the coordinator splits it into two groups and places students; a student can only be placed in a group whose subject matches their choice. Changes keep history (DL-24).
- **One lead teacher per group (DL-134).** A teaching assignment names the teacher of a group, with role LEAD, CO_TEACHER or ASSISTANT, and dates. Exactly one LEAD at a time. This is "who teaches what": Assessment uses it to open mark sheets (SR-AS-04), messaging uses it for "own students" (DL-127). Like the class teacher (DL-73), an assignment **sets the teacher's data scope to the group's students automatically**; it never adds a capability, and Tier 3 stays out.
- **Academic responsibilities route approvals, they don't grant access (DL-135).** Level coordinator, programme coordinator, head of department and academic supervisor are recorded with their scope (a programme and grade band such as CBSE 6–8, or a subject department such as Sciences). Other modules use them to find the right approver: register corrections (DL-100), mark moderation (AS07), out passes (DL-102). They suggest an access scope (DL-16 default) but never grant one.
- **Period plans say how much of each subject a week (DL-136).** For each programme offering × grade × subject × year, the coordinator sets periods per week, how many are double periods, and the room type needed. GIIS teaches additional languages "approximately two and half hours per week" (HB p.141–142). A teacher's load is the sum of the periods of their assignments; the maximum load and minimum free periods are campus settings with no group default (DL-59 pattern; OQ-149).
- **Bell schedules belong to campus × segment (DL-137).** A bell schedule lists the periods of a school day with start and end times and a kind: teaching, registration, assembly, break, lunch, activity. A campus can have several (GIIS SG: Nursery and KG 9:00–12:30; Grades 1–12 9:00–15:40; students reach school by 8:50, HB p.22) and a variant for assembly days ("Morning assembly is held on assigned weekdays", HB p.22). Each year gets its own version. Period times at GIIS are Open (OQ-147).
- **A timetable is a versioned weekly pattern (DL-138).** A timetable covers one campus and one or more programme offerings for a date range inside their years (DL-96: CBSE and IB DP years start in different months at SG-ECS). It places lessons (group × teacher × room) on day × period cells of a 5-day week or an N-day cycle (OQ-148). In v1 the coordinator or timetabler builds it by hand with live clash checks, or imports it from today's tool (OQ-151); the Principal publishes it (OQ-150). Each publish is a new version effective from a date; the old one stays for history. Automatic generation comes later and always needs a person to approve (Arch § Security).
- **Hard clashes block publishing; soft ones warn (DL-139).** Hard: a teacher in two lessons at once, a room double-booked, a student in two groups at once, a lesson on a non-teaching period. Soft: a teacher over their maximum load, a subject's periods not matching its plan, the wrong room type, a teacher teaching at two campuses with no gap. Checks run across every offering at the campus and the teacher's other campuses, because teachers teach across offerings (IGCSE and IB DP at SG-ECS) and campuses (SG-PGL and SG-ECS, OQ-154).
- **Dated lessons come from the timetable and the school days (DL-140).** For each school day of the published school-day calendar (AT01), the published timetable produces dated lessons. Holidays and closures produce none; exam days and events can cancel lessons. A one-off change (another room, another period, cancelled) is made on the dated lesson with a reason; it never edits the timetable. Dated lessons are what a teacher's "My day", a student's day and period registers use, and what cover will use later.
- **A teacher absence lists the lessons that need cover (DL-143).** When a teacher will be away (from the staff leave record once Staff and HR exists, or entered by the coordinator or office), Teaching records only *that* they are away and when: whole days or some periods, never why (the reason stays with Staff and HR, Tier 2). Every dated lesson the teacher leads in that time is listed as needing cover. For each one the cover arranger picks: another teacher teaches it (COVER), a teacher supervises it (SUPERVISION), it joins another group's lesson (MERGE), or it is cancelled (CANCELLED). The arranger at GIIS is Open (OQ-162).
- **Cover suggestions are fair and explained (DL-144).** The system suggests teachers who are free in that period at that campus, putting first those who already teach the group or the subject, then those with the fewest covers this term; it never suggests someone over the campus's weekly cover limit (setting, no default; OQ-164). The arranger can pick anyone free. Covers are counted separately from teaching load.
- **A cover teacher sees the group only for that day (DL-145).** A confirmed cover gives the cover teacher data scope for the group's students for that dated lesson's day only, so they can take a period register or a class register when the class teacher is away (answers the "cover teacher" part of OQ-110). It adds no capability and no Tier 3. The cover teacher sees the lesson, the student list and any cover note from the absent teacher; never marks, cases or other history. The absent teacher, the cover teacher and the class teacher are notified; families are not, unless the lesson is cancelled (OQ-157).
- **Rooms live here until Campus Services owns them (DL-141).** Teaching keeps a simple room list per campus: code, name, type, capacity, building. GIIS rooms are already named per class ("S4 - 04", HB p.9–14; CL01 `home_room`). CL01 `home_room` becomes a reference to this list. Room booking for events is out of scope.
- **Everyone sees the timetable they need; data stays at the campus (DL-142).** Teachers see their own timetable and any class, group, teacher or room timetable at their campus; coordinators build within their band; parents and students see their own (MyGIIS "Timetable", HB p.34–35; apps later, DL-118). Timetables, groups and rooms are Tier 1 and live in the campus data location (DL-47); group views see counts and loads only.

## Entities

| ID | Entity | Module | Status |
|---|---|---|---|
| TE01 | Bell Schedule | Teaching | Known need (Arch § Data model "Period"; HB p.22 school timing); model Confirmed (DL-137) |
| TE02 | Bell Period | Teaching | Known need (Arch § Data model "Period"; Attendance AT03 `period_id`); times Open (OQ-147) |
| TE03 | Room | Teaching (until Campus Services) | Known entity (Arch § Data model Organisation "Building, Room… feed timetabling"; HB p.9–14); model Confirmed (DL-141) |
| TE04 | Teaching Group | Teaching | Known need (Assessment AS07 `teaching_group_id`; HB p.52 Language 2 and 3 choices; OQ-77); model Confirmed (DL-132) |
| TE05 | Group Membership | Teaching | Confirmed (DL-133) |
| TE06 | Teaching Assignment | Teaching (OBJ-12) | Known entity (Employee DD D02 "Teacher Subject Allocation"; Arch § Users and roles "Subject teacher: subject and sections"); model Confirmed (DL-134) |
| TE07 | Academic Responsibility | Teaching (OBJ-12) | Known entity (Employee DD D01 "Academic Role Assignment"; Arch § Users and roles coordinator); model Confirmed (DL-135) |
| TE08 | Period Plan | Teaching | Known need (HB p.141–142 "approximately two and half hours per week"); model Confirmed (DL-136) |
| TE09 | Timetable | Teaching (OBJ-27) | Known entity (Arch § Data model "Timetable"; MyGIIS Timetable, HB p.34–35); model Confirmed (DL-138) |
| TE10 | Timetable Lesson | Teaching (OBJ-27) | Confirmed (DL-138, DL-139) |
| TE11 | Dated Lesson | Teaching (OBJ-27) | Known entity (Employee DD D03 "Timetable Teaching Session"); model Confirmed (DL-140) |
| TE12 | Teacher Absence | Teaching (OBJ-27) | Known need (Arch module 12 "cover"; AT03 "cover teacher"); model Confirmed (DL-143) |
| TE13 | Cover Arrangement | Teaching (OBJ-27) | Known need (Arch module 12 "cover"); model Confirmed (DL-143, DL-144, DL-145) |

ST02, ST03, CL01, CL03, CL04, AC02, AC03, AC07, AC08, AC09, AT01, AT02 and E01 are referenced, not redefined. Employee DD D01, D02 and D03 are replaced by TE07, TE06 and TE11; the source file stays as it is (`00-sources/`).

## Detailed attribute dictionary

### TE01 — Bell Schedule

**Purpose:** The shape of a school day for some of a campus: when it starts and ends, and its periods and breaks.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| bell_schedule_id | UUID | PK | Y | T1 | Confirmed (DL-137) | |
| campus_id | UUID | FK:OR04 | Y | T1 | Known (HB p.22 timings are per campus) | |
| academic_year_id | UUID | FK:AC07 | Y | T1 | Confirmed (DL-137) | A new version each year; offerings with different years get their own (DL-96). |
| name | VARCHAR(60) | UK with campus + year | Y | T1 | Assumed | "Grades 1–12 regular day", "Nursery and KG". |
| applies_to | JSON | — | Y | T1 | Known that timings differ by segment (HB p.22) | Segments or programme offerings, optionally grades. GIIS SG: Nursery and KG; GMP Extended, IB PYP Early Years, Grades 1–12. |
| day_variant | ENUM | — | Y | T1 | Known that assembly days differ (HB p.22 "assigned weekdays"); other variants Assumed | REGULAR, ASSEMBLY_DAY, SHORT_DAY, EXAM_DAY. One REGULAR per `applies_to`. |
| used_on_days | JSON | — | C | T1 | Assumed | For non-REGULAR variants: which weekdays or cycle days use it (assembly rota, OQ-147). |
| day_start / day_end | TIME | — | Y | T1 | Known for GIIS SG (HB p.22: 9:00–15:40 Grades 1–12; 9:00–12:30 Nursery and KG; extended programme to 15:30) | |
| arrive_by | TIME | — | N | T1 | Known for GIIS SG (HB p.22: "reach school by 8:50") | Shown to families; Attendance keeps its own late rule (AT07). |
| schedule_status | ENUM | — | Y | T1 | Assumed (DL-27) | DRAFT, ACTIVE, RETIRED. A timetable can only use an ACTIVE schedule. |

### TE02 — Bell Period

**Purpose:** One slot in a bell schedule, such as Period 3, 10:50–11:30. The "timetable period" Attendance period registers point at.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| bell_period_id | UUID | PK | Y | T1 | Known need (AT03 `period_id`) | |
| bell_schedule_id | UUID | FK:TE01 | Y | T1 | Assumed | |
| sequence | INT | UK with schedule | Y | T1 | Assumed | Order in the day. |
| label | VARCHAR(24) | — | Y | T1 | Assumed | "P1", "Break", "Lunch", "Assembly". Labels per campus. |
| period_kind | ENUM | — | Y | T1 | Known kinds (HB p.22: assembly, break, lunch); codes Assumed (DL-27) | TEACHING, REGISTRATION, ASSEMBLY, BREAK, LUNCH, ACTIVITY. Only TEACHING and ACTIVITY take lessons. |
| start_time / end_time | TIME | — | Y | T1 | Open for GIIS (OQ-147); sample values invented | Must sit inside the schedule's day and not overlap. |
| duration_minutes | INT | — | Y | T1 | Derived | |

### TE03 — Room

**Purpose:** A space where lessons happen.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| room_id | UUID | PK | Y | T1 | Known entity (Arch § Data model "Room") | Referenced by CL01 `home_room`, TE10, TE11. |
| campus_id | UUID | FK:OR04 | Y | T1 | Known | |
| room_code | VARCHAR(24) | UK with campus | Y | T1 | Known format for GIIS SG (HB p.9–14 "S4 - 04"; Arch "Block B #02-01A") | As the campus writes it. |
| room_name | VARCHAR(60) | — | N | T1 | Assumed | "Chemistry Lab 2". |
| building | VARCHAR(40) | — | N | T1 | Known (Arch § Data model "Building") | Free text until Campus Services models buildings. |
| room_type | ENUM | — | Y | T1 | Assumed (DL-27) | CLASSROOM, SCIENCE_LAB, COMPUTER_LAB, ART_ROOM, MUSIC_ROOM, SPORTS, LIBRARY, HALL, OTHER. |
| capacity | INT | — | N | T1 | Assumed | Soft check against group size (DL-139). |
| room_status | ENUM | — | Y | T1 | Assumed (DL-27) | IN_USE, OUT_OF_USE, RETIRED. OUT_OF_USE rooms raise a hard clash for new lessons. |

### TE04 — Teaching Group

**Purpose:** The students taught one subject together in one year: a whole class, or a set drawn from several classes.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| teaching_group_id | UUID | PK | Y | T1 | Known need (AS07 `teaching_group_id`) | Referenced by AS07, AT03, TE05, TE06, TE10. |
| campus_id | UUID | FK:OR04 | Y | T1 | Known | |
| academic_year_id | UUID | FK:AC07 | Y | T1 | Known (DL-96) | Offering comes from the year. |
| offered_subject_id | UUID | FK:AC09 | Y | T1 | Confirmed (DL-116: "teaching groups… stay with Teaching") | The grade and subject come from it. |
| group_kind | ENUM | — | Y | T1 | Confirmed (DL-132) | CLASS (one class, everyone in it), CHOICE (students who chose this subject, from source classes), SPLIT (one of several groups of the same class subject, e.g. ability sets; OQ-152). |
| class_id | UUID | FK:CL01 | C | T1 | Assumed | Required for CLASS. |
| source_class_ids | UUID[] | FK:CL01 | C | T1 | Known need (Assessment sample "Grade 7 Hindi (L2)" across 7A–7C) | Required for CHOICE and SPLIT. Members come from these classes only. |
| group_name | VARCHAR(60) | UK with campus + year | Y | T1 | Assumed | "7B Mathematics", "Grade 7 Hindi (L2)", "DP1 Biology HL". Generated for CLASS; editable. |
| size_limit | INT | — | N | T1 | Assumed; campus setting per subject (OQ-152) | Soft check. |
| block_code | VARCHAR(16) | — | N | T1 | Known need (L2 options taught at the same time across a grade; IB DP option blocks, OQ-153) | Groups with the same block run in parallel: "G7-L2" holds the Hindi, Tamil and French groups. |
| group_status | ENUM | — | Y | T1 | Assumed (DL-27) | PLANNED, ACTIVE, CLOSED. CLASS groups are created when the class becomes ACTIVE (CL01). |

### TE05 — Group Membership

**Purpose:** That one student is in one teaching group, from when to when.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| membership_id | UUID | PK | Y | T1 | Confirmed (DL-133) | |
| teaching_group_id | UUID | FK:TE04 | Y | T1 | Assumed | |
| enrolment_id | UUID | FK:ST02 | Y | T1 | Known (membership belongs to the enrolment, like CL04) | |
| subject_choice_id | UUID | FK:ST03 | C | T1 | Known need (ST03) | Required for CHOICE and SPLIT; must match the group's subject. |
| valid_from / valid_to | DATE | — | Y / N | T1 | Confirmed pattern (DL-24) | |
| source | ENUM | — | Y | T1 | Confirmed (DL-133) | FROM_CLASS (follows CL04), FROM_CHOICE (follows ST03), PLACED (coordinator, for SPLIT or a choice with several groups). |
| placed_by_user_id | UUID | FK:User | C | T1 | Assumed | Required for PLACED. |

### TE06 — Teaching Assignment

**Purpose:** Which teacher teaches a teaching group, in what role, from when. OBJ-12. Replaces Employee DD D02.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| assignment_id | UUID | PK | Y | T1 | Known entity (Employee DD D02 `subject_allocation_id`) | |
| teaching_group_id | UUID | FK:TE04 | Y | T1 | Known (D02 subject + grade + section) | The group carries subject, grade, class and year. |
| employee_id | UUID | FK:E01 | Y | T1 | Known (D02) | Must have an active employment at the campus (E02, E07). |
| assignment_role | ENUM | — | Y | T1 | Assumed (DL-27, DL-134) | LEAD, CO_TEACHER, ASSISTANT. One LEAD at a time per group. |
| periods_per_week | DECIMAL(4,1) | — | Y | T1 | Confirmed (DL-136) | Defaults from the period plan (TE08); counts toward load. Co-teaching share: OQ-158. |
| valid_from / valid_to | DATE | — | Y / N | T1 | Known (D02 is effective-dated by year); Confirmed pattern (DL-24) | A mid-year change ends one row and starts another. |
| assigned_by_user_id | UUID | FK:User | Y | T1 | Known role (Arch § Users and roles: coordinator "Timetables"); who exactly Open (OQ-150) | |
| assignment_status | ENUM | — | Y | T1 | Assumed (DL-27) | PROPOSED, CONFIRMED, ENDED. Only CONFIRMED sets data scope (SR-TE-05). |

### TE07 — Academic Responsibility

**Purpose:** A staff member's academic role over part of a campus, such as Level Coordinator for CBSE 6–8 or Head of Sciences. Replaces Employee DD D01.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| responsibility_id | UUID | PK | Y | T1 | Known entity (Employee DD D01) | |
| employee_id | UUID | FK:E01 | Y | T1 | Known (D01) | |
| responsibility_type | ENUM | — | Y | T1 | Known titles (HB p.7–8, p.234: Academic Supervisors, Coordinators, Level Coordinator, Head of Department, IB Coordinator, Diploma Programme Coordinator); codes Assumed (DL-27) | LEVEL_COORDINATOR, PROGRAMME_COORDINATOR, HEAD_OF_DEPARTMENT, ACADEMIC_SUPERVISOR, CAREERS_COUNSELLOR, OTHER. Separate from access roles (D01). |
| campus_id | UUID | FK:OR04 | Y | T1 | Known (D01) | |
| programme_offering_id | UUID | FK:AC06 | C | T1 | Known (D01 `curriculum_id`) | For coordinators. |
| grade_band_id | UUID | FK:AC04 | C | T1 | Known (Arch: coordinator scope "IBDP, CBSE 11-12, IGCSE 9-10"); bands Open (OQ-129) | |
| department | VARCHAR(40) | — | C | T1 | Assumed | For heads of department: "Sciences", "Languages". Department list per campus. |
| academic_year_id | UUID | FK:AC07 | Y | T1 | Known (D01) | |
| valid_from / valid_to | DATE | — | Y / N | T1 | Known (D01) | |

### TE08 — Period Plan

**Purpose:** How many periods a week a grade spends on a subject, and how they must be placed.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| period_plan_id | UUID | PK | Y | T1 | Confirmed (DL-136) | |
| offered_subject_id | UUID | FK:AC09 | Y | UK | Assumed | One plan per offered subject (grade, subject, year). |
| periods_per_week | DECIMAL(4,1) | — | Y | T1 | Known for GIIS additional languages: about 2½ hours a week (HB p.141–142); other values Open | Per cycle when the campus uses an N-day cycle (OQ-148). |
| double_periods | INT | — | N | T1 | Assumed | How many of the periods come as back-to-back pairs (labs, art). |
| room_type_needed | ENUM | — | N | T1 | Assumed | Same values as TE03 `room_type`. Soft check. |
| max_per_day | INT | — | N | T1 | Assumed | Soft check, e.g. no more than 2 Mathematics periods a day. |
| plan_status | ENUM | — | Y | T1 | Assumed (DL-27) | DRAFT, APPROVED. The coordinator approves within their band. |

### TE09 — Timetable

**Purpose:** One version of the weekly lesson pattern for a campus and some of its programmes, from a date.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| timetable_id | UUID | PK | Y | T1 | Known entity (Arch § Data model "Timetable") | |
| campus_id | UUID | FK:OR04 | Y | T1 | Known | |
| programme_offering_ids | UUID[] | FK:AC06 | Y | T1 | Confirmed (DL-138) | One or more offerings; their years must overlap the date range. |
| academic_year_ids | UUID[] | FK:AC07 | Y | T1 | Confirmed rule (DL-96) | One year per offering. |
| name | VARCHAR(80) | — | Y | T1 | Assumed | "CBSE Middle, Semester 2". |
| version | INT | — | Y | T1 | Confirmed (DL-138) | Increments on every publish of the same name. |
| cycle_days | INT | — | Y | T1 | Open (OQ-148) | 5 = a Monday–Friday week. A rotating cycle (Day 1–6) needs a day-number calendar. |
| bell_schedule_ids | UUID[] | FK:TE01 | Y | T1 | Confirmed (DL-137) | Which schedules its cells use. |
| effective_from / effective_to | DATE | — | Y / N | T1 | Confirmed (DL-138) | Starts on a school day. Publishing a new version closes the previous one the day before. |
| timetable_status | ENUM | — | Y | T1 | Assumed (DL-27) | DRAFT, PUBLISHED, SUPERSEDED, ARCHIVED. See enums. |
| hard_clash_count / soft_clash_count | INT | — | Y | T1 | Confirmed (DL-139); derived | Publish needs zero hard clashes. |
| built_by_user_id | UUID | FK:User | Y | T1 | Known role (Arch: coordinator "Timetables"); timetabler Open (OQ-150) | |
| published_by_user_id / published_at | — | — | C | T1 | Assumed: Principal (OQ-150) | |
| import_source | VARCHAR(40) | — | N | T1 | Open (OQ-151) | e.g. "aSc export 2026-11-20". Imported lessons still go through the clash check. |
| change_note | TEXT | — | C | T1 | Assumed | Required to publish a version after the first: what changed and why. Sent with the change notice (SR-TE-10). |

### TE10 — Timetable Lesson

**Purpose:** One lesson in the pattern: a group with its teacher in a room, on a day of the cycle in a period.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| lesson_id | UUID | PK | Y | T1 | Confirmed (DL-138) | |
| timetable_id | UUID | FK:TE09 | Y | T1 | Assumed | |
| cycle_day | INT | — | Y | T1 | Assumed | 1–5 for Monday–Friday. |
| bell_period_id | UUID | FK:TE02 | Y | T1 | Assumed | Must be a TEACHING or ACTIVITY period. |
| span_periods | INT | — | Y | T1 | Assumed | 1, or 2 for a double period. |
| teaching_group_id | UUID | FK:TE04 | Y | T1 | Assumed | |
| employee_ids | UUID[] | FK:E01 | Y | T1 | Assumed | Defaults from the group's CONFIRMED assignments (TE06); the LEAD first. |
| room_id | UUID | FK:TE03 | N | T1 | Assumed | Empty is a soft clash ("No room"). |
| block_code | VARCHAR(16) | — | N | T1 | Assumed | Copied from the group; all groups of a block must sit in the same cells. |
| clash_flags | JSON | — | N | T1 | Confirmed (DL-139); derived | e.g. `[{"kind":"TEACHER_DOUBLE_BOOKED","severity":"HARD","with":"lesson_id"}]`. |

### TE11 — Dated Lesson

**Purpose:** A lesson on a real date, generated from the published timetable, with any one-off change. Replaces Employee DD D03.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| dated_lesson_id | UUID | PK | Y | T1 | Known entity (Employee DD D03 `teaching_session_id`) | Read by period registers (AT03) and, later, cover. |
| lesson_id | UUID | FK:TE10 | Y | T1 | Confirmed (DL-140) | |
| lesson_date | DATE | — | Y | T1 | Known (D03 `session_date`) | A school day in AT01 for the group's offering (DL-98). |
| bell_period_id | UUID | FK:TE02 | Y | T1 | Known (D03 `period_id`) | Can differ from the pattern when moved. |
| teaching_group_id | UUID | FK:TE04 | Y | T1 | Assumed | |
| employee_ids | UUID[] | FK:E01 | Y | T1 | Known (D03 `employee_id`) | The planned teachers. Who actually teaches it is the cover teacher when COVERED (TE13). |
| room_id | UUID | FK:TE03 | N | T1 | Assumed | |
| lesson_status | ENUM | — | Y | T1 | Known that only some statuses are consumed (D03 `timetable_status`); values Assumed (DL-27) | SCHEDULED, CHANGED, NEEDS_COVER, COVERED, CANCELLED. See enums. |
| change_kind | ENUM | — | C | T1 | Confirmed (DL-140) | ROOM, PERIOD, CANCELLED_EVENT, CANCELLED_EXAM, CANCELLED_OTHER. |
| change_reason | VARCHAR(200) | — | C | T1 | Assumed | Required for any change. Shown to the teacher; to families only for cancellations (OQ-157). |
| changed_by_user_id / changed_at | — | — | C | T1 | Assumed | |

### TE12 — Teacher Absence

**Purpose:** That a teacher will be away from some lessons, and which lessons need cover. Never why.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| absence_id | UUID | PK | Y | T1 | Confirmed (DL-143) | |
| employee_id | UUID | FK:E01 | Y | T1 | Known (Employee DD keys by `employee_id`) | |
| campus_id | UUID | FK:OR04 | Y | T1 | Known | One row per campus where the teacher has lessons. |
| date_from / date_to | DATE | — | Y | T1 | Assumed | Inclusive. |
| periods | JSON | — | N | T1 | Assumed | Null = whole days; otherwise bell periods on each date (a teacher out for training P5–P8). |
| absence_kind | ENUM | — | Y | T1 | Assumed (DL-27, DL-143) | LEAVE, DUTY (school trip, invigilation elsewhere, training), OTHER. Never a medical or personal reason (Tier 2, Staff and HR). |
| source | ENUM | — | Y | T1 | Assumed | STAFF_LEAVE (created by Staff & HR when leave is approved or sick leave reported, with dates and periods only: [staff & HR dictionary](./staff-hr-data-dictionary.md) DL-156, SR-HR-06), ENTERED (coordinator or office), TIMETABLE_EVENT. How GIIS teachers report absence today: OQ-163, OQ-168. |
| reported_at | TIMESTAMPTZ | — | Y | T1 | Assumed | Same-morning reports show first on the cover board. |
| cover_note | TEXT | — | N | T1 | Assumed | Work set by the absent teacher ("Worksheet 7 on the shared drive"). Shown to the cover teacher only. |
| recorded_by_user_id | UUID | FK:User | Y | T1 | Assumed | |
| absence_status | ENUM | — | Y | T1 | Assumed (DL-27) | PLANNED, ACTIVE, ENDED, CANCELLED. Cancelling returns covered lessons to SCHEDULED. |

### TE13 — Cover Arrangement

**Purpose:** What happens to one dated lesson whose teacher is away.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| cover_id | UUID | PK | Y | T1 | Confirmed (DL-143) | |
| dated_lesson_id | UUID | FK:TE11 | Y | T1 | Assumed | One arrangement per dated lesson (unique). |
| absence_id | UUID | FK:TE12 | Y | T1 | Assumed | |
| cover_kind | ENUM | — | Y | T1 | Assumed (DL-27, DL-143) | COVER (another teacher teaches), SUPERVISION (a teacher supervises set work), MERGE (joins another group's lesson), CANCELLED. |
| cover_employee_id | UUID | FK:E01 | C | T1 | Assumed | Required for COVER and SUPERVISION. Must be free in that period (hard check, any campus). |
| merged_into_dated_lesson_id | UUID | FK:TE11 | C | T1 | Assumed | Required for MERGE; room capacity is a soft check. |
| room_id | UUID | FK:TE03 | N | T1 | Assumed | Defaults to the lesson's room. |
| suggestion_rank | INT | — | N | T1 | Confirmed (DL-144) | Where the chosen teacher was in the suggestion list; 0 = picked by hand. |
| arranged_by_user_id / arranged_at | — | — | C | T1 | Open who arranges (OQ-162) | |
| cover_status | ENUM | — | Y | T1 | Assumed (DL-27) | NEEDED, ARRANGED, DONE, CANCELLED. NEEDED with under an hour to go is flagged "Urgent". |
| scope_granted_until | TIMESTAMPTZ | — | C | T1 | Confirmed (DL-145) | End of the lesson's school day; the derived scope ends then. |

## A teaching year and how it maps to other modules

| Moment | Teaching record | Other module | Rule |
|---|---|---|---|
| Next year set up | TE01 schedules and TE08 plans copied forward as DRAFT | Year PLANNING (AC07), offered subjects (AC09) | SR-TE-01, SR-TE-07 |
| Classes created | TE04 CLASS groups for every subject everyone takes | CL01 ACTIVE | SR-TE-02 |
| Subject choices made | TE04 CHOICE groups; TE05 members FROM_CHOICE | ST03 | SR-TE-03 |
| Teachers assigned | TE06 CONFIRMED; load calculated | Data scope set (Registry), like DL-73 | SR-TE-05, SR-TE-06 |
| Timetable built | TE09 DRAFT, TE10 lessons, clash check | Bell schedules, rooms | SR-TE-08, SR-TE-09 |
| Timetable published | TE09 PUBLISHED; TE11 lessons for each school day | AT01 school days; notifications (DL-129) | SR-TE-10, SR-TE-11 |
| School day | TE11 lessons for "My day" | AT03 period registers where on (DL-99) | SR-TE-12 |
| Holiday or closure added | TE11 lessons that day removed | AT02 | SR-TE-11 |
| Assessment held | Teacher from TE06 opens the mark sheet | AS07 per group | SR-TE-13 |
| Student moves class | TE05 FROM_CLASS rows move; choice groups re-checked | CL04 | SR-TE-04 |
| Teacher leaves mid-year | TE06 ENDED; groups without a LEAD flagged | Staff and HR (E02 end) | SR-TE-06 |
| Teacher away tomorrow | TE12; their TE11 lessons NEEDS_COVER; TE13 arranged | Staff leave HR06 (DL-156); notifications | SR-TE-16, SR-TE-17 |
| Cover lesson starts | Cover teacher has scope for that day | AT03 register taken by the cover teacher | SR-TE-18 |

## Field visibility by role

`M` manage, `A` approve, `V` view, `O` own (own lessons, groups or class), `—` none. All cells Assumed (DL-26 rule) except where cited.

| Record | Principal (campus) | Coordinator (band) | Head of department | Subject teacher | Class teacher | Campus office | Parent | Student | Group leadership |
|---|---|---|---|---|---|---|---|---|---|
| Bell schedules | A | M (OQ-150) | V | V | V | V | V (school day times) | V | — |
| Rooms | V | V | V | V | V | M (DL-141) | — | — | counts |
| Teaching groups and members | V | M (Known: Arch "Timetables") | V (department) | O | O (class) | V | — | own groups | counts |
| Teaching assignments, load | V | M | V (department) | O | O (class) | V | teacher names on the timetable | teacher names | load totals |
| Academic responsibilities | M | V | V | V | V | V | — | — | — |
| Period plans | A | M | V (department) | V | V | — | — | — | — |
| Timetable draft and clashes | V | M | V | — | — | — | — | — | — |
| Published timetable | A publish (OQ-150) | V | V | V (whole campus) | V | V | own child (HB p.34–35; app later, DL-118) | own | — |
| Dated lessons and changes | V | M | V | O (change room with coordinator OK, OQ-160) | V (class) | V | own child's cancellations (OQ-157) | own | — |
| Teacher absences (no reason) | V | M (OQ-162) | V (department) | own | V (class affected) | M (enter) | — | — | — |
| Cover arrangements, cover counts | V | M (OQ-162) | V (department) | own covers, cover note | V (class) | V | cancellations only | changed teacher on own timetable | counts |

## Enumerations (candidates, DL-27 rule)

| Entity | Field | Values |
|---|---|---|
| TE01 | day_variant | REGULAR, ASSEMBLY_DAY, SHORT_DAY, EXAM_DAY |
| TE01 | schedule_status | DRAFT, ACTIVE, RETIRED |
| TE02 | period_kind | TEACHING, REGISTRATION, ASSEMBLY, BREAK, LUNCH, ACTIVITY |
| TE03 | room_type | CLASSROOM, SCIENCE_LAB, COMPUTER_LAB, ART_ROOM, MUSIC_ROOM, SPORTS, LIBRARY, HALL, OTHER |
| TE03 | room_status | IN_USE, OUT_OF_USE, RETIRED |
| TE04 | group_kind | CLASS, CHOICE, SPLIT |
| TE04 | group_status | PLANNED, ACTIVE, CLOSED |
| TE05 | source | FROM_CLASS, FROM_CHOICE, PLACED |
| TE06 | assignment_role | LEAD, CO_TEACHER, ASSISTANT |
| TE06 | assignment_status | PROPOSED, CONFIRMED, ENDED |
| TE07 | responsibility_type | LEVEL_COORDINATOR, PROGRAMME_COORDINATOR, HEAD_OF_DEPARTMENT, ACADEMIC_SUPERVISOR, CAREERS_COUNSELLOR, OTHER |
| TE08 | plan_status | DRAFT, APPROVED |
| TE09 | timetable_status | DRAFT, PUBLISHED, SUPERSEDED, ARCHIVED |
| TE10 | clash kind (in `clash_flags`) | Hard: TEACHER_DOUBLE_BOOKED, ROOM_DOUBLE_BOOKED, STUDENT_DOUBLE_BOOKED, NOT_A_TEACHING_PERIOD, ROOM_OUT_OF_USE, BLOCK_SPLIT. Soft: OVER_MAX_LOAD, PLAN_MISMATCH, ROOM_TYPE, ROOM_TOO_SMALL, NO_ROOM, CROSS_CAMPUS_NO_GAP, OVER_MAX_PER_DAY |
| TE11 | lesson_status | SCHEDULED, CHANGED, NEEDS_COVER, COVERED, CANCELLED |
| TE11 | change_kind | ROOM, PERIOD, CANCELLED_EVENT, CANCELLED_EXAM, CANCELLED_OTHER |
| TE12 | absence_kind | LEAVE, DUTY, OTHER |
| TE12 | source | STAFF_LEAVE, ENTERED, TIMETABLE_EVENT |
| TE12 | absence_status | PLANNED, ACTIVE, ENDED, CANCELLED |
| TE13 | cover_kind | COVER, SUPERVISION, MERGE, CANCELLED |
| TE13 | cover_status | NEEDED, ARRANGED, DONE, CANCELLED |

### Status chips (proposed, AP-14)

| List | Value → screen label → chip |
|---|---|
| Timetable | DRAFT → Draft → default · PUBLISHED → Live → success · SUPERSEDED → Replaced → default · ARCHIVED → Archived → default |
| Dated lesson | SCHEDULED → (no chip) · CHANGED → Changed → info · NEEDS_COVER → Needs cover → error · COVERED → Cover → secondary · CANCELLED → Cancelled → warning |
| Cover | NEEDED → Needs cover → error (Urgent under an hour) · ARRANGED → Arranged → success · DONE → Done → default · CANCELLED → Cancelled → warning |
| Clash | HARD → Must fix → error · SOFT → Check → warning |
| Teaching assignment | PROPOSED → Proposed → default · CONFIRMED → Confirmed → success · ENDED → Ended → default |

## Business rules

- **SR-TE-01 (Confirmed, DL-137; Known times for GIIS SG, HB p.22):** Every campus × segment has one ACTIVE REGULAR bell schedule per year; periods don't overlap and sit inside the day. GIIS SG Grades 1–12: 9:00–15:40; Nursery and KG: 9:00–12:30.
- **SR-TE-02 (Confirmed, DL-132):** When a class becomes ACTIVE, a CLASS group is created for each offered subject the whole grade takes (AC09 with no choice group). Removing one needs the coordinator.
- **SR-TE-03 (Confirmed, DL-132, DL-133; Confirmed DL-116):** A subject in a choice group gets CHOICE groups across its source classes; members are students whose ST03 choice matches. A student without a matching group appears in "Not in a group" for the coordinator.
- **SR-TE-04 (Confirmed, DL-133):** Class moves (CL04) move FROM_CLASS memberships on the move date and re-check choice groups against the new class; history is kept.
- **SR-TE-05 (Confirmed, DL-134; pattern Confirmed DL-73):** A CONFIRMED teaching assignment sets the teacher's data scope to the group's current members (Registry auto-derive). It never adds a capability; Tier 3 is excluded; ending the assignment removes the scope.
- **SR-TE-06 (Confirmed, DL-134):** Each ACTIVE group has exactly one LEAD at any date. A group without a LEAD shows as "No teacher" to the coordinator and blocks publishing a timetable that includes it.
- **SR-TE-07 (Confirmed, DL-136; Known for GIIS languages, HB p.141–142):** A teacher's load = the sum of `periods_per_week` of their CONFIRMED assignments at all campuses. Over the campus maximum is a soft clash. No group default (DL-59 pattern; OQ-149).
- **SR-TE-08 (Confirmed, DL-139):** Hard clashes: a teacher in two lessons in the same cell (any campus); a room in two lessons in the same cell; a student in two groups in the same cell; a lesson on a period that isn't TEACHING or ACTIVITY; an OUT_OF_USE room; groups of one block in different cells. Any hard clash blocks publish.
- **SR-TE-09 (Confirmed, DL-139):** Soft clashes: load over maximum; a group's lessons per week ≠ its period plan; more than `max_per_day`; wrong room type or room too small; no room; a teacher at two campuses in back-to-back periods (travel, OQ-154). Each needs a note to publish with it.
- **SR-TE-10 (Confirmed, DL-138):** Publishing creates a new version effective from a school day, closes the old one the day before and notifies affected teachers, and families when their child's timetable changes (DL-129, REQUIRED). The Principal publishes (OQ-150).
- **SR-TE-11 (Confirmed, DL-140; Confirmed DL-98):** Dated lessons exist only on school days of a PUBLISHED school-day calendar for the group's offering. Adding a closure (AT02) cancels that day's lessons; an exam window can cancel lessons for the grades sitting it.
- **SR-TE-12 (Confirmed, DL-140; Confirmed DL-99):** With period registers on, a period register is due for each dated lesson of a CLASS group; for CHOICE groups, who takes the period register is Open (OQ-110).
- **SR-TE-13 (Confirmed DL-93; SR-AS-04):** A teacher can enter marks only for groups where they hold a CONFIRMED assignment; the mark sheet's default teacher is the LEAD.
- **SR-TE-14 (Confirmed, DL-140):** A one-off change on a dated lesson needs a reason and is audited; it never edits the timetable. Repeated changes to the same lesson suggest a new version.
- **SR-TE-15 (Confirmed DL-83 pattern):** Fee status never removes a student from a group or a timetable.
- **SR-TE-16 (Confirmed, DL-143):** A teacher absence marks every dated lesson the teacher leads in that time NEEDS_COVER and opens a TE13 row NEEDED. Teaching never stores why the teacher is away.
- **SR-TE-17 (Confirmed, DL-144):** Suggestions list teachers free in that period (no lesson, no other cover, not absent) at that campus, ordered: teaches this group → teaches this subject → fewest covers this term. Anyone at the campus weekly cover limit is left out (OQ-164). The arranger may pick anyone free.
- **SR-TE-18 (Confirmed, DL-145; pattern Confirmed DL-73):** An ARRANGED COVER or SUPERVISION gives the cover teacher data scope for the group's students for that school day only, so they can take that period's register (and the class register if the absent teacher is the class teacher, OQ-165). No capability, no Tier 3, no marks or cases.
- **SR-TE-19 (Confirmed, DL-145):** The absent teacher, the cover teacher and the class teacher are told when cover is arranged; families only when a lesson is cancelled (OQ-157). Every arrangement is audited.

## Events

BellScheduleActivated, TeachingGroupCreated, GroupMembershipChanged (Assessment, Attendance), TeachingAssignmentChanged (Registry derives scope; Assessment, Communications), ResponsibilityChanged (other modules' approval routing), TimetablePublished (Notifications, Attendance), DatedLessonChanged (Notifications; teacher and, for cancellations, families), TeacherLoadOverMaximum, TeacherAbsenceRecorded, CoverArranged (Registry derives one-day scope; Attendance; Notifications), CoverNeededUrgent. Events carry IDs only (architecture.md).

## Capabilities (DL-53 naming)

`teaching.allocation.view`, `teaching.allocation.manage` (exist in the catalogue; groups, members and assignments) · `teaching.responsibility.manage` · `teaching.plan.manage` · `teaching.bell.manage` · `teaching.room.manage` (campus office) · `teaching.timetable.view` · `teaching.timetable.build` · `teaching.timetable.publish` · `teaching.lesson.change` · `teaching.load.view` · `teaching.absence.record` · `teaching.cover.arrange` · `teaching.cover.view` (own covers).

## Decisions (DL-131…DL-145 Confirmed)

| ID | Decision | Why |
|---|---|---|
| DL-131 (**Confirmed**, Henry 24 Nov 2026, decision card "Allocation + timetable + cover") | Teaching & Timetable is one module in the Teaching domain (`teaching_learning`, capability prefix `teaching`): teaching groups, assignments, academic responsibilities, period plans, bell schedules, rooms (for now), timetables, dated lessons, and cover for absent teachers. It owns what Employee DD D01–D03 left "External". Homework, lesson plans, syllabus, LMS and automatic generation come later | 22 files point at "who teaches what" and "timetable period" with nothing behind them; Arch Phase 1 needs "timetable" |
| DL-132 (**Confirmed**, Henry 24 Nov 2026, decision card "Groups as proposed") | The teaching group is what gets taught: every whole-grade subject is a class group ("7B Mathematics"); chosen subjects get groups across classes ("Grade 7 Hindi (L2)"); IB DP subjects are groups. Streams (CBSE 11 Science, Commerce) are classes; electives inside them are groups (answers OQ-77) | HB p.52 language choices; Assessment already marks by language group (AS07); keeps classes simple |
| DL-133 (**Confirmed**, Henry 24 Nov 2026, decision card "Automatic, coordinator splits") | Group members follow class placement and subject choice automatically; the coordinator splits a big choice into several groups and places students, only into a group matching their choice; history kept | No double entry; ST03 stays the one record of choices |
| DL-134 (**Confirmed**, Henry 24 Nov 2026, decision card "One lead, students automatically") | A teaching assignment names one LEAD teacher per group (plus co-teachers and assistants), effective-dated. A CONFIRMED assignment sets the teacher's data scope to the group's students automatically, like the class teacher (DL-73); no capability, no Tier 3 | DL-93 and DL-127 need "own students"; DL-73 already did this for classes |
| DL-135 (**Confirmed**, Henry 24 Nov 2026, decision card "Route approvals, suggest access") | Academic responsibilities (level and programme coordinator, head of department, academic supervisor) are recorded with their band or department and used to route approvals in other modules; they suggest access scope but never grant it (DL-16 default) | Employee DD D01; approvals in DL-100, DL-102 and AS07 name "the coordinator" |
| DL-136 (**Confirmed**, Henry 24 Nov 2026, decision card "Each campus, no group default") | Period plans per offered subject set periods per week, double periods and room type; teacher load = sum of assigned periods; maximum load and free periods are campus settings with no group default | HB p.141–142 (languages about 2½ hours a week); DL-59 pattern |
| DL-137 (**Confirmed**, Henry 24 Nov 2026, decision card "Campus × segment, each year") | Bell schedules per campus × segment per year, with variants for assembly days; each period has a kind (teaching, registration, assembly, break, lunch, activity) | HB p.22 timings differ by segment; assembly "on assigned weekdays" |
| DL-138 (**Confirmed**, Henry 24 Nov 2026, decision card "Versions, Principal publishes") | A timetable is a versioned weekly (or N-day) pattern per campus covering one or more programmes; built by hand or imported in v1 with live clash checks; published by the Principal from a school day; each publish is a new version | Teachers cross offerings; years differ by offering (DL-96); Arch keeps a human approver for any optimisation |
| DL-139 (**Confirmed**, Henry 24 Nov 2026, decision card "Hard blocks, soft warns") | Hard clashes (teacher, room or student double-booked; non-teaching period; room out of use; block split) block publishing; soft ones (load, plan mismatch, room type or size, no room, cross-campus gap) need a note | A published timetable must be workable on day one |
| DL-140 (**Confirmed**, Henry 24 Nov 2026, decision card "Dated lessons, one-off changes") | Dated lessons are generated from the published timetable for each school day (AT01); one-off changes (room, period, cancelled) are made on the dated lesson with a reason and never edit the timetable; they feed "My day", period registers and later cover | Employee DD D03; DL-98 school days; DL-99 period registers |
| DL-141 (**Confirmed**, Henry 24 Nov 2026, decision card "Simple list in Teaching for now") | Teaching keeps a simple room list per campus (code, type, capacity, building) until Campus Services owns rooms; CL01 `home_room` points at it | Arch § Data model "Rooms… feed timetabling"; HB p.9–14 |
| DL-142 (**Confirmed**, Henry 25 Nov 2026, decision card "Campus-wide for teachers") | Teachers see their own and any campus timetable; coordinators build within their band; parents and students see their own (data now, apps later, DL-118). All Tier 1, held in the campus data location; group views see counts and loads only | MyGIIS Timetable parity (HB p.34–35); DL-47 |
| DL-143 (**Confirmed**, Henry 25 Nov 2026, decision card "Never record why, four outcomes") | A teacher absence records only that and when a teacher is away (days or periods, never why); every lesson they lead in that time needs cover; the arranger chooses cover, supervision, merge or cancel for each | Arch module 12 "cover"; the reason is Staff and HR data (Tier 2) |
| DL-144 (**Confirmed**, Henry 25 Nov 2026, decision card "Fit first, then fairness, limit") | Cover suggestions: free teachers at the campus, those who teach the group or subject first, then fewest covers this term; nobody at the campus weekly cover limit (setting, no default); covers counted apart from load | Fair, explainable suggestions; DL-59 pattern |
| DL-145 (**Confirmed**, Henry 25 Nov 2026, decision card "That day only, register access") | A confirmed cover gives the cover teacher scope for that group's students for that school day only, so they can take the register; no capability, no Tier 3, no marks or cases. Absent, cover and class teacher are told; families only for cancellations | DL-73 pattern; answers the "cover teacher" part of OQ-110 |

## Open questions (new)

| ID | Question | Why it matters for design |
|---|---|---|
| OQ-147 | What are the period times at each GIIS SG campus (how many periods, how long, when breaks and lunch fall), do they differ by segment, and which weekdays are assembly days? | Bell schedules (TE01, TE02); every timetable grid |
| OQ-148 | Does each campus run a Monday–Friday week or a rotating cycle (Day 1–6), and do Indian campuses have Saturday lessons? | Timetable cycle; school-day calendar link |
| OQ-149 | What is the maximum teaching load per week for a full-time teacher, the minimum free periods, and how is part-time load set? | Load warnings (SR-TE-07) |
| OQ-150 | Who builds the timetable at each campus (coordinator, a timetabler, Vice Principal), who approves period plans, and who publishes? | Capabilities and approval step |
| OQ-151 | Which tool builds timetables today (aSc, Untis, spreadsheet, MyGIIS), and should Campus OS import its output at cutover? | Import path; v1 effort |
| OQ-152 | How are language and elective groups formed: one group per subject across a grade, or split; what is the largest group; are ESL and learning support pulled out of other lessons? | Group sizes, SPLIT groups |
| OQ-153 | Are IB DP (and CBSE 11–12 elective) subjects taught in fixed option blocks, and what happens to a student whose choices clash? | Block rules (TE04 `block_code`) |
| OQ-154 | Do teachers teach at both SG-PGL and SG-ECS, on the same day, and how much travel time is needed between campuses? | Cross-campus clash rule |
| OQ-155 | Where does the room list live today (HB p.9–14 lists one room per class), and are labs and halls booked separately? | Room list import; booking scope |
| OQ-156 | Do CCAs, assemblies, library and off-site lessons (swimming) appear on the timetable as lessons? | ACTIVITY periods and groups |
| OQ-157 | Which timetable changes are families told about (new version, cancelled lesson, room change), and how? | Change notices |
| OQ-158 | How is co-teaching counted (shared load, both enter marks), and are learning support teachers assigned to groups? | Assignment roles and load |
| OQ-159 | Must a teacher be approved or qualified for a subject or board (CBSE, Cambridge, IB) before an assignment, and who keeps that record? | Assignment check; Staff and HR link |
| OQ-160 | How often do timetables change mid-year, who may make one-off room or period changes, and with how much notice? | Change rights; version cadence |
| OQ-161 | How do OWIS Singapore and other brands structure their day and timetable (periods, cycle, blocks)? | Bell schedule and cycle beyond GIIS; OWIS sample is synthetic |
| OQ-162 | Who arranges cover at each GIIS campus (level coordinator, Vice Principal, a cover officer), and by what time each morning? | Cover board owner, urgent flag |
| OQ-163 | How do teachers report an absence today (call, email, HR system), by when, and will Staff and HR leave feed Teaching automatically? | Absence source and timing |
| OQ-164 | Is there a weekly cover limit per teacher, are relief (external) teachers used, and are covers paid or counted toward load? | Suggestion rule, relief teachers |
| OQ-165 | When the class teacher is away, does the first period's cover teacher take the day register, or the co-class teacher (CL03)? | Register ownership on cover days |

## Readiness

- **Can be designed now:** cover board for today and tomorrow (lessons needing cover, suggestions, arrange in one step), teacher's "My covers", coordinator's timetable builder for a class or group with live clash panel, teacher week view with load, teaching groups and assignments list per grade (with "No teacher" and "Not in a group"), bell schedule and period plan settings, room timetable, teacher's "My day" with changed and cancelled lessons, publish step with change note.
- **Keep generic, label Pending:** period times (OQ-147), cycle (OQ-148), maximum load (OQ-149), who publishes (OQ-150), import (OQ-151), option blocks (OQ-153), family notices for changes (OQ-157), cover arranger and limit (OQ-162, OQ-164).

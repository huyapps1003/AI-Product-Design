# Campus OS — Classes: design brief for Claude Design

**Built from:** `product-truth/03-domain/classes-data-dictionary.md` v0.1 (PR #19), OBJ-15 card, Student Directory Spec v1.0, Admissions dictionary (DL-61), GIIS Student Handbook 2025-26 (HB) p.9–14, p.128–131, p.169
**Sample data:** `2-sample-data.json`. Where this brief and the dictionary disagree, the dictionary wins.

## 1. Context

- Global Campus OS is the school platform GSG runs for all its brands. GIIS SMART Campus Punggol (SG-PGL) is the detailed example. An OWIS campus (SG-OWD) shows a calendar-year school rolling over while GIIS is mid-year.
- Scope: **how students are grouped into classes for a year, and how they move up at year end.** Timetable, subject teachers, electives, attendance and promotion rules are other modules.
- Main viewer: **Dr Anita Rao, Principal, SG-PGL**. Today is **14 Nov 2026**; AY 2026-27 runs 13 Jul 2026 to 25 Jun 2027. Other viewers: Meera Joshi (Academic Coordinator, CBSE Grades 6–8), Farah Ismail (Class teacher, 7B), Lina Goh (Admissions Officer), Claire Tan (Principal, OWIS SG-OWD).
- Staff web app, desktop first; the class teacher view must work on a phone.

## 2. Words

| Use | Meaning | Don't use |
|---|---|---|
| **Class** | A group of students for one year, such as 7B or IB PYP 2A (DL-29, DL-70) | Section, homeroom (in UI) |
| **Class plan** | Number of classes × size limit for a grade and year; its total is the Admissions seat limit (DL-71) | Capacity plan |
| **Size limit** | Most students a class may hold (campus setting) | Strength, max |
| **Class teacher**, **Co-class teacher** | Looks after the class day to day (DL-73) | Form tutor, homeroom teacher |
| **AEPP support** | English support taken as a subject; the student stays in their class (DL-74) | AEPP class |
| **Class move** | Moving to another class of the same campus, programme and year; not a transfer (DL-72) | Transfer, reassign |
| **Rollover** | Year-end run that moves students into next year's grades and classes (DL-75) | Migration, promotion batch |
| **Promotion**, **Retained**, **Conditional** | Outcomes from Assessment (HB p.128–131) | Pass/fail |
| **House** | Student team that runs across years, e.g. House B (DL-76) | |
| **Segment**, **Student ID**, **Data location** | As in the Student Directory brief | |

## 3. Chips

Class: Draft · default, Active · success, Closed · default. Student flag: AEPP support · secondary.
Class fill: "29 of 32" with a bar; Full · warning; Over limit · error (only with Principal approval).
Class plan: Draft · default, Submitted · info, Approved · success. Size limit "Not set" · warning.
Placement reason: New enrolment, Allocation, Rollover, Class move, Correction (all default).
Rollover run: Draft · default, Outcomes pending · warning, Ready for approval · info, Approved · info, Applied · success, Cancelled · default.
Rollover line: Awaiting outcome · warning, Ready · success, Needs decision · warning, Awaiting approval case · info, Applied · success, Excluded · default.
Outcome: Promoted · success, Conditional · warning, Retained · warning, Graduating · info, Leaving · default, New admission elsewhere · info.

## 4. Rules that change the UI

1. **One class, one year** (DL-70). The year selector changes which classes you see. Closed classes and their rosters are read-only.
2. **Class plan = seat limit** (DL-71, DL-61). Each grade row shows classes × size limit = seat limit, and the Admissions numbers (enrolled, held). The Principal approves. A plan with a size limit "Not set" can't be approved. A plan can't go below enrolled + held. Admissions sees the seat limit read-only with "Set by the Principal's class plan".
3. **Full class** (DL-71). Placing into a full class is blocked; the coordinator can send it to the Principal with a reason. The Principal can approve one over the limit; it's logged.
4. **Placement history** (DL-72). A class move ends one placement and starts another on a date; the enrolment doesn't change. A reason note is required and is restricted (coordinator and Principal only).
5. **Needs a class** (SR-CL-06). Confirmed students from Admissions appear in a queue with a suggested class (fewest students, house balance). Confirmed students can be pre-placed from their start date.
6. **Class teacher** (DL-73). Every active class has one. 8C shows "No class teacher" as a warning. Assigning a class teacher automatically gives them access to that class for the same dates; it never adds what they can do, and restricted data is excluded.
7. **AEPP** (DL-74, Confirmed). AEPP is a support subject, not a class. Yuki stays in 5B with an "AEPP support" flag on the roster; AEPP teaching groups belong to Teaching.
8. **Rollover** (DL-75). Outcomes come from Assessment; Classes never edits them. Lines that need a decision (retained, conditional) block approval until resolved. Leavers and moves into another brand are excluded, and the brand move goes to Admissions as a new admission (DL-63). A stage-campus promotion needs no extra approval (DL-62); any other campus change needs both principals. The Principal approves the whole run, then it applies.
9. **Houses** (DL-76). House is kept across years. The house screen shows counts per house; allocation rules are pending (OQ-76).
10. **Restricted on rosters** (DL-30, DL-66). Rosters show the "Medical alert" flag and the attendance threshold (90% or 85%) only, never health detail, Student Pass or passport.
11. **Settings, no group default.** Size limits and house lists are campus settings. Missing values show "Not set" and block the step.
12. **No delete.** Close a class (end of year or merged), end a placement, cancel a run.

## 5. Who sees what (summary)

Principal: everything at the campus; approves class plans, over-limit placements and rollover runs. Academic coordinator: prepares plans, allocates and moves students, prepares rollover (who does this is pending, OQ-73). Class teacher: their own class only (roster, house, flags), on phone too. Subject teachers: rosters of their own classes. Admissions: class plan and seat limit read-only, and whether their enrolled students have a class. Parents and students: own class name, class teacher and house; classmates pending (OQ-75). Group leadership: counts only.

## 6. Pending decisions (show "Pending decision")

OQ-73 who allocates and moves students, and whether a move needs approval or a parent notice · OQ-74 size limits per brand and grade (CLSP and Grades 11–12 "Not set" at SG-PGL) · OQ-75 what parents see, when class lists publish · OQ-76 house allocation rules · OQ-77 streams and elective sets ("11 Science A") · OQ-78 mixed-age classes · OQ-79 late results after rollover, who decides retention.

## 7. Suggested screens

1. Classes overview (campus × year, grouped by programme and grade, fill bars, class teacher, room) · 2. Class detail (Roster, Staff, History) · 3. Class plan with seat limits and Admissions numbers · 4. Needs a class queue + place dialog · 5. Class move (with full-class and Principal approval) · 6. Rollover runs and run review · 7. Houses · 8. Class teacher "My class" on phone · 9. Class settings with "Not set".

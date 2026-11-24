# Campus OS — Teaching & Timetable: design brief for Claude Design

**Built from:** `product-truth/03-domain/teaching-timetable-data-dictionary.md` v0.1, OBJ-12 and OBJ-27 cards, Classes (DL-73), Assessment (DL-93), Attendance (DL-98, DL-99), Cases & Communications (DL-127, DL-129), GIIS Student Handbook 2025-26 (HB) p.9–14, p.22, p.34–35, p.52, p.141–142
**Sample data:** `2-sample-data.json`. Where this brief and the dictionary disagree, the dictionary wins.

## 1. Context

- Global Campus OS is the school platform GSG runs for all its brands. GIIS SMART Campus Punggol (SG-PGL) is the example campus; OWIS Singapore (SG-OWD) shows "Not set".
- Scope (Henry, 24 Nov 2026, DL-131): **who teaches which students which subject, in which room, at what time, and who covers a lesson when its teacher is away.** Homework, lesson plans, syllabus, LMS and automatic timetable generation come later.
- Main viewer: **Meera Joshi, Level Coordinator for CBSE Grades 6–8 at SG-PGL, on desktop.** Today is **Tuesday 24 Nov 2026, 08:20**. Arvind Menon (Mathematics 7A–7C and 8A–8C, class teacher 7A) called in at 07:32: he is away all day. Other viewers: Dr Anita Rao (Principal, publishes timetables), Farah Ismail (class teacher 7B, Science; on her phone), Rachel Lim (campus office), Claire Tan (OWIS).
- Coordinator and Principal screens are desktop. A teacher's "My day" is phone first.

## 2. Words

| Use | Meaning | Don't use |
|---|---|---|
| **Teaching group** | Students taught one subject together: "7B Mathematics" (a class) or "Grade 7 Hindi (L2)" (from 7A–7C) (DL-132) | Section, set, batch |
| **Lead teacher** | The one teacher responsible for a group (DL-134) | Owner, main teacher |
| **Load** | Teaching periods a week; "37 of 34" when over the campus maximum (DL-136) | Workload hours, FTE |
| **Bell schedule** | The periods and breaks of a school day (DL-137) | Bell times, day plan |
| **Timetable** | The weekly pattern. Published version = **Live**; working copy = **Draft** (DL-138) | Schedule, roster |
| **Clash** | **Must fix** (teacher, room or student in two places) or **Check** (load, room type, plan) (DL-139) | Conflict, error |
| **Block** | Groups taught at the same time across a grade, e.g. Language 2 (Hindi, Tamil, French) | Option line |
| **Cover** | Another teacher takes the lesson. **Supervision**: a teacher watches set work. **Merge**: joins another lesson (DL-143) | Relief, substitution (avoid in UI) |
| **Away** | A teacher is not available; never why (DL-143) | Sick, on MC, absent (for staff) |

## 3. Chips

Timetable: Draft · default, Live · success, Replaced · default.
Lesson today: Changed · info, Needs cover · error, Cover · secondary, Cancelled · warning.
Cover: Needs cover · error (add "Urgent" when it starts within an hour), Arranged · success, Done · default.
Clash: Must fix · error, Check · warning.
Assignment: Proposed · default, Confirmed · success, Ended · default. "No teacher" · error. "Not in a group" · warning.
Room: Out of use · warning.

## 4. Rules that change the UI

1. **Groups come from classes and choices** (DL-132, DL-133). Each class gets a group for every subject the whole grade takes. Language 2 and 3 are groups across 7A–7C, run as blocks (all Hindi, Tamil and French at the same time). Members follow class placement and subject choices; show "Not in a group" for Rhea Nair (7C, joined 16 Nov, no Language 3 choice yet).
2. **One lead teacher per group** (DL-134). A group without one shows "No teacher" (7C Visual Arts from 1 Dec). Assigning a teacher gives them the group's students for marks and messages, nothing more.
3. **Load** (DL-136). Show each teacher's periods a week against the campus maximum (sample 34). Arvind is at 37: a "Check" clash, not a block.
4. **Bell schedule** (DL-137, HB p.22). Grades 1–12: 9:00–15:40, arrive by 8:50; Nursery and KG: 9:00–12:30. Period times in the sample are invented: label "Times pending (OQ-147)". Breaks and lunch are rows in the grid but take no lessons.
5. **Live and Draft** (DL-138). The grid shows Live v2 (from 28 Sep) by default. The Draft v3 (from 4 Jan 2027) is edited beside it with a change list. Only the Principal publishes (pending OQ-150); publishing needs a change note and sends the change to affected teachers and families.
6. **Clashes** (DL-139). A side panel lists clashes live as cells change: 2 Must fix (Nikhil Sood in two lessons Thu P5; Seminar room double-booked Thu P2), 3 Check. "Publish" is disabled while any Must fix remains, with the count on the button. Each clash has a "Show" that highlights the cells and a suggested fix.
7. **Dated lessons** (DL-140). Today's lessons come from the Live timetable. A one-off change (room, period, cancel) is made on today's lesson with a reason and never edits the timetable: 7C Science moved from Lab 2 (out of use until 27 Nov) to Lab 1.
8. **Cover** (DL-143–DL-145). Arvind is "Away" (no reason shown). His 8 lessons today are on the cover board in period order. Each row shows suggestions: free teachers, Mathematics teachers first (Meera), then fewest covers this term, never anyone at the weekly cover limit (sample 3). One tap arranges; the arranger can pick anyone free, or choose Supervision, Merge or Cancel. P1 starts in 50 minutes: "Urgent". The cover teacher sees the students for that day only, Arvind's cover note, and can take the register. Families hear only about cancellations.
9. **Fee status never appears here** (DL-83 pattern).
10. **Settings, no group default.** Period times, cycle, maximum load and cover limit show the value or "Not set" (OWIS: "Not set").

## 5. Who sees what (summary)

Coordinator: groups, assignments, plans, timetable draft and cover within the band. Principal: everything at the campus; publishes. Teacher: own timetable and covers, any class or room timetable at the campus. Class teacher: own class. Campus office: rooms, records absences. Parent and student: own timetable (apps later, DL-118). Group leadership: counts and loads only.

## 6. Pending decisions (show "Pending decision")

OQ-147 period times · OQ-148 week or rotating cycle · OQ-149 maximum load · OQ-150 who builds and publishes · OQ-151 import from today's tool · OQ-152 language group sizes · OQ-153 IB DP option blocks · OQ-154 teaching at two campuses · OQ-155 room list · OQ-156 CCAs and assemblies on the timetable · OQ-157 which changes families hear about · OQ-158 co-teaching · OQ-159 teacher subject approval · OQ-160 who makes one-off changes · OQ-161 OWIS day · OQ-162 who arranges cover · OQ-163 how absences are reported · OQ-164 cover limit, relief teachers · OQ-165 who takes the day register when the class teacher is away.

## 7. Suggested screens

1. Cover board today (Meera) · 2. Class timetable 7B (Live, week grid with blocks) · 3. Timetable builder: Draft v3 beside Live, clash panel, Publish (Dr Rao) · 4. Teaching groups and assignments, Grade 7 · 5. Teachers and load · 6. My day (Farah, phone) with a cover lesson and a room change · 7. Bell schedule, rooms and period plan.

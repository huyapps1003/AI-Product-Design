# Campus OS — Attendance: design brief for Claude Design

**Built from:** `product-truth/03-domain/attendance-data-dictionary.md` v0.1, OBJ-20 card, Classes (DL-73), Assessment (DL-92, DL-95), Fees (DL-83), Student Directory (DL-30), GIIS Student Handbook 2025-26 (HB) p.23, p.128–131, p.166–167, p.174, GIIS SG Holiday Schedule 2026-27
**Sample data:** `2-sample-data.json`. Where this brief and the dictionary disagree, the dictionary wins.

## 1. Context

- Global Campus OS is the school platform GSG runs for all its brands. GIIS SMART Campus Punggol (SG-PGL) is the example campus.
- Scope (approved by Henry, 19 Nov 2026): **whether each student was in school each school day, why not, and what happens when attendance falls too low.** Conduct (incidents, sanctions, house points), the full calendar engine, staff attendance, bus boarding and gate hardware are other modules.
- Main viewer: **Farah Ismail, class teacher of 7B, SG-PGL, on her phone.** Today is **Thursday 19 Nov 2026, 10:20**, Semester 2 of the CBSE year (6 Apr 2026 – 19 Mar 2027). Deepavali (9–10 Nov) was a holiday; December vacation starts 5 Dec. Other viewers: Meera Joshi (Academic Coordinator, Grades 6–8), Dr Anita Rao (Principal), Siti Rahman (reception), Lina Goh (Admissions, owns Student Pass matters, Tier 3), Neha Sharma (parent of Aarav, 7B).
- Class teacher and parent screens are phone first. Coordinator, Principal, reception and Admissions screens are desktop.

## 2. Words

| Use | Meaning | Don't use |
|---|---|---|
| **School day** | A weekday in the curriculum's term dates that isn't a holiday or break (DL-98) | Working day, instructional day |
| **Register** | One class's attendance for one day (DL-99) | Roll call, attendance sheet |
| **Present**, **Late**, **Left early**, **Absent**, **On leave**, **School activity** | The six marks | Tardy, AWOL, excused/unexcused |
| **Leave request** | A parent's request for days off (DL-101) | Absence note, leave application form |
| **Out pass** | Permission to leave early, signed out at reception (HB p.23) | Early dismissal slip |
| **Late sign-in** | Arrival after 09:00 recorded at reception (HB p.174) | Tardy slip |
| **Correction** | A change to a locked register, with a reason (DL-100) | Edit, override |
| **Attendance threshold** | The minimum % a student must keep, e.g. "Threshold 90%" | Student Pass rule (never in teacher or coordinator UI) |
| **At risk** / **Below threshold** | Projected or current % under the threshold | Failing, defaulter |
| **ICA review** | Tier 3 Case for Admissions when a Student Pass rule is hit (DL-105) | Only in Lina Goh's view |

## 3. Chips

Mark: Present · success, Late · warning, Left early · warning, Absent · error, On leave · info, School activity · info.
Register: Not taken · default (warning "Overdue" after cut-off), Submitted · info, Locked · success. "No class teacher" · error.
Leave: Submitted · info, Approved · success, Declined · error, Cancelled · default, Certificate due · warning, Closed · default. Route: "Class teacher" · default, "Principal" · secondary.
Gate: Late sign-in · warning, Out pass · info, Excused (bus, doctor's note, prior permission) · default.
Standing: On track · success, At risk · warning, Below threshold · error, Can't reach threshold · error.
Correction: Requested · info, Applied · success, Rejected · default.
Alert: Open · warning, Acknowledged · info, Resolved · success, Dismissed · default.

## 4. Rules that change the UI

1. **School days only** (DL-98). Registers open only on school days. Holidays and breaks show on the calendar and are skipped in the date picker. The % always divides by school days (196 this year at SG-PGL).
2. **One mark per student per day** (DL-99). The register defaults everyone to Present; the teacher taps the exceptions. Students already on approved leave or signed in late at reception arrive pre-filled with a "from leave" or "from reception" tag. Period registers are off at SG-PGL.
3. **Cut-off and lock** (DL-100). Registers are due by the campus cut-off (sample 09:15, pending OQ-110) and lock at end of day. After lock, changing a mark asks for a reason and goes to the coordinator; the mark shows "Correction requested" until approved. Approved leave and gate events update marks without approval.
4. **Unexplained absence** (DL-107). Absent at the cut-off with no approved leave sends the parent a same-day notice. Show "Parent notified 09:30" on the student row.
5. **Leave routing** (DL-101, HB p.23, p.166). 1–2 school days: the class teacher approves. More than 2: the Principal decides, the class teacher and coordinator see it read-only, and a Case is opened. Count school days, not calendar days (Manav: 30 Nov – 4 Dec = 5). Declining needs a note the parent sees. Medical leave shows "Certificate due" until a certificate is uploaded on return; the teacher sees "Certificate received", never the file.
6. **Gate log** (DL-102, HB p.23, p.174). Reception records late sign-ins (time, stated reason, exemption: school bus, doctor's note, prior permission) and out passes (approved by the class teacher or level coordinator before the student leaves, who collected them). Exempt lates don't count toward the late steps. A fourth late in a term (Classes 1–5) shows the next step.
7. **Attendance % and threshold** (DL-103, DL-104). Each student shows their % for the rule that applies, the threshold, best-case projection and standing. Teachers and coordinators see **"Threshold 90%"** only, never "Student Pass" or anything about ICA (DL-30). Warning margin is a campus setting (sample 3 points).
8. **ICA review** (DL-105). Only Lina Goh (Tier 3) sees ICA review Cases. The Case lists the trigger (8 consecutive school days; or month under 90% without a valid reason), the steps, and fields to record the filing date and reference. The system never files on its own.
9. **Other modules get the % only** (DL-106). Report cards show days attended of school days; no reasons. Never show fee status in Attendance (DL-83).
10. **Parent view** (DL-107). Parent sees today's mark and times, this month and semester % against the threshold, their leave requests, and "Apply for leave". Never other children.
11. **Settings, no group default.** Thresholds, cut-off, lock time, leave routing limit, late steps: show the value or "Not set" (OWIS shows "Not set").
12. **No delete.** Cancel a leave request, reject a correction, dismiss an alert with a reason.

## 5. Who sees what (summary)

Class teacher: own class register, marks, leave for own class (approves 1–2 days, out passes), %, threshold only. Coordinator: all registers in their band, corrections to approve, at-risk list, out pass approvals. Principal: whole campus, leave over 2 days, rules, school-day calendar. Reception: gate log only. Admissions (Tier 3): ICA review Cases, medical certificate files. Parent: own child. Group leadership: counts and % only.

## 6. Pending decisions (show "Pending decision")

OQ-108 does approved leave count against the % · OQ-109 leave approver when away · OQ-110 cut-off, lock, who corrects, cover registers · OQ-111 partial days, late steps for Classes 6–12 · OQ-112 ICA filing steps · OQ-113 thresholds for other brands · OQ-114 school activities · OQ-115 mid-year joiners · OQ-116 authorised pickup list · OQ-117 student access · OQ-118 retention and MyGIIS history · OQ-119 Saturday and grade-only days · OQ-120 who maintains holidays · OQ-121 gate devices.

## 7. Suggested screens

1. Class teacher register (phone, 7B today) · 2. Class teacher inbox: leave requests, alerts · 3. 7B attendance (% and standing per student) · 4. Coordinator register board (Grades 6–8) and corrections · 5. Reception gate log · 6. Principal leave decision (Manav, 5 days) · 7. ICA review Case (Lina Goh) · 8. School-day calendar (2026-27) and campus rules · 9. Parent: today, %, apply for leave.

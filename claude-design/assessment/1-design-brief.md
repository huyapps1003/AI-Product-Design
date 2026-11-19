# Campus OS — Assessment & Report Cards: design brief for Claude Design

**Built from:** `product-truth/03-domain/assessment-data-dictionary.md` v0.1, OBJ-19 card, Classes dictionary (DL-75), Fees & Billing (DL-83), GIIS Student Handbook 2025-26 (HB) p.34, p.43, p.65–88, p.127–133, GIIS SG Exam Schedule 2026-27
**Sample data:** `2-sample-data.json`. Where this brief and the dictionary disagree, the dictionary wins.

## 1. Context

- Global Campus OS is the school platform GSG runs for all its brands. GIIS SMART Campus Punggol (SG-PGL) is the detailed example. OWIS Digital Campus (SG-OWD) shows a calendar-year school deciding promotions now; East Coast (SG-ECS) shows IB predicted grades.
- Scope: **how students are assessed, how marks become grades and report cards, and what result decides promotion.** Exam calendar engine, invigilation, question papers, online exam portal, board submissions, recommendation letters and attendance are other modules.
- Main viewer: **Meera Joshi, Academic Coordinator, CBSE Grades 6–8, SG-PGL**. Today is **Sunday 15 Nov 2026**, in Term 2 of AY 2026-27 (Term 1 report cards went out 9 Oct; PT2 is running on Tuesdays; Yearly exams 3–19 Mar 2027). Other viewers: Dr Anita Rao (Principal), Dr Suresh Pillai (Academic Supervisor), Arvind Menon (Maths teacher 7A–7C and 8A–8C), Farah Ismail (Class teacher 7B, Science teacher), Neha Sharma (parent of Aarav, 7B), Claire Tan (Principal, OWIS SG-OWD), Rahul Bose (IB DP Coordinator, SG-ECS).
- Staff web app, desktop first. Teacher mark entry and the parent view must work on a phone.

## 2. Words

| Use | Meaning | Don't use |
|---|---|---|
| **Scheme template** | GSG's starting rules for a board (CBSE 6-8, IB DP…) | Pack (in UI) |
| **Assessment scheme** | A brand-country's version: components, weights, grade scale, promotion rule (DL-87) | Grading policy |
| **Assessment** | One dated test for a grade and subject, e.g. Grade 7 PT2 Mathematics, 17 Nov | Exam event |
| **Mark sheet** | The marks for one assessment and one class or language group (DL-88) | Grade book |
| **Publish marks** | Make locked marks visible to parents (MyGIIS parity) | Release (that's for report cards) |
| **Report card** | Released results for one term or semester; never edited, reissued if corrected (DL-90) | Transcript, result slip |
| **Appeal** | Parent's request to re-check a published mark; one round (DL-91) | Complaint, re-exam |
| **Promotion decision** | Proposed and final year-end outcome; goes to the Classes rollover (DL-92) | Pass/fail |
| **Re-test** | A second attempt that can change a conditional outcome | Retake, makeup |
| **Predicted grade** | Teacher's judgment of a final board grade (IB DP, CBSE 12) | Forecast |
| **Approved absence**, **Exempt**, **NA** | Codes for a missing mark (DL-95) | Medical (never show a reason) |

## 3. Chips

Scheme: Draft · default, Submitted · info, Approved · success, Superseded · default. "Differs from template" · secondary.
Assessment: Scheduled · default, Marking · info, Locked · success, Published · success, Cancelled · default.
Mark sheet: Not open · default, Open · info, Submitted · info, Returned · warning, Moderated · info, Locked · success. Overdue entry · warning.
Grade: show the grade text (A1, B2…); E · error, D · warning. Result Incomplete · warning.
Report card: Draft · default, Ready · info, Released · success, Superseded · default. "Reissued v2" · secondary. "Seen by parent" · default.
Appeal: Submitted · info, With teacher · info, Awaiting endorsement · warning, Upheld · success, Not upheld · default, Withdrawn · default, Late · error.
Promotion: Proposed · default, Needs decision · warning, Re-test pending · warning, Confirmed · success, Sent to rollover · info. Outcomes: Promoted · success, Weak pass · warning, Conditional · warning, Detained · error, Retained on request · warning, Board result pending · info.
Predicted: Draft · default, Submitted · info, Released · success, Under appeal · warning, Revised · info.

## 4. Rules that change the UI

1. **Template, then scheme** (DL-87, Confirmed). The scheme editor shows each component and weight, and marks what differs from the GSG template. Weights in a term must add up to 100 or Submit is disabled. An Academic Supervisor approves; an approved version is read-only. A change is a new version from a later year: v3 (AY 2027-28) is Submitted while v2 stays in use for 2026-27.
2. **Calendar** (DL-88). Assessments sit in exam windows; each shows date, subject, syllabus-sent date and status. The calendar engine itself is another module.
3. **Mark entry** (SR-AS-04, SR-AS-05). Teachers see only their own subjects and classes. Each student needs a mark (0 to max), or a code: Absent, Approved absence, Exempt, NA. Submit is disabled until all are filled. Out-of-range marks are blocked inline.
4. **Moderation and lock** (DL-88). The coordinator moderates submitted sheets (can return with a note), then locks. After lock, a change needs a reason (appeal, re-test, correction) and keeps history.
5. **Publish marks** (DL-93). After lock the coordinator publishes an assessment's marks to parents; this starts the appeal window.
6. **Grades are calculated** (DL-89). Results grids show component marks, total and grade; there is no "edit grade" control. To change a grade, correct the mark.
7. **Proration** (DL-95, HB p.43). An approved absence can be prorated only with the Principal's approval. Never show the reason; it lives in Attendance.
8. **Release report cards** (DL-90). Release per grade and term once every result is Final. A released card is read-only; a corrected result creates "v2" and notifies parents. Show "Seen by parent" counts.
9. **Fees never block** (DL-83, Confirmed). Never show fee status on any assessment screen, and never hold a card for fees.
10. **Appeals** (DL-91). The parent's Appeal button shows until the window closes (GIIS SG CBSE 6-8: 2 working days after scripts are returned). After that: "Results accepted". One appeal per mark, no second appeal. Staff see a queue with response due dates. Outcome: upheld (mark changes, card reissued if released) or not upheld.
11. **Promotion** (DL-92, DL-75). The rule proposes; anything needing judgment (below attendance threshold, detention, retention) is "Needs decision" and the Principal decides. Re-tests show their date. Only confirmed decisions go to the Classes rollover. GIIS SG shows only a mid-year "watch list" now.
12. **Predicted grades** (DL-94). Teachers see the calculated basis next to their prediction. The coordinator releases. An appeal opens a committee case headed by the Principal.
13. **Settings, no group default.** Appeal windows, release deadline, attendance thresholds, student access: show the value or "Not set".
14. **No delete.** Cancel an assessment, supersede a scheme or card, withdraw an appeal.

## 5. Who sees what (summary)

Principal: everything at the campus; approves proration, decides detention and retention, chairs predicted-grade appeals. Academic Supervisor: approves scheme versions. Coordinator: calendar, moderation, lock, publish, release, appeals, promotion review for their band. Subject teacher: own subjects and classes, enter marks, re-mark appeals. Class teacher: all subjects of own class (read), report card remarks. Parent: published marks, released report cards, appeals for own children; no fees here. Student: own released report cards only if the campus allows (pending OQ-96). GSG platform team: templates.

## 6. Pending decisions (show "Pending decision")

OQ-93 who approves a brand-country scheme · OQ-94 report card layouts · OQ-95 does release need the Principal · OQ-96 student access · OQ-97 which appeal window applies to which grades · OQ-98 who the Examination Board is, who moderates · OQ-99 who confirms straight promotions, how re-tests combine · OQ-100 board results import · OQ-101 online exam portal import · OQ-102 mid-year joiners · OQ-103 non-GIIS schemes · OQ-104 PYP (Toddle) and KG HPC · OQ-105 who enters co-scholastic grades · OQ-106 retention and MyGIIS history.

## 7. Suggested screens

1. Coordinator overview (to-dos: sheets to moderate, sheets overdue, appeals due, scheme waiting) · 2. Assessment calendar (Grade 7 Term 2) · 3. Mark sheet moderation and lock · 4. Teacher mark entry (phone) · 5. Results grid 7B Term 1 · 6. Report card release + card preview (Aarav) · 7. Appeals queue and detail · 8. Scheme editor with template differences and approval · 9. Promotion review (OWIS Grade 4) · 10. Predicted grades (SG-ECS) · 11. Parent: marks, report cards, appeal.

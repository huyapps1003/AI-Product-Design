# Starter prompts for Claude Design

Attach `1-design-brief.md`, `2-sample-data.json` and `2-sample-directory.csv` to the Claude Design project first, then paste Prompt 1. Use the follow-up prompts one screen at a time.

---

## Prompt 1 — first prototype (paste this first)

```
You are designing a clickable prototype for Global Campus OS, the school platform that Global Schools Group (GSG) runs for its school brands. This prototype covers the Student Directory and the Admission and Enrolment module. GIIS is the example brand.

Read the attached files before designing:
- 1-design-brief.md: vocabulary, entities, status chips and colours, who sees what, rules that change the UI, and what is still undecided.
- 2-sample-data.json: synthetic students, guardians, enrolments, cases and staff users. Use only these records; don't invent other students.
- 2-sample-directory.csv: the same students as a flat table.

Build a desktop staff web app prototype with these screens, linked together:
1. Student directory: a table of all 14 students with search, a campus switcher (SMART Campus Punggol, East Coast Campus, Noida Campus, All campuses) and filters for grade, class, segment, programme and enrolment status. Columns: photo/initials + name + Student ID, grade · class, programme, campus, enrolment status chip, guardians with verification state, open case.
2. Student profile, opened from a row. Header: photo, name, Student ID with a copy button, record and enrolment status chips, current campus and class. Tabs: Overview, Enrolment history (timeline, ended enrolments kept), Guardians, Contact and address, Documents (restricted), External IDs, History.
3. Medical alert and AEPP: on the profile show a "Medical alert" flag with no detail where the sample has one (DL-66), and show AEPP as a placement note on the regular enrolment, never a separate enrolment (DL-65).

The viewer is Lina Goh, Admissions Officer at SMART Campus Punggol. Today is 6 November 2026.

Rules to follow:
- Use the words in section 2 of the brief: "Student ID", "Class" (like 7B), "Segment" (Pre-Primary, Primary, Middle, Higher). Never write "Section", "admission number" or "cell" (say "Data location"). Student ID format is decided (DL-67): use the sample IDs as they are.
- Use the status labels and chip colours from section 4. Every chip has a text label.
- Restricted (Tier 3) fields such as passport numbers and immigration status show masked (•••••) with a lock icon and a "Reveal" action that says "This view is recorded".
- No "Delete" on enrolments, guardians or addresses: use "End…" and "Show history".
- Don't design anything listed in section 7 of the brief as settled. Use a neutral "Pending decision" placeholder instead.
- Use the design system attached to this project. Public Sans; sidebar 280px; header 64px; content max width 1200px.
```

---

## Follow-up prompts (one at a time)

**Year-end promotion across stage campuses (Sokha Chan)**
```
Add a year-end promotion preview for Harrods Phnom Penh. Sokha Chan moves from the Lower Primary campus (KH-HLP) to the Upper Primary campus (KH-HUP) with the new academic year. Show it as a promotion (PROMOTION_ROLLOVER), not a transfer, with no extra approval (DL-62). Add one example of a year-end move to another brand, flagged "Starts a new admission" (DL-63). The campus transfer flow must refuse a year-end stage move (SR-16).
```

**Complete enrolment (Daniel Lee)**
```
Add a "Complete enrolment" flow for Daniel Lee (Confirmed, IBDP Year 1, East Coast Campus). Steps: confirm details → assign class and house → subject choices → guardians and verification → preview of what gets created (student login, parent app access for verified guardians, timetable and bus route to follow) → done. Block "Complete" until the cooling-off period has ended on 12 Nov 2026, and show why. Follow rule SR-07 in the brief.
```

**Campus transfer case (Rohan Mehta)**
```
Add the campus transfer case for Rohan Mehta from SMART Campus Punggol to East Coast Campus. Show both approvals (Dr Anita Rao approved on 30 Oct; Suresh Pillai pending), an SLA countdown to 17 Nov 2026, and a preview of the new enrolment. Make clear the Student ID stays GS-2021-000455. Add a notice that a new contract is needed only when the programme changes. Then show Kavya Nair's profile as an example of a completed transfer in the enrolment timeline.
```

**Guardian verification queue**
```
Add a guardian verification queue for Admissions. Maria Fernandes (mother of Sara Fernandes) is Pending verification. Show that she has "No parent app access yet" until verified, with Verify and Reject actions. Rejecting asks for a reason.
```

**Student Pass cases (Admissions only)**
```
Add a Student Pass list for Admissions at SMART Campus Punggol: passes expiring within 60 days (Yuki Watanabe, 15 Dec 2026), open cases (Yuki's renewal waiting for a photo; Daniel Lee's application in review). Then show the same student profile (Aarav Sharma) as Farah Ismail, class teacher of 7B: she sees "Attendance threshold 90%" with no reason and no Student Pass or immigration fields.
```

**Role views**
```
Add a "View as" switcher at the top of the student profile with: Admissions Officer, Principal, Class teacher (own class), Subject teacher, Nurse, Transport executive. Apply the visibility table in section 5 of the brief to every field group: edit, view, masked with lock, or hidden.
```

**MyGIIS sync conflict and external IDs**
```
On Riya Joshi's profile, show the MyGIIS sync conflict on her address: a "Conflict" chip and a side-by-side compare with "Keep Campus OS" and "Accept MyGIIS" actions; never overwrite automatically. On Kabir Singh's External IDs tab, show APAAR as "Refused by parent" with the refusal letter, and Riya's as "Not generated".
```

**Parent app (mobile)**
```
Add a mobile parent app for Neha Sharma. "My children" shows Aarav (SMART Campus Punggol, 7B) and Ishita (East Coast Campus, 9A) as two cards. "Update profile" lets her edit passport details, immigration status, addresses and contacts for each child and for both parents, like MyGIIS Profile Update. After she submits, show a "Sent to school" state.
```

---

## Review checklist before sharing a prototype

- No "Section", no "admission number", no invented students.
- Student ID is the same before and after every transfer.
- Restricted fields masked with a lock for everyone except Admissions.
- Class teachers never see Student Pass or immigration status.
- Open items (section 7 of the brief) shown as "Pending decision", not as designed features.
- No legal entity named as data controller anywhere.

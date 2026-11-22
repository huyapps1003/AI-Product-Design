# Starter prompts for Claude Design (Cases & Communications)

Attach `1-design-brief.md` and `2-sample-data.json`, then paste Prompt 1.

## Prompt 1 — first prototype

```
You are designing a clickable prototype of the Cases & Communications module of Global Campus OS, the school platform Global Schools Group runs for its brands. GIIS SMART Campus Punggol is the example campus.

Read the attached files first:
- 1-design-brief.md: words, chips, rules that change the UI, who sees what, pending decisions.
- 2-sample-data.json: cases, case policies, feedback, conversations, announcements, notifications. Use only these records.

The viewer is Priya Menon, Customer Relations Officer at SMART Campus Punggol (SG-PGL), on desktop. Today is Monday 23 November 2026, 09:30.

Build these linked screens:
1. Case inbox with tabs Mine, My queues, Overdue, Waiting for family, All I can see. Overdue first, then next due date. CASE-SG-2026-1297 (PTM link, from the helpdesk) is New and Overdue: acknowledgement was due Fri 20 Nov. CASE-SG-2026-1281 (air-conditioning 5C) is due today. CASE-SG-2026-1259 (late payment charge) is With committee. One quiet line says 2 restricted cases are not shown.
2. Case detail for CASE-SG-2026-1297: deadline ladder Acknowledge (Fri 20 Nov, missed) → Reply and close (Fri 27 Nov) → Supervisor (Tue 8 Dec) → Dispute committee (Thu 17 Dec). Timeline, a composer with "Reply to family" and "Internal note" modes, people and approvals. "Acknowledge and take it" sends the reply date to the family.

Rules:
- Deadlines are dates counted in working days; Deepavali 9–10 Nov is skipped.
- Overdue is a flag beside the status, not a status.
- Internal notes are amber and never go to the family.
- Anything in section 6 shows "Pending decision".
- Use the attached design system (MinV). Public Sans.
```

## Follow-ups (one at a time)

**Log feedback**
```
Add "Log feedback" for things that arrive by email, phone, letter, helpdesk or in person. Fields: type (complaint, compliment, suggestion), topic, from, received by, received on (day 0), what they said. A live preview shows the four complaint deadlines as dates and who will own it by topic (topics and owners: Pending decision OQ-133). Compliments and suggestions only show "acknowledge by" and go to the term review.
```

**Restricted case and waiting**
```
Switch to Dr Anita Rao. Open CASE-SG-2026-1270, a complaint about comments in a Grade 6 class, Restricted. It is Waiting for family: clock paused since Wed 11 Nov, 8 working days so far; Reply and close now due Wed 25 Nov. "Family replied, resume" restarts the clock. Priya does not see this case at all.
```

**Approvals**
```
As Dr Anita Rao, open CASE-SG-2026-1285: course change CBSE to IGCSE Grade 9 at East Coast. Two approvals: Principal, originating campus (Dr Rao, approved 19 Nov) and Principal, receiving campus (waiting; East Coast principal has access to this case only). Decision due Mon 30 Nov (3 weeks, HB p.236).
```

**Teacher messages (phone)**
```
Switch to Farah Ismail on her phone. Messages: one thread per 7B student with their guardians. Neha Sharma's bus message was turned into complaint CASE-SG-2026-1296; the thread is read-only and links to it. Arjun Menon has an unread reply. In a thread, show "Turn into complaint" and a hint not to include health or immigration details (check: Pending decision OQ-136).
```

**Announcements**
```
As Farah: compose to Class 7B guardians, sends now; choosing Grade 7 changes the button to "Send for approval". As Meera: the Grades 6–8 mid-term timetable is Awaiting approval. As Dr Rao: approve it (approver: Pending decision OQ-138). Show read and confirmed counts on sent announcements.
```

**Case policies**
```
As Vivek Menon, show GIIS Singapore case policies: complaint 2 / 7 / 14 / 21 working days then CPE referral; withdrawal; campus transfer; refund (Finance Controller + Country Director); leave over 2 days. As Claire Tan (OWIS), the complaint policy is "Not set" (Pending decision OQ-143).
```

**Notifications and parent view**
```
As Priya: delivery log (one SMS failed and fell back to email; one newsletter skipped by preference) and templates (CaseAcknowledged, AttendanceAbsentUnexplained on WhatsApp, a restricted-case notice with no title). As Neha on a phone (Parent app, later): My requests with the bus complaint, its next reply date and replies only; notification settings where only Optional can be turned off.
```

## Review checklist

- [ ] Every deadline is a date, counted in working days.
- [ ] Overdue sits beside the status; overdue cases sort first.
- [ ] Restricted cases are absent for people not named; only a count line.
- [ ] Internal notes never appear in the family view.
- [ ] OWIS and GIIS India show "Not set", never GIIS numbers.
- [ ] Teachers message only their own class's guardians.
- [ ] Safety and Required notifications can't be muted.
- [ ] No Tier 3 details in notifications or messages.
- [ ] Open questions show "Pending decision".

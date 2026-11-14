# Starter prompts for Claude Design (Admissions)

Attach `1-design-brief.md` and `2-sample-data.json`, then paste Prompt 1.

## Prompt 1 — first prototype

```
You are designing a clickable prototype of the Admissions module of Global Campus OS, the school platform Global Schools Group runs for its brands. GIIS is the example brand.

Read the attached files first:
- 1-design-brief.md: words, chips, rules that change the UI, who sees what, pending decisions.
- 2-sample-data.json: enquiries, applications, capacity and campus settings. Use only these records.

The viewer is Lina Goh, Admissions Officer at SMART Campus Punggol (SG-PGL). Today is 12 November 2026. Desktop staff web app.

Build these linked screens:
1. Enquiry queue: the SG-PGL enquiries with status chip, children named, source, next action and owner. Show Sara Haddad's unrouted two-brand enquiry in an "Unassigned" tab with a "Pending decision (OQ-63)" note.
2. Pipeline board: columns Submitted, Documents pending, Assessment, Under review, Waitlisted, Offered, Accepted, Enrolled, with one card per SG-PGL application. Each card shows the child, grade and programme, start date, Student Pass/MOE badge and the next blocker. Hide Aditi Gupta (Noida), who is outside Lina's scope.
3. Application detail for Noah Fernandes, with tabs Overview, Documents, Assessments, Offer and contract, Fees, Approval case, History. Show the cooling-off countdown (ends 17 Nov 2026, "7 working days (campus setting)") and the ICA case Submitted.

Rules:
- Use the words and chip colours in the brief; never "Section", "lead" or "admission number".
- No Student ID before an offer (DL-57).
- Passport and birth certificate are masked with a lock and "Reveal (recorded)". The SEN outcome shows only the outcome code.
- Rule values are shown as campus settings. Anything in section 6 of the brief shows "Pending decision".
- No delete: use Withdraw, Rescind or Close with a reason.
- Use the attached design system (MinV). Public Sans; sidebar 280px; header 64px; content max width 1200px.
```

## Follow-ups (one at a time)

**Capacity and waitlist**
```
Add a Capacity screen for SG-PGL AY 2026-27: per grade show limit, enrolled, held, free, and the override buffer. Grade 7 CBSE is full. Open Li Wei Zhang (Waitlisted, position 3) and show the waitlist order rule from the brief (siblings, staff children, completion date). Show the AEPP placement result as a placement inside the Grade 7 enrolment (DL-65).
```

**Issue offer with override**
```
Add "Issue offer" for Li Wei Zhang into the full Grade 7. As Lina, the button is disabled with "Grade full: an Admissions Manager can override". Switch the viewer to Marcus Lee (Admissions Manager): allow the override within the buffer of 2 with a required reason; a third extra seat routes to the Principal for approval. Offer validity shows "Pending decision (OQ-66)".
```

**Sibling application**
```
Show Kavya Sharma's application with a "Possible existing family" match to Neha Sharma (guardian of Aarav Sharma). Staff confirm the match; never merge automatically (DL-58). Show "Sibling: first on waitlist" as a badge.
```

**SEN pre-admission**
```
Open Arjun Pillai (Under review, MOE approval needed). The SEN evaluation shows only "Needs discussion" with a lock and "Open in Wellbeing (counsellor only)". The decision step (Offer / Waitlist / Reject) shows "Who decides: Pending decision (OQ-66)".
```

**Rejected Student Pass**
```
Open Hana Kim: ICA rejected the Student Pass on 2 Nov 2026. Show the enrolment Cancelled, the contract Terminated and the registration fee Refunded less the admin charge, citing the GIIS Singapore campus setting.
```

**Enrolment handoff**
```
Open Ravi Iyer (Enrolled, class 6C). Show the handoff summary: Student ID, class, timetable and bus to follow, student login, and parent app access only for verified guardians. Link to his Student Directory profile.
```

**Campus settings "Not set"**
```
Add Campus admission settings for SG-PGL and IN-NOI side by side: cooling-off days, fees at admission, required documents, required assessments, offer validity. IN-NOI shows "Not set" for cooling-off; opening Aditi Gupta's application as Pooja Malhotra shows the contract step blocked with "Not set: ask your Admissions Manager".
```

## Review checklist

- [ ] Only sample records used; Aditi Gupta hidden from Lina.
- [ ] No Student ID before the offer; Sofia Rossi and later have one.
- [ ] Every rule value labelled as a campus setting; IN-NOI shows "Not set".
- [ ] T3 data masked; SEN shows the outcome only.
- [ ] Capacity: officer blocked, manager buffer, Principal beyond.
- [ ] Pending decisions shown as such, never invented.
- [ ] No "Section", "lead", "admission number"; no Delete.

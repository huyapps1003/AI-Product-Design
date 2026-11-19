# Starter prompts for Claude Design (Attendance)

Attach `1-design-brief.md` and `2-sample-data.json`, then paste Prompt 1.

## Prompt 1 — first prototype

```
You are designing a clickable prototype of the Attendance module of Global Campus OS, the school platform Global Schools Group runs for its brands. GIIS SMART Campus Punggol is the example campus.

Read the attached files first:
- 1-design-brief.md: words, chips, rules that change the UI, who sees what, pending decisions.
- 2-sample-data.json: school days and rules, the 7B register for today, gate log, leave requests, corrections, 7B summary, alerts, register board, parent view. Use only these records.

The viewer is Farah Ismail, class teacher of 7B at SMART Campus Punggol (SG-PGL), on her phone. Today is Thursday 19 November 2026, 10:20.

Build these linked phone screens:
1. Today's register for 7B: 32 students, submitted 09:12. Aarav Sharma Late 09:18 "from reception" (bus, excused). Diya Menon On leave (medical, certificate due). Ishaan Gupta and Ayaan Khan Absent; Ishaan shows "Parent notified 09:30"; Ayaan shows "8th school day in a row". Rehan Ali has an approved out pass for 11:30. Medical alert flags only, never details.
2. Inbox: Kiara D'Souza's 2-day leave to approve or decline; Manav Joshi's 5-day leave "With the Principal" (read only); Diya's certificate due; alerts for Fatima Noor (below threshold) and Sahil Verma (at risk).
3. 7B attendance: each student with %, threshold, best case and standing. Fatima "Threshold 90%, November 83.3%, can't reach threshold". Never show "Student Pass" or ICA.

Rules:
- Use the words and chip colours in the brief.
- Default everyone to Present; tap exceptions. After lock, changes need a reason and go to the coordinator.
- Count school days, not calendar days.
- Never show fee status. Anything in section 6 shows "Pending decision".
- Use the attached design system (MinV). Public Sans.
```

## Follow-ups (one at a time)

**Coordinator register board and corrections**
```
Switch to Meera Joshi on desktop. Show today's registers for 6A–8C: 7C "Not taken, overdue 65 min" (Grace Wong on a field trip; who covers is Pending decision OQ-110), 8C "No class teacher". Corrections to approve: Sahil Verma 17 Nov Absent → Late with reason. Approving updates his % and standing.
```

**Reception gate log**
```
Switch to Siti Rahman at the reception desk. Today's log: late sign-ins (Aarav 09:18 bus, excused; Tara Menon 5A 09:26, 4th late this term, next step "note to parents from Level Coordinator"; Kunal Shah 8A doctor's note) and out passes (Ava Lim 6B 10:05, collected by mother; Rehan Ali 7B 11:30 approved, not signed out yet). Add a late sign-in in two taps: student, time (now), reason, exemption.
```

**Principal leave decision**
```
As Dr Anita Rao, open Manav Joshi's leave: 30 Nov – 4 Dec, 5 school days, travel, planned. Show the reason, his current attendance and threshold, and Approve / Decline (decline needs a note to the parent). The class teacher and coordinator are informed.
```

**ICA review (Tier 3)**
```
As Lina Goh (Admissions), open CASE-SG-2026-1291: Ayaan Khan, 8 consecutive school days absent (6, 11–13, 16–19 Nov) with no valid reason. Steps: check with the family, file with ICA if no valid reason (how: Pending decision OQ-112), record filing date and reference. Also show Fatima Noor as "Watch: November may end under 90%".
```

**School days and rules**
```
As Dr Anita Rao, show the AY 2026-27 school-day calendar for CBSE at SG-PGL: semesters, public holidays, breaks, 196 school days (matches the published schedule). Show campus rules: Student Pass 90% per month, Class 6+ 85% per semester, KG–5 75% per term (Principal may relax), more than 7 consecutive days → ICA review. OWIS SG-OWD rules show "Not set".
```

**Parent view**
```
Switch to Neha Sharma on a phone. Aarav today: Late 09:18, school bus delayed, excused. November 12 of 12 days, Semester 2 %, threshold 90% per month. "Apply for leave": dates, type, reason; medical asks for a certificate on return; the screen says who will approve based on school days.
```

## Review checklist

- [ ] Teachers and coordinators never see "Student Pass" or ICA; only "Threshold 90%".
- [ ] Days are school days; holidays skipped.
- [ ] Locked registers change only with a reason.
- [ ] Leave over 2 school days goes to the Principal.
- [ ] Teacher sees "Certificate received", never the file.
- [ ] No fee status anywhere.
- [ ] Settings show a value or "Not set".
- [ ] Open questions show "Pending decision".

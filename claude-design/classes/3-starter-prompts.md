# Starter prompts for Claude Design (Classes)

Attach `1-design-brief.md` and `2-sample-data.json`, then paste Prompt 1.

## Prompt 1 — first prototype

```
You are designing a clickable prototype of the Classes module of Global Campus OS, the school platform Global Schools Group runs for its brands. GIIS SMART Campus Punggol is the example campus.

Read the attached files first:
- 1-design-brief.md: words, chips, rules that change the UI, who sees what, pending decisions.
- 2-sample-data.json: classes, class plans, the 7B roster, the needs-a-class queue, placement history and rollover runs. Use only these records.

The viewer is Dr Anita Rao, Principal of SMART Campus Punggol (SG-PGL). Today is 14 November 2026, in AY 2026-27. Desktop staff web app.

Build these linked screens:
1. Classes overview for SG-PGL AY 2026-27: grouped by programme and grade, one card per class with name, class teacher, room and a fill bar ("29 of 32"). 7B and 7C are Full. 8C shows "No class teacher" as a warning.
2. Class detail for 7B with tabs Roster, Staff, History. The roster shows name, house, Medical alert flag and attendance threshold only. Hiroshi Tanaka joined mid-year on 7 Oct 2026.
3. Class plan: each grade with classes × size limit = seat limit, plus Admissions enrolled and held. AY 2027-28 CBSE Grade 6 (4 × 32) is Submitted and waiting for Dr Rao to approve. CLSP Grade 6 has size limit "Not set" and can't be approved.

Rules:
- Use the words and chip colours in the brief; never "Section", "homeroom" or "strength".
- Size limits are campus settings; "Not set" blocks the step.
- Rosters never show health detail, Student Pass or passport.
- Anything in section 6 of the brief shows "Pending decision".
- No delete: close a class, end a placement, cancel a run.
- Use the attached design system (MinV). Public Sans; sidebar 280px; header 64px; content max width 1200px.
```

## Follow-ups (one at a time)

**Needs a class**
```
Add a "Needs a class" queue: Noah Fernandes (Confirmed from Admissions, CBSE Grade 6, starts 5 Jan 2027, suggested 6B) and Mateo Garcia (IB PYP Grade 3, suggested 3C). Placing pre-places from the start date. Show why each class is suggested.
```

**Class move and full class**
```
As Meera Joshi (Academic Coordinator), move a student from 7A to 7B. 7B is full (32 of 32), so the move is blocked with "Send to Principal" and a required reason. Switch to Dr Rao and approve it; the class shows "33 of 32 · Over limit (approved)". The reason note is visible to the coordinator and Principal only. Who may move students is "Pending decision (OQ-73)".
```

**Rollover (OWIS)**
```
Switch the viewer to Claire Tan, Principal of OWIS Digital Campus (SG-OWD). Its year ends in November. Show the AY 2026 → AY 2027 rollover for 4 Orchid: 14 promoted, Nora Ang retained (needs decision), Ryan Goh conditional (re-test pending), Owen Lee leaving (excluded), Priya Raman moving to GIIS (excluded: new admission via Admissions). Approval is blocked until the two decisions are made. Then show the run Applied.
```

**Stage-campus promotion**
```
Open the applied Harrods rollover: Sokha Chan moved from KH-HLP Year 4 to KH-HUP Year 5. Show "Stage-campus promotion: no extra approval needed (DL-62)". Contrast with a non-stage campus change, which needs both principals.
```

**Houses**
```
Add a Houses screen for SG-PGL: House A–D with colour and student counts, and students without a house. Allocation rules show "Pending decision (OQ-76)". Houses carry over between years.
```

**Class teacher on phone**
```
Switch the viewer to Farah Ismail (Class teacher, 7B) on a phone. She sees only 7B: roster, houses, flags, and today's attendance shortcut. No other classes, no class plan, no move action.
```

**Admissions view**
```
Switch to Lina Goh (Admissions). The class plan is read-only with "Seat limit set by the Principal's class plan". Show whether Ravi Iyer (6C) and Noah Fernandes (not yet placed) have a class.
```

## Review checklist

- [ ] Only sample records used.
- [ ] Never "Section"; classes are per year.
- [ ] Seat limit = class plan; Admissions read-only.
- [ ] Full class blocked; Principal approval logged.
- [ ] Class move keeps the enrolment; reason note restricted.
- [ ] Rosters show flags and thresholds only.
- [ ] Rollover: outcomes read-only, decisions block approval, brand moves excluded.
- [ ] "Not set" and "Pending decision" shown, never invented.
- [ ] No Delete.

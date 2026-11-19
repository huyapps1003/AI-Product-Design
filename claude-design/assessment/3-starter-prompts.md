# Starter prompts for Claude Design (Assessment & Report Cards)

Attach `1-design-brief.md` and `2-sample-data.json`, then paste Prompt 1.

## Prompt 1 — first prototype

```
You are designing a clickable prototype of the Assessment & Report Cards module of Global Campus OS, the school platform Global Schools Group runs for its brands. GIIS SMART Campus Punggol is the example campus.

Read the attached files first:
- 1-design-brief.md: words, chips, rules that change the UI, who sees what, pending decisions.
- 2-sample-data.json: schemes, the Grade 7 Term 2 calendar, mark sheets, 7B Term 1 results, report cards, appeals, promotion decisions, predicted grades. Use only these records.

The viewer is Meera Joshi, Academic Coordinator for CBSE Grades 6-8 at SMART Campus Punggol (SG-PGL). Today is 15 November 2026, Term 2 of AY 2026-27. Desktop staff web app.

Build these linked screens:
1. Overview: to-do cards (Grade 7 Hindi L2 PT2 waiting for moderation, French L2 ready to lock, Tamil L2 and 8B/8C Maths still open (due 17 Nov), Ishaan Gupta's appeal due 23 Nov, scheme v3 waiting for the Academic Supervisor) and the Grade 7 Term 2 calendar (PT2 ICT published, L2 marking, Maths 17 Nov next).
2. Mark sheet: Grade 7 PT2 Hindi (L2) submitted by Pooja Sharma; moderate, return with a note, or lock. After lock, publish marks to parents.
3. Results grid for 7B Term 1: one row per student, per subject PT1, Notebook, Subject enrichment, Half Yearly, total and calculated grade. Sahil Verma's E in Mathematics shows as an error chip. Ananya Iyer's Science shows "Reissued v2 after appeal". No edit-grade control.

Rules:
- Use the words and chip colours in the brief; never "transcript" or "pass/fail".
- Grades are calculated, never typed. Absence shows a code, never a reason.
- Never show fee status anywhere.
- Anything in section 6 of the brief shows "Pending decision".
- No delete: cancel, supersede, withdraw.
- Use the attached design system (MinV). Public Sans; sidebar 280px; header 64px; content max width 1200px.
```

## Follow-ups (one at a time)

**Teacher mark entry (phone)**
```
Switch to Arvind Menon on a phone. Open Grade 8 PT2 Mathematics for 8B (out of 20, due 17 Nov). Five students have no mark yet; Harish Jain has "Approved absence" (proration needs the Principal). A mark of 22 is blocked inline. Submit stays disabled until every student has a mark or a code. He sees only his own classes.
```

**Report card release and preview**
```
As Meera, show Term 1 releases: Grade 6, 7, 8 released with "Seen by parent" counts. Open Aarav Sharma's Term 1 card (7B): subjects with component marks and grades, co-scholastic grades, attendance 96 of 101 days, class teacher remark. Layout is "Pending decision (OQ-94)". Show Ananya Iyer's card history: v1 superseded, v2 released after an upheld appeal.
```

**Parent view and appeal**
```
Switch to Neha Sharma on a phone. She sees Aarav's published PT2 ICT mark (18/20) with "Appeal until 16 Nov", upcoming PT2 Maths (17 Nov, syllabus sent) and his report cards. No fees, no unpublished marks. Raising an appeal asks for a reason and shows the response time.
```

**Appeals queue**
```
As Meera, show appeals: Ishaan Gupta (PT2 ICT, with teacher, due 23 Nov), Ananya Iyer (upheld 44 → 48, C1 → B2, card reissued), Kabir Malhotra (not upheld), Ethan Lim (late, rejected: results accepted). "Examination Board" endorsement shows "Pending decision (OQ-98)" for who sits on it.
```

**Scheme editor**
```
Switch to Dr Suresh Pillai (Academic Supervisor). Show the GIIS Singapore CBSE 6-8 scheme: v2 approved for AY 2026-27 (Term 1 and Term 2 components and weights, CBSE 6-8 grade bands, promotion rule) with "Differs from template" notes, and v3 for AY 2027-28 submitted by Meera with its change summary. He can approve or return v3. Weights add up to 100 per term.
```

**Promotion review (OWIS)**
```
Switch to Claire Tan, Principal of OWIS Digital Campus. Grade 4, AY 2026: 15 confirmed and sent to the rollover, Nora Ang proposed Detained (needs her decision), Ryan Goh conditional with a Maths re-test on 24 Nov, Owen Lee excluded (leaving). Deciding Nora's line sends it to the Classes rollover. OWIS rules are synthetic (OQ-103).
```

**Predicted grades**
```
Switch to Rahul Bose (IB DP Coordinator, SG-ECS). IB Year 2 predicted grades released 2 Oct. Arjun Nair's Physics HL (basis 5.4, predicted 5) is under appeal: committee of Principal, Academic Supervisor, IB Coordinator and Career Counsellor meets 20 Nov.
```

## Review checklist

- [ ] Only sample records used.
- [ ] Teachers see only their own subjects and classes.
- [ ] Submit blocked until every student has a mark or code; out-of-range blocked.
- [ ] Grades calculated, no grade editing; changes after lock need a reason.
- [ ] Parents see only published marks and released cards; no fee status anywhere.
- [ ] Released cards read-only; corrections reissue v2.
- [ ] Appeal button ends at the window; one round.
- [ ] Promotion: judgment cases need a decision; only confirmed go to the rollover.
- [ ] "Not set" and "Pending decision" shown, never invented.
- [ ] No Delete.

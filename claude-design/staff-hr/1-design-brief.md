# Campus OS — Staff & HR: design brief for Claude Design

**Built from:** `product-truth/03-domain/staff-hr-data-dictionary.md` v0.1, OBJ-05, OBJ-06, OBJ-28…OBJ-31 cards, Teaching (DL-143, DL-156), Cases (DL-125), GIIS Student Handbook 2025-26 (HB) p.138, p.210–213
**Sample data:** `2-sample-data.json`. Where this brief and the dictionary disagree, the dictionary wins.

## 1. Context

- Global Campus OS is the school platform GSG runs for all its brands. GIIS Singapore (SMART Campus Punggol SG-PGL, East Coast SG-ECS) is the example; OWIS Singapore shows "Not set".
- Scope (Henry, 25 Nov 2026, DL-146 "Full HR"): **who works for the school, on what terms, when they are away, whether they are cleared to work with children, how they are hired, start and leave, and how they are reviewed and trained.** Pay, salary and bank details stay in payroll and never appear.
- Main viewer: **Nadia Rahman, HR Business Partner for GIIS Singapore, on desktop.** Today is **Wednesday 25 Nov 2026, 09:15**. Other viewers: Dr Anita Rao (Principal SG-PGL: approves long and term-time leave, vacancies, offers; moderates appraisals), Meera Joshi (line manager of six teachers: approves their leave), Farah Ismail (teacher: self-service on her phone), Vivek Menon (Country Director: approves new headcount), Claire Tan (OWIS).
- HR, Principal and line manager screens are desktop. A staff member's own leave, balance and training are phone first.

## 2. Words

| Use | Meaning | Don't use |
|---|---|---|
| **Staff** / **staff member** | Anyone employed, teaching or not | Employee (in UI), worker, resource |
| **Line manager** | Who the person reports to; approves leave, leads appraisal (DL-150) | Supervisor, boss, reporting officer |
| **Away** | What colleagues see when someone is on leave; never the type (DL-156) | Sick, on MC, on leave (to colleagues) |
| **Leave** | Time off of a type: Annual, Sick, Childcare… The type is seen by the person, line manager, Principal and HR only | Absence (that's Teaching's word) |
| **Balance** | Days left this leave year, calculated (DL-155) | Quota, credit |
| **Compliance status** | One chip: **Cleared**, **Due soon**, **Action needed**, **Not cleared** (DL-158) | Vetting, compliant/non-compliant |
| **Check** | Background or police check, work pass, training certificate, qualification | Document (too vague) |
| **Vacancy** | An approved post to hire for (DL-159) | Requisition, job (in UI) |
| **Starts {date}** / **Leaves {date}** | A joiner before the first day / a leaver with an end date (DL-160) | Onboarding, offboarding (in UI) |
| **Handover** | What a leaver's work passes to: groups, class, cases | Transition |

## 3. Chips

Employment: Starts {date} · info, Active · success, Leaves {date} · warning, Suspended · warning, Left · default.
Compliance status: Cleared · success, Due soon · warning, Action needed · warning, Not cleared · error.
Check: To verify · info, Valid · success, Expires {date} · warning, Expired · error, Waived · default.
Leave: Waiting for {name} · info, Approved · success, Declined · error, Cancelled · default, Certificate due · warning.
Vacancy: Waiting approval · info, Open · success, On hold · warning, Filled · default.
Checklist task: Overdue · error, Done · success.
Tier 3 field: lock icon with "Restricted" and who can see it.

## 4. Rules that change the UI

1. **One record per person** (DL-148). Sunita Verma, a Hindi candidate, is already a parent at SG-PGL: show "Matched to an existing person" on her application, never a second profile.
2. **Who sees what** (DL-151). Directory fields (name, photo, position, department, campus, work contact) for all colleagues. Personal details for HR. Identity numbers, work pass and background check results and medical certificates show a lock unless the viewer has Tier 3 access; Nadia has it in this sample.
3. **Campus OS owns the whole staff record** (DL-147, Confirmed). Every field is edited here, nothing is locked to another system. Farah's Employment tab shows "Sent to payroll" for the changes payroll receives. No salary anywhere.
4. **Leave routing** (DL-154). Line manager first; the Principal also when longer than 3 days (sample) or a teacher's annual leave falls on school days. Lakshmi Rao's 2 school days need Meera, then Dr Rao. Each request shows the balance after and how many lessons it affects.
5. **Leave tells Teaching only "Away"** (DL-156). Arvind Menon reported sick at 07:32 yesterday (after the 07:00 cut-off: show "Reported late"). It created his absence on the cover board at once. Colleagues and coordinators see "Away"; Meera, Dr Rao and HR see "Sick leave · Certificate due 27 Nov". Show a line on approvals: "Approving creates cover for 11 lessons".
6. **Compliance status gates work, never fires** (DL-158). Nikhil Sood's work pass expired on 20 Nov: Not cleared. He keeps his lessons but can't get new teaching groups or cover, and an HR case is open; show that, not "suspended". Daniel Koh (starts 4 Jan) can't start until his background check clears. Ananya's first aid expired: Action needed, not blocking. Alerts at 90, 60 and 30 days.
7. **Recruitment** (DL-159). Vacancy needs approval (Principal; Country Director for new headcount). Interviewers see others' feedback only after submitting their own. An accepted offer creates "Starts {date}" and the joining checklist.
8. **Leaving** (DL-160). Jason Teo leaves 18 Dec: his page lists every handover item from other modules (8B and 8C Mathematics, co-class teacher 8B, two parent conversations, an unlocked mark sheet) with an owner each.
9. **Appraisal is private** (DL-161). Seen by the person, reviewer, Principal and HR; ratings hidden until shared; scale "Not set".
10. **Staff concerns never show here** (DL-163). A concern about a staff member is a restricted case; a suspended person shows "Suspended" with no reason.
11. **Settings, no group default.** Entitlements, checks, thresholds and the appraisal scale show the value or "Not set" (OWIS).

## 5. Who sees what (summary)

Staff member: own record, leave, balance, checks, training, appraisal. Colleagues: directory and "Away". Line manager: team leave with type, compliance chips, appraisal as reviewer. Principal: campus staff, approvals, moderation; Tier 3 only with a grant. HR: everything in the brand-country; Tier 3 with a grant. Coordinator: "Away" and compliance chip when assigning teachers. Group leadership: counts only.

## 6. Pending decisions (show "Pending decision")

OQ-166 go-live date and payroll feed per brand-country · OQ-167 concurrent employments · OQ-168 sick report cut-off · OQ-169 entitlements · OQ-170 teachers' leave on school days · OQ-171 Singapore checks · OQ-172 India checks · OQ-173 who sees Tier 3 · OQ-174 vacancy and offer approvers · OQ-175 hiring stages · OQ-176 candidate retention · OQ-177 appraisal timing and reviewer · OQ-178 rating scale · OQ-179 training targets · OQ-180 training budget · OQ-181 leaving process · OQ-182 who is a teacher's line manager · OQ-183 department structure · OQ-184 grievances · OQ-185 staff record retention · OQ-186 relief teachers · OQ-187 staff children benefits · OQ-188 self-service approvals · OQ-189 OWIS processes · OQ-190 Designated Person backup · OQ-191 India statutory registers.

## 7. Suggested screens

1. Staff directory with compliance chips and filters (Nadia) · 2. Staff profile, Farah (tabs: Overview, Employment, Leave, Compliance, Training, Appraisal) · 3. Leave approvals inbox with team calendar (Meera) · 4. Apply for leave and balance (Farah, phone) · 5. Compliance dashboard (Nadia) · 6. Recruitment board for VAC-034 · 7. Joining (Daniel Koh) and leaving (Jason Teo) checklists · 8. Appraisal cycle overview and Farah's appraisal.

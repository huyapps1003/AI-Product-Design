# Cross-module consistency review, 28 Nov 2026

**Status:** Review (findings Known, proposals pending Henry), 28 Nov 2026 · **Owner:** Henry
**Cites:** ten module dictionaries in `03-domain/`, `04-access/`, `07-decisions/decision-log.md`, `08-open-questions/open-questions.md`, previous review of 22 Nov 2026 (thread "Suggest next direction for Campus OS")
**Tóm tắt (VI):** Rà soát chéo mười từ điển module sau khi Cases & Communications, Teaching & Timetable, Staff & HR và Academic structure được merge: khóa ngoại, vai trò và quyền, đánh số DL/OQ/OBJ, thuật ngữ. Lỗi cơ học đã sửa; các điểm cần quyết định chuyển thành OQ-230…OQ-234.

Scope: Student Directory, Organisation, Admissions, Classes, Fees & Billing, Assessment, Attendance, Academic structure, Cases & Communications, Teaching & Timetable, Staff & HR, at the merge of PR #28. Checks were scripted (every `FK:` tag, every `Referenced by` list, every capability code, every `Assumed (DL-xx)` line) and then read by hand.

## What is healthy

- **Numbering.** DL-01…DL-164 and OQ-01…OQ-193 have no gaps or duplicates. OBJ cards exist for OBJ-01…16, 19, 20, 23, 25…31; OBJ-17, 18, 21, 22, 24 are registered without cards, as planned.
- **No orphan keys.** Every `FK:XX00` tag points at an entity a dictionary defines. The ~20 orphans from 22 Nov are gone.
- **Capabilities.** Every capability code used in object cards, dictionaries and screens is in the permission catalogue, and every catalogue code is used somewhere. The only codes outside it are settings (`tm.overrides.enabled`, `tm.scope.auto_derive`), the template `hr.admin.standard`, the DL-34 example `student.contact.view`, and `wellbeing.health_note.view` in Admin Platform scenario S5 (Wellbeing has no dictionary yet).
- **Decisions.** 157 of 164 DLs are Confirmed. Only DL-09, DL-13, DL-14, DL-15 and DL-21 stay Assumed (held 22 Nov). DL-109 and DL-111…DL-116 are Confirmed (22 Nov), not Assumed.

## Fixed in this PR (mechanical)

| # | Finding | Fix |
|---|---|---|
| F1 | 47 key columns named their target in words: `FK:User` (34), `FK:Case` (11), `FK:Class`, `FK:Contract` | Repointed to `FK:A01`, `FK:CC03`, `FK:CL01`, `FK:AD08`. `FK:Person`, `FK:Calendar`, `FK:File`, `FK:Wellbeing record` and `FK:setting definition` stay: no dictionary defines them yet (OQ-234; Calendar engine and Wellbeing later) |
| F2 | Five key columns said only `FK` | AS06, CL06, FB04 → `FK:AC07`; FB09 → `FK:FB08 / FB04`; HR11 → `FK:OR04 / E03 / E05` |
| F3 | 14 keys had no FK tag at all (composite unique keys and paired columns) | Tagged: AD05, AD06, AD07, AS09, AS10, AS12, AS13, AT01, CC09, CL02, CL07, FB02, HR05, HR17 |
| F4 | Eleven "Referenced by" lists were stale (for example E01 listed 3 of 31 referrers; CL01 listed none of its six) | Rewritten from the actual keys |
| F5 | Two confirmed decisions still had text placeholders: CL01 `home_room` "becomes FK:TE03" (DL-141) and AS01/AS02 free-text `grade_band` that AC04 "replaces" (DL-115) | Now `home_room_id FK:TE03` and `grade_band_id FK:AC04` |
| F6 | CC09 and AT01 both used the primary key name `calendar_id` | CC09 key renamed `working_calendar_id` (nothing pointed at it) |
| F7 | ~45 lines still said "Assumed (DL-xx)" for decisions Henry has confirmed (DL-18, 23…26, 47, 48, 50, 55…57, 60, 96, 98, 101, 112, 114…116, 120…122, 125, 127…130) | Now "Confirmed". Lines citing DL-27 as a *pattern* for other modules' enums, and "DL-59 pattern" rules, were left as they are |
| F8 | OQ-192 and OQ-193 rows had three of seven columns | Completed |
| F9 | "Data location" (DL-47) still showed as "data region" on the Organisation screen | Renamed on every label and in the sample regions list |
| F10 | "IBDP" in prose where DL-112 asks for "IB DP" (variation catalogue, ST02 example, assessment scale names, student-directory design brief and prompts) | Fixed. Codes (`IBDP`, `IBDP-1`, `FP-ECS-IBDP`) are identifiers and stay |
| F11 | Global Admin sample person was "Daniel Ong" in the organisation sample and screen but "Daniel Tan" in roles-personas and the Admin Platform slice. "Daniel Ong" is also the Facilities Manager in the cases sample | Global Admin is Daniel Tan everywhere; the Facilities Manager keeps Daniel Ong |
| F12 | Screen headers for Cases & Communications and Fees still listed their DLs as Assumed; OBJ-13 card said DL-47…DL-50 Assumed; 04-access README said DL-16…DL-21 Assumed | Updated |
| F13 | index.json said "cards OBJ-01..OBJ-14" | Lists the cards that exist |
| F14 | Role catalogue (25 Oct) didn't know 13 roles that later dictionaries name (brand-country academic head, head of department, admissions officer and manager, finance executive, HR brand-country, campus office…) | New table "School roles named by module dictionaries"; mapping to access roles left to the Admin Platform spec |
| F15 | Same-named records in different modules (leave request AT06/HR06, offer AD07/HR14, school day vs working day vs work pattern) | "Not to be confused" entries in the glossary |

## Proposals for Henry (not applied)

Each has an open question so designs keep the element generic until Henry decides. Candidate decision IDs DL-190…DL-199 are reserved for this review (DL-165…DL-189 belong to the Admin Platform spec).

| # | Proposal | Recommendation | OQ |
|---|---|---|---|
| P1 | **One public holiday list.** OR03, AT02 and CC09 each hold public holidays; staff leave counts them without a source | The brand-country holds the list (OR03); AT02, CC09 and leave read it, and campuses add their own closures | OQ-230 |
| P2 | **Period registers for choice groups.** AT03 is keyed by class; a lesson for a group drawn from several classes has no register | Period registers key to the dated lesson (TE11), day registers stay by class | OQ-231 |
| P3 | **Enrolment grade as a key.** ST02 stores `grade_level_code` text; every other module keys to AC02 | Add `grade_level_id FK:AC02` to ST02 (needs a v1.1 of the locked Student Directory) | OQ-232 |
| P4 | **One name per key.** `grade_id` / `grade_level_id`, `offering_id` / `programme_offering_id`, `subject_id` / `pack_subject_id` | Key columns carry the target's key name; rename in one PR with the samples | OQ-233 |
| P5 | **A home for Person.** Nine fields point at `FK:Person` but no dictionary lists Person's attributes | The Admin Platform (identity) dictionary defines Person next to A01 User Account | OQ-234 |
| P6 | **Held early decisions.** DL-09 (separation of duties), DL-13 (invite timing), DL-14 (overrides), DL-21 (delegation) are Admin Platform behaviour; DL-15 (language) is cross-cutting | Settle DL-09, 13, 14, 21 inside the Admin Platform spec; DL-15 as a one-line card | — |

## Not found / not checked

- Sample JSON files were checked only for the names fixed above, not field by field against the dictionaries.
- Older screens (app, employee-*, hr-dashboard) were not re-read; the 22 Nov re-sample covered them.
- 00-sources (Employee DD v1, Student DD v1) are sources and were not edited. Their E, ST and A entities now live in the module dictionaries (A01…A17 move to the Admin Platform dictionary).

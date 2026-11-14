# Campus OS — Student Directory: design brief for Claude Design

**For:** prototyping the Admission and Enrolment module and the Student Directory in Claude Design
**Built from:** `product-truth/03-domain/student-directory-data-dictionary.md` (merged 30 Oct 2026), `04-access/rbac-model.md`, `06-experience/admin-platform/rules-to-ui.md`, Architecture v1 § Admission to enrolment, GIIS Student Handbook 2025-26 (HB)
**Sample data:** `2-sample-data.json` (synthetic people, GIIS example brand)

This is a compact, design-ready digest. Where it and the full dictionary disagree, the dictionary wins.

---

## 1. Product context in five lines

- **Global Campus OS** is the school platform Global Schools Group (GSG) runs for all of its school brands. GIIS (Global Indian International School) is the example brand in all sample data. Never present a GIIS fact as true for every brand.
- Campuses in the sample: `SG-PGL` SMART Campus Punggol and `SG-ECS` East Coast Campus (Singapore), `IN-NOI` Noida Campus (India). Academic year AY 2026-27.
- Programmes: GMP (KG), CBSE 1–12, IB PYP, Cambridge CLSP 6–8, IGCSE 9–10, IBDP.
- Staff work in the **staff web app** (desktop). Parents use the **parent app** (mobile). Students use the **student app** (mobile, access still open).
- Students and staff sign in with Google Workspace SSO. Parents sign in with OTP.

## 2. Words to use (and not use)

| Use | Meaning | Don't use |
|---|---|---|
| **Student ID** | The number people see, e.g. `GS-2026-004817`. Unique across Campus OS, stays the same after a campus transfer (DL-31). The format is not decided (OQ-36): any consistent placeholder is fine. | Admission number, roll number |
| **Class** | A group of students like 7B (DL-29) | Section |
| **Segment** | Pre-Primary, Primary, Middle, Higher (DL-29) | Section, wing |
| **Enrolment** | A student placed in a campus, programme, grade and class for one academic year | Admission (that is the pipeline before it) |
| **Guardian** | Mother, father, legal guardian or local guardian | Parent (only in parent-facing copy) |
| **Family link** | A guardian's access to a child in the parent app | |
| **Campus transfer** | Same student, new enrolment at another campus | Re-admission |

UI copy is English in v1.

## 3. What a student record holds

The directory answers three questions: **who the student is, where they are enrolled, and who acts for them.**

| Entity | Key fields to show | Notes for design |
|---|---|---|
| **Student** | Student ID, legal first and last name, preferred name, date of birth, photo, nationality, home language, record status | Internal key `student_id` is never shown. Sex or gender has no agreed values (OQ-37): leave it out or show a generic field. |
| **Enrolment** (many per student) | Campus, academic year, programme, grade, segment, class, house, status, start date, end date, end reason, contract, linked previous enrolment | History is kept: transfers and promotions end the old enrolment and start a new one. |
| **Subject choices** | Subject, type (core, second language, third language, elective, support), valid from/to | e.g. second language Hindi, Tamil, French, Mandarin; third language Sanskrit (GIIS). |
| **Guardians** (many per student) | Name, relationship, can collect the student, emergency contact, call order, receives communications, verification status, preferred language | A guardian can have children on several campuses and countries. |
| **Addresses** | Type (current, international home), address, country, postcode | Student Pass holders need an international home address (SG). |
| **Contacts** | Channel (phone, email, WhatsApp), value, primary, verified | |
| **Identity documents** 🔒 | Type (passport, Student Pass), number, issuing country, issue and expiry dates, scan, verified by | **Restricted (Tier 3).** Masked by default. |
| **Immigration status** 🔒 | Country, status (citizen, PR, Dependent Pass, Student Pass), valid from/to, linked case | **Restricted (Tier 3).** Only the resulting attendance threshold leaves this area. |
| **External IDs** | Scheme (APAAR, CBSE registration, IB / Cambridge candidate number, MyGIIS legacy ID), value, status, sync status | APAAR can be "Refused" (with the parent's refusal kept) or "Not generated". Show those states, don't hide them. |
| **Student login** | Linked account, status | Created on enrolment. |

## 4. Status vocabulary and chip colours

MinV Label, `soft` variant, small. Never colour alone: always the text label.

| Object | Status → label · colour |
|---|---|
| Enrolment | OFFERED → Offered · info · CONFIRMED → Confirmed · secondary · ENROLLED → Enrolled · success · DEFERRED → Deferred · warning · TRANSFERRED_OUT → Transferred out · default · WITHDRAWN → Withdrawn · default · COMPLETED → Completed · default · CANCELLED → Cancelled · default |
| Student record | ACTIVE → Active · success · INACTIVE → Inactive · default · ARCHIVED → Archived · default |
| Guardian verification | PENDING → Pending verification · warning · VERIFIED → Verified · success · REJECTED → Rejected · error |
| External ID | ACTIVE → Active · success · PENDING → Pending · info · REFUSED → Refused by parent · warning · NOT_GENERATED → Not generated · default · RETIRED → Retired · default |
| Sync (MyGIIS) | PENDING → Syncing · info · SYNCED → Synced · success · FAILED → Sync failed · error · CONFLICT → Conflict · warning |
| Case (transfer, withdrawal, Student Pass) | OPEN → New · info · IN_REVIEW → In review · secondary · WAITING → Waiting for info · warning · ESCALATED → Overdue · error · APPROVED → Done · success · REJECTED → Rejected · error · CANCELLED → Withdrawn · default |
| Document expiry (derived) | > 60 days → no chip · ≤ 60 days → Expires {date} · warning · past → Expired · error |

The colour mapping for enrolment, guardian and external ID statuses is a proposal following the existing Case and Sync pattern. All enum values are candidates (DL-27).

## 5. Who sees what

`E` edit · `V` view · `M` masked (lock icon, "Visible to Admissions") · `—` hidden · `?` open

| Field group | Admissions | Principal / VP | Coordinator | Class teacher | Subject teacher | Nurse / counsellor | Transport / security | Parent (own child) |
|---|---|---|---|---|---|---|---|---|
| Name, class, grade, house, programme | E | V | V | V | V | V | V | V |
| Enrolment status, dates, transfer history | E | V (approves transfers) | V | V (status only) | — | — | — | V |
| Photo | E | V | V | V | V | V | V | V |
| Date of birth | E | V | V | V | — | V | — | V |
| Guardians: names, relationship, can collect, emergency | E | V | V | V | — | V | V (collection only) | V |
| Guardian phone and email | E | V | V | V | M | V | V (transport only) | E |
| Addresses | E | V | M | M | — | — | V (pickup point only) | E |
| Identity documents, nationality, immigration | E (restricted access) | M | — | — | — | — | — | E |
| Attendance threshold (e.g. "90%", no reason) | V | V | V | V | V | — | — | V |
| External IDs (APAAR, board IDs) | E | V | V | — | — | — | — | V |

- Parents edit their child's passport, immigration status, addresses and contacts themselves, like MyGIIS Profile Update today (DL-28).
- Class teachers see "Attendance threshold 90%" but never why (no Student Pass badge) (DL-30).
- Group leadership sees counts only, never names.
- Every view is limited to the user's campus and scope first. A Grade 7 class teacher sees only 7B.

## 6. Rules that change the UI

| Rule | UI behaviour |
|---|---|
| Student ID never changes, even after a transfer (DL-31) | Read-only with a copy button in the profile header. |
| History is never overwritten (SR-03) | No "Delete" on enrolments, guardians, addresses. Use "End…" and show ended rows greyed under "Show history". Enrolment tab is a timeline. |
| Campus transfer needs both principals to approve; decision within 3 weeks; profile updated within 3 working days (SR-05, HB p.235) | Transfer is a case with two approval steps (sending and receiving principal) and an SLA countdown. Same student, same Student ID, new enrolment. |
| A course change needs a new contract; a campus change with the same course does not (SR-04) | Transfer wizard shows a notice when the programme changes: "A new contract is needed." |
| Withdrawal effective date = last attendance date (SR-06) | Withdrawal form asks for last attendance date and sets it as the end date. |
| On enrolment the student gets a class, timetable, bus route and logins (SR-07) | "Complete enrolment" step: assign class, house, subjects; preview of what gets created (logins for student and parents, family links). |
| Student Pass holders need an international home address (SR-08) | Blocking error on save if missing. |
| Student Pass → 90% monthly attendance threshold; ICA notice after 7 consecutive days of absence (SR-09) | Admissions sees the Student Pass and its case. Teachers only see the threshold. |
| Restricted data (SR-10) | Masked value `•••••` with lock icon; "Reveal" is logged ("This view is recorded"). Never sent to AI features. |
| A family link is created only from a verified guardian (SR-11) | Guardian with "Pending verification" shows "No parent app access yet" and a "Verify" action for Admissions. |
| Enrolment never gives staff access (SR-12) | — |
| MyGIIS data that conflicts is quarantined (SR-14) | "Conflict" chip with a side-by-side compare; never auto-overwrite. Synced fields show "Synced from MyGIIS". |
| Changes to enrolment, guardians, documents, immigration are audited (SR-15) | History tab on the profile; high-impact edits ask for a reason. |

## 7. Not decided yet: keep generic

Design these as generic or "pending" placeholders. Don't invent values.

| Open item | What to do in a design |
|---|---|
| Student ID format (OQ-36) | Use the sample format consistently; don't build validation on it. |
| Sex / gender values, country-specific identity fields (OQ-37) | Leave out, or one generic field labelled "Pending". |
| Legal custody, court orders (OQ-11) | Don't design. |
| Consent records, photo consent (OQ-10) | Don't design consent screens. |
| Health alerts on the student card (OQ-42) | Don't show allergies or medical flags. |
| Student app access (OQ-19) | Keep student-facing screens out of scope. |
| AEPP as programme or placement (OQ-45) | Show AEPP as a class or placement note, not a separate enrolment flow. |
| Data controller name (OQ-06) | Never name a legal entity in notices or exports. |
| House names per campus (OQ-20) | Use "House A–D" placeholders. |
| Admissions pipeline (enquiry, application, assessment, offer, contract, registration fee) | Not in this dictionary yet (next one). Pipeline screens can be lightweight; field lists are not agreed. Known: 7-working-day cooling-off after contract signature (HB), registration fee, AEPP placement test. |

## 8. Suggested screens for the Admission and Enrolment prototype

Staff web app (MinV dashboard shell, sidebar 280px, header 64px, content max 1200px):

1. **Student directory** — table with search and filters (campus, grade, class, segment, programme, enrolment status). Columns: photo + name + Student ID, grade · class, programme, campus, enrolment status, guardians count / verification. Campus switcher in the header.
2. **Student profile** — header (photo, name, Student ID with copy, status chips, current class) and tabs: Overview, Enrolment history (timeline), Guardians, Contact and address, Documents 🔒, External IDs, History (audit).
3. **Admissions pipeline board** — kanban by stage: Enquiry → Application → Assessment → Offered → Confirmed (contract signed, cooling-off countdown) → Enrolled. Generic cards; see section 7.
4. **Complete enrolment** — stepper: confirm details → assign class and house → subject choices → guardians and verification → preview what gets created → done.
5. **Campus transfer case** — request, both principals' approvals, SLA countdown, new enrolment preview, contract notice if programme changes.
6. **Guardian verification queue** — pending relationships with evidence and Verify / Reject.
7. **Student Pass cases** (Admissions only) — expiry list, case status, ICA notice flags.

Parent app (mobile):

8. **My children** — card per child across campuses (one guardian, two campuses in the sample data).
9. **Update profile** — MyGIIS-style edit of passport, immigration status, addresses and contacts, with "Sent to school" state.

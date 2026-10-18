# 02 — Key scenarios

**Status:** Proposed, 18 Oct 2026 · Closes gap G5 (with `04-flows/`)

**Tóm tắt (VI):** 5 kịch bản then chốt mà v1 phải chạy trọn vẹn. Mỗi kịch bản có trigger, persona, kết quả mong muốn, màn hình liên quan và rule chi phối. Chi tiết flow + state ở `04-flows/`.

Each scenario is written as a story with synthetic GIIS data from [00 § Canonical sample data](./00-product-brief.md#canonical-sample-data). Together they touch every v1 screen at least once.

| ID | Scenario | Primary persona | Other personas | Flow |
|---|---|---|---|---|
| S1 | Onboard a new teacher to first login | HR Admin | Global Admin, Module Admin, Teacher | [S1](./04-flows/S1-onboard-teacher.md) |
| S2 | Add a second campus / transfer a teacher | HR Admin | Campus Leader (both campuses), Module Admin, Global Admin | [S2](./04-flows/S2-campus-transfer.md) |
| S3 | Employee exit and access revocation | HR Admin | Global Admin, Module Admins, Campus Leader | [S3](./04-flows/S3-employee-exit.md) |
| S4 | Set up a module team for the new academic year | Module Admin | Global Admin (template publish), Teacher | [S4](./04-flows/S4-module-team-setup.md) |
| S5 | Campus access review and "why can they see this?" | Campus Leader | Global Admin, Platform Auditor | [S5](./04-flows/S5-access-review.md) |

---

## S1 — Onboard a new teacher to first login

**Story.** Priya Nair joins SG-PGL on 1 Nov 2026 as a CBSE 6–8 Mathematics teacher. Meera (HR Admin) creates her record. Daniel (Global Admin) grants the teaching modules at SG-PGL. Kenji (Module Admin, Attendance and Conduct) adds her as `TEACHER` for sections 7B and 7C. Priya signs in with Google on day one and sees exactly her classes.

- **Trigger:** signed offer (outside the platform) or HRIS sync (Decision D01).
- **Steps:** create Employee + Employment + Org Assignment + Campus Assignment (wizard, SCR-HR-04) → system emits EmploymentStarted → **Onboarding access request** case opens with a suggested access bundle by position ("Subject Teacher @ SG-PGL") → Global Admin approves GA grants (SCR-GOV-02) → Module Admins get "pending member" tasks → each adds membership, role, scope (SCR-TM-02) → account INVITED → ACTIVE on first SSO login.
- **Done when:** Priya has ACTIVE account, linked employee, GA grants for 4 modules × 1 campus, 4 TM memberships with scope; every step has an audit event.
- **Must not happen:** access granted because a campus was picked in the HR wizard (BR-06); a second Employee for an existing person (BR-01, duplicate check).
- **Rules:** BR-01, BR-04, BR-05, BR-06, BR-07, BR-12, BR-13, BR-15, BR-19; Decisions D01, D02, D05.
- **Edge cases:** person already exists from a past contract (rehire → new Employment on same Employee); no work email yet; start date in the future (status PENDING, account not invitable until 7 days before start, DL-13).

## S2 — Add a second campus / transfer a teacher

**Story.** From Term 2, Priya also teaches IGCSE 9–10 at East Coast on Wednesdays (additional assignment). In AY 2027-28 she transfers fully to East Coast.

- **Trigger:** HR adds or ends a campus assignment (SCR-HR-06).
- **Steps:** HR adds E07 for SG-ECS (reason "Additional teaching", `is_primary = false`) → CampusAffiliationChanged → **Access review** case (type *Affiliation change*) opens with a diff: "Campus added: SG-ECS. No access granted yet." → SG-ECS Campus Leader approves → Global Admin adds campus grant → SG-ECS Module Admins add memberships. For a transfer: HR ends SG-PGL assignment with `valid_to`, sets SG-ECS primary → review case proposes *removal* of SG-PGL memberships effective `valid_to + 1` and asks the SG-PGL Campus Leader to confirm.
- **Done when:** access matches affiliation, old-campus access ends on the effective date, history kept.
- **Must not happen:** silent grant or silent removal (Decision D07); overlapping duplicate assignments for the same campus (BR-14).
- **Rules:** BR-02, BR-03, BR-06, BR-13, BR-14, BR-15; Decisions D04, D07.

## S3 — Employee exit and access revocation

**Story.** Rahul Verma (Class Teacher, IN-NOI) resigns; last day 31 Mar 2027.

- **Trigger:** HR sets Employment `end_date` and `termination_reason_code` (SCR-HR-05) or HRIS sends EmploymentEnded.
- **Steps:** HR enters end date → confirmation dialog lists everything that will be reviewed (account, 3 GA grants, 4 memberships, class-teacher allocation for 6A) → **Exit access review** case opens → on `end_date + 1` the system sets grants to EXPIRED and memberships to DEACTIVATED automatically unless the reviewer chose *revoke now* or *extend (with reason, max 30 days)* → account SUSPENDED, then DISABLED after the retention window → Module Admin of Teaching and Learning is told 6A has no class teacher.
- **Done when:** no active grant or membership after `end_date`; case closed with attestation; audit complete.
- **Must not happen:** automatic reactivation on rehire (Decision D10); deleting history (BR-03).
- **Rules:** BR-05, BR-10, BR-12; Decisions D10, D12.

## S4 — Set up a module team for the new academic year

**Story.** Before AY 2027-28 at SG-PGL, Kenji (Module Admin, Attendance and Conduct) rolls the team forward: new sections, new teachers, and Global Admin has published template `attendance.teacher` v3 (adds "edit marks after lock with reason").

- **Trigger:** academic-year rollover task opened by the calendar engine 30 days before AY start, or a new template version published.
- **Steps:** open Team (SCR-TM-01) → "Prepare AY 2027-28" banner → review suggested scopes from academic allocations (entity D02) in a bulk table → accept/adjust per member → choose whether members move to template v3 (each binding is version-pinned, BR-18) → preview effective-permission changes → apply with effective date = AY start.
- **Done when:** every member has a scope for the new AY or is explicitly ended; template bindings chosen; diff recorded.
- **Must not happen:** an empty scope meaning "all sections" (BR-17); a published template edited in place (BR-18).
- **Rules:** BR-07, BR-08, BR-15, BR-16, BR-17, BR-18; Decisions D08, D09, D11, D14.

## S5 — Campus access review and "why can they see this?"

**Story.** Each term, Dr Anita Rao (Principal, SG-PGL) attests who can access SG-PGL. A parent complaint asks why a transport executive could see a student's medical note; she needs the answer from one screen.

- **Trigger:** termly attestation case (calendar engine: first week of each semester) or ad-hoc investigation.
- **Steps:** Campus dashboard (SCR-CL-01) shows "Access review: 212 people, 18 flagged" → review list grouped by risk (Restricted capability holders, cross-campus users, overrides, grants without recent login > 60 days) → keep / reduce / revoke per row, bulk keep for unflagged → submit attestation. Investigation: search the person → Effective Permissions (SCR-GA-04) → select capability `wellbeing.health_note.view` → explanation chain shows grant → membership → role → template version → scope → (override) → each link opens its audit event.
- **Done when:** attestation submitted with ≥ 95% rows decided; revocations applied; investigation exportable as evidence.
- **Rules:** BR-08, BR-11, BR-12, BR-16, BR-17; Decisions D08, D12, D13.

---

## Coverage check

| Screen group | S1 | S2 | S3 | S4 | S5 |
|---|:-:|:-:|:-:|:-:|:-:|
| Staff and HR (SCR-HR) | ● | ● | ● | | ○ |
| Global Admin (SCR-GA) | ● | ● | ● | ● | ● |
| Team Management (SCR-TM) | ● | ● | ○ | ● | ○ |
| Governance (SCR-GOV) | ● | ● | ● | ○ | ● |
| Campus Leader (SCR-CL) | | ● | ○ | | ● |
| Self-service (SCR-ME) | ● | ○ | | ○ | |

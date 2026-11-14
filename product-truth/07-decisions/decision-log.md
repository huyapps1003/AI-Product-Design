# Decision log

**Status:** Living document · started 18 Oct 2026 · **Owner:** Henry

**Tóm tắt (VI):** Các giả định đã chọn để thiết kế được ngay, kèm lý do và phương án đã loại. Viết theo dạng dùng thẳng cho case study: bối cảnh → quyết định → lý do → cần ai xác nhận.

Status values: **Assumed** (chosen by design, needs confirmation) · **Confirmed** (stakeholder agreed) · **Superseded**.

**Evidence lines.** Since 25 Oct 2026 an entry may carry an *Evidence* line citing research (`RES-F…`, `MC-…`) and open questions (`OQ-…`). Evidence never changes an entry's status by itself; only a new or superseding DL entry does.

The "Linked" column uses the old flat file numbers: `00` = admin-platform brief, `01` = roles-personas, `07` = rbac-model; `G…` = archived gap analysis.

| ID | Decision | Status | Linked |
|---|---|---|---|
| DL-01 | v1 design scope is the Admin Platform | Assumed | G1, 00 |
| DL-02 | Personas map to GA/TM constructs, not to new tables | Assumed | G2, 01, Arch Q9 |
| DL-03 | One canonical sample data set based on GIIS campuses | Assumed | G8 |
| DL-04 | R01 module catalog = 12 campus modules + 2 platform modules | Assumed | G3 |
| DL-05 | Admin Platform lives inside the staff web app | Assumed | G12 |
| DL-06 | Desktop-first for admins, mobile-first for self-service | Assumed | G16 |
| DL-07 | Permission evaluation: deny wins, fail closed | Assumed | Decision D08 |
| DL-08 | Access requests and reviews reuse the shared Case object | Assumed | G11, Decision D07, D10 |
| DL-09 | Separation of duties between HR, Global Admin and Module Admin | Assumed | 01 |
| DL-10 | Adopt candidate status enums with fixed colours | Assumed | Decision D15 |
| DL-11 | Tier 1/2/3 = Internal/Confidential/Restricted | Assumed | G10, Decision D13 |
| DL-12 | Rename entity D01–D03 to X01–X03 | Proposed | G13 |
| DL-13 | Account invite timing and suspension cascade | Assumed | Decision D10 |
| DL-14 | Per-member overrides off by default | Assumed | Decision D09 |
| DL-15 | UI language English, per-user locale for dates and numbers | Assumed | E01 `preferred_locale` |
| DL-16 | Derived scope is suggestion by default; auto-apply opt-in per module | Assumed | 07, Decision D11 |
| DL-17 | Grants use scope-node hierarchy (Group > Country > Campus) | Assumed; refined by DL-35, DL-40 | 07, A03/A04 |
| DL-18 | Relationship-based access for parents via family links, not grants | Assumed; refined by DL-28, DL-37 | 07 |
| DL-19 | Record-level ACL for restricted cases and Tier 3 fields | Assumed | 07, Decision D13 |
| DL-20 | Templates have BASE and MODULE categories | Assumed | 07, A08 |
| DL-21 | Time-boxed delegation with reason, max duration, and audit | Assumed | 07 |
| DL-22 | Scope is every GSG brand; GSG operates Campus OS; SmartLearn excluded | **Confirmed** (Henry, 25 Oct 2026) | RES-F01, RES-F02 |
| DL-23 | `student_id` is an immutable UUID, separate from Student ID, board IDs and legacy IDs | **Confirmed** (Henry, 13 Nov 2026) | Student dictionary ST01, ST10 |
| DL-24 | Enrolment and student directory records are effective-dated; history never overwritten | **Confirmed** (Henry, 13 Nov 2026) | Student dictionary ST02 |
| DL-25 | Student–guardian relationship is the directory record; Family Link is created only from a verified relationship | **Confirmed** (Henry, 13 Nov 2026) | Student dictionary ST05, DL-18 |
| DL-26 | Tiers and role visibility for student fields Architecture doesn't classify | **Confirmed** (Henry, 13 Nov 2026) | Student dictionary § Field visibility, DL-11 |
| DL-27 | Candidate enums for the student directory | **Confirmed** (Henry, 13 Nov 2026) | Student dictionary § Candidate enums |
| DL-28 | Parents edit their child's profile as in MyGIIS Profile Update | **Confirmed** (Henry, 30 Oct 2026) | OQ-38, DL-18, HB p.35 |
| DL-29 | Product terms: Class (a group like 7B) and Segment (Pre-Primary…Higher); "Section" not used | **Confirmed** (Henry, 30 Oct 2026) | OQ-40 |
| DL-30 | Teachers see a student's attendance threshold, not the Student Pass behind it | **Confirmed** (Henry, 30 Oct 2026) | OQ-41, DL-11 |
| DL-31 | Student ID is unique across Campus OS and stays the same on campus transfer | **Confirmed** (Henry, 30 Oct 2026) | OQ-36 |
| DL-32 | One Access Registry (Identity and Roles) is the only writer of access definitions and assignments; modules keep no permission tables | **Confirmed** (Henry, 7 Nov 2026) | RBAC review 7 Nov; permission-catalogue |
| DL-33 | `04-access/permission-catalogue.md` is the canonical list of capabilities, field policies and template contents | **Confirmed** (Henry, 7 Nov 2026) | DL-32 |
| DL-34 | Field visibility is expressed as capabilities on field groups, not school roles | **Confirmed** (Henry, 7 Nov 2026) | BR-06, DL-26 |
| DL-35 | TM memberships sit on a scope node, like grants; campus is the default | **Confirmed** (Henry, 7 Nov 2026) | A05, DL-17 |
| DL-36 | Delegation (A16) and Tier 3 type grants (A17) are registry entities and steps in evaluation | **Confirmed** (Henry, 7 Nov 2026) | DL-19, DL-21 |
| DL-37 | The parent path is an alternative path, not a layer; relationship-derived scope removed from Layer 3 | **Confirmed** (Henry, 7 Nov 2026) | DL-18, DL-28 |
| DL-38 | The decision service returns the deciding layer and the source record | **Confirmed** (Henry, 7 Nov 2026) | DL-07 |
| DL-39 | Global layer holds Tier 1 identity, directory index, scope tree, access definitions and de-identified analytics only | **Confirmed** (Henry, 7 Nov 2026) | Arch § System architecture |
| DL-40 | Scope tree Group › Brand › Brand-country › Campus › Segment / Programme offering | **Confirmed** (Henry, 7 Nov 2026) | OQ-14, OQ-17, DL-17 |
| DL-41 | Legal entity and campus operator are effective-dated links; `operating_entity_id` on enrolment, employment and cases | **Confirmed** (Henry, 7 Nov 2026) | OQ-02, OQ-04, MC-03 |
| DL-42 | One Person with Student, Guardian and Employee profiles and one account | **Confirmed** (Henry, 7 Nov 2026) | OQ-35 |
| DL-43 | One writer per entity, per the object registry | **Confirmed** (Henry, 7 Nov 2026) | OQ-44, Employee DD D01 |
| DL-44 | Consent, purpose, sharing record and retention policy are first-class records with empty legal slots | **Confirmed** (Henry, 7 Nov 2026) | OQ-07, OQ-09, OQ-10, OQ-43 |
| DL-45 | Legacy systems are authoritative per field until a cutover date; conflicts are quarantined | **Confirmed** (Henry, 7 Nov 2026) | OQ-44, Student DD D09 |
| DL-46 | Design works through three layers: architecture rules, object cards, slices | **Confirmed** (Henry, 7 Nov 2026) | 03-domain/architecture.md, 03-domain/objects/ |
| DL-47 | Product term for a cell is "Data location" everywhere (staff and parent UI) | **Confirmed** (Henry, 12 Nov 2026) | Organisation dictionary, DL-39 |
| DL-48 | One Scope node record per tree node with typed profiles; nodes close, never move | **Confirmed** (Henry, 12 Nov 2026) | Organisation dictionary OR01, DL-40 |
| DL-49 | Every publicly listed campus is a Campus node, including stage campuses | **Confirmed** (Henry, 12 Nov 2026) | Organisation dictionary OR04, OQ-50 |
| DL-50 | Candidate enums for the organisation tree, legal entities and settings | **Confirmed** (Henry, 12 Nov 2026) | Organisation dictionary |
| DL-51 | The GSG platform team owns the Identity and Roles service and the Access Registry | **Confirmed** (Henry, 12 Nov 2026) | OQ-46, DL-32 |
| DL-52 | The Campus Leader approves a platform-operator or support session into their campus | **Confirmed** (Henry, 12 Nov 2026) | OQ-48, OQ-15 |
| DL-53 | Capability codes follow `module.object.action` in every module | **Confirmed** (Henry, 12 Nov 2026) | OQ-49, DL-33 |
| DL-54 | The academic year decides promotion vs transfer: grade up into a new year is a promotion, any campus move within a year is a transfer | **Confirmed** (Henry, 12 Nov 2026) | OQ-50, DL-49, student dictionary SR-16 |
| DL-55 | One global Person for a guardian; guardian profile held in each child's data location | **Confirmed** (Henry, 12 Nov 2026) | OQ-39, DL-39, DL-42 |
| DL-56 | Enquiry per family; application per child per target (campus × offering × grade × academic year) | **Confirmed** (Henry, 14 Nov 2026) | Admissions dictionary AD01–AD03 |
| DL-57 | Student and Enrolment (OFFERED) are created when an offer is issued | **Confirmed** (Henry, 14 Nov 2026) | Admissions dictionary, ST02 |
| DL-58 | Enquirers and applicants matched to existing Persons via the DL-42 review queue | **Confirmed** (Henry, 14 Nov 2026) | DL-42 |
| DL-59 | Admission rules (cooling-off, fees, documents, assessments, offer validity) are scope-tree settings; GIIS SG values are sample defaults | Assumed (proposed 12 Nov 2026) | MC-14, DL-22 |
| DL-60 | SEN evaluation and identity documents stay Tier 3 with their owners; Admissions sees outcome and link | **Confirmed** (Henry, 14 Nov 2026) | DL-30 pattern |
| DL-61 | Intake capacity = seat limit per campus × offering × grade × academic year; overrides need a reason | Assumed (proposed 12 Nov 2026) | OQ-65 |
| DL-62 | Stage-campus promotion needs no extra approval; any other year-end campus change needs both principals | **Confirmed** (Henry, 13 Nov 2026) | OQ-60, DL-54 |
| DL-63 | A year-end move into another brand or country is a new admission, not a promotion | **Confirmed** (Henry, 13 Nov 2026) | OQ-60, OQ-07 |
| DL-64 | A guardian's own contact and address changes apply in every data location; per-child flags stay per child | **Confirmed** (Henry, 13 Nov 2026) | OQ-61, DL-55, DL-28 |
| DL-65 | AEPP is a placement inside a regular enrolment, not a programme offering | **Confirmed** (Henry, 13 Nov 2026) | OQ-45, DL-29 |
| DL-66 | Student card shows a "Medical alert" flag only, no detail | **Confirmed** (Henry, 13 Nov 2026) | OQ-42 |
| DL-67 | Student ID = prefix + year of first admission + global sequence, no campus or brand code | **Confirmed** (Henry, 13 Nov 2026) | OQ-36, DL-31 |
| DL-68 | Students see only their own Tier 1 profile and enrolment | **Confirmed** (Henry, 13 Nov 2026) | OQ-19 |

---

### DL-01 — v1 design scope is the Admin Platform
- **Context.** The architecture describes the whole OS; the Data Dictionary and every existing screen describe employee and access management. Henry's personas are all staff/admin.
- **Decision.** Design Staff and HR + Global Admin + Team Management + Governance first, plus Campus Leader and Teacher self-service views.
- **Why.** Every campus module depends on people and access being right; this slice has the most complete data spec; it is self-contained for a case study.
- **Alternatives rejected.** Parent app first (matches roadmap Phase 1 "daily loop", but no data dictionary exists for it yet); one campus module end to end (would need to invent its access model anyway).
- **Confirm with.** Henry.

### DL-02 — Personas map to GA/TM constructs
- **Context.** Architecture open question #9: how role-plus-scope maps to GA/TM.
- **Decision.** Architecture "role + scope" = GA campus grant (scope ceiling) + TM membership with access role and data scope. Personas are bundles of these; HR position and academic role never grant access.
- **Why.** Keeps the retained GA/TM architecture (Data Dictionary confirmed input) and gives architecture roles a concrete home.
- **Confirm with.** Data architect, Global Admin owner.

### DL-03 — Canonical sample data
- **Context.** Existing screens use Singapore/Hong Kong/Kuala Lumpur/Jakarta/Global HQ and Azure AD; the architecture uses Punggol, East Coast, Noida and Google Workspace.
- **Decision.** Use the set in [00 § Canonical sample data](../06-experience/admin-platform/README.md#canonical-sample-data); add a synthetic "Group Office" campus for group staff.
- **Why.** Matches the GIIS knowledge files and the architecture; avoids a designer or generator mixing two worlds.
- **Follow-up.** Update `screens/*.html` to this data set.
- **Evidence (25 Oct 2026).** Doesn't match scope: every sample campus is GIIS, and the scope is now all 12 brands (research §10; RES-F01; DL-22). Which other brand to add is open (OQ-33).

### DL-04 — Module catalog
- **Decision.** R01 = 12 campus modules (Architecture) + `global_admin` + `governance`. Academic Allocation and Timetable are parts of `teaching_learning`, not modules.
- **Why.** GA grants and the sidebar need one list; Academic Allocation and Timetable are owned by teaching operations and are named in the dictionary as external owners, not as separate products.
- **Alternatives rejected.** Making every owner in the dictionary a module (too many grants per teacher).
- **Confirm with.** Henry; whoever owns Academic Allocation today.

### DL-05 — Admin inside the staff web app
- **Decision.** One staff web app; admin sections appear by role. No separate "Admin Portal" login. Existing mocks labelled "Admin Portal" become the admin sections of this app.
- **Why.** Campus Leaders and Module Admins are also everyday staff; a second app splits their work. The architecture already defines one "Staff app and web".

### DL-06 — Device targets
- **Decision.** Admin screens desktop-first (≥ 1280 px, usable at 1024 px). Self-service (SCR-ME-*), approvals (SCR-GOV-02) and sign-in work at 375 px.
- **Why.** Bulk tables and matrices don't fit phones; teachers and principals act on approvals between classes.

### DL-07 — Permission evaluation
- **Decision.** GA ceiling → membership → union of allows across bindings → any deny wins → scope groups (AND inside, OR across) → no scope = no data.
- **Why.** Fail-closed is the safest default for children's data (PDPA, DPDP) and is easy to explain in one chain.
- **Confirm with.** Data architect (Decision D08).

### DL-08 — Cases for access
- **Decision.** Add case types `ACCESS_REQUEST`, `ONBOARDING_ACCESS`, `AFFILIATION_REVIEW`, `EXIT_REVIEW`, `CAMPUS_ATTESTATION` to the architecture's shared Case object with line-level decisions and SLA timers.
- **Why.** The architecture already has one Case object with SLAs and a workflow engine; access approvals get the same inbox, timers and audit for free.

### DL-09 — Separation of duties
- **Decision.** HR Admin edits people, not access. Global Admin edits the access ceiling and catalogs, not people. Module Admin edits teams inside the ceiling. Granting `GLOBAL_ADMIN` needs a second Global Admin.
- **Why.** Prevents one person creating a fake employee and granting them access; easy to audit.
- **Evidence (25 Oct 2026).** Partial: covers HR, Global Admin and Module Admin; platform operator and vendor support are not mentioned (research §10; RES-F06; OQ-15).

### DL-10 — Status vocabulary
- **Decision.** Adopt the Data Dictionary's candidate enums and the colour table in [05 § Status vocabulary](../06-experience/admin-platform/rules-to-ui.md#status-vocabulary).
- **Why.** Designers need fixed labels now; changing an enum later changes one table.

### DL-11 — Sensitivity tiers
- **Decision.** Tier 1 = Internal, Tier 2 = Confidential, Tier 3 = Restricted; one lock treatment (P-LOCK) for anything above the viewer's level.
- **Evidence (25 Oct 2026).** Partial: PDPC treats children's data as generally sensitive, while product truth puts students' academic data in Tier 2 (research §10; OQ-13).

### DL-12 — Rename academic entities
- **Decision (proposed for Data Dictionary v1.1).** Entity D01/D02/D03 → X01/X02/X03, keeping Decision D01–D15 as is.
- **Why.** "D01" currently means two things.

### DL-13 — Account invite timing and suspension cascade
- **Decision.** Invite email sent 7 days before employment start (or at creation if start is sooner); account can't sign in before start date. Employment SUSPENDED → account SUSPENDED; account changes never change employment.
- **Confirm with.** HR and IT (Decision D10).

### DL-14 — Overrides off
- **Decision.** Per-member overrides (A11) hidden behind `tm.overrides.enabled = false`; designs include the tab so it can be turned on.
- **Why.** Feature existence is open (Decision D09); templates + scope cover the known cases.

### DL-15 — Language
- **Decision.** UI copy in English for all campuses in v1; dates, numbers and time zones follow the user's `preferred_locale` and the campus `time_zone`. Hindi or other UI languages are a later decision.
- **Why.** GIIS instruction and staff communication in the files are English; second languages (Hindi, Tamil, French, Mandarin, Sanskrit) are subjects, not staff UI requirements.
- **Evidence (25 Oct 2026).** Partial: DPDP gives parents a right to notices in scheduled Indian languages, and GSG has campuses in non-English-speaking countries (research §10; MC-12; OQ-25).

---


### DL-16 — Derived scope is suggestion by default
- **Context.** Academic allocations (entity D02) and org assignments (E04) naturally define what a teacher or staff member should see. Problem #3: manual scope drifts from allocations every term.
- **Decision.** Derived scope sources (allocation, org unit) generate scope *suggestions* that Module Admin confirms. A per-module flag `tm.scope.auto_derive` enables auto-apply where confirmed safe.
- **Why.** Automatic scope keeps data access aligned with teaching assignments without removing human oversight. Auto-apply is opt-in because some modules (Finance, Governance) need manual control.
- **Alternatives rejected.** Fully automatic (no human check — risky for sensitive modules); fully manual only (status quo, known to drift).
- **Confirm with.** Module owners, data architect.

### DL-17 — Hierarchical grant ceiling
- **Context.** Problem #1 and #4: flat per-campus grants mean O(users × campuses) work for every new campus and every cross-campus role (Country Director, Group HR).
- **Decision.** Grants (A03/A04) gain a `scope_node` field: GROUP, COUNTRY, or CAMPUS. A grant at a parent node covers all descendants. Existing campus grants map to `scope_node_type = CAMPUS`.
- **Why.** Country Directors and Group staff get one grant instead of N. New campuses inherit grants automatically. The change is additive — campus-level grants still work unchanged.
- **Alternatives rejected.** "Wildcard campus" grant (ambiguous, no hierarchy for partial coverage); automatic grants from org assignment (conflates affiliation with access, violates BR-06).
- **Confirm with.** Data architect, Global Admin owner.
- **Evidence (25 Oct 2026).** Partial: covers Group, Country and Campus; there is no brand level, and a country grant can't tell brands or entities apart (research §08, §10; RES-F06; OQ-14, OQ-17).

### DL-18 — Relationship-based access for parents
- **Context.** Problem #4: parents need to see their child's data. GA/TM grants and memberships don't model "my child" — they model "this module at this campus".
- **Decision.** A new entity F01 Family Link (parent account → student enrolment) is the authorization. Parents get a fixed read-only capability set (R03) scoped to their linked children. No GA grant or TM membership is created.
- **Why.** Parent access is inherently relational ("my child"), not role-based. Forcing it into GA/TM would create phantom memberships, confuse Module Admins, and break separation of duties. Custody changes and withdrawals cleanly revoke access by ending the link.
- **Alternatives rejected.** Parent as a TM member with VIEWER role (membership explosion; Module Admin sees parents in their team); parent as a special GA grant (grant doesn't carry the child relationship).
- **Confirm with.** Admissions owner, legal (custody and data sharing).
- **Refined by DL-28 (30 Oct 2026).** Parents may also edit their child's profile as in MyGIIS Profile Update.

### DL-19 — Record-level ACL
- **Context.** Problem #5 and Tier 3 fields. A discipline case or a restricted HR field should be visible only to specific people, not everyone with the capability.
- **Decision.** New entity A15 Record ACL Entry. Cases can be marked `visibility = RESTRICTED`; only assignee, committee, and users with `gov.case.view_all` see content. Tier 3 field access requires an explicit ACL entry. Every Tier 3 read is logged.
- **Why.** Capabilities answer "can this role do this action?"; ACLs answer "on this specific record?". Children's sensitive data (PDPA, DPDP) needs both layers.
- **Confirm with.** Data architect, legal/compliance.

### DL-20 — BASE and MODULE template categories
- **Context.** Problem #2: 12 modules × multiple roles = many templates with duplicated capabilities (view profile, view directory, etc.).
- **Decision.** Templates (A08) gain a `category`: BASE (shared capabilities like profile view, directory search) or MODULE (module-specific). A role-template binding resolves both: union of allows from BASE + MODULE; deny in either wins.
- **Why.** A "Teacher base" template covers the 15–20 capabilities every teacher needs regardless of module. Module-specific templates add only the delta. Template count drops from O(modules × roles) to O(modules + roles).
- **Confirm with.** Data architect.

### DL-21 — Time-boxed delegation
- **Context.** Problem #6: a principal on leave can't delegate approval authority to the VP without giving them permanent grants.
- **Decision.** An acting grant: user A delegates capability set C to user B for a stated period and reason. The delegation is audited, capped at 90 days, and auto-expires. The delegator must hold the capabilities being delegated. Delegated actions are logged as "B acting for A".
- **Why.** Schools have well-defined leave and coverage patterns. Without delegation, either the VP gets permanent over-access or approvals stall during leave.
- **Alternatives rejected.** Shared accounts (violates audit trail); permanent dual grants (over-access); manual grant/revoke cycle (error-prone, often forgotten).
- **Confirm with.** HR, Global Admin owner.
- **Evidence (25 Oct 2026).** Consistent with cover and acting patterns; vendor support access isn't mentioned (research §10; OQ-15).

### DL-22 — Scope is every GSG brand; GSG operates Campus OS
- **Context.** Architecture v1, the sample data and the design layer describe one brand (GIIS), 3 campuses and 2 countries. GSG has 12 brands and claims 64 campuses in 11 countries (RES-F01), assembled largely by acquisition (RES-F02).
- **Decision.** Global Campus OS serves all GSG school brands, and Global Schools Group runs (operates) the platform. SmartLearn is out of scope for Campus OS product truth.
- **Why.** Stated by Henry on 25 Oct 2026.
- **What it changes now.** Product truth labels GIIS facts as GIIS examples, not as the whole scope. It does not decide how brands fit the hierarchy, access model or data-controller model; those are open (OQ-01…OQ-07, OQ-14…OQ-17, OQ-26, OQ-33).
- **Confirmed by.** Henry, 25 Oct 2026.

### DL-23 — Student identity key
- **Decision.** Every learner has one immutable, never-reused `student_id` (UUID). The Student ID people see (DL-31), board and national IDs and legacy portal IDs are separate attributes or mappings (ST10).
- **Why.** Same reasoning as BR-01 and E08 for staff; a transfer keeps one identity.
- **Confirm with.** Data architect.

### DL-24 — Effective-dated enrolment
- **Decision.** Enrolments, guardian relationships, addresses, documents and identifiers carry valid dates. A transfer or rollover ends the old enrolment and creates a new one linked by `previous_enrolment_id`.
- **Why.** Architecture's effective-dating rule; BR-03.
- **Confirm with.** Data architect, Admissions owners.

### DL-25 — Guardian relationship vs. family link
- **Decision.** The student–guardian relationship (ST05) records who a guardian is to a student. The Family Link (F01, DL-18) is created only from a VERIFIED relationship and ends when it ends.
- **Why.** Keeps "who is this person to the child" separate from "what can they see", as BR-06 does for staff.
- **Confirm with.** Admissions owners, legal (OQ-10, OQ-11).

### DL-26 — Student field tiers and visibility
- **Decision.** Use the tiers marked `T2*` and the Assumed cells of the field visibility matrix in [student-directory-data-dictionary](../03-domain/student-directory-data-dictionary.md#field-visibility-by-role).
- **Why.** Architecture classifies directory, academic, finance and identity data but not addresses, contacts or dates of birth.
- **Confirm with.** Admissions owners, DPOs. OQ-13 may raise tiers.

### DL-27 — Student directory enums
- **Decision.** Adopt the candidate enums in the student dictionary.
- **Why.** Designers need fixed labels now; same approach as DL-10.

### DL-28 — Parents edit the student profile as in MyGIIS
- **Context.** DL-18 gives parents a read-only capability set, but MyGIIS Profile Update lets parents update passport details, immigration status, addresses and contact details of the child and both parents (HB p.35), and MyGIIS parity is the Phase 1 gate.
- **Decision.** Follow MyGIIS: parents edit these fields for their own children. This adds profile edit to the parent capability set (R03); everything else in DL-18 stands.
- **Confirmed by.** Henry, 30 Oct 2026. Answers OQ-38.

### DL-29 — Class and Segment
- **Context.** Architecture uses "Section" for a school segment (tenancy table) and for a class (data model).
- **Decision.** **Class** is a group of students such as 7B. **Segment** is a school segment such as Pre-Primary, Primary, Middle or Higher. "Section" is not used as a product term.
- **Confirmed by.** Henry, 30 Oct 2026. Answers OQ-40 (the AEPP part is OQ-45).

### DL-30 — Attendance threshold without the Student Pass
- **Context.** Class teachers apply the 90% attendance rule for Student Pass holders, but immigration status is Tier 3.
- **Decision.** Teachers and coordinators see the student's attendance threshold (for example "Attendance threshold 90%") without the reason. Student Pass and immigration status stay Tier 3 for Admissions and other named roles with a record ACL.
- **Confirmed by.** Henry, 30 Oct 2026. Answers OQ-41.

### DL-31 — Student ID
- **Decision.** The student number is shown as **Student ID**. It is unique across Campus OS and stays the same when a student transfers between campuses. The format is still open (OQ-36).
- **Confirmed by.** Henry, 30 Oct 2026.

### DL-32 — Access Registry is the single source of truth for permissions
- **Context.** Permission rules were written in six places (Employee DD A01–A14, rbac-model, roles-personas, rules-to-ui, the student field matrix, role-catalogue) and none was canonical; the owners of A06, A08, A09, R02 and A14 were "TBD".
- **Decision.** One Access Registry, owned by the Identity and Roles core service, is the only writer of access **definitions** (R01, R02, field policy, A06, A08/A09, R03, R05, purpose catalogue) and **assignments** (A03/A04, A05/A07/A10, A12/A13, F01, A15, A16, A17, operator sessions). Inputs (scope tree, employment, allocations, guardian relationships, enrolments) stay with their owning domains and are read by ID; a change to them creates a review or a suggestion, never access (BR-06). One decision service per country cell evaluates every request; modules store no permissions.
- **Why.** Each new module would otherwise add another matrix that can disagree with the others.
- **Confirmed by.** Henry, 7 Nov 2026.

### DL-33 — Permission catalogue
- **Decision.** [`04-access/permission-catalogue.md`](../04-access/permission-catalogue.md) is the canonical product-truth list of capabilities, field policies and template contents. roles-personas, rules-to-ui and the student field matrix link to it and are views of it.
- **Confirmed by.** Henry, 7 Nov 2026.

### DL-34 — Field visibility by capability
- **Decision.** Each field group has a tier and the capabilities that view, edit or reveal it (for example `student.contact.view`, `student.identity.reveal`). Templates hand capabilities to access roles. School roles and personas are labels for design, not keys for visibility.
- **Why.** School roles never grant access (BR-06), so a matrix keyed by them can't be enforced.
- **Confirmed by.** Henry, 7 Nov 2026.

### DL-35 — Memberships on a scope node
- **Decision.** A05 gains `scope_node_type` and `scope_node_id`, like A03/A04 (DL-17). Campus remains the default. A membership at a parent node covers every descendant inside the grant ceiling.
- **Why.** With campus-only memberships a Country Director still needed one membership per campus per module.
- **Confirmed by.** Henry, 7 Nov 2026.

### DL-36 — Delegation and Tier 3 type grants as entities
- **Decision.** DL-21 delegation becomes registry entity A16 Delegation (delegator, delegate, capability set, scope node, reason, valid dates, at most 90 days). A Tier 3 type grant, A17 (grantee, record type, scope node, reason, approver, valid dates), lets named roles read a class of restricted records without one A15 row per record. Both are steps in the evaluation algorithm. Who approves an A17 grant is OQ-47.
- **Confirmed by.** Henry, 7 Nov 2026.

### DL-37 — Parent path is an alternative path
- **Decision.** Layer 4 (family link) is an alternative path evaluated beside the staff path, not a layer every request passes. Relationship is removed as a Layer 3 derived-scope source. R03 includes profile edit (DL-28).
- **Confirmed by.** Henry, 7 Nov 2026.

### DL-38 — Explained decisions
- **Decision.** Every answer from the decision service carries the deciding layer and the source record (grant, membership, family link, ACL, delegation). This powers "Why can I see this?" (AP-07) and "Inherited from…" chips.
- **Confirmed by.** Henry, 7 Nov 2026.

### DL-39 — What the global layer holds
- **Decision.** The global layer holds Tier 1 only: Person and account, a directory index (name, role chips, cell pointer), the scope tree and legal entities, access definitions, and de-identified analytics. Everything else lives in the country cell that owns the record.
- **Confirmed by.** Henry, 7 Nov 2026.

### DL-40 — Scope tree with Brand
- **Decision.** The scope tree is Group › Brand › Brand-country › Campus, with Segment and Programme offering below campus. A Brand-country node (for example GIIS Singapore) holds the regulator pack, currency and holidays; its country code selects the data cell. Grants and memberships can sit at Group, Brand, Brand-country or Campus.
- **Why.** Answers OQ-14 (where brand sits) and OQ-17 (a country-level grant now stays inside one brand).
- **Refines.** DL-17.
- **Confirmed by.** Henry, 7 Nov 2026.

### DL-41 — Legal entity and campus operator
- **Decision.** Legal entities (GSG operator, school operating entities, franchise and JV partners, employers) sit beside the scope tree. Campus operator is an effective-dated link from campus to operating entity. Enrolment, employment and cases copy `operating_entity_id` at creation. That field is the controller slot; it is named as controller only after OQ-06 is answered.
- **Why.** Gives OQ-02 and OQ-04 a structure; the actual list of entities is still OQ-02.
- **Confirmed by.** Henry, 7 Nov 2026.

### DL-42 — One Person
- **Decision.** One Person per human (`person_id`, global, Tier 1) with Student (ST01), Guardian (ST04) and Employee (E01) profiles in cells, and one account (A01) linked to the Person. Domain keys (`student_id`, `employee_id`) stay and link to `person_id`. Possible duplicates go to a review queue and are never merged automatically.
- **Why.** Answers OQ-35 and Student DD D01: a parent who is also staff is one person with one login and a context switch.
- **Confirmed by.** Henry, 7 Nov 2026.

### DL-43 — One writer per entity
- **Decision.** Every entity has exactly one owning domain, listed in [`03-domain/objects/README.md`](../03-domain/objects/README.md). Other domains read by ID or through events and show the data read-only with a source chip.
- **Confirmed by.** Henry, 7 Nov 2026.

### DL-44 — Governance records
- **Decision.** Consent, purpose catalogue, sharing record and retention policy are first-class records. Their legal fields (lawful basis, controller) stay empty until counsel answers OQ-06 and the related questions.
- **Why.** Gives OQ-07, OQ-09, OQ-10 and OQ-43 a structure without inventing the legal answers.
- **Confirmed by.** Henry, 7 Nov 2026.

### DL-45 — Legacy authority per field
- **Decision.** During migration an authority register records, per field group and system, who is authoritative, from when and in which direction it syncs. Legacy systems (MyGIIS, other brands' SIS) stay authoritative until each field's cutover date; Campus OS shows those fields read-only with a source chip. Conflicts are quarantined for a named reviewer.
- **Why.** Answers the structure of OQ-44 and Student DD D09. Which systems and dates apply per brand stays open (OQ-27, OQ-28).
- **Confirmed by.** Henry, 7 Nov 2026.

### DL-46 — Three-layer design workflow
- **Decision.** The AI design workflow loads, in order: Layer 0 architecture rules (AR) and patterns (AP) in [`03-domain/architecture.md`](../03-domain/architecture.md); Layer 1 design object cards (OBJ) in [`03-domain/objects/`](../03-domain/objects/README.md); Layer 2 design slices in `06-experience/`. Object cards are the design view of the data dictionaries and never a second truth.
- **Confirmed by.** Henry, 7 Nov 2026.

### DL-47 — "Data location" as the product term for a cell
- **Context.** Henry asked why the architecture says "cell" (9 Nov 2026). It is infrastructure jargon that designers and school staff don't use.
- **Decision.** Staff and parent UI both say **Data location**, shown as "Data location: Singapore". "Cell" stays in engineering docs. The entity is OR06 Data location. Replaces the earlier proposal of "data region" for staff and "data home" for parents (one term reads clearly in both).
- **Status.** Confirmed by Henry, 12 Nov 2026.

### DL-48 — Scope node record
- **Decision.** Each tree node is one Scope node (OR01) with a type and a materialised path; Brand, Brand-country, Campus and Segment are typed profiles. `campus_id` equals the campus node ID. Nodes are closed, never deleted or re-parented; a campus that changes brand gets a new node.
- **Why.** Grants (A03, A04), memberships (A05) and `tenant_path` all point at one key, and history survives brand moves.
- **Status.** Confirmed by Henry, 12 Nov 2026.

### DL-49 — Listed campuses are Campus nodes
- **Decision.** Every campus on GSG's public list is a Campus node, including stage campuses of one school such as Harrods Lower and Upper Primary in Phnom Penh.
- **Why.** The tree matches how GSG describes itself; whether a move between stage campuses is a transfer stays open (OQ-50).
- **Status.** Confirmed by Henry, 12 Nov 2026: each listed stage campus is a separate Campus node. The transfer question is answered by DL-54.

### DL-50 — Organisation candidate enums
- **Decision.** Use the candidate enums in the [Organisation dictionary](../03-domain/organisation-data-dictionary.md#candidate-enums--approved-dl-50) (node type and status, campus status, stages served, data location status, legal entity type and status, operator role, setting override policy).
- **Why.** Designers need fixed labels; same approach as DL-27.
- **Status.** Confirmed by Henry, 12 Nov 2026.

### DL-51 — Access Registry owner
- **Decision.** The GSG platform team owns the Identity and Roles service and the Access Registry, and approves changes to the permission catalogue.
- **Why.** Answers OQ-46; every access screen can name who to ask.
- **Status.** Confirmed by Henry, 12 Nov 2026.

### DL-52 — Support session approval
- **Decision.** A platform-operator or support session into a campus's data needs the Campus Leader's approval. Recommended shape, not yet confirmed: read-only, capped at 4 hours, with security break-glass as the only exception (school notified within 24 hours).
- **Why.** Answers OQ-48; no standing operator access to school data.
- **Status.** Approver confirmed by Henry, 12 Nov 2026; session limits Assumed.

### DL-53 — Capability naming
- **Decision.** Every capability code follows `module.object.action`. New module capabilities are reviewed by the registry owner (DL-51).
- **Why.** Answers OQ-49; names stay readable in templates and access reviews.
- **Status.** Naming confirmed by Henry, 12 Nov 2026; reviewer follows DL-51.

### DL-54 — Promotion or transfer: the academic year decides
- **Context.** Harrods Phnom Penh splits one school into stage campuses (DL-49), so a year-end move changes campus without being a transfer (OQ-50).
- **Decision.** Moving up a grade into a new academic year is a **promotion**, and the campus may change. Any campus move within the same academic year is a **transfer**. Enrolments end with PROMOTION_ROLLOVER or CAMPUS_TRANSFER accordingly (student dictionary SR-16). The transfer flow never offers a year-end stage move.
- **Why.** Answers OQ-50 with one rule that works for stage campuses and ordinary rollover alike.
- **Still open.** Approvals for a campus-changing promotion, and promotion into another brand or country (OQ-60).
- **Confirmed by.** Henry, 12 Nov 2026.

### DL-55 — Guardian per data location
- **Decision.** A guardian is one global Person with one login (DL-42). The global layer holds only the Person and directory index entry (DL-39). The guardian profile, relationship, addresses and contacts are held in the data location of each linked child, so a parent with children in two countries has two guardian profiles on one Person.
- **Why.** Answers OQ-39. Guardian data stays with the child's data, and the parent still has one login.
- **Still open.** Whether a contact change in one data location applies in the other (OQ-61).
- **Confirmed by.** Henry, 12 Nov 2026.

### DL-56 to DL-61 — Admissions pipeline (proposed)
- **Context.** Second module, Admissions and Enrolment; scope approved by Henry 12 Nov 2026 (enquiry to enrolment handoff).
- **Decision.** See the table above and [admissions dictionary § Decisions proposed](../03-domain/admissions-data-dictionary.md#decisions-dl-56-57-58-60-confirmed-14-nov-2026-dl-59-61-assumed).
- **Why.** Lets the pipeline be designed while the rules other than GIIS Singapore's are unknown.
- **Status.** DL-56, DL-57, DL-58, DL-60 Confirmed by Henry, 14 Nov 2026. DL-59 and DL-61 Assumed, under discussion.

### DL-62…DL-68 — Student Directory v1.0 lock answers
- **Context.** The lock list in the student directory spec (PR #13) gave a recommendation per open item; Henry accepted all of them on 13 Nov 2026, which locked the spec as v1.0.
- **DL-62.** A promotion between stage campuses of one school needs no extra approval. Any other campus change at year end needs both principals, like a transfer (SR-05).
- **DL-63.** A year-end move into another brand or country is a new admission with a new contract, not a promotion. Data moves still need OQ-07.
- **DL-64.** A guardian's own phone, email and address changes apply in every data location they have a profile in. Per-child flags (can-collect, emergency contact, priority, communications) stay per child.
- **DL-65.** AEPP is a placement inside a regular enrolment: class AEPP 1 or AEPP 2 plus a SUPPORT subject choice.
- **DL-66.** The student card shows a "Medical alert" flag, never the detail, to class teachers, transport and security. Detail stays Tier 3 in Wellbeing and Inclusion.
- **DL-67.** Student ID format: fixed prefix, year of first admission, global sequence (e.g. GS-2024-001842). No campus or brand code, so it never changes.
- **DL-68.** Students see their own Tier 1 profile and enrolment only. Partner staff access (rest of OQ-19) stays open.
- **Confirmed by.** Henry, 13 Nov 2026.

## How design direction becomes architecture

The architecture records only what is decided. Design direction lives here, in `04-access/` and in `06-experience/` until it is confirmed.

1. **Assumed:** the choice lives in the design files and in this log. The architecture's open question (if any) links to it as "working design direction (not decided)".
2. **Changed:** don't rewrite the old entry. Add a new DL entry, mark the old one **Superseded by DL-xx** with the date and reason, then update the design files. The trail of superseded entries is the case-study story.
3. **Confirmed:** copy the decision into the architecture (close the open question or add it to the relevant section), mark the DL entry **Confirmed** with date and who confirmed it, and bump the architecture version (v1.0 → v1.1).

## How to add a decision

Add a row to the table and a section below with Context, Decision, Why, Alternatives rejected (if any) and Confirm with. When a stakeholder confirms, change the status and add the date and name.

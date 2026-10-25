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
| DL-17 | Grants use scope-node hierarchy (Group > Country > Campus) | Assumed | 07, A03/A04 |
| DL-18 | Relationship-based access for parents via family links, not grants | Assumed | 07 |
| DL-19 | Record-level ACL for restricted cases and Tier 3 fields | Assumed | 07, Decision D13 |
| DL-20 | Templates have BASE and MODULE categories | Assumed | 07, A08 |
| DL-21 | Time-boxed delegation with reason, max duration, and audit | Assumed | 07 |
| DL-22 | Scope is every GSG brand; GSG operates Campus OS; SmartLearn excluded | **Confirmed** (Henry, 25 Oct 2026) | RES-F01, RES-F02 |

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

## How design direction becomes architecture

The architecture records only what is decided. Design direction lives here, in `04-access/` and in `06-experience/` until it is confirmed.

1. **Assumed:** the choice lives in the design files and in this log. The architecture's open question (if any) links to it as "working design direction (not decided)".
2. **Changed:** don't rewrite the old entry. Add a new DL entry, mark the old one **Superseded by DL-xx** with the date and reason, then update the design files. The trail of superseded entries is the case-study story.
3. **Confirmed:** copy the decision into the architecture (close the open question or add it to the relevant section), mark the DL entry **Confirmed** with date and who confirmed it, and bump the architecture version (v1.0 → v1.1).

## How to add a decision

Add a row to the table and a section below with Context, Decision, Why, Alternatives rejected (if any) and Confirm with. When a stakeholder confirms, change the status and add the date and name.

# 06 — Decision log

**Status:** Living document · started 18 Oct 2026 · **Owner:** Henry

**Tóm tắt (VI):** Các giả định đã chọn để thiết kế được ngay, kèm lý do và phương án đã loại. Viết theo dạng dùng thẳng cho case study: bối cảnh → quyết định → lý do → cần ai xác nhận.

Status values: **Assumed** (chosen by design, needs confirmation) · **Confirmed** (stakeholder agreed) · **Superseded**.

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
- **Decision.** Use the set in [00 § Canonical sample data](./00-product-brief.md#canonical-sample-data); add a synthetic "Group Office" campus for group staff.
- **Why.** Matches the GIIS knowledge files and the architecture; avoids a designer or generator mixing two worlds.
- **Follow-up.** Update `screens/*.html` to this data set.

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

### DL-10 — Status vocabulary
- **Decision.** Adopt the Data Dictionary's candidate enums and the colour table in [05 § Status vocabulary](./05-rules-to-ui.md#status-vocabulary).
- **Why.** Designers need fixed labels now; changing an enum later changes one table.

### DL-11 — Sensitivity tiers
- **Decision.** Tier 1 = Internal, Tier 2 = Confidential, Tier 3 = Restricted; one lock treatment (P-LOCK) for anything above the viewer's level.

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

---

## How design direction becomes architecture

The architecture records only what is decided. Design direction lives here and in files `00`–`05` until it is confirmed.

1. **Assumed:** the choice lives in the design files and in this log. The architecture's open question (if any) links to it as "working design direction (not decided)".
2. **Changed:** don't rewrite the old entry. Add a new DL entry, mark the old one **Superseded by DL-xx** with the date and reason, then update the design files. The trail of superseded entries is the case-study story.
3. **Confirmed:** copy the decision into the architecture (close the open question or add it to the relevant section), mark the DL entry **Confirmed** with date and who confirmed it, and bump the architecture version (v1.0 → v1.1).

## How to add a decision

Add a row to the table and a section below with Context, Decision, Why, Alternatives rejected (if any) and Confirm with. When a stakeholder confirms, change the status and add the date and name.

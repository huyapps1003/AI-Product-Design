# Gap analysis — is product truth ready to design from? (18 Oct 2026)

> **Archived 25 Oct 2026.** Kept as the record of why the design layer exists. File names below refer to the old flat layout: `00` → `06-experience/admin-platform/README.md`, `01` → `02-users/roles-personas.md`, `02`–`05` → `06-experience/admin-platform/`, `06` → `07-decisions/decision-log.md`. Current gaps live in [`../08-open-questions/`](../08-open-questions/open-questions.md).

**Tóm tắt (VI):** Architecture v1 đủ tốt ở tầng *hệ thống* (module, tenancy, data model, roadmap) nhưng **chưa đủ để wireframe**. Thiếu: phạm vi thiết kế, persona rõ ràng (map sang GA/TM), danh sách màn hình, flow có state, và mapping business rule → UI. Cấu trúc `00`–`06` trong PR này lấp các khoảng trống đó.

**Verdict:** Not ready on its own. The Product Architecture answers *what the platform is*; the Employee Data Dictionary answers *what the data is*. Neither answers *what a designer draws*: which screens exist, who sees them, what state they are in, and what the UI does when a rule fires. The files `00-` to `06-` added alongside this review close those gaps for the first design slice.

Reviewed: `Global_Campus_OS_Product_Architecture_v1.md`, `Global_Campus_OS_Employee_Data_Dictionary_v1.md`, `prompts/`, `design-tokens/patterns/`, `screens/*.html`, and the GIIS knowledge files (Handbook 2025-26, Exam and Holiday Schedules 2026-27, Noida Almanac 2025-26).

## What is already strong

| Area | Why it holds up | Where |
|---|---|---|
| Platform shape | 4 apps → 12 modules → 8 core services is clear and stable | Architecture § Domains and modules |
| Tenancy | Group > Country > Campus > Section > Programme offering, with inheritance and effective dating | Architecture § Multi-campus |
| GIIS grounding | Real thresholds and SLAs (90/85% attendance, 2/7/14/21-day complaints, refund tables) | Architecture § How GIIS operates today |
| Employee + access data | 25 entities, keys, cardinality, 20 business rules, 15 open decisions, change events | Data Dictionary |
| Separation of concerns | HR position ≠ access role ≠ academic role; campus affiliation ≠ campus access (BR-06) | Data Dictionary § Modeling boundaries |
| Visual system | MinV tokens, blocks and shells exist; screens can be generated consistently | `design-tokens/`, `prompts/system.md` |

## Gaps that block wireframing

Severity: **P0** = a designer would have to guess; **P1** = they can start but will rework; **P2** = polish.

### G1 — No design scope (P0)
The architecture covers the whole school OS (parent, student, staff, leadership apps; 12 modules; 4 phases). The Data Dictionary and every existing screen cover an **Admin Portal** (Staff and HR + GA/TM access). Nothing says which one is being designed now, what is out of scope, or how success is measured.
→ Fixed in [`00-product-brief.md`](../06-experience/admin-platform/README.md): v1 design scope = the **Admin Platform** (Staff and HR, Global Admin, Team Management, Governance) that gates every campus module.

### G2 — Personas don't exist in either doc (P0)
Henry's five personas (HR Admin, Global Admin, Campus Leader, Module Admin, Teacher) appear in neither document. The architecture lists 17 *school* roles (Principal, Coordinator, Nurse…); the dictionary lists *access constructs* (GA grant, TM membership, access role, template). Architecture open question #9 ("how does role-plus-scope map onto GA/TM?") is unanswered. Without this, no screen can decide what to show or hide.
→ Fixed in [`01-roles-personas.md`](../02-users/roles-personas.md): each persona mapped to GA grants, TM memberships, access roles, data scope, owned entities and screens.

### G3 — Module catalog conflict (P0)
Three different module lists are in play:
- Architecture: 12 campus modules (Admissions and Enrolment … Staff and HR).
- Data Dictionary: `R01 Module Catalog` with no values.
- Screens: TM memberships in "Employee Management", "Academic Allocation", "Timetable" — none of which is one of the 12.

A designer cannot build a module switcher, a grant dialog or a membership table until R01 has values.
→ Proposed canonical R01 list in [`03-ia-module-map.md`](../06-experience/admin-platform/ia-screen-map.md#module-catalog-r01); decision in [`06-decision-log.md`](../07-decisions/decision-log.md) (DL-04).

### G4 — No screen inventory (P0)
Only Staff and HR has screens (4 HTML files). The existing sidebar already shows Campuses, Org Structure, Job Positions, Access & Roles, Permission Templates, Reports and Audit Log, but none has a spec. None of the 12 campus modules has a screen list; each has one line in "Covers".
→ Fixed for the admin slice in [`03-ia-module-map.md`](../06-experience/admin-platform/ia-screen-map.md): 37 screens with IDs, owner persona, entities, MinV shell and blocks. Campus modules get a navigation stub only (designed in later slices).

### G5 — Workflows have no states (P0)
Architecture workflows are numbered happy paths. They lack: actor per step in UI terms, entry points, statuses and transitions, empty/loading/error/denied states, notifications, and what happens on rejection or timeout. Access workflows (onboarding, transfer, exit) don't exist at all even though D07 and D10 depend on them.
→ Fixed in [`02-key-scenarios.md`](../06-experience/admin-platform/scenarios.md) and [`04-flows/`](../06-experience/admin-platform/flows/): 5 scenarios, each with a flow diagram, a state machine and a per-screen state table.

### G6 — Business rules not mapped to UI (P0)
BR-01…BR-20 and decisions D01…D15 say what the system must do but not how the screen behaves (disabled control? warning banner? blocking dialog? hidden field?). Example: BR-15 (TM membership needs an active GA grant) could be a disabled campus option, an inline error, or an auto-created access request — each a different design.
→ Fixed in [`05-rules-to-ui.md`](../06-experience/admin-platform/rules-to-ui.md): every BR and D mapped to a concrete UI behavior, screen and component.

### G7 — Permission evaluation undefined (P0, from D08/D09)
"Effective permissions" is the core screen of an access product, and it can't be drawn without knowing how multiple roles, deny entries, data-scope groups and missing scope combine.
→ Default chosen (deny wins, union of allows, AND inside a scope group, OR across groups, no scope = no data) in DL-07 and shown in [`05-rules-to-ui.md`](../06-experience/admin-platform/rules-to-ui.md). Needs stakeholder confirmation.

### G8 — Contradictory placeholder content (P1)
Existing screens use campuses **Singapore, Hong Kong, Kuala Lumpur, Jakarta, Global HQ**, the domain `globalcampus.edu`, legal entities like "Global Campus Hong Kong Limited", and **Microsoft Azure AD** as IdP. The architecture names **SMART Campus Punggol, East Coast (Singapore) and Noida (India)** and **Google Workspace** SSO. A generator reading both will mix them.
→ Canonical sample data set in [`00-product-brief.md`](../06-experience/admin-platform/README.md#canonical-sample-data) (DL-03). Existing screens should be updated in a follow-up.

### G9 — Status vocabulary and badge colours (P1, from D15)
Enums are "candidates only", and there are four overlapping lifecycles (employee record, employment, account, membership). Designers need one status → label → colour table.
→ Adopted candidate enums with MinV colours in [`05-rules-to-ui.md`](../06-experience/admin-platform/rules-to-ui.md#status-vocabulary) (DL-10).

### G10 — Sensitivity tiers don't line up (P1)
Architecture uses Tier 1/2/3; the dictionary uses Internal/Confidential/Restricted. D13 (who sees which field) is open. Designers need one visual treatment for restricted data.
→ Mapping and field-visibility matrix in [`05-rules-to-ui.md`](../06-experience/admin-platform/rules-to-ui.md#field-visibility-decision-d13) (DL-11).

### G11 — Who approves access? (P1)
GA grants show `granted_by_user_id` but no request/approval flow. Teachers have no way to ask for access; transfers say "initiate review workflow" (D07) without defining it.
→ Access Request and Access Review cases defined (reusing the architecture's shared **Case** object) in DL-08 and [`04-flows/S2`](../06-experience/admin-platform/flows/S2-campus-transfer.md), [`S3`](../06-experience/admin-platform/flows/S3-employee-exit.md).

### G12 — Shell and navigation behaviour (P1)
Not defined: how the campus/scope switcher behaves for a multi-campus user, whether Admin Platform is a separate portal or part of the staff web app, and what a user with no grants sees.
→ Shell spec in [`03-ia-module-map.md`](../06-experience/admin-platform/ia-screen-map.md#shell) (DL-05, DL-06).

### G13 — ID collision D01–D03 (P2)
`D01`–`D03` are both **entities** (Academic Role Assignment, Teacher Subject Allocation, Timetable Teaching Session) and **decisions** (Source of truth, Employee identifiers, Concurrent employments). Henry's "D01–D15" means the decisions.
→ This folder always writes "Decision D0x" vs "entity D0x". Recommend renaming the entities to `X01`–`X03` in Data Dictionary v1.1 (DL-12).

### G14 — Mislabelled rule references in screens (P2)
`employee-add.html` cites **BR-13** for "must not create reporting cycles"; BR-13 is `valid_to ≥ valid_from`. The no-cycle rule lives only in E04's attribute note.
→ Added as **BR-21** proposal in [`05-rules-to-ui.md`](../06-experience/admin-platform/rules-to-ui.md); fix the screen in the follow-up.

### G15 — Bulk import and HRIS sync undefined (P2)
Screens show "Import CSV"; the dictionary has E08 mapping and BR-20 (quarantine conflicts), but no import or conflict-resolution flow.
→ Screens SCR-HR-09/10 listed in the IA; full flow deferred until Decision D01 (source of truth) lands.

### G16 — Device and accessibility targets (P2)
No stated breakpoints per persona. The architecture says staff-heavy screens work best on web; teachers will check their profile on mobile.
→ Desktop-first for admin personas, mobile-ready for Teacher self-service (DL-06).

## Module-by-module readiness

| # | Module | Screen-level detail today | Ready to wireframe after this PR? |
|---|---|---|---|
| 12 | Staff and HR | 4 screens, full data dictionary | **Yes** |
| — | Global Admin (platform) | Nav items only | **Yes** (screens SCR-GA-*) |
| — | Team Management (per module) | One table inside the profile screen | **Yes** (screens SCR-TM-*) |
| — | Governance (audit, access review) | Timeline inside profile | **Yes** (screens SCR-GOV-*) |
| 3 | Teaching and Learning (Academic Allocation, Timetable) | Referenced read-only from HR profile | Partly: read-only panels only; own module spec needed |
| 1, 2, 4–11 | Remaining campus modules | One line each | **No**: need their own data dictionary + scenarios, following this folder's pattern |

## What still needs a human decision

The P0 decisions D01–D10 from the Data Dictionary remain open. This folder designs against an explicit default for each (see `06-decision-log.md`), so design can proceed, but these must be confirmed before the schema freezes. The three with the largest UI impact:

1. **Decision D08** — permission evaluation (drives the Effective Permissions panel).
2. **Decision D09** — whether per-member overrides exist (adds or removes a whole tab).
3. **Decision D01** — internal master vs external HRIS (decides whether HR fields are editable or read-only with a "synced from" badge).

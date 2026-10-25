# Admin Platform — information architecture and screen map

**Status:** Proposed, 18 Oct 2026 · Closes gaps G3, G4, G12

**Tóm tắt (VI):** Định nghĩa shell (thanh scope campus, module switcher), danh mục module chuẩn cho R01, cây điều hướng của Admin Platform và **danh sách 37 màn hình** có ID, persona, entity, layout MinV và kịch bản liên quan. Designer chọn một ID và bắt đầu wireframe.

## Shell

One staff web app (`Staff app and web` in the architecture). Admin Platform is not a separate product; it is a group of navigation sections that appear when the user holds the matching roles (DL-05).

```
┌────────────────────────────────────────────────────────────────────────────┐
│ [GIIS logo]  Scope: Singapore › SMART Campus Punggol ▾   🔍 Search   🔔  (MI)│  AppBar 64px
├───────────────┬────────────────────────────────────────────────────────────┤
│ Home          │  Breadcrumb                                                │
│ ── Campus ──  │  Page title                         [secondary] [primary]  │
│ Attendance…   │                                                            │
│ Teaching…     │  page content (max 1200px, 40px padding)                   │
│ ── Admin ──   │                                                            │
│ Staff and HR  │                                                            │
│ Global Admin  │                                                            │
│ Governance    │                                                            │
│ ── Me ──      │                                                            │
│ My profile    │                                                            │
└───────────────┴────────────────────────────────────────────────────────────┘
 Sidebar 280px (88px collapsed) — MinV dashboardShell
```

The logo shows GIIS because the sample data is GIIS-only. How brand identity appears for users who work across brands is open (OQ-26); the scope switcher has no brand level (OQ-14).

**Scope switcher rules**

| Situation | Behaviour |
|---|---|
| User has GA campus grants for 1 campus | Switcher shows the campus as static text, no dropdown |
| 2+ campuses | Dropdown grouped by country; last used campus remembered per user |
| Global roles (GLOBAL_ADMIN, global HR_ADMIN, PLATFORM_AUDITOR) | Extra option "All campuses" at top; lists show a Campus column only in this mode |
| Switching campus | Sidebar re-renders to modules with an active TM membership on that campus; current page reloads if allowed, else goes to Home with a toast "You don't have *Module* at *Campus*" |
| Campus INACTIVE (E06) | Hidden from switcher except for Global Admin, shown with "Inactive" chip |

**Sidebar rules**
- Campus modules appear only if the user has an active membership for the selected campus (not just a GA grant). A GA grant without membership shows the module greyed with tooltip "Access granted, waiting for team setup" (S1).
- Admin sections appear by role: Staff and HR (`HR_ADMIN`, `HR_VIEWER`, `CAMPUS_LEADER` read); Global Admin (`GLOBAL_ADMIN`; `CAMPUS_LEADER` sees "Users & access" read-only); Governance (everyone with any review task, `CAMPUS_LEADER`, `PLATFORM_AUDITOR`, `GLOBAL_ADMIN`).
- Every module page has a **Team** tab (Team Management) visible to `MODULE_ADMIN` of that module + campus.
- Notification bell lists: access requests awaiting me, review cases assigned to me, my own request outcomes.

## Module catalog (R01)

Proposed `module_code` values so GA grants, TM memberships and the switcher use one list (DL-04).

| `module_code` | Name | Type | Has Team tab | In v1 design |
|---|---|---|:-:|:-:|
| `admissions` | Admissions and Enrolment | Campus | ● | stub |
| `student_records` | Student Records | Campus | ● | stub |
| `teaching_learning` | Teaching and Learning (includes Academic Allocation and Timetable) | Campus | ● | read-only panels |
| `assessment` | Assessment and Reporting | Campus | ● | stub |
| `attendance_conduct` | Attendance and Conduct | Campus | ● | Team tab only (S4) |
| `wellbeing` | Wellbeing and Inclusion | Campus | ● | stub |
| `fees_finance` | Fees and Finance | Campus | ● | stub |
| `campus_services` | Campus Services | Campus | ● | stub |
| `communication_cases` | Communication and Cases | Campus | ● | stub |
| `activities_awards` | Activities and Awards | Campus | ● | stub |
| `careers` | Careers and Universities | Campus | ● | stub |
| `staff_hr` | Staff and HR | Campus + Admin | ● | **full** |
| `global_admin` | Global Admin | Platform | — | **full** |
| `governance` | Governance (access review, audit, reports) | Platform | — | **full** |

Mapping of names used in existing screens: "Employee Management" → `staff_hr`; "Academic Allocation" → `teaching_learning` (capabilities `allocation.*`); "Timetable" → `teaching_learning` (capabilities `timetable.*`).

## Navigation tree (Admin Platform)

```
Home ................................................. SCR-HOME-01
Staff and HR
├─ HR Dashboard ...................................... SCR-HR-01
├─ Employees ......................................... SCR-HR-02
│  ├─ Employee profile (tabs) ........................ SCR-HR-03
│  ├─ Add employee (wizard) .......................... SCR-HR-04
│  ├─ End employment (dialog) ........................ SCR-HR-05
│  └─ Add / end campus assignment (drawer) ........... SCR-HR-06
├─ Org structure ..................................... SCR-HR-07
├─ Job positions ..................................... SCR-HR-08
└─ Import and sync ................................... SCR-HR-09 → SCR-HR-10
Global Admin
├─ Campuses .......................................... SCR-GA-01 → SCR-GA-02
├─ Users and access .................................. SCR-GA-03 → SCR-GA-04
│  └─ Grant module / campus (dialog) ................. SCR-GA-05
├─ Modules ........................................... SCR-GA-06
├─ Capabilities ...................................... SCR-GA-07
├─ Access roles ...................................... SCR-GA-08
└─ Permission templates .............................. SCR-GA-09 → SCR-GA-10
[Any module] › Team
├─ Members ........................................... SCR-TM-01
├─ Add member (wizard) ............................... SCR-TM-02
├─ Member detail ..................................... SCR-TM-03
├─ Data scope builder (drawer) ....................... SCR-TM-04
└─ AY rollover (bulk) ................................ SCR-TM-05
Governance
├─ My tasks (requests and reviews) ................... SCR-GOV-01 → SCR-GOV-02
├─ Campus access review (attestation) ................ SCR-GOV-03
├─ Audit log ......................................... SCR-GOV-04
└─ Reports ........................................... SCR-GOV-05
Campus overview (Campus Leader home) ................. SCR-CL-01
Me
├─ My profile and access ............................. SCR-ME-01
└─ Request access (dialog) ........................... SCR-ME-02
System
├─ Sign in (Google Workspace SSO) ..................... SCR-AUTH-01
├─ No access / 403 ................................... SCR-SYS-01
└─ Not found / 404 ................................... SCR-SYS-02
```

## Screen list

Shell names and blocks are from `design-tokens/patterns/screens.json` and `design-tokens/patterns/blocks/`. "Exists" means an HTML mock is in `screens/`; it may need the fixes listed in the [gap analysis](../../_archive/gap-analysis-2026-10-18.md) (G8, G14).

### Staff and HR

| ID | Screen | Personas | Entities | Shell · blocks | Key content and actions | Scenarios | State |
|---|---|---|---|---|---|---|---|
| SCR-HR-01 | HR Dashboard | HR Admin, Campus Leader (read) | E01, E02, E07, A14 | dashboardShell · page-header, stats-row, chart-card, data-table | KPIs: active employees, by category, new hires this month, upcoming exits (30 d), pending access reviews; headcount by campus; recent workforce changes | S1, S3 | Exists (`hr-dashboard.html`) |
| SCR-HR-02 | Employee Directory | HR Admin, Campus Leader, Global Admin (read) | E01, E02, E04, E05, E07 | dashboardShell · page-header, filter-bar, data-table, empty-state | Filters: campus, category, employment type, status, org unit; columns: name+email, category, position, primary campus (+N), status, start date; actions: Add, Import, Export | S1–S3 | Exists |
| SCR-HR-03 | Employee Profile | HR Admin, Campus Leader, Global Admin, self | E01–E07, A01, A02, entity D01–D03, A14 | dashboardShell · detail-header + tabs | Tabs: **Overview**, **Employment** (history timeline), **Campus assignments**, **Access** (read summary + link to SCR-GA-04), **Academic** (read-only from Teaching and Learning), **Audit** | S1–S3, S5 | Exists |
| SCR-HR-04 | Add Employee (wizard) | HR Admin | E01, E02, E04, E07 | dashboardShell · stepper + form-section | 4 steps: Person → Employment → Org assignment → Campus assignment; duplicate-person check after step 1; summary step shows "Access is not granted here; an onboarding request will be created" | S1 | Exists (needs duplicate check + summary step) |
| SCR-HR-05 | End employment (dialog) | HR Admin | E02, A03–A05 | dialog · form-section | End date, reason code, preview of access to review, "Revoke immediately" toggle | S3 | New |
| SCR-HR-06 | Add / end campus assignment (drawer) | HR Admin | E07 | drawer · form-section | Campus, reason, valid from/to, primary flag; overlap warning; access-review notice | S2 | New |
| SCR-HR-07 | Org structure | HR Admin | E03, E04 | dashboardShell · tree + detail panel (listDetailShell) | Tree by organisation → division → department → team; members per unit; inactive units greyed | — | New |
| SCR-HR-08 | Job positions | HR Admin | E05 | dashboardShell · data-table | Code, title, job family, status, # holders | — | New |
| SCR-HR-09 | Import employees | HR Admin | E01, E02, E07, E08 | dashboardShell · upload + stepper + data-table | Upload CSV or choose HRIS source → map columns → validate → preview creates/updates/conflicts → commit | — | New (blocked by Decision D01 for HRIS mode) |
| SCR-HR-10 | Sync conflicts | HR Admin | E08 | dashboardShell · data-table + side-by-side diff | Quarantined records with source vs master values; accept source / keep master / merge | — | New |

### Global Admin

| ID | Screen | Personas | Entities | Shell · blocks | Key content and actions | Scenarios | State |
|---|---|---|---|---|---|---|---|
| SCR-GA-01 | Campuses | Global Admin | E06 | dashboardShell · data-table | Code, name, country, time zone, status, # staff, # users with access | — | New |
| SCR-GA-02 | Campus detail | Global Admin, Campus Leader (read) | E06, A04, A05 | dashboardShell · detail-header + tabs | Settings, users with campus grant, modules active, audit | S5 | New |
| SCR-GA-03 | Users and access | Global Admin, Campus Leader (read, own campus) | A01, A02, A03, A04 | dashboardShell · filter-bar, data-table | User, linked employee, account status, last login, # modules, # campuses, flags (no employee link, inactive 60 d, has override) | S1, S5 | New |
| SCR-GA-04 | User access detail | Global Admin, Campus Leader (read), Module Admin (own module) | A01–A13 | dashboardShell · detail-header + tabs | Tabs: **Grants** (GA module × campus matrix), **Memberships** (by module/campus with role, template version, scope), **Effective permissions** (capability search + explanation chain), **Requests**, **Audit** | S1–S5 | New |
| SCR-GA-05 | Grant module / campus (dialog) | Global Admin | A03, A04 | dialog · form-section | Modules × campuses checklist, valid from/to, reason (required), linked request | S1, S2 | New |
| SCR-GA-06 | Modules | Global Admin | R01 | dashboardShell · data-table | Code, name, status, # capabilities, # templates, # members | — | New |
| SCR-GA-07 | Capabilities | Global Admin | R02 | dashboardShell · filter-bar, data-table | Filter by module and action type; sensitivity flag for Restricted-data capabilities | S5 | New |
| SCR-GA-08 | Access roles | Global Admin | A06 | dashboardShell · data-table | Role code, name, category, module, assignable, # assignments | — | New |
| SCR-GA-09 | Permission templates | Global Admin, Module Admin (read) | A08 | dashboardShell · filter-bar, data-table | Template family, type (DEFAULT/STANDARD/ADD_ON), module, latest version, status, # bindings per version | S4 | New |
| SCR-GA-10 | Template editor | Global Admin | A08, A09, R02 | dashboardShell · page-header + capability matrix | Capability rows grouped by action type with Allow / Deny / Not set; version banner; "Publish v*n*" creates a new version; diff vs previous; impact preview (# members on older versions) | S4 | New |

### Team Management (inside any module)

| ID | Screen | Personas | Entities | Shell · blocks | Key content and actions | Scenarios | State |
|---|---|---|---|---|---|---|---|
| SCR-TM-01 | Team members | Module Admin | A05, A07, A10, A12 | dashboardShell · page-header, filter-bar, data-table | Member, role(s), template + version, scope summary ("7B, 7C · Maths · AY 26-27"), status, valid to; banners: pending members from onboarding, AY rollover due | S1, S4 | New |
| SCR-TM-02 | Add member (wizard) | Module Admin | A05, A07, A10, A12/A13 | drawer · stepper + form-section | Pick person (only people with GA grant for this module + campus are selectable; others show "Request access") → role → template version → data scope → effective date → review | S1, S2 | New |
| SCR-TM-03 | Member detail | Module Admin | A05, A07, A10, A11, A12/A13, A14 | drawer or page · tabs | Roles, template binding, scope, overrides (if enabled), effective permissions for this module, audit | S4, S5 | New |
| SCR-TM-04 | Data scope builder | Module Admin | A12, A13 | drawer · form-section | Groups of conditions (AND inside, OR across); dimension pickers for campus, academic year, curriculum, grade, section, subject, org unit; live "matches N students / M sections" preview | S4 | New |
| SCR-TM-05 | AY rollover | Module Admin | A05, A07, A10, A12, entity D02 | dashboardShell · data-table (bulk edit) | Members × suggested new scope from allocations; template upgrade choice; effective date; diff preview; apply | S4 | New |

### Governance, Campus Leader, self-service, system

| ID | Screen | Personas | Entities | Shell · blocks | Key content and actions | Scenarios | State |
|---|---|---|---|---|---|---|---|
| SCR-GOV-01 | My tasks | all approvers | Case (Access Request, Access Review) | dashboardShell · filter-bar, data-table or kanban | Type, subject person, campus, module, due (SLA), status; tabs: Assigned to me / Raised by me / All (admin) | S1–S3, S5 | New |
| SCR-GOV-02 | Case detail | approvers | Case, A03–A07, A14 | dashboardShell · detail-header + timeline + decision panel | What changed (diff), proposed access, per-line decision (approve / reject / modify), comments, SLA timer, audit | S1–S3 | New |
| SCR-GOV-03 | Campus access review | Campus Leader | A03–A13 | dashboardShell · stats-row, filter-bar, data-table (bulk) | Progress, risk flags, keep/reduce/revoke per row, submit attestation | S5 | New |
| SCR-GOV-04 | Audit log | Global Admin, Auditor, Campus Leader (campus), HR Admin (HR events) | A14 | dashboardShell · filter-bar, data-table + detail drawer | Time, actor, action code, entity, campus, reason; redacted before/after in drawer; export | S5 | Partial (timeline in profile) |
| SCR-GOV-05 | Reports | Global Admin, HR Admin, Campus Leader | all | dashboardShell · card-grid of reports | Headcount, access by module × campus, Restricted-capability holders, dormant accounts, exits with open access | S5 | New |
| SCR-CL-01 | Campus overview | Campus Leader | E07, A04, A05, Case | dashboardShell · stats-row, chart-card, data-table | Staff on campus, access holders, pending approvals, review progress, recent changes on campus | S2, S5 | New |
| SCR-ME-01 | My profile and access | every employee | E01, E07, A03–A07 | dashboardShell (mobile-first) · detail-header, cards | My details (editable contact fields), my campuses, my modules per campus with role, my open requests | S1 | New |
| SCR-ME-02 | Request access (dialog) | every employee, Module Admin on behalf | Case | dialog · form-section | Module, campus, role wanted, reason, until date | S1, S2 | New |
| SCR-HOME-01 | Home | everyone | — | dashboardShell · card-grid | Role-based cards: my tasks, my modules, shortcuts | all | New |
| SCR-AUTH-01 | Sign in | everyone | A01 | authShell | "Sign in with Google" only for staff; error states for suspended / not linked | S1 | New |
| SCR-SYS-01 | No access | everyone | — | dashboardShell · empty-state | Explains which part is missing (no grant / no membership / no scope / account suspended) + "Request access" | S1, S4 | New |
| SCR-SYS-02 | Not found | everyone | — | dashboardShell · empty-state | Standard 404 | — | Exists (`empty.html` pattern) |

## Campus modules (stubs for later slices)

Each campus module will need, at minimum: module home, list, detail, create/edit, Team tab (SCR-TM-*), and module settings. Their screen lists come with their own data dictionaries; this file only fixes their names, codes and placement in the sidebar.

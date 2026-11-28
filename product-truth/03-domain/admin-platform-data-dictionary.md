# Global Campus OS — Admin Platform Data Dictionary v0.1 (draft)

**Status:** Proposed draft, 28 Nov 2026. DL-165…DL-167 Confirmed; DL-168…DL-179 Assumed, awaiting Henry; OQ-194…OQ-209 Open · **Owner:** Henry
**Cites:** [Architecture v1](../00-sources/Global_Campus_OS_Product_Architecture_v1.md) § Multi-campus and tenancy model, § Users and roles, § Security ("cross-campus access is granted, time-boxed and logged"; Tier 3 visible only to named roles, every read logged), § Shared core services (Identity and roles, Tenancy and config, Events and audit), § Integrations (Google Workspace, payroll, ERP) · [Employee Data Dictionary v1](../00-sources/Global_Campus_OS_Employee_Data_Dictionary_v1.md) A01–A14, R01, R02, BR-06…BR-17, Decisions D07…D13 · [architecture.md](./architecture.md) AR-01…AR-14, AP-01…AP-14 · [organisation dictionary](./organisation-data-dictionary.md) OR01–OR09 · [academic structure dictionary](./academic-structure-data-dictionary.md) AC06, AC07 · [cases & communications dictionary](./cases-communications-data-dictionary.md) CC01 access case types · [staff & HR dictionary](./staff-hr-data-dictionary.md) E02, E07, HR15 · [04-access/](../04-access/README.md) rbac-model, permission-catalogue, role-catalogue · [data-controller-model](../05-data-governance/data-controller-model.md) · [Admin Platform slice](../06-experience/admin-platform/README.md) S1–S5, SCR-GA/TM/GOV · DL-02, DL-04, DL-05, DL-07…DL-09, DL-13, DL-14, DL-16…DL-21, DL-22, DL-32…DL-53, DL-69, DL-73, DL-96, DL-118, DL-147, DL-149, DL-160, DL-163 · OQ-01, OQ-05, OQ-06, OQ-09, OQ-12, OQ-15, OQ-16, OQ-18, OQ-20, OQ-21, OQ-26, OQ-27, OQ-32, OQ-34, OQ-43, OQ-58, OQ-192
**Tóm tắt (VI):** Từ điển dữ liệu cho Admin Platform, "xương sống" của ứng dụng web cho nhân viên (DL-118): mở brand và campus (checklist go-live, bật module theo campus, sổ "hệ thống nào là nguồn" khi chuyển đổi), đăng nhập và tài khoản (nhà cung cấp danh tính theo brand), vai trò và quyền truy cập (nhà của A01–A17, R01–R03 từ Employee DD v1, thêm A18 phiên hỗ trợ của GSG), rà soát quyền định kỳ, và cài đặt bảo vệ dữ liệu (danh mục mục đích, quy tắc lưu giữ, thông báo quyền riêng tư; bên kiểm soát dữ liệu vẫn để trống đến khi có OQ-06). Mọi quyết định mới (DL-165…DL-179) đang là giả định chờ Henry.

> Drafted by Claude from product truth on 28 Nov 2026 for Henry's review. The Admin Platform has had a design slice since 18 Oct 2026 ([06-experience/admin-platform](../06-experience/admin-platform/README.md)) but no dictionary of its own: its access entities still live in the Employee Data Dictionary v1 (Proposed) and the RBAC model, and brand or campus go-live, module switch-on, sign-in, purposes, retention and support access had no entities. This file covers **how GSG and each brand set Campus OS up, who can sign in, how access is given, reviewed and explained, and the data protection settings every module reads.** The scope tree, legal entities and setting values stay in the [organisation dictionary](./organisation-data-dictionary.md); curricula, programmes and years stay in [academic structure](./academic-structure-data-dictionary.md).

## How to read this file

Same layout and rules as the [Student Directory Spec](./student-directory-data-dictionary.md#how-to-read-this-file): **Known** (cited), **Confirmed** (DL by Henry), **Assumed** (DL, expect change), **Open** (OQ; keep generic in designs); tiers T1/T2/T3 (DL-11); `Y`/`N`/`C` required. Rows carry the shared common fields (`created_at`, `created_by_user_id`, `updated_at`, `updated_by_user_id`, `row_version`, `source_system_code`). Global entities (DL-39) hold no personal data beyond the Tier 1 Person and account; cell entities also carry `tenant_path` and `data_region` (UI term **Data location**, DL-47). GIIS facts are GIIS examples, not group rules (DL-22). People in samples are synthetic.

## Scope

Proposed on a decision card on 28 Nov 2026 (recommended "Setup, access and data protection"; scope recorded as DL-165, **Confirmed** by Henry on 28 Nov 2026).

| In this dictionary | Not in this dictionary (owner) |
|---|---|
| Brand and campus setup: a setup run with a checklist and readiness gates before a campus goes live | Scope tree, brands, brand-countries, campuses, segments, legal entities, campus operators, setting values (organisation dictionary OR01–OR09) |
| Module switch-on per scope node (which modules a campus uses, from when) | Curricula, programmes, academic years and terms (academic structure AC01–AC09); the setup checklist only links to them |
| Legacy system authority register: which system is the source of truth per module and field group until each cutover date (DL-45) | Import mechanics and per-record mappings (ST10, E08, each module's mapping table) |
| Setting definitions: every setting key once, with owner module, lowest level and default | The values themselves (OR09); each module's own rules |
| Sign-in: identity providers per brand, accounts, account links, account lifecycle, service accounts | Staff records, employments, positions (Staff & HR E01–E08); guardian and student records (Student Records) |
| Access: module catalogue, capabilities, access roles, templates, grants, memberships, role assignments, data scope, record ACL, delegation, Tier 3 type grants, family access (home of Employee DD A01–A17, R01–R03; F01 refined) | Capability lists per module (each module dictionary adds rows to the [permission catalogue](../04-access/permission-catalogue.md), DL-33) |
| GSG support access: operator sessions and break-glass (new A18) | Platform security operations, infrastructure, incident response tooling |
| Access reviews: termly attestation cycles that open one Case per campus | The Case engine, deadlines and notifications (Cases & Communications CC01–CC09) |
| Data protection settings: purpose catalogue, retention rules, privacy notices, data protection contacts | Consent records and data-sharing records for transfers (OBJ-17, OBJ-18; Governance, later); who the controller is (OQ-06) |
| Audit event (A14) as the record every change writes | Audit storage and search engine (Platform services) |

## Modeling boundaries

- **Admin Platform is screens, not a separate product (DL-05, Confirmed).** It is a set of navigation sections in the staff web app (DL-118) shown to people with the matching roles. Its data is written by two domains (DL-43): **Tenancy and config** (PA01–PA06, plus OR01–OR09) and the **Identity and Access Registry** (A01–A18, R01–R03, F01, PA07–PA10). Capability prefixes stay as they are: `ga` (module `global_admin`), `tm` (team management inside any module), `gov` (module `governance`), `identity`, `org` (DL-53).
- **This file is the home of the access entities (DL-165).** Employee DD v1 A01–A14 and R01–R02, and the A15–A17, R03 and F01 entities added by the RBAC decisions (DL-19, DL-36, DL-37), are refined here with their IDs kept, the way Staff & HR took over E01–E08 (DL-146). The source file stays unchanged in `00-sources/`. A18 Operator session is new. New non-access entities use the prefix **PA**.
- **A grant is a ceiling; a membership gives work (DL-02, Confirmed; DL-35, DL-40).** A module grant (A03) says a person may work in a module at a scope node and below. A scope grant (A04) puts the person on a node (scope switcher, campus directory, self-service). A membership (A05) at that node or below gives a role, a template and a data scope. Nothing in this file gives data access without a membership, a family link, an ACL entry, a delegation, a Tier 3 type grant or an approved operator session.
- **Three ways access arrives (DL-171).** (1) **Automatically from facts**, through the Registry only: the position's standard access template on a campus assignment (DL-149), class-teacher and teaching scope (DL-73, DL-133), cover scope for one day (DL-145). (2) **By request**: an ACCESS_REQUEST case approved by the Module Admin and, for anything beyond the campus, the next level up. (3) **By an admin directly**: Module Admins add members inside the grant ceiling; Brand and Global Admins set ceilings. Every route writes the same records and audit events, and "Why can I see this?" names the route (DL-38).
- **Campus go-live is a gated run (DL-167).** A campus moves from PLANNED to ACTIVE (OR04, DL-50) only through a setup run whose blocking tasks are done: tree node and operator link, data location live, sign-in connected, at least one programme with a published academic year (AC06, AC07), Campus Leader and Module Admins assigned, module switch-on dates set, and the authority register filled for every switched-on module. Module data screens are empty and closed to members until the module is LIVE at that campus.
- **One account per Person, signed in through the brand's identity provider (DL-169).** Staff sign in with single sign-on and MFA at their brand's identity provider (GIIS: Google Workspace, Known). One Person (DL-42) has one Campus OS account (A01) and may have several sign-in identities, for example a teacher with a GIIS and an OWIS email. Campus OS never stores passwords. Parent and student sign-in come with their apps (DL-118) and are not designed here.
- **Accounts follow employment but are managed on their own (DL-170).** HR creates the employment; the Registry creates the account in INVITED state and sends the invite a set number of days before the start date (DL-13, brand-country setting). It becomes ACTIVE on first sign-in, SUSPENDED when the employment is suspended (DL-163) or on an admin action, and DISABLED the day after the last working day (DL-160, BR-10). Accounts with no sign-in for a set number of days are flagged Dormant for the access review, not suspended automatically (OQ-204).
- **Roles and templates are defined centrally (DL-172).** The GSG platform team (DL-51) keeps the access role catalogue (A06), templates (A08, A09) and capabilities (R02). Templates are versioned and published; a published version never changes. Brands don't create roles in v1; a brand asks the platform team for a template change (OQ-198). Per-member overrides stay off (DL-14, held).
- **Position templates map HR positions to access (DL-173).** Each brand keeps a list "position → standard access template per module" used by DL-149. Brand HR proposes the list, the platform team checks it against the catalogue and publishes it. A position template never includes Tier 3 capabilities. This answers the "who maintains" part of OQ-192 as a proposal; the contents per position stay open.
- **Brand admins work inside one brand (DL-178).** A new access role, **Brand Admin**, holds `ga.*` capabilities at a Brand node: campus setup runs, module switch-on, grants and accounts for that brand's people, read-only on definitions. Global Admin (GSG platform team) keeps definitions, Group-level grants and data protection settings. Granting Global Admin or Brand Admin needs a second Global Admin (DL-09, held).
- **Access is reviewed every term (DL-174).** A review cycle (PA10) opens one CAMPUS_ATTESTATION case per campus at the start of each term (S5). The Campus Leader keeps, reduces or removes each person's access, flagged rows first (Tier 3 holders, cross-campus, dormant, leaving within 30 days, no employee link). Rows left undecided at the due date escalate to the brand-country head; access is never removed automatically by a missed review.
- **GSG support never has standing access to school data (DL-175; approver Confirmed DL-52).** An operator session (A18) is requested by a named GSG support engineer for one campus, one purpose and a set time, approved by the Campus Leader, read-only, at most 4 hours, and every screen they open is logged. Break-glass is for security incidents only, opened by GSG security, and the Campus Leader is told within 24 hours.
- **Data protection settings name no controller (DL-176; AR-04).** The purpose catalogue (PA07), retention rules (PA08) and privacy notices (PA09) are records the Admin Platform edits. Legal fields (lawful basis, controller, periods where unknown) stay empty with a pending marker until counsel answers OQ-06, OQ-09 and OQ-43. A data protection contact per brand-country (A06 role **Data Protection Lead**) reads audit and retention reports for their node but has no record access by default (answers part of OQ-34).
- **Global definitions, local assignments (DL-39, DL-179).** Persons, accounts, sign-in identities, roles, templates, capabilities, module grants, scope grants, the module switch-on table, setting definitions, purposes and the authority register are global (Tier 1). Memberships, data scope, ACLs, delegations, Tier 3 type grants, operator sessions, review cases and audit events live in the campus's data location. Admin screens show the data location on every cell record; a Group or Brand admin sees other countries' memberships as counts unless they hold access there (AR-01).

## Entities

| ID | Entity | Home | Status |
|---|---|---|---|
| Person | Person | Global | Confirmed (DL-42); first dictionary definition (cross-module review OQ-234) |
| A01 | User Account (refined) | Global | Known entity (Employee DD A01); one per Person Confirmed (DL-42); lifecycle Assumed (DL-170) |
| A02 | Account Link (refined from Employee Account Link) | Global | Known entity (Employee DD A02); widened to Person profiles (DL-169) |
| A03 | Module Grant (refined) | Global | Known entity (Employee DD A03); scope node Confirmed (DL-17, DL-40) |
| A04 | Scope Grant (refined from GA Campus Grant) | Global | Known entity (Employee DD A04); scope node Confirmed (DL-40); meaning Assumed (DL-171) |
| A05 | Module Membership (refined) | Cell | Known entity (Employee DD A05); scope node Confirmed (DL-35) |
| A06 | Access Role | Global | Known entity (Employee DD A06); owner Confirmed (DL-51); new roles Assumed (DL-176, DL-178) |
| A07 | Role Assignment | Cell | Known entity (Employee DD A07) |
| A08 | Permission Template | Global | Known entity (Employee DD A08); BASE/MODULE Confirmed (DL-20) |
| A09 | Template Capability | Global | Known entity (Employee DD A09); deny wins Confirmed (DL-07) |
| A10 | Role Template Binding | Cell | Known entity (Employee DD A10) |
| A11 | Member Permission Override | Cell | Known entity (Employee DD A11); off by default (DL-14, held Assumed) |
| A12 | Data Scope Policy | Cell | Known entity (Employee DD A12) |
| A13 | Data Scope Condition | Cell | Known entity (Employee DD A13); derived sources Confirmed (DL-16, DL-37) |
| A14 | Audit Event | Cell, redacted global index | Known entity (Employee DD A14); purpose added (DL-44, AR-10) |
| A15 | Record ACL Entry | Cell | Confirmed (DL-19) |
| A16 | Delegation | Cell | Confirmed entity (DL-36); 90-day cap Assumed (DL-21, held) |
| A17 | Tier 3 Type Grant | Cell | Confirmed entity (DL-36); approver Confirmed (DL-69) |
| A18 | Operator Session | Cell | New; approver Confirmed (DL-52); shape Assumed (DL-175) |
| R01 | Module | Global | Known entity (Employee DD R01); codes Confirmed (DL-04) |
| R02 | Capability | Global | Known entity (Employee DD R02); naming Confirmed (DL-53) |
| R03 | Parent Capability Set | Global | Confirmed (DL-18, DL-28, DL-37) |
| F01 | Family Access Link | Cell | Confirmed (DL-18, DL-37); states pending (OBJ-09) |
| PA01 | Module Switch-on | Global | New, Known (DL-166) |
| PA02 | Setup Run | Global | New, Known (DL-167) |
| PA03 | Setup Task | Global | New, Known (DL-167) |
| PA04 | Authority Register Entry | Global | Confirmed structure (DL-45); entity Assumed (DL-168) |
| PA05 | Identity Provider Connection | Global | New, Assumed (DL-169); GIIS Google Workspace Known |
| PA06 | Setting Definition | Global | Known need (OR09 "definitions belong to each module"); catalogue Assumed (DL-177) |
| PA07 | Purpose | Global | Confirmed structure (DL-44); list Open (OQ-200) |
| PA08 | Retention Rule | Global | Confirmed structure (DL-44); periods Open (OQ-43) |
| PA09 | Privacy Notice | Global | New, Assumed (DL-176); controller Open (OQ-06) |
| PA10 | Access Review Cycle | Global (definition), cell (cases) | Known need (S5, Admin Platform success metric); Assumed (DL-174) |

Referenced, not redefined: OR01–OR09, AC06, AC07, AC08, E01, E02, E05, E07, HR15, ST01, ST04, ST05, CC01, CC03.

## Detailed attribute dictionary

### Person

**Purpose:** One record per human across Campus OS (DL-42), global and Tier 1 only (DL-39). Student (ST01), Guardian (ST04) and Employee (E01) profiles in cells point at it by `person_id`; the account (A01) belongs to it. Defined here because no dictionary defined it although nine fields point at `FK:Person` (cross-module review OQ-234).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| person_id | UUID | PK | Y | T1 | Confirmed (DL-42) | Never reused; survives brand, country and role changes. |
| display_name | VARCHAR(180) | — | Y | T1 | Confirmed (DL-39 directory index) | The name every profile shows by default; legal names stay in cell profiles (T2). |
| profile_pointers | JSON | — | Y | T1 | Confirmed (DL-39 "cell pointer") | One entry per profile: type (STUDENT, GUARDIAN, EMPLOYEE), data location, profile ID. No profile data. |
| role_chips | VARCHAR(40)[] | — | N | T1 | Confirmed (AP-03) | Derived: Student, Guardian, Staff. |
| person_status | ENUM | — | Y | T1 | Known (DL-165) | ACTIVE, MERGED (kept for history, `merged_into_person_id` set), CLOSED (every profile past retention, OQ-43). |
| merged_into_person_id | UUID | FK:Person | C | T1 | Confirmed rule (DL-42: review queue, never automatic) | Set by `identity.person.merge` only. |

### A01 — User Account (refined)

**Purpose:** The one Campus OS account of a Person (DL-42). Credentials stay with the identity provider.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| user_id | UUID | PK | Y | T1 | Known (A01) | Every `*_by_user_id` field points here. |
| person_id | UUID | FK:Person, UK | Y | T1 | Confirmed (DL-42) | One account per Person. |
| account_type | ENUM | — | Y | T1 | Assumed (DL-170) | PERSON, SERVICE. Service accounts belong to an integration (payroll feed, ERP journal), never to a human, and hold no membership in a campus module UI. |
| account_status | ENUM | — | Y | T1 | Known values (A01); rules Assumed (DL-170) | INVITED, ACTIVE, SUSPENDED, DISABLED, CLOSED. Not inferred from employment alone (A01 note). |
| status_reason | ENUM | — | C | T1 | Assumed (DL-170) | Required for SUSPENDED and DISABLED: EMPLOYMENT_SUSPENDED, EMPLOYMENT_ENDED, ADMIN_ACTION, SECURITY, MERGED. Never shows a case reason (DL-163). |
| invited_at, invite_expires_at | TIMESTAMPTZ | — | N | T1 | Assumed (DL-13, held) | Invite sent `account.invite_days_before_start` days before the employment start (brand-country setting, default 7). |
| first_sign_in_at, last_sign_in_at | TIMESTAMPTZ | — | N | T1 | Known (A01 `last_login_at`) | Drives the Dormant flag. |
| dormant | BOOLEAN | — | Y | T1 | Assumed (DL-170); days Open (OQ-204) | Derived daily: ACTIVE and no sign-in for `account.dormant_after_days`. A flag for review, not a status. |
| preferred_locale | VARCHAR(35) | — | N | T1 | Known (E01 `preferred_locale`); DL-15 held | Date and number format. UI copy English in v1 (DL-15). |
| home_data_region_id | UUID | FK:OR06 | N | T1 | Assumed (DL-179) | Where the directory index points first; a Person with profiles in two countries keeps one account. |

### A02 — Account Link (refined)

**Purpose:** A dated sign-in identity of the account at an identity provider. Replaces "Employee Account Link": the profile link is now Person → profiles (DL-42), and A02 holds the identity a person signs in with.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| account_link_id | UUID | PK | Y | T1 | Known (A02) | |
| user_id | UUID | FK:A01 | Y | T1 | Known (A02) | |
| idp_connection_id | UUID | FK:PA05 | Y | T1 | Assumed (DL-169) | Which provider and tenant (A01 `identity_provider_code`, `idp_tenant_id` move here). |
| idp_subject_id | VARCHAR(255) | UK with connection | Y | T1 | Known (A01) | Immutable subject; the join key. Never match by email (DL-42). |
| login_identifier | VARCHAR(254) | — | C | T1 | Known (A01) | The work email shown, e.g. `priya.nair@giis.example`. Display only. |
| is_primary | BOOLEAN | — | Y | T1 | Assumed (DL-169) | One primary link per account; used for notifications. Which brand's email wins for a multi-brand person: OQ-195. |
| linked_from / linked_until | TIMESTAMPTZ | — | Y / N | T1 | Known (A02) | A link ends when the provider account is removed; the Campus OS account stays. |
| link_status | ENUM | — | Y | T1 | Known (A02) | ACTIVE, ENDED. |

### A03 — Module Grant (refined)

**Purpose:** The ceiling: a person may hold memberships in a module at a scope node and every node below it (DL-17, DL-40).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| module_grant_id | UUID | PK | Y | T1 | Known (A03) | |
| user_id | UUID | FK:A01 | Y | T1 | Known | |
| module_id | UUID | FK:R01 | Y | T1 | Known | |
| scope_node_id | UUID | FK:OR01 | Y | T1 | Confirmed (DL-40, DL-48) | GROUP, BRAND, BRAND_COUNTRY or CAMPUS; never SEGMENT (OR-R01). |
| grant_route | ENUM | — | Y | T1 | Assumed (DL-171) | POSITION_TEMPLATE (DL-149), REQUEST (ACCESS_REQUEST case), ADMIN, SYSTEM (GLOBAL_ADMIN implicit grant). Powers "Why can I see this?" (DL-38). |
| source_case_id | UUID | FK:CC03 | C | T1 | Confirmed pattern (DL-08) | Required for REQUEST. |
| grant_status | ENUM | — | Y | T1 | Known (A03) | ACTIVE, REVOKED, EXPIRED. |
| valid_from / valid_to | TIMESTAMPTZ | — | Y / N | T1 | Known | Cross-country grants at most 90 days (rbac-model Layer 1). |
| granted_by_user_id, second_approver_user_id | UUID | FK:A01 | N / C | T1 | Known / DL-09 (held) | Second Global Admin required for Group or Brand grants of `global_admin`. |

### A04 — Scope Grant (refined)

**Purpose:** Puts a person on a scope node: the node appears in their scope switcher (AP-01), they appear in that node's directory, and self-service works there. Gives no module work by itself (DL-171).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| scope_grant_id | UUID | PK | Y | T1 | Known (A04) | |
| user_id | UUID | FK:A01 | Y | T1 | Known | |
| scope_node_id | UUID | FK:OR01 | Y | T1 | Confirmed (DL-40) | Replaces A04 `campus_id`. |
| grant_route, source_case_id | ENUM, UUID | — | Y, C | T1 | Assumed (DL-171) | As A03. A campus assignment (E07) creates a POSITION_TEMPLATE scope grant (DL-149). |
| grant_status, valid_from / valid_to, granted_by_user_id | — | — | — | T1 | Known (A04) | As A03. |

### A05 — Module Membership (refined)

**Purpose:** Operational membership of a module team at a scope node (DL-35). The thing a Module Admin manages (SCR-TM-01).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| membership_id | UUID | PK | Y | T1 | Known (A05 `tm_membership_id`) | |
| user_id | UUID | FK:A01 | Y | T1 | Known | |
| module_id | UUID | FK:R01 | Y | T1 | Known | Module must be LIVE or PILOT at the node (PA01, DL-166). |
| scope_node_id | UUID | FK:OR01 | Y | T1 | Confirmed (DL-35) | Campus by default; must sit at or below an ACTIVE A03 for the same module. |
| membership_route | ENUM | — | Y | T1 | Assumed (DL-171) | POSITION_TEMPLATE, REQUEST, ADMIN, ALLOCATION ("add from allocation", rbac-model Layer 2). |
| membership_status | ENUM | — | Y | T1 | Known (A05) | INVITED, ACTIVE, DEACTIVATED, REMOVED. Deactivated with reason CEILING_REMOVED when the grant ends (rbac-model). |
| valid_from / valid_to | TIMESTAMPTZ | — | Y / N | T1 | Known | |
| assigned_by_user_id | UUID | FK:A01 | N | T1 | Known | |

### A06 — Access Role

**Purpose:** An access role code used in memberships and system grants. Not a job title (E05) and not a school role (role-catalogue).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| role_id | UUID | PK | Y | T1 | Known (A06) | |
| role_code | VARCHAR(100) | UK | Y | T1 | Known values (role-catalogue) + Assumed new (DL-176, DL-178) | GLOBAL_ADMIN, BRAND_ADMIN (new), PLATFORM_AUDITOR, DATA_PROTECTION_LEAD (new), PLATFORM_SUPPORT (new, operator sessions only), HR_ADMIN, HR_VIEWER, MODULE_ADMIN, CAMPUS_LEADER, TEACHER, MEMBER, VIEWER. |
| role_name | VARCHAR(160) | — | Y | T1 | Known | Screen label, e.g. "Brand admin". |
| role_category | ENUM | — | Y | T1 | Known (A06) | SYSTEM, FUNCTIONAL. |
| module_id | UUID | FK:R01 | N | T1 | Known | Null for roles used across modules. |
| lowest_node_type / highest_node_type | ENUM | — | Y | T1 | Assumed (DL-178) | Where the role may be assigned, e.g. BRAND_ADMIN: BRAND only; CAMPUS_LEADER: BRAND_COUNTRY to CAMPUS. |
| needs_second_approver | BOOLEAN | — | Y | T1 | DL-09 (held) | True for GLOBAL_ADMIN and BRAND_ADMIN. |
| is_assignable, role_status | BOOLEAN, ENUM | — | Y | T1 | Known (A06) | ACTIVE, RETIRED. |

### A07 — Role Assignment

**Purpose:** A dated access role on a membership (Known, A07). Fields unchanged: `role_assignment_id`, `membership_id` (FK:A05), `role_id` (FK:A06), `assignment_status` (ACTIVE, ENDED, REVOKED), `valid_from` / `valid_to`, `assigned_by_user_id`. A membership may hold several roles.

### A08 — Permission Template

**Purpose:** A versioned bundle of capabilities (Known, A08).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| permission_template_id | UUID | PK | Y | T1 | Known | One row per version. |
| template_code | VARCHAR(100) | — | Y | T1 | Known | Family across versions, e.g. `attendance.teacher`. |
| template_name | VARCHAR(160) | — | Y | T1 | Known | |
| template_category | ENUM | — | Y | T1 | Confirmed (DL-20) | BASE (shared across modules), MODULE. |
| template_type | ENUM | — | Y | T1 | Known (A08) | DEFAULT, STANDARD, ADD_ON. |
| module_id | UUID | FK:R01 | C | T1 | Known | Required for MODULE. |
| version | INTEGER | — | Y | T1 | Known | |
| template_status | ENUM | — | Y | T1 | Known (A08); immutability Assumed (DL-172) | DRAFT, PUBLISHED, RETIRED (A08 "ACTIVE" renamed PUBLISHED so the screen says what happened). A PUBLISHED version never changes. |
| published_by_user_id, published_at | UUID, TIMESTAMPTZ | — | C | T1 | Seed (`ga.template.publish`) | |
| includes_tier3 | BOOLEAN | — | Y | T1 | Assumed (DL-173) | Derived from A09. A position template (DL-149) must be false. |

### A09 — Template Capability

**Purpose:** One allow or deny inside a template version (Known, A09): `template_capability_id`, `permission_template_id` (FK:A08), `capability_id` (FK:R02), `permission_effect` ALLOW / DENY. Deny wins (DL-07, Confirmed).

### A10 — Role Template Binding

**Purpose:** Pins a role assignment to template versions (Known, A10): `role_template_binding_id`, `role_assignment_id` (FK:A07), `permission_template_id` (FK:A08), `valid_from` / `valid_to`, `binding_status` ACTIVE / REVOKED. A binding resolves BASE + MODULE (DL-20). Upgrading to a new version is a new binding, usually at AY rollover (SCR-TM-05).

### A11 — Member Permission Override

**Purpose:** A per-membership allow or deny (Known, A11). The feature stays **off** (DL-14, held Assumed); the entity is kept so the model doesn't change if it is switched on. No screen shows it in v1.

### A12 — Data Scope Policy and A13 — Data Scope Condition

**Purpose:** Which records a membership covers (Known, A12, A13). A12: `scope_policy_id`, `membership_id`, `scope_policy_status`, `valid_from` / `valid_to`. A13 adds `source_type` STATIC, ALLOCATION, ORG_UNIT (DL-16; RELATIONSHIP removed by DL-37), `source_ref`, `scope_group_key` (AND inside a group, OR across groups, ia-screen-map SCR-TM-04) and `scope_dimension`: CAMPUS, ACADEMIC_YEAR, PROGRAMME (was CURRICULUM; DL-117 screen term), GRADE, CLASS (was SECTION; DL-29), SEGMENT, SUBJECT, TEACHING_GROUP (DL-133), DEPARTMENT (was ORG_UNIT; DL-150). No scope means no data (BR-17).

### A14 — Audit Event

**Purpose:** Immutable record of every change and every Tier 3 read (Known, A14; Arch § Security). Fields as A14, plus:

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| scope_node_id | UUID | FK:OR01 | Y | T1 | Assumed | Lets a Campus Leader filter their campus (SCR-GOV-04). |
| purpose_code | VARCHAR(40) | FK:PA07 | C | T1 | Confirmed rule (DL-44, AR-10) | Required on Tier 3 reveal, cross-cell access and operator sessions. |
| acting_for_user_id | UUID | FK:A01 | C | T1 | Confirmed (DL-36) | When a delegate (A16) acted. |
| operator_session_id | UUID | FK:A18 | C | T1 | Assumed (DL-175) | Every event inside a support session. |
| before_ref_or_redacted, after_ref_or_redacted | JSON | — | N | T3 | Known (A14 Restricted) | Viewed with `gov.audit.view_snapshot`. |

Retention of audit events per country: OQ-199.

### A15 — Record ACL Entry

**Purpose:** Named access to one restricted record (Confirmed, DL-19): `acl_id`, `record_type`, `record_id`, `grantee_user_id`, `access_level` READ / WRITE, `reason`, `valid_from` / `valid_to`, `granted_by`. Created by case assignment and participants (CC05) or by an approved request; nobody grants it to themselves (rbac-model Layer 5).

### A16 — Delegation

**Purpose:** Someone acts for someone else for a while (Confirmed entity, DL-36).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| delegation_id | UUID | PK | Y | T1 | Confirmed (DL-36) | |
| delegator_user_id, delegate_user_id | UUID | FK:A01 | Y | T1 | Confirmed | Different people; the delegate must hold a scope grant on the node. |
| capability_set | VARCHAR(160)[] | FK:R02 | Y | T1 | Confirmed | Approvals only by default (leave, cases, placements); never Tier 3 reveal unless the delegate holds A17 themselves. |
| scope_node_id | UUID | FK:OR01 | Y | T1 | Confirmed | |
| reason | VARCHAR(300) | — | Y | T1 | Confirmed | Shown in the "Acting for" banner. |
| valid_from / valid_to | TIMESTAMPTZ | — | Y | T1 | Confirmed; max 90 days (DL-21, held) | Ends early if either person's account is suspended. |
| delegation_status | ENUM | — | Y | T1 | Assumed | SCHEDULED, ACTIVE, ENDED, REVOKED. |

### A17 — Tier 3 Type Grant

**Purpose:** Standing access to a class of restricted records at a node, e.g. staff work passes at GIIS Singapore (Confirmed, DL-36).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| type_grant_id | UUID | PK | Y | T1 | Confirmed (DL-36) | |
| grantee_user_id | UUID | FK:A01 | Y | T1 | Confirmed | Must also hold the module capability (e.g. `hr.restricted.view`). |
| record_type | VARCHAR(60) | — | Y | T1 | Confirmed | e.g. STAFF_IDENTITY, STUDENT_IDENTITY, MEDICAL_CERTIFICATE, RESTRICTED_CASE:ICA_REVIEW. |
| scope_node_id | UUID | FK:OR01 | Y | T1 | Confirmed | |
| reason | VARCHAR(300) | — | Y | T1 | Confirmed | |
| approver_user_id | UUID | FK:A01 | Y | T1 | Confirmed (DL-69): the Campus Leader of the node | When the Campus Leader is the grantee, the brand-country head approves (OQ-205). |
| request_case_id | UUID | FK:CC03 | Y | T1 | Confirmed pattern (DL-08) | ACCESS_REQUEST case. |
| valid_from / valid_to | TIMESTAMPTZ | — | Y | T1 | Assumed (DL-69): end of academic year | Renewal is a new request. |
| type_grant_status | ENUM | — | Y | T1 | Assumed | ACTIVE, EXPIRED, REVOKED. |

### A18 — Operator Session (new)

**Purpose:** Time-boxed, approved access by GSG support into one campus's data (DL-52, DL-175). No standing operator access exists.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| operator_session_id | UUID | PK | Y | T1 | Assumed (DL-175) | |
| operator_user_id | UUID | FK:A01 | Y | T1 | Assumed | Holds PLATFORM_SUPPORT at Group; no campus memberships. |
| scope_node_id | UUID | FK:OR01 | Y | T1 | Assumed | One campus. |
| module_ids | UUID[] | FK:R01 | Y | T1 | Assumed | Only the modules named in the request. |
| session_kind | ENUM | — | Y | T1 | Assumed (DL-52) | SUPPORT, BREAK_GLASS. |
| purpose_code | VARCHAR(40) | FK:PA07 | Y | T1 | Confirmed rule (AR-10) | SUPPORT_INVESTIGATION or SECURITY_INCIDENT. |
| ticket_ref | VARCHAR(120) | — | Y | T1 | Assumed | The school's support ticket or incident number. |
| access_level | ENUM | — | Y | T1 | Assumed (DL-52 shape) | READ_ONLY. WRITE is not offered in v1. |
| requested_minutes | INTEGER | — | Y | T1 | Assumed (DL-52 shape) | At most `operator_session.max_minutes` (Group setting, 240). |
| approver_user_id, approved_at | UUID, TIMESTAMPTZ | FK:A01 | C | T1 | Confirmed (DL-52): Campus Leader | Not required for BREAK_GLASS; then `notified_at` within 24 hours. |
| started_at, ended_at | TIMESTAMPTZ | — | N | T1 | Assumed | Ends at the cap, on the operator's "End session", or when the Campus Leader ends it. |
| session_status | ENUM | — | Y | T1 | Assumed | REQUESTED, APPROVED, ACTIVE, ENDED, DECLINED, EXPIRED. |
| tier3_allowed | BOOLEAN | — | Y | T1 | Assumed (DL-175) | False for SUPPORT; Tier 3 stays masked. BREAK_GLASS: OQ-207. |

### R01 — Module

Known (R01): `module_id`, `module_code` (DL-04 list: 12 campus modules + `global_admin`, `governance`), `module_name`, `module_status` AVAILABLE / RETIRED. Added: `module_kind` CAMPUS / PLATFORM (DL-04), `has_team_tab` (ia-screen-map), `owner_domain` (DL-43).

### R02 — Capability

Known (R02): `capability_id`, `module_id`, `capability_code` (`module.object.action`, DL-53), `action_type` VIEW, CREATE, UPDATE, DELETE, APPROVE, EXPORT, MANAGE, `capability_status` AVAILABLE / RETIRED. Added: `tier` (T1, T2 or T3 of the data it opens, DL-34; T3 shows a lock chip on SCR-GA-07) and `catalogue_status` SEED / NEW / REVIEWED (the permission catalogue column; REVIEWED once the registry owner signs it off, DL-51, DL-53).

### R03 — Parent Capability Set

Fixed capability set for the parent path (Confirmed, DL-18, DL-28, DL-37). Not editable in the Admin Platform in v1; shown read-only on SCR-GA-08.

### F01 — Family Access Link

Parent path link (Confirmed, DL-18, DL-37): `family_access_id`, `guardian_person_id`, `student_id`, `enrolment_id`, `link_status`. States stay pending (OBJ-09) until the Parent app is designed (DL-118); the Admin Platform shows a count per campus and the link on the student profile only.

### PA01 — Module Switch-on (new)

**Purpose:** Which modules a scope node uses, from when (DL-166). Grants and memberships for a module are only possible where it is LIVE or PILOT.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| switch_on_id | UUID | PK | Y | T1 | Known (DL-166) | |
| module_id | UUID | FK:R01 | Y | T1 | Assumed | Campus modules only; platform modules are always on. |
| scope_node_id | UUID | FK:OR01 | Y | T1 | Assumed | Set at Brand-country or Campus; inherits downward like a setting (AR-03). |
| switch_status | ENUM | — | Y | T1 | Assumed | PLANNED, PILOT, LIVE, PAUSED, OFF. |
| live_from | DATE | — | C | T1 | Assumed; real dates Open (OQ-05, OQ-197) | Required for PILOT and LIVE. |
| pilot_user_ids | UUID[] | FK:A01 | C | T1 | Assumed | PILOT: only these people can be members. |
| authority_complete | BOOLEAN | — | Y | T1 | Assumed (DL-168) | Derived: every field group of the module has a PA04 row at this node. LIVE needs true. |
| set_by_user_id | UUID | FK:A01 | Y | T1 | Assumed | Brand Admin or Global Admin. |

### PA02 — Setup Run (new)

**Purpose:** The checklist that takes a brand-country or campus from PLANNED to ACTIVE (DL-167).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| setup_run_id | UUID | PK | Y | T1 | Known (DL-167) | |
| scope_node_id | UUID | FK:OR01 | Y | T1 | Assumed | A BRAND_COUNTRY or CAMPUS node with status PLANNED. |
| run_kind | ENUM | — | Y | T1 | Assumed | BRAND_COUNTRY_SETUP, CAMPUS_OPENING, CAMPUS_MIGRATION (an existing school moving onto Campus OS from legacy systems). |
| target_go_live | DATE | — | Y | T1 | Assumed | |
| owner_user_id | UUID | FK:A01 | Y | T1 | Assumed | Usually the Brand Admin. |
| approver_user_id | UUID | FK:A01 | C | T1 | Known (DL-167): brand-country head; Open (OQ-196) | Signs off go-live. |
| run_status | ENUM | — | Y | T1 | Assumed | DRAFT, IN_PROGRESS, READY, LIVE, CANCELLED. READY when every blocking task is DONE. |
| go_live_case_id | UUID | FK:CC03 | C | T1 | Assumed | CAMPUS_GO_LIVE case for the sign-off. |

### PA03 — Setup Task (new)

**Purpose:** One step in a setup run. Most tasks are checked by the system from the owning record; a few are manual.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| setup_task_id | UUID | PK | Y | T1 | Assumed | |
| setup_run_id | UUID | FK:PA02 | Y | T1 | Assumed | |
| task_code | VARCHAR(40) | — | Y | T1 | Known (DL-167) | See the setup task list below. |
| blocking | BOOLEAN | — | Y | T1 | Assumed | Blocking tasks gate READY. |
| check_mode | ENUM | — | Y | T1 | Assumed | AUTO (read from the owning record), MANUAL (owner ticks with a note). |
| source_ref | VARCHAR(120) | — | N | T1 | Assumed | The record that satisfies an AUTO task, e.g. an AC07 year ID. |
| owner_role | VARCHAR(60) | — | Y | T1 | Assumed | Who does it: Brand Admin, Global Admin, brand HR, campus academic coordinator, Finance. |
| task_status | ENUM | — | Y | T1 | Assumed | OPEN, DONE, NOT_NEEDED, BLOCKED. |
| due_date | DATE | — | N | T1 | Assumed | |

**Setup task list (Assumed, DL-167).** `B` blocking.

| task_code | Done when | Owner | B |
|---|---|---|:-:|
| NODE_CREATED | OR01 node exists with parent and code (codes placeholder, OQ-55) | Global Admin | B |
| OPERATOR_LINKED | OR08 OPERATOR link valid on go-live date (entity name pending OQ-02) | Global Admin | B |
| DATA_LOCATION_LIVE | The brand-country's OR06 is LIVE (OQ-21 outside SG and IN) | Platform team | B |
| SIGN_IN_CONNECTED | A PA05 connection for the brand is ACTIVE and a test sign-in passed | Brand Admin | B |
| PROGRAMMES_SET | At least one AC06 programme OPEN or opening at the campus | Academic coordinator | B |
| YEAR_PUBLISHED | Each programme has an AC07 year PUBLISHED (DL-113) | Principal | B |
| LEADER_ASSIGNED | A CAMPUS_LEADER membership is ACTIVE from go-live | Brand Admin | B |
| MODULES_PLANNED | Each module to use has a PA01 row with `live_from` | Brand Admin | B |
| AUTHORITY_FILLED | PA04 rows exist for every field group of every switched-on module | Brand Admin with brand IT | B |
| MODULE_ADMINS | A MODULE_ADMIN membership for each switched-on module | Brand Admin | B |
| POSITION_TEMPLATES | The brand's position → template list is published (DL-173) | Platform team | B |
| STAFF_IMPORTED | Staff records imported and accounts INVITED (DL-147, DL-170) | Brand HR | B |
| NOTICE_PUBLISHED | Privacy notices for staff (and later parents) PUBLISHED for the brand-country (PA09) | Data Protection Lead | B |
| SETTINGS_REVIEWED | Brand-country settings without a value listed and accepted (PA06) | Brand Admin | — |
| FEE_PLANS | Fee plans approved (FB02) if `fees_finance` is switched on | Finance Controller | — |
| CLASSES_PLANNED | Class plan approved (CL02) if Classes is used | Principal | — |
| TRAINING_DONE | Campus admins trained (manual) | Brand Admin | — |

### PA04 — Authority Register Entry

**Purpose:** Which system is the source of truth for a field group at a node, until when (DL-45, DL-168). Drives source chips (AP-04) and conflict quarantine (AP-12).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| authority_id | UUID | PK | Y | T1 | Confirmed structure (DL-45) | |
| scope_node_id | UUID | FK:OR01 | Y | T1 | Assumed (DL-168): Brand-country, campus exceptions allowed | |
| module_id | UUID | FK:R01 | Y | T1 | Assumed | |
| field_group | VARCHAR(80) | — | Y | T1 | Confirmed structure (DL-45) | From the object card, e.g. `student.identity`, `staff.record`. `*` for the whole module (Staff & HR, DL-147). |
| authoritative_system | VARCHAR(60) | — | Y | T1 | Known for GIIS: MyGIIS, Google Workspace; others Open (OQ-27, OQ-28) | `CAMPUS_OS` after cutover. |
| sync_direction | ENUM | — | Y | T1 | Confirmed structure (DL-45) | INBOUND (legacy → Campus OS, read-only here), OUTBOUND (Campus OS → payroll, ERP), NONE. |
| cutover_date | DATE | — | C | T1 | Open per brand (OQ-27, OQ-28, OQ-166) | Until this date the field group is read-only with a source chip. |
| conflict_reviewer_role | VARCHAR(60) | — | Y | T1 | Confirmed (DL-45 "named reviewer") | Who clears the quarantine. |
| entry_status | ENUM | — | Y | T1 | Assumed | PLANNED, ACTIVE, CUT_OVER. |

### PA05 — Identity Provider Connection (new)

**Purpose:** How a brand's staff sign in (DL-169).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| idp_connection_id | UUID | PK | Y | T1 | Assumed | |
| scope_node_id | UUID | FK:OR01 | Y | T1 | Assumed: Brand, or Brand-country when a brand runs several tenants (OQ-194) | |
| provider | ENUM | — | Y | T1 | Known for GIIS (Google Workspace, Admin Platform brief); others Open (OQ-194) | GOOGLE_WORKSPACE, MICROSOFT_ENTRA, OTHER_OIDC. |
| tenant_ref | VARCHAR(160) | — | Y | T1 | Assumed | Provider tenant or domain ID. |
| email_domains | VARCHAR(120)[] | — | Y | T1 | Assumed | Synthetic in samples, e.g. `giis.example`. |
| audience | ENUM | — | Y | T1 | Assumed (DL-118) | STAFF. PARENT and STUDENT later. |
| mfa_required | BOOLEAN | — | Y | T1 | Known (brief: "staff SSO, MFA") | True for STAFF; can't be switched off. |
| provisioning | ENUM | — | Y | T1 | Assumed (DL-170) | CAMPUS_OS_INVITES (Campus OS creates accounts from employments; the provider only authenticates). Directory sync from the provider: OQ-194. |
| connection_status | ENUM | — | Y | T1 | Assumed | DRAFT, TESTING, ACTIVE, DISABLED. |

### PA06 — Setting Definition

**Purpose:** Every setting key, defined once (DL-177). OR09 holds the values; this says what a key means, who owns it and where it may be set.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| setting_key | VARCHAR(120) | PK | Y | T1 | Known examples (Arch § Tenancy; module dictionaries) | e.g. `account.invite_days_before_start`, `attendance.register_lock_time`. |
| owner_module_id | UUID | FK:R01 | Y | T1 | Known (OR09 "definitions belong to each module") | |
| label, help_text | VARCHAR | — | Y | T1 | Assumed | Shown on the settings screen. |
| value_type | ENUM | — | Y | T1 | Assumed | BOOLEAN, INTEGER, DURATION, TIME, DATE, ENUM, TEXT, JSON. |
| highest_level / lowest_level | ENUM | — | Y | T1 | Assumed (DL-177); which keys: Open (OQ-20) | e.g. GROUP to CAMPUS. A value can't be set outside this range. |
| default_value | JSON | — | N | T1 | Assumed | Empty means "Not set": the module shows "Not set" and never invents a group default (DL-59). |
| default_override_policy | ENUM | — | Y | T1 | Confirmed values (DL-50) | OPEN, LOCKED_BELOW. |
| source_ref | VARCHAR(120) | — | N | T1 | Known where cited | The DL or handbook page behind the key. |
| definition_status | ENUM | — | Y | T1 | Assumed | ACTIVE, RETIRED. |

### PA07 — Purpose

**Purpose:** Why personal data is used, asked on Tier 3 reveal, cross-cell access and operator sessions (DL-44, AR-10).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| purpose_code | VARCHAR(40) | PK | Y | T1 | Confirmed structure (DL-44); list Open (OQ-200) | Sample list: EDUCATION_DELIVERY, SAFEGUARDING, ADMISSIONS_DECISION, FEES_COLLECTION, REGULATORY_REPORT, STAFF_ADMINISTRATION, SUPPORT_INVESTIGATION, SECURITY_INCIDENT, ACCESS_REVIEW. |
| label, description | VARCHAR | — | Y | T1 | Confirmed structure | Shown in the purpose picker (AP-10). |
| consent_needed | ENUM | — | Y | T1 | Confirmed field (DL-44); values Open (OQ-10) | YES, NO, PENDING. PENDING shows the pending marker. |
| lawful_basis | VARCHAR(80) | — | N | T1 | Open (OQ-09) | Empty until counsel answers. |
| asked_on | ENUM[] | — | Y | T1 | Confirmed (AR-10) | TIER3_REVEAL, CROSS_CELL, OPERATOR_SESSION. |
| purpose_status | ENUM | — | Y | T1 | Assumed | DRAFT, ACTIVE, RETIRED. |

### PA08 — Retention Rule

**Purpose:** How long a record type is kept per country, then anonymised or deleted (DL-44).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| retention_rule_id | UUID | PK | Y | T1 | Confirmed structure (DL-44) | |
| country_code | CHAR(2) | — | Y | T1 | Confirmed (DL-44 "per country pack") | |
| record_type | VARCHAR(60) | — | Y | T1 | Assumed | e.g. STUDENT_RECORD, GUARDIAN_PROFILE, STAFF_RECORD, CANDIDATE, AUDIT_EVENT, CASE, MESSAGE. |
| trigger_event | ENUM | — | Y | T1 | Assumed | ENROLMENT_ENDED, EMPLOYMENT_ENDED, APPLICATION_CLOSED, CREATED, CASE_CLOSED. |
| keep_for_months | INTEGER | — | C | T1 | Open (OQ-43, OQ-185, OQ-199) | Empty shows "Pending · OQ-43"; nothing is deleted while empty. |
| then_action | ENUM | — | C | T1 | Confirmed values (DL-44) | ANONYMISE, DELETE. |
| legal_hold_allowed | BOOLEAN | — | Y | T1 | Assumed | A Case or regulator request can pause it. |
| rule_status | ENUM | — | Y | T1 | Assumed | DRAFT, ACTIVE, RETIRED. Activating needs the Data Protection Lead of that country (DL-176). |

### PA09 — Privacy Notice (new)

**Purpose:** The notice staff (and later families) see, per brand-country and audience, versioned (DL-176).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| notice_id | UUID | PK | Y | T1 | Assumed | One row per version. |
| brand_country_node_id | UUID | FK:OR03 | Y | T1 | Assumed; per operating entity instead: Open (OQ-202) | |
| audience | ENUM | — | Y | T1 | Assumed | STAFF, GUARDIAN, STUDENT, CANDIDATE. |
| version, published_at | INTEGER, TIMESTAMPTZ | — | Y / C | T1 | Assumed | |
| controller_text | VARCHAR(240) | — | Y | T1 | Open (OQ-06) | Always "[data controller — OQ-06]" until answered (AP-09). |
| body_ref | UUID | — | Y | T1 | Assumed | Document in the content store. Languages: OQ-25. |
| purpose_codes | VARCHAR(40)[] | FK:PA07 | Y | T1 | Assumed | |
| ack_required | BOOLEAN | — | Y | T1 | Assumed | Staff acknowledge on first sign-in after a new version. An acknowledgement is not consent (OBJ-17). |
| notice_status | ENUM | — | Y | T1 | Assumed | DRAFT, PUBLISHED, SUPERSEDED. |

### PA10 — Access Review Cycle

**Purpose:** A review round that opens one CAMPUS_ATTESTATION case per campus (S5, DL-174).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| review_cycle_id | UUID | PK | Y | T1 | Assumed (DL-174) | |
| scope_node_id | UUID | FK:OR01 | Y | T1 | Assumed | Usually a Brand-country; one case per campus below it. |
| cycle_kind | ENUM | — | Y | T1 | Assumed | TERMLY, AD_HOC (e.g. after an incident). |
| term_ref | UUID | FK:AC08 | C | T1 | Assumed (S5 "week 1 of Semester 2") | Starts in week 1 of the term of the campus's main programme. |
| due_working_days | INTEGER | — | Y | T1 | Known (S5: 10 working days) | Deadline on each case (CC04). |
| flag_rules | ENUM[] | — | Y | T1 | Known (S5) | TIER3_HOLDER, CROSS_CAMPUS, OVERRIDE, DORMANT, LEAVING_30_DAYS, NO_EMPLOYEE_LINK, OPERATOR_SESSION_USED. |
| cycle_status | ENUM | — | Y | T1 | Assumed | SCHEDULED, OPEN, CLOSED. |

Each campus case lists one line per person with any A03, A04, A05, A16 or A17 at the campus, decided KEEP, REDUCE or REMOVE; REDUCE and REMOVE lines become membership changes routed to the Module Admin or Brand Admin (S5).

## Cardinality and referential integrity

| Parent | Child | Cardinality | FK | Meaning | Status |
|---|---|---|---|---|---|
| Person | A01 | 1 → 0..1 | person_id | One account per Person | Confirmed (DL-42) |
| A01 | A02 | 1 → 1..N | user_id | Several sign-in identities, one primary | Assumed (DL-169) |
| PA05 | A02 | 1 → 0..N | idp_connection_id | | Assumed |
| A01 | A03, A04 | 1 → 0..N | user_id | Ceilings on scope nodes | Known, DL-40 |
| A03 + A04 | A05 | covers | module_id, scope_node_id | A membership needs both at or above its node | Known (A05), DL-35 |
| PA01 | A05 | gates | module_id, scope_node_id | Only LIVE or PILOT modules take members | Known (DL-166) |
| A05 | A07 → A10 | 1 → 0..N | membership_id | Roles and pinned templates | Known |
| A08 | A09 | 1 → 0..N | permission_template_id | | Known |
| A05 | A12 → A13 | 1 → 0..N | membership_id | Data scope | Known, DL-16 |
| A01 | A16, A17, A18 | 1 → 0..N | user ids | Special access | Confirmed (DL-36), Assumed (DL-175) |
| OR01 | PA02 | 1 → 0..N | scope_node_id | One open run per node at a time | Known (DL-167) |
| PA02 | PA03 | 1 → 1..N | setup_run_id | | Assumed |
| OR01 × R01 | PA04 | 1 → 1..N | scope_node_id, module_id | One row per field group | Assumed (DL-168) |
| R01 | PA06 | 1 → 0..N | owner_module_id | | Assumed (DL-177) |
| PA06 | OR09 | 1 → 0..N | setting_key | Values per node | Known (OR09) |
| PA10 | CC03 | 1 → 1..N | review_cycle_id | One CAMPUS_ATTESTATION case per campus | Assumed (DL-174) |

## Field visibility by role

`M` manage, `A` approve, `V` view, `C` counts only, `O` own, `—` none. All cells Assumed (DL-171, DL-176, DL-178) except where cited.

| Record | Global Admin (GSG) | Brand Admin | Campus Leader | Module Admin | Data Protection Lead | Platform Auditor | Platform Support | Staff (self) |
|---|---|---|---|---|---|---|---|---|
| Module switch-on, setup runs | M | M (own brand) | V (own campus), A go-live if brand-country head | V | V | V | — | — |
| Authority register | M | M (own brand) | V | V (own module) | V | V | — | — |
| Setting definitions | M | V | V | V | V | V | — | — |
| Identity providers | M | M (own brand) | — | — | — | V | — | — |
| Accounts: status, last sign-in, dormant (T1) | M | M (own brand) | V (own campus) | V (own members) | — | V | — | O |
| Roles, templates, capabilities | M (DL-51) | V | V | V | V | V | — | — |
| Grants (A03, A04) | M | M (own brand, not Group) | V (own campus) | V (own module) | — | V | — | O |
| Memberships, roles, scope (A05–A13) | V; M at Group | M (own brand) | V | M (own module, own campus, DL-09 held) | — | V | — | O |
| Delegation (A16) | V | V | M (own) | M (own) | — | V | — | O |
| Tier 3 type grants (A17) | V | V | A (DL-69) | request | V (counts and holders) | V | — | O |
| Operator sessions (A18) | V | V | A (DL-52) | — | V | V | request, O | — |
| Access review case | V | V, escalation | M (attest) | lines routed to them | V (report) | V | — | — |
| Purposes, retention rules, notices | M | V | V | — | M (own country, DL-176) | V | — | V notices |
| Audit events (redacted) | V | V (own brand) | V (own campus) | V (own module) | V (own country) | V | own session only | O (own access history) |
| Audit before/after snapshots (T3) | with `gov.audit.view_snapshot` | — | — | — | with capability | with capability | — | — |
| Other countries' memberships | C unless access there (AR-01) | C | — | — | C | C | — | — |

## Enumerations (candidates, DL-27 rule)

| Entity | Field | Values |
|---|---|---|
| A01 | account_type | PERSON, SERVICE |
| A01 | account_status | INVITED, ACTIVE, SUSPENDED, DISABLED, CLOSED |
| A01 | status_reason | EMPLOYMENT_SUSPENDED, EMPLOYMENT_ENDED, ADMIN_ACTION, SECURITY, MERGED |
| A03, A04, A05 | grant_route / membership_route | POSITION_TEMPLATE, REQUEST, ADMIN, SYSTEM, ALLOCATION (A05 only) |
| A06 | role_code (new values) | BRAND_ADMIN, DATA_PROTECTION_LEAD, PLATFORM_SUPPORT |
| A08 | template_status | DRAFT, PUBLISHED, RETIRED |
| A13 | scope_dimension | CAMPUS, ACADEMIC_YEAR, PROGRAMME, GRADE, CLASS, SEGMENT, SUBJECT, TEACHING_GROUP, DEPARTMENT |
| A16 | delegation_status | SCHEDULED, ACTIVE, ENDED, REVOKED |
| A17 | type_grant_status | ACTIVE, EXPIRED, REVOKED |
| A18 | session_kind | SUPPORT, BREAK_GLASS |
| A18 | session_status | REQUESTED, APPROVED, ACTIVE, ENDED, DECLINED, EXPIRED |
| PA01 | switch_status | PLANNED, PILOT, LIVE, PAUSED, OFF |
| PA02 | run_kind | BRAND_COUNTRY_SETUP, CAMPUS_OPENING, CAMPUS_MIGRATION |
| PA02 | run_status | DRAFT, IN_PROGRESS, READY, LIVE, CANCELLED |
| PA03 | task_status | OPEN, DONE, NOT_NEEDED, BLOCKED |
| PA04 | sync_direction | INBOUND, OUTBOUND, NONE |
| PA04 | entry_status | PLANNED, ACTIVE, CUT_OVER |
| PA05 | provider | GOOGLE_WORKSPACE, MICROSOFT_ENTRA, OTHER_OIDC |
| PA05 | connection_status | DRAFT, TESTING, ACTIVE, DISABLED |
| PA07 | consent_needed | YES, NO, PENDING |
| PA08 | then_action | ANONYMISE, DELETE |
| PA09 | notice_status | DRAFT, PUBLISHED, SUPERSEDED |
| PA10 | cycle_status | SCHEDULED, OPEN, CLOSED |
| CC01 | case type (new) | CAMPUS_GO_LIVE, OPERATOR_SESSION (existing: ACCESS_REQUEST, ONBOARDING_ACCESS, AFFILIATION_REVIEW, EXIT_REVIEW, CAMPUS_ATTESTATION) |

### Status chips (proposed, AP-14)

| List | Value → screen label → chip |
|---|---|
| Account | INVITED → Invited → info · ACTIVE → Active → success · SUSPENDED → Suspended → warning · DISABLED → Disabled → error (rules-to-ui) · CLOSED → Closed → default · Dormant flag → Dormant {n} days → warning (outlined) |
| Grant, membership, type grant | ACTIVE → Active → success · scheduled → Starts {date} → info · EXPIRED → Expired → default · REVOKED → Removed → error |
| Module switch-on | PLANNED → Planned {date} → info · PILOT → Pilot → secondary · LIVE → Live → success · PAUSED → Paused → warning · OFF → Off → default |
| Setup run | IN_PROGRESS → {n} of {m} done → info · READY → Ready for sign-off → success · LIVE → Live → success · CANCELLED → Cancelled → default |
| Setup task | OPEN → (none) · DONE → Done → success · BLOCKED → Blocked → error · NOT_NEEDED → Not needed → default |
| Authority | PLANNED → Cutover {date} → info · ACTIVE (legacy) → From {system} → info soft (AP-04) · CUT_OVER → Campus OS → success |
| Identity provider | TESTING → Testing → warning · ACTIVE → Connected → success · DISABLED → Disabled → default |
| Operator session | REQUESTED → Waiting for {approver} → info · APPROVED → Approved → info · ACTIVE → Live · ends {time} → warning · ENDED → Ended → default · DECLINED → Declined → error · Break-glass → Break-glass → error |
| Template | DRAFT → Draft → default · PUBLISHED → v{n} published → success · RETIRED → Retired → default |
| Purpose, retention, notice | DRAFT → Draft → default · ACTIVE / PUBLISHED → Active → success · pending legal field → Pending · OQ-xx → pending (AP-13) |

## Business rules

- **AP-R01 (Assumed, DL-166):** A membership (A05) can be created only where its module is LIVE, or PILOT and the person is on the pilot list. Pausing a module keeps memberships but closes the module's screens with "Paused by {Brand Admin} on {date}".
- **AP-R02 (Assumed, DL-167):** A campus's `campus_status` changes PLANNED → ACTIVE only when its setup run is READY and the CAMPUS_GO_LIVE case is approved. A module can't be LIVE at a campus that isn't ACTIVE.
- **AP-R03 (Assumed, DL-168; Confirmed DL-45):** A module can't be LIVE at a node until every field group has an authority row. Before a field group's cutover date it is read-only in Campus OS with a source chip; inbound conflicts go to the named reviewer's quarantine (AP-12), never merged silently.
- **AP-R04 (Assumed, DL-169; Confirmed DL-42):** A sign-in identity is matched to an account by provider subject only. A new subject with an email matching an existing account goes to the Person review queue (DL-58), never auto-linked.
- **AP-R05 (Assumed, DL-170; DL-13 held):** EmploymentCreated → account INVITED (if the Person has none); invite sent `account.invite_days_before_start` days before start; first sign-in → ACTIVE; EmploymentSuspended → SUSPENDED (no reason shown, DL-163); last working day passed with no other active employment → DISABLED and all grants, memberships, delegations and type grants end that day (BR-10). A Person who is also a guardian keeps the account for the parent path.
- **AP-R06 (Assumed, DL-171; Confirmed DL-149, DL-73):** Automatic routes write grants and memberships with their route and source; a person or admin can remove an automatic grant, and the removal sticks until the source fact changes again.
- **AP-R07 (Assumed, DL-172):** A published template version never changes. Publishing a new version shows how many members are on older versions; moving them is a separate, previewed action (SCR-GA-10, SCR-TM-05).
- **AP-R08 (Assumed, DL-173):** A position template can't include a Tier 3 capability or a capability of a module that isn't LIVE at the campus.
- **AP-R09 (DL-09 held):** Granting GLOBAL_ADMIN or BRAND_ADMIN needs a second Global Admin's approval. Nobody approves their own grant, type grant or operator session.
- **AP-R10 (Assumed, DL-174):** A review cycle opens CAMPUS_ATTESTATION cases on its start date; each case lists flagged rows first; undecided rows at the deadline escalate to the brand-country head and never remove access by themselves. Submitted REMOVE lines take effect the next day.
- **AP-R11 (Assumed, DL-175; Confirmed DL-52 approver):** An operator session starts only after the Campus Leader approves (or as BREAK_GLASS by GSG security), is read-only, ends at the cap, masks Tier 3 for SUPPORT, logs every screen with the session ID, and appears on the campus's audit log and in the next access review.
- **AP-R12 (Assumed, DL-176; AR-04):** No screen, notice or export names a controller; `controller_text` shows the placeholder. A retention rule with an empty period deletes nothing.
- **AP-R13 (Assumed, DL-177):** A setting value (OR09) is accepted only at levels inside its definition's range and where no ancestor has LOCKED_BELOW.
- **AP-R14 (Confirmed, DL-38):** Every access answer names its route: grant, membership, position template, derived scope, delegation, ACL, type grant or operator session (AP-07).
- **AP-R15 (Assumed, DL-179; Confirmed AR-01):** Admin lists that cross data locations show counts for locations where the viewer has no access; opening a record there is not offered.

## Events

AccountInvited, AccountActivated, AccountSuspended, AccountDisabled, SignInIdentityLinked, GrantChanged, MembershipChanged (Module Admin, access review), TemplatePublished (members on older versions), DelegationStarted/Ended (banner), TypeGrantApproved/Expired, OperatorSessionRequested/Started/Ended (Campus Leader notified), BreakGlassOpened (Campus Leader notified within 24 hours), ModuleSwitchChanged (sidebar, membership checks), SetupTaskDone, CampusWentLive (OR04 status, modules), AuthorityCutOver (source chips switch to Campus OS), SettingDefinitionChanged, NoticePublished (staff acknowledgement on next sign-in), ReviewCycleOpened (one case per campus). Payloads carry IDs and `tenant_path` only (DL-45).

## Capabilities (DL-53 naming)

Existing seeds: `ga.module_grant.manage`, `ga.campus_grant.manage`, `ga.catalog.manage`, `ga.template.publish`, `tm.membership.manage`, `tm.scope.manage`, `gov.access_review.decide`, `gov.audit.view`, `gov.audit.view_snapshot`, `gov.report.export`, `gov.case.view_all`, `identity.account.view`, `identity.account.suspend`, `identity.person.view`, `identity.person.merge`, `org.campus_operator.manage`, `org.setting.manage`. New, added to the [permission catalogue](../04-access/permission-catalogue.md): `ga.module_switch.manage` · `ga.setup.manage`, `ga.setup.approve` · `ga.authority.manage` · `ga.setting_definition.manage` · `identity.idp.manage` · `identity.account.invite` · `ga.delegation.manage` · `ga.type_grant.request`, `ga.type_grant.approve` · `ga.operator_session.request`, `ga.operator_session.approve`, `ga.operator_session.break_glass` · `gov.review_cycle.manage` · `gov.purpose.manage` · `gov.retention.manage` · `gov.notice.manage`.

## Decisions (DL-165…DL-179, Assumed, awaiting Henry)

| ID | Decision | Why |
|---|---|---|
| DL-165 | **Confirmed** (Henry, 28 Nov 2026). Admin Platform scope: setup and go-live, module switch-on, authority register, setting definitions, sign-in and accounts, roles and access, special access (delegation, Tier 3 grants, operator sessions), access reviews, purposes, retention and notices. This dictionary is the home of Employee DD A01–A14, R01–R02 and the DL-defined A15–A17, R03, F01 (IDs kept, refined here); new A18 and PA01–PA10 | The Admin Platform slice (18 Oct) has no dictionary; access entities still sit in a Proposed source file; go-live, sign-in and data protection had no entities. Same pattern as Staff & HR taking over E01–E08 (DL-146) |
| DL-166 | **Confirmed** (Henry, 28 Nov 2026). Modules are switched on per Brand-country or Campus (PA01: Planned, Pilot, Live, Paused, Off, inherited downward); members only where Live or Pilot | Brands will go live module by module (Arch § Roadmap phases); grants for a module a campus doesn't use would be noise and a risk |
| DL-167 | **Confirmed** (Henry, 28 Nov 2026). A campus or brand-country goes live through a setup run: a checklist of blocking and non-blocking tasks, most checked from the owning record; READY when blocking tasks are done; the brand-country head signs off on a CAMPUS_GO_LIVE case | One place to see what's missing; nothing goes live with no Campus Leader, no published year or no sign-in. Approver = brand-country head (OQ-196 answered 28 Nov 2026) |
| DL-168 | Authority register (PA04) per brand-country × module × field group: authoritative system, sync direction, cutover date, conflict reviewer; campus exceptions allowed; a module is Live only when its rows are complete | Gives DL-45 an entity and a screen; per brand-country because legacy systems are per brand (OQ-27) |
| DL-169 | Staff sign in through their brand's identity provider with SSO and MFA (GIIS: Google Workspace); one account per Person with one or more sign-in identities, matched by provider subject, never by email; parent and student sign-in later | DL-42 one login; GIIS fact from the brief; brands use different providers (OQ-194) |
| DL-170 | Accounts: created INVITED from the employment, invite N days before start (brand-country setting, default 7; DL-13), ACTIVE on first sign-in, SUSPENDED with the employment, DISABLED the day after the last working day; Dormant is a flag for review after N days, not a status; service accounts for integrations | Joins DL-13, DL-160, DL-163 and BR-10 into one lifecycle; avoids silent suspension of rarely used accounts (OQ-204) |
| DL-171 | Access arrives three ways, all through the Registry: automatically from facts (position template DL-149, class and teaching scope DL-73, DL-133, cover DL-145), by an ACCESS_REQUEST case, or by an admin inside the ceiling; every record carries its route. A scope grant (A04) puts a person on a node; a module grant (A03) is the module ceiling | One explanation model for "Why can I see this?" (DL-38) across automatic and manual access |
| DL-172 | The GSG platform team keeps roles, templates and capabilities (DL-51); templates are versioned, a published version never changes; brands don't create roles in v1; overrides stay off (DL-14) | Keeps one catalogue across 12 brands; change requests go through the registry owner |
| DL-173 | Each brand keeps a position → standard template list (per module) for DL-149; brand HR proposes, the platform team publishes; never Tier 3 | Answers the "who maintains" half of OQ-192; contents per position stay open |
| DL-174 | Termly access review: a cycle per brand-country opens a CAMPUS_ATTESTATION case per campus in week 1 of term, due in 10 working days (S5); flagged rows first; undecided rows escalate to the brand-country head, never auto-remove | Makes the S5 flow and the "≥95% attested per term" metric concrete; auto-removal would cut teachers off mid-term |
| DL-175 | Operator sessions (A18): named GSG support engineer, one campus, named modules, purpose and ticket, read-only, at most 4 hours, Tier 3 masked, every screen logged; Campus Leader approves (DL-52); break-glass for security only, Campus Leader told within 24 hours | Confirms the DL-52 limits that are still Assumed and gives them an entity; answers most of OQ-15 |
| DL-176 | Data protection settings in the Admin Platform: purpose catalogue (GSG), retention rules per country, privacy notices per brand-country × audience; legal fields stay empty with pending markers; a Data Protection Lead role per brand-country views audit and retention and activates rules, with no record access by default | DL-44 records need an editor; keeps AR-04 (no controller named); partly answers OQ-34 |
| DL-177 | Setting definitions catalogue (PA06): every setting key once with owner module, allowed levels, default (empty means "Not set") and default override policy; the settings screen groups keys by module and shows inheritance (AP-02) | OR09 holds values but nobody defined the keys; every module dictionary adds campus or brand-country settings |
| DL-178 | New access role Brand Admin at a Brand node: setup runs, module switch-on, grants and accounts for that brand; definitions stay with Global Admin (GSG); granting Brand or Global Admin needs a second Global Admin (DL-09) | 12 brands can't all route day-to-day setup through the GSG platform team; keeps brand boundaries (DL-40) |
| DL-179 | Admin Platform navigation: Setup (Brands and campuses, Modules, Settings, Sign-in, Legacy systems) · People and access (Users, Roles and templates, Requests, Special access, Reviews) · Data protection (Data locations, Purposes, Retention, Notices) · Audit. Staff screens move to the Staff & HR module. Definitions are global; assignments, sessions, cases and audit live in the campus's data location, shown on each record | The 18 Oct screen map grouped by entity owner (GA, TM, GOV); this groups by admin task and reflects DL-146 and the data architecture |

## Open questions (new)

| ID | Question | Why it matters for design | Evidence | Affects | Who can answer | Status |
|---|---|---|---|---|---|---|
| OQ-194 | Which identity provider does each brand use for staff (GIIS: Google Workspace), does any brand run several tenants, and should Campus OS sync users from the provider or only authenticate? | Sign-in setup (PA05), account creation | Admin Platform brief (GIIS Google Workspace); OQ-27 | admin-platform dictionary, OBJ-32 | Brand IT leads | Open |
| OQ-195 | When a person works for two brands and has two work emails, which one is primary for notifications and the directory? | A02 `is_primary`, directory display | DL-42; GIIS SMART and OWIS Digital share Punggol (OQ-52) | A02, OBJ-01 | Henry, group HR | Open |
| OQ-196 | Who signs off a campus or brand-country go-live: brand-country head, Country Director, brand head or GSG operations? | PA02 approver, CAMPUS_GO_LIVE case | OQ-58 (who creates campuses) | PA02, OBJ-32 | Henry, GSG group office | **Answered** (Henry, 28 Nov 2026): brand-country head signs off → DL-167 |
| OQ-197 | Which modules does each brand go live with first, and in what order and dates? | Module switch-on samples and setup plans | Arch § Roadmap phases; OQ-05, OQ-32 | PA01, OBJ-32 | Henry, GSG leadership | Open |
| OQ-198 | Who acts as Brand Admin at each brand (brand IT lead, operations head), and may a brand ever create its own access roles or templates? | Brand Admin role (DL-178), template requests (DL-172) | OQ-16; role-catalogue "Brand leadership" | A06, permission catalogue | Henry, brand heads | Open |
| OQ-199 | How long are audit events kept per country, and may a redacted copy be kept longer in the global index? | Audit retention rule (PA08 AUDIT_EVENT) | Arch § Security; OQ-43 | A14, PA08 | Counsel per country, GSG IT security | Open |
| OQ-200 | What is the initial purpose catalogue, who owns it (group DPO, counsel, platform team), and which purposes need consent? | Purpose picker (AP-10), PA07 | DL-44; OQ-09, OQ-10 | PA07, OBJ-34 | Group DPO, counsel | Open |
| OQ-201 | Does each brand-country have a named data protection contact, and are they GSG staff, school staff or external? | Data Protection Lead role (DL-176), notice contact line | OQ-34; role-catalogue "Data protection: group and country DPOs" (no role defined) | A06, PA09 | Henry, group legal | Open |
| OQ-202 | Is a privacy notice issued per brand-country, per operating entity or per campus, and in which languages? | PA09 key, notice screens | OQ-02, OQ-06, OQ-25 | PA09, OBJ-34 | Counsel, brand marketing | Open |
| OQ-203 | Which integrations need service accounts (payroll feed, ERP journals, MyGIIS sync, Google Workspace), and who approves and rotates their keys? | Service accounts (A01 SERVICE), authority register | DL-77, DL-147; Arch § Integrations; OQ-12 | A01, PA04 | GSG IT security, brand IT | Open |
| OQ-204 | After how many days without sign-in is an account Dormant, and should dormant accounts ever be suspended automatically? | Dormant flag, access review flags | ia-screen-map SCR-GA-03 ("inactive 60 d"); S5 "no login > 60 days" | A01, PA10 | Henry, GSG IT security | Open |
| OQ-205 | Who approves a Tier 3 type grant or an operator session when the Campus Leader is the grantee, is away, or the node is above campus? | A17 and A18 approver routing | DL-69 "Watch"; DL-52 | A17, A18, OBJ-35 | Henry | Open |
| OQ-206 | Who attests access held at Brand-country, Brand and Group nodes (people with no campus), and how often? | Access review coverage beyond campuses | S5 covers campuses only | PA10, OBJ-36 | Henry, GSG IT security | Open |
| OQ-207 | Who in GSG may open break-glass, may it ever show Tier 3, and who besides the Campus Leader is told? | A18 BREAK_GLASS rules | DL-52; OQ-15; PowerSchool support-credential breach (research §08) | A18, OBJ-35 | GSG IT security, counsel | Open |
| OQ-208 | Do vendors or implementation partners (not GSG staff) ever need operator sessions, and on what contract? | Operator session eligibility | OQ-12 sub-processors; DL-52 | A18 | GSG IT, procurement | Open |
| OQ-209 | How will parents and students sign in (email or phone one-time code, MyGIIS migration, brand identity provider)? | Parent and Student apps (later, DL-118); account model must not block it | DL-42, DL-118; OQ-28 | A01, A02, PA05 | Henry, GIIS IT | Open |

Existing questions this dictionary depends on: OQ-01, OQ-02, OQ-05, OQ-06, OQ-09, OQ-10, OQ-12, OQ-15, OQ-16, OQ-18, OQ-20, OQ-21, OQ-25, OQ-26, OQ-27, OQ-28, OQ-32, OQ-34, OQ-43, OQ-55, OQ-58, OQ-166, OQ-185, OQ-192.

## Source of truth and integration ownership

| Domain | Entities | Authority | Consumers | Contract |
|---|---|---|---|---|
| Identity and Access Registry | Person, A01–A18, R01–R03, F01, PA07–PA10 | GSG platform team (DL-51) | Every module (decision service per cell), apps (sign-in, switcher), audit | Explained decisions (DL-38); events with IDs only |
| Tenancy and config | PA01–PA06 (and OR01–OR09) | GSG platform team; Brand Admins for their brand (DL-178) | Every module (switch-on, settings, source chips), migration tooling | Resolved value with source node (AP-02); `authoritative_system` per field group |
| Identity providers | Authentication, MFA | Each brand's provider (GIIS: Google Workspace) | A02 | OIDC or SAML; subject ID only |
| Staff & HR | E02, E07 | Staff & HR (DL-146) | Registry (account lifecycle, position templates) | EmploymentStarted/Ended/Suspended, CampusAssignmentChanged |
| Cases & Communications | CC03 cases | Engagement (DL-119) | Access requests, reviews, go-live, operator sessions | Case types above; deadlines from CC02 |
| Platform services | A14 storage and search | Platform services | Audit screens, reports | Append-only; redacted global index |

## Readiness

Design can start on: Setup (brand-country and campus setup runs, module switch-on matrix, authority register, settings), Users and access (account list and detail with routes, roles and templates, requests, special access), Reviews (S5 with the new flags), Data protection (purposes, retention, notices with pending markers) and Audit. Show operating entities as "[operating entity — OQ-02]", the controller as "[data controller — OQ-06]", retention periods as "Pending · OQ-43", identity providers other than GIIS's as pending (OQ-194) and go-live dates as sample values (OQ-197). Don't design Brand Admin as able to edit roles or templates (DL-172), parent sign-in (OQ-209) or a region level (OQ-59).

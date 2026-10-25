# Global Campus OS — Employee Data Dictionary v1.0

**Status:** Proposed specification for review; 14 October 2026.  
**Scope:** Employee Master Data + Access Management (GA/TM).

## Confirmed inputs

- All employees (teaching and non-teaching).
- Employees may have assignments at multiple campuses.
- Retain existing Global Admin (GA) and Team Management (TM) permission architecture.
- Employee authoritative source remains unresolved (internal master versus external HRIS).

## Modeling boundaries

- `employee_id` is an immutable global person key; employment records and employer-scoped employee numbers are separate.
- Employee campus affiliation is not identical to campus access or module membership.
- HR position, system/functional access role and academic role are distinct.
- All mutable records use the shared system fields in the **Common fields** section.
- `TIMESTAMPTZ` denotes an offset-aware timestamp stored/compared in UTC. Date interval endpoints are inclusive; time-grant boundaries should be half-open `[start, end)` after confirmation.
- `Y` = required, `N` = optional, `C` = conditionally required.
- All example values are synthetic; all enums are unapproved candidates.
- Data sensitivity: **Internal** for ordinary operational attributes; **Confidential** for personally identifying employment/contact details; **Restricted** for highly sensitive audit content.
- PK/FK field and type choices are proposals subject to data architect review.

## Entities

| ID | Entity | Domain | Owner / authority | Status |
|---|---|---|---|---|
| E01 | Employee | Employee Master | Employee Master / HRM — TBD | Confirmed population; fields proposed |
| E02 | Employment | Employee Master | Employee Master / HRM — TBD | Proposed |
| E03 | Organizational Unit | Organization | Organization Management — TBD | Proposed |
| E04 | Organizational Assignment | Organization | Employee Master / HRM — TBD | Proposed |
| E05 | Job Position | Organization | HRM / Organization Management — TBD | Proposed |
| E06 | Campus | Organization | Global Admin | Proposed |
| E07 | Employee Campus Assignment | Employee Master | Employee Master / HRM — TBD | Confirmed multi-campus; details proposed |
| E08 | External Employee Mapping | Integration | Integration / HRM — TBD | Proposed |
| A01 | User Account | Identity & Access | Identity & Access | Proposed |
| A02 | Employee Account Link | Identity & Access | Identity & Access | Proposed |
| A03 | GA Module Grant | Global Admin | Global Admin | Proposed |
| A04 | GA Campus Grant | Global Admin | Global Admin | Proposed |
| A05 | TM Module Membership | Team Management | Team Management | Proposed |
| A06 | Access Role Definition | Access Configuration | Role Catalog / Global Admin — TBD | Proposed |
| A07 | TM Role Assignment | Team Management | Team Management | Proposed |
| A08 | Permission Template | Access Configuration | Permission Configuration — TBD | Proposed |
| A09 | Template Capability | Access Configuration | Permission Configuration — TBD | Proposed |
| A10 | Role Template Binding | Team Management | Team Management | Proposed |
| A11 | Member Permission Override | Team Management | Team Management | Open — feature existence |
| A12 | Data Scope Policy | Team Management | Team Management | Proposed refinement |
| A13 | Data Scope Condition | Team Management | Team Management | Proposed refinement |
| A14 | Audit Event | Governance | Shared Audit Service — TBD | Proposed |
| R01 | Module Catalog | Access Configuration | Global Admin / Platform | Proposed |
| R02 | Capability Catalog | Access Configuration | Global Admin / Platform — TBD | Proposed |
| D01 | Academic Role Assignment | Academic Integration | Academic Allocation | Proposed |
| D02 | Teacher Subject Allocation | Academic Integration | Academic Allocation | Proposed |
| D03 | Timetable Teaching Session | Academic Integration | Timetable | Proposed |

## Detailed attribute dictionary

### E01 — Employee

**Purpose:** Durable identity of a person across employers, campuses and accounts.  
**Proposed owner:** Employee Master / HRM — TBD  
**Scope/status:** Core / Confirmed population; fields proposed

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| employee_id | UUID | PK | Y | Internal | Immutable global employee identifier; never reuse. |
| legal_first_name | VARCHAR(120) | — | Y | Confidential | Government/legal first name; localization may require alternative structure. |
| legal_last_name | VARCHAR(120) | — | C | Confidential | Required only where local naming conventions support family name. |
| preferred_name | VARCHAR(160) | — | N | Internal | Preferred display name; not a legal identifier. |
| work_email | VARCHAR(254) | — | N | Internal | Institutional contact; not necessarily unique or login identity. |
| work_phone | VARCHAR(40) | — | N | Internal | Work contact; preserve country dialing format. |
| profile_photo_ref | VARCHAR(512) | — | N | Confidential | Secure asset reference, not raw image bytes. |
| preferred_locale | VARCHAR(35) | — | N | Internal | BCP 47 language/locale for employee-facing UI. |
| record_status | ENUM | — | Y | Internal | Employee master record state, independent of employment and account status. |

### E02 — Employment

**Purpose:** Employment engagement/contract lifecycle; may be multiple over time.  
**Proposed owner:** Employee Master / HRM — TBD  
**Scope/status:** Core / Proposed

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| employment_id | UUID | PK | Y | Internal | Unique engagement/contract instance. |
| employee_id | UUID | FK:E01 | Y | Internal | Person associated with this engagement. |
| employer_org_id | UUID | FK:Org Catalog | Y | Internal | Contracting legal employer; source catalog TBD. |
| employee_number | VARCHAR(64) | — | C | Internal | External-facing HR number; uniqueness scoped by employer unless confirmed otherwise. |
| employee_category | ENUM | — | Y | Internal | Teaching, non-teaching, administration or defined local categories. |
| employment_type | ENUM | — | Y | Internal | Permanent, fixed-term, part-time, casual/other categories. |
| start_date | DATE | — | Y | Internal | First effective day of this engagement. |
| end_date | DATE | — | N | Internal | Last effective day; null indicates open-ended. |
| employment_status | ENUM | — | Y | Internal | Engagement lifecycle; distinct from account and employee record statuses. |
| termination_reason_code | VARCHAR(64) | — | N | Confidential | Controlled reason where policy permits; restrict detail. |

### E03 — Organizational Unit

**Purpose:** Department/team/organization hierarchy and reporting structure.  
**Proposed owner:** Organization Management — TBD  
**Scope/status:** Core / Proposed

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| org_unit_id | UUID | PK | Y | Internal | Organizational node key. |
| parent_org_unit_id | UUID | FK:E03 | N | Internal | Parent unit, null for permitted roots. |
| organization_id | UUID | FK:Org Catalog | Y | Internal | Owning organization or legal/entity group. |
| unit_code | VARCHAR(64) | — | Y | Internal | Code unique in approved organization scope. |
| unit_name | VARCHAR(160) | — | Y | Internal | Display name. |
| unit_type | ENUM | — | Y | Internal | Department, division, team, or other hierarchy type. |
| unit_status | ENUM | — | Y | Internal | Whether selectable for new assignments. |

### E04 — Organizational Assignment

**Purpose:** Effective-dated employee position, department and reporting assignment.  
**Proposed owner:** Employee Master / HRM — TBD  
**Scope/status:** Core / Proposed

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| org_assignment_id | UUID | PK | Y | Internal | Effective-dated role-in-organization instance. |
| employment_id | UUID | FK:E02 | Y | Internal | Employment engagement this assignment belongs to. |
| org_unit_id | UUID | FK:E03 | Y | Internal | Department/unit receiving the employee. |
| position_id | UUID | FK:E05 | Y | Internal | HR position, not access role. |
| manager_assignment_id | UUID | FK:E04 | N | Internal | Reports-to reference, self-FK; must not create cycles. |
| is_primary | BOOLEAN | — | Y | Internal | Primary organizational role flag; number allowed TBD. |
| valid_from | DATE | — | Y | Internal | Beginning of assignment interval. |
| valid_to | DATE | — | N | Internal | End of interval (inclusive); null=open. |
| assignment_status | ENUM | — | Y | Internal | Lifecycle of organizational assignment. |

### E05 — Job Position

**Purpose:** Job title or position catalog independent from application roles.  
**Proposed owner:** HRM / Organization Management — TBD  
**Scope/status:** Core / Proposed

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| position_id | UUID | PK | Y | Internal | Position catalog key. |
| position_code | VARCHAR(64) | — | Y | Internal | Code unique in owner-defined scope. |
| position_title | VARCHAR(160) | — | Y | Internal | Human resources job title, not permission role. |
| job_family | VARCHAR(100) | — | N | Internal | Broad reporting/HR classification. |
| position_status | ENUM | — | Y | Internal | Whether position can receive new assignments. |

### E06 — Campus

**Purpose:** Authoritative physical/operational campus reference.  
**Proposed owner:** Global Admin  
**Scope/status:** Reference / Proposed

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| campus_id | UUID | PK | Y | Internal | Globally stable campus identifier. |
| campus_code | VARCHAR(64) | — | Y | Internal | Code uniqueness scope defined by GA. |
| campus_name | VARCHAR(180) | — | Y | Internal | Display name. |
| organization_id | UUID | FK:Org Catalog | Y | Internal | Parent organization/brand owning campus. |
| country_code | CHAR(2) | — | Y | Internal | ISO 3166-1 alpha-2 country. |
| time_zone | VARCHAR(64) | — | Y | Internal | IANA timezone for campus-local operations. |
| campus_status | ENUM | — | Y | Internal | Active/inactive for new assignments/access. |

### E07 — Employee Campus Assignment

**Purpose:** Effective-dated employment affiliation to one or more campuses.  
**Proposed owner:** Employee Master / HRM — TBD  
**Scope/status:** Core / Confirmed multi-campus; details proposed

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| campus_assignment_id | UUID | PK | Y | Internal | Individual effective-dated employment/campus affiliation. |
| employment_id | UUID | FK:E02 | Y | Internal | Employment engagement making the assignment. |
| campus_id | UUID | FK:E06 | Y | Internal | Assigned campus. |
| is_primary | BOOLEAN | — | Y | Internal | Primary-campus indicator; HQ/no-primary case unresolved. |
| valid_from | DATE | — | Y | Internal | First date employee is affiliated with campus. |
| valid_to | DATE | — | N | Internal | Final affiliated date (inclusive), null=open. |
| assignment_status | ENUM | — | Y | Internal | Planned, active, ended etc, pending policy. |
| assignment_reason | VARCHAR(100) | — | N | Internal | Transfer/secondment/additional teaching reason code. |

### E08 — External Employee Mapping

**Purpose:** Reconciles internal employees with external HRIS identities and sync provenance.  
**Proposed owner:** Integration / HRM — TBD  
**Scope/status:** Conditional / Proposed

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| mapping_id | UUID | PK | Y | Internal | Individual external-system identity link. |
| employee_id | UUID | FK:E01 | Y | Internal | Internal canonical employee key. |
| source_system_code | VARCHAR(100) | — | Y | Internal | External system code from managed catalog. |
| source_tenant_id | VARCHAR(100) | — | N | Internal | External tenant/organization partition. |
| external_employee_id | VARCHAR(160) | — | Y | Internal | Identifier used by the external source; unique per source+tenant. |
| sync_status | ENUM | — | Y | Internal | Most recent identity sync state. |
| source_updated_at | TIMESTAMPTZ | — | N | Internal | Last modification timestamp received from source. |
| last_synced_at | TIMESTAMPTZ | — | N | Internal | Last successful processing timestamp. |

### A01 — User Account

**Purpose:** Authentication-linked account independent of employee engagement.  
**Proposed owner:** Identity & Access  
**Scope/status:** Core / Proposed

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| user_id | UUID | PK | Y | Internal | Canonical application-user key. |
| identity_provider_code | VARCHAR(100) | — | Y | Internal | Identity provider; credentials remain with IdP. |
| idp_tenant_id | VARCHAR(160) | — | N | Internal | IdP tenant namespace for subject uniqueness. |
| idp_subject_id | VARCHAR(255) | — | Y | Internal | Immutable provider subject; unique within provider tenant. |
| login_identifier | VARCHAR(254) | — | C | Internal | Display/login handle; not canonical join key. |
| account_status | ENUM | — | Y | Internal | Invited, active, suspended, disabled etc, independently managed. |
| last_login_at | TIMESTAMPTZ | — | N | Internal | Last observed successful authentication. |

### A02 — Employee Account Link

**Purpose:** Explicit time-bounded mapping of a person to login account(s).  
**Proposed owner:** Identity & Access  
**Scope/status:** Core / Proposed

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| account_link_id | UUID | PK | Y | Internal | Person/account association identifier. |
| employee_id | UUID | FK:E01 | Y | Internal | Employee linked to account. |
| user_id | UUID | FK:A01 | Y | Internal | Account being linked. |
| linked_from | TIMESTAMPTZ | — | Y | Internal | Beginning of active link. |
| linked_until | TIMESTAMPTZ | — | N | Internal | End of association, null=open. |
| link_status | ENUM | — | Y | Internal | Link activity independent of account activity. |

### A03 — GA Module Grant

**Purpose:** Outer module entitlement boundary for a user.  
**Proposed owner:** Global Admin  
**Scope/status:** Core / Proposed

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| ga_module_grant_id | UUID | PK | Y | Internal | Outer module entitlement record. |
| user_id | UUID | FK:A01 | Y | Internal | Authorized account. |
| module_id | UUID | FK:R01 | Y | Internal | Module covered by entitlement. |
| grant_status | ENUM | — | Y | Internal | Active/revoked/expired grant state. |
| valid_from | TIMESTAMPTZ | — | Y | Internal | Start of granted access. |
| valid_to | TIMESTAMPTZ | — | N | Internal | End of entitlement, null=open. |
| granted_by_user_id | UUID | FK:A01 | N | Internal | Administrator or system principal initiating grant. |

### A04 — GA Campus Grant

**Purpose:** Outer campus entitlement boundary for a user.  
**Proposed owner:** Global Admin  
**Scope/status:** Core / Proposed

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| ga_campus_grant_id | UUID | PK | Y | Internal | Outer campus entitlement record. |
| user_id | UUID | FK:A01 | Y | Internal | Authorized account. |
| campus_id | UUID | FK:E06 | Y | Internal | Campus included in GA-access boundary. |
| grant_status | ENUM | — | Y | Internal | Active/revoked/expired grant state. |
| valid_from | TIMESTAMPTZ | — | Y | Internal | Start of campus grant. |
| valid_to | TIMESTAMPTZ | — | N | Internal | End of campus grant, null=open. |
| granted_by_user_id | UUID | FK:A01 | N | Internal | Administrator or system principal granting access. |

### A05 — TM Module Membership

**Purpose:** Scoped operational membership in a module and campus.  
**Proposed owner:** Team Management  
**Scope/status:** Core / Proposed

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| tm_membership_id | UUID | PK | Y | Internal | Campus/module operational member identity. |
| user_id | UUID | FK:A01 | Y | Internal | User member in the module. |
| module_id | UUID | FK:R01 | Y | Internal | Module membership. |
| campus_id | UUID | FK:E06 | Y | Internal | Campus scope; must be covered by GA campus grant. |
| membership_status | ENUM | — | Y | Internal | Invited/active/deactivated/removed etc, exact policy open. |
| valid_from | TIMESTAMPTZ | — | Y | Internal | Beginning of membership. |
| valid_to | TIMESTAMPTZ | — | N | Internal | End of membership, null=open. |
| assigned_by_user_id | UUID | FK:A01 | N | Internal | Who assigned member. |

### A06 — Access Role Definition

**Purpose:** Global/system or functional access-role catalog (not HR job title).  
**Proposed owner:** Role Catalog / Global Admin — TBD  
**Scope/status:** Core / Proposed

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| role_id | UUID | PK | Y | Internal | Access role catalog ID. |
| role_code | VARCHAR(100) | — | Y | Internal | Stable code independent of display label. |
| role_name | VARCHAR(160) | — | Y | Internal | User-facing functional role label. |
| role_category | ENUM | — | Y | Internal | SYSTEM or FUNCTIONAL; academic domain managed separately. |
| module_id | UUID | FK:R01 | N | Internal | Module-specific role; null for globally scoped roles. |
| is_assignable | BOOLEAN | — | Y | Internal | Determines if role can be assigned operationally. |
| role_status | ENUM | — | Y | Internal | Active/retired role state. |

### A07 — TM Role Assignment

**Purpose:** Effective-dated functional/system access role assigned to a membership.  
**Proposed owner:** Team Management  
**Scope/status:** Core / Proposed

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| role_assignment_id | UUID | PK | Y | Internal | Specific membership-to-access-role assignment. |
| tm_membership_id | UUID | FK:A05 | Y | Internal | Membership receiving the role. |
| role_id | UUID | FK:A06 | Y | Internal | Assigned access role. |
| assignment_status | ENUM | — | Y | Internal | Lifecycle of role binding. |
| valid_from | TIMESTAMPTZ | — | Y | Internal | Role starts. |
| valid_to | TIMESTAMPTZ | — | N | Internal | Role ends, null=open. |
| assigned_by_user_id | UUID | FK:A01 | N | Internal | Assigning actor. |

### A08 — Permission Template

**Purpose:** Versioned reusable permission bundle.  
**Proposed owner:** Permission Configuration — TBD  
**Scope/status:** Core / Proposed

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| permission_template_id | UUID | PK | Y | Internal | Immutable template-version key. |
| template_code | VARCHAR(100) | — | Y | Internal | Stable family identifier across versions. |
| template_name | VARCHAR(160) | — | Y | Internal | Template display name. |
| template_type | ENUM | — | Y | Internal | DEFAULT, STANDARD, ADD_ON; other types TBD. |
| module_id | UUID | FK:R01 | Y | Internal | Owning module. |
| version | INTEGER | — | Y | Internal | Version number; edits after publication create new version. |
| template_status | ENUM | — | Y | Internal | Draft, active, retired. |

### A09 — Template Capability

**Purpose:** Action-level grant/deny entry inside a specific template version.  
**Proposed owner:** Permission Configuration — TBD  
**Scope/status:** Core / Proposed

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| template_capability_id | UUID | PK | Y | Internal | Permission entry inside a template version. |
| permission_template_id | UUID | FK:A08 | Y | Internal | Versioned template container. |
| capability_id | UUID | FK:R02 | Y | Internal | Atomic module action. |
| permission_effect | ENUM | — | Y | Internal | Allow/deny; deny precedence requires confirmation. |

### A10 — Role Template Binding

**Purpose:** Binding of assigned access role to versioned template(s).  
**Proposed owner:** Team Management  
**Scope/status:** Conditional / Proposed

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| role_template_binding_id | UUID | PK | Y | Internal | Specific assigned-role to template-version binding. |
| role_assignment_id | UUID | FK:A07 | Y | Internal | Instance of role assignment. |
| permission_template_id | UUID | FK:A08 | Y | Internal | Pinned template version. |
| valid_from | TIMESTAMPTZ | — | Y | Internal | When this binding becomes active. |
| valid_to | TIMESTAMPTZ | — | N | Internal | When binding ceases to apply. |
| binding_status | ENUM | — | Y | Internal | Active or revoked. |

### A11 — Member Permission Override

**Purpose:** Conditional per-membership capability override within GA boundaries.  
**Proposed owner:** Team Management  
**Scope/status:** Conditional / Open — feature existence

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| permission_override_id | UUID | PK | Y | Internal | Conditional capability exception. |
| tm_membership_id | UUID | FK:A05 | Y | Internal | Membership to which exception applies. |
| capability_id | UUID | FK:R02 | Y | Internal | Capability being overridden. |
| override_effect | ENUM | — | Y | Internal | ALLOW or DENY, limited by GA maximum authorization. |
| override_reason | VARCHAR(500) | — | C | Internal | Reason mandatory if policy enables overrides. |
| valid_from | TIMESTAMPTZ | — | Y | Internal | When override starts. |
| valid_to | TIMESTAMPTZ | — | N | Internal | When override ends. |
| override_status | ENUM | — | Y | Internal | Active or revoked. |

### A12 — Data Scope Policy

**Purpose:** Effective-dated data filter policy for a membership.  
**Proposed owner:** Team Management  
**Scope/status:** Core / Proposed refinement

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| scope_policy_id | UUID | PK | Y | Internal | Versioned scoped-access policy for one membership. |
| tm_membership_id | UUID | FK:A05 | Y | Internal | Membership whose data access is restricted. |
| scope_policy_status | ENUM | — | Y | Internal | Active or revoked. |
| valid_from | TIMESTAMPTZ | — | Y | Internal | When policy applies. |
| valid_to | TIMESTAMPTZ | — | N | Internal | When policy ceases to apply. |

### A13 — Data Scope Condition

**Purpose:** Dimension/value predicate used in composite scope evaluation.  
**Proposed owner:** Team Management  
**Scope/status:** Core / Proposed refinement

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| scope_condition_id | UUID | PK | Y | Internal | Single scoped dimension/value restriction. |
| scope_policy_id | UUID | FK:A12 | Y | Internal | Parent policy. |
| scope_group_key | UUID | — | Y | Internal | Proposed conjunctive group identifier; grouping semantics TBD. |
| scope_dimension | ENUM | — | Y | Internal | CAMPUS/ACADEMIC_YEAR/CURRICULUM/GRADE/SECTION/SUBJECT/ORG_UNIT etc. |
| scope_reference_id | UUID | FK:Domain catalog | Y | Internal | Reference to selected domain value; validate by dimension. |

### A14 — Audit Event

**Purpose:** Immutable record of important employee, account and authorization changes.  
**Proposed owner:** Shared Audit Service — TBD  
**Scope/status:** Core / Proposed

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| audit_event_id | UUID | PK | Y | Internal | Immutable audit event. |
| occurred_at | TIMESTAMPTZ | — | Y | Internal | UTC event time. |
| actor_user_id | UUID | FK:A01 | N | Internal | Initiator, null for system actions with principal separately recorded. |
| actor_type | ENUM | — | Y | Internal | USER or SYSTEM, depending on trigger. |
| entity_type | VARCHAR(100) | — | Y | Internal | Changed record type. |
| entity_id | UUID | — | Y | Internal | Changed record key. |
| action_code | VARCHAR(100) | — | Y | Internal | Controlled event/action name. |
| before_ref_or_redacted | JSON | — | N | Restricted | Minimal redacted before-state, or secure reference; avoid secret exposure. |
| after_ref_or_redacted | JSON | — | N | Restricted | Minimal redacted after-state, or secure reference; avoid secret exposure. |
| request_id | VARCHAR(128) | — | N | Internal | Correlation identifier for cross-service debugging. |
| reason_code | VARCHAR(100) | — | N | Internal | Optional standardized reason for high-impact changes. |

### R01 — Module Catalog

**Purpose:** Central list of application modules recognized by GA and TM.  
**Proposed owner:** Global Admin / Platform  
**Scope/status:** Reference / Proposed

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| module_id | UUID | PK | Y | Internal | Globally recognized module key. |
| module_code | VARCHAR(100) | — | Y | Internal | Unique stable machine identifier. |
| module_name | VARCHAR(160) | — | Y | Internal | User-facing label. |
| module_status | ENUM | — | Y | Internal | Available/retired status. |

### R02 — Capability Catalog

**Purpose:** Atomic feature/action permissions referenced by templates and overrides.  
**Proposed owner:** Global Admin / Platform — TBD  
**Scope/status:** Reference / Proposed

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| capability_id | UUID | PK | Y | Internal | Atomic authorization action key. |
| module_id | UUID | FK:R01 | Y | Internal | Module containing capability. |
| capability_code | VARCHAR(160) | — | Y | Internal | Unique stable feature/action key. |
| action_type | ENUM | — | Y | Internal | View/create/update/delete/approve/export etc. |
| capability_status | ENUM | — | Y | Internal | Available/retired status. |

### D01 — Academic Role Assignment

**Purpose:** Academic responsibilities referenced by employee_id and academic scope.  
**Proposed owner:** Academic Allocation  
**Scope/status:** External — do not own / Proposed

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| academic_assignment_id | UUID | PK | Y | Internal | Managed by Academic Allocation; reference-only sample. |
| employee_id | UUID | FK:E01 | Y | Internal | Canonical employee reference; linking key agreement needed. |
| academic_role_id | UUID | FK:Academic Role Catalog | Y | Internal | Academic role, separate from access role catalog. |
| campus_id | UUID | FK:E06 | Y | Internal | Responsibility campus. |
| academic_year_id | UUID | FK:Academic Year | Y | Internal | Academic-year scope. |
| curriculum_id | UUID | FK:Curriculum | C | Internal | Curriculum when responsibility is limited to one. |
| valid_from | DATE | — | Y | Internal | Academic assignment effective date. |
| valid_to | DATE | — | N | Internal | Academic assignment end date. |

### D02 — Teacher Subject Allocation

**Purpose:** Employee-to-subject/grade-section eligibility and responsibility.  
**Proposed owner:** Academic Allocation  
**Scope/status:** External — do not own / Proposed

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| subject_allocation_id | UUID | PK | Y | Internal | Academic Allocation-owned allocation sample. |
| employee_id | UUID | FK:E01 | Y | Internal | Employee allocated to subject/grade. |
| campus_id | UUID | FK:E06 | Y | Internal | Scope campus. |
| academic_year_id | UUID | FK:Academic Year | Y | Internal | Scope year. |
| curriculum_id | UUID | FK:Curriculum | Y | Internal | Scope curriculum. |
| subject_id | UUID | FK:Subject Catalog | Y | Internal | Allocated subject. |
| grade_id | UUID | FK:Grade Catalog | C | Internal | Grade when assignment is grade specific. |
| section_id | UUID | FK:Section Catalog | C | Internal | Section when assignment is section specific. |

### D03 — Timetable Teaching Session

**Purpose:** Scheduled sessions consumed by teacher utilization and other modules.  
**Proposed owner:** Timetable  
**Scope/status:** External — do not own / Proposed

| Attribute | Type | Key | Req. | Sensitivity | Validation / meaning |
|---|---|---|:---:|---|---|
| teaching_session_id | UUID | PK | Y | Internal | Timetable-owned unique actual scheduled session. |
| employee_id | UUID | FK:E01 | Y | Internal | Scheduled teacher reference. |
| campus_id | UUID | FK:E06 | Y | Internal | Campus. |
| academic_year_id | UUID | FK:Academic Year | Y | Internal | Academic year. |
| session_date | DATE | — | Y | Internal | Calendar date. |
| period_id | UUID | FK:Period Catalog | Y | Internal | Scheduled period. |
| subject_id | UUID | FK:Subject Catalog | N | Internal | Subject/activity mapping managed by timetable. |
| timetable_status | ENUM | — | Y | Internal | Only eligible statuses consumed; definition belongs to Timetable. |

## Cardinality and referential integrity

| Parent | Child | Cardinality | FK | Meaning | Status |
|---|---|---|---|---|---|
| E01 | E02 | 1 -> 0..N | E02.employee_id | One person may have multiple engagements across time; simultaneous contracts unresolved. | Proposed |
| E02 | E04 | 1 -> 0..N | E04.employment_id | Effective dated organizational roles, including historic moves. | Proposed |
| E03 | E03 | 1 -> 0..N | E03.parent_org_unit_id | Acyclic hierarchy; HQ and department structure TBD. | Proposed |
| E03 | E04 | 1 -> 0..N | E04.org_unit_id | Employee may hold positions in multiple departments. | Proposed |
| E05 | E04 | 1 -> 0..N | E04.position_id | HR title independent of permissions. | Proposed |
| E02 | E07 | 1 -> 0..N | E07.employment_id | Multi-campus assignment is confirmed; primary rule unresolved. | Proposed |
| E06 | E07 | 1 -> 0..N | E07.campus_id | Campus affiliation does not grant system access. | Proposed |
| E01 | E08 | 1 -> 0..N | E08.employee_id | External HRIS identities reconciled to canonical employee. | Proposed |
| E01 | A02 | 1 -> 0..N | A02.employee_id | Supports historical links, active-account cap TBD. | Proposed |
| A01 | A02 | 1 -> 0..N | A02.user_id | Recommend maximum one employee per active user account. | Proposed |
| A01 | A03 | 1 -> 0..N | A03.user_id | Module access ceiling established by GA. | Proposed |
| A01 | A04 | 1 -> 0..N | A04.user_id | Campus access ceiling established by GA. | Proposed |
| A01 | A05 | 1 -> 0..N | A05.user_id | Membership must pass both GA module and campus checks. | Proposed |
| E06 | A05 | 1 -> 0..N | A05.campus_id | Membership campus separate from HR campus affiliation. | Proposed |
| R01 | A05 | 1 -> 0..N | A05.module_id | Each membership is tied to one module. | Proposed |
| A05 | A07 | 1 -> 0..N | A07.tm_membership_id | Supports multi-role membership. | Proposed |
| A06 | A07 | 1 -> 0..N | A07.role_id | Access roles distinct from academic roles. | Proposed |
| A07 | A10 | 1 -> 0..N | A10.role_assignment_id | Conditional binding model pending stakeholder confirmation. | Proposed |
| A08 | A10 | 1 -> 0..N | A10.permission_template_id | Templates are version pinned. | Proposed |
| A08 | A09 | 1 -> 0..N | A09.permission_template_id | Template holds action-level permissions. | Proposed |
| R02 | A09 | 1 -> 0..N | A09.capability_id | Capability referenced from normalized catalog. | Proposed |
| A05 | A11 | 1 -> 0..N | A11.tm_membership_id | Override feature not yet confirmed. | Proposed |
| A05 | A12 | 1 -> 0..N | A12.tm_membership_id | Policy may restrict campus/academic/data scope. | Proposed |
| A12 | A13 | 1 -> 0..N | A13.scope_policy_id | Compound group semantics undecided. | Proposed |
| E01 | D01 | 1 -> 0..N | D01.employee_id | External Academic Allocation ownership. | Proposed |
| E01 | D02 | 1 -> 0..N | D02.employee_id | External subject allocation ownership. | Proposed |
| E01 | D03 | 1 -> 0..N | D03.employee_id | External Timetable ownership. | Proposed |

## Shared common fields

| Attribute | Type | Req. | Definition |
|---|---|---|---|
| created_at | TIMESTAMPTZ | Required | UTC creation timestamp for every persisted entity. |
| created_by_user_id | UUID nullable | Conditional | Initiator of record creation, or system principal if imported. |
| updated_at | TIMESTAMPTZ | Required | Last persisted update timestamp. |
| updated_by_user_id | UUID nullable | Conditional | Last modifying user/system principal. |
| row_version | INTEGER | Required | Optimistic concurrency/version check for mutable records. |
| source_system_code | VARCHAR(100) nullable | Conditional | Provenance for externally sourced writes where needed. |

## Candidate enums — require approval

| Entity | Field | Values | Description |
|---|---|---|---|
| E01 | record_status | ACTIVE, INACTIVE, ARCHIVED | Employee master retention state; not HR termination. |
| E02 | employee_category | TEACHING, NON_TEACHING, ADMINISTRATION, OTHER | Business grouping; overlap with admin roles to validate. |
| E02 | employment_type | PERMANENT, FIXED_TERM, PART_TIME, CONTRACT, OTHER | Engagement type, independent of teaching category. |
| E02 | employment_status | PENDING, ACTIVE, SUSPENDED, ENDED | Separation/leave policies TBD. |
| E03 | unit_type | ORGANIZATION, DIVISION, DEPARTMENT, TEAM | Hierarchy classifications. |
| E03 | unit_status | ACTIVE, INACTIVE | New assignments blocked when inactive. |
| E04 | assignment_status | PLANNED, ACTIVE, ENDED, CANCELLED | Date/status consistency required. |
| E05 | position_status | ACTIVE, INACTIVE | Position catalog status. |
| E06 | campus_status | ACTIVE, INACTIVE | Global Admin campus state. |
| E07 | assignment_status | PLANNED, ACTIVE, ENDED, CANCELLED | Multi-campus effective-date states. |
| E08 | sync_status | PENDING, SYNCED, FAILED, CONFLICT | Source-of-truth and conflict reconciliation TBD. |
| A01 | account_status | INVITED, ACTIVE, SUSPENDED, DISABLED, CLOSED | Do not infer status solely from employment. |
| A02 | link_status | ACTIVE, ENDED | No re-use of identities without explicit lifecycle rule. |
| A03/A04 | grant_status | ACTIVE, REVOKED, EXPIRED | Effective grant policy. |
| A05 | membership_status | INVITED, ACTIVE, DEACTIVATED, REMOVED | Distinguish revoked access and historical member record. |
| A06 | role_category | SYSTEM, FUNCTIONAL | Academic role managed by Academic Allocation. |
| A06 | role_status | ACTIVE, RETIRED | Allow assignments only to active role definitions. |
| A07 | assignment_status | ACTIVE, ENDED, REVOKED | Role assignment lifecycle. |
| A08 | template_type | DEFAULT, STANDARD, ADD_ON | Existing conceptual template categories. |
| A08 | template_status | DRAFT, ACTIVE, RETIRED | Published templates immutable/versioned: proposed. |
| A09 | permission_effect | ALLOW, DENY | Deny precedence to confirm. |
| A11 | override_effect | ALLOW, DENY | Enable only if feature is approved. |
| A12 | scope_policy_status | ACTIVE, REVOKED | Data scope constraint active status. |
| A13 | scope_dimension | CAMPUS, ACADEMIC_YEAR, CURRICULUM, GRADE, SECTION, SUBJECT, ORG_UNIT | Actual allowed dimensions vary by consuming module. |
| A14 | actor_type | USER, SYSTEM | System principal identity/correlation needed. |
| R02 | action_type | VIEW, CREATE, UPDATE, DELETE, APPROVE, EXPORT, MANAGE | Catalog owned centrally; expand per module. |

## Atomic business rules

- **BR-01 (Proposed):** Each person shall have one immutable, never-reused canonical employee_id. [Decision: D02]
- **BR-02 (Confirmed requirement; model proposed):** One employee may be affiliated with multiple campuses through distinct effective-dated assignment records. [Decision: D04]
- **BR-03 (Proposed):** A historical employment/campus period must not be overwritten when a new period begins. [Decision: D03]
- **BR-04 (Assumed):** A person may exist as an employee without an active user account. [Decision: D05]
- **BR-05 (Proposed):** Employment status and account status are independent and require explicit lifecycle coordination. [Decision: D10]
- **BR-06 (Proposed):** Campus employment affiliation alone does not grant application campus access. [Decision: D07]
- **BR-07 (Existing architecture retained):** TM permissions and memberships must remain inside the GA module and campus entitlement boundary.
- **BR-08 (Open):** Overlapping role permissions must use one deterministic combination and deny-precedence policy. [Decision: D08]
- **BR-09 (Existing integration direction):** Academic assignments are owned by Academic Allocation and scoped to campus/academic year/curriculum as relevant. [Decision: D11]
- **BR-10 (Proposed):** An employment exit must initiate a documented access-review or revocation process. [Decision: D10]
- **BR-11 (Proposed):** Sensitive employee fields may be viewed/changed only by permitted actors and must be minimized in exports. [Decision: D13]
- **BR-12 (Proposed):** High-impact changes to employment, campus, account, role, access and data scope must produce immutable audit events. [Decision: D12]
- **BR-13 (Proposed):** Assignment valid_to must not precede valid_from.
- **BR-14 (Proposed):** Overlapping assignments for the same employment/campus require an explicitly approved policy; duplicates must be rejected by default. [Decision: D04]
- **BR-15 (Proposed based on GA/TM architecture):** TM membership creation must validate an active GA module grant and a campus grant at the target effective time.
- **BR-16 (Conditional proposal):** A membership override, if supported, must never expand access beyond GA entitlements. [Decision: D09]
- **BR-17 (Proposed):** Missing data scope must not silently mean global or unrestricted access. [Decision: D08]
- **BR-18 (Proposed):** Published permission template versions should remain immutable; updates should create a new version. [Decision: D09]
- **BR-19 (Proposed):** Active user-account-to-employee links must pass uniqueness checks before creation and historical links must be retained. [Decision: D05]
- **BR-20 (Proposed):** Source conflicts must be quarantined or flagged for resolution rather than silently overwriting employee master data. [Decision: D01]

## Unresolved decisions

| ID | Priority | Decision | Question | Default direction (not confirmed) |
|---|---|---|---|---|
| D01 | P0 | Source of truth | Which system writes employee identity, employment and campus affiliation? | Support internal or external HRIS via mapping until decided. |
| D02 | P0 | Employee identifiers | Is there one person-level employee number, a separate number per employer, or both? | Keep immutable UUID; employee_number belongs to Employment for now. |
| D03 | P0 | Concurrent employments | Can a person hold concurrent employment engagements/legal employers? | Allow historical N, enforce active policy later. |
| D04 | P0 | Primary campus | Must each active employment have exactly one primary campus? Can HQ staff have none? | Support multiple; do not enforce exactly one until approved. |
| D05 | P0 | Account linking | Can one employee have multiple active accounts? Can one account map to multiple employees? | Prefer 0..1 active account per employee and employee per account. |
| D06 | P0 | Organization model | Which unit owns department, legal employer, brand, HQ, country and campus hierarchies? | Keep separate catalog references; validate hierarchy with clients. |
| D07 | P0 | Campus transfer access | Does changing affiliation trigger or require GA/TM grant changes? | Never silently grant; initiate review workflow. |
| D08 | P0 | Permission evaluation | How are multiple roles, deny, data-scope groups and missing scope combined? | Fail closed; define explicit union/intersection/deny logic. |
| D09 | P0 | Templates and overrides | Are per-user overrides allowed, and can inherited templates be changed retroactively? | Treat override and role-template-binding features as conditional. |
| D10 | P0 | Lifecycle coordination | What states/approvals cascade from exit, leave, rehire or suspension? | Explicit events and no automatic reactivation without approval. |
| D11 | P1 | Academic allocation | How are academic role/subject scopes kept independent but consistent with employment and permissions? | Academic Allocation remains assignment owner. |
| D12 | P1 | History and retention | What retention and legally required deletion/anonymization periods apply by country? | Use effective-dated history; implement retention by jurisdiction. |
| D13 | P1 | Field privacy | Which fields can a manager, teacher, HR admin and module admin view/edit? | Classify by data sensitivity and operations. |
| D14 | P1 | Scope identity | Are academic scopes based on common global catalog IDs or per-campus IDs? | Refer to authoritative academic catalog IDs with explicit mapping. |
| D15 | P1 | Status vocabulary | What precise employment, membership and account statuses/transitions are supported? | Enumerations in workbook are candidates only. |

## Source of truth and integration ownership

| Domain | Related entities | Proposed authority | Consumers | Contract |
|---|---|---|---|---|
| Employee identity | E01 | Employee Master/HRM TBD | HRM, TM, GA, Academic Allocation, Timetable | Canonical ID, profile reads and change events |
| Employment engagements | E02 | Employee Master/HRM TBD | Employee Directory, IAM, GA, academic | Join, end, suspend signals; avoid direct permission grants |
| Organization structure | E03/E05 | Org Management/HRM TBD | Directory, TM, reporting | Versioned unit/position catalogs |
| Employee campus affiliation | E07 | Employee Master/HRM TBD | GA, TM, Academic Allocation | Employment affiliation not equal to grant |
| Campus catalog | E06 | Global Admin | Employee Master, TM, academic modules | Stable campus IDs; active status |
| Authentication/user identity | A01/A02 | IAM | GA, TM, Notification, Activity Log | SSO provider and account-link lifecycle |
| Global entitlements | A03/A04 | Global Admin | TM, all modules | Hard ceiling for module/campus access |
| Membership and operational roles | A05/A07 | Team Management | All functional modules | Must be within GA entitlement |
| Permission templates | A08/A09 | Permission Configuration TBD | GA, TM, all modules | Versioning/assignment policy open |
| Scoped access | A12/A13 | Team Management | Functional modules | Group semantics and precedence open |
| Academic role and teaching mapping | D01/D02 | Academic Allocation | TM, Timetable, utilization | Reference by employee_id + academic scope |
| Scheduled sessions | D03 | Timetable | Teacher Utilization, LMS | Timetable is source of scheduled activity |
| Audit events | A14 | Shared Audit Service TBD | Security, Support, Compliance | Immutable redacted event log |

## Change event candidates

| Event | Entity | Suggested payload | Consumers | Risk policy |
|---|---|---|---|---|
| EmployeeCreated | E01 | employee_id, changed_fields | Directory, identity reconciliation | Idempotent replay; source authority TBD |
| EmploymentStarted | E02 | employment_id, employee_id, employer_id, start_date | IAM onboarding, campus operations | Do not silently enable account |
| EmploymentEnded | E02 | employment_id, employee_id, end_date, reason_code | IAM, GA, TM, Academic Allocation | Revocation review required; preserve audit |
| CampusAffiliationChanged | E07 | employee_id, campus_id, valid dates, primary flag | GA, TM, Academic Allocation | No automatic access grant |
| AccountStatusChanged | A01 | user_id, old_status, new_status | GA, TM, all modules | Fail closed for disabled accounts |
| GAGrantChanged | A03/A04 | user_id, grant_type, target_id, interval | Team Management | Validate existing TM memberships and notify |
| MembershipChanged | A05 | user_id, module_id, campus_id, status | Functional module | Preserve history; audit |
| RoleAssignmentChanged | A07 | membership_id, role_id, effective interval | Functional module | Recompute effective rights |
| ScopePolicyChanged | A12/A13 | membership_id, policy_id, scope dimensions | Functional module | Invalidate effective permissions cache |

## Implementation guardrails

- Enforce referential integrity where domains share a storage boundary; use validated integration references otherwise.
- Do not join people by email, display name or employee_number; use canonical IDs and explicit external mappings.
- Enforce `valid_to >= valid_from`, prevent overlapping duplicate active assignments unless approved, and time-bound grants.
- Never silently upgrade GA or TM authorization when an employment or campus affiliation changes.
- Passwords, access tokens, sensitive identity documents and full audit snapshots do not belong in this Employee Data Dictionary.
- Review jurisdiction-specific privacy, access/export restrictions, retention and data residency before production.

## Readiness

**Not ready to freeze as a physical database schema until P0 decisions D01–D10 are resolved.**

# Global Campus OS — Organisation Data Dictionary v0.1

**Status:** Proposed, 9 Nov 2026; DL-47…DL-50 Confirmed 12 Nov 2026 · **Owner:** Henry
**Cites:** [architecture.md § Organisation and tenancy](./architecture.md#organisation-and-tenancy-dl-40-dl-41) · [tenancy-and-scope](./tenancy-and-scope.md) · [gsg-landscape](../01-context/gsg-landscape.md) · [gsg-footprint.json](../01-context/gsg-footprint.json) · [Employee Data Dictionary v1](../00-sources/Global_Campus_OS_Employee_Data_Dictionary_v1.md) E06, A03, A04, Org Catalog · [Architecture v1 § Multi-campus and tenancy model](../00-sources/Global_Campus_OS_Product_Architecture_v1.md#multi-campus-and-tenancy-model) · DL-17, DL-29, DL-35, DL-39…DL-41, DL-45 · AR-01…AR-04 · MC-01…MC-04, MC-06, MC-14 · OQ-02…OQ-05, OQ-20…OQ-22, OQ-50…OQ-59
**Tóm tắt (VI):** Từ điển dữ liệu cho cây tổ chức của Campus OS: nút phạm vi (Group › Brand › Brand-country › Campus › Segment), vùng dữ liệu (cell), pháp nhân và liên kết "ai vận hành campus" theo thời gian, cùng giá trị cài đặt kế thừa. Danh sách pháp nhân, mã campus và vùng dữ liệu ngoài SG/IN vẫn là câu hỏi mở.

> Drafted by Claude from product truth on 9 Nov 2026 for Henry's review. The structure (tree, data location per country, legal entity beside the tree) is Confirmed by DL-39…DL-41. DL-47…DL-50 were confirmed by Henry on 12 Nov 2026; OQ-57…OQ-59 are added to the register.

## How to read this file

- Same layout as the [Student Directory Data Dictionary](./student-directory-data-dictionary.md): entities, one attribute table per entity, then cardinality, enums, rules, decisions and integration ownership.
- **Status** per attribute: **Known** (cited), **Confirmed** (DL by Henry), **Assumed** (DL, expect change), **Open** (OQ; keep generic in designs).
- **Tier:** everything here is **T1** Internal. The organisation tree lives in the global layer (DL-39), so no field may hold personal data.
- Rows carry the ED shared common fields (`created_at`, `created_by_user_id`, `updated_at`, `updated_by_user_id`, `row_version`, `source_system_code`); not repeated per table.
- Real brand, campus and country names come from the public campus list ([gsg-landscape](../01-context/gsg-landscape.md) § 1). Campus codes are placeholders (OQ-55).

## Scope of this dictionary

| In this dictionary | Not in this dictionary (owner) |
|---|---|
| Scope tree: Group, Brand, Brand-country, Campus, Segment | Programme offering, academic year, term, grade, class, subject (Academic structure; OBJ-15) |
| Data location (cell) per country | Regulator pack contents, reports and identifiers (Country packs; OQ-22) |
| Legal entity and the effective-dated campus operator link | Which entity is controller or processor (OQ-06; Governance dictionary) |
| Inherited setting values and who may override them | Individual settings per module (each module's dictionary) |
| — | HR org units, divisions, departments (ED E03; not the scope tree) |

## Modeling boundaries

- **The scope tree is not the legal structure.** Brand, Brand-country and Campus are how GSG runs and grants access. Legal entities sit beside the tree and link to campuses over time (DL-41). An operating entity is never a scope level and never a picker option.
- **The scope tree is not the HR org chart.** ED E03 org units (departments, teams) describe reporting lines. Grants and memberships sit on scope nodes only (DL-35, DL-40).
- **Brand-country is a node, not a country.** "GIIS Singapore" and "OWIS Singapore" are two nodes in one country and one data location. A grant on GIIS Singapore never reaches OWIS Singapore (DL-40, answers OQ-17).
- **Data location follows country.** A Brand-country's country code selects its data location (DL-39, DL-40). Whether any country shares or splits a region is open (OQ-21). Product term: DL-47.
- **Campus is what GSG lists as a campus.** A school split by stage across buildings (Harrods Phnom Penh) is several campuses. Whether moving between them is a transfer is OQ-50.
- **Segment sits on a campus.** Segment (DL-29) is a campus-level grouping such as Primary; programme offerings belong to Academic structure.

## Entities

| ID | Entity | Home | Status |
|---|---|---|---|
| OR01 | Scope node | Global | Confirmed structure (DL-40); record shape Confirmed (DL-48) |
| OR02 | Brand | Global | Known (12 brands, footprint); level Confirmed (DL-40) |
| OR03 | Brand-country | Global | Confirmed (DL-40); 21 nodes Known (footprint) |
| OR04 | Campus | Global | Known (Arch § Tenancy; ED E06; 62 listed campuses); extends E06 |
| OR05 | Campus segment | Global | Term Confirmed (DL-29); values per campus Open (OQ-20) |
| OR06 | Data location | Global | Confirmed (DL-39); regions beyond SG and IN Open (OQ-21) |
| OR07 | Legal entity | Global | Confirmed structure (DL-41); list Open (OQ-02) |
| OR08 | Campus operator | Global | Confirmed (DL-41); values Open (OQ-02, OQ-03) |
| OR09 | Setting value | Global (definition), node | Known rule (AR-03); overridable list Open (OQ-20) |

## Detailed attribute dictionary

### OR01 — Scope node

**Purpose:** One record per node of the tree, so grants (A03, A04), memberships (A05) and `tenant_path` all point at the same thing.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| scope_node_id | UUID | PK | Y | T1 | Confirmed (DL-48) | Stable; never reused. Grants and memberships store it as `scope_node_id`. |
| node_type | ENUM | — | Y | T1 | Confirmed levels (DL-40) | GROUP, BRAND, BRAND_COUNTRY, CAMPUS, SEGMENT. Replaces GROUP/COUNTRY/CAMPUS in rbac-model (DL-17 → DL-40). |
| parent_scope_node_id | UUID | FK:OR01 | C | T1 | Confirmed (DL-40) | Null only for the single GROUP node. Parent type is fixed: BRAND→GROUP, BRAND_COUNTRY→BRAND, CAMPUS→BRAND_COUNTRY, SEGMENT→CAMPUS. |
| path | VARCHAR(512) | UK | Y | T1 | Confirmed (DL-48) | Materialised ancestor path, e.g. `gsg/giis/giis-sg/sg-pgl`; the `tenant_path` copied onto records. |
| display_name | VARCHAR(180) | — | Y | T1 | Known (footprint) | Shown in the breadcrumb (AP-01). Brand legal vs display name: OQ-56. |
| node_status | ENUM | — | Y | T1 | Confirmed (DL-50) | See enums. A closed node keeps its history and grants are ended, not deleted. |
| valid_from / valid_to | DATE | — | Y / N | T1 | Known rule (AR-03 effective dating) | When the node exists in the tree. |

Group has no profile entity: there is one GROUP node, GSG (Known, DL-22). Regions above Brand-country, if GSG uses them, are open (OQ-59).

### OR02 — Brand

**Purpose:** A school brand GSG runs. Owns brand identity, brand policy templates and brand-wide roles (DL-40).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| scope_node_id | UUID | PK, FK:OR01 | Y | T1 | Confirmed (DL-48) | node_type = BRAND. |
| brand_code | VARCHAR(16) | UK | Y | T1 | Placeholder (OQ-55) | Footprint uses GIIS, OWIS, WIT, HIS, VIK, GLE, HH, REG, DWI, EAS, CISM, DOM as working codes. |
| display_name | VARCHAR(180) | — | Y | T1 | Known (footprint); Open (OQ-56) | e.g. "Global Indian International School". |
| legal_name | VARCHAR(240) | — | N | T1 | Open (OQ-56) | Not the operating entity; a brand may span several entities (OR07). |
| brand_assets_ref | VARCHAR(512) | — | N | T1 | Open (OQ-26) | Logo and colours; how brand shows in shared shells is open. |

### OR03 — Brand-country

**Purpose:** A brand in one country. Holds the regulator pack, currency and tax, holidays and fee protection; selects the data location (DL-40).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| scope_node_id | UUID | PK, FK:OR01 | Y | T1 | Confirmed (DL-48) | node_type = BRAND_COUNTRY. |
| country_code | CHAR(2) | UK with brand | Y | T1 | Confirmed (DL-40) | ISO 3166-1 alpha-2. One node per brand and country. UK campuses use `GB`. |
| data_region_id | UUID | FK:OR06 | Y | T1 | Confirmed (DL-39, DL-40) | Derived from country_code; never chosen per campus. |
| regulator_pack_code | VARCHAR(32) | — | C | T1 | Known for SG, IN (regulatory-context); others Open (OQ-22) | Shown as pending outside SG and IN. |
| currency_code | CHAR(3) | — | Y | T1 | Known (Arch § Tenancy "currency and tax") | ISO 4217. |
| holiday_calendar_ref | UUID | FK:Calendar | N | T1 | Known (Arch § Tenancy) | Public holidays; campuses add their own (OR09). |
| languages_offered | VARCHAR(35)[] | — | N | T1 | Known field (Arch § Tenancy); values Open (OQ-25, OQ-53) | BCP 47 tags. UI copy stays English in v1 (DL-15). |

### OR04 — Campus

**Purpose:** A school site where students enrol and staff are assigned. Extends ED E06 (same `campus_id`) with its place in the tree.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| campus_id | UUID | PK | Y | T1 | Known (ED E06) | Globally stable. Equals the campus `scope_node_id` (DL-48). |
| campus_code | VARCHAR(64) | UK | Y | T1 | Known field (E06); standard Open (OQ-55) | Sample codes (SG-PGL, IN-NOI…) are placeholders. |
| campus_name | VARCHAR(180) | — | Y | T1 | Known (footprint) | Names repeat across brands ("Whitefield"); always show with brand. |
| brand_country_node_id | UUID | FK:OR03 | Y | T1 | Confirmed (DL-40) | Replaces E06 `organization_id` as the parent. Brand and country derive from it. |
| city | VARCHAR(120) | — | Y | T1 | Known (footprint) | |
| address_ref | UUID | — | N | T1 | Open (OQ-52) | Whether campuses of different brands share a site is open. |
| time_zone | VARCHAR(64) | — | Y | T1 | Known (E06) | IANA, e.g. Asia/Singapore. |
| stages_served | ENUM[] | — | N | T1 | Known (footprint labels, e.g. preschool, primary) | Descriptive only; not Segments (OR05) and not a transfer rule (OQ-50). |
| campus_status | ENUM | — | Y | T1 | Known ACTIVE, INACTIVE (E06); extended Confirmed (DL-50) | See enums. Whether unlisted campuses still operate: OQ-57. |
| first_release | BOOLEAN | — | N | T1 | Open (OQ-05, OQ-32) | Not shown in designs until answered. |

### OR05 — Campus segment

**Purpose:** A school segment on one campus, such as Primary (DL-29). Owns school-day timings and segment events (Arch § Tenancy).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| scope_node_id | UUID | PK, FK:OR01 | Y | T1 | Confirmed (DL-48) | node_type = SEGMENT. Not a grant level (DL-40 grants stop at Campus); used for settings and data scope. |
| segment_code | VARCHAR(32) | UK with campus | Y | T1 | Known values for GIIS Noida (Pre-Primary, Primary, Middle, Higher; Noida Almanac) | Other campuses' lists: OQ-20. Never "Section" (DL-29). |
| display_name | VARCHAR(80) | — | Y | T1 | Known (Noida Almanac) | |
| grade_range | VARCHAR(64) | — | N | T1 | Open (OQ-51) | Grade levels come from the programme pack (OBJ-15). |

### OR06 — Data location

**Purpose:** Where a country's school data lives: one deployment ("cell") per country (DL-39). Product term: **Data location**, shown as "Data location: Singapore" (DL-47, Confirmed).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| data_region_id | UUID | PK | Y | T1 | Confirmed (DL-39) | |
| data_region_code | VARCHAR(16) | UK | Y | T1 | Confirmed (DL-47) | e.g. `SG`, `IN`. Engineering may call it a cell. |
| display_name | VARCHAR(80) | — | Y | T1 | Confirmed (DL-47) | Country name, e.g. "Singapore"; UI label reads "Data location: Singapore". |
| country_codes | CHAR(2)[] | — | Y | T1 | Known for SG, IN (Arch); others Open (OQ-21) | One country per region until OQ-21 says otherwise. |
| region_status | ENUM | — | Y | T1 | Confirmed (DL-50) | PLANNED, LIVE. Eight footprint countries are PLANNED placeholders (OQ-21). |

### OR07 — Legal entity

**Purpose:** A company that operates campuses, employs staff or runs the platform (DL-41). Fills ED's "Org Catalog" (`employer_org_id`, `organization_id`).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| legal_entity_id | UUID | PK | Y | T1 | Confirmed (DL-41) | Target of `operating_entity_id` and ED `employer_org_id`. |
| legal_name | VARCHAR(240) | — | Y | T1 | Open (OQ-02) | No entity is named in product truth. Designs show "[operating entity — OQ-02]". |
| entity_type | ENUM | — | Y | T1 | Confirmed values (DL-41) | GROUP_OPERATOR, SCHOOL_OPERATOR, PARTNER, EMPLOYER (one entity may be several; stored as one primary type plus OR08 links). |
| country_of_incorporation | CHAR(2) | — | Y | T1 | Open (OQ-02) | |
| registration_number | VARCHAR(64) | — | N | T1 | Open (OQ-02) | Format per country. |
| partner_agreement_ref | UUID | — | C | T1 | Open (OQ-03) | Required for PARTNER; terms unknown. |
| entity_status | ENUM | — | Y | T1 | Confirmed (DL-50) | ACTIVE, DISSOLVED. |

Controller, fiduciary or processor roles are **not** attributes here; they belong to the Governance dictionary once OQ-06 is answered (AR-04).

### OR08 — Campus operator

**Purpose:** Which legal entity operates a campus, and from when (DL-41). Enrolment, employment and cases copy `operating_entity_id` from the link valid on their creation date.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| campus_operator_id | UUID | PK | Y | T1 | Confirmed (DL-41) | |
| campus_id | UUID | FK:OR04 | Y | T1 | Confirmed (DL-41) | |
| legal_entity_id | UUID | FK:OR07 | Y | T1 | Confirmed (DL-41); values Open (OQ-02) | |
| operator_role | ENUM | — | Y | T1 | Confirmed (DL-50) | OPERATOR, FRANCHISEE, JV_PARTNER. Which campuses are partner-run: OQ-03. |
| valid_from / valid_to | DATE | — | Y / N | T1 | Confirmed (DL-41) | No overlapping OPERATOR links for one campus. Handover effects: OQ-04. |
| change_case_id | UUID | FK:Case | N | T1 | Confirmed (DL-50) | The case behind a handover. |

### OR09 — Setting value

**Purpose:** A setting set at one scope node and inherited below it (AR-03, AP-02).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| setting_value_id | UUID | PK | Y | T1 | Known rule (AR-03) | |
| setting_key | VARCHAR(120) | FK:setting definition | Y | T1 | Known examples (Arch § Tenancy: calendar, fee plans, school-day timings) | Definitions belong to each module. |
| scope_node_id | UUID | FK:OR01 | Y | T1 | Known (AR-03) | Where the value is set. |
| value | JSON | — | Y | T1 | Known (AR-03) | Never personal data. |
| override_policy | ENUM | — | Y | T1 | Confirmed values (DL-50); which settings: Open (OQ-20) | OPEN, LOCKED_BELOW. Shown as "Locked by GSG" or "Inherited from GIIS Singapore" (AP-02). |
| valid_from / valid_to | DATE | — | Y / N | T1 | Known (AR-03 effective dating) | |

## Cardinality and referential integrity

| Parent | Child | Cardinality | FK | Meaning | Status |
|---|---|---|---|---|---|
| OR01 GROUP | OR02 | 1 → 1..N | parent_scope_node_id | GSG has 12 listed brands | Known |
| OR02 | OR03 | 1 → 1..N | parent_scope_node_id | GIIS has 5 brand-countries, Witty 1 | Known (footprint) |
| OR03 | OR04 | 1 → 1..N | brand_country_node_id | 62 listed campuses across 21 nodes | Known (footprint) |
| OR04 | OR05 | 1 → 0..N | parent_scope_node_id | Segments per campus | Known for Noida only |
| OR06 | OR03 | 1 → 0..N | data_region_id | Several brands share one country's region (e.g. GIIS, OWIS, Heath House in SG) | Confirmed (DL-40) |
| OR04 | OR08 | 1 → 1..N over time | campus_id | One OPERATOR at a time; history kept | Confirmed (DL-41) |
| OR07 | OR08 | 1 → 0..N | legal_entity_id | One entity may operate many campuses | Confirmed (DL-41) |
| OR01 | A03, A04, A05 | 1 → 0..N | scope_node_id | Grants and memberships on a node cover its descendants | Confirmed (DL-35, DL-40) |
| OR04 | ST02, E07 | 1 → 0..N | campus_id | Enrolments and campus assignments | Known |
| OR08 | ST02, E02, Case | copied | operating_entity_id | Copied at creation; never re-pointed | Confirmed (DL-41) |

## Candidate enums — approved (DL-50)

| Entity | Field | Values | Source of terms |
|---|---|---|---|
| OR01 | node_type | GROUP, BRAND, BRAND_COUNTRY, CAMPUS, SEGMENT | DL-40, DL-29 |
| OR01 | node_status | ACTIVE, CLOSED | ED E06 ACTIVE/INACTIVE, renamed for tree nodes |
| OR04 | campus_status | PLANNED, ACTIVE, INACTIVE, CLOSED | ED E06 (ACTIVE, INACTIVE) plus opening and closure |
| OR04 | stages_served | PRESCHOOL, PRIMARY, SECONDARY, SENIOR_SECONDARY, POST_SECONDARY | Footprint labels; POST_SECONDARY for Regent Swinburne (OQ-54) |
| OR06 | region_status | PLANNED, LIVE | DL-39 |
| OR07 | entity_type | GROUP_OPERATOR, SCHOOL_OPERATOR, PARTNER, EMPLOYER | DL-41 |
| OR07 | entity_status | ACTIVE, DISSOLVED | — |
| OR08 | operator_role | OPERATOR, FRANCHISEE, JV_PARTNER | DL-41, OQ-03 |
| OR09 | override_policy | OPEN, LOCKED_BELOW | AR-03, MC-14 (iSAMS Central Manage, Arbor pushdowns) |

Chip colours for design: ACTIVE and LIVE `success`; PLANNED `info`; INACTIVE `warning`; CLOSED and DISSOLVED `default` (proposed, AP-14).

## Business rules

- **OR-R01 (Confirmed, DL-40):** The tree is Group › Brand › Brand-country › Campus › Segment. Grants and memberships sit at Group, Brand, Brand-country or Campus and cover every node below.
- **OR-R02 (Confirmed, DL-40):** One Brand-country per brand and country; its country selects the data location.
- **OR-R03 (Confirmed, DL-39):** The tree, legal entities and settings live in the global layer and hold no personal data.
- **OR-R04 (Confirmed, DL-41):** A campus has exactly one OPERATOR link on any date. Records copy `operating_entity_id` on creation and keep it after a handover.
- **OR-R05 (Known, AR-03):** Settings inherit downward and are effective-dated; a lower node may override only where the setting is OPEN.
- **OR-R06 (Confirmed, DL-48):** Nodes are never deleted or re-parented. A campus that moves brand closes and a new node opens; records keep the old `tenant_path`.
- **OR-R07 (Known, Arch § Tenancy):** No screen hard-codes one campus's calendar, currency, grading scale or regulator.
- **OR-R08 (Known, ED BR-06 principle):** Campus affiliation or operator links never grant access; only grants and memberships do.
- **OR-R09 (Known, SR-13 pattern):** Campuses are joined by `campus_id`, never by name; names repeat across brands.

## Decisions (Confirmed by Henry, 12 Nov 2026)

| ID | Decision | Why |
|---|---|---|
| DL-47 | Product term for a cell is **Data location** in staff and parent UI ("Data location: Singapore"); "cell" stays in engineering docs | "Cell" is infrastructure jargon; one plain term reads clearly for staff and parents. Confirmed by Henry, 12 Nov 2026 |
| DL-48 | One Scope node record (OR01) per tree node with typed profiles (OR02–OR05); `campus_id` equals the campus node ID; nodes are closed, never deleted or moved | Grants, memberships and `tenant_path` point at one key; history survives brand moves |
| DL-49 | Every campus on GSG's public list is a Campus node, including stage campuses of one school (Harrods Phnom Penh) | Keeps the tree matching how GSG describes itself; the transfer question stays in OQ-50 (recommended: a stage move is promotion, not transfer) |
| DL-50 | Candidate enums above, including PLANNED and CLOSED campus states and setting override policy | Designers need fixed labels; same approach as DL-27 |

## Open questions (added to the register)

| ID | Question | Why it matters for design | Evidence | Status |
|---|---|---|---|---|
| OQ-57 | Which campuses operate today, and are GIIS East Coast, GIIS Bangkok and OWIS Suntec still running? | Sample data uses East Coast (SG-ECS) for the type A transfer example; campus pickers must match reality | Not on the public list on 9 Nov 2026 but named elsewhere ([gsg-landscape](../01-context/gsg-landscape.md) § 1) | Open |
| OQ-58 | Which team creates, renames and closes brands, campuses and segments, and who approves a campus handover? | Campus catalogue and operator-change screens need an owner and an approver | ED E06 owner "Global Admin"; OQ-46 (registry owner) | Open |
| OQ-59 | Does GSG group brand-countries into regions (for example South-East Asia) for leadership, reporting or support? | The tree has no level between Group and Brand, so a regional head cannot be granted a region | Not in Architecture or the public site | Open |

Existing questions this dictionary depends on: OQ-02, OQ-03, OQ-04, OQ-05, OQ-06, OQ-20, OQ-21, OQ-22, OQ-25, OQ-26, OQ-32, OQ-50, OQ-51, OQ-52, OQ-53, OQ-54, OQ-55, OQ-56.

## Source of truth and integration ownership

| Domain | Entities | Proposed authority | Consumers | Contract |
|---|---|---|---|---|
| Scope tree | OR01–OR05 | Tenancy and config (team: OQ-58) | Access Registry, every module, apps (scope switcher) | Stable `scope_node_id`, `path`; node events |
| Data locations | OR06 | Platform (infrastructure) | Global edge routing, Access Registry (cell step) | Region per country |
| Legal entities and operators | OR07, OR08 | Tenancy and config | Admissions, Staff and HR, Engagement (copy `operating_entity_id`), Governance | Effective-dated link; copy on create |
| Settings | OR09 | Tenancy and config holds values; each module defines its keys | All modules | Resolved value with source node (AP-02) |
| Legacy campus lists | E06 in MyGIIS and brand SIS | Legacy until cutover (DL-45) | Migration | Mapping by `campus_id`, never by name |

## Change event candidates

| Event | Entity | Payload | Consumers | Risk policy |
|---|---|---|---|---|
| ScopeNodeOpened | OR01 | scope_node_id, node_type, parent | Access Registry, search | — |
| ScopeNodeClosed | OR01 | scope_node_id, valid_to | Access Registry (end grants), Admissions | Ends grants on the node; records kept |
| CampusOperatorChanged | OR08 | campus_id, old and new legal_entity_id, valid_from | Admissions, HR, Engagement, Governance | New records copy the new entity; old ones unchanged (OQ-04) |
| SettingChanged | OR09 | scope_node_id, setting_key, valid_from | Modules using the key | Effective-dated; no personal data |

## Capabilities

`ga.catalog.manage` (Seed) covers the campus catalogue. Two **New** capabilities are proposed for the [permission catalogue](../04-access/permission-catalogue.md): `org.campus_operator.manage` (OR07, OR08) and `org.setting.manage` (OR09, limited by `override_policy`).

## Readiness

Design can start on scope switcher and breadcrumb (AP-01), the campus catalogue (SCR-GA-01, SCR-GA-02) and inherited settings (AP-02). Show operating entity as "[operating entity — OQ-02]", campus codes as placeholders (OQ-55) and data locations beyond Singapore and India as Planned (OQ-21). Don't design a region level (OQ-59) or name any controller (OQ-06).

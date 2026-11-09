# Tenancy and scope hierarchy

**Status:** Confirmed (DL-40, DL-41, Henry, 7 Nov 2026); settings ownership still open, 7 Nov 2026 · **Owner:** Henry
**Cites:** [Organisation dictionary](./organisation-data-dictionary.md) (entities OR01–OR09) · [Architecture v1 § Multi-campus and tenancy model](../00-sources/Global_Campus_OS_Product_Architecture_v1.md#multi-campus-and-tenancy-model) · [architecture.md](./architecture.md) · DL-17, DL-35, DL-40, DL-41 · RES-F06 · MC-01, MC-02, MC-03, MC-14 · research §08
**Tóm tắt (VI):** Cây phạm vi Group › Brand › Brand-country › Campus › Segment / Programme offering (DL-40). Pháp nhân nằm cạnh cây, gắn với campus theo thời gian (DL-41); danh sách pháp nhân vẫn là câu hỏi mở (OQ-02).

## The hierarchy product truth defines

| Level | Examples (GIIS) | Owns and can override | Status |
|---|---|---|---|
| Group | GSG | Programme pack masters, group policies, module catalogue, group analytics | Proposed (Architecture) |
| Brand | GIIS, OWIS | Brand identity, brand policy templates, brand-wide roles | Confirmed (DL-40) |
| Brand-country | GIIS Singapore, GIIS India | Data cell (by country), regulator pack, currency and tax, public holidays, refund and fee protection rules, languages offered | Confirmed (DL-40); contents from Architecture "Country" |
| Campus | SMART Campus Punggol, East Coast, Noida | Calendar, fee plans, buildings and rooms, transport routes, contacts and escalation owners | Proposed |
| Segment (Architecture: "Section", DL-29) | Pre-Primary, Primary, Middle, Higher | School day timings, assembly days, segment events | Proposed |
| Programme offering | CBSE 6–8 at Punggol, IGCSE 9–10 at East Coast | Subjects, assessment calendar, grading scale version, promotion rule version | Proposed |

Rules from Architecture v1: settings inherit downward; every row carries `tenant_path` (group/country/campus); policies, fee plans and grading scales are effective-dated; Singapore and Indian data stay in their own country cells; a thin global layer holds identity, directory and anonymised group analytics; parents link to children on any campus or country.

## Access grants on the hierarchy

Grants and memberships are made at a scope node (Group, Brand, Brand-country or Campus) and cover every node below it (DL-17, DL-35, DL-40). Details: [`../04-access/rbac-model.md`](../04-access/rbac-model.md#layer-1--hierarchical-grant-ceiling).

## What the hierarchy does not hold yet

Brand and legal entity were answered on 7 Nov 2026; the rest are known to vary across the group but have no place in the hierarchy. Product truth does not choose where they go.

| Dimension | What is known | Why it matters | Open question |
|---|---|---|---|
| **Brand** | Answered: a level of the tree (DL-40) | — | — |
| **Legal entity** | Answered: beside the tree; campus operator is an effective-dated link and `operating_entity_id` is copied onto enrolment, employment and cases (DL-41). Which entities exist is still open | Data ownership and access after a campus changes hands | OQ-02, OQ-04 |
| **Group vs school settings** | Vendors such as iSAMS (Central Manage) and Arbor (pushdowns) separate settings the group fixes from those a school may change (MC-14) | Settings screens need to show who may change what | OQ-20 |
| **Countries beyond SG and IN** | Product truth defines two country cells; GSG operates in 11 countries | Number of cells and regulator packs | OQ-21, OQ-22 |

## For design

- Show the current scope in the shell on every staff screen (Architecture; principle 7 in the [Admin Platform brief](../06-experience/admin-platform/README.md#principles-for-designers)).
- Never hard-code one campus's calendar, grading scale, currency or regulator into a screen.
- The scope picker and breadcrumb show Group › Brand › Brand-country › Campus (AP-01, DL-40). Operating entities are not scope levels; show them on the record, never as a picker level (DL-41).

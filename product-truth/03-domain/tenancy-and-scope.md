# Tenancy and scope hierarchy

**Status:** Proposed (Architecture v1) and Assumed (DL-17), with open gaps, 25 Oct 2026 · **Owner:** Henry
**Cites:** [Architecture v1 § Multi-campus and tenancy model](../00-sources/Global_Campus_OS_Product_Architecture_v1.md#multi-campus-and-tenancy-model) · DL-17 · RES-F06 · MC-01, MC-02, MC-03, MC-14 · research §08
**Tóm tắt (VI):** Cấu trúc phân cấp Group › Country › Campus › Section › Programme offering như Architecture v1 mô tả. Thương hiệu (brand) và pháp nhân (legal entity) chưa có chỗ trong phân cấp; đây là câu hỏi mở, không phải thiết kế.

## The hierarchy product truth defines

| Level | Examples (GIIS) | Owns and can override | Status |
|---|---|---|---|
| Group | GSG (Architecture v1 says "GIIS group") | Programme pack masters, brand, core policies, group analytics | Proposed |
| Country | Singapore, India | Data region, regulator pack, currency and tax, public holidays, refund and fee protection rules, languages offered | Proposed |
| Campus | SMART Campus Punggol, East Coast, Noida | Calendar, fee plans, buildings and rooms, transport routes, contacts and escalation owners | Proposed |
| Section | Pre-Primary, Primary, Middle, Higher | School day timings, assembly days, segment events | Proposed |
| Programme offering | CBSE 6–8 at Punggol, IGCSE 9–10 at East Coast | Subjects, assessment calendar, grading scale version, promotion rule version | Proposed |

Rules from Architecture v1: settings inherit downward; every row carries `tenant_path` (group/country/campus); policies, fee plans and grading scales are effective-dated; Singapore and Indian data stay in their own country cells; a thin global layer holds identity, directory and anonymised group analytics; parents link to children on any campus or country.

## Access grants on the hierarchy

Grants are made at a scope node (Group, Country or Campus) and cover every node below it (DL-17, Assumed). Details: [`../04-access/rbac-model.md`](../04-access/rbac-model.md#layer-1--hierarchical-grant-ceiling).

## What the hierarchy does not hold yet

These are known to vary across the group but have no place in the hierarchy. Product truth does not choose where they go.

| Dimension | What is known | Why it matters | Open question |
|---|---|---|---|
| **Brand** | 12 brands with their own identity, curricula, policies and portals, across countries (MC-02). Architecture v1 puts "brand" as something the Group level owns. | Brand-wide settings and brand-wide access can't be expressed; one country can host several brands | OQ-14, OQ-17, OQ-26 |
| **Legal entity** | School operators, employers and franchise or JV partners sit behind campuses and can change over time (MC-03). The Data Dictionary records the employer on employment (E02) and leaves the organisation model open (Decision D06). | Data ownership and access after a campus changes hands | OQ-02, OQ-04 |
| **Group vs school settings** | Vendors such as iSAMS (Central Manage) and Arbor (pushdowns) separate settings the group fixes from those a school may change (MC-14) | Settings screens need to show who may change what | OQ-20 |
| **Countries beyond SG and IN** | Product truth defines two country cells; GSG operates in 11 countries | Number of cells and regulator packs | OQ-21, OQ-22 |

## For design

- Show the current scope in the shell on every staff screen (Architecture; principle 7 in the [Admin Platform brief](../06-experience/admin-platform/README.md#principles-for-designers)).
- Never hard-code one campus's calendar, grading scale, currency or regulator into a screen.
- Don't add a brand or entity level to a scope picker or breadcrumb until OQ-14 / OQ-02 are answered. Where brand matters, keep the element generic and cite the OQ.

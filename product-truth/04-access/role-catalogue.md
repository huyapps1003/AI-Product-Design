# Role catalogue

**Status:** School roles Known (GIIS Handbook via Architecture v1); access roles Assumed (A06 values, DL-02); missing families Open, 25 Oct 2026 · **Owner:** Henry
**Cites:** [Architecture v1 § Users and roles](../00-sources/Global_Campus_OS_Product_Architecture_v1.md#users-and-roles) · [roles-personas § Access role catalog](../02-users/roles-personas.md#access-role-catalog-a06--proposed-values) · RES-F06 · research §03, §08
**Tóm tắt (VI):** Gom mọi tên vai trò đang có trong product truth (vai trò trường học, access role A06, persona) và liệt kê các nhóm vai trò chưa được định nghĩa. Chỉ ghi nhận, không tự tạo vai trò mới.

Three kinds of "role" exist. Don't mix them in a screen.

| Kind | What it is | Grants access? |
|---|---|---|
| **School role** | A job people do, named in the GIIS Handbook (Architecture v1) | No. It is held through grants and memberships |
| **Access role** (A06) | A code used in TM memberships and system grants | Yes, through its permission template |
| **Persona** | A design bundle of the above (P1–P5) | No. Never stored |

## School roles (Architecture v1, GIIS)

| School role | Scope | Access role mapping (Assumed, from roles-personas) | Persona |
|---|---|---|---|
| Group leadership | All countries | — (Leadership views not designed) | — |
| Country Director | Country | `CAMPUS_LEADER` (country-level variant) | P3 variant |
| Finance Controller | Country | `MODULE_ADMIN` when running a module team | P4 example |
| Campus Principal / Vice Principal / Deputy Principal | Campus or section | `CAMPUS_LEADER`, `VIEWER` | P3 |
| Academic Supervisor | Campus | `CAMPUS_LEADER` (read-only variant) | P3 variant |
| Programme or level coordinator | Programme and grade band | `MODULE_ADMIN` when running a module team | P4 example |
| Class teacher | Section | `TEACHER` | P5 |
| Subject teacher | Subject and sections | `TEACHER` | P5 |
| Counsellor (career, behavioural, SEN) | Campus | Not mapped; holds memberships | — |
| Admissions and customer relations | Campus or country | Not mapped; holds memberships | — |
| Transport executive | Campus | `MODULE_ADMIN` when running a module team | P4 example |
| Nurse | Campus | `MODULE_ADMIN` when running a module team (nurse lead); otherwise not mapped; holds memberships | P4 example |
| ECA / CCA in-charge | Campus | `MODULE_ADMIN` when running a module team | P4 example |
| Security / reception | Campus | Not mapped; holds memberships | — |
| Discipline committee | Campus | Record ACL on cases (DL-19) | — |
| Parent / guardian | Own children, across campuses | Family link, not an access role (DL-18) | — |
| Student | Self | Not defined (OQ-19) | — |

"Not mapped" follows [roles-personas § Architecture roles not yet personas](../02-users/roles-personas.md#architecture-roles-not-yet-personas): these people hold memberships, and their access role is set when their module is designed. These roles come from one brand's handbook. Whether other brands use the same roles hasn't been researched.

## School roles named by module dictionaries (since 12 Nov 2026)

The ten module dictionaries name these roles in their "Field visibility by role" tables and rules. This list only records them; none has an access-role mapping yet, so they hold memberships like the "Not mapped" roles above (cross-module review, 28 Nov 2026).

| Role as named | Scope | Named in | Status | Same as |
|---|---|---|---|---|
| Brand-country academic head | Brand-country | Academic structure (approves published date changes) | Confirmed (DL-113) | — |
| GSG platform team | Group | Academic structure, Access Registry owner | Confirmed (DL-51) | Platform operator family (OQ-15) |
| Academic coordinator / Coordinator (programme band) | Programme offering × grade band | Academic structure, Assessment, Attendance, Classes, Teaching & Timetable, Staff & HR | Known (Arch § Users and roles) | "Programme or level coordinator" above |
| Head of department | Campus × subject | Teaching & Timetable (TE07), Staff & HR (appraisal reviewer example) | Known title (HB p.7–8); confirmed as academic responsibility (DL-135) | — |
| Admissions officer | Campus | Admissions | Assumed (admissions dictionary) | "Admissions and customer relations" above |
| Admissions manager | Brand-country | Admissions | Assumed (admissions dictionary) | — |
| Assessor | Assigned applications | Admissions (AD05) | Assumed | Record-level, not a standing role |
| Admissions / registrar (Tier 3 team) | Campus | Attendance (ICA cases), Student Directory | Known need (DL-105) | — |
| Finance executive | Campus | Fees & Billing | Assumed (fees dictionary) | — |
| HR (brand-country), e.g. HR Business Partner | Brand-country | Staff & HR | Confirmed scope (DL-151) | — |
| Campus office | Campus | Teaching & Timetable, Cases & Communications (office queue) | Assumed | Overlaps "Security / reception" |
| Line manager, appraisal reviewer | Relationship | Staff & HR | Confirmed (DL-150) | Not a role: a reporting line (glossary) |
| Case assignee, participants | Record | Cases & Communications | Confirmed (DL-120, DL-125) | Record ACL (A15) |

Which access role (A06) and position template (OQ-192) each of these maps to belongs to the Admin Platform spec.

## Access roles (A06, Assumed values)

`GLOBAL_ADMIN` · `PLATFORM_AUDITOR` · `HR_ADMIN` · `HR_VIEWER` · `MODULE_ADMIN` · `CAMPUS_LEADER` · `TEACHER` · `MEMBER` · `VIEWER`. Definitions and the persona × capability matrix are in [roles-personas.md](../02-users/roles-personas.md#access-role-catalog-a06--proposed-values). Self-service (My profile, My access) comes with an active account and is not a role.

Proposed new values (admin platform dictionary, Assumed): `BRAND_ADMIN` at a Brand node (DL-178; who holds it OQ-198), `DATA_PROTECTION_LEAD` at a Brand-country node (DL-176; OQ-201), `PLATFORM_SUPPORT` for GSG support engineers, usable only through approved operator sessions (DL-175; OQ-15). Where each role may be assigned and whether it needs a second approver are in the [admin platform dictionary § A06](../03-domain/admin-platform-data-dictionary.md#a06--access-role).

## Role families with no definition

People in these groups exist (research §03), but product truth has no school role, access role or persona for them. Don't invent one in a design; cite the open question.

| Family | Who | Evidence | Open |
|---|---|---|---|
| Platform operator | GSG platform admins, support, security | Henry: GSG runs Campus OS; PowerSchool support-portal breach | OQ-15 |
| Brand leadership | Brand heads, brand academic leads | 12 brands; acquired schools keep their identity | OQ-14, OQ-16 |
| Data protection | Group and country DPOs | DPO named among central functions; DPDP and PDPA duties | OQ-34 |
| Partner staff | Franchise and JV operators' staff | GIIS campuses opened through local partners | OQ-19 |
| Students | Students as users beyond "self" | Architecture names the Student app | OQ-19 |

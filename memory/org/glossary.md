---
name: glossary
description: Canonical GCOS terms, ID prefixes, concepts that must never be merged, and deprecated words to replace. Open before writing any UI copy, label or spec.
layer: org
write: human-only
sources:
  - product-truth/01-roles-personas.md (Four things that are easy to confuse)
  - product-truth/03-ia-module-map.md (Module catalog R01)
  - product-truth/05-rules-to-ui.md (Reusable UI patterns, Status vocabulary)
  - product-truth/gap-analysis-2026-10-18.md (G3, G8, G13)
  - product-truth/06-decision-log.md (DL-04, DL-11, DL-12)
reviewed: 2026-10-21
---

# Glossary

**Tóm tắt (VI):** Ba phần: (1) bốn khái niệm **không bao giờ được gộp**, (2) bảng tiền tố ID để agent không đọc nhầm, (3) từ cũ phải thay bằng từ chuẩn. Phần (3) là danh sách dreaming dùng để bắt drift.

## 1. Never merge these

| Concept | Lives in | Grants access? |
|---|---|---|
| **HR position** — "Subject Teacher", "Principal" | E05 via E04 | No |
| **Campus affiliation** — where someone works | E07 | No (BR-06) |
| **Academic role** — class teacher of 7B, teaches Maths 6–8 | entity D01/D02 (→ X01/X02, DL-12) | No; only feeds scope *suggestions* (DL-16) |
| **Access** — GA grant → TM membership → role → template → data scope | A03/A04 → A05 → A07 → A08/A09 → A12/A13 | **Yes** |

A **persona** is a design idea (a typical bundle of the four). The system never stores "persona".

Copy test: a label that says a position, campus or academic role *gives* access is wrong. "Priya works at SG-ECS" ≠ "Priya has access at SG-ECS".

## 2. Core terms

| Term | Meaning | Not |
|---|---|---|
| **GA (Global Admin) grant** | Outer ceiling: which modules × campuses a user may ever be given | Not usable access by itself |
| **GA ceiling** | Intersection of a user's GA module and campus grants | — |
| **TM (Team Management) membership** | Usable access in one module at one campus | Not possible without a GA grant (BR-15) |
| **Access role** | Catalog entry (A06), e.g. `HR_ADMIN`, `MODULE_ADMIN`, `TEACHER` | Not an HR position |
| **Permission template** | Versioned set of capability allow/deny (A08/A09). "Published v{n}" in UI | — |
| **Data scope** | Which records inside a module: campus, section, grade, subject, org unit | — |
| **Effective permission** | Final result: ALLOWED, DENIED or NO SCOPE. Deny wins, no scope = no data (DL-07) | — |
| **Override** | Per-member exception; off by default (DL-14) | — |
| **Case** | Shared object for access requests and access reviews (DL-08) | Not a ticket or task |
| **Access review** | Case opened by transfer, exit or periodic attestation | — |
| **Attest / attestation** | Campus Leader confirms who has access, per term | — |
| **End / Revoke** | Dated ending of a record (P-HISTORY) | Never "Delete" for employment, assignment or grant |
| **Scope switcher** | Shell control showing Country › Campus | — |
| **Sensitivity tier** | Tier 1 = Internal, Tier 2 = Confidential, Tier 3 = Restricted (DL-11) | Use the words in UI, not tier numbers |

## 3. ID prefixes

| Prefix | Means | Source |
|---|---|---|
| `E01`–`E08` | Employee entities (E01 Employee, E02 Employment, E06 Campus, E07 Campus assignment…) | Data Dictionary |
| `A01`–`A14` | Access entities (A01 User account, A03/A04 GA grants, A05 TM membership…) | Data Dictionary |
| `R01`, `R02` | Reference catalogs (R01 Module catalog) | Data Dictionary |
| **Decision D01–D15** | Open data decisions | Data Dictionary |
| **entity D01–D03** | Academic entities — collides with Decision D01–D03. Always write "Decision D0x" or "entity D0x"; proposed rename X01–X03 (DL-12) | Data Dictionary |
| `BR-01`–`BR-21` | Business rules | Data Dictionary, `05` |
| `DL-01`… | Design decision log entries | `06` |
| `SCR-…` | Screen IDs, e.g. SCR-HR-01 | `03` |
| `S1`–`S5` | Key scenarios (S1 Onboard teacher, S2 Campus transfer, S3 Employee exit, S4 Module team setup, S5 Access review) | `02`, `04-flows/` |
| `P-…` | Reusable UI patterns (P-BLOCK, P-WARN, P-LOCK, P-NOACCESS…) | `05` |
| `P0`–`P2` | Gap severity | gap analysis |
| `G1`… | Gap IDs | gap analysis |
| `GP-n`, `WF-n` | Org principles | `memory/org/principles.md` |

## 4. Module names

Use the R01 names exactly (`03-ia-module-map.md#module-catalog-r01`): Admissions and Enrolment · Student Records · Teaching and Learning · Assessment and Reporting · Attendance and Conduct · Wellbeing and Inclusion · Fees and Finance · Campus Services · Communication and Cases · Activities and Awards · Careers and Universities · Staff and HR · Global Admin · Governance.

Write "and", not "&" ("Staff and HR", not "Staff & HR").

## 5. Deprecated → canonical

Found in older mocks (`screens/*.html`) and early prompts. Treat any occurrence in new output as drift.

| Don't write | Write |
|---|---|
| Employee Management (module) | Staff and HR (`staff_hr`) |
| Academic Allocation (module) | Teaching and Learning, capabilities `allocation.*` |
| Timetable (module) | Teaching and Learning, capabilities `timetable.*` |
| Hong Kong, Kuala Lumpur, Jakarta campuses | SG-PGL, SG-ECS, IN-NOI, GRP-HQ only |
| Global HQ | Group Office (`GRP-HQ`) |
| `globalcampus.edu` | `giis.example` |
| "Global Campus Hong Kong Limited" and similar entities | "GIIS Singapore (employer)", "GIIS India (employer)" |
| Microsoft Azure AD | Google Workspace (SSO, MFA) |
| Tier 2 / Tier 3 in UI copy | Confidential / Restricted |
| Delete (employment, assignment, grant) | End… / Revoke… |
| "Persona" as a stored field | Grants, memberships, roles, scope |

---
name: product-vision
description: What Global Campus OS is, who it serves, what v1 designs (Admin Platform) and what it must not design. Open first on any design task.
layer: org
write: human-only
sources:
  - product-truth/00-product-brief.md
  - product-truth/Global_Campus_OS_Product_Architecture_v1.md
  - product-truth/06-decision-log.md (DL-01, DL-04, DL-05, DL-06)
reviewed: 2026-10-21
---

# Product vision

**Tóm tắt (VI):** Global Campus OS là hệ điều hành vận hành cho chuỗi trường GIIS nhiều quốc gia. Đợt thiết kế v1 chỉ làm **Admin Platform**: nhân sự và quyền truy cập, là "cửa ngõ" cho 12 module. Không có người và quyền đúng thì không module nào chạy được.

## In one sentence

One place where HR knows every employee, Global Admin sets the outer boundary of access, each module's admin runs their team inside that boundary, and campus leaders can see and attest who has access to their campus — with every change explained and audited.

> Campus affiliation is a fact about employment. Access is a decision. The platform never confuses the two.

## The problem v1 solves

1. **Scattered identity** — no single employee record survives a contract change, a second campus or a rehire.
2. **Access drift** — access is granted by hand; transfers keep old access, exits leave accounts alive.
3. **No answer to "who can see what, and why?"** for auditors, Principals or regulators (EduTrust, PDPA, DPDP).

## Shape of the platform

- 4 apps (Parent, Student, Staff web, Leadership) → 12 campus modules → shared core services.
- Admin Platform is **not a separate product**: it is a set of sections inside the Staff web app, visible when the user holds the matching roles (DL-05).
- Tenancy: Group › Country › Campus › Section › Programme offering.

## v1 design scope

| In | Out (non-goals) |
|---|---|
| Staff and HR (employee master, employment, org, positions, campus assignments) | Parent, Student and Leadership apps |
| Global Admin console (campuses, accounts, grants, catalogs, templates) | The 11 other modules' own screens — they appear only as switcher entries and grant targets |
| Team Management inside each module (members, roles, templates, data scope) | Payroll, leave, appraisal, training, recruitment |
| Governance (access requests, reviews, audit log, reports) | Editing academic allocations or timetables |
| Campus Leader view, Teacher self-service | Auth UI beyond SSO sign-in and the no-access page |

Devices: desktop-first for admins, mobile-first for self-service (DL-06).

## The five personas (detail: `product-truth/01-roles-personas.md`)

| Persona | Example person | Core job |
|---|---|---|
| HR Admin | Meera Iyer (GRP-HQ) | One correct, current record per employee |
| Global Admin | Daniel Tan (GRP-HQ) | Outer boundary of access is right |
| Campus Leader | Dr Anita Rao (Principal, SG-PGL) | Sees and attests access on own campus |
| Module Admin | Kenji Lim (Attendance and Conduct, SG-PGL) | Runs the module team inside the GA ceiling |
| Teacher | Priya Nair (SG-PGL + SG-ECS) | Sees own access, requests more |

## How we know it works

| Metric | Target |
|---|---|
| Employee created → first correct login | ≤ 1 working day |
| Access changes with audit event and actor | 100% |
| Silent grants on transfer | 0 |
| Access removed after employment ends | ≤ 24 h |
| "Why does this person have this permission?" | Answered from one screen |
| Every v1 screen traces to a scenario and a rule | Yes |

## Canonical sample data

Always use the set in `product-truth/00-product-brief.md#canonical-sample-data` (GIIS; SG-PGL, SG-ECS, IN-NOI, GRP-HQ; Google Workspace SSO; `@giis.example`; AY 2026-27). Never invent campuses, people or domains.

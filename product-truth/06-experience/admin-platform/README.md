# 00 — Product brief: Global Campus OS Admin Platform

**Status:** Proposed, 18 Oct 2026 · **Owner:** Henry
**Reads with:** [Product Architecture v1](../../00-sources/Global_Campus_OS_Product_Architecture_v1.md) (whole platform) · [Employee Data Dictionary v1](../../00-sources/Global_Campus_OS_Employee_Data_Dictionary_v1.md) (data)

**Tóm tắt (VI):** Thiết kế đợt đầu tập trung vào **Admin Platform**: quản lý nhân sự (Staff and HR) và quyền truy cập (Global Admin + Team Management) cho mọi campus GIIS. Đây là "cửa ngõ" của 12 module: chưa có người và quyền đúng thì không module nào chạy được.

## Problem

GIIS runs one brand across countries with different employers, calendars and regulators (Singapore: SMART Campus Punggol and East Coast; India: Noida). Staff move between campuses, teach across programmes (CBSE, IB, IGCSE, CLSP, GMP) and need access to several modules at once. Today:

- **Employee identity is scattered.** There is no single employee record that survives a contract change, a second campus or a rehire (Data Dictionary BR-01, BR-02).
- **Access is granted by hand and drifts.** Campus affiliation, HR position and system access get conflated, so a transfer silently keeps old access and an exit leaves accounts alive (BR-06, BR-10).
- **Nobody can answer "who can see what, and why?"** for an auditor, a Principal or a regulator (EduTrust, PDPA, DPDP), because grants, roles, templates and data scope live in different places.

## Vision

One place where HR knows every employee, Global Admin sets the outer boundary of access, each module's admin runs their team inside that boundary, and campus leaders can see and attest who has access to their campus, with every change explained and audited.

> "Campus affiliation is a fact about employment. Access is a decision. The platform never confuses the two."

## Scope of this design slice (v1)

| In scope | Notes |
|---|---|
| **Staff and HR** (module 12) | Employee master, employment, org assignment, job positions, campus assignments, org structure (E01–E07) |
| **Global Admin console** | Campus catalog, user accounts, GA module and campus grants, module, capability and role catalogs, permission templates (A01–A04, A06, A08, A09, R01, R02, E06) |
| **Team Management** inside each module | Members, role assignments, template binding, data scope, overrides if enabled (A05, A07, A10–A13) |
| **Governance** | Access requests, access reviews (transfer, exit, periodic attestation), audit log, access reports (A14) |
| **Campus Leader view** | Campus workforce and access overview, pending approvals |
| **Teacher self-service** | My profile, my campuses, my access, request access |
| Read-only references | Academic role and subject allocations (entity D01/D02) and timetable sessions (entity D03) shown in the employee profile |

## Non-goals (v1)

- Parent app, Student app and Leadership views ([Architecture § Apps](../../00-sources/Global_Campus_OS_Product_Architecture_v1.md#domains-and-modules)).
- Designing the 11 other campus modules' own screens. They appear only as entries in the module switcher and as targets of grants.
- Payroll, leave balances, appraisal, training and recruitment (Staff and HR "cover, training, appraisal" is a later slice).
- Editing academic allocations or timetables (owned by Teaching and Learning, BR-09).
- Authentication UI beyond SSO sign-in and the no-access page; credentials stay with the IdP.
- Physical schema freeze (blocked on Decisions D01–D10).

## Success metrics

| Metric | Target | How measured |
|---|---|---|
| New teacher from "employee created" to first successful login with correct access | ≤ 1 working day | EmploymentStarted → first `last_login_at` with an active TM membership |
| Access changes with an audit event and actor | 100% | A14 coverage of GA/TM change events |
| Silent access grants on transfer or affiliation change | 0 | CampusAffiliationChanged without a linked review case |
| Access removed after employment ends | ≤ 24 h from `end_date` | EmploymentEnded → all grants REVOKED/EXPIRED |
| Access review (attestation) completion per campus per term | ≥ 95% | Review cases closed vs opened |
| "Why does this person have this permission?" answered from one screen | Yes, for any user | Effective Permissions panel shows source of every allow/deny |
| Design traceability | Every v1 screen maps to a scenario and a rule | `03-ia-module-map.md` ↔ `02`, `05` |

## Principles for designers

1. **Show the boundary.** Whenever a TM action happens, show the GA ceiling it sits inside (BR-07).
2. **Explain every permission.** Any allow or deny shows where it came from: grant, role, template version, scope, override.
3. **Never grant by side effect.** Transfers, rehires and campus changes create a review, never access (BR-06, Decision D07).
4. **Fail closed, visibly.** Missing scope or an expired grant shows a clear "no access" state, not an empty table that looks like "no data" (BR-17).
5. **History is never overwritten.** Ending something is an action with a date, not a delete (BR-03).
6. **Restricted looks restricted.** Confidential and Restricted fields carry a lock treatment and are masked by default for roles without the right.
7. **Campus is always in view.** The current scope (Country › Campus) is visible in the shell on every staff screen.

## Canonical sample data

Use these in every design so screens agree with each other and with the architecture. All people are synthetic.

| Kind | Values |
|---|---|
| Group | GIIS (Global Indian International School) |
| Countries | Singapore (SG, SGD, Asia/Singapore) · India (IN, INR, Asia/Kolkata) |
| Campuses (E06) | `SG-PGL` SMART Campus Punggol · `SG-ECS` East Coast Campus · `IN-NOI` Noida Campus · `GRP-HQ` Group Office (no students; for group-level staff; assumed) |
| Employers (synthetic) | "GIIS Singapore (employer)" · "GIIS India (employer)" |
| Sections | Pre-Primary (KG), Primary (1–5), Middle (6–8), Higher (9–12) |
| Programmes | GMP (KG), CBSE 1–12, IB PYP, Cambridge CLSP 6–8, IGCSE 9–10, IBDP |
| Org units | Mathematics Dept, Science Dept, Languages Dept, IB PYP Team, Admissions, Finance, Transport, Human Resources, IT |
| Positions (E05) | Class Teacher, Subject Teacher, Programme Coordinator, Academic Supervisor, Vice Principal, Principal, HR Executive, Finance Controller, Transport Executive, Nurse, Counsellor |
| IdP | Google Workspace (staff SSO, MFA) |
| Email pattern | `firstname.lastname@giis.example` (synthetic domain) |
| Academic year | AY 2026-27 (Singapore: two semesters; Noida: from April) |
| Example people | Meera Iyer (Global HR Admin, GRP-HQ) · Daniel Tan (Global Admin, GRP-HQ) · Dr Anita Rao (Principal, SG-PGL) · Kenji Lim (Module Admin, Attendance and Conduct, SG-PGL) · Priya Nair (CBSE 6–8 Maths teacher, SG-PGL + SG-ECS) · Rahul Verma (Class Teacher, IN-NOI) |
| Realistic numbers | ~420 staff total; SG-PGL 210, SG-ECS 95, IN-NOI 100, GRP-HQ 15 (illustrative only) |

## Out-of-slice dependencies

- Decisions D01–D10 (Data Dictionary) remain open; this slice designs against defaults in [`06-decision-log.md`](../../07-decisions/decision-log.md).
- Calendar engine, rules engine and Cases-and-SLAs core services (Architecture) are assumed to exist; Access Review and Access Request reuse the shared **Case** object.

## File map

| File | Answers |
|---|---|
| [01-roles-personas.md](../../02-users/roles-personas.md) | Who uses it and what access each persona actually holds |
| [02-key-scenarios.md](./scenarios.md) | The 5 scenarios v1 must support end to end |
| [03-ia-module-map.md](./ia-screen-map.md) | Shell, navigation, module catalog, screen list |
| [04-flows/](./flows/) | Flow + state machine + screen states per scenario |
| [05-rules-to-ui.md](./rules-to-ui.md) | BR-01…BR-21 and Decisions D01…D15 → UI behaviour |
| [06-decision-log.md](../../07-decisions/decision-log.md) | Assumptions chosen and why |
| [07-rbac-model.md](../../04-access/rbac-model.md) | Effective RBAC model: six layers, evaluation algorithm, v1 scope |
| [gap-analysis-2026-10-18.md](../../_archive/gap-analysis-2026-10-18.md) | Why this folder exists: the review of v1 product truth |

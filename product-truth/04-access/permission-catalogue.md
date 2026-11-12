# Permission catalogue

**Status:** Canonical (DL-33, Henry, 7 Nov 2026); seed v0.1 · **Owner:** Henry (registry owner: GSG platform team, DL-51)
**Cites:** DL-32…DL-38 · [roles-personas § capability matrix](../02-users/roles-personas.md#persona--capability-matrix-admin-slice) · [student dictionary § Field visibility by role](../03-domain/student-directory-data-dictionary.md#field-visibility-by-role) · [rules-to-ui § Field visibility](../06-experience/admin-platform/rules-to-ui.md#field-visibility-decision-d13) · Employee DD R01, R02, A06, A08, A09
**Tóm tắt (VI):** Danh mục quyền chuẩn duy nhất: capability, chính sách trường (field policy) và thành phần template. Các ma trận trong roles-personas, rules-to-ui và từ điển học sinh là "góc nhìn" của file này.

This file is the single product-truth source for what can be permitted (DL-33). In the product it is held by the Access Registry (DL-32). Other matrices are views of it: when they disagree, this file wins and the view is corrected.

## How access is decided

Staff path (DL-07, updated by DL-35…DL-38): cell → grant ceiling on a scope node → membership on a scope node → capabilities from bound templates (deny wins) → data scope (no scope, no data) → delegation (A16) if acting for someone → record ACL (A15) or Tier 3 type grant (A17) for restricted records → allowed, with the deciding layer and source record. Parent path (DL-37): family link → record belongs to the child → capability in R03. Either path allowing is enough.

## Capabilities

Codes follow `module.object.action` (DL-53; new capabilities reviewed by the registry owner). **Seed** = taken from an existing matrix; **New** = introduced by an object card for DL-34 and needing the module owner's review.

| Code | Action type (R02) | Object | Status |
|---|---|---|---|
| `hr.employee.view` | VIEW | OBJ-05 | Seed (roles-personas) |
| `hr.employee.create` / `hr.employee.update` | CREATE / UPDATE | OBJ-05 | Seed |
| `hr.employment.end` | UPDATE | OBJ-06 | Seed |
| `hr.campus_assignment.manage` | MANAGE | OBJ-05 | Seed |
| `hr.confidential.view` | VIEW | OBJ-05, OBJ-06 | Seed |
| `ga.module_grant.manage` / `ga.campus_grant.manage` | MANAGE | OBJ-07 | Seed |
| `ga.catalog.manage` | MANAGE | R01, R02, A06, E06 | Seed |
| `org.campus_operator.manage` | MANAGE | OBJ-14 | New (organisation dictionary; owner OQ-58) |
| `org.setting.manage` | MANAGE | OBJ-13 | New (organisation dictionary; overridable settings OQ-20) |
| `ga.template.publish` | APPROVE | A08 | Seed |
| `tm.membership.manage` | MANAGE | OBJ-08 | Seed |
| `tm.scope.manage` | MANAGE | OBJ-08 | Seed |
| `gov.access_review.decide` | APPROVE | OBJ-10 | Seed |
| `gov.audit.view` | VIEW | OBJ-11 | Seed |
| `gov.report.export` | EXPORT | OBJ-11 | Seed |
| `gov.case.view_all` | VIEW | OBJ-10 | Seed (rbac-model Layer 5) |
| `gov.audit.view_snapshot` | VIEW | OBJ-11 | New |
| `identity.person.view`, `identity.person.merge` | VIEW, MANAGE | OBJ-01 | New |
| `identity.account.view`, `identity.account.suspend` | VIEW, UPDATE | OBJ-01 | New |
| `student.profile.view`, `student.profile.view_confidential`, `student.profile.edit` | VIEW, UPDATE | OBJ-02 | New |
| `student.address.view` | VIEW | OBJ-02 | New |
| `student.identity.reveal` | VIEW | OBJ-02 (T3) | New |
| `student.attendance_threshold.view` | VIEW | OBJ-02 | New (DL-30) |
| `student.enrolment.view` | VIEW | OBJ-03 | New |
| `admissions.enrolment.create`, `.end`, `.transfer` | CREATE, UPDATE | OBJ-03 | New |
| `student.guardian.view`, `student.guardian_contact.view` | VIEW | OBJ-04 | New |
| `student.guardian.verify`, `student.guardian.edit` | APPROVE, UPDATE | OBJ-04 | New |
| `teaching.allocation.view`, `teaching.allocation.manage` | VIEW, MANAGE | OBJ-12 | New |

## Field policies

Each field group has a tier and the capability that views it (DL-34). Edit rights come from the capability in the object card's `edit`. T3 always also needs A15 or A17.

| Field group | Object | Tier | View capability | T3 also needs |
|---|---|---|---|---|
| Name, preferred name, Student ID, class, grade, house, programme | OBJ-02 | T1 | `student.profile.view` | — |
| Photo, date of birth | OBJ-02 | T2 | `student.profile.view_confidential` | — |
| Addresses | OBJ-02 | T2 | `student.address.view` | — |
| Identity documents, nationality, immigration status | OBJ-02 | T3 | `student.identity.reveal` | A17 or A15; read logged |
| Attendance threshold | OBJ-02 | T2 | `student.attendance_threshold.view` | — |
| Enrolment status, dates, end reason, transfer history | OBJ-03 | T2 | `student.enrolment.view` | — |
| Guardian names, relationship, can-collect, emergency flag | OBJ-04 | T2 | `student.guardian.view` | — |
| Guardian phone and email | OBJ-04 | T2 | `student.guardian_contact.view` | — |
| Employee name, work contact, position, campus | OBJ-05 | T1 | `hr.employee.view` | — |
| Legal names, photo, termination reason | OBJ-05, OBJ-06 | T2 | `hr.confidential.view` | — |
| Audit before and after snapshots | OBJ-11 | T3 | `gov.audit.view_snapshot` | — |

## Template contents

Which capabilities each access role's templates hold is a view of the [persona × capability matrix](../02-users/roles-personas.md#persona--capability-matrix-admin-slice) for the admin slice. Templates for the new student capabilities are written when the Admissions and Student Records slice is designed; until then the student field matrix shows which school roles are expected to hold them (marked pending, BR-06).

## Fixed capability sets

| Set | Holds | Source |
|---|---|---|
| R03 Parent | Read own child's attendance, grades, communication and own cases; edit profile, documents, addresses and contacts | DL-18, DL-28 |
| R05 Student | Pending | OQ-19 |

## Open

OQ-47 (A17 approver) · answered: OQ-46 → DL-51, OQ-48 → DL-52, OQ-49 → DL-53 · OQ-15, OQ-16, OQ-34 (operator, brand, DPO roles).

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
| `hr.restricted.view` | VIEW | OBJ-05, OBJ-28, OBJ-29 | New (staff & HR dictionary, DL-151; Tier 3 identity, work pass, checks, medical certificates; always with A17, OQ-173) |
| `hr.self.view`, `hr.self.update` | VIEW, UPDATE | OBJ-05 | New (DL-151; own record; contact and emergency contacts edited directly) |
| `hr.org.manage`, `hr.contract.manage`, `hr.checklist.manage` | MANAGE, MANAGE, MANAGE | OBJ-06 | New (DL-150, DL-152, DL-160) |
| `hr.leave.request`, `hr.leave.approve`, `hr.leave.manage`, `hr.leave_policy.manage`, `hr.leave_template.manage` | CREATE, APPROVE, MANAGE, MANAGE, MANAGE | OBJ-28 | New (DL-153…DL-156; leave templates GSG HR only, DL-153; line manager approves, Principal for long or term-time leave) |
| `hr.compliance.view`, `hr.compliance.manage`, `hr.compliance.waive`, `hr.compliance_rule.manage` | VIEW, MANAGE, APPROVE, MANAGE | OBJ-29 | New (DL-157, DL-158; view = status chip only; Principal waives) |
| `hr.vacancy.request`, `hr.vacancy.approve`, `hr.recruitment.manage`, `hr.interview.feedback`, `hr.offer.approve` | CREATE, APPROVE, MANAGE, CREATE, APPROVE | OBJ-30 | New (DL-159; approvers OQ-174) |
| `hr.appraisal.manage`, `hr.appraisal.review`, `hr.appraisal.moderate`, `hr.training.request`, `hr.training.approve` | MANAGE, UPDATE, APPROVE, CREATE, APPROVE | OBJ-31 | New (DL-161, DL-162) |
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
| `identity.account.invite` | CREATE | OBJ-01 | New (admin platform dictionary, DL-170) |
| `identity.idp.manage` | MANAGE | OBJ-32 | New (DL-169; Global Admin, Brand Admin for own brand) |
| `ga.module_switch.manage` | MANAGE | OBJ-32 | New (DL-166) |
| `ga.setup.manage`, `ga.setup.approve` | MANAGE, APPROVE | OBJ-32 | New (DL-167; approver = brand-country head, OQ-196 answered) |
| `ga.authority.manage` | MANAGE | OBJ-32 | New (DL-168) |
| `ga.setting_definition.manage` | MANAGE | OBJ-13 | New (DL-177; Global Admin) |
| `ga.delegation.manage` | MANAGE | OBJ-35 | New (DL-36, DL-21 held) |
| `ga.type_grant.request`, `ga.type_grant.approve` | CREATE, APPROVE | OBJ-35 | New (DL-69; Campus Leader approves, OQ-205) |
| `ga.operator_session.request`, `ga.operator_session.approve`, `ga.operator_session.break_glass` | CREATE, APPROVE, MANAGE | OBJ-35 | New (DL-52, DL-175; break-glass GSG security, OQ-207) |
| `gov.review_cycle.manage` | MANAGE | OBJ-36 | New (DL-174) |
| `gov.purpose.manage`, `gov.retention.manage`, `gov.notice.manage` | MANAGE | OBJ-34 | New (DL-176; retention activated by the country's Data Protection Lead) |
| `student.profile.view`, `student.profile.view_confidential`, `student.profile.edit` | VIEW, UPDATE | OBJ-02 | New |
| `student.address.view` | VIEW | OBJ-02 | New |
| `student.identity.reveal` | VIEW | OBJ-02 (T3) | New |
| `student.attendance_threshold.view` | VIEW | OBJ-02 | New (DL-30) |
| `student.enrolment.view` | VIEW | OBJ-03 | New |
| `admissions.enrolment.create`, `.end`, `.transfer` | CREATE, UPDATE | OBJ-03 | New |
| `admissions.enquiry.view`, `.create`, `.assign` | VIEW, CREATE, UPDATE | OBJ-16 | New (admissions dictionary) |
| `admissions.application.view`, `.edit`, `.decide` | VIEW, UPDATE, APPROVE | OBJ-16 | New |
| `admissions.document.review` | VIEW, UPDATE | OBJ-16 (T3) | New |
| `admissions.assessment.record` | UPDATE | OBJ-16 | New |
| `admissions.capacity.manage` | MANAGE | OBJ-16 | New (DL-61) |
| `admissions.offer.issue`, `.rescind` | CREATE, UPDATE | OBJ-16 | New |
| `admissions.contract.issue`, `.void` | CREATE, UPDATE | OBJ-16 | New |
| `admissions.approval_case.manage` | MANAGE | OBJ-16, OBJ-10 (T3) | New |
| `classes.class.view`, `classes.class.manage` | VIEW, MANAGE | OBJ-15 | New (classes dictionary) |
| `classes.roster.view` | VIEW | OBJ-15, OBJ-02 | New; teachers scoped to own classes |
| `classes.plan.edit`, `classes.plan.approve` | UPDATE, APPROVE | OBJ-15 (feeds OBJ-16 seat limit) | New (DL-71) |
| `classes.staff.assign` | MANAGE | OBJ-15 | New (DL-73) |
| `classes.placement.manage`, `classes.placement.approve_over_limit` | UPDATE, APPROVE | OBJ-15, OBJ-03 | New (DL-72; who: OQ-73) |
| `classes.house.assign` | UPDATE | OBJ-15, OBJ-03 | New (DL-76) |
| `classes.rollover.prepare`, `classes.rollover.approve` | MANAGE, APPROVE | OBJ-15, OBJ-03 | New (DL-75) |
| `assessment.template.manage` | MANAGE | OBJ-19 | New (DL-87; GSG platform team) |
| `academic.pack.view` | VIEW | OBJ-25 | New (academic structure dictionary, DL-109; every membership, Tier 1) |
| `academic.pack.manage_template` | MANAGE | OBJ-25 | New (DL-110; GSG platform team) |
| `academic.pack.manage`, `academic.pack.approve` | MANAGE, APPROVE | OBJ-25 | New (DL-110, DL-111; brand-country; approver OQ-122) |
| `academic.year_template.manage` | MANAGE | OBJ-25 | New (DL-96; brand-country) |
| `academic.offering.manage`, `academic.offering.approve` | MANAGE, APPROVE | OBJ-25 | New (DL-112; approver OQ-122) |
| `academic.year.manage`, `academic.year.publish`, `academic.year.change_dates` | MANAGE, APPROVE, APPROVE | OBJ-25 | New (DL-113; coordinator prepares, Principal publishes, brand-country academic head approves date changes) |
| `academic.subjects.manage`, `academic.subjects.approve` | MANAGE, APPROVE | OBJ-25 | New (DL-116) |
| `assessment.scheme.edit`, `assessment.scheme.approve` | UPDATE, APPROVE | OBJ-19 | New (DL-87; Academic Supervisor approves, OQ-93) |
| `assessment.calendar.manage` | MANAGE | OBJ-19 | New (Arch § Key workflows: coordinator) |
| `assessment.mark.enter`, `assessment.mark.moderate`, `assessment.mark.lock`, `assessment.mark.correct` | UPDATE, APPROVE, APPROVE, UPDATE | OBJ-19 | New (DL-88; enter scoped to own teaching, OBJ-12) |
| `assessment.mark.approve_proration` | APPROVE | OBJ-19 | New (DL-95; Principal, HB p.43) |
| `assessment.mark.publish` | APPROVE | OBJ-19 | New (DL-93; coordinator publishes locked marks to parents) |
| `assessment.result.view` | VIEW | OBJ-19, OBJ-03 | New (DL-93) |
| `assessment.report_card.generate`, `assessment.report_card.release`, `assessment.report_card.remark` | CREATE, APPROVE, UPDATE | OBJ-19 | New (DL-90) |
| `assessment.appeal.raise`, `assessment.appeal.review`, `assessment.appeal.endorse` | CREATE, UPDATE, APPROVE | OBJ-19 | New (DL-91; raise = parent path) |
| `assessment.promotion.review`, `assessment.promotion.decide` | MANAGE, APPROVE | OBJ-19, OBJ-15 | New (DL-92; Principal decides) |
| `assessment.predicted.enter`, `assessment.predicted.release` | UPDATE, APPROVE | OBJ-19 | New (DL-94) |
| `attendance.calendar.manage` | MANAGE | OBJ-20 | New (attendance dictionary, DL-98; who: OQ-120) |
| `attendance.register.take`, `attendance.register.view` | UPDATE, VIEW | OBJ-20 | New (DL-99; take scoped to own classes, DL-73) |
| `attendance.mark.edit_after_lock`, `attendance.correction.approve` | UPDATE, APPROVE | OBJ-20 | Named in S4; New approval (DL-100; coordinator, OQ-110) |
| `attendance.leave.request`, `attendance.leave.approve` | CREATE, APPROVE | OBJ-20, OBJ-10 | New (DL-101; parent path; class teacher 1–2 days, Principal above) |
| `attendance.certificate.view` | VIEW (Tier 3) | OBJ-20, OBJ-02 | New (DL-101; medical certificate file) |
| `attendance.gate.record`, `attendance.out_pass.approve` | CREATE, APPROVE | OBJ-20 | New (DL-102; reception/security; class teacher or level coordinator approves) |
| `attendance.rule.manage`, `attendance.summary.view` | MANAGE, VIEW | OBJ-20 | New (DL-103, DL-104) |
| `attendance.ica.manage` | MANAGE (Tier 3) | OBJ-20, OBJ-10 | New (DL-105; Admissions/registrar) |
| `cases.type.manage` | MANAGE | OBJ-10 | New (cases & communications dictionary, DL-121; GSG platform team) |
| `cases.policy.manage` | MANAGE | OBJ-10 | New (DL-121; brand-country) |
| `cases.case.view`, `cases.case.create`, `cases.case.work` | VIEW, CREATE, UPDATE | OBJ-10 | New (DL-120; queue and data scope apply) |
| `cases.case.decide` | APPROVE | OBJ-10 | New (DL-120; module approvals such as `attendance.leave.approve` still apply) |
| `cases.case.restrict` | UPDATE | OBJ-10 (T3) | New (DL-125) |
| `cases.feedback.manage`, `cases.report.view` | MANAGE, VIEW | OBJ-10 | New (DL-123; reports are counts only, DL-130) |
| `comms.conversation.use`, `comms.conversation.oversee` | CREATE, VIEW | OBJ-26 | New (DL-127; oversight OQ-142) |
| `comms.announcement.post`, `comms.announcement.approve` | CREATE, APPROVE | OBJ-26 | New (DL-128; approver OQ-138) |
| `comms.template.manage`, `comms.notification_log.view` | MANAGE, VIEW | OBJ-26 | New (DL-129) |
| `fees.catalogue.manage` | MANAGE | OBJ-23 | New (fees & billing dictionary, DL-78) |
| `fees.plan.edit`, `fees.plan.approve` | UPDATE, APPROVE | OBJ-23 | New (DL-78; Finance Controller approves) |
| `fees.account.view`, `fees.account.manage` | VIEW, MANAGE | OBJ-23, OBJ-04 | New (DL-79) |
| `fees.adjustment.request`, `fees.adjustment.approve` | CREATE, APPROVE | OBJ-23 | New (DL-81; approver OQ-83) |
| `fees.run.prepare`, `fees.run.approve` | MANAGE, APPROVE | OBJ-23 | New (DL-80) |
| `fees.invoice.view`, `fees.invoice.issue_one_off`, `fees.invoice.void` | VIEW, CREATE, UPDATE | OBJ-23 | New (DL-80) |
| `fees.credit_note.create`, `fees.credit_note.approve` | CREATE, APPROVE | OBJ-23 | New (DL-80) |
| `fees.payment.record`, `fees.payment.reverse` | CREATE, UPDATE | OBJ-23 | New (DL-82) |
| `fees.status.view` | VIEW | OBJ-23, OBJ-03 | New (DL-83; Admissions, Principal; never teachers) |
| `fees.refund.prepare`, `fees.refund.approve`, `fees.refund.pay` | MANAGE, APPROVE, UPDATE | OBJ-23, OBJ-10 | New (DL-84; `.pay` reveals masked bank details) |
| `fees.fps.manage` | MANAGE | OBJ-23 | New (SG regulator pack) |
| `student.guardian.view`, `student.guardian_contact.view` | VIEW | OBJ-04 | New |
| `student.guardian.verify`, `student.guardian.edit` | APPROVE, UPDATE | OBJ-04 | New |
| `teaching.allocation.view`, `teaching.allocation.manage` | VIEW, MANAGE | OBJ-12 | New (teaching & timetable dictionary, DL-132…DL-134; groups, members, assignments; coordinator within band) |
| `teaching.responsibility.manage`, `teaching.load.view` | MANAGE, VIEW | OBJ-12 | New (DL-135, DL-136; Principal manages responsibilities) |
| `teaching.bell.manage`, `teaching.plan.manage` | MANAGE, MANAGE | OBJ-27 | New (DL-136, DL-137; who approves OQ-150) |
| `teaching.room.manage` | MANAGE | OBJ-27 | New (DL-141; campus office until Campus Services) |
| `teaching.timetable.view`, `teaching.timetable.build`, `teaching.timetable.publish` | VIEW, UPDATE, APPROVE | OBJ-27 | New (DL-138, DL-139; Principal publishes, OQ-150) |
| `teaching.lesson.change` | UPDATE | OBJ-27 | New (DL-140; one-off changes with a reason, OQ-160) |
| `teaching.absence.record`, `teaching.cover.arrange`, `teaching.cover.view` | CREATE, UPDATE, VIEW | OBJ-27 | New (DL-143…DL-145; arranger OQ-162; cover gives one-day scope, no capability) |

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
| Legal names, personal contact, address, date of birth, nationality, emergency contacts, contract terms, leaving reason (photo moves to the T1 directory, DL-151) | OBJ-05, OBJ-06 | T2 | `hr.confidential.view` | — |
| Staff identity numbers, work pass, background check results, medical certificates | OBJ-05, OBJ-28, OBJ-29 | T3 | `hr.restricted.view` | A17 (DL-69); read logged |
| Leave type and balances (colleagues see "Away" only) | OBJ-28 | T2 | `hr.leave.approve` (own via `hr.self.view`) | — |
| Appraisal goals, reviews, rating; candidate CVs and feedback | OBJ-30, OBJ-31 | T2 | `hr.appraisal.review`, `hr.recruitment.manage` | — |
| Audit before and after snapshots | OBJ-11 | T3 | `gov.audit.view_snapshot` | — |

## Template contents

Which capabilities each access role's templates hold is a view of the [persona × capability matrix](../02-users/roles-personas.md#persona--capability-matrix-admin-slice) for the admin slice. Templates for the new student capabilities are written when the Admissions and Student Records slice is designed; until then the student field matrix shows which school roles are expected to hold them (marked pending, BR-06).

## Fixed capability sets

| Set | Holds | Source |
|---|---|---|
| R03 Parent | Read own child's attendance, grades, communication and own cases; edit profile, documents, addresses and contacts | DL-18, DL-28 |
| R05 Student | Pending | OQ-19 |

## Open

answered: OQ-46 → DL-51, OQ-47 → DL-69, OQ-48 → DL-52, OQ-49 → DL-53 · OQ-15, OQ-16, OQ-34 (operator, brand, DPO roles).

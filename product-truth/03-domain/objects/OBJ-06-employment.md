---
id: OBJ-06
name: Employment
entities: [E02, E03, E04, E05, HR01, HR15]   # employment, department, org assignment, position, contract term, checklist task
owner: staff_hr
home: cell
operating_entity: operating_entity_id   # copied at creation (DL-41); employer is not the data controller
effective_dated: true
states:                 # E02 employment_status (rules-to-ui)
  PENDING: info         # label "Starts {date}"
  ACTIVE: success
  SUSPENDED: warning    # no reason shown outside the case (DL-163)
  ENDED: default        # label "Left"
  LEAVING: warning      # derived: end date set; label "Leaves {date}" (DL-160)
field_groups:
  - name: Employee number, employer, category, type, start and end date, position, department, line manager
    tier: T1
    capability: hr.employee.view
    edit: [hr.employee.update, hr.org.manage]
  - name: Contract terms (FTE, hours, probation, notice, work pattern, signed contract)
    tier: T2
    capability: hr.confidential.view
    edit: [hr.contract.manage]
  - name: Leaving reason
    tier: T2
    capability: hr.confidential.view
  - name: Joining and leaving checklists, handover list
    tier: T1
    capability: hr.employee.view        # each owner sees own tasks
    edit: [hr.checklist.manage]
actions:
  - hr.employment.end            # sets the end date; handover tasks (DL-160); account cascade (DL-13)
  - hr.contract.manage
  - hr.org.manage
  - hr.checklist.manage
events: [EmploymentStarted, EmploymentEnded, EmploymentSuspended, LeavingDateSet, LineManagerChanged]
variation: [MC-03]
open: [OQ-167, OQ-181, OQ-182, OQ-183, OQ-185]
sample: product-truth/03-domain/samples/staff-hr-sample.json
patterns: [AP-04, AP-05, AP-14]
---

# OBJ-06 · Employment

**Status:** Confirmed structure (DL-43, DL-46), 7 Nov 2026; refined with the staff & HR dictionary v0.1, 25 Nov 2026 · **Owner:** Henry
**Cites:** [staff & HR dictionary](../staff-hr-data-dictionary.md) · Employee DD E02–E05 · DL-13, DL-41, DL-148, DL-150, DL-152, DL-160, DL-163

One engagement with one legal employer, with its position, department, line manager and contract terms; every change is a new effective-dated row (DL-24, DL-152). Setting an end date shows "Leaves {date}" and lists everything to hand over (DL-160). Suspension cascades to the account; account changes never change employment (DL-13). Salary is never stored (DL-147).

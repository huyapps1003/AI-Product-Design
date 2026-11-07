---
id: OBJ-05
name: Employee
entities: [E01, E07, E08]
owner: staff_hr
home: cell              # cell of primary employment
effective_dated: true
states:                 # E01 record_status (rules-to-ui)
  ACTIVE: success
  INACTIVE: default
  ARCHIVED: default
sync_states:            # E08 sync_status
  PENDING: info
  SYNCED: success
  FAILED: error
  CONFLICT: warning
field_groups:           # rules-to-ui § Field visibility (D13)
  - name: Name, preferred name, work email, work phone, position, campus
    tier: T1
    capability: hr.employee.view
  - name: Legal names, photo
    tier: T2
    capability: hr.confidential.view
actions:
  - hr.employee.create
  - hr.employee.update
  - hr.campus_assignment.manage   # never grants access (BR-06)
events: [EmployeeCreated, CampusAssignmentChanged]
variation: [MC-03, MC-13]
open: [Employee DD D01 HR source of truth per brand, OQ-27]
sample: product-truth/06-experience/admin-platform/README.md#canonical-sample-data
patterns: [AP-03, AP-04, AP-12, AP-14]
---

# OBJ-05 · Employee

**Status:** Confirmed structure (DL-43, DL-46), 7 Nov 2026; values cite their sources · **Owner:** Henry
**Cites:** see `entities`, `states` and comments above

Campus affiliation never grants access; it opens a review (BR-06, Decision D07).

Rules: [`../architecture.md`](../architecture.md). Capabilities: [`../../04-access/permission-catalogue.md`](../../04-access/permission-catalogue.md).

---
id: OBJ-06
name: Employment
entities: [E02, E03, E04, E05]
owner: staff_hr
home: cell
operating_entity: employer_org_id   # employer is not the data controller
effective_dated: true
states:                 # E02 employment_status (rules-to-ui)
  PENDING: info         # label "Starts {date}"
  ACTIVE: success
  SUSPENDED: warning
  ENDED: default
  LEAVING: warning      # derived: ACTIVE with end_date within 30 days
field_groups:
  - name: Employee number, employer, employment type, start and end date
    tier: T1
    capability: hr.employee.view
  - name: Termination reason
    tier: T2
    capability: hr.confidential.view
actions:
  - hr.employment.end            # cascade per DL-13
events: [EmploymentStarted, EmploymentEnded, EmploymentSuspended]
variation: [MC-03]
open: [Employee DD D03 concurrent employments, D06 organisation model]
sample: product-truth/06-experience/admin-platform/README.md#canonical-sample-data
patterns: [AP-04, AP-05, AP-14]
---

# OBJ-06 · Employment

**Status:** Confirmed structure (DL-43, DL-46), 7 Nov 2026; values cite their sources · **Owner:** Henry
**Cites:** see `entities`, `states` and comments above

Suspension cascades to the account; account changes never change employment (DL-13).

Rules: [`../architecture.md`](../architecture.md). Capabilities: [`../../04-access/permission-catalogue.md`](../../04-access/permission-catalogue.md).

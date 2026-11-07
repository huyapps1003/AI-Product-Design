---
id: OBJ-12
name: Teaching allocation
entities: [D01, D02, D03]
owner: teaching            # Academic Allocation and Timetable
home: cell
effective_dated: true
states: pending            # allocation statuses not defined; timetable_status belongs to Timetable (AP-13)
field_groups:
  - name: Role, subject, grade and class allocations, sessions
    tier: T1
    capability: teaching.allocation.view
actions:
  - teaching.allocation.manage
events: [AllocationChanged]   # feeds derived scope suggestions (DL-16)
variation: [MC-07]
open: [Employee DD D11, D14 academic scope IDs]
sample: product-truth/06-experience/admin-platform/README.md#canonical-sample-data
patterns: [AP-04, AP-05]
---

# OBJ-12 · Teaching allocation

**Status:** Confirmed structure (DL-43, DL-46), 7 Nov 2026; values cite their sources · **Owner:** Henry
**Cites:** see `entities`, `states` and comments above

Allocations suggest scope; they never grant access by themselves (DL-16, BR-06).

Rules: [`../architecture.md`](../architecture.md). Capabilities: [`../../04-access/permission-catalogue.md`](../../04-access/permission-catalogue.md).

---
id: OBJ-11
name: Audit event
entities: [A14]
owner: platform_services   # audit service; append only
home: cell                 # redacted global index
states: none               # immutable
actor_types: [USER, SYSTEM]
field_groups:
  - name: Who, what, when, acting for, purpose
    tier: T1
    capability: gov.audit.view
  - name: Before and after snapshots
    tier: T3
    capability: gov.audit.view_snapshot
actions:
  - gov.report.export
events: []
variation: []
open: [OQ-09 purpose values]
sample: product-truth/06-experience/admin-platform/README.md#canonical-sample-data
patterns: [AP-06, AP-07]
---

# OBJ-11 · Audit event

**Status:** Confirmed structure (DL-43, DL-46), 7 Nov 2026; values cite their sources · **Owner:** Henry
**Cites:** see `entities`, `states` and comments above

Every grant change and every Tier 3 read writes an event, with "B acting for A" when delegated (DL-36).

Rules: [`../architecture.md`](../architecture.md). Capabilities: [`../../04-access/permission-catalogue.md`](../../04-access/permission-catalogue.md).

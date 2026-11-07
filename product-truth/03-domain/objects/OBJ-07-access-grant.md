---
id: OBJ-07
name: Access grant
entities: [A03, A04]
owner: identity_access     # written by Global Admin through the Access Registry (DL-32)
home: global               # replicated to every cell
scope_node: [GROUP, BRAND, BRAND_COUNTRY, CAMPUS]   # DL-17, DL-40
effective_dated: true
states:                    # A03/A04 grant_status
  ACTIVE: success
  REVOKED: error
  EXPIRED: default
actions:
  - ga.module_grant.manage   # Group or Brand level needs a second Global Admin (DL-09)
  - ga.campus_grant.manage
events: [GrantChanged]
variation: [MC-13]
open: [OQ-15 platform operator access, OQ-48 support sessions]
sample: product-truth/06-experience/admin-platform/README.md#canonical-sample-data
patterns: [AP-01, AP-05, AP-07, AP-14]
---

# OBJ-07 · Access grant

**Status:** Confirmed structure (DL-43, DL-46), 7 Nov 2026; values cite their sources · **Owner:** Henry
**Cites:** see `entities`, `states` and comments above

A grant is a ceiling; it never gives data access without a membership (DL-02).

Rules: [`../architecture.md`](../architecture.md). Capabilities: [`../../04-access/permission-catalogue.md`](../../04-access/permission-catalogue.md).

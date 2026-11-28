---
id: OBJ-08
name: Team membership
entities: [A05, A07, A10, A12, A13]
owner: identity_access     # written by Module Admin through the Access Registry
home: cell
scope_node: [GROUP, BRAND, BRAND_COUNTRY, CAMPUS]   # DL-35; campus default
effective_dated: true
states:                    # A05 membership_status
  INVITED: info
  ACTIVE: success
  DEACTIVATED: warning
  REMOVED: default
scope_sources: [STATIC, ALLOCATION, ORG_UNIT]   # relationship removed (DL-37); ORG_UNIT dimension shown as Department (DL-150)
routes: [POSITION_TEMPLATE, REQUEST, ADMIN, ALLOCATION]   # how the membership arrived (DL-171); module must be Live or Pilot (DL-166)
actions:
  - tm.membership.manage
  - tm.scope.manage            # derived scope is a suggestion by default (DL-16)
events: [MembershipChanged, ScopeChanged]
variation: [MC-07]
open: [Employee DD D08 scope group semantics]
sample: product-truth/03-domain/samples/admin-platform-sample.json   # admin platform dictionary (DL-165)
patterns: [AP-01, AP-05, AP-07, AP-14]
---

# OBJ-08 · Team membership

**Status:** Confirmed structure (DL-43, DL-46), 7 Nov 2026; values cite their sources · **Owner:** Henry
**Cites:** see `entities`, `states` and comments above

No scope means no data (BR-17); the explainer names the membership behind each answer (DL-38).

Rules: [`../architecture.md`](../architecture.md). Capabilities: [`../../04-access/permission-catalogue.md`](../../04-access/permission-catalogue.md).

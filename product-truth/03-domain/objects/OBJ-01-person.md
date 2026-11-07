---
id: OBJ-01
name: Person
entities: [Person (DL-42), A01, A02]
owner: identity_access
home: global            # Tier 1 only (DL-39); profiles live in cells
effective_dated: false
states:                 # A01 account_status (rules-to-ui § Status vocabulary)
  INVITED: info
  ACTIVE: success
  SUSPENDED: warning
  DISABLED: error
  CLOSED: default
field_groups:
  - name: Display name, role chips (Student, Guardian, Staff), home cell
    tier: T1
    capability: identity.person.view
  - name: Login, identity provider, last login
    tier: T1
    capability: identity.account.view
actions:
  - identity.person.merge        # review queue only, never automatic (DL-42)
  - identity.account.suspend
events: [PersonCreated, PersonMerged, AccountStatusChanged]
variation: [MC-15]
open: [OQ-39 which cell holds a guardian with children in two countries]
sample: product-truth/06-experience/admin-platform/README.md#canonical-sample-data
patterns: [AP-03, AP-07, AP-14]
---

# OBJ-01 · Person

**Status:** Confirmed structure (DL-43, DL-46), 7 Nov 2026; values cite their sources · **Owner:** Henry
**Cites:** see `entities`, `states` and comments above

One login per human with a Staff / Parent / Student context switch (AR-05). Profiles are separate cards: OBJ-02, OBJ-04, OBJ-05.

Rules: [`../architecture.md`](../architecture.md). Capabilities: [`../../04-access/permission-catalogue.md`](../../04-access/permission-catalogue.md).

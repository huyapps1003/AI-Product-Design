---
id: OBJ-01
name: Person
entities: [Person (DL-42), A01, A02]   # A01 account, A02 sign-in identity; refined in the admin platform dictionary (DL-165, DL-169)
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
open: [OQ-195 primary email for multi-brand staff, OQ-204 dormant threshold, OQ-209 parent and student sign-in]   # OQ-39 answered by DL-55
sample: product-truth/03-domain/samples/admin-platform-sample.json   # admin platform dictionary (DL-165)
patterns: [AP-03, AP-07, AP-14]
---

# OBJ-01 · Person

**Status:** Confirmed structure (DL-43, DL-46), 7 Nov 2026; values cite their sources · **Owner:** Henry
**Cites:** see `entities`, `states` and comments above

One login per human with a Staff / Parent / Student context switch (AR-05). Profiles are separate cards: OBJ-02, OBJ-04, OBJ-05.

Rules: [`../architecture.md`](../architecture.md). Capabilities: [`../../04-access/permission-catalogue.md`](../../04-access/permission-catalogue.md).

---
id: OBJ-36
name: Access review
entities: [PA10, CC03]   # review cycle; one CAMPUS_ATTESTATION case per campus
owner: identity_access   # cycle; the case runs on the Case engine (DL-08, DL-120)
home: cell               # cases in the campus's data location; cycle definition global
scope_node: [BRAND_COUNTRY, CAMPUS]
effective_dated: false
states:                  # PA10 cycle_status; each case uses OBJ-10 states
  SCHEDULED: info
  OPEN: warning          # "{n} of {m} campuses attested"
  CLOSED: default
line_decisions: [KEEP, REDUCE, REMOVE]   # S5
flags: [TIER3_HOLDER, CROSS_CAMPUS, OVERRIDE, DORMANT, LEAVING_30_DAYS, NO_EMPLOYEE_LINK, OPERATOR_SESSION_USED]
field_groups:
  - name: Review lines (person, access held, flags, decision)
    tier: T1
    capability: gov.access_review.decide   # Campus Leader
  - name: Review cycles
    tier: T1
    capability: gov.review_cycle.manage    # Global or Brand Admin
actions:
  - gov.review_cycle.manage
  - gov.access_review.decide
  - gov.report.export
events: [ReviewCycleOpened, MembershipChanged]
variation: []
open: [OQ-204, OQ-206]
sample: product-truth/03-domain/samples/admin-platform-sample.json
patterns: [AP-07, AP-13, AP-14]
---

# OBJ-36 · Access review

**Status:** Proposed with the admin platform dictionary v0.1, 28 Nov 2026 (DL-174 Assumed) · **Owner:** Henry
**Cites:** [admin platform dictionary](../admin-platform-data-dictionary.md) · [S5 flow](../../06-experience/admin-platform/flows/S5-access-review.md) · DL-08

Every term a review cycle opens one attestation case per campus, due in 10 working days (S5). The Campus Leader keeps, reduces or removes each person's access, flagged rows first. Undecided rows escalate to the brand-country head; a missed review never removes access by itself (DL-174).

---
id: OBJ-35
name: Special access
entities: [A16, A17, A18]   # delegation, Tier 3 type grant, operator session
owner: identity_access
home: cell                 # in the campus's data location (DL-179)
scope_node: [BRAND_COUNTRY, CAMPUS]
effective_dated: true      # every item has valid dates or a time cap
states:                    # A18 session_status; A16 and A17 use their own lists (dictionary)
  REQUESTED: info          # "Waiting for {approver}"
  APPROVED: info
  ACTIVE: warning          # "Live · ends {time}"
  ENDED: default
  DECLINED: error
  EXPIRED: default
field_groups:
  - name: Delegations ("Acting for" banner)
    tier: T1
    capability: ga.delegation.manage       # delegator or admin
  - name: Tier 3 type grants
    tier: T1
    capability: ga.type_grant.request
    approve: [ga.type_grant.approve]       # Campus Leader (DL-69); OQ-205 when they are the grantee
  - name: Operator sessions and break-glass
    tier: T1
    capability: ga.operator_session.request
    approve: [ga.operator_session.approve] # Campus Leader (DL-52)
actions:
  - ga.delegation.manage
  - ga.type_grant.request
  - ga.type_grant.approve
  - ga.operator_session.request
  - ga.operator_session.approve
  - ga.operator_session.break_glass
events: [DelegationStarted, DelegationEnded, TypeGrantApproved, TypeGrantExpired, OperatorSessionStarted, OperatorSessionEnded, BreakGlassOpened]
variation: [MC-13]
open: [OQ-15, OQ-205, OQ-207, OQ-208]
sample: product-truth/03-domain/samples/admin-platform-sample.json
patterns: [AP-06, AP-07, AP-10, AP-13, AP-14]
---

# OBJ-35 · Special access

**Status:** Proposed with the admin platform dictionary v0.1, 28 Nov 2026 (A16, A17 Confirmed entities DL-36; DL-175 Assumed) · **Owner:** Henry
**Cites:** [admin platform dictionary](../admin-platform-data-dictionary.md) · DL-21, DL-36, DL-52, DL-69

Three kinds of access that exist for a limited time and always name who approved them: a delegation (someone acts for someone else, at most 90 days, DL-21 held), a Tier 3 type grant (standing access to a class of restricted records, approved by the Campus Leader, DL-69) and an operator session (GSG support, read-only, at most 4 hours, approved by the Campus Leader, DL-52, DL-175). All three show on the campus's access review and audit log.

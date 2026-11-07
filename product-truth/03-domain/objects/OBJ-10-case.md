---
id: OBJ-10
name: Case
entities: [Case (Architecture § Data model), A15]
owner: communication_cases
home: cell
operating_entity: copied at creation   # DL-41
visibility: [NORMAL, RESTRICTED]        # DL-19
states:                    # rules-to-ui § Status vocabulary
  OPEN: info               # label "New"
  IN_REVIEW: secondary
  WAITING: warning         # "Waiting for info"
  ESCALATED: error         # "Overdue"
  APPROVED: success        # "Done"
  APPLIED: success
  REJECTED: error
  CANCELLED: default       # "Withdrawn"
field_groups:
  - name: Restricted case content
    tier: T3
    capability: gov.case.view_all   # or A15 entry as assignee or committee
actions:
  - gov.access_review.decide
events: [CaseOpened, CaseEscalated, CaseClosed]
variation: [MC-05]
open: [OQ-47 Tier 3 type grant approver]
sample: product-truth/06-experience/admin-platform/README.md#canonical-sample-data
patterns: [AP-06, AP-07, AP-14]
---

# OBJ-10 · Case

**Status:** Confirmed structure (DL-43, DL-46), 7 Nov 2026; values cite their sources · **Owner:** Henry
**Cites:** see `entities`, `states` and comments above

One Case object for complaints, appeals, withdrawals, Student Pass issues and access requests (DL-08); SLAs per Architecture.

Rules: [`../architecture.md`](../architecture.md). Capabilities: [`../../04-access/permission-catalogue.md`](../../04-access/permission-catalogue.md).

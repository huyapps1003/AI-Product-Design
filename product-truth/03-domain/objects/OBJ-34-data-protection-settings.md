---
id: OBJ-34
name: Data protection settings
entities: [PA07, PA08, PA09]   # purpose, retention rule, privacy notice
owner: identity_access     # purposes are registry definitions (DL-32); retention and notices edited in the Admin Platform
home: global
operating_entity: pending  # notices show "[data controller — OQ-06]" (AP-09)
effective_dated: true      # notice versions, rule status
states:                    # PA09 notice_status; PA07 and PA08 use DRAFT, ACTIVE, RETIRED
  DRAFT: default
  PUBLISHED: success
  SUPERSEDED: default
field_groups:
  - name: Purposes
    tier: T1
    capability: gov.audit.view
    edit: [gov.purpose.manage]             # Global Admin; owner OQ-200
  - name: Retention rules
    tier: T1
    capability: gov.audit.view
    edit: [gov.retention.manage]           # Data Protection Lead of the country activates (DL-176)
  - name: Privacy notices
    tier: T1
    capability: identity.account.view      # staff see their own notices
    edit: [gov.notice.manage]
actions:
  - gov.purpose.manage
  - gov.retention.manage
  - gov.notice.manage
events: [NoticePublished]
variation: [MC-03, MC-09, MC-10]
open: [OQ-06, OQ-09, OQ-10, OQ-25, OQ-43, OQ-199, OQ-200, OQ-201, OQ-202]
sample: product-truth/03-domain/samples/admin-platform-sample.json
patterns: [AP-09, AP-10, AP-13, AP-14]
---

# OBJ-34 · Data protection settings

**Status:** Proposed with the admin platform dictionary v0.1, 28 Nov 2026 (DL-176 Assumed) · **Owner:** Henry
**Cites:** [admin platform dictionary](../admin-platform-data-dictionary.md) · [data-controller-model](../../05-data-governance/data-controller-model.md) · DL-44, AR-04, AR-10

The purpose catalogue feeds the purpose prompt on Tier 3 reveal, cross-cell access and support sessions (AR-10). Retention rules are per country and record type; a rule with no period deletes nothing and shows "Pending · OQ-43". Privacy notices are versioned per brand-country and audience and never name a controller until OQ-06 is answered. Consent records and data-sharing records are not here (OBJ-17, OBJ-18).

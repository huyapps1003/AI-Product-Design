---
id: OBJ-14
name: Legal entity
entities: [OR07, OR08]
owner: tenancy_config      # team Open (OQ-58)
home: global               # DL-39
tenant_path: none (beside the tree, DL-41)
operating_entity: this object
effective_dated: true      # campus operator link (DL-41)
states:                    # DL-50 candidates; colours proposed for review
  ACTIVE: success
  DISSOLVED: default
operator_roles: [OPERATOR, FRANCHISEE, JV_PARTNER]   # DL-50; which campuses Open (OQ-03)
field_groups:
  - name: Legal name, type, country, registration number
    tier: T1
    capability: ga.catalog.manage      # view and edit; wider viewing Open (OQ-58)
    edit: [org.campus_operator.manage]
  - name: Campuses operated, with dates
    tier: T1
    capability: ga.catalog.manage
    edit: [org.campus_operator.manage] # handover effects Open (OQ-04)
actions:
  - org.campus_operator.manage         # New; asks "from when" (AP-05) and the change case
events: [CampusOperatorChanged]
variation: [MC-03]
open: [OQ-02 which entities exist, OQ-03 partner-run campuses, OQ-04 handover, OQ-06 controller roles, OQ-56 brand legal names, OQ-58 owner and approver]
sample: product-truth/03-domain/samples/organisation-sample.json   # all entities are "[operating entity — OQ-02]"
patterns: [AP-05, AP-09, AP-13, AP-14]
---

# OBJ-14 · Legal entity

**Status:** Proposed card, 9 Nov 2026 (Organisation dictionary v0.1) · **Owner:** Henry
**Cites:** see `entities`, `states` and comments above · [organisation dictionary](../organisation-data-dictionary.md)

An operating entity is not a scope level: show it on the record, never in the scope picker (DL-41). Records keep the entity copied when they were created. Never call it the data controller (AR-04, OQ-06). Until OQ-02 is answered, every entity name is the placeholder "[operating entity — OQ-02]".

Rules: [`../architecture.md`](../architecture.md). Capabilities: [`../../04-access/permission-catalogue.md`](../../04-access/permission-catalogue.md).

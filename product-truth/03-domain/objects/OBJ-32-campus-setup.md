---
id: OBJ-32
name: Campus setup and go-live
entities: [PA01, PA02, PA03, PA04, PA05]   # module switch-on, setup run, setup task, authority register entry, identity provider connection
owner: tenancy_config      # Brand Admin or Global Admin through Tenancy and config (DL-43)
home: global               # Tier 1 only (DL-39)
scope_node: [BRAND, BRAND_COUNTRY, CAMPUS]
operating_entity: none     # the OPERATOR_LINKED task reads OR08; entity names pending OQ-02
effective_dated: true      # switch-on live_from, authority cutover_date
states:                    # PA02 run_status; colours proposed (AP-14)
  DRAFT: default
  IN_PROGRESS: info        # "{n} of {m} done"
  READY: success           # "Ready for sign-off"
  LIVE: success
  CANCELLED: default
module_switch_states:      # PA01 switch_status
  PLANNED: info
  PILOT: secondary
  LIVE: success
  PAUSED: warning
  OFF: default
field_groups:
  - name: Setup runs, tasks, module switch-on
    tier: T1
    capability: ga.setup.manage            # Brand Admin (own brand), Global Admin; Campus Leader views own campus
    edit: [ga.setup.manage, ga.module_switch.manage]
    approve: [ga.setup.approve]            # brand-country head (DL-167; OQ-196)
  - name: Legacy systems (authority register)
    tier: T1
    capability: ga.authority.manage
    source: authoritative_system per field group (DL-45)
  - name: Sign-in connection
    tier: T1
    capability: identity.idp.manage
actions:
  - ga.setup.manage
  - ga.setup.approve
  - ga.module_switch.manage
  - ga.authority.manage
  - identity.idp.manage
events: [ModuleSwitchChanged, SetupTaskDone, CampusWentLive, AuthorityCutOver]
variation: [MC-01, MC-02, MC-14]
open: [OQ-05, OQ-21, OQ-27, OQ-28, OQ-55, OQ-58, OQ-194, OQ-196, OQ-197]
sample: product-truth/03-domain/samples/admin-platform-sample.json
patterns: [AP-01, AP-02, AP-04, AP-12, AP-13, AP-14]
---

# OBJ-32 · Campus setup and go-live

**Status:** Proposed with the admin platform dictionary v0.1, 28 Nov 2026 (DL-166…DL-169 Assumed, awaiting Henry) · **Owner:** Henry
**Cites:** [admin platform dictionary](../admin-platform-data-dictionary.md) · OR01–OR08, AC06, AC07 · DL-45, DL-50, DL-113

A campus goes live through a setup run: a checklist where most tasks tick themselves from the owning record (node, operator link, data location, sign-in, published year, Campus Leader, module dates, legacy system rows) and the brand-country head signs off (DL-167). Modules are switched on per brand-country or campus, and nobody can be a member of a module that isn't Live or in Pilot there (DL-166). The legacy systems list says which system is the source for each field group until its cutover date; until then the field is read-only with a source chip (DL-45, DL-168).

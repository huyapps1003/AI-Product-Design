---
id: OBJ-13
name: Scope node and campus
entities: [OR01, OR02, OR03, OR04, OR05, OR06, OR09]
owner: tenancy_config      # team Open (OQ-58)
home: global               # DL-39; Tier 1 only, no personal data
tenant_path: the node's own path
operating_entity: shown from OBJ-14   # never a scope level (DL-41)
effective_dated: true      # nodes and setting values (AR-03); nodes close, never move (DL-48)
states:                    # DL-50 candidates; colours proposed for review
  PLANNED: info
  ACTIVE: success
  INACTIVE: warning
  CLOSED: default
levels: [GROUP, BRAND, BRAND_COUNTRY, CAMPUS, SEGMENT]   # DL-40; grants stop at CAMPUS
field_groups:
  - name: Name, level, parent, path, status
    tier: T1
    capability: any staff with a grant on the node or above
    edit: [ga.catalog.manage]
  - name: Campus code, city, time zone, stages served
    tier: T1
    capability: any staff with a grant on the node or above
    edit: [ga.catalog.manage]          # code standard Open (OQ-55)
  - name: Data region
    tier: T1
    source: OR06 via Brand-country     # read-only; derived from country (DL-40)
  - name: Inherited settings
    tier: T1
    capability: per setting's module
    edit: [org.setting.manage]         # New; who may override Open (OQ-20)
actions:
  - ga.catalog.manage                  # open, rename, close a node
  - org.setting.manage                 # set or override a value where OPEN (AP-02)
events: [ScopeNodeOpened, ScopeNodeClosed, SettingChanged]
variation: [MC-01, MC-02, MC-04, MC-06, MC-14]
open: [OQ-20 overridable settings, OQ-21 data regions beyond SG and IN, OQ-26 brand in shared shells, OQ-50 stage campuses, OQ-52 shared sites, OQ-55 campus codes, OQ-56 brand names, OQ-57 unlisted campuses, OQ-58 tree owner, OQ-59 regions above brand-country]
sample: product-truth/03-domain/samples/organisation-sample.json   # full list: 01-context/gsg-footprint.json
patterns: [AP-01, AP-02, AP-08, AP-13, AP-14]
---

# OBJ-13 · Scope node and campus

**Status:** Proposed card, 9 Nov 2026 (Organisation dictionary v0.1; DL-47…DL-50 Assumed) · **Owner:** Henry
**Cites:** see `entities`, `states` and comments above · [organisation dictionary](../organisation-data-dictionary.md)

The breadcrumb reads Group › Brand › Brand-country › Campus, e.g. "GIIS · Singapore · SMART Campus Punggol" (AP-01). Show a campus with its brand, because names repeat across brands. Call a cell a **data region** (DL-47). Show data regions other than Singapore and India as Planned (OQ-21).

Rules: [`../architecture.md`](../architecture.md). Capabilities: [`../../04-access/permission-catalogue.md`](../../04-access/permission-catalogue.md).

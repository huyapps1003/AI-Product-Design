---
id: OBJ-33
name: Access role and template
entities: [A06, A08, A09, A10, R01, R02, R03]   # access role, template version, template capability, binding, module, capability, parent set
owner: identity_access     # GSG platform team owns definitions (DL-51)
home: global
effective_dated: true      # template versions are immutable once published (DL-172)
states:                    # A08 template_status
  DRAFT: default
  PUBLISHED: success       # "v{n} published"
  RETIRED: default
field_groups:
  - name: Roles, templates, capabilities, position templates
    tier: T1
    capability: ga.catalog.manage          # Global Admin edits; everyone with an admin role views
    edit: [ga.catalog.manage]
    approve: [ga.template.publish]
  - name: Capability tier lock
    tier: T1
    capability: ga.catalog.manage          # T3 capabilities carry a lock chip and "Record ACL or Tier 3 grant needed"
actions:
  - ga.catalog.manage
  - ga.template.publish
events: [TemplatePublished]
variation: [MC-07]
open: [OQ-192, OQ-198]
sample: product-truth/03-domain/samples/admin-platform-sample.json
patterns: [AP-05, AP-06, AP-07, AP-14]
---

# OBJ-33 · Access role and template

**Status:** Proposed with the admin platform dictionary v0.1, 28 Nov 2026 (DL-172, DL-173, DL-178 Assumed) · **Owner:** Henry
**Cites:** [admin platform dictionary](../admin-platform-data-dictionary.md) · [permission catalogue](../../04-access/permission-catalogue.md) · DL-07, DL-14, DL-20, DL-33, DL-51, DL-53, DL-149

Access roles and templates are defined once by the GSG platform team; brands ask for changes (DL-172). A template is versioned and a published version never changes; publishing shows how many members are still on older versions. Each brand keeps a position → standard template list used when a campus assignment changes (DL-149, DL-173); it never contains Tier 3 capabilities.

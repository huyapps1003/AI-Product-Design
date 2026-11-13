---
id: OBJ-04
name: Guardian
entities: [ST04, ST05, ST07]
owner: student_records
home: cell              # one profile per child's data location (DL-55)
effective_dated: true
relationship_types: [MOTHER, FATHER, LEGAL_GUARDIAN, LOCAL_GUARDIAN, OTHER]   # DL-27
states:                 # ST05 verification_status (DL-25); colours proposed for review
  PENDING: warning
  VERIFIED: success
  REJECTED: error
field_groups:
  - name: Names, relationship, can-collect, emergency flag
    tier: T2
    capability: student.guardian.view
  - name: Guardian phone and email
    tier: T2
    capability: student.guardian_contact.view
    edit: [parent_own]   # DL-28
actions:
  - student.guardian.verify      # VERIFIED creates the family link (OBJ-09, DL-25)
  - student.guardian.edit
events: [GuardianRelationshipChanged]
variation: [MC-09, MC-12]
open: [OQ-11 custody records, OQ-10 consent, OQ-25 notice language]   # DL-58 own contact changes apply in every data location
sample: product-truth/03-domain/samples/student-sample.json   # viewer Lina Goh, SG-PGL; other brands' scope nodes from gsg-landscape
patterns: [AP-03, AP-04, AP-05, AP-13, AP-14]
---

# OBJ-04 · Guardian

**Status:** Confirmed structure (DL-43, DL-46), 7 Nov 2026; values cite their sources · **Owner:** Henry
**Cites:** see `entities`, `states` and comments above

A relationship is who the guardian is to the child; access comes only from the family link (AR-09).

Rules: [`../architecture.md`](../architecture.md). Capabilities: [`../../04-access/permission-catalogue.md`](../../04-access/permission-catalogue.md).

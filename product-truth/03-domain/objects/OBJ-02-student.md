---
id: OBJ-02
name: Student
entities: [ST01, ST06, ST07, ST08, ST09, ST10]
owner: student_records
home: cell              # cell of current enrolment
effective_dated: true   # DL-24
states:                 # ST01 record_status (DL-27); colours as E01
  ACTIVE: success
  INACTIVE: default
  ARCHIVED: default
field_groups:           # student dictionary § Field visibility by role; capabilities per DL-34
  - name: Name, preferred name, Student ID, class, grade, house, programme
    tier: T1
    capability: student.profile.view
  - name: Photo, date of birth
    tier: T2
    capability: student.profile.view_confidential
  - name: Addresses
    tier: T2
    capability: student.address.view
    edit: [student.profile.edit, parent_own]   # DL-28
  - name: Identity documents, nationality, immigration status
    tier: T3
    capability: student.identity.reveal       # A17 type grant or A15 ACL (DL-36)
    edit: [admissions, parent_own]            # DL-28
  - name: Attendance threshold (reason not shown)
    tier: T2
    capability: student.attendance_threshold.view   # DL-30
actions:
  - student.profile.edit
  - student.identity.reveal      # AP-06: reason asked, read logged
events: [StudentCreated, ProfileEditedByParent, ImmigrationStatusChanged]
variation: [MC-05, MC-08, MC-09]
open: [OQ-37 identity fields per country, OQ-43 retention, OQ-19 student login grade]   # DL-60 medical alert flag only; DL-61 Student ID format; DL-62 student sees own T1
sample: product-truth/03-domain/samples/student-sample.json   # viewer Lina Goh, SG-PGL; other brands' scope nodes from gsg-landscape
patterns: [AP-03, AP-04, AP-05, AP-06, AP-13, AP-14]
---

# OBJ-02 · Student

**Status:** Confirmed structure (DL-43, DL-46), 7 Nov 2026; values cite their sources · **Owner:** Henry
**Cites:** see `entities`, `states` and comments above

Student ID is unique across Campus OS and kept on transfer (DL-31). Immigration data never appears outside Tier 3; teachers see only the threshold (DL-30).

Rules: [`../architecture.md`](../architecture.md). Capabilities: [`../../04-access/permission-catalogue.md`](../../04-access/permission-catalogue.md).

---
id: OBJ-03
name: Enrolment
entities: [ST02, ST03]
owner: admissions          # creates; student_records holds (AR-06)
home: cell
tenant_path: group/brand/brand-country/campus
operating_entity: copied at creation   # DL-41; controller unnamed until OQ-06
effective_dated: true      # DL-24; a transfer ends one enrolment and starts another
states:                    # DL-27 values; colours proposed for review
  OFFERED: info
  CONFIRMED: info
  ENROLLED: success
  DEFERRED: warning
  TRANSFERRED_OUT: default
  WITHDRAWN: default
  COMPLETED: default
  CANCELLED: error
end_reasons: [WITHDRAWAL, CAMPUS_TRANSFER, COURSE_CHANGE, PROMOTION_ROLLOVER, GRADUATION, NON_DELIVERY, EXPULSION, OTHER]
field_groups:
  - name: Status, dates, end reason, transfer history
    tier: T2
    capability: student.enrolment.view
    edit: [admissions.enrolment.end]
  - name: Class, grade, programme offering, subject choices
    tier: T1
    source: OBJ-15         # read-only here; owned by Academic structure (AP-04)
actions:
  - admissions.enrolment.create
  - admissions.enrolment.end          # asks end reason and "from when" (AP-05)
  - admissions.enrolment.transfer     # AP-11 transfer review
events: [StudentEnrolled, EnrolmentEnded, StudentTransferred]
variation: [MC-06, MC-07, MC-10]
open: [OQ-45 AEPP as offering or placement, OQ-07 cross-entity transfer]
sample: pending   # student sample set (Lina Goh, SG-PGL) is in project files, not yet in the repo
patterns: [AP-01, AP-04, AP-05, AP-11, AP-13, AP-14]
---

# OBJ-03 · Enrolment

**Status:** Confirmed structure (DL-43, DL-46), 7 Nov 2026; values cite their sources · **Owner:** Henry
**Cites:** see `entities`, `states` and comments above

Never call a class a "Section" (DL-29). A cross-cell transfer needs a sharing record (DL-44).

Rules: [`../architecture.md`](../architecture.md). Capabilities: [`../../04-access/permission-catalogue.md`](../../04-access/permission-catalogue.md).

---
id: OBJ-20
name: Attendance mark
entities: [AT01, AT02, AT03, AT04, AT05, AT06, AT07, AT08, AT09, AT10]
owner: attendance          # feature area of Student Records (DL-43); prefix attendance (DL-97); team key attendance_conduct until Conduct is designed
home: cell                 # campus data location (DL-47); group dashboards get counts and % only
tenant_path: group/brand/brand-country/campus
operating_entity: none     # enrolment holds operating_entity_id (DL-41)
effective_dated: true      # school days per campus × offering × year (DL-96, DL-98); rules valid from a year; marks versioned by corrections (DL-100)
states:                    # mark_code, DL-27 rule; colours proposed for review
  PRESENT: success
  LATE: warning
  LEFT_EARLY: warning
  ABSENT: error
  ON_LEAVE: info
  SCHOOL_ACTIVITY: info
# Register register_status: NOT_TAKEN default (warning after due_at), SUBMITTED info, LOCKED success
# Leave request_status: SUBMITTED info, APPROVED success, DECLINED error, CANCELLED default, CERTIFICATE_DUE warning, CLOSED default
# Summary standing: ON_TRACK success, AT_RISK warning, BELOW error, CANNOT_RECOVER error
field_groups:
  - name: School days and holidays
    tier: T1
    capability: attendance.register.view
    edit: [attendance.calendar.manage]           # who: OQ-120
  - name: Register and day marks
    tier: T2
    capability: attendance.register.view         # class teacher own class (DL-73); parents own child (DL-107)
    edit: [attendance.register.take, attendance.mark.edit_after_lock]
    approve: [attendance.correction.approve]     # coordinator (DL-100, OQ-110)
  - name: Leave requests and reasons
    tier: T2
    capability: attendance.register.view
    edit: [attendance.leave.request]             # parent path
    approve: [attendance.leave.approve]          # class teacher 1–2 days, Principal above (DL-101)
  - name: Medical certificate file
    tier: T3
    capability: attendance.certificate.view      # others see "Certificate received" only
  - name: Gate log
    tier: T2
    capability: attendance.register.view
    edit: [attendance.gate.record]               # reception / security
    approve: [attendance.out_pass.approve]       # class teacher or level coordinator (HB p.23)
  - name: Attendance % and threshold
    tier: T2
    capability: attendance.summary.view          # threshold only, never why (DL-30)
    edit: [attendance.rule.manage]
  - name: ICA review
    tier: T3
    capability: attendance.ica.manage            # Admissions/registrar; never teachers (DL-105)
actions:
  - attendance.calendar.manage
  - attendance.register.take
  - attendance.mark.edit_after_lock
  - attendance.correction.approve
  - attendance.leave.request
  - attendance.leave.approve
  - attendance.gate.record
  - attendance.out_pass.approve
  - attendance.rule.manage
  - attendance.ica.manage
events: [CalendarPublished, RegisterSubmitted, RegisterLocked, AttendanceMarked, MarkCorrected, LeaveRequested, LeaveDecided, CertificateReceived, GateEventRecorded, AttendanceSummaryUpdated, AttendanceAlertRaised, IcaReviewOpened]
variation: [MC-06, MC-07]
open: [OQ-41, OQ-108, OQ-109, OQ-110, OQ-111, OQ-112, OQ-113, OQ-114, OQ-115, OQ-116, OQ-117, OQ-118, OQ-119, OQ-120, OQ-121]
sample: product-truth/03-domain/samples/attendance-sample.json
# Reasons, notes and certificates never leave Attendance; other modules read AT09 only (DL-106). Fee status never blocks (DL-83).
patterns: [AP-01, AP-02, AP-04, AP-05, AP-13, AP-14]
---

# OBJ-20 · Attendance mark

**Status:** Proposed with the attendance dictionary v0.1, 19 Nov 2026 (DL-97…DL-107 Assumed, awaiting Henry) · **Owner:** Henry
**Cites:** [attendance dictionary](../attendance-data-dictionary.md) · Arch § Key workflows (Absence and leave) · HB p.23, p.166–167, p.174 · DL-30, DL-73, DL-83, DL-96

One mark per student per school day is the record (DL-99). School days come from the curriculum's year at the campus minus holidays and breaks (DL-98), so every % has the same denominator. The class teacher takes the register; it locks at end of day and later changes need a reason (DL-100). Leave and gate events change marks on their own (DL-101, DL-102). Thresholds, warning margins, cut-off times and leave routing limits are campus settings: show the value or "Not set", never a hard-coded number (DL-59 pattern). Teachers see a threshold, never the Student Pass behind it (DL-30); the ICA review is a Tier 3 Case (DL-105).

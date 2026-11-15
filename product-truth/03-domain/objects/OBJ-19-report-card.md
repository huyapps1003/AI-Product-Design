---
id: OBJ-19
name: Mark and report card
entities: [AS01, AS02, AS03, AS04, AS05, AS06, AS07, AS08, AS09, AS10, AS11, AS12, AS13]
owner: assessment          # Assessment domain (architecture.md); prefix assessment (DL-87)
home: cell                 # campus data location; schemes with the brand-country; templates global, no student data
tenant_path: group/brand/brand-country/campus
operating_entity: none     # enrolment holds operating_entity_id (DL-41)
effective_dated: true      # scheme versions valid from an academic year (DL-87); mark versions (DL-88); card versions (DL-90)
states:                    # report card card_status, DL-27 rule; colours proposed for review
  DRAFT: default
  READY: info
  RELEASED: success
  SUPERSEDED: default
# Mark sheet sheet_status: NOT_OPEN default, OPEN info, SUBMITTED info, RETURNED warning, MODERATED info, LOCKED success
# Appeal appeal_status: SUBMITTED info, WITH_TEACHER info, AWAITING_ENDORSEMENT warning, UPHELD success, NOT_UPHELD default, WITHDRAWN default, LATE_REJECTED error
# Promotion decision_status: PROPOSED default, NEEDS_DECISION warning, RETEST_PENDING warning, CONFIRMED success, SENT_TO_ROLLOVER info
field_groups:
  - name: Scheme (components, weights, grade scale, promotion rule)
    tier: T1
    capability: assessment.result.view
    edit: [assessment.scheme.edit]
    approve: [assessment.scheme.approve]        # Academic Supervisor (DL-87); brand-country approver OQ-93
  - name: Assessment calendar
    tier: T1
    capability: assessment.result.view
    edit: [assessment.calendar.manage]           # coordinator (Arch step 1)
  - name: Marks before release
    tier: T2
    capability: assessment.mark.enter            # own subjects and classes (OBJ-12); class teacher own class read (DL-73)
    edit: [assessment.mark.enter, assessment.mark.moderate, assessment.mark.lock, assessment.mark.correct]
  - name: Results and report cards
    tier: T2
    capability: assessment.result.view           # parents own children after release (DL-93); students per campus setting (OQ-96)
    edit: [assessment.report_card.generate, assessment.report_card.remark]
    approve: [assessment.report_card.release]
  - name: Appeals
    tier: T2
    capability: assessment.appeal.review
    edit: [assessment.appeal.raise]
    approve: [assessment.appeal.endorse]         # Examination Board (OQ-98)
  - name: Promotion decisions
    tier: T2
    capability: assessment.promotion.review
    approve: [assessment.promotion.decide]       # Principal for detention/retention; run approval in Classes (DL-75)
  - name: Predicted grades
    tier: T2
    capability: assessment.predicted.enter
    approve: [assessment.predicted.release]
actions:
  - assessment.template.manage                   # GSG platform team
  - assessment.scheme.edit
  - assessment.scheme.approve
  - assessment.calendar.manage
  - assessment.mark.enter
  - assessment.mark.moderate
  - assessment.mark.lock
  - assessment.mark.correct
  - assessment.mark.approve_proration            # Principal (HB p.43)
  - assessment.mark.publish                      # coordinator; parents see marks after this (DL-93)
  - assessment.report_card.generate
  - assessment.report_card.release
  - assessment.report_card.remark
  - assessment.appeal.raise                      # parent
  - assessment.appeal.review
  - assessment.appeal.endorse
  - assessment.promotion.review
  - assessment.promotion.decide
  - assessment.predicted.enter
  - assessment.predicted.release
events: [SchemeTemplatePublished, SchemeApproved, AssessmentScheduled, MarkSheetSubmitted, MarkSheetLocked, MarksPublished, MarkChanged, ResultsCalculated, ReportCardsReleased, ReportCardReissued, AppealRaised, AppealDecided, PromotionProposed, PromotionConfirmed, PredictedGradesReleased]
variation: [MC-06, MC-07]
open: [OQ-79, OQ-93, OQ-94, OQ-95, OQ-96, OQ-97, OQ-98, OQ-99, OQ-100, OQ-101, OQ-102, OQ-103, OQ-104, OQ-105, OQ-106]
sample: product-truth/03-domain/samples/assessment-sample.json
# Fee status never shown or checked (DL-83). Grades are calculated, never typed (DL-89). Absence = code only, no medical detail (DL-95, DL-66).
patterns: [AP-01, AP-02, AP-04, AP-05, AP-06, AP-13, AP-14]
---

# OBJ-19 · Mark and report card

**Status:** Proposed with the assessment dictionary v0.1, 15 Nov 2026 (DL-87…DL-95 Confirmed) · **Owner:** Henry
**Cites:** [assessment dictionary](../assessment-data-dictionary.md) · DL-73, DL-75, DL-83, DL-87

Rules come from a GSG template per board that each brand-country adapts; an Academic Supervisor approves each version (DL-87). Marks are entered per class on a mark sheet and locked after moderation; grades are calculated from the scheme version for that year. A report card is a released snapshot that never changes; a correction reissues it. Parents see only published marks and released report cards, and fee status never holds a report card back (DL-83). Promotion outcomes go to the Classes rollover, where the Principal approves (DL-75). Appeal windows, release deadlines and attendance thresholds are settings: show the value or "Not set", never a hard-coded number.

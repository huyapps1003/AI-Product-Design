---
id: OBJ-30
name: Vacancy and candidate
entities: [HR11, HR12, HR13, HR14]   # vacancy, candidate application, interview feedback, offer
owner: staff_hr
home: cell
tenant_path: group/brand/brand-country/campus
operating_entity: none
effective_dated: false
states:                    # HR11 vacancy_status; colours proposed
  DRAFT: default
  PENDING_APPROVAL: info
  OPEN: success
  ON_HOLD: warning
  FILLED: default
  CANCELLED: default
# Candidate stage: APPLIED … OFFER (default), HIRED success, REJECTED "Not progressed" default, WITHDRAWN default
# Offer: PENDING_APPROVAL info, SENT info, ACCEPTED success, DECLINED default
field_groups:
  - name: Vacancy (position, campus, FTE, start, reason)
    tier: T1
    capability: hr.recruitment.manage
    edit: [hr.vacancy.request, hr.recruitment.manage]
    approve: [hr.vacancy.approve]
  - name: Candidates, CVs, interview feedback, offers
    tier: T2
    capability: hr.recruitment.manage      # hiring manager for own vacancy; interviewers own feedback first
    edit: [hr.interview.feedback, hr.recruitment.manage]
    approve: [hr.offer.approve]
actions:
  - hr.vacancy.request
  - hr.vacancy.approve
  - hr.recruitment.manage
  - hr.interview.feedback
  - hr.offer.approve
events: [VacancyOpened, OfferAccepted]
variation: []
open: [OQ-174, OQ-175, OQ-176]
sample: product-truth/03-domain/samples/staff-hr-sample.json
patterns: [AP-05, AP-14]
---

# OBJ-30 · Vacancy and candidate

**Status:** Proposed with the staff & HR dictionary v0.1, 25 Nov 2026 (DL-159 Confirmed by Henry, 27 Nov 2026) · **Owner:** Henry
**Cites:** [staff & HR dictionary](../staff-hr-data-dictionary.md) · DL-58

An approved vacancy collects candidates, matched to existing Persons through the review queue (DL-58). Interviewers record feedback before seeing others'. An accepted offer creates the employment as "Starts {date}" with a joining checklist (DL-159, DL-160).

---
id: OBJ-31
name: Appraisal and training
entities: [HR16, HR17, HR18]   # appraisal cycle, appraisal, training record
owner: staff_hr
home: cell
tenant_path: group/brand/brand-country/campus
operating_entity: none
effective_dated: false
states:                    # HR17 stage; colours proposed
  GOALS: info
  IN_YEAR: info
  SELF_REVIEW: info
  MANAGER_REVIEW: info
  MODERATION: info
  SHARED: success
  CLOSED: default          # "Done"; overdue stage shows "Overdue" warning
# Training request_status: REQUESTED info, APPROVED success, REJECTED default, COMPLETED success, RECORDED default
field_groups:
  - name: Appraisal goals, observations, reviews, rating
    tier: T2
    capability: hr.appraisal.review        # employee (own), reviewer, Principal, HR; never colleagues
    edit: [hr.appraisal.review]
    approve: [hr.appraisal.moderate]
  - name: Appraisal cycles
    tier: T1
    capability: hr.appraisal.manage
    edit: [hr.appraisal.manage]
  - name: Training records
    tier: T1
    capability: hr.employee.view           # own and team; certificates T2
    edit: [hr.training.request]
    approve: [hr.training.approve]
actions:
  - hr.appraisal.manage
  - hr.appraisal.review
  - hr.appraisal.moderate
  - hr.training.request
  - hr.training.approve
events: [AppraisalShared, TrainingCompleted]
variation: []
open: [OQ-177, OQ-178, OQ-179, OQ-180]
sample: product-truth/03-domain/samples/staff-hr-sample.json
patterns: [AP-05, AP-14]
---

# OBJ-31 · Appraisal and training

**Status:** Proposed with the staff & HR dictionary v0.1, 25 Nov 2026 (DL-161, DL-162 Assumed) · **Owner:** Henry
**Cites:** [staff & HR dictionary](../staff-hr-data-dictionary.md) · Arch module 12 · HB p.138 · DL-96

A yearly appraisal per brand-country: goals, mid-year check-in, self-review, manager review, Principal moderation, then shared with the employee (DL-161). Ratings are Tier 2 and not linked to pay in Campus OS. Training records count hours and can renew a compliance item such as child-protection training or IB Category 1 (DL-162).

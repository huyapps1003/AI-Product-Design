---
id: OBJ-15
name: Class
entities: [CL01, CL02, CL03, CL04, CL05, CL06, CL07]
owner: academic_structure  # feature area "Classes" (DL-70); programme offering, year, term, grade setup still pending its own dictionary
home: cell                 # data location of the campus
tenant_path: group/brand/brand-country/campus
operating_entity: none     # classes carry no contract; enrolment holds operating_entity_id (DL-41)
effective_dated: true      # placements (CL04) and class staff (CL03) are dated; classes are per academic year (DL-70)
states:                    # class_status, DL-27 rule; colours proposed for review
  DRAFT: default
  ACTIVE: success
  CLOSED: default
# Class plan (CL02) is the seat limit Admissions reads (DL-61, DL-71)
# Rollover run (CL06) run_status: DRAFT, OUTCOMES_PENDING, READY_FOR_APPROVAL, APPROVED, APPLIED, CANCELLED
field_groups:
  - name: Class, room, class teacher
    tier: T1
    capability: classes.class.view
    edit: [classes.class.manage, classes.staff.assign]
  - name: Class plan and seat limit
    tier: T1
    capability: classes.class.view
    edit: [classes.plan.edit]
    approve: [classes.plan.approve]          # Principal (DL-61)
  - name: Roster
    tier: T1
    capability: classes.roster.view          # teachers: own classes only
    source: OBJ-02, OBJ-03                   # student fields follow the student dictionary visibility table; medical alert flag only (DL-66), threshold only (DL-30)
  - name: Placement reason note
    tier: T2
    capability: classes.placement.manage
  - name: Rollover outcomes
    tier: T2
    source: Assessment                       # outcome only (DL-75)
    capability: classes.rollover.prepare
actions:
  - classes.plan.edit
  - classes.plan.approve
  - classes.class.manage
  - classes.staff.assign
  - classes.placement.manage                 # who: OQ-73
  - classes.placement.approve_over_limit     # Principal (DL-71)
  - classes.house.assign
  - classes.rollover.prepare
  - classes.rollover.approve                 # Principal
events: [ClassPlanApproved, ClassCreated, ClassActivated, ClassClosed, ClassTeacherAssigned, StudentPlaced, StudentClassMoved, HouseAssigned, RolloverRunApproved, RolloverApplied]
variation: [MC-06, MC-07, MC-14]
open: [OQ-20, OQ-73, OQ-74, OQ-75, OQ-76, OQ-77, OQ-78, OQ-79]
sample: product-truth/03-domain/samples/classes-sample.json
# Never "Section" (DL-29). AEPP 1/2 = PLACEMENT classes (DL-74, DL-65). Stage-campus promotion no extra approval; other year-end campus change both principals; cross-brand/country = new admission (DL-62, DL-63)
patterns: [AP-01, AP-02, AP-04, AP-05, AP-13, AP-14]
---

# OBJ-15 · Class

**Status:** Proposed with the classes dictionary v0.1, 14 Nov 2026 (DL-70, DL-72, DL-75 Confirmed 14 Nov 2026; DL-71, DL-73, DL-74, DL-76 Assumed) · **Owner:** Henry
**Cites:** [classes dictionary](../classes-data-dictionary.md) · DL-29, DL-54, DL-61, DL-62, DL-63, DL-65

A class is one group for one year (DL-70). The class plan is where the seat limit is set; Admissions shows it read-only (DL-71). Class size limits and house lists are the campus's own settings; show the value or "Not set", never a hard-coded number (DL-59 pattern). Programme offerings, academic years, terms and grades are referenced here and still need the Academic structure dictionary.

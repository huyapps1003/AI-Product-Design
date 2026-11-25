---
id: OBJ-05
name: Employee
entities: [E01, E07, E08, HR02]   # employee (refined), campus assignment, external mapping, emergency contact
owner: staff_hr            # Staff and HR domain (DL-43); prefix hr (DL-146)
home: cell                 # data location of the employing entity's country (DL-164)
tenant_path: group/brand/brand-country/campus
operating_entity: none     # on E02 (OBJ-06)
effective_dated: true
states:                    # E01 record_status (rules-to-ui)
  ACTIVE: success
  INACTIVE: default
  ARCHIVED: default
compliance_states:         # E01 compliance_status, derived (DL-158); colours proposed
  CLEARED: success
  DUE_SOON: warning
  ACTION_NEEDED: warning
  NOT_CLEARED: error
sync_states:               # E08 sync_status
  PENDING: info
  SYNCED: success
  FAILED: error
  CONFLICT: warning
field_groups:              # DL-151
  - name: Directory (preferred name, photo, position, department, campus, work contact)
    tier: T1
    capability: hr.employee.view           # colleagues at the campus
    edit: [hr.employee.update]
  - name: Personal contact, address, date of birth, nationality, gender, emergency contacts
    tier: T2
    capability: hr.confidential.view
    edit: [hr.employee.update, hr.self.update]   # self edits contact and emergency contacts at once
  - name: Legal names
    tier: T2
    capability: hr.confidential.view
    edit: [hr.employee.update]             # HR verifies a document; may be HRIS-owned (DL-147, AP-04)
  - name: Identity document numbers
    tier: T3
    capability: hr.restricted.view         # plus A17 (DL-69)
  - name: Compliance status (chip only)
    tier: T1
    capability: hr.compliance.view         # line manager, coordinator when assigning (DL-158)
actions:
  - hr.employee.create
  - hr.employee.update
  - hr.self.update
  - hr.campus_assignment.manage   # never grants access (BR-06, DL-149)
events: [EmployeeCreated, CampusAssignmentChanged, ComplianceStatusChanged]
variation: [MC-03, MC-13]
open: [OQ-166, OQ-167, OQ-173, OQ-183, OQ-188]
sample: product-truth/03-domain/samples/staff-hr-sample.json
patterns: [AP-03, AP-04, AP-12, AP-14]
---

# OBJ-05 · Employee

**Status:** Confirmed structure (DL-43, DL-46), 7 Nov 2026; refined with the staff & HR dictionary v0.1, 25 Nov 2026 (DL-146 Confirmed, DL-147…DL-164 Assumed) · **Owner:** Henry
**Cites:** [staff & HR dictionary](../staff-hr-data-dictionary.md) · Employee DD E01, E07, E08 · DL-42, DL-148, DL-149, DL-151

The staff profile of one Person (DL-42, DL-148). Colleagues see the directory; HR sees Tier 2; identity numbers need Tier 3 access. Campus affiliation never grants access; it opens a review (BR-06, DL-149). Fields an HRIS still owns show a "From {system}" chip (DL-147). Compliance status is one chip anyone who assigns or manages the person can see; the details stay with HR (DL-158).

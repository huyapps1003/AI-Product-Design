---
id: OBJ-29
name: Compliance record
entities: [HR08, HR09, HR10]   # compliance item type, requirement, record
owner: staff_hr
home: cell
tenant_path: group/brand/brand-country/campus
operating_entity: none
effective_dated: true
states:                    # HR10 record_status; colours proposed
  SUBMITTED: info          # "To verify"
  VALID: success
  EXPIRING: warning        # "Expires {date}"
  EXPIRED: error
  REJECTED: error
  WAIVED: default          # Principal, with reason
# Employee-level rollup is E01 compliance_status (OBJ-05): Cleared, Due soon, Action needed, Not cleared
field_groups:
  - name: Item, issued and expiry dates, status
    tier: T1
    capability: hr.compliance.view
    edit: [hr.compliance.manage]
  - name: Qualifications and training certificates
    tier: T2
    capability: hr.confidential.view
  - name: Work pass, background check, medical check (numbers, results, documents)
    tier: T3
    capability: hr.restricted.view         # plus A17 (DL-69, OQ-173)
  - name: Requirements per brand-country and position
    tier: T1
    capability: hr.compliance.view
    edit: [hr.compliance_rule.manage]
actions:
  - hr.compliance.manage
  - hr.compliance.waive
  - hr.compliance_rule.manage
events: [ComplianceItemExpiring, ComplianceStatusChanged]
# Not cleared blocks start, new teaching assignments and cover suggestions; never ends employment (DL-158)
variation: [MC-13]
open: [OQ-159, OQ-171, OQ-172, OQ-173, OQ-179]
sample: product-truth/03-domain/samples/staff-hr-sample.json
patterns: [AP-04, AP-05, AP-14]
---

# OBJ-29 · Compliance record

**Status:** Proposed with the staff & HR dictionary v0.1, 25 Nov 2026 (DL-157, DL-158 Assumed) · **Owner:** Henry
**Cites:** [staff & HR dictionary](../staff-hr-data-dictionary.md) · HB p.138, p.210–213 · DL-30, DL-69

Work passes, background checks, safeguarding training, qualifications and board training, each with dates and a status. Requirements are set per brand-country and position (DL-157). They roll up into one compliance status per employee; Not cleared blocks starting work and new teaching, and opens an HR case, but never ends employment by itself (DL-158). Results and numbers of checks and passes are Tier 3.

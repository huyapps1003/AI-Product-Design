---
id: OBJ-28
name: Staff leave
entities: [HR03, HR04, HR05, HR06, HR07]   # leave type, leave policy, leave balance, leave request, approval step
owner: staff_hr
home: cell
tenant_path: group/brand/brand-country/campus
operating_entity: none
effective_dated: true      # policies are versions (DL-153); balances per leave year (DL-155)
states:                    # HR06 request_status; colours proposed
  DRAFT: default
  PENDING: info            # "Waiting for {approver}"
  APPROVED: success
  REJECTED: error          # "Declined"
  CANCELLED: default
  CERTIFICATE_DUE: warning
field_groups:
  - name: Away today (no type)
    tier: T1
    capability: hr.employee.view           # colleagues; Teaching gets TE12 only (DL-156)
  - name: Leave requests with type, balances
    tier: T2
    capability: hr.leave.approve           # employee (own), line manager, Principal, HR
    edit: [hr.leave.request, hr.leave.manage]
    approve: [hr.leave.approve]
  - name: Medical certificate
    tier: T3
    capability: hr.restricted.view         # plus A17; line manager sees "Certificate received"
  - name: Leave types and policies
    tier: T1
    capability: hr.employee.view
    edit: [hr.leave_policy.manage]
actions:
  - hr.leave.request
  - hr.leave.approve
  - hr.leave.manage
  - hr.leave_policy.manage
events: [LeaveRequested, LeaveApproved, LeaveCancelled]
variation: [MC-06]
open: [OQ-163, OQ-168, OQ-169, OQ-170, OQ-182, OQ-189]
sample: product-truth/03-domain/samples/staff-hr-sample.json
patterns: [AP-05, AP-13, AP-14]
---

# OBJ-28 · Staff leave

**Status:** Proposed with the staff & HR dictionary v0.1, 25 Nov 2026 (DL-153…DL-156 Confirmed by Henry, 27 Nov 2026) · **Owner:** Henry
**Cites:** [staff & HR dictionary](../staff-hr-data-dictionary.md) · TE12, DL-143 · DL-36, DL-59

Staff apply for leave or report sick the same morning; the line manager approves, and the Principal too for long or term-time leave (DL-154). Balances are calculated, never typed (DL-155). Each brand-country adopts GSG's country leave template and adjusts it, never below the statutory minimum (DL-153). Approved leave makes the teacher "Away" in Teaching so cover can be arranged, and never sends the type or reason (DL-156).

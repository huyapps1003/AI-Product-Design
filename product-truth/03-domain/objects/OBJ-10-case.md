---
id: OBJ-10
name: Case
entities: [CC01, CC02, CC03, CC04, CC05, CC06, CC07, CC08, CC09, A15]
owner: communication_cases   # Engagement domain (DL-43); prefix cases (DL-119)
home: cell                   # campus data location; two-campus cases live at the originating campus (DL-130)
tenant_path: group/brand/brand-country/campus
operating_entity: copied at creation   # DL-41
effective_dated: true        # a case keeps the case policy version it started with (DL-121)
visibility: [NORMAL, RESTRICTED]        # DL-19; RESTRICTED invisible to anyone not named (DL-125)
states:                      # DL-124 (Assumed); replaces the earlier rules-to-ui Case row when confirmed
  NEW: info                  # "New"
  IN_PROGRESS: secondary     # "In progress"
  WAITING: warning           # "Waiting for family"; deadline clock paused (DL-122)
  DECIDED: success           # label from outcome: Approved / Resolved (success), Rejected (error), No action (default)
  APPLIED: success           # "Done": the owning module has acted (DL-120)
  CLOSED: default
  WITHDRAWN: default
# Flags beside the status: Overdue (error) when a deadline is breached; escalation step "With supervisor" / "With committee" / "External referral" (warning) (DL-123)
# Deadline (CC04) status: PENDING default, MET success, BREACHED error, SKIPPED default, PAUSED warning
field_groups:
  - name: Case header (number, type, status, due date)
    tier: T1
    capability: cases.case.view
    edit: [cases.case.work]
  - name: Case content, replies and documents
    tier: T2                 # tier of the case type (CC01)
    capability: cases.case.view
    edit: [cases.case.work]
  - name: Internal notes
    tier: T2
    capability: cases.case.view   # never shown to families (DL-126)
    edit: [cases.case.work]
  - name: Approvals
    tier: T1
    capability: cases.case.view
    approve: [cases.case.decide, gov.access_review.decide]   # plus module approvals, e.g. attendance.leave.approve
  - name: Restricted case content
    tier: T3
    capability: gov.case.view_all   # or A15 entry as assignee or participant, or A17 type grant (DL-69)
  - name: Case policy and types
    tier: T1
    capability: cases.case.view
    edit: [cases.policy.manage, cases.type.manage]
  - name: Feedback (compliments, suggestions)
    tier: T2
    capability: cases.feedback.manage
actions:
  - cases.case.create
  - cases.case.work
  - cases.case.decide
  - cases.case.restrict
  - cases.feedback.manage
  - cases.policy.manage
  - cases.type.manage
  - cases.report.view
  - gov.access_review.decide
events: [CaseOpened, CaseAssigned, CaseAcknowledged, CaseWaiting, CaseResumed, CaseDeadlineBreached, CaseEscalated, CaseDecided, CaseApplied, CaseClosed, CaseWithdrawn, FeedbackReceived]
variation: [MC-05]
open: [OQ-130, OQ-131, OQ-132, OQ-133, OQ-134, OQ-135, OQ-137, OQ-141, OQ-143, OQ-144]
sample: product-truth/03-domain/samples/cases-communications-sample.json
patterns: [AP-06, AP-07, AP-14]
---

# OBJ-10 · Case

**Status:** Confirmed structure (DL-43, DL-46), 7 Nov 2026; detail proposed with the cases & communications dictionary v0.1, 22 Nov 2026 (DL-119 Confirmed; DL-120…DL-130 Assumed) · **Owner:** Henry
**Cites:** [cases & communications dictionary](../cases-communications-data-dictionary.md) · Arch § Data model ("one shared object with SLA timers"), § Key workflows (Feedback and complaints) · HB p.233–237, p.245, p.249–250 · DL-08, DL-19, DL-41, DL-84

One Case object for complaints, appeals, refunds, withdrawals, transfers, leave over 2 days, Student Pass and ICA reviews and access requests (DL-08, DL-120). The case holds status, owner, deadlines, escalation, approvals and the timeline; the module that owns the request keeps the detail and applies the decision. GSG defines case types; each brand-country sets deadlines, routing and approvers in a versioned policy (DL-121). Deadlines count office working days and always show a date (DL-122). For GIIS SG complaints: acknowledge 2, close 7, supervisor 14, Dispute Resolution Committee 21 working days, then the CPE scheme (HB p.249–250). Show settings as values or "Not set", never hard-coded numbers (DL-59 pattern). Restricted cases do not appear at all for people who aren't named (DL-125).

Rules: [`../architecture.md`](../architecture.md). Capabilities: [`../../04-access/permission-catalogue.md`](../../04-access/permission-catalogue.md).

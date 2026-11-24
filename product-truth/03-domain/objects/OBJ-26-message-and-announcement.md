---
id: OBJ-26
name: Message and announcement
entities: [CC10, CC11, CC12, CC13, CC14, CC15, CC16]
owner: communication_cases   # Engagement domain (DL-43); prefix comms (DL-119)
home: cell                   # campus data location; brand-country announcements fan out per campus (DL-130)
tenant_path: group/brand/brand-country/campus
operating_entity: none
effective_dated: false       # templates are versioned (DL-129)
states:                      # DL-27 rule; colours proposed for review
  # Conversation
  OPEN: info
  CLOSED: default
  CONVERTED: secondary       # "Turned into case {number}"
# Announcement status: DRAFT default, AWAITING_APPROVAL warning, SCHEDULED info, SENT success, WITHDRAWN default
# Notification delivery: SENT info, DELIVERED success, FAILED error, SKIPPED_BY_PREFERENCE default
# Category chips: SAFETY error, REQUIRED warning, OPTIONAL default
field_groups:
  - name: Conversations with guardians
    tier: T2
    capability: comms.conversation.use       # staff: students in data scope (DL-127); guardians: own child
  - name: Conversation oversight
    tier: T2
    capability: comms.conversation.oversee   # when allowed: OQ-142
  - name: Announcements
    tier: T1
    capability: comms.announcement.post      # own class; wider audience needs approval (DL-128)
    approve: [comms.announcement.approve]    # approver OQ-138
  - name: Read and acknowledgement counts
    tier: T1
    capability: comms.announcement.post
  - name: Notification templates
    tier: T1
    capability: comms.template.manage        # brand-country
  - name: Delivery log
    tier: T1
    capability: comms.notification_log.view
  - name: Notification preferences
    tier: T1
    capability: self                         # each person edits their own; SAFETY and REQUIRED cannot be muted (DL-129)
actions:
  - comms.conversation.use
  - comms.conversation.oversee
  - comms.announcement.post
  - comms.announcement.approve
  - comms.template.manage
  - comms.notification_log.view
events: [ConversationStarted, MessageSent, AnnouncementSent, AnnouncementAcknowledged, NotificationSent, NotificationFailed]
variation: [MC-05]
open: [OQ-25, OQ-136, OQ-137, OQ-138, OQ-139, OQ-140, OQ-142, OQ-145, OQ-146]
sample: product-truth/03-domain/samples/cases-communications-sample.json
patterns: [AP-01, AP-02, AP-14]
---

# OBJ-26 · Message and announcement

**Status:** Proposed with the cases & communications dictionary v0.1, 22 Nov 2026 (DL-119, DL-120 Confirmed; DL-121…DL-130 Assumed) · **Owner:** Henry
**Cites:** [cases & communications dictionary](../cases-communications-data-dictionary.md) · Arch § Data model (Engagement: Message, Announcement), § Tech stack (one notification service; parent preferences per channel and language) · HB p.24, p.34 · ST04, ST07 · DL-30, DL-73

Every conversation is between staff and the guardians of one student; staff reach only students in their data scope (DL-127). Announcements go to an audience rule, never a typed list, and anything wider than the author's own class needs approval (DL-128). All modules notify families through one service: templates per brand-country, language and channel; SAFETY and REQUIRED notices cannot be muted; no Tier 3 content in any notification (DL-129). A read acknowledgement is not consent (OBJ-17).

# Campus OS — Cases & Communications: design brief for Claude Design

**Built from:** `product-truth/03-domain/cases-communications-data-dictionary.md` v0.1, OBJ-10 and OBJ-26 cards, Attendance (DL-101, DL-105, DL-107), Fees (DL-84), Assessment (DL-94), RBAC (DL-08, DL-19, DL-69), GIIS Student Handbook 2025-26 (HB) p.8–9, p.24, p.34, p.233–237, p.245, p.249–250
**Sample data:** `2-sample-data.json`. Where this brief and the dictionary disagree, the dictionary wins.

## 1. Context

- Global Campus OS is the school platform GSG runs for all its brands. GIIS SMART Campus Punggol (SG-PGL) is the example campus; OWIS Digital Campus (SG-OWD) is the second brand.
- Scope (approved by Henry, 22 Nov 2026, DL-119): **anything a person must act on by a deadline, and every message the school sends or receives about a student.** One Case engine shared by every module, complaints and feedback, staff ↔ guardian messages, announcements and one notification service. PTM booking, surveys and consent are later.
- Main viewer: **Priya Menon, Customer Relations Officer, SG-PGL, on desktop.** Today is **Monday 23 Nov 2026, 09:30**. Other viewers: Dr Anita Rao (Principal), Farah Ismail (class teacher 7B, phone), Meera Joshi (Academic Coordinator, Grades 6–8), Lina Goh (Admissions, Tier 3), Vivek Menon (Country Director, GIIS Singapore), Claire Tan (Principal, OWIS Digital Campus), Neha Sharma (parent; Parent app is later, DL-118).
- Staff screens are desktop, except the class teacher's messages (phone first, DL-06).

## 2. Words

| Use | Meaning | Don't use |
|---|---|---|
| **Case** | Anything tracked with an owner and a deadline (complaint, refund, leave over 2 days, withdrawal…) | Ticket, request, issue |
| **Complaint**, **Compliment**, **Suggestion** | The three kinds of feedback (HB p.249) | Grievance, kudos |
| **Working day** | Mon–Fri minus public holidays; deadlines count these (DL-122) | Business day, school day |
| **Acknowledge** | First reply saying we've received it (2 working days at GIIS SG) | Confirm receipt |
| **Reply to family** / **Internal note** | Sent to the family vs staff only (DL-126) | Comment, public/private |
| **Waiting for family** | Clock paused until they reply (DL-122) | On hold, pending |
| **With supervisor** / **With committee** | Escalation steps 14 and 21 (DL-123) | Level 2, tier 2 |
| **Dispute Resolution Committee** | Chaired by the Country Director (HB p.250) | Board, panel |
| **CPE referral** | External scheme proposed after day 21; recorded only | Appeal to regulator |
| **Restricted case** | Opens only for people named on it; others in the same queue see a locked row (DL-125) | Private, confidential |
| **Conversation** | Messages with the guardians of one student (DL-127) | Chat, thread |
| **Announcement** | Notice to an audience (campus, grade, class) (DL-128) | Broadcast, circular |
| **Safety / Required / Optional** | Notification categories; only Optional can be turned off (DL-129) | Priority levels |

## 3. Chips

Case status: New · info, In progress · secondary, Waiting for family · warning, Approved / Resolved · success, Rejected · error, No action · default, Done · success, Closed · default, Withdrawn · default.
Beside status: **Overdue** · error; **With supervisor** / **With committee** · warning; **Restricted** (lock icon) · default.
Deadline step: met (green tick), current (dark), paused (amber), missed (red !), later (grey).
Announcement: Draft · default, Awaiting approval · warning, Scheduled · info, Sent · success, Withdrawn · default. Category: Safety · error, Required · warning, Optional · default.
Delivery: Delivered · success, Sent · info, Failed · error, Skipped by preference · default.

## 4. Rules that change the UI

1. **One inbox for every case** (DL-120). Complaints, leave over 2 days, refunds, withdrawals, transfers, appeals, ICA reviews and access requests share one list. The owning module keeps the detail; the case links to it ("Detail lives in Fees FB14").
2. **Deadlines are dates, counted in working days** (DL-122). Show "Mon 30 Nov", never only "7 days". Weekends and public holidays are skipped (Deepavali 9–10 Nov). Day 0 is when it was received, even if it was logged later.
3. **GIIS SG complaint ladder** (DL-123, HB p.249–250). Acknowledge by day 2, reply and close by day 7, supervisor by day 14, Dispute Resolution Committee (Country Director) by day 21, then CPE referral. A missed step moves the case up automatically and tells the next owner; the earlier owner keeps access.
4. **Overdue is a flag, not a status** (DL-124). A case can be In progress and Overdue. Sort overdue first, then by next due date.
5. **Waiting for family pauses the clock** (DL-122). Show "Clock paused since …" and that later deadlines move.
6. **Restricted cases are locked rows** (DL-125). If you're not named but can see the queue, the case shows as a locked row: "Restricted case", type, received and due dates; no student name, title or content, and it can't be opened. Never in search, and never on teacher or class screens (DL-30). ICA review is always restricted; a complaint about a named teacher can be made restricted.
7. **Reply vs internal note** (DL-126). The composer has two modes. Internal notes are amber and never shown to the family. In v1 replies go by email.
8. **Approvals** (DL-08, DL-84, DL-62). Some cases need several approvers (refund: Finance Controller + Country Director; campus transfer: both principals). Each approver's decision is shown separately.
9. **Settings, not hard-coded numbers.** OWIS and GIIS India have no documented complaint or refund policy: show "Not set".
10. **Messages are about one student** (DL-127). Teachers reach guardians of their own class; no parent-to-parent. "Turn into complaint" creates a case dated from the parent's message. Warn: don't put health or immigration details in messages.
11. **Announcements need an audience and maybe approval** (DL-128). Class teacher → own class sends now; grade or campus → "Send for approval". "Ask families to confirm they've read it" is not consent.
12. **Notifications** (DL-129). Safety and Required can't be muted; families pick the channel. Never show Tier 3 details or restricted case titles in a notification.

## 5. Who sees what (summary)

Customer Relations: own queues (customer relations, helpdesk), log feedback, compliments and suggestions, delivery log. Principal: everything at the campus except restricted cases they aren't named on; approvals; announcements to approve; deadline performance. Class teacher: conversations with own class's guardians, own class announcements, cases they raised. Coordinator: academic cases for their band, grade announcements. Admissions (Tier 3): withdrawals, transfers, Student Pass and ICA review cases. Country Director: committee cases, refund approvals, case policies for the brand-country. Parent (later): own requests and replies, messages, announcements, notification settings. Group: counts only.

## 6. Pending decisions (show "Pending decision")

OQ-130 what counts as a working day (vacations, India Saturdays, end of day) · OQ-131 does the clock pause; are 14 and 21 counted from receipt · OQ-132 enrolment clarification hours · OQ-133 complaint topics and owners · OQ-134 anonymous complaints, complaints about staff · OQ-135 compliment and suggestion review · OQ-136 Tier 3 check in messages · OQ-137 retention, MyGIIS history · OQ-138 who approves announcements · OQ-139 channels and providers · OQ-140 quiet hours, teacher reply time · OQ-141 result appeals on cases · OQ-142 who may read teacher-parent messages · OQ-143 other brands' complaint processes · OQ-144 helpdesk triage · OQ-145 students raising complaints · OQ-146 both guardians or primary only · OQ-25 languages.

## 7. Suggested screens

1. Case inbox (Priya) · 2. Case detail with deadline ladder, timeline, reply / note, approvals · 3. Log feedback (complaint with live deadline preview) · 4. Compliments and suggestions · 5. Teacher messages on a phone (Farah) · 6. Announcements (compose, approve) · 7. Case policies (Vivek; OWIS "Not set") · 8. Notifications: templates and delivery log · 9. Deadline performance (Dr Rao) · 10. Parent view: my requests, messages, notification settings (later).

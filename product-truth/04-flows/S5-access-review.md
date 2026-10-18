# S5 — Campus access review and "why can they see this?"

**Persona:** Campus Leader (Dr Anita Rao, SG-PGL) · Global Admin · Platform Auditor
**Screens:** SCR-CL-01, SCR-GOV-03, SCR-GOV-01, SCR-GA-04 (Effective permissions), SCR-GOV-04, SCR-GOV-05
**Rules:** BR-08, BR-11, BR-12, BR-16, BR-17 · Decisions D08, D12, D13

**Tóm tắt (VI):** Mỗi học kỳ, Campus Leader xác nhận ai đang có quyền trên campus mình (ưu tiên các dòng rủi ro). Khi có sự cố, mở Effective Permissions để thấy chuỗi giải thích: grant → membership → role → template → scope → override, mỗi mắt xích dẫn tới audit event.

## Flow — termly attestation

```mermaid
flowchart TD
  A[[Calendar engine: week 1 of Semester 2, SG-PGL]] --> B[[Case CAMPUS_ATTESTATION, due 10 working days]]
  B --> C([Campus Leader: Campus overview SCR-CL-01<br/>card 'Access review: 212 people · 18 flagged'])
  C --> D[SCR-GOV-03 grouped list]
  D --> D1[Flagged first:<br/>Restricted capability holders · cross-campus users · overrides ·<br/>no login > 60 days · employment ending < 30 days · no employee link]
  D1 --> E{Per row}
  E -- Keep --> F
  E -- Reduce --> E1[Pick memberships/roles to remove → creates lines for Module Admin] --> F
  E -- Revoke all on campus --> E2[Creates REMOVE lines effective tomorrow] --> F
  F[Bulk 'Keep remaining 194 unflagged'] --> G{All rows decided?}
  G -- no --> G1[Submit disabled: '6 rows left']
  G -- yes --> H[Submit attestation: signed by Dr Anita Rao, timestamp]
  H --> I[Removal lines routed to Module Admins / Global Admin]
  I --> J[Attestation report available in SCR-GOV-05 for EduTrust evidence]
```

## Flow — investigation

```mermaid
flowchart LR
  A([Search person: Arjun S.]) --> B[SCR-GA-04 › Effective permissions]
  B --> C[Search capability 'health note' → wellbeing.health_note.view]
  C --> D[Result: ALLOWED at SG-PGL for 3 students]
  D --> E[Explanation chain]
  E --> E1[GA module grant Wellbeing and Inclusion · granted 12 Aug by Daniel Tan · reason 'Bus first aid']
  E --> E2[TM membership Wellbeing @ SG-PGL · role MEMBER]
  E --> E3[Template wellbeing.member v1 · capability Allow]
  E --> E4[Scope: ROUTE = Bus 7 riders]
  E --> E5[Override: none]
  E1 & E2 & E3 & E4 --> F[Each link → audit event drawer SCR-GOV-04]
  F --> G[Actions: Request removal · Export explanation PDF]
```

## States — Campus attestation (Case `CAMPUS_ATTESTATION`)

Uses the shared Case lifecycle ([README](./README.md#shared-case-object-access-request--access-review)) with one extra rule: `APPROVED` means *attested*; each row decision is stored as a line. A case past due goes `ESCALATED` to the Country Director.

## Screen states

| Screen | Empty | Loading | Error | No access | Success |
|---|---|---|---|---|---|
| SCR-CL-01 | "No review due. Next review: week 1 of Semester 1." | Card skeletons | — | Campus Leader only; VP sees own section's staff | Card turns green "Attested 14 Oct" |
| SCR-GOV-03 | "No one has access to SG-PGL" (unlikely; shows CTA to Global Admin) | Table skeleton with progress bar | Submit fails → keep decisions locally, retry banner | Read-only for Auditor | Confirmation dialog with counts (kept / reduced / revoked) |
| SCR-GA-04 Effective permissions | "Search a capability to see if and why this person has it" | Inline spinner per capability | Evaluation error → "Couldn't evaluate. Showing grants only." | Campus Leader sees only rows for own campus; others "+ access at 1 other campus (hidden)" | Result badge ALLOWED (success) / DENIED (error) / NO SCOPE (warning) with chain |
| SCR-GOV-04 | "No events match these filters" | — | — | Restricted before/after values masked unless `PLATFORM_AUDITOR` or `GLOBAL_ADMIN` (BR-11) | Export toast "CSV ready (redacted)" |

## Explanation chain — design rules

1. Order is always: GA module → GA campus → Membership → Role → Template version → Scope → Override.
2. A broken link is shown in red with the reason ("Campus grant expired 1 Apr") and the result is DENIED.
3. A Deny entry anywhere shows first, with "Deny wins" chip (DL-07).
4. Scope link shows the matching condition group in plain words: "Section is 7B **or** 7C, **and** subject is Mathematics, **and** AY is 2026-27".

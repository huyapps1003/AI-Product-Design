# S3 — Employee exit and access revocation

**Persona:** HR Admin → system → Global Admin / Module Admins → Campus Leader
**Screens:** SCR-HR-03, SCR-HR-05, SCR-GOV-01, SCR-GOV-02, SCR-GA-04, SCR-HR-01 (upcoming exits), SCR-TM-01
**Rules:** BR-03, BR-05, BR-10, BR-12 · Decisions D10, D12

**Tóm tắt (VI):** Nghỉ việc = HR đặt ngày kết thúc. Hệ thống mở Exit Review, mặc định **tự động gỡ toàn bộ quyền vào ngày end_date + 1**. Không tự kích hoạt lại khi tái tuyển dụng.

## Flow

```mermaid
flowchart TD
  A([HR: Profile › Employment › End employment<br/>SCR-HR-05]) --> B[End date 31 Mar 2027, reason RESIGNATION]
  B --> C[Impact preview:<br/>Account ACTIVE · 3 GA module grants · 1 campus grant · 4 memberships<br/>Academic: class teacher 6A, Maths 6A–6C]
  C --> D{Revoke immediately?}
  D -- no (default) --> E[Employment stays ACTIVE until end date]
  D -- yes, reason required --> E2[All grants REVOKED now, account SUSPENDED]
  E --> F[[Case EXIT_REVIEW, due = end date<br/>lines: REMOVE each grant/membership effective 1 Apr]]
  F --> G([Module Admins / Global Admin see task<br/>can Extend ≤ 30 days with reason, e.g. handover of marks])
  G --> H[[1 Apr 00:00 campus time: scheduled job]]
  H --> I[Grants EXPIRED · memberships DEACTIVATED · account SUSPENDED<br/>events EmploymentEnded, GAGrantChanged, MembershipChanged]
  I --> J[Teaching and Learning Module Admin notified:<br/>'6A has no class teacher from 1 Apr']
  I --> K[Account DISABLED after retention window, then CLOSED]
  F --> L([Campus Leader sees exit in SCR-CL-01 'Outgoing'])
```

## States — Employment (E02)

```mermaid
stateDiagram-v2
  [*] --> PENDING: start date in future
  PENDING --> ACTIVE: start date reached
  PENDING --> ENDED: cancelled before start (reason)
  ACTIVE --> SUSPENDED: HR action (e.g. investigation)
  SUSPENDED --> ACTIVE: HR reinstates (reason)
  ACTIVE --> ENDED: end_date passed
  SUSPENDED --> ENDED
  ENDED --> [*]
  note right of ENDED
    Rehire = new Employment on the same Employee.
    Never reopens this one; never restores old access (Decision D10).
  end note
```

Employment SUSPENDED → account SUSPENDED (proposed cascade, DL-13). Account changes never change employment (BR-05).

## Screen states

| Screen | Error / validation | Success / feedback |
|---|---|---|
| SCR-HR-05 | End date required; end date before start date blocked; end date > 90 days ahead → warning "Exits are usually entered within 90 days" | Toast "Exit recorded. Access will end on 1 Apr 2027." + link to case |
| SCR-HR-03 header | — | Status chip "Leaving 31 Mar" (warning) until end date, then "Ended" (default grey) |
| SCR-GOV-02 extend | Extension > 30 days blocked; reason required | Line shows "Extended to 30 Apr by Kenji Lim" |
| SCR-HR-01 | — | KPI "Upcoming exits (30 days): 4" → filtered directory |
| SCR-GA-04 for ended user | All tabs read-only, banner "Employment ended 31 Mar 2027. Access ended 1 Apr 2027." | — |
| Rehire on SCR-HR-04 | Duplicate check finds ended person → "Rehire Rahul Verma" path; summary says "Previous access will not be restored." | New Employment row in timeline; old ones kept |

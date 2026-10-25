# S4 — Set up a module team for the new academic year

**Persona:** Module Admin (Kenji, Attendance and Conduct @ SG-PGL) · Global Admin (publishes template) · Teacher (affected)
**Screens:** SCR-GA-10, SCR-GA-09, SCR-TM-01, SCR-TM-03, SCR-TM-04, SCR-TM-05, SCR-SYS-01
**Rules:** BR-07, BR-08, BR-15, BR-16, BR-17, BR-18 · Decisions D08, D09, D11, D14

**Tóm tắt (VI):** Trước năm học mới, Module Admin chuyển team sang AY mới: scope gợi ý từ phân công giảng dạy, chọn có nâng template lên version mới không, xem trước thay đổi quyền rồi áp dụng theo ngày bắt đầu năm học.

## Flow

```mermaid
flowchart TD
  P0([Global Admin: Template editor SCR-GA-10<br/>attendance.teacher v2 → draft v3]) --> P1[Add capability attendance.mark.edit_after_lock = Allow]
  P1 --> P2[Publish v3 → v2 stays ACTIVE for existing bindings BR-18]
  P2 --> N[Module Admins with bindings on v2 notified]

  A([Calendar engine: 30 days before AY 2027-28 at SG-PGL]) --> B[SCR-TM-01 banner:<br/>'Prepare AY 2027-28 · 38 members · current scopes end with AY 2026-27']
  N --> B
  B --> C[SCR-TM-05 AY rollover table]
  C --> D[Per member: current scope · suggested scope from allocations entity D02 · template version]
  D --> E{Decision per row}
  E -- accept suggestion --> F
  E -- edit scope --> E1[SCR-TM-04 scope builder] --> F
  E -- end membership --> F
  F[Template: keep v2 / move to v3, bulk select] --> G[Preview: '+1 capability for 30 members · 3 members lose section 8A']
  G --> H{Any member with empty scope?}
  H -- yes --> H1[Block apply: 'Kenji: 2 members have no scope for AY 2027-28. Set one or end their membership.' BR-17]
  H -- no --> I[Apply, effective from SG-PGL AY 2027-28 start date]
  I --> J[RoleAssignmentChanged / ScopePolicyChanged events; audit]
```

## States

**TM Membership (A05)**

```mermaid
stateDiagram-v2
  [*] --> INVITED: added before user has logged in
  [*] --> ACTIVE: added for active user, valid_from ≤ today
  INVITED --> ACTIVE: user's first login
  ACTIVE --> DEACTIVATED: valid_to passed, GA grant lost, or exit
  DEACTIVATED --> ACTIVE: Module Admin reactivates (new valid_from, reason) — only if GA grant still active
  ACTIVE --> REMOVED: Module Admin removes
  DEACTIVATED --> REMOVED
  REMOVED --> [*]
```

**Permission Template version (A08)**

```mermaid
stateDiagram-v2
  [*] --> DRAFT
  DRAFT --> ACTIVE: Publish (immutable from here)
  DRAFT --> [*]: Discard
  ACTIVE --> RETIRED: Retire (only when 0 bindings, or after bindings migrated)
  RETIRED --> [*]
```

## Screen states

| Screen | Empty | Error / validation | No access | Success |
|---|---|---|---|---|
| SCR-GA-10 | New template: "Start from blank or copy from an existing template" | Editing an ACTIVE version → editor opens read-only with "Create v4 draft"; capability set to both Allow in one template and Deny in an ADD_ON → info "Deny wins (DL-07)" | Module Admin sees read-only | "v3 published. 38 bindings still on v2." |
| SCR-TM-01 | "No members yet" | GA grant expired for a member → row amber "Access ceiling removed. Membership will deactivate." (BR-07) | Team tab hidden | — |
| SCR-TM-04 | First condition prompt "Who should this member see?" | No condition → Save disabled, helper "A member with no scope sees nothing" (BR-17); condition referencing previous AY → warning | — | Live preview "Matches 2 sections · 61 students" |
| SCR-TM-05 | "No members to roll over" | Rows without scope highlighted error; apply blocked | — | Summary: "Applied to 36 members, ended 2" |
| SCR-TM-03 Overrides tab | Hidden entirely if overrides are disabled (Decision D09 default) | Override that exceeds GA ceiling → not selectable (BR-16); reason required | — | Override chip on member row |
| SCR-SYS-01 (teacher, day 1 of AY, scope not set) | — | — | "You don't have any classes in *Attendance and Conduct* for AY 2027-28 yet. Kenji Lim manages this team." + Request access | — |

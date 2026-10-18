# S2 — Add a second campus / transfer a teacher

**Persona:** HR Admin → Campus Leaders (target and source campus) → Global Admin → Module Admins
**Screens:** SCR-HR-03 (Campus assignments tab), SCR-HR-06, SCR-GOV-01, SCR-GOV-02, SCR-GA-04, SCR-TM-02, SCR-CL-01
**Rules:** BR-02, BR-03, BR-06, BR-13, BR-14, BR-15 · Decisions D04, D07

**Tóm tắt (VI):** Thay đổi campus là sự kiện HR, **không** tự động thêm hay bớt quyền. Hệ thống mở Affiliation Review: campus mới cần duyệt để cấp quyền, campus cũ được đề xuất gỡ quyền theo ngày hiệu lực.

## Flow

```mermaid
flowchart TD
  A([HR: Profile › Campus assignments › Add campus<br/>SCR-HR-06]) --> B[Campus SG-ECS, reason Additional teaching,<br/>valid from 5 Jan 2027, primary off]
  B --> C{Overlap check BR-14}
  C -- same campus, overlapping dates --> C1[Block: 'Priya already has an active SG-ECS assignment 5 Jan–open.']
  C -- ok --> D[Save → E07 PLANNED until valid_from, then ACTIVE]
  D --> E[[Case AFFILIATION_REVIEW<br/>diff: + SG-ECS · proposed lines: campus grant SG-ECS, memberships copied from SG-PGL as suggestions]]
  E --> F([SG-ECS Campus Leader approves campus line<br/>SCR-GOV-02])
  F --> G([Global Admin applies campus grant])
  G --> H([SG-ECS Module Admins add memberships with SG-ECS scope<br/>SCR-TM-02])
  H --> I[Teacher sees campus switcher: SG-PGL / SG-ECS]

  T([Transfer variant: HR ends SG-PGL valid_to 31 Mar 2027,<br/>sets SG-ECS primary]) --> T1[[Case lines also include REMOVE:<br/>SG-PGL campus grant + 4 memberships, effective 1 Apr 2027]]
  T1 --> T2([SG-PGL Campus Leader confirms removal<br/>or extends with reason, max 30 days])
  T2 --> T3[On 1 Apr: grants EXPIRED, memberships DEACTIVATED automatically]
```

## States — Employee Campus Assignment (E07)

```mermaid
stateDiagram-v2
  [*] --> PLANNED: valid_from in future
  [*] --> ACTIVE: valid_from ≤ today
  PLANNED --> ACTIVE: valid_from reached
  PLANNED --> CANCELLED: HR cancels before start
  ACTIVE --> ENDED: valid_to passed
  ENDED --> [*]
  CANCELLED --> [*]
```

Primary flag: at most one ACTIVE primary per employment is the default but not enforced (Decision D04); if none, the profile shows "No primary campus" (info chip), never an error. Group Office staff may have none.

## Screen states

| Screen | Empty | Error / validation | No access | Success |
|---|---|---|---|---|
| SCR-HR-03 Campus assignments tab | "No campus assignment. Add one so this person appears in campus lists." | — | Campus Leader sees own-campus rows; others collapsed as "+1 other campus" | — |
| SCR-HR-06 | — | `valid_to < valid_from` inline error (BR-13); overlap block (BR-14); setting primary while another is primary → confirm "Make SG-ECS primary and unset SG-PGL?" | Only campuses in HR scope | Persistent notice in drawer footer: "This doesn't change access. An affiliation review will open." Then toast with link to case |
| SCR-GOV-02 (affiliation) | — | Approving a membership line before the campus grant line → line disabled "Needs campus grant first" | Source and target leaders each see only their campus lines | Diff panel: green "+ SG-ECS" / red "− SG-PGL (from 1 Apr)" |
| SCR-CL-01 | "No incoming or outgoing staff this term" | — | — | Card "Incoming staff (2) · Outgoing (1)" |

## Edge cases

- **Ending the last campus assignment while employment is active:** allowed, warning "Priya will have no campus. She won't appear in any campus list."
- **Back-dated assignment** (valid_from in the past): allowed for HR with reason; review case marks effective date "already passed" in amber; access is never back-dated.
- **Cross-country transfer (SG → IN):** different employer → HR is told "This is a new employment with GIIS India" and routed to *End employment* + *Add employment* (Decision D03 default) rather than a campus change.

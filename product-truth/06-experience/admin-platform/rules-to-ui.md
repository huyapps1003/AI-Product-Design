# Admin Platform — rules to UI

**Status:** Proposed, 18 Oct 2026 · Closes gaps G6, G7, G9, G10, G14

**Tóm tắt (VI):** Mỗi business rule (BR-01…BR-21) và mỗi quyết định mở (Decision D01…D15) được dịch thành hành vi UI cụ thể: control nào bị disable, cảnh báo nào hiện, dialog nào chặn, field nào bị ẩn. Với quyết định chưa chốt, thiết kế theo default ghi ở đây và đánh dấu bằng feature flag.

> **ID note.** "Decision D01…D15" = the Data Dictionary's *Unresolved decisions*. "entity D01–D03" = Academic Role Assignment, Teacher Subject Allocation, Timetable Teaching Session. They are different things (see DL-12).

## Reusable UI patterns

Every rule below uses one of these patterns, so designs stay consistent.

| Pattern | Use when | MinV implementation |
|---|---|---|
| **P-BLOCK** inline error | Input violates a hard rule | TextField `error` + helper text; primary button disabled |
| **P-WARN** non-blocking warning | Allowed but unusual; may need a reason | `warning` soft Alert above the field; "Continue" requires reason text if stated |
| **P-NOTICE** consequence notice | Action has an effect elsewhere the user might not expect | `info` soft Alert in drawer/dialog footer |
| **P-REASON** reason dialog | High-impact change (BR-12) | Dialog with reason code select + free text (500 chars), shows "This is recorded in the audit log" |
| **P-PREVIEW** impact preview | Bulk or cascading change | Card listing affected items with +/− diff chips before confirm |
| **P-DISABLED** disabled with why | Option exists but user/target isn't eligible | Disabled row/option + tooltip with the reason + optional "Request" link |
| **P-LOCK** restricted field | Confidential/Restricted field the viewer can't see | Value masked `•••••`, lock icon, tooltip "Visible to HR Admin" |
| **P-SYNC** source badge | Field owned by an external system | Chip `Synced from <source>` + field read-only, "Edit in <source>" link |
| **P-EFFECTIVE** effective dating | Anything with valid_from/valid_to | Two date pickers, "Open-ended" checkbox, timeline preview |
| **P-HISTORY** history not delete | Ending a record | "End…" action instead of "Delete"; ended rows shown greyed under "Show history" |
| **P-NOACCESS** fail-closed state | Missing grant/membership/scope | Empty-state with specific cause + "Request access", never an empty table |

## Business rules (Data Dictionary BR-01…BR-20, plus BR-21)

| Rule | What it says | UI behaviour | Pattern | Screens |
|---|---|---|---|---|
| BR-01 | One immutable employee_id per person | Employee ID shown read-only with copy button in profile "System record"; never editable. Add-employee runs a duplicate-person check after step 1 and offers "Use existing person" | P-WARN | SCR-HR-03, SCR-HR-04 |
| BR-02 | Multiple campus affiliations via separate records | Campus assignments tab is a list with "+ Add campus"; directory shows primary campus + "+N" chip | P-EFFECTIVE | SCR-HR-02, SCR-HR-03, SCR-HR-06 |
| BR-03 | History never overwritten | No delete on employment, org or campus assignments; "End…" action; timeline keeps ended items; edits to a past period need P-REASON | P-HISTORY | SCR-HR-03, SCR-HR-05, SCR-HR-06 |
| BR-04 | Employee may exist without account | Profile Access tab shows "No login account" state with "Create account" (HR) / "Invite" when email exists | P-NOACCESS | SCR-HR-03, SCR-GA-03 |
| BR-05 | Employment and account status independent | Two separate chips in profile header: Employment status, Account status; changing one shows P-NOTICE about the other ("Account stays Active. Suspend it too?") | P-NOTICE | SCR-HR-03, SCR-HR-05, SCR-GA-04 |
| BR-06 | Campus affiliation ≠ access | Footer notice on every campus-assignment form: "This doesn't change access. An affiliation review will open." | P-NOTICE | SCR-HR-04, SCR-HR-06 |
| BR-07 | TM inside GA boundary | Team member pages show the GA ceiling ("Access ceiling: *Attendance and Conduct* at SG-PGL, until open") above memberships; when ceiling expires, memberships show amber "Ceiling removed" | P-NOTICE, P-DISABLED | SCR-TM-01, SCR-TM-03, SCR-GA-04 |
| BR-08 | Deterministic role combination + deny precedence | Effective permissions shows a single result per capability with the explanation chain; Deny entries listed first with "Deny wins" chip | — | SCR-GA-04, SCR-TM-03 |
| BR-09 | Academic assignments owned by Academic Allocation | Academic tab read-only with P-SYNC "Managed in Teaching and Learning" + deep link | P-SYNC | SCR-HR-03 |
| BR-10 | Exit starts access review | End-employment dialog shows impact preview and creates EXIT_REVIEW case; cannot be skipped | P-PREVIEW | SCR-HR-05, SCR-GOV-02 |
| BR-11 | Sensitive fields limited and minimised in exports | Field visibility matrix below; exports default to Internal fields only, Confidential needs an explicit checkbox + P-REASON | P-LOCK, P-REASON | SCR-HR-02, SCR-HR-03, SCR-GOV-04, SCR-GOV-05 |
| BR-12 | High-impact changes audited | P-REASON on: end employment, campus change, account suspend/reactivate, any GA grant, role/scope change, template publish, override; every such screen has an Audit tab | P-REASON | all write screens |
| BR-13 | valid_to ≥ valid_from | Date pickers: end-date picker cannot select before start; manual entry → inline error "End date must be on or after start date" | P-BLOCK | every P-EFFECTIVE form |
| BR-14 | No duplicate overlapping assignments | Saving an overlapping same-campus assignment blocks with link to the existing one | P-BLOCK | SCR-HR-04, SCR-HR-06 |
| BR-15 | TM membership needs active GA module + campus grant at effective time | Person picker in Add member shows ineligible people disabled with reason and "Request access" (creates ACCESS_REQUEST); effective date beyond GA `valid_to` → error | P-DISABLED, P-BLOCK | SCR-TM-02, SCR-TM-05 |
| BR-16 | Override never exceeds GA ceiling | Override capability list only contains capabilities inside the ceiling; whole tab hidden while overrides disabled (Decision D09) | P-DISABLED | SCR-TM-03 |
| BR-17 | Missing scope ≠ global access | Scope is a required step; Save disabled with "A member with no scope sees nothing"; at runtime users with no scope get SCR-SYS-01 with cause "no scope" | P-BLOCK, P-NOACCESS | SCR-TM-02, SCR-TM-04, SCR-TM-05, SCR-SYS-01 |
| BR-18 | Published templates immutable | ACTIVE versions open read-only with "Create v*n+1* draft"; bindings show pinned version + "v3 available" chip | P-DISABLED | SCR-GA-09, SCR-GA-10, SCR-TM-01 |
| BR-19 | Account–employee link uniqueness, history kept | Link account dialog blocks if account already linked to another active employee; link history list in Access tab | P-BLOCK, P-HISTORY | SCR-GA-04, SCR-HR-03 |
| BR-20 | Source conflicts quarantined | Import/sync never overwrites silently; conflicts go to SCR-HR-10 with side-by-side diff; profile shows warning chip "Sync conflict" | P-WARN | SCR-HR-09, SCR-HR-10, SCR-HR-03 |
| **BR-21** (new, proposed) | Reporting lines must not form cycles (from E04 note) | Manager picker excludes the employee and anyone who reports to them; error if a cycle is detected on save | P-BLOCK | SCR-HR-04, SCR-HR-03 |

## Unresolved decisions (Data Dictionary D01…D15) — design defaults

| Decision | Question | Design default (until confirmed) | UI impact | Flag |
|---|---|---|---|---|
| D01 | Source of truth for employee data | Internal master is editable; HRIS mode supported via P-SYNC on synced fields | Same screens; synced fields render read-only with source chip | `hr.source = internal \| hris` |
| D02 | Employee identifiers | UUID global; `employee_number` per employment shown on Employment card | Directory shows employee number of current employment | — |
| D03 | Concurrent employments | Allowed historically; at most one ACTIVE per employer shown as warning if second | Add employment: P-WARN "Priya already has an active employment with GIIS Singapore" | — |
| D04 | Primary campus | Multiple allowed; zero allowed (Group Office); one primary recommended | "No primary campus" info chip; making a second primary asks to unset the first | — |
| D05 | Account linking | 0..1 active account per employee and per account | Link dialog blocks second active link | — |
| D06 | Organization model | Separate catalogs: employer, org unit tree, campus | Three different pickers in the wizard, never merged | — |
| D07 | Campus transfer access | Never silent; AFFILIATION_REVIEW case | S2 flow | — |
| D08 | Permission evaluation | See *Evaluation rules* below | Effective permissions panel | — |
| D09 | Templates and overrides | Overrides **off**; role-template binding on, version-pinned | Overrides tab hidden; designs include it behind the flag | `tm.overrides.enabled` |
| D10 | Lifecycle coordination | Exit → auto-expire at end_date + 1; employment SUSPENDED → account SUSPENDED; rehire never restores | S3 flow | — |
| D11 | Academic allocation | Owned by Teaching and Learning; suggestions only for scope | Academic tab read-only; "Use allocations" button in scope builder | — |
| D12 | History and retention | Keep all history visible to HR; retention purge shown as "Anonymised on …" rows | Timeline shows anonymised entries greyed | — |
| D13 | Field privacy | Matrix below | P-LOCK | — |
| D14 | Scope identity | Global catalog IDs, displayed with campus prefix where per-campus ("SG-PGL · 7B") | Scope chips always show campus prefix in All-campuses mode | — |
| D15 | Status vocabulary | Candidate enums adopted as below | Badge colours | — |

### Evaluation rules (Decision D08 default)

1. No active GA module grant **or** no GA campus grant → **DENIED** (reason: ceiling).
2. No active TM membership for module + campus → **DENIED** (reason: not a member).
3. Collect capabilities from all active role-template bindings (+ overrides if enabled). Any **Deny** → **DENIED**. Otherwise any **Allow** → candidate ALLOWED. None → **DENIED** (reason: not in template).
4. Data scope: conditions inside a group are AND; groups are OR. No policy or no groups → **NO SCOPE** (treated as denied, BR-17).
5. Result shown as `ALLOWED` / `DENIED` / `NO SCOPE` with the first failing link highlighted.

## Status vocabulary

One label and colour per status, used in chips (MinV Label `soft`, size sm). Candidate enums from the Data Dictionary (Decision D15).

| Object | Status → label · colour |
|---|---|
| Employee record (E01) | ACTIVE → Active · success · INACTIVE → Inactive · default · ARCHIVED → Archived · default |
| Employment (E02) | PENDING → Starts {date} · info · ACTIVE → Active · success · SUSPENDED → Suspended · warning · ENDED → Ended · default · (derived) ACTIVE with end_date ≤ 30 d → Leaving {date} · warning |
| Org / campus assignment (E04, E07) | PLANNED → Planned · info · ACTIVE → Active · success · ENDED → Ended · default · CANCELLED → Cancelled · default |
| User account (A01) | INVITED → Invited · info · ACTIVE → Active · success · SUSPENDED → Suspended · warning · DISABLED → Disabled · error · CLOSED → Closed · default |
| GA grant (A03, A04) | ACTIVE → Active · success · REVOKED → Revoked · error · EXPIRED → Expired · default |
| TM membership (A05) | INVITED → Invited · info · ACTIVE → Active · success · DEACTIVATED → Deactivated · warning · REMOVED → Removed · default |
| Template (A08) | DRAFT → Draft · info · ACTIVE → Published v{n} · success · RETIRED → Retired · default |
| Sync (E08) | PENDING → Syncing · info · SYNCED → Synced · success · FAILED → Sync failed · error · CONFLICT → Conflict · warning |
| Case | OPEN → New · info · IN_REVIEW → In review · secondary · WAITING → Waiting for info · warning · ESCALATED → Overdue · error · APPROVED/APPLIED → Done · success · REJECTED → Rejected · error · CANCELLED → Withdrawn · default |
| Effective permission | ALLOWED · success · DENIED · error · NO SCOPE · warning |

`default` = text secondary on background neutral. Never use colour alone: every chip has a text label.

## Field visibility (Decision D13)

Architecture tiers and dictionary sensitivity mapped as: **Tier 1 = Internal**, **Tier 2 = Confidential**, **Tier 3 = Restricted** (DL-11).

`E` edit · `V` view · `M` masked (P-LOCK) · `—` hidden

| Field group (examples) | Sensitivity | HR Admin | Global Admin | Campus Leader | Module Admin | Teacher (self) | Teacher (others) |
|---|---|---|---|---|---|---|---|
| Name, preferred name, work email, work phone, position, campus | Internal | E | V | V | V (members) | E (preferred name, phone) / V | V (directory) |
| Legal names, photo | Confidential | E | V | V | M | V | M (photo shown if published) |
| Employee number, employer, employment type, start/end date | Internal | E | V | V | — | V | — |
| Termination reason | Confidential | E | — | V (own campus) | — | — | — |
| Manager / reporting line | Internal | E | V | V | — | V | — |
| Account login, IdP, last login | Internal | V | E | V | V (members) | V | — |
| GA grants, memberships, scope | Internal | V | E | V | E (own module) | V | — |
| Audit before/after snapshots | Restricted | M (HR events: V) | V | M | M | — | — |

## Architecture-level rules that affect admin screens

| Rule (Architecture) | UI behaviour |
|---|---|
| Least privilege by scope; cross-campus access time-boxed and logged | Any grant across countries requires `valid_to` (max 90 days) and P-REASON |
| Tier 3 visible only to named roles, every read logged | Capabilities touching Tier 3 marked with a lock chip in SCR-GA-07/10 and flagged in SCR-GOV-03 |
| MFA for staff | SCR-AUTH-01 relies on Google Workspace MFA; account detail shows "MFA enforced by Google Workspace" |
| Every staff screen shows tenant scope | Scope switcher always visible (03 § Shell) |
| AI features keep a human approver, never send Tier 3 to a model | Any AI suggestion (e.g. suggested access bundle) is labelled "Suggested" and needs an explicit approve |

# 07 — Effective RBAC model for Global Campus OS

**Status:** Design direction (working assumption), 18 Oct 2026 · Case study for the Admin Platform design · Decision records: DL-16…DL-21

**Tóm tắt (VI):** Mô hình RBAC đề xuất gồm 6 lớp: grant hierarchy (thay GA phẳng), operational membership (giữ TM), derived scope (từ phân công giảng dạy), relationship-based access (gia đình), record-level ACL (case/Tier 3), và delegation. Giữ nguyên GA/TM entities, mở rộng thêm 3 thành phần.

> This model builds on the Data Dictionary's GA/TM architecture. It does not replace GA/TM — it adds three layers (derived scopes, relationship-based access, record-level ACL) and restructures grants as a hierarchy. Everything here is design direction until confirmed per the [06 process](./06-decision-log.md#how-design-direction-becomes-architecture).

---

## Why the current model is not enough

The GA/TM model (A03/A04 ceiling → A05 membership → A07 role → A08 template → A12/A13 scope) handles the staff admin platform well. Seven gaps appear when we extend to the full Campus OS:

| # | Problem | Example | Impact |
|---|---|---|---|
| 1 | **Flat grants, no hierarchy** | A new campus needs manual grants for every staff member who should access it | O(users × campuses) admin work; delays campus launches |
| 2 | **Membership explosion** | A teacher on 2 campuses × 4 modules = 8 memberships, each with scope | Module Admins manage hundreds of rows; drift is inevitable |
| 3 | **Manual scope vs. allocations** | Academic Allocation says Priya teaches 7B Maths, but her scope must be set separately | Scope drifts from reality every term |
| 4 | **No relationship-based access** | A parent needs to see their child's attendance — no grant/membership model covers "my child" | Parent/student apps can't launch on GA/TM alone |
| 5 | **No record-level access** | A discipline case should be visible only to assigned committee members, not all Campus Leaders | Tier 3 data leaks to anyone with the capability |
| 6 | **No delegation** | Principal on leave; VP needs to approve access requests temporarily | VP either gets permanent grants or work stalls |
| 7 | **Cell boundary evaluation** | Singapore data must be evaluated in SG cell; a global report must not pull raw Tier 3 from IN cell | Current model has no evaluation-location concept |

---

## Proposed six-layer model

```
┌─────────────────────────────────────────────────────────────┐
│  Layer 6: Policy evaluation (per cell + global index)       │
├─────────────────────────────────────────────────────────────┤
│  Layer 5: Record-level ACL (case assignee, Tier 3 holders) │
├─────────────────────────────────────────────────────────────┤
│  Layer 4: Relationship-based access (parent→child link)     │
├─────────────────────────────────────────────────────────────┤
│  Layer 3: Data scope (static + derived from allocations)    │
├─────────────────────────────────────────────────────────────┤
│  Layer 2: Operational membership (TM: role + template)      │
├─────────────────────────────────────────────────────────────┤
│  Layer 1: Hierarchical grant ceiling (replaces flat GA)     │
└─────────────────────────────────────────────────────────────┘
```

Each layer is evaluated top-down; a request must pass every layer to succeed. Fail at any layer = denied.

---

### Layer 1 — Hierarchical grant ceiling

**Replaces:** flat A03 module grant + A04 campus grant.
**Keeps:** A03/A04 entities, adds a `scope_node` field and inheritance rules.

| Concept | Current GA | Proposed |
|---|---|---|
| Grant target | One module + one campus per grant | One module + one **scope node** (Group, Country, Campus) |
| Inheritance | None | Grant at Country inherits to all campuses in that country; Group inherits to all countries |
| New campus | Manual grant per user | Automatically covered by parent node grant |
| Cross-campus | Separate grants per campus | One Country-level grant covers all campuses |
| Time-boxing | `valid_from`/`valid_to` exists | Same; cross-country grants max 90 days (architecture rule) |

**Scope node hierarchy:**

```
Group (GRP-HQ)
├── Country: Singapore (SG)
│   ├── Campus: SG-PGL (Punggol)
│   └── Campus: SG-ECS (East Coast)
├── Country: India (IN)
│   └── Campus: IN-NOI (Noida)
└── (future countries)
```

**Rules:**
- A grant at node N for module M means the user *may* hold memberships in module M at N or any descendant of N.
- Revoking a parent grant cascades: memberships at child nodes are deactivated with reason `CEILING_REMOVED`.
- `GLOBAL_ADMIN` is implicitly granted at Group for `global_admin` module (no separate grant needed).
- The four-eyes rule (DL-09) still applies: granting at Country or Group level needs a second Global Admin.

**Data Dictionary changes:** Add `scope_node_type` (GROUP/COUNTRY/CAMPUS) and `scope_node_id` to A03 and A04. Existing per-campus grants map to `scope_node_type = CAMPUS`.

---

### Layer 2 — Operational membership (TM)

**Keeps:** A05 membership, A06 role definition, A07 role assignment, A08/A09 templates, A10 bindings. No structural changes.

**Refinements:**

| Area | Change | Why |
|---|---|---|
| Template inheritance | Templates can be marked `BASE` (shared across modules) or `MODULE` (module-specific); a binding resolves BASE + MODULE, union of allows, deny wins | Reduces template duplication; a "Teacher base" template covers common capabilities |
| Bulk membership | Module Admin can "add from allocation" — system creates memberships for everyone in the academic allocation for that module + campus + AY | Fixes problem #2; Module Admin reviews and confirms rather than creating one by one |
| Version migration | AY rollover (S4) can bulk-migrate template versions with preview | Already designed in S4; confirmed as the standard mechanism |

---

### Layer 3 — Data scope (static + derived)

**Keeps:** A12 scope policy, A13 scope condition. **Adds:** derived scope sources.

| Scope type | Source | Example | Refreshed |
|---|---|---|---|
| **Static** (current) | Module Admin sets conditions manually | `GRADE = 6,7,8` AND `SUBJECT = Mathematics` | Manual; AY rollover |
| **Derived: allocation** | Academic Allocation (entity D02) | "Sections and subjects from Priya's AY 2027-28 timetable" | Automatic when allocation changes; Module Admin confirms or auto-applies per module setting |
| **Derived: org unit** | E04 org assignment | "Staff in my department" for HR Viewer | Automatic when org assignment changes |
| **Derived: relationship** | Family link (Layer 4) | "My children's sections" | Automatic when enrolment changes |

**Rules:**
- Derived scope is a **suggestion** by default; Module Admin confirms it (DL-16).
- A module can opt into **auto-apply** for derived scopes (flag `tm.scope.auto_derive = true`), where changes apply immediately and Module Admin is notified for review.
- Static and derived conditions combine: static conditions are AND with derived; multiple derived sources are OR.
- `NO SCOPE = NO DATA` (BR-17) still holds; a derived scope that resolves to empty is flagged.

**Data Dictionary changes:** Add `source_type` (STATIC/ALLOCATION/ORG_UNIT/RELATIONSHIP) and `source_ref` to A13 scope condition. Add module-level setting `tm.scope.auto_derive`.

---

### Layer 4 — Relationship-based access

**New layer.** Not needed for the Admin Platform (v1 case study), but the model must accommodate it for Parent and Student apps.

| Concept | Description |
|---|---|
| **Family link** | A verified link between a parent account and a student enrolment (from Admissions module). Not a grant or membership. |
| **Implied capabilities** | A parent with a family link can read their child's attendance, grades, communication, and cases — scoped to that child only. |
| **No grant needed** | The family link itself is the authorization; no GA grant or TM membership is created. |
| **Boundary** | A parent sees only their own children. A parent who is also staff has two separate access paths (staff via Layers 1–3, parent via Layer 4). |
| **Revocation** | Withdrawal or enrolment end removes the link and access. Custody changes update the link via HR/Admissions. |

**How it evaluates:**
1. Is the requester a parent with an active family link to a student? → Yes: proceed.
2. Is the requested record about that student? → Yes: proceed.
3. Is the capability in the parent-allowed set (read attendance, read grades, read communication, view cases assigned to them)? → Yes: ALLOWED.
4. Otherwise: DENIED (fall through to Layers 1–3 if the requester is also staff).

**Data Dictionary changes:** New entity F01 Family Link (`family_link_id`, `parent_account_id`, `student_enrolment_id`, `relationship_type`, `status`, `valid_from`, `valid_to`). New reference R03 Parent Capability Set (read-only list of capabilities available through family links).

---

### Layer 5 — Record-level ACL

**New layer.** Needed for cases, Tier 3 fields, and cross-module workflows.

| Use case | ACL mechanism |
|---|---|
| **Case access** | Case object already has `assigned_to` and `committee_members`. Proposal: only the assignee, committee members, and users with `gov.case.view_all` can read the case. Others see "Case exists" but not content. |
| **Tier 3 fields** | Fields marked `sensitivity = RESTRICTED` are visible only to users with an explicit Tier 3 grant for that record type + scope. Every read is logged (architecture rule). |
| **Cross-module workflow** | A case step assigned to a user in another module gives that user read access to the case record for the duration of the step, without a module membership. |

**Rules:**
- Record-level ACL is checked **after** Layers 1–3. Having the capability is necessary but not sufficient for Tier 3 and cases; the ACL narrows further.
- ACL entries are time-scoped (for workflow steps) or permanent (for case committee).
- No one can grant themselves record-level access; it flows from case assignment or explicit Tier 3 grants by Global Admin.

**Data Dictionary changes:** Add A15 Record ACL Entry (`acl_id`, `record_type`, `record_id`, `grantee_account_id`, `access_level` READ/WRITE, `reason`, `valid_from`, `valid_to`, `granted_by`). Extend Case object with `visibility = RESTRICTED | NORMAL`.

---

### Layer 6 — Policy evaluation by cell

**New layer.** Ensures data residency compliance.

| Concept | Rule |
|---|---|
| **Cell** | Each country (or region) runs its own evaluation engine with its own data store (architecture: SG cell, IN cell). |
| **Identity is global** | Account (A01), grants (A03/A04), and role definitions (A06) are replicated to every cell from the global index. |
| **Data is local** | Employee records, memberships, scope, cases, and academic data live in the country cell where the campus belongs. |
| **Cross-cell query** | A user with a Group-level grant can query multiple cells, but each cell evaluates locally and returns only what the user may see. Tier 3 data never leaves the cell in raw form. |
| **Aggregated views** | Global dashboards show aggregated counts/metrics, not individual records, unless the user has cell-level access to the underlying data. |

**No Data Dictionary entity changes** — this is an infrastructure concern, not a data model change. The evaluation algorithm (DL-07) runs identically in each cell.

---

## Evaluation algorithm (updated from DL-07)

For a request: *user U wants capability C on record R in module M at campus X:*

```
1. CELL       → Route to the cell that owns campus X.
2. CEILING    → U has an active grant for module M at scope node ≥ campus X?
                 No → DENIED (reason: no grant).
3. MEMBERSHIP → U has an active TM membership for M @ X?
                 No → DENIED (reason: not a member).
4. CAPABILITY → Collect capabilities from all role-template bindings
                 (+ overrides if enabled).
                 Any DENY for C → DENIED (deny wins).
                 No ALLOW for C → DENIED (reason: not in template).
5. SCOPE      → Evaluate static + derived scope conditions against R.
                 No matching scope → NO SCOPE (denied, BR-17).
6. RECORD ACL → If R is a Case with visibility RESTRICTED
                 or R contains Tier 3 fields:
                 U in ACL for R? No → DENIED (reason: restricted record).
                 Every Tier 3 read → audit log entry.
7. RESULT     → ALLOWED.
```

**Relationship-based access (Layer 4) is a separate path:**

```
1. FAMILY LINK → U has active family link to student S?
                  No → fall through to staff path above.
2. RECORD      → R belongs to student S?
                  No → DENIED.
3. CAPABILITY  → C in parent capability set (R03)?
                  No → DENIED.
4. RESULT      → ALLOWED (scoped to S only).
```

If a user is both parent and staff, the system evaluates both paths; either ALLOWED is sufficient.

---

## What changes vs. current GA/TM

| Component | Current | Proposed | Breaking? |
|---|---|---|---|
| A03/A04 grants | Flat (module × campus) | Hierarchical (module × scope node) | Additive: campus grants still work; hierarchy is opt-in |
| A05–A10 memberships | Manual creation | Manual + "add from allocation" bulk | Additive |
| A12/A13 scope | Static conditions only | Static + derived sources | Additive: `source_type = STATIC` is the default |
| Family access | Not modelled | New entity F01 + capability set R03 | New; no impact on staff path |
| Record-level ACL | Not modelled | New entity A15 | New; only restricts further, never expands |
| Cell evaluation | Implied by architecture | Explicit in evaluation algorithm | Clarification, not change |
| Templates | One category | BASE + MODULE categories | Additive; existing templates become MODULE |

**Key design principle:** every change is additive or further-restricting. No existing ALLOWED decision becomes DENIED under the new model unless a record-level ACL is added (which is a deliberate restriction).

---

## v1 case study scope

For the Admin Platform design (DL-01), we design with **Layers 1–3 and Layer 5 (cases only)**:

| Layer | In v1 scope? | Why |
|---|---|---|
| 1. Hierarchical grants | **Yes** — design the UI to show scope nodes, but all sample data uses campus-level grants | The UI should not break when a Country grant appears |
| 2. TM membership | **Yes** — full design | Core of the admin platform |
| 3. Data scope (static) | **Yes** — full design | Core of the admin platform |
| 3. Data scope (derived) | **Yes** — "suggested from allocations" in SCR-TM-02/04 | Already in S1 and S4 flows |
| 4. Relationship-based | **No** — Parent app is out of v1 scope | Document the model; design later |
| 5. Record-level ACL (cases) | **Yes** — case visibility in SCR-GOV-01/02 | Already implied by Case flows |
| 5. Record-level ACL (Tier 3) | **Partial** — P-LOCK treatment designed, ACL UI deferred | P-LOCK is in 05-rules-to-ui.md |
| 6. Cell evaluation | **No** — infrastructure concern | No UI impact for v1 |

---

## Screen impact summary

| Screen | Change from current design | Layer |
|---|---|---|
| SCR-GA-03 (grants) | Grant form shows scope-node picker (Group/Country/Campus) instead of just campus picker; inheritance preview | L1 |
| SCR-GA-04 (user access) | Effective permissions panel shows which scope node the grant comes from; "Inherited from SG" chip | L1 |
| SCR-TM-02 (add member) | "Add from allocations" button; scope step pre-filled from D02 with "Suggested" chip | L2, L3 |
| SCR-TM-04 (scope builder) | Source toggle: Static / From allocations / From org unit; live preview | L3 |
| SCR-TM-05 (AY rollover) | Derived scope column shows allocation changes; auto-apply option per module | L3 |
| SCR-GOV-01 (task inbox) | Case rows show visibility badge (Normal / Restricted); restricted cases hidden from non-ACL users | L5 |
| SCR-GOV-02 (case detail) | Committee members list; "You can see this case because you are assigned to it" notice | L5 |
| SCR-GA-07 (capabilities) | Tier 3 capabilities marked with lock chip; tooltip "Record-level ACL required" | L5 |

---

## Mapping to architecture open questions

| Architecture question | This model's answer |
|---|---|
| **Q9: How role + scope maps to GA/TM** | Layers 1–3: hierarchical grants → membership → role + template → static/derived scope |
| **D08: Permission evaluation** | Six-layer algorithm above; deny wins; fail closed |
| **D09: Overrides** | Unchanged — off by default (DL-14); Layer 2 refinement |
| **D11: Academic allocation** | Layer 3 derived scope from entity D02; suggestion by default, auto-apply opt-in |
| **D13: Field privacy** | Layer 5 record-level ACL for Tier 3; P-LOCK for UI |
| **D14: Scope identity** | Global catalog IDs; scope node hierarchy adds GROUP/COUNTRY levels |

---

## Decisions introduced

| ID | Decision | Status |
|---|---|---|
| DL-16 | Derived scope is a suggestion by default; auto-apply is opt-in per module | Assumed |
| DL-17 | Grants use a scope-node hierarchy (Group > Country > Campus), not flat campus-only | Assumed |
| DL-18 | Relationship-based access for parents uses family links, not grants or memberships | Assumed |
| DL-19 | Record-level ACL for cases (visibility RESTRICTED) and Tier 3 fields | Assumed |
| DL-20 | Templates have BASE and MODULE categories; binding resolves both | Assumed |
| DL-21 | Time-boxed delegation: acting grants with reason, max duration, and audit | Assumed |

See [06-decision-log.md](./06-decision-log.md) for full entries.

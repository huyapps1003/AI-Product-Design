# Access models (RBAC)

**Status:** Mixed: GA/TM Known (Data Dictionary), six-layer model Assumed (DL-16…DL-21), platform and brand access Open, 25 Oct 2026 · **Owner:** Henry
**Cites:** [Architecture v1 § Users and roles](../00-sources/Global_Campus_OS_Product_Architecture_v1.md#users-and-roles), [§ Security](../00-sources/Global_Campus_OS_Product_Architecture_v1.md#security-privacy-and-compliance) · Data Dictionary A01–A14 · DL-02, DL-07, DL-09, DL-16…DL-21 · RES-F06 · research §08
**Tóm tắt (VI):** Tổng quan các mô hình truy cập: ai được vào bằng con đường nào, phần nào đã biết, phần nào là giả định, phần nào còn mở. Không đề xuất mô hình mới cho các khoảng trống.

## Files

| File | Answers |
|---|---|
| this README | Which access paths exist for which actors, and the constraints every path must meet |
| [role-catalogue.md](./role-catalogue.md) | Every role name in product truth, how they relate, and the role families with no definition |
| [rbac-model.md](./rbac-model.md) | The six-layer model and evaluation algorithm (Assumed, DL-16…DL-21) |

## Three sources, one model

| Source | What it defines | Status |
|---|---|---|
| Architecture v1 | Access is role plus scope. 17 school roles granted at a level of Group › Country › Campus › Section; permissions flow down | Proposed |
| Employee Data Dictionary v1 | Two layers: Global Admin (GA) grants set a ceiling of modules × campuses; Team Management (TM) membership carries a role, a permission template and a data scope | Confirmed input |
| [rbac-model.md](./rbac-model.md) | Six layers: hierarchical grants, operational membership, static and derived scope, relationship-based access, record-level ACL, evaluation per country cell | Assumed |

DL-02 (Assumed) records how the first two relate: architecture "role + scope" = GA grant (ceiling) + TM membership with role and data scope.

## Access paths by actor

Which way each kind of actor gets access, as far as product truth defines it. The model was written for one brand's campuses (RES-F06).

| Actor | Access path | Status | Gaps |
|---|---|---|---|
| School staff (teachers, operations, leaders) | GA grant → TM membership → role + template → data scope (Layers 1–3), record ACL for restricted cases (Layer 5) | Known (GA/TM); Assumed (layers) | — |
| Central functions (Group HR, Global Admin, auditor) | Same path with group-level grants; system roles `GLOBAL_ADMIN`, `PLATFORM_AUDITOR` | Assumed (DL-17, A06 values) | DPO has no role (OQ-34) |
| Acting staff (cover, leave) | Time-boxed delegation | Assumed (DL-21) | — |
| Parents and guardians | Family link to a student enrolment; fixed read-only capability set (Layer 4) | Assumed (DL-18) | How custody is recorded (OQ-11); consent (OQ-10) |
| Students | "Self" scope in Architecture v1 | Known (scope only) | No access path defined (OQ-19) |
| Brand leadership | None | Open | No brand level in the hierarchy (OQ-14, OQ-16) |
| GSG platform team (admins, support, security) | None | Open | No platform-operator or support roles (OQ-15) |
| Franchise / JV partner staff | None | Open | OQ-19, OQ-03 |
| Group staff working on another country's data | Layer 6: data stays in its cell; each cell evaluates locally | Assumed | How cross-cell access is approved (OQ-18) |
| Regulators and other recipients by law | Not users; receive disclosures and exports | Known (Architecture § Integrations) | Recipients outside SG and IN (OQ-22) |

## Constraints every access path must meet (Known or Assumed)

| Constraint | Status | Source |
|---|---|---|
| Campus affiliation, HR position and academic role never grant access | Known | BR-06; [roles-personas](../02-users/roles-personas.md) |
| Deny wins; fail closed; no scope = no data | Assumed / Known | DL-07; BR-17 |
| HR edits people, Global Admin edits the access ceiling, Module Admin edits teams; granting `GLOBAL_ADMIN` needs a second Global Admin | Assumed | DL-09 |
| Least privilege by scope: a Grade 4 class teacher sees their section, not the campus | Known | Architecture § Security |
| Cross-campus access is granted, time-boxed and logged | Known | Architecture § Security |
| Tier 3 data is visible only to named roles and every read is logged | Known | Architecture § Security; DL-19 |
| Child protection and discipline cases are restricted to designated officers and committees | Known | Architecture § Security; GIIS Handbook |
| Transfers, rehires and campus changes create a review, never access | Assumed | BR-06, Decision D07 default; principle 3 in the [Admin Platform brief](../06-experience/admin-platform/README.md#principles-for-designers) |
| Every access change has an audit event and actor | Known | A14; Admin Platform success metrics |

## What the research found the model doesn't cover

From research §08. Each is an open question, not a design.

| Finding | Open |
|---|---|
| GSG's platform team operates a system that holds every brand's data; the PowerSchool breach began with a vendor support credential | OQ-15 |
| Brand leadership works across countries but only within one brand | OQ-14, OQ-16 |
| One country can host campuses of several brands and entities | OQ-17 |
| Campuses change hands through acquisition; partners can leave | OQ-04 |
| Access to children's data needs a lawful purpose, and sometimes consent | OQ-09, OQ-10 |
| Group staff may need data held in another country | OQ-18 |

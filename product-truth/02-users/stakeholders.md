# Users and stakeholders

**Status:** Known (from desk research; no users interviewed), 25 Oct 2026 · **Owner:** Henry
**Cites:** research §03 · [Architecture v1 § Users and roles](../00-sources/Global_Campus_OS_Product_Architecture_v1.md#users-and-roles) · [roles-personas.md](./roles-personas.md)
**Tóm tắt (VI):** Ai dùng Campus OS và ai đặt ra yêu cầu, trên toàn bộ GSG. Persona chi tiết hiện chỉ có cho Admin Platform; các nhóm khác chưa có persona (OQ-34).

No users have been interviewed. Segments come from internal GIIS documents, product truth and public sources. Every persona is therefore an assumption until user research happens.

## People who use the platform

| Segment | Who | How often, where | What the group structure means for them | Persona today | Evidence |
|---|---|---|---|---|---|
| Families | Parents and guardians, students | Daily or weekly, mostly mobile | Children can be on different campuses; transfers move them between campuses and countries | None (Parent app out of v1) | Handbook: MyGIIS features; transfer only through MyGIIS |
| Teaching staff | Class and subject teachers, coordinators | Every period, web and mobile | Some teach across campuses and programmes | P5 Teacher | Handbook coordinator lists |
| Campus operations | Admissions, finance, transport, nurse, counsellors, security, CCA | All day, desktop | Routes, fees and contacts are specific to each campus | Users who hold memberships, not personas | Handbook contact matrix |
| Leadership | Principals, VPs, Country Directors | Weekly, plus approvals between classes | Approvals with deadlines; answerable to regulators | P3 Campus Leader | Refunds approved by Country Director and Finance Controller; complaints escalate at 14 and 21 days |
| Brand leadership | Brand heads, brand academic leads | Weekly, desktop | Each brand has its own curricula, identity and policies across countries | None (OQ-16, OQ-34) | 12 brands; acquired schools keep their identity |
| Central functions | Group HR, Global Admin, IT, group academic leads, DPO | Daily, desktop | Need to answer "who can see what, and why" across the group | P1 HR Admin, P2 Global Admin (DPO: none) | Product truth personas |
| Platform operator | GSG's platform team: admins, support, security | Continuous | Operate one platform that holds every brand's data | None (OQ-15, OQ-34) | Henry: GSG runs Campus OS |

## People who decide or impose requirements

| Group | Who | What they can do |
|---|---|---|
| Sponsors | GSG group leadership | Set direction and fund it. GSG's site emphasises shared curricula, a global community and "seamless transfers" |
| Gatekeepers | Brand heads, Principals, Country Directors; IT (identity, devices); DPOs and legal (consent, residency); programme coordinators (results release) | Can block adoption |
| Regulators and boards | Singapore: CPE/SSG (EduTrust), ICA (Student Pass), PDPC. India: CBSE (APAAR, list of candidates), Data Protection Board. IB and Cambridge | Set non-negotiable rules ([regulatory context](../05-data-governance/regulatory-context.md)) |
| Partners and vendors | Franchise and JV operators, payment gateways, insurers (FPS, medical), Google Workspace, transport and LMS vendors | Exchange data with the schools ([data-controller model](../05-data-governance/data-controller-model.md)) |

## Observations that shape design

- **One person, several roles.** One Person can be a parent on one campus and staff on another (Architecture § High-level data model).
- **The family that moves is a network-specific user.** Transfers need one month's notice and no pending dues; both principals approve, with acknowledgement in 2 working days and a decision in 3 weeks; no change of course in Grades 9–10 or 11–12 (GIIS Handbook). GSG promises "seamless transfers" but its FAQ answer on transfers is empty. How data moves between entities on transfer is open (OQ-07).
- **Teachers act in short gaps.** Attendance, leave approvals and parent messages happen between periods.
- **Principals carry regulatory risk.** They approve leave over 2 days, suspensions and transfers, and handle complaint escalation.

## Where roles and personas are defined

- School roles (17, from the GIIS Handbook), access roles (A06) and missing role families: [`../04-access/role-catalogue.md`](../04-access/role-catalogue.md).
- Admin Platform personas P1–P5 mapped to GA/TM: [`roles-personas.md`](./roles-personas.md).

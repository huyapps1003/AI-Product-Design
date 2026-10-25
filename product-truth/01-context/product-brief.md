# Product brief: Global Campus OS

**Status:** Known (scope) and Proposed (architecture), 25 Oct 2026 · **Owner:** Henry
**Cites:** DL-22 · RES-F01…F07 · [Architecture v1 § Summary](../00-sources/Global_Campus_OS_Product_Architecture_v1.md#summary)
**Tóm tắt (VI):** Campus OS là một nền tảng do GSG vận hành cho tất cả các thương hiệu trường của GSG. File này nêu những gì đã biết về phạm vi, vấn đề và mức độ product truth đang bao phủ; những gì chưa biết nằm ở `08-open-questions/`.

This is the platform-level brief. Each design slice has its own brief in [`../06-experience/`](../06-experience/README.md).

## Who runs it and for whom

| Statement | Status | Source |
|---|---|---|
| Global Schools Group (GSG) runs Campus OS as the platform operator | Known | Henry, 25 Oct 2026 (DL-22) |
| Campus OS serves **all GSG school brands**, not only GIIS | Known | Henry, 25 Oct 2026 (DL-22) |
| GSG has 12 school brands and claims 64 campuses in 11 countries; GIIS, the largest brand, claims 35 campuses in 10 countries | Known (public claims; figures disagree with each other) | RES-F01; [operating context](./operating-context.md) |
| SmartLearn is out of scope | Known | Henry, 25 Oct 2026 |
| Which campuses, brands and countries are in the first release | Open | OQ-05 |

## What the platform is (Architecture v1, Proposed)

One multi-tenant platform where every campus is configuration, not a fork. The architecture rests on five bets:

1. **Campus as configuration.** A Group › Country › Campus › Section hierarchy with settings that inherit downward.
2. **Programme packs.** Curricula load as data (levels, subjects, grade scales, rules).
3. **Policy as rules.** School policies (attendance, promotion, refunds, SLAs) are versioned rules the system enforces.
4. **Calendar engine.** Different academic years, terms and working weeks per campus.
5. **Evolve MyGIIS, don't replace it overnight.** GIIS's parent portal becomes the first release of the parent app.

It has 4 experience surfaces (Parent app, Student app, Staff app and web, Leadership views), 12 campus modules and 8 shared core services; see [`../03-domain/modules-and-services.md`](../03-domain/modules-and-services.md).

The research compared each bet with the evidence: bets 1–4 are consistent with it; bet 5 covers GIIS only, because the other 11 brands' portals haven't been researched (research §10; OQ-27).

## The problem, as the evidence describes it

| # | What is known | Evidence |
|---|---|---|
| 1 | The group is assembled, not uniform. Since 2021 GSG has grown mostly by acquiring schools that bring their own systems, curricula and identities. Some GIIS campuses opened through local partners. The employer, the school brand and the legal owner of a campus's data can be different parties. | RES-F02 |
| 2 | Differences between campuses are structural: six curricula on one campus, academic years starting in April and mid-year, different working weeks, regulators and currencies per country. | RES-F03; MC-01…MC-15 |
| 3 | Group-level control is now a standard vendor feature, but no vendor reviewed combines CBSE, IB and Cambridge with Singapore and Indian regulator workflows and per-country residency. | RES-F04 |
| 4 | Rules on children's data are tightening on a set timeline (DPDP core obligations around May 2027; PDPC children's guidelines since March 2024; APAAR for CBSE). | RES-F05 |
| 5 | The access model has no answer yet for platform-level or brand-level access. | RES-F06 |
| 6 | Who controls the data has not been defined. | RES-F07 |

## How much product truth covers today

| Area | Coverage | Where |
|---|---|---|
| Brands | GIIS only | Architecture v1; sample data (DL-03) |
| Campuses and countries | 3 campuses (SG-PGL, SG-ECS, IN-NOI) plus GRP-HQ, in 2 countries | Architecture v1; [admin-platform sample data](../06-experience/admin-platform/README.md#canonical-sample-data) |
| Users and roles | 17 school roles; 5 admin personas; no platform-operator or brand roles | [`../02-users/`](../02-users/stakeholders.md), [`../04-access/role-catalogue.md`](../04-access/role-catalogue.md) |
| Access model | Six layers, all Assumed (DL-16…DL-21), written for one brand | [`../04-access/`](../04-access/README.md) |
| Data ownership | Employer per employment only; no data controller | [`../05-data-governance/data-controller-model.md`](../05-data-governance/data-controller-model.md) |
| Regulation | Singapore and India only | [`../05-data-governance/regulatory-context.md`](../05-data-governance/regulatory-context.md) |
| Design-ready slices | Admin Platform (DL-01, Assumed) | [`../06-experience/admin-platform/`](../06-experience/admin-platform/README.md) |

## Open questions that shape the whole platform

OQ-01 (operating entity) · OQ-05 (first release scope) · OQ-06 (data controller) · OQ-14 (brand in the hierarchy) · OQ-15 (platform-operator access) · OQ-26 (brand identity in shared surfaces) · OQ-30 (build, buy or hybrid). Full list: [`../08-open-questions/open-questions.md`](../08-open-questions/open-questions.md).

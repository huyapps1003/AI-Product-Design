---
id: RES-2026-10-25-foundation
title: Global Campus OS for Global Schools Group — foundation research
type: research (findings only)
status: v3 · 25 Oct 2026
owner: Henry
scope: All GSG brands; GSG runs Campus OS
reads_with: product-truth/Global_Campus_OS_Product_Architecture_v1.md, product-truth/00–07
published_view: https://claude.ai/artifact/TvZwexKQU4Kj5HqhYyRbcG
---

# Global Campus OS for Global Schools Group: what we know

> **Research, not product truth.** This file records evidence and gaps. It proposes no solutions and never overrides `product-truth/`. Cite findings by ID (for example `RES-F06`, `MC-09`) when a design or decision depends on them.

GSG runs one Campus OS for all of its brands. This report records what the research found about the platform's users, the differences between brands, campuses and countries, the market, regulation, access control and data ownership. It describes the problem space only and proposes no solutions.

## 00 · Summary

These are the findings that matter most for the product architecture. Each is backed by a source in the sections below.

1.  **RES-F01 · Product truth covers a small part of the scope.** Architecture v1 and its sample data describe one brand (GIIS), 3 campuses and 2 countries. GSG runs Campus OS for 12 brands and publicly claims 64 campuses in 11 countries. GIIS alone claims 35 campuses in 10 countries.
2.  **RES-F02 · The group is assembled, not uniform.** Since 2021 GSG has grown by acquiring schools that come with their own systems, curricula and identities. GIIS opened some of its Thailand and India campuses through local partners, as joint ventures or franchises. The employer, the school brand and the legal owner of a campus's data can therefore be different parties.
3.  **RES-F03 · Differences between campuses are large and structural.** Within GIIS alone the files show six curricula on one campus, academic years starting in April and in mid-year, Saturdays as non-working days in Noida, and separate regulators and currencies in each country.
4.  **RES-F04 · Group-level control is now a standard vendor feature.** iSAMS launched Central Manage for school groups in March 2026, Arbor sells trust-wide "pushdowns" to over 925 UK trusts, and Engage is the single source of truth for an 18-school group in four countries. No vendor we reviewed showed CBSE, IB and Cambridge in one data model together with Singapore and Indian regulator workflows and per-country data residency.
5.  **RES-F05 · The rules on children's data are tightening on a set timeline.** India's DPDP Rules were notified on 14 Nov 2025, and the core obligations, including verifiable parental consent for under-18s, apply around May 2027. Singapore's PDPC has told organisations to treat children's data as generally sensitive since March 2024. CBSE requires an APAAR ID for board exams, or a recorded parental refusal.
6.  **RES-F06 · The current RBAC model has no answer yet for platform-level or brand-level access.** The six-layer model in product truth was written for one brand's campuses. It has no roles for GSG as platform operator or for brand leadership, and its scope hierarchy (Group › Country › Campus) has no brand level. The PowerSchool breach began with a single compromised credential on a vendor's support portal.
7.  **RES-F07 · Who controls the data has not been defined.** Under both PDPA and DPDP, the party that decides why and how data is processed is responsible for it. GSG runs the platform, while separate legal entities enrol students and employ staff. Product truth records employers but not data controllers, data-sharing between entities, or processing purposes.

## 01 · Scope and method

This is desk research. It combines what the project already knows with public market and regulatory sources. No users were interviewed, which is the biggest limit on these findings.

| Source                                                    | What it gave us                                                                                                               | Confidence                                              |
|-----------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------|
| GIIS Student Handbook 2025-26 (Singapore)                 | Roles, escalation paths, attendance, grading, refund and complaint rules, MyGIIS features, transfer policy                    | Primary, internal          |
| Exam and Holiday Schedules 2026-27; Noida Almanac 2025-26 | Calendar shapes, assessment rhythm, segment structure, working week                                                           | Primary, internal          |
| Product truth in `AI-Product-Design/product-truth`        | Architecture v1, Employee Data Dictionary v1, personas, flows, RBAC model, DL-01 to DL-21                                     | Our own assumptions      |
| globalschools.com teardown (25 Oct 2026)                  | Group structure, brand portfolio, domains, public promises                                                                    | Public claims            |
| Market and vendor sources                                 | ISC Research via ICEF Monitor; vendor pages for iSAMS, Veracross, Engage, Faria, Arbor; GEMS case study; India ERP comparison | Vendor marketing         |
| Regulatory sources                                        | DPDP Rules 2025 summaries; PDPC children's guidelines summary; CBSE APAAR reporting; EdWeek on PowerSchool                    | Secondary, needs counsel |
| Henry, 25 Oct 2026                                        | Scope is all GSG brands; GSG runs Campus OS                                                                                   | Stated direction           |

Vendor capabilities are as each vendor describes them, not tested. Legal points are summaries, not legal advice.

## 02 · Operating context

Global Schools Group runs Campus OS for every brand it owns, and it grows by acquiring schools.

| Figure | Meaning |
|---|---|
| **1 / 3 / 2** | brands / campuses / countries in Architecture v1 |
| **12** | GSG school brands in scope |
| **64 / 11** | GSG campuses / countries claimed |
| **35 / 10** | GIIS campuses / countries claimed, the largest brand |

GIIS and GSG figures are their own public claims and don't agree with each other. GSG's site says 11 countries while its country filter lists 10, and the GIIS page's own description says 7 countries and 23+ campuses.

### How the group is structured

- **Group and platform operator.** Global Schools Group, headquartered at the GIIS SMART Campus in Punggol, runs Campus OS. Staff use group mailboxes (@globalschools.com).
- **Brands.** GIIS, OWIS, Dwight School Seoul, Emirates American School, Glendale, Harrods International, Heath House, Chinese International School Manila, Domuschola, Regent, Vikaasa and Witty. Across them the group teaches IB, Cambridge, CBSE, ICSE, American, British, Montessori, Waldorf and other curricula.
- **Growth by acquisition.** GSG built GIIS campuses from 2002 to 2019. From 2021 it has mostly acquired or partnered with established schools.
- **Countries.** GIIS lists Singapore, India, Malaysia, Japan, Thailand, UAE, Vietnam, Saudi Arabia, South Korea and Cambodia. Other brands add the Philippines and the UK.
- **Operating entities.** GIIS's history says its Thailand and first India campuses opened "through local partners, via joint ventures or franchises". The Employee Data Dictionary has one synthetic employer per country (GIIS Singapore, GIIS India).

### Systems families and staff use today

At GIIS, MyGIIS is the parent portal, with 11 features: calendar, exams and results, fees history, directory, messaging, PTM booking, profile update, bus requests, syllabus, timetable and worksheets. Transfer applications can only be made through MyGIIS. Group admissions enquiries go through enquiry.globalschools.com and hiring through careers.globalschools.com, and the family's chosen school isn't carried from one site to the next. We haven't researched which portals and SIS tools the other 11 brands use.

## 03 · Users and stakeholders

Six groups use the system. Behind them are decision-makers and outside parties who impose requirements. One person often holds several of these roles, sometimes on different campuses.

### People who use it

| Segment           | Who                                                               | How often, where                       | What the group structure means for them                                                   | Evidence                                                                                           |
|-------------------|-------------------------------------------------------------------|----------------------------------------|-------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| Families          | Parents and guardians, students                                   | Daily or weekly, mostly mobile         | Children can be on different campuses; transfers move them between campuses and countries | Handbook: MyGIIS features; transfer only through MyGIIS                                            |
| Teaching staff    | Class and subject teachers, coordinators by programme band        | Every period, web and mobile           | Some teach across campuses and programmes                                                 | Handbook coordinator lists; persona Priya Nair (SG-PGL and SG-ECS)                                 |
| Campus operations | Admissions, finance, transport, nurse, counsellors, security, CCA | All day, desktop                       | Routes, fees and contacts are specific to each campus                                     | Handbook contact matrix routes each topic to a role on a campus                                    |
| Leadership        | Principals, VPs, Country Directors                                | Weekly, plus approvals between classes | Approvals with deadlines; answerable to regulators                                        | Refunds approved by Country Director and Finance Controller; complaints escalate at 14 and 21 days |
| Brand leadership  | Brand heads, brand academic leads                                 | Weekly, desktop                        | Each brand has its own curricula, identity and policies across countries                  | 12 brands; acquired schools keep their identity                                                    |
| Central functions | Group HR, Global Admin, IT, group academic leads, DPO             | Daily, desktop                         | Need answers to "who can see what, and why" across the group                              | Product truth 00 and 01 (Admin Platform personas)                                                  |
| Platform operator | GSG's platform team: admins, support, security                    | Continuous                             | Operate one platform that holds every brand's data                                        | Henry: GSG runs Campus OS                                                                          |

### People who decide or impose requirements

#### Sponsors

*Set direction and fund it*

GSG group leadership. GSG's public site emphasises the network: shared curricula, a global community and "seamless transfers".

#### Gatekeepers

*Can block adoption*

Brand heads, Principals and Country Directors, who own outcomes. IT, who run identity and devices. DPOs and legal teams, who own consent and residency. Programme coordinators, who own results release.

#### Regulators and boards

*Set non-negotiable rules*

Singapore: CPE/SSG (EduTrust), ICA (Student Pass), PDPC. India: CBSE (APAAR, list of candidates), the Data Protection Board. IB and Cambridge for registrations and predicted grades.

#### Partners and vendors

*Exchange data with the schools*

Franchise and joint-venture operators, payment gateways, insurers (FPS, medical), Google Workspace, transport and LMS vendors.

### Observations

- **One person, several roles.** Architecture v1 already notes that one Person can be a parent on one campus and staff on another.
- **The family that moves is a network-specific user.** The handbook's transfer rules require one month's notice and no pending dues. Both principals must approve, with acknowledgement in 2 working days and a decision in 3 weeks. No change of course is allowed in Grades 9–10 or 11–12. GSG's website promises "seamless transfers" but its FAQ answer on transfers is empty.
- **Teachers act in short gaps.** Attendance, leave approvals and parent messages happen between periods.
- **Principals carry regulatory risk.** They approve leave over 2 days, suspensions and transfers, and handle complaint escalation.

## 04 · What varies across the group

This catalogue lists what differs between brands, campuses and countries, the level each difference sits at, and whether product truth describes it today.

| ID    | What varies                                                                      | Level                                                       | Evidence                                                                                                 | In product truth today                                   |
|-------|----------------------------------------------------------------------------------|-------------------------------------------------------------|----------------------------------------------------------------------------------------------------------|----------------------------------------------------------|
| MC-01 | Settings that differ by campus but share defaults                                | Group › Country › Campus › Section › Programme offering     | Different calendars, fees and regulators in SG and Noida                                                 | Described                   |
| MC-02 | Brand identity, curricula, policies and portals                                  | Brand, across countries                                     | 12 brands; acquisitions since 2021                                                                       | Not described             |
| MC-03 | Legal entity behind a campus: school operator, employer, franchise or JV partner | Campus, can change over time                                | GIIS Thailand and India campuses through local partners; one employer per country in the data dictionary | Employer only             |
| MC-04 | Data residency and cross-border transfer rules                                   | Country                                                     | Singapore and India cells in Architecture v1; other countries not assessed                               | SG and IN only            |
| MC-05 | Regulator reports, identifiers and inspections                                   | Country, sometimes emirate or state                         | ICA, CPE/SSG, FPS in Singapore; CBSE and APAAR in India                                                  | SG and IN only            |
| MC-06 | Academic year start, terms and working week                                      | Country and campus                                          | Noida starts in April with Saturdays non-working; Singapore runs semesters                               | Partly                    |
| MC-07 | Several curricula on one campus, each with its own grades and rules              | Programme offering                                          | Six curricula at GIIS Singapore                                                                          | Described (programme packs) |
| MC-08 | Board and national identifiers and registrations                                 | Curriculum and country                                      | APAAR for CBSE board exams; IB and Cambridge candidate registration                                      | Partly                    |
| MC-09 | Consent and notice rules for children's data                                     | Country, then per child and purpose                         | DPDP verifiable parental consent; PDPC children's guidelines; APAAR "refused" records                    | Not described             |
| MC-10 | Transfers between campuses, brands and countries                                 | Two campuses, possibly two legal entities and two countries | Handbook transfer policy; GSG "seamless transfers"                                                       | Record move only          |
| MC-11 | Comparing campuses whose academic years start at different times                 | Group                                                       | April and mid-year starts                                                                                | Partly                    |
| MC-12 | Currency, tax, date formats, staff and parent languages                          | Country and campus; parent preference                       | SGD and INR, GST; campuses in Arabic-, Japanese-, Korean- and Khmer-speaking countries                   | Partly                    |
| MC-13 | Group staff in one country working with another country's data                   | User and country                                            | GRP-HQ in Singapore; India data held in India                                                            | Partly                    |
| MC-14 | Which settings the group fixes and which a school may change                     | Each setting                                                | iSAMS Central Manage and Arbor pushdowns both make this distinction                                      | Not described             |
| MC-15 | One identity and directory across the group                                      | Group                                                       | @globalschools.com staff mail; Google Workspace                                                          | Described                   |

Described in product truthPartly named but not modelledNot described absent from product truth

## 05 · Core domains

These are the 12 modules and 8 core services as Architecture v1 defines them, grouped by what they do, with the evidence behind each and what the market already covers.

| Domain                  | Modules and services in product truth                                                                                                   | Evidence in GIIS files                                                        | Market coverage found                                                                                            |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------|
| Platform core           | Identity and roles, Tenancy and config, Calendar engine, Rules engine, Programme packs, Cases and SLAs, Notifications, Events and audit | Handbook thresholds, SLAs, refund tables; two calendar shapes                 | Group configuration (iSAMS, Arbor). Versioned policy rules weren't found as a product feature                    |
| Learner lifecycle       | Admissions and Enrolment, Student Records, Careers and Universities                                                                     | Transfer policy, Student Pass cases, AEPP placement, LOR and predicted grades | Admissions in most SIS suites and OpenApply. Transfers between schools in a group weren't found as a feature     |
| Teaching and assessment | Teaching and Learning, Assessment and Reporting                                                                                         | CBSE eighths, IB 1–7, IGCSE and CLSP scales; re-evaluation in 2 days          | IB and Cambridge in international suites and Toddle or ManageBac; CBSE formats in Indian ERPs; iSAMS claims CBSE |
| Daily operations        | Attendance and Conduct, Campus Services, Activities and Awards                                                                          | 90% and 85% attendance rules, ICA notice after 7 days, gate log, bus requests | Common across SIS and ERP products                                                                               |
| Care                    | Wellbeing and Inclusion                                                                                                                 | Counselling, SEN, child protection restricted to named roles                  | Health and wellness modules in Veracross; wellbeing structures in iSAMS                                          |
| Money                   | Fees and Finance                                                                                                                        | Term billing in 4-month blocks, refund tables 3A and 3B, FPS                  | Billing in most suites; fees are a strength of Indian ERPs                                                       |
| Engagement              | Communication and Cases                                                                                                                 | Complaint SLAs of 2, 7, 14 and 21 working days                                | Messaging is common (SchoolsBuddy, parent apps). SLA-tracked cases weren't found                                 |
| People                  | Staff and HR, plus Global Admin, Team Management and Governance from the data dictionary                                                | Staff across campuses and programmes                                          | Central HR records in Arbor for UK trusts                                                                        |

## 06 · Market and competitor landscape

The international school sector is large and growing, mostly in Asia. The software that serves it falls into five types.

| Figure | Meaning |
|---|---|
| **15,075** | international K-12 schools, up 2% a year |
| **7.7M** | students enrolled |
| **US$69.3B** | annual fee income |
| **58%** | of schools are in Asia |

ISC Research, reported by ICEF Monitor on 25 Feb 2026. Growth now comes more from local families than from expatriates, and 78% of schools follow a hybrid local-and-global model. India grew 2% and the UAE 7%.

### Five types of competitor

#### International SIS suites

*Veracross · iSAMS by IRIS · Engage by Tes · PowerSchool · Blackbaud*

**Cover** student records, admissions, billing and parent portals for IB, British and US schools. Group features are recent: iSAMS Central Manage launched in March 2026.

**Not found:** Indian board formats (beyond iSAMS's general CBSE claim), Singapore regulator workflows, or per-country residency.

#### Teaching and curriculum layer

*Faria (ManageBac+, OpenApply, SchoolsBuddy) · Toddle*

**Cover** IB planning, portfolios, admissions and school-to-home messaging. Toddle reported 1,500+ schools and over 20% of IB schools in Jan 2023. Faria supports OneRoster 1.2, Clever and public APIs.

**Not found:** group-level features. These products integrate with an SIS.

#### Indian school ERPs

*Entab CampusCare · Fedena · Teachmint*

**Cover** CBSE and ICSE formats, fees and transport, at roughly ₹1–10 lakh a year for 1,000 students, depending on vendor.

**Not found:** IB and Cambridge, international regulators, or group control. Entab implementations reportedly take 3–6 months.

#### Built by the group itself

*GEMS Education (250+ schools)*

**Covers** attendance, leave, timetables, fees, transport tracking and an AI chat assistant (GEMS Genie), built on OutSystems low-code with a parent app.

**Not found:** cost, team size, or how it handles residency.

#### Group MIS (UK analogy)

*Arbor for multi-academy trusts*

**Covers** trust-wide dashboards, central HR, and pushdowns of assessment frameworks, pay scales and policies. Schools "work autonomously, but with accountability". Over 925 trusts migrated.

**Not found:** multi-country or multi-regulator use.

### Vendor evidence

| Vendor                   | What the sources show                                                                                                                                                                                                                                  | What the sources don't show                                                     |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| iSAMS by IRIS            | Central Manage (9 Mar 2026) pushes audited settings, such as global lists, custom fields and wellbeing structures, to every school in a group. Central Analytics is separate. Claims to run CBSE alongside IB, British and US curricula in one system. | CBSE grading or board formats; residency options; Singapore regulator workflows |
| Veracross                | A full SIS suite used in 60+ countries (a third-party profile says 3,200+ schools). Open API with 200+ partners, including Toddle.                                                                                                                     | Multi-campus or group features on its international page; Indian boards         |
| Engage by Tes            | "Single source of truth" for XCL Education's 18 schools and 15,000 students across Thailand, Vietnam, Malaysia and Singapore. It replaced paper records, Phoenix, Oracle, Excel and a custom system.                                                   | Measured results; rollout beyond Malaysia and a Vietnam pilot                   |
| PowerSchool              | More than 16,000 customers. Breach in December 2024 through a compromised credential on its PowerSource support portal.                                                                                                                                | Use in the markets where GSG operates                                           |
| Faria and Toddle         | IB-first teaching layer. OneRoster 1.2, Clever, public APIs; OpenApply file sync to iSAMS and PowerSchool planned for early 2026.                                                                                                                      | Use as the main system of record                                                |
| Entab, Fedena, Teachmint | CBSE formats built in (Entab); APIs on higher tiers (Fedena); mobile-first interface (Teachmint).                                                                                                                                                      | IB, Singapore or group control. The comparison comes from a competitor's blog   |
| GEMS (in-house)          | One low-code system across 250+ schools in over a dozen countries.                                                                                                                                                                                     | Cost, residency approach                                                        |

### What the market shows

1.  **Vendors are now competing on group-level features.** iSAMS, Arbor and Engage all sell central control and group reporting.
2.  **Products connect through shared standards.** OneRoster 1.2 for rosters, LTI for learning tools, Clever and OIDC for sign-in.
3.  **Vendors sell "one record" as their core value.** Veracross and Engage both position themselves as the system of record.
4.  **Indian vendors treat CBSE formats as basic.** All three Indian ERPs reviewed include board formats.
5.  **Vendor support access is a real attack path.** The PowerSchool breach came through a support tool, not the product itself.
6.  **The largest groups also build.** GEMS built its own system across 250+ schools.

## 07 · Regulation

Both countries in product truth have tightened rules on children's data, on overlapping timelines.

| When | What | Detail |
|---|---|---|
| 28 Mar 2024 | PDPC children's guidelines | Treat children's data as sensitive. Run impact assessments before launch. Teenagers can consent if they understand the notice; otherwise ask parents. |
| Dec 2024 | PowerSchool breach | A compromised support credential exposed data about families and educators. |
| 14 Nov 2025 | DPDP Rules notified | The Data Protection Board is set up. Everything else starts in phases. |
| 2026 board exams | APAAR for CBSE | Mandatory, but schools may enter "refused" (parent withheld consent, with a copy kept) or "NOGEN". |
| ≈ May 2027 | DPDP core obligations | Notices, consent, children's data, security and retention. Architecture v1 plans to launch the India cell from June 2027. |

| Topic                   | Singapore                                                                                                                                                 | India                                                                                                                            |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------|
| Consent for children    | PDPC guidelines: a child is anyone up to 18. Teenagers may consent if they understand the notice, and schools may require parental consent at higher ages | DPDP: verifiable parental consent for under-18s, using reliable ID or DigiLocker. Some education and safety processing is exempt |
| Breach notification     | Notify affected children, and parents where possible                                                                                                      | Notify affected people at once. Initial notice to the Board without delay, full report within 72 hours                           |
| Notices and language    | Plain, age-appropriate language                                                                                                                           | Notice available in English or a scheduled Indian language on request (DPDP Act s.5(3))                                          |
| Evidence for regulators | EduTrust, ICA Student Pass cases, FPS                                                                                                                     | CBSE list of candidates, APAAR status, processing logs kept at least one year                                                    |
| Parties defined by law  | Organisation (decides) and data intermediary (processes on its behalf)                                                                                    | Data fiduciary (decides) and data processor (processes on its behalf)                                                            |
| Penalties               | Financial penalties under PDPA                                                                                                                            | Up to INR 2.5 billion per breach of obligations                                                                                  |

These are summaries from secondary legal sources. The scope of the education exemptions, and the rules in GSG's other 9 countries, have not been researched.

## 08 · Access control findings

This section covers what product truth defines for access today, and what the research found that the model doesn't yet cover.

### The model in product truth today

- **Architecture v1:** access is a role plus a scope. The architecture lists 17 school roles, granted at a level of Group › Country › Campus › Section, with permissions flowing down.
- **Employee Data Dictionary v1:** two layers. Global Admin (GA) grants set a ceiling of modules × campuses. Inside it, a Team Management (TM) membership carries a role, a permission template and a data scope. All of these are confirmed inputs.
- **07-rbac-model.md:** six layers. Hierarchical grants, operational membership, static and derived scope, relationship-based access for parents, record-level ACLs, and evaluation inside each country cell. All of these are recorded as Assumed (DL-16 to DL-21).

### What the research found that the model doesn't cover

| Finding                                                                 | Evidence                                                                   | Where product truth stands                                                            |
|-------------------------------------------------------------------------|----------------------------------------------------------------------------|---------------------------------------------------------------------------------------|
| GSG's platform team operates a system that holds every brand's data     | Henry: GSG runs Campus OS. PowerSchool breach through a support credential | No platform-operator or support roles in the role catalogue (A06)                     |
| Brand leadership works across countries but only within one brand       | 12 brands; brand heads and academic leads                                  | The scope hierarchy has no brand level (DL-17 covers Group, Country and Campus)       |
| The same country can host campuses of several brands and entities       | GSG's country and brand filters; partner-run GIIS campuses                 | A country-level grant can't tell brands or entities apart                             |
| Campuses change hands through acquisition, and partners can leave       | Growth by acquisition since 2021; JV and franchise campuses                | Grants and memberships have no link to the legal entity behind a campus               |
| Access to children's data needs a lawful purpose, and sometimes consent | DPDP verifiable parental consent; PDPC purpose limitation; APAAR refusals  | Capabilities and audit events carry no purpose; there is no consent record            |
| Group staff may need data held in another country                       | GRP-HQ in Singapore; India data held in India                              | Layer 6 says data stays in its cell but doesn't say how cross-cell access is approved |

## 09 · Data controller findings

This section covers what the law defines, which parties exist in GSG's structure, and what product truth records about data ownership today.

### What the law defines

Both laws separate the party that decides why and how personal data is used from the party that processes it on that party's behalf. Under Singapore's PDPA these are the *organisation* and the *data intermediary*. Under India's DPDP Act they are the *data fiduciary* and the *data processor*. The deciding party carries the obligations to give notice, get consent, keep records, answer rights requests and report breaches.

### Parties that exist in GSG's structure

| Party                                  | Examples from the research                                                             | What is known                                                                             | What is not known                                                            |
|----------------------------------------|----------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------|------------------------------------------------------------------------------|
| School operating entities              | The legal entities behind GIIS Singapore, GIIS India and each acquired brand's schools | They enrol students and employ staff. The data dictionary models one employer per country | How many entities there are, and which entity owns each campus               |
| Franchise and JV partners              | GIIS Thailand and early India campuses                                                 | GIIS opened some campuses through local partners                                          | Which campuses are still partner-run, and the data terms in their agreements |
| GSG as platform operator               | The GSG entity that runs Campus OS                                                     | GSG runs the platform for all brands                                                      | Which GSG entity, and on what contractual basis it holds school data         |
| GSG group functions                    | Group HR, group academic and quality oversight, leadership dashboards                  | Architecture v1 plans group analytics on de-identified data only                          | Which group purposes need identifiable data                                  |
| Sub-processors                         | Cloud host, Google Workspace, payment gateways, messaging providers                    | Named in Architecture v1 integrations                                                     | The list for each country                                                    |
| Data subjects                          | Students, parents, staff                                                               | Parents act for children under 18 in India; some teenagers can consent in Singapore       | How custody and guardianship are recorded today                              |
| Recipients required by law or contract | ICA, CPE/SSG, CBSE, IB, Cambridge, insurers                                            | Disclosures are named in the handbook and in Architecture v1 integrations                 | Recipients in GSG's other 9 countries                                        |

### Where product truth stands

- Every row carries `tenant_path` (group, country, campus). Nothing records which legal entity controls the data.
- The Employee Data Dictionary records the employer of each employment. Architecture v1 doesn't record the controller of student and family data.
- Product truth doesn't model data sharing between entities, such as when a student transfers between brands or a group function views campus data.
- Architecture v1 keeps names, passports and health data inside each country cell and sends de-identified events to group analytics.

Which party is controller, processor or joint controller in each case is a legal question. This research only identifies the parties and the gaps.

## 10 · Product truth vs evidence

The five architecture bets and the decisions most affected by the findings, each compared with the evidence. Verdicts describe what the evidence shows. All DL entries stay Assumed until a stakeholder confirms them.

| Item in product truth                               | Evidence                                           | What the evidence shows                                                                                                                                                                                  |
|-----------------------------------------------------|----------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Bet 1 · Campus as configuration                     | Consistent            | iSAMS, Arbor and Engage all centralise configuration across a group. Product truth doesn't say which settings the group fixes and which a school may change (MC-14), or describe brand settings (MC-02). |
| Bet 2 · Programme packs                             | Consistent            | Multi-curriculum support is common among vendors, and CBSE formats are basic in India. Board identifiers such as APAAR are only partly covered (MC-08).                                                  |
| Bet 3 · Policy as rules                             | Consistent            | We found no vendor that offers versioned policy rules as a feature. The handbook contains many rules with dates and thresholds.                                                                          |
| Bet 4 · Calendar engine                             | Consistent            | The calendars confirm the differences. Working week and cross-campus comparison are only partly covered (MC-06, MC-11).                                                                                  |
| Bet 5 · Evolve MyGIIS                               | Partial             | This covers GIIS. The portals of the other 11 brands haven't been researched.                                                                                                                            |
| One cell per country                                | Untested beyond 2   | Product truth defines Singapore and India cells. GSG operates in 11 countries, and their residency rules haven't been researched.                                                                        |
| DL-01 · Admin Platform first                        | Consistent            | Access gates every module, and the PowerSchool case shows access as a breach path.                                                                                                                       |
| DL-03 · Sample data: SG-PGL, SG-ECS, IN-NOI, GRP-HQ | Doesn't match scope | All sample campuses are GIIS. The scope is now all 12 brands.                                                                                                                                            |
| DL-07 · Deny wins, fail closed                      | Consistent            | Matches PDPC's guidance on children's data and the scale of DPDP penalties.                                                                                                                              |
| DL-09 · Separation of duties                        | Partial             | Covers HR, Global Admin and Module Admin. Platform operator and vendor support aren't mentioned.                                                                                                         |
| DL-11 · Tiers 1/2/3                                 | Partial             | PDPC treats children's data as generally sensitive. Product truth puts students' academic data in Tier 2.                                                                                                |
| DL-15 · English UI                                  | Partial             | Matches staff communication in the files. DPDP gives parents a right to notices in scheduled Indian languages, and GSG has campuses in non-English-speaking countries.                                   |
| DL-17 · Hierarchical grants                         | Partial             | Covers Group, Country and Campus. There is no brand level (section 08).                                                                                                                                  |
| DL-18 · Family links for parents                    | Consistent            | Matches how both laws treat parents as acting for children. Product truth has no record of consent.                                                                                                      |
| DL-19 · Record-level ACL                            | Consistent            | The handbook restricts child protection and counselling records to named roles.                                                                                                                          |
| DL-21 · Time-boxed delegation                       | Consistent            | Matches cover and acting patterns. Vendor support access isn't mentioned.                                                                                                                                |

The research doesn't bear on DL-02, 04, 05, 06, 08, 10, 12, 13, 14, 16 or 20.

## 11 · Open questions and research gaps

What this desk research couldn't answer.

### Questions for Henry and stakeholders

- Which GSG legal entity runs the platform, and what agreements does it have with each school entity?
- Which campuses are run by franchise or JV partners today, and do they come onto the platform?
- Which portals and SIS tools does each of the other 11 brands use, and who owns them?
- What does MyGIIS run on, and can it expose an API?
- Which of GSG's other 9 countries have data residency or cross-border transfer rules?
- Who owns programme pack masters and policy templates across brands?
- Who owns the group enquiry system, and does its data flow to the schools?

### Research not yet done

| Gap                 | Who could fill it                                                                       | What it would establish                                                         |
|---------------------|-----------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Contextual inquiry  | Teachers, coordinators, admissions staff at a GIIS campus and at another brand's campus | Real workarounds; how much work happens on mobile                               |
| Family research     | Families, including ones moving between campuses or brands                              | Where today's portals fail; language needs; how consent is experienced          |
| Vendor verification | Demos from iSAMS and Veracross                                                          | Whether vendor claims hold for GSG's curricula and regulators                   |
| Legal review        | Group and country DPOs, counsel                                                         | Controller roles, how the education exemptions apply, residency in each country |
| Systems inventory   | IT at each brand and campus                                                             | Current tools, owners, contracts and data held                                  |

## 12 · Sources

Web sources read on 25 Oct 2026.

- Project files: GIIS Student Handbook 2025-26; Exam Schedule 2026-27; Holiday Schedule 2026-27; GIIS Noida Almanac 2025-26.
- Product truth: [AI-Product-Design/product-truth](https://github.com/huyapps1003/AI-Product-Design/tree/main/product-truth) (Architecture v1, Data Dictionary v1, 00–07, gap analysis).
- Earlier research: Global Schools Group teardown, 25 Oct 2026 (project artifact).
- Market: [ICEF Monitor on ISC Research, Feb 2026](https://monitor.icef.com/2026/02/continuing-expansion-of-k-12-international-school-sector-driven-more-by-growing-local-demand/).
- Vendors: [iSAMS Central Manage](https://www.isams.com/company/press/isams-launches-central-manage-to-deliver-unified-control-for-multi-school-groups-and-multi-campus-schools/); [iSAMS multi-curriculum](https://blog.isams.com/managing-multi-curriculum-an-sis-for-ib-british-us-and-cbse-schools-in-the-mena-region); [Veracross international](https://www.veracross.com/schools/international/); [Veracross profile](https://yespress.io/veracross); [Engage at XCL Education](https://www.tes.com/for-schools/engage/case-studies/xcl-education-south-east-asia); [Faria, Connected by Design](http://www.faria.org/insights/connected-by-design/); [Toddle Series A (Inc42)](https://inc42.com/?p=377095); [Arbor MAT MIS](https://arbor-education.com/products/group-mat-mis); [GEMS on OutSystems (Xebia)](https://xebia.com/de/customer-stories/gems-digital-transformationsemsem/); [BSME on ISP standardisation](https://www.bsme.org.uk/scaling-excellence-how-multi-school-groups-are-enhancing-student-outcomes-through-standardised-systems); [India ERP comparison (Fuzen, a competitor)](https://www.fuzen.io/posts/school-management-software-comparison-fuzen-vs-entab-fedena-teachmint).
- Security: [EdWeek on the PowerSchool breach](https://www.edweek.org/technology/what-schools-should-know-about-the-powerschool-data-breach/2025/01).
- Regulation: [India Briefing on DPDP Rules 2025](https://www.india-briefing.com/news/dpdp-rules-2025-india-data-protection-law-compliance-40769.html); [Bird & Bird on PDPC children's guidelines](https://cm.twobirds.com/en/insights/2024/singapore/singapore-pdpc-issues-advisory-guidelines-on-the-pdpa); [Careers360 on APAAR and CBSE LoC](https://news.careers360.com/cbse-board-exams-2026-schools-submit-loc-without-apaar-id-rule-relaxed-over-lack-of-parental-consent-wrong-data-technical-issues/amp); [ICA Student Pass for PEIs](https://www.ica.gov.sg/pass/studentpass/peo).
- GIIS footprint: [GIIS history page](https://globalindianschool.org/my/about/giis-history/).

Vendor figures are each vendor's own claims. Legal summaries are not legal advice.

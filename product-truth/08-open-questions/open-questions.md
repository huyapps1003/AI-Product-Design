# Open questions

**Status:** Living register · started 25 Oct 2026 · **Owner:** Henry
**Cites:** [foundation research](../../research/2026-10-25-campus-os-foundation-research.md) (RES-F01…F07, MC-01…MC-15) · [Architecture v1 § Open questions](../00-sources/Global_Campus_OS_Product_Architecture_v1.md#open-questions) · [Data Dictionary § Unresolved decisions](../00-sources/Global_Campus_OS_Employee_Data_Dictionary_v1.md)

**Tóm tắt (VI):** Danh sách mọi câu hỏi chưa có câu trả lời. Product truth không tự đặt ra giải pháp cho những điểm này; thiết kế không được coi chúng là đã quyết.

Every gap that product truth knows about but cannot answer lives here, with one ID. Files elsewhere in `product-truth/` cite the ID instead of guessing an answer.

**Rules**

1. A question records what is unknown, why it matters for design, and the evidence behind it. It never carries a proposed answer.
2. When a question is answered, add a decision to [`../07-decisions/decision-log.md`](../07-decisions/decision-log.md), set the row's status to **Answered → DL-xx**, and update the files listed under *Affects*. Don't delete the row.
3. **For AI design work:** if a screen depends on an Open question, don't invent the answer. Keep the element generic, label it as pending, and note the OQ ID in the screen notes.

Status values: **Open** · **Answered → DL-xx** · **Withdrawn** (with reason).

The Data Dictionary's own unresolved decisions (Decision D01–D15) are tracked in that file and in [`rules-to-ui.md`](../06-experience/admin-platform/rules-to-ui.md#unresolved-decisions-data-dictionary-d01d15--design-defaults); they are not repeated here.

---

## A · Organisation and legal entities

| ID | Question | Why it matters for design | Evidence | Affects | Who can answer | Status |
|---|---|---|---|---|---|---|
| OQ-01 | Which GSG legal entity runs the platform, and on what contractual basis does it hold each school's data? | Decides who appears as operator in notices, audit exports and support access | RES-F07; research §09, §11 | data-controller-model, regulatory-context | Henry, GSG legal | Open |
| OQ-02 | How many school operating entities are there, and which entity operates each campus? | No field records the entity behind a campus; every screen showing "who owns this data" depends on it | MC-03; research §09 | tenancy-and-scope, data-controller-model | GSG legal, finance | Open |
| OQ-03 | Which campuses are run by franchise or joint-venture partners today, do they come onto the platform, and what data terms are in their agreements? | Partner-run campuses may be a different party from the brand and from GSG | RES-F02; research §02, §11 | operating-context, data-controller-model | Henry, GSG legal | Open |
| OQ-04 | What happens to a campus's grants, memberships and data when it changes hands (acquisition, partner exit)? | Grants and memberships have no link to the legal entity behind a campus | Research §08 (row 4); RES-F02 | rbac-model, data-controller-model | GSG legal, platform owner | Open |
| OQ-05 | Which campuses, brands and countries are in the first release beyond Singapore and Noida? | Sets which brands, regulators and calendars the first designs must cover | Architecture Q2; RES-F01 | product-brief, admin-platform | Henry, GSG leadership | Open |

## B · Data controller and privacy

| ID | Question | Why it matters for design | Evidence | Affects | Who can answer | Status |
|---|---|---|---|---|---|---|
| OQ-06 | For student and family data, employee data and group analytics, which party is controller (organisation / data fiduciary), processor (data intermediary / data processor) or joint controller? | Notices, consent screens, rights requests and breach flows name the deciding party | RES-F07; research §07, §09 | data-controller-model | Group and country DPOs, counsel | Open |
| OQ-07 | How is data shared between entities: a student transferring between brands or countries, or a group function viewing campus data? | Transfer and group-view screens need to show what moves and under whose authority | MC-10, MC-13; research §09 | data-controller-model, tenancy-and-scope | DPOs, counsel | Open |
| OQ-08 | Which group purposes need identifiable data, rather than the de-identified data Architecture v1 plans for group analytics? | Decides what group dashboards may show | Research §09; Architecture § Multi-campus and tenancy model | data-controller-model, data-classification | GSG leadership, DPOs | Open |
| OQ-09 | How is a lawful purpose recorded when someone accesses children's data? | Capabilities and audit events carry no purpose today | MC-09; research §08 (row 5) | rbac-model, data-controller-model | DPOs, counsel | Open |
| OQ-10 | What does the consent record hold (purpose, who consented, refusals such as APAAR "refused"), and how does it relate to access? | Architecture v1 names a *Consent* entity and parent "consents", but defines no fields, purposes or link to access evaluation | MC-09; RES-F05; research §07 | data-controller-model, regulatory-context | DPOs, admissions owner | Open |
| OQ-11 | How are custody and guardianship recorded today? | Family links (DL-18) depend on who may act for a child | Research §09 | rbac-model (Layer 4) | Admissions owners per brand | Open |
| OQ-12 | Which sub-processors handle data in each country? | Needed for notices and the data-controller model | Research §09; Architecture § Integrations | data-controller-model | IT, procurement | Open |
| OQ-13 | Should children's academic data stay in Tier 2? PDPC treats children's data as generally sensitive. | Changes masking and lock treatment on student screens | DL-11; research §10 | data-classification | DPOs | Open |

## C · Access (RBAC)

| ID | Question | Why it matters for design | Evidence | Affects | Who can answer | Status |
|---|---|---|---|---|---|---|
| OQ-14 | Where does *brand* sit in the tenancy and scope hierarchy? | The hierarchy (Group › Country › Campus) has no brand level, so brand-wide grants and brand settings can't be expressed | RES-F06, MC-02; DL-17 | tenancy-and-scope, rbac-model, role-catalogue | Henry, data architect | **Answered → DL-40** (Brand and Brand-country levels) |
| OQ-15 | What access does GSG's platform team (admins, support, security) hold, and how is support access to school data controlled? | No platform-operator or support roles exist; the PowerSchool breach began with a support credential | RES-F06; research §06, §08; DL-09 | role-catalogue, rbac-model | Henry, GSG IT security | Open |
| OQ-16 | What do brand heads and brand academic leads need to see and do across countries? | Brand leadership has no role or persona | Research §03, §08 | role-catalogue, stakeholders | Brand heads | Open |
| OQ-17 | When one country hosts campuses of several brands or entities, how is a country-level grant limited? | A country grant can't tell brands or entities apart | Research §08 (row 3) | rbac-model, tenancy-and-scope | Data architect, DPOs | **Answered → DL-40** (a Brand-country grant stays inside one brand) |
| OQ-18 | How is cross-cell access approved when group staff need data held in another country? | Layer 6 says data stays in its cell but not how cross-cell access is approved | MC-13; research §08 (row 6) | rbac-model | Data architect, DPOs | Open |
| OQ-19 | What access do students have beyond "self", and do franchise or JV partner staff use the platform? | No access path is defined for students or partner staff | Architecture § Users and roles; RES-F02 | role-catalogue | Henry | Partly answered → DL-68 (students); partner staff Open |

## D · Configuration and variation

| ID | Question | Why it matters for design | Evidence | Affects | Who can answer | Status |
|---|---|---|---|---|---|---|
| OQ-20 | Who owns programme pack masters and policy templates across brands, and which settings may a school change? | Settings screens need to show what is fixed by the group and what a school may override | MC-14; Architecture Q7; research §11 | tenancy-and-scope, variation-catalogue | GSG academic leads, brand heads | Open |
| OQ-21 | Which of GSG's other 9 countries have data residency or cross-border transfer rules? | Decides whether more country cells exist | MC-04; Architecture Q6 | regulatory-context, tenancy-and-scope | Counsel per country | Open |
| OQ-22 | Which regulator reports, identifiers and inspections apply outside Singapore and India? | Regulator packs exist only for SG and IN | MC-05 | regulatory-context | Country directors, counsel | Open |
| OQ-23 | Which board and national identifiers must student records carry per curriculum and country, including recorded refusals? | APAAR and board registrations are only partly covered | MC-08; RES-F05 | variation-catalogue, regulatory-context | Programme coordinators | Open |
| OQ-24 | How do different academic-year starts and working weeks appear in group views and comparisons? | Group dashboards compare campuses whose years start in April and mid-year | MC-06, MC-11 | variation-catalogue | Group academic leads | Open |
| OQ-25 | Which languages must parent notices and UIs support, beyond English? | DPDP gives a right to notices in scheduled Indian languages; GSG has campuses in Arabic-, Japanese-, Korean- and Khmer-speaking countries | MC-12; DL-15; research §07 | regulatory-context, admin-platform | DPOs, brand heads | Open |
| OQ-26 | How does brand identity appear in shared surfaces: the staff shell, one group parent app or brand-branded apps? | The shell mock shows a GIIS logo; staff and families can belong to several brands | MC-02; Architecture Q5 | ia-screen-map, product-brief | Henry, brand heads | Open |

## E · Systems and migration

| ID | Question | Why it matters for design | Evidence | Affects | Who can answer | Status |
|---|---|---|---|---|---|---|
| OQ-27 | Which portals and SIS tools does each of the other 11 brands use, and who owns them? | Bet 5 (evolve MyGIIS) covers GIIS only | Research §02, §10, §11 | operating-context | IT at each brand | Open |
| OQ-28 | What does MyGIIS run on, and can it expose an API? | Migration path for the GIIS parent experience | Architecture Q3; research §11 | operating-context | GIIS IT | Open |
| OQ-29 | Who owns the group enquiry system, and does its data flow to the schools? | The family's chosen school isn't carried from enquiry to the school today | Research §02, §11 | operating-context | GSG admissions | Open |
| OQ-30 | Build, buy or hybrid? | Changes which screens Campus OS owns | Architecture Q1; research §06 | product-brief | Henry, GSG leadership | Open |
| OQ-31 | Which LMS is in use, and should the OS embed it or replace it? | Teaching screens depend on it | Architecture Q4 | modules-and-services | Academic leads | Open |
| OQ-32 | Which campus pilots first? | Sets the first real data and calendar for designs | Architecture Q8 | product-brief | Henry | Open |

## F · Design inputs

| ID | Question | Why it matters for design | Evidence | Affects | Who can answer | Status |
|---|---|---|---|---|---|---|
| OQ-33 | Which non-GIIS brand and campuses should the canonical sample data include? | All sample campuses are GIIS, so designs can't show multi-brand cases (DL-03) | Research §10 (DL-03 verdict); RES-F01 | admin-platform README (sample data) | Henry | Open |
| OQ-34 | Which personas does the platform need beyond the Admin Platform five (brand leadership, platform operator, DPO, families who move)? | Personas exist only for the admin slice | Research §03 | stakeholders, roles-personas | Henry, after user research | Open |

## G · Student directory

Added 30 Oct 2026 with the [student dictionary](../03-domain/student-directory-data-dictionary.md).

| ID | Question | Why it matters for design | Evidence | Affects | Who can answer | Status |
|---|---|---|---|---|---|---|
| OQ-35 | Is there one shared Person record across students, guardians and staff, or separate identities per role? | Decides whether a parent who is also staff has one profile or two | Architecture § Data model "Person"; Data Dictionary uses `employee_id` | student dictionary, rbac-model | Data architect | **Answered → DL-42** (one Person, role profiles, one account) |
| OQ-36 | What is the format and uniqueness scope of the Student ID, and does it change on transfer? | Directory search, ID cards and exports show it | No source defines it | student dictionary | Henry, Admissions owners | **Answered → DL-31, DL-67** (unique, kept on transfer; format DL-67) |
| OQ-37 | Which identity attributes must a student record hold in each country (date of birth, sex or gender values, name structure, nationality, home language)? | Form fields and validation differ by country | MC-05 | student dictionary | Admissions owners, DPOs | Open |
| OQ-38 | Can parents edit the student profile, documents and contacts themselves, as in MyGIIS Profile Update? | DL-18 gave parents a read-only set | HB p.35; DL-18 | student dictionary, rbac-model | Henry | **Answered → DL-28** |
| OQ-39 | Do any student or guardian fields sit in the global identity and directory layer, and which cell holds a guardian whose children are in two countries? | Cross-campus search and transfers | Architecture § System architecture | student dictionary, data-controller-model | Data architect, DPOs | **Answered → DL-55** (one Person; guardian profile per child's data location) |
| OQ-40 | Which product term means a segment and which a class (Architecture uses "Section" for both)? | Labels on every enrolment screen | Architecture § Tenancy vs. § Data model | glossary, tenancy-and-scope | Henry | **Answered → DL-29** |
| OQ-41 | Can teachers apply the Student Pass attendance threshold without seeing Tier 3 immigration data? | Attendance warnings | Architecture § Absence and leave, § Security | student dictionary | Henry | **Answered → DL-30** |
| OQ-42 | Which health alerts (allergies, medical conditions), if any, appear on the student card for teachers, transport and security? | Tier 3 health data vs. safety on trips and buses | HB medical conditions; Architecture § Security | student dictionary, data-classification | Nurses, DPOs | **Answered → DL-66** |
| OQ-43 | How long are student and guardian records kept after withdrawal or graduation, per country, and what is deleted or anonymised? | Archive and delete states | regulatory-context; Data Dictionary Decision D12 | student dictionary, regulatory-context | Counsel per country | Open |
| OQ-44 | During migration, which system is authoritative for student data: MyGIIS, other brands' SIS, or Campus OS? | Which fields are read-only with a source chip | Architecture § Integrations; OQ-27, OQ-28 | student dictionary | GIIS IT, brand IT | **Answered → DL-45** (authoritative per field until cutover; which systems per brand: OQ-27, OQ-28) |
| OQ-45 | Is AEPP a programme offering with its own enrolment, or a placement inside a regular class? | Whether an AEPP student has an AEPP enrolment or a flag on a regular one | Architecture § Admission to enrolment; HB § AEPP | student dictionary | Admissions owners, AEPP coordinator | **Answered → DL-65** |


## H · Access registry

Added 7 Nov 2026 with DL-32…DL-38 (RBAC review).

| ID | Question | Why it matters for design | Evidence | Affects | Who can answer | Status |
|---|---|---|---|---|---|---|
| OQ-46 | Which team owns the Identity and Roles service and the Access Registry? | Every access screen names who to ask and who approves catalogue changes | DL-32; Employee DD owners "TBD" for A06, A08, A09, R02, A14 | permission-catalogue, rbac-model | Henry, GSG IT | **Answered → DL-51** (GSG platform team) |
| OQ-47 | Who approves a Tier 3 type grant (A17), and for how long? | The request and approval screens for restricted record classes | DL-36; DL-30 (Admissions manages Student Pass cases) | rbac-model, data-classification | DPOs, Henry | Open |
| OQ-48 | Who approves a platform-operator or support session into a school's data, and can the school refuse it? | Support console, approval case and the banner a school sees | OQ-15; research §08 (PowerSchool support credential) | role-catalogue, rbac-model | GSG IT security, brand heads | **Answered → DL-52** (Campus Leader approves) |
| OQ-49 | Do capability codes follow one naming scheme across modules, and who reviews a module's new capabilities? | Capability names appear in template and access-review screens | DL-33 | permission-catalogue | Data architect | **Answered → DL-53** (`module.object.action`) |

## I · GSG footprint

Added 9 Nov 2026 from the public campus list ([gsg-landscape](../01-context/gsg-landscape.md) § 2).

| ID | Question | Why it matters for design | Evidence | Affects | Who can answer | Status |
|---|---|---|---|---|---|---|
| OQ-50 | When a student moves between stage campuses of one school, is that a promotion or a campus transfer? | The transfer flow must not offer routine stage moves; enrolment history shows them differently | Harrods Phnom Penh lists Early Years, Lower Primary, Upper Primary, Lower Secondary and Upper Secondary campuses | gsg-landscape, OBJ-03, campus transfer slice | GSG group office, Harrods | **Answered → DL-54** (the academic year decides: grade up into a new year is a promotion) |
| OQ-51 | Which curricula make up the group programme catalogue, and which brand labels are the same programme? | Programme offering pickers, grading scales and report cards | 34 curriculum labels on the campus list, e.g. "A-levels", "AS-A levels", "Cambridge 'A'"; Architecture v1 plans CBSE, IB and Cambridge packs only | OBJ-15, tenancy-and-scope | Group academics | Open |
| OQ-52 | Do campuses of different brands share a site, rooms, buses or gates? | Rooms, transport and visitor screens; cross-brand transfers between neighbours | GIIS SMART Campus and OWIS Digital Campus are both in Punggol; GIIS and OWIS both have a Whitefield campus in Bangalore | OBJ-13, Campus services | GSG group office | Open |
| OQ-53 | Which languages and scripts must names, notices and UI support per brand-country? | Name fields in native script, notice language, UI beyond English v1 | OWIS Saudi Arabia has an Arabic site; campuses in Japan, Cambodia and South Korea | OBJ-01, OQ-25 | Brand leads | Open |
| OQ-54 | Which campuses teach learners over 18, and how do guardian and consent rules apply to them? | Guardian links, consent and parent-app access for adult learners | Regent lists the Swinburne Foundation Program (university pathway) | OBJ-02, OBJ-04, OBJ-09 | Regent, counsel | Open |
| OQ-55 | What is the campus ID and code standard, and who assigns codes? | Campus pickers, exports, sample data codes | No codes on the campus list; names repeat across brands ("Whitefield", "Manila", "Dubai") | OBJ-13, sample data | GSG group office | Open |
| OQ-56 | What is each brand's legal name and display name? | Brand shown in scope breadcrumb, notices and exports | GSG pages use "Harrods International School" and "Harrods International Academy", "Chinese International School Manila" and "Chinese Indian International School", and two Heath House names | OBJ-13, glossary | GSG group office | Open |

## J · Organisation tree

Added 9 Nov 2026 with the [Organisation dictionary](../03-domain/organisation-data-dictionary.md).

| ID | Question | Why it matters for design | Evidence | Affects | Who can answer | Status |
|---|---|---|---|---|---|---|
| OQ-57 | Which campuses operate today, and are GIIS East Coast, GIIS Bangkok and OWIS Suntec still running? | Sample data uses East Coast (SG-ECS) for the same-entity transfer example; campus pickers must match reality | Not on the public campus list on 9 Nov 2026 but named elsewhere ([gsg-landscape](../01-context/gsg-landscape.md) § 1) | organisation dictionary, samples, S2 campus transfer | GSG group office | Open |
| OQ-58 | Which team creates, renames and closes brands, campuses and segments, and who approves a campus handover? | Campus catalogue and operator-change screens need an owner and an approver | Employee Data Dictionary E06 owner "Global Admin"; OQ-46 | OBJ-13, OBJ-14, permission-catalogue | Henry, GSG group office | Open |
| OQ-59 | Does GSG group brand-countries into regions (for example South-East Asia) for leadership, reporting or support? | The tree has no level between Group and Brand, so a regional head can't be granted a region | Not in Architecture or on the public site | tenancy-and-scope, rbac-model | GSG leadership | Open |

## K · Student directory lock

Added 12 Nov 2026 while locking the [student directory spec](../03-domain/student-directory-data-dictionary.md).

| ID | Question | Why it matters for design | Evidence | Affects | Who can answer | Status |
|---|---|---|---|---|---|---|
| OQ-60 | When a promotion changes campus, does it need the approvals a campus transfer needs (both principals), and does a promotion into another brand or country count as a promotion or as a new admission? | Year-end rollover screens and approval steps; cross-entity data moves | DL-54; HB p.235; DL-49; OQ-07 | student dictionary, OBJ-03 | Henry, Admissions owners | **Answered → DL-62, DL-63** |
| OQ-61 | When a guardian with profiles in two data locations changes their phone, email or address in one, does the change apply in the other? | Parent profile edit and staff guardian screens | DL-55; DL-28 | student dictionary, OBJ-04 | Henry, DPOs | **Answered → DL-64** |

## L · Admissions

Added 12 Nov 2026 with the [admissions dictionary](../03-domain/admissions-data-dictionary.md).

| ID | Question | Why it matters for design | Evidence | Affects | Who can answer | Status |
|---|---|---|---|---|---|---|
| OQ-62 | Can families apply online before enrolment, and with what login? (GIIS gives MyGIIS logins only on enrolment, HB p.34) | Decides whether there is a parent applicant portal or staff-entered applications only | Admissions dictionary; HB; Arch § Admission to enrolment | admissions dictionary, OBJ-16 | Henry, Admissions owners per brand | Open |
| OQ-63 | Who owns an enquiry that names several campuses or brands, and where is an unrouted group enquiry held? | Routing, ownership and data location for group-site enquiries (with OQ-29) | Admissions dictionary; HB; Arch § Admission to enrolment | admissions dictionary, OBJ-16 | Henry, Admissions owners per brand | Open |
| OQ-64 | Which brands, grades and offerings require entrance tests or interviews, what are the age-to-grade cut-offs, and do parents see results? | Assessment steps and grade suggestions | Admissions dictionary; HB; Arch § Admission to enrolment | admissions dictionary, OBJ-16 | Henry, Admissions owners per brand | Open |
| OQ-65 | Who sets seat limits, can they be overridden, and how is the waitlist ordered (siblings, staff children, date)? | Capacity view and waitlist screens | Admissions dictionary; HB; Arch § Admission to enrolment | admissions dictionary, OBJ-16 | Henry, Admissions owners per brand | **Answered → DL-61** |
| OQ-66 | Who decides an application and issues an offer (officer, manager, principal), and how long is an offer valid? | Approval step and expiry | Admissions dictionary; HB; Arch § Admission to enrolment | admissions dictionary, OBJ-16 | Henry, Admissions owners per brand | Open |
| OQ-67 | What contract applies in each country, who signs for the family, and is e-signature allowed? | Contract screen and signature flow; only GIIS SG (CPE) is documented | Admissions dictionary; HB; Arch § Admission to enrolment | admissions dictionary, OBJ-16 | Henry, Admissions owners per brand | Open |
| OQ-68 | Which fees are due at admission per brand and country (application, registration, deposit), how much, and when? | Fee step; HB names application and registration fees only | Admissions dictionary; HB; Arch § Admission to enrolment | admissions dictionary, OBJ-16 | Henry, Admissions owners per brand | Open |
| OQ-69 | How long are enquiries and unsuccessful applications kept, and what marketing consent is collected? | Retention and consent on enquiry forms (with OQ-10) | Admissions dictionary; HB; Arch § Admission to enrolment | admissions dictionary, OBJ-16 | Henry, Admissions owners per brand | Open |
| OQ-70 | Do education agents submit applications, and what can they see? (HB p.246 mentions an agent commission fee) | Agent channel and access | Admissions dictionary; HB; Arch § Admission to enrolment | admissions dictionary, OBJ-16 | Henry, Admissions owners per brand | Open |
| OQ-71 | How are returning students, deferred starts and cross-brand or cross-country moves at year end handled (new admission or not, OQ-60)? | Whether a returning student reuses ST01 and skips steps | Admissions dictionary; HB; Arch § Admission to enrolment | admissions dictionary, OBJ-16 | Henry, Admissions owners per brand | Open |
| OQ-72 | Which documents are required per country, grade and nationality? | Document checklist defaults | Admissions dictionary; HB; Arch § Admission to enrolment | admissions dictionary, OBJ-16 | Henry, Admissions owners per brand | Open |

## Research not yet done

These are evidence gaps, not product questions. They are listed in [research §11](../../research/2026-10-25-campus-os-foundation-research.md#11--open-questions-and-research-gaps): contextual inquiry with staff, family research, vendor verification, legal review and a systems inventory. No users have been interviewed yet, so every persona and scenario is an assumption.

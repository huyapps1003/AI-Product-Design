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
| OQ-14 | Where does *brand* sit in the tenancy and scope hierarchy? | The hierarchy (Group › Country › Campus) has no brand level, so brand-wide grants and brand settings can't be expressed | RES-F06, MC-02; DL-17 | tenancy-and-scope, rbac-model, role-catalogue | Henry, data architect | Open |
| OQ-15 | What access does GSG's platform team (admins, support, security) hold, and how is support access to school data controlled? | No platform-operator or support roles exist; the PowerSchool breach began with a support credential | RES-F06; research §06, §08; DL-09 | role-catalogue, rbac-model | Henry, GSG IT security | Open |
| OQ-16 | What do brand heads and brand academic leads need to see and do across countries? | Brand leadership has no role or persona | Research §03, §08 | role-catalogue, stakeholders | Brand heads | Open |
| OQ-17 | When one country hosts campuses of several brands or entities, how is a country-level grant limited? | A country grant can't tell brands or entities apart | Research §08 (row 3) | rbac-model, tenancy-and-scope | Data architect, DPOs | Open |
| OQ-18 | How is cross-cell access approved when group staff need data held in another country? | Layer 6 says data stays in its cell but not how cross-cell access is approved | MC-13; research §08 (row 6) | rbac-model | Data architect, DPOs | Open |
| OQ-19 | What access do students have beyond "self", and do franchise or JV partner staff use the platform? | No access path is defined for students or partner staff | Architecture § Users and roles; RES-F02 | role-catalogue | Henry | Open |

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
| OQ-35 | Is there one shared Person record across students, guardians and staff, or separate identities per role? | Decides whether a parent who is also staff has one profile or two | Architecture § Data model "Person"; Data Dictionary uses `employee_id` | student dictionary, rbac-model | Data architect | Open |
| OQ-36 | What is the format and uniqueness scope of the Student ID, and does it change on transfer? | Directory search, ID cards and exports show it | No source defines it | student dictionary | Henry, Admissions owners | **Answered → DL-31** (unique, kept on transfer); format still open |
| OQ-37 | Which identity attributes must a student record hold in each country (date of birth, sex or gender values, name structure, nationality, home language)? | Form fields and validation differ by country | MC-05 | student dictionary | Admissions owners, DPOs | Open |
| OQ-38 | Can parents edit the student profile, documents and contacts themselves, as in MyGIIS Profile Update? | DL-18 gave parents a read-only set | HB p.35; DL-18 | student dictionary, rbac-model | Henry | **Answered → DL-28** |
| OQ-39 | Do any student or guardian fields sit in the global identity and directory layer, and which cell holds a guardian whose children are in two countries? | Cross-campus search and transfers | Architecture § System architecture | student dictionary, data-controller-model | Data architect, DPOs | Open |
| OQ-40 | Which product term means a segment and which a class (Architecture uses "Section" for both)? | Labels on every enrolment screen | Architecture § Tenancy vs. § Data model | glossary, tenancy-and-scope | Henry | **Answered → DL-29** |
| OQ-41 | Can teachers apply the Student Pass attendance threshold without seeing Tier 3 immigration data? | Attendance warnings | Architecture § Absence and leave, § Security | student dictionary | Henry | **Answered → DL-30** |
| OQ-42 | Which health alerts (allergies, medical conditions), if any, appear on the student card for teachers, transport and security? | Tier 3 health data vs. safety on trips and buses | HB medical conditions; Architecture § Security | student dictionary, data-classification | Nurses, DPOs | Open |
| OQ-43 | How long are student and guardian records kept after withdrawal or graduation, per country, and what is deleted or anonymised? | Archive and delete states | regulatory-context; Data Dictionary Decision D12 | student dictionary, regulatory-context | Counsel per country | Open |
| OQ-44 | During migration, which system is authoritative for student data: MyGIIS, other brands' SIS, or Campus OS? | Which fields are read-only with a source chip | Architecture § Integrations; OQ-27, OQ-28 | student dictionary | GIIS IT, brand IT | Open |
| OQ-45 | Is AEPP a programme offering with its own enrolment, or a placement inside a regular class? | Whether an AEPP student has an AEPP enrolment or a flag on a regular one | Architecture § Admission to enrolment; HB § AEPP | student dictionary | Admissions owners, AEPP coordinator | Open |

## Research not yet done

These are evidence gaps, not product questions. They are listed in [research §11](../../research/2026-10-25-campus-os-foundation-research.md#11--open-questions-and-research-gaps): contextual inquiry with staff, family research, vendor verification, legal review and a systems inventory. No users have been interviewed yet, so every persona and scenario is an assumption.

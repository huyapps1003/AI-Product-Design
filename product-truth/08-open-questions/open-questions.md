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
| OQ-20 | Who owns programme pack masters and policy templates across brands, and which settings may a school change? | Settings screens need to show what is fixed by the group and what a school may override | MC-14; Architecture Q7; research §11 | tenancy-and-scope, variation-catalogue | GSG academic leads, brand heads | Partly answered → DL-110 (programme packs: GSG template per board, brand-country adapts); policy templates and other settings still Open |
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
| OQ-33 | Which non-GIIS brand and campuses should the canonical sample data include? | All sample campuses are GIIS, so designs can't show multi-brand cases (DL-03) | Research §10 (DL-03 verdict); RES-F01 | admin-platform README (sample data) | Henry | Answered → DL-03 (OWIS Singapore, Henry 22 Nov 2026) |
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
| OQ-47 | Who approves a Tier 3 type grant (A17), and for how long? | The request and approval screens for restricted record classes | DL-36; DL-30 (Admissions manages Student Pass cases) | rbac-model, data-classification | DPOs, Henry | **Answered → DL-69** (Campus Leader alone) |
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

## M · Classes

Added 14 Nov 2026 with the [classes dictionary](../03-domain/classes-data-dictionary.md).

| ID | Question | Why it matters for design | Evidence | Affects | Who can answer | Status |
|---|---|---|---|---|---|---|
| OQ-73 | Who prepares the class plan, allocates students and moves them during the year, and does a class move need approval or a notice to parents? | Who sees the allocation screens; whether a move is one step or an approval | Classes dictionary; HB p.9–14, p.128–131, p.169 | classes dictionary, OBJ-15 | Henry, principals and academic heads per brand | Open |
| OQ-74 | What class size limits apply per brand, country, programme and grade (board, regulator or early-years ratios)? | Class plan defaults and over-limit warnings | Classes dictionary; HB p.9–14, p.128–131, p.169 | classes dictionary, OBJ-15 | Henry, principals and academic heads per brand | Open |
| OQ-75 | When are class lists published, and what do parents and students see (class teacher, classmates' names)? | Parent app class card and publishing step | Classes dictionary; HB p.9–14, p.128–131, p.169 | classes dictionary, OBJ-15 | Henry, principals and academic heads per brand | Open |
| OQ-76 | How are houses allocated (siblings together, balanced by size), and does a student keep their house after a campus transfer or stage-campus promotion? | House allocation suggestions (with OQ-20) | Classes dictionary; HB p.9–14, p.128–131, p.169 | classes dictionary, OBJ-15 | Henry, principals and academic heads per brand | Open |
| OQ-77 | Are streams (CBSE 11 Science, Commerce) and elective sets part of the class, or subject groups owned by Teaching? | Class names like "11 Science A"; Grade 10 → 11 rollover | Classes dictionary; HB p.9–14, p.128–131, p.169 | classes dictionary, OBJ-15, OBJ-12 | Henry, principals and academic heads per brand | **Answered → DL-132** (Henry, 24 Nov 2026): streams are classes; electives and language choices are teaching groups |
| OQ-78 | Do any campuses run composite or mixed-age classes (Montessori/GMP, small campuses) beyond AEPP? | Whether a regular class can span grades or offerings | Classes dictionary; HB p.9–14, p.128–131, p.169 | classes dictionary, OBJ-15 | Henry, principals and academic heads per brand | Open |
| OQ-79 | When results arrive after the rollover (re-tests, conditional promotion, Grade 10 and 12 board results), how and by when is the class changed, and who decides retention? | Late changes to an applied rollover | Classes dictionary; HB p.9–14, p.128–131, p.169 | classes dictionary, OBJ-15 | Henry, principals and academic heads per brand | Open |

## N · Fees & Billing

Added 14 Nov 2026 with the [fees & billing dictionary](../03-domain/fees-billing-data-dictionary.md).

| ID | Question | Why it matters for design | Evidence | Affects | Who can answer | Status |
|---|---|---|---|---|---|---|
| OQ-80 | Which fee types and billing frequencies does each brand-country use (term, monthly, annual upfront), and are there early-payment or annual-payment discounts? | Fee plan editor, billing periods, parent fee schedule | Fees & billing dictionary; Arch § Fees, withdrawal and refund; HB p.237–247 | fees & billing dictionary, OBJ-23 | Henry, Finance Controllers per brand-country | Open |
| OQ-81 | Which accounting system or ERP does each brand-country use, and what journal detail and chart of accounts does it need? | Ledger codes on fee types; journal export | Fees & billing dictionary; Arch § Fees, withdrawal and refund; HB p.237–247 | fees & billing dictionary, OBJ-23 | Henry, Finance Controllers per brand-country | Open |
| OQ-82 | Which payment methods and gateways does each country use (cards, PayNow, UPI, GIRO, bank transfer, cheque, cash), and who pays card charges? | Pay screen, counter screen, surcharges | Fees & billing dictionary; Arch § Fees, withdrawal and refund; HB p.237–247 | fees & billing dictionary, OBJ-23 | Henry, Finance Controllers per brand-country | Open |
| OQ-83 | What discounts exist per brand (sibling, staff child, early bird), do they stack, and who approves discounts, scholarships, waivers and credit notes, up to what amount? | Adjustment approval steps and limits | Fees & billing dictionary; Arch § Fees, withdrawal and refund; HB p.237–247 | fees & billing dictionary, OBJ-23 | Henry, Finance Controllers per brand-country | Open |
| OQ-84 | What are the due dates and reminder schedule, are late fees charged, and does an overdue balance have any consequence (report card, exam slip, re-enrolment, transfer certificate)? | Reminder settings; what other modules may do with fee status (DL-83) | Fees & billing dictionary; Arch § Fees, withdrawal and refund; HB p.237–247 | fees & billing dictionary, OBJ-23 | Henry, Finance Controllers per brand-country | Open |
| OQ-85 | Who can be a payer (one or both parents, a split, an employer, an embassy, a scholarship body), and do sponsors get statements or a login? | Billing account setup and payer picker | Fees & billing dictionary; Arch § Fees, withdrawal and refund; HB p.237–247 | fees & billing dictionary, OBJ-23 | Henry, Finance Controllers per brand-country | Open |
| OQ-86 | What are the Table 3A and 3B percentages in the GIIS SG contract (the handbook shows them only as images), and what refund rules, approvers and deadlines apply in other brand-countries? | Refund calculator; only GIIS SG text rules are known | Fees & billing dictionary; Arch § Fees, withdrawal and refund; HB p.237–247 | fees & billing dictionary, OBJ-23 | Henry, Finance Controllers per brand-country | Open |
| OQ-87 | Which fees carry GST or VAT per country, at what rate, and is e-invoicing required (for example India GST e-invoices)? | Tax lines, invoice numbering and format | Fees & billing dictionary; Arch § Fees, withdrawal and refund; HB p.237–247 | fees & billing dictionary, OBJ-23 | Henry, Finance Controllers per brand-country | Open |
| OQ-88 | Which countries besides Singapore require fee protection, escrow or deposit protection, and what must be recorded? | Regulator packs and the FPS record | Fees & billing dictionary; Arch § Fees, withdrawal and refund; HB p.237–247 | fees & billing dictionary, OBJ-23 | Henry, Finance Controllers per brand-country | Open |
| OQ-89 | Do any brands take security deposits or caution money, and when are they returned? | DEPOSIT fee category; exit clearance | Fees & billing dictionary; Arch § Fees, withdrawal and refund; HB p.237–247 | fees & billing dictionary, OBJ-23 | Henry, Finance Controllers per brand-country | Open |
| OQ-90 | How much fee history moves from MyGIIS and current finance systems at cutover, and are opening balances imported? | Parent History tab on day one (MyGIIS parity) | Fees & billing dictionary; Arch § Fees, withdrawal and refund; HB p.237–247 | fees & billing dictionary, OBJ-23 | Henry, Finance Controllers per brand-country | Open |
| OQ-91 | How long must invoices, receipts and refund records be kept per country, and what may be deleted after a family leaves? | Retention policy for finance records (with OQ-10) | Fees & billing dictionary; Arch § Fees, withdrawal and refund; HB p.237–247 | fees & billing dictionary, OBJ-23 | Henry, Finance Controllers per brand-country | Open |
| OQ-92 | Can a campus bill in a currency other than its country's (for example USD), and who sets exchange rates? | Currency on plans, invoices, payments | Fees & billing dictionary; Arch § Fees, withdrawal and refund; HB p.237–247 | fees & billing dictionary, OBJ-23 | Henry, Finance Controllers per brand-country | Open |

## O · Assessment & Report Cards

Added 15 Nov 2026 with the [assessment dictionary](../03-domain/assessment-data-dictionary.md).

| ID | Question | Why it matters for design | Evidence | Affects | Who can answer | Status |
|---|---|---|---|---|---|---|
| OQ-93 | Who approves an assessment scheme for a whole brand-country (the Academic Supervisor is a campus role), who in GSG maintains the board templates, and may one campus vary its brand-country scheme? | Scheme approval step; who sees "Submit for approval" | Assessment dictionary; HB p.43, p.65–88, p.127–133; Exam Schedule 2026-27 | assessment dictionary, OBJ-19 | Henry, GSG academic leadership | Open |
| OQ-94 | What report card layouts does each brand use (sections, signatures, logos, languages, printed or download only)? | Report card preview and layout setting | Assessment dictionary; HB p.43, p.65–88, p.127–133; Exam Schedule 2026-27 | assessment dictionary, OBJ-19 | Henry, academic heads per brand | Open |
| OQ-95 | Does a release need the Principal's sign-off, and what is the release deadline per brand (GIIS SG: within one month, HB p.127)? | Release button and deadline warnings | Assessment dictionary; HB p.43, p.65–88, p.127–133; Exam Schedule 2026-27 | assessment dictionary, OBJ-19 | Henry, principals | Open |
| OQ-96 | From which grade may students see their own marks and report cards in the student app? | Student app results tab (extends DL-68) | Assessment dictionary; HB p.43, p.65–88, p.127–133; Exam Schedule 2026-27 | assessment dictionary, OBJ-19 | Henry, principals | Open |
| OQ-97 | The GIIS handbook gives two appeal windows: 2 working days after scripts are returned (CBSE 1–8, p.88) and at least 7 working days from results (p.127). Which applies to which grades, and what do other brands use? | Appeal window setting and Appeal button cut-off | Assessment dictionary; HB p.43, p.65–88, p.127–133; Exam Schedule 2026-27 | assessment dictionary, OBJ-19 | Henry, GIIS SG academic team | Open |
| OQ-98 | Who is the "Examination Board" that endorses appeal outcomes (HB p.127), and who moderates mark sheets (coordinator or HOD)? | Appeal and moderation approvers | Assessment dictionary; HB p.43, p.65–88, p.127–133; Exam Schedule 2026-27 | assessment dictionary, OBJ-19 | Henry, GIIS SG academic team | Open |
| OQ-99 | Who confirms straight promotions (coordinator or Principal), and how are re-tests and improvement tests scheduled and combined with the original mark? | Promotion review and re-test results (with OQ-79) | Assessment dictionary; HB p.43, p.65–88, p.127–133; Exam Schedule 2026-27 | assessment dictionary, OBJ-19 | Henry, principals | Open |
| OQ-100 | How do Grade 10 and 12 board results and IB final results come in (import or manual), and do they appear on report cards? | BOARD_RESULT_PENDING lines; integrations scope | Assessment dictionary; HB p.43, p.65–88, p.127–133; Exam Schedule 2026-27 | assessment dictionary, OBJ-19 | Henry, exam officers | Open |
| OQ-101 | Is the online exam portal (Arch step 3) a separate product, and do its marks import into mark sheets? | Mark sheet Import action | Assessment dictionary; HB p.43, p.65–88, p.127–133; Exam Schedule 2026-27 | assessment dictionary, OBJ-19 | Henry, GSG IT | Open |
| OQ-102 | How are marks handled for mid-year joiners and transfers in (prorated, carried from the previous campus, left blank)? | Incomplete results for joiners | Assessment dictionary; HB p.43, p.65–88, p.127–133; Exam Schedule 2026-27 | assessment dictionary, OBJ-19 | Henry, coordinators | Open |
| OQ-103 | What assessment schemes do brands other than GIIS use (OWIS, Harrods, others)? | Template coverage; non-GIIS samples are synthetic | Assessment dictionary; HB p.43, p.65–88, p.127–133; Exam Schedule 2026-27 | assessment dictionary, OBJ-19 | Henry, academic heads per brand | Open |
| OQ-104 | Does PYP reporting stay in Toddle (HB p.87), and does the KG Holistic Progress Card move to Campus OS? | Whether descriptor report cards are in Phase 2 | Assessment dictionary; HB p.43, p.65–88, p.127–133; Exam Schedule 2026-27 | assessment dictionary, OBJ-19 | Henry, PYP and KG coordinators | Open |
| OQ-105 | Who enters co-scholastic and Holistic Progress Card descriptors, and against which indicators? | Descriptor entry screens | Assessment dictionary; HB p.43, p.65–88, p.127–133; Exam Schedule 2026-27 | assessment dictionary, OBJ-19 | Henry, coordinators | Open |
| OQ-106 | How long are marks, report cards and answer-script records kept (some scripts five years, HB p.88), and how much MyGIIS results history moves at cutover? | Retention policy; past report cards on day one | Assessment dictionary; HB p.43, p.65–88, p.127–133; Exam Schedule 2026-27 | assessment dictionary, OBJ-19 | Henry, DPO, GSG IT | Open |
| OQ-107 | When do IB PYP and KG (GMP) years start at each GIIS SG campus, and are Noida's staggered starts by grade band (7-8 April) separate academic years or one year with different first days? | Year dates per offering (DL-96); rollover timing; billing periods | HB p.68, p.87, p.109; Exam Schedule 2026-27; Noida Almanac 2025-26 | DL-96, classes, fees, assessment dictionaries | Henry, campus academic heads | Open |

## P · Attendance

Added 19 Nov 2026 with the [attendance dictionary](../03-domain/attendance-data-dictionary.md).

| ID | Question | Why it matters for design | Evidence | Affects | Who can answer | Status |
|---|---|---|---|---|---|---|
| OQ-108 | Does approved leave (especially medical leave with a certificate) count against the attendance %, or only against the ICA rule's "without a valid reason"? HB p.23 says 90% for all students; p.166 gives 90/85/75% by group. | The % parents see; promotion and ICA | Attendance dictionary; HB p.23, p.128–131, p.166–167, p.174; Holiday Schedule 2026-27 | attendance dictionary, OBJ-20 | Henry, GIIS SG school leadership | **Answered → DL-103** (leave) and **DL-108** (minimum by student group, HB p.166), 21 Nov 2026 |
| OQ-109 | Who approves leave when the class teacher or Principal is away, and do other brands route by days the same way? | Approval routing and delegation | Attendance dictionary; HB p.23, p.128–131, p.166–167, p.174; Holiday Schedule 2026-27 | attendance dictionary, OBJ-20 | Henry, GIIS SG school leadership | Open |
| OQ-110 | What are the register cut-off and lock times per campus, who may correct a locked mark, who takes the register when the class teacher is absent, and with period registers which period decides LATE and LEFT_EARLY? | Overdue registers, correction approvals, cover | Attendance dictionary; HB p.23, p.128–131, p.166–167, p.174; Holiday Schedule 2026-27 | attendance dictionary, OBJ-20 | Henry, GIIS SG school leadership | Open |
| OQ-111 | How are partial days (half-day leave, appointments) recorded, and what are the late-arrival steps for Middle and Senior students (HB p.174–175)? | Mark codes and repeated-late alerts | Attendance dictionary; HB p.23, p.128–131, p.166–167, p.174; Holiday Schedule 2026-27 | attendance dictionary, OBJ-20 | Henry, GIIS SG school leadership | Open |
| OQ-112 | At GIIS SG, who files the ICA notice, how, within what time, and what counts as a "valid reason"? | ICA review Case steps | Attendance dictionary; HB p.23, p.128–131, p.166–167, p.174; Holiday Schedule 2026-27 | attendance dictionary, OBJ-20 | Henry, GIIS SG school leadership | Open |
| OQ-113 | What attendance thresholds and reporting duties apply to other brands and countries (GIIS India, OWIS, regulators)? | Rule defaults beyond GIIS SG | Attendance dictionary; HB p.23, p.128–131, p.166–167, p.174; Holiday Schedule 2026-27 | attendance dictionary, OBJ-20 | Henry, GIIS SG school leadership | Open |
| OQ-114 | Do school activities (trips, competitions, HB p.167) count as present, and who records them? | SCHOOL_ACTIVITY code | Attendance dictionary; HB p.23, p.128–131, p.166–167, p.174; Holiday Schedule 2026-27 | attendance dictionary, OBJ-20 | Henry, GIIS SG school leadership | Open |
| OQ-115 | For mid-year joiners and transfers in, is the % measured from the start date, and does a stage-campus move carry it across? | Summary denominator | Attendance dictionary; HB p.23, p.128–131, p.166–167, p.174; Holiday Schedule 2026-27 | attendance dictionary, OBJ-20 | Henry, GIIS SG school leadership | Open |
| OQ-116 | Is there an authorised-pickup list per student for out passes, and where does it live? | Out pass check | Attendance dictionary; HB p.23, p.128–131, p.166–167, p.174; Holiday Schedule 2026-27 | attendance dictionary, OBJ-20 | Henry, GIIS SG school leadership | Open |
| OQ-117 | From which grade may students see their own attendance? | Student app attendance (extends DL-68) | Attendance dictionary; HB p.23, p.128–131, p.166–167, p.174; Holiday Schedule 2026-27 | attendance dictionary, OBJ-20 | Henry, GIIS SG school leadership | Open |
| OQ-118 | How long are attendance records and medical certificates kept, and how much MyGIIS attendance history moves at cutover? | Retention; history on day one | Attendance dictionary; HB p.23, p.128–131, p.166–167, p.174; Holiday Schedule 2026-27 | attendance dictionary, OBJ-20 | Henry, GIIS SG school leadership | Open |
| OQ-119 | Are there Saturday school days, exam study-leave days or grade-only days off? | School-day calendar by grade | Attendance dictionary; HB p.23, p.128–131, p.166–167, p.174; Holiday Schedule 2026-27 | attendance dictionary, OBJ-20 | Henry, GIIS SG school leadership | Open |
| OQ-120 | Who maintains holidays and closures (campus office or brand-country), and who adds unplanned closures? | Calendar edit rights | Attendance dictionary; HB p.23, p.128–131, p.166–167, p.174; Holiday Schedule 2026-27 | attendance dictionary, OBJ-20 | Henry, GIIS SG school leadership | Open |
| OQ-121 | Do gate devices exist at GIIS SG campuses today, and what do they send? | Device feed vs desk entry (Phase 2) | Attendance dictionary; HB p.23, p.128–131, p.166–167, p.174; Holiday Schedule 2026-27 | attendance dictionary, OBJ-20 | Henry, GIIS SG school leadership | Open |

## Q · Academic structure

Added 22 Nov 2026 with the [academic structure dictionary](../03-domain/academic-structure-data-dictionary.md).

| ID | Question | Why it matters for design | Evidence | Affects | Who can answer | Status |
|---|---|---|---|---|---|---|
| OQ-122 | Who approves a brand-country pack version, and a new or closing programme offering at a campus (brand-country academic head, Academic Supervisor, Country Director)? | Approver on pack and offering screens | Academic structure dictionary; HB p.47–62, p.68, p.109; Holiday Schedule 2026-27 | academic structure dictionary, OBJ-25 | Henry, GSG academic leads | Open |
| OQ-123 | Are the GMP "Extended" courses (Nursery, K1, K2 Extended, HB p.48–49) longer-day variants of the same grade, separate grades, or a different offering? | Grade list, class names ("K1 A Extnd"), fee plans | Academic structure dictionary; HB p.47–62, p.68, p.109; Holiday Schedule 2026-27 | academic structure dictionary, OBJ-25 | Henry, GIIS SG school leadership | Open |
| OQ-124 | Which board codes and accreditation numbers (CBSE affiliation and subject codes, IB school code, Cambridge centre number) must Campus OS hold, and where are they printed? | Report cards, transfer certificates, board registrations | Academic structure dictionary; HB p.47–62, p.68, p.109; Holiday Schedule 2026-27 | academic structure dictionary, OBJ-25 | Henry, principals and academic heads per brand | Open |
| OQ-125 | What subject-choice rules apply per grade (Language 2 and 3 combinations, CBSE 11 combinations, IB DP 3 HL + 3 SL plus core), are they blocking or a warning, and which next-grade routes exist after CLSP 8, IGCSE 10 and IB PYP 5? | Subject choice screens; rollover next grade | Academic structure dictionary; HB p.47–62, p.68, p.109; Holiday Schedule 2026-27 | academic structure dictionary, OBJ-25 | Henry, principals and academic heads per brand | Open |
| OQ-126 | Who records regulator approval for a subject (HB p.54 Applied Mathematics), and who may change a published year's or term's dates? | Pending-approval subjects; date-change approval | Academic structure dictionary; HB p.47–62, p.68, p.109; Holiday Schedule 2026-27 | academic structure dictionary, OBJ-25 | Henry, principals | Partly answered → DL-113 (brand-country academic head approves date changes); regulator approval for subjects still Open |
| OQ-127 | What grade labels does each brand-country use (Class, Grade, Year, Standard; Nursery vs Pre-K)? The handbook says "Class 7"; the classes sample says "Grade 7". | Every screen that shows a grade | Academic structure dictionary; HB p.47–62, p.68, p.109; Holiday Schedule 2026-27 | academic structure dictionary, OBJ-25 | Henry, academic heads per brand | Open |
| OQ-128 | What entry ages per grade apply per country (cut-off dates), and should Admissions check them? | Admissions age check (AD02) | Academic structure dictionary; HB p.47–62, p.68, p.109; Holiday Schedule 2026-27 | academic structure dictionary, OBJ-25 | Henry, Admissions owners per brand | Open |
| OQ-129 | Are coordinator bands the same as assessment bands (e.g. CBSE 6–8 for both), or do campuses group grades differently for people and for exams? | One band list or two | Academic structure dictionary; HB p.47–62, p.68, p.109; Holiday Schedule 2026-27 | academic structure dictionary, OBJ-25 | Henry, GIIS SG academic team | Open |

## R · Cases & Communications

Added 22 Nov 2026 with the [cases & communications dictionary](../03-domain/cases-communications-data-dictionary.md).

| ID | Question | Why it matters for design | Evidence | Affects | Who can answer | Status |
|---|---|---|---|---|---|---|
| OQ-130 | What counts as a "working day" for case deadlines at each campus: are school vacations working days for the office, what about India's Saturdays (NWD), and at what time does a due day end? | Every case deadline date | Cases & communications dictionary; HB p.8, p.24, p.34, p.233–237, p.245, p.249–250 | cases & communications dictionary, OBJ-10, OBJ-26 | Henry, GIIS SG school leadership | Open |
| OQ-131 | Does the complaint clock stop while the school waits for the complainant, and are the 14 and 21 working days counted from receipt or from the previous step? | Deadline ladder dates | Cases & communications dictionary; HB p.8, p.24, p.34, p.233–237, p.245, p.249–250 | cases & communications dictionary, OBJ-10, OBJ-26 | Henry, GIIS SG school leadership | Open |
| OQ-132 | Are the enrolment clarification response times (Admissions Manager 48 hours; Finance Controller and Customer Relations 24 hours, HB p.24) cases with hour-based deadlines, or service targets on messages? | Hours vs working days | Cases & communications dictionary; HB p.8, p.24, p.34, p.233–237, p.245, p.249–250 | cases & communications dictionary, OBJ-10, OBJ-26 | Henry, GIIS SG school leadership | Open |
| OQ-133 | Which complaint topics and process owners does GIIS SG use, and who is each owner's supervisor at step 14? | Routing and escalation | Cases & communications dictionary; HB p.8, p.24, p.34, p.233–237, p.245, p.249–250 | cases & communications dictionary, OBJ-10, OBJ-26 | Henry, GIIS SG school leadership | Open |
| OQ-134 | Are anonymous complaints accepted, and how are complaints about a named staff member handled (who must not see them)? | Feedback form; restricted complaints | Cases & communications dictionary; HB p.8, p.24, p.34, p.233–237, p.245, p.249–250 | cases & communications dictionary, OBJ-10, OBJ-26 | Henry, GIIS SG school leadership | Open |
| OQ-135 | When is the internal review of compliments and suggestions held (HB p.249 C), and who attends? | Feedback review cycle and report | Cases & communications dictionary; HB p.8, p.24, p.34, p.233–237, p.245, p.249–250 | cases & communications dictionary, OBJ-10, OBJ-26 | Henry, GIIS SG school leadership | Open |
| OQ-136 | Should the system check messages for Tier 3 content (health, immigration) or only warn the sender? | Message composer | Cases & communications dictionary; HB p.8, p.24, p.34, p.233–237, p.245, p.249–250 | cases & communications dictionary, OBJ-10, OBJ-26 | Henry, DPO | Open |
| OQ-137 | How long are cases, messages, announcements and delivery logs kept, can a sender delete a message, and how much MyGIIS message history moves at cutover? | Retention, migration | Cases & communications dictionary; HB p.8, p.24, p.34, p.233–237, p.245, p.249–250 | cases & communications dictionary, OBJ-10, OBJ-26 | Henry, DPO, GIIS SG IT | Open |
| OQ-138 | Who approves a grade-wide or campus-wide announcement at GIIS SG (coordinator, Vice Principal, Principal, marketing), and who may send brand-country announcements? | Announcement approval | Cases & communications dictionary; HB p.8, p.24, p.34, p.233–237, p.245, p.249–250 | cases & communications dictionary, OBJ-10, OBJ-26 | Henry, GIIS SG school leadership | Open |
| OQ-139 | Which channels does each brand-country use today (email, SMS, WhatsApp Business, app push), with which providers, and who pays for SMS and WhatsApp? | Channels on templates and preferences | Cases & communications dictionary; HB p.8, p.24, p.34, p.233–237, p.245, p.249–250 | cases & communications dictionary, OBJ-10, OBJ-26 | Henry, GSG IT | Open |
| OQ-140 | Are there quiet hours for teacher ↔ parent messages and optional notifications, and is there an expected reply time for teachers? | Messages, notification settings | Cases & communications dictionary; HB p.8, p.24, p.34, p.233–237, p.245, p.249–250 | cases & communications dictionary, OBJ-10, OBJ-26 | Henry, GIIS SG school leadership | Open |
| OQ-141 | Should result appeals (AS11), which keep their own status and deadline, also run on a Case so they appear in the same inbox? | Case inbox coverage | Cases & communications dictionary; HB p.8, p.24, p.34, p.233–237, p.245, p.249–250 | cases & communications dictionary, OBJ-10, OBJ-26 | Henry | Open |
| OQ-142 | May a Principal or coordinator read teacher ↔ parent conversations, and when (complaint, safeguarding concern, on request)? | Conversation oversight; privacy notice | Cases & communications dictionary; HB p.8, p.24, p.34, p.233–237, p.245, p.249–250 | cases & communications dictionary, OBJ-10, OBJ-26 | Henry, DPO, school leadership | Open |
| OQ-143 | What complaint processes do other brands and countries use (OWIS Singapore, GIIS India; regulators other than CPE), and do they have a dispute committee? | Case policies beyond GIIS SG | Cases & communications dictionary; HB p.8, p.24, p.34, p.233–237, p.245, p.249–250 | cases & communications dictionary, OBJ-10, OBJ-26 | Henry, brand leads | Open |
| OQ-144 | Who is the recipient of a complaint that arrives at a general helpdesk address (HB p.8), and who triages the helpdesk queue? | Intake rule, queues | Cases & communications dictionary; HB p.8, p.24, p.34, p.233–237, p.245, p.249–250 | cases & communications dictionary, OBJ-10, OBJ-26 | Henry, GIIS SG school leadership | Open |
| OQ-145 | Do students raise complaints or message teachers in the system, and from which grade? | Student app, feedback form | Cases & communications dictionary; HB p.8, p.24, p.34, p.233–237, p.245, p.249–250 | cases & communications dictionary, OBJ-10, OBJ-26 | Henry, school leadership | Open |
| OQ-146 | Which notifications must reach both guardians, and which only the primary contact or bill payer? | Notification recipients | Cases & communications dictionary; HB p.8, p.24, p.34, p.233–237, p.245, p.249–250 | cases & communications dictionary, OBJ-10, OBJ-26 | Henry, GIIS SG school leadership | Open |

## S · Teaching & Timetable

Added 24 Nov 2026 with the [teaching & timetable dictionary](../03-domain/teaching-timetable-data-dictionary.md).

| ID | Question | Why it matters for design | Evidence | Affects | Who can answer | Status |
|---|---|---|---|---|---|---|
| OQ-147 | What are the period times at each GIIS SG campus (how many periods, how long, when breaks and lunch fall), do they differ by segment, and which weekdays are assembly days? | Bell schedules (TE01, TE02); every timetable grid | Teaching & timetable dictionary; HB p.9–14, p.22, p.34–35, p.52, p.141–142 | teaching & timetable dictionary, OBJ-27 | Henry, GIIS SG school leadership | Open |
| OQ-148 | Does each campus run a Monday–Friday week or a rotating cycle (Day 1–6), and do Indian campuses have Saturday lessons? | Timetable cycle; school-day calendar link | Teaching & timetable dictionary; HB p.9–14, p.22, p.34–35, p.52, p.141–142 | teaching & timetable dictionary, OBJ-27 | Henry, GIIS SG and India school leadership | Open |
| OQ-149 | What is the maximum teaching load per week for a full-time teacher, the minimum free periods, and how is part-time load set? | Load warnings (SR-TE-07) | Teaching & timetable dictionary; HB p.9–14, p.22, p.34–35, p.52, p.141–142 | teaching & timetable dictionary, OBJ-12, OBJ-27 | Henry, GIIS SG school leadership, HR | Open |
| OQ-150 | Who builds the timetable at each campus (coordinator, a timetabler, Vice Principal), who approves period plans, and who publishes? | Capabilities and approval step | Teaching & timetable dictionary; HB p.9–14, p.22, p.34–35, p.52, p.141–142 | teaching & timetable dictionary, OBJ-27 | Henry, GIIS SG school leadership | Open |
| OQ-151 | Which tool builds timetables today (aSc, Untis, spreadsheet, MyGIIS), and should Campus OS import its output at cutover? | Import path; v1 effort | Teaching & timetable dictionary; HB p.9–14, p.22, p.34–35, p.52, p.141–142 | teaching & timetable dictionary, OBJ-27 | Henry, GIIS SG IT and academic team | Open |
| OQ-152 | How are language and elective groups formed: one group per subject across a grade, or split; what is the largest group; are ESL and learning support pulled out of other lessons? | Group sizes, SPLIT groups | Teaching & timetable dictionary; HB p.9–14, p.22, p.34–35, p.52, p.141–142 | teaching & timetable dictionary, OBJ-12, OBJ-27 | Henry, GIIS SG academic team | Open |
| OQ-153 | Are IB DP (and CBSE 11–12 elective) subjects taught in fixed option blocks, and what happens to a student whose choices clash? | Block rules (TE04 `block_code`) | Teaching & timetable dictionary; HB p.9–14, p.22, p.34–35, p.52, p.141–142 | teaching & timetable dictionary, OBJ-12, OBJ-27 | Henry, IB DP Coordinator SG-ECS | Open |
| OQ-154 | Do teachers teach at both SG-PGL and SG-ECS, on the same day, and how much travel time is needed between campuses? | Cross-campus clash rule | Teaching & timetable dictionary; HB p.9–14, p.22, p.34–35, p.52, p.141–142 | teaching & timetable dictionary, OBJ-27 | Henry, GIIS SG school leadership | Open |
| OQ-155 | Where does the room list live today (HB p.9–14 lists one room per class), and are labs and halls booked separately? | Room list import; booking scope | Teaching & timetable dictionary; HB p.9–14, p.22, p.34–35, p.52, p.141–142 | teaching & timetable dictionary, OBJ-27 | Henry, GIIS SG campus office | Open |
| OQ-156 | Do CCAs, assemblies, library and off-site lessons (swimming) appear on the timetable as lessons? | ACTIVITY periods and groups | Teaching & timetable dictionary; HB p.9–14, p.22, p.34–35, p.52, p.141–142 | teaching & timetable dictionary, OBJ-27 | Henry, GIIS SG academic team | Open |
| OQ-157 | Which timetable changes are families told about (new version, cancelled lesson, room change), and how? | Change notices | Teaching & timetable dictionary; HB p.9–14, p.22, p.34–35, p.52, p.141–142 | teaching & timetable dictionary, OBJ-27 | Henry, GIIS SG school leadership | Open |
| OQ-158 | How is co-teaching counted (shared load, both enter marks), and are learning support teachers assigned to groups? | Assignment roles and load | Teaching & timetable dictionary; HB p.9–14, p.22, p.34–35, p.52, p.141–142 | teaching & timetable dictionary, OBJ-12, OBJ-27 | Henry, GIIS SG academic team | Open |
| OQ-159 | Must a teacher be approved or qualified for a subject or board (CBSE, Cambridge, IB) before an assignment, and who keeps that record? | Assignment check; Staff and HR link | Teaching & timetable dictionary; HB p.9–14, p.22, p.34–35, p.52, p.141–142 | teaching & timetable dictionary, OBJ-12, OBJ-27 | Henry, GIIS SG HR and academic heads | Partly answered → DL-158 (qualification record kept in Staff & HR; a missing one warns, Assumed); whether it is required stays Open |
| OQ-160 | How often do timetables change mid-year, who may make one-off room or period changes, and with how much notice? | Change rights; version cadence | Teaching & timetable dictionary; HB p.9–14, p.22, p.34–35, p.52, p.141–142 | teaching & timetable dictionary, OBJ-27 | Henry, GIIS SG school leadership | Open |
| OQ-161 | How do OWIS Singapore and other brands structure their day and timetable (periods, cycle, blocks)? | Bell schedule and cycle beyond GIIS; OWIS sample is synthetic | Teaching & timetable dictionary; HB p.9–14, p.22, p.34–35, p.52, p.141–142 | teaching & timetable dictionary, OBJ-27 | Henry, OWIS leads | Open |
| OQ-162 | Who arranges cover at each GIIS campus (level coordinator, Vice Principal, a cover officer), and by what time each morning? | Cover board owner, urgent flag | Teaching & timetable dictionary; HB p.9–14, p.22, p.34–35, p.52, p.141–142 | teaching & timetable dictionary, OBJ-27 | Henry, GIIS SG school leadership | Open |
| OQ-163 | How do teachers report an absence today (call, email, HR system), by when, and will Staff and HR leave feed Teaching automatically? | Absence source and timing | Teaching & timetable dictionary; HB p.9–14, p.22, p.34–35, p.52, p.141–142 | teaching & timetable dictionary, OBJ-27 | Henry, GIIS SG HR | **Partly answered → DL-156** (Staff & HR leave creates the absence automatically, Assumed); how and by when teachers report: OQ-168 |
| OQ-164 | Is there a weekly cover limit per teacher, are relief (external) teachers used, and are covers paid or counted toward load? | Suggestion rule, relief teachers | Teaching & timetable dictionary; HB p.9–14, p.22, p.34–35, p.52, p.141–142 | teaching & timetable dictionary, OBJ-27 | Henry, GIIS SG school leadership, HR | Open |
| OQ-165 | When the class teacher is away, does the first period's cover teacher take the day register, or the co-class teacher (CL03)? | Register ownership on cover days | Teaching & timetable dictionary; HB p.9–14, p.22, p.34–35, p.52, p.141–142 | teaching & timetable dictionary, OBJ-27 | Henry, GIIS SG school leadership | Open |

## T · Staff & HR

Added 25 Nov 2026 with the [staff & HR dictionary](../03-domain/staff-hr-data-dictionary.md).

| ID | Question | Why it matters for design | Evidence | Affects | Who can answer | Status |
|---|---|---|---|---|---|---|
| OQ-166 | Which HR and payroll systems does each brand-country use today, when can each stop editing staff data (go-live date), and can each payroll system accept a feed from Campus OS? | Cutover plan; payroll feed (SR-HR-01, DL-147) | Staff & HR dictionary; Employee DD D01–D15; HB p.138, p.210–213 | staff & HR dictionary, OBJ-05 | Henry, HR heads per brand-country | Open |
| OQ-167 | Do staff hold contracts with two GSG legal entities at once (for example teaching at GIIS and OWIS), and does group or country office staff have a home campus? | Concurrent employments; primary campus | Staff & HR dictionary; Employee DD D01–D15; HB p.138, p.210–213 | staff & HR dictionary, OBJ-06 | Henry, Group HR | Open |
| OQ-168 | How and by when do GIIS staff report sick today, and who is told first (line manager, coordinator, HR)? | Same-day report cut-off; cover timing (with OQ-163) | Staff & HR dictionary; Employee DD D01–D15; HB p.138, p.210–213 | staff & HR dictionary, OBJ-28 | Henry, GIIS SG HR | Open |
| OQ-169 | What are the leave types and entitlements at GIIS SG, GIIS India and OWIS SG (by category and service), the leave year, and the carry-forward rule? | Leave policies; every balance | Staff & HR dictionary; Employee DD D01–D15; HB p.138, p.210–213 | staff & HR dictionary, OBJ-28 | Henry, HR heads per brand-country | Open |
| OQ-170 | Can teaching staff take annual leave on school days at all, and do school vacations count as leave for teachers? | School-day rule; days counted | Staff & HR dictionary; Employee DD D01–D15; HB p.138, p.210–213 | staff & HR dictionary, OBJ-28 | Henry, GIIS SG HR and school leadership | Open |
| OQ-171 | Which checks does Singapore require for school staff (police or background check, CPE teacher requirements, child-protection training), how often, and for which roles? | GIIS SG requirements | Staff & HR dictionary; Employee DD D01–D15; HB p.138, p.210–213 | staff & HR dictionary, OBJ-29 | Henry, GIIS SG HR | Open |
| OQ-172 | Which checks and registrations does India require (police verification, CBSE affiliation norms for teacher qualifications), and how are they renewed? | GIIS India requirements | Staff & HR dictionary; Employee DD D01–D15; HB p.138, p.210–213 | staff & HR dictionary, OBJ-29 | Henry, GIIS India HR | Open |
| OQ-173 | Who in HR may see work pass and background check results, and who approves their Tier 3 type grant? | A17 holders; T3 screens | Staff & HR dictionary; Employee DD D01–D15; HB p.138, p.210–213 | staff & HR dictionary, OBJ-29 | Henry, DPOs | Open |
| OQ-174 | Who approves a vacancy and an offer (Principal, Country Director, Finance Controller), and does replacing a leaver need approval? | Approval steps | Staff & HR dictionary; Employee DD D01–D15; HB p.138, p.210–213 | staff & HR dictionary, OBJ-30 | Henry, Country Directors, Finance Controllers | Open |
| OQ-175 | Does GIIS run demo lessons, reference checks or panels for teacher hiring, and from how many interviewers? | Candidate stages; feedback | Staff & HR dictionary; Employee DD D01–D15; HB p.138, p.210–213 | staff & HR dictionary, OBJ-30 | Henry, GIIS SG HR | Open |
| OQ-176 | How long may unsuccessful candidates' data be kept per country, and may they be contacted for future roles? | Retention; consent | Staff & HR dictionary; Employee DD D01–D15; HB p.138, p.210–213 | staff & HR dictionary, OBJ-30 | Henry, DPOs | Open |
| OQ-177 | When does the appraisal cycle run for teachers and for non-teaching staff, and who is a teacher's usual appraisal reviewer (HOD, coordinator, Vice Principal)? | Cycle dates; reviewer | Staff & HR dictionary; Employee DD D01–D15; HB p.138, p.210–213 | staff & HR dictionary, OBJ-31 | Henry, GIIS SG HR and school leadership | Open |
| OQ-178 | What rating scale and criteria does each brand use, and are lesson observations part of appraisal? | Rating field; observation notes | Staff & HR dictionary; Employee DD D01–D15; HB p.138, p.210–213 | staff & HR dictionary, OBJ-31 | Henry, HR heads per brand | Open |
| OQ-179 | Is there a CPD hours target per year, and which trainings are mandatory (IB category workshops, child protection, first aid)? | Training targets; compliance links | Staff & HR dictionary; Employee DD D01–D15; HB p.138, p.210–213 | staff & HR dictionary, OBJ-31 | Henry, GIIS SG academic heads | Open |
| OQ-180 | Do training requests need a budget or cost approval, and who holds the training budget? | Training approval step | Staff & HR dictionary; Employee DD D01–D15; HB p.138, p.210–213 | staff & HR dictionary, OBJ-31 | Henry, Finance Controllers | Open |
| OQ-181 | What happens today when a staff member leaves mid-year: notice periods, handover owner, exit interview? | Leaving checklist; notice | Staff & HR dictionary; Employee DD D01–D15; HB p.138, p.210–213 | staff & HR dictionary, OBJ-06 | Henry, GIIS SG HR | Open |
| OQ-182 | Who is the line manager for teachers at GIIS (level coordinator, HOD, Vice Principal), and for office staff? | Leave and appraisal routing | Staff & HR dictionary; Employee DD D01–D15; HB p.138, p.210–213 | staff & HR dictionary, OBJ-06, OBJ-28 | Henry, GIIS SG school leadership | Open |
| OQ-183 | Are there staff positions shared across campuses (one HR team for SG-PGL and SG-ECS), and which department structure does each campus use? | E03 scope; directory | Staff & HR dictionary; Employee DD D01–D15; HB p.138, p.210–213 | staff & HR dictionary, OBJ-05 | Henry, GIIS SG HR | Open |
| OQ-184 | Are staff concerns that aren't child protection (performance, grievances) handled in the same restricted case, or separately? | Case types | Staff & HR dictionary; Employee DD D01–D15; HB p.138, p.210–213 | staff & HR dictionary, OBJ-10 | Henry, GIIS SG HR and school leadership | Open |
| OQ-185 | How long are staff records kept after leaving, per country, and what is deleted or anonymised? | Retention (with OQ-43) | Staff & HR dictionary; Employee DD D01–D15; HB p.138, p.210–213 | staff & HR dictionary, OBJ-05, OBJ-06 | Henry, DPOs | Open |
| OQ-186 | Do staff use relief or casual teachers, and are they employees in Campus OS? | Casual employment; cover (with OQ-164) | Staff & HR dictionary; Employee DD D01–D15; HB p.138, p.210–213 | staff & HR dictionary, OBJ-06 | Henry, GIIS SG school leadership | Open |
| OQ-187 | Do staff children get admission priority or fee benefits that HR must confirm (DL-61 waitlist "staff children")? | Link to Admissions and Fees | Staff & HR dictionary; Employee DD D01–D15; HB p.138, p.210–213 | staff & HR dictionary, OBJ-16, OBJ-23 | Henry, Admissions owners, Finance Controllers | Open |
| OQ-188 | Which staff self-service changes need HR approval (bank details are out of scope), and does HR need to see an audit of them? | Self-service rules | Staff & HR dictionary; Employee DD D01–D15; HB p.138, p.210–213 | staff & HR dictionary, OBJ-05 | Henry, HR heads per brand-country | Open |
| OQ-189 | Do OWIS and other brands use different leave, appraisal or compliance processes from GIIS? | Brand variation; OWIS shows "Not set" | Staff & HR dictionary; Employee DD D01–D15; HB p.138, p.210–213 | staff & HR dictionary, OBJ-28, OBJ-29, OBJ-31 | Henry, OWIS leadership | Open |
| OQ-190 | Is the Designated Person for Child Protection always the Principal at every campus, and who is the backup? | STAFF_CONCERN case access | Staff & HR dictionary; Employee DD D01–D15; HB p.138, p.210–213 | staff & HR dictionary, OBJ-10 | Henry, GIIS school leadership | Open |
| OQ-191 | Do Indian campuses need statutory HR registers (attendance register for staff, PF, gratuity) inside Campus OS? | India scope; payroll boundary | Staff & HR dictionary; Employee DD D01–D15; HB p.138, p.210–213 | staff & HR dictionary, OBJ-06 | Henry, GIIS India HR | Open |
| OQ-192 | Which access does each position's standard template give at a campus (for example Teacher, Head of Department, HR Business Partner, Campus Admin), and who maintains the templates? | Campus-move access (DL-149, SR-HR-03) |
| OQ-193 | Who in GSG maintains the country leave templates and statutory minimums (group HR, legal, each country lead), and how is a law change checked? | Leave templates (DL-153, SR-HR-19) |

## V · Admin Platform

Added 28 Nov 2026 with the [admin platform dictionary](../03-domain/admin-platform-data-dictionary.md). Section U is used by the cross-module consistency review.

| ID | Question | Why it matters for design | Evidence | Affects | Who can answer | Status |
|---|---|---|---|---|---|---|
| OQ-194 | Which identity provider does each brand use for staff (GIIS: Google Workspace), does any brand run several tenants, and should Campus OS sync users from the provider or only authenticate? | Sign-in setup (PA05), account creation | Admin Platform brief (GIIS Google Workspace); OQ-27 | admin-platform dictionary, OBJ-32 | Brand IT leads | **Partly answered** (Henry, 28 Nov 2026): one GSG-wide provider → DL-169; rest → OQ-210 |
| OQ-195 | When a person works for two brands and has two work emails, which one is primary for notifications and the directory? | A02 `is_primary`, directory display | DL-42; GIIS SMART and OWIS Digital share Punggol (OQ-52) | A02, OBJ-01 | Henry, group HR | Open |
| OQ-196 | Who signs off a campus or brand-country go-live: brand-country head, Country Director, brand head or GSG operations? | PA02 approver, CAMPUS_GO_LIVE case | OQ-58 (who creates campuses) | PA02, OBJ-32 | Henry, GSG group office | **Answered** (Henry, 28 Nov 2026): brand-country head signs off → DL-167 |
| OQ-197 | Which modules does each brand go live with first, and in what order and dates? | Module switch-on samples and setup plans | Arch § Roadmap phases; OQ-05, OQ-32 | PA01, OBJ-32 | Henry, GSG leadership | Open |
| OQ-198 | Who acts as Brand Admin at each brand (brand IT lead, operations head), and may a brand ever create its own access roles or templates? | Brand Admin role (DL-178), template requests (DL-172) | OQ-16; role-catalogue "Brand leadership" | A06, permission catalogue | Henry, brand heads | Open |
| OQ-199 | How long are audit events kept per country, and may a redacted copy be kept longer in the global index? | Audit retention rule (PA08 AUDIT_EVENT) | Arch § Security; OQ-43 | A14, PA08 | Counsel per country, GSG IT security | Open |
| OQ-200 | What is the initial purpose catalogue, who owns it (group DPO, counsel, platform team), and which purposes need consent? | Purpose picker (AP-10), PA07 | DL-44; OQ-09, OQ-10 | PA07, OBJ-34 | Group DPO, counsel | Open |
| OQ-201 | Does each brand-country have a named data protection contact, and are they GSG staff, school staff or external? | Data Protection Lead role (DL-176), notice contact line | OQ-34; role-catalogue "Data protection: group and country DPOs" (no role defined) | A06, PA09 | Henry, group legal | Open |
| OQ-202 | Is a privacy notice issued per brand-country, per operating entity or per campus, and in which languages? | PA09 key, notice screens | OQ-02, OQ-06, OQ-25 | PA09, OBJ-34 | Counsel, brand marketing | Open |
| OQ-203 | Which integrations need service accounts (payroll feed, ERP journals, MyGIIS sync, Google Workspace), and who approves and rotates their keys? | Service accounts (A01 SERVICE), authority register | DL-77, DL-147; Arch § Integrations; OQ-12 | A01, PA04 | GSG IT security, brand IT | Open |
| OQ-204 | After how many days without sign-in is an account Dormant, and should dormant accounts ever be suspended automatically? | Dormant flag, access review flags | ia-screen-map SCR-GA-03 ("inactive 60 d"); S5 "no login > 60 days" | A01, PA10 | Henry, GSG IT security | Open |
| OQ-205 | Who approves a Tier 3 type grant or an operator session when the Campus Leader is the grantee, is away, or the node is above campus? | A17 and A18 approver routing | DL-69 "Watch"; DL-52 | A17, A18, OBJ-35 | Henry | Open |
| OQ-206 | Who attests access held at Brand-country, Brand and Group nodes (people with no campus), and how often? | Access review coverage beyond campuses | S5 covers campuses only | PA10, OBJ-36 | Henry, GSG IT security | Open |
| OQ-207 | Who in GSG may open break-glass, may it ever show Tier 3, and who besides the Campus Leader is told? | A18 BREAK_GLASS rules | DL-52; OQ-15; PowerSchool support-credential breach (research §08) | A18, OBJ-35 | GSG IT security, counsel | Open |
| OQ-208 | Do vendors or implementation partners (not GSG staff) ever need operator sessions, and on what contract? | Operator session eligibility | OQ-12 sub-processors; DL-52 | A18 | GSG IT, procurement | Open |
| OQ-209 | How will parents and students sign in (email or phone one-time code, MyGIIS migration, brand identity provider)? | Parent and Student apps (later, DL-118); account model must not block it | DL-42, DL-118; OQ-28 | A01, A02, PA05 | Henry, GIIS IT | Open |
| OQ-210 | Which product is the GSG-wide staff identity provider, how and when does each brand's staff move onto it (GIIS from Google Workspace), and does Campus OS sync users from it or only authenticate? | PA05 setup, go-live task SIGN_IN_CONNECTED, brand IT effort | DL-169; OQ-194, OQ-27 | admin-platform dictionary, OBJ-32 | GSG IT, brand IT leads | Open |

## Research not yet done

These are evidence gaps, not product questions. They are listed in [research §11](../../research/2026-10-25-campus-os-foundation-research.md#11--open-questions-and-research-gaps): contextual inquiry with staff, family research, vendor verification, legal review and a systems inventory. No users have been interviewed yet, so every persona and scenario is an assumption.

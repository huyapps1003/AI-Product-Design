# Glossary

**Status:** Living, 25 Oct 2026 · **Owner:** Henry
**Cites:** Architecture v1 · Employee Data Dictionary v1 · research §07, §09 · DL-17…DL-21
**Tóm tắt (VI):** Thuật ngữ dùng thống nhất trong product truth và trong màn hình. AI tạo màn hình phải dùng đúng các tên này.

Use these terms exactly in screens and specs. Where a term's definition is still open, the entry says so.

## Organisation

| Term | Meaning | Status |
|---|---|---|
| **GSG** (Global Schools Group) | The group that owns the school brands and runs Campus OS | Known (DL-22) |
| **Platform operator** | The party that runs Campus OS: GSG. Which GSG legal entity is open (OQ-01) | Known / Open |
| **Brand** | A school brand in the group, for example GIIS or OWIS. Has its own identity, curricula and policies across countries. A level in the scope tree below Group | Confirmed (DL-40) |
| **Brand-country** | A brand in one country, for example GIIS Singapore. Holds the regulator pack, currency and holidays; its country selects the data cell | Confirmed (DL-40) |
| **Person** | One record per human, with Student, Guardian and Employee profiles and one login | Confirmed (DL-42) |
| **Design object** | Something users see and act on (Student, Enrolment, Case), described by an object card `OBJ-nn` | Confirmed (DL-46) |
| **Country** | A tenancy level that owns data location, regulator pack, currency and tax, public holidays and languages offered | Known (Architecture) |
| **Campus** | A school site, identified by a campus code such as `SG-PGL` (catalog entity E06) | Known |
| **Segment** | A school segment of a campus such as Pre-Primary, Primary, Middle, Higher. Architecture calls this level "Section"; don't use that word in screens | Confirmed (DL-29) |
| **Class** | A group of students taught together, such as 7B | Confirmed (DL-29) |
| **Class plan** | How many classes a grade runs in a year and each class's size limit; set by the Principal; its total is the Admissions seat limit | Assumed (DL-71; seat rule Confirmed DL-61) |
| **Placement class** | A class such as AEPP 1 that takes students from several grades, who keep their grade enrolment | Assumed (DL-74) |
| **Class move** | Moving a student to another class of the same campus, programme and year; not a transfer | Assumed (DL-72) |
| **Rollover** | The year-end run that moves a campus's students into next year's grades and classes (promotion, retention, stage-campus promotion) | Assumed (DL-75); promotion vs transfer Confirmed (DL-54) |
| **House** | A student team that runs across years, such as House B | Known (HB p.169); list per campus Open (OQ-20) |
| **Student ID** | The number people use for a student; unique across Campus OS and kept on campus transfer | Confirmed (DL-31) |
| **Programme offering** | A curriculum taught on a campus for a grade band, for example CBSE 6–8 at Punggol | Known (Architecture) |
| **Programme pack** | A curriculum loaded as data: levels, subjects, grade scale, rules, templates | Known (Architecture) |
| **Country cell** | A deployment of the platform that holds one country's data in that country's region | Known (Architecture) |
| **Data location** | Product term for a country cell in staff and parent UI ("Data location: Singapore"); "cell" only in engineering docs (OR06) | Confirmed (DL-47) |
| **School operating entity** | The legal entity behind a school that enrols students and employs staff. Linked to each campus by date; copied onto enrolments, employments and cases (DL-41) | Structure Confirmed (DL-41); which ones is open (OQ-02) |
| **Employer** | The legal employer on an employment record (`employer_org_id` on E02; catalog TBD) | Known (Data Dictionary) |
| **Franchise / JV partner** | A local partner through which some GIIS campuses opened | Known that they exist (OQ-03) |

## Access

| Term | Meaning | Status |
|---|---|---|
| **GA grant** (Global Admin grant) | The ceiling of what a user may access: modules × campuses (A03, A04) | Known (Data Dictionary) |
| **Scope node** | A node of the scope tree: Group, Brand, Brand-country, Campus or Segment. Grants and memberships sit at Group, Brand, Brand-country or Campus (OR01) | Confirmed (DL-40); record Assumed (DL-48) |
| **TM membership** (Team Management) | A user's place in one module on one campus, with a role, a permission template and a data scope (A05–A13) | Known (Data Dictionary) |
| **Access role** | A named bundle used in TM, such as `TEACHER` or `MODULE_ADMIN` (A06) | Assumed values ([role catalogue](../04-access/role-catalogue.md)) |
| **Permission template** | A versioned list of allowed and denied capabilities (A08, A09) | Known (Data Dictionary) |
| **Capability** | One action code, such as `hr.employee.view` (R02) | Known (Data Dictionary) |
| **Data scope** | Which records a membership covers (grade, section, subject…) (A12, A13). No scope means no data (BR-17) | Known |
| **Family link** | The link between a parent account and a student enrolment that authorises parent access | Assumed (DL-18) |
| **Record ACL** | A per-record access list for restricted cases and Tier 3 fields | Assumed (DL-19) |
| **Delegation / acting grant** | Time-boxed transfer of capabilities from one user to another | Assumed (DL-21) |
| **Persona** | A design concept: a typical bundle of grants, memberships and scope. Never stored | Assumed (DL-02) |
| **HR position**, **campus affiliation**, **academic role** | Facts about employment and teaching. None of them grants access (BR-06) | Known |

## Data protection

| Term | Meaning | Status |
|---|---|---|
| **Organisation** / **data intermediary** | Singapore PDPA: the party that decides why and how data is processed / the party that processes it on its behalf | Known (secondary legal source) |
| **Data fiduciary** / **data processor** | India DPDP Act: the same two roles | Known (secondary legal source) |
| **Data controller** | Used in product truth as the umbrella term for the deciding party (organisation or data fiduciary). Which party is controller for which data is open (OQ-06) | Known term / Open assignment |
| **Data subject** | Students, parents, staff | Known |
| **Sub-processor** | A vendor processing data for the platform, such as the cloud host, Google Workspace, payment gateways | Known that they exist (OQ-12) |
| **Tier 1 / 2 / 3** | Internal / Confidential / Restricted sensitivity | Assumed mapping (DL-11) |
| **Verifiable parental consent** | DPDP requirement for under-18s, using reliable ID or DigiLocker | Known (secondary legal source) |
| **APAAR ID** | CBSE learner ID required for board exams; schools may record "refused" or "NOGEN" | Known |

## Not to be confused

- **Decision D01–D15** (Data Dictionary open decisions) ≠ **entity D01–D03** (academic entities) ≠ **DL-01…** (decision log).
- **Group** (the tenancy level, GSG) ≠ **group function** (a central team such as Group HR).
- **Employer** ≠ **data controller**. Product truth records the first, not the second.

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
| **Class plan** | How many classes a grade runs in a year and each class's size limit; set by the Principal; its total is the Admissions seat limit | Confirmed (DL-71; seat rule Confirmed DL-61) |
| **Class move** | Moving a student to another class of the same campus, programme and year; not a transfer | Confirmed (DL-72) |
| **Billing account** | What one payer (a parent or a sponsor) settles in one brand-country; siblings they pay for share one statement | Confirmed (DL-79) |
| **Fee plan** | A campus's fees for one programme offering, grade(s) and academic year; approved by the Finance Controller and versioned, never edited | Confirmed (DL-78) |
| **Fee status** | CLEAR, DUE, OVERDUE or PAUSED per enrolment; the only finance fact other modules read | Confirmed (DL-83) |
| **Assessment scheme** | A brand-country's rules for one programme and grade band in a year: components and weights, grade scale, promotion rule; copied from a GSG template and approved per version | Confirmed (DL-87) |
| **Mark sheet** | The marks for one assessment and one class; entered by the teacher, moderated and locked by the coordinator | Confirmed (DL-88) |
| **Report card** | A student's released results for one reporting period (term or semester); never edited, reissued if corrected | Confirmed (DL-90) |
| **Appeal** | A parent's request to re-check a released mark; one round only | Confirmed (DL-91) |
| **Academic year** | The dates of one school year for one curriculum at one campus; a campus running several curricula has several (GIIS SG CBSE April–March, IB DP July–May) | Confirmed (DL-96) |
| **Rollover** | The year-end run, per campus and curriculum, that moves students into next year's grades and classes (promotion, retention, stage-campus promotion) | Confirmed (DL-75); promotion vs transfer Confirmed (DL-54) |
| **School day** | A weekday inside a curriculum's term dates at a campus that isn't a holiday, break or closure; the base of every attendance % | Confirmed (DL-98) |
| **Register** | One class's attendance for one school day, taken by the class teacher; locks at end of day | Assumed (DL-99, DL-100) |
| **Leave request** | A parent's request for days off school; 1–2 days the class teacher approves, more the Principal | Confirmed (DL-101) |
| **Out pass** | Permission for a student to leave school early, recorded at reception | Known (HB p.23) |
| **Attendance threshold** | The minimum attendance % a student must keep; teachers see the number, not why it applies | Confirmed (DL-30) |
| **Case** | Anything tracked with an owner and a deadline: complaint, refund, leave over 2 days, withdrawal, transfer, appeal, ICA review, access request. The owning module keeps the detail | Known (Arch § Data model); model Confirmed (DL-120) |
| **Working day** | A day that counts toward a case deadline: Monday to Friday minus public holidays; not the same as a school day | Confirmed (DL-122); Open (OQ-130) |
| **Complaint**, **Compliment**, **Suggestion** | The three kinds of feedback (HB p.249). Only a complaint opens a case | Known (HB p.249) |
| **Restricted case** | A case only the people named on it can see; others don't see it in any list | Confirmed (DL-19); Confirmed (DL-125) |
| **Conversation** | Messages between staff and the guardians of one student | Confirmed (DL-127) |
| **Announcement** | A notice sent to an audience (campus, grade, class); may ask families to confirm they've read it, which is not consent | Confirmed (DL-128) |
| **Teaching group** | The students taught one subject together: a whole class ("7B Mathematics") or a group across classes ("Grade 7 Hindi (L2)") | Confirmed (DL-132) |
| **Teaching assignment** | Which teacher teaches a teaching group; one lead teacher per group. Gives the teacher their students | Confirmed (DL-134) |
| **Bell schedule** | The periods, breaks and assembly of a school day for part of a campus | Confirmed (DL-137); times Open (OQ-147) |
| **Timetable** | The weekly pattern of lessons. On screens a published version is "Live"; each change is a new version | Confirmed (DL-138) |
| **Clash** | A timetable problem. "Must fix" (teacher, room or student in two places) blocks publishing; "Check" (load, room type) needs a note | Confirmed (DL-139) |
| **Cover** | Another teacher taking or supervising a lesson when its teacher is away; the cover teacher sees the students for that day only | Confirmed (DL-143, DL-145) |
| **Line manager** | The person a staff member reports to; approves their leave and sees their compliance status. Not an access role | Confirmed (DL-150) |
| **Appraisal reviewer** | The person who runs a staff member's appraisal; the line manager unless HR sets someone else, such as the Head of Department | Confirmed (DL-150) |
| **Away** | A staff member is not at work. Colleagues and Teaching see only "Away", never the leave type or reason | Confirmed (DL-156) |
| **Compliance status** | One chip per staff member from their checks, passes and qualifications: Cleared, Due soon, Action needed, Not cleared | Confirmed (DL-158) |
| **Vacancy** | An approved post to hire for; candidates apply to a vacancy | Confirmed (DL-159) |
| **Leaving** | A staff member with an end date set; screens show "Leaves {date}" and a handover list | Confirmed (DL-160) |
| **House** | A student team that runs across years, such as House B | Known (HB p.169); list per campus Open (OQ-20) |
| **Student ID** | The number people use for a student; unique across Campus OS and kept on campus transfer | Confirmed (DL-31) |
| **Programme** (data: programme offering) | What a campus runs: a curriculum for a grade range, for example CBSE 1–12 at Punggol or IB Diploma at East Coast. Statuses: Opening soon, Open, No new students, Closed. Use "Programme" on screens; "programme offering" only in data and specs | Confirmed (DL-112, DL-117) |
| **Curriculum** (data: programme pack) | A curriculum held as data: grade levels, grade bands, subjects. GSG keeps a template per board; each brand-country adapts it into its own version ("CBSE, GIIS Singapore version 2026.1"). Grade scales and rules are in Assessment. Use "Curriculum" on screens | Confirmed (DL-110, DL-117) |
| **Grade level** | One level in a pack (Class 6, IB PYP 2, Kindergarten 1), with the level that normally comes next | Known (HB p.47–62); model Confirmed (DL-115); labels Open (OQ-127) |
| **Grade band** | A named group of grade levels, such as CBSE 6–8, shared by assessment schemes, coordinators and calendars | Confirmed (DL-115) |
| **Year template** | A brand-country's default year and term dates for one curriculum, which campuses copy | Confirmed (DL-96) |
| **Term** | An academic slice of a year (Semester 1, Term 1). Not a fee billing period, even where both are called "Term" | Confirmed (DL-114) |
| **Offered subject** | A subject a grade takes at an offering in one year, alone or as a choice (Language 2: Hindi, Tamil or French) | Confirmed (DL-116) |
| **Country cell** | A deployment of the platform that holds one country's data in that country's region | Known (Architecture) |
| **Data location** | Product term for a country cell in staff and parent UI ("Data location: Singapore"); "cell" only in engineering docs (OR06) | Confirmed (DL-47) |
| **School operating entity** | The legal entity behind a school that enrols students and employs staff. Linked to each campus by date; copied onto enrolments, employments and cases (DL-41) | Structure Confirmed (DL-41); which ones is open (OQ-02) |
| **Employer** | The legal employer on an employment record (`employer_org_id` on E02; catalog TBD) | Known (Data Dictionary) |
| **Franchise / JV partner** | A local partner through which some GIIS campuses opened | Known that they exist (OQ-03) |

## Access

| Term | Meaning | Status |
|---|---|---|
| **GA grant** (Global Admin grant) | The ceiling of what a user may access: modules × campuses (A03, A04) | Known (Data Dictionary) |
| **Scope node** | A node of the scope tree: Group, Brand, Brand-country, Campus or Segment. Grants and memberships sit at Group, Brand, Brand-country or Campus (OR01) | Confirmed (DL-40); record Confirmed (DL-48) |
| **TM membership** (Team Management) | A user's place in one module on one campus, with a role, a permission template and a data scope (A05–A13) | Known (Data Dictionary) |
| **Access role** | A named bundle used in TM, such as `TEACHER` or `MODULE_ADMIN` (A06) | Assumed values ([role catalogue](../04-access/role-catalogue.md)) |
| **Permission template** | A versioned list of allowed and denied capabilities (A08, A09) | Known (Data Dictionary) |
| **Capability** | One action code, such as `hr.employee.view` (R02) | Known (Data Dictionary) |
| **Data scope** | Which records a membership covers (grade, section, subject…) (A12, A13). No scope means no data (BR-17) | Known |
| **Family link** | The link between a parent account and a student enrolment that authorises parent access | Confirmed (DL-18) |
| **Record ACL** | A per-record access list for restricted cases and Tier 3 fields | Confirmed (DL-19) |
| **Delegation / acting grant** | Time-boxed transfer of capabilities from one user to another | Assumed (DL-21) |
| **Persona** | A design concept: a typical bundle of grants, memberships and scope. Never stored | Confirmed (DL-02) |
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
- **Leave request** (a parent asks for a student's days off, AT06, DL-101) ≠ **staff leave request** (an employee asks for leave, HR06, DL-153…DL-155). Screens say "Student leave" and "Staff leave" wherever both can appear (cross-module review, 28 Nov 2026).
- **Offer** (admissions offer to a family, AD07) ≠ **job offer** (recruitment offer to a candidate, HR14).
- **School day** (AT01 calendar, counts attendance) ≠ **working day** (CC09 calendar, counts case deadlines) ≠ **staff work pattern** (HR01, counts leave days).
- **Term** (academic, AC08) ≠ **billing period** (fees, FB04), even where both are called "Term" (DL-114).

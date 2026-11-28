# Global Campus OS — Academic Structure Data Dictionary v0.1 (draft)

**Status:** Proposed draft, 22 Nov 2026. DL-109…DL-117 all Confirmed (Henry, 22 Nov 2026); OQ-122…OQ-129 Open · **Owner:** Henry
**Cites:** [Architecture v1](../00-sources/Global_Campus_OS_Product_Architecture_v1.md) § Principles 2 (programme packs) and 4 (campus calendar engine), § Context (GIIS calendars), § Multi-campus and tenancy model ("Programme offering … subjects, assessment calendar, grading scale version, promotion rule version"), § Data model (Academic structure: Academic year, Term, Programme pack, Grade level, Subject, Section (class), Enrolment), § Programme pack as data, § Open questions Q7 · [Organisation dictionary](./organisation-data-dictionary.md) OR03, OR04, OR05, OR09 · [Student Directory Spec](./student-directory-data-dictionary.md) ST02, ST03 · [Admissions](./admissions-data-dictionary.md) AD02, AD03 · [Classes](./classes-data-dictionary.md) CL01, CL02, CL06 · [Fees & Billing](./fees-billing-data-dictionary.md) FB02, FB04 · [Assessment](./assessment-data-dictionary.md) AS01, AS02, AS06 · [Attendance](./attendance-data-dictionary.md) AT01, AT08 · GIIS Student Handbook 2025-26 (HB) p.47–62 (Master Course/Module List), p.68, p.87, p.109 · GIIS SG Holiday Schedule 2026-27 · GIIS SG Exam Schedule 2026-27 · DL-22, DL-27, DL-29, DL-43, DL-53, DL-54, DL-59, DL-62, DL-65, DL-74, DL-75, DL-87, DL-96 · MC-06, MC-07, MC-14 · OQ-20, OQ-24, OQ-51, OQ-77, OQ-78, OQ-107
**Tóm tắt (VI):** Từ điển dữ liệu cho cấu trúc học thuật dùng chung: gói chương trình (CBSE, IB, Cambridge, GMP) do GSG giữ mẫu và từng brand-country điều chỉnh, các khối lớp và thứ tự lên lớp, nhóm khối, môn học của gói, chương trình mở tại từng cơ sở, mẫu năm học của brand-country, năm học và học kỳ theo cơ sở × chương trình, và danh sách môn được dạy theo khối trong một năm. Mọi module khác tham chiếu tới đây thay vì tự định nghĩa.

> Drafted by Claude on 22 Nov 2026 after Henry asked to start on the Academic structure dictionary (repo review, same day) and chose pack ownership on a decision card (DL-110). It mostly gathers decisions already made in other modules (DL-96 above all) into one place, so that the 22 fields across seven dictionaries that point at "Academic year", "Offering", "Grade", "Subject" or "Calendar" point at a defined record.

## How to read this file

Same layout and rules as the [Student Directory Spec](./student-directory-data-dictionary.md#how-to-read-this-file): **Known** (cited), **Assumed** (DL), **Open** (OQ); tiers T1/T2/T3 (DL-11; everything here is Tier 1 reference data, no personal data); `Y`/`N`/`C` required. Every row also carries `tenant_path` and the shared common fields. Global records (packs) have no data location; campus records (offerings, years) are copied to the campus's data location (DL-47) so every module reads them locally. Facts from the GIIS Handbook are **GIIS examples**, not group rules (DL-22). All enums are unapproved candidates (DL-27 rule). Example values are GIIS facts or synthetic, as labelled.

## Scope

Scope proposed 22 Nov 2026; pack ownership chosen by Henry the same day (DL-110). **What curricula a campus teaches, which grades and subjects they contain, and when each year and term runs.**

| In this dictionary | Not in this dictionary (owner) |
|---|---|
| Programme pack per board (CBSE, IB PYP, IB DP, Cambridge CLSP, IGCSE, Global Montessori Plus): GSG template, brand-country version (DL-110) | Grading scales, components, promotion rules, report card templates (Assessment AS01–AS05, DL-87) |
| Grade levels of a pack, their order and the next level (feeds rollover) | Classes, class plans, rollover runs (Classes CL01–CL07) |
| Grade bands (CBSE 6–8, IGCSE 9–10) used by schemes, coordinator scope and calendars | Coordinator and teacher scope itself (Access Registry, A12/A13) |
| Pack subjects: catalogue per pack with type and level (HL/SL) | Subject-teacher allocation, teaching groups, streams ([Teaching & Timetable](./teaching-timetable-data-dictionary.md): OBJ-12; DL-132) |
| Programme offering: campus × pack × grade range | Segments (Organisation OR05, DL-29) |
| Year template per brand-country × pack (DL-96 default) | Holidays, closures, events, PTMs, exam windows (Calendar engine, later; holidays stay in Attendance AT02 until then) |
| Academic year per campus × offering, its status and dates | Fee billing periods (Fees FB04; they sit inside the year but are not academic terms) |
| Terms inside a year (semesters, terms) | Timetable periods ([Teaching & Timetable](./teaching-timetable-data-dictionary.md) TE02) |
| Offered subjects per offering × grade × year, with choice groups (L2, L3, electives) | A student's own subject choices (Student Records ST03, which points here) |

## Modeling boundaries

- **Screen terms (DL-117, Confirmed 22 Nov 2026).** Screens say **Curriculum** for a programme pack and **Programme** for a programme offering; offering statuses read Opening soon (PLANNED), Open (ACTIVE), No new students (PHASING_OUT), Closed (CLOSED). This file keeps the data names.

- **Packs are GSG templates that each brand-country adapts (DL-110, Confirmed 22 Nov 2026).** The GSG platform team keeps one pack per board: grade levels, grade bands and the subject catalogue. Each brand-country adapts it into its own versioned pack (adds local subjects such as Tamil, drops what it never teaches, renames grade labels) and campuses choose offerings from that version. This is the same pattern as assessment scheme templates (DL-87), so a scheme always refers to a pack version from the same board. It answers OQ-20 for packs; policy templates and other settings stay open there.
- **A pack version is never edited after it is published (DL-111).** A change (new subject, renamed grade label) is a new version, valid from an academic year. Past years keep the version they started with, the same way "2025-26 report cards keep using 2025-26 rules" (Arch § Programme pack as data).
- **An offering is a curriculum on a campus for a grade range (DL-112).** GIIS SG Punggol runs CBSE 1–12, Cambridge CLSP 6–8, IB PYP and GMP (KG); East Coast runs CBSE, IGCSE 9–10 and IB DP. The grade range can be the whole pack or part of it. AEPP is never an offering (DL-65) and is a SUPPORT subject (DL-74).
- **Years belong to the offering (DL-96, Confirmed 15 Nov 2026).** Academic year and term dates are set per campus × offering. The brand-country keeps a year template per pack (AC05); a campus copies it into its year and may adjust. GIIS SG 2026-27: CBSE, CLSP and IGCSE 6 Apr 2026 – 19 Mar 2027; IB DP 13 Jul 2026 – May 2027.
- **A year moves through four states and one is current (DL-113).** PLANNING (next year being set up: class plans, fee plans, schemes), PUBLISHED (dates fixed, other modules can build on it), CURRENT (the year today falls in), CLOSED (after rollover; read-only). One offering has at most one CURRENT year; next year can be PLANNING or PUBLISHED at the same time. Changing dates after PUBLISHED needs a reason, shows the impact first, is approved by the brand-country academic head (Confirmed 22 Nov 2026), and tells every module that keyed on them (school days, billing periods, assessments).
- **A term is an academic slice of a year (DL-114).** Terms (GIIS: Semester 1 and Semester 2; Term 1 and Term 2 in CBSE 1–8 assessment) lie inside the year's dates, don't overlap and are listed in order. Assessment, attendance thresholds per term or semester (DL-108) and report cards read them. Fee billing periods (three 4-month blocks at GIIS SG, Arch § Context) are a Fees concept (FB04) and are not terms, even where the screen says "Term 2".
- **Grade levels know what comes next (DL-115).** Each level has an order and a `next_grade_level_id` in the same pack or another pack (CBSE 10 → CBSE 11; CLSP 8 → IGCSE 9; IGCSE 10 → IB DP 1 or CBSE 11). Rollover (DL-75) proposes the next level; if the campus has no offering for it, the stage-campus promotion rule applies (DL-62). Display labels ("Class 6", "Grade 6", "Year 7", "Standard 7") are a brand-country choice on the pack version (OQ-127).
- **Offered subjects are per offering, grade and year (DL-116).** The brand-country's pack lists every subject; the campus marks which ones run for a grade in a year and groups the choices (Language 2: Hindi, Tamil or French; IB DP Group 1–6). A student's choice (ST03) must be one of the year's offered subjects. Teaching groups, sets and streams are Teaching's (OQ-77).
- **Nothing here is personal data.** All records are Tier 1 reference data. Students, staff and fees point at these records; these records never point at people.

## Entities

| ID | Entity | Module | Status |
|---|---|---|---|
| AC01 | Programme Pack | Academic structure (GSG template, brand-country version) | Known (Arch § Principles 2, § Programme pack as data); ownership Confirmed (DL-110) |
| AC02 | Grade Level | Academic structure | Known (Arch: "grade levels" in a pack; HB p.47–62 course list) |
| AC03 | Pack Subject | Academic structure | Known (Arch: "subjects" in a pack; HB p.47–62) |
| AC04 | Grade Band | Academic structure | Known use (Arch § Key workflows "assessment calendar for a programme and grade band"; AS02 grade band; HB p.68 grade bands) ; model Confirmed (DL-115) |
| AC05 | Year Template | Academic structure (brand-country) | Confirmed (DL-96: "the brand-country keeps a default per curriculum") |
| AC06 | Programme Offering | Academic structure (campus) | Known (Arch § Multi-campus and tenancy model, MC-07); model Confirmed (DL-112) |
| AC07 | Academic Year | Academic structure (campus × offering) | Confirmed key (DL-96); lifecycle Confirmed (DL-113) |
| AC08 | Term | Academic structure | Known (Arch § Data model "Term"; HB p.45, p.68, p.109); model Confirmed (DL-114) |
| AC09 | Offered Subject | Academic structure (campus × offering × grade × year) | Known need (Arch: offering holds "subjects"; ST03 "subject choices for a year"); model Confirmed (DL-116) |

ST02, ST03, AD02, CL01, FB02, FB04, AS02, AS06, AT01 and AT08 are referenced, not redefined. Their `FK:Academic year`, `FK:Offering`, `FK:Programme offering`, `FK:Grade` and `FK:Subject` columns now point at AC07, AC06, AC02 and AC03 (see [Field references fixed](#field-references-fixed-in-other-dictionaries)).

## Detailed attribute dictionary

### AC01 — Programme Pack

**Purpose:** One curriculum held as data. A GSG template per board, and a versioned brand-country adaptation of it.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| pack_id | UUID | PK | Y | T1 | Known entity (Arch § Data model "Programme pack") | One row per template version or brand-country version. |
| pack_code | VARCHAR(24) | UK with owner + version | Y | T1 | Assumed | CBSE, IBPYP, IBDP, CLSP, IGCSE, GMP. Group catalogue beyond these: OQ-51. |
| pack_name | VARCHAR(120) | — | Y | T1 | Known (HB p.47–62 course titles) | e.g. "Central Board of Secondary Education (CBSE)", "International Baccalaureate Diploma Programme". |
| pack_level | ENUM | — | Y | T1 | Confirmed (DL-110) | GSG_TEMPLATE or BRAND_COUNTRY. |
| owner_scope_node_id | UUID | FK:OR01 | Y | T1 | Confirmed (DL-110) | GROUP for templates, OR03 brand-country for adaptations. |
| template_pack_id | UUID | FK:AC01 | C | T1 | Confirmed (DL-110) | Required for BRAND_COUNTRY: the template version it adapts. |
| awarding_body | VARCHAR(120) | — | Y | T1 | Known (HB p.47–62 "Awarded by") | e.g. "Cambridge Assessment International Education"; "Global Indian International School (Affiliated to CBSE)". |
| version_label | VARCHAR(24) | — | Y | T1 | Confirmed (DL-111) | e.g. "2026.1". |
| valid_from_year_label | VARCHAR(16) | — | Y | T1 | Confirmed (DL-111) | First academic year it applies to, e.g. "2026-27". |
| pack_status | ENUM | — | Y | T1 | Confirmed (DL-111) | DRAFT, PUBLISHED, RETIRED. Published versions are read-only. |
| approved_by | UUID | FK:A01 | C | T1 | Open (OQ-122) | Who approves a brand-country version. |
| grade_label_style | VARCHAR(24) | — | N | T1 | Confirmed (DL-115); values Open (OQ-127) | How grades are shown for this brand-country: "Class 6", "Grade 6", "Year 7". |

### AC02 — Grade Level

**Purpose:** One level inside a pack, in order, with the level a student normally moves to next.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| grade_level_id | UUID | PK | Y | T1 | Known entity (Arch "Grade level") | Referenced by AC03, AC04, AC06, AC09, AD02, AD03, AD05, AD06, AD07, AS06, CL01, CL02, CL07, FB02 (ST02 stores the `grade_level_code`, not this key). |
| pack_id | UUID | FK:AC01 | Y | T1 | Known | |
| grade_code | VARCHAR(16) | UK with pack | Y | T1 | Assumed | e.g. CBSE-06, IBDP-1, GMP-K1. |
| display_label | VARCHAR(40) | — | Y | T1 | Known values (HB p.47–62: "Nursery", "Kindergarten 1", "Class 6", "IB PYP – 2", "IB Diploma Programme Year 1") | Brand-country version may rename (OQ-127). |
| sequence | INT | UK with pack | Y | T1 | Confirmed (DL-115) | Order inside the pack. |
| typical_age | INT | — | N | T1 | Open (OQ-128) | Entry age per country; Admissions age check reads it. |
| next_grade_level_id | UUID | FK:AC02 | N | T1 | Confirmed (DL-115) | Same or another pack (CLSP 8 → IGCSE 9). Empty for a final level (CBSE 12, IB DP 2). Several routes: OQ-125. |
| is_final | BOOLEAN | — | Y | T1 | Assumed | Final level: rollover proposes GRADUATING (CL07). |
| variant_of_grade_level_id | UUID | FK:AC02 | N | T1 | Open (OQ-123) | GMP "Extended" courses (HB p.48–49): a variant of the same level, or a separate level. |

### AC03 — Pack Subject

**Purpose:** A subject the pack can offer. Campuses pick from this list for each grade and year (AC09).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| pack_subject_id | UUID | PK | Y | T1 | Known entity (Arch "Subject") | Referenced by AC09, AS06, AS09, AS13, ST03 and teaching allocation (OBJ-12). |
| pack_id | UUID | FK:AC01 | Y | T1 | Known | |
| subject_code | VARCHAR(24) | UK with pack | Y | T1 | Assumed | e.g. CBSE-SCI, IBDP-BIO-HL. Board codes where they exist (OQ-124). |
| subject_name | VARCHAR(120) | — | Y | T1 | Known (HB p.47–62) | e.g. "Social Science", "Mathematics Analysis and Approaches". |
| subject_type | ENUM | — | Y | T1 | Confirmed values (ST03 `choice_type`, DL-27) | CORE, SECOND_LANGUAGE, THIRD_LANGUAGE, ELECTIVE, SUPPORT. Same list as ST03 so a choice always matches its subject. |
| level | ENUM | — | N | T1 | Known (HB p.60–62: "Higher Level", "Standard Level") | HL, SL, CORE_COMPONENT (TOK, EE, CAS). IB DP only for now. |
| subject_group | VARCHAR(40) | — | N | T1 | Known need (HB p.49–56 "Language 2 – Hindi, Tamil, French"); Assumed | e.g. "Language 2", "IB Group 4 Sciences". Choice rules: AC09. |
| grade_level_ids | UUID[] | FK:AC02 | Y | T1 | Known (HB lists subjects per class) | Levels it can be taught in. |
| requires_regulator_approval | BOOLEAN | — | N | T1 | Known (HB p.54: Applied Mathematics "offered post approval from regulatory authorities") | When true, AC09 needs an approval reference (OQ-126). |
| added_by_brand_country | BOOLEAN | — | Y | T1 | Confirmed (DL-110) | A local addition, not in the GSG template. |

### AC04 — Grade Band

**Purpose:** A named group of grades inside a pack that schemes, coordinators and calendars share, such as "CBSE 6–8".

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| grade_band_id | UUID | PK | Y | T1 | Confirmed (DL-115) | Referenced by AS01 and AS02 schemes (`grade_band_id`), TE07 academic responsibilities, coordinator data scope (A13) and, later, exam windows (Calendar). |
| pack_id | UUID | FK:AC01 | Y | T1 | Known | Brand-country version. |
| band_label | VARCHAR(40) | — | Y | T1 | Known (HB p.68 "Grades 6-8"; Exam Schedule 2026-27 per band; sample "CBSE Grades 6-8") | |
| grade_level_ids | UUID[] | FK:AC02 | Y | T1 | Assumed | Contiguous levels of the same pack. A level can sit in more than one band set (assessment bands vs coordinator bands: OQ-129). |

### AC05 — Year Template

**Purpose:** The brand-country's default year and term dates for one pack and year label. Campuses copy it into their academic years (DL-96).

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| year_template_id | UUID | PK | Y | T1 | Confirmed (DL-96) | |
| brand_country_node_id | UUID | FK:OR03 | Y | T1 | Confirmed (DL-96) | |
| pack_id | UUID | FK:AC01 | Y | T1 | Confirmed (DL-96) | Brand-country version. |
| year_label | VARCHAR(16) | UK with node + pack | Y | T1 | Known style ("AY 2026-27" in samples; OWIS calendar year "AY 2026") | |
| start_date / end_date | DATE | — | Y | T1 | Known for GIIS SG (Holiday Schedule 2026-27: 6 Apr 2026 – 19 Mar 2027; IB DP from 13 Jul 2026, HB p.109) | |
| terms | JSON | — | Y | T1 | Known for GIIS SG (Semester 1 6 Apr – 18 Sep 2026; Semester 2 28 Sep 2026 – 19 Mar 2027) | Same shape as AC08. |
| template_status | ENUM | — | Y | T1 | Assumed | DRAFT, PUBLISHED. |

### AC06 — Programme Offering

**Purpose:** A curriculum taught on one campus for a grade range, such as CBSE 1–12 at Punggol or IB DP at East Coast.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| programme_offering_id | UUID | PK | Y | T1 | Known entity (Arch § Multi-campus "Programme offering") | Referenced by AC07, AD02, AD03, AD05, AD06, AD07, AS02, AT01, CL01, CL02, CL06, CL07, FB02, FB04, ST02, TE07, TE09. |
| campus_id | UUID | FK:OR04 | Y | T1 | Known | |
| pack_code | VARCHAR(24) | — | Y | T1 | Confirmed (DL-110) | The brand-country pack this offering follows; the version comes from each academic year (AC07 `pack_id`). |
| offering_label | VARCHAR(60) | UK with campus | Y | T1 | Known values (samples: "CBSE 1-12", "Cambridge CLSP 6-8", "IGCSE 9-10", "IB DP", "IB PYP", "GMP (KG)") | One spelling everywhere: "IB DP", not "IBDP". |
| first_grade_level_id / last_grade_level_id | UUID | FK:AC02 | Y | T1 | Confirmed (DL-112) | The grade range taught here. |
| segment_codes | VARCHAR(32)[] | — | N | T1 | Known values per campus (OR05, DL-29) | Pre-Primary, Primary, Middle, Higher at GIIS. Derived from grades where the campus maps them. |
| offering_status | ENUM | — | Y | T1 | Confirmed (DL-112) | PLANNED, ACTIVE, PHASING_OUT (no new admissions; current students continue), CLOSED. |
| accreditation_ref | VARCHAR(80) | — | N | T1 | Known need (CBSE affiliation, IB authorisation, Cambridge centre); Assumed | Shown on report cards and transfer certificates. Number formats: OQ-124. |
| opened_on / closed_on | DATE | — | C | T1 | Assumed | |
| settings_level | — | — | — | T1 | Confirmed level (DL-40: "Segment / Programme offering"); Confirmed (DL-112) | The offering is the settings level under the campus where subjects, scheme and rule versions are chosen. It is not an OR01 scope node (OR01 `node_type` has no OFFERING); staff scope by offering goes through membership data scope (A13). |

### AC07 — Academic Year

**Purpose:** The dates of one school year for one offering at one campus. Every year-based record in other modules points here.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| academic_year_id | UUID | PK | Y | T1 | Confirmed key (DL-96) | Referenced by AC08, AC09, AD02, AD03, AD06, AS02, AS06, AS12, AT01, AT08, CL01, CL02, CL06, FB02, FB04, HR16, ST02, TE01, TE04, TE07, TE09. |
| programme_offering_id | UUID | FK:AC06 | Y | T1 | Confirmed (DL-96) | Campus comes from the offering. |
| year_label | VARCHAR(16) | UK with offering | Y | T1 | Known style ("AY 2026-27") | |
| pack_id | UUID | FK:AC01 | Y | T1 | Confirmed (DL-111) | The pack version used all year. |
| year_template_id | UUID | FK:AC05 | N | T1 | Confirmed (DL-96) | Where the dates were copied from. |
| start_date / end_date | DATE | — | Y | T1 | Confirmed (DL-96); GIIS SG values Known | CBSE SG-PGL 2026-04-06 → 2027-03-19; IB DP SG-ECS 2026-07-13 → May 2027. PYP and KG: OQ-107. |
| differs_from_template | BOOLEAN | — | Y | T1 | Assumed; derived | Flags campus changes for the brand-country. |
| year_status | ENUM | — | Y | T1 | Confirmed (DL-113) | PLANNING, PUBLISHED, CURRENT, CLOSED. |
| date_change_reason | TEXT | — | C | T1 | Confirmed (DL-113) | Required for any date change after PUBLISHED; the brand-country academic head approves (DL-113). |
| previous_academic_year_id | UUID | FK:AC07 | N | T1 | Assumed | The same offering's year before; rollover (CL06) runs from one to the next. |

### AC08 — Term

**Purpose:** An academic slice of a year (semester or term) that assessments, report cards and attendance periods use.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| term_id | UUID | PK | Y | T1 | Known entity (Arch "Term") | Read by AS assessments and report cards, and by AT08 thresholds measured per term or semester (DL-108, through `measured_over`, not a key). |
| academic_year_id | UUID | FK:AC07 | Y | T1 | Confirmed (DL-96) | |
| term_label | VARCHAR(40) | — | Y | T1 | Known (HB p.45 "two semesters"; p.68 "Term 1 April – September") | "Semester 1", "Term 1". |
| term_kind | ENUM | — | Y | T1 | Confirmed (DL-114) | SEMESTER, TERM, TRIMESTER. A year uses one kind. |
| sequence | INT | UK with year | Y | T1 | Assumed | |
| start_date / end_date | DATE | — | Y | T1 | Known for GIIS SG 2026-27 (Holiday Schedule) | Inside the year; terms don't overlap; gaps between them are breaks. |

### AC09 — Offered Subject

**Purpose:** A subject a grade takes at one offering in one year, and how students choose between subjects.

| Attribute | Type | Key | Req. | Tier | Status | Validation / meaning |
|---|---|---|:---:|---|---|---|
| offered_subject_id | UUID | PK | Y | T1 | Confirmed (DL-116) | |
| academic_year_id | UUID | FK:AC07 | Y | T1 | Confirmed (DL-96) | Offering comes from the year. |
| grade_level_id | UUID | FK:AC02 | Y | T1 | Known (HB lists subjects per class) | |
| pack_subject_id | UUID | FK:AC03 | Y | T1 | Known | Must belong to the year's pack version. |
| choice_group | VARCHAR(40) | — | C | T1 | Known need (HB p.52: Class 6 Language 2 Hindi, Tamil or French; Language 3 Sanskrit, Hindi or Tamil) | Empty for subjects every student in the grade takes. |
| choose_min / choose_max | INT | — | C | T1 | Known need; GIIS rules Open (OQ-125) | Per choice group, e.g. Language 2: 1 / 1. IB DP 3 HL + 3 SL rules: OQ-125. |
| offered_status | ENUM | — | Y | T1 | Confirmed (DL-116) | OFFERED, PENDING_APPROVAL (regulator approval, HB p.54), WITHDRAWN. |
| approval_ref | VARCHAR(80) | — | C | T1 | Open (OQ-126) | Required when the pack subject needs regulator approval. |

## How other modules use this

| Module | Reads | Rule |
|---|---|---|
| Student Records (ST02, ST03) | AC07 year, AC06 offering, AC02 grade; ST03 subject from AC03, validated against AC09 | SR-AC-07 |
| Admissions (AD02, AD03) | Offerings that are ACTIVE and admitting; years PUBLISHED or PLANNING; grade ranges | SR-AC-04 |
| Classes (CL01, CL02, CL06) | Year per offering; next grade (AC02) for rollover; closes the year after rollover | SR-AC-05, DL-75 |
| Fees (FB02, FB04) | Fee plans per offering × grade × year; billing periods must lie inside AC07 dates (they are not AC08 terms) | SR-AC-06 |
| Assessment (AS02, AS06) | Scheme per pack version and grade band (AC04); assessments per subject (AC03) and term (AC08) | DL-87, DL-111 |
| Attendance (AT01, AT08) | AC08 term dates for school days (DL-98); term or semester periods for thresholds (DL-108) | DL-98 |

## Field visibility by role

`M` manage, `A` approve, `V` view, `—` none. All cells Assumed (DL-26 rule). Reference data is Tier 1 and visible to every signed-in staff member in scope; parents and students see labels only (their child's grade, year dates, term dates, subjects).

| Record | GSG platform team | Brand-country academic head | Principal (campus) | Academic coordinator | Teachers, office | Parent / student |
|---|---|---|---|---|---|---|
| GSG pack template | M | V | V | V | V | — |
| Brand-country pack version, grade bands, subjects | V | M, A (OQ-122) | V | V | V | own grade and subjects |
| Year template | V | M | V | V | V | — |
| Offering | V | A (new or closing offering, OQ-122) | M | V | V | own offering label |
| Academic year and terms | V | A (date changes after publishing, DL-113) | A (publish) | M (prepare) | V | year and term dates |
| Offered subjects and choice groups | V | V | A | M | V | own grade's choices |

## Enumerations (candidates, DL-27 rule)

| Entity | Field | Values |
|---|---|---|
| AC01 | pack_level | GSG_TEMPLATE, BRAND_COUNTRY |
| AC01 | pack_status | DRAFT, PUBLISHED, RETIRED |
| AC03 | subject_type | CORE, SECOND_LANGUAGE, THIRD_LANGUAGE, ELECTIVE, SUPPORT (same as ST03 `choice_type`) |
| AC03 | level | HL, SL, CORE_COMPONENT |
| AC05 | template_status | DRAFT, PUBLISHED |
| AC06 | offering_status | PLANNED, ACTIVE, PHASING_OUT, CLOSED |
| AC07 | year_status | PLANNING, PUBLISHED, CURRENT, CLOSED |
| AC08 | term_kind | SEMESTER, TERM, TRIMESTER |
| AC09 | offered_status | OFFERED, PENDING_APPROVAL, WITHDRAWN |

## Business rules

- **SR-AC-01 (Confirmed, DL-110):** A brand-country pack version adapts exactly one GSG template version of the same board. Subjects it adds are flagged `added_by_brand_country`; subjects it drops are simply absent.
- **SR-AC-02 (Confirmed, DL-111):** A PUBLISHED pack version is read-only. Changes create a new version valid from a named year; a year keeps the pack version it started with.
- **SR-AC-03 (Confirmed, DL-96):** An academic year belongs to one campus × offering. Its dates are copied from the brand-country year template and may be changed by the campus; the change is flagged to the brand-country.
- **SR-AC-04 (Confirmed, DL-112):** Admissions can only open applications for an ACTIVE offering, for grades inside its range, into a year that is PLANNING, PUBLISHED or CURRENT. A PHASING_OUT offering takes no new applications.
- **SR-AC-05 (Confirmed, DL-113):** One CURRENT year per offering. A year becomes CLOSED only after its rollover run is applied (CL06 APPLIED), and is then read-only.
- **SR-AC-06 (Confirmed, DL-114):** Terms lie inside the year, don't overlap, and are numbered in order. Fee billing periods (FB04) must lie inside the year's dates but are not terms.
- **SR-AC-07 (Confirmed, DL-116):** A student's subject choice (ST03) must be an OFFERED subject for their grade and year, and each choice group's min and max must be met before the enrolment can be ENROLLED (blocking or warning: OQ-125).
- **SR-AC-08 (Confirmed, DL-113):** Changing a PUBLISHED year's or term's dates needs a reason and the brand-country academic head's approval, shows its impact before approval, is audited, and raises AcademicYearDatesChanged so Attendance recalculates school days, Fees flags affected billing periods and Assessment flags assessments outside the new dates.
- **SR-AC-09 (Confirmed, DL-115):** Rollover proposes each student's next grade from `next_grade_level_id`. If the campus has no ACTIVE offering for that grade, the line becomes a stage-campus promotion candidate (DL-62) or LEAVING.
- **SR-AC-10 (Known, Arch § Principles 1):** No module hard-codes a year, term, grade label or subject. Screens show the campus's values or "Not set" (DL-59 pattern).

## Events

PackVersionPublished, OfferingOpened, OfferingStatusChanged, YearTemplatePublished, AcademicYearCreated, AcademicYearPublished, AcademicYearBecameCurrent, AcademicYearDatesChanged, AcademicYearClosed, OfferedSubjectsPublished. Events carry IDs only (architecture.md).

## Capabilities (new, DL-53 naming)

`academic.pack.manage_template` (GSG platform team) · `academic.pack.manage`, `.approve` (brand-country) · `academic.year_template.manage` · `academic.offering.manage`, `.approve` · `academic.year.manage`, `.publish`, `.change_dates` · `academic.subjects.manage`, `.approve`. `academic.pack.view`: everyone with a membership can read academic structure in their scope (Tier 1).

## Field references fixed in other dictionaries

Before this file, these columns pointed at records nothing defined. They now point here (same PR):

| Dictionary | Column | Was | Now |
|---|---|---|---|
| Student Directory ST02 | academic_year_id | FK:Calendar | FK:AC07 |
| Student Directory ST02 | programme_offering_id | FK:Programme offering | FK:AC06 |
| Student Directory ST03 | subject_id | FK:Programme pack subject | FK:AC03 (validated against AC09) |
| Admissions AD02 | interested_grade_id / interested_offering_id / interested_academic_year_id | FK:Grade / Programme offering / Academic year | FK:AC02 / AC06 / AC07 |
| Admissions AD03 | programme_offering_id, grade_id, academic_year_id | FK:Offering / Grade / Academic year | FK:AC06 / AC02 / AC07 |
| Classes CL01 | academic_year_id, programme_offering_id, grade_id | FK:Academic year / Offering / Grade | FK:AC07 / AC06 / AC02 |
| Classes CL06 | programme_offering_id | FK:Offering | FK:AC06 |
| Fees FB02 | programme_offering_id, academic_year_id | FK:Offering / Academic year | FK:AC06 / AC07 |
| Fees FB04 | programme_offering_id | FK:Offering, "set when the offering's year differs from the campus default" | FK:AC06, always set (years are per offering, DL-96) |
| Assessment AS02 | programme_offering_id, valid_from_academic_year_id | FK:Offering / Academic year | FK:AC06 / AC07 |
| Assessment AS06 | grade_id, subject_id | FK:Grade / Subject | FK:AC02 / AC03 |
| Attendance AT08 | valid_from_year_id | FK:Academic year | FK:AC07 |

Still pointing outside this file, on purpose: Assessment `exam_window_id` and Organisation `holiday_calendar_ref` (Calendar engine, later), Attendance `period_id` now points at [Teaching & Timetable](./teaching-timetable-data-dictionary.md) TE02.

## Decisions

| ID | Decision | Why |
|---|---|---|
| DL-109 (**Confirmed**, Henry, 22 Nov 2026) | Academic structure is its own dictionary in the Academic structure domain (DL-43) with capability prefix `academic`. It owns packs, grade levels, pack subjects, grade bands, year templates, offerings, years, terms and offered subjects; other modules reference it | 22 fields in seven dictionaries pointed at undefined records; Arch § Data model lists these as one group |
| DL-110 (**Confirmed**, Henry, 22 Nov 2026) | The GSG platform team keeps one programme pack template per board (grade levels, grade bands, subject catalogue). Each brand-country adapts it into its own versioned pack; campuses choose offerings from that version. Answers OQ-20 for packs | Same pattern as assessment scheme templates (DL-87); keeps group reporting comparable while GIIS SG can add Tamil and GIIS India can drop French |
| DL-111 (**Confirmed**, Henry, 22 Nov 2026) | Pack versions are immutable once published and valid from a named year; a year keeps its pack version | Arch § Programme pack as data ("2025-26 report cards keep using 2025-26 rules") |
| DL-112 (**Confirmed**, Henry, 22 Nov 2026) | A programme offering is campus × brand-country pack × grade range, with status PLANNED, ACTIVE, PHASING_OUT, CLOSED; one label spelling everywhere | Arch § Multi-campus; GIIS SG campuses run several curricula; PHASING_OUT lets a campus stop admitting without ending current students |
| DL-113 (**Confirmed**, Henry, 22 Nov 2026) | Academic year lifecycle PLANNING → PUBLISHED → CURRENT → CLOSED; one CURRENT per offering; closes after rollover; date changes after publishing need a reason and notify dependent modules | Next year's class plans, fee plans and schemes must be built before it starts; school days and billing depend on the dates |
| DL-114 (**Confirmed**, Henry, 22 Nov 2026) | Terms are academic slices inside a year, in order, without overlap; fee billing periods are a Fees concept and not terms | GIIS SG has two semesters (HB p.45) but bills in three 4-month blocks (Arch § Context "term billing in 4-month blocks"); one word was used for both |
| DL-115 (**Confirmed**, Henry, 22 Nov 2026) | Grade levels are ordered per pack with a next level (possibly in another pack); grade bands are named groups of levels per pack; display labels are a brand-country choice | Rollover needs the next grade (DL-75); schemes and coordinators work by band (HB p.68, AS02) |
| DL-116 (**Confirmed**, Henry, 22 Nov 2026) | Offered subjects are set per offering × grade × year from the pack's subjects, with choice groups and min/max; ST03 choices must match them; teaching groups and streams stay with Teaching | HB p.47–62 lists subjects per class with language options; OQ-77 keeps streams open |
| DL-117 (**Confirmed**, Henry, 22 Nov 2026) | Screen terms: programme pack → "Curriculum", programme offering → "Programme"; statuses Opening soon, Open, No new students, Closed; data names unchanged | Henry asked for friendlier terms than "programme offering"; "Course" avoided because the handbook uses it per grade |

## Open questions (new)

| ID | Question | Why it matters for design |
|---|---|---|
| OQ-122 | Who approves a brand-country pack version, and a new or closing programme offering at a campus (brand-country academic head, Academic Supervisor, Country Director)? | Approver on pack and offering screens |
| OQ-123 | Are the GMP "Extended" courses (Nursery, K1, K2 Extended, HB p.48–49) longer-day variants of the same grade, separate grades, or a different offering? | Grade list, class names ("K1 A Extnd", HB p.9–14) and fee plans |
| OQ-124 | Which board codes and accreditation numbers (CBSE affiliation and subject codes, IB school code, Cambridge centre number) must Campus OS hold, and where are they printed? | Report cards, transfer certificates, board registrations |
| OQ-125 | What subject-choice rules apply per grade (Language 2 and 3 combinations, CBSE 11 combinations, IB DP 3 HL + 3 SL plus core), are they blocking or a warning, and are there several next-grade routes after Class 8 and 10 (CLSP → IGCSE or CBSE; IGCSE → IB DP or CBSE 11)? | Subject choice screens; rollover next grade |
| OQ-126 | Who records regulator approval for a subject (HB p.54 Applied Mathematics)? (Date changes answered by DL-113: brand-country academic head.) | Pending-approval subjects |
| OQ-127 | What grade labels does each brand-country use (Class, Grade, Year, Standard; Nursery vs Pre-K), and are they the same in Singapore and India? | Every screen that shows a grade |
| OQ-128 | What entry ages per grade apply per country (cut-off dates), and should Admissions check them? | Admissions age check (AD02) |
| OQ-129 | Are coordinator bands the same as assessment bands (e.g. CBSE 6–8 for both), or do campuses group grades differently for people and for exams? | One band list or two |

Still open from earlier and relevant here: OQ-24 (group views across different year dates), OQ-51 (group curriculum catalogue), OQ-77 (streams), OQ-78 (mixed-age classes), OQ-107 (PYP, KG and Noida staggered starts).

## Readiness

- **Can be designed now:** pack and version browser (GSG template vs brand-country version, subjects added locally), offering list per campus, academic year setup copied from the brand-country template with differences highlighted, term editor, year status timeline (planning next year while this one is current), offered subjects per grade with choice groups, year-date change with impact preview (school days, billing periods, assessments).
- **Keep generic, label Pending:** approvers (OQ-122; regulator approval OQ-126), GMP Extended (OQ-123), board codes (OQ-124), choice rules and routes (OQ-125), grade labels (OQ-127), entry ages (OQ-128), PYP/KG start dates (OQ-107).

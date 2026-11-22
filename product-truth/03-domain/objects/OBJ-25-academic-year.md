---
id: OBJ-25
name: Academic year and programme offering
entities: [AC01, AC02, AC03, AC04, AC05, AC06, AC07, AC08, AC09]
owner: academic            # Academic structure domain (DL-43); prefix academic (DL-109)
home: global+cell          # packs and year templates global; offerings, years, terms, offered subjects copied to the campus data location (DL-47)
tenant_path: group/brand/brand-country/campus
operating_entity: none     # reference data; enrolments hold operating_entity_id (DL-41)
effective_dated: true      # pack versions valid from a year (DL-111); years per campus × offering (DL-96)
states:                    # year_status, DL-27 rule; colours proposed for review
  PLANNING: info
  PUBLISHED: default
  CURRENT: success
  CLOSED: default
# Offering offering_status: PLANNED info, ACTIVE success, PHASING_OUT warning, CLOSED default
# Screen terms (DL-117): pack = "Curriculum", offering = "Programme"; PLANNED "Opening soon", ACTIVE "Open", PHASING_OUT "No new students", CLOSED "Closed"
# Pack pack_status: DRAFT info, PUBLISHED success, RETIRED default
# Offered subject offered_status: OFFERED success, PENDING_APPROVAL warning, WITHDRAWN default
field_groups:
  - name: Programme pack (GSG template)
    tier: T1
    capability: academic.pack.view               # every signed-in staff member
    edit: [academic.pack.manage_template]        # GSG platform team (DL-110)
  - name: Brand-country pack, grade levels, grade bands, subjects
    tier: T1
    capability: academic.pack.view
    edit: [academic.pack.manage]
    approve: [academic.pack.approve]             # who: OQ-122
  - name: Year template
    tier: T1
    capability: academic.pack.view
    edit: [academic.year_template.manage]
  - name: Programme offering
    tier: T1
    capability: academic.pack.view
    edit: [academic.offering.manage]
    approve: [academic.offering.approve]         # new or closing offering, OQ-122
  - name: Academic year and terms
    tier: T1
    capability: academic.pack.view               # parents see own child's year and term dates
    edit: [academic.year.manage]
    approve: [academic.year.publish, academic.year.change_dates]   # date changes after publish, OQ-126
  - name: Offered subjects and choice groups
    tier: T1
    capability: academic.pack.view
    edit: [academic.subjects.manage]
    approve: [academic.subjects.approve]
actions:
  - academic.pack.manage_template
  - academic.pack.manage
  - academic.pack.approve
  - academic.year_template.manage
  - academic.offering.manage
  - academic.offering.approve
  - academic.year.manage
  - academic.year.publish
  - academic.year.change_dates
  - academic.subjects.manage
  - academic.subjects.approve
events: [PackVersionPublished, OfferingOpened, OfferingStatusChanged, YearTemplatePublished, AcademicYearCreated, AcademicYearPublished, AcademicYearBecameCurrent, AcademicYearDatesChanged, AcademicYearClosed, OfferedSubjectsPublished]
variation: [MC-06, MC-07, MC-14]
open: [OQ-24, OQ-51, OQ-77, OQ-107, OQ-122, OQ-123, OQ-124, OQ-125, OQ-126, OQ-127, OQ-128, OQ-129]
sample: product-truth/03-domain/samples/academic-structure-sample.json
# Read by Student Records, Admissions, Classes, Fees, Assessment and Attendance. Never hard-code a year, term, grade label or subject (SR-AC-10).
patterns: [AP-01, AP-02, AP-04, AP-05]
---

# OBJ-25 · Academic year and programme offering

**Status:** Proposed with the academic structure dictionary v0.1, 22 Nov 2026 (DL-109…DL-112, DL-114…DL-117 Confirmed 22 Nov 2026; DL-113 Assumed) · **Owner:** Henry
**Cites:** [academic structure dictionary](../academic-structure-data-dictionary.md) · Arch § Principles 2 and 4, § Multi-campus and tenancy model, § Data model (Academic structure) · HB p.47–62, p.68, p.109 · DL-87, DL-96, DL-110

The GSG platform team keeps one pack per board; each brand-country adapts it into a versioned pack (DL-110), and each campus runs offerings from that version. Years and terms belong to the campus × offering (DL-96): GIIS SG CBSE runs April–March while IB DP at East Coast runs July–May. A year is PLANNING, PUBLISHED, CURRENT or CLOSED; next year is set up while this one is current (DL-113). Screens show the campus's own labels and dates or "Not set", never a hard-coded value. Pending values: PYP and KG start dates (OQ-107), grade labels (OQ-127), choice rules (OQ-125).

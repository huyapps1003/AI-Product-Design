# GSG data landscape (as-is)

**Status:** Known (public campus list, 9 Nov 2026) · Open where marked · **Owner:** Henry
**Cites:** [globalschools.com/campus](https://globalschools.com/campus/) (all 62 cards loaded 9 Nov 2026) · [globalindianschool.org](https://globalindianschool.org/) · [owis.org](https://owis.org/in/about/) · [operating-context](./operating-context.md) · [Architecture v1 § Integrations](../00-sources/Global_Campus_OS_Product_Architecture_v1.md#integrations) · DL-39…DL-46 · OQ-01…OQ-03, OQ-07, OQ-21, OQ-27…OQ-29, OQ-39, OQ-43, OQ-50…OQ-56
**Tóm tắt (VI):** Bức tranh hiện trạng của GSG mà kiến trúc dữ liệu cần: 62 campus, 12 thương hiệu, 10 quốc gia, 21 nút brand-country và 10 vùng dữ liệu (cell); hệ thống nguồn; sổ thẩm quyền dữ liệu GIIS; bốn loại chuyển campus; câu hỏi gửi GSG. Không có quyết định nào trong file này.

The target architecture is settled (DL-39…DL-46). This file is the current picture it sits on: where GSG's campuses are, which systems hold their data today, and what a campus transfer would cross. Machine copy: [`gsg-footprint.json`](./gsg-footprint.json). Working doc with comments: [GSG data landscape](https://claude.ai/artifact/GKADEyd6za31PrUBCqfG2e).

## How the AI design workflow uses this

| Section | Feeds (DL-46) | Becomes | What a screen does with it |
|---|---|---|---|
| 1 Footprint | Layer 1 | OBJ-13 Scope node and campus, OBJ-14 Legal entity (cards pending the Organisation dictionary); sample scope nodes | Scope breadcrumb and picker show real brands and campuses (AP-01); entity stays the placeholder (AP-09, OQ-02) |
| 2 Campus findings | Layer 0 | OQ-50…OQ-56 | Dependent elements carry a Pending marker (AR-13, AP-13) |
| 3 Source systems | Layer 0 | Source names for AR-12 | Fields owned elsewhere show a source chip naming the system (AP-04) |
| 4 Authority register | Layer 1 | `source` and read-only flags on cards | Read-only fields and conflict queue (AP-04, AP-12) |
| 5 Transfer matrix | Layer 2 | Campus transfer slice | What moves and who approved it (AP-11); types B–D Pending |

## 1. Footprint

GSG's public campus list shows **62 campuses, 12 brands, 10 countries and 21 brand-country nodes** (DL-40). The site also claims 64 campuses in 11 countries; see gaps. Operating entities are Open for every campus (OQ-02); campus codes don't exist yet (OQ-55).

| Brand | Campuses | Brand-country nodes (campuses) |
|---|---|---|
| GIIS | 16 | India 8 · Japan 4 · UAE 2 · Singapore 1 · Malaysia 1 |
| OWIS | 9 | Singapore 3 · India 2 · Japan 2 · Saudi Arabia 2 |
| Witty International School | 9 | India 9 (4 schools, 5 Witty Kids preschools) |
| Harrods International Academy | 7 | Cambodia 7 |
| Vikaasa | 6 | India 6 |
| Glendale International School | 4 | India 3 · UAE 1 |
| Heath House | 4 | UK 3 · Singapore 1 |
| Regent International School | 3 | Malaysia 3 |
| Dwight School Seoul | 1 | South Korea 1 |
| Emirates American School | 1 | UAE 1 |
| Chinese International School Manila | 1 | Philippines 1 |
| Domuschola International School | 1 | Philippines 1 |

| Country (candidate data region, OQ-21) | Campuses | Brands |
|---|---|---|
| India | 28 | GIIS, OWIS, Witty, Vikaasa, Glendale |
| Cambodia | 7 | Harrods |
| Japan | 6 | GIIS, OWIS |
| Singapore | 5 | GIIS, OWIS, Heath House |
| Malaysia | 4 | GIIS, Regent |
| UAE | 4 | GIIS, Glendale, Emirates American School |
| UK | 3 | Heath House |
| Saudi Arabia | 2 | OWIS |
| Philippines | 2 | CISM, Domuschola |
| South Korea | 1 | Dwight |

Six countries hold more than one brand, so one data region serves several brands; the Brand-country level (DL-40) carries that.

**Gaps (probably explain 62 vs 64 and 10 vs 11):** GIIS East Coast (Siglap, Singapore) is named on the GIIS site and used as `SG-ECS` in sample data but isn't listed; GIIS Bangkok (Thailand) and OWIS Suntec (Singapore) are named on brand sites but not listed; "Heath House International School" (UK) may duplicate the two London campuses.

### All 62 listed campuses

| Brand | Campus | City | Country | Flags |
|---|---|---|---|---|
| GIIS | SMART Campus | Singapore | Singapore | shared_area_other_brand:OQ-52 |
| GIIS | Bannerghatta | Bangalore | India | — |
| GIIS | Whitefield | Bangalore | India | shared_area_other_brand:OQ-52 |
| GIIS | Hadapsar | Pune | India | — |
| GIIS | Balewadi | Pune | India | — |
| GIIS | Charholi | Pune | India | — |
| GIIS | Ahmedabad | Ahmedabad | India | — |
| GIIS | Noida | Noida | India | — |
| GIIS | Nagpur | Nagpur | India | — |
| GIIS | Higashi Kasai | Tokyo | Japan | — |
| GIIS | Nishi Kasai | Tokyo | Japan | — |
| GIIS | Seisincho | Tokyo | Japan | — |
| GIIS | Kita Kasai | Tokyo | Japan | — |
| GIIS | Dubai | Dubai | UAE | — |
| GIIS | Abu Dhabi | Abu Dhabi | UAE | — |
| GIIS | Kuala Lumpur | Kuala Lumpur | Malaysia | — |
| OWIS | Digital Campus | Singapore | Singapore | shared_area_other_brand:OQ-52 |
| OWIS | Nanyang | Singapore | Singapore | — |
| OWIS | Newton | Singapore | Singapore | — |
| OWIS | Whitefield | Bangalore | India | shared_area_other_brand:OQ-52 |
| OWIS | Sarjapur | Bangalore | India | — |
| OWIS | Osaka | Osaka | Japan | — |
| OWIS | Tsukuba | Tsukuba | Japan | — |
| OWIS | Al Muruj | Riyadh | Saudi Arabia | — |
| OWIS | Al Jazeera | Riyadh | Saudi Arabia | — |
| WIT | WIS Malad | Mumbai | India | — |
| WIT | WIS Borivali | Mumbai | India | — |
| WIT | WIS Bhilwara | Bhilwara | India | — |
| WIT | WIS Udaipur | Udaipur | India | — |
| WIT | Witty Kids Goregaon | Mumbai | India | preschool_only |
| WIT | Witty Kids Borivali | Mumbai | India | preschool_only |
| WIT | Witty Kids R C Vyas | Bhilwara | India | preschool_only |
| WIT | Witty Kids Shastri Nagar | Bhilwara | India | preschool_only |
| WIT | Witty Kids Udaipur | Udaipur | India | preschool_only |
| HIS | Early Years | Phnom Penh | Cambodia | stage_campus:OQ-50 |
| HIS | Lower Primary | Phnom Penh | Cambodia | stage_campus:OQ-50 |
| HIS | Upper Primary | Phnom Penh | Cambodia | stage_campus:OQ-50 |
| HIS | Lower Secondary | Phnom Penh | Cambodia | stage_campus:OQ-50 |
| HIS | Upper Secondary | Phnom Penh | Cambodia | stage_campus:OQ-50 |
| HIS | Olympic | Phnom Penh | Cambodia | — |
| HIS | Toul Kork | Phnom Penh | Cambodia | — |
| VIK | Vikaasa World School | Madurai | India | — |
| VIK | Vikaasa Public School | Madurai | India | — |
| VIK | Vikaasa Progressive School | Madurai | India | — |
| VIK | Vikaasa Heritage Campus | Madurai | India | — |
| VIK | Vikaasa Jubilee School | Madurai | India | — |
| VIK | Montessori Campus | Not stated | India | preschool_only |
| GLE | Sun City | Hyderabad | India | — |
| GLE | Tellapur | Hyderabad | India | — |
| GLE | Financial District | Hyderabad | India | — |
| GLE | Dubai | Dubai | UAE | — |
| HH | Tranquil Hall | London | UK | — |
| HH | Wemyss Road | London | UK | — |
| HH | Heath House International School | Not stated | UK | — |
| HH | Heath House Campus | Singapore | Singapore | — |
| REG | Klang | Klang | Malaysia | adult_learners_possible:OQ-54 |
| REG | Sungei Petani | Sungei Petani | Malaysia | adult_learners_possible:OQ-54 |
| REG | Kuantan | Kuantan | Malaysia | adult_learners_possible:OQ-54 |
| DWI | Seoul | Seoul | South Korea | — |
| EAS | Al Qarrayen | Sharjah | UAE | — |
| CISM | Manila | Manila | Philippines | — |
| DOM | Manila | Manila | Philippines | — |

Flags: `stage_campus` OQ-50 · `shared_area_other_brand` OQ-52 · `adult_learners_possible` OQ-54 · `preschool_only`. Cities come from the card or the brand website.

### Sample scope nodes for screen prompts

Codes are placeholders until OQ-55 is answered.

| Code | Campus | Brand-country · data region | Variation it tests |
|---|---|---|---|
| SG-PGL | GIIS SMART Campus, Punggol | GIIS Singapore · Singapore | Current sample; multi-brand country |
| SG-OWD | OWIS Digital Campus, Punggol | OWIS Singapore · Singapore | Neighbouring campus of another brand (OQ-52); cross-brand transfer |
| IN-NOI | GIIS Noida | GIIS India · India | Second and largest data region |
| KH-HUP | Harrods Upper Primary, Phnom Penh | Harrods Cambodia · Cambodia | Stage campus (OQ-50); national curriculum |
| IN-WKB | Witty Kids Borivali, Mumbai | Witty India · India | Preschool-only campus feeding a school |
| UK-HTH | Heath House Tranquil Hall, London | Heath House UK · UK | Non-Asian regulator; British curriculum |
| MY-RKL | Regent Klang | Regent Malaysia · Malaysia | University pathway; possible adult learners (OQ-54) |

Records for these nodes are in [`../03-domain/samples/student-sample.json`](../03-domain/samples/student-sample.json), with `KH-HLP` (Harrods Lower Primary) added so a stage campus move can be shown. `SG-ECS` stays in the sample data until GSG confirms East Coast still operates.

## 2. What the campus list adds

| # | Finding | Evidence | Affects | OQ |
|---|---|---|---|---|
| 1 | One school can be split across stage campuses; promotion changes campus | Harrods Phnom Penh: Early Years, Lower Primary, Upper Primary, Lower Secondary, Upper Secondary | Transfer must not treat a stage move as a transfer | OQ-50 |
| 2 | 34 curriculum labels, the same programme named several ways | "A-levels" / "AS-A levels" / "Cambridge 'A'"; Khmer National Programme, British National Curriculum, American Common Core, Waldorf, Swinburne Foundation | Programme packs need one catalogue with aliases; Architecture v1 plans CBSE, IB, Cambridge only | OQ-51 |
| 3 | Campuses of two brands in the same area | GIIS SMART Campus and OWIS Digital Campus (Punggol); GIIS and OWIS Whitefield (Bangalore) | Shared sites, rooms, buses across brands | OQ-52 |
| 4 | Schools publish in Arabic, Japanese, Khmer, Korean | OWIS Saudi Arabia Arabic site; campuses in Japan, Cambodia, South Korea | Native-script names, notice language (OQ-25), UI beyond English v1 | OQ-53 |
| 5 | Some learners may be over 18 | Regent Swinburne Foundation Program | Guardian model, consent, DOB rules | OQ-54 |
| 6 | No campus codes; names repeat across brands | Two "Whitefield", "Manila", "Dubai" campuses | Campus ID and code standard; show "Brand · Campus" | OQ-55 |
| 7 | Brand names differ across GSG's own pages | "Harrods International School" vs "Academy"; "Chinese International School Manila" vs "Chinese Indian International School"; two Heath House entries | Legal and display name per brand | OQ-56 |
| 8 | Each brand runs its own website, some per country | 13 domains, e.g. owis.org/in, /sa, /sg, /jp | Likely separate enquiry and portal systems per brand | OQ-27, OQ-29 |

## 3. Source-system inventory

| Data domain | GIIS today | Other brands | Campus OS writer (DL-43) | Open |
|---|---|---|---|---|
| Student profile, guardians, addresses, passport and immigration | MyGIIS Profile Update (parent side); staff-side store unknown | Open | Student Records | OQ-28 |
| Enquiry and application | enquiry.globalschools.com | Open | Admissions | OQ-29 |
| Campus transfer request | MyGIIS only (handbook) | Open | Admissions with Engagement | OQ-28 |
| Exams, results, timetable, syllabus, worksheets | MyGIIS | Open | Assessment, Teaching | OQ-28 |
| Fees and payments | MyGIIS fees history; unnamed accounting or ERP | Open | Fees and finance | Architecture Q1 |
| Staff identity and mail | Google Workspace (@globalschools.com) | Open | Identity and Access Registry | — |
| Staff HR and payroll | Unknown | Open | Staff and HR | OQ-27 |
| Hiring | careers.globalschools.com | Open | Staff and HR | — |
| Learning platform | Google Classroom (assumed) | Open | Teaching | OQ-31 |
| Board and regulator IDs | CBSE, IB, Cambridge, ICA, SSG (file exports) | Open | Student Records, Engagement | — |

## 4. Authority register seed (GIIS, DL-45)

Architecture v1 plans a MyGIIS sync bridge in Phase 0 (Oct 2026–Mar 2027) and retirement in Phase 3 (AY 2028-29 onward). No field group has a cutover date.

| Field group | Authoritative now | Sync direction | Cutover | Conflict reviewer |
|---|---|---|---|---|
| Student name, Student ID, class, grade | MyGIIS (assumed) | MyGIIS → Campus OS | Open | Open |
| Addresses and contacts (parent-edited, DL-28) | MyGIIS | Two-way | Open | Open |
| Identity documents and immigration status | MyGIIS | Two-way | Open | Open |
| Guardians and relationships | MyGIIS (assumed) | MyGIIS → Campus OS | Open | Open |
| Enrolment history and transfers | MyGIIS (assumed) | MyGIIS → Campus OS | Open | Open |
| Results history | MyGIIS | MyGIIS → Campus OS | Open | Open |
| Fees history | MyGIIS and the ERP | Open | Open | Open |
| Staff login | Google Workspace | Two-way | Stays | — |

"Assumed" rows rest on MyGIIS being the only system known; a staff-side SIS, if one exists, is the authority instead (OQ-27, OQ-28).

## 5. Campus transfer matrix

Every transfer ends one enrolment and starts another, keeping the Student ID (DL-24, DL-31).

| Type | Example | What moves | Authorised by | Designable now | Blocking |
|---|---|---|---|---|---|
| A. Same entity, same data region | GIIS Punggol → East Coast | Nothing leaves the region; new enrolment, same profile | Both principals (handbook) | Yes, if both share an entity | OQ-02 |
| B. Different entity, same region | GIIS campus → partner-run campus in the same country | Profile stays; new entity gets a defined data set | Sharing record (DL-44) | Structure only | OQ-02, OQ-03, OQ-07 |
| C. Different country | GIIS Singapore → GIIS India | Profile created in target region; origin keeps history; Tier 3 never moves raw | Sharing record + cross-border rules | Structure only | OQ-07, OQ-21, OQ-39, OQ-43 |
| D. Different brand | GIIS SMART Campus → OWIS Digital Campus | As B or C, plus the target brand's policies and systems | Sharing record | No | OQ-07, OQ-27, OQ-56 |

A move between stage campuses of one school is a promotion, not a transfer, until OQ-50 is answered; the transfer flow must not offer it.

## 6. Questions for GSG

**Group office:** brand, brand-country and campus list with go-live order (OQ-33) · entity per campus and partner-run campuses (OQ-02, OQ-03) · platform operator entity (OQ-01) · do East Coast, Bangkok and Suntec belong in scope · stage campus moves (OQ-50) · shared sites (OQ-52) · brand names and campus codes (OQ-55, OQ-56) · learners over 18 (OQ-54).
**Counsel:** authority to move data across entities, countries and brands (OQ-07) · data residency per country (OQ-21) · retention after transfer (OQ-43).
**GIIS IT:** staff-side SIS, MyGIIS platform and API (OQ-28) · fees and HR systems · authority and conflict reviewer per field group.
**Each brand's IT:** SIS, portal, finance and HR systems and owners (OQ-27) · export formats · conflict reviewers.

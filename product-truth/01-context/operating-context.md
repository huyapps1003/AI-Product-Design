# Operating context: Global Schools Group

**Status:** Known (with confidence labels), 25 Oct 2026 · **Owner:** Henry
**Cites:** RES-F01, RES-F02 · research §02 · [Architecture v1 § How GIIS operates today](../00-sources/Global_Campus_OS_Product_Architecture_v1.md#how-giis-operates-today)
**Tóm tắt (VI):** Bức tranh tổ chức mà Campus OS phục vụ: tập đoàn, 12 thương hiệu, quốc gia, pháp nhân và hệ thống đang dùng. Mỗi dòng ghi rõ nguồn và mức tin cậy.

Use this file for realistic content and to understand which parties a screen may involve. Confidence labels follow the research: *internal primary*, *public claim*, *stated direction*.

## Footprint

| Figure | Value | Confidence |
|---|---|---|
| Brands / campuses / countries in Architecture v1 | 1 / 3 / 2 | Our own assumptions |
| GSG school brands | 12 | Public claim |
| GSG campuses / countries | 64 / 11 | Public claim (the site's country filter lists 10) |
| GIIS campuses / countries | 35 / 10 | Public claim (the GIIS page's own description says 23+ campuses in 7 countries) |

The figures don't agree with each other. Don't put any of them in a design as an exact count.

## Structure

| Level | What is known | Confidence |
|---|---|---|
| **Group and platform operator** | Global Schools Group, headquartered at the GIIS SMART Campus in Punggol, Singapore, runs Campus OS. Staff use @globalschools.com mailboxes. | Stated direction (Henry); public claim |
| **Brands** | GIIS, OWIS, Dwight School Seoul, Emirates American School, Glendale, Harrods International, Heath House, Chinese International School Manila, Domuschola, Regent, Vikaasa, Witty | Public claim |
| **Curricula across brands** | IB, Cambridge, CBSE, ICSE, American, British, Montessori, Waldorf and others | Public claim |
| **Growth** | GSG built GIIS campuses from 2002 to 2019. From 2021 it has mostly acquired or partnered with established schools, which keep their own identity. | Public claim |
| **Countries** | GIIS: Singapore, India, Malaysia, Japan, Thailand, UAE, Vietnam, Saudi Arabia, South Korea, Cambodia. Other brands add the Philippines and the UK. | Public claim |
| **Operating entities** | GIIS's Thailand and first India campuses opened "through local partners, via joint ventures or franchises". The Data Dictionary has one synthetic employer per country (GIIS Singapore, GIIS India). | Public claim; our own assumptions |

Not known: how many legal entities exist and which operates each campus (OQ-02), which campuses are partner-run today (OQ-03), which GSG entity runs the platform (OQ-01).

## Systems in use

| System | What is known | Open |
|---|---|---|
| **MyGIIS** (GIIS parent portal) | 11 features: calendar, exams and results, fees history, directory, messaging, PTM booking, profile update, bus requests, syllabus, timetable, worksheets. Transfer applications only go through MyGIIS. | What it runs on and whether it has an API (OQ-28) |
| **enquiry.globalschools.com** | Group admissions enquiries. The family's chosen school isn't carried to the next site. | Owner and data flow to schools (OQ-29) |
| **careers.globalschools.com** | Group hiring | — |
| **Google Workspace** | Staff SSO and mail (Architecture: also students) | — |
| **Other brands' portals and SIS** | Not researched | OQ-27 |

## GIIS as the detailed example

GIIS is the only brand with internal source material (Student Handbook 2025-26 Singapore, Exam and Holiday Schedules 2026-27, Noida Almanac 2025-26). Its facts (curricula, calendars, thresholds, SLAs) are in [Architecture v1 § How GIIS operates today](../00-sources/Global_Campus_OS_Product_Architecture_v1.md#how-giis-operates-today). Use them as GIIS examples; don't assume other brands work the same way.

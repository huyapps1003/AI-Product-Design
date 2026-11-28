# Design object registry (Layer 1)

**Status:** Confirmed structure (DL-43, DL-46, Henry, 7 Nov 2026); card values cite their sources · **Owner:** Henry
**Cites:** DL-42, DL-43, DL-46 · [architecture.md](../architecture.md) · [student dictionary](../student-directory-data-dictionary.md) · [organisation dictionary](../organisation-data-dictionary.md) · [Employee Data Dictionary v1](../../00-sources/Global_Campus_OS_Employee_Data_Dictionary_v1.md) · [rules-to-ui](../../06-experience/admin-platform/rules-to-ui.md)
**Tóm tắt (VI):** Danh mục "design object": những thứ người dùng nhìn thấy và thao tác. Mỗi object có một card (YAML) với entity, domain sở hữu, trạng thái và màu chip, nhóm trường theo tier, hành động và câu hỏi mở. 14 card đã có; các card còn lại chờ data dictionary.

A design object is something a user sees, finds and acts on. Each maps to dictionary entities and has exactly one owning domain (DL-43). The card's YAML front matter is what the AI design workflow reads; the dictionaries stay the engineering source.

## Registry

| ID | Object | Entities | Owner (writer) | Home | Top tier | Card |
|---|---|---|---|---|---|---|
| OBJ-01 | Person | Person, A01, A02 (defined in the [admin platform dictionary](../admin-platform-data-dictionary.md), DL-165) | Identity and Access Registry | Global | T1 | [card](./OBJ-01-person.md) |
| OBJ-02 | Student | ST01, ST06–ST10 | Student Records | Cell | T3 | [card](./OBJ-02-student.md) |
| OBJ-03 | Enrolment | ST02, ST03 | Admissions creates, Student Records holds | Cell | T2 | [card](./OBJ-03-enrolment.md) |
| OBJ-04 | Guardian | ST04, ST05, ST07 | Student Records | Cell | T2 | [card](./OBJ-04-guardian.md) |
| OBJ-05 | Employee | E01, E07, E08, HR02 (employee, campus assignment, external mapping, emergency contact; staff & HR dictionary, DL-146) | Staff and HR | Cell | T2 (T3 identity numbers) | [card](./OBJ-05-employee.md) |
| OBJ-06 | Employment | E02–E05, HR01, HR15 (employment, department, org assignment, position, contract term, checklist task) | Staff and HR | Cell | T2 | [card](./OBJ-06-employment.md) |
| OBJ-07 | Access grant | A03 module grant, A04 scope grant (admin platform dictionary) | Access Registry (Global Admin) | Global | T1 | [card](./OBJ-07-access-grant.md) |
| OBJ-08 | Team membership | A05, A07, A10, A12, A13 (admin platform dictionary) | Access Registry (Module Admin) | Cell | T1 | [card](./OBJ-08-team-membership.md) |
| OBJ-09 | Family access | F01, R03 | Access Registry (system) | Cell | T1 | [card](./OBJ-09-family-access.md) |
| OBJ-10 | Case | CC01–CC09 (case type, case policy, case, deadline, participant, decision, activity, feedback item, working-day calendar), A15 | Engagement (Cases & Communications, DL-119) | Cell | T3 when restricted | [card](./OBJ-10-case.md) |
| OBJ-11 | Audit event | A14 | Platform services | Cell, redacted global index | T3 snapshots | [card](./OBJ-11-audit-event.md) |
| OBJ-12 | Teaching allocation | TE04–TE07 (teaching group, group membership, teaching assignment, academic responsibility; replace Employee DD D01, D02) | Teaching (DL-131) | Cell | T1 | [card](./OBJ-12-teaching-allocation.md) |
| OBJ-13 | Scope node and campus | OR01–OR06, OR09, PA06 (setting definition) | Tenancy and config | Global | T1 | [card](./OBJ-13-scope-node.md) |
| OBJ-14 | Legal entity | OR07, OR08 | Tenancy and config | Global | T1 | [card](./OBJ-14-legal-entity.md) |
| OBJ-15 | Class | CL01–CL07 (class, class plan, class staff, placement, house, rollover); year, term, pack, offering, grade and subject are OBJ-25 | Academic structure (Classes, DL-70) | Cell | T2 (rollover outcomes, move notes) | [card](./OBJ-15-class.md) |
| OBJ-16 | Application | AD01–AD10 | Admissions | Cell | T3 (documents, SEN outcome) | [card](./OBJ-16-application.md) |
| OBJ-17 | Consent | Consent, purpose | Engagement | Cell | T2 | Needs the Governance dictionary |
| OBJ-18 | Transfer | Sharing record, two enrolments | Admissions with Engagement | Cell | T2 | Needs the Governance dictionary |
| OBJ-19 | Mark and report card | AS01–AS13 (scheme template, assessment scheme, component, grade scale, promotion rule, assessment, mark sheet, mark, subject result, report card, appeal, promotion decision, predicted grade) | Assessment (DL-87) | Cell | T2 | [card](./OBJ-19-report-card.md) |
| OBJ-20 | Attendance mark | AT01–AT10 (school day calendar, non-school day, register, mark, correction, leave request, gate event, rule, summary, alert) | Attendance (DL-97) | Cell | T2 (T3 certificates, ICA Case) | [card](./OBJ-20-attendance-mark.md) |
| OBJ-21…22, OBJ-24 | Incident, wellbeing record, route | Architecture § High-level data model | Their modules | Cell | T2 or T3 | Needs a dictionary per module |
| OBJ-23 | Billing account and invoice | FB01–FB17 (fee type, fee plan, billing period, billing account, responsibility, adjustment, billing run, invoice, payment, credit note, refund, reminder, billing pause, fee protection) | Fees and finance (DL-77) | Cell | T2 | [card](./OBJ-23-invoice.md) |
| OBJ-25 | Academic year and programme offering | AC01–AC09 (programme pack, grade level, pack subject, grade band, year template, programme offering, academic year, term, offered subject) | Academic structure (DL-109; packs DL-110) | Global (packs) and cell (offerings, years) | T1 | [card](./OBJ-25-academic-year.md) |
| OBJ-26 | Message and announcement | CC10–CC16 (conversation, message, announcement, receipt, notification template, notification, preference) | Engagement (Cases & Communications, DL-119) | Cell | T2 (messages) | [card](./OBJ-26-message-and-announcement.md) |
| OBJ-27 | Timetable and lesson | TE01–TE03, TE08–TE13 (bell schedule, bell period, room, period plan, timetable, timetable lesson, dated lesson, teacher absence, cover arrangement; replace Employee DD D03) | Teaching (DL-131) | Cell | T1 | [card](./OBJ-27-timetable-and-lesson.md) |
| OBJ-28 | Staff leave | HR03–HR07 (leave type, leave policy, leave balance, leave request, approval step) | Staff and HR (DL-146) | Cell | T2 (T3 medical certificates) | [card](./OBJ-28-staff-leave.md) |
| OBJ-29 | Compliance record | HR08–HR10 (compliance item type, requirement, record) | Staff and HR (DL-146) | Cell | T3 (work pass, checks) | [card](./OBJ-29-compliance-record.md) |
| OBJ-30 | Vacancy and candidate | HR11–HR14 (vacancy, candidate application, interview feedback, offer) | Staff and HR (DL-146) | Cell | T2 | [card](./OBJ-30-vacancy-and-candidate.md) |
| OBJ-31 | Appraisal and training | HR16–HR18 (appraisal cycle, appraisal, training record) | Staff and HR (DL-146) | Cell | T2 | [card](./OBJ-31-appraisal-and-training.md) |
| OBJ-32 | Campus setup and go-live | PA01–PA05 (module switch-on, setup run, setup task, legacy systems register, identity provider connection) | Tenancy and config (Admin Platform, DL-165) | Global | T1 | [card](./OBJ-32-campus-setup.md) |
| OBJ-33 | Access role and template | A06, A08–A10, R01–R03 | Identity and Access Registry (GSG platform team, DL-51) | Global | T1 | [card](./OBJ-33-access-role-and-template.md) |
| OBJ-34 | Data protection settings | PA07–PA09 (purpose, retention rule, privacy notice) | Identity and Access Registry (Admin Platform, DL-176) | Global | T1 | [card](./OBJ-34-data-protection-settings.md) |
| OBJ-35 | Special access | A16–A18 (delegation, Tier 3 type grant, operator session) | Identity and Access Registry | Cell | T1 (opens T3 for A17) | [card](./OBJ-35-special-access.md) |
| OBJ-36 | Access review | PA10, CC03 (review cycle, CAMPUS_ATTESTATION cases) | Identity and Access Registry with Engagement | Cell | T1 | [card](./OBJ-36-access-review.md) |

## Card format

One Markdown file per object, `OBJ-nn-<slug>.md`, YAML front matter first:

| Key | Holds |
|---|---|
| `id`, `name` | OBJ ID and the UI term (glossary) |
| `entities` | Dictionary entity IDs behind the object |
| `owner`, `home` | Owning domain (`module_code`, DL-04) and global or cell (DL-39) |
| `operating_entity`, `effective_dated` | Controller slot (DL-41) and history rule (DL-24) |
| `states` | Lifecycle value → chip colour (AP-14); `pending` when not defined |
| `field_groups` | Name, tier, the capability that views it (DL-34), who edits, `source` when owned elsewhere |
| `actions` | Capability codes from the [permission catalogue](../../04-access/permission-catalogue.md) |
| `events`, `variation`, `open` | Change events, MC IDs, OQ or Decision D IDs |
| `sample`, `patterns` | Sample record set, AP IDs the object uses |

## Card rules

- A card never states a value that isn't in a dictionary, a DL or a source. A gap becomes an entry under `open`, and a missing state list is `pending`.
- Chip colours for new lists (Enrolment, Guardian verification) are proposed for review; existing ones come from rules-to-ui § Status vocabulary.
- When a dictionary changes, update its cards in the same PR.

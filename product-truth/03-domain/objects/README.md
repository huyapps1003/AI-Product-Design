# Design object registry (Layer 1)

**Status:** Confirmed structure (DL-43, DL-46, Henry, 7 Nov 2026); card values cite their sources · **Owner:** Henry
**Cites:** DL-42, DL-43, DL-46 · [architecture.md](../architecture.md) · [student dictionary](../student-directory-data-dictionary.md) · [Employee Data Dictionary v1](../../00-sources/Global_Campus_OS_Employee_Data_Dictionary_v1.md) · [rules-to-ui](../../06-experience/admin-platform/rules-to-ui.md)
**Tóm tắt (VI):** Danh mục "design object": những thứ người dùng nhìn thấy và thao tác. Mỗi object có một card (YAML) với entity, domain sở hữu, trạng thái và màu chip, nhóm trường theo tier, hành động và câu hỏi mở. 12 card đã có; các card còn lại chờ data dictionary.

A design object is something a user sees, finds and acts on. Each maps to dictionary entities and has exactly one owning domain (DL-43). The card's YAML front matter is what the AI design workflow reads; the dictionaries stay the engineering source.

## Registry

| ID | Object | Entities | Owner (writer) | Home | Top tier | Card |
|---|---|---|---|---|---|---|
| OBJ-01 | Person | Person, A01, A02 | Identity and Access Registry | Global | T1 | [card](./OBJ-01-person.md) |
| OBJ-02 | Student | ST01, ST06–ST10 | Student Records | Cell | T3 | [card](./OBJ-02-student.md) |
| OBJ-03 | Enrolment | ST02, ST03 | Admissions creates, Student Records holds | Cell | T2 | [card](./OBJ-03-enrolment.md) |
| OBJ-04 | Guardian | ST04, ST05, ST07 | Student Records | Cell | T2 | [card](./OBJ-04-guardian.md) |
| OBJ-05 | Employee | E01, E07, E08 | Staff and HR | Cell | T2 | [card](./OBJ-05-employee.md) |
| OBJ-06 | Employment | E02–E05 | Staff and HR | Cell | T2 | [card](./OBJ-06-employment.md) |
| OBJ-07 | Access grant | A03, A04 | Access Registry (Global Admin) | Global | T1 | [card](./OBJ-07-access-grant.md) |
| OBJ-08 | Team membership | A05, A07, A10, A12, A13 | Access Registry (Module Admin) | Cell | T1 | [card](./OBJ-08-team-membership.md) |
| OBJ-09 | Family access | F01, R03 | Access Registry (system) | Cell | T1 | [card](./OBJ-09-family-access.md) |
| OBJ-10 | Case | Case, A15 | Engagement | Cell | T3 when restricted | [card](./OBJ-10-case.md) |
| OBJ-11 | Audit event | A14 | Platform services | Cell, redacted global index | T3 snapshots | [card](./OBJ-11-audit-event.md) |
| OBJ-12 | Teaching allocation | D01–D03 | Teaching | Cell | T1 | [card](./OBJ-12-teaching-allocation.md) |
| OBJ-13 | Scope node and campus | Tree node, campus operator, settings | Tenancy and config | Global | T1 | Needs the Organisation dictionary |
| OBJ-14 | Legal entity | Legal entity | Tenancy and config | Global | T1 | Needs the Organisation dictionary |
| OBJ-15 | Class and programme offering | Academic year, term, programme pack, offering, grade, subject, class | Academic structure | Global (packs), cell | T1 | Needs the Academic structure dictionary |
| OBJ-16 | Application | Enquiry, application, offer, contract | Admissions | Cell | T2 | Needs the Admissions dictionary |
| OBJ-17 | Consent | Consent, purpose | Engagement | Cell | T2 | Needs the Governance dictionary |
| OBJ-18 | Transfer | Sharing record, two enrolments | Admissions with Engagement | Cell | T2 | Needs the Governance dictionary |
| OBJ-19…24 | Mark and report card, attendance mark, incident, wellbeing record, invoice, route | Architecture § High-level data model | Their modules | Cell | T2 or T3 | Needs a dictionary per module |

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

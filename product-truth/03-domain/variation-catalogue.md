# What varies across the group

**Status:** Known (research catalogue MC-01…MC-15) mapped to product truth, 25 Oct 2026 · **Owner:** Henry
**Cites:** research §04 (MC-01…MC-15) · RES-F03
**Tóm tắt (VI):** 15 điểm khác biệt giữa thương hiệu, campus và quốc gia, mỗi điểm ghi rõ cấp độ, product truth đã mô tả tới đâu và câu hỏi mở liên quan.

Every screen that shows group, brand, country or campus data should check this table: a value that varies must not be hard-coded.

Coverage: **Described** (product truth models it) · **Partly** (named but not modelled) · **Not described**.

| ID | What varies | Level | Coverage | Where product truth describes it | Open |
|---|---|---|---|---|---|
| MC-01 | Settings that differ by campus but share defaults | Group › Country › Campus › Section › Programme offering | Described | [tenancy-and-scope](./tenancy-and-scope.md) | — |
| MC-02 | Brand identity, curricula, policies and portals | Brand, across countries | Not described | — | OQ-14, OQ-26, OQ-27 |
| MC-03 | Legal entity behind a campus: operator, employer, franchise or JV partner | Campus, can change over time | Partly (employer only) | Data Dictionary E02 `employer_org_id` | OQ-02, OQ-03, OQ-04 |
| MC-04 | Data residency and cross-border transfer rules | Country | Partly (SG and IN only) | Architecture § tenancy (country cells) | OQ-21 |
| MC-05 | Regulator reports, identifiers and inspections | Country, sometimes emirate or state | Partly (SG and IN only) | Architecture § tenancy (regulator packs); [regulatory-context](../05-data-governance/regulatory-context.md) | OQ-22 |
| MC-06 | Academic year start, terms and working week | Country and campus | Partly | Architecture § How GIIS operates today; calendar engine | OQ-24 |
| MC-07 | Several curricula on one campus, each with its own grades and rules | Programme offering | Described | Programme packs ([modules-and-services](./modules-and-services.md)) | — |
| MC-08 | Board and national identifiers and registrations | Curriculum and country | Partly | Architecture § Integrations (CBSE, IB, Cambridge) | OQ-23 |
| MC-09 | Consent and notice rules for children's data | Country, then per child and purpose | Not described (Architecture names a Consent entity only) | — | OQ-09, OQ-10 |
| MC-10 | Transfers between campuses, brands and countries | Two campuses, possibly two entities and two countries | Partly (record move only) | Architecture § tenancy ("a transfer is a record move") | OQ-07 |
| MC-11 | Comparing campuses whose academic years start at different times | Group | Partly | — | OQ-24 |
| MC-12 | Currency, tax, date formats, staff and parent languages | Country and campus; parent preference | Partly | Architecture § tenancy (localisation); DL-15 | OQ-25 |
| MC-13 | Group staff in one country working with another country's data | User and country | Partly | [rbac-model Layer 6](../04-access/rbac-model.md#layer-6--policy-evaluation-by-cell) | OQ-18 |
| MC-14 | Which settings the group fixes and which a school may change | Each setting | Not described | — | OQ-20 |
| MC-15 | One identity and directory across the group | Group | Described | Architecture § tech stack (Google Workspace SSO); global identity layer | — |

## Examples to use in designs (GIIS)

- Singapore runs two semesters; Noida's year starts in April and Saturdays are non-working (MC-06).
- GIIS Singapore teaches six curricula on one campus: GMP, CBSE, IB PYP, Cambridge CLSP, IGCSE, IBDP (MC-07).
- Currencies SGD and INR; GST (MC-12).

Equivalent facts for other brands haven't been researched.
